# CFix — Investigación 03: auditoría técnica de soluciones existentes

**Fecha:** 11 de octubre de 2026  
**Estado:** auditoría documental preliminar  
**Objetivo:** identificar capacidades existentes, limitaciones documentadas y posibles oportunidades de investigación relacionadas con la reducción del almacenamiento físico.

## 1. Resumen ejecutivo

Se analizaron tres referencias relevantes:

- **LincleLINK:** deduplicación de archivos idénticos entre instalaciones y versiones, con almacenamiento compartido mediante enlaces duros.
- **Flummox:** compresión sin pérdida, deduplicación por fragmentos, almacenamiento compartido y restauración verificada.
- **Windows Server Data Deduplication:** implementación de producción que identifica fragmentos redundantes, mantiene una copia de los datos compartidos y permite acceso transparente a los archivos optimizados.

El análisis también examinó un caso documentado por Arrowhead Game Studios sobre la duplicación deliberada de datos para reducir tiempos de carga en discos mecánicos.

**Conclusión provisional:** la compresión, la deduplicación por archivos, la deduplicación por fragmentos y la combinación de compresión con almacenamiento compartido son técnicas con antecedentes sustanciales. CFix no debe presentarlas como novedosas por sí mismas.

Una posible línea de investigación es estudiar cómo reducir la capacidad física ocupada sin degradar excesivamente la latencia, el rendimiento de lectura, la recuperación o la integridad de los datos.

Todavía no se ha demostrado que exista una solución novedosa ni una ventaja técnica de CFix frente a los sistemas analizados.

## 2. LincleLINK

Repositorio: https://github.com/Radioo/LincleLINK

### 2.1. Mecanismo

LincleLINK mantiene una biblioteca de datos compartidos. Cuando varias instalaciones contienen archivos idénticos, puede conservar una sola copia y materializar las distintas instalaciones mediante enlaces duros.

El sistema también contempla la verificación de archivos para reutilizar datos existentes durante descargas torrent.

### 2.2. Ventajas

- Evita almacenar varias copias físicas de archivos idénticos.
- Conserva nombres y estructuras de directorios al materializar instalaciones.
- Puede ser útil para bibliotecas con múltiples versiones o instalaciones relacionadas.
- Evita volver a descargar archivos que ya existen y han sido verificados.

### 2.3. Limitaciones

- Los archivos diferentes no se comparten automáticamente aunque tengan contenido parcialmente coincidente.
- Los enlaces duros hacen que varias rutas compartan el mismo objeto; una modificación directa puede afectar a todas ellas.
- El almacenamiento y los destinos deben respetar las restricciones del sistema de archivos y del mismo volumen.
- No se localizó en esta revisión un benchmark público normalizado que permita atribuirle un porcentaje general de ahorro para una biblioteca representativa.

### 2.4. Implicación para CFix

Compartir archivos idénticos entre versiones es una capacidad existente. Una propuesta que se limite a identificar archivos duplicados y sustituirlos por referencias no constituye, por sí sola, una contribución diferenciada.

Fuente principal: https://github.com/Radioo/LincleLINK

## 3. Flummox

Repositorio: https://github.com/bybrooklyn/flummox

### 3.1. Mecanismo

Flummox combina varias estrategias:

1. Compresión sin pérdida.
2. Fragmentación de contenido para identificar datos compartidos.
3. Deduplicación de fragmentos idénticos.
4. Almacenamiento compartido entre juegos relacionados.
5. Verificación y restauración de los datos originales.

Su documentación contempla diferentes mecanismos según el sistema operativo y el sistema de archivos.

### 3.2. Benchmark de instalaciones completas

El informe publicado el 18 de septiembre de 2026 compara cuatro instalaciones de juegos con un referente de compresión LZX basado en bloques de 32 KiB.

| Métrica | Flummox | Referencia LZX |
|---|---:|---:|
| Tamaño lógico original | 1.811.546.877 bytes | 1.811.546.877 bytes |
| Bytes asignados | 1.131.470.848 | 1.186.734.080 |
| Porcentaje retenido | 62,46 % | 65,51 % |

En ese conjunto, Flummox utilizó 55.263.232 bytes menos que la referencia.

Estas cifras describen un experimento específico. No representan una garantía para cualquier juego, sistema de archivos o dispositivo.

La referencia LZX se construyó con `wimlib`; la documentación indica que una comparación directa con el comportamiento nativo de Windows requiere una medición adicional.

Fuente:
https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-18-lzx.md

### 3.3. Compartición entre juegos

En dos pares de juegos relacionados, el proyecto reporta un ahorro adicional de 227.545.088 bytes al compartir fragmentos entre almacenes, equivalente al 14,76 % de la asignación de los almacenes independientes ya comprimidos.

Este porcentaje no debe interpretarse como una reducción del 14,76 % respecto de los datos originales.

