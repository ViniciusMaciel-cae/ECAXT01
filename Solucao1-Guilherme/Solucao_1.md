# Documentação Técnica: Sistema de Roda de Medição Independente com Encoder (SRMIE)

**Projeto:** Instrumentação Angular de Cúpula Astronômica  
**Data:** Setembro 2026  
**Equipe:** Engenharia Mecatrônica – UNIFEI / LNA

---

## 1. Detalhamento Técnico e Cinemática

O **Sistema de Roda de Medição Independente com Encoder (SRMIE)** é uma arquitetura de sensoriamento projetada para medir com precisão o azimute de cúpulas astronômicas. O princípio fundamental do projeto é o **desacoplamento total entre a função de tração e a função de medição**.

### Princípio de Funcionamento
O mecanismo consiste em uma roda emborrachada cujo eixo é acoplado em *direct-drive* a um encoder rotativo incremental. Este conjunto não fornece força motriz; ele atua puramente como um sensor tátil em contato constante com o anel guia (trilho) da cúpula. 

Para manter o contato ininterrupto, o conjunto é montado na extremidade de um **braço pantográfico articulado**, que possui um grau de liberdade de movimento puramente radial. Uma mola helicoidal aplica uma pré-carga mecânica constante que pressiona a roda contra o trilho.

### Absorção de Irregularidades
Cúpulas astronômicas de grande porte apresentam anéis estruturais sujeitos a deformações térmicas, tolerâncias de usinagem e ovalizações milimétricas (excentricidade rotacional). Sistemas rígidos baseados em engrenagens ou fitas ópticas coladas ao perímetro falham sob essas variações. O braço pantográfico do SRMIE resolve esse problema atuando como uma "suspensão ativa": o braço avança ou recua passivamente, absorvendo o erro radial do trilho sem perder o contato tátil e mantendo a roda tangencial à pista de rolagem.

### Formulação Matemática e Cinemática
O projeto opera sob o regime cinemático de **rolamento puro sem deslizamento**, garantido pela ausência de torque motor na roda e pela alta aderência do elastômero. A relação de transmissão (razão cinemática $\gamma$) é definida por:

$$ \gamma = \frac{R}{r} $$

Onde:
- $R$ = Raio médio do trilho de contato da cúpula (ex: 1.500 mm).
- $r$ = Raio dinâmico da roda de medição (ex: 50 mm).

A velocidade tangencial $v$ na interface de contato é idêntica para ambos os corpos. Assim, o deslocamento angular da cúpula ($\theta_c$) e o deslocamento angular da roda ($\theta_r$) relacionam-se por:

$$ \theta_c \cdot R = \theta_r \cdot r \implies \theta_r = \theta_c \cdot \left(\frac{R}{r}\right) $$

Como a relação geométrica do projeto prevê $R=1500$ mm e $r=50$ mm, a razão é $\gamma = 30$. Para cada $1^{\circ}$ de giro da cúpula, a roda gira $30^{\circ}$.

---

## 2. Especificações do Sistema

Os parâmetros nominais de hardware adotados para a validação mecânica e elétrica do SRMIE são detalhados a seguir:

- **Dimensões Geométricas:**
  - Raio da Cúpula ($R$): 1.500 mm.
  - Diâmetro exato da Roda: 100 mm (Raio $r$ = 50 mm).
  - Razão Cinemática ($\gamma$): 1:30.
- **Roda de Medição:**
  - Material da banda de rodagem: Poliuretano (PU) fundido.
  - Dureza especificada: 75 a 85 Shore A (otimizado para aderência sem deformação elástica excessiva).
- **Sensoriamento (Encoder):**
  - Tipo: Incremental óptico ou magnético, eixo sólido, flange de montagem fixada ao pantógrafo.
  - Resolução nativa: 1.024 PPR (Pulsos Por Revolução).
  - Decodificação: Leitura em quadratura (Fases A e B), multiplicando a resolução por 4 (4.096 pulsos/revolução da roda).
- **Desempenho Algorítmico Calculado:**
  - Pulsos totais para 360° da Cúpula: $N_{total} = 1024 \times 4 \times 30 = 122.880$ pulsos.
  - Resolução Angular de Controle: $\frac{360^{\circ}}{122.880} \approx 0,0029^{\circ}$ (aproximadamente 0,175 arcminutos).
- **Mecânica Articulada:**
  - Braço: Perfil de alumínio ou chapa galvanizada.
  - Tensão: Mola de compressão (força de contato normal recomendada entre 15 N e 35 N).

---

## 3. Vantagens

A adoção do sistema SRMIE confere as seguintes vantagens tecnológicas à automação do observatório:

1. **Imunidade à Patinagem de Tração:** Como o sistema é independente da motorização, se a roda tratora da cúpula escorregar durante a partida, a leitura de azimute não sofrerá derivação (creep error).
2. **Alta Resolução Sensível:** O ganho mecânico ($\gamma = 30$) acoplado à decodificação em quadratura entrega resoluções sub-arcminuto de forma econômica, igualando ou superando fitas absolutas caríssimas.
3. **Absorção Contínua de Irregularidades:** O pantógrafo compensa ativamente distorções térmicas sazonais, assentamento do prédio e ovalizações do trilho de forma mecânica e fluida.
4. **Fácil Retromontagem (Retrofit):** Pode ser instalado em cúpulas antigas ou já existentes sem necessidade de modificação estrutural pesada na alvenaria ou no anel principal.

---

## 4. Desvantagens / Restrições

Como todo arranjo mecatrônico físico, existem limitações técnicas a serem consideradas no ciclo de vida:

