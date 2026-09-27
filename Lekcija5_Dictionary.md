# Lekcija 5: Dictionary\<TKey, TValue\>

**Programiranje, 4\. razred · Nastavna celina: Generički tipovi u C\#**

Lekcija se radi na dva časa: **deo A** (osnove rečnika) na 2\. susretu i **deo B** (prolazak kroz rečnik, rečnik sa listama, izbor kolekcije) na 3\. susretu.

## Cilj lekcije

Posle ove lekcije znaćeš:

- šta su **ključ** i **vrednost** i kako da dodaješ, čitaš, menjaš i brišeš elemente rečnika `Dictionary<TKey, TValue>` bez pucanja programa,  
- da prođeš kroz ceo rečnik petljom `foreach` i da napraviš rečnik čije su vrednosti liste (`Dictionary<string, List<int>>`),  
- kada da koristiš rečnik, a kada listu.

---

## Mali rečnik pojmova (nastavak)

| Srpski | Engleski | Primer |
| :---- | :---- | :---- |
| rečnik | dictionary | `Dictionary<string, string>` |
| ključ | key | `"jabuka"` |
| vrednost | value | `"apple"` |
| par ključ–vrednost | key-value pair | `"jabuka"` → `"apple"` |
| izlazni parametar | out parameter | `out prevod` |
| par ključ–vrednost (tip) | KeyValuePair | `KeyValuePair<string, int>` |

---

## Podsetimo se

- **Lista** `List<T>` čuva elemente redom, a do elementa dolazimo **indeksom**: `ocene[2]`.  
- Da bismo u listi našli neku vrednost, koristimo `Contains` ili `IndexOf`, a oni prolaze kroz listu element po element.  
- **Klasa sa dva parametra tipa** piše se `Par<T1, T2>`, na primer `Par<string, int>` za ime i ocenu.  
- **Parametar `ref`** omogućava metodi da promeni promenljivu iz `Main`. Piše se i u deklaraciji i u pozivu.

---

## Objašnjenje

### Šta je rečnik?

Zamislimo srpsko-engleski rečnik sa 10 000 reči u listi parova. Da bismo našli prevod reči „jabuka“, morali bismo da pregledamo listu od početka dok je ne nađemo.

**Rečnik (dictionary)** je kolekcija **parova ključ–vrednost (key-value pairs)**. Do vrednosti ne dolazimo indeksom (0, 1, 2…), nego **ključem (key)**: `recnik["jabuka"]` odmah daje `"apple"`. Rečnik je napravljen tako da pronalaženje po ključu bude brzo i kada ima mnogo elemenata.

`Dictionary<TKey, TValue>` ima **dva parametra tipa**, kao naš `Par<T1, T2>`:

- `TKey` je tip **ključa** (na primer `string` za reč ili ime),  
- `TValue` je tip **vrednosti** (na primer `string` za prevod ili `int` za ocenu).

Najvažnije pravilo: **ključevi su jedinstveni**. Jedan ključ može da se pojavi samo jednom. Vrednosti mogu da se ponavljaju: i Ana i Luka mogu da imaju ocenu 5, ali ne mogu postojati dva ključa „Ana“.

I `Dictionary` se nalazi u `System.Collections.Generic`, kao i `List`.

### Analogija iz svakodnevnog života

**Garderoba u pozorištu:** ostaviš jaknu i dobiješ **broj** (ključ). Kad se vratiš i daš broj, dobiješ baš **svoju jaknu** (vrednost), bez pretraživanja svih jakni.

- Dva ista broja ne postoje (ključevi su jedinstveni).  
- Mogu postojati dve iste jakne (vrednosti se mogu ponavljati).  
- Ako daš broj koji ne postoji, garderoberka nema šta da ti vrati. U C\#-u je to greška `KeyNotFoundException`.

### Deo B: prolazak kroz ceo rečnik

Kada želimo da ispišemo **sve** parove, koristimo `foreach`. Svaki element rečnika je jedan par tipa **`KeyValuePair<TKey, TValue>`**, koji ima dva svojstva: `Key` (ključ) i `Value` (vrednost).

Rečnik ima i dva korisna svojstva:

- `Keys`: svi ključevi (na primer, svi nazivi proizvoda),  
- `Values`: sve vrednosti (na primer, sve cene).

