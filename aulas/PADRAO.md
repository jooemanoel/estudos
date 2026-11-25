# Padrão das audioaulas

Use estas regras para gerar o roteiro de uma audioaula sobre um item do `conteudo.md`. As aulas são para o concurso da CGU (AFFC, TI, Desenvolvimento de Sistemas, banca Cebraspe), mas o texto da aula não menciona concurso, cargo nem banca: basta o item do edital.

O modelo de referência é a aula `aula_1.4.4_planejamento_e_recursos_roteiro_audio.txt`.

## Escopo

- Cubra todos os subitens do item no `conteudo.md`, na ordem, sem pular nenhum.
- Indique a base normativa de cada tema (lei, decreto, artigo). Se não tiver certeza de um dispositivo ou da redação vigente, diga isso uma única vez, no ponto em que aparece, em uma frase curta. Não repita a ressalva no resumo nem no gabarito.
- Destaque alterações legislativas recentes que a banca costuma explorar.
- Não repita o que já foi visto no item anterior: faça no máximo uma frase de ponte. Não aprofunde temas do item seguinte, só cite que virão.

## Estilo e concisão

O ouvinte já conhece o formato das aulas e o estilo Cebraspe. Cada frase deve trazer conteúdo novo. Não inclua:

- orientações sobre como resolver os itens ou estudar ("Pense. Certo ou errado?", "ouça todos e anote", "leia com atenção", "desconfie de sempre e nunca", "um erro anula um acerto", "antes de marcar, simule");
- explicação genérica dos padrões de pegadinha da banca (generalizações, inversão de regra e exceção etc.). Aplique esses padrões aos itens, sem descrevê-los;
- frases de transição ou de animação ("Olá de novo", "Vamos lá", "Agora, atenção", "Esse ponto é muito importante", "Chegamos ao fim");
- recapitulação ao fim de cada bloco. O resumo final é o único lugar de revisão;
- repetição do mesmo conceito em mais de um bloco. Se uma pegadinha já foi explicada no bloco, ela não volta no resumo; no gabarito, apenas remeta ao dispositivo;
- encerramento motivacional ou mensagem de despedida.

Use frases curtas e diretas. Use exemplo prático (situação de trabalho de um auditor ou desenvolvedor na CGU) só quando ele esclarecer algo que a explicação sozinha não deixa claro, no máximo um por subitem.

## Duração

- A faixa é teto, não meta: escreva o necessário para cobrir os subitens com profundidade, sem encher. Referência: 3.500 a 6.000 palavras.
- Estenda quando o conteúdo exigir (muitos subitens, legislação extensa ou muito cobrada, temas técnicos interdependentes, norma recente). Limite de referência: cerca de 10.000 palavras.
- A cobertura completa tem prioridade: nunca resuma superficialmente um subitem para caber na faixa.
- Se passar de cerca de 10.000 palavras, divida em partes. A abertura da parte seguinte lista só os blocos que virão, sem recapitular a anterior. O resumo e os itens com gabarito ficam na última parte, cobrindo a aula inteira.
- Ao entregar (fora do arquivo), informe a contagem aproximada de palavras e, se passar de 6.000, explique o motivo em uma frase.

## Formato para áudio

- O arquivo contém apenas o texto a ser lido em voz alta. Sem glossário de pronúncia, notas ao leitor ou qualquer trecho que não deva ser ouvido.
- Texto corrido: sem tabelas, marcadores, setas ou símbolos.
- Escreva por extenso números de artigos, incisos e siglas na primeira ocorrência (ex.: "artigo cento e dezessete, inciso nove, da Lei oito mil cento e doze").
- Os títulos de seção ficam sozinhos na linha, em maiúsculas, com uma linha em branco antes e depois. Parágrafos são separados por linha em branco: o gerador de áudio transforma isso em pausa.

## Pronúncia de termos em inglês

O texto é lido por uma voz em português (pt-BR-AntonioNeural).

