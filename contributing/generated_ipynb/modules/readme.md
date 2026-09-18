# Módulos gerados (saída)

Os módulos `.ipynb` gerados pela *skill* `gerar-modulo-ipynb` a partir do material de
[`../../raw_content/`](../../raw_content/) ficam aqui, com o nome `module_NN.ipynb` (dois
dígitos: `module_07`, não `module_7`).

Antes de abrir a *pull request*, abra o notebook no Jupyter ou no Colab e rode **"Executar
tudo"**. Para validar estrutura e execução:

```bash
python .claude/skills/gerar-modulo-ipynb/scripts/validar_notebook.py \
  contributing/generated_ipynb/modules/module_NN.ipynb
```

Depois de aprovado na *pull request*, o autor move o notebook para
`bootcamps/<lang>/<bootcamp>/Módulos/` e publica as páginas do site. Passo a passo completo em
[`CONTRIBUTING.md`](../../../CONTRIBUTING.md).
