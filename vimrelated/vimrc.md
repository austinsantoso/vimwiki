```vim
call plug#begin('~/.config/nvim/plugged')
" Colorscheme
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
Plug 'sainnhe/sonokai'

" Guality of Life
Plug 'preservim/nerdtree'
Plug 'preservim/nerdcommenter'
" Plug 'Yggdroot/indentLine'
Plug 'frazrepo/vim-rainbow'

Plug 'vimwiki/vimwiki'

" Git
Plug 'airblade/vim-gitgutter'
Plug 'tpope/vim-fugitive'

call plug#end()

" ===========================================================
" Choose a character to be leader key
" Delaults to '\'
" let mapleader = ' '

" Use system clipboard, a bit buggy
set clipboard+=unnamedplus

" vertically center dociment when entering insert mode
" autocmd InsertEnter * norm zz

" Remove Trailing whitespace on save
autocmd BufWritePre * %s/\s\+$//e

" ===========================================================
" BASICS
set number relativenumber
set encoding=UTF-8
set termguicolors
set incsearch

" syntax things
filetype plugin indent on " enabled by default in neovim
set nocompatible " ignored in neovim
syntax on

" Tab Settings
" using hard tabs
set tabstop=4
" when indenting with '>', use 4 spaces width
set shiftwidth=4
" On pressing tab, insert space
" set expandtab
" delete all spaces till tabstop
set softtabstop=4
set smarttab


" Cursorline and columnline
set cursorline
set cursorcolumn
highlight CursorLine ctermbg=Yellow cterm=bold guibg=#2b2b2b
highlight CursorColumn ctermbg=Yellow cterm=bold guibg=#2b2b2b

" ===========================================================
" MAPPINGS
" ===========================================================
" Splitting
set splitbelow splitright

" Shortcutting split navigation
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

" Make adjusting sizes a bit more friendly
nnoremap <silent> <A-Left> :vertical resize -3<CR>
nnoremap <silent> <A-Right> :vertical resize +3<CR>
nnoremap <silent> <A-Up> :resize +3<CR>
nnoremap <silent> <A-Down> :resize -3<CR>

" Convert two vertical split to two horizontal split
map <Leader>th <C-w>t<C-w>H
map <Leader>tk <C-w>t<C-w>K

" Shortcut Split Opening
nnoremap <leader>h :split<Space>
nnoremap <leader>v :vsplit<Space>

" map S to replace All
nnoremap S :%s//gci<Left><Left><Left><Left>

" ===========================================================
" Colorscheme
let g:sonokai_style="andromeda"
let g:airline_theme='sonokai'
" let g:airline_powerline_fonts = 1

" let g:sonokai_termcolor=16

let g:airline#extensions#hunk#enabled = 1

colorscheme sonokai
" ========================================================
" Plugins

" NERDTree plugin
" toggle nerdtree with Ctrl-N
map <C-o> :NERDTreeToggle<CR>
" How can I close vim if the only window left open is a NERDTree?
autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif

" indentLine
let g:indentLine_char_list = ['|', '¦', '┆', '┊']

" Tab lines for hard tab
set listchars=tab:\|\ " there is a space needed after \
set list

" rainbow brackets
let g:rainbow_active = 1

" NerdCommenter
" bind Ctrl + x to toggle comment " need a better bidning for this
map <C-x> <leader>c<Space>
" Add a space after comment delimiter
let g:NERDSpaceDelims=1

" vimwiki
let g:vimwiki_list = [{'path': '~/vimwiki/',
			\ 'syntax': 'markdown', 'ext': '.md'}]
" ========================================================
" Directory settings for stuff
if !isdirectory($HOME.'/.config/nvim')
	call mkdir($HOME.'/.config/nvim', "", 0770)
endif
if !isdirectory($HOME.'/.config/nvim/undo-dir')
	call mkdir($HOME.'/.config/nvim/undo-dir', "", 0770)
endif
set undodir=~/.config/nvim/undo-dir//
set undofile

if !isdirectory($HOME.'/.config/nvim/backup-dir')
	call mkdir($HOME.'/.config/nvim/backup-dir', "", 0700)
endif

set backupdir=~/.config/nvim/backup-dir//

" this is for .swp
" set directory=~/nvim/backup-dir//
" set backup

let g:python3_host_prog = '/usr/bin/python3'
```
