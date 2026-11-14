# SDD Lab — versão portátil

Este pacote contém o laboratório existente (oficina e cupom). O conteúdo ainda não foi adaptado ao DoaCerto.

## Abrir localmente

Na pasta com index.html, execute:

    python3 -m http.server 8000

Abra http://localhost:8000. Não abra o HTML diretamente com file://, pois o site usa módulos JavaScript.

## Publicar gratuitamente no GitHub Pages

1. Crie um repositório público no GitHub (por exemplo sdd-lab).
2. Extraia este ZIP e coloque os arquivos na raiz do repositório. index.html deve ficar na raiz.
3. Faça commit e push na branch main.
4. Abra Settings > Pages.
5. Em Source, escolha Deploy from a branch.
6. Escolha main e /(root), depois Save.
7. Quando a implantação terminar, abra a URL mostrada pelo GitHub.

O arquivo .nojekyll já está incluído. A autenticação da hospedagem anterior não faz parte deste código.

## Estrutura

- index.html: estrutura da página.
- style.css: estilos.
- core.mjs: etapas, exemplos, regras de revisão e artefatos OpenSpec.
- app.mjs: navegação, edição, simulações, importação/exportação.

As sessões ficam no localStorage de cada navegador. Elas não são compartilhadas entre pessoas ou dispositivos. Baixe uma sessão JSON no site para transportá-la. O site não chama uma API de IA e não executa Python ou a CLI do OpenSpec.

## Adaptar para DoaCerto

Substitua os exemplos e as regras originais de core.mjs e as simulações e explicações de app.mjs. Não basta alterar o nome na capa: requisitos, perguntas, cenários, design, relatórios e simulações precisam seguir o mesmo domínio. Use uma chave diferente de localStorage para que sessões antigas não se misturem.

O aplicativo Python do DoaCerto deve permanecer em um projeto separado, com Python 3, biblioteca padrão e unittest. Não é necessário hospedá-lo para demonstrar a aplicação de terminal.

## Referências

https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site
https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
