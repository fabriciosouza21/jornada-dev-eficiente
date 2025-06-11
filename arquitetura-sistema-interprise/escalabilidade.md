# Escalabilidade

## Replicas de banco de dados

- Consistencia eventual
- replicar aliviar a carga do banco de dados principal

![alt text](image.png)

## Caches

- reduzir o nomero de vezes que acessamos o banco de dados
- Na memória, em disco ou distribuído
- consistência eventual
- estratégias de invalidação

![alt text](image-1.png)

## Escala horizontal e regional

- decidir se vair duplicar a aplicação em várias regiões ou não
- decidir como vai ser feita a sincronização entre as regiões

![alt text](image-2.png)

## Processamento assíncrono

- escalabilidade horizontal
- filas de mensagens
- pode afetar a Ux

![alt text](image-3.png)

## BDs de escrita e leitura separados(ETL)

- Query de relatórios
- ETL (Extract, Transform, Load)
- replicação de dados

![alt text](image-4.png)
