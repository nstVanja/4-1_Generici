# Lekcija 3: Generičke klase

**Programiranje, 4. razred · Nastavna celina: Generički tipovi u C#**

## Cilj lekcije

Posle ove lekcije znaćeš:

- da napišeš generičku klasu sa **konstruktorom, svojstvima i metodama**,
- da napraviš generičku klasu sa **dva parametra tipa**, `Par<T1, T2>`,
- da praviš objekte iste generičke klase sa različitim tipovima.

---

## Podsetimo se

- **Generički tip (generic type)** ima **parametar tipa (type parameter)** `T`, koji zamenjujemo **argumentom tipa (type argument)**: `Kutija<int>`, `Kutija<string>`.
- **Konstruktor** je posebna metoda koja se poziva pri `new`. Ima isto ime kao klasa i nema povratni tip. U njemu postavljamo početne vrednosti polja.
- **Svojstvo (property)** je „kontrolisan pristup“ polju, sa delovima `get` i `set`:

```csharp
private int ocena;
public int Ocena
{
    get { return ocena; }
    set { ocena = value; }
}
```

- **Auto-svojstvo** je kraći zapis kada nam ne treba posebna logika: `public int Ocena { get; set; }`.
- Ključna reč **`var`** kaže kompajleru da sam zaključi tip promenljive sa desne strane: `var u = new Ucenik();`.

---

## Objašnjenje

U prvoj lekciji smo napravili jednostavnu klasu `Kutija<T>` sa dve metode. Generička klasa može da ima sve što ima i obična klasa: **polja, konstruktore, svojstva i metode**. Svuda gde bi inače pisao konkretan tip, možeš da napišeš `T`.

Dve stvari su važne:

1. **Konstruktor se zove kao klasa, ali bez `<T>`.** Klasa je `Kutija<T>`, a konstruktor je `public Kutija(T pocetniSadrzaj)`.
2. **Klasa može da ima više parametara tipa**, razdvojenih zarezom: `class Par<T1, T2>`. Tada je `T1` tip prvog, a `T2` tip drugog podatka. Na primer, `Par<string, int>` čuva ime učenika i ocenu.

Svaki put kad napišemo novi argument tipa, dobijamo **novi tip**: `Kutija<int>` i `Kutija<string>` imaju isti kod, ali za kompajler su to dva različita tipa.

### Analogija iz svakodnevnog života

Zamisli **kalup za kolače** u obliku zvezde. Kalup je uvek isti, ali ti biraš testo: čokoladno, vanilu ili medenjake. Svaki kolač ima oblik zvezde, ali je od drugog testa.

- kalup = generička klasa `Kutija<T>` (oblik: polja, svojstva, metode)
- vrsta testa = argument tipa (`int`, `string`…)
- gotov kolač = objekat (`new Kutija<int>(5)`)

`Par<T1, T2>` je kalup sa **dve pregrade**, recimo za dve vrste nadeva, gde za svaku pregradu posebno biraš nadev.

---

## Sintaksa

```csharp
class NazivKlase<T>
{
    private T polje;

    // Konstruktor: ime klase BEZ <T>
    public NazivKlase(T pocetnaVrednost)
    {
        polje = pocetnaVrednost;
    }

    // Svojstvo tipa T
    public T Svojstvo
    {
        get { return polje; }
        set { polje = value; }
    }
}

// Klasa sa dva parametra tipa
class NazivKlase2<T1, T2>
{
    public T1 Prvi { get; set; }      // auto-svojstvo tipa T1
    public T2 Drugi { get; set; }     // auto-svojstvo tipa T2
}

// Pravljenje objekata
NazivKlase<int> a = new NazivKlase<int>(5);
var b = new NazivKlase2<string, int>();
```

---

## Primeri

### Primer 1: Kutija&lt;T&gt; sa konstruktorom i svojstvom

```csharp
using System;

namespace Lekcija3Primer1
{
    class Kutija<T>
    {
        private T sadrzaj;

        // Konstruktor – ime je Kutija, bez <T>
        public Kutija(T pocetniSadrzaj)
        {
            sadrzaj = pocetniSadrzaj;
        }

        // Svojstvo tipa T
        public T Sadrzaj
        {
            get { return sadrzaj; }
            set { sadrzaj = value; }
        }

        public void Opisi()
        {
            Console.WriteLine("U kutiji je: " + sadrzaj);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Kutija<int> ocena = new Kutija<int>(5);
            Kutija<string> predmet = new Kutija<string>("Programiranje");

            ocena.Opisi();
            predmet.Opisi();

            ocena.Sadrzaj = 4;                  // menjamo sadržaj preko svojstva
            ocena.Opisi();

            int duplo = ocena.Sadrzaj * 2;      // Sadrzaj je int, pa može da se računa
            Console.WriteLine("Duplo: " + duplo);

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
U kutiji je: 5
U kutiji je: Programiranje
U kutiji je: 4
Duplo: 8
```

**Objašnjenje:**

