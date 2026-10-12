# CFix — Investigación 04: protocolo experimental inicial

**Fecha:** 11 de octubre de 2026  
**Estado:** protocolo propuesto; no ejecutado  
**Dependencias:** Investigación 03 — auditoría técnica de soluciones existentes; anexo de Tensor Archive Games.

## 1. Propósito

Diseñar un experimento pequeño, reproducible y refutable para averiguar cuánto almacenamiento físico puede ahorrarse en corpus de videojuegos mediante técnicas existentes, qué costes introduce cada método y si queda una limitación concreta que justifique investigar un mecanismo CFix.

Este protocolo no presupone que CFix tenga una arquitectura novedosa ni que vaya a superar a las herramientas existentes.

## 2. Pregunta experimental

Para corpus de videojuegos que contengan archivos repetidos entre instalaciones, versiones o mods, ¿qué ahorro neto de almacenamiento físico se obtiene con métodos existentes y cuál es el coste en tiempo de optimización, lectura, restauración, actualización, CPU y memoria?

Solo después de medir esas referencias se podrá formular una hipótesis de mejora específica para CFix.

## 3. Hipótesis exploratoria

**H-EXP-01 — ahorro neto bajo límites de coste.** Al menos una técnica evaluada puede reducir el espacio físico ocupado por al menos uno de los corpus seleccionados, manteniendo restauración exacta y un coste de rendimiento dentro de límites definidos antes de ejecutar las pruebas.

Es una hipótesis sobre técnicas de almacenamiento en general, no una afirmación de novedad de CFix.

### Criterios para debilitar o refutar la hipótesis

- El ahorro desaparece al contabilizar índices, metadatos, fragmentación, copias auxiliares y bytes realmente asignados.
- La restauración no reproduce exactamente los archivos originales.
- El método incumple los límites de rendimiento o recursos fijados antes de la prueba.
- Los resultados no pueden repetirse en las mismas condiciones.
- El ahorro solo aparece frente a una configuración de referencia débil o no comparable.

## 4. Corpus y alcance

Comenzar con dos o tres corpus pequeños que el investigador tenga derecho a utilizar. No redistribuir archivos protegidos ni subir contenido de juegos a servicios externos sin autorización.

Seleccionar, si es posible:

1. **Corpus A — instalación individual:** un juego representativo, para medir cuánto aporta la compresión cuando no hay otras instalaciones con las que compartir datos.
2. **Corpus B — instalaciones relacionadas:** dos versiones o una instalación y un conjunto de mods relacionados, para medir la redundancia entre versiones.
3. **Corpus C — muchos archivos pequeños:** un juego o conjunto de datos con abundantes archivos pequeños, para observar el peso de los metadatos y el coste de acceso.

Evitar que el primer experimento dependa de un solo título o de datos especialmente favorables. Registrar las razones de selección y los sesgos posibles.

### Manifiesto del corpus

Guardar un manifiesto que incluya:

- Identificador del corpus y fecha de captura.
- Juego, versión, mods y procedencia de los archivos.
- Sistema operativo y sistema de archivos de origen.
- Número de archivos, tamaño lógico total y distribución de tamaños.
- Hash criptográfico por archivo y, si resulta útil, hash del manifiesto.
- Tipos de datos y presencia de archivos ya comprimidos o cifrados.
- Relaciones conocidas entre instalaciones.
- Exclusiones, archivos inaccesibles y cualquier transformación aplicada.

Congelar el corpus durante una ronda de comparación. Si cambia, crear una nueva versión del manifiesto y repetir los métodos comparables.

## 5. Métodos de referencia

