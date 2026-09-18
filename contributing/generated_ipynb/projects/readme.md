# Projetos práticos gerados (saída)

Os projetos práticos `.ipynb` gerados pela *skill* `gerar-projeto-pratico-ipynb` ficam aqui, com
o nome `project_NN.ipynb`, onde `NN` é o número do módulo correspondente (dois dígitos).

A fonte do projeto pode ser um módulo de [`../modules/`](../modules/), um módulo já publicado em
`bootcamps/<lang>/<bootcamp>/Módulos/`, material cru de [`../../raw_content/`](../../raw_content/)
ou uma combinação deles.

Antes de abrir a *pull request*, rode o notebook como um aluno o receberia (células entregues,
antes de escrever qualquer solução). Para validar:

```bash
python .claude/skills/gerar-projeto-pratico-ipynb/scripts/validar_projeto.py \
  contributing/generated_ipynb/projects/project_NN.ipynb
```

Depois de aprovado, o autor move o notebook para `bootcamps/<lang>/<bootcamp>/Projetos/`. Passo a
passo completo em [`CONTRIBUTING.md`](../../../CONTRIBUTING.md).
