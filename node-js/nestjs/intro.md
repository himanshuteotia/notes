Great choice! NestJS is a powerful framework for building scalable and maintainable backend applications with Node.js and TypeScript. Let's start step by step.

---

### 🧱 **What is NestJS?**
NestJS is a framework for building **server-side applications** using **TypeScript** (or JavaScript). It’s built on top of:
- **Express.js** (by default)
- **Fastify** (optional, for better performance)
- Uses **Dependency Injection**, **Decorators**, and **Modules** – like Angular.

---

### ✅ **Why use NestJS?**
- Fully supports TypeScript
- Clean code structure using modules
- Built-in support for middleware, interceptors, guards, pipes
- Easy to integrate with databases (TypeORM, Prisma, etc.)
- Built-in CLI

---

### 📦 Step 1: **Install NestJS CLI**
```bash
npm i -g @nestjs/cli
```

---

### 🛠️ Step 2: **Create a New Project**
```bash
nest new my-nest-app
```

Select **npm** or **yarn** and it will create the project.

---

### 📂 Project Structure Overview

```
src/
├── app.controller.ts     // Route handlers
├── app.service.ts        // Business logic
├── app.module.ts         // Root module
main.ts                   // Entry point
```

---

### 🚀 Step 3: **Run the app**
```bash
npm run start:dev
```
Visit [http://localhost:3000](http://localhost:3000)

---

### 🧪 Step 4: **Create a New Module, Controller, and Service**
```bash
nest g module user
nest g controller user
nest g service user
```

This will create:
- `user.module.ts`
- `user.controller.ts`
- `user.service.ts`

---

### 🧑‍💻 Example: Add a Route

In `user.controller.ts`:
```ts
import { Controller, Get } from '@nestjs/common';

@Controller('user')
export class UserController {
  @Get()
  getUser() {
    return { name: 'Himanshu', role: 'Developer' };
  }
}
```

Now access: `http://localhost:3000/user`


Awesome! Let's move to the **next important part: Connecting to a database and creating a simple CRUD API** using NestJS.

---

## 📦 Step 5: Connect to a Database (MongoDB)

We'll use **Mongoose**, a popular ODM for MongoDB.

### 🔌 Install Mongoose in your NestJS app
```bash
npm install @nestjs/mongoose mongoose
```

---

### 🧠 Step 6: Create a User Schema (Model)

```bash
nest g module user
nest g controller user
nest g service user
```

#### `user.schema.ts`
```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';

export type UserDocument = User & Document;

@Schema()
export class User {
  @Prop()
  name: string;

  @Prop()
  email: string;
}

export const UserSchema = SchemaFactory.createForClass(User);
```

---

### 🔧 Step 7: Register Mongoose in App Module

#### `app.module.ts`
```ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { UserModule } from './user/user.module';

@Module({
  imports: [
    MongooseModule.forRoot('mongodb://localhost/nest-demo'),
    UserModule,
  ],
})
export class AppModule {}
```

---

### 🛠️ Step 8: Register Schema in User Module

#### `user.module.ts`
```ts
import { Module } from '@nestjs/common';
import { MongooseModule } from '@nestjs/mongoose';
import { User, UserSchema } from './user.schema';
import { UserService } from './user.service';
import { UserController } from './user.controller';

@Module({
  imports: [MongooseModule.forFeature([{ name: User.name, schema: UserSchema }])],
  providers: [UserService],
  controllers: [UserController],
})
export class UserModule {}
```

---

## ✍️ Step 9: Create Service Methods

#### `user.service.ts`
```ts
import { Injectable } from '@nestjs/common';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import { User, UserDocument } from './user.schema';

@Injectable()
export class UserService {
  constructor(@InjectModel(User.name) private userModel: Model<UserDocument>) {}

  async create(user: Partial<User>): Promise<User> {
    return this.userModel.create(user);
  }

  async findAll(): Promise<User[]> {
    return this.userModel.find();
  }
}
```

---

## 🌐 Step 10: Create Routes in Controller

#### `user.controller.ts`
```ts
import { Body, Controller, Get, Post } from '@nestjs/common';
import { UserService } from './user.service';

@Controller('user')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  async create(@Body() body) {
    return this.userService.create(body);
  }

  @Get()
  async findAll() {
    return this.userService.findAll();
  }
}
```

---

Now you can:
- POST `http://localhost:3000/user` with `{ "name": "Himanshu", "email": "h@abc.com" }`
- GET `http://localhost:3000/user` to see all users

Perfect! Let's now move to **input validation using DTOs** – a very important feature in NestJS.

---

## ✅ Step 11: Add Validation using DTOs

NestJS uses **class-validator** and **class-transformer** for request validation.

### 📦 Install packages
```bash
npm install class-validator class-transformer
```

---

### 📁 Step 12: Create a DTO (Data Transfer Object)

Create a file: `create-user.dto.ts`

```ts
import { IsEmail, IsNotEmpty } from 'class-validator';

export class CreateUserDto {
  @IsNotEmpty()
  name: string;

  @IsEmail()
  email: string;
}
```

---

### ✋ Step 13: Enable Validation Globally

#### In `main.ts` (entry point)
```ts
import { ValidationPipe } from '@nestjs/common';
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe()); // ✅ enable validation
  await app.listen(3000);
}
bootstrap();
```

---

### 🧪 Step 14: Use DTO in Controller

#### `user.controller.ts`
```ts
import { Body, Controller, Get, Post } from '@nestjs/common';
import { UserService } from './user.service';
import { CreateUserDto } from './create-user.dto';

@Controller('user')
export class UserController {
  constructor(private readonly userService: UserService) {}

  @Post()
  async create(@Body() body: CreateUserDto) {
    return this.userService.create(body);
  }

  @Get()
  async findAll() {
    return this.userService.findAll();
  }
}
```

---

### 🚫 Now if you POST without a name or invalid email:
You’ll get an error like:
```json
{
  "statusCode": 400,
  "message": ["name should not be empty", "email must be an email"],
  "error": "Bad Request"
}
```

Great! Let's now implement **Update and Delete (PUT / DELETE) APIs** — completing the full **CRUD** for the `User` module.

---

## 🔁 Step 15: Update a User (PUT)

### ✍️ Update DTO

Create `update-user.dto.ts`
```ts
import { IsEmail, IsOptional, IsString } from 'class-validator';

export class UpdateUserDto {
  @IsOptional()
  @IsString()
  name?: string;

  @IsOptional()
  @IsEmail()
  email?: string;
}
```

---

### ⚙️ Service Method for Update

#### `user.service.ts`
```ts
import { UpdateUserDto } from './update-user.dto';
import { Types } from 'mongoose'; // for ObjectId type

// ...

async update(id: string, data: UpdateUserDto) {
  return this.userModel.findByIdAndUpdate(id, data, { new: true });
}
```

---

### 🌐 Add PUT Route

#### `user.controller.ts`
```ts
import { Param, Put } from '@nestjs/common';
import { UpdateUserDto } from './update-user.dto';

@Put(':id')
async update(@Param('id') id: string, @Body() body: UpdateUserDto) {
  return this.userService.update(id, body);
}
```

---

## 🗑️ Step 16: Delete a User (DELETE)

### ⚙️ Service Method

#### `user.service.ts`
```ts
async delete(id: string) {
  return this.userModel.findByIdAndDelete(id);
}
```

---

### 🌐 Add DELETE Route

#### `user.controller.ts`
```ts
import { Delete } from '@nestjs/common';

@Delete(':id')
async delete(@Param('id') id: string) {
  return this.userService.delete(id);
}
```

---

## ✅ Final Routes

| Method | Endpoint          | Description           |
|--------|-------------------|-----------------------|
| GET    | `/user`           | Get all users         |
| POST   | `/user`           | Create new user       |
| PUT    | `/user/:id`       | Update existing user  |
| DELETE | `/user/:id`       | Delete user by ID     |

