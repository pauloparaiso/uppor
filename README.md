Under Pressure Passing Odds Ratio (UPPOR)


Neste exercício prático proponho-me apresentar uma nova métrica, Under Pressure Passing Odds Ratio (UPPOR), com o objetivo de medir a associação entre o tipo de passes realizados pelos jogadores de uma determinada equipa e a existência de pressão por parte da equipa adversária.


	Contextualização

Na reunião com a equipa técnica, o treinador explicou detalhadamente o seu modelo de jogo. Referiu que privilegia o jogo em posse, com uma maior prevalência de passes curtos, relativamente aos passes médios e longos, no sentido de procurar controlar o jogo e desgastar a equipa adversária. Referiu também que a sua equipa tem sentido dificuldades quando as equipas adversárias pressionam os nossos jogadores, obrigando-os a fugir aos princípios do jogo em posse com passes curtos e apostando, mais do que deviam, em passes de média e longa distância. 


	Objetivo da métrica

Neste sentido, propus a métrica Under Pressure Passing Odds Ratio (UPPOR), com o objetivo de medir a associação entre a realização de passes de média e longa distância e a existência de pressão por parte da equipa adversária. Assim, depois de calculada, para uma determinada partida, teremos uma medida da relação existente entre o número de passes não curtos e o número de passes curtos, quando realizados em pressão ou não.
Esta métrica é inspirada na Odds Ratio, muito utilizada, por exemplo, em Epidemiologia. De forma a se perceber em que consiste a Odds Ratio apresento, em seguida, um exemplo que presente verificar a associação entre o facto de pessoas estarem expostas a radiação e serem ou não saudáveis.

Exemplo da Epidemiologia:  Suponhamos que, numa determinada cidade com 1000 pessoas, houve um surto de radiação. Das 400 pessoas que estiveram expostas à radiação, 20 desenvolveram uma determinada doença rara, enquanto que as restantes 380 permaneceram saudáveis. Das restantes 600 pessoas que não estiveram expostas à radiação, 6 desenvolveram a doença e 594 mantiveram-se saudáveis.

	Doentes	Saudáveis
Expostas	20	380
Não expostas	6	594

De entre as pessoas que estiveram expostas à radiação, a odd de ficarem doentes é dada por  Odd_exposta=20/380≈0,0526 e, de entre as pessoas que não estiveram expostas à radiação, a odd de ficarem doentes é dada por Odd_(não exposta)=6/594≈0,0101.
Assim, a Odds Ratio é a razão entre a odd de ficarem doentes, se estiveram expostas à radiação e a odd de ficarem doentes, se não estiveram expostas à radiação, ou seja,

Odds Ratio=(Odd_exposta)/(Odd_(não exposta) )=(20⁄380)/(6⁄594)≈5,2

Para esta situação, podemos concluir que a prevalência de pessoas doentes, comparativamente às saudáveis, quando expostas à radiação, é cerca de cinco vezes superior relativamente ao caso em que não foram expostas à radiação.

	Forma de cálculo

No contexto do futebol, podemos definir a Under Pressure Passing Odds Ratio (UPPOR) como uma Odds Ratio entre a odd dos jogadores fazerem passes não curtos (de média ou longa distância) quando sujeitos a pressão por parte da equipa adversária e a odd dos jogadores fazerem passes não curtos quando não estão sujeitos a pressão. Neste âmbito, urge definir o que consideramos ser um passe curto e um passe de média ou longa distância. Definimos passe curto como sendo um passe com comprimento igual ou inferior a 10 metros, sendo os restantes considerados de média ou longa distância. Para definir um passe realizado sob pressão, usamos a definição da StatsBomb que refere que um passe sob pressão surge quando há uma aproximação de um jogador adversário ao nosso jogador ou o jogador adversário bloqueia uma linha de passe. Assim, 
UPPOR=(Odd_pressionado)/(Odd_(não pressionado) ), onde 
Odd_pressionado=(Número de passes não curtos quando pressionado)/(Número de passes curtos quando pressionado) e
Odd_(não pressionado)=(Número de passes não curtos quando não pressionado)/(Número de passes curtos quando não pressionado) 
 Assim sendo se:
	UPPOR>1, diz-se que existe uma associação positiva entre o número de passes não curtos efetuados e a existência de pressão no passe;
	UPPOR<1, diz-se que existe uma associação negativa entre o número de passes não curtos efetuados e a existência de pressão no passe;
	UPPOR=1, diz-se que não existe associação entre o número de passes não curtos efetuados e a existência de pressão no passe.
