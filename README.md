# Práctica MLOps: Versionado de Modelos con MLflow y Git (Actividad 2)

Este repositorio contiene el trabajo realizado para la Actividad 2 de la asignatura de MLOps, centrada en la implementación de un flujo de trabajo práctico para el versionado de modelos de Machine Learning.

## Descripción del Proyecto

El objetivo principal de esta práctica es demostrar cómo gestionar eficazmente las diferentes versiones de un modelo de análisis de sentimiento utilizando herramientas clave de MLOps:

*   **MLflow:** Para el seguimiento exhaustivo de experimentos (parámetros, métricas) y el registro formal de modelos, permitiendo gestionar y comparar diferentes versiones.
*   **Git y GitHub:** Como sistema de control de versiones para el código fuente del cuaderno Jupyter, los conjuntos de datos utilizados, los modelos iniciales y, crucialmente, los metadatos y artefactos generados por MLflow durante el seguimiento local.

El flujo de trabajo implementado en el cuaderno Jupyter (`ACTIVIDAD2_FABIAN_JIMENEZ_LLODRA.ipynb`) sigue estos pasos:

1.  **Configuración del Entorno:** Instalación de dependencias Python y descarga de recursos NLTK.
2.  **Inicialización:** Clonación de un repositorio base (proporcionado por el profesor) y configuración del seguimiento local de MLflow dentro de la estructura del proyecto.
3.  **Carga y Evaluación Inicial:** Carga de un modelo de análisis de sentimiento preentrenado y evaluación de su rendimiento en un nuevo conjunto de datos para detectar posible *drift* (degradación del rendimiento).
4.  **Reentrenamiento:** Si la evaluación lo justifica (o como parte de un ciclo), se reentrena el modelo utilizando el nuevo conjunto de datos.
5.  **Evaluación Post-Reentrenamiento:** Se evalúa el modelo recién entrenado en conjuntos de entrenamiento y prueba para verificar su rendimiento y generalización.
6.  **Versionado con MLflow:** Se registra el modelo reentrenado en el **Registro de Modelos de MLflow**, creando automáticamente una nueva versión si el modelo ya existía. Las métricas clave del modelo reentrenado también se registran en la ejecución de MLflow asociada.
7.  **Persistencia en GitHub:** Se utilizan comandos Git para confirmar (commit) todos los cambios locales (incluyendo el código y el directorio `mlflow/` con los datos de seguimiento) y enviarlos (push) a este repositorio de GitHub.

## Estructura del Repositorio

*   **`ACTIVIDAD2_FABIAN_JIMENEZ_LLODRA.ipynb`:** El cuaderno Jupyter principal que contiene todo el código y las explicaciones del flujo de trabajo.
*   **`datasets/`:** Directorio que contiene los conjuntos de datos utilizados.
    *   `Apple-Twitter-Sentiment.csv`: Dataset original (posiblemente).
    *   `Apple-Twitter-Sentiment_new.csv`: Dataset utilizado para la evaluación del drift y el reentrenamiento.
*   **`modelos/`:** Directorio que contiene el modelo inicial y artefactos relacionados.
    *   `sentiment_analysis_pipeline.joblib`: El pipeline de análisis de sentimiento inicial preentrenado.
    *   `lda_model.joblib`: (Si aplica) Otro modelo inicial cargado.
    *   `preprocess_text_function.joblib`: La función de preprocesamiento de texto guardada.
*   **`mlflow/`:** Directorio generado por MLflow que contiene los metadatos de los experimentos, ejecuciones (runs), parámetros, métricas y artefactos registrados localmente. **Este directorio es esencial y se incluye en el control de versiones de Git** para mantener la trazabilidad completa del proyecto.
    *   `mlflow/mlruns/`: Contiene los directorios de los experimentos y sus ejecuciones.
    *   `mlflow/models/`: Contiene los metadatos de los modelos registrados localmente.
*   **`README.md`:** Este archivo.

## Cómo Ejecutar

1.  **Clonar el Repositorio:**
    ```bash
    git clone git@github.com:fajillo77/actividad2-mlops.git
    cd actividad2-mlops
    ```
2.  **Entorno:** Se recomienda utilizar un entorno virtual (como `venv` o `conda`) para instalar las dependencias.
3.  **Instalar Dependencias:** (Si no se ejecuta dentro del notebook)
    ```bash
    pip install scikit-learn==1.3.1 pandas numpy==1.26.4 nltk==3.9.1 seaborn==0.13.2 xgboost mlflow==2.19.0 flask pyngrok joblib notebook
    ```
4.  **Ejecutar el Cuaderno:** Abrir y ejecutar el archivo `.ipynb` (`ACTIVIDAD2_FABIAN_JIMENEZ_LLODRA.ipynb` o la versión mejorada) utilizando Jupyter Notebook, JupyterLab o Google Colab.
    *   **Nota sobre Google Colab:** Si se ejecuta en Colab, será necesario montar Google Drive para acceder a la clave SSH (si se utiliza la sección de push a GitHub) y asegurarse de que la ruta a la clave (`/content/drive/MyDrive/id_rsa`) sea correcta.
5.  **Interfaz de MLflow (Opcional):** Para visualizar los experimentos registrados localmente, puedes ejecutar la interfaz de usuario de MLflow desde la terminal, dentro del directorio `Practica_MLOps` (o el nombre que le hayas dado al clonar):
    ```bash
    mlflow ui --backend-store-uri mlflow/
    ```
    Luego, accede a `http://localhost:5000` (o la URL que indique) en tu navegador.

## Requisitos de Configuración para el Push a GitHub

Para que la última celda del cuaderno (que realiza el `git push`) funcione correctamente, se necesita:

1.  **Una clave SSH:** Generada y guardada (por ejemplo, en Google Drive si usas Colab).
2.  **Clave SSH pública añadida a GitHub:** La parte pública de tu clave SSH debe estar registrada en la configuración de tu cuenta de GitHub.
3.  **URL SSH del Repositorio Correcta:** En la celda 8 del cuaderno, la variable `GITHUB_REPO_SSH_URL` debe contener la URL SSH de *tu* repositorio de destino (p. ej., `git@github.com:tu_usuario/tu_repo.git`), no un placeholder.

## Conclusiones de la Práctica

Esta actividad demuestra la importancia y la viabilidad de aplicar principios MLOps como el seguimiento de experimentos y el versionado de modelos. MLflow facilita enormemente el registro y la gestión de diferentes iteraciones de un modelo, mientras que Git/GitHub proporciona la infraestructura para versionar todo el proyecto, incluyendo el código y los resultados trazables de MLflow, lo cual es fundamental para la reproducibilidad y la colaboración en proyectos de Machine Learning.
