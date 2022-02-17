Padrões de Projeto
=======================================================

As nosas API's desenvolvidas em Laravel deverão possuir os seguintes requisitos iniciais:

- Versão do PHP 8.1
- Laravel 8.x
- Mysql e Postgress (last stable version)
- Vue 3

OBS: Para outros bancos como <b>Oracle</b> a versão deverá ser definida no momento da proposta do projeto.
What is this? <a name="what"></a>

Regras de nomenclatura
-------------
Toda a padronização de nomes dos projetos deverá ser feita em <b>Inglês</b>, salvo exceções como nomes específicos de regras sem uma tradução confiável para o inglês, está regra se aplica tanto para nomes de Entidades, Métodos e End Points como também para variáveis. As descrições contidas em anotações podem seguir em português

A seguir a estrutura da nomenclatura dos elementos da aplicação:

- Classes: Em **UpperCamelCase**. Ex: `OrderItems`
- Funções: Em **lowerCamelCase**. Ex: `getList`
- Variáveis: Em **lowerCamelCase**. Ex: `listOfItems`
- Endpoints: Em **kebab-case**. Ex: `get-by-state`

Seguem mais algumas convenções específicas para a estrutura do framework Laravel

<h3>Models</h3>

- Deve estar no singular.
- Assim como as demais classes deve estar em **UpperCamelCase**.
- As propriedades da model devem estar em snake_case. Ex: `$this->created_at`
- Os métodos que representam relacionamentos na model devem seguir o seguinte padrão:
  - Relacionamentos do tipo hasOne ou belongsTo devem estar no singular. Ex: `public function client()`
  - Relacionamentos do tipo hasMany, belongsToMany ou hasManyThrough devem estar no plural. Ex: `public function products()`

**Exemplo:**
```php
class Product extends Model
{
    protected $fillable = [
        'name',
        'value',
        'category_id',
        'order',
    ];

    public function category()
    {
        return $this->belongsTo(Category::class, 'category_id');
    }

    public function orders()
    {
        return $this->belongsToMany(Order::class, 'orders', 'product_id');
    }
}
```

<h3>Controllers</h3>

- Deve estar no singular.
- Assim como as demais classes deve estar em **UpperCamelCase**.
- Sempre finalizado com Controller

**Exemplo:**
```php
class ProductController extends Controller
{}
```

<h3>Testes</h3>

As normas a seguir se aplicam para testes do tipo Feature, para testes unitários serão definidas outras normas no futuro.

- O nome do método deve ser o mesmo do Controller testado seguido pelo termo **Test**. Ex: `UserControllerTest()`
- O nome dos métodos do teste devem sempre iniciar com **testMust** seguido pelo que será testado em **camelCase**. Ex: `testMustReturnUserList`

<h3>Traits</h3>

- Deve ser um adjetivo. Ex: `Sortable`

OBS: Algumas vezes a lógica contida da Trait pode não fazer sentido utilizar esta regra, nestes casos não a problema desde que o nome reflita o que a Trait se propões a fazer.

<h3>Blade</h3>

- Deve estar em snake_case. Ex: `list_products.blade.php`

