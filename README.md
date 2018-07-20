# Rscmake

remote build wrapper for gcc+make to make it work as an ale linter

## Install

to install as an vim plugin to enable ale / dispatch support

### Install ale+rscmake and enable rscmake as a linter

```vim
" Install the plugins
Plug 'w0rp/ale'
Plug 'yaahallo/rscmake', { 'do': './install.sh' }

" Enabled linters by filetype
let g:ale_linters = {
    \ 'cpp' : ['rscmake', 'cppcheck', 'clangtidy', 'gcovcheck'],
    \ 'rust' : [],
    \ }

" optional: change ale lint output format to indicates which linter each error
" message originates from
let g:ale_echo_msg_format = '%code: %%s %linter%'
```

### Optional: Install dispatch for full builds

```vim
Plug 'tpope/vim-dispatch'
nnoremap <leader>d :Dispatch<CR>
```

Dispatch needs to know which compiler to run in each situation. This is
indicated by b:makeprg when using :Make, but can be overriden for :Dispatch,
which gets its compiler from b:dispatch which I generally find to be the easier
to use option.

Here is an example of managing the b:dispatch variable for different file
types, and in some cases different projects of the same filetype

```vim
function! SetupEnvironment()
  let l:path = expand('%:p')
  if l:path =~# '/home/jlusby/git/scale-product'
    let b:dispatch = 'rscmake'
  elseif l:path =~# '/home/jlusby/git/notjobless'
    let b:dispatch = './make.sh'
  elseif !empty(glob('./CMakeLists.txt')) && !empty(glob('./build'))
      let b:dispatch = 'make -C build/'
  endif
endfunction
autocmd FileType cpp call SetupEnvironment()
autocmd FileType rust let b:dispatch = 'cargo build'
autocmd FileType go let b:dispatch = 'go build %'
```
