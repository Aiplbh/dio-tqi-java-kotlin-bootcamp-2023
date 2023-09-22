# O poder das funções em Kotlin

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Desmistificando Kotlin para programadores Java
Instrutor: Venilton Falvo Jr. @falvojr - 29/08/23 a 29/10/23
```

## Apresentação

Vamos aprofundar nossos estudos em conceitos mais avançados da linguagem de programação Kotlin, explorando funções de escopo e novos tipos.

### Objetivos

Fazer uma abordagem prática das seguintes funções:

##### Funções de Escopo

- let
- run
- with
- apply
- also

##### Funções avançadas

- Infix Functions
- Operator Functions
- Higher Order Functions
- Lambda Functions
- Extensions
- Suspend Functions Coroutines

## Funções de Escopo

### Let

A função da biblioteca padrão Kotlin `let` pode ser usada para definição de escopo e verificações de nulos. Quando chamado em um objeto, `let` executa o bloco de código fornecido e retorna o resultado de sua última expressão. O objeto é acessível dentro do bloco pela referência *`it`* (por padrão) ou por um nome personalizado.

```kotlin
fun customPrint(s: String) {
    print(s.uppercase())
}

fun main() {
    val empty = "test".let {               // 1
        customPrint(it)                    // 2
        it.isEmpty()                       // 3
    }
    println(" is empty: $empty")


    fun printNonNull(str: String?) {
        println("Printing \"$str\":")

        str?.let {                         // 4
            print("\t")
            customPrint(it)
            println()
        }
    }

    fun printIfBothNonNull(strOne: String?, strTwo: String?) {
        strOne?.let { firstString ->       // 5 
            strTwo?.let { secondString ->
                customPrint("$firstString : $secondString")
                println()
            }
        }
    }

    printNonNull(null)
    printNonNull("my string") 
    printIfBothNonNull("First","Second") 
}
```
**Observações e comentários do código**

- //1. Chama o bloco fornecido no resultado da string "test".

- //2. Chama a função em "test" pela referência it.

- //3. `let` retorna o valor desta expressão.

- //4. Usa chamada segura, então `let` e seu bloco de código serão executados apenas em valores não nulos.

- //5. Usa o nome personalizado em vez de `it`, para que o `let` aninhado possa acessar o objeto de contexto do `let` externo.

**Saída do código**
```
TEST is empty: false
Printing "null":
Printing "my string":
	MY STRING
FIRST : SECOND
```

### Run

Assim como let, `run` é outra função de escopo da biblioteca padrão. Basicamente, faz a mesma coisa: executa um bloco de código e retorna seu resultado. A diferença é que dentro do run o objeto é acessado por `this`. Isso é útil quando você deseja chamar os métodos do objeto em vez de passá-lo como argumento.

```kotlin
fun main() {
    fun getNullableLength(ns: String?) {
        println("for \"$ns\":")
        ns?.run {                                                  // 1
            println("\tis empty? " + isEmpty())                    // 2
            println("\tlength = $length")                           
            length                                                 // 3
        }
    }
    getNullableLength(null)
    getNullableLength("")
    getNullableLength("some string with Kotlin")
}
```
**Observações e comentários do código**

- //1. Chama o bloco fornecido em uma variável anulável.

- //2. Dentro da execução, os membros do objeto são acessados ​​sem o seu nome.

- //3. `run` retorna `length` da `String` fornecida se não for `null`.

**Saída do código**
```
for "null":
for "":
	is empty? true
	length = 0
for "some string with Kotlin":
	is empty? false
	length = 23
```
### With

`with` é uma função sem extensão que pode acessar membros de seu argumento de forma concisa: você pode omitir o nome da instância ao se referir a seus membros.

```kotlin
class Configuration(var host: String, var port: Int) 

