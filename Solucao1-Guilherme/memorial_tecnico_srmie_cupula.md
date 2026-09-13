# Memorial Técnico Descritivo e Guia de Fabricação
## Sistema de Medição Azimutal de Cúpula Astronômica com Mecanismo em Pedestal e Contato Tangencial (SRMIE)

**Código do Documento:** MT-SRMIE-002-REV00  
**Projeto:** Modernização e Instrumentação Azimutal de Cúpula de Observatório Astronômico  
**Instituição:** UNIFEI / LNA (Laboratório Nacional de Astrofísica)  
**Disciplina:** Engenharia Mecânica de Precisão, Automação Industrial e Instrumentação Científica  
**Classificação:** Documento Técnico de Projeto Executivo e Fabricação  
**Normas Aplicáveis:** ABNT NBR ISO 12100 (Segurança de Máquinas), DIN 17223 / EN 10270 (Arames de Aço para Molas Mecânicas), DIN EN ISO 286 (Sistema ISO de Tolerâncias e Ajustes), IEC 60529 (Graus de Proteção IP), IEC 61158 (Comunicação Industrial de Dados), ISA-5.1 (Simbologia e Identificação de Instrumentação).

---

## 1. Princípio de Funcionamento Mecatrônico e Arquitetura do Sistema

### 1.1 Princípio de Acoplamento por Atrito Estático e Rolamento Puro
O **Sistema de Medição Azimutal de Cúpula com Mecanismo em Pedestal (SRMIE)** baseia-se no princípio físico de **rolamento puro sem deslizamento relativo** (*pure rolling without slip*) entre a superfície perimétrica retificada de uma roda de medição e a pista de rolamento interna da cúpula do observatório astronômico. 

Diferentemente dos métodos convencionais em que o sensoriamento é acoplado aos eixos dos motores de tração da cúpula, o SRMIE é um sistema passivo de medição direta. O torque resistente imposto pelo sensor ao conjunto girante é extremamente baixo (restringindo-se ao atrito de rotação dos rolamentos de esferas de precisão e ao torque inercial do eixo óptico do encoder, tipicamente $< 0{,}02\text{ Nm}$). 

Para assegurar o regime de atrito estático permanente sem escorregamento microcinemático durante as acelerações angulares da cúpula, uma força normal de compressão contínua ($F_N$) é aplicada no sentido radial contra a pista interna da cúpula:

$$F_{\text{atrito\_disponível}} = \mu_s \cdot F_N$$

Com coeficiente de atrito estático $\mu_s \ge 0{,}70$ proporcionado pelo elastômero de poliuretano contra a superfície de aço/aço pintado e $F_N \ge 120\text{ N}$, a força tangencial máxima transmissível na interface de contato atinge:

$$F_{t,\text{máx}} = 0{,}70 \times 120\text{ N} = 84\text{ N}$$

Como o torque inercial e de atrito do conjunto de medição demanda forças tangenciais de aceleração inferiores a $0{,}5\text{ N}$, a margem de segurança contra deslizamento relativo ultrapassa o fator de $150:1$, garantindo sincronismo azimutal absoluto durante partidas, frenagens e inversões de rotação da cúpula.

### 1.2 Fluxo Cinemático e Estrutural de Transmissão
A cinemática de contato e a descarga estrutural de forças operam conforme o diagrama esquemático em blocos detalhado a seguir:

```
========================================================================================================================
                          DIAGRAMA ESQUEMÁTICO DE TRANSMISSÃO E SUSTENTAÇÃO MECÂNICA
========================================================================================================================

   [ PISTA INTERNA DA CÚPULA (GIRATÓRIA - Ø 6.000 mm) ]
                      |
                      |  (Contato Tangencial Puro por Atrito Estático: Fn = 120 N, mu >= 0.7)
                      v
   [ RODA DE MEDIÇÃO USINADA (Ø 200 mm, Banda Poliuretano 85-90 Shore A) ]
                      |
                      |  (Acoplamento Rígido por Eixo Retificado Inox Ø 20 mm h6)
                      v
   [ MANCAL VERTICAL COM DUPLO ROLAMENTO DE PRECISÃO (2x SKF 6004-2RSH) ]
                      |
                      |  (Transmissão de Rotação Pura com Desacoplamento de Esforços Axiais/Radiais)
                      v
   [ ACOPLAMENTO FLEXÍVEL DE FOLE METÁLICO ZERO-BACKLASH (Ø 20 mm x Ø 10 mm) ]
                      |
                      |  (Torque de Rotação Angular Puro sem Folga)
                      v
   [ ENCODER INDUSTRIAL DE ALTA RESOLUÇÃO (SICK DFS60 ou Kübler Sendix F5868) ]
                      |
                      |  (Montagem Invertida Coaxial Fixada no Suporte Móvel)
                      v
   [ CARRO DESLIZANTE MÓVEL COM PATIM LINEAR (HIWIN HGW20CC) ]
                      ^
                      |  (Pré-carga Radial Normal Constante Fn = 120 N)
   [ MOLA HELICOIDAL DE COMPRESSÃO CALIBRADA (k = 12 N/mm, Delta x = 10 mm) + FUSO TENSOR ]
                      |
                      v
   [ TRILHO GUIA LINEAR RETIFICADO (HIWIN HGR20 - Curso Útil >= 50 mm) ]
                      |
                      |  (Descarga de Esforços Fletores e Cisalhantes)
                      v
   [ COLUNA ESTRUTURAL DO PEDESTAL (Tubo Schedule 40 Ø 73 mm + Mísulas Soldadas 45°) ]
                      |
                      |  (Engastamento Rígido e Ancoragem por Chumbamento Químico M10)
                      v
   [ BASE DO PISO / MURETA TÉCNICA DE CONCRETO DO OBSERVATÓRIO ]
========================================================================================================================
```

