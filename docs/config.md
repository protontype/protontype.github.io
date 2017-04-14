# Configurações 

## Configuração do projeto TypeScript

As seguintes configurações no **tsconfig.json** são necessárias para o
funcionamento.

```json
{
    "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true
    }
}
```

## Configurações da Aplicação

Por padrão a aplicação procurará um arquivo ```proton.json``` que poderá ter as configurações: 

```json
{
  "port": "3000",
  "defaultRoutes": true,
  "database": {
    "name": "proton-example",
    "username": "",
    "password": "",
    "options": {
      "dialect": "sqlite",
      "storage": "./src/test/proton.sqlite",
      "define": {
        "underscored": "true"
      },
      "logging": false
    }
  },
  "cors": {
    "origin": "*",
    "methods": ["GET", "POST", "OPTIONS", "PUT", "PATCH", "DELETE"],
    "allowedHeaders": ["Content-Type", "Authorization"]
  },
  "logger": {
    "enabled": false,
    "transports": [
      {
        "type": "file",
        "options": {
          "filename": "./test/logs.log"
        }
      },
      {
        "type": "console"
      }
    ]
  },
  "https": {
    "enabled": false,
    "key": "./src/cert/cert.key",
    "cert": "./src/cert/cert.cert"
  }
}
```

## Estrutura do Arquivo de Configuração

```javascript
export interface GlobalConfig {
    port: number;
    database: DatabaseConfig;
    cors?: cors.CorsOptions;
    logger?: LoggerConfig;
    https?: HTTPSConfig;
    defaultRoutes?: boolean;
}
export interface DatabaseConfig {
    name: string;
    username: string;
    password: string;
    options: sequelize.Options;
}
export interface DBDefine {
    underscored: boolean;
}
export interface HTTPSConfig {
    enabled: boolean;
    key: string;
    cert: string;
}
export interface LoggerConfig {
    enabled: boolean;
    transports: {
        type: string;
        options: winston.TransportOptions;
    }[];
}
```

### GlobalConfig

| Proriedade        | Tipo             | Descrição                                                                                                                                                                                                                                   |
|-------------------|------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **port**          | number           | Porta em que a aplicação irá levantar                                                                                                                                                                                                       |
| **database**      | DatabaseConfig   | Configuração da base de dados usada pela aplicação                                                                                                                                                                                          |
| **cors**          | cors.CorsOptions | Configuração do CORS da aplicação. O Protontype usa o módulo [cors](https://www.npmjs.com/package/cors) para fazer esse trabalho. Esta propriedade segue o mesmo [objeto de configuração do módulo cors](https://www.npmjs.com/package/cors) |
| **logger**        | LoggerConfig     | Configurações de log                                                                                                                                                                                                                        |
| **https**         | HTTPSConfig      | Habilita e configura o HTTPS na aplicação                                                                                                                                                                                                   |
| **defaultRoutes** | boolean          | Habilita a configuração das rotas parões que a aplicação disponibiliza.                                                                                                                                                                     |

### DatabaseConfig