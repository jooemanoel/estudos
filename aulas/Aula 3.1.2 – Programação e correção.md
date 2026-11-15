# Aula 3.1.2 – Programação e correção

Nov 13, 2026 · @João

## Mapa do item

O item 3.1.2 é o tópico “2 Programação e correção” do bloco “Engenharia de Software e Programação”, primeiro bloco dos conhecimentos específicos do cargo 4. Ele tem oito subitens e é a base de Java para tudo o que vem depois no edital (Spring, JPA, Spring Batch, testes).

A correspondência abaixo usa a sua numeração hierárquica e a ordem do [edital verticalizado de Desenvolvimento de Sistemas](https://arquivos.voceconcursado.com.br/wp-content/uploads/2026/10/09190213/Edital-Verticalizado-cgu-2026-desenvolvimento-de-sistemas.pdf), montado a partir do [Edital CGU nº 1/2026](https://cdn.direcaoconcursos.com.br/uploads/2026/10/CGU-edital.pdf).

| Subitem | Conteúdo no edital | O que o Cebraspe tende a explorar |
| --- | --- | --- |
| 3.1.2.1 | Linguagem Java: tipos de dados, estruturas de controle, métodos, classes e objetos | Saída de trechos de código, promoção numérica, passagem de parâmetros |
| 3.1.2.2 | Encapsulamento, interfaces, herança e polimorfismo | Sobrescrita × sobrecarga, despacho dinâmico, regras de interface |
| 3.1.2.3 | Coleções e tipos genéricos | Escolha da coleção, nulos, ordem, curingas e apagamento de tipos |
| 3.1.2.4 | Igualdade de objetos, mutabilidade, tratamento de exceções e leitura e escrita de arquivos | `==` × `equals`, contrato com `hashCode`, `finally`, try-with-resources |
| 3.1.2.5 | Estruturas de dados, algoritmos e pseudocódigo | Teste de mesa, pilha × fila, ordenação e busca |
| 3.1.2.6 | Leitura, depuração e testes de borda | Achar o defeito no trecho, valores-limite |
| 3.1.2.7 | Complexidade em decisões de desempenho | Big-O aplicado à escolha de estrutura |
| 3.1.2.8 | Modularidade e padrões de projeto aplicados à manutenção | Coesão, acoplamento, SOLID, padrões GoF |

O edital não fixa a versão do Java. Esta aula indica a versão em que cada recurso entrou, para você julgar itens que a mencionem.

Na P3 cada item vale 2 pontos e cada erro desconta 2. Em itens de código, só marque depois de fazer o teste de mesa completo.

## 3.1.2.1 Linguagem Java: tipos, controle, métodos, classes e objetos

Java é fortemente tipada, compilada para bytecode e executada pela JVM. Quase todo item de código depende de três regras: promoção numérica, passagem por valor e ordem de inicialização.

### Tipos de dados

Há oito tipos primitivos; todo o resto é referência a objeto.

| Tipo | Tamanho | Observação |
| --- | --- | --- |
| `byte` | 8 bits | -128 a 127 |
| `short` | 16 bits | com sinal |
| `int` | 32 bits | padrão para literais inteiros |
| `long` | 64 bits | literal com sufixo `L` |
| `float` | 32 bits | literal com sufixo `f` |
| `double` | 64 bits | padrão para literais decimais |
| `char` | 16 bits | sem sinal, unidade UTF-16 |
| `boolean` | não especificado | apenas `true` ou `false`; não converte para inteiro |

Regras que viram item de prova:

- **Valor padrão.** Atributos recebem valor padrão (`0`, `false`, `null`). Variável local não recebe: usá-la sem inicializar é erro de compilação.
- **Promoção.** Em operações aritméticas, `byte`, `short` e `char` são promovidos a `int`. Por isso `b = b + 1` não compila para `byte b`, mas `b += 1` compila, pois a atribuição composta embute a conversão.
- **Divisão inteira.** `7 / 2` resulta em `3`; `7 / 2.0` resulta em `3.5`. `7 % 2` resulta em `1`.
- **Estouro.** Inteiros estouram em silêncio: `Integer.MAX_VALUE + 1` vira `Integer.MIN_VALUE`, sem exceção.
- **Ponto flutuante.** `0.1 + 0.2 == 0.3` é `false`. Valores monetários usam `BigDecimal`.
- **Divisão por zero.** Com inteiros lança `ArithmeticException`; com `double` resulta em `Infinity` ou `NaN`, sem exceção.
- **Wrappers.** `Integer`, `Long` etc. são objetos imutáveis. O autoboxing usa `Integer.valueOf`, que reaproveita instâncias de -128 a 127.
- **`var`** (Java 10). Inferência de tipo apenas para variáveis locais com inicializador. O tipo continua estático.

```java
Integer a = 127, b = 127;
Integer c = 128, d = 128;
System.out.println(a == b);      // true  (mesma instância em cache)
System.out.println(c == d);      // false (objetos distintos)
System.out.println(c.equals(d)); // true
```

### Estruturas de controle

- `if/else`, operador ternário, `for`, for-each, `while` e `do-while`. O `do-while` executa o corpo pelo menos uma vez.
- `&&` e `||` fazem curto-circuito: o segundo operando só é avaliado se necessário. `&` e `|` avaliam sempre os dois.
- `i++` devolve o valor antigo e depois incrementa; `++i` incrementa e devolve o novo.
- `switch` clássico aceita `byte`, `short`, `char`, `int`, seus wrappers, `enum` e `String` (Java 7). Não aceita `long`, `float`, `double` nem `boolean`.
- Sem `break`, o `switch` clássico “cai” para o próximo `case` (fall-through). A forma com seta `->` (Java 14) não cai e pode devolver valor.
- `break` encerra o laço; `continue` pula para a próxima iteração.

```java
int x = 2;
switch (x) {
    case 1: System.out.print("A");
    case 2: System.out.print("B");
    case 3: System.out.print("C");
    default: System.out.print("D");
}
// imprime BCD
```

### Métodos

Java passa argumentos **sempre por valor**. Para objetos, o valor copiado é a referência: o método altera o estado do objeto, mas não troca o objeto do chamador.

```java
static void altera(StringBuilder sb, int n) {
    sb.append("!");            // altera o objeto compartilhado
    sb = new StringBuilder();  // só muda a cópia local da referência
    n = 99;                    // só muda a cópia local
}

StringBuilder s = new StringBuilder("oi");
int k = 1;
altera(s, k);
System.out.println(s + " " + k); // oi! 1
```

- **Assinatura** é nome mais lista de tipos dos parâmetros. O tipo de retorno não faz parte dela.
- **Sobrecarga** são métodos de mesmo nome com parâmetros diferentes. Mudar só o retorno não compila.
- **`static`** pertence à classe, não à instância. Método estático não acessa `this` nem membros de instância diretamente.
- **Varargs** (`int... v`) é recebido como array e deve ser o último parâmetro.

### Classes e objetos

Classe é o molde; objeto é a instância criada com `new` e armazenada no heap. A variável guarda a referência.

- O compilador só gera o construtor padrão, sem argumentos, se a classe não declarar nenhum construtor.
- `this(...)` chama outro construtor da mesma classe; `super(...)` chama o da superclasse. Qualquer um deles deve ser a primeira instrução.
- Ordem de inicialização: blocos e atributos estáticos, uma vez, no carregamento da classe. Depois, a cada `new`: inicializadores de instância e construtor da superclasse, seguidos pelos da subclasse.
- `final` em variável impede nova atribuição; em método impede sobrescrita; em classe impede herança.
- Um arquivo `.java` tem no máximo uma classe pública de nível superior, com o mesmo nome do arquivo.
- Objetos sem referência alcançável ficam elegíveis para o coletor de lixo. Não há liberação manual de memória.

| Modificador | Mesma classe | Mesmo pacote | Subclasse em outro pacote | Qualquer lugar |
| --- | --- | --- | --- | --- |
| `private` | sim | não | não | não |
| sem modificador | sim | sim | não | não |
| `protected` | sim | sim | sim | não |
| `public` | sim | sim | sim | sim |

`record` (Java 16) declara uma classe de dados imutável: atributos `private final`, construtor canônico, acessores, `equals`, `hashCode` e `toString` gerados. `enum` define um conjunto fechado de instâncias.

## 3.1.2.2 Encapsulamento, interfaces, herança e polimorfismo

O ponto mais cobrado é a diferença entre o que se decide na compilação (sobrecarga, atributos, métodos estáticos) e o que se decide na execução (métodos de instância sobrescritos).

### Encapsulamento

Encapsular é esconder o estado interno e expor apenas operações que preservam as regras do objeto. Em Java: atributos `private` e métodos públicos que validam.

- Getter e setter para tudo não é encapsulamento; é só acesso indireto. O ganho está em proteger invariantes.
- Devolver uma lista interna mutável quebra o encapsulamento. Devolva cópia ou visão não modificável.
- Encapsulamento reduz acoplamento: quem usa a classe depende do contrato, não da implementação.

### Herança

- `extends` cria relação “é um”. Java tem herança simples de classes: uma classe estende só uma outra.
- Toda classe descende de `Object`.
- Construtores não são herdados. O construtor da subclasse chama `super()` implicitamente; se a superclasse não tiver construtor sem argumentos, a chamada explícita é obrigatória.
- Membros `private` existem no objeto, mas não são acessíveis à subclasse.
- `final class` não pode ser estendida. `sealed` (Java 17) restringe quais classes podem estender, com `permits`.
- Herança acopla fortemente. Prefira composição quando a relação for “tem um” ou só reúso de código.

### Interfaces e classes abstratas

| Aspecto | Interface | Classe abstrata |
| --- | --- | --- |
| Quantas por classe | várias (`implements A, B`) | uma (`extends`) |
| Atributos | apenas constantes `public static final` | qualquer tipo de atributo |
| Construtor | não tem | tem, usado pelas subclasses |
| Métodos com corpo | `default` e `static` (Java 8), `private` (Java 9) | sim, livremente |
| Métodos sem corpo | `public abstract` implícito | precisam de `abstract` explícito |
| Instanciação direta | não | não |

- Uma interface pode estender várias interfaces.
- Se duas interfaces trazem o mesmo método `default`, a classe é obrigada a sobrescrevê-lo.
- Interface funcional tem exatamente um método abstrato e pode ser alvo de lambda.
- Classe concreta deve implementar todos os métodos abstratos herdados; senão, deve ser `abstract`.

### Polimorfismo

Uma referência do tipo da superclasse ou interface pode apontar para objetos de subtipos. A chamada de método de instância é resolvida pelo **tipo real do objeto**, em tempo de execução (ligação dinâmica).

|  | Sobrescrita (override) | Sobrecarga (overload) |
| --- | --- | --- |
| Onde | subclasse redefine método herdado | mesma classe ou subclasse, mesmo nome |
| Parâmetros | idênticos | diferentes |
| Resolução | execução, pelo tipo do objeto | compilação, pelo tipo declarado dos argumentos |
| Retorno | igual ou subtipo (covariante) | livre |

Regras de sobrescrita:

- A visibilidade não pode ficar mais restrita.
- Não pode declarar exceções verificadas novas ou mais amplas.
- Métodos `final`, `static` e `private` não são sobrescritos.
- `@Override` é opcional, mas faz o compilador acusar erro se nada estiver sendo sobrescrito.

Atributos e métodos estáticos **não** são polimórficos. Eles são resolvidos pelo tipo declarado da referência; a subclasse apenas os oculta.

```java
class Animal {
    String nome = "animal";
    String som() { return "..."; }
    static String tipo() { return "A"; }
}
class Cao extends Animal {
    String nome = "cao";
    @Override String som() { return "au"; }
    static String tipo() { return "C"; }
}

Animal a = new Cao();
System.out.println(a.som());  // au      (execução: objeto é Cao)
System.out.println(a.nome);   // animal  (compilação: referência é Animal)
System.out.println(a.tipo()); // A       (estático: referência é Animal)
```

Sobrecarga olha o tipo declarado do argumento, não o objeto:

```java
static void f(Object o) { System.out.println("Object"); }
static void f(String s) { System.out.println("String"); }

Object x = "texto";
f(x); // Object
```

Conversões de referência:

- Upcast (subtipo para supertipo) é implícito e sempre seguro.
- Downcast exige conversão explícita e pode lançar `ClassCastException` na execução.
- `instanceof` testa o tipo antes; com `null` devolve `false`. Desde o Java 16: `if (a instanceof Cao c) { ... }`.

## 3.1.2.3 Coleções e tipos genéricos

A escolha da coleção depende de quatro perguntas: há ordem, aceita duplicata, aceita nulo e quanto custa cada operação. `Map` faz parte do framework de coleções, mas **não** estende `Collection`.

```
Iterable
  Collection
    List   -> ArrayList, LinkedList
    Set    -> HashSet, LinkedHashSet, TreeSet (SortedSet)
    Queue  -> PriorityQueue
      Deque -> ArrayDeque, LinkedList
Map (hierarquia separada) -> HashMap, LinkedHashMap, TreeMap (SortedMap)
```

### Implementações principais

| Classe | Ordem de iteração | Duplicatas | Nulos | Custo típico |
| --- | --- | --- | --- | --- |
| `ArrayList` | inserção, com índice | sim | sim | `get` O(1); inserir no fim O(1) amortizado; inserir no meio O(n) |
| `LinkedList` | inserção | sim | sim | pontas O(1); `get(i)` O(n) |
| `HashSet` | sem garantia | não | um nulo | `add` e `contains` O(1) médio |
| `LinkedHashSet` | inserção | não | um nulo | O(1) médio |
| `TreeSet` | ordenada | não | não, na ordem natural | O(log n) |
| `HashMap` | sem garantia | chaves únicas | uma chave nula, valores nulos | `get` e `put` O(1) médio |
| `LinkedHashMap` | inserção (ou acesso) | chaves únicas | como `HashMap` | O(1) médio |
| `TreeMap` | ordenada pela chave | chaves únicas | chave nula não, na ordem natural | O(log n) |
| `ArrayDeque` | inserção | sim | não | pontas O(1); serve de pilha e de fila |
| `PriorityQueue` | heap; iteração não ordenada | sim | não | `offer` e `poll` O(log n); `peek` O(1) |

Pontos de atenção:

- `put` em chave existente substitui o valor e devolve o anterior. `add` em `Set` com elemento repetido devolve `false`.
- `HashSet` e `HashMap` dependem de `equals` e `hashCode`. `TreeSet` e `TreeMap` dependem de `compareTo` ou de um `Comparator`, inclusive para decidir duplicidade.
- `Comparable` define a ordem natural dentro da própria classe. `Comparator` define ordens externas e alternativas.
- `Vector` e `Hashtable` são sincronizadas e legadas. `Hashtable` e `ConcurrentHashMap` não aceitam chave nem valor nulos.
- Para pilha, prefira `ArrayDeque` (`push`, `pop`, `peek`) à classe legada `Stack`.

### Coleções não modificáveis

| Criação | Comportamento |
| --- | --- |
| `List.of(...)` (Java 9) | imutável; `add`, `set` e `remove` lançam `UnsupportedOperationException`; não aceita `null` |
| `Arrays.asList(...)` | tamanho fixo; `set` funciona e altera o array original; `add` e `remove` lançam exceção |
| `Collections.unmodifiableList(l)` | visão somente leitura; mudanças feitas em `l` aparecem na visão |
| `List.copyOf(l)` (Java 10) | cópia imutável, independente de `l` |

### Iteração e modificação

Remover elementos da coleção dentro de um for-each lança `ConcurrentModificationException`, mesmo com uma única thread. Os iteradores são fail-fast.

```java
List<Integer> nums = new ArrayList<>(List.of(1, 2, 3, 4));

for (Integer n : nums) {
    if (n % 2 == 0) nums.remove(n);   // ConcurrentModificationException
}

nums.removeIf(n -> n % 2 == 0);       // forma correta
```

Outra armadilha: em `List<Integer>`, `remove(1)` remove o elemento do **índice** 1. Para remover o valor 1, use `remove(Integer.valueOf(1))`.

### Tipos genéricos

Genéricos dão segurança de tipos em tempo de compilação e dispensam conversões explícitas.

- **Apagamento de tipos (type erasure).** Os parâmetros de tipo não existem em tempo de execução. `List<String>` e `List<Integer>` têm a mesma classe.
- Consequências: não se pode fazer `new T()`, `new T[10]`, `obj instanceof List<String>`, nem sobrecarregar métodos que diferem só no argumento de tipo.
- Não aceitam primitivos: `List<int>` não compila; usa-se `List<Integer>`.
- **Invariância.** `List<Integer>` não é subtipo de `List<Number>`, embora `Integer` seja subtipo de `Number`. Arrays são covariantes e falham só na execução, com `ArrayStoreException`.
- Operador diamante (`new ArrayList<>()`, Java 7) infere o argumento de tipo.
- Tipo limitado: `<T extends Comparable<T>>` restringe `T` a tipos comparáveis.

Curingas seguem a regra PECS (producer extends, consumer super):

| Declaração | Pode ler como | Pode adicionar |
| --- | --- | --- |
| `List<? extends Number>` | `Number` | nada, exceto `null` |
| `List<? super Integer>` | `Object` | `Integer` |
| `List<?>` | `Object` | nada, exceto `null` |

```java
static double soma(List<? extends Number> xs) {   // aceita List<Integer>, List<Double>
    double s = 0;
    for (Number n : xs) s += n.doubleValue();
    return s;
}

static <T extends Comparable<T>> T maior(List<T> xs) {  // método genérico
    T m = xs.get(0);
    for (T x : xs) if (x.compareTo(m) > 0) m = x;
    return m;
}
```

## 3.1.2.4 Igualdade de objetos, mutabilidade, exceções e arquivos

São quatro temas com um ponto em comum: erros que compilam sem aviso e só aparecem na execução.

### Igualdade de objetos

`==` compara valores em primitivos e **referências** em objetos. `equals` compara conteúdo, desde que a classe o sobrescreva; a versão herdada de `Object` usa `==`.

Contrato de `equals`: reflexivo, simétrico, transitivo, consistente e `x.equals(null)` devolve `false`.

Contrato de `hashCode`:

- Objetos iguais por `equals` devem ter o mesmo `hashCode`.
- O inverso não vale: objetos diferentes podem ter o mesmo `hashCode` (colisão).
- Quem sobrescreve `equals` deve sobrescrever `hashCode`. Sem isso, `HashSet` e `HashMap` não encontram o objeto.

```java
class Cpf {
    private final String numero;
    Cpf(String numero) { this.numero = numero; }

    @Override public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Cpf outro)) return false;
        return numero.equals(outro.numero);
    }
    @Override public int hashCode() { return Objects.hash(numero); }
}

Set<Cpf> set = new HashSet<>();
set.add(new Cpf("123"));
set.contains(new Cpf("123")); // true; sem hashCode sobrescrito, quase certamente false
```

Casos clássicos com `String`:

```java
String a = "java";
String b = "java";
String c = new String("java");
String p = "ja";

a == b            // true  (literais compartilham o pool)
a == c            // false (new cria outro objeto)
a.equals(c)       // true
a == c.intern()   // true
a == "ja" + "va"  // true  (constante resolvida na compilação)
a == p + "va"     // false (concatenação em tempo de execução)
```

Outros pontos:

- `Objects.equals(x, y)` trata `null` sem lançar exceção.
- Arrays não sobrescrevem `equals`; use `Arrays.equals(a, b)`.
- `BigDecimal.equals` considera a escala: `2.0` e `2.00` são diferentes. Para valor numérico, use `compareTo`.
- `record` gera `equals` e `hashCode` com base em todos os componentes.

### Mutabilidade

Objeto imutável não muda de estado depois de construído. `String`, os wrappers, `BigDecimal` e `LocalDate` são imutáveis.

- Receita de imutabilidade: classe `final`, atributos `private final`, sem setters e com cópia defensiva de componentes mutáveis.
- `final` na variável fixa a referência, não o objeto. `final List<String> l` ainda permite `l.add("x")`.
- Métodos de `String` devolvem nova instância. `s.toUpperCase()` sem atribuição não altera `s`.
- `StringBuilder` é mutável e não sincronizado. `StringBuffer` é mutável e sincronizado.
- Concatenar `String` com `+` dentro de laço cria um objeto por iteração, com custo total quadrático.
- Objetos imutáveis são seguros entre threads e bons como chave de `HashMap`.
- Alterar um atributo usado no `hashCode` depois de inserir o objeto em um `HashSet` faz o conjunto “perdê-lo”.

### Tratamento de exceções

```
Throwable
  Error                (OutOfMemoryError, StackOverflowError) - não se trata
  Exception            verificadas: IOException, SQLException, InterruptedException
    RuntimeException   não verificadas: NullPointerException, IllegalArgumentException,
                       ArithmeticException, ClassCastException,
                       ArrayIndexOutOfBoundsException, NumberFormatException
```

- **Verificadas (checked).** O compilador exige `try/catch` ou `throws` na assinatura.
- **Não verificadas (unchecked).** `RuntimeException` e subclasses; indicam, em geral, erro de programação. `Error` também não é verificado.
- `throw` lança uma exceção; `throws` declara na assinatura que o método pode lançá-la.
- `catch` mais específico deve vir antes do mais genérico. A ordem inversa é erro de compilação, por código inalcançável.
- Multi-catch (`catch (IOException | SQLException e)`) não aceita tipos com relação de herança entre si.
- `finally` executa com ou sem exceção, inclusive quando há `return` no `try`. Só não executa com `System.exit` ou queda da JVM.
- `return` dentro do `finally` substitui o retorno do `try` e descarta a exceção pendente.

```java
static int f() {
    try { return 1; }
    finally { System.out.print("fim "); }
}   // imprime "fim " e devolve 1

static int g() {
    try { throw new RuntimeException(); }
    finally { return 2; }
}   // devolve 2; a exceção é engolida
```

**Try-with-resources** (Java 7) fecha automaticamente todo recurso que implemente `AutoCloseable`.

- O fechamento ocorre na ordem inversa da declaração.
- Os recursos são fechados **antes** da execução do `catch` e do `finally`.
- Se o corpo e o `close` lançarem exceções, prevalece a do corpo; a outra fica em `getSuppressed()`.

Boas práticas cobradas: não capturar `Exception` genérica sem necessidade, não deixar `catch` vazio e preservar a causa ao relançar (`new MinhaExcecao(msg, e)`).

### Leitura e escrita de arquivos

Há duas APIs: `java.io` (fluxos) e `java.nio.file` (`Path` e `Files`, desde o Java 7).

| Necessidade | Opção |
| --- | --- |
| Bytes | `InputStream` e `OutputStream` |
| Texto | `Reader` e `Writer` |
| Menos acessos ao disco | `BufferedReader`, `BufferedWriter`, `BufferedInputStream` |
| Ponte de bytes para texto | `InputStreamReader` com charset explícito |
| Arquivo pequeno inteiro | `Files.readString` (Java 11), `Files.readAllLines` |
| Arquivo grande, linha a linha | `Files.lines` (stream preguiçoso) ou `BufferedReader.readLine` |

```java
Path origem = Path.of("entrada.csv");
Path destino = Path.of("saida.csv");

try (BufferedReader in = Files.newBufferedReader(origem, StandardCharsets.UTF_8);
     BufferedWriter out = Files.newBufferedWriter(destino, StandardCharsets.UTF_8,
             StandardOpenOption.CREATE, StandardOpenOption.APPEND)) {
    String linha;
    while ((linha = in.readLine()) != null) {   // null indica fim do arquivo
        out.write(linha.trim());
        out.newLine();
    }
} catch (NoSuchFileException e) {
    System.err.println("Arquivo não encontrado: " + e.getFile());
} catch (IOException e) {
    throw new UncheckedIOException(e);
}
```

- `readLine()` devolve `null` no fim do arquivo; `read()` devolve `-1`.
- `IOException` é verificada. `FileNotFoundException` (`java.io`) e `NoSuchFileException` (`java.nio.file`) são subclasses dela.
- Escrita sobrescreve por padrão. `Files.write` e `Files.writeString` usam `CREATE`, `TRUNCATE_EXISTING` e `WRITE`. Para acrescentar, use `APPEND` ou `new FileWriter(arq, true)`.
- Os métodos de `Files` usam UTF-8 por padrão. `FileReader` e `FileWriter` usam o charset padrão da plataforma, que passou a ser UTF-8 no Java 18.
- `Files.readAllLines` carrega tudo na memória. `Files.lines` lê sob demanda e deve ser fechado com try-with-resources.
- Sem `close` ou `flush`, dados de um fluxo com buffer podem não chegar ao disco.

## 3.1.2.5 Estruturas de dados, algoritmos e pseudocódigo

O edital pede o conceito independente de linguagem: saber o comportamento de cada estrutura, o custo das operações e executar pseudocódigo à mão.

### Estruturas lineares

| Estrutura | Política | Operações | Em Java |
| --- | --- | --- | --- |
| Vetor (array) | acesso por índice | acesso O(1); inserir no meio O(n); tamanho fixo | `int[]`, `ArrayList` |
| Lista encadeada | nós ligados por referência | inserir com o nó em mãos O(1); busca O(n) | `LinkedList` |
| Pilha | LIFO: último a entrar, primeiro a sair | empilhar, desempilhar e topo em O(1) | `ArrayDeque` |
| Fila | FIFO: primeiro a entrar, primeiro a sair | enfileirar e desenfileirar em O(1) | `ArrayDeque`, `LinkedList` |
| Deque | inserção e remoção nas duas pontas | O(1) nas pontas | `ArrayDeque` |

Usos típicos: pilha para chamadas de método, desfazer e busca em profundidade. Fila para processamento em ordem de chegada e busca em largura.

### Tabela de dispersão (hash)

- A função de hash mapeia a chave para uma posição. Custo médio O(1) para inserir, buscar e remover.
- Chaves diferentes podem cair na mesma posição (colisão). Tratamentos: encadeamento ou endereçamento aberto.
- Com muitas colisões, o pior caso é O(n).
- `HashMap` usa encadeamento, capacidade inicial 16 e fator de carga 0,75. Ao ultrapassar o limite, redimensiona e redistribui.
- Desde o Java 8, listas de colisão longas viram árvores rubro-negras, o que limita o pior caso a O(log n).

### Árvores, heaps e grafos

- **Árvore binária de busca.** Menores à esquerda, maiores à direita. O percurso em ordem (esquerda, raiz, direita) visita as chaves em ordem crescente.
- Busca, inserção e remoção custam O(h), em que h é a altura. Inserir dados já ordenados degenera a árvore em lista: O(n).
- **Árvores balanceadas** (AVL, rubro-negra) mantêm altura O(log n). `TreeMap` e `TreeSet` são rubro-negras.
- **Percursos.** Pré-ordem: raiz, esquerda, direita. Em ordem: esquerda, raiz, direita. Pós-ordem: esquerda, direita, raiz.
- **Heap binário.** Árvore completa em que o pai é menor (min-heap) ou maior (max-heap) que os filhos. Topo em O(1); inserir e remover em O(log n). Base da `PriorityQueue`.
- **Árvore B.** Nós com muitas chaves, usada em índices de bancos de dados.
- **Grafo.** Vértices e arestas. Matriz de adjacência ocupa O(V²); lista de adjacência ocupa O(V + E).
- Busca em largura (BFS) usa fila e acha o menor caminho em número de arestas. Busca em profundidade (DFS) usa pilha ou recursão.

### Busca e ordenação

Busca linear custa O(n) e funciona em qualquer sequência. Busca binária custa O(log n) e **exige dados ordenados** com acesso por índice.

| Algoritmo | Melhor | Médio | Pior | Estável | Memória extra |
| --- | --- | --- | --- | --- | --- |
| Bubble sort | O(n), com parada antecipada | O(n²) | O(n²) | sim | O(1) |
| Selection sort | O(n²) | O(n²) | O(n²) | não | O(1) |
| Insertion sort | O(n) | O(n²) | O(n²) | sim | O(1) |
| Merge sort | O(n log n) | O(n log n) | O(n log n) | sim | O(n) |
| Quick sort | O(n log n) | O(n log n) | O(n²) | não | O(log n) de pilha |
| Heap sort | O(n log n) | O(n log n) | O(n log n) | não | O(1) |

- Ordenação estável preserva a ordem relativa de elementos com chaves iguais.
- O pior caso do quick sort aparece com pivô ruim, por exemplo o primeiro elemento em entrada já ordenada.
- Nenhuma ordenação por comparação faz melhor que O(n log n) no pior caso.
- Em Java, `Arrays.sort` com primitivos usa quicksort de duplo pivô (não estável). Com objetos e em `Collections.sort`, usa TimSort (estável).

Técnicas de projeto: divisão e conquista (merge sort, busca binária), algoritmo guloso (escolha local ótima), programação dinâmica (reaproveita subproblemas) e recursão, que precisa de caso-base.

### Pseudocódigo e teste de mesa

Antes de executar, confira três convenções do enunciado: se o índice começa em 0 ou 1, se o limite do `para` é inclusivo e se `div` é divisão inteira. O símbolo `←` é atribuição; `=` é comparação.

```
função busca(v[0..n-1], x)
    ini ← 0
    fim ← n - 1
    enquanto ini ≤ fim faça
        meio ← (ini + fim) div 2
        se v[meio] = x então
            retorne meio
        senão se v[meio] < x então
            ini ← meio + 1
        senão
            fim ← meio - 1
    fim-enquanto
    retorne -1
```

Teste de mesa para `v = [2, 5, 8, 12, 16, 23, 38]` e `x = 23`:

| Iteração | ini | fim | meio | v\[meio\] | Ação |
| --- | --- | --- | --- | --- | --- |
| 1 | 0 | 6 | 3 | 12 | 12 < 23, então ini ← 4 |
| 2 | 4 | 6 | 5 | 23 | encontrado, retorna 5 |

Variações que tornam o item errado:

- Trocar `ini ≤ fim` por `ini < fim` deixa de examinar o último candidato.
- Trocar `ini ← meio + 1` por `ini ← meio` pode gerar laço infinito.
- Aplicar o algoritmo a vetor não ordenado devolve resultado sem garantia.

## 3.1.2.6 Leitura, depuração e testes de borda

Este subitem dá nome ao tópico (“correção”): o edital quer que você leia um trecho, diga o que ele faz, localize o defeito e escolha os casos de teste que o revelam.

### Leitura de código

Método para itens com trecho de código:

1. Leia as declarações: tipos, valores iniciais e escopo de cada variável.
2. Monte uma tabela com uma coluna por variável e uma linha por passo.
3. Execute linha a linha, sem pular iterações, anotando cada mudança.
4. Confira a primeira e a última iteração de cada laço.
5. Só então compare com a afirmação do item.

Defeitos que o examinador costuma plantar:

| Defeito | Exemplo | Efeito |
| --- | --- | --- |
| Erro por um (off-by-one) | `i <= v.length` | `ArrayIndexOutOfBoundsException` na última iteração |
| Divisão inteira | `media = soma / n` com inteiros | perde a parte decimal |
| Comparação de referência | `s1 == s2` com `String` | `false` para conteúdos iguais |
| Nulo não tratado | `nome.length()` com `nome` nulo | `NullPointerException` |
| Falta de `break` | `switch` clássico | executa os casos seguintes |
| Atribuição no lugar de comparação | `if (ativo = false)` | compila com `boolean` e nunca entra |
| Ponto e vírgula solto | `for (...) ;` | corpo do laço vazio |
| Sombreamento | parâmetro com o mesmo nome do atributo, sem `this` | o atributo não é alterado |
| Estouro | `(ini + fim) / 2` com valores grandes | índice negativo |
| Resultado ignorado | `s.trim();` sem atribuição | `s` permanece igual |

### Depuração

Depurar é localizar a causa de um defeito já observado. Testar é revelar que o defeito existe. São atividades distintas.

Processo:

1. Reproduzir a falha de forma confiável, com a menor entrada possível.
2. Formular uma hipótese sobre a causa.
3. Isolar: reduzir o trecho suspeito, por exemplo por bissecção do código ou do histórico (`git bisect`).
4. Observar o estado real com depurador ou logs e comparar com o esperado.
5. Corrigir a causa, não o sintoma.
6. Escrever um teste que falhava antes e passa depois, para evitar regressão.

Recursos de um depurador:

- **Ponto de parada (breakpoint):** suspende a execução em uma linha. O condicional só para quando uma expressão é verdadeira.
- **Step over:** executa a linha inteira, sem entrar nos métodos chamados.
- **Step into:** entra no método chamado. **Step out:** termina o método atual e volta ao chamador.
- **Watch e inspeção:** mostram valores de variáveis e expressões no ponto de parada.
- **Pilha de chamadas:** mostra a sequência de métodos ativos.

Leitura de stack trace em Java: a primeira linha traz o tipo da exceção e a mensagem. O primeiro `at` é onde ela foi lançada; os seguintes são os chamadores. `Caused by` indica a exceção original, e a causa raiz é a última da cadeia.

### Testes de borda

Defeitos se concentram nos limites dos intervalos. Duas técnicas de caixa-preta trabalham juntas:

- **Partição de equivalência.** Divide as entradas em classes que o programa deve tratar do mesmo modo. Basta um representante por classe, inclusive das inválidas.
- **Análise de valor limite.** Testa as fronteiras de cada classe: o limite e seus vizinhos imediatos.

Exemplo: campo que aceita idade de 18 a 65, inclusive.

| Técnica | Valores de teste |
| --- | --- |
| Partição de equivalência | um menor que 18, um entre 18 e 65, um maior que 65 |
| Valor limite com dois valores por fronteira | 17, 18, 65, 66 |
| Valor limite com três valores por fronteira | 17, 18, 19, 64, 65, 66 |

```java
static boolean podeAposentar(int idade) {
    return idade > 65;   // requisito: "a partir de 65 anos"
}
```

Testar com 30 e 70 passa e não revela nada. Só o valor de borda 65 mostra que o correto é `>=`.

Bordas que valem para quase todo código:

- Coleção vazia, com um elemento, com duplicatas, já ordenada e em ordem inversa.
- `null`, texto vazio e texto só com espaços.
- Zero, negativos, `Integer.MAX_VALUE` e `Integer.MIN_VALUE`.
- Primeiro e último índice.
- Datas: 29 de fevereiro, virada de mês e de ano, fuso horário.
- Arquivo inexistente, vazio ou sem quebra de linha no final.

Em JUnit 5, `assertEquals(esperado, obtido)` recebe o valor esperado primeiro. `assertThrows` verifica exceções e `@ParameterizedTest` roda o mesmo teste para vários valores de borda.

## 3.1.2.7 Complexidade em decisões de desempenho

Complexidade mede como o custo cresce com o tamanho da entrada, não o tempo em segundos. O edital pede o uso prático: escolher estrutura e algoritmo com base nesse crescimento.

### Notação

- **O (O grande):** limite superior do crescimento. É a mais usada, em geral para o pior caso.
- **Ω (ômega):** limite inferior.
- **Θ (teta):** limite justo, superior e inferior ao mesmo tempo.
- Constantes e termos de menor ordem são descartados: 3n² + 10n + 5 é O(n²).
- Pior caso, caso médio e melhor caso são análises diferentes e não se confundem com O, Ω e Θ.

Ordem de crescimento, do melhor para o pior: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ), O(n!).

