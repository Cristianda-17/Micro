---


---

<h1 id="div-aligncentercomunicacion-serialdiv"><div align="center">Comunicacion Serial</div></h1>
<p>Cristian David Sotelo Nieto (134770)<br>
Juan Contreras (134126)<br>
Carlos Andres Estupiñan (82149)<br>
Universidad Ecci<br>
Microcontroladores<br>
Jorge Eduardo Cote<br>
01/06/2025</p>
<hr>
<h1 id="comunicación-serial-síncrona">Comunicación Serial Síncrona</h1>
<ul>
<li>La comunicación serial síncrona es una <em>interfaz punto a punto</em> basada en el modelo <em>maestro-esclavo</em>.</li>
<li>Permite <em>comunicación full dúplex</em>, es decir, envío y recepción simultáneos.</li>
<li>El <em>maestro</em> suele ser un microcontrolador y el <em>esclavo</em>, un periférico (sensor, actuador, módulo de comunicación, etc.).</li>
<li>Requiere una <em>señal de reloj</em> para sincronizar la transmisión y recepción de datos.</li>
</ul>
<hr>
<h2 id="interfaz-spi-serial-peripheral-interface">Interfaz SPI (Serial Peripheral Interface)</h2>
<h3 id="líneas-utilizadas-por-spi">Líneas utilizadas por SPI:</h3>
<ul>
<li><em>MOSI</em> (Master Out, Slave In): línea de datos del maestro al esclavo.</li>
<li><em>MISO</em> (Master In, Slave Out): línea de datos del esclavo al maestro.</li>
<li><em>SCK</em> (Serial Clock): reloj generado por el maestro.</li>
<li><em>SS</em> (Slave Select): señal para seleccionar al esclavo.</li>
</ul>
<hr>
<h2 id="aplicaciones-del-spi">Aplicaciones del SPI</h2>
<p>Se utiliza para comunicar microcontroladores con:</p>
<ul>
<li><em>Memorias EEPROM</em></li>
<li><em>Registros de desplazamiento</em></li>
<li><em>Conversores analógico-digital (A/D)</em></li>
<li><em>Drivers de pantallas</em></li>
</ul>
<hr>
<h2 id="módulo-mssp-en-pic18f4550">Módulo MSSP en PIC18F4550</h2>
<p>El <em>Master Synchronous Serial Port (MSSP)</em> permite la implementación de:</p>
<ul>
<li><em>SPI</em></li>
<li><em>I2C</em></li>
</ul>
<p>Modos de operación:</p>
<ul>
<li><em>Maestro:</em> solo disponible en SPI.</li>
<li><em>Esclavo:</em> con sistema de direccionamiento (especialmente en I2C).</li>
</ul>
<hr>
<h2 id="registros-de-configuración-spi">Registros de Configuración SPI</h2>
<h3 id="principales-registros-en-modo-spi">Principales registros en modo SPI:</h3>
<ul>
<li><em>SSPCON1:</em> configuración del modo (maestro/esclavo, reloj, etc.).</li>
<li><em>SSPSTAT:</em> supervisión del estado de transmisión.</li>
<li><em>SSPBUF:</em> buffer de datos de transmisión/recepción.</li>
<li><em>SSPSR:</em> registro de desplazamiento (no accesible directamente).</li>
</ul>
<hr>
<h2 id="parámetros-de-configuración">Parámetros de Configuración</h2>
<p>Se deben configurar los siguientes parámetros:</p>
<ul>
<li><em>Modo maestro:</em> SCK como salida.</li>
<li><em>Modo esclavo:</em> SCK como entrada.</li>
<li><em>Polaridad del reloj:</em> estado inactivo del reloj.</li>
<li><em>Fase de datos:</em> flanco del reloj para captura de datos.</li>
<li><em>Tasa de transferencia:</em> solo en modo maestro.</li>
<li><em>Selección de esclavo:</em> solo en modo esclavo.</li>
</ul>
<hr>
<h2 id="configuración-de-pines-en-el-pic18f4550">Configuración de Pines en el PIC18F4550</h2>

<table>
<thead>
<tr>
<th>Señal</th>
<th>Pin</th>
<th>Dirección</th>
<th>Observación</th>
</tr>
</thead>
<tbody>
<tr>
<td>SDI</td>
<td>TRISB&lt;0&gt;</td>
<td>Entrada</td>
<td>Configurar como entrada digital</td>
</tr>
<tr>
<td>SDO</td>
<td>TRISC&lt;7&gt;</td>
<td>Salida</td>
<td></td>
</tr>
<tr>
<td>SCK</td>
<td>TRISB&lt;1&gt;</td>
<td>Depende</td>
<td>0: maestro, 1: esclavo</td>
</tr>
<tr>
<td>SS</td>
<td>TRISA&lt;5&gt;</td>
<td>Entrada</td>
<td>Configurar como entrada digital</td>
</tr>
</tbody>
</table><hr>
<h2 id="conexión-típica">Conexión Típica</h2>
<ul>
<li>El <em>maestro inicia</em> la transferencia de datos al controlar la señal <em>SCK</em>.</li>
<li>El <em>registro SSPBUF</em> debe ser escrito para iniciar una transmisión.</li>
<li>Si solo se va a *recibir datos, se debe configurar *<em>SDO como entrada</em>.</li>
</ul>
<hr>
<h2 id="tasa-de-bits-modo-maestro">Tasa de Bits (Modo Maestro)</h2>
<p>La velocidad de transmisión se puede configurar como:</p>
<ul>
<li>Fosc / 4  o TCY</li>
<li>Fosc / 16 o 4TCY</li>
<li>Fosc / 64 o 16TCY</li>
</ul>
<p>Usando <em>Timer2</em> se puede alcanzar un máximo de <em>12 Mbps</em>.</p>
<hr>
<h2 id="modo-esclavo">Modo Esclavo</h2>
<ul>
<li>La transmisión/recepción se activa al recibir la señal <em>SCK</em>.</li>
<li>Cuando se completa la recepción del último bit, se genera una <em>interrupción</em>.</li>
<li>La línea <em>SS</em> en nivel bajo habilita el esclavo.</li>
</ul>
<hr>
<h2 id="señales-en-ambos-modos">Señales en Ambos Modos</h2>
<ul>
<li>En <em>modo maestro</em>, las señales SPI son controladas por el microcontrolador.</li>
<li>En <em>modo esclavo</em>, las señales son activadas por el maestro, y el esclavo responde según su configuración.</li>
</ul>
<hr>
<h2 id="en-conclusión-podríamos-decir-que">En conclusión podríamos decir que:</h2>
<p>La <em>comunicación serial síncrona</em> mediante <em>SPI</em> es fundamental para conectar microcontroladores con dispositivos externos. El <em>PIC18F4550</em> ofrece soporte completo mediante su módulo <em>MSSP</em>, con registros dedicados y pines configurables. La clave está en configurar correctamente los modos, pines, y registros para asegurar una transmisión eficaz y sin errores.</p>

