# Solução de Medição Angular para Cúpula de Telescópio

## 1. Objetivo

Esta solução foi desenvolvida para medir a posição angular de uma cúpula
de telescópio de forma contínua, permitindo que o sistema de controle
conheça a posição real da cúpula e utilize essa informação como
**feedback de posição** em uma malha fechada.

A proposta utiliza uma conversão **rotação → deslocamento linear →
leitura eletrônica**, formada por:

-   pinhão;
-   cremalheira associada ao movimento da cúpula;
-   acoplamento de junta;
-   encoder linear;
-   bloco de guia linear;
-   mecanismo de mola;
-   base estrutural.

> **Hipótese mecânica importante:** o desenho apresentado não mostra
> explicitamente a cremalheira. Para que a solução funcione conforme
> descrita, o pinhão deve engrenar em uma cremalheira solidária à cúpula
> ou a um elemento que tenha deslocamento proporcional ao ângulo da
> cúpula. Alternativamente, pode ser utilizada uma roda dentada/rolete
> de medição em contato com uma pista. A interface pinhão--cremalheira
> é, portanto, um requisito de projeto.

------------------------------------------------------------------------

## 2. Princípio de funcionamento

A cúpula gira em torno do seu eixo azimutal. Esse movimento angular é
transmitido ao pinhão.

O pinhão engrena com uma cremalheira ou elemento equivalente. Quando a
cúpula gira:

1.  o pinhão gira;
2.  o contato entre pinhão e cremalheira transforma a rotação em
    deslocamento linear;
3.  esse deslocamento movimenta o carro associado ao encoder;
4.  o encoder linear mede o deslocamento;
5.  o sistema de controle converte o deslocamento linear medido em
    ângulo;
6.  o ângulo calculado é comparado com a posição desejada.

O princípio pode ser representado por:

**Ângulo da cúpula → rotação do pinhão → deslocamento linear → encoder →
posição angular**

Essa arquitetura permite instalar o sensor em uma região relativamente
protegida e utilizar uma geometria mecânica simples para obter a posição
angular.

------------------------------------------------------------------------

## 3. Equacionamento

Considerando um pinhão de módulo `m` e número de dentes `z`, seu
diâmetro primitivo é:

``` text
d = m · z
```

O deslocamento linear correspondente a uma volta do pinhão é:

``` text
L = π · d
```

Para um ângulo `θ` em graus:

``` text
x = (π · d / 360) · θ
```

Logo:

``` text
θ = (360 · x) / (π · d)
```

onde:

-   `θ` = posição angular da cúpula \[°\];
-   `x` = deslocamento medido pelo encoder \[mm\];
-   `d` = diâmetro primitivo do pinhão \[mm\].

### Exemplo de dimensionamento

Considere:

-   módulo do pinhão: `m = 1`;
-   número de dentes: `z = 20`;
-   diâmetro primitivo: `d = 20 mm`;
-   resolução do encoder linear: `5 µm = 0,005 mm`.

O deslocamento por volta é:

``` text
L = π · 20
L ≈ 62,83 mm/rev
```

Portanto, uma volta do pinhão corresponde a 360° da posição mecânica
equivalente.

A resolução angular ideal seria:

``` text
Δθ = 0,005 / 62,83 · 360
Δθ ≈ 0,0286°
```

Com encoder de 1 µm, a resolução teórica cairia para aproximadamente:

``` text
Δθ ≈ 0,0057°
```

**Atenção:** esses valores representam resolução geométrica/quantização.
A precisão real será limitada por folga da engrenagem, erro de montagem,
flexão estrutural, erro do encoder, temperatura, desgaste e
deslizamento.

------------------------------------------------------------------------

# 4. Descrição dos componentes

## 4.1 Pinhão

O pinhão é o elemento responsável por transformar a relação mecânica
entre a rotação da cúpula e o deslocamento do sistema de medição.

### Recomendações

-   aço carbono ou aço inoxidável;
-   módulo compatível com a cremalheira;
-   largura suficiente para suportar o esforço sem deformação;
-   tratamento superficial quando houver exposição à umidade;
-   montagem rígida no eixo;
-   concentricidade adequada.

