# Lekcija 1: Osnovni koncept generičkih tipova

**Programiranje, 4. razred · Nastavna celina: Generički tipovi u C#**

## Cilj lekcije

Posle ove lekcije znaćeš:

- zašto pisanje iste klase za svaki tip podatka (KutijaInt, KutijaString…) pravi problem,
- kako parametar tipa `T` rešava taj problem i kako se piše generička klasa `Kutija<T>`,
- zašto je generičko rešenje bezbednije od rešenja sa tipom `object`.

---

## Mali rečnik pojmova

Ove termine koristimo kroz celu nastavnu celinu:

| Srpski | Engleski | Primer |
|---|---|---|
| generički tip | generic type | `Kutija<T>` |
| parametar tipa | type parameter | `T` u `Kutija<T>` |
| argument tipa | type argument | `int` u `Kutija<int>` |
| sigurnost tipova | type safety | kompajler ne dozvoljava da u `Kutija<int>` stavimo tekst |
| eksplicitna konverzija (kastovanje) | cast | `(int)nekiObjekat` |
| pakovanje / raspakivanje | boxing / unboxing | `object o = 5;` / `int x = (int)o;` |
| greška pri kompajliranju | compile-time error | Visual Studio podvuče kod crvenom linijom |
| greška pri izvršavanju | runtime error | program se prekida dok radi |
| izuzetak | exception | `InvalidCastException` |

---

## Podsetimo se

Za ovu lekciju treba ti ono što već znaš o **klasama i objektima**:

- **Klasa** je opis (plan) objekta. Sadrži polja (podatke) i metode (ponašanje).
- **Objekat** pravimo operatorom `new`: `Ucenik u = new Ucenik();`
- **Polje** ima tip, na primer `private int ocena;`. U njega može da se upiše **samo** vrednost tog tipa.
- **Metoda** može da prima parametre i vraća vrednost određenog tipa: `public int Uzmi() { ... }`
- Tip `object` je „najopštiji“ tip u C#-u. Promenljivoj tipa `object` možemo da dodelimo bilo koju vrednost.

---

## Objašnjenje

### Problem: isti kod, različiti tipovi

Zamislimo da nam treba klasa koja čuva **jednu vrednost**, recimo kutija u koju stavimo ocenu. Napravićemo klasu `KutijaInt`. Posle nam zatreba kutija za ime učenika, pa pravimo `KutijaString`. Zatim kutija za znak, pa `KutijaChar`…

Te klase su **skoro identične**. Razlikuju se samo po tipu podatka koji čuvaju. To je loše iz više razloga:

- kod se ponavlja (kopiraj-nalepi),
- ako nađemo grešku, moramo da je ispravimo na više mesta,
- za svaki novi tip pišemo novu klasu.

### Prvi pokušaj: tip object

Pošto u `object` može da stane bilo šta, mogli bismo da napravimo jednu klasu `KutijaObject`. Ali tada:

- Kad vadimo vrednost, moramo da je **kastujemo** nazad u pravi tip: `(int)kutija.Uzmi()`.
- Kompajler **ne proverava** šta stavljamo u kutiju. Ako stavimo tekst, a vadimo broj, greška se javlja tek **dok program radi** i program se prekida.
- Kad broj (`int`) stavimo u `object`, C# ga „pakuje“ u objekat, a kad ga vadimo, „raspakuje“ ga. To se zove **pakovanje i raspakivanje (boxing / unboxing)**. Za sada je dovoljno da znaš da je to dodatni posao za računar i da se ne vidi u kodu.

### Rešenje: generički tip

**Generički tip (generic type)** je klasa (ili metoda) koja umesto konkretnog tipa koristi **parametar tipa (type parameter)**. Parametar tipa se obično zove `T` i piše se u izlomljenim zagradama: `Kutija<T>`.

`T` je kao „rupa“ za tip. Kada pravimo objekat, kažemo koji tip ide na mesto `T`. Taj konkretni tip se zove **argument tipa (type argument)**:

- `Kutija<int>`: kutija u kojoj je `T` zamenjeno sa `int`,
- `Kutija<string>`: kutija u kojoj je `T` zamenjeno sa `string`.

Tako pišemo **jednu klasu**, a dobijamo kutiju za bilo koji tip. Pritom kompajler pazi da u `Kutija<int>` ne stavimo tekst. To je **sigurnost tipova (type safety)**: greška se otkriva pri kompajliranju, a ne kod korisnika.

### Analogija iz svakodnevnog života

Zamisli **kutiju za odlaganje sa praznom nalepnicom**, kakve se kupuju u knjižari. Prodavnica ima samo jedan model kutije (jedna klasa). Kad je doneseš kući, na nalepnicu napišeš **„OLOVKE“** ili **„PUNJAČI“**. Od tog trenutka znaš šta je unutra i ne moraš da otvaraš kutiju i pogađaš.

- model kutije iz prodavnice = generička klasa `Kutija<T>`
- prazna nalepnica = parametar tipa `T`
- ono što napišeš na nalepnicu = argument tipa (`int`, `string`…)

