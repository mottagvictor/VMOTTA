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

## Instruções de Execução

Siga os passos abaixo para rodar o projeto localmente ou na nuvem.

### Pré-requisitos
*   **Neo4j Desktop** instalado localmente, ou uma instância ativa no **Neo4j AuraDB** (nuvem gratuita).
*   Acesso ao **Neo4j Browser** (interface web de consultas).

### Passo 1: Configuração das Constraints e Carga de Dados
1. Abra o seu **Neo4j Browser**.
2. Copie o conteúdo do arquivo `script.cypher` deste repositório.
3. Cole no terminal do Neo4j Browser e clique no botão **Play (Run)**.

> *Nota: Este script criará automaticamente os índices de unicidade de ID para evitar duplicidade e populará a base com 10 usuários, 10 filmes, séries, atores, diretores e seus respectivos históricos de engajamento.*

### Passo 2: Validando a Carga de Dados
Para garantir que tudo foi importado corretamente, execute a seguinte consulta na barra de comandos:
```cypher
MATCH (n) RETURN labels(n) AS Entidade, count(n) AS Total;
```

### Passo 3: Testando seu primeiro Motor de Recomendação
Execute a query abaixo para testar o potencial do grafo. Ela busca recomendações para a usuária **Alice** com base no comportamento de usuários semelhantes (filtragem colaborativa):

```cypher
MATCH (u:User {name: "Victor"})-[r1:WATCHED]->(m:Movie)
MATCH (m)<-[r2:WATCHED]-(outro:User)-[:WATCHED]->(rec:Movie)
WHERE NOT (u)-[:WATCHED]->(rec) 
  AND r1.rating >= 4.0 
  AND r2.rating >= 4.0
RETURN rec.title AS FilmeRecomendado, COUNT(*) AS ForcaDaRecomendacao
ORDER BY ForcaDaRecomendacao DESC
LIMIT 3;

