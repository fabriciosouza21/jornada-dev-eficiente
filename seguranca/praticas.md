# Práticas

## Valide suas Entradas

Entradas não confiáveis são uma das maiores fontes de vulnerabilidades.

* E se um invasor inserir 5MB de texto em um campo e seu aplicativo armazenar isso no banco de dados?
* Atenção a vulnerabilidades relacionadas a XML.

## Sanitize suas Entradas

Remova ou neutralize caracteres especiais que possam ser usados em injeções de código, como `<`, `>`, `"`, `'`, `;`, etc.

## Integer e Float Overflow

* Verifique os limites de variáveis inteiras e de ponto flutuante.
* Lembre-se de que o estouro pode ocorrer em valores intermediários durante cálculos.

## Memória e Estouro de Buffer

* Exemplo clássico: **Heartbleed Bug**, que permitia acesso à memória do servidor por meio de buffers mal manipulados.

## Testes

* Vá além do "caminho feliz":

  * Teste com entradas inválidas.
  * Valide o projeto de segurança.
  * Realize testes de penetração (pentests).

## Trate Exceções Corretamente

* Trate erros com clareza, sem expor detalhes internos da aplicação.
* Nunca retorne mensagens de erro completas ao usuário final.

## Logs e Monitoramento

* Registre todas as falhas relevantes: login, controle de acesso e validações de entrada no backend.
* Mantenha os logs por tempo suficiente para permitir análises forenses.
* Formate os logs para serem compatíveis com ferramentas de gerenciamento centralizado.
* Estabeleça alertas e monitoramento eficazes.
* **Nunca registre informações confidenciais** como senhas, tokens ou dados sensíveis.

## Crie APIs sobre APIs Não Seguras

* Encapsule operações inseguras com APIs que apliquem validações e controles adicionais.
* Comunique-se com sua equipe sobre essas decisões e implemente controles em camadas.

## Revise o Código com a Segurança em Mente

* Revisões de código são oportunidades valiosas para identificar vulnerabilidades.
* Utilize um **checklist de segurança** durante as revisões.

## Use Ferramentas de Segurança

* Leve a sério os alertas emitidos por ferramentas de análise estática/dinâmica.
* Não apenas corrija o código — entenda **a causa raiz** do problema para evitar reincidência.

## Documente Suas Preocupações de Segurança

* Registre todas as preocupações específicas de segurança identificadas.
* Mantenha a documentação das **melhores práticas de segurança** adotadas no projeto.
