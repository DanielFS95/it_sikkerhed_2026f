Dette er et skole projekt på Zealand Næstved




<img width="1548" height="501" alt="image" src="https://github.com/user-attachments/assets/62760804-0999-487c-9868-38be83ec41ff" />





# Opgave: Teststrategier & Security Gates
Herunder er besvarelsen af testopgaverne for emnet: **Sikker Fil-upload og Malware-scanning**.

[cite_start]Besvarelsen er struktureret som en "stabel" (Testpyramide), der viser sammenhængen mellem testteknikker og de relevante Security Gates[cite: 148, 172].

## 🏗️ Test & Security Stack

### 🔝 TOPPEN: System, Strategi & Overblik
[cite_start]I dette lag ligger de overordnede strategier og de tests, der dækker hele brugerrejsen for at bevare det fulde overblik[cite: 156, 160].

| Teknik | Konkret Eksempel for IT-sikkerhed | Security Gate |
| :--- | :--- | :--- |
| **Test Pyramiden** | [cite_start]Som strategi sikrer vi en sund balance med mange Unit tests i bunden og få, men kritiske End-to-End tests af upload-flowet i toppen[cite: 148, 156]. | [cite_start]**Go / No-Go gate**: Endelig kontrol af, at alle sikkerhedstjek er udført, og monitorering er aktiv[cite: 172, 202]. |
| **Cycle Process Test** | [cite_start]Vi tester, om serveren kan håndtere 500 upload/slet-cyklusser i træk uden at løbe tør for diskplads eller lække hukommelse[cite: 122, 124]. | [cite_start]**Release candidate gate**: Validering af systemets stabilitet og "endurance" før release[cite: 172, 195]. |

### 🟦 MIDTEN: Logik, Data & Integration
[cite_start]Her tester vi forretningslogikken og hvordan systemets komponenter (f.eks. applikation og virus-scanner) spiller sammen[cite: 138, 153].

| Teknik | Konkret Eksempel for IT-sikkerhed | Security Gate |
| :--- | :--- | :--- |
| **Decision Table** | [cite_start]Logik for malware-håndtering: Hvis filen er korrekt formateret, men indeholder en virus-signatur, skal den afvises og logges[cite: 139, 143]. | [cite_start]**System security gate**: Verificering af session-håndtering og rollebaseret adgang til filer[cite: 172, 188]. |
| **CRUD(L)** | [cite_start]Test af fil-handlinger: **C**reate (Upload), **R**ead (Download), **U**pdate (Omdøb), **D**elete (Slet), **L**ist (Se filer)[cite: 115, 120]. | [cite_start]**Integration security gate**: Sikring af at integrationer kører over SSL/TLS og bruger "Least Privilege"[cite: 172, 183]. |

### 🧱 BUNDEN: Det Tekniske Fundament (Unit Tests)
[cite_start]Det laveste lag fokuserer på de enkelte kodestumper og tekniske regler for at fange fejl tidligt[cite: 154, 162].

| Teknik | Konkret Eksempel for IT-sikkerhed | Security Gate |
| :--- | :--- | :--- |
| **Ækvivalens klasser** | [cite_start]Vi grupperer fil-typer i kategorier: Godkendte (f.eks. .pdf, .jpg) og forbudte/farlige (f.eks. .exe, .sh)[cite: 78, 79]. | [cite_start]**Code / Dev gate**: Sikring af input-validering og kørsel af statisk kodeanalyse (SAST)[cite: 172, 175]. |
| **Grænseværdi test** | [cite_start]Hvis maks filstørrelse er 10MB, tester vi: 9.9MB (accepter), 10.0MB (accepter) og 10.1MB (afvis)[cite: 85, 91]. | [cite_start]**Code / Dev gate**: Her tjekkes for basale logiske fejl og "hardcoded secrets" i koden[cite: 172, 176]. |

## 💻 Programmering: Data-dreven Unit Test (PyTest)
