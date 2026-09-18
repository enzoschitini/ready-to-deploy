# Material cru (entrada)

Coloque aqui os arquivos "crus" do conteúdo que você quer contribuir: anotações, um notebook
antigo, `.md`, `.txt`, exemplos em `.py`, um PDF, uma lista de tópicos, um CSV de exemplo. Não
precisa formatar nem organizar — é esse material que a *skill* `gerar-modulo-ipynb` vai reescrever
no padrão do curso.

Duas formas de organizar, as duas funcionam:

```
raw_content/
├── module_10.ipynb              um arquivo só, já com o número do módulo
└── testes-automatizados/        ou uma subpasta com vários arquivos do mesmo assunto
    ├── anotacoes.md
    ├── exemplos.py
    └── roteiro-da-aula.txt
```

Com vários arquivos, prefira a subpasta: fica claro o que faz parte da mesma contribuição.

**Não coloque aqui** material protegido por direitos autorais de outra pessoa ou escola, dados
pessoais, credenciais, nem arquivos binários grandes.

Depois, peça ao Claude: *"gere o módulo 10 a partir dos arquivos em
`contributing/raw_content/testes-automatizados/`"*. Detalhes em
[`CONTRIBUTING.md`](../../CONTRIBUTING.md).
