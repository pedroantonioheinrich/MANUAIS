# Manual Completo de NestJS

## Sumário
1. [Introdução ao NestJS](#introdução-ao-nestjs)
2. [Arquitetura e Conceitos Fundamentais](#arquitetura-e-conceitos-fundamentais)
3. [Instalação e Primeira Aplicação](#instalação-e-primeira-aplicação)
4. [Estrutura de Projeto](#estrutura-de-projeto)
5. [Módulos](#módulos)
6. [Controladores](#controladores)
7. [Providers e Serviços](#providers-e-serviços)
8. [Injeção de Dependências](#injeção-de-dependências)
9. [Pipes - Validação e Transformação](#pipes---validação-e-transformação)
10. [Guards - Autenticação e Autorização](#guards---autenticação-e-autorização)
11. [Interceptadores](#interceptadores)
12. [Middleware](#middleware)
13. [Filters - Tratamento de Exceções](#filters---tratamento-de-exceções)
14. [Configuração e Variáveis de Ambiente](#configuração-e-variáveis-de-ambiente)
15. [Banco de Dados e ORM](#banco-de-dados-e-orm)
16. [Autenticação JWT](#autenticação-jwt)
17. [WebSockets](#websockets)
18. [GraphQL](#graphql)
19. [Microserviços](#microserviços)
20. [Testes](#testes)
21. [Deploy e Produção](#deploy-e-produção)
22. [Boas Práticas e Padrões](#boas-práticas-e-padrões)

---

## Introdução ao NestJS

NestJS é um framework Node.js progressivo para construção de aplicações server-side eficientes, confiáveis e escaláveis. Utiliza TypeScript por padrão e combina elementos de OOP, FP e FRP.

### Princípios Fundamentais
- **Modularidade**: Aplicação estruturada em módulos independentes
- **Injeção de Dependências**: Gerenciamento automático de dependências
- **Arquitetura em Camadas**: Separação clara de responsabilidades
- **Plataforma Agnóstica**: Funciona com Express ou Fastify

### Por que usar NestJS?
- TypeScript nativo
- Arquitetura inspirada no Angular
- Ecossistema robusto e ativo
- Documentação excelente
- Suporte a REST, GraphQL, WebSockets, Microserviços
- Testabilidade nativa

---

## Arquitetura e Conceitos Fundamentais

### Arquitetura Modular
```
┌─────────────────────────────────────────┐
│             Módulo Principal            │
├─────────┬─────────┬─────────┬──────────┤
│ Módulo  │ Módulo  │ Módulo  │  Módulo  │
│   A     │   B     │   C     │    D     │
└─────────┴─────────┴─────────┴──────────┘
```

### Componentes Principais
1. **Modules**: Agrupamento de funcionalidades
2. **Controllers**: Manipulam requisições HTTP
3. **Providers**: Serviços, repositories, factories
4. **Middleware**: Processamento antes dos controllers
5. **Pipes**: Validação e transformação
6. **Guards**: Autenticação e autorização
7. **Interceptors**: Transformação de requests/responses
8. **Filters**: Tratamento de exceções

### Ciclo de Vida da Requisição
```
Request → Middleware → Guards → Interceptors → Pipes → Controller → Service → Interceptors → Response
```

---

## Instalação e Primeira Aplicação

### Pré-requisitos
- Node.js (v14 ou superior)
- npm ou yarn
- Conhecimento básico de TypeScript

### Instalação CLI
```bash
# Instalar CLI globalmente
npm i -g @nestjs/cli

# Criar novo projeto
nest new meu-projeto

# Opções disponíveis
nest new meu-projeto --package-manager yarn
nest new meu-projeto --skip-git
```

### Estrutura Inicial
```bash
meu-projeto/
├── src/
│   ├── app.controller.ts
│   ├── app.controller.spec.ts
│   ├── app.module.ts
│   ├── app.service.ts
│   └── main.ts
├── test/
├── .eslintrc.js
├── .gitignore
├── .prettierrc
├── nest-cli.json
├── package.json
├── README.md
├── tsconfig.build.json
└── tsconfig.json
```

### Primeiro Controller
```typescript
// src/app.controller.ts
import { Controller, Get } from '@nestjs/common';
import { AppService } from './app.service';

@Controller()
export class AppController {
  constructor(private readonly appService: AppService) {}

  @Get()
  getHello(): string {
    return this.appService.getHello();
  }

  @Get('users')
  getUsers(): string[] {
    return ['John', 'Jane', 'Bob'];
  }
}
```

### Executando a Aplicação
```bash
# Desenvolvimento (watch mode)
npm run start:dev

# Produção
npm run build
npm run start:prod

# Debug
npm run start:debug
```

---

## Estrutura de Projeto

### Estrutura Recomendada
```
src/
├── common/
│   ├── decorators/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   ├── middleware/
│   └── pipes/
├── config/
│   └── configuration.ts
├── database/
│   ├── entities/
│   └── migrations/
├── modules/
│   ├── auth/
│   │   ├── auth.controller.ts
│   │   ├── auth.module.ts
│   │   ├── auth.service.ts
│   │   ├── dto/
│   │   ├── guards/
│   │   ├── strategies/
│   │   └── interfaces/
│   ├── users/
│   └── products/
├── shared/
│   ├── constants/
│   └── utils/
├── app.module.ts
└── main.ts
```

### Configuração do Nest CLI
```json
// nest-cli.json
{
  "collection": "@nestjs/schematics",
  "sourceRoot": "src",
  "compilerOptions": {
    "deleteOutDir": true,
    "webpack": false,
    "tsConfigPath": "tsconfig.json"
  },
  "generateOptions": {
    "spec": true
  }
}
```

### Comandos CLI Úteis
```bash
# Gerar módulo
nest generate module users

# Gerar controller
nest generate controller users

# Gerar service
nest generate service users

# Gerar resource completo
nest generate resource users

# Gerar interface
nest generate interface users/user

# Gerar guard
nest generate guard auth/guards/jwt

# Gerar middleware
nest generate middleware common/middleware/logging
```

---

## Módulos

### Módulo Básico
```typescript
// src/users/users.module.ts
import { Module } from '@nestjs/common';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';

@Module({
  controllers: [UsersController],
  providers: [UsersService],
  exports: [UsersService], // Exporta para outros módulos
})
export class UsersModule {}
```

### Módulo de Funcionalidade
```typescript
// src/auth/auth.module.ts
import { Module, forwardRef } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { UsersModule } from '../users/users.module';
import { AuthService } from './auth.service';
import { AuthController } from './auth.controller';
import { JwtStrategy } from './strategies/jwt.strategy';
import { LocalStrategy } from './strategies/local.strategy';

@Module({
  imports: [
    forwardRef(() => UsersModule), // Resolve dependência circular
    PassportModule,
    JwtModule.register({
      secret: process.env.JWT_SECRET,
      signOptions: { expiresIn: '1d' },
    }),
  ],
  controllers: [AuthController],
  providers: [AuthService, JwtStrategy, LocalStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

### Módulo Global
```typescript
// app.module.ts
import { Module, Global } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import { AppController } from './app.controller';
import { AppService } from './app.service';

@Global() // Torna o módulo global
@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
    }),
  ],
  controllers: [AppController],
  providers: [AppService],
  exports: [AppService],
})
export class AppModule {}
```

### Módulos Dinâmicos
```typescript
// database/database.module.ts
import { Module, DynamicModule } from '@nestjs/common';
import { createConnection, ConnectionOptions } from 'typeorm';

@Module({})
export class DatabaseModule {
  static forRoot(options: ConnectionOptions): DynamicModule {
    const providers = [
      {
        provide: 'DATABASE_CONNECTION',
        useFactory: async () => await createConnection(options),
      },
    ];

    return {
      module: DatabaseModule,
      providers,
      exports: providers,
    };
  }
}

// Uso
imports: [DatabaseModule.forRoot(databaseConfig)]
```

---

## Controladores

### Controller Básico
```typescript
// src/users/users.controller.ts
import {
  Controller,
  Get,
  Post,
  Put,
  Delete,
  Body,
  Param,
  Query,
  Headers,
  HttpCode,
  HttpStatus,
  Header,
} from '@nestjs/common';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';

@Controller('users') // Prefixo da rota: /users
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @HttpCode(HttpStatus.OK)
  @Header('Cache-Control', 'none')
  findAll(
    @Query('page') page: number = 1,
    @Query('limit') limit: number = 10,
  ) {
    return this.usersService.findAll(page, limit);
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(id);
  }

  @Post()
  create(@Body() createUserDto: CreateUserDto) {
    return this.usersService.create(createUserDto);
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateUserDto: UpdateUserDto) {
    return this.usersService.update(id, updateUserDto);
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return this.usersService.remove(id);
  }

  // Sub-rotas
  @Get(':id/orders')
  getUserOrders(@Param('id') id: string) {
    return this.usersService.getUserOrders(id);
  }
}
```

### Decorators HTTP
```typescript
@Get(':id')              // GET /users/:id
@Post()                  // POST /users
@Put(':id')              // PUT /users/:id
@Delete(':id')           // DELETE /users/:id
@Patch(':id')            // PATCH /users/:id
@Options()               // OPTIONS /users
@Head()                  // HEAD /users
@All()                   // Todos os métodos
```

### Decorators de Parâmetros
```typescript
@Param('id')             // Parâmetro de rota
@Query('search')         // Query string ?search=term
@Body()                  // Corpo da requisição
@Headers('authorization')// Cabeçalhos
@Ip()                    // IP do cliente
@Session()               // Sessão
@HostParam()             // Parâmetro do host
@Req()                   // Request object
@Res()                   // Response object
```

### Versionamento de API
```typescript
@Controller({
  path: 'users',
  version: '1', // Versão global
})
export class UsersControllerV1 {
  @Get()
  @Version('1') // Versão específica
  findAllV1() { /* ... */ }

  @Get()
  @Version('2')
  findAllV2() { /* ... */ }
}

// Configuração no main.ts
const app = await NestFactory.create(AppModule);
app.enableVersioning({
  type: VersioningType.URI, // /v1/users
  // type: VersioningType.HEADER, // X-API-Version: 1
  // type: VersioningType.MEDIA_TYPE, // Accept: application/vnd.api+json;version=1
});
```

---

## Providers e Serviços

### Service Básico
```typescript
// src/users/users.service.ts
import { Injectable, NotFoundException } from '@nestjs/common';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import { User } from './interfaces/user.interface';

@Injectable()
export class UsersService {
  private users: User[] = [];

  findAll(page: number = 1, limit: number = 10): User[] {
    const start = (page - 1) * limit;
    return this.users.slice(start, start + limit);
  }

  findOne(id: string): User {
    const user = this.users.find(user => user.id === id);
    if (!user) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
    return user;
  }

  create(createUserDto: CreateUserDto): User {
    const user: User = {
      id: Date.now().toString(),
      ...createUserDto,
      createdAt: new Date(),
      updatedAt: new Date(),
    };
    this.users.push(user);
    return user;
  }

  update(id: string, updateUserDto: UpdateUserDto): User {
    const userIndex = this.users.findIndex(user => user.id === id);
    
    if (userIndex === -1) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
    
    this.users[userIndex] = {
      ...this.users[userIndex],
      ...updateUserDto,
      updatedAt: new Date(),
    };
    
    return this.users[userIndex];
  }

  remove(id: string): void {
    const userIndex = this.users.findIndex(user => user.id === id);
    
    if (userIndex === -1) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
    
    this.users.splice(userIndex, 1);
  }
}
```

### Custom Providers
```typescript
// providers/constants.ts
export const DATABASE_CONNECTION = 'DATABASE_CONNECTION';
export const USER_REPOSITORY = 'USER_REPOSITORY';

// database/database.providers.ts
import { Connection, createConnection } from 'typeorm';
import { DATABASE_CONNECTION } from '../providers/constants';

export const databaseProviders = [
  {
    provide: DATABASE_CONNECTION,
    useFactory: async (): Promise<Connection> => {
      return await createConnection({
        type: 'postgres',
        host: process.env.DB_HOST,
        port: parseInt(process.env.DB_PORT),
        username: process.env.DB_USERNAME,
        password: process.env.DB_PASSWORD,
        database: process.env.DB_NAME,
        entities: [__dirname + '/../**/*.entity{.ts,.js}'],
        synchronize: process.env.NODE_ENV === 'development',
      });
    },
  },
];

// users/users.providers.ts
import { Connection, Repository } from 'typeorm';
import { User } from './user.entity';
import { DATABASE_CONNECTION, USER_REPOSITORY } from '../providers/constants';

export const userProviders = [
  {
    provide: USER_REPOSITORY,
    useFactory: (connection: Connection) =>
      connection.getRepository(User),
    inject: [DATABASE_CONNECTION],
  },
];
```

### Factory Providers
```typescript
// config/config.factory.ts
import { ConfigService } from '@nestjs/config';

export const createConfig = (env: string) => ({
  provide: 'CONFIG',
  useFactory: (configService: ConfigService) => {
    return {
      environment: env,
      apiUrl: configService.get('API_URL'),
      maxConnections: parseInt(configService.get('MAX_CONNECTIONS', '10')),
    };
  },
  inject: [ConfigService],
});

// Uso no módulo
@Module({
  providers: [
    ConfigService,
    createConfig(process.env.NODE_ENV),
  ],
})
```

### Value e Class Providers
```typescript
@Module({
  providers: [
    // Value Provider
    {
      provide: 'API_KEY',
      useValue: process.env.API_KEY || 'default-key',
    },
    
    // Class Provider (alias)
    {
      provide: 'MailService',
      useClass: process.env.NODE_ENV === 'development' 
        ? DevelopmentMailService 
        : ProductionMailService,
    },
    
    // Factory Provider
    {
      provide: 'DATA_SOURCE',
      useFactory: async () => {
        const dataSource = new DataSource();
        await dataSource.initialize();
        return dataSource;
      },
    },
  ],
})
```

---

## Injeção de Dependências

### Injeção por Construtor
```typescript
@Injectable()
export class UsersService {
  constructor(
    @Inject(USER_REPOSITORY)
    private userRepository: Repository<User>,
    
    @Inject('CACHE_MANAGER')
    private cacheManager: Cache,
    
    private readonly configService: ConfigService,
    
    @Optional()
    private readonly optionalService?: OptionalService,
  ) {}
}
```

### Property-based Injection
```typescript
@Injectable()
export class UsersService {
  @Inject(USER_REPOSITORY)
  private userRepository: Repository<User>;

  @Inject('API_KEY')
  private apiKey: string;
}
```

### Escopos
```typescript
// DEFAULT - Singleton (padrão)
@Injectable({ scope: Scope.DEFAULT })

// REQUEST - Nova instância por requisição
@Injectable({ scope: Scope.REQUEST })
export class UsersService {
  constructor(@Inject(REQUEST) private request: Request) {}
}

// TRANSIENT - Nova instância por provider
@Injectable({ scope: Scope.TRANSIENT })
```

### Resolução de Dependências Circulares
```typescript
// Module A
@Module({
  imports: [forwardRef(() => ModuleB)],
})
export class ModuleA {}

// Module B
@Module({
  imports: [forwardRef(() => ModuleA)],
})
export class ModuleB {}

// Service A
@Injectable()
export class ServiceA {
  constructor(
    @Inject(forwardRef(() => ServiceB))
    private serviceB: ServiceB,
  ) {}
}
```

---

## Pipes - Validação e Transformação

### Pipes Built-in
```typescript
@Get(':id')
findOne(
  @Param('id', ParseIntPipe) id: number, // Converte para número
) {}

@Post()
create(
  @Body(new ValidationPipe()) createUserDto: CreateUserDto,
) {}

@Get('uuid/:id')
findByUuid(
  @Param('id', ParseUUIDPipe) id: string, // Valida UUID
) {}

@Get('boolean/:flag')
getByFlag(
  @Param('flag', ParseBoolPipe) flag: boolean, // Converte para boolean
) {}

@Get('array')
getArray(
  @Query('ids', new ParseArrayPipe({ items: Number, separator: ',' }))
  ids: number[],
) {}

@Get('optional')
findOptional(
  @Query('id', new DefaultValuePipe('default')) id: string,
) {}
```

### Pipes Customizados
```typescript
// validation.pipe.ts
import {
  PipeTransform,
  Injectable,
  ArgumentMetadata,
  BadRequestException,
} from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  async transform(value: any, metadata: ArgumentMetadata) {
    if (!value) {
      throw new BadRequestException('No data submitted');
    }
    
    const { metatype } = metadata;
    
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    
    const object = plainToClass(metatype, value);
    const errors = await validate(object);
    
    if (errors.length > 0) {
      throw new BadRequestException('Validation failed');
    }
    
    return object;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}
```

### Validação com class-validator
```bash
npm install class-validator class-transformer
```

```typescript
// dto/create-user.dto.ts
import {
  IsString,
  IsEmail,
  IsNotEmpty,
  MinLength,
  MaxLength,
  IsOptional,
  IsNumber,
  IsBoolean,
  IsDate,
  IsArray,
  ValidateNested,
  IsEnum,
} from 'class-validator';
import { Type } from 'class-transformer';
import { ApiProperty } from '@nestjs/swagger';

export enum UserRole {
  ADMIN = 'admin',
  USER = 'user',
  GUEST = 'guest',
}

export class CreateAddressDto {
  @IsString()
  @IsNotEmpty()
  street: string;

  @IsString()
  @IsNotEmpty()
  city: string;

  @IsString()
  @IsNotEmpty()
  country: string;
}

export class CreateUserDto {
  @ApiProperty({ example: 'John Doe', description: 'User full name' })
  @IsString()
  @IsNotEmpty()
  @MinLength(3)
  @MaxLength(50)
  name: string;

  @ApiProperty({ example: 'john@example.com', description: 'User email' })
  @IsEmail()
  @IsNotEmpty()
  email: string;

  @IsString()
  @IsNotEmpty()
  @MinLength(6)
  password: string;

  @IsNumber()
  @IsOptional()
  age?: number;

  @IsBoolean()
  @IsOptional()
  isActive?: boolean;

  @IsEnum(UserRole)
  @IsOptional()
  role?: UserRole;

  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => CreateAddressDto)
  @IsOptional()
  addresses?: CreateAddressDto[];

  @IsDate()
  @Type(() => Date)
  @IsOptional()
  birthDate?: Date;
}
```

### Configuração Global
```typescript
// main.ts
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true, // Remove propriedades não decoradas
      forbidNonWhitelisted: true, // Rejeita propriedades não decoradas
      transform: true, // Transforma tipos automaticamente
      transformOptions: {
        enableImplicitConversion: true,
      },
      disableErrorMessages: process.env.NODE_ENV === 'production',
    }),
  );
  
  await app.listen(3000);
}
```

---

## Guards - Autenticação e Autorização

### Guard Básico
```typescript
// guards/auth.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  UnauthorizedException,
} from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    const token = this.extractToken(request);
    
    if (!token) {
      throw new UnauthorizedException('No token provided');
    }
    
    return this.validateToken(token);
  }

  private extractToken(request: Request): string | null {
    const authHeader = request.headers['authorization'];
    if (!authHeader) return null;
    
    const [type, token] = authHeader.split(' ');
    return type === 'Bearer' ? token : null;
  }

  private validateToken(token: string): boolean {
    // Implemente sua lógica de validação
    return token === 'valid-token';
  }
}
```

### Guard com JWT
```typescript
// guards/jwt-auth.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  handleRequest(err, user, info) {
    if (err || !user) {
      throw err || new UnauthorizedException('Invalid token');
    }
    return user;
  }
}

// guards/roles.guard.ts
import {
  Injectable,
  CanActivate,
  ExecutionContext,
  ForbiddenException,
} from '@nestjs/common';
import { Reflector } from '@nestjs/core';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.get<string[]>(
      'roles',
      context.getHandler(),
    );
    
    if (!requiredRoles) {
      return true;
    }
    
    const { user } = context.switchToHttp().getRequest();
    
    if (!user || !user.roles) {
      throw new ForbiddenException('User has no roles');
    }
    
    const hasRole = requiredRoles.some(role => user.roles.includes(role));
    
    if (!hasRole) {
      throw new ForbiddenException(
        `User needs one of these roles: ${requiredRoles.join(', ')}`,
      );
    }
    
    return true;
  }
}
```

### Decorators Personalizados
```typescript
// decorators/roles.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const ROLES_KEY = 'roles';
export const Roles = (...roles: string[]) => SetMetadata(ROLES_KEY, roles);

// decorators/public.decorator.ts
import { SetMetadata } from '@nestjs/common';

export const IS_PUBLIC_KEY = 'isPublic';
export const Public = () => SetMetadata(IS_PUBLIC_KEY, true);
```

### Uso dos Guards
```typescript
@Controller('users')
@UseGuards(JwtAuthGuard, RolesGuard)
export class UsersController {
  @Get()
  @Roles('admin', 'moderator')
  findAll() {
    return this.usersService.findAll();
  }

  @Get('profile')
  @Public() // Rota pública
  getProfile() {
    return this.usersService.getPublicProfile();
  }

  @Get(':id')
  @Roles('admin')
  findOne(@Param('id') id: string) {
    return this.usersService.findOne(id);
  }
}
```

---

## Interceptadores

### Interceptor Básico
```typescript
// interceptors/logging.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  Logger,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  private readonly logger = new Logger(LoggingInterceptor.name);

  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const request = context.switchToHttp().getRequest();
    const { method, url, body, params, query } = request;
    const now = Date.now();

    this.logger.log(
      `Request: ${method} ${url} | Body: ${JSON.stringify(body)} | Params: ${JSON.stringify(params)} | Query: ${JSON.stringify(query)}`,
    );

    return next.handle().pipe(
      tap((data) => {
        const response = context.switchToHttp().getResponse();
        const statusCode = response.statusCode;
        const responseTime = Date.now() - now;

        this.logger.log(
          `Response: ${method} ${url} ${statusCode} | Time: ${responseTime}ms`,
        );
      }),
    );
  }
}
```

### Transformação de Response
```typescript
// interceptors/transform.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
  success: boolean;
  data: T;
  message?: string;
  timestamp: string;
}

@Injectable()
export class TransformInterceptor<T>
  implements NestInterceptor<T, Response<T>>
{
  intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Observable<Response<T>> {
    return next.handle().pipe(
      map((data) => ({
        success: true,
        data,
        timestamp: new Date().toISOString(),
      })),
    );
  }
}
```

### Tratamento de Timeout
```typescript
// interceptors/timeout.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
  RequestTimeoutException,
} from '@nestjs/common';
import { Observable, throwError, TimeoutError } from 'rxjs';
import { catchError, timeout } from 'rxjs/operators';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      timeout(5000), // Timeout de 5 segundos
      catchError((err) => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException());
        }
        return throwError(() => err);
      }),
    );
  }
}
```

### Cache Interceptor
```typescript
// interceptors/cache.interceptor.ts
import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  CallHandler,
} from '@nestjs/common';
import { Observable, of } from 'rxjs';
import { tap } from 'rxjs/operators';
import { CACHE_MANAGER } from '@nestjs/cache-manager';
import { Inject } from '@nestjs/common';

