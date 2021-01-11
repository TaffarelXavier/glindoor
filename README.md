# Documentação da api do Delivery

Glossário:

- **`Clientes`** - são os clientes que comprarão no sistema.
- **`Parceiros`** - são as lojas cadastradas e aptas para vendas.
- **`RECURSO`** - é qualquer dado pertecente a um determinado outro recurso. Por exemplo, digamos que um registro de vendas da tabela `orders` é um recurso de uma empresa. Por exemplo, os `pedidos` são um recurso de um cliente ou de uma loja.
- **`QUERY`** - são os parâmetros passados na url, que é diferente do params, que são passados diretamente na rota e faz parte dela (rota). Por exemplo:-
  - `params: users/1`
  - `query: users?id=1`.

---

> <div style="font-size:40px; text-align:center"># APIREST</div>

---

> `POST` **/company/upadate-catalogo-url** | JWT - Obrigatório

### Atualiza a url de uma empresa

### Parâmetros

- url_site
  - A nova url
- company_id
  - O id da empresa

### Respostas

- A url já existe

```
{
  "error": true,
  "message": "URL_ALREADY_EXISTS"
}
```

- Não existe uma empresa para o `id` passado

```
{
  "error": true,
  "message": "USER_NOT_FOUND"
}
```

- Em caso de sucesso

```
{
  "error": false,
  "message": "SUCCESS"
}
```

---

`GET` **company/categories-by-company**

### Busca as categorias criadas pela empresa dona do recurso.

### Parâmetros

- `company_id` _`(query)`_
  - O `id` da empresa dona do recurso
- `place` _`(query)`_
  - O lugar onde será mostrada, que pode ser:
    - **`parceiro`**
      - Será mostrado no `aplicativo` na área do parceiro
    - **`dashboard`**
      - Será mostrado no `dashboard`

### Respostas

- **O id da empresa passado não foi encontrado**

```
{
  "error": true,
  "message": "Empresa não encontrada"
}
```

- **Em caso de sucesso**

```
[
  {
    "name": "GRÃOS",
    "id": 1,
    "amount_products": 1
  }
]
```

---

`GET` **/company/categories**

### Mostra todas as categorias, sem exceção, de uma empresa.

### Parâmetros

- company_id
  - O `id` da empresa dona do recurso

### Respostas

- O id passado não existe
  ```
  []
  ```
- Em caso de sucesso

```
[
  {
    "id": 1,
    "domain": "http://localhost:3010",
    "uf_id": 27,
    "city_id": 37655,
    "first_access": 0,
    "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
    "categories": [
      {
        "id": 1,
        "name": "GRÃOS"
      },
      {
        "id": 2,
        "name": "COZINHA"
      },
      {
        "id": 3,
        "name": "PADARIA"
      }
    ]
  }
]
```

---

## `POST` **/alterar-banner**

- JWT - Obrigatório

### Mostra todas as categorias, sem exceção, de uma empresa.

> ### Parâmetros

- company_id
  - O `id` da empresa dona do recurso
- type
- O tipo, que poderá ser
  - logo
  - banner

> ### Respostas

- O arquivo não foi anexado
  ```
  {
    "error": true,
    "message": "FILE_NOT_UPLOADED"
  }
  ```
- O `id` da empresa dona do recurso não existe

  ```
  {
  "error": true,
  "message": "COMPANY_NOT_EXISTS"
  }
  ```

- Em caso de sucesso

  ```
  {
  "error": false,
  "companyImage": {
      "name": "39611b95-038b-4925-b2d1-80e782dfc2b1",
      "extension": "png",
      "type": "banner",
      "company_id": "1",
      "created_at": "2021-01-09 13:24:15",
      "updated_at": "2021-01-09 13:24:15",
      "id": 6
  }
  }
  ```

---

> `GET` **mostrar-banner/:path**

### Mostra o banner da empresa

> ### Parâmetros

- path
  - o nome da imagem completo
    - Detalhe: no banco, é passado o nome da imagem sem sufixo à frente, mas, no servidor, são armazenadas duas **imagens** com os respectivos nomes:
      - imagename_original.extensão (400 pixels ~)
      - imagename_placeholder.extensão (10 pixels)

Ou seja, deve ser passado, no path, o nome completo, ou com original, na frente ou placeholder.
Exemplo: se no banco tiver uma imagem com o nome `39611b95-038b-4925-b2d1-80e782dfc2b1`, então, o `path` deverá ter
**39611b95-038b-4925-b2d1-80e782dfc2b1_original.png** ou
**39611b95-038b-4925-b2d1-80e782dfc2b1_placeholder.png**.

> ### Respostas

- Se o parâmetro não for passado
  ```
  E_ROUTE_NOT_FOUND: Route not found GET /mostrar-banner/
  ```
- Se o parâmetro for passado, mas o recurso não existir no `banco de dados`
  ```
  404: Not Found
  ```
- Em caso de sucesso
  - A imagem na tela

---

> `GET` **/estados**

## Mostra um array de objeto

> ### Respostas

```
[
    {
        "est_id": 1,
        "pais_id": 1,
        "est_nome": "Acre",
        "est_sigla": "AC",
        "created_at": "2019-09-13 11:03:38",
        "updated_at": "2019-09-13 11:04:14"
    },
    {
        "est_id": 2,
        "pais_id": 1,
        "est_nome": "Alagoas",
        "est_sigla": "AL",
        "created_at": "2019-09-13 11:03:38",
        "updated_at": "2019-09-13 11:04:14"
    },
    ...mais
]

```

---

> `GET` **/buscar-cidades-por-estado**

## Mostra cidades por estado

> ### Parâmetros

- `estado_id *`

  - O id do estado

