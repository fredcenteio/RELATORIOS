

**UNIVERSIDADE DE CABO VERDE** 

FACULDADE DE CIÊNCIAS E TECNOLOGIA 

**Desenvolvimento de um Sistema de Monitorização Ambiental da Qualidade do ar e Variáveis Meteorológicas** 

Edú Agnelo dos Reis Silva Jorge 

VERSÃO FINAL 

Relatório realizado no âmbito da 

Licenciatura em Engenharia Eletrotécnica e de Computadores 

Área Telecomunicações 

Orientador: Professor Mestre Mateus Andrade 

Coorientadores: Edson Moreno & Fred Centeio 



© Edú Agnelo dos Reis Silva Jorge, 2026 

ii 

## **Agradecimentos** 

Antes de mais, apresento o meu profundo agradecimento ao meu orientador, Professor Engenheiro Mestre Mateus Neves Andrade, pelo acompanhamento rigoroso, pelas valiosas orientações técnicas, pela disponibilidade e pelo apoio constante ao longo do desenvolvimento deste projeto. A sua orientação foi determinante para a concretização deste trabalho e para o meu crescimento académico e profissional. 

Expresso igualmente o meu sincero agradecimento ao Professor Gregory Jenkins pelo incentivo permanente, pela confiança demonstrada e pelo apoio técnico prestado durante o desenvolvimento do projeto. 

Aos meus colegas Edson Moreno e Fred Centeio, manifesto o meu reconhecimento pela colaboração, espírito de entreajuda e companheirismo no Laboratório de Telecomunicações. A partilha de conhecimentos, experiências e ideias foi essencial para ultrapassar diversos desafios e contribuiu significativamente para a evolução deste trabalho. Tenho aprendido muito com ambos, tanto a nível técnico como pessoal. 

Agradeço também a todos os docentes do curso de Engenharia Eletrotécnica – Telecomunicações da Universidade de Cabo Verde, pelos conhecimentos transmitidos ao longo da minha formação, os quais constituíram a base científica e técnica indispensável para a realização deste projeto. 

À minha família, e de forma muito especial aos meus pais, expresso a minha mais profunda gratidão pelo amor incondicional, pelo apoio, pela paciência, pelos sacrifícios e pela confiança depositada em mim ao longo de todo o meu percurso académico. Aos meus amigos, agradeço o incentivo, a compreensão e o apoio nos momentos mais exigentes. 

Por fim, deixo o meu sincero reconhecimento a todas as pessoas e instituições que, de forma direta ou indireta, contribuíram para a concretização deste projeto. O apoio e a colaboração de todos foram fundamentais para a realização deste trabalho. 

iii 

## **Resumo** 

A monitorização da qualidade do ar assume particular importância em Cabo Verde devido à ocorrência de episódios de bruma seca, associados ao transporte de poeiras provenientes do deserto do Saara, que podem afetar a saúde pública. Neste contexto, este trabalho apresenta o desenvolvimento de um Sistema de Monitorização Ambiental da Qualidade do Ar e Variáveis Meteorológicas, baseado em tecnologias de Internet das Coisas (IoT), destinado à recolha, transmissão, processamento e visualização de dados ambientais em tempo quase real. O sistema utiliza uma estação baseada no microcontrolador ESP32, equipada com sensores para medir as concentrações de material particulado (PM1.0, PM2.5 e PM10), a temperatura, a humidade relativa e a pressão atmosférica, permitindo ainda estimar a altitude. Na implementação experimental, os dados são transmitidos por Wi-Fi através do protocolo MQTT e encaminhados pelo Node-RED. Posteriormente, são processados pelo backend desenvolvido em Node.js e armazenados numa base de dados MongoDB. A informação é disponibilizada numa aplicação web desenvolvida em Angular, que permite visualizar as medições, consultar históricos, analisar gráficos e acompanhar os alertas gerados pelo sistema. A plataforma integra também funcionalidades de registo e gestão de múltiplas estações de monitorização, permitindo organizar os dados e comparar as condições ambientais de diferentes locais. O módulo GSM/GPRS SIM800L foi previsto como alternativa de comunicação para futuras implementações, mas não foi integrado nem validado experimentalmente no protótipo desenvolvido. 

A solução desenvolvida demonstra a aplicação de tecnologias IoT na monitorização ambiental, disponibilizando informação sobre a qualidade do ar e as condições meteorológicas. Constitui, assim, uma ferramenta de apoio à investigação e à análise dos fenómenos atmosféricos em Cabo Verde, com possibilidade de expansão futura da rede de estações. 

**Palavras-chave:** qualidade do ar, material particulado, monitorização ambiental, Internet das Coisas (IoT), sensores ambientais, Variáveis Meteorológicas, Cabo Verde. 

iv 

## **Abstract** 

Quality monitoring is of particular importance in Cape Verde due to the occurrence of "dry haze" (bruma seca) episodes—associated with the transport of dust from the Sahara Desert— which can impact public health. In this context, this work presents the development of an environmental monitoring system for air quality and meteorological variables, based on Internet of Things (IoT) technologies, designed for the collection, transmission, processing, and visualization of environmental data in near real-time. The system utilizes a station based on an ESP32 microcontroller, equipped with sensors to measure particulate matter concentrations (PM1.0, PM2.5, and PM10), temperature, relative humidity, and atmospheric pressure (which allows for altitude estimation). In the experimental implementation, data is transmitted via Wi-Fi using the MQTT protocol and routed through Node-RED. Subsequently, it is processed by a backend developed in Node.js and stored in a MongoDB database. The information is made available through a web application built with Angular, which enables users to view forecasts, consult historical data, analyze charts, and monitor alerts generated by the system. The platform also includes features for registering and managing multiple monitoring stations, facilitating data organization and the comparison of environmental conditions across different locations. A SIM800L GSM/GPRS module was included as a communication alternative for future implementations, though it was neither integrated into nor experimentally validated in the current prototype. 

The developed solution demonstrates the application of IoT technologies in environmental monitoring, providing information on air quality and environmental conditions. It serves as a tool to support research and the analysis of atmospheric phenomena in Cape Verde, with the potential for future expansion of the network of stations. 

**Keywords** : air quality, particulate matter, environmental monitoring, Internet of Things (IoT), environmental sensors, meteorological variables, Cape Verde. 

v 

## **Índice** 

|**Ca**|**pítulo 1 - Introdução............................................................................................................ 1**|
|---|---|
|1.1.|Enquadramento....................................................................................................................1|
|1.2.|Objetivo geral.......................................................................................................................3|
|1.3.|Objetivos específicos...........................................................................................................4|
|1.4.|Motivação............................................................................................................................4|
|1.5.|Estado da arte.......................................................................................................................4|
|1.6.|Estrutura do relatório...........................................................................................................6|
|**Ca**|**pítulo 2 – Fundamentação teórica......................................................................................8**|
|2.1.|Qualidade do ar e poluição atmosférica...............................................................................8|
|2.2.|Impactos da poluição atmosférica na saúde.........................................................................8|
|2.3.|Influência das variáveis meteorológicas na qualidade do ar................................................9|
|2.4.|Descrição da área de estudo.................................................................................................9|
|2.5.|Monitorização da qualidade do ar......................................................................................10|
|2.6.|Índice europeu da qualidade do ar.....................................................................................11|
|**Ca**|**pítulo 3 - Tecnologias e ferramentas................................................................................13**|
|**3.1. **|**Módulos de hardware**......................................................................................................13|
|3.1.1|.<br>Placa DOIT ESP32......................................................................................................13|
|3.1.2|.<br>Sensor DHT22.............................................................................................................15|
|3.1.3|.<br>BMP180.......................................................................................................................15|
|3.1.4|.<br>Módulo SIM800L........................................................................................................16|
|3.1.5|.<br>Sensor SPS30..............................................................................................................17|
|3.1.6|.<br>OPC-N3 (Optical Particle Counter)............................................................................18|
|3.1.7|.<br>PMS5003.....................................................................................................................19|
|3.1.8|.<br>Módulo carregador de bateria de íon lítio da Adafruit bq25185.................................20|
|3.1.9|.<br>Painel Solar..................................................................................................................21|
|3.1.1|0.<br>Open Log.....................................................................................................................22|
|**3.2. **|**Aplicações e Softwares**.....................................................................................................22|
|3.2.1|.<br>Mongo DB...................................................................................................................23|
|3.2.2|.<br>Node-RED...................................................................................................................24|
|3.2.3|.<br>Node.JS........................................................................................................................25|
|3.2.4|.<br>Angular........................................................................................................................26|
|3.2.5|.<br>Arduíno Integrated Development Environment..........................................................26|
|3.2.6|.<br>Visual Studio Code......................................................................................................26|
|3.2.7|.<br>Keycloak......................................................................................................................27|



vi 

|3.2.|8.<br>TypeScript...................................................................................................................27|
|---|---|
|**3.3. **|**Protocolos e interfaces de Comunicação........................................................................28**|
|3.3.|1.<br>Comunicação GPRS....................................................................................................28|
|3.3.|2.<br>Interface de Comunicação UART...............................................................................28|
|3.3.|3.<br>Interface de Comunicação I²C.....................................................................................29|
|3.3.|4.<br>Interface de Comunicação SPI....................................................................................29|
|3.3.|5.<br>Protocolo de comunicação HTTP e HTTPS................................................................30|
|3.3.|6.<br>Protocolo de Comunicação MQTT.............................................................................30|
|**Ca**<br>**ambie**|**pítulo 4 - Metodologia de desenvolvimento de um sistema de monitorização**<br>**ntal.......................................................................................................................................32**|
|4.1.|Estudo dos sensores de qualidade do Ar e meteorologia...................................................32|
|4.2.|Análise dos componentes eletrónicos e do sistema de alimentação..................................35|
|4.3.|Integração dos componentes do Sistema...........................................................................40|
|4.4.|Definição da conceção do protótipo..................................................................................41|
|4.5.|Análise e gestão de dados..................................................................................................42|
|4.6.|Tabela de Ligação..............................................................................................................42|
|4.7.|Análise financeira do protótipo..........................................................................................43|
|4.8.|Dimensionamento energético do sistema...........................................................................44|
|**Ca**|**pítulo 5 – Arquitetura do sistema de Comunicação........................................................53**|
|5.1.|Estrutura global do sistema................................................................................................53|
|5.2.|Estação de monitorização ambiental – AirCV - UNICV...................................................54|
|5.3.|Broker MQTT....................................................................................................................54|
|5.4.|Node-Red...........................................................................................................................54|
|5.5.|AirCV – Plataforma web de monitorização ambiental......................................................60|
|5.5.|1.<br>Comunicação entre Node-Red e Backend(Node.JS)...................................................60|
|5.5.|2.<br>Comunicação entre Backend(Node.JS) e API REST..................................................61|
|5.5.|3.<br>Comunicação entre Backend e Mongo DB.................................................................61|
|5.5.|4.<br>Comunicação entre Frontend e Backend.....................................................................62|
|5.6.|Fluxo geral do sistema.......................................................................................................62|
|5.7.|Servidor..............................................................................................................................63|
|**Ca**|**pítulo 6 –Testes funcionais e Análise dos Resultados.....................................................64**|
|6.1.|Implantação da estação e condições de teste.....................................................................64|
|6.2.|Âmbito dos ensaios: capacidade testadas e capacidades previstas....................................66|
|6.3.|Testes funcionais da plataforma AirCV.............................................................................68|
|6.4.|Desempenho da comunicação e da estação.......................................................................80|
|6.5.|Registo e integração de diferentes tipos de estações de monitorização ambiental............83|



vii 

|6.6. Análise geral do sistema....................................................................................................89|
|---|
|**Capítulo 7 – Conclusão e Trabalhos futuros........................................................................90**|
|7.1. Conclusão...........................................................................................................................90|
|7.2. Trabalhos futuros...............................................................................................................91|
|**Referências..............................................................................................................................92**|



viii 

## **Lista de figuras** 

|Figura 1 - Tempestade de bruma seca na cidade da Praia. Fonte: [2]...........................................1|
|---|
|Figura 2 - Localização geográfica de Cabo Verde. Fonte: [17]...................................................10|
|Figura 3 – Índice Europeu da qualidade do Ar<br>(µg/m<br>3<br>). Fonte:[21]...........................................12|
|Figura 4 - Placa DOIT ESP32 DevKit. Fonte: [23].....................................................................13|
|Figura 5 - Sensor DHT22. Fonte: [25].........................................................................................15|
|Figura 6 - Sensor BMP180. Fonte: [27].......................................................................................16|
|Figura 7 - Módulo GSM. Fonte: [29]...........................................................................................16|
|Figura 8 - PM Sensor. Fonte: [31]...............................................................................................17|
|Figura 9 - Sensor OPC-N3. Fonte: [33].......................................................................................18|
|Figura 10 - Sensor PMS5003. Fonte: [36]...................................................................................19|
|Figura 11 - Módulo carregador de bateria. Fonte:[38]................................................................20|
|Figura 12 - Painel fotovoltaico. Fonte: [40].................................................................................21|
|Figura 13 - Módulo SD Card Open Log. Fonte: [41]..................................................................22|
|Figura 14 - Ambiente gráfico do Node-Red. Fonte: [Autor].......................................................25|
|Figura 15 - Ambiente gráfico do Arduíno IDE. Fonte: [Autor]..................................................26|
|Figura 16 - Ambiente gráfico do VS Code. Fonte:[Autor]..........................................................27|
|Figura 17 - Página de autenticação do Keycloak. Fonte: [49].....................................................27|
|Figura 18 - Comunicação GPRS. Fonte: [53]..............................................................................28|
|Figura 19 - Estrutura na qual o protocolo UART atua. Fonte: [55].............................................29|
|Figura 20 - Estrutura na qual o protocolo I2C atua. Fonte: [56]..................................................29|
|Figura 21 - Comunicação entre Cliente-Servidor. Fonte: [59]....................................................30|
|Figura 22 - Protocolo HTTPS. Fonte: [60]..................................................................................30|
|Figura 23 - Funcionamento do protocolo MQTT. Fonte: [63]....................................................31|
|Figura 24 - Registo do monitor série no teste do OPC-N3. Fonte: [Autor].................................33|
|Figura 25 - Registo do monitor série no teste do SPS30. Fonte: [Autor]....................................33|
|Figura 26 - Registo do monitor série no teste do PMS5003, DHT22 e BMP180. Fonte: [Autor]<br>.............................................................................................................................................34|



ix 

|Figura 27 - Painel fotovoltaico de 6V, 9W. Fonte: [64]..............................................................36<br>Figura 28 - Módulo carregador de bateria. Fonte: [65]...............................................................37|
|---|
|Figura 29 - Bateria de iões de lítio. Fonte:[66]............................................................................37|
|Figura 30 - Divisor resistivo utilizado como sensor de tensão CC. Fonte: [Autor].....................38|
|Figura 31 - Voltímetro digital. Fonte: [Autor].............................................................................39|
|Figura 32 - Esquema do sistema de alimentação da estação. Fonte: [Autor]..............................39|
|Figura 33 - Esquema de ligação da estação de monitorização ambiental. Fonte: [Autor]...........41|
|**Figura 34**- Caixa da estação Quant AQ adaptada para o protótipo de monitorização<br>ambiental. Fonte [Autor].....................................................................................................42|
|Figura 35 - Evolução da tensão da bateria durante o processo de descarga. Fonte: [Autor].......48|
|Figura 36 - Variação da tensão da bateria em função do tempo, com intervalo de registo de<br>60 segundos. Fonte [Autor].................................................................................................49|
|Figura 37 - Arquitetura geral do sistema. Fonte: [Autor]............................................................53|
|Figura 38– Fluxo 1 de trabalho Node-RED. Fonte: [Autor]........................................................55|
|Figura 39 – Fluxo 2 de trabalho Node-RED. Fonte: [Autor].......................................................56|
|Figura 40 - Configuração da estação para comunicação com o Node-RED. Fonte: [Autor]......57|
|Figura 41 - Configuração do nó de entrada MQTT. Fonte: [Autor]............................................58|
|Figura 42 - Dados recebido no Node-RED. Fonte: [Autor].........................................................59|
|Figura 43 – Dados enviados para backend via API. Fonte: [Autor]............................................61|
|Figura 44 - Comunicação Frontend-Backend. Fonte[Autor].......................................................62|
|Figura 45 - Fluxo geral do sistema. Fonte: [Autor].....................................................................63|
|Figura 46  - Estação de monitorização ambiental. Fonte: [Autor]...............................................65|
|Figura 47 - Mapa interativo para visualização das estações de monitorização disponíveis.<br>Fonte: [Autor]......................................................................................................................69|
|Figura 48 – Autenticação Keycloak. Fonte: [Autor]...................................................................70|
|Figura 49 -  Registo de um novo sensor na plataforma. Fonte: [Autor]......................................71|
|Figura 50 - Área de gestão de sensores. Fonte: [Autor]..............................................................71|
|Figura 51 - Interface de configuração do sensor e ligação à API. Fonte: [Autor].......................72<br>Figura 52 - Módulo Mapa para visualização da localização da estação. Fonte: [Autor].............73|



x 

|Figura 53 - Módulo Mapa para visualização dos dados em tempo real. Fonte: [Autor].............74|
|---|
|Figura 54 – Geração automáticas das Alertas. Fonte: [Autor].....................................................75|
|Figura 55 – Módulo Alertas sobre estado de indisponibilidade da estação. Fonte: [Autor]........76|
|Figura 56 – Módulo Alertas sobre o estado da qualidade do ar. Fonte: [Autor].........................76|
|Figura 57  - Módulos de investigação: Análise de Dados . Fonte: [Autor].................................77|
|Figura 58 – Módulos de investigação: Históricos dos dados. Fonte: [Autor].............................78|
|Figura 59 - Estado operacional da estação de monitorização ambiental. Fonte: [Autor]............79|
|Figura 60 - indicadores calculados pelo fluxo Node-RED. Fonte [Autor]..................................81|
|Figura 61 – Registo da Estação Meteorológica. Fonte: [Autor]..................................................84|
|Figura 62 – API responsável para receção dos dados de Weather Station. Fonte: [Autor].........85|
|Figura 63 – Visualização dos dados em tempo real da Weather Station. Fonte: [Autor]............87|
|Figura 64 - Visualização dos dados em tempo real da Weather Station. Fonte: [Autor]............88|
|Figura 65 - Estado operacional da Weather Station. Fonte: [Autor]...........................................89|



xi 

## **Lista de tabelas** 

#### er 

|Tabela 1 - especificações da placa DOIT ESP32 DevKit. Fonte: [22] 14|
|---|
|Tabela 2 - Especificações técnica do sensor DHT22. Fonte: [24]...............................................15|
|Tabela 3 - Especificações técnica do sensor BMP180. Fonte: [26].............................................16|
|Tabela 4 - Especificações técnica do módulo GSM. Fonte: [28]................................................16|
|Tabela 5 - Especificações técnica do sensor SPS30. Fonte: [30]................................................17|
|Tabela 6 - Especificações técnica do sensor OPC – N3. Fonte: [34]..........................................18|
|Tabela 7 - Especificações técnica do sensor PMS5003. Fonte: [37]...........................................19|
|Tabela 8 - Especificações técnica do Módulo SD. Fonte:[41].....................................................22|
|Tabela 9 - Comparação dos sensores de material particulado. Fonte: [Autor], com base nas<br>fichas técnicas e nos testes preliminares.............................................................................34|
|Tabela 10 - Ligação de sistema de monitorização ambiental AirCV – UNICV. Fonte: [Autor] 43|
|Tabela 11  – Análise financeira do protótipo. Fonte: [Autor]......................................................44|
|Tabela 12   – Consumo estimado dos componentes da estação. Fonte: [Autor].........................45|
|Tabela 13 - Autonomia e consumo diário por cenário. Fonte: [Autor].......................................46|
|Tabela 14 - Tempo de descarga por intervalo de tensão (dados da Figura 36). Fonte: [Autor]. .49|
|Tabela 15 - Balanço energético diário do sistema Fonte: [Autor]...............................................51|
|Tabela 16 - Tópicos MQTT utilizados. Fonte: [Autor]...............................................................57|
|Tabela 17 – Grupos de campos do payload JSON. Fonte: [Autor].............................................59|
|Tabela 18 - Condições e configuração dos ensaios. Fonte [Autor].............................................66|
|Tabela 19 - Estado de validação das capacidades do sistema. Fonte [Autor]..............................67|
|Tabela 20 - Indicadores de desempenho da comunicação e da estação. Fonte [Autor]...............80|



xii 

## **Abreviaturas e Siglas** 

