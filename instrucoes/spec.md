## UC 1 - abrir bilhete

### Endpoint
POST /bilhetes

### Entrada
O endpoint vai receber o objeto 'placa'

A placa deve possuir especificamente 7 caracteres alfanuméricos maiúsculos

A 'entrada' é opcional e quando presente segue o ISO-8601 com fuso, abrindo o bilhete naquele instante em vez de "agora"

A 'entrada' não informada usa o tempo atual

Quando a 'entrada' está no formato errado, a API retorna um 422 '{"erro": "entrada_invalida"}'

### Resposta de sucesso

A placa é validada e se não tiver um bilhete aberto com a mesma placa, a API retorna um HTTP 201 com:

'id'
'placa'
'entrada'
'status'

O 'status' deve estar 'aberto'

### Critérios de aceite

Uma placa que o sistema aceita sem um bilhete aberto cria um novo bilhete

O bilhete novo deve ter o 'status: "aberto"'

A resposta de criação deve enviar um HTTP 201

Uma placa com um bilhete aberto retorna um HTTP 409 com o erro '{"erro":"bilhete_em_aberto"}'

Placas inválidas retornam um HTTP 422