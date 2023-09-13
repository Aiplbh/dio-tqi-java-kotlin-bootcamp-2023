# Introdução prática à linguagem de programação Kotlin

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Desmistificando Kotlin para programadores Java
Instrutor: Venilton Falvo Jr. @falvojr - 29/08/23 a 29/10/23
```

## Apresentação

### Objetivos

- Objetivo: Dominar a sintaxe do Kotlin através de exemplos (hands on). 

- Usar a documentação e o site oficial do Kotlin para [aprender por exemplos](https://play.kotlinlang.org/byExample/overview)

### Ementa
- O Básico Sobre Funções
- Variáveis
- Null Safety
- Classes
- Generics

### Elementos práticos da documentação oficial do Kotlin

- [Link Oficial do Kotlin](https://kotlinlang.org/)
- [Documentação oficial](https://kotlinlang.org/docs/home.html)
- [Playground Kotlin](https://play.kotlinlang.org/)
- [Kotlin HandsOn](https://kotlinlang.org/docs/kotlin-hands-on.html)
- [Kotlin by examples](https://play.kotlinlang.org/byExample/overview)
- [Kotlin Koans - Desafios](https://play.kotlinlang.org/koans/overview)

## Hands ON


### Olá, Mundo!

```kotlin 
package org.kotlinlang.play         // 1

fun main() {                        // 2
    println("Hello, World!")        // 3
}
```

**Observações e comentários do código**

- Kotlin é, usualmente, definido em pacotes. 
- A nomeação dos pacotes é livre, mas geralmente segue o padrão [nome da organização invertido].[nome do pacote] Ex:  ``` com.aiple.helloWorld ```
- Uso de ponto e vírgula no final de cada linha (statment) é opcional.
- A função `main()` pode ser declarada sem argumentos e é o ponto de entrada de uma aplicação Kotlin.

---
### Funções | Valores de parâmetros padrão e argumentos nomeados

**Default Parameter Values and Named Arguments**

```kotlin
fun printMessage(message: String): Unit {                               // 1
    println(message)
}

fun printMessageWithPrefix(message: String, prefix: String = "Info") {  // 2
    println("[$prefix] $message")
}

fun sum(x: Int, y: Int): Int {                                          // 3
    return x + y
}

fun multiply(x: Int, y: Int) = x * y                                    // 4

fun main() {
    printMessage("Hello")                                               // 5
    printMessageWithPrefix("Hello", "Log")                              // 6
    printMessageWithPrefix("Hello")                                     // 7
    printMessageWithPrefix(prefix = "Log", message = "Hello")           // 8
    println(sum(1, 2))                                                  // 9
    println(multiply(2, 4))                                             // 10
}
```

- //1. No Kotlin pode-se definir o tipo de retorno de uma função. A palavra `Unit` indica que essa função não retorna nada (void). Ela pode ser omitida.

- //2. Nesse exemplo são passados dois parâmetros (message e prefix), sendo que o segundo já é inicializado com um valor `default` em tempo de execução. Outra particularidade desse exemplo é que a interpolação de strings. Veja que println imprime uma string "entre aspas" interpolada pelas variáveis `message` e `prefix` através do caracter $variável.

- //3. Esse exemplo mostra uma função onde foi definido o tipo de retorno (`Int`).

- //4. Função em linha. Nesse caso a palavra reservada `return` pode ser omitida.

- //5. Chama a primeira função passando como argumento a palavra ´Hello`.

- //6. Chama a segunda função passando os dois parâmetros: message e prefix.

- //7. Chama a segunda função omitindo o segundo parâmetro que nesse caso assume o valor `Info`

- //8. Chama a segunda função usando argumentos nomeados. Nesse caso a ordem pode ser invertida na chamada da função, mas o resultado do print será o mesmo da definição.

- //9. Imprime a soma de 1 e 2

- //10. Imprime a multiplicação de 2 e 4

Saídas obtidas no Kotlin Playground:

```
Hello
[Log] Hello
[Info] Hello
[Log] Hello
3
8
```

**Functions with vararg Parameters**

*Varargs* permite que se passe qualquer numero de argumentos, sparando-os por virgula.

```kotlin
fun main() {
    fun printAll(vararg messages: String) {                            // 1
        for (m in messages) println(m)       // trata messages como array
    }
    printAll("Hello", "Hallo", "Salut", "Hola", "你好")                 // 2

    fun printAllWithPrefix(vararg messages: String, prefix: String) {  // 3
        for (m in messages) println(prefix + m)
    }
    printAllWithPrefix(
        "Hello", "Hallo", "Salut", "Hola", "你好",
        prefix = "Greeting: "                                          // 4
    )

    fun log(vararg entries: String) {
        printAll(*entries)                                             // 5
    }
    log("Hello", "Hallo", "Salut", "Hola", "你好")
}
```

