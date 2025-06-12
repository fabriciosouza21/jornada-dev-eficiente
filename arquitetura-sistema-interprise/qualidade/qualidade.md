# Qualidade

## Reúso de Código

* A reutilização de código é uma das coisas mais difíceis de acertar:

  * Compartilhar muito pouco leva à repetição e a mais bugs
  * Compartilhar demais gera acoplamento excessivo
* Crie bibliotecas ou frameworks para resolver **problemas comuns** — eles tendem a ser mais independentes.
  Tratar algo apenas como um "módulo separado" pode gerar **problemas de coesão e acoplamento**.
* Defina claramente **quem é o dono** do código reutilizado
* Aumentar a reutilização **entre módulos de diferentes aplicações** pode introduzir muitas dependências e dificultar a manutenção

> **Referência:** Sam Newman — *Building Microservices*
> *"Não reutilize código sem planejamento."*

## Dívida Técnica

* Todo mundo tem — você **não é exceção**
* Você não consegue tomar as melhores decisões quando **ainda entende pouco do problema**
* A dívida técnica te torna **mais lento no longo prazo**

### Problemas comuns:

* Modularização fraca
* Testes frágeis ou inexistentes
* Falta de documentação
* Falta de ownership claro
* Tecnologias obsoletas ou internas não mantidas

> A dívida técnica é inevitável. O importante é **como você lida com ela**.

* Lute contra o *Technical Debt (TD)* como parte do trabalho contínuo
* Evite grandes refatorações do tipo "big bang"
* Foco só no que é fácil pode esconder o que **realmente precisa ser resolvido**
* **Planos de teste e ciclos de refatoração** são essenciais

## Testando

**Testes rápidos são prioridade.**

* Esqueça a dicotomia *“teste de unidade vs. teste de integração”*
* A abordagem deve ser **dividir para conquistar**
* As suítes de testes devem fornecer **sinal**, não **ruído**
  (Evite testes instáveis ou *flaky tests*)
* **Teste de contrato** é essencial para sistemas distribuídos
