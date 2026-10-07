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

Somente bilhetes com status aberto devem ser considerados ativos

Os bilhetes ativos devem ser apresentados do mais recente para o mais antigo

O endpoint deve retornar um array, mesmo quando não houver bilhetes ativos

A data informada no relatório deve seguir o formato AAAA-MM-DD

O relatório deve considerar os bilhetes encerrados no dia informado

O faturamento deve ser apresentado em centavos inteiros

O tempo médio deve ser arredondado com 0,5 para cima

Somente bilhetes com status aberto podem ser cancelados

O cancelamento não gera cobrança

Um bilhete cancelado não pode ser cancelado novamente

Um bilhete cancelado não deve possuir saida ou valor_centavos

A consulta deve retornar todos os bilhetes associados à placa informada

O histórico deve incluir bilhetes abertos, encerrados e cancelados

Os resultados devem ser apresentados do mais recente para o mais antigo

Uma placa sem histórico deve retornar um array vazio

A tolerância inicial definida por TOLERANCIA_MINUTOS é gratuita

Se a duração for igual ou menor que a tolerância, o valor será zero

Se a duração ultrapassar a tolerância, a cobrança será feita desde o primeiro minuto

A tolerância não deve ser descontada do tempo cobrado

Uma placa não pode possuir mais de um bilhete com status aberto

Uma placa com bilhete encerrado pode abrir um novo bilhete

Uma placa com bilhete cancelado pode abrir um novo bilhete

A verificação deve ocorrer antes da criação do novo bilhete