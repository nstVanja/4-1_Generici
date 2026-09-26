# Lekcija 2: Generičke metode

**Programiranje, 4. razred · Nastavna celina: Generički tipovi u C#**

## Cilj lekcije

Posle ove lekcije znaćeš:

- da napišeš **generičku metodu** sa parametrom tipa `<T>`,
- da je pozoveš na dva načina: sa **eksplicitno navedenim tipom** i uz **zaključivanje tipa**,
- da napišeš metode `Zameni<T>` i `IspisiNiz<T>` koje rade za bilo koji tip.

---

## Podsetimo se

- **Parametar tipa (type parameter)** `T` je „mesto“ za tip koji se bira kasnije. U prošloj lekciji smo ga koristili u klasi `Kutija<T>`.
- **Statička metoda** u klasi `Program` poziva se direktno iz `Main`: `static void Ispisi(int x) { ... }`.
- **Parametar sa `ref`** prenosi samu promenljivu, a ne njenu kopiju. Promena unutar metode vidi se i posle poziva. `ref` se piše **i u deklaraciji i u pozivu**:

```csharp
static void Uvecaj(ref int broj)
{
    broj = broj + 1;
}
// poziv:
int x = 5;
Uvecaj(ref x);   // x je sada 6
```

- **Niz** ima fiksnu dužinu koju čitamo svojstvom `Length`. Elementima pristupamo indeksom od `0` do `Length - 1`.

---

## Objašnjenje

Ne mora cela klasa da bude generička. Često nam treba samo **jedna metoda** koja radi isti posao za različite tipove. Primer: metoda koja menja vrednosti dve promenljive. Bez generičkih tipova pisali bismo `ZameniInt`, `ZameniString`, `ZameniChar`… a sve bi imale isti kod.

**Generička metoda (generic method)** ima svoj parametar tipa, koji se piše u izlomljenim zagradama **posle imena metode**: `Zameni<T>`. Unutar metode `T` koristimo kao običan tip: za parametre, povratnu vrednost i lokalne promenljive.

Generičku metodu možemo da pozovemo na dva načina:

1. **Eksplicitno navodimo tip:** `Zameni<int>(ref a, ref b);`
2. **Zaključivanje tipa (type inference):** `Zameni(ref a, ref b);`. Kompajler sam vidi da su `a` i `b` tipa `int`, pa zaključuje da je `T = int`.

Drugi način je kraći i češće se koristi. Prvi je koristan kada kompajler ne može sam da zaključi tip ili kada želimo da kod bude jasniji.

### Analogija iz svakodnevnog života

Kako se zamenjuje sadržaj dve čaše, na primer soka i vode? Treba nam **treća, prazna čaša**: sok presipamo u praznu, vodu u čašu gde je bio sok, pa sok iz pomoćne čaše u čašu gde je bila voda.

Postupak je **isti** bez obzira šta je u čašama: sok, voda ili mleko. Bitno je samo da je u obe čaše **ista vrsta** tečnosti, a pomoćna čaša mora da odgovara. To je generička metoda: postupak (kod) je isti, a tip `T` je „vrsta tečnosti“.

---

## Sintaksa

```csharp
// Deklaracija: <T> ide posle imena metode
static PovratniTip NazivMetode<T>(T parametar1, T parametar2)
{
    T pomocna;       // T koristimo kao običan tip
    // ...
}

// Poziv sa eksplicitnim tipom:
NazivMetode<int>(5, 7);

// Poziv uz zaključivanje tipa (kompajler sam vidi da je T = int):
NazivMetode(5, 7);
```

---

## Primeri

### Primer 1: Zameni&lt;T&gt;

```csharp
using System;

namespace Lekcija2Primer1
{
    class Program
    {
        // Generička metoda: menja vrednosti dve promenljive istog tipa
        static void Zameni<T>(ref T a, ref T b)
        {
            T pomocna = a;   // pomoćna promenljiva je takođe tipa T
            a = b;
            b = pomocna;
        }

        static void Main(string[] args)
        {
            int x = 3;
            int y = 8;
            Console.WriteLine("Pre zamene:   x = {0}, y = {1}", x, y);
            Zameni<int>(ref x, ref y);        // tip naveden eksplicitno
            Console.WriteLine("Posle zamene: x = {0}, y = {1}", x, y);

            string prvi = "Ana";
            string drugi = "Luka";
            Console.WriteLine("Pre zamene:   {0}, {1}", prvi, drugi);
            Zameni(ref prvi, ref drugi);      // kompajler zaključuje da je T = string
            Console.WriteLine("Posle zamene: {0}, {1}", prvi, drugi);

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Pre zamene:   x = 3, y = 8
Posle zamene: x = 8, y = 3
Pre zamene:   Ana, Luka
Posle zamene: Luka, Ana
```