- > ### Respostas
- O id do estado não foi passado
  ```
  {
    "error": true,
    "message": "ESTADO_FIELD_IS_REQUIRED"
  }
  ```

---

> `GET` **/order-status-client/:user_id**

## Descrição

> ### Parâmetros

- `user_id`
  - O `id` do usuário dono do recurso
- > ### Respostas

---

> # `GET` **product/get-product-by-id/:product_id**

- JWT

## Buscar um produto pelo seu id

> ### Parâmetros

- `product_id`

  - O `id` do produto

> ### Respostas

- Parâmetro não foi passado

  ```HttpException
  E_ROUTE_NOT_FOUND: Route not found GET /product/get-product-by-id/
  ```

- Em caso de sucesso

```
{
  "error": false,
  "product": {
    "id": 1,
    "name": "TESTE",
    "price": 23,
    "description": "descrição",
    "active": 1,
    "amount": 100,
    "uuid": "b35072b5-8193-441f-b93e-46a7ec8bc4ea",
    "category_id": 1,
    "company_id": 1,
    "measure_id": 1,
    "updated_at": "2021-01-08 21:57:32",
    "thumbnails": [
      {
        "id": 1,
        "image_name": "52d02334-96d0-4ca0-b813-33585b4ade39",
        "image_extension": null,
        "image_ext": "png",
        "isthumbinal": 1,
        "product_id": 1
      }
    ],
    "measure": {
      "id": 1,
      "measure": "un",
      "description": null,
      "company_id": 1
    },
    "category": {
      "id": 1,
      "name": "GRÃOS",
      "company_id": 1
    }
  }
}
```

- O `id` do produto não existe
  ```
  []
  ```

---

> `POST` **/product**

- **JWT é obrigatório**

## Cria um produto

> ### Parâmetros

Deve ser passado um `json`:

```
{
    "name":"",
    "active":"",
    "price":"",
    "amount":"",
    "description":"",
    "category_id":"",
    "company_id":"",
    "measure_id":"",
    "uuid":""
}
```

- `name` (String)
  - O nome do produto
- `active` (boolean)
  - Definirá se o produto estará visível ou não área do parceiro
- `price` (float)
  - O preço do produto
- `amount` (int)
  - A quantidade do produto
- `description` (string)
  - A descrição do produto
- `category_id` (int)
  - A categoria à qual esse produto pertence
- `company_id` (int)
  - O `id` da empresa dona do recurso
- `measure_id` (int)
  - A medida do produto
- `uuid` (string) - Deve ser gerado no cliente, pois o mesmo será passado tanto para o firebase quanto para o banco, esse se responsabilizará pelo relacionamento de ambos.

> ### Respostas

- Em caso de sucesso

  ```
  {
    "product": {
        "name": "ARROZ INTEGRAL",
        "price": 10.1,
        "amount": 20,
        "company_id": 1,
        "category_id": 1,
        "description": "Descrição",
        "measure_id": 1,
        "uuid": null,
        "active": 0,
        "created_at": "2021-01-09 18:14:02",
        "updated_at": "2021-01-09 18:14:02",
        "id": 3
    },
    "stock": {
        "quantidade": 20,
        "preco": 10.1,
        "product_id": 3,
        "created_at": "2021-01-09 18:14:02",
        "updated_at": "2021-01-09 18:14:02",
        "id": 2
    }
  }
  ```

---

> `PUT` **/product**

- **JWT é obrigatório**

## Atualiza um produto

> ### Parâmetros

Deve ser passado um `json`:

```
{
	"id":1,
	"name": "Arroz Integral",
	"active": "1",
	"price": 10.10,
	"amount": 20,
	"description": "Descrição",
	"category": {
		"value": 1
	},
	"company_id": 1,
	"measure": {
		"id": 1
	},
	"stock": {
		"quantidade": 10,
		"preco": 15.8
	}
}
```

- `id` (int)
  - O id do produto
- `name` (String)
  - O nome do produto
- `active` (boolean)
  - Definirá se o produto estará visível ou não área do parceiro
- `price` (float)
  - O preço do produto
- `amount` (int)
  - A quantidade do produto
- `description` (string)
  - A descrição do produto
- `category` (objeto)
  - A categoria à qual esse produto pertence
    - Deverá passar obrigatoriamente esta chave cujo valor deverá ser um `inteiro`:
      - value
- `company_id` (int)
  - O `id` da empresa dona do recurso
- `measure` (objeto)
  - A medida do produto
    - deve ser passado obrigatoriamente, estes campos:
      - id
- `stock` (objeto) - Deve ser gerado no cliente. O objeto deverá ter, obrigatoriamente, estes campos:
  - quantidade
  - preco

> ### Respostas

- **O produto não existe**

```
{
  "error": true,
  "message": "PRODUCT_NOT_EXISTS"
}
```

- **Em caso de sucesso**

```
{
  "id": 1,
  "name": "ARROZ INTEGRAL",
  "price": 10.1,
  "description": "Descrição",
  "active": "1",
  "amount": 20,
  "uuid": "b35072b5-8193-441f-b93e-46a7ec8bc4ea",
  "category_id": 1,
  "company_id": 1,
  "measure_id": 1,
  "created_at": "2021-01-07 21:09:58",
  "updated_at": "2021-01-09 18:40:45"
}
```

---

> `POST` **/product/delete-image**

- **JWT é obrigatório**

## Excluir uma imagem de um produto

> ### Parâmetros

- **`image_id`**
  - o `id` da imagem
- **`company_id`**

  - O `id` da empresa dona do recurso

> ### Respostas

- A image não existe

```
{
  "error": true,
  "message": "NOT_POSSIBLE_DELETE_IMAGE"
}
```

- Em caso de sucesso

