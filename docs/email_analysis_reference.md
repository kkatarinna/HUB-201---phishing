# Email Security & Phishing Detection Checklist

Ovaj dokument sadrži pregled svih ključnih delova mejla koje treba proveriti za detekciju phishing napada, kao i osnovni pregled procesa razvoja proizvoda.

Cilj je obuhvatiti sve platforme koje koriste mail (gmail, outlook web mail, yahoo...)

Razvoj web aplikacije + ekstenzije za web browsere + mobilne aplikacije 

SRODNE APLIKKACIJE: [proofpoint](https://www.proofpoint.com/us)

---

## 1. SMTP - Envelope nivo (Transportni sloj)

**Opis:**  
SMTP je protokol za slanje mejla sa jednog na drugi server. Sadrži:

- `MAIL FROM` (pravi pošiljalac / return path)  
- `RCPT TO` (primalac)  

> Napomena: `MAIL FROM` se može razlikovati od header-a u mejlu, zato je važno proveravati ga.

**Provere:**

1. IP adresa pošiljaoca  
   - neočekivani domen  
   - sumnjiva država  
   - nema reverse DNS-a  
2. Uporediti `MAIL FROM` iz SMTP-a i `From` iz header-a  
3. SPF provera – da li IP adresa sme da šalje mejlove za taj domen  
4. HELO / EHLO provera  

```text
EHLO mail.example.com
MAIL FROM: attacker@gmail.com
RCPT TO: victim@company.com
DATA
```
## 2.  Header
Header mejla sadrži informacije o pošiljaocu, primaocu, datumu i putanji mejla kroz servere, i predstavlja ključni deo za detekciju sumnjivih i zlonamernih poruka.
**Osnovni delovi:**

- `From:` (prikazani pošiljalac)  
- `To:`  
- `Subject:`  
- `Date:`  
- `Reply-To:` ⚠️ često različit od `From`  
- `Return-Path:` ⚠️

**Provere:**

1. Mismatch između `Reply-To` i `From`  
2. Received chain – da li prvi sender (originalni domen) odgovara IP domenu  
3. Autentikacija: SPF, DKIM, DMARC  
   - Provera potpisivanja mejla  
   - Neki servisi već imaju integrisanu proveru

---

## 3. Body

### Plain text
- Idealan za NLP klasifikaciju ili keyword scoring

### HTML deo
- Mismatch u linkovima i njihovom prikazu (tekst vs URL)  
- Skriveni elementi (`display:none`)  
- `<form>` tagovi i ostali sumnjivi HTML elementi

---

##  4.1 URL analiza

**Provera za svaki URL:**

- Domen i subdomeni  
- Dužina URL-a  
- IP umesto domena  
- Punycode (`xn--`)  
- Mnogi subdomeni → sumnjivo  
- Domen ≠ brand (npr. `paypal.secure-login.ru`)  
- URL shorteneri

**Dodatne analize:**

- Whois analiza – domen mlađi od 30 dana je sumnjiv  
- Sandboxing – korišćenje gotovih servisa za sigurnu analizu URL-a

---

## 4.2 Attachments (Prilozi)

- Tip fajla (`.exe`, `.zip`, `.docm`)  
- Ime fajla  
- MIME type  

**Dodatne provere:**

- Sandboxing za sigurnu analizu priloženih fajlova

---

## Edukacija u cilju prodaje proizvoda

- Obuka korisnika i awareness kampanje kao deo phishing prevencije

---

## Proces razvoja proizvoda (Startup Flow)

1. **Ideja** – definisanje problema i osnovne ideje rešenja  
2. **Validacija** – proverava se da li problem zaista postoji i da li korisnici žele rešenje  
3. **Prototip** – osnovni model proizvoda (skice, klikabilni dizajn, demo)  
4. **MVP (Minimum Viable Product)** – minimalna funkcionalna verzija proizvoda sa ključnim funkcionalnostima  
5. **PMF (Product-Market Fit)** – potvrđuje se da proizvod rešava problem i da ga korisnici žele  
6. **Iteracije** – stalno unapređivanje proizvoda na osnovu feedback-a  
7. **Skaliranje** – rast korisnika, tržišta i infrastrukture  
8. **Monetizacija** – definisanje kako proizvod zarađuje novac  
9. **Ekspanzija** – širenje proizvoda na nova tržišta i segmente

---