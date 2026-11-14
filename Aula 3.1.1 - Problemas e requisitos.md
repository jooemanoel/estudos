# Aula: 3.1.1 Problemas e requisitos

**Concurso:** CGU – AFFC, especialidade TI – Desenvolvimento de Sistemas (Cebraspe)
**Bloco:** 3 Conhecimentos específicos › 3.1 Engenharia de software e programação

---

## 1. Visão geral

Este item cobre a etapa **anterior** ao código: entender o problema, traduzi-lo em requisitos verificáveis e garantir que a solução digital sirva a pessoas e a processos reais. Em prova, ele costuma aparecer em casos curtos de um órgão público, e a banca testa se você distingue conceitos parecidos: necessidade, requisito, restrição e critério de aceitação.

O encadeamento lógico dos cinco subitens é este:

**problema público → necessidades das pessoas → requisitos → restrições e critérios de aceitação → solução digital que se encaixa no processo de trabalho**

O desenho centrado nas pessoas atravessa todas as etapas.

| Subitem | Tema |
|---|---|
| 3.1.1.1 | Necessidades de usuários |
| 3.1.1.2 | Requisitos funcionais e não funcionais |
| 3.1.1.3 | Restrições e critérios de aceitação |
| 3.1.1.4 | Desenho centrado nas pessoas |
| 3.1.1.5 | Relações entre problema público, processo de trabalho e solução digital |

---

## 2. Necessidades de usuários (3.1.1.1)

### Conceito

Necessidade é aquilo que a pessoa precisa alcançar ou resolver. Requisito é a forma como o sistema vai atender a essa necessidade. A diferença importa porque os usuários costumam pedir soluções ("quero um botão para exportar planilha") quando a necessidade real é outra ("preciso prestar contas ao meu chefe toda semana").

### Quem são os usuários

Vão além de quem digita no sistema: cidadãos, servidores de atendimento, analistas, gestores, auditores, outros órgãos que consomem a API. Todos são partes interessadas (*stakeholders*), e as necessidades podem conflitar entre si.

### Como levantar necessidades (elicitação)

- entrevistas e questionários;
- observação do trabalho real (muitas vezes revela o que ninguém sabia explicar);
- análise de documentos, normas e manifestações já recebidas (reclamações recorrentes são ótima fonte);
- oficinas com os envolvidos;
- prototipagem, para que as pessoas reajam a algo concreto;
- mapas de jornada e personas, para organizar o que foi aprendido.

### Armadilhas de prova

- Levantar necessidades **só com a gestão** deixa de fora quem usa o serviço na ponta.
- Aceitar o pedido literal sem investigar a necessidade é um erro de análise.
- Necessidades **mudam**, então levantá-las uma única vez no início não basta.

---

## 3. Requisitos funcionais e não funcionais (3.1.1.2)

**Funcionais (RF)** descrevem *o que* o sistema faz: registrar manifestação, anexar arquivos, consultar andamento, gerar relatório.

**Não funcionais (RNF)** descrevem *como* ele deve se comportar ou *quão bem* deve fazê-lo. Uma boa referência de classificação é a ISO/IEC 25010:

| Qualidade | Exemplo de RNF |
|---|---|
| Desempenho | consulta responde em até 3 s com 500 usuários simultâneos |
| Segurança | acesso à identidade do denunciante só para perfil autorizado, com registro de auditoria |
| Confiabilidade/disponibilidade | disponibilidade mensal de 99,5% |
| Usabilidade/acessibilidade | operável por leitor de tela |
| Manutenibilidade | módulos com baixo acoplamento e documentação atualizada |
| Compatibilidade/interoperabilidade | expor dados por API REST em JSON |
| Portabilidade | funcionar nos navegadores e dispositivos mais usados pelo público |

### Qualidades de um bom requisito

Claro e não ambíguo, verificável, consistente com os demais, rastreável até a necessidade que o originou, viável e priorizado.