**Objašnjenje:**

1. `static void Zameni<T>(ref T a, ref T b)`: `<T>` posle imena kaže da je metoda generička. Oba parametra su tipa `T`, znači **istog** tipa.
2. `T pomocna = a;`: pomoćna promenljiva („treća čaša“) mora da bude istog tipa kao `a` i `b`.
3. `Zameni<int>(ref x, ref y);`: tip je naveden eksplicitno. `ref` se piše i u pozivu.
4. `Zameni(ref prvi, ref drugi);`: tip nije naveden. Kompajler vidi da su argumenti `string` i sam zaključuje da je `T = string`.
5. Ista metoda radi i za `int` i za `string`, a napisali smo je samo jednom.

### Primer 2: Ispis niza bilo kog tipa

```csharp
using System;

namespace Lekcija2Primer2
{
    class Program
    {
        // Ispisuje elemente niza bilo kog tipa, razdvojene zarezom
        static void IspisiNiz<T>(T[] niz)
        {
            for (int i = 0; i < niz.Length; i++)
            {
                Console.Write(niz[i]);
                if (i < niz.Length - 1)
                {
                    Console.Write(", ");   // zarez ne pišemo posle poslednjeg elementa
                }
            }
            Console.WriteLine();
        }

        static void Main(string[] args)
        {
            int[] ocene = { 5, 4, 5, 3 };
            string[] predmeti = { "Programiranje", "Matematika", "Fizika" };
            bool[] prisustvo = { true, false, true };

            Console.Write("Ocene: ");
            IspisiNiz(ocene);               // T = int (zaključeno)

            Console.Write("Predmeti: ");
            IspisiNiz<string>(predmeti);    // T = string (navedeno)

            Console.Write("Prisustvo: ");
            IspisiNiz(prisustvo);           // T = bool (zaključeno)

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Ocene: 5, 4, 5, 3
Predmeti: Programiranje, Matematika, Fizika
Prisustvo: True, False, True
```

**Objašnjenje:**

1. `static void IspisiNiz<T>(T[] niz)`: parametar je **niz** elemenata tipa `T`. Može da primi `int[]`, `string[]`, `bool[]`…
2. `Console.Write(niz[i]);`: `Console.Write` ume da ispiše vrednost bilo kog tipa, pa ovde ne moramo da znamo šta je `T`.
3. `if (i < niz.Length - 1)`: zarez pišemo posle svakog elementa osim poslednjeg.
4. `IspisiNiz(ocene);`: kompajler vidi da je `ocene` tipa `int[]`, pa zaključuje `T = int`.
5. Logičke vrednosti se ispisuju kao `True` i `False` (velikim početnim slovom).

### Primer 3: Pretraga u nizu (rezultati takmičenja)

```csharp
using System;

namespace Lekcija2Primer3
{
    class Program
    {
        // Vraća prvi element niza – povratni tip je T
        static T Prvi<T>(T[] niz)
        {
            return niz[0];
        }

        // Broji koliko puta se tražena vrednost pojavljuje u nizu
        static int Prebroj<T>(T[] niz, T trazeni)
        {
            int brojac = 0;
            foreach (T element in niz)
            {
                if (element.Equals(trazeni))   // za T ne možemo da koristimo ==
                {
                    brojac++;
                }
            }
            return brojac;
        }

        // Vraća indeks prvog pojavljivanja tražene vrednosti ili -1 ako je nema
        static int NadjiIndeks<T>(T[] niz, T trazeni)
        {
            for (int i = 0; i < niz.Length; i++)
            {
                if (niz[i].Equals(trazeni))
                {
                    return i;
                }
            }
            return -1;
        }

        static void Main(string[] args)
        {
            // Pobednici školskog kviza po nedeljama
            string[] pobednici = { "Ana", "Luka", "Milica", "Luka", "Nikola" };
            int[] ocene = { 5, 3, 5, 4, 5, 2 };

            Console.WriteLine("Prvi pobednik: " + Prvi(pobednici));
            Console.WriteLine("Luka je pobedio " + Prebroj(pobednici, "Luka") + " puta.");
            Console.WriteLine("Broj petica: " + Prebroj(ocene, 5));
            Console.WriteLine("Prva dvojka je na indeksu: " + NadjiIndeks(ocene, 2));
            Console.WriteLine("Indeks Marka: " + NadjiIndeks(pobednici, "Marko"));

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Prvi pobednik: Ana
Luka je pobedio 2 puta.
Broj petica: 3
Prva dvojka je na indeksu: 5
Indeks Marka: -1
```

**Objašnjenje:**