@Injectable()
export class CacheInterceptor implements NestInterceptor {
  constructor(@Inject(CACHE_MANAGER) private cacheManager: Cache) {}

  async intercept(
    context: ExecutionContext,
    next: CallHandler,
  ): Promise<Observable<any>> {
    const request = context.switchToHttp().getRequest();
    const key = this.generateCacheKey(request);

    const cachedData = await this.cacheManager.get(key);
    
    if (cachedData) {
      return of(cachedData);
    }

    return next.handle().pipe(
      tap(async (data) => {
        await this.cacheManager.set(key, data, 60 * 1000); // Cache por 1 minuto
      }),
    );
  }

  private generateCacheKey(request: any): string {
    const { method, url, params, query } = request;
    return `${method}:${url}:${JSON.stringify(params)}:${JSON.stringify(query)}`;
  }
}
```

### Uso dos Interceptors
```typescript
// Controller level
@Controller('users')
@UseInterceptors(TransformInterceptor, LoggingInterceptor)
export class UsersController {}

// Method level
@Get()
@UseInterceptors(CacheInterceptor)
findAll() {}

// Global
const app = await NestFactory.create(AppModule);
app.useGlobalInterceptors(
  new TransformInterceptor(),
  new LoggingInterceptor(),
);
```

---

## Middleware

### Middleware Básico
```typescript
// middleware/logger.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`[${new Date().toISOString()}] ${req.method} ${req.url}`);
    next();
  }
}

