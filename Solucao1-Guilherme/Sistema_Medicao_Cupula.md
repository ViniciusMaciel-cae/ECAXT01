# SRMIE - Sistema de Roda de Medição Independente com Encoder

**Aplicação:** Instrumentação Angular de Cúpulas Astronômicas  
**Revisão:** 1.0 - Setembro 2026  
**Autores:** Equipe de Mecatrônica – UNIFEI / LNA

---

## 1. Visão Geral da Solução

### 1.1 Contexto e Problema

Cúpulas astronômicas de pequeno e médio porte utilizam tipicamente um motor de tração acoplado ao trilho circular para rotacionar a estrutura. Nesse arranjo, o encoder de posição angular está associado ao motor ou ao eixo de transmissão - não ao trilho diretamente.

Essa arquitetura introduz três mecanismos de erro sistemático:

| Fonte de Erro | Descrição | Impacto |
|---|---|---|
| **Patinagem por torque** | Escorregamento pneu/trilho durante aceleração/desaceleração | Erro cumulativo de posição (2-5°) |
| **Fita/régua óptica** | Folgas, emendas e contaminação da fita abrasiva | Saltos de pulsos, posição errática |
| **Folga mecânica** | Backlash nas engrenagens do redutor | Histerese na reversão de sentido |

### 1.2 Princípio da Solução - Desacoplamento Mecânico Total

O SRMIE resolve esses problemas ao separar completamente a função de **tração** da função de **medição**:

- **Motor de Tração** ➔ Roletes de Tração ➔ Rotaciona Cúpula (responsável pelo movimento)
- **Roda de Medição** ➔ Encoder ➔ Conta Pulsos ➔ Azimute (responsável APENAS pela medição, SEM torque motor)

Como a roda de medição não transmite torque, o escorregamento por carga é eliminado. A condição de rolamento puro é mantida por:

1. **Força de contato controlada** via mola de compressão no braço pantográfico (15-35 N nominal).
2. **Material de alta aderência** na banda de rolamento (borracha de poliuretano, Shore 70A).
3. **Geometria de contato estável** garantida pelo pivô articulado que absorve excentricidades do trilho.

---

## 2. Formulação Cinemática

### 2.1 Parâmetros Geométricos

| Símbolo | Descrição | Valor do Projeto |
|---|---|---|
| R | Raio do trilho da cúpula (raio médio de contato) | 1.500 mm |
| r | Raio da roda de medição | 50 mm |
| γ | Razão cinemática | R/r = 30 |

### 2.2 Relação de Transmissão Cinemática

A relação entre o ângulo de rotação da cúpula (θ_c) e o ângulo percorrido pela roda de medição (θ_r) decorre diretamente da condição de **rolamento puro sem deslizamento**:

```
v_contato = θ_c × R = θ_r × r
```

Onde θ_c é a velocidade angular da cúpula e θ_r é a velocidade angular da roda. Integrando no tempo:

```
θ_r = θ_c × (R/r) = θ_c × γ = 30 × θ_c
```

Para cada grau de rotação da cúpula, a roda de medição completa **30°** de giro. Para uma volta completa da cúpula (360°), a roda gira **30 voltas completas**.

### 2.3 Condição de Rolamento Sem Deslizamento

O deslizamento relativo `s` na interface roda/trilho é definido como:

```
s = (v_roda - v_trilho) / v_trilho × 100%
```

A condição de rolamento puro exige `s = 0`, satisfeita quando `θ_r × r = θ_c × R`, ou seja `θ_r / θ_c = R/r = 30`.

**Fatores que garantem s ≈ 0 no SRMIE:**
- Ausência de torque motor na roda de medição (força tangencial Ft ≈ 0)
- Força normal de contato F_N controlada pela mola (regime elástico, não plástico)
- Velocidade de operação baixa (cúpula: tipicamente < 3°/s), evitando regimes inerciais

---

## 3. Cálculo de Pulsos e Resolução Angular

### 3.1 Especificação do Encoder

| Parâmetro | Valor |
|---|---|
| Tipo | Encoder rotativo incremental (óptico ou magnético) |
| Resolução nativa | 1.024 PPR (Pulsos Por Revolução) |
| Modo de decodificação | Quadratura ×4 (canais A e B em 4 flancos) |
| Resolução efetiva | 1.024 × 4 = 4.096 pulsos/volta da roda |

### 3.2 Pulsos por Volta Completa da Cúpula

```
N_total = PPR × 4 × γ = 1.024 × 4 × 30 = 122.880 pulsos/360°
```

### 3.3 Resolução Angular

```
dθ = 360° / N_total = 360° / 122.880 ≈ 0,0029° ≈ 0,175'
```

