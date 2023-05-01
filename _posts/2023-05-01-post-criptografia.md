---
title: "Princícios da criptografia com JWT"
categories:
  - Blog
tags:
  - Autenticação
---

Um JWT é uma string codificada que consiste em três partes separadas por pontos: o cabeçalho, o payload e a assinatura. O cabeçalho contém informações sobre o tipo de token e o algoritmo de criptografia usado para proteger o token. O payload contém informações sobre o usuário, como nome e ID. A assinatura é usada para verificar se o token é autêntico e não foi modificado durante a transmissão.

![JWT-PRINCIPIO](/assets/images/post-jwt/jwt-exemple.png)

Em uma aplicação web que usa autenticação JWT, os princípios da criptografia token são aplicados da seguinte forma:

Geração do token: Um token JWT é gerado quando um usuário faz login na aplicação. O token contém informações sobre o usuário, como o nome de usuário e as permissões de acesso.

Armazenamento do token: O token JWT é armazenado no lado do cliente, geralmente em um cookie ou no armazenamento local do navegador. O servidor não armazena o token JWT, mas usa a chave de assinatura para verificar se o token é válido.

Acesso ao token: Quando um usuário tenta acessar uma rota protegida, ele ou ela precisa enviar o token JWT correspondente. O servidor verifica a validade do token JWT usando a chave de assinatura e permite ou nega o acesso com base nas permissões contidas no token.

Segurança do token: O token JWT é criptografado usando uma chave de assinatura secreta, que é armazenada apenas no servidor. Isso garante que o token não possa ser falsificado ou manipulado por um invasor. Além disso, a conexão entre o cliente e o servidor é protegida por SSL / TLS, o que garante que o token não possa ser interceptado por um terceiro.

Proteção de dados: A autenticação JWT é usada principalmente para proteger rotas e recursos que requerem autenticação. A técnica de criptografia token ajuda a prevenir o acesso não autorizado e a garantir a segurança dos dados confidenciais da aplicação.

Em resumo, a autenticação JWT é um exemplo de como os princípios da criptografia token são aplicados em uma aplicação web. O uso de tokens JWT garante a segurança das informações confidenciais do usuário e protege a aplicação contra acesso não autorizado.
