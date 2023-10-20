# Estudo de plataforma embarcada - Raspberry Pi Pico W
<h2>I. Manual de referência</h2>
<h3>1.1 Informações gerais do Raspberry Pi Pico W</h3>
O Raspberry Pi Pico W foi projetado para ser uma plataforma de desenvolvimento flexível e de baixo custo para o RP2040, com uma interface sem fio de 2,4 GHz, tendo suporte para funções centrais e periféricas do Bluetooth LE e suporte para Bluetooth Clássico, WiFi 4 (802.11n), WPA3 e SoftAP (até quatro clientes). A interface sem fio é conectada via SPI ao RP2040. O Pico W foi projetado para usar pin-headers soldados ou para ser posicionado como um "módulo" montável em superfície. Há pads SMT sob o conector USB e o botão BOOTSEL, que permitem que esses sinais sejam acessados se usados como um módulo SMT soldado por refluxo. Alguns recursos principais são:

<ul><li>2 MB de memória flash;</li>
<li>porta micro USB B para alimentação e dados (e para reprogramação do flash);</li>
<li>Expõe 26 E/S de uso geral (GPIO) multifuncionais de 3,3 V;</li>
<li>23 GPIO são apenas digitais, sendo que três também são compatíveis com ADC;</li>
<li>Várias opções para alimentar facilmente a unidade a partir de micro USB, fontes externas ou baterias;</li>
<li>Alta qualidade, baixo custo e alta disponibilidade;</li>
<li>SDK abrangente, exemplos de software e documentação;</li>
<li>Porta de depuração de fio serial (SWD) Arm de 3 pinos;</li>
<li>Arquitetura de fonte de alimentação simples, porém altamente flexível;</li>
<li>USB1.1 integrado (dispositivo ou host);</li>
<li>30 E/S multifuncionais de uso geral (quatro podem ser usadas para ADC);</li>
<li>Tensão de E/S de 1,8 a 3,3 V;</li>
<li>Conversor analógico-digital (ADC) de 12 bits e 500ksps;</li>
<li>Vários periféricos digitais</li>
<ul><li>2 × UART, 2 × I2C, 2 × SPI, 16 × canais PWM</li> </ul>
<li>1 temporizador com 4 alarmes e 1 relógio de tempo real;</li>
<li>2 blocos de E/S programável (PIO), 8 máquinas de estado no total;</li>
<li>E/S de alta velocidade flexível e programável pelo usuário;</li>
<li>Pode emular interfaces como cartão SD e VGA;</li>
</ul>

O Raspberry Pi Pico W fornece um circuito externo mínimo, porém flexível, para dar suporte ao chip RP2040: memória flash, um cristal, fontes de alimentação e desacoplamento, e conector USB. Quatro E/S do RP2040 são usadas para funções internas: acionamento de um LED, controle de energia da fonte de alimentação de modo comutado (SMPS) integrada e detecção das tensões do sistema.

O Raspberry Pi Pico W usa um SMPS buck-boost integrado que é capaz de gerar os 3,3 V necessários (para alimentar o RP2040 e os circuitos externos) a partir de uma ampla gama de tensões de entrada (1,8 a 5,5 V). Isso permite uma flexibilidade significativa na alimentação da unidade a partir de várias fontes, como uma única célula de íons de lítio ou três células AA em série. Os carregadores de bateria também podem ser facilmente integrados à cadeia de alimentação do Pico W.

A reprogramação do flash do Pico W pode ser feita usando um USB, ou a porta de depuração de fio serial (SWD) padrão pode redefinir o sistema e carregar e executar o código sem pressionar nenhum botão. A porta SWD também pode ser usada para depurar interativamente o código em execução no RP2040.

