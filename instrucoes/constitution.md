## Regras Operacionais

A placa deve conter 7 caracteres alfanuméricos, todas as letras devem ser maiúsculas

Datas e horários devem seguir o padrão ISO-8601 com fuso

O valor cobrado será por fração, caso passe 1 minuto, a próxima fração já é cobrada

O valor sempre será em centavos inteiros, caso o centavo fique quebrado, ele deve ser arredondado para cima

A API utiliza a porta 8002

Os endpoints do sistema devem documentar as respostas de erro e sucesso

O encerramento só pode ocorrer em bilhetes existentes

Um bilhete já encerrado não pode ser encerrado novamente

O tempo de permanência deve ser calculado em minutos inteiros

A cobrança deve arredondar o tempo para cima conforme a FRACAO_MINUTOS

O valor cobrado nunca pode ultrapassar o TETO_DIARIO_CENTAVOS

O valor retornado deve ser sempre um número inteiro em centavos