Um pinhão pequeno aumenta a resolução angular por unidade de
deslocamento, mas aumenta a quantidade de deslocamento necessária para
grandes ângulos e pode aumentar a sensibilidade a erros de fabricação.

------------------------------------------------------------------------

## 4.2 Cremalheira

A cremalheira deve ser rigidamente associada à estrutura móvel da
cúpula.

Sua função é transformar a rotação relativa entre cúpula e base em
deslocamento linear.

### Requisitos

-   módulo igual ao do pinhão;
-   alinhamento adequado;
-   baixa folga;
-   comprimento suficiente para cobrir todo o movimento útil;
-   fixação rígida;
-   proteção contra sujeira e água.

Para uma cúpula com rotação de 360°, é necessário considerar se a
cremalheira será:

-   circular, acompanhando o perímetro da cúpula; ou
-   linear, em uma solução mecânica com movimento equivalente.

Para uma cúpula astronômica, a alternativa circular normalmente é mais
natural.

------------------------------------------------------------------------

## 4.3 Encoder linear

O encoder linear é o sensor responsável pela medição direta do
deslocamento do carro.

Uma opção particularmente interessante é um **encoder linear magnético
incremental**, pois tende a ser mais tolerante a poeira e contaminantes
do que soluções ópticas abertas.

Como referência industrial, a Balluff possui a família BML de encoders
lineares magnéticos. Há versões incrementais com saída A/B e resolução
na faixa de micrômetros, além de versões absolutas com interfaces como
SSI e BiSS-C.

Exemplo de produto:

-   Balluff BML-S1F1: sistema magnético incremental;
-   saída digital A/B em RS-422;
-   resolução de 5 µm em uma configuração disponível;
-   construção compacta.

A Balluff também possui sistemas BML com comprimentos de medição de
vários metros, o que demonstra a adequação da tecnologia para sistemas
de deslocamento maiores.

------------------------------------------------------------------------

## 4.4 Bloco de guia linear

O bloco de guia mantém o conjunto do encoder alinhado e permite que ele
se movimente somente no eixo previsto.

A utilização de uma guia linear é importante porque evita que forças
laterais sejam transmitidas diretamente ao encoder.

Para um protótipo, guias do tipo MGN12 podem ser utilizadas.

Para uma aplicação externa permanente, recomenda-se utilizar uma guia
industrial dimensionada para:

-   massa do conjunto;
-   aceleração;
-   vibração;
-   exposição ambiental;
-   número de ciclos;
-   tolerâncias de montagem.

------------------------------------------------------------------------

## 4.5 Mecanismo de mola

A mola mantém o conjunto pressionado contra a referência mecânica.

Sua função é compensar pequenas variações de distância, desalinhamentos
e deformações.

### Vantagens

-   reduz a possibilidade de perda de contato;
-   permite compensação de tolerâncias;
-   reduz esforços causados por desalinhamentos;
-   facilita a montagem;
-   permite que o conjunto acompanhe pequenas irregularidades.

### Cuidados

A força da mola deve ser suficiente para manter o contato, mas não
excessiva.

Uma força muito elevada pode:

-   aumentar o atrito;
-   gerar desgaste;
-   aumentar o torque necessário do sistema de acionamento;
-   deformar componentes;
-   introduzir erro de medição.

------------------------------------------------------------------------

## 4.6 Acoplamento de junta

O acoplamento entre o eixo e o elemento de medição deve absorver
pequenos desalinhamentos.

Um acoplamento flexível é preferível a uma ligação rígida quando houver
possibilidade de:

-   desalinhamento angular;
-   desalinhamento paralelo;
-   pequenas vibrações;
-   expansão térmica.

O acoplamento deve ser selecionado considerando torque, rotação máxima,
desalinhamento admissível e ambiente.

------------------------------------------------------------------------

# 5. Arquitetura da instalação

A montagem recomendada é:

``` text
                 CÚPULA
                   │
            Cremalheira circular
                   │
                 Pinhão
                   │
           Acoplamento flexível
                   │
           Carro do encoder
                   │
        ┌────────────────────┐
        │  Encoder linear    │
        └────────────────────┘
                   │
             Guia linear
                   │
                  Base
                   │
             Controlador
```

O conjunto deve ser instalado em uma região da base que permita:

