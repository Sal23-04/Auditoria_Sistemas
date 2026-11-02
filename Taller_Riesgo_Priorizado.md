# Taller Integrado: Matriz de Riesgos y Plan de Auditoría de Sistemas
**Auditoría de Sistemas (ET0114)**  
**Docente:** Juan Duque  

**Integrantes:**
- Salomón Camargo Londoño

---

# PARTE 1: MATRIZ INICIAL DE RIESGOS DE SISTEMAS

## 1. Actualización del Contexto del Caso

**Organización:** Lean Solutions Group (LSG)  
**Proceso / Sistema a Analizar:** *Proceso de Gestión de Accesos e Identidades (IAM) y Operación de Datos Sensibles en la Plataforma ERP Microsoft Dynamics 365 (F&O) para la Operación Nearshore.*

**Contexto actualizado:**  
Lean Solutions Group administra operaciones logísticas y financieras transfronterizas para más de 500 clientes en EE. UU. desde sus sedes en Colombia. Este análisis se enfoca en la plataforma **Microsoft Dynamics 365 F&O**, sistema donde se procesan transacciones financieras, órdenes de carga y datos protegidos de clientes. La principal preocupación es que fallas en el control de acceso de personal remoto o contratistas generen fugas de información protegida (violando normas HIPAA/GDPR) o alteraciones no autorizadas en datos operativos, afectando la continuidad del servicio y la reputación de la empresa.

---

## 2. Declaración de Supuestos

Para el desarrollo de esta matriz, se establecen los siguientes supuestos razonables sobre la infraestructura y procesos de LSG:

1. **Supuesto 1 (Operación remota):** El 60% del personal operativo de BPO y desarrollo trabaja bajo modalidad híbrida/remota accediendo al ERP mediante conexiones VPN y credenciales corporativas de Azure Active Directory (Entra ID).
2. **Supuesto 2 (Rotación de personal):** Al ser una empresa del sector BPO/Nearshoring, existe una rotación de personal operativo de aproximadamente el 15-20% anual en cargos de soporte y analistas de datos.
3. **Supuesto 3 (Integración de sistemas):** Las aplicaciones propietarias como LeanTek y herramientas de telefonía IP consumen datos e interfaces (APIs) directamente conectadas a Microsoft Dynamics 365.
4. **Supuesto 4 (Ambientes de desarrollo):** Existen entornos de pruebas (Sandbox/QA) que contienen réplicas o volcados de bases de datos de producción para pruebas de software y automatizaciones de IA.

---

## 3. Identificación de Activos / Procesos Concretos

1. **Activo 1 (Aplicación / ERP):** Plataforma *Microsoft Dynamics 365 Finance & Operations* (módulo financiero y logístico).
2. **Activo 2 (Información / Base de Datos):** Base de datos con registros transaccionales e información confidencial de clientes en EE. UU. (datos protegidos bajo HIPAA/NDAs).
3. **Activo 3 (Proceso de TI):** Proceso de desvinculación y baja de usuarios de la infraestructura corporativa (*Offboarding*).
4. **Proceso 4 (Proceso de Negocio):** Proceso de autorización y control de cambios en la configuración del ERP y paso a producción.
5. **Activo 5 (Infraestructura / Credenciales):** Directorio Activo en la Nube (*Azure AD / Entra ID*) y sistemas de autenticación de acceso remoto.

---

## 4. Matriz de Análisis de Riesgos

*Leyenda de Escala:*  
- **Probabilidad (1 a 5):** 1: Raro | 2: Poco probable | 3: Posible | 4: Probable | 5: Muy probable  
- **Impacto (1 a 5):** 1: Menor | 2: Limitado | 3: Relevante | 4: Interrupción/Exposición sensible | 5: Grave/Sanción o caída crítica  
- **Nivel de Riesgo:** Probabilidad × Impacto (Ordenado de mayor a menor prioridad)

---

