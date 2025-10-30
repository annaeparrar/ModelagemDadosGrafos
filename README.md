-----

````
# MODELAGEM DE DADOS EM GRAFOS PARA SERVIÇO DE STREAMING

Este repositório contém a modelagem de um banco de dados de grafos (usando o Neo4j/Cypher como referência) para um serviço de streaming de vídeo. O modelo é otimizado para explorar conexões complexas, como sistemas de recomendação baseados em afinidade.

## 1. ESTRUTURA DO GRAFO (Nós e Relacionamentos)

### NÓS (ENTIDADES)
* **User:** Usuários da plataforma.
    * Propriedades: `userId`, `nome`
* **Filme:** Conteúdo do tipo Filme.
    * Propriedades: `filmeId`, `titulo`, `ano`
* **Serie:** Conteúdo do tipo Série.
    * Propriedades: `serieId`, `titulo`, `ano`
* **Ator:** Pessoas que atuaram no conteúdo.
    * Propriedades: `atorId`, `nome`
* **Diretor:** Pessoas que dirigiram o conteúdo.
    * Propriedades: `diretorId`, `nome`
* **Genero:** Classificação do conteúdo.
    * Propriedades: `nome`

### RELACIONAMENTOS (CONEXÕES)
* **(User)-[:ASSISTIU {rating, data}]->(Filme/Serie)**
    * Propriedade: `rating` (avaliação de 1 a 5), `data`
* **(Ator)-[:ATUOU_EM {personagem}]->(Filme/Serie)**
    * Propriedade: `personagem`
* **(Diretor)-[:DIRIGIU]->(Filme/Serie)**
* **(Filme/Serie)-[:TEM_GENERO]->(Genero)**

## 2. SCRIPT DE CRIAÇÃO EM CYPHER

O script abaixo cria a estrutura do modelo e popula o grafo com mais de 10 usuários, 10 atores e 10 títulos (Filmes/Séries).

