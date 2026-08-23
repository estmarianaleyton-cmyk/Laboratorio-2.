# Laboratorio-2

**Universidad Militar Nueva Granada**

**Asignatura:** Instrumentación biomedica y biosensores

**Estudiantes:** Dubrasca Martínez, Mariana Leyton, Joshara Valentina Palacios

**Fecha:** 20 de agosto del 2026

**Título de la práctica:** Estimación del nivel de estrés basada en la respuesta galvánica cutánea (GSR)

# **Introducción**
La respuesta galvánica de la piel (GSR) es una señal fisiológica relacionada con los cambios en la conductancia eléctrica de la piel, los cuales pueden variar ante diferentes estímulos y actividades realizadas por una persona. Su medición permite observar cambios en la actividad de las glándulas sudoríparas y, por lo tanto, analizar la respuesta del organismo ante distintas condiciones.

En este laboratorio se desarrolló inicialmente un circuito de adquisición de señal mediante electrodos para medir la respuesta galvánica de la piel. Posteriormente, con el propósito de implementar un sistema de adquisición inalámbrico, se incorporó una ESP32 para realizar la lectura y transmisión de los datos. Se realizaron diferentes pruebas mientras se desarrollaban actividades variadas, con el fin de observar cómo estas condiciones producían cambios en la señal obtenida.
# **Metodología**
## ***Diseño del circuito de acondicionamiento***
Se diseñó un circuito divisor de voltaje para transducir las variaciones de resistencia de la piel, asociadas a la actividad electrodérmica en una señal de voltaje medible. El circuito se conformó por un par de electrodos Ag-AgCl, que actúan como una resistencia variable dependiente de la conductancia cutánea del sujeto, en serie con una resistencia fija de 68 kΩ. En paralelo a la salida del divisor se conectó un capacitor cerámico de 1 µF, formando un filtro pasabajos RC cuya frecuencia de corte teórica es:

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Frecuencia%20de%20corte.png>

***Circuito***

<img width="449" height="468" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/circuito1.jpeg>  <img width="449" height="468" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/circuito2.jpeg>

Este filtro se incluyó para atenuar componentes de alta frecuencia como ruido eléctrico e interferencia de red sin afectar la componente de interés, dado que las variaciones fisiológicas de la GSR ocurren en escalas de tiempo del orden de segundos.

## ***Revisión normativa de la seguridad eléctrica***
Siguiendo el numeral A.2 de la guía, se consultó la norma IEC 60479 ("Effects of current on human beings and livestock"), que establece los umbrales fisiológicos de corriente eléctrica sobre el cuerpo humano según su intensidad y tipo, alterna o continua. La Tabla 1 resume dichos efectos:

<img width="649" height="568" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Tabla.png>

Es importante aclarar que esta tabla de efectos fisiológicos, ampliamente citada en literatura de seguridad eléctrica, proviene originalmente de la norma IEC 60479. De forma complementaria, la norma IEC 60601-1 ("Medical electrical equipment — General requirements for basic safety and essential performance") constituye el marco normativo de seguridad para el diseño de equipos electromédicos. Ambas normas se consideraron en el diseño del presente circuito: IEC 60479 como base de los umbrales de corriente segura e IEC 60601-1 como referencia general de buenas prácticas de seguridad eléctrica aplicables a dispositivos de uso biomédico.

## ***Cálculo de corriente máxima***
Siguiendo estos criterios de seguridad, y considerando el caso extremo en el que la resistencia de la piel del sujeto tiende a cero (R_skin = 0 Ω, cortocircuito), se verificó que la corriente circulante no superara 1 mA (umbral de percepción según la Tabla 1, tomado como margen conservador de seguridad). Con una alimentación de 3.3 VDC, voltaje suministrado por el pin 3V3 de la ESP32 y la resistencia fija de 68 kΩ como única limitante en el caso extremo:

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Corriente%20maxima.png>

Este valor es considerablemente inferior al límite de 1 mA establecido, e incluso se ubica por debajo del umbral de percepción (0-4 mA en corriente continua según la Tabla 1), confirmando que el diseño cumple ampliamente con los márgenes de seguridad requeridos para uso en contacto directo con piel humana.

