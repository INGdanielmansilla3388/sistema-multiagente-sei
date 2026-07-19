## ☁️ Sobre el despliegue en OCI (Oracle Cloud Infrastructure)

**Estado: cuenta OCI bloqueada por Oracle, fuera de mi control — deploy funcional entregado en infraestructura alternativa (Railway + Streamlit Community Cloud) mientras se resuelve.**

Intenté activar mi cuenta de Oracle Cloud Infrastructure siguiendo el proceso oficial
en **7 oportunidades distintas** entre el 14/07/2026 y la fecha de esta entrega,
cargando los datos de una tarjeta Visa válida en cada intento. Oracle generó cargos
de verificación de USD 1,00 en cada intento (reversados automáticamente, sin
completar nunca la activación de la cuenta).

Elevé un reclamo formal al equipo de soporte de Oracle Next Education (Alura) el
14/07/2026, quienes confirmaron que el problema corresponde específicamente a la
plataforma de Oracle Cloud y me derivaron al soporte directo de Oracle. Contacté
al soporte de Oracle por chat en múltiples oportunidades adicionales, sin
respuesta a la fecha de esta entrega.

**Evidencia del reclamo:** ver `docs/evidencia_reclamo_oci.pdf` en este repositorio
(correo completo con el intercambio con Oracle Next Education y el mensaje de
verificación de Oracle Cloud).

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
