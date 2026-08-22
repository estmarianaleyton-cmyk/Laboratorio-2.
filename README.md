# Laboratorio-2

**Universidad Militar Nueva Granada**

**Asignatura:** Instrumentación biomedica y biosensores

**Estudiantes:** Dubrasca Martínez, Mariana Leyton, Joshara Valentina Palacios

**Fecha:** 20 de agosto del 2026

**Título de la práctica:** Estimación del nivel de estrés basada en la respuesta galvánica cutánea (GSR)

# **Introducción**

# **Metodología**
## ***Diseño del circuito de acondicionamiento***
Se diseñó un circuito divisor de voltaje para transducir las variaciones de resistencia de la piel, asociadas a la actividad electrodérmica en una señal de voltaje medible. El circuito se conformó por un par de electrodos Ag-AgCl, que actúan como una resistencia variable dependiente de la conductancia cutánea del sujeto, en serie con una resistencia fija de 68 kΩ. En paralelo a la salida del divisor se conectó un capacitor cerámico de 1 µF, formando un filtro pasabajos RC cuya frecuencia de corte teórica es:

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Frecuencia%20de%20corte.png>

***Circuito***

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Frecuencia%20de%20corte.png>

Este filtro se incluyó para atenuar componentes de alta frecuencia como ruido eléctrico e interferencia de red sin afectar la componente de interés, dado que las variaciones fisiológicas de la GSR ocurren en escalas de tiempo del orden de segundos.

## ***Revisión normativa de la seguridad eléctrica***
Siguiendo el numeral A.2 de la guía, se consultó la norma IEC 60479 ("Effects of current on human beings and livestock"), que establece los umbrales fisiológicos de corriente eléctrica sobre el cuerpo humano según su intensidad y tipo, alterna o continua. La Tabla 1 resume dichos efectos:

<img width="649" height="568" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Tabla.png>

Es importante aclarar que esta tabla de efectos fisiológicos, ampliamente citada en literatura de seguridad eléctrica, proviene originalmente de la norma IEC 60479. De forma complementaria, la norma IEC 60601-1 ("Medical electrical equipment — General requirements for basic safety and essential performance") constituye el marco normativo de seguridad para el diseño de equipos electromédicos. Ambas normas se consideraron en el diseño del presente circuito: IEC 60479 como base de los umbrales de corriente segura e IEC 60601-1 como referencia general de buenas prácticas de seguridad eléctrica aplicables a dispositivos de uso biomédico.

## ***Cálculo de corriente máxima***
Siguiendo estos criterios de seguridad, y considerando el caso extremo en el que la resistencia de la piel del sujeto tiende a cero (R_skin = 0 Ω, cortocircuito), se verificó que la corriente circulante no superara 1 mA (umbral de percepción según la Tabla 1, tomado como margen conservador de seguridad). Con una alimentación de 3.3 VDC, voltaje suministrado por el pin 3V3 de la ESP32 y la resistencia fija de 68 kΩ como única limitante en el caso extremo:

<img width="449" height="168" alt="image" src=  >