|API|Application Programming Interface|
|---|---|
|Dashboard|Painel de visualização de dados|
|Datalogger|Sistema de aquisição de dados|
|ESP32|Microcontrolador com conectividade Wi-Fi e Bluetooth|
|JSON|JavaScript Object Notation (Formato de intercâmbio de dados)|
|BSON|Binary JSON(<br>JSON binário)|
|IAQ|Índice da qualidade do ar|
|TLS|<br>Transport Layer Security|
|DC/CC|Corrente contínua|
|CRUD|Create, Read, Update and Delete (criar, ler, atualizar e eliminar)|
|ADC/DAC|Analog to Digital Converter/Digital to Analog Converter (Conversor Analógico-<br>Digital/Conversor Digital-Analógico)|
|GET|método HTTP usado para solicitar dados de um servidor de um servidor<br>(Hypertext Transfer Protocol GET)|
|POST|método HTTP usado para enviar dados a um servidor (Hypertext Transfer<br>Protocol POST|
|IBM|International Business Machines Corporation|
|RTC|Real Time Clock|
|UART|Universal Asynchronous Receiver/Transmitter|
|PM|Particulate Matter (Material Particulado)|
|PM<br>₁|<br>PM com diâmetro ≤ 1 µm|
|PM .<br>₂ ₅|PM com diâmetro ≤ 2,5 µm|
|PM₄|PM com diâmetro ≤ 4 µm|
|PM₁₀|Partículas com diâmetro ≤ 10 µm|



xiii 

|HTTPS|Hypertext Transfer Protocol Secure|
|---|---|
|PTS|Partículas Totais em Suspensão|
|Vcc|Tensão de alimentação|
|Vout|Tensão de saída|
|GPIO|General Purpose Input/Output (Entrada/Saída de Uso Geral)|
|SPI|Serial Peripheral Interface (Interface Serial Periférica)|
|PWM|Pulse Width Modulation (Modulação por Largura de Pulso)|
|GND|Ground (Terra, referência de potencial elétrico)|
|TCP|Transmission Control Protocol (Protocolo de Controle de Transmissão)|
|LED|Light Emitting Diode|
|LoRaWAN|Long Range Wide Area Network|
|GSM|Global System for Mobile Communications|
|GPRS|General Packet Radio Service|
|Kb/s|Kilo byte por segundo|
|µg/m³|microgramas por metro cúbico|
|MQTT|Message Queuing Telemetry Transport|
|NoSQL<br>|Not Only SQL (abordagem de bases de dados não relacionais)|
|IntelliSense|autocompletar inteligente de código|
|HTTP|Hypertext Transfer Protocol|
|V|Volt|
|A|Ampere|
|OASIS|Organization for the Advancement of Structured Information Standards|
|ISO|International Organization for Standardization|



xiv 

## **Capítulo 1 - Introdução** 

### **1.1.Enquadramento** 

Cabo Verde, devido à sua localização geográfica no Atlântico Oriental e à proximidade com o continente africano, é recorrentemente afetado por episódios de intrusão de massas de ar carregadas de poeiras minerais provenientes do deserto do Saara. Este fenómeno, conhecido localmente como bruma seca e associado aos ventos harmattan, ocorre com maior frequência entre os meses de dezembro e fevereiro[1]. Durante estes períodos, o ar torna-se visivelmente turvo, como ilustrado na **Figura 1** , devido à elevada concentração de partículas finas em suspensão. 



**Figura 1 -** Tempestade de bruma seca na cidade da Praia. Fonte: [2] 

A presença destas partículas pode reduzir a visibilidade, degradar a qualidade do ar e agravar problemas respiratórios e irritações oculares. Neste contexto, este trabalho visa contribuir para reduzir a falta de informação ambiental local, através do desenvolvimento de uma estação de monitorização de baixo custo, capaz de medir as concentrações de partículas (PM1.0, PM2.5 e PM10), a temperatura, a humidade relativa e a pressão atmosférica. Estes parâmetros permitem acompanhar parcialmente as condições atmosféricas, mas não abrangem todos os poluentes nem substituem as medições de referência ou uma avaliação dos riscos para a saúde. A solução pretende, assim, disponibilizar informação útil para a observação ambiental local e apoiar futuras análises da qualidade do ar em Cabo Verde. 

1 

### **Qualidade de ar em Cabo Verde** 

Para além da influência das massas de ar provenientes do Saara, a qualidade do ar em Cabo Verde é igualmente condicionada por fatores antropogénicos, como o aumento progressivo da frota automóvel, o crescimento das atividades industriais, secas longas e os efeitos das alterações climáticas. A conjugação destes elementos contribui para a deterioração gradual da qualidade do ar, reforçando a necessidade de estratégias eficazes de monitorização e gestão ambiental. 

De acordo com [3], no dia 5 de junho de 2019, assinalando o Dia Mundial do Ambiente, o Ministério da Agricultura e Ambiente, através da Direção Nacional do Ambiente (DNA), promoveu uma palestra subordinada ao tema _“_ Qualidade do Ar e Desafios do Desenvolvimento Sustentável _”_ . A sessão abordou os impactos das emissões provenientes do tráfego rodoviário na qualidade do ar e na saúde pública, bem como medidas preventivas alinhadas com as metas de desenvolvimento sustentável. Foram igualmente discutidos os efeitos da bruma seca sobre o setor agropecuário e sobre a qualidade ambiental do arquipélago. Segundo o Diretor Nacional do Ambiente, Alexandre Nevsky Rodrigues, embora os níveis atuais de poluição atmosférica em Cabo Verde não constituam uma ameaça grave, é fundamental reforçar políticas preventivas capazes de garantir a manutenção de padrões adequados para a proteção da saúde pública. Entre os principais desafios identificados destaca-se precisamente a bruma seca, caracterizada pela intrusão de poeiras minerais originadas na costa ocidental africana, que atinge o país de forma recorrente e prolongada, reduzindo significativamente a visibilidade e agravando os índices de partículas inaláveis no ar. Sandra Freire, docente do Departamento de Ciências e Tecnologias da Universidade de Cabo Verde, explica que as partículas finas transportadas pela bruma seca possuem dimensões suficientes para penetrar profundamente no sistema respiratório, podendo representar risco para doenças respiratórias e cardiovasculares. Apesar disso, ainda não existem dados quantitativos que permitam avaliar de forma conclusiva o impacto desse fenómeno na população cabo-verdiana [3]. 

Segundo Estudos realizados em Cabo Verde [4], há evidência de presença significativa de partículas finas em suspensão na atmosfera, frequentemente associadas a fenómenos meteorológicos regionais, como a bruma seca, bem como ao incremento das atividades antropogénicas. A literatura demonstra igualmente uma marcada variação sazonal na qualidade do ar, caracterizada por períodos de maior concentração de poeiras durante determinadas épocas do ano. Estas flutuações reforçam a importância de uma monitorização contínua, assegurando que a população se mantenha informada sobre potenciais riscos para a saúde pública e permitindo a implementação de medidas preventivas adequadas para preservar níveis de qualidade do ar considerados seguros. 

2 

De acordo com informações disponibilizadas pelo Instituto Nacional de Meteorologia e Geofísica [5], Cabo Verde dispõe de equipamentos especializados destinados à monitorização de parâmetros atmosféricos relevantes para a avaliação da qualidade do ar. Apesar de o país ainda não possuir um quadro regulamentar abrangente especificamente orientado para o controlo e gestão da poluição atmosférica, podem ser utilizadas como referência as diretrizes da Organização Mundial da Saúde (OMS), que estabelecem limites de exposição para diversos poluentes atmosféricos. 

Segundo [6], a qualidade do ar no arquipélago é influenciada de forma crescente pelas emissões provenientes do tráfego rodoviário, num contexto em que inexistem ainda mecanismos regulatórios específicos para o controlo destas emissões. Os resultados obtidos apontam para concentrações de poluentes, nomeadamente dióxido de azoto (NO2) e partículas inaláveis, que ultrapassam, em determinados períodos, os valores recomendados pela OMS. Tal cenário evidencia a necessidade urgente de fortalecer as estratégias de monitorização e de adotar políticas eficazes de gestão da qualidade do ar em Cabo Verde 

As estações convencionais permitem medir diferentes poluentes atmosféricos, mas os custos de aquisição, instalação e manutenção podem limitar a sua expansão. Neste contexto, a Internet das Coisas (IoT) tem favorecido o desenvolvimento de soluções de monitorização de baixo custo, que integram sensores, microcontroladores e sistemas de comunicação para recolher e transmitir dados ambientais. Estas soluções permitem distribuir pontos de monitorização e disponibilizar dados através de plataformas digitais. Contudo, a precisão e a fiabilidade dos sensores dependem das suas características, das condições ambientais e da calibração, devendo ser avaliadas, preferencialmente, por comparação com equipamentos de referência. 

A integração dos dados em plataformas digitais facilita a consulta de históricos e a análise da evolução das concentrações de partículas. Embora estes sistemas possam apoiar a observação ambiental, a previsão de episódios críticos exige métodos adicionais e validação específica. Assim, a monitorização baseada em IoT constitui uma solução complementar às estações convencionais, contribuindo para ampliar a disponibilidade de informação ambiental e apoiar a investigação sobre a qualidade do ar. 

### **1.2. Objetivo geral** 

O objetivo geral deste projeto consiste no Desenvolvimento de um Sistema de Monitorização Ambiental da Qualidade do Ar e Variáveis Meteorológicas. 

3 

### **1.3. Objetivos específicos** 

Os objetivos específicos definem as etapas necessárias para atingir o objetivo geral: 

- Projetar e implementar uma estação de monitorização ambiental de baixo custo, baseada no ESP32 e em sensores eletrónicos, para a medição das concentrações de material particulado (PM1.0, PM2.5 e PM10), da temperatura, da humidade relativa e da pressão atmosférica; 

- Desenvolver e implementar um mecanismo de comunicação por Wi-Fi para transmitir os dados recolhidos pela estação até à plataforma de monitorização, através de um protocolo de comunicação adequado; 

- Implementar uma plataforma de visualização e monitorização dos dados ambientais, permitindo a consulta das medições, a análise dos respetivos históricos e o acompanhamento da evolução dos parâmetros monitorizados. 

### **1.4. Motivação** 

A poluição atmosférica constitui uma das principais ameaças à saúde pública e ao equilíbrio ambiental, sobretudo em países em desenvolvimento, onde a monitorização ambiental permanece limitada devido ao elevado custo das estações tradicionais. Esta conjuntura evidencia a necessidade de soluções tecnológicas mais acessíveis, que possibilitem o acompanhamento contínuo dos níveis de poluentes e a identificação precoce de riscos para a população. Os avanços propiciados pela Internet das Coisas permitem atualmente conceber sensores de baixo custo que podem ser facilmente instalados em áreas urbanas ou remotas, formando redes descentralizadas de monitorização. Estas redes contribuem para uma análise mais abrangente dos fenómenos ambientais, fornecendo dados essenciais para investigadores, autoridades e cidadãos. 

Este projeto promove a utilização de tecnologias acessíveis na monitorização ambiental e na sensibilização para a qualidade do ar. O sistema desenvolvido permite acompanhar as concentrações de partículas e as condições meteorológicas, podendo apoiar atividades de investigação e aprendizagem. Contudo, por se tratar de uma solução indicativa, sem calibração e validação metrológica e regulamentar, os dados devem ser interpretados com prudência, não devendo fundamentar, isoladamente, decisões clínicas, avisos oficiais ou políticas públicas. 

### **1.5. Estado da arte** 

A monitorização de poluentes atmosféricos desempenha um papel crucial na avaliação da qualidade do ar, especialmente em regiões onde os impactos da poluição são mais expressivos. 

4 

Diversas investigações têm explorado soluções de baixo custo para suprir limitações associadas às estações de referência. No estudo referido em [7], foi desenvolvido um sistema de monitorização da qualidade do ar orientado para a deteção de material particulado em ambientes domésticos. O protótipo utilizou o microcontrolador ESP8266 em conjunto com o sensor DSM501, recorrendo à comunicação Wi-Fi para envio dos dados à plataforma ThingSpeak. A comparação entre os dados recolhidos e os registados por estações oficiais demonstrou que sistemas de baixo custo podem fornecer medições coerentes e operacionalmente úteis. Outro trabalho relevante é o de [8], que propôs uma estação de monitorização de baixo custo utilizando sensores da empresa Aphasense. O sistema integrou sensores dedicados à medição de diversos poluentes, incluindo CO (CO-B4), NO (NO₂ ₂-B43F), O (OX-B431) e partículas (HM3301 e₃ PMS5003). A transmissão de dados foi realizada via LoRaWAN, permitindo comunicação de longo alcance e baixo consumo. Para validação, aplicaram-se modelos de correção baseados em regressão linear, Random Forest e no algoritmo de compensação da Aphasense, destacando-se o Random Forest como o mais eficaz. Os resultados mostraram ainda que a humidade exerce forte influência nos sensores de baixo custo. No trabalho [9] foi desenvolvida uma estação de monitorização equipada com seis sensores (GP2Y1014AU0F, DSM501, MQ-7, GSNT11, SO₂AF e MiCS2610-11) destinados à deteção de PM₁₀, PM2.5, CO, NO₂, SO e O₂ ₃. O sistema utilizou o microcontrolador ATmega328P e o sensor DHT11 para temperatura e humidade. A transmissão dos dados ocorreu via protocolo Zigbee utilizando o módulo XBee S2C, sendo posteriormente disponibilizados numa aplicação móvel. A análise dos resultados evidenciou que variáveis ambientais, como temperatura, humidade relativa e velocidade do vento, influenciam significativamente as concentrações de PM2.5, além de demonstrarem que a localização dos pontos de medição afeta diretamente a qualidade dos dados obtidos. 

A análise comparativa permite verificar que os trabalhos estudados apresentam diferentes abordagens à monitorização da qualidade do ar, tanto ao nível dos sensores utilizados como das tecnologias de comunicação e das plataformas de disponibilização dos dados. Alguns concentram-se na deteção de poluentes específicos, enquanto outros abrangem um conjunto mais alargado de contaminantes atmosféricos. 

Neste contexto, o contributo do presente trabalho consiste no desenvolvimento de uma solução integrada de monitorização ambiental, que combina uma estação baseada no ESP32 com uma plataforma web para a recolha, transmissão, processamento, armazenamento e visualização dos dados. Para além da medição de material particulado, o sistema permite acompanhar variáveis meteorológicas, calcular um índice de qualidade do ar com base nas concentrações de PM2.5 e PM10 e consultar o histórico das medições. A integração com o AirCV permite ainda gerir múltiplas estações de monitorização numa única plataforma. 

5 

Assim, a principal diferenciação da solução desenvolvida reside na integração entre a estação de aquisição e a plataforma de gestão e visualização, proporcionando uma estrutura que pode ser expandida para incluir novas estações e variáveis ambientais. Contudo, ao contrário dos estudos que recorreram a estações oficiais ou a modelos de correção para validar as medições, a validação metrológica e a avaliação prolongada em ambiente exterior constituem aspetos que deverão ser aprofundados em trabalhos futuros. 

### **1.6. Estrutura do relatório** 

Este relatório está organizado em 7 capítulos 

#### **Capítulo 1 – Introdução** 

Neste capítulo é apresentado o enquadramento geral do tema, contextualizando o problema em estudo. São descritas a motivação, os objetivos do projeto, estado de arte e a estrutura do relatório. 

#### **Capítulo 2 – Fundamentação Teórica** 

Apresenta os conceitos fundamentais relacionados com a poluição do ar, os principais poluentes e os seus impactos na saúde. Aborda também as características geográficas e climáticas de Cabo Verde, os princípios da monitorização da qualidade do ar e o Índice de Qualidade do Ar. 

#### **Capítulo 3 – Tecnologias e ferramentas utilizadas** 

Apresenta as tecnologias, ferramentas e componentes utilizados no desenvolvimento do projeto, incluindo as respetivas aplicações e justificações técnicas. 

#### **Capítulo 4 – Metodologia de Desenvolvimento** 

Descreve a metodologia adotada no desenvolvimento do sistema, abrangendo a seleção dos sensores e componentes eletrónicos, o sistema de alimentação, a integração dos módulos, a conceção preliminar do protótipo, a aquisição, gestão e visualização dos dados, a tabela de ligações, a análise financeira e o dimensionamento energético. 

#### **Capítulo 5 – Arquitetura do Sistema** 

Descreve a arquitetura de comunicação e o fluxo de dados entre a estação de monitorização e a plataforma AirCV. São apresentados os principais componentes, nomeadamente o ESP32, MQTT, broker, Node-RED, backend em Node.js, MongoDB, frontend em Angular e servidor, bem como a integração entre os diferentes módulos. 

6 

#### **Capítulo 6 – Testes e Análise dos Resultados** 

Apresenta os testes realizados em ambiente controlado e a análise dos dados obtidos através da plataforma AirCV. Avalia a integração dos componentes, a estabilidade da comunicação, o funcionamento da interface, o desempenho do sistema e a gestão de utilizadores e perfis. 

#### **Capítulo 7 – Conclusões e Trabalhos Futuros** 

Sintetiza os principais resultados e conclusões do trabalho, identificando as limitações da solução desenvolvida e apresentando propostas de melhoria e trabalhos futuros. 

7 

## **Capítulo 2 – Fundamentação teórica** 

### **2.1. Qualidade do ar e poluição atmosférica** 

De acordo [10], a poluição atmosférica caracteriza-se pela alteração da composição natural do ar através da presença de substâncias químicas, físicas ou biológicas que comprometem a sua qualidade. Esta contaminação pode ocorrer em ambientes exteriores ou interiores e resulta de múltiplas fontes, destacando-se os sistemas de combustão domésticos, os veículos automóveis, as atividades industriais e os incêndios florestais. Entre os principais poluentes que constituem riscos significativos para a saúde humana incluem-se o material particulado (MP), o monóxido de carbono (CO), o ozono (O₃), o dióxido de azoto (NO₂) e o dióxido de enxofre (SO₂). A exposição prolongada a estes contaminantes, tanto em espaços abertos como fechados, encontrase associada ao desenvolvimento de doenças respiratórias e cardiovasculares e é considerada um dos fatores determinantes do aumento da mortalidade a nível global. 

### **2.2. Impactos da poluição atmosférica na saúde** 

Segundo a Organização Mundial da Saúde [11], aproximadamente 99% da população mundial vive em áreas onde a qualidade do ar não cumpre os padrões recomendados, e cerca de nove em cada dez pessoas respiram diariamente ar contaminado por substâncias tóxicas. Adicionalmente, a emissão de gases com efeito de estufa e os impactos ambientais associados agravam este problema, tornando a qualidade do ar uma questão de crescente preocupação a nível global. 

A poluição atmosférica é reconhecida como o segundo maior fator de risco global em termos de mortalidade e ocupa igualmente a segunda posição entre as principais causas de morte em crianças com menos de cinco anos, situando-se apenas atrás da desnutrição, devido aos seus efeitos adversos sobre a saúde respiratória e o desenvolvimento infantil. Em 2021, estimou-se que aproximadamente 8,1 milhões de óbitos tenham sido atribuíveis à poluição do ar, sendo que 58% destes estiveram relacionados com a exposição a partículas finas (PM2,5) [12]. A poluição do ar em ambientes exteriores constitui uma das maiores ameaças à saúde a nível global, afetando tanto áreas urbanas como rurais. De acordo com dados da Organização Mundial da Saúde[13], cerca de 4,2 milhões de mortes prematuras ocorrem anualmente devido à exposição a poluentes atmosféricos, em particular partículas inaláveis que penetram profundamente nos pulmões. Estes contaminantes estão fortemente associados ao desenvolvimento de doenças cardíacas, doenças respiratórias crónicas e determinados tipos de cancro, como o pulmonar. Das mortes atribuídas à poluição do ar, estima-se que aproximadamente 68% se devem a doenças cardiovasculares, incluindo enfarte agudo do miocárdio e acidente vascular cerebral; 14% a doença pulmonar obstrutiva crónica (DPOC); 14% a infeções respiratórias graves das vias respiratórias inferiores; 

8 

e 4% ao cancro do pulmão. Observa-se ainda uma desigualdade significativa no impacto da poluição atmosférica: a maioria das vítimas reside em países de rendimento baixo e médio, onde se registam 89% das mortes prematuras associadas à má qualidade do ar. As regiões do Sudeste Asiático e do Pacífico Ocidental apresentam os níveis mais elevados de exposição e mortalidade. Estes dados evidenciam a influência direta da poluição atmosférica na mortalidade global e na incidência de doenças cardiovasculares. 

Segundo estudos da Organização Mundial da Saúde [14], cerca de 2,4 mil milhões de pessoas utilizam combustíveis poluentes para cozinhar e aquecer as suas habitações, o que contribui para aproximadamente 3,2 milhões de mortes anuais atribuíveis à poluição do ar em ambientes domésticos. Esta exposição prolongada está associada ao aumento da incidência de doenças respiratórias crónicas, infeções das vias respiratórias inferiores, doenças cardiovasculares e cancro do pulmão, constituindo um problema significativo de saúde pública, sobretudo em populações vulneráveis. 

### **2.3. Influência das variáveis meteorológicas na qualidade do ar** 

Os parâmetros ambientais são fatores que influenciam diretamente a qualidade do ar. Estudos indicam que existe uma forte correlação entre as condições meteorológicas e a concentração de poluentes atmosféricos [15]. Entre as variáveis que podem intensificar a poluição destacam-se a velocidade do vento, a humidade relativa e as inversões térmicas em baixas altitudes. Como muitos poluentes atmosféricos afetam o clima e partilham fontes de emissão com gases de efeito de estufa, a redução da poluição do ar traduz-se em benefícios significativos para a saúde, o desenvolvimento sustentável e o ambiente. 

Cabo Verde não está imune aos desafios associados à qualidade do ar, influenciada tanto pela sua localização geográfica como pelas vulnerabilidades sociais e económicas do país. Estes fatores tornam a monitorização ambiental uma necessidade crescente, essencial para compreender e mitigar os impactos ambientais na saúde pública e no bem-estar das populações. 

### **2.4. Descrição da área de estudo** 

Cabo Verde é um pequeno país arquipelágico constituído por dez ilhas de origem vulcânica, organizadas em dois grupos: Barlavento e Sotavento. O grupo do Barlavento integra as ilhas de Santo Antão, São Vicente, Santa Luzia, São Nicolau, Sal e Boa Vista, enquanto o grupo do Sotavento compreende Maio, Santiago, Fogo e Brava. Além das ilhas principais, o arquipélago inclui diversos ilhéus, entre os quais se destacam Branco, Raso, Luís Carneiro, Grande e Ilhéu de Cima. Geograficamente, Cabo Verde situa-se entre as latitudes 14°23′ e 17°12′ N e as longitudes 

9 

22°40′ e 25°22′ W, aproximadamente a 500 km da costa ocidental africana  [16]. A **Figura 2** apresenta a localização geográfica de Cabo Verde no contexto do Oceano Atlântico. 



**Figura 2 -** Localização geográfica de Cabo Verde. Fonte: **[17]** 

Segundo o Plano Nacional de Adaptação às Mudanças Climáticas [18], Cabo Verde é um Pequeno Estado Insular em Desenvolvimento, situado na costa ocidental africana, com clima árido a semiárido, quente e seco. Apresenta duas estações: a chuvosa, de meados de julho a outubro, e a seca, no restante do ano. A temperatura média anual ronda os 25 °C, a humidade relativa varia entre 60 % e 85 % e a precipitação anual é inferior a 100 mm nas ilhas mais áridas, podendo atingir 600 mm nas ilhas montanhosas. A redução das chuvas e os períodos prolongados de seca agravam a vulnerabilidade climática do arquipélago. 

A qualidade do ar em Cabo Verde é afetada pela bruma seca, pelo aumento do tráfego rodoviário, pelo desenvolvimento industrial e pelas alterações climáticas. A bruma seca, resultante do transporte de poeiras do Saara, reduz a visibilidade e agrava problemas respiratórios, enquanto o tráfego contribui para a emissão de poluentes. Neste contexto, um sistema de monitorização ambiental baseado em sensores de baixo custo e tecnologias de informação e comunicação permite recolher e divulgar dados continuamente, apoiando a avaliação das condições atmosféricas, a proteção da saúde pública e a gestão ambiental. 

### **2.5. Monitorização da qualidade do ar** 

A monitorização da qualidade do ar pode ser realizada através de estações fixas, sensores portáteis e sistemas de monitorização móveis ou remotos. Estas tecnologias permitem avaliar a concentração de poluentes atmosféricos, como partículas em suspensão e gases, contribuindo para a análise das condições ambientais [19]. 

10 

#### **Estações de monitorização fixas** 

Instaladas em locais estratégicos, como zonas urbanas e áreas de tráfego intenso, estas estações realizam medições contínuas dos poluentes atmosféricos e de variáveis meteorológicas, como temperatura, humidade relativa e velocidade do vento. 

#### **Sensores portáteis** 

Os sensores portáteis permitem recolher dados em diferentes locais, sendo úteis em campanhas temporárias e na avaliação da distribuição espacial dos poluentes, complementando as medições das estações fixas. 

#### **<mark>Sistemas de monitorização remoto</mark>** 

Estes sistemas utilizam sensores instalados em veículos terrestres ou aéreos, como drones, permitindo realizar medições ao longo de diferentes trajetos e em locais de difícil acesso. 

### **Componentes da Estações de monitorização fixas** 

As estações automáticas de monitorização do ar possuem cabines climatizadas que abrigam 

os seguintes componentes [20]: 

- Monitores de partículas em suspensão: PM1, PM10, PM2,5; 

- Analisadores de gases: SO₂, CO, O e NO ;₃ ₓ 

- Sensores meteorológicos: temperatura, humidade, velocidade e direção do vento; 

- Datalogger com acesso à internet para recolha e transmissão contínua de dados, e demais materiais para o funcionamento do sistema. 

No âmbito deste projeto, o sistema de monitorização ambiental é baseado em princípios semelhantes aos das estações automáticas convencionais, mas utilizando sensores de baixo custo. Esta abordagem permite uma monitorização contínua e eficiente da qualidade do ar, adaptada às necessidades locais, com cobertura flexível e custos reduzidos, mantendo a relevância científica e prática do projeto. 

### **2.6. Índice europeu da qualidade do ar** 

Na ausência de um enquadramento regulamentar nacional abrangente, as diretrizes da Organização Mundial da Saúde constituem uma referência para a avaliação da qualidade do ar em Cabo Verde. Neste trabalho, utiliza-se o Índice Europeu de Qualidade do Ar (IEQA), com base nas concentrações de PM2,5 e PM10, os poluentes monitorizados pela estação desenvolvida. A classificação considera as concentrações horárias e as categorias definidas pela Agência 

11 

Europeia do Ambiente, prevalecendo a categoria mais desfavorável entre os dois poluentes [21]. A ausência de medições de NO₂, O e SO constitui uma limitação do sistema, uma vez que estes₃ ₂ poluentes também são relevantes para a avaliação da qualidade do ar. A **Figura 3** apresenta os intervalos de concentração e as respetivas categorias do índice. 



<!-- Start of picture text -->
Poluente Nivel do indice<br>(com base nas concentragées de poluentes em g/m3)<br>Extremamente<br>Bom Justo Moderado Pobre Muito ruim ‘<br>ruim<br>Particulas com<br>menos de2.5 |05 615 1650 51-90 91-140 140<br>im (PM 25)<br>Particulas com<br>menos de 10 |O45 16-45 46-120 421495 196-270 >270<br>im (PM 10)<br>Oz6nio(Os) |0460 (SET 101-120 421160 | 161-180 >180<br>Didxido de<br>nitrogénio (NO |O40 11.25 26-60 61-100 101-150 >150<br>2)<br>Diéxido<br>enxofre (SOde 2) 0-20 21-40 41425 926190 191-275 >275<br><!-- End of picture text -->

**Figura 3** – Índice Europeu da qualidade do Ar <mark>(µg/m</mark> 3 <mark>). Fonte:</mark> **[21]** 

As faixas do Índice Europeu de Qualidade do Ar estão alinhadas com as Diretrizes de Qualidade do Ar da Organização Mundial da Saúde (AQG 2021), particularmente no que respeita ao material particulado. 

12 

## **Capítulo 3 - Tecnologias e ferramentas** 

Este capítulo apresenta as principais tecnologias, componentes e ferramentas utilizadas no desenvolvimento do sistema de monitorização ambiental. 

### **3.1. Módulos de hardware** 

Os componentes físicos que integram o sistema foram criteriosamente escolhidos para garantir a recolha, processamento e transmissão de dados ambientais relevantes, incluindo concentrações de poluentes atmosféricos, temperatura, humidade e pressão atmosférica. 

#### **3.1.1. Placa DOIT ESP32** 

A placa de desenvolvimento DOIT ESP32 constitui o núcleo de processamento do sistema. Integra conectividade Wi-Fi e Bluetooth a 2,4 GHz, permitindo adquirir dados dos sensores, efetuar o seu processamento inicial e transmiti-los para plataformas externas, características que a tornam adequada a aplicações de Internet das Coisas (IoT) [22]. No sistema desenvolvido, o ESP32 comunica com os sensores através das interfaces correspondentes. A estabilidade das leituras depende também da alimentação elétrica, das ligações e do funcionamento dos sensores. A **Figura 4** apresenta a placa utilizada e a **Tabela 1** resume as suas principais características técnicas **.** 



<!-- Start of picture text -->
xm (1-06 D°-c)-a<br>as (2-0 ORB Gig Gior2s & vsri vos!<br>[Adcs_§ GioP: SEAR On| ___Efe) ofO~(35)}ege c iopzza © 2c scaC |<br>ees S)-0 Be 0-2) es<br>Paer § ciorss HI 5) Ce Gls peep re cco<br>qoe-es 3-7-0 Bs Ei) o-(22)- aco<br>[paca[DACT _@Sf AD C1ScIs g& G iorzsIOPZ5 SEBS)BAIEAJO=© ©)m/eLLL)matesneeh|Waien |S3 ORREOE©0 PSEPB cal closesioniors g&verso] vs risoxriss | |<br>[TOUCHES ADCIS & GioPi4 BEF) Bi: yy OfpaipO~(26) cae coreg Seeec<br>[cno SR I© yy of eng clove apcn groucn]<br>Cvinsv See © ft GRIP bx! of Remi cops Brash cx]<br><!-- End of picture text -->

