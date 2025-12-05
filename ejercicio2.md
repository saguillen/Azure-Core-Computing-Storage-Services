# Ejercicio 2: Migración de Legacy a Cloud Híbrido

## 🎯 Objetivo

Diseñar una arquitectura híbrida realista con Arc y VMs que permita comenzar una migración gradual sin interrumpir la producción.

---

## 📌 Contexto

Una empresa industrial posee:

- **Aplicación .NET monolítica** en servidores on-premises
- **SQL Server** instalado en infraestructura local
- **8 servidores físicos** ejecutando aplicaciones críticas
- **Backups manuales** sin automatización
- **Sin monitorización centralizada** de la salud de sistemas
- **Objetivo**: Empezar a migrar a Azure sin parar la producción

---

## 🧩 Restricciones

- ⏱️ **Latencia crítica**: Los sistemas no pueden tolerar retrasos significativos
- 🔐 **Datos sensibles**: Información confidencial que requiere cumplimiento normativo
- 🛠️ **No refactorización a corto plazo**: El código legacy no puede modificarse inmediatamente
- 📋 **Ventana de mantenimiento mínima**: Cambios deben realizarse sin parar servicios

---

## 🏗️ Arquitectura Inicial Esperada

Para esta migración inicial deberías considerar el uso de:

- **Azure Virtual Machines**: Para hospedar réplicas de aplicaciones en la nube
- **Azure Backup**: Solución centralizada de backups automatizados
- **Recovery Services Vault**: Almacenamiento de puntos de recuperación
- **Azure Arc**: Gestión unificada de recursos híbridos
- **Log Analytics Workspace**: Centralización de logs y métricas

---

## 🔄 Evolución Obligatoria (12 meses)

La arquitectura debe evolucionar hacia:

- **Migración del backend**: Partes de la aplicación moviéndose a la nube gradualmente
- **Recuperación ante desastres completa**: Plan DR fully operativo
- **Auditoría de seguridad**: Validación de cumplimiento y mejora de postura de seguridad

---

## ❓ Preguntas de Discusión

1. **¿Qué servicios permanecen on-premises y cuáles se trasladan a Azure?**
   - Justifica tu decisión en función de latencia, costo y complejidad
        * Inicialmente se trasladaría la aplicación monolitica para despues empezar a mover una a una las aplicaciónes criticas a App Service. Por ultimo dado que es lo más complejo y lo que puede tardar sería la base de datos en SQL Server a Azure SQL. Todo esto caundo se ha implementado Azure Arc para la gestión hibrida de los servicios que ya están en la nube con los que están on-premise.

2. **¿Cómo se gestionarán los parches y las actualizaciones del SO?**
   - Considera la uniformidad entre ambos entornos
        * Se propone usar Azure Arc gestionando unificadamente todo lo que está on-premise. Cuando esto ya suceda se unifica con la nube y con lo que se pueda empezar a pasar a Serverless. En caso de que no suceda por costes, tambien por el medio de  Arc se gestiona localmente.

3. **¿Dónde se centralizarán los logs y las métricas de monitorización?**
   - Define una estrategia de observabilidad híbrida
        * 1. El uso se Log Analytics para poder monitorear toda la infraestructura que ya está conectada en la nube y on-premise con el uso de Arc.
        2. Application Insights nos puede servir para saber que está ocurriendo exactamente en la aplicación sin necesariamente saber todo sobre la infraestructura. 
        3. Azure Monitor con el uso de alertas pre configuradas para la gestión de metricas sobre recursos y tomar decisiones.

4. **¿Cómo montarías un plan de recuperación ante desastres?**
   - Incluye RPO (Recovery Point Objective) y RTO (Recovery Time Objective)
        * Implementar Azure Backup y Site Recovery. Gestión de replicación automatica entre regiones. Con esto se puede gestionar un RPO y RPO adecuados en terminos de costos y tiempo.

5. **¿Qué impacto tiene esta solución en la gestión de identidades y accesos?**
   - ¿Cómo se sincroniza AD con Azure Entra ID?
        * Lo ideal: aplicar Azure Policy Globalmente. Dado que se tiene Azure Arc, nos facilita esta operación. Gestion de seguridad Nativa de Azure sin codigo en las apps serverless. Y gestión de 





## 📊 Habilidades que Refuerza

✓ Selección del modelo de computación adecuado  
✓ Diseño de arquitecturas evolutivas y escalables  
✓ Evaluación de trade-offs entre costo y rendimiento  
✓ Gestión de observabilidad, seguridad y resiliencia en entornos híbridos  
✓ Pensamiento cloud-native vs. conservación de legacy  
✓ Planificación de disaster recovery realista

---

## Solución

