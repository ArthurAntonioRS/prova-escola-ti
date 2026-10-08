# Especificação

## UC 1 - Abrir bilhete

### Endpoint

POST /bilhetes

### Entrada

O endpoint recebe a placa do veículo

A placa deve possuir exatamente 7 caracteres alfanuméricos e utilizar letras maiúsculas

O campo `entrada` é opcional. Quando informado, deve seguir o formato ISO-8601 com fuso horário e será utilizado como o instante de abertura do bilhete

Quando `entrada` não for informada, deve ser utilizado o instante atual

Quando `entrada` estiver em formato inválido, a API deve retornar HTTP 422 com `{"erro": "entrada_invalida"}`

### Resposta de sucesso

Após validar a placa e verificar que não existe outro bilhete aberto para a mesma placa, a API deve criar o bilhete e retornar HTTP 201 contendo:

* `id`
* `placa`
* `entrada`
* `status`

O campo `status` deve possuir o valor `"aberto"`

### Critérios de aceite

Uma placa válida que não possua outro bilhete aberto deve resultar na criação de um novo bilhete

O bilhete criado deve possuir `status: "aberto"`

A criação deve retornar HTTP 201

Uma placa que já possua um bilhete aberto deve retornar HTTP 409 com `{"erro": "bilhete_em_aberto"}`

Uma placa ausente ou inválida deve retornar HTTP 422 com `{"erro": "placa_invalida"}`

Uma entrada inválida deve retornar HTTP 422 com `{"erro": "entrada_invalida"}`

## UC 2 - Encerrar bilhete

### Endpoint

POST /bilhetes/{id}/encerramento

### Entrada

O endpoint recebe o identificador do bilhete pela URL

O bilhete informado deve existir e possuir status `aberto`

### Resposta de sucesso

Quando o bilhete puder ser encerrado, a API deve retornar HTTP 200 contendo:

* `id`
* `placa`
* `entrada`
* `saida`
* `minutos`
* `valor_centavos`

A `saida` deve representar o instante do encerramento

### Regras de cobrança

O tempo deve ser calculado em minutos

A cobrança deve utilizar frações de `FRACAO_MINUTOS`, sempre arredondando para cima

Uma duração exatamente igual ao tamanho da fração deve cobrar uma única fração

Um minuto adicional deve resultar na cobrança da próxima fração

O valor de uma fração deve ser calculado a partir de `TARIFA_HORA_CENTAVOS`

O valor final nunca pode ultrapassar `TETO_DIARIO_CENTAVOS`

O valor deve ser retornado somente como número inteiro em centavos

### Critérios de aceite

Um bilhete aberto deve poder ser encerrado e retornar HTTP 200

Um bilhete inexistente deve retornar HTTP 404 com `{"erro": "bilhete_nao_encontrado"}`

Um bilhete já encerrado deve retornar HTTP 409 com `{"erro": "bilhete_ja_encerrado"}`

O valor calculado deve respeitar a fração de cobrança e o teto diário

O valor retornado nunca deve ser um número de ponto flutuante

## UC 3 - Listar bilhetes ativos

### Endpoint

GET /bilhetes/ativos

### Resposta de sucesso

O endpoint deve retornar HTTP 200 contendo um array com os bilhetes que possuem status `aberto`

Os bilhetes devem ser apresentados do mais recente para o mais antigo

### Critérios de aceite

Somente bilhetes com status `aberto` devem aparecer

Bilhetes encerrados não devem aparecer

Bilhetes cancelados não devem aparecer

Quando não houver bilhetes ativos, a API deve retornar HTTP 200 com um array vazio

## UC 4 - Relatório diário

### Endpoint

GET /relatorios/diario?data=AAAA-MM-DD

### Entrada

O parâmetro `data` deve seguir exatamente o formato `AAAA-MM-DD`

Quando a data estiver inválida, a API deve retornar HTTP 422 com `{"erro": "data_invalida"}`

### Resposta de sucesso

A API deve retornar HTTP 200 contendo:

* `data`
* `total_bilhetes`
* `faturamento_centavos`
* `tempo_medio_minutos`

O relatório deve considerar somente os bilhetes encerrados na data informada

O faturamento deve ser apresentado em centavos inteiros

O tempo médio deve ser arredondado com `0,5` para cima