| Classe | Operações para n = 1.000.000 | Exemplo |
| --- | --- | --- |
| O(1) | 1 | acesso por índice, `HashMap.get` médio |
| O(log n) | cerca de 20 | busca binária, `TreeMap.get` |
| O(n) | 1.000.000 | busca linear, percorrer lista |
| O(n log n) | cerca de 20.000.000 | merge sort, TimSort |
| O(n²) | 1.000.000.000.000 | dois laços aninhados sobre a mesma entrada |

### Como calcular

- Instruções em sequência somam; prevalece o termo dominante.
- Laços aninhados multiplicam.
- Laço que divide o problema pela metade a cada passo é O(log n).
- Laço de n passos com trabalho O(log n) em cada um é O(n log n).
- Recursão que faz duas chamadas por nível sem reaproveitar resultados, como Fibonacci ingênuo, é exponencial.

```java
for (int i = 0; i < n; i++)          // n vezes
    for (int j = i; j < n; j++)      // n, n-1, ..., 1
        soma++;                      // total n(n+1)/2, logo O(n²)

for (int i = 1; i < n; i *= 2)       // 1, 2, 4, ... logo O(log n)
    soma++;
```

**Custo amortizado.** `ArrayList.add` no fim é O(1) amortizado: quase sempre barato, com um redimensionamento O(n) ocasional. A média por operação continua constante.