Desta forma, a equipa que obtiver valores de UPPOR superiores a 1 está a apostar mais em passes de média ou longa distância, quando pressionada. Se obtiver valores inferiores a 1 está a aumentar a prevalência de passes curtos. Valores próximos de 1 mostram que a existência de pressão não influi na decisão do tipo de passe realizado.   

	Descrição específica de cada uma das variáveis a registar

Para os exemplos presentes no “proof of concept” que se segue, utilizei a base de dados livre da StatsBomb, presente em https://github.com/statsbomb/open-data utilizando, especificamente, dois jogos do EURO2024.
De modo a proceder ao cálculo da UPPOR, necessitamos de identificar os eventos passes em bola corrida da equipa em análise (não estão, portanto, contemplados os pontapés de saída, os pontapés de baliza, lançamentos, livres, cantos e lançamentos de linha lateral) e de registar as seguintes variáveis:

	coordenadas (x,y), em jardas, da localização onde o passe inicia;
	coordenadas (x,y), em jardas, da localização onde o passe termina.

Com base nas coordenadas anteriores podemos, se não houver já essa variável definida na base de dados, definir o comprimento de passe. No caso dos exemplos seguintes, utilizei a variável “pass_length” já existente e converti-a para metros, uma vez que a mesma é apresentada em jardas (uma jarda equivale a 0,9144 metros). Assim sendo, conseguimos contabilizar o número de passes de cada tipo (curto ou de média ou longa distância).
Necessitamos também das seguintes variáveis:
	timestamp do passe e sua duração;
	timestamp da pressão e sua duração.

Um dado passe foi realizado sob pressão se o intervalo de tempo da pressão intersetar o intervalo de tempo do passe. Assim, se não estiver já definido na base de dados, conseguimos definir um identificador (booleano) de pressão realizada (1 para existência de pressão e 0 para não existência de pressão). Nos exemplos apresentados, utilizei a variável “under_pressure” presente na base de dados da StatsBomb. Desta forma, conseguimos contabilizar o número de passes curtos e de média ou longa distância que foram (ou não) realizados sob pressão.  


	Caracterização da amostra

A amostra utilizada diz respeito aos eventos registados em dois jogos de futebol. Nestes, começamos por identificar os eventos passe para uma das equipas intervenientes (nome do evento registado como passe), que não estejam definidos como lançamentos laterais, livres, cantos, pontapés de saída ou pontapés de saída, ou seja, passes “em bola corrida”. Determinamos o comprimento de cada passe, identificando-os como curtos ou de média/longa distância e verificamos se foram realizados sob pressão.





Exemplo 1: Meias-finais do EURO2024 – Países Baixos vs Inglaterra

Observemos as primeiras linhas da tabela que regista os passes em jogo corrido realizados pelos Países Baixos contra a Inglaterra, nas meias-finais do EURO2024:
Número do passe	x	y	x final	y final	Sob pressão	Comprimento do passe
1	30.9	26.7	59.4	36.8	0	30.236732
2	35.0	36.5	41.0	25.0	0	12.971122
3	41.0	25.0	27.0	36.5	0	18.117670
4	26.5	38.2	26.7	62.0	0	23.800840
5	29.7	61.4	40.9	57.8	0	11.764353
…	…	…	…	…	…	…

A tabela contém 414 observações de passes, dos quais 63 foram identificados como curtos e os restantes 351 como médios/longos. 
 
No que diz respeito à variável “sob pressão”, podemos identificar 4 passes curtos sob pressão e 34 passes médios/longos sob pressão. Já no caso de não ter havido pressão, podemos identificar 59 passes curtos e 317 passes médios/longos. Nos gráficos abaixo, vermelho significa ter sido realizado sob pressão e verde significa não ter sido realizado sob pressão.
 


 



