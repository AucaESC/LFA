# Aula 05 — Expressões Regulares

## Material do estudante — versão com gabarito

**Disciplina:** Linguagens Formais e Autômatos
**Curso:** Engenharia de Software
**Profª:** Kadidja Valéria
**Atividade avaliativa:** reconhecimento e construção de padrões — **0,5 ponto**

> Este documento preenche apenas as lacunas de produção do estudante (seções 8, 9, 10 e 11 do material original), combinando o raciocínio das duas tentativas de resolução em uma única resposta consolidada.

---

## Seção 8 · Exercício guiado

Para cada item, o raciocínio segue o processo pedido no roteiro: **alfabeto → blocos → escolhas/ordem/repetição → exemplos → expressão.**

### 🟦 Exercício 1 — Sufixo `00`

| Etapa | Resposta |
|---|---|
| Alfabeto | \(\Sigma=\{0,1\}\) |
| Blocos | um prefixo livre de qualquer tamanho + um sufixo fixo `00` |
| Escolha/ordem/repetição | o prefixo repete livremente `0` e `1` em qualquer ordem (fecho de Kleene); o sufixo é literal e não admite variação |
| Aceita | `00`, `100`, `0100`, `11100`, `0000` |
| Rejeita | \(\varepsilon\), `0`, `01`, `101`, `001` |

**Sua expressão:**
```regex
^[01]*00$
```

O `[01]*` cobre o mesmo papel que `(0|1)*` — é apenas uma notação mais compacta para "zero ou mais símbolos do alfabeto". As âncoras `^…$` garantem que os dois últimos caracteres da *palavra inteira* sejam `00`, e não apenas de um trecho dela.

---

### 🟦 Exercício 2 — Exatamente dois `a`

| Etapa | Resposta |
|---|---|
| Alfabeto | \(\Sigma=\{a,b\}\) |
| Blocos | três regiões de `b` (antes do 1º `a`, entre os dois `a`, depois do 2º `a`) intercaladas com dois `a` fixos |
| Escolha/ordem/repetição | cada região de `b` é `b*`; os dois `a` aparecem exatamente uma vez cada, em ordem fixa — não há alternância nem opcionalidade sobre eles |
| Aceita | `aa`, `aba`, `baab`, `aabbb`, `bbabbab` |
| Rejeita | \(\varepsilon\), `bbb` (zero `a`), `a` (um `a`), `aaa` (três `a`), `bababa` (três `a`) |

**Sua expressão:**
```regex
^b*ab*ab*$
```

O ponto-chave é que apenas dois `a` estão escritos explicitamente na expressão — como não existe `*` ou `+` sobre nenhum deles, é estruturalmente impossível que um terceiro `a` seja aceito.

---

### 🟦 Exercício 3 — Identificador acadêmico

| Etapa | Resposta |
|---|---|
| Blocos | 2 letras maiúsculas + 3 algarismos + 1 letra minúscula opcional |
| Escolha/ordem/repetição | as duas primeiras posições são fixas em quantidade (`{2}`), os três algarismos também (`{3}`); a letra final tem cardinalidade 0 ou 1 (`?`) |
| Aceita | `AB123`, `ZX007`, `RT999a`, `XY000m` |
| Rejeita | `A123` (1 maiúscula), `ABC123` (3 maiúsculas), `AB12` (2 algarismos), `AB123xy` (2 minúsculas), `AB123M` (opcional deve ser minúscula) |

**Sua expressão:**
```regex
^[A-Z]{2}[0-9]{3}[a-z]?$
```

As âncoras impedem que sobrem caracteres antes ou depois — sem elas, `AB123XYZ999` poderia ser parcialmente reconhecida por um motor que aceita correspondência de trecho.

---

## Seção 9 · Desafio final — Código de matrícula acadêmica

### Regex final

```regex
^(CCO|ESW|SIS)-202[4-9]-[0-9]{4}-[MTN]$
```

*(equivalente com grupo não capturante, caso o motor exija: `^(?:CCO|ESW|SIS)-202[4-9]-[0-9]{4}-[MTN]$`)*

### Justificativa por blocos

| # | Bloco | Regex | O que garante |
|---:|---|---|---|
| 1 | início | `^` | nada é aceito antes do primeiro caractere |
| 2 | curso | `(CCO\|ESW\|SIS)` | somente as três siglas, em maiúsculas, sem variação |
| 3 | separador | `-` | hífen obrigatório entre curso e ano |
| 4 | ano | `202[4-9]` | fixa a centena/dezena `202` e restringe a unidade a `4–9`, cobrindo exatamente 2024–2029 |
| 5 | separador | `-` | hífen obrigatório entre ano e número |
| 6 | número | `[0-9]{4}` | exatamente quatro algarismos, nem mais nem menos |
| 7 | separador | `-` | hífen obrigatório entre número e turno |
| 8 | turno | `[MTN]` | apenas um caractere entre as três opções válidas |
| 9 | fim | `$` | nada é aceito depois do turno |

