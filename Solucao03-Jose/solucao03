# Documentação Técnica: Sistema de Medição por Fita Metálica e Sensores Indutivos (SMFMSI)

**Projeto:** Instrumentação Angular de Cúpula Astronômica  
**Data:** Setembro 2026  
**Equipe:** José Rafael de Carvalho / LNA  

---

## 1. Detalhamento Técnico e Cinemática

O **Sistema de Medição por Fita Metálica e Sensores Indutivos (SMFMSI)** é uma arquitetura de sensoriamento industrial projetada para substituir sistemas antigos e falhos baseados em leitores ópticos e códigos de barra. O princípio fundamental do projeto baseia-se na contagem de pulsos magnéticos gerados por uma chapa ranhurada, utilizando o conceito de leitura em quadratura.

### Princípio de Funcionamento

O mecanismo principal é composto por uma fita metálica espessa instalada em toda a circunferência interna da cúpula. Essa fita é dividida visual e fisicamente em duas faixas:

- **Faixa Inferior:** Totalmente lisa, atuando como pista de rolagem para a roda guia.
- **Faixa Superior:** Usinada a laser com furos retangulares regulares (semelhante a um zíper metálico).

Para realizar a leitura, dois sensores indutivos industriais são fixados em um cabeçote de medição. Esses sensores operam detectando a presença (estado 1) ou ausência (estado 0) de metal conforme os furos passam por eles.

### Absorção de Irregularidades e Estabilidade Mecânica

Para garantir que a distância de leitura (*air gap*) entre os sensores e a fita permaneça constante mesmo com trepidações e ovalizações do prédio, o sistema utiliza um **Totem de Fixação** estrutural preso ao concreto. A partir desse totem, estendem-se **Guias Lineares** de aço por onde o cabeçote dos sensores desliza horizontalmente.

Uma mola de compressão empurra o cabeçote constantemente contra a cúpula, fazendo com que uma roda guia de contato, montada na horizontal no próprio cabeçote, deslize suavemente sobre a faixa inferior lisa da fita de metal.

Essa roda limita o avanço do sistema, garantindo um espaço de ar perfeitamente constante na casa dos **5 milímetros**.

### Formulação Matemática e Cinemática

O projeto opera contabilizando variações de estado lógico. O deslocamento angular é obtido pela quantidade de furos e pela defasagem física dos sensores.

Definindo o número total de furos na circunferência completa como $N$, a leitura em quadratura analisa as bordas de subida e descida dos dois sensores defasados, multiplicando a contagem de eventos por 4.

A resolução angular do sistema ($R_a$) é dada por:

$$
R_a = \frac{360^{\circ}}{4 \cdot N}
$$

Se a fita for usinada para comportar **1.800 furos** ao longo da circunferência da cúpula, a resolução efetiva do controlador será de **0,05° por pulso processado**.

---

## 2. Especificações do Sistema

Os parâmetros nominais de hardware adotados para a validação do SMFMSI são detalhados a seguir:

- **Estrutura da Fita:** Chapa de aço galvanizado cortada a laser em formato curvo, fixada na parede interna (côncava) da cúpula.
- **Roda Guia:** Roda orientada no eixo horizontal. Material da banda de rodagem: Poliuretano (PU) fundido. Dureza especificada: 75 a 85 Shore A (otimizado para evitar deformação que altere o *air gap*).
- **Sensoriamento:** Sensores indutivos tubulares M18 PNP com distância sensora de 8 mm.
- **Processamento de Sinais:** Controlador Lógico Programável (CLP) modelo MicroLogix 1100 Series B. Utilização das entradas de *High-Speed Counter* (HSC).
- **Ambiente de Simulação e Validação:** Lógica Ladder estruturada no RSLogix Micro Starter Lite e validada no RSLogix Emulate 500 via RSLinx Classic.
- **Mecânica Articulada:** Sistema de guias lineares duplas de aço polido, suportadas por um totem de concreto ou metal na estrutura fixa.

---

## 3. Vantagens

A adoção da fita metálica com sensores indutivos entrega as seguintes vantagens tecnológicas diretas:

- **Imunidade Ambiental Absoluta:** Sensores magnéticos não emitem nenhuma radiação luminosa que interfira na instrumentação astronômica e são completamente imunes a poeira, graxa e degradação de etiquetas.
- **Zero Ponto Cego:** Diferente de leitores de código de barras que falham ao parar entre marcações, a parada do sensor em frente a um furo ou a uma face maciça é interpretada como um estado válido, mantendo a integridade do dado.
- **Adequação Orçamentária Excepcional:** A solução atende e supera o teto estipulado de R$ 1.400,00, entregando robustez industrial com componentes de prateleira baratos.
- **Sistema de Guias Lineares:** Impede a torção mecânica e a queda do cabeçote de leitura sob a ação da gravidade, garantindo paralelismo absoluto entre os sensores e a fita.

