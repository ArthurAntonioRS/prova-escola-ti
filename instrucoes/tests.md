# Casos de Teste

## UC1 — Abrir bilhete

Cenário Entrada Esperado
Placa válida  'ABC1D23'  HTTP 201 e status 'aberto' 
Placa ausente  sem 'placa'  HTTP 422 'placa_invalida' 
Placa com 6 caracteres e 8 caracteres  'ABC123' e 'ABC12345'  HTTP 422 'placa_invalida' 
Placa em minúsculas  'abc1d23'  HTTP 422  'placa_invalida' 
Entrada ISO-8601 válida  entrada válida   HTTP 201 
Entrada inválida  'data-invalida'   HTTP 422 'entrada_invalida' 
Placa já aberta   placa com bilhete aberto   HTTP 409 'bilhete_em_aberto' 

## Critérios adicionais

Depois de um bilhete válido ser criado:

O status deve ser o 'aberto'

O identificador único crescente é criado

A placa que é retornada tem que ser igual à placa enviada

A entrada corresponde ao valor informado ou seu instante quando a entrada não for fornecida


## UC2 — Encerrar bilhete

Cenário	Entrada	Esperado

Bilhete aberto  ID de bilhete aberto	HTTP 200 com saida, minutos e valor_centavos

Bilhete inexistente	ID inexistente	HTTP 404 bilhete_nao_encontrado

Bilhete já encerrado	ID de bilhete encerrado	HTTP 409 bilhete_ja_encerrado

Duração exata da fração	duração igual a FRACAO_MINUTOS	Cobrança de exatamente 1 fração

Um minuto acima da fração	FRACAO_MINUTOS + 1	Cobrança da próxima fração

Valor acima do teto	duração suficiente para ultrapassar o teto	Valor limitado a TETO_DIARIO_CENTAVOS

## Critérios adicionais

Depois de um bilhete aberto ser encerrado:

O status deixa de ser aberto

A resposta deve possuir id, placa, entrada, saida, minutos e valor_centavos

A saida deve representar o momento do encerramento

O valor deve ser calculado utilizando a FRACAO_MINUTOS, TARIFA_HORA_CENTAVOS e TETO_DIARIO_CENTAVOS

## UC3 — Listar ativos

Cenário	Entrada	Esperado
Existem bilhetes abertos	GET /bilhetes/ativos	HTTP 200 com os bilhetes abertos
Não existem bilhetes ativos	Nenhum bilhete aberto	HTTP 200 com array vazio
Bilhete encerrado	Bilhete com status encerrado	Não deve aparecer na lista
Bilhete cancelado	Bilhete com status cancelado	Não deve aparecer na lista
Vários bilhetes ativos	Mais de um bilhete aberto	Mais recentes primeiro

## Critérios adicionais

A resposta deve ser um array

Apenas bilhetes com status aberto podem aparecer

A ordenação deve colocar os bilhetes mais recentes primeiro

## UC4 — Relatório diário

Cenário	Entrada	Esperado
Data válida	2026-10-05	HTTP 200 com relatório
Data inválida	05-10-2026	HTTP 422 data_invalida
Data sem bilhetes	Data sem encerramentos	HTTP 200 com valores zerados
Bilhetes encerrados no dia	Data com bilhetes encerrados	Contabiliza os bilhetes e faturamento
Média com resultado exato	Tempos cuja média seja inteira	Retorna a média inteira
Média com .5	Média de 47,5	Retorna 48

## Critérios adicionais

O relatório deve retornar:

'data'
'total_bilhetes'
'faturamento_centavos'
'tempo_medio_minutos'

O 'total_bilhetes', faturamento e tempo médio devem considerar somente os bilhetes encerrados na data informada

## UC5 — Cancelar bilhete

Cenário	Entrada	Esperado
Bilhete aberto	ID de bilhete aberto	HTTP 200 com status: "cancelado"
Bilhete inexistente	ID inexistente	HTTP 404 bilhete_nao_encontrado
Bilhete encerrado	ID de bilhete encerrado	HTTP 409 bilhete_nao_aberto
Bilhete já cancelado	ID de bilhete cancelado	HTTP 409 bilhete_nao_aberto

## Critérios adicionais

Após o cancelamento:

O status deve ser cancelado

Não deve existir cobrança

Não deve ser gerada saida

Não deve ser gerado valor_centavos

A placa volta a poder abrir um novo bilhete

## UC6 — Histórico por placa

Cenário	Entrada	Esperado
Placa com histórico	ABC1D23	HTTP 200 com todos os bilhetes
Placa sem histórico	Placa inexistente	HTTP 200 com array vazio
Placa com bilhete encerrado	Placa com encerramento	Bilhete aparece no histórico
Placa com bilhete cancelado	Placa com cancelamento	Bilhete aparece no histórico
Vários bilhetes	Placa com vários registros	Mais recentes primeiro

## Critérios adicionais

A consulta deve utilizar a placa informada

Todos os status devem ser considerados

Nenhum bilhete de outra placa deve aparecer

A resposta deve sempre ser um array

## UC7 — Tolerância gratuita

Cenário	Entrada	Esperado
Duração igual à tolerância	TOLERANCIA_MINUTOS	valor_centavos = 0
Duração abaixo da tolerância	Menor que TOLERANCIA_MINUTOS	valor_centavos = 0
Um minuto acima da tolerância	TOLERANCIA_MINUTOS + 1	Cobra desde o primeiro minuto
Tolerância igual a zero	TOLERANCIA_MINUTOS = 0	Cobrança normal
Duração acima da tolerância	Tempo superior à tolerância	Aplica cobrança integral

## Critérios adicionais

Quando a duração estiver dentro da tolerância, o valor deve ser 0

Quando a duração ultrapassar a tolerância, o cálculo deve considerar todo o período desde a entrada, e não somente o período após a tolerância

## UC8 — Uma vaga por placa

Cenário	Entrada	Esperado
Placa sem bilhete aberto	ABC1D23	HTTP 201
Placa com bilhete aberto	ABC1D23	HTTP 409 bilhete_em_aberto
Placa com bilhete encerrado	ABC1D23	HTTP 201
Placa com bilhete cancelado	ABC1D23	HTTP 201
Novo bilhete após encerramento	Placa anteriormente encerrada	Novo bilhete criado
Novo bilhete após cancelamento	Placa anteriormente cancelada	Novo bilhete criado

## Critérios adicionais

Antes de criar um bilhete, o sistema deve verificar se existe outro bilhete da mesma placa com status aberto

Caso exista, a criação deve ser recusada com HTTP 409 e:

{"erro": "bilhete_em_aberto"}

Caso o bilhete anterior esteja encerrado ou cancelado, a placa poderá abrir um novo bilhete