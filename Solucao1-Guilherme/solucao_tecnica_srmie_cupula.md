# Memorial Técnico Descritivo e Especificação de Engenharia
## Sistema de Roda de Medição com Mecanismo em Pedestal (SRMIE) para Leitura Azimutal de Cúpula Astronômica

**Documento:** MT-SRMIE-001-REV01  
**Projeto:** Automação e Instrumentação Azimutal de Cúpula Astronômica  
**Instituição:** UNIFEI / LNA (Laboratório Nacional de Astrofísica)  
**Disciplina:** Instrumentação Científica, Engenharia Mecânica de Precisão e Mecatrônica  
**Classificação:** Documento Técnico de Projeto Executivo / Memorial de Engenharia  
**Normas de Referência:** ISO 12100 (Segurança de Máquinas), ISO 13849-1 (Sistemas de Comando Relacionados à Segurança), DIN 2095 (Molas Cilíndricas de Compressão), DIN EN ISO 286 (Sistema ISO de Tolerâncias e Ajustes), IEC 60529 (Graus de Proteção Providos por Invólucros - Códigos IP), ISA-5.1 (Identificação e Simbologia de Instrumentação).

---

## 1. Resumo Executivo e Contexto do Projeto

### 1.1 Contexto e Aplicação
Em observatórios astronômicos modernos, a fenda de observação (*slit*) da cúpula rotativa deve acompanhar com rigor e sincronismo o eixo azimutal do telescópio. Desvios no apontamento angular da cúpula resultam em vinhetamento óptico (*vignetting*), obstrução da linha de visada do telescópio e indução de turbulência aerodinâmica local. A precisão requerida para a leitura da coordenada azimutal situa-se tipicamente na faixa sub-arcminuto ($< 0{,}02^\circ$), demandando repetibilidade metrológica, imunidade a ruídos mecânicos e alta disponibilidade operacional durante as campanhas noturnas de observação.

### 1.2 Problema Superado
Historicamente, sistemas de automação de cúpulas utilizam duas abordagens convencionais de medição azimutal, ambas dotadas de fragilidades severas em ambientes de operação contínua:
1. **Encoders acoplados aos eixos dos motores redutores ou rodas de tração direta:** Apresentam desvios cumulativos inaceitáveis decorrentes de patinagem mecânica (*slip/creep*), deformações viscoelásticas das bandas de rodagem sob carga estática e folgas (*backlash*) nas transmissões mecânicas durante reversões de giro.
2. **Sistemas baseados em fitas ópticas ou magnéticas coladas ao perímetro do trilho:** Apresentam alta sensibilidade à contaminação ambiental (poeira, fuligem, graxa dos mancais de giro e condensação térmica noturna), além de risco crítico de descolamento ou danos mecânicos decorrentes das tolerâncias estruturais de batimento radial (*runout*) e dilatações térmicas diferenciais da cúpula (que podem atingir dezenas de milímetros em anéis de 3 a 10 metros de diâmetro).

### 1.3 Conceito Central da Solução SRMIE
O **Sistema de Roda de Medição com Mecanismo em Pedestal (SRMIE)** soluciona definitivamente esses entraves por meio do **desacoplamento total entre as forças de sustentação/tração da cúpula e o sistema metrológico de medição**. 

O sistema consiste em uma unidade de instrumentação cinemática independente, onde uma roda de medição de alta precisão roda em regime de atrito puro diretamente sobre o anel periférico da cúpula. O contato permanente e sem deslizamento é assegurado por uma pré-carga elástica calibrada gerada por mola helicoidal com ajuste micrométrico. O conjunto sensório-mecânico é assentado sobre uma guia linear de esferas recirculantes montada no topo de um pedestal rígido de piso, firmemente ancorado na laje técnica de concreto do observatório.

```
=============================================================================================
                      DIAGRAMA ESQUEMÁTICO FUNCIONAL DO SISTEMA SRMIE
=============================================================================================

            +-------------------------------------------------------------+
            |      TRILHO GUIA CILÍNDRICO / ANEL PERIFÉRICO DA CÚPULA      |
            +-------------------------------------------------------------+
                                           ^
                                           | Contato Tangencial Puro (Fn = 120 N)
                                           v
                   +-----------------------------------------------+
                   | RODA DE MEDIÇÃO USINADA (Ø 200 mm, PU 90 ShA) |
                   +-----------------------------------------------+
                                           |
                                [Eixo em Mancal P5]
                                           |
                        [Acoplamento Flexível de Fole Metálico]
                                           |
                   +-----------------------------------------------+
                   | ENCODER INCREMENTAL SICK DFS60 (Até 65.536 PPR)|
                   +-----------------------------------------------+
                                           |
   [Fuso Tensor M12]   +---------------------------------------+
   =====( >>> )======> | MESA DESLIZANTE / PATIM LINEAR HIWIN | <=== [Mola Helicoidal 120 N]
                       +---------------------------------------+
                                           |
                       +---------------------------------------+
                       | GUIA LINEAR COM CURSO RADIAL ±35 mm   |
                       +---------------------------------------+
                                           |
                       +---------------------------------------+
                       | PEDESTAL ESTRUTURAL RÍGIDO DE AÇO     |
                       | (Mísulas de reforço / Mesa retificada)|
                       +---------------------------------------+
                                           |
                       +---------------------------------------+
                       | PLACA BASE ANCORADA NA LAJE TÉCNICA   |
                       +---------------------------------------+
```

