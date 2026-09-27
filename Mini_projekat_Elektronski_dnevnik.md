# Mini-projekat: Elektronski dnevnik

**Programiranje, 4\. razred · Nastavna celina: Generički tipovi u C\#**

## Šta pravimo?

Konzolni program koji vodi ocene za odeljenje. Podaci se čuvaju u rečniku:

Dictionary\<string, List\<int\>\> dnevnik

- **ključ** je ime učenika (`string`),  
- **vrednost** je lista njegovih ocena (`List<int>`).

Program ima meni:

\=== ELEKTRONSKI DNEVNIK \===

1 \- Dodaj ucenika

2 \- Upisi ocenu

3 \- Obrisi ucenika

4 \- Prosek ucenika

5 \- Ispisi sve

0 \- Kraj

Izbor:

Projekat se radi na **jednom času**, u 5 koraka. Posle svakog koraka program mora da se kompajlira i pokrene. Na kraju je spisak dodatnih zadataka za one koji završe ranije.

*Napomena:* tekst u konzoli je bez slova č, ć, š, ž i đ, jer ih konzola na nekim računarima prikazuje kao upitnike. Komentari u kodu mogu da imaju ta slova.

---

## Šta ti treba iz lekcija

| Potrebno | Gde je objašnjeno |
| :---- | :---- |
| `List<int>`, `Add`, `Count`, `RemoveAt` | Lekcija 4 |
| `ContainsKey`, `TryGetValue`, `Add`, `Remove` | Lekcija 5, deo A |
| `foreach` sa `KeyValuePair`, rečnik sa listama | Lekcija 5, deo B |
| prosek sa `(double)`, `ToString("0.00")`, `string.Join` | Lekcija 5, Primer 6 |
| `int.TryParse(tekst, out broj)` | novo, objašnjeno u koraku 2 |

---

## Korak 0: Kostur programa (4 minuta)