```
{
  "error": false,
  "message": "SUCCESS",
  "data": 1
}
```

- Não foi passado um **`id`** válido de uma empresa.

```
{
  "error": true,
  "message": "USER_NOT_LOGGED"
}
```

---

> `GET` **/products-by-category-id**

## Buscar produtos por uma categoria específica

> ### Parâmetros

- **`category_id`**
  - O `id` da categoria
- **`company_id`**
  - O `id` da empresa

> ### Respostas

- Em caso de sucesso

```
[
  {
    "id": 1,
    "name": "ARROZ INTEGRAL",
    "price": 10.1,
    "description": "Descrição",
    "active": 1,
    "amount": 20,
    "uuid": "b35072b5-8193-441f-b93e-46a7ec8bc4ea",
    "category_id": 1,
    "company_id": 1,
    "measure_id": 1,
    "created_at": "2021-01-07 21:09:58",
    "updated_at": "2021-01-09 18:40:45",
    "stock": [
      {
        "id": 1,
        "quantidade": 10,
        "preco": 15.8,
        "quant_vendidos": null,
        "prazo_validade": null,
        "product_id": 1
      }
    ],
    "thumbnails": [],
    "measure": {
      "id": 1,
      "measure": "un",
      "description": null,
      "company_id": 1
    }
  },
  {
    "id": 3,
    "name": "ARROZ INTEGRAL",
    "price": 10.1,
    "description": "Descrição",
    "active": 0,
    "amount": 20,
    "uuid": null,
    "category_id": 1,
    "company_id": 1,
    "measure_id": 1,
    "created_at": "2021-01-09 18:14:02",
    "updated_at": "2021-01-09 18:14:02",
    "stock": [
      {
        "id": 2,
        "quantidade": 20,
        "preco": 10.1,
        "quant_vendidos": null,
        "prazo_validade": null,
        "product_id": 3
      }
    ],
    "thumbnails": [],
    "measure": {
      "id": 1,
      "measure": "un",
      "description": null,
      "company_id": 1
    }
  }
]
```

- A categoria não existe

```
[]
```

- A empresa não existe

```
[]
```

---

> `GET` **/product/detalhe**

## Mostra o produto e seus detalhes, como

- Estoque
- medida
- empresa
- Entre outros

> ### Parâmetros

- `company_url` (string)
  - a url (slug) da empresa
- `product_id` (int)

  - O id do produto

> ### Respostas

- Se pelo menos um dos parâmetros não forem passados

```
{
  "error": true,
  "message": "PARAMETERS_NOT_PASS"
}
```

- A url de empresa não existe

```
{
  "error": true,
  "message": "PRODUCT_NOT_FOUND_3"
}
```

- O produto existe, mas não está relacionado à empresa passada.

```
{
  "error": true,
  "message": "PRODUCT_NOT_FOUND_3"
}
```

- O produto **não** existe, e a empresa existe ou não.

```
{
  "error": true,
  "message": "PRODUCT_NOT_FOUND_1"
}
```

- Em caso de sucesso

```
{
  "id": 1,
  "name": "ARROZ INTEGRAL",
  "price": 10.1,
  "description": "Descrição",
  "active": 1,
  "amount": 100,
  "uuid": "b35072b5-8193-441f-b93e-46a7ec8bc4ea",
  "category_id": 1,
  "measure_id": 1,
  "updated_at": "2021-01-10 23:21:50",
  "stock": [
    {
      "id": 1,
      "quantidade": 100,
      "preco": 15.8,
      "quant_vendidos": null,
      "prazo_validade": null,
      "product_id": 1
    }
  ],
  "thumbnails": [
    {
      "id": 3,
      "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
      "image_extension": null,
      "image_ext": "png",
      "isthumbinal": 0
    },
    {
      "id": 4,
      "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
      "image_extension": null,
      "image_ext": "png",
      "isthumbinal": 0
    },
    {
      "id": 5,
      "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
      "image_extension": null,
      "image_ext": "png",
      "isthumbinal": 0
    },
    {
      "id": 6,
      "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
      "image_extension": null,
      "image_ext": "png",
      "isthumbinal": 0
    },
    {
      "id": 7,
      "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
      "image_extension": null,
      "image_ext": "png",
      "isthumbinal": 0
    }
  ],
  "measure": {
    "id": 1,
    "measure": "un",
    "description": null,
    "company_id": 1
  },
  "company": {
    "id": 1,
    "name": "Glindoor 1",
    "domain": "http://localhost:3010",
    "about": "Sobre",
    "address": "Endereço",
    "phone": "6399480630",
    "uf_id": 27,
    "city_id": 37655,
    "first_access": 0,
    "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
    "url_si
```

---

> `POST` **/product/upload/:product_id/:isthumbnail**

## Faz uploads de imagens para um produto específico

A estrutura do formulário deve ser to tipo **`multipart`** para o parâmetro **`image[]`**.

> ### Parâmetros

- `product_id`
  - O `id` do produto
- `isthumbnail`
  - É o nome da imagem que deverá ser a principal imagem do produto
    - Por exemplo:
      ```
      4b839d81-97e6-4b14-a702-f4dce1c4400d.png
      ```
- `image[]`
  - Os arquivos (Falta concluir essa parte)

> ### Respostas

- Em caso de sucesso

```
[
  {
    "image_name": "arroz-tipo-1-pacote-5kg-broto-legal-pct.png",
    "image_ext": "png",
    "product_id": 1,
    "isthumbinal": 0,
    "created_at": "2021-01-09 22:16:34",
    "updated_at": "2021-01-09 22:16:34",
    "id": 3
  }
]
```

---

> `POST` **/login**

## Inicia a sessão do cliente (não é o parceiro)

> ### Parâmetros

