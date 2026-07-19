# SE01 — Set de documentos normalizado (cuadrado con SE01-5000-G-LD-001, fuente de verdad)

Proyecto: SE01 — AMPLIACIÓN PLANTA EVAPORACIÓN SALAR NORTE
Cliente: MINERA NOA LITIO
Áreas válidas: 1000 Evaporación | 2000 MVR | 3000 Carbonatación | 4000 Piletas | 5000 General

## Documentos GENERALES (área 5000 — no pertenecen a un sector físico)

| Código | Tipo (catálogo) | Descripción | Rev |
|---|---|---|---|
| SE01-5000-G-LD-001 | LD | Codificación de Documentos (MAESTRO, fuente de verdad) | 0 |
| SE01-5000-G-LD-002 | LD | Control de Documentos | A |
| SE01-5000-G-LD-003 | LD | Listado de Documentos Entregables | 1 |
| SE01-5000-G-CR-001 | CR | Cronograma | C |
| SE01-5000-G-RFI-001 | — | Status RFI | A |
| SE01-5000-G-TT-001 | TT | Transmittal | A |
| SE01-5000-G-BT-001 | BT | Base Técnica de Contrato (general) | 0 |
| SE01-5000-E-SDI-001 | SDI | Solicitud de Información — Eléctrica | 0 |
| SE01-5000-E-MM-001 | MM | Minuta de Reunión — Eléctrica | A |

Corrección aplicada: "Bases técnicas para contrato de obra eléctrica" que antes tenía
tipo inventado "PE" y área 1000 (mal) → ahora tipo real **BT** (Base Técnica de
Contrato, existe en el catálogo oficial) y área **5000** (documento general, no
pertenece a un sector físico de planta).

## Documentos TÉCNICOS — Disciplina Eléctrica (por sector físico real)

| Código | Área | Tipo | Descripción | Rev |
|---|---|---|---|---|
| SE01-1000-E-LC-010 | Evaporación | LC | Listado de cargas eléctricas | B |
| SE01-1000-E-LC-020 | Evaporación | LC | Listado de cables | A |
| SE01-1000-E-HD-010 | Evaporación | HD | Hoja de datos TGBT | A |
| SE01-1000-E-PL-010 | Evaporación | PL | Plano unifilar TGBT | A |
| SE01-1000-E-PL-011 | Evaporación | PL | Plano de canalizaciones sector prioritario | B |
| SE01-2000-E-PL-010 | MVR | PL | Plano unifilar MCC-02 | B |
| SE01-2000-E-MC-005 | MVR | MC | Memoria de cálculo caída de tensión ramal bombas salmuera | A |

Nota: TGBT (tablero general) se ubica físicamente en el sector Evaporación (1000);
MCC-02 alimenta el sector MVR (2000) — de ahí la diferencia de área entre ambos
tableros, cada uno en su código correcto.

## Ciclo de vida de revisión (confirmado, según CONTROL_DE_DOCUMENTOS real)

Etapas de diseño/revisión: A, B, C, D, E, F, J, K
Etapas aptas para construcción: 0, 1, 2, 3, 4, 5
El último estadio varía por documento — no todos llegan a la misma revisión final.
