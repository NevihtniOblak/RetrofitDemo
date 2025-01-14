# RetrofitDemo

## Retrofit

Retrofit je priljubljena in robustna knjižnica za Android, ki poenostavi povezovanje z API-ji, omogoča preprosto izvajanje HTTP zahtevkov in pretvorbo odgovorov v uporabne Java objekte. Zasnovana je za uporabo REST principov, kar omogoča enostavno integracijo s številnimi zunanjimi storitvami. Retrofit podpira različne oblike serializacije odgovorov, kot sta Gson in Moshi, kar poenostavi delo z JSON podatki. Knjižnica se lahko enostavno prilagodi različnim potrebam in omogoča učinkovito obvladovanje napak, časovnih omejitev in asinhronih operacij.

## ❓ Zakaj?

Za retrofit sem se odločil predvsem zaradi njegove preprostosti za uporabo, visoke združjivosti z drugimi knjižnicami, ter razširjenosti, saj ima knjižnica široko bazo uporabnikov.

## ✅ Prednosti

- Enostavno definiranje in uporaba API-jev.
- Podpora za različne pretvornike in asinhrone okvirje.
- Odlična združljivost z OkHttp in drugimi knjižnicami.
- Redno vzdrževanje in podpora aktivne skupnosti.
- Zmogljivosti za obravnavo kompleksnih scenarijev (npr. preslikave v grafe objektov).

## ❌ Slabosti

- Odvisna od OkHttp, kar pomeni večji paket za osnovno implementacijo.
- Zahteva dodatne knjižnice za delo z JSON-om (npr. Gson ali Moshi).
- Manj primerna za preproste projekte, ki ne potrebujejo kompleksnih zahtev.

## 📊 Statistika repozitorija
- 43.2k zvezdic
- 1.6k watchers
- 7.3k forkov

## 🛠️ Vzdrževanje projekta
- redne posodobitve -> zadnji commit 10. januar 2025
- Open issues: 123
- Contributers: 160

## ⚖️ Licenca
Knjižnica uporablja Apache 2.0 licenco:

- Programsko opremo lahko prosto uporabljamo v osebne in komercialne namene, vendar moramo spoštovati določene pogoje.
- Imamo pravico do spreminjanja izvorne kode in ustvarjanja izpeljanih del, prav tako pa jih lahko distribuiramo naprej. V kolikor to storimo, moramo vključiti izvorno obvestilo o avtorstvu in licenco.
- Brez garancij: Program, ki je pod to licenco, se distribuira brez garancij ali jamstev. To pomeni, da razvijalci in nosilci avtorskih pravic niso odgovorni za morebitno škodo ali težave, ki nastanejo zaradi uporabe programske opreme.