- email
- password

> ### Respostas

- **Se pelo menos um parâmetro não for passado**

```
{
  "error": true,
  "message": "PARAMETERS_NOT_PASS"
}
```

- **O usuário não existe**

```
{
  "error": true,
  "message": "UserNotFoundException: E_USER_NOT_FOUND: Cannot find user with email as [EMAIL PASSADO]"
}
```

- **A senha está incorreta**

```
{
  "error": true,
  "message": "PasswordMisMatchException: E_PASSWORD_MISMATCH: Cannot verify user password"
}
```

- **Em caso de sucesso**

```
{
  "success": true,
  "token": {
    "type": "bearer",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjEsImlhdCI6MTYxMDI0NTQ0NiwiZXhwIjoxNjEwODUwMjQ2fQ.jBNSqlv8nrg23YrD9h1gIBtrzQ3Dq1SdYFacXa0iZKQ",
    "refreshToken": "f6796cd70e92314f9063b27998d1c470m4XOT5Sw8kzzWE0MyuDP8rhdQ0ETHyvk30GNjbnS5ThW/AnXOmCYjgsUzd8wkt07"
  },
  "user": {
    "id": 1
  }
}
```

---

> `POST` **/users**

## Cadastra um novo cliente (o comprador, não o parceiro)

> ### Parâmetros

- firstname
  - Nome completo
- email
  - Email (obrigatório)
- password
  - Senha (obrigatório)
- avatar
  - imagem principal do cliente
- provider_signup
  - O tipo de cadastro, poder ser:
    - email
    - facebook
    - google
    - github
    - twitter

**Exemplo**

```
{
"firstname":"João da Silva Andrade",
"email":"emailexample@gmail.com",
"password":"chkdsk",
"avatar":"http://placehold.it/32x32",
"provider_signup":"email"
}
```

> ### Respostas

- Em caso de sucesso

```
{
  "token": {
    "type": "bearer",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjEsImlhdCI6MTYxMDI0Mjc4MCwiZXhwIjoxNjEwODQ3NTgwfQ.Lg7IRiUwRq1VLdopnRO9rTE3R7Wr2oBjhSCA26b3pw4",
    "refreshToken": null
  },
  "user": {
    "id": 1
  }
}
```

---

> `POST` **Ativar um cliente**

- JWT

## Ativa um novo cliente

> ### Parâmetros

- > ### Respostas

---

> `POST` **/recover_password**

## Envia um token de segurança para o email passado.

> ### Parâmetros

- email

> ### Respostas

- **O email está em branco**

  ```
  {
    "error": true,
    "message": "EMAIL_IS_EMPTY"
  }
  ```

- **Em caso de sucesso**

  ```
  {
  "error": false,
  "message": "SUCCESS",
  "recoverPassword": {
      "ischecked": 0,
      "token": "5c3b48b0-fa59-401d-a226-29dc1d1e8cf9",
      "user_id": 1,
      "created_at": "2021-01-09 22:53:50",
      "updated_at": "2021-01-09 22:53:50",
      "id": 1
  }
  }
  ```

  Após o envio, o cliente deverá acessar seu email para poder gerar a nova senha.

---

> `POST` **/verify_token**

## Verifica o token gerado na rota /recover_password

> ### Parâmetros

- token _`(query)`_

  - É o token gerado na rota (/recover_password) no objeto `recoverPassword.token`.

> ### Respostas

- **Se nenhum parâmetro for passado**

```
{
  "error": true,
  "message": "TOKEN_NOT_VALID"
}
```

- **O token é válido**

```
{
  "error": false,
  "message": "TOKEN_IS_VALID",
  "status": "SUCCESS"
}
```

- **O token não é válido**

```
{
  "error": true,
  "message": "TOKEN_NOT_FOUND"
}
```

---

> `POST` **/change_password**

- JWT

## De fato, agora, o usuário, depois de ter passado nos passos das rotas `/recover_password` e `verify_token`, ele poderá alterar a senha.

> ### Parâmetros

- **`token`**
  - O token gerado em `/recover_password`
- **`password`**
  - A nova senha
- **`repeat_password`**
  - A repetição da nova senha

```
{
  "token":"5c3b48b0-fa59-401d-a226-29dc1d1e8cf9",
  "password":"123456",
  "repeat_password":"123456"
}
```

> ### Respostas

- **O `password` é diferente da `repeat_senha`**

```
{
"error": true,
"message": "PASSWORDS_NOT_EQUALS"
}
```

- **O token não foi passado**

```
{
"error": true,
"message": "TOKEN_NOT_VALID"
}
```

- **O token não é valido**

```
{
  "error": true,
  "message": "TOKEN_NOT_FOUND"
}
```

- **Em caso de sucesso**

```
{
"error": false,
"message": "PASSWORD_UPDATED"
}
```

---

> `PUT` **/user**

- **JWT é obrigatório**

## Atualiza os dados de um cliente específico

> ### Parâmetros

- **firstname**
  - O primeiro nome
- **lastname**
  - O último nome
- **phone**
  - o telefone
- **address**
  - O endereço
- **number**
  - o número da casa
- **district**
  - o bairro
- **id**
  - O `id` do cliente
- **uf_id**
  - O `id` do estado
- **city_id**
  - O `id` da cidade

**Exemplo**

```
{
	"firstname": "",
	"lastname": "",
	"phone": "",
	"address": "",
	"number": "",
	"district": "",
	"id": "",
	"city_id": "",
	"uf_id": ""
}
```

> ### Respostas

```
{
"error": true,
"message": "FIRSTNAME_NOT_IS_VALID"
}
```

-

```
{
"error": true,
"message": "LASTNAME_NOT_IS_VALID"
}
```