---

## 2. Especificações Técnicas de Engenharia

A tabela a seguir consolida os parâmetros nominais de projeto e as características técnicas dos subsistemas mecânico, eletroeletrônico e estrutural do SRMIE:

| Parâmetro / Subsistema | Especificação Técnica Detalhada | Critério de Projeto / Justificativa |
| :--- | :--- | :--- |
| **Roda de Medição** | Diâmetro externo nominal: $\varnothing 200{,}00 \pm 0{,}02\text{ mm}$<br>Largura da banda de rodagem: $25\text{ mm}$<br>Núcleo: Alumínio estrutural 6061-T6 usinado e anodizado<br>Revestimento: Poliuretano (PU) usinado com dureza $90 \pm 2\text{ Shore A}$ | Diâmetro otimizado para ampliação da base cinemática e baixa velocidade angular; dureza selecionada para minimizar histerese de deformação elástica sob compressão contínua sem perder coeficiente de atrito estático ($\mu_s \ge 0{,}65$). |
| **Transdutor Angular (Encoder)** | Modelo: SICK DFS60 (DFS60A-S4AA65536) ou equivalente metrológico<br>Princípio: Óptico incremental com saídas diferenciais em quadratura ($A, \bar{A}, B, \bar{B}, Z, \bar{Z}$)<br>Resolução: $65.536\text{ PPR}$ programável via software (ou $10.000\text{ PPR}$ nativo)<br>Interface elétrica: RS-422 (Line Driver / TTL), $V_{cc} = 10\text{ a }30\text{ VDC}$<br>Grau de proteção: IP67 (eixo e carcaça) | Alta densidade de pulsos permitindo discriminação submecanica; saídas diferenciais imunes a transitórios eletromagnéticos induzidos por motores e inversores do observatório. |
| **Mecanismo de Atuação / Carga** | Tipo: Mola helicoidal de compressão em aço liga 51CrV4 (DIN 2095)<br>Constante elástica ($k$): $8{,}0\text{ N/mm}$<br>Carga nominal de trabalho: $120{,}0\text{ N}$ (deflexão $\Delta x = 15{,}0\text{ mm}$)<br>Mecanismo de ajuste: Fuso roscado métrico M12 em aço inox com contraporca sextavada e escala milimétrica | Carga normal ($F_N$) dimensionada para garantir aderência estática irrestrita contra o trilho, impedindo deslizamento sob acelerações dinâmicas da cúpula ($\alpha_{\text{máx}} = 0{,}5^\circ/\text{s}^2$). |
| **Guia de Translação Radial** | Modelo: Trilho linear retificado com patim de esferas recirculantes de quatro pistas de contato a 45° (Série Hiwin HGW20CC ou MGN15/HGR20)<br>Curso linear livre: $\pm 35\text{ mm}$ (curso útil total de $70\text{ mm}$)<br>Capacidade de carga dinâmica ($C$): $17{,}75\text{ kN}$<br>Vedação: Raspadores frontais de topo duplos e selos laterais | Rigidez a momentos fletores nos planos vertical e transversal ($M_x, M_y, M_z$), garantindo grau de liberdade unidirecional puramente radial e absorvendo o batimento da cúpula. |
| **Topologia de Montagem** | Disposição vertical coaxial inversa: Pedestal de piso ancorado, roda de contato no plano horizontal superior, mancal de precisão intermediário e encoder instalado na parte inferior da mesa móvel.<br>Perfil superior: Topo liso e rebaixado sem projeções de eixos | Elimina riscos de interferência mecânica com a borda de saia ou anel da cúpula; protege o sensor óptico contra intempéries e partículas por gravidade. |
| **Cabeamento e Interconexão** | Cabo de instrumentação flexível especial PUR, $4 \times 2 \times 0{,}25\text{ mm}^2$ (AWG 24), blindagem dupla (malha de cobre estanhado trançado $> 85\%$ + fita de alumínio/poliéster), conector circular industrial M12 metálico 8 pinos codificação A (IP67).<br>Laço de alívio (*service loop*): Raio mínimo de curvatura dinâmico $R_{\text{curv}} = 100\text{ mm}$ | Integridade física contra fadiga mecânica por flexão cíclica contínua decorrente da absorção das oscilações da cúpula. |

---

## 3. Detalhamento Mecânico e Funcional

### 3.1 Arquitetura Estrutural da Base e Coluna do Pedestal
A sustentação estática e a estabilidade dinâmica do SRMIE baseiam-se em uma coluna estrutural tubular fabricada em aço estrutural ASTM A500 Grau B (perfil quadrado $100 \times 100 \times 6{,}35\text{ mm}$ ou tubo cilíndrico $\varnothing 114{,}3 \times 6{,}02\text{ mm}$). 