Fuente:
https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-18-pools.json

### 3.4. Agrupación de archivos pequeños

El informe de septiembre de 2026 compara dos versiones del formato de almacenamiento sobre subconjuntos de archivos pequeños.

- Celeste: reducción del 40,2 % del tamaño serializado respecto del formato anterior.
- RetroArch: reducción del 13,8 % respecto del formato anterior.

Los tiempos de lectura aleatoria aumentaron ligeramente en los experimentos publicados.

Estos resultados se refieren a dos corpus concretos y a una comparación entre versiones de la propia implementación. No demuestran una ventaja universal ni permiten extrapolar el porcentaje a juegos completos.

Fuente:
https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-24-small-files.md

### 3.5. Limitaciones relevantes

- Los datos previamente comprimidos pueden dejar poco margen para una compresión adicional.
- Los fragmentos grandes pueden mejorar la compresión, pero aumentar el trabajo de descompresión durante lecturas aleatorias.
- Los metadatos y la estructura del almacén influyen en el tamaño final.
- El acceso y la restauración dependen de los mecanismos implementados.
- La compatibilidad con sistemas de archivos y plataformas varía.

### 3.6. Implicación para CFix

Una arquitectura que combine compresión, deduplicación por fragmentos, compartición entre juegos y restauración verificada ya tiene un antecedente público relevante.

Para justificar una propuesta diferente habrá que identificar una carga de trabajo, una limitación medible o un mecanismo concreto que no quede resuelto adecuadamente por los sistemas existentes.

Fuentes:
- https://github.com/bybrooklyn/flummox
- https://github.com/bybrooklyn/flummox/blob/main/docs/how-it-works.md

## 4. Windows Server Data Deduplication

Documentación oficial:
https://learn.microsoft.com/en-us/windows-server/storage/data-deduplication/understand

### 4.1. Capacidades existentes

Microsoft documenta una implementación que:

- Divide archivos en fragmentos de tamaño variable.
- Identifica fragmentos idénticos.
- Conserva una copia de cada fragmento único.
- Puede comprimir los datos almacenados.
- Sustituye los datos redundantes por referencias.
- Permite el acceso transparente a los archivos optimizados.

El sistema también contempla tareas de mantenimiento para recuperar fragmentos que han dejado de utilizarse.

### 4.2. Implicación para CFix

La deduplicación por fragmentos con acceso transparente no es una capacidad inédita. Tampoco lo es combinar deduplicación y compresión.

La investigación debe centrarse en un problema específico, por ejemplo, un patrón de redundancia que las soluciones existentes no aprovechen eficientemente, siempre que los experimentos demuestren esa limitación.

Las restricciones de compatibilidad y operación deben comprobarse para cada entorno. No se debe asumir que una solución diseñada para Windows Server es adecuada sin modificaciones para una biblioteca de juegos de escritorio.

## 5. Caso documentado: HELLDIVERS 2

Fuente oficial:
https://www.arrowheadgamestudios.com/2025/10/helldivers-2-tech-blog-1-install-size/

En octubre de 2025, Arrowhead Game Studios explicó que la versión para PC de HELLDIVERS 2 contenía datos duplicados para reducir los tiempos de búsqueda de recursos en discos mecánicos.

La duplicación permitía colocar copias de recursos cerca de otros datos necesarios durante la carga. El estudio explicó que eliminar toda esa duplicación podía multiplicar aproximadamente por diez los tiempos de carga en determinados escenarios con HDD.

La cifra corresponde al caso descrito por el desarrollador y no debe generalizarse a todos los videojuegos.

El artículo también señala compromisos relacionados con:

- Tiempos de carga.
- Uso de memoria RAM.
- Distribución física de los datos.
- Compresión y coste de descompresión.
- Complejidad de modificar el motor para ofrecer contenido opcional.

### Implicación para CFix

Reducir la cantidad de datos persistidos puede eliminar redundancia que se introdujo deliberadamente para mejorar el rendimiento.

Por lo tanto, el objetivo no debe ser minimizar el tamaño a cualquier coste. Hay que evaluar conjuntamente:

- Espacio físico ocupado.
- Latencia de lectura.
- Tiempo de carga.
- Consumo de CPU y memoria.
- Integridad y capacidad de recuperación.

Una posible hipótesis consiste en diferenciar redundancia innecesaria de redundancia que cumple una función de rendimiento. Esta hipótesis todavía requiere experimentación.

## 6. Matriz comparativa

| Capacidad | LincleLINK | Flummox | Windows Server |
|---|---|---|---|
| Archivos idénticos | Sí | Sí, mediante fragmentos y almacenes | Sí, mediante fragmentos |
| Coincidencias parciales | No como mecanismo principal | Sí | Sí |
| Compresión sin pérdida | No es su función principal | Sí | Sí |
| Compartición entre instalaciones o conjuntos | Sí | Sí | Sí, según la carga y configuración |
| Orientación específica a juegos | Sí | Sí | No |
| Benchmarks públicos revisados | No localizados | Sí, para conjuntos concretos | La medición depende de la carga |
| Restauración o acceso transparente | Mediante enlaces duros | Mediante el formato y los mecanismos de acceso | Integrado con el sistema de archivos |

