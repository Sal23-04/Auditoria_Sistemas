# Taller Final: Informe de Auditoría y Plan de Acción
**Asignatura:** ET0114 · Auditoría de Sistemas  
**Docente:** Juan Duque  
**Fecha:** 24 de noviembre de 2026  

**Integrantes:**
* Salomón Camargo Londoño

---

## 1. Identificación

* **Organización / Caso:** Lean Solutions Group (LSG).
* **Sistema / Proceso Evaluado:** Proceso de Gestión de Accesos e Identidades (IAM) — Subproceso de desvinculación de usuarios (*Offboarding*) en *Microsoft Entra ID (Azure AD)* y el ERP *Microsoft Dynamics 365 Finance & Operations*.
* **Naturaleza del Ejercicio:** **Simulación Académica.** Dado que no se cuenta con autorización ni acceso a los registros internos reales de la empresa, las pruebas, registros de usuarios, logs y resultados presentados en este informe corresponden a un conjunto de datos ficticios diseñados de forma coherente con el caso para demostrar la ejecución metodológica de la auditoría.

---

## 2. Resumen Ejecutivo

Durante el desarrollo de la asignatura se evaluó el ecosistema tecnológico de **Lean Solutions Group (LSG)**, empresa de servicios *nearshore* y BPO con operaciones transfronterizas entre Colombia y Estados Unidos. Tras seleccionar los marcos de referencia (COBIT 2019, ISO/IEC 27001, NIST CSF e ITIL v4) y construir la matriz de riesgos, se priorizó el riesgo **R1 (Nivel 20/25 - Muy Alto)**: *fuga de información confidencial de clientes por el uso de cuentas activas de excolaboradores tras su desvinculación*.

Para ejecutar el plan de auditoría sobre el tercer trimestre de 2026 (Q3), se diseñó y aplicó una **prueba simulada** de cumplimiento sobre una muestra de 10 colaboradores retirados, cruzando los registros simulados de Gestión Humana contra la exportación de estado de cuentas de *Azure AD* y los registros de inicio de sesión (*Sign-in logs*).

* **Resultado Principal (Simulado):** De las 10 cuentas revisadas, **2 cuentas (20% de la muestra)** excedieron el criterio interno simulado de desactivación máxima de 2 horas hábiles tras el retiro (una se desactivó 4 días después y otra permanecía activa al cierre de la revisión), lo que originó el hallazgo **H-01**. En los logs simulados se detectó un inicio de sesión exitoso posterior al retiro en una de las cuentas, aunque no se pudo confirmar extracción de datos.
* **Acción Propuesta:** Implementar flujos automatizados de ciclo de vida (*Lifecycle Workflows*) en *Microsoft Entra ID* integrados con el sistema de Gestión Humana y ejecutar conciliaciones mensuales obligatorias de cuentas activas sin contrato vigente.

---

## 3. Objetivo y Alcance

### Objetivo de Auditoría
> **Determinar si las cuentas de acceso corporativo en Microsoft Entra ID (Azure AD) y los permisos asociados en Microsoft Dynamics 365 F&O del personal desvinculado de Lean Solutions Group fueron desactivados oportunamente conforme al procedimiento vigente durante el tercer trimestre de 2026.**

### Alcance Funcional y Exclusiones
* **Proceso Evaluado:** Subproceso de desvinculación y revocación de credenciales lógicas (*Offboarding*).
* **Sistemas y Componentes:** Directorio Activo en la nube (*Azure AD / Entra ID*), asignación de roles en *Microsoft Dynamics 365 F&O* y registros de autenticación (*Sign-in Logs*).
* **Periodo Revisado:** 1 de julio de 2026 al 30 de septiembre de 2026 (Q3 2026).
* **Población y Muestra:** Población real desconocida por falta de acceso interno. Para efectos de la simulación, se definió una población ficticia de 48 retiros en el trimestre y se seleccionó una **muestra dirigida de 10 casos** (7 analistas operativos remotos y 3 usuarios con roles financieros en Dynamics 365).
* **Inclusiones:** Verificación de marcas de tiempo (*timestamps*) entre la liquidación laboral y la deshabilitación técnica de la cuenta, así como revisión de intentos de acceso posteriores al retiro.
* **Exclusiones:** Proceso de contratación (*Onboarding*), gestión de contraseñas de personal activo y controles de acceso físico (carnés o torniquetes) a las sedes de LSG. *Limitación:* La exclusión de controles físicos implica que la conclusión se restringe exclusivamente al acceso lógico remoto y en la nube.

