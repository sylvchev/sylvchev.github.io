---
layout: page
permalink: /cv/
title: CV
description: My curriculum
nav: true
nav_order: 4
---

### Short version

Current situation:

- Full professor (professeur des Universités), Université Paris-Saclay, since 2022
- Member of [LISN-CNRS](https://www.lisn.upsaclay.fr)
- Head of [AO team](https://www.lisn.upsaclay.fr/recherche/departements-et-equipes/algorithmes-apprentissage-et-calcul/apprentissage-et-optimisation-2/), joint INRIA team [TAU](https://team.inria.fr/tau2/) (co-head G. Charpiat)
- Teaching at the Computer Science departement of [IUT d'Orsay](https://www.iut-orsay.universite-paris-saclay.fr/) and [Master IA](https://www.universite-paris-saclay.fr/formation/master/informatique/m1-artificial-intelligence)
- PEDR holder since 2016

Past positions:

- Associate professor (Maître de conférences), IUT de Vélizy, Université de Versailles St Quentin, 2011-2022
- Member of [LISV lab](http://www.lisv.uvsq.fr/), team Assistance and Interfaces, 2011-2022
- Elected member of the [61th section of CNU](https://www.conseil-national-des-universites.fr/cnu/#/entite/entiteName/CNU/idChild/33), 2019-2022
- Academic leave for 1 year in 2019 (CRCT 2 semesters), granted by CNU
- Post-doc fellow in [LTCI](https://images.telecom-paristech.fr/staff.html) (Signal processing department, [Télécom ParisTech](https://ltci.telecom-paristech.fr/)) on Brain-Computer Interfaces in EEG
- Post-doc fellow in [TAU team](https://www.inria.fr/en/teams/tau) ([INRIA Saclay](https://www.inria.fr/en) - [LRI](https://www.lri.fr/) funded by the ASAP ANR project on deep learning and swarm intelligence.
- ATER (french assistant professor) in [ETIS lab](https://www-etis.ensea.fr/) in [neurocybernetic team](https://perso-etis.ensea.fr/neurocyber/web/fr/) on neural models of visual preattention and in Computer Science Department of [Université de Cergy Pontoise](https://www.u-cergy.fr/fr/index.html)
- PhD thesis on the implementation of a preattentional system with spiking neurons, in [LIMSI-CNRS](https://www.limsi.fr/fr/), under the supervision of Philippe Tarroux and Hélène Paugam-Moisy in 2009.
- Moniteur (french TA) in [Computer Science Department of IUT Orsay](http://www.iut-orsay.u-psud.fr/).

### Supervision

<!-- Generated from the CV repository: edit it there, then run `uv run bin/sync_cv.py`. -->

PhD students:

{% for s in site.data.supervision.phd %}

- **{{ s.name }}**, {{ s.funding }}: _"{{ s.title }}"_. {{ s.supervision }}. {{ s.status }}{% if s.position %}. Now {{ s.position }}{% endif %}.
  {%- endfor %}

{% for g in site.data.supervision.interns %}

<details>
<summary><strong>{{ g.group }}</strong> ({{ g.items | size }})</summary>
<ul>
{% for s in g.items %}
<li><strong>{{ s.name }}</strong>, {{ s.details }}: <em>"{{ s.title }}"</em></li>
{% endfor %}
</ul>
</details>
{% endfor %}

---

### Academic curriculum vitae

You can find my full CV <a class="page-link" href="{{ '/cv/cv.pdf' | relative_url }}">here</a> (in French)
