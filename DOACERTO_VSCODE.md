# DoaCerto: roteiro para conduzir o laboratório no VS Code

Este repositório é a **interface didática**. O projeto Python será criado em outra pasta, `doacerto/`. A página permite explorar e exportar um rascunho; ela não executa OpenSpec, agente ou testes Python.

## A pergunta que abre a aula

> Uma OSC pediu um sistema para organizar doações. O que significa “organizar” neste contexto?

Peça respostas antes de mostrar código. Uma implementação imediata pode escolher, por conta própria, se aceita doação negativa, se descarta excedentes e se esconde produtos cuja meta foi atingida. SDD torna essas escolhas explícitas em requisitos, cenários e testes.

## O que conta como boa especificação

- **Objetivo e usuário:** quem precisa decidir o quê com o relatório.
- **Escopo:** quatro capacidades do MVP; registrar o que fica de fora.
- **Vocabulário:** produto, estoque, meta, falta e doação.
- **Invariantes:** estoque e meta são inteiros não negativos; `falta = max(0, meta - estoque)`; erro não muda o estado.
- **Comportamento observável:** entrada, ação, saída e efeito no estoque. `SHALL` indica obrigação no formato OpenSpec.
- **Critérios de aceite:** sucesso, erro e fronteira; valores concretos, resultado mensurável e referência ao requisito.
- **Design:** como implementar, por exemplo dicionário em memória e funções Python. Não substitui a regra de negócio.
- **Rastreabilidade:** cada requisito tem cenário e teste; cada tarefa aponta para a spec.

Critério ruim: “aceitar doações corretamente”. Critério verificável: “Dado Arroz com estoque 8 e meta 20, quando receber 5 unidades, então o estoque será 13 e faltarão 7”.

## Decisões de referência para a demonstração

1. Doações podem ultrapassar a meta. O excedente permanece no estoque; a falta é zero.
2. Estoque e meta são inteiros não negativos. Doação é inteiro positivo. `bool`, frações e números em texto são inválidos.
3. Nome é obrigatório após remover espaços nas pontas. A identidade usa `casefold()`; cadastro repetido é erro. O nome exibido preserva a grafia do primeiro cadastro, sem espaços externos.
4. Doação para produto inexistente gera `KeyError`. Entrada inválida gera `ValueError`. A falha não altera o catálogo.
5. O relatório contém todos os produtos, ordenados por nome sem diferenciar maiúsculas/minúsculas. Falta positiva entra em “precisam de doação”; falta zero entra em “meta atingida”. Meta zero já está atingida.
6. O aplicativo de terminal usa dados em memória, sem persistência. A saída textual mostra tabela e as duas seções, inclusive quando estiverem vazias.

Essas são decisões do exemplo, para a aula andar. Em um projeto real, confirme com a OSC antes de tratar hipóteses como aprovadas.

## Roteiro de 30 minutos (10:20–10:50)

| Horário | Ação na tela | Fala e evidência |
| --- | --- | --- |
| 10:20–10:22 | Abra a pasta `doacerto/` no VS Code; confirme Python, Node e OpenSpec; rode `openspec init`. | O OpenSpec configura artefatos e integração do assistente. Observe o nome de comando que o `init` imprimir. |
| 10:22–10:26 | No chat do agente, invoque **explore** com o prompt abaixo. | Pergunte à turma sobre excedente, números negativos, inexistentes e relatório. Registre as decisões. |
| 10:26–10:32 | Invoque **propose** com o prompt abaixo. | Inspecione `proposal.md`, `specs/doacoes/spec.md`, `design.md` e `tasks.md`. O design pode variar com o schema/perfil. |
| 10:32–10:36 | Leia a spec e os cenários; peça **update** se houver lacuna; rode `openspec validate gerenciar-doacoes --strict`. | Verifique limites, erros sem mutação e correspondência entre requisitos e cenários. A validação da CLI é estrutural. |
| 10:36–10:42 | Invoque **apply** somente após revisar o contrato. | O agente implementa `app.py` e `tests/test_doacoes.py`, marcando tarefas concluídas após verificação. |
| 10:42–10:46 | Rode `python3 -m unittest discover -s tests -v` e `python3 app.py`. | Compare a saída com Arroz 8/20, Feijão 15/15 e Sabonete 3/10. |
| 10:46–10:48 | Se houver divergência, descreva o comportamento esperado e peça correção; repita os testes. | Mostre que a spec é critério de revisão, não enfeite documental. |
| 10:48–10:50 | Salve evidências e prepare `docs/spec.md`, `docs/prompts.md`, `docs/charter.md`. | Esses três documentos são exigências do hackathon; o OpenSpec não os produz automaticamente. |

**Antes da aula:** instale a CLI conforme a documentação oficial. A instalação via npm requer Node.js 22.12.0 ou superior: `npm install -g @fission-ai/openspec@latest`. No projeto, use `openspec init` e configure o agente escolhido. A invocação varia: a documentação usa `/opsx:explore` e `/opsx:propose` como nomes canônicos; Copilot no VS Code costuma expor `/opsx-explore` e `/opsx-propose`, enquanto Codex usa `$openspec-explore` e `$openspec-propose`. Siga o que o `init` mostrar e o autocomplete do chat. Comandos do chat vão **no chat**, não no terminal. Comandos `openspec ...` vão **no terminal**.

