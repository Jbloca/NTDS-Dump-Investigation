# 🔍 Análisis de Exfiltración de NTDS.dit en el Dominio Forela

## **📝 Descripción del Caso**
El ambiente del dominio de **Forela** se encuentra comprometido debido a un ataque persistente al controlador de dominio (DC). Tras haber detectado previamente la exfiltración de la base de datos **NTDS.dit** mediante la utilidad **vssadmin**, los atacantes lograron acceder nuevamente al DC utilizando **ntdsutil.exe** para volcar la base de datos.  

🎯 **Objetivo**: Identificar los pasos críticos del ataque y extraer información clave para su documentación y prevención.

---

## **❓ Preguntas de Investigación**
1. 🕒 **¿Cuál es la última marca de tiempo en la que el servicio de Microsoft Shadow Copy entró en estado de ejecución, indicando el posible inicio del dumping NTDS?**  
   - Respuesta: *Análisis pendiente.*

2. 📂 **¿Cuál es la ruta completa del archivo NTDS volado?**  
   - Respuesta: `/ruta/completa/identificada/ntds.dit`.

3. 🗓️ **¿Cuándo se creó el vertedero de la base de datos en el disco?**  
   - Respuesta: *Fecha y hora obtenida del análisis del evento.*

4. ✅ **¿Cuándo se consideró completa y lista para su uso la base de datos recién desechada?**  
   - Respuesta: *Fecha y hora obtenida del registro del evento.*

5. 🔍 **¿Qué fuente de eventos proporciona datos de estado de la base de datos como creación y desprendimiento?**  
   - Respuesta: *Nombre de la fuente de eventos.*

6. 🛡️ **Cuando **ntdsutil.exe** se utiliza para voltear la base de datos, ¿qué dos grupos de usuarios se enumeran para validar los privilegios de la cuenta utilizada?**  
   - Respuesta: *Grupos enumerados en orden alfabético: `Administrators Domain Admins`.*

7. ⏱️ **¿Cuál es el Tiempo de Inicio de Sesión de la sesión maliciosa usando el Logon ID?**  
   - Respuesta: *Tiempo de inicio de sesión identificado.*

---

## **🔧 Metodología**
### 1️⃣ Recolección de Evidencia
- **📄 Fuentes de Logs**:  
  - Registros de eventos de Windows (Evtx).  
  - Análisis de logs del controlador de dominio.  

- **🛠️ Herramientas utilizadas**:  
  - Event Viewer.  
  - PowerShell para extraer datos clave.  
  - Registros de ntdsutil.exe y servicios asociados.  

### 2️⃣ Análisis del Servicio de Copia de Sombras (Shadow Copy)  
Identificar las marcas de tiempo de activación del servicio utilizando:  
`powershell
Get-WinEvent -LogName "Application" | Where-Object { $_.Message -like "*Shadow Copy*"}`

### 3️⃣ Verificación de la Creación y Estado del Archivo NTDS
Revisar los registros asociados al proceso de volcado con ntdsutil.exe y buscar:
  - 🛣️ Ruta de volcado.
  - 📅 Eventos que marcan la creación y finalización.

### 4️⃣ Validación de Privilegios
Inspeccionar las enumeraciones realizadas por ntdsutil.exe para los grupos de usuarios privilegiados.

### 5️⃣ Identificación del Tiempo de Inicio de Sesión
Correlacionar el Logon ID con registros de inicio de sesión para determinar la hora exacta.

--- 

### ** 📊 Resultados**
Respuestas a las preguntas clave:
  1. 🕒 Última marca de tiempo del servicio Shadow Copy en ejecución: 2024-05-15 05:39:55.
  2. 📂 Ruta completa del archivo NTDS volado:  C:\Windows\Temp\dump_tmp\Active Directory\ntds.dit.
  3. 🗓️ Fecha de creación del vertedero de la base de datos: 2024-05-15 05:39:56.
  4. ✅ Fecha de finalización de la base de datos volcada: 2024-05-15 05:39:58.
  5. 🔍 Fuente de eventos utilizada para rastrear el estado de la base de datos: ESENT.
  6. 🛡️ Grupos enumerados por ntdsutil.exe: Administrators, Backup Operators.
  7. ⏱️ Tiempo de inicio de sesión malicioso: 2024-05-15 05:36:31

---

### ** 📚 Lecciones Aprendidas
  1. 💡 La persistencia del atacante demuestra la necesidad de un monitoreo continuo en entornos críticos.
  2. 🔒 Es crucial implementar herramientas de detección de amenazas avanzadas para identificar actividades sospechosas relacionadas con ntdsutil.exe.
  3. 👨‍💻 Establecer políticas de privilegios mínimos reduce el impacto de las cuentas comprometidas.

---

### **🚀 Próximos Pasos**
  1. ⚙️ Implementar medidas para restringir el uso de herramientas administrativas como ntdsutil.exe.
  2. 🚨 Configurar alertas automáticas para eventos críticos relacionados con:
    - Activación de Shadow Copy.
    - Modificaciones o accesos a la base de datos NTDS.dit.
  3. 📈 Reforzar las auditorías de seguridad y la capacitación de los administradores de dominio.
---

## **👨‍💻 Autor** 

**[Jorge Balarezo Cardenas]**  
- LinkedIn: [Enlace a mi perfil](https://www.linkedin.com/in/jorge-balarezo-cardenas/)  
- Email: [jbalarezocarden@gmail.com]
