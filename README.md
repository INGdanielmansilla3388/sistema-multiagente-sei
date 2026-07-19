# 🏗️ Sistema Multi-Agente SEI

**Challenge AluraAgente — Oracle Next Education (ONE) "AI for Tech", Alura Latam / Oracle**

Agente de inteligencia artificial corporativo capaz de responder, en lenguaje natural,
preguntas de los colaboradores de una empresa de ingeniería sobre el avance de sus
proyectos, horas cargadas, documentación técnica y comunicaciones con clientes —
sin necesidad de abrir manualmente ningún documento, planilla o base de datos.

**🔗 App en producción:** https://sei-agente.streamlit.app/

---

## 📋 Descripción general

SEI Ingeniería S.R.L. (empresa ficticia creada para este Challenge) es una consultora
de ingeniería eléctrica que ejecuta proyectos industriales — en este caso, dos
proyectos de ejemplo en el sector de minería de litio: **SE01** (Ampliación Planta
de Evaporación, Salar Norte) y **SE02** (Ampliación de Almacenes de Repuestos).

Como en cualquier empresa de ingeniería real, la información del día a día está
repartida entre una base de datos de gestión (avance, horas, riesgos, hitos) y una
gran cantidad de documentos técnicos y de comunicación con el cliente (memorias de
cálculo, minutas de reunión, solicitudes de información, transmittals). Encontrar un
dato puntual entre todo eso suele significar abrir varios archivos y recorrer
planillas.

Este proyecto resuelve ese problema con un **sistema multi-agente**: un orquestador
recibe la pregunta en español, decide automáticamente a qué fuente de información
consultar (o varias a la vez), y devuelve una respuesta directa y verificable —
**nunca inventada**: si un dato no está disponible en la fuente consultada, el
sistema lo informa explícitamente en vez de completar con una suposición.

---

## 🏛️ Arquitectura de la solución

```
                         ┌─────────────────────┐
      Pregunta en   ───► │   AGENTE PM          │
      lenguaje natural   │   (orquestador,       │
                         │   LangGraph)          │
                         └──────────┬───────────┘
                                    │
                    Triaje semántico con LLM decide
                    qué sub-agente(s) consultar
                                    │
        ┌─────────────┬────────────┼────────────┬─────────────┐
        ▼             ▼            ▼            ▼             │
 ┌────────────┐ ┌────────────┐ ┌────────────┐ ┌────────────────┐
 │ SQL-Avance │ │ Timesheet  │ │    RAG-    │ │      RAG-       │
 │            │ │            │ │Conocimiento│ │ Comunicaciones   │
 │ Avance,    │ │ Horas      │ │            │ │                  │
 │ SPI/CPI,   │ │ cargadas,  │ │ Memorias   │ │ Minutas, SDI/RFI,│
 │ riesgos,   │ │ tareas por │ │ de cálculo,│ │ transmittals con │
 │ hitos      │ │ persona    │ │ listados de│ │ el cliente       │
 │            │ │            │ │ cargas,    │ │                  │
 │ (MySQL)    │ │ (MySQL)    │ │ normas     │ │                  │
 │            │ │            │ │ (FAISS)    │ │ (FAISS)          │
 └────────────┘ └────────────┘ └────────────┘ └────────────────┘
        │             │            │            │
        └─────────────┴────────────┴────────────┘
                          │
                 ┌────────▼────────┐
                 │  Síntesis final  │
                 │  (LLM combina    │
                 │  resultados)     │
                 └──────────────────┘
```

### Decisión de diseño: triaje semántico, no reglas por palabras clave

El orquestador no usa reglas fijas del tipo "si la pregunta contiene la palabra X,
usar el agente Y" — ese enfoque se rompe apenas aparece una pregunta formulada con
otro vocabulario. En cambio, un LLM lee la pregunta completa y decide con criterio
qué sub-agente(s) son necesarios — incluyendo combinar más de uno cuando la pregunta
lo requiere (por ejemplo, "horas cargadas en memorias de cálculo" necesita
Timesheet **y** RAG-Conocimiento a la vez). Esto permite que el sistema escale a
preguntas nunca vistas antes, sin reprogramar reglas.

### Principio transversal: cero alucinación