**Tempo e espaço.** Complexidade também mede memória. Merge sort troca O(n) de espaço extra por tempo garantido; cache e memoização trocam memória por tempo.

### Decisões típicas

| Situação | Escolha ruim | Escolha melhor |
| --- | --- | --- |
| Testar pertinência muitas vezes | `list.contains` em laço: O(n) cada, O(n²) no total | `HashSet.contains`: O(1) médio |
| Buscar por chave | percorrer lista de objetos | `HashMap` indexado pela chave |
| Manter dados ordenados ou consultar faixas | reordenar a lista a cada inserção | `TreeMap` ou `TreeSet`: O(log n) |
| Acesso por posição em laço | `LinkedList.get(i)`: O(n) cada | `ArrayList.get(i)`: O(1) |
| Montar texto em laço | `String` com `+`: quadrático | `StringBuilder.append`: linear |
| Muitas buscas em dados estáticos | busca linear repetida | ordenar uma vez e usar busca binária |
| Sempre retirar o menor ou o maior | ordenar a cada retirada | `PriorityQueue`: O(log n) por operação |
| Uma consulta ao banco por item de uma lista | n + 1 consultas | uma consulta com junção ou em lote |

### Limites da análise

- Big-O descreve tendência para entradas grandes. Para n pequeno, as constantes podem inverter o resultado.
- O(1) de hash é caso **médio**. Com função de hash ruim, o custo se degrada.
- `ArrayList` costuma superar `LinkedList` na prática, por localidade de memória, mesmo em inserções no meio.
- Otimize depois de medir. Um profiler mostra o gargalo real; otimização prematura piora a legibilidade sem ganho comprovado.
- Em sistemas reais, entrada e saída (rede, disco, banco) costumam pesar mais que o processamento em memória.

