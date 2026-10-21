# Atividade sobre Agentes Inteligentes

**Disciplina:** Inteligência Artificial

**Professora:** Kadidja Valéria

**Estudante:** Cauã Eduardo Silva

**Turma:** Ciência da Computação

**Aula:** 20/10/2026

---

## Exercício 2.3

Para cada afirmação: julgamento (V/F) e justificativa.

**a. Falso.**
Racionalidade não exige saber tudo. Exige escolher a melhor ação com base no que o agente percebe. Com informação parcial, ele ainda pode pesar as probabilidades e escolher a ação de maior utilidade esperada.

**b. Verdadeiro.**
O agente reativo puro age só com a percepção atual e não tem memória. Se duas situações diferentes geram a mesma percepção, mas pedem ações diferentes, ele erra ou fica repetindo a mesma ação.

**c. Verdadeiro.**
Se a medida de desempenho dá a mesma pontuação para qualquer ação, nenhuma escolha é melhor que outra. Assim, todo agente é racional.

**d. Falso.**
A função de agente recebe a sequência completa de percepções. O programa de agente recebe só a percepção atual e guarda em variáveis o que precisa do passado.

**e. Falso.**
Nem toda função é computável. Uma função que resolvesse o Problema da Parada, por exemplo, não pode ser implementada em nenhum computador.

**f. Verdadeiro.**
Se todas as ações levam ao mesmo resultado ótimo, escolher ao acaso é tão bom quanto qualquer outra escolha.

**g. Verdadeiro.**
Se dois ambientes exigem o mesmo comportamento para dar o melhor resultado, o mesmo agente pode ser racional nos dois.

**h. Falso.**
Mesmo sem sensores, as ações continuam afetando o resultado. Um agente que usa um plano bem pensado é racional, e um que age de forma desordenada não é.

**i. Falso.**
Racionalidade maximiza o resultado esperado, mas não elimina a sorte nem as cartas ocultas. O agente pode fazer a melhor jogada e ainda assim perder uma mão.

---

## Exercício 2.4

### Caso 1 — Jogar futebol

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Outros robôs bloqueiam a visão das câmeras. |
| Número de agentes | Multiagente (misto) | Coopera com o time e compete com os adversários. |
| Previsibilidade | Estocástico | Escorregões, rebotes e decisões dos rivais geram incerteza. |
| Dependência | Sequencial | Cada jogada muda a situação das seguintes. |
| Mudança | Dinâmico | A bola e os jogadores se mexem enquanto o robô decide. |
| Natureza | Contínuo | Velocidade, ângulos e tempo variam continuamente. |
| Conhecimento | Conhecido | As regras e a física da bola são conhecidas. |

### Caso 2 — Explorar os oceanos subterrâneos de Titã

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Sem luz e com sonar de alcance limitado, a sonda só vê o que está perto. |
| Número de agentes | Agente único | Correntes e fenômenos naturais não são outros agentes. |
| Previsibilidade | Estocástico | Turbulências e jatos térmicos são imprevisíveis. |
| Dependência | Sequencial | A energia gasta agora limita as rotas futuras. |
| Mudança | Dinâmico | Correntes e temperatura mudam durante a decisão. |
| Natureza | Contínuo | Pressão, profundidade e posição variam continuamente. |
| Conhecimento | Desconhecido | O oceano de Titã ainda precisa ser descoberto pela exploração. |

### Caso 3 — Comprar livros usados de IA na Internet

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | O estado real do livro e a concorrência não aparecem no anúncio. |
| Número de agentes | Multiagente competitivo | Outros compradores disputam o mesmo exemplar. |
| Previsibilidade | Estocástico | O item pode ser vendido a outra pessoa na hora da compra. |
| Dependência | Sequencial | O dinheiro gasto em um livro reduz o que sobra para outras ofertas. |
| Mudança | Semidinâmico | Os anúncios não mudam durante o cálculo, mas podem expirar se demorar. |
| Natureza | Discreto | Títulos, preços e notas são valores discretos. |
| Conhecimento | Conhecido | As regras de compra e pagamento são conhecidas. |

### Caso 4 — Jogar uma partida de tênis

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | O robô vê a bola, mas não a rotação exata nem a estratégia do adversário. |
| Número de agentes | Multiagente competitivo | O oponente tenta ativamente fazer o robô errar. |
| Previsibilidade | Estocástico | Irregularidades da quadra, vento e impacto nas cordas alteram o resultado. |
| Dependência | Sequencial | Uma devolução ruim prejudica a posição para o golpe seguinte. |
| Mudança | Dinâmico | A bola se move rápido enquanto o robô calcula o golpe. |
| Natureza | Contínuo | Posição, velocidade e torque variam continuamente. |
| Conhecimento | Conhecido | As regras do jogo e a física da bola são conhecidas. |

### Caso 5 — Praticar tênis contra uma parede

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Pequenos detalhes da bola e da parede não podem ser medidos. |
| Número de agentes | Agente único | A parede é um objeto passivo, não toma decisões. |
| Previsibilidade | Estocástico | É bem previsível, mas ainda há pequenas variações no quique. |
| Dependência | Sequencial | Cada rebatida define como será a próxima. |
| Mudança | Dinâmico | A bola volta sem parar e atrasos quebram a sequência. |
| Natureza | Contínuo | Espaço, velocidade e aceleração são contínuos. |
| Conhecimento | Conhecido | As leis de reflexão e de impacto são conhecidas. |

### Caso 6 — Realizar um salto de altura

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Completamente observável | O sarrafo, a pista e a área de impulsão são fixos e bem medidos. |
| Número de agentes | Agente único | O salto é individual, sem outros agentes interferindo. |
| Previsibilidade | Determinístico | Em ginásio fechado, sem vento, a física do salto é previsível. |
| Dependência | Sequencial | A corrida de aproximação define a energia do salto. |
| Mudança | Dinâmico | O robô precisa agir em milissegundos enquanto já está em movimento. |
| Natureza | Contínuo | Distâncias, velocidades e torques variam continuamente. |
| Conhecimento | Conhecido | O movimento é descrito por equações físicas conhecidas. |

### Caso 7 — Participar de um leilão

| Propriedade | Classificação | Justificativa |
|---|---|---|
| Observabilidade | Parcialmente observável | Os lances são públicos, mas o limite de cada rival não. |
| Número de agentes | Multiagente competitivo | Os participantes disputam o mesmo item. |
| Previsibilidade | Estocástico | Os lances finais dos rivais não podem ser previstos com certeza. |
| Dependência | Sequencial | Um lance antecipado pode mudar a estratégia dos concorrentes. |
| Mudança | Dinâmico | O cronômetro corre enquanto o agente decide. |
| Natureza | Discreto | Os lances sobem em incrementos fixos. |
| Conhecimento | Conhecido | As regras do leilão são definidas previamente. |
