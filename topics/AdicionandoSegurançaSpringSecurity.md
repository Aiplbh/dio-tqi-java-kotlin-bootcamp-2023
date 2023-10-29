# Adicionando Segurança a uma API REST com Spring Security

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Ganhando produtividade com Java e Spring Boot
Instrutor: Gleyson Sampaio - 29/08/23 a 29/10/23
```

## Índice

- [Apresentação inicial](#apresentação-inicial)
- [Objetivos](#objetivos)
- [Intro de nivelamento](#intro-de-nivelamento)
- [Habilitando segurança com Spring](#habilitando-segurança-com-spring)
- [Autenticação simples](#autenticação-simples)
- [Configuração do Adapter](#configuração-do-adapter)
- [Autenticação com Banco de Dados](#autenticação-com-banco-de-dados)
- [JWT (Jason Web Token) Parte 1](#jwt-jason-web-token-parte-1)
- [JWT (Jason Web Token) Parte 2](#jwt-jason-web-token-parte-2)
- [JWT (Jason Web Token) Parte 3](#jwt-jason-web-token-parte-3)
- [JWT (Jason Web Token) Parte 4](#jwt-jason-web-token-parte-4)
- [Conclusão](#conclusão)
- [Referências](#referencias)

## Apresentação inicial

Nesse curso iremos explorar os seguintes tópicos sobre Spring Boot Security:

- Introdução sobre segurança
- Habilitando a segurança com Spring
- Configuração de Adapters
- Autenticação com banco de dados
- Implementação de uma funcionalidade de JWT

### Intro de nivelamento

**Terminologias**

**Autenticação** ("Quem é esse usuário ?")

Refere-se ao processo de verificação da identidade de um usuário, com base nas credenciais fornecidas. Um exemplo comum é inserir um nome de usuário e uma senha ao fazer login em um site. 

**Autorização** (O que o usuário pode fazer?)

Refere-se ao processo de determinar se um usuário tem permissão adequada para executar uma ação específica ou ler dados específicos, supondo que o usuário seja autenticado com exito.

**Princípio**

Refere-se ao usuário autenticado no momento. 

**Autoridade concedida**

Refere-se à permissão do usuário autenticado.

**Função** 
Refere-se a um grupo de permissões do usuário autenticado.


**O que é o Spring Security**

O Spring Security é um projeto da família Spring que fornece recursos de segurança para aplicações baseadas em Java, incluindo aplicações Spring Boot.

Ele é uma estrutura poderosa e flexível para lidar com autenticação e autorização, o que o torna uma escolha popular para proteger aplicações web e serviços.

O Spring Security oferece diversas funcionalidades essenciais, incluindo:

- **Autenticação**: Permite que os desenvolvedores autentiquem os usuários de suas aplicações. Isso envolve a verificação das credenciais dos usuários, como nome de usuário e senha, em um sistema de armazenamento seguro, como um banco de dados.

- **Autorização**: Controla o acesso dos usuários a diferentes partes da aplicação ou ações específicas. Isso garante que os usuários tenham permissão para realizar ações somente após a autenticação e a validação de suas permissões.

- **Proteção contra ataques de segurança**: O Spring Security ajuda a proteger contra ameaças comuns, como ataques de injeção, ataques de sessão e outros vetores de ataque comuns.

- **Gestão de Sessão**: Gerencia a sessão do usuário de forma segura, permitindo o controle sobre o tempo de expiração da sessão e o controle de sessões ativas.

- **Integração com diversos sistemas de autenticação**: O Spring Security é altamente configurável e permite integrar com sistemas de autenticação externos, como LDAP, OAuth, OpenID Connect, SAML, e outros.

- **Personalização e Extensibilidade**: Os desenvolvedores podem personalizar e estender o Spring Security para atender às necessidades específicas de suas aplicações.

O Spring Security é frequentemente usado em aplicações web baseadas em Spring, incluindo aquelas construídas com o framework Spring Boot. 

Ele fornece uma camada de segurança sólida e permite que os desenvolvedores controlem quem pode acessar as partes protegidas de suas aplicações, garantindo a confidencialidade e a integridade dos dados e serviços.

Em resumo, o Spring Security é um grupo de filtros de servlet, que ajudam você a adicionar autenticação e autorização ao seu aplicativo da web.


## Spring Boot Security com JWT

### Habilitando segurança com Spring

Em um projeto Spring Boot Web você precisa somente incluir a dependência no pom.xml:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-security</artifactId>
</dependency>
```
Quando você inclui esta dependência ao iniciar a aplicação, será solicitado um usuário e senha.

Para exemplificar vamos criar um app web com o Spring Initializr com os starters: Web e Spring Securit:

Ao rodar a aplicação já como as dependências inseridas receberemos uma tela de login gerada automaticamente pelo Spring Security:

![Tela de Login](./img/m8-login.png)

O Spring Security, possui um usuário padrão chamado **user** e toda vez que sua aplicação é iniciada ele gera uma senha aleatória no console da IDE:

```
Using generated security password: 6950aa31-b730-4731-bf6a-82eac7fb78be
```

### Autenticação simples

O Spring possui algumas configurações para definir os usuários na sua camada de segurança, através da geração de senha no terminal do IDE.

Para aplicações em produção esta não é uma abordagem um tanto aconselhável, e é por isso que vamos conhecer algumas outras configurações de segurança.

### No application.properties

```properties
spring.security.user.name=aiplbh
spring.security.user.password=dio123
spring.security.user.roles=USERS
```

### Geração de acesso em memória

Esse tipo de configuração permite criar mais de usuário e perfis de acesso. É necessário criar uma classe que estenda `WebSecurityConfigurerAdapter` conforme abaixo:

```java
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;


@Configuration
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class WebSecurityConfig extends WebSecurityConfigurerAdapter {
	@Override
    protected void configure(AuthenticationManagerBuilder auth) throws Exception {
		auth.inMemoryAuthentication()
                .withUser("user")
                .password("{noop}user123")
                .roles("USERS")
                .and()
                .withUser("admin")
                .password("{noop}master123")
                .roles("MANAGERS");
    }
}
```
Comentários sobre o código:

Linha 8 - Definimos que está é uma classe de configuração;

Linha 9 - Aplica a configuração de segurança padrão do Spring ao nosso aplicativo;

Linha 10 - Exige que os usuários tenham um perfil (role) apropriado usando anotações de segurança;

Linha 13 - Gerenciador de credenciais em memória;

Linha 16 e 19 - Determinam a criptografia aplicada na senha de cada usuário.

Existem algumas implementações de criptografias utilizadas pelo Spring Security:

- Use {bcrypt} for BCryptPasswordEncoder (mais comum);
- Use {noop} for NoOpPasswordEncoder;
- Use {pbkdf2} for Pbkdf2PasswordEncoder;
- Use {scrypt} for SCryptPasswordEncoder;
- Use {sha256} for StandardPasswordEncoder.





### Configuração do Adapter

### Autenticação com Banco de Dados

### JWT (Jason Web Token) Parte 1

### JWT (Jason Web Token) Parte 2

### JWT (Jason Web Token) Parte 3

### JWT (Jason Web Token) Parte 4

## Conclusão



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