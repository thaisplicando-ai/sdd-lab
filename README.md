# SDD Lab — DoaCerto

Laboratório interativo de Spec-Driven Development com OpenSpec. **DoaCerto** é o exemplo principal; cupom de desconto permanece como exemplo secundário. O laboratório web ajuda a explorar requisitos, escrever regras e critérios de aceite, visualizar artefatos, revisar e experimentar um modelo didático.

## Abrir localmente

Na pasta deste repositório:

```sh
python3 -m http.server 8000
```

Abra `http://localhost:8000`. O site usa módulos JavaScript, portanto deve ser servido por HTTP. Ele salva sessões no `localStorage` do navegador; use “Salvar sessão” para transportar uma cópia JSON.

## Para o webinar e o VS Code

- [DOACERTO_VSCODE.md](DOACERTO_VSCODE.md): roteiro de 30 minutos, decisões, instalação, comandos OpenSpec, prompts para o agente, revisão, testes e evidências do hackathon.
- [DOACERTO_SPEC.md](DOACERTO_SPEC.md): especificação de referência completa, pronta para copiar para `openspec/changes/gerenciar-doacoes/specs/doacoes/spec.md` no projeto Python.

O site exporta **rascunhos** dos artefatos OpenSpec conforme as edições feitas na interface. Ele não chama uma API de IA, não executa a CLI do OpenSpec e não roda Python. O projeto Python `doacerto/` é criado separadamente no VS Code, com Python 3, biblioteca padrão e `unittest`.

## Estrutura deste repositório

- `index.html`: estrutura da página.
- `style.css`: estilos.
- `core.mjs`: etapas, exemplos, revisão e artefatos.
- `app.mjs`: navegação, edição, simulações e exportação.

## Publicar no GitHub Pages

Coloque os arquivos na raiz de um repositório público e ative **Settings → Pages → Deploy from a branch → main → /(root)**. O `index.html` deve permanecer na raiz.

## Crie seu próprio projeto no laboratório

Escolha **Meu projeto** na primeira etapa. Escreva o pedido, quem será atendido, o problema e o objetivo. Na etapa **Explorar requisitos**, marque o que entra no escopo e preencha **Nome do requisito** e **O que o sistema deve garantir?**; depois responda às perguntas. Na etapa seguinte, refine a redação da spec. Escreva cenários de sucesso, erro e limite. Em **Artefatos OpenSpec**, confira a proposta, a spec, o design e as tarefas; baixe o ZIP. Em **Revisar e experimentar**, selecione um cenário e compare o resultado esperado com o que observou em um protótipo ou teste.

O botão **Entenda o OpenSpec** explica o papel de cada arquivo, `SHALL`/`MUST` e os comandos do agente. A comparação do projeto próprio é uma revisão guiada: a página não interpreta regras livres nem executa a implementação. Leve o ZIP para um repositório inicializado com OpenSpec, valide os artefatos com a CLI e rode os testes do produto.