// middleware/auth.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';
import * as jwt from 'jsonwebtoken';

@Injectable()
export class AuthMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    const token = req.headers.authorization?.split(' ')[1];
    
    if (!token) {
      return res.status(401).json({ message: 'No token provided' });
    }
    
    try {
      const decoded = jwt.verify(token, process.env.JWT_SECRET);
      req['user'] = decoded;
      next();
    } catch (error) {
      return res.status(401).json({ message: 'Invalid token' });
    }
  }
}
```

### Middleware Funcional
```typescript
// middleware/cors.middleware.ts
import { Request, Response, NextFunction } from 'express';

export function corsMiddleware(
  req: Request,
  res: Response,
  next: NextFunction,
) {
  res.header('Access-Control-Allow-Origin', '*');
  res.header('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE, OPTIONS');
  res.header('Access-Control-Allow-Headers', 'Content-Type, Authorization');
  
  if (req.method === 'OPTIONS') {
    return res.sendStatus(200);
  }
  
  next();
}

// middleware/rate-limit.middleware.ts
import { Request, Response, NextFunction } from 'express';

const requestCounts = new Map<string, number>();

export function rateLimitMiddleware(
  req: Request,
  res: Response,
  next: NextFunction,
) {
  const ip = req.ip;
  const currentCount = requestCounts.get(ip) || 0;
  
  if (currentCount >= 100) {
    return res.status(429).json({ message: 'Too many requests' });
  }
  
  requestCounts.set(ip, currentCount + 1);
  
  // Reset counter após 1 minuto
  setTimeout(() => {
    requestCounts.delete(ip);
  }, 60000);
  
  next();
}
```

### Configuração no Módulo
```typescript
// app.module.ts
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './middleware/logger.middleware';
import { AuthMiddleware } from './middleware/auth.middleware';

@Module({
  // ...
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('*'); // Todas as rotas

    consumer
      .apply(AuthMiddleware)
      .exclude(
        { path: 'auth/login', method: RequestMethod.POST },
        { path: 'auth/register', method: RequestMethod.POST },
      )
      .forRoutes(
        { path: 'users', method: RequestMethod.ALL },
        { path: 'products', method: RequestMethod.ALL },
      );

    // Múltiplos middlewares
    consumer
      .apply(corsMiddleware, rateLimitMiddleware)
      .forRoutes('*');
  }
}
```

### Global Middleware
```typescript
// main.ts
import * as morgan from 'morgan';
import * as helmet from 'helmet';
import * as compression from 'compression';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  app.use(helmet());
  app.use(compression());
  app.use(morgan('combined'));
  
  await app.listen(3000);
}
```

---

## Filters - Tratamento de Exceções

### Filter Básico
```typescript
// filters/http-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  Logger,
} from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  private readonly logger = new Logger(HttpExceptionFilter.name);

  catch(exception: HttpException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();
    const exceptionResponse = exception.getResponse();

    const errorResponse = {
      success: false,
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      method: request.method,
      message:
        typeof exceptionResponse === 'string'
          ? exceptionResponse
          : (exceptionResponse as any).message,
      error:
        typeof exceptionResponse === 'string'
          ? exceptionResponse
          : (exceptionResponse as any).error,
    };

    this.logger.error(
      `${request.method} ${request.url} ${status}`,
      exception.stack,
    );

    response.status(status).json(errorResponse);
  }
}
```

### Filter para Exceções Customizadas
```typescript
// exceptions/business.exception.ts
export class BusinessException extends Error {
  constructor(
    public readonly code: string,
    public readonly message: string,
    public readonly statusCode: number = 400,
  ) {
    super(message);
  }
}

// filters/business-exception.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
} from '@nestjs/common';
import { BusinessException } from '../exceptions/business.exception';
import { Request, Response } from 'express';

@Catch(BusinessException)
export class BusinessExceptionFilter implements ExceptionFilter {
  catch(exception: BusinessException, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    response.status(exception.statusCode).json({
      success: false,
      errorCode: exception.code,
      message: exception.message,
      timestamp: new Date().toISOString(),
      path: request.url,
    });
  }
}
```

### Filter Global
```typescript
// filters/all-exceptions.filter.ts
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
  Logger,
} from '@nestjs/common';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger(AllExceptionsFilter.name);

  catch(exception: unknown, host: ArgumentsHost) {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse();
    const request = ctx.getRequest();

    let status = HttpStatus.INTERNAL_SERVER_ERROR;
    let message = 'Internal server error';
    let errorCode = 'INTERNAL_ERROR';

    if (exception instanceof HttpException) {
      status = exception.getStatus();
      const exceptionResponse = exception.getResponse();
      message =
        typeof exceptionResponse === 'string'
          ? exceptionResponse
          : (exceptionResponse as any).message || exception.message;
      errorCode = (exceptionResponse as any).error || 'HTTP_ERROR';
    } else if (exception instanceof Error) {
      message = exception.message;
      errorCode = exception.name;
    }

    // Log do erro
    this.logger.error(
      `${request.method} ${request.url} ${status} - ${message}`,
      exception instanceof Error ? exception.stack : '',
    );

    response.status(status).json({
      success: false,
      statusCode: status,
      errorCode,
      message,
      timestamp: new Date().toISOString(),
      path: request.url,
    });
  }
}
```

### Uso dos Filters
```typescript
// Controller level
@Controller('users')
@UseFilters(HttpExceptionFilter, BusinessExceptionFilter)
export class UsersController {
  @Get(':id')
  @UseFilters(AllExceptionsFilter)
  findOne(@Param('id') id: string) {
    if (id === '0') {
      throw new BusinessException('USER_NOT_FOUND', 'User not found', 404);
    }
    return this.usersService.findOne(id);
  }
}