---

## 4. Criterios y Metodología

### Criterios de Evaluación
1. **Criterio Interno (Simulado para el ejercicio):** Según la *Política Ficticia de Gestión de Identidades y Accesos (POL-IAM-04)* planteada para esta simulación, toda cuenta corporativa debe ser deshabilitada en un plazo máximo de **2 horas hábiles** posteriores a la hora efectiva de terminación del contrato notificada por Gestión Humana.
2. **Criterio Normativo — ISO/IEC 27001:2022 (Control A.5.18 / A.9.2.6):** Los derechos de acceso de todos los empleados y partes externas a la información y activos de procesamiento deben ser retirados a la terminación de su empleo, contrato o acuerdo.
3. **Criterio de Gobierno — COBIT 2019 (DSS05.04):** Asegurar que las modificaciones y revocaciones de derechos de acceso se ejecuten de manera oportuna y trazable.

### Metodología y Procedimientos Ejecutados (Simulación)
1. **Evaluación del Diseño (P1 - Simulado):** Revisión documental de la política ficticia `POL-IAM-04` y validación mediante entrevista simulada con los líderes de Gestión Humana y TI, confirmando que la notificación de bajas se realiza por correo electrónico hacia el Helpdesk sin integración automática.
2. **Prueba Sustantiva de Operación (P2 - Simulado):** Extracción y cruce tabular entre el reporte simulado de retiros de Gestión Humana (`Reporte_Bajas_Q3_2026.csv`) y el reporte simulado de estado de cuentas del directorio (`Export_AzureAD_Q3.csv`). Para los casos con desviación superior a 2 horas, se inspeccionaron las trazas simuladas en `Azure_Signin_Logs_Q3.csv`.

---

## 5. Resultados y Hallazgos

### Resumen de Resultados de la Prueba (Muestra Simulada)
* **Total de cuentas examinadas:** 10 cuentas de excolaboradores retirados en Q3 2026.
* **Cuentas conformes (desactivadas ≤ 2 horas hábiles):** 8 cuentas (80% de la muestra).
* **Excepciones identificadas (> 2 horas hábiles):** 2 cuentas (20% de la muestra), detalladas en el Papel de Trabajo PT-01 (Anexo).

### Hallazgo Sustentado

#### **H-01: Desactivación tardía y omisión de baja de cuentas corporativas tras el retiro laboral**

1. **Criterio:** La política interna simulada de gestión de accesos (`POL-IAM-04`) y el control **A.5.18 de ISO/IEC 27001** exigen que las cuentas corporativas sean deshabilitadas dentro de las **2 horas hábiles** siguientes a la terminación efectiva del vínculo laboral.
2. **Condición:** En la prueba ejecutada sobre la muestra simulada de 10 excolaboradores del Q3 2026 (Papel de Trabajo `PT-01`), se observó que **2 cuentas incumplieron el tiempo establecido**:
   * El usuario `USR-04` (Analista BPO Salud) fue liquidado el 14/08/2026 a las 17:00, pero su cuenta en Azure AD fue deshabilitada el 18/08/2026 a las 11:30 (**4 días calendario / ~26 horas hábiles de retraso**). Adicionalmente, los *Sign-in logs* simulados registraron un inicio de sesión exitoso desde una IP externa el 16/08/2026.
   * El usuario `USR-09` (Auxiliar Contable ERP) fue retirado el 25/09/2026 a las 16:00 y al cierre de la extracción de datos (30/09/2026) su cuenta figuraba aún en estado **`Enabled` (Activa)** en Azure AD y Dynamics 365, aunque sin registros de inicio de sesión posteriores al retiro.
