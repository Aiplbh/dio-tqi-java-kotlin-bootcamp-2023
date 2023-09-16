# Estruturas de Controle de Fluxo e Coleções em Kotlin

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Desmistificando Kotlin para programadores Java
Instrutor: Venilton Falvo Jr. @falvojr - 29/08/23 a 29/10/23
```
## Apresentação

Nesse treinamento usaremos como referência dois links principais:

- [Kotlin by examples](https://play.kotlinlang.org/byExample/overview)

- [Repositório do Bootcamp](https://github.com/Aiplbh/dio-tqi-java-kotlin-bootcamp-2023/blob/main/topics/IntroducaoPraticaLinguagemKotlin.md)

### Objetivos

Explorar as principais estruturas condicionais e de repetição controle de fluxo além de conhecer algumas das principais coleções do Kotlin.

## Controle de fluxo

Estruturas de controle de fluxo permitem que o programador controle a ordem em que as instruções são executadas em um programa, permitindo se execute diferentes ações com base em diferentes condições.

Em vez da instrução *switch* amplamente usada em outras linguagens, o Kotlin fornece uma forma mais flexível e clara: a estrutura `when` . Ela pode ser usada como uma declaração (*statment*) ou como uma expressão (*expression*).

### When statement

```kotlin
fun main() {
    cases("Hello")
    cases(1)
    cases(0L)
    cases(MyClass())
    cases("hello")
}

fun cases(obj: Any) {                                
    when (obj) {                                     // 1   
        1 -> println("One")                          // 2
        "Hello" -> println("Greeting")               // 3
        is Long -> println("Long")                   // 4
        !is String -> println("Not a string")        // 5
        else -> println("Unknown")                   // 6
    }   
}

class MyClass
```
**Observações e comentários do código**

- //1. Esta é uma declaração `when`.

- //2. Verifica se obj é igual a 1.

- //3. Verifica se obj é igual a "Hello". 

- //4. Executa verificação de tipo (É tipo `Long` ?)

- //5. Executa verificação inversa de tipo. (Não é tipo `String`?)

- //6. Instrução padrão (senão ou otherwise - pode ser omitida).

**Saída do código anterior**
```
Greeting
One
Long
Not a string
Unknown (refere-se ao tipo Class que não foi encontrada)
```

⚠️ Observe que todas as *branches* são verificadas sequencialmente até que uma delas seja satisfeita. Assim, apenas a primeira *branch* adequada será executada.

### When Expression

Outra forma de se utilizar a cláusula `when` é através de um expressão em uma função.

Nesse caso as branches não tem que ser veriicadas sequencialmente.

```kotlin
fun main() {
    println(whenAssign("Hello"))
    println(whenAssign(3.4))
    println(whenAssign(1))
    println(whenAssign(MyClass()))
}

fun whenAssign(obj: Any): Any {
    val result = when (obj) {                   // 1
        1 -> "one"                              // 2
        "Hello" -> 1                            // 3
        is Long -> false                        // 4
        else -> 42                              // 5
    }
    return result
}