// Method level
@Post()
@UseFilters(new HttpExceptionFilter())
create(@Body() createUserDto: CreateUserDto) {}

// Global
const app = await NestFactory.create(AppModule);
app.useGlobalFilters(new AllExceptionsFilter());
```

---

## Configuração e Variáveis de Ambiente

### Configuração com @nestjs/config
```bash
npm install @nestjs/config
```

```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config';
import configuration from './config/configuration';
import validationSchema from './config/validation';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true,
      load: [configuration],
      validationSchema,
      envFilePath: [`.env.${process.env.NODE_ENV}`, '.env'],
      ignoreEnvFile: process.env.NODE_ENV === 'production',
    }),
  ],
})
export class AppModule {}
```

### Arquivos de Configuração
```typescript
// config/configuration.ts
export default () => ({
  environment: process.env.NODE_ENV || 'development',
  port: parseInt(process.env.PORT, 10) || 3000,
  database: {
    host: process.env.DB_HOST,
    port: parseInt(process.env.DB_PORT, 10) || 5432,
    username: process.env.DB_USERNAME,
    password: process.env.DB_PASSWORD,
    name: process.env.DB_NAME,
  },
  jwt: {
    secret: process.env.JWT_SECRET,
    expiresIn: process.env.JWT_EXPIRES_IN || '1d',
  },
  redis: {
    host: process.env.REDIS_HOST,
    port: parseInt(process.env.REDIS_PORT, 10) || 6379,
  },
  email: {
    host: process.env.EMAIL_HOST,
    port: parseInt(process.env.EMAIL_PORT, 10) || 587,
    user: process.env.EMAIL_USER,
    pass: process.env.EMAIL_PASS,
  },
});

// config/validation.ts
import * as Joi from 'joi';

export default Joi.object({
  NODE_ENV: Joi.string()
    .valid('development', 'production', 'test')
    .default('development'),
  
  PORT: Joi.number().default(3000),
  
  DB_HOST: Joi.string().required(),
  DB_PORT: Joi.number().default(5432),
  DB_USERNAME: Joi.string().required(),
  DB_PASSWORD: Joi.string().required(),
  DB_NAME: Joi.string().required(),
  
  JWT_SECRET: Joi.string().required(),
  JWT_EXPIRES_IN: Joi.string().default('1d'),
  
  REDIS_HOST: Joi.string().required(),
  REDIS_PORT: Joi.number().default(6379),
});
```

### Config Service
```typescript
// config/config.service.ts
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class AppConfigService {
  constructor(private configService: ConfigService) {}

  get isDevelopment(): boolean {
    return this.nodeEnv === 'development';
  }

  get isProduction(): boolean {
    return this.nodeEnv === 'production';
  }

  get nodeEnv(): string {
    return this.configService.get<string>('environment');
  }

  get port(): number {
    return this.configService.get<number>('port');
  }

  get databaseConfig() {
    return {
      host: this.configService.get<string>('database.host'),
      port: this.configService.get<number>('database.port'),
      username: this.configService.get<string>('database.username'),
      password: this.configService.get<string>('database.password'),
      name: this.configService.get<string>('database.name'),
    };
  }

  get jwtConfig() {
    return {
      secret: this.configService.get<string>('jwt.secret'),
      expiresIn: this.configService.get<string>('jwt.expiresIn'),
    };
  }
}

// Uso em serviços
@Injectable()
export class AuthService {
  constructor(
    private configService: ConfigService,
    private appConfigService: AppConfigService,
  ) {
    const jwtSecret = this.configService.get<string>('jwt.secret');
    const dbHost = this.appConfigService.databaseConfig.host;
  }
}
```

### Configuração TypeSafe
```typescript
// config/config.types.ts
export interface DatabaseConfig {
  host: string;
  port: number;
  username: string;
  password: string;
  name: string;
}

export interface JwtConfig {
  secret: string;
  expiresIn: string;
}

export interface AppConfig {
  environment: string;
  port: number;
  database: DatabaseConfig;
  jwt: JwtConfig;
}

// config/config.factory.ts
import { ConfigFactory, ConfigObject } from '@nestjs/config';
import { AppConfig } from './config.types';

export const configFactory: ConfigFactory<ConfigObject> = () => ({
  environment: process.env.NODE_ENV,
  port: parseInt(process.env.PORT, 10),
  database: {
    host: process.env.DB_HOST,
    port: parseInt(process.env.DB_PORT, 10),
    username: process.env.DB_USERNAME,
    password: process.env.DB_PASSWORD,
    name: process.env.DB_NAME,
  },
  jwt: {
    secret: process.env.JWT_SECRET,
    expiresIn: process.env.JWT_EXPIRES_IN,
  },
} as AppConfig);
```

---

## Banco de Dados e ORM

### TypeORM (PostgreSQL)
```bash
npm install @nestjs/typeorm typeorm pg
```

```typescript
// app.module.ts
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'postgres',
      host: process.env.DB_HOST,
      port: parseInt(process.env.DB_PORT, 10),
      username: process.env.DB_USERNAME,
      password: process.env.DB_PASSWORD,
      database: process.env.DB_NAME,
      entities: [__dirname + '/**/*.entity{.ts,.js}'],
      synchronize: process.env.NODE_ENV === 'development', // Cuidado em produção!
      logging: process.env.NODE_ENV === 'development',
      migrations: [__dirname + '/migrations/*{.ts,.js}'],
      cli: {
        migrationsDir: 'src/migrations',
      },
    }),
  ],
})
```

### Entidades
```typescript
// entities/user.entity.ts
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  CreateDateColumn,
  UpdateDateColumn,
  OneToMany,
  ManyToOne,
  ManyToMany,
  JoinTable,
  Index,
  Unique,
} from 'typeorm';
import { Exclude } from 'class-transformer';
import { Role } from './role.entity';
import { Order } from './order.entity';

@Entity('users')
@Unique(['email'])
@Index('IDX_USER_EMAIL', ['email'])
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ length: 100 })
  name: string;

  @Column({ length: 100 })
  email: string;

  @Column()
  @Exclude() // Não incluir nas respostas
  password: string;

  @Column({ default: true })
  isActive: boolean;

  @Column({ nullable: true })
  avatarUrl: string;

  @Column({ type: 'date', nullable: true })
  birthDate: Date;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  @ManyToOne(() => Role, (role) => role.users, { eager: true })
  role: Role;

  @OneToMany(() => Order, (order) => order.user)
  orders: Order[];

  @ManyToMany(() => User)
  @JoinTable({
    name: 'user_friends',
    joinColumn: { name: 'user_id', referencedColumnName: 'id' },
    inverseJoinColumn: { name: 'friend_id', referencedColumnName: 'id' },
  })
  friends: User[];
}

// entities/order.entity.ts
import {
  Entity,
  Column,
  PrimaryGeneratedColumn,
  ManyToOne,
  OneToMany,
} from 'typeorm';
import { User } from './user.entity';
import { OrderItem } from './order-item.entity';

export enum OrderStatus {
  PENDING = 'pending',
  PROCESSING = 'processing',
  SHIPPED = 'shipped',
  DELIVERED = 'delivered',
  CANCELLED = 'cancelled',
}

@Entity('orders')
export class Order {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ type: 'decimal', precision: 10, scale: 2 })
  total: number;

  @Column({
    type: 'enum',
    enum: OrderStatus,
    default: OrderStatus.PENDING,
  })
  status: OrderStatus;

  @Column({ type: 'json', nullable: true })
  metadata: any;

  @ManyToOne(() => User, (user) => user.orders)
  user: User;

  @OneToMany(() => OrderItem, (orderItem) => orderItem.order, { cascade: true })
  items: OrderItem[];
}
```

### Repositórios Customizados
```typescript
// repositories/user.repository.ts
import { EntityRepository, Repository } from 'typeorm';
import { User } from '../entities/user.entity';

@EntityRepository(User)
export class UserRepository extends Repository<User> {
  async findActiveUsers(): Promise<User[]> {
    return this.createQueryBuilder('user')
      .where('user.isActive = :isActive', { isActive: true })
      .orderBy('user.createdAt', 'DESC')
      .getMany();
  }

  async findByEmail(email: string): Promise<User | undefined> {
    return this.createQueryBuilder('user')
      .leftJoinAndSelect('user.role', 'role')
      .where('user.email = :email', { email })
      .getOne();
  }

  async searchUsers(searchTerm: string): Promise<User[]> {
    return this.createQueryBuilder('user')
      .where('user.name LIKE :searchTerm', { searchTerm: `%${searchTerm}%` })
      .orWhere('user.email LIKE :searchTerm', { searchTerm: `%${searchTerm}%` })
      .getMany();
  }
}

// providers/user.providers.ts
import { UserRepository } from './repositories/user.repository';

export const userProviders = [
  {
    provide: 'USER_REPOSITORY',
    useFactory: (connection: Connection) =>
      connection.getCustomRepository(UserRepository),
    inject: ['DATABASE_CONNECTION'],
  },
];
```

### Serviço com TypeORM
```typescript
// services/user.service.ts
import { Injectable, Inject } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository, Like, Between, In, Not } from 'typeorm';
import { User } from '../entities/user.entity';
import { CreateUserDto } from '../dto/create-user.dto';
import { UpdateUserDto } from '../dto/update-user.dto';