Cada sub-agente responde ÚNICAMENTE con lo que encuentra en su fuente (resultado real
de una consulta SQL, o fragmentos de documentos recuperados por búsqueda semántica).
Si no encuentra el dato, lo dice explícitamente. Todas las respuestas de este README
fueron verificadas manualmente contra los datos crudos de la base y los documentos
originales antes de ser incluidas como ejemplos.

---

## 🛠️ Tecnologías y herramientas utilizadas

| Categoría | Herramienta |
|---|---|
| Orquestación de agentes | LangGraph (`StateGraph`) |
| Framework LLM | LangChain (`langchain_core`, `langchain_groq`, `langchain_community`) |
| Modelo de lenguaje | Groq — `openai/gpt-oss-120b` |
| Embeddings | HuggingFace `intfloat/multilingual-e5-small` |
| Base vectorial (RAG) | FAISS |
| Base de datos relacional | MySQL (alojada en Railway) |
| Frontend | Streamlit |
| Despliegue | Streamlit Community Cloud (Plan B — ver sección OCI abajo) |
| Extracción de documentos | `python-docx`, `openpyxl` |
| Control de versiones | Git + GitHub |

---

## ▶️ Instrucciones para ejecutar el proyecto

### Opción 1 — Usar la app ya desplegada
Entrar directamente a **https://sei-agente.streamlit.app/** — no requiere instalación.

### Opción 2 — Ejecutar localmente
```bash
git clone https://github.com/INGdanielmansilla3388/sistema-multiagente-sei.git
cd sistema-multiagente-sei
pip install -r requirements.txt
```
Crear el archivo `.streamlit/secrets.toml` con:
```toml
GROQ_API_KEY = "tu_key_de_groq"
DATABASE_URL = "tu_url_de_base_de_datos_mysql"
```
Ejecutar:
```bash
streamlit run app.py
```

---

## 💬 Ejemplos de preguntas y respuestas reales

Todas las respuestas siguientes fueron generadas por la app en producción y
verificadas manualmente contra los datos crudos de la base de datos y los
documentos originales.

**Pregunta (SQL-Avance):** ¿Cuál es el SPI actual del proyecto SE01?
> El SPI actual del proyecto SE01 es 0.92.

**Pregunta (SQL-Avance):** ¿Qué riesgos abiertos tiene el proyecto SE02?
> Riesgo abierto del proyecto SE02 — Código: R-SE02-001. Descripción: Demora en la
> respuesta del cliente (Litio Andino SA) sobre los criterios de diseño de
> iluminación exterior para la playa de camiones. Impacto: Retraso en la emisión de
> la memoria de cálculo y del plano asociado. Nivel de riesgo: Alta (probabilidad
> media). Plan de mitigación: Aplicar niveles de iluminación según la norma IRAM
> AADL J20-04 y registrar la decisión en el SDI. Estado: En seguimiento (última
> actualización 06/07/2026).

**Pregunta (SQL-Avance):** ¿Qué hitos vencen esta semana entre SE01 y SE02?
> Los hitos que vencen esta semana (13-19 julio 2026) son: ENTREGA PLANO UNIFILAR
> MCC-02 (REV. DEFINITIVA) — vence 18 julio 2026 (SE01); ENTREGA ALCANCE TÉCNICO
> ELÉCTRICO PARA LICITACIÓN (SE02-300-E-PE-001) — vence 17 julio 2026 (SE02).

**Pregunta (Timesheet):** ¿Qué hizo Lucía Fernández en el proyecto SE02 la semana pasada?
> La semana pasada Lucía Fernández trabajó en el proyecto SE02 realizando: redacción
> del SDI de iluminación para la playa de camiones; reunión de coordinación con
> Litio Andino SA y elaboración de la minuta SE02; actualización del cronograma;
> seguimiento del status de los RFI; revisión del plano unifilar del tablero
> seccional antes de su emisión; revisión de la memoria de cálculo de iluminación;
> preparación del transmittal SE02.

**Pregunta (Timesheet + RAG-Conocimiento, combinados):** ¿Cuántas horas totales se
cargaron a memorias de cálculo entre SE01 y SE02?
> Se cargaron 14.50 horas en total a memorias de cálculo entre los proyectos SE01
> y SE02.