### Critérios de aceite

Uma data válida deve retornar HTTP 200

Uma data inválida deve retornar HTTP 422

O total de bilhetes deve representar os bilhetes encerrados no dia informado

O faturamento deve representar a soma dos valores dos bilhetes encerrados no dia

O tempo médio deve considerar somente os bilhetes encerrados no dia

## UC 5 - Cancelar bilhete

### Endpoint

POST /bilhetes/{id}/cancelamento

### Entrada

O identificador do bilhete é informado pela URL

O bilhete deve existir e possuir status `aberto`

### Resposta de sucesso

Quando o cancelamento for realizado, a API deve retornar HTTP 200 com o status `"cancelado"`

### Regras

Somente bilhetes abertos podem ser cancelados

O cancelamento não gera cobrança

Um bilhete cancelado não deve possuir `saida` nem `valor_centavos`

### Critérios de aceite

Um bilhete aberto deve poder ser cancelado

Um bilhete inexistente deve retornar HTTP 404 com `{"erro": "bilhete_nao_encontrado"}`

Um bilhete encerrado deve retornar HTTP 409 com `{"erro": "bilhete_nao_aberto"}`

Um bilhete cancelado também deve retornar HTTP 409 com `{"erro": "bilhete_nao_aberto"}`

Após o cancelamento, a placa deve poder abrir um novo bilhete

## UC 6 - Histórico por placa

### Endpoint

GET /bilhetes?placa=ABC1D23

### Resposta de sucesso

A API deve retornar HTTP 200 contendo um array com todos os bilhetes associados à placa informada

O histórico deve incluir bilhetes com qualquer status

Os resultados devem ser apresentados do mais recente para o mais antigo

Quando a placa nunca tiver estacionado, deve ser retornado um array vazio

### Critérios de aceite

Todos os bilhetes da placa devem ser retornados

Bilhetes abertos, encerrados e cancelados devem aparecer

Bilhetes pertencentes a outras placas não devem aparecer

A resposta deve ser HTTP 200

## UC 7 - Tolerância gratuita

### Regra

Os primeiros `TOLERANCIA_MINUTOS` de um bilhete são gratuitos

Quando a duração for menor ou igual à tolerância, o valor deve ser `0`

Quando a duração ultrapassar a tolerância, a cobrança deve ocorrer desde o primeiro minuto

A tolerância não deve ser descontada do tempo utilizado para calcular a cobrança

### Critérios de aceite

Uma duração igual à tolerância deve resultar em `valor_centavos: 0`

Uma duração menor que a tolerância deve resultar em `valor_centavos: 0`

Uma duração um minuto acima da tolerância deve gerar cobrança integral desde o primeiro minuto

Quando `TOLERANCIA_MINUTOS` for `0`, a cobrança deve ocorrer normalmente

## UC 8 - Uma vaga por placa

### Regra

Uma placa não pode possuir mais de um bilhete com status `aberto`

Antes de criar um novo bilhete, o sistema deve verificar se existe outro bilhete aberto para a mesma placa

### Critérios de aceite

Uma placa sem bilhete aberto deve poder criar um novo bilhete

Uma placa com bilhete aberto deve retornar HTTP 409 com `{"erro": "bilhete_em_aberto"}`

Uma placa cujo bilhete anterior foi encerrado deve poder criar um novo bilhete

Uma placa cujo bilhete anterior foi cancelado deve poder criar um novo bilhete

## Erros

| Situação                           | Status | Body                                 |
| ---------------------------------- | -----: | ------------------------------------ |
| Placa ausente ou inválida          |    422 | `{"erro": "placa_invalida"}`         |
| `entrada` fora de ISO-8601         |    422 | `{"erro": "entrada_invalida"}`       |
| `data` fora de `AAAA-MM-DD`        |    422 | `{"erro": "data_invalida"}`          |
| Bilhete inexistente                |    404 | `{"erro": "bilhete_nao_encontrado"}` |
| Encerrar bilhete já encerrado      |    409 | `{"erro": "bilhete_ja_encerrado"}`   |
| Cancelar bilhete não aberto        |    409 | `{"erro": "bilhete_nao_aberto"}`     |
| Abrir bilhete com placa já ocupada |    409 | `{"erro": "bilhete_em_aberto"}`      |