# MGDB

É uma biblioteca em PHP para trabalhar com banco de dados MySQL/MariaDB de uma forma mais fácil e rápida. Com o MGDB é possível consultar, inserir, alterar e excluir registros de um banco de dados, usando query ou prepare.

## Instalação

Você pode instalar manualmente, baixando o arquivo zip e extraindo na sua pasta ou instalar pelo Composer.

```
composer require mugomes/mgdb
```

## Primeiros Passos

Para usar o MGDB no seu projeto é bem simples, basta você instanciar a classe que você quer usar.

Exemplo

```
$db = ['server' => 'localhost', 'username' => 'admin', 'password' => 'admin', 'database' => 'meudb'];
$db1 = new select($db);
```
Se você está em um servidor local para desenvolvimento do seu projeto, ative a opção "sandbox" para caso houver erros, que sejam exibidos na página.

Exemplo

```
$sandbox = true;
$db = ['server' => 'localhost', 'username' => 'admin', 'password' => 'admin', 'database' => 'meudb'];
$db1 = new select($db, $sandbox);
```

Para consultar um registro use a classe "select", para inserir use a classe "insert", para atualizar use a classe "update", para excluir um registro use a classe "delete".

## Consultando Registro

No exemplo abaixo iremos fazer uma consulta básica de um registro usando query.

```
$sandbox = true;
$db = ['server' => 'localhost', 'username' => 'admin', 'password' => 'admin', 'database' => 'meudb', 'prefix' => 'tb_'];

$db1 = new select($db);
$db1->table('clientes')
    ->select();

while ($row = $db1->fetch()) {
    $db1->rows($row);

    echo $db1->row('nome');
}

$db1->close();
```

Você pode usar o array direto ou usar a classe rows para armazenar o array e depois obter o valor com row.

Qual a diferença de um para o outro?

```
echo empty($row['nome']) ? '' : $row['nome'];
echo $db1->row('nome');
```

Viu a diferença? O método row já faz a verificação se a chave nome contém valor e retorna o valor, dessa forma o código fica mais limpo e organizado.

Para usar com o modo preparado é só realizar da seguinte forma:

```
$db1 = new select($dbBlogs1);

$db1->table('users')
    ->where('id')->prepared($txtID)
    ->select();

$db1->getResult();

while ($row = $db1->fetch()) {
    $db1->rows($row);

    echo $db1->row('nome');
}

$db1->close();
    
```

No modo preparado foi usado apenas o método prepared e o getResult de extra, o restante do código permanece igual.

## Métodos

- table: Caso precise definir mais de uma tabela para usar o "INNER JOIN" é só setar o table novamente.

- where: Onde
- semAspas: Ideal para comparar dados de colunas
- and: o "e" está como padrão, mas caso tenha usado o "or" recomendo usar o "and" para voltar para o padrão, caso não use mais o "ou".
- or: Ou
- andNot: e não
- orNot: ou não
- whereCustom: caso não queira usar os métodos acima você pode customizar o where usando o whereCustom.

- orderby: Ordem, se definir o argumento como false a ordem será decrescente.

- limit: Aonde começar a consulta do registro e quantos registros mostrar.

- prepared: passar os valores para usar o modo preparado.

- add: Para adicionar ou atualizar registros usando a classe insert ou update. Caso esteja usando o modo preparado, você também deve usar o prepared.

```
$db2 = new insert($db);
$db2->table('users')
    ->add('valor')->prepared($txtValor)
    ->insert();
$db2->close();

$db2 = new update($db);
$db2->table('users')
    ->add('valor')->prepared($txtValor)
    ->where('id')->prepared($txtID)
    ->insert();
$db2->close();
```
- idinsert: para obter o ID do registro inserido.

- count: para obter a quantidade de registros retornados em uma consulta.

- close: limpa a tabela da memória (caso esteja) e fecha a conexão com o banco de dados.

# Doação
Se você puder ajudar esse projeto financeiramente, vou deixar formas de apoio abaixo.

 - PIX: pixmugomes@gmail.com
 - Asaas: https://www.asaas.com/c/72mxu6ilkis5rwxz

# License

The MGDB is provided under:

[SPDX-License-Identifier: MIT](https://spdx.org/licenses/MIT.html)

Licensed under the MIT license.