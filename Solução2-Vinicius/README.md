# Sistema de Medição Angular para Cúpula de Telescópio
## Encoder absoluto multivoltas com pinhão leitor, cremalheira e pré-carga por mola

> **Base do documento:** solução apresentada na imagem fornecida e informações do `README.md` do projeto.  
> **Observação:** os preços de mercado são referências encontradas em setembro de 2026 e podem variar conforme fornecedor, configuração, impostos, câmbio e disponibilidade.

---

## 1. Visão geral da solução

A solução proposta realiza a medição do **azimute da cúpula astronômica** por meio de uma transmissão mecânica entre uma **cremalheira circular fixada à cúpula** e um **pinhão leitor conectado a um encoder absoluto multivoltas**.

O princípio de funcionamento é:

1. A cúpula gira em torno do seu eixo vertical.
2. A cremalheira acompanha a rotação da cúpula.
3. A cremalheira movimenta o pinhão por engrenamento direto.
4. O pinhão gira o eixo do encoder.
5. O encoder fornece a posição angular absoluta do seu eixo.
6. O PLC converte a posição do eixo do encoder para o azimute real da cúpula, considerando a relação mecânica entre a cremalheira e o pinhão.

A arquitetura evita a necessidade de instalar um encoder diretamente no grande eixo de rotação da cúpula. Em vez disso, utiliza a **multiplicação mecânica natural** proporcionada pela diferença entre o diâmetro efetivo da cremalheira e o diâmetro do pinhão.

O README do projeto descreve justamente essa abordagem como uma captura mecânica por cremalheira e pinhão, seguida da conversão do movimento para o encoder e do processamento da posição pelo PLC. fileciteturn0file0L1-L5

---

## 2. Arquitetura mecânica

A solução ilustrada é composta principalmente pelos seguintes elementos:

- **Cremalheira circular:** instalada na estrutura rotativa da cúpula.
- **Pinhão leitor:** engrenado diretamente na cremalheira.
- **Eixo do pinhão:** transmite a rotação ao encoder.
- **Acoplamento de junta:** conecta mecanicamente o eixo do pinhão ao eixo do encoder.
- **Encoder absoluto multivoltas:** realiza a leitura da posição.
- **Bloco de guia linear:** permite o deslocamento controlado do conjunto sensor/pinhão.
- **Mecanismo de mola:** aplica pré-carga sobre o conjunto para manter o pinhão pressionado contra a cremalheira.
- **Base:** estrutura fixa responsável pela sustentação do conjunto.

### 2.1. Função do mecanismo de mola

O mecanismo de mola mostrado na solução possui uma função importante: **manter o pinhão permanentemente engrenado com a cremalheira**.

Como a cúpula é uma estrutura de grande dimensão, pequenas variações geométricas, desalinhamentos, vibrações ou dilatações podem alterar ligeiramente a distância entre a cremalheira e o conjunto de leitura.

A mola permite que o conjunto se mova no guia linear, mantendo a força de contato entre os dentes.

Isso ajuda a:

- evitar perda de contato entre pinhão e cremalheira;
- reduzir erros provocados por pequenas variações de distância;
- compensar parcialmente desalinhamentos;
- reduzir impactos durante pequenas irregularidades;
- manter a transmissão mecânica durante toda a rotação.

A força da mola deve ser suficiente para garantir o engrenamento, mas não excessiva, pois uma força muito elevada aumenta atrito, desgaste e esforço sobre os rolamentos e dentes.

---

# 3. Funcionamento do sistema

## 3.1. Captura mecânica

A cremalheira é instalada ao redor da cúpula.

Quando a cúpula gira, seus dentes percorrem o pinhão. Como existe engrenamento entre os dois elementos, o movimento angular da cúpula é convertido em rotação do pinhão.

Esse método apresenta uma vantagem importante em relação a uma medição puramente baseada em atrito: **o movimento é transmitido por contato positivo entre dentes**, reduzindo a possibilidade de escorregamento.

