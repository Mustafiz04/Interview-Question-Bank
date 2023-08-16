# NESTJS Interview Questions and Answers

Welcome to the NESTJS interview questions and answers documentation! This guide aims to provide you with a comprehensive list of questions that may be asked during an interview related to NESTJS, a popular Node.js framework for building efficient, scalable, and maintainable server-side applications. Alongside each question, you will find a detailed answer to help you prepare for your interview.

## Table of Contents

1. [What is NESTJS?](#what-is-nestjs)
2. [What are the key features of NESTJS?](#key-features)
3. [Explain Dependency Injection in NESTJS.](#dependency-injection)
4. [How does Middleware work in NESTJS?](#middleware)
5. [What are Guards in NESTJS?](#guards)
6. [What is Interceptor and how is it used in NESTJS?](#interceptors)
7. [Explain Pipes in NESTJS and provide some use cases.](#pipes)
8. [What is a Module in NESTJS?](#modules)
9. [How do you handle CORS in NESTJS?](#cors)
10. [Explain the concept of Filters in NESTJS.](#filters)

---

## 1. What is NESTJS? <a name="what-is-nestjs"></a>

NESTJS is a progressive Node.js framework that is built to provide an organized architecture for building efficient and scalable server-side applications. It utilizes modern JavaScript and TypeScript features, along with design principles from frameworks like Angular. NESTJS helps developers create well-structured, maintainable codebases by promoting the use of modules, decorators, and dependency injection.

## 2. What are the key features of NESTJS? <a name="key-features"></a>

NESTJS boasts several key features that make it a powerful framework for building backend applications:

- **Modular Architecture**: NESTJS encourages the use of modules to organize and separate functionality.
- **Dependency Injection**: NESTJS utilizes dependency injection to manage component dependencies efficiently.
- **Decorators**: Decorators are used to annotate and define components in NESTJS.
- **Middleware**: Middleware allows for handling requests and responses globally before they reach the route handler.
- **Guards**: Guards are used to control access to routes and resources.
- **Interceptors**: Interceptors intercept the execution flow, enabling preprocessing of requests and responses.
- **Pipes**: Pipes are used for data transformation and validation.
- **Exception Filters**: Exception filters handle exceptions thrown during request processing.
- **WebSockets**: NESTJS supports WebSockets for real-time communication.
- **CLI**: The NESTJS CLI assists in project generation, management, and scaffolding.

## 3. Explain Dependency Injection in NESTJS. <a name="dependency-injection"></a>

Dependency Injection (DI) is a fundamental concept in NESTJS that enables components to declare their dependencies instead of creating them directly. This promotes loose coupling and modular design. NESTJS provides the `@Injectable()` decorator to indicate that a class is injectable, and the `constructor` of the injectable class is used to specify its dependencies.

Example:

```typescript
@Injectable()
class UserService {
  constructor(private readonly userRepository: UserRepository) {}
}
```

## 4. How does Middleware work in NESTJS? <a name="middleware"></a>
Middleware in NESTJS is a way to process requests and responses globally before they reach the route handlers. Middleware functions are executed sequentially in the order they are added to the pipeline. Middleware can modify request/response objects, execute code, or even short-circuit the request-response cycle.

Example:
```typescript
@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log(`Request received: ${req.method} ${req.url}`);
    next();
  }
}
```

## 5. What are Guards in NESTJS? <a name="guards"></a>
Guards in NESTJS are used to protect routes and resources. They determine whether a request should be processed based on certain conditions. Guards can be used for authentication, authorization, role-based access, and more. They implement the CanActivate interface and can return a boolean, a Promise<boolean>, or an Observable<boolean>.

Example:

```typescript
@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean | Promise<boolean> {
    const request = context.switchToHttp().getRequest();
    return AuthService.isAuthenticated(request);
  }
}
```


## 6. What is Interceptor and how is it used in NESTJS? <a name="interceptors"></a>
Interceptors in NESTJS intercept the execution flow of requests and responses, allowing you to modify or transform them. Interceptors can be used for logging, modifying data, or handling errors globally. They implement the NestInterceptor interface and can process requests, responses, or both.

Example:

```typescript
@Injectable()
export class TransformInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(map(data => ({ data })));
  }
}
```

## 7. Explain Pipes in NESTJS and provide some use cases. <a name="pipes"></a>
Pipes in NESTJS are used for data transformation and validation. They can be applied to route parameters, request payloads, and responses. Pipes can transform data types, validate input, and sanitize data. NESTJS provides built-in pipes for common use cases, and you can also create custom pipes.

Example:

```typescript
@Post()
@UsePipes(new ValidationPipe())
create(@Body() createUserDto: CreateUserDto) {
  // ...
}
```
Use cases for pipes:

- Validation: Ensure input data meets specific requirements.
- Data Transformation: Convert data formats or structures.
- Sanitization: Remove unwanted or dangerous data.
- Custom Formatting: Apply custom formatting to input or output.

## 8. What is a Module in NESTJS? <a name="modules"></a>
A module in NESTJS is a way to organize components, controllers, services, and other related features in a structured manner. Modules encapsulate functionality and promote separation of concerns. Each application has at least one root module, and modules can be nested within each other to build complex applications.

Example:

```typescript
@Module({
  imports: [DatabaseModule],
  controllers: [UserController],
  providers: [UserService],
})
export class UserModule {}
```

## 9. How do you handle CORS in NESTJS? <a name="cors"></a>
Cross-Origin Resource Sharing (CORS) can be configured in NESTJS using the CorsModule or by setting CORS options in the createNestApplication function. This allows you to define which origins are allowed to access your API, along with other CORS-related settings.

Example:
```typescript
const app = await NestFactory.create(AppModule);
app.enableCors({
  origin: 'https://example.com',
  methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
  credentials: true,
});
```

## 10. Explain the concept of Filters in NESTJS. <a name="filters"></a>
Filters in NESTJS are used to handle exceptions that occur during request processing. They catch exceptions thrown by route handlers, guards, interceptors, and pipes. Filters can format and modify error responses before sending them to the client. NESTJS provides built-in exception filters, and you can also create custom filters.

Example:
```typescript
@Catch(HttpException)
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {
    const response = host.switchToHttp().getResponse();
    const status = exception.getStatus();
    response.status(status).json({
      statusCode: status,
      message: exception.message,
    });
  }
}
```