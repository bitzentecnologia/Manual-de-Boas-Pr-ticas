Criando uma Api Rest
=======================================================


Criação dos endpoints
-------------

O nome dos endpoints deve ser um substantivo e representar uma **Entidade** dentro da aplicação.
  
Ex: `api/products`

A Definição da ação(verbo) que é feita no endpoint já é representada pelo método HTTP, por exemplo, o método **GET** já indica que se está sendo retornado algo. Segue um exemplo do jeito certo e errado de criar o endpoint: 

- **Errado** `/cities/get-by-state/{stateID}`
- **Certo** `/states/{stateId}/cities`

Os métodos HTTP devem ser respeitados obedecendo-se então as suas funções:

  - **GET** deve trazer dados
  - **POST** deve criar dados
  - **PUT** deve atualizar dados existentes
  - **DELETE** deve excluir dados

Formato da Requisição/Retornos
-------------
Todas as requisições e retornos devem estar no formato JSON.

O Modelo padrão para retornos da api tanto para sucesso quanto para erro deve ser o seguinte:

```json
{
  "status": 0,
  "message": "",
  "data": []
}
```
- **status**: Deve trazer o código de status das respostas HTTP, deve estar no formato **integer** e respeitar o correto status code.
- **message**: Deve trazer a mensagem de retorno conforme a situação. Em caso de erros aqui deve estar o descritivo do erro e o detalhamento deve estar no data. Ex: `Campos obrigatórios não preenchidos!`
- **data**: Aqui deve estar o corpo do retorno, caso seja uma ação **GET** aqui deve estar o que espera-se retornar, caso seja **POST** aqui deve estar o que criado e caso seja **erro** aqui deve estar os detalhes do erro (como campos obrigatórios não preenchidos)


Lidando com os Erros
-------------

Como detalhado acima, os erros devem ser tratados de forma a termos uma mensagem a ser exibida para o usuário de forma amigável e um corpo de retorno que pode ser tratado para melhor detalhamento do erro.

**Exemplo**
```json
{
  "status": 400,
  "message": "Informações inválidas!",
  "data": {
    "name": [
      "Nome é obrigatório."
    ],
    "address": [
      "Endereço do Cliente é obrigatório."
    ],
    "address.zip_code": [
      "CEP do Cliente é obrigatório."
    ]
  }
}
```

Todos os retornos devem ser tratados e retornados em português, com excessão caso o projeto tenha internacionalização, nesse caso deverá ser na lingua padrão do projeto.

Deve-se também tratar corretamente os códigos de erro, respeitando assim suas definições:

- **400**. Deve ser utilizado quando a entrada do lado do cliente falhou na validação.
- **401**. Deve ser utilizado quando o usuário não está autorizado a acessar um recurso. Geralmente retorna quando o usuário não está autenticado.
- **403**. Deve ser utilizado quando o usuário está autenticado, mas não tem permissão para acessar um recurso.
- **404**. Deve ser utilizado quando um recurso requisitado não foi encontado.

Os erros da faixa **500** são destinados para erros do servidor, portanto não deverão ser lançados manualmente pela aplicação. 

Rotas paginadas e filtráveis
-------------

O projeto deve ser possível ser paginado e filtrado, isso é aplicável para rotas que tenham como intuito trazer listas para serem utilizadas em Tabelas e DataTables.

O [Projeto Padrão Laravel](https://github.com/bitzentecnologia/default-laravel-project) já possui um tratamento tanto para o filtro quanto para as questões de paginação. Este tratamento constitui no seguinte.

- Deve-se passar o parâmetro `search` na rota com o termo a ser filtrado.
- Por padrão será feita a busca utilizando a coluna name na tabela relacionada ao endpoint, porém caso queira expandir é necessário criar um arquivo de **Filter** específico.
- A paginação é feita ao informar como parâmetro os atributos `perPage` e `page`


