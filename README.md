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

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Frecuencia%20de%20corte.png >

***Circuito***

<img width="449" height="168" alt="image" src= https://github.com/estmarianaleyton-cmyk/Laboratorio-2./blob/main/Frecuencia%20de%20corte.png >

Este filtro se incluyó para atenuar componentes de alta frecuencia como ruido eléctrico e interferencia de red sin afectar la componente de interés, dado que las variaciones fisiológicas de la GSR ocurren en escalas de tiempo del orden de segundos.

## ***Revisión normativa de la seguridad eléctrica***
Siguiendo el numeral A.2 de la guía, se consultó la norma IEC 60479 ("Effects of current on human beings and livestock"), que establece los umbrales fisiológicos de corriente eléctrica sobre el cuerpo humano según su intensidad y tipo, alterna o continua. La Tabla 1 resume dichos efectos:






