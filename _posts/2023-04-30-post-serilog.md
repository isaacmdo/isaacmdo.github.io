---
title: "Logs com .NET + Serilog"
categories:
  - .NET
tags:
  - Logs
  - Monitoramento
---

Serilog é uma biblioteca de logging para .NET que permite que os desenvolvedores capturem e armazenem mensagens de log de forma flexível e eficiente. É altamente configurável e extensível, com suporte para vários destinos de log, como arquivos, bancos de dados, Elasticsearch e muitos outros.

O Serilog é conhecido por sua sintaxe de escrita intuitiva e fácil de usar, que permite aos desenvolvedores criar mensagens de log significativas e legíveis. O Serilog também oferece uma ampla gama de recursos, incluindo suporte para logging estruturado, filtragem de mensagens de log e enriquecimento de mensagens de log com informações adicionais, como contexto de usuário e dados de solicitação.

Exemplo de como usar o Serilog:

```csharp
using Serilog;

class Program
{
    static void Main()
    {
        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console()
            .WriteTo.File("log.txt", rollingInterval: RollingInterval.Day)
            .CreateLogger();

        Log.Information("Hello, Serilog!");

        Log.CloseAndFlush();
    }
}
```

Neste exemplo, estamos criando uma nova instância do logger Serilog e configurando-o para escrever logs tanto no console quanto em um arquivo de log com rotação diária. Em seguida, estamos registrando uma mensagem de log com o nível de informação usando a sintaxe de chamada de método simples e, por fim, fechando o logger.

Este é apenas um exemplo simples, mas o Serilog oferece muitos recursos poderosos para lidar com casos de uso mais complexos. Se você estiver procurando por uma biblioteca de logging flexível e fácil de usar para .NET, o Serilog é definitivamente uma excelente opção a considerar.


Quando há necessidade de registo no banco:

Um caso de uso para o registro de log no banco de dados pode ser o monitoramento de atividades de usuários em um aplicativo web, para rastrear suas ações e identificar possíveis problemas ou anomalias. Nesse caso, seria interessante armazenar informações como a data e hora da ação, o nome do usuário, a página ou recurso acessado e outras informações relevantes.

Aqui está um exemplo de como configurar o Serilog para gravar logs em um banco de dados SQL Server:

```csharp
using Serilog;
using Serilog.Sinks.MSSqlServer;

class Program
{
    static void Main()
    {
        var connectionString = "Server=myServerAddress;Database=myDataBase;User Id=myUsername;Password=myPassword;";
        
        Log.Logger = new LoggerConfiguration()
            .WriteTo.MSSqlServer(connectionString, "Logs")
            .CreateLogger();

        Log.Information("Hello, Serilog!");

        Log.CloseAndFlush();
    }
}
```

Neste exemplo, estamos configurando o Serilog para gravar logs no banco de dados SQL Server usando o pacote NuGet Serilog.Sinks.MSSqlServer. Passamos a string de conexão como parâmetro para o método MSSqlServer, juntamente com o nome da tabela onde os logs serão armazenados ("Logs", neste caso).

Em seguida, estamos registrando uma mensagem de log com o nível de informação usando a sintaxe de chamada de método simples. O Serilog irá gravar esta mensagem no banco de dados configurado.

Lembre-se de que, para utilizar o Serilog com o banco de dados, é necessário criar a tabela correspondente antes de executar o código acima. O pacote Serilog.Sinks.MSSqlServer inclui um script SQL para criação da tabela, que pode ser encontrado no repositório oficial do Serilog no GitHub.

Divercidade de saídas:

O Serilog permite que os desenvolvedores personalizem a saída das mensagens de log em vários formatos, como JSON, XML, texto simples, entre outros. Isso pode ser útil para padronizar a formatação e facilitar o processamento e análise dos logs.

Aqui está um exemplo de como configurar o Serilog para escrever logs no formato JSON:

```csharp
using Serilog;
using Serilog.Formatting.Json;

class Program
{
    static void Main()
    {
        Log.Logger = new LoggerConfiguration()
            .WriteTo.Console(new JsonFormatter())
            .CreateLogger();

        Log.Information("Hello, Serilog!");

        Log.CloseAndFlush();
    }
}
```

Neste exemplo, estamos configurando o Serilog para escrever logs no console no formato JSON, usando o pacote NuGet Serilog.Formatting.Json. Passamos uma instância da classe JsonFormatter como parâmetro para o método Console, que será usada para formatar as mensagens de log.

Em seguida, estamos registrando uma mensagem de log com o nível de informação usando a sintaxe de chamada de método simples. O Serilog irá escrever esta mensagem no console no formato JSON.

A vantagem de usar um formato estruturado como o JSON é que ele permite a fácil integração com ferramentas de análise de log, como o Elasticsearch e Kibana, que podem extrair e visualizar informações de log importantes de forma mais eficiente.

Além disso, o Serilog permite abstrair a lógica de saída de log do código da aplicação, facilitando a substituição ou reconfiguração dos destinos de log sem precisar alterar o código. Por exemplo, se quisermos mudar o formato de saída de log de JSON para texto simples, basta alterar a configuração do Serilog, sem precisar alterar o código da aplicação. Isso é conhecido como separação de preocupações e pode tornar a manutenção do código mais fácil e menos propensa a erros.