O README destaca justamente que o engrenamento direto elimina o *slip* e fornece repetibilidade contínua. fileciteturn0file0L3-L5

---

## 3.2. Transmissão para o encoder

O pinhão é conectado ao encoder por meio de um acoplamento.

O acoplamento deve:

- transmitir o torque necessário;
- compensar pequenos desalinhamentos;
- evitar transmitir cargas radiais excessivas para o eixo do encoder;
- possuir baixa folga torsional;
- ser compatível com os diâmetros dos eixos.

O conjunto deve ser montado de modo que o pinhão seja sustentado por seus próprios rolamentos/eixo. O encoder deve atuar principalmente como **elemento de medição**, e não como suporte estrutural do pinhão.

---

# 4. Relação mecânica

A relação entre o número de dentes da cremalheira e o número de dentes do pinhão determina quantas voltas o encoder realiza durante uma volta completa da cúpula.

De acordo com o modelo apresentado no README:

$$
R_t =
R_{crem} \times R_{redutor}
$$

Para o sistema sem redutor adicional:

$$
R_t =
\frac{Z_{cremalheira}}
{Z_{pinhão}}
$$

onde:

- $R_t$ = relação de transmissão total;
- $Z_{cremalheira}$ = número total de dentes equivalentes da cremalheira circular;
- $Z_{pinhão}$ = número de dentes do pinhão;
- $R_{redutor}$ = relação de um eventual redutor adicional.

Como a cremalheira possui um número de dentes muito maior que o pinhão, o encoder realiza várias voltas durante uma única volta completa da cúpula. fileciteturn0file0L13-L19

### Exemplo

Considerando:

- cremalheira equivalente: 1200 dentes;
- pinhão: 20 dentes;

temos:

$$
R_t = \frac{1200}{20}=60
$$

Portanto:

> **1 volta da cúpula → 60 voltas do encoder.**

Essa relação é particularmente interessante para a medição porque aumenta a resolução angular efetiva da cúpula.

---

# 5. Resolução angular

Considerando um encoder com resolução de $B_{single}$ bits por revolução:

$$
N_{encoder}=2^{B_{single}}
$$

Com um encoder de 12 bits:

$$
N_{encoder}=2^{12}=4096
$$

Se a relação mecânica for:

$$
R_t=60
$$

então:

$$
N_{cúpula}=4096 \times 60
$$

$$
N_{cúpula}=245760
$$

posições teóricas por volta da cúpula.

A resolução angular teórica será:

$$
\theta_{min}=
\frac{360^\circ}
{2^{B_{single}}\times R_t}
$$

Para o exemplo:

$$
\theta_{min}=
\frac{360^\circ}{245760}
\approx0,0014648^\circ
$$

Convertendo para segundos de arco:

$$
0,0014648^\circ \times 3600
\approx5,27''
$$

Portanto, a solução pode alcançar **aproximadamente 5,27 segundos de arco de resolução teórica**, considerando um encoder de 12 bits e relação mecânica $R_t=60$.

O README apresenta o mesmo exemplo de 12 bits e $R_t=60$, chegando a 245.760 posições e aproximadamente 5,27". fileciteturn0file0L21-L35

> **Importante:** 5,27" é a **resolução teórica do sistema**, não necessariamente a precisão absoluta final. A precisão real dependerá também da precisão do encoder, erro de passo da cremalheira, excentricidade, folga, desalinhamento, deformações estruturais e calibração.

---

# 6. Encoder absoluto multivoltas

A escolha por um **encoder absoluto multivoltas** é adequada porque o sistema pode exigir várias revoluções do eixo do encoder para representar uma única volta da cúpula.

Encoders absolutos fornecem um código correspondente à posição do eixo, enquanto modelos multivoltas também registram a quantidade de revoluções realizadas.

