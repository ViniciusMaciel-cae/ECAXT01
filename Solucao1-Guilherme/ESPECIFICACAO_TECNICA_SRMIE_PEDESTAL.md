# Especificação Técnica de Engenharia: Sistema de Roda de Medição Independente com Encoder (SRMIE)
## Arquitetura em Pedestal Compacto com Atuação Linear de Base

**Documento:** ET-SRMIE-002-REV03  
**Projeto:** Automação e Instrumentação Azimutal de Cúpula Astronômica  
**Data:** Setembro de 2026  
**Instituição:** UNIFEI / LNA (Laboratório Nacional de Astrofísica)  
**Disciplina:** Engenharia Mecatrônica & Projeto de Sistemas Mecânicos  
**Classificação:** Documentação Técnica de Engenharia / Projeto Executivo  

---

## 1. Resumo Executivo e Princípio de Operação

### 1.1 Contexto e Objetivo
O rastreamento astronômico de precisão exige que a fenda de observação da cúpula acompanhe com fidelidade sub-arcminuto as coordenadas azimutais do telescópio. Em sistemas convencionais, a leitura de posição angular frequentemente depende de encoders montados no eixo dos motores de tração ou acoplados a roletes de apoio do trilho de sustentação. Tais abordagens sofrem com patinamento mecânico (*slip/creep*), deformações sob carga estática do domo e acúmulo de erros cumulativos.

O **Sistema de Roda de Medição Independente com Encoder (SRMIE)** soluciona definitivamente essa deficiência ao desacoplar $100\%$ a instrumentação metrológica da infraestrutura motora e de sustentação de carga. O subsistema opera como um sensor tátil cinemático de contato puro, cuja única finalidade é traduzir o deslocamento do anel periférico da cúpula em pulsos elétricos de quadratura.

```
       CÚPULA (ANEL DOURADO Ø 3000 mm)
                  ||
                  || [Contato Tangencial Puro]
                  \/
         +------------------+
         | Roda de Medição  |  Ø 100 mm / 200 mm (Banda PU 85-90 Shore A)
         +--------+---------+
                  |  [Eixo de Acoplamento Direto / Fole Metálico]
         +--------+---------+
         | Encoder Coaxial  |  Montado Embaixo, Apontando para Cima
         +--------+---------+
                  |  [Flange Usinada com Parafusos M4]
         +--------+---------+
         |  Haste Vertical  |  Coluna Curta Ø 40 mm (h = 240 mm, Alta Rigidez)
         +--------+---------+
                  |  [Mísulas Triangulares de Reforço Soldadas]
         +--------+---------+      Mola Horizontal (k = 12 N/mm)
         |  Sapata Móvel    |<==== [Pré-carga FN = 120 N] <== [Batente Fixo]
         +--------+---------+
            | |        | |   [4 Patins Lineares HGW20CC com Raspadores]
         ===+=+========+=+===
         [ Trilhos Guia HGR20 na Mesa Elevada / Pedestal de Fundação ]
         ====================
         PISO DO OBSERVATÓRIO (Y = 0,000 m) + MURETA ESTRUTURAL
```

### 1.2 Arquitetura Mecânica em Pedestal Compacto
A concepção final do SRMIE adota um **pedestal vertical compacto hiperestático**, operando com um único grau de liberdade translacional puramente radial ($1\text{-DOF}$ no eixo $X$):

1. **Roda de Medição Horizontal ($XZ$):** Montada na cota de topo ($Y = 0{,}856\,\text{m}$), tangenciando internamente o anel guia usinado da cúpula com folga nominal nula ($0{,}000\,\text{mm}$).
2. **Encoder Coaxial Subjacente:** Posicionado imediatamente abaixo da roda ($Y = 0{,}766\,\text{m}$), apontando verticalmente para cima com eixo retificado acoplado ao cubo da roda. Essa montagem inferior protege os elementos ópticos contra queda de detritos e garante simetria de rotação livre de balanço.
3. **Haste Vertical Compacta:** Coluna tubular usinada curta ($h = 240\,\text{mm}$, diâmetro de $40\,\text{mm}$), reforçada na base por quatro mísulas triangulares soldadas. O comprimento reduzido em cerca de $67\%$ em relação aos conceitos de coluna longa elimina momentos fletores dinâmicos e frequências de ressonância parasitas.
4. **Sapata Deslizante e Guias Lineares de Precisão:** Carro rígido de aço estrutural sustentado por quatro patins de esferas recirculantes de alta rigidez sobre trilhos lineares duplos retificados, fixados em uma mesa de apoio elevada ($Y = 0{,}470\,\text{m}$).
5. **Pilar de Fundação e Travamento na Parede:** A mesa de apoio é sustentada a partir do piso de concreto ($Y = 0{,}000\,\text{m}$) por um pilar tubular estrutural com flange de base ancorada por chumbadores Parabolt e amarrada à mureta circular do observatório por mão-francesa soldada, neutralizando qualquer momento de tombamento.

