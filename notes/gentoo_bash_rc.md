```bash
# User specific aliases and functions
if [[ ${EUID} == 0 ]] ; then
  PS1='\[\033[01;31m\]\h\[\033[01;34m\] \W \$\[\033[00m\] '
else
  PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w \$\[\033[00m\] '
fi

# Gentoo (/etc/bash/bashrc)
if [[ ${EUID} == 0 ]] ; then     
    PS1='\[\033[01;31m\]\h\[\033[01;34m\] \W \$\[\033[00m\] '    
else   
    PS1='\[\033[01;32m\]\u@\h\[\033[01;34m\] \w \$\[\033[00m\] '
fi
```