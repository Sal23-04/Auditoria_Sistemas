# Taller: elige el marco correcto para tu auditoría

**Docente:** Juan Duque  

### Integrantes del Grupo:
* Salomon Camargo Londoño

---

## 1. Organización y Caso Elegido

* **Organización:** Lean Solutions Group (LSG)
* **Sector:** Servicios Nearshore / BPO, soluciones tecnológicas y gestión operativa para empresas internacionales (principalmente de logística, transporte y servicios financieros en EE. UU.).

---

## 2. Descripción del Contexto

Lean Solutions Group es un proveedor multinacional de servicios *nearshore* que ofrece soluciones de personal, desarrollo de software (LeanTek), automatizaciones de IA y BPO para más de 500 clientes internacionales en sectores críticos como logística, finanzas y salud. Maneja ecosistemas de TI complejos que incluyen ERP corporativos (*Microsoft Dynamics 365 F&O*), plataformas en la nube (*Azure / Power Platform*), herramientas de monitorización y sistemas de telecomunicaciones (*IP-PBX/BPO*). Sus principales riesgos son el cumplimiento regulatorio estricto en la transferencia transfronteriza y confidencialidad de datos, fallas en la continuidad operativa, e interrupciones en el servicio que impacten la reputación y causen sanciones financieras.

---

## 3. Marcos Elegidos y Orden de Aplicación

Para auditar de forma integral a Lean Solutions Group, se adopta un enfoque híbrido en la siguiente secuencia de aplicación:

1. **COBIT (Nivel Estratégico y Gobierno de TI):** Se aplica en primer lugar.
2. **ISO/IEC 27001 (Nivel de Seguridad de la Información y Cumplimiento):** Se aplica en segundo lugar.
3. **ITIL (Nivel Operativo y Gestión de Servicios):** Se aplica en tercer lugar.
4. **NIST Cybersecurity Framework (NIST CSF - Referencia Complementaria de Segundo Nivel):** Se aplica como apoyo especializado técnico al marco ISO.

---

## 4. Justificación de la Selección y Secuencia Lógica

### ¿Por qué esta combinación?
Al ser un operador BPO y tecnológico global, la auditoría debe garantizar primero que las metas de TI están alineadas con los objetivos de negocio del cliente (Gobierno), asegurar que los datos sensibles manejados desde sedes satélites operan bajo estándares estrictos de ciberseguridad (Seguridad), y garantizar la entrega ininterrumpida de servicios de mesa de ayuda y soporte (Operación).

### Justificación del Orden:

* **1. COBIT (Primero):** Es necesario evaluar la gobernanza integral desde la alta dirección antes de bajar a los aspectos técnicos. COBIT permite auditar si la junta directiva y el equipo directivo de LSG tienen una visión clara de los riesgos tecnológicos corporativos, la gestión del presupuesto de TI y el alineamiento estratégico entre sus operaciones en Colombia y las exigencias normativas de sus clientes en EE. UU.
* **2. ISO/IEC 27001 + NIST CSF (Segundo):** Dado que Lean Solutions Group gestiona información privada y sensible de terceros (datos financieros, logística y salud), el mayor riesgo reputacional y legal radica en las brechas de seguridad. Una vez evaluada la gobernanza con COBIT, se audita el Sistema de Gestión de Seguridad de la Información (SGSI) bajo ISO/IEC 27001 para validar la confidencialidad, integridad y disponibilidad de la información, apoyándonos en los 5 pilares de NIST CSF (Identificar, Proteger, Detectar, Responder, Recuperar) para evaluar la postura ante ciberataques.
* **3. ITIL (Tercero):** Con la gobernanza establecida y los controles de seguridad validados, se pasa al nivel operativo. ITIL permite auditar la calidad, estabilidad y gestión de los servicios de TI (gestión de incidentes, solicitudes de servicio, cambios en plataformas de software y monitoreo de SLAs contractuales de los clientes).

---

## 5. Evidencia Propuesta por Marco

### A. Evidencia para COBIT (Gobierno y Alineación Estratégica)
* **Evidencia 1 (Alineación Estratégica - Marco EDM01/APO02):** Actas del Comité Directivo de TI y Matriz de Alineación Estratégica donde se evidencia la aprobación del plan de inversión tecnológica alineado con las metas de expansión multinacional y cumplimiento normativo corporativo.
* **Evidencia 2 (Gestión de Riesgos de TI - Marco APO12):** Matriz de Riesgos Corporativos de TI (*IT Risk Register*) actualizada, con la clasificación de riesgos cibernéticos, impactos financieros definidos, planes de mitigación aprobados por el CISO y actas de revisión trimestral.

### B. Evidencia para ISO/IEC 27001 + NIST CSF (Seguridad de la Información)
* **Evidencia 1 (Control de Acceso y Gestión de Identidades - Anexo A.9 / NIST PR.AC):** Matriz de Roles y Permisos en el ERP corporativo (*Microsoft Dynamics 365*) y en el Active Directory, acompañada de las trazabilidades/logs de auditoría de revisiones semestrales de privilegios y el uso obligatorio de Autenticación de Múltiple Factor (MFA) para acceso remoto.
* **Evidencia 2 (Continuidad de Seguridad y Respuesta a Incidentes - Anexo A.17 / NIST RS.RP):** Plan de Respuesta a Incidentes de Ciberseguridad y Políticas de Copias de Seguridad (*Backup*), junto con los informes firmados del último simulacro de *Ransomware* / prueba de recuperación de desastres (DRP) realizado en los entornos en la nube de la empresa.

### C. Evidencia para ITIL (Gestión de Servicios de TI)
* **Evidencia 1 (Gestión de Incidentes y Cumplimiento de SLAs):** Reportes mensuales extraídos de la herramienta de *Service Desk* (ej. ServiceNow o Jira Service Management) que muestren el volumen de tickets de soporte, los tiempos promedios de primera respuesta, el porcentaje de resolución en primer nivel y el indicador de cumplimiento de los Acuerdos de Nivel de Servicio (SLA) comprometidos con los clientes.
* **Evidencia 2 (Gestión de Cambios - Change Enablement):** Registros y actas del Comité de Control de Cambios (CAB - *Change Advisory Board*) del último trimestre, adjuntando la documentación de evaluación de riesgos, planes de reversión (*rollback*) y pruebas de homologación/QA antes del pase a producción de actualizaciones del software propio o del ERP corporativo.

---
