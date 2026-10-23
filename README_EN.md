# FaceAttendEDU-RAPS

Learning-evidence package (RAPS) for **FaceAttend EDU**, an attendance-management system with facial recognition built in SENA's Software Analysis and Development technology program (ADSO), cohort 3145556.

[Versión en español](README.md)

## Purpose

This repository collects the formal deliverables the ADSO program requires to assess the project: requirements, analysis, technical proposal, design, code artifacts, testing, implementation, improvement actions, manuals and the project article. Each folder is one deliverable, and every document exists in Spanish and English.

It contains no source code. The code and the team's working documentation live in other repositories (see [Related repositories](#related-repositories)).

## Language convention

- In folders `01` to `10`, each deliverable has an `es/` (Spanish) and an `en/` (English) subfolder with the same content.
- In `Anexos/01-Historias_de_usuarios` the subfolders are upper-case: `ES/` and `EN/`. Spanish files are named `Historia_de_usuario.md` and English files `user-stories.md`.
- `Anexos/02-Mapa_navegación` holds a single file (`navigation-map.md`), written in Spanish; there is no English version.
- Folder names are in Spanish because they follow the SENA deliverable names; the English name of each one is shown next to it below.

## Index

### [01-Informe de especificación de requisitos](01-Informe%20de%20especificación%20de%20requisitos/en/) — Requirements specification report

- [Requirements_Specification_Report.md](01-Informe%20de%20especificación%20de%20requisitos/en/Requirements_Specification_Report.md)

### [02-Documentación de análisis del software](02-Documentación%20de%20análisis%20del%20software/en/) — Software analysis documentation

- [Software_Analysis_Documentation.md](02-Documentación%20de%20análisis%20del%20software/en/Software_Analysis_Documentation.md)
- [data-flow.md](02-Documentación%20de%20análisis%20del%20software/en/data-flow.md)
- [role-flows.md](02-Documentación%20de%20análisis%20del%20software/en/role-flows.md)
- [`use-cases/`](02-Documentación%20de%20análisis%20del%20software/en/use-cases/) (4)

### [03-Documentación de propuesta técnica del software](03-Documentación%20de%20propuesta%20técnica%20del%20software/en/) — Software technical proposal

- [Activity_Plan_FaceAttendEDU.md](03-Documentación%20de%20propuesta%20técnica%20del%20software/en/Activity_Plan_FaceAttendEDU.md)
- [Gantt_Chart.md](03-Documentación%20de%20propuesta%20técnica%20del%20software/en/Gantt_Chart.md)
- [Hardware_Requirements_Table.md](03-Documentación%20de%20propuesta%20técnica%20del%20software/en/Hardware_Requirements_Table.md)
- [TECHNICAL_REPORT_FaceAttendEDU.md](03-Documentación%20de%20propuesta%20técnica%20del%20software/en/TECHNICAL_REPORT_FaceAttendEDU.md)

### [04-Documentación de diseño del software](04-Documentación%20de%20diseño%20del%20software/en/) — Software design documentation

- [MODELS.md](04-Documentación%20de%20diseño%20del%20software/en/MODELS.md)
- [Software_Design_Documentation.md](04-Documentación%20de%20diseño%20del%20software/en/Software_Design_Documentation.md)
- [`class-diagrams/`](04-Documentación%20de%20diseño%20del%20software/en/class-diagrams/) (8)
- [`data-dictionary/`](04-Documentación%20de%20diseño%20del%20software/en/data-dictionary/) (8)
- [`sequence-diagrams/`](04-Documentación%20de%20diseño%20del%20software/en/sequence-diagrams/) (6)

### [05-Artefactos de código](05-Artefactos%20de%20código/en/) — Code artifacts

- [Code-Documentation.md](05-Artefactos%20de%20código/en/Code-Documentation.md)
- [dependencies.md](05-Artefactos%20de%20código/en/dependencies.md)
- [environment-variables.md](05-Artefactos%20de%20código/en/environment-variables.md)

