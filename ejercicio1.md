



## ✅ EJERCICIO 1 — From "Startup MVP" to Producción

### 🎯 Objetivo

Elegir correctamente entre PaaS, IaaS y Serverless, y justificar la decisión arquitectónica.

---

## Solución: From "Startup MVP" to Producción


### 🔄 Evolución Obligatoria (a los 6 meses)


**Pregunta clave:** ¿Qué debería haber hecho diferente desde el inicio?

***Respuesta***: Se debería haber tenido en cuenta el contexto de start-up y que en un tiempo razonable van a crecer tanto en trafico y servicis. Se debe negociar un sistema de facturación que beneficie el crecimiento de la compañia sin que salga muy costoso para los servicios que se consumen para aumentar la flexibilidad y al mismo tiempo revisar alternativas (ej. App nativa en la nube, Almacenamiento NoSQL,) 


## 🏗️ Arquitectura Inicial que Deben Proponer

**Deben evaluar y justificar la elección entre:**

| Componente | Opciones | Consideración |
|-----------|----------|----------------|
| **Computación** | Azure App Service | Debido al poco numeor de desarrolladores sin experiencia DevOps y despliegue en la nube. LA aplicacipón se despliega tal y como fue diseñada mientras el equipo se enfoca totalmente en la logica de las reservas. |
| **Base de datos** | Azure SQL Database |Si tenemos una base de datos relacional que funciona, la estrategia inicial sería dejarlo tal y como está. Se está considerando que es un equipo pequeño y Azure SQL DB se encarga de gestionar la seguridad, backups automaticos configurables y con una muy alta disponibildiad que permite hacer auditoria. |
| **Gestión de secretos** | Azure Key Vault vs variables de entorno | Lo mejor sería hacer un balance entre las variables de entorno, y Azure Key Vault. Se puede autorizar a Azure Key Vault de que lea estos secretos y puede ser un hibrido para que no se modifique tanto el cdigo y se cumpla con el tiempo establecido.|
| **Observabilidad** | Log Analytics + Application Insights | Se requiere un nivel de monitoreo alto dado que la aplicación debe tener un tiempo de sisponibilidad bastante alto. No se puede disponer de muchos recursos solo para determinar si hay un bug en las reservas. El servicio de App Insigutes no es muy costoso y es más beneficio que costo lo que supone.|

**Servicios clave de Azure a considerar:**

- Azure App Service
- Azure Functions
- Azure SQL Database
- Azure Key Vault
- Log Analytics Workspace
- Application Insights

---

### 📌 Contexto

Una startup lanza una **API para reservas online** con los siguientes requisitos:

- Frontend web + API REST
- Base de datos relacional
- Autenticación con secretos
- Logging y métricas básicas
- **Presupuesto muy bajo**
- **Equipo pequeño sin experiencia en Kubernetes**

---

### 🧩 Restricciones

- ⏰ Time to market muy corto (lanzamiento en 4 semanas)
- 📊 Tráfico bajo los primeros 3 meses
- 👥 Sin DevOps dedicado (solo 2 desarrolladores)
- 🔒 Cumplir requisitos mínimos de seguridad
- 💰 Presupuesto limitado para infraestructura

---

### 🏗️ Arquitectura Inicial que Deben Proponer

**Deben evaluar y justificar la elección entre:**

| Componente | Opciones | Consideración |
|-----------|----------|----------------|
| **Computación** | Azure App Service vs Azure Functions vs VM (IaaS) | ¿Tiempo de desarrollo vs control? |
| **Base de datos** | Azure SQL Database vs Azure Database for MySQL vs Cosmos DB | ¿Relacional o NoSQL? |
| **Gestión de secretos** | Azure Key Vault vs variables de entorno | ¿Cómo manejar credenciales? |
| **Observabilidad** | Log Analytics + Application Insights vs Métricas básicas | ¿Qué nivel de monitoring? |

**Servicios clave de Azure a considerar:**

