Exploração e análise de dados de crédito com SQL


Os dados:
Os dados retratam detalhes dos clientes de um banco e incluem as seguintes categorias:

idade = idade do cliente
sexo = sexo do cliente (F ou M)
dependentes = número de dependentes do cliente
escolaridade = nível de escolaridade do clientes
salario_anual = faixa salarial do cliente
tipo_cartao = tipo de cartao do cliente
qtd_produtos = quantidade de produtos comprados nos últimos 12 meses
iteracoes_12m = quantidade de iterações/transacoes nos ultimos 12 meses
meses_inativo_12m = quantidade de meses que o cliente ficou inativo
limite_credito = limite de credito do cliente
valor_transacoes_12m = valor das transações dos ultimos 12 meses
qtd_transacoes_12m = quantidade de transacoes dos ultimos 12 meses


A tabela foi criada no AWS Athena em conjunto com o S3 Bucket, utilizando uma versão dos dados disponíveis em: https://github.com/andre-marcos-perez/ebac-course-utils/tree/main/dataset

Examinando os dados:
A primeira etapa da análise consiste em compreender os elementos presentes em nossa matéria-prima. Vamos começar a exploração dos dados:

Qual é a quantidade de informações que temos em nossa base de dados?
Query: SELECT count(*) FROM credito

Reposta: 
![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/linhas.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T004657Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=01396582fa98c0dcec206d87595bdd423c7651ccc19d7848873451b67f1477bc2da715c2cfe2288fa99bb08f966483407aff697a8da746b391cdd1f95cee67387b779cebedf598eff2bdfde1e32c83034d8e6feeda925c5c53655c8940095485d904e916a5f1a7bf32eff18091d32d77cbd2d1ee9016c2bc55f2dbf98eb519872e23257c15fe51e5d0cd5c65a3d244a725a9f129114b6fcb47fa602f823f36dcff3577ff32e640970be2b27bbf0c322e54ae2818289519c298654ccc102b2123a7744b03fbe502ce4de7ec0378cd7b3733c85e25f8b177d061352ffc2ad5ab37f9ee72443e9a2db0753b1b46db63cc3f20a1a934a187657068f7fee6fadfeb96)


ps.: Dados de amostragem.

Como são os dados

Query: SELECT * FROM credito LIMIT 10;Dez primeiras linhas do dataset

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/credito.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T004650Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=2a0aa87d3d2394c66eada3ae1b5ac0126f395351f76f2709a395ca1f087cfa9a15ea046cd9655da5f2c48d14a43361f377b5e6ac4868ea0b2d92bd1557aad0f776a2efeef2cc67d7754a9ca6d05505544c44e7c3712df12f8d8a4d2dceb7c68dfd07eac3c085d151bcc2bb6b11fe907f10afdc81c5f32d1eefaa6a798bfe7a67937d8933eb0317bcb9376853f771eb7a52f9b9aff0563fa5ce02aa56fe45b13f363af1c0221eae252abc9f11ef2866117fda9dc6a844465b13e42ecb2e842d17f6e10013694ec7f46582553ed8b81900e6a2cc0d50df2089690fea36f4f6fd242a2f5f474093c49a154e5beab305be8bf467e5957537a31ce74a03256cf23e9e)

É possível haver algumas lacunas na tabela (indicadas como 'valor na'). Vamos investigar mais detalhadamente os valores de cada coluna!

Aqui estão os tipos de dados de cada categoria:

Query: DESCRIBE credito

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/describe.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T012040Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=4068493d2364ac0d349ffa68490d7e369b0bf0ca3033a2dc493fd106f3d538dfcb18b8982fb55ad82761528d831b29fa4ffd05dfe672cc1b4eeb4cd5a5ff6a15903eba6fdebd0c57a69b95008d87289395cd24712eec3f36eedcb46f0759bb49ded4bb83283de42a878ac3df273e1a71534965d4915f9a1496e84b2414d57390570d50f2752cf48e0343e1f7c49fa13a5f28f25fecf1c02e0991f25f40e4d0b4904a3f9590d3b22b7bede7508057e34e9f0b394eb92c5cbe786f4bb2f0fdc9173eac5c39ba92c86fa1f33cfd176a0c415a7ecd82a15068e052ab6d9090fbb95ce82877b3ed2e856557a512e6913a75ed6af11b3ab8b3b66eece006a725ef4551)

