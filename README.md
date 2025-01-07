# Somativa
# API de Integração Plannera

**Versão:** 1.0  
**Data:** 07/01/2025  
**Criado por:** Pedro Altran  
**Revisado por:** Guilherme Polim e Pedro Altran

---
 
## Visão Geral 

Esta API fornece endpoints para autenticação e recebimento de dados provenientes da plataforma Plannera. A API utiliza autenticação baseada em JWT (JSON Web Token) e possui validação dos dados enviados.

## Índice
1. [Autenticação](#autenticação)
2. [Endpoints](#endpoints)
3. [Validações](#validações)
4. [Exemplos de Uso](#exemplos-de-uso)

## Autenticação

  

### Endpoint: `/generate_token`
**Método:** POST

Gera um token JWT para autenticação nas demais rotas da API.  

#### Requisição

```json

{

"username": "seu_usuario",

"password": "sua_senha"

}

```

  

#### Resposta de Sucesso

-  **Código:** 200

-  **Conteúdo:**

```json

{

"token": "seu_jwt_token"

}

```

  

#### Respostas de Erro

-  **400:** Corpo da requisição inválido ou campos obrigatórios ausentes

-  **401:** Credenciais inválidas

  

### Autenticação nas Requisições

Para endpoints de envio de dados, inclua o token no cabeçalho Authorization:

```

Authorization: Bearer seu_jwt_token

```

  

## Endpoints

  

### `/report_c_connection_bi_1`

**Método:** POST

  

Endpoint para envio de dados do relatório Conexão Bi 1.

  

#### Estrutura dos Dados

```json

{

"ID": "string",

"VERSAO": "string",

"TIPO": "string",

"MES": "string",

"BU": "string",

"REGIONAL": "string",

"SEGMENTO": "string",

"CODIGO_PRODUTO": "integer",

"DESCRICAO_PRODUTO": "string",

"MARCA": "string",

"VOLUME": "float",

"PRECO_UNITARIO": "float",

"RECEITA_TOTAL": "float",

"CENTRO_CUSTO": "integer",

"TERR": "string",

"CD": "integer",

"VOLUME_SOLICITADO": "float",

"VALOR_SOLICITADO": "float"

}

```

  

#### Formatos Aceitos

- Objeto único

- Array de objetos

  

#### Resposta de Sucesso

-  **Código:** 200

-  **Conteúdo:**

```json

{

"data": {

"envelope": {

"total_records": "número_de_registros"

},

"columns": ["lista_de_colunas"],

"records": [

["valores_do_registro"]

]

}

}

```

  

#### Respostas de Erro

-  **400:** Dados inválidos ou mal formatados

-  **401:** Token ausente, expirado ou inválido

  

## Validações

  

### Validação de Dados

O sistema realiza as seguintes validações:

- Presença de todos os campos obrigatórios

- Tipos de dados corretos para cada campo

- Presença de campos extras não especificados

  

### Erros de Validação

Em caso de erro na validação, a resposta incluirá:

- Campos ausentes

- Campos extras não permitidos

- Campos com tipos de dados incorretos

  

## Observações Importantes

1. O token JWT expira após 1 hora

3. A API aceita tanto objetos únicos quanto arrays de objetos

  

## Exemplos de Uso

  
### 1. Obter Token
##### https://plannera.azurewebsites.net/api/generate_token
#### cURL
```bash

curl  -X  POST  https://plannera.azurewebsites.net/api/generate_token  \

-H "Content-Type: application/json" \

-d  '{"username": "seu_usuario", "password": "sua_senha"}'

```
##
#### Python
```bash
response = requests.post(
"https://plannera.azurewebsites.net/api/generate_token",
json={"username":  "seu_usuario",  "password":  "sua_senha"}) 
token = response.json()["token"]
```

 
##

### 2. Enviar Dados

##### https://plannera.azurewebsites.net/api/report_c_connection_bi_1


#### cURL
```bash

curl  -X  POST  https://sua-api/report_c_connection_bi_1  \

-H "Authorization: Bearer seu_token" \

-H  "Content-Type: application/json"  \

-d '{

"ID":  "001",

"VERSAO":  "1.0",

"TIPO":  "VENDA",

"MES":  "202401",

...

}'
```
##
#### Python
```bash
dados =  { "ID":  "001", "VERSAO":  "1.0", "TIPO":  "VENDA",...}
response = requests.post(
	"https://sua-api/report_c_connection_bi_1",
	headers={"Authorization":  f"Bearer {token}"},
	json=dados
)
```

_Somativa - 2025_