1. `public Kutija(T pocetniSadrzaj)`: konstruktor prima početnu vrednost tipa `T`. Njegovo ime je `Kutija`, **bez** `<T>`.
2. `public T Sadrzaj { get {...} set {...} }`: svojstvo je tipa `T`. U `Kutija<int>` vraća `int`, a u `Kutija<string>` vraća `string`.
3. `new Kutija<int>(5)`: argument tipa `int` ide u izlomljene zagrade, a početna vrednost `5` u obične zagrade, kao argument konstruktora.
4. `ocena.Sadrzaj = 4;`: zove se deo `set` svojstva. `value` je ovde `4`.
5. `ocena.Sadrzaj * 2`: kompajler zna da je `Sadrzaj` tipa `int`, pa množenje radi bez kastovanja.

### Primer 2: Par&lt;T1, T2&gt;, klasa sa dva parametra tipa

```csharp
using System;

namespace Lekcija3Primer2
{
    // Čuva dva podatka koji mogu biti različitih tipova
    class Par<T1, T2>
    {
        public T1 Prvi { get; set; }
        public T2 Drugi { get; set; }

        public Par(T1 prvi, T2 drugi)
        {
            Prvi = prvi;
            Drugi = drugi;
        }

        public void Ispisi()
        {
            Console.WriteLine("(" + Prvi + ", " + Drugi + ")");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // učenik i ocena
            Par<string, int> ocena = new Par<string, int>("Luka", 5);

            // srpsko-engleski rečnik: reč i prevod
            var rec = new Par<string, string>("jabuka", "apple");

            // redni broj u dnevniku i prisustvo
            Par<int, bool> prisustvo = new Par<int, bool>(12, true);

            ocena.Ispisi();
            rec.Ispisi();
            prisustvo.Ispisi();

            Console.WriteLine(ocena.Prvi + " ima ocenu " + ocena.Drugi);
            Console.WriteLine("Engleski: " + rec.Drugi);

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
(Luka, 5)
(jabuka, apple)
(12, True)
Luka ima ocenu 5
Engleski: apple
```

**Objašnjenje:**

1. `class Par<T1, T2>`: dva parametra tipa, razdvojena zarezom. Mogu biti različiti tipovi, ali i isti (`Par<string, string>`).
2. `public T1 Prvi { get; set; }`: auto-svojstvo tipa `T1`. Za `Par<string, int>` to je `string`.
3. `new Par<string, int>("Luka", 5)`: redosled argumenata tipa mora da odgovara redosledu vrednosti u konstruktoru: prvo `string`, pa `int`.
4. `var rec = new Par<string, string>(...)`: sa `var` ne ponavljamo dugačak tip sa leve strane. Kompajler ga zaključuje sam.
5. `ocena.Prvi + " ima ocenu " + ocena.Drugi`: svojstvima pristupamo kao kod obične klase.

### Primer 3: Ranac&lt;T&gt;, inventar u igrici

```csharp
using System;

namespace Lekcija3Primer3
{
    // Ranac sa ograničenim brojem mesta; može da čuva predmete bilo kog tipa
    class Ranac<T>
    {
        private T[] predmeti;   // niz elemenata tipa T
        private int broj;       // koliko je mesta zauzeto

        public Ranac(int kapacitet)
        {
            predmeti = new T[kapacitet];
            broj = 0;
        }

        // Svojstvo samo za čitanje (ima samo get)
        public int Broj
        {
            get { return broj; }
        }

        // Dodaje predmet; vraća false ako je ranac pun
        public bool Dodaj(T predmet)
        {
            if (broj == predmeti.Length)
            {
                return false;
            }
            predmeti[broj] = predmet;
            broj++;
            return true;
        }

        public T Uzmi(int indeks)
        {
            return predmeti[indeks];
        }

        public void IspisiSve()
        {
            Console.WriteLine("Sadrzaj (" + broj + "/" + predmeti.Length + "):");
            for (int i = 0; i < broj; i++)
            {
                Console.WriteLine("  " + i + ": " + predmeti[i]);
            }
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Ranac<string> ranac = new Ranac<string>(3);
            ranac.Dodaj("sekira");
            ranac.Dodaj("kaciga");
            ranac.Dodaj("napitak");
            bool uspelo = ranac.Dodaj("mapa");      // ranac je već pun

            Console.WriteLine("Dodata mapa? " + uspelo);
            ranac.IspisiSve();
            Console.WriteLine("Na indeksu 1 je: " + ranac.Uzmi(1));

            // Isti kod, drugi tip: poeni osvojeni na nivoima
            Ranac<int> poeni = new Ranac<int>(4);
            poeni.Dodaj(120);
            poeni.Dodaj(350);
            poeni.Dodaj(90);
            poeni.IspisiSve();

            int ukupno = 0;
            for (int i = 0; i < poeni.Broj; i++)
            {
                ukupno = ukupno + poeni.Uzmi(i);
            }
            Console.WriteLine("Ukupno poena: " + ukupno);

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Dodata mapa? False
Sadrzaj (3/3):
  0: sekira
  1: kaciga
  2: napitak
Na indeksu 1 je: kaciga
Sadrzaj (3/4):
  0: 120
  1: 350
  2: 90
Ukupno poena: 560
```

