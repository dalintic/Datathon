# 📊 Predicción de Ventas por Producto  

## 📌 Descripción  
Este proyecto tiene como objetivo **predecir la cantidad diaria de ventas por producto** a partir de datos históricos, información de calendario y condiciones climáticas.  

Para ello:  
- Se entrena un modelo individual para cada producto.  
- Se evalúa con un horizonte de 30 días.  
- Posteriormente, los modelos individuales se combinan en un **ensemble** para automatizar las predicciones de todos los productos.  
- El modelo final se **registra en MLflow**, se **empaqueta en Docker** y se **sirve como API REST** para facilitar su integración en aplicaciones externas.  
- Finalmente, los resultados de las predicciones se **suben a la base de datos**.  

---

## 🛠️ Tecnologías utilizadas  
- **Python 3.10+**
- **Scikit-learn** → Preprocesamiento y entrenamiento de modelos  
- **MLflow** → Registro y gestión de modelos  
- **Joblib** → Guardado de modelos individuales  
- **Matplotlib & Seaborn** → Visualización de métricas  
- **Docker** → Productivización del modelo  
- **Base de datos (sandbox)** → Almacenamiento de resultados finales  

---

## 📂 Estructura del proyecto
📦 datathon/
├─ 📁 archivos/ # Datos fuente (Excel)
│ ├─ 📄 ArticulosPanaderia.xlsx
│ ├─ 📄 Calendario.xlsx
│ └─ 📄 CantidadPedida.xlsx
├─ 🛠️ mlartifacts/ # Artefactos de MLflow
├─ 📊 mlruns/ # Historial de ejecuciones MLflow
├─ 🤖 product_models/ # Modelos individuales por producto y artefactos del ensemble
│ ├─ 📄 model_{PRODUCT_ID}.pkl
│ ├─ 📄 manifest.json
│ ├─ 📈 pred_vs_actual_{PRODUCT_ID}.png
│ └─ 📈 pred_vs_actual_timeline_{PRODUCT_ID}.png
├─ 📓 Datathon_EDA.ipynb # Notebook de análisis exploratorio
├─ 📓 Datathon.ipynb # Notebook principal (preprocesamiento, entrenamiento y ensemble)
├─ 📝 README.md
└─ 📦 requirements.txt

---
## 📊 Flujo del proyecto

### 1️⃣ Preprocesamiento
- Se rellenan valores nulos con la media en variables numéricas.  
- Se normalizan las variables numéricas con **MinMaxScaler**.  
- Se codifican las variables categóricas con **OneHotEncoder**.  
- Todo esto se integra en un **pipeline de transformación de datos**.

### 2️⃣ Entrenamiento individual
- Se entrena un modelo por cada producto usando su histórico de ventas.  
- El horizonte de prueba se fija en **30 días**:  
  - Entrenamiento con datos previos.  
  - Evaluación con los últimos 30 días.  
- Se registran métricas como **MAE** y **R²**, junto con gráficos de comparación.  
- Cada modelo se guarda como `model_{PRODUCT_ID}.pkl`.

### 3️⃣ Ensamble
- Los modelos entrenados se combinan en un **ensemble**.  
- Se define un `manifest.json` con la información de cada producto y sus columnas de entrada.  
- El ensemble se registra en MLflow como un único modelo:
    Registrado como: models:/Datathon/<num_version>
    Created version '<num_version>' of model 'Datathon'.

### 4️⃣ Productivización
- Se crea una imagen Docker con el modelo registrado en MLflow.  
- El modelo puede ejecutarse en un contenedor y exponerse en un puerto, lo que permite **hacer peticiones vía API REST**.

### 5️⃣ Predicciones y base de datos
- Se generan predicciones de ventas para todos los productos.  
- Se construye un DataFrame final con columnas relevantes. 

---
## ⚙️ Instalación  
1. Clona el repositorio:  
   ```bash
   git clone https://github.com/tu_usuario/ventas-prediccion.git
   cd ventas-prediccion
2. Instala las dependencias: 
    ```bash
    pip install -r requirements.txt
3. Inicia MLflow Tracking Server en local: 
    ```bash
    export MLFLOW_TRACKING_URI=http://localhost:5000
    mlflow ui
4. Ejecuta el notebook principal paso a paso para:
  - Preprocesar datos
  - Entrenar modelos individuales
  - Crear el ensemble
  - Registrar en MLflow
  - Subir resultados a la base de datos