class MyClass
```
**Observações e comentários do código**

- //1. Esta é uma expressão `when`.

- //2. Define o valor como "one" se obj for igual a 1.

- //3. Define o valor como 1 se obj for igual a "Hello".

- //4. Define o valor como *false* se obj for uma instância de *Long*.

- //5. Define o valor 42 se nenhuma das condições anteriores for satisfeita.

**Saída do código anterior**
```
1
42
one
42
```

⚠️ Ao contrário do `when declarativo` , a `branch` padrão geralmente é necessária na `expressão when`, exceto no caso em que o compilador pode verificar se outras `branches` cobrem todos os casos possíveis.

## Estruturas de repetição (Loops)

Kotlin oferece suporte a todos os loops comumente usados: `for`, `while` e `do-while`.

### Loops | for

O `for` em Kotlin funciona da mesma maneira que na maioria das linguagens de programação.

```kotlin
fun main(args: Array<String>) {                         //(*)
    val cakes = listOf("carrot", "cheese", "chocolate")

    for (cake in cakes) {                               // 1
        println("Yummy, it's a $cake cake!")
    }
}
```
**Observações e comentários do código**

- //(*) A função main() não necessita do parâmetro `args: Array<String>` a partir da versão 1.3 do Kotlin e portanto peoderia ser omitida.
- //1. Percorre cada bolo (*'cake'*) da lista..

**Saída do código anterior**
```
Yummy, it's a carrot cake!
Yummy, it's a cheese cake!
Yummy, it's a chocolate cake!
```
### Loops | while e do-while

A principal diferença entre o `while` e o `do-while` é que no *while* a condição é testada no **início** de cada repetição.

No *do-while* a condição é testada no **final** de cada repetição.

```kotlin
fun eatACake() = println("Eat a Cake")
fun bakeACake() = println("Bake a Cake")

fun main(args: Array<String>) {
    var cakesEaten = 0
    var cakesBaked = 0
    
    while (cakesEaten < 5) {                    // 1
        eatACake()
        cakesEaten ++
    }
    
    do {                                        // 2
        bakeACake()
        cakesBaked++
    } while (cakesBaked < cakesEaten)
}
```
**Observações e comentários do código**

- //1. Executa o bloco enquanto a condição for verdadeira.

- //2. Executa o bloco primeiro e depois verifica a condição.

**Saída do código**

```
Eat a Cake
Eat a Cake
Eat a Cake
Eat a Cake
Eat a Cake
Bake a Cake
Bake a Cake
Bake a Cake
Bake a Cake
Bake a Cake
```

### Loops | iterator

No Kotlin você pode definir seus próprios iteradores em suas classes implementando o operador `iterador` nelas. Vamos ao código:

```kotlin
class Animal(val name: String)

class Zoo(val animals: List<Animal>) {

    operator fun iterator(): Iterator<Animal> {             // 1
        return animals.iterator()                           // 2
    }
}

