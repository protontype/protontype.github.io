# Middleware de autenticação

**Protontype** usa o projeto [passportjs.org](http://passportjs.org/ "") para autenticação das rotas.

Um middleware de autenticação deve ser uma classe que *extends* de ```AuthMiddleware``` e deve implementar o método:

```javascript
authenticate(): express.Handler
```

O exemplo abaixo demonstra um middleware de autenticação JWT

```javascript
export class JWTAuthMiddleware extends AuthMiddleware {
    private passportInstance: passport.Passport;
    private config: SpecificConfig = ProtonConfigLoader.loadConfig();

    public configMiddlewares(): void {
        this.passportInstance = passport;
        let userModel: UsersModel = this.protonApplication.getModel<UsersModel>(ModelNames.USERS);

        let params: StrategyOptions = {
            secretOrKey: this.config.jwtSecret,
            jwtFromRequest: ExtractJwt.fromAuthHeader()
        };

        const strategy: Strategy = new Strategy(params, async (payload: any, done: VerifiedCallback) => {
            try {
                let user: User = await userModel.getInstance().findById(payload.id);
                if (user) {
                    return done(null, {
                        id: user.id,
                        email: user.email
                    });
                }
                return done(null, false);
            } catch (error) {
                return done(error, null);
            }
        });
        this.passportInstance.use(strategy);
        this.protonApplication.getExpress().use(this.passportInstance.initialize());
    }

    public authenticate(): express.Handler {
        return this.passportInstance.authenticate("jwt", this.config.jwtSession);
    }

}
```

## Configurando na Aplicação

```javascript
let protonApp = new ProtonApplication();
protonApp
    .withAuthMiddleware(new JWTAuthMiddleware())
    .bootstrap();
```