@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private userRepository: Repository<User>,
  ) {}

  async findAll(
    page: number = 1,
    limit: number = 10,
    filters?: any,
  ): Promise<{ data: User[]; total: number }> {
    const [data, total] = await this.userRepository.findAndCount({
      where: this.buildWhereClause(filters),
      relations: ['role'],
      order: { createdAt: 'DESC' },
      skip: (page - 1) * limit,
      take: limit,
    });

    return { data, total };
  }

  async findOne(id: string): Promise<User> {
    const user = await this.userRepository.findOne({
      where: { id },
      relations: ['role', 'orders'],
    });

    if (!user) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }

    return user;
  }

  async create(createUserDto: CreateUserDto): Promise<User> {
    const user = this.userRepository.create(createUserDto);
    return await this.userRepository.save(user);
  }

  async update(id: string, updateUserDto: UpdateUserDto): Promise<User> {
    const user = await this.findOne(id);
    Object.assign(user, updateUserDto);
    return await this.userRepository.save(user);
  }

  async remove(id: string): Promise<void> {
    const result = await this.userRepository.delete(id);
    
    if (result.affected === 0) {
      throw new NotFoundException(`User with ID ${id} not found`);
    }
  }

  private buildWhereClause(filters: any) {
    const where: any = {};

    if (filters?.name) {
      where.name = Like(`%${filters.name}%`);
    }

    if (filters?.email) {
      where.email = Like(`%${filters.email}%`);
    }

    if (filters?.isActive !== undefined) {
      where.isActive = filters.isActive;
    }

    if (filters?.createdFrom && filters?.createdTo) {
      where.createdAt = Between(
        new Date(filters.createdFrom),
        new Date(filters.createdTo),
      );
    }

    if (filters?.roleIds) {
      where.role = In(filters.roleIds);
    }

    return where;
  }
}
```

### Migrations
```bash
# Gerar migration
npx typeorm migration:generate -n CreateUsersTable

# Executar migrations
npx typeorm migration:run

# Reverter migration
npx typeorm migration:revert
```

```typescript
// migrations/1234567890-CreateUsersTable.ts
import { MigrationInterface, QueryRunner, Table } from 'typeorm';

export class CreateUsersTable1234567890 implements MigrationInterface {
  public async up(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.createTable(
      new Table({
        name: 'users',
        columns: [
          {
            name: 'id',
            type: 'uuid',
            isPrimary: true,
            generationStrategy: 'uuid',
            default: 'uuid_generate_v4()',
          },
          {
            name: 'name',
            type: 'varchar',
            length: '100',
          },
          {
            name: 'email',
            type: 'varchar',
            length: '100',
            isUnique: true,
          },
          {
            name: 'password',
            type: 'varchar',
          },
          {
            name: 'is_active',
            type: 'boolean',
            default: true,
          },
          {
            name: 'created_at',
            type: 'timestamp',
            default: 'now()',
          },
          {
            name: 'updated_at',
            type: 'timestamp',
            default: 'now()',
          },
        ],
      }),
      true,
    );
  }

  public async down(queryRunner: QueryRunner): Promise<void> {
    await queryRunner.dropTable('users');
  }
}
```

---

## Autenticação JWT

### Configuração do Passport
```bash
npm install @nestjs/passport passport passport-local
npm install @nestjs/jwt passport-jwt
npm install bcrypt
npm install @types/passport-local @types/passport-jwt
```

```typescript
// auth/auth.module.ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { PassportModule } from '@nestjs/passport';
import { ConfigModule, ConfigService } from '@nestjs/config';
import { AuthService } from './auth.service';
import { AuthController } from './auth.controller';
import { UsersModule } from '../users/users.module';
import { JwtStrategy } from './strategies/jwt.strategy';
import { LocalStrategy } from './strategies/local.strategy';

@Module({
  imports: [
    UsersModule,
    PassportModule,
    JwtModule.registerAsync({
      imports: [ConfigModule],
      useFactory: async (configService: ConfigService) => ({
        secret: configService.get<string>('JWT_SECRET'),
        signOptions: {
          expiresIn: configService.get<string>('JWT_EXPIRES_IN', '1d'),
        },
      }),
      inject: [ConfigService],
    }),
  ],
  controllers: [AuthController],
  providers: [AuthService, LocalStrategy, JwtStrategy],
  exports: [AuthService],
})
export class AuthModule {}
```

### Strategies
```typescript
// auth/strategies/local.strategy.ts
import { Strategy } from 'passport-local';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { AuthService } from '../auth.service';

@Injectable()
export class LocalStrategy extends PassportStrategy(Strategy) {
  constructor(private authService: AuthService) {
    super({
      usernameField: 'email',
      passwordField: 'password',
    });
  }

  async validate(email: string, password: string): Promise<any> {
    const user = await this.authService.validateUser(email, password);
    
    if (!user) {
      throw new UnauthorizedException('Invalid credentials');
    }
    
    return user;
  }
}

// auth/strategies/jwt.strategy.ts
import { ExtractJwt, Strategy } from 'passport-jwt';
import { PassportStrategy } from '@nestjs/passport';
import { Injectable } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';

@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor(private configService: ConfigService) {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      ignoreExpiration: false,
      secretOrKey: configService.get<string>('JWT_SECRET'),
    });
  }

  async validate(payload: any) {
    return {
      userId: payload.sub,
      email: payload.email,
      roles: payload.roles,
    };
  }
}
```

### Auth Service
```typescript
// auth/auth.service.ts
import { Injectable } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { UsersService } from '../users/users.service';
import * as bcrypt from 'bcrypt';

@Injectable()
export class AuthService {
  constructor(
    private usersService: UsersService,
    private jwtService: JwtService,
  ) {}

  async validateUser(email: string, password: string): Promise<any> {
    const user = await this.usersService.findByEmail(email);
    
    if (user && await this.comparePasswords(password, user.password)) {
      const { password, ...result } = user;
      return result;
    }
    
    return null;
  }

  async login(user: any) {
    const payload = {
      email: user.email,
      sub: user.id,
      roles: user.roles,
    };
    
    return {
      access_token: this.jwtService.sign(payload),
      refresh_token: this.jwtService.sign(payload, { expiresIn: '7d' }),
      user: {
        id: user.id,
        email: user.email,
        name: user.name,
        roles: user.roles,
      },
    };
  }

  async refreshToken(token: string) {
    try {
      const payload = this.jwtService.verify(token);
      const user = await this.usersService.findOne(payload.sub);
      
      if (!user) {
        throw new Error('User not found');
      }
      
      const newPayload = {
        email: user.email,
        sub: user.id,
        roles: user.roles,
      };
      
      return {
        access_token: this.jwtService.sign(newPayload),
      };
    } catch (error) {
      throw new UnauthorizedException('Invalid refresh token');
    }
  }

  async hashPassword(password: string): Promise<string> {
    const saltRounds = 10;
    return await bcrypt.hash(password, saltRounds);
  }

  async comparePasswords(
    plainPassword: string,
    hashedPassword: string,
  ): Promise<boolean> {
    return await bcrypt.compare(plainPassword, hashedPassword);
  }
}
```

### Auth Controller
```typescript
// auth/auth.controller.ts
import {
  Controller,
  Post,
  Body,
  UseGuards,
  Request,
  Get,
} from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { AuthService } from './auth.service';
import { LoginDto } from './dto/login.dto';
import { RegisterDto } from './dto/register.dto';
import { RefreshTokenDto } from './dto/refresh-token.dto';

@Controller('auth')
export class AuthController {
  constructor(private authService: AuthService) {}

  @Post('register')
  async register(@Body() registerDto: RegisterDto) {
    const hashedPassword = await this.authService.hashPassword(
      registerDto.password,
    );
    
    // Crie o usuário aqui
    const user = await this.usersService.create({
      ...registerDto,
      password: hashedPassword,
    });
    
    return this.authService.login(user);
  }

  @UseGuards(AuthGuard('local'))
  @Post('login')
  async login(@Body() loginDto: LoginDto, @Request() req) {
    return this.authService.login(req.user);
  }

  @Post('refresh')
  async refresh(@Body() refreshTokenDto: RefreshTokenDto) {
    return this.authService.refreshToken(refreshTokenDto.refresh_token);
  }

  @UseGuards(AuthGuard('jwt'))
  @Get('profile')
  getProfile(@Request() req) {
    return req.user;
  }

  @UseGuards(AuthGuard('jwt'))
  @Post('logout')
  logout() {
    // Implementar lógica de logout (blacklist token)
    return { message: 'Logged out successfully' };
  }
}
```

### Guards de Autenticação
```typescript
// auth/guards/jwt-auth.guard.ts
import { Injectable, ExecutionContext } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';
import { Reflector } from '@nestjs/core';
import { IS_PUBLIC_KEY } from '../../decorators/public.decorator';

@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  constructor(private reflector: Reflector) {
    super();
  }

  canActivate(context: ExecutionContext) {
    const isPublic = this.reflector.getAllAndOverride<boolean>(IS_PUBLIC_KEY, [
      context.getHandler(),
      context.getClass(),
    ]);

    if (isPublic) {
      return true;
    }

    return super.canActivate(context);
  }
}

// auth/guards/local-auth.guard.ts
import { Injectable } from '@nestjs/common';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class LocalAuthGuard extends AuthGuard('local') {}
```

---

## WebSockets

### Configuração do WebSocket
```bash
npm install @nestjs/websockets @nestjs/platform-socket.io
```

```typescript
// websocket/websocket.module.ts
import { Module } from '@nestjs/common';
import { WebSocketGateway } from './websocket.gateway';

@Module({
  providers: [WebSocketGateway],
})
export class WebSocketModule {}