fun main() {

    val zoo = Zoo(listOf(Animal("zebra"), Animal("lion")))

    for (animal in zoo) {                                   // 3
        println("Watch out, it's a ${animal.name}")
    }
}
```
**Observações e comentários do código**

- //1. Define um *iterador* em uma classe. Deve ser nomeado *`iterator()`* e ter o modificador *`operator`*.

- //2. Retorna o iterador que atende aos seguintes requisitos do método:
    - next(): Animal
    - hasNext(): Boolean

- //3. Percorre a lista `animals` na variável `zoo` com o iterador definido pelo usuário.

### Ranges | loops com int

As `ranges` são interessantes pois tendem a deixar o código bem idiomático, dando clareza e facilitando a intelegibilidade do código.

Existe um conjunto de ferramentas para definir intervalos em Kotlin. Vamos dar uma breve olhada neles.

```kotlin
fun main() {
    for(i in 0..3) {             // 1
        print(i)
    }
    print(" ")

    for(i in 0 until 3) {        // 2
        print(i)
    }
    print(" ")

    for(i in 2..8 step 2) {      // 3
        print(i)
    }
    print(" ")

    for (i in 3 downTo 0) {      // 4
        print(i)
    }
    print(" ")
}
```
**Observações e comentários do código**

- //1. Itera em um intervalo que começa de 0 a 3 (inclusive). Como ```for(i=0; i<=3; ++i)``` em outras linguagens de programação (C/C++/Java).

- //2. Itera em um intervalo de 0 a 3 (exclusivo). Como for loop em Python ou como ```for(i=0; i<3; ++i)``` em outras linguagens de programação (C/C++/Java).

- //3. Itera em um intervalo com uma etapa de incremento personalizada para elementos consecutivos.

- //4. Itera em um intervalo na ordem inversa.

**Saída do código**
```
0123 012 2468 3210
```

### Ranges | loops com char

Os `ranges` suportam não apenas números, mas também `strings`.

```kotlin
fun main() {
    for (c in 'a'..'d') {        // 1
        print(c)
    }
    print(" ")

    for (c in 'z' downTo 's' step 2) { // 2
        print(c)
    }
    print(" ")
}
```
**Observações e comentários do código**

- //1. Itera em um intervalo de caracteres em ordem alfabética.

- //2. Os intervalos de caracteres também suportam *step* e *downTo*.

**Saída do código**
```
0123 012 2468 3210
```

### Ranges | if e loops com char

Os intervalos (`ranges`) também são úteis em instruções *if*:

```kotlin
fun main() {
    val x = 2
    if (x in 1..5) {            // 1
        print("x is in range from 1 to 5")
    }
    println()

    if (x !in 6..10) {          // 2
        print("x is not in range from 6 to 10")
    }
}
```
**Observações e comentários do código**

- //1. Verifica se um valor está no intervalo.

- //2. *`!in`* é o oposto de *`in`*.

**Saída do código**
```
x is in range from 1 to 5
x is not in range from 6 to 10
```

### Verificações de igualdade == e ===

Kotlin usa '==' para comparação estrutural e '===' para comparação referencial.

A comparação referencial está relacionada com a posição de memória usada pela variável. Já a comparação estrutural é lógica.

```kotlin
fun main() {

    val authors = setOf("Shakespeare", "Hemingway", "Twain")
    val writers = setOf("Twain", "Shakespeare", "Hemingway")

    println(authors == writers)   // 1
    println(authors === writers)  // 2
}
```
**Observações e comentários do código**

- //1. Retorna `true` porque '==' chama autores.equals(writers) e os conjuntos (sets) ignoram a ordem dos elementos.

- //2. Retorna `false` porque para '===' `authors` e `writers` são referências distintas.

⚠️ Em uma lista (list) pode haver elementos repetidos, mas em um conjunto (set) não.

**Saída do código**
```
true  - Porque `authors` e `writers` tem os mesmos elementos.
false - Porque `authors` e `writers` foram instanciados em momentos diferentes e ocupam posições de memória diferentes. 
```

### Expressão condicional

Em Kotlin não há operador ternário ```condition ? then : else``` (condição ? então : senão). Em vez disso, pode-se usar o `if` como uma expressão:

```kotlin
fun main() {
    fun max(a: Int, b: Int) = if (a > b) a else b   // 1

    println(max(99, -42))
    println(max(99, 101))
}
```
**Observações e comentários do código**

- //1. O `if` é uma expressão aqui: retorna um valor.

**Saída do código**
```
99
101
```

O mesmo código acima escrito na forma clássica da função:
```kotlin
fun main() {
    fun maxClassic (a: Int, b: Int): Int {
        if (a > b) {
            return a 
        } else {
            return b
        }
    }   

    println(maxClassic(99, -42))
    println(maxClassic(99, 101))
}
```
## Coleções

Agora vamos entrar no mundo das estruturas de dados em Kotlin.

### Listas (List)

Essa é uma das estruturas mais utilizadas em Kotlin. 

`list` é uma coleção **ordenada** de itens. No Kotlin, as listas podem ser mutáveis ​​(*MutableList*) ou somente leitura (*List*). 

Para criação de listas, use as funções de biblioteca padrão `listOf()` para listas somente leitura e `mutableListOf()` para listas mutáveis. 

Para evitar modificações indesejadas, obtenha visualizações somente leitura de listas mutáveis ​​convertendo-as em List.

```kotlin
val systemUsers: MutableList<Int> = mutableListOf(1, 2, 3)        // 1
val sudoers: List<Int> = systemUsers                              // 2

fun addSystemUser(newUser: Int) {                                 // 3
    systemUsers.add(newUser)                      
}

fun getSysSudoers(): List<Int> {                                  // 4
    return sudoers
}