### 1.3 Dinâmica de Ação e Reação da Mola Horizontal
O contato ininterrupto da roda com a pista é assegurado por uma **mola helicoidal de compressão em aço temperado**, montada horizontalmente em linha direta de atuação ($Y = 0{,}520\,\text{m}$):
- **Extremidade Reativa:** Apoiada em um copo usinado montado em um batente vertical fixo na placa-base, reforçado por nervuras triangulares que descarregam a reação no piso e na parede do observatório.
- **Extremidade Ativa:** Acomodada no copo receptor da sapata móvel, empurrando a coluna e a roda continuamente contra a face interna do trilho da cúpula com uma força normal estática calculada ($F_N \approx 100\text{ a }150\,\text{N}$).
- **Compensação Dinâmica Sem Deflexão Angular:** Quando a cúpula gira e apresenta excentricidades sazonais, desalinhamentos térmicos ou ovalizações construtivas ($\Delta X = \pm 10\text{ a }\pm 25\,\text{mm}$), a sapata móvel desliza linearmente sobre os trilhos retificados. Como o movimento é puramente prismático ($1\text{-DOF}$), **não ocorre rotação em arco, nem inclinação angular do eixo do encoder**, preservando o ângulo de incidência tangencial a $90{,}00^{\circ}$ em qualquer ponto da pista de $360^{\circ}$.

---

## 2. Especificações Técnicas e Parâmetros Cinemáticos

### 2.1 Tabela de Dimensões e Parâmetros Geométricos

| Parâmetro Construtivo | Símbolo | Valor Nominal | Tolerância / Faixa | Unidade |
|:---|:---:|:---:|:---:|:---:|
| Diâmetro nominal do anel da cúpula | $D_{\text{cúpula}}$ | $3000{,}0$ | $\pm 5{,}0$ | $\text{mm}$ |
| Raio nominal da pista de contato | $R_{\text{cúpula}}$ | $1500{,}0$ | $\pm 2{,}5$ | $\text{mm}$ |
| Diâmetro nominal da roda de medição | $D_{\text{roda}}$ | $100{,}0$ | $\pm 0{,}02$ (após retífica) | $\text{mm}$ |
| Diâmetro alternativo para grandes cúpulas | $D_{\text{roda,alt}}$ | $200{,}0$ | $\pm 0{,}03$ | $\text{mm}$ |
| Raio nominal da roda ($D=100\,\text{mm}$) | $r_{\text{roda}}$ | $50{,}0$ | $\pm 0{,}01$ | $\text{mm}$ |
| Altura livre da haste vertical | $h_{\text{haste}}$ | $240{,}0$ | $\pm 0{,}5$ | $\text{mm}$ |
| Diâmetro externo do tubo da haste | $\varnothing_{\text{haste}}$ | $40{,}0$ | $\pm 0{,}1$ | $\text{mm}$ |
| Espessura da parede do tubo da haste | $t_{\text{haste}}$ | $5{,}0$ | $\pm 0{,}2$ | $\text{mm}$ |
| Cota de trabalho da roda de medição | $Y_{\text{roda}}$ | $856{,}0$ | $\pm 1{,}0$ | $\text{mm}$ |
| Cota do encoder coaxial | $Y_{\text{encoder}}$ | $766{,}0$ | $\pm 1{,}0$ | $\text{mm}$ |
| Cota do eixo central da mola | $Y_{\text{mola}}$ | $520{,}0$ | $\pm 0{,}5$ | $\text{mm}$ |
| Cota superior da mesa de apoio linear | $Y_{\text{base}}$ | $470{,}0$ | $\pm 1{,}0$ | $\text{mm}$ |
| Curso linear útil da sapata deslizante | $S_{\text{util}}$ | $\pm 35{,}0$ | Máximo $\pm 50{,}0$ | $\text{mm}$ |
| Curso mecânico total dos trilhos | $L_{\text{trilho}}$ | $240{,}0$ | $\pm 1{,}0$ | $\text{mm}$ |
| Largura de contato da banda de PU | $b_{\text{banda}}$ | $20{,}0$ | $\pm 0{,}2$ | $\text{mm}$ |
| Dureza elastomérica da banda | - | $85\text{ a }90$ | $\pm 2$ | Shore A |

### 2.2 Parâmetros Elásticos da Mola de Compressão

A mola horizontal opera na região elástica linear, dimensionada segundo a norma DIN 2089 (Classe 1):

$$F_N = k \cdot (\Delta x_0 + \Delta x_{\text{din}})$$

Onde:
- $k$: Constante elástica da mola helicoidal.
- $\Delta x_0$: Deformação de montagem (pré-carga estática no raio nominal $R_0 = 1500\,\text{mm}$).
- $\Delta x_{\text{din}}$: Variação dinâmica causada pela ovalização e batimento radial da cúpula ($\pm 15\,\text{mm}$).

| Propriedade Mecânica da Mola | Símbolo | Valor Adotado | Unidade |
|:---|:---:|:---:|:---:|
| Material do arame | - | Aço Mola DIN 17223 Classe C / Inox 302 | - |
| Diâmetro do arame | $d_w$ | $4{,}80$ | $\text{mm}$ |
| Diâmetro externo da espira | $D_e$ | $30{,}00$ | $\text{mm}$ |
| Diâmetro médio da espira | $D_m = D_e - d_w$ | $25{,}20$ | $\text{mm}$ |
| Número de espiras ativas | $n_a$ | $7{,}5$ | espiras |
| Número total de espiras (extremidades esquadrejadas e retificadas) | $n_t$ | $9{,}5$ | espiras |
| Comprimento livre (sem carga) | $L_0$ | $130{,}0$ | $\text{mm}$ |
| Comprimento de montagem nominal (pré-carga) | $L_{\text{mont}}$ | $120{,}0$ | $\text{mm}$ |
| Pré-compressão estática nominal ($\Delta x_0$) | $\Delta x_0$ | $10{,}0$ | $\text{mm}$ |
| **Constante elástica calculada** | $k$ | **$12{,}0$** | $\text{N/mm}$ |
| **Força estática de pré-carga** | $F_{N,0} = k \cdot \Delta x_0$ | **$120{,}0$** | $\text{N}$ ($\approx 12{,}2\,\text{kgf}$) |
| Força normal mínima (máxima retração da cúpula, $\Delta x = -15\,\text{mm}$) | $F_{N,\min}$ | $72{,}0$ | $\text{N}$ |
| Força normal máxima (máximo avanço da pista, $\Delta x = +15\,\text{mm}$) | $F_{N,\max}$ | $168{,}0$ | $\text{N}$ |
| Tensão de cisalhamento máxima corrigida ($\tau_{\text{Wahl}}$) | $\tau_{\max}$ | $685{,}0$ | $\text{MPa}$ ($< 0{,}56 \cdot R_m$) |