Após termos identificado os tipos de dados presentes, é hora de focarmos nas variáveis que não são numéricas.

Agora, gostaríamos de saber: quais são as diferentes categorias de escolaridade disponíveis no conjunto de dados?

Query: SELECT DISTINCT escolaridade FROM credito

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/escolaridade.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T011527Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=755ab6bba7cf6efc4a1f6ffc39217d37f849b5b1aa85d291049be1e9dd51ccdb1b27dfc7140548847806562d7449f1aa0c681e895fc5abef9164c43288baed9538a3de4026a155a4e09c2aae6377ea58eff41786723d8061f37494f27dba8e9712847ea18c511cf0985ae937d6b22037128571262df8d13a5551ac8422a58ebc4b1d3b13f4e71946c576e134bddc23c574399f4a4328c3d0c73c5f9feaa3502af8dd9d5440a534e5b9a0d3156795e4988cd3514c6ded4869970dcb430a5a568e9affbf48bb62da1f11c6ff2e42599b93880b78723646c34ce25c3e81838ac2c0e2dd9f5c16d9fe6edd3c3a353d11ec5e52134582944af085fa166f0b72a90fae)

Diversos graus de escolaridade estão presentes nos dados, e é evidente que existem células com valores nulos (representados por "na") no conjunto de dados. Abordaremos isso posteriormente!

Agora, vamos investigar se há mais células com valores nulos.

Quais são os diferentes tipos de estado civil disponíveis no conjunto de dados?

Query: SELECT DISTINCT estado_civil FROM credito
![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/estadocivil.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T011613Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=912c11a0a3746a5495050ead0b8c8b9622c7bd327936e68b9018a698f62f6adeb1fcaae34f41389d1610a9d7db645411e1e69b28eef6ec5f741e8596e20b57dd07f32db70205f91bf017c64b9e8b1484c43a761434e0b5a904807cc1360296adc43aff07c5ad397f4eabb77c7bb391c616c138289db3ddac438953075c6aff23fec10948126ca945069194497b30b4a7ec21031dff1b54498d0e8fdf1476cfb8da40e711dc1ef7856d864aea9003be8c88035d5dcace2ece0d9114b4e40e7f2acabd4f4c816e5eb6b5d31d0c362186280687d000b50988565a78316e50dcff4726ded400565a9e76204e779be6ec2a3150432a37eade9bb8e8686b85aa64fd28)


Tipos de estado civil

Novamente encontramos valores nulos nos dados de estado civil!

Quais são os tipos de salario_anual disponíveis no dataset?

Query: SELECT DISTINCT salario_anual FROM credito

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/salarioanual.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T011637Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=6567d63e2846d8a789c3591d5b8fb52fa625ab6992fe8f692f38ef88c909e9b2c492aabc73bdd53120fb93e6b015d78ccdb2c41971c64e47883a943c0564dab17b24e6a0b5bb6a9f822eb5c974b3f5fd5ac2903ec810542d31cdbba6e8a2326f825b14450993b208d929171eab47d6ecdfa306a965d8839c2ae2fef74736f89492f58482b94cb745ddb9ea541b850387466326e1ee3c8e5f5dbb8b7d20a7262895c8a4fcd3d203904453291b053b6f4b96981a559f12d58ec7d05f1094ec15808d4037d8a0d767a16e8cbd5f05110ec48dceecdb4027340ed29ac24f7a0aab730a6f94e0447160996a9594a2646f6eb1eda49f7a94b27e7596a32d12fd30539d)

Tipos de salario anual

No conjunto de dados, os salários não são apresentados com os valores exatos do ganho de cada cliente. Em vez disso, são fornecidas faixas salariais. Além disso, há registros com valores nulos.

Agora, gostaríamos de saber: quais são os diferentes tipos de cartão disponíveis no conjunto de dados?

Query: SELECT DISTINCT tipo_cartao FROM credito

