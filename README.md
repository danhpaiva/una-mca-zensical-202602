# una-mca-zensical-202602

Site de apoio da disciplina **Matemática Computacional Aplicada (UNA)**: aulas, listas de exercícios e o cronograma da turma 2026.2. O conteúdo é escrito em Markdown e publicado como site estático via [Zensical](https://zensical.org), com deploy automático no GitHub Pages.

## Estrutura de pastas

```
.
├── docs/                    # Todo o conteúdo publicado (site_dir de origem)
│   ├── index.md             # Página inicial
│   ├── setup.md             # Guia do professor (configuração/publicação)
│   ├── ferramentas.md       # Ferramentas de apoio
│   ├── assets/               # Imagens, logo, favicon
│   ├── javascripts/          # Configuração do MathJax
│   ├── aulas/                # Aulas 01–12 (conteúdo base da disciplina)
│   ├── listas/                # Listas de exercícios 01–12
│   └── turma-202602/          # Cronograma e aulas específicas da turma 2026.2
├── mkdocs.yml                # Configuração do site (tema, navegação, extensões)
├── requirements.txt          # Dependências Python (Zensical)
├── .github/
│   ├── workflows/ci.yml      # Build + deploy automático no GitHub Pages
│   └── dependabot.yml        # Atualização automática de dependências
├── site/                     # Build gerado (não versionado, ver .gitignore)
└── LICENSE
```

> Observação: o arquivo de configuração continua se chamando `mkdocs.yml` — é o nome de arquivo que o próprio Zensical lê (com compatibilidade retroativa ao formato do MkDocs), não uma sobra da ferramenta antiga.

## Como rodar localmente

Pré-requisito: Python 3.

```bash
git clone https://github.com/danhpaiva/una-mca-zensical-202602.git
cd una-mca-zensical-202602

python -m venv venv
# Windows:
.\venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate

pip install -r requirements.txt

zensical serve
```

Acesse `http://127.0.0.1:8000` no navegador. O servidor recarrega automaticamente a cada alteração nos arquivos em `docs/`.

Para gerar o build estático manualmente (o mesmo comando usado no CI):

```bash
zensical build --strict
```

O resultado é gravado em `site/` (pasta ignorada pelo Git — é sempre gerada, nunca editada à mão).

## Publicação (GitHub Pages)

O deploy é automático via GitHub Actions ([.github/workflows/ci.yml](.github/workflows/ci.yml)), mas depende de duas configurações manuais que só precisam ser feitas uma vez, em **Settings** do repositório no GitHub:

1. **Settings → Pages → Build and deployment → Source**: selecione **"GitHub Actions"** (não deixe em "Deploy from a branch").
2. **Settings → Actions → General → Workflow permissions**: selecione **"Read and write permissions"** e salve.

Sem esses dois ajustes, o workflow falha ao publicar ou o deploy fica preso em "Deployment failed, try again later".

### Fluxo de branches do CI

- **Push em `develop`**: o workflow roda `zensical build --strict` apenas para validar que o site builda sem erros. Nada é publicado.
- **Push em `main`**: além de validar o build, o site é enviado como artefato do Pages e publicado em produção (job `deploy`).

Isso permite testar mudanças em `develop` (ou em uma branch de feature aberta contra ela) antes de promover para `main` e publicar de fato.

## Como personalizar

Praticamente toda a configuração do site vive em [mkdocs.yml](mkdocs.yml):

- **Nome da disciplina**: chave `site_name`.
- **Menu lateral / navegação**: seção `nav:` — cada entrada mapeia um título a um arquivo Markdown em `docs/`.
- **Cores e tema**: bloco `theme.palette` — troque `primary`/`accent` (ex.: `indigo`, `red`, `deep purple`).
- **Extensões Markdown** (admonitions, abas, tabelas, mermaid, MathJax): bloco `markdown_extensions`.
- **Botão "editar esta página"**: usa `repo_url` + `edit_uri`, ambos no topo do arquivo.

Consulte os comentários dentro do próprio `mkdocs.yml` para detalhes sobre o que ainda não é suportado nativamente pelo Zensical (ex.: `exclude_docs`) e como contornar.

## Licença

Distribuído sob a licença MIT — veja [LICENSE](LICENSE).
