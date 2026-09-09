# Análise Comparativa de Sistemas de Medição de Posição Azimutal para Cúpulas Astronômicas

Captura Mecânica: Uma cremalheira circular fixada na base da cúpula engrena em um pinhão leitor fixo na estrutura imóvel do observatório. O engrenamento direto elimina o deslizamento (slip), garantindo repetibilidade contínua.
Conversão Angular: O giro da cúpula aciona o pinhão, que transmite a rotação ao eixo de um encoder absoluto.
Processamento: O encoder gera uma palavra digital contendo a posição angular exata. O controlador (PLC) lê essa contagem, aplica a relação mecânica de transmissão e calcula o azimute em tempo real ($0^\circ \text{ a } 360^\circ$).

# Aprofundamento Técnico e Detalhamento das Abordagens
Esta ideia geral pode ser implementada através de duas topologias distintas de acoplamento mecânico e codificação digital, cujos cálculos de resolução, relações de transmissão, equações e quadro comparativo estão documentados em detalhe no arquivo criado anteriormente:

---

## 1. Solução 1: Mapeamento de 1 Volta do Encoder para 1 Volta da Cúpula

Nesta arquitetura, utiliza-se um arranjo mecânico intermediário (caixa de engrenagens ou conjunto multiplicador/redutor) projetado para compensar exatamente a relação de transmissão da cremalheira e do pinhão. Dessa forma, uma rotação completa de 360° da cúpula astronômica resulta em exatamente uma rotação completa (360°) do eixo do encoder absoluto.

### 1.1. Configuração e Relações Mecânicas

Seja $R_{\text{crem}}$ a relação de transmissão entre a cremalheira circular da cúpula e o pinhão de acoplamento:

$$R_{\text{crem}} = \frac{Z_{\text{cremalheira}}}{Z_{\text{pinhão}}} = \frac{D_{\text{cúpula}}}{D_{\text{pinhão}}}$$

Onde:
* **$Z_{\text{cremalheira}}$ / $D_{\text{cúpula}}$:** Número total de dentes ou diâmetro primitivo da cúpula.
* **$Z_{\text{pinhão}}$ / $D_{\text{pinhão}}$:** Número de dentes ou diâmetro primitivo do pinhão leitor.

Para obter uma relação total de transmissão $R_{t} = 1$, instala-se uma caixa de engrenagens entre o pinhão e o encoder com relação inversa:

$$R_{\text{redutor}} = \frac{1}{R_{\text{crem}}} = \frac{Z_{\text{pinhão}}}{Z_{\text{cremalheira}}}$$

A relação total do sistema ($R_{t}$) é dada por:

$$R_{t} = R_{\text{crem}} \times R_{\text{redutor}} = \left( \frac{Z_{\text{cremalheira}}}{Z_{\text{pinhão}}} \right) \times \left( \frac{Z_{\text{pinhão}}}{Z_{\text{cremalheira}}} \right) = 1$$

### 1.2. Equações para Cálculo de Resolução

Com $R_{t} = 1$, a resolução angular na cúpula depende exclusivamente da resolução nativa do encoder *singleturn* de $B$ bits ($N_{\text{encoder}} = 2^B$ posições/volta):

$$\text{Contagens Totais por Volta da Cúpula} = N_{\text{encoder}} \times R_{t} = 2^B \times 1 = 2^B$$

A menor variação angular detectável na cúpula ($\theta_{\text{mínimo}}$ em graus) é dada por:

$$\theta_{\text{mínimo}} = \frac{360^\circ}{2^B}$$

### 1.3. Vantagens

* **Uso de Encoder Singleturn Simples:** Elimina a necessidade de hardware com contagem de voltas, utilizando apenas a faixa de $0^\circ$ a $360^\circ$ do sensor.
* **Simplificação de Firmware/PLC:** A leitura digital do encoder mapeia diretamente o azimute da cúpula ($0^\circ \text{ a } 360^\circ$) sem necessidade de gerenciar estouro de contagem (*rollover*) de voltas no software do controlador.

### 1.4. Desvantagens