**Objašnjenje:**

1. `private T[] predmeti;`: generička klasa može da ima i **niz** elemenata tipa `T`.
2. `predmeti = new T[kapacitet];`: niz tipa `T` pravimo u konstruktoru, a kapacitet (broj mesta) zadajemo pri `new`.
3. `public int Broj { get { return broj; } }`: svojstvo ima samo `get`, pa spolja može da se čita, ali ne i da se menja.
4. `if (broj == predmeti.Length) return false;`: niz ima **fiksnu dužinu**. Kada se popuni, više ništa ne može da se doda. Zato `Dodaj("mapa")` vraća `false`.
5. `ukupno = ukupno + poeni.Uzmi(i);`: pošto je `poeni` tipa `Ranac<int>`, `Uzmi` vraća `int` i sabiranje radi bez kastovanja.
6. Ista klasa `Ranac<T>` radi i za predmete (`string`) i za poene (`int`).

*Za razmišljanje:* ranac ima fiksan kapacitet. Šta ako ne znamo unapred koliko ćemo predmeta imati? Odgovor je u sledećoj lekciji: `List<T>`.

---

> ### Zapamtite!
>
> 1. Generička klasa može da ima **polja, konstruktore, svojstva i metode** tipa `T`.
> 2. Konstruktor se zove kao klasa, ali **bez** `<T>`: `public Kutija(T vrednost)`.
> 3. Više parametara tipa odvajamo zarezom: `class Par<T1, T2>`.
> 4. Pri pravljenju objekta argument tipa ide u `< >`, a vrednosti za konstruktor u `( )`: `new Kutija<int>(5)`.
> 5. `Kutija<int>` i `Kutija<string>` su **različiti tipovi**, iako su nastali iz iste klase.

---

> ### Česte greške
>
> **1. `<T>` u imenu konstruktora**
>
> ```csharp
> public Kutija<T>(T pocetniSadrzaj)    // GREŠKA
> ```
> Kompajler javlja grešku (*Method must have a return type*), jer misli da je ovo metoda bez povratnog tipa.
> Ispravno: `public Kutija(T pocetniSadrzaj)`
>
> **2. Pogrešan redosled argumenata tipa**
>
> ```csharp
> Par<int, string> p = new Par<int, string>("Luka", 5);   // GREŠKA
> ```
> Prvi podatak je `"Luka"` (string), pa i prvi argument tipa mora biti `string`.
> Ispravno: `new Par<string, int>("Luka", 5)`
>
> **3. Zaboravljena vrednost za konstruktor**
>
> ```csharp
> Kutija<int> k = new Kutija<int>();    // GREŠKA
> ```
> Kutija ima samo konstruktor koji prima vrednost, pa kompajler javlja da ne postoji konstruktor bez argumenata (*does not contain a constructor that takes 0 arguments*).
> Ispravno: `new Kutija<int>(5)`

---

## Vežbe na času

**Zadatak 1 (osnovni).** Koristeći klasu `Par<T1, T2>` iz Primera 2, napravi mali **telefonski imenik** od tri kontakta, gde je prvi podatak ime, a drugi broj telefona (oba tipa `string`, na primer `"064/123-456"`). Ispiši sve kontakte metodom `Ispisi`, a zatim samo broj telefona drugog kontakta.

**Zadatak 2 (srednji).** Dodaj u klasu `Par<T1, T2>` metodu `public Par<T2, T1> Obrni()` koja vraća **novi** par sa zamenjenim podacima. Na primer, od para `("jabuka", "apple")` nastaje par `("apple", "jabuka")`, a od `("Luka", 5)` nastaje `(5, "Luka")`. Ispiši oba obrnuta para.

**Zadatak 3 (napredni).** Dopuni klasu `Ranac<T>` iz Primera 3:

- metodom `public bool Sadrzi(T predmet)` koja vraća `true` ako je predmet u rancu (poređenje radi sa `Equals`, kao u prošloj lekciji),
- metodom `public bool Ukloni(int indeks)` koja uklanja predmet sa zadatog indeksa tako što sve predmete iza njega pomeri za jedno mesto ulevo i smanji `broj`. Ako indeks nije ispravan (manji od 0 ili veći ili jednak `broj`), metoda vraća `false`.

U `Main` napuni ranac, ukloni srednji predmet, pa ispiši ranac pre i posle uklanjanja.

---

## Pitanja za diskusiju

1. Po čemu su `Kutija<int>` i `Kutija<string>` iste, a po čemu se razlikuju?
2. Kada biste koristili `Par<string, int>`, a kada biste napravili posebnu klasu `Ucenik` sa svojstvima `Ime` i `Ocena`? Šta je čitljivije u većem programu?
3. Naš `Ranac<T>` ima fiksni kapacitet. Kako biste napravili ranac koji može da se „proširi“ kada se napuni?
