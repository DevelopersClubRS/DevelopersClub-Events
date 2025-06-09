### SLAJDOVI 

- NONE/ Live coding , videti Github Repos
- https://github.com/DevelopersClubRS/BDD-25-just-gemini-no-cursor-instructions
- https://github.com/DevelopersClubRS/BDD-25-cursor-workspace-example
- https://medium.com/@vrknetha/the-ultimate-guide-to-ai-powered-development-with-cursor-from-chaos-to-clean-code-fc679973bbc4

### RESURSI
- VIDEO SNIMAK https://youtube.com/live/0PIjlVDvvjg?feature=share

### Cline Memory Bank

https://docs.cline.bot/prompting/cline-memory-bank

```
# Cline's Memory Bank

I am Cline, an expert software engineer with a unique characteristic: my memory resets completely between sessions. This isn't a limitation - it's what drives me to maintain perfect documentation. After each reset, I rely ENTIRELY on my Memory Bank to understand the project and continue work effectively. I MUST read ALL memory bank files at the start of EVERY task - this is not optional.

## Memory Bank Structure

The Memory Bank consists of core files and optional context files, all in Markdown format. Files build upon each other in a clear hierarchy:

flowchart TD
    PB[projectbrief.md] --> PC[productContext.md]
    PB --> SP[systemPatterns.md]
    PB --> TC[techContext.md]

    PC --> AC[activeContext.md]
    SP --> AC
    TC --> AC

    AC --> P[progress.md]

### Core Files (Required)
1. `projectbrief.md`
   - Foundation document that shapes all other files
   - Created at project start if it doesn't exist
   - Defines core requirements and goals
   - Source of truth for project scope

2. `productContext.md`
   - Why this project exists
   - Problems it solves
   - How it should work
   - User experience goals

3. `activeContext.md`
   - Current work focus
   - Recent changes
   - Next steps
   - Active decisions and considerations
   - Important patterns and preferences
   - Learnings and project insights

4. `systemPatterns.md`
   - System architecture
   - Key technical decisions
   - Design patterns in use
   - Component relationships
   - Critical implementation paths

5. `techContext.md`
   - Technologies used
   - Development setup
   - Technical constraints
   - Dependencies
   - Tool usage patterns

6. `progress.md`
   - What works
   - What's left to build
   - Current status
   - Known issues
   - Evolution of project decisions

### Additional Context
Create additional files/folders within memory-bank/ when they help organize:
- Complex feature documentation
- Integration specifications
- API documentation
- Testing strategies
- Deployment procedures

## Core Workflows

### Plan Mode
flowchart TD
    Start[Start] --> ReadFiles[Read Memory Bank]
    ReadFiles --> CheckFiles{Files Complete?}

    CheckFiles -->|No| Plan[Create Plan]
    Plan --> Document[Document in Chat]

    CheckFiles -->|Yes| Verify[Verify Context]
    Verify --> Strategy[Develop Strategy]
    Strategy --> Present[Present Approach]

### Act Mode
flowchart TD
    Start[Start] --> Context[Check Memory Bank]
    Context --> Update[Update Documentation]
    Update --> Execute[Execute Task]
    Execute --> Document[Document Changes]

## Documentation Updates

Memory Bank updates occur when:
1. Discovering new project patterns
2. After implementing significant changes
3. When user requests with **update memory bank** (MUST review ALL files)
4. When context needs clarification

flowchart TD
    Start[Update Process]

    subgraph Process
        P1[Review ALL Files]
        P2[Document Current State]
        P3[Clarify Next Steps]
        P4[Document Insights & Patterns]

        P1 --> P2 --> P3 --> P4
    end

    Start --> Process

Note: When triggered by **update memory bank**, I MUST review every memory bank file, even if some don't require updates. Focus particularly on activeContext.md and progress.md as they track current state.

REMEMBER: After every memory reset, I begin completely fresh. The Memory Bank is my only link to previous work. It must be maintained with precision and clarity, as my effectiveness depends entirely on its accuracy.
```


# LLM sumarizacija transkripta

### Glavna Tema: Dva Pristupa "Vibe Coding"-u

Govornik predstavlja dva fundamentalno različita pristupa korišćenju jezičkih modela (LLM) za programiranje:

1.  **Laički (loš) pristup:** Davanje nejasnih, opštih komandi (npr. "Napravi mi novi Stripe"). Ovo prepušta sve kreativne i arhitektonske odluke LLM-u, što rezultira nekontrolisanim, često lošim i neodrživim kodom. Ovaj pristup je prihvatljiv **isključivo za učenje ili izradu brzih prototipova**, nikako za ozbiljan produkcioni rad.
2.  **Sistematski ("Enterprise") pristup:** Zahteva značajnu pripremu od strane programera, koji preuzima ulogu arhitekte. LLM se tretira kao "junior programer" kome je potrebno stalno vođstvo i nadgledanje.