- Se o número não for passado ou for menor que 11 caracteres

```
{
    "error": true,
    "message": "PHONE_NOT_IS_VALID"
}
```

-

```
{
  "error": true,
  "message": "ADDRESS_NOT_IS_VALID"
}
```

- O id do usuário não foi passado corretamente.

```
{
  "error": true,
  "message": "REQUEST_NOT_VALID_USER"
}
```

- Se nenhum dado foi atualizado

```
{
  "error": true,
  "message": "Nenhum dado foi atualizado"
}
```

- Em caso de sucesso, ou de algum dado for alterado.

```
{
  "error": false,
  "message": [
    {
      "id": 1,
      "firstname": "Taffarel",
      "lastname": "  Xavier",
      "ischecked": 1,
      "phone": "63999480631",
      "avatar": "http://placehold.it/32x32",
      "district": "asdfa",
      "email": "taffarelxavier7@gmail.com",
      "address": "Rua Manoel Matos",
      "city_id": 321,
      "uf_id": 27,
      "number": "145",
      "estado": {
        "est_id": 27,
        "pais_id": 1,
        "est_nome": "Tocantins",
        "est_sigla": "TO"
      },
      "cidade": {
        "cid_id": 321,
        "cid_nome": "São José Do Jacuípe"
      }
    }
  ]
}
```

---

> `GET` **/users**

JWT é obrigatório

## Lista todos os clientes

> ## Params

> ## Responses

- Em caso de sucesso
  ```
  {
  "error": false,
  "message": [
    {
      "id": 1,
      "firstname": "Taffarel",
      "lastname": "  Xavier",
      "ischecked": 1,
      "token": "2179b938-65d3-4ac0-aa02-92e6c3cc4d66",
      "phone": "63999480631",
      "avatar": "http://placehold.it/32x32",
      "district": "asdfa",
      "provider_signup": "email",
      "email": "taffarelxavier7@gmail.com",
      "address": "Rua Manoel Matos",
      "password": "$2a$10$ogTVhWGefZMi1rDct9fc..nnOnS9xyNcWl/UE8xWJ8o4ttc7eXPqa",
      "created_at": "2021-01-09 22:39:40",
      "updated_at": "2021-01-09 23:54:36",
      "city_id": 321,
      "uf_id": 27,
      "number": "145"
    }
  ]
  }
  ```

---

> `GET` **/get-user**

JWT é obrigatório

## Busca o usuário pelo `id` dele.

> ## Params

- `user_id` _(query)_
  - O ID do usuário

> ## Responses

- Em caso de sucesso

```
[
  {
    "id": 1,
    "firstname": "Taffarel",
    "lastname": "  Xavier",
    "ischecked": 1,
    "phone": "63999480631",
    "avatar": "http://placehold.it/32x32",
    "district": "asdfa",
    "email": "taffarelxavier7@gmail.com",
    "address": "Rua Manoel Matos",
    "city_id": 321,
    "uf_id": 27,
    "number": "145",
    "estado": {
      "est_id": 27,
      "pais_id": 1,
      "est_nome": "Tocantins",
      "est_sigla": "TO"
    },
    "cidade": {
      "cid_id": 321,
      "cid_nome": "São José Do Jacuípe"
    }
  }
]
```

- Algum parâmetro não foi passado:

```
{
 error: true,
 message: "ANY_PARAMETERS_NOT_PASS"
}
```

---

> `PUT` **/user/update_password**

**JWT é obrigatório**

## Altera a senha do cliente

**Exemplo:**

```
{
"password":"123456",
"confirm_password":"123456",
"actual_password":"8237459",
"id":1
}
```

> ## Params

- `password`
  - A nova senha
- `confirm_password`
  - Confirmação da nova senha
- `actual_password`
  - A senha atual
- `id`
  - O ID do cliente

> ## Responses

- Em caso de sucesso

```
{
  "error": false,
  "message": "UPDATED_SUCCESSFUL",
  "token": {
    "type": "bearer",
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjEsImlhdCI6MTYxMDI4NjYyNCwiZXhwIjoxNjEwODkxNDI0fQ.Sp0jzzX9hEkfRk7LdS85aasa3VPeNOzhj0v-2K9lPOQ",
    "refreshToken": null
  }
}
```

- A senha foi passada em branco ou o total de caracteres é igual a zero:

```
{
"error": true,
"message": "PASSWORD_NOT_IS_VALID"
}
```

- A nova senha, a confirmação da nova senha e a atual são iguais:

```
{
"error": true,
"message": "NEW_PASSWORD_EQUALS_ACTUAL_PASSWORD"
}
```

---

> `POST` **/update-token-client**

**JWT é obrigatório**

## Atualiza o token de atualização

> ## Params

- `refreshToken`
  - O token que é gerado na requisição válida no **`login`**.

> ## Responses

- **Em caso de sucesso**