```
                                    +===========================+
                                    | MESA RETIFICADA SUPERIOR  |
                                    +===========================+
                                      /                       \
                                     / [Mísulas Soldadas 45°]  \
                                    /                           \
                                   +-----------------------------+
                                   |                             |
                                   |    COLUNA TUBULAR DE AÇO    |
                                   |    (ASTM A500 - Parede 6mm) |
                                   |                             |
                                   +-----------------------------+
                                      /                       \
                                     / [Mísulas de Reforço 45°]\
                                    /                           \
                                  +===============================+
                                  |   PLACA DE BASE (e = 20 mm)   |
                                  +===============================+
                                    |     |     |     |     |
                                  [4x Chumbadores Químicos M12]
```

- **Placa de Base:** Confeccionada em chapa de aço carbono SAE 1020 com espessura de $20\text{ mm}$, usinada em fresadora para garantir planicidade inferior a $0{,}05\text{ mm}$. Dotada de 4 furos oblongos de $\varnothing 14\text{ mm}$ em um raio de furação de $220\text{ mm}$, viabilizando microajustes angulares durante a fase de alinhamento com o raio da cúpula.
- **Reforços Antitorção:** Quatro mísulas triangulares em chapa de $9{,}52\text{ mm}$ ($3/8^{\prime\prime}$) soldadas a $45^\circ$ conectam as faces da coluna à placa de base, elevando o momento de inércia da seção de engastamento e prevenindo oscilações modais induzidas por vibrações do prédio do observatório.
- **Mesa Superior:** Placa retificada de $16\text{ mm}$ soldada e reforçada por mãos francesas no topo da coluna, servindo de base de assentamento usinada para o trilho linear Hiwin HGR20.

### 3.2 Cinemática de Contato e Resolução Angular
O sistema adota cinemática de contato pontual/linear tangencial no plano horizontal da cúpula. O grau de liberdade de translação é restringido a um vetor estritamente radial em relação ao centro de rotação da cúpula astronômica:

$$\vec{v}_{\text{roda}} = \vec{v}_{\text{trilho}} = \omega_{\text{cúpula}} \times \vec{R}_{\text{trilho}}$$

A relação de transmissão cinemática ($\gamma$) entre a cúpula e a roda de medição é expressa por:

$$\gamma = \frac{D_{\text{trilho}}}{D_{\text{roda}}}$$

Para uma cúpula padrão de observatório com diâmetro de trilho $D_{\text{trilho}} = 3000{,}0\text{ mm}$ e a roda de medição nominal $D_{\text{roda}} = 200{,}0\text{ mm}$:

$$\gamma = \frac{3000{,}0}{200{,}0} = 15{,}0$$

Com o encoder SICK DFS60 operando em decodificação de quadratura $4\times$ ($4 \times 65.536 = 262.144\text{ contagens/revolução}$ da roda):

$$N_{\text{total\_360}} = \gamma \times 262.144 = 15{,}0 \times 262.144 = 3.932.160\text{ pulsos/giro da cúpula}$$

A resolução teórica de posicionamento azimutal da cúpula ($\delta\theta$) é calculada por:

$$\delta\theta = \frac{360^\circ}{3.932.160} = 0{,}0000915^\circ \approx 0{,}329\text{ arcsegundos}$$

> [!NOTE]
> Essa resolução nanométrica de medição assegura que a banda morta de controle (*deadband*) do inversor de frequência do motor de tração da cúpula opere com folga metrológica de mais de duas ordens de grandeza em relação ao limite estipulado de sincronismo astronômico ($< 1\text{ arcminuto}$).

### 3.3 Montagem Coaxial do Encoder e Desacoplamento de Cargas
Uma das premissas críticas deste projeto de instrumentação é a preservação dos rolamentos internos do encoder óptico contra esforços mecânicos de choque e forças radiais:

```
           [ RODA DE MEDIÇÃO Ø 200 mm COM BANDA PU 90 ShA ]
                                  |
               +------------------+------------------+
               | Eixo de Transmissão Ø 20 mm (Inox)  |
               +------------------+------------------+
                                  |
        +----------------------------------------------------+
        | MANCAL DUPLO USINADO COM ROLAMENTOS DE PRECISÃO P5 |
        | (2x Rolamentos rígidos de esferas 6004-2RSH pré-c.) |
        +----------------------------------------------------+
                                  |
                +-----------------+-----------------+
                | Acoplamento Flexível Fole Metálico|
                | (Zero-Backlash, Rigidez Torsional)|
                +-----------------+-----------------+
                                  |
        +----------------------------------------------------+
        | ENCODER SICK DFS60 (Flange de fixação na mesa)     |
        | Eixo Ø 10 mm (Submetido unicamente a torque puro)  |
        +----------------------------------------------------+
```

