# Material de Estudo: Automação Industrial Aplicada a uma Esteira Transportadora Inteligente

## 1. Introdução

Este material de estudo foi elaborado com apoio do Gemini Notebook, utilizando documentação técnica e materiais audiovisuais sobre automação industrial, sensores, microcontroladores e visão computacional.

O conteúdo está relacionado ao desenvolvimento de uma esteira transportadora inteligente com dimensões aproximadas de **100 cm × 25 cm**, utilizando:

* Microcontrolador ESP32-S3.
* Motorredutor de corrente contínua de 12 V.
* Sensores para detecção de materiais.
* Servomotores SG90.
* Notebook com webcam para visão computacional.
* Python e OpenCV para processamento de imagens.

O objetivo é organizar conhecimentos técnicos, registrar as fontes consultadas e relacionar os conceitos estudados à aplicação prática do projeto.

---

## 2. Sensores indutivos e capacitivos

### 2.1 Conceitos teóricos

**Sensor indutivo:** detecta objetos metálicos sem contato físico. Seu funcionamento utiliza um campo eletromagnético de alta frequência. Quando um metal se aproxima, correntes induzidas alteram a oscilação interna do sensor, provocando a mudança do sinal de saída.

**Sensor capacitivo:** detecta materiais metálicos e não metálicos, dependendo das características do objeto e dos ajustes do sensor. Sua operação baseia-se na alteração da capacitância elétrica causada pela aproximação de um material.

**Histerese:** diferença entre a distância de acionamento e a distância de desacionamento do sensor. Essa característica ajuda a evitar comutações repetitivas causadas por pequenas oscilações do objeto.

**Conexões elétricas:** muitos sensores industriais de três fios utilizam marrom para alimentação positiva, azul para 0 V e preto para saída. Entretanto, a pinagem, a tensão e o tipo de saída devem ser confirmados no manual do modelo utilizado. Sensores podem apresentar saídas PNP ou NPN.

### 2.2 Aplicação na esteira

O sensor indutivo pode identificar a passagem de peças metálicas. Um sensor capacitivo pode auxiliar na detecção de objetos não metálicos, como plástico ou papelão, desde que o modelo e a regulagem sejam adequados à aplicação.

A combinação dos sensores pode fornecer informações complementares para a classificação dos materiais.

### 2.3 Perguntas e respostas

**Pergunta 1:** Qual é a principal diferença entre os sensores indutivo e capacitivo?

**Resposta:** O indutivo utiliza um campo eletromagnético para detectar metais. O capacitivo detecta alterações de capacitância e pode identificar materiais metálicos e não metálicos.

**Pergunta 2:** Por que a histerese é importante?

**Resposta:** Ela ajuda a evitar que pequenas vibrações ou variações na posição do objeto provoquem acionamentos repetitivos próximos ao limite de detecção.

### 2.4 Fontes consultadas

