# Lekcija 4: List\<T\>

**Programiranje, 4\. razred · Nastavna celina: Generički tipovi u C\#**

Lekcija se radi na dva časa: **deo A** (osnove liste) i **deo B** (pretraga, sortiranje, lista objekata i česte greške).

## Cilj lekcije

Posle ove lekcije znaćeš:

- da napraviš **listu** `List<T>` i da u nju dodaješ, ubacuješ i iz nje uklanjaš elemente,  
- da prođeš kroz listu petljama `for` i `foreach` i da koristiš metode `Contains`, `IndexOf`, `Sort` i `Reverse`,  
- kada da koristiš niz, a kada listu, i kako da izbegneš dve najčešće greške sa listama.

---

## Mali rečnik pojmova (nastavak)

| Srpski | Engleski | Primer |
| :---- | :---- | :---- |
| kolekcija | collection | lista, rečnik |
| lista | list | `List<int>` |
| element | element / item | `5` u listi ocena |
| indeks | index | `ocene[0]` |
| inicijalizator kolekcije | collection initializer | `new List<int> { 5, 4, 3 }` |

---

## Podsetimo se

- **Generička klasa** ima parametar tipa: `Kutija<T>`. Kada pravimo objekat, biramo argument tipa: `new Kutija<int>()`.  
- **Niz** ima **fiksnu dužinu** koja se zadaje pri pravljenju: `int[] ocene = new int[5];`. Dužinu čitamo svojstvom `Length`.  
- Elementima niza pristupamo **indeksom** od `0` do `Length - 1`.  
- Klasa `Ranac<T>` iz prošle lekcije imala je problem: kada se napuni, više ništa ne može da se doda.

---

## Objašnjenje

### Šta je List\<T\>?

`List<T>` je **gotova generička klasa** koja dolazi uz C\#. Ona čuva elemente tipa `T` redom, kao niz, ali **sama raste i smanjuje se**: kad dodamo element, lista se produži, a kad ga uklonimo, lista se skrati.

To je **kolekcija (collection)**: objekat koji čuva više vrednosti. Sada kada znaš šta je `<T>`, zapis `List<int>` je jasan: to je lista čiji su elementi tipa `int`.

Da bismo koristili `List<T>`, na vrhu programa mora da postoji red:

using System.Collections.Generic;

Visual Studio 2010 ga sam dodaje kada napraviš novu konzolnu aplikaciju. Nemoj ga brisati.

### Niz i lista

|  | Niz `int[]` | Lista `List<int>` |
| :---- | :---- | :---- |
| Dužina | fiksna, zadaje se pri pravljenju | menja se sama |
| Broj elemenata | `Length` | `Count` |
| Pravljenje | `new int[5]` | `new List<int>()` |
| Dodavanje i uklanjanje | nije moguće | `Add`, `Insert`, `Remove`, `RemoveAt` |
| Pristup elementu | `niz[i]` | `lista[i]` |
| Kada ga koristiti | broj elemenata je poznat i ne menja se (dani u nedelji) | broj elemenata se menja (ocene, inventar, spisak učenika) |

### Analogija iz svakodnevnog života

**Niz je kao kutija za jaja:** ima tačno 10 mesta. Ne možeš da dodaš jedanaesto jaje, a ne možeš ni da „izbaciš“ mesto iz kutije.

**Lista je kao spisak u telefonu (beleške):** dopisuješ stavke na kraj, ubacuješ novu stavku između dve postojeće, brišeš ono što ne treba. Redni brojevi se **sami pomeraju**: kad obrišeš drugu stavku, treća postaje druga.

---

## Sintaksa

using System.Collections.Generic;

List\<int\> ocene \= new List\<int\>();                    // prazna lista

List\<string\> imena \= new List\<string\> { "Ana", "Luka" };  // lista sa početnim elementima

ocene.Add(5);              // dodaje element na kraj

ocene.Insert(0, 4);        // ubacuje element 4 na indeks 0

ocene.Remove(5);           // uklanja PRVO pojavljivanje vrednosti 5

ocene.RemoveAt(0);         // uklanja element na indeksu 0

int x \= ocene\[0\];          // čitanje po indeksu

ocene\[0\] \= 3;              // izmena po indeksu

int broj \= ocene.Count;    // broj elemenata

bool ima \= ocene.Contains(5);   // da li lista sadrži vrednost