(Več o licenci) [https://opensource.org/license/apache-2-0]

## Primer uporabe

### Prikaz vremena za 7 dni z uporabo Open-Meteo API

<img width="805" alt="Posnetek zaslona 2025-01-14 211814" src="https://github.com/user-attachments/assets/89d356b2-74ef-4f38-99b8-de0272ed6b41" />

Predpogoj za uporabo retrofit je dodatek ustreznih dependency-jev v gradle ter dovoljenje za uporabo interent a v AndroidManifest.xml

<img width="450" alt="Posnetek zaslona 2025-01-14 211401" src="https://github.com/user-attachments/assets/36845eeb-2ade-4329-b3a2-d5147ff6375b" />

<img width="450" alt="Posnetek zaslona 2025-01-14 211317" src="https://github.com/user-attachments/assets/5c1e2568-3adc-4d2b-965f-60a9f84cc490" />


Definiramo vmesnik kjer bodo metode predstavljale različne endpointe pri komunikaciji. Dodatno logiko dodajamo z anotacijo parametrov teh metod. Npr. QUERY pomeni da se parameter doda kot del argumentov poizvedbe.

<img width="450" alt="image" src="https://github.com/user-attachments/assets/7f65fac4-23a1-4911-b76f-3de270d7b729" />

Ustvarimo Retrofit objekt preko katerega bomo klicali metode oz. pošiljali poizvedbe. Dodamo si še osnovo URL, ter ostale parametre. Zato da lahko podatke serealiziramo in deserealiziramo potrebujemo ConverterFactory

<img width="450" alt="Posnetek zaslona 2025-01-14 211524" src="https://github.com/user-attachments/assets/d9dd90ac-2cbe-4fcb-b0b3-b9e22fb44786" />

Pomožni razredi za hranjenje podatkov o vremenu iz Open-Meteo API

<img width="300" alt="Posnetek zaslona 2025-01-14 211601" src="https://github.com/user-attachments/assets/f7c4c064-5ac9-400a-bf82-c39c563a5f89" />

<img width="300" alt="Posnetek zaslona 2025-01-14 211633" src="https://github.com/user-attachments/assets/1201a962-313e-43f4-aec8-7fbd6833a809" />

V MainActivity si lahko zapišemo funkcijo, ki nam vrne objekt, ki implementira prej definiran vmesnik ter kot osnovo za serializacijo/deserializacijo uporablja naš preddefiniran razred za hrambo podatkov.
Ko pokličemo našo metodo nam je vrnjen Call objekt preko katerega lahko izvedemo asinhron klic na API z enqueue. Dodamo dve callback funkciji. V primeru uspeha vrnjene rezultate obdelamo z *displayWeather*, sicer pa izpišemo napako.

<img width="450" alt="Posnetek zaslona 2025-01-14 211706" src="https://github.com/user-attachments/assets/70038cee-4121-4c4b-94b4-634aebffe272" />

DisplayWeather funkcija nam podatke formatira in izpiše na TextView v MainActivity

<img width="450" alt="Posnetek zaslona 2025-01-14 211717" src="https://github.com/user-attachments/assets/8ad948e8-62c6-4ce0-8cac-91fdc329854b" />

V MainActivity-ju v create metodi zgolj pokličemo našo *fetchAndDisplayWeatherData* funkcijo

<img width="400" alt="Posnetek zaslona 2025-01-14 211733" src="https://github.com/user-attachments/assets/edddb0b1-4048-435a-9bfe-83d841915f08" />

Izris v aplikaciji:

<img width="250" alt="Posnetek zaslona 2025-01-14 212005" src="https://github.com/user-attachments/assets/71f698d7-bf9d-4f4e-aef0-910688bf35c3" />


## Integracija v projektu

Retrofit smo v projektu uporabili pri intergraciji PORA z UVRVRV. Izdelali smo 2 funkcionalnosti: 
- slikanje sadja in ugotavljanje ali je sveže ter katero je
- skeniranje barcode-a za ugotavljanje izdelka

V obeh primerih se na Azure strežnik pošlje slika, iz katere se razberejo informacije.

Definiran vmesnik za pošiljanje zahtevkov na Azure, kjer imamo skripti izdelani pri predmetu *Uvod v računalniški vid in razpoznavanje vzorcev* :
<img width="429" alt="image" src="https://github.com/user-attachments/assets/d29268fc-d720-4de4-93ac-250d8dba8690" />

Definiranje objekta za retrofit
<img width="363" alt="image" src="https://github.com/user-attachments/assets/cf55ebd3-4d8e-49b5-96fc-b1e8f6a7a768" />

Funkcija za izvajanje dejanskih klicov na Azure:

1. del funkcije: razpoznavalnik svežega sadja

<img width="487" alt="image" src="https://github.com/user-attachments/assets/a71acd80-d20b-429d-a1bb-88493dc3f971" />

2. del funkcije: razpoznavalnik črtnih kod

<img width="443" alt="image" src="https://github.com/user-attachments/assets/fbae0f67-57a5-4887-bdae-f718f7959cb3" />

Zgledi poizvedb v aplikaciji:

![Screenshot_20250114_225339_TuringComplete](https://github.com/user-attachments/assets/7277012f-87dc-4316-92f0-88e67598d7af)
![Screenshot_20250114_230401_TuringComplete](https://github.com/user-attachments/assets/2da140c9-8e44-476e-841e-1808bf897fcc)
<img width="646" alt="Posnetek zaslona 2025-01-14 230057" src="https://github.com/user-attachments/assets/b84e2c8e-1859-4f52-91c4-c34453cbd370" />








