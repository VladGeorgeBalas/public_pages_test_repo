---
weight: 1
title: "Laborator 1 - Bune practici de programare"
---

---

##  Asigurarea lizibilitătii
### Denumiri și comentarii

Pentru ca un program să fie ușor de citit, este nevoie în primul rând ca numele
variabilelor și funcțiilor să fie bine alese și să descrie explicit scopul acestora în program.
Prezența comentariilor în cod nu este opțională.
Fără ele, codul poate fi greu de înțeles chiar și de persoana care l-a scris.
De asemenea, expresiile matematice și/sau logice folosite e
bine să fie cât mai simple: o linie de cod să facă un singur lucru.

Cele două exemple de cod de mai jos produc același rezultat.
Funcționalitatea codului din primul exemplu este ușor de identificat.
Însă doar în al doilea exemplu scopul codului este evident, datorită
utilizării unor denumiri relevante pentru variabile.
Deși codul din cea de-a doua variantă este mai lung,
expresia testată în `if` se citește mai ușor.

:::{code} c
:caption: Denumiri leneșe și expresii complicate
:linenos:

if ((a == 1 || a == 2 || a == 12) && (b == 6 || b == 7) && (c < 16)) {
    x = 1;
} else {
    x = 0;
}
:::


:::{code} c
:caption: Denumiri descriptive și expresii simple
:linenos:

isWinter = month == 1 || month == 2 || month == 12;
isWeekend = day == 6 || day == 7;
isLight = hour < 16;

if (isWinter && isWeekend && isLight) {
    ski = 1;
} else {
    ski = 0;
}
:::

### Numere magice
Numerele 0, 1 și 2, datorită sensurilor evidente pe care le au în diverse contexte, sunt
singurele care ar trebui să fie folosite ca atare.
Restul valorilor constante care sunt necesare în program e bine să fie salvate în variabile,
pentru a evita eventuale confuzii, pentru a face codul ușor de citit și înțeles și pentru a
ușura eventualele modificări.
Intră în această categorie dimensiunile de orice fel, numărul de zile dintr-o lună etc.


### Respectarea convențiilor

Este bine ca numele variabilelor și funcțiilor care se referă la lucruri similare să urmeze aceeași regulă.
În exemplul următor, variabila folosită pentru aria cercului nu respectă convenția de nume.
Deși în momentul în care îi este atribuită valoarea, scopul ei pare evident, faptul că
numele variabilei nu conține și denumirea formei geometrice pe care o reprezintă poate
genera confuzii ulterioare.

:::{code} c
:caption: Nerespectarea convenției de nume
:linenos:

area = M_PI * r * r;
area_tri = 0.5 * b * h;
area_square = l * l;
:::

O altă convenție care trebuie respectată se referă la ordinea parametrilor
în funcțiile cu scop asemănător.

În exemplul următor, cele două funcții, pentru calculul mediei elementelor
unui vector, respectiv ale unei matrici pătratice, nu respectă această regulă.
Dacă un programator va folosi funcția
`meanVec`, iar apoi imediat `meanMat`, cel mai probabil se va aștepta ca primul
parametru al acesteia din urmă să fie tot structura pentru care se calculează media.
În cazul particular al acestui exemplu, compilatorul va semnala imediat eroarea,
deoarece tipul datelor pentru cei doi parametri este diferit. Dacă se întâmplă, însă, ca
aceștia să aibă același tip de date, eroarea va fi mai greu de identificat.

:::{code} c
:caption: Nerespectarea convenției de ordine a parametrilor
:linenos:

int meanVec(int vec[], int dim) {
    int mean = 0, sum = 0;
    int i;
    for (i = 0; i < dim; i++) {
        sum = sum + vec[i];
    }
    mean = sum / dim;
    return mean;
}

