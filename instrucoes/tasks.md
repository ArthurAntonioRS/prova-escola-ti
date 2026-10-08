# Tarefas

## UC1 — Abrir bilhete

* Criar o endpoint `POST /bilhetes`
* Implementar a validação da placa
* Implementar o tratamento do campo `entrada`
* Verificar se já existe um bilhete aberto para a placa
* Criar e persistir o bilhete
* Configurar as respostas HTTP `201`, `409` e `422`
* Validar utilizando os casos de teste definidos

## UC2 — Encerrar bilhete

* Criar o endpoint `POST /bilhetes/{id}/encerramento`
* Localizar o bilhete pelo identificador
* Verificar se o bilhete pode ser encerrado
* Calcular a duração entre entrada e saída
* Aplicar a cobrança por fração
* Aplicar o teto diário
* Persistir os dados do encerramento
* Configurar os erros de bilhete inexistente e já encerrado
* Validar utilizando os casos de teste definidos

## UC3 — Listar bilhetes ativos

* Criar o endpoint `GET /bilhetes/ativos`
* Consultar os bilhetes com status `aberto`
* Ordenar os resultados do mais recente para o mais antigo
* Retornar array vazio quando não houver bilhetes ativos
* Validar utilizando os casos de teste definidos

## UC4 — Relatório diário

* Criar o endpoint `GET /relatorios/diario`
* Validar o parâmetro `data`
* Selecionar os bilhetes encerrados na data informada
* Calcular o total de bilhetes
* Calcular o faturamento em centavos
* Calcular e arredondar o tempo médio
* Configurar o erro de data inválida
* Validar utilizando os casos de teste definidos

## UC5 — Cancelar bilhete

* Criar o endpoint `POST /bilhetes/{id}/cancelamento`
* Localizar o bilhete pelo identificador
* Verificar se o bilhete possui status `aberto`
* Alterar o status para `cancelado`
* Garantir que o cancelamento não gere cobrança ou saída
* Configurar os erros de bilhete inexistente e não aberto
* Validar utilizando os casos de teste definidos

## UC6 — Histórico por placa

* Criar o endpoint `GET /bilhetes`
* Validar e receber a placa como parâmetro de consulta
* Buscar todos os bilhetes associados à placa
* Incluir bilhetes de qualquer status
* Ordenar os resultados do mais recente para o mais antigo
* Retornar array vazio quando não houver histórico
* Validar utilizando os casos de teste definidos

## UC7 — Tolerância gratuita

* Aplicar `TOLERANCIA_MINUTOS` no cálculo da cobrança
* Garantir valor zero quando a duração estiver dentro da tolerância
* Garantir cobrança integral quando a tolerância for ultrapassada
* Garantir que a tolerância não seja descontada da duração cobrada
* Validar os limites da tolerância utilizando os casos de teste definidos

## UC8 — Uma vaga por placa

* Verificar a existência de bilhete aberto antes de criar um novo
* Impedir a criação de um segundo bilhete aberto para a mesma placa
* Retornar HTTP 409 quando a placa estiver ocupada
* Permitir nova abertura após encerramento
* Permitir nova abertura após cancelamento
* Validar os conflitos utilizando os casos de teste definidos