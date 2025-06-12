# Vulnerabilidades Comuns

## Controle de Acesso, Identificação e Autenticação

* Siga o princípio do menor privilégio.
* Os usuários não devem conseguir obter mais permissões apenas "ajustando a solicitação".
* Usuários não devem conseguir fazer requisições para a API de atualização de permissões (`Update Permissions API`) utilizando um ID diferente do seu.
* A API deve restringir o acesso para todos os verbos HTTP, como `POST`, `PUT` e `DELETE`.
* Imponha limites dentro das APIs e das operações de negócio (rate limit).
* Limite e atrase tentativas consecutivas de login com falha. Monitore esses eventos.
* Projete um sistema de controle de acesso adequado desde o início do projeto.

## Falhas Criptográficas

* Dados confidenciais não devem ser transmitidos em texto não criptografado.
* Avalie se realmente é necessário armazenar esses dados; descarte-os sempre que possível.
* Certifique-se dos algoritmos utilizados para criptografar informações.
* Nunca implemente seu próprio algoritmo de criptografia.
* Utilize funções de geração de aleatoriedade recomendadas.
* Aplique HTTPS e mantenha certificados corretamente configurados.
* Permita apenas níveis fortes de criptografia.

## Injeção

* Atenção a:

  * SQL
  * Comandos de shell
  * OGNL
  * XML e JSON
  * Desserialização
* Nunca confie em dados fornecidos pelo usuário.

## Configuração Incorreta de Segurança

* Verifique se há nomes de usuários e senhas padrão em ferramentas administrativas ou dispositivos.
* Evite expor páginas da web, contas ou serviços desnecessários.
* Mensagens de erro e rastreamentos de pilha não devem ser retornados ao usuário final com detalhes sensíveis.
* Não armazene chaves de API ou credenciais confidenciais em texto simples ou em arquivos acessíveis a invasores ou malware.

## Componentes Vulneráveis

* Você tem controle de todos os componentes do sistema e sabe quais versões estão em execução?
* Verifique se algum software está vulnerável ou desatualizado.
* Atualize dependências, frameworks e plataformas com frequência adequada.
* Assegure-se de que as configurações desses componentes sejam seguras.
* Confirme que a biblioteca baixada é autêntica.
* Evite o uso de fontes não confiáveis para dependências.

## Quanto Mais Cedo, Melhor

* A segurança deve estar presente em todas as fases do desenvolvimento.
* Realize exercícios de modelagem de ameaças.
* Incorpore a segurança no dia a dia:

  * Nas reuniões diárias
  * Durante o desenvolvimento de novas funcionalidades
  * Durante os testes
  * Nas revisões de código
  * No monitoramento do sistema

