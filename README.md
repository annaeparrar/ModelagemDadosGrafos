# ModelagemDadosGrafos
1) Diagrama/Esboço da Modelagem de Dados em GrafosA modelagem será composta pelos seguintes Nós (Entidades) e Relacionamentos (Conexões):Tipo de Nó (Label)Propriedades ChaveDescriçãoUseruserId (ID), nomeO usuário do serviço de streaming.FilmefilmeId (ID), titulo, anoUm filme no catálogo.SerieserieId (ID), titulo, anoUma série no catálogo.AtoratorId (ID), nomeO artista que atua em filmes/séries.DiretordiretorId (ID), nomeA pessoa que dirige filmes/séries.Generonome (ID)O gênero do conteúdo (Ação, Comédia, etc.).
2) Tipo de Relacionamento,De/Para,Propriedades Chave,Descrição
ASSISTIU,User → Filme/Serie,"rating (1-5), data",Indica que um usuário assistiu e avaliou.
ATUOU_EM,Ator → Filme/Serie,personagem,Indica o papel do ator.
DIRIGIU,Diretor → Filme/Serie,-,Indica quem dirigiu a produção.
TEM_GENERO,Filme/Serie → Genero,-,Classifica o conteúdo em um gênero.
Representação Visual Simplificada:$$[User] \rightarrow [ASSISTIU] \rightarrow [Filme | Serie]$$$$[Ator] \rightarrow [ATUOU\_EM] \rightarrow [Filme | Serie]$$$$[Diretor] \rightarrow [DIRIGIU] \rightarrow [Filme | Serie]$$$$[Filme | Serie] \rightarrow [TEM\_GENERO] \rightarrow [Genero]$$
