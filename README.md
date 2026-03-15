# SBC_LaptopPowerFlowchart

# Sistema Experto en CLIPS: Diagnóstico de Fallas de Alimentación en Laptops

## Descripción del Proyecto
El código es un sistema experto escrito en CLIPS que guía al usuario paso a paso para diagnosticar por qué una laptop no enciende. Todo funciona a partir de un único hecho llamado nodo, que indica en qué parte del árbol de decisión se encuentra el sistema. Cada regla corresponde a una pregunta del diagrama: cuando se activa, elimina el nodo actual, hace una pregunta y, según la respuesta, avanza al siguiente nodo o da un diagnóstico final. Esto permite que el sistema siga de manera ordenada el flujo del diagrama, incluyendo tanto las rutas originales como las tres extensiones modernas (Hard Reset, códigos LED y diagnóstico USB‑C). En conjunto, el código reproduce el razonamiento de un técnico y permite llegar a diagnósticos claros de forma interactiva.
El motor de inferencia fue programado en **CLIPS (C Language Integrated Production System)**, tomando como base heurística el diagrama de decisión *Laptop Power Flowchart* del portal *If it Jams* (2011). El modelo original fue analizado, traducido a reglas de producción y expandido con conocimientos técnicos contemporáneos para cubrir las arquitecturas de hardware modernas.

## Adiciones Implementadas
El sistema base fue robustecido con tres ramificaciones diagnósticas fundamentales para evitar falsos positivos y reparaciones invasivas innecesarias:
* **Drenado de Energía (Hard Reset):** Detección de bloqueos lógicos en el Controlador Embebido (EC) por acumulación de estática.
* **Diagnóstico LED (POST):** Interpretación de códigos de error mediante patrones de parpadeo, adaptándose a la ausencia de bocinas internas en equipos ultradelgados.
* **Protocolo USB-C (Power Delivery):** Evaluación de fallas de negociación digital entre el chip del cargador y el puerto, reemplazando la medición de voltaje bruto tradicional.

## Arquitectura Técnica
El sistema opera bajo el paradigma de **encadenamiento hacia adelante (forward chaining)** y fue modelado lógicamente como una máquina de estados finitos para garantizar un flujo controlado.

* **Representación del Conocimiento:** Se utiliza un único `deftemplate` llamado `nodo` que actúa como rastreador del estado actual del diagnóstico en la Memoria de Trabajo.
* **Lógica de Transición:** El control de flujo se maneja mediante constructores `defrule`. 
* **Manejo de Memoria:** Para evitar ciclos infinitos y disparo múltiple de reglas, cada transición de estado implementa la función `retract` para eliminar el hecho condicional previo, seguido de un `assert` para inyectar el nuevo nodo o, en su defecto, emitir el diagnóstico final.

## Requisitos (Versión)
* CLIPS v6.31