> [!NOTE]
> A força normal de projeto mantida permanentemente entre $72\,\text{N}$ e $168\,\text{N}$ garante tensão de contato hertziana adequada sobre a banda de PU ($85\text{--}90$ Shore A), assegurando atrito de rolamento estável com coeficiente estático $\mu_e \ge 0{,}65$ sem induzir histerese viscoelástica prejudicial.

### 2.3 Formulação Matemática da Cinemática e Resolução Angular

O sistema opera estritamente no regime de **rolamento puro sem deslizamento**. A velocidade linear instantânea na linha de contato é contínua:

$$v_{\text{contato}} = \omega_{\text{cúpula}} \cdot R_{\text{cúpula}} = \omega_{\text{roda}} \cdot r_{\text{roda}}$$

Integrando em relação ao tempo, a relação fundamental de transmissão geométrica ($\gamma$) é dada por:

$$\gamma = \frac{R_{\text{cúpula}}}{r_{\text{roda}}} = \frac{D_{\text{cúpula}}}{D_{\text{roda}}}$$

Para os dois dimensionamentos de roda contemplados no projeto:
1. **Configuração Padrão ($D_{\text{roda}} = 100\,\text{mm}$):**
   $$\gamma = \frac{3000\,\text{mm}}{100\,\text{mm}} = 30{,}0000$$
2. **Configuração para Grandes Cúpulas ($D_{\text{roda}} = 200\,\text{mm}$):**
   $$\gamma = \frac{3000\,\text{mm}}{200\,\text{mm}} = 15{,}0000$$

#### Contagem de Pulsos em Quadratura ($4\times$)
O encoder gera dois canais defasados em $90^{\circ}$ elétricos (Canal A e Canal B). A interface receptora no CLP ou placa de contagem rápida FPGA detecta todas as bordas de subida e descida de ambos os canais, multiplicando a resolução nativa por quatro:

$$\text{PPR}_{\text{quad}} = 4 \times \text{PPR}_{\text{nativo}}$$

O total de pulsos acumulados por uma rotação de $360^{\circ}$ completos da cúpula ($N_{\text{total}}$) é:

$$N_{\text{total}} = \text{PPR}_{\text{quad}} \times \gamma = 4 \times \text{PPR} \times \left(\frac{D_{\text{cúpula}}}{D_{\text{roda}}}\right)$$

A resolução azimutal incremental mínima discreta ($\delta\theta_{\text{cúpula}}$) resulta em:

$$\delta\theta_{\text{cúpula}} = \frac{360^{\circ}}{N_{\text{total}}} \quad [\text{graus}]$$

$$\delta\theta_{\text{arcsec}} = \delta\theta_{\text{cúpula}} \times 3600 \quad [\text{segundos de arco}]$$

#### Matriz de Resolução Angular Conforme o Sensor

| Diâmetro Roda ($D_{\text{roda}}$) | PPR Nativo | Modo de Leitura | Pulsos/Rev Roda | Relação $\gamma$ | Pulsos/360° Cúpula ($N_{\text{total}}$) | Resolução Azimutal (°/pulso) | Resolução em Arcsegundos |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **$100\,\text{mm}$** | $1.024$ | $4\times$ Quadratura | $4.096$ | $30$ | **$122.880$** | **$0{,}002930^{\circ}$** | **$10{,}55''$** |
| **$100\,\text{mm}$** | $10.000$ | $4\times$ Quadratura | $40.000$ | $30$ | **$1.200.000$** | **$0{,}000300^{\circ}$** | **$1{,}08''$** |
| **$100\,\text{mm}$** | $65.536$ | $4\times$ Quadratura | $262.144$ | $30$ | **$7.864.320$** | **$0{,}0000458^{\circ}$** | **$0{,}165''$** |
| **$200\,\text{mm}$** | $1.024$ | $4\times$ Quadratura | $4.096$ | $15$ | **$61.440$** | **$0{,}005859^{\circ}$** | **$21{,}10''$** |
| **$200\,\text{mm}$** | $10.000$ | $4\times$ Quadratura | $40.000$ | $15$ | **$600.000$** | **$0{,}000600^{\circ}$** | **$2{,}16''$** |
| **$200\,\text{mm}$** | $65.536$ | $4\times$ Quadratura | $262.144$ | $15$ | **$3.932.160$** | **$0{,}0000915^{\circ}$** | **$0{,}330''$** |