**Figura 4** - Placa DOIT ESP32 DevKit. Fonte: **[23]** 

13 

**Tabela 1 - especificações** da placa DOIT ESP32 DevKit. Fonte: **[22]** 

|**Categoria**|**Itens**|**Especificação**|
|---|---|---|
||Protocolos|802.11 b/g/n (802.11n (2.4GHz) up to 150 Mbps)|
|WiFi|Frequência|2.4 GHz ~ 2.5 GHz|
||Protocolos|Bluetooth v4.2 BR/EDR e BLE|
|Bluetooth|Radio|Sensibilidade de receção de -97dbm e transmissão<br>+12dBm|
||Módulos de<br>interfaces|SD card, UART, SPI, SDIO, I2C, LED PWM, Motor<br>PWM, I2S, IR, pulse counter, GPIO, capacitive touch<br>sensor, ADC, DAC|
||Sensor|Hall Sensor, Touch Sensor|
||Cristal integrado|40 MHz|
||SPI flash integrado|4 MB|
||Tensão de operação|3.0 V ~ 3.6 V|
|Hardware|Corrente de<br>operação|80 mA (típico), até 240 mA em transmissão<br>Wi-Fi/Bluetooth, modem-sleep 20mA ~ 68mA,<br>Lightsleep 0,8mA, deep-sleep 10uA ~ 150uA|
||Temperatura de<br>operação<br>recomendada|–40 °C ~ +85 °C|
||CPU|Xtensa 32 bit. 448KB de ROM, 520KB de SRAM|
||RTC|16KB SRAM|



14 

#### **3.1.2. Sensor DHT22** 

O DHT22 é um sensor digital utilizado para medir a temperatura e a humidade relativa do ar. Integra um elemento capacitivo para medir a humidade e um elemento sensível à temperatura, transmitindo os valores através de uma interface digital [24]. No sistema desenvolvido, o DHT22 foi utilizado para obter dados de temperatura e humidade relativa, complementando as medições de partículas e pressão atmosférica realizadas pelos restantes sensores. A comunicação com o ESP32 é efetuada através de uma interface digital. A representatividade das medições no exterior depende das condições de instalação, nomeadamente da proteção contra a radiação solar direta e a chuva, da ventilação e do afastamento de fontes de calor. 



<!-- Start of picture text -->
2 iT22 pins ee<br>22 veeowTA eee~weosmae<br>4 onobead ~o6eemNS<br>~<br>; /}<br>ae ‘<br><!-- End of picture text -->

**Figura 5** - Sensor DHT22. Fonte: **[25]** 

**Tabela 2 -** Especificações técnica do sensor DHT22. Fonte: **[24]** 

|Modelo|AM2302 ou DHT22|
|---|---|
|Tensão da operação|3.3 - 6v|
|Faixa de medição de umidade|0 a 100%|
|Faixa de medição da temperatura|-40º a +80°C|
|Corrente|1 mA – 1,5 mA|



#### **3.1.3. BMP180** 

O BMP180 é um sensor digital utilizado para medir a pressão atmosférica e a temperatura. A partir da pressão medida, permite estimar a altitude através de uma relação barométrica, cujo resultado depende da pressão de referência ao nível do mar e das condições atmosféricas consideradas [26]. No sistema desenvolvido, o BMP180 foi utilizado para obter a pressão atmosférica e a temperatura, complementando as medições de humidade relativa e partículas realizadas pelos restantes sensores. A comunicação com o ESP32 é efetuada através da interface 

15 

I²C. A altitude calculada constitui uma estimativa e pode variar com as alterações meteorológicas da pressão, mesmo quando a estação permanece na mesma localização. 



<!-- Start of picture text -->
_—. aes<br>eR<br>=" Dune:(Osuoe (@))e330)<br>—= me pi:ost, a yeny ty23<br>—= Bains<br>=== Oyen *4O)<br><!-- End of picture text -->

**Figura 6 -** Sensor BMP180. Fonte: **[27]** 

**Tabela 3 - Especificações** técnica do sensor BMP180. Fonte: **[26]** 

|Modelo|BMP180|
|---|---|
|Tensão da operação|1.8 - 3.6v|
|Faixa de medição da temperatura|-40º a +85°C|
|Faixa de medição da pressão|300hPa a 1100hPa|
|Corrente|5uA|



#### **3.1.4. Módulo SIM800L** 

O SIM800L é um módulo de comunicação móvel que permite a transmissão de dados através de redes GSM/GPRS, suportando as bandas de frequência de 850, 900, 1800 e 1900 MHz [28]. Neste trabalho, foi considerado como uma alternativa à ligação Wi-Fi para a transmissão de dados através da rede móvel. Contudo, não foi utilizado experimentalmente, pelo que a comunicação GSM/GPRS não foi testada nem validada, permanecendo como uma funcionalidade prevista para desenvolvimento futuro. 



<!-- Start of picture text -->
ficou} oa<br>fxg H AD ae ierg ay Ue<br>>Pl cee feST PO.Oe Oeacces p=<br>mi pi) a] oS cs<br>> A <n ODO on Re Oo<br>E> Og 8 . 1 OFsox]<br><!-- End of picture text -->

**Figura 7 -** Módulo GSM. Fonte: **[29]** 

**Tabela 4 -** Especificações técnica do módulo GSM. Fonte: **[28]** 

16 



<!-- Start of picture text -->
Tensão de Operação 3.4V ~ 4.4V<br>2W para frequências de 850 e 900 MHz, e 1W para<br>           Potência de transmissão<br>as frequências de 1800 e 1900 MHz<br>Temperatura normal de operação -40ºC ~ +85ºC<br>downlink e uplink 85.6 kbps<br>Dados GPRS<br>Integra o protocolo TCP/IP<br>Interface SIM Suporte SIM Card<br>Corrente no modo sleep 1.88mA<br>Corrente no modo inativo 1.87mA<br>Corrente de pico 2A<br>.<br>3.1.5.Sensor SPS30<br>O  SPS30 , desenvolvido pela Sensirion, é um sensor digital que utiliza a dispersão de luz laser<br>para medir a concentração de partículas em suspensão no ar, disponibilizando valores de PM1,<br>PM2,5, PM4 e PM10 [35]. No projeto, foi considerado como uma opção para a monitorização de<br>partículas, tendo sido submetido a testes durante a fase de seleção dos sensores [30]. A  Figura 8<br>apresenta o sensor e a  Tabela 5  as suas principais especificações.<br><!-- End of picture text -->



**Figura 8** - PM Sensor. Fonte: **[31]** 

**Tabela 5 -** Especificações técnica do sensor SPS30 **.** Fonte: **[30]** 

|Modelo|PMS5003|
|---|---|
|Tensão da operação|4.5V – 5.5V|



17 

|Módulos de interfaces|UART e I2C|
|---|---|
|Faixa de concentração em massa|0 – 1000 ug/m3|
|Classes de concentração em massa|PM1, PM2.5, PM4 e PM10|
||PM1.0 → deteta partículas de 0,3 a 1,0 µm|
|Tamanho Faixa de partículas|PM2.5 → de**0**,3 a 2,5 µm<br>PM4 → de 0,3 a 4,0 µm|
||PM10 → de 0,3 a 10,0 µm|
|Modo sleep|38 ~ 50 uA|
|Modo inativo|300 ~ 360 uA|
|Modo de medição|45 ~ 65 mA|



#### **3.1.6. OPC-N3 (Optical Particle Counter)** 

O **OPC-N3** é um contador ótico de partículas que utiliza um feixe laser e um fotodetetor para estimar a dimensão e a concentração de partículas em suspensão no ar [32]. No projeto, foi considerado como uma opção para a monitorização de partículas, tendo sido submetido a testes durante a fase de seleção dos sensores. O sensor é apresentado na **Figura 9** , e a T **abela 6** descreve suas principais especificações. 



**Figura 9 -** Sensor OPC-N3. Fonte: **[33]** 

**Tabela 6 -** Especificações técnica do sensor OPC – N3 **.** Fonte: **[34]** 

18 

|Modelo|OPC – N3|
|---|---|
|Tensão da operação|4.8 – 5.2 V|
||SPI (modo 1, comunicação em tempo real)|
|Módulos de interfaces|Micro-USB (atualização de firmware e modo<br>standalone)|
|Faixa de concentração em massa|0 – 2000 ug/m3|
|Tamanho/Faixa de partículas|0.35 ~ 40um (cobre PM1, PM2.5, PM10 e até<br>partículas maiores)|
|Classes de concentração em massa|PM1, PM2.5 e PM10<br>PM4.25 como opção|
|Modo inativo / não medição|45mA|
|Modo de medição|180mA|



#### **3.1.7.  PMS5003** 

O **PMS5003** é um sensor digital que utiliza a dispersão de luz laser em que a luz refletida pelas partículas é analisada por um fotodetetor e processada internamente para determinar a dimensão e concentração das partículas [35]. No projeto, foi considerado como uma opção para a monitorização de partículas, tendo sido submetido a testes durante a fase de seleção dos sensores. A **Figura 10** apresenta o sensor e a **Tabela 7** as suas principais especificações. 



<!-- Start of picture text -->
ioe<br>aie 4 RESET<br>cr Og he<br>GND *% @<br>RK<br>SET<br><!-- End of picture text -->

**Figura 10** - Sensor PMS5003. Fonte: **[36]** 

**Tabela 7** - Especificações técnica do sensor PMS5003. Fonte: **[37]** 

Modelo PMS5003/Plantower 

19 



|Tensão da operação|4.5V – 5.5V|
|---|---|
|Módulo de interface|UART|
|Faixa máxima de concentração em<br>massa|≥ 1000 ug/m3|
|Classes de concentração em massa|PM1, PM2.5 e PM10|
||PM1.0 → detecta partículas de 0,3 a 1,0 µm|
|Faixa de medição de partículas|PM2.5 → de 1,0 a 2,5 µm|
||PM10→de 2,5 a 10,0 µm|
|Faixa de concentração eficaz|0 ~ 500 μg/m3|
|Modo espera|≤ 200 μA|
|Modo de medição|≤ 100 mA|



#### **3.1.8. Módulo carregador de bateria de íon lítio da Adafruit bq25185** 

O Adafruit bq25185 (modelo 6106) é um módulo carregador de baterias Li-Ion/Li-Po de uma célula, compatível com alimentação por USB Tipo-C, fonte DC ou painel solar. Incorpora um conversor elevador que disponibiliza uma saída estabilizada de 5 V, até 1 A [38]. No sistema desenvolvido, foi utilizado para gerir o carregamento da bateria de iões de lítio através do painel solar, permitindo a alimentação do sistema durante o funcionamento. A **Figura 11** apresenta o carregador utilizado no projeto. 



<!-- Start of picture text -->
a= a<br>REatone) Bie &<br>——= me ad Gin<br><!-- End of picture text -->

**Figura 11 -** Módulo carregador de bateria. Fonte: **[38]** 

20 

Principais características do módulo [38]: 

- **Múltiplas fontes de alimentação** : USB Tipo-C, fontes de corrente contínua (CC) e painéis solares. 

- **Carregador bq25185** : compatível com baterias Li-Ion/Li-Po de uma célula. 

- Conversor elevador TPS61023: saída estabilizada de 5 V, com corrente máxima de 1 A. 

- **Gestão de energia** : permite alimentar a carga e carregar a bateria simultaneamente. 

- **Carregamento solar** : regulação dinâmica da energia extraída do painel solar. 

- **Corrente de carregamento configurável** : 1 A ou 500 mA, através da alteração do jumper. 

- **Indicadores LED** : sinalização do estado de carregamento, de erro e da saída de 5 V. 

- **Entrada EN** : permite activar ou desativar o conversor de 5 V. 

- **Proteção** : mecanismos de proteção contra sobretensão, sobrecarga, curtocircuito e sobreaquecimento. 

- **Conectividade** : terminais JST-PH e conectores de parafuso para facilitar a ligação dos componentes. 

#### **3.1.9.Painel Solar** 

O painel solar fotovoltaico converte a radiação solar em energia elétrica através de células fotovoltaicas semicondutoras que geram corrente contínua quando expostas à luz solar [39]. No sistema desenvolvido, foi utilizado um painel para fornecer energia ao módulo carregador, contribuindo para a alimentação autónoma da estação de monitorização ambiental. 



<!-- Start of picture text -->
i Moldura de Aluminio<br>— Cr Vidro Especial<br>SS © Pelicula Encapsulante -EVA<br>OX<<—<<<<—- )——— CélulasFotovoltaicas<br>_SS)iS © —Caixa BacksheetPeliculade JungaoEncapsulante (fundo protetor)-EVA.<br><!-- End of picture text -->

**Figura 12** - Painel fotovoltaico. Fonte: **[40]** 

21 

### **3.1.10. Open Log** 

O módulo de cartão SD permite leitura e escrita em Cartões SD e funciona como armazenamento externo em microcontroladores, como Arduíno e ESP32 [41]. É ideal para registo local de dados, armazenamento de séries temporais longas e operação offline. Na **Figura 13** observa-se o módulo, enquanto a **Tabela 8** apresenta os seus detalhes técnicos. 