Exemplo 2: Final do EURO2024 – Espanha vs Inglaterra

Observemos as primeiras linhas da tabela que regista os passes em jogo corrido realizados pela Espanha contra a Inglaterra, na final do EURO2024:
Número do passe	x	y	x final	y final	Sob pressão	Comprimento do passe
1	8.9	57.2	20.6	75.8	0	21.973848
2	28.6	76.4	50.2	68.5	0	22.999348
3	61.9	67.1	65.1	66.9	0	3.206244
4	70.4	73.3	71.0	78.3	0	5.035871
5	64.5	77.5	56.8	75.5	1	7.955501
…	…	…	…	…	…	…

A tabela contém 549 observações de passes, dos quais 109 foram identificados como curtos e os restantes 440 como médios/longos. 
  
No que diz respeito à variável “sob pressão”, podemos identificar 27 passes curtos sob pressão e 62 passes médios/longos sob pressão. Já no caso de não ter havido pressão, podemos identificar 82 passes curtos e 378 passes médios/longos. Nos gráficos abaixo, vermelho significa ter sido realizado sob pressão e verde significa não ter sido realizado sob pressão.
  







	Resultados obtidos

Tendo em consideração a métrica definida, estes foram os resultados obtidos para os dois exemplos considerados. 

Exemplo 1: Meias-finais do EURO2024 – Países Baixos vs Inglaterra

Neste jogo, os Países Baixos efetuaram, quando pressionados, 34 passes de média ou longa distância e 4 passes curtos. Quando não estavam pressionados, efetuaram 317 passes de média ou longa distância e 59 passes curtos.

	Passes médio/longos	Passes curtos
Pressionado	34	4
Não pressionado	317	59
	
De entre os passes que decorreram quando o jogador estava pressionado, a odd de a equipa fazer passes de médio/longo curso é dada por Odd_pressionado=34/4=8,5 e, de entre os passes que decorreram quando o jogador não estava pressionado, a odd de a equipa fazer passes de médio/longo curso é dada por Odd_(não pressionado)=317/59≈5,37.
Assim, a UPPOR é a razão entre a odd de a equipa fazer passes de médio/longo curso, se os passes decorreram quando o jogador estava pressionado e a odd de a equipa fazer passes de médio/longo curso, se os passes decorreram quando o jogador estava não pressionado, ou seja,

UPPOR=(Odd_pressionado)/(Odd_(não pressionado) )=(34⁄4)/(317⁄59)≈1,58

Neste exemplo, podemos concluir que, quando em pressão, a equipa faz 1,58 vezes mais passes não curtos do que passes curtos, mostrando que existe uma associação positiva entre o número de passes não curtos realizados e o facto de os passes serem realizados sob pressão. Os Países Baixos mostraram, nesta partida, uma tendência a alterar o seu estilo de jogo apostando mais, quando pressionados, nos passes não curtos.










Exemplo 2: Final do EURO2024 – Espanha vs Inglaterra

Neste jogo, Espanha efetuou, quando pressionada, 62 passes de média ou longa distância e 27 passes curtos. Quando não estavam pressionados, efetuaram 378 passes de média ou longa distância e 82 passes curtos.

	Passes médio/longos	Passes curtos
Pressionado	62	27
Não pressionado	378	82
	
De entre os passes que decorreram quando o jogador estava pressionado, a odd de a equipa fazer passes de não curtos é dada por Odd_pressionado=62/27≈2,3 e, de entre os passes que decorreram quando o jogador não estava pressionado, a odd de a equipa fazer passes não curtos é dada por Odd_(não pressionado)=378/82≈4,61.
Assim, a UPPOR é a razão entre a odd de a equipa fazer passes não curtos, se os passes decorreram quando o jogador estava pressionado e a odd de a equipa fazer passes não curtos, se os passes decorreram quando o jogador estava não pressionado, ou seja,

UPPOR=(Odd_pressionado)/(Odd_(não pressionado) )=(62⁄27)/(378⁄82)≈0,5