1. **Eixo Dedicado com Mancalização Independente:** A roda de medição é chavetada em um eixo de aço inoxidável AISI 316L ($\varnothing 20\text{ mm}$ usinado e retificado h6), apoiado por um mancal de liga de alumínio aeronáutico 7075-T6 contendo dois rolamentos de esferas de precisão (SKF 6004-2RSH, classe ISO P5 / ABEC 5).
2. **Absorção da Pré-Carga de 120 N:** Toda a força radial de contato de $120\text{ N}$ e eventuais forças verticais parasitas são totalmente absorvidas pelos rolamentos do mancal dedicado e descarregadas no patim Hiwin.
3. **Acoplamento Flexível de Fole Metálico (*Bellows Coupling*):** A ligação entre o eixo de saída do mancal e o eixo de entrada do encoder ($\varnothing 10\text{ mm}$) é realizada por um acoplamento de fole de níquel/inox com fixação por abraçadeira de aperto radial (*clamping hub*). Esse elemento garante:
   - Folga angular zero (*zero backlash*) com alta rigidez torsional ($> 1200\text{ Nm/rad}$).
   - Isolamento total contra desvios de concentricidade ($\le 0{,}15\text{ mm}$) e desalinhamentos axiais/angulares ($\le 1{,}5^\circ$).
   - Proteção absoluta dos rolamentos do encoder contra cargas axiais e radiais superiores a $20\text{ N}$.

### 3.4 Sistema de Proteção Ambiental e Defletores
Em cúpulas de observatórios, contaminantes como pó atmosférico, fezes de aves, insetos e graxa sintética ejetada dos trilhos de movimentação constituem risco metrológico. O conjunto integra:
- **Defletor Superior em Domo:** Cobertura de proteção em chapa de alumínio naval dobrada de $2\text{ mm}$, montada sobre o pedestal, formando um labirinto mecânico que cobre o encoder e a guia linear, impedindo a queda de condensação noturna ou detritos por gravidade.
- **Conjunto Raspador de Trilho (*Wiper System*):** Duas lâminas tangenciais de feltro industrial de lã prensada de alta densidade (espessura $6\text{ mm}$), impregnadas com óleo mineral fino, montadas imediatamente antes da área de contato da roda de medição, responsáveis por varrer partículas sólidas e microgotículas de umidade da pista de rolagem antes do ponto de medição.
- **Vedação do Patim Linear:** Patins Hiwin equipados de fábrica com vedação dupla de lábio elastomérico (DD Seal) e raspadores metálicos adicionais de extremidade contra partículas abrasivas.

---

## 4. Análise de Vantagens e Desvantagens / Restrições

### 4.1 Matriz Comparativa de Arquiteturas de Sensoriamento Azimutal

| Critério Técnico de Avaliação | Encoder em Roda Motora (Tração Direta) | Fita Óptica / Magnética no Perímetro | SRMIE em Pedestal com Guia Linear (Solução Adotada) |
| :--- | :--- | :--- | :--- |
| **Imunidade a Deslizamento (*Slip/Creep*)** | **Crítica:** Patina em acelerações e variações de atrito do trilho. | **Excelente:** Fixa ao anel, sem deslizamento relativo. | **Excelente:** Desacoplado da tração, carga constante de $120\text{ N}$, sem deslizamento. |
| **Absorção de *Runout* e Ovalização** | **Inexistente:** Erros integrados no redutor mecânico. | **Muito Crítica:** Desalinhamento da cabeça leitora se distância variar $> 1\text{ mm}$. | **Excelente:** Guia linear absorve até $\pm 35\text{ mm}$ de oscilação radial do anel. |
| **Robustez a Poeira, Umidade e Graxa** | **Boa:** Sensores internos em carcaças seladas. | **Baixa:** Fita ótica cega com condensação e graxa; leitor magnético atrai limalha. | **Excelente:** Roda com raspadores periféricos e encoder protegido em caixa IP67. |
| **Custo de Implantação e Retrofit** | **Baixo:** Mecanicamente simples, porém ineficaz em precisão. | **Muito Alto:** Fita perimétrica de grande diâmetro com custo linear elevado. | **Médio:** Baseado em COTS industriais e caldeiraria simples sobre pedestal. |
| **Ergonomia e Acesso de Manutenção** | **Ruim:** Montado junto a motorredutores pesados de teto/parede. | **Ruim:** Acesso perigoso a todo o perímetro superior da cúpula. | **Excelente:** Concentrado em um único ponto acessível na laje técnica. |

### 4.2 Detalhamento das Vantagens do SRMIE
1. **Contato Permanente Assegurado:** A carga elástica de $120\text{ N}$ calculada para a banda de PU $90\text{ Shore A}$ assegura pressão de contato estático de Hertz controlada ($\sigma_{\text{contato}} \approx 1{,}8\text{ MPa}$), garantindo atrito estático suficiente para acelerar o conjunto inercial da roda e encoder sem escorregamento microcinemático.
2. **Desacoplamento Cinemático e Dinâmico:** Eventuais choques ou solavancos nos motores tracionadores da cúpula não transmitem esforços dinâmicos para a cadeia cinemática de medição.
3. **Absorção Dinâmica de Deformações Térmicas e Ovalizações:** A cúpula pode sofrer variações sazonais de raio de até $\pm 20\text{ mm}$ decorrentes de amplitudes térmicas de $25^\circ\text{C}$ sem alterar a precisão angular da medição, graças à liberdade de curso linear do patim.
4. **Topo Desobstruído e Seguro:** A disposição do encoder no nível inferior do suporte móvel mantém o topo da roda plano, eliminando qualquer risco de esmagamento ou colisão com elementos estruturais móveis da cúpula.