1. `static T Prvi<T>(T[] niz)`: generička metoda može i da **vrati** vrednost tipa `T`. Za niz stringova vraća `string`, a za niz celih brojeva vraća `int`.
2. `static int Prebroj<T>(T[] niz, T trazeni)`: povratni tip je običan `int` (broj pojavljivanja), a parametri su generički.
3. `element.Equals(trazeni)`: za dve vrednosti tipa `T` **ne možemo** da pišemo `==`, jer kompajler ne zna koji će tip biti `T` ni da li za njega postoji operator `==`. Metoda `Equals` postoji za svaki tip, pa nju koristimo.
4. `return -1;`: ako petlja prođe ceo niz, a vrednost nije nađena, vraćamo `-1` (taj indeks ne postoji, pa jasno znači „nema“).
5. `Prebroj(ocene, 5)`: kompajler zaključuje `T = int` iz oba argumenta. Zato oba moraju biti istog tipa.

---

> ### Zapamtite!
>
> 1. Parametar tipa generičke metode piše se **posle imena metode**: `static void Zameni<T>(...)`.
> 2. Generička metoda može da se pozove sa eksplicitnim tipom (`Zameni<int>(...)`) ili uz zaključivanje tipa (`Zameni(...)`).
> 3. Zaključivanje tipa radi samo ako kompajler može da vidi tip **iz argumenata**.
> 4. Za poređenje dve vrednosti tipa `T` koristi `Equals`, a ne `==`.
> 5. `T` može da bude tip parametra, tip povratne vrednosti i tip lokalne promenljive.

---

> ### Česte greške
>
> **1. Zaboravljeno `<T>` posle imena metode**
>
> ```csharp
> static void Zameni(ref T a, ref T b)     // GREŠKA
> ```
> Poruka: *The type or namespace name 'T' could not be found*. Kompajler ne zna šta je `T`.
> Ispravno: `static void Zameni<T>(ref T a, ref T b)`
>
> **2. Argumenti različitih tipova**
>
> ```csharp
> int x = 5;
> string ime = "Ana";
> Zameni(ref x, ref ime);                 // GREŠKA
> ```
> Poruka: *The type arguments for method ... cannot be inferred from the usage*. `T` ne može biti istovremeno `int` i `string`.
>
> **3. Poređenje sa `==`**
>
> ```csharp
> if (element == trazeni)                 // GREŠKA
> ```
> Poruka: *Operator '==' cannot be applied to operands of type 'T' and 'T'*.
> Ispravno: `if (element.Equals(trazeni))`
>
> **4. Zaboravljeno `ref` u pozivu**
>
> ```csharp
> Zameni(x, y);                           // GREŠKA
> ```
> Poruka: *Argument 1 must be passed with the 'ref' keyword*.
> Ispravno: `Zameni(ref x, ref y);`

---

## Vežbe na času

**Zadatak 1 (osnovni).** Prepravi metodu `IspisiNiz<T>` iz Primera 2 tako da ispis izgleda ovako: `[5, 4, 5, 3]`, sa uglastim zagradama na početku i kraju. Pozovi je za niz ocena, niz imena drugova iz klupe i niz znakova `char[] slova = { 'C', 'S', 'H', 'A', 'R', 'P' };`.

**Zadatak 2 (srednji).** Napiši generičku metodu `static void ObrniNiz<T>(T[] niz)` koja okreće redosled elemenata u nizu (prvi postaje poslednji…). Unutar nje koristi metodu `Zameni<T>` iz Primera 1. Proveri je na nizu celih brojeva i na nizu stringova, a rezultat ispiši metodom `IspisiNiz`.

*Pomoć:* menjaj element `i` sa elementom `niz.Length - 1 - i`, ali samo do polovine niza. Metodi `Zameni` elemente niza prosleđuješ ovako: `Zameni(ref niz[i], ref niz[j]);`

**Zadatak 3 (napredni).** Napiši generičku metodu `static T[] Spoji<T>(T[] prvi, T[] drugi)` koja vraća **novi** niz sa svim elementima prvog, a zatim drugog niza. Na primer, spajanje `{ 5, 4 }` i `{ 3, 2, 1 }` daje `{ 5, 4, 3, 2, 1 }`. Isprobaj je sa nizovima stringova (spisak učenika iz dve grupe).

*Pomoć:* novi niz praviš sa `T[] rezultat = new T[prvi.Length + drugi.Length];`

---

## Pitanja za diskusiju

1. Kada biste pri pozivu eksplicitno naveli tip (`Zameni<int>(...)`), iako kompajler može sam da ga zaključi?
2. Zašto kompajler ne dozvoljava `==` za dve vrednosti tipa `T`, a dozvoljava `Equals`?
3. Koje metode koje ste ranije pisali za nizove (najveći element, pretraga, ispis…) mogu da postanu generičke? Postoji li neka koja **ne može**, i zašto?
