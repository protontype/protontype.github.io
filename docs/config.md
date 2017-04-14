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
| **cors**          | cors.CorsOptions | Configuração do CORS da aplicação. O Protontype usa o módulo [cors](https://www.npmjs.com/package/cors) para fazer esse trabalho. Esta propriedade segue o mesmo [objeto de configuração do módulo cors](https://www.npmjs.com/package/cors)|
| **logger**        | LoggerConfig     | Configurações de log                                                                                                                                                                                                                        |
| **https**         | HTTPSConfig      | Habilita e configura o HTTPS na aplicação                                                                                                                                                                                                   |
| **defaultRoutes** | boolean          | Habilita a configuração das rotas parões que a aplicação disponibiliza.                                                                                                                                                                     |

### DatabaseConfig

| Propriedade | Tipo              | Descrição                                                                                |
|-------------|-------------------|------------------------------------------------------------------------------------------|
| **name**        | string            | Nome do banco de dados                                                                   |
| **username**    | string            | Username para conexão com banco de dados                                                 |
| **password**    | string            | Senha para conexão com o banco de dados                                                  |
| **options**    | sequelize.Options | [Opções de configuração do Sequelize](http://docs.sequelizejs.com/en/v3/api/sequelize/). |

#### sequelize.Options

| Propriedade | Tipo   | Descrição                                                                        |
|-------------|--------|----------------------------------------------------------------------------------|
| **dialect**     | string | Dialeto do banco de dados. Podem ser: mysql, postgres, sqlite, mariadb ou mssql. |
| **storage**     | string | Somente usado para sqlite. Local do aquivo usado para o banco de dados           |

### LoggerConfig

| Propriedade | Tipo    | Descrição                                         |
|-------------|---------|---------------------------------------------------|
| **enabled**     | boolean | Habilita o log                                    |
| **transports**  | Object  | Configura onde os logs serão exibidos ou gravados |

#### transports
| Propriedade | Tipo                     | Descrição                                                         |
|-------------|--------------------------|-------------------------------------------------------------------|
| **type**        | string                   | Pode ser: "file" ou "console"                                     |
| **options**     | winston.TransportOptions | Opções do módulo [Winston](https://www.npmjs.com/package/winston) |

**Exemplo**
```json
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
  }
```

### HTTPSConfig

| Propriedade | Tipo    | Descrição                                        |
|-------------|---------|--------------------------------------------------|
| **enabled**     | boolean | Habilita o HTTPS                                 |
| **key**         | string  | Chave privada do certificado                     |
| **cert**        | string  | Arquivo que contém o certificado (chave pública) |