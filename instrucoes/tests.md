# Casos de Teste

## UC1 — Abrir bilhete

| Cenário                 | Entrada                  | Esperado                     |
| ----------------------- | ------------------------ | ---------------------------- |
| Placa válida            | `ABC1D23`                | HTTP 201 e status `aberto`   |
| Placa ausente           | Sem `placa`              | HTTP 422 `placa_invalida`    |
| Placa com 6 caracteres  | `ABC123`                 | HTTP 422 `placa_invalida`    |
| Placa com 8 caracteres  | `ABC12345`               | HTTP 422 `placa_invalida`    |
| Placa em minúsculas     | `abc1d23`                | HTTP 422 `placa_invalida`    |
| Entrada ISO-8601 válida | Entrada válida com fuso  | HTTP 201                     |
| Entrada inválida        | `data-invalida`          | HTTP 422 `entrada_invalida`  |
| Placa já aberta         | Placa com bilhete aberto | HTTP 409 `bilhete_em_aberto` |

## UC2 — Encerrar bilhete

| Cenário              | Entrada                          | Esperado                             |
| -------------------- | -------------------------------- | ------------------------------------ |
| Bilhete aberto       | ID existente e aberto            | HTTP 200                             |
| Bilhete inexistente  | ID inexistente                   | HTTP 404 `bilhete_nao_encontrado`    |
| Bilhete já encerrado | ID encerrado                     | HTTP 409 `bilhete_ja_encerrado`      |
| Fração exata         | Duração = `FRACAO_MINUTOS`       | Cobrança de 1 fração                 |
| Um minuto acima      | Duração = `FRACAO_MINUTOS + 1`   | Cobrança da próxima fração           |
| Teto atingido        | Valor calculado maior que o teto | Valor igual a `TETO_DIARIO_CENTAVOS` |

## UC3 — Listar ativos

| Cenário           | Entrada                       | Esperado                         |
| ----------------- | ----------------------------- | -------------------------------- |
| Bilhetes ativos   | Dois ou mais bilhetes abertos | HTTP 200, mais recentes primeiro |
| Nenhum ativo      | Nenhum bilhete aberto         | HTTP 200 com array vazio         |
| Bilhete encerrado | Bilhete encerrado             | Não aparece                      |
| Bilhete cancelado | Bilhete cancelado             | Não aparece                      |

## UC4 — Relatório diário

| Cenário             | Entrada                | Esperado                           |
| ------------------- | ---------------------- | ---------------------------------- |
| Data válida         | `2026-10-05`           | HTTP 200                           |
| Data inválida       | `05-10-2026`           | HTTP 422 `data_invalida`           |
| Sem bilhetes        | Data sem encerramentos | HTTP 200 com valores zerados       |
| Bilhetes encerrados | Data com encerramentos | Contabiliza bilhetes e faturamento |
| Média inteira       | Média = `47`           | `tempo_medio_minutos = 47`         |
| Média com 0,5       | Média = `47,5`         | `tempo_medio_minutos = 48`         |

## UC5 — Cancelar bilhete

| Cenário                        | Entrada        | Esperado                          |
| ------------------------------ | -------------- | --------------------------------- |
| Bilhete aberto                 | ID aberto      | HTTP 200 com status `cancelado`   |
| Bilhete inexistente            | ID inexistente | HTTP 404 `bilhete_nao_encontrado` |
| Bilhete encerrado              | ID encerrado   | HTTP 409 `bilhete_nao_aberto`     |
| Bilhete cancelado              | ID cancelado   | HTTP 409 `bilhete_nao_aberto`     |
| Novo bilhete após cancelamento | Mesma placa    | HTTP 201                          |

## UC6 — Histórico por placa

| Cenário             | Entrada                   | Esperado                       |
| ------------------- | ------------------------- | ------------------------------ |
| Placa com histórico | `ABC1D23`                 | HTTP 200 com todos os bilhetes |
| Placa sem histórico | Placa inexistente         | HTTP 200 com array vazio       |
| Bilhete aberto      | Placa com bilhete aberto  | Bilhete aparece                |
| Bilhete encerrado   | Placa com encerramento    | Bilhete aparece                |
| Bilhete cancelado   | Placa com cancelamento    | Bilhete aparece                |
| Vários registros    | Placa com vários bilhetes | Mais recentes primeiro         |

## UC7 — Tolerância gratuita

| Cenário                  | Entrada                                | Esperado                      |
| ------------------------ | -------------------------------------- | ----------------------------- |
| Abaixo da tolerância     | Duração menor que `TOLERANCIA_MINUTOS` | `valor_centavos = 0`          |
| Exatamente na tolerância | Duração = `TOLERANCIA_MINUTOS`         | `valor_centavos = 0`          |
| Um minuto acima          | Duração = `TOLERANCIA_MINUTOS + 1`     | Cobra desde o primeiro minuto |
| Tolerância zero          | `TOLERANCIA_MINUTOS = 0`               | Cobrança normal               |

## UC8 — Uma vaga por placa

| Cenário                     | Entrada     | Esperado                     |
| --------------------------- | ----------- | ---------------------------- |
| Placa sem bilhete aberto    | Nova placa  | HTTP 201                     |
| Placa com bilhete aberto    | Mesma placa | HTTP 409 `bilhete_em_aberto` |
| Placa com bilhete encerrado | Mesma placa | HTTP 201                     |
| Placa com bilhete cancelado | Mesma placa | HTTP 201                     |
