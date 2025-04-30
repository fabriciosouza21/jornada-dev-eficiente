# Exemplo

praticas obrigátorios

utilizar o LLM para gerar exemplos de atividades

# 10 Exercícios de Desenvolvimento de APIs com Spring Boot

## Tabela de Exercícios por Tipo de Relacionamento

| Tipo de Relacionamento | Nome do Exercício | Descrição | Entidades | Desafios |
|------------------------|-------------------|-----------|-----------|----------|
| **Sem Relacionamentos** | Cadastro de Produtos | API para gerenciar catálogo de produtos | `Produto` (nome, preço, descrição, estoque) | Implementar atualização de estoque e aplicação de descontos |
| **Sem Relacionamentos** | Cadastro de Tarefas | API para gerenciar lista de tarefas | `Tarefa` (título, descrição, status, data de vencimento) | Implementar filtro por status e marcação de conclusão |
| **Relacionamento 1:1** | Cliente e Endereço | Sistema que associa clientes com seus endereços | `Cliente` (nome, CPF, email), `Endereco` (logradouro, número, complemento, cidade, estado, CEP) | Garantir persistência em cascata do endereço junto com o cliente |
| **Relacionamento 1:1** | Veículo e Registro de Propriedade | Sistema para cadastrar veículos e documentos | `Veiculo` (marca, modelo, ano, placa), `RegistroPropriedade` (proprietário, data de aquisição, documento) | Garantir que cada veículo tenha exatamente um registro |
| **Relacionamento 1:N** | Pedidos e Clientes | Sistema onde clientes fazem vários pedidos | `Cliente` (nome, email, telefone), `Pedido` (número, data, itens, valor total) | Listar todos os pedidos de um cliente específico |
| **Relacionamento 1:N** | Cursos e Departamentos | Sistema para gerenciar cursos universitários | `Departamento` (nome, telefone, email), `Curso` (nome, descrição, carga horária, código) | Listar todos os cursos de um departamento |
| **Relacionamento N:M** | Cursos e Alunos | Sistema de matrícula em cursos | `Curso` (nome, descrição, carga horária), `Aluno` (nome, matrícula, email) | Matricular alunos em cursos e listar matrículas |
| **Relacionamento N:M** | Livros e Autores | Sistema para gerenciar biblioteca | `Livro` (título, ano, ISBN), `Autor` (nome, biografia, nacionalidade) | Associar múltiplos autores a livros e vice-versa |
| **Múltiplos Relacionamentos** | E-commerce | Plataforma de vendas online | `Produto`, `Categoria`, `Pedido`, `Cliente`, `Carrinho` | Gerenciar produtos em categorias, pedidos e carrinhos |
| **Desafio Avançado** | Sistema Universitário Integrado | Sistema completo para universidades | `Aluno`, `Professor`, `Curso`, `Departamento`, `Matricula`, `ProjetoPesquisa`, `Publicacao` | Implementar autenticação JWT, APIs externas, dashboard analítico, motor de busca e sistema de recomendação |

## Implementação dos Exercícios

Cada exercício deve incluir:

1. **Modelagem de Entidades**
   - Utilizar anotações JPA adequadas (@Entity, @Id, @OneToOne, @OneToMany, @ManyToOne, @ManyToMany)
   - Configurar CascadeType e FetchType apropriados

2. **Camadas da Aplicação**
   - Repository: interfaces que estendem JpaRepository
   - Service: lógica de negócios
   - Controller: endpoints RESTful
   - DTO: objetos para transferência de dados

3. **Funcionalidades Comuns**
   - Operações CRUD para todas as entidades
   - Validação de dados de entrada
   - Tratamento de exceções

## Tecnologias Recomendadas

- Spring Boot
- Spring Data JPA
- Spring Security
- Hibernate
- MySQL/PostgreSQL/H2
- Maven/Gradle
