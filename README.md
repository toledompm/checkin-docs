# Resumo:

# 1 Introdução:

A epidemia da covid-19 forçou nossa adaptação a várias questões da vida cotidiana. Uma significativa parcela da população brasileira precisou adequar-se às demandas do trabalho, que eram executadas majoritariamente de forma presencial, consolidando as modalidades de trabalho remoto e híbrido, exigidas pela necessidade de afastamento social.

Em se tratando do trabalho híbrido, a flexibilidade do lugar de onde e de quando se exercer as tarefas, com a possibilidade de realizá-las no próprio local de trabalho ou outro de livre escolha, tem se mostrado uma opção viável, trazendo o melhor dos dois mundos, o do trabalho convencional e o do teletrabalho. No entanto, traz consigo seus próprios desafios. Um deles é a organização de ambientes de trabalho compartilhados, dificultando saber quais estações de trabalho estão sendo ocupadas e por quem. Isso levou empresas a desenvolverem suas próprias soluções para esse controle.

Pré pandemia, horários rígidos e supervisão a todo momento era a norma para o mercado de trabalho, no entanto, forçadas a mudar, companhias vêm enxergando cada vez mais os benefícios de uma jornada de trabalho flexível.

“Estamos exercitando um olhar diferente [dentro do RH], que tem a ver com inclusão e respeito às individualidades, e conciliar o que é necessidade da empresa com as possibilidades das pessoas”

- Elisabete Rello, diretora de RH da Bayer no Brasil.

![graph](images/graph-1.png)

