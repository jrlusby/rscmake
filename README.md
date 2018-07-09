# rscmake

remote build wrapper for gcc+make to make it work as an ale linter

## install

to install as an vim plugin to enable ale / dispatch support

```vimscript
Plug 'w0rp/ale'
let g:ale_linters = {
    \ 'cpp' : ['rscmake', 'cppcheck', 'clangtidy', 'gcovcheck'],
    \ 'rust' : [],
    \ }

let g:ale_echo_msg_format = '%code: %%s %linter%'

Plug 'jrlusby/rscmake', { 'do': './install.sh' }

Plug 'tpope/vim-dispatch'
nnoremap <leader>d :Dispatch<CR>
nnoremap <leader>c :Copen<CR>

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
