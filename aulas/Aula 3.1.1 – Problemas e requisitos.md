# Aula 3.1.1 – Problemas e requisitos

Nov 14, 2026 · @João

## Mapa do item

O item 3.1.1 é o tópico “1 Problemas e requisitos”, que abre o bloco “Engenharia de Software e Programação” dos conhecimentos específicos do cargo 4. Ele tem cinco subitens e trata do que vem antes do código: entender o problema, quem é afetado e o que a solução precisa cumprir.

A correspondência abaixo usa a sua numeração hierárquica e o [edital verticalizado de Desenvolvimento de Sistemas](https://arquivos.voceconcursado.com.br/wp-content/uploads/2026/10/09190213/Edital-Verticalizado-cgu-2026-desenvolvimento-de-sistemas.pdf), montado a partir do [Edital CGU nº 1/2026](https://cdn.direcaoconcursos.com.br/uploads/2026/10/CGU-edital.pdf).

| Subitem | Conteúdo no edital | O que o Cebraspe tende a explorar |
| --- | --- | --- |
| 3.1.1.1 | Necessidades de usuários | Necessidade × solução pedida, partes interessadas, técnicas de elicitação |
| 3.1.1.2 | Requisitos funcionais e não funcionais | Classificação de um requisito dado, RNF mensurável, características de qualidade |
| 3.1.1.3 | Restrições e critérios de aceitação | Restrição × requisito × premissa, histórias de usuário, critério de aceitação × definição de pronto |
| 3.1.1.4 | Desenho centrado nas pessoas | Princípios e atividades da ISO 9241-210, usabilidade, acessibilidade, prototipagem |
| 3.1.1.5 | Relações entre problema público, processo de trabalho e solução digital | Software como meio, simplificar antes de automatizar, produto × resultado |

O tom do edital é aplicado: espere situações de um órgão público em que você deve identificar o problema real, classificar requisitos e julgar se a solução proposta resolve a causa.

Este item conversa com outros pontos do seu edital: usabilidade e acessibilidade (interfaces e experiência do usuário), gestão de produtos (descoberta, priorização e critérios de aceitação) e contratações (estudo técnico preliminar).

## 3.1.1.1 Necessidades de usuários

Necessidade é o problema que a pessoa precisa resolver; requisito é a tradução dessa necessidade em algo que o sistema deve cumprir. O erro clássico é registrar a solução que o usuário pediu, em vez da necessidade que está por trás do pedido.

### Necessidade, desejo e solução

| Conceito | O que é | Exemplo em uma ouvidoria |
| --- | --- | --- |
| Necessidade | problema ou objetivo real do usuário | saber em que pé está a manifestação que registrou |
| Desejo ou pedido | a forma como o usuário expressa o que quer | “quero um botão para ligar para o atendente” |
| Solução | um meio, entre vários, de atender à necessidade | acompanhamento on-line por protocolo, com aviso a cada mudança de situação |
| Requisito | declaração verificável do que a solução deve fazer ou ter | o sistema deve exibir a situação atual e o histórico da manifestação a partir do protocolo |

- Usuários costumam pedir soluções. Cabe a quem levanta requisitos perguntar “para quê?” até chegar à necessidade.
- A técnica dos **cinco porquês** aprofunda da queixa até a causa.
- Necessidades podem ser **explícitas** (ditas), **implícitas** (óbvias para o usuário, e por isso não ditas) e **latentes** (o usuário ainda não percebe).

### Partes interessadas

Parte interessada (stakeholder) é qualquer pessoa ou organização que afeta o sistema ou é afetada por ele. Usuário é só um dos tipos.

| Papel | Quem é | Risco se for ignorado |
| --- | --- | --- |
| Usuário final | quem opera o sistema: cidadão, servidor, auditor | sistema correto no papel e inutilizável na prática |
| Cliente ou demandante | área de negócio que pede a solução | prioridades erradas |
| Patrocinador | quem autoriza e financia | projeto sem recursos ou sem respaldo |
| Gestor do serviço | responsável pelo processo de trabalho | automação de um processo que ele não reconhece |
| Afetados indiretos | cidadãos sobre quem o sistema decide, outros órgãos | danos a direitos, retrabalho em integrações |
| Áreas de controle e apoio | jurídico, segurança, proteção de dados, infraestrutura | requisitos legais e operacionais descobertos tarde |

- Quem pede (cliente) e quem usa (usuário) frequentemente não são a mesma pessoa, e suas necessidades podem conflitar.
- Em serviço público, o usuário não escolhe o fornecedor. Por isso, é preciso considerar todos os perfis, inclusive pessoas com deficiência, com baixo letramento digital e sem acesso estável à internet.
- A Lei nº 13.460/2017 trata dos direitos do usuário de serviços públicos, e a Lei nº 14.129/2021 (Governo Digital) orienta a prestação digital de serviços.

### Engenharia de requisitos

É o processo de descobrir, analisar, documentar, validar e gerenciar requisitos. As atividades se repetem ao longo do projeto; não formam uma fase única no início.

1. **Elicitação (levantamento):** descobrir necessidades junto às partes interessadas e a outras fontes.
2. **Análise e negociação:** classificar, detectar conflitos, priorizar.
3. **Especificação:** registrar em forma compreensível e verificável.
4. **Validação:** confirmar com as partes interessadas que os requisitos descrevem o sistema certo.
5. **Gerenciamento:** controlar mudanças e manter a rastreabilidade.

### Técnicas de elicitação

| Técnica | Quando serve | Limitação |
| --- | --- | --- |
| Entrevista (aberta ou estruturada) | entender contexto e motivos | depende do que a pessoa lembra e sabe dizer |
| Questionário | muitos respondentes, dados quantitativos | pouca profundidade; perguntas mal feitas induzem |
| Observação ou etnografia | captar o trabalho como ele é, inclusive o conhecimento tácito | consome tempo; a presença do observador altera o comportamento |
| Oficina (workshop, JAD) | resolver conflitos entre áreas e decidir em conjunto | exige facilitação; vozes dominantes |
| Brainstorming | gerar muitas ideias | não valida nem prioriza |
| Prototipagem | requisitos vagos ou de interface | usuário pode confundir protótipo com produto pronto |
| Análise de documentos e sistemas legados | normas, formulários, regras já vigentes | documentos desatualizados |
| Análise de dados de uso | registros de acesso, reclamações de ouvidoria, pesquisas de satisfação | mostra o que acontece, não o porquê |

- Não existe técnica melhor em absoluto. O usual é combinar várias.
- **Observação** é a indicada quando o usuário não consegue descrever o que faz: o trabalho real difere do trabalho prescrito.
- **Persona** é um perfil fictício, baseado em pesquisa, que representa um grupo de usuários. **Jornada do usuário** descreve, passo a passo, a experiência de alguém ao usar o serviço, com dificuldades em cada etapa.

### Dificuldades típicas

- **Escopo:** limites do sistema mal definidos.
- **Entendimento:** usuários não sabem bem o que precisam, omitem o que acham óbvio ou usam termos que o analista interpreta de outro modo.
- **Volatilidade:** requisitos mudam com o tempo. Mudança é esperada e deve ser gerenciada, não proibida.
- **Conflito:** partes interessadas diferentes querem coisas incompatíveis. Resolve-se por negociação e priorização, não por decisão isolada do desenvolvedor.

## 3.1.1.2 Requisitos funcionais e não funcionais

Requisito funcional diz **o que** o sistema faz; requisito não funcional diz **com que qualidade ou sob quais condições** ele faz. A cobrança típica é classificar um requisito dado e reconhecer quando ele está mal escrito.

### Definições

- **Requisito funcional (RF):** serviço, função ou comportamento do sistema diante de entradas e situações. Exemplo: “o sistema deve permitir que o auditor anexe evidências a um achado”.
- **Requisito não funcional (RNF):** propriedade ou restrição sobre o sistema como um todo, como desempenho, segurança, disponibilidade e usabilidade. Exemplo: “a consulta deve responder em até 2 segundos em 95% das requisições”.
- **Requisito de domínio:** vem da área de aplicação, e pode ser funcional ou não funcional. Exemplo: regra de cálculo definida em norma.
- **Regra de negócio:** política ou condição da organização, que existe mesmo sem sistema. O sistema a implementa por meio de requisitos.

Níveis de detalhe:

- **Requisitos de usuário:** declarações em linguagem natural, de alto nível, para clientes e gestores.
- **Requisitos de sistema:** descrição detalhada do que será implementado, para a equipe técnica.

### Classificação dos não funcionais

Pela origem (Sommerville):

| Tipo | Origem | Exemplos |
| --- | --- | --- |
| De produto | comportamento do próprio software | desempenho, confiabilidade, usabilidade, segurança |
| Organizacionais | políticas e procedimentos de quem desenvolve ou usa | linguagem e padrões obrigatórios, processo de desenvolvimento, ambiente operacional |
| Externos | fatores de fora do sistema e da organização | legislação, interoperabilidade com outros órgãos, requisitos éticos e regulatórios |

Pelas características de qualidade de produto da ISO/IEC 25010 (edição de 2011, a mais cobrada):

| Característica | Pergunta que responde |
| --- | --- |
| Adequação funcional | as funções atendem às necessidades, de forma completa e correta? |
| Eficiência de desempenho | quanto tempo e recursos consome? |
| Compatibilidade | convive e troca informações com outros sistemas? |
| Usabilidade | pode ser usado com eficácia, eficiência e satisfação? |
| Confiabilidade | funciona sem falhar, é disponível e se recupera? |
| Segurança | protege dados e controla acessos? |
| Manutenibilidade | é fácil de modificar e testar? |
| Portabilidade | pode ser transferido para outro ambiente? |

A edição de 2023 da norma renomeou usabilidade para capacidade de interação e portabilidade para flexibilidade, e acrescentou a característica de segurança contra danos (safety).

Outro modelo citado é o FURPS+: funcionalidade, usabilidade, confiabilidade, desempenho e suportabilidade, mais restrições.

### Como classificar

| Requisito | Tipo | Por quê |
| --- | --- | --- |
| O sistema deve gerar relatório de achados por unidade auditada. | Funcional | descreve uma função |
| O sistema deve registrar em trilha de auditoria cada alteração de parecer. | Funcional | é um comportamento, embora motivado por segurança |
| O sistema deve estar disponível 99,5% do tempo, por mês. | Não funcional, de produto | confiabilidade |
| As senhas devem ser armazenadas com função de hash com sal. | Não funcional, de produto | segurança |
| O sistema deve ser desenvolvido em Java, com Spring Boot. | Não funcional, organizacional | restrição de implementação |
| O tratamento de dados pessoais deve observar a LGPD. | Não funcional, externo | origem legal |
| Um novo servidor deve conseguir registrar uma demanda em até 5 minutos, sem treinamento. | Não funcional, de produto | usabilidade, com medida |

Pontos de atenção:

- RNF **não** é menos importante. Uma falha de RNF, como segurança ou disponibilidade, pode inutilizar o sistema inteiro, enquanto a falta de uma função costuma ter contorno.
- RNF costuma afetar a arquitetura como um todo e é difícil de acrescentar depois.
- Um RNF pode **gerar** requisitos funcionais. “O sistema deve ser seguro” desdobra-se em autenticação, perfis de acesso e registro de operações.
- RNFs conflitam entre si: mais segurança pode custar desempenho e usabilidade. A solução é negociar e priorizar.
- “Não funcional” não quer dizer “que não funciona” nem “opcional”.

### Requisito bem escrito

Um bom requisito é correto, não ambíguo, completo, consistente com os demais, **verificável**, rastreável, viável e necessário.

| Mal escrito | Problema | Reescrito |
| --- | --- | --- |
| O sistema deve ser rápido. | não verificável | 95% das consultas devem responder em até 2 segundos, com 500 usuários simultâneos |
| O sistema deve ser fácil de usar. | subjetivo | 90% dos usuários de teste devem concluir o cadastro em até 3 minutos, sem ajuda |
| O sistema deve usar uma tabela USUARIO com índice no CPF. | descreve a solução, não a necessidade | o sistema deve localizar o usuário pelo CPF |
| O sistema deve permitir cadastro, consulta e exportação de relatórios e ser seguro. | vários requisitos em um | separar em requisitos atômicos |

- RNF precisa de **métrica**. Sem número e condição de medição, não há como testar.
- Requisito descreve o problema a resolver (“o quê”); o projeto descreve a solução (“como”).
- **Rastreabilidade** liga cada requisito à sua origem (para trás) e ao que o implementa e o testa (para frente). Ela sustenta a análise de impacto de mudanças.
- **Verificação** pergunta se o produto foi construído corretamente, conforme a especificação. **Validação** pergunta se foi construído o produto certo, que atende à necessidade.
- Priorização MoSCoW: deve ter (must), deveria ter (should), poderia ter (could) e não terá agora (won’t).

## 3.1.1.3 Restrições e critérios de aceitação

Restrição limita as soluções possíveis; critério de aceitação define quando um requisito foi atendido. Os dois delimitam o trabalho: a restrição antes de construir, o critério na hora de aceitar a entrega.

### Restrições

Restrição é uma condição imposta, que a equipe não escolhe e que reduz o espaço de soluções.

| Tipo | Exemplos |
| --- | --- |
| Legal e normativa | LGPD, Lei de Acesso à Informação, acessibilidade exigida pela Lei nº 13.146/2015, Lei do Governo Digital |
| Técnica | linguagem e plataforma padronizadas, integração obrigatória com sistema legado, autenticação pela conta gov.br |
| Orçamentária | teto de gastos do projeto, custo de licenças e de nuvem |
| De prazo | data fixada em norma ou em determinação de órgão de controle |
| Organizacional | equipe disponível, processo de desenvolvimento do órgão, política de segurança |
| De contexto de uso | conexão lenta em unidades regionais, uso em celular, público com baixo letramento digital |

Distinções cobradas:

| Conceito | O que é | Exemplo |
| --- | --- | --- |
| Requisito | o que a solução deve fazer ou ter | exportar o relatório em PDF |
| Restrição | limite imposto à solução ou ao projeto | usar o banco de dados já contratado pelo órgão |
| Premissa | algo assumido como verdadeiro sem comprovação, para planejar | o órgão parceiro fornecerá a API até março |
| Risco | evento incerto que pode afetar o resultado | a API do parceiro pode atrasar |

- Na classificação de Sommerville, as restrições de implementação entram nos requisitos não funcionais. Em outras fontes, aparecem como categoria própria. As duas leituras são aceitas; observe como o item define.
- Premissa que não se confirma vira problema. Por isso, toda premissa gera um risco a acompanhar.
- Restrições devem ser identificadas cedo, porque condicionam a arquitetura e o custo.
- Tripla restrição clássica de projetos: escopo, prazo e custo, com a qualidade afetada pelas três. Em abordagens ágeis, costuma-se fixar prazo e custo e variar o escopo.

### Histórias de usuário

Em métodos ágeis, o requisito costuma ser registrado como história de usuário, uma descrição curta do ponto de vista de quem se beneficia.

```
Como <papel>,
quero <funcionalidade>,
para <benefício>.
```

- **Três Cs:** Cartão (registro curto), Conversa (detalhamento entre equipe e dono do produto) e Confirmação (critérios de aceitação).
- **INVEST:** uma boa história é Independente, Negociável, Valiosa, Estimável, Pequena (small) e Testável.
- **Épico** é uma história grande demais para uma iteração, que precisa ser fatiada.
- História de usuário não é especificação completa: é um lembrete para a conversa.

### Critérios de aceitação

São condições objetivas e verificáveis que a entrega precisa satisfazer para ser aceita. Cada critério deve ter resultado binário: passou ou não passou.

- São escritos **antes** do desenvolvimento, pelo dono do produto com a equipe.
- Descrevem o resultado esperado, não a implementação.
- Cobrem o caminho principal, as exceções e os limites.
- Podem incluir aspectos não funcionais, como tempo de resposta e acessibilidade.
- Servem de base para os testes de aceitação.

Formato de cenário, usado em BDD (desenvolvimento guiado por comportamento) com a linguagem Gherkin:

```
História: Como cidadão, quero acompanhar minha manifestação pelo protocolo,
para saber a situação sem precisar ligar para a ouvidoria.

Cenário: protocolo existente
  Dado que existe a manifestação de protocolo 2026-000123
  Quando informo o protocolo 2026-000123 na consulta
  Então vejo a situação atual e a data da última movimentação

Cenário: protocolo inexistente
  Dado que não existe manifestação com o protocolo 9999-999999
  Quando informo o protocolo 9999-999999 na consulta
  Então vejo a mensagem "Protocolo não encontrado"
  E nenhum dado pessoal é exibido
```

**Dado** descreve o contexto inicial, **Quando** descreve a ação e **Então** descreve o resultado observável.

### Critério de aceitação × definição de pronto

|  | Critérios de aceitação | Definição de pronto (DoD) | Definição de preparado (DoR) |
| --- | --- | --- | --- |
| Vale para | uma história específica | todos os itens do produto | itens que entrarão na iteração |
| Responde | esta história faz o que deveria? | o trabalho atingiu o padrão de qualidade da equipe? | o item está claro o bastante para começar? |
| Exemplo | protocolo inexistente exibe mensagem de erro | código revisado, testes automatizados passando, documentação atualizada | história estimada e com critérios de aceitação escritos |

- Uma história só está concluída quando atende aos seus critérios de aceitação **e** à definição de pronto.
- No Scrum, a definição de pronto é o compromisso associado ao incremento.
- Teste de aceitação valida com o usuário ou o dono do produto; teste de sistema verifica contra a especificação.
- Critério vago, como “funcionar bem”, não é critério: não permite decidir se passou.

## 3.1.1.4 Desenho centrado nas pessoas

Desenho centrado nas pessoas é projetar a partir do entendimento de quem usa, do que essa pessoa precisa fazer e de onde ela faz, com avaliação repetida junto a usuários reais. A referência normativa é a ISO 9241-210, que usa o termo projeto centrado no ser humano.

### Princípios da ISO 9241-210

1. O projeto se baseia no entendimento explícito de usuários, tarefas e ambientes.
2. Os usuários são envolvidos ao longo de todo o projeto e desenvolvimento.
3. O projeto é conduzido e refinado por avaliação centrada no usuário.
4. O processo é iterativo.
5. O projeto aborda toda a experiência do usuário.
6. A equipe reúne competências e perspectivas multidisciplinares.

### Atividades

Depois de planejar o processo, quatro atividades se repetem até que a solução atenda aos requisitos:

1. Compreender e especificar o **contexto de uso**: quem são os usuários, que tarefas realizam, em que ambiente físico, social e técnico.
2. Especificar os **requisitos do usuário**.
3. Produzir **soluções de projeto**, de esboços a protótipos.
4. **Avaliar** as soluções em relação aos requisitos, com usuários.

Se a avaliação reprova a solução, volta-se à atividade necessária. O ciclo não é uma cascata.

### Usabilidade, experiência e acessibilidade

| Conceito | Definição | Referência |
| --- | --- | --- |
| Usabilidade | grau em que usuários específicos atingem objetivos específicos com **eficácia, eficiência e satisfação**, em um contexto de uso específico | ISO 9241-11 |
| Experiência do usuário (UX) | percepções e respostas da pessoa antes, durante e depois do uso; inclui emoções e expectativas | ISO 9241-210 |
| Acessibilidade | possibilidade de uso por pessoas com a mais ampla variedade de capacidades, inclusive com deficiência | WCAG, eMAG |

- **Eficácia:** o usuário consegue concluir a tarefa, corretamente. **Eficiência:** com quanto esforço e tempo. **Satisfação:** com que conforto e aceitação.
- Usabilidade não é propriedade absoluta do produto: depende do usuário, da tarefa e do contexto.
- UX é mais ampla que usabilidade. Um sistema pode ser usável e, ainda assim, gerar má experiência.
- A WCAG organiza a acessibilidade em quatro princípios: **perceptível, operável, compreensível e robusto**, com níveis de conformidade A, AA e AAA.
- Acessibilidade beneficia a todos, não só pessoas com deficiência: legendas, contraste e navegação por teclado ajudam em muitos contextos.

### Design thinking e duplo diamante

São abordagens para explorar o problema antes de fixar a solução. As duas alternam **divergência** (abrir possibilidades) e **convergência** (escolher).

| Modelo | Etapas |
| --- | --- |
| Design thinking (cinco etapas) | empatia, definição, ideação, prototipação, teste |
| Duplo diamante | descobrir, definir, desenvolver, entregar |

No duplo diamante, o primeiro diamante trata do **problema certo** (descobrir diverge, definir converge). O segundo trata da **solução certa** (desenvolver diverge, entregar converge). As etapas não são lineares: testar pode devolver o trabalho à definição do problema.

### Ferramentas

| Ferramenta | Para que serve |
| --- | --- |
| Persona | representar um grupo de usuários com base em pesquisa |
| Mapa de empatia | registrar o que o usuário diz, pensa, faz e sente |
| Jornada do usuário | visualizar etapas, pontos de contato e dificuldades |
| Protótipo de baixa fidelidade | testar ideia e fluxo cedo, com papel ou wireframe; barato de descartar |
| Protótipo de alta fidelidade | testar aparência e interação próximas do produto final |
| Teste de usabilidade | observar usuários reais executando tarefas reais |
| Avaliação heurística | especialistas inspecionam a interface com base em princípios, sem usuários |

- Teste de usabilidade envolve **usuários**; avaliação heurística envolve **especialistas**. O examinador costuma trocar.
- Testes com poucos usuários, repetidos a cada iteração, revelam a maior parte dos problemas. A referência clássica de Nielsen é de cinco usuários por rodada.
- Protótipo serve para aprender barato. Quanto mais cedo o erro aparece, menor o custo de corrigir.
- Opinião de gestor ou de desenvolvedor não substitui a observação do usuário real.

As dez heurísticas de Nielsen: visibilidade do estado do sistema; correspondência entre o sistema e o mundo real; controle e liberdade do usuário; consistência e padrões; prevenção de erros; reconhecimento em vez de memorização; flexibilidade e eficiência de uso; estética e projeto minimalista; ajuda para reconhecer, diagnosticar e corrigir erros; ajuda e documentação.

### No serviço público

- O público é universal e não pode trocar de fornecedor. Projetar para o usuário médio exclui quem mais depende do serviço.
- **Linguagem simples:** textos que o cidadão entende na primeira leitura, sem jargão administrativo.
- **Barreiras digitais:** falta de acesso, de equipamento, de letramento digital ou de acessibilidade. A solução digital deve prever canais alternativos e apoio.
- **Pedir uma vez só:** não exigir do cidadão informação que a administração já possui.
- Envolver usuários não é fazer tudo o que pedem: é confrontar as soluções com evidências de uso.

## 3.1.1.5 Problema público, processo de trabalho e solução digital

A solução digital é um meio: ela só gera valor se melhorar um processo de trabalho que, por sua vez, enfrenta um problema público. O subitem pede que você percorra essa cadeia nos dois sentidos e identifique onde ela se rompe.

### A cadeia

1. **Problema público:** diferença entre a situação atual e a situação desejada, relevante para a coletividade. Exemplo: prestações de contas de convênios levam, em média, três anos para serem analisadas.
2. **Causas:** o que produz o problema. Exemplo: análise manual de documentos, dados repetidos em várias planilhas, falta de critério de priorização.
3. **Processo de trabalho:** conjunto de atividades que transforma insumos em resultados, com papéis, regras e sistemas. Exemplo: receber, triar, analisar, diligenciar e concluir a prestação de contas.
4. **Solução digital:** software, dados e integrações que apoiam ou automatizam partes do processo. Exemplo: triagem automática por risco e cruzamento com bases já existentes.
5. **Resultado:** mudança observada no problema. Exemplo: tempo médio de análise reduzido, com a mesma qualidade.

- Sintoma não é causa. “O sistema é lento” pode ser sintoma de um processo com etapas desnecessárias.
- A **árvore de problemas** organiza o raciocínio: problema central no tronco, causas nas raízes e efeitos na copa.
- Nem todo problema público pede software. Alternativas: mudar a norma, simplificar o processo, capacitar pessoas, reutilizar uma solução que já existe.

### Processo de trabalho

- **AS-IS** é o processo como funciona hoje. **TO-BE** é o processo redesenhado.
- Primeiro simplifica-se o processo, depois automatiza-se. Automatizar um processo ruim apenas produz o erro mais rápido.
- A notação usual é o **BPMN**, com estes elementos básicos:

| Elemento | Representa | Símbolo |
| --- | --- | --- |
| Evento | algo que acontece: início, intermediário ou fim | círculo |
| Atividade | trabalho executado: tarefa ou subprocesso | retângulo de cantos arredondados |
| Gateway | decisão, divisão ou junção de caminhos | losango |
| Fluxo de sequência | ordem das atividades dentro de um participante | seta contínua |
| Fluxo de mensagem | comunicação entre participantes distintos | seta tracejada |
| Piscina e raia | participante do processo e suas subdivisões por papel | retângulos que contêm o fluxo |

- Gateway **exclusivo** (XOR) segue um único caminho; **paralelo** (AND) segue todos; **inclusivo** (OR) segue um ou mais.
- O levantamento do processo revela requisitos: regras de negócio, pontos de decisão, papéis (que viram perfis de acesso) e trocas de informação (que viram integrações).

### Graus de mudança

| Termo | O que muda | Exemplo |
| --- | --- | --- |
| Digitização | o suporte: do papel para o arquivo digital | digitalizar o formulário em PDF |
| Digitalização | o processo passa a rodar em meio digital | formulário eletrônico com tramitação no sistema |
| Transformação digital | o serviço é repensado a partir das possibilidades digitais | o benefício é concedido automaticamente, sem requerimento, com dados que o Estado já possui |

Trocar o papel por uma tela, mantendo as mesmas etapas e exigências, não é transformação digital.

### Medir o que importa

| Nível | Pergunta | Exemplo |
| --- | --- | --- |
| Insumo | o que foi consumido? | horas da equipe, orçamento |
| Produto (output) | o que foi entregue? | sistema implantado, funcionalidades em produção |
| Resultado (outcome) | o que mudou para o usuário ou para o processo? | tempo de análise caiu de 3 anos para 8 meses |
| Impacto | o que mudou no problema público? | mais recursos mal aplicados são recuperados a tempo |

- Entregar o sistema é produto, não resultado. Sucesso se mede pela mudança no problema.
- O objetivo deve ser definido **antes**, com linha de base e meta, para que o resultado possa ser verificado.
- A rastreabilidade completa liga problema, objetivo, requisito, funcionalidade, teste e indicador.

### Riscos da solução digital

- **Solucionismo tecnológico:** começar pela tecnologia e depois procurar um problema para ela.
- **Automatizar a ineficiência:** informatizar etapas que deveriam ser eliminadas.
- **Exclusão:** o canal digital único deixa de fora quem não tem acesso, equipamento ou habilidade.
- **Rigidez:** o sistema cristaliza uma regra que a norma permitiria flexibilizar, e o caso atípico fica sem saída.
- **Decisão automatizada sem revisão:** erros e vieses atingem muitas pessoas de uma vez, sem explicação nem recurso.
- **Duplicação:** construir o que já existe em outro órgão, em vez de reutilizar ou integrar.
- **Dependência de fornecedor:** a solução fica presa a quem a construiu.

### Ligação com os requisitos

Cada requisito deve responder a uma pergunta simples: que parte do problema ele ajuda a resolver? Requisito sem ligação com um objetivo é candidato a corte. Problema sem requisito correspondente é lacuna.

Perguntas que organizam a análise de uma demanda:

1. Qual é o problema, para quem, e qual a evidência de que ele existe?
2. Quais são as causas, e quais delas estão ao alcance de uma solução digital?
3. Como o processo funciona hoje, e o que pode ser eliminado antes de automatizar?
4. Que alternativas existem, inclusive as não digitais e as já prontas?
5. Como saberemos que deu certo?

## Revisão: pegadinhas de certo ou errado

As afirmações abaixo reúnem as trocas mais prováveis em cada subitem. Desconfie de “sempre”, “apenas”, “dispensa” e “basta”.

| Subitem | Afirmação | Gabarito | Por quê |
| --- | --- | --- | --- |
| 3.1.1.1 | No levantamento de requisitos, deve-se registrar fielmente a solução solicitada pelo usuário, pois ele é quem melhor conhece a própria necessidade. | Errado | O usuário conhece o problema; a solução pedida deve ser investigada até a necessidade. |
| 3.1.1.1 | A observação é técnica adequada quando os usuários têm dificuldade de descrever como executam suas tarefas. | Certo | Capta o conhecimento tácito e o trabalho real. |
| 3.1.1.1 | As partes interessadas de um sistema limitam-se aos seus usuários finais. | Errado | Incluem demandante, patrocinador, gestores, áreas de apoio e afetados indiretos. |
| 3.1.1.1 | A elicitação de requisitos ocorre uma única vez, no início do projeto. | Errado | As atividades de requisitos são iterativas e se repetem. |
| 3.1.1.2 | “O sistema deve responder às consultas em até 2 segundos” é requisito funcional. | Errado | É não funcional, de desempenho. |
| 3.1.1.2 | Requisitos não funcionais são menos críticos que os funcionais, pois não afetam o funcionamento do sistema. | Errado | Falha em RNF pode inutilizar o sistema inteiro. |
| 3.1.1.2 | A exigência de conformidade com a LGPD é exemplo de requisito não funcional externo. | Certo | Origem legal, fora do sistema e da organização. |
| 3.1.1.2 | “O sistema deve ser amigável” é um requisito não funcional adequadamente especificado. | Errado | Não é verificável; falta métrica. |
| 3.1.1.2 | Validação verifica se o produto foi construído conforme a especificação. | Errado | Isso é verificação; validação confirma que é o produto certo. |
| 3.1.1.3 | Premissa é uma limitação imposta ao projeto, enquanto restrição é uma hipótese assumida como verdadeira. | Errado | Os conceitos estão trocados. |
| 3.1.1.3 | Critérios de aceitação são específicos de cada história, ao passo que a definição de pronto se aplica a todos os itens. | Certo | Diferença de abrangência. |
| 3.1.1.3 | No formato Dado-Quando-Então, a cláusula “Quando” descreve o resultado esperado. | Errado | “Quando” é a ação; “Então” é o resultado. |
| 3.1.1.3 | Na sigla INVEST, a letra T indica que a história deve ser testável. | Certo | Independente, negociável, valiosa, estimável, pequena, testável. |
| 3.1.1.4 | Segundo a ISO 9241-11, usabilidade envolve eficácia, eficiência e satisfação em um contexto de uso específico. | Certo | Definição da norma. |
| 3.1.1.4 | No projeto centrado no ser humano, os usuários são consultados no início, para o levantamento, e ao final, para a homologação. | Errado | O envolvimento é contínuo, e o processo é iterativo. |
| 3.1.1.4 | A avaliação heurística é realizada por usuários reais executando tarefas no sistema. | Errado | É feita por especialistas; com usuários é o teste de usabilidade. |
| 3.1.1.4 | Os princípios da WCAG são: perceptível, operável, compreensível e robusto. | Certo | Quatro princípios. |
| 3.1.1.5 | A implantação do sistema no prazo e no custo previstos demonstra que o problema público foi resolvido. | Errado | Isso é produto; resultado é a mudança no problema. |
| 3.1.1.5 | Recomenda-se automatizar o processo tal como ele é executado hoje e simplificá-lo depois, com o sistema em produção. | Errado | Simplifica-se antes de automatizar. |
| 3.1.1.5 | No BPMN, o gateway paralelo ativa todos os caminhos de saída. | Certo | O exclusivo ativa apenas um. |