* **Perda Severa de Resolução Angular:** A precisão digital fica limitada aos bits do encoder. Por exemplo, com um encoder de 12 bits ($4096$ posições), a precisão é de $0,0878^\circ$ ($\approx 5,26'$ de arco), o que exige encoders industriais de custo elevado ($18 \text{ a } 20 \text{ bits}$) para alcançar precisão arcsegundar.
* **Amplificação de Folga Mecânica (Backlash):** A adição da caixa de engrenagens intermediária insere folgas mecânicas que se somam diretamente à leitura do encoder, gerando zonas mortas no posicionamento.
* **Maior Complexidade e Manutenção Mecânica:** O conjunto adicional de engrenagens aumenta o atrito, o ponto de falha e a necessidade de lubrificação/alinhamento mecânico.

---

## 2. Solução 2: Acoplamento Direto com Encoder Absoluto Multivoltas

Nesta abordagem — amplamente adotada em observatórios profissionais —, o encoder é acoplado diretamente ao eixo do pinhão leitor que engrena na cremalheira da cúpula, aproveitando a multiplicação mecânica natural do sistema sem caixas de redução intermediárias adicionais.

### 2.1. Configuração e Relações Mecânicas

A relação de transmissão total ($R_{t}$) é determinada diretamente pelo acoplamento cremalheira/pinhão (e um eventual redutor industrial padronizado, caso utilizado):

$$R_{t} = R_{\text{crem}} \times R_{\text{redutor}} = \frac{Z_{\text{cremalheira}}}{Z_{\text{pinhão}}} \times R_{\text{redutor}}$$

Como $Z_{\text{cremalheira}} \gg Z_{\text{pinhão}}$, o eixo do encoder realiza $R_{t}$ voltas completas para cada 1 volta da cúpula ($R_{t} \gg 1$).

### 2.2. Equações para Cálculo de Resolução e Azimute

Utilizando um encoder absoluto **multivoltas** com $B_{\text{single}}$ bits por volta (resolução angular) e $B_{\text{multi}}$ bits para contagem de voltas:

$$\text{Contagens por Volta do Encoder} = 2^{B_{\text{single}}}$$

$$\text{Contagens Totais por Volta da Cúpula} = 2^{B_{\text{single}}} \times R_{t}$$

A menor variação angular detectável na cúpula ($\theta_{\text{mínimo}}$ em graus) passa a ser:

$$\theta_{\text{mínimo}} = \frac{360^\circ}{2^{B_{\text{single}}} \times R_{t}}$$

O cálculo do azimute real no controlador (PLC) é realizado aplicando a razão mecânica:

$$\text{Azimute (º)} = \left( \frac{\text{Contagem Atual do Encoder}}{\text{Resolução por Volta}} \times 360^\circ \right) \times \frac{1}{R_{t}}$$

### 2.3. Vantagens

* **Alta Resolução Angular (Amplificação Mecânica):** A própria relação da cremalheira multiplica a resolução do encoder por $R_{t}$. Um encoder de 12 bits com $R_{t} = 60$ atinge $245.760$ posições em $360^\circ$, resultando em uma precisão de $\approx 5,27''$ (segundos de arco).
* **Minimização de Folgas Mecânicas:** A ausência de caixas multiplicadoras intermediárias reduz drasticamente o *backlash* acumulado no sistema de leitura.
* **Simplicidade e Robustez Mecânica:** Menor quantidade de componentes móveis, reduzindo pontos de falha e manutenção.
* **Imunidade à Perda de Energia:** O encoder multivoltas retém a contagem exata de voltas e a posição angular do pinhão mesmo se desligado ou movimentado manualmente sem alimentação.

### 2.4. Desvantagens

* **Custo do Sensor:** Encoders absolutos multivoltas são mais caros do que modelos *singleturn* equivalentes.
* **Lógica de Controle no Software:** Requer tratamento no software do PLC para conversão da escala de voltas do pinhão para o azimute de $0^\circ \text{ a } 360^\circ$ da cúpula.

---

## 3. Quadro Comparativo das Soluções

| Parâmetro / Característica | Solução 1: Caixa 1:1 + Encoder Singleturn | Solução 2: Direto/Multivoltas + Pinhão Leitor |
| :--- | :--- | :--- |
| **Relação Mecânica Total ($R_t$)** | $1 : 1$ (1 volta encoder = 1 volta cúpula) | $R_t \gg 1$ (várias voltas por volta da cúpula) |
| **Tipo de Encoder Requerido** | Absoluto Singleturn | Absoluto Multivoltas |
| **Fonte da Resolução Angular** | Exclusivamente do encoder | Multiplicação mecânica ($N_{\text{encoder}} \times R_t$) |
| **Precisão Angular Típica (Ex. 12 bits)** | $\approx 0,0878^\circ \ (5,26')$ [Baixa] | $\approx 0,00146^\circ \ (5,27'')$ [Alta] |
| **Impacto do Backlash (Folga)** | Alto (amplificado pelas engrenagens) | Baixo (acoplamento direto no pinhão) |
| **Complexidade Mecânica** | Elevada (caixa de redução extra) | Baixa (acoplamento direto) |
| **Complexidade de Software/PLC** | Muito Baixa (direta $0-360^\circ$) | Média (mapeamento de voltas do pinhão) |
| **Recomendação de Aplicação** | Sistemas didáticos ou de baixa precisão | Observatórios astronômicos de alta precisão |
| **Custo**|Médio / Elevado: Embora o encoder singleturn seja mais barato individualmente, o custo mecânico global aumenta (caixa de engrenagens customizada, usinagem e manutenção contínua) | Baixo / Moderado: O sensor (multivoltas) tem um custo individual um pouco maior, mas o custo total do sistema é significativamente menor devido à extrema simplicidade mecânica |

---

**Conclusão:** Embora a Solução 1 ofereça simplicidade de programação, a **Solução 2 (Encoder Multivoltas acoplado ao pinhão leitor)** é o padrão de engenharia recomendado para observatórios astronômicos devido à extrema precisão angular proporcionada pela relação de transmissão mecânica e à eliminação de folgas mecânicas intermediárias.

---

## 4. Especificações Técnicas do Sistema

### 4.1. Parâmetros Mecânicos da Cúpula

| Parâmetro | Valor Típico | Unidade |
| :--- | :---: | :---: |
| Diâmetro da cúpula astronômica | 2,0 – 10,0 | m |
| Diâmetro primitivo da cremalheira circular | Igual ao da cúpula | m |
| Módulo da cremalheira | 2 – 6 | mm |
| Número de dentes da cremalheira ($Z_{\text{cremalheira}}$) | 400 – 5000 | dentes |
| Diâmetro do pinhão leitor | 20 – 80 | mm |
| Número de dentes do pinhão ($Z_{\text{pinhão}}$) | 10 – 40 | dentes |
| Relação de transmissão natural ($R_{\text{crem}}$) | 30 – 300 | adimensional |

### 4.2. Especificações do Encoder — Solução 1 (Singleturn)

| Parâmetro | Especificação Mínima | Especificação Recomendada |
| :--- | :---: | :---: |
| Tipo | Absoluto Singleturn | Absoluto Singleturn |
| Resolução (bits) | 12 bits (4 096 pos.) | 18 – 20 bits (262 144 – 1 048 576 pos.) |
| Interface de comunicação | SSI / RS-422 | SSI / BiSS-C / EtherCAT |
| Precisão angular no encoder | ≤ ±0,088° | ≤ ±0,001° |
| Tensão de alimentação | 10 – 30 V CC | 24 V CC |
| Proteção IP | IP54 | IP65 / IP67 |
| Temperatura de operação | −10 °C a +70 °C | −40 °C a +85 °C |
| Eixo / Flange | 6 mm / B10 | 10 mm / B10 ou cego |

### 4.3. Especificações do Encoder — Solução 2 (Multivoltas)

| Parâmetro | Especificação Mínima | Especificação Recomendada |
| :--- | :---: | :---: |
| Tipo | Absoluto Multivoltas | Absoluto Multivoltas |
| Resolução singleturn (bits) | 12 bits (4 096 pos./volta) | 16 – 17 bits (65 536 – 131 072 pos./volta) |
| Capacidade multivoltas (bits) | 12 bits (4 096 voltas) | 16 bits (65 536 voltas) |
| Interface de comunicação | SSI / Profibus | SSI / BiSS-C / EtherCAT / PROFINET |
| Resolução efetiva na cúpula (ex. $R_t = 60$, 12 bits) | ≈ 5,27″ | < 0,1″ |
| Tensão de alimentação | 10 – 30 V CC | 24 V CC |
| Proteção IP | IP64 | IP67 |
| Temperatura de operação | −10 °C a +70 °C | −40 °C a +85 °C |
| Retenção de posição sem energia | Sim (Wiegand / bateria) | Sim (Wiegand sem bateria) |

### 4.4. Especificações do Controlador (PLC/Microcontrolador)

| Parâmetro | Requisito |
| :--- | :--- |
| Entradas digitais SSI | Mínimo 1 canal SSI de alta resolução |
| Frequência de aquisição | ≥ 100 Hz (para rastreamento em tempo real) |
| Resolução da variável de processo | 32 bits inteiros com sinal |
| Comunicação superior | Ethernet / ModBus TCP / PROFINET / ASCOM |
| Saída de controle | PWM / Analógico (para motor de rotação da cúpula) |

---

## 5. Detalhamento de Instalação

### 5.1. Visão Geral do Processo de Instalação

```
┌─────────────────────────────────────────────────────────────────────────┐
│  FLUXO DE INSTALAÇÃO DO SISTEMA DE MEDIÇÃO AZIMUTAL                    │
│                                                                          │
│  [1] Preparação    → [2] Montagem     → [3] Cabeamento  → [4] Config.  │
│      Mecânica           do Encoder         e Proteção        e Teste    │
└─────────────────────────────────────────────────────────────────────────┘
```

### 5.2. Etapa 1 — Preparação Mecânica

1. **Verificação da cremalheira circular:** Inspecionar desgaste, módulo e concentricidade. Substituir segmentos desgastados antes da instalação do encoder.
2. **Seleção e usinagem do pinhão leitor:** Fabricar ou selecionar o pinhão com módulo compatível com a cremalheira. O diâmetro deve ser calculado para obter a relação $R_{\text{crem}}$ desejada.
3. **Fabricação do suporte (bracket):** Confeccionar em aço inox ou alumínio anodizado um suporte rígido para o encoder e o pinhão, fixado à estrutura imóvel do observatório. O suporte deve permitir ajuste de folga (backlash) pelo deslocamento radial do conjunto.
4. **Verificação de folga de engrenamento:** Ajustar a distância entre centros pinhão/cremalheira para obter folga mínima sem travamento. Folga recomendada: **0,1 × módulo**.

### 5.3. Etapa 2 — Montagem do Encoder

1. **Acoplamento ao eixo do pinhão (Solução 2):** Utilizar acoplamento flexível de fole metálico ou disco flexível para compensar desalinhamentos angulares (≤ 1°) e axiais (≤ 0,5 mm), preservando a precisão e evitando cargas radiais excessivas no eixo do encoder.
2. **Acoplamento com caixa de redução (Solução 1):** Montar a caixa de redução entre o pinhão e o encoder, garantindo alinhamento coaxial com tolerância de concentricidade ≤ 0,05 mm. Verificar relação de transmissão resultante após montagem.
3. **Fixação e torque:** Apertar os parafusos de fixação com torque definido pelo fabricante. Utilizar trava de rosca (Loctite 243 ou equivalente) em ambientes sujeitos a vibração.
4. **Verificação de rotação livre:** Rotacionar manualmente a cúpula 360° e confirmar ausência de interferências mecânicas, arrastar e pontos de travamento.

### 5.4. Etapa 3 — Cabeamento e Proteção

| Item | Especificação |
| :--- | :--- |
| Cabo de sinal (SSI/BiSS-C) | Blindado, par trançado, AWG 24, comprimento máx. 100 m |
| Cabo de alimentação | Mínimo 2 × AWG 22, com fusível de 0,5 A no positivo |
| Aterramento da blindagem | Apenas no lado do controlador (aterramento em um ponto) |
| Proteção do encoder | Capa de borracha ou caixa plástica IP67 adicional em ambientes externos |
| Raio mínimo de curvatura | 10 × diâmetro externo do cabo |
| Passa-cabos | Prensa-cabo PG11 com vedação para IP65 |

> **Atenção:** Manter os cabos de sinal do encoder separados (distância mínima de 20 cm) de cabos de alimentação de motores para evitar interferências eletromagnéticas (EMI).

### 5.5. Etapa 4 — Configuração e Comissionamento

1. **Definição do ponto zero (referência):** Posicionar a cúpula com a abertura voltada para o Norte verdadeiro (azimute = 0° / 360°). Registrar a leitura bruta do encoder nesta posição como **offset de referência** no PLC.
2. **Programação da relação mecânica:** Inserir no software do controlador o valor de $R_t$ calculado e o offset de referência.
3. **Validação da leitura de azimute:**
   - Mover a cúpula para azimutes conhecidos (90°, 180°, 270°) com auxílio de bússola ou clinômetro digital.
   - Comparar a leitura do sistema com o valor esperado.
   - Erro máximo aceitável: ±0,1° para usos amadores, ±0,01° para sistemas profissionais.
4. **Teste de repetibilidade:** Executar 10 ciclos completos de rotação e retornar ao ponto zero; o desvio entre leituras deve ser inferior à resolução teórica calculada.
5. **Integração com software astronômico:** Configurar a interface ASCOM / INDI conforme o protocolo utilizado (ModBus, Serial, Ethernet) para comunicação com o software de controle da montagem (Stellarium, Cartes du Ciel, TheSkyX, etc.).

---

## 6. Vantagens e Desvantagens / Restrições por Solução

### 6.1. Solução 1 — Caixa Redutora 1:1 + Encoder Singleturn

#### ✅ Vantagens

* **Programação extremamente simples:** Leitura direta de 0° a 360°, sem conversões matemáticas no PLC.
* **Sensor de menor custo unitário:** Encoders singleturn são universalmente disponíveis e mais baratos que modelos multivoltas.
* **Compatibilidade com controladores legados:** Não exige suporte a multivoltas no hardware de leitura.
* **Referenciamento imediato ao ligar:** Não há ambiguidade de volta — a posição absoluta é inequívoca em qualquer posição da cúpula.

#### ❌ Desvantagens e Restrições

* **Precisão angular severamente limitada:** Para alcançar ≤ 0,01°, são necessários encoders de ≥ 15 bits, com custo equivalente ou superior ao multivoltas.
* **Folga mecânica amplificada:** Cada engrenagem adicional na caixa redutora contribui com folga acumulada, gerando zonas mortas de posicionamento.
* **Custo total elevado:** Caixa de redução customizada (relação não-padrão), usinagem de pinhões especiais e manutenção periódica encarecem o projeto.
* **Restrição de velocidade:** Caixas de redução customizadas podem ter limitações de velocidade máxima de operação.
* **Restrição ambiental:** Caixas de engrenagens expostas a ambientes úmidos (cúpulas abertas) exigem vedação especial e lubrificação frequente.

### 6.2. Solução 2 — Acoplamento Direto com Encoder Absoluto Multivoltas

#### ✅ Vantagens

* **Alta resolução com sensor econômico:** A multiplicação mecânica natural do pinhão amplifica a resolução, permitindo usar encoders de menor resolução nativa para atingir precisão elevada.
* **Menor backlash acumulado:** Sem engrenagens intermediárias, as únicas folgas são as do par cremalheira/pinhão, controladas diretamente.
* **Robustez mecânica:** Poucos componentes móveis, menor probabilidade de falha e manutenção reduzida.
* **Retenção de posição sem energia:** A tecnologia Wiegand utilizada em encoders multivoltas premium mantém a contagem de voltas mesmo sem alimentação, sem necessidade de bateria.
* **Padrão industrial reconhecido:** Amplamente documentado e suportado por fabricantes de instrumentação e automação.

#### ❌ Desvantagens e Restrições

* **Custo unitário do encoder:** Encoders absolutos multivoltas custam tipicamente 2× a 4× o valor de um modelo singleturn equivalente.
* **Lógica de conversão no software:** O firmware/PLC deve implementar a conversão da leitura bruta de voltas/posição para azimute em graus, gerenciando corretamente o rollover de 360°.
* **Limitação de voltas:** O número máximo de voltas rastreadas é limitado pela capacidade multivoltas (ex.: 4 096 voltas para 12 bits). Para $R_t > 4096$, o encoder não consegue cobrir a totalidade de uma volta da cúpula — cenário improvável na prática.
* **Restrição de protocolo:** Não todos os PLCs de baixo custo suportam nativamente protocolos de alta velocidade como BiSS-C ou EtherCAT; pode ser necessário adaptador.

---

## 7. Produtos de Mercado e Análise de Custo

### 7.1. Encoders Absolutos Singleturn — Produtos de Mercado

| Fabricante | Modelo | Resolução | Interface | Proteção | Preço Estimado (USD) |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Heidenhain** | ECN 1313 | 23 bits | EnDat 2.2 | IP64 | \$800 – \$1 200 |
| **Sick** | AFS60A-S4AK065536 | 16 bits | SSI | IP67 | \$350 – \$550 |
| **Baumer** | BMMV 08I-1-R-360-8-B3 | 13 bits | SSI | IP67 | \$200 – \$350 |
| **Kübler** | 8.5820.1331.1024 | 10 bits | SSI | IP65 | \$150 – \$250 |
| **Omron** | E6C2-AG5C 360P/R | 9 bits | SSI | IP50 | \$80 – \$150 |

### 7.2. Encoders Absolutos Multivoltas — Produtos de Mercado

| Fabricante | Modelo | Resolução Singleturn | Capacidade Multivoltas | Interface | Proteção | Preço Estimado (USD) |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **Heidenhain** | EQN 1325 | 25 bits | 12 bits (4 096 v.) | EnDat 2.2 | IP64 | \$1 200 – \$2 000 |
| **Sick** | AFM60A-S4AK262144 | 18 bits | 12 bits | SSI / BiSS-C | IP67 | \$500 – \$800 |
| **Baumer** | GXMMW.B10P3614 | 17 bits | 16 bits | SSI | IP67 | \$400 – \$700 |
| **Kübler** | 8.5868.1331.G131 | 13 bits | 12 bits | SSI / Profibus | IP67 | \$300 – \$500 |
| **Pepperl+Fuchs** | WCS3B-LS311S | 13 bits | 16 bits | SSI | IP65 | \$350 – \$600 |
| **Posital (FRABA)** | OCD-S5B1B-1416-S100-PRM | 16 bits | 14 bits | SSI | IP67 | \$250 – \$450 |

> **Nota:** Preços estimados com base em cotações de mercado (2024–2025). Valores reais variam conforme quantidade, distribuidor, câmbio e tarifas de importação.

### 7.3. Componentes Mecânicos Auxiliares — Produtos de Mercado

| Componente | Fabricante / Modelo | Aplicação | Preço Estimado (USD) |
| :--- | :--- | :--- | :---: |
| Acoplamento flexível de fole | **Rexnord** / Jaw Coupling | Conexão encoder-pinhão (Sol. 2) | \$30 – \$80 |
| Caixa de redução planetária | **Neugart** PLE 040 | Redução 1:N customizada (Sol. 1) | \$300 – \$800 |
| Pinhão de aço inox (M3, Z20) | Usinagem local / KHK Gears | Pinhão leitor | \$20 – \$60 |
| Suporte/Bracket em alumínio | Usinagem local | Fixação do encoder na estrutura | \$50 – \$150 |
| Cabo blindado SSI (por metro) | **Lapp** / ÖLFLEX® SERVO | Cabeamento de sinal | \$3 – \$8 /m |
| Conector M12 (8 pinos) | **Phoenix Contact** / **Binder** | Conexão encoder | \$8 – \$20 |

### 7.4. Estimativa de Custo Total do Sistema

| Componente | Solução 1 (Singleturn + Caixa) | Solução 2 (Multivoltas Direto) |
| :--- | :---: | :---: |
| Encoder | \$150 – \$500 | \$300 – \$900 |
| Caixa de redução customizada | \$300 – \$800 | — |
| Pinhão leitor e acoplamento | \$50 – \$150 | \$50 – \$130 |
| Suporte mecânico e usinagem | \$100 – \$250 | \$60 – \$150 |
| Cabeamento e conectores | \$50 – \$120 | \$50 – \$120 |
| PLC / controlador compatível | \$200 – \$500 | \$200 – \$500 |
| Mão de obra de instalação | \$150 – \$400 | \$100 – \$250 |
| **Total Estimado** | **\$1 000 – \$2 720** | **\$760 – \$2 050** |
| **Custo de Manutenção Anual** | \$150 – \$300 | \$30 – \$80 |

> **Análise de Custo Total de Propriedade (TCO):** Considerando um ciclo de vida de 10 anos, a Solução 2 representa uma economia estimada de **30% a 50%** em relação à Solução 1, mesmo com o maior custo inicial do encoder multivoltas, devido à eliminação da caixa de redução e à redução drástica dos custos de manutenção mecânica.

---

**Conclusão Geral:** Para implementações em observatórios astronômicos, a **Solução 2** — encoder absoluto multivoltas acoplado diretamente ao pinhão leitor — é superior em todos os critérios técnicos relevantes (precisão, robustez, backlash, manutenção) e apresenta custo total de propriedade inferior ao longo da vida útil do sistema. A Solução 1 permanece adequada para contextos didáticos, protótipos de baixo orçamento ou aplicações onde a simplicidade de software supera as limitações de precisão.