// main.ts
import { IoAdapter } from '@nestjs/platform-socket.io';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  // Configurar WebSocket
  app.useWebSocketAdapter(new IoAdapter(app));
  
  await app.listen(3000);
}
```

### WebSocket Gateway
```typescript
// websocket/websocket.gateway.ts
import {
  WebSocketGateway,
  WebSocketServer,
  SubscribeMessage,
  OnGatewayInit,
  OnGatewayConnection,
  OnGatewayDisconnect,
  ConnectedSocket,
  MessageBody,
} from '@nestjs/websockets';
import { Server, Socket } from 'socket.io';
import { Logger, UseGuards } from '@nestjs/common';
import { JwtWsGuard } from './guards/jwt-ws.guard';

@WebSocketGateway({
  namespace: 'chat',
  cors: {
    origin: process.env.CLIENT_URL,
    credentials: true,
  },
})
export class WebSocketGateway
  implements OnGatewayInit, OnGatewayConnection, OnGatewayDisconnect
{
  @WebSocketServer() server: Server;
  private logger: Logger = new Logger('WebSocketGateway');

  afterInit(server: Server) {
    this.logger.log('WebSocket Gateway initialized');
  }

  async handleConnection(client: Socket) {
    try {
      const token = client.handshake.auth.token;
      
      if (!token) {
        client.disconnect();
        return;
      }

      // Validar token JWT
      const payload = await this.validateToken(token);
      client.data.user = payload;
      
      this.logger.log(`Client connected: ${client.id}`);
      this.server.emit('user-connected', { userId: payload.sub });
    } catch (error) {
      client.disconnect();
    }
  }

  handleDisconnect(client: Socket) {
    this.logger.log(`Client disconnected: ${client.id}`);
    if (client.data.user) {
      this.server.emit('user-disconnected', { userId: client.data.user.sub });
    }
  }

  @SubscribeMessage('join-room')
  handleJoinRoom(
    @ConnectedSocket() client: Socket,
    @MessageBody() roomId: string,
  ) {
    client.join(roomId);
    this.logger.log(`Client ${client.id} joined room ${roomId}`);
    client.emit('joined-room', roomId);
  }

  @SubscribeMessage('leave-room')
  handleLeaveRoom(
    @ConnectedSocket() client: Socket,
    @MessageBody() roomId: string,
  ) {
    client.leave(roomId);
    this.logger.log(`Client ${client.id} left room ${roomId}`);
    client.emit('left-room', roomId);
  }

  @UseGuards(JwtWsGuard)
  @SubscribeMessage('send-message')
  handleMessage(
    @ConnectedSocket() client: Socket,
    @MessageBody() data: { roomId: string; message: string },
  ) {
    const { roomId, message } = data;
    
    this.server.to(roomId).emit('new-message', {
      userId: client.data.user.sub,
      message,
      timestamp: new Date().toISOString(),
    });
  }

  @SubscribeMessage('typing')
  handleTyping(
    @ConnectedSocket() client: Socket,
    @MessageBody() data: { roomId: string; isTyping: boolean },
  ) {
    const { roomId, isTyping } = data;
    
    client.to(roomId).emit('user-typing', {
      userId: client.data.user.sub,
      isTyping,
    });
  }

  private async validateToken(token: string): Promise<any> {
    // Implementar validação JWT
    return { sub: 'user-id', email: 'user@example.com' };
  }
}
```

### Guard para WebSocket
```typescript
// websocket/guards/jwt-ws.guard.ts
import { CanActivate, ExecutionContext, Injectable } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class JwtWsGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const client = context.switchToWs().getClient();
    return !!client.data.user;
  }
}
```

### Service para WebSocket
```typescript
// websocket/websocket.service.ts
import { Injectable } from '@nestjs/common';
import { WebSocketGateway, WebSocketServer } from '@nestjs/websockets';
import { Server } from 'socket.io';

@WebSocketGateway()
@Injectable()
export class WebSocketService {
  @WebSocketServer()
  server: Server;

  emitToUser(userId: string, event: string, data: any) {
    this.server.emit(event, { userId, ...data });
  }

  emitToRoom(roomId: string, event: string, data: any) {
    this.server.to(roomId).emit(event, data);
  }

  emitToAll(event: string, data: any) {
    this.server.emit(event, data);
  }

  getUserRooms(userId: string): string[] {
    // Implementar lógica para obter salas do usuário
    return [];
  }
}
```

---

## GraphQL

### Configuração do GraphQL
```bash
npm install @nestjs/graphql @nestjs/apollo graphql apollo-server-express
```

```typescript
// app.module.ts
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: 'schema.gql',
      playground: process.env.NODE_ENV === 'development',
      introspection: process.env.NODE_ENV === 'development',
      context: ({ req }) => ({ req }),
      formatError: (error) => {
        const { message, path } = error;
        return {
          message,
          path,
          timestamp: new Date().toISOString(),
        };
      },
    }),
  ],
})
export class AppModule {}
```

### Resolvers
```typescript
// users/users.resolver.ts
import {
  Resolver,
  Query,
  Mutation,
  Args,
  Context,
  Int,
  Subscription,
} from '@nestjs/graphql';
import { UseGuards } from '@nestjs/common';
import { UsersService } from './users.service';
import { User } from './entities/user.entity';
import { CreateUserInput } from './dto/create-user.input';
import { UpdateUserInput } from './dto/update-user.input';
import { GqlAuthGuard } from '../auth/guards/gql-auth.guard';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Resolver(() => User)
export class UsersResolver {
  constructor(private readonly usersService: UsersService) {}

  @Query(() => [User], { name: 'users' })
  @UseGuards(GqlAuthGuard)
  findAll(
    @Args('page', { type: () => Int, defaultValue: 1 }) page: number,
    @Args('limit', { type: () => Int, defaultValue: 10 }) limit: number,
  ) {
    return this.usersService.findAll(page, limit);
  }

  @Query(() => User, { name: 'user' })
  @UseGuards(GqlAuthGuard)
  findOne(@Args('id', { type: () => String }) id: string) {
    return this.usersService.findOne(id);
  }

  @Mutation(() => User)
  createUser(@Args('createUserInput') createUserInput: CreateUserInput) {
    const user = this.usersService.create(createUserInput);
    pubSub.publish('userCreated', { userCreated: user });
    return user;
  }

  @Mutation(() => User)
  @UseGuards(GqlAuthGuard)
  updateUser(
    @Args('id') id: string,
    @Args('updateUserInput') updateUserInput: UpdateUserInput,
  ) {
    return this.usersService.update(id, updateUserInput);
  }

  @Mutation(() => Boolean)
  @UseGuards(GqlAuthGuard)
  async removeUser(@Args('id') id: string): Promise<boolean> {
    await this.usersService.remove(id);
    return true;
  }

  @Subscription(() => User)
  userCreated() {
    return pubSub.asyncIterator('userCreated');
  }
}
```

### Entidades GraphQL
```typescript
// users/entities/user.entity.ts
import { ObjectType, Field, ID, Int } from '@nestjs/graphql';

@ObjectType()
export class User {
  @Field(() => ID)
  id: string;

  @Field()
  name: string;

  @Field()
  email: string;

  @Field(() => Boolean, { defaultValue: true })
  isActive: boolean;

  @Field(() => Date)
  createdAt: Date;

  @Field(() => Date)
  updatedAt: Date;

  @Field(() => [Order], { nullable: true })
  orders?: Order[];
}

// DTOs para GraphQL
import { InputType, Field } from '@nestjs/graphql';
import { IsEmail, IsNotEmpty, MinLength } from 'class-validator';

@InputType()
export class CreateUserInput {
  @Field()
  @IsNotEmpty()
  @MinLength(3)
  name: string;

  @Field()
  @IsEmail()
  email: string;

  @Field()
  @MinLength(6)
  password: string;
}
```

### Guards GraphQL
```typescript
// auth/guards/gql-auth.guard.ts
import { ExecutionContext, Injectable } from '@nestjs/common';
import { GqlExecutionContext } from '@nestjs/graphql';
import { AuthGuard } from '@nestjs/passport';

@Injectable()
export class GqlAuthGuard extends AuthGuard('jwt') {
  getRequest(context: ExecutionContext) {
    const ctx = GqlExecutionContext.create(context);
    return ctx.getContext().req;
  }
}
```

---

## Microserviços

### Configuração de Microserviços
```bash
npm install @nestjs/microservices
```

```typescript
// main.ts (Microserviço)
import { NestFactory } from '@nestjs/core';
import { MicroserviceOptions, Transport } from '@nestjs/microservices';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(
    AppModule,
    {
      transport: Transport.TCP,
      options: {
        host: '0.0.0.0',
        port: 3001,
      },
    },
  );

  await app.listen();
}

// main.ts (Aplicação principal)
import { MicroserviceOptions, Transport } from '@nestjs/microservices';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  // Conectar ao microserviço
  app.connectMicroservice<MicroserviceOptions>({
    transport: Transport.TCP,
    options: {
      host: 'localhost',
      port: 3001,
    },
  });

  await app.startAllMicroservices();
  await app.listen(3000);
}
```

### Tipos de Transporte
```typescript
// TCP
transport: Transport.TCP,
options: {
  host: 'localhost',
  port: 3001,
}

// Redis
transport: Transport.REDIS,
options: {
  url: 'redis://localhost:6379',
}

// NATS
transport: Transport.NATS,
options: {
  url: 'nats://localhost:4222',
}

// Kafka
transport: Transport.KAFKA,
options: {
  client: {
    brokers: ['localhost:9092'],
  },
  consumer: {
    groupId: 'my-group',
  },
}

// RabbitMQ
transport: Transport.RMQ,
options: {
  urls: ['amqp://localhost:5672'],
  queue: 'my_queue',
  queueOptions: {
    durable: false,
  },
}
```

### Controllers de Microserviços
```typescript
// users/users.controller.ts (Microserviço)
import { Controller } from '@nestjs/common';
import { MessagePattern, Payload } from '@nestjs/microservices';
import { UsersService } from './users.service';