---

## 4. Desvantagens / Restrições

Existem limitações técnicas inerentes à escolha de componentes que requerem atenção:

- **Necessidade de Calibração Pós-Apagão:** Embora o MicroLogix 1100 utilize memória retentiva para salvar o último valor lido instantaneamente durante perdas de energia curtas, a movimentação manual do teto com o painel elétrico desligado exigirá um ciclo autônomo de *Homing* (retorno a um terceiro sensor de Ponto Zero) ao religar.
- **Limite Mecânico de Resolução:** A precisão do sistema está limitada ao quão finos e próximos os furos podem ser cortados na chapa de aço pelo fornecedor sem comprometer a integridade estrutural do "zíper".
- **Desgaste da Roda Guia:** A erosão natural da roda de poliuretano, ainda que lenta, altera minimamente o *air gap* dos sensores ao longo de vários anos, necessitando de inspeção visual periódica.

---

## 5. Produtos de Mercado e Custo (BOM)

A tabela abaixo lista os componentes de mercado validados para o protótipo, destacando a economia do projeto:

| Componente | Especificações Técnicas (Mercado) | Marca/Modelo Recomendado | Preço Estimado (R$) |
|:---|:---|:---|---:|
| **Sensores Indutivos (x3)** | Tubular M18, PNP, dist. sensora 8 mm, operação 24 VCC. (2 para quadratura, 1 para Ponto Zero). | **WEG** (SL8-18G1LPA) | R$ 116,37 (Total) |
| **Fita Metálica (Zíper)** | Corte a laser em chapa fina de aço, dividida em faixa lisa e faixa perfurada. | Caldeiraria / Corte Local | R$ 250,00 - R$ 400,00 |
| **Conj. Guias e Totem** | Guias lineares de aço, mancais, mola de compressão e roda guia horizontal em PU. | Fabricação própria / Peças padrão | R$ 150,00 - R$ 300,00 |
| **Controlador e Fixação** | MicroLogix 1100 Series B, cabos blindados e bornes ferroviários. | Estoque / Indústria Diversa | R$ 100,00 - R$ 150,00 (Cabos/Acessórios) |
| **TOTAL ESTIMADO** | Protótipo mecânico funcional com margem de segurança. | - | **R$ 616,37 a R$ 966,37** |

---

## 6. Detalhamento de Instalação

Para garantir o sucesso da solução de *contactless* e o bom processamento do controlador, a equipe deve seguir estas premissas físicas no laboratório ou no campo:

### 6.1 Integração Mecânica

- **Montagem do Totem:** O suporte vertical base deve ser fixado ao piso ou à estrutura estática inferior e estar completamente nivelado. As guias lineares estendidas a partir dele precisam estar perfeitamente ortogonais à parede curva (côncava) da fita metálica.
- **Ajuste do Air Gap:** A roda de poliuretano, montada em posição horizontal, é o batente físico do mecanismo. Antes de ligar o sistema, os sensores indutivos M18 devem ser rosqueados no cabeçote até que recuem cerca de 5 mm em relação à extremidade tangencial da roda. Os sensores **nunca** podem tocar a fita.
- **Orientação da Fita:** A fita deve ser soldada ou parafusada na cúpula garantindo que a faixa inferior maciça receba o rolamento da guia, e a superior perfurada interaja com as faces sensíveis magnéticas.

### 6.2 Integração Elétrica e Lógica

- **Tratamento de Sinais Rápidos:** Os cabos dos sensores M18 devem ser ligados exclusivamente às entradas designadas para *High-Speed Counter* do hardware alvo.
- **Memória Retentiva no Ladder:** No RSLogix Micro Starter Lite, configure os blocos de contagem para escrever diretamente em arquivos inteiros do tipo N7 ou registradores dedicados para garantir retentividade. A lógica deve prever uma interrupção segura do pulso quando o CLP identificar queda de tensão.
- **Comissionamento Virtual:** Antes do *deploy* final, execute a rotina utilizando o Factory IO ancorado ao RSLogix Emulate 500 para validar as inversões bruscas de direção e verificar se a lógica Ladder não perde pulsos da quadratura.