<h3>1.2 Especificações</h3>
O Pico W é uma placa de circuito impresso de face única de 51 mm × 21 mm × 1 mm, e 40 pinos, com uma porta micro USB sobreposta à borda superio. A antena sem fio integrada está localizada na borda inferior. Se algo for colocado próximo à antena (em qualquer dimensão), a eficácia da antena será reduzida.
<h4>1.2.1 Pico W pinout</h4>
A pinagem do Pico W foi projetada para destacar diretamente o máximo possível da GPIO do RP2040 e da função do circuito interno, além de fornecer um número adequado de pinos de aterramento para reduzir a interferência eletromagnética (EMI) e a diafonia de sinal. Além dos pinos GPIO e de aterramento, há outros sete pinos na interface principal de 40 pinos:
<ul><li>VBUS: é a tensão de entrada do micro-USB, conectada ao pino 1 da porta micro-USB, que é nominalmente de 5 V (ou 0 V se o USB não estiver conectado ou não for alimentado).</li>
<li>VSYS: é a tensão de entrada principal do sistema, que pode variar na faixa permitida de 1,8 V a 5,5 V, e é usada pelo SMPS integrado para gerar 3,3 V para o RP2040 e seu GPIO.</li>
<li>3V3_EN: conecta-se ao pino de habilitação do SMPS integrado e é puxado para cima por meio de um resistor de 100kΩ. Para desativar os 3,3 V (que também desativa a potência do RP2040), coloque esse pino em curto.</li>
<li>3V3: é a fonte principal de 3,3 V, gerada pelo SMPS integrado. Esse pino pode ser usado para alimentar circuitos externos (recomenda-se manter a carga nesse pino abaixo de 300 mA).</li>
<li>ADC_VREF: é a tensão de alimentação do ADC e é gerada no Pico W pela filtragem da alimentação de 3,3 V. Esse pino pode ser usado com uma referência externa se for necessário um melhor desempenho do ADC.</li>
<li>AGND: é a referência de aterramento para GPIO26-29. Há um plano de aterramento analógico separado que passa por baixo desses sinais e termina nesse pino. Se o ADC não for usado ou se o desempenho do ADC não for crítico, esse pino poderá ser conectado ao terra digital.</li>
<li>RUN: é o pino de ativação do RP2040 e tem um resistor de pull-up interno (no chip) para 3,3 V de cerca de 50kΩ. Para reiniciar o RP2040, coloque esse pino em um curto-circuito baixo.</li>
</ul>
Por fim, há também seis pontos de teste (TP1-TP6), que podem ser acessados se necessário, por exemplo, se for usado como um módulo de montagem em superfície. Esses pontos são respectivamente Ground, USB DM, USB DP, WL_GPIO1/SMPS PS pin (não deve ser usado), WL_GPIO0/LED (não recomendável) e BOOTSEL. TP1, TP2 e TP3 podem ser usados para acessar sinais USB em vez de usar a porta micro-USB. O TP6 pode ser usado para colocar o sistema no modo de programação USB de armazenamento em massa.

<h4>1.2.2 Condições operacionais recomendadas</h4>
As condições operacionais do Pico W são, em grande parte, em função das condições operacionais especificadas por seus componentes:
<ul><li>Temp. de operação máxima: 70°C (incluindo autoaquecimento)</li>
<li>Temperatura de operação mínima: -20°C</li>
<li>VBUS 5V ± 10%.</li>
<li>VSYS Mínimo 1,8V</li>
<li>VSYS Máx. 5,5V</li>
</ul>
<h3>1.3 Informações de Aplicações</h3>
<h4>1.3.1 Programação do flash</h4>
O flash de 2 MB integrado pode ser (re)programado usando a porta de depuração de fio serial ou pelo modo especial de dispositivo de armazenamento em massa USB. A maneira mais simples de reprogramar o flash do Pico W é usar o modo USB. O código de inicialização do USB é armazenado na ROM do RP2040, portanto não pode ser sobrescrito acidentalmente.