int meanMat(int dim, int mat[]) {
    int mean = 0, sum = 0;
    int i, j;
    for (i = 0; i < dim; i++) {
        for (j = 0; j < dim; j++) {
            sum = sum + mat[i][j];
        }
    }
    mean = sum / (dim * dim);
    return mean;
}
:::

În unele situații în care se lucrează cu variabile care testează valoarea de adevăr a
unor expresii, programatorul poate decide să apeleze, de asemenea, la convenții.

Scopul codului din exemplul de mai jos este de a testa dacă
înregistrările dintr-o bază de date asociate unui student sunt complete/valide.
Regula folosită la introducerea datelor este următoarea: dacă respectivul câmp nu
conține date, valoarea asociată acestuia este `-1`.
Programatorul decide să asocieze fiecărui câmp din baza de date câte o variabilă
(booleană) care semnalează validitatea acestuia.
Are două variante: fie valoarea variabilei va fi `True (1)` dacă respectivul
câmp conține date `isValidID`, fie valoarea ei va fi `True` dacă respectivul câmp
este gol (`isEmptyCNP`).
Va trebui să aleagă o variantă, adică să stabilească o convenție,
și să o folosească pentru toate câmpurile.
În caz contrar, ca în exemplu, codul este susceptibil la erori.

:::{code} c
:caption: Nerespectarea convențiilor
:linenos:

if (student.id == -1) isValidID = 0
if (student.cnp == -1) isEmptyCNP = 1
:::

### Cod duplicat
Dacă o secvență de cod este folosită în cel puțin două locuri
diferite din program, atunci secvența respectivă trebuie să fie introdusă într-o funcție
și apelată acolo unde este nevoie.
Altfel, orice modificare adusă liniilor respective de cod
trebuie făcută în toate locurile în care apar.
Observația este cu atât mai importantă în cazul în care
secvențele în cauză apar în fișiere diferite.

Această observație este valabilă inclusiv în unele
situații când secvența de cod nu este folosită în mod identic
în mai multe părți din program, ci ușor modificată: se pot găsi soluții
pentru a trata unitar cazurile particulare de utilizare.
Un exemplu în acest sens va fi discutat in laboratorul de astăzi.

---

## Asigurarea corectitudinii

### Reutilizarea variabilelor
Reutilizarea variabilelor pentru alte scopuri este, de obicei, descurajată,
deoarece poate duce la erori de logică, dacă nu se ține o evidență corectă a sensului
pe care îl are variabila în secvențe diferite de cod.
Fac excepție, firește, variabilele de tip contor, iar observația nu se aplică nici
suprascrierii unei variabile cu rezultatul prelucrării ei.

### Scopul variabilelor