## 3.1.2.8 Modularidade e padrões de projeto aplicados à manutenção

O recorte do edital é a manutenção: como dividir o código para que uma mudança afete poucos lugares. A meta é **alta coesão e baixo acoplamento**.

### Modularidade

- **Coesão:** o quanto os elementos de um módulo servem a um mesmo propósito. Quanto maior, melhor.
- **Acoplamento:** o quanto um módulo depende de outros. Quanto menor, melhor.
- **Ocultação de informação:** o módulo expõe uma interface estável e esconde decisões internas.
- **Separação de responsabilidades:** apresentação, regras de negócio e acesso a dados em camadas distintas.
- **Inversão e injeção de dependências:** a classe recebe suas dependências prontas, por abstração, em vez de instanciá-las.
- Em Java, a modularidade aparece em classes, pacotes, artefatos (JAR) e módulos do JPMS (Java 9), com `module-info.java`, `exports` e `requires`.

Princípios SOLID:

| Letra | Princípio | Ideia |
| --- | --- | --- |
| S | Responsabilidade única | uma classe deve ter um só motivo para mudar |
| O | Aberto-fechado | aberta para extensão, fechada para modificação |
| L | Substituição de Liskov | o subtipo deve poder substituir o tipo base sem quebrar o programa |
| I | Segregação de interfaces | várias interfaces específicas em vez de uma geral |
| D | Inversão de dependência | depender de abstrações, não de implementações concretas |

