---


---

<h1 id="div-aligncenterinterrupcionesdiv"><div align="center">Interrupciones</div></h1>
<p>Cristian David Sotelo Nieto (134770)<br>
Juan Contreras (134126)<br>
Carlos Andres Estupiñan (82149)<br>
Universidad Ecci<br>
Microcontroladores<br>
Jorge Eduardo Cote<br>
01/06/2025</p>
<hr>
<h1 id="comunicación-i2c">Comunicación I2C</h1>
<h2 id="introducción-al-protocolo-i2c">1. Introducción al protocolo I2C</h2>
<p>El protocolo <em>I2C (Inter-Integrated Circuit)</em> es un estándar de comunicación <strong>serial, síncrona y half-duplex</strong>, diseñado por <strong>Philips</strong> (actualmente NXP Semiconductors) en 1980. Su principal objetivo es permitir la interconexión eficiente de <strong>múltiples dispositivos electrónicos</strong> (microcontroladores, sensores, memorias EEPROM, DACs, etc.) utilizando únicamente <strong>dos líneas de comunicación</strong>.</p>
<p><strong>Ventajas:</strong></p>
<ul>
<li>Uso eficiente del cableado (solo dos líneas).</li>
<li>Permite múltiples dispositivos esclavos y múltiples maestros.</li>
<li>Protocolo bien estandarizado y ampliamente soportado.</li>
</ul>
<p><strong>Desventajas:</strong></p>
<ul>
<li>Velocidad moderada comparada con SPI o UART.</li>
<li>Requiere resistencias externas (pull-up).</li>
<li>No apto para largas distancias.</li>
</ul>
<hr>
<h2 id="arquitectura-y-líneas-de-comunicación">2. Arquitectura y líneas de comunicación</h2>
<p>El protocolo I2C sigue una arquitectura <strong>maestro-esclavo</strong>, donde:</p>
<ul>
<li>El <strong>maestro</strong> inicia la comunicación y controla el reloj.</li>
<li>El <strong>esclavo</strong> responde a las peticiones del maestro según su dirección.</li>
</ul>
<h3 id="líneas-utilizadas">Líneas utilizadas:</h3>
<ul>
<li><strong>SCL (Serial Clock Line)</strong>: línea de reloj, generada por el maestro.</li>
<li><strong>SDA (Serial Data Line)</strong>: línea de datos bidireccional.</li>
</ul>
<p>Ambas líneas requieren <strong>resistencias pull-up</strong>, típicamente de 4.7 kΩ o 10 kΩ.</p>
<hr>
<h2 id="modos-de-operación-y-velocidades">3. Modos de operación y velocidades</h2>

<table>
<thead>
<tr>
<th>Modo</th>
<th>Velocidad máxima</th>
<th>Características adicionales</th>
</tr>
</thead>
<tbody>
<tr>
<td>Standard Mode</td>
<td>100 kbit/s</td>
<td>Modo básico</td>
</tr>
<tr>
<td>Fast Mode</td>
<td>400 kbit/s</td>
<td>Compatible con mayoría de dispositivos modernos</td>
</tr>
<tr>
<td>Fast Mode Plus</td>
<td>1 Mbit/s</td>
<td>Mayor velocidad para sensores avanzados</td>
</tr>
<tr>
<td>High-Speed Mode</td>
<td>3.4 Mbit/s</td>
<td>Requiere handshaking especial</td>
</tr>
<tr>
<td>Ultra-Fast Mode</td>
<td>5.0 Mbit/s (solo TX)</td>
<td>Solo transmisión; sin capacidad de lectura</td>
</tr>
</tbody>
</table><ul>
<li>Se pueden mezclar dispositivos de distintas velocidades.</li>
<li>Incluye <strong>arbitraje</strong> para manejar conflictos entre múltiples maestros.</li>
</ul>
<hr>
<h2 id="dirección-de-dispositivos">4. Dirección de dispositivos</h2>
<p>Cada dispositivo conectado al bus I2C posee una dirección única:</p>
<ul>
<li><strong>Dirección de 7 bits</strong>: hasta 128 dispositivos (algunos valores están reservados).</li>
<li><strong>Dirección de 10 bits</strong>: permite más dispositivos en buses grandes.</li>
</ul>
<h3 id="ejemplo-de-dirección-de-7-bits">Ejemplo de dirección de 7 bits:</h3>

