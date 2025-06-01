---


---

<h1 id="div-aligncentercomunicacion-serial-scidiv"><div align="center">Comunicacion Serial SCI</div></h1>
<p>Cristian David Sotelo Nieto (134770)<br>
Juan Contreras (134126)<br>
Carlos Andres Estupiñan (82149)<br>
Universidad Ecci<br>
Microcontroladores<br>
Jorge Eduardo Cote<br>
01/06/2025</p>
<hr>
<h2 id="comunicación-serial-sci">Comunicación Serial SCI</h2>
<p>La comunicación serial es un método fundamental para intercambiar datos entre dispositivos electrónicos. En el caso de los microcontroladores, se implementa mediante diversos protocolos que determinan la forma en que los bits son transmitidos y recibidos.</p>
<h3 id="tipos-de-comunicación">Tipos de Comunicación</h3>
<p>Hay dos maneras principales de transmitir información:</p>
<ul>
<li><strong>Serial</strong>: En este método, se envía un bit a la vez a través de un canal de comunicación, lo que crea una cola de espera en el puerto. Es más eficiente en cuanto a cableado.</li>
<li><strong>Paralela</strong>: Aquí, se transmiten varios bits al mismo tiempo, lo que requiere más recursos físicos.</li>
</ul>
<h3 id="modos-de-transmisión">Modos de transmisión</h3>
<ul>
<li><strong>Simplex</strong>: Los datos fluyen en una sola dirección, desde un transmisor a un receptor. Un ejemplo es una transmisión de radio tradicional.</li>
<li><strong>Half-Duplex</strong>: Los datos pueden transmitirse en ambas direcciones, pero no simultáneamente. Los dispositivos deben negociar el acceso al medio. Los walkie-talkies son un ejemplo de esto.</li>
<li><strong>Full-Duplex</strong>: Los datos pueden transmitirse y recibirse simultáneamente. Una conversación telefónica es un ejemplo de comunicación full-duplex.</li>
</ul>
<h3 id="tipos-de-comunicación-serial">Tipos de Comunicación Serial</h3>
<p>Dependiendo de la sincronización entre los dispositivos, se clasifica en:</p>
<ul>
<li><strong>Síncrona</strong>: Requiere una señal de reloj compartida entre el emisor y el receptor, lo que permite una transmisión más rápida y confiable.</li>
<li><strong>Asíncrona</strong>: No usa una señal de reloj compartida; en cambio, depende de una tasa de baudios fija que el receptor debe conocer para interpretar correctamente los datos.</li>
</ul>
<h3 id="topologías-de-comunicación">Topologías de Comunicación</h3>
<ul>
<li><strong>Bus</strong>: Esta configuración facilita la conexión de varios aparatos a un mismo medio de comunicación. Por esta razón, es imprescindible contar con un método de asignación de direcciones que haga posible saber qué aparato envía datos y a quién van dirigidos. Si bien esta estructura es muy útil en redes con muchos nodos, su eficacia puede disminuir si demasiados aparatos tratan de hablar a la vez, lo cual causa choques y demoras en el envío.</li>
<li><strong>Punto a punto</strong>: Solo dos dispositivos se comunican entre sí, sin necesidad de direcciones.</li>
</ul>
<h3 id="arquitectura-de-comunicación">Arquitectura de comunicación</h3>
<ul>
<li><strong>Maestro-esclavo</strong>: El maestro inicia la comunicación y controla el reloj. El esclavo responde a las peticiones del maestro según su dirección.</li>
<li><strong>Todos iguales</strong>: Cualquier dispositivo puede iniciar la comunicación si el medio está libre.</li>
</ul>
<h3 id="condiciones-eléctricas">Condiciones eléctricas</h3>
<ul>
<li><strong>Terminación sencilla (Single-ended)</strong>: Las tierras de los hilos que transmiten los datos son comunes. En grandes distancias, pueden verse afectadas por ruido.</li>
<li><strong>Diferencial</strong>: La señal transmitida utiliza la diferencia de potencial entre dos hilos. Tiene un mejor comportamiento en grandes distancias, ya que, si el ruido afecta, los dos hilos se afectan por igual y la diferencia tiene poca afectación.</li>
</ul>
<h3 id="protocolos-en-los-microcontroladores">Protocolos en los microcontroladores</h3>
<ul>
<li><strong>SCI (UART)</strong>: Serial Communication Interface, protocolos de comunicación serial asíncrona. UART es el acrónimo de Universal Asynchronous Receiver Transmitter que implementa incoherentemente la comunicación como en dos hilos (uno para transmisión - TX - y el otro para recepción - RX -), donde cada uno de los lados tiene su generador de reloj propio.</li>
<li><strong>SPI</strong>: Serial Peripheral Interface, que implementa la comunicación serial síncrona.</li>
<li><strong>I2C</strong>: Inter-Integrated Circuit, bus síncrono que aplica una arquitectura maestro-esclavo.</li>
<li><strong>USB</strong>: Universal Serial Bus.</li>
</ul>
<h3 id="uart-universal-asynchronous-receiver-transmitter">UART (Universal Asynchronous Receiver Transmitter)</h3>
<p>El módulo UART permite la configuración de varios parámetros:</p>
<ul>
<li><strong>Bits de Datos</strong>: El número de bits de datos puede seleccionarse (por ejemplo, de 5 a 9 bits en un microcontrolador Atmega).</li>
<li><strong>Bit de paridad</strong>: Un bit opcional que se añade a la trama de transmisión para la detección de errores de comunicación. Su valor depende de la cantidad de '1’s que tenga la trama. Las opciones comunes de paridad son par, impar o sin paridad.</li>
<li><strong>Bit de parada</strong>: Se puede seleccionar si se desea 1 o 2 bits de parada.</li>
<li><strong>Tasa de Baudios</strong>: Se configura la velocidad de transmisión (símbolos por segundo) mediante un registro.</li>
</ul>
<p>La comunicación serial asíncrona se puede expresar en el formato <code>D[E|O|N]S</code>, donde <code>D</code> es el número de bits de datos y <code>S</code> es el número de bits de parada. <code>E</code>, <code>O</code> o <code>N</code> representan paridad par, impar o sin paridad, respectivamente. Por ejemplo, <code>8E1</code> significa 8 bits de datos, paridad par y 1 bit de parada.</p>
<h3 id="eusart-enhanced-universal-synchronous-asynchronous-receiver-transmitter">EUSART (Enhanced Universal Synchronous Asynchronous Receiver Transmitter)</h3>
<p>El módulo EUSART, también conocido como la interfaz de comunicación serial SCI, extiende la funcionalidad de UART utilizando un modo síncrono de transmisión.</p>
<ul>
<li>
<p><strong>Modo Síncrono</strong>: Utiliza una línea adicional para usar la misma señal de reloj entre los dos dispositivos, lo que resulta en una transmisión más rápida que su modo asíncrono. Se puede configurar como maestro o esclavo, con polaridad de reloj configurable.</p>
</li>
<li>
<p><strong>Modo Asíncrono</strong>: El hilo de la señal de reloj se deja libre. Ofrece características como restablecimiento por caída de señal, autocalibración de la tasa de baudios y transmisión de carácter de ruptura de 12 bits (1 bit de inicio, 10 bits de datos y 1 bit de parada).</p>
</li>
</ul>
<p>El EUSART puede configurarse en modo full-duplex para comunicarse con otros microcontroladores o PCs, o en modo half-duplex para comunicarse con circuitos integrados como conversores A/D, D/A o memorias EEPROM.</p>
<h3 id="pines-de-comunicación">Pines de comunicación</h3>
<p>Los pines de EUSART están multiplexados con el <strong>Puerto C</strong> del microcontrolador (por ejemplo, en el PIC):</p>
<ul>
<li><code>RC6</code> → <strong>TX</strong> (Transmisión)</li>
<li><code>RC7</code> → <strong>RX</strong> (Recepción)</li>
</ul>
<h3 id="registros-involucrados">Registros involucrados</h3>
<ul>
<li><code>TRISC</code>: Configura los pines como entrada o salida.</li>
<li><code>TXSTA</code>: Controla la configuración del transmisor.</li>
<li><code>RCSTA</code>: Controla la configuración del receptor.</li>
<li><code>BAUDCON</code>: Configura detalles de la comunicación (modo de 8 o 16 bits, polaridad, velocidad).</li>
<li><code>SPBRG</code>: Define la tasa de baudios.</li>
</ul>
<h3 id="configuración-típica-de-comunicación-asíncrona">Configuración típica de comunicación asíncrona</h3>
<pre class=" language-c"><code class="prism  language-c"><span class="token comment">// Configurar TX (RC6) como salida y RX (RC7) como entrada</span>
TRISC6 <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>  <span class="token comment">// Salida para TX</span>
TRISC7 <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>  <span class="token comment">// Entrada para RX</span>

