---
title: "Serilog com Elasticsearch + Kibana"
categories:
  - .NET
tags:
  - Logs
  - Monitoramento
---

O Elasticsearch foi criado para lidar com grandes quantidades de dados e oferecer uma plataforma de busca e análise de dados poderosa e fácil de usar.

O Elasticsearch pode ser utilizado em conjunto com o Serilog, que é uma biblioteca de logging para .NET, para armazenar e analisar registros de logs em tempo real. A principal vantagem de utilizar o Elasticsearch com o Serilog é a capacidade de centralizar os logs de diversas aplicações em um único local, o que permite uma análise mais completa e eficiente dos registros de log.

Um caso de uso comum é em aplicações web, onde múltiplos servidores podem estar gerando logs simultaneamente, é possível centralizar os logs gerados por cada servidor de diversas aplicações em um único local. Isso permite aos desenvolvedores monitorar o desempenho do sistema, identificar possíveis problemas e rastrear transações específicas.

Além disso, o Elasticsearch permite a criação de visualizações e dashboards personalizados para a análise dos logs, o que facilita a identificação de padrões e tendências nos registros de log. Isso pode ajudar a identificar gargalos no sistema, problemas de segurança e outras informações úteis para a melhoria contínua do sistema, também é possível criar alertas para monitorar o desempenho da aplicação em tempo real, permitindo que o suporte possa agir imediatamente caso algum problema seja identificado.

Uso com Serilog

Usar o Serilog com o Elasticsearch é uma boa opção para armazenar e consultar logs em grande escala. O Elasticsearch é um mecanismo de busca distribuído que permite a análise e visualização de dados em tempo real. Usando o Serilog, podemos enviar logs diretamente para o Elasticsearch, permitindo a fácil consulta e análise de dados de log.

Aqui está um exemplo de como configurar o Serilog para enviar logs para o Elasticsearch:

```csharp
using Serilog;
using Serilog.Sinks.Elasticsearch;

class Program
{
    static void Main()
    {
        var options = new ElasticsearchSinkOptions(new Uri("http://localhost:9200"))
        {
            AutoRegisterTemplate = true,
            IndexFormat = "myapp-{0:yyyy.MM.dd}"
        };

        Log.Logger = new LoggerConfiguration()
            .WriteTo.Elasticsearch(options)
            .CreateLogger();

        Log.Information("Hello, Serilog!");

        Log.CloseAndFlush();
    }
}
```

Neste exemplo, estamos configurando o Serilog para enviar logs para o Elasticsearch usando o pacote NuGet Serilog.Sinks.Elasticsearch. Passamos uma instância da classe ElasticsearchSinkOptions como parâmetro para o método Elasticsearch, que contém as opções de configuração para o Elasticsearch.

Estamos definindo a URL do Elasticsearch como http://localhost:9200, habilitando o registro automático de modelos de índice (AutoRegisterTemplate = true) e definindo o formato do índice como myapp-{0:yyyy.MM.dd}. Isso significa que o Serilog criará um índice para cada dia, com o formato myapp-ano.mês.dia (por exemplo, myapp-2023.04.30).

Em seguida, estamos registrando uma mensagem de log com o nível de informação usando a sintaxe de chamada de método simples. O Serilog enviará esta mensagem para o Elasticsearch, que irá armazená-la no índice correspondente.

A vantagem de usar o Elasticsearch com o Serilog é que podemos usar as poderosas funcionalidades de busca e análise do Elasticsearch para explorar e visualizar nossos logs em tempo real. Além disso, o Serilog permite enviar logs para vários destinos ao mesmo tempo, então podemos enviar logs para o console, arquivo e Elasticsearch simultaneamente, sem precisar alterar o código da aplicação.

Kibana

Kibana é uma ferramenta de visualização e análise de dados que funciona em cima do Elasticsearch. Com o Kibana, podemos extrair insights valiosos dos nossos dados de log, identificando padrões, erros e comportamentos anômalos em tempo real.

Para usar o Kibana, precisamos primeiro ter o Elasticsearch instalado e configurado. Em seguida, podemos instalar o Kibana e acessá-lo através do navegador. Aqui estão os passos básicos para começar a usar o Kibana:

1- Acesse a página de download do Kibana (https://www.elastic.co/downloads/kibana) e baixe a  versão correspondente ao seu sistema operacional.

2- Extraia os arquivos do Kibana para um diretório de sua escolha.

3- Abra o arquivo kibana.yml em um editor de texto e configure a URL do Elasticsearch:

```less
elasticsearch.hosts: ["http://localhost:9200"]
```

4- Inicie o Kibana executando o comando correspondente ao seu sistema operacional (por exemplo, no Windows, execute .\bin\kibana.bat a partir do diretório do Kibana).

5- Abra o navegador e acesse http://localhost:5601 para acessar a interface do Kibana

Uma vez conectado ao Kibana, podemos criar índices no Elasticsearch e configurar consultas de pesquisa para extrair informações úteis dos nossos dados de log. Podemos também criar painéis personalizados que exibam informações relevantes de forma clara e intuitiva.

O Kibana oferece várias opções de visualização de dados, incluindo gráficos de barra, tabelas, mapas e mais. Podemos também configurar alertas para sermos notificados de eventos importantes, como erros críticos ou picos de tráfego.

No geral, o Kibana é uma ferramenta poderosa para visualização e análise de dados de log e pode ajudar a identificar problemas rapidamente e tomar ações corretivas para garantir a saúde e desempenho do sistema.