**Pregunta (RAG-Conocimiento):** ¿Qué dice el listado de cargas eléctricas de SE01
sobre el sector Evaporación?
> El Listado de Cargas Eléctricas — Sector Evaporación (doc. SE01-1000-E-LC-010
> rev.A, vigente) indica: carga instalada total 420 kW; carga simultánea estimada
> (FS=0.85) 357 kW; cargas principales: bombas de salmuera 4×30 kW, agitadores
> 2×22 kW, compresores MVR-01 y MVR-02 2×75 kW, servicios auxiliares (potencia no
> detallada).

**Pregunta (RAG-Comunicaciones):** ¿Cuál fue la respuesta de Litio Andino sobre
iluminación de playa de camiones?
> Litio Andino informó que no cuenta con un criterio propio documentado para la
> iluminación exterior de la playa de camiones. Por ello, autoriza a SEI a aplicar
> los niveles establecidos por la norma IRAM AADL J20-04 (aplicable a depósitos y
> áreas de maniobra), tal como consta en la minuta SE02-400-E-MM-001 rev.A y en la
> solicitud de información SE02-400-E-SDI-002 rev.0.

**Pregunta (RAG-Comunicaciones):** ¿Qué se acordó en la minuta de SE01 sobre el
esquema de puesta a tierra?
> En la minuta SE01-5000-E-MM-003 rev.B se acordó adoptar un esquema de puesta a
> tierra tipo TN-S, con cinco conductores que van desde la fuente hasta la
> utilización. SEI se encargará de la ingeniería siguiendo ese esquema.

---

## ☁️ Sobre el despliegue en OCI (Oracle Cloud Infrastructure)

**Estado: cuenta OCI bloqueada por Oracle, fuera de mi control — deploy funcional
entregado en infraestructura alternativa (Railway + Streamlit Community Cloud)
mientras se resuelve.**

Intenté activar mi cuenta de Oracle Cloud Infrastructure siguiendo el proceso
oficial en **7 oportunidades distintas** entre el 14/07/2026 y la fecha de esta
entrega, cargando los datos de una tarjeta Visa válida en cada intento. Oracle
generó cargos de verificación de USD 1,00 en cada intento (reversados
automáticamente, sin completar nunca la activación de la cuenta).

Elevé un reclamo formal al equipo de soporte de Oracle Next Education (Alura) el
14/07/2026, quienes confirmaron que el problema corresponde específicamente a la
plataforma de Oracle Cloud y me derivaron al soporte directo de Oracle. Contacté al
soporte de Oracle por chat en múltiples oportunidades adicionales, sin respuesta a
la fecha de esta entrega.

**Evidencia del reclamo:** ver [`docs/evidencia_reclamo_oci.pdf`](docs/evidencia_reclamo_oci.pdf)
en este repositorio (correo completo con el intercambio con Oracle Next Education y
el mensaje de verificación de Oracle Cloud).

Este es un problema documentado y conocido para usuarios de Argentina/Latinoamérica
con tarjetas de crédito locales — el procesador de pagos de Oracle (CyberSource) no
está completando la verificación correctamente para varias tarjetas de la región.

**Decisión tomada:** para no bloquear la entrega del Challenge por un problema ajeno
a mi control, desplegué la aplicación en **Railway (MySQL) + Streamlit Community
Cloud (aplicación)** — ambos servicios cloud, gratuitos, con el mismo nivel de
funcionalidad que hubiera tenido en OCI Compute. La arquitectura fue diseñada desde
el principio para ser portable a OCI sin cambios de código, únicamente
reconfigurando la cadena de conexión a base de datos y el destino de despliegue.

**Si la cuenta OCI se activa después de esta entrega**, migraré la base de datos y
el despliegue a Oracle Cloud Infrastructure y actualizaré este README con la
evidencia correspondiente.

---

## 📄 Licencia

Ver [`LICENSE`](LICENSE) — derechos reservados, uso académico autorizado para
evaluación del Challenge, uso comercial no autorizado sin permiso escrito del autor.

---

## 👤 Autor

**Ing. Daniel Alejandro Mansilla**
[LinkedIn](https://www.linkedin.com/in/dmansilla783-li/)

Proyecto desarrollado como parte del Challenge AluraAgente, formación final del
programa Oracle Next Education (ONE) "AI for Tech" — Alura Latam / Oracle,
Generación 10.