```cypher
// 1. REGRAS: Restrições de Unicidade
CREATE CONSTRAINT IF NOT EXISTS FOR (u:User) REQUIRE u.userId IS UNIQUE;
CREATE CONSTRAINT IF NOT EXISTS FOR (a:Ator) REQUIRE a.atorId IS UNIQUE;
CREATE CONSTRAINT IF NOT EXISTS FOR (d:Diretor) REQUIRE d.diretorId IS UNIQUE;
CREATE CONSTRAINT IF NOT EXISTS FOR (f:Filme) REQUIRE f.filmeId IS UNIQUE;
CREATE CONSTRAINT IF NOT EXISTS FOR (s:Serie) REQUIRE s.serieId IS UNIQUE;
CREATE CONSTRAINT IF NOT EXISTS FOR (g:Genero) REQUIRE g.nome IS UNIQUE;


// 2. CRIAÇÃO DOS NÓS

// Gêneros
MERGE (g:Genero {nome: 'Acao'});
MERGE (g:Genero {nome: 'Comedia'});
MERGE (g:Genero {nome: 'Drama'});
MERGE (g:Genero {nome: 'Ficcao_Cientifica'});
MERGE (g:Genero {nome: 'Suspense'});

// Usuários (+10)
CREATE (u1:User {userId: 1, nome: 'Ana_Silva'});
CREATE (u2:User {userId: 2, nome: 'Bruno_Melo'});
CREATE (u3:User {userId: 3, nome: 'Carla_Santos'});
CREATE (u4:User {userId: 4, nome: 'Daniel_Costa'});
CREATE (u5:User {userId: 5, nome: 'Eduarda_Alves'});
CREATE (u6:User {userId: 6, nome: 'Fernando_Lima'});
CREATE (u7:User {userId: 7, nome: 'Giovana_Pereira'});
CREATE (u8:User {userId: 8, nome: 'Hugo_Rodrigues'});
CREATE (u9:User {userId: 9, nome: 'Isabela_Gomes'});
CREATE (u10:User {userId: 10, nome: 'Julio_Freitas'});

// Diretores (+10)
CREATE (d1:Diretor {diretorId: 101, nome: 'Dir_Xavier'});
CREATE (d2:Diretor {diretorId: 102, nome: 'Dir_Yara'});
CREATE (d3:Diretor {diretorId: 103, nome: 'Dir_Zeca'});
CREATE (d4:Diretor {diretorId: 104, nome: 'Dir_Alice'});
CREATE (d5:Diretor {diretorId: 105, nome: 'Dir_Beto'});
CREATE (d6:Diretor {diretorId: 106, nome: 'Dir_Cecilia'});
CREATE (d7:Diretor {diretorId: 107, nome: 'Dir_Dario'});
CREATE (d8:Diretor {diretorId: 108, nome: 'Dir_Erica'});
CREATE (d9:Diretor {diretorId: 109, nome: 'Dir_Felipe'});
CREATE (d10:Diretor {diretorId: 110, nome: 'Dir_Gloria'});

// Atores (+10)
CREATE (a1:Ator {atorId: 201, nome: 'Ator_A'});
CREATE (a2:Ator {atorId: 202, nome: 'Ator_B'});
CREATE (a3:Ator {atorId: 203, nome: 'Ator_C'});
CREATE (a4:Ator {atorId: 204, nome: 'Ator_D'});
CREATE (a5:Ator {atorId: 205, nome: 'Ator_E'});
CREATE (a6:Ator {atorId: 206, nome: 'Ator_F'});
CREATE (a7:Ator {atorId: 207, nome: 'Ator_G'});
CREATE (a8:Ator {atorId: 208, nome: 'Ator_H'});
CREATE (a9:Ator {atorId: 209, nome: 'Ator_I'});
CREATE (a10:Ator {atorId: 210, nome: 'Ator_J'});

// Filmes/Séries (+10)
CREATE (f1:Filme {filmeId: 301, titulo: 'O Ataque dos Dados', ano: 2023});
CREATE (f2:Filme {filmeId: 302, titulo: 'A Linguagem Secreta', ano: 2022});
CREATE (f3:Filme {filmeId: 303, titulo: 'Riso na Rede', ano: 2021});
CREATE (f4:Filme {filmeId: 304, titulo: 'A Fuga do Nó', ano: 2024});
CREATE (f5:Filme {filmeId: 305, titulo: 'O Grande Merge', ano: 2023});

CREATE (s1:Serie {serieId: 401, titulo: 'Grafos: A Jornada', ano: 2022});
CREATE (s2:Serie {serieId: 402, titulo: 'Conexões Ocultas', ano: 2023});
CREATE (s3:Serie {serieId: 403, titulo: 'O Código da Comédia', ano: 2024});
CREATE (s4:Serie {serieId: 404, titulo: 'A Última Query', ano: 2021});
CREATE (s5:Serie {serieId: 405, titulo: 'O Mistério da Chave', ano: 2022});


// 3. CRIAÇÃO DOS RELACIONAMENTOS

// ATUOU_EM
MATCH (a1:Ator {atorId: 201}), (f1:Filme {filmeId: 301}) CREATE (a1)-[:ATUOU_EM {personagem: 'Heroi'}]->(f1);
MATCH (a2:Ator {atorId: 202}), (f2:Filme {filmeId: 302}) CREATE (a2)-[:ATUOU_EM {personagem: 'Cientista'}]->(f2);
MATCH (a3:Ator {atorId: 203}), (s1:Serie {serieId: 401}) CREATE (a3)-[:ATUOU_EM {personagem: 'Protagonista'}]->(s1);
MATCH (a4:Ator {atorId: 204}), (f3:Filme {filmeId: 303}) CREATE (a4)-[:ATUOU_EM {personagem: 'Comediante'}]->(f3);
MATCH (a5:Ator {atorId: 205}), (s2:Serie {serieId: 402}) CREATE (a5)-[:ATUOU_EM {personagem: 'Detetive'}]->(s2);
MATCH (a1:Ator {atorId: 201}), (s1:Serie {serieId: 401}) CREATE (a1)-[:ATUOU_EM {personagem: 'Secundario'}]->(s1);

// DIRIGIU
MATCH (d1:Diretor {diretorId: 101}), (f1:Filme {filmeId: 301}) CREATE (d1)-[:DIRIGIU]->(f1);
MATCH (d2:Diretor {diretorId: 102}), (f2:Filme {filmeId: 302}) CREATE (d2)-[:DIRIGIU]->(f2);
MATCH (d3:Diretor {diretorId: 103}), (s1:Serie {serieId: 401}) CREATE (d3)-[:DIRIGIU]->(s1);
MATCH (d4:Diretor {diretorId: 104}), (f3:Filme {filmeId: 303}) CREATE (d4)-[:DIRIGIU]->(f3);
MATCH (d5:Diretor {diretorId: 105}), (s2:Serie {serieId: 402}) CREATE (d5)-[:DIRIGIU]->(s2);
MATCH (d1:Diretor {diretorId: 101}), (s5:Serie {serieId: 405}) CREATE (d1)-[:DIRIGIU]->(s5);

// TEM_GENERO
MATCH (f1:Filme {filmeId: 301}), (g:Genero {nome: 'Acao'}) CREATE (f1)-[:TEM_GENERO]->(g);
MATCH (f2:Filme {filmeId: 302}), (g:Genero {nome: 'Ficcao_Cientifica'}) CREATE (f2)-[:TEM_GENERO]->(g);
MATCH (f3:Filme {filmeId: 303}), (g:Genero {nome: 'Comedia'}) CREATE (f3)-[:TEM_GENERO]->(g);
MATCH (s1:Serie {serieId: 401}), (g:Genero {nome: 'Drama'}) CREATE (s1)-[:TEM_GENERO]->(g);
MATCH (s2:Serie {serieId: 402}), (g:Genero {nome: 'Suspense'}) CREATE (s2)-[:TEM_GENERO]->(g);
MATCH (f1:Filme {filmeId: 301}), (g:Genero {nome: 'Suspense'}) CREATE (f1)-[:TEM_GENERO]->(g);

// ASSISTIU (com avaliação)
MATCH (u1:User {userId: 1}), (f1:Filme {filmeId: 301}) CREATE (u1)-[:ASSISTIU {rating: 5, data: date('2024-10-25')}]->(f1);
MATCH (u2:User {userId: 2}), (f1:Filme {filmeId: 301}) CREATE (u2)-[:ASSISTIU {rating: 4, data: date('2024-10-26')}]->(f1);
MATCH (u3:User {userId: 3}), (f2:Filme {filmeId: 302}) CREATE (u3)-[:ASSISTIU {rating: 3, data: date('2024-10-20')}]->(f2);
MATCH (u4:User {userId: 4}), (s1:Serie {serieId: 401}) CREATE (u4)-[:ASSISTIU {rating: 5, data: date('2024-10-15')}]->(s1);
MATCH (u1:User {userId: 1}), (s1:Serie {serieId: 401}) CREATE (u1)-[:ASSISTIU {rating: 4, data: date('2024-10-27')}]->(s1);
MATCH (u5:User {userId: 5}), (f3:Filme {filmeId: 303}) CREATE (u5)-[:ASSISTIU {rating: 2, data: date('2024-10-28')}]->(f3);
MATCH (u6:User {userId: 6}), (s2:Serie {serieId: 402}) CREATE (u6)-[:ASSISTIU {rating: 4, data: date('2024-10-29')}]->(s2);
MATCH (u7:User {userId: 7}), (f4:Filme {filmeId: 304}) CREATE (u7)-[:ASSISTIU {rating: 5, data: date('2024-10-30')}]->(f4);
MATCH (u8:User {userId: 8}), (f5:Filme {filmeId: 305}) CREATE (u8)-[:ASSISTIU {rating: 3, data: date('2024-10-30')}]->(f5);
MATCH (u9:User {userId: 9}), (s4:Serie {serieId: 404}) CREATE (u9)-[:ASSISTIU {rating: 4, data: date('2024-10-29')}]->(s4);
MATCH (u10:User {userId: 10}), (s5:Serie {serieId: 405}) CREATE (u10)-[:ASSISTIU {rating: 5, data: date('2024-10-28')}]->(s5);
````

```
```