| ID | Método | Objetivo |
|---|---|---|
| B0 | Archivos originales sin optimizar | Medir tamaño y rendimiento de referencia |
| B1 | Compresión nativa pertinente al entorno | Medir una opción realista de sistema |
| B2 | Zstandard, con versión y parámetros publicados | Establecer una referencia de compresión configurable |
| B3 | Deduplicación por archivo | Medir el beneficio de compartir archivos idénticos |
| B4 | Deduplicación por fragmentos | Medir redundancia parcial entre archivos |
| B5 | Flummox, si es reproducible en el entorno | Evaluar una implementación que combina compresión y deduplicación |
| B6 | Tensor Archive Games, si está disponible y puede verificarse | Contrastar las afirmaciones publicadas con el mismo corpus |
| CFix | Prototipo propio, solo tras definir un mecanismo concreto | Evaluar una hipótesis diferenciada frente a los mejores baselines |

No todos los métodos estarán necesariamente disponibles en el mismo sistema operativo o sistema de archivos. Documentar incompatibilidades y no presentar resultados de corpus diferentes como si fueran una comparación directa.

Para cada herramienta, registrar URL, versión o commit, comandos, parámetros, backend, dependencias y limitaciones conocidas. Si una herramienta no puede ejecutarse, marcarla como no evaluada y explicar por qué.

## 6. Métricas

### 6.1 Almacenamiento

- Tamaño lógico original en bytes.
- Tamaño del almacén optimizado.
- Bytes físicos asignados en el sistema de archivos.
- Índices, tablas, manifiestos y metadatos.
- Copias temporales o auxiliares necesarias durante la operación.
- Espacio de recuperación, redundancia o réplicas, si forman parte del diseño.
- Número de archivos, objetos o fragmentos.

Informar por separado el tamaño lógico y el espacio físico. No asumir que el tamaño de los archivos equivale a los bloques realmente asignados.

Fórmula principal, usando el mismo límite de sistema para numerador y denominador:

```
Ahorro neto = 1 - (bytes físicos del método / bytes físicos de la referencia)
```

Cada valor físico debe incluir los metadatos y auxiliares atribuibles al método dentro del alcance declarado. Expresar el resultado como porcentaje y conservar también los bytes absolutos.

### 6.2 Rendimiento y coste

- Tiempo de preparación u optimización.
- Tiempo de restauración completa.
- Rendimiento de lectura secuencial.
- Latencia y rendimiento de lecturas aleatorias.
- Tiempo y coste de actualizar un archivo pequeño y un conjunto grande.
- CPU, RAM y almacenamiento temporal máximo.
- Errores, fallos de integridad y capacidad de recuperación tras interrupción.

### 6.3 Equivalencia e integridad

Restaurar cada corpus a un directorio limpio y comparar hashes de todos los archivos con el manifiesto original. Una prueba parcial no equivale a verificar el corpus completo.

## 7. Procedimiento

1. Registrar el hardware, sistema operativo, sistema de archivos, tamaño de clúster, espacio libre y versiones de las herramientas.
2. Crear y verificar el manifiesto del corpus.
3. Medir B0 y conservar los registros.
4. Ejecutar cada método por separado sobre una copia idéntica del corpus.
5. Registrar comandos, configuración, salida estándar, errores y tiempos.
6. Medir bytes asignados y costes auxiliares una vez finalizada la operación, siguiendo el mismo criterio para todos los métodos.
7. Restaurar los datos en un destino limpio y comparar todos los hashes.
8. Ejecutar las pruebas de lectura y actualización con un guion fijo.
9. Repetir las mediciones de tiempo al menos tres veces, siempre que sea viable; reportar mediana y dispersión, no solo el mejor resultado.
10. Separar pruebas con caché fría y caliente cuando se puedan controlar y medir de manera fiable.
11. Evitar ejecutar dos métodos simultáneamente; registrar tareas de mantenimiento del sistema que puedan contaminar las medidas.
12. Publicar los resultados negativos y los casos en que una herramienta no pudo evaluarse.

No borrar los originales hasta completar y verificar la restauración.

## 8. Umbrales antes de medir

Antes de observar los resultados, definir umbrales prácticos para:

