# Instrucciones para agentes AI (Copilot)

Breve: Este repositorio es mínimo y contiene principalmente el notebook `Examen.ipynb` con la implementación de utilidades numéricas en Python (principalmente NumPy). El objetivo de estas instrucciones es permitir que un agente AI sea inmediatamente productivo al editar, probar y entender el código.

- **Punto de entrada principal**: `Examen.ipynb` — revisar las celdas de código para encontrar funciones clave.

- **Arquitectura / propósito**: Código orientado a cálculos numéricos y resolución de sistemas lineales; no hay servicios web ni tests automatizados. Las funciones trabajan directamente sobre arreglos NumPy y registran información con `logging`.

- **Funciones críticas**:
  - `eliminacion_gaussiana(A: np.ndarray)` — espera una matriz ampliada de tamaño n x (n+1). Implementa pivoteo parcial por magnitud y sustitución hacia atrás. Revisar mensajes `logging.info` que imprimen la matriz transformada.
  - `ajustar_min_cuadrados(xs, ys, gradiente)` — construye la matriz aumentada `Ab` llamando a las funciones de derivadas parciales y resuelve con `eliminacion_gaussiana`.
  - `der_parcial_a0(xs, ys)` — función placeholder en `Examen.ipynb`; las derivadas parciales deben devolver una fila de la matriz aumentada (num_pars + 1 elementos).

- **Convenciones observables**:
  - Validaciones simples mediante `assert` para verificar formas y longitudes (ej.: `assert len(xs) == len(ys)`).
  - Uso de `numpy` como dependencia principal; evitar conversiones innecesarias excepto cuando se normaliza la entrada.
  - Logging: `logging.info` y `logging.debug` se usan para traza; mantener el formato actual al instrumentar código.

- **Flujos de trabajo recomendados**:
  - Entender / ejecutar el notebook localmente: instalar dependencias y usar Jupyter.
    - Instalación mínima: `pip install numpy jupyter`
    - Ejecutar notebook interactivamente: `jupyter notebook` o `jupyter lab` y abrir `Examen.ipynb`.
    - Ejecutar notebook por línea de comandos: `jupyter nbconvert --to notebook --execute Examen.ipynb --output executed.ipynb` (útil para validar cambios sin abrir UI).
  - Extraer a script para pruebas rápidas: `jupyter nbconvert --to script Examen.ipynb` genera `Examen.py` que puede ejecutarse con `python Examen.py` tras adaptar imports si es necesario.

- **Patrones de edición**:
  - Mantener firmas existentes de funciones públicas (no renombrar parámetros sin razón).
  - Para agregar derivadas parciales, devolver exactamente una fila con `num_pars + 1` elementos (coeficientes + término independiente) para que `ajustar_min_cuadrados` pueda ensamblar `Ab` correctamente.
  - Cuando modifiques `eliminacion_gaussiana`, respeta su contrato: recibe matriz ampliada y devuelve vector solución (np.ndarray).

- **Dependencias e integración**:
  - Dependencias: `numpy` y la librería estándar `logging`.
  - No hay integraciones externas detectadas (APIs, bases de datos, CI). Si agregas CI, documenta comandos de ejecución del notebook en el README.

- **Ejemplos concretos (para probar cambios)**:
  - Validar la eliminación: en una celda nueva del notebook crear `A = np.array([[2,1,5],[4,-6,-2]])` y llamar a `eliminacion_gaussiana(A.copy())` para confirmar salida esperada.
  - Probar ajuste: implementar `der_parcial_a0` que devuelva una fila y usar `ajustar_min_cuadrados(xs, ys, [der_parcial_a0, ...])`.

- **Qué evitar**:
  - No convertir innecesariamente estructuras a listas/tuplas si el código espera `np.ndarray`.
  - No cambiar el formato de logs ni las aserciones sin ajustar llamadas dependientes.

Si algo en estas instrucciones no queda claro o quieres que incluya más ejemplos concretos (por ejemplo, bloques de prueba con valores numéricos), dime y actualizo el archivo.