- Azure App Service
- Azure Functions
- Azure SQL Database
- Azure Key Vault
- Log Analytics Workspace
- Application Insights

---

### 🔄 Evolución Obligatoria (a los 6 meses)

Después del lanzamiento inicial, la startup enfrenta un nuevo escenario:

- 📈 **10x tráfico** respecto al mes de lanzamiento
- 🔴 **Picos fuertes** en campañas de marketing
- 📋 **SLA más exigente** (99.9% uptime)
- 💸 **Primeros problemas de coste** (factura inesperada)
- 🐛 **Problemas de rendimiento** en lectura de datos
- 🔐 **Nuevos requisitos de cumplimiento** (GDPR, auditoría)

**Pregunta clave:** ¿Qué debería haber hecho diferente desde el inicio?

---


### ❓ Preguntas para los Alumnos

Trabajando en grupos, respondan:

1. **¿App Service o Functions?**
   - ¿Cuál es mejor para una API REST?
   - ¿Qué implicaciones tiene cada uno para escalado?
      * Para el contexto de una API Rest, Web Apps etc. lo mejor es usar App Service. Con App Service se puede determinar tanto el escalado (Horizontal y vertical) y el coste, además de poder desplegar la aplicación en contenedores de docker. Provee una disponibilidad bastante alta y capacidad de gestionar la seguridad de multiples formas. Functions se utiliza para funciones concretas, en el contexto de la app de reservas, Azure Functions puede ejecutarse en una funcion integrada con un trigger, por ejemplo, el evento de generar un ticket.
   

2. **¿Single DB o escalar lectura?**
   - ¿Cómo manejar la replicación?
   - ¿Read replicas o sharding?
      * En este contexto usar ambas. Al inicio, con un trafico leve, empezar usando Single DB. Con un trafico bajo el objetivo principal es tener la aplicación funcionando el mayor tiempo posible y se puede lograr perfecramente con una Sngle DB. Una vez el trafico aumente el primer punto de fallo va a ser la Api de consultas. Una buena estrategia es hacer replcias de lectura. Read Replicas. Dividir toda la logica del negocio en un Writer y las consultas menos criticas como lsitar reservas en una Replica de Lectura. Sharding no se usa en este contexto ya que la Start-Up está lejos de tener los limites practicos y volumenes de datos de la industria para implementarlo necesariamente.

3. **¿Dónde almacenan secretos?**
   - ¿Por qué usar Key Vault?
   - ¿Cuál es el riesgo de no usarlo?
      * Se propone usar Azure Key Vault dado que con key Vault se pueden manejar los secretos de la aplicación con facilidad sin necesidad de cambiar demasiado el codigo. Con la App ya gestionada en la nube se puede implementar con relativa rapidez y un costo bajo. El riesgo de no usar Key Vault es tener Secretos manejados dentro de la maquina lo que puede ser un riesgo de seguridad teniendo en cuenta que los desarrolladores e ingenieros tienen acceso a estos recursos. Tambien limita la flexibilidad para implementar otro tipo de servicios.

4. **¿Qué parte se vuelve el cuello de botella?**
   - ¿La computación, la BD o la red?
   - ¿Cómo lo descubren?
      * La BD puede ser el prinicpal cuello de botella. En un punto con el crecimiento del numero de usuarios, se harán cada vez más y más consultas lo que puede aumentar los tiempos de espera entre reservas.

5. **¿Qué cambiarían primero al escalar?**
   - Priorizar acciones según ROI
   - Considerar tiempo de implementación
      * Al escalar lo primero es seguir una serie de objetivos primarios. 1. Optimizar consultas, esto resuelve los tiempos de espera en la base de datos y no suele tardar mucho. 2. Implementar estrategias de caching para consultas repetidas 3. Escalamiento vertical, reducir cuello de botella en CPU/RAM o DB al 90% 4. Implementación de Read Replicas. Y por ultimo como ultimo recurso si el caché ya no es suficiente probar implementar Cache distribuida (Redis)
---