@Controller()
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @MessagePattern({ cmd: 'get_users' })
  getUsers(@Payload() data: { page: number; limit: number }) {
    return this.usersService.findAll(data.page, data.limit);
  }

  @MessagePattern({ cmd: 'get_user' })
  getUser(@Payload() id: string) {
    return this.usersService.findOne(id);
  }

  @MessagePattern({ cmd: 'create_user' })
  createUser(@Payload() createUserDto: any) {
    return this.usersService.create(createUserDto);
  }

  @EventPattern('user_created')
  handleUserCreated(@Payload() data: any) {
    console.log('User created:', data);
  }
}
```

### Cliente de Microserviços
```typescript
// services/users-microservice.service.ts
import { Injectable, Inject, OnModuleInit } from '@nestjs/common';
import { ClientProxy, ClientGrpc } from '@nestjs/microservices';
import { Observable } from 'rxjs';

@Injectable()
export class UsersMicroserviceService implements OnModuleInit {
  constructor(
    @Inject('USERS_SERVICE') private client: ClientProxy,
    @Inject('USERS_GRPC_SERVICE') private grpcClient: ClientGrpc,
  ) {}

  onModuleInit() {
    // Conectar ao microserviço
    this.client.connect();
  }

  async getUsers(page: number = 1, limit: number = 10): Promise<any> {
    return this.client.send({ cmd: 'get_users' }, { page, limit }).toPromise();
  }

  async getUser(id: string): Promise<any> {
    return this.client.send({ cmd: 'get_user' }, id).toPromise();
  }

  async createUser(userData: any): Promise<any> {
    return this.client.send({ cmd: 'create_user' }, userData).toPromise();
  }

  // Usando gRPC
  private usersGrpcService: any;

  onModuleInitGrpc() {
    this.usersGrpcService = this.grpcClient.getService('UsersService');
  }

  async getUserGrpc(id: string): Promise<any> {
    return this.usersGrpcService.getUser({ id }).toPromise();
  }
}
```

### Configuração do Módulo
```typescript
// app.module.ts
import { Module } from '@nestjs/common';
import { ClientsModule, Transport } from '@nestjs/microservices';

@Module({
  imports: [
    ClientsModule.register([
      {
        name: 'USERS_SERVICE',
        transport: Transport.TCP,
        options: {
          host: 'localhost',
          port: 3001,
        },
      },
      {
        name: 'USERS_GRPC_SERVICE',
        transport: Transport.GRPC,
        options: {
          package: 'users',
          protoPath: join(__dirname, 'users/users.proto'),
        },
      },
    ]),
  ],
  providers: [UsersMicroserviceService],
})
export class AppModule {}
```

---

## Testes

### Configuração de Testes
```json
// package.json
{
  "scripts": {
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
    "test:e2e": "jest --config ./test/jest-e2e.json"
  }
}

// jest.config.js
module.exports = {
  moduleFileExtensions: ['js', 'json', 'ts'],
  rootDir: 'src',
  testRegex: '.*\\.spec\\.ts$',
  transform: {
    '^.+\\.(t|j)s$': 'ts-jest',
  },
  collectCoverageFrom: ['**/*.(t|j)s'],
  coverageDirectory: '../coverage',
  testEnvironment: 'node',
  moduleNameMapper: {
    '^src/(.*)$': '<rootDir>/$1',
  },
};
```

### Testes Unitários
```typescript
// users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { getRepositoryToken } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { UsersService } from './users.service';
import { User } from './entities/user.entity';
import { CreateUserDto } from './dto/create-user.dto';
import { NotFoundException } from '@nestjs/common';

describe('UsersService', () => {
  let service: UsersService;
  let repository: Repository<User>;

  const mockUser: User = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
    password: 'hashedpassword',
    isActive: true,
    createdAt: new Date(),
    updatedAt: new Date(),
  };

  const mockRepository = {
    find: jest.fn().mockResolvedValue([mockUser]),
    findOne: jest.fn().mockResolvedValue(mockUser),
    create: jest.fn().mockReturnValue(mockUser),
    save: jest.fn().mockResolvedValue(mockUser),
    delete: jest.fn().mockResolvedValue({ affected: 1 }),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getRepositoryToken(User),
          useValue: mockRepository,
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
    repository = module.get<Repository<User>>(getRepositoryToken(User));
  });

  it('should be defined', () => {
    expect(service).toBeDefined();
  });

  describe('findAll', () => {
    it('should return an array of users', async () => {
      const result = await service.findAll();
      expect(result).toEqual([mockUser]);
      expect(repository.find).toHaveBeenCalled();
    });
  });

  describe('findOne', () => {
    it('should return a user by id', async () => {
      const result = await service.findOne('1');
      expect(result).toEqual(mockUser);
      expect(repository.findOne).toHaveBeenCalledWith({ where: { id: '1' } });
    });

    it('should throw NotFoundException if user not found', async () => {
      mockRepository.findOne.mockResolvedValue(null);
      
      await expect(service.findOne('999')).rejects.toThrow(NotFoundException);
    });
  });

  describe('create', () => {
    it('should create and return a user', async () => {
      const createUserDto: CreateUserDto = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123',
      };

      const result = await service.create(createUserDto);
      
      expect(repository.create).toHaveBeenCalledWith(createUserDto);
      expect(repository.save).toHaveBeenCalled();
      expect(result).toEqual(mockUser);
    });
  });

  describe('update', () => {
    it('should update and return a user', async () => {
      const updateUserDto = { name: 'Updated Name' };
      
      jest.spyOn(service, 'findOne').mockResolvedValue(mockUser);
      
      const result = await service.update('1', updateUserDto);
      
      expect(service.findOne).toHaveBeenCalledWith('1');
      expect(repository.save).toHaveBeenCalledWith({
        ...mockUser,
        ...updateUserDto,
      });
      expect(result.name).toBe('Updated Name');
    });
  });

  describe('remove', () => {
    it('should delete a user', async () => {
      await service.remove('1');
      
      expect(repository.delete).toHaveBeenCalledWith('1');
    });

    it('should throw NotFoundException if user not found', async () => {
      mockRepository.delete.mockResolvedValue({ affected: 0 });
      
      await expect(service.remove('999')).rejects.toThrow(NotFoundException);
    });
  });
});
```

### Testes de Integração
```typescript
// app.e2e-spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from './../src/app.module';
import { Connection } from 'typeorm';

describe('AppController (e2e)', () => {
  let app: INestApplication;
  let connection: Connection;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();

    connection = moduleFixture.get(Connection);
  });

  afterAll(async () => {
    await connection.close();
    await app.close();
  });

  beforeEach(async () => {
    await connection.synchronize(true); // Recria o banco de dados
  });

  describe('Authentication', () => {
    it('/auth/register (POST)', () => {
      return request(app.getHttpServer())
        .post('/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'password123',
        })
        .expect(201)
        .expect((res) => {
          expect(res.body).toHaveProperty('access_token');
          expect(res.body.user).toHaveProperty('email', 'test@example.com');
        });
    });

    it('/auth/login (POST)', async () => {
      // Primeiro registra o usuário
      await request(app.getHttpServer())
        .post('/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'password123',
        });

      return request(app.getHttpServer())
        .post('/auth/login')
        .send({
          email: 'test@example.com',
          password: 'password123',
        })
        .expect(200)
        .expect((res) => {
          expect(res.body).toHaveProperty('access_token');
        });
    });
  });

  describe('Users', () => {
    let authToken: string;

    beforeEach(async () => {
      // Registra e obtém token
      const registerResponse = await request(app.getHttpServer())
        .post('/auth/register')
        .send({
          name: 'Test User',
          email: 'test@example.com',
          password: 'password123',
        });

      authToken = registerResponse.body.access_token;
    });

    it('/users (GET)', () => {
      return request(app.getHttpServer())
        .get('/users')
        .set('Authorization', `Bearer ${authToken}`)
        .expect(200)
        .expect((res) => {
          expect(Array.isArray(res.body)).toBe(true);
        });
    });

    it('/users (POST)', () => {
      return request(app.getHttpServer())
        .post('/users')
        .set('Authorization', `Bearer ${authToken}`)
        .send({
          name: 'New User',
          email: 'new@example.com',
          password: 'password123',
        })
        .expect(201)
        .expect((res) => {
          expect(res.body).toHaveProperty('id');
          expect(res.body.email).toBe('new@example.com');
        });
    });
  });
});
```

### Testes de Controladores
```typescript
// users.controller.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { UsersController } from './users.controller';
import { UsersService } from './users.service';
import { CreateUserDto } from './dto/create-user.dto';
import { UpdateUserDto } from './dto/update-user.dto';
import { JwtAuthGuard } from '../auth/guards/jwt-auth.guard';