fun main() {
    val configuration = Configuration(host = "127.0.0.1", port = 9000) 

    with(configuration) {
        println("$host:$port")
    }

    // instead of:
    println("${configuration.host}:${configuration.port}")    
}
```

**Saída do código**
```
127.0.0.1:9000
127.0.0.1:9000
```

### Apply

`apply` executa um bloco de código em um objeto e retorna o próprio objeto. Dentro do bloco, o objeto é referenciado por `this`. Esta função é útil para inicializar objetos.

```kotlin
data class Person(var name: String, var age: Int, var about: String) {
    constructor() : this("", 0, "")
}

fun main() {
    val jake = Person()                                     // 1
    val stringDescription = jake.apply {                    // 2
        name = "Jake"                                       // 3
        age = 30
        about = "Android developer"
    }.toString()                                            // 4
    println(stringDescription)
}
```
**Observações e comentários do código**

- //1. Cria uma instância Person() com valores de propriedade padrão.

- //2. Aplica o bloco de código (próximas 3 linhas) à instância.

- //3. Dentro de apply, é equivalente a jake.name = "Jake".

- //4. O valor de retorno é a própria instância, portanto você pode encadear outras operações.

**Saída do código**
```
Person(name=Jake, age=30, about=Android developer)
```

### Also

`Also` funciona como `apply`: executa um determinado bloco e retorna o objeto chamado. Dentro do bloco, o objeto é referenciado por `it`, então é mais fácil passá-lo como argumento. Esta função é útil para incorporar ações adicionais, como registro em cadeias de chamadas.

```kotlin
data class Person(var name: String, var age: Int, var about: String) {
             constructor() : this("", 0, "")
}
         
fun writeCreationLog(p: Person) {
    println("A new person ${p.name} was created.")              
}
         
fun main() {
    val jake = Person("Jake", 30, "Android developer")   // 1
        .also {                                          // 2 
            writeCreationLog(it)                         // 3
        }
}
```
**Observações e comentários do código**

- //1. Cria um objeto Person() com os valores de propriedade fornecidos.

- //2. Aplica o bloco de código fornecido ao objeto. O valor de retorno é o próprio objeto.

- //3. Chama a função de registro passando o objeto como argumento.

**Saída do código**
```
A new person Jake was created.
```

## Tipos de funções


### Infix functions

As infix functions (funções infixas) em Kotlin são funções especiais que podem ser chamadas usando uma sintaxe de operador infix (infix notation). Isso significa que você pode chamá-las sem usar a notação de ponto ou parênteses e, em vez disso, usará um operador entre o objeto e o argumento da função. As infix functions são usadas principalmente para melhorar a legibilidade do código em certos cenários, tornando-o mais próximo da linguagem natural.

```kotlin
fun main() {

  infix fun Int.times(str: String) = str.repeat(this)        // 1
  println(2 times "Bye ")                                    // 2

  val pair = "Ferrari" to "Katrina"                          // 3
  println(pair)

  infix fun String.onto(other: String) = Pair(this, other)   // 4
  val myPair = "McLaren" onto "Lucas"
  println(myPair)

  val sophia = Person("Sophia")
  val claudia = Person("Claudia")
  sophia likes claudia                                       // 5
}

class Person(val name: String) {
  val likedPeople = mutableListOf<Person>()
  infix fun likes(other: Person) { likedPeople.add(other) }  // 6
}
```

### Operator functions
Em Kotlin, as "operator functions" são funções que permitem que os operadores matemáticos e outros operadores sejam usados com tipos de dados personalizados de maneira intuitiva e conveniente. Essas funções são definidas usando o modificador "operator" e são associadas a símbolos de operador específicos, como "+", "-", "*", "/", "==", ">", "<" e assim por diante.

As operator functions podem ser usadas não apenas com operadores matemáticos, mas também com outros operadores, como igualdade (==), comparação (>, <, >=, <=) e muito mais. Elas tornam o código mais conciso e intuitivo quando se trabalha com tipos de dados personalizados em Kotlin.

```kotlin
fun main() {
    operator fun Int.times(str: String) = str.repeat(this)       // 1
    println(2 * "Bye ")                                          // 2

    operator fun String.get(range: IntRange) = substring(range)  // 3
    val str = "Always forgive your enemies; nothing annoys them so much."
    println(str[0..14])                                          // 4
}
````

