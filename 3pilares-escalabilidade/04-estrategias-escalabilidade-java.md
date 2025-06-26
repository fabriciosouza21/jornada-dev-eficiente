# Estratégias de Escalabilidade para Aplicações Java

## 1. Otimização de Recursos Locais

### Configuração da JVM

#### Memória Heap
Local onde os objetos são alocados na JVM.

**Parâmetros de configuração:**
- `-Xms1024m`: Define o tamanho inicial da heap
- `-Xmx1024m`: Define o tamanho máximo da heap

#### Integração com Docker
- **Configuração padrão:** Desde o Java 11
- **Parâmetro recomendado:** `-XX:+UseContainerSupport`
  - Permite que a JVM reconheça os limites de recursos do container

## 2. Escalabilidade Vertical (Scale Up)

Quando otimizações de configuração não são mais suficientes:

- **CPU:** Processadores mais potentes
- **Memória:** Aumento da RAM disponível
- **Infraestrutura:** Melhoria geral dos componentes de hardware

**Limitação:** Existe um teto para o quanto uma única máquina pode ser aprimorada.

## 3. Escalabilidade Horizontal (Scale Out)

### Cluster Básico
- **Conceito:** Adicionar mais máquinas ao sistema
- **Implementação:** Criação de um cluster que atua em conjunto
- **Load Balancer:** Distribui requisições entre as máquinas
- **Característica:** Sem redundância de dados

## 4. Alta Disponibilidade (High Availability - HA)

### Implementação com Redundância
- **Replicação de Estados:** Estados são replicados entre todas as máquinas
- **Benefício:** Garantia de disponibilidade mesmo com falha de máquinas
- **Desafio:** Com muitas máquinas, o número elevado de operações de replicação pode causar:
  - Problemas de latência
  - Redução do throughput

## 5. Arquitetura Stateless

### Solução para Problemas de Replicação
- **Conceito:** Remover os estados das máquinas da aplicação
- **Vantagem:** Elimina a necessidade de replicação entre máquinas

### Implementação
- Uso de cache distribuído (exemplo: Redis)
- Estados centralizados e compartilhados
- Máquinas da aplicação permanecem sem estado local

### Benefícios da Abordagem Stateless
- Maior simplicidade na escalabilidade
- Redução da complexidade de sincronização
- Melhor performance em clusters grandes
- Facilita a adição/remoção de máquinas dinamicamente