<!-- Start of picture text -->
; =,<br>)| (SaBQRCED,<br>IU<br>60.0660<br><!-- End of picture text -->

**Figura 13** - Módulo SD Card Open Log. Fonte: **[41]** 

**Tabela 8 -** Especificações técnica do Módulo SD. Fonte: **[41]** 

|Tensão de operação|3,3 – 5V|
|---|---|
|Corrente em modo repouso|2-3mA|
|Corrente<br>durante a gravação|10-20 mA|
|Alto desempenho|Opera a 16 MHz|
|Suporta cartão SD|Sim, até 32GB|
|Interface UART|VCC, GND, TX, RX, DTR|



### **3.2. Aplicações e Softwares** 

Com a expansão das soluções de Internet das Coisas (IoT) e dos sistemas de monitorização em tempo real, torna-se necessário lidar com grandes volumes de dados provenientes de sensores distribuídos. Nestes contextos, a utilização de bases de dados NoSQL, como o MongoDB, revelase particularmente vantajosa. 

22 

#### **3.2.1. Mongo DB** 

O MongoDB é um sistema de gestão de bases de dados NoSQL orientado a documentos. Ao contrário do modelo relacional tradicional, utiliza um esquema flexível **,** permitindo armazenar documentos com estruturas que podem variar dentro da mesma coleção. Os documentos são armazenados internamente no formato BSON (Binary JSON), que suporta diferentes tipos de dados, incluindo números, textos, datas, matrizes e documentos aninhados [42]. No sistema desenvolvido, o MongoDB foi utilizado para armazenar as leituras ambientais e as informações associadas às estações de monitorização. A utilização de documentos permite agrupar numa mesma estrutura os valores de temperatura, humidade, pressão e concentrações de partículas, juntamente com informações como a identificação da estação, localização e instante da medição. 

Um exemplo simplificado de documento utilizado no sistema é apresentado abaixo: 

{ "sensorId": "6a64ae570db0f7792bb16edf", "timestamp": "2026-11-04T17:16:50.695Z", "weather": { "temperature": 25.9, "humidity": 64.6, "pressure": <mark>1008.23</mark> , "altitude": <mark>41.87,</mark> }, "airQuality": { "pm1": 5.76, "pm25": 17.30, "pm10": 31.04, "no2": 0, "so2": 0, "co": 0, "aqi": "moderado", "deviceId": "ESP32-001", "location": "UNICV" }, "diagnostics": { "battery_voltage": 3.6, "panel_voltage": <mark>4.5</mark> , 

23 

"signal": 100, 

"bmp_status": true, "battery_status": "Baixa", "power_status": "Bateria baixa" } } 

O contrato de dados define a estrutura das mensagens utilizadas na comunicação entre o módulo de aquisição, o backend e a base de dados. Cada registo deve permitir identificar a estação, associar as medições a um instante temporal e representar os valores ambientais com tipos e unidades consistentes. 

Os campos relativos à temperatura, à humidade, à pressão atmosférica e à concentração de partículas devem ser representados por valores numéricos. O identificador da estação deve utilizar uma designação uniforme em todos os componentes do sistema. Do mesmo modo, o campo temporal deve seguir um formato consistente, preferencialmente ISO 8601, com indicação explícita do fuso horário. 

Os valores ausentes ou inválidos devem ser tratados de acordo com as regras de validação implementadas no backend, evitando que dados incompletos sejam interpretados como medições válidas. O exemplo de JSON apresentado tem finalidade ilustrativa; a estrutura definitiva deve corresponder ao payload recebido e ao documento efetivamente persistido no MongoDB. 

Este modelo permite organizar as medições ambientais de forma estruturada e facilita a consulta e análise das séries temporais produzidas pelas estações. 

Para o armazenamento de medições contínuas, o MongoDB disponibiliza ainda coleções de séries temporais (time series collections) [43], concebidas para dados associados a uma referência temporal, como os provenientes de sensores ambientais. 

#### **3.2.2. Node-RED** 

O **Node-RED** é um ambiente de desenvolvimento visual baseado em fluxos (flow-based programming), de código aberto, inicialmente criado pela equipa IBM Emerging Technology e atualmente mantido pela OpenJS Foundation [44]. A sua finalidade consiste em facilitar a integração entre dispositivos físicos, APIs, serviços web e sistemas distribuídos, recorrendo a uma interface gráfica intuitiva, conforme ilustrado na **Figura 14.** 

24 



**Figura 14** - Ambiente gráfico do Node-Red. Fonte: [Autor] 

**Segundo** [44] **, as principais Características do Node-Red são:** 

- **Edição visual baseada em navegador:** Permite criar, configurar e interligar fluxos por meio de um editor gráfico intuitivo, utilizando nós disponibilizados na paleta. A implementação das alterações é realizada com apenas um clique. 

- **Funções personalizadas em JavaScript:** O ambiente integra um editor de texto que possibilita a criação de lógica personalizada, ampliando a flexibilidade do sistema. 

- **Biblioteca integrada:** Disponibiliza um repositório interno para armazenamento e reutilização de funções, fluxos e modelos, agilizando o desenvolvimento. 

- **Baseado em Node.js:** tira partido de uma arquitetura leve, orientada a eventos e não bloqueante, adequada tanto para execução em nuvem como em dispositivos de baixo custo, como o Raspberry Pi. 

#### **3.2.3. Node.JS** 

O Node.js é um ambiente de execução de JavaScript utilizado no desenvolvimento do lado do servidor. Baseia-se num modelo assíncrono e orientado a eventos, adequado para aplicações que necessitam de gerir múltiplas operações de entrada e saída de forma eficiente [45]. 

No presente projeto, o Node.js foi utilizado no desenvolvimento do backend do AirCV, responsável por receber e processar os dados provenientes do Node-RED, calcular o índice de qualidade do ar, armazenar as leituras na base de dados MongoDB e disponibilizar os dados ao frontend através de uma API REST. 

25 

#### **3.2.4. Angular** 

O Angular é um framework de código aberto, desenvolvido pela Google, utilizado na criação de aplicações web estruturadas e escaláveis. Baseia-se em TypeScript e permite organizar as funcionalidades através de componentes reutilizáveis [46]. No projeto, foi utilizado no desenvolvimento da interface web, permitindo visualizar as estações de monitorização, consultar as medições ambientais, apresentar mapas e acompanhar o índice de qualidade do ar. A comunicação com o backend é realizada através de requisições HTTP à API REST. 

#### **3.2.5. Arduíno Integrated Development Environment** 

O **Arduíno IDE** é um ambiente de desenvolvimento integrado que disponibiliza uma interface simples e funcional para escrever, editar, compilar e carregar programas para controladores da plataforma Arduíno [47]. No projeto, foi utilizado para desenvolver e carregar o firmware no ESP32, bem como para monitorizar a comunicação série durante os testes dos 

sensores. Na **Figura 15** podemos observar o ambiente gráfico do Arduíno IDE: 



**Figura 15** - Ambiente gráfico do Arduíno IDE. Fonte: [Autor] 

#### **3.2.6.Visual Studio Code** 

**Visual Studio Code** é um editor de código-fonte gratuito e multiplataforma, desenvolvido pela Microsoft, que suporta diferentes linguagens de programação e ferramentas de desenvolvimento [48]. O Visual Studio Code foi utilizado como ambiente principal para editar e organizar o código do frontend Angular, do backend Node.js e dos restantes componentes de software do AirCV. Na **Figura 16** podemos observar o ambiente gráfico de programação do VS Code. 

26 



**Figura 16** - Ambiente gráfico do VS Code. Fonte:[Autor] 

#### **3.2.7. Keycloak** 

O **Keycloak** uma plataforma de código aberto destinada à gestão de identidade e controlo de acesso, permitindo implementar mecanismos de autenticação e autorização em aplicações e serviços [49].  No projeto, foi utilizado para gerir a autenticação dos utilizadores e controlar o acesso às funcionalidades restritas do AirCV. A **Figura 17** abaixo demonstra página de autenticação do Keycloak. 



<!-- Start of picture text -->
Sign in to your account<br>Password<br>°<br><!-- End of picture text -->

**Figura 17 -** Página de autenticação do Keycloak. Fonte: **[49]** 

#### **3.2.8.TypeScript** 

O TypeScript é uma linguagem baseada em JavaScript que adiciona tipagem de dados para tornar o código mais seguro e facilitar a deteção de erros durante o desenvolvimento [50]. No projeto, foi utilizado no desenvolvimento do frontend angular, contribuindo para a organização do código e a implementação das funcionalidades da interface de visualização do AirCV. 

27 

### **3.3. Protocolos e interfaces de Comunicação** 

Os protocolos de comunicação representam um conjunto de regras que padronizam a forma como as informações são enviadas e recebidas entre dispositivos em uma rede [51]. 

#### **3.3.1.Comunicação GPRS** 

O GPRS (General Packet Radio Service) é uma tecnologia de transmissão de dados baseada em comutação de pacotes sobre redes GSM, permitindo a comunicação de dispositivos através de redes móveis [52]. 



<!-- Start of picture text -->
_ o3<br><!-- End of picture text -->

**Figura 18** - Comunicação GPRS. Fonte: **[53]** 

No sistema desenvolvido, o GPRS foi considerado como uma possível alternativa à comunicação Wi-Fi, através do módulo SIM800L. Contudo, este módulo não foi instalado nem integrado no sistema, pelo que a comunicação através da rede móvel não foi implementada nem validada experimentalmente no âmbito deste trabalho. Assim, a utilização do GPRS é apresentada como uma possibilidade para trabalhos futuros, cuja implementação exigirá a integração do módulo no hardware e no firmware, bem como a realização de testes de ligação à rede móvel e de transmissão de dados. 

#### **3.3.2. Interface de Comunicação UART** 

O UART (Universal Asynchronous Receiver/Transmitter) é uma interface de comunicação serial assíncrona utilizada para a troca de dados entre dispositivos eletrónicos. A comunicação é realizada através das linhas TX e RX, juntamente com uma referência comum de massa (GND), não sendo necessário um sinal de clock partilhado [54]. No sistema desenvolvido, a interface UART foi utilizada na comunicação entre o ESP32 e o módulo OpenLog, permitindo o envio de dados para o respetivo registo local num cartão microSD. Esta comunicação possibilita o armazenamento das medições adquiridas pela estação, nomeadamente quando ocorrem falhas na comunicação MQTT, permitindo a sua posterior recuperação após o restabelecimento da ligação à rede. O módulo SIM800L foi considerado como uma possível alternativa para futuras 

28 

comunicações através da rede móvel, mas não foi instalado nem integrado no sistema, pelo que não foram realizados ensaios de comunicação UART ou de transmissão GPRS com esse módulo. 



<!-- Start of picture text -->
DCH<br>foo foe<br><!-- End of picture text -->

**Figura 19** - Estrutura na qual o protocolo UART atua. Fonte: **[55]** 

#### **3.3.3. Interface de Comunicação I²C** 

O **I²C (Inter-Integrated Circuit)** é uma interface de comunicação serial síncrona que utiliza duas linhas principais: SDA, responsável pela transmissão dos dados, e SCL, responsável pelo sinal de clock. Permite a ligação de vários dispositivos ao mesmo barramento, cada um identificado pelo respetivo endereço[56]. No sistema desenvolvido, o I²C foi utilizado para estabelecer a comunicação entre o ESP32 e o sensor BMP180, através dos pinos SDA e SCL configurados no microcontrolador. 



<!-- Start of picture text -->
Master ATSDA<br>Slave Slave Slave<br><!-- End of picture text -->

**Figura 20** - Estrutura na qual o protocolo I2C atua. Fonte: **[56]** 

#### **3.3.4. Interface de Comunicação SPI** 

O SPI (Serial Peripheral Interface) é uma interface de comunicação serial síncrona utilizada para a transferência de dados entre um dispositivo controlador e um ou mais dispositivos periféricos. A comunicação é realizada através das linhas SCK, MOSI, MISO e CS, permitindo uma transferência de dados geralmente mais rápida do que interfaces seriais simples [57]. No sistema desenvolvido, o SPI é utilizado na comunicação entre o ESP32 e o sensor de qualidade do ar OPC-N3, permitindo a aquisição de dados relativos às partículas atmosféricas. 

29 

#### **3.3.5. Protocolo de comunicação HTTP e HTTPS** 

O HTTP (Hypertext Transfer Protocol) é um protocolo da camada de aplicação utilizado na comunicação entre clientes e servidores, seguindo normalmente um modelo de pedido e resposta. É amplamente utilizado na implementação de serviços web e APIs [58]. No sistema desenvolvido, o HTTP é utilizado na comunicação entre os componentes de software, nomeadamente na transferência dos dados entre o Node-RED e o backend e no acesso do frontend à API REST disponibilizada pelo backend. 



**Figura 21** - Comunicação entre Cliente-Servidor. Fonte: **[59]** 

O HTTPS corresponde à utilização do HTTP sobre TLS, proporcionando cifragem e proteção dos dados durante a transmissão. A sua utilização é particularmente relevante no acesso às interfaces e serviços disponibilizados pelo sistema através da rede [58]. 



**Figura 22** - Protocolo HTTPS. Fonte: **[60]** 

#### **3.3.6. Protocolo de Comunicação MQTT** 

O MQTT (Message Queuing Telemetry Transport) é um protocolo leve de comunicação baseado no modelo publish/subscribe, adequado a aplicações IoT que necessitam de transmitir pequenas quantidades de dados entre dispositivos e servidores [61]. 

Neste modelo, os dispositivos publicam mensagens em tópicos, enquanto os clientes interessados subscrevem esses tópicos através de um broker MQTT. Neste modelo, os dispositivos publicam mensagens em tópicos, enquanto os clientes interessados subscrevem esses tópicos através de um broker MQTT. 

30 

No sistema desenvolvido, o MQTT é utilizado para a comunicação entre as estações de monitorização e o broker, permitindo a publicação das leituras ambientais e das informações de estado dos dispositivos. O Node-RED e os restantes componentes do sistema podem subscrever os tópicos correspondentes para receber e processar os dados. 

O MQTT disponibiliza três níveis de qualidade de serviço (QoS): QoS 0 (at most once), QoS 1 (at least once) e QoS 2 (exactly once). No sistema desenvolvido, foi utilizado QoS 2, garantindo a entrega da mensagem uma única vez ao nível do protocolo MQTT, embora não assegure, por si só, o processamento único ou o armazenamento correto dos dados na aplicação. O tratamento de falhas de conectividade e o reenvio de registos através do cartão SD dependem dos mecanismos efetivamente implementados [62] **.** 



<!-- Start of picture text -->
— punish 8 1]<br>/ j= \ jubscriber<br>/ iC) & \ Pubish Data MaTT<br>ae<br>MO S/ Sie “a<br>Publisher :<br>Subseriber<br><!-- End of picture text -->

**Figura 23** - Funcionamento do protocolo MQTT. Fonte: **[63]** 

31 

# **Capítulo 4** - **Metodologia de desenvolvimento de um sistema de monitorização ambiental** 

A metodologia de desenvolvimento foi organizada em etapas que abrangeram a seleção dos sensores, a análise dos componentes eletrónicos e da alimentação, a integração do protótipo, a gestão dos dados e a avaliação técnica e energética do sistema. 

### **4.1. Estudo dos sensores de qualidade do Ar e meteorologia** 

A primeira etapa metodológica consistiu no estudo e na análise comparativa dos sensores de material particulado e de parâmetros meteorológicos. Para garantir uma seleção objetiva, foram definidos previamente os seguintes critérios de avaliação: 

- C1 – funcionamento estável durante o período de teste (critério eliminatório); 

- C2 – plausibilidade das leituras (valores dentro dos limites físicos e coerentes entre leituras 

   - consecutivas); 

- C3 – grandezas medidas (frações PM1,0, PM2,5 e PM10); 

- C4 – exatidão declarada pelo fabricante; 

- C5 – interface de comunicação e facilidade de integração com o ESP32; 

- C6 – consumo energético em funcionamento; 

- C7 – custo de aquisição; 

- C8 – vida útil declarada. 

Para a medição de material particulado, foram avaliados os sensores SPS30 (Sensirion), OPCN3 (Alphasense) e PMS5003 (Plantower). A avaliação combinou a análise das fichas técnicas (critérios C3 a C8) com testes preliminares de integração em laboratório (critérios C1 e C2), nos quais cada sensor foi ligado individualmente ao ESP32 e as leituras foram acompanhadas através do monitor série. 

#### **Resultados dos testes preliminares** 

O OPC-N3 inicializou corretamente (ventoinha e laser ativos) e forneceu leituras plausíveis (Figura 25): PM1,0 = 13,13 µg/m³, PM2,5 = 47,04 µg/m³ e PM10 = 96,66 µg/m³, com temperatura interna de 38,81 °C, humidade relativa interna de 30,30 %, caudal de amostragem de 5,12 mL/s e período de amostragem de 29,9 s, bem como a distribuição de contagens pelas 24 classes granulométricas. O sensor manteve-se operacional durante o ensaio [indicar duração e número de amostras], sem leituras inválidas, pelo que cumpriu os critérios C1 e C2. As concentrações de PM2,5 e PM10 são superiores às registadas com o SPS30 e com o PMS5003; 

32 

contudo, como os ensaios decorreram em dias diferentes, esta diferença não permite comparar o desempenho dos sensores). 