Outros princípios: DRY (não repetir conhecimento), KISS (preferir o simples), YAGNI (não implementar o que ainda não é necessário) e Lei de Deméter (falar só com colaboradores diretos).

### Padrões de projeto GoF

São 23 padrões em três grupos.

| Grupo | Padrões |
| --- | --- |
| Criacionais (5) | Abstract Factory, Builder, Factory Method, Prototype, Singleton |
| Estruturais (7) | Adapter, Bridge, Composite, Decorator, Facade, Flyweight, Proxy |
| Comportamentais (11) | Chain of Responsibility, Command, Interpreter, Iterator, Mediator, Memento, Observer, State, Strategy, Template Method, Visitor |

Os mais cobrados e o ganho de manutenção de cada um:

| Padrão | Intenção | Ganho na manutenção | Exemplo em Java ou Spring |
| --- | --- | --- | --- |
| Strategy | encapsular algoritmos intercambiáveis | nova regra sem alterar o cliente; elimina cadeias de `if` | `Comparator` |
| Template Method | superclasse fixa o esqueleto; subclasses preenchem passos | fluxo comum em um só lugar | `AbstractList` |
| Observer | notificar dependentes sobre mudanças | emissor não conhece os receptores | eventos do Spring |
| Factory Method | delegar a criação a subclasses | cliente não depende da classe concreta | `Calendar.getInstance` |
| Abstract Factory | criar famílias de objetos relacionados | troca a família inteira de uma vez | fábricas de parsers XML |
| Builder | construir objeto complexo passo a passo | evita construtores com muitos parâmetros | `StringBuilder`, `HttpRequest.newBuilder` |
| Singleton | garantir instância única com acesso global | ponto único de controle; dificulta testes se usado em excesso | `Runtime.getRuntime` |
| Adapter | converter uma interface em outra esperada | integra código legado sem alterá-lo | `InputStreamReader` |
| Decorator | acrescentar responsabilidades dinamicamente | evita explosão de subclasses | `BufferedReader` sobre `Reader` |
| Facade | interface única e simples para um subsistema | clientes não dependem dos detalhes internos | fachadas de serviço |
| Proxy | substituto que controla o acesso ao objeto real | adiciona transação, cache ou segurança sem tocar na classe | proxies do `@Transactional` |
| Composite | tratar objeto e composição de modo uniforme | código cliente único para árvore e folha | menus, árvores de componentes |
| Chain of Responsibility | passar a requisição por uma cadeia de tratadores | etapas entram e saem sem afetar as demais | filtros do Spring Security |
| Command | encapsular uma requisição como objeto | permite fila, registro e desfazer | `Runnable` |
| Iterator | percorrer sem expor a estrutura interna | troca a coleção sem mudar o percurso | `Iterator` |