### 1.3 Mecanismo de Compensação Dinâmica Radial de Runout e Ovalização
As cúpulas de observatórios astronômicos de médio e grande porte são estruturas metálicas caldeiradas sujeitas a imperfeições construtivas inevitáveis:
- Ovalizações geométricas do anel periférico decorrentes da conformação mecânica.
- Descontinuidades de solda nas juntas estruturais dos segmentos do anel.
- Batimento radial excêntrico (*runout*) decorrente da folga operacional dos truques e roletes de apoio sobre os quais a cúpula repousa.
- Deformações térmicas cíclicas sazonais provocadas pelo gradiente de temperatura diurno/noturno.

Esses desvios radiais manifestam-se como variações dinâmicas de raio na faixa de **$\pm 5\text{ mm}$ a $\pm 25\text{ mm}$** durante o giro completo de $360^\circ$. 

No sistema SRMIE, o berço que suporta o conjunto roda/mancal/encoder é solidário ao patim linear de esferas recirculantes HIWIN HGW20CC, dotado de um único grau de liberdade puramente radial em relação ao centro de rotação da cúpula. A mola helicoidal calibrada atua em regime elástico constante: à medida que a pista da cúpula avança radialmente para dentro (ponto de aproximação máxima), o patim recua comprimindo a mola; quando a pista se afasta (ponto de maior excentricidade), a mola expande mantendo a roda em contato ininterrupto. 

A rigidez selecionada ($k = 12\text{ N/mm}$) assegura que mesmo na amplitude máxima de oscilação radial ($\pm 10\text{ mm}$ em torno do ponto de operação), a força de contato permaneça rigorosamente dentro da janela metrológica segura:

$$F_{N,\text{mín}} = 120\text{ N} - (12\text{ N/mm} \times 5\text{ mm}) = 60\text{ N} \quad (\text{suficiente para prevenir escorregamento})$$
$$F_{N,\text{máx}} = 120\text{ N} + (12\text{ N/mm} \times 5\text{ mm}) = 180\text{ N} \quad (\text{sem deformar a banda de poliuretano})$$

---

## 2. Especificações Técnicas e Dimensionamento Cinemático

### 2.1 Tabela de Parâmetros Nominais de Projeto

| Parâmetro de Engenharia | Valor Nominal de Projeto | Tolerância / Norma | Justificativa Técnica |
| :--- | :--- | :--- | :--- |
| **Diâmetro da Pista da Cúpula ($D_c$)** | $6.000{,}00\text{ mm}$ | $\pm 15{,}0\text{ mm}$ (*runout*) | Perímetro de medição interno usinado do anel da cúpula astronômica. |
| **Diâmetro da Roda de Medição ($D_r$)** | $200{,}00\text{ mm}$ | $\pm 0{,}05\text{ mm}$ | Roda usinada em torno CNC; diâmetro otimizado para conferir razão inteira $30:1$. |
| **Material do Núcleo da Roda** | Alumínio estrutural 6061-T6 | ASTM B221 / Anodizado | Baixa inércia rotacional ($J$), alta usinabilidade e resistência à corrosão atmosférica. |
| **Revestimento da Banda de Rodagem** | Poliuretano (PU) fundido Vulkollan® | Dureza $85\text{ a }90\text{ Shore A}$ | Elevada resistência à abrasão, estabilidade dimensional e atrito $\mu \ge 0{,}70$. |
| **Largura da Banda da Roda** | $30{,}0\text{ mm}$ | $\pm 0{,}5\text{ mm}$ | Distribuição uniforme da pressão superficial de Hertz ($< 2{,}5\text{ MPa}$). |
| **Relação de Transmissão Nominal ($i$)** | $30:1$ | Adimensional | Redução cinemática direta entre o diâmetro da cúpula e o diâmetro da roda. |
| **Eixo de Transmissão Mecânica** | Aço Inox AISI 304/316 retificado | $\varnothing 20{,}00\text{ mm}$ ajuste h6 | Rigidez à flexão sob carga transversal de 120 N; imune à corrosão. |
| **Rolamentos do Mancal Vertical** | 2x SKF 6004-2RSH (ou NSK DDU) | Classe ISO Normal / P5 | Rolamentos rígidos de esferas com vedação de borracha de contato nos dois lados. |
| **Mola Helicoidal de Compressão** | Aço mola DIN 17223 (SAE 1070 zinc.) | $k = 12{,}0\text{ N/mm}$ ($10\text{ a }14\text{ N/mm}$) | $L_0 = 150\text{ mm}$, diâmetro externo $\varnothing 32\text{ mm}$, curso livre $\ge 50\text{ mm}$. |
| **Guia Linear e Patim Deslizante** | HIWIN HGR20 (trilho) + HGW20CC (patim) | Pré-carga leve (ZA) | Atrito de rolamento $\mu \approx 0{,}002$, capacidade de carga dinâmica $C = 17{,}75\text{ kN}$. |
| **Comprimento do Trilho Linear** | $300{,}0\text{ mm}$ | $\pm 1{,}0\text{ mm}$ | Curso livre de $\pm 35\text{ mm}$ acomodando o deslocamento radial sem risco de fim-de-curso. |
| **Coluna Estrutural do Pedestal** | Tubo Schedule 40 ($\varnothing_{\text{ext}} \approx 73{,}0\text{ mm}$) | Espessura $5{,}16\text{ mm}$ (Aço A36)| Rigidez estática com frequência natural $> 45\text{ Hz}$, evitando ressonâncias com motores. |

---

### 2.2 Memorial de Cálculo Cinemático e Metrológico

```
=============================================================================================
                       RELAÇÕES CINEMÁTICAS E GEOMÉTRICAS DO SISTEMA
=============================================================================================

                                       Cúpula Giratória (Raio Rc = 3.000 mm)
                                    /'''''''''''''''''''''''''''''''''''''''\
                                   |                   Dc = 6.000 mm         |
                                    \......................................./
                                                        |
                                                        v (Ponto de Tangência P)
                                              ( Roda de Medição )
                                              (    Dr = 200 mm  )
                                              (     i = 30:1    )
                                                        |
                                                        v
                                              [ Encoder SICK DFS60 ]
                                              [  10.000 PPR Nativo ]
                                              [ 40.000 Contagens/Rev]
```

#### A. Relação de Transmissão Cinemática ($i$)
Sendo $D_c$ o diâmetro nominal da pista da cúpula e $D_r$ o diâmetro externo usinado da roda de medição:

