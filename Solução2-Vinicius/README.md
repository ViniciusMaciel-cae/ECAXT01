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

---

**Conclusão:** Embora a Solução 1 ofereça simplicidade de programação, a **Solução 2 (Encoder Multivoltas acoplado ao pinhão leitor)** é o padrão de engenharia recomendado para observatórios astronômicos devido à extrema precisão angular proporcionada pela relação de transmissão mecânica e à eliminação de folgas mecânicas intermediárias.