### 1. Explore — cole no chat do agente

```text
[Invoque o comando explore do OpenSpec instalado]
Vamos explorar o DoaCerto, um app Python de terminal para uma OSC. Ainda não implemente nem gere código. O pedido é “organizar necessidades de doações”: cadastrar produto (nome, estoque e meta), calcular falta, registrar doação e gerar relatório. Identifique ambiguidades sobre excedente, entradas inválidas, produto inexistente, identidade dos nomes, meta zero e presença de produtos completos no relatório. Para cada pergunta, apresente opções, impacto e uma recomendação. Mantenha o MVP sem banco, web, API, autenticação ou bibliotecas externas. Termine com decisões e pontos que ainda precisam de confirmação.
```

### 2. Propose — após decidir, cole no chat

```text
[Invoque o comando propose do OpenSpec instalado]
Crie a change `gerenciar-doacoes` para a capacidade `doacoes`. Use as decisões e a especificação de referência em DOACERTO_SPEC.md. Gere proposta, spec com cenários Dado/Quando/Então, design e tarefas no schema instalado. Não implemente ainda. Garanta que cada requisito tenha cenários de sucesso, erro ou limite relevantes. Se houver contradição ou decisão em aberto, sinalize antes de inventar uma regra. Use Python 3, biblioteca padrão e unittest; dados só em memória.
```

### 3. Review — peça uma revisão focada

```text
Revise `openspec/changes/gerenciar-doacoes/` contra DOACERTO_SPEC.md. Liste ambiguidades, requisitos sem cenário, cenários sem resultado verificável, regras contraditórias e tarefas sem teste. Corrija os artefatos mantendo os quatro itens do MVP. Não implemente código ainda. Depois execute `openspec validate gerenciar-doacoes --strict` e informe o resultado.
```

Se o perfil instalado expuser `update`, use o comando do agente para alterar artefatos após a revisão. Se expuser `verify`, use-o após a implementação; ele faz uma revisão adicional, mas não substitui `unittest` nem a conferência manual. `sync` e `archive` pertencem ao encerramento da change, depois da demonstração e da verificação.

### 4. Apply e testes — cole no chat

```text
[Invoque o comando apply do OpenSpec instalado]
Implemente somente a change `gerenciar-doacoes` revisada. Crie `app.py` e `tests/test_doacoes.py`; use apenas Python 3, biblioteca padrão e unittest. Faça as regras testáveis separadas da impressão do relatório. Cubra os cenários da spec e as fronteiras: estoque/meta zero, excedente, doação zero/negativa/fração, produto inexistente, duplicidade de nome e relatório com seções vazias. Rode `python3 -m unittest discover -s tests -v`, `python3 app.py` e `openspec validate gerenciar-doacoes --strict`. Marque tarefas somente quando verificadas. Relate comandos, resultados e qualquer diferença da spec. Não arquive a change.
```

### 5. Correção — se um teste ou saída divergir

```text
A implementação diverge do cenário [ID]: [resultado observado]. O resultado exigido pela spec é [resultado esperado]. Corrija a menor parte necessária, acrescente ou ajuste um teste que reproduza a falha e rode novamente todos os testes e a validação. Registre a evidência; não mude a regra da spec para acomodar um bug.
```

## Artefatos esperados no projeto Python

```text
doacerto/
├── openspec/
│   ├── config.yaml
│   └── changes/gerenciar-doacoes/
│       ├── proposal.md
│       ├── specs/doacoes/spec.md
│       ├── design.md
│       └── tasks.md
├── docs/
│   ├── spec.md
│   ├── prompts.md
│   └── charter.md
├── app.py
├── tests/test_doacoes.py
└── README.md
```

`DOACERTO_SPEC.md` neste repositório é o contrato de referência pronto para copiar. Cole seu conteúdo em `openspec/changes/gerenciar-doacoes/specs/doacoes/spec.md` **depois de inicializar e criar a change**, ou peça ao agente que o use como fonte ao gerar os artefatos. `docs/spec.md` pode conter uma versão legível e o link para a spec OpenSpec; `docs/prompts.md` registra prompts usados e ajustes; `docs/charter.md` registra problema, público, objetivos, escopo e decisões. Guarde saída dos testes e revisão como evidência do hackathon.

## Fontes oficiais

- [Instalação](https://github.com/Fission-AI/OpenSpec/blob/main/docs/installation.md)
- [Comandos do agente e variações por ferramenta](https://github.com/Fission-AI/OpenSpec/blob/main/docs/commands.md)
- [CLI e validação](https://github.com/Fission-AI/OpenSpec/blob/main/docs/cli.md)
- [Fluxos](https://github.com/Fission-AI/OpenSpec/blob/main/docs/workflows.md)
