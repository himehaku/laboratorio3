# Laboratorio de aplicación inteligente adaptativa

## Identificación

- Integrantes: Bárbara Oyarzo Alfaro
- Curso y sección: Ingeniería Web Avanzada
- URL del repositorio: https://github.com/himehaku/laboratorio3.git
- Fecha: 09-11-2026

## Objetivo y alcance

Complete un recomendador de recursos de aprendizaje que represente los intereses
de cada usuario y actualice su perfil al recibir la interacción **Me interesa**.
El núcleo se implementa en Python y se ejecuta por consola. Una interfaz Angular
puede incorporarse como extensión después de esta actividad.

El catálogo, los perfiles, las validaciones, la aplicación por consola y el registro
de evidencias se proporcionan. Complete únicamente los cuatro bloques señalados
en `motor.py` y documente su interpretación. No modifique los datos de las pruebas
ni la aplicación para obtener un resultado esperado.

La representación usa **solo la columna etiquetas**. Títulos y descripciones sirven
para presentar el catálogo. Todos los recursos continúan disponibles después de
una interacción; no se excluye el recurso valorado. Esto permite comparar el mismo
conjunto de 12 candidatos antes y después de la actualización del perfil.

## Preparación

Use Python 3.10 o posterior. Abra una terminal en esta carpeta.

```bash
python -m venv .venv
```

En Windows puede usar `py -m venv .venv`.

Activación en PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Activación en CMD:

```bat
.venv\Scripts\activate.bat
```

Activación en macOS o Linux:

```bash
source .venv/bin/activate
```

Instalación y comprobación:

```bash
python -m pip install -r requirements.txt
python -c "import sys; print(sys.executable)"
python app.py --comprobar
```

La comprobación funciona con los cuatro bloques sin completar. Debe informar
12 recursos, 4 usuarios y una matriz TF-IDF de 12 filas. Registre el comando de
activación que utilizó: .venv\Scripts\Activate.ps1. Al finalizar, registre las versiones instaladas:

```bash
python -m pip freeze > requirements.txt
```

## Funciones que debe completar

| Función | Entrada principal | Salida y requisito |
|---|---|---|
| `crear_perfil` | Texto de intereses y vectorizador ya ajustado | Vector NumPy de una dimensión obtenido con `transform`; no ajustar nuevamente el vocabulario. |
| `calcular_puntuaciones` | Vector del perfil y matriz de recursos | Un valor de similitud del coseno por recurso. |
| `recomendar` | Perfil y catálogo indexado | Hasta 3 resultados con puntuación positiva; orden descendente y desempate por `id` ascendente. |
| `actualizar_perfil` | Perfil anterior y vector del recurso valorado | Vector normalizado de la combinación `0.4 * perfil + 0.6 * recurso`. |

Use `construir_resultado` al construir cada recomendación. Esta función proporciona
`id`, `titulo`, `puntuacion` y una `explicacion` basada en términos compartidos.

Un perfil vacío o compuesto por palabras fuera del vocabulario produce un vector
nulo. En esos casos deben obtenerse puntuaciones cero y una lista de recomendaciones
vacía; la aplicación ya proporciona el mensaje informativo.

El parámetro 0.6 es una elección didáctica que da mayor peso a la interacción reciente.
No es un valor validado experimentalmente. Una similitud no es una probabilidad de
interés. El ejercicio no entrena un modelo supervisado ni demuestra comprensión
semántica de los recursos.

## Ejecución después de completar el motor

Modo interactivo:

```bash
python app.py
```

Seleccione un usuario. Puede consultar el catálogo, solicitar recomendaciones y
registrar **Me interesa** indicando el ID de cualquier recurso, aunque no aparezca
entre los tres primeros. Un ID desconocido muestra un aviso y no actualiza el perfil.
Los perfiles y sus historiales de interacciones se conservan por separado durante
la ejecución. La consola muestra el historial del usuario activo. Al reiniciar se
cargan nuevamente los intereses iniciales del JSON y los historiales quedan vacíos.

Demostración reproducible:

```bash
python app.py --demo
```

Este comando parte de los perfiles iniciales, ejecuta T1-T4 y genera:

- `evidencias/reporte_demo.json`: perfiles, historiales, recomendaciones y comprobaciones.
- `evidencias/puntuaciones_antes_despues.csv`: los mismos 12 recursos con sus
  puntuaciones anteriores, posteriores y diferencias, incluyendo ceros.

Que la aplicación muestre **Cumple** es una comprobación orientativa. Revise las
puntuaciones y explique qué evidencia respalda cada resultado.

## Pruebas funcionales y evidencias

