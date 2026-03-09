## Primera entrega del proyecto de Sistemas Embebidos

## Roles

### Technical Leader
Ana José Parra Palacio
### Firmware Engineer
Maria Paulina Corrales Suaza
### Hardware Integration Engineer
Esteban Arenas
### Verification & Testing Engineer
...

## Proyecto de Sistemas Embebidos 
### Smart Insole – Sistema para Análisis de Pisada y Prevención de Lesiones en Corredores

### Introducción
El running es una de las actividades deportivas más populares a nivel mundial, pero también una de las que mayor riesgo de lesiones presenta, especialmente en corredores recreativos y de alto rendimiento (van Gent et al., 2007). Estas lesiones, principalmente de tipo por sobrecarga, afectan significativamente la continuidad deportiva, la calidad de vida y generan costos médicos y económicos considerables (van Poppel et al., 2014). 

En el contexto de la biomecánica deportiva, el monitoreo continuo y en tiempo real de la pisada emerge como una estrategia prometedora para la prevención temprana de lesiones, permitiendo detectar patrones de riesgo y corregirlos antes de que se conviertan en patologías crónicas (Lee et al., 2025; Nigg et al., 2015).

Sin embargo, los sistemas actuales de plantillas inteligentes presentan limitaciones en términos de autonomía, robustez ante fallos, integración de logging estructurado y retroalimentación inmediata en entornos reales de entrenamiento.

El proyecto "Smart Insole" busca abordar esta brecha mediante el desarrollo de un sistema embebido integrado en una plantilla inteligente, que combine sensado preciso de presión plantar, activación háptica correctiva, registro de eventos con trazabilidad temporal y de errores, comunicación inalámbrica eficiente e interfaz de usuario intuitiva.

El enfoque se basa en buenas prácticas de ingeniería embebida, incluyendo trazabilidad de requisitos, manejo robusto de fallos, verificación funcional y documentación técnica formal, simulando un proceso cercano al entorno industrial.

### Formulación del problema
Las lesiones por sobrecarga en corredores representan una carga significativa para la salud deportiva y la atención médica. Revisiones sistemáticas reportan incidencias anuales entre el 30 % y el 79 % en corredores de larga distancia, con valores promedio cercanos al 50 % en poblaciones recreativas (van Gent et al., 2007; van Poppel et al., 2014).

Las patologías más frecuentes incluyen fascitis plantar, tendinopatías de Aquiles, periostitis tibial y fracturas por estrés, todas asociadas a mecanismos biomecánicos repetitivos como impactos excesivos (>3–4 g), distribución desigual de presión plantar, asimetría izquierda/derecha, sobreuso y factores externos (calzado inadecuado, superficies irregulares) (Nigg et al., 2015). 

La detección temprana de estos desequilibrios requiere monitoreo continuo de la pisada durante la actividad real, algo que los métodos tradicionales (análisis de laboratorio con plataformas de presión) no permiten. Sistemas wearables in-shoe con sensores de presión han demostrado utilidad para identificar patrones de riesgo y proporcionar retroalimentación inmediata, pero los dispositivos existentes presentan limitaciones críticas:

- Falta de manejo robusto de fallos (desconexión o saturación de sensores).
- Ausencia de logging estructurado con niveles de severidad y códigos de error.
- Dependencia excesiva de conexión constante a dispositivos externos sin autonomía local.
- Rigidez estructural que compromete la comodidad en uso prolongado.
- Retroalimentación limitada (visual o sonora) que no siempre es práctica durante la carrera (Subramaniam et al., 2022; Van Hooren et al., 2019).

En este escenario, surge la necesidad de un sistema embebido autónomo, robusto y de bajo costo que integre sensado preciso de presión plantar, activación correctiva háptica, registro de eventos con trazabilidad temporal y de errores, comunicación inalámbrica eficiente e interfaz de usuario intuitiva, todo ensamblado en una implementación física confiable y documentada bajo buenas prácticas de ingeniería embebida. El proyecto "Smart Insole" se propone resolver esta brecha, ofreciendo un prototipo funcional para prevención de lesiones por sobrecarga en corredores.

### Alcance del proyecto
El proyecto "Smart Insole" abarca el diseño, implementación y verificación funcional de un sistema embebido basado en ESP32 programado en C (usando ESP-IDF), integrado en una plantilla inteligente para monitoreo de presión plantar en corredores, con el objetivo de prevenir lesiones por sobrecarga mediante retroalimentación háptica en tiempo real.

### Alcance incluido
- Definición formal del problema biomecánico y especificación de requisitos funcionales y no funcionales, con matriz de trazabilidad (SRTM) para garantizar alineación entre requisitos, diseño, implementación y pruebas.
- Sensado de presión plantar en al menos un punto anatómico clave (talón) utilizando sensor FSR, con amplificación adecuada y manejo robusto de fallos (desconexión, saturación, valores fuera de rango).
- Activación de retroalimentación correctiva mediante actuador háptico (motor de vibración) controlado por lógica de firmware basada en umbrales biomecánicos predefinidos.
- Implementación de un módulo de logging estructurado con timestamp, niveles de severidad (INFO, WARNING, ERROR) y códigos de error específicos, para registro de eventos relevantes durante la actividad.
- Comunicación bidireccional:
    - I2C para integración interna del sensor → para conectar el sensor FSR dentro de la plantilla.
    - Justificación
        - Bajo consumo de energía → Usa muy poca corriente, ideal para un dispositivo con batería que debe durar horas caminando o corriendo.
        - Alta velocidad en distancias cortas → Puede ir hasta 400 kHz o más, pero en distancias de 10–20 cm (dentro de la plantilla) funciona perfecto y sin errores.
        - Ideal para sensores cercanos → En una plantilla no se necesitan cables largos, así que I2C es la opción más limpia, eficiente y con menos cables que cualquier otro protocolo.
    - UART para transmisión de datos a interfaz serial (justificación: simplicidad, bajo costo, facilidad de depuración en prototipo estudiantil con ESP-IDF, baudrate 115200 para alta velocidad sin perder compatibilidad estándar).