![](https://github.com/marianeneiva/sqlEBAC/blob/main/cartao.png?raw=true)

Tipos de cartão

Análise dos Dados

Após uma exploração inicial dos dados para compreender as informações contidas no banco de dados, é hora de analisar esses dados para entender melhor o contexto. Vamos começar fazendo algumas perguntas: No conjunto de dados, quantos clientes estão em cada faixa salarial?

Query: select count(*), salario_anual from credito group by salario_anual
![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/salarioanual.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T011637Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=6567d63e2846d8a789c3591d5b8fb52fa625ab6992fe8f692f38ef88c909e9b2c492aabc73bdd53120fb93e6b015d78ccdb2c41971c64e47883a943c0564dab17b24e6a0b5bb6a9f822eb5c974b3f5fd5ac2903ec810542d31cdbba6e8a2326f825b14450993b208d929171eab47d6ecdfa306a965d8839c2ae2fef74736f89492f58482b94cb745ddb9ea541b850387466326e1ee3c8e5f5dbb8b7d20a7262895c8a4fcd3d203904453291b053b6f4b96981a559f12d58ec7d05f1094ec15808d4037d8a0d767a16e8cbd5f05110ec48dceecdb4027340ed29ac24f7a0aab730a6f94e0447160996a9594a2646f6eb1eda49f7a94b27e7596a32d12fd30539d)

Quantidade para cada faixa salarial

A grande parte dos clientes nesse conjunto de dados tem uma renda inferior a 40 mil. Além disso, há 235 clientes cuja faixa salarial não foi informada ou está ausente. Em certo sentido, pode ser vantajoso para a empresa concentrar seus esforços nesse segmento de renda mais baixa.

No que diz respeito à distribuição de gênero, quantos clientes são do sexo masculino e quantos são do sexo feminino neste banco de dados?

Query: select count(*), sexo from credito group by sexo

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/quantidade%20por%20sexo.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T011711Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=221690359903ddfcf08ef02cb24a7a428713f8a29a5f34d87918af0aa38ede0bf1e4f67ffd9b358bb59fe900245ef41a43dd5eab90fb88506c835aedd65b9eda4fe0892ad6d03ef0acad583d618830cc9efb1a14f674a579cbc82e79af48ae6ae632f788bbb432348fca6ad850dc7e754483cd0cd301153652a3c28e1f8f6ca2da0ed9692071f151965efa146b190ed5d8ec73a6550d0c39876a9e18d5e23fe3709d45e7795c22b8cd1fab5bf55c96513aaad59c3b578250c699217f519573682f5225d82d1d6d012db15199c40c91ccbbbda9b7997ef11d8ea673f06d93d6d02f4bb0c6b7ec2d4c95ff68d3e940c0761fc6730591420b0af54d44de9ae9dec7)

Quantidade para cada sexo

Quantidade para cada sexo - gráfico

A maioria dos clientes desse banco é homem! Do csv extraído dos dados é possível gerar o gráfico em pizza com para melhor a proporção de masculino/feminino

Queremos focar o nosso marketing de maneira adequada para nossos clientes, qual será a idade deles?

Query: select avg(idade) as media_idade, min(idade) as min_idade, max(idade) as max_idade, sexo from credito group by sexoMédia de idades por sexo

