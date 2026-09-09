# SRMIE — Sistema de Roda de Medição Independente com Encoder

**Aplicação:** Instrumentação Angular de Cúpulas Astronômicas  
**Revisão:** 1.0 — Setembro 2026  
**Autores:** Equipe de Mecatrônica · UNIFEI / LNA

---

## 1. Visão Geral da Solução

### 1.1 Contexto e Problema

Cúpulas astronômicas de pequeno e médio porte utilizam tipicamente um motor de tração acoplado ao trilho circular para rotacionar a estrutura. Nesse arranjo, o encoder de posição angular está associado ao motor ou ao eixo de transmissão — não ao trilho diretamente.

Essa arquitetura introduz três mecanismos de erro sistemático:

| Fonte de Erro | Descrição | Impacto |
|---|---|---|
| **Patinagem por torque** | Escorregamento pneu/trilho durante aceleração/desaceleração | Erro cumulativo de posição (2–5°) |
| **Fita/régua óptica** | Folgas, emendas e contaminação da fita abrasiva | Saltos de pulsos, posição errática |
| **Folga mecânica** | Backlash nas engrenagens do redutor | Histerese na reversão de sentido |

### 1.2 Princípio da Solução — Desacoplamento Mecânico Total

O SRMIE resolve esses problemas ao separar completamente a função de **tração** da função de **medição**:

- **Motor de Tração** → Roletes de Tração → Rotaciona Cúpula (responsável pelo movimento)
- **Roda de Medição** → Encoder → Conta Pulsos → Azimute (responsável APENAS pela medição, SEM torque motor)

Como a roda de medição não transmite torque, o escorregamento por carga é eliminado. A condição de rolamento puro é mantida por:

1. **Força de contato controlada** via mola de compressão no braço pantográfico (15–35 N nominal).
2. **Material de alta aderência** na banda de rolamento (borracha de poliuretano, Shore 70A).
3. **Geometria de contato estável** garantida pelo pivô articulado que absorve excentricidades do trilho.

---

## 2. Formulação Cinemática

### 2.1 Parâmetros Geométricos

| Símbolo | Descrição | Valor do Projeto |
|---|---|---|
| R | Raio do trilho da cúpula (raio médio de contato) | 1.500 mm |
| r | Raio da roda de medição | 50 mm |
| λ | Razão cinemática | R/r = 30 |

### 2.2 Relação de Transmissão Cinemática

A relação entre o ângulo de rotação da cúpula (θ_c) e o ângulo percorrido pela roda de medição (θ_r) decorre diretamente da condição de **rolamento puro sem deslizamento**:

```
v_contato = ω_c · R = ω_r · r
```

Onde ω_c é a velocidade angular da cúpula e ω_r é a velocidade angular da roda. Integrando no tempo:

```
θ_r = θ_c · (R/r) = θ_c · λ = 30 · θ_c
```

Para cada grau de rotação da cúpula, a roda de medição completa **30°** de giro. Para uma volta completa da cúpula (360°), a roda gira **30 voltas completas**.

### 2.3 Condição de Rolamento Sem Deslizamento

O deslizamento relativo `s` na interface roda/trilho é definido como:

```
s = (v_roda - v_trilho) / v_trilho × 100%
```

A condição de rolamento puro exige `s = 0`, satisfeita quando `ω_r · r = ω_c · R`, ou seja `ω_r / ω_c = R/r = 30`.

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
N_total = PPR × 4 × λ = 1.024 × 4 × 30 = 122.880 pulsos/360°
```

### 3.3 Resolução Angular

```
δθ = 360° / N_total = 360° / 122.880 ≈ 0,0029° ≈ 0,175'
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
| 360° (Norte — 1 volta) | 122.880 |

---

## 4. Recomendações Mecânicas

### 4.1 Material e Dureza da Roda de Medição

| Propriedade | Especificação Recomendada | Justificativa |
|---|---|---|
| **Material** | Poliuretano fundido (PU) | Melhor resistência ao desgaste vs. borracha natural |
| **Dureza Shore** | 70A (±5A) | Equilíbrio entre aderência (< 60A deforma) e rigidez (> 80A desliza) |
| **Acabamento** | Superfície lisa (Ra ≤ 1,6 µm) | Maximiza área de contato real |
| **Largura da banda** | 20–30 mm | Distribui pressão de Hertz; evita mordedura de arestas |
| **Reposição** | A cada 500 h de operação | Desgaste abrasivo pode alterar o raio efetivo r |

> **IMPORTANTE:** A variação do raio efetivo por desgaste introduz erro sistemático proporcional. Uma redução de 1 mm no raio (50 → 49 mm) altera λ para ~30,61, causando erro de ~2% na leitura. A calibração periódica do raio efetivo é mandatória.

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
| Pré-carga da mola (instalada) | 1,5–2,0 mm de compressão além do livre |
| Curso total do braço | ± 8 mm (absorve excentricidades do trilho) |
| Rigidez da mola | 8–12 N/mm |
| Material do braço | Alumínio 6061-T6 ou perfil estrutural 20×20 mm |

### 4.3 Checklist de Comissionamento

- [ ] Verificar paralelismo do eixo da roda com teodolito ou nível digital
- [ ] Medir força de contato com dinamômetro (alvo: 20–25 N)
- [ ] Realizar homing e anotar posição de referência (chave fim-de-curso)
- [ ] Rotacionar cúpula manualmente 360° e verificar contagem: deve ser 122.880 ± 50 pulsos
- [ ] Verificar ausência de saltos de pulso no canal A e B do encoder
- [ ] Checar temperatura do rolamento após 30 min de operação (< 50°C)

---

## 5. Arquitetura de Controle e Integração

```
[Roda de Medição PU]
       |
       v
[Encoder Incremental — 1024 PPR · Quadratura ×4]
       |
       v
[Módulo HSC no CLP — High-Speed Counter — 122.880 pulsos/360°]
       |
       v
[Bloco de Cálculo — θ = P × 360° / 122880]
      / \
     /   \
    v     v
[SCADA]  [Malha PID de Posição]
              |
              v
        [Motor de Tração — Desacoplado da Medição]
```

---

## 6. Sumário de Desempenho

| Métrica | Valor SRMIE | Requisito Típico |
|---|---|---|
| Resolução angular | **0,0029°** (0,175') | < 1' |
| Pulsos por volta completa | **122.880** | — |
| Razão cinemática R/r | **30** | — |
| Erro de patinagem | **≈ 0** (sem torque motor) | < 0,1° |
| Faixa de operação | 0° – 360° (infinito com reset) | Full azimute |
| Temperatura de operação | −10°C a +50°C | −5°C a +40°C |
| Proteção ambiental | IP65 | IP54 mínimo |

---

*Documento gerado pelo Sistema de Documentação Técnica · UNIFEI/ECAEXT01 · Setembro 2026*