Fabricantes industriais como Baumer, Hengstler, Dynapar e ifm disponibilizam encoders absolutos multivoltas com diferentes interfaces industriais.

A Baumer, por exemplo, possui modelos com 13 bits de resolução singleturn e 12 bits multiturn, com interfaces como SSI e BiSS C. Um modelo da família EN580E.ML especifica 8192 posições por volta, 4096 revoluções e precisão absoluta de ±0,03°. 

A Hengstler AC58 possui versões singleturn/multiturn e interfaces SSI, BiSS, EtherCAT, CANopen, Profibus e outras, chegando a configurações de até 34 bits dependendo da interface. 

A ifm também destaca o uso de encoders absolutos multivoltas para medição angular e disponibiliza versões com diferentes barramentos industriais. 

---

# 7. Processamento pelo PLC

O PLC recebe a posição do encoder e realiza a conversão para o sistema de coordenadas da cúpula.

Uma forma simplificada de cálculo é:

$$
\theta_{encoder}=
\frac{N_{atual}}
{N_{volta}}
\times360^\circ
$$

Depois:

$$
Azimute=
\frac{\theta_{encoder}}
{R_t}
$$

Para evitar valores acima de 360°, o software deve aplicar uma operação módulo:

$$
Azimute =
\left(
\frac{N_{atual}}
{N_{volta}\times R_t}
\times360
\right)
\bmod360
$$

O README descreve esse processo como a leitura da palavra digital do encoder pelo PLC, aplicação da relação mecânica e cálculo do azimute entre 0° e 360°. fileciteturn0file0L3-L5

---

# 8. Referenciamento e calibração

Mesmo utilizando um encoder absoluto, recomenda-se estabelecer um procedimento de **referência mecânica do azimute**.

Uma estratégia possível é utilizar:

- sensor de referência;
- sensor indutivo;
- fim de curso;
- marca mecânica;
- posição astronômica conhecida;
- ou uma combinação dessas estratégias.

O procedimento pode ser:

1. Definir uma posição física conhecida da cúpula como 0°.
2. Posicionar a cúpula nessa referência.
3. Ler o valor do encoder.
4. Armazenar o *offset* correspondente.
5. Aplicar o *offset* em todos os cálculos posteriores.

Assim:

$$
Azimute =
\left[
\frac{N_{encoder}-N_{offset}}
{N_{volta}\times R_t}
\times360
\right]\bmod360
$$

Isso permite compensar a posição física de instalação do encoder.

---

# 9. Vantagens

## 9.1. Alta resolução angular

A transmissão mecânica aumenta o número de posições detectáveis na cúpula.

O README identifica a amplificação mecânica da resolução como uma das principais vantagens da solução. fileciteturn0file0L37-L41

---

## 9.2. Baixa possibilidade de escorregamento

Como a transmissão utiliza dentes, o movimento não depende de atrito entre uma roda e a superfície.

Isso é especialmente interessante para uma cúpula de telescópio, que pode apresentar grandes dimensões e condições ambientais variáveis.

---

## 9.3. Baixa complexidade mecânica

A arquitetura não necessita necessariamente de:

- redutor de alta precisão;
- encoder de grande diâmetro instalado diretamente no eixo da cúpula;
- sistema óptico de leitura da posição;
- correia longa para transmissão do movimento.

O conjunto fica relativamente compacto.

---

## 9.4. Redução do impacto de folgas intermediárias

Como o encoder é instalado diretamente no pinhão leitor, não há uma cadeia de vários redutores entre o elemento que acompanha a cúpula e o sensor.

O README destaca a redução do *backlash* acumulado como uma vantagem da configuração direta. fileciteturn0file0L39-L41

---

## 9.5. Manutenção relativamente simples

Os principais componentes mecânicos são comerciais:

- engrenagens;
- cremalheiras;
- guias lineares;
- molas;
- acoplamentos;
- rolamentos;
- encoder industrial.

Isso facilita substituição e manutenção.

---

## 9.6. Possibilidade de integração industrial