int gde \= ocene.IndexOf(5);     // indeks prvog pojavljivanja ili \-1

ocene.Sort();                   // sortira rastuće

ocene.Reverse();                // obrće redosled

ocene.Clear();                  // briše sve elemente

---

## Primeri: deo A (osnove liste)

### Primer 1: Ocene u dnevniku

using System;

using System.Collections.Generic;   // ovde se nalazi List\<T\>

namespace Lekcija4Primer1

{

    class Program

    {

        static void Main(string\[\] args)

        {

            List\<int\> ocene \= new List\<int\>();   // prazna lista celih brojeva

            ocene.Add(5);

            ocene.Add(3);

            ocene.Add(4);

            Console.WriteLine("Broj ocena: " \+ ocene.Count);

            Console.WriteLine("Prva ocena: " \+ ocene\[0\]);

            ocene\[1\] \= 4;          // ispravka ocene na indeksu 1

            ocene.Add(5);          // lista se sama produžava

            Console.Write("Ocene:");

            for (int i \= 0; i \< ocene.Count; i++)

            {

                Console.Write(" " \+ ocene\[i\]);

            }

            Console.WriteLine();

            int zbir \= 0;

            foreach (int ocena in ocene)

            {

                zbir \= zbir \+ ocena;

            }

            Console.WriteLine("Zbir: " \+ zbir \+ ", broj ocena: " \+ ocene.Count);

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Broj ocena: 3

Prva ocena: 5

Ocene: 5 4 4 5

Zbir: 18, broj ocena: 4

**Objašnjenje:**

1. `using System.Collections.Generic;`: bez ovog reda kompajler ne zna šta je `List`.  
2. `List<int> ocene = new List<int>();`: pravimo **praznu** listu. Njen `Count` je `0`.  
3. `ocene.Add(5);`: `Add` dodaje element **na kraj** liste. Posle tri poziva `Count` je `3`.  
4. `ocene[1] = 4;`: postojeći element menjamo po indeksu, isto kao kod niza. Ocena `3` postaje `4`.  
5. `i < ocene.Count`: kod liste koristimo `Count`, a ne `Length`.  
6. `foreach (int ocena in ocene)`: `foreach` prolazi kroz sve elemente redom. Tip promenljive (`int`) mora da odgovara tipu elemenata liste.

### Primer 2: Inventar u igrici (Insert, Remove, RemoveAt)

using System;

using System.Collections.Generic;

namespace Lekcija4Primer2

{

    class Program

    {

        // Generička metoda (Lekcija 2), sada za listu: ispisuje indeks i element

        static void IspisiListu\<T\>(List\<T\> lista)

        {

            for (int i \= 0; i \< lista.Count; i++)

            {

                Console.WriteLine("  \[" \+ i \+ "\] " \+ lista\[i\]);

            }

        }

        static void Main(string\[\] args)

        {

            // Inicijalizator kolekcije: lista odmah dobija tri elementa

            List\<string\> inventar \= new List\<string\> { "sekira", "kaciga", "napitak" };

            Console.WriteLine("Inventar pre izmena:");

            IspisiListu(inventar);

            inventar.Insert(1, "mapa");     // ubacuje na indeks 1, ostali se pomeraju udesno

            inventar.Remove("napitak");     // uklanja po vrednosti

            inventar.RemoveAt(0);           // uklanja po indeksu

            inventar.Add("luk");            // dodaje na kraj

            Console.WriteLine("Inventar posle izmena:");

            IspisiListu(inventar);

            Console.WriteLine("Ukupno predmeta: " \+ inventar.Count);

            bool uklonjen \= inventar.Remove("zmaj");   // "zmaj" nije u listi

            Console.WriteLine("Uklonjen zmaj? " \+ uklonjen);

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Inventar pre izmena:

  \[0\] sekira

  \[1\] kaciga

  \[2\] napitak

Inventar posle izmena:

  \[0\] mapa

  \[1\] kaciga

  \[2\] luk

Ukupno predmeta: 3

Uklonjen zmaj? False

**Objašnjenje:**

1. `new List<string> { "sekira", "kaciga", "napitak" }`: **inicijalizator kolekcije (collection initializer)**. Lista se pravi i odmah puni.  
2. `static void IspisiListu<T>(List<T> lista)`: generička metoda radi za listu bilo kog tipa.  
3. `inventar.Insert(1, "mapa");`: ubacuje „mapa“ na indeks 1\. Elementi od indeksa 1 nadalje pomeraju se za jedno mesto udesno. Lista je sada: sekira, mapa, kaciga, napitak.  
4. `inventar.Remove("napitak");`: uklanja element **po vrednosti**. Lista: sekira, mapa, kaciga.  
5. `inventar.RemoveAt(0);`: uklanja element **po indeksu**. Posle toga se svi pomeraju ulevo: mapa je sada na indeksu 0\.  
6. `inventar.Remove("zmaj")`: ako elementa nema, ništa se ne uklanja i metoda vraća `false`. Program se **ne** prekida.

---

## Primeri: deo B (pretraga, sortiranje, lista objekata)

### Primer 3: Rezultati takmičenja (Contains, IndexOf, Sort, Reverse, Clear)

using System;

using System.Collections.Generic;

namespace Lekcija4Primer3

{

    class Program

    {

        // Ispisuje naslov i sve elemente liste u jednom redu

        static void IspisiRed\<T\>(string naslov, List\<T\> lista)

        {

            Console.Write(naslov \+ ":");

            foreach (T element in lista)

            {

                Console.Write(" " \+ element);

            }

            Console.WriteLine();

        }

        static void Main(string\[\] args)

        {

            // Poeni učenika na školskom takmičenju iz programiranja

            List\<int\> poeni \= new List\<int\> { 72, 95, 60, 88, 95 };

            Console.WriteLine("Ima li 100 poena? " \+ poeni.Contains(100));

            Console.WriteLine("Prvih 95 poena je na indeksu " \+ poeni.IndexOf(95));

            Console.WriteLine("Indeks za 50 poena: " \+ poeni.IndexOf(50));

            poeni.Sort();                        // rastući redosled

            IspisiRed("Sortirano", poeni);

            poeni.Reverse();                     // obrće redosled

            IspisiRed("Obrnuto", poeni);

            Console.WriteLine("Najbolji rezultat: " \+ poeni\[0\]);

            List\<string\> takmicari \= new List\<string\> { "Milica", "Ana", "Luka" };

            takmicari.Sort();                    // stringovi se sortiraju po abecedi

            IspisiRed("Po abecedi", takmicari);

            poeni.Clear();                       // briše sve elemente

            Console.WriteLine("Posle Clear: " \+ poeni.Count \+ " elemenata");

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Ima li 100 poena? False

Prvih 95 poena je na indeksu 1

Indeks za 50 poena: \-1

Sortirano: 60 72 88 95 95

Obrnuto: 95 95 88 72 60

Najbolji rezultat: 95

Po abecedi: Ana Luka Milica

Posle Clear: 0 elemenata

**Objašnjenje:**

1. `poeni.Contains(100)`: vraća `true` ili `false`, u zavisnosti od toga da li vrednost postoji u listi.  
2. `poeni.IndexOf(95)`: vraća indeks **prvog** pojavljivanja. Iako se 95 pojavljuje dva puta, rezultat je `1`.  
3. `poeni.IndexOf(50)`: kada vrednosti nema, `IndexOf` vraća `-1`. Isto smo radili ručno u metodi `NadjiIndeks` u Lekciji 2\.  
4. `poeni.Sort();`: sortira listu **rastuće**. Menja samu listu, ne pravi novu.  
5. `poeni.Reverse();`: obrće redosled. Posle `Sort` i `Reverse` lista je sortirana opadajuće, pa je najbolji rezultat na indeksu `0`.  
6. `takmicari.Sort();`: lista stringova sortira se po abecedi.  
7. `poeni.Clear();`: briše sve elemente. Lista i dalje postoji, ali je prazna (`Count` je `0`).

### Primer 4: Lista objekata klase Ucenik

using System;

using System.Collections.Generic;

namespace Lekcija4Primer4

{

    class Ucenik

    {

        public string Ime { get; set; }

        public int Ocena { get; set; }

        public Ucenik(string ime, int ocena)

        {

            Ime \= ime;

            Ocena \= ocena;

        }

    }

    class Program

    {

        static void Main(string\[\] args)

        {

            List\<Ucenik\> odeljenje \= new List\<Ucenik\>();

            odeljenje.Add(new Ucenik("Ana", 5));

            odeljenje.Add(new Ucenik("Luka", 3));

            odeljenje.Add(new Ucenik("Milica", 4));

            odeljenje.Add(new Ucenik("Nikola", 2));

            Console.WriteLine("Spisak:");

            foreach (Ucenik u in odeljenje)

            {

                Console.WriteLine("  " \+ u.Ime \+ " \- " \+ u.Ocena);

            }

            // Traženje učenika sa najvećom ocenom

            Ucenik najbolji \= odeljenje\[0\];

            foreach (Ucenik u in odeljenje)

            {

                if (u.Ocena \> najbolji.Ocena)

                {

                    najbolji \= u;

                }

            }

            Console.WriteLine("Najbolji: " \+ najbolji.Ime);

            // Nova lista sa imenima učenika koji imaju ocenu 4 ili 5

            List\<string\> dobri \= new List\<string\>();

            foreach (Ucenik u in odeljenje)

            {

                if (u.Ocena \>= 4\)

                {

                    dobri.Add(u.Ime);

                }

            }

            Console.WriteLine("Ocenu 4 ili 5 imaju: " \+ string.Join(", ", dobri));

            odeljenje\[1\].Ocena \= 4;   // Luka je popravio ocenu

            Console.WriteLine(odeljenje\[1\].Ime \+ " sada ima " \+ odeljenje\[1\].Ocena);

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Spisak:

  Ana \- 5

  Luka \- 3

  Milica \- 4

  Nikola \- 2

Najbolji: Ana

Ocenu 4 ili 5 imaju: Ana, Milica

Luka sada ima 4

**Objašnjenje:**

1. `List<Ucenik> odeljenje`: argument tipa može da bude i **naša klasa**. Svaki element liste je jedan objekat tipa `Ucenik`.  
2. `odeljenje.Add(new Ucenik("Ana", 5));`: pravimo novi objekat i odmah ga dodajemo u listu.  
3. `foreach (Ucenik u in odeljenje)`: u svakom prolasku `u` je jedan učenik, pa pišemo `u.Ime` i `u.Ocena`.  
4. `Ucenik najbolji = odeljenje[0];`: pretpostavimo da je prvi najbolji, pa ga poredimo sa ostalima. Isti postupak ste radili za najveći element niza.  
5. `string.Join(", ", dobri)`: spaja sve elemente liste stringova u jedan tekst, sa `", "` između njih.  
6. `odeljenje[1].Ocena = 4;`: `odeljenje[1]` je objekat (Luka), pa mu menjamo svojstvo `Ocena` direktno u listi.

---

> ### Zapamtite\!

> 1. `List<T>` je lista koja **sama raste i smanjuje se**. Za nju treba `using System.Collections.Generic;`.  
> 2. Broj elemenata liste je `Count` (kod niza je `Length`).  
> 3. `Add` dodaje na kraj, `Insert` ubacuje na zadati indeks, `Remove` uklanja po vrednosti, a `RemoveAt` po indeksu.  
> 4. Posle `Insert` i `RemoveAt` **indeksi ostalih elemenata se menjaju**.  
> 5. `IndexOf` vraća `-1` kada elementa nema, a `Remove` tada vraća `false`. Program se ne prekida.

---

> ### Česte greške

> **1\. Indeks van opsega: ArgumentOutOfRangeException**  
>   
> Lista ima elemente na indeksima od `0` do `Count - 1`. Pristup bilo kom drugom indeksu prekida program.  
>   
> List\<int\> ocene \= new List\<int\> { 5, 4, 3 };  
>   
> Console.WriteLine(ocene\[3\]);                  // GREŠKA: indeksi su 0, 1 i 2  
>   
> for (int i \= 0; i \<= ocene.Count; i++)        // GREŠKA: \<= ide jedan korak predaleko  
>   
> {  
>   
>     Console.WriteLine(ocene\[i\]);  
>   
> }  
>   
> List\<int\> prazna \= new List\<int\>();  
>   
> prazna\[0\] \= 5;                                // GREŠKA: prazna lista nema indeks 0  
>   
> Poruka: *ArgumentOutOfRangeException: Index was out of range. Must be non-negative and less than the size of the collection.*  
>   
> **Ispravka:**  
>   
> for (int i \= 0; i \< ocene.Count; i++)         // \< umesto \<=  
>   
> {  
>   
>     Console.WriteLine(ocene\[i\]);  
>   
> }  
>   
> prazna.Add(5);                                // u praznu listu dodajemo sa Add  
>   
> int indeks \= 3;  
>   
> if (indeks \>= 0 && indeks \< ocene.Count)      // proveri indeks pre pristupa  
>   
> {  
>   
>     Console.WriteLine(ocene\[indeks\]);  
>   
> }  
>   
> **2\. Menjanje liste unutar foreach petlje: InvalidOperationException**  
>   
> Dok `foreach` prolazi kroz listu, lista **ne sme** da se menja (dodavanje ili uklanjanje).  
>   
> foreach (Ucenik u in odeljenje)  
>   
> {  
>   
>     if (u.Ocena \== 1\)  
>   
>     {  
>   
>         odeljenje.Remove(u);                  // GREŠKA  
>   
>     }  
>   
> }  
>   
> Poruka: *InvalidOperationException: Collection was modified; enumeration operation may not execute.*  
>   
> **Ispravka:** koristi `for` petlju koja ide **unazad**, od poslednjeg ka prvom elementu:  
>   
> for (int i \= odeljenje.Count \- 1; i \>= 0; i--)  
>   
> {  
>   
>     if (odeljenje\[i\].Ocena \== 1\)  
>   
>     {  
>   
>         odeljenje.RemoveAt(i);  
>   
>     }  
>   
> }  
>   
> Zašto unazad? Kada uklonimo element, svi elementi **iza** njega pomere se ulevo. Ako idemo unazad, ti elementi su već provereni, pa nijedan ne preskočimo.  
>   
> **3\. Length umesto Count**  
>   
> for (int i \= 0; i \< ocene.Length; i++)        // GREŠKA pri kompajliranju  
>   
> Poruka: *'System.Collections.Generic.List\<int\>' does not contain a definition for 'Length'*. Ispravno: `ocene.Count`.

---

## Vežbe na času

### Deo A

**Zadatak 1 (osnovni).** Napravi program koji sa tastature učitava 5 ocena (`int.Parse(Console.ReadLine())`) i dodaje ih u `List<int>`. Zatim ispiši sve ocene u jednom redu i broj ocena.

**Zadatak 2 (srednji).** Napravi `List<string>` sa spiskom za kupovinu od 4 stavke. Ubaci „hleb“ na početak liste, ukloni treću stavku (po indeksu) i dodaj „mleko“ na kraj. Ispiši spisak pre i posle izmena, sa rednim brojevima **od 1** (1. hleb, 2\. …).

**Zadatak 3 (napredni).** Učitavaj cele brojeve sa tastature dok korisnik ne unese `0` (nula se ne dodaje u listu). Zatim ispiši:

- sve unete brojeve,  
- najveći uneti broj (napiši petlju, bez gotove metode),  
- **novu** listu koja sadrži samo parne brojeve iz prve liste.

### Deo B

**Zadatak 4 (osnovni).** Napravi listu od 6 imena učenika iz tvog odeljenja. Korisnik unosi jedno ime, a program ispisuje da li je to ime u listi (`Contains`) i na kom je indeksu (`IndexOf`). Na kraju sortiraj listu i ispiši je po abecedi.

**Zadatak 5 (srednji).** Napravi klasu `Proizvod` sa svojstvima `Naziv` (string) i `Cena` (int, cena u dinarima) i konstruktorom. U `Main` napravi `List<Proizvod>` sa 5 proizvoda iz prodavnice. Ispiši sve proizvode, najjeftiniji proizvod i ukupnu cenu svih proizvoda.

**Zadatak 6 (napredni).** Napravi listu ocena `{ 5, 1, 3, 1, 4, 1, 2 }`. Ukloni **sve** jedinice `for` petljom koja ide unazad i ispiši koliko je jedinica uklonjeno i kako lista izgleda posle toga. Zatim probaj isto da uradiš `foreach` petljom i zapiši poruku greške koju dobiješ.

---

## Pitanja za diskusiju

1. Kada biste izabrali niz, a kada listu? Navedite po jedan primer iz života (dnevnik, igrica, prodavnica).  
2. Posle `RemoveAt(0)` svi elementi menjaju indeks. Šta to znači ako uklanjamo elemente u `for` petlji koja ide **unapred**?  
3. Zašto C\# ne dozvoljava menjanje liste unutar `foreach` petlje? Šta bi moglo da se desi da dozvoljava?