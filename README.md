Dette er et skole projekt på Zealand Næstved




<img width="1548" height="501" alt="image" src="https://github.com/user-attachments/assets/62760804-0999-487c-9868-38be83ec41ff" />

# Case: Sikker Brugerstyring og Password-validering

Dette eksempel tager udgangspunkt i et system, hvor brugere kan oprette en profil, logge ind og tildeles rettigheder via **Role-Based Access Control (RBAC)**.  
Fokus er på sikker input-validering, korrekt adgangskontrol og robust teststrategi.

---

## 1. Ækvivalensklasser (Equivalence Partitioning)

Input opdeles i grupper, der forventes at blive behandlet ens af systemet.

### Password-validering

- **Gyldige klasser**
  - Passwords på **8–20 tegn**
  - Indeholder både **bogstaver og tal**
  - (Evt. specialtegn, hvis det er et krav)

- **Ugyldige klasser**
  - Passwords under 8 tegn
  - Passwords over 20 tegn
  - Passwords uden påkrævede tegn (fx specialtegn)

**Sikkerhedsperspektiv:**  
Sikrer at valideringslogikken konsekvent afviser svage passwords og reducerer risikoen for brute-force og credential stuffing.

---

## 2. Grænseværditest (Boundary Value Analysis)

De kritiske grænser for password-længde testes systematisk.

| Længde | Forventet resultat |
|------|-------------------|
| 7    | Afvis |
| 8    | Accepter |
| 9    | Accepter |
| 19   | Accepter |
| 20   | Accepter |
| 21   | Afvis |

**Sikkerhedsperspektiv:**  
Forhindrer *off-by-one* fejl, som kan tillade for korte (usikre) passwords eller forårsage fejl ved ekstremt lange input.

---

## 3. CRUD(L) – Brugerkontoens livscyklus

Vi tester hele livscyklussen for en brugerkonto med fokus på adgangskontrol.

- **Create**  
  Opret bruger med sikkert hashed password (fx bcrypt / Argon2).

- **Read**  
  Brugeren må kun kunne se egne profiloplysninger.

- **Update**  
  Ændring af password kræver verificering med det gamle password.

- **Delete**  
  Sletning af konto understøtter GDPR (*Right to be Forgotten*).

- **List**  
  Oversigt over brugere er kun tilgængelig for brugere med admin-rolle.

---

## 4. Decision Table Test (Adgangskontrol ved login)

Decision tables anvendes til at teste login-logik baseret på flere samtidige betingelser.

| Betingelse            | Forsøg 1 | Forsøg 2 | Forsøg 3 |
|----------------------|----------|----------|----------|
| Korrekt password     | Nej      | Nej      | Ja       |
| Antal fejl < 5       | Ja       | Nej      | Ja       |
| **Handling**         | Fejlbesked | Lås konto | Log ind |

**Sikkerhedsperspektiv:**  
Reducerer risikoen for brute-force angreb og sikrer korrekt kontolåsning.

---

## 5. Cycle Process Test (State Transition)

Test af brugerens tilstand og tilladte overgange i systemet.

### Bruger-tilstande

- **Start:** Uregistreret
- **Handling:** Tilmelding → Afventer e-mail-verifikation
- **Handling:** Verifikation → Aktiv bruger
- **Handling:** 5 forkerte loginforsøg → Låst konto

**Sikkerhedsperspektiv:**  
Sikrer at brugeren ikke kan omgå verifikation eller hoppe direkte til en aktiv konto.

---

## 6. Testpyramiden

Sikkerhedstests fordeles efter Testpyramiden for optimal effektivitet.

- **Unit Tests (Bunden)**  
  Hurtige tests af hashing-algoritmer og input-validering.

- **Integration Tests (Midten)**  
  Test af databaseintegration, korrekt lagring af brugere og korrekt udstedelse af JWT-tokens.

- **E2E / UI Tests (Toppen)**  
  Manuel eller automatiseret test af hele login-flowet i browseren.

---

## Security Gates (DevSecOps)

Testene placeres strategisk i CI/CD-pipelinen som **Security Gates**.

### Commit / Build Gate (Static)

- Unit tests og grænseværditests
- Statisk analyse for:
  - Hardcoded secrets
  - Svage kryptografiske mønstre

### Test / Staging Gate (Dynamic)

- Automatiserede integrationstests (DAST)
- Decision table tests
- Cycle process tests
- Verifikation af CRUD(L)-rettigheder  
  (fx at en almindelig bruger ikke kan liste alle brugere)

### Release Gate

- Endelig penetrationstest eller manuel sikkerhedsaudit
- Gennemgang af adgangskontrol før produktion

---