<!-- Start of picture text -->
last_PM1: 13.13209<br>last_PM2_5: 47.03521<br>last_PM1@: 96.65918<br>opc_temp: 38.80827<br>opc_hum: 30.30136<br>opc_flow_rate: 5.12<br>opc_sampling period: 29.9<br>ope_fan_rev: @<br>opc_laser_status: 587<br>> ope bins: array[241<br><!-- End of picture text -->

**Figura 24 -** Registo do monitor série no teste do OPC-N3. Fonte: [Autor] 

O SPS30 produziu leituras plausíveis e coerentes, por exemplo, PM1,0 = 3,69 µg/m³, PM2,5 = 5,51 µg/m³, PM4,0 = 6,80 µg/m³ e PM10 = 7,06 µg/m³, que foram publicadas com sucesso via MQTT ( **Figura 25** ). Contudo, durante o ensaio, o sensor deixou de responder à comunicação UART/SHDLC, não tendo sido possível confirmar a causa da falha. Assim, o SPS30 não cumpriu o critério eliminatório C1 e foi excluído da seleção final por razões operacionais. Esta decisão é provisória, uma vez que não foram recolhidos dados suficientes para distinguir uma eventual falha do sensor de problemas de alimentação, cablagem ou comunicação. A identificação da causa exigiria novos ensaios com alimentação e ligações controladas. 



<!-- Start of picture text -->
12:48:07,123 -><br>12:48:07.123 -> # Leitura dos Sensores:<br>12:48:07.123 -> DHT22 -> Temp: 23.90 °C | Hum: 41.00 %<br>12:48:07.123 -> BMP180 -> Temp: 23.60 °C | Press: 1007.16 hPa | Alt: 50.91 m<br>12:48:07.123 -> SPS30 -> PMl: 3.69 | PM2.5: 5.51 | PM4: 6.80 | PM10: 7.06<br>12:48:07,160 -> -------------------<br>12:48:07.160 -> # Publicando dados no MOTT...<br>12:48:12.004 -> Sending: 0x7E 0x00 0x03 0x00 OxFC Ox7E<br>12:48:12,118 -> Received: Ox7E 0x00 0x03 0x00 0x28 Ox40 Ox72 OxCS Ox68 Ox40 OxBE<br>12:48:12.118 -><br>12:48:12.118 -> # Leitura dos Sensores:<br>12:48:12.118 -> DHT22 -> Temp: 23.90 °C | Hum: 40.90 §<br>12:48:12.118 -> BMP180 -> Temp: 23.60 °C | Press: 1007.13 hPa | Alt: 51.41 m<br>12:48:12,162 -> SPS30 -> PMI: 3.79 | PM2.5: 5.66 | PM4: 6.99 | FMIO: 7.25<br><!-- End of picture text -->

**Figura 25 -** Registo do monitor série no teste do SPS30. Fonte: [Autor] 

O PMS5003 foi inicializado através da interface UART e apresentou leituras plausíveis (por exemplo, PM1,0 = 5 µg/m³, PM2,5 = 8 µg/m³ e PM10 = 9 µg/m³), mantendo-se operacional durante o ensaio e permitindo a publicação dos dados via MQTT ( **Figura 26** ). Estes resultados sustentaram a sua seleção para a implementação do sistema. Contudo, na ausência de registos estatísticos e de ensaios comparativos simultâneos com um equipamento de referência, não é 

33 

possível confirmar a estabilidade a longo prazo nem concluir que o sensor apresenta exatidão ou desempenho equivalente ao SPS30. 



<!-- Start of picture text -->
Saida Monitor Serial x<br>Nao conectado. Selecione uma placa e uma porta para conectar automaticamente<br>2ef40r40.9U9 “> LuiUsasszanuL FnDDUUS~..<br>14:16:21.525 -> PMSS5003 inicializado via UART.<br>14:16:21.525 -> ¥ Conectando ao MQTT...conectado!<br>14:17:13.963 -> # Leitura dos Sensores:<br>14:17:13.963 -> DHT22 -> Temp: 25.5 °C | Hum: 44.8 %<br>14:17:13.963 -> BMP180 -> Temp: 26.2 °C Press: 1006.8 hPa Alt: 54.7 m<br>14:17:13.963 -> PMSSO03 -> PM1.0: 5 PM2.5: 8 PM10: 9<br><!-- End of picture text -->

**Figura 26 -** Registo do monitor série no teste do PMS5003, DHT22 e BMP180. Fonte: [Autor] 

A **Tabela 9** sintetiza a comparação dos três sensores segundo os critérios definidos. 

**Tabela 9 -** Comparação dos sensores de material particulado. Fonte: [Autor], com base nas fichas técnicas e nos testes preliminares 

|**Critério**|**SPS30**|**OPC-N3**|**PMS5003**|
|---|---|---|---|
|C1 –|Não – deixou de|||
|Funcionamento<br>estável<br>(eliminatório)|funcionar ao longo do<br>tempo|Sim|Sim|
|C2 – Plausibilidade|Sim, enquanto|||
|das leituras|funcionou|Sim|Sim|
|C3 – Grandezas<br>medidas|PM1,0, PM2,5,<br>PM4,0, PM10|PM1,0, PM2,5,<br>PM10 (24 classes,<br>0,35–40 µm)|PM1,0, PM2,5, PM10<br>(contagens 0,3–10<br>µm)|
|C4 – Exatidão||||
|declarada|[ver tabela 5]|[ver tabela 6]|[ver tabela 7]|
|C5 – Interface|UART ou I²C|SPI|UART TTL 3,3 V|
|C6 – Consumo em<br>medição|45 ~ 65 mA a 5 V|≈ 180 mA a 5 V|≤ 100 mA a 5 V|
|C7 – Custo unitário|€ 54,85|539,01 €|35,05 €|
|C8 – Vida útil||Fabricante não||
|declarada|> 10 anos|especifica vida útil|≥ 3 anos|



34 

|**Critério**|**SPS30**|**OPC-N3**|**PMS5003**|
|---|---|---|---|
|||em anos||
|Resultado|Excluído (C1)|Selecionado|Apto - alternativa|



#### **Decisão final** 

O SPS30 foi excluído por não cumprir o critério eliminatório C1, pelo que o OPC-N3 e o PMS5003 reuniram condições para integrar o protótipo. Selecionou-se o OPC-N3 pela maior resolução granulométrica (24 classes, 0,35–40 µm), que permite caracterizar a distribuição de tamanhos das partículas e não apenas as frações PM1,0, PM2,5 e PM10, e pelos parâmetros de diagnóstico que disponibiliza (caudal de amostragem, período de amostragem, estado do laser e temperatura e humidade internas), úteis para validar cada leitura. Reconhecem-se, porém, as suas limitações: o consumo em medição (≈ 180 mA a 5 V) é quase o dobro do PMS5003, o que obriga a dimensionar a alimentação solar para esta carga ou a operar o sensor em ciclos de medição, desligando a ventoinha e o laser entre leituras; o custo unitário é mais elevado; não mede a fração PM4,0; e a interface SPI é mais exigente na integração do que a UART. Como acontece com os sensores óticos de baixo custo em geral, as concentrações podem ser sobrestimadas com humidade relativa elevada, pelo que as medições de PM devem ser interpretadas como indicativas até serem calibradas por comparação com um equipamento de referência. O PMS5003 fica identificado como alternativa: cumpriu C1 e C2, tem menor consumo e menor custo e liga-se diretamente ao ESP32 por UART, sendo a opção indicada caso o balanço energético do sistema solar não comporte o OPC-N3. 

Para os parâmetros meteorológicos foram selecionados o DHT22 (temperatura e humidade relativa) e o BMP180 (pressão atmosférica, temperatura e altitude estimada), que forneceram leituras plausíveis e coerentes entre si durante os testes, com uma diferença de temperatura de cerca de 0,3 °C entre os dois sensores ( **Figura 26** ). 

### **4.2. Análise dos componentes eletrónicos e do sistema de alimentação** 

A segunda etapa metodológica centrou-se no estudo dos componentes eletrónicos e do sistema de alimentação autónoma. Foi analisada a seleção do microcontrolador, tendo sido escolhido o ESP32 devido ao seu baixo consumo, capacidade de processamento, interfaces de comunicação e compatibilidade com sensores ambientais. Este atua como unidade central de aquisição, processamento e transmissão de dados. 

Com base na análise energética dos componentes, foi definida uma Arquitetura de alimentação solar adequada a locais remotos, tendo em conta a elevada disponibilidade de 

35 

radiação solar em Cabo Verde. Contudo, a autonomia efetiva do sistema não foi validada através de ensaios prolongados no exterior, dependendo do consumo energético e das condições meteorológicas. 

#### **Arquitetura do Sistema de Alimentação** 

O sistema de alimentação integra um painel fotovoltaico, uma bateria recarregável de iões de lítio e o módulo de carregamento Adafruit bq25185 (modelo 6106), que gere o carregamento da bateria e disponibiliza uma saída estabilizada de 5 V para alimentar os componentes da estação. Contudo, a estabilidade do fornecimento de energia durante a noite e em condições de baixa luminosidade depende da capacidade da bateria e do consumo do sistema, não tendo sido validada através de ensaios prolongados. 

Nesta metodologia, foi selecionado um painel fotovoltaico de 6 V e 9 W, constituído por células monocristalinas, com uma corrente nominal aproximada de 1,5 A ( **Figura 27** ). Este painel é responsável pela captação da energia solar e pelo fornecimento da energia primária ao sistema. 





**Figura 27 -** Painel fotovoltaico de 6V, 9W. Fonte: **[64]** 

Para a gerir o carregamento da bateria, foi utilizado o módulo Adafruit bq25185 (modelo 6106), compatível com alimentação através de painel solar. Este módulo regula o processo de carregamento, limita a corrente, integra proteção térmica e permite alimentar a carga enquanto a bateria é carregada ( **Figura 28** ) 



<!-- Start of picture text -->
O=mcm ImIgIO%<br>eee en<br>RESize * |<br>(Oe900°990S90)3, ea ar Fea ¢<br>222229 2h<br>Witt<br>| l l l l<br><!-- End of picture text -->



<!-- Start of picture text -->
QO seiery<br>DetachChorgerA cere& ete51 B00"= @)<br>@onpd — teeSet ivmper gt)<br>SsvoullAme) . BO<br>323aa8e@0<br>0999,9,0:909900000() 29 9O<br><!-- End of picture text -->

**Figura 28 -** Módulo carregador de bateria. Fonte: **[65]** 

36 

#### **Armazenamento e Regulação de Energia** 

O O armazenamento de energia é realizado através de uma bateria Li-Po de 3,7 V e 6600 mAh ( **Figura 29** ). O módulo Adafruit bq25185 (modelo 6106) gere o carregamento da bateria e disponibiliza uma saída estabilizada de 5 V para alimentar o ESP32, o sensor de partículas, o módulo de cartão SD e ESP 32 alimenta outros sensores com 3,3V. O SIM800L não foi instalado nem integrado no sistema, pelo que a sua alimentação não foi testada. 



**Figura 29 -** Bateria de iões de lítio. Fonte: **[66]** 

#### **Monitorização da Tensão do Sistema** 

Com o objetivo de acompanhar o desempenho energético da estação, foi estudada a implementação de um sistema de monitorização da tensão elétrica. A metodologia adotada baseia-se no princípio do divisor resistivo, permitindo reduzir valores de tensão contínua a níveis compatíveis com o conversor analógico-digital (ADC) do ESP32. 

O divisor de tensão foi dimensionado para limitar a tensão aplicada à entrada ADC do ESP32 a menos de 3,3 V, para tensões de entrada até 20 V ( **Figura 30** ). Esta solução permite monitorizar a tensão do painel solar, da bateria e da alimentação do sistema. A precisão das leituras depende dos valores e tolerâncias dos resistores, da configuração e calibração do ADC e das ligações de massa, devendo ser verificada experimentalmente. 

37 



<!-- Start of picture text -->
GND<br><!-- End of picture text -->

**Figura 30** - Divisor resistivo utilizado como sensor de tensão CC. Fonte: [Autor] 

Na **Figura 30** é possível observar a presença de dois resistores fundamentais para o circuito. Assim, aplica-se a teoria de análise de circuitos elétricos com o objetivo de determinar a tensão de saída VOUT. Dessa forma, obtém-se a seguinte expressão: 



Com base na equação apresentada, é possível determinar os valores adequados para os resistores R1 e R2, de forma que a tensão de saída Vout atinja o valor máximo de 3,176 V para uma tensão de entrada VCC específica. 

Neste caso, foi estabelecido que a tensão máxima a ser medida corresponde a VCC=20V. Assim, quando essa tensão atingir seu valor máximo, o microcontrolador deverá receber no pino de leitura uma tensão máxima de 3,176 V. 

Dessa maneira, considerando VCC=20V e Vout=3,176V, pode-se calcular os valores apropriados das resistências do divisor de tensão. Para efeito de projeto, define-se inicialmente R2=10kΩ (kilo ohm). 



A tensão máxima prevista para o pino do ESP32 é de 3,176 V. Contudo, devido à indisponibilidade do resistor de 53 kΩ, foi utilizado um resistor de 56 kΩ em seu lugar. Com essa alteração, o divisor de tensão passou a fornecer uma tensão máxima no pino do ESP32 de aproximadamente 3,026 V, quando a tensão de entrada (VCC) é 19,97 V. Essa variação é mínima 

38 

e permanece dentro de uma faixa segura para o funcionamento do microcontrolador, garantindo a integridade do pino de leitura analógica. 



**Figura 31** - Voltímetro digital. Fonte: [Autor] 

A montagem do circuito ( **Figura 32** ) foi concebida para permitir a monitorização de diferentes pontos do sistema de alimentação da estação, nomeadamente a tensão proveniente do painel solar, a tensão da bateria e a tensão de saída que alimenta o ESP32 e os restantes módulos eletrónicos. A precisão das medições depende da tolerância dos resistores e da configuração do ADC do ESP32, devendo ser validada por comparação com um multímetro de referência. Na ausência desses ensaios, esta funcionalidade deve ser considerada preliminar. 



<!-- Start of picture text -->
& z<br>co<br><!-- End of picture text -->

**Figura 32 -** Esquema do sistema de alimentação da estação. Fonte: [Autor] 

39 

### **4.3.Integração dos componentes do Sistema** 

A etapa de integração do protótipo envolveu a ligação dos sensores ambientais ao ESP32 e a configuração da comunicação entre os módulos do sistema. O ESP32 foi utilizado como unidade central de controlo devido à sua compatibilidade com sensores e capacidade de comunicação WiFi, permitindo a transmissão contínua dos dados recolhidos. 

A integração permitiu a aquisição, processamento e envio das medições para o sistema de gestão de dados, incluindo o Node-RED e a aplicação frontend desenvolvida em Angular para visualização em tempo real. Foram ainda organizadas as ligações elétricas e a distribuição de energia entre o sistema de alimentação, o microcontrolador e os sensores, garantindo estabilidade e funcionamento integrado da estação. 

A **Figura 33** apresenta o esquema de ligação da estação de monitorização ambiental, evidenciando a interligação entre os principais componentes do sistema. 

Legendas dos componentes apresentados na **Figura 33** : 

- 1 – Sensor de partículas OPC-N3 

- 2 – Sensor meteorológico BMP180 

- 3 – Microcontrolador ESP 32 

- 4 – Sensor meteorológico DHT22 

- 5 – Módulo SD Open Log 

- 6 – Voltímetro Digital 

- 7 – Bateria Li-po 

- 8 – Módulo carregador de bateria de íon lítio da Adafruit bq25185 

- 9 – Painel fotovoltaico 6 V / 9 W 

40 



<!-- Start of picture text -->
2<br>1<br>4<br>:<br>3<br>5 9<br>6<br>7<br>8<br>Ho<br><!-- End of picture text -->

**Figura 33** - Esquema de ligação da estação de monitorização ambiental. Fonte: [Autor] 

### **4.4.Definição da conceção do protótipo** 

Nesta fase, foi definida a arquitetura funcional do protótipo e realizada a montagem experimental em protoboard para testar a integração elétrica e funcional do microcontrolador, dos sensores e dos sistemas de alimentação e comunicação, bem como avaliar diferentes configurações de ligação. Esta abordagem permitiu verificar a compatibilidade entre os componentes e apoiar as decisões técnicas para a implementação do sistema. A conceção física foi concretizada através da adaptação da caixa de uma estação **Quant AQ** , que permite organizar e proteger os componentes. A caixa dispõe de aberturas para a circulação do ar e foi concebida para proteger os componentes da chuva e da exposição solar direta, permitindo a entrada de ar necessária à medição das partículas atmosféricas. A Figura apresenta a caixa utilizada. 

41 





**Figura 34** - Caixa da estação Quant AQ adaptada para o protótipo de monitorização ambiental. Fonte [Autor] 

### **4.5.Análise e gestão de dados** 

A análise e a gestão dos dados são essenciais para assegurar a integridade, a consistência e a relevância das medições do sistema de monitorização ambiental. A comunicação entre o ESP32 e o Node-RED, através do protocolo MQTT, permite o envio das leituras. No backend, uma API REST valida e disponibiliza os dados, que são armazenados no MongoDB, em coleções organizadas para facilitar a consulta e a manutenção do histórico de medições. 

No frontend desenvolvido em Angular, os valores de PM1,0, PM2,5, PM10, temperatura, humidade, pressão e altitude são apresentados em dashboards atualizáveis em tempo real. A arquitetura permite acompanhar as medições, consultar o histórico e apoiar a interpretação da qualidade do ar através do Índice de Qualidade do Ar (IQA). A fiabilidade da informação depende da validação dos dados recebidos pelo backend, da identificação temporal das amostras e do tratamento de valores ausentes ou duplicados. Para facilitar a interpretação, a interface deve apresentar a data e hora da última atualização, permitindo distinguir medições recentes de dados desatualizados. 

### **4.6. Tabela de Ligação** 

As ligações foram organizadas por interface de comunicação (UART, I²C, SPI, digital e analógica), facilitando a montagem e o diagnóstico de eventuais falhas. A **Tabela 10** apresenta, para cada módulo, os pinos utilizados, a interface de comunicação, a tensão de alimentação e as respetivas observações técnicas. As ligações devem ser confirmadas com base no esquema e na montagem efetivamente realizada. O SIM800L, por não ter sido integrado no protótipo final, não é considerado nas ligações implementadas. 

42 

**Tabela 10 -** Ligação de sistema de monitorização ambiental AirCV – UNICV. Fonte: 

**[Autor]** 

|**Dispositivo**|**Pino do dispositivo**|**Pino do ESP32**|**Interface**|**Alimentação**|
|---|---|---|---|---|
|OPC – N3|VCC<br>GND<br>MISO<br>MOSI|VIN<br>GND<br>GPIO 19<br>GPIO 23|SPI|5 V|
||SCK|GPIO 18|||
||CS|GPIO 5|||
|DHT22|VCC<br>DATA<br>GND|3,3 V<br>GPIO 13<br>GND|Digital|3,3 V|
||VCC|3,3 V|||
|BMP180|GND<br>SCL|GND<br>GPIO 22|I²C|3,3 V|
||SDA|GPIO 21|||
||VCC|VIN|||
||TX2|RX2|||
|Open Log|RX2|TX2|UART|5 V|
||GND|GND|||
||Tensão do painel solar (VU)|GPIO 34|||
|Voltímetro<br>digital|Tensão da bateria (BAT)<br>Tensão de saída (5 V)|GPIO 32<br>GPIO 35|Analógica<br>(ADC1)|—|



### **4.7. Análise financeira do protótipo** 

A presente análise financeira tem como objetivo apresentar o investimento realizado na aquisição dos principais componentes utilizados no desenvolvimento do protótipo da estação de monitorização ambiental. Para esse efeito, são identificados os componentes adquiridos, as respetivas quantidades e os preços unitários, permitindo determinar o valor total investido nos materiais utilizados na implementação do sistema. 

43 

**Tabela 11  –** Análise financeira do protótipo. Fonte: [Autor] 

|**Componente**|**Qtd.**|**Preço unitário**<br>**(€)**|
|---|---|---|
|OPC – N3|1|539,01|
|BMP180|1|12,91|
|DHT22|1|1,24|
|Open Log|1|1,55|
|ESP32-WROOM|1|10,00|
|Painel fotovoltaico 6 V / 9 W|1|68,64|
|Carregador solar Adafruit|1|12,99|
|(BQ24074)|||
|Bateria Li-ion 3,7 V / 6600 mAh|1|21,29|
|Total||667,63|



### **4.8. Dimensionamento energético do sistema** 

Nesta secção apresenta-se o dimensionamento energético do sistema de alimentação da estação, com o objetivo de verificar se a arquitetura escolhida (painel fotovoltaico, módulo Adafruit 6106 e bateria) garante autonomia adequada face ao consumo dos componentes. A análise segue cinco passos: estimativa do consumo de cada componente; cálculo da potência total em dois cenários de funcionamento; determinação da energia disponível na bateria e da autonomia teórica; validação experimental da autonomia com envio de dados a cada 60 s; e, por fim, comparação entre a energia produzida pelo painel e a energia consumida. 

#### **Consumo dos componentes** 

A corrente de cada componente foi estimada com base nas respetivas fichas técnicas (Tabelas 1, 2, 3, 6 e 8), considerando os valores máximos especificados durante o funcionamento ativo. A potência de cada módulo resulta do produto entre a tensão de alimentação e a corrente (P = V × I). Os valores da **Tabela 12** refletem, assim, o pior caso de cada componente enquanto ativo: constituem uma estimativa teórica conservadora, mas não representam necessariamente o consumo contínuo do sistema, que depende também dos perfis de funcionamento de cada módulo e das perdas do circuito de alimentação. 

44 

**Tabela 12   –** Consumo estimado dos componentes da estação. Fonte: [Autor] 

|**Componentes**|**Tensão**|**Corrente**|**Potência**|**Tabela**|
|---|---|---|---|---|
|OPC – N3|5V|180 mA|0,9 W|Tabela 6|
|BMP180|3,3V|5 µA|0,0000165 W|Tabela 3|
|DHT22|3,3V|1,5 mA|0,00495 W|Tabela 2|
|ESP32-WROOM (WiFi)|5V|240 mA|1,20 W|Tabela 1|
|Open Log (repouso)|5V|3 mA|0,015 W|Tabela 8|
|Open Log (gravação)|5V|20mA|0,1 W|Tabela 8|



#### **Cenários de funcionamento** 

O módulo de cartão SD destina-se ao armazenamento temporário das medições quando não é possível comunicar com o broker MQTT. Em funcionamento normal permanece em repouso, evitando escritas desnecessárias. Definiram-se, por isso, dois cenários, cuja potência total resulta da soma das potências do ESP32, do OPC-N3, do OpenLog, do DHT22 e do BMP180: 



#### **Cenário A — Funcionamento normal (comunicação Wi-Fi disponível):** 

A estação liga-se à rede Wi-Fi e ao broker MQTT e publica periodicamente as medições na plataforma AirCV. Os dados são transmitidos diretamente, pelo que o módulo SD permanece em repouso. 



#### **Cenário B — Falha de comunicação prolongada (SD Card em escrita contínua):** 

Quando falha a ligação Wi-Fi ou a comunicação com o broker MQTT, a estação ativa o armazenamento local: as medições continuam a ser adquiridas e registadas no cartão SD, preservando os respetivos instantes de aquisição. Restabelecida a comunicação, os registos armazenados são enviados para o broker, permitindo recuperar os dados não transmitidos durante a interrupção. 

Ptotal = 1,20 + 0,90 + 0,1 + 0,00495 + 0,0000165 ≈ 2,205 W 

45 

#### **Energia Disponível na Bateria** 

A energia nominal armazenada na bateria é determinada pelo produto entre a sua tensão nominal e a capacidade. Considerando uma bateria Li-Po de 3,7 V e 6600 mAh (6,6 Ah), obtémse: 

Energia bateria = V_bateria × Capacidade = 3,7V×6,6Ah=24,42 Wh. 

Este valor é a energia teoricamente armazenada nas condições nominais e serve de referência para avaliar a autonomia na ausência de carregamento solar. A energia efetivamente utilizável é inferior, pois a descarga de uma célula de iões de lítio deve ser interrompida antes do esgotamento total. 

#### **Autonomia teórica e Consumo Diário** 

A autonomia teórica e o consumo diário foram estimados considerando a potência máxima de cada componente durante 24 horas consecutivas. Esta abordagem constitui uma simplificação conservadora do dimensionamento energético, assumindo que todos os módulos consomem continuamente a potência máxima especificada, em todos os segundos do dia, independentemente dos respetivos ciclos de funcionamento. Deste modo, não são consideradas as reduções de consumo nos períodos de menor atividade nem os intervalos entre transmissões de dados. Embora esta hipótese possa sobrestimar o consumo real da estação, permite estabelecer um limite superior para a energia consumida e, consequentemente, uma estimativa inferior da autonomia da bateria, introduzindo uma margem de segurança no dimensionamento do sistema. A **Tabela 13** apresenta os resultados. 

**Tabela 13 -** Autonomia e consumo diário por cenário. Fonte: [Autor] 

||**Cenário A (normal)**|**Cenário B (falha prolongada)**|
|---|---|---|
|Autonomia sem sol|24,42 / 2,12 ≈ 11,5 h|24,42 / 2,205 ≈ 11,1 h|
|Consumo diário da estação|2,12 × 24 = 50,9 Wh/dia|2,205 × 24 = 52,9 Wh/dia|



O **Cenário B** consome ligeiramente mais do que o **Cenário A** , devido à atividade adicional de escrita no cartão SD. 

Convém distinguir dois parâmetros com finalidades diferentes. As 24 h são apenas o período de referência para exprimir o consumo em Wh/dia, o que não significa que a estação transmita dados continuamente durante 24 h nem que o intervalo entre envios seja de 24 h. Os 60 s são o intervalo entre aquisições e envios de dados. O cálculo teórico não depende deste 

46 

intervalo, porque admite todos os módulos ativos em permanência; o intervalo de 60 s é relevante para o ensaio experimental descrito a seguir. 

#### **Autonomia experimental com envio de dados a cada 60 s** 

Para complementar o dimensionamento teórico, foi realizada uma validação experimental da autonomia da bateria durante o funcionamento normal da estação (Cenário A). Neste ensaio, a aquisição e a transmissão dos dados para a plataforma AirCV foram realizadas em intervalos de 60 segundos, correspondendo a 60 registos por hora e 1440 registos num período de 24 horas. Ao contrário do dimensionamento teórico, que considera o consumo máximo contínuo durante 24 horas, esta análise permite observar experimentalmente a evolução da tensão da bateria nas condições reais de funcionamento da estação. Cada ponto dos gráficos corresponde a uma leitura de tensão associada a um envio de dados, permitindo analisar os períodos de descarga, estabilidade e eventual carregamento. O período analisado decorreu entre as 18:00 de 12/11/2026 e as 12:00 de 13/11/2026 (18 h), o que corresponde a cerca de 1080 leituras. 

A **Figura 35** reproduz o separador «Gráficos de Energia» da plataforma, com o filtro de 1 dia. O gráfico superior mostra a tensão do painel solar, que se manteve praticamente constante, em cerca de 0,9 V, ao longo de todo o período. O gráfico inferior mostra a tensão da bateria, com a legenda de cores usada pela plataforma: verde para «A carregar», vermelho para «A descarregar» e cinzento para «Estável». A bateria parte de cerca de 4,1 V, desce de forma contínua até cerca de 3,5 V por volta das 04:00 (trecho vermelho), permanece aproximadamente nesse valor durante cerca de 3 h (trecho cinzento) e volta a descer até cerca de 3,15 V às 12:00. Não existe nenhum trecho verde: durante o ensaio não foram identificados períodos de carga, pelo que a bateria funcionou sem contributo solar efetivo, o que corresponde à condição «sem sol» considerada na **Tabela 13** . 

47 



<!-- Start of picture text -->
Monitorizacao - Estacao - AirCV - UNICV x<br>Visio Geral © Analise de Bateria<br>© Tempo quase real 3dias Tis —_Personalizado<br>Tensao do Painel Solar<br>Tensao da Bateria (V)<br>eA en .<br>——____eeae<br><!-- End of picture text -->

**Figura 35 -** Evolução da tensão da bateria durante o processo de descarga. Fonte: [Autor] 

A **Figura 36** mostra o separador «Análise de Bateria» para o mesmo período. A plataforma divide a evolução da tensão em períodos de descarga (a vermelho, à esquerda) e de carga (a verde, à direita) e indica, para cada período de descarga, o intervalo de datas, o tempo total e o tempo gasto em cada intervalo de 0,1 V. Foram identificados dois períodos de descarga (das 18:00 às 04:00, com 10 h, e das 07:00 às 12:00, com 5 h), separados pelo trecho estável de 3 h visível na **Figura 36** ; as 10 h, as 3 h e as 5 h somam as 18 h observadas. Na secção de carga surge a indicação de que não foram identificados períodos de carga, em coerência com a **Figura 36** . Os dados da **Figura 36** estão reunidos na **Tabela 14** , acompanhados da taxa média de descida da tensão em cada intervalo. 

48 



<!-- Start of picture text -->
Monitoriza¢gao - Estacao - AirCV - UNICV<br>88 Visio Geral 1 Graficos de Energia. © Anilise de Bateria<br>‘© Tempo quase real EP saies — 7aias —Personalizado<br>© Descarga © carga<br>12/11/2026, 18:00<br>~ 13/11/2026, 04:00 Total: 10100 min<br>Intervalo de tensio Tempo<br>40V—3.9V 200 min<br>39V—38V 115 min<br>38V—37V 145 min<br>37V—36V 2h00 min<br>36V—35V 200 min<br>1371/2026, 07:00<br>~ 13/1/2026, 12:00 Total: 5100 min<br>Intervalo de tensso Tempo<br>35V—3.4V 2h 00min<br>34V—33V 1h<br>47 min<br>33V—32V 50 min<br><!-- End of picture text -->

**Figura 36 -** Variação da tensão da bateria em função do tempo, com intervalo de registo de 

60 segundos. Fonte [Autor] 

**Tabela 14 -** Tempo de descarga por intervalo de tensão (dados da Figura 36). Fonte: 

[Autor] 

|**Intervalo de tensão**|**Tempo**|
|---|---|
|4,0 → 3,9 V|2 h 00 min|
|3,9 → 3,8 V|1 h 15 min|
|3,8 → 3,7 V|1 h 45 min|
|3,7 → 3,6 V|2 h 00 min|
|3,6 → 3,5 V|2 h 00 min|
|Restante (total − intervalos listados)|1 h 00 min|
|Estável|3 h 00 min|
|3,5 → 3,4 V|2 h 00 min|



49 

|**Intervalo de tensão**|**Tempo**|
|---|---|
|3,4 → 3,3 V|1 h 47 min|
|3,3 → 3,2 V|0 h 50 min|
|Restante (total − intervalos listados)|0 h 23 min|
|**Tempo total observado**|**18 h 00 min**|



**Nota:** os intervalos listados na plataforma somam 9 h 00 min (período 1) e 4 h 37 min (período 2), valores inferiores aos totais indicados. A diferença (1 h 00 min e 23 min) corresponde ao tempo fora dos intervalos visíveis, provavelmente de 4,1 à 4,0 V e de 3,2 V até cerca de 3,15 V, onde o gráfico começa e termina. 

A taxa de descida varia ao longo do ensaio, confirmando que a tensão de uma célula de iões de lítio não diminui linearmente com o tempo. Entre 4,0 V e 3,8 V, a descida é mais rápida; na gama intermédia, entre 3,8 V e 3,3 V, torna-se mais lenta e relativamente constante; abaixo de 3,3 V, volta a acelerar. Este último comportamento indica a aproximação do limite de descarga, justificando a definição de um limiar de alerta de bateria fraca acima desse valor. 

Em síntese, durante o envio de dados a cada 60 s e sem carregamento solar, a tensão da bateria desceu de cerca de 4,1 V para cerca de 3,15 V em 18 h, pelo que a autonomia experimental foi de pelo menos 18 h, cerca de 1,6 vezes a autonomia teórica do Cenário A (11,5 h). Admitindo que a energia extraída da bateria não excedeu os 24,42 Wh nominais, a potência média da estação durante o ensaio foi, no máximo: 

Pmédia ≤ Ebateria / t = 24,42 Wh / 18 h ≈ 1,36 W 

Este limite corresponde a cerca de 64 % dos 2,12 W estimados para o Cenário A. A diferença é coerente com o facto de os valores das fichas técnicas corresponderem a picos de corrente em funcionamento ativo (como a transmissão Wi-Fi do ESP32) e não à corrente média, e de os módulos não operarem em permanência à potência máxima. A estimativa teórica deve, portanto, ser entendida como um valor conservador, e a autonomia de 11,5 h como um limite inferior. 

Estes resultados devem ser interpretados com cautela: a tensão é apenas um indicador indireto da energia restante e a sua curva depende da corrente, da temperatura e do estado da célula; o ensaio corresponde a uma única descarga, sem medição direta da corrente; e a capacidade real da bateria pode diferir da nominal. A medição da corrente absorvida nos vários modos de 

50 

funcionamento (normal, transmissão, escrita no SD e falha prolongada) permitiria validar a potência média e ensaiar também o Cenário B. 

#### **Energia Produzida pelo Painel Solar** 

A produção de um painel fotovoltaico deve ser estimada em horas de luz solar útil. Cabo Verde recebe, em média, entre 6 e 10 horas de sol por dia, consoante a época do ano e a ilha. Para o painel de 9 W, obtém-se: 





Estes valores são teóricos: assumem a potência nominal do painel durante todas as horas de sol, sem perdas no carregador. Importa notar que, no ensaio da **Figura 35** , a tensão do painel se manteve em cerca de 0,9 V e não houve carga da bateria, pelo que esta produção não foi verificada experimentalmente 

#### **Balanço Energético** 

O balanço energético é a diferença entre a energia produzida pelo painel e a energia consumida pela estação: 



Considerando uma produção entre 54 Wh/dia (6 h de sol) e 90 Wh/dia (10 h de sol) e os consumos da **Tabela 12** , obtêm-se os resultados da **Tabela 15** : 

**Tabela 15 -** Balanço energético diário do sistema Fonte: [Autor] 

|**Cenário**|**Consumo diário**|**Energia produzida**|**Saldo energético**|
|---|---|---|---|
|A – Funcionamento normal|50,9 Wh/dia|54–90 Wh/dia|+3,1 a +39,1 Wh/dia|
|B – Falha prolongada|52,9 Wh/dia|54–90 Wh/dia|+1,1 a 37,1 Wh/dia|



No Cenário A, a energia produzida é superior ao consumo diário, com balanço positivo entre +3,1 e +39,1 Wh/dia. No Cenário B o balanço também é positivo, entre +1,1 e +37,1 Wh/dia. Nos dias de menor disponibilidade solar (6 h de sol), porém, a margem é muito reduzida (3,1 e 1,1 Wh/dia, respetivamente), inferior a 7 % do consumo diário. Assim, o sistema apresenta balanço positivo nos dois cenários, mas uma falha de comunicação prolongada com pouca radiação solar 

51 

deixa-o praticamente sem margem, podendo levar à descarga progressiva da bateria se existirem perdas adicionais no circuito de alimentação ou vários dias consecutivos de baixa produção. 

Estes balanços baseiam-se no consumo teórico de 24 h à potência máxima. Se se adotar, para o Cenário A, o limite superior obtido experimentalmente (1,36 W, isto é, no máximo cerca de 32,6 Wh/dia), a margem passaria a ser de, pelo menos, +21,4 Wh/dia com 6 h de sol e +57,4 Wh/dia com 10 h de sol, antes de perdas de carga. Este resultado é indicativo e deve ser confirmado com a medição direta da corrente e com um ensaio em que o painel carregue efetivamente a bateria. 

52 

# **Capítulo 5 – Arquitetura do sistema de Comunicação** 

A arquitetura do sistema de comunicação foi desenvolvida para garantir a recolha, transmissão, processamento, armazenamento e disponibilização eficiente dos dados de qualidade do ar. Integra sensores, protocolos de comunicação, plataformas de processamento, backend, base de dados e interface de visualização, funcionando de forma coordenada para assegurar fiabilidade, escalabilidade e consistência na gestão da informação. 

### **5.1. Estrutura global do sistema** 

O fluxo de dados inicia-se na estação de monitorização, responsável pela aquisição dos parâmetros ambientais. As medições são publicadas periodicamente através do protocolo MQTT num broker e recebidas pelo Node-RED, que as disponibiliza ao backend através de consultas HTTP periódicas (polling). O backend, desenvolvido em Node.js/Express, valida os dados, calcula o Índice de Qualidade do Ar (IQA), armazena as medições no MongoDB e disponibilizaas ao frontend AirCV através de uma API REST. A autenticação dos utilizadores da área privada é assegurada pelo Keycloak. A arquitetura permite integrar múltiplas estações, identificadas por um identificador único e centralizar o histórico das medições. A **Figura 37** apresenta a arquitetura geral do sistema implementado. 



<!-- Start of picture text -->
ia yy MQTT Publish<br>d +<— Linux Server - Ubuntu<br>: t MOTT 24.04.2 LTS<br>t Subscription<br>- Backend Frontend<br>n Estagdes de Man eiasigrtr2.6:2.400-1060 (Nodejs/Express) (Angular)<br>Monitorizagao Ambiental Sibectotin<br>> =e emit - = HTTP (API REST) - Gs) L HTTP (API REST)| iow<br>Broker<br>ad MQTT Publish Node RED query Query<br>MQTT https://172.16.2.168:4200<br>HTTP (APIREST) | Query<br>MQTT Publish<br>YF«<br>{t , ‘SubscriptionMOTT ry MongobDB.<br>. https://172.16.2.169:27017<br>Estagao de Monitorizagao<br>“Ambiental - AirCV - UNICV<br><!-- End of picture text -->