3. **Causa:** **Pendiente de confirmar.** Aunque en la simulación se presume una falla por procesamiento manual de correos entre Gestión Humana y Helpdesk, no se cuenta con evidencia documental verificada sobre la causa raíz administrativa o técnica real en la organización.
4. **Efecto o Riesgo:**
   * *Consecuencia observada (en la simulación):* Exposición efectiva de la sesión corporativa del usuario `USR-04` durante 4 días, con un acceso autenticado ocurrido fuera de la relación laboral.
   * *Riesgo potencial:* Un excolaborador o un tercero con sus credenciales puede acceder remotamente a *Dynamics 365* y descargar bases de datos protegidas de clientes en EE. UU., generando incumplimientos legales (HIPAA/GDPR), sanciones económicas y pérdida de contratos.
5. **Recomendación:** Automatizar la revocación de credenciales vinculando el sistema de Gestión Humana con los flujos de ciclo de vida (*Lifecycle Workflows*) de *Microsoft Entra ID* para deshabilitar cuentas de forma inmediata al registrar el retiro, e instaurar un control detectivo mensual de conciliación de usuarios activos contra nómina vigente.

---

## 6. Conclusión y Limitaciones

### Conclusión
Con base en la evidencia simulada examinada en el Papel de Trabajo `PT-01`, se concluye que el control de desactivación de accesos tras la desvinculación presenta **debilidades significativas en su operación**, evidenciadas en una tasa de excepción del 20% dentro de la muestra evaluada (2 de 10 casos fuera de criterio). De replicarse este comportamiento en un entorno real, Lean Solutions Group estaría expuesta a un nivel de riesgo muy alto de fuga de información transfronteriza y accesos no autorizados en *Dynamics 365*.

### Limitaciones del Trabajo
* **Carácter Simulado:** Los datos, registros de usuarios, fechas y trazas de inicio de sesión fueron creados con fines exclusivamente académicos. Los resultados demuestran la aplicabilidad de la metodología de auditoría, pero **no representan una evaluación real** del cumplimiento de Lean Solutions Group.
* **Alcance Muestral:** Debido a que se analizó una muestra dirigida de 10 casos ficticios, los porcentajes obtenidos no pueden extrapolarse estadísticamente a toda la población real de empleados de la compañía.
* **Confirmación de Causas:** Las causas de las desviaciones permanecen como pendientes de confirmar al no haberse realizado inspecciones reales ni descargos formales con el personal de la empresa.

---

## 7. Plan de Acción

| ID del Hallazgo | Acción Propuesta | Responsable Propuesto | Prioridad y Justificación | Plazo Propuesto | Evidencia de Cierre |
| :---: | :--- | :--- | :--- | :--- | :--- |
| **H-01** | **1. Mitigación inmediata:** Deshabilitar de inmediato las cuentas activas de personal retirado (`USR-09`) y auditar la actividad realizada durante la ventana de exposición (`USR-04`).<br>**2. Automatización:** Configurar *Lifecycle Workflows* en *Microsoft Entra ID* conectados al sistema de RH para la baja automática al registrar la fecha de retiro.<br>**3. Control detectivo:** Realizar conciliaciones mensuales entre nómina activa y usuarios habilitados en Azure AD / Dynamics 365. | Líder de Seguridad de la Información (CISO) y Administrador de Identidades (TI), en coordinación con Gestión Humana. | **Alta (Crítica).** Responde al riesgo **R1 (20/25)**; una cuenta activa tras el retiro permite el acceso remoto directo a datos financieros y de salud regulados por HIPAA/GDPR. | **5 días hábiles** para depuración inmediata.<br><br>**45 días calendario** para automatización y primer control mensual *(sujeto a aprobación interna)*. | **1.** Capturas/logs de deshabilitación inmediata de cuentas pendientes.<br>**2.** Configuración documentada del flujo automático en Entra ID.<br>**3.** Acta firmada de la primera conciliación mensual RH vs. TI sin excepciones. |

