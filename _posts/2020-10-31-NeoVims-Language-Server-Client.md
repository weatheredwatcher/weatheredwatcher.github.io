---
title:  "Neovims built-in LanguageServer Client and why you should use it"
date:   2020-10-31
categories: [development]
tags: [development]
---

As a fan of the [Language Server
Protocol](https://microsoft.github.io/language-server-protocol/) introduced by
Microsoft, I was very excited to hear, that [NeoVim](https://neovim.io/) (an
aggressive refactor of the Vim) will soon be shipping it's own language server.

Here I will show why I like the Language Server Protocol and how to configure
the built-in language server client to make it a bit more user friendly.

When I first looked into using the NeoVims internal language server, [jdhao's
blog post](https://jdhao.github.io/2019/11/20/neovim_builtin_lsp_hands_on/)
really helped me out -- I invite you to check it out as an additional resource.

## The Language Server Protocol -- What's all the fuss about
In essence the Language Server Protocol seeks to separate the functionality of
an IDE into two components:

 - The Language Server, which is programming language specific. It analyzes the
   code in the programming language you are using and implements the typical
   set of functions an IDE provides such as looking up code completion, looking
   up definitions, renaming variables, searching for symbols etc.
 - The Language Server Client, typically a plugin of the editor, which provides
   the services of the Language Server to the user.

The Language Server Protocol defines a standardized way for these two
components to interact with each other.  An example of such a communication can
be seen below[^language-server-img-src].

![Visualization of the communication between Language Server and Language
Server Client][language-server-com]

The benefit of splitting the services of an IDE into two components becomes
quite apparent: If every Language Server and Language Server Client follow
the specifications, it is only necessary to implement **a single Language
Server per programming language** and a **single Language Server Client for
each Editor**.  Thus all editors would benefit as soon as a language server
implements new functionality and the keybindings and additional functionalities
between different programming languages would be more consistent within
a single editor.

Especially if you use Vim or NeoVim for programming this flexibility with
regard to the programming language is amazing and makes your editor setup more
lightweight (by only requiring the installation of a single plugin), consistent
and flexible with regard to new programming languages.  Simply setup the
language server in your editor and you are good to go!


## NeoVims internal Language Server Client

Due to these benefits, there were many implementations of language servers for
vim (such as [vim-lsp](https://github.com/prabirshrestha/vim-lsp),
[LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim),
[vim-lsc](https://github.com/natebosch/vim-lsc)).  Nevertheless I often
experienced speed issues when using any of the plugins.  Thankfully, NeoVim
announced that they will be shipping a language server client built-in some
time ago.  It is not yet in the stable release but requires installing the
developer branch of NeoVim.  If you use `brew` this can for example be done
using the command `brew install neovim --HEAD`, which will install the most
recent version of NeoVim directly from the git repository.

Additionally, there is a lua-based plugin
[nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) which provides
routines to integrate a diverse set of language servers.  It's installation is
dependent on your plugin manager, for
[vim-plug](https://github.com/junegunn/vim-plug) it is installed by adding
`Plug 'neovim/nvim-lspconfig'` to your NeoVim config.

Afterwards, you can configure your language server using one of the
preconfigured profiles. For example, if you program in Python you first need to
install the python language server
```bash
pip install 'python-language-server[all]'
```
and then add the configuration
```vimscript
lua << EOF
require('nvim_lsp').pyls.setup({})
EOF
autocmd Filetype python setlocal omnifunc=v:lua.vim.lsp.omnifunc
```
which executes the line between the `EOF` statements as lua code. This is
required as we are configuring a plugin written in lua.  We need
to configure vim to actually use the Language Server Client for providing
completions. This is done in the last line of the snippet by setting omnifunc,
which allows us to trigger completion using `<C-x><C-o>`.  While this works, it
is by far not optimal, as the completion calls take place synchronously, at the
end of the article I will give some pointers on how to improve the experience
by using completion managers.

## Faster completion
To my knowledge there are currently two completion managers which support the
NeoVim internal Language Serve Client: [NCM2](https://github.com/ncm2/ncm2) and
[completion-nvim](https://github.com/nvim-lua/completion-nvim) where the first is
written largely in python and the second is written in lua.  When I first
tested `completion-nvim` I ran into some issues which was probably due to the
freshness of the project this can be a different story now.  Nevertheless,
I will here show how to set up `NCM2` as this is what I went for in the end.

First you need to of course install and enable `NCM2` for Plug this can be done
using the following commands

```vimscript
call plug#begin('~/.vim/plugged')
" Installing ncm2 with Plug
Plug 'ncm2/ncm2'
Plug 'roxma/nvim-yarp'
call plug#end()

autocmd BufEnter * call ncm2#enable_for_buffer()
```
where the last line activates the completion manager for every buffer you
enter.

`NCM2` has support for the NeoVim built-in Language Server Client (see this
[pull request](https://github.com/ncm2/ncm2/pull/178)), but it does
not yet seem to be well documented.  The support can be activated by adding
a callback to the language server setup routine described above

```vimscript

" Setup language server client
lua << EOF
local nvim_lsp = require('nvim_lsp')
local ncm2 = require('ncm2')

require('nvim_lsp').pyls.setup({
    on_init = require('ncm2').register_lsp_source,
});
EOF
```

## Getting more out of the language server - Diagnostics and Definitions
While this gives us some basic functionality, the Language Server is capable of
doing much more! In the next section I will be concentrating on looking up
definitions of functions/variables and displaying diagnostics. In the end
I will give indications on some further useful functions. For a full list of
features and details on how to use them in NeoVim please consider the [NeoVim
Language Server Client documentation](https://neovim.io/doc/user/lsp.html).

### Diagnostics
In particular I found the default setting of publishing diagnostic information
as virtual text at the end of the line rather annoying.  When I program
I prefer to only see the code and not some additional text indicating that this
function is missing a docstring etc. Changing the behaviour of NeoVims internal
Language Server Client can be done by modifying the callbacks it triggers when
it gets information from the Language Aerver.  The default callbacks are defined
[here](https://github.com/neovim/neovim/blob/master/runtime/lua/vim/lsp/callbacks.lua).

The callback of our interest is
["textDocument/publishDiagnostics"](https://github.com/neovim/neovim/blob/ca7449db46062098cc9b0c84401655cba7d3a53f/runtime/lua/vim/lsp/callbacks.lua#L76).
Here we simply want to remove the line
`util.buf_diagnostics_virtual_text(bufnr, result.diagnostics)` which adds the
virtual text annotations.  We can patch this by adding the following to our
config

```vimscript
lua << EOF
--- Define our own callbacks
local util = require 'vim.lsp.util'
local vim = vim
local api = vim.api
local buf = require 'vim.lsp.buf'

M['textDocument/publishDiagnostics'] = function(_, _, result)
  if not result then return end
  local uri = result.uri
  local bufnr = vim.uri_to_bufnr(uri)
  if not bufnr then
    err_message("LSP.publishDiagnostics: Couldn't find buffer for ", uri)
    return
  end

  -- https://microsoft.github.io/language-server-protocol/specifications/specification-current/#diagnostic
  -- The diagnostic's severity. Can be omitted. If omitted it is up to the
  -- client to interpret diagnostics as error, warning, info or hint.
  -- TODO: Replace this with server-specific heuristics to infer severity.
  for _, diagnostic in ipairs(result.diagnostics) do
    if diagnostic.severity == nil then
      diagnostic.severity = protocol.DiagnosticSeverity.Error
    end
  end

  util.buf_clear_diagnostics(bufnr)

  -- Always save the diagnostics, even if the buf is not loaded.
  -- Language servers may report compile or build errors via diagnostics
  -- Users should be able to find these, even if they're in files which
  -- are not loaded.
  util.buf_diagnostics_save_positions(bufnr, result.diagnostics)

  -- Unloaded buffers should not handle diagnostics.
  --    When the buffer is loaded, we'll call on_attach, which sends textDocument/didOpen.
  --    This should trigger another publish of the diagnostics.
  --
  -- In particular, this stops a ton of spam when first starting a server for current
  -- unloaded buffers.
  if not api.nvim_buf_is_loaded(bufnr) then
    return
  end
  util.buf_diagnostics_underline(bufnr, result.diagnostics)
  -- util.buf_diagnostics_virtual_text(bufnr, result.diagnostics)
  util.buf_diagnostics_signs(bufnr, result.diagnostics)
  vim.api.nvim_command("doautocmd User LspDiagnosticsChanged")
end
EOF
```

Which does exactly the same as the original callback but comments out the line
(`--` is a comment in lua) that we mentioned before.

In order to now see diagnostic information when placing the cursor on a line
with an error we can call the function `vim.lsp.util.show_line_diagnostics()`
after some hover time using `autocmd`:
```vimscript
autocmd CursorHold  <buffer> lua vim.lsp.util.show_line_diagnostics()
```

Additionally, we probably want to set the colors used to indicate a error or
warning to fit to the color scheme we using.  This can be done by defining the
appropriate highlight groups

```vimscipt
" Highlighting applied to floating window
highlight LspDiagnosticsErrorFloating guifg=#fb4934 gui=NONE ctermfg=NONE ctermbg=NONE cterm=NONE
highlight LspDiagnosticsWarningFloating guifg=#fabd2f gui=NONE ctermfg=NONE ctermbg=NONE cterm=NONE
highlight LspDiagnosticsInfoFloating guifg=#83a598 gui=NONE ctermfg=NONE ctermbg=NONE cterm=NONE
" Highlighting applied to code
highlight LspDiagnosticsUnderlineError guifg=NONE guibg=NONE guisp=#fb4934 gui=undercurl ctermfg=NONE ctermbg=NONE cterm=undercurl
highlight LspDiagnosticsUnderlineWarning guifg=NONE guibg=NONE guisp=#fabd2f gui=undercurl ctermfg=NONE ctermbg=NONE cterm=undercurl
highlight LspDiagnosticsUnderlineInfo guifg=NONE guibg=NONE guisp=#83a598 gui=undercurl ctermfg=NONE ctermbg=NONE cterm=undercurl
```

The values above are set to match the [gruvbox
8 colorscheme](https://github.com/lifepillar/vim-gruvbox8) and a terminal
supporting true color. They can be adapted to your personal
taste[^vim-highlighting].  If you want them to change dependent on your
colorscheme you should check out the vim predefines highlight groups (see
`:help highligh-groups`) and map accordingly using `highlight link`.  You
should keep in mind to set these after setting your color scheme though
otherwise you might run into problems.

### Definitions

One of the best features of vim in my opinion is the tagstack (if you don't
know about it, check out `:help tagstack` and get your mind blown away ;) ).
The tagstack allows you to do what you need to do when doing programming:
*Understanding what the function you are calling does under the hood.*  It
enables jumping to the definitions of functions, variables etc. and when you
got your information to jump back to where you came from.  The Language Server
Client allows you to jump to the definition of an object using `:lua
vim.lsp.buf.definition()` and we could just manually map this to the key
combination `<C-]>` which is used to jump to a tag.  Yet this would discard all
the other fancy combinations that are already defined for the tagstack such as
jump to definition in new window/in a preview window etc.  Thus the more
wholistic approach would be to override the tagstack functionality using
a custom `tagfunc`. I found a [single
reference](https://daisuzu.hatenablog.com/entry/2019/12/06/005543) on the
Japanese internet where somebody does exactly this. I adopted the code slightly
to work with the NeoVim internal Language Server Client[^add-reference].
```lua
local lsp = require 'vim.lsp'
local util = require 'vim.lsp.util'
local log = require 'vim.lsp.log'
local vim = vim

-- Ref (in Japanese): https://daisuzu.hatenablog.com/entry/2019/12/06/005543
-- Ref: https://qrunch.net/@igrep/entries/K6sUDofcmvtnRqzk
function tagfunc_nvim_lsp(pattern, flags, info)
 local result = {}
 local isSearchingFromNormalMode = flags == "c"

 local method
 local params
 if isSearchingFromNormalMode then
   -- Jump to the definition of the symbol under the cursor
   -- when called by CTRL-]
   method = 'textDocument/definition'
   params = util.make_position_params()
 else
   -- NOTE: Currently I'm not sure how this clause is tested
   --       because `:tag` command doesn't seem to use `tagfunc`.

   -- Search with `pattern` when called by ex command (e.g. `:tag`)
   method = 'workspace/symbol'

   -- Delete "\<" from `pattern` when prepended.
   -- Perhaps the server doesn't support regex in vim!
   params = {}
   if string.find(flags, 'i') then
     params.query = string.sub(pattern, '^\\<', '')
   else
     params.query = pattern
   end
 end
 local client_id_to_results, err = lsp.buf_request_sync(0, method, params, 800)
 if err then
   print('Error when calling tagfunc: ' .. err)
   return result
 end

 for _client_id, results in pairs(client_id_to_results) do
   for i, lsp_result in ipairs(results.result) do
     local name
     local location
     if isSearchingFromNormalMode then
       name = pattern
       location = lsp_result
     else
       name = lsp_result.name
       location = lsp_result.location
     end
     local location_for_tagfunc = {
       name = name,
       filename = vim.uri_to_fname(location.uri),
       cmd = tostring(location.range.start.line + 1)
     }
     table.insert(result, location_for_tagfunc)
 end
 end
 return result
end
```

Be aware that **the above code is written in lua** thus it must either be
integrated using the `lua << EOF` trick or saved in a separate file under
`~/.config/nvim/lua` and loaded prior to usage. In my case I save the above
code under `~/.config/nvim/lua/tagfunc_nvim_lsp.lua` and load it in vim using
```vimscript
lua require 'tagfunc_nvim_lsp'
setlocal tagfunc=v:lua.tagfunc_nvim_lsp
```

### Additional goodies

#### Highlighting references to variable under the cursor
```vimscipt
autocmd CursorHold  <buffer> lua vim.lsp.buf.document_highlight()
autocmd CursorHoldI <buffer> lua vim.lsp.buf.document_highlight()
autocmd CursorMoved <buffer> lua vim.lsp.buf.clear_references()

" References to the same variable
highlight LspReference guifg=NONE guibg=#665c54 guisp=NONE gui=NONE cterm=NONE ctermfg=NONE ctermbg=59
highlight! link LspReferenceText LspReference
highlight! link LspReferenceRead LspReference
highlight! link LspReferenceWrite LspReference
```

#### Formatting
The below snippet implements formatting the file when triggering a write to
disk.  It needed some additional fixes to prevent the cursor position from
being lost after the reformat[^preserve-cursor-position].
```vimscipt
function! Preserve(command)
    try
        " Preparation: save last search, and cursor position.
        let l:win_view = winsaveview()
        let l:old_query = getreg('/')
        silent! execute 'keepjumps ' . a:command
    finally
        " try restore / reg and cursor position
        call winrestview(l:win_view)
        call setreg('/', l:old_query)
    endtry
endfunction

autocmd BufWritePre <buffer> call Preserve('lua vim.lsp.buf.formatting_sync(nil, 1000)')
```

#### Renaming variables
The below snippet allows you to rename the variable under the cursor:
```vimscipt
function! LspRename()
    call inputsave()
    let l:newname = input('Rename to: ')
    call inputrestore()
    call luaeval('vim.lsp.buf.rename("'.l:newname.'")')
endfunction
```

#### Keybindings
In my vim config I wrapped a lot of the above functionality together into
a function called SetupLsp which I then call dependent on the input file type.
```vimscript
function SetupLsp()
    nnoremap <silent> <buffer> gd    <cmd>lua vim.lsp.buf.declaration()<CR>
    nnoremap <silent> <buffer> K  <cmd>lua vim.lsp.buf.hover()<CR>
    nnoremap <silent> <buffer> <c-k> <cmd>lua vim.lsp.buf.signature_help()<CR>
    inoremap <silent> <buffer> <c-k> <cmd>lua vim.lsp.buf.signature_help()<CR>
    nnoremap <silent> <buffer> <leader>ls    <cmd>lua vim.lsp.buf.document_symbol()<CR>
    nnoremap <silent> <buffer> gW    <cmd>lua vim.lsp.buf.workspace_symbol()<CR>
    nnoremap <buffer> <leader>lr <cmd>call LspRename()<CR>
    autocmd CursorHold  <buffer> lua vim.lsp.buf.document_highlight()
    autocmd CursorHold  <buffer> lua vim.lsp.util.show_line_diagnostics()
    autocmd CursorHoldI <buffer> lua vim.lsp.buf.document_highlight()
    autocmd CursorMoved <buffer> lua vim.lsp.buf.clear_references()
    autocmd BufWritePre <buffer> call Preserve('lua vim.lsp.buf.formatting_sync(nil, 1000)')
    lua require 'tagfunc_nvim_lsp'
    setlocal tagfunc=v:lua.tagfunc_nvim_lsp
    setlocal signcolumn=yes
endfunction

function SetupPython()
    setlocal colorcolumn=80
    setlocal tw=79
    setlocal signcolumn=yes
    setlocal spell
    setlocal tabstop=4 shiftwidth=4 expandtab
    call SetupLsp()
endfunction

autocmd Filetype python call SetupPython()
```


[language-server-sum]: https://microsoft.github.io/language-server-protocol/overviews/lsp/img/language-server-sequence.png

[^language-server-img-src]: The visualization is taken form the language server
  protocol documentation available
  [here](https://microsoft.github.io/language-server-protocol/overviews/lsp/overview/).

[^preserve-cursor-position]: For further information see [this stackexchange
  discussion](https://vi.stackexchange.com/questions/7761/how-to-restore-the-position-of-the-cursor-after-executing-a-normal-command),
  from which the command originated.

[^vim-highlighting]: Check out `:help highlight-cterm` and `:help
  highlight-gui` for the available settings.

[^add-reference]: There was a further reference on the path to accomplishing
  this [here](https://daisuzu.hatenablog.com/entry/2019/12/06/005543) yet the
  webpage seems down now.
to accomplishing this .