**Ponto-chave:** RNF vago ("o sistema deve ser rápido") não é testável. O RNF útil é **mensurável** ("até 3 segundos para 95% das consultas").

### Níveis de requisito

Há requisitos de negócio (objetivo da organização), de usuário (o que a pessoa precisa fazer) e de sistema (detalhamento técnico). Regras de negócio, como "prazo de resposta de 20 dias, prorrogável por mais 10", vêm da norma e geram requisitos.

---

## 4. Restrições e critérios de aceitação (3.1.1.3)

**Restrição** é um limite imposto de fora, que reduz as opções de solução: prazo, orçamento, tecnologia obrigatória, hospedagem em infraestrutura do órgão, legislação (proteção de dados, transparência, acessibilidade), normas internas de segurança. A restrição não nasce da necessidade do usuário.

### Restrição × RNF

Uma forma prática de distinguir: o RNF descreve uma qualidade desejada do produto ("responder em 3 s"); a restrição é uma imposição que condiciona o projeto ("deve rodar nos servidores do órgão"). Na prática as fronteiras se tocam, e a banca costuma dar pistas no enunciado, como "determinação da área de segurança" ou "imposto por norma".

### Critério de aceitação

Condição objetiva que permite dizer "este requisito foi entregue". Características:

- é verificável por teste ou inspeção, sem opinião subjetiva;
- é combinado antes do desenvolvimento, com o demandante;
- orienta os testes de aceitação.

Um formato muito usado é o **dado/quando/então**:

> *Dado* que o usuário é analista sem perfil de acesso à identidade, *quando* abrir uma manifestação, *então* o nome do denunciante não é exibido nem retornado pela API.

### Critério de aceitação × definição de pronto (DoD)

O critério é específico de cada requisito ou história. A definição de pronto é o padrão geral de qualidade da equipe, válido para tudo (testes passando, revisão de código feita, documentação atualizada).

---

## 5. Desenho centrado nas pessoas (3.1.1.4)

Abordagem iterativa em que o projeto parte das pessoas e do contexto de uso, e não da tecnologia. O ciclo, nos moldes da ISO 9241-210, tem quatro atividades que se repetem:

1. entender e especificar o **contexto de uso**;
2. especificar os **requisitos das pessoas** usuárias;
3. produzir **soluções de design** (protótipos);
4. **avaliar** com usuários reais e voltar ao passo 1 se preciso.

### Princípios

Usuários participam desde o início; o processo é iterativo; considera-se a experiência completa (antes, durante e depois do uso); a equipe é multidisciplinar.

### No setor público

Isso inclui a **diversidade** do público: pessoas com deficiência, idosos, baixa escolaridade, pouca conectividade, dispositivos modestos, povos e comunidades tradicionais. Daí vêm as **barreiras digitais** (acessibilidade técnica, linguagem complexa, exigência de equipamento ou conta que a pessoa não tem) e a necessidade de manter **canais alternativos** de atendimento. Para acessibilidade, os parâmetros de referência são as diretrizes WCAG e o eMAG, o modelo de acessibilidade do governo federal.

### Armadilhas de prova

- Validar com usuários **só no fim** é erro: a validação é contínua.
- Acessibilidade não é "extra" a tratar depois: é requisito desde o início.
- Testar apenas com a equipe de TI não representa o público real.

---

## 6. Problema público, processo de trabalho e solução digital (3.1.1.5)

A ideia central: **tecnologia não é fim em si mesma.** O sistema deve resolver um problema público (por exemplo, manifestações que demoram a ser respondidas), e isso passa por entender o processo de trabalho que produz o resultado.

### Sequência recomendada

1. delimitar o problema público e como ele se mede;
2. mapear o processo atual (*as-is*): etapas, responsáveis, gargalos, retrabalho, redundâncias;
3. redesenhar o processo (*to-be*): eliminar etapas inúteis, simplificar, redistribuir responsabilidades;
4. só então definir a solução digital que sustenta o processo redesenhado;
5. medir resultados.