<h4>1.3.2 General purpose I/O</h4>
O GPIO do Pico W é alimentado pelo trilho de 3,3 V integrado e é fixado em 3,3 V. O Pico W expõe 26 dos 30 possíveis pinos GPIO, encaminhando-os diretamente para os pinos de header do Pico W. GPIO0 a GPIO22 são apenas digitais, e GPIO 26-28 podem ser usados como GPIO digital ou como entradas ADC (selecionáveis por software). Os GPIO 26-29 são compatíveis com ADC e têm um diodo reverso interno para o trilho VDDIO (3,3 V), de modo que a tensão de entrada não deve exceder VDDIO cerca de 300 mV a mais. Os pinos GPIO 0 a 25 (e os pinos de depuração) não têm essa restrição e, portanto, a tensão pode ser aplicada com segurança a esses pinos quando o RP2040 estiver sem alimentação até 3,3 V.
<h4>1.3.3 Usando o ADC</h4>
O ADC não possui uma referência interna, ele utiliza a própria alimentação como referência. No Pico W, o pino ADC_AVDD (alimentação do ADC) é gerado a partir dos 3,3V do SMPS, utilizando um filtro R-C (201Ω em 2,2μF). Isso depende da precisão da saída de 3,3V do SMPS, mas não filtra completamente o ruído da fonte de alimentação. O ADC consome corrente, resultando em um desvio de cerca de 30mV. É possível reduzir o desvio alterando a resistência entre o ADC_VREF e o pino 3,3V, mas isso aumenta o ruído. Alternativamente, ao forçar o modo PWM do fornecimento de energia, o ripple inerente pode ser reduzido, embora isso diminua a eficiência em cargas leves. O desvio do ADC pode ser minimizado conectando um segundo canal do ADC à terra para estimar o desvio. Para um desempenho aprimorado do ADC, uma referência de tensão externa de 3.0V, como o LM4040, pode ser conectada ao pino ADC_VREF. No entanto, isso limita a faixa de medição do ADC a sinais de 0V a 3.0V e consome corrente contínua. O ADC do RP2040 foi qualificado apenas em 3.0/3.3V, mas deve funcionar a partir de cerca de 2V.
<h4>1.3.4 USB</h4>
O RP2040 tem um PHY e um controlador USB1.1 integrados que podem ser usados tanto no modo dispositivo quanto no modo host. O Pico W adiciona os dois resistores externos de 27Ω necessários e leva essa interface a uma porta micro-USB padrão. A porta USB pode ser usada para acessar o carregador de inicialização USB (modo BOOTSEL) armazenado na ROM de inicialização do RP2040. Ela também pode ser usada pelo código do usuário para acessar um dispositivo ou host USB externo.
<h2>II. Interfaces de comunicação</h2>
<h3>2.1 UART</h3>
Universal Asynchronous Receiver/Transmitter (UART) ou transmissor/receptor assíncrono universal define um protocolo, ou seja, um conjunto de regras para a troca de dados seriais entre dois dispositivos. 

O UART executa:
<ul><li>Conversão de serial para paralelo nos dados recebidos de um dispositivo periférico</li>
<li>Conversão de paralelo para serial nos dados transmitidos para o dispositivo periférico.</li></ul>

A CPU lê e grava dados e informações de controle/status por meio da interface AMBA APB. Os caminhos de transmissão e recepção são armazenados em buffer com memórias FIFO internas, permitindo que até 32 bytes sejam armazenados independentemente nos modos de transmissão e recepção. 

O UART inclui um gerador de taxa de transmissão programável que gera um relógio interno de transmissão e recepção comum a partir da entrada do relógio de referência interna da UART, UARTCLK

O UART pode gerar:
<ul><li>Interrupções individualmente mascaráveis das condições de recepção (incluindo tempo limite), transmissão, status do modem e erro</li>
<li>Uma única interrupção combinada, de modo que a saída seja ativada se qualquer uma das interrupções individuais for ativada e desmascarada</li>
<li>Sinais de solicitação de DMA para interface com um controlador de acesso direto à memória (DMA). Se ocorrer um erro de enquadramento, paridade ou interrupção durante a recepção, o bit de erro apropriado será definido e armazenado no FIFO. Se ocorrer uma condição de sobrecarga, o bit de registro será definido imediatamente e os dados do FIFO serão impedidos de serem sobrescritos. É possível programar os FIFOs para terem 1 byte de profundidade, fornecendo uma interface UART convencional com buffer duplo. Há um recurso de controle de fluxo de hardware programável que usa a entrada nUARTCTS e a saída nUARTRTS para controlar automaticamente o fluxo de dados seriais.</li></ul>

