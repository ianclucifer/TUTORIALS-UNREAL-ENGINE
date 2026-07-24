# 🏃 SPRINT SYSTEM - Alergare Rapidă (ÎNCEPĂTOR)

## Ce vrem să facem?

Vreți să alergi mai repede când apăsați SHIFT!

```
1. Apasă SHIFT = alergare rapidă (+100% viteză)
2. Pierde Stamina pe parcurs
3. Cand Stamina = 0 → nu mai poți alerg
4. Ecranul arată Stamina bar
```

**Asta este Sprint System!** 🏃💨

---

## Variabilele pe care le Trebuie

```
🔸 IsRunning: Boolean = FALSE (alergezi?)
🔸 SprintMultiplier: Float = 2.0 (dublezi viteza)
🔸 SprintStaminaCost: Float = 15.0 (consum stamina/sec)
🔸 NormalSpeed: Float = 600.0 (viteza normala)
🔸 SprintSpeed: Float = 1200.0 (viteza alergare)
```

---

## PASUL 1: Adăugați Variabilele

### În Blueprint Personajului:

```
Variables Panel → Click "+"

1️⃣ Name: "IsRunning"
   Type: Boolean
   Default: FALSE

2️⃣ Name: "SprintMultiplier"
   Type: Float
   Default: 2.0

3️⃣ Name: "SprintStaminaCost"
   Type: Float
   Default: 15.0

4️⃣ Name: "NormalSpeed"
   Type: Float
   Default: 600.0

5️⃣ Name: "SprintSpeed"
   Type: Float
   Default: 1200.0
```

---

## PASUL 2: Programare Input (Tastă SHIFT)

### Event Graph:

```
Click dreapta
Search: "Input Action"
Click "Input Action SprintStart"

Acesta detectează când apasă SHIFT
```

### Conectare:

```
Input SprintStart (Pressed)
  ↓
  Check: Stamina >= SprintStaminaCost?
  ↓ (True)
  Set IsRunning = TRUE
  Set Character Speed = SprintSpeed
  Start Drain Stamina Timer
  ↓ (False)
  Print: "Nu ai destulă energie!"
```

---

## PASUL 3: Opriți Sprint

### Creatiii Input Release:

```
Input SprintStart (Released)
  ↓
  Set IsRunning = FALSE
  Set Character Speed = NormalSpeed
  Stop Drain Stamina Timer
```

---

## PASUL 4: Consum Stamina

### Custom Event: "DrainstaminaPerSecond"

```
Logic:
While IsRunning = TRUE:
  - Stamina -= SprintStaminaCost * DeltaTime
  
  - Daca Stamina <= 0:
    → Stop Running
    → IsRunning = FALSE
    → Speed = NormalSpeed
    → Start Recovery (Stamina regeneră)
```

---

## PASUL 5: UI Stamina Bar

### În Canvas:

```
1. Add Progress Bar
   Name: "StaminaBar"
   Position: Sub Health Bar

2. Bind Percent:
   → Stamina / MaxStamina

3. Color: Galben (Stamina e în regulă)
   Hover: Alb
   Când obosit: Roșu

4. Add Text: "Stamina: 50/100"
   Bind → "{Stamina} / {MaxStamina}"
```

---

## PASUL 6: Animații Sprint

### Character Animation:

```
1. Deschideți Animation Blueprint

2. Creatii State Machine:
   - Idle (stare cu picioarele)
   - Walking (mers)
   - Running (alergare rapidă)

3. Conectare:
   Idle → Walking → Running
   
   Tranziție:
   - Walking → Running: dacă IsRunning = TRUE
   - Running → Walking: dacă IsRunning = FALSE
```

---

## PASUL 7: Sunet Sprint

### Audio Cues:

```
1. Add Audio Component

2. Creatii Sound Cue: "Sprint_Sound"

3. Logic:
   - Când IsRunning = TRUE
   → Play Sprint Sound (Loopin)
   
   - Când IsRunning = FALSE
   → Stop Sprint Sound
```

---

## PASUL 8: Test

### Apasați Play:

```
✅ Apasă SHIFT → personajul aleargă
✅ Viteză crește (mai repede pe ecran)
✅ Stamina bar scade
✅ După ce Stamina = 0 → forced stop
✅ Animație schimbă la alergare
✅ Sunet de alergare se aude
```

---

## ⚠️ GREȘELI COMUNE

❌ **GREȘEALĂ 1:** Sprint prea rapid
```
Fix: Reduceți SprintMultiplier la 1.5
```

❌ **GREȘEALĂ 2:** Stamina se consumă prea lent
```
Fix: Măriți SprintStaminaCost la 25.0
```

❌ **GREȘEALĂ 3:** Input SHIFT nu merge
```
Fix: Verificați Project Settings → Input
→ Adăugați "SprintStart" action
→ Mapați la Lshift Key
```

---

## 📚 URMĂTORUL PAS

Cititi: `03-MOVEMENT/03-crouch-beginner.md` (Mers ghemuït)

**Gata! Ați creat Sprint System!** 🏃‍♂️💨
