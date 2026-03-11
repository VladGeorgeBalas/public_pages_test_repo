---
weight: 3
title: "Laborator 3 - Valori si adrese"
---

## Numere aleatorii

În multe aplicații din statistică sau criptografie este necesară generarea unor numere aleatorii.
Ele sunt de asemenea utile în testarea performanțelor unui algoritm, când se folosesc, cel mai adesea, inputuri aleatorii pentru a evita testarea situațiilor favorabile sau defavorabile și a obține o medie realistă ce caracterizează performanțele.

Însă, datorită naturii deterministe a calculatoarelor, acestea nu pot genera numere cu adevărat aleatorii.
O pot face dacă se bazează pe procese externe, de regulă fizice, care produc astfel de numere, cum ar fi zgomotul atmosferic sau unele procese electromagnetice.
În lipsa unui astfel de proces, numerele ce pot fi generate cu un calculator se numesc pseudo-aleatorii, adică
au proprietăți suficient de asemănătoare celor aleatorii.

Generatoarele de numere pseudoaleatorii sunt algoritmi care folosesc o valoare de inițializare, denumită `seed`, pentru a genera numere ce urmăresc o anumită distribuție (uniformă, normală etc).
Prezența seedului face ca outputul funcției să fie predictibil, spre deosebire de cazul numerelor cu adevărat aleatorii.

În C, pentru a genera numere întregi aleatorii cu distribuție uniformă puteți folosi biblioteca `time.h`, după cum urmează:

:::{code} c
:linenos:
#include <time.h>

srand(time(NULL));
int x = rand();
:::

Linia 3 din codul anterior generează seedul de la care se vor genera ulterior numerele aleatorii.
Aceasta, reprezentând o inițializare, este necesară o singură dată (exceptând cazul în care în aplicație
este necesară în mod explicit reinițializarea acestuia).

Această funcție trebuie utilizată, însă, cu precauție, datorită faptului că prezintă
probleme de securitate: dacă sunt necesare numere cu adevărat aleatorii, spre exemplu pentru a
nu fi știute de terți, funcția nu este potrivită deoarece cunoașterea seedului poate duce la cunoașterea
tuturor numerelor aleatorii generate ulterior în baza acestuia.

Numărul aleator va fi generat uniform în intervalul `[0, RAND_MAX]`, unde `RAND_MAX` este, de regulă, `32767`.
În cazul în care se dorește generarea unor numere în alt interval, se poate scala rezultatul
obținut cu funcția `rand()` la intervalul dorit, cu observația că această scalare nu va conserva
întocmai proprietatea de uniformitate a distribuției.

Presupunem că dorim găsirea valorii corespunzătoare lui $x$ din intervalul $[x_1, x_2]$ în
noul interval $[k_1, k_2]$. Pentru scalare vom aplica formula:

$$
\begin{equation*}
	x_{scale} = (k_2 - k_1)\frac{x - x_1}{x_2 - x_1} + k_1, \forall x \in [x_1, x_2].
\end{equation*}
$$

Alte funcții pentru generarea de numere aleatorii (provenind din alte biblioteci, nu din cea standard)
permit generarea și a altor tipuri de distribuții (nu doar uniformă), permit specificarea
intervalului în care să se găsească numărul (pentru a evita pierderea uniformității la scalare)
sau specifică în mod explicit algoritmul de generare a numerelor.
Pentru scopul educativ al aplicațiilor din acest manual, însă, funcția `rand()` este suficient de bună.

## Pointeri

În mod obișnuit, o variabilă este înțeleasă ca un nume simbolic care are asociat un tip de date și
o valoare (de exemplu, `int x = 2`).
Această definiție este valabilă până în momentul compilării, al cărei scop, printre altele,
este de a traduce expresiile ce folosesc denumiri simbolice în instrucțiuni cod mașină.
Din momentul compilării, variabilele devin adrese de memorie, iar valorile lor asociate sunt valorile
care se găsesc în respectivele adrese.

Variabilele de tip pointeri sunt un tip de date care nu stochează direct o valoare, ci adresa unei locații de memorie.
De cele mai multe ori este vorba de o locație de memorie unde se găsește o valoare a altei variabile.

Un pointer are, ca orice alt tip de variabilă, asociate o adresă și o valoare.
Presupunem că un pointer `p` este folosit pentru a reține adresa unei variabile `x`.

* Valoarea lui `p` este egală cu adresa variabilei `x`.
* Adresa lui `p` va fi deci adresa unde este stocată adresa variabilei `x`.

<!-- %Adresa lui \lstinline|p| este adresa de memorie la care se găsește pointerul \lstinline|p|, deci va fi adresa unde este stocată adresa variabilei \lstinline|x|. \\ -->

<!-- %Valoarea \textit{r-value} a lui \lstinline|p| este egală cu \textit{l-value} a variabilei \lstinline|x| (adresa variabilei~\lstinline|x|). \\
%Valoarea \textit{l-value} a lui \lstinline|p| este adresa de memorie la care se găsește pointerul \lstinline|p|, deci va fi adresa unde este stocată adresa variabilei \lstinline|x|. \\ -->

