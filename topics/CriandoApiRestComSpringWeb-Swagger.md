
# Criando uma API REST Documentada com Spring Web e Swagger

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Ganhando produtividade com Java e Spring Boot
Instrutor: Gleyson Sampaio - 29/08/23 a 29/10/23
```

## Índice

- [Visão geral do curso](#visão-geral-do-curso)
- [Objetivos](#objetivos)
- [Intro de nivelamento](#intro-de-nivelamento)
- [Criando uma REST API](#criando-uma-rest-api)
- [REST Controller](#restcontroller)
- [Documentando API com SWAGGER](#documentando-api-com-swagger)
- [Habilitando o tratamento de exceções de negócio com handlers](#habilitando-o-tratamento-de-exceções-de-negócio-com-handlers)
- [Referências](#referencias)

## Apresentação inicial

### Visão geral do curso

Nesse curso veremos os recursos dispopnibilizados pelo Framework Spring, usando o Sprin Boot para criação de uma REST API documentada com SWAGGER.

## Objetivos

- Criar um projeto Web com o Spring Web REST API
- Configurar os Controllers
- Documentar a API com Swagger
- Fazer o tratamento de exceções com Handlers


## Intro de nivelamento

**API**

Uma API (interface application program) é um código programável que faz a “ponte” de comunicação entre duas aplicações distintas.

**API REST**

Uma API REST, ou API Representational State Transfer, é um estilo de arquitetura de software para projetar sistemas distribuídos, como serviços web. 

Ela é baseada em princípios simples que utilizam o protocolo HTTP (Hypertext Transfer Protocol) para criar interfaces de programação de aplicativos (APIs) que são fáceis de entender, usar e manter. Abaixo estão os principais conceitos de uma API REST:

- **Recursos (Resources)**: Em uma API REST, os dados são representados como recursos, que podem ser objetos, informações ou qualquer entidade que você deseje expor pela API. Cada recurso é identificado por um URL (Uniform Resource Locator).

- **Operações HTTP**: As operações HTTP padrão, como GET (para recuperar informações), POST (para criar novos recursos), PUT (para atualizar recursos existentes) e DELETE (para remover recursos), são usadas para interagir com os recursos.

- **Estado (Stateless)**: Uma API REST é stateless, o que significa que cada solicitação do cliente para o servidor deve conter todas as informações necessárias para entender e processar a solicitação. Não há estado da sessão armazenado no servidor entre as solicitações. Isso torna as APIs REST altamente escaláveis.

- **Representações**: Os recursos podem ter várias representações, como JSON, XML, HTML, etc. Os clientes solicitam a representação desejada dos recursos. Por exemplo, um mesmo recurso pode ser disponibilizado em formato JSON ou XML.

- **HATEOAS** (*Hypermedia as the Engine of Application State*): É um princípio da API REST que incentiva a inclusão de links em respostas, permitindo que os clientes naveguem na API de forma dinâmica, descobrindo os recursos relacionados. Isso melhora a descoberta e a navegação na API.

- **Identificação Única (Uniform Interface)**: As URLs devem ser estruturadas de forma consistente e padronizada. Por exemplo, para acessar um recurso, você deve usar URLs descritivas e previsíveis.

- **Semântica (Semantics)**: As operações da API REST devem ser semânticas, ou seja, o significado de cada operação deve ser claramente definido. Por exemplo, o uso do método HTTP GET deve ser para recuperar informações, e o método HTTP POST deve ser para criar novos recursos.

**REST e RESTful**

A API REST (representational state transfer) é como um guia de boas práticas e  RESTful é a capacidade de determinado sistema aplicar os princípios de REST.

**Spring Web REST API**

O Spring Web REST API é uma parte do ecossistema Spring Framework dedicada ao desenvolvimento de APIs RESTful em aplicações Java. APIs RESTful (Representational State Transfer) são um estilo de arquitetura de software que utiliza os princípios do HTTP (Hypertext Transfer Protocol) para criar serviços web que são simples, escaláveis e fáceis de consumir.

O Spring REST, fornece ferramentas e bibliotecas para criar, implantar e gerenciar APIs RESTful de maneira eficiente. Alguns dos principais componentes e recursos do Spring Web REST API incluem:

- **Spring Web MVC**: O Spring Web REST API baseia-se no Spring Web MVC, que é o módulo do Spring Framework para desenvolvimento de aplicativos web. Ele permite criar controladores que respondem a solicitações HTTP e retornam dados em formato JSON ou XML, comumente usados em APIs RESTful.

- **Anotações REST**: O Spring Framework fornece anotações como @RestController e @RequestMapping que tornam mais fácil definir endpoints da API, mapear métodos HTTP (GET, POST, PUT, DELETE, etc.) e criar respostas JSON ou XML.

- **Conversão de Dados**: O Spring REST API oferece suporte para a conversão automática de objetos Java em formatos de saída comuns, como JSON e XML. Isso permite que os dados sejam transmitidos pela API de forma eficiente e legível.

- **Validação de Dados**: O Spring suporta a validação de dados de entrada, ajudando a garantir que os dados enviados à API estejam em conformidade com as regras de negócios.

- **Documentação da API**: O Spring REST API é frequentemente usado em conjunto com ferramentas como o Swagger para gerar documentação interativa e amigável para a API.

- **Segurança**: O Spring Security pode ser integrado para proteger os endpoints da API com autenticação e autorização.

- **Tratamento de Erros**: O Spring Web REST API facilita o tratamento e a personalização de respostas de erro, fornecendo feedback significativo ao consumidor da API.

**Swagger**

O `Swagger` é um conjunto de ferramentas de código aberto independente que auxilia no design, na construção, na documentação e no consumo de APIs (Application Programming Interfaces). 

Ele é amplamente usado para criar documentação interativa e amigável para APIs da web, tornando mais fácil para desenvolvedores e equipes entenderem, testarem e consumirem APIs.


## Projeto Spring Boot REST API

### Criando uma REST API

1. Utilizar o [Spring Initializr](https://start.spring.io/) para criar a estrutura do projeto
2. Selecione a opção Maven Project
3. Selecione a opção Language - Java

**Preenchimento**

- Group: Nome do grupo organizacional: Dio
- Artifact: Identificação do projeto: my-first-web-api
- Name: Nome do Projeto (igual ao artifact)
- Description: Descrição do Projeto: opcional
- Package Name: Nome do pacote raíz da sua aplicação: dio.web.api
- Packaging: Tipo de Build da sua aplicação, pode manter .jar
- Java: Versão do Java JDK e JRE que está utilizando

**Adicionar as dependências**

- Adicione a dependencia Spring Web: Build web, including RESTful, applications using Spring MVC. Uses Apache Tomcat as the default embedded container.

- Após criação do projeto e atualização dos arquivos, o arquivo pom.xml everá conter a seguinte tag:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
Essa configuração garante o uso de um servidor web embutido que é o Apache TomCat como servidor padrão operando na porta 8080, além de implementar o modelo RESTful.

### Controllers

Um controller é um recurso que disponibiliza as funcionalidades de negócio da aplicação, através do protocolo HTTP.  Vamos criar uma classe de exemplo:

Crie a classe WelcomeController.java

```java
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.GetMapping;

