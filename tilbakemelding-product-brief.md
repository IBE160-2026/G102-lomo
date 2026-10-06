# Tilbakemelding på product brief

| | |
|---|---|
| **Gruppe** | G102 – G102-lomo |
| **Product brief** | `product-brief.md` (commit 7fd2087) |
| **Tilbakemelding fra** | Faglærer i IBE160 (utarbeidet med KI-støtte) |
| **Dato** | 2026-10-06 |

## Samlet vurdering

- **Godt utgangspunkt med justeringer.** Gruppen kan gå videre og innarbeide punktene under.

**Det som er bra:**

1. KarriereKlar har en tydelig kjerneflyt: last opp CV, lim inn stillingsannonse, få gap-analyse, søknadsutkast og CV-forslag. Det er lett å se for seg skjermbildene, og det gir et godt utgangspunkt for PRD og stories.
2. Avgrensningen er gjennomtenkt: brukeren limer inn annonseteksten selv (ingen skraping av Finn.no eller LinkedIn), og betaling, integrasjoner og mobilapp er bevisst utsatt. Sammenligningen med generiske chatboter er også konkret og relevant.

**De viktigste endringene:**

1. Suksesskriteriene er ikke testbare i emnet: «over 80 % av brukerne rapporterer …» krever mange brukere, og «innen 5 sekunder» er urealistisk for en språkmodell som skal lage både analyse og søknadsbrev. Lag kriterier som kan bli testtilfeller.
2. Appen er helt avhengig av en språkmodell, men briefen sier ikke hvilken, hva den koster, eller hvordan sensor kan kjøre appen uten deres nøkkel. Legg inn en plan for nøkkel, kostnad og demomodus.
3. V1 inneholder tre filformater inn (PDF, DOCX, TXT), tre ut (DOCX, PDF, Markdown), innlogging og kryptering i tillegg til fire KI-funksjoner. Smal inn formatene. Fila bør også lagres som ren Markdown – i dag er alle overskrifter og lister «escapet» med `\`, slik at formateringen ikke vises.

## Vanskelighetsgrad og gjennomførbarhet

### Vurdert vanskelighetsgrad

- **Middels**

**Sammenlignbart med:** 2) AI CV- og søknadsassistent (middels) – briefen er i praksis dette forslaget.

**Begrunnelse:**

| Faktor | Nivå (lav / middels / høy) | Kommentar |
|---|---|---|
| Domenelogikk – hvor mange og hvor kompliserte regler og beregninger må stemme? | Middels | Lite klassisk regellogikk, men gap-analysen må defineres: hva regnes som et krav i annonsen, og når er det dekket av CV-en? |
| Datamodell – antall entiteter og relasjoner mellom dem | Lav | Bruker, CV, stillingsannonse og genererte resultater. Enkel modell. |
| Brukere, roller og innlogging | Middels | Én rolle med innlogging. Innloggingen må virke lokalt uten eksterne kontoer. |
| KI-funksjonalitet i appen, f.eks. kall til språkmodell, prompts i koden og håndtering av usikre svar | Høy | Gap-analyse, ATS-råd, søknadsbrev med valgbar tone og CV-forslag. KI-svarene er kjernen, og kvaliteten er vanskelig å måle. |
| Integrasjoner og eksterne tjenester, f.eks. API-er, betaling og e-post | Middels | Språkmodell-API. Ingen andre eksterne tjenester i v1 – bra. |
| Sanntid, samtidighet eller flere brukere som påvirker hverandre | Lav | Brukerne påvirker ikke hverandre. |
| Filhåndtering, f.eks. opplasting, PDF-lesing og eksport | Høy | Lesing av PDF, DOCX og TXT, og eksport til DOCX, PDF og Markdown. Pålitelig tekstuttrekk fra CV-er med kolonner og tabeller er krevende. |
| Sikkerhet og personvern | Middels | CV-er inneholder personopplysninger. Innlogging og kryptering er nevnt, og at CV-teksten sendes til en ekstern språkmodell bør også omtales. |

**Hva vanskelighetsgraden betyr for dere:**

- _Middels:_ Et godt balansert valg. Pass på at kjerneflyten blir ferdig og stabil før dere legger til mer. For KarriereKlar betyr det: få gap-analysen og søknadsbrevet til å fungere godt med PDF-CV og innlimt annonse før dere legger til flere filformater og eksportvalg.

### Gjennomførbarhet med BMAD og Claude Code

Dere skal planlegge med BMAD (product brief → PRD → arkitektur → epics og stories) og implementere med Claude Code. Vurderingen under tar hensyn til at det må være tid til hele denne flyten, og til testing, retting og README til slutt.

| Spørsmål | Vurdering (OK / risiko / stor risiko) | Kommentar |
|---|---|---|
| **Tid og omfang** – kan v1 realistisk bli ferdig og stabil i løpet av semesteret, med tid til flere iterasjoner? | Risiko | Kjerneflyten er realistisk for én person. Seks filformater, innlogging og kryptering samtidig gjør det stramt. |
| **BMAD-flyten** – er briefen konkret nok til at PRD, arkitektur og stories kan lages uten store hull, og blir det overkommelig mange stories? | OK | Funksjonene og avgrensningen er tydelige nok til å lage PRD og stories. |
| **Egnet for Claude Code** – bruker løsningen en vanlig, godt dokumentert teknologistakk som Claude Code håndterer godt, eller krever den nisjeteknologi, spesialmaskinvare eller mye manuell konfigurasjon? | OK | En webapp med opplasting, innlogging og LLM-kall passer Claude Code godt. Velg ett bibliotek for PDF-lesing og ett for eksport. |
| **Kontroll på KI-ens arbeid** – kan gruppen selv avgjøre om koden gjør det riktige? Krever domenet kunnskap gruppen ikke har, f.eks. avanserte beregninger eller fagregler, så er det vanskelig å kvalitetssikre. | Risiko | Dere kan vurdere om et søknadsbrev er godt, men det er vanskeligere å vite om gap-analysen er fullstendig. Lag noen faste testsett (CV + annonse) der dere på forhånd har bestemt hvilke krav som mangler. |
| **Testbarhet** – finnes det tydelige regler og forventede resultater som tester kan skrives mot? | Risiko | Filopplasting, eksport og innlogging kan testes godt. KI-delen må testes med faste eksempler, sjekk av struktur og mock-svar. |
| **Kjørbar for sensor** – kan appen kjøres lokalt etter README, uten gruppens nøkler, betalte kontoer eller egen infrastruktur? | Risiko | Ikke beskrevet. Sensor trenger en testbruker, eksempel-CV-er og enten egen nøkkel med tydelig oppskrift eller en demomodus. |
| **Avhengigheter og kostnader** – krever løsningen betalte API-er, f.eks. språkmodeller, og finnes det en plan for kostnad, testmodus eller mock-data? | Stor risiko | Ingen plan for modell, kostnad eller mock-data. Uten dette kan verken tester eller sensor kjøre kjerneflyten. |

**Konklusjon om gjennomførbarhet:**

- **Gjennomførbart med justert omfang.** Se forslagene under.

**Forslag til justering av omfang eller vanskelighetsgrad:**

1. V1: innlogging, CV som PDF (eventuelt også TXT), annonse som innlimt tekst, gap-analyse, søknadsbrev med valgbar tone og CV-forslag, med eksport til ett format (f.eks. DOCX eller PDF). Flytt øvrige formater til trinn 2.
2. Legg inn en demomodus med mock-svar fra språkmodellen og noen fiktive eksempel-CV-er og annonser i repoet, så både tester og sensor kan kjøre kjerneflyten.
3. Vis gap-analysen strukturert (krav → finnes i CV / mangler → forslag), så resultatet blir lettere å vurdere og teste enn fri tekst.

## Hvorfor product brief er viktig for mappen

Product brief er utgangspunktet for PRD, arkitektur, stories og til slutt koden. Del 1 av mappen vurderes blant annet på om sensor kan følge en sporbar vei fra plan til ferdig app. Den vurderes også på om appen gjør det dere har beskrevet, om den er testet, om den er godt designet, og om den kan kjøres etter README. Et uklart, for stort eller for lite brief gjør alt dette vanskeligere senere. Det er mye enklere å rette nå enn sent i semesteret.

## 1. Gjennomgang av briefens deler

| Del av brief | Status | Kommentar |
|---|---|---|
| Executive Summary – er det klart hva appen er, og hvilket problem den løser? | OK | Klart hva KarriereKlar gjør og hvorfor. |
| The Problem – er problemet konkret, med reelle situasjoner og brukere? | OK | To konkrete utfordringer: generiske søknader og «nøkkelord-blindhet». |
| The Solution – beskriver løsningen brukeropplevelsen, ikke bare teknologi? | OK | Beskriver hva brukeren gjør og får. Legg gjerne til at brukeren kan redigere utkastet før eksport. |
| What Makes This Different – er vurderingen ærlig og realistisk? | Juster | Sammenligningen med ChatGPT og veiledere er god. Nevn også eksisterende spesialverktøy (f.eks. Jobscan eller Teal), og hva dere gjør annerledes enn dem. |
| Who This Serves – er primærbrukerne tydelige, og vet vi hva de trenger? | Juster | «Studenter, nyutdannede og arbeidssøkere» er bredt. Velg én konkret primærbruker, f.eks. en student som søker sommerjobb eller første jobb etter studiene. |
| Success Criteria – kan kriteriene faktisk sjekkes eller testes? | Endre | Erstatt med testbare kriterier, f.eks. «for testsett A finner gap-analysen de tre manglende kravene vi har definert», «en DOCX-CV på to sider leses uten tap av tekst», «en ugyldig fil gir en forståelig feilmelding», «brukeren kan slette sine data». |
| Scope – er det klart hva som er med i første versjon, og hva som ikke er det? | Juster | Tydelig hva som ikke er med. «Inkludert» bør smalnes inn (formater), og deles gjerne i en punktliste så det blir lettere å lage stories. |
| Vision – henger visjonen sammen med resten uten å blåse opp omfanget? | OK | Intervjusimulering og kursforslag er naturlige utvidelser og holdes utenfor v1. |

## 2. Utgangspunkt for del 1 av mappen

Punktene følger kriteriene i sensorveiledningen for del 1. Vektene i parentes viser hvor mye hvert kriterium teller i del 1.

| Kriterium i del 1 | Hva briefen bør legge til rette for | Status | Kommentar |
|---|---|---|---|
| **1. Prosess og KI-styring** (30 %) | Brief som er presis nok til at PRD og stories kan bygges direkte på den, slik at krav kan spores fra brief til kode. | Juster | Godt grunnlag. Rett formateringen av fila, og lagre gjerne promptene for gap-analysen og søknadsbrevet, så sensor ser hvordan dere har styrt og forbedret dem. |
| **2. Funksjonalitet og omfang** (20 %) | Realistisk omfang for gruppen og semesteret: en tydelig kjerneflyt som kan bli ferdig og stabil, og nok innhold til å vise reell funksjonalitet. | Juster | Tydelig kjerneflyt med nok funksjonalitet. Smal inn filformatene. |
| **3. Kvalitetssikring og testing** (15 %) | Suksesskriterier og funksjoner som er konkrete nok til å bli testtilfeller. | Endre | Lag testbare kriterier og faste testsett med forventet resultat. |
| **4. Design og brukeropplevelse** (10 %) | Tydelige brukere og brukssituasjoner som designet kan bygges rundt, gjerne med de viktigste skjermbildene eller flytene skissert. | Juster | Skisser hvordan gap-analysen, søknadsutkastet og CV-forslagene vises, og hvordan brukeren redigerer før eksport. |
| **5. Kodekvalitet og arkitektur** (10 %) | Teknologivalg som er begrunnet og ikke mer komplekse enn appen trenger. | OK | Ingen teknologivalg ennå, og det er greit. Samle LLM-kallene i én modul som kan byttes med mock. |
| **6. README og kjørbarhet** (10 %) | Løsning som andre kan kjøre lokalt uten betalte kontoer, og uten tilgang til gruppens egne tjenester og nøkler. | Endre | Planlegg `.env.example`, testbruker, eksempel-CV-er og demomodus allerede nå. |
| **7. Ryddighet i repoet** (5 %) | En plan for hvor hemmeligheter, testdata og dokumentasjon skal ligge. | Juster | Bruk bare fiktive CV-er som testdata, aldri ekte. API-nøkler i `.env`. Flytt briefen til en egen mappe for planleggingsdokumenter. |

## 3. Neste steg for gruppen

1. Lagre briefen på nytt som ren Markdown, uten `\` foran overskrifter og lister.
2. Erstatt suksesskriteriene med testbare kriterier, og lag 3–5 fiktive testsett (CV + annonse) med forventet gap-analyse.
3. Velg språkmodell, og beskriv kostnad, demomodus og hvordan sensor kjører appen uten deres nøkkel.
4. Smal inn filformatene i v1, og gå deretter videre til PRD.

Oppdater product brief i repoet når dere har gjort endringene, slik at historikken viser hvordan planen utviklet seg. Det er en del av prosessen sensor ser etter.