Dependendo do encoder escolhido, o sistema pode ser integrado a:

- PLC;
- CANopen;
- Profibus;
- PROFINET;
- EtherCAT;
- SSI;
- BiSS;
- IO-Link;
- outras interfaces industriais.

---

# 10. Desvantagens e restrições

## 10.1. Dependência da qualidade mecânica da cremalheira

A resolução calculada não garante automaticamente a mesma precisão angular.

Erros no passo dos dentes, excentricidade da cremalheira, deformações e desalinhamento podem gerar erro na medição.

---

## 10.2. Desgaste mecânico

O pinhão e a cremalheira são elementos de contato mecânico.

Com o tempo pode ocorrer:

- desgaste dos dentes;
- aumento de folga;
- contaminação;
- corrosão;
- perda de precisão;
- alteração do engrenamento.

É necessário prever inspeção e manutenção.

---

## 10.3. Necessidade de pré-carga

A mola é importante para manter o contato, mas precisa ser dimensionada corretamente.

Uma mola excessivamente rígida pode:

- aumentar o atrito;
- aumentar o desgaste;
- gerar esforços elevados;
- aumentar a carga nos rolamentos.

Uma mola muito fraca pode permitir perda de engrenamento.

---

## 10.4. Sensibilidade ao desalinhamento

O conjunto deve ser instalado com cuidado.

Desalinhamentos podem gerar:

- cargas radiais no eixo do encoder;
- vibração;
- desgaste;
- variação da relação instantânea;
- erros de medição.

---

## 10.5. Custo do encoder

Encoders absolutos multivoltas industriais são significativamente mais caros que encoders incrementais simples.

O README também aponta o custo superior do encoder absoluto multivoltas como uma desvantagem. fileciteturn0file0L43-L47

---

## 10.6. Complexidade do software

O PLC precisa converter:

**posição do encoder → posição do pinhão → posição angular da cúpula.**

Também devem ser tratados:

- *offset* de referência;
- sentido de rotação;
- transição 359° → 0°;
- posição absoluta;
- limites de operação;
- diagnóstico;
- falhas de comunicação.

O README classifica essa parte como uma complexidade média de software/PLC. fileciteturn0file0L51-L62

---

# 11. Detalhamento da instalação

## 11.1. Instalação da cremalheira

A cremalheira deve ser instalada concentricamente ao eixo de rotação da cúpula.

Recomenda-se:

1. Dividir a cremalheira em segmentos.
2. Posicionar os segmentos ao longo do perímetro.
3. Garantir continuidade do passo dos dentes entre segmentos.
4. Verificar a concentricidade em relação ao eixo da cúpula.
5. Verificar a altura e orientação dos dentes.
6. Fixar rigidamente os segmentos.
7. Fazer inspeção completa do engrenamento.

Para aplicações de maior precisão, é preferível utilizar cremalheira industrial de qualidade conhecida e controlar cuidadosamente as emendas entre os segmentos.

---

## 11.2. Instalação do conjunto de leitura

A base fixa deve ser instalada na estrutura imóvel do observatório.

Sobre ela são instalados:

- guia linear;
- bloco móvel;
- suporte do eixo;
- pinhão;
- encoder;
- mecanismo de mola.

O conjunto deve permitir um pequeno deslocamento radial do pinhão em relação à cremalheira.

---

## 11.3. Montagem do pinhão

O pinhão deve ser montado em eixo próprio com rolamentos adequados.

A sequência recomendada é:

**pinhão → eixo → acoplamento → encoder**

O encoder não deve receber diretamente os esforços radiais ou axiais provenientes do engrenamento.

---

## 11.4. Montagem da mola

A mola deve atuar no sentido de pressionar o conjunto do pinhão contra a cremalheira.

A pré-carga deve ser ajustável.

Uma solução prática é utilizar:

- parafuso de ajuste;
- suporte da mola;
- mola helicoidal de compressão;
- batente mecânico.