> [!IMPORTANT]
> Mesmo na configuração de entrada ($1.024\,\text{PPR}$), o sistema entrega resolução de $\approx 10{,}5$ segundos de arco ($\approx 0{,}175$ arcminutos), superando em ordens de grandeza os requisitos de abertura e alinhamento de fendas de cúpula astronômica (tipicamente toleradas em $\pm 0{,}5^{\circ}$ ou $\pm 30$ arcminutos). Com o encoder SICK DFS60 de $65.536\,\text{PPR}$, a resolução alcança $0{,}165$ arcsegundos, permitindo apontamento espectroscópico de altíssima fidelidade.

---

## 3. Lista de Componentes e Produtos Comerciais (BOM de Mercado)

A tabela abaixo relaciona os componentes de grau industrial especificados para manufatura do protótipo e lote cabeça-de-série:

| Item | Denominação Técnica | Fabricante / Modelo de Referência | Especificações Técnicas Chave | Qtd. |
|:---:|:---|:---|:---|:---:|
| **1.1** | **Encoder Incremental Industrial** *(Opção Padrão)* | **SICK**<br>Modelo: `DFS60B-S4PA10000`<br>ou `DFS60A-S4PK65536` | Resolução programável até $65.536\,\text{PPR}$, eixo sólido $\varnothing 10\,\text{mm}$ com face plana, flange de fixação servo/faceira $\varnothing 50\,\text{mm}$, grau de proteção IP67, temperatura operacional $-40^{\circ}\text{C a }+100^{\circ}\text{C}$, saída elétrica TTL/RS-422 ou HTL (Push-Pull diferencial), conector M12 metálico macho 8 pinos. | 1 un. |
| **1.2** | **Encoder Absoluto Multivoltas** *(Opção Alternativa)* | **KÜBLER**<br>Modelo: `Sendix 5868`<br>(Multiturn Optical) | Resolução singleturn de 16 bits ($65.536$ passos) + multiturn de 12 bits ($4.096$ voltas). Interface de barramento industrial PROFINET, EtherCAT ou SSI. IP67, carcaça de alumínio anodizado marítimo, eixo $\varnothing 10\,\text{mm}$. Elimina ciclo de homing em caso de falta de energia. | 1 un. |
| **2.1** | **Trilho Guia Linear Retificado** | **HIWIN**<br>Série: `HGR20R-240-H` | Par de trilhos perfilados em aço para rolamento temperado por indução e retificado, classe de precisão H (High), com tratamento superficial anticorrosivo de cromo duro (*Raydent*) ou aço inoxidável, comprimento total $240\,\text{mm}$, furação para parafusos M5. | 2 un. |
| **2.2** | **Patins Lineares com Raspadores** | **HIWIN**<br>Série: `HGW20CC-Z0-H` | Quatro patins flangeados com esferas recirculantes em circuito fechado, pré-carga leve Z0 (sem folga radial), equipados com raspadores frontais duplos e retentores de vedação azuis em borracha NBR contra pó e umidade. Capacidade dinâmica $C = 17{,}75\,\text{kN}$. | 4 un. |
| **3.1** | **Roda de Medição de Precisão** | **Fabricação Especializada**<br>ou **KÜBLER / SICK**<br>Ref: `BEF-MR010020R` | Diâmetro externo final $\varnothing 100{,}00 \pm 0{,}02\,\text{mm}$, largura $20\,\text{mm}$. Núcleo usinado em tarugo de Alumínio 6061-T6 com 8 raios aliviados e parafusos perimétricos no aro. Banda de rodagem em Poliuretano (PU) fundido de alto desempenho vulcanizado diretamente sobre o cubo metálico. Dureza $85\text{ a }90$ Shore A, retificada concentricamente em relação ao furo central ($\varnothing 20\,\text{mm}$ H7). | 1 un. |
| **4.1** | **Acoplamento Flexível de Precisão** | **R+W Antriebselemente**<br>Modelo: `BK3 / 15 / 10-20`<br>ou **KTR** `RADEX-NC` | Acoplamento elástico tipo fole metálico (Bellows) em aço inoxidável multicamadas com cubos de fixação por aperto radial (clamp hub). Zero-backlash torsional, furo $\varnothing 10\,\text{mm}$ H7 (eixo do encoder) $\times$ $\varnothing 20\,\text{mm}$ H7 (cubo da roda), torque admissível $15\,\text{N}\cdot\text{m}$, rigidez torsional $2.800\,\text{N}\cdot\text{m/rad}$, absorção de desvios axiais e angulares. | 1 un. |
| **5.1** | **Mola de Compressão Helicoidal** | **Sotreq / Molas Brasil**<br>Ref: `MC-30-130-12` | Aço mola DIN 17223 Classe C (SAE 1070 zincado a quente) ou Aço Inoxidável AISI 302/316. Fio $\varnothing 4{,}8\,\text{mm}$, externo $\varnothing 30{,}0\,\text{mm}$, $L_0 = 130\,\text{mm}$, $k = 12\,\text{N/mm}$, espiras esquadrejadas e retificadas. | 1 un. |
| **6.1** | **Conjunto Mecânico Estrutural** | **Oficina Mecânica CNC**<br>(Projeto Próprio) | - Pilar de sustentação tubular $80 \times 80 \times 4\,\text{mm}$ em Aço ASTM A36.<br>- Flange de piso espessura $16\,\text{mm}$ com 4 furos $\varnothing 14\,\text{mm}$.<br>- Mesa superior de apoio $300 \times 140 \times 14\,\text{mm}$ aplainada.<br>- Coluna curta $\varnothing 40 \times 5\,\text{mm}$ ($h = 240\,\text{mm}$) em Alumínio 6061 usinada.<br>- Sapata móvel $140 \times 120 \times 14\,\text{mm}$ com alojamento para patins.<br>- Copos usinados da mola e mísulas triangulares de rigidez. | 1 cj. |
| **7.1** | **Cabo Blindado de Instrumentação** | **LAPP GROUP**<br>Modelo: `ÖLFLEX ROBUST 210`<br>ou `UNITRONIC BUS PUR` | Cabo flexível 4 pares trançados ($4 \times 2 \times 0{,}25\,\text{mm}^2$), condutores de cobre estanhado, isolação em polipropileno, blindagem em malha trançada de cobre estanhado com cobertura $> 85\%$, capa externa em PUR preto/amarelo de alta resistência mecânica, resistente a óleos, radiação UV e operante a baixas temperaturas ($-40^{\circ}\text{C}$). | 10 m |
| **7.2** | **Conectores e Caixa de Passagem** | **PHOENIX CONTACT**<br>ou **WEIDMÜLLER** | - Conector industrial reto fêmea M12 blindado 8 pólos em metal fundido IP67.<br>- Caixa de bornes de passagem em alumínio injetado IP66 montada na base elevada, equipada com bornes push-in aterrados com conexão direta de blindagem (SHIELD). | 1 cj. |
| **8.1** | **Elementos de Fixação e Chumbamento** | **HILTI**<br>Modelo: `HST3 M12x115/20`<br>+ Parafusos Inox A2-70 | 4 chumbadores mecânicos de expansão de alto desempenho Parabolt M12 em aço carbono zincado para concreto fissurado/não fissurado; parafusos Allen cabeça cilíndrica DIN 912 em Inox A2 (M4, M5, M8) para fixação dos trilhos e sapatas. | 1 cj. |

