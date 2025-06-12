# Infraestrutura e Plataforma

## Cloud Native e Self-Service

O serviço deve ser **efêmero**, ou seja, não deve ser persistente. A ideia é que ele possa ser criado e destruído facilmente, sem perda de dados ou funcionalidade.

* Os aplicativos devem estar **"prontos para a nuvem"**
* Eles precisam ser:

  * Escaláveis horizontalmente
  * **Independentes de local**
  * Efêmeros
  * Replicáveis
* O verdadeiro poder da nuvem não está apenas na infraestrutura ilimitada, mas nas **APIs** disponíveis
* **Multi-nuvem?**

  * Usar uma infraestrutura agnóstica como serviço pode ser útil
  * Mas **não tenha medo** de utilizar as ferramentas nativas e poderosas que seu provedor de nuvem oferece

## Times de Plataforma

* As equipes não devem reinventar a roda em questões como:

  * Dados
  * Segurança
  * Desempenho
  * Bootstrapping
  * Configuração
* Existe um **trade-off** entre ter uma única plataforma padronizada e manter espaço para inovação
* Uso de **radar tecnológico** e **ADRs (Architecture Decision Records)** para registrar decisões técnicas
* Construir "em casa" vs. adotar estruturas prontas deve ser uma decisão consciente

> O time de plataforma é responsável por criar um ambiente de desenvolvimento robusto.
> No caso de Java, por exemplo, geralmente é esse time que fornece um projeto inicial (bootstrapping) com dependências e classes comuns, como módulos de segurança, logging e configuração.

O time de plataforma também pode definir quais tecnologias são aprovadas ou bloqueadas — por exemplo, quais linguagens (Java, Go, Rust etc.) estão disponíveis para uso nas equipes de produto.

## Infraestrutura como Enabling Teams

* Equipes de infraestrutura **não devem atuar apenas de forma reativa**, apagando incêndios
* Elas devem **habilitar** os times de produto a **construírem sua própria infraestrutura**

  * Essa mudança é especialmente desafiadora à medida que a empresa escala
  * Traz ganhos por **economia de escala**
* **Infraestrutura self-service** é um objetivo-chave

> Times de infraestrutura oferecem **suporte e ferramentas**, não fazem o trabalho pelo time de produto.
> Por exemplo:
> Um time de integração contínua fornece a ferramenta (CI pipeline), mas **não configura os jobs por você**.
> Um time de segurança fornece scanners, linters e guidelines, mas **não escreve o código seguro por você**.

