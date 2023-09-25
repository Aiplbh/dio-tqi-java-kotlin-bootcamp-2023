# Aperfeiçoe Sua Lógica e Pensamento Computacional

```
Bootcamp Code Update TQI Back End com Java e Kotlin
Módulo: Explorando Padrões de Projeto na prática com Kotlin
Instrutor: Venilton Falvo Jr. @falvojr - 29/08/23 a 29/10/23
```

## Apresentação


## PBL, Lógica e Pensamento Computacional


## Desafios de código

Para todas as linguagens exploradas será usado o mesmo enunciado a seguir:

---

**Desafio**

Faça um programa que calcule e imprima o salário a ser transferido para um funcionário.

Para realizar o cálculo, receba o valor bruto do salário e o adicional dos benefícios.

O salário a ser transferido é calculado da seguinte maneira: 

Salario = (valorBrutoSalario - percentualImpostoSalario) + adicionalBeneficios

Cálculo do imposto

Seguir as alíquotas:

| Faixa salarial | Alíquota % |
|----------------| -----------|
| R$ 0,00 a R$ 1100,00 | 5.00 % |
| R$ 1100,01 a R$ 2500.00 | 10.00 % |
| R$ 2500,01 e acima | 15.00 |

**Entrada**

A entrada consiste em vários arquivos de teste que conterão o ***valor bruto do salário*** e o ***adicional dos benefícios***. Conforme mostrado no exemplo de entrada a seguir.

**Saída**

Para cada arquivo de entrada, terá um arquivo de saída. E como mencionado no Desafio, será gerado uma linha com um número que terá a diferença entre o valor bruto do salário e o percentual de imposto mediante a faixa salarial somado com o adicional dos benefícios. Use o exemplo abaixo para clarear o que está sendo pedido:

| Entrada | Saída |
| --------|-------|
| 2000 | 2050,00 |
| 250 |         |

---

## Desafio Java

Utilizei a IDE Intellij Idea para compilar o código

```java
import java.util.Scanner;

public class DesafioJava {

    public static void main (String [] args) {

        // Leitura dos valores de entrada com Scanner()

        Scanner leitorEntrada = new Scanner (System.in);
        float salarioBruto = leitorEntrada.nextFloat();
        float valorBeneficio = leitorEntrada.nextFloat();

        // Cálculo do imposto

        float  valorImposto = 0;

        if (salarioBruto >= 0 && salarioBruto <= 1100.00) {
            valorImposto = 0.05F * salarioBruto;
        } else if (salarioBruto >= 1000.01 && salarioBruto <= 2500.00) {
            valorImposto = 0.10F * salarioBruto;
        } else {
            valorImposto = 0.15F * salarioBruto;
        }

        // Impressão da saída com duas casas decimais

        float saida = salarioBruto - valorImposto + valorBeneficio;
        System.out.println (String.format("%.2f", saida));

    }
}
```
## Desafio C#

```C#
// Para ler e escrever dados em C# utilizamos os seguintes métodos da classe Console:
// - Console.ReadLine: lê UMA linha de dado de Entrada
// - Console.WriteLine: imprime um texto de Saída saltando uma linha após a impressão
// O método ReadLine recebe uma String, daí a necessidade de conversão para float com Parse.

using System;

public class DesafioCS {

    public static void Main() {

        // Leitura dos dados de entrada
        float salarioBruto = float.Parse(Console.Readline());
        float valorBeneficio = float.Parse(Console.Readline());

        // Cálculo do imposto

        float valorImposto = 0;

        if (salarioBruto >= 0 && salarioBruto <= 1100.00) {
            valorImposto = 0.05F * salarioBruto;
        } else if (salarioBruto >= 1000.01 && salarioBruto <= 2500.00) {
            valorImposto = 0.10F * salarioBruto;
        } else {
            valorImposto = 0.15F * salarioBruto;
        }

        // Impressão da saída com duas casas decimais

        float saida = salarioBruto - valorImposto + valorBeneficio;
        Console.WriteLine (saida.ToString("0.00"));
    }
}
```
## Desafio JavaScript