### 4.3 Desvantagens e Restrições Técnicas (Mitigações de Engenharia)

> [!WARNING]
> A mitigação das restrições térmicas e de desgaste exige a parametrização precisa das rotinas de comissionamento descritas abaixo.

- **Variação Térmica do Raio da Roda de Poliuretano:**
  - *Restrição:* O coeficiente de dilatação térmica linear do poliuretano fundido é de aproximadamente $\alpha_{\text{PU}} \approx 1{,}5 \times 10^{-4}\text{ K}^{-1}$. Um gradiente térmico de $\Delta T = \pm 15^\circ\text{C}$ (típico de sítios astronômicos de alta montanha, como o Pico dos Dias/LNA) gera variação radial de $\Delta r = r_0 \cdot \alpha_{\text{PU}} \cdot \Delta T = 100 \times (1{,}5 \times 10^{-4}) \times 15 = \pm 0{,}225\text{ mm}$, o que alteraria a razão de escala em aproximadamente $0{,}22\%$.
  - *Ação de Mitigação:* Incorporação de um sensor de temperatura industrial PT100 na mesa do pedestal. O algoritmo no CLP/PAC calcula a correção térmica em tempo real:
    $$D_{\text{efetivo}}(T) = D_0 \cdot \left[1 + \alpha_{\text{eq}} \cdot (T - T_0)\right]$$
    Adicionalmente, rotinas automáticas de reindexação ao marco absoluto (sensor Home) a cada volta completa eliminam o erro acumulado.
- **Desgaste da Banda de Rodagem:**
  - *Restrição:* O atrito de rolamento induz desgaste microscópico abrasivo do PU ao longo de milhões de rotações.
  - *Ação de Mitigação:* Adoção de formulação de poliuretano base Vulkollan® de altíssima resistência à abrasão ($< 35\text{ mm}^3$ conforme DIN ISO 4649). Calibração periódica automatizada via SCADA e substituição programada da roda com intervalo previsto de 5 anos de campanha contínua.
- **Espaço Físico Ocupado na Laje Técnica:**
  - *Restrição:* A projeção do pedestal na laje requer uma área livre de piso de aproximadamente $400 \times 400\text{ mm}$.
  - *Ação de Mitigação:* Posicionamento do pedestal alinhado com pilares ou regiões de guarda-corpo técnico existentes, com passagem de cabos embutida em leitos subterrâneos ou canaletas industriais metálicas para evitar interferência na circulação da equipe de astrofísica.

---

## 5. Procedimento Detalhado de Instalação e Comissionamento

```
=============================================================================================
             FLUXOGRAMA SEQUENCIAL DE INSTALAÇÃO E COMISSIONAMENTO DO SRMIE
=============================================================================================

 [1. LOCAÇÃO E ANCORAGEM]   --> Furação da laje, limpeza de poeira e cura do chumbador químico
            |
 [2. NIVELAMENTO DO PEDESTAL]--> Ajuste de ortogonalidade (< 0,05 mm/m) e alinhamento radial
            |
 [3. MONTAGEM MECATRÔNICA]  --> Instalação da guia linear Hiwin, mancal, acoplamento e encoder
            |
 [4. CALIBRAÇÃO DA PRÉ-CARGA]--> Fuso M12 ajustado para 120 N com dinamômetro / deflexão da mola
            |
 [5. CABEAMENTO E LOOP]     --> Roteamento com raio > 100 mm, aterramento 360° e teste elétrico
            |
 [6. CALIBRAÇÃO METROLÓGICA]--> Giro de 360° contra chave de Home óptico / Determinação do D_efetivo
```

### 5.1 Etapa 1: Locação e Chumbamento Químico da Placa de Base
1. Com a cúpula estacionada e travada, utilizar alinhador laser posicionado no centro geométrico do edifício para projetar o raio vetorial exato até a pista de contato do anel de rolamento.
2. Marcar a posição dos 4 pontos de ancoragem da placa de base no concreto da laje técnica.
3. Executar furação com broca de vídia $\varnothing 16\text{ mm}$ até a profundidade de embutimento de $110\text{ mm}$.
4. Limpar rigorosamente os furos com ar comprimido isento de óleo e escova circular de aço em pelo menos três ciclos alternados.
5. Injetar resina química à base de epóxi puro de alta performance (Hilti HIT-RE 500 V4 ou Fischer FIS EM Plus).
6. Inserir barras roscadas em aço inoxidável AISI 316 M12 com comprimento útil exposto de $50\text{ mm}$. Respeitar o tempo de cura química estipulado pelo fabricante ($24\text{ horas}$ a $20^\circ\text{C}$) antes da aplicação de qualquer esforço de torque.