@RestController
public class WelcomeController {
    @GetMapping("/welcome")
    public String welcome(){
        return "Welcome to a Spring Boot REST API";
    }
}
```
Comentários sobre o código:

`@RestController`: Esta anotação marca a classe WelcomeController como um controlador Spring, indicando que ele é responsável por lidar com solicitações HTTP e gerar respostas. No contexto de uma API REST, os controladores são responsáveis por definir os endpoints da API e as operações que podem ser executadas nesses endpoints. A partir daí o Spring Boot passa a enxergá-lo como um Componente Rest.    

`import org.springframework.web.bind.annotation.GetMapping;`: Esta linha importa a anotação @GetMapping do Spring Framework, que é usada para mapear uma operação de leitura (HTTP GET) para um método específico do controlador.

`@GetMapping("/welcome")`: Aqui, a anotação @GetMapping é usada para mapear a operação HTTP GET no caminho (URL) "/welcome" para o método welcome(). Isso significa que quando alguém fizer uma solicitação HTTP GET para o URL "/welcome", o método welcome() será executado.

`public String welcome() {`: Este é o início do método welcome(), que é chamado quando uma solicitação GET é feita para "/welcome". Ele é uma operação que não recebe parâmetros e retorna uma string.

`return "Welcome to a Spring Boot REST API";`: Dentro do método welcome(), há uma instrução return que retorna a string "Welcome to a Spring Boot REST API". Essa string será a resposta da API quando alguém acessar o endpoint "/welcome" usando um navegador da web ou um cliente de API. A resposta é uma mensagem simples que informa que o usuário está acessando uma API REST do Spring Boot.

É aconselhavel criar essa classe dentro do pacote Controller:

### RESTController

Um ***Rest Controller*** em Spring, nada mais é que uma classe contendo anotações específicas para a disponibilização de recursos HTTPs, baseados em nossos serviços e regras de negócio.

Anotações e configurações mais comuns:

- **@RestController**: Responsável por designar o bean de compoment, que surporta requisições HTTP com base na arquitetura REST.

- **@RequestMapping("prefix")**: Determina qual a URI comum para todos os recursos disponibilizados pelo Controller.

- **@GetMapping**: Determina que o método aceitará requisições HTTP do tipo GET.

- **@PostMapping**: Determina que o método aceitará requisições HTTP do tipo POST.

- **@PutMapping**: Determina que o método aceitará requisições HTTP do tipo PUT.

- **@DeleteMapping**: Determina que o método aceitará requisições HTTP do tipo DELETE.

- **@RequestBody**: Converte um JSON para o tipo do objeto esperado como parâmetro no método.

- **@PathVariable**: Consegue determinar que parte da URI será composta por parâmetros recebidos nas requisições. 


Para exemplificar o uso dessas anotações vamos simular um sistema de gestão de usuário User.java sobre o qual executaremos nosso CRUD.

Para implementação do Sistema proposto usamos as seguintes anotações:

- @Autowired
- @GetMapping
- @PathVariable
- @RestController
- @RequestBody
- @RequestMapping


### Documentando API com SWAGGER

Swagger é uma linguagem de descrição de interface, para descrever APIs RESTful expressas usando JSON. O Swagger é usado junto com um conjunto de ferramentas de software de código aberto para projetar, construir, documentar e usar serviços da Web RESTful.

![Swagger Interface](./img/m7-swaggerInterface.png)

Além de documentar, o Swagger permite testar nossa API sem uso de programas externos como o Postman.

### Configurando o projeto com Swagger

1. Abra o arquivo pom.xml e adicione as dependências do Swagger conforme código abaixo:

```xml
<!-- SWAGGER DOCUMENTACAO -->
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger2</artifactId>
	<version>2.9.2</version>
</dependency>
<dependency>
	<groupId>io.springfox</groupId>
	<artifactId>springfox-swagger-ui</artifactId>
	<version>2.9.2</version>
</dependency>
```

2. Crie um pacote para conter todas as classes responsáveis pela nossa documentação.

3. Crie uma classe chamada `SwaggerConfig.java` e adicione os métodos abaixo:

```java
import java.util.Arrays;
import java.util.HashSet;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration
@EnableSwagger2
public class SwaggerConfig {
     
//...INSIRA OS CÓDIGOS ABAIXO

    private Contact contato() {
		return new Contact(
				"Seu nome",
				"http://www.seusite.com.br", 
				"voce@seusite.com.br");
	}

}
```
4. Crie na classe `SwaggerConfig.java` o  método para retornar Informações da API:

```java
private ApiInfoBuilder informacoesApi() {
		 
		ApiInfoBuilder apiInfoBuilder = new ApiInfoBuilder();
 
		apiInfoBuilder.title("Title - Rest API");
		apiInfoBuilder.description("API exemplo de uso de Springboot REST API");
		apiInfoBuilder.version("1.0");
		apiInfoBuilder.termsOfServiceUrl("Termo de uso: Open Source");
		apiInfoBuilder.license("Licença - Sua Empresa");
		apiInfoBuilder.licenseUrl("http://www.seusite.com.br");
		apiInfoBuilder.contact(this.contato());
 
		return apiInfoBuilder;
 
	}
```
5. Crie um Docket (documento) em forma de @Bean:

```java
@Bean
public Docket detalheApi() {
	Docket docket = new Docket(DocumentationType.SWAGGER_2);
 
	docket
	.select()
	.apis(RequestHandlerSelectors.basePackage("dio.api.controller"))
	.paths(PathSelectors.any())
	.build()
	.apiInfo(this.informacoesApi().build())
	.consumes(new HashSet<String>(Arrays.asList("application/json")))
	.produces(new HashSet<String>(Arrays.asList("application/json")));
	
	return docket;
}
```
- ⚠️ Linha 7 - Deve conter o nome do pacote que estão os Controllers da aplicação "dio.api.controller"

6. Após iniciar a aplicação, acesse os recursos através do link: [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html) 

### Habilitando o tratamento de exceções de negócio com handlers

O tratamento de exceção na ciência da computação, é o mecanismo responsável pelo tratamento da ocorrência de condições, que alteram o fluxo normal da execução, de programas de computadores.

![Tratamento de exceções](./img/m7-exceptionHandlers.png)

No Spring Boot esses tratamentos são implementados através dos "exception handlers". Esses componentes lidam com exceções (ou erros) que ocorrem durante a execução de um aplicativo Spring. 

Eles são usados para capturar exceções lançadas por seu aplicativo e tomar medidas apropriadas, como retornar uma resposta de erro amigável ao cliente ou realizar algum tipo de tratamento de erro personalizado.

Em resumo, um `manipulador de exceção` (Exception Handler) é o código que estipula o que um programa fará quando um evento anômalo, interromper o fluxo normal das instruções desse programa.


### RestControllerAdvice

Spring 3.2 traz suporte para um `@ExceptionHandler` global, com a  anotação @ControllerAdvice.  A anotação `@ControllerAdvice` nos permite consolidar nossos múltiplos `@ExceptionHandlers`, espalhados em um único componente global de tratamento de erros. VAntagens:

- Isso nos dá controle total sobre o corpo da resposta, bem como o código de status.
- Ele fornece o mapeamento de várias exceções ao mesmo método, para serem tratadas em conjunto.
- Ele faz bom uso da resposta RESTful ResposeEntity mais recente .

Estratégias de uso dos Exception Handlers

Definir as exceções do Negócio

Customização das mensagens

## Conclusão

Nesse tópico foram abordados sos seguintes tópicos:

- Criação de um projeto Web
- Estruturação de Controllers

    - @Autowired
    - @GetMapping
    - @PathVariable
    - @RestController
    - @RequestBody
    - @RequestMapping

- Documentação da API com Swagger
- Habilitação de tratamento de exceções
- Interação com API via Postman

## Referencias

- [GitHub do curso](https://github.com/digitalinnovationone/dio-springboot/tree/main/springboot-web-swagger)

- [Conceitos sobre Rest e restful](https://becode.com.br/o-que-e-api-rest-e-restful/)

- [API REstful](https://4success.com.br/api-rest-e-restful/)

- [Exception Handlers](https://wwww.baeldung.com/exception-handling-for-rest-with-spring)

- [Exception Handlers Server Side](https://www.theserverside.com/definition/exception-handler)

## Certificado

![Certificado](./img/m3-certificadoApiRestSwaggerHandlers.png)

```
Disclaimer:

Todo o material aqui apresentado foi gerado a partir de minhas anotações 
de aula durante o excelente treinamento ministrado  pela Instrutor 
Gleyson Sampaio. Proibida a reprodução e veiculação sem ciência e 
autorização da DIO e do autor.
```