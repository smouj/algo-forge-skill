title: "Habilidad Algo Forge"
description: "Una habilidad especializada de OpenClaw para crear algoritmos personalizados a partir de descripciones en lenguaje natural, automatizando la generación de código para tareas computacionales complejas."
author: "Equipo OpenClaw"
version: "1.2.0"
tags: ["algorithms", "generation", "automation", "code", "nlp", "optimization"]
category: "Herramientas de Desarrollo"
requires: ["python>=3.8", "openai-api", "numpy", "scipy"]
env_vars: ["OPENAI_API_KEY", "ALGO_FORGE_MODEL=gpt-4-turbo"]
---

# Habilidad Algo Forge

## Propósito

Algo Forge está diseñado para cerrar la brecha entre problemas computacionales descritos por humanos y algoritmos ejecutables. Se destaca en transformar especificaciones en lenguaje natural en código optimizado y listo para producción en múltiples lenguajes de programación (Python, JavaScript, C++, Rust). Los casos de uso reales clave incluyen:

- **Modelado Financiero**: Generar algoritmos de simulación Monte Carlo para evaluación de riesgo de portafolios a partir de descripciones como "simular 10,000 trayectorias de precios de acciones usando movimiento browniano geométrico con volatilidad 0.2 y deriva 0.05".
- **Pipelines de Procesamiento de Datos**: Crear algoritmos ETL para procesar grandes conjuntos de datos, como "extraer logs JSON de S3, filtrar por timestamp y ID de usuario, agregar métricas por hora, y enviar salida a PostgreSQL".
- **Preprocesamiento de Machine Learning**: Automatizar scripts de ingeniería de características, por ejemplo, "normalizar características numéricas usando z-score, codificar one-hot variables categóricas con menos de 10 valores únicos, y manejar valores faltantes con imputación de mediana".
- **Problemas de Optimización**: Resolver desafíos combinatorios como "implementar un algoritmo genético para optimizar el diseño de almacén minimizando la distancia de viaje para 50 productos y 10 pasillos".
- **Análisis en Tiempo Real**: Construir algoritmos de streaming para "calcular promedios de ventana deslizante de datos de sensores en intervalos de 5 minutos con ponderación de decaimiento exponencial".

## Alcance

Algo Forge opera a través de integración de interfaz de línea de comandos con OpenClaw, enfocándose en generación y refinamiento de algoritmos. Los comandos admitidos incluyen:

- `/algo-forge generate <description>`: Crea algoritmo inicial a partir de prompt en lenguaje natural
- `/algo-forge refine <file> --optimize=performance`: Mejora algoritmo existente con indicadores de optimización específicos (performance, memory, readability)
- `/algo-forge test <file> --input=data.json --output=results.csv`: Ejecuta algoritmo contra datos de prueba y valida salidas
- `/algo-forge benchmark <file> --metrics=time,space --iterations=100`: Evalúa rendimiento del algoritmo en múltiples ejecuciones
- `/algo-forge document <file> --format=markdown`: Genera documentación completa con análisis de complejidad
- `/algo-forge migrate <file> --to=javascript`: Traduce algoritmo a lenguaje diferente preservando la lógica

Dependencias: Requiere acceso a OpenAI API, NumPy para cálculos numéricos, y SciPy para algoritmos científicos. El entorno debe tener ALGO_FORGE_MODEL configurado al modelo GPT preferido.

## Proceso de Trabajo

Algo Forge sigue un flujo de trabajo estructurado de 5 pasos para generación confiable de algoritmos:

1. **Análisis de Prompt**: Analizar descripción en lenguaje natural usando NLP para identificar tipo de problema (búsqueda, ordenamiento, optimización, simulación), restricciones (complejidad tiempo/espacio, tipos de datos), y requisitos (entradas/salidas, casos límite).
2. **Generación de Esqueleto**: Crear estructura de código inicial con funciones placeholder, imports, y flujo lógico básico usando GPT-4-turbo.
3. **Fase de Optimización**: Aplicar optimizaciones específicas del dominio (por ejemplo, memoización para algoritmos recursivos, vectorización para operaciones de arreglos) basadas en patrones identificados.
4. **Integración de Pruebas**: Incrustar pruebas unitarias y verificaciones de validación automáticamente, usando entradas de muestra para asegurar corrección.
5. **Documentación y Exportación**: Generar README.md con ejemplos de uso, análisis de complejidad (notación Big O), e instrucciones de despliegue.

