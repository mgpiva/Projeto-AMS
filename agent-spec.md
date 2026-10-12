# agent-spec.md — modelo do meu agente

<!--
Este arquivo é para preencher, não para ler e devolver. As perguntas de cada
seção estão em comentário como este: elas não aparecem na pré-visualização,
aparecem no editor. Responda ABAIXO de cada seção, apagando nada delas.

As seções que ainda não foram vistas em aula ficam de pé, com a linha
"quando se preenche" dizendo o encontro em que isso acontece. O documento
cresce a cada laboratório — o de hoje é o primeiro.

Preencha com exemplos concretos do SEU domínio. Uma linha que funcionaria
para qualquer empresa é uma linha genérica — o exemplo do seu segmento
(agente-spec-exemplo.pdf, no mesmo pacote) mostra o contraste.
-->

**Domínio:** AMS — Assistente de Manutenção Segura
**Autor(a):** Marília Girardi Piva · **Última revisão:** 11/10/2026

---

## 1. PEAS

<!-- Perguntas que guiam o preenchimento:
  - Performance: o que conta como sucesso, em uma fórmula? Sobre o quê se
    conta, sobre qual conjunto, quanto é sucesso? Qual termo impede a
    trapaça mais óbvia (fechar rápido sem resolver)?
  - Environment: onde o agente opera? O que existe lá — sistemas, pessoas,
    documentos, ruído? Cite elementos concretos.
  - Actuators: o que o agente pode FAZER? Inclua comunicar e escalar:
    informar a decisão é uma ação, e passar adiante também é.
  - Sensors: o que o agente consegue PERCEBER? Só o que ele enxerga existe
    para ele.
  Um PEAS que menciona tecnologia de solução (banco vetorial, nome de modelo)
  está descrevendo a solução — o PEAS descreve o problema.
-->
**Quando se preenche:** no Laboratório 01, hoje. Refinado na Atividade Orientada.

**Performance:**

Taxa de avaliações aprovadas na revisão técnica (%) = (número de avaliações aprovadas / total de avaliações revisadas) × 100.

A avaliação considera ordens de manutenção do domínio AMS, utilizando os dados de sensores e a descrição da intervenção disponíveis em cada caso. Uma avaliação só é aprovada quando a classificação de risco, as normas indicadas e a recomendação de bloqueio e etiquetagem (LOTO) são consideradas adequadas pelo revisor técnico. Recomendações críticas inseguras impedem a aprovação.

Critério de sucesso proposto: taxa de aprovação de pelo menos 90%, com zero recomendações críticas inseguras no conjunto avaliado. A meta é inicial e deverá ser validada tecnicamente. 

**Environment:**

Planta industrial com máquinas de produção monitoradas por sensores; ordens de serviço preventivas e corretivas; técnicos de manutenção que executam as intervenções; supervisores de segurança que auditam as recomendações; histórico de falhas disponível; e documentação regulamentar aplicável, incluindo NR-10, NR-12 e NR-35. As ordens podem conter descrições incompletas, e as condições reais da máquina podem não ser totalmente representadas pelas leituras disponíveis.

**Actuators:**

Classificar o risco de falha como baixo, médio ou alto; indicar as normas regulamentares potencialmente aplicáveis; recomendar se a intervenção exige bloqueio e etiquetagem (LOTO); apresentar a justificativa da avaliação; informar quando os dados forem insuficientes ou houver incerteza relevante; comunicar o parecer ao técnico; e encaminhar casos críticos ou inconclusivos para avaliação humana. O agente apoia a decisão, mas não executa o bloqueio físico nem substitui a decisão final do técnico.

**Sensors:**

Leituras disponíveis de temperatura do ar, temperatura do processo, rotação, torque e desgaste da ferramenta; identificação e tipo da máquina, quando disponíveis; indicadores de falha presentes nos dados; identificador e descrição da ordem de serviço; e conteúdo dos documentos regulamentares disponibilizados ao sistema. Histórico detalhado de manutenção e contexto adicional só serão considerados quando estiverem efetivamente disponíveis nas fontes consultadas.

---

## 2. O ambiente em sete dimensões

<!--
Perguntas que guiam o preenchimento (uma por dimensão):
  - Observabilidade: existe algo que muda a decisão e o agente não mede?
  - Número de agentes: existe alguém cujas ações mudam o seu resultado?
    Um humano que reage ao agente conta.
  - Determinismo: a mesma ação no mesmo estado dá sempre o mesmo efeito?
  - Episódico ou sequencial: a decisão de agora afeta as decisões seguintes?
  - Estático ou dinâmico: o ambiente muda enquanto o agente delibera?
  - Discreto ou contínuo: estados e ações são contáveis?
  - Conhecido ou desconhecido: o agente sabe de antemão as regras? (Trata do
    que ele SABE, não do que ele VÊ.)
  Cada classificação vem com uma frase de justificativa — a etiqueta sozinha
  não vale nada.
-->
**Quando se preenche:** no Laboratório 02, no sábado (encontro 2).


