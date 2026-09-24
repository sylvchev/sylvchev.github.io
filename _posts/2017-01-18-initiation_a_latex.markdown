---
layout: post
title: Initiation à LaTeX
date: 2017-01-18 15:00:00
description: Pour bien commencer
---

### Logiciels 
__Windows__
- Il y a [MikTeX](http://miktex.org/), la distribution LaTeX la plus répandue. Elle est utilisée par beaucoup d'environnements ou IDE LaTeX.
- Un bon IDE est [TeXmaker](http://www.xm1math.net/texmaker/), qui s'installe facilement. Attention, n'oubliez pas d'installer MikTeX pour que TeXmaker fonctionne correctement.
- Avec MikTex, vous pouvez utiliser votre éditeur de texte favori et compiler en ligne de commande

__Linux__
- La distribution LaTeX de référence est Texlive, elle est installée par défaut sur Ubuntu et sur bon nombre de distribution
- Un bon éditeur est [Kile](https://kde.org/applications/office/org.kde.kile) sur Kde, pour Ubuntu/Gnome il y a [Latexila](http://projects.gnome.org/latexila/).

__Mac__
- La suite [TeXshop](http://pages.uoregon.edu/koch/texshop/) semble être la référence, elle est installée de base avec la distribution Texlive.
- L'installeur [MacTeX](http://mirror.ctan.org/systems/mac/mactex/MacTeX.pkg) se charge d'installer TeXlive et TeXshop
- Un bon résumé est [disponible ici](http://macdevcenter.com/pub/a/mac/2004/02/03/latex.html)

### Pour commencer 
 Vous pouvez copier-coller ces quelques lignes et compléter les blancs comme bon vous semble :

{% highlight latex %}

\documentclass[a4paper,11pt]{article}

\usepackage{times}
\usepackage{textcomp}
\usepackage{amssymb,amsmath,amsfonts}
% si latex se plaint, commenter la ligne suivante et décommenter la ligne d'après
\usepackage[latin1]{inputenc}
% \usepackage[utf8]{inputenc}
\usepackage[T1]{fontenc}
\usepackage[francais]{babel}

\begin{document}

\date{\today}
\title{Titre}
\author{auteur}
\maketitle

\section{Un titre}
\label{premier}

\subsection{Un sous-titre}
\label{st}

votre texte

\bibliography{<mettre le nom de votre fichier bib>}
\bibliographystyle{plain}
\end{document}

{% endhighlight %}

### Canevas latex pour une thèse

Voici une archive qui contient un style possible pour la thèse, qui n'attend que vous pour être amélioré et diffusé plus largement. Vous pouvez le télécharger ici: <https://github.com/sylvchev/latex-template-uvsq-saclay>

### Canevas Beamer pour une présentation 

Pour faire une présentation aux couleurs du laboratoire et de l'Université, voici un canevas latex qui utilise le package beamer pour générer des diapositives. Le fichier sera disponible bientôt.

### Documents et liens 

Il y au moins un livre sur latex à la bibliothèque et beaucoup sur internet. Certains sont disponible gratuitement sur le site de [LaTeX](http://www.latex-project.org/).


__Des documents utiles__
- Un [aide mémoire](https://tex.loria.fr/general/aide-memoire-latex-seguin1998.pdf) bien pratique
- Un [document de référence](https://www.ntg.nl/doc/bayart/lxmanuel.pdf), issu de l'ESIEE, très complet, très bien expliqué.
- Un [petit résumé](http://www-hep.colorado.edu/~jcumalat/4610_fall_10/bibtex_guide.pdf) fonctionnement de Bibtex
- Un [document](http://tug.ctan.org/info/bibtex/tamethebeast/ttb_en.pdf) apprendre à bien utiliser Bibtex

__Some useful links__
- [Latex wiki](http://en.wikibooks.org/wiki/LaTeX), a good online guide. Easy to search and contains several examples.
- [Online example](https://www.writelatex.com/examples) includes several templates. It is possible to create  a document online, compile and view it.

### Des outils de conversions

Il est possible de convertir des documents LaTeX en documents office ou en HTML.
Malheureusement, ces convertisseurs sont assez sensibles (des fois ils ne fonctionnent pas) et peuvent donner des mises en formes très différentes. Je ne sais pas comment ils fonctionnent pour les maths et si les équations sont bien rendues. J’ai eu pas mal de soucis à les utiliser, je viens d’essayer de convertir un document LaTeX avec des équations que j’utilise à l’IUT mais je n’ai pas réussi à le convertir. La page du convertisseur est ici :
<https://www.tug.org/tex4ht/> avec des explications relativement claires ici <https://github.com/michal-h21/helpers4ht/wiki/tex4ht-tutorial>

Une fois qu’il est installé, il est possible de convertir le document latex avec la commande :

    mk4ht oolatex fichier_latex.tex

Il sera sans doute nécessaire de modifier largement le fichier latex pour que la conversion fonctionne. 
Il existe d’autres possibilité, comme simplement copier/coller le latex dans document office et enlever les balises LaTeX. Ces possibilités sont explorées ici : <http://www.tug.org/utilities/texconv/textopc.html> et là <https://en.wikibooks.org/wiki/LaTeX/Export_To_Other_Formats#Convert_to_RTF>

J'ai essayé latex2rtf pour générer un fichier RTF, les maths ne passent pas mais j'ai obtenu un résultat exploitable avec :

    pdflatex mon_fichier.tex && latex2rtf mon_fichier.tex