Neste exemplo, podemos concluir que, quando em pressão, a equipa faz 0,5 vezes mais passes não curtos do que passes curtos, mostrando que existe uma associação negativa entre o número de passes não curtos realizados e o facto de os passes serem realizados sob pressão. Dito de outra forma, quando em pressão, a equipa faz 2 vezes mais passes curtos do que passes não curtos, mostrando que existe uma associação positiva entre o número de passes curtos realizados e o facto de os passes serem realizados sob pressão. Espanha mostrou, nesta partida, que a forma de reagir à pressão da equipa adversária poderia passar por apostar mais nos passes curtos.










	Implicações para a prática

A métrica UPPOR, hoje introduzida, pretende auxiliar as equipas técnicas no sentido medir a associação entre o tipo de passe realizado (curto ou não curto) por uma equipa, numa determinada partida, e a existência de pressão exercida pelo adversário, nos passes realizados. Neste sentido, calculando a UPPOR, conseguem perceber qual a resposta da equipa face à pressão: se existe alguma alteração no padrão de passes realizados face à mesma e, em caso afirmativo, em que sentido foi feita essa alteração. Ou a equipa altera os seus princípios de jogo, passando a ter uma maior prevalência de passes não curtos, ou pretende manter-se fiel aos princípios de jogo da equipa apostando mais ainda, nos passes curtos. Note-se, no entanto, que esta métrica está apenas a medir associação entre variáveis, não pretendendo concluir se a pressão é uma causa para o padrão de passe.
A utilização desta métrica foi pensada para uma contabilização por equipa, ao longo de uma partida de futebol. A mesma não foi implementada a nível individual, por jogador, uma vez que, face ao tempo relativamente baixo de análise poderia, para alguns jogadores, levar a uma não observação de alguma das variáveis em análise, pondo em causa a aplicação da fórmula. Por exemplo, se para um dado jogador, numa determinada partida, não fossem observados passes curtos (estando ou não pressionado) faria com a Odd respetiva não pudesse ter calculada. 
Uma forma de fazer uma implementação da métrica a um nível que não fosse de toda a equipa poderia passar por fazer uma implementação setorial, para tentar verificar, no seio da equipa, quais os setores (defensivo, intermediário, ofensivo) em que a mudança do padrão de passes é mais evidente. Visto por um outro prisma, a verificação das zonas do campo onde a pressão adversária está mais associada à mudança do padrão de passes seria outra vertente a considerar.
Apesar de, como antes referi, a implementação a nível individual da métrica pudesse não ser fazível, ao utilizar dados agregados ao longo de várias partidas, aumentando o tamanho da nossa amostra, poderia começar a poder dar-nos insights sobre a visão individual da métrica. 
A implementação da métrica traz alguns desafios a nível técnico. Por um lado, necessitamos de conhecer as coordenadas de início e fim do evento (passe) de modo a determinar o comprimento do passe e classificá-lo como curto ou não. Alguns testes poderiam ser realizados sobre qual seria o valor ótimo de distância a considerar. Por exemplo, analisar passes longos (distância superior a 20 metros) face a passes não longos poderia ser outra abordagem.  
O conceito de passe feito sob pressão é outro dos desafios a considerar. Para este trabalho, trabalhei sobre a definição da StatsBomb, embora seja um pouco blackbox, não tendo encontrado consenso face à distância do jogador adversário ao que faz o passe, para que seja considerado under-pressure ou qual a definição específica de bloquear a linha de passe. Uma vez que diferentes fornecedores de dados podem definir pressão de forma distinta, encontrar uma definição consensual que sirva os propósitos será o próximo passo a dar.  
Em suma, este exercício permitiu refletir sobre a implementação de métricas individuais e coletivas no contexto de uma equipa de futebol, consolidando os conteúdos abordados ao longo do módulo. Para além de saber que o número de métricas passíveis de ser implementadas ser infinito, também dentro de cada métrica introduzida, há muitas particularidades a serem consideradas. A implementação desta métrica poderá ser um auxílio neste caso específico. 