-   acesso para manutenção;
-   proteção contra chuva;
-   inspeção visual;
-   ajuste da mola;
-   ajuste do alinhamento;
-   substituição do encoder.

------------------------------------------------------------------------

# 6. Detalhamento da instalação

## Etapa 1 --- Definição do ponto de medição

Escolher uma região estrutural da cúpula onde seja possível obter
movimento mecânico proporcional ao ângulo azimutal.

O ponto deve apresentar:

-   baixa flexão;
-   baixa vibração;
-   boa rigidez;
-   acesso para manutenção.

------------------------------------------------------------------------

## Etapa 2 --- Instalação da cremalheira

A cremalheira deve ser instalada concentricamente em relação ao eixo de
rotação da cúpula.

O erro de concentricidade é especialmente importante porque pode gerar
uma velocidade de deslocamento variável durante a rotação.

A estrutura deve possuir regulagem para permitir:

-   ajuste radial;
-   ajuste vertical;
-   ajuste de paralelismo;
-   ajuste da profundidade de engrenamento.

------------------------------------------------------------------------

## Etapa 3 --- Instalação do pinhão

O pinhão deve ser montado no eixo correspondente e alinhado com a
cremalheira.

Deve-se evitar:

-   desalinhamento;
-   engrenamento excessivo;
-   folga excessiva;
-   contato apenas em uma região dos dentes.

------------------------------------------------------------------------

## Etapa 4 --- Montagem da guia linear

A guia deve ser fixada em uma base rígida.

A superfície de montagem deve ser suficientemente plana para não
introduzir esforços no carro.

Para protótipos, uma guia MGN12 de aproximadamente 300 mm pode ser
suficiente para validar o conceito. Em uma aplicação final, o curso deve
ser definido a partir da geometria real do pinhão e do deslocamento
necessário.

------------------------------------------------------------------------

## Etapa 5 --- Montagem do encoder

O encoder deve ser instalado no carro de forma que seu eixo de medição
esteja alinhado com o deslocamento produzido pelo pinhão.

O fabricante do encoder deve ser seguido quanto a:

-   distância entre sensor e escala;
-   tolerância de paralelismo;
-   fixação;
-   raio mínimo do cabo;
-   aterramento;
-   alimentação;
-   blindagem.

Encoders magnéticos Balluff BML, por exemplo, possuem especificações
explícitas de distância de leitura e tolerâncias de instalação.

------------------------------------------------------------------------

## Etapa 6 --- Ajuste da mola

A mola deve manter o conjunto pressionado contra a referência mecânica
durante todo o movimento.

O ajuste deve ser realizado verificando:

1.  contato em toda a faixa angular;
2.  ausência de travamento;
3.  ausência de escorregamento;
4.  torque adicional exigido pelo acionamento;
5.  repetibilidade da leitura.

------------------------------------------------------------------------

## Etapa 7 --- Calibração

Após a instalação, deve-se determinar a relação real entre deslocamento
e ângulo.

Recomenda-se utilizar pelo menos três pontos de referência:

``` text
0°
180°
360°
```

Idealmente, utilizar mais pontos, por exemplo:

``` text
0°, 30°, 60°, 90°, ..., 360°
```

Os valores devem ser comparados com uma referência externa.

A curva de calibração pode ser armazenada no controlador para compensar
pequenos erros sistemáticos.

------------------------------------------------------------------------

# 7. Sistema eletrônico

O encoder pode fornecer sinais incrementais A/B.

A sequência dos sinais determina o sentido de movimento.

A posição pode ser obtida por contagem de pulsos:

``` text
Pulsos → Deslocamento → Ângulo
```

Para um encoder incremental, é recomendável utilizar:

-   entrada de contador de alta velocidade;
-   interrupções;
-   FPGA;
-   PLC com entrada de encoder;
-   microcontrolador com periférico de encoder.

Em um sistema baseado em STM32, por exemplo, os timers podem operar no
modo de encoder para realizar a contagem dos canais A/B.

### Cuidados elétricos

Como a cúpula pode possuir motores, inversores e cabos de potência
próximos ao sensor, recomenda-se:

-   cabo blindado;
-   aterramento adequado;
-   separação entre potência e sinal;
-   par trançado para sinais diferenciais;
-   interface RS-422 quando disponível;
-   proteção contra surtos;
-   fonte estabilizada.

