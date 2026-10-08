# Regras Operacionais

A placa deve conter exatamente 7 caracteres alfanuméricos, com letras sempre em maiúsculas

Datas e horários devem seguir o padrão ISO-8601 com fuso horário

O valor cobrado deve ser calculado em frações de `FRACAO_MINUTOS`, sempre arredondando o tempo para cima

O valor de uma fração deve ser calculado a partir de `TARIFA_HORA_CENTAVOS`

O valor cobrado nunca pode ultrapassar `TETO_DIARIO_CENTAVOS`

Os primeiros `TOLERANCIA_MINUTOS` de um bilhete são gratuitos. Caso a tolerância seja ultrapassada, a cobrança deve ser feita desde o primeiro minuto

Valores monetários devem ser representados exclusivamente como centavos inteiros

Os bilhetes devem possuir identificador único e crescente, iniciado em 1

A API deve utilizar a porta definida por `PORTA_SERVICO`

Os endpoints devem documentar suas respostas de sucesso e de erro

Uma placa pode possuir no máximo um bilhete com status `aberto` simultaneamente