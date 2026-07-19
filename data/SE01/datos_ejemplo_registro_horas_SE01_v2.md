# Datos de ejemplo — registro_horas (SE01, cuadrado con Codificación maestra)
Proyecto SE01 — AMPLIACIÓN PLANTA EVAPORACIÓN SALAR NORTE — Cliente: MINERA NOA LITIO
Empleado: Ing. Martín Paredes (Depto. Eléctrico, SEI)

| # | Documento | Área | Tipo tarea | Hs norm. | descripcion_tarea |
|---|---|---|---|---|---|
| 1 | SE01-1000-E-LC-010_A | Evaporación | DIS | 4.0 | Elaboración listado total de cargas eléctricas, análisis distribución de circuitos |
| 2 | SE01-1000-E-LC-010_A | Evaporación | CAL | 3.5 | Análisis de caída de tensión y elección de aislación PVC o XLPE, listado total de cargas |
| 3 | SE01-1000-E-LC-010_A | Evaporación | REV | 2.0 | Consulta con revisor técnico caída de tensión, verificación AEA 90364-7-771 |
| 4 | SE01-1000-E-LC-010_B | Evaporación | COR | 3.0 | Modificación de listado total de cargas conforme última revisión de layout del cliente |
| 5 | SE01-1000-E-HD-010_A | Evaporación | ESP | 3.0 | Adecuación hoja de datos técnica tablero TGBT según requerimiento de cliente |
| 6 | SE01-1000-E-PL-010_A | Evaporación | DIB | 4.0 | Esquema unifilar TGBT, definición de alimentadores principales |
| 7 | SE01-1000-E-PL-011_A | Evaporación | DIB | 5.0 | Elaboración plano de canalizaciones sector prioritario solicitado por el cliente |
| 8 | SE01-1000-E-PL-011_B | Evaporación | COR | 2.5 | Modificación traza de canalizaciones, restricción de paso bajo módulos H |
| 9 | SE01-2000-E-PL-010_B | MVR | DIS | 6.5 | Actualización unifilar MCC-02 según revisión B, incorpora nuevo alimentador VFD |
| 10 | SE01-2000-E-MC-005_A | MVR | CAL | 4.0 | Cálculo de caída de tensión circuito ramal bombas salmuera, verificación IEC 60364 |
| 11 | SE01-1000-E-LC-020_A | Evaporación | ESP | 2.0 | Cotización de cables XLPE y aislación PVC para evaluación técnico-económica |
| 12 | SE01-5000-G-CR-001_C | General | GES | 1.5 | Análisis de cronograma y ajuste por falta de información del cliente |

Notas:
- Todos los códigos cuadran con SE01-5000-G-LD-001 (Codificación maestra) y con
  SE01-5000-G-LD-003 (Listado de Documentos).
- Se eliminó el registro duplicado "esquema unifilar" que antes tenía código
  inventado EE-010 — ahora unificado bajo PL-010 (Plano, tipo real del catálogo).
- Tipos de tarea del catálogo tipos_tarea: DIS, CAL, REV, COR, ESP, DIB, GES.
- SE02 (instrumentación) queda pendiente — a definir en próximo paso según indicación
  de Daniel.