fun main() {
    addSystemUser(4)                                              // 5 
    println("Tot sudoers: ${getSysSudoers().size}")               // 6
    getSysSudoers().forEach {                                     // 7
        i -> println("Some useful info on user $i")
    }
    // getSysSudoers().add(5) <- Error!                           // 8
}
```

**Observações e comentários do código**

- //1. Cria uma MutableList com três elementos (1,2,3)

- //2. Cria uma cópia somente leitura (lista sudoers) da lista mutável systemUsers.

- //3.  função que adiciona um novo item à MutableList.

- //4. Função que retorna todos os items da lista Sudoers(read-only)

- //5. Atualiza a MutableList systemUsers. Todas as visualizações somente leitura relacionadas também são atualizadas, pois apontam para o mesmo objeto. Por isso a lista Sudoers é atualizada automaticamente. 

- ⚠ Observe que embora o objeto systemUsers esteja declarado como val (imutáavel) ele contém uma lista mutável. Assim devemos entender que sua instância é imutável e não os elementos que ela contém. Isso significa que não pode haver mais de uma instância de systemUsers.

- //6. Recupera e imprime o tamanho da lista somente leitura Sudoers.

- //7. Percorre e imprime a lista (read-only) Sudoers

- //8. A tentativa de gravar na lista Sudoers somente leitura causa um erro de compilação.

**Saída do código com a última linha comentada**
```
Total sudoers: 4
Some useful info on user 1
Some useful info on user 2
Some useful info on user 3
Some useful info on user 4
```

**Saída do código com a última linha 'descomentada'**
```
Unresolved reference: add
Classifier 'Error' does not have a companion object, and thus must be initialized here
Unexpected tokens (use ';' to separate expressions on the same line)
```

### Conjuntos (Set)

São tipos de dados como as listas, porém não são ordenados, `não` permitindo elementos `duplicados`.

Um conjunto (set) é uma coleção não ordenada que não suporta duplicatas. Para criar conjuntos, existem as funções `setOf()` e `mutableSetOf()`. 

Uma visualização somente leitura de um conjunto mutável pode ser obtida convertendo-o em Set.


```kotlin
val openIssues: MutableSet<String> = mutableSetOf("uniqueDescr1", "uniqueDescr2", "uniqueDescr3") // 1

fun addIssue(uniqueDesc: String): Boolean {                                                       
    return openIssues.add(uniqueDesc)                                                             // 2
}

fun getStatusLog(isAdded: Boolean): String {                                                       
    return if (isAdded) "registered correctly." else "marked as duplicate and rejected."          // 3
}

fun main() {
    val aNewIssue: String = "uniqueDescr4"
    val anIssueAlreadyIn: String = "uniqueDescr2" 

    println("Issue $aNewIssue ${getStatusLog(addIssue(aNewIssue))}")                              // 4
    println("Issue $anIssueAlreadyIn ${getStatusLog(addIssue(anIssueAlreadyIn))}")                // 5 
}
```

**Observações e comentários do código**

- //1. Cria um conjunto 'openIssues' mutável com determinados elementos.

- //2. Função que retorna um valor booleano caso um elemento seja realmente adicionado.

- //3.  Retorna uma string de confirmação ou rejeição da operação de inserção de um novo elemento.

- ⚠ Em seguida duas novas variáveis são criadas: uma com um novo elemento e uma com um elemento já existente no conjunto.

- //4. Imprime uma mensagem de sucesso: o `novo` elemento é adicionado ao Conjunto.


- //5. Imprime uma mensagem de falha: o elemento não pode ser adicionado porque duplica um elemento existente (uniqueDescr2)

**Saída do código**
```
Issue uniqueDescr4 registered correctly.
Issue uniqueDescr2 marked as duplicate and rejected.
```
### Mapas (Map)

Um mapa é uma coleção de pares chave/valor, onde cada chave é única e é usada para recuperar o valor correspondente. Para criar mapas, existem funções `mapOf()` e `mutableMapOf()`. 

**Observação:**
Usar a função `to` do tipo *infix* torna a inicialização menos 'ruidosa'. 

Uma visualização somente leitura de um mapa mutável pode ser obtida convertendo-o em Map.

```kotlin
const val POINTS_X_PASS: Int = 15
val EZPassAccounts: MutableMap<Int, Int> = mutableMapOf(1 to 100, 2 to 100, 3 to 100)   // 1
val EZPassReport: Map<Int, Int> = EZPassAccounts                                        // 2