## Operații cu pointeri
Pointerii sunt tipuri de date derivate.
Pentru a declara un pointer către o variabilă de tip `int` se  utilizează `int *`, pentru a declara un
pointer către o variabilă de tip `void` se utilizează `void *` ș.a.m.d..
Așadar un pointer nu este doar o adresă, ci o adresă care are asociată un tip de date.

Cu toate că este necesară specificarea tipului de date către care referențiază un pointer,
spațiul pe care îl ocupă este același indiferent de tipul variabilei a cărei adresă o stochează:
valoarea tuturor pointerilor este o adresă.
Mai exact, spațiul ocupat de un pointer este de 4 sau 8 biți (în funcție de arhitectură).
De aici decurge un beneficiu al utilizării pointerilor: dacă se dorește transmiterea unei
variabile (spre exemplu, într-o funcție), este mai eficientă transmiterea adresei acesteia,
printr-un pointer, decât a valorii, prin variabila propriu-zisă, care poate fi mult mai mare decât 4 biți.

Deoarece pointerii fac legătura dintre o valoare și adresa la care este salvată aceasta,
lucrul cu pointerii presupune două tipuri de operații:
1) dându-se o variabilă, se dorește aflarea adresei de memorie unde se găsește și
2) dându-se o adresă, se dorește aflarea valorii salvate acolo.

Prima operație se efectuează cu operatorul referință, `&`, care calculează adresa elementului către care
indică un pointer.

:::{code} c
int x;
int* p;
x = 5;
p = &x;	// p stores the address of x
:::

Operația inversă se realizează cu operatorul de dereferențiere `*`, care urmărește referința
unui pointer, cu scopul de a recupera valoarea către care indică.
Prin urmare, utilizând operatorii `*` și `&` putem obține o echivalență de tipul:

`*p` -> `x` -> `*(&x)`

Evident, pentru a putea utiliza operatorul de dereferențiere, pointerul trebuie să conțină o adresă validă.
Dacă un pointer este declarat ca variabilă globală, însă neinițializat, el va fi în mod automat inițializat cu
`NULL`, adică o constantă care semnalează că pointerul nu referențiază ceva.
În cazul în care variabila este locală, un pointer va fi inițializat implicit cu o valoare aleatorii
(ca orice variabilă, de altfel), care poate corespunde unei adrese de memorie neaccesibile programatorului.
Este inclusă aici situația în care pointerul este declarat în `main()`, care este de asemenea o funcție.
Pentru a evita eventuale erori (de logică), este indicată inițializarea cu `NULL` a pointerilor declarați locali.

Pointeri au avantajul că oferă programatorului posibilitatea de a lucra direct cu memoria, dar
necesită și atenție în plus.

![pointeri-2](/images/lab3/2pointeri.png "Copie superficială și copie adâncă")

În exemplul următor pointerii `p1` și `p2` referențiază către aceeași zonă de memorie, în care
se găsește variabila `x`. Cazul corespunde diagramei din stânga din figura {{< addlink name="pointeri-2" text="de mai sus" >}}.

Orice modificare adusă lui `x` prin intermediul lui `p1` va fi vizibilă și pentru `p2` și viceversa.
În acest caz, `p2` se numește **copie superficială**.
După execuția liniei 6 din exemplul de mai jos, dacă dorim să aflăm ce valoare se află salvată la
adresa de memorie stocată în pointerul `p1`, vom utiliza operatorul de referință, `*p1` și vom obține
valoarea **6**, anume cea a variabilei `x1` incrementată. Același rezultat îl vom obține și prin `*p2`.

În capitolul următor vom întâlni un exemplu de situație în care o astfel de copie este utilă.

:::{code} c
:caption: Copie superficiala

int x1;
int* p1, p2;
x1 = 5;
p1 = &x1;
p2 = p1;
x1++;
:::

Alteori, din contră, este nevoie ca acțiunea a doi pointeri diferiți asupra unei valori să fie realizată
independent. Această situație se poate rezolva creând o copie a variabilei, ca în exemplul de mai jos.
În acest caz, ce corespunde diagramei din dreapta din figura {{< addlink name="pointeri-2" text="de mai sus" >}}, `p2` se numește copie adâncă.
După execuția liniei 7 din exemplul de mai jos, dacă afișăm valoarea stocată la adresa de memorie
pe care o reține pointerul `p1`, utilizând operatorul `*`, **vom obține valoarea 6**, anume cea a
variabilei `x1` incrementată. În schimb, `*p2` **va returna valoarea 5**, deoarece aceasta se găsește
la altă locație de memorie față de cea a variabilei `x1`.

:::{code} c
:caption: Copie adanca

int x1, x2;
int* p1, p2;
x1 = 5;
x2 = x1;
p1 = &x1;
p2 = &x2;
x1++;
:::

## 4) Pointeri și tablouri

Atunci când se declară o variabilă de tip tablou, numele său este echivalent cu un pointer
către adresa primului element din tablou.
Variabila în sine nu este un pointer, dar se comportă ca și cum ar fi.
Regula care face posibilă această echivalență se numește **decay to pointer**.