| Caso | Entrada o acción | Resultado esperado | Resultado observado y evidencia | Estado |
|---|---|---|---|---|
| T1 | Recomendar para u1 y u2 sin interacciones | R02 primero para u1; R07 primero para u2; orden descendente, máximo 3 y puntuaciones positivas. | 	u1 recibió R02, R12 y R03. u2 recibió R07, R08 y R09. Las recomendaciones están ordenadas correctamente. | Cumple |
| T2 | u1 indica Me interesa sobre R04 y se recalcula | Cambia el perfil; R04 pasa al primer lugar; aumenta la puntuación de R05; se conservan los 12 candidatos. | Al valorar R04, cambió el perfil de u1. R04 pasó al primer lugar y R05 aumentó de 0.0000 a 0.4378. Se mantuvieron los 12 recursos. | Cumple |
| T3 | Consultar u2 después de la interacción de u1 | El perfil, el historial y las puntuaciones de u2 se mantienen. | El perfil, las recomendaciones y el historial de u2 no cambiaron después de la interacción de u1. | Cumple |
| T4 | Recomendar para u3 y u4 | Cero puntuaciones positivas, lista vacía y mensaje informativo para ambos. | u3 y u4 no recibieron recomendaciones porque sus intereses estaban vacíos o no eran reconocidos. Se mostró un mensaje informativo. | Cumple |


### Comparación de la adaptación

- Perfil de u1 antes de valorar R04: {"pandas": 0.753247, "datos": 0.465091, "python": 0.465091}.
- Perfil de u1 después de valorar R04: {"python": 0.637116, "api": 0.446269, "web": 0.446269, "pandas": 0.376477, "datos": 0.232455}
- Puntuación de R05 antes y después: aumentó de 0.0000 a 0.4378.
- Por qué ese cambio es evidencia de adaptación: porque al valorar R04, el sistema agregó nuevos intereses al perfil y cambió las recomendaciones. R05 aumentó su puntuación sin haber sido seleccionado.
- Problemas detectados y acciones realizadas: se completaron las cuatro funciones pendientes y se realizaron las pruebas. Todas funcionaron correctamente.


No sustituya estas observaciones por cifras inventadas. Use los resultados de su ejecución.

## Preguntas evaluadas

1. ¿Qué información describe el contenido de los recursos y qué información constituye el perfil del usuario? Identifique ambas en los archivos del proyecto.
   Respuesta: 

En recursos.csv se encuentran el ID, título, descripción y etiquetas de cada recurso. Solo las etiquetas se utilizan para calcular la similitud. En perfiles.json se almacenan los usuarios y sus intereses iniciales, que se convierten en vectores TF-IDF


2. ¿Por qué se utiliza fit_transform sobre el catálogo y transform sobre los intereses del usuario? ¿Qué problema causaría ajustar un vectorizador diferente para cada perfil?
   Respuesta: 

fit_transform() construye el vocabulario y los vectores del catálogo, mientras que transform() sirve para represetnar los intereses utilizando ese mismo vocabulario. Utilizar vectorizadores diferentes produciría vectores incompatibles y similitudes incorrectas

3. ¿Por qué el mismo catálogo puede producir recomendaciones distintas para u1 y u2? ¿Una similitud de 0,80 significa que existe un 80 % de probabilidad de que el recurso guste? Justifique.
   Respuesta: 

Porque cada usuario tiene intereses diferentes, por ejemplo u1 se relaciona con Python y datos, mientras que u2 con Angular. Una similitud de 0,80 no representa una probabilidad, sino el grado de coincidencia entre dos vectores

4. ¿En qué se diferencia modificar manualmente los intereses iniciales de la adaptación implementada? Identifique el evento que provoca el cambio, el estado modificado y su efecto observable.
   Respuesta: 

La adaptación ocurre automáticamente cuando el usuario selecciona "Me interesa". Esto actualiza su vector mediante actualizar_perfil(), registra la interacción y recalcula las recomendaciones sin modificar los intereses iniciales del JSON

5. Relacione su implementación con las etapas del ciclo MAPE-K estudiado en clase: Monitorizar, Analizar, Planificar, Ejecutar y Conocimiento. Indique una función, operación o dato de su programa para cada etapa.
   Respuesta: 
   
Monitorizar: registrar la interacción "Me interesa"

Analizar: calcular similitudes mediante calcular_puntuaciones()

Planificar: seleccionar y ordenar recursos mediante recomendar()

Ejecutar: modificar el vector utilizando actualizar_perfil()

Conocimiento: mantener los perfiles, historiales y vectores TF-IDF

6. Indique una limitación del mecanismo implementado y proponga una mejora concreta. Explique qué dato adicional o qué cambio del algoritmo exigiría esa mejora.
   Respuesta: 
   
Una limitación del mecanismo podría ser que el sistema solo considera interacciones positivas. Una mejora sería incorporar "No me interesa", registrando valoraciones negativas y modificando el algoritmo para reducir la influencia de recursos rechazados en futuras recomendaciones

## Archivos de la entrega

Entregue `app.py`, `motor.py`, `datos/recursos.csv`, `datos/perfiles.json`,
`requirements.txt`, `.gitignore`, este README completado y los dos archivos de
`evidencias/`. No incluya `.venv`, `__pycache__` ni dependencias instaladas.

### Reproducción desde una copia nueva

Compruebe que otra persona puede crear el entorno, instalar las dependencias y
ejecutar los comandos anteriores utilizando únicamente los archivos entregados.
Registre aquí cualquier instrucción adicional necesaria: [Completar].
