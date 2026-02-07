Dette er et skole projekt på Zealand Næstved




<img width="1548" height="501" alt="image" src="https://github.com/user-attachments/assets/62760804-0999-487c-9868-38be83ec41ff" />

# Opgave: Teststrategier & Security Gates
Denne besvarelse dækker sikkerhedsvalidering af et **Kommentar-modul** (f.eks. til en blog eller social platform). [cite_start]Målet er at sikre korrekt datahåndtering og forhindre ondsindet input gennem systematiske testteknikker[cite: 1, 115].

## 🏗️ Strategisk Overblik: Test Pyramiden
[cite_start]Vi anvender **Test Pyramiden** som vores styrende strategi for at sikre en effektiv og hurtig testindsats[cite: 148, 168]:

* **Unit Tests (Bunden):** Her ligger flest tests. [cite_start]De verificerer lynhurtigt (1-10ms), om "farlige" tegn bliver fjernet eller omkodet korrekt i koden[cite: 154, 155, 167].
* [cite_start]**Integration Tests (Midten):** Verificerer at kommentarerne gemmes sikkert i databasen og hentes ud igen uden datatab[cite: 152, 153].
* [cite_start]**End-to-End Tests (Toppen):** Færre, men vigtige tests, der tjekker hele brugerrejsen i UI'en fra afsendelse til visning[cite: 149, 157, 160].

## 🛡️ Security Gates & Test Teknikker
[cite_start]Implementeringen følger en række "gates" for at minimere risici gennem hele udviklingsforløbet[cite: 170, 171].

### 1. Code / Dev Security Gate (Det tekniske fundament)
[cite_start]Fokus på input-validering og kodestandarder[cite: 175, 176].

* [cite_start]**Ækvivalens klasser:** Vi opdeler input i logiske grupper for at kategorisere forskelle[cite: 78, 79]:
    * **Gyldige:** Almindelig tekst (f.eks. "Godt indlæg!").
    * [cite_start]**Ugyldige:** HTML/Scripts (f.eks. `<script>`) – Skal blokeres for at undgå XSS[cite: 83].
* [cite_start]**Grænseværdi test:** Vi tester grænserne for kommentarens længde (f.eks. maks 500 tegn)[cite: 84, 85]:
    * [cite_start]**499 tegn:** Skal tillades (lige under)[cite: 87, 92].
    * [cite_start]**500 tegn:** Skal tillades (lige på)[cite: 88, 93].
    * [cite_start]**501 tegn:** Skal afvises (lige over)[cite: 89, 94].

### 2. Integration Security Gate (Forbindelsen)
[cite_start]Sikring af sikker kommunikation og korrekt autorisation[cite: 183, 184].

* [cite_start]**CRUD(L):** Verificering af de grundlæggende dataoperationer[cite: 114, 115]:
    * [cite_start]**Create (C):** Oprettelse af en ny kommentar knyttet til den rigtige bruger[cite: 116].
    * [cite_start]**Read (R):** Visning af kommentaren uden at eksponere sensitive data[cite: 117].
    * [cite_start]**Update (U):** Kun ejeren må kunne ændre i kommentaren[cite: 118].
    * [cite_start]**Delete (D):** Sikker sletning fra både UI og database[cite: 119].

### 3. System Security Gate (Logik & Flow)
[cite_start]Her tester vi forretningslogikken og systemets modstandsdygtighed[cite: 188, 192].

* [cite_start]**Decision Table Test:** Logik for rettighedsstyring[cite: 138, 139]:
    | Er bruger logget ind? | Egen kommentar? | Admin-rolle? | Handling/Resultat |
    | :--- | :--- | :--- | :--- |
    | Nej | - | - | Kan kun læse |
    | Ja | Ja | Nej | Kan redigere/slette |
    | Ja | Nej | Ja | Kan slette (moderation) |
* [cite_start]**Cycle Process Test:** Vi tester systemets stabilitet ved at lade en proces køre gentagne driftscyklusser (opret/slet) for at sikre, at ydeevnen ikke falder over tid[cite: 121, 122, 124].

---

## 💻 Programmering: Data-dreven Unit Test (PyTest)
[cite_start]I overensstemmelse med opgaven er her en læsbar datadreven test, der dækker både grænseværdier og logik[cite: 222, 223]. Se filen `test_comments.py`.
