# **Iptables Firewall Instalācijas un Konfigurācijas Instrukcija uz Debian Servera**

Šī instrukcija detalizēti apraksta, kā instalēt, konfigurēt un testēt `IPTABLES` ugunsmūri uz Debian operētājsistēmas.

---

## **1. Sistēmas Prasības**
Lai uzstādītu un konfigurētu `IPTABLES`, jums būs nepieciešams:
- Operētājsistēma: `Debian 10` vai jaunāka.
- Root piekļuve vai sudo tiesības.
- Interneta savienojums, lai lejupielādētu nepieciešamās pakotnes.
- SSH piekļuve (nav obligāta, bet noderīga).

---

## **2. Instalācijas Soļi**

### **2.1. Atjauniniet Sistēmu**
Atjauniniet pakotnes un nodrošinieties, ka sistēma ir jaunākajā versijā:
```bash
sudo apt update && sudo apt upgrade -y
```
### **2.1 Instalējiet iptables**

```bash
sudo apt install iptables -y
```

### **2.3. Instalējiet iptables-persistent**
Lai automātiski saglabātu un atjaunotu iptables noteikumus:

```bash
sudo apt install iptables-persistent -y
```
---

## **3. Pārbaudiet un Konfigurējiet IPTABLES**

### **3.1. Apskatiet Esošos Noteikumus**
Lai redzētu pašreizējās politikas un noteikumus:

```bash
sudo iptables -L -v
```
### **3.2. Uzstādiet Pamata Politiku**
-P INPUT DROP: Bloķē visu ienākošo trafiku pēc noklusējuma.
-P FORWARD DROP: Bloķē visus pārsūtītos savienojumus.
-P OUTPUT ACCEPT: Atļauj visus izejošos savienojumus (tādējādi ļaujot serverim izveidot savienojumus uz citiem serveriem, piemēram, atjauninājumu serveriem).
```bash
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT
```

### **3.3. Saglabājiet Noteikumus**
Pēc konfigurācijas saglabājiet iptables noteikumus, lai tie paliktu aktīvi pēc servera restartēšanas:

```bash
sudo netfilter-persistent save
```
## **3.4.  Visu noteikumu un politiku atiestatīšana**
Šī komanda dzēš visus ķēžu noteikumus, bet nemaina politikas (piemēram, DROP, ACCEPT utt.).
```bash
sudo iptables -F
```



---
## **4. Testēšana**
### **4.1. Testējiet SSH Savienojumu**
Pārliecinieties, ka SSH savienojums darbojas, pieslēdzoties no citas ierīces un Pārliecinieties, ka serveris atbild uz ping pieprasījumiem:

```bash
ssh [lietotāja_vārds]@[servera_IP]
ping [servera_IP]
```
### **4.3. Testējiet Bloķēšanas Funkcionalitāti**
Lai bloķētu konkrētu IP adresi (piemēram, 192.168.1.100):

```bash 
sudo iptables -I INPUT -s 192.168.1.100 -j DROP
```
Lai apskatīt bloķētus IP
```bash
sudo iptables -L
```
---
## **5. Papildu Konfigurācijas**
### **5.1. Atļaujiet Papildu Portus**
Ja jums ir nepieciešams atļaut citu servisu, piemēram, HTTP (ports 80):
```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```

### **5.2. Dzēsiet Noteikumus**
Lai dzēstu konkrētu noteikumu, vispirms apskatiet noteikumu numurus:
```bash
sudo iptables -L --line-numbers
```
Piemēram, lai dzēstu 2 noteikumu:
```bash
sudo iptables -D INPUT 2
```
---
## **6. Atjaunošana **
Lai nodrošinātu, ka noteikumi netiek zaudēti pēc restartēšanas:
```bash
sudo netfilter-persistent save
```











