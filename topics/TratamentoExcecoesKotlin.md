# Tratamento de Exceções em Kotlin
```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Desmistificando Kotlin para programadores Java
Instrutor: Venilton Falvo Jr. @falvojr - 29/08/23 a 29/10/23
```

## Apresentação

Como lidar com eventuais erros que ocorrem no dia-a-dia de todo programador é o escopo dessa unidade. Será fornecido um conjunto de informações que possibilitará o melhor entendimento e a busca de soluções para cada tipo de exception.

## Nivelamento

Exceções em linguagens de programação, muitas vezes chamadas de "exceções" ou "erros de exceção", são eventos ou condições anormais que ocorrem durante a execução de um programa. 

Elas indicam situações inesperadas ou inadequadas que podem interromper o fluxo normal de execução de um programa. Quando uma exceção ocorre, a linguagem de programação fornece mecanismos para capturar, tratar e, em alguns casos, relatar essas exceções.

Aqui estão alguns conceitos-chave relacionados a exceções:

**Causas de Exceções**: As exceções podem ser causadas por diversos motivos, como divisão por zero, acesso a um índice fora dos limites de um array, tentativa de abrir um arquivo inexistente, entre outros. Elas são geralmente erros ou condições não previstos durante a codificação.

**Captura de Exceções**: Para lidar com exceções, as linguagens de programação oferecem estruturas de controle, como blocos "try-catch" (ou "try-except" em algumas linguagens). Quando um bloco "try" é executado, o programa monitora se ocorre uma exceção. Se ocorrer, o bloco "catch" correspondente é executado para tratar a exceção.

**Tratamento de Exceções**: O tratamento de exceções envolve tomar medidas apropriadas para lidar com a exceção. Isso pode incluir registrar o erro, exibir uma mensagem de erro para o usuário, tentar uma ação alternativa ou encerrar o programa de forma controlada.

**Propagação de Exceções**: Em alguns casos, é possível propagar (lançar) exceções para um nível superior na hierarquia de chamadas de função. Isso permite que partes diferentes do código lidem com a mesma exceção de maneira adequada ao contexto.

**Exceções Personalizadas**: Além das exceções predefinidas pela linguagem, os programadores podem criar suas próprias exceções personalizadas para representar erros específicos do domínio de aplicação.

**Exceções Não Tratadas**: Se uma exceção não for tratada (ou seja, não for capturada por um bloco "try-catch" apropriado), ela geralmente levará à interrupção do programa e pode gerar mensagens de erro não controladas.

### Objetivos