![](https://storage.googleapis.com/kagglesdsdata/datasets/4669457/7942116/avgidadesexo.png?X-Goog-Algorithm=GOOG4-RSA-SHA256&X-Goog-Credential=databundle-worker-v2%40kaggle-161607.iam.gserviceaccount.com%2F20240326%2Fauto%2Fstorage%2Fgoog4_request&X-Goog-Date=20240326T010021Z&X-Goog-Expires=345600&X-Goog-SignedHeaders=host&X-Goog-Signature=6212cc3124145b4c7e0e971ae7a6eef7b82c02ffcb948a19b87827c01cf2bc775977c631a7a43dcc4b7cdce1070f387cd33f4f284f1c412b86a8d8c84c924fb72172a0bc4d1afce28cdfb60ee40a0b1e2a2bb992313adba3fc894077a7db8de945877936a8572be99f3aa14eb6e863515c85c10b97125f799ba8287d3d8cef69632753b7c4af4847ef81ddc8a0c91d4d8f81708de30e5e8b43b9e4be5c9c614d01969e7de47f29bf136258e477d57301882367f554e2274d9dbc9125fb93b10a387133e20fc587da48e8fde46f24c18fa6d4183d78359ec05df1d0046a3babe7a17b3b3090cdfc6f6b3ae40bb1e41a4a7d602a7a000399c5b7a60c52e44fb659)

Após esta análise, não foi possível encontrar nenhuma informação significativa. Tanto a menor idade quanto a média de idade são praticamente idênticas entre os dois sexos. A única discrepância observada é na idade máxima, porém, essa diferença é quase insignificante devido à sua pequena magnitude.

Qual a maior e menor transação dos clientes?

Query: select min(valor_transacoes_12m) as transacao_minima, max(valor_transacoes_12m) as transacao_minima from creditoValor transacoes

![](https://github.com/marianeneiva/sqlEBAC/blob/main/transacoes.png?raw=true)

Nesse banco de dados temos soma de transações em 12 meses variam de 510.16 a 5776.58

Quais as características dos clientes que possuem os maiores creditos?

Query: select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo from credito where escolaridade != 'na' and tipo_cartao != 'na' group by escolaridade, tipo_cartao, sexo order by limite_credito desc limit 10

![](https://github.com/marianeneiva/sqlEBAC/blob/main/limite_desc.png?raw=true)

Valor limite

Não parece haver um impacto da escolaridade no limite. O limite mais alto é oferecido para um homem sem educação formal. O cartão também parece não estar relacionado com a escolaridade nem com o limite. Dentre os maiores limites, encontramos clientes com cartão: gold, silver, platinum e blue

Quais as características dos clientes que possuem os menores creditos?

Query: select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo from credito where escolaridade != 'na' and tipo_cartao != 'na' group by escolaridade, tipo_cartao, sexo order by limite_credito asc

![](https://github.com/marianeneiva/sqlEBAC/blob/main/limite_asc.png?raw=true)

Valor limite

Dessa vez conseguimos perceber que não há clientes com cartão platinum dentre os menores limites. Também foi possível perceber que a maioria dos menores limites são mulheres enquanto nos maiores limites predomina homens.

Será que as mulheres gastam mais?

Query: select max(valor_transacoes_12m) as maior_valor_gasto, avg(valor_transacoes_12m) as media_valor_gasto, min(valor_transacoes_12m) as min_valor_gasto, sexo from credito group by sexo

![](https://github.com/marianeneiva/sqlEBAC/blob/main/quemgastamais.png?raw=true)
Valor transacoes/sexo


Apesar da diferença nos limites, os gastos de homens e mulheres são similares!

Por fim,

O salário impacta no limite?

Query: select avg(qtd_produtos) as qts_produtos, avg(valor_transacoes_12m) as media_valor_transacoes, avg(limite_credito) as media_limite, sexo, salario_anual from credito where salario_anual != 'na' group by sexo, salario_anual order by avg(valor_transacoes_12m) desc

![](https://github.com/marianeneiva/sqlEBAC/blob/main/salarioAnualMediaLimite.png?raw=true)

Valor salario_anualLimite

Conclusão:

A análise do dataset de crédito revelou alguns insights interessantes:

A maioria dos clientes tem uma renda de até 40 mil.
A maioria dos clientes é do sexo masculino.
Parece não haver influência da escolaridade no limite de crédito ou no tipo de cartão.
A maioria dos clientes com os maiores limites de crédito é do sexo masculino.
Por outro lado, a maioria dos clientes com os menores limites de crédito é do sexo feminino.
Não há presença de cartões Platinum entre os clientes com os menores limites de crédito.
A faixa salarial tem um impacto direto no limite de crédito.
Não há clientes do sexo feminino com salário anual acima de 60 mil.
Uma análise mais profunda dos dados pode explicar por que as mulheres têm limites de crédito menores. Isso também pode ser um reflexo de questões culturais que precisam ser reconsideradas!
