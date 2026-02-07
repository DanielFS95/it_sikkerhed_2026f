Dette er et skole projekt på Zealand Næstved




<img width="1548" height="501" alt="image" src="https://github.com/user-attachments/assets/62760804-0999-487c-9868-38be83ec41ff" />

# Teststrategier & Security Gates

Denne besvarelse beskriver sikkerhedsvalidering af et **Kommentar-modul**.  
Formålet er at sikre korrekt datahåndtering og forhindre ondsindet input gennem systematiske testteknikker.

## Strategisk Overblik: Testpyramiden

Projektet anvender **Testpyramiden** som overordnet teststrategi for at sikre en effektiv og hurtig testindsats:

- **Unit Tests (Bunden)**  
  Størstedelen af testene ligger her. Unit tests verificerer meget hurtigt (1–10 ms), at farlige tegn bliver fjernet eller korrekt omkodet i koden.

- **Integration Tests (Midten)**  
  Sikrer at kommentarer gemmes korrekt i databasen og kan hentes igen uden datatab eller ændringer.

- **End-to-End Tests (Toppen)**  
  Færre, men vigtige tests, der kontrollerer hele brugerrejsen i UI’en – fra afsendelse af kommentar til korrekt visning.

## Security Gates & Testteknikker

Implementeringen følger en række **security gates**, der reducerer risiko gennem hele udviklingsforløbet.

### 1. Code / Dev Security Gate (Det tekniske fundament)

Fokus på input-validering og overholdelse af kodestandarder.

- **Ækvivalensklasser**  
  Input opdeles i logiske grupper for at identificere forskelle, f.eks. almindelig tekst versus input med kodesymboler.

- **Grænseværdistest**  
  Kommentarens længde testes ved definerede grænser (fx maks. 500 tegn) ved at afprøve værdier lige under, præcis på og lige over grænsen.

### 2. Integration Security Gate (Forbindelsen)

Sikring af korrekt kommunikation mellem komponenter samt korrekt autorisation.

- **CRUD(L)**  
  Verificering af de grundlæggende dataoperationer: Create, Read, Update, Delete og List.

### 3. System Security Gate (Logik & Flow)

Test af forretningslogik og systemets samlede robusthed.

- **Decision Table Test**  
  Test af rettighedsstyring, f.eks. hvem der må redigere eller slette kommentarer baseret på roller og regler.

- **Cycle Process Test**  
  Systemets stabilitet testes ved gentagne driftscyklusser for at sikre, at ydeevne og funktionalitet ikke degraderes over tid.

## Programmering: Datadrevet Unit Test (PyTest)

I overensstemmelse med **LEG-arbejdsmetoden** anvendes datadrevne unit tests for at sikre læsbarhed og systematisk testdækning.

Implementeringen findes i filen:

```text
tests/test_comments.py
