# Resumo de Notas: Qualidade de Software e Design de Código

Este documento apresenta um resumo estruturado de anotações sobre qualidade de software, design de código e boas práticas de desenvolvimento.

## 📋 Índice
- [Qualidade de Software](#qualidade-de-software)
- [Design de Código](#design-de-código)
- [Guidelines e Métricas](#guidelines-e-métricas)

## Qualidade de Software

### Fundamentos
- **Qualidade vai além das práticas**: Um software de qualidade não apenas segue práticas, mas faz o que deve fazer e pode ser mantido.
- **Criticidade constante**: Seja sempre crítico e adote princípios que façam sentido para sua equipe.

### Fatores que Impactam o Tempo de Desenvolvimento
- **Fluidez**: Domínio da IDE, das dependências do projeto e das tecnologias utilizadas.
- **Pre-work**: Análise detalhada do problema antes de iniciar a implementação.
- **Conhecimento técnico**: Domínio das tecnologias utilizadas no projeto (ex: Hibernate, Spring).
- **Acoplamento mental**: Compreensão clara do funcionamento interno do código além da sintaxe.

### Requisitos e Negócio
- **Visão de negócio**: "Triturar" os requisitos desde a concepção e fazer double-check com stakeholders.
- **Aprimoramento de domínio**: Criar um plano de aprendizagem do domínio do negócio gera valor para o desenvolvimento.
- **Vocabulário e contexto**: Entendimento do negócio ajuda a compreender a nomenclatura usada no código e seu contexto.

## Design de Código

### Arquitetura vs Design
- **Arquitetura**: Decisões mais estáveis que respeitam atributos de qualidade.
- **Design**: Decisões aplicadas no dia a dia que melhoram a experiência de desenvolvimento.

### Cognitive-Driven Development (CDD)
- Baseado na teoria da carga cognitiva, considerando a limitação da memória de trabalho.
- Permite mensurar a complexidade do código através de ICP (Intrinsic Complex Point).
- Cria indicadores dinâmicos de qualidade que se adaptam ao projeto.

### Princípios de Design
- **Open/Closed**: Buscar mecanismos para evoluir o código sem modificar o existente.
- **Acoplamento com frameworks**: Considerar o trade-off entre acoplamento e benefícios.
- **DTOs com métodos**: Padrões como toModel, toEntity, toDTO melhoram a usabilidade.
- **Camadas**: Verificar a real necessidade de múltiplas camadas para evitar indireções desnecessárias.
- **Coesão**: Manter junto o que faz sentido estar junto.
- **Postergação de generalização**: Esperar o máximo para generalizar, permitindo que a ideia amadureça.

### Testes e Qualidade
- **Testes de qualidade**: Simular o código em produção, preferencialmente com testes de integração.
- **Legado**: Abraçar o código legado como fonte de conhecimento e oportunidade de aprendizado.

## Guidelines e Métricas

### Métrica CDD
1. Condicional (if, loops, ternário): 1 ICP
2. Bloco de código (try, catch, case, funções como argumentos): 1 ICP
3. Acoplamento com classes específicas: 1 ICP

**Avaliação:**
- Projeto novo: limite de 10 ICPs por arquivo
- Legado: limite inicial de 50 ICPs

### Testes Automatizados
- Buscar cobertura de 90% ou mais
- Priorizar testes integrados
- Atenção à velocidade de execução
- Técnicas: MC/DC, Boundary Testing, Property Based Testing

### Logs
- Info: alterações de estado, chamadas a serviços externos
- Error: tratamentos que permitem continuidade do fluxo
- Debug: caminhos de decisão (com parcimônia)

### Padrões Recomendados
- Controller 100% coeso
- Services 100% coeso
- DTO como value object com métodos auxiliares

---

Estas notas representam um guia prático para desenvolvimento de software com qualidade, focando em aspectos técnicos e cognitivos que impactam a manutenibilidade e eficiência do código.
