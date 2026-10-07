## Plano 

### API

A aplicação disponibiliza a API REST e ouve a porta 8002

## Persistência

Os bilhetes devem ser mantidos em persistencia, pois a aplicação permite uma busca por placas

Essa persistencia criada identifica os status atuais de cada bilhete e verifica se cada placa possui um bilhete ativo

## Identificação

Os bilhetes obrigatóriamente devem ter um identificados único e crescente iniciado em 1

## Datas

As datas são armazendas e retornadas com o seu fuso horário específico

O informar 'entrada' manualmente é obrigatório para permitir testes determinísticos