- Sempre que existir um termo consagrado em português, use-o (ex.: "injeção de dependências", "inversão de controle", "chave estrangeira").
- Quando o termo em inglês for indispensável, use grafia fonética aportuguesada. Exemplos: Spring Boot → Sprín Búti; framework → freimuórqui; endpoint → endpóinti; JSON → djêisson; REST → rést; token → tôquen; cache → quéchi; deploy → diplói; bean → bín; OAuth → ô áuti.
- Siglas lidas letra a letra são escritas por extenso: JPA → "jota pê á"; API → "á pê í"; SQL → "ésse quê éle"; HTTP → "agá tê tê pê".
- Anotações e símbolos: "@Autowired" → "arroba ótouáierd"; "@Transactional" → "arroba transácheonal".
- Nomes em camelCase são separados e escritos foneticamente: "findById" → "fáind bai ái di".
- Não leia código linha a linha: descreva o que o código faz e cite só os nomes essenciais, em grafia fonética.

## Estrutura

Siga exatamente esta sequência e estes títulos:

```
AUDIOAULA. ITEM UM PONTO QUATRO PONTO QUATRO DO EDITAL: PLANEJAMENTO E RECURSOS


ABERTURA

No máximo quatro frases. Diga o item, liste os subitens (ou os blocos) e, se houver,
uma frase sobre o que a banca costuma cobrar especificamente deste item.


BLOCO UM: TÍTULO DO SUBITEM

Explicação, base legal e, quando útil, um exemplo.

Pegadinhas do bloco um.

Cada pegadinha em uma ou duas frases: a afirmação errada típica e a correção, com o dispositivo.


BLOCO DOIS: ...


RESUMO FINAL: PONTOS PARA NÃO ESQUECER

Um ou dois pontos por subitem, em frases curtas, sem reexplicar.


ITENS DE FIXAÇÃO COM GABARITO

Item um. Afirmação no estilo Cebraspe.

Gabarito: certo. Justificativa de uma ou duas frases, com o dispositivo.

Item dois. ...

Gabarito: errado. ...


ENCERRAMENTO

A próxima aula trata do item um ponto quatro ponto cinco, serviços e transformação digital.
```

Detalhes:

- **Título:** "AUDIOAULA. ITEM [número por extenso] DO EDITAL: [TEMA]". Em aula dividida, acrescente ". PARTE UM DE DUAS" (ou "DOIS DE DUAS" etc.).
- **Blocos:** um por subitem, numerados por extenso ("BLOCO UM", "BLOCO DOIS"...). Um subitem extenso pode ser dividido em mais de um bloco; diga isso na abertura.
- **Pegadinhas:** ficam no fim do bloco a que pertencem, depois da linha "Pegadinhas do bloco [número]." Não há seção de pegadinhas separada.
- **Itens de fixação:** 12, sem instruções antes, cada um seguido imediatamente do seu gabarito ("Gabarito: certo." ou "Gabarito: errado."). Misture itens literais da norma e situacionais, com pelo menos metade explorando as pegadinhas dos blocos. Em aulas estendidas ou divididas, até 20 itens, distribuídos proporcionalmente entre os subitens.
- **Encerramento:** uma frase anunciando o próximo item. Na primeira parte de uma aula dividida, anuncia a parte seguinte.
- **Aula dividida:** só a última parte tem resumo e itens de fixação.

## Entrega

- Arquivo TXT em UTF-8 nesta pasta (`estudos/aulas`), chamado `aula_[ITEM]_[tema_em_snake_case]_roteiro_audio.txt`. Exemplo: `aula_1.4.5_servicos_e_transformacao_digital_roteiro_audio.txt`.
- Se houver divisão: `aula_[ITEM]_[tema]_parte1_roteiro_audio.txt`, `aula_[ITEM]_[tema]_parte2_roteiro_audio.txt` etc.

## Depois do roteiro (no Claude Code)

1. Em `audioaulas-mobile`, rode `npm run gerar-audios -- [ITEM]` para gerar o MP3. Leva cerca de 10 minutos por aula.
2. Rode `npm run publicar-aulas` para enviar TXT, MP3 e catálogo à release `audioaulas`. Faça o commit do `src/catalogo.json` atualizado no audioaulas-mobile.
3. Faça o commit do TXT e do MP3 no repositório `estudos`.
