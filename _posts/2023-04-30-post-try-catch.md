---
title: "Você usa try-catch nos controllers?"
categories:
  - .NET
tags:
  - Boas práticas
  - Desenvolvimento
---

Ao desenvolver um projeto ASP.NET 6, pode ser tentador usar try-catch em cada método do controller para lidar com exceções. Embora isso possa parecer uma boa prática à primeira vista, existem algumas desvantagens em adotar essa abordagem.

Podemos remover facilmente, criando um middleware para tirar o try-catch dos controllers em um projeto ASP.NET 6:

```csharp
using Microsoft.AspNetCore.Http;
using System;
using System.Threading.Tasks;

public class ErrorHandlingMiddleware
{
    private readonly RequestDelegate _next;

    public ErrorHandlingMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await _next.Invoke(context);
        }
        catch (Exception ex)
        {
            // Lógica de manipulação de erros aqui
            // Por exemplo, você pode registrar o erro em um log ou retornar uma resposta de erro personalizada
            context.Response.StatusCode = StatusCodes.Status500InternalServerError;
            await context.Response.WriteAsync("Ocorreu um erro no servidor.");
        }
    }
}
```

Em seguida, você pode registrar o middleware no método Configure do arquivo Program.cs do seu projeto:

```csharp
      // ...

        // Adicione o middleware de manipulação de erros antes dos outros middlewares
        app.UseMiddleware<ErrorHandlingMiddleware>();
        // ...
```

Por que remover o try-catch dos controllers?

1- Redundância de código

Quando usamos try-catch em cada método do controller, estamos repetindo a mesma lógica de tratamento de exceções várias vezes. Isso pode levar a um código redundante e difícil de manter. Além disso, se a lógica de tratamento de exceções precisar ser alterada, todas as ocorrências em cada método do controller precisam ser atualizadas, o que pode ser uma tarefa tediosa e propensa a erros.

2- Dificuldade na identificação de exceções

Quando usamos try-catch em cada método do controller, estamos capturando todas as exceções que ocorrem em um bloco de código, incluindo as exceções que não esperávamos. Isso pode dificultar a identificação de exceções específicas que precisam ser tratadas de maneira diferente. Além disso, ao capturar todas as exceções em um bloco de código, podemos estar ocultando exceções importantes que precisam ser tratadas em níveis superiores da aplicação.

3- Dificuldade em testar o código

Quando usamos try-catch em cada método do controller, estamos introduzindo lógica de tratamento de exceções em nossos métodos de negócios. Isso pode dificultar os testes unitários, pois agora precisamos testar a lógica de tratamento de exceções juntamente com a lógica de negócios. Isso pode levar a testes mais complexos e demorados, o que pode afetar negativamente a qualidade do nosso código.

3- Centralização do tratamento de exceções

Remover o try-catch dos controllers permite que o tratamento de exceções seja centralizado em um único lugar, em vez de ser repetido em cada método do controller. Isso torna mais fácil identificar e tratar exceções específicas, reduz a redundância do código e simplifica os testes de unidade.

Uma abordagem melhor seria usar um middleware de manipulação de erros, como o que foi mostrado no exemplo acima. O middleware de manipulação de erros pode capturar exceções em um nível superior da aplicação, antes de chegar aos controllers, permitindo que você trate as exceções de forma centralizada e personalizada, como registrar o erro em um log ou retornar uma resposta de erro personalizada.