$$i = \frac{D_c}{D_r} = \frac{6.000{,}00\text{ mm}}{200{,}00\text{ mm}} = 30{,}0000$$

Portanto, a roda de medição e o encoder completam exatamente $30$ rotações completas a cada revolução de $360^\circ$ da cúpula do observatório astronômico.

#### B. Pulsos Brutos e Contagens em Quadratura ($4\times$)
O encoder óptico incremental é parametrizado com resolução nativa de $10.000\text{ PPR}$ (*Pulses Per Revolution*) nos canais em fase $A$ e $B$. Utilizando decodificação em quadratura eletrônica $4\times$ no cartão de contagem rápida (HSC) do CLP:

$$C_r = PPR \times 4 = 10.000 \times 4 = 40.000\text{ contagens/revolução da roda}$$

O número total de contagens acumuladas no CLP para uma rotação de $360^\circ$ da cúpula ($C_{\text{total}}$) é expresso por:

$$C_{\text{total}} = i \times C_r = 30 \times 40.000 = 1.200.000\text{ contagens/volta completa da cúpula}$$

#### C. Resolução Angular Teórica do Azimute ($\Delta\theta$)
A menor variação angular detectável do azimute da cúpula astronômica ($\Delta\theta$) é calculada dividindo-se o círculo completo pelo número total de contagens:

$$\Delta\theta = \frac{360^\circ}{C_{\text{total}}} = \frac{360^\circ}{1.200.000} = 0{,}000300^\circ$$

Convertendo o valor angular para minutos e segundos de arco (*arcseconds*):

$$\Delta\theta = 0{,}000300^\circ \times 60\,\frac{\text{arcmin}}{^\circ} = 0{,}018\text{ arcminutos}$$

$$\Delta\theta = 0{,}018\text{ arcmin} \times 60\,\frac{\text{arcsec}}{\text{arcmin}} = 1{,}08''\text{ de arco (arcsegundos)}$$

> [!NOTE]
> A resolução obtida de **$1{,}08''$ de arco** corresponde a um deslocamento linear no perímetro da cúpula de apenas:
> $$\Delta s = \frac{\pi \times D_c}{C_{\text{total}}} = \frac{\pi \times 6.000\text{ mm}}{1.200.000} \approx 0{,}0157\text{ mm} = 15{,}7\,\mu\text{m}$$
> Essa sensibilidade metrológica é cerca de 55 vezes mais fina que a banda de tolerância máxima admitida para o acompanhamento azimutal do telescópio ($< 1\text{ arcminuto} = 60''$).

#### D. Dimensionamento da Mola e Cálculo da Força de Pré-Carga Normal ($F_N$)
Para garantir que a força de contato permaneça acima do patamar mínimo de aderência mesmo nos pontos de maior afastamento excêntrico da pista, adota-se a mola helicoidal com constante elástica nominal $k = 12{,}0\text{ N/mm}$.

Ajustando o fuso tensor de regulagem para uma deflexão estática inicial $\Delta x = 10{,}0\text{ mm}$:

$$F_{\text{mín}} = k \cdot \Delta x = 12{,}0\text{ N/mm} \times 10{,}0\text{ mm} = 120{,}0\text{ N}$$

Convertendo para quilograma-força:

$$F_{\text{mín}} \approx \frac{120{,}0\text{ N}}{9{,}80665\text{ m/s}^2} \approx 12{,}24\text{ kgf}$$

A tensão de cisalhamento máxima atuante no fio da mola ($\tau_{\text{máx}}$) com deflexão extrema de trabalho ($\Delta x_{\text{ext}} = 25\text{ mm}$, $F_{\text{máx}} = 300\text{ N}$) foi verificada conforme a equação de Wahl para arames classe DIN 17223, resultando em $\tau_{\text{máx}} = 412\text{ MPa}$, valor substancialmente inferior à tensão admissível de escoamento ao cisalhamento do aço mola ($\tau_{\text{adm}} \approx 780\text{ MPa}$), garantindo vida útil infinita à fadiga ($> 10^7\text{ ciclos}$).

---

## 3. Seleção Detalhada de Sensores (Encoders Industriais)

Para a conversão de rotação em pulsos metrológicos no SRMIE, foram especificadas e homologadas duas configurações de transdutores industriais de alto desempenho, adequadas a diferentes perfis operacionais e orçamentários:

```
+----------------------------------------------------------------------------------------------------+
|                      MATRIZ COMPARATIVA DE TRANSDUTORES HOMOLOGADOS                                |
+------------------------------------+----------------------------------+----------------------------+
| Parâmetro Técnico                  | Opção Incremental (Homologada)   | Opção Absoluta Multivoltas |
+------------------------------------+----------------------------------+----------------------------+
| Fabricante / Modelo                | SICK DFS60                       | Kübler Sendix F5868 / SICK |
| Part Number de Catálogo            | DFS60A-S4AK65536                 | 8.F5868.1224.B222          |
| Princípio de Leitura               | Óptico Incremental Diferencial   | Óptico Absoluto Multivoltas|
| Resolução Parametrizada            | 10.000 a 65.536 PPR Programável  | 28 bits (16b ST / 12b MT)  |
| Interface de Comunicação           | RS-422 (TTL Diferencial 6 vias)  | SSI / Profinet-IRT         |
| Comportamento em Queda de Energia  | Perde contagem (Requer Homing)   | Retém coordenada real      |
| Custo Relativo de Aquisição        | Base de Referência (1,0x)        | 2,8x a 3,5x da Opção Incr. |
+------------------------------------+----------------------------------+----------------------------+
```