### 5.2 Etapa 2: Nivelamento do Pedestal e Alinhamento Ortogonal
1. Posicionar o pedestal sobre as barras roscadas utilizando porcas de nivelamento inferiores e arruelas esféricas de compensação de ângulo.
2. Posicionar nível de precisão de bolha para mecânica pesada (resolução $0{,}02\text{ mm/m}$) sobre a mesa retificada superior do pedestal.
3. Ajustar as quatro porcas inferiores até obter nivelamento estrito nos eixos longitudinal e transversal ($\le 0{,}05\text{ mm/m}$).
4. Posicionar esquadro de precisão de granito/aço classe 0 contra a face guia da mesa superior e referenciar perpendicularmente à tangente da cúpula com auxílio de relógio comparador milesimal. Tolerância angular admitida: erro inferior a $\pm 0{,}05^\circ$ ($3\text{ arcmin}$).
5. Aplicar o torque definitivo nas porcas superiores com torquímetro calibrado: $T_{\text{aperto}} = 65\text{ Nm}$.

### 5.3 Etapa 3: Fixação do Conjunto Linear, Roda e Encoder
1. Limpar as superfícies de assentamento do trilho com desengraxante atóxico e montar o trilho linear Hiwin HGR20. Apertar os parafusos M5 classe 12.9 do centro para as extremidades com torque de $8{,}8\text{ Nm}$ utilizando adesivo trava-rosca anaeróbico de médio torque (Loctite 242).
2. Acoplar a mesa deslizante móvel aos patins HGW20CC. Verificar deslizamento suave manual, com resistência de atrito uniforme inferior a $3\text{ N}$.
3. Instalar o conjunto do mancal independente pré-montado na mesa móvel.
4. Acoplar o encoder SICK DFS60 na flange inferior usinada, mantendo concentricidade rigorosa com o eixo do mancal.
5. Fixar o acoplamento de fole metálico nos eixos do mancal e do encoder, garantindo que não haja qualquer esforço axial de montagem sobre o sensor óptico (inserção suave deslizante). Apertar os parafusos de fixação tangencial do fole com $1{,}8\text{ Nm}$.
6. Instalar a roda de medição de $\varnothing 200\text{ mm}$ no topo do eixo, fixando-a com chaveta de precisão e anel elástico de retenção axial em inox DIN 471.

### 5.4 Etapa 4: Ajuste da Pré-Carga da Mola (Força Nominal de 120 N)
1. Recuar totalmente o fuso roscado M12 até que a mola helicoidal fique em seu comprimento livre ($L_0 = 75{,}0\text{ mm}$).
2. Liberar o mecanismo móvel até que a banda de poliuretano encoste suavemente no trilho da cúpula (ponto de contato zero, $L_{\text{contato}} = 75{,}0\text{ mm}$).
3. Posicionar célula de carga portátil tipo panqueca (ou dinamômetro digital de compressão) entre o batente do fuso e a mola móvel para validação metrológica.
4. Girar o fuso tensor M12 com passo de $1{,}75\text{ mm}$ até atingir a deflexão de projeto:
   $$\Delta x = \frac{F_N}{k} = \frac{120{,}0\text{ N}}{8{,}0\text{ N/mm}} = 15{,}0\text{ mm}$$
   (Correspondente a 8 voltas e meia no parafuso tensor).
5. Validar a leitura na célula de carga: $120{,}0 \pm 5{,}0\text{ N}$.
6. Travar a contraporca sextavada de aço inoxidável com chave fixa de $19\text{ mm}$ e aplicar laca de selagem de segurança (*torque seal*) vermelha sobre a rosca.

### 5.5 Etapa 5: Roteamento do Cabeamento e Laço de Alívio (*Service Loop*)
1. Conectar o conector fêmea industrial metálico M12 reto blindado na carcaça do encoder SICK DFS60, travando o anel roscado com torque manual firme ($0{,}6\text{ Nm}$).
2. Moldar o cabo flexível PUR em forma de ômega ($\Omega$), criando um laço de alívio com raio de curvatura dinâmico superior a $100\text{ mm}$ ancorado na mesa móvel e na coluna fixa por presilhas almofadadas de borracha EPDM.
3. Rumo à caixa de junção de campo (JB-01) montada na base do pedestal, passar o cabo através de prensa-cabo metálico M16 com contato de blindagem EMC $360^\circ$ de latão niquelado.
4. Conectar a malha de blindagem do cabo à barra de aterramento de instrumentação limpa (PE) exclusivamente na extremidade da caixa de junção, prevenindo loops de terra (*ground loops*).
5. Executar teste de resistência de isolamento dos condutores elétricos com megômetro a $500\text{ VDC}$ ($R_{\text{iso}} > 100\text{ M}\Omega$) e validação da continuidade de sinais $A, \bar{A}, B, \bar{B}, Z, \bar{Z}$.

