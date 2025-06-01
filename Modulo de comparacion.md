---


---

<h1 id="div-aligncentermodulo-de-comparaciondiv"><div align="center">Modulo de comparacion</div></h1>
<p><em>Cristian David Sotelo Nieto (134770)</em><br>
<em>Juan Contreras (134126)</em><br>
<em>Carlos Andres Estupiñan (82149)</em><br>
<em>Universidad ECCI</em><br>
<em>Microcontroladores</em><br>
<em>Jorge Eduardo Cote</em><br>
<em>31/05/2025</em></p>
<hr>
<h2 id="módulo-de-comparación--pwm--captura">Módulo de comparación / PWM / Captura</h2>
<p>En los dispositivos integrados actuales, los microcontroladores no se limitan a procesar código; también se comunican activamente con señales del entorno físico. Los módulos de captura, comparación y PWM (modulación de ancho de pulso) son elementos fundamentales que posibilitan esta comunicación. Estos componentes son cruciales para la medición de intervalos de tiempo, la creación de eventos sincronizados y la generación de señales analógicas aproximadas. En este documento se explica a fondo la operación y el ajuste de estos módulos, con ejemplos específicos para el microcontrolador PIC18F4550.</p>
<hr>
<h2 id="módulo-de-captura">Módulo de Captura</h2>
<p>El módulo de captura se utiliza para registrar el tiempo exacto en que ocurre un evento, generalmente externo, como un flanco de subida o bajada de una señal. Este evento se sincroniza con el conteo de un temporizador interno (timer), almacenando el valor actual del temporizador en un registro especial.</p>
<p><strong>Características principales:</strong></p>
<ul>
<li>Etiquetado temporal de eventos.</li>
<li>Asociación con flancos (subida/bajada) o eventos internos (como el ADC en Atmega16).</li>
<li>Permite generar una interrupción cuando ocurre el evento.</li>
<li>Granularidad determinada por el prescaler del temporizador; se recomienda uno lo más pequeño posible para mejor resolución.</li>
</ul>
<p><strong>Funcionamiento modo de captura:</strong></p>
<ul>
<li>Para llevar a cabo la captura, ya sea el Timer1 o el Timer3 se preparan para medir el valor.</li>
<li>El registro <code>T3CON</code> es clave para elegir cuál de los dos, Timer1 o 3, se usará para registrar la entrada que mencionamos.</li>
<li>Si el pin <code>CCP1</code> detecta un cambio, ya sea que suba o que baje, se activa el bit <code>CCP1IF</code>, que nos avisa de una interrupción. Para desactivarlo, hay que hacerlo por software.</li>
<li>En el momento en que hay una interrupción, el valor actual del temporizador se guarda en el registro <code>CCPR1</code> (tanto en <code>CCPR1H</code> como en <code>CCPR1L</code>); si revisamos este valor, sabremos cuándo ocurrió el evento.</li>
</ul>
<p><strong>Ejemplo:</strong><br>
Medición del período de una señal PWM externa</p>
<hr>
<h2 id="módulo-de-comparación">Módulo de Comparación</h2>
<p>El Módulo de Comparación crea un evento cuando el contador de un temporizador llega a un número que el usuario ha establecido. Puedes ajustar este evento para que sea una señal alta o baja, un cambio hacia arriba o hacia abajo, una interrupción o un cambio de estado. Además, permite reiniciar automáticamente el contador cuando llega al valor que se está comparando. Este módulo funciona con una resolución de 16 bits, que es común en microcontroladores como el PIC18F4550.</p>
<p><strong>Características principales:</strong></p>
<ul>
<li><strong>Configuración flexible de eventos:</strong> Puedes definir eventos de diferentes niveles, como alto, bajo, flanco de subida o bajada, interrupciones y cambios de estado (toggle).</li>
<li><strong>Restablecimiento automático del contador:</strong> Cuando se alcanza el valor de comparación, el módulo tiene la capacidad de reiniciar el contador de forma automática.</li>
<li><strong>Interacción con otros módulos:</strong> Se puede integrar con otros periféricos del microcontrolador, como PWM y UART, lo que permite aplicaciones avanzadas en sistemas embebidos.</li>
<li><strong>Aplicaciones en control de procesos:</strong> Es especialmente útil en sistemas de control de motores, generación de señales de sincronización y medición de tiempos en sistemas electrónicos.</li>
</ul>
<p><strong>Ejemplo aplicado:</strong><br>
Activación de una salida digital cada 5 milisegundos</p>
<hr>
<h2 id="pwm">PWM</h2>
<p>PWM es un método que emplea una onda cuadrada con una duración de pulso variable. Esta duración del pulso significa un nivel de voltaje continuo. Es crucial recordar que, a pesar de que la amplitud del pulso cambia, la velocidad de la señal debe permanecer igual.</p>
<hr>
<h3 id="ciclo-útil">Ciclo útil:</h3>
<p>El ciclo útil se define como la razón entre el tiempo que la señal permanece en nivel alto (TON) y el período total (T):</p>
<p><span class="katex--display"><span class="katex-display"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><semantics><mrow><mrow><mtext>Ciclo&nbsp;</mtext><mover accent="true"><mtext>u</mtext><mo>ˊ</mo></mover><mtext>til</mtext></mrow><mo>=</mo><mfrac><msub><mi>T</mi><mrow><mi>O</mi><mi>N</mi></mrow></msub><mi>T</mi></mfrac><mo>×</mo><mn>100</mn></mrow><annotation encoding="application/x-tex">
\text{Ciclo útil} = \frac{T_{ON}}{T} \times 100%
</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 0.69444em; vertical-align: 0em;"></span><span class="mord text"><span class="mord">Ciclo&nbsp;</span><span class="mord accent"><span class="vlist-t"><span class="vlist-r"><span class="vlist" style="height: 0.69444em;"><span class="" style="top: -3em;"><span class="pstrut" style="height: 3em;"></span><span class="mord">u</span></span><span class="" style="top: -3em;"><span class="pstrut" style="height: 3em;"></span><span class="accent-body" style="left: -0.25em;"><span class="mord">ˊ</span></span></span></span></span></span></span><span class="mord">til</span></span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right: 0.277778em;"></span></span><span class="base"><span class="strut" style="height: 2.04633em; vertical-align: -0.686em;"></span><span class="mord"><span class="mopen nulldelimiter"></span><span class="mfrac"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.36033em;"><span class="" style="top: -2.314em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord mathnormal" style="margin-right: 0.13889em;">T</span></span></span><span class="" style="top: -3.23em;"><span class="pstrut" style="height: 3em;"></span><span class="frac-line" style="border-bottom-width: 0.04em;"></span></span><span class="" style="top: -3.677em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"><span class="mord mathnormal" style="margin-right: 0.13889em;">T</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.328331em;"><span class="" style="top: -2.55em; margin-left: -0.13889em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mathnormal mtight" style="margin-right: 0.10903em;">ON</span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.686em;"><span class=""></span></span></span></span></span><span class="mclose nulldelimiter"></span></span><span class="mspace" style="margin-right: 0.222222em;"></span><span class="mbin">×</span><span class="mspace" style="margin-right: 0.222222em;"></span></span><span class="base"><span class="strut" style="height: 0.64444em; vertical-align: 0em;"></span><span class="mord">100</span></span></span></span></span></span></p>
<hr>
<h3 id="valor-promedio-de-la-señal">Valor promedio de la señal:</h3>
<p>El valor promedio de una señal PWM depende del valor del ciclo útil y el nivel lógico alto (V):</p>
<p><span class="katex--display"><span class="katex-display"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML" display="block"><semantics><mrow><mover accent="true"><msub><mi>V</mi><mrow><mi>A</mi><mi>V</mi></mrow></msub><mo stretchy="true">‾</mo></mover><mo>=</mo><mfrac><mrow><mi>A</mi><msub><mi>T</mi><mrow><mi>O</mi><mi>N</mi></mrow></msub></mrow><mi>T</mi></mfrac></mrow><annotation encoding="application/x-tex">
\overline{V_{AV}} =  \frac{AT_{ON}}{T}
</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height: 1.03333em; vertical-align: -0.15em;"></span><span class="mord overline"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.88333em;"><span class="" style="top: -3em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord"><span class="mord mathnormal" style="margin-right: 0.22222em;">V</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.328331em;"><span class="" style="top: -2.55em; margin-left: -0.22222em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mathnormal mtight">A</span><span class="mord mathnormal mtight" style="margin-right: 0.22222em;">V</span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span></span></span></span><span class="" style="top: -3.80333em;"><span class="pstrut" style="height: 3em;"></span><span class="overline-line" style="border-bottom-width: 0.04em;"></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span><span class="mspace" style="margin-right: 0.277778em;"></span><span class="mrel">=</span><span class="mspace" style="margin-right: 0.277778em;"></span></span><span class="base"><span class="strut" style="height: 2.04633em; vertical-align: -0.686em;"></span><span class="mord"><span class="mopen nulldelimiter"></span><span class="mfrac"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 1.36033em;"><span class="" style="top: -2.314em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord mathnormal" style="margin-right: 0.13889em;">T</span></span></span><span class="" style="top: -3.23em;"><span class="pstrut" style="height: 3em;"></span><span class="frac-line" style="border-bottom-width: 0.04em;"></span></span><span class="" style="top: -3.677em;"><span class="pstrut" style="height: 3em;"></span><span class="mord"><span class="mord mathnormal">A</span><span class="mord"><span class="mord mathnormal" style="margin-right: 0.13889em;">T</span><span class="msupsub"><span class="vlist-t vlist-t2"><span class="vlist-r"><span class="vlist" style="height: 0.328331em;"><span class="" style="top: -2.55em; margin-left: -0.13889em; margin-right: 0.05em;"><span class="pstrut" style="height: 2.7em;"></span><span class="sizing reset-size6 size3 mtight"><span class="mord mtight"><span class="mord mathnormal mtight" style="margin-right: 0.10903em;">ON</span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.15em;"><span class=""></span></span></span></span></span></span></span></span></span><span class="vlist-s">​</span></span><span class="vlist-r"><span class="vlist" style="height: 0.686em;"><span class=""></span></span></span></span></span><span class="mclose nulldelimiter"></span></span></span></span></span></span></span></p>
<p>Donde:</p>
<ul>
<li>( \overline{V} ) es el valor promedio.</li>
<li>( V_{MAX} ) es el voltaje máximo de la señal (típicamente 5V).</li>
<li>( T_{ON} ) es el tiempo en alto.</li>
<li>( T ) es el período total de la señal.</li>
</ul>
<hr>
<h2 id="módulo-pwm-en-microcontroladores">Módulo PWM en Microcontroladores</h2>
<p>El módulo PWM opera utilizando un temporizador como base de tiempo (frecuencia) y comparando su valor con un registro que define el tiempo de activación (TON). Cuando el temporizador alcanza el valor definido, la salida cambia de estado (por ejemplo, de alto a bajo), generando así un pulso de ancho controlado.</p>
<p><strong>Configuración:</strong></p>
<ul>
<li>Configurar el período de la señal PWM usando el registro <code>PR2</code>.</li>
<li>Definir el ciclo útil con el registro <code>CCPRxL</code>.</li>
<li>Configurar los pines de salida habilitando el pin correspondiente en <code>TRISC</code> (como salida digital).</li>
<li>Ajustar y activar el prescaler del temporizador <code>TMR2</code>.</li>
<li>Configurar el módulo <code>CCPx</code> en modo PWM (bits del registro <code>CCPxCON</code>).</li>
</ul>

