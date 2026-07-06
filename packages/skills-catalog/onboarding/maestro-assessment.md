# Assessment Inicial — Escala Maestro

> Avaliação diagnóstica executada pelo Judge na primeira sessão do Consultor de IA.
> O objetivo é identificar o nível real do usuário para adaptar linguagem e profundidade
> antes de apresentar qualquer framework técnico.

## Princípio

O Judge detecta, não pergunta diretamente. As perguntas são situacionais,
não autoavaliativas. O nível é determinado pelas respostas, não pela declaração do usuário.

---

## Sequência de avaliação

### Etapa 1: Situação inicial (obrigatória)

"Você tem um problema para resolver ou uma ideia para construir.
Por onde você começa?"

Opções apresentadas ao Consultor de IA:

1. Anoto o problema, converso com os envolvidos e defino o que o sistema precisa fazer antes de tocar no código
2. Pesquiso soluções parecidas, esboço uma arquitetura e começo a implementar
3. Abro o editor e começo a codar para entender o problema
4. Peço para a IA gerar o projeto inteiro e ajusto depois

**Interpretação:**
- Opção 1: indica maturidade em processo (Praticante ou acima)
- Opção 2: indica perfil técnico com alguma estrutura (Praticante)
- Opção 3: indica perfil hands-on sem processo (Aprendiz)
- Opção 4: indica curiosidade com IA mas sem processo (Aprendiz)

---

### Etapa 2: Contexto técnico (adaptativa)

Se opção 1 ou 2 na Etapa 1:

"Quando você decide a estrutura técnica de um projeto, o que considera primeiro?"

1. Domínio do negócio e regras que o sistema precisa respeitar
2. Performance e escalabilidade da solução
3. A stack que o time já conhece
4. O que a IA recomendar como melhor prática

**Interpretação:**
- Opção 1: alinhado com DDD (Regente ou Maestro)
- Opção 2 e 3: pragmático com alguma estrutura (Praticante)
- Opção 4: dependente de IA sem critério próprio (Aprendiz a Praticante)

---

### Etapa 3: Experiência com documentação (obrigatória)

"Qual desses documentos você já criou ou já viu funcionar num projeto real?"

- Business Case
- Architecture Decision Record (ADR)
- User Story com critérios Gherkin
- Runbook
- Postmortem
- Nenhum desses

**Interpretação:**
- 4 ou mais: Regente ou Maestro
- 2 a 3: Praticante
- 1 ou Nenhum: Aprendiz

---

## Resultado e próximo passo

Após as 3 etapas, o Judge combina as respostas e define o nível inicial:

| Padrão de respostas | Nível inicial | Primeiro passo recomendado |
|---|---|---|
| Processo claro + conhece documentos + critério de arquitetura | Regente | Pipeline sdlc-completo com autonomia aumentada |
| Algum processo + conhece 2 a 3 documentos | Praticante | Skills sdlc em sequência com assistência média |
| Sem processo definido + poucos documentos | Aprendiz | Glossário + skill planning com guia passo a passo |

O nível inicial não é definitivo. O Judge reavalia a cada 5 sessões ou quando o Consultor de IA concluir o critério de progressão do nível atual.

---

## Modo demonstração

Se o Consultor de IA ainda não configurou a chave de API, o assessment roda com dados fictícios
de um projeto de referência (sistema de gestão de reservas de espaços de coworking).
O nível é avaliado normalmente. A chave de API é solicitada apenas ao final, quando o Consultor
vai executar a primeira skill de verdade.