---

## 4. Orçamento Estimado de Implementação

Os custos foram calculados considerando preços médios praticados no mercado brasileiro (distribuidores autorizados e prestadores de serviços de usinagem) e convertidos à taxa cambial de referência de $\text{US\$ } 1{,}00 = \text{R\$ } 5{,}50$.

### 4.1 Planilha de Custos por Subsistema

```
=========================================================================================================
ITEM   SUBSISTEMA / COMPONENTES                      REF. DE MERCADO           CUSTO (BRL)    CUSTO (USD)
=========================================================================================================
1.0    SENSORES E ELETRÔNICA
1.1    Encoder Incremental SICK DFS60 (10.000 PPR)  SICK do Brasil             R$ 1.650,00     $ 300.00
1.2    Conector M12 Metálico Blindado 8P            Phoenix Contact            R$    95,00     $  17.27
1.3    Cabo Blindado PUR 4 Pares (10 m)              Lapp Group                 R$   140,00     $  25.45
1.4    Caixa de Bornes e Terminais de Aterramento    Weidmüller                 R$   180,00     $  32.73
       --------------------------------------------------------------------------------------------------
       Subtotal Sensores e Eletrônica                                          R$ 2.065,00     $ 375.45

2.0    GUIAS LINEARES E ELEMENTOS DE MÁQUINA
2.1    Par de Trilhos Retificados HIWIN HGR20 (240mm) Kalatec Automação         R$   420,00     $  76.36
2.2    4 Patins com Raspador HIWIN HGW20CC          Kalatec Automação         R$   580,00     $ 105.45
2.3    Acoplamento Fole Metálico Zero-Backlash      KTR / Kalatec              R$   290,00     $  52.73
2.4    Mola de Compressão Helicoidal DIN 17223      Molas Brasil               R$    65,00     $  11.82
       --------------------------------------------------------------------------------------------------
       Subtotal Elementos de Máquina                                           R$ 1.355,00     $ 246.36

3.0    MATÉRIA-PRIMA E USINAGEM CNC
3.1    Tarugos Alumínio 6061-T6 (Roda, Haste, Colar) Distribuidora Metal        R$   280,00     $  50.91
3.2    Chapas e Perfis de Aço ASTM A36 (Pedestal)    Gerdau / Aço Comercial     R$   190,00     $  34.55
3.3    Serviço de Usinagem CNC e Retífica Concentr.  Tornearia / CNC Mec        R$   850,00     $ 154.55
3.4    Vulcanização e Retífica Banda PU 85 Shore A   Poliuretanos Ind.          R$   320,00     $  58.18
3.5    Tratamento Superficial (Anodização + Pintura) Eletrogalvânica            R$   180,00     $  32.73
       --------------------------------------------------------------------------------------------------
       Subtotal Matéria-Prima e Usinagem                                       R$ 1.820,00     $ 330.91

4.0    FIXAÇÕES, CHUMBAMENTO E MISCELÂNEAS
4.1    Kit 4 Chumbadores Parabolt Hilti M12x115      Hilti Brasil               R$    88,00     $  16.00
4.2    Kit Parafusos Allen Inox A2 DIN 912 / Arruelas Ciser Parafusos           R$    65,00     $  11.82
4.3    Abraçadeiras Usinadas e Prensa-Cabos M16      HellermannTyton            R$    42,00     $   7.64
       --------------------------------------------------------------------------------------------------
       Subtotal Fixações e Miscelâneas                                         R$   195,00     $  35.45

5.0    MÃO DE OBRA ESPECIALIZADA (PROTOTIPAGEM)
5.1    Montagem de Bancada e Ajuste de Folgas        Técnico Mecatrônico Sênior R$   600,00     $ 109.09
5.2    Instalação em Campo, Ancoragem e Alinhamento  Engenheiro de Aplicação    R$   900,00     $ 163.64
5.3    Comissionamento Elétrico e Calibração 360°    Engenheiro de Controle     R$   750,00     $ 136.36
       --------------------------------------------------------------------------------------------------
       Subtotal Mão de Obra e Calibração                                       R$ 2.250,00     $ 409.09
=========================================================================================================
TOTAL GERAL ESTIMADO (PROTÓTIPO OPERACIONAL COMPLETO)                          R$ 7.685,00   US$ 1.397,27
=========================================================================================================
```

