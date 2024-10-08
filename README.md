# hot-reload.nvim
Reload your neovim config on the fly! It works with any lua file.

![screenshot_2024-09-23_20-45-38_129993510](https://github.com/user-attachments/assets/fc0301b3-7983-438e-b82e-024578c5a16a)

<div align="center">
  <a href="https://discord.gg/ymcMaSnq7d" rel="nofollow">
      <img src="https://img.shields.io/discord/1121138836525813760?color=azure&labelColor=6DC2A4&logo=discord&logoColor=black&label=Join the discord server&style=for-the-badge" data-canonical-src="https://img.shields.io/discord/1121138836525813760">
    </a>
</div>

## Table of contents

- [Why](#why)
- [How to install](#how-to-install)
- [Available options](#available-options)
- [Example of a real config](#example-of-a-real-config)
- [FAQ](#faq)

## Why
* The main case of use is to re-apply your Neovim config without having to close and re-open Neovim.
* But you can use it to load any lua file.

### Warning
Make sure the files you specify on `reload_files` are actually suitable to be hot-reloaded.

* **Example**: If you hot-reload a file that create autocmds, be aware hot-reloading the file won't delete any previously loaded autocmd. Same thing for highlights, variables, etc.

## How to install
In the example we use the lazy package manager

```lua
{
  "Zeioth/hot-reload.nvim",
  dependencies = "nvim-lua/plenary.nvim",
  event = "BufEnter",
  opts = {}
}
```

## Available options
All options described here are 100% optional and you don't need to define them to use this plugin.

| Name                | Default value  | Description                                                                                                                                                          |
|---------------------|----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **reload_files**         | `string[]`       | Table of file paths with the files you want to hot-reload.  |
| **reload_callback**          | `function() end`          | Optional function where you can specify things to do after the files have hot-reloaded. See example below. |
| **reload_all**     | `true`          | If true, all files in `reload_files` will hot-reload every time you write any of the files included in `reload_files`, in order. If false, only the file you write will be reloaded. |
| **notify** | `true`| If true, a notification will be displayed when a file is hot-reloaded. |
| **event** | `BufWritePost`| Event that trigger the hot-reload. Don't change this unless you know what you are doing. |

## Example of a real config

```lua
-- hot-reload.nvim [config reload]
-- https://github.com/Zeioth/hot-reload.nvim
{
  "Zeioth/hot-reload.nvim",
  dependencies = { "nvim-lua/plenary.nvim" },
  event = "BufEnter",
  opts = function()
    local config_dir = vim.fn.stdpath("config") .. "/lua/base/"
    return {
      -- Files to be hot-reloaded when modified.
      reload_files = {
        config_dir .. "1-options.lua",
        config_dir .. "4-mappings.lua"
      },
      -- Things to do after hot-reload trigger.
      reload_callback = function()
        vim.cmd(":silent! colorscheme " .. vim.g.default_colorscheme) -- nvim     colorscheme reload command.
        vim.cmd(":silent! doautocmd ColorScheme")                     -- heirline colorscheme reload event.
      end
    }
  end
},
```

## 🌟 Get involved
One of the biggest challenges NormalNvim face is marketing. So share the project and tell your friends!
[![Stargazers over time](https://starchart.cc/Zeioth/hot-reload.nvim.svg?variant=adaptive)](https://starchart.cc/Zeioth/hot-reload.nvim)

## Fix a bug and send a PR to appear as contributor

<a href="https://github.com/NormalNvim/NormalNvim/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=NormalNvim/NormalNvim" />
</a>

## Credits
This GPL3 Neovim plugin has been developed for NormalNvim. It's based on the GPL3 hot reload snippet from AstroNvim v3. So please support both projects if you enjoy this plugin.

## FAQ
* **Can I use this on any Neovim distro?** `Yes`. It is distro agnostic, as all the plugins released by [NormalNvim](https://normalnvim.github.io/).
* **Can I use this plugin to reload my plugins too?** `Lazy` will reload the plugins automatically when you modify their config. But if you are using any other plugin manager, you might want to.
 
## Roadmap
* It would be a cool idea to allow specifying a callback per file to hot-reload. But it's unclear how many people would actually use this.
* It would be cool (but not necessary) to have an option `reload_directories`.
