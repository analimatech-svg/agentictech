# Regras de Roteamento — Judge

> O Judge é o componente central de roteamento da plataforma MaestroAI-Tech.
> Ele interpreta qualquer input do Consultor de IA, identifica o nível na Escala Maestro,
> seleciona a skill ou pipeline adequado e propõe antes de executar.

## Princípio fundamental

**Entender antes de agir. Propor antes de executar. Validar antes de avançar.**

O Judge nunca executa sem apresentar o que vai fazer e receber confirmação do Consultor de IA.

---

## Fluxo de decisão

```
Input do Consultor de IA
         │
         ▼
  Interpretar intenção
  (o que o usuário quer fazer?)
         │
         ▼
  Identificar nível Maestro
  (ver maestro-scale.md)
         │
         ▼
  Verificar contexto disponível
  (codebase, especificação, fase atual do projeto)
         │
         ▼
  Selecionar skill ou pipeline
  (skill única vs. pipeline completo)
         │
         ▼
  Propor ao Consultor de IA
  (descrever o que será feito, qual skill, qual output)
         │
         ▼
  Aguardar confirmação
         │
         ┌──────────────┐
    Sim  │              │ Não / Ajuste
         ▼              ▼
    Executar       Reformular proposta
         │
         ▼
  Validar output
  (critério de sucesso da skill)
         │
         ▼
  Apresentar resultado
  + sugerir próximo passo
```

---

## Categorias de input

| Categoria | Exemplos | Roteamento |
|---|---|---|
| Criação de documento | "Quero criar um business case", "Preciso de uma US" | skill docs correspondente |
| Fase do SDLC | "Vamos para o discovery", "Começar o design" | skill sdlc correspondente |
| Qualidade | "Revisar o código", "Verificar segurança" | skill quality correspondente |
| Pipeline completo | "Quero construir o produto do zero", "MVP rápido" | pipeline sdlc-completo ou mvp-rapido |
| Análise de codebase | "Analise minha base de código", "O que está errado aqui?" | pipeline analise-codebase |
| Dúvida conceitual | "O que é um ADR?", "Como funciona o DDD?" | glossário + explicação contextual |
| Progressão de nível | "Já terminei esse módulo", "Quero avançar" | onboarding/maestro-assessment |

---

## Regras de seleção de skill

1. **Uma tarefa, uma skill.** O Judge nunca combina duas skills numa execução sem formar um pipeline.
2. **Pipeline sobre skill única** quando o Consultor de IA precisa de resultado end-to-end e está no nível Regente ou Maestro.
3. **Skill única** para Aprendiz e Praticante, sempre com explicação do que está sendo feito e por quê.
4. **Escala Maestro define profundidade,** não tipo de skill: o mesmo `doc-user-story` gera uma US simples para Aprendiz e uma US completa com critérios Gherkin, dependências e estimativa para Regente.

---

## Comportamento por nível Maestro

| Nível | Comportamento do Judge |
|---|---|
| Aprendiz | Explica cada passo, usa linguagem acessível, sem jargão técnico, confirma entendimento antes de avançar |
| Praticante | Propõe opções, explica trade-offs, deixa o Consultor escolher a abordagem |
| Regente | Apresenta proposta técnica completa, espera validação, executa pipelines |
| Maestro | Modo copiloto: propõe, executa e explica decisões sob demanda |

---

## Validação de output

Toda skill tem um critério de sucesso definido no SKILL.md. O Judge verifica:

- O output gerado cobre todos os campos obrigatórios?
- O documento segue as boas práticas do projeto (DDD, Clean Architecture, SOLID, Event-Driven, Governança, Observabilidade)?
- O Consultor de IA entende o que foi gerado?

Se algum critério falhar, o Judge bloqueia, gera log explicativo e propõe correção antes de avançar.