### 4.2 Projeção para Lote Cabeça-de-Série (3 a 5 Unidades)
Para fabricação seriada voltada a múltiplos observatórios astronômicos associados ao LNA:
- O custo de usinagem e vulcanização de PU apresenta redução de escala de $\approx 35\%$.
- O custo unitário estimado do hardware completo cai para **R$ 4.200,00 a R$ 4.800,00 (US$ 760 a US$ 870)** por unidade (excluindo despesas de deslocamento de campo).

---

## 5. Vantagens da Solução em Pedestal Compacto

A arquitetura de pedestal de base com mola horizontal direta e haste encurtada supera substancialmente os arranjos mecânicos anteriores (braços pantográficos articulados e sistemas suspensos no topo):

```
+----------------------------------------------------+----------------------------------------------------+
| ARQUITETURAS ANTERIORES (BRAÇOS / PANTÓGRAFOS)    | ARQUITETURA SRMIE (PEDESTAL COMPACTO ELEVADO)      |
+----------------------------------------------------+----------------------------------------------------+
| • Articulações com pinos cilíndricos e folgas      | • Guia linear prismática retificada de altíssima   |
|   angulares cumulativas (backlash na reversão).    |   rigidez; zero folga angular ou torcional.        |
| • O braço oscilante gira em arco, variando o       | • Translação pura 1-DOF: a roda tangencia sempre a |
|   ângulo de contato da roda em relação ao trilho.   |   90,00° a pista, independente da excentricidade.  |
| • Colunas longas e esbeltas sujeitas a flexão      | • Haste curta (h = 240 mm) com momento de inércia  |
|   elástica, vibrações de ressonância e "chatter".  |   elevado; elimina deformações e ressonâncias.     |
| • Estrutura volumosa ocupando o topo da cúpula,    | • Topo da cúpula 100% desobstruído; campo visual   |
|   interferindo com fenda, cabos e telescópio.      |   e movimentação de cúpula completamente livres.   |
| • Manutenção perigosa e complexa em altura.        | • Todo o conjunto atuador fica em cota acessível   |
|                                                    |   ao operador em solo firme (Y = 0,47 a 0,52 m).   |
+----------------------------------------------------+----------------------------------------------------+
```

### 5.1 Rigidez Mecânica e Estabilidade Estática
- **Centro de Gravidade Rebaixado:** Toda a massa estrutural pesada (pilar tubular, base de aço de $14\,\text{mm}$, trilhos HIWIN e molas de alta compressão) situa-se próxima ao piso rígido do observatório.
- **Isolamento de Cargas no Eixo do Sensor:** A carcaça do encoder não recebe esforços radiais da mola nem cargas de tombamento. A carga normal da mola ($F_N \approx 120\,\text{N}$) atua diretamente entre a sapata deslizante e o batente de base, enquanto o eixo do encoder é solicitado puramente por torque hidrostático infinitesimal de rolamento do rolamento interno ($< 0{,}02\,\text{N}\cdot\text{m}$).

### 5.2 Topo da Cúpula 100% Desobstruído
- A ausência de vigas suspensas, tirantes e suportes de parede no hemisfério superior da cúpula elimina qualquer risco de interferência física com o movimento do obturador da fenda (*shutter doors*), cabos umbilicais do telescópio ou calhas de iluminação.

### 5.3 Ergonomia e Manutenibilidade
- A inspeção periódica da mola, a lubrificação dos patins lineares via graxeiras laterais e a verificação das conexões elétricas na caixa de junção são realizadas confortavelmente a partir do piso, sem necessidade de andaimes, escadas ou interrupção do funcionamento do telescópio.

---

## 6. Desvantagens, Restrições e Riscos Operacionais

Como todo sistema mecatrônico baseado em contato por atrito elastomérico, existem limites físicos de projeto que exigem mitigação metrológica rigorosa:

### 6.1 Riscos e Limites do Acoplamento por Atrito
1. **Condensação Térmica Severa e Geada:** Em noites de alta umidade relativa ($> 95\%$) combinada com temperaturas sub-zero (frequentes no Pico dos Dias / LNA durante o inverno), pode ocorrer deposição de película de orvalho ou microcristais de gelo na face de aço do trilho.
   - *Efeito:* Redução drástica do coeficiente de atrito estático ($\mu$ cai de $0{,}70$ para $< 0{,}20$), podendo ocasionar microdeslizamentos durante acelerações bruscas da cúpula.
   - *Mitigação:* Instalação de uma escova de feltro/latão de limpeza montada no chassi do pedestal imediatamente à frente da roda para varredura contínua do trilho, e rampa de aceleração da cúpula parametrizada em valor suave ($a_{\max} \le 0{,}05\,\text{m/s}^2$).

