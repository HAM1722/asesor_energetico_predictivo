
[README.md](https://github.com/user-attachments/files/22989953/README.md)
# Asesor Energético Predictivo

Sistema de análisis predictivo avanzado para el consumo energético de restaurantes, optimizado específicamente para negocios de comida italiana en Cali, Colombia. Utiliza regresión Ridge con features meteorológicas reales, horas de operación y lags temporales.

## 🎯 Características Principales

- **✅ Modelo Validado:** MAPE de 3.5% (excelencia en precisión)
- **📊 Datos Reales:** Integración de datos meteorológicos de Cali, Colombia
- **🏪 Optimizado para Restaurantes:** Horas de operación y patrones estacionales
- **📈 Predicciones Futuras:** Forecast de 3 meses con análisis estacional
- **📄 Informes Profesionales:** Reportes Word automáticos con gráficos y tablas
- **🔧 Fácil Configuración:** Pipeline automatizado con validación de datos

## Estructura del Proyecto

```
asesor_energetico_predictivo/
├── data/                    # Datos de entrada (Excel)
├── models/                  # Modelos entrenados (.pkl)
├── output/                  # Gráficos e informes generados
├── src/                     # Código fuente
│   ├── config.py           # Configuración y rutas
│   ├── data_loader.py      # Carga y validación de datos
│   ├── preprocess.py       # Preprocesamiento y features
│   ├── model.py            # Entrenamiento y evaluación
│   ├── forecast.py         # Predicciones a futuro
│   ├── report.py           # Generación de reportes
│   ├── weather_api.py      # Integración meteorológica
│   ├── weather_scraper.py  # Web scraping meteorológico
│   └── main.py             # Pipeline principal
├── requirements.txt         # Dependencias
└── README.md               # Este archivo
```

## Instalación

1. **Crear entorno virtual:**
```powershell
python -m venv venv
.\venv\Scripts\Activate.ps1
```

2. **Instalar dependencias:**
```powershell
pip install -r requirements.txt
```

## Uso

### Preparación de Datos

Coloca tu archivo Excel en la carpeta `data/` con el nombre `Consumo_Energia_CLIENTE.xlsx`.

**Columnas requeridas:**
- `Mes`: Fecha del mes (formato fecha)
- `Consumo_KWh`: Consumo en kilovatios-hora

**Columnas opcionales:**
- `Tarifa_Promedio`: Tarifa promedio del mes
- `Valor_Factura`: Valor total de la factura
- `temperatura`: Temperatura promedio mensual (°C) - datos reales de Cali
- `humedad`: Humedad relativa promedio mensual (%)
- `precipitacion`: Precipitación total mensual (mm)
- `horas_operacion`: Horas de operación del restaurante (10h/día promedio)
- `Evento/Observacion`: Notas especiales del mes

**Requisitos:**
- Mínimo 36 meses de datos
- Sin duplicados en la columna Mes
- Continuidad mensual (sin meses faltantes)

### Ejecución

```powershell
python src/main.py
```

### Salidas Generadas

El sistema genera automáticamente:

- **📊 Gráfico:** `output/real_vs_pred.png` - Comparación real vs predicho con fechas reales
- **📄 Informe Word:** `output/informe_CLIENTE_YYYY-MM-DD.docx` - Reporte completo con:
  - Resumen ejecutivo y métricas
  - Tabla de desviaciones con fechas reales (no índices numéricos)
  - Gráfico comparativo embebido
  - **🆕 Sección de Predicciones Futuras** (próximos 3 meses)
  - Recomendaciones basadas en el rendimiento
- **🤖 Modelo:** `models/model_ridge.pkl` - Modelo entrenado guardado

## Características del Modelo

### Features Utilizadas (10 features total)
- **Features de calendario:** `mes_sin`, `mes_cos` (capturan estacionalidad)
- **Lags temporales:** `lag1`, `lag2`, `lag3` (consumo de meses anteriores)
- **Datos meteorológicos reales:** `temperatura`, `humedad`, `precipitacion` (datos de Cali)
- **Horas de operación:** `horas_operacion` (10 horas/día promedio, variación estacional)
- **Tarifa promedio:** `Tarifa_Promedio` (si está disponible en los datos)

### Algoritmo
- **Regresión Ridge** con validación cruzada temporal
- **Grid Search** para optimización de hiperparámetros
- **StandardScaler** para normalización de features

### Métricas de Evaluación
- **MAPE:** Error absoluto porcentual medio (objetivo ≤ 20%)
- **MAE:** Error absoluto medio
- **RMSE:** Raíz del error cuadrático medio

## Interpretación de Resultados

### Criterio de Aceptación
- **MAPE ≤ 20%:** Modelo ACEPTADO ✅
- **MAPE > 20%:** Modelo requiere revisión ⚠️

### Estados de Desviación
- **ESTABLE:** |desviación| ≤ 20%
- **ALERTA:** |desviación| > 20%

## Personalización

### Modificar Parámetros

Edita `src/config.py` para cambiar:
- Valores de alpha para Ridge
- Proporción de train/test
- Número de meses a predecir
- Umbral de MAPE para alertas

### Agregar Features

Modifica `src/preprocess.py` para incluir:
- Más lags temporales
- Features de tendencia
- Variables externas
- Días festivos y eventos especiales

### Datos Meteorológicos

El sistema incluye:
- **Datos reales de Cali:** Temperatura (20-33°C), humedad (65-85%) y precipitación históricos
- **Variación estacional:** Patrones reales de clima tropical colombiano
- **Integración automática:** Prioriza datos existentes en Excel, con fallback a simulación realista

## Solución de Problemas

### Error: "Faltan columnas obligatorias"
- Verifica que tu Excel tenga las columnas `Mes` y `Consumo_KWh`

### Error: "Se requieren al menos 36 meses"
- Asegúrate de tener al menos 3 años de datos históricos

### Error: "Meses faltantes detectados"
- Verifica que no falten meses en tu serie temporal
- Los datos deben tener continuidad mensual

### MAPE > 20%
- Revisa la calidad de los datos históricos
- Considera agregar más variables explicativas
- Prueba diferentes valores de alpha

## Características Específicas para Restaurantes

### Optimizado para Restaurantes Italianos en Cali
- **Horas de operación:** 10 horas diarias promedio (300h/mes)
- **Variación estacional:** Diciembre (360h), Noviembre (345h), temporada alta
- **Clima tropical:** Datos meteorológicos específicos de Cali (20-33°C)
- **Consumo promedio:** 3,800-4,200 KWh/mes

### Rendimiento Actual
- **MAPE: 3.5%** ✅ (Excelencia en precisión)
- **MAE: 139.91 KWh** (Error promedio bajo)
- **RMSE: 170.38 KWh** (Error cuadrático medio)
- **📈 Predicciones 2025:** Octubre 3,961 KWh, Noviembre 3,872 KWh, Diciembre 3,767 KWh
- **📊 Datos históricos:** Septiembre 2022 - Septiembre 2025 (37 meses)
- **🎯 Período de test:** Marzo 2023 - Septiembre 2022 (7 meses para validación)

## 🚀 Últimas Actualizaciones

### v2.0 - Mejoras en Reportes (Septiembre 2025)
- ✅ **Tabla de desviaciones con fechas reales** (no más índices numéricos)
- ✅ **Sección de Predicciones Futuras** en informe Word
- ✅ **Análisis estacional** para cada predicción
- ✅ **Validación de datos mejorada** con limpieza automática de columnas
- ✅ **Pipeline robusto** con manejo de errores mejorado

### Características Implementadas
- 🔧 **10 features** incluyendo datos meteorológicos reales y horas de operación
- 📊 **Validación temporal** con split cronológico (79.4% train, 20.6% test)
- 🎯 **Optimización automática** de hiperparámetros con GridSearch
- 📈 **Predicción recursiva** para 3 meses futuros
- 📄 **Informes profesionales** con gráficos embebidos

## Próximas Mejoras

- 🔗 Integración con Google Sheets
- 📧 Envío automático de reportes por email
- 📱 Dashboard web interactivo
- 🤖 Modelos de ensemble (Random Forest, XGBoost)
- 🔍 Detección automática de outliers
- 📅 Análisis de eventos especiales y días festivos
- ⏰ Predicción de demanda por horarios específicos
- 🌐 API REST para integración con otros sistemas