<h3>2.2 I2C (Inter Integrated Circuit)</h3>
O I2C é uma interface de dois fios comumente usada para conectar dispositivos para transferência de dados em baixa velocidade usando o clock SCL e os fios de dados SDA. Esses fios transportam informações entre os dispositivos conectados ao barramento. Cada dispositivo é reconhecido por um endereço exclusivo e pode operar como "transmissor" ou "receptor", dependendo da função do dispositivo. Os dispositivos também podem ser considerados como mestres ou escravos ao realizar transferências de dados. Um mestre é um dispositivo que inicia uma transferência de dados no barramento e gera os sinais de relógio para permitir essa transferência. Nesse momento, qualquer dispositivo endereçado é considerado um escravo.

Cada controlador I2C é baseado em uma configuração do IP Synopsys DW_apb_i2c (v2.01). Os seguintes recursos são suportados:
<ul><li>Mestre ou escravo (padrão para o modo Mestre)</li>
<li>Modo padrão, modo rápido ou modo rápido plus</li>
<li>Endereço escravo padrão 0x055</li>
<li>Suporta endereçamento de 10 bits no modo mestre</li>
<li>Buffer de transmissão de 16 elementos</li>
<li>Buffer de recepção de 16 elementos</li>
<li>Pode ser acionado por DMA</li>
<li>Pode gerar interrupções</li>
</ul>

Cada controlador deve conectar seu clock SCL e dados SDA a um par de GPIOs. O padrão I2C exige que os drivers conduzam um sinal baixo ou, quando não for acionado, o sinal será puxado para cima. Isso se aplica a SCL e SDA. Os pads GPIO devem ser configurados para:
- pull-up ativado
- taxa de variação limitada
- gatilho schmitt ativado

O bloco I2C pode operar nos seguintes modos:
- modo padrão (com taxas de dados de 0 a 100kbps),
- modo rápido (com taxas de dados menores ou iguais a 400 kbps),
- modo rápido plus (com taxas de dados menores ou iguais a 1000kbps).

Esses modos não são compatíveis:
- Modo de alta velocidade (com taxas de dados menores ou iguais a 3,4 Mbps),
- Modo de velocidade ultra-rápida (com taxas de dados menores ou iguais a 5 Mbps).

Um exemplo de dispositivos de modo de alta velocidade são os monitores LCD, ADCs de alta contagem de bits e EEPROMs de alta capacidade. Esses dispositivos Esses dispositivos normalmente precisam transferir grandes quantidades de dados.

<h3>2.3 SPI (Serial Peripheral Interface)</h3>
O SSP da PrimeCell é uma interface mestre ou escrava para comunicação serial síncrona com dispositivos periféricos que possuem interfaces seriais síncronas Motorola SPI, National Semiconductor Microwire ou Texas Instruments. O SSP da PrimeCell realiza a conversão serial-paralela nos dados recebidos de um dispositivo periférico. A CPU acessa informações de dados, controle e status por meio da interface AMBA APB. Os caminhos de transmissão e recepção são armazenados em buffer com memórias FIFO internas, permitindo que até oito valores de 16 bits sejam armazenados independentemente nos modos de transmissão e recepção. Os dados seriais são transmitidos no SSPTXD e recebidos no SSPRXD. 

O PrimeCell SSP inclui um divisor de relógio de taxa de bits programável e um pré-escalonador para gerar o relógio de saída serial, SSPCLKOUT, a partir do relógio de entrada, SSPCLK. As taxas de bits são compatíveis com 2 MHz ou mais, sujeitas à escolha da frequência para SSPCLK, e a taxa máxima de bits é determinada pelos dispositivos periféricos. É possível usar os registros de controle SSPCR0 e SSPCR1 para programar o modo de operação, o formato de quadro e o tamanho do PrimeCell SSP, e tamanho.
<h2>III. Prova de conceito</h2>
O objetivo da prova de conceito é demonstrar o uso do protocolo I2C para conectar um display LCD 16x2 ao Raspberry Pi Pico W.

O display possui quatro pinos: GND (aterramento), VCC (energia), SDA e SCL (os dois fios necessários para a interface I2C) que são conectados respectivamente nos pinos do Pico W GND.2, VBUS (5V), GP0 e GP1.

Link do código:
<a src="https://github.com/dtonavitor/estudo-de-plataforma-embarcada/tree/main/src">https://github.com/dtonavitor/estudo-de-plataforma-embarcada/tree/main/src</a><br>
Link do simulador: <a src="https://wokwi.com/projects/379109848075599873">https://wokwi.com/projects/379109848075599873</a>