2. **Necessidade de Paralelismo Rigoroso dos Trilhos Lineares:**
   - O eixo longitudinal dos trilhos HIWIN deve estar alinhado rigorosamente segundo o vetor radial do centro geométrico do anel da cúpula. Um desvio angular de alinhamento ($\alpha_{\text{err}} > 0{,}5^{\circ}$) induz uma força lateral parasitária contínua sobre a banda de PU, gerando desgaste assimétrico em cunha e aumento do atrito de escorregamento axial.

3. **Desgaste Natural e Variação do Raio Dinâmico ($r_{\text{roda}}$):**
   - O atrito de rolamento contínuo provoca abrasão micrométrica no poliuretano ao longo dos meses de operação contínua.
   - *Impacto Numérico:* Se o diâmetro da roda sofre erosão de apenas $0{,}20\,\text{mm}$ ($\Delta D = -0{,}20\,\text{mm}$, passando de $100{,}00\,\text{mm}$ para $99{,}80\,\text{mm}$), a razão de transmissão real salta de $30{,}0000$ para $30{,}0601$. Ao final de uma volta de $360^{\circ}$, essa variação acumula um erro não calibrado de:
     $$\Delta\theta_{\text{erro}} = 360^{\circ} \times \left(1 - \frac{99{,}80}{100{,}00}\right) = 0{,}72^{\circ} \quad (43{,}2\text{ minutos de arco})$$
   - *Mitigação Obrigatória:* Implementação de rotina de autocalibração periódica em software via chave de indexação absoluta (sensor indutivo de Norte astronômico).

---

## 7. Guia Passo a Passo de Fabricação, Montagem e Comissionamento

### Fase 1: Fabricação e Retífica da Roda de Medição
1. Usinar o núcleo de Alumínio 6061-T6 em torno CNC com precisão IT6, executando os raios aliviados e o rasgo de chaveta/acoplamento central ($\varnothing 20\,\text{mm}$ H7).
2. Tratar a superfície externa do aro com jateamento de microesferas de vidro e aplicação de primer de adesão elastomérica.
3. Realizar a fundição centrífuga da banda de Poliuretano tipo TDI/MOCA com dureza controlada em $88 \pm 2$ Shore A, deixando sobremetal de usinagem de $1{,}5\,\text{mm}$.
4. Montar a roda em mandril de alta precisão entre pontas em retificadora cilíndrica universal; retificar o diâmetro externo da banda para $\varnothing 100{,}00 \pm 0{,}01\,\text{mm}$ garantindo batimento radial (*runout*) inferior a $0{,}015\,\text{mm}$.

```
                 RETÍFICA CONCÊNTRICA DA BANDA DE POLIURETANO
                 
      Ponta Fixa                                           Ponta Giratória
       +------+      +-------------------------------+      +------+
       | Mand |=====>| Núcleo de Alumínio 6061-T6    |<=====| Mand |
       +------+      +---------------+---------------+      +------+
                                     |
                          [Rebolo de Retífica] ===> Tolerância: Ø 100,00 ± 0,01 mm
                                                     Batimento Radial < 0,015 mm
```

### Fase 2: Fixação e Ancoragem Estrutural da Base
1. Marcar a posição radial no piso do observatório, garantindo que o eixo central dos trilhos lineares aponte com precisão teodolítica para o centro de rotação da cúpula.
2. Furar o concreto estrutural com broca de vídia $\varnothing 14\,\text{mm}$ em profundidade mínima de $100\,\text{mm}$. Limpar completamente os furos com ar comprimido e escova helicoidal.
3. Posicionar a chapa de piso do pilar estrutural (`foundPillar`) e inserir os 4 chumbadores Parabolt Hilti HST3 M12. Aplicar torque de aperto de $60\,\text{N}\cdot\text{m}$ com torquímetro calibrado.
4. Ajustar os parafusos de nivelamento da mesa elevada ($Y = 0{,}470\,\text{m}$) utilizando nível de precisão centesimal ($0{,}02\,\text{mm/m}$).
5. Aparafusar a mão-francesa de parede (`wallGusset`) na mureta estática do observatório com chumbadores químicos de ampola epóxi, garantindo ancoragem hiperestática bidirecional.

### Fase 3: Montagem das Guias e Ajuste da Pré-Carga da Mola
1. Limpar as superfícies retificadas da mesa e instalar os dois trilhos HIWIN HGR20. Verificar o paralelismo mútuo com relógio comparador milesimal ao longo de todo o curso de $240\,\text{mm}$ (tolerância de paralelismo $< 0{,}010\,\text{mm}$).
2. Deslizar a sapata móvel com os 4 patins HGW20CC. Verificar deslizamento livre e uniforme por gravidade (sem pontos de travamento).
3. Acoplar a coluna curta usinada ($\varnothing 40\,\text{mm}$) à sapata móvel através dos 4 parafusos de base e travar as mísulas de rigidez.
4. Inserir a mola helicoidal de aço temperado entre o copo do batente fixo traseiro e o copo da sapata móvel.
5. **Ajuste da Pré-Carga:** Com auxílio de um dinamômetro digital de tração/compressão, empurrar o pedestal até a posição nominal de contato com o anel guia ($R_0 = 1500\,\text{mm}$). Ajustar o parafuso de encosto traseiro do batente até registrar a força estática de projeto:
   $$F_{N,\text{nominal}} = 120 \pm 10\,\text{N} \quad (\approx 12{,}2\,\text{kgf})$$
6. Travar a contraporca do ajustador de pré-carga e marcar com lacre de torque azul.