```
{
  "type": "bearer",
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1aWQiOjEsImRhdGEiOnsiaWQiOjEsImZpcnN0bmFtZSI6IlRhZmZhcmVsIiwibGFzdG5hbWUiOiIgIFhhdmllciIsImlzY2hlY2tlZCI6MSwidG9rZW4iOiIyMTc5YjkzOC02NWQzLTRhYzAtYWEwMi05MmU2YzNjYzRkNjYiLCJwaG9uZSI6IjYzOTk5NDgwNjMxIiwiYXZhdGFyIjoiaHR0cDovL3BsYWNlaG9sZC5pdC8zMngzMiIsImRpc3RyaWN0IjoiYXNkZmEiLCJwcm92aWRlcl9zaWdudXAiOiJlbWFpbCIsImVtYWlsIjoidGFmZmFyZWx4YXZpZXI3QGdtYWlsLmNvbSIsImFkZHJlc3MiOiJSdWEgTWFub2VsIE1hdG9zIiwiY3JlYXRlZF9hdCI6IjIwMjEtMDEtMDkgMjI6Mzk6NDAiLCJ1cGRhdGVkX2F0IjoiMjAyMS0wMS0xMCAxMDo1MDoyNCIsImNpdHlfaWQiOjMyMSwidWZfaWQiOjI3LCJudW1iZXIiOiIxNDUifSwiaWF0IjoxNjEwMjkwMTI2LCJleHAiOjE2MTA4OTQ5MjZ9.vXxAMp9KdA9Qcd2kIrsYITL6uk0DO5wEfbgfgYRqq-w",
  "refreshToken": "e4ea2387aab263b916dcdc7f3126e5e0sJ6/mJZZhukuVRkZcACdgtgU8lCrK10/NsXYE1sODUQTL07wpDDA2EQ/bnoLOEI6"
}
```

- **Não foi passado o token**

```
{
  "error": true,
  "message": "InvalidRefreshToken: E_INVALID_JWT_REFRESH_TOKEN: Invalid refresh token undefined"
}
```

---

> `POST` **order**

**JWT é obrigatório**

## Cria um novo pedido

> ## Examples

```
{
	"uuid": "uuidexample",
	"company_id": 1,
	"form_payment": "1",
	"payback": "1",
	"notes": "Notas",
	"user_id": 1,
	"produtos": [
		{
			"units": 10,
			"price": 4.50,
			"puid": "puidexame",
			"id": 1,
			"name": "Arroz",
			"measure":"String Nome"
		}
	],
	"city_id": 3135,
	"uf_id": 27,
	"address": "Rua Manoel Matos",
	"number": "45",
	"district": "Centro",
	"phone": "6399480630",
	"rate": 10
}
```

> ## Params

- uuid
  - uuid gerado no cliente
- company_id
  - id da empresa onde se está comprando
- form_payment
  - Forma de pagamento
- payback
  - Troco
- notes
  - Obervações
- user_id
  - Id do cliente
- produtos
  - Array de objetos contendo:
    - units
    - price
    - puid
    - id
    - name
    - measure
- city_id
  - Id da cidade
- uf_id
  - ID do estado
- address
  - Endereço
- number
  - Número da casa para entrega
- district
  - Bairro para entrega
- phone
  - Telefone de quem está pedindo
- rate
  - Taxa

> ## Responses

- **Em caso de sucesso**

```
{
	"uuid": "uuidexample",
	"company_id": 1,
	"form_payment": "1",
	"payback": "1",
	"notes": "Notas",
	"user_id": 1,
	"produtos": [
		{
			"units": 10,
			"price": 4.50,
			"puid": "puidexame",
			"id": 1,
			"name": "Arroz",
			"measure":"String Nome"
		}
	],
	"city_id": 3135,
	"uf_id": 27,
	"address": "Rua Manoel Matos",
	"number": "45",
	"district": "Centro",
	"phone": "6399480630",
	"rate": 10
}
```

- **Se o array `produtos` não existir**

```
{
  "error": true,
  "message": "E_PRODUCTS_IS_EMPTY"
}
```

- **Se faltar algum dos campos acima**

```
{
  "error": true,
  "message": "FIELDS_ADDRESS_CITYID_DISTRICT_PHONE_UFID_ARE_REQUIRED"
}
```

- **Foi passado um uuid que já existe**

```
{
  "error": true,
  "message": "Duplicate entry 'uuidexample_que_ja_existe' for key 'orders_uuid_unique'"
}
```

- **O estoque está vazio**

```
{
  "error": true,
  "message": "NOT_POSSIBLE_TO_SAVE_ORDER_STOCK_IS_EMPTY"
}
```

---

> `GET` **/order**

**JWT é obrigatório**

## Busca pedidos para empresa

> ## Examples

```
http://localhost:3333/order?uuid=uuidexample
```

> ## Params

- uuid _(query)_
  - É gerado no ato de criar um novo pedido, está na tabela `order`

> ## Responses

- **Em caso de sucesso**

```
[
  {
    "id": 1,
    "uuid": "uuidexample",
    "notes": "Notas",
    "payback": 1,
    "form_payment": "1",
    "status": "novo",
    "company_id": 1,
    "user_id": 1,
    "created_at": "2021-01-10 15:38:55",
    "updated_at": "2021-01-10 15:38:55",
    "address": "Rua Manoel Matos",
    "number": "45",
    "district": "Centro",
    "rate": "10",
    "city_id": 3135,
    "uf_id": 27,
    "phone": "6399480630",
    "user": {
      "id": 1,
      "firstname": "Taffarel",
      "lastname": "  Xavier",
      "ischecked": 1,
      "token": "2179b938-65d3-4ac0-aa02-92e6c3cc4d66",
      "phone": "63999480631",
      "avatar": "http://placehold.it/32x32",
      "district": "asdfa",
      "provider_signup": "email",
      "email": "taffarelxavier7@gmail.com",
      "address": "Rua Manoel Matos",
      "city_id": 321,
      "uf_id": 27,
      "number": "145"
    },
    "estado": {
      "pais_id": 1,
      "est_sigla": "TO",
      "created_at": "2019-09-13 11:03:40",
      "updated_at": "2019-09-13 11:04:14"
    },
    "cidade": {
      "cid_nome": "Augustinópolis",
      "created_at": "2019-09-13 11:07:48",
      "updated_at": "2019-09-13 11:04:14"
    },
    "pedidos": [],
    "status_clients": [
      {
        "id": 1,
        "status_name": "novo",
        "active": 1,
        "created_at": "2021-01-10 15:38:55",
        "updated_at": "2021-01-10 15:38:55"
      }
    ],
    "company": {
      "id": 1,
      "name": "Glindoor 1",
      "domain": "http://localhost:3010",
      "about": "Sobre",
      "address": "Endereço",
      "phone": "6399480630",
      "uf_id": 27,
      "city_id": 37655,
      "first_access": 0,
      "email": "taffarelxavier7@gmail.com",
      "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
      "url_site": "glindoor11",
      "images": [
        {
          "id": 3,
          "name": "banner_default_company",
          "extension": "jpg",
          "type": "banner",
          "active": 1,
          "company_id": 1,
          "created_at": "2021-01-09 00:17:51",
          "updated_at": "2021-01-09 00:17:51"
        },
        {
          "id": 4,
          "name": "b00a3871-d800-4fdf-ba92-6e2ff9ca7e55",
          "extension": "jpeg",
          "type": "logo",
          "active": 1,
          "company_id": 1,
          "created_at": "2021-01-09 12:25:21",
          "updated_at": "2021-01-09 12:25:21"
        },
        {
          "id": 6,
          "name": "39611b95-038b-4925-b2d1-80e782dfc2b1",
          "extension": "png",
          "type": "banner",
          "active": 1,
          "company_id": 1,
          "created_at": "2021-01-09 13:24:15",
          "updated_at": "2021-01-09 13:24:15"
        }
      ],
      "estado": {
        "est_id": 27,
        "pais_id": 1,
        "est_nome": "Tocantins",
        "est_sigla": "TO"
      },
      "cidade": null
    }
  }
]
```