fun updatePointsCredit(accountId: Int) {
    if (EZPassAccounts.containsKey(accountId)) {                                        // 3
        println("Updating $accountId...")                                               
        EZPassAccounts[accountId] = EZPassAccounts.getValue(accountId) + POINTS_X_PASS  // 4
    } else {
        println("Error: Trying to update a non-existing account (id: $accountId)")
    } 
}

fun accountsReport() {
    println("EZ-Pass report:")
    EZPassReport.forEach {                                                              // 5
        k, v -> println("ID $k: credit $v")
    }
}

fun main() {
    accountsReport()                                                                    // 6
    updatePointsCredit(1)                                                               // 7
    updatePointsCredit(1)                                                               
    updatePointsCredit(5)                                                               // 8 
    accountsReport()                                                                    // 9
}
```
**Observações e comentários do código**

- //1. Cria um mapa mutável PassAccounts <Int, Int> com os pares conta : pontuação.

- //2. Cria um mapa imutável (read-only map) que é uma `view` de `PassAccounts`

- //3. Função que atualiza a pontuação de uma conta, verificando antes se ela já existe (lembre que a chave deve ser única) com o método `.containsKey()`. 

- //4. Caso o valor chave (key value) exista será feita a atualização dos pontos. Caso não exista será emitida uma mensagem informativa.

- //5. Função que percorre o mapa imutável `PassReport` e imprime os pares de chave/valor.

- //6. Lê o saldo de pontos da conta `PassReport` no mapa imutável (read-only), antes das atualizações.

- //7. Atualiza uma conta existente (key = 1) duas vezes.

- //8. Tenta atualizar uma conta inexistente (key = 5): imprime uma mensagem de erro.

- //9. Lê o saldo de pontos da conta `PassReport` no mapa imutável (read-only), antes das atualizações.

**Saída do código**

```
EZ-Pass report:
ID 1: credit 100
ID 2: credit 100
ID 3: credit 100
Updating 1...
Updating 1...
Error: Trying to update a non-existing account (id: 5)
EZ-Pass report:
ID 1: credit 130
ID 2: credit 100
ID 3: credit 100
```

### Funções úteis

Iremos aprender algumas funções que são muito úteis para se trabalhar com as estruturas de dados acima: list, set e map.

- #### **filter**

A função de filtro permite filtrar coleções. É necessário um predicado de filtro como parâmetro lambda (função anônima). O predicado é aplicado a cada elemento. Os elementos que tornam o predicado verdadeiro são retornados na coleção de resultados.

```kotlin
fun main() {

    val numbers = listOf(1, -2, 3, -4, 5, -6)      // 1

    val positives = numbers.filter { x -> x > 0 }  // 2

    val negatives = numbers.filter { it < 0 }      // 3

    println("Numbers: $numbers")
    println("Positive Numbers: $positives")
    println("Negative Numbers: $negatives")
}
```
**Observações e comentários do código**

- //1. Define a coleção (lista) de 'numbers' (imutável).

- //2. Através do `predicado` obtém números positivos (`.filter { x -> x > 0 }`)

- //3.  Usa a notação `it` mais curta para obter números negativos. Essa notação também poderia ter sido usada no exemplo anterior (`.filter { it < 0 }`)

**Saída do código**

```
Numbers: [1, -2, 3, -4, 5, -6]
Positive Numbers: [1, 3, 5]
Negative Numbers: [-2, -4, -6]
```

- #### **map**

A função de extensão `map()` permite aplicar uma transformação a todos os elementos de uma coleção. É necessária uma função de transformador como parâmetro lambda.

```kotlin
fun main() {

    val numbers = listOf(1, -2, 3, -4, 5, -6)     // 1

    val doubled = numbers.map { x -> x * 2 }      // 2

    val tripled = numbers.map { it * 3 }          // 3

    println("Numbers: $numbers")
    println("Doubled Numbers: $doubled")
    println("Tripled Numbers: $tripled")

    println("O tipo de doubled é : ${doubled::class.simpleName}")
    println("O tipo de tripled é : ${doubled::class.simpleName}")
}
```


**Observações e comentários do código**

- //1. Define a coleção (lista) de 'numbers' (imutável).

- //2. Através do `predicado` que é uma função anônima obtém uma coleção que é o dobro de `numbers` com `.map { x -> x * 2 }`

- //3.  Usa a notação `it` mais curta para obter o triplo de `numbers`. Essa notação também poderia ter sido usada no exemplo anterior ( `.map { it * 3 }` )

**Saída do código**

```
Numbers: [1, -2, 3, -4, 5, -6]
Doubled Numbers: [2, -4, 6, -8, 10, -12]
Tripled Numbers: [3, -6, 9, -12, 15, -18]
O tipo de doubled é : ArrayList
O tipo de tripled é : ArrayList
```

- #### **any, all e none**

Estas funções verificam a existência de elementos de coleção que correspondem a um determinado predicado.

Elas retornam `true` se a coleção contiver pelo menos um elemento que corresponda ao predicado fornecido.

```kotlin
fun main() {

    val numbersAny = listOf(1, -2, 3, -4, 5, -6)            // 1
    val anyNegative = numbersAny.any { it < 0 }             // 2
    val anyGT6 = numbersAny.any { it > 6 }                  // 3

    println("\nFunction any returns true if the collection contains at least one element that matches the given predicate.")
    println("Numbers: $numbersAny")
    println("Is there any number less than 0: $anyNegative")
    println("Is there any number greater than 6: $anyGT6")



    val numbersAll = listOf(1, -2, 3, -4, 5, -6)            // 4
    val allEven = numbersAll.all { it % 2 == 0 }            // 5
    val allLess6 = numbersAll.all { it < 6 }                // 6

    println ("\nFunction all returns true if all elements in collection match the given predicate.")
    println("Numbers: $numbersAll")
    println("All numbers are even: $allEven")
    println("All numbers are less than 6: $allLess6")



    val numbersNone = listOf(1, -2, 3, -4, 5, -6)            // 7
    val allEvenb = numbersNone.none { it % 2 == 1 }           // 8
    val allLess6b = numbersNone.none { it > 6 }               // 9

    println("\nFunction none returns true if there are no elements that match the given predicate.")
    println("Numbers: $numbersNone")
    println("All numbers are even: $allEvenb")
    println("No element greater than 6: $allLess6b") 
}
```
**Saída do código**
```
Function any returns true if the collection contains at least one element that matches the given predicate.
Numbers: [1, -2, 3, -4, 5, -6]
Is there any number less than 0: true
Is there any number greater than 6: false

Function all returns true if all elements in collection match the given predicate.
Numbers: [1, -2, 3, -4, 5, -6]
All numbers are even: false
All numbers are less than 6: true

Function none returns true if there are no elements that match the given predicate.
Numbers: [1, -2, 3, -4, 5, -6]
All numbers are even: false
No element greater than 6: true
```


## Materiais de apoio e questionário

[Material de apoio no repositório da DIO]( 
https://github.com/digitalinnovationone/aprenda-kotlin-com-exemplos)


## Certificado

![Certificado](../img/)

```
Disclaimer:

Todo o material aqui apresentado foi gerado a partir de minhas anotações de aula durante o excelente
treinamento ministrado  pelo Instrutor Venilton Falvo Jr. @falvojr. Proibida a reprodução e veiculação
sem a ciência e autorização da DIO.
```