**Figura 37** - Arquitetura geral do sistema. Fonte: [Autor] 

53 

Como a estação transmite medições em intervalos de 60 s e o backend recolhe os dados do Node-RED por polling a cada 30 s, o sistema funciona em tempo quase real, e não em tempo real estrito. Teoricamente, o atraso nominal máximo associado a estes dois intervalos é de aproximadamente 90 s, antes de considerar o processamento e a atualização do dashboard. Este valor constitui uma estimativa, não um resultado de medição experimental. 

### **5.2. Estação de monitorização ambiental – AirCV - UNICV** 

A estação de monitorização integra sensores para medição de parâmetros atmosféricos (temperatura, humidade, pressão, altitude) e partículas inaláveis (PM1.0, PM2.5 e PM10). O microcontrolador ESP32 gere a leitura dos sensores e a transmissão dos dados, encapsulados no formato JSON. 

A comunicação da estação com a plataforma é realizada através do protocolo MQTT, utilizando a ligação Wi-Fi, que foi implementada e testada experimentalmente. O ESP32 executa rotinas de reconexão automática para recuperar a ligação em caso de falhas momentâneas. As medições são publicadas no broker MQTT a cada 60 segundos. Embora a comunicação GSM/GPRS tenha sido considerada na arquitetura e prevista na conceção inicial, o módulo SIM800L não foi instalado nem integrado no protótipo final. Por conseguinte, esta alternativa não foi validada experimentalmente, tendo os ensaios sido realizados exclusivamente através de Wi-Fi. A implementação futura de GSM/GPRS exigirá a integração do modem e a validação da configuração do APN, da gestão da ligação, dos mecanismos de reconexão e do comportamento perante a indisponibilidade da rede. 

### **5.3. Broker MQTT** 

No âmbito deste projeto, foi utilizado um broker MQTT privado (GS2_Broker) baseado numa solução open-source para gestão da comunicação MQTT, instalado no servidor descrito na secção «Servidor». O broker atua como intermediário na transmissão das mensagens: recebe as publicações da estação e encaminha-as para os clientes subscritores, entre os quais se encontra o Node-RED, e entrega à estação os comandos publicados pelo Node-RED. 

### **5.4. Node-Red** 

O Node-RED desempenha o papel de componente intermediário entre a estação de monitorização e o backend implementado em Node.js. Após a estação transmitir os dados via GPRS ou Wi-Fi, estes são publicados no broker MQTT e, de seguida, encaminhados ao NodeRED, que se encontra subscrito ao tópico correspondente. Nesta plataforma, os dados são estruturados, filtrados e convertidos para um formato uniforme, garantindo que a informação 

54 

chega ao backend pronta para armazenamento no MongoDB e posterior disponibilização para visualização em tempo real. 

#### **Comunicação entre a Estação e o Node-Red** 

A comunicação entre a estação de monitorização e o Node-RED é realizada através do protocolo MQTT, baseado no modelo **publish/subscribe** , adequado para aplicações IoT devido à sua leveza e fiabilidade. A estação publica periodicamente as medições no tópico **estacion_data (Dados da estação)** , enquanto o estado de ligação é enviado pelo tópico **status_data (Status da ligação)** . Em caso de falha de comunicação, os dados são armazenados no cartão de memória e enviados posteriormente através do tópico **offline_estacion_data (Dados offline)** . 

O Node-RED permanece subscrito a estes tópicos, permitindo a receção automática das mensagens e garantindo a aquisição contínua e integrada dos dados ambientais. 



<!-- Start of picture text -->
Status<br>1B comecea © enga7 8<br>@ da igacso - —— UGBBUGIZBIES<br>+ oy comected<br>{| Dados envipar a Dasnooarddos<br>(7 tgenemssocs $479] tuneton42 6 np<br>Puplcagao<br>@ comociea de dads omine _~ NGRBUGBBNE-) a 1B connacteaDados orine  ~SNGBBURZEIEY©<br><!-- End of picture text -->

**Figura 38–** Fluxo 1 de trabalho Node-RED. Fonte: [Autor] 

O sistema desenvolvido permite a configuração remota da estação de monitorização baseada no ESP32, evitando a necessidade de acesso físico ao dispositivo para alterar os seus parâmetros. As configurações são realizadas através de comandos enviados pelo Node-RED ou pela interface de administração da plataforma, publicados em config/get e config/set, sendo as respostas da estação recebidas em config/current permitindo a gestão dos equipamentos mesmo em locais de difícil acesso. 

Conforme apresentado na **Figura 39** , são disponibilizados dois modos principais: 

- **Consulta das configurações atuais:** permite visualizar remotamente os parâmetros armazenados na estação, facilitando operações de monitorização e diagnóstico. 

- **Alteração das configurações:** permite configurar parâmetros da estação, como a rede Wi-Fi, o servidor MQTT, a identificação do dispositivo e o intervalo de publicação, bem como solicitar a reinicialização e a reposição das configurações de fábrica. A seleção de 

55 

GSM/GPRS e a configuração do módulo SIM800L foram previstas, mas não foram integradas nem validadas no protótipo final. A gestão do cartão SD depende das funcionalidades efetivamente implementadas no firmware. 

O sistema inclui ainda a sincronização do relógio interno e a recuperação dos dados armazenados localmente no cartão SD durante falhas de comunicação. Todas as configurações são guardadas na memória não volátil do ESP32, permanecendo disponíveis após reinicializações ou interrupções de energia. 

Esta funcionalidade aumenta a flexibilidade e facilita a gestão centralizada de múltiplas estações de monitorização sem necessidade de intervenção física. 



<!-- Start of picture text -->
9<br>ge eesani<br>(U[esocnes1 cso ‘—§ EY @<br>> €i °<br>C) > 3 <i j<br>(> P €i<br>> 4 €i b<br>(1) > B ef a Taso )<br>C) > b—i p<br>() > P £i 2<br>> G €i °<br>() > P £i 2<br>> Ei 3<br>CO) > 5 ti 3<br>omen — ee<br><!-- End of picture text -->

**Figura 39 –** Fluxo 2 de trabalho Node-RED. Fonte: [Autor] 

#### **Configuração da Estação para comunicação com o Node-RED** 

A estação estabelece comunicação com o broker MQTT através de uma ligação TCP/IP, utilizando o endereço do servidor e a porta 1883 ( **Figura 40** ). Esta porta corresponde à comunicação MQTT sem cifragem TLS, pelo que a proteção dos dados depende das medidas de segurança configuradas no broker e na rede. No protótipo final, a ligação à Internet é realizada através da interface Wi-Fi do ESP32. A comunicação GSM/GPRS, prevista inicialmente através do módulo SIM800L e dependente de um cartão SIM ativo e da configuração do APN da operadora, não foi integrada nem validada experimentalmente. Após estabelecer a ligação, o 

56 

ESP32 publica periodicamente as medições no broker MQTT, com um intervalo de 60 segundos. 



<!-- Start of picture text -->
// ===== CONFIGURACOES MQTT (PADRAO) =====<br>#define MQTT_SERVER_DEFAULT “*###e#x=exxsh<br>#define MQTT_PORT_DEFAULT 1883<br>// ===== TOPICOS MQTT =====<br>define MQTT_TOPIC_DATA “estation_data”<br>#define MQTT_TOPIC_CONFIG_GET “config/get"<br>#define MQTT_TOPIC_CONFIG_SET “config/set™<br>#define MQTT_TOPIC_CLOCK_SYNC “station/clock/sync”<br>#define MQTT_TOPIC_STATUS “station/status”<br>#define MQTT_TOPIC_CONFIG_CURRENT “config/current”<br>#define MQTT_TOPIC_OFFLINE_DATA — “of fline_estation_data”<br>define MQTT_TOPIC_OFFLINE_STATUS “offline_station/status”<br><!-- End of picture text -->

**Figura 40** - Configuração da estação para comunicação com o Node-RED. Fonte: [Autor] 

Importa salientar que a porta 1883 corresponde à comunicação MQTT sem cifragem TLS. Neste modo, as credenciais, as medições e os comandos de configuração podem ser intercetados ou alterados por terceiros com acesso à rede. No protótipo, testado numa rede local controlada, esta configuração foi utilizada para validar a comunicação entre os componentes. Contudo, para uma implantação em redes públicas ou através de GPRS, recomenda-se a utilização de MQTT sobre TLS, geralmente na porta 8883, com validação do certificado do broker e, idealmente, credenciais ou certificados individuais por estação, reforçando a confidencialidade, a integridade e o controlo de acesso. 

#### **Tópicos MQTT** 

A **Tabela 16** apresenta os tópicos MQTT utilizados na versão atual do firmware, conforme definidos no código da estação ( **Figura 40** ). Para cada tópico, são indicados a finalidade e a direção da comunicação. A nomenclatura apresentada corresponde à configuração definida no firmware e deve ser consistente com a utilizada no fluxo Node-RED. 

**Tabela 16 -** Tópicos MQTT utilizados. Fonte: [Autor] 

|**Tópico**|**Sentido**|**Função**|
|---|---|---|
|estation_data|Estação → Node-RED|Medições periódicas e estado do dispositivo|
|station/status|Estação → Node-RED|Estado da ligação (online/offline)|



57 

|**Tópico**|**Sentido**|**Função**|
|---|---|---|
|offline_estation_data|Estação → Node-RED|Reenvio das medições guardadas no cartão<br>SD durante falhas de comunicação|
|offline_station/status|Estação → Node-RED|Estado do processo de envio dos dados<br>offline|
|config/get|Node-RED → Estação|Pedido das configurações atuais|
|config/current|Estação → Node-RED|Resposta com as configurações atuais|
|config/set|Node-RED → Estação|Alteração de configurações e comandos<br>(reboot, reset_factory, pedido de dados<br>offline)|
|station/clock/sync|Node-RED → Estação|Sincronização do relógio interno|



#### **Configuração do Node-RED para receção dos dados da estação** 

Para receber os dados enviados pela estação, o Node-RED necessita de ser configurado com um nó **MQTT Input** , associado ao tópico no qual são publicadas as medições. Este nó especifica o endereço do broker MQTT, a porta utilizada e os parâmetros de autenticação, quando aplicável. Após concluída a configuração, o Node-RED passa a subscrever o tópico da estação, recebendo automaticamente todas as mensagens emitidas. Os dados são, então, encaminhados para os fluxos de processamento, permitindo a conversão, filtragem, validação e posterior disponibilização dos valores para o backend. 



<!-- Start of picture text -->
Edit mattin node<br>.<br>‘@ Properties *A88<br>@ Server GS2_Broker va i+<br>S Topic estation_data<br>@ Qos 2 v<br>‘Name Dados da estacdo<br><!-- End of picture text -->

**Figura 41** - Configuração do nó de entrada MQTT. Fonte: [Autor] 

Após a conclusão das configurações, o Node-RED passa a receber corretamente os dados enviados pela estação, tal como demonstrado na **Figura 42** apresentada abaixo. 

58 



<!-- Start of picture text -->
largest_free_block:uptine: 38 110580<br>sensor_dht_ok: true<br>sensor_bmp_ok: true<br>sensor_ope_ok: true<br>Last_tempDHT: 29.3<br>Last_hum: 63.4<br>Last_tempon: 30<br>last_press: 1006.08<br>last_alt: 59.95055<br>last_pyi: 3.809826<br>Last_pM2_S: 14.1936<br>Last_pmie: 32.89352<br>opc_temp: 33.66521<br>ope_hum: 44.1397<br>opc_flow_rate: 4.93<br>opc_sampling_period: 29.99<br>ope_fan_rev: @<br>ope_laser_status: 591<br>> opc_bins: arroy[26]<br>veet 4.8<br>vbat: 4<br>battery_percent: 50<br>battery_status: “Cheia®<br>power_status: "Painel Solar Ativo”<br>solar_status: “Carregando” >- 6)<br>voltage diff: 0.8<br>charging efficiency: 83<br>sdok: true<br>sd_device: “opentog”<br>‘sd_mode: "NEWLOG”<br>sd_file: “LOGoo0x.1XT (auto)”<br>offline_buffer_bytes: @<br><!-- End of picture text -->



<!-- Start of picture text -->
estain_data msg payload = Object-<br>vobjecta<br>device id: "ESP32-001"<br>~<br>seq:a 2<br>t0_ms:ss 1793972460133<br>timestamp: "96/11/2026 13:40:59"<br>location: “UNICV™<br>Latitude: 0<br>longitude: ©<br>connection_type:vad "wifi<br>time_synced: true<br>status: "online"<br>signal_quality:sols y “Fair”<br>(¢né115200-FalFirmare_version:back-NewLog™“v9.4.0-0p |B)?<br>chip_model: “ESP32-DeuD-V3"<br>chip_revision: 301<br>cpu_freq: 200<br>free_heap: 212712<br>Lorgest_free_block: 110580<br>uptine: 88<br>sensor_dht_ok: true<br>sensor_bmp_ok: true<br>sensor_ope_ok: true<br>Last_tempoHT: 29.3<br>Last_hum: 63.4<br>Last_tempsnp: 30<br>last_press: 1006.08<br>last_alt: 59.95055<br>Last_pmi: 3.809826<br>Lest_pH2_s: 14.1936<br>Lest_pie: 32.89352<br>opc_temp: 3.66521.<br><!-- End of picture text -->

**Figura 42 -** Dados recebido no Node-RED. Fonte: [Autor] 

#### **Tabela 17** – Grupos de campos do payload JSON. Fonte: [Autor] 

|**Grupo**|**Campos**|**Unidades / observações**|
|---|---|---|
|Identificação e<br>localização|device_id, location, latitude, longitude,<br>timestamp|Coordenadas geográficas são<br>expressas em graus decimais;<br>timestamp gerado pelo relógio<br>da estação no momento da|
|||aquisição|
|Estado da ligação|connection_type, status, signal_quality,<br>time_synced|connection_type: wifi ou gsm|
|Variáveis<br>meteorológicas|last_tempDHT, last_tempBMP, last_hum,<br>last_press, last_alt|°C, °C, %, hPa, m|
|Material particulado|last_PM1, last_PM2_5, last_PM10|µg/m³ (sensor PMS5003)|



59 

|**Grupo**|**Campos**|**Unidades / observações**|
|---|---|---|
|Estado dos sensores|sensor_dht_ok, sensor_bmp_ok,|Permitem distinguir valor|
|e do SD|sensor_pms_ok, sd_ok, sd_error|ausente de valor nulo|
|Energia|vcc, vbat,battery_percent<br>,battery_status,<br>power_status, solar_status, voltage_diff,<br>charging_efficiency|V, V, %, texto, texto, texto, V,<br>%|
|Diagnóstico do<br>dispositivo|firmware_version, chip_model,<br>chip_revision, cpu_freq, free_heap,<br>largest_free_block, uptime|MHz, bytes, bytes, s|



#### **5.5. AirCV – Plataforma web de monitorização ambiental** 

O **AirCV** é o dashboard web desenvolvido neste projeto para monitorização da qualidade do ar e das variáveis meteorológicas em tempo quase real. A aplicação foi implementada utilizando o framework Angular, permitindo uma interface moderna, responsiva e organizada para visualização e análise dos dados provenientes das estações de monitorização. 

A plataforma permite acompanhar o estado das estações, consultar parâmetros ambientais, visualizar históricos e analisar a qualidade do ar. A arquitetura baseada em componentes e serviços garante a separação entre a interface e a lógica de comunicação com o backend, sendo os dados obtidos através de uma API REST, sem acesso direto à base de dados MongoDB, aumentando a segurança e escalabilidade do sistema. 

O controlo de acesso é realizado através do Keycloak, responsável pela autenticação e gestão de utilizadores. O dashboard possui dois modos de acesso: público, destinado à consulta dos dados ambientais e localização das estações, e privado, reservado a utilizadores autenticados, permitindo funções de gestão, configuração e análise avançada do sistema. 

### **5.5.1. Comunicação entre Node-Red e Backend(Node.JS)** 

O Node-RED atua como concentrador de dados, recebendo as medições da estação através do protocolo MQTT e disponibilizando-as por meio de endpoints HTTP. O backend, desenvolvido em Node.js, consulta estes endpoints a cada 30 segundos (polling), obtendo os dados mais recentes em formato JSON. 

Após a validação, o backend calcula parâmetros derivados, como o Índice de Qualidade do Ar (IQAr), armazena os dados no MongoDB e atualiza o estado dos sensores. Este fluxo assegura a recolha, processamento e armazenamento consistentes da informação, permitindo ao dashboard desenvolvido em Angular aceder aos dados atualizados através da API REST. 

60 



<!-- Start of picture text -->
[get] /PMS5003 —— function 42 ———{ http<br><!-- End of picture text -->

**Figura 43 –** Dados enviados para backend via API. Fonte: [Autor] 

### **5.5.2. Comunicação entre Backend(Node.JS) e API REST** 

O backend comunica periodicamente com as APIs responsáveis pelas medições dos sensores através de requisições HTTP GET, executadas automaticamente a cada 30 segundos. Em cada ciclo, consulta os endpoints disponibilizados pelo Node-RED, recebendo os dados em formato JSON com parâmetros como temperatura, humidade, pressão atmosférica e partículas em suspensão. 

Após a receção, os dados são validados, processados e, quando necessário, utilizados para calcular parâmetros derivados, como o Índice de Qualidade do Ar (IQAr). De seguida, a informação é armazenada no MongoDB. Em caso de falha na comunicação ou indisponibilidade do sensor, o backend atualiza temporariamente o estado do dispositivo, retomando a verificação no ciclo seguinte. 

Desta forma, o backend atua como um módulo centralizado de recolha e processamento, garantindo a disponibilização contínua de dados atualizados e fiáveis para visualização em tempo real no dashboard. 

### **5.5.3. Comunicação entre Backend e Mongo DB** 

A aplicação frontend desenvolvida em Angular não acede diretamente ao MongoDB. A comunicação é realizada através do backend desenvolvido em Node.js, que atua como camada de abstração e validação. Utilizando o Express.js para o roteamento e o Mongoose para o acesso ao MongoDB, o backend disponibiliza uma API REST para consulta, inserção e atualização de dados. 

Sempre que o dashboard necessita de informação, envia requisições HTTP ao backend, que processa os pedidos, interage com o MongoDB e devolve a resposta em formato JSON. Este modelo garante maior segurança, isolamento da base de dados e uma comunicação eficiente entre o frontend e a base de dados. 

61 

### **5.5.4. Comunicação entre Frontend e Backend** 

A **Figura 44** apresenta o fluxo de comunicação entre o utilizador e o sistema de monitorização. O processo inicia-se quando o utilizador acede ao dashboard e solicita a consulta das leituras dos sensores. O Angular Service envia uma requisição HTTP GET para o endpoint /api/sensors, disponibilizado pelo backend desenvolvido em Node.js. O backend processa o pedido, consulta a base de dados MongoDB e devolve os resultados em formato JSON, que são apresentados na interface. O Angular comunica exclusivamente com a API do backend, não acedendo diretamente à base de dados. 

Após a receção, o Angular Service trata os dados e disponibiliza-os aos componentes da interface, onde são apresentados no dashboard. Este fluxo permite uma visualização dinâmica e atualizada das condições ambientais monitorizadas. 



<!-- Start of picture text -->
Comunicacao Frontend-Backend<br>|_Sollcttar dados__<br>Request<br>Query sensores<br>Dados<br>JSON Response<br>ae Observable data<br>Dados processados,<br>User interface Angular Service bttpclient Backend API Database<br><!-- End of picture text -->

**Figura 44** - Comunicação Frontend-Backend. Fonte[Autor] 

### **5.6. Fluxo geral do sistema** 

A **Figura 45** apresenta uma visão global da arquitetura do sistema proposto, destacando a interação entre sensores IoT, backend, base de dados e frontend para a monitorização ambiental. 

62 



<!-- Start of picture text -->
Sensores loT<br>Backend Collector<br>QI Calculator<br>|<br>Mongob8<br>REST APL<br>Angular Frontend<br>Dashboard U! Polting Timer<br><!-- End of picture text -->

**Figura 45** - Fluxo geral do sistema. Fonte: [Autor] 

### **5.7. Servidor** 

Para suportar o funcionamento contínuo da estação de monitorização e garantir a disponibilidade dos serviços associados ao processamento e armazenamento de dados, foi utilizado um Servidor local dedicado à execução dos principais componentes da infraestrutura digital do projeto. O servidor foi configurado para alojar os seguintes sistemas essenciais : 

- **Node-RED** , responsável pelo fluxo de receção, tratamento e encaminhamento dos dados provenientes da estação ; 

- **MongoDB** , utilizado como base de dados principal para armazenamento das medições ; 

- **Ambiente Node.js** para execução da API REST que interliga o backend ao frontend ; 

- **Angular,** utilizado no desenvolvimento de frontend para visualização dos dados. 

A centralização dos serviços num servidor local facilita a gestão do fluxo de dados, a manutenção da plataforma e o armazenamento histórico das medições. O sistema utiliza o Ubuntu Server 24.04.2 LTS para alojar os serviços necessários ao funcionamento da plataforma. No entanto, a estabilidade, a segurança e a disponibilidade dependem também da configuração das portas de comunicação, das regras de firewall, dos mecanismos de autenticação, da gestão de credenciais, das atualizações do sistema e das rotinas de cópia de segurança. 

63 

## **Capítulo 6 –Testes funcionais e Análise dos Resultados** 

Este capítulo apresenta os ensaios realizados ao protótipo de monitorização ambiental e à plataforma AirCV. Os ensaios tiveram dois objetivos complementares: 

- verificar, através de testes funcionais, que cada componente do sistema executa a função para a qual foi concebido — aquisição, transmissão, armazenamento, visualização, gestão de estações e geração de alertas; 

- quantificar o desempenho da cadeia de comunicação e da estação, através de indicadores como a taxa de entrega de mensagens, a latência, a disponibilidade e a autonomia. 

Os ensaios descritos neste capítulo são de natureza funcional e de desempenho. Não incluíram comparação com instrumento de referência, pelo que não permitem afirmar a exatidão, a precisão ou o estado de calibração dos sensores. Os valores ambientais apresentados devem, por isso, ser interpretados como indicativos. 

O capítulo está organizado da seguinte forma: começa pela descrição da estação e das condições de ensaio; define o âmbito dos ensaios, separando as capacidades testadas das previstas; apresenta os testes funcionais da plataforma AirCV e a integração de diferentes tipos de estações; analisa o desempenho da comunicação. 

### **6.1. Implantação da estação e condições de teste** 

A estação de monitorização foi instalada no laboratório de Engenharia Eletrotécnica – Telecomunicações da Uni-CV ( **Figura 46),** num ambiente interior controlado, posicionada de forma a permitir circulação de ar junto às entradas do sensor de partículas. Este ambiente permitiu acompanhar o funcionamento do sistema, detetar anomalias e efetuar ajustes, mas reproduz as condições de uma instalação exterior (radiação solar direta, chuva, vento, poeiras, variações térmicas acentuadas). 

64 



<!-- Start of picture text -->
“<br>: a<br>MODULAIR™PM<br><!-- End of picture text -->



<!-- Start of picture text -->
—— Hee,<br>5<br>q , aM —_—}—<br>. —— y =<br>Oy<br><!-- End of picture text -->