<table>
<thead>
<tr>
<th>Dirección (binaria)</th>
<th>Bit R/W</th>
<th>Función</th>
</tr>
</thead>
<tbody>
<tr>
<td><code>1010000</code></td>
<td><code>0</code></td>
<td>Escritura a EEPROM</td>
</tr>
<tr>
<td><code>1010000</code></td>
<td><code>1</code></td>
<td>Lectura de EEPROM</td>
</tr>
</tbody>
</table><blockquote>
<p>Algunos dispositivos permiten configurar los bits menos significativos mediante pines A0, A1, A2.</p>
</blockquote>
<hr>
<h2 id="transmisión-de-datos">5. Transmisión de datos</h2>
<p>La transmisión en I2C sigue una secuencia bien definida:</p>
<ol>
<li><strong>Idle</strong>: ambas líneas (SDA y SCL) están en alto.</li>
<li><strong>START (S)</strong>: SDA pasa de alto a bajo mientras SCL está en alto.</li>
<li>El maestro envía la <strong>dirección + bit R/W</strong>.</li>
<li>El esclavo responde con <strong>ACK (bit bajo)</strong> si reconoce la dirección.</li>
<li>Se transmiten los datos en bloques de <strong>8 bits</strong>, cada uno seguido de un ACK/NACK.</li>
<li><strong>STOP §</strong>: SDA pasa de bajo a alto mientras SCL está en alto.</li>
</ol>
<h3 id="características-adicionales">Características adicionales:</h3>
<ul>
<li><strong>Clock stretching</strong>: el esclavo puede mantener la línea SCL baja para demorar la transmisión.</li>
<li><strong>ACK polling</strong>: útil para saber si un dispositivo está ocupado (como una EEPROM escribiendo).</li>
</ul>
<hr>
<h2 id="niveles-eléctricos">6. Niveles eléctricos</h2>
<p>Los niveles lógicos dependen de la tensión de alimentación (VDD), típicamente 3.3V o 5V.</p>

<table>
<thead>
<tr>
<th>Nivel lógico</th>
<th>Rango de voltaje</th>
</tr>
</thead>
<tbody>
<tr>
<td>Bajo (0)</td>
<td>−0.5 VDD a 0.3 VDD</td>
</tr>
<tr>
<td>Alto (1)</td>
<td>0.7 VDD a VDD + 0.5</td>
</tr>
</tbody>
</table><ul>
<li>Utiliza lógica <strong>directa</strong> (1 lógico = nivel alto).</li>
<li>SDA y SCL deben tener resistencias pull-up, ya que los drivers son de colector abierto.</li>
</ul>
<hr>
<h2 id="implementación-en-el-pic18f4550">7. Implementación en el PIC18F4550</h2>
<p>El PIC18F4550 incluye el módulo <strong>MSSP (Master Synchronous Serial Port)</strong>, que permite implementar I2C por hardware.</p>
<h3 id="características">Características:</h3>
<ul>
<li>Soporta <strong>modo maestro</strong> y <strong>modo esclavo</strong>.</li>
<li>Interrupciones para condiciones START, STOP, y ACK.</li>
<li>Compatible con direcciones de <strong>7 y 10 bits</strong>.</li>
</ul>
<h3 id="pines-utilizados">Pines utilizados:</h3>
<ul>
<li><strong>RB0</strong> → SDA (entrada/salida)</li>
<li><strong>RB1</strong> → SCL (entrada/salida)</li>
</ul>
<p>Ambos deben configurarse como <strong>entradas digitales</strong> para funcionamiento correcto en I2C.</p>
<ul>
<li>Configuración básica:<pre class=" language-c"><code class="prism  language-c">SSPADD <span class="token operator">=</span> <span class="token punctuation">(</span>Fosc <span class="token operator">/</span> <span class="token punctuation">(</span><span class="token number">4</span> <span class="token operator">*</span> BitRate<span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token operator">-</span> <span class="token number">1</span><span class="token punctuation">;</span>
<span class="token comment">// Ejemplo para 4 MHz y 100 kbps: SSPADD = (4 MHz / (4 * 100,000)) - 1 = 9</span>
</code></pre>
</li>
</ul>

