# Aula 3.1.3 – Concorrência e processamento

Nov 15, 2026 · @João

## Mapa do item

O item 3.1.3 é o tópico “3 Concorrência e processamento” do bloco “Engenharia de Software e Programação”. Ele tem quatro subitens e trata do que muda quando várias tarefas ou requisições executam ao mesmo tempo: o que pode dar errado, como proteger o estado e quanto isso custa em desempenho.

A correspondência abaixo usa a sua numeração hierárquica e o [edital verticalizado de Desenvolvimento de Sistemas](https://arquivos.voceconcursado.com.br/wp-content/uploads/2026/10/09190213/Edital-Verticalizado-cgu-2026-desenvolvimento-de-sistemas.pdf), montado a partir do [Edital CGU nº 1/2026](https://cdn.direcaoconcursos.com.br/uploads/2026/10/CGU-edital.pdf).

| Subitem | Conteúdo no edital | O que o Cebraspe tende a explorar |
| --- | --- | --- |
| 3.1.3.1 | Concorrência, paralelismo e assincronia | Troca entre os três conceitos, `start` × `run`, pools de threads, `CompletableFuture`, threads virtuais |
| 3.1.3.2 | Condições de corrida e sincronização | `i++` não atômico, `volatile` × `synchronized`, classes atômicas, coleções concorrentes |
| 3.1.3.3 | Compartilhamento de estado, bloqueios e falhas | Bean singleton com estado, deadlock e condições de Coffman, exceção em thread, saturação de pool |
| 3.1.3.4 | Correção e desempenho sob requisições concorrentes | Atualização perdida, níveis de isolamento, bloqueio otimista × pessimista, idempotência, testes de carga |

O edital é centrado em Java e Spring, então os exemplos seguem essa pilha. Os conceitos, porém, são cobrados também de forma independente de linguagem.

### Estratégias gerais da banca neste tema

- **Troca de par conceitual.** A definição de um conceito é atribuída ao seu vizinho: concorrência por paralelismo, deadlock por livelock, otimista por pessimista.
- **Garantia exagerada.** Palavras como “garante”, “elimina”, “sempre” e “qualquer” aplicadas a um mecanismo que resolve só parte do problema. Exemplo: “`volatile` garante atomicidade”.
- **Meia verdade no final.** Item longo e correto, com um detalhe errado na última oração. Leia até o ponto final.
- **Código curto com saída “determinística”.** O item afirma que o programa “sempre imprime” certo valor. Em código com corrida, a resposta correta quase nunca é “sempre”.
- **Mais é melhor.** Afirma-se que mais threads, mais isolamento ou mais sincronização melhoram tudo. Em concorrência, toda escolha tem custo.

Na P3 cada erro desconta 2 pontos. Em item de concorrência, pergunte primeiro: o que é compartilhado, quem altera e o que protege.

## 3.1.3.1 Concorrência, paralelismo e assincronia

São três ideias diferentes: concorrência é **lidar** com várias tarefas no mesmo período, paralelismo é **executar** várias no mesmo instante e assincronia é **não ficar parado** esperando um resultado. A banca cobra, antes de tudo, a capacidade de não confundi-las.

### Os três conceitos

| Conceito | Ideia | Precisa de vários núcleos? | Exemplo |
| --- | --- | --- | --- |
| Concorrência | tarefas em andamento no mesmo período, intercaladas | não | um núcleo alternando entre duas requisições |
| Paralelismo | tarefas executando no mesmo instante físico | sim | quatro núcleos processando quatro lotes de registros |
| Assincronia | quem chama não bloqueia; o resultado chega depois | não | JavaScript, com uma única thread e laço de eventos |

- Concorrência é propriedade da **estrutura** do programa; paralelismo é propriedade da **execução**.
- Todo paralelismo envolve concorrência, mas há concorrência sem paralelismo.
- Assincronia não exige múltiplas threads. Síncrono × assíncrono descreve **quem espera**; bloqueante × não bloqueante descreve **se a thread fica presa**.

### Processo e thread

|  | Processo | Thread |
| --- | --- | --- |
| Memória | espaço de endereçamento próprio | compartilha o heap do processo |
| Pilha | própria | própria, uma por thread |
| Custo de criação e troca de contexto | maior | menor |
| Isolamento | falha tende a ficar contida | falha pode afetar o processo inteiro |

Variáveis locais ficam na pilha e não são compartilhadas. Objetos ficam no heap e podem ser compartilhados: é aí que nascem os problemas dos próximos subitens.

### Threads em Java

```java
Thread t = new Thread(() -> System.out.println(Thread.currentThread().getName()));
t.run();    // executa na thread atual; nenhuma thread nova é criada
t.start();  // cria a nova thread, que executa run()
```

- `Runnable` não devolve valor nem lança exceção verificada. `Callable<V>` devolve valor e pode lançar.
- Chamar `start()` duas vezes na mesma thread lança `IllegalThreadStateException`.
- `join()` faz a thread atual esperar outra terminar.
- `sleep()` pausa a thread **sem liberar** os monitores que ela detém. `wait()` libera o monitor.
- `interrupt()` não mata a thread: apenas sinaliza. Métodos bloqueantes respondem com `InterruptedException`.
- A JVM só encerra quando terminam todas as threads não daemon.
- A ordem de execução entre threads não é garantida; depende do escalonador.

Estados de `Thread.State`: `NEW`, `RUNNABLE`, `BLOCKED` (esperando um monitor), `WAITING` (espera sem prazo: `wait`, `join`), `TIMED_WAITING` (espera com prazo: `sleep`) e `TERMINATED`. Não existe estado `RUNNING` na enumeração: `RUNNABLE` cobre pronta e em execução.

### Executores e pools

Criar uma thread por tarefa é caro e sem limite. O `ExecutorService` separa a tarefa de quem a executa e reaproveita threads.

| Fábrica | Comportamento |
| --- | --- |
| `newFixedThreadPool(n)` | n threads fixas; fila sem limite |
| `newCachedThreadPool()` | cria threads sob demanda, sem limite; reaproveita ociosas |
| `newSingleThreadExecutor()` | uma thread; tarefas em ordem |
| `newScheduledThreadPool(n)` | execução agendada ou periódica |
| `ForkJoinPool` | divisão e conquista com roubo de trabalho (work stealing) |
| `newVirtualThreadPerTaskExecutor()` | uma thread virtual por tarefa (Java 21) |

```java
ExecutorService pool = Executors.newFixedThreadPool(4);
Future<Integer> f = pool.submit(() -> calcular());  // Callable
Integer r = f.get();                                 // bloqueia até o resultado
pool.shutdown();                                     // conclui as pendentes e encerra
```

- `execute` recebe `Runnable` e não devolve nada. `submit` devolve um `Future`.
- `Future.get()` **bloqueia**. Usar `Future` não torna o código não bloqueante.
- `shutdown()` deixa de aceitar tarefas e conclui as já enviadas. `shutdownNow()` tenta interromper as que estão em execução e devolve as que estavam na fila.

### Assincronia com CompletableFuture

```java
CompletableFuture<String> nome = CompletableFuture
        .supplyAsync(() -> buscarOrgao(id))     // executa em outra thread
        .thenApply(Orgao::nome)                 // transforma o resultado
        .exceptionally(e -> "indisponível");    // trata a falha
```

- Compõe etapas sem bloquear: `thenApply` transforma, `thenCompose` encadeia outra operação assíncrona, `thenCombine` junta dois resultados, `allOf` espera vários.
- Sem executor informado, usa o `ForkJoinPool.commonPool()`.
- `join()` e `get()` continuam sendo bloqueantes.

### Threads virtuais e streams paralelos

- **Threads virtuais** (Java 21) são leves, gerenciadas pela JVM e montadas sobre poucas threads de plataforma. Quando bloqueiam em entrada e saída, liberam a thread portadora.
- Servem para muitas tarefas que **esperam** (I/O). Não aceleram cálculo: o número de núcleos é o mesmo.
- Não devem ser colocadas em pool; cria-se uma por tarefa.
- **Streams paralelos** usam o pool comum e ajudam em cálculo sobre muitos dados, com operações sem estado. Com poucos dados, I/O ou estado compartilhado, pioram o resultado.

### Tarefa de CPU × tarefa de I/O

| Tipo | Gargalo | Tamanho do pool | Mais threads ajudam? |
| --- | --- | --- | --- |
| Limitada por CPU | processador | próximo ao número de núcleos | não; aumentam a troca de contexto |
| Limitada por I/O | espera por rede, disco ou banco | maior que o número de núcleos | sim, até o limite do recurso externo |

**Lei de Amdahl.** O ganho máximo com paralelismo é limitado pela parte sequencial do programa.

```latex
S(n) = \frac{1}{(1 - p) + \frac{p}{n}}
```

Com 90% do programa paralelizável (p = 0,9), quatro núcleos dão ganho de cerca de 3,1 vezes, e infinitos núcleos não passam de 10 vezes.

### Na web

- **Modelo servlet (Spring MVC):** uma thread do pool por requisição, bloqueada enquanto espera o banco. Simples, mas limitado pelo tamanho do pool.
- **Modelo reativo (Spring WebFlux):** poucas threads em laço de eventos, com I/O não bloqueante. Uma chamada bloqueante dentro dele trava as demais requisições.
- **`@Async`:** executa o método em outra thread. Exige `@EnableAsync` e funciona por proxy: chamada feita de dentro da própria classe não passa pelo proxy e roda de forma síncrona.
- **JavaScript:** uma thread e laço de eventos. `async/await` é açúcar sintático sobre `Promise`; `await` suspende a função, não a thread. Microtarefas (promises) executam antes da próxima macrotarefa (`setTimeout`).

### Pegadinhas e estratégias da banca

| Como o item vem | Por que está errado |
| --- | --- |
| “Concorrência exige processador com múltiplos núcleos.” | Isso é paralelismo; concorrência ocorre por intercalação em um núcleo. |
| “Programar de forma assíncrona implica criar múltiplas threads.” | Laço de eventos faz assincronia com uma thread. |
| “O método `run()` inicia uma nova thread.” | Quem inicia é `start()`. |
| “Dobrar o número de threads dobra a vazão.” | Depende do gargalo; há troca de contexto, contenção e Lei de Amdahl. |
| “Threads virtuais aumentam o desempenho de tarefas de cálculo intensivo.” | O ganho é em tarefas que esperam I/O. |
| “`Future.get()` devolve o resultado sem bloquear.” | Bloqueia até a tarefa terminar. |
| “`sleep()` libera o bloqueio mantido pela thread.” | Quem libera o monitor é `wait()`. |
| “Streams paralelos são sempre mais rápidos que os sequenciais.” | Há custo de divisão e junção; com poucos dados, perdem. |

Estratégia: ao ler “concorrente”, “paralelo” ou “assíncrono”, troque mentalmente a palavra pelas outras duas. Se a frase só faz sentido com outra, o item está errado.

## 3.1.3.2 Condições de corrida e sincronização

Há condição de corrida quando o resultado depende da ordem em que as threads se intercalam. A correção exige três garantias — atomicidade, visibilidade e ordenação — e cada mecanismo de sincronização entrega um subconjunto delas.

### O problema

```java
class Contador {
    private int valor;
    void incrementar() { valor++; }   // três passos: ler, somar, gravar
}
```

Duas threads chamam `incrementar()` mil vezes cada. O resultado pode ser 2000, mas pode ser menor: não há garantia.

| Passo | Thread A | Thread B | valor |
| --- | --- | --- | --- |
| 1 | lê 5 |  | 5 |
| 2 |  | lê 5 | 5 |
| 3 | grava 6 |  | 6 |
| 4 |  | grava 6 | 6 |

Houve dois incrementos e o valor subiu um. É a **atualização perdida**.

Os dois padrões de corrida:

- **Ler-modificar-gravar:** `valor++`, `saldo = saldo - x`.
- **Verificar-depois-agir:** `if (!existe(k)) inserir(k)`. Entre a verificação e a ação, outra thread muda a condição.

Vocabulário:

- **Seção crítica:** trecho que acessa estado compartilhado e não pode ser executado por duas threads ao mesmo tempo.
- **Exclusão mútua:** garantia de que só uma thread por vez entra na seção crítica.
- **Corrida de dados (data race):** duas threads acessam a mesma variável, ao menos uma grava, sem sincronização. É um caso particular de condição de corrida.
- **Thread-safe:** a classe se comporta corretamente sob acesso concorrente, sem coordenação adicional de quem a usa.

### As três garantias

| Garantia | Significado | Falha típica |
| --- | --- | --- |
| Atomicidade | a operação composta acontece inteira ou não acontece, sem intercalação | atualização perdida em `valor++` |
| Visibilidade | a escrita de uma thread é vista pelas outras | thread nunca enxerga `parar = true` e fica em laço |
| Ordenação | as instruções não são vistas fora de ordem por outras threads | objeto publicado antes de ser totalmente construído |

O modelo de memória do Java define essas garantias pela relação **acontece-antes** (happens-before). Sem ela, o compilador e o processador podem reordenar instruções e manter valores em cache.

### synchronized

Usa o monitor (bloqueio intrínseco) de um objeto. Garante exclusão mútua **e** visibilidade.

| Forma | Objeto bloqueado |
| --- | --- |
| método de instância `synchronized` | `this` |
| método `static synchronized` | o objeto `Class` da classe |
| bloco `synchronized (obj) { }` | o objeto indicado |

- É **reentrante**: a thread que já detém o monitor pode adquiri-lo de novo sem travar.
- Dois métodos sincronizados de instância do **mesmo objeto** disputam o mesmo monitor.
- Instâncias diferentes têm monitores diferentes: sincronizar em `this` não protege um atributo estático.
- Método estático e método de instância sincronizados **não** se excluem; os monitores são distintos.
- O monitor é liberado ao sair do bloco, inclusive por exceção.
- Métodos não sincronizados da mesma classe continuam acessíveis a outras threads.

### volatile

Garante visibilidade e ordenação para leituras e escritas da variável. **Não** garante atomicidade de operações compostas.

- Uso adequado: sinalizador simples, gravado por uma thread e lido por outras (`volatile boolean parar`).
- `volatile int valor; valor++` continua com corrida.
- Não bloqueia nenhuma thread.

### Classes atômicas

`AtomicInteger`, `AtomicLong` e `AtomicReference` oferecem operações compostas atômicas sem bloqueio, por **CAS** (comparar e trocar).

```java
AtomicInteger valor = new AtomicInteger();
valor.incrementAndGet();           // atômico, sem lock
valor.compareAndSet(5, 6);         // grava 6 somente se ainda for 5
```

- CAS é abordagem **otimista**: tenta, e repete se outra thread alterou o valor no meio.
- Cada operação é atômica isoladamente. Duas chamadas em sequência não formam uma operação atômica.
- `LongAdder` rende mais que `AtomicLong` sob alta disputa de escrita.

### Bloqueios explícitos

```java
private final ReentrantLock lock = new ReentrantLock();

void debitar(BigDecimal v) {
    lock.lock();
    try {
        saldo = saldo.subtract(v);
    } finally {
        lock.unlock();   // obrigatório: a liberação não é automática
    }
}
```

| Recurso | `synchronized` | `ReentrantLock` |
| --- | --- | --- |
| Liberação | automática | manual, no `finally` |
| Tentar sem esperar, ou com prazo | não | `tryLock()` |
| Espera interrompível | não | `lockInterruptibly()` |
| Política justa (ordem de chegada) | não | opcional |
| Várias condições de espera | uma por monitor | várias (`Condition`) |

- `ReadWriteLock`: vários leitores simultâneos **ou** um único escritor. Compensa quando há muito mais leitura que escrita.
- `StampedLock`: acrescenta leitura otimista, sem bloqueio, validada depois.

### Coordenação entre threads

- `wait()`, `notify()` e `notifyAll()` são métodos de `Object` e só podem ser chamados por quem detém o monitor. Fora disso: `IllegalMonitorStateException`.
- `wait()` deve ficar dentro de um `while`, e não de um `if`, por causa dos despertares espúrios.
- `notify()` acorda uma thread qualquer; `notifyAll()` acorda todas.

| Sincronizador | Para que serve | Reutilizável? |
| --- | --- | --- |
| `Semaphore` | limitar a N acessos simultâneos | sim |
| `CountDownLatch` | esperar N eventos terminarem | não |
| `CyclicBarrier` | N threads esperam umas pelas outras em um ponto | sim |
| `BlockingQueue` | produtor e consumidor; bloqueia se cheia ou vazia | sim |

### Coleções concorrentes

| Coleção | Característica |
| --- | --- |
| `ConcurrentHashMap` | leituras sem bloqueio, escrita com bloqueio fino; não aceita chave nem valor nulos; iterador não lança `ConcurrentModificationException` |
| `CopyOnWriteArrayList` | cada escrita copia o array; boa para muita leitura e pouca escrita |
| `Collections.synchronizedList` | um único bloqueio para tudo; a iteração exige sincronização manual |
| `ConcurrentLinkedQueue` | fila sem bloqueio |

```java
if (!mapa.containsKey(k)) {          // corrida, mesmo com ConcurrentHashMap
    mapa.put(k, criar());
}

mapa.computeIfAbsent(k, c -> criar());   // atômico
```

Coleção thread-safe garante cada operação isolada. A **composição** de operações continua sendo responsabilidade de quem usa; para isso existem `putIfAbsent`, `computeIfAbsent`, `compute` e `merge`.

`HashMap` comum, alterado por várias threads, pode perder entradas e corromper a estrutura interna.

### Pegadinhas e estratégias da banca

| Como o item vem | Por que está errado |
| --- | --- |
| “Declarar o contador como `volatile` torna o incremento seguro.” | `volatile` dá visibilidade, não atomicidade. |
| “O programa sempre imprime 2000.” | Com corrida, o valor pode ser menor; nada é garantido. |
| “Métodos sincronizados de instância e estáticos compartilham o mesmo bloqueio.” | Um usa `this`; o outro, o objeto `Class`. |
| “Uma thread que já detém o monitor entra em deadlock ao chamar outro método sincronizado do mesmo objeto.” | `synchronized` é reentrante. |
| “Com `ConcurrentHashMap`, a sequência verificar e inserir é atômica.” | Só cada chamada é atômica; use `computeIfAbsent`. |
| “`ReentrantLock` é liberado automaticamente ao fim do método.” | Exige `unlock()` no `finally`. |
| “`CountDownLatch` pode ser reiniciado para novo uso.” | Não pode; `CyclicBarrier` pode. |
| “`wait()` pode ser chamado em qualquer ponto do código.” | Só com o monitor adquirido. |
| “Sincronizar todos os métodos elimina qualquer problema de concorrência.” | Operações compostas entre chamadas continuam em corrida, e surge risco de deadlock. |

Estratégia: em item com mecanismo de sincronização, pergunte qual das três garantias o cenário exige e se o mecanismo citado a entrega. `volatile` cobre visibilidade; atômicas cobrem uma operação; bloqueio cobre um trecho inteiro.

## 3.1.3.3 Compartilhamento de estado, bloqueios e falhas

Só existe problema de concorrência quando há estado **mutável e compartilhado**; retire um dos dois adjetivos e o problema desaparece. Quando o compartilhamento é inevitável, entram os bloqueios, e com eles novos modos de falha.

### Três estratégias para o estado

| Estratégia | Como | Exemplo |
| --- | --- | --- |
| Não compartilhar (confinamento) | cada thread usa seus próprios dados | variáveis locais, parâmetros, `ThreadLocal` |
| Não alterar (imutabilidade) | objetos que não mudam depois de criados | `String`, `record`, `LocalDate`, `List.of` |
| Sincronizar | proteger cada acesso ao estado mutável | `synchronized`, bloqueios, atômicas, coleções concorrentes |

A ordem da tabela é a ordem de preferência. Sincronizar é o último recurso, porque custa desempenho e abre espaço para deadlock.

- Objeto **sem estado** (stateless) é sempre thread-safe.
- Objeto imutável é thread-safe sem sincronização.
- `SimpleDateFormat` não é thread-safe; `DateTimeFormatter` é imutável e pode ser compartilhado.

### Estado em aplicações Spring

Beans do Spring são **singleton** por padrão: uma única instância atende a todas as requisições, em várias threads.

```java
@Service
class RelatorioService {
    private final List<String> linhas = new ArrayList<>();   // compartilhado entre requisições

    List<String> gerar(Long orgaoId) {
        linhas.clear();                   // uma requisição apaga os dados da outra
        linhas.addAll(consultar(orgaoId));
        return linhas;
    }
}
```

O defeito só aparece com acessos simultâneos: um usuário recebe dados de outro. A correção é transformar o atributo em variável local.

- Controladores, serviços e repositórios devem ser sem estado mutável. Dependências injetadas e `final` não são problema.
- Outros escopos: `prototype` (nova instância a cada injeção), `request` e `session`.
- O Spring **não** torna o bean thread-safe; apenas gerencia seu ciclo de vida.
- O `EntityManager` injetado é um proxy que entrega um contexto de persistência por transação; a entidade em si não deve ser compartilhada entre threads.

**ThreadLocal** guarda um valor por thread. O Spring Security usa esse recurso no `SecurityContextHolder`, e o controle de transações também.

- Em pools, a thread é reaproveitada: valor não removido vaza para a próxima requisição. Chame `remove()` no `finally`.
- O valor não acompanha a tarefa quando ela muda de thread. Em método `@Async`, o contexto de segurança e a transação não se propagam por padrão.

**Várias instâncias da aplicação.** `synchronized` e os bloqueios do Java valem dentro de uma JVM. Com duas réplicas atrás de um balanceador, não protegem nada entre elas. Aí a proteção precisa estar no banco de dados (transação, restrição de unicidade, bloqueio de linha) ou em bloqueio distribuído, e a sessão deve ficar em armazenamento externo.

### Deadlock

Duas ou mais threads esperam, cada uma, um recurso que a outra detém. Nenhuma avança.

```java
// Thread 1                          // Thread 2
synchronized (contaA) {              synchronized (contaB) {
    synchronized (contaB) { }            synchronized (contaA) { }
}                                    }
```

As quatro condições de Coffman, **todas necessárias** ao mesmo tempo:

| Condição | Significado | Como quebrar |
| --- | --- | --- |
| Exclusão mútua | o recurso só serve a uma thread por vez | usar estruturas sem bloqueio ou dados imutáveis |
| Posse e espera | a thread segura um recurso enquanto pede outro | adquirir tudo de uma vez |
| Não preempção | o recurso não pode ser tomado à força | `tryLock` com prazo e desistência |
| Espera circular | A espera B, que espera A | ordem global de aquisição dos bloqueios |

- Basta eliminar **uma** das quatro para o deadlock ser impossível.
- A técnica mais usada é a ordem fixa: sempre bloquear a conta de menor identificador primeiro.
- A JVM **não** resolve deadlock de `synchronized` sozinha. O diagnóstico é feito por thread dump (`jstack`, `jcmd`), que aponta o ciclo.
- Bancos de dados detectam deadlock entre transações e abortam uma delas, a vítima, que deve ser repetida.
- Chamar código externo ou fazer I/O com o bloqueio retido aumenta o risco e a contenção.

### Outros problemas de progresso

| Problema | As threads estão | O que acontece |
| --- | --- | --- |
| Deadlock | bloqueadas | espera circular; ninguém avança |
| Livelock | ativas | reagem uma à outra sem parar e não progridem |
| Inanição (starvation) | uma delas, pronta | nunca recebe o recurso, tomado sempre por outras |
| Contenção | esperando em fila | muitas disputam o mesmo bloqueio; a vazão cai |

- Bloqueio justo (`new ReentrantLock(true)`) atende por ordem de chegada e evita inanição, com perda de vazão.
- Bloqueio de granularidade fina reduz a contenção, mas aumenta a complexidade e o risco de deadlock.
- **Segurança** (safety): nada de errado acontece. **Vivacidade** (liveness): algo de útil acaba acontecendo. Corrida viola a primeira; deadlock viola a segunda.

### Falhas em código concorrente

- A exceção não tratada encerra apenas a thread em que ocorreu. Ela **não** se propaga para a thread que a criou.
- Com `execute`, a exceção vai para o `UncaughtExceptionHandler`, e o pool repõe a thread.
- Com `submit`, a exceção fica guardada no `Future` e só aparece em `get()`, embrulhada em `ExecutionException`. Sem chamar `get()`, a falha passa em silêncio.
- Em `CompletableFuture`, a falha percorre a cadeia até `exceptionally` ou `handle`.
- Exceção dentro de `synchronized` libera o monitor, mas pode deixar o objeto em estado inconsistente.
- `InterruptedException` não deve ser engolida: propague, ou restaure o sinal com `Thread.currentThread().interrupt()`.

### Saturação de pools

O `ThreadPoolExecutor` trata uma tarefa nova nesta ordem:

1. Se há menos threads que o tamanho básico (core), cria uma thread.
2. Senão, coloca a tarefa na fila.
3. Se a fila está cheia e há menos threads que o máximo, cria uma thread.
4. Senão, rejeita.

| Política de rejeição | Efeito |
| --- | --- |
| `AbortPolicy` (padrão) | lança `RejectedExecutionException` |
| `CallerRunsPolicy` | quem enviou executa a tarefa, o que freia a entrada |
| `DiscardPolicy` | descarta em silêncio |
| `DiscardOldestPolicy` | descarta a mais antiga da fila |

- Com fila sem limite, o pool nunca passa do tamanho básico, e a fila pode crescer até esgotar a memória.
- Todas as threads presas esperando um serviço lento tornam a aplicação inteira indisponível. Defesas: prazo limite (timeout) em toda chamada externa, filas limitadas, pools separados por dependência (bulkhead) e disjuntor (circuit breaker).
- Tarefa que espera outra tarefa do **mesmo** pool pode travar o pool inteiro.

### Pegadinhas e estratégias da banca

| Como o item vem | Por que está errado |
| --- | --- |
| “Por serem gerenciados pelo contêiner, os beans singleton do Spring são thread-safe.” | O Spring não sincroniza nada; atributo mutável é compartilhado. |
| “O deadlock ocorre quando qualquer uma das quatro condições está presente.” | As quatro precisam ocorrer juntas. |
| “No livelock, as threads ficam bloqueadas indefinidamente.” | No livelock, estão ativas; bloqueadas é deadlock. |
| “A exceção lançada em uma thread filha é propagada à thread que a criou.” | Encerra só a própria thread. |
| “O pool cria threads até o máximo antes de enfileirar tarefas.” | Enfileira depois do tamanho básico; só cresce com a fila cheia. |
| “O uso de `synchronized` garante exclusão mútua entre as instâncias da aplicação em um cluster.” | Vale apenas dentro de uma JVM. |
| “A JVM detecta e desfaz automaticamente os deadlocks.” | Quem aborta a vítima é o banco de dados, entre transações. |
| “Variáveis locais precisam de sincronização por serem acessadas por várias threads.” | Ficam na pilha de cada thread. |

Estratégia: em cenário de aplicação web, procure o atributo de instância mutável em um bean. Em cenário de travamento, pergunte se as threads estão paradas (deadlock) ou trabalhando sem avançar (livelock).

## 3.1.3.4 Correção e desempenho sob requisições concorrentes

Em uma aplicação web, a concorrência que mais causa defeitos não está nas threads, e sim nos **dados**: duas requisições leem o mesmo registro, decidem com base nele e gravam. Proteger isso custa vazão, e o subitem pede o equilíbrio entre as duas coisas.

### O cenário

- Cada requisição HTTP é atendida por uma thread do pool do servidor. No Tomcat embutido do Spring Boot, o padrão é de 200 threads.
- Cada transação ocupa uma conexão do pool de conexões. No HikariCP, o padrão é de 10 conexões.
- Com várias instâncias da aplicação, o único ponto comum é o banco de dados. É nele que a correção precisa ser garantida.

### Atualização perdida

Duas requisições debitam 80 de uma conta com saldo 100.

| Passo | Requisição A | Requisição B | Saldo no banco |
| --- | --- | --- | --- |
| 1 | lê saldo 100 |  | 100 |
| 2 |  | lê saldo 100 | 100 |
| 3 | verifica 100 ≥ 80 e grava 20 |  | 20 |
| 4 |  | verifica 100 ≥ 80 e grava 20 | 20 |

Foram debitados 160 de uma conta com 100, e o saldo final é 20. O `synchronized` não resolve se houver mais de uma instância, e o nível de isolamento padrão da maioria dos bancos também não impede.

### Anomalias e níveis de isolamento

| Anomalia | O que acontece |
| --- | --- |
| Leitura suja | lê dado gravado por transação ainda não confirmada |
| Leitura não repetível | relê a mesma linha e encontra outro valor, confirmado por outra transação |
| Leitura fantasma | repete a consulta e surgem ou somem **linhas** que atendem ao filtro |
| Atualização perdida | duas transações leem, e a gravação de uma sobrescreve a da outra |

Pelo padrão SQL:

| Nível | Leitura suja | Leitura não repetível | Leitura fantasma |
| --- | --- | --- | --- |
| READ UNCOMMITTED | possível | possível | possível |
| READ COMMITTED | impedida | possível | possível |
| REPEATABLE READ | impedida | impedida | possível |
| SERIALIZABLE | impedida | impedida | impedida |

- Quanto maior o isolamento, maior a correção e **menor** a vazão: mais bloqueios ou mais transações abortadas.
- Padrão em PostgreSQL, Oracle e SQL Server: READ COMMITTED. Em MySQL com InnoDB: REPEATABLE READ.
- As implementações variam. No PostgreSQL, READ UNCOMMITTED se comporta como READ COMMITTED, e REPEATABLE READ também evita fantasmas.
- Não repetível diz respeito a uma linha que **mudou**. Fantasma diz respeito ao **conjunto** de linhas.

### Bloqueio otimista e pessimista

|  | Otimista | Pessimista |
| --- | --- | --- |
| Hipótese | conflito é raro | conflito é frequente |
| Mecanismo | coluna de versão conferida na gravação | bloqueio da linha na leitura (`SELECT ... FOR UPDATE`) |
| Bloqueia outras transações? | não | sim, até o fim da transação |
| No conflito | a segunda gravação falha e deve ser repetida | a segunda transação espera |
| Em JPA | `@Version` | `LockModeType.PESSIMISTIC_WRITE` |
| Risco | muitas repetições sob alta disputa | espera, deadlock, queda de vazão |

```java
@Entity
class Conta {
    @Id Long id;
    BigDecimal saldo;
    @Version Long versao;   // conferida e incrementada a cada UPDATE
}
```

O JPA gera `UPDATE conta SET saldo = ?, versao = 6 WHERE id = ? AND versao = 5`. Se nenhuma linha for alterada, lança `OptimisticLockException`.

Bloqueio otimista **detecta** o conflito na gravação; não o impede. Ele serve bem a telas de edição, em que o usuário lê agora e grava minutos depois.

### Outras técnicas de correção

**Atualização atômica no banco.** A verificação e a alteração vão na mesma instrução:

```sql
UPDATE conta
   SET saldo = saldo - 80
 WHERE id = 1 AND saldo >= 80;
```

A aplicação confere o número de linhas afetadas. Zero significa saldo insuficiente.

**Restrição de unicidade.** “Verificar se o CPF existe e depois inserir” é verificar-depois-agir: duas requisições passam pela verificação. A garantia real é a restrição `UNIQUE` no banco, com tratamento do erro de violação.

**Idempotência.** Operação idempotente produz o mesmo estado final se executada uma ou várias vezes.

| Método HTTP | Seguro (não altera estado) | Idempotente |
| --- | --- | --- |
| GET, HEAD, OPTIONS | sim | sim |
| PUT | não | sim |
| DELETE | não | sim |
| POST | não | não |
| PATCH | não | não necessariamente |

- Repetições acontecem: clique duplo, reenvio automático após timeout, reentrega de mensagem.
- Para tornar um POST seguro contra repetição, usa-se **chave de idempotência**: o cliente envia um identificador único, e o servidor reconhece a repetição e devolve a mesma resposta.
- Idempotente não é o mesmo que seguro: DELETE altera o estado, mas repeti-lo não muda o resultado final.

**Transações no Spring.** `@Transactional` usa o isolamento padrão do banco, salvo configuração em `isolation`. Por padrão, desfaz a transação em `RuntimeException` e `Error`, e não em exceção verificada. Funciona por proxy: chamada interna à própria classe não abre transação.

### Desempenho

| Medida | Significado |
| --- | --- |
| Latência | tempo de resposta de uma requisição |
| Vazão (throughput) | requisições atendidas por unidade de tempo |
| Percentis (p95, p99) | tempo abaixo do qual ficam 95% ou 99% das requisições |
| Saturação | grau de ocupação de um recurso: threads, conexões, CPU |

- A **média** esconde a cauda. Um p99 alto com média baixa indica que parte dos usuários tem experiência ruim.
- Latência e vazão não são a mesma coisa. Perto da saturação, a vazão estabiliza e a latência dispara, por causa das filas.
- O gargalo costuma estar no recurso mais escasso: em geral o pool de conexões, não o de threads.
- Transação longa segura conexão e bloqueios. Não se faz chamada a serviço externo dentro de transação.
- Aumentar o pool de conexões além do que o banco suporta **piora** o desempenho.
- Escala horizontal exige aplicação sem estado, com sessão e cache fora da instância.

Proteções sob carga:

| Técnica | O que faz |
| --- | --- |
| Prazo limite (timeout) | não espera indefinidamente por uma dependência |
| Limite de taxa (rate limiting) | restringe requisições por cliente, respondendo 429 |
| Contrapressão (backpressure) | o consumidor sinaliza ao produtor para reduzir o ritmo |
| Disjuntor (circuit breaker) | para de chamar uma dependência que está falhando |
| Compartimentos (bulkhead) | isola recursos para que uma dependência lenta não derrube tudo |
| Fila com processamento assíncrono | absorve picos e responde depois |
| Cache | evita repetir leitura cara, com risco de dado desatualizado |

Repetir automaticamente uma operação que falhou só é seguro se ela for idempotente, e as repetições devem ter intervalo crescente para não agravar a sobrecarga.

### Como testar

| Teste | Objetivo |
| --- | --- |
| Carga | comportamento sob o volume esperado |
| Estresse | ponto de ruptura, acima do esperado, e como o sistema se recupera |
| Pico (spike) | aumento súbito de volume |
| Resistência (soak) | carga mantida por longo período; revela vazamentos |

- Defeito de concorrência é **não determinístico**: pode não aparecer em mil execuções e surgir em produção.
- Teste que passa não prova ausência de corrida. A correção vem do projeto: estado confinado, operações atômicas, restrições no banco.
- Teste unitário com uma thread não exercita concorrência.
- Ferramentas de carga citadas em provas: JMeter, Gatling e k6. Para diagnóstico: thread dump e perfiladores.

### Pegadinhas e estratégias da banca

| Como o item vem | Por que está errado |
| --- | --- |
| “O bloqueio otimista impede que duas transações leiam o mesmo registro.” | Não bloqueia leitura; detecta o conflito ao gravar. |
| “O bloqueio pessimista é indicado quando os conflitos são raros.” | É o inverso; com conflito raro, usa-se o otimista. |
| “No nível READ COMMITTED não ocorrem leituras não repetíveis.” | Ele impede só a leitura suja. |
| “A leitura fantasma ocorre quando a transação lê dado não confirmado.” | Isso é leitura suja. |
| “O método POST é idempotente.” | Não é; PUT e DELETE são. |
| “Por alterar o estado do servidor, o método DELETE não é idempotente.” | Confunde idempotente com seguro. |
| “Elevar o isolamento para SERIALIZABLE aumenta a correção e a vazão.” | Aumenta a correção e reduz a vazão. |
| “O tempo médio de resposta é suficiente para avaliar o desempenho sob carga.” | É preciso olhar percentis. |
| “Teste de estresse avalia o sistema sob a carga normal esperada.” | Isso é teste de carga. |
| “A verificação prévia de existência na aplicação garante que não haverá registros duplicados.” | É verificar-depois-agir; a garantia é a restrição `UNIQUE`. |

Estratégia: em cenário com duas requisições e um registro, localize a leitura, a decisão e a gravação. Se houver intervalo entre elas sem bloqueio, versão ou instrução atômica, há corrida.

## Questões de fixação

Julgue cada afirmação como certa ou errada antes de consultar o gabarito, que vem logo depois. São cinco por subitem.

**3.1.3.1 Concorrência, paralelismo e assincronia**

1. Em um computador com um único núcleo de processamento, é possível haver execução concorrente de tarefas, mas não execução paralela.
2. Em Java, a invocação direta do método `run()` de um objeto `Thread` faz que o código seja executado na thread chamadora, sem a criação de nova thread.
3. De acordo com a Lei de Amdahl, se 80% de um programa for paralelizável, o ganho máximo de desempenho será de 5 vezes, qualquer que seja o número de núcleos.
4. As threads virtuais, disponíveis a partir do Java 21, são mapeadas uma a uma para threads do sistema operacional, o que as torna indicadas para tarefas de uso intensivo de CPU.
5. Em JavaScript, a palavra-chave `await` bloqueia a thread principal até que a promise seja resolvida.

**3.1.3.2 Condições de corrida e sincronização**

6. Um contador do tipo `AtomicInteger`, incrementado por várias threads com `incrementAndGet()`, dispensa o uso de `synchronized` para que a contagem seja correta.
7. É adequado declarar como `volatile` uma variável booleana usada como sinal de parada, gravada por uma thread e lida por outra.
8. Se uma classe possui dois métodos de instância `synchronized`, duas threads podem executá-los ao mesmo tempo sobre o mesmo objeto, desde que cada uma chame um método diferente.
9. Um `ReadWriteLock` permite que várias threads leiam simultaneamente, mas exige exclusividade para a escrita.
10. Em uma lista obtida por `Collections.synchronizedList`, a iteração com for-each dispensa sincronização adicional.

**3.1.3.3 Compartilhamento de estado, bloqueios e falhas**

11. Em um serviço Spring de escopo singleton, as variáveis locais declaradas nos métodos são compartilhadas entre as requisições simultâneas.
12. Impor uma ordem global para a aquisição de bloqueios elimina a condição de espera circular e, com isso, previne o deadlock.
13. Se uma tarefa enviada a um `ExecutorService` por `submit()` lançar exceção e o método `get()` do `Future` nunca for chamado, a falha poderá passar despercebida.
14. Em um `ThreadPoolExecutor` configurado com fila sem limite de capacidade, o número de threads cresce até o máximo configurado à medida que a carga aumenta.
15. Ocorre inanição (starvation) quando uma thread permanece indefinidamente sem acesso a um recurso, embora as demais continuem progredindo.

**3.1.3.4 Correção e desempenho sob requisições concorrentes**

16. Em JPA, quando duas transações leem a mesma entidade anotada com `@Version` e ambas tentam alterá-la, a segunda a gravar recebe uma exceção de bloqueio otimista.
17. Pelo padrão SQL, o nível REPEATABLE READ impede leituras sujas e não repetíveis, mas admite leituras fantasmas.
18. O reenvio automático de uma requisição POST após esgotamento do prazo de resposta é seguro, pois o protocolo HTTP garante o descarte de requisições duplicadas.
19. Em um serviço com tempo médio de resposta de 200 ms e percentil 99 de 8 s, cerca de 1% das requisições leva 8 s ou mais, o que a média não revela.
20. Aumentar o tamanho do pool de conexões com o banco de dados sempre eleva a vazão da aplicação.

### Gabarito comentado

| Questão | Gabarito | Comentário |
| --- | --- | --- |
| 1 | Certo | Um núcleo intercala tarefas (concorrência); paralelismo exige mais de um. |
| 2 | Certo | Só `start()` cria a nova thread. |
| 3 | Certo | Limite = 1 ÷ (1 − 0,8) = 5. |
| 4 | Errado | Muitas threads virtuais compartilham poucas threads de plataforma, e o ganho é em tarefas de I/O. |
| 5 | Errado | `await` suspende a função assíncrona; a thread segue atendendo o laço de eventos. |
| 6 | Certo | O incremento é atômico, por CAS. |
| 7 | Certo | O caso exige visibilidade, e não há operação composta. |
| 8 | Errado | Os dois métodos disputam o monitor do mesmo objeto (`this`). |
| 9 | Certo | Vários leitores ou um escritor. |
| 10 | Errado | A iteração exige bloco `synchronized` sobre a lista. |
| 11 | Errado | Variáveis locais ficam na pilha de cada thread; compartilhados são os atributos. |
| 12 | Certo | Basta eliminar uma das quatro condições de Coffman. |
| 13 | Certo | A exceção fica guardada no `Future` e só surge em `get()`. |
| 14 | Errado | Novas threads acima do tamanho básico só são criadas com a fila cheia; fila sem limite nunca enche. |
| 15 | Certo | Definição de inanição; no deadlock, ninguém progride. |
| 16 | Certo | O `UPDATE` com a versão antiga não altera linha alguma. |
| 17 | Certo | Fantasmas só são impedidos em SERIALIZABLE, segundo o padrão. |
| 18 | Errado | POST não é idempotente; a proteção depende da aplicação, por exemplo com chave de idempotência. |
| 19 | Certo | É o que o percentil 99 significa. |
| 20 | Errado | Além da capacidade do banco, mais conexões aumentam a contenção e pioram o desempenho. |