**Figura 46  -** Estação de monitorização ambiental. Fonte: [Autor] 

A **Figura 46** apresenta a configuração da estação de monitorização utilizada nos ensaios laboratoriais, correspondente à arquitetura final implementada. O protótipo integra: 

- o microcontrolador ESP32; 

- o sensor de partículas OPC-N3 (PM1.0, PM2.5 e PM10); 

- o sensor DHT22 (temperatura e humidade relativa); 

- o sensor BMP180 (pressão atmosférica); 

- um módulo de cartão SD para armazenamento local; 

- um sistema de alimentação composto por painel solar e bateria recarregável. 

#### **Alimentação e comunicação** 

A alimentação do sistema foi assegurada por um painel fotovoltaico de 6 V e 9 W, ligado ao módulo de carregamento solar Adafruit 6106, baseado no circuito integrado BQ25185. Este módulo gere o carregamento da bateria e integra um conversor elevador (boost) que disponibiliza uma saída regulada de 5 V, utilizada para alimentar o ESP32, o sensor OPC-N3 e o módulo de cartão SD. 

A comunicação foi realizada por Wi-Fi, utilizando o protocolo MQTT para a transmissão das medições, devido à simplicidade e ao controlo proporcionados pelo ambiente laboratorial. O módulo SIM800L (GSM/GPRS), previsto como alternativa ao Wi-Fi, não foi integrado nem 

65 

ensaiado. Do mesmo modo, a autonomia energética da estação em condições reais de instalação exterior não foi avaliada nesta fase. 

#### **Condições de ensaio** 

A **Tabela 18** resume as condições e a configuração usadas nos ensaios. Os campos assinalados entre parênteses retos devem ser completados com os valores registados durante o ensaio. 

**Tabela 18 -** Condições e configuração dos ensaios. Fonte [Autor] 

|**Parâmetro**|**Valor**|
|---|---|
|Local|Laboratório de Eng. Eletrotécnica – Telecomunicações, Uni-CV<br>(interior)|
|Protocolo de mensagens|MQTT|
|Intervalo de aquisição dos<br>sensores|60 s|
|Broker MQTT, porta, TLS e<br>QoS|Broker mosquito no servidor local(GS_broker) , 1880, sem<br>TLS, QoS 2|
|Tópico(s) MQTT e formato da<br>mensagem|estacion_data – JSON|
|Ligação de rede|rede Wi-Fi usada, RSSI médio em dBm|
|Sincronização de relógio|NTP no ESP32|
|Alimentação durante o ensaio|Painel solar/fonte USB + bateria ; capacidade da bateria em<br>6600mAh|
|Sensores ativos|OPC-N3, DHT22, BMP180|
|Comunicação GSM/GPRS|Não integrada / não ensaiada|



**6.2. Âmbito dos ensaios: capacidade testadas e capacidades previstas** 

Para evitar que funcionalidades previstas sejam lidas como funcionalidades concluídas, a Tabela 18 separa o que foi efetivamente ensaiado do que está apenas preparado na plataforma ou previsto como trabalho futuro. 

66 

**Tabela 19 -** Estado de validação das capacidades do sistema. Fonte [Autor] 

|**Capacidade**|**Estado**|**Observação**|
|---|---|---|
|Aquisição PM1.0/PM2.5/PM10<br>(PMS5003)|Testado (funcional)|Sem comparação com referência<br>Figura 50|
|Aquisição de temperatura, HR<br>(DHT22) e pressão (BMP180)|Testado (funcional)|Sem comparação com referência<br>Figura 50|
|Altitude|Derivada|Calculada a partir da pressão;<br>não é uma medição<br>independente. Figura 50|
|Transmissão Wi-Fi/MQTT → API<br>→ base de dados|Testado(funcional)|Figura 51|
|Visualização em tempo quase real<br>(mapa)|Testado(funcional)|Figuras 52 e 53|
|Registo, configuração e gestão de<br>estações|Testado(funcional)|Figuras 49, 50 e 51|
|Autenticação Keycloak e perfis de<br>acesso|Testado(funcional)|Figura 48|
|Alertas de qualidade do ar e de<br>estação offline|Testado (funcional)|Figuras 54, 55 e 56|
|Monitorização operacional (bateria,<br>painel, sensores)|Testado (funcional)|Figura 59|
|Armazenamento local em SD e<br>reenvio após falha|Parcialmente testado|Armazenamento integrado;<br>reenvio automático não validado<br>experimentalmente|
|Estação meteorológica (Weather<br>Station)|Testado com estação<br>física|Figura 63|
|Comunicação GSM/GPRS<br>(SIM800L)|Previsto|Não integrado nem ensaiado|
|Sensores de CO, NO , O , SO ,<br>₂<br>₃<br>₂<br>vento e precipitação|Previsto|A plataforma apenas reserva os<br>campos|
|Autonomia em instalação exterior|Não avaliado|Ensaio em laboratório|



67 

### **6.3. Testes funcionais da plataforma AirCV** 

A plataforma AirCV foi utilizada para receber, armazenar, organizar e apresentar os dados transmitidos pela estação. A arquitetura integra o fluxo de aquisição e encaminhamento no NodeRED, uma API desenvolvida em Node.js/Express e uma base de dados MongoDB. O frontend, desenvolvido em Angular, consulta os serviços disponibilizados pela API, não acedendo diretamente à base de dados. Os dados transmitidos pela estação foram integrados na aplicação web desenvolvida em Angular, permitindo a visualização das concentrações de PM1.0, PM2.5 e PM10, bem como da temperatura, humidade relativa, pressão atmosférica, altitude estimada e índice de qualidade do ar (IQA). A plataforma organiza a informação em painéis estruturados, facilitando a consulta das medições e a análise das variações ambientais. 

A plataforma disponibiliza uma área pública (mapa e consulta das estações públicas) e uma área reservada, com acesso autenticado através do Keycloak, para administradores e investigadores 

#### **Mapa interativo** 

Por predefinição, conforme ilustrado na **Figura 47** , o dashboard apresenta um mapa interativo destinado à localização geográfica das estações de monitorização registadas. Na configuração apresentada, o mapa não contém marcadores, uma vez que ainda não existem estações registadas. A validação da integração geográfica deverá incluir um teste com coordenadas conhecidas, a verificação dos limites válidos de latitude e longitude e a confirmação da associação entre cada estação e as respetivas medições. Deverá também ser previsto o tratamento de estações sem coordenadas válidas ou sem localização definida. No protótipo desenvolvido, as coordenadas são configuradas manualmente, não sendo utilizado um módulo GPS para determinar automaticamente a posição geográfica. Após o registo e a ativação das estações pelo administrador, a apresentação dos dados deverá respeitar as permissões de acesso definidas. 

68 



**Figura 47** - Mapa interativo para visualização das estações de monitorização disponíveis. Fonte: [Autor] 

69 

#### **Autenticação e gestão de utilizadores** 

A autenticação dos utilizadores é delegada no Keycloak, conforme ilustrado na **Figura 48** , responsável pela gestão de identidades e pelo processo de início de sessão. O acesso às funcionalidades da plataforma depende do perfil atribuído a cada utilizador. O painel de perfil permite consultar as informações da conta. Os administradores dispõem de permissões adicionais que lhes permitem criar utilizadores, atribuir perfis, definir palavras-passe iniciais e gerir as estações de monitorização. 



<!-- Start of picture text -->
Perfil<br>do Utilizador<br>FD) Aamin airy<br>2<br>Registode Utilizadores<br><!-- End of picture text -->



<!-- Start of picture text -->
one<br>AirCV<br>Faga login na sua conta.<br>= e mim tae mii<br><!-- End of picture text -->

**Figura 48** – Autenticação Keycloak. Fonte: [Autor] 

#### **Registo e gestão de estações** 

O registo de estações é uma funcionalidade reservada aos administradores. Para adicionar uma estação à plataforma, o administrador introduz informações como o nome, o tipo de estação (meteorológica, de qualidade do ar ou híbrida), a localização, as coordenadas, o estado inicial e o tipo de dispositivo, conforme ilustrado na **Figura 49** . 

Após o registo, a estação passa a dispor de uma área de gestão ( **Figura 50** ), na qual são apresentados as variáveis recebidas e o estado da comunicação (Online/Offline). Esta área disponibiliza também ações de edição, configuração, reinício, exportação e remoção. No modal «Configurar» ( **Figura 51** ), podem ser definidos o endpoint da API, os parâmetros de autenticação 

70 

e o intervalo de atualização. As coordenadas são introduzidas manualmente, uma vez que a estação não dispõe de um recetor GNSS. 