### Riesgo 1: Fuga de datos de salud/financieros por uso de cuentas de excolaboradores
- **Activo / Proceso afectado:** Activo 3 (Proceso de desvinculación / *Offboarding*) y Activo 2 (Base de datos confidencial en Dynamics 365).
- **Amenaza:** Excolaborador desvinculado que intenta ingresar a la red corporativa para extraer información o sabotear operaciones. *(Agente externo/interno no autorizado).*
- **Vulnerabilidad:** Ausencia de sincronización automática entre el sistema de Gestión Humana y Azure AD, permitiendo que cuentas sigan activas días después del retiro.
- **Descripción del Riesgo:** Un excolaborador aprovecha que su cuenta corporativa no fue deshabilitada al momento de su renuncia e ingresa remotamente al ERP, descargando bases de datos de clientes estadounidenses, lo que ocasiona sanciones legales por HIPAA/GDPR y demandas contractuales.
- **Probabilidad:** 4 (Probable)
- **Impacto:** 5 (Grave)
- **Nivel de Riesgo:** **20 (Muy Alto)**
- **Control Actual / Evidencia de Inexistencia:** No existe automatización. El reporte de retiros se envía por correo semanalmente al equipo de Helpdesk para deshabilitación manual (Evidencia: correos con retrasos de 3 a 5 días en la aplicación de bajas).
- **Control Propuesto:** Implementar un flujo automatizado en *Azure AD (Lifecycle Workflows)* integrado con la plataforma de RH para la desactivación inmediata de credenciales al registrar la liquidación, complementado con auditorías mensuales de cuentas activas sin contrato vigente.
- **Justificación de la Prioridad:** Prioridad Máxima (20). Representa el mayor riesgo reputacional y financiero de LSG por tratarse de transferencias transfronterizas reguladas.

---

### Riesgo 2: Exposición de datos de producción en entornos de pruebas sin cifrado
- **Activo / Proceso afectado:** Activo 2 (Base de datos con información de clientes) y Supuesto 4 (Entornos Sandbox/QA).
- **Amenaza:** Ciberdelincuente o desarrollador no autorizado que accede a un ambiente de pruebas menos protegido. *(Infiltrado o fuga de información).*
- **Vulnerabilidad:** Copia directa de la base de datos de producción hacia ambientes de desarrollo sin aplicar técnicas de enmascaramiento o anonimización.
- **Descripción del Riesgo:** Al probar un nuevo módulo de software en el entorno Sandbox, se utiliza una copia idéntica de la base de datos de producción con datos reales de clientes. Un desarrollador o un atacante con acceso al entorno de pruebas filtra esta información, generando una brecha de seguridad.
- **Probabilidad:** 3 (Posible)
- **Impacto:** 5 (Grave)
- **Nivel de Riesgo:** **15 (Alto)**
- **Control Actual / Evidencia de Inexistencia:** Inexistencia de política de sanitización. Los desarrolladores clonan las bases de datos de producción directamente mediante scripts para realizar pruebas de integración (Evidencia: repositorios de QA con datos reales identificables).
- **Control Propuesto:** Aplicar enmascaramiento dinámico de datos (*Dynamic Data Masking*) y anonimización obligatoria mediante scripts estandarizados antes de restaurar cualquier base de datos en entornos que no sean de producción.
- **Justificación de la Prioridad:** Prioridad Alta (15). Ataca directamente el cumplimiento con el marco ISO 27001 (Anexo A.14 / Seguridad en desarrollo).

---