Kutija tipa `object` je kutija **bez nalepnice**: u nju možeš da staviš bilo šta, ali svaki put moraš da otvoriš i proveriš šta je unutra.

---

## Sintaksa

```csharp
// Deklaracija generičke klase: T je parametar tipa
class NazivKlase<T>
{
    private T polje;                  // polje tipa T

    public void Metoda(T vrednost)    // parametar tipa T
    {
        polje = vrednost;
    }

    public T DrugaMetoda()            // povratna vrednost tipa T
    {
        return polje;
    }
}

// Upotreba: na mesto T stavljamo konkretan tip (argument tipa)
NazivKlase<int> objekat1 = new NazivKlase<int>();
NazivKlase<string> objekat2 = new NazivKlase<string>();
```

---

## Primeri

### Primer 1: Problem, dve skoro iste klase

```csharp
using System;

namespace Lekcija1Primer1
{
    // Kutija koja može da čuva samo cele brojeve
    class KutijaInt
    {
        private int sadrzaj;

        public void Stavi(int vrednost)
        {
            sadrzaj = vrednost;
        }

        public int Uzmi()
        {
            return sadrzaj;
        }
    }

    // Ista kutija, ali za tekst – kod je skoro identičan!
    class KutijaString
    {
        private string sadrzaj;

        public void Stavi(string vrednost)
        {
            sadrzaj = vrednost;
        }

        public string Uzmi()
        {
            return sadrzaj;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            KutijaInt kutijaSaOcenom = new KutijaInt();
            kutijaSaOcenom.Stavi(5);

            KutijaString kutijaSaImenom = new KutijaString();
            kutijaSaImenom.Stavi("Marko");

            Console.WriteLine("Ocena: " + kutijaSaOcenom.Uzmi());
            Console.WriteLine("Ime: " + kutijaSaImenom.Uzmi());

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Ocena: 5
Ime: Marko
```

**Objašnjenje:**

1. Klase `KutijaInt` i `KutijaString` imaju isto polje `sadrzaj` i iste metode `Stavi` i `Uzmi`.
2. Jedina razlika je tip: `int` u prvoj, `string` u drugoj klasi.
3. Za kutiju sa znakom (`char`) ili logičkom vrednošću (`bool`) trebale bi nam još dve iste klase. To je problem koji rešavamo u ovoj lekciji.

### Primer 2: Kutija za sve (object) i njene zamke

```csharp
using System;

namespace Lekcija1Primer2
{
    // Jedna kutija za sve – čuva vrednost tipa object
    class KutijaObject
    {
        private object sadrzaj;

        public void Stavi(object vrednost)
        {
            sadrzaj = vrednost;
        }

        public object Uzmi()
        {
            return sadrzaj;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            KutijaObject kutija1 = new KutijaObject();
            kutija1.Stavi(5);                    // int se pakuje u object (boxing)

            int ocena = (int)kutija1.Uzmi();     // moramo da kastujemo nazad (unboxing)
            Console.WriteLine("Ocena: " + ocena);

            KutijaObject kutija2 = new KutijaObject();
            kutija2.Stavi("Marko");              // kompajler ne proverava šta stavljamo
            Console.WriteLine("U drugoj kutiji je: " + kutija2.Uzmi());

            // Probajte da uklonite // sa početka sledećeg reda:
            // int broj = (int)kutija2.Uzmi();   // kompajlira se, ali PUCA pri izvršavanju!

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Ocena: 5
U drugoj kutiji je: Marko
```

**Objašnjenje:**

1. `private object sadrzaj;`: u polje tipa `object` može da stane bilo šta, pa je ovo jedna klasa za sve tipove.
2. `kutija1.Stavi(5);`: broj 5 se pakuje u objekat (boxing). To se ne vidi u kodu, ali računar radi dodatni posao.
3. `(int)kutija1.Uzmi()`: metoda vraća `object`, pa moramo sami da kažemo da je unutra `int` (kastovanje, raspakivanje).
4. `kutija2.Stavi("Marko");`: kompajler ne zna da smo mislili da u kutije stavljamo samo brojeve, pa ne javlja grešku.
5. Ako uklonite `//` ispred reda `int broj = (int)kutija2.Uzmi();`, program će se **kompajlirati bez greške**, ali će se pri pokretanju prekinuti sa izuzetkom **`InvalidCastException`** (tekst ne može da se pretvori u broj). Greška se otkriva kasno, dok program radi.

### Primer 3: Rešenje, generička klasa Kutija&lt;T&gt;