### Dois novos casos válidos

1. `SIS-2028-0456-T` — curso, ano, número e turno dentro das regras.
2. `ESW-2024-7000-M` — testa o ano no limite inferior combinado com outro curso.

### Dois novos casos inválidos

1. `CCO-2024-00001-M` — cinco algarismos no bloco numérico, violando o `{4}` exato.
2. `CCO-2024-0001-m` — turno em minúscula; `[MTN]` é sensível a maiúsculas/minúsculas.

### Perguntas para justificar

**1. Qual subexpressão representa a escolha entre cursos?**
`(CCO|ESW|SIS)` — um grupo de alternância que restringe a entrada a exatamente essas três palavras, rejeitando qualquer outra combinação de letras, como `ADS` ou `esw`.

**2. Como o intervalo de anos foi limitado sem aceitar `2030`?**
Fixando os três primeiros algarismos como `202` e restringindo apenas o último dígito ao intervalo `[4-9]`. Para chegar a `2030` seria necessário alterar o dígito das dezenas (de `2` para `3`), posição que a expressão trata como literal e não permite variar.

**3. Por que `{4}` é diferente de `+` no bloco numérico?**
`{4}` exige exatamente quatro repetições — nem menos, nem mais. Já `+` significa "uma ou mais", sem limite superior, o que aceitaria indevidamente números como `1` ou `12345`.

**4. Qual é a função das âncoras?**
`^` e `$` obrigam que a correspondência cubra a cadeia inteira, do primeiro ao último caractere, impedindo que prefixos ou sufixos extras (como um espaço, uma letra a mais, ou outro código colado à matrícula) sejam aceitos.

**5. Sua expressão aceita alguma cadeia que viola as regras?**
Não. Cada bloco está fechado: o curso é restrito a três literais, o ano a seis valores possíveis, o número a exatamente quatro dígitos, o turno a três literais, os separadores são hífens fixos, e as âncoras impedem qualquer excedente. A bateria de testes da próxima seção — incluindo casos de fronteira e "quase corretos" — não encontra nenhuma entrada inválida que passe pela expressão.

---

### Desafio extra — DFA equivalente

**Estados por progresso de bloco:** o autômato pode ser descrito como um "trilho" sequencial: leitura do curso (três ramos distintos, um por palavra `CCO`/`ESW`/`SIS`, já que cada sigla é lida letra a letra) → estado pós-primeiro-hífen → quatro estados para os dígitos do ano (os três primeiros validam exatamente `2`, `0`, `2`; o quarto ramifica entre `4` e `9`) → estado pós-segundo-hífen → quatro estados para os dígitos do número (cada um aceita qualquer símbolo de `[0-9]`) → estado pós-terceiro-hífen → um estado que ramifica em `M`/`T`/`N` → **estado de aceitação**.

**Onde ocorrem as ramificações:**
- na primeira letra do curso (`C` → rumo a `CCO`, `E` → rumo a `ESW`, `S` → rumo a `SIS`);
- no último dígito do ano (`4` a `9` seguem para o próximo bloco, qualquer outro dígito vai para o sumidouro);
- no caractere de turno (`M`, `T` ou `N` levam à aceitação; qualquer outro símbolo vai para o sumidouro).

**Único estado de aceitação:** o estado alcançado imediatamente após a leitura de um turno válido na última posição da cadeia.

**Por que caracteres extras levam à rejeição:** o estado de aceitação não possui nenhuma transição de saída válida — qualquer símbolo lido a partir dele (um caractere a mais depois do turno) cai no estado sumidouro, do qual não existe caminho de volta à aceitação. O mesmo vale para qualquer símbolo fora do esperado em qualquer posição da cadeia (letra errada no curso, dígito fora de `4–9` no ano, separador diferente de `-`, turno inválido).

---

## Seção 10 · Registro dos testes — Regex Learn

| Entrada | Esperado | Obtido | Regra verificada |
|---|---|---|---|
| `CCO-2024-0001-M` | aceita | aceita | curso, limite inferior do ano e formato geral |
| `ESW-2026-1042-N` | aceita | aceita | formato geral |
| `SIS-2029-9999-T` | aceita | aceita | limite superior do ano e do número |
| `CCO-2027-0100-N` | aceita | aceita | número com zero à esquerda |
| `ESW-2025-4321-M` | aceita | aceita | formato geral |
| `ADS-2026-0001-N` | rejeita | rejeita | curso inexistente |
| `CCO-2030-0001-M` | rejeita | rejeita | ano fora do intervalo |
| `SIS-2027-123-N` | rejeita | rejeita | número com apenas três algarismos |
| `esw-2026-1042-N` | rejeita | rejeita | curso em minúsculas |
| `CCO/2026/0001/M` | rejeita | rejeita | separador incorreto |
| `CCO-2026-0001-X` | rejeita | rejeita | turno inexistente |
| caso criado 1 — `CCO-2024-0000-M` | aceita | aceita | ano no limite inferior exato |
| caso criado 2 — `SIS-2029-9999-N` | aceita | aceita | ano no limite superior exato |
| caso criado 3 — `CC0-2026-1234-M` | rejeita | rejeita | "quase correto" — zero no lugar da letra `O` |
| caso criado 4 — `CCO-2026-1234-MX` | rejeita | rejeita | caractere extra depois do turno |