- Compreender o funcionamento das Exceptions em Kotlin
- Compreender as pilhas de exceção do Kotlin
- [Link da documentação oficial sobre Exceptions](https://kotlinlang.org/docs/exceptions.html)

### Introdução

Todas as classes de exceção em Kotlin herdam a classe `Throwable`. Cada exceção possui uma mensagem, um rastreamento de pilha e uma causa opcional.

Para lançar um objeto de exceção, use a expressão `throw`:

```kotlin
fun main() {
    throw Exception("Hi There!")
}
```
**Saída do código**

```
Exception in thread "main" java.lang.Exception: Hi There!
 at FileKt.main (File.kt:2) 
 at FileKt.main (File.kt:-1) 
 at jdk.internal.reflect.NativeMethodAccessorImpl.invoke0 (:-2) 
```


### Manipulando as primeiras exceções


Para capturar uma exceção, use a expressão try...catch:

```kotlin
try {
    // some code
} catch (e: SomeException) {
    // handler
} finally {
    // optional finally block
}
```
Exemplo: 
```kotlin
fun main() {

    val a = 10
    val b = 0
    
    try {
        val divisao = a/b
    } catch (e: ArithmeticException){
        println ("Erro de divisão por zero")
    } catch(e: Throwable) {
        //e.printStackTrace()  (imprime a pilha de exception )
        println(e.message)
    } finally {
        println("Esse bloco é opcional")
    }
}
```

Pode haver zero ou mais blocos catch e o bloco final pode ser omitido (finally). No entanto, pelo menos um bloco `catch` ou `finally` é necessário.


⚠ O bloco `finally`, caso exista, __sempre__ será executado.

### Try é uma expression

try é uma expressão, o que significa que pode ter um valor de retorno:

```kotlin
val a: Int? = try { input.toInt() } catch (e: NumberFormatException) { null }
```
👍 Pelo exemplo vemos que é possível usar try..catch para uma variável.

No exemplo acima para a variável imutável `a` declarada como Int com possibilidade de receber `null` é feita a tentativa (try) de conversão para inteiro e caso se receba uma exceção do tipo `NumberFormatException` a variável receberá `null`.

O valor retornado de uma expressão `try` é a última expressão no bloco try ou a última expressão no bloco `catch` (ou blocos). O conteúdo do bloco `finally` não afeta o resultado da expressão.

Usando o exemplo anteriormente criado:

```kotlin
fun main() {

    val a = 10
    val b = 0
    
    // usando uma try expression aplicada a uma atribuição de variável
    val divisao = try {a/b} catch (e: ArithmeticException) {"Divisão por zero. Null atribuído"}
    println(divisao)  
}
```
**Saída do código**

```
Divisão por zero. Null atribuído
```

### Checked Exceptions

Em Java, "checked exceptions" (exceções verificadas) são um tipo de exceção que o compilador exige que você trate explicitamente em seu código. Essas exceções são verificadas pelo compilador durante o tempo de compilação e, se você não as tratar corretamente, seu código não será compilado com sucesso.

![Checked Exceptions in Java](.img/checkedException.PNG)

Kotlin `não` possui exceções verificadas (checked exceptions). 

Assim o desenvolvedor não está obrigado a tratar nenhuma exception embora seja desejável analisar cada caso.

Kotlin, sendo interoperável com Java, permite a utilização e o tratamento de qualquer exceção originada do Java, incluindo a IOException.

O Kotlin não diferencia entre exceções checked e unchecked, um conceito presente em Java. Isso significa que, enquanto no Java você é obrigado a tratar ou declarar uma exceção checked, em Kotlin isso não é uma exigência. 

Entretanto, ainda é possível e muitas vezes recomendado tratar essas exceções para evitar crashes inesperados e garantir que seu código seja seguro e estável.

Aqui está um exemplo de como você pode tratar uma IOException em Kotlin:

```kotlin
import java.io.IOException

fun main() {
    try {
        // O código que pode lançar uma IOException vai aqui
    } catch (e: IOException) {
        // Trate a IOException aqui println("Ocorreu uma IOException: ${e.message}")
    }
}
```

### Exceções customizadas e idiomáticas

```kotlin
fun vote (name: String, age: Int) {
    if (age < 16) {
        throw IllegalArgumentException("Apenas maiores de 16 anos podem votar")
    }
    println("Voto de $name realizado com sucesso)
}

fun main() {
    vote("Venilton", 33)
}
```
A documentação sugere que se utilize `annotations` para indicar a necessidade de tratamento de `exceptions`, como abaixo exemplificado, onde foi criada uma classe `IllegalVoterException` para tratar especificamente a `exception`.

```kotlin
class IllegalVoterException (message: String) : Throwable(message)

@Throws(IllegalVoterException::class)  // Exception customizada
fun vote (name: String, age: Int) {
    if (age < 16) {
        throw IllegalVoterException("Apenas maiores de 16 anos podem votar")
    }
    println("Voto de $name realizado com sucesso)
}

fun main() {
    vote("Venilton", 33)
    vote("Maria", 14)
    vote("Renan", 50)
}
```
Outra possibilidade seria tratar a exceptio com try..catch:

```kotlin
class IllegalVoterException (message: String) : Throwable(message)

@Throws(IllegalVoterException::class)  // Exception customizada
fun vote (name: String, age: Int) {
    if (age < 16) {
        throw IllegalVoterException("Apenas maiores de 16 anos podem votar")
    }
    println("Voto de $name realizado com sucesso")
}

fun main() {
    var quantidadeVotos = 0
    quantidadeVotos += try {vote("Venilton", 33); 1; } catch (e: IllegalVoterException) {0}
    quantidadeVotos += try {vote("Maria", 14); 1; } catch (e: IllegalVoterException) {0}
    quantidadeVotos += try {vote("Renan", 50); 1; } catch (e: IllegalVoterException) {0}
    println("Total de votos apurados: $quantidadeVotos")
}
```
**Saída do código**

```
Voto de Venilton realizado com sucesso
Voto de Renan realizado com sucesso
Total de votos apurados: 2
```

### Throw é uma Expression, Tipo Nothing e Conclusão

Asim como o `try`, o `throw` também é uma `expression`. Sendo assim você pode usá-la, por exemplo, como parte de uma expressão de Elvis:

```kotlin
val s = person.name ?: throw IllegalArgumentException("Name required")
```
A expressão `throw` tem o tipo `Nothing`. Este tipo não possui valores e é usado para marcar locais de código que nunca podem ser alcançados. No seu próprio código, você pode usar `Nothing` para marcar uma função que nunca retorna.


## Conclusão

Nesse módulo foram apresentados os principais tópicos referentes às Exceções em Kotlin.

Inicialmente foi apresentado a classe Throwable responsável no Kotlin por lançar um exceção ou `exception`. 

Para tratamento das `exceptions` o Kotlin utiliza o recurso de `try..catch..finally` com algumas particularidades como a possibilidade de usá-lo como expressões, podendo ser aplicado até mesmo a uma variável.

Uma discussão sobre a obrigatóriedade de se tratar exceções contrapondo algumas linguagens que a utilizam como o Java, foi conduzida de forma a criar uma base de referência importante para argumentação ante a não-obrigatoriedade desse recurso no Kotlin, demonstrando a importância de se atribuir aos desenvolvedores o papel de adequar o código à real necessidade de cada modelo de negócio.

Finalmente foi apresentado um exemplo prático de utilização das Exceptions e seu tratamento customizado.

Como trata-se de um tema importante e também muito complexo uma vez que exige uma vivência com o desenvolvimento de código para melhor aproveitamento do treinamento, não foi possível uma assimilação mais aprofundada do conteúdo.

Mesmo com essas exigências o Instrutor Venilton Falvo Jr @falvojr conseguiu brilhantemente elucidar e esclarecer as principais questões relativas ao tópico, principalmente na criação de um código prático de exemplo, fora da documentação oficial do Kotlin.

## Materiais de apoio e referências

[Repositório da DIO](https://github.com/digitalinnovationone/aprenda-kotlin-com-exemplos)

[Documentação oficial do Kotlin](https://kotlinlang.org/)

[Ferramenta de Inteligência artificial ChatGPT](https://chat.openai.com/)

[Ferramenta de Inteligência artificial AI Converter](https://aicodeconvert.com/)

## Certificado

![Certificado](../img/m3-certificadoTratamentoExcecoesKotlin.png)

```
Disclaimer:

Todo o material aqui apresentado foi gerado a partir de minhas anotações de aula durante o excelente
treinamento ministrado  pelo Instrutor Venilton Falvo Jr. @falvojr. Proibida a reprodução e veiculação
sem a ciência e autorização da DIO e do autor.
```