### Riesgo 3: Alteración de parámetros financieros por asignación excesiva de privilegios
- **Activo / Proceso afectado:** Activo 1 (Microsoft Dynamics 365 F&O) y Activo 5 (Directorio Activo).
- **Amenaza:** Empleado con privilegios mal configurados que modifica tablas maestras o parámetros de facturación. *(Modificación no autorizada).*
- **Vulnerabilidad:** Acumulación de roles de usuario en Dynamics 365 sin respetar el principio de menor privilegio ni la segregación de funciones (SoD).
- **Descripción del Riesgo:** Un analista de soporte cuenta con permisos de administrador en el ERP y modifica por error la parametrización de impuestos o cuentas de cobro de un cliente, provocando errores en los reportes financieros e inconsistencias en la facturación.
- **Probabilidad:** 3 (Posible)
- **Impacto:** 4 (Interrupción / Sensible)
- **Nivel de Riesgo:** **12 (Alto)**
- **Control Actual / Evidencia de Inexistencia:** Revisión informal de permisos. Se otorgan roles de "Administrador de Sistema" para resolver tickets rápidamente (Evidencia: matriz de permisos donde 15 usuarios operativos tienen permisos de administración).
- **Control Propuesto:** Rediseñar la matriz de roles y perfiles en Dynamics 365 bajo el principio de menor privilegio, e implementar revisiones trimestrales obligatorias de accesos con aprobación formal de los dueños del proceso (Alineado con COBIT APO12).
- **Justificación de la Prioridad:** Prioridad Alta (12). Previene errores operativos y fraude interno en la gestión del ERP corporativo.

---

### Riesgo 4: Caída del servicio por pases a producción sin pruebas ni autorización
- **Activo / Proceso afectado:** Proceso 4 (Control de cambios en el ERP y desarrollos LeanTek).
- **Amenaza:** Error humano durante la actualización de un componente de software en el entorno de producción. *(Falla técnica/humana).*
- **Vulnerabilidad:** Falta de un Comité de Control de Cambios (CAB) formal y ausencia de planes de reversión (*rollback*) documentados.
- **Descripción del Riesgo:** Un equipo de desarrollo despliega una actualización en el módulo de telefonía o integraciones del ERP directamente en horario laboral sin validación previa. El cambio genera incompatibilidad, interrumpiendo el servicio durante 4 horas y haciendo incumplir los SLAs 24/7 de los clientes.
- **Probabilidad:** 3 (Posible)
- **Impacto:** 4 (Interrupción / Sensible)
- **Nivel de Riesgo:** **12 (Alto)**
- **Control Actual / Evidencia de Inexistencia:** Inexistencia de ventana de cambios estandarizada. Los pases a producción los realiza el líder técnico según la disponibilidad de tiempo (Evidencia: ausencia de registros de aprobación o actas de cambios).
- **Control Propuesto:** Formalizar la práctica de Gestión de Cambios basada en ITIL v4, exigiendo que todo cambio en producción cuente con la aprobación del CAB, pruebas documentadas en QA y un plan de *rollback* verificado antes del despliegue.
- **Justificación de la Prioridad:** Prioridad Alta (12). Responde a la necesidad operativa de mantener la continuidad de negocio y los SLAs contractuales.

---

### Riesgo 5: Secuestro de sesiones de acceso remoto por falta de autenticación robusta
- **Activo / Proceso afectado:** Activo 5 (Directorio Activo / Azure AD) e Infraestructura de Acceso Remoto.
- **Amenaza:** Ataque de *Phishing* o ingeniería social dirigido a colaboradores remotos para robar credenciales. *(Ataque cibernético).*
- **Vulnerabilidad:** Permiso de autenticación basada únicamente en usuario y contraseña para ciertos usuarios operativos en sedes satélite o trabajo remoto.
- **Descripción del Riesgo:** Un analista remoto ingresa sus credenciales en un sitio web falso de phishing. Al no tener configurado el Factor Múltiple de Autenticación (MFA), el atacante toma el control de la sesión, ingresa al Directorio Activo y escala privilegios dentro de la red corporativa de LSG.
- **Probabilidad:** 2 (Poco probable)
- **Impacto:** 5 (Grave)
- **Nivel de Riesgo:** **10 (Medio)**
- **Control Actual / Evidencia de Inexistencia:** MFA habilitado de manera opcional o únicamente para el personal directivo (Evidencia: configuración de políticas de acceso condicional donde solo el 30% del personal tiene MFA activo).
- **Control Propuesto:** Implementar la directiva de Acceso Condicional en Azure AD que exija Autenticación de Múltiple Factor (MFA) obligatoria e intransferible para el 100% de los colaboradores que accedan a la red corporativa de manera remota.
- **Justificación de la Prioridad:** Prioridad Media (10). Aunque la probabilidad disminuye con controles básicos, el impacto es crítico si se llega a consumar un ataque de *Ransomware* (Alineado con NIST CSF - Proteger).