------------------------------------------------------------------------

# 8. Vantagens da solução

## 8.1 Medição indireta com alta resolução

A utilização do pinhão permite converter pequenos deslocamentos em uma
representação precisa da posição angular.

## 8.2 Possibilidade de redundância

A solução pode coexistir com outro sensor, como:

-   encoder absoluto;
-   sensor de referência;
-   sensor Hall;
-   fim de curso;
-   inclinômetro.

Isso é particularmente interessante para um sistema astronômico, no qual
a perda de referência pode comprometer o apontamento.

## 8.3 Manutenção relativamente simples

O conjunto pode ser construído de forma modular:

-   encoder;
-   guia;
-   mola;
-   pinhão;
-   acoplamento.

Isso facilita a substituição individual dos componentes.

## 8.4 Possibilidade de utilizar componentes comerciais

Guias lineares, encoders, acoplamentos e componentes de transmissão são
facilmente encontrados no mercado.

## 8.5 Boa possibilidade de proteção ambiental

Um encoder magnético pode ser instalado dentro de uma carcaça protegida,
enquanto a cremalheira e o pinhão podem receber proteção contra chuva e
poeira.

## 8.6 Custo inferior a soluções metrológicas de alto desempenho

Uma arquitetura construída com componentes industriais convencionais
pode custar significativamente menos que um sistema absoluto de alta
precisão para máquinas-ferramenta.

------------------------------------------------------------------------

# 9. Desvantagens e restrições

## 9.1 Folga mecânica

A folga entre pinhão e cremalheira pode produzir erro de posição,
principalmente durante a inversão do sentido de rotação.

Esse é um dos principais pontos de atenção da solução.

### Mitigação

Pode-se utilizar:

-   pinhão de baixa folga;
-   cremalheira de precisão;
-   pré-carga;
-   sistema de dois pinhões;
-   engrenagem dividida com mola;
-   calibração por sentido.

------------------------------------------------------------------------

## 9.2 Erro de concentricidade

Se a cremalheira circular não estiver perfeitamente concêntrica ao eixo
da cúpula, a relação entre ângulo e deslocamento pode variar ao longo da
volta.

Esse erro pode ser corrigido parcialmente por calibração.

------------------------------------------------------------------------

## 9.3 Desgaste

O contato mecânico entre pinhão e cremalheira sofre desgaste.

É necessário considerar:

-   lubrificação;
-   material;
-   dureza;
-   corrosão;
-   contaminação;
-   frequência de operação.

------------------------------------------------------------------------

## 9.4 Vibração

Uma cúpula possui grandes massas e pode sofrer vibrações devido ao vento
e ao motor.

A estrutura do sensor deve possuir rigidez suficiente para evitar que
vibrações sejam interpretadas como movimento angular.

------------------------------------------------------------------------

## 9.5 Dependência de montagem

A precisão final não depende somente do encoder.

O erro total pode ser influenciado por:

``` text
Erro total =
encoder
+ engrenagem
+ montagem
+ flexão
+ folga
+ temperatura
+ calibração
```

------------------------------------------------------------------------

## 9.6 Limitação de curso

O encoder linear possui um curso máximo.

O curso necessário deve ser calculado antes da seleção.

Por exemplo, com um pinhão de diâmetro primitivo de 20 mm:

``` text
360° → 62,83 mm
```

Portanto, para medir uma volta completa sem utilizar um sistema circular
de referência, o curso linear necessário seria aproximadamente 62,8 mm.

------------------------------------------------------------------------

# 10. Produtos comerciais que podem ser utilizados

## 10.1 Encoder linear magnético Balluff BML

A família Balluff BML oferece soluções magnéticas lineares incrementais
e absolutas.

Um exemplo, o BML0916, possui:

-   saída digital A/B;
-   interface RS-422;
-   resolução de 5 µm;
-   corpo compacto;
-   construção em alumínio.

É uma das opções mais próximas do conceito apresentado.

Fonte: Balluff --- BML0916.

------------------------------------------------------------------------

## 10.2 Balluff BML S1H / BML S1G

Para uma solução mais sofisticada, existem versões absolutas da família
BML.

