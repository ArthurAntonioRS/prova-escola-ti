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