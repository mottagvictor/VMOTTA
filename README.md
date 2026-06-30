# VMOTTA
NEO4J - Serviço de Streaming


## Contexto do Problema
Serviço de Streaming de filmes e séries, diferente dos sistemas tradicionais, focamos nos relacionamentos para criar um sistema de recomendação poderoso para sugerir um filme com base no que os usuário assistiram, que pertencem ao mesmo gênero e que foram dirigidos pelo mesmo diretor.


## Escolha por Bancos em Grafos
A escolha pelo Neo4j e pelo modelo de grafos justifica-se por três pilares fundamentais:
1. RELACIONAMENTO EM GRAFOS: as conexões ('WATCHED', 'ACTED_IN') são armazenadas fisicamente no disco junto aos dados ('User', 'Movie')
2. PERFORMANCE ESCALÁVEL: O algoritmo de busca do Neo4j utiliza navegação direta pelos ponteiros da memória, o tempo de resposta de uma recomendação depende apenas do tamanho do subgrafo vizinho, e não do tamanho total do banco de dados.
3. FLEXIBILIDADE DE MODELO: Adicionar novas entidades ou novos tipos de conexões no futuro não exige migrações complexas de esquema.


## Modelagem do Grafo (Esboço do Modelo)
Os dados foram estruturados utilizando Rótulos (Labels) para as entidades e Tipos de Relacionamento irecionados para as conexões.


### Propriedades por Nó
* User: 'id' (Unique), 'name'
* Movie: 'id' (Unique), 'title'
* Series: 'id' (Unique), 'title'
* Genre: 'id' (Unique), 'name'
* Actor: 'id' (Unique), 'name'
* Director: 'id' (Unique), 'name'
