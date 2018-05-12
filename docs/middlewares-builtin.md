# Built-in Middlewares

O Protontype disponibiliza alguns middlewares embarcados. Para saber como criar e usar middlewares, ver sessão de [Middlewares](middlewares) desta documentação

## BodyParserMiddleware

Middleware que faz o parse do request para propriedade ```params.req.body``` no formato texto. Para pasers do request em mais formatos pode-se usar o módulo [proton-body-parser](https://github.com/protontype/proton-body-parser).

```typescript
@RouterClass({baseUrl: "/tasks"})
export class TaskRouter extends ExpressRouter {
    @Route({
        endpoint: '/',
        method: Method.GET,
        middlewares: [ new BodyParserMiddleware()]
    })
    listTask(params: RouterFunctionParams) {
        console.log(params.req.body);
    }
}
```
> O [TypeORMCrudRouter](basecrudrouter) usa o ```BodyParserMiddleware``` para ler os campos recebidos no request das suas rotas default.

## JsonContentMiddleware(**[pretty?: boolean]**)

Middleware que adiciona informações básicas no cabeçalho das respostas em formato JSON. Pode ser informado através do parâmetro booleano ```pretty``` se o JSON deve ser indentado.

```typescript
let basicJsonMiddleware = new JsonContentMiddleware();
let app = new ProtonApplication()
    .addMiddleware(basicJsonMiddleware).start();
``` 

```typescript
let indentedJsonMiddleware = new JsonContentMiddleware(true);
let app = new ProtonApplication()
    .addMiddleware(indentedJsonMiddleware).start();
``` 

## CORS

O CORS já tem suporte embarcado no Protontype e pode ser ativado através do arquivo de configuração. Ver sessão de [Configurações](config#configuracoes-da-aplicacao).

## Helmet

O middleware [Helmet](https://github.com/helmetjs/helmet) ajuda a proteger a aplicação de algumas vulnerabilidades conhecidas através de configurações no cabeçalho HTTP. Este middleware já é habilitado por padrão no ```start()``` da aplicação.

