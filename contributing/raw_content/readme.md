# Raw content (input)

Place here the "raw" files for the content you want to contribute: lecture notes, an old
notebook, `.md`, `.txt`, examples in `.py`, a PDF, a list of topics, a sample CSV. It doesn't
need to be formatted or organized — this is exactly the material the `gerar-modulo-ipynb`
*skill* will rewrite to the course's standard.

Two ways to organize it, both work:

```
raw_content/
├── module_10.ipynb              a single file, already named with the module number
└── automated-tests/             or a subfolder with several files on the same topic
    ├── notes.md
    ├── examples.py
    └── lesson-outline.txt
```

With several files, prefer the subfolder: it makes clear what belongs to the same contribution.

**Don't put here** material protected by someone else's or another school's copyright, personal
data, credentials, or large binary files.

Then ask Claude: *"generate module 10 from the files in
`contributing/raw_content/automated-tests/`"*. Details in
[`CONTRIBUTING.md`](../../CONTRIBUTING.md).
