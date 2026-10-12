CFix — Investigación sobre reducción estructural de la demanda de almacenamiento
Propósito
Investigar si es posible reducir de forma sustancial la cantidad de almacenamiento físico necesaria para ofrecer servicios digitales equivalentes, evitando persistir información redundante, reutilizando estructuras compartidas y conservando solo la información necesaria para reconstruir datos o satisfacer tareas definidas.
El objetivo no es asumir que existe una solución novedosa, sino identificar límites reales, revisar el estado del arte y formular hipótesis que puedan ponerse a prueba.
Pregunta central
¿Es posible conservar únicamente la información independiente necesaria para reconstruir datos o satisfacer tareas explícitas, reutilizando estructuras de forma segura y evitando persistir resultados derivables, sin trasladar costes inaceptables a cómputo, memoria, red, energía o fiabilidad?

Principios de investigación
1. Evidencia antes que afirmaciones. Las afirmaciones técnicas importantes deben enlazarse con fuentes verificables o experimentos reproducibles.
2. Separación epistemológica. Distinguir tecnologías existentes, problemas abiertos, hipótesis propias y resultados experimentales.
3. Novedad no presupuesta. Una combinación prometedora no se considerará novedosa hasta compararla con literatura, implementaciones y patentes pertinentes.
4. Equivalencia explícita. Diferenciar reconstrucción exacta sin pérdida, aproximación semántica y conservación orientada a tareas.
5. Ahorro neto. Contabilizar metadatos, índices, réplicas, fragmentación y copias de seguridad; no medir solo el tamaño del archivo principal.
6. Reproducibilidad. Registrar conjuntos de datos, versiones, parámetros, entorno, procedimientos y resultados.
7. Privacidad y resiliencia desde el diseño. La reutilización entre usuarios o sistemas no presupone que sus datos puedan compartirse libremente.
8. Refutabilidad. Cada hipótesis debe indicar qué resultado la debilitaría o descartaría.
Cinco fronteras iniciales
Estas fronteras organizan la exploración; no implican que sean novedosas ni exhaustivas.
1. Información irreducible y estructura compartida: detectar estructura común más allá de duplicados exactos, respetando los límites de la compresión sin pérdida.
2. Reutilización entre sistemas independientes: compartir representaciones o bloques de forma segura, considerando privacidad, propiedad, disponibilidad, aislamiento y coordinación.
3. Regeneración frente a persistencia: determinar cuándo conviene almacenar una descripción, receta, parámetros o procedencia para regenerar un resultado, en vez de conservarlo completo.
4. Representación relacional: estudiar grafos, componentes y relaciones reutilizables frente al almacenamiento de objetos aislados, incluyendo el coste de metadatos.
5. Almacenamiento orientado a tareas: investigar cuándo basta conservar información necesaria para tareas explícitas, sin confundir esto con la posibilidad de reconstruir los datos originales.
Criterios de evaluación
Toda propuesta importante debe evaluarse, cuando corresponda, por:
- Reducción neta de bytes físicos: después de metadatos, índices, réplicas y redundancia de resiliencia.
- Equivalencia del servicio: exactitud de reconstrucción o calidad medible para la tarea acordada.
- Rendimiento: latencia, throughput, escrituras, lecturas y tiempo de recuperación.
- Coste desplazado: CPU, RAM, red, energía, hardware auxiliar y complejidad operativa.
- Escalabilidad: comportamiento al crecer los datos, los usuarios y los sistemas.
- Seguridad y fiabilidad: aislamiento, integridad, disponibilidad, recuperación ante fallos y obligaciones de retención.
- Generalización: resultados sobre datos de prueba no usados para diseñar o ajustar el método.
- Impacto en capacidad física: escenarios explícitos de adopción, replicación, crecimiento y posibles efectos rebote.
Una mejora en un conjunto de datos no implica automáticamente una reducción proporcional de la demanda mundial de HDD o SSD.
Estructura propuesta
CFix/
├── README.md
├── research/
│   ├── literature-review.md
│   ├── existing-systems.md
│   ├── patents.md
│   └── bibliography.md
├── open-problems/
│   └── problem-register.md
├── hypotheses/
│   └── hypothesis-register.md
├── experiments/
│   ├── methodology.md
│   └── results/
└── decision-log.md
Crear archivos solo cuando haya contenido real que registrar; no hace falta poblar toda la estructura de inmediato.
Registro de fuentes
Para cada fuente relevante, registrar como mínimo:
- Identificador estable (DOI, URL oficial, número de patente o versión).
- Título, autores o entidad responsable y fecha.
- Tipo de fuente y fecha de consulta.
- Afirmación o resultado que respalda.
- Método, conjunto de datos y condiciones evaluadas.
- Limitaciones y posibles conflictos de interés.
- Relación con otras fuentes y preguntas que deja abiertas.
Priorizar artículos originales, documentación oficial, especificaciones, patentes y evaluaciones reproducibles. Las reseñas sirven para descubrir trabajos, pero no sustituyen la verificación de las fuentes primarias.
Registro de hipótesis
Cada hipótesis debe incluir:
1. ID y estado: propuesta, en revisión, en experimento, apoyada, refutada o pausada.
2. Problema concreto y evidencia de que importa.
3. Trabajos previos más cercanos.
4. Mecanismo propuesto y diferencias potenciales respecto de técnicas existentes.
5. Predicciones cuantificables.
6. Baseline competitivo y protocolo experimental.
7. Métricas, costes y riesgos.
8. Resultado que la refutaría.
9. Próximo paso y decisión tomada.
Método de trabajo
1. Mapear el estado del arte en las cinco fronteras.
2. Identificar sistemas implementados y patentes relevantes.
3. Construir una matriz de capacidades, limitaciones y costes.
4. Identificar una limitación concreta que no esté resuelta satisfactoriamente.
5. Formular hipótesis estrechas y falsables, sin declarar novedad anticipadamente.
6. Diseñar experimentos comparados con baselines competitivos.
7. Reproducir, documentar y someter a crítica los resultados.
8. Estimar el impacto físico solo después de medir el ahorro neto y explicitar los supuestos de adopción.
Registro de decisiones
Las decisiones relevantes se documentarán en decision-log.md, con fecha, contexto, evidencia considerada, alternativas, decisión y condiciones para revisarla. También se registrarán las hipótesis descartadas: los resultados negativos evitan repetir trabajo y delimitan el espacio de búsqueda.
Estado actual
Fase: definición del marco y revisión sistemática del estado del arte.
Todavía no se ha establecido: una arquitectura novedosa, una mejora demostrada, una patente potencialmente válida ni un impacto cuantificado sobre la demanda global de almacenamiento.
Nota sobre los límites teóricos
La compresión sin pérdida no puede reducir estrictamente todos los archivos arbitrarios; algunos datos no contienen redundancia aprovechable por un método dado. Por ello, la investigación también examina cambios en qué se persiste, qué se comparte y qué debe poder reconstruirse, declarando siempre las garantías ofrecidas.