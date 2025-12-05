# Ejercicio 3 — Arquitectura Cloud-Native de Alta Escala

## 🎯 Objetivo

Decidir cuándo usar AKS, ACA, Batch, Redis y Cosmos DB correctamente.

---

## 📌 Contexto

Plataforma de procesamiento de imágenes con IA:

- Subida de ficheros
- Procesamiento en background con GPU
- Picos impredecibles
- Resultados disponibles por API
- Requisitos de latencia bajos

---

## 🧩 Restricciones

- Procesos que duran minutos
- Coste por uso
- Escalabilidad extrema
- Tolerancia a fallos

---

## 🏗️ Arquitectura Esperada

- Azure Kubernetes Service o Azure Container Apps
- Azure Batch
- Azure Data Lake Storage
- Azure Cosmos DB
- Azure Cache for Redis

---

## 🔄 Evolución Obligatoria

- Soportar eventos en tiempo real
- Usuarios globales
- SLA > 99.95%
- Auditorías de acceso a datos

---

## ❓ Preguntas de Discusión

1. **¿Kubernetes o ACA?**
   - ¿Qué criterios determinan cada elección?
   - ¿Cuál es más económico para picos impredecibles?
        * Es mucho más economico la gestión de Coste por uso con picos impredecibles. Con el Escalado a cero de ACA se tiene no solo el el bajo coste los dias de bajo trafico pero con ACA se puede tener una forma más eficiente de hacer autoscaling por ejemplo. 

2. **¿Batch o jobs en AKS?**
   - ¿Cómo manejaría cada opción los procesos que duran minutos?
   - ¿Cuál se adapta mejor a spot VMs?
        * Considerando el uso de ACA se puede implementar Azure Batch. De esta manera para peticiones de bajo costo y baja latencia está el computo normal manejado por ACA cuando ya se requiera el computo pesado, Azure Batch. Así se puede manejar una Escalabilidad Alta y se sigue manejando un coste por uso que habiamos planteado anteriormente. 

3. **¿Cosmos DB o SQL?**
   - ¿Qué tipo de datos necesita la plataforma?
   - ¿Cuáles son las ventajas de distribución global?
        * Para el tipo de datos que son Imagenes es más adecuado el uso de almacenamiento NoSQL. Es un tipo de archivo no estructurado que se puede guardar junto con información como datos del usuario, información sobre los trabajos hechos por la IA como JSONs. Ventajas, SLA > 99.999%, Escalabilidad horizontal ilimitada, coste por uso (Requests)

4. **¿Dónde aplicar caché?**
   - ¿Qué datos merece la pena cachear?
   - ¿Cómo impacta en la latencia?
        * Dado que son trabajos de IA en procesamiento de imagenes se pueden guardar alternativas de datos que imacten de forma positiva la latencia. Por ejemplo, los resultados de cada usuario se pueden guardar para consulta rapida, tokens de sesión, metadatos de los modelos de IA utilizados.

5. **¿Cómo proteger secretos y accesos?**
   - ¿Qué servicios de Azure utilizar?
   - ¿Cómo implementar auditorías?
        * Todo lo que son Claves de Cifrado de Cosmos DB, Certificados SSL/TLS va gestionado con Azure Key Vault. Con el uso del principio de Secret Zero ACA no gestiona ni ve ningun Secreto. Se gestionan roles implementados por el sistema. 