Essa resolução de **0,175 arcminutos** supera os requisitos típicos de pontaria de telescópios amadores e semi-profissionais (geralmente < 1').

### 3.4 Equação de Azimute em Tempo Real

Dado o contador de pulsos acumulado P(t):

```
θ_azimute(t) = P(t) / N_total × 360° = P(t) / 122.880 × 360°
```

### 3.5 Tabela de Referência Rápida

| Posição Cúpula | Pulsos Acumulados |
|---|---|
| 0° (Homing/Norte) | 0 |
| 90° (Leste) | 30.720 |
| 180° (Sul) | 61.440 |
| 270° (Oeste) | 92.160 |
| 360° (Norte - 1 volta) | 122.880 |

---

## 4. Recomendações Mecânicas

### 4.1 Material e Dureza da Roda de Medição

| Propriedade | Especificação Recomendada | Justificativa |
|---|---|---|
| **Material** | Poliuretano fundido (PU) | Melhor resistência ao desgaste vs. borracha natural |
| **Dureza Shore** | 70A (±5A) | Equilíbrio entre aderência (< 60A deforma) e rigidez (> 80A desliza) |
| **Acabamento** | Superfície lisa (Ra = 1,6 μm) | Maximiza área de contato real |
| **Largura da banda** | 20-30 mm | Distribui pressão de Hertz; evita mordedura de arestas |
| **Reposição** | A cada 500 h de operação | Desgaste abrasivo pode alterar o raio efetivo r |

> **IMPORTANTE:** A variação do raio efetivo por desgaste introduz erro sistemático proporcional. Uma redução de 1 mm no raio (50 ➔ 49 mm) altera γ para ≈30,61, causando erro de ≈2% na leitura. A calibração periódica do raio efetivo é mandatória.

### 4.2 Alinhamento do Pantógrafo

#### Paralelismo do Eixo da Roda
O eixo da roda de medição deve ser **estritamente paralelo ao eixo vertical da cúpula** (eixo Z).

**Tolerância máxima:** α < 0,5° (verificar com relógio comparador durante instalação).

#### Força de Contato
A mola de compressão deve fornecer força normal F_N:

```
15 N ≤ F_N ≤ 35 N
```

- **Abaixo de 15 N:** risco de perda de contato em vibrações e excentricidades do trilho
- **Acima de 35 N:** pressão excessiva acelera desgaste do PU; aumenta resistência ao rolamento

#### Parâmetros do Braço Pantográfico

| Parâmetro | Valor |
|---|---|
| Pré-carga da mola (instalada) | 1,5-2,0 mm de compressão além do livre |
| Curso total do braço | ± 8 mm (absorve excentricidades do trilho) |
| Rigidez da mola | 8-12 N/mm |
| Material do braço | Alumínio 6061-T6 ou perfil estrutural 20x20 mm |

### 4.3 Checklist de Comissionamento

- [ ] Verificar paralelismo do eixo da roda com teodolito ou nível digital
- [ ] Medir força de contato com dinamômetro (alvo: 20-25 N)
- [ ] Realizar homing e anotar posição de referência (chave fim-de-curso)
- [ ] Rotacionar cúpula manualmente 360° e verificar contagem: deve ser 122.880 ± 50 pulsos
- [ ] Verificar ausência de saltos de pulso no canal A e B do encoder
- [ ] Checar temperatura do rolamento após 30 min de operação (< 50°C)

### 4.4 Função do Braço Pantográfico com Mola
O braço pantográfico atua como um sistema de suspensão ativa para a roda de medição. Trilhos de cúpulas astronômicas frequentemente apresentam ovalização, excentricidades e irregularidades radiais de alguns milímetros ao longo do perímetro. O braço rotaciona em torno de seu pivô para absorver essas imperfeições, enquanto a mola helicoidal mantém a roda pressionada contra o trilho. Isso garante que a força normal permaneça dentro da faixa de 15 a 35 N, evitando a perda de contato (o que faria o encoder perder pulsos) sem sobrecarregar a estrutura.

### 4.5 Justificativa da Independência Total da Roda de Medição
Diferente das rodas tratoras, a roda de medição do SRMIE não possui função de tração e não está acoplada a nenhum motor. Como não precisa transmitir torque para vencer a inércia da cúpula, a força tangencial na interface roda/trilho é praticamente zero. Isso elimina o microdeslizamento (creep) e o escorregamento inerentes aos sistemas de tração, que podem acumular erros de 2° a 5° por volta. A roda independente atinge um regime de rolamento puro (slip ≈ 0).

### 4.6 Acoplamento Direto Eixo-Encoder
O encoder é acoplado diretamente ao eixo da roda de medição (direct-drive), sem polias, correias ou engrenagens intermediárias. O corpo do encoder é fixado rigidamente ao braço pantográfico, de modo que apenas o seu eixo rotacione junto com a roda. Essa arquitetura elimina completamente o "backlash" (folga mecânica de reversão) e erros periódicos de transmissão, garantindo que cada grau de rotação da roda corresponda exata e instantaneamente a um deslocamento angular no sensor.

---

## 5. Arquitetura de Controle e Integração

```
[Roda de Medição PU]
       |
       v
[Encoder Incremental - 1024 PPR × Quadratura ×4]
       |
       v
[Módulo HSC no CLP - High-Speed Counter - 122.880 pulsos/360°]
       |
       v
[Bloco de Cálculo - θ = P × 360° / 122880]
      / \
     /   \
    v     v
[SCADA]  [Malha PID de Posição]
              |
              v
        [Motor de Tração - Desacoplado da Medição]
```

---

## 6. Sumário de Desempenho

| Métrica | Valor SRMIE | Requisito Típico |
|---|---|---|
| Resolução angular | **0,0029°** (0,175') | < 1' |
| Pulsos por volta completa | **122.880** | - |
| Razão cinemática R/r | **30** | - |
| Erro de patinagem | **≈ 0** (sem torque motor) | < 0,1° |
| Faixa de operação | 0° - 360° (infinito com reset) | Full azimute |
| Temperatura de operação | -10°C a +50°C | -5°C a +40°C |
| Proteção ambiental | IP65 | IP54 mínimo |

---

## 7. Especificações de Hardware, Orçamento (BOM) e Instalação

### 7.1 Lista de Materiais e Preços Estimados

| Componente | Especificações Técnicas | Sugestão de Modelo / Marca | Preço Estimado (R$) |
|:---|:---|:---|:---|
| **Encoder Incremental** | 1024 PPR, Quadratura (Fases A/B/Z), eixo sólido de 6mm ou 8mm, Push-pull/HTL ou Line Driver, IP65. | Autonics E50S8-1024-3-T-24 ou Sick DFS60 | R$ 650,00 - R$ 900,00 |
| **Roda de Medição** | Diâmetro ext. exato de 100mm (r=50mm), banda em Poliuretano (PU) ou Borracha Nitrílica, dureza 75-85 Shore A. Furo central alinhado ao eixo do encoder. | Sick (Série BEF-MR) ou usinagem local (alma em alumínio, recobrimento em PU). | R$ 180,00 - R$ 350,00 |
| **Braço e Suporte** | Chapa de aço galvanizado 3mm ou perfil de alumínio 20x20mm, pivô com rolamentos/mancais miniatura para giro suave. | Fabricação própria (corte laser/dobra) | R$ 120,00 - R$ 200,00 |
| **Mola de Tensão** | Mola de compressão em aço inox. Constante elástica k ≈ 10 N/mm. Curso útil de compressão de ~5mm. | Peça de prateleira (ex: molas para matrizes/estampos) | R$ 30,00 - R$ 60,00 |
| **Acoplamento Flexível** | Acoplamento elástico tipo oldham ou fole (6x6mm ou 6x8mm), caso a roda não seja montada diretamente no eixo do encoder. | Kalatec ou similar | R$ 50,00 - R$ 90,00 |
| **Cabeamento e Acessórios** | Cabo blindado multivias (ex: 4 vias + malha), cantoneiras L para fixação, parafusos M8. | Diversos | R$ 80,00 - R$ 150,00 |
| **TOTAL ESTIMADO** | Valores base para montagem de 1 conjunto (podem variar conforme fornecedor e cotação do dólar). | - | **R$ 1.110,00 a R$ 1.750,00** |

### 7.2 Diretrizes de Instalação Física na Cúpula

A equipe de montagem deve seguir rigorosamente os passos abaixo para garantir a precisão do sistema:

#### Calibração da Pressão da Mola
A força normal (pressão) da roda contra o trilho deve ser ajustada através da pré-carga da mola para a faixa ótima de 15 N a 35 N.
* **Perigo de mola frouxa (< 15 N):** A roda pode perder contato durante vibrações ou nas áreas mais irregulares do trilho, patinando e perdendo a contagem de pulsos, o que exige um "homing" do zero.
* **Perigo de mola excessivamente tensionada (> 35 N):** Esmaga a borracha da roda. Isso diminui o raio dinâmico de rolamento `r`. Como a nossa calibração cinemática assume exatamente 50 mm, qualquer deformação permanente introduzirá um erro sistemático de cálculo no CLP, além de desgastar prematuramente o poliuretano.

#### Alinhamento Axial (Nivelamento)
O suporte de fixação do SRMIE deve ser posicionado de forma que o eixo de rotação da roda fique perfeitamente **paralelo** à parede do anel da cúpula (plano Z-vertical).
* Utilize um nível bolha de precisão ou nível digital.
* Se a roda for montada inclinada (ângulo de camber/caster involuntário), ela sofrerá forças transversais durante o giro. Esse arrasto lateral ("scrubbing") causa saltos de pulso e rasga a banda de rolamento em poucas semanas de operação.

#### Imunidade a Ruído Elétrico
O encoder enviará milhares de pulsos de onda quadrada por segundo (1024 PPR x 4 x 30) para o controlador.
* **Aterramento e Blindagem:** Utilize exclusivamente cabos manga com blindagem em malha de cobre. Aterrar a malha do cabo **em apenas uma das pontas** (preferencialmente no painel do CLP) para evitar loops de terra.
* **Roteamento de Cabos:** Jamais passe o cabo de sinal do encoder na mesma calha ou conduíte dos cabos de potência do motor de tração, especialmente se este for acionado por um inversor de frequência (ruído de chaveamento PWM). Mantenha uma distância física mínima de 15 cm entre os cabos de sinal e de potência.

---

*Documento gerado pelo Sistema de Documentação Técnica – UNIFEI/ECAEXT01 – Setembro 2026*