* [OMRON — Overview of Proximity Sensors](https://www.ia.omron.com/support/guide/41/overview.html)
* [OMRON — Technical Guide](https://www.ia.omron.com/support/guide/index.html)
* [YouTube — The Inductive and Capacitive Sensor](https://www.youtube.com/watch?v=MechatronicNeheyler)

---

## 3. GPIO do ESP32-S3 e leitura de sensores

### 3.1 Conceitos teóricos

GPIO significa *General Purpose Input/Output*. São pinos que podem ser configurados para receber sinais digitais ou controlar dispositivos externos.

O ESP32-S3 utiliza lógica de 3,3 V em seus GPIOs. **Os pinos não devem receber diretamente sinais industriais de 5 V, 12 V ou 24 V.** A interface elétrica deve ser dimensionada de acordo com o sensor e a placa utilizada, podendo exigir optoacopladores, conversores de nível ou outros circuitos de condicionamento.

**Entradas flutuantes:** ocorrem quando um pino não possui um nível lógico definido. Resistores de pull-up ou pull-down podem estabelecer um estado padrão.

**Interrupções:** permitem que o microcontrolador reaja a determinadas mudanças no sinal de entrada sem precisar verificar continuamente cada pino no programa principal.

**Filtros de ruído:** podem ajudar a rejeitar pulsos indesejados, mas precisam ser configurados de acordo com as características do sinal e não substituem um projeto elétrico adequado.

### 3.2 Aplicação na esteira

Quando uma peça passa pelo sensor indutivo, sua saída muda de estado. Após o condicionamento elétrico adequado, o ESP32-S3 pode ler essa mudança e registrar a passagem da peça.

O programa poderá utilizar essa informação para coordenar a detecção com o acionamento dos atuadores.

### 3.3 Perguntas e respostas

**Pergunta 1:** Por que não se deve conectar diretamente uma saída de sensor de 12 V ao GPIO do ESP32-S3?

**Resposta:** Porque a tensão pode ultrapassar os limites elétricos do microcontrolador e danificar a entrada. É necessário utilizar uma interface de condicionamento compatível com o sinal e com o circuito.

**Pergunta 2:** Qual é a vantagem de utilizar interrupções?

**Resposta:** Elas permitem responder a eventos específicos de entrada sem depender exclusivamente da verificação contínua dos pinos no programa principal.

### 3.4 Fontes consultadas

* [Espressif — GPIO & RTC GPIO: ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html)
* [YouTube — ESP32 Master Course: Hardware Interfacing](https://www.youtube.com/watch?v=GelmarTechLab)

---

## 4. PWM e controle de atuadores

### 4.1 Conceitos teóricos

PWM (*Pulse Width Modulation*), ou modulação por largura de pulso, é uma técnica que controla a duração dos pulsos de um sinal digital dentro de um período.

O ciclo de trabalho (*duty cycle*) representa a proporção do período em que o sinal permanece em nível alto.

O ESP32-S3 possui o periférico LEDC, que permite gerar sinais PWM configuráveis.

No ESP-IDF, o processo geral envolve:

1. Configurar o temporizador, incluindo frequência e resolução.
2. Configurar o canal e associá-lo a um GPIO.
3. Atualizar o ciclo de trabalho conforme a necessidade da aplicação.

### 4.2 Aplicação na esteira

**Motorredutor CC de 12 V:** um sinal PWM pode controlar a velocidade por meio de um driver de potência compatível. O GPIO não deve alimentar o motor diretamente.

**Servomotor SG90:** utiliza pulsos de controle para posicionar seu eixo. A frequência e a largura dos pulsos devem respeitar as especificações do fabricante e podem precisar de ajuste durante os testes.

O PWM usado para controlar a velocidade de um motor CC não deve ser confundido com o sinal de posicionamento de um servomotor.

### 4.3 Perguntas e respostas

**Pergunta 1:** Quais são as etapas básicas para configurar PWM com o periférico LEDC?

**Resposta:** Configurar o temporizador, configurar o canal associado ao GPIO e atualizar o ciclo de trabalho do sinal.

**Pergunta 2:** Qual é a diferença entre utilizar PWM em um motor CC e em um servomotor?

**Resposta:** No motor CC, o PWM pode regular a potência média aplicada e, consequentemente, influenciar a velocidade. No servomotor, os pulsos de controle indicam a posição desejada do eixo.

### 4.4 Fontes consultadas

* [Espressif — LED Control (LEDC): ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/v5.5/esp32s3/api-reference/peripherals/ledc.html)
* [YouTube — ESP32 Master Course: Hardware Interfacing](https://www.youtube.com/watch?v=GelmarTechLab)

---

## 5. Visão computacional com Python e OpenCV

### 5.1 Conceitos teóricos

O OpenCV é uma biblioteca utilizada para processamento de imagens e visão computacional. Com Python, é possível capturar imagens de uma webcam, processar os quadros e analisar características dos objetos.

Um fluxo básico de processamento pode incluir:

1. **Captura de imagem:** obter quadros da webcam.
2. **Região de interesse (ROI):** selecionar somente a área relevante da esteira.
3. **Escala de cinza:** simplificar a representação da imagem.
4. **Limiarização:** separar regiões da imagem conforme a intensidade dos pixels.
5. **Detecção de contornos:** identificar os limites de regiões que se destacam do fundo.
6. **Análise de área:** calcular a área dos contornos em pixels.
7. **Classificação:** aplicar regras para identificar objetos que precisam de atenção.

A área em pixels, isoladamente, não permite identificar com segurança a classe ou o material de um objeto. Os resultados dependem da iluminação, do fundo, da posição da câmera e dos critérios de classificação.

### 5.2 Aplicação na esteira

Uma webcam posicionada sobre a esteira poderá capturar imagens dos objetos em movimento. O Python e o OpenCV poderão analisar essas imagens para identificar características visuais relevantes.

Se o sistema detectar uma condição inesperada, poderá enviar um comando ao ESP32-S3 por comunicação serial USB, desde que o protocolo e o tratamento de falhas sejam implementados e testados.

A parada de emergência não deve depender desse processamento por software.

### 5.3 Perguntas e respostas

**Pergunta 1:** Qual é a função da região de interesse (ROI)?

**Resposta:** Restringir o processamento à área relevante da esteira, reduzindo a influência de elementos externos à região analisada.

**Pergunta 2:** Como `cv2.contourArea()` pode ajudar na análise de objetos?

**Resposta:** A função calcula a área de um contorno em pixels. Esse valor pode ser usado como uma das características para comparar objetos, desde que os limites sejam calibrados para o ambiente real.

### 5.4 Fontes consultadas

* [OpenCV — Documentação e tutoriais](https://docs.opencv.org/4.12.0/index.html)
* [YouTube — Identify Objects Moving on a Conveyor Belt Using OpenCV with Python](https://www.youtube.com/watch?v=Pysource)

---

## 6. Integração dos componentes do protótipo

### 6.1 Arquitetura proposta

O sistema pode ser organizado em dois níveis:

**Notebook:**

* Executa o código Python e OpenCV.
* Recebe imagens da webcam.
* Analisa características visuais dos objetos.
* Envia comandos ao microcontrolador pela comunicação serial.

**ESP32-S3:**

* Recebe os sinais dos sensores.
* Executa a lógica de controle.
* Comanda os drivers e os atuadores.
* Coordena as ações conforme os comandos recebidos e o estado dos sensores.

### 6.2 Alimentação e interfaces elétricas

A integração elétrica deve considerar:

* Alimentação compatível com a placa ESP32-S3.
* Fonte adequada ao motorredutor de 12 V.
* Fonte dimensionada para os servomotores, conforme suas especificações e corrente de pico.
* Drivers compatíveis com a corrente, a tensão e o tipo de motor.
* Condicionamento elétrico das saídas dos sensores.
* Proteção contra sobrecorrente e ruídos elétricos.
* Referência de terra compatível com a arquitetura escolhida.

A necessidade de interligar os GNDs depende do circuito e do uso de isolamento. Em interfaces isoladas, a conexão deve seguir o projeto elétrico; não se deve unir referências indiscriminadamente.

### 6.3 Aplicação na esteira

Quando uma peça entrar na região de detecção, os sensores e a webcam poderão fornecer informações complementares. O ESP32-S3 poderá utilizar esses dados para decidir se deve manter o transporte ou acionar um mecanismo de separação.

A sincronização entre a posição do objeto, o tempo de processamento e o movimento do atuador precisará ser testada no protótipo.

### 6.4 Perguntas e respostas

**Pergunta 1:** Por que o motor precisa de um driver de potência?

**Resposta:** Porque os pinos do microcontrolador fornecem sinais de controle, não a corrente necessária para alimentar diretamente um motor. O driver faz a interface entre o sinal lógico e a carga.

**Pergunta 2:** Qual é a função do notebook na arquitetura proposta?

**Resposta:** Executar o processamento de visão computacional e transmitir informações ao ESP32-S3, que realiza o controle dos componentes conectados a ele.

### 6.5 Fontes consultadas

* [Espressif — GPIO do ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html)
* [Espressif — LEDC do ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/v5.5/esp32s3/api-reference/peripherals/ledc.html)
* [OpenCV — Documentação](https://docs.opencv.org/4.12.0/index.html)
* [Siemens SCE — Cursos de automação](https://www.siemens.com/pt-br/content/sce-educational-institutions/plc-courses/)
* [Schneider Electric](https://www.se.com/ww/en/)

---

## 7. Limitações, falhas e segurança

### 7.1 Ruído elétrico e cargas indutivas

Motores, relés e outras cargas indutivas podem produzir perturbações elétricas durante a comutação. O projeto deve considerar drivers adequados, proteção contra transientes, desacoplamento da alimentação e organização da fiação.

A escolha de diodos de roda-livre ou de outros circuitos de supressão depende do tipo de carga e do driver utilizado. A solução deve ser validada conforme os manuais dos componentes.

### 7.2 Botões e entradas digitais

Botões mecânicos podem apresentar repiques de contato (*bouncing*). Filtros por software ou hardware podem ajudar a evitar múltiplas leituras de um único acionamento.

Os pinos do ESP32-S3 também devem ser escolhidos conforme as funções da placa, os periféricos utilizados e as restrições indicadas na documentação técnica.

### 7.3 Parada de emergência

A parada de emergência deve ser tratada como uma função de segurança, não como um simples comando de software.

A implementação precisa ser avaliada pelo instrutor responsável, considerando os riscos mecânicos e elétricos, os componentes utilizados e os requisitos de segurança aplicáveis. A função de emergência não deve depender exclusivamente do ESP32-S3, do notebook ou da webcam.

### 7.4 Limitações da visão computacional

A identificação visual pode ser afetada por:

* Mudanças de iluminação.
* Sombras e reflexos.
* Variações na posição e orientação dos objetos.
* Vibração da câmera.
* Semelhança visual entre objetos diferentes.
* Interrupção da comunicação serial.

Essas limitações devem ser registradas e avaliadas durante os testes.

### 7.5 Perguntas e respostas

**Pergunta 1:** Por que é necessário avaliar a proteção contra transientes elétricos no acionamento de motores?

**Resposta:** Porque a comutação de cargas indutivas pode gerar perturbações que danificam componentes ou interferem no funcionamento do circuito.

**Pergunta 2:** Por que a parada de emergência não deve depender exclusivamente do software?

**Resposta:** Porque o microcontrolador, o programa ou o computador podem falhar. A função de emergência deve utilizar uma arquitetura de segurança apropriada e validada para os riscos envolvidos.

### 7.6 Fontes consultadas

* [Espressif — GPIO do ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html)
* [OMRON — Technical Guide](https://www.ia.omron.com/support/guide/index.html)
* [Schneider Electric](https://www.se.com/ww/en/)

---

## 8. Limitações das referências

As fontes reunidas oferecem fundamentos sobre sensores, GPIO, PWM e processamento de imagens. Entretanto, não substituem os manuais específicos dos componentes utilizados no protótipo.

Antes da montagem e dos testes, será necessário confirmar:

* Modelo, tensão e tipo de saída dos sensores.
* Compatibilidade elétrica entre sensores e ESP32-S3.
* Corrente de partida e requisitos do motorredutor.
* Corrente necessária para os servomotores.
* Compatibilidade do driver de potência.
* Pinagem efetiva da placa ESP32-S3.
* Parâmetros de imagem e critérios de classificação.
* Requisitos de segurança aplicáveis à montagem.

As configurações devem ser validadas por documentação técnica e testes supervisionados.

---

## 9. Conclusão

O estudo das fontes permitiu organizar conceitos fundamentais de automação industrial e relacioná-los ao desenvolvimento de uma esteira transportadora inteligente.

A integração de sensores, microcontrolador, acionamento de motores e visão computacional pode contribuir para a identificação e a separação de objetos. Entretanto, o desempenho depende do dimensionamento elétrico e mecânico, da qualidade dos sinais, da calibração dos sensores e da validação do software.

O material servirá como base de consulta para as próximas etapas do projeto, incluindo o planejamento das conexões, o desenvolvimento dos códigos e os testes do protótipo.

---

## 10. Próximas etapas

* [ ] Confirmar os modelos e as especificações dos componentes.
* [ ] Elaborar o diagrama elétrico.
* [ ] Definir as conexões entre sensores, ESP32-S3 e drivers.
* [ ] Testar individualmente os sensores.
* [ ] Testar o controle do motor e dos servomotores.
* [ ] Desenvolver o script de visão computacional.
* [ ] Implementar e testar a comunicação serial.
* [ ] Registrar resultados, falhas e ajustes.
* [ ] Atualizar a documentação com as evidências dos testes.

---

## 11. Fontes de pesquisa

* [OMRON — Overview of Proximity Sensors](https://www.ia.omron.com/support/guide/41/overview.html)
* [OMRON — Technical Guide](https://www.ia.omron.com/support/guide/index.html)
* [Espressif — GPIO & RTC GPIO: ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html)
* [Espressif — LED Control (LEDC): ESP32-S3](https://docs.espressif.com/projects/esp-idf/en/v5.5/esp32s3/api-reference/peripherals/ledc.html)
* [OpenCV — Documentação](https://docs.opencv.org/4.12.0/index.html)
* [Siemens SCE — Cursos de automação](https://www.siemens.com/pt-br/content/sce-educational-institutions/plc-courses/)
* [Schneider Electric](https://www.se.com/ww/en/)
* [YouTube — Identify Objects Moving on a Conveyor Belt Using OpenCV with Python](https://www.youtube.com/watch?v=Pysource)
* [YouTube — ESP32 Master Course: Hardware Interfacing](https://www.youtube.com/watch?v=GelmarTechLab)
* [YouTube — The Inductive and Capacitive Sensor](https://www.youtube.com/watch?v=MechatronicNeheyler)

---

*Material elaborado para fins de estudo e documentação do projeto de automação industrial. As informações técnicas devem ser conferidas nas referências oficiais e validadas antes da implementação física.*
