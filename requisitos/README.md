# Engenharia de Requisitos para Desenvolvedores

## Introdução

Este documento apresenta uma metodologia prática para refinar e validar requisitos de software, visando minimizar custos, reduzir retrabalho e entregar soluções que atendam às necessidades reais dos stakeholders.

## Por que investir em Requisitos?

- Requisitos bem especificados reduzem significativamente os custos de mudança
- Mudanças na fase de requisitos são muito mais baratas que na fase de implementação
- Estudos mostram que a falta de especificação clara é a principal causa de problemas em projetos de software
- Requisitos bem refinados geram entregas mais assertivas, com menos falhas

## Método de Refinamento de Requisitos

A metodologia proposta segue um processo em 5 passos, inspirado no conceito de "Shape Up" da Basecamp:

### Passo 1: Buscar acesso à fonte original

- Converse diretamente com quem idealizou o requisito (stakeholder)
- A fonte mais confiável é sempre quem teve a ideia inicialmente
- Estabeleça um canal de comunicação direto, quando possível

### Passo 2: Analisar artefatos de requisitos

- Leia a documentação disponível sobre o requisito
- Se não for possível acessar o idealizador, procure quem escreveu o documento
- Utilize a técnica dos "5 porquês" para entender a causa raiz do problema
- Crie seu próprio artefato de requisitos baseado no seu entendimento

**Importante**: Nunca comece a codificar sem ter certeza do que deve ser feito.

### Passo 3: Double Check e Materialização

**3.1 Double Check**
- Valide seu entendimento com quem idealizou o requisito
- Evite aprendizagem negativa (aprender de forma incorreta)
- Se não puder validar, tenha consciência dos riscos

**3.2 Materialização do Entendimento**
- Utilize técnicas de visualização para concretizar o entendimento:
  - **Breadboarding**: Crie um fluxo escrito simples que simbolize o caminho do requisito
  - **Fat Marker Sketch**: Faça desenhos simples agrupando funcionalidades

### Passo 4: Testar cenários

- Utilize ferramentas como Cucumber para descrever cenários de teste
- IA generativa pode ajudar como ponto de partida (não de chegada)
- Foque em testes end-to-end que validem o valor entregue
- Gere valor com o mínimo de trabalho possível

### Passo 5: Descrever a implementação

"Se você não for capaz de imaginar, como você vai ser capaz de implementar?"

- Crie um passo a passo técnico detalhado
- Desenvolva histórias de usuário (Como um... eu quero... para que...)
- Estabeleça critérios claros de aceitação

## Template para Documentação de Requisitos

Um bom documento de requisitos deve conter:

1. Contexto e problema a ser resolvido
2. Solução proposta
3. Fluxo de interação (Breadboarding)
4. Protótipo visual (Fat Marker Sketch)
5. Histórias de usuário
6. Critérios de aceitação
7. Descrição técnica da implementação
8. Cenários de teste

## Tipos de Requisitos

- **Requisitos Funcionais**: Possuem entrada e saída definidas, representam funcionalidades
- **Requisitos Não-Funcionais**: Restrições como tempo de resposta, disponibilidade, escalabilidade

## Benefícios da Abordagem

Para o desenvolvedor:
- Maior clareza sobre o que deve ser implementado
- Minimização de retrabalho
- Melhor percepção das necessidades reais

Para a empresa:
- Redução de custos de desenvolvimento
- Entregas mais assertivas
- Maior satisfação dos clientes

## Como Exercitar

Pratique entender completamente o que deve ser feito antes de começar a codificar. Adote o "método shape up" em seus projetos, mesmo que sua empresa não o utilize formalmente.

---

## Referências

- Shape Up (Basecamp): https://basecamp.com/shapeup
- Software Engineering (Sommerville)
- Diversos artigos acadêmicos sobre Engenharia de Requisitos Ágil