Nesse exemplo todas as funções testadas estão dentro do escopo do main(). No entanto elas poderiam ter sido declaradas fora (globais).

**Observações e comentários do código**

- //1. O modificador vararg transforma um parâmetro em *vararg*.

- //2. Isso permite chamar *printAll* com qualquer número de argumentos de string.

- //3. Graças aos parâmetros nomeados, você pode até adicionar outro parâmetro do mesmo tipo após o vararg. Isso não seria permitido em Java porque não há como passar um valor.

- //4. Usando parâmetros nomeados, você pode definir um valor para prefixar separadamente do *vararg*.

- //5. Em tempo de execução, um *vararg* é apenas um array. Para passá-lo como um parâmetro *vararg*, use o operador spread especial `'*'` que permite passar `*entries` (um vararg de String) em vez de somente `entries` sem `'*'` (um Array<String>).


### Variáveis no Kotlin

Agora vamos ver como o Kotlin lida com as variáveis. Veremos que o Kotlin explora muito a `inferência de tipos`. 

A inferência de tipos é um conceito na programação que se refere à capacidade do compilador ou da linguagem de programação em determinar automaticamente o tipo de uma variável ou expressão com base no contexto em que ela é usada. 

Em outras palavras, é a capacidade do sistema de tipos da linguagem de deduzir o tipo de uma variável sem a necessidade de especificá-lo explicitamente.

 Aqui estão alguns exemplos de como a inferência de tipos funciona no Kotlin:

Na declaração de variáveis
```kotlin
val numero = 42 // O compilador infere que 'numero' é do tipo Int automaticamente.
val nome = "Alice" // O compilador infere que 'nome' é do tipo String automaticamente.
```

Em coleções:
```kotlin
val numeros = listOf(1, 2, 3, 4, 5) // O compilador infere que 'numeros' é uma lista de Int automaticamente.
```

⚠️ No entanto, vale ressaltar que o Kotlin ainda é uma linguagem de tipagem estática, o que significa que os tipos são verificados em tempo de compilação, mesmo que a inferência de tipos seja usada.

### Variáveis val e var

As variáveis declaradas como `var` são **mutáveis** e as variáveis declaradas como `val` são **imutáveis**:

```
val = imutável
var = mutável
```
Exemplos:

```kotlin
var a: String = "initial"  // 1
println(a)
val b: Int = 1             // 2
val c = 3                  // 3
```
**Observações e comentários do código**

- //1. Declara a variável ' a ' como mutável e a inicializa.

- //2. Declara a variável ' b ' como imutável e a inicializa.

- //3. Declara a variável ' c ' como imutável e a inicializa, sem especificar o tipo, que será inferido pelo compilador.

⚠️ Se você tentar atribuir um valor a uma variável declarada como val, o compilador emitirá um erro.

Além disso, se você tentar usar uma variável do tipo `val` sem que a mesma esteja **inicializada** ocorrerá um erro:

```kotlin
fun main() {
    var e: Int  // 1
    println(e)  // 2
}
```
Error: Variable 'e' must be initialized

### Null safety nulidade

Para definir uma variável como `nullable`, ou seja, que aceita receber `null` deve-se acrescentar um sinal de interrogação no final do tipo da variável. Exemplo para uma variável chamada `nullable`

```kotlin
var nullable: String? = "You can keep a null here"      // 3

nullable = null 
```

Essa característica da linguagem força o desenvolvedor a se procupar com exceções do tipo `NullPointerException` evitando possíveis problemas ainda em tempo de produção.

### Classes
As classes em Kotlin são declaradas usando a palavra-chave *class*.

A declaração da classe consiste no:

- nome da classe, 
- no cabeçalho da classe (especificando seus parâmetros de tipo, o construtor primário etc.),
- corpo da classe, entre chaves. 

Tanto o cabeçalho quanto o corpo são opcionais; se a classe não tiver corpo, chaves podem ser omitidas.

Exemplos:

```kotlin
class Customer                                  // 1

class Contact(val id: Int, var email: String)   // 2

fun main() {

    val customer = Customer()                   // 3
    
    val contact = Contact(1, "mary@gmail.com")  // 4

    println(contact.id)                         // 5
    contact.email = "jane@gmail.com"            // 6
}
```
**Observações e comentários do código**
- //1. Declara uma classe chamada *Customer* sem nenhuma propriedade. Nesse caso um construtor não parametrizado é criado automaticamente pelo Kotlin.

- //2. Declara uma classe com duas propriedades: `id` imutável e `email` mutável, e um construtor com dois parâmetros *id* e *email*.

