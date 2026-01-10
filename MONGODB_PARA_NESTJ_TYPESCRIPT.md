# Manual Completo de MongoDB para NestJS e TypeScript

## Sumário
1. [Introdução ao MongoDB com NestJS](#introdução-ao-mongodb-com-nestjs)
2. [Arquitetura e Conceitos MongoDB](#arquitetura-e-conceitos-mongodb)
3. [Instalação e Configuração](#instalação-e-configuração)
4. [Conexão e Configuração do Database](#conexão-e-configuração-do-database)
5. [Schemas e Models com Mongoose](#schemas-e-models-com-mongoose)
6. [DTOs e Validação](#dtos-e-validação)
7. [Repositórios e Serviços](#repositórios-e-serviços)
8. [CRUD Completo](#crud-completo)
9. [Queries Avançadas](#queries-avançadas)
10. [Aggregation Framework](#aggregation-framework)
11. [Indexes e Performance](#indexes-e-performance)
12. [Transações](#transações)
13. [Relacionamentos](#relacionamentos)
14. [Recursos Avançados](#recursos-avançados)
15. [Testes com MongoDB](#testes-com-mongodb)
16. [Performance e Otimização](#performance-e-otimização)
17. [Segurança](#segurança)
18. [Deploy e Produção](#deploy-e-produção)
19. [Boas Práticas](#boas-práticas)

---

## Introdução ao MongoDB com NestJS

MongoDB é um banco de dados NoSQL orientado a documentos que oferece alta performance, alta disponibilidade e escalabilidade fácil. Com NestJS, utilizamos o Mongoose como ODM (Object Document Mapper) para interagir com o MongoDB.

### Por que MongoDB com NestJS?
- **Schema-flexível**: Documentos JSON-like
- **Escalabilidade horizontal**: Sharding nativo
- **Alta performance**: Leitura/escrita rápidas
- **Agregações poderosas**: Pipeline aggregation
- **Índices eficientes**: Secondary indexes, text search
- **Transações ACID**: Suporte completo desde v4.0

### Stack Tecnológica Recomendada
```
NestJS + TypeScript + Mongoose + MongoDB Atlas/Compass
```

---

## Arquitetura e Conceitos MongoDB

### Conceitos Fundamentais
- **Database**: Contêiner para collections
- **Collection**: Grupo de documentos (similar a tabelas)
- **Document**: Registro JSON-like (similar a linhas)
- **Field**: Par chave-valor em um documento
- **Embedded Documents**: Documentos dentro de documentos
- **References**: Relacionamentos entre documentos

### Modelagem de Dados
```typescript
// Embedding (dados relacionados juntos)
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  name: "João Silva",
  address: {
    street: "Rua A",
    city: "São Paulo",
    zip: "01310-000"
  }
}

// Referencing (relacionamentos)
{
  _id: ObjectId("507f1f77bcf86cd799439011"),
  name: "João Silva",
  orders: [
    ObjectId("507f1f77bcf86cd799439012"),
    ObjectId("507f1f77bcf86cd799439013")
  ]
}
```

---

## Instalação e Configuração

### Instalação de Dependências
```bash
# Dependências principais
npm install @nestjs/mongoose mongoose

# Dependências de desenvolvimento
npm install -D @types/mongoose

# Para validação
npm install class-validator class-transformer

# Para variáveis de ambiente
npm install @nestjs/config

# Para logging (opcional)
npm install mongoose-paginate-v2 mongoose-aggregate-paginate-v2
```

### Configuração do Ambiente
```env
# .env
MONGODB_URI=mongodb://localhost:27017/nestjs-db
MONGODB_URI_PROD=mongodb+srv://username:password@cluster.mongodb.net/nestjs-db?retryWrites=true&w=majority
MONGODB_USERNAME=admin
MONGODB_PASSWORD=secret
MONGODB_DATABASE=nestjs-db
MONGODB_AUTH_SOURCE=admin
```

---

## Conexão e Configuração do Database

### Módulo de Configuração
```typescript
// database/database.module.ts
import { Module, Global } from '@nestjs/common';
import { ConfigService } from '@nestjs/config';
import { MongooseModule } from '@nestjs/mongoose';

@Global()
@Module({
  imports: [
    MongooseModule.forRootAsync({
      imports: [ConfigModule],
      useFactory: async (configService: ConfigService) => ({
        uri: configService.get<string>('MONGODB_URI'),
        useNewUrlParser: true,
        useUnifiedTopology: true,
        connectionFactory: (connection) => {
          connection.plugin(require('mongoose-autopopulate'));
          return connection;
        },
      }),
      inject: [ConfigService],
    }),
  ],
  exports: [MongooseModule],
})
export class DatabaseModule {}
```

### Configuração Avançada
```typescript
// database/configuration.ts
export const getMongoConfig = async (
  configService: ConfigService,
): Promise<MongooseModuleOptions> => {
  const uri = configService.get<string>('MONGODB_URI');
  const username = configService.get<string>('MONGODB_USERNAME');
  const password = configService.get<string>('MONGODB_PASSWORD');
  const database = configService.get<string>('MONGODB_DATABASE');

  return {
    uri: `mongodb+srv://${username}:${password}@cluster.mongodb.net/${database}?retryWrites=true&w=majority`,
    useNewUrlParser: true,
    useUnifiedTopology: true,
    retryAttempts: 5,
    retryDelay: 3000,
    connectionFactory: (connection) => {
      // Plugins globais
      connection.plugin(require('mongoose-autopopulate'));
      connection.plugin(require('mongoose-paginate-v2'));
      
      // Event listeners
      connection.on('connected', () => {
        console.log('MongoDB connected successfully');
      });
      
      connection.on('error', (err) => {
        console.error('MongoDB connection error:', err);
      });
      
      return connection;
    },
  };
};

// Uso no módulo
MongooseModule.forRootAsync({
  imports: [ConfigModule],
  useFactory: getMongoConfig,
  inject: [ConfigService],
})
```

### Múltiplas Conexões
```typescript
// database/multiple-connections.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';

@Module({
  imports: [
    // Conexão principal
    MongooseModule.forRoot('mongodb://localhost:27017/main', {
      connectionName: 'main',
    }),
    
    // Conexão para logs
    MongooseModule.forRoot('mongodb://localhost:27017/logs', {
      connectionName: 'logs',
    }),
    
    // Conexão para cache
    MongooseModule.forRoot('mongodb://localhost:27017/cache', {
      connectionName: 'cache',
    }),
  ],
})
export class MultipleConnectionsModule {}

// Uso com múltiplas conexões
@Injectable()
export class LogService {
  constructor(
    @InjectConnection('logs') private logsConnection: Connection,
  ) {}
}
```

---

## Schemas e Models com Mongoose

### Schema Básico
```typescript
// schemas/user.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document, Types } from 'mongoose';
import { Exclude, Transform } from 'class-transformer';

export type UserDocument = User & Document;

@Schema({
  timestamps: true, // Cria createdAt e updatedAt automaticamente
  toJSON: {
    virtuals: true, // Inclui virtuals no JSON
    transform: (doc, ret) => {
      delete ret.password; // Remove campos sensíveis
      delete ret.__v; // Remove versão do documento
      return ret;
    },
  },
  toObject: {
    virtuals: true,
    transform: (doc, ret) => {
      delete ret.password;
      delete ret.__v;
      return ret;
    },
  },
})
export class User {
  @Transform(({ value }) => value.toString())
  _id: Types.ObjectId;

  @Prop({
    type: String,
    required: [true, 'Name is required'],
    trim: true,
    minlength: [2, 'Name must be at least 2 characters'],
    maxlength: [50, 'Name cannot exceed 50 characters'],
  })
  name: string;

  @Prop({
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    trim: true,
    match: [
      /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/,
      'Please provide a valid email',
    ],
  })
  email: string;

  @Prop({
    type: String,
    required: [true, 'Password is required'],
    minlength: [6, 'Password must be at least 6 characters'],
    select: false, // Não retorna em queries por padrão
  })
  @Exclude() // Exclui do class-transformer
  password: string;

  @Prop({
    type: String,
    enum: ['user', 'admin', 'moderator'],
    default: 'user',
  })
  role: string;

  @Prop({
    type: Boolean,
    default: true,
  })
  isActive: boolean;

  @Prop({
    type: Date,
    default: null,
  })
  emailVerifiedAt: Date;

  @Prop({
    type: String,
    default: null,
  })
  avatar: string;

  @Prop({
    type: [String],
    default: [],
  })
  tags: string[];

  @Prop({
    type: Map,
    of: String,
    default: {},
  })
  metadata: Map<string, string>;

  @Prop({
    type: Types.ObjectId,
    ref: 'Company',
  })
  company: Types.ObjectId;

  // Campos virtuais
  fullName?: string;

  // Campos automáticos do timestamps
  createdAt?: Date;
  updatedAt?: Date;
}

export const UserSchema = SchemaFactory.createForClass(User);

// Índices para performance
UserSchema.index({ email: 1 }, { unique: true });
UserSchema.index({ name: 1 });
UserSchema.index({ role: 1, isActive: 1 });
UserSchema.index({ createdAt: -1 });
UserSchema.index({ 'metadata.key': 1 }); // Índice para Map fields

// Virtuals
UserSchema.virtual('fullName').get(function () {
  return `${this.name} (${this.email})`;
});

// Middleware (hooks)
UserSchema.pre<UserDocument>('save', async function (next) {
  // Hash password antes de salvar
  if (this.isModified('password')) {
    const salt = await bcrypt.genSalt(10);
    this.password = await bcrypt.hash(this.password, salt);
  }
  next();
});

// Métodos de instância
UserSchema.methods.comparePassword = async function (
  candidatePassword: string,
): Promise<boolean> {
  return bcrypt.compare(candidatePassword, this.password);
};

UserSchema.methods.toJSON = function () {
  const user = this.toObject();
  delete user.password;
  delete user.__v;
  return user;
};

// Métodos estáticos
UserSchema.statics.findByEmail = function (email: string) {
  return this.findOne({ email }).exec();
};

UserSchema.statics.findActiveUsers = function () {
  return this.find({ isActive: true }).exec();
};
```

### Schema com Subdocumentos
```typescript
// schemas/address.schema.ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

@Schema({ _id: false }) // Não cria _id para subdocumentos
export class Address {
  @Prop({ required: true })
  street: string;

  @Prop({ required: true })
  city: string;

  @Prop({ required: true })
  state: string;

  @Prop({ required: true })
  zipCode: string;

  @Prop()
  country: string;

  @Prop({ default: false })
  isPrimary: boolean;
}

export const AddressSchema = SchemaFactory.createForClass(Address);

// schemas/user.schema.ts
@Schema()
export class User {
  // ... outros campos

  @Prop({ type: [AddressSchema], default: [] })
  addresses: Address[];

  @Prop({ type: AddressSchema })
  primaryAddress: Address;
}
```

### Schema com Discriminadores (Herança)
```typescript
// schemas/base-product.schema.ts
@Schema({ discriminatorKey: 'type', timestamps: true })
export class BaseProduct {
  @Prop({ required: true })
  name: string;

  @Prop({ required: true })
  price: number;

  @Prop({ required: true })
  type: string;
}

// schemas/book-product.schema.ts
@Schema()
export class BookProduct extends BaseProduct {
  @Prop({ required: true })
  author: string;

  @Prop({ required: true })
  isbn: string;

  @Prop()
  publisher: string;
}

// schemas/electronic-product.schema.ts
@Schema()
export class ElectronicProduct extends BaseProduct {
  @Prop({ required: true })
  brand: string;

  @Prop({ required: true })
  model: string;

  @Prop()
  warranty: number; // meses
}

// product.module.ts
@Module({
  imports: [
    MongooseModule.forFeature([
      {
        name: BaseProduct.name,
        schema: BaseProductSchema,
        discriminators: [
          { name: BookProduct.name, schema: BookProductSchema },
          { name: ElectronicProduct.name, schema: ElectronicProductSchema },
        ],
      },
    ]),
  ],
})
export class ProductModule {}
```

### Enums e Validação Customizada
```typescript
// enums/user.enum.ts
export enum UserRole {
  USER = 'user',
  ADMIN = 'admin',
  MODERATOR = 'moderator',
  GUEST = 'guest',
}

export enum UserStatus {
  ACTIVE = 'active',
  INACTIVE = 'inactive',
  SUSPENDED = 'suspended',
  DELETED = 'deleted',
}

// schemas/user.schema.ts
@Schema()
export class User {
  @Prop({
    type: String,
    enum: Object.values(UserRole),
    default: UserRole.USER,
  })
  role: UserRole;

  @Prop({
    type: String,
    enum: Object.values(UserStatus),
    default: UserStatus.ACTIVE,
  })
  status: UserStatus;

  @Prop({
    type: Date,
    validate: {
      validator: function (value: Date) {
        return value < new Date();
      },
      message: 'Birth date must be in the past',
    },
  })
  birthDate: Date;

  @Prop({
    type: String,
    validate: {
      validator: function (v: string) {
        return /^\+?[1-9]\d{1,14}$/.test(v);
      },
      message: (props) => `${props.value} is not a valid phone number!`,
    },
  })
  phone: string;

  @Prop({
    type: Number,
    min: [0, 'Age cannot be negative'],
    max: [150, 'Age cannot exceed 150'],
  })
  age: number;
}
```

---

## DTOs e Validação

### DTOs para CRUD
```typescript
// dto/create-user.dto.ts
import {
  IsString,
  IsEmail,
  IsNotEmpty,
  MinLength,
  MaxLength,
  IsOptional,
  IsEnum,
  IsArray,
  ValidateNested,
  IsBoolean,
  IsDateString,
  IsNumber,
  Matches,
} from 'class-validator';
import { Type } from 'class-transformer';
import { ApiProperty, ApiPropertyOptional } from '@nestjs/swagger';
import { UserRole, UserStatus } from '../enums/user.enum';

export class CreateAddressDto {
  @ApiProperty({ example: '123 Main St' })
  @IsString()
  @IsNotEmpty()
  street: string;

  @ApiProperty({ example: 'New York' })
  @IsString()
  @IsNotEmpty()
  city: string;

  @ApiProperty({ example: 'NY' })
  @IsString()
  @IsNotEmpty()
  @MaxLength(2)
  state: string;

  @ApiProperty({ example: '10001' })
  @IsString()
  @IsNotEmpty()
  @Matches(/^\d{5}(-\d{4})?$/, {
    message: 'Invalid ZIP code format',
  })
  zipCode: string;

  @ApiPropertyOptional({ example: 'USA' })
  @IsString()
  @IsOptional()
  country?: string;
}

export class CreateUserDto {
  @ApiProperty({ example: 'John Doe', description: 'User full name' })
  @IsString()
  @IsNotEmpty()
  @MinLength(2)
  @MaxLength(50)
  name: string;

  @ApiProperty({ example: 'john@example.com', description: 'User email' })
  @IsEmail()
  @IsNotEmpty()
  email: string;

  @ApiProperty({ example: 'password123', description: 'User password' })
  @IsString()
  @IsNotEmpty()
  @MinLength(6)
  @MaxLength(100)
  password: string;

  @ApiPropertyOptional({ example: 'admin', enum: UserRole })
  @IsEnum(UserRole)
  @IsOptional()
  role?: UserRole;

  @ApiPropertyOptional({ example: true })
  @IsBoolean()
  @IsOptional()
  isActive?: boolean;

  @ApiPropertyOptional({ example: '1990-01-01' })
  @IsDateString()
  @IsOptional()
  birthDate?: string;

  @ApiPropertyOptional({ example: 30 })
  @IsNumber()
  @IsOptional()
  age?: number;

  @ApiPropertyOptional({ example: '+1234567890' })
  @IsString()
  @IsOptional()
  phone?: string;

  @ApiPropertyOptional({ type: [CreateAddressDto] })
  @IsArray()
  @ValidateNested({ each: true })
  @Type(() => CreateAddressDto)
  @IsOptional()
  addresses?: CreateAddressDto[];

  @ApiPropertyOptional({ example: ['tag1', 'tag2'] })
  @IsArray()
  @IsString({ each: true })
  @IsOptional()
  tags?: string[];

  @ApiPropertyOptional({
    example: { department: 'IT', location: 'Remote' },
    type: 'object',
  })
  @IsOptional()
  metadata?: Record<string, any>;
}

// dto/update-user.dto.ts
import { PartialType } from '@nestjs/swagger';
import { CreateUserDto } from './create-user.dto';

export class UpdateUserDto extends PartialType(CreateUserDto) {}

// dto/filter-user.dto.ts
export class FilterUserDto {
  @ApiPropertyOptional({ example: 'John' })
  @IsString()
  @IsOptional()
  search?: string;

  @ApiPropertyOptional({ example: 'admin', enum: UserRole })
  @IsEnum(UserRole)
  @IsOptional()
  role?: UserRole;

  @ApiPropertyOptional({ example: true })
  @IsBoolean()
  @IsOptional()
  isActive?: boolean;

  @ApiPropertyOptional({ example: '2023-01-01' })
  @IsDateString()
  @IsOptional()
  startDate?: string;

  @ApiPropertyOptional({ example: '2023-12-31' })
  @IsDateString()
  @IsOptional()
  endDate?: string;

  @ApiPropertyOptional({ example: 1 })
  @IsNumber()
  @IsOptional()
  page?: number = 1;

  @ApiPropertyOptional({ example: 10 })
  @IsNumber()
  @IsOptional()
  limit?: number = 10;

  @ApiPropertyOptional({ example: 'createdAt' })
  @IsString()
  @IsOptional()
  sortBy?: string = 'createdAt';

  @ApiPropertyOptional({ example: 'desc', enum: ['asc', 'desc'] })
  @IsString()
  @IsOptional()
  sortOrder?: 'asc' | 'desc' = 'desc';
}

// dto/response-user.dto.ts
export class UserResponseDto {
  @ApiProperty({ example: '507f1f77bcf86cd799439011' })
  id: string;

  @ApiProperty({ example: 'John Doe' })
  name: string;

  @ApiProperty({ example: 'john@example.com' })
  email: string;

  @ApiProperty({ example: 'admin', enum: UserRole })
  role: UserRole;

  @ApiProperty({ example: true })
  isActive: boolean;

  @ApiProperty({ example: '2023-01-01T00:00:00.000Z' })
  createdAt: Date;

  @ApiProperty({ example: '2023-01-01T00:00:00.000Z' })
  updatedAt: Date;

  constructor(user: any) {
    this.id = user._id.toString();
    this.name = user.name;
    this.email = user.email;
    this.role = user.role;
    this.isActive = user.isActive;
    this.createdAt = user.createdAt;
    this.updatedAt = user.updatedAt;
  }
}
```

### Validação Global
```typescript
// main.ts
import { ValidationPipe } from '@nestjs/common';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  app.useGlobalPipes(
    new ValidationPipe({
      whitelist: true, // Remove propriedades não decoradas
      forbidNonWhitelisted: true, // Lança erro para propriedades não decoradas
      transform: true, // Transforma tipos automaticamente
      transformOptions: {
        enableImplicitConversion: true,
      },
      validationError: {
        target: process.env.NODE_ENV !== 'production',
        value: process.env.NODE_ENV !== 'production',
      },
    }),
  );
  
  await app.listen(3000);
}
```

---

## Repositórios e Serviços

### Padrão Repository
```typescript
// repositories/base.repository.ts
import { Model, Document, FilterQuery, UpdateQuery } from 'mongoose';
import { NotFoundException } from '@nestjs/common';

export abstract class BaseRepository<T extends Document> {
  constructor(protected readonly model: Model<T>) {}

  async create(createDto: any): Promise<T> {
    const created = new this.model(createDto);
    return created.save();
  }

  async findById(id: string): Promise<T> {
    const document = await this.model.findById(id).exec();
    if (!document) {
      throw new NotFoundException(`${this.model.modelName} with id ${id} not found`);
    }
    return document;
  }

  async findOne(filter: FilterQuery<T>): Promise<T | null> {
    return this.model.findOne(filter).exec();
  }

  async find(filter: FilterQuery<T> = {}): Promise<T[]> {
    return this.model.find(filter).exec();
  }

  async update(id: string, updateDto: UpdateQuery<T>): Promise<T> {
    const document = await this.model
      .findByIdAndUpdate(id, updateDto, { new: true })
      .exec();
    
    if (!document) {
      throw new NotFoundException(`${this.model.modelName} with id ${id} not found`);
    }
    
    return document;
  }

  async delete(id: string): Promise<T> {
    const document = await this.model.findByIdAndDelete(id).exec();
    
    if (!document) {
      throw new NotFoundException(`${this.model.modelName} with id ${id} not found`);
    }
    
    return document;
  }

  async exists(filter: FilterQuery<T>): Promise<boolean> {
    const count = await this.model.countDocuments(filter).exec();
    return count > 0;
  }

  async count(filter: FilterQuery<T> = {}): Promise<number> {
    return this.model.countDocuments(filter).exec();
  }
}

// repositories/user.repository.ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model, Types } from 'mongoose';
import { BaseRepository } from './base.repository';
import { User, UserDocument } from '../schemas/user.schema';
import { FilterUserDto } from '../dto/filter-user.dto';

@Injectable()
export class UserRepository extends BaseRepository<UserDocument> {
  constructor(@InjectModel(User.name) userModel: Model<UserDocument>) {
    super(userModel);
  }

  async findByEmail(email: string): Promise<UserDocument | null> {
    return this.model.findOne({ email }).select('+password').exec();
  }

  async findActiveUsers(): Promise<UserDocument[]> {
    return this.model.find({ isActive: true }).exec();
  }

  async searchUsers(filterDto: FilterUserDto): Promise<{
    data: UserDocument[];
    total: number;
    page: number;
    limit: number;
    pages: number;
  }> {
    const {
      search,
      role,
      isActive,
      startDate,
      endDate,
      page = 1,
      limit = 10,
      sortBy = 'createdAt',
      sortOrder = 'desc',
    } = filterDto;

    const query: any = {};

    // Filtro de busca
    if (search) {
      query.$or = [
        { name: { $regex: search, $options: 'i' } },
        { email: { $regex: search, $options: 'i' } },
      ];
    }

    // Filtros específicos
    if (role) query.role = role;
    if (isActive !== undefined) query.isActive = isActive;

    // Filtro por data
    if (startDate || endDate) {
      query.createdAt = {};
      if (startDate) query.createdAt.$gte = new Date(startDate);
      if (endDate) query.createdAt.$lte = new Date(endDate);
    }

    // Ordenação
    const sort: any = {};
    sort[sortBy] = sortOrder === 'desc' ? -1 : 1;

    // Paginação
    const skip = (page - 1) * limit;

    // Executar queries em paralelo
    const [data, total] = await Promise.all([
      this.model
        .find(query)
        .sort(sort)
        .skip(skip)
        .limit(limit)
        .populate('company')
        .exec(),
      this.model.countDocuments(query).exec(),
    ]);

    return {
      data,
      total,
      page,
      limit,
      pages: Math.ceil(total / limit),
    };
  }

  async updatePassword(
    userId: string,
    newPassword: string,
  ): Promise<UserDocument> {
    const user = await this.findById(userId);
    user.password = newPassword;
    return user.save();
  }

  async addAddress(
    userId: string,
    address: any,
  ): Promise<UserDocument> {
    return this.model.findByIdAndUpdate(
      userId,
      { $push: { addresses: address } },
      { new: true },
    ).exec();
  }

  async removeAddress(
    userId: string,
    addressId: string,
  ): Promise<UserDocument> {
    return this.model.findByIdAndUpdate(
      userId,
      { $pull: { addresses: { _id: addressId } } },
      { new: true },
    ).exec();
  }

  async getUserStats(): Promise<any> {
    return this.model.aggregate([
      {
        $group: {
          _id: '$role',
          count: { $sum: 1 },
          avgAge: { $avg: '$age' },
          maxAge: { $max: '$age' },
          minAge: { $min: '$age' },
        },
      },
      {
        $project: {
          role: '$_id',
          count: 1,
          avgAge: { $round: ['$avgAge', 2] },
          maxAge: 1,
          minAge: 1,
          _id: 0,
        },
      },
      { $sort: { count: -1 } },
    ]).exec();
  }
}
```

### Serviços
```typescript
// services/user.service.ts
import {
  Injectable,
  NotFoundException,
  ConflictException,
  BadRequestException,
} from '@nestjs/common';
import { UserRepository } from '../repositories/user.repository';
import { CreateUserDto } from '../dto/create-user.dto';
import { UpdateUserDto } from '../dto/update-user.dto';
import { FilterUserDto } from '../dto/filter-user.dto';
import { UserResponseDto } from '../dto/response-user.dto';
import * as bcrypt from 'bcrypt';

@Injectable()
export class UserService {
  constructor(private readonly userRepository: UserRepository) {}

  async create(createUserDto: CreateUserDto): Promise<UserResponseDto> {
    // Verificar se email já existe
    const existingUser = await this.userRepository.findByEmail(
      createUserDto.email,
    );
    
    if (existingUser) {
      throw new ConflictException('Email already exists');
    }

    // Hash da senha
    const salt = await bcrypt.genSalt(10);
    const hashedPassword = await bcrypt.hash(createUserDto.password, salt);

    // Criar usuário
    const user = await this.userRepository.create({
      ...createUserDto,
      password: hashedPassword,
    });

    return new UserResponseDto(user);
  }

  async findAll(filterDto: FilterUserDto): Promise<{
    data: UserResponseDto[];
    total: number;
    page: number;
    limit: number;
    pages: number;
  }> {
    const result = await this.userRepository.searchUsers(filterDto);
    
    return {
      ...result,
      data: result.data.map(user => new UserResponseDto(user)),
    };
  }

  async findOne(id: string): Promise<UserResponseDto> {
    const user = await this.userRepository.findById(id);
    return new UserResponseDto(user);
  }

  async update(id: string, updateUserDto: UpdateUserDto): Promise<UserResponseDto> {
    // Não permitir atualização de email
    if (updateUserDto.email) {
      throw new BadRequestException('Email cannot be updated');
    }

    // Hash da senha se fornecida
    if (updateUserDto.password) {
      const salt = await bcrypt.genSalt(10);
      updateUserDto.password = await bcrypt.hash(updateUserDto.password, salt);
    }

    const user = await this.userRepository.update(id, updateUserDto);
    return new UserResponseDto(user);
  }

  async remove(id: string): Promise<{ message: string }> {
    await this.userRepository.delete(id);
    return { message: 'User deleted successfully' };
  }

  async findByEmail(email: string): Promise<UserResponseDto> {
    const user = await this.userRepository.findByEmail(email);
    
    if (!user) {
      throw new NotFoundException('User not found');
    }
    
    return new UserResponseDto(user);
  }

  async comparePassword(
    userId: string,
    candidatePassword: string,
  ): Promise<boolean> {
    const user = await this.userRepository.findById(userId);
    return bcrypt.compare(candidatePassword, user.password);
  }

  async changePassword(
    userId: string,
    oldPassword: string,
    newPassword: string,
  ): Promise<{ message: string }> {
    // Verificar senha antiga
    const isValid = await this.comparePassword(userId, oldPassword);
    
    if (!isValid) {
      throw new BadRequestException('Invalid old password');
    }

    // Atualizar senha
    await this.userRepository.updatePassword(userId, newPassword);
    
    return { message: 'Password changed successfully' };
  }

  async getStats(): Promise<any> {
    return this.userRepository.getUserStats();
  }

  async deactivate(id: string): Promise<UserResponseDto> {
    const user = await this.userRepository.update(id, {
      isActive: false,
      deactivatedAt: new Date(),
    });
    
    return new UserResponseDto(user);
  }

  async activate(id: string): Promise<UserResponseDto> {
    const user = await this.userRepository.update(id, {
      isActive: true,
      deactivatedAt: null,
    });
    
    return new UserResponseDto(user);
  }
}
```

---

## CRUD Completo

### Module Configuration
```typescript
// users/users.module.ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { User, UserSchema } from './schemas/user.schema';
import { UserRepository } from './repositories/user.repository';
import { UserService } from './services/user.service';
import { UserController } from './controllers/user.controller';

@Module({
  imports: [
    MongooseModule.forFeature([{ name: User.name, schema: UserSchema }]),
  ],
  controllers: [UserController],
  providers: [UserRepository, UserService],
  exports: [UserService],
})
export class UsersModule {}
```

### Controller Completo
```typescript
// controllers/user.controller.ts
import {
  Controller,
  Get,
  Post,
  Body,
  Patch,
  Param,
  Delete,
  Query,
  HttpCode,
  HttpStatus,
  UseGuards,
  UseInterceptors,
  ClassSerializerInterceptor,
} from '@nestjs/common';
import {
  ApiTags,
  ApiOperation,
  ApiResponse,
  ApiBearerAuth,
  ApiQuery,
} from '@nestjs/swagger';
import { UserService } from '../services/user.service';
import { CreateUserDto } from '../dto/create-user.dto';
import { UpdateUserDto } from '../dto/update-user.dto';
import { FilterUserDto } from '../dto/filter-user.dto';
import { UserResponseDto } from '../dto/response-user.dto';
import { JwtAuthGuard } from '../../auth/guards/jwt-auth.guard';
import { RolesGuard } from '../../auth/guards/roles.guard';
import { Roles } from '../../auth/decorators/roles.decorator';
import { UserRole } from '../enums/user.enum';

@ApiTags('users')
@Controller('users')
@UseGuards(JwtAuthGuard, RolesGuard)
@UseInterceptors(ClassSerializerInterceptor)
@ApiBearerAuth()
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  @Roles(UserRole.ADMIN)
  @ApiOperation({ summary: 'Create a new user' })
  @ApiResponse({
    status: HttpStatus.CREATED,
    description: 'User created successfully',
    type: UserResponseDto,
  })
  @ApiResponse({
    status: HttpStatus.CONFLICT,
    description: 'Email already exists',
  })
  async create(@Body() createUserDto: CreateUserDto): Promise<UserResponseDto> {
    return this.userService.create(createUserDto);
  }

  @Get()
  @Roles(UserRole.ADMIN, UserRole.MODERATOR)
  @ApiOperation({ summary: 'Get all users with pagination' })
  @ApiQuery({ name: 'search', required: false })
  @ApiQuery({ name: 'role', required: false, enum: UserRole })
  @ApiQuery({ name: 'isActive', required: false, type: Boolean })
  @ApiQuery({ name: 'page', required: false, type: Number })
  @ApiQuery({ name: 'limit', required: false, type: Number })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'List of users',
  })
  async findAll(@Query() filterDto: FilterUserDto): Promise<{
    data: UserResponseDto[];
    total: number;
    page: number;
    limit: number;
    pages: number;
  }> {
    return this.userService.findAll(filterDto);
  }

  @Get(':id')
  @Roles(UserRole.ADMIN, UserRole.MODERATOR, UserRole.USER)
  @ApiOperation({ summary: 'Get user by ID' })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'User found',
    type: UserResponseDto,
  })
  @ApiResponse({
    status: HttpStatus.NOT_FOUND,
    description: 'User not found',
  })
  async findOne(@Param('id') id: string): Promise<UserResponseDto> {
    return this.userService.findOne(id);
  }

  @Patch(':id')
  @Roles(UserRole.ADMIN, UserRole.MODERATOR)
  @ApiOperation({ summary: 'Update user' })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'User updated',
    type: UserResponseDto,
  })
  @ApiResponse({
    status: HttpStatus.NOT_FOUND,
    description: 'User not found',
  })
  async update(
    @Param('id') id: string,
    @Body() updateUserDto: UpdateUserDto,
  ): Promise<UserResponseDto> {
    return this.userService.update(id, updateUserDto);
  }

  @Delete(':id')
  @Roles(UserRole.ADMIN)
  @HttpCode(HttpStatus.NO_CONTENT)
  @ApiOperation({ summary: 'Delete user' })
  @ApiResponse({
    status: HttpStatus.NO_CONTENT,
    description: 'User deleted',
  })
  @ApiResponse({
    status: HttpStatus.NOT_FOUND,
    description: 'User not found',
  })
  async remove(@Param('id') id: string): Promise<{ message: string }> {
    return this.userService.remove(id);
  }

  @Get('stats/summary')
  @Roles(UserRole.ADMIN)
  @ApiOperation({ summary: 'Get user statistics' })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'User statistics',
  })
  async getStats(): Promise<any> {
    return this.userService.getStats();
  }

  @Patch(':id/deactivate')
  @Roles(UserRole.ADMIN, UserRole.MODERATOR)
  @ApiOperation({ summary: 'Deactivate user' })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'User deactivated',
    type: UserResponseDto,
  })
  async deactivate(@Param('id') id: string): Promise<UserResponseDto> {
    return this.userService.deactivate(id);
  }

  @Patch(':id/activate')
  @Roles(UserRole.ADMIN, UserRole.MODERATOR)
  @ApiOperation({ summary: 'Activate user' })
  @ApiResponse({
    status: HttpStatus.OK,
    description: 'User activated',
    type: UserResponseDto,
  })
  async activate(@Param('id') id: string): Promise<UserResponseDto> {
    return this.userService.activate(id);
  }
}
```

---

## Queries Avançadas

### Query Builders
```typescript
// utils/query-builder.ts
export class MongoQueryBuilder {
  static buildTextSearch(fields: string[], search: string): any {
    if (!search) return {};
    
    return {
      $or: fields.map(field => ({
        [field]: { $regex: search, $options: 'i' },
      })),
    };
  }

  static buildDateRange(
    field: string,
    startDate?: Date,
    endDate?: Date,
  ): any {
    const dateFilter: any = {};
    
    if (startDate) {
      dateFilter.$gte = startDate;
    }
    
    if (endDate) {
      dateFilter.$lte = endDate;
    }
    
    return Object.keys(dateFilter).length > 0
      ? { [field]: dateFilter }
      : {};
  }

  static buildArrayFilter(
    field: string,
    values: any[],
    operator: '$in' | '$nin' | '$all' = '$in',
  ): any {
    if (!values || values.length === 0) return {};
    
    return { [field]: { [operator]: values } };
  }

  static buildExistsFilter(field: string, exists: boolean): any {
    return { [field]: { $exists: exists } };
  }

  static buildNestedFilter(
    path: string,
    filter: any,
  ): any {
    return { [path]: filter };
  }
}

// Uso no repositório
async advancedSearch(criteria: any) {
  const query: any = {};

  // Busca por texto
  if (criteria.search) {
    Object.assign(query, MongoQueryBuilder.buildTextSearch(
      ['name', 'email', 'description'],
      criteria.search,
    ));
  }

  // Filtro por data
  if (criteria.startDate || criteria.endDate) {
    Object.assign(query, MongoQueryBuilder.buildDateRange(
      'createdAt',
      criteria.startDate,
      criteria.endDate,
    ));
  }

  // Filtro por array
  if (criteria.tags && criteria.tags.length > 0) {
    Object.assign(query, MongoQueryBuilder.buildArrayFilter(
      'tags',
      criteria.tags,
      '$all',
    ));
  }

  // Filtro por existência
  if (criteria.hasAvatar !== undefined) {
    Object.assign(query, MongoQueryBuilder.buildExistsFilter(
      'avatar',
      criteria.hasAvatar,
    ));
  }

  // Filtro aninhado
  if (criteria.addressCity) {
    Object.assign(query, MongoQueryBuilder.buildNestedFilter(
      'address.city',
      { $regex: criteria.addressCity, $options: 'i' },
    ));
  }

  return this.model.find(query).exec();
}
```

### Operadores de Query
```typescript
// Operadores de comparação
const users = await userModel.find({
  age: { $gt: 18, $lt: 65 }, // Greater than, Less than
  salary: { $gte: 3000, $lte: 10000 }, // Greater/Less than or equal
  status: { $ne: 'inactive' }, // Not equal
});

// Operadores lógicos
const users = await userModel.find({
  $or: [
    { role: 'admin' },
    { role: 'moderator' },
  ],
  $and: [
    { isActive: true },
    { emailVerified: true },
  ],
  $nor: [
    { status: 'banned' },
    { isDeleted: true },
  ],
  $not: { age: { $lt: 18 } },
});

// Operadores de array
const users = await userModel.find({
  tags: { $in: ['premium', 'vip'] }, // Contém algum
  tags: { $nin: ['blocked', 'spam'] }, // Não contém
  tags: { $all: ['user', 'verified'] }, // Contém todos
  tags: { $size: 3 }, // Tamanho específico
  tags: { $elemMatch: { $regex: '^premium' } }, // Match em elementos
});

// Operadores de elemento
const users = await userModel.find({
  email: { $exists: true }, // Campo existe
  phone: { $type: 'string' }, // Tipo específico
});

// Expressões regulares
const users = await userModel.find({
  name: { $regex: '^Jo', $options: 'i' }, // Case insensitive
  email: { $regex: '@gmail\\.com$' },
});
```

### Paginação Avançada com Cursor
```typescript
// repositories/paginated.repository.ts
export class PaginatedRepository<T extends Document> {
  constructor(protected readonly model: Model<T>) {}

  async paginateWithCursor(
    filter: FilterQuery<T> = {},
    limit: number = 10,
    cursor?: string,
    sortField: string = '_id',
    sortOrder: 1 | -1 = 1,
  ): Promise<{
    data: T[];
    nextCursor: string | null;
    hasNextPage: boolean;
  }> {
    const query: any = { ...filter };

    // Aplicar cursor
    if (cursor) {
      query[sortField] = sortOrder === 1
        ? { $gt: cursor }
        : { $lt: cursor };
    }

    // Buscar dados (limit + 1 para verificar se há mais páginas)
    const data = await this.model
      .find(query)
      .sort({ [sortField]: sortOrder })
      .limit(limit + 1)
      .exec();

    // Verificar se há mais páginas
    const hasNextPage = data.length > limit;
    
    // Remover item extra se houver
    if (hasNextPage) {
      data.pop();
    }

    // Calcular próximo cursor
    const nextCursor = hasNextPage && data.length > 0
      ? data[data.length - 1][sortField].toString()
      : null;

    return {
      data,
      nextCursor,
      hasNextPage,
    };
  }
}
```

---

## Aggregation Framework

### Pipeline Básico
```typescript
// repositories/analytics.repository.ts
export class AnalyticsRepository {
  async getUserAnalytics(startDate: Date, endDate: Date): Promise<any[]> {
    return this.userModel.aggregate([
      // Stage 1: Filtrar por data
      {
        $match: {
          createdAt: {
            $gte: startDate,
            $lte: endDate,
          },
        },
      },
      
      // Stage 2: Agrupar por mês e status
      {
        $group: {
          _id: {
            year: { $year: '$createdAt' },
            month: { $month: '$createdAt' },
            status: '$status',
          },
          count: { $sum: 1 },
          totalAge: { $sum: '$age' },
          avgAge: { $avg: '$age' },
        },
      },
      
      // Stage 3: Ordenar
      {
        $sort: {
          '_id.year': 1,
          '_id.month': 1,
          '_id.status': 1,
        },
      },
      
      // Stage 4: Formatar resultado
      {
        $project: {
          _id: 0,
          year: '$_id.year',
          month: '$_id.month',
          status: '$_id.status',
          count: 1,
          avgAge: { $round: ['$avgAge', 2] },
          date: {
            $dateToString: {
              format: '%Y-%m',
              date: {
                $dateFromParts: {
                  year: '$_id.year',
                  month: '$_id.month',
                },
              },
            },
          },
        },
      },
      
      // Stage 5: Agrupar novamente para pivot
      {
        $group: {
          _id: '$date',
          year: { $first: '$year' },
          month: { $first: '$month' },
          stats: {
            $push: {
              k: '$status',
              v: {
                count: '$count',
                avgAge: '$avgAge',
              },
            },
          },
        },
      },
      
      // Stage 6: Converter array para objeto
      {
        $project: {
          _id: 0,
          date: '$_id',
          year: 1,
          month: 1,
          stats: { $arrayToObject: '$stats' },
        },
      },
    ]).exec();
  }
}
```

### Lookup (Joins)
```typescript
async getUsersWithOrders(): Promise<any[]> {
  return this.userModel.aggregate([
    // Lookup para orders
    {
      $lookup: {
        from: 'orders',
        localField: '_id',
        foreignField: 'userId',
        as: 'orders',
      },
    },
    
    // Lookup para order items
    {
      $lookup: {
        from: 'orderitems',
        localField: 'orders._id',
        foreignField: 'orderId',
        as: 'orderItems',
      },
    },
    
    // Lookup para produtos
    {
      $lookup: {
        from: 'products',
        localField: 'orderItems.productId',
        foreignField: '_id',
        as: 'products',
      },
    },
    
    // Desnormalizar orders
    {
      $unwind: {
        path: '$orders',
        preserveNullAndEmptyArrays: true,
      },
    },
    
    // Agrupar estatísticas
    {
      $group: {
        _id: '$_id',
        name: { $first: '$name' },
        email: { $first: '$email' },
        totalOrders: { $sum: 1 },
        totalAmount: { $sum: '$orders.total' },
        avgOrderValue: { $avg: '$orders.total' },
        products: { $addToSet: '$products' },
        lastOrderDate: { $max: '$orders.createdAt' },
      },
    },
    
    // Formatar resultado
    {
      $project: {
        _id: 0,
        userId: '$_id',
        name: 1,
        email: 1,
        totalOrders: 1,
        totalAmount: { $round: ['$totalAmount', 2] },
        avgOrderValue: { $round: ['$avgOrderValue', 2] },
        uniqueProducts: { $size: '$products' },
        lastOrderDate: 1,
        customerType: {
          $switch: {
            branches: [
              {
                case: { $gte: ['$totalAmount', 10000] },
                then: 'Premium',
              },
              {
                case: { $gte: ['$totalAmount', 5000] },
                then: 'Gold',
              },
              {
                case: { $gte: ['$totalAmount', 1000] },
                then: 'Silver',
              },
            ],
            default: 'Bronze',
          },
        },
      },
    },
    
    // Ordenar por total gasto
    {
      $sort: { totalAmount: -1 },
    },
  ]).exec();
}
```

### Aggregation com Facet (Multi-resultados)
```typescript
async getDashboardStats(): Promise<any> {
  return this.userModel.aggregate([
    // Pipeline comum
    {
      $match: {
        isActive: true,
      },
    },
    
    // Facet para múltiplos resultados
    {
      $facet: {
        // Estatísticas gerais
        overallStats: [
          {
            $group: {
              _id: null,
              totalUsers: { $sum: 1 },
              avgAge: { $avg: '$age' },
              minAge: { $min: '$age' },
              maxAge: { $max: '$age' },
            },
          },
        ],
        
        // Estatísticas por role
        statsByRole: [
          {
            $group: {
              _id: '$role',
              count: { $sum: 1 },
              avgAge: { $avg: '$age' },
            },
          },
          {
            $project: {
              _id: 0,
              role: '$_id',
              count: 1,
              avgAge: { $round: ['$avgAge', 2] },
            },
          },
          { $sort: { count: -1 } },
        ],
        
        // Distribuição por idade
        ageDistribution: [
          {
            $bucket: {
              groupBy: '$age',
              boundaries: [0, 18, 25, 35, 50, 65, 100],
              default: 'Other',
              output: {
                count: { $sum: 1 },
                avgSalary: { $avg: '$salary' },
              },
            },
          },
        ],
        
        // Usuários recentes
        recentUsers: [
          {
            $sort: { createdAt: -1 },
          },
          {
            $limit: 10,
          },
          {
            $project: {
              _id: 0,
              name: 1,
              email: 1,
              role: 1,
              createdAt: 1,
            },
          },
        ],
        
        // Crescimento mensal
        monthlyGrowth: [
          {
            $group: {
              _id: {
                year: { $year: '$createdAt' },
                month: { $month: '$createdAt' },
              },
              count: { $sum: 1 },
            },
          },
          {
            $sort: {
              '_id.year': 1,
              '_id.month': 1,
            },
          },
          {
            $project: {
              _id: 0,
              year: '$_id.year',
              month: '$_id.month',
              count: 1,
            },
          },
        ],
      },
    },
    
    // Desestruturar facet
    {
      $project: {
        overallStats: { $arrayElemAt: ['$overallStats', 0] },
        statsByRole: 1,
        ageDistribution: 1,
        recentUsers: 1,
        monthlyGrowth: 1,
      },
    },
  ]).exec();
}
```

### Performance Tips para Aggregation
```typescript
// Otimização de aggregation pipeline
export const optimizedAggregation = [
  // 1. Colocar $match no início para reduzir documentos
  {
    $match: {
      isActive: true,
      createdAt: { $gte: new Date('2023-01-01') },
    },
  },
  
  // 2. Usar $project para selecionar apenas campos necessários
  {
    $project: {
      name: 1,
      email: 1,
      role: 1,
      createdAt: 1,
      metadata: 1,
    },
  },
  
  // 3. Usar $lookup com pipeline para filtragem
  {
    $lookup: {
      from: 'orders',
      let: { userId: '$_id' },
      pipeline: [
        {
          $match: {
            $expr: {
              $and: [
                { $eq: ['$userId', '$$userId'] },
                { $gte: ['$total', 100] },
              ],
            },
          },
        },
        { $limit: 5 }, // Limitar resultados do lookup
      ],
      as: 'recentOrders',
    },
  },
  
  // 4. Usar $unwind com preserveNullAndEmptyArrays
  {
    $unwind: {
      path: '$recentOrders',
      preserveNullAndEmptyArrays: true,
    },
  },
  
  // 5. Usar $facet para operações paralelas
  {
    $facet: {
      // ... facets
    },
  },
];
```

---

## Indexes e Performance

### Tipos de Índices
```typescript
// Configuração de índices no schema
@Schema()
export class Product {
  @Prop({ index: true }) // Índice simples
  name: string;

  @Prop({ unique: true }) // Índice único
  sku: string;

  @Prop({
    index: true,
    sparse: true, // Índice esparso (ignora null/undefined)
  })
  category: string;

  @Prop({
    type: Map,
    of: Number,
  })
  ratings: Map<string, number>;
}

// Índices compostos
ProductSchema.index({ category: 1, price: -1 });
ProductSchema.index({ name: 'text', description: 'text' }); // Text index

// Índice TTL (Time To Live)
ProductSchema.index(
  { createdAt: 1 },
  { expireAfterSeconds: 60 * 60 * 24 * 30 }, // 30 dias
);

// Índice parcial
ProductSchema.index(
  { isActive: 1 },
  { partialFilterExpression: { isActive: true } },
);

// Índice 2dsphere para geolocalização
const LocationSchema = new Schema({
  type: { type: String, enum: ['Point'], default: 'Point' },
  coordinates: { type: [Number], default: [0, 0] },
});

LocationSchema.index({ coordinates: '2dsphere' });
```

### Gerenciamento de Índices
```typescript
// services/index-manager.service.ts
import { Injectable, Logger } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';

@Injectable()
export class IndexManagerService {
  private readonly logger = new Logger(IndexManagerService.name);

  constructor(
    @InjectModel('User') private userModel: Model<any>,
    @InjectModel('Product') private productModel: Model<any>,
  ) {}

  async createIndexes(): Promise<void> {
    try {
      // Criar índices para User
      await this.userModel.createIndexes();
      
      // Criar índices específicos
      await this.userModel.collection.createIndex(
        { email: 1 },
        { unique: true, background: true },
      );
      
      await this.userModel.collection.createIndex(
        { 'address.location': '2dsphere' },
        { background: true },
      );

      // Criar índices para Product
      await this.productModel.collection.createIndex(
        { name: 'text', description: 'text' },
        { weights: { name: 10, description: 5 }, background: true },
      );

      this.logger.log('Indexes created successfully');
    } catch (error) {
      this.logger.error('Error creating indexes', error);
      throw error;
    }
  }

  async getIndexStats(): Promise<any> {
    const userStats = await this.userModel.collection.indexes();
    const productStats = await this.productModel.collection.indexes();
    
    return {
      users: {
        count: userStats.length,
        indexes: userStats,
      },
      products: {
        count: productStats.length,
        indexes: productStats,
      },
    };
  }

  async dropIndex(indexName: string, modelName: string): Promise<void> {
    const model = this.getModelByName(modelName);
    await model.collection.dropIndex(indexName);
  }

  async rebuildIndexes(): Promise<void> {
    // Reconstruir todos os índices
    await this.userModel.collection.reIndex();
    await this.productModel.collection.reIndex();
  }

  private getModelByName(name: string): Model<any> {
    switch (name) {
      case 'User':
        return this.userModel;
      case 'Product':
        return this.productModel;
      default:
        throw new Error(`Model ${name} not found`);
    }
  }
}
```

### Performance Monitoring
```typescript
// middleware/query-logger.middleware
import { Injectable, Logger } from '@nestjs/common';
import { Connection } from 'mongoose';

@Injectable()
export class QueryLogger {
  private readonly logger = new Logger('MongoDB');

  constructor(@InjectConnection() private connection: Connection) {
    this.setupQueryLogging();
  }

  private setupQueryLogging(): void {
    const connection = this.connection;

    // Log de queries lentas
    connection.on('query', (query) => {
      if (query.duration > 100) { // Queries > 100ms
        this.logger.warn(`Slow query detected: ${JSON.stringify(query)}`);
      }
    });

    // Log de erros
    connection.on('error', (error) => {
      this.logger.error(`MongoDB connection error: ${error.message}`);
    });

    // Log de conexão
    connection.on('connected', () => {
      this.logger.log('MongoDB connected successfully');
    });

    // Log de desconexão
    connection.on('disconnected', () => {
      this.logger.warn('MongoDB disconnected');
    });
  }

  async getPerformanceStats(): Promise<any> {
    const adminDb = this.connection.db.admin();
    
    const [serverStatus, currentOp, collStats] = await Promise.all([
      adminDb.serverStatus(),
      this.connection.db.command({ currentOp: 1 }),
      this.connection.db.command({ collStats: 'users' }),
    ]);

    return {
      connections: serverStatus.connections,
      memory: serverStatus.mem,
      operations: currentOp.inprog,
      collectionStats: collStats,
    };
  }
}
```

### Otimização de Queries
```typescript
// Query optimization techniques
export class QueryOptimizer {
  // 1. Use projection para retornar apenas campos necessários
  static optimizeProjection(fields: string[]): any {
    const projection: any = {};
    fields.forEach(field => {
      projection[field] = 1;
    });
    return projection;
  }

  // 2. Use lean() para queries de leitura
  static useLeanQuery<T>(query: any): Promise<T[]> {
    return query.lean().exec();
  }

  // 3. Use batch operations para múltiplos documentos
  static async bulkUpdate(
    model: Model<any>,
    updates: Array<{ filter: any; update: any }>,
  ): Promise<any> {
    const bulkOps = updates.map(({ filter, update }) => ({
      updateOne: {
        filter,
        update,
        upsert: false,
      },
    }));

    return model.bulkWrite(bulkOps);
  }

  // 4. Use cursor para grandes datasets
  static async streamLargeDataset(
    model: Model<any>,
    query: any,
    batchSize: number = 1000,
    callback: (batch: any[]) => Promise<void>,
  ): Promise<void> {
    const cursor = model.find(query).cursor({ batchSize });

    for await (const doc of cursor) {
      await callback([doc]);
    }
  }

  // 5. Cache frequent queries
  static async cachedQuery<T>(
    key: string,
    ttl: number,
    queryFn: () => Promise<T>,
    cacheService: any,
  ): Promise<T> {
    const cached = await cacheService.get(key);
    
    if (cached) {
      return cached;
    }

    const result = await queryFn();
    await cacheService.set(key, result, ttl);
    
    return result;
  }
}
```

---

## Transações

### Configuração para Transações
```typescript
// database/transaction.service.ts
import { Injectable, Logger } from '@nestjs/common';
import { InjectConnection } from '@nestjs/mongoose';
import { Connection, ClientSession } from 'mongoose';

@Injectable()
export class TransactionService {
  private readonly logger = new Logger(TransactionService.name);

  constructor(@InjectConnection() private connection: Connection) {}

  async withTransaction<T>(
    operation: (session: ClientSession) => Promise<T>,
  ): Promise<T> {
    const session = await this.connection.startSession();
    
    try {
      session.startTransaction();
      
      const result = await operation(session);
      
      await session.commitTransaction();
      return result;
    } catch (error) {
      await session.abortTransaction();
      this.logger.error('Transaction aborted', error);
      throw error;
    } finally {
      session.endSession();
    }
  }

  async withRetry<T>(
    operation: (session: ClientSession) => Promise<T>,
    maxRetries: number = 3,
  ): Promise<T> {
    for (let i = 0; i < maxRetries; i++) {
      try {
        return await this.withTransaction(operation);
      } catch (error) {
        if (i === maxRetries - 1) {
          throw error;
        }
        
        // Wait before retry (exponential backoff)
        await new Promise(resolve => 
          setTimeout(resolve, Math.pow(2, i) * 1000)
        );
        
        this.logger.warn(`Retrying transaction (attempt ${i + 1})`);
      }
    }
    
    throw new Error('Max retries exceeded');
  }
}
```

### Exemplo de Transação Completa
```typescript
// services/order.service.ts
@Injectable()
export class OrderService {
  constructor(
    private readonly transactionService: TransactionService,
    @InjectModel(Order.name) private orderModel: Model<OrderDocument>,
    @InjectModel(Product.name) private productModel: Model<ProductDocument>,
    @InjectModel(User.name) private userModel: Model<UserDocument>,
  ) {}

  async createOrderWithTransaction(orderData: CreateOrderDto): Promise<Order> {
    return this.transactionService.withTransaction(async (session) => {
      // 1. Verificar estoque de todos os produtos
      const productUpdates = [];
      const products = [];
      
      for (const item of orderData.items) {
        const product = await this.productModel
          .findById(item.productId)
          .session(session)
          .exec();
        
        if (!product) {
          throw new NotFoundException(
            `Product ${item.productId} not found`,
          );
        }
        
        if (product.stock < item.quantity) {
          throw new BadRequestException(
            `Insufficient stock for product ${product.name}`,
          );
        }
        
        productUpdates.push({
          filter: { _id: product._id },
          update: {
            $inc: { stock: -item.quantity },
            $set: { updatedAt: new Date() },
          },
        });
        
        products.push(product);
      }

      // 2. Atualizar estoque dos produtos
      await this.productModel.bulkWrite(productUpdates, { session });

      // 3. Calcular total do pedido
      const total = products.reduce((sum, product, index) => {
        return sum + product.price * orderData.items[index].quantity;
      }, 0);

      // 4. Criar o pedido
      const order = new this.orderModel({
        ...orderData,
        total,
        status: 'pending',
      });
      
      await order.save({ session });

      // 5. Atualizar histórico do usuário
      await this.userModel.findByIdAndUpdate(
        orderData.userId,
        {
          $push: {
            orders: order._id,
            orderHistory: {
              orderId: order._id,
              total,
              date: new Date(),
            },
          },
          $inc: { totalSpent: total },
        },
        { session },
      );

      // 6. Registrar no log de transações
      await this.createTransactionLog(order, products, session);

      return order;
    });
  }

  private async createTransactionLog(
    order: Order,
    products: Product[],
    session: ClientSession,
  ): Promise<void> {
    const logEntry = {
      orderId: order._id,
      userId: order.userId,
      total: order.total,
      products: products.map(p => p._id),
      timestamp: new Date(),
      type: 'order_created',
    };

    // Inserir em collection separada para logs
    await session.client
      .db()
      .collection('transaction_logs')
      .insertOne(logEntry, { session });
  }
}
```

### Transações Distribuídas (Multi-document)
```typescript
// services/payment.service.ts
@Injectable()
export class PaymentService {
  async processPayment(paymentData: any): Promise<any> {
    return this.transactionService.withTransaction(async (session) => {
      // Operação 1: Criar registro de pagamento
      const payment = await this.paymentModel.create([paymentData], { session });

      // Operação 2: Atualizar saldo da conta
      await this.accountModel.findByIdAndUpdate(
        paymentData.accountId,
        {
          $inc: { balance: -paymentData.amount },
          $push: {
            transactions: {
              paymentId: payment[0]._id,
              amount: paymentData.amount,
              type: 'debit',
              date: new Date(),
            },
          },
        },
        { session },
      );

      // Operação 3: Atualizar saldo do destinatário
      await this.accountModel.findByIdAndUpdate(
        paymentData.recipientId,
        {
          $inc: { balance: paymentData.amount },
          $push: {
            transactions: {
              paymentId: payment[0]._id,
              amount: paymentData.amount,
              type: 'credit',
              date: new Date(),
            },
          },
        },
        { session },
      );

      // Operação 4: Registrar no ledger
      await this.ledgerModel.create(
        [
          {
            fromAccount: paymentData.accountId,
            toAccount: paymentData.recipientId,
            amount: paymentData.amount,
            paymentId: payment[0]._id,
            timestamp: new Date(),
          },
        ],
        { session },
      );

      return payment[0];
    });
  }
}
```

---

## Relacionamentos

### Embedding vs Referencing
```typescript
// Exemplo: Blog com posts e comentários

// 1. EMBEDDING (Para dados frequentemente acessados juntos)
@Schema()
export class Post {
  @Prop({ required: true })
  title: string;

  @Prop({ required: true })
  content: string;

  @Prop({ type: [CommentSchema] }) // Comentários embutidos
  comments: Comment[];

  @Prop({ type: AuthorSchema }) // Autor embutido
  author: Author;
}

// 2. REFERENCING (Para relacionamentos muitos-para-muitos ou dados grandes)
@Schema()
export class User {
  @Prop({ type: [Types.ObjectId], ref: 'Post' }) // Referência para posts
  posts: Types.ObjectId[];

  @Prop({ type: [Types.ObjectId], ref: 'Comment' }) // Referência para comentários
  comments: Types.ObjectId[];
}
```

### Population (Referências)
```typescript
// repositories/post.repository.ts
export class PostRepository {
  async findPostWithComments(postId: string): Promise<PostDocument> {
    return this.postModel
      .findById(postId)
      .populate({
        path: 'comments',
        select: 'content author createdAt',
        options: { sort: { createdAt: -1 } },
        populate: {
          path: 'author',
          select: 'name avatar',
        },
      })
      .populate('author', 'name email avatar')
      .exec();
  }

  async findPostsByUserWithDetails(userId: string): Promise<PostDocument[]> {
    return this.postModel
      .find({ author: userId })
      .populate({
        path: 'comments',
        populate: {
          path: 'author',
          select: 'name',
        },
      })
      .populate('author', 'name email')
      .sort({ createdAt: -1 })
      .exec();
  }

  // Population com filtros
  async findPostsWithActiveComments(): Promise<PostDocument[]> {
    return this.postModel
      .find()
      .populate({
        path: 'comments',
        match: { isActive: true }, // Filtra apenas comentários ativos
        select: 'content author',
      })
      .exec();
  }
}
```

### Virtual Population
```typescript
// schemas/user.schema.ts
UserSchema.virtual('postCount', {
  ref: 'Post',
  localField: '_id',
  foreignField: 'author',
  count: true, // Contagem em vez de documentos
});

UserSchema.virtual('posts', {
  ref: 'Post',
  localField: '_id',
  foreignField: 'author',
  options: { sort: { createdAt: -1 } },
});

// Configuração para incluir virtuals
UserSchema.set('toObject', { virtuals: true });
UserSchema.set('toJSON', { virtuals: true });

// Uso no repositório
async getUserWithPosts(userId: string): Promise<UserDocument> {
  return this.userModel
    .findById(userId)
    .populate('posts')
    .populate('postCount')
    .exec();
}
```

### Relacionamentos Bidirecionais
```typescript
// Middleware para manter consistência
UserSchema.pre('save', function (next) {
  // Atualizar referências em outras collections
  next();
});

UserSchema.post('remove', async function (doc) {
  // Remover posts do usuário
  await mongoose.model('Post').deleteMany({ author: doc._id });
  
  // Remover comentários do usuário
  await mongoose.model('Comment').deleteMany({ author: doc._id });
});

// Serviço para gerenciar relacionamentos
@Injectable()
export class RelationshipService {
  async addPostToUser(userId: string, postId: string): Promise<void> {
    await Promise.all([
      this.userModel.findByIdAndUpdate(userId, {
        $push: { posts: postId },
      }),
      this.postModel.findByIdAndUpdate(postId, {
        author: userId,
      }),
    ]);
  }

  async removePostFromUser(userId: string, postId: string): Promise<void> {
    await Promise.all([
      this.userModel.findByIdAndUpdate(userId, {
        $pull: { posts: postId },
      }),
      this.postModel.findByIdAndUpdate(postId, {
        $unset: { author: 1 },
      }),
    ]);
  }
}
```

---

## Recursos Avançados

### Full Text Search
```typescript
// schemas/product.schema.ts
ProductSchema.index(
  { title: 'text', description: 'text', tags: 'text' },
  {
    weights: {
      title: 10,
      description: 5,
      tags: 3,
    },
    name: 'TextIndex',
  },
);

// repositories/product.repository.ts
async searchProducts(
  query: string,
  filters?: any,
): Promise<ProductDocument[]> {
  const searchQuery: any = {
    $text: { $search: query },
  };

  if (filters?.category) {
    searchQuery.category = filters.category;
  }

  if (filters?.priceRange) {
    searchQuery.price = {
      $gte: filters.priceRange.min,
      $lte: filters.priceRange.max,
    };
  }

  return this.productModel
    .find(searchQuery)
    .select({
      score: { $meta: 'textScore' }, // Incluir score de relevância
    })
    .sort({
      score: { $meta: 'textScore' }, // Ordenar por relevância
    })
    .exec();
}
```

### Geospatial Queries
```typescript
// schemas/location.schema.ts
@Schema()
export class Location {
  @Prop({
    type: {
      type: String,
      enum: ['Point'],
      required: true,
    },
    coordinates: {
      type: [Number],
      required: true,
    },
  })
  geometry: {
    type: string;
    coordinates: [number, number]; // [longitude, latitude]
  };

  @Prop({ required: true })
  address: string;

  @Prop()
  city: string;

  @Prop()
  country: string;
}

LocationSchema.index({ geometry: '2dsphere' });

// repositories/location.repository.ts
export class LocationRepository {
  async findNearbyLocations(
    coordinates: [number, number],
    maxDistance: number = 1000, // metros
  ): Promise<LocationDocument[]> {
    return this.locationModel
      .find({
        geometry: {
          $near: {
            $geometry: {
              type: 'Point',
              coordinates,
            },
            $maxDistance: maxDistance,
          },
        },
      })
      .exec();
  }

  async findLocationsWithinPolygon(polygon: number[][][]): Promise<LocationDocument[]> {
    return this.locationModel
      .find({
        geometry: {
          $geoWithin: {
            $geometry: {
              type: 'Polygon',
              coordinates: polygon,
            },
          },
        },
      })
      .exec();
  }

  async calculateDistance(
    point1: [number, number],
    point2: [number, number],
  ): Promise<number> {
    const result = await this.locationModel.aggregate([
      {
        $geoNear: {
          near: { type: 'Point', coordinates: point1 },
          distanceField: 'distance',
          spherical: true,
        },
      },
      {
        $match: {
          'geometry.coordinates': point2,
        },
      },
      {
        $project: {
          distance: 1,
        },
      },
    ]).exec();

    return result[0]?.distance || 0;
  }
}
```

### Change Streams (Real-time)
```typescript
// services/change-stream.service.ts
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { InjectConnection } from '@nestjs/mongoose';
import { Connection, ChangeStream } from 'mongoose';

@Injectable()
export class ChangeStreamService implements OnModuleInit, OnModuleDestroy {
  private changeStreams: ChangeStream[] = [];

  constructor(@InjectConnection() private connection: Connection) {}

  async onModuleInit(): Promise<void> {
    await this.setupChangeStreams();
  }

  async onModuleDestroy(): Promise<void> {
    await this.cleanupChangeStreams();
  }

  private async setupChangeStreams(): Promise<void> {
    // Watch users collection
    const userStream = this.connection
      .collection('users')
      .watch([], { fullDocument: 'updateLookup' });

    userStream.on('change', (change) => {
      this.handleUserChange(change);
    });

    this.changeStreams.push(userStream);

    // Watch orders collection
    const orderStream = this.connection
      .collection('orders')
      .watch([], { fullDocument: 'updateLookup' });

    orderStream.on('change', (change) => {
      this.handleOrderChange(change);
    });

    this.changeStreams.push(orderStream);
  }

  private handleUserChange(change: any): void {
    switch (change.operationType) {
      case 'insert':
        console.log('New user created:', change.fullDocument);
        // Enviar para WebSocket, criar perfil, etc.
        break;
      case 'update':
        console.log('User updated:', change.fullDocument);
        break;
      case 'delete':
        console.log('User deleted:', change.documentKey);
        break;
    }
  }

  private handleOrderChange(change: any): void {
    // Lógica específica para mudanças em orders
    // Enviar notificações, atualizar cache, etc.
  }

  private async cleanupChangeStreams(): Promise<void> {
    for (const stream of this.changeStreams) {
      await stream.close();
    }
  }

  async watchCollection(
    collectionName: string,
    callback: (change: any) => void,
  ): Promise<ChangeStream> {
    const stream = this.connection
      .collection(collectionName)
      .watch([], { fullDocument: 'updateLookup' });

    stream.on('change', callback);
    this.changeStreams.push(stream);

    return stream;
  }
}
```

### Versioning com Mongoose
```typescript
// schemas/versioned.schema.ts
@Schema({
  timestamps: true,
  versionKey: 'version', // Usar campo 'version' em vez de '__v'
  optimisticConcurrency: true, // Habilitar controle de concorrência
})
export class VersionedDocument {
  @Prop({ default: 0 })
  version: number;

  @Prop({ required: true })
  data: string;
}

// Serviço com controle de concorrência
@Injectable()
export class VersionedService {
  async updateWithVersion(
    id: string,
    updateData: any,
    expectedVersion: number,
  ): Promise<VersionedDocument> {
    const document = await this.model.findById(id);
    
    if (!document) {
      throw new NotFoundException('Document not found');
    }

    // Verificar versão
    if (document.version !== expectedVersion) {
      throw new ConflictException('Document has been modified by another user');
    }

    // Atualizar com incremento de versão
    document.data = updateData.data;
    document.version += 1;

    return document.save();
  }

  async atomicUpdate(
    id: string,
    updateFn: (doc: any) => any,
  ): Promise<VersionedDocument> {
    let retries = 3;
    
    while (retries > 0) {
      try {
        const document = await this.model.findById(id);
        
        if (!document) {
          throw new NotFoundException('Document not found');
        }

        // Aplicar função de update
        const updatedData = updateFn(document.toObject());
        
        // Salvar com controle de concorrência
        document.set(updatedData);
        return await document.save();
      } catch (error) {
        if (error.name === 'VersionError') {
          retries--;
          if (retries === 0) throw error;
          
          // Aguardar um pouco antes de tentar novamente
          await new Promise(resolve => setTimeout(resolve, 100));
          continue;
        }
        throw error;
      }
    }
    
    throw new Error('Max retries exceeded');
  }
}
```

---

## Testes com MongoDB

### Configuração para Testes
```typescript
// test/setup.ts
import { MongoMemoryServer } from 'mongodb-memory-server';
import { MongooseModule, MongooseModuleOptions } from '@nestjs/mongoose';
import { DynamicModule } from '@nestjs/common';

let mongod: MongoMemoryServer;

export const rootMongooseTestModule = (
  options: MongooseModuleOptions = {},
): DynamicModule => {
  return MongooseModule.forRootAsync({
    useFactory: async () => {
      mongod = await MongoMemoryServer.create();
      const mongoUri = mongod.getUri();
      
      return {
        uri: mongoUri,
        ...options,
      };
    },
  });
};

export const closeInMongodConnection = async (): Promise<void> => {
  if (mongod) {
    await mongod.stop();
  }
};

// test/test-database.module.ts
@Module({
  imports: [
    rootMongooseTestModule(),
    MongooseModule.forFeature([
      { name: User.name, schema: UserSchema },
      { name: Product.name, schema: ProductSchema },
    ]),
  ],
  exports: [MongooseModule],
})
export class TestDatabaseModule {}
```

### Testes Unitários
```typescript
// users.service.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { getModelToken } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { UsersService } from './users.service';
import { User, UserDocument } from './schemas/user.schema';
import { CreateUserDto } from './dto/create-user.dto';
import { ConflictException } from '@nestjs/common';

describe('UsersService', () => {
  let service: UsersService;
  let model: Model<UserDocument>;

  const mockUser = {
    _id: '507f1f77bcf86cd799439011',
    name: 'John Doe',
    email: 'john@example.com',
    password: 'hashedpassword',
    isActive: true,
    save: jest.fn(),
  };

  const mockUserModel = {
    new: jest.fn().mockResolvedValue(mockUser),
    constructor: jest.fn().mockResolvedValue(mockUser),
    find: jest.fn(),
    findOne: jest.fn(),
    findById: jest.fn(),
    create: jest.fn(),
    findByIdAndUpdate: jest.fn(),
    findByIdAndDelete: jest.fn(),
    exec: jest.fn(),
  };

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getModelToken(User.name),
          useValue: mockUserModel,
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
    model = module.get<Model<UserDocument>>(getModelToken(User.name));
  });

  describe('create', () => {
    it('should create a user successfully', async () => {
      const createUserDto: CreateUserDto = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123',
      };

      jest.spyOn(model, 'findOne').mockReturnValue({
        exec: jest.fn().mockResolvedValue(null),
      } as any);

      jest.spyOn(model, 'create').mockResolvedValue(mockUser as any);

      const result = await service.create(createUserDto);
      
      expect(model.findOne).toHaveBeenCalledWith({
        email: createUserDto.email,
      });
      expect(result).toEqual(mockUser);
    });

    it('should throw ConflictException if email exists', async () => {
      const createUserDto: CreateUserDto = {
        name: 'John Doe',
        email: 'john@example.com',
        password: 'password123',
      };

      jest.spyOn(model, 'findOne').mockReturnValue({
        exec: jest.fn().mockResolvedValue(mockUser),
      } as any);

      await expect(service.create(createUserDto))
        .rejects
        .toThrow(ConflictException);
    });
  });

  describe('findAll', () => {
    it('should return an array of users', async () => {
      const users = [mockUser, mockUser];
      
      jest.spyOn(model, 'find').mockReturnValue({
        exec: jest.fn().mockResolvedValue(users),
      } as any);

      const result = await service.findAll();
      
      expect(model.find).toHaveBeenCalled();
      expect(result).toEqual(users);
    });
  });
});
```

### Testes de Integração
```typescript
// users.integration.spec.ts
import { Test, TestingModule } from '@nestjs/testing';
import { INestApplication } from '@nestjs/common';
import * as request from 'supertest';
import { AppModule } from '../app.module';
import { TestDatabaseModule, closeInMongodConnection } from '../test/setup';
import { CreateUserDto } from './dto/create-user.dto';

describe('UsersController (integration)', () => {
  let app: INestApplication;

  beforeAll(async () => {
    const moduleFixture: TestingModule = await Test.createTestingModule({
      imports: [AppModule, TestDatabaseModule],
    }).compile();

    app = moduleFixture.createNestApplication();
    await app.init();
  });

  afterAll(async () => {
    await app.close();
    await closeInMongodConnection();
  });

  describe('/users (POST)', () => {
    it('should create a user', () => {
      const createUserDto: CreateUserDto = {
        name: 'Test User',
        email: 'test@example.com',
        password: 'password123',
      };

      return request(app.getHttpServer())
        .post('/users')
        .send(createUserDto)
        .expect(201)
        .expect((res) => {
          expect(res.body).toHaveProperty('_id');
          expect(res.body.name).toBe(createUserDto.name);
          expect(res.body.email).toBe(createUserDto.email);
          expect(res.body.password).toBeUndefined(); // Password não deve ser retornado
        });
    });

    it('should return 409 for duplicate email', async () => {
      const createUserDto: CreateUserDto = {
        name: 'Test User',
        email: 'duplicate@example.com',
        password: 'password123',
      };

      // Primeira criação
      await request(app.getHttpServer())
        .post('/users')
        .send(createUserDto);

      // Segunda tentativa com mesmo email
      return request(app.getHttpServer())
        .post('/users')
        .send(createUserDto)
        .expect(409);
    });
  });

  describe('/users (GET)', () => {
    it('should return array of users', async () => {
      // Criar alguns usuários primeiro
      const users = [
        { name: 'User 1', email: 'user1@example.com', password: 'pass1' },
        { name: 'User 2', email: 'user2@example.com', password: 'pass2' },
      ];

      for (const user of users) {
        await request(app.getHttpServer())
          .post('/users')
          .send(user);
      }

      return request(app.getHttpServer())
        .get('/users')
        .expect(200)
        .expect((res) => {
          expect(Array.isArray(res.body)).toBe(true);
          expect(res.body.length).toBeGreaterThanOrEqual(2);
        });
    });

    it('should filter users by query parameters', async () => {
      return request(app.getHttpServer())
        .get('/users')
        .query({ isActive: true, limit: 5 })
        .expect(200)
        .expect((res) => {
          expect(res.body).toHaveProperty('data');
          expect(res.body).toHaveProperty('total');
          expect(res.body).toHaveProperty('page');
          expect(res.body).toHaveProperty('pages');
        });
    });
  });
});
```

### Factory para Testes
```typescript
// test/factories/user.factory.ts
import { faker } from '@faker-js/faker';
import { User } from '../../schemas/user.schema';

export class UserFactory {
  static create(overrides: Partial<User> = {}): Partial<User> {
    return {
      name: faker.person.fullName(),
      email: faker.internet.email().toLowerCase(),
      password: faker.internet.password(),
      role: 'user',
      isActive: true,
      ...overrides,
    };
  }

  static createMany(count: number, overrides: Partial<User> = {}): Partial<User>[] {
    return Array.from({ length: count }, () => this.create(overrides));
  }

  static createAdmin(overrides: Partial<User> = {}): Partial<User> {
    return this.create({
      role: 'admin',
      ...overrides,
    });
  }
}

// Uso nos testes
const testUser = UserFactory.create();
const adminUser = UserFactory.createAdmin();
const inactiveUsers = UserFactory.createMany(5, { isActive: false });
```

---

## Performance e Otimização

### Monitoring e Metrics
```typescript
// monitoring/mongo-monitor.service.ts
import { Injectable, Logger } from '@nestjs/common';
import { InjectConnection } from '@nestjs/mongoose';
import { Connection } from 'mongoose';
import * as promClient from 'prom-client';

@Injectable()
export class MongoMonitorService {
  private readonly logger = new Logger(MongoMonitorService.name);
  
  private queryDuration = new promClient.Histogram({
    name: 'mongodb_query_duration_seconds',
    help: 'Duration of MongoDB queries in seconds',
    labelNames: ['collection', 'operation', 'status'],
    buckets: [0.1, 0.5, 1, 2, 5],
  });

  private queryErrors = new promClient.Counter({
    name: 'mongodb_query_errors_total',
    help: 'Total number of MongoDB query errors',
    labelNames: ['collection', 'operation'],
  });

  constructor(@InjectConnection() private connection: Connection) {
    this.setupMonitoring();
  }

  private setupMonitoring(): void {
    const originalExec = require('mongoose').Query.prototype.exec;

    require('mongoose').Query.prototype.exec = async function (...args) {
      const start = Date.now();
      const collection = this.model.collection.collectionName;
      const operation = this.op;

      try {
        const result = await originalExec.apply(this, args);
        const duration = (Date.now() - start) / 1000;

        this.monitorService.queryDuration
          .labels(collection, operation, 'success')
          .observe(duration);

        // Log slow queries
        if (duration > 1) {
          this.logger.warn(`Slow query detected: ${duration}s`, {
            collection,
            operation,
            query: this.getQuery(),
            options: this.getOptions(),
          });
        }

        return result;
      } catch (error) {
        const duration = (Date.now() - start) / 1000;

        this.monitorService.queryDuration
          .labels(collection, operation, 'error')
          .observe(duration);

        this.monitorService.queryErrors
          .labels(collection, operation)
          .inc();

        throw error;
      }
    };
  }

  async getMetrics(): Promise<string> {
    const metrics = await Promise.all([
      promClient.register.metrics(),
      this.getDatabaseMetrics(),
    ]);

    return metrics.join('\n');
  }

  private async getDatabaseMetrics(): Promise<string> {
    const adminDb = this.connection.db.admin();
    
    try {
      const [serverStatus, dbStats] = await Promise.all([
        adminDb.serverStatus(),
        this.connection.db.stats(),
      ]);

      const metrics = [];

      // Connection metrics
      metrics.push(`mongodb_connections_current{type="active"} ${serverStatus.connections.active}`);
      metrics.push(`mongodb_connections_current{type="available"} ${serverStatus.connections.available}`);

      // Memory metrics
      metrics.push(`mongodb_memory_bytes{type="resident"} ${serverStatus.mem.resident}`);
      metrics.push(`mongodb_memory_bytes{type="virtual"} ${serverStatus.mem.virtual}`);

      // Database metrics
      metrics.push(`mongodb_database_size_bytes ${dbStats.dataSize}`);
      metrics.push(`mongodb_database_storage_bytes ${dbStats.storageSize}`);
      metrics.push(`mongodb_database_index_size_bytes ${dbStats.indexSize}`);

      // Collection metrics
      const collections = await this.connection.db.collections();
      for (const collection of collections) {
        const stats = await collection.stats();
        metrics.push(`mongodb_collection_documents{collection="${collection.collectionName}"} ${stats.count}`);
        metrics.push(`mongodb_collection_size_bytes{collection="${collection.collectionName}"} ${stats.size}`);
      }

      return metrics.join('\n');
    } catch (error) {
      this.logger.error('Error getting database metrics', error);
      return '';
    }
  }

  async getPerformanceReport(): Promise<any> {
    const adminDb = this.connection.db.admin();
    
    const [serverStatus, currentOp] = await Promise.all([
      adminDb.serverStatus(),
      adminDb.command({ currentOp: 1 }),
    ]);

    // Analisar queries lentas
    const slowQueries = currentOp.inprog
      .filter((op: any) => op.secs_running > 5)
      .map((op: any) => ({
        type: op.op,
        ns: op.ns,
        running: op.secs_running,
        query: op.query,
      }));

    return {
      server: {
        version: serverStatus.version,
        uptime: serverStatus.uptime,
        connections: serverStatus.connections,
        memory: serverStatus.mem,
        network: serverStatus.network,
      },
      operations: {
        total: currentOp.inprog.length,
        slow: slowQueries.length,
        slowQueries,
      },
      recommendations: this.generateRecommendations(serverStatus, slowQueries),
    };
  }

  private generateRecommendations(serverStatus: any, slowQueries: any[]): string[] {
    const recommendations: string[] = [];

    // Recomendações baseadas em métricas
    if (serverStatus.connections.current > 1000) {
      recommendations.push('Consider increasing connection pool size');
    }

    if (serverStatus.mem.resident > 0.8 * serverStatus.mem.systemMem) {
      recommendations.push('Consider adding more RAM or optimizing memory usage');
    }

    // Recomendações baseadas em queries lentas
    slowQueries.forEach((query) => {
      if (query.type === 'query') {
        recommendations.push(`Add index on collection ${query.ns} for query: ${JSON.stringify(query.query)}`);
      }
    });

    return recommendations;
  }
}
```

### Connection Pooling
```typescript
// database/connection-pool.config.ts
export const getConnectionPoolConfig = (): MongooseModuleOptions => ({
  uri: process.env.MONGODB_URI,
  poolSize: 10, // Número máximo de conexões no pool
  serverSelectionTimeoutMS: 5000, // Timeout para seleção de servidor
  socketTimeoutMS: 45000, // Timeout para sockets
  family: 4, // Usar IPv4
  maxPoolSize: 50, // Pool máximo
  minPoolSize: 5, // Pool mínimo
  maxIdleTimeMS: 10000, // Tempo máximo que uma conexão pode ficar ociosa
  waitQueueTimeoutMS: 10000, // Timeout para conexões na fila de espera
  connectTimeoutMS: 10000, // Timeout para conexão inicial
});

// Monitoramento do pool
@Injectable()
export class PoolMonitorService {
  private readonly logger = new Logger(PoolMonitorService.name);

  constructor(@InjectConnection() private connection: Connection) {
    this.monitorPool();
  }

  private monitorPool(): void {
    const pool = (this.connection as any).client.s.options.pool;

    setInterval(() => {
      const stats = {
        totalConnections: pool.totalConnectionCount,
        activeConnections: pool.availableConnectionCount,
        pendingRequests: pool.waitQueueSize,
      };

      this.logger.debug('Connection pool stats', stats);

      // Alertas
      if (stats.pendingRequests > 10) {
        this.logger.warn('High number of pending connection requests', stats);
      }

      if (stats.activeConnections / stats.totalConnections > 0.8) {
        this.logger.warn('Connection pool approaching capacity', stats);
      }
    }, 30000); // A cada 30 segundos
  }

  async getPoolStats(): Promise<any> {
    const pool = (this.connection as any).client.s.options.pool;
    
    return {
      total: pool.totalConnectionCount,
      active: pool.availableConnectionCount,
      idle: pool.totalConnectionCount - pool.availableConnectionCount,
      pending: pool.waitQueueSize,
      minSize: pool.minPoolSize,
      maxSize: pool.maxPoolSize,
    };
  }
}
```

### Caching Strategy
```typescript
// services/cached-user.service.ts
import { Injectable, Inject, CACHE_MANAGER } from '@nestjs/common';
import { Cache } from 'cache-manager';

@Injectable()
export class CachedUserService {
  private readonly CACHE_TTL = 60 * 5; // 5 minutos
  private readonly CACHE_PREFIX = 'user';

  constructor(
    private readonly userService: UserService,
    @Inject(CACHE_MANAGER) private cacheManager: Cache,
  ) {}

  async findOne(id: string): Promise<UserResponseDto> {
    const cacheKey = `${this.CACHE_PREFIX}:${id}`;
    
    // Tentar obter do cache
    const cached = await this.cacheManager.get<UserResponseDto>(cacheKey);
    
    if (cached) {
      return cached;
    }

    // Buscar do banco de dados
    const user = await this.userService.findOne(id);
    
    // Armazenar no cache
    await this.cacheManager.set(cacheKey, user, this.CACHE_TTL);
    
    return user;
  }

  async invalidateCache(id: string): Promise<void> {
    const cacheKey = `${this.CACHE_PREFIX}:${id}`;
    await this.cacheManager.del(cacheKey);
  }

  async findManyWithCache(ids: string[]): Promise<UserResponseDto[]> {
    const cacheKeys = ids.map(id => `${this.CACHE_PREFIX}:${id}`);
    
    // Buscar todos do cache
    const cachedUsers = await Promise.all(
      cacheKeys.map(key => this.cacheManager.get<UserResponseDto>(key))
    );

    // Identificar quais não estão em cache
    const missingIds = ids.filter((_, index) => !cachedUsers[index]);
    const cachedResults = cachedUsers.filter(Boolean) as UserResponseDto[];

    if (missingIds.length === 0) {
      return cachedResults;
    }

    // Buscar faltantes do banco
    const missingUsers = await Promise.all(
      missingIds.map(id => this.userService.findOne(id))
    );

    // Armazenar faltantes no cache
    await Promise.all(
      missingUsers.map((user, index) =>
        this.cacheManager.set(
          `${this.CACHE_PREFIX}:${missingIds[index]}`,
          user,
          this.CACHE_TTL
        )
      )
    );

    return [...cachedResults, ...missingUsers];
  }
}
```

### Bulk Operations
```typescript
// services/bulk-operations.service.ts
@Injectable()
export class BulkOperationsService {
  async bulkInsert(
    data: any[],
    batchSize: number = 1000,
  ): Promise<{ inserted: number; errors: any[] }> {
    const errors: any[] = [];
    let inserted = 0;

    // Processar em batches
    for (let i = 0; i < data.length; i += batchSize) {
      const batch = data.slice(i, i + batchSize);
      
      try {
        await this.userModel.insertMany(batch, { ordered: false });
        inserted += batch.length;
      } catch (error) {
        if (error.writeErrors) {
          errors.push(...error.writeErrors);
          inserted += error.nInserted;
        } else {
          throw error;
        }
      }
    }

    return { inserted, errors };
  }

  async bulkUpdate(
    updates: Array<{ filter: any; update: any }>,
    options?: any,
  ): Promise<any> {
    const bulkOps = updates.map(({ filter, update }) => ({
      updateOne: {
        filter,
        update,
        upsert: options?.upsert || false,
      },
    }));

    return this.userModel.bulkWrite(bulkOps, {
      ordered: false,
      ...options,
    });
  }

  async bulkDelete(filters: any[]): Promise<any> {
    const bulkOps = filters.map(filter => ({
      deleteOne: { filter },
    }));

    return this.userModel.bulkWrite(bulkOps, { ordered: false });
  }

  async parallelBulkOperations(
    operations: Array<() => Promise<any>>,
    concurrency: number = 5,
  ): Promise<any[]> {
    const results: any[] = [];
    const queue = [...operations];

    // Executar operações em paralelo
    const workers = Array.from({ length: concurrency }, async () => {
      while (queue.length > 0) {
        const operation = queue.shift();
        if (operation) {
          try {
            const result = await operation();
            results.push(result);
          } catch (error) {
            results.push({ error: error.message });
          }
        }
      }
    });

    await Promise.all(workers);
    return results;
  }
}
```

---

## Segurança

### Data Encryption
```typescript
// security/encryption.service.ts
import { Injectable } from '@nestjs/common';
import { createCipheriv, createDecipheriv, randomBytes, scrypt } from 'crypto';
import { promisify } from 'util';

@Injectable()
export class EncryptionService {
  private readonly algorithm = 'aes-256-gcm';
  private readonly key: Buffer;

  constructor() {
    // Em produção, use variáveis de ambiente
    this.key = Buffer.from(process.env.ENCRYPTION_KEY, 'hex');
  }

  async encrypt(text: string): Promise<{ encrypted: string; iv: string; authTag: string }> {
    const iv = randomBytes(16);
    const cipher = createCipheriv(this.algorithm, this.key, iv);

    let encrypted = cipher.update(text, 'utf8', 'hex');
    encrypted += cipher.final('hex');

    const authTag = cipher.getAuthTag().toString('hex');

    return {
      encrypted,
      iv: iv.toString('hex'),
      authTag,
    };
  }

  async decrypt(encrypted: string, iv: string, authTag: string): Promise<string> {
    const decipher = createDecipheriv(
      this.algorithm,
      this.key,
      Buffer.from(iv, 'hex'),
    );

    decipher.setAuthTag(Buffer.from(authTag, 'hex'));

    let decrypted = decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');

    return decrypted;
  }
}

// Schema com campos encriptados
@Schema()
export class SensitiveData {
  @Prop({
    type: String,
    required: true,
    get: function (this: any) {
      // Decriptar ao ler
      if (this.encryptedData && this.encryptionIv && this.encryptionTag) {
        const encryptionService = new EncryptionService();
        return encryptionService.decrypt(
          this.encryptedData,
          this.encryptionIv,
          this.encryptionTag,
        );
      }
      return null;
    },
    set: function (value: string) {
      // Encriptar ao salvar
      const encryptionService = new EncryptionService();
      const { encrypted, iv, authTag } = encryptionService.encrypt(value);
      
      this.encryptedData = encrypted;
      this.encryptionIv = iv;
      this.encryptionTag = authTag;
      
      return value;
    },
  })
  sensitiveInfo: string;

  @Prop()
  encryptedData: string;

  @Prop()
  encryptionIv: string;

  @Prop()
  encryptionTag: string;
}
```

### Query Injection Protection
```typescript
// security/query-sanitizer.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class QuerySanitizerService {
  sanitizeQuery(query: any): any {
    const sanitized = { ...query };

    // Remover operadores perigosos
    const dangerousOperators = ['$where', '$eval', '$accumulator', '$function'];
    
    dangerousOperators.forEach(op => {
      this.removeOperator(sanitized, op);
    });

    // Sanitizar campos de texto
    if (typeof sanitized === 'object') {
      this.sanitizeObject(sanitized);
    }

    return sanitized;
  }

  private removeOperator(obj: any, operator: string): void {
    if (obj && typeof obj === 'object') {
      Object.keys(obj).forEach(key => {
        if (key === operator) {
          delete obj[key];
        } else if (Array.isArray(obj[key])) {
          obj[key].forEach((item: any) => this.removeOperator(item, operator));
        } else if (typeof obj[key] === 'object') {
          this.removeOperator(obj[key], operator);
        }
      });
    }
  }

  private sanitizeObject(obj: any): void {
    Object.keys(obj).forEach(key => {
      if (typeof obj[key] === 'string') {
        // Remover caracteres perigosos
        obj[key] = obj[key].replace(/[;'"\\]/g, '');
      } else if (typeof obj[key] === 'object') {
        this.sanitizeObject(obj[key]);
      }
    });
  }

  validateQueryStructure(query: any, allowedFields: string[]): boolean {
    const keys = this.getAllKeys(query);
    
    // Verificar se todos os campos são permitidos
    return keys.every(key => allowedFields.includes(key));
  }

  private getAllKeys(obj: any): string[] {
    let keys: string[] = [];
    
    if (obj && typeof obj === 'object') {
      Object.keys(obj).forEach(key => {
        keys.push(key);
        if (typeof obj[key] === 'object') {
          keys = [...keys, ...this.getAllKeys(obj[key])];
        }
      });
    }
    
    return keys;
  }
}
```

### Field Level Security
```typescript
// security/field-redaction.middleware.ts
import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class FieldRedactionMiddleware implements NestMiddleware {
  private readonly sensitiveFields = [
    'password',
    'creditCard',
    'ssn',
    'token',
    'secret',
  ];

  use(req: Request, res: Response, next: NextFunction) {
    const originalJson = res.json;
    
    res.json = function (body: any) {
      if (body && typeof body === 'object') {
        body = this.redactSensitiveFields(body);
      }
      
      return originalJson.call(this, body);
    }.bind(this);

    next();
  }

  private redactSensitiveFields(obj: any): any {
    if (Array.isArray(obj)) {
      return obj.map(item => this.redactSensitiveFields(item));
    }

    if (obj && typeof obj === 'object') {
      const result = { ...obj };
      
      Object.keys(result).forEach(key => {
        if (this.sensitiveFields.some(field => 
          key.toLowerCase().includes(field.toLowerCase())
        )) {
          result[key] = '[REDACTED]';
        } else if (typeof result[key] === 'object') {
          result[key] = this.redactSensitiveFields(result[key]);
        }
      });

      return result;
    }

    return obj;
  }
}

// Uso no controller
@Controller('users')
@UseInterceptors(FieldRedactionInterceptor)
export class UsersController {
  // ...
}
```

### Role-based Access Control (RBAC)
```typescript
// security/rbac.service.ts
import { Injectable } from '@nestjs/common';

@Injectable()
export class RbacService {
  private readonly permissions = {
    user: {
      read: ['own'],
      update: ['own'],
      delete: ['own'],
    },
    moderator: {
      read: ['all'],
      update: ['all'],
      delete: ['own'],
      create: ['all'],
    },
    admin: {
      read: ['all'],
      update: ['all'],
      delete: ['all'],
      create: ['all'],
      manage: ['all'],
    },
  };

  canAccess(
    userRole: string,
    resource: string,
    action: string,
    resourceOwnerId?: string,
    currentUserId?: string,
  ): boolean {
    const rolePermissions = this.permissions[userRole];
    
    if (!rolePermissions) {
      return false;
    }

    const actionPermissions = rolePermissions[action];
    
    if (!actionPermissions) {
      return false;
    }

    // Verificar se tem acesso a 'all'
    if (actionPermissions.includes('all')) {
      return true;
    }

    // Verificar se tem acesso a 'own' e é o proprietário
    if (actionPermissions.includes('own') && 
        resourceOwnerId && 
        currentUserId && 
        resourceOwnerId === currentUserId) {
      return true;
    }

    return false;
  }

  filterDataByRole(data: any, userRole: string): any {
    const fieldsToRemove = this.getFieldsToRemove(userRole);
    
    return this.removeFields(data, fieldsToRemove);
  }

  private getFieldsToRemove(role: string): string[] {
    const fieldRules = {
      user: ['password', 'internalNotes', 'salary', 'ssn'],
      moderator: ['password', 'ssn'],
      admin: [],
    };

    return fieldRules[role] || fieldRules.user;
  }

  private removeFields(obj: any, fields: string[]): any {
    if (Array.isArray(obj)) {
      return obj.map(item => this.removeFields(item, fields));
    }

    if (obj && typeof obj === 'object') {
      const result = { ...obj };
      
      fields.forEach(field => {
        delete result[field];
      });

      Object.keys(result).forEach(key => {
        if (typeof result[key] === 'object') {
          result[key] = this.removeFields(result[key], fields);
        }
      });

      return result;
    }

    return obj;
  }
}
```

---

## Deploy e Produção

### Configuração de Produção
```typescript
// config/production.config.ts
import { registerAs } from '@nestjs/config';

export default registerAs('mongodb', () => ({
  uri: process.env.MONGODB_URI,
  options: {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    retryWrites: true,
    w: 'majority',
    authSource: 'admin',
    poolSize: 10,
    maxPoolSize: 50,
    minPoolSize: 5,
    maxIdleTimeMS: 30000,
    socketTimeoutMS: 45000,
    connectTimeoutMS: 10000,
    serverSelectionTimeoutMS: 5000,
    heartbeatFrequencyMS: 10000,
    autoIndex: false, // Desabilitar em produção
    family: 4,
  },
}));
```

### Health Check
```typescript
// health/mongodb.health.ts
import { Injectable } from '@nestjs/common';
import { HealthIndicator, HealthIndicatorResult } from '@nestjs/terminus';
import { InjectConnection } from '@nestjs/mongoose';
import { Connection } from 'mongoose';

@Injectable()
export class MongodbHealthIndicator extends HealthIndicator {
  constructor(@InjectConnection() private connection: Connection) {
    super();
  }

  async isHealthy(key: string): Promise<HealthIndicatorResult> {
    try {
      // Testar conexão
      await this.connection.db.admin().ping();
      
      // Obter estatísticas
      const stats = await this.connection.db.stats();
      
      return this.getStatus(key, true, {
        version: this.connection.version,
        database: this.connection.name,
        collections: stats.collections,
        documents: stats.objects,
        storage: {
          dataSize: this.formatBytes(stats.dataSize),
          storageSize: this.formatBytes(stats.storageSize),
          indexSize: this.formatBytes(stats.indexSize),
        },
        connections: {
          current: (this.connection as any).connections.length,
        },
      });
    } catch (error) {
      return this.getStatus(key, false, {
        message: error.message,
      });
    }
  }

  private formatBytes(bytes: number): string {
    const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
    
    if (bytes === 0) return '0 Byte';
    
    const i = Math.floor(Math.log(bytes) / Math.log(1024));
    
    return Math.round(bytes / Math.pow(1024, i)) + ' ' + sizes[i];
  }
}
```

### Backup Strategy
```typescript
// backup/mongodb-backup.service.ts
import { Injectable, Logger } from '@nestjs/common';
import { Cron, CronExpression } from '@nestjs/schedule';
import { exec } from 'child_process';
import { promisify } from 'util';

const execAsync = promisify(exec);

@Injectable()
export class MongodbBackupService {
  private readonly logger = new Logger(MongodbBackupService.name);

  @Cron(CronExpression.EVERY_DAY_AT_2AM)
  async performDailyBackup(): Promise<void> {
    try {
      const timestamp = new Date().toISOString().replace(/[:.]/g, '-');
      const backupPath = `/backups/mongodb-${timestamp}`;
      
      const command = `mongodump \
        --uri="${process.env.MONGODB_URI}" \
        --out="${backupPath}" \
        --gzip`;

      await execAsync(command);
      
      this.logger.log(`Backup completed: ${backupPath}`);
      
      // Limpar backups antigos (manter últimos 7 dias)
      await this.cleanOldBackups();
    } catch (error) {
      this.logger.error('Backup failed', error);
      throw error;
    }
  }

  private async cleanOldBackups(): Promise<void> {
    const maxAgeDays = 7;
    const cutoffDate = new Date();
    cutoffDate.setDate(cutoffDate.getDate() - maxAgeDays);

    // Lógica para remover backups antigos
    // Implementar de acordo com seu sistema de arquivos
  }

  async restoreBackup(backupPath: string): Promise<void> {
    try {
      const command = `mongorestore \
        --uri="${process.env.MONGODB_URI}" \
        --gzip \
        "${backupPath}"`;

      await execAsync(command);
      
      this.logger.log(`Restore completed from: ${backupPath}`);
    } catch (error) {
      this.logger.error('Restore failed', error);
      throw error;
    }
  }
}
```

### Docker Configuration
```dockerfile
# Dockerfile para aplicação NestJS + MongoDB
FROM node:18-alpine AS builder

WORKDIR /app

COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

FROM node:18-alpine AS production

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY --from=builder /app/dist ./dist
COPY --from=builder /app/node_modules ./node_modules

EXPOSE 3000

CMD ["node", "dist/main"]
```

```yaml
# docker-compose.prod.yml
version: '3.8'

services:
  mongodb:
    image: mongo:6
    container_name: nestjs-mongodb
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGODB_USERNAME}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGODB_PASSWORD}
      MONGO_INITDB_DATABASE: ${MONGODB_DATABASE}
    ports:
      - "27017:27017"
    volumes:
      - mongodb_data:/data/db
      - ./mongo-init.js:/docker-entrypoint-initdb.d/mongo-init.js:ro
    command: >
      mongod
      --auth
      --bind_ip_all
      --wiredTigerCacheSizeGB 1.5
    healthcheck:
      test: echo 'db.runCommand("ping").ok' | mongosh localhost:27017/test --quiet
      interval: 30s
      timeout: 10s
      retries: 3

  app:
    build:
      context: .
      target: production
    container_name: nestjs-app
    restart: always
    depends_on:
      mongodb:
        condition: service_healthy
    environment:
      NODE_ENV: production
      MONGODB_URI: mongodb://${MONGODB_USERNAME}:${MONGODB_PASSWORD}@mongodb:27017/${MONGODB_DATABASE}?authSource=admin
    ports:
      - "3000:3000"
    volumes:
      - ./logs:/app/logs

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: ${MONGODB_USERNAME}
      ME_CONFIG_MONGODB_ADMINPASSWORD: ${MONGODB_PASSWORD}
      ME_CONFIG_MONGODB_URL: mongodb://${MONGODB_USERNAME}:${MONGODB_PASSWORD}@mongodb:27017/

volumes:
  mongodb_data:
```

### Script de Inicialização do MongoDB
```javascript
// mongo-init.js
db = db.getSiblingDB('admin');

db.createUser({
  user: process.env.MONGO_INITDB_ROOT_USERNAME,
  pwd: process.env.MONGO_INITDB_ROOT_PASSWORD,
  roles: [
    { role: 'root', db: 'admin' },
    { role: 'readWrite', db: process.env.MONGO_INITDB_DATABASE },
  ],
});

db = db.getSiblingDB(process.env.MONGO_INITDB_DATABASE);

// Criar collections e índices iniciais
db.createCollection('users');
db.users.createIndex({ email: 1 }, { unique: true });
db.users.createIndex({ createdAt: -1 });

db.createCollection('products');
db.products.createIndex({ name: 'text', description: 'text' });
db.products.createIndex({ category: 1, price: 1 });

// Inserir dados iniciais se necessário
if (process.env.NODE_ENV === 'development') {
  db.users.insertOne({
    name: 'Admin User',
    email: 'admin@example.com',
    password: '$2b$10$YourHashedPasswordHere',
    role: 'admin',
    isActive: true,
    createdAt: new Date(),
    updatedAt: new Date(),
  });
}
```

---

## Boas Práticas

### 1. Design de Schemas
- **Embedding para relacionamentos 1:1 ou 1:few**
- **Referencing para relacionamentos 1:many ou many:many**
- **Evitar arrays que crescem sem limite**
- **Usar tipos de dados apropriados** (ObjectId para referências)
- **Considerar padrões de acesso** ao modelar dados

### 2. Performance
- **Criar índices** para campos frequentemente consultados
- **Usar projeção** para retornar apenas campos necessários
- **Implementar paginação** para grandes datasets
- **Usar lean()** para queries de leitura
- **Monitorar queries lentas** regularmente

### 3. Segurança
- **Validar todos os inputs** no schema e DTOs
- **Usar roles e permissões** no nível do banco
- **Encriptar dados sensíveis**
- **Implementar rate limiting**
- **Auditar operações** importantes

### 4. Manutenção
- **Versionar schemas** com cuidado
- **Manter índices otimizados**
- **Implementar backups automáticos**
- **Monitorar performance** constantemente
- **Documentar decisões** de modelagem

### 5. Código
- **Separar responsabilidades** (DTOs, Schemas, Services, Repositories)
- **Usar transações** para operações atômicas
- **Implementar tratamento de erros** apropriado
- **Escrever testes** para todas as funcionalidades
- **Manter documentação** atualizada

### 6. Operações
- **Configurar connection pooling** apropriadamente
- **Implementar health checks**
- **Monitorar métricas** do MongoDB
- **Planejar capacidade** (scaling)
- **Ter plano de disaster recovery**

### Checklist de Produção
- [ ] Índices criados e otimizados
- [ ] Connection pooling configurado
- [ ] Backups automáticos implementados
- [ ] Monitoramento configurado
- [ ] Health checks implementados
- [ ] Logging configurado
- [ ] Segurança implementada (autenticação, autorização)
- [ ] Performance testada com carga
- [ ] Plano de scaling definido
- [ ] Documentação atualizada

### Ferramentas Recomendadas
- **MongoDB Atlas**: Cloud managed MongoDB
- **MongoDB Compass**: GUI para gerenciamento
- **Studio 3T**: IDE para MongoDB
- **MongoDB Charts**: Visualização de dados
- **Prometheus + Grafana**: Monitoramento
- **MongoDB Ops Manager**: Gerenciamento on-premise

### Recursos de Aprendizado
- [Documentação Oficial MongoDB](https://docs.mongodb.com/)
- [MongoDB University](https://university.mongodb.com/)
- [NestJS + MongoDB](https://docs.nestjs.com/techniques/mongodb)
- [Mongoose Documentation](https://mongoosejs.com/docs/guide.html)
- [MongoDB Best Practices](https://www.mongodb.com/docs/manual/administration/production-notes/)

## Conclusão

MongoDB com NestJS oferece uma combinação poderosa para construir aplicações modernas, escaláveis e de alta performance. A flexibilidade do MongoDB combinada com a estrutura robusta do NestJS permite criar APIs RESTful eficientes e mantíveis.

Lembre-se de sempre:
1. **Modelar dados** baseado nos padrões de acesso da aplicação
2. **Monitorar performance** constantemente
3. **Implementar segurança** em todas as camadas
4. **Escrever testes** abrangentes
5. **Planejar para escala** desde o início

Com as práticas e padrões apresentados neste manual, você estará bem equipado para construir aplicações robustas usando MongoDB e NestJS.

---
*Última atualização: Janeiro 2024*
*Versões: NestJS 10, Mongoose 7, MongoDB 6*
