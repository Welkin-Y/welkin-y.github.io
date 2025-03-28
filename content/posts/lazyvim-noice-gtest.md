---
title: "Lazyvim Noice Gtest"
date: 2024-12-12T22:14:17+09:00
draft: false
---

## Config for noice

In nvim config foler, by default `~/.config/nvim/lua/plugins`, 
create config file `ui.lua`, and add the following lines:

```lua
return {
  -- other ui configs
  {
    "folke/noice.nvim",
    opts = {
      cmdline = {
        view = "cmdline",
      },
      popupmenu = {
        backend = "cmp",
      },
    }
  },
}
```

Because I am a traditional person used to vim command line at the bottom.  
Sadly this config will be overwritten after updating LazyVim.

### cmd backend Tab key not working

As Described in this [issue](https://github.com/folke/noice.nvim/issues/958), 
tab key only works before cmp plugin initialized. A temporary solution 
could be modify `~/.local/share/nvim/lazy/nvim-cmp/lua/cmp/utils/keymap.lua` 
as follows:

```
---Register keypress handler.
keymap.listen = function(mode, lhs, callback)
  if mode == 'c' and lhs == '<Tab>' then
    return
  end
  -- Original implementation continues
end
```

It will disable keymapping for tab key in command mode.


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