```csharp
using System;

namespace Lekcija1Primer3
{
    // Jedna generička klasa umesto KutijaInt, KutijaString, ...
    // T je parametar tipa – „mesto“ za tip koji biramo kasnije
    class Kutija<T>
    {
        private T sadrzaj;

        public void Stavi(T vrednost)
        {
            sadrzaj = vrednost;
        }

        public T Uzmi()
        {
            return sadrzaj;
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Kutija<int> kutijaSaOcenom = new Kutija<int>();
            kutijaSaOcenom.Stavi(5);
            int ocena = kutijaSaOcenom.Uzmi();       // nema kastovanja!

            Kutija<string> kutijaSaImenom = new Kutija<string>();
            kutijaSaImenom.Stavi("Marko");
            string ime = kutijaSaImenom.Uzmi();

            // kutijaSaOcenom.Stavi("Marko");       // GREŠKA pri kompajliranju!

            Console.WriteLine("Ocena: " + ocena);
            Console.WriteLine("Ime: " + ime);
            Console.WriteLine("Ocena + 1 = " + (ocena + 1));

            Console.ReadKey();
        }
    }
}
```

**Izlaz u konzoli:**

```
Ocena: 5
Ime: Marko
Ocena + 1 = 6
```

**Objašnjenje:**

1. `class Kutija<T>`: `T` je parametar tipa. Unutar klase `T` koristimo kao da je običan tip.
2. `private T sadrzaj;`: polje je tipa `T`. U `Kutija<int>` to je `int`, a u `Kutija<string>` to je `string`.
3. `Kutija<int> kutijaSaOcenom = new Kutija<int>();`: `int` je argument tipa. Tip se piše i levo i desno od znaka `=`.
4. `int ocena = kutijaSaOcenom.Uzmi();`: metoda `Uzmi` ovde vraća baš `int`, pa kastovanje nije potrebno.
5. Ako uklonite `//` ispred `kutijaSaOcenom.Stavi("Marko");`, Visual Studio odmah podvuče red crvenom linijom i javi grešku *Argument 1: cannot convert from 'string' to 'int'*. Greška je otkrivena **pre** pokretanja. To je sigurnost tipova.
6. `(ocena + 1)`: sa vrednošću iz kutije možemo odmah da računamo, jer kompajler zna da je to `int`.

---

> ### Zapamtite!
>
> 1. **Generički tip** se piše jednom, a radi za bilo koji tip podatka.
> 2. **Parametar tipa** (`T`) piše se u izlomljenim zagradama posle imena klase: `class Kutija<T>`.
> 3. Kada pravimo objekat, `T` zamenjujemo konkretnim tipom (**argument tipa**): `new Kutija<int>()`.
> 4. Generička klasa je **bezbednija** od rešenja sa `object`: pogrešan tip otkriva kompajler, a ne korisnik programa.
> 5. Sa generičkim tipom **nema kastovanja** pri vađenju vrednosti.

---

> ### Česte greške
>
> **1. Zaboravljen argument tipa pri pravljenju objekta**
>
> ```csharp
> Kutija k = new Kutija();              // GREŠKA
> ```
> Poruka: *Using the generic type 'Kutija&lt;T&gt;' requires 1 type arguments*.
> Ispravno: `Kutija<int> k = new Kutija<int>();`
>
> **2. Različiti tipovi levo i desno**
>
> ```csharp
> Kutija<int> k = new Kutija<string>();  // GREŠKA
> ```
> `Kutija<int>` i `Kutija<string>` su **različiti tipovi**. Ispravno: isti argument tipa sa obe strane.
>
> **3. Pogrešan tip vrednosti**
>
> ```csharp
> Kutija<int> k = new Kutija<int>();
> k.Stavi("pet");                       // GREŠKA: "pet" je string, a ne int
> ```
> Ispravno: `k.Stavi(5);`

---

## Vežbe na času

**Zadatak 1 (osnovni).** Pokreni Primer 3. Zatim dodaj kutiju tipa `Kutija<char>` u koju ćeš staviti prvo slovo svog imena i kutiju tipa `Kutija<bool>` sa vrednošću `true` (da li si danas prisutan). Ispiši sadržaj obe kutije.

**Zadatak 2 (srednji).** U Primeru 3 ukloni `//` ispred reda `kutijaSaOcenom.Stavi("Marko");`. Prepiši u svesku poruku greške koju prikazuje Visual Studio. Zatim isto uradi sa Primerom 2 (red `int broj = ...`): pokreni program i zapiši šta se desilo. U dve rečenice objasni razliku između ove dve greške.

**Zadatak 3 (napredni).** Dopuni klasu `Kutija<T>`:

- dodaj polje `private bool imaSadrzaj;` koje se postavlja na `true` u metodi `Stavi`,
- dodaj metodu `public bool JePrazna()` koja vraća `true` ako u kutiju još ništa nije stavljeno,
- u `Main` napravi jednu praznu i jednu punu kutiju i ispiši rezultat metode `JePrazna()` za obe.

---

## Pitanja za diskusiju

1. Zašto je bolje da grešku otkrije kompajler nego da se program prekine dok ga koristi neko drugi?
2. Setite se programa koje ste ranije pisali. Gde ste pisali skoro isti kod za različite tipove (na primer, metode za ispis niza celih brojeva i niza stringova)?
3. Ako u `object` može da stane bilo šta, zašto su nam onda uopšte potrebni generički tipovi?