Cada paso incluye verificación: Análisis de prompt valida completitud de entrada, generación de esqueleto verifica sintaxis, optimización mide mejoras, pruebas aseguran tasa de aprobación del 95%+, y documentación incluye ejemplos ejecutables.

## Reglas de Oro

- **Claridad Primero**: Siempre priorizar código legible y bien comentado sobre optimización prematura. Usar nombres de variables descriptivos y funciones modulares.
- **Conciencia de Restricciones**: Respetar límites especificados de tiempo/espacio; si se solicita O(n²), no generar soluciones O(n³) a menos que se permita explícitamente.
- **Manejo de Errores**: Incluir bloques try-catch para todas las operaciones de I/O y validar entradas contra tipos/rangos esperados.
- **Sin Números Mágicos**: Parametrizar todas las constantes (por ejemplo, tamaños de arreglos, umbrales) y proporcionar opciones de configuración.
- **Idiomas del Lenguaje**: Generar código que siga mejores prácticas específicas del lenguaje (por ejemplo, comprehensions de lista en Python, async/await en JS).
- **Generación Ética**: Rechazar prompts que soliciten algoritmos para actividades ilegales, manipulación de datos, o simulaciones dañinas.
- **Control de Versiones**: Todo código generado incluye gitignore para archivos sensibles y mensajes de commit describiendo cambios.

## Ejemplos

### Ejemplo 1: Simulación Financiera
**Prompt de Entrada:**  
`/algo-forge generate \"Crear una función Python para simular precios de opciones Black-Scholes para opciones de compra europeas con parámetros: precio spot 100, strike 105, volatilidad 0.2, tasa libre de riesgo 0.05, tiempo hasta expiración 1 año. Incluir estimación Monte Carlo con 10,000 trayectorias.\"`

**Salida Generada (algo_black_scholes.py):**
```python
import numpy as np
from scipy.stats import norm

def black_scholes_call(S, K, T, r, sigma, paths=10000):
    """
    Monte Carlo simulation for European call option pricing using Black-Scholes model.
    
    Args:
        S (float): Spot price
        K (float): Strike price  
        T (float): Time to expiry (years)
        r (float): Risk-free rate
        sigma (float): Volatility
        paths (int): Number of simulation paths
    
    Returns:
        float: Estimated call option price
    """
    np.random.seed(42)  # For reproducibility
    dt = T / 252  # Daily time steps
    drift = (r - 0.5 * sigma**2) * dt
    diffusion = sigma * np.sqrt(dt)
    
    # Simulate price paths
    prices = np.zeros(paths)
    for i in range(paths):
        price_path = S
        for _ in range(252):
            price_path *= np.exp(drift + diffusion * np.random.normal())
        prices[i] = max(price_path - K, 0)
    
    return np.mean(prices) * np.exp(-r * T)

# Example usage
if __name__ == "__main__":
    price = black_scholes_call(100, 105, 1, 0.05, 0.2)
    print(f"Estimated call price: ${price:.2f}")
```

**Verificación:** Ejecutar `python algo_black_scholes.py` - debería mostrar ~$8.02 (aproxima el valor analítico de Black-Scholes).

### Ejemplo 2: Optimización de Pipeline de Datos
**Prompt de Entrada:**  
`/algo-forge refine data_pipeline.py --optimize=performance`

**Antes (Original):**
```python
def process_data(data):
    result = []
    for item in data:
        if item['status'] == 'active':
            result.append(item['value'] * 2)
    return result
```

**Después (Optimizado):**
```python
import numpy as np

def process_data(data):
    """
    Optimized data processing using vectorized operations.
    Filters active items and doubles values.
    """
    values = np.array([item['value'] for item in data if item['status'] == 'active'])
    return (values * 2).tolist()
```