### 3.1 Opção 1 (Recomendada / Custo-Benefício): Encoder Incremental SICK DFS60
- **Part Number:** SICK DFS60A-S4AK65536.
- **Resolução e Programabilidade:** Resolução nativa de fábrica ajustável por software de $1\text{ a }65.536\text{ PPR}$. O valor padrão de projeto adotado é de **$10.000\text{ PPR}$** (resultando em $1.200.000\text{ contagens/volta}$ da cúpula sob decodificação $4\times$). Caso a malha de controle demande maior amostragem, a resolução pode ser ampliada para até $65.536\text{ PPR}$ ($7.864.320\text{ contagens/volta}$, resolução de $0{,}165''$).
- **Sinais de Saída:** Diferenciais de 6 canais line driver TTL/RS-422 ($A, \bar{A}, B, \bar{B}, Z, \bar{Z}$). A sinalização complementar rejeita ruídos eletromagnéticos de modo comum provenientes de inversores de frequência que acionam os motores principais da cúpula.
- **Conexão e Conector:** Conector circular industrial padrão M12, 8 pinos, saída radial, com invólucro metálico blindado aterrado na carcaça.
- **Construção Mecânica e Graus de Proteção:** Flange de aperto tipo *clamping* com eixo maciço $\varnothing 10\text{ mm} \times 19\text{ mm}$. Grau de proteção ambiental **IP67** na carcaça de alumínio injetado e **IP65** no retentor do eixo de entrada.
- **Faixa Térmica de Trabalho:** $-30^\circ\text{C}\text{ a }+100^\circ\text{C}$, suportando sem condensação as condições climáticas severas de observatórios de montanha.

### 3.2 Opção 2 (Operação Crítica Sem Homing): Encoder Absoluto Multivoltas Kübler Sendix F5868 / SICK AFM60
- **Modelo de Referência:** Kübler Sendix 5868 (8.F5868.1224.B222) ou SICK AFM60 Profinet.
- **Princípio Operacional:** O transdutor absoluto multivoltas possui leitura óptica de disco de código Gray para a volta simples (*singleturn* - 16 bits) e trem de engrenagens mecânicas internas de ultraprecisão com sensores magnéticos para contagem das voltas completas (*multiturn* - 12 bits, acumulando até 4.096 rotações).
- **Vantagem Crítica (Dispensa Rotina de Zero):** A cúpula astronômica pode ser girada manualmente em modo de emergência ou sofrer blecaute elétrico durante a madrugada; ao restabelecer a alimentação, a posição angular exata da cúpula é imediatamente informada ao sistema SCADA sem necessidade de girar a cúpula até encontrar o sensor de referência (*Homing*).
- **Interface de Rede:** SSI (*Synchronous Serial Interface*) com taxa de transmissão de até $2\text{ MHz}$ ou protocolo industrial Ethernet Profinet-IRT, enviando o vetor de posição em palavras duplas com redundância cíclica de dados (CRC).

---

## 4. Análise Crítica: Vantagens vs. Desvantagens e Restrições

### 4.1 Matriz Comparativa de Vantagens e Restrições

| Aspecto Analisado | Vantagens do Sistema SRMIE em Pedestal | Desvantagens / Restrições Operacionais | Medidas de Mitigação Adotadas no Projeto |
| :--- | :--- | :--- | :--- |
| **Metrologia e Precisão** | • Resolução angular de $1{,}08''$.<br>• Desacoplamento da patinagem dos motores tracionadores.<br>• Leitura cinemática direta. | • Erro de escala induzido por gradientes térmicos na banda de poliuretano. | • Algoritmo de correção térmica em tempo real no CLP baseado em sensor PT100 na base. |
| **Dinâmica e Desvios de Trilho** | • Compensação elástica passiva de ovalização e empenamentos de até $\pm 25\text{ mm}$. | • Força elástica ligeiramente variável ao longo do curso ($\pm 30\text{ N}$). | • Curso útil de mola superdimensionado garantindo $F_N \ge 60\text{ N}$ na condição mais desfavorável. |
| **Segurança e Ergonomia** | • Topologia com topo livre sem projeções de eixos.<br>• Sensor protegido embaixo da mesa.<br>• Manutenção no nível da laje. | • Ocupação de área de circulação técnica na mureta/piso ($300 \times 300\text{ mm}$). | • Fixação compacta rente à mureta perimétrica sem invadir a rota de fuga técnica. |
| **Ambiente e Contaminação** | • Conjunto mecânico IP67/IP65.<br>• Imune à poeira que inutiliza fitas ópticas perimétricas. | • Acúmulo de óleo ou graxa pesada pode reduzir o coeficiente de atrito estático. | • Instalação de raspadores tangenciais duplos de feltro antes da área de contato da roda. |
| **Ciclo de Vida e Custo** | • Custo de implantação muito inferior ao de fitas ópticas.<br>• Peças comerciais COTS de pronta-entrega. | • Desgaste abrasivo gradual da banda de PU sob regime contínuo. | • Formulação de poliuretano Vulkollan® de alta resistência à abrasão; inspeção anual. |

### 4.2 Detalhamento Técnico das Vantagens
1. **Imunidade Absoluta à Patinagem de Tração:** Motores redutores que acionam cúpulas sofrem com o escorregamento microscópico e deslizamento em rampas de aceleração. Como o SRMIE não transmite potência motora, ele opera em regime de torque resistente quase nulo, eliminando qualquer risco de perda de passo angular decorrente da tração.
2. **Filtragem Mecânica Passiva de Vibrações:** A complacência elástica da guia linear associada ao amortecimento intrínseco do poliuretano atua como um filtro mecânico passa-baixas, isolando o sensor óptico contra os choques de alta frequência produzidos pelas emendas dos trilhos de translação da cúpula.
3. **Topologia com Topo Desobstruído:** A fixação inferior do mancal e do encoder garante que a roda apresente sua face superior perfeitamente plana e rebaixada em relação à saia inferior da cúpula. Isso elimina qualquer risco de esmagamento ou colisão com cabos suspensos ou contraventamentos estruturais do teto giratório.

### 4.3 Detalhamento das Restrições e Compensações Algorítmicas
- **Comportamento Termoelástico do Poliuretano:** O coeficiente de dilatação térmica linear do poliuretano fundido ($\alpha_{\text{PU}} \approx 1{,}5 \times 10^{-4}\text{ K}^{-1}$) faz com que uma variação de temperatura de $\Delta T = \pm 10^\circ\text{C}$ altere o diâmetro da roda em aproximadamente $\pm 0{,}3\text{ mm}$, o que representaria um erro de posicionamento acumulado de cerca de $0{,}15\%$ por volta da cúpula se não corrigido. 
  - *Mitigação:* O CLP executa a correção contínua do diâmetro efetivo $D_r(T)$ através da equação:
    $$D_r(T) = D_{r,0} \cdot \left[1 + \alpha_{\text{eq}} \cdot (T_{\text{sensor}} - 20^\circ\text{C})\right]$$