---

## 5. Matriz Resumen de Priorización

| ID | Activo / Proceso Afectado | Riesgo Identificado | Prob. (P) | Imp. (I) | Nivel (P×I) | Clasificación |
| :-: | :--- | :--- | :-: | :-: | :-: | :--- |
| **R1** | Proceso de Bajas / Base de Datos | Fuga de datos confidenciales por cuentas de excolaboradores activas. | 4 | 5 | **20** | **Muy Alto (Seleccionado)** |
| **R2** | Entornos QA / Base de Datos | Exposición de datos reales de clientes en ambientes de desarrollo sin cifrar. | 3 | 5 | **15** | **Alto** |
| **R3** | ERP Dynamics 365 / Permisos | Modificación no autorizada de parámetros financieros por permisos excesivos. | 3 | 4 | **12** | **Alto** |
| **R4** | Proceso de Control de Cambios | Indisponibilidad del servicio por pases a producción no probados (Caída de SLA). | 3 | 4 | **12** | **Alto** |
| **R5** | Azure AD / Credenciales Remotas | Intrusión por Phishing debido a la falta de MFA obligatorio en personal remoto. | 2 | 5 | **10** | **Medio** |

---

# PARTE 2: PLAN DE AUDITORÍA DEL RIESGO PRIORIZADO (R1)

## 1. Identificación del Plan

* **Organización:** Lean Solutions Group (LSG)
* **Sistema / Proceso Evaluado:** Gestión de Accesos e Identidades (IAM) en el proceso de desvinculación de personal (*Offboarding*) para la plataforma *Microsoft Dynamics 365 F&O* y *Azure AD / Entra ID*.

---

## 2. Contexto y Riesgo Seleccionado (R1)

### Contexto del Caso
Lean Solutions Group opera como BPO y proveedor *nearshore* para más de 500 clientes en EE. UU. (sectores de salud, finanzas y logística). Maneja un ecosistema en la nube (*Azure* y *Dynamics 365*) con alta rotación de personal operativo. La mayor preocupación es la filtración de datos sensibles por controles deficientes en el retiro de accesos remotos.

### Ficha del Riesgo Seleccionado de la Matriz (R1)
* **ID:** **R1** (Nivel de Riesgo: **20 / 25 - Muy Alto**)
* **Activo Afectado:** Proceso de *Offboarding* (Activo 3) y Base de Datos Confidencial en Dynamics 365 (Activo 2).
* **Amenaza:** Excolaborador desvinculado que intenta ingresar a la red corporativa.
* **Vulnerabilidad:** Falta de sincronización automática entre el sistema de Gestión Humana y Azure AD (bajas manuales diferidas).
* **Consecuencia:** Fuga de datos de clientes estadounidenses, resultando en sanciones por HIPAA/GDPR y pérdida de contratos.
* **Control Actual / Evidencia de Inexistencia:** Gestión manual por correo electrónico al Helpdesk (Evidencia: solicitudes con retrasos de 3 a 5 días en la deshabilitación).
* **Justificación de Prioridad:** Es la vulnerabilidad operativa de mayor riesgo reputacional y legal debido a la transferencia transfronteriza de datos.

---

## 3. Objetivo de Auditoría