### [06-Documentación de pruebas](06-Documentación%20de%20pruebas/en/) — Testing documentation

- [Unit_test_documentation.md](06-Documentación%20de%20pruebas/en/Unit_test_documentation.md)

### [07-Procedimiento de implementación del software](07-Procedimiento%20de%20implementación%20del%20software/en/) — Software implementation procedure

- [Software_implementation_documentation.md](07-Procedimiento%20de%20implementación%20del%20software/en/Software_implementation_documentation.md)

### [08-Documentación de acciones correctivas, preventivas y de mejoramiento del software](08-Documentación%20de%20acciones%20correctivas,%20preventivas%20y%20de%20mejoramiento%20del%20software/en/) — Corrective, preventive and improvement actions

- [Corrective_Preventive_and_Software_Improvement_Actions_Documentation.md](08-Documentación%20de%20acciones%20correctivas,%20preventivas%20y%20de%20mejoramiento%20del%20software/en/Corrective_Preventive_and_Software_Improvement_Actions_Documentation.md)

### [09-Manuales](09-Manuales/en/) — Manuals

- [Administrator_Manual.md](09-Manuales/en/Administrator_Manual.md)
- [Installation_Manual.md](09-Manuales/en/Installation_Manual.md)
- [Technical_Manual.md](09-Manuales/en/Technical_Manual.md)
- [User_Manual.md](09-Manuales/en/User_Manual.md)

### [10-Articulo del proyecto](10-Articulo%20del%20proyecto/en/) — Project article

- [Article_FaceAttendEDU.md](10-Articulo%20del%20proyecto/en/Article_FaceAttendEDU.md)

### [Anexos](Anexos/) — Annexes

- User stories: [ES index](Anexos/01-Historias_de_usuarios/ES/Index_HU.md) · [EN index](Anexos/01-Historias_de_usuarios/EN/Index_HU.md)

  | Module | ES | EN |
  |---|---|---|
  | Academic management (HU-ACAD) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-ACAD/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-ACAD/user-stories.md) |
  | Attendance (HU-ATT) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-ATT/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-ATT/user-stories.md) |
  | Configuration (HU-CONF) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-CONF/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-CONF/user-stories.md) |
  | History (HU-HIST) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-HIST/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-HIST/user-stories.md) |
  | Identity and access (HU-IAM) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-IAM/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-IAM/user-stories.md) |
  | Justifications (HU-JUS) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-JUS/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-JUS/user-stories.md) |
  | Reports (HU-REP) | [Historia_de_usuario.md](Anexos/01-Historias_de_usuarios/ES/HU-REP/Historia_de_usuario.md) | [user-stories.md](Anexos/01-Historias_de_usuarios/EN/HU-REP/user-stories.md) |

- Navigation map: [navigation-map.md](Anexos/02-Mapa_navegación/navigation-map.md)

The `use-cases/`, `class-diagrams/`, `data-dictionary/` and `sequence-diagrams/` folders hold Mermaid (`.mmd`) or PlantUML (`.puml`) diagrams, or per-context tables; the number in parentheses is the file count.

## Authors

Team 10, cohort 3145556, Software Analysis and Development technology program, SENA.

- Jonattan Steven Rizo Solano
- Diego Andrés Gutiérrez Nuñez
- Juan David Arboleda Perdomo

Instructor: José de Jesús Motta Vargas.

## Related repositories

| Repository | Contents | Relation to this package |
|---|---|---|
| [ProyectoFaceAttendEDU](https://github.com/Jonas7891/ProyectoFaceAttendEDU) | Code: 9 microservices behind Kong, Liquibase migrations, Expo web client and mobile app. | The product these deliverables document. The test and quality reports (IEEE 829, ISO/IEC 25010) are produced there. |
| [fae-docs](https://github.com/code-sena/fae-docs) | The team's working documentation: context, domain, architecture, ADRs, governance and quality. | The technical source the RAPS documents are derived from. |
| FaceAttendEDU-RAPS (this one) | Formal bilingual deliverables for SENA assessment. | Consolidated version in academic format. |