Napravi novi projekat (File → New → Project → Visual C\# → Console Application), nazovi ga **ElektronskiDnevnik** i ceo sadržaj fajla `Program.cs` zameni ovim kosturom. Meni već radi, a metode su prazne: to su tvoji koraci.

using System;

using System.Collections.Generic;

namespace ElektronskiDnevnik

{

    class Program

    {

        // KORAK 1: dodavanje novog učenika

        static void DodajUcenika(Dictionary\<string, List\<int\>\> dnevnik)

        {

            // TODO

        }

        // KORAK 2: upis ocene postojećem učeniku

        static void UpisiOcenu(Dictionary\<string, List\<int\>\> dnevnik)

        {

            // TODO

        }

        // KORAK 3: ispis svih učenika i ocena

        static void IspisiSve(Dictionary\<string, List\<int\>\> dnevnik)

        {

            // TODO

        }

        // KORAK 4: prosek jedne liste ocena

        static double Prosek(List\<int\> ocene)

        {

            // TODO

            return 0;

        }

        // KORAK 4: prosek učenika čije ime unese korisnik

        static void PrikaziProsek(Dictionary\<string, List\<int\>\> dnevnik)

        {

            // TODO

        }

        // KORAK 5: brisanje učenika

        static void ObrisiUcenika(Dictionary\<string, List\<int\>\> dnevnik)

        {

            // TODO

        }

        static void Main(string\[\] args)

        {

            // Ključ je ime učenika, vrednost je lista njegovih ocena

            Dictionary\<string, List\<int\>\> dnevnik \= new Dictionary\<string, List\<int\>\>();

            string izbor \= "";

            while (izbor \!= "0")

            {

                Console.WriteLine();

                Console.WriteLine("=== ELEKTRONSKI DNEVNIK \===");

                Console.WriteLine("1 \- Dodaj ucenika");

                Console.WriteLine("2 \- Upisi ocenu");

                Console.WriteLine("3 \- Obrisi ucenika");

                Console.WriteLine("4 \- Prosek ucenika");

                Console.WriteLine("5 \- Ispisi sve");

                Console.WriteLine("0 \- Kraj");

                Console.Write("Izbor: ");

                izbor \= Console.ReadLine();

                switch (izbor)

                {

                    case "1": DodajUcenika(dnevnik); break;

                    case "2": UpisiOcenu(dnevnik); break;

                    case "3": ObrisiUcenika(dnevnik); break;

                    case "4": PrikaziProsek(dnevnik); break;

                    case "5": IspisiSve(dnevnik); break;

                    case "0": Console.WriteLine("Dovidjenja\!"); break;

                    default: Console.WriteLine("Nepoznata opcija."); break;

                }

            }

            Console.ReadKey();

        }

    }

}

**Proveri:** pokreni program (F5). Meni se prikazuje, izbor 0 završava program, a nepoznat izbor ispisuje „Nepoznata opcija.“

**Zašto ovako?**

- Rečnik se pravi u `Main` i **prosleđuje** svakoj metodi. Sve metode rade sa istim rečnikom.  
- Izbor iz menija čitamo kao tekst (`"1"`, `"2"`…), pa program ne puca ako korisnik slučajno unese slovo.

---

## Korak 1: Dodavanje učenika (6 minuta)

**Cilj:** metoda `DodajUcenika` pita za ime i dodaje učenika sa **praznom** listom ocena.

**Šta treba da uradiš:**

1. Ispiši `Ime ucenika:`  i pročitaj ime.  
2. Ako je ime prazno (`ime == ""`), ispiši poruku i ne dodaj ništa.  
3. Ako učenik već postoji (`ContainsKey`), ispiši da je već u dnevniku.  
4. Inače dodaj par: ključ je ime, vrednost je `new List<int>()`. Ispiši `Dodat: Ana`.

**Pomoć:** ne koristi `Add` pre provere, jer `Add` za postojeći ključ prekida program (`ArgumentException`).

**Proveri:** dodaj Anu, pa ponovo Anu. Drugi put mora da se ispiše poruka, a program ne sme da pukne.

---

## Korak 2: Upis ocene (7 minuta)

**Cilj:** metoda `UpisiOcenu` dodaje ocenu u listu postojećeg učenika.

**Šta treba da uradiš:**

1. Pročitaj ime. Pomoću `TryGetValue` proveri da li učenik postoji i odmah uzmi njegovu listu:  
     
   List\<int\> ocene;  
     
   if (\!dnevnik.TryGetValue(ime, out ocene))  
     
   {  
     
       Console.WriteLine("Ucenik " \+ ime \+ " ne postoji.");  
     
       return;   // prekida metodu, vraćamo se u meni  
     
   }  
     
2. Pročitaj ocenu. Za pretvaranje teksta u broj koristi **`int.TryParse`**. To je metoda koja radi kao `TryGetValue`: vraća `true` ako je tekst ispravan broj i upisuje broj kroz `out` parametar. Za razliku od `int.Parse`, ne puca kada korisnik unese slovo.  
     
   int ocena;  
     
   if (int.TryParse(Console.ReadLine(), out ocena) && ocena \>= 1 && ocena \<= 5\)  
     
   {  
     
       // ocena je ispravna  
     
   }  
     
3. Ako je ocena ispravna, dodaj je u listu (`ocene.Add(ocena)`) i ispiši `Ana: upisana ocena 5`. Inače ispiši `Neispravna ocena.`

**Važno:** `ocene` koju dobiješ iz `TryGetValue` je **ista lista** koja je u rečniku, a ne kopija. Kada dodaš ocenu u nju, ocena je dodata i u dnevnik.

**Proveri:** upiši Ani ocene 5 i 4\. Probaj ocenu 7, slovo „a“ i ime koje ne postoji. Program ne sme da pukne.

---

## Korak 3: Ispis svih učenika (5 minuta)

**Cilj:** metoda `IspisiSve` ispisuje svakog učenika i njegove ocene.

**Šta treba da uradiš:**

1. Ako je dnevnik prazan (`dnevnik.Count == 0`), ispiši `Dnevnik je prazan.` i završi metodu.  
2. Prođi kroz rečnik petljom `foreach (KeyValuePair<string, List<int>> par in dnevnik)`.  
3. Ako učenik nema ocena (`par.Value.Count == 0`), ispiši `Luka: nema ocena`. Inače ispiši ocene razdvojene zarezom: `string.Join(", ", par.Value)`.

**Proveri:** dodaj Luku bez ocena. Ispis treba da izgleda ovako:

Ana: 5, 4

Luka: nema ocena

---

## Korak 4: Prosek (6 minuta)

**Cilj:** metoda `Prosek` računa prosek jedne liste, a `PrikaziProsek` ga ispisuje za učenika čije ime unese korisnik.

**Šta treba da uradiš:**

1. U metodi `Prosek` saberi ocene petljom i vrati `(double)zbir / ocene.Count`. Bez `(double)` bi se delila dva cela broja i prosek bi bio pogrešan.  
2. U metodi `PrikaziProsek` pročitaj ime i proveri ga sa `TryGetValue`. Ako učenik nema ocena, ispiši `Ana jos nema ocena.` **Ne deli nulom\!** Inače ispiši `Ana, prosek: 4,50` (`Prosek(ocene).ToString("0.00")`).  
3. Dopuni `IspisiSve` iz koraka 3 tako da posle ocena ispiše i prosek: `Ana: 5, 4 (prosek 4,50)`.

**Proveri:** Ana sa ocenama 5 i 4 ima prosek 4,50. Sa ocenama 5, 4 i 5 prosek je 4,67. (Na računaru sa engleskim podešavanjima ispis je 4.50 i 4.67.)

---

## Korak 5: Brisanje učenika (4 minuta)

**Cilj:** metoda `ObrisiUcenika` briše učenika iz dnevnika.

**Šta treba da uradiš:** pročitaj ime i pozovi `dnevnik.Remove(ime)`. `Remove` vraća `true` ako je učenik obrisan i `false` ako ga nije bilo, pa ti posebna provera ne treba. Ispiši `Obrisan: Luka` ili `Ucenik Luka ne postoji.`

**Proveri:** obriši Luku, pa ispiši sve. Probaj da obrišeš Luku još jednom.

---

## Primer rada gotovog programa

(Meni se prikazuje posle svake akcije. Ovde je izostavljen da bi primer bio kraći.)

Izbor: 1

Ime ucenika: Ana

Dodat: Ana

Izbor: 1

Ime ucenika: Luka

Dodat: Luka

Izbor: 2

Ime ucenika: Ana

Ocena (1-5): 5

Ana: upisana ocena 5

Izbor: 2

Ime ucenika: Ana

Ocena (1-5): 4

Ana: upisana ocena 4

Izbor: 2

Ime ucenika: Marko

Ucenik Marko ne postoji.

Izbor: 5

Ana: 5, 4 (prosek 4,50)

Luka: nema ocena

Izbor: 4

Ime ucenika: Ana

Ana, prosek: 4,50

Izbor: 3

Ime ucenika: Luka

Obrisan: Luka

Izbor: 0

Dovidjenja\!

---

## Dodatni zadaci (za one koji završe ranije)

1. **Ispis po abecedi:** u opciji 5 ispiši učenike poređane po imenu. *Pomoć:* `List<string> imena = new List<string>(dnevnik.Keys); imena.Sort();` pa prođi kroz listu imena.  
2. **Najbolji učenik:** nova opcija 6 ispisuje učenika sa najvećim prosekom. Učenike bez ocena preskoči.  
3. **Brisanje poslednje ocene:** nova opcija 7 briše poslednju upisanu ocenu učenika (ako je ima). *Pomoć:* `RemoveAt(ocene.Count - 1)`.  
4. **Zaključna ocena:** u ispisu dodaj zaključnu ocenu, zaokružen prosek. *Pažnja:* `Math.Round(4.5)` u C\#-u daje 4, a ne 5\! Upotrebi `Math.Round(prosek, MidpointRounding.AwayFromZero)`.

---

## Šta se ocenjuje

| Stavka | Poeni |
| :---- | :---- |
| Korak 1: dodavanje bez duplikata | 2 |
| Korak 2: upis ocene, provera učenika i ocene | 3 |
| Korak 3: ispis svih | 2 |
| Korak 4: tačan prosek, bez deljenja nulom | 2 |
| Korak 5: brisanje | 1 |
| **Ukupno** | **10** |
| Svaki dodatni zadatak | \+1 |