---

## Seção 11 · Perguntas de reflexão

**1. Toda expressão regular formal representa uma linguagem regular?**
Sim. A definição clássica constrói expressões apenas por união, concatenação e fecho de Kleene — operações que, por construção, preservam a regularidade. Logo, qualquer expressão obtida dessa forma denota necessariamente uma linguagem regular.

**2. Toda linguagem regular pode ser representada por uma expressão regular?**
Sim, na direção inversa do mesmo resultado. O Teorema de Kleene estabelece a equivalência entre expressões regulares e autômatos finitos: se uma linguagem é reconhecida por um DFA ou NFA, existe uma expressão regular que a descreve, e vice-versa.

**3. Qual é a relação entre uma ER, um NFA e um DFA?**
Os três formalismos têm exatamente o mesmo poder de reconhecimento: `Regex ⇔ NFA ⇔ DFA`. É possível converter uma expressão regular em um NFA (construção de Thompson), um NFA em um DFA (construção de subconjuntos) e um DFA de volta em uma expressão regular. As estruturas diferem, mas a classe de linguagens reconhecida é sempre a mesma — as linguagens regulares.

**4. Qual é a diferença entre uma expressão regular teórica e as extensões de motores de programação?**
A ER formal admite apenas três operações — união, concatenação e estrela de Kleene — com poder expressivo idêntico ao de autômatos finitos. Motores de regex usados em linguagens de programação (PCRE e similares) acrescentam recursos como backreferences, lookahead/lookbehind, grupos condicionais e até recursão. Alguns desses recursos são só conveniência sintática; outros — sobretudo backreferences e recursão — permitem reconhecer padrões que vão além do que qualquer autômato finito reconhece, deixando de ser "regular" no sentido formal.

**5. Por que um autômato finito reconhece paridade, mas não consegue contar arbitrariamente e comparar duas quantidades sem limite?**
Reconhecer paridade exige lembrar apenas um bit de informação (par/ímpar), o que cabe perfeitamente em um número finito e fixo de estados. Já contar e comparar quantidades sem limite superior exigiria memória proporcional ao tamanho da entrada — e um DFA nunca tem mais estados do que os definidos previamente, então não há como ele "guardar" uma contagem que cresce indefinidamente.

**6. Por que \(\{a^nb^n \mid n\geq0\}\) não é regular?**
Suponha, por absurdo, um DFA hipotético com \(k\) estados que reconheça essa linguagem. Ao processar a palavra \(a^kb^k\), pelo princípio da casa dos pombos, dois prefixos distintos de `a` levariam necessariamente ao mesmo estado. Isso permite "bombear" (repetir) um trecho de `a` sem alterar a quantidade correspondente de `b`, produzindo uma palavra fora da linguagem mas ainda aceita pelo autômato — uma contradição. Esse é exatamente o argumento formalizado pelo Lema do Bombeamento (e também demonstrável pelo Teorema de Myhill-Nerode). Na prática, o problema é que um DFA precisaria "lembrar" quantos `a` já leu para depois exigir a mesma quantidade de `b`, e isso demandaria memória ilimitada — algo que uma quantidade finita de estados não oferece.

**7. O que muda ao passarmos de linguagens regulares para linguagens livres de contexto?**
Ganha-se uma pilha: memória em princípio ilimitada, porém de acesso restrito (LIFO — o último elemento empilhado é o primeiro a ser desempilhado). Os autômatos correspondentes, os autômatos de pilha (PDA), conseguem empilhar um símbolo para cada `a` lido e desempilhar um a cada `b`, comparando as quantidades sem jamais precisar de um número fixo de estados para isso — exatamente a capacidade que falta a um DFA, cuja única "memória" é o próprio conjunto finito de estados.

---

## Resumo consolidado das expressões

| Exercício | Regex |
|---|---|
| Binárias que terminam em `00` | `^[01]*00$` |
| Exatamente dois `a` sobre `{a,b}` | `^b*ab*ab*$` |
| Identificador (2 maiúsculas + 3 algarismos + minúscula opcional) | `^[A-Z]{2}[0-9]{3}[a-z]?$` |
| Matrícula acadêmica | `^(CCO\|ESW\|SIS)-202[4-9]-[0-9]{4}-[MTN]$` |