Organização estrutural do projeto
-------------
Para fins de referência da estrutura do projeto (organização dos diretórios), deve-se sempre utilizar o [Projeto Padrão Laravel](https://github.com/bitzentecnologia/default-laravel-project).

Porém, referente as regras determinadas sobre a utilização das Entidades presentes no projeto, segue as normas definidas:

- **Models**. Devem apenas conter o que é referente ao modelo do banco, portanto os métodos presentes devem apenas representar os relacionamentos da tabela retratada ou seus atributos.
- **Controllers**. Devem se preocupar apenas em redirecionar para qual Service a requisição será direcionada, nenhuma regra de negócio ou consulta deve ser tratada aqui.
- **Services**. Representam a regra de negócio, toda a lógica referente a Entidade deve ser tratada aqui, porém nenhum método que faça consultas ou alterações no banco devem estar presentes em seus métodos.
- **Repositories**. Devem conter toda a interação com o banco de dados, os seus métodos devem contar todas as querys relevantes para a Entidade.

Padrões de Projeto
-------------

Os projetos realizados pela Bitzen passaram a ser cobrados alguns padrões e convenções para desenvolvimento de aplicações em PHP. Alguns desses padrões (como PSR2) já são checados automáticamente pela ferramenta de CI do projeto, isso caso ele tenha sido criado a partir do [Projeto Padrão Laravel](https://github.com/bitzentecnologia/default-laravel-project).



Testes Automatizados
-------------

Ao elaborar um **Teste Automatizado** tenha em mente os seguintes pontos:

- Cada Entidade de Teste deve refletir um Controller do projeto, e os seus Métodos refletir interações com os endpoints que se comunicam com este Controller.
- Ao pensar no teste, pense primeiro nas interações possíveis com o endpoint, tentando abordar o máximo possível de casos de uso.
- Ao criar o teste, deve-se procurar validar não apenas o retorno positivo ou negativo, deve-se também validar o corpo do retorno (quando existir) para certificar-se que corresponde com o esperado.
- Sempre que necessário criar registros para o teste, deve-se utilizar as Factory da Model a ser testada.


**Exemplo:**
```php
class UserControllerTest extends TestCase
{
    private $endPoint = "/api/users";

    public function testMustReturnUserList()
    {
        $users = User::factory(10)->create();
        $response = $this->json('GET', $this->endPoint);

        $response
            ->assertStatus(200)
            ->assertJsonStructure([
                'data' => [
                    '*' => array_keys($users->first()->toArray()),
                ],
            ]);
    }

    public function testMustReturnUserWithSearch()
    {
        $users = User::factory(10)->create();
        $url = sprintf('%s?search=%s', $this->endPoint, $users->first()->name);
        $response = $this->json('GET', $url);

        $response
            ->assertStatus(200)
            ->assertJsonStructure([
                'data' => [
                    '*' => array_keys($users->first()->toArray()),
                ],
            ]);
    }
}
```

Utilizando as Seeders
-------------

O objetivo das Seeders no projeto deverá ser única e exclusivamente para a criação de registros padrões no banco de dados, informações que são obrigatórias existir e das quais o usuário não tem acesso para criar ou excluir.

Dados para uma tabela de **Estados** ou **Categorias** são bons exemplos de informações que podem ser inseridas utilizando uma Seeder. É recomendável evitar inserir, por exemplo, usuários master que não pertençam ao cliente (os famosos usuários de teste), este tipo de usuário deve ser limitado apenas a bases locais e de testes e portanto inseridos por outros meios.

Dados de Testes **nunca** devem estar em uma Seeder, isso pode fazer com que o banco de produção fique poluído, ao invés de criar uma Seeder para ter registros a serem testados, o correto é a criação correta das Factories e estas serem utilizadas dentro dos Testes do sistema.

Outra prática que deve ser evitada na Seeder é a fixação de ID na hora de criar um registro, colunas do tipo autoincrement não nos permitem ter a previsibilidade total do seu valor gerado e devido a isso não devemos assumir que o mesmo ID que está para um registro em nossa base se manterá nas temais. Ao invés de fixar um ID o correto é utilizar uma referencia, algo único que identifique aquele registro sem ser o ID, e utilizar está referência para buscar o registro correto e pegar seu ID atual no momento em que a Seeder criará o novo registro.

**Exemplo**
```php
class ProductSeeder extends Seeder
{
    private $products = [

        [ 
            'name' => 'Televisão Marca X', 
            'category_id' => 'Eletrônicos', 
        ],
        [ 
            'name' => 'Home Theater Y', 
            'category_id' => 'Eletrônicos', 
        ],
    ];
    
    public function run()
    {
        foreach ($this->products as $product) {
            if (!Product::where('name', $product['name'])->first()) {
                $product['category_id'] = $this->getCategory($product['category_id']);
                Product::create($product);
            }
        }
    }
    
    public function getCategory($categoryId)
    {
        $category = Category::where('name', $categoryId)->first();
        return $category->id;
    }
}
```