```javascript
// Na DIO as funções `get`e `print`são acessíveis globalmente
// - gets: lê UMA linha com dados de entrada (inputs) do user
// - print: imprime um texto de saída (output), pulando linha

// Leitura dos dados de entrada
const salarioBruto = parseFloat(gets());
const valorBeneficio = parseFloat(gets());

// Cálculo do imposto através da função 'calcularImposto(salario)'
const valorImposto = calcularImposto(salarioBruto);

// Cálcula valor líquido 
const saida = salarioBruto - valorImposto + valorBeneficio;

// Imprime saída com duas casas decimais
print (saida.toFixed(2));

// função para o cálculo do imposto

function calcularImposto (salario){
    let aliquota;
    
    if (salario >= 0 && salario <= 1100.00) {
            aliquota = 0.05F ;
        } else if (salario >= 1000.01 && salario <= 2500.00) {
            aliquota = 0.10F;
        } else {
            aliquota = 0.15F;
        }
    return aliquota*salario 
}
```

## Desafio Python

```python
# Para ler e escrever dados em Python, utilizamos as seguintes funções:
# - input: lê UMA linha com dados de entrada (input) do usuário
# - print: imprime um texto de saída (output), pulando uma linha

# Função para cálculo do imposto
def calcular_imposto(salario)
  aliquota = 0.00
  if (salario >= 0 and salario <= 1100):
    aliquota = 0.05
  elif (salario >= 1000.01 and salario <= 2500.00):
    aliquota = 0.10
  else:
    aliquota = 0.15

  return aliquota*salario

# Leitura dos dados de entrada

salario_bruto = float(input())
valor_beneficio = float(input())

# Cálcula valor líquido 
valor_imposto = calcular_imposto(valor_salario)
saida = valor_salario - valor_imposto + valor_beneficio

# Imprime saída com duas casas decimais
print(f'{saida:2f}')

```
Desafio Kotlin

```kotlin
object OrgaoArrecadador {

    fun calcularImposto (salario: Double) : Double {
        val aliquota = when {
            (salario >= 0 and salario <= 1100) -> 0.05
            (salario >= 1000.01 and salario <= 2500.00) -> 0.10
        else -> 0.15
        }
    return aliquota * salario
    }
}

fun main()  {
    val salarioBruto = readLine().toDouble()
    val valorBeneficio = = readLine().toDouble()

    // Cálculo do imposto
    val valorImposto = OrgaoArrecadador.calcularImposto(valorSalario)

    // Imprime saída
    val saída valorSalario - valorImposto + valorBeneficio
    println(String.format(%.2f, saida))
}
```

## Conclusão

Nesse módulo foi possível entender com clareza a importância da Lógica e do Pensamento computacional na solução de problemas. 

A capacidade de gerar código, embora seja uma importante habilidade a ser desenvolvida, fica como instrumento para aplicar a uma solução lógica.

Faltou explicar o código em Kotlin, onde uma estrutura do tipo (!!) aparece no entrada de dados do usuário. 

O operador !! é chamado de operador de assertiva não nula (non-null assertion operator). Ele é usado para indicar que o valor retornado por readLine() não pode ser nulo. Se readLine() retornar null, isso resultará em uma exceção do tipo NullPointerException. Em outras palavras, o código assume que o valor de readLine() não é nulo.

## Materiais de apoio e referências

[Repositório da DIO](https://github.com/digitalinnovationone/aprenda-kotlin-com-exemplos)

[Documentação oficial do Kotlin](https://kotlinlang.org/)

[Ferramenta de Inteligência artificial ChatGPT](https://chat.openai.com/)

[Ferramenta de Inteligência artificial AI Converter](https://aicodeconvert.com/)

## Certificado

![Certificado](../img/m3-certificadoLogicaPensamentoComputacional.png)

```
Disclaimer:

Todo o material aqui apresentado foi gerado a partir de minhas anotações de aula durante o excelente
treinamento ministrado  pelo Instrutor Venilton Falvo Jr. @falvojr. Proibida a reprodução e veiculação
sem a ciência e autorização da DIO e do autor.
```
