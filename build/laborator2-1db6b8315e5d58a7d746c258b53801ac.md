---
weight: 2
title: "Laborator 2 - Controlul versiunilor & Git"
---

---

## Ce este git?

[**Git**](https://git-scm.com/downloads) este un sistem de management al proiectelor
software și de gestionare a versiunilor codului sursă.
Un proiect software în Git se numește `repository` și poate conține fișiere cod sursă, baze de date,
fișiere text cu documentație etc.

În Git fiecare versiune a unui program se numește `commit` și conține modificările aduse
unuia sau mai multor fișiere.
Astfel, istoricul modificărilor este conținut în lista commiturilor și se poate reveni
cu ușurință la o versiune anterioară a programelor.
De aceea este important ca fiecare commit să se refere la o singură modificare logică
(inclusiv în cazul în care afectează mai multe fișiere).

Datorită faptului că este un sistem distribuit,
Git se pretează pentru lucrul în echipă, fiecare membru având o copie locală a software-ului,
iar modificările aduse de fiecare fiind integrate pe un server.

Fiecare programator își poate alege modalitatea în care salvează aceste modificări,
însă atunci când se lucrează în echipă trebuie ținut cont de faptul că ele pot afecta munca altor colegi.
De aceea, este o idee bună ca versiunile (commiturile) să fie funcționale (să se poată compila),
chiar dacă nu sunt finalizate sau nu acoperă eventualele cazuri particulare.

Git permite de asemenea crearea unor ramuri de dezvoltare separate (denumite `branch-uri`), astfel
încât funcționalitățile (eventual incomplete) create pe o anumită ramură să nu afecteze dezvoltarea
întregului software.
Ramura principală se numește, de regulă, `main` (sau `master`) și conține o versiune
funcțională și stabilă a software-ului.
Operația prin care o ramură (ajunsă în stadiul final de dezvoltare) se integrează cu o
alta se numește `merge`.

Pentru început vom vedea cum se creează și cum se folosește un repository local, iar apoi cum se
sincronizează acesta cu unul aflat pe server (`remote`).

---

## Setup
### Instalare

Este necesar ca întâi să instalați Git, varianta pentru sistemul vostru de operare. [Start here](https://git-scm.com/downloads).

În timpul instalării, în fereastra `Adjusting the name of the initial branch in new repositories`,
alegeți `Override the default branch name for new repositories`, cu textul `main`.
Pentru restul ferestrelor puteți lăsa opțiunile implicite.

### Repository local

Pentru a utiliza Git într-un proiect software e nevoie de inițializarea unui
repository în directorul care conține programele din proiect.
Apoi, ori de câte ori se aduc modificări unuia sau mai multor fișiere,
aceste modificări vor fi incluse într-un commit nou.

Toate aceste operații se realizează în linia de comandă (Command Prompt în Windows sau Terminal în Linux).

### Configurare

Pentru a inițializa un repository nou, în linia de comandă navigați în directorul
dorit și folosiți comanda:

:::{code} bash
git init
:::

Configurați apoi user-ul, cu nume și adresa de email (pot fi fictive dacă lucrați pe un
calculator din laborator):

:::{code} bash
git config user.name "Codin Codescu"
git config user.email "codin@codescu.com"
:::

---

## Utilizare

Crearea unui commit se realizează în două faze. Prima se numește *staging* și presupune semnalarea
fișierelor modificate care urmează să fie integrate în noul commit.
Aceasta se face utilizând comanda `git add`.

:::{code} bash
git add fisier1.c fisier2.cpp
:::
Această etapă reprezintă pregătirea pentru commit, iar modificările aduse aici sunt reversibile.
Spre exemplu, comanda de mai jos elimină `fisier2.cpp` din lista celor ce vor fi incluse în commit:

:::{code} bash
git restore --staged fisier2.cpp
:::
Puteți vedea oricând ce fișiere sunt `staged` utilizând comanda:

:::{code} bash
git status
:::

Atunci când sunteți siguri că doriți să integrați modificările într-un nou commit, folosiți comanda:
:::{code}bash
git commit -m "subiect"
:::

unde `"subiect"` conține o scurtă descriere (de preferat de cel mult 50 de caractere) a commitului.
Se obișnuiește ca subiectul să fie redactat în modul imperativ, spre exemplu
"Fix bug at line 202", "Optimize memory usage in function myFunction",
"Add function to compute shortest path", sau "Modify function myFunction to include NULL case".

După ce un commit a fost creat, acesta nu se mai poate modifica și va rămâne în istoricul proiectului.
Se poate reveni, însă, la un commit anterior, în cazul în care în acesta s-au strecurat greșeli.

---

## Conectarea la un repository remote

[GitLab](https://gitlab.com/) și [GitHub](https://github.com/)  sunt două exemple de platforme ce oferă servicii de găzduire pentru software și care folosesc sistemul Git.

### Conectare prin ssh

SSH (Secure Shell) este un protocol de rețea care permite conectarea în siguranță la alte calculatoare.
Autentificarea se realizează printr-un sistem de criptare asimetric bazat pe o pereche de chei:
una publică (folosită pentru a cripta mesajele) și alta privată (folosită pentru a decripta mesajele).

Pentru a vă crea propria pereche de chei ssh, utilizați comanda:
:::{code} bash
ssh-keygen
:::

Vi se va cere să alegeți un director în care să salvați cele două chei.
Pentru a evita setări suplimentare ulterioare, alegeți directorul default, apăsând `Enter`. Implicit,
directorul este `C:\Users\<nume_utilizator>\.ssh` (sau `/home/<nume_utilizator>/.ssh` daca folositi Linux).
În funcție de setările sistemului de operare, acest director ar putea fi *hidden*.

Aceasta generează două fișiere în directorul ales: unul conținând cheia publică, ce are
extensia *.pub* și altul, cu aceeași denumire aleasă, dar fără extensie, conținând cheia privată.

Pentru a crea conexiunea cu serverul GitLab sau GitHub trebuie să adăugați cheia publică în
contul vostru (să copiați conținutul fișierului cu cheia publică):

în **GitHub**, la Profile -> Settings -> SSH and GPG keys -> New SSH key.

în **GitLab**, la Profile -> Preferences -> SSH keys -> Add a SSH key.

Dacă nu lucrați pe calculatorul personal și lăsați cheia privată SSH, oricine are acces la calculator va
putea face modificări în contul vostru remote. În acest caz, asigurați-vă că atunci când ați terminat de
lucru ștergeți perechea de chei din calculator. De fiecare dată când veți dori să vă conectați pe contul
remote de pe un calculator nou va trebui să generați o nouă pereche de chei.

### Repository remote

Până acum ați creat un repository local. Este nevoie să creați și unul pe server, apoi să le sincronizați pe cele două.

Pentru a crea un repository nou,

în **GitHub**, la Profile -> Your repositories -> New.

în **GitLab**, la New Project -> Create Blank Project.

În ambele variante, asigurați-vă că ați debifat opțiunea **Initialize repository with a README**.
Aceasta trebuie debifată deoarece urmează să încărcați pe server un repository local deja existent
și aveți nevoie de un repository fără niciun commit pe server. Dacă ați fi bifat opțiunea, serverul
ar fi creat un prim commit pentru a adăuga fișierul README în proiect. Fiecare proiect trebuie să conțină,
însă, un fișier README, așadar îl veți crea voi ulterior.

Pentru a vă putea conecta repository-ul local la cel de pe server, specificați prin comanda *git remote*
adresa repository-ului remote:

:::{code} bash
git remote add origin git@github.com:user/repo.git
:::

Atât în **GitHub**, cât și în **GitLab**, adresa o obțineți navigând în repository,
apoi Code -> (Clone with) SSH.

După ce ați executat comanda precedentă, *origin* va fi un *alias* pentru adresa
`git@github.com:user/repo.git` și îl veți putea folosi de fiecare dată când vreți să vă referiți
la această adresă pentru a trimite modificările pe server sau a le prelua de acolo.

În acest moment repository-ul vostru local este conectat la cel de pe server.
Puteți avea mai multe copii locale (pe mai multe calculatoare) ale repository-ului remote,
iar ele pot fi sincronizate cu ajutorul următoarelor comenzi.

Pentru a obține cea mai nouă versiune a repository-ului de pe server, folosiți comanda `git pull`:
:::{code} bash
git pull origin main
:::
Observați specificarea adresei (prin aliasul `origin`) de la care se
face preluarea repository-ului. Iar `main` (sau alternativ `master`,
în funcție de cum a fost configurat repository-ul) se referă la `branch`-ul de pe care se face copierea.

Pentru a publica pe server commiturile create local se folosește comanda `git push`:
:::{code} bash
git push origin main
:::

Ordinea acestor comenzi este importantă atunci când lucrați cu un repository remote,
în special atunci când colaborați cu alți programatori.
Înainte de a trimite pe server modificări (cu `push`) trebuie să vă
asigurați că aveți ultima versiune a programului, altfel commiturile pe care încercați să
le trimiteți vor fi în urma celor deja existente pe server, situație care se numește conflict.
Într-o anumită măsură, conflictele sunt inevitabile dacă lucrați în echipă și mai mulți programatori
modifică în același timp același program sau aceeași secvență de cod.
Există un mecanism automat de rezolvare a conflictelor, însă în cazul în care acesta eșuează, ele trebuie
rezolvate manual.

Pentru a evita situația în care există commituri noi în repository-ul remote, pe care nu le aveți
local în momentul în care vreți să trimiteți modificări pe server, faceți `pull` înainte
de `push`.

Pentru a vă obșnui cu git, găsiți [aici](https://gitimmersion.com/) și [aici](https://learngitbranching.js.org/) două tutoriale bune și [aici un joc](https://ohmygit.org/).