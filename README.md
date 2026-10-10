# Segundo Cérebro — Automação Industrial

## Sobre o projeto

Este projeto consiste no desenvolvimento de um segundo cérebro com apoio de inteligência artificial, utilizando o Gemini Notebook para reunir, organizar e consultar conhecimentos técnicos relacionados à Automação Industrial e à Eletromecânica.

O estudo tem como aplicação prática uma **esteira transportadora inteligente**, idealizada para identificar materiais metálicos e não metálicos e reconhecer objetos inesperados por meio de visão computacional.

## Objetivo

Construir uma base de conhecimento fundamentada em fontes técnicas confiáveis, capaz de auxiliar na compreensão dos componentes, na escolha de soluções, na integração entre hardware e software e na identificação de limitações e riscos durante o desenvolvimento do protótipo.

## Aplicação prática

A esteira inteligente está sendo estudada com os seguintes componentes e recursos previstos:

* **Microcontrolador:** ESP32-S3.
* **Acionamento:** motorredutor de corrente contínua de 12 V.
* **Detecção de materiais:** sensores para identificar características dos objetos, incluindo sensores indutivos para detecção de metais.
* **Direcionamento dos materiais:** estudo de servomotores SG90.
* **Visão computacional:** utilização da webcam de um notebook para análise de objetos inesperados.
* **Alimentação elétrica:** estudo de fontes, conversores de tensão e circuitos de acionamento.
* **Painel de comando:** botão liga/desliga e botão de emergência.

Os componentes e a arquitetura ainda precisam ser validados por testes. As características definitivas, ligações elétricas e resultados experimentais serão documentados conforme o desenvolvimento do protótipo.

## Ferramenta utilizada

O Gemini Notebook é utilizado para organizar as fontes, formular perguntas, produzir respostas fundamentadas, revisar conceitos e gerar materiais de estudo.

As respostas devem ser verificadas com base nas fontes adicionadas ao notebook. Quando uma informação não estiver disponível, essa limitação deve ser registrada, evitando especificações ou conclusões sem comprovação.

## Documentação técnica e cursos
## 📚 Fontes de pesquisa e materiais de estudo

As fontes abaixo foram reunidas no Gemini Notebook para apoiar os estudos sobre automação industrial e o desenvolvimento da esteira inteligente para identificação e separação de materiais.

### 📖 Documentação técnica e cursos

| Fonte              | Conteúdo                                                                     | Link                                                                                                                        |
| ------------------ | ---------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| OMRON              | Conceitos e funcionamento de sensores de proximidade                         | [Acessar documentação](https://www.ia.omron.com/support/guide/41/overview.html)                                             |
| OMRON              | Guias técnicos de sensores e automação industrial                            | [Acessar guia técnico](https://www.ia.omron.com/support/guide/index.html)                                                   |
| Espressif          | GPIO e RTC GPIO do ESP32-S3                                                  | [Consultar documentação](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-reference/peripherals/gpio.html) |
| Espressif          | LED Control (LEDC): controle de sinais PWM no ESP32-S3                       | [Consultar documentação](https://docs.espressif.com/projects/esp-idf/en/v5.5/esp32s3/api-reference/peripherals/ledc.html)   |
| OpenCV             | Tutoriais de visão computacional e processamento de imagens                  | [Acessar tutoriais](https://docs.opencv.org/4.12.0/index.html)                                                              |
| Siemens SCE        | Cursos e materiais educacionais sobre automação e controladores programáveis | [Acessar cursos](https://www.siemens.com/pt-br/content/sce-educational-institutions/plc-courses/)                           |
| Schneider Electric | Referência para soluções de energia e automação industrial                   | [Acessar site](https://www.se.com/ww/en/)                                                                                   |

### 🎥 Vídeos do YouTube

| Material                                                                                     | Tema                                                                         | Link                                                                       |
| -------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Identify Objects Moving on a Conveyor Belt Using OpenCV with Python                          | Identificação e rastreamento de objetos em esteiras com visão computacional  | [Assistir no YouTube](https://www.youtube.com/watch?v=Pysource)            |
| ESP32 Master Course – Episode 4: Hardware Interfacing – Sensors, Actuators & GPIO Interrupts | Integração de sensores, atuadores e GPIO com ESP32                           | [Assistir no YouTube](https://www.youtube.com/watch?v=GelmarTechLab)       |
| The Inductive and Capacitive Sensor – Different Types and Applications                       | Princípios de funcionamento e aplicações de sensores indutivos e capacitivos | [Assistir no YouTube](https://www.youtube.com/watch?v=MechatronicNeheyler) |

### 🎯 Aplicação das fontes no projeto

Os materiais foram selecionados para fundamentar os seguintes tópicos:

* **Sensores industriais:** estudo dos princípios de funcionamento dos sensores indutivos e capacitivos para auxiliar na identificação de materiais.
* **ESP32-S3:** compreensão das entradas e saídas digitais e da integração entre o microcontrolador e os componentes eletrônicos.
* **Controle de atuadores:** estudo de PWM e do controle de servomotores e outros dispositivos.
* **Visão computacional:** utilização do OpenCV com Python para analisar imagens capturadas por uma webcam e identificar objetos na esteira.
* **Automação industrial:** contato com conceitos, equipamentos e aplicações industriais presentes nos materiais da Siemens e da Schneider Electric.


## Perguntas de pesquisa

* Como funciona um sensor indutivo e quais são suas limitações na detecção de metais?
* Como o ESP32-S3 pode receber sinais de sensores e comandar atuadores?
* Quais cuidados devem ser considerados na alimentação do microcontrolador, dos motores e dos servomotores?
* Como a visão computacional pode ajudar a identificar objetos inesperados na esteira?
* Qual é a diferença entre o botão liga/desliga e o botão de emergência?
* Quais testes são necessários para validar o funcionamento integrado do protótipo?

## Materiais produzidos

Esta seção será atualizada à medida que os materiais forem gerados e revisados.

* [ ] Respostas do Gemini Notebook com referências às fontes.
* [ ] Mapa mental dos principais conceitos.
* [ ] Apresentação de slides.
* [ ] Resumo em áudio.
* [ ] Capturas de tela que comprovem o desenvolvimento do segundo cérebro.
* [ ] Registro dos testes, limitações e melhorias do protótipo.

## Segurança

O botão de emergência e o circuito de acionamento devem ser tratados como aspectos críticos de segurança. A implementação precisa ser avaliada conforme os riscos da máquina e os requisitos aplicáveis. Não se deve presumir que um comando de software no microcontrolador, isoladamente, garante uma parada de emergência segura.

## Conclusão

O projeto busca integrar pesquisa técnica, inteligência artificial e conhecimentos de Eletromecânica para apoiar o desenvolvimento de uma esteira transportadora inteligente. A documentação será ampliada conforme as pesquisas, os testes e os resultados forem obtidos.

## Repositório

https://github.com/niinofonseca/segundo-cerebro-automacao-industrial