**Saída do código**

```
Bye Bye 
Always forgive 
```

### Elvis operator

O operador Elvis em Kotlin é uma forma concisa de lidar com valores nulos (null) em expressões condicionais. Ele permite que você forneça um valor padrão caso uma expressão seja nula. O operador Elvis é representado por `?:`.

Aqui está a sintaxe básica do operador Elvis:

```kotlin
val resultado = valorPossivelmenteNulo ?: valorPadrao
```
Neste caso:

`valorPossivelmenteNulo` é a expressão que você quer avaliar quanto à possibilidade de ser nula. `valorPadrao` é o valor que será usado se valorPossivelmenteNulo for nulo.

Aqui está um exemplo prático:

```kotlin
val nome: String? = null
val nomeNaoNulo = nome ?: "Nome Padrão"

println(nomeNaoNulo) // Isso imprimirá "Nome Padrão" porque 'nome' é nulo.
```
**Cometário do código**

Neste exemplo, se nome for nulo, o valor padrão "`Nome Padrão`" será atribuído à variável `nomeNaoNulo`. Se nome não for nulo, o valor de nome será atribuído a `nomeNaoNulo`.

O operador Elvis é uma maneira elegante e eficiente de lidar com valores nulos em Kotlin, tornando o código mais limpo e legível. 

Ele é frequentemente usado em expressões condicionais e em casos em que você deseja fornecer um valor padrão quando uma variável é nula.

### High order function parameters

Em Kotlin, Higher-Order Functions (Funções de Ordem Superior) são funções que podem receber outras funções como parâmetros ou retorná-las como resultados. Isso significa que você pode tratar funções como objetos de primeira classe, permitindo uma programação mais flexível e poderosa. 

1. **Passagem de funções como parâmetros**: Você pode passar funções como argumentos para outras funções. Isso permite que você crie funções mais genéricas e reutilizáveis que podem se adaptar a diferentes comportamentos, dependendo das funções que são passadas como argumentos.

2. **Retorno de funções**: As Higher-Order Functions também podem retornar funções como resultados. Isso é útil quando você deseja criar fábricas de funções ou construir funções com base em certas configurações ou condições.

3. **Mapeamento e transformação de coleções**: Você pode usar funções de ordem superior como map, filter, reduce e fold para manipular coleções de maneira concisa e expressiva. Isso torna a operação em listas, conjuntos e mapas mais eficiente e legível.

4. **Tratamento de eventos e callbacks**: Em programas assíncronos ou em frameworks de programação reativa, as Higher-Order Functions são usadas para lidar com callbacks e eventos, tornando o código mais conciso e fácil de entender.

5. **Composição de funções**: Você pode compor funções para criar novas funções complexas a partir de funções simples. Isso é útil para criar pipelines de processamento de dados ou para dividir problemas em partes menores e mais gerenciáveis.

6. **Injeção de dependência**: Em Kotlin, as Higher-Order Functions são frequentemente usadas em conjunto com a injeção de dependência para configurar componentes e serviços em aplicativos de forma flexível e dinâmica.

```kotlin
fun calculate(x: Int, y: Int, operation: (Int, Int) -> Int): Int {  // 1
    return operation(x, y)                                          // 2
}

fun sum(x: Int, y: Int) = x + y                                     // 3

fun main() {
    val sumResult = calculate(4, 5, ::sum)                          // 4
    val mulResult = calculate(4, 5) { a, b -> a * b }               // 5
    println("sumResult $sumResult, mulResult $mulResult")
}
```

**Observações e comentários do código**

- //1. Declara uma função de ordem superior. São necessários dois parâmetros inteiros, x e y. Além disso, leva outra operação de função como parâmetro. Os parâmetros de operação e o tipo de retorno também são definidos na declaração.

- //2. A função de ordem superior retorna o resultado da invocação da operação com os argumentos fornecidos.