O BML S1H pode trabalhar com SSI/BiSS-C e apresenta resolução
submicrométrica em determinadas configurações.

O BML S1G possui opções para comprimentos de medição de até dezenas de
metros e resolução configurável.

Essas soluções são mais apropriadas quando o projeto exige elevada
confiabilidade e precisão.

------------------------------------------------------------------------

## 10.3 HEIDENHAIN LC 400

A HEIDENHAIN possui encoders lineares absolutos LC 400 destinados a
aplicações de alta precisão.

É uma solução de nível muito superior ao necessário para um protótipo
acadêmico, mas serve como referência para uma aplicação profissional de
alta precisão.

A série possui diversos comprimentos de medição e recursos específicos
para instalação e alinhamento.

------------------------------------------------------------------------

## 10.4 Renishaw TONiC

A Renishaw possui sistemas de encoders lineares ópticos de alto
desempenho.

São adequados quando a prioridade é precisão muito elevada.

Para uma cúpula externa, entretanto, a proteção ambiental e o custo
precisam ser avaliados cuidadosamente.

------------------------------------------------------------------------

# 11. Alternativa comercial simplificada

Uma alternativa é utilizar diretamente um **encoder rotativo com roda de
medição**.

Nesse caso:

``` text
Cúpula → roda de medição → encoder rotativo → controlador
```

Existem inclusive suportes comerciais com mola para manter a roda em
contato com a superfície.

No Brasil, encontram-se kits de suporte com mola e roda de medição para
encoders incrementais.

Essa arquitetura pode ser consideravelmente mais barata e mais simples
de implementar, mas introduz a possibilidade de:

-   escorregamento;
-   desgaste da roda;
-   erro de contato;
-   sujeira;
-   variação de diâmetro efetivo da roda.

Por isso, a solução com pinhão/cremalheira tende a ser mecanicamente
mais determinística quando a transmissão é bem projetada.

------------------------------------------------------------------------

# 12. Estimativa de custo

Os valores abaixo são estimativas para projeto no Brasil e devem ser
usados como referência de orçamento, não como cotação.

  Componente                                                  Faixa estimada
  ----------------------------------------------- --------------------------
  Encoder linear magnético de baixo/médio custo         R\$ 500 -- R\$ 2.500
  Encoder linear industrial de alta precisão        R\$ 3.000 -- R\$ 15.000+
  Guia linear                                              R\$ 70 -- R\$ 350
  Pinhão                                                   R\$ 50 -- R\$ 200
  Cremalheira                                             R\$ 100 -- R\$ 500
  Acoplamento flexível                                     R\$ 50 -- R\$ 250
  Mola e suporte                                           R\$ 30 -- R\$ 150
  Base usinada/fixadores                                  R\$ 150 -- R\$ 600
  Proteção/carenagem                                      R\$ 100 -- R\$ 500
  Cabeamento e conectores                                  R\$ 50 -- R\$ 250
  Eletrônica/interface                                    R\$ 100 -- R\$ 500
  Usinagem e montagem                                   R\$ 300 -- R\$ 1.500

### Total estimado

**Protótipo econômico:**

> aproximadamente **R\$ 1.400 -- R\$ 3.500**

**Sistema industrial intermediário:**

> aproximadamente **R\$ 3.500 -- R\$ 8.000**

**Sistema de alta precisão:**

> pode ultrapassar **R\$ 10.000 -- R\$ 20.000**, dependendo
> principalmente do encoder, precisão mecânica e serviço de
> instalação/calibração.

Como referência de mercado brasileiro, anúncios atuais de encoders
rotativos incrementais de 1024 PPR variam aproximadamente de R\$ 200 a
mais de R\$ 3.000, dependendo da marca, interface e especificação. Isso
demonstra que o sensor escolhido tem grande impacto no orçamento.

------------------------------------------------------------------------

# 13. Exemplo de configuração recomendada para protótipo

Para validar o conceito antes de investir em um encoder industrial caro,
uma configuração inicial poderia ser:

  Item          Especificação sugerida
  ------------- ------------------------------------------
  Pinhão        módulo 1, 20 dentes
  Cremalheira   módulo 1
  Encoder       linear magnético incremental, 5 µm
  Guia          MGN12 ou equivalente
  Curso         ≥ 70 mm
  Mola          pré-carga regulável
  Acoplamento   flexível
  Controlador   STM32 / ESP32 / PLC
  Interface     A/B, preferencialmente diferencial
  Proteção      IP54 ou superior para conjunto protegido
  Referência    sensor de zero + encoder incremental