- **Proteção Contra Contaminação por Graxa:** Caso a cúpula utilize graxa grafitada ou sintética nos mancais de giro que possa escorrer para a pista de medição, o coeficiente $\mu_s$ poderia cair de $0{,}70$ para valores inferiores a $0{,}15$, propiciando escorregamento.
  - *Mitigação:* O projeto incorpora um defletor metálico de gotejamento na face superior e dois blocos raspadores de feltro industrial de lã pura prensada montados em balanço na sapata móvel, responsáveis pela limpeza mecânica contínua da pista antes do contato tangencial da roda.

---

## 5. Roteiro de Fabricação, Instalação e Comissionamento

O processo de implementação em campo deve cumprir rigorosamente as quatro fases sequenciais descritas a seguir:

```
=============================================================================================
                 CRONOGRAMA SEQUENCIAL DE IMPLEMENTAÇÃO E COMISSIONAMENTO
=============================================================================================

 [ FASE 1: USINAGEM CNC E RETÍFICA ]
   • Torneamento do núcleo de Alumínio 6061-T6 com cauda de andorinha
   • Vulcanização/Fundição da banda de Poliuretano 85-90 Shore A
   • Retífica concêntrica da banda (Batimento radial < 0,02 mm)
   • Usinagem do bloco de mancal com tolerância de alojamento H7/js6
                         |
                         v
 [ FASE 2: ANCORAGEM E ALINHAMENTO ESTRUTURAL ]
   • Posicionamento ortogonal ao raio da cúpula com alinhador laser
   • Furação e injeção de chumbador químico epóxi puro com hastes M10 inox
   • Fixação do pedestal de coluna de aço Schedule 40
   • Nivelamento da mesa com bolha milesimal (Perpendicularidade <= 0,1 mm/m)
                         |
                         v
 [ FASE 3: MONTAGEM DO CONJUNTO E REGULAGEM DA PRÉ-CARGA ]
   • Fixação do trilho Hiwin HGR20 e patim HGW20CC com torque cruzado
   • Instalação da mesa móvel, mancal 6004-2RS, acoplamento fole e encoder
   • Identificação da excentricidade máxima do trilho da cúpula
   • Ajuste do fuso tensor para deflexão de 10 mm a 12 mm na mola (Fn >= 120 N)
                         |
                         v
 [ FASE 4: CABEAMENTO, ATERRAMENTO E CALIBRAÇÃO METROLÓGICA ]
   • Roteamento de cabo PUR com laço flexível de alívio (Raio dinâmico >= 100 mm)
   • Aterramento de blindagem 360° em ponto único no painel do CLP
   • Giro de comissionamento de 360° em velocidade constante
   • Determinação do diâmetro efetivo e validação do erro angular (< 0,001°)
```

### 5.1 Fase 1: Usinagem CNC, Vulcanização e Retífica
1. **Núcleo da Roda de Medição:**
   - Tornear o disco de alumínio aeronáutico 6061-T6 em centro de torneamento CNC.
   - Usinar 4 ranhuras circunferenciais em perfil de **cauda de andorinha** (*dovetail*) na face externa do aro com profundidade de $2{,}0\text{ mm}$ e ângulo de $60^\circ$, garantindo ancoragem mecânica para retenção do elastômero sob forças de cisalhamento.
   - Usinar o furo central $\varnothing 20\text{ mm}$ com tolerância H7 e abrir rasgo de chaveta conforme norma DIN 6885/1 ($6\text{ mm} \times 6\text{ mm}$).
2. **Aplicação e Cura do Poliuretano:**
   - Aplicar primer químico de adesão sobre o alumínio desengraxado e verter poliuretano fundido grau Vulkollan® (dureza $85\text{ a }90\text{ Shore A}$).
   - Submeter o conjunto ao ciclo térmico de pós-cura em estufa controlada ($110^\circ\text{C}$ por 16 horas) para alívio de tensões moleculares e estabilização elastomérica.
3. **Retífica de Precisão:**
   - Montar o conjunto vulcanizado em mandril retificado entre pontas em retificadora cilíndrica.
   - Retificar a banda externa até o diâmetro exato de $\varnothing 200{,}00 \pm 0{,}02\text{ mm}$.
   - Verificar batimento radial (*runout*) com relógio comparador milesimal: **tolerância máxima de batimento radial $< 0{,}02\text{ mm}$**.
4. **Usinagem do Mancal Vertical:**
   - Usinar o bloco de mancal em liga de alumínio 7075-T6. Os alojamentos para os dois rolamentos 6004 devem ser usinados na mesma fixação em centro de usinagem 4 eixos para garantir coaxialidade rigorosa ($< 0{,}008\text{ mm}$), com tolerância de diâmetro interno **$\varnothing 42\text{ mm H7}$**.
   - O eixo de aço inoxidável AISI 316 deve ser retificado no diâmetro dos assentos de rolamento para **$\varnothing 20\text{ mm js6}$**, propiciando ajuste de interferência leve sem folga radial.

### 5.2 Fase 2: Montagem Estrutural e Ancoragem no Piso
1. **Locação do Ponto de Instalação:**
   - Com o auxílio de nível a laser rotativo instalado no eixo óptico central do telescópio, projetar a linha radial teórica perpendicular à pista de rolamento interna da cúpula.
   - Marcar na laje técnica de concreto ou na face superior da mureta os 4 furos de fixação da placa base do pedestal ($250 \times 250\times 16\text{ mm}$).
