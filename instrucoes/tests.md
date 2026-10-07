# Casos de Teste

## UC1 — Abrir bilhete

| Cenário | Entrada | Esperado |
|---|---|---|
| Placa válida | 'ABC1D23' | HTTP 201 e status 'aberto' |
| Placa ausente | sem 'placa' | HTTP 422 'placa_invalida' |
| Placa com 6 caracteres e 8 caracteres | 'ABC123' e 'ABC12345' | HTTP 422 'placa_invalida' |
| Placa em minúsculas | 'abc1d23' | HTTP 422 'placa_invalida' |
| Entrada ISO-8601 válida | entrada válida | HTTP 201 |
| Entrada inválida | 'data-invalida' | HTTP 422 'entrada_invalida' |
| Placa já aberta | placa com bilhete aberto | HTTP 409 'bilhete_em_aberto' |

## Critérios adicionais

Depois de um bilhete válido ser criado:

O status deve ser o 'aberto'

O identificador único crescente é criado

A placa que é retornada tem que ser igual à placa enviada

A entrada corresponde ao valor informado ou seu instante quando a entrada não for fornecida