### 5.6 Etapa 6: Procedimento de Calibração Inicial do Diâmetro Efetivo
1. Posicionar uma bandeira metálica ferromagnética de aço carbono com espessura de $3\text{ mm}$ e largura de $10\text{ mm}$ fixada na face lateral da cúpula, alinhada com um sensor de proximidade indutivo fixo de alta repetibilidade ($< 0{,}01\text{ mm}$), que servirá como marco de referência absoluto de azimute zero ($0{,}000^\circ$).
2. No painel de controle ou sistema SCADA, zerar o registrador do cartão de contagem rápida (HSC) no momento da primeira detecção de subida do sensor indutivo.
3. Comandar a rotação lenta e contínua da cúpula no sentido horário a uma velocidade constante de $1{,}0^\circ/\text{s}$, completando exatamente uma volta ($360^\circ$) até a segunda detecção da mesma bandeira de referência pelo sensor de proximidade.
4. Ler o valor total de pulsos acumulados no registrador ($N_{\text{leitura\_360}}$).
5. Repetir o teste no sentido anti-horário e calcular a média ponderada para eliminar eventuais histereses dinâmicas:
   $$\bar{N}_{360} = \frac{N_{\text{horário}} + N_{\text{anti-horário}}}{2}$$
6. Sabendo-se o perímetro real do anel do trilho $S_{\text{cupula}}$ (medido por topografia a laser ou trena de precisão calibrada) e os pulsos por volta do encoder ($PPR_{\text{encoder}} = 262.144\text{ pulsos/rev}$ em quadratura), calcular o diâmetro efetivo de contato $D_{\text{efetivo}}$:
   $$D_{\text{efetivo}} = \frac{S_{\text{cúpula}} \times PPR_{\text{encoder}}}{\pi \times \bar{N}_{360}}$$
7. Gravar os coeficientes metrológicos de calibração na memória permanente retentiva do CLP e validar com 3 giros consecutivos completos. Erro aceitável máximo de repetibilidade: $\le \pm 2\text{ pulsos}$ no encoder ($< 0{,}001^\circ$ de cúpula).

---

## 6. Lista de Materiais Comerciais (BOM) e Estimativa de Custos

Os valores orçados referem-se a custos médios praticados no mercado nacional e internacional para componentes de alta confiabilidade em aplicações científicas e industriais críticas (Base: Câmbio USD/BRL = R$ 5,20, Setembro/2026).

### 6.1 Componentes Comerciais de Prateleira (COTS)

| Item | Descrição / Especificação do Componente | Fabricante / Referência | Qtd. | Custo Unit. (BRL) | Custo Total (BRL) |
| :---: | :--- | :--- | :---: | :---: | :---: |
| 1.1 | Encoder incremental óptico programável (até 65.536 PPR, RS-422, IP67, conector M12) | SICK / DFS60A-S4AA65536 | 1 un | R$ 3.850,00 | R$ 3.850,00 |
| 1.2 | Conjunto de guia linear com trilho retificado HGR20 (L = 250 mm) e 2 patins de esferas com flanges HGW20CC | HIWIN / HGW20CCZ0C | 1 cj | R$ 980,00 | R$ 980,00 |
| 1.3 | Roda de medição de precisão $\varnothing 200\text{ mm}$, núcleo em alumínio usinado, banda de poliuretano usinado $90\text{ Shore A}$ | Blickle / RTH 200/20H ou Sick BEF-MR010020R (mod.) | 1 un | R$ 1.450,00 | R$ 1.450,00 |
| 1.4 | Mola helicoidal de compressão em aço liga 51CrV4 (DIN 2095, $D_e = 32\text{ mm}, d = 4{,}5\text{ mm}, L_0 = 75\text{ mm}, k = 8\text{ N/mm}$) | Molas Balbino / DIN 2095-E | 2 un | R$ 85,00 | R$ 170,00 |
| 1.5 | Rolamento rígido de esferas de precisão (Classe ISO P5 / ABEC 5, 2RSH, $\varnothing 20 \times \varnothing 47 \times 14\text{ mm}$) | SKF / 6004-2RSH/P5 | 2 un | R$ 190,00 | R$ 380,00 |
| 1.6 | Acoplamento flexível de fole metálico em aço inox/níquel, fixação por aperto radial clamp, $\varnothing 20\text{ mm} \times \varnothing 10\text{ mm}$, zero-backlash | R+W / BKL 030 ou KTR TOOLFLEX | 1 un | R$ 680,00 | R$ 680,00 |
| 1.7 | Kit de parafusaria estrutural em aço inoxidável AISI 304/316 (DIN 912 M5, M8, M12, arruelas de pressão e porcas Parlock) | Ciser / Wurth | 1 kit | R$ 240,00 | R$ 240,00 |
| **Subtotal COTS** | | | | | **R$ 7.750,00** |

### 6.2 Itens de Caldeiraria, Usinagem e Fabricação Estrutural

