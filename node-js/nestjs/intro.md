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



Now let’s go **step-by-step to build full JWT Authentication** in NestJS. This will include:

✅ Signup → 🔐 Login → 🎟️ JWT Token → 🛡️ Guard-protected route

---

## 🔐 Step-by-Step: Full JWT Auth in NestJS

### 📦 Step 1: Install Packages
```bash
npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
npm install --save-dev @types/passport-jwt @types/bcrypt
```

---

## 👤 Step 2: Create `AuthModule`

```bash
nest g module auth
nest g service auth
nest g controller auth
```

---

## 🔒 Step 3: Create User Auth DTOs

#### `auth/dto/signup.dto.ts`
```ts
import { IsEmail, IsNotEmpty } from 'class-validator';

export class SignupDto {
  @IsEmail()
  email: string;

  @IsNotEmpty()
  password: string;
}
```

#### `auth/dto/login.dto.ts`
```ts
import { IsEmail, IsNotEmpty } from 'class-validator';

export class LoginDto {
  @IsEmail()
  email: string;

  @IsNotEmpty()
  password: string;
}
```

---

## 🧠 Step 4: User Model + Password Hashing

In `user.schema.ts`:
```ts
import { Prop, Schema, SchemaFactory } from '@nestjs/mongoose';
import { Document } from 'mongoose';
import * as bcrypt from 'bcrypt';

export type UserDocument = User & Document;

@Schema()
export class User {
  @Prop({ required: true, unique: true })
  email: string;

  @Prop({ required: true })
  password: string;
}

export const UserSchema = SchemaFactory.createForClass(User);

// hash password before save
UserSchema.pre<UserDocument>('save', async function (next) {
  if (!this.isModified('password')) return next();
  this.password = await bcrypt.hash(this.password, 10);
  next();
});
```

---

## 🔑 Step 5: AuthService – Signup + Login

#### `auth.service.ts`
```ts
import { Injectable, UnauthorizedException } from '@nestjs/common';
import { JwtService } from '@nestjs/jwt';
import { InjectModel } from '@nestjs/mongoose';
import { Model } from 'mongoose';
import * as bcrypt from 'bcrypt';
import { User, UserDocument } from '../user/user.schema';
import { SignupDto } from './dto/signup.dto';
import { LoginDto } from './dto/login.dto';

@Injectable()
export class AuthService {
  constructor(
    private jwtService: JwtService,
    @InjectModel(User.name) private userModel: Model<UserDocument>,
  ) {}

  async signup(dto: SignupDto) {
    const user = new this.userModel(dto);
    await user.save();
    return this.signToken(user._id, user.email);
  }

  async login(dto: LoginDto) {
    const user = await this.userModel.findOne({ email: dto.email });
    if (!user || !(await bcrypt.compare(dto.password, user.password))) {
      throw new UnauthorizedException('Invalid credentials');
    }
    return this.signToken(user._id, user.email);
  }

  private signToken(userId: string, email: string) {
    const payload = { sub: userId, email };
    return {
      access_token: this.jwtService.sign(payload),
    };
  }
}
```

---

## 🧬 Step 6: AuthController – Signup + Login Routes

#### `auth.controller.ts`
```ts
import { Body, Controller, Post } from '@nestjs/common';
import { AuthService } from './auth.service';
import { SignupDto } from './dto/signup.dto';
import { LoginDto } from './dto/login.dto';

@Controller('auth')
export class AuthController {
  constructor(private readonly authService: AuthService) {}

  @Post('signup')
  signup(@Body() dto: SignupDto) {
    return this.authService.signup(dto);
  }

  @Post('login')
  login(@Body() dto: LoginDto) {
    return this.authService.login(dto);
  }
}
```

---

## 🔧 Step 7: Configure JWT in `auth.module.ts`

```ts
import { Module } from '@nestjs/common';
import { JwtModule } from '@nestjs/jwt';
import { AuthService } from './auth.service';
import { AuthController } from './auth.controller';
import { MongooseModule } from '@nestjs/mongoose';
import { User, UserSchema } from '../user/user.schema';

@Module({
  imports: [
    JwtModule.register({
      secret: 'your-secret-key',
      signOptions: { expiresIn: '1d' },
    }),
    MongooseModule.forFeature([{ name: User.name, schema: UserSchema }]),
  ],
  controllers: [AuthController],
  providers: [AuthService],
})
export class AuthModule {}
```

---

## 🛡️ Step 8: Protect Routes with JWT Guard

Use previous `jwt.strategy.ts` and `jwt-auth.guard.ts`.

Then in any controller:
```ts
@UseGuards(JwtAuthGuard)
@Get('me')
getProfile(@Request() req) {
  return req.user;
}
```

---

## ✅ Result

1. **POST `/auth/signup`** with `{ email, password }` → returns JWT  
2. **POST `/auth/login`** → returns JWT  
3. **GET `/user/me`** → pass `Authorization: Bearer <token>` → returns user info