> **Determinar si las cuentas de acceso corporativo (Azure AD) y permisos en Microsoft Dynamics 365 F&O asignados al personal desvinculado de Lean Solutions Group son desactivados de manera oportuna (plazo máximo de 2 horas hábiles) conforme al procedimiento de desvinculación vigente durante el tercer trimestre de 2026.**

---

## 4. Alcance Funcional y Exclusiones

1. **Proceso:** Subproceso de baja y deshabilitación de accesos lógicos (*Offboarding*).
2. **Componentes:** *Microsoft Entra ID (Azure AD)*, *Microsoft Dynamics 365 F&O* y logs de autenticación.
3. **Población:** Total de colaboradores y contratistas retirados en el periodo evaluado.
4. **Periodo:** 1 de julio de 2026 al 30 de septiembre de 2026 (Q3 2026).
5. **Responsables Entrevistados:** Líder de Gestión Humana, CISO/Líder de TI y Administrador del ERP.
6. **Exclusiones:** Seguridad física de las instalaciones y proceso de contratación (*Onboarding*).

---

## 5. Criterios de Evaluación

* **Criterio Interno:** Deshabilitación de accesos en un máximo de **2 horas hábiles** posteriores al retiro efectivo (Política interna de IAM por solicitar).
* **ISO/IEC 27001 (Anexo A.9.2.6 / A.5.18):** Retiro inmediato de derechos de acceso a la información al finalizar la relación laboral.
* **COBIT 2019 (DSS05.04):** Gestión oportuna de altas, bajas y modificaciones de privilegios de acceso.

---

## 6. Programa de Trabajo

| ID | Control a Evaluar | Criterio y Fuente | Procedimiento Paso a Paso | Población / Muestra | Evidencia Esperada | Posibles Excepciones |
| :-: | :--- | :--- | :--- | :--- | :--- | :--- |
| **P1** | Diseño del procedimiento de desvinculación. | Política interna IAM / ISO 27001 (Control A.9.2.6). | **1.** Revisar la política de IAM vigente.<br>**2.** Entrevistar a los líderes de RH y TI.<br>**3.** Verificar la existencia de flujos automatizados o SLAs de atención. | Documentación formal del proceso IAM. | Política escrita de IAM, SOP de bajas y diagrama del flujo de trabajo. | Ausencia de política formal o falta de SLA definido para la deshabilitación. |
| **P2** | Operación efectiva de la desactivación de cuentas. | Tolerancia máx. de 2 horas tras el retiro (Política interna). | **1.** Solicitar el reporte oficial de bajas a RH.<br>**2.** Exportar el estado de cuentas en Azure AD y Dynamics 365.<br>**3.** Cruzar fechas (Retiro vs. Desactivación).<br>**4.** Revisar *Sign-in logs* en casos de retraso. | 100% de colaboradores retirados en el Q3 2026 (~50 registros). | Listado de bajas de RH, exportación de Azure AD e historial de *Sign-in logs*. | Cuentas activas días después del retiro; inicios de sesión exitosos posteriores a la liquidación. |

---

## 7. Organización y Cronograma
[Semana 1: Preparación] ➔ [Semana 2: Solicitud de Información] ➔ [Semana 3: Pruebas y Cruce de Datos] ➔ [Semana 4: Cierre]

* **Responsable:** Salomón Camargo Londoño (Auditor Líder y Técnico).
* **Dependencias:** Entrega oportuna de la base de bajas por RH y exportaciones de logs por el equipo de TI.

---

## 8. Supuestos y Limitaciones

* **Hechos Comprobados:** LSG opera como BPO *nearshore* regulado utilizando la suite de Microsoft.
* **Supuestos Declarados:** Se estima una población aproximada de 50 bajas en el trimestre Q3 2026.
* **Información por Solicitar:** Versión firmada de la Política de IAM y listado real de desvinculaciones.
* **Propuesta del Equipo:** Objetivo, alcance, diseño del programa de trabajo y matriz de trazabilidad.