Redosled kojim `foreach` prolazi kroz rečnik **nije zagarantovan**. Kada samo dodajemo parove, obično je isti kao redosled dodavanja, ali na to ne treba računati. Ako nam treba određeni redosled, prepišemo ključeve u listu i sortiramo je.

Dok `foreach` prolazi kroz rečnik, rečnik **ne sme da se menja**: ni dodavanje, ni brisanje, ni izmena vrednosti. Isto pravilo važi i za listu (Lekcija 4).

### Deo B: rečnik čije su vrednosti liste

Vrednost u rečniku može da bude **bilo kog tipa**, pa i lista. `Dictionary<string, List<int>>` je rečnik u kom je ključ tekst (ime učenika ili naziv predmeta), a vrednost **cela lista** celih brojeva (ocene). Tako jednom ključu pridružujemo više vrednosti. To je osnova elektronskog dnevnika.

Pre nego što u listu dodamo ocenu, lista mora da **postoji**: kada se ključ pojavi prvi put, dodajemo ga sa praznom listom `new List<int>()`.

### Deo B: kada rečnik, a kada lista?

| Pitanje | Lista `List<T>` | Rečnik `Dictionary<TKey, TValue>` |
| :---- | :---- | :---- |
| Kako dolazim do elementa? | po rednom broju (indeksu) | po ključu (imenu, nazivu, šifri) |
| Da li je bitan redosled? | da, redosled je sačuvan | ne, redosled nije zagarantovan |
| Da li se vrednosti ponavljaju? | mogu | ključevi ne mogu, vrednosti mogu |
| Primeri | ocene iz jednog predmeta, red čekanja, inventar | imenik, cenovnik, prevodi, broj glasova |

**Pravilo:** ako često pitaš „koja je vrednost za **ovo ime**?“, izaberi rečnik. Ako pitaš „koji je **sledeći** ili **peti** element?“, izaberi listu.

---

## Sintaksa

using System.Collections.Generic;

// Prazan rečnik: ključ je string, vrednost je int

Dictionary\<string, int\> ocene \= new Dictionary\<string, int\>();

// Rečnik sa početnim parovima (inicijalizator kolekcije)

Dictionary\<string, string\> recnik \= new Dictionary\<string, string\>

{

    { "jabuka", "apple" },

    { "pas", "dog" }

};

ocene.Add("Ana", 5);            // dodaje NOV par; greška ako ključ već postoji

int x \= ocene\["Ana"\];           // čitanje po ključu; greška ako ključ ne postoji

ocene\["Ana"\] \= 4;               // izmena postojećeg ili dodavanje novog para

bool ima \= ocene.ContainsKey("Ana");   // da li ključ postoji

bool obrisan \= ocene.Remove("Ana");    // briše par; vraća false ako ga nema

int broj \= ocene.Count;                // broj parova

int vrednost;

if (ocene.TryGetValue("Luka", out vrednost))   // bezbedno čitanje

{

    // ključ postoji, vrednost je upisana u promenljivu vrednost

}

// Deo B: prolazak kroz sve parove

foreach (KeyValuePair\<string, int\> par in ocene)

{

    Console.WriteLine(par.Key \+ ": " \+ par.Value);

}

foreach (string ime in ocene.Keys) { /\* samo ključevi \*/ }

foreach (int ocena in ocene.Values) { /\* samo vrednosti \*/ }

// Deo B: rečnik čije su vrednosti liste

Dictionary\<string, List\<int\>\> dnevnik \= new Dictionary\<string, List\<int\>\>();

dnevnik.Add("Ana", new List\<int\>());   // prvo prazna lista

dnevnik\["Ana"\].Add(5);                 // pa dodajemo u listu

---

## Primeri: deo A

### Primer 1: Srpsko-engleski rečnik

using System;

using System.Collections.Generic;

namespace Lekcija5Primer1

{

    class Program

    {

        static void Main(string\[\] args)

