Monitor de temperatura e umidade com ESP32
O ESP32 lê a temperatura e a umidade do ambiente com um DHT11 e hospeda um painel web na rede Wi-Fi local. Quando a temperatura atinge o limite configurado, o dispositivo acende um LED e emite um alerta sonoro.
Materiais e montagem
- Placa ESP32, cabo USB e jumpers
- Sensor DHT11
- LED com resistor em série de 220 Ω a 330 Ω
- Buzzer ativo ou módulo de buzzer com entrada compatível com 3,3 V
Componente	Ligação no ESP32
DHT11 — DATA	GPIO 5 (D5)
DHT11 — VCC e GND	3V3 e GND
LED — ânodo	GPIO 4 (D4), com resistor em série
LED — cátodo	GND
Buzzer — entrada de sinal	GPIO 2 (D2)
Buzzer — terra	GND


Se estiver usando um módulo de buzzer com pino VCC separado, alimente-o conforme a especificação do módulo. Se o DHT11 for avulso e não tiver resistor de pull-up, ligue um resistor de 4,7 kΩ a 10 kΩ entre DATA e 3V3.
Instalação e execução
1. Abra o sketch .ino na Arduino IDE e instale o pacote de placas esp32 da Espressif Systems.
2. Pelo Gerenciador de Bibliotecas, instale DHT sensor library e Adafruit Unified Sensor, ambas da Adafruit. As bibliotecas WiFi.h e WebServer.h vêm com o pacote ESP32.
3. Configure WIFI_SSID com o nome de uma rede Wi-Fi de 2,4 GHz e WIFI_SENHA com a senha. Para rede aberta, use uma string vazia na senha.
4. Conecte o ESP32 por USB, selecione a placa e a porta serial na Arduino IDE e envie o sketch.
5. Abra o Monitor Serial em 115200 baud. Quando o ESP32 conectar, copie o endereço exibido após Abra no navegador. Abra esse endereço em um computador ou celular conectado à mesma rede.
Use valores de exemplo para as credenciais se publicar o sketch.
Funcionamento
O firmware tenta ler o sensor a cada 2 segundos. O painel mostra temperatura em °C, umidade relativa em %, estado do alarme, endereço IP, intensidade do sinal Wi-Fi e tempo de funcionamento. O gráfico mantém até 40 amostras válidas na memória da página e reinicia ao recarregá-la.
Condição	Comportamento
Temperatura abaixo do limite	LED e buzzer desligados
Temperatura igual ou acima do limite	LED aceso e três bipes por leitura
Falha na leitura do DHT11	LED acende durante dois bipes curtos e o painel indica erro


Altere TEMP_LIMITE para ajustar o limite padrão de 20,0 °C. A faixa de umidade exibida no painel é informativa. O alarme usa a temperatura.
Rotas HTTP
Rota	Resposta
GET /	Painel HTML hospedado no ESP32
GET /dados	JSON com temp, umid, limite, alarme, erro, ip, rssi e uptime


O servidor usa a porta 80. Em caso de falha no sensor, a API retorna erro: true e envia zero em temp e umid como valores de preenchimento.
Solução de problemas
Sintoma	O que conferir
DHT.h: No such file or directory	Instalação da biblioteca DHT sensor library
Erro ao ler o DHT!	Alimentação, GND, fio DATA no GPIO 5, resistor de pull-up e tipo DHT11 no código
Falha na conexão Wi-Fi	Nome, senha e disponibilidade da rede de 2,4 GHz. O sketch aguarda até 20 segundos na inicialização
O painel não abre	Use o IP que aparece no Monitor Serial, com http://, e confirme que a rede permite comunicação entre os dispositivos