### Fase 4: Roteamento Elétrico, Conexão e Aterramento
1. Montar o encoder coaxial na flange superior da haste curta, apontando para cima.
2. Unir o eixo do encoder ao cubo da roda de medição através do acoplamento flexível de fole metálico R+W, aplicando torque de aperto de $4{,}5\,\text{N}\cdot\text{m}$ nas braçadeiras radiais.
3. Conectar o cabo industrial blindado ao conector metálico M12 do encoder.
4. Fixar o cabo ao longo da coluna curta utilizando as duas presilhas usinadas com inserto de borracha EPDM antivibratória, criando um laço de alívio de tensão (*drip loop*) antes de penetrar na caixa de passagem.
5. **Conexão e Aterramento Anti-Ruído (Eliminação de Ground Loops):**
   - Conectar os pares trançados de sinal diferencial ($A+/A-$, $B+/B-$, $Z+/Z-$) aos bornes rápidos da caixa de junção.
   - **Blindagem do Cabo (Shield):** Conectar a malha trançada em $360^{\circ}$ através do prensa-cabo metálico com mola EMC na caixa de junção.
   - **Regra de Ouro de Aterramento:** Aterrar a blindagem **estritamente em um único ponto** (no barramento de terra do painel do CLP). No lado do encoder, manter a carcaça conectada ao chassi mecânico aterrado, mas a blindagem isolada de retorno de potência para prevenir correntes de circulação parasitas induzidas pela partida dos motores inversores da cúpula.

```
                    ESQUEMA DE ATERRAMENTO EMC (SINGLE-POINT GROUND)
                    
     ENCODER INDUSTRIAL                  CAIXA DE PASSAGEM                PAINEL PRINCIPAL CLP
     [ DFS60 M12 Metálico ]             [ Base Elevada SRMIE ]              [ Barramento PE ]
      +-----------------+                +-----------------+               +-----------------+
      | Sinais A, B, Z  |===============>| Bornes Push-In  |==============>| Entrada Rápida  |
      |                 |                |                 |               | Placa Contadora |
      | Malha Blindagem |===============>| Mola EMC 360°   |==============>| BARRAMENTO DE   |
      +-----------------+                +-----------------+               | TERRA ÚNICO     |
            |                                                              +--------+--------+
      [Chassi Mecânico]                                                             |
            |                                                                       v
     [Piso do Observatório] <--------------------------------------------- [ Malha de Terra ]
```

### Fase 5: Protocolo de Calibração em Campo (Procedimento de 360°)
1. **Verificação de Sentido de Giro:**
   - Girar manualmente a cúpula em sentido horário (visada superior). Verificar no display de telemetria se o contador de pulsos incrementa positivamente ($0 \to 360^{\circ}$). Em caso de contagem decrescente, inverter os sinais $A+$ com $B+$ no conector.
2. **Indexação do Ponto Zero (Homing Absoluto):**
   - Girar a cúpula até que a palheta metálica acione o sensor indutivo fixo de referência (Norte Astronômico). O controlador zera o registrador de contagem rápida (`Reg_Pulso = 0`).
3. **Determinação Experimental do Fator de Escala Real ($K_{\text{calib}}$):**
   - Comandar a rotação contínua da cúpula em velocidade constante de cruzeiro ($v = 1{,}5^{\circ}/\text{s}$) completando exatamente 5 voltas inteiras ($1800^{\circ}$) monitoradas pelo sensor de índice de Norte.
   - Registrar a contagem final acumulada de pulsos no CLP ($P_{\text{medido}}$).
   - Calcular o número médio de pulsos por revolução real ($N_{\text{real}}$):
     $$N_{\text{real}} = \frac{P_{\text{medido}}}{5}$$
   - Atualizar a constante cinemática no software de controle da cúpula:
     $$K_{\text{calib}} = \frac{360{,}000000^{\circ}}{N_{\text{real}}} \quad [^{\circ}/\text{pulso}]$$
4. **Ensaio de Repetibilidade e Histerese:**
   - Realizar 10 ciclos de ida e volta de $90^{\circ}$, $180^{\circ}$ e $270^{\circ}$ parando em alvos ópticos/lasers fixos de calibração.
   - Registrar o desvio azimutal. O sistema é considerado homologado se o erro de repetibilidade for inferior a $\pm 0{,}01^{\circ}$ ($\pm 36$ segundos de arco) em todas as medições.

---

## 8. Conclusão e Parecer de Engenharia

O **Sistema de Roda de Medição Independente com Encoder (SRMIE)** em configuração de **Pedestal Compacto Elevado** representa o estado-da-arte para retrofit metrológico de cúpulas astronômicas. A eliminação completa do braço pantográfico articulado e sua substituição por guias lineares prismáticas de base com mola direta de compressão conferem ao sistema:
1. Imunidade total a erros de patinamento e folgas angulares de reversão;
2. Integridade estrutural contra vibrações de ressonância sob vento e aceleração;
3. Resolução azimutal nativa de $10{,}55''$ (com encoder de $1.024\,\text{PPR}$) até sub-segundo de arco ($0{,}165''$ com encoder de $65.536\,\text{PPR}$);
4. Facilidade de operação, comissionamento e manutenção a custo extremamente competitivo frente a sistemas ópticos perimétricos comerciais.

O projeto cumpre integralmente os requisitos operacionais estabelecidos pelo Laboratório Nacional de Astrofísica (LNA), estando apto para manufatura mecânica, instrumentação eletrônica e comissionamento em campo.