Există doar trei excepții, situații în care variabila se comportă conform tipului ei,
nu ca un pointer, și anume: utilizarea variabilei împreună cu operatorul `sizeof`, cu operatorul referință,
`&`, și cazul în care variabila este un tablou de
caractere inițializat cu valoare constantă (de ex. `const char* nume = "Popescu";`).

În exemplul de mai jos se afișează dimensiunea tabloului `a`, iar rezultatul este 40, anume spațiul
pentru 10 elemente de tip `int`, a câte 4 biți fiecare.
Dacă `sizeof` nu ar constitui o excepție de la **decay to pointer**, rezultatul ar fi dimensiunea
unui pointer la `int` și ar fi destul de nefolositor.

:::{code} c
:caption: Aflarea dimensiunii unui tablou

int a[10];
printf("sizeof a: %zu \n", sizeof(a));
:::

Însă, odată ce un tablou a fost echivalat cu un pointer (s-a produs **decay to pointer**), nu se mai
poate reveni la tablou, variabila trebuie tratată ca pointer.
Această situație se întâmplă, cel mai adesea, când un tablou este transmis
într-o funcție, ca în exemplul următor.

:::{code} c
:caption: Decay to pointer

void f(int* x){
	printf("sizeof x: %zu", sizeof(x));
}

int main(){
	int v[10];
	printf("sizeof v: %zu", sizeof(v));

	f(v);
	return 0;
}
:::

Funcția `f` primește ca parametru un pointer.
În `main` este apelată cu tabloul `v`, ceea nu contravine compatibilității între tipurile de date,
având în vedere că `v` este echivalent cu un pointer.
**Însă atunci când se încearcă aflarea dimensiunii lui `x` în interiorul funcției,
rezultatul este 8, anume dimensiunea unui pointer, și nu 40, dimensiunea tabloului**.

Mai mult, operatorul `[]`, prin care se indexează un vector este implementat în C sub forma
`v[i] == *(v + i)`.

Datorită faptului că tabloul poate fi folosit ca pointer (mai puțin în excepțiile discutate anterior),
în practică apare rar situația în care este nevoie ca o variabilă să fie un pointer la tablou.
Însă acest tip poate apărea în programe ca o consecință a **decay to pointer**:
dacă o matrice este transmisă într-o funcție (similar cu felul în care vectorul `v` este
transmis în `f` în exemplul anterior), variabila va fi echivalată cu un pointer la tablou.

## 5) Alocarea dinamică

În limbajul C alocarea, realocarea și dealocarea dinamică a spațiului se face cu ajutorul următoarelor funcții:
:::{code} c
void* malloc(size_t size);
void* calloc(size_t num, size_t size);
void* realloc(void* p, size_t new_size);
void* free(void* p);
:::

Funcțiile pentru alocare de spațiu (`malloc`, `calloc`, `realloc`) returnează un pointer către zona
de memorie care a fost alocată.
În cazul în care nu s-a putut realiza alocarea, pointerul returnat este `NULL`.
Din acest motiv, este necesară testarea valorii acestuia pentru a verifica alocarea.

Diferența dintre funcțiile `malloc` și `calloc` este că prima (sau `malloc`) nu inițializează datele,
iar a doua (adica `calloc`) inițializează variabila pentru care se alocă spațiu cu 0 sau `NULL`
(în funcție de tipul acesteia).

Pentru a realoca spațiul pentru o variabilă, se folosește funcția `realloc`.
Aceasta primește ca parametru un pointer către zona de memorie care trebuie realocată și
noua dimensiune necesară.
În cazul în care funcția `realloc` primește ca parametru pointer la `NULL`, aceasta se comportă la fel ca
funcția `malloc`.
Dacă realocarea nu s-a putut realiza (spre exemplu, din lipsă de spațiu), `realloc` va returna un
pointer `NULL`.
De aceea, este indicat să se evite suprascrierea directă a unui pointer ca urmare a realocării,
deoarece se poate pierde adresa datelor.
O copie (superficială) a acestora poate preîntâmpina această situație.

În momentul în care variabila către care referențiază pointerul nu mai este necesară, se recomandă
eliberarea spațiului și setarea pointerului la valoarea `NULL`.
Se evită astfel situația în care zona de memorie a fost eliberată, dar pointerul indică în continuare
către ea **dangling pointer**).

**Echivalența dintre pointeri și tablouri face posibil ca pointerul returnat de
funcția `malloc` să fie interpretat ca tablou și folosit ca atare.**
În exemplul următor este alocat spațiu pentru trei întregi și returnat pointerul `p`
către blocul respectiv de memorie.
Pointerul poate fi folosit ca un tablou de trei elemente, cu indexarea specifică tablourilor, `[]`.

:::{code} c
:caption:Inițializarea unui tablou ca pointer
:linenos:

int dim = 3;
int* p = (int*) malloc(dim * sizeof(int));
p[1] = 1234;
printf("p[1] = %d", p[1]);
:::