Esta tabla compara funciones documentadas, no resultados obtenidos bajo un benchmark común.

## 7. Oportunidades de investigación para CFix

Las siguientes líneas son hipótesis, no afirmaciones de novedad.

### H1. Optimización conjunta de espacio y rendimiento

Investigar si es posible reducir datos redundantes sin aumentar excesivamente los tiempos de lectura en cargas sensibles a la latencia.

### H2. Selección adaptativa de representación

Estudiar cuándo conviene conservar un archivo completo, compartir fragmentos, comprimirlo o almacenar diferencias, teniendo en cuenta el coste total de acceso y reconstrucción.

### H3. Gestión de redundancia con función de rendimiento

Determinar si existe una forma de reducir el espacio ocupado por datos duplicados sin perder los beneficios de localidad que motivaron su duplicación.

### H4. Reducción neta en bibliotecas reales

Medir el ahorro físico después de contabilizar metadatos, fragmentación, estructuras auxiliares y requisitos de recuperación.

Ninguna de estas líneas puede considerarse novedosa hasta completar la búsqueda de antecedentes y la comparación experimental correspondiente.

## 8. Protocolo experimental

Para cada hipótesis se recomienda:

1. Seleccionar conjuntos de datos representativos y documentar su procedencia.
2. Registrar el tamaño lógico original y la asignación física de referencia.
3. Ejecutar herramientas estándar bien configuradas.
4. Ejecutar la estrategia candidata sobre los mismos datos.
5. Contabilizar payload, metadatos, referencias y estructuras auxiliares.
6. Verificar la reconstrucción byte a byte.
7. Medir escritura, lectura aleatoria, lectura secuencial y reconstrucción.
8. Repetir los experimentos bajo condiciones comparables.
9. Registrar configuración, versiones, resultados brutos y fallos.
10. Documentar también los casos en que la propuesta pierde frente a la referencia.

La métrica de ahorro físico neto será:

**Ahorro neto = 1 − (bytes físicos totales de la propuesta / bytes físicos totales de referencia)**

Los bytes físicos totales deben medirse con una definición consistente y deben incluir todos los componentes necesarios para mantener el mismo nivel de integridad y disponibilidad.

Una estimación basada en el tamaño comprimido de los archivos no equivale a una medición directa de la capacidad física liberada.

## 9. Conclusiones

1. Compartir archivos idénticos entre versiones es una capacidad existente.
2. La deduplicación por fragmentos ya permite compartir partes de archivos diferentes.
3. La combinación de compresión, deduplicación y restauración verificada tiene antecedentes públicos.
4. Existen benchmarks que demuestran que el tamaño final depende del corpus, la técnica y los costes de almacenamiento auxiliares.
5. La redundancia puede cumplir una función de rendimiento y no debe eliminarse sin medir sus consecuencias.
6. Todavía no se ha demostrado una ventaja técnica específica de CFix ni una oportunidad novedosa.

La conclusión más importante es metodológica: **CFix necesita identificar una limitación concreta y demostrar una mejora neta frente a herramientas existentes, no simplemente combinar técnicas conocidas.**

## 10. Próxima etapa

La siguiente investigación debería centrarse en cuantificar el compromiso entre capacidad física y rendimiento de acceso.

Se propone seleccionar un conjunto de datos representativo, comparar compresión y deduplicación con una referencia convencional, y medir el impacto de cada técnica sobre las lecturas y la reconstrucción.

El objetivo será determinar si existe un escenario en el que la reducción de capacidad física pueda mejorarse sin degradar de forma inaceptable el rendimiento.

## Referencias principales

1. LincleLINK, repositorio y documentación: https://github.com/Radioo/LincleLINK
2. Flummox, repositorio: https://github.com/bybrooklyn/flummox
3. Flummox, benchmark LZX: https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-18-lzx.md
4. Flummox, benchmark de archivos pequeños: https://github.com/bybrooklyn/flummox/blob/main/docs/benchmarks/2026-09-24-small-files.md
5. Microsoft, Understanding Data Deduplication: https://learn.microsoft.com/en-us/windows-server/storage/data-deduplication/understand
6. Arrowhead Game Studios, HELLDIVERS 2: Tech Blog #1 — Install Size: https://www.arrowheadgamestudios.com/2025/10/helldivers-2-tech-blog-1-install-size/

**Nota de alcance:** este documento resume una auditoría documental preliminar. No constituye una revisión exhaustiva de patentes, una auditoría independiente del código, una reproducción de los benchmarks ni un dictamen de patentabilidad.