---

## 8. Referencias y Anexos

### Referencias y Trazabilidad de Entregables Anteriores
* **Taller 1 (Marcos de Referencia):** Selección y justificación de COBIT 2019, ISO/IEC 27001, NIST CSF e ITIL v4 para Lean Solutions Group.
* **Taller 2 (Matriz Inicial de Riesgos):** Fila **R1** — *Fuga de datos de salud/financieros por uso de cuentas de excolaboradores* (Probabilidad: 4, Impacto: 5, Nivel: 20 - Muy Alto).
* **Taller 3 (Plan de Auditoría):** Objetivo, alcance Q3 2026 y diseño de procedimientos P1 (Diseño) y P2 (Operación).
* **Normatividad Internacional:** ISO/IEC 27001:2022 (Anexo A.5.18) y COBIT 2019 (DSS05.04).

---

### Anexo A: Papel de Trabajo `PT-01` — Prueba de Desactivación de Cuentas (Datos Simulados)

* **Objetivo de la prueba:** Verificar que las cuentas de excolaboradores retirados en Q3 2026 hayan sido deshabilitadas en *Azure AD / Dynamics 365* dentro de las 2 horas hábiles siguientes a su retiro.
* **Fuente de datos (Simulada):** Cruce entre `Reporte_Bajas_Q3_2026.csv` (RH) y `Export_AzureAD_Q3.csv` (TI).

| ID Caso (Ficticio) | Rol / Área | Fecha y Hora Retiro (RH) | Fecha y Hora Baja (Azure AD) | Desviación Estimada | Estado Cuenta | Sign-in Log Posterior | Resultado Prueba |
| :---: | :--- | :--- | :--- | :--- | :---: | :---: | :---: |
| `USR-01` | Analista Logística | 03/07/2026 17:00 | 03/07/2026 17:45 | 45 mins | Disabled | No | **Conforme** |
| `USR-02` | Soporte BPO | 15/07/2026 12:00 | 15/07/2026 13:10 | 1h 10m | Disabled | No | **Conforme** |
| `USR-03` | Auxiliar Financiero | 29/07/2026 17:00 | 29/07/2026 18:30 | 1h 30m | Disabled | No | **Conforme** |
| `USR-04` | **Analista BPO Salud** | **14/08/2026 17:00** | **18/08/2026 11:30** | **~26h hábiles (4d)** | **Disabled** | **Sí (16/08 21:14)** | **Excepción (H-01)** |
| `USR-05` | Analista Datos | 21/08/2026 16:00 | 21/08/2026 16:50 | 50 mins | Disabled | No | **Conforme** |
| `USR-06` | Coordinador Cuenta | 02/09/2026 17:00 | 02/09/2026 18:00 | 1h 00m | Disabled | No | **Conforme** |
| `USR-07` | Analista Logística | 10/09/2026 15:00 | 10/09/2026 16:20 | 1h 20m | Disabled | No | **Conforme** |
| `USR-08` | Especialista ERP | 18/09/2026 17:00 | 18/09/2026 17:15 | 15 mins | Disabled | No | **Conforme** |
| `USR-09` | **Auxiliar Contable ERP** | **25/09/2026 16:00** | **Sin registro (N/A)** | **> 5 días (Abierta)** | **Enabled** | **No** | **Excepción (H-01)** |
| `USR-10` | Soporte Técnico | 29/09/2026 17:00 | 29/09/2026 18:40 | 1h 40m | Disabled | No | **Conforme** |