A configuração de 20 dentes e módulo 1 produz aproximadamente:

``` text
62,83 mm/rev
```

Assim, um encoder com resolução de 5 µm fornece aproximadamente:

``` text
0,0286° por incremento ideal
```

Esse valor é mais do que suficiente para um protótipo de controle de
azimute, desde que os erros mecânicos sejam controlados.

------------------------------------------------------------------------

# 14. Calibração recomendada

Para obter melhor desempenho, recomenda-se realizar uma calibração
completa após a montagem.

Um procedimento possível:

1.  posicionar a cúpula em 0°;
2.  zerar o contador;
3.  mover para 30°;
4.  registrar a leitura;
5.  repetir em intervalos de 30° até 360°;
6.  repetir o procedimento no sentido contrário;
7.  calcular erro de posição;
8.  identificar histerese;
9.  criar uma tabela de compensação.

A diferença entre os resultados de ida e volta é especialmente
importante para identificar **backlash**.

------------------------------------------------------------------------

# 15. Recomendações de projeto

Antes da fabricação definitiva, recomenda-se:

### Prioridade 1 --- Resolver a transmissão mecânica

A interface:

**cúpula → cremalheira → pinhão → encoder**

deve possuir geometria claramente definida.

### Prioridade 2 --- Reduzir folga

O backlash pode ser mais significativo que a resolução do encoder.

### Prioridade 3 --- Proteger o conjunto

O sensor deve ser protegido contra:

-   chuva;
-   poeira;
-   condensação;
-   insetos;
-   radiação solar direta;
-   corrosão.

### Prioridade 4 --- Criar referência absoluta

Mesmo utilizando encoder incremental, recomenda-se adicionar um sensor
de referência/zero.

### Prioridade 5 --- Testar antes da instalação

O sistema deve ser testado em bancada com:

-   rotação lenta;
-   rotação rápida;
-   inversão de sentido;
-   vibração;
-   ciclos repetitivos.

------------------------------------------------------------------------

# 16. Conclusão

A solução proposta é uma arquitetura **mecatrônica de medição angular
indireta**, na qual o movimento angular da cúpula é convertido em
deslocamento linear e posteriormente medido por um encoder.

Sua principal vantagem é combinar:

-   alta resolução;
-   componentes comerciais;
-   manutenção relativamente simples;
-   possibilidade de calibração;
-   integração direta com um controlador;
-   possibilidade de operação em malha fechada.

O ponto mais crítico do projeto não é a resolução do encoder, mas a
**qualidade da transmissão mecânica**. Folga, concentricidade, rigidez,
desalinhamento e escorregamento podem gerar erros muito maiores do que o
erro de quantização do sensor.

Para um protótipo acadêmico, recomenda-se começar com um encoder
magnético incremental de aproximadamente 5 µm, guia linear compacta,
pinhão/cremalheira módulo 1 e referência de zero. Depois de validar a
arquitetura, pode-se migrar para um encoder absoluto industrial caso os
requisitos de confiabilidade e precisão justifiquem o investimento.

------------------------------------------------------------------------

# 17. Referências e produtos consultados

-   Balluff --- família de encoders lineares magnéticos BML.
-   Balluff --- BML0916, encoder linear magnético incremental.
-   Balluff --- BML S1H / BML S1G.
-   HEIDENHAIN --- encoders lineares LC 400.
-   Mercado Livre Brasil --- referências de preços de encoders
    incrementais de 1024 PPR.
-   Mercado Livre Brasil --- referências de preços de guias lineares
    MGN12.
-   PRIMTEC --- suporte com mola e roda de medição para encoder
    incremental.
-   SE Instrumentos --- suportes articulados e soluções de medição com
    encoder.

> **Observação de orçamento:** preços de marketplace são voláteis e
> podem variar com câmbio, impostos, frete, disponibilidade e
> fornecedor. Para a especificação final da cúpula, deve-se solicitar
> cotação diretamente ao fabricante/distribuidor.