Durata de viață a variabilelor trebuie să respecte scopul acestora: dacă o variabilă nu
este necesară decât în interiorul unei funcții, ea nu trebuie să fie declarată
ca variabilă globală. Argumente în acest sens, dar și situații în care utilizarea
variabilelor globale în C nu este o problemă, găsiți [aici](https://pk.org/rutgers/notes/pdf/Cstyle.pdf).

### Semnalarea rapidă a erorilor
Un cod bine scris va genera eventualele erori cât mai repede cu putință.
Compilatorul face o serie de verificări în urma cărora se pot genera erori,
printre care verificarea sintaxei și a scrierii, a definirii și utilizării corecte a
tipurilor de date, a apelării corecte de funcții (conform numărului de parametri și în
ce privește tipul datelor returnate).
Se verifică, de asemenea, validitatea operațiilor matematice (cum ar fi împărțirea la 0)
și a indexării în date de tip tablou (unde indexul trebuie să aibă mereu o valoare pozitivă).

Aceste verificări, care asigură (în parte) sănătatea codului sunt fie statice
(`static checks`), fie dinamice (`dynamic checks`).
Diferența între ele se referă la momentul în care se produc: înainte de compilare
(static) sau la runtime (dinamic).
Limbaje de programare diferite abordează diferit aceste verificări.
Spre exemplu, C și C++ (ca și Java, Haskell, Pascal ș.a.) sunt limbaje
de tip `static typing`, ceea ce înseamnă că tipurile variabilelor sunt cunoscute la compilare.
Printre limbajele de tip `dynamic typing` se numără Python, Lisp, PHP etc.
În cazul acestora, verificarea tipului variabilelor se face în momentul în care
programul se execută (la runtime).

Însă erorile dintr-un program pot fi și de natură logică, pe care niciuna din
verificările anterioare nu le poate observa.
Rămâne responsabilitatea programatorului să verifice dacă anumite condiții ce țin
de logica algoritmului sunt îndeplinite și să semnaleze cât de repede se poate
dacă nu sunt respectate.
Altfel, erorile pot trece neobservate și genera confuzii mai târziu, făcând mai grea
identificarea lor.

Un mod prin care programatorul se poate asigura că evită erorile este
prin documentarea și verificarea presupunerilor.
Spre exemplu, dacă se așteaptă ca o funcție să primească o variabilă
ce reprezintă o zi calendaristică, trebuie verificat faptul că valoarea
primită face parte din valorile admise.

---

## Ghiduri de stil

Companiile au, de cele mai multe ori, ghiduri de stil pe care programatorii angajați
trebuie să le repecte, astfel încât programele pe care le dezvoltă să fie unitare,
iar membrilor echipei să le fie ușor să lucreze cu codul scris de colegi.
Dacă în cazul companiilor aceste documente sunt destinate uzului intern, unele
universități sau profesori au făcut publice ghiduri similare, spre exemplu
  [Krzyzanowski](https://pk.org/rutgers/notes/pdf/Cstyle.pdf),
  [Pérez](https://www.cs.umd.edu/~nelson/classes/resources/cstyleguide/),
  [CS50](https://cs50.readthedocs.io/style/c/),
  [Thereska](https://users.ece.cmu.edu/~eno/coding/CCodingStandard.html).
Dezvoltatorii kernel-ului Linux au, de asemenea, un [ghid de stil](https://www.kernel.org/doc/html/v4.10/process/coding-style.html).
Pentru alte limbaje de programare există ghiduri universale, ce trebuie
respectate de toți programatorii, indiferent de afiliere, cum este cazul
[python](https://peps.python.org/pep-0008/).

Veți observa că uneori regulile sunt diferite.
Spre exemplu, unele ghiduri pentru limbajul C recomandă ca în cazul
instrucțiunilor decizionale acolada care deschide structura să fie plasată pe
aceeași linie cu aceasta, iar cea care o închide să fie plasată pe o linie separată.
În cazul în care există și o ramură `else`, aceasta trebuie plasată
pe aceeași linie care închide ramura `if`, ca în exemplul de mai jos.

:::{code} c
:caption: Poziționarea acoladelor - varianta 1
:linenos:

if (x == 0) {
    a = 5;
} else {
    a = 2 * x;
}
:::

Alte ghiduri acceptă o altă variantă de formatare a acestor instrucțiuni, în care toate acoladele sunt plasate pe linii distincte, ca în exemplul următor.

:::{code} c
:caption: Poziționarea acoladelor - varianta 2
:linenos:

if (x == 0)
{
    a = 5;
}
else
{
    a = 2 * x;
}
:::

Chiar dacă alegerea unei anumite variante poate părea o problemă de preferință personală,
toate au ca scop lizibilitatea codului.
Varianta potrivită poate depinde, de asemenea, și de natura și complexitatea aplicațiilor.

Cu toate că limbajul C permite omiterea acoladelor atunci când blocul de cod conține o singură
linie, este recomandat ca ele să fie folosite și în această situație, pentru a semnala mai clar
intenția din spatele respectivelor instrucțiuni.
În plus, dacă este necesară adăugarea unor noi linii în blocul respectiv, inclusiv pentru a facilita
depanarea, acoladele trebuie introduse.