- Ahorro neto mínimo que justificaría complejidad adicional.
- Degradación máxima aceptable de lectura y restauración.
- Tiempo máximo de optimización y actualización.
- Límites de CPU, RAM y espacio temporal.
- Garantías de integridad y recuperación exigidas.

Los umbrales deben reflejar el uso objetivo y quedar registrados con fecha. No ajustarlos después de ver los resultados para favorecer una solución. Si aún no hay información suficiente para escogerlos, hacer una ronda piloto y marcarla explícitamente como calibración, no como prueba confirmatoria.

## 9. Tabla de resultados

Completar solo después de ejecutar los experimentos; no rellenar con estimaciones presentadas como medidas.

| Método | Corpus/manifiesto | Bytes lógicos | Bytes físicos asignados | Ahorro neto | Optimización | Restauración | Lectura aleatoria | CPU/RAM | Hashes correctos |
|---|---|---:|---:|---:|---:|---:|---:|---|---|
| B0 | | | | | | | | | |
| B1 | | | | | | | | | |
| B2 | | | | | | | | | |
| B3 | | | | | | | | | |
| B4 | | | | | | | | | |
| B5 | | | | | | | | | |
| B6 | | | | | | | | | |
| CFix | | | | | | | | | |

## 10. Riesgos y controles

- **Corpus no representativo:** usar más de un patrón de datos y explicar el sesgo de selección.
- **Datos ya comprimidos o cifrados:** medirlos por separado; no prometer compresión adicional sustancial.
- **Denominadores diferentes:** publicar bytes originales, físicos y la fórmula usada.
- **Metadatos omitidos:** contar todo el estado requerido para operar y restaurar.
- **Caché y procesos de fondo:** controlar condiciones o registrar las limitaciones.
- **Configuración de referencia débil:** usar parámetros documentados y razonablemente competitivos.
- **Confundir ahorro local con impacto global:** no extrapolar el porcentaje de un corpus a la demanda mundial de HDD/SSD sin modelar adopción, crecimiento, réplicas y efectos rebote.
- **Confundir simulación con evidencia física:** una simulación puede validar un modelo bajo sus supuestos, pero no demuestra por sí sola ahorro en hardware real.

## 11. Puerta de decisión para un prototipo CFix

No comenzar un prototipo propio hasta poder responder, con referencias:

1. ¿Cuál es el mejor baseline medido para la carga de trabajo elegida?
2. ¿Qué redundancia o coste deja sin resolver?
3. ¿Qué mecanismo concreto propone CFix y en qué difiere de técnicas existentes?
4. ¿Qué predicción cuantitativa podría refutar la propuesta?
5. ¿Qué coste adicional introduce en lecturas, escrituras, CPU, RAM, resiliencia o complejidad?
6. ¿Cómo se reproducirá la comparación y se verificará la integridad?

Si los métodos existentes resuelven suficientemente el problema, registrar ese resultado y cambiar la pregunta de investigación en lugar de construir una alternativa sin ventaja demostrable.

## 12. Referencias iniciales

- CFix, marco de investigación: https://github.com/dieee-93/CFix/blob/main/README.md
- Auditoría técnica de soluciones existentes: https://github.com/dieee-93/CFix/blob/main/research/Investigaci%C3%B3n%2003%20auditor%C3%ADa%20t%C3%A9cnica%20de%20soluciones%20existentes.md
- Tensor Archive Games, benchmarks: https://tensorarchive.games/benchmarks/
- Flummox: https://github.com/bybrooklyn/flummox
- Flummox, benchmark LZX: https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-18-lzx.md
- Microsoft, descripción de Data Deduplication: https://learn.microsoft.com/en-us/windows-server/storage/data-deduplication/understand
- FastCDC, USENIX ATC 2016: https://www.usenix.org/conference/atc16/technical-sessions/presentation/xia

## 13. Estado

**No ejecutado.** Este documento define cómo realizar las primeras pruebas; no contiene resultados experimentales propios ni evidencia de que CFix mejore las herramientas existentes.
