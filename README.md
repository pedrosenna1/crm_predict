Propensao de conversao de leads (CRM)
====================================

Objetivo
--------
- Usar um modelo de ML para estimar a probabilidade de cada cliente aceitar uma proposta, priorizando quem tem maior propensao de compra.

Fontes de dados
---------------
- Datasets/crm_clientes.csv: cadastro do cliente (tipo de cliente, data de cadastro).
- Datasets/crm_contatos.csv: historico de contatos (tempo de resposta em horas, data do ultimo contato).
- Datasets/crm_propostas.csv: propostas enviadas (status, valor, data de envio).

Etapas principais (machine_learning.ipynb)
------------------------------------------
- Leitura dos tres CSVs e conversao de datas.
- Agregacoes por cliente:
	- Contatos: quantidade, media de tempo de resposta, data do ultimo contato.
	- Propostas: quantidade e valor medio.
- Uniao das agregacoes com a base de clientes e criacao do alvo `comprou` (status de proposta `Ganha`).
- Tratamento: arredondamento de valores numericos e preenchimento de ausencias com zero ou False.
- Selecionadas features numericas: `qtde_proposta`, `qtde_contato`, `media_tempo_proposta`, `media_valor_proposta`.
- Split train/test (80/20) e treinamento de `LogisticRegression` com `random_state=16`.
- Predicao de probabilidades e avaliacao via matriz de confusao com limiar customizado de 0.38 para classificar comprado vs. nao comprado.

Regra de negocio adotada
------------------------
- Priorizar clientes com probabilidade prevista de compra igual ou maior que 40%.
- Ordenar pela coluna `%_comprar` (probabilidade em %) e, em seguida, pelo maior `media_valor_proposta` para focar em tickets mais altos.
- Selecionar o top 10 clientes resultantes para ação comercial imediata.

Como reproduzir
---------------
- Abrir e executar o notebook machine_learning.ipynb em ordem.
- Garantir que os CSVs estejam na pasta Datasets conforme descrito.
- Sao usadas dependencias principais: pandas, scikit-learn.

Próximas evoluções
-------------------
- Adicionar novas variaveis comportamentais (ex.: tempo desde o ultimo contato, sazonalidade).
- Testar modelos de maior expressividade (Random Forest, Gradient Boosting) e calibrar probabilidades.

