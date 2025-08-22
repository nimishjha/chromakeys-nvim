# ChromaKeys

## Create color schemes within Neovim using only the keyboard

Generate entire color schemes with one keypress, or set each highlight group's color separately.

## Installation

Place `chromakeys_nvim.lua` in your `~/.config/nvim/lua` directory.

## Usage

Add this to your init.lua:

```
local chromakeys = require("chromakeys_nvim")

local keymapOptions = { noremap=true, silent=true }

vim.keymap.set('n', '<A-Left>',  chromakeys.previousScope)
vim.keymap.set('n', '<A-Right>', chromakeys.nextScope)
vim.keymap.set('n', '<A-r>',     chromakeys.randomizeColor)
vim.keymap.set('n', '<A-1>',     chromakeys.previousColorFunction)
vim.keymap.set('n', '<A-2>',     chromakeys.nextColorFunction)
vim.keymap.set('n', '<A-3>',     chromakeys.generateColorScheme)
vim.keymap.set('n', '<F8>',      chromakeys.generateColorScheme)
vim.keymap.set('n', '<A-g>',     chromakeys.generateColorScheme)
vim.keymap.set('n', '<A-k>',     chromakeys.previousColorScheme)
vim.keymap.set('n', '<A-l>',     chromakeys.nextColorScheme)
vim.keymap.set('n', '<F5>',      chromakeys.increaseHue)
vim.keymap.set('n', '<S-F5>',    chromakeys.decreaseHue)
vim.keymap.set('n', '<F6>',      chromakeys.increaseSaturation)
vim.keymap.set('n', '<S-F6>',    chromakeys.decreaseSaturation)
vim.keymap.set('n', '<F7>',      chromakeys.increaseLightness)
vim.keymap.set('n', '<S-F7>',    chromakeys.decreaseLightness)
vim.keymap.set('n', '<F9>',      chromakeys.increaseHueLarge)
vim.keymap.set('n', '<S-F9>',    chromakeys.decreaseHueLarge)
vim.keymap.set('n', '<F10>',     chromakeys.increaseSaturationLarge)
vim.keymap.set('n', '<S-F10>',   chromakeys.decreaseSaturationLarge)
vim.keymap.set('n', '<F11>',     chromakeys.increaseLightnessLarge)
vim.keymap.set('n', '<S-F11>',   chromakeys.decreaseLightnessLarge)
vim.keymap.set('n', '<F2>',      chromakeys.saveTheme)

vim.api.nvim_create_user_command('CkselectHighlightGroup', function(opts) chromakeys.setScope(opts.args)   end, { nargs = 1 })
vim.api.nvim_create_user_command('CkshowHighlightGroups',  function()     chromakeys.showHighlightGroups() end, {})
vim.api.nvim_create_user_command('Ckdebug',                function()     chromakeys.showInfo()            end, {})
vim.api.nvim_create_user_command('Cksavetheme',            function()     chromakeys.saveTheme()           end, {})
```

## Manual color scheme creation (adjust individual scopes separately)

Select a scope, then use a color adjustment function.

There are four special scopes:

- `base`: This sets the base color. If you're creating a color scheme manually (as opposed to using the palettes), this is the first scope you need to set.
- `all`: This applies the command to all scopes. Useful if you want to adjust the hue/saturation/lightness of the entire scheme.
- `allExceptFgDefault`: This adjusts everything but the foreground scope color (and the scope colors derived from it - see below).
- `allExceptFgDefaultAsOne`: Same as allExceptFgDefault, but it changes all the affected scopes in lockstep.

## Generate entire color schemes in one go using palettes

Use `chromakeys.previousColorFunction` and `chromakeys.nextColorFunction` to select the color generator function. `chromakeys.generateColorScheme` uses the selected function to generate a color scheme. Most of these functions take their base hue and saturation from the base scope. These base settings have a large effect: the same palette function will generate strikingly different-looking color schemes based on the lightness and saturation settings of the base scope.

## Saving color schemes

Use `chromakeys.saveTheme`. This will save the color scheme with a name based on the foreground color in the format `ck<BaseColorName><number>.vim` in `~/.config/nvim/colors`. It will never overwrite any existing files.

## Navigating color schemes

Use `chromakeys.nextColorScheme` and `chromakeys.previousColorScheme`. Note that this will only work with color schemes in `~/.config/nvim/colors`.

## I don't like writing ~5KB to my SSD every time I press a key

`rsync` your `~/.config/nvim/colors` folder to a RAM drive, and symlink to it.
