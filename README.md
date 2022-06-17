# vim-setup
The ultimate Guide To Neovim and Tmux for fullstack engineers


## Pre install
- **Iterm2**
  ```
    brew install iterm2
  ```
- **NeoVim**
  ```
    brew install neovim
  ```
- **Tmux**
  ```
    brew install tmux
  ```
- **Vim-Plug**
  ```
    curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
  ```
- **Python3 & Pip**
  ```
    brew install python3
    python3 get-pip.py
  ```
- **Powerline**
  ```
    git clone https://github.com/powerline/powerline $HOME/powerline
  ```
  **Set —editable option**
  ```
    python3 -m pip install --editable=$HOME/powerline
  ```
  **check whether could import powerline library**
  ```
    /usr/bin/python3 -c "import powerline;print(powerline.__file__)"
  ```
  **If this result is displayed then Ok**
  ```
    $HOME/powerline/powerline/__init__.py
  ```
  
※ If you are interested in fish, following installation

- **Fish shell**
  ```
    brew install fish
  ```
  Install fisher
  ```
    curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
  ```
  Install [oh-my-fish](https://github.com/oh-my-fish/oh-my-fish)
  ```
    curl https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish
  ```
  Install [tide](https://github.com/IlanCosman/tide)
  ```
    fisher install IlanCosman/tide@v5
  ```
  
  
 ## Configure .tmux.conf file
 For mac, you can open it by this command
  ```
    vim ~/.tmux.conf
  ```
  
**After opening then add this content. This is my config, you can configure your configurations.**
```

# -*- coding: utf-8 -*-
  set-option -g default-terminal xterm-256color
  set-window-option -g xterm-keys on
  
  set-option -ag terminal-overrides ",*256col*:colors=256:Tc"
  
  unbind-key C-b
  set-option -g prefix C-t
  bind-key C-t send-prefix
  # <prefix>+S to devide horizontal
  bind-key S split-window -h
  # Restoring clear screen(C-l)
  bind C-l send-keys 'C-l'
 
  set-window-option -g mode-keys vi
  # key-binding for selecting screen
  #bind-key h select-pane -L
  #bind-key j select-pane -D
  #bind-key k select-pane -U
  #bind-key l select-pane -R
 
 
  # Smart pane switching with awareness of Vim splits.
  # See: https://github.com/christoomey/vim-tmux-navigator
  is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
      | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
  bind-key -n 'C-h' if-shell "$is_vim" 'send-keys C-h'  'select-pane -L'
  bind-key -n 'C-j' if-shell "$is_vim" 'send-keys C-j'  'select-pane -D'
  bind-key -n 'C-k' if-shell "$is_vim" 'send-keys C-k'  'select-pane -U'
  bind-key -n 'C-l' if-shell "$is_vim" 'send-keys C-l'  'select-pane -R'
  tmux_version='$(tmux -V | sed -En "s/^tmux ([0-9]+(.[0-9]+)?).*/\1/p")'
  if-shell -b '[ "$(echo "$tmux_version < 3.0" | bc)" = 1 ]' \
      "bind-key -n 'C-\\' if-shell \"$is_vim\" 'send-keys C-\\'  'select-pane -l'"
  if-shell -b '[ "$(echo "$tmux_version >= 3.0" | bc)" = 1 ]' \
      "bind-key -n 'C-\\' if-shell \"$is_vim\" 'send-keys C-\\\\'  'select-pane -l'"
 
  bind-key -T copy-mode-vi 'C-h' select-pane -L
  bind-key -T copy-mode-vi 'C-j' select-pane -D
  bind-key -T copy-mode-vi 'C-k' select-pane -U
  bind-key -T copy-mode-vi 'C-l' select-pane -R
  bind-key -T copy-mode-vi 'C-\' select-pane -l
 
  set -g mouse off
  # set maximum number can back to history by C-u, C-d
  set-option -g history-limit 999999999
 
  # set reload file setting by cmd + r
  bind-key r source-file ~/.tmux.conf \; display “Reloaded"
bind-key t setw synchronize-panes \; display "synchronize-panes #{?pane_synchronized,on,off}"
 
  set-option -g display-panes-time 2147483647
  # set base-index begin 1 not 0 as default
  set-option -g base-index 1
  set-window-option -g pane-base-index 1
 
  # Match message-style with powerline
  set-option -g message-command-style bg=black,fg=cyan
  set-option -g message-style         bg=black,fg=cyan
 
  # disable bell
  set-option -g bell-action none
 
  set-option -g focus-events on
  set-window-option -g aggressive-resize on
 
  # set default-shell will be used in tmux
  set-option -g default-shell /usr/local/bin/fish
 
  # enable powerline on status bar
  run-shell "/usr/bin/python3 $HOME/powerline/scripts/powerline-config tmux setup"


```


## Configure init.vim
```
  mkdir ~/.config/nvim
  touch ~/.config/nvim/init.vim
```
Open init.vim file and add following lines to use configurations in .vimrc for NeoVim
```
  set runtimepath^=~/.vim runtimepath+=~/.vim/after
  let &packpath=&runtimepath
  source ~/.vimrc
```
  
  
## Configure .vimrc file
 For mac, you can open it by this command
  ```
    vim ~/.vimrc
  ```
  
  **After opening then add this content. This is my config, you can configure your configurations.**
  ```
  
autocmd!
scriptencoding utf-8
" stop loading config if it's on tiny or small
if !1 | finish | endif

set nocompatible
 
set encoding=utf-8
set number
set cursorline
set relativenumber
set tabstop=4
set shiftwidth=4
set expandtab
set smartindent
set smarttab
set nobackup
set nowrap
set showmatch
set htsearch
set wildmenu

" status line left side
set statusline+=\ %F\ %M\ %Y\ %R


call plug#begin('~/.vim/plugged')

" load setting of vim from powerline
Plug '~/powerline/powerline/bindings/vim'
set laststatus=2
set showtabline=2

Plug 'tomasr/molokai'
Plug 'jpalardy/vim-slime'
Plug 'junegunn/fzf', {'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
" Auto complete
Plug 'valloric/youcompleteme'
" Nerdtree
Plug 'scrooloose/nerdtree'
" Support Js
Plug 'pangloss/vim-javascript'
" Support jsx
Plug 'mxw/vim-jsx'
" Support highlight
Plug 'sheerun/vim-polyglot'
" Neo vim
Plug 'shougo/deoplete.nvim'
" Support typescript
Plug 'leafgarland/typescript-vim'
" Nodejs extension
Plug 'neoclide/coc.nvim'
" Support json
Plug 'elzr/vim-json'
" Support html
 Plug 'othree/html5.vim'
" Support css
Plug 'hail2u/vim-css3-syntax'
" Vim theme
Plug 'vim-airline/vim-airline-themes’
" Tabline
Plug 'vim-airline/vim-airline'
" Support choose like tmux's display-pane
Plug 't9md/vim-choosewin'
" Support git
 Plug 'tpope/vim-fugitive'
" Colorschemes
Plug 'flazz/vim-colorschemes'
" vim colors solarized
Plug 'altercation/vim-colors-solarized'
" auto format
Plug 'chiel92/vim-autoformat'
" Split window
Plug 'anotherproksy/ez-window'
" Auto-complete for quotes, parens, brackets, etc
Plug 'raimondi/delimitmate'
" Auto pairs for inserting, deleting brackets,parens,quotes in pair
 Plug 'jiangmiao/auto-pairs'
" Tmux navigator
Plug 'christoomey/vim-tmux-navigator'
" plugin supports full path fuzzy file, buffer, mru, tag,...finder for vim
Plug 'ctrlpvim/ctrlp.vim'
" Vim icons
Plug 'ryanoasis/vim-devicons'
" Tagbar
Plug 'majutsushi/tagbar'
" Super tab
Plug 'ervandew/supertab'
Plug 'dracula/vim’


Plug 'prettier/vim-prettier', {
    \   'do': 'yarn install',
     \ 'for': [ 'javascript','typescript','css','less','scss','json','graphql','markdown','react','vue','dart','yaml','html']}
 
  call plug#end()
 
  let g:coc_global_extensions = ['coc-tsserver',
                    \'coc-python',
                    \'coc-pydocstring',
                    \'coc-json',
                    \'coc-vue',
                    \'coc-flutter',
                    \'coc-html-css-support',
                    \'coc-css',
                    \'coc-sql',
                    \'coc-yaml']
 
 
  let g:airline_powerline_fonts = 1
 
  " Custom own key-binding
 
  let g:slime_no_mappings = 1
  xmap <C-j> <Plug>SlimeRegionSend
  let g:slime_target = "tmux"
  let g:slime_default_config = {"socket_name": get(split($TMUX, ","), 0), "target_pane": ":.2"}
  let g:slime_python_ipython = 1
  let g:slime_dont_ask_default = 1
 
  " config autoformat
  let g:autoformat_autoindent = 0
  let g:autoformat_retab = 0
  let g:autoformat_remove_trailing_spaces = 0
 
  " shows hidden files/folders by default in Nerdtree
  let g:NERDTreeShowHidden = 1
  let g:NERDTreeMinimalUI = 0
  let g:NERDTreeIgnore = ['node_modules']
  let NERDTreeStatusLine='NERDTree'
  " Automaticaly close nvim if NERDTree is only thing left open
  autocmd bufenter * if (winnr("$") == 1 && exists("b:NERDTree") && b:NERDTree.isTabTree()) | q | endif
  " Toggle
  nnoremap <silent> <C-a> :NERDTreeToggle<CR>

" open new split panes to right and below
  set splitright
  set splitbelow
  " turn terminal to normal mode with escape
  tnoremap <Esc> <C-\><C-n>
  " start terminal in insert mode
  au BufEnter * if &buftype == 'terminal' | :startinsert | endif
  " open terminal on ctrl+n
  function! OpenTerminal()
  split term://zsh
  resize 10
  endfunction
  nnoremap <c-n> :call OpenTerminal()<CR>
  let g:prettier#autoformat_config_present = 1
  let g:prettier#config#config_precedence = 'prefer-file'
 
 
  " Custom Key Binding
  let g:tmux_navigator_no_mappings = 1
 
  nnoremap <silent> {Left-Mapping} :TmuxNavigateLeft<cr>
  nnoremap <silent> {Down-Mapping} :TmuxNavigateDown<cr>
  nnoremap <silent> {Up-Mapping} :TmuxNavigateUp<cr>
  nnoremap <silent> {Right-Mapping} :TmuxNavigateRight<cr>
  nnoremap <silent> {Previous-Mapping} :TmuxNavigatePrevious<cr>
 
 
  " disable while zoomed in tmux
  let g:tmux_navigator_disable_when_zoomed = 1
 
  " key-binding for searching files and folders
  let g:ctrlp_map = '<c-p>'
  let g:ctrlp_cmd = 'CtrlP'
 
 
  if (has("termguicolors"))
    filetype plugin indent on
    set termguicolors
  endif
  syntax enable
  colorscheme dracula
  
  ```
  **After adding contents run this command to install plguins**
    ```
      :PlugInstall
    ```
  
  ## Result
  After finished configuration. This is the result.
  Very nice 🥳 I love Vim 😁
  
  <img src="https://tech.sensetime.jp/wp-content/uploads/2021/12/screenshot_nvim_appearance.png" width=1000 height=400  />
  
  
  
  
  
 
 
 
 
 
 
 
 
