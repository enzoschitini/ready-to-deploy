# Como contribuir com o Ready To Deploy

O **Ready To Deploy** é um curso de Python para iniciantes, do zero ao primeiro deploy. Cada
módulo do curso nasce como um **notebook Jupyter** (`.ipynb`) e só depois é publicado como
páginas do site.

Você pode contribuir com **conteúdo**: um módulo novo, uma reescrita de um módulo existente ou
um projeto prático. Não é preciso saber mexer no site — o site é a etapa final, e ela é feita
pelo autor do curso depois da aprovação.

O caminho é sempre o mesmo:

```
clonar o repositório
      ↓
colocar seu material "cru" em  contributing/raw_content/
      ↓
pedir ao Claude para gerar     →  contributing/generated_ipynb/modules/module_NN.ipynb
(com as skills deste repo)     →  contributing/generated_ipynb/projects/project_NN.ipynb
      ↓
revisar o notebook você mesmo (rodar do início ao fim)
      ↓
commit + pull request  →  o autor revisa, aprova e publica no site
```

---

## 1. O que você precisa

| Requisito | Para quê |
| --- | --- |
| Git e uma conta no GitHub | clonar o repositório e abrir a *pull request* |
| [Claude Code](https://claude.com/claude-code) | rodar as *skills* de geração que já vêm no repositório (`.claude/skills/`) |
| Python 3.10 ou mais novo | os validadores executam todas as células do notebook |
| Jupyter, VS Code ou Google Colab | abrir e revisar o notebook gerado |

Você não precisa instalar dependências do site (`node_modules/`) para contribuir com conteúdo.

## 2. Clone e crie um branch

Se você não tem permissão de escrita no repositório, faça um *fork* primeiro e clone o seu
*fork*:

```bash
git clone https://github.com/<seu-usuario>/ready-to-deploy.git
cd ready-to-deploy
git checkout -b conteudo/modulo-10-testes
```

Use um nome de branch que diga o que você está trazendo: `conteudo/modulo-10-testes`,
`conteudo/projeto-modulo-04`, `conteudo/reescreve-modulo-02`.

## 3. Coloque seu material cru em `contributing/raw_content/`

**Material cru** é tudo o que você já tem sobre o assunto, sem nenhuma formatação especial:
anotações de aula, um notebook antigo, um `.md`, um `.txt`, exemplos em `.py`, um PDF, uma
lista de tópicos, um CSV de exemplo. Não precisa estar bonito nem organizado — é justamente
esse material que a *skill* vai reescrever no padrão do curso.

Duas formas de organizar, as duas funcionam:

```
contributing/raw_content/
├── module_10.ipynb                  ← um arquivo só, já com o número do módulo
└── testes-automatizados/            ← ou uma subpasta com vários arquivos do mesmo assunto
    ├── anotacoes.md
    ├── exemplos.py
    └── roteiro-da-aula.txt
```

Se o material tem vários arquivos, **prefira a subpasta**: fica claro o que faz parte da mesma
contribuição.

**Não coloque aqui:** material protegido por direitos autorais de outra pessoa ou escola
(slides, capítulos de livro, apostilas de cursos pagos), dados pessoais, credenciais, nem
arquivos binários grandes (vídeos, imagens de centenas de MB). Você precisa ter o direito de
contribuir com o que subir — a contribuição entra no repositório sob a licença do projeto
(veja [LICENSE](../LICENSE)).

## 4. Gere o módulo com o Claude

Abra o Claude Code na raiz do repositório e peça, em português, algo como:

> Gere o módulo 10 a partir dos arquivos em `contributing/raw_content/testes-automatizados/`.

O Claude vai usar a *skill* **`gerar-modulo-ipynb`**, que já está no repositório. Ela:

1. lê **todos** os arquivos crus que você indicou;
2. reescreve o conteúdo no padrão oficial do curso (cabeçalho, tabela de Tópicos, seções
   numeradas, dicas, avisos, resumo final), corrigindo erros e expandindo o que está raso;
3. salva em **`contributing/generated_ipynb/modules/module_NN.ipynb`**;
4. roda o validador, que executa **todas** as células e confere a estrutura, até dar
   `APROVADO`.

Se o número do módulo não estiver claro, o Claude vai perguntar em que bootcamp e em que
posição do curso ele entra. Se você não souber, diga isso: o autor pode renumerar na revisão.

O conteúdo é escrito em **português do Brasil**. A versão italiana do curso é feita depois pelo
autor — você não precisa traduzir nada.

Se quiser rodar o validador por conta própria depois de editar o notebook à mão:

```bash
python .claude/skills/gerar-modulo-ipynb/scripts/validar_notebook.py \
  contributing/generated_ipynb/modules/module_10.ipynb
```

## 5. Gere o projeto prático (opcional, mas muito bem-vindo)

Cada módulo tem um **projeto prático**: um notebook onde o aluno pratica os conceitos dentro de
uma história — uma empresa fictícia, colegas pedindo coisas no chat, missões com resultado
esperado, dicas e desafios extras.

Peça ao Claude:

> Gere o projeto prático do módulo 10.

Ele vai usar a *skill* **`gerar-projeto-pratico-ipynb`**, que aceita como fonte:

- um módulo que você acabou de gerar, em `contributing/generated_ipynb/modules/`;
- um módulo **já publicado**, em `bootcamps/<lang>/<bootcamp>/Módulos/module_NN.ipynb` (use
  isso para criar o projeto de um módulo que já está no ar);
- **material cru** de `contributing/raw_content/`, quando ainda não existe módulo;
- **os dois juntos** — por exemplo, o módulo mais um arquivo cru com os dados ou o cenário que
  você quer usar.

O resultado vai para **`contributing/generated_ipynb/projects/project_NN.ipynb`** (`NN` é o
número do módulo correspondente).

Esse projeto tem uma garantia embutida: cada "Resultado esperado" do enunciado é conferido
contra uma solução que roda de verdade. Se um número não bater, o notebook **não** é gravado —
então nenhum valor do enunciado é chute.

```bash
python .claude/skills/gerar-projeto-pratico-ipynb/scripts/validar_projeto.py \
  contributing/generated_ipynb/projects/project_10.ipynb
```

## 6. Revise você mesmo antes de abrir a PR

O validador garante que o notebook roda e tem a estrutura certa. Ele não garante que o conteúdo
é bom — isso é o seu papel. Antes de abrir a *pull request*, confira:

- [ ] Abri o notebook no Jupyter/Colab e rodei **"Executar tudo"** sem nenhum erro.
- [ ] A tabela de **Tópicos** bate com as seções do notebook, na mesma ordem.
- [ ] As explicações dizem **por que** algo funciona, não só como — um iniciante entenderia.
- [ ] Os nomes de variáveis estão em **português**, `snake_case`, descritivos
      (`quantidade_vendas`, não `qtd`).
- [ ] Não sobrou nada do material original que não seja seu: logo, nome de outra escola,
      "Caderno de Aula", links pessoais antigos.
- [ ] Nada fora do escopo: o módulo não usa conceitos que o curso só ensina depois dele.
- [ ] No projeto prático: rodei como um aluno (células entregues, antes de escrever qualquer
      coisa) e todos os enunciados fazem sentido.

## 7. Commit e pull request

Suba **só** o que faz parte da sua contribuição: os arquivos crus e os notebooks gerados.

```bash
git add contributing/raw_content/testes-automatizados \
        contributing/generated_ipynb/modules/module_10.ipynb \
        contributing/generated_ipynb/projects/project_10.ipynb
git commit -m "conteudo: modulo 10 - testes automatizados"
git push origin conteudo/modulo-10-testes
```

Depois abra a *pull request* para o branch `main`, descrevendo:

1. **O que é** a contribuição (módulo novo, reescrita, projeto prático) e a que bootcamp e
   posição do curso ela se destina.
2. **De onde veio o material cru** — é seu, é original, você tem o direito de contribuir com ele.
3. **O que o validador disse** (`APROVADO`) e que você rodou o notebook do início ao fim.
4. **Dúvidas e decisões** que o autor deve revisar: numeração, algo que ficou de fora, um tópico
   que você não teve certeza se cabia ali.

Se o Claude gerou o notebook, o relatório final que ele te deu na conversa é uma ótima base para
essa descrição.

## 8. O que acontece depois

1. O autor do curso revisa o conteúdo na *pull request*.
2. Se precisar, ele comenta e você ajusta no mesmo branch (basta novos commits).
3. Aprovado e com merge feito, o notebook sai de `contributing/` e vai para
   `bootcamps/<lang>/<bootcamp>/Módulos|Projetos/`, que é a pasta do conteúdo oficial.
4. O autor publica o módulo no site (páginas HTML + cadastro nos JSONs de `course_content/`,
   com a *skill* `gerar-modulo-html`) e cuida da versão italiana.

Ou seja: a sua parte termina na *pull request*. Você não precisa gerar HTML, mexer em JSON nem
traduzir nada.

---

## Mapa do repositório

| Pasta | O que é | Você mexe? |
| --- | --- | --- |
| `contributing/raw_content/` | material cru, a entrada das *skills* | **Sim** — é onde você coloca seus arquivos |
| `contributing/generated_ipynb/modules/` | módulos `.ipynb` gerados | **Sim** — saída da *skill* de módulo |
| `contributing/generated_ipynb/projects/` | projetos práticos `.ipynb` gerados | **Sim** — saída da *skill* de projeto |
| `.claude/skills/` | as *skills* de geração (instruções, construtores e validadores) | Só se quiser melhorar o próprio processo |
| `bootcamps/<lang>/<bootcamp>/` | notebooks **oficiais**, já aprovados (`Módulos/`, `Projetos/`) | **Não** — leitura apenas; o autor move o conteúdo para cá |
| `course_content/<lang>/` | o curso publicado: páginas HTML e os JSONs (`bootcamps.json`, `modules.json`, `lessons.json`) | **Não** — gerado pelo autor na publicação |
| `application_content/`, `pages/`, `assets/`, `index.html` | o site em si | **Não** para contribuições de conteúdo |

## Convenções rápidas

- **Nomes de arquivo:** `module_NN.ipynb` para módulos, `project_NN.ipynb` para projetos, sempre
  com dois dígitos (`module_07`, não `module_7`).
- **Um módulo por pull request.** Módulo + o projeto prático dele na mesma PR é ótimo; dois
  módulos diferentes, melhor separar.
- **Idioma do conteúdo:** português do Brasil.
- **Público:** iniciante absoluto. Termos em inglês em *itálico*, código entre `crases`.
- **Nunca sobrescreva** um notebook de `bootcamps/` nem o material cru de outra pessoa.

## Melhorando as skills

As *skills* em `.claude/skills/` são parte do projeto e também aceitam contribuição: uma regra
que faltava, um validador mais rigoroso, uma instrução ambígua. Se for mudar uma *skill*,
explique na *pull request* qual problema real de conteúdo aquilo evita — e, de preferência, gere
um módulo com a versão nova para mostrar a diferença.

## Dúvidas

Abra uma *issue* descrevendo o que você quer contribuir antes de investir muito tempo,
especialmente se for um módulo novo: assim dá para alinhar tema, numeração e escopo primeiro.

Obrigado por contribuir. 🚀
