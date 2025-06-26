# Cache - Estratégias e Implementação

## Objetivo Principal

> Picos de acesso podem aumentar nosso response time.

Aplicar cache para aliviar a carga do banco de dados.

> A consulta mais rápida é aquela que nem chega no banco de dados.

## Níveis de Cache no Hibernate

### First Level Cache
- **Entity Manager** funciona como cache de primeiro nível
- Cache em nível de transação
- Automático

### Second Level Cache
- Cache em nível de aplicação
- Cache compartilhado entre transações
- Requer configuração manual

#### Configuração Básica
```properties
hibernate.cache.use_second_level_cache=true
```

**Importante:** Entidades não são cacheadas automaticamente - você decide quais cachear.

#### Anotações para Cache
```java
@Entity
@Cache
class NotaFiscal {

    @Cache
    @OneToMany(fetch = FetchType.EAGER)
    List<ItemNotaFiscal> itens;
}
```

## Fine Tuning - Estratégias de Cache

### Caching Strategies
- **READ_ONLY:** Dados que quase nunca mudam
- **NONSTRICT_READ_WRITE:** Dados não críticos
- **READ_WRITE:** Dados que mudam com frequência

### Configuração de Provider

```xml
<property
    name="hibernate.javax.cache.provider"
    value="org.ehcache.jsr107.EhcacheCachingProvider"/>
<property
    name="hibernate.javax.cache.region.factory_class"
    value="org.hibernate.cache.jcache.JCacheRegionFactory"/>
<property
    name="hibernate.javax.cache.uri"
    value="ehcache.xml"/>
```

### Configuração EhCache (ehcache.xml)

```xml
<cache name="com.deveficiente.domain.Produto"
       maxElementsInMemory="1000"
       eternal="false"
       timeToIdleSeconds="86400"    <!-- 24H idle (tempo de ociosidade) -->
       timeToLiveSeconds="259200"   <!-- 72H live (tempo de vida) -->
       memoryStoreEvictionPolicy="LRU" <!-- Least Recently Used -->
       overflowToDisk="true">
</cache>
```

**LRU:** Produtos menos utilizados são removidos ou movidos para o disco.

## Cache em Diferentes Camadas

O cache pode estar em qualquer camada:
- Apresentação
- Negócio
- Persistência

### Processamento de Conteúdo Cacheado

O cache pode evitar reprocessamento de conteúdo:
- Geração de códigos de barras
- Geração de QR codes
- Cálculos complexos

## Spring Cache - Cache em Nível de Controller

### Implementação com @Cacheable

```java
@RestController
public class ReciboController {

    private NotaFiscalRepository notaFiscalRepository;

    @Cacheable("recibosCache")
    @GetMapping("/api/recibos/{notaFiscalId}")
    public ReciboResponse gerarRecibo(@PathVariable Long notaFiscalId) {

        NotaFiscal nota = notaFiscalRepository.findById(notaFiscalId)
            .orElseThrow(); // NotaFiscalNotFoundException::new

        List<Item> itens = // lógica de processamento
        BigDecimal total = // lógica de processamento

        return // constrói o objeto de resposta ReciboResponse
    }
}
```

### Vantagens do Cache em Controller
- Cacheia o método inteiro
- Evita processamento completo da requisição
- Escopo maior de otimização
