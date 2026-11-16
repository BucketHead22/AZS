# Analýza a zpracování signálů – skripta

## O těchto skriptech

Skripta vznikla zpracováním všech deseti přednáškových prezentací předmětu **AZS – Analýza a zpracování signálů** (417 slidů). Pořadí kapitol odpovídá pořadí přednášek. Text slidů je rozvinut do souvislého výkladu: vzorce jsou odvozeny nebo vysvětleny, příklady ze slidů jsou **vyřešeny krok za krokem** (výsledky jsou ověřeny výpočtem) a každá kapitola končí **shrnutím** a **kontrolními otázkami** (u početních je uvedeno řešení).

**Značky v textu:**
- **„Doplnění“** – vysvětlení nebo vzorec, který na slidech není, ale je potřeba k pochopení nebo k řešení úloh (např. bilineární transformace v kap. 9),
- **„Oprava slidu“ / „Pozor“** – místo, kde slide obsahuje překlep nebo chybu; ve skriptech je uvedena správná verze. Souhrnný seznam je v **příloze B**,
- **„Řešený příklad“** – příklad ze slidů (nebo ilustrační příklad) s úplným postupem.

## Obsah předmětu (podle úvodní přednášky)

| Téma předmětu | Kapitola |
|---|---|
| Úvod | 1 |
| Analogové a diskrétní signály, vlastnosti, rozdělení signálů, vzorkování, kvantizace, rekonstrukce | 2 |
| Číselné řady a jejich vlastnosti, základní operace; náhodné signály | 3 |
| Analýza v časové oblasti: diskrétní systémy, linearita a časová invariance, LTI systémy, impulsní odezva | 4 |
| Konvoluce, korelace, periodická konvoluce a korelace, metody výpočtu | 4 |
| Analýza ve frekvenční oblasti: DTFT, frekvenční odezva; DFT, FFT, spektrální analýza, odhad spektra | 5 |
| Z-transformace, přenosová funkce, spojování systémů, analýza systémů | 6 |
| Filtry – základní vlastnosti, frekvenční charakteristiky, typy filtrů, filtry s lineární fází | 7 |
| Číslicová filtrace I: FIR filtry, vlastnosti a metody návrhu | 8 |
| Číslicová filtrace II: IIR filtry, adaptivní filtrace, realizační struktury | 9 |
| Časově-frekvenční analýza (STFT, vlnková transformace, matching pursuit) | 10 |

## Literatura

**Základní:**
- A. Ambardar: *Digital Signal Processing: A Modern Introduction*. Thomson Learning, 2006.
- E. Lai: *Practical Digital Signal Processing for Engineers and Technicians*. Elsevier, 2004.
- Li Tan: *Digital Signal Processing: Fundamentals and Applications*. Elsevier, 2008.
- R. J. Schilling, S. L. Harris: *Fundamentals of Digital Signal Processing Using MATLAB*. Cengage Learning, 2012.

**Doporučená:**
- S. W. Smith: *The Scientist and Engineer's Guide to Digital Signal Processing*. California Technical Publishing, 1999 (volně dostupná online).
- S. K. Mitra: *Digital Signal Processing: A Computer-Based Approach*. McGraw-Hill, 2002.
- J. Jan: *Číslicová filtrace, analýza a restaurace signálů*. VUTIUM, 2002.
- S. J. Orfanidis: *Introduction to Signal Processing*. Prentice Hall, 1996.
- V. Davídek, P. Sovka: *Číslicové zpracování signálů a implementace*. ČVUT, 1996.

Většina příkladů na slidech pochází z Ambardarovy učebnice – kdo chce víc procvičovat, najde tam podobné úlohy.

## Značení

| Symbol | Význam |
|---|---|
| $x(t)$, $x[n]$ | spojitý (analogový) a diskrétní signál |
| $t_s = T_s = T$ | perioda vzorkování |
| $S = f_s = 1/t_s$ | vzorkovací frekvence |
| $f$ [Hz], $\omega = 2\pi f$ [rad/s] | analogová frekvence a úhlová frekvence |
| $F = f/S$ [cykly/vzorek] | číslicová (normalizovaná) frekvence, základní rozsah $-0{,}5 \le F \le 0{,}5$ |
| $\Omega = 2\pi F$ [rad/vzorek] | číslicová úhlová frekvence, základní rozsah $-\pi \le \Omega \le \pi$ |
| $\delta(t)$, $\delta[n]$ | Diracův, resp. jednotkový impuls |
| $u(t)$, $u[n]$ | jednotkový skok |
| $h[n]$, $H(z)$, $H(F)$ | impulsní odezva, přenosová funkce, frekvenční odezva |
| $*$, $\circledast$ | lineární, resp. periodická (kruhová) konvoluce |
| $X(F)$, $X(\Omega)$ | DTFT |
| $X_{DFT}[k]$, $X[k]$ | $N$-bodová DFT |
| $\mathrm{sinc}(x) = \dfrac{\sin(\pi x)}{\pi x}$ | normovaná funkce sinc |

> **Pozor na značení frekvencí:** slidy 2. přednášky používají $\Omega$ pro *analogovou* úhlovou frekvenci (rad/s) a $F$ pro analogovou frekvenci v Hz. Od 3. přednášky (podle Ambardara) je $F$ a $\Omega$ vždy *číslicová* frekvence. Skripta to dodržují podle kapitol.

---

## Obsah

- [1. Úvod do zpracování signálů](#1-úvod-do-zpracování-signálů)
    - [1.1 Co je číslicové zpracování signálů (DSP)](#11-co-je-číslicové-zpracování-signálů-dsp)
    - [1.2 Signál](#12-signál)
    - [1.3 Systém](#13-systém)
    - [1.4 Příklady systémů pro zpracování signálů](#14-příklady-systémů-pro-zpracování-signálů)
    - [1.5 Aplikační oblasti zpracování signálů](#15-aplikační-oblasti-zpracování-signálů)
    - [1.6 Klasifikace metod zpracování signálů](#16-klasifikace-metod-zpracování-signálů)
    - [1.7 Výhody a nevýhody číslicového zpracování](#17-výhody-a-nevýhody-číslicového-zpracování)
- [2. Analogové signály, vzorkování, kvantizace a rekonstrukce](#2-analogové-signály-vzorkování-kvantizace-a-rekonstrukce)
    - [2.1 Spojité (analogové) signály](#21-spojité-analogové-signály)
    - [2.2 Sinusoidy](#22-sinusoidy)
    - [2.3 Elementární spojité signály](#23-elementární-spojité-signály)
    - [2.4 Vzorkování, kvantizace a rekonstrukce – celkový řetězec](#24-vzorkování-kvantizace-a-rekonstrukce-–-celkový-řetězec)
    - [2.5 Vzorkování (sampling)](#25-vzorkování-sampling)
    - [2.6 Vzorkování v praxi – antialiasingový filtr](#26-vzorkování-v-praxi-–-antialiasingový-filtr)
    - [2.7 Vzorkování pásmových signálů (bandpass sampling)](#27-vzorkování-pásmových-signálů-bandpass-sampling)
    - [2.8 Druhy vzorkování](#28-druhy-vzorkování)
    - [2.9 A/D převodníky](#29-ad-převodníky)
    - [2.10 Kvantizace](#210-kvantizace)
    - [2.11 Rekonstrukce signálu (D/A převod)](#211-rekonstrukce-signálu-da-převod)
    - [2.12 D/A převodníky](#212-da-převodníky)
    - [2.13 Řešený příklad – určení vzorkovací periody](#213-řešený-příklad-–-určení-vzorkovací-periody)
    - [Shrnutí kapitoly 2](#shrnutí-kapitoly-2)
- [3. Diskrétní signály (číselné řady), jejich vlastnosti a základní operace; náhodné signály](#3-diskrétní-signály-číselné-řady-jejich-vlastnosti-a-základní-operace-náhodné-signály)
    - [3.1 Diskrétní signál](#31-diskrétní-signál)
    - [3.2 Charakteristiky diskrétního signálu](#32-charakteristiky-diskrétního-signálu)
    - [3.3 Klasifikace diskrétních signálů](#33-klasifikace-diskrétních-signálů)
    - [3.4 Základní diskrétní signály](#34-základní-diskrétní-signály)
    - [3.5 Diskrétní sinusoida](#35-diskrétní-sinusoida)
    - [3.6 Základní operace se signály](#36-základní-operace-se-signály)
    - [3.7 Symetrie signálu](#37-symetrie-signálu)
    - [3.8 Řešené příklady k operacím se signály](#38-řešené-příklady-k-operacím-se-signály)
    - [3.9 Úvod do náhodných signálů](#39-úvod-do-náhodných-signálů)
    - [3.10 Pravděpodobnostní rozložení](#310-pravděpodobnostní-rozložení)
    - [3.11 Vlastnosti náhodných signálů a jejich odhad](#311-vlastnosti-náhodných-signálů-a-jejich-odhad)
    - [3.12 Šum](#312-šum)
    - [Shrnutí kapitoly 3](#shrnutí-kapitoly-3)
- [4. Diskrétní systémy, impulsní odezva, konvoluce a korelace](#4-diskrétní-systémy-impulsní-odezva-konvoluce-a-korelace)
    - [4.1 Diskrétní systém a jeho vlastnosti](#41-diskrétní-systém-a-jeho-vlastnosti)
    - [4.2 Základní stavební bloky diskrétních systémů](#42-základní-stavební-bloky-diskrétních-systémů)
    - [4.3 Analýza diskrétních systémů v časové oblasti](#43-analýza-diskrétních-systémů-v-časové-oblasti)
    - [4.4 Výpočet odezvy – řešení diferenční rovnice](#44-výpočet-odezvy-–-řešení-diferenční-rovnice)
    - [4.5 Odezva při nulovém vstupu a při nulovém stavu (ZIR, ZSR)](#45-odezva-při-nulovém-vstupu-a-při-nulovém-stavu-zir-zsr)
    - [4.6 Impulsní odezva diskrétního systému](#46-impulsní-odezva-diskrétního-systému)
    - [4.7 Diskrétní konvoluce](#47-diskrétní-konvoluce)
    - [4.8 Odezva na periodický vstup a periodická konvoluce](#48-odezva-na-periodický-vstup-a-periodická-konvoluce)
    - [4.9 Dekonvoluce (identifikace systému)](#49-dekonvoluce-identifikace-systému)
    - [4.10 Diskrétní korelace](#410-diskrétní-korelace)
    - [4.11 Inverzní systémy](#411-inverzní-systémy)
    - [Shrnutí kapitoly 4](#shrnutí-kapitoly-4)
- [5. Analýza signálů ve frekvenční oblasti – Fourierova transformace](#5-analýza-signálů-ve-frekvenční-oblasti-–-fourierova-transformace)
    - [5.1 Základní idea transformace](#51-základní-idea-transformace)
    - [5.2 Typy Fourierových transformací](#52-typy-fourierových-transformací)
    - [5.3 DTFT – Fourierova transformace diskrétního signálu (Discrete-Time Fourier Transform)](#53-dtft-–-fourierova-transformace-diskrétního-signálu-discrete-time-fourier-transform)
    - [5.4 Frekvenční odezva systému](#54-frekvenční-odezva-systému)
    - [5.5 Diskrétní Fourierova transformace (DFT)](#55-diskrétní-fourierova-transformace-dft)
    - [5.6 Prosakování ve spektru (spectral leakage)](#56-prosakování-ve-spektru-spectral-leakage)
    - [5.7 Časová okna (spektrální vyhlazení)](#57-časová-okna-spektrální-vyhlazení)
    - [5.8 Odhad spektra](#58-odhad-spektra)
    - [5.9 Rychlá Fourierova transformace (FFT)](#59-rychlá-fourierova-transformace-fft)
    - [Shrnutí kapitoly 5](#shrnutí-kapitoly-5)
- [6. Z-transformace](#6-z-transformace)
    - [6.1 Oboustranná Z-transformace](#61-oboustranná-z-transformace)
    - [6.2 Vlastnosti oboustranné Z-transformace](#62-vlastnosti-oboustranné-z-transformace)
    - [6.3 Převod signálu do Z-oblasti](#63-převod-signálu-do-z-oblasti)
    - [6.4 Z-rovina, nuly a póly](#64-z-rovina-nuly-a-póly)
    - [6.5 Přenosová funkce systému](#65-přenosová-funkce-systému)
    - [6.6 Realizace přenosové funkce](#66-realizace-přenosové-funkce)
    - [6.7 Kauzalita a stabilita LTI systému](#67-kauzalita-a-stabilita-lti-systému)
    - [6.8 Inverzní Z-transformace](#68-inverzní-z-transformace)
    - [6.9 Jednostranná Z-transformace](#69-jednostranná-z-transformace)
    - [6.10 Využití Z-transformace k analýze systémů](#610-využití-z-transformace-k-analýze-systémů)
    - [Shrnutí kapitoly 6](#shrnutí-kapitoly-6)
- [7. Číslicová filtrace – základní vlastnosti filtrů](#7-číslicová-filtrace-–-základní-vlastnosti-filtrů)
    - [7.1 Číslicové filtry](#71-číslicové-filtry)
    - [7.2 Frekvenční charakteristika a její grafická interpretace](#72-frekvenční-charakteristika-a-její-grafická-interpretace)
    - [7.3 FIR a IIR filtry](#73-fir-a-iir-filtry)
    - [7.4 FIR filtry s lineární fází](#74-fir-filtry-s-lineární-fází)
    - [7.5 Další typy filtrů](#75-další-typy-filtrů)
    - [7.6 Ideální filtry a frekvenční transformace](#76-ideální-filtry-a-frekvenční-transformace)
    - [Shrnutí kapitoly 7](#shrnutí-kapitoly-7)
- [8. Návrh FIR filtrů](#8-návrh-fir-filtrů)
    - [8.1 Úlohy při návrhu FIR filtru](#81-úlohy-při-návrhu-fir-filtru)
    - [8.2 Metoda Fourierových řad (okénková metoda) – postup 1](#82-metoda-fourierových-řad-okénková-metoda-–-postup-1)
    - [8.3 Okna pro omezení zákmitů](#83-okna-pro-omezení-zákmitů)
    - [8.4 Návrh podle tolerančního schématu – postup 2](#84-návrh-podle-tolerančního-schématu-–-postup-2)
    - [8.5 Spektrální transformace](#85-spektrální-transformace)
    - [8.6 Půlpásmový (half-band) FIR filtr](#86-půlpásmový-half-band-fir-filtr)
    - [8.7 Metoda frekvenčního vzorkování](#87-metoda-frekvenčního-vzorkování)
    - [Shrnutí kapitoly 8](#shrnutí-kapitoly-8)
- [9. IIR filtry, adaptivní filtrace a realizační struktury](#9-iir-filtry-adaptivní-filtrace-a-realizační-struktury)
    - [9.1 Vlastnosti IIR filtrů](#91-vlastnosti-iir-filtrů)
    - [9.2 Základní metody návrhu IIR filtrů](#92-základní-metody-návrhu-iir-filtrů)
    - [9.3 Aproximace amplitudové charakteristiky (ad 1)](#93-aproximace-amplitudové-charakteristiky-ad-1)
    - [9.4 Obecný postup návrhu IIR filtru](#94-obecný-postup-návrhu-iir-filtru)
    - [9.5 Doplnění: převod analogového filtru na číslicový](#95-doplnění-převod-analogového-filtru-na-číslicový)
    - [9.6 Adaptivní filtrace](#96-adaptivní-filtrace)
    - [9.7 Realizační struktury filtrů](#97-realizační-struktury-filtrů)
    - [Shrnutí kapitoly 9](#shrnutí-kapitoly-9)
- [10. Časově-frekvenční analýza signálu](#10-časově-frekvenční-analýza-signálu)
    - [10.1 Krátkodobá Fourierova transformace (STFT – Short-Time Fourier Transform)](#101-krátkodobá-fourierova-transformace-stft-–-short-time-fourier-transform)
    - [10.2 Waveletová (vlnková) transformace](#102-waveletová-vlnková-transformace)
    - [10.3 Diskrétní waveletová transformace (DWT) – víceúrovňová dekompozice](#103-diskrétní-waveletová-transformace-dwt-–-víceúrovňová-dekompozice)
    - [10.4 Matching pursuit (MP)](#104-matching-pursuit-mp)
    - [Shrnutí kapitoly 10](#shrnutí-kapitoly-10)
- [Příloha A – Přehled nejdůležitějších vzorců](#příloha-a-–-přehled-nejdůležitějších-vzorců)
    - [Signály, vzorkování, kvantizace (kap. 2–3)](#signály-vzorkování-kvantizace-kap-2–3)
    - [Systémy, konvoluce, korelace (kap. 4)](#systémy-konvoluce-korelace-kap-4)
    - [Fourierova transformace (kap. 5)](#fourierova-transformace-kap-5)
    - [Z-transformace (kap. 6)](#z-transformace-kap-6)
    - [Filtry (kap. 7–9)](#filtry-kap-7–9)
- [Příloha B – Opravy a upřesnění k slidům](#příloha-b-–-opravy-a-upřesnění-k-slidům)

---

# 1. Úvod do zpracování signálů

## 1.1 Co je číslicové zpracování signálů (DSP)

**DSP (Digital Signal Processing)** je matematická a algoritmická manipulace s číslicovými signály, jejímž cílem je **extrahovat důležité informace, které jsou signálem přenášeny**.

```
 Vstupní signál  ──►  [ DSP ]  ──►  Zpracovaný signál
```

Abychom tento obrázek pochopili, musíme odpovědět na tři otázky: *co je signál*, *co je systém* a *co je zpracování*.

---

## 1.2 Signál

> **Signál** je fyzikální veličina, která přenáší nějakou zprávu. Zpráva může obsahovat určité množství informace – kolik informace obsahuje, závisí na příjemci.

Signál lze reprezentovat jako **závislost jednoho parametru (závislá proměnná) na jiném parametru (nezávislá proměnná)**. Nejčastěji je nezávislou proměnnou **čas**, ale může to být cokoliv jiného (např. prostorová souřadnice u obrazu). Příklady signálů: EKG, řečový/audio záznam, EEG, obraz (fotografie), video, sinusovka, amplitudově modulovaný signál.

**Důležité souvislosti pojmu signál:**

- signály (fyzikální veličiny) během přenosu zprávy často **mění svoji formu** (např. akustický tlak → elektrické napětí → čísla v paměti),
- tentýž signál může u různých příjemců vyvolat **různou reakci**,
- **maximum informace** nese zpráva tehdy, když příjemce předem **neví**, co bude obsahovat (informace = odstranění neurčitosti),
- zprávu a informaci získáváme **sledováním vybraných parametrů signálu** (amplituda, frekvence, fáze, tvar…),
- signály využíváme zejména ke **zjištění stavu různých systémů** (živých i neživých) – např. EKG o stavu srdce, vibrace o stavu ložiska.

### Modely signálů

- Model používáme vždy, když se pokoušíme popsat něco reálného.
- Model je **popis reality zjednodušenými prostředky** (řadou čísel, matematickou funkcí, časovým průběhem apod.).
- Cílem modelování je vytvořit popis, který je **v daném čase a z hlediska našich potřeb a cílů dostatečný**.
- Model může být jednoduchý nebo složitý; **neexistuje univerzální a dokonalý model** – volba modelu se řídí účelem, ke kterému ho chceme použít.

### Typy signálů

| Kritérium | Rozdělení | Příklad |
|---|---|---|
| počet kanálů | **skalární** vs. **vektorové** | výstup měřiče krevního tlaku vs. 128kanálové EEG |
| předvídatelnost | **deterministický** vs. **náhodný** | výstup generátoru signálů vs. šum, EEG |
| počet nezávislých proměnných | **jednorozměrný** vs. **vícerozměrný** | řečový signál (čas) vs. obrázek (2 souřadnice) |
| spojitost | **spojitý** vs. **diskrétní** | viz tabulka níže |

Spojitost se posuzuje zvlášť v **čase** a zvlášť v **amplitudě**:

| | **spojitá amplituda** | **diskrétní amplituda** |
|---|---|---|
| **spojitý čas** | **analogový** signál | **kvantovaný** signál |
| **diskrétní čas** | **vzorkovaný** signál | **číslicový (digitální)** signál |

Signály, se kterými se setkáváme v běžném životě, jsou obvykle **spojité v čase i amplitudě – analogové**:

- elektrické: napětí, proud, elektrická a magnetická pole,
- mechanické: momenty, úhly, úhlové rychlosti,
- akustické: vibrace, zvukové vlny,
- fyzikální: tlaky, teploty, koncentrace.

> **DSP systémy zpracovávají signály, které jsou vzorkované, popř. kvantované.**
> Cesta: analogový → (vzorkování) → vzorkovaný → (kvantování) → digitální.

### Informace a šum

Každý signál přenáší **informaci**, ne každá informace je ale pro koncového uživatele užitečná. Cílem zpracování signálů je **extrahovat „užitečnou“ informaci** ze vstupního signálu.

> **Šum** je signál, který není užitečný. Co označíme jako šum, **závisí na aplikaci**.

- Posloucháme-li rozhovor dvou lidí, je šumem rozhovor dalších osob.
- Při vyšetřování **evokovaných potenciálů** jsou šumem artefakty (oči, svaly), ale **i vlastní EEG záznam** (evokovaná odezva je v něm „utopená“).
- Častým zdrojem rušení je **síťové napětí 50 Hz** (v USA 60 Hz) – typicky se odstraňuje pásmovou zádrží (notch filtrem). Na slidu je ukázka zašuměného EKG a téhož signálu po vyčištění.

---

## 1.3 Systém

> **Systém** je pravidlo, které přiřazuje signálu $x$ jiný signál $y$. V technice se pod pojmem systém obvykle rozumí fyzikální (obvodová, programová) realizace tohoto pravidla.

$$x \;\longrightarrow\; \boxed{\;\mathcal{T}\{\cdot\}\;} \;\longrightarrow\; y = \mathcal{T}\{x\}$$

**Typy systémů:**

- podle typu zpracovávaného signálu: **analogové** × **číslicové**,
- podle typu závislosti mezi vstupem a výstupem: **lineární** (speciálně **lineární časově invariantní – LTI**) × **nelineární**.

### Základní uspořádání DSP systémů

1. **Analogový signál → vzorkování → DSP → rekonstrukce → analogový signál** (např. číslicová filtrace audia),
2. **Analogový signál → vzorkování → DSP → informace o signálu** (např. měření tepové frekvence z EKG),
3. **Číslicový signál → DSP → rekonstrukce → analogový signál** (např. syntéza zvuku),
4. **Číslicový signál → DSP → číslicový signál** (např. komprese dat, zpracování uložených záznamů).

Úplný řetězec (podrobněji v kap. 2):

```
Analog. vstup → [Analog. filtr] → [ADC] → [DSP] → [DAC] → [Rekonstrukční filtr] → Analog. výstup
                 (antialiasing)                                (anti-image)
```

---

## 1.4 Příklady systémů pro zpracování signálů

### Komunikační technika – amplitudová modulace a demodulace

Modulace (AM) – užitečný signál $x(t)$ moduluje amplitudu nosné o úhlové frekvenci $\Omega_0$:

$$y(t) = A\,[1 + m\,x(t)]\cdot\cos(\Omega_0 t)$$

kde $m$ je hloubka modulace. Demodulace (synchronní) – přijatý signál násobíme znovu nosnou a filtrujeme dolní propustí (LPF):

$$r(t) = y(t)\cdot\cos(\Omega_0 t), \qquad \tilde{x}(t) = \mathrm{LPF}\{r(t)\}$$

*Proč to funguje:* $\cos^2(\Omega_0 t) = \tfrac12[1+\cos(2\Omega_0 t)]$, takže

$$r(t) = \frac{A}{2}[1 + m\,x(t)] + \frac{A}{2}[1+m\,x(t)]\cos(2\Omega_0 t).$$

Druhý člen leží kolem frekvence $2\Omega_0$ a dolní propust ho odstraní; zbude $\tfrac{A}{2}[1+m x(t)]$. Po odstranění stejnosměrné složky $\tfrac{A}{2}$ dostaneme $\tfrac{Am}{2}x(t)$ – původní signál (až na měřítko). Na slidech je vidět původní signál, modulovaný signál, signál posunutý do základního pásma (baseband) a demodulovaný signál.

### Filtrace

**Filtrace je nejčastěji používaná operace v DSP.** Cílem filtrace je:

- změna frekvenčního obsahu signálu, např. odstranění některých rušivých frekvencí,
- odstranění šumu v signálu,
- výběr pouze určitých frekvencí, které přenášejí informaci.

Základní typy filtrů:

| Filtr | Zkratka | Funkce |
|---|---|---|
| **Dolní propust** (lowpass) | LPF / DP | odstraňuje **vysoké** frekvence |
| **Horní propust** (highpass) | HPF / HP | odstraňuje **nízké** frekvence |
| **Pásmová propust** (bandpass) | BPF / PP | propouští frekvence z určitého pásma |
| **Pásmová zádrž** (bandstop) | BSF / PZ | potlačuje frekvence určitého pásma |
| **Notch filtr** (úzká zádrž) | – | odstraňuje jednu konkrétní frekvenci (např. 50 Hz) |

**Příklad ze slidů:** signál je součtem tří sinusovek 50 Hz, 110 Hz a 210 Hz (generováno v MATLABu: `x1=sin(2*pi*50*t); x2=sin(2*pi*110*t); x3=sin(2*pi*210*t); y=x1+x2+x3;`):

- dolní propust s mezní frekvencí **80 Hz** → na výstupu zbude jen **50 Hz**,
- horní propust s mezní frekvencí **150 Hz** → zbude jen **210 Hz**,
- pásmová propust **80–150 Hz** → zbude jen **110 Hz**,
- pásmová zádrž **80–150 Hz** → zbude **50 Hz + 210 Hz**.

### Tónová volba (DTMF – Dual-Tone Multi-Frequency)

Každá klávesa telefonu je kódována **součtem dvou sinusovek** – jedné z **dolní skupiny** (řádek) a jedné z **horní skupiny** (sloupec):

| | **1209 Hz** | **1336 Hz** | **1477 Hz** | **1633 Hz** |
|---|---|---|---|---|
| **697 Hz** | 1 | 2 | 3 | A |
| **770 Hz** | 4 | 5 | 6 | B |
| **852 Hz** | 7 | 8 | 9 | C |
| **941 Hz** | * | 0 | # | D |

Dekodér: signál se rozdělí **dolní propustí (do ~1000 Hz)** a **horní propustí (od ~1200 Hz)** na dolní (600–1000 Hz) a horní skupinu (1200–1700 Hz), každá větev projde omezovačem (limiter) a bankou **4 pásmových propustí** s detektory. Která dvojice detektorů „sepne“, ta určuje stisknutou klávesu.

---

## 1.5 Aplikační oblasti zpracování signálů

Zpracování signálů je **závislé na aplikační oblasti**:

- **Letectví a kosmonautika** – navigační systémy, úprava vesmírných fotografií, řízení sond.
- **Medicína** – diagnostika (CT, MRI, ultrazvuk), **analýza EEG/EKG**, ukládání a zpracování medicínských dat a snímků.
- **Komerce** – komprese zvuku, obrazu a videa, videoefekty, videokonference.
- **Telefonie** – komprese dat a hlasu, potlačení echa, multiplex signálů, filtrace.
- **Armáda** – analýza signálů radaru a sonaru, navigace raket.
- **Průmysl** – třídění a počítání výrobků, tribodiagnostika, řízení technologických procesů.
- **Věda** – simulace a modelování, monitorování zemětřesení, analýza snímků meteorologických družic.

---

## 1.6 Klasifikace metod zpracování signálů

1. **Podle linearity**
   - *lineární* – platí **princip superpozice**; lze je poměrně snadno matematicky modelovat a analyzovat,
   - *nelineární* – nelze je dobře matematicky charakterizovat a analyzovat; používají se zřídka.
2. **Podle setrvačnosti (paměti)**
   - *systémy bez paměti* – využívají jen **okamžité hodnoty vstupu** (funkční měniče, korektory nelinearit),
   - *systémy s pamětí* – obsahují **zpožďovací členy** (paměťové registry):
     - **nerekurzivní** – výstup počítají jen ze vstupních hodnot (i zpožděných) → **FIR filtry**,
     - **rekurzivní** – výstup počítají ze vstupních hodnot **i ze zpožděných výstupních hodnot** → **IIR filtry**.
3. **Podle počtu proměnných** – *jednorozměrné* (signál závisí na jediné proměnné, nejčastěji čase) × *vícerozměrné* (dvě a více proměnných, např. obraz).
4. **Podle proměnnosti operátoru v čase** – *časově invariantní* (běžné zpracování) × *časově proměnné* (např. **adaptivní filtrace**).
5. **Podle typu realizace** – *číslicové* (vzorky reprezentovány čísly, zpravidla binárně kódovanými) × *diskrétní analogové* (vzorky jsou analogové veličiny, ale výpočet probíhá v diskrétním čase – např. obvody se spínanými kapacitory).

---

## 1.7 Výhody a nevýhody číslicového zpracování

**Omezení (nevýhody):**

- **omezený kmitočtový rozsah** – číslicové zpracování je vhodné pro nižší frekvenční pásma; převodníky pro vysoké frekvence jsou drahé (použitelnost řádově do stovek MHz),
- **degradace signálu vlivem A/D převodu** (vzorkování, kvantizační šum),
- **cena zařízení**.

**Výhody:**

- **pružnost** – vlastnosti systému jsou dány konstantami (koeficienty) a programem, které lze snadno měnit,
- **časová stálost a reprodukovatelnost** – vlastnosti se nemění vlivem teploty, stárnutí součástek ani okolního prostředí; zpracování je dokonale reprodukovatelné,
- **časově neomezená paměť** – lze zpracovávat i velmi pomalé signály (analogově by to vyžadovalo objemné a drahé obvody),
- **odpadají problémy vzájemného ovlivňování** soustav (impedanční přizpůsobení, vliv elektromagnetického pole…),
- **spolehlivá realizace i velmi komplikovaných systémů**,
- **slučitelnost s informačními systémy**,
- **multiplexní provoz** – výkonný systém může současně zpracovávat více signálů.

---

### Kontrolní otázky ke kapitole 1

1. Jaký je rozdíl mezi analogovým, vzorkovaným, kvantovaným a číslicovým signálem?
2. Proč je pojem „šum“ závislý na aplikaci? Uveďte příklad z biomedicíny.
3. Jaký je rozdíl mezi rekurzivním a nerekurzivním systémem? Který odpovídá FIR a který IIR filtru?
4. Které frekvence obsahuje signál klávesy „5“ v tónové volbě?
5. Vysvětlete princip synchronní demodulace AM signálu.
6. Uveďte tři výhody a tři nevýhody číslicového zpracování signálů.

---

# 2. Analogové signály, vzorkování, kvantizace a rekonstrukce

## 2.1 Spojité (analogové) signály

- jsou obvykle **spojitou funkcí času** (popř. frekvence),
- lze je popsat **matematickým výrazem, graficky nebo tabulkou hodnot**,
- reálné signály se popisují špatně, proto se vytvářejí jejich **aproximace či modely** vhodné pro matematickou reprezentaci.

### Klasifikace spojitých signálů

| Typ | Definice |
|---|---|
| **konečný** (časově omezený) | nenulový jen na konečném intervalu |
| **nekonečný** | – |
| **levostranný** | $y(t) = 0$ pro $t > a$ |
| **pravostranný** | $y(t) = 0$ pro $t < a$ |
| **kauzální** | $y(t) = 0$ pro $t < 0$ |
| **nekauzální** | není kauzální |
| **periodický** | $x_p(t) = x_p(t \pm nT)$, $n \in \mathbb{Z}$ |

Periodické signály jsou **nekonečné** signály, které se po určité době (periodě $T$) opakují. Z toho plyne: **jednostranné ani časově omezené signály nemohou být periodické.**

---

## 2.2 Sinusoidy

Sinusoidy hrají ve zpracování signálů klíčovou roli, protože:

- lze je **jednoduše generovat**,
- snadno se s nimi pracuje – jejich **matematické vlastnosti jsou dobře známy**,
- **všechny ostatní signály lze vytvořit jako součet sinusoid** (Fourierovy řady, Fourierova transformace – kap. 5),
- **odezva lineárního (LTI) systému na harmonický vstup je opět harmonická se stejnou frekvencí** – mění se jen amplituda a fáze (tzv. *sinus fidelity*).

### Rovnice spojité sinusoidy

$$y(t) = A\cdot\sin(\Omega t - \Theta)$$

- $A$ – **amplituda**,
- $\Omega$ – **úhlová frekvence** [rad/s],
- $\Theta$ – **fáze** [rad].

**Vztahy mezi parametry** (frekvence $F$ v Hz = 1/s, perioda $T$ v s, úhlová frekvence $\Omega$ v rad/s):

$$F = \frac{\Omega}{2\pi},\qquad \Omega = 2\pi F,\qquad T = \frac{1}{F},\qquad \Omega = \frac{2\pi}{T}$$

- **Perioda** $T$ – doba, po které se signál znovu opakuje.
- **Úhlová frekvence** – míra rychlosti změny signálu, normalizovaná na interval $[0, 2\pi]$ (jedna perioda = $2\pi$ radiánů).
- **Fáze** – o kolik radiánů je sinusovka posunutá od počátku. Je-li posunutá o $t_\Theta$ sekund, pak
$$\Theta = 2\pi F t_\Theta = 2\pi\frac{t_\Theta}{T}.$$

**Vlastnosti spojité sinusoidy:**

- pro libovolné $F$ je $x_a(t)$ **periodická**: $x_a(t+T) = x_a(t)$, kde $T = 1/F$ je **základní perioda**,
- spojité sinusoidy s **různou frekvencí jsou vždy různé** (u diskrétních to neplatí – viz kap. 3!),
- s rostoucí frekvencí roste počet oscilací v daném časovém intervalu.

### Komplexní exponenciála a záporné frekvence

Sinusoidy úzce souvisí s **komplexními exponenciálními signály**:

$$x_a(t) = A\,e^{j(\Omega t + \theta)}, \qquad e^{\pm j\Phi} = \cos\Phi \pm j\sin\Phi \quad\text{(Eulerův vztah)}$$

Fyzikálně je frekvence kladná (počet cyklů za jednotku času). Matematicky se zavádí i **záporná frekvence**, protože reálnou sinusoidu lze zapsat jako součet **dvou komplexně sdružených fázorů** rotujících opačnými směry:

$$x_a(t) = A\cos(\Omega t + \Theta) = \frac{A}{2}e^{j(\Omega t + \Theta)} + \frac{A}{2}e^{-j(\Omega t + \Theta)}$$

Proto má reálný signál spektrum symetrické kolem nuly – každá reálná sinusoida o frekvenci $F$ má ve spektru dvě čáry: na $+F$ a $-F$, každou s amplitudou $A/2$.

### Vlastnosti sinu a kosinu

| Vlastnost | Vztah |
|---|---|
| sin a cos se liší jen fázovým posuvem 90° | $\sin\Theta = \cos\!\left(\Theta - \frac{\pi}{2}\right)$, $\cos\Theta = \sin\!\left(\Theta + \frac{\pi}{2}\right)$ |
| periodicita ($k \in \mathbb{Z}$) | $\cos(\Theta + 2\pi k) = \cos\Theta$ |
| kosinus je **sudá** funkce | $\cos(-\Theta) = \cos\Theta$ |
| sinus je **lichá** funkce | $\sin(-\Theta) = -\sin\Theta$ |
| nulové body sinu | $\sin(\pi k) = 0$ |
| jednotkové hodnoty kosinu | $\cos(2\pi k) = 1$ |
| hodnoty $-1$ kosinu | $\cos\!\left(2\pi\left(k + \tfrac12\right)\right) = -1$ |

### Užitečné goniometrické vztahy

$$\sin^2\alpha + \cos^2\alpha = 1$$
$$\cos^2\alpha = \frac{1 + \cos 2\alpha}{2}, \qquad \sin^2\alpha = \frac{1 - \cos 2\alpha}{2}$$
$$\cos 2\alpha = \cos^2\alpha - \sin^2\alpha, \qquad \sin 2\alpha = 2\sin\alpha\cos\alpha$$
$$\sin(\alpha \pm \beta) = \sin\alpha\cos\beta \pm \cos\alpha\sin\beta$$
$$\cos(\alpha \pm \beta) = \cos\alpha\cos\beta \mp \sin\alpha\sin\beta$$
$$\sin\alpha\sin\beta = \frac{\cos(\alpha-\beta)}{2} - \frac{\cos(\alpha+\beta)}{2}$$
$$\sin\alpha\cos\beta = \frac{\sin(\alpha+\beta)}{2} + \frac{\sin(\alpha-\beta)}{2}$$
$$\cos\alpha\cos\beta = \frac{\cos(\alpha+\beta)}{2} + \frac{\cos(\alpha-\beta)}{2}$$

> **Pozor – oprava slidu:** na slidu je u součinu $\sin\alpha\sin\beta$ znaménko „+“. Správně je $\sin\alpha\sin\beta = \tfrac12[\cos(\alpha-\beta) - \cos(\alpha+\beta)]$.

Tyto vztahy jsou důležité: **součin sinusovek vytváří součtové a rozdílové frekvence** (princip modulace, viz AM v kap. 1), a **mocnina sinusovky** vytváří vyšší harmonické (viz řešený příklad na konci kapitoly).

### Kombinace (součet) sinusoid

$$y(t) = x_1(t) + x_2(t) + \dots + x_n(t)$$

*Je kombinace periodická? Jaká je perioda?*

> **Společná perioda** součtu sinusovek je nejmenší doba, za kterou **každá** ze sinusovek dokončí **celistvý počet cyklů**:
> $$T = \frac{1}{f_0} = \mathrm{NSN}(T_1, T_2, \dots, T_n), \qquad f_0 = \mathrm{NSD}(f_1, f_2, \dots, f_n)$$
> (NSN – nejmenší společný násobek, NSD – největší společný dělitel). $f_0$ je **základní frekvence** součtu.

Součet je periodický **jen tehdy, když jsou poměry všech frekvencí racionální čísla** (jinak NSD neexistuje).

*Příklad:* $y(t) = \sin(2\pi\cdot 4t) + \cos(2\pi\cdot 6t)$: $f_0 = \mathrm{NSD}(4, 6) = 2\ \text{Hz}$, $T = 0{,}5\ \text{s}$. Naopak $\sin(2\pi t) + \sin(2\pi\sqrt{2}\,t)$ periodický **není** ($\sqrt2$ je iracionální).

---

## 2.3 Elementární spojité signály

### Exponenciální funkce
$$x(t) = e^{-\alpha t}, \qquad \alpha > 0 \quad\text{(klesající exponenciála)}$$

### Jednotkový skok
$$u(t) = \begin{cases} 0 & t < 0 \\ 1 & t > 0 \end{cases}$$
Pro $t = 0$ není definován; **Heavisideův jednotkový skok** definuje $u(0) = 0{,}5$.

### Lineární funkce (rampa, ramp)
$$r(t) = t\,u(t) = \begin{cases} 0 & t < 0 \\ t & t > 0 \end{cases}$$

### Obdélníková funkce (rect)
$$\Pi(t) \triangleq \begin{cases} 1 & -\tfrac12 \le t \le \tfrac12 \\ 0 & \text{jinde} \end{cases}
\qquad
\frac{1}{\beta}\Pi\!\left(\frac{t}{\beta}\right) \triangleq \begin{cases} 1/\beta & -\tfrac{\beta}{2} \le t \le \tfrac{\beta}{2} \\ 0 & \text{jinde} \end{cases}$$

Druhá funkce má šířku $\beta$, výšku $1/\beta$ a tedy **jednotkovou plochu** pro libovolné $\beta$.

### Jednotkový (Diracův) impuls

- je **limitním případem obdélníkové funkce** s jednotkovou plochou pro $\beta \to 0$,
- je to nekonečně vysoká, nekonečně úzká špička s **jednotkovou (konečnou) plochou**:

$$\delta(t) = \lim_{\beta\to 0}\frac{1}{\beta}\Pi\!\left(\frac{t}{\beta}\right), \qquad \int_{-\infty}^{\infty}\delta(t)\,dt = 1, \qquad \delta(t) = 0 \ \text{pro}\ t \neq 0$$

> Pozor: na slidu je v limitě překlep $\Pi(1/\beta)$; správně $\Pi(t/\beta)$.

**Vlastnosti Diracova impulsu:**

1. $f(t)\,\delta(t - t_0) = f(t_0)\,\delta(t - t_0)$ – součin s impulsem „vybere“ hodnotu funkce,
2. $\displaystyle\int_a^b f(t)\,\delta(t-t_0)\,dt = f(t_0)$ pro $a < t_0 < b$ – **vzorkovací (filtrační) vlastnost**,
3. $\displaystyle\int_a^b f(t)\,\delta^{(n)}(t-t_0)\,dt = (-1)^n f^{(n)}(t_0)$ pro $a < t_0 < b$,
4. $\delta(at) = \dfrac{1}{|a|}\,\delta(t)$ – změna měřítka,
5. $f(t) * \delta(t) = f(t)$, $\quad f(t) * \delta(t - t_0) = f(t - t_0)$ – impuls je **neutrální prvek konvoluce**, posunutý impuls signál posune,
6. $f(t) * \delta^{(n)}(t) = f^{(n)}(t)$.

> Na slidu je v bodě 5 zapsáno $f(t)*\delta(t_0) = f(t_0)$ – myšlena je ve skutečnosti vlastnost 2 (vzorkování); konvoluce s posunutým impulsem dává posunutý signál $f(t-t_0)$.

### Funkce sinc

$$\mathrm{sinc}(t) = \frac{\sin(\pi t)}{\pi t}$$

- tlumená oscilace,
- **maximum = 1 pro $t = 0$** (hodnota určena l'Hospitalovým pravidlem nebo aproximací $\sin a \approx a$ pro malé $a$),
- **prochází nulou pro $t = \pm1, \pm2, \pm3, \dots$**,
- plocha pod $\mathrm{sinc}(t)$ i pod $\mathrm{sinc}^2(t)$ je rovna ploše trojúhelníku ABC s vrcholy $(0,1)$, $(-1,0)$, $(1,0)$, tj. **rovna 1**.

Funkce sinc je Fourierovým obrazem obdélníku (a naopak) – proto se objevuje u ideální filtrace i rekonstrukce.

---

## 2.4 Vzorkování, kvantizace a rekonstrukce – celkový řetězec

```
Analog. vstup → [Analogový filtr] → [ADC] → [DSP] → [DAC] → [Rekonstrukční filtr] → Analog. výstup
                 frekv. omezený     číslicový   zpracovaný   výstupní
                 signál             signál      signál       signál
```

1. **Analogový (antialiasingový) filtr** frekvenčně omezí signál,
2. **ADC** signál vzorkuje (diskretizace času) a kvantuje (diskretizace amplitudy),
3. **DSP** provede vlastní zpracování,
4. **DAC** převede čísla zpět na napětí (typicky schodovitý průběh),
5. **rekonstrukční (anti-image) filtr** vyhladí výstup – odstraní spektrální repliky.

---

## 2.5 Vzorkování (sampling)

> **Vzorkování** je proces, při kterém je spojitý signál $x(t)$ transformován na posloupnost diskrétních vzorků $x(nT_s)$.

- $T_s$ – **perioda vzorkování** (vzorkovací interval), na slidech též $t_s$ či $T$,
- $f_s = \dfrac{1}{T_s}$ – **vzorkovací frekvence**, na slidech též $S$.

Vzorkovač si lze představit jako **spínač**, který se sepne každých $T_s$ sekund. Klíčová otázka: **jakou frekvenci $f_s$ zvolit?**

### Ideální vzorkování

Ideálně vzorkovaný signál je **vážený součet Diracových impulsů**, kde vahou je hodnota analogového signálu v okamžiku vzorkování. Matematicky jde o **násobení signálu posloupností impulsů** (vzorkovací funkcí) $i(t)$:

$$x_I(t) = x(t)\cdot i(t) = x(t)\sum_{n=-\infty}^{\infty}\delta(t - nt_s) = \sum_{n=-\infty}^{\infty}x(nt_s)\,\delta(t-nt_s) = \sum_{n=-\infty}^{\infty}x[n]\,\delta(t-nt_s)$$

(použita vlastnost 1 Diracova impulsu). Posloupnost $x[n] = x(nt_s)$ je výsledný **diskrétní signál**.

> **Vzorkování vede vždy ke ztrátě informace!** (Ztrácíme hodnoty mezi vzorky.) Vhodnou volbou vzorkovací frekvence lze tuto ztrátu minimalizovat – u frekvenčně omezených signálů dokonce odstranit.

### Spektrum vzorkovaného signálu

Volba $f_s$ vychází z rozložení spektra vzorkovaného signálu.

Posloupnost impulsů $i(t)$ je **periodický signál s periodou $t_s = 1/S$**. Jeho Fourierova řada má všechny koeficienty stejné, $I[k] = 1/t_s = S$, takže jeho **Fourierova transformace je opět posloupnost impulsů** v násobcích $S$:

$$I(f) = \sum_{k=-\infty}^{\infty} I[k]\,\delta(f - kS) = S\sum_{k=-\infty}^{\infty}\delta(f - kS)$$

Násobení v čase odpovídá **konvoluci ve frekvenci**, a konvoluce s posunutým impulsem signál posune. Proto:

$$\boxed{X_I(f) = X(f) * I(f) = S\sum_{k=-\infty}^{\infty} X(f - kS)}$$

> **Spektrum ideálně vzorkovaného signálu je periodické s periodou $S = f_s$.** Skládá se z kopií (replik, „spektrálních obrazů“) původního spektra $X(f)$ posunutých o celistvé násobky $f_s$ a vynásobených $S$.

Předpokládejme, že signál má spektrum omezené na interval $\langle -B, B\rangle$. Aby se repliky nepřekrývaly, musí horní okraj základní repliky ($B$) ležet pod dolním okrajem první posunuté repliky ($S - B$):

$$S - B > B \quad\Longrightarrow\quad S > 2B$$

**Tři případy volby vzorkovací frekvence:**

| Případ | Podmínka | Důsledek |
|---|---|---|
| **převzorkování** (oversampling) | $S > 2B$ | spektra se neovlivňují, rekonstrukce bez ztráty informace |
| **kritické vzorkování** | $S = 2B$ | spektra se ještě nepřekrývají, ale je to „na hraně“ – šum může způsobit problém |
| **podvzorkování** (undersampling) | $S < 2B$ | spektra se **překrývají** → **aliasing**, signál nelze správně rekonstruovat |

### Vzorkovací (Shannonova) věta

> **Vzorkovací věta (Shannon, Kotělnikov, Nyquist):** Je-li signál $x(t)$ spojitý v čase a obsahuje pouze složky s frekvencemi menšími než $f_{max}$ (je **frekvenčně omezený**), pak ho lze **přesně rekonstruovat** z posloupnosti vzorků $x(nT)$, pokud pro vzorkovací frekvenci $f_s = 1/T$ platí
> $$f_s \ge 2 f_{max} \quad (\text{přísně vzato } f_s > 2f_{max}).$$

- $f_N = 2 f_{max}$ – **Nyquistova frekvence, kritická vzorkovací frekvence** (angl. *Nyquist rate*),
- $t_N = \dfrac{1}{f_N} = \dfrac{1}{2B}$ – **Nyquistův interval, kritický vzorkovací interval**.

> **Pozor na terminologii:** na slidu je kritická frekvence zapsána jako $f_{max}/2$ – jde o překlep, správně $2f_{max}$. V části literatury (a v MATLABu) se jako „Nyquistova frekvence“ označuje **polovina vzorkovací frekvence $f_s/2$** – nejvyšší frekvence, kterou lze při dané $f_s$ jednoznačně zachytit (tzv. *folding frequency*). Oba pojmy spolu souvisí: signál je správně navzorkován, pokud $f_{max} < f_s/2$.

Proč přísná nerovnost: sinusovka o frekvenci přesně $f_s/2$ může být navzorkována třeba právě v průchodech nulou a pak jsou všechny vzorky nulové.

### Aliasing

> **Aliasing** je jev, ke kterému dochází, je-li signál vzorkován frekvencí menší, než je Nyquistova (kritická) frekvence. V rekonstruovaném signálu se pak **místo původní frekvence objeví jiná frekvence $f_{alias}$**, která výsledný signál poruší.

$$\boxed{f_{alias} = f - M\cdot S}$$

kde $M$ je celé číslo, které transformuje $f_{alias}$ do **základního frekvenčního pásma** $-0{,}5S < f_{alias} < 0{,}5S$.

Záporná hodnota $f_{alias}$ znamená, že se objeví frekvence $|f_{alias}|$ s **opačnou fází** (pro sinus: $\sin(-2\pi f t) = -\sin(2\pi f t)$).

**Příklad ze slidů ($f_s = 100$ Hz):** sinusovka 40 Hz je navzorkována správně ($40 < 50$). Sinusovka **90 Hz** dá vzorky, kterými lze proložit sinusovku **10 Hz** ($90 - 100 = -10$) – po rekonstrukci uslyšíme/uvidíme 10 Hz.

#### Řešený příklad – aliasing

*Na vstup vzorkovače přichází sinusoida o frekvenci $f_1 = 100$ Hz. Vzorkujte ji frekvencemi $f_s = 240, 140, 90, 35$ Hz. Kdy nastane aliasing a jaká bude frekvence $f_a$?*

Aliasing nastane, když $f_s < 2f_1 = 200$ Hz.

| $f_s$ [Hz] | základní pásmo | aliasing? | výpočet | $f_a$ |
|---|---|---|---|---|
| 240 | $(-120, 120)$ | **ne** | $100 - 0\cdot240$ | **100 Hz** |
| 140 | $(-70, 70)$ | ano | $100 - 1\cdot140 = -40$ | **40 Hz** (s obrácenou fází) |
| 90 | $(-45, 45)$ | ano | $100 - 1\cdot90 = 10$ | **10 Hz** |
| 35 | $(-17{,}5;\ 17{,}5)$ | ano | $100 - 3\cdot35 = -5$ | **5 Hz** (s obrácenou fází) |

---

## 2.6 Vzorkování v praxi – antialiasingový filtr

Většina reálných signálů **není frekvenčně přesně omezená** (obsahuje např. vysokofrekvenční šum). Proto se **před A/D převodník zařazuje antialiasingový filtr – analogová dolní propust**, která signál frekvenčně omezí na pásmo $\langle -B, B\rangle$.

> **Antialiasingový filtr je vždy analogový** (Butterworth, Čebyšev) – musí působit *před* vzorkováním, protože po navzorkování už aliasové složky nelze od užitečných odlišit.
> Filtr **nemá ideální (obdélníkovou) charakteristiku** → **nikdy nevolíme $f_s = 2f_c$, ale vždy vyšší!**

Řetězec: *analogový filtr → vzorkovač s pamětí (sample & hold) → ADC kódování → číslicová hodnota*.

### Butterworthův filtr

Amplitudová frekvenční charakteristika Butterworthova filtru (dolní propust) řádu $n$ s mezní frekvencí $f_c$:

$$|H(f)| = \frac{1}{\sqrt{1 + \left(\dfrac{f}{f_c}\right)^{2n}}}$$

V $f = f_c$ je $|H| = 1/\sqrt2 \approx 0{,}707$ (pokles o 3 dB) pro libovolný řád; čím vyšší řád, tím strmější přechod.

**Antialiasingový filtr 2. řádu** (zapojení Sallen–Key s operačním zesilovačem jako sledovačem): zvolíme $C_2$ a dopočítáme

$$R_1 = R_2 = \frac{1{,}4142}{C_2\,2\pi f_c}, \qquad C_1 = \frac{1}{R_1R_2C_2(2\pi f_c)^2}.$$

### Úroveň aliasingového šumu

Uvažujme nejhorší případ – vstupní signál s plochým spektrem. Neideální filtr propustí i složky nad $f_s/2$; složka na frekvenci $f_s - f_a$ se po vzorkování „zrcadlí“ (přeloží) na frekvenci $f_a$. **Úroveň aliasingového šumu** na frekvenci $f_a$ je poměr velikosti tohoto obrazu k velikosti užitečné složky:

$$\text{anl}\,[\%] = \frac{X_a}{X(f)\big|_{f=f_a}} = \frac{|H(f)|_{f = f_s - f_a}}{|H(f)|_{f = f_a}} = \frac{\sqrt{1 + \left(\dfrac{f_a}{f_c}\right)^{2n}}}{\sqrt{1 + \left(\dfrac{f_s - f_a}{f_c}\right)^{2n}}}$$

#### Řešený příklad 1

*Butterworthova DP 2. řádu, $f_c = 3{,}4$ kHz, $f_s = 8000$ Hz.*

**a) Úroveň aliasingového šumu na mezní frekvenci $f_a = f_c$:**
$$\text{anl} = \frac{\sqrt{1 + 1^4}}{\sqrt{1 + \left(\frac{8000 - 3400}{3400}\right)^4}} = \frac{\sqrt2}{\sqrt{1 + 1{,}353^4}} = \frac{1{,}414}{\sqrt{4{,}351}} = \frac{1{,}414}{2{,}086} \approx 0{,}678 = \mathbf{67{,}8\,\%}$$

**b) Na frekvenci $f_a = 1000$ Hz:**
$$\text{anl} = \frac{\sqrt{1 + (1000/3400)^4}}{\sqrt{1 + (7000/3400)^4}} = \frac{\sqrt{1{,}0075}}{\sqrt{1 + 17{,}97}} = \frac{1{,}0037}{4{,}355} \approx \mathbf{23{,}0\,\%}$$

**c) Případ a) při $f_s = 16\,000$ Hz:**
$$\text{anl} = \frac{\sqrt2}{\sqrt{1 + (12\,600/3400)^4}} = \frac{1{,}414}{\sqrt{189{,}6}} = \frac{1{,}414}{13{,}77} \approx \mathbf{10{,}3\,\%}$$

Zdvojnásobení vzorkovací frekvence (převzorkování) výrazně snížilo aliasing.

#### Řešený příklad 2

*Butterworthova DP, $f_c = 8$ kHz, $f_s = 40$ kHz. Jaký musí být řád filtru, aby úroveň aliasingového šumu (na $f_c$) klesla pod 1 %?*

$$\frac{\sqrt{2}}{\sqrt{1 + \left(\frac{40 - 8}{8}\right)^{2n}}} < 0{,}01 \;\Rightarrow\; 1 + 4^{2n} > \frac{2}{0{,}0001} = 20\,000 \;\Rightarrow\; 2n > \frac{\ln 19\,999}{\ln 4} = 7{,}14 \;\Rightarrow\; n > 3{,}57$$

Je potřeba filtr **4. řádu** ($n = 4$).

---

## 2.7 Vzorkování pásmových signálů (bandpass sampling)

Mějme signál omezený na pásmo $B = f_H - f_L$, kde $f_L \neq 0$ je nejnižší a $f_H$ nejvyšší frekvence v signálu. Je nutné vzorkovat podle Nyquistovy věty $S \ge 2f_H$?

Pokud $B \ll f_H$, **není**. Lze **využít aliasing** záměrně – posunuté repliky spektra se „vejdou“ do prázdných mezer mezi pásmy a nepřekryjí se. Lze tedy vzorkovat frekvencí nižší než $2f_H$.

**Nejnižší možná vzorkovací frekvence:**
$$S = \frac{2f_H}{N}, \qquad N = \mathrm{int}\!\left(\frac{f_H}{B}\right)$$

**Obecně přípustné rozsahy vzorkovací frekvence:**
$$\frac{2f_H}{k} \le S \le \frac{2f_L}{k-1}, \qquad k = 1, 2, \dots, N$$

- pro $k = N$ → nejnižší vzorkovací frekvence,
- pro $k = 1$ → vzorkování podle Nyquistovy věty ($S \ge 2f_H$, horní mez je $\infty$).

#### Řešený příklad

*Signál $x_c(t)$: $f_H = 6$ kHz, $f_L = 4$ kHz, $B = 2$ kHz.*

$N = \mathrm{int}(6/2) = 3$, $\quad \dfrac{2\cdot6}{k} \le S \le \dfrac{2\cdot4}{k-1}$, $k = 1, 2, 3$:

| $k$ | $S$ | poznámka |
|---|---|---|
| 3 | 4 kHz | nejmenší použitelná vzorkovací frekvence |
| 2 | 6 kHz ≤ S ≤ 8 kHz | (např. 7 kHz) |
| 1 | ≥ 12 kHz | splňuje Nyquistovu větu |

Pozor – mezi těmito intervaly jsou „zakázané“ frekvence (např. $S = 5$ kHz nebo $S = 10$ kHz), při nichž by se repliky překryly.

---

## 2.8 Druhy vzorkování

### Ideální vzorkování
Násobení posloupností Diracových impulsů (viz výše) – teoretický model; spektrum = periodicky opakované $X(f)$ s konstantní vahou $S$.

### Přirozené vzorkování
- signál prochází spínačem, který se zapíná každých $t_s$ sekund na dobu $t_d$,
- spínač je modelován **periodickým pulsem $p(t)$ jednotkové výšky a šířky $t_d$** s periodou $t_s$,
- vzorkovaný signál $x_N(t) = x(t)\,p(t)$ je po dobu $t_d$ roven $x(t)$ (vrchol pulsu kopíruje průběh signálu).

Spektrum: $X_N(f) = X(f) * P(f) = \sum_k P[k]\,X(f - kS)$, kde $P[k]$ jsou koeficienty Fourierovy řady obdélníkového pulsu ($P[k] = \frac{t_d}{t_s}\mathrm{sinc}\!\left(k\frac{t_d}{t_s}\right)$). **Repliky mají nezkreslený tvar**, ale různou velikost (váhy $P[k]$ s obálkou sinc).

### Vzorkování s pamětí (Zero-Order Hold, ZOH, sample & hold)
- **běžně používané v praxi**,
- ZOH vzorkovač obsahuje **paměť, která podrží hodnotu signálu až do příchodu dalšího vzorku** → schodovitý průběh,
- matematicky: ideální vzorkování následované obvodem s impulsní odezvou $h(t)$ = obdélník šířky $t_s$: $x_{ZOH}(t) = x_I(t) * h(t)$.

Spektrum ideálně vzorkovaného signálu se násobí $H(f) = t_s\,\mathrm{sinc}(f t_s)\,e^{-j\pi f t_s}$ → repliky jsou **zkreslené sinc obálkou** (tzv. sinc zkreslení), viz také rekonstrukce v části 2.11.

---

## 2.9 A/D převodníky

Tři základní typy: **paralelní (flash)**, **s postupnou aproximací (SAR)** a **sigma-delta**.

### Flash (paralelní) A/D převodník

**Princip:** vstupní napětí $V_{in}$ se současně porovnává **komparátory** s referenčními napětími z **odporového děliče** ($V_R/4, V_R/2, 3V_R/4$ pro 2 bity). Výstupy komparátorů (tzv. teploměrový kód) se zakódují do binární podoby.

- ➕ **velmi rychlý** převod (probíhá paralelně v jednom kroku),
- ➖ **vyšší spotřeba**,
- ➖ **drahý** – rezistory děliče musí být přesné; pro $n$-bitový převod je potřeba **$2^n$ přesných rezistorů a $2^n - 1$ komparátorů** (pro 8 bitů už 255 komparátorů).

### Převodník s postupnou aproximací (SAR)

Skládá se ze tří bloků:
- **SAR** (successive approximation register) – registr postupné aproximace,
- **komparátor**,
- **D/A převodník** (v zpětné vazbě).

**Princip (binární vyhledávání):** Nejprve jsou všechny bity SAR nastaveny na 0. Počínaje **nejvyšším bitem (MSB)** se postupně jednotlivé bity nastavují na 1, provede se D/A převod a výstupní hodnota $x_q$ se porovná se vstupním napětím. Je-li vstupní napětí **vyšší**, bit zůstane nastaven, jinak se vynuluje, a pokračuje se dalším bitem. **Po $n$ cyklech** je převod ukončen a v SAR je výsledek.

#### Řešený příklad – SAR

*10bitový ADC, $V_{ref} = 1$ V, $V_{in} = 0{,}6$ V.* Váhy bitů: 0,5; 0,25; 0,125; 0,0625; 0,03125; 0,015625; 0,0078125; 0,00390625; 0,001953125; 0,0009765625 V.

| krok | zkoušený bit | testované napětí [V] | $V_{in} \ge$ ? | stav registru |
|---|---|---|---|---|
| 1 | b9 | 0,5 | ano | 1 |
| 2 | b8 | 0,75 | ne | 10 |
| 3 | b7 | 0,625 | ne | 100 |
| 4 | b6 | 0,5625 | ano | 1001 |
| 5 | b5 | 0,59375 | ano | 10011 |
| 6 | b4 | 0,609375 | ne | 100110 |
| 7 | b3 | 0,6015625 | ne | 1001100 |
| 8 | b2 | 0,59765625 | ano | 10011001 |
| 9 | b1 | 0,599609375 | ano | 100110011 |
| 10 | b0 | 0,6005859375 | ne | **1001100110** |

Výsledek: kód $1001100110_2 = 614$, odpovídající napětí $0{,}5996$ V ($614/1024$).

### Sigma-delta převodník

- moderní typ, dosahuje **vysokého rozlišení (16–24 bitů) za nízkou cenu**,
- **jednoduchý hardware** – jádrem je **integrátor a komparátor** (1bitový ADC) se zpětnou vazbou přes 1bitový DAC; pracuje s mnohonásobně vyšší frekvencí ($K f_s$, převzorkování) a generuje **sled pulzů (1bitový datový tok), jejichž hustota (střední hodnota počtu jedniček) odpovídá vstupnímu napětí**,
- následuje **číslicový filtr a decimátor**, který z bitového toku vytvoří $N$-bitová slova s frekvencí $f_s$,
- lze ho chápat i jako **převodník napětí–frekvence následovaný čítačem**.

Příklad průběhů: pro $V_{in} = 0$ je výstup komparátoru 0101 0101… (hustota 4/8), pro $V_{in} = +V_{ref}/2$ je výstup 0111 0111… (hustota 6/8 = 3/4).

- ➕ vysoké rozlišení, **nepotřebuje přesné součástky**,
- ➖ vhodný pouze pro **malou šířku pásma** (nízké frekvence – audio, měření).

### Porovnání převodníků

| Typ ADC | Cena | Rozlišení | Rychlost |
|---|---|---|---|
| Flash | vysoká | 4–12 bitů | vysoká |
| Postupná aproximace | nízká | 8–16 bitů | středně rychlé (do ~5 MSps) |
| Sigma-delta | nízká | 12–24 bitů | nízká |

*Příklad z praxe:* mikrokontrolér **STM32F4 (ARM)** má převodník **s postupnou aproximací**, rozlišení lze nastavit na 6, 8, 10 nebo 12 bitů.

---

## 2.10 Kvantizace

> **Kvantizace** je proces, při kterém je vzorkovaný signál s „nekonečnou“ přesností převeden na signál s **konečnou přesností** (obvykle binární čísla).

- např. **3bitový kvantizátor** převádí hodnoty vstupního rozsahu jen do **8 výstupních hodnot**,
- **při kvantizaci dochází ke ztrátě přesnosti** (nevratně),
- kvantizátor bývá součástí A/D převodníku.

**Typy kvantizátorů:**
- **unipolární** – převádí hodnoty z intervalu $\langle 0, x_{max}\rangle$,
- **bipolární** – převádí hodnoty z intervalu $\langle -x_1, x_2\rangle$ (kladné i záporné).

**Základní vztahy** ($m$ = počet bitů):

| Veličina | Vztah |
|---|---|
| počet kvantizačních úrovní | $L = 2^m$ |
| kvantizační krok (rozlišení převodníku) | $\Delta = \dfrac{x_{max} - x_{min}}{L}$ |
| index úrovně | $i = \mathrm{round}\!\left(\dfrac{x - x_{min}}{\Delta}\right)$ |
| kvantizační úroveň | $x_q = x_{min} + i\Delta,\quad i = 0, 1, \dots, L-1$ |
| kvantizační chyba | $e_q = x_q - x,\quad -\tfrac{\Delta}{2} \le e_q \le \tfrac{\Delta}{2}$ |

(Pro vstup na samém horním konci rozsahu by vyšlo $i = L$, prakticky se omezí na $L-1$ – nastává přetížení.)

**3bitový unipolární kvantizátor:** kódy 000–111 odpovídají úrovním $0, \Delta, 2\Delta, \dots, 7\Delta$. Převodní charakteristika je schodovitá; chyba $e_q$ má pilovitý průběh mezi $-\Delta/2$ a $+\Delta/2$ (nad $7{,}5\Delta$ už chyba roste – přetížení).

**3bitový bipolární kvantizátor:** kódy 000–111 odpovídají úrovním $-4\Delta, -3\Delta, \dots, 0, \dots, 3\Delta$ (kód 100 ↔ 0, tzv. offsetový binární kód).

#### Ilustrační příklad

*3bitový ADC s rozsahem 0–5 V, vstup 3,2 V:* $L = 8$, $\Delta = 5/8 = 0{,}625$ V, $i = \mathrm{round}(3{,}2/0{,}625) = \mathrm{round}(5{,}12) = 5$, $x_q = 5\cdot0{,}625 = 3{,}125$ V, kód **101**, chyba $e_q = 3{,}125 - 3{,}2 = -0{,}075$ V.

### Kvantizační šum a SNR

Chyba kvantizace se v praxi vyjadřuje parametrem **SNR (signal-to-noise ratio)** – poměrem výkonu signálu k výkonu šumu vzniklého kvantizací:

$$\mathrm{SNR} = \frac{E(x^2)}{E(e_q^2)}, \qquad \mathrm{SNR}_{dB} = 10\log_{10}(\mathrm{SNR})\ [\text{dB}]$$

**Výkon kvantizačního šumu.** Předpokládáme, že chyba $e_q$ má **rovnoměrné rozdělení** na intervalu $\langle -\Delta/2, \Delta/2\rangle$ (hustota $1/\Delta$). Pak

$$E(e_q^2) = \int_{-\Delta/2}^{\Delta/2} e^2\,\frac{1}{\Delta}\,de = \frac{1}{\Delta}\left[\frac{e^3}{3}\right]_{-\Delta/2}^{\Delta/2} = \frac{\Delta^2}{12}.$$

> **Oprava slidu:** na slidu je uvedeno $E(e_q^2) = \Delta/12$; správně je $\boxed{E(e_q^2) = \Delta^2/12}$ (jen tak vychází i následující vzorec).

Dosazením $E(x^2) = x_{rms}^2$:

$$\mathrm{SNR}_{dB} = 10\log_{10}\frac{x_{rms}^2}{\Delta^2/12} = 10\log_{10}12 + 20\log_{10}\frac{x_{rms}}{\Delta} = \boxed{10{,}79 + 20\log_{10}\!\left(\frac{x_{rms}}{\Delta}\right)}$$

**Praktický vzorec** (z naměřených vzorků signálu a chyby):

$$\mathrm{SNR} = \frac{\frac1N\sum_{n=0}^{N-1}x^2(n)}{\frac1N\sum_{n=0}^{N-1}e_q^2(n)} = \frac{\sum_{n=0}^{N-1}x^2(n)}{\sum_{n=0}^{N-1}e_q^2(n)}$$

**SNR pro sinusoidu s amplitudou $A$ kvantovanou do $m$ bitů** (sinusovka využívá celý rozsah):

$x_{max} - x_{min} = 2A \Rightarrow \Delta = \dfrac{2A}{2^m}$, $\quad x_{rms} = \dfrac{A}{\sqrt2}$:

$$\mathrm{SNR} = \frac{A^2/2}{\Delta^2/12} = \frac{6A^2\cdot 2^{2m}}{4A^2} = 1{,}5\cdot 2^{2m}
\quad\Rightarrow\quad \boxed{\mathrm{SNR}_{dB} = 1{,}76 + 6{,}02\,m\ [\text{dB}]}$$

> **Každý bit navíc zlepší SNR o cca 6 dB.** (Např. 16bitový audio převodník: $1{,}76 + 96{,}3 \approx 98$ dB.)

---

## 2.11 Rekonstrukce signálu (D/A převod)

Při rekonstrukci číslicového signálu zpět na analogový je nutné **doplnit chybějící část signálu – interpolovat signál mezi vzorky**.

### Ideální rekonstrukce – sinc interpolace

Vzorkovaný signál lze vyjádřit jako $x_I(t) = \sum_n x[n]\,\delta(t - nt_s)$, kde $x[n]$ jsou vzorky diskrétního signálu.

Jeho spektrum obsahuje repliky $S\cdot X(f - kS)$. Chceme-li dostat zpět pouze $X(f)$, stačí signál přivést na **ideální dolní propust s mezní frekvencí $0{,}5S$ a zesílením $1/S$**:

$$H(f) = \begin{cases} 1/S & |f| < 0{,}5S \\ 0 & \text{jinde}\end{cases}
\quad\Longleftrightarrow\quad h(t) = \mathrm{sinc}\!\left(\frac{t}{t_s}\right)$$

V časové oblasti (konvoluce s posunutými impulsy = součet posunutých impulsních odezev):

$$\boxed{x(t) = x_I(t) * h(t) = \sum_{n=-\infty}^{\infty} x[n]\,h(t - nt_s) = \sum_{n=-\infty}^{\infty} x[n]\,\mathrm{sinc}\!\left(\frac{t - nt_s}{t_s}\right)}$$

> Analogový signál je **vážený součet posunutých funkcí sinc**, kde váhami jsou hodnoty vzorků. (Na slidu ukázka rekonstrukce z $x[n] = [1, 2, 3, 4]$.) V okamžicích vzorkování $t = kt_s$ má vždy jen jedna sinc funkce hodnotu 1 a ostatní 0 – rekonstrukce prochází přesně vzorky.

**Ideální rekonstrukce je v praxi nepoužitelná** – funkce sinc **není kauzální** (je nenulová i pro $t < 0$) a je nekonečně dlouhá. Kauzální systém aktivovaný v čase $t = 0$ produkuje odezvu začínající v $t = 0$; ideální rekonstruktor by musel odpovídat dříve, než je aktivován – to v reálných systémech nelze.

### Schodovitá interpolace (staircase, ZOH)

- hodnota rekonstruovaného signálu je **rovna předchozímu vzorku** (drží se do dalšího vzorku),
- interpolační funkce je pravoúhlý impuls šířky $t_s$: $h(t) = \mathrm{rect}\!\left[\dfrac{t - 0{,}5t_s}{t_s}\right]$,
- ➕ rychlá rekonstrukce, vhodná pro **on-line a real-time** systémy (je kauzální),
- ➕ přesná rekonstrukce pro signály, které jsou **po částech konstantní**.

Frekvenční charakteristika ZOH: $|H_{ZOH}(f)| = t_s\,|\mathrm{sinc}(f t_s)|$ – oproti ideálnímu obdélníku na $\pm f_s/2$:
- **částečně utlumí i základní repliku** (u $f_s/2$ asi o 3,9 dB),
- **nepotlačí úplně vyšší repliky** (přežívající repliky, *surviving replicas*).

> Rekonstruovaný signál neobsahuje jen základní pásmo, ale i části frekvenčních replik → **repliky je nutné odfiltrovat tzv. anti-image (rekonstrukčním) filtrem** – analogovou dolní propustí za DAC.

### Lineární interpolace

Hodnoty mezi vzorky se určí lineární aproximací (spojení vzorků úsečkami). Interpolační funkce je trojúhelník $h(t) = \mathrm{tri}(t/t_s)$ (šířka $2t_s$):

$$\tilde{x}(t) = x[n] + (t - nt_s)\,\frac{x[n+1] - x[n]}{t_s}, \qquad nt_s \le t < (n+1)t_s$$

Spektrum $\propto \mathrm{sinc}^2(f t_s)$ – repliky jsou potlačeny lépe než u ZOH. (Potřebuje však znát *následující* vzorek, tj. v reálném čase zavádí zpoždění o jeden vzorek.)

### Kosinová interpolační funkce (raised cosine)

$$h_{rc}(t) = \frac{\cos\!\left(\pi R\,t/t_s\right)}{1 - \left(2R\,t/t_s\right)^2}\,\mathrm{sinc}\!\left(\frac{t}{t_s}\right), \qquad 0 \le R \le 1$$

Pro $R = 0$ přechází v ideální sinc. Pro $R > 0$ klesá rychleji než sinc (lze ji zkrátit), za cenu širšího přechodového pásma.

---

## 2.12 D/A převodníky

Zajišťují **převod vstupní číslicové informace (datového slova $b_{n-1} \dots b_0$) na výstupní analogový signál** (obvykle napětí), s využitím referenčního napětí.

**Typy:**
- **přímé** – datové slovo je přímo převedeno na výstupní napětí (proud); realizují se **příčkovou (R–2R) nebo váhovou strukturou odporové sítě**,
- **nepřímé** – převod ve dvou krocích: číslo → pomocný diskrétní signál (šířka pulzu, počet pulzů za jednotku času…) → analogový signál.

### D/A převodník s váhovou strukturou odporové sítě

**Princip:** bity vstupního slova ovládají přepínače, které mají v sérii rezistory s hodnotami $R, 2R, 4R, \dots$ (každá další je **dvojnásobkem předchozí**). Rezistory jsou připojeny k invertujícímu vstupu operačního zesilovače se zpětnovazebním rezistorem $R_2$; spínáním se mění přenos zesilovače:

$$U_{v\acute{y}st} = -U_{ref}\,\frac{R_2}{R}\sum_{i=0}^{n}\frac{B_i}{2^i}$$

($B_0$ je MSB připojený přes $R$, $B_1$ přes $2R$ atd.). Nevýhoda: potřeba mnoha přesných rezistorů s velmi rozdílnými hodnotami – proto se častěji používá příčková síť **R–2R**, která vystačí se dvěma hodnotami odporů.

### Nepřímé D/A převodníky

Pomocnou veličinou je **impulz**:

- **PWM (Pulse Width Modulation)** – měronosnou veličinou je **šířka impulzu** konstantní amplitudy (nebo poměr šířky pulzu k době převodu – střída),
- **PDM (Pulse Density Modulation)** – měronosnou veličinou je **počet impulzů** konstantní šířky a amplitudy během doby převodu.

**Princip PWM převodníku:** vstupní datové slovo slouží jako předvolba **zpětného čítače** taktovaného generátorem hodinových impulzů. Při průchodu nulou se překlopí **RS klopný obvod**, čímž se zastaví odečítání. Po uplynutí doby převodu (určené druhým $n$-bitovým čítačem do $2^n$) se klopný obvod překlopí zpět do jednotkového stavu a převod se opakuje. Výstupní impulzy jsou převedeny na analogový signál **filtrem (dolní propustí)** – výstupní napětí je úměrné střední hodnotě, tj. šířce pulzu.

---

## 2.13 Řešený příklad – určení vzorkovací periody

*Uvažujme pásmově omezený signál $x_a(t) = \sin(4\pi t)\,[1 + \cos^2(2\pi t)]$.*
*a) Pomocí goniometrických vztahů určete maximální frekvenci v signálu. b) Určete periodu vzorkování $T$, aby bylo možné signál ze vzorků správně rekonstruovat.*

**a)** $\cos^2(2\pi t) = \frac12 + \frac12\cos(4\pi t)$, takže

$$x_a(t) = \sin(4\pi t)\left[\tfrac32 + \tfrac12\cos(4\pi t)\right] = \tfrac32\sin(4\pi t) + \tfrac12\underbrace{\sin(4\pi t)\cos(4\pi t)}_{\frac12\sin(8\pi t)}$$

$$x_a(t) = \tfrac32\sin(2\pi\cdot 2t) + \tfrac14\sin(2\pi\cdot 4t)$$

Signál obsahuje frekvence 2 Hz a 4 Hz → $f_{max} = 4$ Hz.

**b)** $f_s > 2f_{max} = 8$ Hz $\Rightarrow$ $T < 1/8 = 0{,}125$ s.

---

## Shrnutí kapitoly 2

- Sinusoida $A\sin(\Omega t - \Theta)$; $\Omega = 2\pi F = 2\pi/T$; reálná sinusoida = dva fázory na $\pm F$.
- Součet sinusoid je periodický, jsou-li poměry frekvencí racionální; $f_0 = \mathrm{NSD}(f_i)$.
- Elementární signály: $u(t)$, $r(t)$, $\Pi(t)$, $\delta(t)$ (vzorkovací vlastnost!), $\mathrm{sinc}(t)$.
- Ideální vzorkování = násobení hřebenem impulsů → **spektrum se periodizuje s periodou $f_s$**: $X_I(f) = S\sum_k X(f-kS)$.
- **Shannonova věta:** $f_s > 2f_{max}$; jinak **aliasing** $f_{alias} = f - MS$.
- Antialiasingový filtr je analogová DP před ADC; vždy $f_s > 2f_c$ s rezervou; úroveň aliasingu $|H(f_s-f_a)|/|H(f_a)|$.
- Pásmové signály lze vzorkovat pod $2f_H$: $\frac{2f_H}{k} \le S \le \frac{2f_L}{k-1}$.
- ADC: flash (rychlý, drahý), SAR ($n$ kroků, binární hledání), sigma-delta (přesný, pomalý).
- Kvantizace: $L = 2^m$, $\Delta = (x_{max}-x_{min})/L$, šum $\Delta^2/12$, $\mathrm{SNR} = 1{,}76 + 6{,}02m$ dB.
- Rekonstrukce: ideální sinc (nekauzální), ZOH (sinc zkreslení, repliky → anti-image filtr), lineární, raised cosine.

### Kontrolní otázky
1. Odvoďte spektrum ideálně vzorkovaného signálu. Proč je periodické?
2. Formulujte vzorkovací větu. Co je aliasing a jak se mu brání?
3. Proč musí být antialiasingový filtr analogový? Proč nevolíme $f_s = 2f_c$?
4. Jakou nejnižší vzorkovací frekvencí lze vzorkovat signál v pásmu 20–25 kHz?
   *(Řešení: $B = 5$ kHz, $N = \mathrm{int}(25/5) = 5$, $S = 2\cdot25/5 = 10$ kHz.)*
5. Popište činnost SAR převodníku. Kolik kroků potřebuje 12bitový převod?
6. Odvoďte výkon kvantizačního šumu a vztah $\mathrm{SNR}_{dB} = 1{,}76 + 6{,}02m$.
7. Proč je ideální sinc rekonstrukce nerealizovatelná? Jaké má nevýhody ZOH?

---

# 3. Diskrétní signály (číselné řady), jejich vlastnosti a základní operace; náhodné signály

## 3.1 Diskrétní signál

> **Diskrétní signál** $x[n]$ je funkcí **celočíselné** nezávislé proměnné $n$ (index vzorku).
>
> **Pozor!** Signál **není definován mezi dvěma po sobě jdoucími vzorky** – a **není tam ani nulový**. Hodnota „$x[1{,}5]$“ prostě neexistuje.

Diskrétní signál (posloupnost vzorků) lze zadat:

- **jako funkci**, např. $x[n] = \begin{cases} 1 & n = 1, 3 \\ 4 & n = 2 \\ 0 & \text{jinak} \end{cases}$
- **tabulkou hodnot**:

| $n$ | … | −2 | −1 | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|---|---|---|
| $x[n]$ | … | 0 | 0 | 0 | 1 | 4 | 1 | 0 | 0 |

- **jako posloupnost** $x[n] = \{\dots, 0, 0, \underset{\uparrow}{0}, 1, 4, 1, 0, 0\}$ – šipka (na slidech v této kapitole též **červená** nebo podtržená hodnota) označuje vzorek s indexem $n = 0$,
- **jako posloupnost konečné délky** $x[n] = \{3, -1, -2, 5, \underset{\uparrow}{0}, 4, 1, -1\}$.

> **Označení frekvencí (důležité pro celý zbytek skript).** Od této kapitoly používáme značení podle Ambardara:
>
> | Symbol | Význam | Jednotka |
> |---|---|---|
> | $f$ | analogová frekvence | Hz |
> | $\omega = 2\pi f$ | analogová úhlová frekvence | rad/s |
> | $S = f_s = 1/t_s$ | vzorkovací frekvence | Hz |
> | $F = f/S$ | **číslicová (normalizovaná) frekvence** | cykly/vzorek |
> | $\Omega = 2\pi F = \omega t_s$ | **číslicová úhlová frekvence** | rad/vzorek |
>
> (V kapitole 2 bylo $\Omega$ použito pro analogovou úhlovou frekvenci – tak jsou psány slidy 2. přednášky. Od teď je $\Omega$ vždy **číslicová** úhlová frekvence.)

---

## 3.2 Charakteristiky diskrétního signálu

| Charakteristika | Definice |
|---|---|
| diskrétní součet | $S_D = \sum_{n=-\infty}^{\infty} x[n]$ |
| absolutní součet | $S_A = \sum_{n=-\infty}^{\infty} \lvert x[n]\rvert$ |
| kumulativní součet | $S_C[n] = \sum_{k=-\infty}^{n} x[k]$ (diskrétní obdoba integrálu) |
| energie | $E = \sum_{n=-\infty}^{\infty} \lvert x[n]\rvert^2 = \sum_{n=-\infty}^{\infty} p[n]$ |

**Energie** neperiodického signálu je definována jako součet okamžitých výkonů $p[n] = |x[n]|^2$. Pro **periodické signály je energie nekonečná** a neposkytuje žádnou užitečnou informaci – proto se počítají **průměrné hodnoty v rámci jedné periody**:

$$x_{av} = \frac{1}{N}\sum_{m=0}^{N-1}x[m] \quad\text{(průměrná hodnota)}, \qquad P = \frac{1}{N}\sum_{m=0}^{N-1}|x[m]|^2 \quad\text{(průměrný výkon)}$$

U **neperiodických** signálů je lze uvažovat jako limitní případy:

$$x_{av} = \lim_{L\to\infty}\frac{1}{2L+1}\sum_{m=-L}^{L}x[m], \qquad P = \lim_{L\to\infty}\frac{1}{2L+1}\sum_{m=-L}^{L}|x[m]|^2$$

- signály s **konečnou energií** → **energetické signály** (jejich průměrný výkon je nulový),
- signály s **konečným (nenulovým) průměrným výkonem** → **výkonové signály** (jejich energie je nekonečná; např. periodické signály, skok).

U diskrétních signálů se energie a výkon obvykle **neudávají v joulech a wattech** – zpracováváme bezrozměrné řady čísel.

#### Řešené příklady
1. $x[n] = \{\underset{\uparrow}{1}, 2, 0, 0, 4\}$: $\quad E = 1^2 + 2^2 + 0 + 0 + 4^2 = \mathbf{21}$.
2. $x[n] = \{\dots, \underset{\uparrow}{1}, 2, 0, 0, 4, 1, 2, 0, 0, 4, \dots\}$ (perioda $N = 5$): $\quad P = \dfrac{1}{5}(1 + 4 + 0 + 0 + 16) = \mathbf{4{,}2}$.

---

## 3.3 Klasifikace diskrétních signálů

| Typ | Definice |
|---|---|
| **levostranný** | nulový pro $n > N$ |
| **pravostranný** | nulový pro $n < N$ |
| **kauzální** | nulový pro $n < 0$ |
| **antikauzální** | nulový pro $n \ge 0$ (na slidu „$n \ge 0$“; někdy se připouští i nenulová hodnota v $n=0$) |
| **periodický** | $x[n] = x[n \pm kN]$, $k = 1, 2, 3, \dots$ |

U periodického diskrétního signálu musí být **perioda $N$ vždy celé číslo** (počet vzorků).

> **Perioda součtu periodických diskrétních signálů je rovna nejmenšímu společnému násobku dílčích period.**

#### Řešené příklady
a) $x[n] = \{\dots, \underset{\uparrow}{1}, 2, 0, 0, 4, 1, 2, 0, 0, 4, \dots\}$ → perioda $N = 5$.

b) $x[n] = \{\dots, \underset{\uparrow}{1}, 2, 1, 2, \dots\}$ ($N_1 = 2$), $y[n] = \{\dots, \underset{\uparrow}{1}, 2, 3, 1, 2, 3, \dots\}$ ($N_2 = 3$). Perioda $g[n] = x[n] + y[n]$ je $N = \mathrm{NSN}(2, 3) = 6$; jedna perioda:

| $n$ | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| $x[n]$ | 1 | 2 | 1 | 2 | 1 | 2 |
| $y[n]$ | 1 | 2 | 3 | 1 | 2 | 3 |
| $g[n]$ | **2** | **4** | **4** | **3** | **3** | **5** |

---

## 3.4 Základní diskrétní signály

### Jednotkový impuls
$$\delta[n] = \begin{cases} 1 & n = 0 \\ 0 & n \neq 0 \end{cases}$$

Na rozdíl od Diracova impulsu je diskrétní impuls **obyčejná, dobře definovaná posloupnost** (hodnota 1, ne nekonečno).

**Vlastnosti:**

$$x[n]\,\delta[n-k] = x[k]\,\delta[n-k]$$

**Filtrační (vzorkovací) vlastnost (sifting property)** – impuls „propustí“ pouze hodnotu v $n = k$:

$$\sum_{n=-\infty}^{\infty} x[n]\,\delta[n-k] = x[k]$$

Důsledek – **každý diskrétní signál lze zapsat jako součet posunutých impulsů vážených hodnotami $x[k]$**:

$$\boxed{x[n] = \sum_{k=-\infty}^{\infty} x[k]\,\delta[n-k]}$$

Toho se využívá při výpočtu **odezvy LTI systému**, známe-li jeho **impulsní charakteristiku** (→ konvoluce, kap. 4).

### Jednotkový skok
$$u[n] = \begin{cases} 0 & n < 0 \\ 1 & n \ge 0 \end{cases} \qquad u[n] = \sum_{k=0}^{\infty}\delta[n-k]$$

(V diskrétním čase je $u[0] = 1$ jednoznačně definováno.) Platí také $\delta[n] = u[n] - u[n-1]$ a $u[n] = \sum_{k=-\infty}^{n}\delta[k]$.

### Lineární funkce (rampa)
$$r[n] = n\,u[n] = \begin{cases} 0 & n < 0 \\ n & n \ge 0 \end{cases} \qquad r[n] = \sum_{k=0}^{\infty}k\,\delta[n-k]$$

### Diskrétní pulzní signály

**Obdélníkový pulz** (délka $2N+1$ vzorků):
$$\mathrm{rect}\!\left(\frac{n}{2N}\right) = \begin{cases} 1 & |n| \le N \\ 0 & \text{pro ostatní } n \end{cases}$$

**Trojúhelníkový pulz:**
$$\mathrm{tri}\!\left(\frac{n}{N}\right) = \begin{cases} 1 - \dfrac{|n|}{N} & |n| \le N \\ 0 & \text{pro ostatní } n \end{cases}$$

### Diskrétní funkce sinc
$$\mathrm{sinc}\!\left(\frac{n}{N}\right) = \frac{\sin(n\pi/N)}{n\pi/N}, \qquad \mathrm{sinc}(0) = 1$$

- je rovna nule v $n = kN$, $k = \pm1, \pm2, \pm3, \dots$,
- obálka má **hlavní lalok** a klesající **postranní laloky**.

### Diskrétní exponenciála
$$x[n] = \alpha^n\,u[n]$$

- pro reálné $\alpha$: $|\alpha| > 1$ → exponenciála **roste**, $|\alpha| < 1$ → **klesá**; pro $\alpha < 0$ navíc **střídá znaménko**,
- pro komplexní $\alpha = r e^{j\theta}$: $\quad x[n] = r^n[\cos(n\theta) + j\sin(n\theta)]\,u[n]$ – komplexní oscilace s exponenciálně rostoucí ($r > 1$) nebo tlumenou ($r < 1$) obálkou.

> Na slidu je uvedeno „roste pro $\alpha > 0$, klesá pro $\alpha < 0$“ – to platí pro tvar $e^{\alpha n}$. Pro tvar $\alpha^n$ rozhoduje, zda $|\alpha| > 1$ nebo $|\alpha| < 1$ (grafy na slidu odpovídají $\alpha > 1$ a $0 < \alpha < 1$).

---

## 3.5 Diskrétní sinusoida

Diskrétní sinusoida vznikne **vzorkováním analogové sinusoidy** $x(t) = \cos(2\pi f t + \Theta)$ v okamžicích $nt_s$ (vzorkovací frekvence $S = 1/t_s$):

$$x[n] = \cos(2\pi f\,nt_s + \Theta) = \cos\!\left(2\pi n\frac{f}{S} + \Theta\right) = \cos(2\pi nF + \Theta) = \cos(\Omega n + \Theta)$$

nebo obecná **komplexní sinusoida**:

$$x[n] = e^{j(2\pi nF + \Theta)} = \cos(2\pi nF + \Theta) + j\sin(2\pi nF + \Theta)$$

**Vztah analogových a číslicových frekvencí:**

| analogová $f$ [Hz] | $-S$ | $-0{,}5S$ | $0$ | $0{,}5S$ | $S$ |
|---|---|---|---|---|---|
| číslicová $F = f/S$ | $-1$ | $-0{,}5$ | $0$ | $0{,}5$ | $1$ |
| analogová úhlová $\omega = 2\pi f$ | $-2\pi S$ | $-\pi S$ | $0$ | $\pi S$ | $2\pi S$ |
| číslicová úhlová $\Omega = 2\pi F$ | $-2\pi$ | $-\pi$ | $0$ | $\pi$ | $2\pi$ |

### Diskrétní sinusoida není „obyčejná“ sinusoida!

Analogová sinusoida $x(t) = \cos(2\pi f_0 t + \Theta)$ má dvě významné vlastnosti:

1. je **periodická** pro každou frekvenci $f_0$,
2. je **jednoznačně určena svou frekvencí** (různé frekvence → různé signály).

> **POZOR! Diskrétní sinusoidy tyto vlastnosti obecně nesplňují.**

#### 1) Periodicita diskrétní sinusoidy

Požadujeme $x[n + N] = x[n]$:

$$\cos(2\pi nF_0 + \Theta) \overset{?}{=} \cos(2\pi(n+N)F_0 + \Theta) = \cos(2\pi nF_0 + \Theta + 2\pi NF_0)$$

Výrazy jsou ekvivalentní, **pouze pokud je $NF_0$ celé číslo $k$**, tj.

$$\boxed{F_0 = \frac{k}{N}}$$

> **Diskrétní sinusoida je periodická pouze tehdy, když je její číslicová frekvence racionální číslo** (podíl dvou celých čísel). Je-li zlomek $k/N$ v základním tvaru, je **perioda $N$** vzorků a **jedna perioda diskrétní sinusoidy vznikne z $k$ period analogového signálu**.

*Příklady:*
- $\cos(0{,}2\pi n)$: $F_0 = 0{,}1 = 1/10$ → periodická, $N = 10$,
- $\cos(0{,}3\pi n)$: $F_0 = 0{,}15 = 3/20$ → periodická, $N = 20$ (jedna perioda obsahuje 3 cykly analogové sinusovky),
- $\cos(0{,}5 n)$: $F_0 = \frac{0{,}5}{2\pi} = \frac{1}{4\pi}$ → iracionální → **neperiodická** (vzorky se nikdy přesně neopakují, přestože obálka vypadá jako sinusovka).

#### 2) Nejednoznačnost frekvence (aliasing)

Přičtěme k $F_0$ celé číslo $m$:

$$\cos(2\pi n(F_0 + m) + \Theta) = \cos(2\pi nF_0 + \Theta + 2\pi nm) = \cos(2\pi nF_0 + \Theta) = x[n]$$

protože $nm$ je celé číslo. Důsledky:

- **diskrétní sinusoidy s frekvencemi $F_0$ a $F_0 \pm m$ jsou identické**,
- **spektrum diskrétní sinusoidy (diskrétního signálu) je periodické** s periodou 1 (v $F$), resp. $2\pi$ (v $\Omega$),
- **vzorky sinusoid s frekvencemi $F_0 \pm m$ nelze od sebe odlišit**.

Proto se zavádí **centrální perioda (základní rozsah)**:

$$\boxed{-0{,}5 \le F \le 0{,}5} \qquad (\text{tj. } -\pi \le \Omega \le \pi)$$

> Diskrétní sinusoidy lze **jednoznačně identifikovat pouze tehdy, mají-li frekvenci v základním rozsahu**. Sinusoidu s frekvencí $F_0$ mimo tento rozsah lze reprezentovat **alias frekvencí** $F_a = F_0 - M$, kde $M$ je celé číslo, které $F_a$ převede do základního rozsahu.

To je diskrétní pohled na aliasing z kap. 2: analogová frekvence $f_0 > S/2$ dává $F_0 = f_0/S > 0{,}5$, tedy alias.

*Příklad:* $x[n] = \cos(1{,}6\pi n)$ → $F_0 = 0{,}8$ → $F_a = 0{,}8 - 1 = -0{,}2$ → $x[n] = \cos(-0{,}4\pi n) = \cos(0{,}4\pi n)$. (Pro sinus: $\sin(1{,}6\pi n) = -\sin(0{,}4\pi n)$ – alias se zápornou frekvencí obrací fázi.) Nejvyšší „rychlost změny“ má diskrétní signál s $F = 0{,}5$: $\cos(\pi n) = (-1)^n$.

---

## 3.6 Základní operace se signály

### Posun signálu (shift)

Signál $x[n]$ posuneme v čase záměnou $n$ za $n - k$ ($k$ celé):

- $k > 0$ – **zpoždění** o $k$ vzorků (posun **doprava**): $x[n-k]$,
- $k < 0$ – **předstih** (posun **doleva**): $x[n+|k|]$.

Při posunu se počátek signálu přesune na $n_N = n \pm k$. Je-li signál uložen (off-line zpracování), jsou možné oba posuny; **při real-time zpracování není posun doleva (předstih) možný** – vyžadoval by budoucí vzorky.

### Otočení signálu (time reversal)

$y[n] = x[-n]$ je **časově otočená verze** („zrcadlový obraz“) $x[n]$ okolo počátku $n = 0$.

Signál $y[n] = x[-n-k]$ lze získat **dvěma způsoby**:

a) $x[n]$ → **zpoždění** o $k$ → $x[n-k]$ → **otočení** → $x[-n-k]$,

b) $x[n]$ → **otočení** → $x[-n]$ → **předsunutí** (posun doleva) o $k$ → $x[-(n+k)] = x[-n-k]$.

> Pravidlo: při kombinaci operací vždy dosazujeme za $n$ v *celém* argumentu. Např. $x[-n+1] = x[-(n-1)]$ je otočený signál posunutý o 1 **doprava**.

### Decimace (downsampling)

**Decimace** zmenšuje délku signálu odstraněním některých vzorků. Decimace faktorem $N$ znamená, že **vybereme každý $N$-tý vzorek** – výsledný signál je $N$-krát kratší:

$$y[n] = x[Nn]$$

(Vzorky mezi vybranými se nenávratně ztrácejí; pokud signál obsahuje vyšší frekvence, může decimace způsobit aliasing – efektivní vzorkovací frekvence klesne $N$-krát.)

### Interpolace (upsampling)

**Interpolace** signál prodlužuje přidáním vzorků. Interpolace faktorem $N$ dává $N$-krát delší signál (vzorky „v $N$-krát kratších úsecích“). Značí se $x^{\uparrow}[n/N]$ (resp. $x[n/N]$).

- Známe-li analogový signál, ze kterého $x[n]$ vznikl (nebo jeho analytický předpis), je interpolace snadná – prostě dopočítáme hodnoty.
- Jinak je nutné vzorky **doplnit**:
  - **„zero interpolation“** – přidané vzorky jsou **nulové** (operace se nazývá **up-sampling**):
  $$y[n] = \begin{cases} x[n/N] & n = 0, \pm N, \pm 2N, \dots \\ 0 & \text{jinak} \end{cases}$$
  - **„step interpolation“** – přidané vzorky jsou **shodné s předchozí hodnotou**,
  - **„linear interpolation“** – přidané vzorky leží na **přímce** mezi sousedními vzorky.

> **POZOR na pořadí operací decimace a interpolace – nejsou to obecně inverzní operace!** Interpolace a následná decimace stejným faktorem vrátí původní signál. **Decimace a následná interpolace ne** – decimací se ztratí vzorky, které už interpolace neobnoví.

### Neceločíselné zpoždění (fractional delay)

V praxi se někdy požaduje zpoždění o necelý počet vzorků (např. o polovinu vzorku). Lze ho realizovat kombinací interpolace a decimace. Chceme-li

$$y[n] = x\!\left[n - \frac{M}{N}\right] = x\!\left[\frac{Nn - M}{N}\right],$$

postupujeme:

```
x[n] → [Interpolace faktorem N] → x[n/N] → [Zpoždění o M vzorků] → x[(n−M)/N] → [Decimace faktorem N] → x[(Nn−M)/N]
```

---

## 3.7 Symetrie signálu

- **Symetrie (sudá symetrie):** $x[n] = x[-n]$ – signál je zrcadlově souměrný podle svislé osy $n = 0$,
- **Antisymetrie (lichá symetrie):** $x[n] = -x[-n]$ – signál je souměrný podle počátku; nutně $x[0] = 0$.

Symetrie a antisymetrie se navzájem vylučují (kromě nulového signálu). Součet symetrického a antisymetrického signálu není ani symetrický, ani antisymetrický. A naopak:

> **Libovolný signál lze rozložit na součet symetrické (sudé) a antisymetrické (liché) složky:**
> $$x[n] = x_e[n] + x_o[n], \qquad x_e[n] = \tfrac12 x[n] + \tfrac12 x[-n], \qquad x_o[n] = \tfrac12 x[n] - \tfrac12 x[-n]$$

Je-li $x[n]$ symetrický, je $x_o[n] = 0$; je-li antisymetrický, je $x_e[n] = 0$.

---

## 3.8 Řešené příklady k operacím se signály

**Příklad 1.** $x[n] = \{2, 3, \underset{\uparrow}{4}, 5, 6, 7\}$ (tj. $n = -2, \dots, 3$). Určete:

| signál | postup | výsledek | rozsah $n$ |
|---|---|---|---|
| a) $y[n] = x[n-3]$ | posun doprava o 3 | $\{2, 3, 4, 5, 6, 7\}$ | $1 \dots 6$ ($y[3] = 4$) |
| b) $f[n] = x[n+2]$ | posun doleva o 2 | $\{2, 3, 4, 5, 6, 7\}$ | $-4 \dots 1$ ($f[-2] = 4$) |
| c) $g[n] = x[-n]$ | otočení | $\{7, 6, 5, \underset{\uparrow}{4}, 3, 2\}$ | $-3 \dots 2$ |
| d) $h[n] = x[-n+1]$ | otočení, posun doprava o 1 | $\{7, 6, 5, 4, 3, 2\}$ | $-2 \dots 3$ ($h[1] = 4$) |
| e) $s[n] = x[-n-2]$ | otočení, posun doleva o 2 | $\{7, 6, 5, 4, 3, 2\}$ | $-5 \dots 0$ ($s[-2] = 4$) |

*Kontrola d):* $h[1] = x[-1+1] = x[0] = 4$ ✓, $h[-2] = x[3] = 7$ ✓.

**Příklad 2.** Rozložte $x[n] = \{4, -2, \underset{\uparrow}{4}, -6, 0\}$ na sudou a lichou složku.

| $n$ | −2 | −1 | 0 | 1 | 2 |
|---|---|---|---|---|---|
| $x[n]$ | 4 | −2 | 4 | −6 | 0 |
| $x[-n]$ | 0 | −6 | 4 | −2 | 4 |
| $x_e[n] = \frac12(x[n] + x[-n])$ | **2** | **−4** | **4** | **−4** | **2** |
| $x_o[n] = \frac12(x[n] - x[-n])$ | **2** | **2** | **0** | **−2** | **−2** |

Kontrola: $x_e + x_o = \{4, -2, 4, -6, 0\}$ ✓.

**Příklad 3.** Pro $x[n] = \{1, \underset{\uparrow}{2}, 5, -1\}$ vytvořte $x[2n]$ a různé verze $x^{\uparrow}[n/3]$.

- **decimace** $x[2n]$: $y[0] = x[0] = 2$, $y[1] = x[2] = -1$, $y[-1] = x[-2] = 0$ → $x[2n] = \{\underset{\uparrow}{2}, -1\}$,
- **zero interpolation**: $\{1, 0, 0, \underset{\uparrow}{2}, 0, 0, 5, 0, 0, -1, 0, 0\}$,
- **step interpolation**: $\{1, 1, 1, \underset{\uparrow}{2}, 2, 2, 5, 5, 5, -1, -1, -1\}$,
- **linear interpolation** (za posledním vzorkem předpokládáme 0): $\{1, \tfrac43, \tfrac53, \underset{\uparrow}{2}, 3, 4, 5, 3, 1, -1, -\tfrac23, -\tfrac13\}$.

**Příklad 4.** Pro $x[n] = \{3, 4, \underset{\uparrow}{5}, 6\}$ určete (step interpolace):

a) $g[n] = x[2n-1]$: nejprve posun ($x[n-1]$), pak decimace: $g[0] = x[-1] = 4$, $g[1] = x[1] = 6$ → $g[n] = \{\underset{\uparrow}{4}, 6\}$.

$h[n] = x[0{,}5n - 1] = x\!\left[\frac{n-2}{2}\right]$: interpolace faktorem 2 (step) → $\{3, 3, 4, 4, \underset{\uparrow}{5}, 5, 6, 6\}$, pak zpoždění o 2 → $h[n] = \{3, 3, \underset{\uparrow}{4}, 4, 5, 5, 6, 6\}$ (kontrola: $h[0] = x[-1] = 4$ ✓).

b) $y[n] = x[2n/3]$: **nejprve interpolace** faktorem 3 → $\{3,3,3,4,4,4,\underset{\uparrow}{5},5,5,6,6,6\}$, **pak decimace** faktorem 2 → $y[n] = \{3, 3, 4, \underset{\uparrow}{5}, 5, 6\}$.

**Příklad 5.** Pro $x[n] = \{2, 4, \underset{\uparrow}{6}, 8\}$ určete $y[n] = x[n - 0{,}5]$ (lineární interpolace).

$x[n - 0{,}5] = x\!\left[\frac{2n-1}{2}\right]$: interpolace faktorem 2 → $\{2, 3, 4, 5, \underset{\uparrow}{6}, 7, 8, 4\}$ → zpoždění o 1 → decimace faktorem 2 → $y[n] = \{3, \underset{\uparrow}{5}, 7, 4\}$.

*Kontrola:* $y[0] = x[-0{,}5] = \frac{4+6}{2} = 5$ ✓, $y[1] = x[0{,}5] = \frac{6+8}{2} = 7$ ✓.

---

## 3.9 Úvod do náhodných signálů

- **Deterministické signály** (predikovatelné) – každá realizace experimentu (měření za stejných podmínek) dává **stejný signál**.
- **Náhodné signály** – stejný experiment dává **pokaždé jiný výsledek**; opakováním získáme soubor realizací – tzv. **náhodný proces $X(t)$**. Výsledek lze popsat jen **pravděpodobnostně**, pravděpodobnost $\Pr(A) \in \langle 0, 1\rangle$.

K **úplnému popisu** náhodného procesu potřebujeme:

1. znát **rozsah hodnot** náhodné proměnné $x$ (konečný i nekonečný),
2. určit, s jakou pravděpodobností se jednotlivé hodnoty vyskytují – **distribuční funkci** $F(x) \in \langle 0, 1\rangle$:

$$F(x_1) = \Pr(x < x_1) = \int_{-\infty}^{x_1} f(x)\,dx, \qquad f(x) = \frac{dF(x)}{dx}, \qquad \Pr(x_1 \le x \le x_2) = F(x_2) - F(x_1)$$

kde $f(x)$ je **hustota pravděpodobnosti** (probability density function, pdf), $\int_{-\infty}^{\infty}f(x)\,dx = 1$.

### Charakteristiky náhodných signálů

> Na slidech je směrodatná odchylka značena $\delta_x$ (rozptyl $\delta_x^2$); zde používáme obvyklé $\sigma_x$.

| Charakteristika | Vztah |
|---|---|
| **střední (očekávaná) hodnota** | $m_x = \bar E(x) = \int_{-\infty}^{\infty} x\,f(x)\,dx$ |
| **střední kvadratická hodnota** | $\bar E(x^2) = \int_{-\infty}^{\infty} x^2 f(x)\,dx$ |
| **$n$-tý moment** | $m_n = \int_{-\infty}^{\infty} x^n f(x)\,dx$ |
| **$n$-tý centrální moment** | $\mu_n = \int_{-\infty}^{\infty} (x - m_x)^n f(x)\,dx$ |
| **rozptyl** ($\mu_2$) | $\sigma_x^2 = \bar E(x - m_x)^2 = \bar E(x^2) - m_x^2$ |

- **Rozptyl** $\sigma_x^2$ určuje **podíl střídavé složky signálu** (výkon střídavé složky),
- $\sigma_x = \sqrt{\sigma_x^2}$ je **směrodatná odchylka** – určuje míru **nejistoty** měření signálu.

Vztah $\sigma_x^2 = \bar E(x^2) - m_x^2$ říká: **celkový výkon = výkon střídavé složky + výkon stejnosměrné složky**.

### Rozptyl periodických signálů

Pro periodický signál s periodou $T$:

$$\sigma_x^2 = \underbrace{\frac{1}{T}\int_0^T x^2(t)\,dt}_{\text{celkový výkon signálu}} - \underbrace{\left[\frac1T\int_0^T x(t)\,dt\right]^2}_{\text{výkon stejnosměrné složky}}$$

| Signál | Předpis | Rozptyl |
|---|---|---|
| sinusoida | $A\cos(2\pi t/T + \Theta)$ | $A^2/2$ |
| pila | $A\,t/T$, $0 \le t \le T$ | $A^2/12$ |
| trojúhelník | $A(1 - \lvert t\rvert/0{,}5T)$, $\lvert t\rvert \le 0{,}5T$ | $A^2/12$ |
| obdélník (střída 50 %) | $A$ pro $0 \le t < 0{,}5T$, $0$ pro $0{,}5T \le t < T$ | $A^2/4$ |

*Ověření pro obdélník:* celkový výkon $A^2/2$, stejnosměrná složka $A/2$ → $\sigma^2 = A^2/2 - A^2/4 = A^2/4$.

---

## 3.10 Pravděpodobnostní rozložení

### Rovnoměrné rozložení

Každá hodnota z intervalu $\langle\alpha, \beta\rangle$ je stejně pravděpodobná; $f(x)$ je pravoúhlý impuls s jednotkovou plochou:

$$f(x) = \begin{cases} \dfrac{1}{\beta - \alpha} & \alpha \le x \le \beta \\ 0 & \text{jinde}\end{cases}
\qquad
F(x) = \begin{cases} 0 & x < \alpha \\ \dfrac{x - \alpha}{\beta - \alpha} & \alpha \le x \le \beta \\ 1 & x > \beta\end{cases}$$

- střední hodnota: $m_x = 0{,}5(\alpha + \beta)$,
- rozptyl: $\sigma_x^2 = \dfrac{(\beta - \alpha)^2}{12}$.

(Odtud kvantizační šum $\Delta^2/12$ z kap. 2 – rovnoměrné rozložení na intervalu šířky $\Delta$.)

### Gaussovo (normální) rozložení

$$f(x) = \frac{1}{\sqrt{2\pi\sigma_x^2}}\exp\!\left[-\frac{(x - m_x)^2}{2\sigma_x^2}\right]$$

> Na slidu chybí ve jmenovateli exponentu druhá mocnina ($2\delta_x$ místo $2\delta_x^2$).

- vrchol v $x = m_x$, sudá symetrie okolo vrcholu,
- inflexní body v $m_x \pm \sigma_x$.

**Vlastnosti:**

1. Má-li $x$ normální rozložení, má normální rozložení i $ax + b$.
2. **Součet** signálů s normálním rozložením má také normální rozložení; **střední hodnota součtu = součet středních hodnot**, **rozptyl součtu = součet rozptylů** (pro nezávislé signály).
3. Střední absolutní odchylka je $\bar E|x - m_x| = \sigma_x\sqrt{2/\pi}$ – u normálního rozložení lze tedy pracovat s $|x - m_x|$ místo se $\sigma_x$.
4. **Všechny momenty vyšších řádů lze určit jen ze znalosti prvních dvou momentů** ($m_x$ a $\sigma_x^2$): liché centrální momenty jsou nulové, pro sudé $n = 2k$:
$$\mu_{2k} = \frac{(2k)!}{k!\,2^k}\,\sigma_x^{2k} \qquad (\text{např. } \mu_4 = 3\sigma_x^4)$$

**Gaussovská distribuční funkce:**
$$F(x) = \frac{1}{\sqrt{2\pi\sigma^2}}\int_{-\infty}^{x}\exp\!\left[-\frac{(\lambda - m_x)^2}{2\sigma^2}\right]d\lambda$$

**Standardní normální rozložení** (střední hodnota 0, rozptyl 1):
$$P(x) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x} e^{-\lambda^2/2}\,d\lambda$$

**Chybová funkce** (tabelovaná, v MATLABu `erf(x)`):
$$\mathrm{erf}(x) = \frac{2}{\sqrt\pi}\int_0^x e^{-\lambda^2}\,d\lambda$$

$$F(x) = P\!\left(\frac{x - m_x}{\sigma}\right) = 0{,}5 + 0{,}5\,\mathrm{erf}\!\left(\frac{x - m_x}{\sigma\sqrt2}\right)$$

**Pravděpodobnost, že $x$ leží mezi $x_1$ a $x_2$:**
$$\Pr[x_1 \le x \le x_2] = F(x_2) - F(x_1) = 0{,}5\left[\mathrm{erf}\!\left(\frac{x_2 - m_x}{\sigma\sqrt2}\right) - \mathrm{erf}\!\left(\frac{x_1 - m_x}{\sigma\sqrt2}\right)\right]$$

**Q-funkce** – pravděpodobnost, že standardní normální veličina nabude hodnoty **větší** než $x$:
$$Q(x) = 1 - P(x) = \frac{1}{\sqrt{2\pi}}\int_x^{\infty}e^{-\lambda^2/2}\,d\lambda = 0{,}5 - 0{,}5\,\mathrm{erf}\!\left(\frac{x}{\sqrt2}\right)$$

$$\Pr[x_1 \le x \le x_2] = Q\!\left(\frac{x_1 - m_x}{\sigma}\right) - Q\!\left(\frac{x_2 - m_x}{\sigma}\right)$$

> **Opravy slidů 31–32:** v erf vzorci násobí koeficient 0,5 *oba* členy; v Q-funkci je správně $\mathrm{erf}(x/\sqrt2)$ (ne $x\sqrt2$); a protože $Q$ je klesající, je v posledním vzorci $Q$ dolní meze minus $Q$ horní meze.

*Užitečné hodnoty:* $\Pr(|x - m_x| \le \sigma) \approx 68{,}3\,\%$, $\Pr(|x - m_x| \le 2\sigma) \approx 95{,}4\,\%$, $\Pr(|x - m_x| \le 3\sigma) \approx 99{,}7\,\%$.

### Centrální limitní věta

> **Součet velkého množství nezávislých náhodných veličin se stejným (libovolným) rozložením a konečnou střední hodnotou a rozptylem konverguje k normálnímu rozložení.** Věta platí i pro diskrétní náhodné veličiny.

$$s = \sum_{i=1}^{n}x_i,\ n \gg 1 \quad\Longrightarrow\quad f_n(s) \approx \frac{1}{\sqrt{2\pi n\sigma_x^2}}\exp\!\left(-\frac{(s - nm_x)^2}{2n\sigma_x^2}\right)$$

(součet má střední hodnotu $nm_x$ a rozptyl $n\sigma_x^2$). Proto se šum v přírodě často chová gaussovsky – vzniká součtem mnoha drobných nezávislých vlivů.

---

## 3.11 Vlastnosti náhodných signálů a jejich odhad

- **Stacionární** signál – pravděpodobnostní popis **nezávisí na počátku časové osy** (statistické vlastnosti se v čase nemění).
- **Ergodický** signál – náhodný signál, jehož určitá časová charakteristika má nulový rozptyl (je ergodický vůči této charakteristice). Prakticky: **časový průměr přes jednu (dostatečně dlouhou) realizaci je roven souborovému průměru přes všechny realizace** – stačí tedy měřit jeden záznam.
- **Striktně ergodický** – stacionární a ergodický.
- **Pseudonáhodný** signál – není úplně náhodný, je to **uměle generovaný** signál s definovaným statistickým rozdělením. Je **periodický s velmi dlouhou periodou**, v rámci periody splňuje požadované statistické vlastnosti.

**Analýza náhodných signálů v praxi:** statistický popis obvykle neznáme – z naměřených hodnot $x_k$, $k = 1, \dots, N$ určujeme **odhady**:

$$m_x = \frac{1}{N}\sum_{k}x_k, \qquad \sigma_x^2 = \frac{1}{N-1}\sum_{k}(x_k - m_x)^2$$

(dělení $N - 1$ dává nestranný odhad rozptylu, protože $m_x$ je sám odhadnut ze stejných dat).

---

## 3.12 Šum

> **Šum** je nežádoucí signál, který interferuje s „užitečným“ signálem (naměřeným nebo přeneseným komunikačním kanálem).

### Klasifikace podle zdroje
a) **akustický šum** – vibrace, pohyb, srážky objektů (klimatizace, ventilátor, doprava, vítr, déšť, hovor lidí),
b) **elektromagnetický šum** – elektrická zařízení (rádio, TV, mobilní telefony),
c) **elektrostatický šum** – obvykle zářivky, výbojky,
d) **zkreslení přenosového kanálu** – echo, útlum,
e) **šum vzniklý zpracováním** – kvantizační šum, ztráta datových paketů při komunikaci.

### Klasifikace podle frekvence
a) **úzkopásmový šum** (narrowband) – obvykle 50/60 Hz z elektrických zařízení,
b) **bílý šum** – čistě náhodný šum s plochou frekvenční charakteristikou (rovnoměrné zastoupení všech frekvencí),
c) **pásmově omezený bílý šum** – plochá charakteristika v pásmu zpracovávaném zařízením,
d) **barevný šum** – šum, který není bílý (širokopásmový šum s neplochým spektrem),
e) **impulzní šum** – krátké pulzy s náhodnou amplitudou a délkou trvání,
f) **krátkodobý šum** – pulzy s delší dobou trvání.

### Bílý šum
- **nekorelovaný** náhodný signál s **rovnoměrnou výkonovou spektrální hustotou** – jeho autokorelace je jediný impuls v nule ($r_{nn}[k] = \sigma^2\delta[k]$), spektrum je ploché,
- čistý bílý šum je **teoretický pojem** (stejný výkon na všech frekvencích v $(-\infty, \infty)$ → nekonečný celkový výkon) – nerealizovatelný,
- v praxi pracujeme s **pásmově omezeným bílým šumem** (ploché spektrum v definovaném rozsahu).

### Barevné šumy
| Šum | Výkonová spektrální hustota | Sklon |
|---|---|---|
| **růžový** (1/f šum) | $\propto 1/f$ | −3 dB/oktávu; **stejná energie ve stejně širokých pásmech v log. měřítku** (např. v každé oktávě) |
| **hnědý** (Brownův) | $\propto 1/f^2$ | −6 dB/oktávu; lze generovat simulací Brownova pohybu (integrací bílého šumu) |
| **modrý** (azurový) | $\propto f$ | +3 dB/oktávu |
| **purpurový** (fialový) | $\propto f^2$ | +6 dB/oktávu |
| **šedý** | – | používá se v psychoakustice (měření křivky stejné hlasitosti) |

### Impulzní a krátkodobý šum
- **Impulzní šum** – krátkodobé impulsy (v audiotechnice do ~3 ms), zdrojem je např. stisk kláves, praskání starých gramofonových desek. Ideální impuls má ploché spektrum; širší pulz má spektrum typu dolní propust, dvojpolaritní pulz spíš typu horní propust.
- **Krátkodobý šum** – relativně krátký ostrý pulz následovaný klesajícími nízkofrekvenčními oscilacemi (např. poškrábaná gramofonová deska).

### Fyzikální zdroje šumu v elektronice
- **Tepelný (Johnsonův) šum** – náhodný (tepelný) pohyb elektronů v odporových strukturách,
- **Výstřelový (shot) šum** – náhodná rekombinace párů elektron–díra v polovodičích, náhodná emise elektronů v elektronkách,
- **Elektromagnetický šum** – vzniká v každém zařízení, které generuje, spotřebovává nebo přenáší elektrickou energii (transformátory, vysílače, výbojky, motory, startéry automobilů).

### Poměr signál/šum (SNR)

Pro zašuměný signál $x(t) = s(t) + A\,n(t)$ ($s$ – užitečný signál, $A n(t)$ – šum s amplitudou $A$):

$$\mathrm{SNR} = 10\log\!\left(\frac{\sigma_s^2}{A^2\sigma_n^2}\right)\ [\text{dB}]$$

**Zlepšení SNR – koherentní průměrování.** Máme-li $M$ realizací stejného (opakujícího se, synchronizovaného) signálu s nekorelovaným šumem s nulovou střední hodnotou, jejich zprůměrováním se užitečný signál zachová, kdežto rozptyl šumu klesne $M$-krát:

$$\mathrm{SNR}_M = \mathrm{SNR}_1 + 10\log_{10}M\ [\text{dB}]$$

(Na slidu: jedna realizace zašuměné sinusovky, průměr 8 a průměr 48 realizací.) Typické použití: **evokované potenciály** v EEG (kap. 1) – odezva na podnět se průměruje přes stovky opakování.

---

## Shrnutí kapitoly 3

- Diskrétní signál je definován jen pro celočíselná $n$; $E = \sum|x[n]|^2$, $P = \frac1N\sum_{\text{perioda}}|x[n]|^2$.
- $x[n] = \sum_k x[k]\delta[n-k]$ – rozklad na impulsy (základ konvoluce).
- Číslicová frekvence $F = f/S$ [cykly/vzorek], $\Omega = 2\pi F$ [rad/vzorek].
- Diskrétní sinusoida je periodická jen pro racionální $F_0 = k/N$; frekvence $F_0$ a $F_0 + m$ jsou nerozlišitelné → základní rozsah $|F| \le 0{,}5$.
- Operace: posun ($x[n-k]$ zpoždění), otočení ($x[-n]$), decimace ($x[Nn]$), interpolace ($x[n/N]$: zero/step/linear); pořadí operací je důležité.
- Rozklad na sudou a lichou složku: $x_{e,o} = \frac12(x[n] \pm x[-n])$.
- Náhodné signály: $m_x$, $\sigma_x^2 = \bar E(x^2) - m_x^2$; rovnoměrné ($\sigma^2 = (\beta-\alpha)^2/12$) a Gaussovo rozložení, erf, Q-funkce, CLV; stacionarita, ergodicita.
- Šumy: bílý (plochý), růžový ($1/f$), hnědý ($1/f^2$), modrý ($f$), fialový ($f^2$); SNR, koherentní průměrování (+10 log M dB).

### Kontrolní otázky
1. Jaká je perioda signálu $x[n] = \cos(0{,}3\pi n) + \sin(0{,}25\pi n)$? *(Řešení: $F_1 = 3/20 \Rightarrow N_1 = 20$; $F_2 = 1/8 \Rightarrow N_2 = 8$; $N = \mathrm{NSN}(20, 8) = 40$.)*
2. Proč je $\cos(0{,}5n)$ neperiodický, přestože vypadá jako sinusovka?
3. Na jakou frekvenci v základním rozsahu se zobrazí $x[n] = \cos(2{,}2\pi n)$? *(Řešení: $F = 1{,}1 \to F_a = 0{,}1$.)*
4. Proč interpolace a decimace nejsou navzájem inverzní? Ukažte na příkladu.
5. Jak realizujete zpoždění o 1/3 vzorku?
6. Odvoďte rozptyl rovnoměrného rozložení.
7. Kolikrát se zlepší SNR (v dB) průměrováním 100 realizací?

---

# 4. Diskrétní systémy, impulsní odezva, konvoluce a korelace

## 4.1 Diskrétní systém a jeho vlastnosti

> **Diskrétní systém** $S$ zpracovává časově diskrétní vstupní signál $x[n]$ a produkuje časově diskrétní výstupní signál $y[n]$.

```
x[n] ──► [ S ] ──► y[n]
```

### Linearita

Systém je **lineární**, splňuje-li **princip superpozice** (aditivitu a homogenitu):

$$A\,x_1[n] + B\,x_2[n] \;\xrightarrow{\;S\;}\; A\,y_1[n] + B\,y_2[n]$$

pro libovolné konstanty $A, B$ a signály $x_1, x_2$ (kde $y_i$ je odezva na $x_i$). Není-li rovnice splněna, je systém **nelineární**.

### Časová invariance

Systém je **časově invariantní**, závisí-li odezva jen na **tvaru** vstupního signálu, a ne na okamžiku, kdy je signál přiveden:

$$x[n - m] \;\xrightarrow{\;S\;}\; y[n - m]$$

Systémy, které tuto vlastnost nesplňují, jsou **časově variantní** (např. **adaptivní filtry**).

### LTI systém – praktický test z diferenční rovnice

**Lineární časově invariantní (LTI) systém** splňuje zároveň linearitu i časovou invarianci. Z diferenční rovnice to poznáme takto:

1. Diferenční rovnice LTI systému **musí mít konstantní koeficienty a nesmí obsahovat konstantní (aditivní) členy**.
2. Vyskytují-li se v rovnici **součiny** vstupů či výstupů (např. $y^2[n-1]$, $x[n]y[n]$), **nelineární funkce** vstupu nebo **konstantní členy** → systém je **nelineární**.
3. Jsou-li koeficienty **funkcí $n$**, nebo je **$n$ násobeno uvnitř argumentu** (např. $y[2n]$) → systém je **časově variantní**.

#### Řešený příklad

| Rovnice | Klasifikace | Důvod |
|---|---|---|
| a) $y[n] - 2y[n-1] = 4x[n]$ | **LTI** | konstantní koeficienty, vše lineární |
| b) $y[n] - 2n\,y[n-1] = x[n]$ | **pouze lineární** (časově variantní) | koeficient $2n$ závisí na $n$ |
| c) $y[n] - 2y^2[n-1] = 2x[n] - 2x[n-1]$ | **nelineární, ale TI** | člen $y^2$ |
| d) $y[n] - 2y[n-1] = 2^{x[n]}x[n]$ | **nelineární, ale TI** | nelineární funkce vstupu |
| e) $y[n] - 4y[n]\,y[2n] = x[n]$ | **nelineární, časově variantní** | součin výstupů a argument $2n$ |

### Kauzalita

U **kauzálního** systému výstup $y[n]$ **nezávisí na budoucích hodnotách vstupu** – v rovnici se nevyskytují členy typu $x[n+2]$ (relativně k nejnovějšímu výstupu). Systémy pracující v reálném čase musí být kauzální.

$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0\,x[n+K] \quad\text{je kauzální pro } K \le 0$$
$$y[n+L] + A_1y[n+L-1] + \dots + A_Ly[n] = B_0\,x[n+K] \quad\text{je kauzální pro } K \le L$$

(rozhoduje, zda nejpokročilejší vstupní vzorek není „před“ nejpokročilejším výstupním). Je-li systém popsán přenosovou funkcí (kap. 6)

$$H(z) = \frac{B_0z^P + B_1z^{P-1} + \dots + B_{P-1}z + B_P}{A_0z^Q + A_1z^{Q-1} + \dots + A_{Q-1}z + A_Q},$$

je kauzální, pokud **$P \le Q$** (stupeň čitatele nepřevyšuje stupeň jmenovatele).

### Statický a dynamický systém

Závisí-li odezva v čase $n = n_0$ **pouze na hodnotě vstupu v témže okamžiku** $n_0$, je systém **statický** (bez paměti), jinak je **dynamický** (s pamětí). Obecně statický systém: $y[n+\alpha] = k\,x[n+\alpha]$.

#### Řešený příklad

| Systém | Klasifikace |
|---|---|
| $y[n] = x[n+2]$ | nekauzální, dynamický |
| $y[n+4] + y[n+3] = x[n+2]$ | kauzální, dynamický (po posunu: $y[n] + y[n-1] = x[n-2]$) |
| $y[n] = 2x[\alpha n]$, $\alpha = 1$ | kauzální, statický |
| $y[n] = 2x[\alpha n]$, $\alpha < 1$ | kauzální, dynamický (pro $n \ge 0$) |
| $y[n] = 2x[\alpha n]$, $\alpha > 1$ | nekauzální, dynamický |
| $y[n] = 2(n+1)\,x[n]$ | kauzální, statický, **časově variantní** |

(Poznámka: pro $\alpha < 1$ a záporná $n$ by systém potřeboval budoucí hodnoty; klasifikace na slidu předpokládá $n \ge 0$.)

---

## 4.2 Základní stavební bloky diskrétních systémů

| Blok | Funkce | Značka |
|---|---|---|
| **zpoždění** | $x[n] \to x[n-1]$ | blok $z^{-1}$ |
| **posun (předstih)** | $x[n] \to x[n+1]$ | blok $z^{1}$ (nekauzální) |
| **násobení konstantou** | $x[n] \to A\,x[n]$ | trojúhelník $A$ |
| **větvení signálu** | $x[n] \to x[n], x[n]$ | uzel |
| **násobení signálů** | $y[n] = x_1[n]\cdot x_2[n]$ | kroužek × (nelineární!) |
| **součet signálů** | $y[n] = x_1[n] + x_2[n]$ | kroužek + / Σ |

### Realizační struktury LTI systémů

Uvažujme diferenční rovnici 1. řádu:

$$y[n] = -a_1\,y[n-1] + b_0\,x[n] + b_1\,x[n-1]$$

**Přímá forma I (direct form I)** – přímo kopíruje rovnici: využívá **dva zpožďovací členy** – jeden ve vstupní (nerekurzivní) větvi, druhý ve výstupní (rekurzivní) větvi:

```
x[n] ──┬──[b0]──►(+)──── v[n] ────►(+)──┬──► y[n]
       │          ▲                  ▲   │
     [z⁻¹]        │                  │ [z⁻¹]
       └──[b1]────┘                  └─[−a1]┘
```

Přímá forma I = kaskáda „nerekurzivní část → rekurzivní část“. Protože u LTI systémů na pořadí kaskády nezáleží (konvoluce je komutativní), můžeme pořadí prohodit: nejprve rekurzivní část, pak nerekurzivní. Obě části pak pracují se **stejným zpožděným signálem** $w[n]$, takže zpožďovací linky lze **sloučit**:

**Přímá forma II (direct form II, kanonická):**

$$w[n] = -a_1\,w[n-1] + x[n], \qquad y[n] = b_0\,w[n] + b_1\,w[n-1]$$

```
x[n] ──►(+)──── w[n] ──┬──[b0]──►(+)──► y[n]
         ▲             │          ▲
         │           [z⁻¹]        │
         │             │ w[n−1]   │
         └────[−a1]────┴──[b1]────┘
```

> **Přímá forma II se používá v praktických aplikacích** – je efektivnější, obsahuje **jedinou zpožďovací linku** (minimální počet zpožďovacích členů = řád systému).

Pro rovnici $N$-tého řádu je převod stejný: přímá forma I má $M + N$ zpožďovacích členů (dvě linky), přímá forma II jen $\max(M, N)$ – jedna linka $w[n-1], w[n-2], \dots, w[n-N]$, ze které se odebírají jak zpětné vazby s koeficienty $-a_k$, tak dopředné větve s koeficienty $b_k$.

#### Řešené příklady – bloková schémata

**a)** $y[n] - 0{,}6y[n-1] = x[n]$, tj. $y[n] = x[n] + 0{,}6\,y[n-1]$: sčítačka na vstupu, výstup se větví do zpožďovacího členu $z^{-1}$ a přes zesílení 0,6 se vrací do sčítačky.

**b)** $y[n] - \tfrac16 y[n-1] - \tfrac16 y[n-2] = 4x[n]$: (ve tvaru přímé formy II) $w[n] = x[n] + \tfrac16 w[n-1] + \tfrac16 w[n-2]$, $y[n] = 4w[n]$ – dvě zpoždění za sebou, zpětné vazby se zesílením $1/6$, výstup přes zesílení 4.

**c) Opačná úloha** – určete diferenční rovnici ze schématu (na slidu struktura přímé formy II se zpětnými vazbami $+1\cdot w[n-1]$ a $+2\cdot w[n-2]$ a dopřednými větvemi $2\cdot w[n]$ a $-1\cdot w[n-1]$):

$$w[n] = x[n] + w[n-1] + 2w[n-2], \qquad y[n] = 2w[n] - w[n-1]$$
$$\Rightarrow\quad \boxed{y[n] - y[n-1] - 2y[n-2] = 2x[n] - x[n-1]}$$

*Postup:* koeficienty zpětných vazeb (s opačným znaménkem) dávají levou stranu, dopředné koeficienty dávají pravou stranu.

---

## 4.3 Analýza diskrétních systémů v časové oblasti

Diskrétní systémy lze v časové oblasti popsat:

- **diferenční rovnicí** – použitelné pro lineární, nelineární i časově variantní systémy; pro LTI systémy lze využít princip superpozice,
- **impulsní odezvou $h[n]$** – popisuje odezvu ustáleného (relaxovaného) LTI systému; výstup je **konvoluční součet** vstupu a impulsní odezvy,
- **stavovými rovnicemi a stavovými proměnnými** – systém $n$-tého řádu se popíše soustavou $n$ diferenčních rovnic 1. řádu; vhodné pro složité a nelineární systémy.

### Systémy popsané diferenční rovnicí

**Obecný rekurzivní filtr $N$-tého řádu – ARMA (autoregressive moving average):**
$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]$$

**Nerekurzivní filtr (FIR) – „klouzavý průměr“, MA (moving average):**
$$y[n] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]$$

**Rekurzivní filtr (IIR) $N$-tého řádu – AR (autoregressive):**
$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0x[n]$$

---

## 4.4 Výpočet odezvy – řešení diferenční rovnice

- **Nerekurzivní systémy** – jednoduché: výstup je vážený součet vstupních hodnot.
- **Rekurzivní systémy** – musíme řešit diferenční rovnici $N$-tého řádu a znát **počáteční podmínky** $y[-1], y[-2], \dots, y[-N]$.

### Rekurzivní (iterační) řešení

Postupně počítáme $y[0], y[1], y[2], \dots$ Je přímočaré (tak počítá i počítač), ale obecný tvar řešení z něj často nelze snadno vyčíst.

**Příklady ze slidů (Ambardar):**

**a)** $y[n] = a_1y[n-1] + b_0u[n]$, $y[-1] = 0$:
$$y[0] = b_0,\quad y[1] = a_1b_0 + b_0 = b_0(1 + a_1),\quad y[2] = b_0(1 + a_1 + a_1^2),\ \dots$$
$$y[n] = b_0(1 + a_1 + \dots + a_1^n) = b_0\,\frac{1 - a_1^{n+1}}{1 - a_1}$$
(součet geometrické řady).

**b)** $y[n] = a_1y[n-1] + b_0\,n\,u[n]$, $y[-1] = 0$: $y[0] = 0$, $y[1] = b_0$, $y[2] = a_1b_0 + 2b_0$, $y[3] = a_1^2b_0 + 2a_1b_0 + 3b_0$, … obecně $y[n] = b_0(a_1^{n-1} + 2a_1^{n-2} + \dots + n)$ – uzavřený tvar lze odvodit jen pracně („What a chore!“) → potřebujeme systematičtější metodu.

**c)** $y[n] = y[n-1] + x[n] - x[n-3]$, $x[n] = \delta[n]$, $y[-1] = 0$:
$$y[0] = 1,\ y[1] = 1,\ y[2] = 1,\ y[3] = 1 - 1 = 0,\ y[4] = 0,\ \dots$$
Impulsní odezva má jen 3 nenulové hodnoty – tento „rekurzivní“ systém je ve skutečnosti **FIR filtr v přestrojení** ($y[n] = x[n] + x[n-1] + x[n-2]$, klouzavý součet).

### Formální (analytické) řešení

Pro rovnici
$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = x[n]$$
je řešení součtem **homogenního** a **partikulárního** řešení: $y[n] = y_H[n] + y_P[n]$.

**1) Homogenní řešení** (pravá strana nulová):
$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = 0$$

Řešením je lineární kombinace exponenciál $z^n$, jejichž základy jsou **kořeny charakteristické rovnice**:
$$1 + A_1z^{-1} + \dots + A_Nz^{-N} = 0 \quad\Longleftrightarrow\quad z^N + A_1z^{N-1} + \dots + A_N = 0$$

Pro $N$ **reálných různých** kořenů $z_1, \dots, z_N$:
$$y_H[n] = K_1z_1^n + K_2z_2^n + \dots + K_Nz_N^n$$

**Tvar homogenního řešení (přirozené odezvy) podle typu kořenů:**

| Kořen charakteristické rovnice | Tvar přirozené odezvy |
|---|---|
| reálný, jednoduchý $r$ | $K r^n$ |
| komplexně sdružený pár $re^{\pm j\Omega}$ | $r^n[K_1\cos(n\Omega) + K_2\sin(n\Omega)]$ |
| reálný, $(p+1)$-násobný $r$ | $r^n(K_0 + K_1n + K_2n^2 + \dots + K_pn^p)$ |
| komplexní, $(p+1)$-násobný $re^{\pm j\Omega}$ | $r^n\cos(n\Omega)(A_0 + A_1n + \dots + A_pn^p) + r^n\sin(n\Omega)(B_0 + B_1n + \dots + B_pn^p)$ |

**2) Partikulární řešení** závisí na tvaru vstupu (pravé strany):

| Pravá strana (buzení) | Tvar partikulárního řešení |
|---|---|
| konstanta $C_0$ | konstanta $C_1$ |
| $\alpha^n$ | $C\alpha^n$ |
| $\cos(n\Omega + \beta)$ | $C_1\cos(n\Omega) + C_2\sin(n\Omega)$, resp. $C\cos(n\Omega + \phi)$ |
| $\alpha^n\cos(n\Omega + \beta)$ | $\alpha^n[C_1\cos(n\Omega) + C_2\sin(n\Omega)]$ |
| $n$ | $C_0 + C_1n$ |
| $n^p$ | $C_0 + C_1n + \dots + C_pn^p$ |
| $n\,\alpha^n$ | $\alpha^n(C_0 + C_1n)$ |
| $n^p\alpha^n$ | $\alpha^n(C_0 + C_1n + \dots + C_pn^p)$ |
| $n\cos(n\Omega + \beta)$ | $(C_1 + C_2n)\cos(n\Omega) + (C_3 + C_4n)\sin(n\Omega)$ |

> **Pozor:** je-li základ $\alpha$ pravé strany zároveň **kořenem charakteristické rovnice** ($p$-násobným), musí se tvar partikulárního řešení **vynásobit $n^p$**.

**3) Postup:**
1. najdeme kořeny charakteristické rovnice → $y_H[n]$ s neznámými $K_i$,
2. dosazením předpokládaného tvaru do rovnice určíme konstanty $y_P[n]$,
3. **konstanty $K_i$ určíme z počátečních podmínek** – nejbezpečněji tak, že z rekurze spočítáme $y[0], y[1], \dots, y[N-1]$ a dosadíme do $y_H + y_P$.

**Interpretace:**
- pro **stabilní** systémy určuje $y_H$ **přechodový stav** (exponenciálně odezní),
- pro **harmonický vstup** určuje $y_P$ **ustálený stav**, který je také harmonický (se stejnou frekvencí).

### Řešené příklady

**Příklad 1.** Systém ze schématu: $y[n] = x[n] + 0{,}6y[n-1]$, tj. $y[n] - 0{,}6y[n-1] = x[n]$; vstup $x[n] = (0{,}4)^n$, $n \ge 0$; $y[-1] = 10$.

- charakteristická rovnice $z - 0{,}6 = 0 \Rightarrow z = 0{,}6$, $\;y_H = K(0{,}6)^n$,
- partikulární: $y_P = C(0{,}4)^n$; dosazením $C(0{,}4)^n - 0{,}6C(0{,}4)^{n-1} = (0{,}4)^n \Rightarrow C(1 - 1{,}5) = 1 \Rightarrow C = -2$,
- $y[0] = 0{,}6\cdot10 + 1 = 7 = K - 2 \Rightarrow K = 9$.

$$\boxed{y[n] = 9(0{,}6)^n - 2(0{,}4)^n,\quad n \ge 0}$$

*Kontrola:* $y[1] = 0{,}6\cdot7 + 0{,}4 = 4{,}6$; vzorec: $5{,}4 - 0{,}8 = 4{,}6$ ✓.

**Příklad 2.** $y[n] - 0{,}5y[n-1] = 5\cos(0{,}5n\pi)$, $n \ge 0$, $y[-1] = 4$.

- $y_H = K(0{,}5)^n$,
- $y_P = C_1\cos(0{,}5\pi n) + C_2\sin(0{,}5\pi n)$. Protože $\cos(0{,}5\pi(n-1)) = \sin(0{,}5\pi n)$ a $\sin(0{,}5\pi(n-1)) = -\cos(0{,}5\pi n)$, dosazením:
$$(C_1 + 0{,}5C_2)\cos(0{,}5\pi n) + (C_2 - 0{,}5C_1)\sin(0{,}5\pi n) = 5\cos(0{,}5\pi n)$$
$\Rightarrow C_2 = 0{,}5C_1$, $1{,}25C_1 = 5 \Rightarrow C_1 = 4$, $C_2 = 2$,
- $y[0] = 0{,}5\cdot4 + 5 = 7 = K + 4 \Rightarrow K = 3$.

$$\boxed{y[n] = 3(0{,}5)^n + 4\cos(0{,}5\pi n) + 2\sin(0{,}5\pi n) = \underbrace{3(0{,}5)^n}_{\text{přechodový stav}} + \underbrace{4{,}47\cos(0{,}5\pi n - 26{,}6^\circ)}_{\text{ustálený stav}}}$$

*Kontrola:* $y[1] = 0{,}5\cdot7 + 0 = 3{,}5$; vzorec: $1{,}5 + 0 + 2 = 3{,}5$ ✓.

**Příklad 3.** $y[n] - 0{,}5y[n-1] = 3(0{,}5)^n$, $n \ge 0$, $y[-1] = 2$.

- kořen $z = 0{,}5$ **je shodný se základem pravé strany** → $y_P = Cn(0{,}5)^n$; dosazením $Cn(0{,}5)^n - C(n-1)(0{,}5)^n = 3(0{,}5)^n \Rightarrow C = 3$,
- $y[0] = 0{,}5\cdot2 + 3 = 4 = K \Rightarrow K = 4$.

$$\boxed{y[n] = (4 + 3n)(0{,}5)^n,\quad n \ge 0}$$

*Kontrola:* $y[1] = 0{,}5\cdot4 + 1{,}5 = 3{,}5$; vzorec: $7\cdot0{,}5 = 3{,}5$ ✓.

**Příklad 4.** Systém ze schématu: $y[n] - \tfrac16y[n-1] - \tfrac16y[n-2] = 4x[n]$, $x[n] = u[n]$, $y[-1] = 1$, $y[-2] = 12$.

- $z^2 - \tfrac16z - \tfrac16 = 0 \Rightarrow z_1 = \tfrac12$, $z_2 = -\tfrac13$, $\;y_H = K_1(\tfrac12)^n + K_2(-\tfrac13)^n$,
- $y_P = C$: $C(1 - \tfrac16 - \tfrac16) = 4 \Rightarrow C = 6$,
- z rekurze: $y[0] = \tfrac16 + 2 + 4 = \tfrac{37}{6}$, $\;y[1] = \tfrac{37}{36} + \tfrac16 + 4 = \tfrac{187}{36}$,
- soustava: $K_1 + K_2 = \tfrac16$, $\;\tfrac12K_1 - \tfrac13K_2 = -\tfrac{29}{36}$ $\Rightarrow K_1 = -0{,}9$, $K_2 = \tfrac{16}{15}$.

$$\boxed{y[n] = -0{,}9\left(\tfrac12\right)^n + \tfrac{16}{15}\left(-\tfrac13\right)^n + 6,\quad n \ge 0}$$

*Kontrola:* $y[2]$ z rekurze $= \tfrac16\cdot\tfrac{187}{36} + \tfrac16\cdot\tfrac{37}{6} + 4 \approx 5{,}8935$; vzorec: $-0{,}225 + 0{,}1185 + 6 = 5{,}8935$ ✓.

---

## 4.5 Odezva při nulovém vstupu a při nulovém stavu (ZIR, ZSR)

Odezvu LTI systému lze zapsat jako součet:

- **ZSR (zero-state response)** $y_{zs}$ – odezva na zadaný vstup při **nulových počátečních podmínkách**,
- **ZIR (zero-input response)** $y_{zi}$ – odezva na **nulový vstup** při zadaných počátečních podmínkách (= homogenní řešení s konstantami určenými z počátečních podmínek).

$$y[n] = y_{zs}[n] + y_{zi}[n]$$

**Postup pro obecnou pravou stranu**
$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]:$$

1. najdeme **ZIR** – nulová pravá strana, zadané počáteční podmínky,
2. najdeme **ZSR pro jednoduchý vstup** $y_{zs0}[n]$ – rovnice s pravou stranou jen $x[n]$, nulové počáteční podmínky,
3. **superpozicí** (díky linearitě a časové invarianci):
$$y[n] = y_{zi}[n] + B_0\,y_{zs0}[n] + B_1\,y_{zs0}[n-1] + \dots + B_M\,y_{zs0}[n-M]$$

#### Řešený příklad

*Určete ZIR, ZSR a celkovou odezvu systému $y[n] - 0{,}6y[n-1] = (0{,}4)^n$, $n \ge 0$, $y[-1] = 10$.*

- **ZIR:** $y_{zi} = K(0{,}6)^n$; z $y_{zi}[-1] = K/0{,}6 = 10 \Rightarrow K = 6$: $\;y_{zi}[n] = 6(0{,}6)^n$.
- **ZSR:** $y_{zs} = K'(0{,}6)^n - 2(0{,}4)^n$ (partikulární řešení viz Příklad 1); při nulových počátečních podmínkách $y_{zs}[0] = x[0] = 1 \Rightarrow K' = 3$: $\;y_{zs}[n] = 3(0{,}6)^n - 2(0{,}4)^n$.
- **Celkem:** $y[n] = 9(0{,}6)^n - 2(0{,}4)^n$ – shoda s Příkladem 1 ✓.

---

## 4.6 Impulsní odezva diskrétního systému

> **Impulsní odezva $h[n]$** ustáleného (relaxovaného – s nulovými počátečními podmínkami) LTI systému je jeho **odezva na jednotkový impuls $\delta[n]$**. Impulsní odezva LTI systém **úplně popisuje**.

### 1) Nerekurzivní (FIR) filtr

Pro FIR filtr délky $M + 1$:
$$y[n] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]$$
dosadíme $x[n] = \delta[n]$:
$$h[n] = B_0\delta[n] + B_1\delta[n-1] + \dots + B_M\delta[n-M] \quad\Longrightarrow\quad \boxed{h[n] = \{B_0, B_1, B_2, \dots, B_M\}}$$

**Impulsní odezva FIR filtru je přímo posloupnost jeho koeficientů** (konečná – odtud „Finite Impulse Response“).

### 2) Rekurzivní systém s jedním vstupním členem

$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = x[n]$$

Řešíme $h[n] + A_1h[n-1] + \dots + A_Nh[n-N] = \delta[n]$. Protože $\delta[n] = 0$ pro $n > 0$, stačí najít **řešení homogenní rovnice** s počáteční podmínkou $h[0] = 1$ (pro vyšší řády navíc $h[-1] = h[-2] = \dots = 0$):

$$h[n] + A_1h[n-1] + \dots + A_Nh[n-N] = 0, \qquad h[0] = 1$$

### 3) Rekurzivní systém s obecnou pravou stranou

$$y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]$$

a) najdeme impulsní odezvu $h_0[n]$ systému s jednoduchou pravou stranou (bod 2),
b) superpozicí:
$$\boxed{h[n] = B_0h_0[n] + B_1h_0[n-1] + \dots + B_Mh_0[n-M]}$$

### Řešené příklady

**1.** $y[n] - 0{,}6y[n-1] = x[n]$: $\;h[n] = K(0{,}6)^n$, $h[0] = 1 \Rightarrow K = 1$:
$$h[n] = (0{,}6)^n\,u[n]$$

**2.** $y[n] - \tfrac16y[n-1] - \tfrac16y[n-2] = x[n]$: kořeny $\tfrac12$, $-\tfrac13$, $\;h[n] = K_1(\tfrac12)^n + K_2(-\tfrac13)^n$. Podmínky $h[0] = 1$, $h[1] = \tfrac16h[0] = \tfrac16$:
$K_1 + K_2 = 1$, $\tfrac12K_1 - \tfrac13K_2 = \tfrac16 \Rightarrow K_1 = 0{,}6$, $K_2 = 0{,}4$:
$$h[n] = \left[0{,}6\left(\tfrac12\right)^n + 0{,}4\left(-\tfrac13\right)^n\right]u[n]$$
*Kontrola:* $h[2] = \tfrac{1}{36} + \tfrac16 = \tfrac{7}{36} \approx 0{,}194$; vzorec $0{,}15 + 0{,}044 = 0{,}194$ ✓.

**3.** $y[n] - 0{,}6y[n-1] = 4x[n]$: $\;h[n] = 4(0{,}6)^n u[n]$.

$y[n] - 0{,}6y[n-1] = 3x[n-1] - x[n]$: s $h_0[n] = (0{,}6)^n u[n]$:
$$h[n] = 3h_0[n-1] - h_0[n] = 3(0{,}6)^{n-1}u[n-1] - (0{,}6)^n u[n] = -\delta[n] + 4(0{,}6)^n u[n-1]$$
(pro $n \ge 1$: $(0{,}6)^{n-1}(3 - 0{,}6) = 4(0{,}6)^n$; pro $n = 0$: $-1$). Ekvivalentně $h[n] = 4(0{,}6)^n u[n] - 5\delta[n]$.

---

## 4.7 Diskrétní konvoluce

- může být provedena v časové nebo ve frekvenční oblasti,
- používá se k určení odezvy ustáleného LTI systému,
- je založena na **linearitě a časové invarianci**.

**Odvození.** Známe impulsní odezvu $h[n]$. Vstup rozložíme na vážené posunuté impulsy (kap. 3):
$$x[n] = \sum_{k=-\infty}^{\infty} x[k]\,\delta[n-k]$$

Díky časové invarianci je odezva na $\delta[n-k]$ rovna $h[n-k]$; díky linearitě je odezva na $x[k]\delta[n-k]$ rovna $x[k]h[n-k]$ a odezva na součet je součtem odezev:

$$\boxed{y[n] = \sum_{k=-\infty}^{\infty} x[k]\,h[n-k] = x[n] * h[n]} \qquad\text{(konvoluční součet)}$$

> **Výstup LTI systému je konvolucí vstupu a impulsní odezvy systému.**

### Vlastnosti konvoluce

1. **Posun:** jsou-li $x[n]$ nebo $h[n]$ posunuty o $n_0$, posune se o $n_0$ i výstup:
$$x[n-n_0] * h[n] = x[n] * h[n-n_0] = y[n-n_0]$$
2. **Součet vzorků:** $\displaystyle\sum_n y[n] = \left(\sum_n x[n]\right)\left(\sum_n h[n]\right)$ (užitečná kontrola výpočtu!)
3. **Kauzalita:** pro kauzální systém ($h[n] = 0$ pro $n < 0$) a kauzální signál ($x[n] = 0$ pro $n < 0$) je i výstup kauzální a součet má konečné meze:
$$y[n] = \sum_{k=0}^{n} x[k]\,h[n-k] = \sum_{k=0}^{n} h[k]\,x[n-k]$$
4. **Komutativita:** $a[n] * b[n] = b[n] * a[n]$
5. **Asociativita:** $(a[n] * b[n]) * c[n] = a[n] * (b[n] * c[n])$
6. **Distributivita:** $a[n] * b[n] + a[n] * c[n] = a[n] * (b[n] + c[n])$
7. **Neutrální prvek:** $x[n] * \delta[n] = x[n]$, $\;x[n] * \delta[n - n_0] = x[n - n_0]$.

### Konvoluce konečných posloupností

Konvoluce dvou konečných posloupností je opět konečná a platí:
1. **počáteční index** $y[n]$ = součet počátečních indexů $x[n]$ a $h[n]$,
2. **koncový index** $y[n]$ = součet koncových indexů,
3. **délka:** $L_y = L_x + L_h - 1$.

### Metody výpočtu konvoluce

**a) Metoda sčítání po sloupcích**
1. zapíšeme $x[n]$ pod $h[n]$,
2. každým vzorkem $x[n]$ vynásobíme celou posloupnost $h[n]$ a výsledek zapíšeme do řádku posunutého na pozici daného vzorku,
3. sečteme sloupce.

*Příklad:* $h[n] = \{\underset{\uparrow}{1}, 2, 2, 3\}$, $x[n] = \{\underset{\uparrow}{2}, -1, 3\}$:

| | | | | | | |
|---|---|---|---|---|---|---|
| $h[n]$ | 1 | 2 | 2 | 3 | | |
| $x[n]$ | 2 | −1 | 3 | | | |
| $2\cdot h$ | 2 | 4 | 4 | 6 | | |
| $-1\cdot h$ | | −1 | −2 | −2 | −3 | |
| $3\cdot h$ | | | 3 | 6 | 6 | 9 |
| **$y[n]$** | **2** | **3** | **5** | **10** | **3** | **9** |

$$y[n] = \{\underset{\uparrow}{2}, 3, 5, 10, 3, 9\} = 2\delta[n] + 3\delta[n-1] + 5\delta[n-2] + 10\delta[n-3] + 3\delta[n-4] + 9\delta[n-5]$$

*Kontrola součtem:* $\sum y = 32 = (1+2+2+3)(2-1+3) = 8\cdot4$ ✓. (Na slidu jsou v pomocném řádku a součtu sloupců překlepy, výsledek je správný.)

**b) Metoda posuvného proužku (sliding strip)**
1. **překlopíme** $x[n]$ a posuneme ho tak, aby jeho poslední vzorek ležel proti prvnímu vzorku $h[n]$,
2. postupně posouváme překlopený $x$ doprava, násobíme protilehlé vzorky a sčítáme.

*Příklad:* $h[n] = \{\underset{\uparrow}{2}, 5, 0, 4\}$, $x[n] = \{\underset{\uparrow}{4}, 1, 3\}$ (překlopeno: 3 1 4):
$$y[0] = 4\cdot2 = 8,\quad y[1] = 1\cdot2 + 4\cdot5 = 22,\quad y[2] = 3\cdot2 + 1\cdot5 + 4\cdot0 = 11,$$
$$y[3] = 3\cdot5 + 1\cdot0 + 4\cdot4 = 31,\quad y[4] = 3\cdot0 + 1\cdot4 = 4,\quad y[5] = 3\cdot4 = 12$$
$$y[n] = \{\underset{\uparrow}{8}, 22, 11, 31, 4, 12\}$$

**c) Metoda násobení polynomů**

Konvoluce dvou konečných řad je **ekvivalentní násobení dvou polynomů**, jejichž koeficienty jsou hodnoty řad:
$$h(z) = 2z^3 + 5z^2 + 0z + 4,\quad x(z) = 4z^2 + 1z + 3$$
$$y(z) = h(z)\cdot x(z) = 8z^5 + 22z^4 + 11z^3 + 31z^2 + 4z + 12 \;\Rightarrow\; y[n] = \{8, 22, 11, 31, 4, 12\}$$

(To je jádro Z-transformace: konvoluce v čase ↔ součin v transformované oblasti – kap. 6.)

### Propojení systémů

**Kaskádní (sériové) spojení:**
$$y_1[n] = x[n] * h_1[n], \quad y[n] = y_1[n] * h_2[n] = (x[n] * h_1[n]) * h_2[n] = x[n] * (h_1[n] * h_2[n])$$
$$\boxed{h[n] = h_1[n] * h_2[n] * \dots * h_n[n]}$$

**Paralelní spojení:**
$$y[n] = y_1[n] + y_2[n] = x[n] * h_1[n] + x[n] * h_2[n] = x[n] * (h_1[n] + h_2[n])$$
$$\boxed{h[n] = h_1[n] + h_2[n] + \dots + h_n[n]}$$

---

## 4.8 Odezva na periodický vstup a periodická konvoluce

> Odezva diskrétního LTI systému na **periodický vstup** s periodou $N$ je také **periodická se stejnou periodou**.

*Příklad:* $x[n] = \{\underset{\uparrow}{1}, 2, -3, 1, 2, -3, \dots\}$ (pro $n \ge 0$), $h[n] = \{\underset{\uparrow}{1}, 1\}$:

| $n$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | … |
|---|---|---|---|---|---|---|---|---|
| $y[n]$ | 1 | 3 | −1 | −2 | 3 | −1 | −2 | … |

$y[n]$ je periodický s periodou 3; jen **první vzorek** nemá „ustálenou“ hodnotu (signál začíná v $n = 0$, chybí mu předchozí perioda). Ustálenou periodu lze spočítat zkráceně: **spočítáme lineární konvoluci jedné periody s $h[n]$ a přesahující vzorky přičteme na začátek**:

$$\{1, 2, -3\} * \{1, 1\} = \{1, 3, -1, -3\} \;\to\; \{1 + (-3), 3, -1\} = \{-2, 3, -1\}$$

### Periodická (cyklická) konvoluce

Konvoluce dvou periodických signálů se **stejnou periodou $N$**; výsledek je periodický se stejnou periodou:

$$y_p[n] = x_p[n] \circledast h_p[n] = \sum_{k=0}^{N-1} x_p[k]\,h_p[(n-k) \bmod N]$$

**Postup (metoda „zabalení“):**
1. spočítáme **lineární konvoluci** jedné periody obou signálů,
2. výsledek rozdělíme na úseky délky $N$ a ty **sečteme**.

*Příklad:* $x_p[n] = \{\underset{\uparrow}{1}, 0, 1, 1\}$, $h_p[n] = \{\underset{\uparrow}{1}, 2, 3, 1\}$, $N = 4$:

lineární konvoluce: $\{1, 2, 4, 4, 5, 4, 1\}$

| index $n$ | 0 | 1 | 2 | 3 |
|---|---|---|---|---|
| první úsek | 1 | 2 | 4 | 4 |
| „zabalený“ zbytek | 5 | 4 | 1 | 0 |
| **periodická konvoluce $y_p[n]$** | **6** | **6** | **5** | **4** |

**Maticový výpočet – cirkulantní matice.** Vytvoříme matici $\mathbf{C}_x$, jejíž sloupce obsahují **cyklicky posunuté verze** signálu $x$:

$$\mathbf{C}_x = \begin{bmatrix} x[0] & x[N-1] & \cdots & x[1] \\ x[1] & x[0] & \cdots & x[2] \\ \vdots & & \ddots & \vdots \\ x[N-1] & x[N-2] & \cdots & x[0]\end{bmatrix}, \qquad \mathbf{y}_p = \mathbf{C}_x\,\mathbf{h}$$

*Příklad:* $x_p = \{1, 0, 2\}$, $h_p = \{1, 2, 3\}$:
$$\mathbf{y}_p = \begin{bmatrix}1 & 2 & 0\\0 & 1 & 2\\2 & 0 & 1\end{bmatrix}\begin{bmatrix}1\\2\\3\end{bmatrix} = \begin{bmatrix}5\\8\\5\end{bmatrix}$$

(Periodická konvoluce je úzce spjata s DFT – součin DFT dvou posloupností odpovídá jejich cyklické konvoluci, kap. 5.)

---

## 4.9 Dekonvoluce (identifikace systému)

Známe-li $h[n]$, určíme výstup $y[n] = h[n] * x[n]$. **Známe-li $x[n]$ a $y[n]$, jak určit $h[n]$?** → **dekonvolucí**.

### 1) Rekurzivní výpočet

Pro kauzální signály $y[n] = \sum_{k=0}^{n} h[k]\,x[n-k]$. Pro $n = 0$: $y[0] = x[0]h[0] \Rightarrow h[0] = \dfrac{y[0]}{x[0]}$.
Pro $n > 0$ oddělíme člen s $k = n$:
$$y[n] = h[n]\,x[0] + \sum_{k=0}^{n-1}h[k]\,x[n-k] \quad\Longrightarrow\quad \boxed{h[n] = \frac{1}{x[0]}\left[y[n] - \sum_{k=0}^{n-1}h[k]\,x[n-k]\right]}$$

Vyhodnocujeme $M - N + 1$ bodů $h[n]$ ($M$, $N$ jsou délky $y$ a $x$). Metoda se používá zřídka – **je citlivá na šum** (chyby se rekurzí kumulují).

### 2) Dělení polynomů

Vstup a výstup chápeme jako polynomy; $h$ získáme jako podíl $y(w)/x(w)$.

*Příklad:* $x[n] = \{\underset{\uparrow}{2}, 5, 0, 4\}$, $y[n] = \{\underset{\uparrow}{8}, 22, 11, 31, 4, 12\}$. Určete $h[n]$.

*Rekurzivně* ($M - N + 1 = 3$ body):
$$h[0] = \tfrac{8}{2} = 4,\quad h[1] = \tfrac{22 - 4\cdot5}{2} = 1,\quad h[2] = \tfrac{11 - (4\cdot0 + 1\cdot5)}{2} = 3$$

*Dělením polynomů:* $(8z^5 + 22z^4 + 11z^3 + 31z^2 + 4z + 12) : (2z^3 + 5z^2 + 4) = 4z^2 + z + 3$.

$$h[n] = \{\underset{\uparrow}{4}, 1, 3\}$$

(Srovnej s příkladem na konvoluci výše.)

---

## 4.10 Diskrétní korelace

**Korelace vyjadřuje míru podobnosti mezi dvěma signály.** Diskrétní **vzájemná korelace** (cross-correlation) $x[n]$ a $h[n]$:

$$r_{xh}[n] = x[n] \star\star h[n] = \sum_{k=-\infty}^{\infty}x[k]\,h[k-n] = \sum_{k=-\infty}^{\infty}x[k+n]\,h[k]$$
$$r_{hx}[n] = h[n] \star\star x[n] = \sum_{k=-\infty}^{\infty}h[k]\,x[k-n] = \sum_{k=-\infty}^{\infty}h[k+n]\,x[k]$$

Korelace **není komutativní**: $r_{hx}[n] = r_{xh}[-n]$.

**Vztah mezi konvolucí a korelací** – korelace je konvoluce s časově otočeným signálem:
$$r_{xh}[n] = x[n] * h[-n], \qquad r_{hx}[n] = h[n] * x[-n]$$
$$N_r = N_x + N_h - 1, \qquad \sum r[n] = \left(\sum x[n]\right)\left(\sum h[n]\right)$$

*Příklad:* $x[n] = \{\underset{\uparrow}{2}, 5, 0, 4\}$, $h[n] = \{\underset{\uparrow}{3}, 1, 4\}$:
$h[-n] = \{4, 1, \underset{\uparrow}{3}\}$, $\;r_{xh}[n] = x[n] * h[-n] = \{8, 22, \underset{\uparrow}{11}, 31, 4, 12\}$ (začíná v $n = -2$).
*Kontrola:* $r_{xh}[0] = \sum_k x[k]h[k] = 2\cdot3 + 5\cdot1 + 0\cdot4 = 11$ ✓.

### Autokorelace

**Autokorelace** je vzájemná korelace signálu se sebou samým:
$$r_{xx}[n] = x[n] \star\star x[n] = x[n] * x[-n]$$

Vlastnosti:
- je to **sudá funkce**: $r_{xx}[n] = r_{xx}[-n]$,
- má **maximum v $n = 0$**: $r_{xx}[n] \le r_{xx}[0]$, přičemž $r_{xx}[0] = \sum x^2[n] = E$ (energie signálu).

*Příklad:* $x[n] = \{2, 5, 0, 4\}$: $r_{xx}[n] = \{8, 20, 10, \underset{\uparrow}{45}, 10, 20, 8\}$, $r_{xx}[0] = 4 + 25 + 0 + 16 = 45$ ✓.

**Využití:** autokorelace se často používá k **detekci signálu skrytého v šumu**. Šum je nekorelovaný se signálem (a sám se sebou pro nenulové posuny) → je-li v datech přítomen užitečný (zvl. periodický) signál, projeví se v autokorelaci výraznými vrcholy, kdežto šum přispívá hlavně do ostrého vrcholu v $n = 0$.

### Periodická korelace / autokorelace

Pro periodické řady se stejnou periodou $N$:
$$r_{xhp}[n] = x[n] \circledast\circledast h[n] = \sum_{k=0}^{N-1}x[k]\,h[k-n], \qquad r_{hxp}[n] = \sum_{k=0}^{N-1}h[k]\,x[k-n]$$
(indexy modulo $N$). Výpočet je obdobný jako u periodické konvoluce.

### Aplikace korelace

**Detekce cíle (radar).** Radar vyšle dotazovací signál $x[n]$. Signál odražený od cíle se vrací zpět a radar přijímá
$$s[n] = \alpha\,x[n - D] + p[n],$$
kde $D$ je zpoždění (úměrné vzdálenosti cíle) a $p[n]$ šum. **Korelační přijímač** (přizpůsobený filtr – matched filter, jehož impulsní odezva je časově otočený vyslaný puls) počítá korelaci $s[n]$ s $x[n]$; **poloha maxima korelace určuje zpoždění $D$**, a to i když je přijatý signál v šumu „okem“ neviditelný.

**Určení periodicity zašuměného signálu a jeho rekonstrukce.** Periodický signál $x[n]$ je poškozen nekorelovaným šumem: $s[n] = x[n] + p[n]$. Postup:
1. **určíme periodu $N$** – v (periodické) autokorelaci $s[n]$ se objevují **vrcholy v násobcích periody**,
2. **signál $x[n]$ odhadneme jako periodickou vzájemnou korelaci $s[n]$ s řadou impulsů** $i[n] = \sum_k \delta[n - kN]$ s periodou $N$ – to v podstatě odpovídá **průměrování jednotlivých period** (koherentní průměrování, kap. 3), šum se tím potlačí.

---

## 4.11 Inverzní systémy

Používají se ke **korekci poruch** (zkreslení), které vznikají např. během měření nebo přenosu signálu:

```
x[n] ──► [ Systém ] ──► y[n] ──► [ Inverzní systém ] ──► x[n]
```

K nalezení inverzního systému stačí **zaměnit vstupy a výstupy v diferenční rovnici**. Impulsní odezva inverzního systému se pak určí z nové diferenční rovnice. Platí $h[n] * h_{inv}[n] = \delta[n]$.

*Příklad:*
- systém: $y[n] + 2y[n-1] = 3x[n] + 4x[n-1]$,
- inverzní systém: $3y[n] + 4y[n-1] = x[n] + 2x[n-1]$.

Aby byl systém **invertibilní**, musí různým vstupům odpovídat různé výstupy (zobrazení vstup→výstup musí být **prosté**). Např. $y[n] = \cos(x[n])$ invertibilní **není** ($x$ a $x + 2\pi$ dávají stejný výstup).

(Poznámka: inverzí FIR systému vzniká IIR systém; aby byl inverzní systém stabilní, musí mít původní systém nuly uvnitř jednotkové kružnice – viz kap. 6.)

---

## Shrnutí kapitoly 4

- Linearita (superpozice), časová invariance, kauzalita, statičnost; LTI test z diferenční rovnice.
- Bloky: $z^{-1}$, násobička, sčítačka; přímá forma I (2 linky) × **přímá forma II** (1 linka, kanonická).
- Diferenční rovnice: ARMA, MA (FIR), AR (IIR); řešení rekurzí nebo $y_H + y_P$ (kořeny charakteristické rovnice, tabulky tvarů), konstanty z počátečních podmínek; $y = y_{zi} + y_{zs}$.
- Impulsní odezva: FIR → koeficienty; IIR → homogenní řešení s $h[0] = 1$; obecně $h = \sum B_k h_0[n-k]$.
- **Konvoluce** $y = x * h$; délka $L_x + L_h - 1$; metody: sloupce, posuvný proužek, polynomy; kaskáda → konvoluce, paralela → součet.
- Periodická konvoluce (zabalení, cirkulantní matice); dekonvoluce (rekurze, dělení polynomů).
- **Korelace** $r_{xh}[n] = x[n] * h[-n]$, autokorelace sudá s maximem v 0; radar, detekce periodicity.
- Inverzní systém: záměna $x \leftrightarrow y$ v rovnici; nutná prostota zobrazení.

### Kontrolní otázky
1. Rozhodněte o linearitě, časové invarianci a kauzalitě: $y[n] = x[n]\,x[n-1]$; $y[n] = n\,x[n-1]$; $y[n] = x[-n]$.
2. Nakreslete přímou formu I a II pro $y[n] = 0{,}5y[n-1] - 0{,}2y[n-2] + x[n] + 2x[n-1]$.
3. Najděte impulsní odezvu $y[n] - 0{,}5y[n-1] = x[n] + x[n-1]$. *(Řešení: $h[n] = (0{,}5)^n u[n] + (0{,}5)^{n-1}u[n-1] = \delta[n] + 3(0{,}5)^n u[n-1]$.)*
4. Spočítejte $\{1, 2, 3\} * \{1, -1\}$ třemi metodami a ověřte součtem vzorků. *(Řešení: $\{1, 1, 1, -3\}$.)*
5. Spočítejte periodickou konvoluci $\{1, 2, 3\} \circledast \{1, 0, 1\}$. *(Řešení: lineární $\{1, 2, 4, 2, 3\}$ → $\{1+2, 2+3, 4\} = \{3, 5, 4\}$.)*
6. Jaký je vztah mezi korelací a konvolucí? Proč je autokorelace sudá?
7. Jak radar určí vzdálenost cíle pomocí korelace?

---

# 5. Analýza signálů ve frekvenční oblasti – Fourierova transformace

## 5.1 Základní idea transformace

```
 x[n] ──► [Transformace] ──► X
  │                          │
  ▼                          ▼
Zpracování v            Zpracování v
časové oblasti          transformované oblasti
  │                          │
  ▼                          ▼
 x'[n] ◄── [Inverzní transformace] ◄── X'
```

Signál lze zpracovat přímo v časové oblasti, nebo ho **transformovat** (např. do frekvenční oblasti), zpracovat tam (některé operace jsou zde mnohem jednodušší – např. konvoluce se mění v násobení) a **inverzní transformací** se vrátit zpět.

---

## 5.2 Typy Fourierových transformací

Podle toho, zda je signál spojitý/diskrétní a periodický/neperiodický, rozlišujeme:

| Operace v časové oblasti | Výsledek ve frekvenční oblasti | Transformace |
|---|---|---|
| neperiodický, spojitý $x(t)$ | neperiodické, spojité $X(f)$ | **FT** (Fourierova transformace) |
| periodické rozšíření $x(t) \Rightarrow x_p(t)$, perioda $T$ | vzorkování $X(f) \Rightarrow X[k]$, interval $1/T = f_0$ | **FS** (Fourierova řada) |
| vzorkování $x_p(t) \Rightarrow x_p[n]$, interval $t_s$ | periodické rozšíření $X[k] \Rightarrow X_{DFS}[k]$, perioda $S = 1/t_s$ | **DFS** (diskrétní Fourierova řada) |
| vzorkování $x(t) \Rightarrow x[n]$, interval 1 | periodické rozšíření $X(f) \Rightarrow X(F)$, perioda 1 | **DTFT** |
| periodické rozšíření $x[n] \Rightarrow x_p[n]$, perioda $N$ | vzorkování $X(F) \Rightarrow X_{DFT}[k]$, interval $1/N$ | **DFT** |

Zjednodušeně:

| Signál | Spektrum |
|---|---|
| neperiodický spojitý | neperiodické spojité (FT) |
| periodický spojitý | **diskrétní** (čárové) spektrum (FS) |
| **diskrétní** neperiodický | **periodické** spojité spektrum (DTFT) |
| diskrétní a periodický | diskrétní a periodické (DFT) |

> **Klíčová dualita:** **vzorkování v jedné oblasti ↔ periodické rozšíření ve druhé oblasti** (a naopak). Diskrétní signál má periodické spektrum; periodický signál má diskrétní spektrum.

---

## 5.3 DTFT – Fourierova transformace diskrétního signálu (Discrete-Time Fourier Transform)

### Definice

DTFT lze stanovit ze Z-transformace (kap. 6):
$$X(z) = \sum_{k=-\infty}^{\infty}x[k]\,z^{-k}, \qquad z \text{ je komplexní číslo } z = |r|e^{j2\pi F} = |r|e^{j\Omega}$$

Pro $r = 1$ (tj. na **jednotkové kružnici** $z = e^{j2\pi F}$):

$$\boxed{X(F) = \sum_{k=-\infty}^{\infty}x[k]\,e^{-j2\pi kF}} \qquad\text{nebo}\qquad \boxed{X(\Omega) = \sum_{k=-\infty}^{\infty}x[k]\,e^{-jk\Omega}}$$

> **$X(F)$ je Z-transformace signálu vyhodnocená na jednotkové kružnici** a reprezentuje diskrétní signál ve frekvenční oblasti.

**Vztah mezi Z-transformací a DTFT:**
- **Z → DTFT:** pro signály, které jsou „absolutně sčitatelné“ ($\sum|x[n]| < \infty$, oblast konvergence obsahuje jednotkovou kružnici), stačí **nahradit $z$ výrazem $e^{j2\pi F}$**,
- **DTFT → Z:** odstranit impulsní členy (typu $\delta(F)$) a nahradit $e^{j2\pi F}$ proměnnou $z$.

### Periodicita DTFT

$$X(F+1) = \sum_k x[k]e^{-j2\pi k(F+1)} = \sum_k x[k]\underbrace{e^{-j2\pi k}}_{=1}e^{-j2\pi kF} = X(F)$$

> **$X(F)$ je periodická v $F$ s jednotkovou periodou** (resp. $X(\Omega)$ s periodou $2\pi$). Stačí ji tedy znát na intervalu **$-0{,}5 \le F \le 0{,}5$** (nebo $0 \le F \le 1$) – tzv. **základní (centrální) perioda**. (Souvisí s tím, že diskrétní sinusoidy s frekvencemi $F$ a $F+1$ jsou totožné – kap. 3.)

| | F-forma | Ω-forma |
|---|---|---|
| základní perioda | $-0{,}5 \le F \le 0{,}5$ nebo $0 \le F \le 1$ | $-\pi \le \Omega \le \pi$ nebo $0 \le \Omega \le 2\pi$ |

### Inverzní DTFT

$$x[n] = \int_{-1/2}^{1/2}X(F)\,e^{j2\pi nF}\,dF \quad(\text{F-forma}), \qquad x[n] = \frac{1}{2\pi}\int_{-\pi}^{\pi}X(\Omega)\,e^{jn\Omega}\,d\Omega \quad(\Omega\text{-forma})$$

(Integruje se přes jednu periodu – stejně jako se u Fourierovy řady počítají koeficienty z jedné periody.)

### Vlastnosti DTFT

| Vlastnost | Signál | F-forma | Ω-forma |
|---|---|---|---|
| otočení (folding) | $x[-n]$ | $X(-F)$ ($= X^*(F)$ pro reálné $x$) | $X(-\Omega)$ |
| časový posun | $x[n-m]$ | $e^{-j2\pi mF}X(F)$ | $e^{-j\Omega m}X(\Omega)$ |
| frekvenční posun | $e^{j2\pi nF_0}x[n]$ | $X(F - F_0)$ | $X(\Omega - \Omega_0)$ |
| posun o půl periody | $(-1)^n x[n]$ | $X(F - 0{,}5)$ | $X(\Omega - \pi)$ |
| modulace | $\cos(2\pi nF_0)\,x[n]$ | $0{,}5[X(F+F_0) + X(F-F_0)]$ | $0{,}5[X(\Omega+\Omega_0) + X(\Omega-\Omega_0)]$ |
| **konvoluce** | $x[n] * y[n]$ | $X(F)\,Y(F)$ | $X(\Omega)\,Y(\Omega)$ |
| **součin** | $x[n]\,y[n]$ | $X(F) \circledast Y(F)$ (periodická konvoluce) | $\frac{1}{2\pi}[X(\Omega) \circledast Y(\Omega)]$ |
| násobení $n$ | $n\,x[n]$ | $\dfrac{j}{2\pi}\dfrac{dX(F)}{dF}$ | $j\dfrac{dX(\Omega)}{d\Omega}$ |
| **Parsevalova věta** | $\sum_k x^2[k]$ | $= \int_1 \lvert X(F)\rvert^2 dF$ | $= \frac{1}{2\pi}\int_{2\pi}\lvert X(\Omega)\rvert^2 d\Omega$ |
| centrální hodnoty | $x[0] = \int_1 X(F)\,dF$ | $X(0) = \sum_n x[n]$ | $X(F)\rvert_{F=0{,}5} = X(\Omega)\rvert_{\Omega=\pi} = \sum_n(-1)^n x[n]$ |

> Nejdůležitější: **konvoluce v čase = součin ve frekvenci**. Proto se výstup LTI systému ve frekvenční oblasti počítá prostým násobením $Y(F) = X(F)H(F)$.
> **Posun v čase = lineární fáze** ve frekvenci (modul se nemění).

### Základní transformační dvojice DTFT (pro $|\alpha| < 1$)

| | Signál $x[n]$ | $X(F)$ | $X(\Omega)$ |
|---|---|---|---|
| 1 | $\delta[n]$ | $1$ | $1$ |
| 2 | $\alpha^n u[n]$ | $\dfrac{1}{1 - \alpha e^{-j2\pi F}}$ | $\dfrac{1}{1 - \alpha e^{-j\Omega}}$ |
| 3 | $n\alpha^n u[n]$ | $\dfrac{\alpha e^{-j2\pi F}}{(1 - \alpha e^{-j2\pi F})^2}$ | $\dfrac{\alpha e^{-j\Omega}}{(1 - \alpha e^{-j\Omega})^2}$ |
| 4 | $(n+1)\alpha^n u[n]$ | $\dfrac{1}{(1 - \alpha e^{-j2\pi F})^2}$ | $\dfrac{1}{(1 - \alpha e^{-j\Omega})^2}$ |
| 5 | $\alpha^{\lvert n\rvert}$ | $\dfrac{1 - \alpha^2}{1 - 2\alpha\cos(2\pi F) + \alpha^2}$ | $\dfrac{1 - \alpha^2}{1 - 2\alpha\cos\Omega + \alpha^2}$ |
| 6 | $1$ | $\delta(F)$ | $2\pi\delta(\Omega)$ |
| 7 | $\cos(2\pi nF_0) = \cos(n\Omega_0)$ | $0{,}5[\delta(F+F_0) + \delta(F-F_0)]$ | $\pi[\delta(\Omega+\Omega_0) + \delta(\Omega-\Omega_0)]$ |
| 8 | $\sin(2\pi nF_0) = \sin(n\Omega_0)$ | $j0{,}5[\delta(F+F_0) - \delta(F-F_0)]$ | $j\pi[\delta(\Omega+\Omega_0) - \delta(\Omega-\Omega_0)]$ |
| 9 | $2F_C\,\mathrm{sinc}(2nF_C) = \dfrac{\sin(n\Omega_C)}{n\pi}$ | $\mathrm{rect}\!\left(\dfrac{F}{2F_C}\right)$ | $\mathrm{rect}\!\left(\dfrac{\Omega}{2\Omega_C}\right)$ |
| 10 | $u[n]$ | $0{,}5\delta(F) + \dfrac{1}{1 - e^{-j2\pi F}}$ | $\pi\delta(\Omega) + \dfrac{1}{1 - e^{-j\Omega}}$ |

(Dvojice 9 je **impulsní odezva ideální dolní propusti** s mezní frekvencí $F_C$ – nekonečně dlouhá a nekauzální; východisko návrhu FIR filtrů metodou oken, kap. 8.)

*Odvození dvojice 2:* $\sum_{n=0}^{\infty}\alpha^n e^{-j2\pi nF} = \sum_{n=0}^{\infty}(\alpha e^{-j2\pi F})^n = \dfrac{1}{1 - \alpha e^{-j2\pi F}}$ (geometrická řada, konverguje pro $|\alpha| < 1$).

### Symetrie DTFT reálného signálu

DTFT reálného signálu je obecně **komplexní** → popisujeme ji **amplitudovým spektrem** (modul, zisk, *gain*) a **fázovým spektrem**:

$$X(F) = R(F) + jI(F) = |X(F)|\,e^{j\phi(F)} = |X(F)|\angle\phi(F)$$

Pro **reálný** signál platí **komplexně sdružená symetrie**:

$$X(F) = X^*(-F), \qquad |X(F)| = |X(-F)| \;(\text{modul je sudý}), \qquad \phi(F) = -\phi(-F) \;(\text{fáze je lichá})$$

Díky periodicitě je modul navíc sudě symetrický i kolem $F = 0{,}5$ a fáze lichá kolem $F = 0{,}5$. **Pro reálný signál proto stačí znát spektrum na intervalu $0 \le F \le 0{,}5$.**

### DTFT periodického diskrétního signálu

**Vzorkování v časové oblasti se transformuje jako periodické rozšíření ve frekvenční oblasti a naopak.** Pro periodický signál s periodou $N$ je tedy spektrum **diskrétní** (impulsy v násobcích $F_0 = 1/N$):

$$X(F) = \frac{1}{N}\sum_{k=0}^{N-1}X_1(kF_0)\,\delta(F - kF_0) \qquad (\text{přes jednu periodu } 0 \le F < 1)$$

kde $X_1(F)$ je DTFT **jedné periody** signálu.

#### Řešený příklad
*$x[n] = \{3, 2, 1, 2\}$ je periodický signál s periodou $N = 4$.*

$X_1(F) = 3 + 2e^{-j2\pi F} + e^{-j4\pi F} + 2e^{-j6\pi F}$, vyhodnoceno v $F = k/4$:
$X_1(0) = 8$, $X_1(\tfrac14) = 3 - 2j - 1 + 2j = 2$, $X_1(\tfrac12) = 3 - 2 + 1 - 2 = 0$, $X_1(\tfrac34) = 2$.

$$X(F) = \tfrac14[8\delta(F) + 2\delta(F - \tfrac14) + 0 + 2\delta(F - \tfrac34)] = 2\delta(F) + 0{,}5\delta(F - \tfrac14) + 0{,}5\delta(F - \tfrac34)$$

### DFT a DFS jako vzorkovaná DTFT

**DFT** diskrétního signálu je definována jako **vzorkovaná DTFT** (jedné periody $x_1[n]$) v bodech $F = kF_0 = k/N$:
$$X_{DFT}[k] = X_1(F)\big|_{F = k/N} = \sum_{n=0}^{N-1}x_1[n]\,e^{-j2\pi nk/N}, \qquad k = 0, 1, \dots, N-1$$

**DFS** $N$-vzorkového signálu je DFT s koeficienty **dělenými délkou periody $N$**:
$$X_{DFS}[k] = \frac{1}{N}X_{DFT}[k] = \frac{1}{N}\sum_{n=0}^{N-1}x_1[n]\,e^{-j2\pi nk/N}$$

---

## 5.4 Frekvenční odezva systému

```
Vstup x[n] → [systém, impulsní odezva h[n]] → výstup y[n] = x[n] * h[n]
    ⇕                     ⇕                              ⇕
Vstup X(F) → [systém, přenos H(F)]          → výstup Y(F) = X(F)·H(F)
```

$$H(F) = \frac{Y(F)}{X(F)} \qquad\text{nebo}\qquad H(\Omega) = \frac{Y(\Omega)}{X(\Omega)}$$

Pro systém popsaný diferenční rovnicí $y[n] + A_1y[n-1] + \dots + A_Ny[n-N] = B_0x[n] + \dots + B_Mx[n-M]$ (díky vlastnosti časového posunu):

$$\boxed{H(F) = \frac{Y(F)}{X(F)} = \frac{B_0 + B_1e^{-j2\pi F} + \dots + B_Me^{-j2\pi MF}}{1 + A_1e^{-j2\pi F} + \dots + A_Ne^{-j2\pi NF}}}$$

> **$H(F)$ je frekvenční odezva systému** (ustálená přenosová funkce). **$H(F)$ je DTFT impulsní odezvy $h[n]$.** Její modul $|H(F)|$ je **amplitudová (modulová) frekvenční charakteristika**, argument $\phi(F)$ je **fázová charakteristika**.

### Analýza systémů pomocí DTFT

1. Určení odezvy systému popsaného diferenční rovnicí nebo přenosovou funkcí – podobně jako pomocí Z-transformace (kap. 6).
2. **Určení ustálené odezvy na sinusový vstup** („sinus fidelity“ z kap. 2):

$$x[n] = A\cos(2\pi nF_0 + \theta) \quad\Longrightarrow\quad y_{ss}[n] = A\,H_0\cos(2\pi nF_0 + \theta + \phi_0)$$

kde $H_0 = |H(F_0)|$ a $\phi_0 = \arg H(F_0)$ – **frekvenční odezvu vyhodnotíme na frekvenci vstupu**: amplituda se vynásobí modulem, k fázi se přičte fáze.

#### Řešený příklad
*Systém $y[n] - 0{,}5y[n-1] = x[n]$; najděte ustálenou odezvu na $x[n] = 10\cos(0{,}5n\pi + 60^\circ)$.*

$H(F) = \dfrac{1}{1 - 0{,}5e^{-j2\pi F}}$; vstup má $2\pi F_0 = 0{,}5\pi \Rightarrow F_0 = 0{,}25$:

$$H(0{,}25) = \frac{1}{1 - 0{,}5e^{-j\pi/2}} = \frac{1}{1 + 0{,}5j} = 0{,}4\sqrt5\,\angle{-26{,}6^\circ} = 0{,}8944\,\angle{-26{,}6^\circ}$$

$$y_{ss}[n] = 10\cdot0{,}8944\cos(0{,}5n\pi + 60^\circ - 26{,}6^\circ) = \mathbf{8{,}944\cos(0{,}5n\pi + 33{,}4^\circ)}$$

---

## 5.5 Diskrétní Fourierova transformace (DFT)

### Definice

**$N$-bodová DFT** signálu s $N$ vzorky:
$$\boxed{X_{DFT}[k] = \sum_{n=0}^{N-1}x[n]\,e^{-j2\pi nk/N} = \sum_{n=0}^{N-1}x[n]\left[\cos\!\left(\frac{2\pi nk}{N}\right) - j\sin\!\left(\frac{2\pi nk}{N}\right)\right], \quad k = 0, 1, \dots, N-1}$$

**Zpětná transformace (IDFT):**
$$\boxed{x[n] = \frac{1}{N}\sum_{k=0}^{N-1}X_{DFT}[k]\,e^{j2\pi nk/N}, \qquad n = 0, 1, \dots, N-1}$$

Protože $e^{j2\pi nk/N}$ je periodická (v $n$ i $k$ s periodou $N$), jsou **DFT i IDFT periodické** – počítáme proto vzorky jen přes jednu periodu. DFT tak implicitně předpokládá, že analyzovaný úsek signálu se **periodicky opakuje** (důležité pro pochopení prosakování!).

DFT a IDFT mají podobný tvar, liší se jen **znaménkem v exponentu a normalizačním faktorem $1/N$**.

### Frekvenční osa DFT

DFT se počítá z úseku signálu délky $T_0 = NT$ ($T = t_s$ perioda vzorkování). Vzorky DFT lze vynášet proti:

| osa | hodnota pro index $k$ | krok |
|---|---|---|
| index | $k = 0, 1, \dots, N-1$ | 1 |
| analogová frekvence | $f = k\,\dfrac{S}{N}$ | $\Delta f = f_0 = \dfrac{S}{N} = \dfrac{1}{NT}$ |
| číslicová frekvence | $F = \dfrac{k}{N}$ | $1/N$ |
| analogová úhlová | $\omega = 2\pi f$ | |
| číslicová úhlová | $\Omega = 2\pi F = \dfrac{2\pi k}{N}$ | |

> **Frekvenční rozlišení (rozestup čar) DFT** $\Delta f = S/N = 1/T_0$ je dáno **délkou analyzovaného úseku**. Index $k = N/2$ odpovídá $f = S/2$; indexy $k > N/2$ odpovídají **záporným frekvencím** ($k \leftrightarrow k - N$).

### Polární tvar DFT, amplitudové, fázové a výkonové spektrum

$$X[k] = X_{real}[k] + jX_{imag}[k]$$
$$X_{mag}[k] = |X[k]| = \sqrt{X_{real}^2[k] + X_{imag}^2[k]}, \qquad X_\Phi[k] = \tan^{-1}\frac{X_{imag}[k]}{X_{real}[k]}$$
$$X_{PS}[k] = X_{mag}^2[k] = X_{real}^2[k] + X_{imag}^2[k] \quad\text{(výkonové spektrum)}$$

Pro zobrazení fyzikálních amplitud se spektrum normuje: **amplitudové spektrum** $A_k = \frac{1}{N}|X[k]|$, **výkonové spektrum** $P_k = \frac{1}{N^2}|X[k]|^2$ (dvoustranné; amplituda reálné sinusovky je $2A_k$).

**Pozor na problémy polární reprezentace:**
- **konverze fáze** – funkce `atan` vrací úhel jen v rozsahu $\langle -\pi/2, \pi/2\rangle$; správný kvadrant (rozsah $(-\pi, \pi\rangle$) dává `atan2(imag, real)` – je třeba zohlednit znaménko reálné části,
- **nulová reálná část** – dělení nulou (přetečení), fáze je v tom případě $\pm 90^\circ$,
- **fáze u velmi malých amplitud** (ztracených v šumu, zaokrouhlovací chyby) může **chaoticky kmitat** – nemá smysl ji interpretovat,
- **fáze se opakuje s periodou $2\pi$** → ve fázové charakteristice vznikají **skoky** (tzv. *wrapped* fáze, „zabalená“); spojitou fázi získáme rozbalením (*unwrapping*),
- **modul je vždy kladný** – kde by reálná charakteristika měnila znaménko (např. imaginární část je nulová), objeví se ve fázi **skok o $\pi$**.

### Řešený příklad – výpočet DFT z definice
*$x[n] = \{1, 2, 1, 0\}$, $N = 4$, $e^{-j2\pi nk/4} = e^{-jnk\pi/2}$:*

$$X[0] = 1 + 2 + 1 + 0 = 4$$
$$X[1] = 1 + 2e^{-j\pi/2} + 1\cdot e^{-j\pi} + 0 = 1 - 2j - 1 = -2j$$
$$X[2] = 1 + 2e^{-j\pi} + 1\cdot e^{-j2\pi} + 0 = 1 - 2 + 1 = 0$$
$$X[3] = 1 + 2e^{-j3\pi/2} + 1\cdot e^{-j3\pi} + 0 = 1 + 2j - 1 = 2j$$

$$X_{DFT}[k] = \{4, -2j, 0, 2j\}$$

*Zpětně (IDFT):* $x[n] = 0{,}25\sum_k X[k]e^{jnk\pi/2}$:
$x[0] = 0{,}25(4 - 2j + 0 + 2j) = 1$, $x[1] = 0{,}25(4 - 2j\cdot j + 0 + 2j\cdot(-j)) = 0{,}25(4 + 2 + 2) = 2$, $x[2] = 0{,}25(4 + 2j - 2j) = 1$, $x[3] = 0{,}25(4 - 2j(-j) + 2j(j)) = 0{,}25(4 - 2 - 2) = 0$ ✓.

### Vlastnosti DFT

| Vlastnost | Signál | DFT | Poznámka |
|---|---|---|---|
| linearita | $k_1x_1[n] + k_2x_2[n]$ | $k_1X_1[k] + k_2X_2[k]$ | |
| periodicita | $x[n] = x[n+N]$ | $X[k] = X[k+N]$ | obě periodické s periodou $N$ |
| **kruhový časový posun** | $x[n - n_0]$ | $X[k]\,e^{-j2\pi kn_0/N}$ | modul se nemění, mění se fáze |
| posun o půl periody | $x[n - 0{,}5N]$ | $(-1)^k X[k]$ | pro sudé $N$ |
| **kruhový frekvenční posun (modulace)** | $x[n]\,e^{j2\pi nk_0/N}$ | $X[k - k_0]$ | |
| modulace | $(-1)^n x[n]$ | $X[k - 0{,}5N]$ | pro sudé $N$ |
| otočení | $x[-n]$ | $X[-k]$ | kruhové otočení |
| součin | $x[n]\,y[n]$ | $\frac1N X[k] \circledast Y[k]$ | periodická konvoluce |
| **konvoluce** | $x[n] \circledast y[n]$ | $X[k]\,Y[k]$ | **periodická** konvoluce! |
| korelace | $x[n] \circledast\circledast y[n]$ | $X[k]\,Y^*[k]$ | periodická korelace |
| centrální hodnoty | $x[0] = \frac1N\sum_k X[k]$ | $X[0] = \sum_n x[n]$ | |
| | $x[\frac N2] = \frac1N\sum_k(-1)^kX[k]$ | $X[\frac N2] = \sum_n(-1)^nx[n]$ | pro sudé $N$ |
| **Parsevalova věta** | $\sum_{n=0}^{N-1}\lvert x[n]\rvert^2$ | $= \frac1N\sum_{k=0}^{N-1}\lvert X[k]\rvert^2$ | |

> **Periodickou konvoluci dvou posloupností délky $N$ určíme jako součin jejich $N$-bodových DFT** (a IDFT). Chceme-li **lineární** konvoluci, musíme obě posloupnosti nejprve doplnit nulami na délku alespoň $L_x + L_h - 1$ (jinak dojde k „zabalení“ – kap. 4).

**Symetrie DFT reálného signálu** – komplexně sdružená symetrie kolem počátku, díky periodicitě i kolem $N/2$:
$$X[-k] = X^*[k], \qquad X[-k] = X[N-k] \;\Rightarrow\; X[k] = X^*[N-k]$$
$$\mathrm{Re}\,X[k] = \mathrm{Re}\,X[N-k],\quad \mathrm{Im}\,X[k] = -\mathrm{Im}\,X[N-k],\quad |X[k]| = |X[N-k]|,\quad \phi(k) = -\phi(N-k)$$

$X[0] = \sum x[n]$ (stejnosměrná složka) a pro sudé $N$ i $X[N/2] = \sum(-1)^nx[n]$ jsou **reálné**. Pro reálný signál tedy nese informaci jen polovina vzorků DFT ($k = 0 \dots N/2$).

**Další vlastnosti:**
- je-li $x[n]$ **reálná a sudá**, je i $X[k]$ **reálná a sudá**,
- je-li $x[n]$ **reálná a lichá**, je $X[k]$ **ryze imaginární a lichá**.

### Vlastnosti fázové charakteristiky

- **posun signálu v čase** se projeví **lineární fází** (sklon úměrný posunu), modul se nemění,
- signál **symetrický kolem $n = 0$** má **nulovou fázi** (reálné spektrum),
- **fáze nese informaci o poloze (časování) detailů signálu.** Rekonstruujeme-li signál jen z modulu (fáze nulová), tvar se úplně ztratí; rekonstrukce jen z fáze (modul jednotkový) zachová polohu hran. Fázi proto nelze ignorovat.

### Velikost DFT vzorků

Je-li $x[n]$ reálná sinusovka s amplitudou $A_0$ a **celistvým počtem cyklů** v $N$ vzorcích, je velikost odpovídajícího vzorku DFT
$$M_r = A_0\,\frac{N}{2}$$
(energie se rozdělí na dvě čáry: $k_0$ a $N - k_0$). Pro komplexní vstup $A_0e^{j2\pi ft}$ je $M_r = A_0N$ (jen jedna čára).

### DFT dvojice

| Signál ($N$ vzorků) | DFT |
|---|---|
| impulz $\{1, 0, 0, \dots, 0\}$ | konstanta $\{1, 1, \dots, 1\}$ |
| konstanta $\{1, 1, \dots, 1\}$ | impulz $\{N, 0, \dots, 0\}$ |
| exponenciála $\alpha^n$ | $\dfrac{1 - \alpha^N}{1 - \alpha e^{-j2\pi k/N}}$ |
| sinusoida $\cos\!\left(2\pi n\dfrac{k_0}{N} + \Theta\right)$ | $0{,}5N\,\delta[k - k_0]\,e^{j\Theta} + 0{,}5N\,\delta[k - (N - k_0)]\,e^{-j\Theta}$ |

> **$N$-bodová sinusoida s celistvým počtem period ($F_0 = k_0/N$) má v DFT pouze dva nenulové vzorky** (v $k_0$ a $N - k_0$).

(Na slidu je u exponenciály v čitateli $1 - \alpha^n$; správně $1 - \alpha^N$ – součet konečné geometrické řady.)

### Inverzní DFT a interpretace

Je-li DFT komplexně sdruženě symetrická, lze $x[n]$ vyjádřit jako **součet sinusoid**: dvojici vzorků $Ae^{j\Theta}$ v $k_0$ a $Ae^{-j\Theta}$ v $N - k_0$ odpovídá sinusoida
$$\frac{2A}{N}\cos\!\left(2\pi n\frac{k_0}{N} + \Theta\right).$$
- $X[0]$ odpovídá **stejnosměrné složce** o velikosti $X[0]/N$,
- $X[N/2]$ (sudé $N$) odpovídá frekvenci $F = 0{,}5$, tj. signálu $K\cos(n\pi)$, kde $K = \frac1N X[N/2]$.

**Alternativní vzorec pro IDFT** (umožňuje použít algoritmus DFT/FFT i pro inverzi):
$$x[n] = \frac1N\left[\sum_{k=0}^{N-1}X^*[k]\,e^{-j2\pi kn/N}\right]^*$$
Postup: 1) obrátíme znaménka imaginárních částí $X[k]$ (komplexní sdružení), 2) spočítáme DFT, 3) obrátíme znaménka imaginárních částí výsledku, 4) vydělíme $N$.

### Rozlišení DFT a doplňování nulami (zero padding)

Doplněním signálu nulami (např. z 12 na 16 nebo 32 vzorků) získáme **hustší vzorkování téže DTFT** – spektrum vypadá hladší, mezi původní čáry se „interpolují“ další hodnoty. **Skutečné rozlišení (schopnost oddělit blízké frekvence) se tím ale nezlepší** – to závisí jen na délce skutečně naměřeného signálu.

### Replikace signálu a nulová interpolace spektra

Mezi replikací v jedné oblasti a nulovou interpolací ve druhé platí dualita:

- **$M$-násobná replikace signálu → nulová interpolace spektra a násobení $M$:**
$$\underbrace{\{x[n], x[n], \dots, x[n]\}}_{M\text{-krát}} \;\leftrightarrow\; M\,X^{\uparrow}_{DFT}[k/M]$$
- **$M$-násobná nulová interpolace signálu → replikace spektra:**
$$x^{\uparrow}[n/M] \;\leftrightarrow\; \underbrace{\{X_{DFT}[k], X_{DFT}[k], \dots, X_{DFT}[k]\}}_{M\text{-krát}}$$

#### Řešený příklad
*$x[n] = \{2, 3, 2, 1\}$, $X_{DFT}[k] = \{8, -2j, 0, 2j\}$. Najděte DFT 12bodových signálů $y[n] = \{x[n], x[n], x[n]\}$ a $h[n] = x[n/3]$ (nulová interpolace).*

a) replikace 3× → nulová interpolace spektra a násobení 3:
$Y_{DFT}[k] = 3X_{DFT}[k/3] = \{24, 0, 0, -6j, 0, 0, 0, 0, 0, 6j, 0, 0\}$

b) nulová interpolace 3× → replikace spektra:
$H_{DFT}[k] = \{8, -2j, 0, 2j, 8, -2j, 0, 2j, 8, -2j, 0, 2j\}$

---

## 5.6 Prosakování ve spektru (spectral leakage)

> **Prosakování** nastává, když u vzorkovaného analogového signálu počítáme DFT z $N$ vzorků, které **neobsahují celistvý počet period** sinusovek obsažených ve vstupním signálu. Ve spektru se pak objeví **nenulové čáry i jinde než na $\pm f_0$** – energie „prosákne“ do sousedních frekvenčních binů.
>
> Prosakování lze **omezit, ale ne zcela odstranit**.

**Proč vzniká:** DFT předpokládá periodické opakování analyzovaného úseku. Není-li v úseku celistvý počet period, má periodické prodloužení na hranicích **skoky** – a ty obsahují mnoho frekvencí.

**Matematicky:** pro $x_r[n] = \cos(2\pi nK/N)$ s obecně **neceločíselným** $K$ je DFT

$$X_r(m) \approx e^{j[\dots]}\cdot\frac{1}{2}\,\frac{\sin[\pi(K - m)]}{\sin[\pi(K - m)/N]} + e^{j[\dots]}\cdot\frac{1}{2}\,\frac{\sin[\pi(K + m)]}{\sin[\pi(K + m)/N]}$$

– spojité spektrum má tvar funkce typu sinc (Dirichletovo jádro) centrované na $K$, a DFT ho **vzorkuje v celočíselných $m$**:
- je-li $K$ **celé**, padnou všechny vzorky kromě $m = K$ přesně **do nul** sinc funkce → jediná čára velikosti $N/2$ (pro jednotkovou amplitudu),
- je-li $K$ **neceločíselné**, vzorky padnou na **postranní laloky** → prosakování.

*Ilustrace (N = 32, $f_s$ = 32 kHz, amplituda 1):* vstup 8,0 kHz → jediná čára velikosti 16 na 8 kHz; vstup 8,5 kHz → dvě čáry ≈ 10,17 na 8 a 9 kHz + prosakování; vstup 8,75 kHz → 14,05 na 9 kHz, 5,15 na 8 kHz + prosakování. Navíc **klesne i amplituda hlavní čáry** (scalloping).

### Určení, zda dochází k prosakování

Určíme číslicovou frekvenci $F_0 = f_0/S$ a index
$$k_F = F_0\,N = \frac{f_0}{S}N$$
- je-li **$k_F$ celé číslo** → k prosakování **nedochází**,
- je-li **$k_F$ neceločíselné** → prosakování nastane a největší hodnoty DFT budou **v okolí $k_F$**.

### Prevence prosakování
1. Pokud to jde, volit počet vzorků tak, aby úsek obsahoval **celistvý počet period** vstupního signálu.
2. Volit **co nejdelší úsek** signálu (co nejvíce vzorků) – prosakování se soustředí blíže k pravé frekvenci.
3. **Násobit vstupní signál vhodným oknem** (viz 5.7).

### Řešené příklady

**Př. 1.** *Tři periody signálu $x(t) = 4\cos(100\pi t)$ jsou vzorkovány dvojnásobkem Nyquistovy frekvence. Určete DFT.*
$f_0 = 50$ Hz, Nyquistova frekvence $2f_0 = 100$ Hz, $S = 200$ Hz. Tři periody trvají $3/50 = 0{,}06$ s → $N = 0{,}06\cdot200 = 12$. $k_F = \frac{50}{200}\cdot12 = 3$ – celé, bez prosakování. Velikost čáry $0{,}5NA = 0{,}5\cdot12\cdot4 = 24$:
$$X_{DFT}[k] = \{0, 0, 0, 24, 0, 0, 0, 0, 0, 24, 0, 0\}$$

**Př. 2.** *$x(t) = 4\sin(72\pi t)$ je vzorkována 128 Hz. Určete minimální počet vzorků bez prosakování a DFT.*
$f_0 = 36$ Hz, $F_0 = 36/128 = 9/32$ → minimální $N = 32$ ($k_0 = 9$). Protože $\sin\theta = \cos(\theta - \pi/2)$, je $\Theta = -\pi/2$:
$X[9] = 0{,}5\cdot32\cdot4\cdot e^{-j\pi/2} = -64j$, $\;X[23] = +64j$, ostatní nulové.

**Př. 3.** *$x(t) = 4\sin(72\pi t) - 6\cos(12\pi t)$ je vzorkován 21 Hz. Minimální $N$ bez prosakování a DFT?*
$f_1 = 36$ Hz → $F_1 = 36/21 = 12/7 \to$ alias $12/7 - 2 = -2/7$; $f_2 = 6$ Hz → $F_2 = 6/21 = 2/7$. Obě složky mají (po aliasingu) frekvenci $2/7$:
$$x[n] = 4\sin(2\pi n\cdot\tfrac{-2}{7}) - 6\cos(2\pi n\cdot\tfrac27) = -4\sin(\tfrac{4\pi n}{7}) - 6\cos(\tfrac{4\pi n}{7})$$
Minimální $N = 7$ ($k_0 = 2$). Kosinová složka dává $0{,}5\cdot7\cdot(-6) = -21$ v $k = 2$ i $5$; sinová složka $-4\sin = -4\cos(\cdot - \pi/2)$ dává $-14e^{-j\pi/2} = 14j$ v $k = 2$ a $-14j$ v $k = 5$:
$$X_{DFT}[k] = \{0, 0, -21 + 14j, 0, 0, -21 - 14j, 0\}$$

**Př. 4.** *Dvě periody signálu $x(t) = 1 + 8\sin(80\pi t)\cos(40\pi t)$ jsou vzorkovány dvojnásobkem Nyquistovy frekvence. Dojde k prosakování? Určete DFT.*
$8\sin(80\pi t)\cos(40\pi t) = 4\sin(120\pi t) + 4\sin(40\pi t)$ → složky 0, 20 a 60 Hz. Základní frekvence $\mathrm{NSD}(20, 60) = 20$ Hz (perioda 0,05 s). Nyquistova frekvence $2\cdot60 = 120$ Hz → $S = 240$ Hz. Dvě periody = 0,1 s → $N = 24$. $k$ pro 20 Hz: $\frac{20}{240}\cdot24 = 2$, pro 60 Hz: $6$ – celá čísla, **k prosakování nedojde**.
$$X[0] = 24,\quad X[2] = -48j,\quad X[6] = -48j,\quad X[18] = 48j,\quad X[22] = 48j,\quad \text{ostatní } 0$$

**Př. 5.** *$x(t) = 2\cos(20\pi t) + 5\cos(100\pi t)$ je vzorkován s $t_s = 0{,}005$ s ($S = 200$ Hz). Určete DFT pro délky 0,1 s, 0,125 s a 1,125 s.*
- **0,1 s → $N = 20$**, $\Delta f = 10$ Hz: $k = 1$ (10 Hz) a $k = 5$ (50 Hz) – celá čísla, **bez prosakování**; normované amplitudy $|X|/N$: 1 a 2,5.
- **0,125 s → $N = 25$**, $\Delta f = 8$ Hz: $k = 1{,}25$ a $6{,}25$ – **prosakování**, čáry jsou rozmazané a hlavní hodnoty menší.
- **1,125 s → $N = 225$**, $\Delta f \approx 0{,}89$ Hz: $k = 11{,}25$ a $56{,}25$ – prosakování je přítomno, ale **soustředěné těsně kolem** skutečných frekvencí (delší signál = jemnější mřížka).

---

## 5.7 Časová okna (spektrální vyhlazení)

**Výběr $N$ vzorků signálu = násobení signálu pravoúhlým oknem.** Spektrum výsledku je konvolucí spektra signálu se spektrem okna – a **pravoúhlé okno má vysoké postranní laloky** (−13 dB), které způsobují prosakování.

Proto se před DFT signál násobí tzv. **vyhlazovacím oknem** $w[n]$, které se ke krajům plynule blíží nule (odstraní skoky na hranicích periodického prodloužení):

$$X_w[m] = \sum_{n=0}^{N-1}w[n]\cdot x[n]\,e^{-j2\pi nm/N}$$

Každé okno má určitou frekvenční charakteristiku a podle toho je vhodné pro určitý typ signálu.

### Délka oken
- **lichý počet vzorků** – používá se většinou **při filtraci** (FIR filtry – symetrie kolem středového vzorku); např. MATLAB `bartlett(65)`, `triang(63)`,
- **sudý počet vzorků** – používá se většinou **pro omezení prosakování** (spektrální analýza). Výhodné je tzv. **periodické okno**, které respektuje periodické prodloužení implicitní v DFT (poslední vzorek „chybí“, aby se okno při periodickém opakování navázalo).

### Charakteristiky okna (v DTFT jeho spektra)

- **šířka hlavního laloku $W_M$** – mezi prvními nulami,
- **$W_3$, $W_6$** – šířka hlavního laloku v poklesu o 3 dB ($20\log\frac{P}{0{,}707P}$) a 6 dB ($20\log\frac{P}{0{,}5P}$),
- **PSL (peak sidelobe level)** – úroveň nejvyššího postranního laloku,
- **rychlost poklesu postranních laloků** (high-frequency decay, dB/oktávu).

**Další charakteristiky:**
- **CG – koherentní zesílení** (coherent gain) – amplituda výstupu dělená amplitudou vstupu (o kolik okno zmenší amplitudu čáry):
$$CG = \frac{1}{N}\sum_{k=0}^{N-1}|w[k]|$$
- **ENBW – ekvivalentní šumová šířka pásma** (o kolik okno zvětší výkon šumu v jednom binu):
$$ENBW = \frac{N\sum_{k=0}^{N-1}|w[k]|^2}{\left|\sum_{k=0}^{N-1}w[k]\right|^2}$$
- **SL – scalloping loss** – poměr koherentního zisku pro signál ležící **v polovině mezi dvěma** analyzujícími frekvencemi DFT a zisku pro signál přesně **na** analyzující frekvenci:
$$SL = 20\log\frac{\left|\sum_{k=0}^{N-1}w[k]\,e^{-j\pi k/N}\right|}{\sum_{k=0}^{N-1}w[k]}$$

### Používaná okna

(Zápis podle slidů: $k = 0{,}5N - n$, $n = 0, 1, \dots, N-1$ – tj. $k$ je vzdálenost od středu okna; $W_M$ je šířka hlavního laloku v číslicové frekvenci.)

| Okno | $w$ | $W_M = K/N$ | Nejvyšší postranní lalok |
|---|---|---|---|
| pravoúhlé (boxcar) | $1$ | $2/N$ | 0,2172 ≈ **−13,3 dB** |
| Bartlett (trojúhelníkové) | $1 - \dfrac{2\lvert k\rvert}{N}$ | $4/N$ | 0,0472 ≈ **−26,5 dB** |
| von Hann (Hanning) | $0{,}5 + 0{,}5\cos\!\left(\dfrac{2\pi k}{N}\right)$ | $4/N$ | 0,0267 ≈ **−31,5 dB** |
| Hamming | $0{,}54 + 0{,}46\cos\!\left(\dfrac{2\pi k}{N}\right)$ | $4/N$ | 0,0073 ≈ **−42,7 dB** |
| Blackman | $0{,}42 + 0{,}5\cos\!\left(\dfrac{2\pi k}{N}\right) + 0{,}08\cos\!\left(\dfrac{4\pi k}{N}\right)$ | $6/N$ | 0,0012 ≈ **−58,1 dB** |
| Kaiser | $\dfrac{I_0\!\left(\pi\beta\sqrt{1 - (2k/N)^2}\right)}{I_0(\pi\beta)}$ | $\dfrac{2\sqrt{1+\beta^2}}{N}$ | $\dfrac{0{,}22\pi\beta}{\sinh(\pi\beta)}$ ≈ −45,7 dB ($\beta = 2$) |

$I_0(\cdot)$ je modifikovaná Besselova funkce nultého řádu; parametr $\beta$ Kaiserova okna **nastavuje úroveň postranních laloků** (kompromis šířka laloku × potlačení). Ekvivalentní tvar Hannova okna v indexech $n$: $w[n] = 0{,}5 - 0{,}5\cos(2\pi n/N)$.

**Vybrané charakteristiky oken** (frekvence v binech):

| Okno | Postranní lalok [dB] | Pokles [dB/okt] | $B_{3dB}$ | $B_{6dB}$ | ENBW | Scalloping loss [dB] | Nejhorší ztráta [dB] |
|---|---|---|---|---|---|---|---|
| pravoúhlé | −13 | −6 | 0,89 | 1,21 | 1,00 | 3,92 | 3,92 |
| trojúhelníkové | −27 | −12 | 1,28 | 1,78 | 1,33 | 1,82 | 3,07 |
| Hann | −32 | −18 | 1,44 | 2,00 | 1,50 | 1,42 | 3,18 |
| Hamming | −43 | −6 | 1,30 | 1,81 | 1,36 | 1,78 | 3,10 |
| Kaiser ($\alpha = 3$) | −69 | −6 | 1,71 | 2,39 | 1,80 | 1,02 | 3,56 |
| Dolph–Čebyšev ($\alpha = 3$) | −60 | 0 | 1,44 | 2,01 | 1,51 | 1,44 | 3,23 |

### Volba okna – kompromisy

- **Zvětšováním délky okna** (u všech oken) **klesá šířka hlavního laloku**, velikost postranních laloků se téměř nemění.
- Ideál: **úzký (a vysoký) hlavní lalok a nízké postranní laloky** – to jsou **protichůdné požadavky**: obvykle s klesající šířkou hlavního laloku rostou postranní laloky (pravoúhlé okno má nejužší lalok, ale nejvyšší postranní laloky).
- **Dynamické rozlišení** – schopnost rozlišit složky s **velmi rozdílnou amplitudou** (slabá složka se nesmí ztratit v postranních lalocích silné) → vyžaduje **nízké postranní laloky**.
- **Frekvenční rozlišení okna** – schopnost odlišit sinusovky s **podobnou amplitudou a blízkou frekvencí** → vyžaduje **úzký hlavní lalok**.

> **Dvě sinusovky nelze od sebe odlišit, je-li jejich frekvenční rozdíl menší než šířka hlavního laloku okna:**
> $$\Delta F = \frac{\Delta f}{S} = W_M = \frac{K}{N}$$
> Ke zmenšení $\Delta F$ musíme zmenšit $W_M$ → **zvětšit $N$, ale ne přidáním nul!** → potřebujeme **více vzorků signálu** (delší záznam).

#### Řešený příklad
*$x(t) = A_1\cos(2\pi f_0t) + A_2\cos(2\pi(f_0 + \Delta f)t)$, $A_1 = A_2 = 1$, $f_0 = 30$ Hz, $S = 128$ Hz. $N$ vzorků doplníme nulami na $N_{FFT}$. Jaké je nejmenší rozlišitelné $\Delta f$ pro pravoúhlé a von Hannovo okno?*

$\Delta f_{min} = W_M\cdot S = \dfrac{K\,S}{N}$; pravoúhlé $K = 2$, Hann $K = 4$:

| | $N$ | $N_{FFT}$ | pravoúhlé | von Hann |
|---|---|---|---|---|
| a) | 256 | 2048 | 1 Hz | 2 Hz |
| b) | 512 | 2048 | 0,5 Hz | 1 Hz |
| c) | 256 | 4096 | 1 Hz | 2 Hz |

Pro $\Delta f = 1$ Hz (30 a 31 Hz) proto: a) pravoúhlé okno dvojici ještě rozliší, Hannovo ne (splynou v jeden plochý vrchol); b) rozliší obě okna; c) je stejné jako a) – **rozlišení nelze zvětšit přidáním nul, ale pouze zvětšením délky signálu**.

*Pokračování – dynamické rozlišení:* je-li $A_2 = 0{,}05$ (26 dB pod $A_1$, $20\log(1/0{,}05) = 26$), pak při $N = 512$ bez okna se slabá složka 31 Hz **ztratí v postranních lalocích** silné složky (−13 dB); s **Hannovým oknem** (postranní laloky −31,5 dB, rychle klesající) je **slabá složka zřetelně viditelná**.

---

## 5.8 Odhad spektra

**Výkonová spektrální hustota (PSD – power spectral density)** $R_{xx}(f)$ výkonového nebo náhodného signálu $x(t)$ je **Fourierova transformace autokorelační funkce** $r_{xx}(t)$ (Wienerův–Chinčinův teorém).
- $R_{xx}(f)$ je **reálná, nezáporná a sudá** funkce; plocha pod ní (resp. $r_{xx}(0)$) je rovna průměrnému výkonu signálu.
- PSD se používá k **odhadu spektra vzorkovaného signálu konečné délky**.

**Dva přístupy k odhadu spektra:**
- **neparametrický** – o signálu nic nepředpokládáme a spektrum odhadujeme přímo z dat (periodogram a jeho varianty),
- **parametrický** – signál modelujeme jako výstup filtru buzeného bílým šumem a odhadujeme koeficienty filtru (např. AR model).

### Periodogram

Založen na DFT (FFT) $N$-vzorkové řady $x[n]$:
$$P[k] = \frac{1}{N}\left|X_{DFT}[k]\right|^2$$

- dobrý odhad pro **deterministické, pásmově omezené výkonové signály**, vzorkované nad Nyquistovou frekvencí,
- **špatný pro signály poškozené šumem** – rozptyl odhadu s rostoucím $N$ neklesá (spektrum je „roztřepené“).

### Welchova metoda

Založena na **průměrování překrývajících se periodogramů**:
1. signál se rozdělí na $K$ **překrývajících se úseků** po $M$ vzorcích (překryv $D$ = 50–75 %),
2. každý úsek se vynásobí $M$-bodovým **oknem** $w[n]$ (omezení prosakování),
3. PSD každého úseku se odhadne periodogramem,
4. $K$ periodogramů se **zprůměruje** → $M$-bodový průměrný periodogram.

Průměrováním klesá rozptyl odhadu (hladší spektrum) za cenu horšího rozlišení (kratší úseky). Metoda je vhodná pro **detekci blízkých frekvencí** (dobré frekvenční rozlišení).

**Modifikované Welchovy metody** – cílem je lepší rozlišení a hladší odhad; úseky mají různou (rostoucí) délku a jsou uspořádány buď **symetricky** (soustředně kolem středu záznamu), nebo **asymetricky** (všechny začínají na začátku záznamu).

### Bartlettova metoda

Jako Welchova metoda, ale **úseky se nepřekrývají a nenásobí se oknem** (resp. jen pravoúhlým).

### Blackmanova–Tukeyova metoda

Určuje PSD z **autokorelace násobené oknem**:
1. $N$-vzorkový signál $x[n]$ se doplní nulami na $2N$ vzorků → $y[n]$ (aby periodická autokorelace odpovídala lineární),
2. spočítá se $Y_{FFT}[k]$ a IFFT součinu $Y_{FFT}[k]Y^*_{FFT}[k]$ → $2N$-bodový **odhad autokorelace** $r_{xx}[n]$,
3. odhad autokorelace se vynásobí **$M$-bodovým oknem** (nejčastěji **Bartlettovým**) – vyhlazení spektra a potlačení vlivu nepřesných hodnot autokorelace pro velká zpoždění,
4. FFT okénkované autokorelace → **vyhlazený periodogram**.

Menší $M$ (užší okno) = lepší vyhlazení, ale hrozí zamaskování některých špiček či slití ostrých detailů; obvykle $M = 0{,}1N$ až $0{,}5N$. Metoda je vhodná pro **dobře oddělené špičky s rozdílnou velikostí** (dobré dynamické rozlišení), **nevhodná pro krátké signály**.

---

## 5.9 Rychlá Fourierova transformace (FFT)

### Maticový zápis DFT a IDFT

Označme **twiddle faktor** $W_N = e^{-j2\pi/N}$. Pak
$$X_{DFT}[k] = \sum_{n=0}^{N-1}x[n]\,W_N^{nk}, \qquad x[n] = \frac1N\sum_{k=0}^{N-1}X_{DFT}[k]\,[W_N^{nk}]^*$$

$N$ rovnic DFT lze zapsat maticově $\mathbf{X} = \mathbf{W}_N\mathbf{x}$ ($\mathbf{X}$, $\mathbf{x}$ jsou matice $N\times1$, $\mathbf{W}_N$ matice $N\times N$):

$$\begin{bmatrix}X[0]\\X[1]\\X[2]\\\vdots\\X[N-1]\end{bmatrix} = \begin{bmatrix}W_N^0 & W_N^0 & W_N^0 & \cdots & W_N^0\\ W_N^0 & W_N^1 & W_N^2 & \cdots & W_N^{N-1}\\ W_N^0 & W_N^2 & W_N^4 & \cdots & W_N^{2(N-1)}\\ \vdots & \vdots & \vdots & \ddots & \vdots\\ W_N^0 & W_N^{N-1} & W_N^{2(N-1)} & \cdots & W_N^{(N-1)(N-1)}\end{bmatrix}\begin{bmatrix}x[0]\\x[1]\\x[2]\\\vdots\\x[N-1]\end{bmatrix}$$

**IDFT maticově:** $\mathbf{x} = \mathbf{W}_N^{-1}\mathbf{X}$, kde $\mathbf{W}_N^{-1} = \frac1N[\mathbf{W}_N^*]^T$.

Přímý výpočet vyžaduje $N^2$ komplexních násobení – pro $N = 1024$ přes milion. **FFT** využívá **symetrie a periodicity** twiddle faktorů:

| | Vztah | Význam |
|---|---|---|
| 1 | $W_N^{n+N} = W_N^n$ | periodicita |
| 2 | $W_N^{n + N/2} = -W_N^n$ | symetrie (posun o půl periody = změna znaménka) |
| 3 | $W_N^{Nk} = 1$ | |
| 4 | $W_N^2 = W_{N/2}$ | twiddle faktor dvojnásobné délky |

**Základní výsledky:**
- **1bodová DFT** čísla $A$ je číslo $A$,
- **2bodová DFT:** $X[0] = x[0] + x[1]$, $X[1] = x[0] - x[1]$ (žádné násobení!).

### Radix-2 FFT s decimací v čase (DIT – decimation in time)

$N$-bodovou DFT ($N = 2^m$) zapíšeme jako součet dvou $N/2$-bodových DFT ze **sudých** a **lichých** vzorků:

$$X[k] = \sum_{n=0}^{N/2-1}x[2n]\,W_N^{2nk} + \sum_{n=0}^{N/2-1}x[2n+1]\,W_N^{(2n+1)k} = \sum_{n=0}^{N/2-1}x[2n]\,W_{N/2}^{nk} + W_N^k\sum_{n=0}^{N/2-1}x[2n+1]\,W_{N/2}^{nk}$$

Označíme $X^e[k]$ – DFT sudých vzorků, $X^o[k]$ – DFT lichých vzorků (obě $N/2$-bodové, tedy periodické s periodou $N/2$):

$$X[k] = X^e[k] + W_N^kX^o[k]$$

Díky periodicitě $X^e, X^o$ a vlastnosti $W_N^{k + N/2} = -W_N^k$ dostaneme **Danielsonovo–Lanczosovo lemma**:

$$\boxed{X[k] = X^e[k] + W_N^kX^o[k], \qquad X\!\left[k + \tfrac N2\right] = X^e[k] - W_N^kX^o[k], \qquad k = 0, 1, \dots, \tfrac N2 - 1}$$

Tento výpočet se nazývá **motýlek (butterfly)**: ze vstupů $A = X^e[k]$, $B = X^o[k]$ dostaneme $A + BW^t$ a $A - BW^t$ – **jedno komplexní násobení a dvě sčítání**.

Rozklad se opakuje rekurzivně, dokud nedojdeme k 2bodovým DFT. Pro $N = 8$ jsou **3 iterace (stupně)** po 4 motýlcích. Aby výstup vyšel v přirozeném pořadí, musí být **vstup v bitově reverzním pořadí**:

| index | binárně | bitová reverze | pořadí ve FFT |
|---|---|---|---|
| 0 | 000 | 000 | 0 |
| 1 | 001 | 100 | 4 |
| 2 | 010 | 010 | 2 |
| 3 | 011 | 110 | 6 |
| 4 | 100 | 001 | 1 |
| 5 | 101 | 101 | 5 |
| 6 | 110 | 011 | 3 |
| 7 | 111 | 111 | 7 |

Vstup 8bodové DIT FFT je tedy $x(0), x(4), x(2), x(6), x(1), x(5), x(3), x(7)$.

```matlab
x = 0:7;                 % vytvoření vektoru indexů
b = dec2bin(x,3);        % konverze do binárních hodnot
c = b(:,3:-1:1);         % bitová reverze
bin2dec(c)               % zpět na desítkové → 0 4 2 6 1 5 3 7
```

**Algoritmus DIT FFT** (výpočet „na místě“):
1. přeuspořádej vstup bitovou reverzí,
2. pro iterace $i = 1, \dots, r$ ($r = \log_2 N$): $s = 2^i$ (rozestup skupin), $g = N/s$ (počet skupin), $b = s/2$ (motýlků ve skupině); pro každou skupinu $k$ a motýlek $m$: $\theta = -2\pi mg/N$, $w = \cos\theta + j\sin\theta$, $n = ks + m$, $y = w\,x(n+b)$, $x(n+b) = x(n) - y$, $x(n) = x(n) + y$,
3. výsledek $X(k) = x(k)$.

### Radix-2 FFT s decimací ve frekvenci (DIF – decimation in frequency)

Vstup rozdělíme na **první a druhou polovinu**, $x^a = x[n]$, $x^b = x[n + N/2]$:

$$X[k] = \sum_{n=0}^{N/2-1}x[n]W_N^{nk} + W_N^{Nk/2}\sum_{n=0}^{N/2-1}x[n + \tfrac N2]W_N^{nk} = \sum_{n=0}^{N/2-1}\left[x[n] + (-1)^k x[n + \tfrac N2]\right]W_N^{nk}$$

Rozdělením na **sudé a liché výstupní indexy**:

$$X[2k] = \sum_{n=0}^{N/2-1}[x^a + x^b]\,W_{N/2}^{nk}, \qquad X[2k+1] = \sum_{n=0}^{N/2-1}[x^a - x^b]\,W_N^n\,W_{N/2}^{nk}, \qquad k = 0, \dots, \tfrac N2 - 1$$

Motýlek DIF: ze vstupů $A$, $B$ vytvoří $A + B$ a $(A - B)W^t$. U DIF je **vstup v přirozeném pořadí a výstup v bitově reverzním pořadí**.

### Porovnání DIT a DIF

| Charakteristika | Decimace ve frekvenci (DIF) | Decimace v čase (DIT) |
|---|---|---|
| počet vzorků | $N = 2^m$ | $N = 2^m$ |
| vstupní posloupnost | přirozené pořadí | bitově reverzní pořadí |
| výsledek DFT | bitově reverzní pořadí | přirozené pořadí |
| výpočet | na místě (in place) | na místě |
| počet stupňů | $m = \log_2N$ | $m = \log_2N$ |
| násobení | $\frac N2\log_2N$ (komplexních) | $\frac N2\log_2N$ |
| sčítání | $N\log_2N$ | $N\log_2N$ |
| motýlků na stupeň | $N/2$ | $N/2$ |
| výstup motýlku | $(A + B)$ a $(A - B)W_N^t$ | $(A + BW_N^t)$ a $(A - BW_N^t)$ |
| twiddle faktory $i$-tého stupně | $t = 2^{i-1}Q$, $Q = 0, \dots, P-1$, $P = 2^{m-i}$ | $t = 2^{m-i}Q$, $Q = 0, \dots, P-1$, $P = 2^{i-1}$ |

### Porovnání DFT a FFT

| | $N$-bodová DFT | $N$-bodová FFT |
|---|---|---|
| algoritmus | řešení $N$ rovnic o $N$ neznámých | $0{,}5N$ motýlků na stupeň, $m$ stupňů, celkem $0{,}5mN$ motýlků |
| násobení | $N$ na rovnici | 1 na motýlek |
| sčítání | $N - 1$ na rovnici | 2 na motýlek |
| **celkem násobení** | $N^2$ | $0{,}5mN = 0{,}5N\log_2N$ |
| **celkem sčítání** | $N(N-1)$ | $mN = N\log_2N$ |

*Příklad:* $N = 1024$ ($m = 10$): DFT ≈ 1 048 576 komplexních násobení, FFT jen 5 120 – **asi 200× rychlejší**.

---

## Shrnutí kapitoly 5

- Vzorkování ↔ periodizace: diskrétní signál má **periodické** spektrum, periodický signál **diskrétní** spektrum.
- **DTFT** $X(F) = \sum x[k]e^{-j2\pi kF}$ = Z-transformace na jednotkové kružnici; periodická s periodou 1; pro reálný signál $|X|$ sudé, fáze lichá.
- **Frekvenční odezva** $H(F)$ = DTFT impulsní odezvy; ustálená odezva na $A\cos(2\pi nF_0 + \theta)$ je $A|H(F_0)|\cos(2\pi nF_0 + \theta + \phi(F_0))$.
- **DFT** $X[k] = \sum_{n=0}^{N-1}x[n]e^{-j2\pi nk/N}$ = vzorkovaná DTFT; $\Delta f = S/N$; součin DFT ↔ **periodická** konvoluce.
- Sinusovka s celistvým počtem period → 2 čáry velikosti $AN/2$; jinak **prosakování** ($k_F = F_0N$ neceločíselné).
- **Okna:** kompromis šířka hlavního laloku (frekvenční rozlišení, $\Delta F = K/N$) × postranní laloky (dynamické rozlišení); doplnění nulami rozlišení nezlepší.
- **Odhad spektra:** periodogram, Welch (překryv + okno + průměr), Bartlett, Blackman–Tukey.
- **FFT** radix-2: DIT (vstup bitově reverzní), DIF (výstup bitově reverzní); $\frac N2\log_2N$ násobení místo $N^2$.

### Kontrolní otázky
1. Proč je spektrum diskrétního signálu periodické? Jaká je perioda v $F$ a v $\Omega$?
2. Odvoďte DTFT signálu $\alpha^n u[n]$ a nakreslete její modul pro $\alpha = 0{,}5$ a $\alpha = -0{,}5$.
3. Spočítejte DFT signálu $\{1, 1, 0, 0\}$. *(Řešení: $\{2, 1 - j, 0, 1 + j\}$.)*
4. Signál 100 Hz je vzorkován 1 kHz a analyzován 64bodovou DFT. Nastane prosakování? Kolik vzorků zvolit, aby nenastalo? *(Řešení: $k_F = 6{,}4$ → ano; např. $N = 50$ nebo 100, tj. násobek 10.)*
5. Jaký je rozdíl mezi frekvenčním a dynamickým rozlišením? Které okno zvolíte pro detekci slabé složky vedle silné?
6. Proč doplnění nulami nezlepší rozlišení? K čemu tedy slouží?
7. Popište motýlek DIT FFT. Kolik násobení ušetříme pro $N = 4096$?
8. Jak spočítáte IDFT pomocí algoritmu pro DFT?

---

# 6. Z-transformace

**Z-transformace je mocný nástroj pro analýzu lineárních diskrétních systémů.** Převádí diferenční rovnice na algebraické rovnice a konvoluci na součin – stejně jako Laplaceova transformace u spojitých systémů.

## 6.1 Oboustranná Z-transformace

$$\boxed{X(z) = \sum_{k=-\infty}^{\infty}x[k]\,z^{-k}}, \qquad z \text{ je komplexní číslo } z = |r|e^{j2\pi F} = |r|e^{j\Omega}$$

- Oboustranná transformace konečné řady je **mocninná (Laurentova) řada** proměnné $z$. Hodnoty $z$ se zakreslují v **Argandově diagramu (z-rovině)**.
- Spolu s $X(z)$ se vždy uvádí **oblast konvergence (ROC – region of convergence)** – množina $z$, pro která řada konverguje.
- **Dvě různé posloupnosti mohou mít stejný výraz $X(z)$, ale s odlišnou oblastí konvergence** – proto je nutné ROC uvádět. Pokud se ROC neuvádí, uvažuje se obvykle **pravostranný** signál.

> Na jednotkové kružnici $|z| = 1$ ($r = 1$) přechází Z-transformace v DTFT: $X(F) = X(z)\big|_{z = e^{j2\pi F}}$ (kap. 5).

### Pravidla pro oblast konvergence

| Signál | ROC |
|---|---|
| **konečné délky** | celá z-rovina, kromě případně $z = 0$ (je-li $x[n] \neq 0$ pro nějaké $n > 0$) a/nebo $z = \infty$ (je-li $x[n] \neq 0$ pro nějaké $n < 0$) |
| **pravostranný** | **vně** kružnice o poloměru větším, než je absolutní hodnota **největšího** pólu: $\lvert z\rvert > \lvert p\rvert_{max}$ |
| **levostranný** | **uvnitř** kružnice o poloměru menším, než je absolutní hodnota **nejmenšího** pólu: $\lvert z\rvert < \lvert p\rvert_{min}$ |
| **oboustranný** | **mezikruží** ohraničené póly |

ROC nikdy neobsahuje póly.

#### Řešené příklady – určení ROC
(a) $x[n] = \{4, -3, \underset{\uparrow}{2}, 6\}$: ROC je $0 < |z| < \infty$ – vylučuje $z = 0$ i $z = \infty$, protože $x[n]$ je nenulový pro $n < 0$ i $n > 0$.

(b) $X(z) = \dfrac{z}{z-2} + \dfrac{z}{z+3}$: ROC závisí na povaze $x[n]$:
- pravostranný → $|z| > 3$ (největší pól $|p|_{max} = 3$),
- levostranný → $|z| < 2$ (nejmenší pól $|p|_{min} = 2$),
- oboustranný → $2 < |z| < 3$.

(Oblast „$|z| < 2$ a zároveň $|z| > 3$“ není platná – ROC musí být společná oběma členům.)

**Cvičení:** (a) $X(z) = \frac{z + 0{,}5}{z}$ → $|z| \neq 0$; (b) $Y(z) = \frac{z+1}{(z - 0{,}1)(z + 0{,}5)}$, pravostranný → $|z| > 0{,}5$; (c) $G(z) = \frac{z}{z+2} + \frac{z}{z-1}$, oboustranný → $1 < |z| < 2$; (d) $H(z) = \frac{z+3}{(z-2)(z+1)}$, levostranný → $|z| < 1$.

---

## 6.2 Vlastnosti oboustranné Z-transformace

| | Vlastnost | Signál | Z-transformace |
|---|---|---|---|
| 1 | **posun** | $x[n - N]$ | $z^{-N}X(z)$ |
| 2 | otočení (reflexe) | $x[-n]$ | $X(1/z)$ |
| 3 | antikauzální část | $x[-n]u[-n-1]$ | $X(1/z) - x[0]$ (pro kauzální $x[n]$) |
| 4 | změna měřítka | $\alpha^n x[n]$ | $X(z/\alpha)$ |
| 5 | násobení $n$ | $n\,x[n]$ | $-z\dfrac{dX(z)}{dz}$ |
| 6 | násobení kosinem | $\cos(n\Omega)\,x[n]$ | $0{,}5\left[X(ze^{j\Omega}) + X(ze^{-j\Omega})\right]$ |
| 7 | násobení sinem | $\sin(n\Omega)\,x[n]$ | $j0{,}5\left[X(ze^{j\Omega}) - X(ze^{-j\Omega})\right]$ |
| 8 | **konvoluce** | $x[n] * h[n]$ | $X(z)\,H(z)$ |

> Vlastnost posunu je základ: **zpoždění o jeden vzorek = násobení $z^{-1}$** (odtud značka $z^{-1}$ pro zpožďovací blok).

---

## 6.3 Převod signálu do Z-oblasti

- **signály konečné délky** → přímo **polynom** proměnné $z$ (resp. $z^{-1}$),
- **ostatní signály** → **převodní tabulka**.

### Tabulka Z-transformací

| | Signál | Z-transformace | ROC |
|---|---|---|---|
| | **Konečné posloupnosti** | | |
| 1 | $\delta[n]$ | $1$ | celá rovina |
| 2 | $u[n] - u[n-N]$ | $\dfrac{1 - z^{-N}}{1 - z^{-1}}$ | $z \neq 0$ |
| | **Kauzální signály** | | |
| 3 | $u[n]$ | $\dfrac{z}{z-1}$ | $\lvert z\rvert > 1$ |
| 4 | $\alpha^n u[n]$ | $\dfrac{z}{z-\alpha}$ | $\lvert z\rvert > \lvert\alpha\rvert$ |
| 5 | $(-\alpha)^n u[n]$ | $\dfrac{z}{z+\alpha}$ | $\lvert z\rvert > \lvert\alpha\rvert$ |
| 6 | $n\,u[n]$ | $\dfrac{z}{(z-1)^2}$ | $\lvert z\rvert > 1$ |
| 7 | $n\alpha^n u[n]$ | $\dfrac{z\alpha}{(z-\alpha)^2}$ | $\lvert z\rvert > \lvert\alpha\rvert$ |
| 8 | $\cos(n\Omega)\,u[n]$ | $\dfrac{z^2 - z\cos\Omega}{z^2 - 2z\cos\Omega + 1}$ | $\lvert z\rvert > 1$ |
| 9 | $\sin(n\Omega)\,u[n]$ | $\dfrac{z\sin\Omega}{z^2 - 2z\cos\Omega + 1}$ | $\lvert z\rvert > 1$ |
| 10 | $\alpha^n\cos(n\Omega)\,u[n]$ | $\dfrac{z^2 - \alpha z\cos\Omega}{z^2 - 2\alpha z\cos\Omega + \alpha^2}$ | $\lvert z\rvert > \lvert\alpha\rvert$ |
| 11 | $\alpha^n\sin(n\Omega)\,u[n]$ | $\dfrac{\alpha z\sin\Omega}{z^2 - 2\alpha z\cos\Omega + \alpha^2}$ | $\lvert z\rvert > \lvert\alpha\rvert$ |
| | **Antikauzální signály** | | |
| 12 | $-u[-n-1]$ | $\dfrac{z}{z-1}$ | $\lvert z\rvert < 1$ |
| 13 | $-n\,u[-n-1]$ | $\dfrac{z}{(z-1)^2}$ | $\lvert z\rvert < 1$ |
| 14 | $-\alpha^n u[-n-1]$ | $\dfrac{z}{z-\alpha}$ | $\lvert z\rvert < \lvert\alpha\rvert$ |
| 15 | $-n\alpha^n u[-n-1]$ | $\dfrac{z\alpha}{(z-\alpha)^2}$ | $\lvert z\rvert < \lvert\alpha\rvert$ |

Všimněte si: dvojice 4 a 14 mají **stejný výraz** $\frac{z}{z-\alpha}$, liší se jen ROC – proto je ROC nezbytná.

*Odvození dvojice 4:* $\sum_{n=0}^{\infty}\alpha^nz^{-n} = \sum_{n=0}^{\infty}(\alpha z^{-1})^n = \dfrac{1}{1 - \alpha z^{-1}} = \dfrac{z}{z-\alpha}$, konverguje pro $|\alpha z^{-1}| < 1$, tj. $|z| > |\alpha|$.

#### Řešené příklady
(a) $x[n] = \delta[n]$ → $X(z) = 1$, ROC celá z-rovina.

(b) $x[n] = 2\delta[n+1] + \delta[n] - 5\delta[n-1] + 4\delta[n-2] = \{2, \underset{\uparrow}{1}, -5, 4\}$ → $X(z) = 2z + 1 - 5z^{-1} + 4z^{-2}$, ROC $0 < |z| < \infty$.

(c) $x[n] = u[n] - u[n-N]$ ($N$ jedniček): $X(z) = 1 + z^{-1} + \dots + z^{-(N-1)} = \sum_{k=0}^{N-1}z^{-k} = \dfrac{1 - z^{-N}}{1 - z^{-1}}$, ROC $|z| > 0$.

**Cvičení:** $\{\underset{\uparrow}{2}, 1, 0, 4\} \to 2 + z^{-1} + 4z^{-3}$; $\;\{2, -3, \underset{\uparrow}{1}, 0, 4\} \to 2z^2 - 3z + 1 + 4z^{-2}$; $\;X(z) = 3z^2 + z - 3z^{-1} + 5z^{-2} \to \{3, 1, \underset{\uparrow}{0}, -3, 5\}$.

---

## 6.4 Z-rovina, nuly a póly

Z-transformaci většiny signálů lze vyjádřit jako **racionální lomenou funkci**:

$$X(z) = \frac{N(z)}{D(z)} = \frac{B_Mz^M + B_{M-1}z^{M-1} + \dots + B_1z + B_0}{A_Nz^N + A_{N-1}z^{N-1} + \dots + A_1z + A_0}$$

- kořeny čitatele $N(z)$, značené $z_i$ → **nuly** (v diagramu **○**),
- kořeny jmenovatele $D(z)$, značené $p_k$ → **póly** (v diagramu **×**).

$$X(z) = K\,\frac{(z - z_1)(z - z_2)\cdots(z - z_M)}{(z - p_1)(z - p_2)\cdots(z - p_N)}$$

$X(z)$ je až na konstantu $K$ **jednoznačně určena polohou nul a pólů**.

### Diagram nul a pólů

*Příklad:*
$$H(z) = \frac{2z(z+1)}{\left(z - \frac13\right)\left(z^2 + \frac14\right)\left(z^2 + 4z + 5\right)}$$
- nuly: $z = 0$, $z = -1$,
- póly: $z = \frac13$, $z = \pm j0{,}5$, $z = -2 \pm j$ (kořeny $z^2 + 4z + 5$),
- $K = 2$.

Komplexní póly a nuly reálných systémů se vyskytují vždy v **komplexně sdružených párech**.

*Opačná úloha:* jaká Z-transformace odpovídá diagramu s nulami v $\pm j0{,}5$ a $\pm j2$, dvojnásobným pólem v počátku a $K = 1$?

$$N(z) = (z - j0{,}5)(z + j0{,}5)(z + j2)(z - j2) = (z^2 + 0{,}25)(z^2 + 4) = z^4 + 4{,}25z^2 + 1, \qquad D(z) = z^2$$
$$X(z) = \frac{z^4 + 4{,}25z^2 + 1}{z^2} = z^2 + 4{,}25 + z^{-2} \;\Rightarrow\; x[n] = \delta[n+2] + 4{,}25\delta[n] + \delta[n-2] = \{1, 0, \underset{\uparrow}{4{,}25}, 0, 1\}$$

Platí $X(z) = X(1/z)$ → $x[n]$ je **sudě symetrický**. Každá nula je spárována se svou **převrácenou hodnotou** ($j0{,}5$ s $-j2$, $-j0{,}5$ s $j2$) – to je charakteristické pro symetrické posloupnosti (a FIR filtry s lineární fází, kap. 8).

---

## 6.5 Přenosová funkce systému

Odezva systému s impulsní odezvou $h[n]$ je konvoluce $y[n] = x[n] * h[n]$; v Z-oblasti se konvoluce mění na **součin**:

$$Y(z) = X(z)\cdot H(z) \quad\Longrightarrow\quad \boxed{H(z) = \frac{Y(z)}{X(z)}}$$

> **$H(z)$ je přenosová funkce systému**, definovaná pouze pro **ustálený (relaxovaný) LTI systém** jako poměr Z-transformace výstupu k Z-transformaci vstupu. **$H(z)$ je Z-transformace impulsní odezvy $h[n]$.**

Pro systém popsaný diferenční rovnicí $\sum_{k=0}^{N}A_ky[n-k] = \sum_{k=0}^{M}B_kx[n-k]$:

$$H(z) = \frac{Y(z)}{X(z)} = \frac{B_0 + B_1z^{-1} + \dots + B_Mz^{-M}}{A_0 + A_1z^{-1} + \dots + A_Nz^{-N}}$$

(koeficienty diferenční rovnice se přímo stávají koeficienty polynomů v $z^{-1}$).

**LTI systém lze popsat** ekvivalentně:
- **přenosovou funkcí** $H(z)$,
- **impulsní odezvou** $h[n]$,
- **diferenční rovnicí**,
- **diagramem nul a pólů** (+ konstanta $K$).

#### Řešené příklady
(a) $y[n] - 0{,}4y[n-1] = 2x[n]$ → $H(z) = \dfrac{2}{1 - 0{,}4z^{-1}} = \dfrac{2z}{z - 0{,}4}$.

(b) $H(z) = \dfrac{z - 1}{z + 0{,}5} = \dfrac{1 - z^{-1}}{1 + 0{,}5z^{-1}}$ → $y[n] + 0{,}5y[n-1] = x[n] - x[n-1]$.

(c) $h[n] = (0{,}5)^n u[n] - \delta[n]$ → $H(z) = \dfrac{z}{z - 0{,}5} - 1 = \dfrac{0{,}5}{z - 0{,}5} = \dfrac{0{,}5z^{-1}}{1 - 0{,}5z^{-1}}$ → $y[n] - 0{,}5y[n-1] = 0{,}5x[n-1]$.

### Spojování systémů

**Kaskádní spojení:**
$$Y(z) = H_2(z)Y_1(z) = H_2(z)\,H_1(z)\,X(z) \;\Rightarrow\; H(z) = H_1(z)\,H_2(z)$$
Obecně pro $n$ kaskádně spojených systémů: $H(z) = H_1(z)\,H_2(z)\cdots H_n(z)$.

**Paralelní spojení:**
$$Y(z) = H_1(z)X(z) + H_2(z)X(z) \;\Rightarrow\; H(z) = H_1(z) + H_2(z)$$
Obecně: $H(z) = H_1(z) + H_2(z) + \dots + H_n(z)$.

(V časové oblasti: kaskáda = konvoluce impulsních odezev, paralela = jejich součet – kap. 4.)

---

## 6.6 Realizace přenosové funkce

Přenosovou funkci lze rozložit na nerekurzivní (FIR) a rekurzivní (IIR, „all-pole“) část:

$$H_N(z) = B_0 + B_1z^{-1} + \dots + B_Mz^{-M} \quad\leftrightarrow\quad y[n] = B_0x[n] + B_1x[n-1] + \dots + B_Mx[n-M]$$
$$H_R(z) = \frac{1}{1 + A_1z^{-1} + \dots + A_Nz^{-N}} \quad\leftrightarrow\quad y[n] = -A_1y[n-1] - \dots - A_Ny[n-N] + x[n]$$

Pro obecnou diferenční rovnici
$$y[n] = -A_1y[n-1] - \dots - A_Ny[n-N] + B_0x[n] + B_1x[n-1] + \dots + B_Nx[n-N]$$
$$H(z) = \frac{B_0 + B_1z^{-1} + \dots + B_Nz^{-N}}{1 + A_1z^{-1} + A_2z^{-2} + \dots + A_Nz^{-N}} = H_N(z)\,H_R(z)$$

- **přímá forma I** – kaskáda $H_N$ a $H_R$ se dvěma zpožďovacími linkami,
- **přímá forma II (kanonická)** – pořadí prohozeno ($H_R$ první), linky sloučeny do jedné; zpětné vazby s koeficienty $-A_k$ vlevo, dopředné větve $B_k$ vpravo (kap. 4).

### Duální (transponovaná) struktura (transposed realization)

Vychází z přímé formy II a vznikne jejím **překlopením**:
- **záměna vstupu a výstupu**,
- **otočení směru toku signálu** ve všech větvích,
- **záměna sumátorů a uzlů (spojek)**.

Přenosová funkce se nezmění. Transponovaná struktura má sčítačky „mezi“ zpožďovacími členy – každý zpožďovací člen ukládá částečný součet; je výhodná z hlediska numerických vlastností a paralelizace.

### Sériová (kaskádní) realizace

Přenosová funkce se zapíše jako **součin** dílčích přenosových funkcí:
$$H_C(z) = H_1(z)\cdot H_2(z)\cdots H_N(z)$$

> Systém $N$-tého řádu se realizuje jako **kaskáda sekcí 2. řádu** (dvojice komplexně sdružených pólů/nul) a jedné **sekce 1. řádu** (je-li $N$ liché). Kaskáda sekcí 2. řádu (**bikvadů**) je numericky mnohem robustnější než jedna struktura vysokého řádu (citlivost pólů na kvantizaci koeficientů).

### Paralelní realizace

Přenosová funkce se zapíše jako **součet** dílčích přenosových funkcí – **rozkladem na parciální zlomky**:
$$H_P(z) = H_1(z) + H_2(z) + \dots + H_N(z)$$
Systém se realizuje jako paralelní spojení subsystémů 1. nebo 2. řádu.

#### Řešený příklad 1 – kaskádní realizace
$$H(z) = \frac{z^2(6z - 2)}{(z - 1)\left(z^2 - \frac16z - \frac16\right)}$$

Rozdělíme na sekci 1. a 2. řádu:
$$H(z) = \underbrace{\frac{6z - 2}{z - 1}}_{H_1(z)}\cdot\underbrace{\frac{z^2}{z^2 - \frac16z - \frac16}}_{H_2(z)} = \frac{6 - 2z^{-1}}{1 - z^{-1}}\cdot\frac{1}{1 - \frac16z^{-1} - \frac16z^{-2}}$$

$H_1$: $y_1[n] = y_1[n-1] + 6x[n] - 2x[n-1]$; $\;H_2$: $y[n] = \frac16y[n-1] + \frac16y[n-2] + y_1[n]$. (Jmenovatel $H_2$ lze dále rozložit: $z^2 - \frac16z - \frac16 = (z - \frac12)(z + \frac13)$, takže je možná i kaskáda tří sekcí 1. řádu.)

#### Řešený příklad 2 – paralelní realizace
$$H(z) = \frac{z^2}{(z-1)(z-0{,}5)}$$

Rozložíme $H(z)/z$ na parciální zlomky:
$$\frac{H(z)}{z} = \frac{z}{(z-1)(z-0{,}5)} = \frac{A}{z-1} + \frac{B}{z-0{,}5}, \quad A = \frac{1}{1 - 0{,}5} = 2, \quad B = \frac{0{,}5}{0{,}5 - 1} = -1$$
$$H(z) = \frac{2z}{z - 1} - \frac{z}{z - 0{,}5} = \frac{2}{1 - z^{-1}} - \frac{1}{1 - 0{,}5z^{-1}}$$

Dvě paralelní sekce 1. řádu: $y_1[n] = y_1[n-1] + 2x[n]$, $\;y_2[n] = 0{,}5y_2[n-1] - x[n]$, $\;y[n] = y_1[n] + y_2[n]$.

---

## 6.7 Kauzalita a stabilita LTI systému

**Kauzalita:**
- v časové oblasti: kauzální systém má $h[n] = 0$ pro $n < 0$,
- v Z-oblasti: **počet nul nesmí převýšit počet pólů** – stupeň čitatele $H(z)$ (v proměnné $z$) nesmí být větší než stupeň jmenovatele; ROC je vně kružnice (pravostranná $h[n]$).

**Stabilita (BIBO – Bounded Input, Bounded Output):** na **omezený vstup reaguje systém omezeným výstupem**. Nutná a postačující podmínka:
$$\boxed{\sum_{k=-\infty}^{\infty}|h[k]| < \infty}$$

**ROC stabilního LTI systému vždy obsahuje jednotkovou kružnici.** Odtud:
- **stabilní a kauzální systém – všechny póly leží uvnitř jednotkové kružnice** ($|p_k| < 1$),
- **stabilní a antikauzální systém – všechny póly leží vně jednotkové kružnice**,
- stabilní oboustranný systém – ROC je mezikruží obsahující jednotkovou kružnici.

> **FIR filtry jsou vždy stabilní** (všechny póly v počátku, $h[n]$ konečná). U IIR filtrů je stabilitu nutné kontrolovat.

*Příklad:* $h[n] = (0{,}5)^n u[n]$ → pól $0{,}5$, $\sum|h| = 2 < \infty$ → stabilní; $h[n] = 2^n u[n]$ → pól $2$ → nestabilní; $h[n] = u[n]$ (pól přesně na kružnici, $z = 1$) → $\sum|h| = \infty$ → **nestabilní** (marginálně stabilní – akumulátor).

**Poloha pólů a tvar impulsní odezvy** (kauzální systém): reálný pól $0 < p < 1$ → exponenciálně klesající odezva; $-1 < p < 0$ → klesající se střídáním znaménka; komplexní pár $re^{\pm j\Omega}$ s $r < 1$ → tlumené kmity s frekvencí $\Omega$; čím blíž je pól jednotkové kružnici, tím pomaleji odezva doznívá a tím **ostřejší je rezonance** frekvenční charakteristiky na frekvenci $\Omega$.

---

## 6.8 Inverzní Z-transformace

Obecný vzorec (křivkový integrál po uzavřené křivce v ROC):
$$x[n] = \frac{1}{2\pi j}\oint X(z)\,z^{n-1}\,dz$$

(Na slidu chybí $j$ ve jmenovateli.) V praxi se téměř nepoužívá; inverzní transformace se určuje kombinací:
1. **přímého převodu**,
2. **dělení polynomů**,
3. **rozkladu na parciální zlomky**.

### 1) Přímý převod

Pro jednoduché případy, kdy je $X(z)$ konečná řada – koeficient u $z^{-k}$ je přímo vzorek $x[k]$:

$$X(z) = 3z^{-1} + 5z^{-3} + 2z^{-4} \;\Rightarrow\; x[n] = 3\delta[n-1] + 5\delta[n-3] + 2\delta[n-4] = \{\underset{\uparrow}{0}, 3, 0, 5, 2\}$$
$$X(z) = 2z^2 - 5z + 5z^{-1} - 2z^{-2} \;\Rightarrow\; x[n] = 2\delta[n+2] - 5\delta[n+1] + 5\delta[n-1] - 2\delta[n-2] = \{2, -5, \underset{\uparrow}{0}, 5, -2\}$$

(Na slidu je ve druhém výsledku navíc chybná hodnota 3.)

### 2) Dělení polynomů

Používá se zejména, chceme-li **jen prvních několik členů** odezvy – bývá rychlejší než rozklad na parciální zlomky. Pro $X(z) = N(z)/D(z)$:
- **pravostranný signál:** $N(z)$ i $D(z)$ uspořádáme podle **sestupných** mocnin $z$ → dělením dostaneme mocninnou řadu v $z^{-1}$,
- **levostranný signál:** uspořádáme podle **vzestupných** mocnin $z$ → dostaneme řadu v $z$.

*Příklad:* $H(z) = \dfrac{z - 4}{z^2 - z + 1}$ (pravostranný):
$$(z - 4) : (z^2 - z + 1) = z^{-1} - 3z^{-2} - 4z^{-3} - z^{-4} + \dots$$
$$h[n] = \delta[n-1] - 3\delta[n-2] - 4\delta[n-3] - \dots = \{\underset{\uparrow}{0}, 1, -3, -4, -1, \dots\}$$

*Postup dělení:* $z : z^2 = z^{-1}$; $z^{-1}(z^2 - z + 1) = z - 1 + z^{-1}$; zbytek $(z - 4) - (z - 1 + z^{-1}) = -3 - z^{-1}$; $-3 : z^2 = -3z^{-2}$; atd. (Kontrola rekurzí $h[n] = h[n-1] - h[n-2] + \delta[n-1] - 4\delta[n-2]$ dává $0, 1, -3, -4, -1$ ✓.)

### 3) Rozklad na parciální zlomky

Princip: $X(z)$ se rozloží na součet jednoduchých zlomků, jejichž inverzní transformace najdeme v tabulce. **Rozkládá se $Y(z) = X(z)/z$** (aby po vynásobení $z$ vznikly členy tvaru $\frac{z}{z - p}$, odpovídající $p^n u[n]$). Rozklad závisí na **pólech** (kořenech jmenovatele):

**a) Různé reálné póly:**
$$Y(z) = \frac{X(z)}{z} = \frac{P(z)}{(z + p_1)(z + p_2)\cdots(z + p_N)} = \frac{k_1}{z + p_1} + \frac{k_2}{z + p_2} + \dots + \frac{k_N}{z + p_N}$$
$$k_m = (z + p_m)\,Y(z)\big|_{z = -p_m}$$

**b) Různé komplexní póly** – vyskytují se v komplexně sdružených párech a jejich koeficienty jsou také komplexně sdružené:
$$Y(z) = \frac{k_1}{z + p_1} + \dots + \frac{A_1}{z + r_1} + \frac{A_1^*}{z + r_1^*} + \frac{A_2}{z + r_2} + \frac{A_2^*}{z + r_2^*} + \dots$$
(stačí spočítat jeden koeficient z každého páru).

**c) Opakující se (násobné) póly** – $Y(z)$ obsahuje pól $(z + r)^k$:
$$Y(z) = \dots + \frac{A_0}{(z + r)^k} + \frac{A_1}{(z + r)^{k-1}} + \dots + \frac{A_{k-1}}{z + r}$$
$$A_0 = (z + r)^kY(z)\big|_{z=-r}, \quad A_1 = \frac{d}{dz}\left[(z + r)^kY(z)\right]\Big|_{z=-r}, \quad A_n = \frac{1}{n!}\frac{d^n}{dz^n}\left[(z + r)^kY(z)\right]\Big|_{z=-r}$$

### Tabulka inverzní Z-transformace (členy rozkladu → kauzální signál, $n \ge 0$)

| | Člen rozkladu $X(z)$ | Kauzální signál $x[n]$, $n \ge 0$ |
|---|---|---|
| 1 | $\dfrac{z}{z - \alpha}$ | $\alpha^n$ |
| 2 | $\dfrac{z}{(z - \alpha)^2}$ | $n\alpha^{n-1}$ |
| 3 | $\dfrac{z}{(z - \alpha)^{N+1}}$ ($N > 1$) | $\dfrac{n(n-1)\cdots(n-N+1)}{N!}\alpha^{n-N}$ |
| 4 | $\dfrac{z(C + jD)}{z - \alpha e^{j\Omega}} + \dfrac{z(C - jD)}{z - \alpha e^{-j\Omega}}$ | $2\alpha^n[C\cos(n\Omega) - D\sin(n\Omega)]$ |
| 5 | $\dfrac{zK\angle\phi}{z - \alpha e^{j\Omega}} + \dfrac{zK\angle{-\phi}}{z - \alpha e^{-j\Omega}}$ | $2K\alpha^n\cos(n\Omega + \phi)$ |
| 6 | $\dfrac{z(C + jD)}{(z - \alpha e^{j\Omega})^2} + \dfrac{z(C - jD)}{(z - \alpha e^{-j\Omega})^2}$ | $2n\alpha^{n-1}\{C\cos[(n-1)\Omega] - D\sin[(n-1)\Omega]\}$ |
| 7 | $\dfrac{zK\angle\phi}{(z - \alpha e^{j\Omega})^2} + \dfrac{zK\angle{-\phi}}{(z - \alpha e^{-j\Omega})^2}$ | $2Kn\alpha^{n-1}\cos[(n-1)\Omega + \phi]$ |
| 8 | $\dfrac{zK\angle\phi}{(z - \alpha e^{j\Omega})^{N+1}} + \dfrac{zK\angle{-\phi}}{(z - \alpha e^{-j\Omega})^{N+1}}$ | $2K\dfrac{n(n-1)\cdots(n-N+1)}{N!}\alpha^{n-N}\cos[(n-N)\Omega + \phi]$ |

Pro **antikauzální** posloupnosti dostaneme signál $-x[n]\,u[-n-1]$, kde $x[n]$ je uvedeno v tabulce.

---

## 6.9 Jednostranná Z-transformace

Používá se při analýze **kauzálních systémů** – zejména **s nenulovými počátečními podmínkami**:

$$X(z) = \sum_{k=0}^{\infty}x[k]\,z^{-k}$$

Vlastnosti jsou podobné jako u oboustranné transformace, jsou jen upravené pro práci s kauzálními signály. Nejdůležitější rozdíl je u **posunu** – do výsledku „vstupují“ počáteční podmínky:

**Posun doprava (zpoždění):**
$$y[n-1] \Leftrightarrow z^{-1}Y(z) + y[-1]$$
$$y[n-2] \Leftrightarrow z^{-2}Y(z) + z^{-1}y[-1] + y[-2]$$
$$y[n-N] \Leftrightarrow z^{-N}Y(z) + z^{-(N-1)}y[-1] + z^{-(N-2)}y[-2] + \dots + y[-N]$$

(Vzorky s indexy $-1, -2, \dots$, které se posunem dostaly do oblasti $n \ge 0$, se musí přičíst.)

**Posun doleva (předstih):**
$$y[n+1] \Leftrightarrow zY(z) - zy[0]$$
$$y[n+2] \Leftrightarrow z^2Y(z) - z^2y[0] - zy[1]$$
$$y[n+N] \Leftrightarrow z^NY(z) - z^Ny[0] - z^{N-1}y[1] - \dots - zy[N-1]$$

(Vzorky, které posunem „vypadly“ do záporných indexů, se musí odečíst.)

**Periodický signál** (zapnutý v $n = 0$):
$$x_p[n]\,u[n] \Leftrightarrow \frac{X_1(z)}{1 - z^{-N}} = \frac{z^N}{z^N - 1}X_1(z)$$
kde $X_1(z)$ je Z-transformace **první periody** $x_1[n]$.

**Věta o počáteční hodnotě:**
$$x[0] = \lim_{z\to\infty}X(z)$$

**Věta o koncové hodnotě** (platí, má-li $(z-1)X(z)$ všechny póly uvnitř jednotkové kružnice):
$$\lim_{n\to\infty}x[n] = \lim_{z\to1}(z-1)\,X(z)$$

---

## 6.10 Využití Z-transformace k analýze systémů

### Systémy popsané diferenční rovnicí
1. převedeme diferenční rovnici do Z-oblasti s využitím **vlastností posunu jednostranné Z-transformace a počátečních podmínek**,
2. vyjádříme $Y(z)$ – rozdělíme na část od počátečních podmínek (**ZIR**) a od vstupu (**ZSR**),
3. provedeme **zpětnou transformaci** (parciální zlomky).

#### Řešený příklad 1
*Řešte $y[n] - 0{,}5y[n-1] = 2(0{,}25)^n u[n]$, $y[-1] = -2$; určete ZIR a ZSR.*

Transformace:
$$Y(z) - 0{,}5\left[z^{-1}Y(z) + y[-1]\right] = \frac{2z}{z - 0{,}25}$$
$$Y(z)\,(1 - 0{,}5z^{-1}) = \underbrace{0{,}5y[-1]}_{=-1} + \frac{2z}{z - 0{,}25}$$

**ZIR:** $Y_{zi}(z) = \dfrac{-1}{1 - 0{,}5z^{-1}} = \dfrac{-z}{z - 0{,}5}$ $\;\Rightarrow\;$ $y_{zi}[n] = -(0{,}5)^n u[n]$

**ZSR:** $Y_{zs}(z) = \dfrac{2z^2}{(z - 0{,}25)(z - 0{,}5)}$; $\;\dfrac{Y_{zs}}{z} = \dfrac{2z}{(z - 0{,}25)(z - 0{,}5)} = \dfrac{-2}{z - 0{,}25} + \dfrac{4}{z - 0{,}5}$
$$y_{zs}[n] = [4(0{,}5)^n - 2(0{,}25)^n]\,u[n]$$

**Celkem:** $\boxed{y[n] = [3(0{,}5)^n - 2(0{,}25)^n]\,u[n]}$

*Kontrola:* $y[0] = 0{,}5\cdot(-2) + 2 = 1$; vzorec $3 - 2 = 1$ ✓. $y[1] = 0{,}5 + 0{,}5 = 1$; vzorec $1{,}5 - 0{,}5 = 1$ ✓.

### Systémy popsané přenosovou funkcí
1. **ZSR:** z přenosové funkce určíme $Y(z) = X(z)H(z)$ a provedeme zpětnou transformaci,
2. **ZIR:** z $H(z)$ sestavíme diferenční rovnici, transformujeme ji s počátečními podmínkami a postupujeme jako výše.

#### Řešený příklad 2
*$H(z) = \dfrac{z^2}{z^2 - \frac16z - \frac16}$, vstup $x[n] = 4u[n]$, počáteční podmínky $y[-1] = 0$, $y[-2] = 12$. Určete ZIR, ZSR, homogenní a partikulární řešení.*

Póly: $z^2 - \frac16z - \frac16 = (z - \frac12)(z + \frac13)$. Diferenční rovnice: $y[n] - \frac16y[n-1] - \frac16y[n-2] = x[n]$.

**ZSR:** $Y_{zs}(z) = H(z)X(z) = \dfrac{z^2}{(z - \frac12)(z + \frac13)}\cdot\dfrac{4z}{z - 1}$
$$\frac{Y_{zs}}{z} = \frac{4z^2}{(z - \frac12)(z + \frac13)(z - 1)} = \frac{-2{,}4}{z - \frac12} + \frac{0{,}4}{z + \frac13} + \frac{6}{z - 1}$$
(např. $k_1 = \frac{4\cdot\frac14}{(\frac12 + \frac13)(\frac12 - 1)} = \frac{1}{-5/12} = -2{,}4$)
$$y_{zs}[n] = \left[-2{,}4\left(\tfrac12\right)^n + 0{,}4\left(-\tfrac13\right)^n + 6\right]u[n]$$

**ZIR:** transformace rovnice s nulovým vstupem:
$$Y(z) - \tfrac16\left[z^{-1}Y + y[-1]\right] - \tfrac16\left[z^{-2}Y + z^{-1}y[-1] + y[-2]\right] = 0$$
$$Y_{zi}(z)\left(1 - \tfrac16z^{-1} - \tfrac16z^{-2}\right) = \tfrac16y[-2] = 2 \;\Rightarrow\; Y_{zi}(z) = \frac{2z^2}{(z - \frac12)(z + \frac13)}$$
$$\frac{Y_{zi}}{z} = \frac{1{,}2}{z - \frac12} + \frac{0{,}8}{z + \frac13} \;\Rightarrow\; y_{zi}[n] = \left[1{,}2\left(\tfrac12\right)^n + 0{,}8\left(-\tfrac13\right)^n\right]u[n]$$

**Celková odezva:**
$$y[n] = y_{zs} + y_{zi} = \underbrace{-1{,}2\left(\tfrac12\right)^n + 1{,}2\left(-\tfrac13\right)^n}_{\text{homogenní (přirozená) odezva}} + \underbrace{6}_{\text{partikulární (vynucená)}},\quad n \ge 0$$

*Kontrola:* $y[0] = \frac16\cdot0 + \frac16\cdot12 + 4 = 6$; vzorec $-1{,}2 + 1{,}2 + 6 = 6$ ✓. Partikulární řešení 6 = $H(1)\cdot4 = \frac{1}{1 - \frac16 - \frac16}\cdot4 = 1{,}5\cdot4$ ✓ (ustálená odezva na konstantu = stejnosměrný zisk × vstup).

> **Pozor:** ZIR a homogenní řešení **nejsou totéž**. ZIR je celá odezva na počáteční podmínky; homogenní řešení obsahuje přirozené módy, ale s konstantami určenými z *celkové* odezvy (tedy i z části ZSR). Podobně ZSR obsahuje vynucenou i část přirozené odezvy.

---

## Shrnutí kapitoly 6

- $X(z) = \sum x[k]z^{-k}$ + **ROC** (pravostranný: vně největšího pólu; levostranný: uvnitř nejmenšího; oboustranný: mezikruží).
- Na jednotkové kružnici $z = e^{j2\pi F}$ → DTFT.
- Klíčové vlastnosti: $x[n-N] \to z^{-N}X(z)$, konvoluce → součin, $\alpha^n x[n] \to X(z/\alpha)$, $nx[n] \to -z\,dX/dz$.
- **Přenosová funkce** $H(z) = Y(z)/X(z)$ = Z-transformace $h[n]$; nuly a póly.
- Realizace: přímá forma I, II, **transponovaná**, **kaskádní** (sekce 2. řádu), **paralelní** (parciální zlomky).
- **Stabilita:** $\sum|h| < \infty$ ⇔ ROC obsahuje jednotkovou kružnici; kauzální stabilní ⇔ **všechny póly uvnitř jednotkové kružnice**.
- Inverze: přímý převod, dělení polynomů, **parciální zlomky $X(z)/z$**.
- Jednostranná transformace: posun doprava přidává počáteční podmínky $y[-1], y[-2], \dots$; věty o počáteční a koncové hodnotě; řešení ZIR + ZSR.

### Kontrolní otázky
1. Proč je nutné k Z-transformaci uvádět ROC? Uveďte dva signály se stejným $X(z)$.
2. Určete Z-transformaci a ROC pro $x[n] = (0{,}5)^n u[n] + (-2)^n u[-n-1]$. *(Řešení: $\frac{z}{z - 0{,}5} - \frac{z}{z + 2}$, ROC $0{,}5 < |z| < 2$.)*
3. Je systém $H(z) = \frac{z}{z^2 - 1{,}5z + 0{,}5}$ stabilní? *(Póly 1 a 0,5 → pól na jednotkové kružnici → není BIBO stabilní.)*
4. Najděte inverzní transformaci $X(z) = \frac{z}{(z - 0{,}5)(z - 1)}$ pro pravostranný signál. *(Řešení: $x[n] = 2[1 - (0{,}5)^n]u[n]$.)*
5. Vysvětlete princip transponované struktury.
6. Proč se IIR filtry vysokého řádu realizují jako kaskáda sekcí 2. řádu?
7. Pomocí věty o koncové hodnotě určete ustálenou odezvu systému $H(z) = \frac{0{,}5z}{z - 0{,}5}$ na jednotkový skok. *(Řešení: $\lim_{z\to1}(z-1)\frac{0{,}5z}{z-0{,}5}\frac{z}{z-1} = 1$.)*

---

# 7. Číslicová filtrace – základní vlastnosti filtrů

## 7.1 Číslicové filtry

**Použití:**
- **separace signálů** – oddělení signálů, které se smíchaly (např. potlačení rušení, šumu, odstranění síťového brumu z EKG),
- **restaurace signálů** – obnovení signálu, který byl nějakým způsobem zkreslen.

**Analogové × číslicové filtry:**

| | Analogové | Číslicové |
|---|---|---|
| výhody | levné, rychlé, velký dynamický rozsah (v amplitudě i frekvenci) | **přesnost**, **univerzálnost** (změna = změna koeficientů), vhodné i pro **nízké frekvence**, **stabilita** (časová stálost vlastností) |
| nevýhody | nevhodné pro nízké kmitočty, nepřesné (tolerance součástek), závislé na okolí (teplota) | omezené kmitočtové pásmo (vzorkování), cena, zpoždění výpočtu |

### Popis filtru

Filtr (LTI systém) lze popsat:
- **impulsní odezvou** (odezva na jednotkový impuls),
- **skokovou odezvou** (odezva na jednotkový skok) – je to kumulativní součet impulsní odezvy,
- **frekvenční odezvou** – DTFT impulsní odezvy (resp. její modul v dB).

> **Stačí znát jednu odezvu, ostatní lze bez problémů vyčíslit** (impulsní → integrací/kumulativním součtem skoková, → FFT frekvenční, → 20 log frekvenční v dB).

### Rozdělení filtrů podle účelu

**Frekvenčně selektivní filtry** – účelem je propustit nebo potlačit složky signálu v určitém frekvenčním pásmu. Realizují se jako FIR nebo IIR filtry. Základní typy:

| Typ | Propustné pásmo (passband) | Nepropustné pásmo (stopband) |
|---|---|---|
| **dolní propust** (low-pass) | $\lvert\omega\rvert < \omega_c$ | $\omega_c < \lvert\omega\rvert \le \pi$ |
| **horní propust** (high-pass) | $\omega_c < \lvert\omega\rvert \le \pi$ | $\lvert\omega\rvert < \omega_c$ |
| **pásmová propust** (band-pass) | $\omega_{c1} < \lvert\omega\rvert < \omega_{c2}$ | ostatní |
| **pásmová zádrž** (band-stop, band-reject) | mimo $\langle\omega_{c1}, \omega_{c2}\rangle$ | $\omega_{c1} < \lvert\omega\rvert < \omega_{c2}$ |

Další typy (viz 7.5): integrátor, derivátor, Hilbertův transformátor, fázové posouvače (all-pass), klouzavý průměr…

---

## 7.2 Frekvenční charakteristika a její grafická interpretace

Frekvenční odezvu získáme z přenosové funkce dosazením $z = e^{j\Omega} = e^{j2\pi F}$ – tj. **vyhodnocením $H(z)$ na jednotkové kružnici**:

| bod na kružnici | $z$ | $\Omega$ | $F$ |
|---|---|---|---|
| kladná reálná osa | $1$ | $0$ | $0$ (stejnosměrná složka) |
| kladná imaginární osa | $j$ | $\pi/2$ | $0{,}25$ |
| záporná reálná osa | $-1$ | $\pi$ | $0{,}5$ (polovina vzorkovací frekvence) |
| záporná imaginární osa | $-j$ | $-\pi/2$ | $-0{,}25$ |

Pohybem bodu $z = e^{j\Omega}$ po horní polovině kružnice od $z = 1$ do $z = -1$ projdeme celé frekvenční pásmo $0 \le F \le 0{,}5$.

### Geometrická interpretace

Pro $H(z) = K\dfrac{\prod(z - z_i)}{\prod(z - p_k)}$ je v bodě $z = e^{j\Omega_0}$ každý činitel $(e^{j\Omega_0} - z_i)$ **vektor od nuly $z_i$ k bodu na kružnici** (podobně pro póly). Proto:

$$|H(\Omega_0)| = K\cdot\frac{\text{součin vzdáleností bodu } e^{j\Omega_0} \text{ od nul}}{\text{součin vzdáleností bodu } e^{j\Omega_0} \text{ od pólů}}$$

a fáze je součet úhlů vektorů od nul minus součet úhlů vektorů od pólů.

*Příklad ze slidu:*
$$H(z) = \frac{8(z - 1)}{(z - 0{,}6 - j0{,}6)(z - 0{,}6 + j0{,}6)}, \qquad |H(\Omega_0)| = 8\frac{|\mathbf{N}_1|}{|\mathbf{D}_1|\,|\mathbf{D}_2|}$$
Póly $0{,}6 \pm j0{,}6 = 0{,}85e^{\pm j\pi/4}$ leží blízko kružnice u $\Omega = \pi/4$ ($F = 1/8$) → tam je $|\mathbf{D}_1|$ malé → **maximum** charakteristiky. Nula v $z = 1$ → nulový přenos pro $F = 0$. Výsledek: **pásmová propust** s maximem kolem $F = 1/8$.

**Pravidla pro „čtení“ diagramu nul a pólů:**
- **pól blízko jednotkové kružnice** → **vrchol (rezonance)** charakteristiky na odpovídající frekvenci (čím blíž, tím vyšší a užší),
- **nula na jednotkové kružnici** → **nulový přenos** na odpovídající frekvenci; nula blízko kružnice → hluboký pokles,
- póly a nuly v počátku ovlivňují jen fázi (vzdálenost od počátku je vždy 1).

**Příklady ze slidu (4 filtry):**
- **filtr 1:** nula v počátku (délka vektoru čitatele = 1), pól na kladné reálné ose blízko $z = 1$ – délka vektoru jmenovatele **roste** s frekvencí → **dolní propust**,
- **filtr 2:** nula v počátku, pól blízko $z = -1$ – délka vektoru jmenovatele **klesá** s frekvencí → **horní propust**,
- **filtr 3:** nula v $z = 1$ – pro $\Omega = 0$ je délka vektoru čitatele nulová a s rostoucí frekvencí poměr čitatel/jmenovatel roste → **horní propust**,
- **filtr 4:** nuly na kružnici v bodě A (a sdruženém), póly těsně vedle uvnitř – v bodě A je přenos nulový, jinde je poměr délek vektorů přibližně konstantní (≈ 1) → **pásmová zádrž** (úzká – notch).

---

## 7.3 FIR a IIR filtry

### FIR (Finite Impulse Response) – filtry s konečnou impulsní odezvou
- impulsní charakteristika má **konečný počet hodnot** ($N$ hodnot pro filtr délky $N$),
- jsou **vždy stabilní** (všechny póly v počátku),
- **lze je navrhnout s lineární fází** (náběžná a doběžná hrana impulsní odezvy jsou shodné – symetrie),
- filtr s lineární fází zpožďuje vstupní signál o $M = (N-1)/2$ vzorků – tzv. **skupinové zpoždění**,
- potřebují **vysoký řád** pro strmé charakteristiky.

### IIR (Infinite Impulse Response) – filtry s nekonečnou impulsní odezvou
- impulsní charakteristika **nemá konečný počet hodnot** (rekurzivní struktura, zpětná vazba),
- jsou **výrazně nižšího řádu** než FIR filtry se stejnými vlastnostmi, z čehož plyne:
  - **nižší výpočetní složitost**,
  - **kratší zpoždění** mezi vstupem a výstupem,
- **není zaručena stabilita** (póly musí být uvnitř jednotkové kružnice),
- **fáze není lineární** a doladění fázové charakteristiky je obtížné,
- obecný návrh je relativně složitý (obvykle se vychází z analogových prototypů – kap. 9),
- jsou **citlivé na numerickou přesnost** výpočtů (kvantizace koeficientů, mezní cykly).

### Skupinové zpoždění

Fáze $\phi(F)$ určuje, o kolik se jednotlivé frekvenční složky zpozdí. **Skupinové zpoždění**
$$\tau(F) = -\frac{1}{2\pi}\frac{d\phi(F)}{dF}\quad[\text{vzorky}]$$
Je-li **fáze lineární** $\phi(F) = -2\pi MF$, je $\tau = M$ **konstantní** – všechny složky se zpozdí o stejný počet vzorků a **tvar signálu se nezkreslí** (jen posune). Nelineární fáze způsobuje **fázové zkreslení** (různé složky se zpozdí různě) – kritické např. u EKG, kde záleží na tvaru vln.

---

## 7.4 FIR filtry s lineární fází

### Vlastnosti

1. **Impulsní odezva $h[n]$ je symetrická** (nebo antisymetrická) **kolem středového bodu**.
2. Frekvenční odezva: $H(F) = A(F)\,e^{-j2\pi MF}$ (symetrická $h[n]$), $\;H(F) = jA(F)\,e^{-j2\pi MF}$ (antisymetrická $h[n]$), kde $A(F)$ je reálná **amplitudová funkce** (může měnit znaménko).
3. **Všechny póly jsou v bodě $z = 0$.**
4. **Nuly se obecně vyskytují v komplexně sdružených recipročních čtveřicích** ($r$, $r^*$, $1/r$, $1/r^*$):
   - nuly **na jednotkové kružnici** jen v komplexně sdružených **dvojicích** (zde $1/r^* = r$),
   - nuly **na reálné ose** jen v **recipročních dvojicích** ($r$, $1/r$),
   - nuly v $z = 1$ a $z = -1$ mohou být **samostatné**.
5. Pro **antisymetrickou** $h[n]$ je v $z = 1$ **lichý** počet nul, pro **symetrickou** **sudý** počet.
6. Je-li $h[n] = \pm h[-n]$, pak $H(z) = \pm H(1/z)$ (odtud reciproká symetrie nul).

### Čtyři typy FIR filtrů s lineární fází

($N$ = délka filtru, $M = (N-1)/2$ = střed symetrie)

**Typ 1 – sudá symetrie, lichý počet vzorků** (střed $M$ je celé číslo):
$$H_1(F) = \left[h[M] + 2\sum_{k=0}^{M-1}h[k]\cos[(M-k)2\pi F]\right]e^{-j2\pi MF} = A_1(F)\,e^{-j2\pi MF}$$
- lineární fáze $-2\pi MF$, konstantní skupinové zpoždění $M$,
- $A_1(F)$ je **sudě symetrická kolem $F = 0$ i $F = 0{,}5$**,
- $|H_1(0)|$ i $|H_1(0{,}5)|$ **mohou být nenulové**,
- **použití:** vhodný pro **všechny typy filtrů**; jediný typ vhodný pro **pásmovou zádrž** (a univerzálně i pro horní propust).

**Typ 2 – sudá symetrie, sudý počet vzorků** (střed $M$ mezi dvěma vzorky):
$$H_2(F) = \left[2\sum_{k=0}^{M-1/2}h[k]\cos[(M-k)2\pi F]\right]e^{-j2\pi MF} = A_2(F)\,e^{-j2\pi MF}$$
- lineární fáze $-2\pi MF$, skupinové zpoždění $M$ (neceločíselné),
- $A_2(F)$ je sudá kolem $F = 0$, **lichá kolem $F = 0{,}5$**,
- **$|H_2(0{,}5)| = 0$ vždy** → nelze realizovat horní propust ani pásmovou zádrž,
- **použití:** dolní propust, pásmová propust. (Slide uvádí i diferenciátor a Hilbertův transformátor – ty ale vyžadují antisymetrii, tedy typ 3 nebo 4.)

**Typ 3 – lichá symetrie (antisymetrie), lichý počet vzorků:**
$$H_3(F) = j\left[2\sum_{k=0}^{M-1}h[k]\sin[(M-k)2\pi F]\right]e^{-j2\pi MF} = A_3(F)\,e^{j(0{,}5\pi - 2\pi MF)}$$
- lineární fáze $\pi/2 - 2\pi MF$, konstantní skupinové zpoždění $M$; prostřední vzorek $h[M] = 0$,
- $A_3(F)$ je **lichá kolem $F = 0$ i $F = 0{,}5$**,
- **$|H_3(0)| = |H_3(0{,}5)| = 0$ vždy**,
- **použití:** pásmová propust, diferenciátor (jen v omezeném pásmu), Hilbertův transformátor.

**Typ 4 – lichá symetrie, sudý počet vzorků:**
$$H_4(F) = j\left[2\sum_{k=0}^{M-1/2}h[k]\sin[2(M-k)\pi F]\right]e^{-j2\pi MF} = A_4(F)\,e^{j(0{,}5\pi - 2\pi MF)}$$
- lineární fáze $\pi/2 - 2\pi MF$, skupinové zpoždění $M$,
- $A_4(F)$ je lichá kolem $F = 0$, **sudá kolem $F = 0{,}5$**,
- **$|H_4(0)| = 0$ vždy**,
- **použití:** horní propust, pásmová propust, diferenciátor, Hilbertův transformátor.

**Souhrn:**

| Typ | Symetrie | Délka $N$ | $H(0)$ | $H(0{,}5)$ | Nuly v $z = 1$ | Nuly v $z = -1$ | Typické použití |
|---|---|---|---|---|---|---|---|
| 1 | sudá | lichá | libovolné | libovolné | sudý počet | sudý počet | všechny selektivní filtry (DP, HP, PP, PZ) |
| 2 | sudá | sudá | libovolné | **0** | sudý počet | **lichý** počet | DP, PP |
| 3 | lichá | lichá | **0** | **0** | **lichý** počet | **lichý** počet | PP, Hilbert, (derivátor) |
| 4 | lichá | sudá | **0** | libovolné | **lichý** počet | sudý počet | HP, PP, derivátor, Hilbert |

(„sudý počet“ zahrnuje i nulu, tj. nula tam nemusí být vůbec.)

> **Slide 15:** „Symetrie typu 1 a 2 se používají u základních frekvenčně selektivních filtrů, typ 3 u derivátoru, typ 4 u Hilbertova transformátoru.“
> *Poznámka:* v běžné literatuře (Mitra, Oppenheim) se uvádí spíše opačně – **typ 4 je přirozenější volbou pro derivátor** (ideální derivátor má největší zisk právě u $F = 0{,}5$, kde typ 3 musí mít nulu), **typ 3 se často používá pro Hilbertův transformátor** (každý druhý koeficient je nulový → úspora výpočtu). Oba antisymetrické typy mají fázi $\pi/2 - 2\pi MF$, tedy „posun o 90°“, který derivátor i Hilbertův transformátor potřebují.

Nuly v $z = \pm1$ přímo vysvětlují omezení: nula v $z = -1$ znamená $H(0{,}5) = 0$, nula v $z = 1$ znamená $H(0) = 0$.

### Řešené příklady

**1) Popisuje $H(z) = 1 + 2z^{-1} + 2z^{-2} + z^{-3}$ filtr s lineární fází?**
$h[n] = \{1, 2, 2, 1\}$ je sudě symetrická, sudá délka → **ano, typ 2**. Kontrola nul: $z^3 + 2z^2 + 2z + 1 = (z + 1)(z^2 + z + 1)$ → nula v $z = -1$ (lichý počet ✓) a nuly $e^{\pm j2\pi/3}$ na jednotkové kružnici.

**2) Diagram nul a pólů pro $h[n] = \delta[n+2] + 4{,}25\delta[n] + \delta[n-2]$:**
$H(z) = z^2 + 4{,}25 + z^{-2} = \dfrac{z^4 + 4{,}25z^2 + 1}{z^2}$ → nuly $\pm j0{,}5$ a $\pm j2$ (reciproká čtveřice), dvojnásobný pól v počátku (viz kap. 6). Symetrická → lineární (zde dokonce nulová) fáze.

**3) Diagram nul a pólů pro $H(z) = z^2 + 2{,}5z - 2{,}5z^{-1} - z^{-2}$; jde o filtr s lineární fází?**
$h[n] = \{1, 2{,}5, \underset{\uparrow}{0}, -2{,}5, -1\}$ – **antisymetrická, lichá délka → typ 3** (lineární fáze ✓).
$z^4 + 2{,}5z^3 - 2{,}5z - 1 = (z^2 - 1)(z^2 + 2{,}5z + 1) = (z-1)(z+1)(z + 0{,}5)(z + 2)$
→ nuly $z = 1$, $z = -1$ (po jedné – liché počty ✓) a reciproká dvojice $-0{,}5$, $-2$ na reálné ose.

**4) Určete všechny nuly filtru typu 1 nejnižšího řádu, víme-li, že nuly jsou v $z = 0{,}5e^{j\pi/3}$ a $z = 1$.**
- $0{,}5e^{j\pi/3}$ není na kružnici ani na reálné ose → celá **reciproká čtveřice**: $0{,}5e^{\pm j\pi/3}$, $2e^{\pm j\pi/3}$,
- typ 1 vyžaduje **sudý** počet nul v $z = 1$ → **dvojnásobná** nula v $z = 1$.

Celkem 6 nul → délka $N = 7$:
$$H(z) = \frac{(z-1)^2(z^2 - 0{,}5z + 0{,}25)(z^2 - 2z + 4)}{z^6}, \quad h[n] = \{1; -4{,}5; 11{,}25; -15{,}5; 11{,}25; -4{,}5; 1\}$$

**5) Totéž pro filtr typu 2.**
Navíc typ 2 vyžaduje **lichý** počet nul v $z = -1$ → přidáme nulu v $z = -1$. Celkem 7 nul, délka $N = 8$:
$h[n] = \{1; -3{,}5; 6{,}75; -4{,}25; -4{,}25; 6{,}75; -3{,}5; 1\}$ (předchozí $h$ konvolvované s $\{1, 1\}$).

**6) Přenosová funkce a impulsní odezva filtru typu 3 nejkratší délky, má-li nulu v $z = j$ a dvě nuly v $z = 1$.**
- nula $j$ leží na kružnici → s ní i $-j$,
- typ 3: **lichý** počet nul v $z = 1$ → dvě nestačí, potřebujeme **tři**,
- typ 3: **lichý** počet nul v $z = -1$ → alespoň **jedna**.

Celkem 6 nul → $N = 7$ (lichá ✓):
$$H(z) = \frac{(z^2 + 1)(z-1)^3(z+1)}{z^6} = 1 - 2z^{-1} + z^{-2} - z^{-4} + 2z^{-5} - z^{-6}$$
$$h[n] = \{1, -2, 1, 0, -1, 2, -1\}$$
(antisymetrická kolem středu, $h[3] = 0$ ✓).

---

## 7.5 Další typy filtrů

### Diskrétní integrátor

Počítá integrál pomocí některého z pravidel numerické matematiky:

| | Pravidlo | Diferenční rovnice |
|---|---|---|
| 1 | obdélníkové | $y(n) = y(n-1) + x(n)$ |
| 2 | lichoběžníkové | $y(n) = y(n-1) + \frac12[x(n) + x(n-1)]$ |
| 3 | Simpsonovo | $y(n) = y(n-2) + \frac13x(n) + \frac43x(n-1) + \frac13x(n-2)$ |
| 4 | Tickovo | $y(n) = y(n-2) + 0{,}3584\,x(n) + 1{,}2832\,x(n-1) + 0{,}3584\,x(n-2)$ |

- **Simpsonovo** pravidlo – vhodné pro frekvence $f \in \langle 0, f_{vz}/4\rangle$ (pro vyšší frekvence selhává – má pól v $z = -1$, tj. na $f_{vz}/2$),
- **obdélníkové a lichoběžníkové** – lze použít i pro frekvence blízko $f_{vz}/2$,
- **Tickovo** pravidlo – pro širší pásmo integrace s malou chybou.

**Ideální diskrétní integrátor:**
$$H_{id}(e^{j\theta}) = \frac{1}{j\theta} = \frac{1}{\theta}e^{-j\pi/2}$$
modul $1/|\theta|$ (velký zisk na nízkých frekvencích), fáze $-\pi/2$ pro $\theta > 0$ a $+\pi/2$ pro $\theta < 0$.

### Diskrétní derivátor

Důležitý např. pro **detekci hran v obraze**, určení zrychlení z rychlosti, detekci QRS komplexů v EKG apod.:

| | Diferenční rovnice | |
|---|---|---|
| 1 | $y(n) = x(n) - x(n-1)$ | zpětná diference (kauzální) |
| 2 | $y(n) = \frac12[x(n+1) - x(n-1)]$ | centrální diference |
| 3 | $y(n) = \frac{1}{12}[x(n-2) - 8x(n-1) + 8x(n+1) - x(n+2)]$ | přesnější aproximace (5 bodů) |

**Ideální diskrétní derivátor:**
$$H_{Did}(e^{j\theta}) = j\theta, \qquad |\theta| \le \pi$$
modul $|\theta|$ (lineárně roste s frekvencí – zesiluje vysokofrekvenční šum!), fáze $+\pi/2$ pro $\theta > 0$, $-\pi/2$ pro $\theta < 0$.

### Hilbertův transformátor

Používá se k **získání imaginární složky ze složky reálné** (analytický signál, obálka, okamžitá frekvence). Je to **fázový posouvač**, který posouvá fázi v základním intervalu o $-\pi/2$ pro $\theta > 0$ a o $+\pi/2$ pro $\theta < 0$:

$$H_{HT}(e^{j\theta}) = \begin{cases} -j & 0 \le \theta < \pi \\ \;\;\,j & -\pi \le \theta < 0\end{cases}$$

modul jednotkový (kromě $\theta = 0$), např. $\cos(\Omega n) \to \sin(\Omega n)$.

### Fázové posouvače (all-pass)

Mají **jednotkovou amplitudovou charakteristiku**, ale mění průběh fázové charakteristiky na požadovaný. Nejčastěji se používají v kombinaci s jinými filtry (např. IIR), aby se dosáhlo požadovaného průběhu výsledné fázové charakteristiky (**linearizace fáze** – fázové korektory/ekvalizéry).

### Filtr klouzavý průměr (moving average)

Realizuje **klouzavý průměr** z aktuálního vzorku a $N-1$ předchozích. Používá se pro potlačení periodického rušení superponovaného na konstantní signál, potlačení krátkodobých výkyvů časové řady (vyhlazování trendu v ekonomice), potlačení šumu.

**Nevážený klouzavý průměr** – všechny koeficienty shodné:
$$y(n) = \frac1N\sum_{i=n-(N-1)}^{n}x(i) = \frac1N[x(n-(N-1)) + \dots + x(n-1) + x(n)]$$

**Frekvenční charakteristika:**
$$H(F) = \frac1N\sum_{k=0}^{N-1}e^{-j2\pi kF} = \frac1N\,\frac{\sin(\pi FN)}{\sin(\pi F)}\,e^{-j\pi(N-1)F}$$
$$|H(f)| = \frac1N\left|\frac{\sin(\pi fN)}{\sin(\pi f)}\right|, \qquad \varphi_{MA}(e^{j\theta}) = -(N-1)\frac{\theta}{2}$$

- je to (velmi špatná) **dolní propust** – hlavní lalok do $F = 1/N$, vysoké postranní laloky,
- **nuly na frekvencích $F = k/N$** → přesně potlačí periodické rušení s periodou $N$ vzorků (a jeho harmonické),
- lineární fáze, zpoždění $(N-1)/2$ vzorků.

Na slidu: obdélníkový puls utopený v šumu, filtrovaný 11bodovým a 51bodovým průměrem – **čím delší filtr, tím menší šum, ale tím méně strmé hrany**. Klouzavý průměr je **optimální** pro tuto úlohu: dává nejnižší šum při dané strmosti hran (je to nejlepší filtr v časové oblasti, ale špatný ve frekvenční).

**Opakované použití (vícenásobný průchod)** – jádro se konvolvuje samo se sebou (2 průchody → trojúhelník, 4 průchody → přibližně Gauss): **lépe odstraňuje šum** (nižší postranní laloky), ale **více rozmazává ostré přechody**.

**Klouzavý průměr s exponenciálním zapomínáním** (exponenciálně vážený klouzavý průměr, EWMA) – váhy $V_1^{n-i}$ exponenciálně klesají se stářím vzorku (nejstarší vzorek má nejmenší váhu):
$$y(n) = \frac{1}{\sum_{i=n-(N-1)}^{n}V_1^{n-i}}\sum_{i=n-(N-1)}^{n}V_1^{n-i}x(i) \;\approx\; (1 - V_1)\sum_{i}V_1^{n-i}x(i)$$
což lze počítat rekurzivně (IIR filtr 1. řádu):
$$y(n) = V_1\,y(n-1) + (1 - V_1)\,x(n), \qquad H(e^{j\theta}) = \frac{1 - V_1}{1 - V_1e^{-j\theta}}$$
(pro $V_1 = 0{,}5$ klesá modul z 1 na $0{,}33$ u $\theta = \pi$; fáze je nelineární).

---

## 7.6 Ideální filtry a frekvenční transformace

### Ideální dolní propust

$$H_{LP}(F) = \mathrm{rect}\!\left(\frac{F}{2F_C}\right) = \begin{cases}1 & |F| < F_C \\ 0 & F_C < |F| \le 0{,}5\end{cases}$$

Impulsní odezva (inverzní DTFT):
$$h_{LP}[n] = \int_{-0{,}5}^{0{,}5}H_{LP}(F)e^{j2\pi nF}dF = \int_{-F_C}^{F_C}\cos(2\pi nF)\,dF = \frac{\sin(2\pi nF)}{2\pi n}\Big|_{-F_C}^{F_C} = \frac{2\sin(2\pi nF_C)}{2\pi n}$$
$$\boxed{h_{LP}[n] = \frac{2F_C\sin(2\pi nF_C)}{2\pi nF_C} = 2F_C\,\mathrm{sinc}(2nF_C)}$$

### Frekvenční transformace

Filtry se obvykle navrhují jako **dolní propusti**; přechod na jiný typ (HP, PP, PZ) se provádí **frekvenční transformací**.

**Horní propust – dva způsoby:**

1. **Posun spektra dolní propusti o 0,5** (modulace $(-1)^n$): $H_{HP}(F) = H(F - 0{,}5)$
$$F_{H1} = 0{,}5 - F_C, \qquad h_{H1}[n] = (-1)^n h[n] = 2(-1)^nF_C\,\mathrm{sinc}(2nF_C)$$
(mezní frekvence horní propusti je $0{,}5 - F_C$).

2. **Odečtení dolní propusti od all-pass filtru** (filtru s $H = 1$, $h = \delta[n]$): $H_{HP}(F) = 1 - H(F)$
$$F_{H2} = F_C, \qquad h_{H2}[n] = \delta[n] - 2F_C\,\mathrm{sinc}(2nF_C)$$

**Pásmová propust** – posun dolní propusti na $\pm F_0$ (modulace kosinem):
$$H_{BP}(F) = H(F + F_0) + H(F - F_0), \qquad h_{BP}[n] = 2h[n]\cos(2\pi nF_0) = 4F_C\cos(2\pi nF_0)\,\mathrm{sinc}(2nF_C)$$
Propustné pásmo je $\langle F_0 - F_C, F_0 + F_C\rangle$, tj. $F_0 = \frac{F_1 + F_2}{2}$, $F_C = \frac{F_2 - F_1}{2}$.

**Pásmová zádrž** – doplněk pásmové propusti:
$$H_{BS}(F) = 1 - H_{BP}(F), \qquad h_{BS}[n] = \delta[n] - h_{BP}[n] = \delta[n] - 4F_C\cos(2\pi nF_0)\,\mathrm{sinc}(2nF_C)$$

#### Řešený příklad
*Najděte impulsní odezvy ideálních filtrů:*

a) **dolní propust $F_C = 0{,}25$:** $h[n] = 0{,}5\,\mathrm{sinc}(0{,}5n)$.

b) **horní propust $F_C = 0{,}3$:** dolní propust s $F_C = 0{,}5 - 0{,}3 = 0{,}2$ posunutá o 0,5: $h_{HP}[n] = (-1)^n(0{,}4)\,\mathrm{sinc}(0{,}4n)$; nebo z dolní propusti s $F_C = 0{,}3$: $h_{HP}[n] = \delta[n] - 0{,}6\,\mathrm{sinc}(0{,}6n)$.

c) **pásmová propust $F_1 = 0{,}1$, $F_2 = 0{,}3$:** $F_C = 0{,}1$, $F_0 = 0{,}2$: $h_{BP}[n] = 0{,}4\cos(0{,}4n\pi)\,\mathrm{sinc}(0{,}2n)$.

d) **pásmová zádrž $F_1 = 0{,}2$, $F_2 = 0{,}4$:** $F_C = 0{,}1$, $F_0 = 0{,}3$: $h_{BS}[n] = \delta[n] - 0{,}4\cos(0{,}6n\pi)\,\mathrm{sinc}(0{,}2n)$.

### Realizovatelnost ideálních filtrů

Ideální filtry jsou v praxi **nerealizovatelné**, protože:
- jsou **nekauzální** – funkce sinc je symetrická kolem $n = 0$ a nekonečně dlouhá,
- jsou **nestabilní** – funkce sinc není absolutně sčitatelná ($\sum|h[n]| = \infty$).

Aby je bylo možné realizovat, je nutné:
1. **omezit impulsní odezvu oknem** (konečná délka – suma pak konverguje),
2. **posunout ji o $M$ vzorků** – impulsní odezva se stane kauzální,
3. zachovat **symetrii kolem středového bodu** – podmínka lineární fáze.

> **Použité okno vždy deformuje tvar frekvenční charakteristiky!** Spektrum okénkovaného filtru je **konvolucí ideální charakteristiky se spektrem okna**:
> - **pravoúhlé okno** (jeho spektrum je Dirichletovo jádro s vysokými postranními laloky) → **zvlnění** v propustném i nepropustném pásmu (Gibbsův jev) a ostrý přechod,
> - **trojúhelníkové okno** (spektrum je Fejérovo jádro – nezáporné, s nízkými postranními laloky) → **hladká** charakteristika bez zvlnění, ale **širší přechodové pásmo**.

Podrobně se návrhem FIR filtrů metodou oken zabývá kapitola 8.

---

## Shrnutí kapitoly 7

- Filtr je úplně popsán impulsní, skokovou nebo frekvenční odezvou; ostatní lze dopočítat.
- Frekvenční odezva = $H(z)$ na jednotkové kružnici; $|H| = K\cdot\frac{\prod\text{vzdáleností od nul}}{\prod\text{vzdáleností od pólů}}$; pól u kružnice → vrchol, nula na kružnici → nulový přenos.
- FIR: vždy stabilní, lze lineární fázi, vysoký řád. IIR: nízký řád, nelineární fáze, nutno hlídat stabilitu.
- Lineární fáze ⇔ (anti)symetrická $h[n]$; skupinové zpoždění $M = (N-1)/2$; nuly v reciprokých čtveřicích.
- Typy 1–4: typ 2 má $H(0{,}5) = 0$, typ 3 má $H(0) = H(0{,}5) = 0$, typ 4 má $H(0) = 0$.
- Speciální filtry: integrátor ($1/j\theta$), derivátor ($j\theta$), Hilbert ($-j\,\mathrm{sgn}\,\theta$), all-pass, klouzavý průměr ($\frac1N\frac{\sin\pi FN}{\sin\pi F}$), EWMA.
- Ideální DP: $h = 2F_C\mathrm{sinc}(2nF_C)$; HP: $(-1)^nh[n]$ nebo $\delta - h$; PP: $2h\cos(2\pi nF_0)$; PZ: $\delta - h_{BP}$.
- Ideální filtry jsou nekauzální a nestabilní → okno + posun; okno deformuje charakteristiku.

### Kontrolní otázky
1. Proč má FIR filtr se symetrickou impulsní odezvou lineární fázi? Jaké je jeho skupinové zpoždění?
2. Lze navrhnout horní propust jako FIR filtr typu 2? Proč?
3. Odhadněte z diagramu nul a pólů typ filtru: pól v $z = 0{,}9$, nula v $z = -1$. *(Dolní propust.)*
4. Navrhněte jednoduchý FIR filtr, který zcela potlačí síťových 50 Hz při $f_s = 500$ Hz. *(Nuly v $e^{\pm j2\pi\cdot0{,}1}$: $H(z) = 1 - 2\cos(0{,}2\pi)z^{-1} + z^{-2}$; nebo klouzavý průměr s $N = 10$.)*
5. Jaká je impulsní odezva ideální pásmové propusti 0,15–0,25?
6. Proč derivátor zesiluje šum?
7. Jaký je rozdíl ve výsledku okénkování pravoúhlým a trojúhelníkovým oknem?

---

# 8. Návrh FIR filtrů

## 8.1 Úlohy při návrhu FIR filtru

Při návrhu FIR filtru řešíme obvykle tyto problémy:

1. **volba požadované frekvenční odezvy** – nejčastěji **ideální charakteristika** normovaná k Nyquistově frekvenci, nebo tzv. **toleranční schéma** (meze propustného a nepropustného pásma, povolené zvlnění a útlum),
2. **stanovení délky (řádu) filtru** – nejčastěji odhadem (první krok), v dalších krocích se **iterativně zpřesňuje**,
3. **zavedení kritéria kvality** pro posouzení skutečné frekvenční charakteristiky:
   - a) **průměr kvadratické chyby** (kritérium **nejmenších čtverců**) – minimalizujeme
   $$E = \sum_{k=0}^{N-1}\Big[\,|H(j\omega_k)| - |H_d(j\omega_k)|\,\Big]^2$$
   - b) **maximální chyba** frekvenční charakteristiky ve specifikované oblasti (propustné, nepropustné) – tzv. **Čebyševova (minimaxová) aproximace** → rovnoměrné zvlnění,
   - c) kritérium založené na aproximaci **Taylorovým rozvojem** požadované charakteristiky – tzv. **Butterworthova (maximálně plochá) aproximace**,
4. **volba metody stanovení koeficientů** filtru.

### Metody návrhu koeficientů
- **metoda Fourierových řad s využitím okna** (okénková metoda, window method),
- **metoda frekvenčního vzorkování**,
- **Remezův algoritmus** (Parks–McClellan) – optimalizovaný návrh s Čebyševovou aproximací chyby (filtr s rovnoměrným zvlněním, *equiripple*; pro dané zadání dává nejkratší filtr).

---

## 8.2 Metoda Fourierových řad (okénková metoda) – postup 1

**Princip:** frekvenční charakteristika diskrétního filtru je **periodická** (s periodou $2\pi$), lze ji tedy rozvinout do **Fourierovy řady**. Koeficienty této řady jsou právě vzorky impulsní odezvy. Rozvoj se **omezí na konečný počet koeficientů** – výsledná odezva aproximuje požadovanou charakteristiku.

**Algoritmus:**
1. specifikace požadované frekvenční charakteristiky $H_d(j\omega)$,
2. stanovení délky filtru $N = M + 1$,
3. výpočet koeficientů $h[n]$, $n = 0, 1, \dots, M$:
$$h_d[n] = \frac{1}{2\pi}\int_{2\pi}|H_d(j\omega)|\,[\cos(m\omega) + j\sin(m\omega)]\,d\omega, \qquad m = n - \frac M2$$
(posun o $M/2$ zajišťuje kauzalitu a symetrii kolem středu),
4. výsledné koeficienty **vynásobíme vhodným oknem**, abychom omezili zákmity ve frekvenční charakteristice (**Gibbsův jev**),
5. pro výsledné koeficienty vypočteme skutečnou odezvu filtru; nevyhovuje-li, zvolíme jiné $N$ (nebo okno) a postup opakujeme.

> **Gibbsův jev:** prosté oříznutí Fourierovy řady (= pravoúhlé okno) způsobí u nespojitosti zákmit, jehož velikost (~9 % skoku, tj. útlum v nepropustném pásmu jen ~21 dB) **se s rostoucí délkou filtru nezmenšuje** – jen se zužuje. Pomůže jen jiné okno.

### Příklad 1 – dolní propust
*Navrhněte FIR filtr ($N = 21$) aproximující ideální dolní propust s mezní frekvencí $f_d = 2$ kHz při vzorkovací frekvenci $f_v = 8$ kHz.*

Normovaná mezní frekvence: $\omega_d = 2\pi f_d/f_v = 2\pi\cdot2/8 = \pi/2$. Protože $|H_d|$ je sudá, sinová část integrálu je nulová:
$$h_d[n] = \frac{1}{2\pi}\int_{-\omega_d}^{\omega_d}\cos(m\omega)\,d\omega = \frac{\sin(m\omega)}{2\pi m}\Big|_{-\pi/2}^{\pi/2} = \frac{\sin(\pi m/2)}{\pi m}, \qquad m = n - 10$$

Pro $m = 0$ ($n = 10$) použijeme l'Hospitalovo pravidlo:
$$h_d[10] = \frac{(\pi/2)\cos(\pi m/2)}{\pi}\Big|_{m=0} = 0{,}5$$

| $n$ | 0, 20 | 1, 19 | 2, 18 | 3, 17 | 4, 16 | 5, 15 | 6, 14 | 7, 13 | 8, 12 | 9, 11 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $h[n]$ | 0 | 0,03537 | 0 | −0,04547 | 0 | 0,06366 | 0 | −0,10610 | 0 | 0,31831 | 0,5 |

(Sudé vzorky kromě středu jsou nulové – jde o půlpásmový filtr, viz 8.6.)

### Příklad 2 – horní propust
*$N = 21$, ideální horní propust s mezní frekvencí $\omega_h = 3\pi/5$.*
$$h[n] = \begin{cases} 1 - \dfrac{\omega_h}{\pi} & m = 0 \\[2mm] -\dfrac{\sin(m\omega_h)}{m\pi} & m \neq 0\end{cases}$$

| $n$ | 0, 20 | 1, 19 | 2, 18 | 3, 17 | 4, 16 | 5, 15 | 6, 14 | 7, 13 | 8, 12 | 9, 11 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $h[n]$ | 0 | 0,03364 | −0,02339 | −0,02673 | 0,05046 | 0 | −0,07568 | 0,06237 | 0,09355 | −0,30273 | 0,4 |

### Příklad 3 – pásmová propust
*$N = 21$, ideální pásmová propust s mezemi $\omega_d = 2\pi/5$, $\omega_h = 3\pi/5$.*
$$h[n] = \begin{cases} \dfrac{\omega_h - \omega_d}{\pi} & m = 0 \\[2mm] \dfrac{\sin(m\omega_h) - \sin(m\omega_d)}{m\pi} & m \neq 0\end{cases}$$

| $n$ | 0, 20 | 1, 19 | 2, 18 | 3, 17 | 4, 16 | 5, 15 | 6, 14 | 7, 13 | 8, 12 | 9, 11 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $h[n]$ | 0 | 0 | 0,04677 | 0 | −0,10091 | 0 | 0,15136 | 0 | −0,18710 | 0 | 0,2 |

### Příklad 4 – pásmová zádrž
*$N = 21$, nepropustné pásmo 1,6–2,4 kHz při $f_v = 8$ kHz, tj. $\omega_d = 2\pi/5$, $\omega_h = 3\pi/5$.*
$$h[n] = \begin{cases} 1 - \dfrac{\omega_h - \omega_d}{\pi} & m = 0 \\[2mm] \dfrac{\sin(m\omega_d) - \sin(m\omega_h)}{m\pi} & m \neq 0\end{cases}$$
Koeficienty jsou opačné než u pásmové propusti, kromě středu: $h[2] = -0{,}04677$, $h[4] = 0{,}10091$, $h[6] = -0{,}15136$, $h[8] = 0{,}18710$, $h[10] = 0{,}8$, liché nulové.

### Příklad 5 – diferenciátor
*Ideální diferenciátor $H(j\omega) = j\omega$ aproximujeme FIR filtrem s lineární fází s antisymetrií typu 3 (lichý počet vzorků), $N = 21$.*
$$h_d[n] = \frac{1}{2\pi}\int_{-\pi}^{\pi}j\omega\,e^{jm\omega}d\omega = -\frac{1}{2\pi}\int_{-\pi}^{\pi}\omega\sin(m\omega)\,d\omega = \begin{cases}0 & m = 0\\[1mm] \dfrac{\cos(\pi m)}{m} & m \neq 0\end{cases}$$

| $n$ | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| $h[n]$ | −0,1 | 0,11111 | −0,125 | 0,14286 | −0,16667 | 0,2 | −0,25 | 0,33333 | −0,5 | 1 | 0 |

a $h[20 - n] = -h[n]$ (antisymetrie). Frekvenční charakteristika roste přibližně lineárně, u $\omega = \pi$ však musí klesnout k nule (typ 3!) a je zvlněná (Gibbsův jev). (V tabulce na slidu je znaménko minus zapsáno u nesprávného indexu; uvedené hodnoty odpovídají $h[0] \dots h[9]$.)

---

## 8.3 Okna pro omezení zákmitů

V uvedených příkladech bylo použito **pravoúhlé okno** (prosté oříznutí). K většímu omezení zákmitů se častěji používají jiná okna (zápis pro $0 \le n \le M$):

| Okno | $w(n)$, $0 \le n \le M$ |
|---|---|
| trojúhelníkové (Bartlett) | $\dfrac{n}{M/2}$ pro $0 \le n \le \frac M2$; $\;2 - \dfrac{2n}{M}$ pro $\frac M2 \le n \le M$ |
| Hann (hanning, von Hann) | $\frac12\left(1 - \cos\dfrac{2\pi n}{M}\right)$ |
| Hamming | $0{,}54 - 0{,}46\cos\dfrac{2\pi n}{M}$ |
| Blackman | $0{,}42 - 0{,}50\cos\dfrac{2\pi n}{M} + 0{,}08\cos\dfrac{4\pi n}{M}$ |

Ekvivalentně v symetrickém zápisu pro $-0{,}5(N-1) \le n \le 0{,}5(N-1)$:

| Okno | $w[n]$ |
|---|---|
| pravoúhlé (boxcar) | $1$ |
| kosinové | $\cos\!\left(\dfrac{n\pi}{N-1}\right)$ |
| Riemannovo | $\mathrm{sinc}^L\!\left(\dfrac{2n}{N-1}\right)$, $L > 0$ |
| Bartlett | $1 - \dfrac{2\lvert n\rvert}{N-1}$ |
| von Hann | $0{,}5 + 0{,}5\cos\!\left(\dfrac{2n\pi}{N-1}\right)$ |
| Hamming | $0{,}54 + 0{,}46\cos\!\left(\dfrac{2n\pi}{N-1}\right)$ |
| Blackman | $0{,}42 + 0{,}5\cos\!\left(\dfrac{2n\pi}{N-1}\right) + 0{,}08\cos\!\left(\dfrac{4n\pi}{N-1}\right)$ |
| Kaiser | $\dfrac{I_0\!\left(\pi\beta\sqrt{1 - 4[n/(N-1)]^2}\right)}{I_0(\pi\beta)}$ |

**Klasická okna – vlastnosti pro návrh FIR dolní propusti** ($M + 1$ = délka filtru):

| Okno | 1. postranní lalok | šířka hlavního laloku | šířka přechodového pásma | minimální útlum filtru |
|---|---|---|---|---|
| obdélník | −13 dB | $4\pi/(M+1)$ | $1{,}8\pi/(M+1)$ | **21 dB** |
| trojúhelník | −25 dB | $8\pi/(M+1)$ | $5{,}6\pi/(M+1)$ | **25 dB** |
| Hann | −31 dB | $8\pi/(M+1)$ | $6{,}2\pi/(M+1)$ | **44 dB** |
| Hamming | −41 dB | $8\pi/(M+1)$ | $6{,}6\pi/(M+1)$ | **53 dB** |
| Blackman | −57 dB | $12\pi/(M+1)$ | $11\pi/(M+1)$ | **74 dB** |

> Útlum filtru v nepropustném pásmu je dán **typem okna**, nikoli délkou filtru. **Délka filtru určuje šířku přechodového pásma.**

**Spektrální charakteristiky oken** (šířky normované délkou $N$):

| Okno | $G_P$ | $G_S/G_P$ | $A_{SL}$ [dB] | $W_M$ | $W_S$ | $W_6$ | $W_3$ | $D_S$ [dB/dek] |
|---|---|---|---|---|---|---|---|---|
| pravoúhlé | 1 | 0,2172 | 13,3 | 1 | 0,81 | 0,6 | 0,44 | 20 |
| kosinové | 0,6366 | 0,0708 | 23 | 1,5 | 1,35 | 0,81 | 0,59 | 40 |
| Riemann | 0,5895 | 0,0478 | 26,4 | 1,64 | 1,5 | 0,86 | 0,62 | 40 |
| Bartlett | 0,5 | 0,0472 | 26,5 | 2 | 1,62 | 0,88 | 0,63 | 40 |
| von Hann | 0,5 | 0,0267 | 31,5 | 2 | 1,87 | 1,0 | 0,72 | 60 |
| Hamming | 0,54 | 0,0073 | 42,7 | 2 | 1,91 | 0,9 | 0,65 | 20 |
| Blackman | 0,42 | 0,0012 | 58,1 | 3 | 2,82 | 1,14 | 0,82 | 60 |
| Kaiser ($\beta = 2{,}6$) | 0,4314 | 0,0010 | 60 | 2,98 | 2,72 | 1,11 | 0,80 | 20 |

$G_P$ – zisk hlavního laloku; $G_S$ – zisk nejvyššího postranního laloku; $A_{SL}$ – útlum postranních laloků ($G_P/G_S$ v dB); $W_M$ – poloviční šířka hlavního laloku; $W_S$ – poloviční šířka hlavního laloku na úrovni $G_S$; $W_6$, $W_3$ – poloviční šířky na úrovni −6 dB a −3 dB; $D_S$ – rychlost poklesu postranních laloků. Pro Kaiserovo okno: $G_P = \frac{|\mathrm{sinc}(j\beta)|}{I_0(\pi\beta)}$, $\frac{G_S}{G_P} = \frac{0{,}22\pi\beta}{\sinh(\pi\beta)}$, $W_M = (1 + \beta^2)^{1/2}$, $W_S = (0{,}661 + \beta^2)^{1/2}$.

### Okna s nastavitelnými vlastnostmi

**Čebyševovo (Dolphovo–Čebyševovo) okno** – všechny postranní laloky mají **stejnou výšku** (equiripple), kterou lze nastavit parametrem $\gamma$ (úroveň postranních laloků ≈ $-20\gamma$ dB, např. $\gamma = 3$ → −60 dB):
$$w_{CH}(n) = \frac{\cos\!\left\{M\cos^{-1}\!\left[\alpha\cos\!\left(\pi\frac nM\right)\right]\right\}}{\cosh\!\left[M\cosh^{-1}(\alpha)\right]}, \qquad \alpha = \cosh\!\left(\frac1N\cosh^{-1}(10^\gamma)\right), \quad n = 0, 1, \dots, M$$
Čím nižší postranní laloky (větší $\gamma$), tím širší hlavní lalok.

**Kaiserovo okno** – parametrem $\beta$ lze plynule měnit kompromis mezi šířkou hlavního laloku a úrovní postranních laloků:
$$w_K(n) = \frac{I_0\!\left[\beta\left(1 - \left[\frac{n - \alpha}{\alpha}\right]^2\right)^{1/2}\right]}{I_0(\beta)}, \qquad 0 \le n \le M, \quad \alpha = \frac M2$$
($I_0$ – modifikovaná Besselova funkce 1. druhu nultého řádu). S rostoucím $\beta$ (6, 7, 8, 9) klesají postranní laloky (cca od −45 dB k −70 dB) a rozšiřuje se hlavní lalok; $\beta = 0$ dává pravoúhlé okno.

---

## 8.4 Návrh podle tolerančního schématu – postup 2

Stejný princip jako postup 1, ale **řád filtru a vhodné okno se určí z parametrů tolerančního schématu**: $A_s$ (útlum v nepropustném pásmu) a $A_p$ (maximální útlum/zvlnění v propustném pásmu).

### Násobení impulsní odezvy oknem

$N$-bodové okno obsahuje $N$ vzorků (včetně obou koncových) a $N - 1$ intervalů. Násobení oknem = násobení vzorku okna vzorkem impulsní odezvy → vzorky okna musí být **symetrické vůči impulsní odezvě**. Okénkovaná symetrická impulsní odezva dolní propusti:

$$h_w[n] = 2F_C\,\mathrm{sinc}(2nF_C)\,w[n], \qquad -0{,}5(N-1) \le n \le 0{,}5(N-1)$$

Pro **sudé $N$** není index $n$ celé číslo (např. $\pm0{,}5, \pm1{,}5, \dots$) – kauzální odezva pak odpovídá **neceločíselnému zpoždění** $0{,}5(N-1)$.

### Tvar spektra okénkovaného filtru

Spektrum okénkovaného filtru = **periodická konvoluce** ideální charakteristiky se spektrem okna:
- **šířka přechodového pásma $F_T$ ≈ šířka hlavního laloku okna**,
- **nejvyšší zvlnění v nepropustném pásmu ≈ určeno (integrovanými) postranními laloky okna**,
- zvlnění v propustném a nepropustném pásmu je u okénkové metody **stejné** ($\delta_p = \delta_s$).

### Toleranční schéma

| Parametr | Význam |
|---|---|
| $F_p$ | konec propustného pásma |
| $F_s$ | začátek nepropustného pásma |
| $F_T = F_s - F_p$ | šířka přechodového pásma |
| $\delta_p$ | zvlnění v propustném pásmu ($1 \pm \delta_p$) |
| $\delta_s$ | zvlnění (maximální zisk) v nepropustném pásmu |
| $A_p$ [dB] | maximální útlum v propustném pásmu |
| $A_s$ [dB] | minimální útlum v nepropustném pásmu |

**Útlum a zisk:**
- lineárně – útlum je převrácená hodnota zisku: $G = |H(f)|$, $A = 1/|H(f)|$,
- v decibelech – útlum je záporná hodnota zisku: $G_{dB} = 20\log|H(f)|$, $A_{dB} = -20\log|H(f)|$.

**Přepočet mezi zvlněním a útlumy:**
$$A_p\,[\text{dB}] = -20\log\frac{1 - \delta_p}{1 + \delta_p}, \qquad A_s\,[\text{dB}] = -20\log\frac{\delta_s}{1 + \delta_p} \approx -20\log\delta_s \quad(\delta_p \ll 1)$$
$$\delta_p = \frac{10^{A_p/20} - 1}{10^{A_p/20} + 1}, \qquad \delta_s = (1 + \delta_p)\,10^{-A_s/20} \approx 10^{-A_s/20}$$

### Postup návrhu FIR filtru okénkovou metodou

1. **normalizace** analogových frekvencí vzorkovací frekvencí → číslicová frekvence $F = f/S$,
2. stanovení $F_p$ a $F_s$ **prototypu – dolní propusti**,
3. mezní frekvence **$F_C = 0{,}5(F_p + F_s)$** (střed přechodového pásma),
4. **volba okna** z tabulky, které splňuje $A_{WS} \ge A_s$ a $A_{WP} \le A_p$,
5. **délka okna $N$** z šířky přechodového pásma: $F_T = F_s - F_p = F_{WS} = C/N$ ⇒ $N = C/F_T$ (konstanta $C$ z tabulky),
6. výpočet impulsní odezvy dolní propusti $h(n) = 2F_C\,\mathrm{sinc}(2nF_C)$, $|n| \le 0{,}5(N-1)$,
7. vynásobení oknem $h_{LP}(n) = w(n)\,h(n)$,
8. **spektrální transformace** na požadovaný typ (pokud je nutná),
9. **ověření frekvenční charakteristiky** a případné doladění $N$ a $F_C$.

**Tabulka parametrů oken pro návrh** (zvlnění $\delta_p = \delta_s$, útlum v propustném pásmu $A_{WP}$, útlum v nepropustném pásmu $A_{WS}$, šířka přechodu $F_{WS} \approx C/N$):

| Okno | $\delta_p = \delta_s$ | $A_{WP}$ [dB] | $A_{WS}$ [dB] | $C$ |
|---|---|---|---|---|
| pravoúhlé | 0,0897 | 1,5618 | 21,7 | 0,92 |
| kosinové | 0,0207 | 0,36 | 33,8 | 2,1 |
| Riemann | 0,0120 | 0,2087 | 38,5 | 2,5 |
| von Hann | 0,0063 | 0,1103 | 44 | 3,21 |
| Hamming | 0,0022 | 0,0384 | 53 | 3,47 |
| Blackman | $1{,}71\cdot10^{-4}$ | $2{,}97\cdot10^{-3}$ | 75,3 | 5,71 |
| Dolph ($R$ = 40 dB) | 0,0036 | 0,0620 | 49 | 3,16 |
| Dolph ($R$ = 50 dB) | $9{,}54\cdot10^{-4}$ | 0,0166 | 60,4 | 3,88 |
| Dolph ($R$ = 60 dB) | $2{,}50\cdot10^{-4}$ | 0,0043 | 72 | 4,6 |
| Harris (0) | $8{,}55\cdot10^{-4}$ | 0,0148 | 61,4 | 5,36 |
| Harris (1) | $1{,}41\cdot10^{-4}$ | $2{,}44\cdot10^{-3}$ | 77 | 7,45 |
| Harris (7) | $5{,}26\cdot10^{-6}$ | $9{,}15\cdot10^{-5}$ | 106 | 7,85 |

### Kaiserovo okno – určení $N$ a $\beta$

1. **zvlnění** v propustné a nepropustné části:
$$\delta_p = \frac{10^{A_p/20} - 1}{10^{A_p/20} + 1}, \qquad \delta_s = 10^{-A_s/20}, \qquad \delta = \min(\delta_p, \delta_s)$$
2. **skutečný útlum** v nepropustné části ze zvlnění $\delta$: $\;A_{s0} = -20\log\delta$ [dB]
3. **délka filtru:**
$$N \ge \begin{cases}\dfrac{A_{s0} - 7{,}95}{14{,}36(F_s - F_p)} + 1 & A_{s0} \ge 21\text{ dB}\\[3mm] \dfrac{0{,}9222}{F_s - F_p} + 1 & A_{s0} < 21\text{ dB}\end{cases}$$
4. **parametr $\beta$:**
$$\beta = \begin{cases}0{,}0351(A_{s0} - 8{,}7) & A_{s0} > 50\text{ dB}\\ 0{,}186(A_{s0} - 21)^{0{,}4} + 0{,}0251(A_{s0} - 21) & 21 \le A_{s0} \le 50\text{ dB}\\ 0 & A_{s0} < 21\text{ dB}\end{cases}$$

(Tyto vzorce platí pro okno ve tvaru $I_0(\pi\beta\sqrt{\cdots})/I_0(\pi\beta)$; v literatuře se často uvádí ekvivalentní tvar s $\beta' = \pi\beta$, např. $\beta' = 0{,}1102(A - 8{,}7)$.)

---

## 8.5 Spektrální transformace

Začínáme návrhem **dolní propusti** s jednotkovým ziskem a mezní frekvencí $F_C = 0{,}5(F_p + F_s)$, jejíž nekauzální impulsní odezva je symetricky oříznuta na délku $N$:
$$h_{LP}[n] = 2F_C\,\mathrm{sinc}(2nF_C), \qquad -0{,}5(N-1) \le n \le 0{,}5(N-1)$$

Pro sudé $N$ jsou hodnoty $n$ neceločíselné a je lepší pracovat s **kauzální verzí** $h_{LP}[k]$, $0 \le k \le N-1$, kde $k = n + 0{,}5(N-1)$:
$$h_{LP}[k] = 2F_C\,\mathrm{sinc}\{2[k - 0{,}5(N-1)]F_C\}, \qquad 0 \le k \le N-1$$

### LP → HP (1. způsob): $H_{HP}(F) = 1 - H_{LP}(F)$

$F_C = 0{,}5(F_p + F_s)$, nekauzální odezva
$$h_{HP}[n] = \delta[n] - h_{LP}[n] = \delta[n] - 2F_C\,\mathrm{sinc}(2nF_C)\,w[n]$$
– **platí jen pro liché $N$** (střední vzorek musí existovat) a jednotkový zisk; pro zisk $G$: $h_{HP}[n] = G\delta[n] - h_{LP}[n]$. Kauzálně:
$$h_{HP}[k] = \delta[k - 0{,}5(N-1)] - 2F_C\,\mathrm{sinc}\{2[k - 0{,}5(N-1)]F_C\}$$

### LP → HP (2. způsob): $H_{HP}(F) = H_{LP}(F - 0{,}5)$

Prototyp dolní propusti s $F_C = 0{,}5 - 0{,}5(F_p + F_s)$, posun spektra o 0,5:
$$h_{HP}[n] = (-1)^n\,2F_C\,\mathrm{sinc}(2nF_C)\,w[n], \qquad\text{kauzálně } h_{HP}[k] = (-1)^kh_{LP}[k]$$
Tato transformace je vhodná pro **FIR i IIR filtry libovolné délky** (sudé i liché).

### LP → BP

1. **Přechodová pásma** $[F_1, F_2]$ a $[F_3, F_4]$ musí být **symetrická** (stejně široká); pokud nejsou, vytvoříme je symetricky podle **užšího** z nich (posuneme $F_1$ nebo $F_4$).
2. **Střed propustného pásma:** $F_0 = 0{,}5(F_2 + F_3) = 0{,}5(F_1 + F_4)$.
3. **Posun spektra dolní propusti** o $\pm F_0$: $H_{BP}(F) = H_{LP}(F + F_0) + H_{LP}(F - F_0)$.
4. **Impulsní odezva:**
$$h_{BP}[n] = 2\cos(2n\pi F_0)\,h_{LP}[n] = 4F_C\,\mathrm{sinc}(2nF_C)\cos(2\pi nF_0), \qquad -\tfrac{N-1}{2} \le n \le \tfrac{N-1}{2}$$
kde mezní frekvence prototypu je $F_C = 0{,}5(F_3 + F_4) - F_0$.
5. Přeindexujeme $k = n + 0{,}5(N-1)$ → kauzální $h_{BP}[k]$.

### LP → BS

**1. způsob** – doplněk pásmové propusti, $H_{BS}(F) = 1 - H_{BP}(F)$; nutný je filtr **se sudou symetrií a lichou délkou $N$ (typ 1)**:
$$h_{BS}[n] = \delta[n] - h_{BP}[n] = \delta[n] - 4F_C\,\mathrm{sinc}(2nF_C)\cos(2\pi nF_0)$$
(Na slidu je omylem uvedeno „typ 4“ – sudá symetrie a lichá délka je podle kap. 7 typ 1; jen ten má nenulový přenos na $F = 0$ i $F = 0{,}5$.)

**2. způsob** – součet dolní propusti s $F_L = 0{,}5(F_1 + F_2)$ a horní propusti s $F_H = 0{,}5(F_3 + F_4)$:
$$h_{BS}[n] = 2F_L\,\mathrm{sinc}(2nF_L) + 2(-1)^n(0{,}5 - F_H)\,\mathrm{sinc}[2n(0{,}5 - F_H)]$$

V obou případech nakonec přeindexujeme $k = n + 0{,}5(N-1)$.

### Řešené příklady (okénková metoda)

**Př. 1 – dolní propust:** $f_p = 2$ kHz, $f_s = 4$ kHz, $A_p = 2$ dB, $A_s = 40$ dB, $S = 20$ kHz.
- $F_p = 0{,}1$, $F_s = 0{,}2$, $F_C = 0{,}15$, $F_T = 0{,}1$,
- okno: potřebujeme $A_{WS} \ge 40$ dB → **von Hann** (44 dB, $A_{WP} = 0{,}11$ dB ≤ 2 dB),
- $N = C/F_T = 3{,}21/0{,}1 = 32{,}1$ → **$N = 33$** (liché),
- $h[n] = 2\cdot0{,}15\,\mathrm{sinc}(0{,}3n)\,w_{Hann}[n] = 0{,}3\,\mathrm{sinc}(0{,}3n)\left[0{,}5 + 0{,}5\cos\frac{2\pi n}{32}\right]$, $|n| \le 16$.

*Alternativně Kaiserovým oknem:* $\delta_p = \frac{10^{0{,}1} - 1}{10^{0{,}1} + 1} = 0{,}115$, $\delta_s = 0{,}01$ → $\delta = 0{,}01$, $A_{s0} = 40$ dB; $N \ge \frac{40 - 7{,}95}{14{,}36\cdot0{,}1} + 1 = 23{,}3$ → $N = 24$ (příp. 25); $\beta = 0{,}186\cdot19^{0{,}4} + 0{,}0251\cdot19 \approx 1{,}08$. Kaiserovo okno vede ke kratšímu filtru.

**Př. 2 – horní propust:** $f_p = 4$ kHz, $f_s = 2$ kHz, ostatní stejné. $F_s = 0{,}1$, $F_p = 0{,}2$, $F_T = 0{,}1$ → opět von Hann, $N = 33$.
- 1. způsob: $F_C = 0{,}15$: $h_{HP}[n] = \delta[n] - 0{,}3\,\mathrm{sinc}(0{,}3n)\,w[n]$,
- 2. způsob: $F_C = 0{,}5 - 0{,}15 = 0{,}35$: $h_{HP}[n] = (-1)^n\,0{,}7\,\mathrm{sinc}(0{,}7n)\,w[n]$.

**Př. 3 – pásmová propust:** propustné pásmo [4, 8] kHz, nepropustné [2, 12] kHz (tj. pod 2 a nad 12 kHz), $A_p = 3$ dB, $A_s = 45$ dB, $S = 25$ kHz.
- $F_1 = 0{,}08$, $F_2 = 0{,}16$, $F_3 = 0{,}32$, $F_4 = 0{,}48$; přechodová pásma 0,08 a 0,16 nejsou symetrická → podle užšího posuneme $F_4 = F_3 + 0{,}08 = 0{,}40$,
- $F_0 = 0{,}5(0{,}16 + 0{,}32) = 0{,}24$, $F_C = 0{,}5(0{,}32 + 0{,}40) - 0{,}24 = 0{,}12$, $F_T = 0{,}08$,
- okno: $A_s = 45$ dB → von Hann (44) nestačí → **Hamming** (53 dB), $C = 3{,}47$,
- $N = 3{,}47/0{,}08 = 43{,}4$ → $N = 45$ (liché),
- $h_{BP}[n] = 4\cdot0{,}12\,\mathrm{sinc}(0{,}24n)\cos(0{,}48\pi n)\,w_{Ham}[n]$, $|n| \le 22$.

---

## 8.6 Půlpásmový (half-band) FIR filtr

**Vlastnosti:**
- má **lichý počet koeficientů**,
- **polovina koeficientů je nulová** ($h[n] = 0$ pro sudé $n \ne 0$, měřeno od středu) → jednodušší realizace, **stačí polovina násobiček**,
- mezní frekvence **$F_C = 0{,}25$** (charakteristika je symetrická kolem bodu $(0{,}25;\,0{,}5)$: $\delta_p = \delta_s$, $F_p + F_s = 0{,}5$) ⇒ při návrhu je potřeba **upravit vzorkovací frekvenci**.

(Proč: $h[n] = 2\cdot0{,}25\,\mathrm{sinc}(0{,}5n) = \frac{\sin(n\pi/2)}{n\pi}$ je nulové pro všechna sudá $n \ne 0$.)

### Postup návrhu
1. **vzorkovací frekvence** $S = 2(f_p + f_s)$ (pro DP a HP), nebo $S = 4f_0$ (pro PP a PZ, $f_0$ = střed pásma),
2. stanovení $F_p$ a $F_s$ dolní propusti,
3. $F_C = 0{,}5(F_p + F_s)$ $(= 0{,}25)$,
4. volba okna z tabulky ($A_{WS} \ge A_s$, $A_{WP} \le A_p$),
5. délka $N$ z $F_T = F_s - F_p = C/N$ – **$N$ musí být liché!**,
6. $h(n) = 2F_C\,\mathrm{sinc}(2nF_C)$, $|n| \le 0{,}5(N-1)$,
7. vynásobení oknem,
8. spektrální transformace (je-li nutná),
9. ověření a případné doladění $N$.

U pásmové propusti/zádrže s $S = 4f_0$ je $F_0 = 0{,}25$ a modulační člen $\cos(2\pi nF_0) = \cos(n\pi/2)$ je nulový pro **lichá** $n$ – opět vychází polovina koeficientů nulová.

#### Řešené příklady
**Půlpásmová dolní propust:** $f_p = 8$ kHz, $f_s = 16$ kHz, $A_p = 1$ dB, $A_s = 50$ dB.
- $S = 2(8 + 16) = 48$ kHz, $F_p = 1/6$, $F_s = 1/3$, $F_C = 0{,}25$ ✓, $F_T = 1/6$,
- okno: 50 dB → **Hamming** (53 dB), $N = 3{,}47/0{,}1667 = 20{,}8$ → $N = 21$ (liché),
- $h[n] = 0{,}5\,\mathrm{sinc}(0{,}5n)\,w_{Ham}[n]$, $|n| \le 10$; nenulové jsou jen $h[0]$ a vzorky s lichým $n$.

**Půlpásmová pásmová zádrž:** nepropustné pásmo [2, 3] kHz, propustné pásmo pod 1 a nad 4 kHz, $A_p = 1$ dB, $A_s = 50$ dB.
- $f_0 = 2{,}5$ kHz → $S = 4f_0 = 10$ kHz; $F_1 = 0{,}1$, $F_2 = 0{,}2$, $F_3 = 0{,}3$, $F_4 = 0{,}4$, $F_0 = 0{,}25$,
- přechodová pásma šířky 0,1; prototyp $F_C = 0{,}1$ (střed přechodu 0,35 minus $F_0$),
- Hamming, $N = 3{,}47/0{,}1 = 34{,}7$ → $N = 35$,
- $h_{BS}[n] = \delta[n] - 0{,}4\,\mathrm{sinc}(0{,}2n)\cos(0{,}5\pi n)\,w_{Ham}[n]$, $|n| \le 17$ – pro lichá $n$ je $\cos(0{,}5\pi n) = 0$.

---

## 8.7 Metoda frekvenčního vzorkování

**Princip:** vycházíme z požadované amplitudové charakteristiky, kterou **navzorkujeme**, a k těmto vzorkům určíme impulsní odezvu (IDFT). Výhodné, má-li charakteristika **složitější tvar** (libovolný průběh).

**Postup:**
1. Vybereme $N$ ekvidistantních vzorků požadované odezvy na intervalu $0 \le F < 1$:
$$H[k] = H(F)\big|_{F = k/N}, \qquad k = 0, 1, \dots, N-1$$
2. Protože $h[n]$ musí být reálná, musí být $H[k]$ **komplexně sdružené kolem $k = 0{,}5N$**; $H[0]$ je samostatný a nastavuje se podle typu filtru ($H[0] = 0$ pro HP a PP).
3. Pro **sudé $N$** není $h[n]$ symetrická; symetrii zajistíme (prodloužením na $N + 1$ vzorků) tak, že nastavíme $h[0] = h[N] = 0{,}5h[0]$.
4. Aby byla $h[n]$ **kauzální**, musíme ji zpozdit → do frekvenční charakteristiky zavedeme **lineární fázi** $H[k] = |H[k]|e^{j\phi[k]}$, kde pro prvních $N/2$ vzorků
$$\phi[k] = \frac{-\pi k(N-1)}{N}, \qquad k = 0, 1, \dots, 0{,}5(N-1)$$
5. Pro filtry **typu 3 a 4** (antisymetrické) přidáme k $\phi[k]$ **konstantní fázový posun** $\pi/2$ (až do indexu $k = 0{,}5N$). Zbývající vzorky jsou komplexně sdružené.
6. Na takto získanou posloupnost aplikujeme **IDFT (IFFT)**.
7. Z koeficientů určíme skutečnou frekvenční charakteristiku, porovnáme s požadovanou, případně **upravíme amplitudy vzorků** (zejména v přechodovém pásmu) a pokračujeme krokem 3.
8. Výslednou impulsní odezvu vynásobíme vhodným oknem.

Výsledná charakteristika **prochází přesně zadanými vzorky**, mezi nimi ale zvlní (interpolace funkcemi typu sinc). **Ostrý skok** mezi vzorky 1 a 0 vyvolá velké zákmity; vložením **přechodového vzorku** (např. 0,5) se zvlnění výrazně zmenší (za cenu širšího přechodu).

#### Řešený příklad – dolní propust frekvenčním vzorkováním
*$N = 10$, vzorky ideální DP v bodech $F = k/10$: $|H[k]| = \{1, 1, 1, 0, 0, 0, 0, 0, 1, 1\}$.*

Pro sudé $N$ s fází $\phi[k] = -0{,}9\pi k$ vychází symetrický filtr typu 2 a IDFT se zjednoduší na
$$h[n] = \frac1N\left[H[0] + 2\sum_{k=1}^{4}|H[k]|\cos\!\left(\frac{2\pi k(n - 4{,}5)}{10}\right)\right] = 0{,}1\left[1 + 2\cos(0{,}2\pi(n - 4{,}5)) + 2\cos(0{,}4\pi(n - 4{,}5))\right]$$
$$h[n] = \{0{,}0716;\ -0{,}0794;\ -0{,}1;\ 0{,}1558;\ 0{,}4520;\ 0{,}4520;\ 0{,}1558;\ -0{,}1;\ -0{,}0794;\ 0{,}0716\}$$
(kontrola: $\sum h = 1 = H[0]$ ✓).

*S přechodovým vzorkem* $|H[2]| = |H[8]| = 0{,}5$: $h[n] = 0{,}1[1 + 2\cos(0{,}2\pi(n - 4{,}5)) + \cos(0{,}4\pi(n - 4{,}5))]$
$$h[n] = \{-0{,}0093;\ -0{,}0485;\ 0;\ 0{,}1867;\ 0{,}3711;\ 0{,}3711;\ 0{,}1867;\ 0;\ -0{,}0485;\ -0{,}0093\}$$
– charakteristika je mnohem méně zvlněná.

*Horní propust* se vzorky $\{0, 0, 0, 1, 1, 1, 1, 1, 0, 0\}$: protože potřebujeme $H(0{,}5) \ne 0$ při sudém $N$, musí jít o **antisymetrický typ 4** (přidaná fáze $\pi/2$). Výsledek je $h_{HP}[n] = (-1)^n h_{LP}[n]$ – tj. posun spektra dolní propusti o 0,5.

---

## Shrnutí kapitoly 8

- Návrh FIR: požadovaná charakteristika (ideál/toleranční schéma), délka, kritérium (nejmenší čtverce, Čebyšev/minimax, Butterworth/maximálně plochá), metoda (okna, frekvenční vzorkování, Remez).
- **Okénková metoda:** $h[n] = h_d[n]\,w[n]$, $h_d$ = Fourierovy koeficienty ideální charakteristiky (DP: $2F_C\mathrm{sinc}(2nF_C)$); pravoúhlé okno → Gibbsův jev (21 dB nezávisle na $N$).
- **Typ okna určuje útlum** (Hann 44 dB, Hamming 53 dB, Blackman 74–75 dB), **délka určuje šířku přechodu** ($N = C/F_T$).
- $F_C = 0{,}5(F_p + F_s)$; $\delta_p = \frac{10^{A_p/20}-1}{10^{A_p/20}+1}$, $\delta_s = 10^{-A_s/20}$.
- **Kaiser:** $N \ge \frac{A_{s0} - 7{,}95}{14{,}36F_T} + 1$, $\beta$ podle $A_{s0}$.
- **Transformace:** HP $= \delta - h_{LP}$ (liché $N$) nebo $(-1)^nh_{LP}$; BP $= 2\cos(2\pi nF_0)h_{LP}$; BS $= \delta - h_{BP}$.
- **Půlpásmový filtr:** $F_C = 0{,}25$, polovina koeficientů nulová, $S = 2(f_p + f_s)$ nebo $4f_0$.
- **Frekvenční vzorkování:** vzorky $|H[k]|$ + lineární fáze → IDFT; přechodové vzorky snižují zvlnění.

### Kontrolní otázky
1. Proč se u okénkové metody nedá zvýšit útlum v nepropustném pásmu prodloužením filtru?
2. Navrhněte (okno, $N$, $h[n]$) dolní propust: $f_p = 1$ kHz, $f_s = 1{,}5$ kHz, $A_s = 50$ dB, $S = 10$ kHz. *(Hamming, $F_T = 0{,}05$, $N = 3{,}47/0{,}05 = 69{,}4 \to 71$, $F_C = 0{,}125$, $h = 0{,}25\,\mathrm{sinc}(0{,}25n)\,w[n]$.)*
3. Proč lze horní propust jako $\delta[n] - h_{LP}[n]$ realizovat jen s lichým $N$?
4. Co je to půlpásmový filtr a proč je výpočetně výhodný?
5. Kdy je výhodná metoda frekvenčního vzorkování? K čemu slouží přechodové vzorky?
6. Spočítejte $\delta_p$ a $\delta_s$ pro $A_p = 1$ dB a $A_s = 60$ dB. *(Řešení: $\delta_p = 0{,}0575$, $\delta_s = 0{,}001$.)*

---

# 9. IIR filtry, adaptivní filtrace a realizační struktury

## 9.1 Vlastnosti IIR filtrů

**Výhody:**
- jsou **výrazně nižšího řádu** než FIR filtry se stejnými vlastnostmi (strmostí), z čehož plyne:
  - **nižší výpočetní složitost**,
  - **kratší zpoždění** mezi vstupem a výstupem.

**Nevýhody:**
- **není zaručena stabilita** (póly musí ležet uvnitř jednotkové kružnice),
- **fáze není lineární**, doladění fázové charakteristiky je obtížné (nutné fázové korektory – all-pass),
- **obecný návrh je relativně složitý** v porovnání s FIR,
- jsou **citlivé na numerickou přesnost** výpočtů (kvantizace koeficientů může posunout póly až ven z kružnice).

---

## 9.2 Základní metody návrhu IIR filtrů

IIR filtry se zpravidla **nenavrhují přímo**, ale z **analogových prototypů** – využívá se propracovaná teorie analogových filtrů. Prototypem je **analogová dolní propust $H_P(s)$ s mezní frekvencí $\omega_C = 1$ rad/s**.

1. **Analogová DP → analogová transformace → analogový filtr $H(s)$ → mapování $s \to z$ → číslicový filtr $H(z)$.**
   Nelze použít pro horní propust (a pásmovou zádrž) při mapování metodou invariantní impulsní odezvy – došlo by k aliasingu (viz 9.5).
2. **Analogová DP → mapování $s \to z$ → číslicová DP $H_P(z)$ → číslicová spektrální transformace (D2D) → libovolný typ číslicového filtru $H(z)$.**
3. **Úprava frekvenční charakteristiky (předzkreslení) a bilineární transformace (A2D)** – analogový prototyp se převede přímo na požadovaný číslicový filtr.

### Tři základní kroky návrhu
1. **stanovení parametrů frekvenční charakteristiky** (toleranční schéma) a **návrh ekvivalentního analogového filtru**,
2. **transformace** přenosové funkce analogového filtru $H(p)$ (resp. $H(s)$) do diskrétní oblasti $H(z)$,
3. **HW nebo SW realizace** filtru.

---

## 9.3 Aproximace amplitudové charakteristiky (ad 1)

Podle toleranční charakteristiky (kap. 8) se navržená amplitudová charakteristika aproximuje jedním ze standardních typů:

### Butterworthova aproximace
- požadovaná frekvenční odezva se v propustném i nepropustném pásmu aproximuje **Taylorovou řadou**,
- charakteristika je **maximálně plochá a monotónní**, bez zákmitů, od $|H(j\Omega)| = 1$ pro $\Omega = 0$ k $|H(j\Omega)| = 0$ pro $\Omega \to \infty$,
- **návrh je jednoduchý**,
- **nevýhoda:** **široké přechodové pásmo**, které lze zúžit jen za cenu **vyššího řádu** (složitější realizace).

$$|H(j\omega)|^2 = \frac{1}{1 + \left(\omega/\omega_c\right)^{2n}}$$

(viz též antialiasingový filtr v kap. 2; v $\omega_c$ je pokles 3 dB pro libovolný řád $n$; strmost za zlomem $-20n$ dB/dekádu).

### Čebyševova aproximace
Dva druhy podle toho, ve které oblasti připustíme zvlnění:
- **Čebyšev I** – v **propustném** pásmu aproximace **Čebyševovým polynomem** → **zvlněný (izoextremální)** průběh; nepropustné pásmo je **hladké** (monotónní):
$$|H(j\omega)|^2 = \frac{1}{1 + \varepsilon^2T_n^2(\omega/\omega_p)}, \qquad \varepsilon^2 = 10^{0{,}1A_p} - 1$$
($T_n$ je Čebyševův polynom $n$-tého řádu, např. $T_2(x) = 2x^2 - 1$),
- **Čebyšev II (inverzní Čebyšev)** – v propustném pásmu **Taylorův polynom** (hladký, monotónní průběh), v **nepropustném** pásmu **Čebyševův polynom** (zvlněný průběh, nuly přenosu).

Oproti Butterworthovi dávají při stejném řádu **strmější přechod**; fáze je však více nelineární (zejména u Čebyševa I u hrany pásma).

### Eliptická aproximace (Cauerův filtr)
- propustná **i** nepropustná část jsou aproximovány Čebyševovým (eliptickým racionálním) průběhem → **zvlnění v obou pásmech**,
- pro zadané požadavky dává **filtr nejnižšího řádu** (nejstrmější přechod), ruční návrh je z uvedených typů **nejsložitější**,
- obvykle se zadávají **3 ze 4 parametrů**, čtvrtý vyjde minimální:
  - maximální zvlnění v propustném pásmu $\delta_1$,
  - šířka přechodového pásma,
  - maximální zvlnění v nepropustném pásmu $\delta_2$,
  - řád filtru $N$.

**Srovnání:**

| Aproximace | Propustné pásmo | Nepropustné pásmo | Řád pro dané zadání | Fáze |
|---|---|---|---|---|
| Butterworth | maximálně plochá | monotónní | nejvyšší | nejlepší (nejméně nelineární) |
| Čebyšev I | zvlněná | monotónní | nižší | horší |
| Čebyšev II | monotónní | zvlněná (nuly) | nižší | lepší než Č. I |
| eliptická | zvlněná | zvlněná | **nejnižší** | nejhorší |

---

## 9.4 Obecný postup návrhu IIR filtru

**Vstup:** toleranční charakteristika – mezní frekvence propustného a nepropustného pásma $f_p$, $f_s$, útlumy $A_p$, $A_s$, vzorkovací frekvence $S$.

**Postup:**
1. **normalizace** mezních frekvencí → $F_p$, $F_s$ (umožní pracovat se vzorkovacím intervalem $t_s = 1$),
2. z normalizovaných frekvencí a útlumů **návrh analogové dolní propusti $H_P(s)$** s mezní frekvencí $\omega_C = 1$ rad/s,
3. **konverze $H_P(s)$ na číslicovou dolní propust $H_P(z)$** (s $\Omega_d = 1$) zvolenou **mapovací funkcí** ($t_s = 1$),
4. **konverze $H_P(z)$ na $H(z)$** číslicovou **spektrální transformací**,
5. pro **bilineární transformaci** lze použít přímo **A2D transformaci** z $H_P(s)$ na $H(z)$.

---

## 9.5 Doplnění: převod analogového filtru na číslicový

> Tato část jde nad rámec slidů – slidy postup jen pojmenovávají. Vzorce jsou standardní a potřebné k tomu, aby šel návrh IIR filtru skutečně provést.

### Metoda invariantní impulsní odezvy (impulse invariance)
Impulsní odezva číslicového filtru jsou **vzorky** impulsní odezvy analogového filtru: $h[n] = t_s\,h_a(nt_s)$. Pól analogového filtru $s = p$ se zobrazí na pól $z = e^{pt_s}$; např.
$$\frac{1}{s - p} \;\longrightarrow\; \frac{t_s}{1 - e^{pt_s}z^{-1}}$$
- zachovává tvar impulsní odezvy a stabilitu (Re $p < 0$ ⇒ $|z| < 1$),
- **vzorkováním vzniká aliasing** frekvenční charakteristiky → použitelné jen pro filtry, jejichž charakteristika na vysokých frekvencích klesá (DP, PP), **ne pro HP a PZ**.

### Bilineární transformace
$$s = \frac{2}{t_s}\cdot\frac{z - 1}{z + 1} = \frac{2}{t_s}\cdot\frac{1 - z^{-1}}{1 + z^{-1}}$$
- celou osu $j\omega$ zobrazí **právě jednou** na jednotkovou kružnici → **žádný aliasing**, lze ji použít pro všechny typy filtrů,
- stabilní analogový filtr (levá polorovina) → stabilní číslicový (vnitřek kružnice),
- frekvenční osa se ale **nelineárně deformuje (warping)**:
$$\omega = \frac{2}{t_s}\tan\frac{\Omega}{2}$$
→ kritické frekvence je nutné **předem zkreslit (prewarping)**: z požadované číslicové frekvence $\Omega_d$ spočítáme analogovou $\omega_a = \frac{2}{t_s}\tan\frac{\Omega_d}{2}$, navrhneme analogový filtr na $\omega_a$ a pak transformujeme.

### Řád Butterworthova filtru
$$n \ge \frac{\log\left[\left(10^{0{,}1A_s} - 1\right)/\left(10^{0{,}1A_p} - 1\right)\right]}{2\log(\omega_s/\omega_p)}$$
(frekvence $\omega_p$, $\omega_s$ po předzkreslení).

#### Ilustrační příklad – číslicová Butterworthova DP 1. řádu
*$f_c = 1$ kHz, $S = 8$ kHz, bilineární transformace ($t_s = 1$).*
- $\Omega_d = 2\pi\cdot1/8 = \pi/4$; předzkreslení $\omega_a = 2\tan(\pi/8) = 0{,}8284$,
- prototyp $H_P(s) = \frac{1}{s+1}$ → $H(s) = \frac{\omega_a}{s + \omega_a}$,
- dosazení $s = 2\frac{z-1}{z+1}$:
$$H(z) = \frac{\omega_a(z + 1)}{(2 + \omega_a)z - (2 - \omega_a)} = \frac{0{,}2929(1 + z^{-1})}{1 - 0{,}4142z^{-1}}$$
- kontrola: $H(1) = 1$ (stejnosměrný zisk), $|H(e^{j\pi/4})| = 0{,}707$ (−3 dB přesně na 1 kHz díky předzkreslení), nula v $z = -1$ ($H = 0$ na $S/2$).

### Číslicové spektrální transformace (D2D)
Z číslicové dolní propusti lze získat HP, PP, PZ záměnou $z^{-1}$ za vhodnou all-pass funkci (Konstantinidesovy transformace), např. DP→HP: $z^{-1} \to -\dfrac{z^{-1} + \alpha}{1 + \alpha z^{-1}}$. Nejjednodušší případ: $H_{HP}(z) = H_{LP}(-z)$ odpovídá posunu spektra o 0,5 (kap. 7, 8).

---

## 9.6 Adaptivní filtrace

> **Adaptivní filtry** jsou systémy, které **přizpůsobují své chování (koeficienty) parametrům vstupního signálu**. Protože koeficienty závisejí na signálu, jsou to systémy **nelineární a časově proměnné**.

**Obecná struktura:**

```
x(k) ──► [ Adaptivní filtr ] ── y(k) ──►(−)
               ▲                          (+)◄── d(k)  (požadovaný signál)
               │                           │
        [ Adaptivní algoritmus ] ◄── e(k) ─┘    e(k) = d(k) − y(k)
```

Filtr zpracovává vstup $x(k)$; výstup $y(k)$ se porovná s **požadovaným signálem $d(k)$**; **chyba $e(k) = d(k) - y(k)$** řídí adaptivní algoritmus, který upravuje koeficienty tak, aby se chyba minimalizovala.

### Specifikace adaptivního systému (3 kroky)
1. **volba typu aplikace** – volba signálů získaných z prostředí, tj. **vstupního a požadovaného signálu**,
2. **volba struktury adaptivního filtru:**
   - **adaptivní FIR** – nejčastěji **transverzální struktura** (přímá forma; vždy stabilní, unimodální chybová plocha),
   - **adaptivní IIR** – nejčastěji kanonická přímá forma,
3. **volba adaptivního algoritmu:**
   - definice **kriteriální funkce $F$** (funkce vstupu, požadovaného výstupu – typicky střední kvadratická chyba $E[e^2(k)]$),
   - definice **minimalizačního algoritmu** pro $F$.

### Typické aplikace

1. **Identifikace systému** – máme neznámý systém („černou skříňku“) a hledáme jeho charakteristiku (impulsní odezvu). Stejný vstup $x(k)$ jde do neznámého systému (jeho výstup = $d(k)$) i do adaptivního filtru; po konvergenci adaptivní filtr **modeluje neznámý systém**.
2. **Ekvalizace kanálu** – použití v komunikacích (např. spojení dvou modemů). Filtr **eliminuje charakteristiku komunikační linky**, která zkresluje přenášený signál. Černá skříňka představuje neznámý kanál, adaptivní filtr je zapojen **za ním** a učí se jeho **inverzi**; požadovaný signál je vstup zpožděný linkou $z^{-M}$, která kompenzuje zpoždění sériového spojení kanálu a filtru.
3. **Predikce signálu** – používá se při **kompresi řečového signálu** (lineární predikce, LPC). Filtr dostává zpožděný signál $x(k - M)$ a snaží se odhadnout současný vzorek $d(k) = x(k)$; přenáší se jen koeficienty a chyba predikce.
4. **Potlačení šumu (noise cancellation)** – potlačení okolního rušení a zvýraznění užitečného signálu (např. telefonování za jízdy autem):
   - $x(k)$ – řečový signál, $v(k)$ – okolní rušení (snímané dalším, vhodně umístěným **referenčním mikrofonem**),
   - primární signál $d(k)$ obsahuje řeč + rušení; adaptivní filtr z reference vytvoří odhad rušení $y(k)$,
   - **$e(k) = d(k) - y(k)$ je vyčištěný signál** (řeč s reference nekoreluje, proto ji filtr „neodečte“).

### Adaptivní FIR transverzální filtr

$$y(k) = \sum_{i=0}^{m}w_i(k)\,x(k - i) = \mathbf{w}^T(k)\,\mathbf{u}(k), \qquad k \ge 0$$
$$\mathbf{u}(k) = [x(k), x(k-1), \dots, x(k-m)]^T, \qquad \mathbf{w}(k) = [w_0(k), w_1(k), \dots, w_m(k)]^T$$

(řada zpožďovacích členů $z^{-1}$, jejichž výstupy se násobí **časově proměnnými** váhami $w_i(k)$ a sčítají).

**Adaptační algoritmy:**
- **LMS** (Least Mean Square),
- **modifikovaný LMS** (např. normalizovaný NLMS),
- **RLS** (Recursive Least Squares).

> **Doplnění – algoritmus LMS** (nad rámec slidů): váhy se posouvají proti gradientu okamžité kvadratické chyby:
> $$e(k) = d(k) - \mathbf{w}^T(k)\mathbf{u}(k), \qquad \mathbf{w}(k+1) = \mathbf{w}(k) + 2\mu\,e(k)\,\mathbf{u}(k)$$
> $\mu$ je krok adaptace (malé $\mu$ = pomalá, ale přesná konvergence; velké $\mu$ = rychlá, ale hrozí nestabilita). **NLMS** normuje krok výkonem vstupu: $\mu \to \mu/(\varepsilon + \|\mathbf{u}(k)\|^2)$. **RLS** konverguje mnohem rychleji, ale je výpočetně náročnější ($\sim m^2$ operací na vzorek místo $\sim m$).

#### Úloha k procvičení (MATLAB – „Decoding a Mystery Message“)
Zpráva je při přenosu kontaminována **nízkofrekvenčním signálem** a **vysokofrekvenčním šumem**; dekódovat ji lze jen zobrazením v čase.
a) Zobrazte signál a jeho DFT – určete pásmo zprávy.
b) Bilineární transformací navrhněte **IIR pásmovou propust 2. řádu**, signál filtrujte příkazy `filter` (běžná filtrace) i `filtfilt` (filtrace s **nulovou fází** – signál se filtruje dopředu i pozpátku, fázové zkreslení se vyruší, modul se umocní na druhou).
c) Alternativně vynulujte v DFT složku nízkofrekvenční kontaminace, proveďte IDFT a vysokofrekvenční šum odstraňte IIR dolní propustí navrženou **metodou invariantní impulsní odezvy**.
d) Porovnejte metody i obě filtrační funkce.

---

## 9.7 Realizační struktury filtrů

### FIR filtry

**1. Přímá forma (direct form, transverzální struktura)** – vychází přímo z výstupní rovnice:
$$y(n) = \sum_{m=0}^{N-1}h(m)\,x(n - m)$$

```
x[k] ─┬─[T]─┬─[T]─┬─ … ─[T]─┐
    h[0]   h[1]  h[2]      h[N−1]
      └──►(Σ)──►(Σ)── … ──►(Σ)──► y[k]
```

Pro filtr $N$-tého řádu (délky $N$) je potřeba:
- $N - 1$ posuvných registrů (zpožďovacích členů),
- $N - 1$ paměťových buněk pro vstupní vzorky,
- $N$ buněk pro koeficienty,
- **$N$ násobiček**,
- $N - 1$ sčítaček.

Snadná realizace v DSP procesorech (operace **MAC – multiply-accumulate**), jedna z nejpoužívanějších struktur, používá se i v adaptivních FIR filtrech.

**2. Transponovaná architektura** – variace přímé formy s podobnými vlastnostmi: vstup se rozvádí ke všem násobičkám současně ($h[N-1], \dots, h[0]$) a zpožďovací členy jsou zařazeny mezi sčítačky (ukládají částečné součty).

**3. Symetrická FIR architektura** – pro FIR filtry **s lineární fází** (symetrické koeficienty): vzorky se stejným koeficientem se **nejprve sečtou a pak jednou vynásobí**, $h[k](x[n-k] + x[n-N+1+k])$ → **poloviční počet násobiček** → rychlejší zpracování.
Struktura **není vhodná pro moderní DSP procesory** – ty jsou optimalizovány na operace MAC (multiply-accumulate), ne na posloupnost add-multiply-add, kterou symetrická struktura vyžaduje.

### IIR filtry

**Přímá forma I a přímá forma II** (kap. 4 a 6). **Přímá forma II je realizačně jednodušší** – pro filtr řádu $N$ potřebuje jen **$N$ posuvných registrů** (přímá forma I $2N$).

**Kaskádní architektura** – výsledný filtr se implementuje jako **kaskáda sekcí 2. řádu** (případně jedné sekce 1. řádu), každá popsaná např. stavovým modelem $[A_i, b_i, c_i, d_i]$:
$$H(z) = \frac{N(z)}{D(z)} = G\prod_{i=1}^{Q}H_i(z), \qquad \sum_{i=1}^{Q}\mathrm{order}(H_i(z)) = N$$

**Sekce 1. řádu:**
$$H_i(z) = \frac{q_{i0} + q_{i1}z^{-1}}{1 - p_iz^{-1}} \;(\text{bikvadratický tvar}), \qquad H_i(z) = q_{i0} + \frac{r_iz^{-1}}{1 - p_iz^{-1}},\; r_i = q_{i1} + p_iq_{i0} \;(\text{přímá forma II})$$

**Sekce 2. řádu (bikvad):**
$$H_i(z) = \frac{w_{i0} + w_{i1}z^{-1} + w_{i2}z^{-2}}{1 - v_{i1}z^{-1} - v_{i2}z^{-2}}, \qquad (z - p_i)(z - p_i^*) = z^2 - v_{i1}z - v_{i2}$$
nebo ve tvaru
$$H_i(z) = d_{i0} + \frac{r_{i1}z^{-1} + r_{i2}z^{-2}}{1 - v_{i1}z^{-1} - v_{i2}z^{-2}}, \quad d_{i0} = w_{i0},\; r_{i1} = w_{i1} + v_{i1}w_{i0},\; r_{i2} = w_{i2} + v_{i2}w_{i0}$$

Jmenovatel sekce 2. řádu odpovídá **dvojici komplexně sdružených pólů** – koeficienty jsou reálné.

### SOS matice (second-order sections)

$$H_i(z) = \frac{B_i(z)}{A_i(z)} = \frac{b_{0i} + b_{1i}z^{-1} + b_{2i}z^{-2}}{1 + a_{1i}z^{-1} + a_{2i}z^{-2}}, \quad i \in [1, L], \qquad \mathrm{SOS} = \begin{bmatrix}b_{01} & b_{11} & b_{21} & 1 & a_{11} & a_{21}\\ b_{02} & b_{12} & b_{22} & 1 & a_{12} & a_{22}\\ \vdots & & & & & \vdots\\ b_{0L} & b_{1L} & b_{2L} & 1 & a_{1L} & a_{2L}\end{bmatrix}$$

**Výpočet SOS matice:**
1. výpočet **nul a pólů**,
2. **seskupení nul a pólů do komplexně sdružených dvojic** na základě **blízkosti** (každý pár pólů se spáruje s nejbližším párem nul – minimalizuje to zesílení jednotlivých sekcí a citlivost) – v MATLABu `[SOS, G] = tf2sos(N, D)`,
3. **implementace subfiltrů 2. řádu.**

#### Řešený příklad
$$H(z) = \frac{N(z)}{D(z)} = \frac{1 - 0{,}5z^{-1} - 0{,}315z^{-2} - 0{,}0185z^{-3}}{1 - 0{,}5z^{-1} + 0{,}5z^{-2} - 0{,}25z^{-3}}$$

```matlab
N = [1 -.5 -.315 -.0185]; D = [1 -.5 .5 -.25];
[SOS, G] = tf2sos(N, D)
SOS = 1.0000  -0.8813  0.0000   1.0000  -0.5000  0.0000
      1.0000   0.3813  0.0210   1.0000   0.0000  0.5000
G = 1
```

$$H(z) = G\,H_1(z)\,H_2(z) = \frac{1 - 0{,}8813z^{-1}}{1 - 0{,}5z^{-1}}\cdot\frac{1 + 0{,}3813z^{-1} + 0{,}021z^{-2}}{1 + 0{,}5z^{-2}}$$

*Kontrola:* $(1 - 0{,}8813z^{-1})(1 + 0{,}3813z^{-1} + 0{,}021z^{-2}) = 1 - 0{,}5z^{-1} - 0{,}315z^{-2} - 0{,}0185z^{-3}$ ✓; $(1 - 0{,}5z^{-1})(1 + 0{,}5z^{-2}) = 1 - 0{,}5z^{-1} + 0{,}5z^{-2} - 0{,}25z^{-3}$ ✓. (Na slidu je ve vzorci překlep $-0{,}185z^{-3}$; správná hodnota $-0{,}0185$ je v řádku MATLABu.) Sekce 1 má reálný pól 0,5, sekce 2 komplexně sdružené póly $\pm j0{,}707$.

---

## Shrnutí kapitoly 9

- IIR: nízký řád, malé zpoždění × stabilita, nelineární fáze, citlivost na kvantizaci.
- Návrh z analogového prototypu (DP, $\omega_C = 1$ rad/s): analogová transformace + mapování, mapování + D2D, nebo **bilineární transformace (A2D) s předzkreslením**.
- Aproximace: **Butterworth** (max. plochá, nejvyšší řád), **Čebyšev I** (zvlnění v propustném), **Čebyšev II** (zvlnění v nepropustném), **eliptická** (zvlnění v obou, nejnižší řád).
- Invariantní impulsní odezva → aliasing (jen DP/PP); bilineární transformace → bez aliasingu, warping $\omega = \frac{2}{t_s}\tan\frac{\Omega}{2}$.
- **Adaptivní filtry:** $e = d - y$ řídí adaptaci (LMS, NLMS, RLS); aplikace: identifikace systému, ekvalizace kanálu, predikce, potlačení šumu.
- **Struktury:** FIR přímá (transverzální, $N$ násobiček), transponovaná, symetrická (½ násobiček, nevhodná pro MAC); IIR přímá forma I/II, **kaskáda sekcí 2. řádu (SOS)**.

### Kontrolní otázky
1. Porovnejte Butterworthovu, Čebyševovu a eliptickou aproximaci.
2. Proč nelze metodou invariantní impulsní odezvy navrhnout horní propust?
3. Co je warping u bilineární transformace a jak se kompenzuje?
4. Navrhněte bilineární transformací číslicovou DP 1. řádu s $f_c = 2$ kHz při $S = 8$ kHz. *(Řešení: $\omega_a = 2\tan(\pi/4) = 2$, $H(z) = \frac{2(z+1)}{4z} = 0{,}5(1 + z^{-1})$.)*
5. Nakreslete zapojení adaptivního filtru pro potlačení šumu a vysvětlete, proč $e(k)$ je vyčištěný signál.
6. Kolik násobiček potřebuje FIR filtr délky 31 v přímé a v symetrické struktuře? *(31 a 16.)*
7. Proč se IIR filtry vysokých řádů realizují jako kaskáda sekcí 2. řádu?

---

# 10. Časově-frekvenční analýza signálu

Fourierova transformace (kap. 5) říká, **které** frekvence signál obsahuje, ale ne **kdy** se vyskytují – je počítána přes celý signál. U **nestacionárních** signálů (řeč, hudba, EEG, EKG, signály s přechodovými jevy) se frekvenční obsah v čase mění. Časově-frekvenční metody popisují signál **současně v čase i ve frekvenci**:

- **krátkodobá Fourierova transformace (STFT)**,
- **waveletová (vlnková) transformace**,
- **matching pursuit**.

---

## 10.1 Krátkodobá Fourierova transformace (STFT – Short-Time Fourier Transform)

**Princip:** abychom zjistili, jak se mění frekvenční obsah signálu v čase, **rozdělíme signál na bloky a pro každý blok určíme spektrum** (DFT/FFT).

- jednotlivé bloky se **mohou překrývat** (bez překryvu, překryv R/4, R/2…; parametr $L$ = počet vzorků mezi začátky sousedních bloků),
- každý blok se **násobí oknem** (kvůli vyhlazení – omezení prosakování, kap. 5),
- **velikost bloku určuje časové a frekvenční rozlišení**:
  - **delší blok → dobré frekvenční, ale špatné časové rozlišení**,
  - **kratší blok → dobré časové, ale špatné frekvenční rozlišení**,
- STFT je funkcí **dvou proměnných – času a frekvence**; její zobrazení (kvadrát modulu, typicky v dB, barevně) se nazývá **spektrogram**.

**Použití:** analýza **řečového signálu** (na spektrogramu řeči jsou vidět harmonické základního tónu hlasivek a formanty), hudby, vibrací, biosignálů.

> **Doplnění – definice** (diskrétní tvar): pro okno $w[n]$ délky $R$ a posun $L$
> $$X[m, k] = \sum_{n=0}^{R-1}x[n + mL]\,w[n]\,e^{-j2\pi nk/R}$$
> ($m$ – index bloku = čas, $k$ – index frekvence). Spektrogram je $|X[m,k]|^2$.
>
> **Princip neurčitosti:** časové rozlišení $\Delta t$ a frekvenční rozlišení $\Delta f$ nelze současně libovolně zlepšovat – jejich součin je zdola omezen ($\Delta t\cdot\Delta f \ge$ konst.; minimum dosahuje Gaussovo okno). Frekvenční rozlišení bloku délky $R$ je $\Delta f \approx S/R$ (kap. 5), časové $\approx R/S$. **STFT má stejné rozlišení na všech frekvencích** – to je její hlavní omezení.

#### Řešený příklad – přiřazení spektrogramů
*Čtyři signály s proměnnou frekvencí (chirpy) a čtyři spektrogramy A–D: A – frekvence lineárně roste (≈ 0,03 → 0,25), B – klesá a pak roste („V“), C – lineárně klesá (0,25 → 0,03), D – roste a pak klesá („Λ“).*

| Signál | Průběh (hustota kmitů) | Spektrogram |
|---|---|---|
| 1 | řídký na začátku, nejhustší uprostřed, řídký na konci | **D** |
| 2 | hustý na začátku, stále řidší | **C** |
| 3 | hustý na začátku, řídký uprostřed, hustý na konci | **B** |
| 4 | řídký na začátku, stále hustší | **A** |

(Okamžitou číslicovou frekvenci odhadneme z periody: perioda 5 vzorků ↔ $F = 0{,}2$, perioda 10 vzorků ↔ $F = 0{,}1$.)

---

## 10.2 Waveletová (vlnková) transformace

Místo nekonečných sinusovek (Fourier) nebo sinusovek v pevném okně (STFT) se signál porovnává s **krátkými vlnkami (wavelety)** $\psi(t)$, které se **posouvají** (parametr polohy $b$) a **roztahují/smršťují** (parametr měřítka $a$).

**Příklady waveletů:**
- (a) **Gaussova vlna** – 1. derivace Gaussovy funkce,
- (b) **mexický klobouk** (Mexican hat) – 2. derivace Gaussovy funkce,
- (c) **Haarův wavelet** – obdélníkový (+1, −1),
- (d) **Morletův wavelet** – kosinusovka v Gaussově obálce (reálná část).

Wavelet musí mít **nulovou střední hodnotu** (kmitá – „vlnka“) a **rychle odeznít** (je lokalizovaný v čase).

### Změna pozice a měřítka
- **změna pozice (posun, $b$)** – wavelet „prochází“ signálem a analyzuje ho v různých časových okamžicích,
- **změna měřítka ($a$)** – **roztažený** wavelet (velké $a$) je pomalý → zachycuje **nízké frekvence**; **smrštěný** wavelet (malé $a$) je rychlý → zachycuje **vysoké frekvence**.

**Vliv měřítka na energetické spektrum waveletu:** při velkém měřítku je spektrum waveletu **úzké a posunuté k nízkým frekvencím**, při malém měřítku **široké a posunuté k vysokým frekvencím**. Wavelet je tedy pásmová propust s **konstantní relativní šířkou pásma** ($\Delta f/f$ = konst.).

### Korelace waveletu a vstupního signálu

Hodnota transformace pro dané $(a, b)$ je **míra podobnosti (korelace) signálu s posunutým a roztaženým waveletem**:

> **Doplnění – spojitá waveletová transformace (CWT):**
> $$T(a, b) = \frac{1}{\sqrt a}\int_{-\infty}^{\infty}x(t)\,\psi^*\!\left(\frac{t - b}{a}\right)dt$$

Součin signálu a waveletu přispívá kladně tam, kde mají stejné znaménko, a záporně, kde mají opačné. Je-li signál v okolí $b$ „podobný“ waveletu (na měřítku $a$), je $|T(a,b)|$ velké. Protože wavelet má nulovou střední hodnotu, **lokální střední hodnota (pomalý trend) signálu se do výsledku nepromítne**.

### Příklady CWT (skalogram)

CWT se zobrazuje jako obraz v rovině ($b$ – poloha/čas na vodorovné ose, $a$ – měřítko na svislé ose; malá měřítka nahoře):

- **signál se zrychlující se oscilací** (chirp): s rostoucí frekvencí se maxima CWT posouvají k **menším měřítkům**,
- **obdélníkový signál (skok)**: tam, kde je signál konstantní, je CWT nulová; **nespojitost** se projeví v okolí skoku, šířka oblasti je úměrná měřítku $a$ (wavelety s $a = 1, 2, 6, 16$) → v rovině $(a, b)$ vzniká **„kužel vlivu“**, který se k malým měřítkům zužuje přímo k místu nespojitosti → **CWT umí přesně lokalizovat singularity (hrany, špičky)**,
- **signál s ostrým skokem a exponenciálním doběhem**: CWT ukazuje výrazný kužel s vrcholem v místě skoku.

**Kompozice signálů a CWT:** součet pomalé sinusovky (a), rychlejší sinusovky (b) a krátkého šumového záblesku (c) dává signál (d), v němž jednotlivé složky „splývají“. V CWT (e) jsou ale **odděleny**: pomalá sinusovka na velkém měřítku $a_2$, rychlá na menším měřítku $a_1$ a šumový záblesk na nejmenších měřítcích, lokalizovaný v čase.

### Odstranění šumu pomocí CWT
Koeficienty CWT v oblasti odpovídající šumu (malá měřítka) nebo nežádoucí složce **vynulujeme** a provedeme **inverzní waveletovou transformaci** → zbude očištěný signál (např. jen pomalá sinusovka). Na rozdíl od běžné filtrace lze zasáhnout **jen v určité časové oblasti** – waveletová analýza je lokální v čase i měřítku.

### Srovnání STFT a waveletové transformace

| | STFT | Wavelety |
|---|---|---|
| analyzující funkce | sinusovky v pevném okně | posouvané a škálované vlnky |
| časově-frekvenční rozlišení | **stejné** na všech frekvencích | **proměnné**: na vysokých frekvencích dobré časové, na nízkých dobré frekvenční rozlišení |
| vhodné pro | kvazistacionární signály (řeč) | přechodové jevy, singularity, signály s více měřítky |

---

## 10.3 Diskrétní waveletová transformace (DWT) – víceúrovňová dekompozice

Měřítka i posuny se volí **dyadicky** (mocniny dvou). Signál $S_{0,n}$ ($n = 0, 1, \dots, N-1$) se na každé úrovni $m$ rozloží na:
- **aproximační koeficienty $S_{m,n}$** (hrubá, nízkofrekvenční část),
- **detailní koeficienty $T_{m,n}$** (jemné detaily, vysokofrekvenční část).

$$S_{0,n} \to S_{1,n} + T_{1,n} \to S_{2,n} + T_{2,n} + T_{1,n} \to \dots \to S_{M,0} + T_{M,0} + \dots + T_{1,n}$$

Na každé úrovni se rozkládá **jen aproximace** z předchozí úrovně; počet koeficientů se v každé úrovni **půlí** (úroveň 1: $T_{1,n}$ má $N/2$ koeficientů, úroveň 2: $N/4$, …). Při **plné dekompozici** ($N = 2^M$) je výsledný **vektor waveletové transformace** $[S_{M}, T_{M}, \dots, T_{3}, T_{2}, T_{1}]$ stejně dlouhý jako původní signál (pro $m = 5$: bloky délky 1, 1, 2, 4, 8, 16 pro $N = 32$).

### Rychlá waveletová transformace (FWT) pomocí filtrů

DWT se počítá pomocí **banky filtrů** (Mallatův algoritmus):

**Dekompozice (analýza):**
```
S(m,n) ──┬──► [LP] ──► (↓2) ──► S(m+1,n) ──┬──► [LP] ──► (↓2) ──► S(m+2,n)
         └──► [HP] ──► (↓2) ──► T(m+1,n)   └──► [HP] ──► (↓2) ──► T(m+2,n)
```
Dolní propust (LP) vytváří aproximaci, horní propust (HP) detaily; **decimace (↓2)** – ponechá se každý druhý vzorek (polovina pásma → stačí poloviční vzorkovací frekvence, kap. 3).

**Rekonstrukce (syntéza):**
```
S(m+2,n) ──► (↑2) ──► [LP'] ──┐
                              (+)──► S(m+1,n) ──► (↑2) ──► [LP'] ──┐
T(m+2,n) ──► (↑2) ──► [HP'] ──┘                                   (+)──► S(m,n)
                                          T(m+1,n) ──► (↑2) ──► [HP'] ──┘
```
**Interpolace nulami (↑2)**, rekonstrukční filtry LP', HP' a součet. Filtry jsou navrženy tak, aby rekonstrukce byla **dokonalá**. Výpočetní složitost je úměrná $N$ (rychlejší než FFT).

**Použití DWT:** komprese (JPEG 2000), odšumování (prahování detailních koeficientů), detekce singularit a událostí (QRS v EKG), extrakce příznaků.

---

## 10.4 Matching pursuit (MP)

- **adaptivní metoda dekompozice signálu**,
- signál se rozkládá na jednotlivé **atomy**, které se vyhledávají ve **slovníku funkcí** – často se používá slovník tzv. **Gaborových funkcí** (atomů):
$$g_{(s,u,v,w)}(t) = g\!\left(\frac{t - u}{s}\right)\cos(vt + w)$$
kde $g$ je **Gaussova obálka**; parametry: $s$ – **měřítko** (šířka), $u$ – **posun** (poloha v čase), $v$ – **frekvence**, $w$ – **fáze**. Slovník obsahuje atomy všech možných kombinací parametrů – od „čistých“ obálek přes sinusovky až po krátké vlnové balíčky (je silně **redundantní**).

### Princip algoritmu
Nechť $\gamma = (s, u, v, w)$ a $f$ je analyzovaný signál.

1. Zvolíme atom $g_{\gamma_0}$ ze slovníku $D$ tak, aby **maximalizoval skalární součin** $|\langle f, g_{\gamma_0}\rangle|$ (atom nejvíce podobný signálu).
2. Signál zapíšeme jako
$$f = \langle f, g_{\gamma_0}\rangle\,g_{\gamma_0} + Rf,$$
kde $Rf$ je **reziduum** (zbytek po odečtení nalezeného atomu).
3. Bod 1 se opakuje na reziduu $Rf$.
4. Po $M$ iteracích dostaneme
$$f = \sum_{k=0}^{M}\langle R^kf, g_{\gamma_k}\rangle\,g_{\gamma_k} + R^Mf,$$
kde $R^kf$ je reziduum po $k$ krocích ($R^0f = f$). **Proces se ukončí, když energie rezidua $\|R^Mf\|^2$ klesne pod předem zvolený práh.**

**Výsledek:** signál je popsán několika atomy s konkrétními parametry (kdy, jak dlouho, na jaké frekvenci, s jakou amplitudou). Časově-frekvenční mapu získáme součtem (Wignerových) rozložení jednotlivých atomů – každý atom je v mapě zobrazen jako „skvrna“ dané polohy, délky a frekvence. Výhoda: **rozlišení se přizpůsobuje signálu** (dlouhé atomy pro stacionární části, krátké pro přechodové jevy).

*Příklad ze slidu:* vstupní signál se dvěma vlnovými balíčky je rozložen na několik Gaborových atomů; v časově-frekvenční mapě je každý atom jedna elipsa (např. vysokofrekvenční balíček uprostřed signálu).

### Aplikace MP: single-trial analýza EEG a detekce ERP komponenty
Evokované potenciály (ERP) jsou v EEG obvykle „utopené“ v pozadí a klasicky se získávají **průměrováním mnoha epoch** (koherentní průměrování, kap. 3). MP umožňuje analýzu **jednotlivých epoch (single-trial)**: v časově-frekvenční mapě odezvy na **cílový podnět (target)** se objeví atom odpovídající komponentě **P300** (kladná vlna cca 300 ms po podnětu), který u **necílového podnětu (non-target)** chybí. Výsledky lze porovnat pro 1 epochu a průměr 15 epoch.

---

## Shrnutí kapitoly 10

- Fourierova transformace neříká, **kdy** se frekvence vyskytují → časově-frekvenční analýza pro nestacionární signály.
- **STFT:** bloky (s překryvem) × okno × FFT → **spektrogram**; delší blok = lepší frekvenční, horší časové rozlišení; rozlišení je pevné pro všechny frekvence.
- **Wavelety** (Gaussova vlna, mexický klobouk, Haar, Morlet): posun $b$ a měřítko $a$; malé $a$ = vysoké frekvence; CWT = korelace se škálovanými vlnkami; výborná lokalizace singularit, odšumování.
- **DWT:** dyadická víceúrovňová dekompozice na aproximace $S$ a detaily $T$; **FWT** banka filtrů LP/HP + decimace 2; rekonstrukce interpolací a filtry LP'/HP'.
- **Matching pursuit:** iterativní výběr Gaborových atomů maximalizujících $|\langle R^kf, g_\gamma\rangle|$, dokud energie rezidua neklesne pod práh; adaptivní rozlišení; single-trial analýza ERP (P300).

### Kontrolní otázky
1. Jak ovlivní délka okna STFT časové a frekvenční rozlišení spektrogramu?
2. Proč wavelety lépe lokalizují krátké přechodové jevy než STFT?
3. Jak se v CWT projeví skoková změna signálu?
4. Popište jeden krok DWT pomocí banky filtrů. Proč lze po filtraci decimovat dvěma?
5. Vysvětlete algoritmus matching pursuit. Kdy se iterace ukončí?
6. K čemu je výhodná single-trial analýza EEG?

---

# Příloha A – Přehled nejdůležitějších vzorců

## Signály, vzorkování, kvantizace (kap. 2–3)

| Pojem | Vzorec |
|---|---|
| sinusoida | $A\sin(\Omega t - \Theta)$, $\Omega = 2\pi F = 2\pi/T$, $\Theta = 2\pi t_\Theta/T$ |
| Euler | $A\cos(\Omega t + \Theta) = \frac A2e^{j(\Omega t + \Theta)} + \frac A2e^{-j(\Omega t + \Theta)}$ |
| perioda součtu | $T = \mathrm{NSN}(T_i)$, $f_0 = \mathrm{NSD}(f_i)$ |
| ideální vzorkování | $x_I(t) = \sum x[n]\delta(t - nt_s)$, $\;X_I(f) = S\sum_k X(f - kS)$ |
| vzorkovací věta | $f_s > 2f_{max}$ |
| aliasing | $f_{alias} = f - MS$, $\;-0{,}5S < f_{alias} < 0{,}5S$ |
| Butterworth | $\lvert H(f)\rvert = 1/\sqrt{1 + (f/f_c)^{2n}}$ |
| úroveň aliasingu | $\dfrac{\lvert H(f_s - f_a)\rvert}{\lvert H(f_a)\rvert}$ |
| pásmové vzorkování | $\dfrac{2f_H}{k} \le S \le \dfrac{2f_L}{k-1}$, $k = 1, \dots, \mathrm{int}(f_H/B)$ |
| kvantizace | $L = 2^m$, $\Delta = \dfrac{x_{max} - x_{min}}{L}$, $i = \mathrm{round}\dfrac{x - x_{min}}{\Delta}$, $x_q = x_{min} + i\Delta$ |
| kvantizační šum | $E(e_q^2) = \Delta^2/12$ |
| SNR | $10{,}79 + 20\log\dfrac{x_{rms}}{\Delta}$; sinusovka: $1{,}76 + 6{,}02m$ dB |
| ideální rekonstrukce | $x(t) = \sum x[n]\,\mathrm{sinc}\!\left(\dfrac{t - nt_s}{t_s}\right)$ |
| energie, výkon | $E = \sum\lvert x[n]\rvert^2$, $\;P = \frac1N\sum_{0}^{N-1}\lvert x[n]\rvert^2$ |
| číslicová frekvence | $F = f/S$, $\Omega = 2\pi F$; periodická sinusoida ⇔ $F = k/N$ racionální |
| sudá/lichá složka | $x_{e,o}[n] = \tfrac12(x[n] \pm x[-n])$ |
| rozptyl | $\sigma^2 = E(x^2) - m^2$; rovnoměrné: $(\beta - \alpha)^2/12$ |
| Q-funkce | $Q(x) = 0{,}5 - 0{,}5\,\mathrm{erf}(x/\sqrt2)$ |
| koherentní průměrování | zisk $10\log M$ dB |

## Systémy, konvoluce, korelace (kap. 4)

| Pojem | Vzorec |
|---|---|
| konvoluce | $y[n] = \sum_k x[k]h[n-k]$, $L_y = L_x + L_h - 1$, $\sum y = \sum x\cdot\sum h$ |
| kaskáda / paralela | $h = h_1 * h_2$ / $h = h_1 + h_2$ |
| korelace | $r_{xh}[n] = \sum_k x[k]h[k-n] = x[n] * h[-n]$ |
| autokorelace | $r_{xx}[n] = r_{xx}[-n]$, $\;r_{xx}[n] \le r_{xx}[0] = E$ |
| homogenní řešení | kořeny $z^N + A_1z^{N-1} + \dots + A_N = 0$ → $y_H = \sum K_iz_i^n$ |
| impulsní odezva IIR | homogenní řešení s $h[0] = 1$; obecně $h = \sum B_kh_0[n-k]$ |
| dekonvoluce | $h[n] = \frac{1}{x[0]}\left[y[n] - \sum_{k=0}^{n-1}h[k]x[n-k]\right]$ |

## Fourierova transformace (kap. 5)

| Pojem | Vzorec |
|---|---|
| DTFT | $X(F) = \sum x[k]e^{-j2\pi kF}$, $\;x[n] = \int_{-1/2}^{1/2}X(F)e^{j2\pi nF}dF$ |
| frekvenční odezva | $H(F) = \dfrac{\sum B_ke^{-j2\pi kF}}{1 + \sum A_ke^{-j2\pi kF}}$ |
| ustálená odezva | $A\cos(2\pi nF_0 + \theta) \to A\lvert H(F_0)\rvert\cos(2\pi nF_0 + \theta + \phi(F_0))$ |
| DFT / IDFT | $X[k] = \sum_{n=0}^{N-1}x[n]e^{-j2\pi nk/N}$, $\;x[n] = \frac1N\sum_{k=0}^{N-1}X[k]e^{j2\pi nk/N}$ |
| rozlišení DFT | $\Delta f = S/N$, $f_k = kS/N$ |
| reálný signál | $X[k] = X^*[N-k]$ |
| sinusovka v DFT | amplituda $A$ → čáry $AN/2$ v $k_0$ a $N - k_0$ |
| prosakování | nenastane ⇔ $k_F = F_0N$ celé |
| Parseval (DFT) | $\sum\lvert x[n]\rvert^2 = \frac1N\sum\lvert X[k]\rvert^2$ |
| rozlišení okna | $\Delta F = W_M = K/N$ (pravoúhlé $K = 2$, Hann/Hamming 4, Blackman 6) |
| periodogram | $P[k] = \frac1N\lvert X[k]\rvert^2$ |
| FFT motýlek DIT | $X[k] = X^e[k] + W_N^kX^o[k]$, $X[k + N/2] = X^e[k] - W_N^kX^o[k]$ |
| složitost | DFT $N^2$, FFT $\frac N2\log_2N$ násobení |

**DTFT dvojice:** $\delta[n] \to 1$; $\;\alpha^nu[n] \to \frac{1}{1 - \alpha e^{-j2\pi F}}$; $\;\cos(2\pi nF_0) \to 0{,}5[\delta(F + F_0) + \delta(F - F_0)]$; $\;2F_C\mathrm{sinc}(2nF_C) \to \mathrm{rect}(F/2F_C)$.

## Z-transformace (kap. 6)

| Pojem | Vzorec |
|---|---|
| definice | $X(z) = \sum x[k]z^{-k}$ + ROC |
| posun | $x[n-N] \to z^{-N}X(z)$ |
| konvoluce | $x * h \to X(z)H(z)$ |
| škálování, $n$ | $\alpha^nx[n] \to X(z/\alpha)$; $\;nx[n] \to -z\,dX/dz$ |
| základní dvojice | $\alpha^nu[n] \to \frac{z}{z-\alpha}$, $\lvert z\rvert > \lvert\alpha\rvert$; $\;n\alpha^nu[n] \to \frac{z\alpha}{(z-\alpha)^2}$ |
| přenosová funkce | $H(z) = \dfrac{Y(z)}{X(z)} = \dfrac{\sum B_kz^{-k}}{\sum A_kz^{-k}}$ |
| stabilita | $\sum\lvert h\rvert < \infty$; kauzální stabilní ⇔ póly uvnitř jednotkové kružnice |
| parciální zlomky | rozkládá se $X(z)/z$; $k_m = (z - p_m)\frac{X(z)}{z}\big\rvert_{z=p_m}$ |
| jednostranná – posun | $y[n-1] \to z^{-1}Y + y[-1]$; $\;y[n-2] \to z^{-2}Y + z^{-1}y[-1] + y[-2]$ |
| počáteční / koncová hodnota | $x[0] = \lim_{z\to\infty}X(z)$; $\;x[\infty] = \lim_{z\to1}(z-1)X(z)$ |

## Filtry (kap. 7–9)

| Pojem | Vzorec |
|---|---|
| geometrie | $\lvert H\rvert = K\dfrac{\prod\text{vzdáleností od nul}}{\prod\text{vzdáleností od pólů}}$ |
| skupinové zpoždění | $\tau = -\frac{1}{2\pi}\frac{d\phi}{dF}$; lineární fáze FIR: $\tau = M = \frac{N-1}{2}$ |
| typy FIR s lin. fází | typ 2: $H(0{,}5) = 0$; typ 3: $H(0) = H(0{,}5) = 0$; typ 4: $H(0) = 0$ |
| ideální DP | $h[n] = 2F_C\,\mathrm{sinc}(2nF_C)$ |
| HP | $\delta[n] - h_{LP}[n]$ ($N$ liché), nebo $(-1)^nh_{LP}[n]$ s $F_C' = 0{,}5 - F_C$ |
| PP / PZ | $2\cos(2\pi nF_0)h_{LP}[n]$; $\;\delta[n] - h_{BP}[n]$ |
| klouzavý průměr | $\lvert H\rvert = \frac1N\left\lvert\frac{\sin\pi FN}{\sin\pi F}\right\rvert$ |
| okénková metoda | $F_C = 0{,}5(F_p + F_s)$, okno podle $A_s$, $N = C/(F_s - F_p)$ |
| zvlnění | $\delta_p = \frac{10^{A_p/20} - 1}{10^{A_p/20} + 1}$, $\;\delta_s = 10^{-A_s/20}$ |
| Kaiser | $N \ge \frac{A_{s0} - 7{,}95}{14{,}36(F_s - F_p)} + 1$ |
| půlpásmový filtr | $F_C = 0{,}25$, $S = 2(f_p + f_s)$ nebo $4f_0$ |
| bilineární transformace | $s = \frac{2}{t_s}\frac{z-1}{z+1}$, $\;\omega = \frac{2}{t_s}\tan\frac{\Omega}{2}$ |
| LMS | $\mathbf w(k+1) = \mathbf w(k) + 2\mu e(k)\mathbf u(k)$ |

**Okna (útlum filtru / $C$):** pravoúhlé 21 dB / 0,92; Hann 44 dB / 3,21; Hamming 53 dB / 3,47; Blackman 75 dB / 5,71.

---

# Příloha B – Opravy a upřesnění k slidům

Při zpracování byly na slidech nalezeny tyto překlepy a nepřesnosti (ve skriptech je všude uvedena správná verze):

| Přednáška, slide | Na slidu | Správně |
|---|---|---|
| 2, slide 9 | $\sin\alpha\sin\beta = \frac{\cos(\alpha-\beta)}{2} + \frac{\cos(\alpha+\beta)}{2}$ | $\frac{\cos(\alpha-\beta)}{2} - \frac{\cos(\alpha+\beta)}{2}$ |
| 2, slide 14 | $\delta(t) = \lim\frac1\beta\Pi\left(\frac1\beta\right)$ | $\lim\frac1\beta\Pi\left(\frac t\beta\right)$ |
| 2, slide 15 | $f(t) * \delta(t_0) = f(t_0)$ | $f(t) * \delta(t - t_0) = f(t - t_0)$; $\int f(t)\delta(t - t_0)dt = f(t_0)$ |
| 2, slide 22 | Nyquistova frekvence $f_{max}/2$ | kritická vzorkovací frekvence $2f_{max}$ |
| 2, slide 46 | $E(e_q^2) = \Delta/12$ | $E(e_q^2) = \Delta^2/12$ |
| 3, slide 11 | $\alpha^n$ roste pro $\alpha > 0$, klesá pro $\alpha < 0$ | roste pro $\lvert\alpha\rvert > 1$, klesá pro $\lvert\alpha\rvert < 1$ (pro $\alpha < 0$ střídá znaménko) |
| 3, slide 29 | v exponentu Gaussovy hustoty $2\delta_x$ | $2\delta_x^2$; sudé momenty $\frac{(2k)!}{k!2^k}\sigma^{2k}$ |
| 3, slide 31 | $0{,}5\,\mathrm{erf}(\cdot) - \mathrm{erf}(\cdot)$ | $0{,}5[\mathrm{erf}(\cdot) - \mathrm{erf}(\cdot)]$ |
| 3, slide 32 | $Q(x) = 0{,}5 - 0{,}5\,\mathrm{erf}(x\sqrt2)$; $P = Q(z_2) - Q(z_1)$ | $Q(x) = 0{,}5 - 0{,}5\,\mathrm{erf}(x/\sqrt2)$; $P = Q(z_1) - Q(z_2)$ |
| 4, slide 6 | $\dots = B_0x[n + L]$, kauzální pro $K \le L$ | $\dots = B_0x[n + K]$, kauzální pro $K \le L$ |
| 4, slide 39 | mezivýsledek „−1 −2 −2 3“ a součet „… 9 9“ | „−1 −2 −2 −3“, součet „… 3 9“ (výsledek $\{2,3,5,10,3,9\}$ je správně) |
| 5, slide 27 | $X[k - k_0] \leftrightarrow e^{-j2\pi kn_0/N}x[n]$ | $X[k - k_0] \leftrightarrow x[n]\,e^{j2\pi nk_0/N}$ |
| 5, slide 37 | DFT exponenciály $\frac{1 - \alpha^n}{1 - \alpha e^{-j2\pi k/N}}$ | $\frac{1 - \alpha^N}{1 - \alpha e^{-j2\pi k/N}}$ |
| 6, slide 11 | $x[n] = \{1, 4{,}25, 1\}$ | $\{1, 0, 4{,}25, 0, 1\}$ ($\delta[n+2] + 4{,}25\delta[n] + \delta[n-2]$) |
| 6, slide 22 | stupeň čitatele musí být menší než stupeň jmenovatele | menší **nebo roven** |
| 6, slide 24 | $x(n) = \frac{1}{2\pi}\oint X(z)z^{n-1}dz$ | $\frac{1}{2\pi j}\oint X(z)z^{n-1}dz$ |
| 6, slide 24 | $x[n] = \{2, -5, 0, 3, 5, -2\}$ | $\{2, -5, 0, 5, -2\}$ |
| 6, slide 25 | $H(z) = \frac{z - 4}{1 - z - z^2}$ | dělení odpovídá $H(z) = \frac{z-4}{z^2 - z + 1}$ |
| 6, slide 31 | $y[n-N] \leftrightarrow \dots + z^{-(N-1)}y[0] + \dots$ | $\dots + z^{-(N-1)}y[-1] + \dots$ |
| 7, slide 12 | typ 2 použití: i diferenciátor a Hilbertův transformátor | symetrické filtry nemají fázi $\pi/2$ – pro ty je nutný typ 3 nebo 4 |
| 7, slide 15 | typ 3 → derivátor, typ 4 → Hilbert | v literatuře spíše typ 4 → derivátor ($H(0{,}5) \ne 0$), typ 3 → Hilbert (viz kap. 7) |
| 8, slide 10 | zadání „pásmová propust“ | jde o pásmovou zádrž |
| 8, slide 11 | „$-h[0] = h[20] = -0{,}1$“ atd. | $h[0] = -h[20] = -0{,}1$ (hodnoty v tabulce jsou $h[0] \dots h[9]$) |
| 8, slide 15 | jmenovatel Čebyševova okna $\cosh[M\cos^{-1}(\alpha)]$ | $\cosh[M\cosh^{-1}(\alpha)]$ |
| 8, slide 36 | pásmová zádrž: „filtr typu 4 (sudá symetrie, lichá délka)“ | sudá symetrie a lichá délka = **typ 1** |
| 9, slide 31 | čitatel $\dots - 0{,}185z^{-3}$ | $\dots - 0{,}0185z^{-3}$ (souhlasí s řádkem MATLABu a SOS) |