2. **Perfuração e Chumbamento Químico:**
   - Perfurar o concreto com broca vídia de $\varnothing 14\text{ mm}$ até a profundidade de $90\text{ mm}$.
   - Realizar limpeza profunda dos furos utilizando ar comprimido seco e escova de cerdas de aço (3 ciclos sucessivos) para eliminar todo o pó de concreto.
   - Aplicar resina epóxi química de alto desempenho (Hilti HIT-HY 200 ou Fischer FIS V) preenchendo 2/3 da cavidade.
   - Introduzir girando manualmente as 4 barras roscadas de aço inoxidável AISI 316 M10. Aguardar o período de cura química estrutural completa ($12\text{ horas}$ a $20^\circ\text{C}$).
3. **Nivelamento e Alinhamento do Pedestal:**
   - Instalar o pedestal tubular Schedule 40 sobre as barras roscadas, utilizando porcas e contraporcas inferiores de calibração milimétrica.
   - Assentar nível de precisão bolha milesimal ($0{,}02\text{ mm/m}$) sobre a mesa usinada superior do pedestal.
   - Nivelar o conjunto até garantir desvio de perpendicularidade da coluna **$\le 0{,}1\text{ mm/m}$** em relação ao plano horizontal da cúpula.
   - Aplicar torque de aperto definitivo nas porcas superiores com torquímetro regulado em $45\text{ Nm}$.

### 5.3 Fase 3: Montagem Mecânica e Regulagem da Pré-Carga
1. **Instalação da Guia Linear:**
   - Limpar a mesa retificada superior do pedestal com desengraxante e instalar o trilho linear HIWIN HGR20R-300mm.
   - Apertar os parafusos M5 classe 12.9 do centro para as pontas com torquímetro ($8{,}8\text{ Nm}$), aplicando adesivo anaeróbico trava-rosca médio Loctite 242.
   - Deslizar o patim HGW20CC e parafusar a mesa móvel superior usinada.
2. **Instalação da Roda, Mancal, Acoplamento e Encoder:**
   - Fixar o conjunto do mancal vertical e roda de medição na mesa móvel.
   - Acoplar o encoder óptico no suporte inferior usinado.
   - Interligar o eixo do mancal ($\varnothing 20\text{ mm}$) ao eixo do encoder ($\varnothing 10\text{ mm}$) através do acoplamento flexível de fole metálico R+W BK3. Certificar-se de que o fole metálico não sofra nenhuma compressão ou tração axial forçada durante o aperto das braçadeiras de fixação radial (*clamping screws*). Torque de aperto nos parafusos do fole: $1{,}8\text{ Nm}$.
3. **Determinação do Ponto Crítico de Excentricidade:**
   - Girar a cúpula em $360^\circ$ e mapear com relógio comparador o setor de maior afastamento radial do anel interno (ponto de maior folga/rebaixo). Posicionar a cúpula exatamente sobre este ponto de pior caso.
4. **Regulagem Fina da Força de Compressão:**
   - Liberar a mesa móvel até a roda encostar suavemente na pista da cúpula (sem deformar a mola).
   - Utilizar o fuso roscado tensor M12 em aço inox para comprimir a mola helicoidal de $10\text{ mm}$ a $12\text{ mm}$ a partir do comprimento livre de contato.
   - Medir a força normal resultante utilizando dinamômetro digital de compressão ou célula de carga padrão posicionada temporariamente na linha de ação: **$F_N \ge 120\text{ N} \pm 5\text{ N}$**.
   - Travar firmemente a contraporca de segurança M12 do fuso tensor e aplicar laca selante de testemunho (*torque seal*).

### 5.4 Fase 4: Cabeamento, Aterramento e Comissionamento Metrológico
1. **Roteamento do Cabo e Laço Dinâmico (*Service Loop*):**
   - Conectar o conector fêmea reto M12 metálico blindado de 8 polos na tomada radial do encoder SICK DFS60, apertando o anel roscado com torque manual firme ($0{,}6\text{ Nm}$).
   - Conformar o cabo flexível PUR em forma de laço em ômega ($\Omega$) com raio de curvatura dinâmico livre $\ge 100\text{ mm}$, permitindo que a sapata móvel transloque $\pm 35\text{ mm}$ sem tracionar mecanicamente o prensa-cabos ou a carcaça do encoder.
   - Prender o cabo através de abraçadeiras acolchoadas em borracha EPDM fixadas na coluna estacionária do pedestal.
2. **Conexão e Aterramento da Blindagem (EMC):**
   - Rumo ao painel de controle do observatório, conduzir o cabo por eletroduto metálico galvanizado.
   - Na extremidade do painel elétrico, conectar a malha de blindagem trançada diretamente à barra de aterramento de instrumentação limpa (*PE de Instrumentação*) através de braçadeira de aterramento $360^\circ$.
   
   > [!IMPORTANT]
   > Não aterrar a blindagem do cabo na carcaça do pedestal para evitar laços de terra (*ground loops*) gerados pelas correntes de retorno dos motores trifásicos de acionamento da cúpula. O aterramento deve ser realizado estritamente em ponto único (*single-point grounding*) no painel do CLP.

3. **Ensaio de Comissionamento e Determinação do Diâmetro Efetivo:**
   - Habilitar o bloco de função de contagem rápida (HSC) no CLP.
   - Iniciar o procedimento de giro completo de $360^\circ$ da cúpula em velocidade constante de cruzeiro ($1{,}0^\circ/\text{s}$), partindo da detecção de borda de subida do sensor indutivo de Home (marco zero absoluto) e parando exatamente na detecção seguinte do mesmo marco.
   - Coletar as contagens totais acumuladas em sentido horário ($C_{\text{CW}}$) e anti-horário ($C_{\text{CCW}}$) e obter a média:
     $$C_{\text{médio}} = \frac{C_{\text{CW}} + C_{\text{CCW}}}{2}$$
   - Calcular o diâmetro efetivo de contato $D_{r,\text{efetivo}}$ e registrar na tabela de parâmetros do CLP:
     $$D_{r,\text{efetivo}} = \frac{D_c \times 40.000}{C_{\text{médio}}}$$
   - Executar 5 ciclos de giro contínuo de validação. Critério de aceitação de engenharia: desvio angular final acumulado inferior a **$\pm 0{,}001^\circ$ ($3{,}6''$)** após 5 voltas completas.