describe('UsersController', () => {
  let controller: UsersController;
  let service: UsersService;

  const mockUser = {
    id: '1',
    name: 'John Doe',
    email: 'john@example.com',
    isActive: true,
    createdAt: new Date(),
    updatedAt: new Date(),
  };

  const mockUsersService = {
    findAll: jest.fn().mockResolvedValue([mockUser]),
    findOne: jest.fn().mockResolvedValue(mockUser),
    create: jest.fn().mockResolvedValue(mockUser),
    update: jest.fn().mockResolvedValue(mockUser),
    remove: jest.fn().mockResolvedValue(undefined),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      controllers: [UsersController],
      providers: [
        {
          provide: UsersService,
          useValue: mockUsersService,
        },
      ],
    })
      .overrideGuard(JwtAuthGuard)
      .useValue({ canActivate: () => true })
      .compile();

    controller = module.get<UsersController>(UsersController);
    service = module.get<UsersService>(UsersService);
  });

  it('should be defined', () => {
    expect(controller).toBeDefined();
  });

  describe('findAll', () => {
    it('should return an array of users', async () => {
      const result = await controller.findAll(1, 10);
      expect(result).toEqual([mockUser]);
      expect(service.findAll).toHaveBeenCalledWith(1, 10);
    });
  });

  describe('create', () => {
    it('should create a user', async () => {
      const createUserDto: CreateUserDto = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123',
      };

      const result = await controller.create(createUserDto);
      
      expect(result).toEqual(mockUser);
      expect(service.create).toHaveBeenCalledWith(createUserDto);
    });
  });

  describe('update', () => {
    it('should update a user', async () => {
      const updateUserDto: UpdateUserDto = { name: 'Updated Name' };
      
      const result = await controller.update('1', updateUserDto);
      
      expect(result).toEqual(mockUser);
      expect(service.update).toHaveBeenCalledWith('1', updateUserDto);
    });
  });
});
```

### Mocking
```typescript
// Mocks para testes
export const mockConfigService = {
  get: jest.fn((key: string) => {
    const config = {
      'JWT_SECRET': 'test-secret',
      'JWT_EXPIRES_IN': '1d',
      'DATABASE_HOST': 'localhost',
      'DATABASE_PORT': 5432,
    };
    return config[key];
  }),
};

export const mockJwtService = {
  sign: jest.fn().mockReturnValue('mocked-jwt-token'),
  verify: jest.fn().mockReturnValue({ sub: '1', email: 'test@example.com' }),
};

export const mockCacheManager = {
  get: jest.fn(),
  set: jest.fn(),
  del: jest.fn(),
};
```

---

## Deploy e Produção

### Configuração para Produção
```typescript
// main.ts (Produção)
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';
import { ValidationPipe } from '@nestjs/common';
import * as helmet from 'helmet';
import * as compression from 'compression';
import * as rateLimit from 'express-rate-limit';

async function bootstrap() {
  const app = await NestFactory.create(AppModule, {
    logger: ['error', 'warn', 'log'], // Em produção, menos logs
    cors: {
      origin: process.env.CLIENT_URL,
      credentials: true,
    },
  });

  // Segurança
  app.use(helmet());
  app.use(
    helmet.contentSecurityPolicy({
      directives: {
        defaultSrc: ["'self'"],
        styleSrc: ["'self'", "'unsafe-inline'"],
        scriptSrc: ["'self'", "'unsafe-inline'", "'unsafe-eval'"],
      },
    }),
  );

  // Performance
  app.use(compression());

  // Rate limiting
  app.use(
    rateLimit({
      windowMs: 15 * 60 * 1000, // 15 minutos
      max: 100, // limite por IP
      message: 'Too many requests from this IP',
    }),
  );

  // Pipes globais
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true,
      forbidNonWhitelisted: true,
      transform: true,
      disableErrorMessages: true, // Em produção
    }),
  );

  // Configuração de porta
  const port = process.env.PORT || 3000;
  
  await app.listen(port, '0.0.0.0');
  console.log(`Application is running on: ${await app.getUrl()}`);
}

bootstrap();
```

### Docker
```dockerfile
# Dockerfile
FROM node:18-alpine AS development

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci

COPY . .

RUN npm run build

FROM node:18-alpine AS production

ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm ci --only=production

COPY --from=development /usr/src/app/dist ./dist

EXPOSE 3000

CMD ["node", "dist/main"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      target: production
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://user:pass@db:5432/dbname
    depends_on:
      - db
      - redis
    networks:
      - app-network

  db:
    image: postgres:15-alpine
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=pass
      - POSTGRES_DB=dbname
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    networks:
      - app-network

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - app
    networks:
      - app-network

networks:
  app-network:
    driver: bridge

volumes:
  postgres_data:
```

### Configuração do PM2
```javascript
// ecosystem.config.js
module.exports = {
  apps: [
    {
      name: 'nest-app',
      script: 'dist/main.js',
      instances: 'max',
      exec_mode: 'cluster',
      env: {
        NODE_ENV: 'production',
        PORT: 3000,
      },
      env_production: {
        NODE_ENV: 'production',
        PORT: 3000,
      },
      error_file: './logs/err.log',
      out_file: './logs/out.log',
      log_file: './logs/combined.log',
      time: true,
      merge_logs: true,
      max_memory_restart: '1G',
      watch: false,
      ignore_watch: ['node_modules', 'logs'],
    },
  ],
};
```

```bash
# Comandos PM2
pm2 start ecosystem.config.js
pm2 stop nest-app
pm2 restart nest-app
pm2 delete nest-app
pm2 logs nest-app
pm2 monit
```

### CI/CD Pipeline
```yaml
# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    
    services:
      postgres:
        image: postgres:15-alpine
        env:
          POSTGRES_USER: test
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Run tests
        run: npm test
        env:
          DATABASE_URL: postgres://test:test@localhost:5432/test
          
  build-and-deploy:
    needs: test
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          
      - name: Install dependencies
        run: npm ci
        
      - name: Build
        run: npm run build
        
      - name: Deploy to Server
        uses: appleboy/scp-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          source: "dist,package.json,package-lock.json"
          target: "/var/www/app"
          
      - name: Restart Application
        uses: appleboy/ssh-action@master
        with:
          host: ${{ secrets.SERVER_HOST }}
          username: ${{ secrets.SERVER_USER }}
          key: ${{ secrets.SSH_PRIVATE_KEY }}
          script: |
            cd /var/www/app
            npm ci --production
            pm2 restart ecosystem.config.js
```

### Monitoramento
```typescript
// health/health.module.ts
import { Module } from '@nestjs/common';
import { TerminusModule } from '@nestjs/terminus';
import { HttpModule } from '@nestjs/axios';
import { HealthController } from './health.controller';
import { DatabaseHealthIndicator } from './database.health';

@Module({
  imports: [TerminusModule, HttpModule],
  controllers: [HealthController],
  providers: [DatabaseHealthIndicator],
})
export class HealthModule {}

// health/health.controller.ts
import { Controller, Get } from '@nestjs/common';
import {
  HealthCheck,
  HealthCheckService,
  HttpHealthIndicator,
  TypeOrmHealthIndicator,
  MemoryHealthIndicator,
} from '@nestjs/terminus';

@Controller('health')
export class HealthController {
  constructor(
    private health: HealthCheckService,
    private http: HttpHealthIndicator,
    private db: TypeOrmHealthIndicator,
    private memory: MemoryHealthIndicator,
  ) {}

  @Get()
  @HealthCheck()
  check() {
    return this.health.check([
      () => this.db.pingCheck('database'),
      () => this.memory.checkHeap('memory_heap', 150 * 1024 * 1024), // 150MB
      () => this.memory.checkRSS('memory_rss', 300 * 1024 * 1024), // 300MB
    ]);
  }
}
```

---

## Boas Práticas e Padrões

### 1. Estrutura de Projeto
```
src/
├── common/           # Código compartilhado
├── config/           # Configurações
├── modules/          # Módulos de funcionalidade
├── shared/           # Utilitários compartilhados
├── main.ts
└── app.module.ts
```

### 2. Organização por Domínio
```
modules/
├── auth/
│   ├── dto/
│   ├── entities/
│   ├── guards/
│   ├── strategies/
│   ├── auth.controller.ts
│   ├── auth.module.ts
│   ├── auth.service.ts
│   └── interfaces/
├── users/
├── products/
└── orders/
```

### 3. Princípios SOLID
```typescript
// Single Responsibility
class UserService {
  // Responsável apenas por operações de usuário
}

// Open/Closed
interface PaymentProcessor {
  process(amount: number): Promise<void>;
}

class CreditCardProcessor implements PaymentProcessor {
  async process(amount: number): Promise<void> {
    // Processar cartão de crédito
  }
}

// Liskov Substitution
abstract class NotificationService {
  abstract send(message: string): Promise<void>;
}

// Interface Segregation
interface UserRepository {
  findById(id: string): Promise<User>;
  findByEmail(email: string): Promise<User>;
}

interface UserCommandRepository {
  save(user: User): Promise<User>;
  delete(id: string): Promise<void>;
}

// Dependency Inversion
class UserController {
  constructor(private userService: UserService) {}
}
```

### 4. DTOs e Validação
```typescript
// Usar class-validator para validação
// Usar class-transformer para transformação
// Separar DTOs de entrada e saída
```

### 5. Tratamento de Erros
```typescript
// Usar exceções personalizadas
// Implementar filters globais
// Logar erros apropriadamente
```

### 6. Performance
- Usar cache quando apropriado
- Implementar paginação
- Otimizar queries de banco
- Usar compression middleware
- Habilitar CORS apropriadamente

### 7. Segurança
- Validar todos os inputs
- Sanitizar dados
- Usar helmet.js
- Implementar rate limiting
- Usar HTTPS em produção
- Validar tokens JWT

### 8. Logging
```typescript
// Implementar logging estruturado
// Usar diferentes níveis de log
// Logar contexto apropriado
```

### 9. Testes
- Testes unitários para serviços
- Testes de integração para APIs
- Testes e2e para fluxos completos
- Mock de dependências externas

### 10. Documentação
- Documentar APIs com Swagger
- Manter README atualizado
- Documentar variáveis de ambiente
- Documentar decisões de arquitetura

---

## Conclusão

NestJS é um framework poderoso que traz estrutura, escalabilidade e manutenibilidade para aplicações Node.js. Com sua arquitetura modular, injeção de dependências robusta e suporte a múltiplos protocolos, é uma excelente escolha para projetos de qualquer tamanho.

### Recursos Adicionais
- [Documentação Oficial](https://docs.nestjs.com/)
- [NestJS GitHub](https://github.com/nestjs/nest)
- [Awesome NestJS](https://github.com/juliandavidmr/awesome-nestjs)
- [NestJS Course](https://courses.nestjs.com/)

### Comunidade
- Discord oficial
- Stack Overflow (tag: nestjs)
- GitHub Discussions

---
*Última atualização: Janeiro 2024*
*Versão do NestJS: 10.0+*
