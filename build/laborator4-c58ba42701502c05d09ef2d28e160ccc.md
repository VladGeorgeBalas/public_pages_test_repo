---
weight: 4
title: 'Laborator 4 - Liste'
---

## 1) Liste simplu înlănțuite

O listă simplu înlănțuită este o structură liniară de date, în care fiecare element (denumit și nod)
conține, pe lângă informația utilă, adresa elementului următor. Excepție face ultimul element al
listei, a cărui adresă următoare este `NULL`.
Figura {{< addlink name="figura1" text="de mai jos" >}} prezintă structura unei liste.

![figura1](/images/lab4/liste.jpg "Listă simplu înlănțuită")

Un element al listei poate fi declarat ca o structură, ca în exemplul următor:

:::{code} c
:caption: Declararea unui element de listă
:linenos:

struct Elem {
    int val;            // info about the node
    struct Elem* next;  // adress of the next node
};

typedef struct Elem Node;
:::

Lista poate fi parcursă doar element cu element, spre exemplu de la stânga la dreapta.
Nu există posibilitatea de a reveni la un element anterior, de aceea adresa de început
a listei trebuie cunoscută mereu, pentru a putea relua parcurgerea de câte ori este nevoie.

Toate operațiile care presupun schimbarea structurii unei liste (inserarea sau ștergerea de elemente)
necesită refacerea legăturilor între elementele acesteia.
Datorită tipului de organizare secvențial, poziția unui element în listă (început, final sau
un element oarecare) influențează modul în care acesta trebuie tratat în diferite operații.

Prezentăm în continuare operația de adăugare a unui element în listă; cea de ștergere este
lăsată ca exercițiu cititorului.

## 2) Adăugarea unui element la începutul listei

Pentru a crea o listă și a adăuga un element se urmează pași:
* declararea unui pointer, denumit în continuare `head` și inițializarea acestuia cu `NULL`.
Acesta va reprezenta începutul listei;
* crearea unui element de listă;
* actualizarea pointerului `head` cu adresa noului element creat.

Figura {{< addlink name="figura2" text="de mai jos" >}} prezintă schematic această operație de adăugare.
Observați că pointerul `next` al noul nod creat are valoarea `NULL`.

![figura2](/images/lab4/add_first_null.png "Adăugarea unui element într-o listă vidă")

În cazul în care se dorește adăugarea unui element la începutul unei liste care conține deja cel puțin un nod, pașii de urmat sunt:
* Crearea unui element de listă.
* Legarea noului nod la primul element existent în listă prin setarea pointerul `next` al noului nod
la adresa primul nod din listă.
* Actualizarea începutului listei prin setarea pointerului `head` la adresa noului element introdus.

Schema acestei operații este prezentată în figura {{< addlink name="figura3" text="de mai jos" >}}.
Observați că adăugarea unui nod nou presupune ștergerea unei legături deja existente în
listă și adăugarea a două legături noi.
Aceste operații sunt necesare indiferent de poziția în listă unde se dorește introducerea unui element.
Ștergerea legăturii vechi nu se realizează explicit, ea se realizează *prin* adăugarea celor două legături
noi, desenate cu albastru în figură.
Ordinea în care se creează aceste două legături este importantă: dacă am crea întâi legătura din
stânga (care presupune setarea adresei începutului listei la noul element) s-ar pierde adresa
primului element și nu ar mai putea fi stabilită legătura din dreapta.
Prin urmare, întotdeauna, prima legătură care se realizează este cea din dreapta.

![figura3](/images/lab4/add_first.png "Adăugarea unui element la începutul listei")

## 3) Adăugarea unui element oarecare în listă
Cazul în care un nod nou trebuie adăugat pe o poziție oarecare este similar adăugării la
începutul listei, însă necesită o operație suplimentară: lista trebuie parcursă până se ajunge la
elementul *dinaintea* poziției în care se dorește adăugarea nodului. Pointerul `next` al acestui element
va fi actualizat cu adresa nodului nou, însă nu înainte de a lega noul nod la
elementul următor din listă (legătura din dreapta).

Parcurgerea listei presupune avansarea element cu element utilizând pointerii `next`, până se ajunge
la `NULL`, ca în exemplul de mai jos.

:::{code} c
:caption: Parcurgerea listei
:linenos:
while (head != NULL) {
	head = head->next;
}
:::

Variabila `head` este un pointer, care la început reprezintă începutul listei.
La fiecare pas al acestei structuri repetitive, însă, adresa de memorie către care
indică acest pointer se va modifica, astfel că se va pierde adresa de început.
Pentru a evita această situație, de fiecare dată când este necesară parcurgerea unei liste,
dar și reținerea primei adrese, se va face o copie superficială a adresei primului element.
Figura {{< addlink name="figura4" text="de mai jos" >}} prezintă schema acestei operații de adăugare.

![figura4](/images/lab4/add_middle.png "Adăugarea unui element oarecare în listă")

## 4) Adăugarea unui element la sfârșitul listei {#adaugarea-unui-element-la-sfarsitul-listei}
Figura {{< addlink name="figura5" text="de mai jos" >}} prezintă cazul în care un element este introdus la finalul listei.
Operația presupune parcurgerea listei până la ultimul element și actualizarea
pointerului `next` al acestuia cu adresa noului nod.

![figura5](/images/lab4/add_last.png "Adăugarea unui element la sfârșitul listei")

## 5) Liste dublu înlănțuite
Faptul că o listă simplu înlănțuită poate fi parcursă într-o singură direcție și nevoia
de a reține o copie a adresei de început a unei liste, pentru a evita pierderea acesteia
atunci când lista este parcursă reprezintă două inconveniente ale listelor simplu înlănțuite.
Acestea pot fi rezolvate prin adăugarea în structura unui element de listă a unui pointer
către elementul anterior.
Lista care rezultă din această operație se numește listă dublu înlănțuită.

:::{code} c
:caption: Element al unei liste dublu înlănțuite
:linenos:
struct Elem {
	int val;            // informatia utila
	struct Elem* prev;  // adresa nodului anterior
	struct Elem* next;  // adresa nodului urmator
};

typedef struct Elem Node;
:::

Pointerul `prev` al primului element din listă are valoarea `NULL` pentru a semnala începutul listei.

Avantajul de a putea parcurge liste în ambele direcții vine cu inconvenientul operațiilor
suplimentare atunci când se dorește adăugarea, modificarea sau ștergerea
unui element dintr-o listă dublu înlănțuită: trebuie actualizate atât legăturile către
elementul următor (prin pointerul `next`), cât și cele către elementul
anterior (prin pointerul `prev`).