**Erro clássico:** informatizar um processo ruim sem revisá-lo. O resultado é um processo ruim mais rápido, ou seja, a ineficiência automatizada.

### Resultado × produto

Entregar o sistema (produto) não é o mesmo que resolver o problema (resultado). Indicadores de resultado medem o efeito para o cidadão, como redução do prazo de resposta e da taxa de manifestações sem retorno. Um indicador como "sistema no ar" mede apenas a entrega.

### Cuidados adicionais

A solução precisa conversar com outros sistemas e bases do governo (interoperabilidade), respeitar as normas de proteção de dados e não excluir quem não tem acesso digital.

---

## 7. Exemplo integrado (contexto CGU)

- **Problema público:** manifestações de ouvidoria demoram a ser respondidas e muitos cidadãos não conseguem acompanhar o andamento.
- **Necessidades:** o cidadão quer saber em que etapa está seu pedido; o analista quer triar rapidamente sem retrabalho; o gestor quer enxergar gargalos.
- **RF:** consultar andamento por protocolo; classificar e encaminhar manifestações; gerar painel de prazos.
- **RNF:** consulta em até 3 s com 500 usuários simultâneos; acessível a leitor de tela; acesso à identidade do denunciante restrito e auditado.
- **Restrições:** operar em servidores do órgão; cumprir a legislação de proteção de dados; entrega em 6 meses.
- **Critério de aceitação (exemplo):** com 500 usuários simultâneos, 95% das consultas respondem em até 3 s, medido em teste de carga.
- **Processo:** antes de codificar, descobre-se que a manifestação passa por três áreas que reenviam o mesmo despacho, e esse fluxo é simplificado.
- **Indicador de resultado:** prazo médio de resposta e percentual de cidadãos que consultam o andamento sem contatar o atendimento.

---

## 8. Pegadinhas frequentes em Cebraspe

- "O requisito não funcional **não precisa** ser testado." (Errado: precisa, e por isso deve ser mensurável.)
- "A **restrição** é a necessidade do usuário." (Errado: é limite imposto ao projeto.)
- "Critérios de aceitação **podem ser definidos após** a entrega." (Errado: devem ser acordados antes.)
- "Digitalizar fielmente o processo **garante** eficiência." (Errado: pode só automatizar a ineficiência.)
- "A validação com usuários é **dispensável** quando há bons especialistas." (Errado: especialista não substitui o usuário real.)
- Afirmações com **"sempre", "somente", "garante", "dispensa"** costumam sinalizar item errado.

---

## 9. Fixação

Julgue (C/E):

1. O requisito "o sistema deve permitir anexar arquivos à manifestação" é não funcional, por tratar de uma capacidade acessória.
2. A exigência de hospedar o sistema em infraestrutura do próprio órgão por determinação de normativo de segurança constitui restrição ao projeto.
3. O desenho centrado nas pessoas pressupõe participação e avaliação com usuários ao longo de todo o ciclo, de modo iterativo.
4. Se o prazo médio de resposta ao cidadão não melhorou, mas o sistema foi entregue conforme especificado, conclui-se que o problema público foi resolvido.

<details>
<summary>Gabarito</summary>

1. **E**: é funcional.
2. **C**
3. **C**
4. **E**: entrega do produto não equivale a resultado.

</details>

---

## 10. Síntese para revisar

- **Necessidade** ≠ **requisito** ≠ **solução**.
- **RF** = o que faz; **RNF** = quão bem/como se comporta (mensurável).
- **Restrição** = limite imposto; **critério de aceitação** = condição verificável de entrega, combinada antes.
- **Centrado nas pessoas** = usuários reais, diversos, em ciclo iterativo, com acessibilidade desde o início.
- **Antes de automatizar**, entender e, se preciso, redesenhar o processo; medir **resultado**, não só entrega.