        {

            // Ključ je srpska reč, vrednost je engleski prevod

            Dictionary\<string, string\> recnik \= new Dictionary\<string, string\>();

            recnik.Add("jabuka", "apple");

            recnik.Add("knjiga", "book");

            recnik.Add("voda", "watter");          // greška u kucanju, ispravićemo je

            Console.WriteLine("Broj pojmova: " \+ recnik.Count);

            Console.WriteLine("jabuka \= " \+ recnik\["jabuka"\]);

            recnik\["voda"\] \= "water";              // ključ postoji: menja se vrednost

            recnik\["pas"\] \= "dog";                 // ključ ne postoji: dodaje se nov par

            Console.WriteLine("voda \= " \+ recnik\["voda"\]);

            Console.WriteLine("Broj pojmova: " \+ recnik.Count);

            string rec \= "prozor";

            if (recnik.ContainsKey(rec))

            {

                Console.WriteLine(rec \+ " \= " \+ recnik\[rec\]);

            }

            else

            {

                Console.WriteLine(rec \+ ": nepoznat pojam");

            }

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Broj pojmova: 3

jabuka \= apple

voda \= water

Broj pojmova: 4

prozor: nepoznat pojam

**Objašnjenje:**

1. `Dictionary<string, string> recnik`: i ključ i vrednost su tipa `string`.  
2. `recnik.Add("jabuka", "apple");`: `Add` prima **dva** argumenta, ključ pa vrednost.  
3. `recnik["jabuka"]`: u uglaste zagrade ne pišemo indeks, nego **ključ**. Rezultat je vrednost `"apple"`.  
4. `recnik["voda"] = "water";`: ključ „voda“ već postoji, pa se njegova vrednost **menja**. Broj parova ostaje isti.  
5. `recnik["pas"] = "dog";`: ključ „pas“ ne postoji, pa se par **dodaje**. Zato je broj pojmova sada 4\.  
6. `recnik.ContainsKey(rec)`: pre čitanja proveravamo da li ključ postoji. Bez te provere, `recnik["prozor"]` bi prekinuo program.

### Primer 2: Telefonski imenik (TryGetValue i Remove)

using System;

using System.Collections.Generic;

namespace Lekcija5Primer2

{

    class Program

    {

        static void Main(string\[\] args)

        {

            // Ključ je ime, vrednost je broj telefona

            Dictionary\<string, string\> imenik \= new Dictionary\<string, string\>

            {

                { "Ana", "064/111-222" },

                { "Luka", "063/333-444" },

                { "Milica", "065/555-666" }

            };

            string broj;   // ovde će TryGetValue upisati pronađeni broj

            if (imenik.TryGetValue("Luka", out broj))

            {

                Console.WriteLine("Luka: " \+ broj);

            }

            if (imenik.TryGetValue("Marko", out broj))

            {

                Console.WriteLine("Marko: " \+ broj);

            }

            else

            {

                Console.WriteLine("Marko nije u imeniku.");

            }

            bool obrisan \= imenik.Remove("Ana");

            Console.WriteLine("Ana obrisana? " \+ obrisan);

            Console.WriteLine("Ponovo brisanje Ane: " \+ imenik.Remove("Ana"));

            Console.WriteLine("Kontakata: " \+ imenik.Count);

            imenik\["Milica"\] \= "066/777-888";    // Milica je promenila broj

            Console.WriteLine("Milica: " \+ imenik\["Milica"\]);

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Luka: 063/333-444

Marko nije u imeniku.

Ana obrisana? True

Ponovo brisanje Ane: False

Kontakata: 2

Milica: 066/777-888

**Objašnjenje:**

1. `{ "Ana", "064/111-222" }`: u inicijalizatoru kolekcije svaki par pišemo u vitičastim zagradama, prvo ključ, pa vrednost.  
2. `string broj;`: promenljivu za rezultat deklarišemo **pre** poziva `TryGetValue`.  
3. `imenik.TryGetValue("Luka", out broj)`: metoda radi dve stvari. Vraća `true` ili `false` (da li ključ postoji), a ako postoji, upisuje vrednost u promenljivu `broj`. Reč **`out`** označava **izlazni parametar (out parameter)**: slično kao `ref`, ali služi da metoda **vrati** vrednost kroz parametar. Piše se i u pozivu.  
4. Za „Marko“ metoda vraća `false`, pa se izvršava `else`. Program ne puca.  
5. `imenik.Remove("Ana")`: prvi put vraća `true` (par je obrisan), a drugi put `false` (Ane više nema). Ni tada program ne puca.  
6. `imenik["Milica"] = "066/777-888";`: menjamo vrednost postojećeg ključa.

### Primer 3: Inventar u igrici sa količinama

using System;

using System.Collections.Generic;

namespace Lekcija5Primer3

{

    class Program

    {

        // Dodaje predmete u inventar: ako predmet postoji, povećava količinu

        static void Pokupi(Dictionary\<string, int\> inventar, string predmet, int kolicina)

        {

            if (inventar.ContainsKey(predmet))

            {

                inventar\[predmet\] \= inventar\[predmet\] \+ kolicina;

            }

            else

            {

                inventar.Add(predmet, kolicina);

            }

        }

        // Troši jedan predmet; kada se potroši poslednji, predmet se briše iz inventara

        static void Iskoristi(Dictionary\<string, int\> inventar, string predmet)

        {

            int kolicina;

            if (inventar.TryGetValue(predmet, out kolicina))

            {

                if (kolicina \== 1\)

                {

                    inventar.Remove(predmet);

                }

                else

                {

                    inventar\[predmet\] \= kolicina \- 1;

                }

                Console.WriteLine("Upotrebljen: " \+ predmet);

            }

            else

            {

                Console.WriteLine("Nema predmeta: " \+ predmet);

            }

        }

        // Ispisuje količinu predmeta (0 ako ga nema)

        static void Stanje(Dictionary\<string, int\> inventar, string predmet)

        {

            int kolicina;

            if (inventar.TryGetValue(predmet, out kolicina))

            {

                Console.WriteLine(predmet \+ ": " \+ kolicina);

            }

            else

            {

                Console.WriteLine(predmet \+ ": 0");

            }

        }

        static void Main(string\[\] args)

        {

            // Ključ je naziv predmeta, vrednost je količina

            Dictionary\<string, int\> inventar \= new Dictionary\<string, int\>();

            Pokupi(inventar, "napitak", 2);

            Pokupi(inventar, "strela", 10);

            Pokupi(inventar, "baklja", 1);

            Pokupi(inventar, "napitak", 1);

            Console.WriteLine("Vrsta predmeta: " \+ inventar.Count);

            Stanje(inventar, "napitak");

            Iskoristi(inventar, "napitak");

            Iskoristi(inventar, "baklja");

            Iskoristi(inventar, "mapa");

            Stanje(inventar, "napitak");

            Stanje(inventar, "baklja");

            Console.WriteLine("Vrsta predmeta: " \+ inventar.Count);

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Vrsta predmeta: 3

napitak: 3

Upotrebljen: napitak

Upotrebljen: baklja

Nema predmeta: mapa

napitak: 2

baklja: 0

Vrsta predmeta: 2

**Objašnjenje:**

1. `Dictionary<string, int> inventar`: ključ je naziv predmeta, a vrednost je broj komada.  
2. U metodi `Pokupi`: ako predmet već postoji, **uvećavamo** vrednost (`inventar[predmet] + kolicina`), a ako ne postoji, dodajemo nov par. Zato drugi „napitak“ nije napravio grešku, nego je povećao količinu na 3\.  
3. `Vrsta predmeta: 3`: `Count` broji **parove** (napitak, strela, baklja), a ne ukupan broj komada.  
4. U metodi `Iskoristi`: `TryGetValue` istovremeno proverava da li predmet postoji i čita količinu. Kada je količina 1, predmet se briše sa `Remove`.  
5. `Iskoristi(inventar, "mapa")`: mape nema, pa `TryGetValue` vraća `false` i ispisuje se poruka. Program ne puca.  
6. Rečnik se prosleđuje metodi kao i lista: metoda menja **isti** rečnik koji je napravljen u `Main`.

---

## Primeri: deo B

### Primer 4: Cenovnik (foreach, Keys, Values)

using System;

using System.Collections.Generic;

namespace Lekcija5Primer4

{

    class Program

    {

        static void Main(string\[\] args)

        {

            // Ključ je naziv proizvoda, vrednost je cena u dinarima

            Dictionary\<string, int\> cenovnik \= new Dictionary\<string, int\>

            {

                { "hleb", 70 },

                { "mleko", 120 },

                { "jaja", 250 },

                { "sok", 180 }

            };

            Console.WriteLine("Cenovnik:");

            foreach (KeyValuePair\<string, int\> par in cenovnik)

            {

                Console.WriteLine("  " \+ par.Key \+ ": " \+ par.Value \+ " din");

            }

            Console.Write("Proizvodi:");

            foreach (string naziv in cenovnik.Keys)

            {

                Console.Write(" " \+ naziv);

            }

            Console.WriteLine();

            int ukupno \= 0;

            foreach (int cena in cenovnik.Values)

            {

                ukupno \= ukupno \+ cena;

            }

            Console.WriteLine("Sve po jedan komad: " \+ ukupno \+ " din");

            cenovnik\["mleko"\] \= 130;   // poskupljenje (van foreach petlje\!)

            Console.WriteLine("Mleko sada: " \+ cenovnik\["mleko"\] \+ " din");

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Cenovnik:

  hleb: 70 din

  mleko: 120 din

  jaja: 250 din

  sok: 180 din

Proizvodi: hleb mleko jaja sok

Sve po jedan komad: 620 din

Mleko sada: 130 din

**Objašnjenje:**

1. `foreach (KeyValuePair<string, int> par in cenovnik)`: svaki element rečnika je **par**. Tip para ima iste argumente tipa kao rečnik: `<string, int>`.  
2. `par.Key` i `par.Value`: ključ (naziv) i vrednost (cena) trenutnog para.  
3. `cenovnik.Keys`: prolazimo samo kroz ključeve, pa je promenljiva tipa `string`.  
4. `cenovnik.Values`: prolazimo samo kroz vrednosti, pa je promenljiva tipa `int`. Tako lako saberemo sve cene.  
5. `cenovnik["mleko"] = 130;`: cenu menjamo **posle** petlje. Unutar `foreach` petlje kroz `cenovnik` to ne bi bilo dozvoljeno (vidi Česte greške).  
6. Umesto dugačkog `KeyValuePair<string, int>` možemo da napišemo i `foreach (var par in cenovnik)`. Kompajler sam zaključuje tip.

### Primer 5: Raspodela ocena (uvećavanje vrednosti)

using System;

using System.Collections.Generic;

namespace Lekcija5Primer5

{

    class Program

    {

        static void Main(string\[\] args)

        {

            // Ocene sa kontrolnog zadatka

            int\[\] ocene \= { 5, 4, 5, 3, 5, 2, 4, 5 };

            // Ključ je ocena, vrednost je koliko puta se ta ocena pojavila

            Dictionary\<int, int\> brojOcena \= new Dictionary\<int, int\>();

            foreach (int ocena in ocene)

            {

                if (brojOcena.ContainsKey(ocena))

                {

                    brojOcena\[ocena\] \= brojOcena\[ocena\] \+ 1;   // već postoji: uvećaj

                }

                else

                {

                    brojOcena.Add(ocena, 1);                   // prvi put: počni od 1

                }

            }

            Console.WriteLine("Raspodela ocena:");

            for (int o \= 5; o \>= 1; o--)     // ispis od petice do jedinice

            {

                int broj;

                if (brojOcena.TryGetValue(o, out broj))

                {

                    Console.WriteLine(o \+ ": " \+ broj);

                }

                else

                {

                    Console.WriteLine(o \+ ": 0");

                }

            }

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Raspodela ocena:

5: 4

4: 2

3: 1

2: 1

1: 0

**Objašnjenje:**

1. `Dictionary<int, int>`: ključ ne mora da bude `string`. Ovde je ključ ocena, a vrednost broj pojavljivanja.  
2. `if (brojOcena.ContainsKey(ocena))`: ovo je **obrazac za brojanje**. Ako ključ već postoji, uvećavamo vrednost. Ako ne postoji, dodajemo ga sa vrednošću 1\.  
3. `brojOcena[ocena] = brojOcena[ocena] + 1;`: izmena vrednosti **postojećeg** ključa. Čitamo staru vrednost, dodamo 1 i upisujemo novu.  
4. `for (int o = 5; o >= 1; o--)`: redosled u rečniku nije zagarantovan, pa za uredan ispis prolazimo kroz sve moguće ocene redom.  
5. `TryGetValue(o, out broj)`: jedinica se nije pojavila, pa je nema u rečniku. `TryGetValue` vraća `false` i ispisujemo 0, bez pucanja programa.

### Primer 6: Ocene po predmetima (rečnik čije su vrednosti liste)

using System;

using System.Collections.Generic;

namespace Lekcija5Primer6

{

    class Program

    {

        // Dodaje ocenu iz predmeta; ako se predmet pojavljuje prvi put, pravi mu praznu listu

        static void DodajOcenu(Dictionary\<string, List\<int\>\> dnevnik, string predmet, int ocena)

        {

            if (\!dnevnik.ContainsKey(predmet))

            {

                dnevnik.Add(predmet, new List\<int\>());

            }

            dnevnik\[predmet\].Add(ocena);   // dnevnik\[predmet\] je List\<int\>

        }

        static void Main(string\[\] args)

        {

            // Ključ je naziv predmeta, vrednost je lista ocena iz tog predmeta

            Dictionary\<string, List\<int\>\> ocene \= new Dictionary\<string, List\<int\>\>();

            DodajOcenu(ocene, "Programiranje", 5);

            DodajOcenu(ocene, "Matematika", 3);

            DodajOcenu(ocene, "Programiranje", 4);

            DodajOcenu(ocene, "Fizika", 5);

            DodajOcenu(ocene, "Programiranje", 5);

            DodajOcenu(ocene, "Matematika", 4);

            foreach (KeyValuePair\<string, List\<int\>\> par in ocene)

            {

                int zbir \= 0;

                foreach (int o in par.Value)

                {

                    zbir \= zbir \+ o;

                }

                double prosek \= (double)zbir / par.Value.Count;

                Console.WriteLine(par.Key \+ ": " \+ string.Join(" ", par.Value)

                    \+ " (prosek " \+ prosek.ToString("0.00") \+ ")");

            }

            Console.ReadKey();

        }

    }

}

**Izlaz u konzoli:**

Programiranje: 5 4 5 (prosek 4,67)

Matematika: 3 4 (prosek 3,50)

Fizika: 5 (prosek 5,00)

*Napomena:* na računaru sa srpskim regionalnim podešavanjima decimalni separator je zarez (4,67). Sa engleskim podešavanjima ispisuje se tačka (4.67). Oba ispisa su ispravna.

**Objašnjenje:**

1. `Dictionary<string, List<int>>`: vrednost je **cela lista**. Jednom predmetu pridružujemo više ocena.  
2. `if (!dnevnik.ContainsKey(predmet)) dnevnik.Add(predmet, new List<int>());`: kada se predmet pojavi prvi put, pravimo mu **praznu listu**. Bez toga ne bi imalo gde da se doda ocena.  
3. `dnevnik[predmet].Add(ocena);`: `dnevnik[predmet]` vraća listu, a na listi pozivamo `Add`. Ovo su dve operacije u jednom redu: prvo rečnik, pa lista.  
4. `par.Value`: u ovom rečniku vrednost je `List<int>`, pa kroz nju prolazimo unutrašnjom `foreach` petljom i imamo `par.Value.Count`.  
5. `(double)zbir / par.Value.Count`: pre deljenja pretvaramo zbir u `double`. Bez toga bi deljenje dva cela broja dalo ceo broj (14 / 3 \= 4\) i prosek bi bio pogrešan.  
6. `prosek.ToString("0.00")`: ispisuje broj sa tačno dve decimale. `string.Join(" ", par.Value)` spaja ocene u jedan tekst, kao u Lekciji 4\.

---

> ### Zapamtite\!

> 1. Rečnik čuva parove **ključ → vrednost**. Do vrednosti dolazimo **ključem**, a ne indeksom. **Ključevi su jedinstveni**.  
> 2. `Add` dodaje samo **nov** par (inače puca), a `recnik[kljuc] = vrednost` **menja** postojeću vrednost ili **dodaje** nov par.  
> 3. Pre čitanja proveri ključ: `ContainsKey` ili, još bolje, `TryGetValue`.  
> 4. Kroz ceo rečnik prolazimo sa `foreach (KeyValuePair<TKey, TValue> par in recnik)`. Unutar te petlje rečnik **ne sme da se menja**.  
> 5. U `Dictionary<string, List<int>>` lista mora prvo da se napravi (`new List<int>()`), pa tek onda u nju dodajemo.

---

> ### Česte greške

> **1\. Čitanje ključa koji ne postoji: KeyNotFoundException**  
>   
> Dictionary\<string, string\> recnik \= new Dictionary\<string, string\>();  
>   
> recnik.Add("jabuka", "apple");  
>   
> Console.WriteLine(recnik\["macka"\]);            // GREŠKA: ključ ne postoji  
>   
> Poruka: *KeyNotFoundException: The given key was not present in the dictionary.*  
>   
> **Ispravka:**  
>   
> string prevod;  
>   
> if (recnik.TryGetValue("macka", out prevod))  
>   
> {  
>   
>     Console.WriteLine(prevod);  
>   
> }  
>   
> else  
>   
> {  
>   
>     Console.WriteLine("Nema prevoda.");  
>   
> }  
>   
> **2\. Dodavanje ključa koji već postoji: ArgumentException**  
>   
> recnik.Add("jabuka", "apple");  
>   
> recnik.Add("jabuka", "an apple");              // GREŠKA: ključ već postoji  
>   
> Poruka: *ArgumentException: An item with the same key has already been added.*  
>   
> **Ispravka:** proveri ključ pre dodavanja, ili upiši vrednost preko uglastih zagrada ako želiš da je zameniš:  
>   
> if (\!recnik.ContainsKey("jabuka"))  
>   
> {  
>   
>     recnik.Add("jabuka", "apple");  
>   
> }  
>   
> recnik\["jabuka"\] \= "an apple";                 // menja vrednost, bez greške  
>   
> **3\. Velika i mala slova u ključu**  
>   
> recnik.Add("jabuka", "apple");  
>   
> Console.WriteLine(recnik\["Jabuka"\]);           // GREŠKA: "Jabuka" nije isto što i "jabuka"  
>   
> Ključevi tipa `string` razlikuju velika i mala slova. Ovo se najčešće desi kada korisnik unosi reč sa tastature. Ispravka: pre pretrage pretvori unos u mala slova, na primer `string rec = Console.ReadLine().ToLower();`.  
>   
> **4\. Zaboravljeno out u pozivu TryGetValue**  
>   
> recnik.TryGetValue("jabuka", prevod);          // GREŠKA pri kompajliranju  
>   
> Poruka: *Argument 2 must be passed with the 'out' keyword*. Ispravno: `recnik.TryGetValue("jabuka", out prevod)`.  
>   
> **5\. Menjanje rečnika unutar foreach petlje: InvalidOperationException (deo B)**  
>   
> // Hoćemo da poskupimo sve proizvode za 10 dinara  
>   
> foreach (KeyValuePair\<string, int\> par in cenovnik)  
>   
> {  
>   
>     cenovnik\[par.Key\] \= par.Value \+ 10;         // GREŠKA: rečnik se menja dok ga foreach čita  
>   
> }  
>   
> // Hoćemo da uklonimo jeftine proizvode  
>   
> foreach (string naziv in cenovnik.Keys)  
>   
> {  
>   
>     if (cenovnik\[naziv\] \< 100\)  
>   
>     {  
>   
>         cenovnik.Remove(naziv);                 // GREŠKA  
>   
>     }  
>   
> }  
>   
> Poruka: *InvalidOperationException: Collection was modified; enumeration operation may not execute.*  
>   
> **Ispravka:** prvo prepiši ključeve u **novu listu**, pa prolazi kroz tu listu. Tada slobodno menjaš rečnik:  
>   
> List\<string\> nazivi \= new List\<string\>(cenovnik.Keys);   // kopija ključeva  
>   
> foreach (string naziv in nazivi)  
>   
> {  
>   
>     cenovnik\[naziv\] \= cenovnik\[naziv\] \+ 10;     // sada je dozvoljeno  
>   
> }  
>   
> foreach (string naziv in nazivi)  
>   
> {  
>   
>     if (cenovnik\[naziv\] \< 100\)  
>   
>     {  
>   
>         cenovnik.Remove(naziv);                 // i ovo je dozvoljeno  
>   
>     }  
>   
> }  
>   
> **6\. Ocena u listu koja ne postoji (deo B)**  
>   
> Dictionary\<string, List\<int\>\> dnevnik \= new Dictionary\<string, List\<int\>\>();  
>   
> dnevnik\["Ana"\].Add(5);                          // GREŠKA: ključ "Ana" još ne postoji  
>   
> Poruka: *KeyNotFoundException*. Ispravka: prvo `dnevnik.Add("Ana", new List<int>());`, pa tek onda `dnevnik["Ana"].Add(5);` (vidi metodu `DodajOcenu` u Primeru 6).  
>   
> **7\. Deljenje celih brojeva pri računanju proseka (deo B)**  
>   
> double prosek \= zbir / ocene.Count;              // GREŠKA u logici: 14 / 3 daje 4  
>   
> Program se kompajlira i radi, ali je prosek pogrešan, jer se prvo dele dva cela broja. Ispravno: `double prosek = (double)zbir / ocene.Count;` (daje 4,67). Pazi i na praznu listu: ako je `Count` 0, prosek nema smisla, pa to proveri pre deljenja.

---

## Vežbe na času

### Deo A

**Zadatak 1 (osnovni).** Napravi rečnik `Dictionary<string, int>` u kom je ključ naziv predmeta, a vrednost ocena (4 predmeta). Ispiši ocenu iz Programiranja, promeni ocenu iz Fizike i proveri da li u rečniku postoji predmet „Hemija“. Za svaku akciju ispiši poruku.

**Zadatak 2 (srednji).** Napravi mali srpsko-engleski rečnik sa 5 reči. Korisnik unosi reč, a program ispisuje prevod ili poruku da reči nema (koristi `TryGetValue`). Program ponavlja unos sve dok korisnik ne unese „kraj“. Unos pretvori u mala slova sa `ToLower()`.

**Zadatak 3 (napredni).** **Brojanje glasova:** korisnik unosi imena kandidata za predsednika odeljenja, jedno po jedno, dok ne unese „kraj“. Program vodi rečnik `ime → broj glasova` (novo ime se dodaje sa 1 glasom, a postojećem se broj uvećava). Na kraju ispiši broj glasova za tri kandidata čija imena zadaš u kodu (0 ako kandidat nije dobio nijedan glas) i ukupan broj različitih kandidata.

### Deo B

**Zadatak 4 (osnovni).** Napravi telefonski imenik (`Dictionary<string, string>`) sa 5 kontakata. Ispiši sve kontakte petljom `foreach` sa `KeyValuePair`, u obliku `Ime: broj`. Zatim, u posebnom redu, ispiši samo imena (`Keys`).

**Zadatak 5 (srednji).** Korisnik unosi jednu rečenicu. Podeli je na reči sa `recenica.ToLower().Split(' ')` i napravi rečnik `reč → broj pojavljivanja`. Ispiši svaku reč i koliko puta se pojavila, a zatim reč koja se pojavila najviše puta.

**Zadatak 6 (napredni).** Napravi rečnik `Dictionary<string, List<string>>` u kom je ključ oznaka odeljenja („IV-1“, „IV-2“), a vrednost lista imena učenika. Napiši metode:

- `DodajUcenika(rečnik, odeljenje, ime)` koja pravi praznu listu ako odeljenje ne postoji,  
- `Ispisi(rečnik)` koja za svako odeljenje ispisuje broj učenika i imena,  
- `Premesti(rečnik, ime, izOdeljenja, uOdeljenje)` koja premešta učenika iz jednog odeljenja u drugo i vraća `false` ako učenika nema u prvom odeljenju.

---

## Pitanja za diskusiju

1. Zašto ključevi u rečniku moraju biti jedinstveni, a vrednosti ne moraju? Šta bi bio problem da postoje dva ista ključa?  
2. Lista ili rečnik: spisak pesama na plejlisti, cene proizvoda po nazivu, rezultati trke po redosledu dolaska, broj glasova po kandidatu? Obrazložite.  
3. Šta bi bio ključ, a šta vrednost u: telefonskom imeniku, dnevniku ocena, cenovniku prodavnice? Da li bi ime učenika uvek bilo dobar ključ? Šta kad u odeljenju postoje dve Ane?