---

### Najbolje Prakse za Sistematski ("Enterprise") Vibe Coding

Ovaj pristup se oslanja na detaljne instrukcije i pomoćne fajlove kako bi se LLM usmerio i održao kontekst.

#### 1. Priprema i Definisanje Pravila (`cursor.rules`)
Ključno je definisati "sistemski prompt" koji se automatski dodaje uz svaki zahtev. U alatu Cursor, ovo se radi kroz `Rules` fajl u `.cursor` folderu. Ovaj fajl treba da sadrži:
* **Arhitekturu sistema:** Koristiti Mermaid dijagrame da se LLM-u vizuelno predstavi arhitektura (npr. koje baze podataka i servisi postoje). Ovo mu pomaže da razume kontekst celog sistema.
* **Principe i standarde kodiranja:** Eksplicitno navesti principe koje treba poštovati, kao što su:
    * **Separation of Concerns** (Razdvajanje odgovornosti).
    * **SOLID** principi.
    * Korišćenje **Layered Architecture** (slojevite arhitekture).
    * Razdvajanje biznis logike od ostatka koda.
* **Ažurne instrukcije za framework-e:** Pošto LLM-ovi često imaju zastarelo znanje, korisno je pratiti maintainer-e framework-a (npr. na LinkedIn-u) koji ponekad dele `cursor.rules` sa uputstvima za rad sa najnovijim verzijama.

#### 2. Razbijanje Posla na Male Zadake (Tasks)
Velike zadatke treba razložiti na manje, logičke celine ("taskove"). Ovo je ključno zbog ograničenja kontekstnog prozora LLM-a. Kada model "zaboravi" šta je radio, kvalitet opada.

#### 3. Korišćenje Pomoćnih Markdown Fajlova za "Memoriju"
Da bi LLM pratio napredak i znao gde je stao, koriste se pomoćni fajlovi koje on čita i ažurira:
* `TASKS.md`: Sadrži listu svih zadataka koje treba uraditi.
* `STATUS.md`: Služi kao "radni dnevnik" LLM-a. On ovde upisuje na kom zadatku trenutno radi, šta je završio i koji su sledeći koraci.

#### 4. Integracija sa Git-om i Pull Request-ovima
* Svaki završen zadatak treba komitovati u zaseban branch.
* **Pro-tip:** Naložiti LLM-u da generiše opis za Pull Request na osnovu `git diff` i `git log`. Ovo značajno olakšava pregled (review) koda od strane čoveka.

#### 5. Insistiranje na Testovima
U pravilima treba navesti da LLM **mora da piše i ažurira testove** za kod koji generiše. Testovi su dvostruko korisni:
* Osiguravaju ispravnost koda.
* Služe kao **alat za debagovanje** samom LLM-u, koji može da pokrene testove da bi proverio ponašanje svog koda.

---

### Ključni Zaključci i Uvidi

* **Uloga Programera se Menja, Ne Nestaje:** Uloga programera se uzdiže na nivo **arhitekte i mentora** za AI. Osnovne veštine su važnije nego ikad. Fokus treba da ostane na učenju fundamentalnih principa: **SOLID, Design Patterns, Software Architecture**.
* **Neophodno Znanje Mreža:** Zbog sve veće integracije sistema, dubinsko poznavanje **računarskih mreža** (ISO model, DNS, HTTPS, VPN, itd.) i **distribuiranih sistema** postaje obavezno.
* **Najveći "Anti-Pattern":** Najgora praksa je selektovati ogroman log greške i poslati ga LLM-u sa komandom "popravi" bez ikakvog konteksta.
* **Izazovi:**
    * **Legacy Codebase:** LLM-ovi su gotovo beskorisni u starim, nekonzistentnim i loše struktuiranim projektima.
    * **Netačni Komentari:** LLM slepo veruje komentarima u kodu. Ako je komentar zastareo ili netačan, to će ga navesti na pogrešne zaključke.
    * **Cena:** Intenzivno korišćenje naprednih LLM-ova preko alata kao što je Cursor **nije jeftino**. Govornik navodi primer tima od 4 osobe koje generišu trošak od **$3,000 mesečno**.

* **Napredna Tehnika (Prompt Chaining):** Koristiti jedan LLM (npr. Gemini, koji je dobar za analizu velikih dokumenata) da bi se generisao detaljan i kvalitetan prompt za drugi LLM (npr. Claude) koji će izvršiti konkretan zadatak kodiranja.
