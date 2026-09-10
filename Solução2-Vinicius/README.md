# Análise Comparativa de Sistemas de Medição de Posição Azimutal para Cúpulas Astronômicas

* Captura Mecânica: Uma cremalheira circular fixada na base da cúpula engrena em um pinhão leitor fixo na estrutura imóvel do observatório. O engrenamento direto elimina o deslizamento (slip), garantindo repetibilidade contínua.
* Conversão Angular: O giro da cúpula aciona o pinhão, que transmite a rotação ao eixo de um encoder absoluto.
* Processamento: O encoder gera uma palavra digital contendo a posição angular exata. O controlador (PLC) lê essa contagem, aplica a relação mecânica de transmissão e calcula o azimute em tempo real ($0^\circ \text{ a } 360^\circ$).

# Aprofundamento Técnico e Detalhamento da Abordagem
---
## Acoplamento Direto com Encoder Absoluto Multivoltas

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