- //3.  Declara uma função que corresponde à assinatura da operação.

- //4. Invoca a função de ordem superior passando dois valores inteiros e o argumento da função `::sum`. `::` é a notação que faz referência a uma função pelo nome, sem executá-la.

- //5. Invoca a função de ordem superior passando uma função lambda como argumento.

**Saída do código**
```
sumResult 9, mulResult 20
```
### High order function returning

```kotlin
fun operation(): (Int) -> Int {                                     // 1
    return ::square
}

fun square(x: Int) = x * x                                          // 2

fun main() {
    val func = operation()                                          // 3
    println(func(2))                                                // 4
}
```
**Observações e comentários do código**

- //1. Declara uma função de ordem superior que retorna uma função. `(Int) -> Int` representa os parâmetros e o tipo de retorno é a referência da função quadrada. (os dosi pontos :: representam referência de onde a função está armazenada)

- //2. Declara uma função correspondente à assinatura.

- //3. Invoca `operation` para obter o resultado atribuído a uma variável. Aqui func torna-se quadrado que é retornado pela operação.

- //4. Invoca a função. A função quadrada é realmente executada.


### Lambda functions

Em Kotlin, funções lambda são funções anônimas, ou seja, funções que não possuem um nome próprio. Elas são frequentemente utilizadas para criar blocos de código que podem ser passados como argumentos para outras funções. As funções lambda são uma parte importante da programação funcional em Kotlin e permitem escrever código mais conciso e expressivo.

```kotlin
val soma: (Int, Int) -> Int = { a, b -> a + b }
```
Neste exemplo, soma é uma variável que armazena uma função lambda que recebe dois parâmetros a e b e retorna a soma deles. Você pode usar essa função lambda da seguinte forma:

```kotlin
val resultado = soma(10, 5) // resultado agora é igual a 15
```

### Extension functions e Extendion Properties

Kotlin permite adicionar novos membros a qualquer classe com o mecanismo de extensões. Ou seja, existem dois tipos de extensões: funções de extensão e propriedades de extensão. Eles se parecem muito com funções e propriedades normais, mas com uma diferença importante: você precisa especificar o tipo que deseja estender.

### Extension functions

Uma função de extensão é uma função que pode ser chamada em uma classe existente como se fosse um método da própria classe. 

Para criar uma função de extensão, você precisa definir uma função em um arquivo Kotlin separado (geralmente chamado de "extensions.kt") e usar a classe que você deseja estender como o tipo de receptor, precedido da palavra-chave fun. 

```kotlin
// Definindo uma função de extensão para a classe String
fun String.contarPalavras(): Int {
    return this.split(" ").size
}

fun main() {
    val texto = "Isso é um exemplo de função de extensão em Kotlin"
    val numeroPalavras = texto.contarPalavras()
    println("Número de palavras: $numeroPalavras")
}
```
**Comentário do código**

Neste exemplo, a função `contarPalavras` é uma função de extensão para a classe `String`, e podemos chamá-la como se fosse um método da classe String.


### Extention Properties

As `Extention Properties` funcionam de maneira semelhante às `Extension functions`, mas em vez de adicionar métodos, elas permitem adicionar propriedades a uma classe existente. 

Para criar uma propriedade de extensão, você precisa definir uma função "getter" e, opcionalmente, uma função "setter" para a propriedade.

```kotlin
// Definindo uma propriedade de extensão para a classe Int
val Int.metade: Int
    get() = this / 2

fun main() {
    val numero = 10
    val metadeDoNumero = numero.metade
    println("Metade do número: $metadeDoNumero")
}
```
**Comentário do código**

Neste exemplo, a propriedade de extensão `metade` é adicionada à classe `Int`, e podemos acessá-la como se fosse uma propriedade normal da classe.


### Extension functions generics



### Suspend functions

