# Contributing — área de criação de conteúdo

Esta é a pasta onde o conteúdo do curso é **criado**, por qualquer pessoa que queira contribuir.
O passo a passo completo está em [`CONTRIBUTING.md`](../CONTRIBUTING.md), na raiz do projeto.

```
contributing/
├── raw_content/          você coloca aqui seu material cru (entrada)
└── generated_ipynb/
    ├── modules/          módulos .ipynb gerados      (skill gerar-modulo-ipynb)
    └── projects/         projetos práticos gerados   (skill gerar-projeto-pratico-ipynb)
```

Resumo do fluxo:

1. coloque seus arquivos em [`raw_content/`](raw_content/);
2. peça ao Claude para gerar o módulo (e, se quiser, o projeto prático) — ele usa as *skills*
   em `.claude/skills/` e salva em [`generated_ipynb/`](generated_ipynb/);
3. rode o notebook do início ao fim para revisar;
4. abra uma *pull request*.

Nada aqui vai para o site direto: depois da aprovação, o autor do curso move os notebooks para
`bootcamps/<lang>/<bootcamp>/` e publica as páginas.