| Item | Descrição / Especificação do Serviço / Componente | Material / Processo | Qtd. | Custo Unit. (BRL) | Custo Total (BRL) |
| :---: | :--- | :--- | :---: | :---: | :---: |
| 2.1 | Coluna estrutural do pedestal ($100 \times 100 \times 6{,}35\text{ mm}, H = 850\text{ mm}$) com mísulas soldadas a $45^\circ$ e pintura eletrostática a pó poliéster texturizada | Tubo ASTM A500 + Chapa SAE 1020 / Soldagem MIG certificada | 1 cj | R$ 1.600,00 | R$ 1.600,00 |
| 2.2 | Placa base retificada de ancoragem ($300 \times 300 \times 20\text{ mm}$) com furação oblonga fresada | Aço SAE 1020 usinado em CNC | 1 un | R$ 850,00 | R$ 850,00 |
| 2.3 | Berço móvel do suporte de mancal e flange de acoplamento do encoder | Alumínio aeronáutico 7075-T6 usinado e anodizado | 1 cj | R$ 1.250,00 | R$ 1.250,00 |
| 2.4 | Eixo de acionamento retificado ($\varnothing 20\text{ mm}$, h6) com canal de chaveta e ranhura para anel elástico | Aço Inox AISI 316L torneado e retificado | 1 un | R$ 420,00 | R$ 420,00 |
| 2.5 | Fuso roscado tensor de pré-carga M12 com escala milimétrica gravada a laser e suporte de batente | Aço Inox AISI 304 | 1 cj | R$ 380,00 | R$ 380,00 |
| 2.6 | Cobertura defletora superior em domo e raspadores tangenciais de feltro | Chapa Alumínio Naval 5052-H32 / Corte laser | 1 cj | R$ 480,00 | R$ 480,00 |
| **Subtotal Fabricação / Usinagem** | | | | | **R$ 4.980,00** |

### 6.3 Eletrônica, Interconexão e Instalação em Campo

| Item | Descrição / Especificação do Componente | Fabricante / Referência | Qtd. | Custo Unit. (BRL) | Custo Total (BRL) |
| :---: | :--- | :--- | :---: | :---: | :---: |
| 3.1 | Cabo especial flexível de instrumentação e controle PUR ($4 \times 2 \times 0{,}25\text{ mm}^2$, blindagem dupla malha/fita) | LAPP KABEL / ÖLFLEX CHAIN 809 CP | 15 m | R$ 32,00 | R$ 480,00 |
| 3.2 | Conector circular reto fêmea M12 metálico blindado 8 polos IP67 (Codificação A) | Phoenix Contact / SACC-M12FS-8CON-PG9-M SH | 2 un | R$ 145,00 | R$ 290,00 |
| 3.3 | Caixa de passagem e junção de campo (JB-01) em alumínio fundido IP66 ($160 \times 160 \times 90\text{ mm}$) com borneira push-in e prensa-cabos EMC | Rittal / Weidmüller | 1 cj | R$ 620,00 | R$ 620,00 |
| 3.4 | Sistema de ancoragem química composto por bisnaga de resina epóxi pura e 4 barras roscadas M12 inox 316 | Hilti / HIT-RE 500 V4 + HAS-U M12 | 1 kit | R$ 450,00 | R$ 450,00 |
| 3.5 | Insumos de montagem (trava-rosca Loctite 242, laca selante de torque, lixas, feltros de reserva, graxa especial de lítio Klüber) | Diversos industriais | 1 kit | R$ 260,00 | R$ 260,00 |
| **Subtotal Eletrônica / Instalação** | | | | | **R$ 2.100,00** |

---

### 6.4 Resumo Financeiro Consolidado e Viabilidade

```
=============================================================================================
                      QUADRO CONSOLIDADO DE CUSTOS DO PROJETO SRMIE
=============================================================================================
  1. Componentes Comerciais de Prateleira (COTS) ...........:  R$  7.750,00  (USD 1.490,38)
  2. Fabricação Estrutural, Caldeiraria e Usinagem CNC .....:  R$  4.980,00  (USD   957,69)
  3. Eletrônica de Conexão e Materiais de Instalação .......:  R$  2.100,00  (USD   403,85)
---------------------------------------------------------------------------------------------
  SUBTOTAL DE MATERIAIS E SERVIÇOS TÉCNICOS ................:  R$ 14.830,00  (USD 2.851,92)
  Taxa de Contingência de Montagem e Testes (10%) ..........:  R$  1.483,00  (USD   285,19)
=============================================================================================
  INVESTIMENTO TOTAL ESTIMADO POR CONJUNTO SRMIE ...........:  R$ 16.313,00  (USD 3.137,11)
=============================================================================================
```

### 6.5 Conclusão de Viabilidade Técnica e Econômica
O orçamento consolidado de aproximadamente **R$ 16.313,00 (três mil e cento e trinta e sete dólares)** demonstra excelente atratividade financeira frente a soluções concorrentes de fita magnética/óptica perimétrica, cujos orçamentos para grandes diâmetros de cúpula ultrapassam R$ 60.000,00 a R$ 90.000,00 entre materiais importados e custos críticos de instalação.

A solução SRMIE reúne simplicidade construtiva com peças de reposição de pronta-entrega (COTS), facilidade de manutenção sem desmontagem de partes móveis da cúpula, e robustez metrológica insensível a deslizamentos e excentricidades dinâmicas, atendendo integralmente aos requisitos de observação científica do Observatório Astronômico.

---

**Engenheiro Responsável:** Equipe de Engenharia Mecânica de Precisão e Instrumentação Científica  
**Registro Profissional (CREA):** Conforme Anotação de Responsabilidade Técnica (ART/CREA-MG)  
**Aprovação do Projeto:** Diretoria Técnica de Instrumentação – UNIFEI / LNA
