# 🗡️ MELEE COMBAT SYSTEM - Lupte de Apropiere (ÎNCEPĂTOR)

## Ce vrem să facem?

Vreți să atacați inamicii cu o sabie!

```
1. Apasă Mouse Click = atacă cu sabie
2. Sabia lovește inamicul = primește daune
3. Animație de atac se joacă
4. Sunet de atac se aude
5. Se vede pe cine ați lovit
```

**Asta este Melee Combat!** ⚔️

---

## Variabilele pe care le Trebuie

```
🔸 CurrentWeapon: String = "Sword" (arma activă)
🔸 MeleeDamage: Float = 25.0 (daune pe atac)
🔸 AttackRange: Float = 200.0 (cât departe poate lovi)
🔸 AttackCooldown: Float = 1.0 (secunde între atacuri)
🔸 IsAttacking: Boolean = FALSE (în mitlocul unui atac?)
🔸 CanAttack: Boolean = TRUE (poate ataca acum?)
```

---

## PASUL 1: Adăugați Variabilele

### În Blueprint Personajului:

```
Variables Panel → Click "+"

1️⃣ Name: "CurrentWeapon"
   Type: String
   Default: "Sword"

2️⃣ Name: "MeleeDamage"
   Type: Float
   Default: 25.0

3️⃣ Name: "AttackRange"
   Type: Float
   Default: 200.0

4️⃣ Name: "AttackCooldown"
   Type: Float
   Default: 1.0

5️⃣ Name: "IsAttacking"
   Type: Boolean
   Default: FALSE

6️⃣ Name: "CanAttack"
   Type: Boolean
   Default: TRUE
```

---

## PASUL 2: Input Attack (Mouse Click)

### Event Graph:

```
Click dreapta
Search: "Left Mouse Button"
Click "LMB Pressed"

Acesta detectează click stânga mouse
```

### Conectare:

```
LMB Pressed
  ↓
  Branch: CanAttack = TRUE?
  ↓ (True)
  Set IsAttacking = TRUE
  Set CanAttack = FALSE
  Call "ExecuteAttack"
  Start AttackCooldown Timer
  ↓ (False)
  Print: "Trebuie să aștepți!"
```

---

## PASUL 3: Execută Atacul

### Custom Event: "ExecuteAttack"

```
Logic:

1. Play Animation: "Attack_Montage"
   (animația personajului care atacă)

2. Sphere Trace (detectare inamici)
   - Din pozitia personajului
   - Rază: AttackRange
   - Cauta: Enemies

3. Pentru fiecare inamic găsit:
   - Aplică Damage = MeleeDamage
   - Play Hit Sound
   - Play Hit Particle Effect
```

### Pași detaliat:

```
1. Custom Event ExecuteAttack
   ↓
2. Search For Enemies In Range
   (folosiți Sphere Trace by Channel)
   ↓
3. For Each Enemy Hit:
   - Call TakeDamage
   - Damage = MeleeDamage
   - Apply Knockback
   - Play Blood Effect
```

---

## PASUL 4: Cooldown de Atac

### Timer:

```
Custom Event: "ResetAttackCooldown"

Logic:
- Set IsAttacking = FALSE
- Set CanAttack = TRUE
- Play Idle Animation
```

### Conectare:

```
ExecuteAttack
  ↓
Set Timer (1.0 secunde)
  ↓ (După 1 secundă)
ResetAttackCooldown
```

---

## PASUL 5: Animații de Atac

### Animation Blueprint:

```
1. Creatii State Machine:
   - Idle
   - Attacking
   - Hit (când inamicul e lovit)

2. Tranziții:
   - Idle → Attacking: dacă IsAttacking = TRUE
   - Attacking → Idle: după 0.8 sec

3. Add Animation Notify:
   - La frame-ul 0.5 al animației
   - Notifică "WeaponHit"
   (la momentul optim al atacului)
```

---

## PASUL 6: Sunet și Efecte

### Sunete:

```
1. Add Audio Component
   Name: "AttackSound"

2. Când ExecuteAttack:
   - Play "Sword_Swing" sound

3. Când Enemy Hit:
   - Play "Sword_Hit" sound
```

### Efecte Vizuale:

```
1. Add Particle System
   Name: "BloodEffect"

2. Când inamicul e lovit:
   - Spawn Blood Particle at Hit Location
   - Tintă: locul atacului
```

---

## PASUL 7: Knockback

### Dupa Atac:

```
1. Pentru fiecare inamic lovit:
   - Calculează Direction (departe de jucător)
   - Apply Impulse = Direction * 500.0
   - Inamicul se împinge înapoi
```

---

## PASUL 8: Test

### Apasați Play:

```
✅ Click mouse → personajul atacă
✅ Animație de atac se joacă
✅ Sunet de atac se aude
✅ Inamicii ating = primesc daune
✅ Inamici sunt împinși înapoi
✅ Sânge se vede pe impact
✅ Nu poti ataca imediat (cooldown)
```

---

## ⚠️ GREȘELI COMUNE

❌ **GREȘEALĂ 1:** Sfera de detectare prea mică
```
Fix: Măriți AttackRange la 300
```

❌ **GREȘEALĂ 2:** Inamicii nu primesc damage
```
Fix: Verificați dacă SpherTrace are channel corect
→ Trace Channel = Weapon
```

❌ **GREȘEALĂ 3:** Puteți ataca prea repede
```
Fix: Măriți AttackCooldown la 1.5
```

---

## 📚 URMĂTORUL PAS

Cititi: `07-COMBAT/02-ranged-beginner.md` (Arme la distanță)

**Gata! Ați creat Melee Combat System!** ⚔️🩸