- //3.  Cria uma instância da classe Customer por meio do construtor padrão. Observe que não há necessidade da palavra-chave `new` no Kotlin.

- //4. Cria uma instância da classe Contact usando o construtor com dois argumentos.

- //5. Acessa a propriedade *id*.

- //6. Atualiza o valor da propriedade *email* (que é do tipo val - mutável)

### Generics | Classes genéricas

Classes e funções genéricas aumentam a capacidade de reutilização do código encapsulando a lógica comum que é *independente* de um tipo genérico específico, como a lógica dentro de um List<T> é independente do que tipo da variável `T`.

Para declarar uma lista genérica usa-se um parâmetro de tipo genérico (`T`) entre os sinais de < e >.

Nesse exemplo será implementado uma classe que desempenha o papel de uma estrutura de dados conhecida como `pilha` com os principais métodos assiciados.

O interessante desse código é que ele infere o tipo de dados da pilha e pode ser usado para implementar pilhas de números ou strings.

Assim a estrutra `pilha` foi implementada sendo que suas operações básicas (push(), pop(), peek(), isEmpty() e size()) foram implementadas através dos métodos definidos para o tipo *Lista* (.add(e), .last(), .removeAt(position), isEmpty())

Exemplo:

```kotlin
class MutableStack<E>(vararg items: E) {              // 1

  private val elements = items.toMutableList()

  fun push(element: E) = elements.add(element)        // 2

  fun peek(): E = elements.last()                     // 3

  fun pop(): E = elements.removeAt(elements.size - 1)

  fun isEmpty() = elements.isEmpty()

  fun size() = elements.size

  override fun toString() = "MutableStack(${elements.joinToString()})"
}

fun <E> mutableStackOf(vararg elements: E) = MutableStack(*elements)

fun main() {
  val stack = mutableStackOf(0.62, 3.14, 2.7)
  stack.push(9.87)
   println("${stack}\n")
  
  println ("Topo da pilha: peek(): ${stack.peek()}\n")
  
  for (i in 1..stack.size()){
      println("Remove ultimo elemento com pop(): ${stack.pop()}")
      println("${stack}\n")
  }
}
```
**Observações e comentários do código**

- //1. Define uma classe genérica MutableStack<E> onde E é chamado de parâmetro de tipo genérico. 

- //2. Dentro da classe genérica, E pode ser usado como parâmetro como qualquer outro tipo. No caso a variável `elemento` é definida como sendo do tipo `E`.

- //3. Você também pode usar *E* como tipo de retorno.

Observe que, no exemplo acima foi utilizado intensamente a  sintaxe abreviada do Kotlin para funções que podem ser definidas em uma única expressão. (in line functions).

**Saída do código acima**

```
MutableStack(0.62, 3.14, 2.7, 9.87)

Topo da pilha: peek(): 9.87

Remove ultimo elemento com pop(): 9.87
MutableStack(0.62, 3.14, 2.7)

Remove ultimo elemento com pop(): 2.7
MutableStack(0.62, 3.14)

Remove ultimo elemento com pop(): 3.14
MutableStack(0.62)

Remove ultimo elemento com pop(): 0.62
MutableStack()
```

### Generics | Funções genéricas

Além de classes genéricas, também é possível trabalhar com *funções genéricas*. Você pode gerar funções independente de um tipo específico. 

Por exemplo, você pode escrever uma função utilitária para criar pilhas mutáveis:

```kotlin
class MutableStack<E>(vararg items: E) {              // 1

  private val elements = items.toMutableList()

  fun push(element: E) = elements.add(element)        // 2

  fun peek(): E = elements.last()                     // 3

  fun pop(): E = elements.removeAt(elements.size - 1)

  fun isEmpty() = elements.isEmpty()

  fun size() = elements.size

  override fun toString() = "MutableStack(${elements.joinToString()})"
}

fun <E> mutableStackOf(vararg elements: E) = MutableStack(*elements)

fun main() {
  val stack = mutableStackOf(0.62, 3.14, 2.7)
  println(stack)
}
```
Observe que o compilador pode inferir o tipo genérico dos parâmetros de *mutableStackOf* para que você não precise escrever *mutableStackOf<'Double'>*(...).

## Materiais de apoio e Questionário

[Repositório da DIO]( 
https://github.com/digitalinnovationone/aprenda-kotlin-com-exemplos)

[Documentação oficial do Kotlin](https://kotlinlang.org/)

## Certificado

![Certificado](../img/m3-CertificadoIntroKotlin.PNG)

```
Disclaimer:

Todos material aqui apresentado foi gerado a partir de minhas anotações pessoais de aula durante o excelente treinamento ministrado
pelo Instrutor Venilton Falvo Jr. @falvojr.
Proibida a reprodução e veiculação sem a ciência e autorização da DIO.
```
