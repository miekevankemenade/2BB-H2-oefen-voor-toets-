# Prompt: nieuwe "oefenen voor de toets"-app maken

Gebruik deze prompt om dezelfde app te maken voor een ander hoofdstuk, bijvoorbeeld H3.

1. Kopieer alles onder de streep.
2. Plak het in een nieuw gesprek met Claude.
3. Vul de vijf regels tussen [ ] in.
4. Voeg screenshots toe van het proefwerk (alle opdrachten) en van de antwoorden.
5. Maak vooraf op GitHub een lege repository met de naam die je hieronder invult.
   Zorg dat Claude daar toegang toe heeft.

---

Je bouwt een nieuwe app "oefenen voor de toets" voor wiskunde, VMBO Basis.
Gebruik de bestaande app als basis.
Die staat in mijn GitHub-repository `miekevankemenade/2BB-H2-oefen-voor-toets-`.
Lees eerst `index.html`, `docent.html`, `config.js` en `README.md`.
Werkt alles daarin, dan werkt het ook in de nieuwe app.

**Nieuwe app**
- Hoofdstuk: [bijv. H3 Verhoudingen]
- Klas: [bijv. 2BB]
- Naam van de nieuwe repository: [bijv. 2BB-H3-oefen-voor-toets]
- Opdrachten in de app: [bijv. "alle opdrachten van het proefwerk"]
- Vragen voor de oefentoets: [bijv. "kies er zelf 20, van elke opdracht minstens één"]

Ik stuur screenshots van het proefwerk en van de antwoorden.

## Werkwijze

1. Clone de basis en zet een kopie in de nieuwe repository.
2. Verander in `index.html` alleen de inhoud:
   - `CTX`: de context per opdracht (tekst, tabel of grafiek links op het scherm);
   - `SECTIES`: de vragen;
   - de assenstelsels (`G..`), lijnen en punten;
   - de domeincodes (`D4..`);
   - `TOETS_IDS`: de vragen die in de oefentoets komen;
   - de opsomming op het startscherm.
   Laat de rest van de code staan. Alle functies daarin zijn al getest en goedgekeurd.
3. Zet in `config.js` een nieuwe `MODULE`-naam, bijv. `"2BB H3 Verhoudingen – oefenen voor de toets"`.
   Pas ook de titel in `docent.html` aan.
   De Supabase-gegevens blijven hetzelfde.
4. Neem alle opdrachten van het proefwerk over.
   Bouw elke afbeelding, tabel en grafiek precies na.
   Controleer elk antwoord met de antwoorden die ik stuur.
5. Test automatisch:
   - elke vraag moet goed te beantwoorden zijn, zonder fouten in de console;
   - test ook de oefentoets;
   - test op een computerscherm en een telefoonscherm (400 px breed).
6. Push naar GitHub en zet GitHub Pages aan (branch main, map /root).
7. Werk `README.md` bij.
8. Vertel me welke vragen je zelf hebt moeten opstellen, omdat ze niet volledig op de screenshots stonden.

## Doelgroep en taal

- Leerlingen VMBO Basis met een taalachterstand en een rekenniveau onder 2F.
- Schrijf in eenvoudig Nederlands: korte zinnen, één opdracht per zin.
- **Elke zin op een eigen regel.** Gebruik in `intro` en `vraag` voor elke zin een apart item.
- Zet formules op een eigen regel, met `'F:'` ervoor, bijv. `'F:32 − 4 × aantal uur = lengte'`.
- Gebruik geen moeilijke woorden zoals "vermenigvuldigingsteken", "letterformule" of "roosterpunt".
- Hints helpen, maar geven het antwoord niet weg.

## Lay-out en huisstijl (staat al in de basis)

- Twee kolommen van gelijke breedte: links de afbeelding, grafiek of tabel, rechts de opdracht.
- Alles past op één scherm.
- Lettertype Montserrat, tekst 10 punt, formules 12 punt.
- Kleuren: roze `#E924D5`, paars `#5E0696`, cyaan `#45EDFF`.
- Een witregel tussen de algemene opdracht en de vraag.
- Bij elke vraag staat het examendomein, bijv. `WI/K/4 Algebraïsche verbanden`.
  Zoek het juiste domein op in de syllabus VMBO 2027.

## Soorten vragen (velden in `SECTIES`)

