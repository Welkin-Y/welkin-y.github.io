---
title: "Noice Popupmenu Message Customization"
date: 2024-12-14T00:10:16+09:00
draft: false
---

## Change opacity of noice popupmenu with cmp backend

To avoid strange rendering of popupmenu with background code,
it is better to make the popupmenu untransparent.
Inside `~/.config/nvim/lua/config/lazy.lua`, add the following code:

```lua
vim.cmd("hi Pmenu blend=0")
```

## Change position of noice message

Noice message is displayed at the right upper corner of the window by default.
If you are using a traditional cmdline at the bottom, it will be distracting
to type command then check the message at the top.

Currently, there are old solutions for noice using nvim-notify backend,
 such as follows:

```lua
require("nofity").setup({
  top_down = false,
})
```

However, it only places the message at right bottom corner, and Lazyvim
at the time of writing this post has switched to the snacks.nvim backend.

In `~/.local/share/nvim/lazy/Lazyvim/lua/lazyvim/plugin/ui.lua`, add in snacks setup:

```lua
return {
  -- Other configurations
  {
    "folke/snacks.nvim",
    opts = {
      dashboard = {
        -- Original configurations
      },
      -- Add opts for snack.notifier
      notifier = {
        top_down = false,
        margin = { top = 1, right = 2 ^ 63 - 1, bottom = 0 },
      },
    },
  },
}
```

This will place the message at the left bottom corner of the window.

To check if your noice message is using snacks.notifier backend,
check in `../noice.nvim/lua/noice/config/views.lua`, and see if the following

```lua
  notify = {
    backend = { "snacks", "notify" },
    fallback = "mini",
    format = "notify",
    replace = false,
    merge = false,
  },
```

## Afterword

In `../snacks.nvim/lua/notifier.lua`, you can find the default opts:

```lua
---@class snacks.notifier.Config
---@field enabled? boolean
---@field keep? fun(notif: snacks.notifier.Notif): boolean # global keep function
local defaults = {
  timeout = 3000, -- default timeout in ms
  width = { min = 40, max = 0.4 },
  height = { min = 1, max = 0.6 },
  -- editor margin to keep free. tabline and statusline are taken into account automatically
  margin = { top = 0, right = 1, bottom = 0 },
  padding = true, -- add 1 cell of left/right padding to the notification window
  sort = { "level", "added" }, -- sort by level and time
  -- minimum log level to display. TRACE is the lowest
  -- all notifications are stored in history
  level = vim.log.levels.TRACE,
  icons = {
    error = " ",
    warn = " ",
    info = " ",
    debug = " ",
    trace = " ",
  },
  keep = function(notif)
    return vim.fn.getcmdpos() > 0
  end,
  ---@type snacks.notifier.style
  style = "compact",
  top_down = true, -- place notifications from top to bottom
  date_format = "%R", -- time format for notifications
  -- format for footer when more lines are available
  -- `%d` is replaced with the number of lines.
  -- only works for styles with a border
  ---@type string|boolean
  more_format = " ↓ %d lines ",
  refresh = 50, -- refresh at most every 50ms
}
```

And the logic used for the message window position:

```lua
function N:layout()
  --- other parts
          notif.win.opts.row = notif.layout.top - 1
          notif.win.opts.col = vim.o.columns - notif.layout.width - self.opts.margin.right
  --- other parts
end
```
