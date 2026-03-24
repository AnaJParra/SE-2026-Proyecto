# Sistema de Retroalimentación Háptica para Pisada - Plantilla Inteligente

## Requisitos Funcionales (RF)

| ID      | Requerimiento Funcional                                                                 | Prioridad |
|---------|-----------------------------------------------------------------------------------------|-----------|
| RF - 01 | El sistema debe adquirir la señal de presión plantar en el talón mediante un sensor FSR cada 10 ms (±2 ms). | Alta      |
| RF - 02 | El sistema debe aplicar un filtro físico simple (media móvil de 5 muestras) a la señal de presión para reducir ruido. | Alta      |
| RF - 03 | El sistema debe detectar cuando la presión en el talón supera un umbral configurable de impacto (por defecto 3.5 g equivalente). | Alta      |
| RF - 04 | El sistema debe activar el actuador háptico (motor de vibración) durante 200 ms cuando se detecte un impacto excesivo. | Alta      |
| RF - 05 | El sistema debe registrar en memoria interna cada evento de impacto excesivo con timestamp. | Media     |
| RF - 06 | El sistema debe implementar un módulo de logging que registre al menos: timestamp, tipo de evento (INFO/WARNING/ERROR), código de error (si aplica) y valor de presión. | Alta      |
| RF - 07 | El sistema debe enviar por UART (115200 baud) en tiempo real el valor de presión actual cada 100 ms cuando esté conectado a un monitor serial. | Media     |
| RF - 08 | El sistema debe mostrar en la pantalla OLED: valor actual de presión (en bits o %), estado del sistema (OK / Alerta / Error) y contador de impactos excesivos. | Alta      |
| RF - 09 | El sistema debe detectar fallos críticos del sensor FSR (valor fuera de rango [50–950] o desconexión) y registrar un evento ERROR con código específico. | Alta      |
| RF - 11 | El sistema debe permitir configurar el umbral de impacto excesivo mediante comandos seriales. | Baja      |

## Requisitos No Funcionales (RNF)

| ID      | Requisito No Funcional                                                                 | Categoría               |
|---------|----------------------------------------------------------------------------------------|-------------------------|
| RNF - 01 | El sistema debe consumir menos de 150 mA en operación normal.                         | Rendimiento / Consumo   |
| RNF - 02 | El firmware debe tener un mecanismo de watchdog que reinicie el ESP32 si el loop principal se cuelga > 5 s. | Confiabilidad           |
| RNF - 03 | El sistema debe ser capaz de operar continuamente durante al menos 4 horas.           | Autonomía               |
| RNF - 04 | La plantilla debe soportar al menos 500 ciclos de pisada (equivalente a ~50 km de carrera) sin degradación significativa del sensor FSR ni del actuador. | Durabilidad             |
| RNF - 05 | El código fuente debe cumplir con un estilo de codificación limpio y comentado.       | Mantenibilidad          |

## Plan de Testing / Pruebas

### 1. Estrategia general de verificación y validación

#### Nivel de prueba 1 – Pruebas unitarias (en host con simuladores o mocks)

**Objetivo:** Verificar que cada función o módulo individual funcione correctamente de forma aislada, sin depender del hardware real ni de otros componentes del sistema.

**Ejemplos de elementos a probar:**
- Función `leer_presion_FSR()` → se simula el valor del ADC con un mock.
- Función `aplicar_filtro_media_movil()` → se pasa un arreglo de 5 valores y se verifica el promedio.
- Función `detectar_impacto_excesivo()` → casos frontera (debajo, encima y en el umbral).
- Función `formatear_log_evento()` → formato correcto de logs.
- Función `calcular_consumo_estimado()` → valores coherentes.

#### Nivel de prueba 2 – Pruebas de integración

**Objetivo:** Verificar que los módulos trabajen correctamente cuando se conectan entre sí.

**Ejemplos:**
- Sensor FSR → ESP32 (lectura y conversión)
- ESP32 → actuador háptico
- ESP32 → pantalla OLED
- ESP32 → UART (envío y recepción de comandos)
- Integración del módulo de logging

#### Nivel de prueba 3 – Pruebas de sistema

**Objetivo:** Validar el prototipo completo en un entorno realista.

**Ejemplos de pruebas:**
- Simulación de carrera (pisadas repetitivas durante 5–10 minutos)
- Verificación de detección, activación háptica, OLED y UART
- Registro correcto de logs con timestamp

#### Nivel de prueba 4 – Pruebas de robustez y manejo de fallos

**Objetivo:** Evaluar el comportamiento ante condiciones anormales.

**Ejemplos:**
- Desconexión del sensor FSR
- Saturación del ADC
- Batería baja
- Activación del watchdog
- Ruido extremo en la señal

### 2. Criterios de aceptación

- 100 % de los requisitos de **alta prioridad** deben pasar.
- Al menos 80 % de los requisitos de media y baja prioridad deben pasar.
- No debe haber fallos críticos (crash, datos corruptos, activación errónea del actuador, consumo excesivo).

## Plantilla para Test Cases

| Test Case | Estado [Pendiente / Passed / Failed] | TC - XXX |
|-----------|--------------------------------------|----------|
| **Requirement Traceability** | RF - XX / RNF - XX |
| **Test Objective** | [Descripción clara del objetivo] |
| **Preconditions** | - [Condición 1]<br>- [Condición 2]<br>- [Condición 3] |
| **Steps** | 1. [Paso 1]<br>2. [Paso 2]<br>3. [Paso 3] |
| **Expected Results** | [Resultado esperado descrito] |
| **Actual Results** | [Se llena después de ejecutar] |
| **Evidence** | [Foto del monitor serial / captura de pantalla OLED / log extraído / etc.] |

---

**Proyecto:** Sistema de alerta háptica para impacto excesivo en el talón  
**Tecnología:** ESP32 + Sensor FSR + OLED + Motor vibración  
**Estado:** En desarrollo / Documentación de requisitos y plan de pruebas