Pares que o examinador troca de propósito:

- **Strategy × State.** Estrutura parecida. No Strategy o cliente escolhe o algoritmo; no State o objeto muda de comportamento conforme seu estado interno.
- **Strategy × Template Method.** Strategy varia por composição; Template Method varia por herança.
- **Adapter × Facade.** Adapter converte uma interface existente; Facade cria uma interface nova e mais simples.
- **Decorator × Proxy.** Mesma interface nos dois. Decorator acrescenta comportamento; Proxy controla o acesso.
- **Factory Method × Abstract Factory.** O primeiro cria um produto por herança; o segundo cria famílias de produtos por composição.

### Padrão aplicado à manutenção

Código com cadeia de condicionais: cada novo tipo de cálculo exige abrir e alterar a classe, o que viola o princípio aberto-fechado.

```java
double calcular(String tipo, double valor) {
    if (tipo.equals("DIARIA")) return valor * 0.10;
    else if (tipo.equals("PASSAGEM")) return valor * 0.05;
    else return 0;
}
```

Com Strategy, cada regra fica em uma classe própria e o cliente depende só da interface:

```java
interface RegraGlosa { double calcular(double valor); }

class GlosaDiaria implements RegraGlosa {
    public double calcular(double valor) { return valor * 0.10; }
}
class GlosaPassagem implements RegraGlosa {
    public double calcular(double valor) { return valor * 0.05; }
}

class Auditoria {
    private final RegraGlosa regra;                 // dependência injetada
    Auditoria(RegraGlosa regra) { this.regra = regra; }
    double glosar(double valor) { return regra.calcular(valor); }
}
```

