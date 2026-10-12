# Anexo de auditoría — Tensor Archive Games

**Fecha de consulta:** 11 de octubre de 2026  
**Estado:** referencia documental preliminar; resultados no reproducidos independientemente  
**Documento relacionado:** [Investigación 03 — auditoría técnica de soluciones existentes](./Investigaci%C3%B3n%2003%20auditor%C3%ADa%20t%C3%A9cnica%20de%20soluciones%20existentes.md)

## 1. Por qué se incorpora

Tensor Archive Games (TAG) publica benchmarks orientados a reducir el espacio ocupado por bibliotecas de videojuegos. Es una referencia directamente relacionada con el problema de CFix y debe incluirse en la comparación de soluciones existentes antes de diseñar una implementación propia.

Fuente primaria: https://tensorarchive.games/benchmarks/

## 2. Resultados publicados por el proveedor

La página consultada presenta, entre otros, estos resultados:

| Corpus o alcance indicado | Tamaño original publicado | Tamaño final publicado | Reducción publicada |
|---|---:|---:|---:|
| Assetto Corsa + mods | 61,83 GB | 15,00 GB | 75,74 % |
| Pavlov VR | 79,69 GB | 35,11 GB | 55,95 % |
| Assetto Corsa Competizione | 20,33 GB | 15,05 GB | 25,96 % |
| Paquetes de Baldur’s Gate 3 | 154,57 GB | 121,53 GB | 21,38 % |

La misma página presenta una comparación agregada de cinco alcances, con 347,58 GB originales:

| Método informado | Tamaño final publicado |
|---|---:|
| Microsoft LZX | 257,90 GB |
| Zstandard | 231,08 GB |
| Tensor Archive Games | 206,72 GB |

Según esas cifras, el tamaño final de TAG sería aproximadamente un 10,54 % menor que el de Zstandard. La página también afirma restauración exacta, con cero bytes distintos, y señala una medición de almacenamiento físico. La fecha de benchmark indicada por el proveedor es el 9 de agosto de 2026.

**Advertencia epistemológica:** todas las cifras anteriores son afirmaciones publicadas por el proveedor, no resultados obtenidos por CFix ni una reproducción independiente. No deben citarse como rendimiento general garantizado. Las cifras individuales y agregadas dependen de los corpus y las condiciones concretas.

## 3. Información que falta verificar para una comparación reproducible

Antes de aceptar la comparación como evidencia suficiente, hay que documentar o solicitar:

1. **Identidad del software:** versión, commit, dependencias, sistema operativo y comandos exactos.
2. **Corpus:** lista de archivos, versiones de juegos, mods incluidos, licencias o permisos de uso, tamaños lógicos y manifiesto de hashes.
3. **Equivalencia:** confirmación de que el método conserva los datos necesarios y puede reconstruir exactamente el corpus evaluado.
4. **Parámetros de los baselines:** versión y opciones de LZX, Zstandard y cualquier otra herramienta; justificar por qué los parámetros elegidos son comparables y no deliberadamente débiles.
5. **Medición física:** unidad usada, tamaño de clúster, bytes asignados, metadatos, índices, almacenamiento auxiliar, copias temporales y cualquier contenido compartido contabilizado.
6. **Rendimiento:** tiempo de empaquetado, restauración, lecturas secuenciales y aleatorias, actualizaciones, uso de CPU y memoria.
7. **Integridad:** procedimiento de restauración a un destino limpio y comparación de hashes de todos los archivos.
8. **Repetibilidad:** número de ejecuciones, variación entre ejecuciones, estado de caché y disponibilidad de scripts, logs y resultados en bruto.
9. **Alcance de producto:** separar las funciones disponibles hoy de las anunciadas o planificadas. La integración con Steam aparece descrita como planificada en la página consultada; no debe tratarse como capacidad ya demostrada sin confirmación adicional.

La ausencia de alguno de estos datos no demuestra que el resultado sea falso. Significa que terceros no pueden reproducirlo plenamente con la información disponible.

## 4. Plan de comparación para CFix

Si la herramienta está disponible y puede evaluarse legal y técnicamente, incluir TAG como un candidato adicional junto a:

- Corpus original sin optimizar.
- Compresión nativa pertinente al entorno.
- Zstandard con versión y parámetros documentados.
- Deduplicación por archivo.
- Deduplicación por fragmentos, cuando exista una implementación reproducible.
- Flummox, registrando commit, backend y configuración.
- CFix, solo después de definir un mecanismo concreto y un prototipo verificable.

Todas las herramientas deben procesar el mismo corpus congelado y superar el mismo control de integridad. Si una solución requiere un flujo de trabajo o una estructura de almacenamiento diferente, registrar esa diferencia en vez de ocultarla.

No comparar porcentajes calculados sobre denominadores distintos. Informar siempre tamaño original, bytes físicos asignados y fórmula empleada:

\[
\text{Ahorro neto}=1-\frac{\text{bytes físicos del método}}{\text{bytes físicos de la referencia}}
\]

El resultado debe incluir todos los metadatos y el almacenamiento auxiliar atribuibles al método, dentro de un límite de sistema declarado.

## 5. Implicaciones para la investigación

TAG refuerza la necesidad de evaluar herramientas existentes antes de construir otra solución. Si sus resultados se reproducen, podrían establecer un baseline competitivo fuerte. Si no se reproducen, la discrepancia debe investigarse y documentarse; por sí sola no demostraría mala fe ni invalidaría todo el enfoque.

El objetivo de CFix no es superar una cifra publicada de forma aislada. Debe identificar una limitación real, definir una carga de trabajo relevante, compararse contra alternativas competitivas y demostrar una ventaja neta sin sacrificar equivalencia, integridad o rendimiento más allá de umbrales predefinidos.

## 6. Fuente

- Tensor Archive Games, benchmarks: https://tensorarchive.games/benchmarks/ (consultado el 11 de octubre de 2026).