Isso permite regular o contato após a instalação.

---

## 11.5. Instalação elétrica

O encoder deve ser alimentado conforme sua especificação.

Em aplicações industriais, uma alimentação de 24 Vcc é comum, mas deve-se utilizar exatamente a tensão indicada pelo modelo escolhido.

O cabo deve ser:

- protegido mecanicamente;
- separado de cabos de potência quando possível;
- adequadamente aterrado/blindado;
- dimensionado para o protocolo utilizado.

A interface do encoder deve ser compatível com o PLC.

---

# 12. Dimensionamento preliminar

Antes da fabricação, devem ser definidos:

| Parâmetro | Símbolo | Necessário |
|---|---:|---|
| Diâmetro da cremalheira | $D_c$ | Sim |
| Número de dentes da cremalheira | $Z_c$ | Sim |
| Número de dentes do pinhão | $Z_p$ | Sim |
| Módulo | $m$ | Sim |
| Resolução singleturn | $B_s$ | Sim |
| Número de voltas multiturn | $B_m$ | Sim |
| Precisão do encoder | — | Sim |
| Folga do conjunto | — | Sim |
| Força da mola | $F_m$ | Sim |
| Massa do conjunto móvel | $m_s$ | Sim |
| Velocidade máxima da cúpula | $\omega_c$ | Sim |
| Temperatura de operação | — | Sim |
| Grau de proteção necessário | IP | Sim |

---

# 13. Exemplo de dimensionamento mecânico

Considere:

- módulo $m=2$;
- pinhão com 20 dentes;
- cremalheira equivalente a 1200 dentes.

O diâmetro primitivo do pinhão é:

$$
d_p=mZ_p
$$

$$
d_p=2\times20=40\,mm
$$

A relação mecânica é:

$$
R_t=\frac{1200}{20}=60
$$

Portanto:

$$
1\text{ volta da cúpula}=60\text{ voltas do pinhão}
$$

Com encoder de 12 bits:

$$
4096\times60=245760
$$

posições teóricas por volta.

---

# 14. Produtos comerciais compatíveis

A seleção final deve ser feita de acordo com:

- resolução;
- precisão;
- número de voltas;
- interface;
- diâmetro do eixo;
- velocidade máxima;
- grau de proteção;
- temperatura;
- disponibilidade de integração com o PLC.

## 14.1. Baumer EN580E.ML

A família Baumer EN580E.ML oferece encoders absolutos multivoltas com:

- 13 bits singleturn;
- 12 bits multiturn;
- até 4096 revoluções;
- interfaces SSI/BiSS, dependendo da versão;
- opções de eixo sólido e eixo vazado.

Um modelo EN580E.ML consultado especifica 8192 passos por revolução, 4096 revoluções e precisão absoluta de ±0,03°.

**Indicação:** opção industrial de alta qualidade para uma solução de medição angular.

Fonte: Baumer Brasil.

---

## 14.2. Hengstler AC58

O AC58 é uma família de encoders absolutos industriais com versões:

- singleturn;
- multiturn;
- eixo sólido;
- eixo vazado.

Dependendo da configuração, pode chegar a:

- 22 bits singleturn;
- 12 bits multiturn;
- até 34 bits combinados.

Há versões com:

- SSI;
- BiSS;
- CANopen;
- Profibus;
- EtherCAT;
- DeviceNet;
- outras interfaces.

**Indicação:** interessante quando há necessidade de flexibilidade de comunicação com o sistema de automação.

---

## 14.3. Dynapar AI25

A família AI25 possui versões absolutas com:

- até 14 bits singleturn;
- 12 bits multiturn;
- CANopen ou DeviceNet;
- alimentação de 10–30 Vcc;
- versões com IP67;
- opções de eixo e hubshaft.

**Indicação:** boa alternativa quando o sistema de controle utiliza CANopen/DeviceNet.

---

## 14.4. ifm