Uma nova regra vira uma nova classe, testável isoladamente, sem tocar em `Auditoria`.

### Refatoração e maus cheiros

Refatorar é melhorar a estrutura interna **sem alterar o comportamento externo**, com testes como rede de segurança.

- Sinais de problema: código duplicado, método longo, classe que faz tudo, lista longa de parâmetros.
- Cirurgia com espingarda: uma mudança exige editar muitas classes. Indica baixa coesão.
- Inveja de recursos: método que usa mais dados de outra classe do que da própria.
- Padrão não é obrigação. Aplicá-lo sem necessidade aumenta a complexidade e dificulta a manutenção.

## Revisão: pegadinhas de certo ou errado

As afirmações abaixo reúnem as trocas mais prováveis em cada subitem. Desconfie de “sempre”, “nunca”, “garante” e “qualquer”.

| Subitem | Afirmação | Gabarito | Por quê |
| --- | --- | --- | --- |
| 3.1.2.1 | Em Java, objetos são passados por referência, de modo que um método pode fazer a variável do chamador apontar para outro objeto. | Errado | A passagem é sempre por valor; copia-se a referência. |
| 3.1.2.1 | Uma variável local do tipo `int` não inicializada vale 0. | Errado | Só atributos têm valor padrão; o uso gera erro de compilação. |
| 3.1.2.1 | Dois métodos com mesmo nome e mesmos parâmetros, mas retornos diferentes, constituem sobrecarga válida. | Errado | O tipo de retorno não integra a assinatura. |
| 3.1.2.2 | A chamada a um método de instância sobrescrito é resolvida em tempo de execução, conforme o tipo do objeto. | Certo | Ligação dinâmica. |
| 3.1.2.2 | Métodos estáticos podem ser sobrescritos e têm comportamento polimórfico. | Errado | São ocultados e resolvidos pelo tipo da referência. |
| 3.1.2.2 | Uma interface pode conter métodos com implementação. | Certo | `default` e `static` desde o Java 8; `private` desde o Java 9. |
| 3.1.2.3 | `Map` é subinterface de `Collection`. | Errado | Hierarquia separada. |
| 3.1.2.3 | `List<Integer>` pode ser atribuída a uma variável do tipo `List<Number>`. | Errado | Genéricos são invariantes; seria preciso `List<? extends Number>`. |
| 3.1.2.3 | As informações de tipo genérico são verificadas na compilação e apagadas no bytecode. | Certo | Apagamento de tipos. |
| 3.1.2.4 | Se dois objetos têm o mesmo `hashCode`, então `equals` entre eles devolve `true`. | Errado | Colisões são permitidas; a implicação vale no sentido inverso. |
| 3.1.2.4 | O bloco `finally` não é executado quando o `try` contém `return`. | Errado | Executa antes de o método retornar. |
| 3.1.2.4 | Em try-with-resources, os recursos são fechados na ordem inversa à da declaração. | Certo | E antes do `catch` e do `finally`. |
| 3.1.2.4 | Declarar uma lista como `final` impede a inclusão de elementos. | Errado | `final` fixa a referência, não o conteúdo. |
| 3.1.2.5 | A busca binária localiza um elemento em O(log n) em qualquer vetor. | Errado | Exige vetor ordenado. |
| 3.1.2.5 | O quick sort tem complexidade O(n log n) no pior caso. | Errado | O pior caso é O(n²); O(n log n) é o caso médio. |
| 3.1.2.6 | Para um campo que aceita valores de 1 a 100, os valores 0, 1, 100 e 101 atendem à análise de valor limite. | Certo | Cada fronteira com um vizinho válido e um inválido. |
| 3.1.2.7 | Substituir buscas repetidas em `ArrayList` por `HashSet` reduz cada busca de O(n) para O(1) no caso médio. | Certo | No pior caso o hash não garante O(1). |
| 3.1.2.8 | Um bom projeto modular busca alto acoplamento e baixa coesão. | Errado | É o inverso. |
| 3.1.2.8 | O padrão Adapter fornece uma interface simplificada para um conjunto de classes de um subsistema. | Errado | Essa é a intenção do Facade. |
| 3.1.2.8 | Refatorar consiste em alterar o comportamento do sistema para corrigir defeitos. | Errado | Refatoração preserva o comportamento externo. |