**Benchmark de Rendimiento:** `time python -c \"import data_pipeline; data_pipeline.process_data([{'value': i, 'status': 'active' if i%2 else 'inactive'} for i in range(100000)])\"` - La versión vectorizada ejecuta 50x más rápido en grandes conjuntos de datos.

### Ejemplo 3: Traducción de Algoritmo
**Prompt de Entrada:**  
`/algo-forge migrate fibonacci.py --to=javascript`

**Entrada en Python:**
```python
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```

**Salida en JavaScript:**
```javascript
function fibonacci(n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// Example usage with memoization optimization
const memo = new Map();
function optimizedFibonacci(n) {
    if (memo.has(n)) return memo.get(n);
    if (n <= 1) return n;
    const result = optimizedFibonacci(n - 1) + optimizedFibonacci(n - 2);
    memo.set(n, result);
    return result;
}
```

## Comandos de Reversión

- **Revertir Generación:** `/algo-forge rollback generate <session_id>` - Remueve todos los archivos creados en la última sesión de generación y restaura el espacio de trabajo al estado pre-generación.
- **Deshacer Refinamiento:** `/algo-forge rollback refine <file> --version=previous` - Revierte archivo a versión antes de la última optimización, preservando historial de git.
- **Reiniciar Benchmark:** `/algo-forge rollback benchmark <file>` - Elimina logs de benchmark y archivos temporales de prueba generados durante análisis de rendimiento.
- **Limpiar Cache:** `/algo-forge rollback cache` - Purga todos los algoritmos y prompts en cache, forzando generación fresca en la siguiente ejecución.

## Dependencias y Requisitos

- **Python 3.8+** con NumPy 1.21+, SciPy 1.7+
- **Clave de API de OpenAI** con créditos suficientes para llamadas GPT-4-turbo (estimado $0.02 por algoritmo complejo)
- **Variables de Entorno:** 
  - `OPENAI_API_KEY`: Tu clave de API de OpenAI
  - `ALGO_FORGE_MODEL`: Por defecto "gpt-4-turbo" (puede sobrescribirse a "gpt-3.5-turbo" para ahorro de costos)
  - `ALGO_FORGE_CACHE_DIR`: Directorio de cache personalizado opcional (por defecto ~/.openclaw/algo-forge-cache)
- **Requisitos del Sistema:** 4GB RAM mínimo, 2GB espacio en disco para cache

## Pasos de Verificación

1. **Verificación de Sintaxis:** Ejecutar `python -m py_compile <generated_file>` o equivalente para el lenguaje objetivo.
2. **Pruebas Unitarias:** Ejecutar pruebas incrustadas con `pytest <generated_file>` (Algo Forge incluye 5-10 casos de prueba automáticamente).
3. **Validación de Rendimiento:** Usar `/algo-forge benchmark` para asegurar que cumple con límites de complejidad especificados.
4. **Consistencia de Salida:** Comparar salidas generadas contra resultados esperados para entradas de muestra.
5. **Prueba de Integración:** Ejecutar en entorno objetivo y verificar que no hay errores de runtime.

## Solución de Problemas

- **Límites de Tasa de API:** Error "429 Too Many Requests" - Esperar 60 segundos o cambiar a modelo más económico.
- **Problemas de Memoria:** Grandes simulaciones fallan - Reducir trayectorias/iteraciones o usar enfoque de streaming.
- **Lógica Incorrecta:** Algoritmo no coincide con descripción - Refinar prompt con restricciones más específicas.
- **Errores de Lenguaje:** Migración falla - Asegurar que runtime del lenguaje objetivo esté instalado y compatible.
- **Degradación de Rendimiento:** Optimizaciones no efectivas - Verificar tipos de datos; considerar procesamiento paralelo para tareas limitadas por CPU.

## Registro de Cambios

- v1.2.0: Agregada migración multi-lenguaje, mejorado manejo de errores
- v1.1.0: Integrado suite de benchmarking, mejorado parsing NLP
- v1.0.0: Lanzamiento inicial con capacidades de generación core