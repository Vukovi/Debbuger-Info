# Debbuger-Info

## 1. Menjanje vrednosti

terminal:
expression NEKA_PROMENLJIVA = nova vrednost
 
ili EDIT BRAKEPOINT:
Action(odaberi: Debbuger command) i onda napisem izraz sa EXPRESSION-om, fakticki ovo iznad sto bih pisao u terminalu


## 2. Dodavanje nove linije u kod

isto kao pod 1. samo sto se jos cekira AUTOMATICALLY CONTINUE AFTER EVALUATING ACTION


## 3. Sumnjiv property & trazenje property-ja po tipu kad znam da odredjeni tip pravi problem

Idi u DABBUGER INSPECTOR i dodaj + i odaberi SYMBOLIC BREAKPOINT

Popuni SYMBOL: bilo koja funkcija ili metoda napisana u ObjC-u, npr: -[UILabel setText:];

debugger ce se onda pokusati da odradi ovaj zahtev i ukoliko moze, dace nekoliko podstavki ovom brakepointu ili jednu podstavku, u stavri onoliko koliko ih pronadje

Zatim popuni ako treba CONDITION: moze da se postavi neki EXPRESSION (npr logicki) koji ce okidati brakepoint samo kad se Condition ispuni

A u konzoli: 
po $arg1 => npr da objekat
po $arg2 => npr da neki broj i onda mora da se kastuje ovako 
po (SEL)$arg2 => npr dobije se “setText:” i znas onda da je metoda
po $arg3 => …..


## 4. Sumnjiva linija (npr na mestu gde sumnjas da je greska, dodaj normalni BP i onda ga edituj)

ACTION —> Debugger Command —> onda upises: 
breakpoint set -- one shot true     ovo ce uciniti da ovaj BP postoji samo 1 i kad se okine automatski ce se izbrisati
breakpoint set -- one shot true -- name “[UILabel setText:]”       npr sa ovim Labelom jer je sumnja jos veca na njega

i naravno stikliras AUTOMATIC CONTINUE

pa onda u konzoli: po $arg1 pa ako otkrijes nesto, proveri sa BP sa EXPRESSION-om


## 5. Zamena postojece linije

Kad je vec iskompajlirao, dakle zaustavio se na BP, i hocu da ima drugaciju liniju koda od postoajece, mogu da kazem BP-u da zanemari tu liniju i da ucita nesto drugo. To je nesto sa npr. Therad.1: breakpoint 7.1
Ovo mozemo da pomreamo misem, tako da pomeramo mesto izvrsenja koda, i obcino ide ispod linije koja je sumnjiva, a zatim iskoristim EXPRESSION da unesem novu liniju koju testiram.

EDIT —> Action —> Debugger command —> thread jump -- by 1 ovim hocu za jednu limniju da spustim izvresenje

zatim kliknem na + i upisem: EXPRESSION (nova linija koda, tj nova funkcija)

i stikliram AUTOMATIC CONTINUE


## 6. Pratim property nekog objekta

Napravim BP na objektu koji ima property koji zelim da pratim, u konzoli na taj property desni klik i odaberem WHATCH


## 7. Nalazenje preko memorijske adrese

Swift: po self.view.recursiveDescription
ovo treba da vrati sve view-eve sa memorijskim adresama i osnovnim informacijama, ali Swift ce da se buni, pa mora da se kuca u objecitiveC
expression -| objC -O -- [self.view recursiveDescription] 

Skracenica: 
command alias poc expression -| objC -O -

pa onda je koristis:
poc 0Xmemorijska adresa

U objectiveC nema ovog zezanja nego se odmah pozove po 0Xmemorijska adresa

U Swiftu bi moglo da se izbegne ovo zezanje koriscenjem swiftovskog:
po unsafeBitCast(m.adresa, to: Tip.self)     moramo dakle da znamo koji Tip je trazeni property
po unsafeBitCast(m.adresa, to: Tip.self).TipovProperty = nova vrednost

a da bi se ovo primenilo na simulatoru: expression CATransition.flush()