- Interfaz de usuario: pantalla OLED local para feedback inmediato (pantalla pequeña que muestra información directamente en la plantilla) e interfaz serial básica (Monitor Serial o terminal en computador) para visualización de logs, estado y errores.
- Implementación física en tarjeta universal soldada, con diagrama esquemático documentado, carcasa funcional resistente al sudor y peso corporal, alimentada por batería externa (LiPo 3.7V compatible con ESP32).
- Diseño arquitectónico completo: diagrama de bloques, arquitectura de firmware, justificación de arquitectura, diagrama de estados del firmware, estrategia de manejo de errores y documentación técnica formal (Embedded Firmware Design Document).
- Verificación y validación mediante casos de prueba funcionales y no funcionales, demostración en laboratorio y trazabilidad completa (SRTM).

### Alcance excluido
- No incluye diseño y fabricación de PCB personalizada.
- No contempla pruebas clínicas, in vivo ni preclínicas.
- No integra aprendizaje automático ni procesamiento avanzado de señales (solo umbrales simples).
- No incluye optimización extrema de consumo para uso >12 horas ni certificación médica.

### Objetivos del Proyecto

Objetivo General
- Desarrollar un sistema embebido basado en ESP32 programado en C (usando ESP-IDF), integrado en una plantilla inteligente ("Smart Insole") para el monitoreo en tiempo real de la presión plantar en corredores, con capacidad de detección de patrones de riesgo biomecánico, retroalimentación háptica correctiva (avisarte algo con vibraciones o toques), registro de eventos con trazabilidad temporal y de errores, comunicación serial eficiente e interfaz de usuario intuitiva, aplicando buenas prácticas de ingeniería embebida para la prevención de lesiones por sobrecarga.

Objetivos Específicos
1.  Definir formalmente el problema biomecánico y especificar los requisitos funcionales y no funcionales del sistema, construyendo una matriz de trazabilidad de requerimientos (SRTM) que garantice alineación completa entre requisitos, diseño, implementación y pruebas.
2.  Diseñar e implementar la etapa de sensado de presión plantar utilizando un sensor FSR en el talón, con amplificación adecuada y mecanismos robustos de detección y manejo de fallos.
3.  Integrar un actuador mecánico háptico (motor de vibración) controlado electrónicamente mediante lógica de firmware basada en umbrales biomecánicos predefinidos.
4.  Desarrollar un módulo de logging estructurado que registre eventos relevantes con timestamp, niveles de severidad y códigos de error específicos.
5.  Implementar un sistema de comunicaciones eficiente compuesto por I2C para integración interna del sensor y UART para transmisión de datos a interfaz serial, justificando técnicamente la selección de protocolos.
6. Crear una interfaz de usuario compuesta por pantalla OLED local para feedback inmediato y interfaz serial básica para visualización de logs, estado y errores.
7. Ensamblar el prototipo en tarjeta universal soldada, con carcasa funcional, fuente de alimentación externa y documentación técnica completa.
8.  Verificar y validar el sistema mediante casos de prueba funcionales y no funcionales, demostrando cumplimiento de requisitos y presentando resultados en una demostración final.

### Referencias
- Van Poppel, D., Scholten‐Peeters, G. G. M., Van Middelkoop, M., & Verhagen, A. P. (2014). Prevalence, incidence and course of lower extremity injuries in runners during a 12‐month follow‐up period. Scandinavian Journal Of Medicine And Science In Sports, 24(6), 943-949. https://doi.org/10.1111/sms.12110
- Van Gent, R. N., Siem, D., Van Middelkoop, M., Van Os, A. G., Bierma-Zeinstra, S. M. A., & Koes, B. W. (2007). Incidence and determinants of lower extremity running injuries in long distance runners: a systematic review. British Journal Of Sports Medicine, 41(8), 469-480. https://doi.org/10.1136/bjsm.2006.033548
- Nigg, B., Baltich, J., Hoerzer, S., & Enders, H. (2015). Running shoes and running injuries: mythbusting and a proposal for two new paradigms: ‘preferred movement path’ and ‘comfort filter’. British Journal Of Sports Medicine, 49(20), 1290-1294. https://doi.org/10.1136/bjsports-2015-095054
- Lee, J., Lee, J., Lee, Y. J., Kim, H., Kwon, Y., Huang, Y., Kuczajda, M., Soltis, I., & Yeo, W. (2025). Flexible Smart Insole and Plantar Pressure Monitoring Using Screen-Printed Nanomaterials and Piezoresistive Sensors. ACS Applied Materials & Interfaces, 17(33), 47153-47161. https://doi.org/10.1021/acsami.5c08296 
- Subramaniam, S., Majumder, S., Faisal, A. I., & Deen, M. J. (2022). Insole-Based Systems for Health Monitoring: Current Solutions and Research Challenges. Sensors, 22(2), 438. https://doi.org/10.3390/s22020438 
- Van Hooren, B., Goudsmit, J., Restrepo, J., & Vos, S. (2019). Real-time feedback by wearables in running: Current approaches, challenges and suggestions for improvements. Journal Of Sports Sciences, 38(2), 214-230. https://doi.org/10.1080/02640414.2019.1690960 

