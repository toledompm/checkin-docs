# Resumo:
# 1. Introdução:
A epidemia do coronavírus nos forçou a se adaptar em vários aspectos, um deles, o trabalho remoto, trouxe um novo modelo de trabalho à tona, o modelo flexível. Ele vem se mostrando como uma opção popular, trazendo o melhor dos dois mundos, no entanto, traz consigo seus próprios desafios. Um deles é a organização de ambientes de trabalho compartilhados, dificultando saber quais estações de trabalho estão sendo ocupadas por quem. Isso levou empresas a desenvolverem suas próprias soluções para esse controle.

Pré pandemia, horários rígidos e supervisão a todo momento era a norma para o mercado de trabalho, no entanto, forçadas a mudar, companhias vêm enxergando cada vez mais os benefícios de uma jornada de trabalho flexível.

“Estamos exercitando um olhar diferente [dentro do RH], que tem a ver com inclusão e respeito às individualidades, e conciliar o que é necessidade da empresa com as possibilidades das pessoas”
- Elisabete Rello, diretora de RH da Bayer no Brasil.

![graph](images/graph-1.png)

Gráfico de Adoção de plano de contingência – Trabalho remoto e horários flexíveis (Fonte: [Estudo Panorama PMEs](https://resultadosdigitais.com.br/blog/panorama-pmes-medidas-trabalho-remoto/))

É importante ressaltar que o movimento para jornadas flexiveis/remotas vem crescendo organicamente a um tempo consideravél, como foi apontado no estudo de [future trends](http://reports.weforum.org/future-of-jobs-2016/employment-trends/) conduzido pelo *World Economic Forum*:

*"...An additional dimension to consider is the general trend towards flexible work, identified by our respondents as one of the biggest drivers of transformation of business models in many industries and therefore also one of the topmost concerns at the national level in many of the Report’s focus countries. Telecommuting, co-working spaces, virtual teams, freelancing and online talent platforms are all on the rise, transcending the physical boundaries of the office or factory floor and redefining the boundary between one’s job and private life in the process. Modern forms of workers’ organization, such as digital freelancers’ unions, and updated labour market regulations are beginning to emerge to complement these new organizational models. The challenge for employers, individuals and governments  alike is going to be to work out ways and means to ensure that the changing nature of work benefits everyone..."*

**Tradução Livre**:

*"...Uma dimensão adicional a ser considerada, é a tendência geral para o trabalho flexível, identificado pelos nossos entrevistados como um dos maiores motores de transformação de modelo de negócios em muitos setores e, portanto, também uma das principais preocupações a nível nacional em muitos dos Países em foco do relatório. Teletrabalho, espaços de co-working, equipes virtuais, freelance e plataformas de talentos online estão todos em ascensão, transcendendo os limites físicos do escritório e redefinindo a fronteira entre o trabalho e a vida privada no processo. Formas modernas de organização dos trabalhadores, como sindicatos de freelancers digitais, e regulamentações atualizadas do mercado de trabalho estão começando a surgir para complementar esses novos modelos organizacionais. O desafio para empregadores, indivíduos e governos será encontrar maneiras e meios de garantir que a natureza mutável do trabalho beneficie a todos..."*

Porém, nem todas empresas que estão flexibilizando seus modelos de trabalho, têm a capacidade de desenvolver um sistema de controle, ou mudar para uma suíte de comunicação interna que inclui tal sistema.

## 1.1 Objetivos do trabalho:

O objetivo geral deste trabalho é disponibilizar um sistema de reserva e checkin para estações de trabalho, acompanhado de instruções para implantação e conexão com suítes corporativas já existentes.

O sistema será composto por dois aplicativos para dispositivos móveis que consumirão uma API principal:
- **Aplicativo destinado ao usuário**: O aplicativo será o meio que o funcionário usará para verificar disponibilidades, realizar reservas e gerar um código para checkin.
- **Aplicativo Totem**: O objetivo deste aplicativo, é ser implantado em dispositivos que disponiveis em areas comuns, como recepções para que os funcionários consigam entrar com seu código temporário e confirmar seu checkin.
- **API**: A API será responsável por expor rotas que serão consumidas por ambos aplicativos. Todo acesso aos dados será feito por ela, sendo necessário diferentes níveis de permissão para cada funcionalidade.

## 1.2 Conteúdo do trabalho:
# 2. Fundamentação técnica:
Neste capitulo serão abordadas as técnologias e padrões utilizados para o desenvolvimento sistema. Assim como justificações para decisões técnicas.

## 2.1 API (Application Programming Interface):

API é uma sigla em inglês, significando "Interface de Programação de Aplicativo". É um conjunto de definições e protocolos para construir e integrar aplicações de software.

Elas surgiram no início da computação, pre-datando por muito tempo computadores pessoais. Na época, APIs eram tipicamente utilizadas como bibliotecas para sistemas operacionais, quase sempre transmitindo mensagens localmente, no mesmo sistema em que estava operando. Aproximadamente 30 anos depois, APIs remotas começaram a se tornar uma importante parte de sistemas de integração de dados.

APIs possibilitam a comunicação entre serviços e produtos, sem necessitar que eles conheçam a implementação de cada. Isso simplifica o processo de desenvolvimento, economizando tempo e dinheiro.

Uma forma de exemplificar APIs, seria trata-las como contratos, com a documentação agindo como acordos entre as aplicações, detalhando o que é esperado que o consumidor forneça em suas requisições, e como a resposta será estruturada.

### 2.1.1 REST (Representational State Transfer):

Há muitos protocolos projetados para padronizar APIs, um dos mais famosos, e o que será utilizado no desenvolvimento deste trabalho, é o padrão REST significando "Transferência de Estado Representacional". APIs que aderem ao protoculo REST são denominadas APIs RESTful. Diferente de outros protocolos como SOPA, REST age mais como um estilo de arquitetura. Isso significa que para uma API ser classificada como RESTful, de acordo com a dissertação de  Roy Fielding ([Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)), basta cumprir as seguintes restrições:

- **Interface uniforme**: Esta é a restrição mais importante a se seguir quando projetando uma API RESTful. Para cumpri-la, é preciso:
  - Identificar recursos durante requisições;
  - Permitir a manipulação de recursos, através de suas representações;
  - Enviar mensagens auto-descritivas;

- **Arquitetura cliente-servidor**: A arquitetura é composta por clientes, servidores e recursos. Requisições são feitas utilizando o protocolo http.

- **Statelessness**: Nenhuma informação do cliente é armazenada no servidor. A responsábilidade de armazenar dados sobre o estado da sessão caem sobre o cliente.

- **Cacheabilidade**: Armazenar respostas para requisições comuns, pode eliminar algumas interações entre as camadas.

- **Sistema em camadas**: Mediar interações entre o cliente e servidor com camadas adicionais, como caches compartilhados, load balancers etc.

- **Código sobre demanda (opcional)**: Servidores podem explorar alguma funcionalidade do cliente, exportando código a ser executado.

### 2.1.2 OpenAPI:
A especificação OpenAPI surgiu recentemente, e vem se tornando o pradrão definitivo para APIs REST. A especificação detalha formas para desenvolver Interfaces REST, para que usuários consigam entender-las através de dedução. A especificação OpenAPI será seguida para desenvolver a API deste trabalho.

sources: https://www.redhat.com/en/topics/api/what-are-application-programming-interfaces

## 2.2 NestJS:
## 2.3 Autenticação:
Para escolher a estratégia de autenticação da API, foram definidos os seguintes requisitos:
- O escopo de uma sessão abrange apenas a requisição que a criou;
- A API será responsável por gerar e validar certificados de autenticação;
- Usuários deverão provar sua identidade para receber um certificado;

Levando estes três requisitos em conta, foram escolhidas uma combinação de estratégias. Os dois primeiros requisitos são satisfeitos pela estratégia `JWT`. O último requisito, trata com dados sensíveis como senhas, por isso foi escolhido uma estratégia mais segura, OAuth2 gerenciado por um provedor terceirizado. Com essa combinação, foi possível garantir um nível elevado de segurança, sem adicionar muita complexidade a aplicação, já que a estratégia de autenticação mais complicada foi delegada e a facilidade de integrar estratégias variadas na framework NestJS.

### 2.3.1 JWT (JSON Web Token):
JSON Web Token é um padrão de mercado aberto, mais especificamente [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519). É um meio compacto e seguro de transferir declarações entre duas partes, no contexto deste trabalho, entre a API e os aplicativos consumidores.

A autenticação principal será feita através de JWTs, gerados e assinados pela API, contendo, de forma segura informações necessárias para autentificar requisições de usuários legitimos. Para esta função serão utilizadas bibliotecas disponibilizadas pela framework NestJS. Elas são: `passport-jwt` e `@nestjs/jwt`.

#### 2.3.1.1 @nestjs/jwt:
A biblioteca `@nestjs/jwt` fornece utilidades jwt, baseadas no pacote `auth0/node-jsonwebtoken`

sources: https://github.com/nestjs/jwt

#### 2.3.1.2 passport-jwt:
A biblioteca `passport-jwt` fornece uma estratégia de autenticação de endpoints, utilizando JSON Web Tokens, seu uso é destinado para proteger APIs RESTful sem o uso de sessões.

sources: http://www.passportjs.org/packages/passport-jwt/

### 2.3.2 OAuth 2.0:
A autorização gerada pela framework OAuth 2.0 permite que um aplicativo de terceiros obtenha acesso limitado a um serviço HTTP, seja em nome de um proprietário de recurso orquestrando uma interação de aprovação entre o proprietário do recurso e o serviço HTTP ou permitindo que o aplicativo de terceiros obter acesso em seu próprio nome. 

sources: https://datatracker.ietf.org/doc/html/rfc6749
#### 2.3.2.1 OAuth2 via Google API:
**NEEDS DATA**

O provedor inicial para autenticação OAuth2 escolhido foi a google, devido a sua presença prevalente no setor coorporativo. 

sources: https://developers.google.com/identity/protocols/oauth2
## 2.4 Web Hooks:
## 2.5 Web Security:
# 3. Desenvolvimento:
## 3.1 Arquitetura do sistema
