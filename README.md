# Tutorial Introdutório: API REST e NestJS

Este tutorial oferece uma introdução completa aos conceitos de API REST e ao framework NestJS, com uma parte prática para consolidar o aprendizado.

## Sumário

1. [Parte 1: Conceitos Básicos de API REST](#parte-1-conceitos-básicos-de-api-rest)
2. [Parte 2: Conceitos Básicos de NestJS](#parte-2-conceitos-básicos-de-nestjs)
3. [Parte 3: Prática com NestJS - CRUD de Tarefas](#parte-3-prática-com-nestjs---crud-de-tarefas)

---

## Parte 1: Conceitos Básicos de API REST

### O que é uma API?

**API (Application Programming Interface)** é um conjunto de definições e protocolos que permite a comunicação entre diferentes aplicações de software. É como um "contrato" que define como diferentes sistemas podem se comunicar.

### O que é REST?

**REST (Representational State Transfer)** é um estilo arquitetural para desenvolvimento de serviços web que utiliza o protocolo HTTP de forma eficiente e padronizada.

### Princípios Fundamentais do REST

1. **Stateless (Sem Estado)**: Cada requisição deve conter toda a informação necessária para ser processada
2. **Cliente-Servidor**: Separação clara entre cliente e servidor
3. **Cacheable**: Respostas devem ser cacheáveis quando possível
4. **Interface Uniforme**: Uso consistente dos métodos HTTP
5. **Sistema em Camadas**: Arquitetura pode ter múltiplas camadas
6. **Code on Demand** (opcional): Servidor pode enviar código executável

### Métodos HTTP Principais

| Método | Descrição | Uso Comum |
|--------|-----------|-----------|
| `GET` | Recuperar dados | Listar/buscar recursos |
| `POST` | Criar novos recursos | Criar novos registros |
| `PUT` | Atualizar recurso completo | Atualização total |
| `PATCH` | Atualizar parcialmente | Atualização parcial |
| `DELETE` | Remover recursos | Exclusão de registros |

### Códigos de Status HTTP

#### 2xx - Sucesso
- `200 OK`: Requisição bem-sucedida
- `201 Created`: Recurso criado com sucesso
- `204 No Content`: Sucesso sem conteúdo de retorno

#### 4xx - Erro do Cliente
- `400 Bad Request`: Requisição malformada
- `401 Unauthorized`: Não autenticado
- `403 Forbidden`: Não autorizado
- `404 Not Found`: Recurso não encontrado

#### 5xx - Erro do Servidor
- `500 Internal Server Error`: Erro interno do servidor
- `503 Service Unavailable`: Serviço indisponível

### Estrutura de URLs REST

```
GET    /api/tasks           # Listar todas as tarefas
GET    /api/tasks/1         # Buscar tarefa com ID 1
POST   /api/tasks           # Criar nova tarefa
PUT    /api/tasks/1         # Atualizar tarefa com ID 1
DELETE /api/tasks/1         # Deletar tarefa com ID 1
```

### Exemplo de Resposta JSON

```json
{
  "id": 1,
  "title": "Estudar NestJS",
  "description": "Aprender os conceitos básicos do framework",
  "status": "aberto",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

---

## Parte 2: Conceitos Básicos de NestJS

### O que é NestJS?

**NestJS** é um framework Node.js para construção de aplicações server-side eficientes e escaláveis. É construído com TypeScript e combina elementos de OOP (Programação Orientada a Objetos), FP (Programação Funcional) e FRP (Programação Reativa Funcional).

### Por que usar NestJS?

- **TypeScript First**: Suporte nativo ao TypeScript
- **Arquitetura Modular**: Organização em módulos reutilizáveis
- **Decorators**: Uso extensivo de decorators para metadados
- **Dependency Injection**: Sistema robusto de injeção de dependência
- **Testing**: Ferramentas integradas para testes
- **Documentação**: Geração automática de documentação OpenAPI/Swagger

### Conceitos Fundamentais

#### 1. Controllers
Responsáveis por lidar com requisições HTTP e retornar respostas.

```typescript
@Controller('tasks')
export class TasksController {
  @Get()
  findAll() {
    return 'Esta ação retorna todas as tarefas';
  }

  @Post()
  create(@Body() createTaskDto: CreateTaskDto) {
    return 'Esta ação adiciona uma nova tarefa';
  }
}
```

#### 2. Services (Providers)
Contêm a lógica de negócio e podem ser injetados em outros componentes.

```typescript
@Injectable()
export class TasksService {
  private tasks: Task[] = [];

  findAll(): Task[] {
    return this.tasks;
  }

  create(task: Task): Task {
    this.tasks.push(task);
    return task;
  }
}
```

#### 3. Modules
Organizam a aplicação em unidades funcionais.

```typescript
@Module({
  controllers: [TasksController],
  providers: [TasksService],
})
export class TasksModule {}
```

#### 4. DTOs (Data Transfer Objects)
Definem a estrutura dos dados que trafegam na API.

```typescript
export class CreateTaskDto {
  title: string;
  description: string;
  status: 'aberto' | 'fazendo' | 'finalizado';
}
```

#### 5. Entities
Representam as estruturas de dados do banco.

```typescript
@Entity()
export class Task {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @Column()
  description: string;

  @Column()
  status: string;
}
```

### Fluxo de Requisição no NestJS

```
Requisição HTTP → Controller → Service → Repository → Database
                     ↓
Resposta HTTP ← Controller ← Service ← Repository ← Database
```

### Decorators Importantes

| Decorator | Uso |
|-----------|-----|
| `@Controller()` | Define um controller |
| `@Get()`, `@Post()`, `@Put()`, `@Delete()` | Métodos HTTP |
| `@Injectable()` | Marca uma classe como provider |
| `@Module()` | Define um módulo |
| `@Body()` | Extrai o corpo da requisição |
| `@Param()` | Extrai parâmetros da URL |
| `@Query()` | Extrai query parameters |

---

## Parte 3: Prática com NestJS - CRUD de Tarefas

### Sumário da Parte Prática

1. [Criação do Projeto NestJS](#1-criação-do-projeto-nestjs)
2. [Configuração do TypeScript](#2-configuração-do-typescript)
3. [Instalação e Configuração de Bibliotecas](#3-instalação-e-configuração-de-bibliotecas)
4. [Configuração do Banco de Dados SQLite3](#4-configuração-do-banco-de-dados-sqlite3)
5. [Implementação da Entity Task](#5-implementação-da-entity-task)
6. [Implementação do CRUD](#6-implementação-do-crud)
7. [Configuração do CORS](#7-configuração-do-cors)
8. [Testes da API](#8-testes-da-api)

### 1. Criação do Projeto NestJS

#### Pré-requisitos
- Node.js (versão 14 ou superior)
- npm ou yarn

#### Instalação do CLI do NestJS
```bash
npm install -g @nestjs/cli
```

#### Criação do Projeto
```bash
nest new tasks-api
cd tasks-api
```

Durante a instalação, escolha `npm` como gerenciador de pacotes.

### 2. Configuração do TypeScript

O NestJS já vem configurado com TypeScript. Verifique o arquivo `tsconfig.json`:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "declaration": true,
    "removeComments": true,
    "emitDecoratorMetadata": true,
    "experimentalDecorators": true,
    "allowSyntheticDefaultImports": true,
    "target": "es2017",
    "sourceMap": true,
    "outDir": "./dist",
    "baseUrl": "./",
    "incremental": true,
    "skipLibCheck": true,
    "strictNullChecks": false,
    "noImplicitAny": false,
    "strictBindCallApply": false,
    "forceConsistentCasingInFileNames": false,
    "noFallthroughCasesInSwitch": false
  }
}
```

### 3. Instalação e Configuração de Bibliotecas

#### Instalação das Dependências
```bash
# TypeORM e SQLite
npm install @nestjs/typeorm typeorm sqlite3

# Class Validator e Class Transformer
npm install class-validator class-transformer

# CORS (já incluído no NestJS core)
# Swagger para documentação (opcional)
npm install @nestjs/swagger swagger-ui-express
```

### 4. Configuração do Banco de Dados SQLite3

#### Configuração no app.module.ts
```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TasksModule } from './tasks/tasks.module';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'sqlite',
      database: 'tasks.db',
      entities: [__dirname + '/**/*.entity{.ts,.js}'],
      synchronize: true, // Apenas para desenvolvimento
    }),
    TasksModule,
  ],
})
export class AppModule {}
```

### 5. Implementação da Entity Task

#### Criação da Entity (src/tasks/task.entity.ts)
```typescript
import { Entity, Column, PrimaryGeneratedColumn, CreateDateColumn, UpdateDateColumn } from 'typeorm';

export enum TaskStatus {
  ABERTO = 'aberto',
  FAZENDO = 'fazendo',
  FINALIZADO = 'finalizado',
}

@Entity()
export class Task {
  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  title: string;

  @Column()
  description: string;

  @Column({
    type: 'text',
    enum: TaskStatus,
    default: TaskStatus.ABERTO,
  })
  status: TaskStatus;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

### 6. Implementação do CRUD

#### DTOs (src/tasks/dto/)

**create-task.dto.ts**
```typescript
import { IsString, IsEnum, IsNotEmpty, IsOptional } from 'class-validator';
import { TaskStatus } from '../task.entity';

export class CreateTaskDto {
  @IsString()
  @IsNotEmpty()
  title: string;

  @IsString()
  @IsNotEmpty()
  description: string;

  @IsEnum(TaskStatus)
  @IsOptional()
  status?: TaskStatus;
}
```

**update-task.dto.ts**
```typescript
import { IsString, IsEnum, IsOptional } from 'class-validator';
import { TaskStatus } from '../task.entity';

export class UpdateTaskDto {
  @IsString()
  @IsOptional()
  title?: string;

  @IsString()
  @IsOptional()
  description?: string;

  @IsEnum(TaskStatus)
  @IsOptional()
  status?: TaskStatus;
}
```

#### Service (src/tasks/tasks.service.ts)
```typescript
import { Injectable, NotFoundException } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { Task } from './task.entity';
import { CreateTaskDto } from './dto/create-task.dto';
import { UpdateTaskDto } from './dto/update-task.dto';

@Injectable()
export class TasksService {
  constructor(
    @InjectRepository(Task)
    private taskRepository: Repository<Task>,
  ) {}

  async findAll(): Promise<Task[]> {
    return this.taskRepository.find();
  }

  async findOne(id: number): Promise<Task> {
    const task = await this.taskRepository.findOne({ where: { id } });
    if (!task) {
      throw new NotFoundException(`Tarefa com ID ${id} não encontrada`);
    }
    return task;
  }

  async create(createTaskDto: CreateTaskDto): Promise<Task> {
    const task = this.taskRepository.create(createTaskDto);
    return this.taskRepository.save(task);
  }

  async update(id: number, updateTaskDto: UpdateTaskDto): Promise<Task> {
    const task = await this.findOne(id);
    Object.assign(task, updateTaskDto);
    return this.taskRepository.save(task);
  }

  async remove(id: number): Promise<void> {
    const task = await this.findOne(id);
    await this.taskRepository.remove(task);
  }
}
```

#### Controller (src/tasks/tasks.controller.ts)
```typescript
import {
  Controller,
  Get,
  Post,
  Body,
  Put,
  Param,
  Delete,
  ParseIntPipe,
  HttpCode,
  HttpStatus,
} from '@nestjs/common';
import { TasksService } from './tasks.service';
import { CreateTaskDto } from './dto/create-task.dto';
import { UpdateTaskDto } from './dto/update-task.dto';
import { Task } from './task.entity';

@Controller('tasks')
export class TasksController {
  constructor(private readonly tasksService: TasksService) {}

  @Get()
  findAll(): Promise<Task[]> {
    return this.tasksService.findAll();
  }

  @Get(':id')
  findOne(@Param('id', ParseIntPipe) id: number): Promise<Task> {
    return this.tasksService.findOne(id);
  }

  @Post()
  @HttpCode(HttpStatus.CREATED)
  create(@Body() createTaskDto: CreateTaskDto): Promise<Task> {
    return this.tasksService.create(createTaskDto);
  }

  @Put(':id')
  update(
    @Param('id', ParseIntPipe) id: number,
    @Body() updateTaskDto: UpdateTaskDto,
  ): Promise<Task> {
    return this.tasksService.update(id, updateTaskDto);
  }

  @Delete(':id')
  @HttpCode(HttpStatus.NO_CONTENT)
  async remove(@Param('id', ParseIntPipe) id: number): Promise<void> {
    return this.tasksService.remove(id);
  }
}
```

#### Module (src/tasks/tasks.module.ts)
```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { TasksController } from './tasks.controller';
import { TasksService } from './tasks.service';
import { Task } from './task.entity';

@Module({
  imports: [TypeOrmModule.forFeature([Task])],
  controllers: [TasksController],
  providers: [TasksService],
})
export class TasksModule {}
```

### 7. Configuração do CORS

#### No main.ts
```typescript
import { NestFactory } from '@nestjs/core';
import { ValidationPipe } from '@nestjs/common';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  
  // Habilitar CORS
  app.enableCors({
    origin: true, // Em produção, especifique domínios específicos
    credentials: true,
  });

  // Habilitar validação global
  app.useGlobalPipes(new ValidationPipe({
    transform: true,
    whitelist: true,
    forbidNonWhitelisted: true,
  }));

  await app.listen(3000);
  console.log('API rodando em http://localhost:3000');
}
bootstrap();
```

### 8. Testes da API

#### Iniciando a Aplicação
```bash
npm run start:dev
```

#### Exemplos de Requisições

**1. Criar uma tarefa (POST /tasks)**
```bash
curl -X POST http://localhost:3000/tasks \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Estudar NestJS",
    "description": "Aprender conceitos básicos do framework",
    "status": "aberto"
  }'
```

**2. Listar todas as tarefas (GET /tasks)**
```bash
curl http://localhost:3000/tasks
```

**3. Buscar uma tarefa específica (GET /tasks/:id)**
```bash
curl http://localhost:3000/tasks/1
```

**4. Atualizar uma tarefa (PUT /tasks/:id)**
```bash
curl -X PUT http://localhost:3000/tasks/1 \
  -H "Content-Type: application/json" \
  -d '{
    "status": "fazendo"
  }'
