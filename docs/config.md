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
      "experimentalDecorators": true,
      "esModuleInterop": true,
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
    "name": "defaultTestConnection",
    "type": "sqlite",
    "database": "proton.sqlite",
    "synchronize": true,
    "logging": false,
    "entities": [
      "./dist/models/**/*.js"
    ]
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

```typescript
export interface GlobalConfig {
    port: number;
    database: any;
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
| **database**      | ConnectionOptions  | Configuração da base de dados usada pela aplicação (Depende do DBConnector usado)                                                                                                                                                                                         |
| **cors**          | cors.CorsOptions | Configuração do CORS da aplicação. O Protontype usa o módulo [cors](https://www.npmjs.com/package/cors) para fazer esse trabalho. Esta propriedade segue o mesmo [objeto de configuração do módulo cors](https://www.npmjs.com/package/cors)|
| **logger**        | LoggerConfig     | Configurações de log                                                                                                                                                                                                                        |
| **https**         | HTTPSConfig      | Habilita e configura o HTTPS na aplicação                                                                                                                                                                                                   |
| **defaultRoutes** | boolean          | Habilita a configuração das rotas parões que a aplicação disponibiliza.                                                                                                                                                                     |

### Database (ConnectionOptions)
Dependerá do DBConnector usado. Por default o Protontype usa o TypeORM. 
As propriedades do objeto **database: {...}** será de acordo com as configurações do TypeORM.

Ver [connection options do TypeORM](http://typeorm.io/#/connection-options/connection-options-example)

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

## Acesso as Configurações na Aplicação

Para ter acesso as propriedades do arquivo de configuração dentro da aplicação, o módulo disponibiiza a classe ```ProtonConfigLoader```.
O método ```loadConfig(filePath?: string)``` retorna um objeto do tipo ```GlobalConfig```.

```typescript
let config: GlobalConfig = ProtonConfigLoader.loadConfig();
```

Pode-se opcionalmente espeficicar o caminho do arquivo. Caso não seja informado a função procurará um arquivo **proton.json** na raiz do projeto.

```typescript
let config: GlobalConfig = ProtonConfigLoader.loadConfig('./config/custom-config.json');
```