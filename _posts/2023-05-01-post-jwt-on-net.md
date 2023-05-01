---
title: "Implementação JWT em .NET"
categories:
  - .NET
tags:
  - Autenticação
---

A autenticação é uma das principais preocupações em aplicativos modernos. Com a popularidade da arquitetura RESTful, a autenticação por token se tornou uma das maneiras mais populares de proteger APIs. JSON Web Token (JWT) é uma dessas soluções que torna a autenticação por token segura e eficiente.

Neste tutorial, iremos implementar a autenticação JWT em um aplicativo .NET.

1. Instale o pacote NuGet System.IdentityModel.Tokens.Jwt para o seu projeto .NET.

2. Configure as informações de autenticação em seu arquivo appsettings.json:

```json
{
  "JwtSettings": {
    "SecretKey": "my_secret_key",
    "Issuer": "my_issuer",
    "Audience": "my_audience",
    "ExpirationInMinutes": 30
  }
}
```
3. Crie uma classe JwtSettings para representar as configurações JWT:
   
```csharp
public class JwtSettings
{
    public string SecretKey { get; set; }
    public string Issuer { get; set; }
    public string Audience { get; set; }
    public int ExpirationInMinutes { get; set; }
}
```
4. Configure as informações JWT em seu arquivo Startup.cs:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    var jwtSettings = Configuration.GetSection("JwtSettings").Get<JwtSettings>();
    services.AddSingleton(jwtSettings);
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidIssuer = jwtSettings.Issuer,
                ValidAudience = jwtSettings.Audience,
                IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(jwtSettings.SecretKey))
            };
        });
    // ...
}
```
5. Crie um método para gerar tokens JWT:

```csharp
public string GenerateJwtToken(string userId, JwtSettings jwtSettings)
{
    var tokenHandler = new JwtSecurityTokenHandler();
    var key = Encoding.ASCII.GetBytes(jwtSettings.SecretKey);
    var tokenDescriptor = new SecurityTokenDescriptor
    {
        Subject = new ClaimsIdentity(new[]
        {
            new Claim(ClaimTypes.Name, userId)
        }),
        Expires = DateTime.UtcNow.AddMinutes(jwtSettings.ExpirationInMinutes),
        SigningCredentials = new SigningCredentials(new SymmetricSecurityKey(key), SecurityAlgorithms.HmacSha256Signature)
    };
    var token = tokenHandler.CreateToken(tokenDescriptor);
    return tokenHandler.WriteToken(token);
}
```

6. Crie um endpoint de login que gera um token JWT:

```csharp
[HttpPost("login")]
public IActionResult Login([FromBody] LoginRequest model)
{
    // Verifica as credenciais do usuário
    var user = _userService.Authenticate(model.Username, model.Password);
    if (user == null)
    {
        return BadRequest(new { message = "Nome de usuário ou senha incorretos" });
    }

    // Gera o token JWT
    var token = GenerateJwtToken(user.Id.ToString(), _jwtSettings);

    // Retorna o token JWT
    return Ok(new { token });
}
```

Obs.: O método "Authenticate" não é diretamente ligado a autenticação JWT, será sua lógica para autenticar o usuário, exemplo:

```csharp
public class UserService
{
    private List<User> _users = new List<User>
    {
        new User { Id = 1, Username = "user1", Password = "password1" },
        new User { Id = 2, Username = "user2", Password = "password2" }
    };

    public User Authenticate(string username, string password)
    {
        var user = _users.SingleOrDefault(x => x.Username == username && x.Password == password);

        if (user == null)
            return null;

        // remove a senha do usuário antes de retornar
        user.Password = null;

        return user;
    }
}
```

7. Proteja seus endpoints com a autenticação JWT:

```csharp
[Authorize]
[HttpGet("me")]
public IActionResult GetCurrentUser()
{
    // Retorna as informações do usuário atual
    var userId = User.FindFirst(ClaimTypes.Name)?.Value;
    var user = _userService.GetById(userId);
    if (user == null)
    {
        return NotFound();
    }
    return Ok(user);
}
```

Lembre-se que este é o princípio da uma autenticação JWT, podemos fazer uma customização sobre cada etapa conforme a necessidade do projeto.