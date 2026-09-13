# Taller: Elección de marcos para auditoría de sistemas

**Docente:** Juan Duque  
**Estudiante:** Salomón Camargo Londoño  

---

## 1. Empresa seleccionada

* **Organización:** Lean Solutions Group (LSG)
* **Sector:** Nearshoring, BPO y soluciones de TI enfocadas en logística, transporte y finanzas para empresas en EE. UU.

---

## 2. Contexto del negocio

Lean Solutions Group le presta servicios de BPO, desarrollo de software (LeanTek), automatización e infraestructura a más de 500 clientes internacionales en áreas como salud y logística. Su operación depende de sistemas como Microsoft Dynamics 365, servicios en Azure/Power Platform, telefonía IP y herramientas de monitoreo. Los riesgos más importantes que enfrentan son el manejo de datos sensibles entre fronteras (cumpliendo HIPAA, GDPR y acuerdos de confidencialidad), la caída de servicios críticos que afecten los SLA 24/7 y la exposición a multas o pérdida de clientes por fallas de seguridad.

---

## 3. Marcos seleccionados y secuencia propuesta

Para auditar una empresa con este perfil, propongo abordar la revisión en este orden:

1. **COBIT:** Nivel directivo / Gobierno de TI
2. **ISO/IEC 27001 (junto con NIST CSF):** Nivel de ciberseguridad y protección de información
3. **ITIL:** Nivel operativo y entrega de servicios

---

## 4. Por qué estos marcos y por qué en este orden

### Justificación de la secuencia
Al ser un proveedor de servicios tecnológicos outsourcing, la auditoría debe empezar revisando cómo se toman las decisiones estratégicas desde arriba (gobierno), seguir con la protección de los activos de información de los clientes (seguridad) y terminar en la ejecución del trabajo diario (operaciones).

* **1. COBIT (Primero):** Antes de revisar servidores o tickets, hay que auditar la gobernanza. Necesitamos confirmar si la directiva de LSG tiene alineados sus planes de TI en Colombia con las exigencias normativas y comerciales que piden los clientes en Estados Unidos, además de cómo priorizan y gestionan el presupuesto tecnológico.
* **2. ISO/IEC 27001 + NIST CSF (Segundo):** El activo fundamental de LSG es la confianza y la información de terceros. Al auditar con ISO 27001 revisamos que existan políticas claras para mantener la confidencialidad e integridad de esos datos. Nos apoyamos en NIST CSF para evaluar la capacidad técnica de detectar, responder y recuperarse ante incidentes o ciberataques.
* **3. ITIL (Tercero):** Con la dirección y la seguridad auditadas, nos enfocamos en la operación cotidiana. ITIL sirve para comprobar que la mesa de ayuda, el soporte de infraestructura y el mantenimiento del software funcionen de manera estable y cumplan los tiempos comprometidos en los SLA.

---

## 5. Evidencias a solicitar por cada marco

### A. Para COBIT (Gobierno y Alineación)
* **Evidencia 1 (Alineación Estratégica - EDM01/APO02):** Actas de las reuniones del Comité Directivo de TI donde conste la aprobación del plan de inversión en tecnología y su alineación con los objetivos del negocio.
* **Evidencia 2 (Gestión de Riesgos - APO12):** La matriz de riesgos de TI (IT Risk Register) actualizada, mostrando la priorización de riesgos de ciberseguridad, planes de acción y su revisión por parte de la gerencia.

### B. Para ISO/IEC 27001 + NIST CSF (Seguridad de la Información)
* **Evidencia 1 (Control de Acceso - Anexo A.9 / PR.AC):** Listado de usuarios y roles asignados en Dynamics 365 y Active Directory, junto con los registros de la última revisión periódica de accesos e historial de autenticación con MFA para usuarios remotos.
* **Evidencia 2 (Continuidad y Ciberseguridad - Anexo A.17 / RS.RP):** Plan de respuesta a incidentes y el informe del último simulacro de restauración de copias de seguridad / recuperación ante desastres (DRP) realizado en Azure.

### C. Para ITIL (Gestión de Servicios)
* **Evidencia 1 (Gestión de Incidentes y SLAs):** Reporte mensual extraído de la plataforma de tickets (Jira o ServiceNow) con las métricas de tiempos de respuesta, nivel de resolución en primer contacto y porcentaje de cumplimiento de SLA por cliente.
* **Evidencia 2 (Gestión de Cambios - Change Enablement):** Actas del Comité de Control de Cambios (CAB) del último trimestre, verificando que los pases a producción en el software o ERP contaron con pruebas de calidad (QA) y plan de reversión (rollback).