1. **Desgaste Natural do Elastômero:** O atrito contínuo causa erosão microscópica do poliuretano. Um desgaste de apenas 0,5 mm no raio da roda gera uma alteração sistemática na razão cinemática $\gamma$, exigindo rotinas de recalibração em software a cada 1 a 2 anos.
2. **Suscetibilidade à Sujeira:** Acúmulo severo de graxa, pó ou detritos na pista de rolagem pode formar ressaltos que, mesmo pequenos, introduzem ruídos de alta frequência na contagem do encoder.
3. **Exigência de Calibração Mecânica Fina:** A mola de compressão possui uma "janela de tensão" estrita (15-35 N). O ajuste incorreto pode comprometer todo o sistema (se frouxa, perde pulsos; se apertada, esmaga a roda e distorce o cálculo instantâneo).
4. **Necessidade de Ciclo de *Homing*:** Por utilizar encoder incremental, a posição absoluta não é retida em quedas de energia. O sistema requer a passagem por um sensor magnético ou chave fim-de-curso para indexar o "Norte" sempre que reiniciado.

---

## 5. Produtos de Mercado e Custo (BOM)

A tabela abaixo ("*Bill of Materials*") apresenta os componentes recomendados de prateleira industrial, prezando por custo-benefício, e o orçamento estimado.

| Componente | Especificações Técnicas (Mercado) | Marca/Modelo Recomendado | Preço Estimado (R$) |
|:---|:---|:---|:---|
| **Encoder Incremental** | Eixo sólido Ø 8mm, 1024 PPR, IP65, saídas A/B/Z (Line Driver / Push-Pull 24V). | **Autonics** Série E50S8 <br> *ou* **Sick** DFS60 | R$ 600,00 - R$ 900,00 |
| **Roda de Medição** | Ø 100mm (r=50mm) de alta precisão. Revestimento em PU, dureza 75-85 Shore A. Furo central alinhado. | **Sick** (Série BEF-MR) <br> *ou* **Kuebler** | R$ 200,00 - R$ 380,00 |
| **Conj. Pantógrafo / Mola** | Estrutura usinada em alumínio/aço. Mola de compressão Inox (k ≈ 10 N/mm). Mancais radiais selados. | Usinagem / Fabricação Nacional (Peças padrão) | R$ 150,00 - R$ 250,00 |
| **Acoplamento Flexível** | Acoplamento tipo Oldham ou Fole elástico (amortecimento de microvibrações do eixo). | **Kalatec** / **KTR** | R$ 60,00 - R$ 110,00 |
| **Cabeamento** | Cabo de Instrumentação manga blindado (4 a 6 vias + malha de cobre estanhado). | Indústria Diversa | R$ 70,00 - R$ 120,00 |
| **TOTAL ESTIMADO** | Montagem de 1 conjunto completo. (Pode variar com frete/câmbio). | - | **R$ 1.080,00 a R$ 1.760,00** |

---

## 6. Detalhamento de Instalação

Para garantir que o equipamento físico replique com precisão as deduções teóricas do projeto mecânico, as diretrizes de campo abaixo são inegociáveis.

### 6.1 Integração Mecânica
* **Alinhamento Axial e Erro de Arraste:** O suporte primário que ancora o braço pantográfico no chassi da alvenaria deve estar milimetricamente nivelado. O eixo da roda de medição deve permanecer perfeitamente vertical (paralelo à face de rolagem do anel). Qualquer inclinação (ângulo de *camber*) provocará "arraste lateral" (*scrubbing*) durante o giro, esfarelando a banda de poliuretano rapidamente e induzindo torque torcional no eixo do encoder. Utilize inclinômetros digitais na base do suporte antes do torque final dos parafusos.
* **Calibração de Tensionamento da Mola:** O ajuste do batente da mola deve ser operado com extremo cuidado:
  * **Tensão Insuficiente (< 15 N):** A roda desliza (patina) sob variações dinâmicas ou poeira, causando omissão na contagem de pulsos.
  * **Tensão Excessiva (> 35 N):** Forças severas geram esmagamento elástico na banda de PU. Isso achata a roda localmente, **reduzindo o raio dinâmico real ($r$)** durante a operação para menos de 50 mm. A consequência imediata é um erro sistemático que distorce todo o mapeamento de 122.880 pulsos para 360°, inviabilizando a calibração do CLP. Utilize dinamômetro de tração/compressão portátil no braço para cravar a pré-carga nos 20-25 N nominais.

### 6.2 Integração Elétrica e Sinal
* **Imunidade a Ruído Eletromagnético (EMI):** Inversores de frequência acionando os motores de tração pesados da cúpula emitem ruídos (frequência de chaveamento PWM) letais para contadores de alta velocidade (HSC) de CLP.
  * O cabo do encoder **nunca** deve compartilhar a mesma calha, conduíte ou bandeja dos cabos de potência do motor. Mantenha no mínimo 20 cm de separação física ou cruzamentos em 90°.
  * Utilize estritamente cabo blindado em malha. **A blindagem deve ser aterrada em apenas uma das extremidades** (preferencialmente na malha de terra do painel do CLP) para evitar o fenômeno de "loops de terra" que podem atuar como antenas para ruídos indutivos.
* **Referenciamento de Fase (Canal Z):** Interligue obrigatoriamente as três saídas (A, B e Z) ao controlador. O sistema operará os canais A e B defasados em 90° elétricos para discernir sentido de giro (horário/anti-horário) via lógica de quadratura. Adicionalmente, utilize um micro-switch fixo ou sensor indutivo na estrutura estacionária acoplado à lógica da Fase Z (Index) para que a cúpula execute seu ciclo autônomo de *Homing* no *startup*, zerando a coordenada de Azimute com repetibilidade absoluta de 1 pulso.