| Dimensão | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | O AMS recebe leituras de sensores e a descrição da ordem de manutenção, mas não observa diretamente todas as condições físicas da máquina nem todas as possíveis causas de falha. |
| Número de agentes | Multiagente | Técnicos de manutenção e supervisores de segurança também tomam decisões e suas ações podem alterar o resultado da intervenção. |
| Determinismo | Estocástico | O AMS utiliza um modelo de linguagem, cuja resposta pode variar; assim, a mesma entrada não garante necessariamente a mesma resposta. |
| Episódico ou sequencial | Episódico | Cada ordem de manutenção é avaliada como um caso independente, sem que a saída de uma avaliação determine automaticamente a próxima. |
| Estático ou dinâmico | Dinâmico | No ambiente industrial pretendido, as condições das máquinas podem mudar durante a análise, tornando uma leitura anterior desatualizada. |
| Discreto ou contínuo | Contínuo | O ambiente inclui grandezas físicas representadas por valores numéricos de sensores, embora as saídas do AMS (como os níveis de risco e a indicação de LOTO) sejam discretas. |
| Conhecido ou desconhecido | Desconhecido | Embora as normas aplicáveis sejam documentadas, o agente não conhece completamente as relações entre os sinais dos sensores, as falhas e suas consequências operacionais. Essas relações precisam ser investigadas e validadas. |

---

## 3. Tipo de agente pretendido

<!--
Perguntas que guiam o preenchimento:
  - O tipo mais simples que resolve: reativo simples, baseado em modelo,
    baseado em objetivos, baseado em utilidade, ou um deles com aprendizagem?
  - A caracterização do ambiente (seção 2) é que decide — parcialmente
    observável elimina o reativo simples.
  - Justifique com a dimensão do ambiente que obriga a escolha.
-->
**Quando se preenche:** no sábado (encontro 2, manhã).

Tipo de agente pretendido: Baseado em objetivos

O AMS deve avaliar ordens de manutenção para apoiar decisões seguras, considerando o risco de falha, as normas potencialmente aplicáveis e a necessidade de bloqueio e etiquetagem (LOTO). A dimensão que justifica essa escolha é a observabilidade parcial: as leituras dos sensores e a descrição da intervenção não revelam necessariamente todas as condições reais da máquina. Por isso, o agente deve considerar o objetivo da avaliação, reconhecer quando as informações disponíveis são insuficientes, comunicar a incerteza e encaminhar casos críticos ou inconclusivos para avaliação humana. Um agente reativo simples, baseado apenas em regras fixas de percepção e ação, não seria suficiente para lidar com essas limitações.

---

## 4. Medida de desempenho

<!--
Perguntas que guiam o preenchimento:
  - A fórmula do campo Performance, agora em definição operacional: como se
    calcula, passo a passo, sem ambiguidade?
  - Métrica primária e duas secundárias. As secundárias pegam o que a
    primária deixa passar (custo, latência, reabertura).
  - O conjunto de casos: de onde vêm, quantos são, de que período.
  - O limiar de sucesso e o valor de hoje.
-->
**Quando se preenche:** a fórmula começa no Laboratório 01; a definição
operacional completa — primária, secundárias, conjunto e limiar — é item da
Atividade Orientada.

[métrica primária e duas secundárias, cada uma com: como se
calcula, sobre qual conjunto, qual o limiar de sucesso e o
valor de hoje]

---

## 5. Restrições

<!--
Perguntas que guiam o preenchimento:
  - Latência aceitável: a partir de quantos segundos a resposta perde valor?
  - Custo aceitável: quanto pode custar cada resolução?
  - O que o agente jamais pode fazer: a ação irreversível que ele não executa
    sozinho. Esta linha vira política em D12.
  - A troca que você aceita: quanto de qualidade por quanto de custo?
    Um exemplo numérico vale mais que a fórmula.
-->
**Quando se preenche:** na Atividade Orientada.

[latência aceitável, custo aceitável, o que o agente jamais
pode fazer sozinho, e a troca qualidade × custo que você
aceita — com um exemplo numérico]

---

## 6. Catálogo de ações

<!--
Perguntas que guiam o preenchimento:
  - Para cada ação: nome, entrada, saída, é reversível?, qual o risco se
    errar? As quatro colunas são obrigatórias — "baixo" não é descrição de
    risco.
  - Ações de leitura E de escrita: as que alteram o mundo são as que exigem
    cuidado, e são justamente as que mais ficam de fora.
  - Este catálogo é, literalmente, a lista de ferramentas que você vai
    implementar em D10.
-->
**Quando se preenche:** na Atividade Orientada — o pacote `D2-07` traz o
modelo da tabela e dois exemplos preenchidos.

[tabela: nome · entrada · saída · é reversível? · risco se
errar — ações de leitura e de escrita]

---

## 7. Formulação por busca

<!--
Perguntas que guiam o preenchimento:
  - Qual fatia do problema é um espaço de estados? (Nem toda fatia é — e
    reconhecer a que não é vale tanto quanto formular a que é.)
  - Os cinco componentes: estado inicial, ações, modelo de transição, teste
    de objetivo, custo de caminho. Comece pelo teste de objetivo.
  - O estado descreve a SITUAÇÃO, não a história de como se chegou nela.
-->
**Quando se preenche:** no Laboratório 03, sábado à tarde (encontro 2).

[os cinco componentes do seu subproblema: estado inicial,
ações, transição, teste de objetivo, custo]