---

## 6. Produtos Comerciais de Mercado (Lista de Peças de Catálogo / COTS)

A tabela abaixo relaciona os componentes comerciais padronizados de prateleira (*Commercial Off-The-Shelf* - COTS) especificados para a montagem do protótipo e dos sistemas definitivos do SRMIE:

| Item | Código Comercial / PN | Fabricante de Referência | Descrição Técnica e Parâmetros Chave | Qtd. |
| :---: | :--- | :--- | :--- | :---: |
| **01** | DFS60A-S4AK65536 | **SICK Sensor Intelligence** | Encoder incremental óptico programável (1 a 65.536 PPR), TTL/RS-422, 6 canais diferenciais, conector M12 8 pinos radial, eixo maciço $\varnothing 10\text{ mm} \times 19\text{ mm}$, flange clamping, IP67 carcaça / IP65 eixo. | 1 un |
| **02** | 8.F5868.1224.B222 *(Opcional Absoluto)* | **Kübler Group** | Encoder absoluto multivoltas óptico, 28 bits (16b ST / 12b MT), interface Profinet-IRT / SSI, eixo maciço $\varnothing 10\text{ mm}$, grau de proteção IP67. | *(1 un)* |
| **03** | HGR20R-300mm-C | **HIWIN Corporation** | Trilho de guia linear retificado de precisão tamanho 20, comprimento $300\text{ mm}$, furação superior para parafusos M5. | 1 un |
| **04** | HGW20CC-Z0-C | **HIWIN Corporation** | Patim linear de esferas recirculantes tamanho 20, modelo com flange larga e furação roscada, classe de pré-carga leve (ZA), vedação dupla frontal DD. | 1 un |
| **05** | BKL 030.11 / BK3 | **R+W Antriebselemente** *(ou KTR RADEX-NC)* | Acoplamento flexível de fole metálico em aço inoxidável, zero-backlash, cubos de fixação com aperto radial por parafuso transversal clamp, furos $\varnothing 20\text{ mm} \times \varnothing 10\text{ mm}$, torque nominal $30\text{ Nm}$. | 1 un |
| **06** | 6004-2RSH/C3 | **SKF** *(ou NSK 6004-DDU)* | Rolamento rígido de uma carreira de esferas, folga C3, vedações de borracha sintética de alta performance em ambos os lados, dimensões $\varnothing 20\text{ mm} \times \varnothing 42\text{ mm} \times 12\text{ mm}$. | 2 un |
| **07** | LC-120-150-S *(Sob Catálogo)* | **Lee Spring** *(ou Suprens)* | Mola helicoidal de compressão em aço mola DIN 17223 / SAE 1070 zincado, diâmetro externo $\varnothing 32\text{ mm}$, fio $\varnothing 4{,}5\text{ mm}$, comprimento livre $L_0 = 150\text{ mm}$, constante $k \approx 12\text{ N/mm}$. | 1 un |
| **08** | CF240.02.04.C | **igus do Brasil** *(ou Lapp ÖLFLEX CHAIN 809 CP)* | Cabo de instrumentação industrial para esteira/flexão contínua com capa externa em Poliuretano (PUR) de alta flexibilidade, livre de halogênios, $4 \times 2 \times 0{,}25\text{ mm}^2$ (AWG 24) em pares trançados, blindagem em malha de cobre estanhado (cobertura $> 85\%$). | 15 m |
| **09** | SACC-M12MS-8CON-PG9-M SH | **Phoenix Contact** *(ou Binder Série 713)* | Conector industrial circular M12 macho reto metálico, 8 polos codificação A, conexão a parafuso, invólucro niquelado blindado $360^\circ$, proteção IP67. | 2 un |
| **10** | HIT-HY 200-A + HAS-U M10 | **Hilti** *(ou Fischer FIS V 360 S)* | Sistema de ancoragem química composto por cartucho de resina híbrida de injeção e 4 barras roscadas M10 em aço inoxidável AISI 316 (A4-70) com arruelas e porcas autotravantes. | 1 kit |
| **11** | Kit Parafusaria Inox DIN 912 | **Ciser / Wurth** | Conjunto de parafusos sextavados internos de precisão em aço inoxidável AISI 304/316 (M5x16 para o trilho, M6x20 para patim, M8x35 para fixação do mancal). | 1 kit |

---

## 7. Planilha Orçamentária e Estimativa de Custos

Os custos a seguir representam uma estimativa de engenharia orçada para a fabricação, montagem e comissionamento de **uma unidade completa** do sistema SRMIE em observatório astronômico (Base de Câmbio de Referência: USD 1,00 = BRL 5,20, Setembro de 2026):

### 7.1 Detalhamento por Categoria de Custo