| type | gebruik | belangrijkste velden |
|---|---|---|
| `num` | één getal | `ans`, `eenheid` |
| `tijd` | een tijdstip | `ans: '7:27'` |
| `tabel` | tabel invullen | `kop`, `xs`, `ans`, `gegeven` (vakjes die al ingevuld zijn) |
| `stappen` | berekening stap voor stap | `begin` (de formule) en `velden: [{tpl: '32 − 4 × {} = lengte', ans: [3], invul: true}, …]`. Elke `{}` is een invulvakje. Zet nooit tussenuitkomsten voor: de leerling vult zelf het getal in en rekent elke stap uit. |
| `stappen` met vrij getal | controleren met de formule | Voeg `vrij: x => [[x], [4 * x, 32 - 4 * x], [32 - 4 * x]]` en `vrijUitleg: x => [...]` toe. De leerling mag dan elk getal invullen. De app rekent de stappen na met dát getal en zegt of het antwoord van de vorige vraag klopt. |
| `stappen` met coördinaten | punten aflezen | `velden: [{tpl: 'B( {} , {} )', ans: [-4, -1]}, …]` |
| `stappen` met `as` | assenstelsel afmaken | `velden: [{as: 'y', pos: [...], ans: [...]}, {lab: '…', soort: 'keuze', opties: ['kies…', …], ans: 2}]`, met `graf: () => maakGrafiek(G..leeg)` |
| `mk` | meerkeuze | `opties`, `ans` (index). Met `multi: true` en `ans: [0, 2]` kiest de leerling er meer, en dan verschijnt de knop "Wis antwoorden". |
| `formule` | formule typen | `orig`, `vars`, eventueel `zonderKeer`, `begin`, `moetStarten`, `aantal: 2` |
| `grafiek` (rechte lijn) | zelf punten zetten | `g` (assenstelsel), `f` (de lijn), `min` (minimum aantal punten), `bron: ['3b']` (tabel van de vorige vraag) |
| `grafiek` (knikgrafiek) | punten verbinden | `pts: [[0,0],[2,50],…]`, `volgorde: 'x'` (of `'klik'`), `min`, `bron` |

Elke vraag heeft ook:
- `hint: [...]`: stappen die helpen zonder het antwoord te geven;
- `uitleg: [...]`: de stap-voor-stap-hulp na 4 fouten en na het inleveren van de toets;
- eventueel `fouten: {'waarde': 'soort fout'}`: voor de foutenanalyse, bijv. `{84: 'Eerst min, dan keer gerekend'}`.

## Grafieken

- Leerlingen tekenen zelf.
  Ze kunnen geen punten typen en zien geen coördinaten als ze over het rooster bewegen.
- Bij een grafiekvraag staat de tabel van de vorige vraag boven het assenstelsel, als één tabel die bij de grafiek hoort.
- Alle afbeeldingen en grafieken staan in de linkerhelft van het scherm.
- Kies de schaal (`xs`, `ys`, `snapX`, `snapY`) zo dat alle punten uit de tabel precies op een roosterpunt vallen.

## Oefentoets (staat al in de basis)

- Een apart tabblad "Oefentoets" met de vragen uit `TOETS_IDS`.
- Geen hints en één poging per vraag: 2 punten als het goed is, 0 punten als het fout is.
- Pas na het inleveren ziet de leerling wat goed en fout was, met de uitleg.
- De uitslag komt bij de docent binnen als "… · oefentoets".
- De leerling kan de toets opnieuw maken.

## Functies die al in de basis zitten (niet veranderen)

- **Punten:** 2 punten als het in één keer goed is, 1 punt bij de tweede poging of na een hint, 0 punten daarna.
- **Cijfer:** (punten ÷ totaal × 9) + 1, afgerond op één decimaal.
- **Schrijffouten tellen niet als fout**, bijvoorbeeld een woord in plaats van een getal, een vergeten haakje of lege vakjes.
- **Hulp bij 4 fouten:** stap-voor-stap-uitleg in beeld.
- **Eindscherm:** foutenanalyse, gokkans, kopieerknop voor Forms en download als .txt of .json.
- **Rekenmachine in examenmodel:**
  - je kunt hem verslepen;
  - het min-teken werkt ook voor negatieve getallen;
  - je kunt de som aanpassen na de uitkomst;
  - "Kopieer naar antwoordvak" zet de uitkomst in het antwoordvak.
- **Voorlezen:** 🔊 Lees voor, met een snelheid die de leerling kiest. Formules worden uitgesproken als woorden.
- **Persoonlijke code per leerling:** het werk wordt na elke vraag online opgeslagen, zodat de leerling de volgende les verder kan.
- **Wat de leerling invult:** bij elke poging wordt bewaard wat de leerling precies invulde. Dat ziet de docent terug.

## Docentenpagina (`docent.html`, staat al in de basis)

- Inloggen.
  Alleen e-mailadressen in de tabel `docenten` kunnen iets zien.
  Je kunt je wachtwoord wijzigen na het inloggen.
- Resultaten per klas en per app, met cijfer, score en gokkans.
- **Gokkans:** klik op Laag, Gemiddeld of Hoog voor de redenen per vraag:
  - antwoord binnen 3 seconden;
  - snel opnieuw fout zonder hint;
  - alle meerkeuzeopties uitgeprobeerd.
- **Wat gaat er mis?:**
  - per vraag hoeveel leerlingen hem in één keer goed hadden;
  - de meest gemaakte fouten;
  - met "Bekijk" de antwoorden per leerling.
  Je kiest tussen Opdrachten en Oefentoets en eventueel een klas.
- **Codes en voortgang:**
  - de code van elke leerling opzoeken;
  - met "Fouten" alle fouten van één leerling bekijken;
  - leerlingen verwijderen.

## Supabase

- Gebruik hetzelfde Supabase-project als de basis (`grmimckwtuczevvgeflc`).
  De tabellen `resultaten`, `voortgang` en `docenten` bestaan al.
- Elke app heeft een eigen `MODULE`-naam in `config.js`.
  Zo kan de docentenpagina filteren per app.
- Maak geen nieuwe tabellen en verander geen beveiliging, tenzij ik daarom vraag.

## Na oplevering

Vraag mij: "Wil je dat ik hier extra vragen aan toevoeg?"
