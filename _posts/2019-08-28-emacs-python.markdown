---
layout: post
title: Emacs python cheatsheet
date: 2019-08-28 07:32:00
description: Finger crossed
---

### Python 4 Emacs
See [Emacs wiki](https://www.emacswiki.org/emacs/PythonProgrammingInEmacs)
- [anaconda-mode](https://github.com/proofit404/anaconda-mode)
- [pyvenv](https://github.com/jorgenschaefer/pyvenv)
- [flycheck-pycheck](https://www.flycheck.org/en/latest/user/quickstart.html)

### Elpy
- `conda install jedi rope flake8 autopep8 yapf black`
- M-x list-package, install elpy
- pyvenv already part of elpy
- [Some info](https://realpython.com/emacs-the-best-python-editor/) here

### Git
- [magit](https://magit.vc/) avec des [infos ici](https://www.masteringemacs.org/article/introduction-magit-emacs-mode-git)

### Markdown
- [markdown-mode](https://jblevins.org/projects/markdown-mode/)
- flycheck-mark
- not used, [gh-md](https://github.com/emacs-pe/gh-md.el)

### Keybindings cheatsheet

|----------|------------|---------------------------------|
| Mode     | Keybinding | Description                     |
|----------|------------|---------------------------------|
| anaconda | C-M-i      | auto-complete                   |
| anaconda | M-.        | find definition                 |
| anaconda | M-=        | show assignement                |
| anaconda | M-r        | show all lines containing focus |
| anaconda | M-?        | show doc                        |
|----------|------------|---------------------------------|
| elpy     | C-c C-c    | send buffer/region to ipython   |
| elpy     | C-RET      | send line to Ipython            |
| elpy     | C-c C-z    | switch to ipython/buffer        |
| elpy     | C-c C-d    | open doc                        |
| elpy     | C-c C-K    | Kill shells                     |
|----------|------------|---------------------------------|
| flycheck | C-c ! l    | list all error                  |
| flycheck | C-c ! n    | next error                      |
| flycheck | C-c ! p    | prev error                      |
| flycheck | C-c ! s    | select checker                  |
|----------|------------|---------------------------------|

### Relevant .emacs snippet

{% highlight emacs %}

(setenv "WORKON_HOME" "/home/sylchev/anaconda3/envs")
(pyvenv-mode 1)
(add-hook 'python-mode-hook 'anaconda-mode)
(add-hook 'python-mode-hook 'anaconda-eldoc-mode)
(add-hook 'python-mode-hook 'elpy-mode)
;; (setq python-shell-interpreter "jupyter"
;;       python-shell-interpreter-args "console --simple-prompt"
;;       python-shell-prompt-detect-failure-warning nil)
;; (add-to-list 'python-shell-completion-native-disabled-interpreters
;;              "jupyter")
 (setq python-shell-interpreter "ipython"
       python-shell-interpreter-args "--simple-prompt -i")
(elpy-enable)
(when (load "flycheck" t t)
  (setq elpy-modules (delq 'elpy-module-flymake elpy-modules))
  (add-hook 'elpy-mode-hook 'flycheck-mode))

{% endhighlight %}
