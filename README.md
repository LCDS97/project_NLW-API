# NLW-API

## Sumário:
- [Sobre](#about)
- [Conceitos](#concepts)

# NLW VALORIZA
> Projeto da semana Next Level Week
## :file_folder: Estrutura do Projeto
<pre>
<p> Futuramente colocarei a estrutura do projeto aqui
</pre>
## Sobre o projeto

<hr>
<h2>:pencil: Conceitos aprendidos </h2> <a name="concepts"></a>

<details>
<summary> Dia 1 - Liftoff</summary>

## Aula 1 - Introdução do Projeto
Utilizaremos o yarn como a biblioteca de dependências para o projeto

> Dependências

Além de aula de hoje ter explicado conceito e diferença de Dev Dependencies e Dependencies:
<pre>
> Dev Dependencies:
As bibliotecas nesse ambiente servem somente para o desenvolvimento do projeto
</pre>
<pre>
> Dependencies:
Bibliotecas que serão utilizados na aplicação em produção
</pre>


> Bibliotecas

Algumas libs que iremos utilizar para iniciar o projeto:
- Express e também @types/express para tipagens comuns do express
- Typescript para node
- ts-node-dev para configurar o node para ler arquivos .ts(typescript)

> Métodos HTTP

Os métodos HTTP que serão utilizados no projeto irá ser:
<pre>
* GET => Buscar uma informação/dado
* POST => Inserir uma informação/dado
* PUT => Alterar uma informação/dado
* DELETE => Deletar uma informação/dado
* PATCH => Alterar uma informação/dado especifíca
</pre>

> Rotas

Sempre dentro das rotas temos dois parâmetros:
<pre>
* Request ou req => Informações/Dados de Entrada
* Response ou res => Informações/Dados de Saída
</pre>

> Programas

No projeto estou utilizando o POSTMAN para gerenciamentos e testes das rotas
Também utilizando o beekeeper studio para o SQL

> Código da AULA 1 - #Together

</details>

<details>
<summary>Dia 2 - Maximum Speed</summary>

## Aula 2 - TypeORM
> Banco de Dados

<details>

<summary>Schema do projeto</summary>
<pre>
├──USER                                     ├──Tag       
├ ├──(PK) ID (uuid)                         ├ ├──(PK) ID (uuid)
├  ├──name (varchar)                        ├  ├──name (varchar)
├  ├──name (varchar)                        ├  ├──created_at (Date)
├  ├──name (varchar)                        ├  ├──updated_at (Date)
├  ├──email (varchar)                       ⬇ 
├  ├──password (varchar)                    ⬇ 
├  ├──admin (boolean)                       ⬇ 
├  ├──created_at (Date)                     ⬇
├  ├──updated_at (Date)                         ├──Compliments
⬇                                               ├ ├──(PK) ID (uuid)
⬇                                               ├  ├──(FK) user_sender (uuid)  
➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡➡        ├  ├──(FK) user_receiver (uuid)
                                                ├  ├──(FK) tag_id (uuid)
                                                ├  ├──created_at (Date)



</pre>


</details>

> Tipos de parâmetros
<pre>
* Routes Params => http://localhost:3000/produtos/(params)
- O route params serve para definir para acessar um parametro para a rota, por exemplo para acessar um id de um produto, ou seja o id é o paramêtro nesse caso
- O route params são obrigatórios pois são implicitos nas rotas

* Query Params => http://localhost:3000/produtos?(chave)=(valor)&(outroChave)=(outroValor)
- Serve nesse para fazer filtros ou buscar dentro das nossas rotas, sempre para começar ele é com "?" e sempre segue o padrão de chave e valor para colocar outro filtro utiliza-se o "&"
- Os Query params são opcionais, não são implicitos nas rotas

* Body Params => {
    "name": "teclado",
    "description": "teclado-bom"
}
- Vem no corpo da requisição os paramêtros, tanto JSON, txt, qualquer valor no corpo da requisição
</pre>
Existem 3 formas para utilizar o banco de Dados

- Pelo próprio driver
- Query Builder
- ORM(Object Relational Map)

No projeto iremos utilizar o ORM com a biblioteca do TypeORM
Para utilizar o TypeORM é necessário também o driver do banco para qual irá utilizar, no projeto ira ser

- typeorm
- reflect-metadata
- sqlite3

### TypeORM

No TypeORM existem varias maneiras para definir suas propriedades de acordo com a documentação, mas no projeto iremos utilizar por JSON, na raiz do projeto iremos criar o **ormconfig.json**

Dentro do JSON do ormconfig.json temos algumas propriedades:

- type: Qual driver de banco ira ser utilizado no projeto
- database: Arquivo criado na raiz do projeto * quando utilizado no projeto de sqlite ele cria um arquivo de banco de dados

Dentro da pasta database iremos criar um index.ts com um import createConnection que vem do typeORM, chamando a função no arquivo e assim, o typeORM ira criar a conexão do banco de Dados

### Migrations

Migrations são vercionamentos do banco de dados da aplicação, ou seja, no caso cada alteração no banco de dados é criado um registro e é criado um **histórico** do banco de dados da aplicação, principalmente útil para o trabalho em equipes pois mantém o banco de dados de todos da equipe da mesma forma, simplesmente roda as migrations e mantém sempre o mesmo banco de dados

Dentro do ormconfig é criado um cli.

CLI é uma ferramenta que pode ser utilizada no terminal de uma forma global na aplicação, no caso do projeto iremos utilizar o cli dentro da nossa biblioteca e indica aonde vai ser criado as migrations da aplicação
É necessário criar um script no package.json da aplicação:
```json
"typeorm": "ts-node-dev ./node_modules/typeorm/cli.js"
```
#### Cheatsheet-Migrations
<details>
<summary>Expandir</summary>
Para criar uma migration:
- yarn typeorm migration:create -n nomeDaMigration

Para executar a migration:
- yarn typeorm migration:run

Para criar entidades:
- yarn typeorm entity:create -n nomeDaEntidade
</details>

Em migrations existe dois métodos:

- **UP**: Serve para criações, alterações e adições

- **DOWN**: Caso precise desfazer é o processo reverso do up com dropTable

Na documentação você pode verificar aonde são salvas as migrations e tipo de arquivo a serem localizados, dentro da ormconfig.json em "migrations"

Para criar entidades de estruturas com a pasta automática e a base do arquivo utiliza-se dentro do cli do ormconfig json

```json
"cli": {
    "migrationsDir": "caminho/diretorio/aqui",
    "entitiesDir": "caminho/diretorio/aqui"
}
```
### Entidades

Entidades são tabelas do projeto, por exemplo entidade User é tabela User do projeto.
Pois no ORM funciona do seguinte fluxo:
<pre>
Entidade < . > ORM < . > BD {users}
</pre>

No arquivo da entidade para referenciar uma entidade a uma tabela do banco, somente colocar o nome da tabela dentro dos parametro do @Entity

Por questões das documentações do TypeORM no getting started, para o typescript, dentro do ts config tem que habilitar duas opções no tsconfig.json
- experimentalDecorators
- emitDecoratorMetadata

Também habilitar para false o strictPropertyInitialization, pois o js irá implicar que os atributos do entity ainda não foram inicializados

Para referencias as PK, colunas, e timestamps da tabela é necessário importar do typeorm as propriedades

{ PrimaryColumn, Column, CreateDateColumn, UpdateDateColumn }

Ira instalar também uma biblioteca da uuid e suas tipagens e importar ele dentro da Users e usaremos o uuid v4
Iremos também criar um construtor dentro da classe de User para o id utilizando a biblioteca do uuid
*Construtor é como a classe vai ser montada quando ela é invocado,ex: new nomeClasse*

### Repositório

Repositórios são para criar métodos, além daqueles que ja possue ou para criar tratativas, tudo isso é criado no customs repositories

No projeto iremos utilizar com uma classe do UsersRepositories.ts
É necessário também importar do typeORM o getCustomRepository para ele gerenciar o repositório que ira ser usado como base para criar um repositório customizado
Iremos extender a classe Repository para o nosso Repositório pois o typeORM ja tem alguns métodos definidos

### Serviços

O que são Services?
O fluxo dele é da seguinte forma:
<pre>
- server -> ( ) -> SERVICE -> Repositories -> BD {users}
</pre>

É a parte da aplicação onde faz todo tratamento e processamento das validações das regras de négocios, é a camada de serviço responsável para validação antes que a requisição seja enviada para o banco de Dados ou tratativa de retorno para o cliente

Toda aplicação tem suas regras de negócios, casos de usos, regras funcionais e não funcionais

<details>
<summary> NLW-Valoriza - Regras de Negócios Exemplo</summary>
### NLW VALORIZA

- Cadastro de usuário
- [ ] Não é permitido cadastrar mais de um usuário com o mesmo e-mail 
- [ ] Não é permitido cadastrar usuário sem e-mail 

- Cadastro de Tag
- [ ] Não é permitido cadastrar mais de uma tag com o mesmo nome
- [ ] Não é permitido cadastrar tag sem nome
- [ ] Não é permitido cadastrar por usuários que não sejam administradores

- Cadastro de elogios
- [ ] Não é permitido um usuário cadastrar um elogio para si
- [ ] Não é permitido cadastrar elogios para usuários invalidos
- [ ] O usuário precisa estar autenticado na aplicação

</details>

Conceito de código limpo - como SOLID para aplicar em projetos

Um dos conceitos é lembrar que toda classe existe por um motivo e que essa mesma classe não pode possuir responsabilidades demais construido dentro delas

No projeto foi criado o CreateUserService aonde sua única responsábilidade vai ser em relação ao cadastro de usuários e suas divisões de responsabilidades

Uma interface é para você indicar a classe quais tipos de objetos ele irá receber

### Controller
O controller no fluxo se encontra antes da camada de SERVICE
<pre>
- server -> controller -> service -> repository -> BD {users}
</pre>

Ele recebe a requisição do servidor e repassa para o service trata essa informação, dentro do controller teremos as informações do nosso request e do nossso response, assim deixando as responsabilidades corretas para aplicação

### Routes

Para não poluir nosso services e definir as rotas da aplicação é criado um arquivo routes.ts dentro do src da aplicação, e nesse arquivo sempre ficas as rotas da aplicação, assim como o caminhos do controllers para serem direcionados a camada de serviços, nesse arquivo vai ficar todas as rotas da aplicação

> Para habilitar o express aceitar json em suas entradas, é necessário utilizar o app.use(express.json())

> Código da Aula 2 #unidade

</details>

<details>
<summary> Dia 3 - In Orbit</summary>

## Aula 3 - Tratativas e Middlewares
Nessa aula, iremos tratar a excessão e como utilizar os tratamentos e estruturas das tags e por fim conhecer o conceito de middleware na aplicação

> Para tratativas de erro HTTP, utilizar o http.cat como guia!

### Tratativas de erro
Ao lançar a excessão que para a camada de controller, temos que fazer a tratativa pelo controller, existem duas formas para fazer a tratativa:

- Pelo método try e catch, ou seja, tente fazer algo se não conseguir cai no catch e recebo o erro
- Pelo método de tratar no server, com um middleware das rotas

Com try e catch fica muito massante para aplicações largas, então a melhor forma é tratar na camada, ao invés de tratar no controller, fazer a tratativa no server com um middleware

<pre>
- Controller -> Service (throw new Error)
Iremos tratar com um middleware para tratativas que ocorrerão no server
- server(middleware) -> controller -> ...
</pre>

### Middleware

Middlewares são interceptadores que usamos dentro de uma requisição tanto como **interromper** ou como **adicionar uma informação** dentro do middleware, seria algo no meio entre a requisição e a resposta

> O papel do middleware é pegar as respostas das rotas e fazer uma tratativas verificando se há algum erro na rota

#### Cheatsheet-Middlewares
<details>
<summary>Expandir</summary>

- Middleware de erro possui 4 paramêtros, do tipo err, request, response e next
```js
 ((err: Error, request: Request, response: Response, next: NextFunction)
```

Temos que verificar qual a instância do erro:

Pode ser que ele seja um erro não tratável que não é tratado pela aplicação, ou erro de servidor como 500

Por padrão do express, ele não consegue capturar os erros de requisição aonde se utiliza o async, ele não consegue capturar os erros que estão vindos (erros assíncronos), ou seja é necessário utilizar uma biblioteca para tratar esses erros

- yarn add express-async-errors

E importar no server.ts que ele ja ira conseguir tratar de lidar com esses erros

Tratando dessa forma é sempre uma regra o middleware ser utilizado depois da rota chamada, pois ele necessita tratar a resposta depois que ela é enviada ao service for chamado

</details>

### Criar estrutura - tags

Iremos criar uma nova migration para as tags, com o nome de CreateTags

Irá conter colunas de ID, name, created_at, updated_at

Agora iremos aplicar as regras de negócio, criaremos uma entidade chamada Tag com as suas columnas e entidades, e também criar um TagsRepositories onde ira extenders as funções de repositório do TypeORM
e também um CreateTagService

> Quando precisa só referenciar somente um valor na interface pode ser executado direto no parametro do execute
```js
class exemploService {
    async execute(valor: tipo)
}
```

Dentro do service iremos tratar alguns erros em questão de verificar se o nome é invalido/incorreto, se a tag ja existe

O conceito do find One do repositories é como exatamente o comando de SQL
SQL:
<pre>
SELECT * FROM EXAMPLE WHERE NAME = "name"
</pre>
TypeORM:
<pre>
const valueAlreadyExists = await exampleRepositories.findOne({ name })
</pre>

Criado também o controller da Tag para tratar request e response e pegar o body name da tag pelo request, e depois do controller criado nós referenciamos ele em nossa rota no route.ts, criando uma rota post e aplicando o handle do controller

Depois a validação vai estar confirmando no banco se ja existe a tag ou se o nome está incorreto/nulo, agora iremos cadastrar a parte onde não é permitido o cadastro de tags por usuários que não sejam administradores

Para verificar é necessário ter na rota dos tags para cadastrar a tag, uma validação para verificar se o usuário que faz a requisição se é administrador ou não

Iremos criar uma pasta de middleware para cadastro de todos essas verificações

É criado o arquivo ensureAdmin para verificar se o usuário que esta fazendo a requisição é admin, como é um middleware de erro sempre é necessário importar os três parametros que são: *Request, Response, NextFunction*

Como no momento do projeto ainda não foi implementado o JWT, iremos controlar a variavel do admin, para true

Para verificar de se o usuário é admin é aplicado o next, caso não seja é retornado o http status 401 de Unauthorized

Algo bacana para utilizar no projeto deixar o status Code global o usuário ja consegue identificar melhor o que realmente está acontecendo para receber aquele status code

Depois usaremos o middleware para nossa rota, no routes.ts

Não se utiliza o router.use no middleware pois se for usado todas as rotas serão obrigados a passar por aquele middleware, ou seja, por exemplo uma rota de cadastro que não existe o usuário não faz sentido ele ter uma verificação de admin, nesse caso de middleware nós especificamos ele entre o caminho da rota e o controller dele;
Você pode colocar quantos middlewares que achar necessário para executar
```js
router.post("/router", exampleMiddleware, exampleMiddleware2, exampleController)
```


> Código da aula 3 - embuscadeevolucao

</details>

<details>

<summary>Dia 4 - Landing</summary>

## Aula 4 - Trabalhando com websockets

### JWT - Json Web Tocken

Através do JWT, ele distribui esse token para poder manter autenticação do usuário e pode atuar

Como funciona o token? É divído em 3 partes

- Header: tipo de Token e o algoritmo de criptografia
- Payload: São informações que precisamos passar de dentro do token, por exemplo, id, email, nome do usuario, tempo de expiração
- Verificação de assinatura: Ele ira concatenar o header e o payload alem da chave secreta do JWT que é criado nossa chave de API

> JAMAIS, coloque a senha do usuário no token, por mais que seja seguro, ou não manipulavel, se houver dados sensíveis é possível descriptografar

Iremos utilizar a biblioteca do jsonwebtoken e suas tipagems @jsonwebtocken como dependencia de desenvolvimento


Para criar nosso token, temos que garantir que é um usuário que exista na banco de dados e seus dados que estão enviando se são corretos
### Hash de senhas

No projeto iremos agora adicionar uma migration contendo uma coluna de senha na tabela de users

- yarn typeorm migration:create -n AlterUserAddPassword

E adicionar uma coluna do type varchar com o nome de password, alem adicionar na entidade de User, no controller e no service

No momento a aplicação esta salvando a senha em texto plano e isso é **PÉSSIMA PRÁTICA DE SEGURANÇA**, pois você pode deixar seus usuarios daquele sistema totalmente vulneraveis, alem da quebra de privacidade.

Iremos instalar a biblioteca do bcryptjs para conseguir, além da suas tipagens

- yarn add bcryptjs
- yarn add @types/bcryptjs

Dentro do CreateUserService, iremos importar o hash da biblioteca do bcryptjs e criar uma criptografia para o password ser convertido no hash desejado

Antes do chamar o repositório para criar o usuário, iremos definir uma const, chamando a função do hash, os dois paramêtros que ele recebe a primeira é a senha e o segundo é o salt, que é tipo de criptografia, ou seja, o tamanho da criptografia para o seu salto, um padrão a ser utilizado é o tamanho 8, e é uma promise definindo um await nele, como o create do repositório é os dados que serão enviados para o banco, temos que alterar para o campo password receber a nossa password com hash, definindo pelo nome do campo e o valor que vai ser atribuido
```js
const passwordHash = await hash(password, 8) // Criado hash da senha com salt 8

const user = usersRepository.create({
    name,
    email,
    admin,
    password: passwordHash // Valor alterado
});
await usersRepository.save(user);
return user;
```

### Autenticação do usuário

Iremos criar um service para a autenticação do usuário, com o nome AuthenticateUserService.ts

A classe de autenticação ira esperar para receber o email e password, e iremos criar uma interface esperando esses dois paramêtros como string

Na classe de execute temos duas tratativas:

- Verificar se email existe;
- Verificar se a senha está correta

Na trativa de se o usuário existe, iremos usar do userRepositorie para verificar se o email ja existe no banco com o findOne

> Quando passa uma informação de retorno com dados sensíveis, por questões de segurança da aplicação, nós precisamos deixar a mensagem de erro mais genérica, afim de deixar aplicação mais segura para evitar de brute Forces

Para verificar a senha usaremos outro método do bcryptjs que é o compare, para permitir comparar duas senhas, pois quando você a informação do usuário para a validação, o usuário irá enviar uma senha com texto plano, e a função compare permite converter esse texto do usuário para um hash, com a intenção de comparar o hash que está no banco de dados para permitir o usuário logar
Essa função esta comparando a informação do user com a do banco de dados, e ele retorna como booleano
- True : Senha Correta
- False: Senha Incorreta
```js
const passwordMatch = await compare(password, user.password);
```

E por fim se tudo estiver certo, iremos gerar o token para o usuário, mas primeiro precisamos importa do jsonwebtocken, importa o sign que vai ser gerado o token, e depois criar uma const de token com a função do sign, passando o payload e o secret do jsonwebtoken, e também temos a opção de definir algumas opções para o nosso token, como um subject, , por exemplo qual informação que passar, por exemplo, o id do usuário, e também um tempo de expiração.

```js
const token = sign({
    exma: user.email
}, "json-secret-aqui" , {
    subject: user.id,
    expiresIn: "1d",
});
return token;
```

Para um cenário ideal um token menor de 15 minutos e utilizar um refreshToken com tempo de expiração maior e quando esse token expirar, em vez do cliente inserir novamente o email e a senha dele, a aplicação (front-end) ele vai armazenar esse refreshToken e cada x tempo que esse token expirar, ele vai auto-gerar um novo token com base no refreshToken

> Para utilização de jsonwebsecret, podemos usar o generator de md5 para maior segurança da nossa aplicação
> Colocar no projeto depois varíaveis de ambiente para uma milha extra

Iremos agora criar um controller para fazer o handle do nosso request de email e password, primeiramente criando a classe, recebendo o token e passando o email e  a senha, e se tudo der certo é retornado um response.json contendo o token.
```js
    async handle(request: Request, response: Response){
        const {email, password} = request.body

        const authenticateUserService = new AuthenticateUserService();

        const token = await authenticateUserService.execute({
            email,
            password
        });

        return response.json(token);
```

Agora iremos adicionar esse controller em nossas rotas, declarar um novo objeto do AuthenticateUserController, e definir uma rota post para a rota que você quiser, no projeto iremos utilizar o login.

Crie uma rota no postman com a rota colocada e teste enviando um body JSON com email e password cadastrado do banco!

> Você pode verificar o token de que veio da rota no site do [!JWT]https://jwt.io, mostrando no token algumas propriedades como o email, tempo de criação e expiração 

</details>