<span class="token comment">// Configurar la tasa de baudios</span>
SPBRG <span class="token operator">=</span> <span class="token number">25</span><span class="token punctuation">;</span> <span class="token comment">// Para 9600 baudios con Fosc = 4MHz</span>

<span class="token comment">// Configurar TXSTA</span>
TXSTAbits<span class="token punctuation">.</span>SYNC <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>   <span class="token comment">// Modo asíncrono</span>
TXSTAbits<span class="token punctuation">.</span>BRGH <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>   <span class="token comment">// Alta velocidad</span>
TXSTAbits<span class="token punctuation">.</span>TXEN <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>   <span class="token comment">// Habilita transmisor</span>

<span class="token comment">// Configurar RCSTA</span>
RCSTAbits<span class="token punctuation">.</span>SPEN <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>   <span class="token comment">// Habilita el puerto serial (RX/ TX)</span>
RCSTAbits<span class="token punctuation">.</span>CREN <span class="token operator">=</span> <span class="token number">1</span><span class="token punctuation">;</span>   <span class="token comment">// Habilita la recepción continua</span>

<span class="token comment">// Configurar BAUDCON (opcional, depende del microcontrolador)</span>
BAUDCONbits<span class="token punctuation">.</span>BRG16 <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span> <span class="token comment">// Usar generador de baudios de 8 bits</span>
<span class="token punctuation">]</span>

Cálculo de <span class="token function">SPBRG</span> <span class="token punctuation">(</span>Tasa de baudios<span class="token punctuation">)</span>
SPBRG <span class="token operator">=</span> <span class="token punctuation">(</span>Fosc <span class="token operator">/</span> <span class="token punctuation">(</span><span class="token number">16</span> <span class="token operator">*</span> BaudRate<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token operator">-</span> <span class="token number">1</span>
</code></pre>