- **Se a `query` `uuid` não for passada**

```
{
  "error": true,
  "message": "UUID_NOT_PASS"
}
```

- **Se o `uuid` não existir ou estiver incorreto**

```
{
  "error": true,
  "message": "ORDER_NOT_EXISTS"
}
```

---

> `GET` **/order-status-client/:user_id**

## Busca os pedidos para um cliente para

> ## Examples

```
http://localhost:3333/order-client/3
```

> ## Params

- id
  - O `ID` do cliente

> ## Responses

- **Em caso de sucesso**

```
[
  {
    "id": 1,
    "uuid": "uuidexample",
    "notes": "Notas",
    "payback": 1,
    "form_payment": "1",
    "status": "novo",
    "company_id": 1,
    "user_id": 1,
    "created_at": "2021-01-10 15:38:55",
    "updated_at": "2021-01-10 15:38:55",
    "address": "Rua Manoel Matos",
    "number": "45",
    "district": "Centro",
    "rate": "10",
    "city_id": 3135,
    "uf_id": 27,
    "phone": "6399480630",
    "company": {
      "id": 1,
      "name": "Glindoor 1",
      "domain": "http://localhost:3010",
      "about": "Sobre",
      "address": "Endereço",
      "phone": "6399480630",
      "uf_id": 27,
      "city_id": 37655,
      "first_access": 0,
      "email": "taffarelxavier7@gmail.com",
      "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
      "url_site": "glindoor11",
      "ordersWithoutClient": [
        {
          "id": 1,
          "uuid": "uuidexample",
          "notes": "Notas",
          "payback": 1,
          "form_payment": "1",
          "status": "novo",
          "company_id": 1,
          "user_id": 1,
          "created_at": "2021-01-10 15:38:55",
          "updated_at": "2021-01-10 15:38:55",
          "address": "Rua Manoel Matos",
          "number": "45",
          "district": "Centro",
          "rate": "10",
          "city_id": 3135,
          "uf_id": 27,
          "phone": "6399480630",
          "pedidos": [],
          "status_clients": [
            {
              "id": 1,
              "status_name": "novo",
              "active": 1,
              "created_at": "2021-01-10 15:38:55",
              "updated_at": "2021-01-10 15:38:55"
            }
          ]
        }
      ]
    },
    "status_clients": [
      {
        "id": 1,
        "status_name": "novo",
        "active": 1,
        "created_at": "2021-01-10 15:38:55",
        "updated_at": "2021-01-10 15:38:55"
      }
    ]
  }
]
```

- **Não foi passado o parâmetro `id` na rota**

```
HttpException
E_ROUTE_NOT_FOUND: Route not found GET /order-status-client/
```

---

> `GET` **/pedido-para-empresa**

**JWT é obrigatório**

## Mostra os **pedidos** da empresa especificada pelo `id` dela.

> ## Examples

```
http://localhost:3333/pedido-para-empresa?company_id=1
```

> ## Params

- `company_id`
  - O id da empresa

> ## Responses

- **Em caso de sucesso**

```
[
  {
    "id": 1,
    "uuid": "uuidexample",
    "payback": 1,
    "form_payment": "1",
    "company_id": 1,
    "user_id": 1,
    "created_at": "2021-01-10 15:38:55",
    "updated_at": "2021-01-10 15:38:55",
    "address": "Rua Manoel Matos",
    "number": "45",
    "district": "Centro",
    "rate": "10",
    "city_id": 3135,
    "uf_id": 27,
    "phone": "6399480630",
    "user": {
      "firstname": "Taffarel",
      "lastname": "  Xavier",
      "phone": "63999480631",
      "avatar": "http://placehold.it/32x32",
      "provider_signup": "email",
      "address": "Rua Manoel Matos",
      "number": "145"
    },
    "company": {
      "id": 1,
      "uf_id": 27,
      "city_id": 37655,
      "created_at": "2021-01-07 21:08:48",
      "updated_at": "2021-01-09 12:44:10"
    },
    "status_clients": [
      {
        "id": 1,
        "status_name": "novo",
        "active": 1,
        "created_at": "2021-01-10 15:38:55"
      }
    ]
  },
  {
    "id": 3,
    "uuid": "uuidexample22",
    "payback": 1,
    "form_payment": "1",
    "company_id": 1,
    "user_id": 3,
    "created_at": "2021-01-10 23:13:37",
    "updated_at": "2021-01-10 23:13:37",
    "address": "Rua Manoel Matos",
    "number": "45",
    "district": "Centro",
    "rate": "10",
    "city_id": 3135,
    "uf_id": 27,
    "phone": "6399480630",
    "user": {
      "firstname": "Taffarel",
      "lastname": null,
      "phone": "6399480630",
      "avatar": "http://placehold.it/32x32",
      "provider_signup": "email",
      "address": "Rua Manoel Matos",
      "number": "45"
    },
    "company": {
      "id": 1,
      "uf_id": 27,
      "city_id": 37655,
      "created_at": "2021-01-07 21:08:48",
      "updated_at": "2021-01-09 12:44:10"
    },
    "status_clients": [
      {
        "id": 2,
        "status_name": "novo",
        "active": 1,
        "created_at": "2021-01-10 23:13:37"
      }
    ]
  }
]
```

