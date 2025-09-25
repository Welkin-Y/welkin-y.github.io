---
title: "Nvim Python Lsp"
date: 2025-09-25T20:45:59+08:00
draft: false
---

> [!NOTE]
> This is a post for a restricted environment.
> And I hope that you will never encounter those situations.

- Special thanks to [GPT5](https://chatgpt.com/)

- neovim version 0.4.3 (WTF 0.4.3 in 2025)
- cpu: arm64
- os: some ubuntu(?) starts with K

## Download python-lsp-server

- python 3.8 (= =) in 2025
- Fine

```bash
pip download --python-version 3.8 --platform \
manylinux2014_aarch64 \
--only-binary=:all: \
--no-deps 
-d ./python-lsp-server \
python-lsp-server
```

## Download vim-lsp

```bash
git clone depth=1 https://github.com/prabirshrestha/vim-lsp.git
```

## From pdf to tgz

- Do not use clip board, b64 strings can be corrupted
- Why pdf? Well, ...

```bash
pdftotext file.pdf | base64 -d > tmp.tgz
tar -xvzf tmp.tgz
```

- or if base64 does not work properly even with `-i` flag

```python
#! /bin/python3
import base64
with open("tmp.txt", "r") as f:
    with open("tmp.tgz", "wb") as out_f:
        out_f.write(base64.b64decode(b64_str))
```

## Install python-lsp-server

```bash
pip install --no-index --find-links ./python-lsp-server *.whl
```

- Do not forget to add path to python-lsp-server to PATH

## python-lsp-server config

- edit `~/.config/pylsp/config.toml`

```toml
[plugins]
pyflakes.enabled = true
pycodestyle.enabled = true
mccabe.enabled = true
rope.enabled = true
mypy.enabled = true
isort.enabled = true
flake8.enabled = true

[plugins.pycodestyle]
maxLineLength = 120 # optional, still enforce 120 chars if needed
]
```

## vim-lsp config

- put vim-lsp at `~/.config/nvim/pack/plugins/start/`
- add in `~/.config/nvim/init.vim`

```vim
if executable('pylsp')
  augroup lsp_python
    autocmd!
    autocmd User lsp_setup call lsp#register_server({
          \ 'name': 'pylsp',
          \ 'cmd': {server_info->['pylsp']},
          \ 'whitelist': ['python'],
          \ })
    autocmd FileType python call lsp#enable()
  augroup END
endif

" Some Key binds
augroup lsp_python_maps
    autocmd!
    " Only map gd after vim-lsp is ready for Python buffers
    autocmd FileType python nnoremap <buffer> gd :LspDefinition<CR>
    autocmd FileType python nnoremap <buffer> gr :LspReferences<CR>
    autocmd FileType python nnoremap <buffer> <leader>rn :LspRename<CR>
    autocmd FileType python nnoremap <buffer> <leader>ca :LspCodeAction<CR>
    autocmd FileType python nnoremap <buffer> <leader>cf :LspDocumentFormat<CR>
    autocmd FileType python nnoremap <buffer> = :LspDocumentRangeFormat<CR>
augroup END
```