A ifm disponibiliza encoders absolutos multivoltas em diferentes famílias e interfaces industriais.

Há opções:

- eixo maciço;
- eixo vazado;
- IO-Link;
- CANopen;
- Profibus;
- Profinet;
- EtherCAT;
- Ethernet/IP, conforme modelo.

**Indicação:** especialmente interessante quando o projeto já utiliza infraestrutura industrial baseada em ifm/IO-Link.

---

# 15. Referências de preços de mercado

Os preços abaixo são **referências**, não valores definitivos de projeto.

| Componente | Exemplo | Preço observado |
|---|---|---:|
| Encoder absoluto multivoltas | ifm RM8002 | ~R$ 3.133 |
| Encoder absoluto multivoltas | ifm RM8004 | ~R$ 3.174 |
| Encoder absoluto multivoltas | ifm RM7013 | ~R$ 8.328 |
| Encoder absoluto multivoltas | Hengstler AX71 | ~R$ 6.175 |
| Encoder Siemens/Heidenhain | 6FX2001-5JE22-3DA0 | ~R$ 9.000 |
| Encoder absoluto Profibus | IC G4AVMDP | ~R$ 3.325 |
| Cremalheira M2, 2 m | aço 1045 | ~R$ 298–310 |
| Pinhão M2 | 15–24 dentes | ~R$ 57–79 |
| Guia linear MGN15 | com patim | ~R$ 255–430 |
| Acoplamento flexível | 6–10 mm | ~R$ 25–150 |
| Mola helicoidal | conforme dimensionamento | ~R$ 20–100* |

\*Estimativa preliminar; a mola deve ser especificada pela força, curso, comprimento e constante elástica necessários.

Os preços de encoder variam bastante conforme resolução, protocolo e grau de proteção. Por exemplo, foram encontrados modelos ifm na faixa de aproximadamente R$ 3,1 mil a R$ 8,3 mil, enquanto um encoder Siemens/Heidenhain específico foi listado a R$ 9 mil.

---

# 16. Estimativa preliminar do custo do sistema

Como o diâmetro da cúpula e o comprimento total da cremalheira não foram fornecidos, o custo deve ser tratado como estimativa.

### Configuração econômica/industrial

| Item | Faixa estimada |
|---|---:|
| Encoder absoluto multivoltas | R$ 3.000–4.500 |
| Pinhão | R$ 60–150 |
| Cremalheira | R$ 300–1.500+ |
| Guia linear | R$ 250–600 |
| Acoplamento | R$ 30–150 |
| Eixo/rolamentos | R$ 150–500 |
| Mola e mecanismo de ajuste | R$ 50–200 |
| Base/suporte usinado | R$ 300–1.500 |
| Cabeamento/conectores | R$ 100–500 |

### Total preliminar

**≈ R$ 4.200 a R$ 9.600**

Esse valor considera apenas o conjunto de medição e seus elementos mecânicos básicos.

Não estão necessariamente incluídos:

- PLC;
- módulo/interface específica do encoder;
- fonte 24 Vcc;
- quadro elétrico;
- instalação;
- usinagem especial;
- pintura/tratamento superficial;
- proteção ambiental;
- calibração;
- projeto mecânico detalhado.

Para uma solução de maior precisão e maior robustez, utilizando encoder industrial premium e componentes mecânicos de precisão, o conjunto pode ultrapassar **R$ 10.000–15.000**.

---

# 17. Recomendações de projeto

Para uma aplicação astronômica, recomenda-se priorizar a **precisão do sistema completo**, e não apenas a resolução do encoder.

A especificação deve considerar:

### Mecânica

- cremalheira com passo controlado;
- pinhão de boa qualidade;
- baixo *backlash*;
- eixo próprio para o pinhão;
- rolamentos adequados;
- guia linear rígida;
- mola com pré-carga ajustável;
- proteção contra sujeira e umidade.

### Encoder

