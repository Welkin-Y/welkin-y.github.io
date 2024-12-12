---
title: "Lazyvim Noice Gtest"
date: 2024-12-12T22:14:17+09:00
draft: false
---

## Config for noice

In `~/.local/share/nvim/lazy/LazyVim/lua/lazyvim/plugins/ui.lua`  
add the following lines:

```lua
  {
    "folke/noice.nvim",
    event = "VeryLazy",
    opts = {
      cmdline = {
        view = "cmdline",
      },
      popupmenu = {
        backend = "cmp",
      },
      lsp = {
        --- original configuaration goes here
      }
  }
```

Because I am a traditional person used to vim command line at the bottom.  
Sadly this config will be overwritten after updating LazyVim.

## Config for neotest-gtest

It is interesting that neotest-gtest's command for mapping code to
executable files `:ConfigureGTest` does not show up in the command
line, if only config `~/.config/nvim/lua/plugins/tests.lua` like
that in the LazyVim tutorial at this point:

```lua
return {
  { "alfaix/neotest-gtest"},
  {
    "nvim-neotest/neotest",
    opts = { adapters = { "neotest-gtest" } },
  },
}
```

To make it work properly, append the following lines in  
`~/.config/nvim/lua/lazyvim/config/lazy.lua`:

```lua
--- Oigirnal configuration goes here
-- Otherwise :ConfigureGTest will not show as command
require("neotest").setup({
  adapters = {
    require("neotest-gtest").setup({}),
  },
})
```

Fortunately, this config will not be overwritten after updating LazyVim.

## Afterword

A long-awaited blog update.

Finally I decided to move to nvim. LazyVim really helps simplify
the customization procedure, while it is still somehow tricky in
some configuration. I tried neotest, for python it is pretty cool,
but it seems so far there are not any good test solution for cpp.
It took me a while before making noetest-gtest work, but it requires
**MANUALLY** matching executables and your code (WTF)
I also tried ctest but failed to get it to work :<