```

**5. Deletar uma tarefa (DELETE /tasks/:id)**
```bash
curl -X DELETE http://localhost:3000/tasks/1
```

#### Estrutura de Arquivos Final
```
src/
├── app.module.ts
├── main.ts
└── tasks/
    ├── dto/
    │   ├── create-task.dto.ts
    │   └── update-task.dto.ts
    ├── task.entity.ts
    ├── tasks.controller.ts
    ├── tasks.module.ts
    └── tasks.service.ts
```

#### Scripts Úteis no package.json
```json
{
  "scripts": {
    "start": "nest start",
    "start:dev": "nest start --watch",
    "start:prod": "node dist/main",
    "test": "jest",
    "test:watch": "jest --watch"
  }
}
```

### Conclusão da Parte Prática

Nesta parte prática, você aprendeu a:

1. ✅ Criar um projeto NestJS do zero
2. ✅ Configurar TypeScript para desenvolvimento
3. ✅ Instalar e configurar bibliotecas essenciais (TypeORM, class-validator)
4. ✅ Configurar banco de dados SQLite3 com TypeORM
5. ✅ Implementar uma entity com TypeORM
6. ✅ Criar DTOs para validação de dados
7. ✅ Implementar um CRUD completo (Create, Read, Update, Delete)
8. ✅ Configurar CORS para permitir requisições cross-origin
9. ✅ Testar a API com diferentes tipos de requisições HTTP

A aplicação resultante é uma API REST funcional para gerenciamento de tarefas com três estados: **aberto**, **fazendo** e **finalizado**.

---

## Próximos Passos

Após completar este tutorial, você pode expandir seus conhecimentos explorando:

- **Autenticação e Autorização** com JWT
- **Testes Unitários e de Integração** com Jest
- **Documentação automática** com Swagger
- **Deploy** em plataformas como Heroku ou Vercel
- **Banco de dados mais robustos** como PostgreSQL
- **Relacionamentos entre entidades**
- **Middleware personalizado**
- **Guards e Interceptors**

## Recursos Adicionais

- [Documentação Oficial do NestJS](https://docs.nestjs.com/)
- [TypeORM Documentation](https://typeorm.io/)
- [REST API Design Best Practices](https://restfulapi.net/)
- [HTTP Status Codes](https://httpstatuses.com/)

---

*Este tutorial foi criado para fins educacionais. Para dúvidas ou sugestões, consulte a documentação oficial das tecnologias utilizadas.*