- absoluto;
- multivoltas;
- resolução mínima de 12 bits singleturn;
- interface compatível com o PLC;
- grau de proteção adequado ao ambiente;
- precisão especificada pelo fabricante.

### Software

- conversão da relação mecânica;
- *offset* de referência;
- tratamento 0°/360°;
- diagnóstico;
- detecção de falha de comunicação;
- limites de operação;
- registro de posição.

---

# 18. Pontos críticos que devem ser validados

Antes da fabricação definitiva, recomenda-se validar experimentalmente:

1. **Backlash do conjunto pinhão/cremalheira.**
2. **Erro de passo da cremalheira.**
3. **Excentricidade da cremalheira circular.**
4. **Força ideal da mola.**
5. **Rigidez do suporte.**
6. **Desalinhamento do eixo do pinhão.**
7. **Precisão real do encoder.**
8. **Erro angular total após calibração.**
9. **Comportamento em baixas velocidades.**
10. **Comportamento durante inversão do sentido de rotação.**
11. **Influência de temperatura.**
12. **Vibração e vento sobre a estrutura.**

---

# 19. Estratégia de validação

Uma forma prática de validar a solução é utilizar uma referência externa de maior precisão.

O procedimento pode ser:

1. Posicionar a cúpula em 0°.
2. Registrar a leitura do encoder.
3. Girar a cúpula para posições conhecidas.
4. Comparar a posição indicada pelo encoder com a posição de referência.
5. Repetir o procedimento nos sentidos horário e anti-horário.
6. Avaliar o erro de retorno.
7. Criar uma curva de erro angular.
8. Implementar compensação no PLC caso necessário.

Isso permite separar:

**resolução do encoder**  
de  
**precisão real do sistema mecânico**.

---

# 20. Conclusão

A solução apresentada utiliza uma arquitetura simples e robusta para transformar a rotação da cúpula em uma posição angular digital.

O conceito central é:

> **Cúpula → Cremalheira → Pinhão → Acoplamento → Encoder absoluto multivoltas → PLC → Azimute**

A utilização de um pinhão pequeno em relação à grande cremalheira da cúpula proporciona uma multiplicação mecânica significativa. Dessa forma, um encoder relativamente compacto consegue fornecer uma elevada resolução angular na cúpula.

A solução também apresenta uma vantagem importante para uma estrutura de grande porte: o encoder não precisa ser instalado diretamente no eixo principal da cúpula.

O mecanismo de mola mostrado no projeto complementa a solução ao manter o pinhão em contato permanente com a cremalheira e compensar pequenas variações de posição.

De acordo com a análise apresentada no README, essa arquitetura apresenta **alta resolução, baixa complexidade mecânica, baixo impacto de folgas intermediárias e boa adequação para aplicações de alta precisão em observatórios astronômicos**. fileciteturn0file0L37-L62

A principal ressalva é que a resolução teórica do encoder não deve ser confundida com a precisão final do sistema. Para uma cúpula astronômica, a qualidade da cremalheira, a montagem, o *backlash*, a rigidez estrutural, a pré-carga da mola e a calibração terão papel fundamental no desempenho final.

---

# 21. Fontes e referências de mercado

- Baumer Brasil — Encoders absolutos industriais e família EN580E.ML.
- Hengstler — Encoder absoluto AC58.
- Dynapar — Encoder absoluto AI25.
- ifm — Encoders absolutos multivoltas.
- FNF Soluções Industriais — Encoder Siemens/Heidenhain.
- Roleste — Guias lineares HIWIN.
- OceanTech Automation — Pinhões e componentes de transmissão.
- Azanelli — Cremalheira módulo 2 em aço 1045.
- Engrenagens Brasil — Cremalheira módulo 2.
- Mercado Livre — referências de preço para acoplamentos e componentes.

> **Data de referência dos preços:** setembro de 2026. Os valores são apenas indicativos e devem ser confirmados com fornecedores antes da compra.