Gráfico de Adoção de plano de contingência – Trabalho remoto e horários flexíveis (Fonte: [Estudo Panorama PMEs](https://resultadosdigitais.com.br/blog/panorama-pmes-medidas-trabalho-remoto/))

É importante ressaltar que o movimento para jornadas flexiveis/remotas vem crescendo organicamente a um tempo consideravél, como foi apontado no estudo de [future trends](http://reports.weforum.org/future-of-jobs-2016/employment-trends/) conduzido pelo _World Economic Forum_:

_"...An additional dimension to consider is the general trend towards flexible work, identified by our respondents as one of the biggest drivers of transformation of business models in many industries and therefore also one of the topmost concerns at the national level in many of the Report’s focus countries. Telecommuting, co-working spaces, virtual teams, freelancing and online talent platforms are all on the rise, transcending the physical boundaries of the office or factory floor and redefining the boundary between one’s job and private life in the process. Modern forms of workers’ organization, such as digital freelancers’ unions, and updated labour market regulations are beginning to emerge to complement these new organizational models. The challenge for employers, individuals and governments alike is going to be to work out ways and means to ensure that the changing nature of work benefits everyone..."_

**Tradução Livre**:

_"...Uma dimensão adicional a ser considerada, é a tendência geral para o trabalho flexível, identificado pelos nossos entrevistados como um dos maiores motores de transformação de modelo de negócios em muitos setores e, portanto, também uma das principais preocupações a nível nacional em muitos dos Países em foco do relatório. Teletrabalho, espaços de coworking, equipes virtuais, freelance e plataformas de talentos online estão todos em ascensão, transcendendo os limites físicos do escritório e redefinindo a fronteira entre o trabalho e a vida privada no processo. Formas modernas de organização dos trabalhadores, como sindicatos de freelancers digitais, e regulamentações atualizadas do mercado de trabalho estão começando a surgir para complementar esses novos modelos organizacionais. O desafio para empregadores, indivíduos e governos será encontrar maneiras e meios de garantir que a natureza mutável do trabalho beneficie a todos..."_

Porém, nem todas empresas que estão flexibilizando seus modelos de trabalho, têm a capacidade de desenvolver um sistema de controle, ou mudar para uma suíte de comunicação interna que inclui tal sistema.

A JB3 Investimentos, uma empresa no ramo financeiro de Santa Catarina, é uma dessas empresas, e busca uma solução externa para gerenciar presenças de seus colaboradores.

## 1.1 Objetivos do trabalho:

O objetivo geral deste trabalho é disponibilizar um sistema de reserva e checkin para estações de trabalho, acompanhado de uma interface web para gerenciar o sistema, além de extrair os dados de forma inteligivel.

O sistema será composto por dois aplicativos para dispositivos móveis que consumirão uma API principal:

- **Aplicativo destinado ao usuário**: O aplicativo será o meio que o funcionário usará para verificar disponibilidades, realizar reservas e gerar um código para checkin.
- **Aplicativo Totem**: O objetivo deste aplicativo, é ser implantado em dispositivos disponíveis em áreas comuns, como recepções para que os funcionários consigam entrar com seu código temporário e confirmar seu checkin.
- **API**: A API será responsável por expor rotas que serão consumidas por ambos aplicativos. Todo acesso aos dados será feito por ela, sendo necessário diferentes níveis de permissão para cada funcionalidade.
- **Interface Web**: O site será acessado apenas por usuários permitidos pela organização, com o intuito de gerenciar o sistema.

## 1.2 Conteúdo do trabalho:

# 2 Fundamentação técnica:

Neste capítulo serão abordadas as tecnologias e padrões utilizados para o desenvolvimento do sistema. Assim como justificações para decisões técnicas.

## 2.1 API (Application Programming Interface):

API é uma sigla em inglês, que significa "Interface de Programação de Aplicativo". É um conjunto de definições e protocolos para construir e integrar aplicações de software.

APIs surgiram no início da computação, pré-datando por muito tempo computadores pessoais. Na época, eram tipicamente utilizadas como bibliotecas para sistemas operacionais, quase sempre transmitindo mensagens localmente no mesmo sistema em que estava operando. Aproximadamente 30 anos depois, APIs remotas começaram a se tornar uma importante parte de sistemas de integração de dados.

APIs possibilitam a comunicação entre serviços e produtos, sem necessitar que conheçam mutuamente a implementação um do outro. Isso simplifica o processo de desenvolvimento, economizando tempo e dinheiro.

Uma forma de exemplificar APIs é tratá-las como contratos, com a documentação agindo como acordos entre as aplicações, detalhando o que é esperado que o consumidor forneça em suas requisições e como a resposta será estruturada.

### 2.1.1 REST (Representational State Transfer):

Há muitos protocolos projetados para padronizar APIs, um dos mais conhecidos, e que será utilizado no desenvolvimento deste trabalho, é o padrão REST, que significa "Transferência de Estado Representacional". APIs que aderem ao protocolo REST são denominadas APIs RESTful. Diferente de outros como SOAP, o protocolo REST age mais como um estilo de arquitetura. Isso significa que para uma API ser classificada como RESTful, de acordo com a dissertação de Roy Fielding ([Architectural Styles and the Design of Network-based Software Architectures](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)), deve cumprir as seguintes restrições:

- **Interface uniforme**: Esta é a restrição mais importante a se seguir quando projetando uma API RESTful. Para cumpri-la, é preciso:
  - Identificar recursos durante requisições;
  - Permitir a manipulação de recursos, por meio de suas representações;
  - Enviar mensagens auto-descritivas;

- **Arquitetura cliente-servidor**: A arquitetura é composta por clientes, servidores e recursos. Requisições são feitas utilizando o protocolo http.

- **Statelessness**: Nenhuma informação do cliente é armazenada no servidor. A responsabilidade de armazenar dados sobre o estado da sessão é do cliente.

- **Cacheability**: A API deve armazenar respostas para requisições comuns, para eliminar algumas interações entre as camadas.

- **Sistema em camadas**: As interações entre o cliente e servidor devem ser mediadas com camadas adicionais, como caches compartilhados, load balancers etc.

### 2.1.2 OpenAPI:

A especificação OpenAPI surgiu recentemente, e vem se tornando o padrão definitivo para APIs REST. A especificação detalha formas para desenvolver Interfaces REST, para que usuários consigam entendê-las através de dedução. A especificação OpenAPI será seguida para desenvolver a API deste trabalho.

sources: https://www.redhat.com/en/topics/api/what-are-application-programming-interfaces

### 2.1.3 Tech Stack:

A stack escolhida para o desenvolvimento da API foi Typescript + Node.js.

Node.js é uma das ferramentas mais utilizadas para desenvolvimento, como demonstrado:

![graph](images/graph-2.png)

[Most used libraries, frameworks, and tools among developers, worldwide, as of early 2020](https://www.statista.com/statistics/793840/worldwide-developer-survey-most-used-frameworks)

Devido a popularidade do Node.js, muitos problemas que são normalmente encontrados durante o desenvolvimento de uma API já foram solucionados por sua comunidade. Isso torna a implementação do projeto mais simples, além de contribuir com sua qualidade.

A linguagem typescript foi escolhida como a linguagem principal do projeto pelas suas vantagens quando comparada à javascript tradicional, pois contém:

- Orientação a Objetos: Conceitos como classes, interfaces e herança são suportados por typescript, tornando o código mais organizado.
- Legibilidade: Tipagem explícita torna o código mais auto-explicativo, possibilitando muitas vezes entendê-lo olhando apenas para as assinaturas das funções e atributos dos objetos.
- Debugging: De acordo com o estudo [To Type or Not to Type:
  Quantifying Detectable Bugs in JavaScript](https://earlbarr.com/publications/typestudy.pdf), typescript consegue detectar 15% dos bugs mais comuns de javascript, durante a fase de transpilação.

Embora isso resulte em um código mais verboso, devido ao tamanho do projeto, as vantagens na organização e legibilidade justificam a adoção dessa linguagem.

#### 2.1.3.1 Node.js:

NodeJS é um ambiente de execução JavaScript baseado em eventos assíncronos. Foi projetado para construir aplicativos de rede escalonáveis.

Isso vai contra os modelos de simultaneidade mais comuns de hoje, que utilizam threads de Sistemas Operacionais. Redes baseadas em threads são relativamente ineficientes e muito difíceis de utilizar. Além disso, os usuários do Node.js não precisam se preocupar com deadlocks, já que não há bloqueio de recursos. Quase nenhuma função no Node.js realiza I/O diretamente, portanto, o processo nunca é bloqueado. Como nada bloqueia, sistemas escaláveis ​​são muito razoáveis ​​para desenvolver em Node.js.

sources: https://nodejs.org/en/about

#### 2.1.3.2 NestJS:

NestJS foi a framework escolhida para desenvolver a API. Ela é uma framework Open Source, feita para construir aplicações backend em Node.js de forma versátil, escalável e fracamente acoplada.

Por baixo dos panos, outras frameworks e bibliotecas, já estabelecidas, são utilizadas, como `express` e `typeorm`.

Nest adiciona uma camada de abstração entre o desenvolvedor e as funcionalidades das frameworks que utiliza, porém ainda é possível acessá-las diretamente, possibilitando utilizar as vastas extensões e módulos desenvolvidos por terceiros para estas frameworks.

Com o crescimento da comunidade Node, surgiram várias ferramentas e bibliotecas excelentes, no entanto nenhuma soluciona o problema de **arquitetura**. A framework NestJS soluciona esse problema empacotando essas ferramentas já existentes, e as disponibiliza ao desenvolvedor de uma forma simples, seguindo uma arquitetura desenvolvida baseada no Angular.

sources: https://docs.nestjs.com/

### 2.1.4 Autenticação:

Para escolher a estratégia de autenticação da API, foram definidos os seguintes requisitos:

- O escopo de uma sessão abrange apenas a requisição que a criou;
- A API será responsável por gerar e validar certificados de autenticação;
- Usuários deverão provar sua identidade para receber um certificado.

Levando estes três requisitos em conta, foram escolhidas uma combinação de estratégias. Os dois primeiros requisitos são satisfeitos pela estratégia `JWT`. O último requisito, trata de dados sensíveis como senhas, por isso foi escolhido uma estratégia mais segura, OAuth2 gerenciado por um provedor terceirizado. Com essa combinação, foi possível garantir um nível elevado de segurança, sem adicionar muita complexidade à aplicação, já que a estratégia de autenticação mais complicada foi delegada e conta com a facilidade de integrar estratégias variadas na framework NestJS.

#### 2.3.1 JWT (JSON Web Token):

JSON Web Token é um padrão de mercado aberto, mais especificamente [RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519). É um meio compacto e seguro de transferir declarações entre duas partes, no contexto deste trabalho, entre a API e os aplicativos consumidores.

A autenticação principal será feita através de JWTs, gerados e assinados pela API, contendo, de forma segura, informações necessárias para autenticar requisições de usuários legítimos. Para esta função serão utilizadas bibliotecas disponibilizadas pela framework NestJS. Elas são: `passport-jwt` e `@nestjs/jwt`.

##### 2.3.1.1 @nestjs/jwt:

A biblioteca `@nestjs/jwt` fornece utilidades jwt, baseadas no pacote `auth0/node-jsonwebtoken`

sources: https://github.com/nestjs/jwt

##### 2.3.1.2 passport-jwt:

A biblioteca `passport-jwt` fornece uma estratégia de autenticação de endpoints, utilizando JSON Web Tokens. Seu uso é destinado para proteger APIs RESTful sem o uso de sessões.

sources: http://www.passportjs.org/packages/passport-jwt/

#### 2.3.2 OAuth 2.0:

A autorização gerada pela framework OAuth 2.0 permite que um aplicativo de terceiros obtenha acesso limitado a um serviço HTTP, seja em nome de um proprietário de recurso, orquestrando uma interação de aprovação entre o proprietário do recurso e o serviço HTTP, ou permitindo que o aplicativo de terceiros obtenha acesso em seu próprio nome.

sources: https://datatracker.ietf.org/doc/html/rfc6749

##### 2.3.2.1 OAuth2 via Google API:

O provedor inicial para autenticação OAuth2 escolhido foi a Google, devido a sua presença prevalente no setor corporativo e sua fácil integração.

sources: https://developers.google.com/identity/protocols/oauth2

#### 2.3.3 RBAC (Role Based Access Control):

Controle de acesso baseado em função restringe acesso a certas funcionalidades baseado na função (role) do usuário. Essa estratégia foi utilizada em conjunto com a estratégia JWT para gerenciar acesso a recursos sensíveis, como acesso aos dados de checkin da organização.

sources: https://digitalguardian.com/blog/what-role-based-access-control-rbac-examples-benefits-and-more

## 2.2 Banco de Dados:

O sistema utilizará um banco de dados para armazenar dados sobre os colaboradores e seus check-ins. O banco será acessado apenas pela API.

### 2.2.1 Postgres:

O banco escolhido para a aplicação foi PostgresSQL, que é um poderoso sistema de banco de dados relacional. Seu código é aberto com mais de 30 anos de desenvolvimento ativo, que lhe renderam uma forte reputação de confiabilidade, robustez de recursos e desempenho.

sources: https://www.postgresql.org/

### 2.2.2 Orm:

ORM (Object Relational Mapper) é uma técnica que permite mapear entidades do banco de dados ao objeto que as representa dentro da aplicação. Ultimamente tem sido muito utilizada.

#### 2.2.2.1 TypeOrm:

Typeorm é um ORM escrito para plataformas NodeJS, Browser, Cordova, PhoneGap, Ionic, React Native, NativeScript, Expo, e Electron. Seu objetivo é sempre suportar as mais novas funcionalidades de JavaScript e providenciar suas próprias melhorias para aplicações que trabalham com banco de dados, desde sistemas com poucas tabelas até aplicações em grande escala que lidam com múltiplos bancos de dados.

### 2.3 Front End Frameworks

Front End Frameworks são projetos feitos para organizar a funcionalidade de diversos aspectos de aplicativos frontend, como websites ou aplicações móveis.

#### 2.3.1 React Native

A framework escolhida para desenvolver os aplicativos móveis foi React Native.

# 3 Desenvolvimento:

Neste capítulo será detalhado o processo de desenvolvimento do sistema, desde arquitetura até funções específicas de cada componente.

## 3.1 Arquitetura do sistema (API):

Como citado no capítulo 2, uma das razões que a framework NestJS foi selecionada, foi o fato dela providenciar uma arquitetura "direto da caixa". Essa arquitetura foi usada como base para desenvolver a API.

Assim como a arquitetura do Angular, a NestJS nos dá acesso a módulos. Cada módulo segue o padrão MVC, de forma isolada, fracamente acoplando os componentes.

Dependências entre módulos são resolvidas através de injeção de dependência. O módulo dependente, deve importar o módulo provedor, e injetar o componente que necessita. As dependências do componente importado serão resolvidas pelo módulo provedor.

## 3.2 Módulo App:

O módulo APP tem a responsabilidade de importar todos outros módulos.

```typescript
@Module({
  imports: [...moduleImports, TypeOrmModule.forRoot()],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}
```

[checkin-api/app.module.ts](https://github.com/toledompm/checkin-api/blob/main/src/app/app.module.ts)

Quando o servidor é iniciado, uma nova `INestApplication` é criada a partir do `AppModule`.

```typescript
import { NestFactory } from "@nestjs/core";
import { AppModule } from "src/app/app.module";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

[checkin-api/main.ts](https://github.com/toledompm/checkin-api/blob/main/src/main.ts)

## 3.3 Módulo Auth:

O módulo `AuthModule`, tem a responsabilidade de definir as estratégias de autenticação da aplicação, além de expor rotas para emitir tokens de acesso.

```typescript
@Module({
  imports: [jwtModule, UserModule],
  providers: [...authProviders, ...serviceProviders],
  controllers: [AuthController],
})
export class AuthModule {}
```

[checkin-api/auth.module.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/auth.module.ts)

### 3.3.1 Controller:

O controller expõe duas rotas, `google/login` e `google/redirect`.

```typescript
@Controller("auth")
export class AuthController {
  constructor(
    @Inject(AUTH_SERVICE) private readonly authService: AuthService
  ) {}

  /**
   * Entrypoint para a API OAuth 2.0 do google.
   */
  @Get("google/login")
  @UseGuards(AuthGuard(GOOGLE_AUTH_STRATEGY))
  googleLogin() {}

  /**
   * Após ser validado pelos serviços do google, o usuário é redirecionado de volta,
   * dessa vez, um novo objeto é adicionado a requisição: user. O Objeto user
   * contém informações básicas sobre a conta google do usuário, essa informação
   * é utilizada para encontrar o usuário na base de dados do sistema. Com todos os
   * dados do usuário carregados, um token de autenticação é gerado e retornado.
   */
  @Get("google/redirect")
  @UseGuards(AuthGuard(GOOGLE_AUTH_STRATEGY))
  googleAuthRedirect(@Req() { user }: Request) {
    return this.authService.googleLogin(user);
  }
}
```

[checkin-api/auth.controller.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/auth.controller.ts)

### 3.3.2 AuthService:

O serviço providenciado por este módulo tem duas funções.

- Validar usuários retornados pela API OAuth 2.0 do google, retornando um token de autenticação gerado pela API.
- Buscar usuários com base nos atributos recebidos por decodificar o token.

```typescript
export interface AuthService {
  googleLogin(user: UserDto): Record<string, any>;
  getUserFromTokenAttributes(attributes: UserAuthTokenAtributes): Promise<User>;
}
```

[checkin-api/auth.service.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/auth.service.ts)

### 3.3.3 Estratégias de Autenticação:

O módulo `AuthModule` também é responsável por implementar todas estratégias de autenticação utilizadas pela aplicação. Como as estratégias extendem a classe abstrata `PassportStrategy` providenciada pela framework NestJS, não foi preciso importá-las individualmente, já que, após registradas no módulo `AuthModule`, a framework as disponibiliza através da função `AuthGuard`.

#### 3.3.3.1 Estratégia OAuth 2.0:

A classe `GoogleStrategy` herda os atributos da classe `PassportStrategy`, passando as configurações importadas da biblioteca `passport-google-oauth20`. Com isso, a maior parte da comunicação e redirecionamento já está pronta, basta implementar a função abstrata `validate`.

```typescript
...
import { Strategy, VerifyCallback } from 'passport-google-oauth20';
...
@Injectable()
export class GoogleStrategy extends PassportStrategy(
  Strategy,
  GOOGLE_AUTH_STRATEGY
) {
  constructor() {
    super(Environment.config.auth.google);
  }

  validate(
    _accessToken: string,
    _refreshToken: string,
    profile: any,
    done: VerifyCallback
  ): void {
    /**
     * Primeiro, os dados nome e email são extraídos do perfil enviado pelos
     * serviços Google.
     */
    const { name, emails } = profile;

    /**
     * Os valores extraídos são formatados conforme a entidade `UserDto`, definida
     * pelo módulo `UserModule`.
     */
    const userDto = {
      email: emails[0].value,
      firstName: name.givenName,
      lastName: name.familyName,
    };

    /**
     * O objeto formatado é enviado para a função done, disponibilizada pela biblioteca
     * passport-google-oauth20. Neste ponto, a requisição é redirecionada a rota google/redirect
     * com as informações do usuário prontas para serem validadas.
     */
    done(null, userDto);
  }
}

```

[checkin-api/google.strategy.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/strategies/google.strategy.ts)

#### 3.3.3.2 Estratégia JWT:

Assim como a estratégia anterior, cabe à classe `JwtStrategy` configurar a classe pai com a estratégia fornecida pela biblioteca `passport-jwt`, e implementar a função abstrata `validate`.

```typescript
...
import { Strategy } from 'passport-jwt';
...
@Injectable()
export class JwtStrategy extends PassportStrategy(Strategy, JWT_AUTH_STRATEGY) {
  @Inject(AUTH_SERVICE)
  private readonly authService: AuthService;

  constructor() {
    const { jwtFromRequest, ignoreExpiration, secret } =
      Environment.config.auth.jwt;

    super({
      jwtFromRequest,
      ignoreExpiration,
      secretOrKey: secret,
    });
  }

  async validate(payload: UserAuthTokenAtributes) {
    /**
     * Toda decodificação e autenticação do token é feita pela classe pai, retornando
     * apenas os atributos do token. Cabe a função validate encontrar o usuário na base
     * pelos atributos fornecidos. Para isso é utilizada a função definida no componente
     * AuthService.
     */
    return this.authService.getUserFromTokenAttributes(payload);
  }
}
```

[checkin-api/jwt.strategy.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/strategies/jwt.strategy.ts)

#### 3.3.3.3 Estratégia Role Based Access Control:

A estratégia RBAC, diferente das anteriores, é implementada sem o auxílio da classe abstrata `PassportStrategy`. Esta estratégia é utilizada em conjunto com a estratégia JWT para rotas específicas que podem ser acessadas apenas por um determinado grupo de usuários.

A estratégia depende de dois componentes:

- decorator `Roles`: O componente `Roles` permite especificar o nível de acesso necessário para cada rota.
- classe `RolesGuard`: O componente `RolesGuard` é responsável por comparar o nível de acesso do usuário atual, com o nível especificado pelo decorator.

A comunicação entre os dois componentes é feita através da classe helper `Reflector` e da função `SetMetadata`.

```typescript
import { Reflector } from '@nestjs/core';
import {
 SetMetadata,
 ...
} from '@nestjs/common';
...
export const Roles = (...roles: UserRole[]) => SetMetadata(ROLES_KEY, roles);

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const requiredRoles = this.reflector.getAllAndOverride<UserRole[]>(
      ROLES_KEY,
      [context.getHandler(), context.getClass()]
    );
    if (!requiredRoles) {
      return true;
    }
    const { user }: { user: User } = context.switchToHttp().getRequest();
    return requiredRoles.some((role) => user.role === role);
  }
}

```

[checkin-api/roles.strategy.ts](https://github.com/toledompm/checkin-api/blob/main/src/auth/strategies/roles.strategy.ts)
