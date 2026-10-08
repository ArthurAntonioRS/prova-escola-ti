# Plano

## API

A aplicação deve disponibilizar uma API REST.

O serviço deve utilizar a porta definida por `PORTA_SERVICO`.

A API deve seguir exatamente os endpoints, métodos HTTP, estruturas de resposta e códigos de status definidos no contrato.

## Persistência

Os bilhetes devem ser mantidos em uma estrutura de persistência durante a execução da aplicação.

A persistência deve permitir consultar bilhetes por identificador e por placa.

Também deve permitir verificar rapidamente se uma placa possui um bilhete com status `aberto`.

## Identificação

Cada bilhete deve possuir um identificador único e crescente iniciado em 1.

O identificador deve permanecer associado ao mesmo bilhete durante todo o seu ciclo de vida.

## Datas e horários

Os horários de entrada e saída devem ser armazenados de forma que o fuso horário seja preservado.

O campo `entrada` informado pelo cliente deve permitir testes determinísticos.

Quando `entrada` não for informada, deve ser utilizado o horário atual.

O cálculo da duração deve utilizar os instantes de entrada e saída.

## Cálculo de cobrança

O cálculo deve utilizar `TARIFA_HORA_CENTAVOS` e `FRACAO_MINUTOS`.

A quantidade de frações deve ser arredondada para cima.

O valor monetário deve ser calculado e armazenado exclusivamente como centavos inteiros, evitando ponto flutuante.

Após o cálculo da cobrança, deve ser aplicado `TETO_DIARIO_CENTAVOS`.

## Tolerância

O cálculo deve verificar `TOLERANCIA_MINUTOS` antes de realizar a cobrança.

Se a duração estiver dentro da tolerância, o valor deve ser zero.

Se a duração ultrapassar a tolerância, o cálculo deve considerar o período integral desde a entrada.

## Relatório

O relatório diário deve selecionar os bilhetes encerrados na data solicitada.

O faturamento deve ser obtido pela soma dos valores cobrados desses bilhetes.

O tempo médio deve ser calculado utilizando somente os tempos dos bilhetes encerrados no dia e arredondado com `0,5` para cima.

## Ordenação

As consultas de bilhetes ativos e histórico por placa devem apresentar os registros mais recentes primeiro.

## Validação

As validações devem ocorrer antes das operações de criação, encerramento ou cancelamento.

Cada erro deve utilizar o código HTTP e o corpo definidos no contrato.