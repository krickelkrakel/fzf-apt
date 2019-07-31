# fzf-apt

This is a small script which allows you to select one or more apt packages with fzf.  
The selected packages will be used to write an apt command to stdin.  
E.g.: '# apt install neovim'  

Usage: fzf-apt [apt flags / options]
E.g.:

- fzf-apt install
- fzf-apt remove