- **O parâmetro não é um inteiro**

```
{
  "error": true,
  "message": "NOT_INTEGER"
}
```

- **O id passado não pertence a uma empresa**

```
{
  "error": true,
  "message": "DADO_NAO_ENCONTRADO"
}
```

- **A empresa existe, mas não tem pedidos**

```
{
  "error": true,
  "message": "DADO_NAO_ENCONTRADO"
}
```

- **O parâmetro não foi passado**

```
{
  "error": true,
  "message": "ID_NOT_PASSED"
}
```

---

> `POST` **orders/obter-soma-total-por-forma-de-pagamento**

**JWT é obrigatório**

## Obter a soma total por forma de pagamento especificada pelo ID

> ## Examples

```
{
 "company_id":2
}
```

> ## Params

- company*id \* *(query)\_
  - O ID da empresa

> ## Responses

- **Em caso de sucesso**

```

```

- **Não tem pedidos pagos**

```
{
  "error": false,
  "message": []
}
```

---

> `GET` **/product-by-company**

## FIXME: Por algum motivo, o nome da rota está `product-by-company`, mas mostra todos os produtos cadastrados. =) kkkkkkk

> ## Examples

```
/product-by-company
```

> ## Params
>
> nothing

> ## Responses

- **Em caso de sucesso**

```
[
  {
    "id": 1,
    "name": "ARROZ INTEGRAL",
    "price": 10.1,
    "description": "Descrição",
    "active": 1,
    "amount": 100,
    "uuid": "b35072b5-8193-441f-b93e-46a7ec8bc4ea",
    "category_id": 1,
    "company_id": 1,
    "measure_id": 1,
    "created_at": "2021-01-07 21:09:58",
    "updated_at": "2021-01-10 23:21:50",
    "stock": [
      {
        "id": 1,
        "quantidade": 100,
        "preco": 15.8,
        "quant_vendidos": null,
        "prazo_validade": null,
        "product_id": 1
      }
    ],
    "company": {
      "id": 1,
      "name": "Glindoor 1",
      "domain": "http://localhost:3010",
      "about": "Sobre",
      "address": "Endereço",
      "phone": "6399480630",
      "uf_id": 27,
      "city_id": 37655,
      "first_access": 0,
      "email": "taffarelxavier7@gmail.com",
      "password": "$2a$10$m40milQGqXkAs85tEKBAqe86Mm0hXskoyd.wbCl9RzUoWNVWtTPJK",
      "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
      "url_site": "glindoor11"
    }
  },
  {
    "id": 3,
    "name": "ARROZ INTEGRAL",
    "price": 10.1,
    "description": "Descrição",
    "active": 0,
    "amount": 20,
    "uuid": null,
    "category_id": 1,
    "company_id": 1,
    "measure_id": 1,
    "created_at": "2021-01-09 18:14:02",
    "updated_at": "2021-01-09 18:14:02",
    "stock": [
      {
        "id": 2,
        "quantidade": 20,
        "preco": 10.1,
        "quant_vendidos": null,
        "prazo_validade": null,
        "product_id": 3
      }
    ],
    "company": {
      "id": 1,
      "name": "Glindoor 1",
      "domain": "http://localhost:3010",
      "about": "Sobre",
      "address": "Endereço",
      "phone": "6399480630",
      "uf_id": 27,
      "city_id": 37655,
      "first_access": 0,
      "email": "taffarelxavier7@gmail.com",
      "password": "$2a$10$m40milQGqXkAs85tEKBAqe86Mm0hXskoyd.wbCl9RzUoWNVWtTPJK",
      "company_uuid": "abe707f3-ee98-49b3-9fbf-fa28cb604c7a",
      "url_site": "glindoor11"
    }
  }
]
```

---

> `POST` **/active_user**

## Quando um cliente faz o seu cadastro, por padrão, a coluna `ischecked` tem o valor padrão igual a zero, esta rota, ativa o usuário atualizando a coluna para o valor `1`.

> ## Examples

```
{
	"token":"2179b938-65d3-4ac0-aa02-92e6c3cc4d66",
	"type":"1"
}
```

> ## Params

- `token`
  - é o toquem gerado e salvo no registro do usuário, na coluna.
- `type`
  - 1

> ## Responses

- **Em caso de sucesso**

```
Se o usuário não estiver ativo, retornará: USER_ACTIVE; caso contrário, USER_IS_ALREADY_ACTIVE.
```

- **Alguma parâmetro não foi passado**

```
{
  "error": true,
  "message": "QUERIES_NOT_PASSED"
}
```

- **Se o token for inválido ou não existir.**

```
TOKEN_INVALID
```

---

Fim