## ***Diseño vestible e inalámbrico***
Para cumplir con el requisito de transmisión inalámbrica y portabilidad, se reemplazó el DAQ por un microcontrolador ESP32 placa DevKit V1, que integra un conversor analógico-digital (ADC) de 12 bits y un módulo de comunicación Bluetooth clásico (SPP), eliminando la necesidad de un DAQ y un cable de datos permanente.
La salida del divisor de voltaje se conectó al pin GPIO34 de la ESP32 (canal ADC1), seleccionado por no compartir hardware con el subsistema WiFi/Bluetooth (a diferencia de los pines ADC2), evitando así interferencia en las lecturas durante la transmisión inalámbrica. La alimentación del divisor se tomó del pin 3V3 de la propia ESP32, garantizando una referencia de tierra común entre el circuito de acondicionamiento y el microcontrolador.
Para las pruebas de movilidad, la ESP32 se alimentó mediante una batería portátil de 5V/1000mA conectada por USB, permitiendo la operación del dispositivo de forma autónoma y sin restricción de cable durante el desplazamiento del sujeto de prueba.

## ***Recepción, vizualización de la señal y definición de umbrales***
La recepción de los datos se implementó en MATLAB mediante un script que se conecta al puerto COM virtual generado por el emparejamiento Bluetooth (SPP) entre el computador y la ESP32. El script realiza lectura continua por el objeto "serialport", actualiza una gráfica en tiempo real y clasifica el nivel de estrés percibido según el voltaje medido.

Siguiendo el procedimiento de la guía, se solicitó al sujeto de prueba, en reposo y cómodamente sentado, realizar una inspiración profunda seguida de una exhalación lenta, registrando el valor máximo y mínimo de la señal GSR observados durante la respuesta. Durante esta fase se identifico que el valor basal de la señal no es estable inmediatamente después de colocar los electrodos, sino que presenta una tendencia decreciente/creciente gradual durante los primeros minutos, atribuible a la hidratación progresiva de la piel bajo el electrodo. Por este motivo, se estableció un período de estabilización previo a la toma del basal de referencia de 3-5 minutos.

Con base en esto, se definieron los umbrales de clasificación (poco estrés, estrés moderado, estrés elevado) como una fracción de la amplitud para el sujeto, en lugar de valores absolutos universales. Los valor obtenidos fueron: minimo de 1.1 V y el maximo de 1.4 V, en base a estos se realizo el cálculo de la amplitud y asi mismo con los umbrales.

<img width="549" height="268" alt="image" src=https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/umbrales.png >

# **Resultados**
## ***Reposo***
<img width="679" height="688" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/reposo%202.0.png>
## ***Lectura***
<img width="679" height="688" alt="image" src="https://github.com/user-attachments/assets/0dbed7b1-88a2-4d26-907a-e9a68660881c" />

# **Análisis de resultados**

# **Discusión**

- ¿A qué se debe que una inspiración profunda incremente la magnitud de la respuesta galvánica cutánea (GSR)?

La respuesta galvánica cutánea está mediada por el sistema nervioso autónomo simpático, específicamente por la activación de las glándulas sudoríparas ecrinas, que son inervadas casi exclusivamente por fibras simpáticas colinérgicas.

- ¿Cuáles serían las ventajas y desventajas de utilizar la GSR como indicador de estrés?

**Ventajas**
- Es no invasiva y de bajo costo, ya que solo requiere un par de electrodos superficiales y un circuito de acondicionamiento simple, sin necesidad de reactivos, agujas, o equipos costosos.
- Tiene una buena resolución temporal, lo cual permite detectar respuestas fisiológicas rápidas ante estímulos discretos, siendo útil en estudios de decisión, atención y respuesta a estímulos puntuales.
- Factible de implementar en dispositivos vestibles. Es posible construir un sistema portátil, inalámbrico y de bajo consumo energético para monitoreo continuo, algo mucho más difícil de lograr con técnicas como EEG o resonancia magnética funcional.

**Desventajas**
- Tiene sensibilidad a artefactos de movimiento y contacto. El movimiento del sujeto (caminar, escribir) puede alterar la presión y el área de contacto de los electrodos con la piel, generando cambios de señal que no corresponden a una verdadera activación fisiológica, sino a variaciones de impedancia por contacto mecánico.
- Dependencia de factores ambientales. La temperatura y humedad ambiente afectan directamente la tasa de sudoración basal, introduciendo una fuente de variabilidad que no está relacionada con el estado emocional o cognitivo del sujeto.
- Alta variabilidad entre individuos. El nivel basal de conductancia varía considerablemente entre sujetos, ya sea por hidratación, grosor de piel, temperatura corporal y edad o tambien en el mismo sujeto a lo largo del tiempo. Esto obliga a una calibración individual y por sesión, limitando su uso como medida absoluta y universal.

# **Conclusiones**

# **Referencias**













