# Zadaci za vežbu kod kuće: Generički tipovi u C\#

**Programiranje, 4\. razred**

Zadaci su poređani od lakših ka težim. Svaki zadatak je poseban konzolni program (Visual Studio 2010, Console Application). Oznaka u zagradi kaže koje gradivo zadatak vežba.

---

## Zadatak 1 (generička metoda) ★

Napiši generičku metodu

static T\[\] Ponovi\<T\>(T vrednost, int n)

koja vraća **novi niz** od n elemenata, a svi su jednaki vrednost. Na primer, Ponovi("C\#", 3\) vraća niz { "C\#", "C\#", "C\#" }. Isprobaj metodu za string, int i char i ispiši nizove metodom IspisiNiz\<T\> iz Lekcije 2\. Jednom je pozovi sa eksplicitnim tipom, a jednom uz zaključivanje tipa.

## Zadatak 2 (generička klasa) ★

Napravi generičku klasu Trojka\<T1, T2, T3\> sa svojstvima Prvi, Drugi i Treci, konstruktorom koji ih postavlja i metodom Ispisi() koja ispisuje (prvi, drugi, treci). Napravi tri objekta sa različitim tipovima, na primer:

- ime učenika, razred i da li je prisutan: Trojka\<string, int, bool\>,  
- predmet, ocena i datum kao tekst: Trojka\<string, int, string\>,  
- tri cela broja (dimenzije kutije): Trojka\<int, int, int\>.

## Zadatak 3 (List\<T\>) ★★

Učitavaj ocene sa tastature dok korisnik ne unese 0 i dodaj ih u List\<int\>. Ocene manje od 1 ili veće od 5 odbij uz poruku. Zatim ispiši:

- broj petica,  
- najmanju i najveću ocenu (petljom, bez gotovih metoda),  
- **novu** listu koja sadrži sve ocene osim jedinica.

Ako nije uneta nijedna ocena, ispiši poruku.

## Zadatak 4 (List\<T\>) ★★

**Red u menzi.** Napravi program sa menijem:

1 \- Stani u red

2 \- Usluzi prvog

3 \- Ispisi red

0 \- Kraj

Opcija 1 pita za ime i dodaje ga na kraj reda. Opcija 2 ispisuje ko je uslužen i uklanja ga sa početka reda. Opcija 3 ispisuje red sa rednim brojevima od 1\. Ako je red prazan, opcije 2 i 3 ispisuju poruku, a program ne sme da pukne.

## Zadatak 5 (lista objekata) ★★

Napravi klasu Film sa svojstvima Naziv i Godina i konstruktorom. U Main napravi List\<Film\> sa šest filmova. Ispiši:

- sve filmove snimljene posle 2010\. godine,  
- najstariji film,  
- zatim ukloni sve filmove snimljene pre 2000\. godine, ispiši koliko je filmova uklonjeno i ispiši listu koja je ostala.

*Pazi:* uklanjanje ne sme da se radi u foreach petlji.

## Zadatak 6 (generička klasa i List\<T\>) ★★★

Napravi generičku klasu Spisak\<T\> koja u sebi ima privatnu listu List\<T\>. Klasa ima:

- metodu Dodaj(T element),  
- metodu int UkloniSve(T vrednost) koja uklanja **sva** pojavljivanja vrednosti i vraća koliko ih je uklonjeno,  
- svojstvo Broj (samo get) koje vraća broj elemenata,  
- metodu Ispisi() koja ispisuje elemente u uglastim zagradama, na primer \[3, 7, 1\].

Isprobaj klasu sa Spisak\<int\> i Spisak\<string\>.

## Zadatak 7 (Dictionary\<TKey, TValue\>) ★★

**Račun u prodavnici.** Napravi cenovnik Dictionary\<string, int\> sa šest proizvoda (naziv → cena u dinarima). Korisnik unosi naziv proizvoda i količinu, sve dok ne unese „kraj“ umesto naziva. Za nepoznat proizvod ispiši poruku. Na kraju ispiši ukupan iznos računa.

## Zadatak 8 (Dictionary\<TKey, TValue\>) ★★★

**Brojanje slova.** Korisnik unosi reč ili rečenicu. Napravi rečnik Dictionary\<char, int\> u kom je ključ slovo, a vrednost broj pojavljivanja tog slova (razmake preskoči, a velika slova pretvori u mala). Ispiši slova **po abecedi**, svako sa brojem pojavljivanja.

*Pomoć:* kroz string može da se prođe petljom foreach (char znak in tekst). Za ispis po abecedi ključeve prepiši u List\<char\> i sortiraj.

## Zadatak 9 (Dictionary\<TKey, TValue\>) ★★★

**Rečnik sa menijem.** Napravi srpsko-engleski rečnik sa menijem:

1 \- Dodaj rec

2 \- Prevedi

3 \- Obrisi rec

4 \- Ispisi sve (po abecedi)

0 \- Kraj

Ako reč već postoji, program pita da li da zameni prevod (d/n). Prevođenje koristi TryGetValue. Sav unos pretvori u mala slova.

## Zadatak 10 (rečnik čije su vrednosti liste) ★★★★

**Takmičenje u tri kruga.** Korisnik unosi broj takmičara, a zatim za svakog ime i poene u tri kruga. Podatke čuvaj u rečniku Dictionary\<string, List\<int\>\> (ime → lista poena po krugovima). Isto ime ne sme da se unese dva puta. Program ispisuje:

- za svakog takmičara poene po krugovima i ukupan zbir, na primer Ana: 20 \+ 15 \+ 25 \= 60,  
- pobednika (najveći zbir),  
- zatim uklanja sve takmičare sa ukupno manje od 50 poena i ispisuje ko ide dalje.

*Pazi:* uklanjanje iz rečnika ne sme da se radi u foreach petlji kroz taj isti rečnik.