Uma co-rotina é uma instância de uma computação suspensível. É conceitualmente semelhante a um thread, no sentido de que é necessário executar um bloco de código que funciona simultaneamente com o restante do código. No entanto, uma co-rotina não está vinculada a nenhum thread específico. Pode suspender sua execução em um thread e retomar em outro.

As co-rotinas podem ser consideradas threads leves, mas há uma série de diferenças importantes que tornam seu uso na vida real muito diferente dos threads.

```kotlin
import kotlinx.coroutines.*

fun main() = runBlocking { // this: CoroutineScope
    launch { // launch a new coroutine and continue
        delay(1000L) // non-blocking delay for 1 second (default time unit is ms)
        println("World!") // print after delay
    }
    println("Hello") // main coroutine continues while a previous one is delayed
}
```
**Comentário do código**

`launch` é um construtor de co-rotinas. Ele lança uma nova co-rotina simultaneamente com o restante do código, que continua funcionando de forma independente. É por isso que `Hello` foi impresso primeiro.

`delay` é uma função de suspensão especial. Ele suspende a co-rotina por um tempo específico. A suspensão de uma co-rotina não bloqueia o thread subjacente, mas permite que outras co-rotinas sejam executadas e usem o thread subjacente para seu código.

`runBlocking` também é um construtor de co-rotinas que conecta o mundo não co-rotina de uma função `main()` regular e o código com co-rotinas dentro de `runBlocking` { ... } chaves. Isso é destacado em um IDE como: "CoroutineScope after the runBlocking opening curly brace".

Se você remover ou esquecer `runBlocking` neste código, receberá um erro na chamada de `launch`, já que o `launch` é declarado apenas no CoroutineScope.

**Saída do código**

```
Hello
World!
```

## Conclusão

Nesse treinamento foram apresentadas diversas funções (functions) da linguagem Kotlin, suas características, versatilidades e aplicações.

As `funções de escopo`: let, run, apply, with e also são voltadas para clareza e concisão do código tornando-o mais idiomático. Não há muito ganho de performance ou produtividade na que tange a geração de linhas de código.

Essa observação se aplica às funções `infix, operator functions e funções lambda` no que diz respeito a melhoria da intelegibilidade do código. 

As `funções de ordem superior` possibilitam a passagem e o recebimento de outras funções como parâmetro o que as torna essenciais para tornar a linguagem mais flexível e adaptativa.

As `extension functions e extension properties` dão grande poder à linguagem possibilitando extender funcionalidades de métodos e propriedades das classes, inclusive nativas.

A última função apresentada foram as `suspend functions` que trabalham de forma análoga às threads, porém com muitas particularidades. Em kotlin são conhecidas como `coroutines` e podem suspender sua execução em uma thread e retomar em outra.

Ao final de um conteúdo tão extenso, ficaram muitas dúvidas. Achei que as explicações dadas sobre os códigos mostrados na documentação do Kotlin estão além do conhecimento adquirido e internalizado da linguagem até esse momento do treinamento.

A sugestão seria trabalhar com exemplos mais simples antes de explorar o conteúdo oficial do Kotlin.

Ainda assim, com as explicações do Instrutor Venilton foi possível compreender (embora ainda sem a sikll para aplicar) os principais tópicos sobre funções de escopo, extensions functions e properties, extension function generics e suspend function.

## Materiais de apoio e referências

[Repositório da DIO](https://github.com/digitalinnovationone/aprenda-kotlin-com-exemplos)

[Documentação oficial do Kotlin](https://kotlinlang.org/)

[Ferramenta de Inteligência artificial ChatGPT](https://chat.openai.com/)

[Ferramenta de Inteligência artificial AI Converter](https://aicodeconvert.com/)

## Certificado

![Certificado](../img/m3-certificadoPoderFuncoesKotlin.png)

```
Disclaimer:

Todo o material aqui apresentado foi gerado a partir de minhas anotações de aula durante o excelente
treinamento ministrado  pelo Instrutor Venilton Falvo Jr. @falvojr. Proibida a reprodução e veiculação
sem a ciência e autorização da DIO e do autor.
```