<!-- Start of picture text -->
) Adicionar Novo Sensor x<br>Howe D0 sensor + 10 DE seNsoR +<br>Estordo-ArcV -UNICY le Hino (comp) .<br>Locauizacho*<br>Pras, Santiago<br>LOCALIZAGOES RAPIOAS<br>uantvee<br>Lonorruoe+<br>aor? 235022<br>varus wrciat 10 DE oisposiivo<br>© ontne = 5<br>woDELo (oPcionat<br>DescRIGAO (OPCIONAL<br><!-- End of picture text -->

**Figura 49 -** Registo de um novo sensor na plataforma. Fonte: [Autor] 



<!-- Start of picture text -->
Estacao - Air - UNICV ome 0<br>) METEOROLOGIA<br>rere e33% 1008.5 hee<br>ca6m tom cade Sem dades<br>sem dedes<br>© QUALIDADE DO AR<br>135 ygim® 301)g/m 34 palm?<br>sem dads sem dads sem dados<br>sem dads<br>& coe) (Ch emer) (4 memecare<br><!-- End of picture text -->

**Figura** 50 **-** Área de gestão de sensores. Fonte: [Autor] 

71 



<!-- Start of picture text -->
© Endpointda API<br>ice<br>Senanime<br>Sore<br>$ Coleta de Dados<br>1 .<br>sl Métricas Especificas<br>0 eco signlQuaty ©Directodo Verto<br>1p © Directodo Vento /,  Velocidadedo Velocidadedo<br>= (Valor) = (Texto)<br><!-- End of picture text -->

**Figura 51 -** Interface de configuração do sensor e ligação à API. Fonte: [Autor] 

#### **Visualização pública em quase tempo real** 

O módulo Mapa, de acesso público, apresenta as estações registadas na sua posição geográfica, coloridas de acordo com a classe de qualidade do ar ( **Figura 52** ). Ao selecionar uma estação, é apresentado um modal com o estado da ligação, as variáveis meteorológicas e a avaliação indicativa da qualidade do ar, incluindo o poluente determinante ( **Figura 53** ). 

72 



**Figura 52 -** Módulo Mapa para visualização da localização da estação. Fonte: [Autor] 

73 



**Figura 53 -** Módulo Mapa para visualização dos dados em tempo real. Fonte: [Autor] 

74 

#### **Alertas** 

A plataforma gera automaticamente alertas quando uma estação deixa de comunicar ou quando a qualidade do ar ultrapassa os limiares definidos. A notificação surge sob a forma de faixa no topo da interface ( **Figura 54** ) e fica registada no módulo Alertas, onde pode ser filtrada por severidade e tipo ( **Figuras 55 e 5** 6). 



**Figura 54 –** Geração automáticas das Alertas. Fonte: [Autor] 

75 



<!-- Start of picture text -->
PAeSiLSEstag&o Offline VER DETALHES x<br>€ Dashboard & Alertas © Todos Ctico-—=—«Alto.©=—=«Médio. «=» «Baixo. Tipos. ¥ Todos ¥ Buscar (titulo, TEEN, ser<br>OC @ & Estacao Offline (5D<br>A estado Estacao - AirCV - UNICV esté offline S<br>nT min- Estagéo- AlrcV - UNICV<br><!-- End of picture text -->

**Figura 55 –** Módulo Alertas sobre estado de indisponibilidade da estação. Fonte: [Autor] 



<!-- Start of picture text -->
PMO - Qualidadedo Ar: Moderada<br>€ Dashboard & Alertas O Todos Critico Alto Médio Baixo Tipos ¥ Todos ¥ Buscar (titulo, mensagem, ser<br>OC @ 4% © PMO- Qualidadedo Ar: Moderada CID @<br>Poluente: PM10 Valor: 54.66592 g/m? $ Localiza¢ao: Praia, Santiago lz Estacdo: Estacdo - AirCV - UNICV § Recomendagées: Populagao em geral: Aproveite suas atividades ao ar livre &<br>habituais. Populacées sensiveis: Considere reduzir atividades intensas ao ar livre caso apresente sintomas.<br>11min EstacBo - ACV - UNICV<br><!-- End of picture text -->

**Figura 56 –** Módulo Alertas sobre o estado da qualidade do ar. Fonte: [Autor] 

76 

#### **Módulos de investigação** 

Os investigadores dispõem ainda dos módulos Análise de Dados ( **Figura 57** ) e histórico ( **Figura 58** ), com indicadores agregados, histogramas, mapas de calor, diagramas de dispersão. 



<!-- Start of picture text -->
€ Dashboard il Analise de Dados Estacdo-arcv-unicv v (IB zn res 200:<br>33.9°C 53% 1<br>& ScatterPlot - Correlagées, Tenwenna vx 40 [i @ Heatmap- Padrées Hordrios Tempera»<br>7 ‘ou oo<br>j sex on<br>:<br>Tt<br>lil Histograma<br>- Distribuicao Temperatura v @ Box Plot - Estatisticas{ENn) “1 unas<br><!-- End of picture text -->

**Figura 57  -** Módulos de investigação: Análise de Dados . Fonte: [Autor] 

77 



<!-- Start of picture text -->
[ eo re<br>B Moserado<br>fim<br>Muto rin<br>Extremamente ruin<br>oa Sexta, 6/11/2026 12:51<br>li Histérico de Dados Estagdo-AirCV-UNICV (Praia, an. v | 3dias v<br>) Temperatura © Pressio A Altitude © Humidade = aa<br>© Humidade . 7 Barémetro<br>‘S Particulas => Gases<br><!-- End of picture text -->

**Figura 58** – Módulos de investigação: Históricos dos dados. Fonte: [Autor] 

78 

#### **Monitorização do estado operacional da estação de monitorização ambiental** 

O modal de monitorização (Figura 60) permite acompanhar remotamente as principais variáveis e o estado operacional da estação, incluindo as tensões do painel solar e da bateria, a qualidade do sinal, o estado dos sensores e o funcionamento do cartão SD. Na captura apresentada, a bateria registava uma tensão de 4,00 V, com uma percentagem indicada de 80% e o estado de alimentação «Carregando». O painel solar encontrava-se ativo, com uma tensão de 4,70 V e uma eficiência de carregamento de 85%. Relativamente aos componentes de armazenamento e aquisição de dados, o cartão SD apresentava o estado «OK», assim como os sensores de qualidade do ar, BMP e DHT. Por sua vez, o campo «Status GSM/GPRS» não apresentava dados, uma vez que o módulo de comunicação não foi integrado na implementação final. Estes indicadores demonstram a utilidade do modal como ferramenta de supervisão e diagnóstico remoto da estação. 



<!-- Start of picture text -->
Monitorizacao - Estacao - AirCV - UNICV x<br>1 Graficos de Energia © Anélisede Bateria.<br>470V 4.00<br>Sem dados 75%<br>ESP32-001 80%<br>Cheia Carregando<br>Painel Solar Ativo 85%<br>oK EDOM<br>oK<br><!-- End of picture text -->

**Figura 59 -** Estado operacional da estação de monitorização ambiental. Fonte: [Autor] 

79 

### **6.4. Desempenho da comunicação e da estação** 

A <mark>avaliação do desempenho da estação teve como objetivo analisar a fiabilidade da comunicação, a disponibilidade do sistema e a capacidade de recuperação perante interrupções da ligação à rede. Foram considerados indicadores relacionados com a entrega de mensagens MQTT, a latência, a disponibilidade, a duração das interrupções, a recuperação dos dados armazenados e a autonomia energética</mark> . 

<mark>Os indicadores foram calculados através de um fluxo desenvolvido no Node-RED, responsável por analisar os dados recebidos e contabilizar os eventos de comunicação. A</mark> **<mark>Tabela</mark> 1** **<mark>9</mark>** <mark>apresenta os indicadores utilizados e os respetivos critérios de avaliação.</mark> 

**Tabela 20 -** Indicadores de desempenho da comunicação e da estação. Fonte [Autor] 

|**Indicador**|**Critério de avaliação**|
|---|---|
|Taxa de entrega MQTT|Relação entre o número de mensagens recebidas e o número de<br>mensagens esperadas.|
|Mensagens duplicadas|Percentagem de mensagens recebidas que correspondem a<br>retransmissões ou registos repetidos.|
|Latência de comunicação|Diferença entre o instante de aquisição da medição e o instante<br>de receção no servidor.|
|Disponibilidade|Relação entre o tempo com comunicação ativa e o tempo total<br>de observação.|
|Duração das interrupções|Análise do número de interrupções, da sua duração média e da<br>duração máxima.|
|Recuperação de dados offline|Verificação dos registos armazenados durante as falhas e<br>posteriormente transmitidos, considerando a preservação dos<br>timestamps e a ausência de duplicados.|
|Autonomia energética|Tempo de funcionamento da estação desde a carga inicial da<br>bateria até ao encerramento do sistema.|



80 

#### **Análise dos resultados** 

A **Figura 60** apresenta os indicadores calculados pelo fluxo Node-RED, com base nas mensagens recebidas até 11/11/2026, às 12:15:20. O período de funcionamento contabilizado foi de 1103,6 minutos (aproximadamente 18,4 horas), sem registo de interrupções de comunicação. 



<!-- Start of picture text -->
03/11/2026, 10:59:26 node: debug 36 .<br>estation_data : msg payload : Object<br>vobject<br>taxa_entrega_pct: 100<br>msg_recebidas: 1004<br>msg_perdidas: @<br>msg_esperadas: 1004<br>msg_duplicadas: 204<br>lat_media_ms: 292.6 git<br>lat_min_ms: 49<br>lat_max_ms: 38635<br>lat_amostras: 1000<br>lat_ultima_ms: 317<br>disponibilidade_pct: 87.65<br>uptime_total_min: 1160.6<br>downtime_total_min: 143.3<br>num_gaps: 5<br>reconn_media_s: 1719.8<br>reconn_max_s: 4324.5<br>reconn_amostras: 5<br>~ultimo_gap: object<br>inicio: "2026-11-<br>02718: 26:57.565Z"<br>fim: "2026-11-@2T19:39:02.0422"<br>duracao_s: 4324.5<br>msgs_perdidas: 71<br><!-- End of picture text -->

**Figura 60 -** indicadores calculados pelo fluxo Node-RED. Fonte [Autor] 

#### **a) Taxa de entrega MQTT e mensagens duplicadas** 

De acordo com os indicadores apresentados no Node-RED, foram contabilizadas 1104 mensagens recebidas para 1104 mensagens esperadas. Não foram registadas mensagens perdidas nem duplicadas, correspondendo a uma taxa de entrega MQTT de 100 %. 

Estes resultados indicam que, durante o período analisado, todas as mensagens esperadas foram recebidas pelo fluxo, sem ocorrência de registos repetidos. A ausência de duplicados traduz o comportamento observado neste ensaio, não garantindo, por si só, que o sistema esteja isento desse fenómeno em condições de falha ou de retransmissão. 

81 

#### **b) Latência da comunicação** 

A latência foi calculada com base em 1104 amostras, tendo sido obtido um valor médio de 286,6 ms, um mínimo de 113 ms e um máximo de 455 ms. 

A diferença reduzida entre os valores mínimo e máximo demonstra uma variação limitada dos atrasos durante o período observado. A latência média inferior a 300 ms indica uma receção das mensagens com atraso reduzido, adequada à monitorização periódica das variáveis ambientais. 

O campo lat_ultima_ms apresentou o valor de 196 ms na última medição considerada. Em conjunto, os 1104 registos de latência permitem caracterizar o comportamento da comunicação neste ensaio. 

#### **c) Disponibilidade da comunicação** 

Durante o período de observação, o fluxo contabilizou 1103,6 minutos de comunicação ativa e zero minutos de interrupção. Não foram identificadas lacunas de comunicação (num_gaps = 0). Considerando o tempo total de observação, a disponibilidade é calculada através da seguinte expressão: 

Disponibilidade = (T_online / (T_online + T_offline)) × 100 

Disponibilidade = (1103,6 / (1103,6 + 0)) × 100 

#### **Disponibilidade = 100 %** 

O resultado demonstra que a estação manteve a comunicação ativa durante todo o período contabilizado pelo fluxo Node-RED. Assim, a disponibilidade observada neste ensaio foi de 100 %. 

#### **d) Duração das interrupções de comunicação** 

Os campos reconn_media_s, reconn_max_s e reconn_amostras permitem caracterizar a duração das interrupções registadas pelo fluxo Node-RED. Na captura analisada, os três indicadores apresentam o valor zero. 

Uma vez que não foram identificadas interrupções, não foi possível calcular uma duração média ou máxima, nem avaliar o tempo de reconexão. 

Deste modo, os resultados apenas demonstram a ausência de interrupções durante o período observado. A avaliação do tempo de reconexão exige um ensaio controlado, com registo do instante de reposição da rede e do instante de receção da primeira mensagem após essa reposição. 

82 

#### **e) Recuperação dos dados offline** 

O sistema prevê o armazenamento das medições no cartão SD durante as falhas de comunicação, permitindo o seu posterior envio após o restabelecimento da ligação. A avaliação deste mecanismo requer a comparação entre os registos gravados localmente e os registos posteriormente recebidos e armazenados na base de dados, verificando a quantidade recuperada, os timestamps e a existência de duplicados. 

Como não foram registadas interrupções nem lacunas de comunicação neste ensaio, a captura da **Figura 60** não permite avaliar o funcionamento da recuperação de dados offline. Para esse efeito, é necessário provocar uma interrupção controlada da rede e verificar o posterior reenvio dos dados armazenados. 

#### **Síntese dos resultados** 

No período analisado, a estação registou uma taxa de entrega MQTT de 100 %, com 1104 mensagens recebidas, nenhuma mensagem perdida e nenhuma mensagem duplicada. A latência média foi de 286,6 ms, com valores entre 113 ms e 455 ms. A disponibilidade calculada foi de 100 %, uma vez que não foram registadas interrupções durante os 1103,6 minutos contabilizados. 

Os resultados demonstram um desempenho estável da comunicação durante o ensaio. Contudo, a recuperação de dados offline, o tempo de reconexão e a autonomia energética não podem ser confirmados através desta captura, devendo ser avaliados em ensaios específicos. 

### **6.5. Registo e integração de diferentes tipos de estações de monitorização ambiental** 

A plataforma AirCV foi concebida para suportar diferentes tipos de estações de monitorização ambiental, classificadas como meteorológicas, de qualidade do ar ou híbridas. Para esse efeito, o modelo de dados inclui campos para variáveis que a estação desenvolvida neste trabalho não mede, como a velocidade e a direção do vento, a precipitação e as concentrações de CO, NO₂, O e SO₃ ₂. Enquanto não recebem valores, estes campos são apresentados na área de gestão com a indicação «Sem dados». 

Importa distinguir dois níveis. O primeiro é o registo de uma estação na interface, em que se indicam o nome, o tipo de sensor, a localização, as coordenadas e o tipo de dispositivo. O segundo é a integração efetiva, que só se verifica quando a plataforma recebe, valida e apresenta os dados enviados pela estação, associados ao seu identificador ( _device_id_ ). O registo, por si só, não demonstra a integração; por isso, o ensaio descrito abaixo cobre ambos os níveis. 

83 

A integração faz-se através da configuração de um endpoint por estação. Nessa configuração indica-se o URL do endpoint e, opcionalmente, uma chave de autenticação; a plataforma testa a ligação e lista os campos disponíveis na mensagem recebida. Em seguida, selecionam-se as categorias de dados a recolher (dados meteorológicos e/ou de qualidade do ar) e o intervalo de atualização. Para que um novo tipo de estação seja aceite, a mensagem tem de respeitar o formato esperado pela API, que define os campos obrigatórios, os tipos de dados, as unidades de medida e as regras de validação de cada variável. 

Para validar esta capacidade, foi registada na plataforma uma estação do tipo meteorológico, designada Weather Station, localizada na Praia, Santiago, UNICV ( **Figura 61).** 

**Registo da Estação Meteorológica - Weather Station** 



<!-- Start of picture text -->
@) Adicionar Novo Sensor 4<br>NOME DO SENSOR * TIPO DE SENSOR *<br>Weather Station \. Meteorolégico ¥<br>LOCALIZAGAO *<br>Praia, Santiago<br>LOCALIZACOES RAPIDAS:<br>LATITUDE * LONGITUDE *<br>14.92238 -23.54818 ><br>STATUS INICIAL TIPO DE DISPOSITIVO<br>© Oniine . Configuravel v<br>MODELO (OPCIONAL)<br>Ex: WeatherPro 300<br>DESCRICAO (OPCIONAL)<br>4<br>Cancelar<br>Se<br><!-- End of picture text -->

**Figura 61 –** Registo da Estação Meteorológica. Fonte: [Autor] 

84 

A estação foi associada a um endpoint próprio, disponibilizado através do Node-RED, com intervalo de atualização de 1 minuto. O teste de ligação foi bem-sucedido e confirmou a disponibilidade dos campos enviados pela estação ( **Figura 62** ). 

#### **Endpoint da API responsável para receção dos dados de Weather Station** 



<!-- Start of picture text -->
* Configurar Sensor: Weather Station x<br>@ Endpoint da API<br>URL DO ENDPOINT<br>http://172.16.2.169:1880/Station<br>CHAVE DA API (OPCIONAL)<br>Chave de autenticagao<br>Conex4o bem-sucedida! Dados disponiveis: device_id, timestamp, longitude,<br>latitude, location, temperature, humidity, pressure, altitude, wind_direction,<br>wind_direction_text, wind_direction_name, wind_direction_full, wind_speed_kmh,<br>wind_speed_ms, wind_speed_desc, beaufort, rpm, rain_ml, rain_mm,<br>rain_intensity, rain_tips, rain_total_ml, voltage_vcc, voltage_bat, battery_percent,<br>battery_status, power_status, solar_status, charging_efficiency, signal_quality,<br>connection_type, status, time_synced<br>$ Coleta de Dados<br>“~ Dados Meteorolégicos<br>OB. Qualidade do Ar<br><!-- End of picture text -->

**Figura 62 –** API responsável para receção dos dados de Weather Station. Fonte: [Autor] 

Os dados recebidos foram apresentados em tempo quase real no mapa da plataforma, quer em valores numéricos ( **Figura 63** ), quer em forma descritiva, por exemplo «Muito quente», «Vento calmo» e «Sudoeste (SW)» ( **Figura 64** ). Foi também possível acompanhar o estado operacional 

85 

da estação ( **Figura 65** ), identificada pelo device_id «Weather_Station_Unicv», incluindo a tensão do painel solar e da bateria, a percentagem e o estado da bateria, a eficiência de carregamento e a qualidade do sinal. A métrica «Tensão de saída (ESP32)», sem valor na mensagem recebida, surge como «Sem dados», o que confirma que a plataforma trata corretamente campos em falta. 

Este ensaio demonstra que a plataforma permite registar uma estação de outro tipo, receber os seus dados através de um endpoint dedicado e apresentá-los sem alterações à sua arquitetura. A integração de sensores de CO, NO₂, O e SO na estação desenvolvida não foi realizada e₃ ₂ constitui trabalho futuro. 

86 

**Visualização de dados da Weather Station** 



**Figura 63 –** Visualização dos dados em tempo real da Weather Station. Fonte: [Autor] 

87 



**Figura 64 -** Visualização dos dados em tempo real da Weather Station. Fonte: [Autor] 

88 

#### **Monitorização do estado operacional da Weather Station** 



<!-- Start of picture text -->
Monitorizagao - Weather Station x<br>METRICA VALOR<br>Tensaodo painel solar 412V<br>Tensao da bateria 4i2V<br>Tensao de saida<br>|ESP32) Sem dados<br>Qualidade do sinal 25%<br>Device ID Weather_Station_Unicv<br>eedBateriaPercentagem hol da 90%<br>Status da Bateria + Cheia<br>Status de Alimentacao @ Carregando<br>Statusdo Painel Solar @ Painel Solar Ativo<br> Eficiéncia de cp<br>Carregamento<br><!-- End of picture text -->

**Figura** 65 **-** Estado operacional da Weather Station. Fonte: [Autor] 

### **6.6. Análise geral do sistema** 

Os ensaios funcionais permitiram verificar o funcionamento das principais funcionalidades da estação de monitorização ambiental e da plataforma AirCV, incluindo a aquisição das medições, a transmissão dos dados e a sua apresentação na aplicação web em tempo quase real. A plataforma permite gerir estações, consultar dados sobre a qualidade do ar e variáveis meteorológicas e apresentar alertas e informações sobre o estado dos dispositivos, de acordo com as funcionalidades implementadas. 

A validação do sistema de autenticação e controlo de acessos, baseado no Keycloak, deve considerar não só o início de sessão, mas também a atribuição de perfis, a proteção das funcionalidades administrativas e o acesso a dados privados. A avaliação global deve ainda ser sustentada por uma matriz de testes que documente os procedimentos, os resultados observados e as evidências recolhidas. Na ausência de testes automatizados e de métricas de latência, perdas de dados e duração dos ensaios, estas limitações devem ser reconhecidas. Por fim, a validação em ambiente exterior continua a ser necessária para avaliar o comportamento da estação perante diferentes condições ambientais e de comunicação. 

89 

## **Capítulo 7 – Conclusão e Trabalhos futuros** 

### **7.1.Conclusão** 

O desenvolvimento do Sistema de Monitorização Ambiental da Qualidade do Ar e Variáveis Meteorológicas demonstrou ser uma solução prática, eficiente e tecnologicamente consistente para a avaliação contínua das condições ambientais, realizada num ambiente controlado no Laboratório de Engenharia Eletrotécnica – Telecomunicações da Uni-Cv. Através da integração de sensores de baixo custo, comunicação sem fios e plataformas IoT, foi possível implementar um sistema inteligente capaz de medir, transmitir, armazenar e visualizar dados ambientais em tempo real. Os resultados obtidos demonstram a concretização dos principais objetivos do projeto, nomeadamente o desenvolvimento de um protótipo capaz de adquirir medições de material particulado (PM1.0, PM2.5 e PM10) e de variáveis meteorológicas, como temperatura, humidade relativa, pressão atmosférica e altitude estimada. A comunicação por Wi-Fi permitiu transmitir os dados para a plataforma AirCV, onde são processados, armazenados e apresentados para consulta e análise. 

#### **Impacto Quantificado** 

Do ponto de vista do impacto, o sistema permitiu a aquisição, o armazenamento e a disponibilização de dados ambientais na plataforma AirCV, possibilitando a consulta das medições e a análise das variações temporais dos poluentes atmosféricos e das condições meteorológicas. A estação publica os dados por Wi-Fi a cada 60 segundos, enquanto o backend consulta o Node-RED a cada 30 segundos. Contudo, a continuidade da recolha, a existência de falhas de comunicação e a integridade do histórico não foram validadas durante um período de ensaio prolongado, pelo que não se pode garantir a atualização contínua nem a completude dos registos. Adicionalmente, a plataforma permite o registo de diferentes tipos de estações de monitorização ambiental e disponibiliza um endpoint da API para a receção de dados, prevendo a integração de estações de qualidade do ar, meteorológicas ou híbridas, desde que respeitem o formato de dados definido. 

#### **Limitações Enfrentadas** 

Apesar dos resultados obtidos, foram identificadas algumas limitações ao longo do desenvolvimento do projeto. Uma das principais dificuldades esteve relacionada com a seleção e integração do sensor de partículas em suspensão. Durante os testes preliminares, foram avaliados diferentes modelos, nomeadamente o SPS30, o OPC-N3 e o PMS5003. O processo de seleção foi 

90 

condicionado por dificuldades de integração, o que exigiu testes comparativos para identificar a solução mais adequada aos requisitos do sistema. 

Outra dificuldade surgiu no dimensionamento do sistema de alimentação, particularmente na disponibilização de uma tensão estável de 5 V para o funcionamento do OPC-N3 e Open Lpog. A solução inicialmente considerada, baseada no módulo Adafruit com circuito BQ25185 e num conversor DC/DC, apresentou limitações devido às exigências de potência do sensor e aos picos de corrente durante o funcionamento do conversor, sobretudo à medida que a tensão da bateria diminuía. Esta situação levou à necessidade de reavaliar a arquitetura de alimentação. 

Ao nível da visualização dos dados, o desenvolvimento da interface web também apresentou desafios iniciais. Foi utilizada, numa fase preliminar, a plataforma Appsmith, mas as limitações de flexibilidade motivaram o desenvolvimento de uma aplicação web própria, baseada em Angular, Node.js e MongoDB 

### **7.2. Trabalhos futuros** 

Como desenvolvimento futuro, propõe-se a integração do módulo de comunicação GSM/GPRS SIM800L, previsto na arquitetura inicial, mas não integrado nem testado experimentalmente no protótipo desenvolvido. Esta integração poderá permitir a transmissão de dados em locais sem cobertura Wi-Fi, desde que exista cobertura de uma rede móvel compatível. Para tal, será necessário validar a alimentação elétrica, considerando os picos de corrente do módulo, a comunicação UART com o ESP32, a configuração do cartão SIM e do APN, a ligação ao broker MQTT e os mecanismos de reconexão. Deverá também ser avaliado o consumo energético durante as transmissões e o respetivo impacto na autonomia da estação. Propõe-se igualmente o reforço da segurança na transmissão e no acesso aos dados, através da implementação de comunicação MQTT com encriptação TLS e mecanismos de autenticação, nomeadamente por token, de modo a proteger a informação transmitida e restringir o acesso não autorizado ao sistema. Outra linha de desenvolvimento consiste na realização de ensaios prolongados da estação em ambiente exterior, permitindo avaliar o comportamento do sistema em condições reais, particularmente a estabilidade das medições, a continuidade da comunicação e a autonomia energética. Estes ensaios permitirão ainda analisar a influência das condições meteorológicas nas leituras dos sensores e identificar eventuais necessidades de calibração e proteção dos equipamentos. 

Por fim, propõe-se a integração de sensores adicionais para a monitorização de outros poluentes atmosféricos, nomeadamente monóxido de carbono (CO), dióxido de azoto (NO₂), ozono (O₃) e dióxido de enxofre (SO₂). Esta expansão permitirá uma caracterização mais abrangente da qualidade do ar e contribuirá para melhorar a identificação de diferentes tipos de poluição atmosférica. 

91 

# **Referências** 

- [1] «Desfrute do bom tempo em Cabo Verde o ano inteiro». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.visit-caboverde.com/sobre-cabo-verde/clima 

- [2] «Cabo Verde - INMG avisa que o país vai estar coberto por bruma seca durante alguns dias», RTP África. Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://rtpafrica.rtp.pt/noticias/cabo-verde-inmg-avisa-que-o-pais-vai-estar-coberto-porbruma-seca-durante-alguns-dias/ 

- [3] V. Marçal, «Palestra sobre “Qualidade do ar e desafios do desenvolvimento sustentável” marca Dia Mundial do Ambiente», Governo de Cabo Verde. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.governo.cv/palestra-sobre-qualidade-do-are-desafios-do-desenvolvimento-sustentavel-marca-dia-mundial-do-ambiente/ 

- [4] A. Z. da Costa, J. P. S. Aniceto, e M. Lopes, «Low-Cost Sensor Network for Air Quality Assessment in Cabo Verde Islands», _Sensors_ , vol. 24, n.<sup>o</sup> 23, p. 7656, jan. 2024, doi: 10.3390/s24237656. 

- [5] M. Cabral, «Qualidade do ar», INMG. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.inmg.gov.cv/index.php/servicos/qualidade-do-ar 

- [6] S. M. Freire, H. Relvas, e M. Lopes, «Impact of traffic emissions on air quality in Cabo Verde», _Environ. Monit. Assess._ , vol. 192, n.<sup>o</sup> 11, p. 726, out. 2020, doi: 10.1007/s10661020-08690-8. 

- [7] «Desenvolvimento de um sensor de baixo custo para monitoramento da qualidade do ar», 2019. 

- [8] M. M. Roncaglio, «Estação de baixo custo para monitoramento da qualidade do ar», dez. 2023, Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://tede.unioeste.br/handle/tede/7090 

- [9] Z. Idrees, Z. Zou, e L. Zheng, «Edge Computing Based IoT Architecture for Low Cost Air Pollution Monitoring Systems: A Comprehensive System Analysis, Design Considerations & Development», _Sensors_ , vol. 18, n.<sup>o</sup> 9, p. 3021, set. 2018, doi: 10.3390/s18093021. 

- [10] «Air pollution». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.who.int/health-topics/air-pollution 

- [11] «Ambient (outdoor) air pollution». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.who.int/news-room/fact-sheets/detail/ambient-(outdoor)-airquality-and-health 

92 

- [12] «State of Global Air Report 2024 | State of Global Air». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.stateofglobalair.org/resources/archived/stateglobal-air-report-2024 

- [13] «Calidad del aire ambiente (exterior) y salud». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.who.int/es/news-room/fact-sheets/detail/ambient(outdoor)-air-quality-and-health 

- [14] «Types of pollutants». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.who.int/teams/environment-climate-change-and-health/air-quality-andhealth/health-impacts/types-of-pollutants 

- [15] «Air Pollution Note – Data you need to know». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.unep.org/interactives/air-pollution-note 

- [16] «Microsoft Word - AECV 2022.docx». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://ine.cv/wp-content/uploads/2025/01/aecv-2022-versao-final.pdf 

- [17] «aecv-2017-versao-final-1.pdf». Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://ine.cv/wp-content/uploads/2017/12/aecv-2017-versao-final-1.pdf 

- [18] «NAP_Cabo Verde_PT.pdf». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://unfccc.int/sites/default/files/resource/NAP_Cabo%20Verde_PT.pdf 

- [19] «Monitoramento da qualidade do ar», Excelência Ambiental. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://excelenciaambiental.com.br/monitoramento-daqualidade-do-ar-2/ 

- [20] «Dados do Monitoramento Contínuo da Qualidade do Ar - FEAM - SISEMA», FEAM. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://feam.br/dados-domonitoramento-cont%C3%ADnuo-da-qualidade-do-ar 

- [21] «Índice Europeu de Qualidade do Ar». Acedido: 16 de dezembro de 2025. [Em linha]. Disponível em: https://airindex.eea.europa.eu/AQI/?utm_source=chatgpt.com# 

- [22] alldatasheet.com, «ESP32 PDF». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: 

http://www.alldatasheet.com/datasheet-pdf/view/1148023/ESPRESSIF/ESP32.html 

- [23] Babaganoush07, _Babaganoush07/ESP32_Devkit_ . (8 de março de 2023). C++. Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://github.com/Babaganoush07/ESP32_Devkit 

93 

- [24] www.alldatasheet.com, «DHT22 datasheet(2/10 Pages) ETC2». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: http://www.alldatasheet.com/html-pdf/1132459/ETC2/DHT22/225/2/DHT22.html 

- [25] «Figure 2.3 DHT22 Temperature Sensor DHT22 / AM-2302 Technical...», ResearchGate. Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://www.researchgate.net/figure/DHT22-Temperature-Sensor-DHT22-AM-2302Technical-Specifications-1-Power-supply-33_fig2_341290941 

- [26] A. Industries, «BMP180 Barometric Pressure/Temperature/Altitude Sensor- 5V ready». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.adafruit.com/product/1603 

- [27] «BMP180 Barometric Pressure/Temperature/Altitude Sensor- 5V ready». Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://core-electronics.com.au/bmp180barometric-pressure-temperature-altitude-sensor-5v-ready.html 

- [28] T. Tecnologia, «Módulo GSM GPRS SIM800L». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.casadarobotica.com/internet-das-coisas/comunicacao/gsm-gps/modulo-gsmgprs-sim800l 

- [29] «SIM800L GSM / GRPS module». Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://nettigo.eu/products/sim800l-gsm-grps-module 

- [30] «Sensirion_PM_Sensors_Datasheet_SPS30.pdf». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://sensirion.com/media/documents/8600FF88/64A3B8D6/Sensirion_PM_Sensors_Da tasheet_SPS30.pdf 

- [31] S. AG, «SPS30 - PM2.5 Sensor for HVAC and air quality applications SPS30». Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://sensirion.com/products/catalog/SPS30 

- [32] «Alphasense Software: OPC-R2, OPC-N3, & More - Alphasense». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.alphasense.com/Downloads/Software 

- [33] «Optical Particle Counter OPC-N3», Alphasense - B2B. Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://store.alphasense.com/opc-n3/ 

- [34] «OPC-N3». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: http://www.apollounion.com/Upload/DownFiles/202108/OPC-N3.pdf 

- [35] Z. Yong e Z. Haoxin, «Digital universal particle concentration sensor», 2016. 

94 

- [36] dimitris kokkinos, «Aprenda como funciona o sensor de partículas e como controlar o sensor PMS5003 usando Arduino - Software Particles». Acedido: 10 de dezembro de 2025. [Em linha]. Disponível em: https://softwareparticles.com/learn-how-particulate-sensorworks-and-how-to-control-the-pms5003-sensor-using-arduino/ 

- [37] A. Industries, «PM2.5 Air Quality Sensor and Breadboard Adapter Kit». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.adafruit.com/product/3686 

- [38] «5V Li-Pol/Li-Ion battery charging module bq25185 with USB Type-C, DC and solar panel inputs - Adafruit 6106», Botland. Acedido: 27 de outubro de 2026. [Em linha]. Disponível em: https://botland.store/charger-modules-for-li-po-batteries/28642-5v-li-polliion-battery-charging-module-bq25185-with-usb-type-c-dc-and-solar-panel-inputsadafruit-6106-5902186320741.html 

- [39] «Painéis solares: tipos, características, preços», REPSOL. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.repsol.pt/particulares/assessoramento/paineis-solares-tipos-carateristicaspreco/ 

- [40] «Funcionamento do painel fotovoltaico – Seja sustentável, seja cliente da EngeSol Energia Solar!» Acedido: 12 de outubro de 2026. [Em linha]. Disponível em: https://engesol.eco.br/fotovoltaico/funcionamento-do-painel-fotovoltaico/ 

- [41] «SparkFun OpenLog». Acedido: 27 de outubro de 2026. [Em linha]. Disponível em: https://www.sparkfun.com/sparkfun-openlog.html 

- [42] «Document Database - NoSQL», MongoDB. Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.mongodb.com/resources/basics/databases/documentdatabases 

- [43] «Sobre dados de séries temporais - Manual do banco de dados - MongoDB Docs». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.mongodb.com/pt-br/docs/manual/core/timeseries/timeseries-bucketing/ 

- [44] «About : Node-RED». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://nodered.org/about/ 

- [45] «Node.js — Introduction to Node.js». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://nodejs.org/en/learn/getting-started/introduction-to-nodejs 

- [46] A. Team, «What is Angular? • Angular». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://angular.dev/ 

95 

- [47] «Arduino Integrated Development Environment (IDE) v1 | Arduino Documentation». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://docs.arduino.cc/software/ide-v1/tutorials/arduino-ide-v1-basics/ 

- [48] «O que é: Visual Studio Code - Editor de Código». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://driftweb.com.br/glossario/o-que-e-visual-studio-code-editorde-codigo/ 

- [49] «Keycloak». Acedido: 21 de julho de 2026. [Em linha]. Disponível em: https://www.keycloak.org/ 

- [50] «JavaScript With Syntax For Types.» Acedido: 21 de julho de 2026. [Em linha]. Disponível em: https://www.typescriptlang.org/ 

- [51] «IBM Sterling Order Management System». Acedido: 29 de julho de 2026. [Em linha]. Disponível em: https://www.ibm.com/docs/pt-br/order-management?topic=organizationdefining-communication-protocols 

- [52] «What Is GPRS? | 1NCE». Acedido: 9 de outubro de 2026. [Em linha]. Disponível em: https://www.1nce.comen-eu/resources/iot-knowledge-base/what-is-gprs 

- [53] «Figure 3. Communication via GSM/GPRS modem.», ResearchGate. Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://www.researchgate.net/figure/Communication-via-GSM-GPRSmodem_fig2_266596544 

- [54] «O que é protocolo de comunicação UART? - Clube do Maker». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://clubedomaker.com/protocolo-uart 

- [55] R. & S. International, «Compreender UART». Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://www.rohde-schwarz.com/br/produtos/teste-e-medicao/essentials-test-equipment/ digital-oscilloscopes/compreender-uart_254524.html 

- [56] «What is the I2C Communication Protocol?», CircuitBread. Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://www.circuitbread.com/tutorials/what-is-the-i2ccommunication-protocol 

- [57] «Introdução ao SPI - Ajuda para desenvolvedores». Acedido: 16 de novembro de 2026. [Em linha]. Disponível em: https://developerhelp.microchip.com/xwiki/bin/view/applications/SPI/? utm_source=chatgpt.com 

- [58] upravnik, «HTTP and HTTPS explained», Study CCNA. Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://study-ccna.com/http-https/ 

96 

- [59] S. Getachew, «Introduction to HTTP: The Backbone of Web Communication», .Net Programming. Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://medium.com/@solomongetachew112/introduction-to-http-the-backbone-of-webcommunication-895253870c4a 

- [60] «Você sabe o que significa aquele cadeado na barra de endereço do seu navegador?», Exame. Acedido: 29 de julho de 2026. [Em linha]. Disponível em: https://exame.com/negocios/significa-aquele-cadeado-barra-de-endereco-navegador/ 

- [61] «MQTT Essentials: Your 2025 Learning Hub for IoT & IIoT Data Streaming». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.hivemq.com/mqtt/ 

- [62] «Protocolo MQTT: O Que é, Como Funciona e Vantagens». Acedido: 28 de fevereiro de 2023. [Em linha]. Disponível em: https://www.industria360.com.br/artigos/mqtt/ 

- [63] A. Al Hanif e M. Ilyas, «Effective Feature Engineering Framework for Securing MQTT Protocol in IoT Environments», _Sensors_ , vol. 24, n.<sup>o</sup> 6, p. 1782, jan. 2024, doi: 10.3390/s24061782. 

- [64] A. Industries, «Colossal 6V 9W Solar Panel». Acedido: 9 de dezembro de 2025. [Em linha]. Disponível em: https://www.adafruit.com/product/2747 

- [65] A. Industries, «Adafruit bq25185 USB / DC / Solar Charger with 5V Boost Board». Acedido: 27 de outubro de 2026. [Em linha]. Disponível em: https://www.adafruit.com/product/6106 

- [66] A. Industries, «Lithium Ion Battery Pack - 3.7V 6600mAh». Acedido: 11 de dezembro de 2025. [Em linha]. Disponível em: https://www.adafruit.com/product/353 

97 