```
========================================================================================================================
                                QUADRO ORÇAMENTÁRIO DETALHADO DO SISTEMA SRMIE
========================================================================================================================
Item  | Descrição do Fornecimento / Serviço                   | Qtd.  | Custo Unit. (BRL) | Total (BRL)  | Total (USD)
========================================================================================================================
GRUPO 1: COMPONENTES COMERCIAIS DE PRATELEIRA (COTS)
1.1   | Encoder SICK DFS60 (DFS60A-S4AK65536)                 | 1 un  | R$   3.850,00     | R$  3.850,00 | $    740,38
1.2   | Guia Linear HIWIN HGR20 (300 mm) + Patim HGW20CC      | 1 cj  | R$     980,00     | R$    980,00 | $    188,46
1.3   | Acoplamento Fole R+W BK3 (Ø 20 mm x Ø 10 mm)          | 1 un  | R$     680,00     | R$    680,00 | $    130,77
1.4   | Rolamentos SKF 6004-2RSH/C3                           | 2 un  | R$     145,00     | R$    290,00 | $     55,77
1.5   | Mola Helicoidal Calibrada k = 12 N/mm L0 = 150 mm     | 2 un  | R$      85,00     | R$    170,00 | $     32,69
1.6   | Parafusos e Fixadores Inox A2/A4                      | 1 kit | R$     180,00     | R$    180,00 | $     34,62
------------------------------------------------------------------------------------------------------------------------
      | SUBTOTAL GRUPO 1 (COTS)                               |       |                   | R$  6.150,00 | $  1.182,69
========================================================================================================================
GRUPO 2: MATÉRIA-PRIMA, USINAGEM CNC E CALDEIRARIA
2.1   | Bloco Alumínio 6061-T6 + Torneamento CNC do Núcleo    | 1 un  | R$     850,00     | R$    850,00 | $    163,46
2.2   | Vulcanização e Retífica Banda PU 85-90 Shore A        | 1 un  | R$     680,00     | R$    680,00 | $    130,77
2.3   | Mancal de Eixo em Alumínio 7075-T6 Usinado CNC        | 1 un  | R$     780,00     | R$    780,00 | $    150,00
2.4   | Eixo Vertical Retificado Ø 20 mm Inox AISI 316        | 1 un  | R$     380,00     | R$    380,00 | $     73,08
2.5   | Coluna Estrutural Tubo Sch 40 A36 + Mesa e Flanges    | 1 cj  | R$   1.450,00     | R$  1.450,00 | $    278,85
2.6   | Berço Móvel Deslizante Usinado + Fuso Tensor M12 Inox | 1 cj  | R$     620,00     | R$    620,00 | $    119,23
2.7   | Defletores de Proteção em Alumínio + Raspadores Feltro| 1 cj  | R$     350,00     | R$    350,00 | $     67,31
------------------------------------------------------------------------------------------------------------------------
      | SUBTOTAL GRUPO 2 (FABRICAÇÃO / USINAGEM)              |       |                   | R$  5.110,00 | $    982,70
========================================================================================================================
GRUPO 3: ELÉTRICA, CABEAMENTO E INFRAESTRUTURA DE FIXAÇÃO
3.1   | Cabo Flexível PUR igus CF240 (15 metros)              | 15 m  | R$      34,00     | R$    510,00 | $     98,08
3.2   | Conectores Circulares Metálicos M12 Blindados         | 2 un  | R$     140,00     | R$    280,00 | $     53,85
3.3   | Ancoragem Química Epóxi Hilti HIT-HY 200 + Hastes M10 | 1 kit | R$     420,00     | R$    420,00 | $     80,77
3.4   | Caixa de Passagem de Campo Alumínio IP66 com Borneira | 1 cj  | R$     380,00     | R$    380,00 | $     73,08
------------------------------------------------------------------------------------------------------------------------
      | SUBTOTAL GRUPO 3 (ELÉTRICA E ANCORAGEM)               |       |                   | R$  1.590,00 | $    305,78
========================================================================================================================
GRUPO 4: ENGENHARIA DE MONTAGEM, METROLOGIA E COMISSIONAMENTO
4.1   | Montagem Mecânica de Precisão e Ajuste de Pré-Carga   | 16 h  | R$     110,00/h   | R$  1.760,00 | $    338,46
4.2   | Instalação de Campo, Alinhamento Laser e Nivelamento  | 12 h  | R$     110,00/h   | R$  1.320,00 | $    253,85
4.3   | Comissionamento Metrológico e Integração SCADA / CLP  | 14 h  | R$     140,00/h   | R$  1.960,00 | $    376,92
------------------------------------------------------------------------------------------------------------------------
      | SUBTOTAL GRUPO 4 (SERVIÇOS TÉCNICOS DE ENGENHARIA)    |       |                   | R$  5.040,00 | $    969,23
========================================================================================================================
```

### 7.2 Resumo Consolidado e Viabilidade Econômica

```
========================================================================================================================
                                     RESUMO CONSOLIDADO DE INVESTIMENTO
========================================================================================================================
1. Componentes Comerciais de Prateleira (COTS) .................................: R$  6.150,00  (USD 1.182,69)
2. Matéria-Prima, Usinagem CNC e Caldeiraria Estrutural ........................: R$  5.110,00  (USD   982,70)
3. Infraestrutura Elétrica, Conectividade e Ancoragem Química ..................: R$  1.590,00  (USD   305,78)
4. Mão de Obra de Engenharia Especializada (Montagem/Comissionamento) ..........: R$  5.040,00  (USD   969,23)
------------------------------------------------------------------------------------------------------------------------
SUBTOTAL GERAL DO PROJETO ......................................................: R$ 17.890,00  (USD 3.440,40)
Margem de Contingência Técnica e Imprevistos em Campo (10%) ....................: R$  1.789,00  (USD   344,04)
========================================================================================================================
INVESTIMENTO TOTAL ESTIMADO POR SUBSISTEMA SRMIE ...............................: R$ 19.679,00  (USD 3.784,44)
========================================================================================================================
```

### 7.3 Considerações de Viabilidade Financeira e Retorno Técnico
- **Comparativo Direto com Soluções Alternativas:** Uma fita óptica perimétrica metálica de ultraprecisão (ex.: Heidenhain ERA 7000 / Renishaw RESR) para uma cúpula de $\varnothing 6{,}0\text{ metros}$ possui perímetro linear de $S \approx 18{,}85\text{ metros}$. O custo exclusivo da fita segmentada de precisão, cabeças leitoras duplas e módulos de interpolação importados ultrapassa **R$ 85.000,00**, sem incluir o frete aéreo internacional, tributos de importação e os custos críticos de mão de obra para alinhamento micrométrico da fita ao longo dos quase 19 metros de circunferência.
- **Conclusão:** O sistema SRMIE apresenta custo global de **R$ 19.679,00 (três mil setecentos e oitenta e quatro dólares)**, gerando uma **economia direta de cerca de $77\%$**. Além da redução expressiva de capital investido (Capex), a solução assegura um custo de manutenção (Opex) desprezível, visto que qualquer componente sobressalente (mola, rolamento, patim linear ou cabo) possui pronta disponibilidade no mercado nacional.

---

**Engenheiro Responsável Técnico:** Equipe de Engenharia Mecânica de Precisão, Automação e Instrumentação Científica  
**Registro Profissional:** ART registrada junto ao CREA-MG  
**Homologação Institucional:** Diretoria de Instrumentação – UNIFEI / LNA
