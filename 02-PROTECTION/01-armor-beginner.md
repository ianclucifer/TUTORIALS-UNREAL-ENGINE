# 💪 ARMOR SYSTEM - Reducerea Daune (ÎNCEPĂTOR)

## Ce vrem să facem?

Vreți armura să vă protejeze! 

```
1. Purtați armură = Primiti mai puține daune
2. Armura se degradează
3. Puteți repara armura
4. Ecranul arată cât de multă armură aveți
```

**Asta este Armor System!** 🛡

---

## Variabilele pe care le Trebuie

```
🔸 ArmorValue: Float = 50.0 (cât de multă armură aveți)
🔸 MaxArmor: Float = 100.0 (maxim armură)
🔸 ArmorDamageReduction: Float = 0.25 (25% daune reduse)
🔸 IsWearingArmor: Boolean = TRUE (purtați armură?)
🔸 ArmorType: String = "IronArmor" (ce tip armură)
```

---

## PASUL 1: Adăugați Variabilele

### În Blueprint-ul Personajului:

```
Variables Panel → Click "+"

1️⃣ Name: "ArmorValue"
   Variable Type: Float
   Default Value: 50.0

2️⃣ Name: "MaxArmor"
   Variable Type: Float
   Default Value: 100.0

3️⃣ Name: "ArmorDamageReduction"
   Variable Type: Float
   Default Value: 0.25

4️⃣ Name: "IsWearingArmor"
   Variable Type: Boolean
   Default Value: TRUE

5️⃣ Name: "ArmorType"
   Variable Type: String
   Default Value: "IronArmor"
```

---

## PASUL 2: Creați Funcția TakeDamageWithArmor

### Event Graph:

```
Click dreapta în Event Graph
Search: "Custom Event"
Click "Custom Event"
Renumiti: "TakeDamageWithArmor"
```

### Adăugați parametru la event:

```
Click pe event
Click Details pe dreapta
Find "Inputs" section
Click "+"

Name: "DamageAmount"
Type: Float
```

---

## PASUL 3: Calculează Damage Redus

### Conectare noduri:

```
TakeDamageWithArmor (Event)
  ↓
Branch (dacă IsWearingArmor = TRUE)
  ↓ (True)
  ↓
  [Damage Calculation]
  FinalDamage = DamageAmount * (1 - ArmorDamageReduction)
  ↓
  Set Health (Health - FinalDamage)
  ↓
  Reduce ArmorValue
```

### Pași detaliat:

```
1. Click din dreapta TakeDamageWithArmor → Branch

2. Verifică IsWearingArmor:
   - Conecta Get IsWearingArmor la Condition

3. (True branch)
   - Crează: 1 - 0.25 = 0.75 (75% damage rămâne)
   - Crează: DamageAmount * 0.75 = FinalDamage
   - Scade Health cu FinalDamage
   - Scade ArmorValue cu (DamageAmount * 0.5)

4. (False branch)
   - Damage normal, fără reducere
```

---

## PASUL 4: Degradare Armură

### În Event Tick:

```
Dacă ArmorValue > 0:
  - Se reduce puțin pe cadru
  - Exemplu: ArmorValue -= 0.01 per frame
  - (reprezentând corodare/uzură)
```

---

## PASUL 5: Reparare Armură

### Creați Custom Event:

```
Click dreapta → Custom Event
Renumiti: "RepairArmor"

Adăugați Input: "RepairAmount" (Float)

Logic:
- ArmorValue = ArmorValue + RepairAmount
- Dacă ArmorValue > MaxArmor
  → ArmorValue = MaxArmor
```

---

## PASUL 6: UI - Armor Bar

### În Canvas (UI):

```
1. Click Add → ProgressBar
   Renumiti: "ArmorBar"

2. Bind to Variable:
   ArmorBar → Details
   → "Percent" → Bind → ArmorValue / MaxArmor

3. Color: Verde (armura e bună)
   Hover: Alb (hover effect)

4. Add Text: "Armor: 50/100"
   Bind Text → "{ArmorValue} / {MaxArmor}"
```

---

## PASUL 7: Test

### Apasați Play:

```
✅ Armura bar apare (verde)
✅ Primind daune = armor se reduce
✅ Armor bar se mișcă
✅ După ce armor moare = damage normal
✅ Puteți repara cu RepairArmor event
```

---

## ⚠️ GREȘELI COMUNE

❌ **GREȘEALĂ 1:** Armor value merge sub 0
```
Fix: Adăugați clamp
ArmorValue = Clamp(ArmorValue, 0, MaxArmor)
```

❌ **GREȘEALĂ 2:** Damage reduction prea mare (100% reducere)
```
Fix: Max reduction = 0.90 (90%)
Nu mai mare!
```

❌ **GREȘEALĂ 3:** Armor se degradează prea rapid
```
Fix: Reduceți degradation rate
Armor -= 0.001 per frame (nu 0.1)
```

---

## 📚 URMĂTORUL PAS

Cititi: `02-PROTECTION/02-body-part-damage-beginner.md` (Daune pe anumite părți ale corpului)

**Gata! Ați creat Armor System!** 🛡️
