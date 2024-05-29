# Tervetuloa Arvaa Bändi -peli sivulle!!!

## Kuvaus
Kyseessä on bändipeli, jossa aloitetaan sanomalla bändi. Seuraavan pelaajan tehtävänä on arvata bändi, joka alkaa edellisen arvauksen viimeisellä kirjaimella. Pelaajalla on kolme yritystä, ennenkuin hän tipahtaa pois. Projekti on kehitysvaiheessa ja yhtyelista ei välttämättä ole vielä erityisen kattava, mutta sitä päivitetään. Peli-idea lähti lapsuuden automatkoista, joilla kyseistä peliä pelattiin ruutuvihkolla tai ihan vaan muistin varassa. Projektin ovat luoneet Janne Juote, Daniel Pozzoli ja Daniel Väkiparta.

Tämä Python-sovellus on interaktiivinen arvauspeli, jossa pelaajan tulee arvata rockbändien nimiä. Jokaisen arvauksen tulee alkaa edellisen bändin viimeisellä kirjaimella. Peliä voi pelata toistuvasti ja se tarjoaa kolme arvauskertaa per pelikerta.
<br>
<br>

## Linkit:
* **Tutustu peliin tarkemmin**: Katso YouTube-videomme, jossa selitämme pelin toimintaperiaatteet ja annamme vinkkejä sen pelaamiseen.
  
* **Kokeile peliä itse**: Siirry [Codespaces](https://organic-yodel-wrgw4pqxq59c95g4.github.dev/) -sivustollemme pelataksesi peliä suoraan selaimessasi.
<br>
<br>

## Pelin kulku
<img src="pelin virtauskaavio.png" alt="komento" style="">

## Toiminnallisuudet
- Lataa bändien nimet tiedostosta ja muokkaa ne käyttökelpoiseen muotoon.
- Tarkistaa käyttäjän syöttämien bändien nimien kelvollisuuden.
- Hallinnoi pelin kulkua ja käyttäjän syötteitä.


## Käyttö
1. Varmista, että Python on asennettu.
2. Suorita peli komennolla:
   <img src="Screenshot 2024-05-29 021312.png" alt="komento" style="">
    
=======

## Pelin koodi ja selitykset

### Moduulien tuonti
```python
import shutil
import pydoc 
```
### Funktiot
<br>

### lataa_bandit
```python
def lataa_bandit(tiedostonimi):
    """
    Lataa bändien nimet tiedostosta, 
    poistaa ylimääräiset välilyönnit 
    ja muuttaa nimet pieniksi kirjaimiksi.
    """
    with open(tiedostonimi, 'r') as tiedosto:
        bandit = [line.strip().lower() for line in tiedosto.readlines()]
    return bandit
```
###### Tämä funktio lataa bändien nimet tiedostosta, poistaa ylimääräiset välilyönnit ja muuttaa kaikki nimet pieniksi kirjaimiksi.
<br>

### on_validi_arvaus
```python
def on_validi_arvaus(bandi, bandit, arvatut_bandit, viimeinen_kirjain):
    """
    Tarkistaa, että arvaus on listalla, 
    ei vielä arvattu ja alkaa oikealla kirjaimella.
    """
    bandi = bandi.lower()
    return (
        bandi in bandit and
        bandi not in arvatut_bandit and
        (bandi[0] == viimeinen_kirjain or viimeinen_kirjain == '')
    )
```
###### Tämä funktio tarkistaa, onko käyttäjän syöttämä arvaus kelvollinen. Arvauksen täytyy olla bändilistalla, ei vielä arvattu ja alkaa oikealla kirjaimella.
<br>

### Pääfunktio
```python
def main():
    # Hallinnoi pelin kulkua ja käyttäjän syötteitä.
    tiedostonimi = 'Bands_List.txt'
    bandit = lataa_bandit(tiedostonimi)
    arvatut_bandit = []
    viimeinen_kirjain = ''
    laskin = 1

    # Pelin ohjeet ja aloitus.
    print("Tervetuloa Arvaa Bändi -peliin!")
    print("Yritä arvata rockbändi. Jokaisen uuden bändin tulee alkaa edellisen bändin viimeisellä kirjaimella.")
    print("Pelissä on kolme yritystä, jonka jälkeen peli päättyy, jos et arvaa oikein.")
    print("Kirjoita 'lopeta' lopettaaksesi pelin.")
    print()

    while True:
        arvaus = input(f"Anna bändi, joka alkaa kirjaimella '{viimeinen_kirjain.upper()}' (tai mikä tahansa, jos ensimmäinen arvaus): ").strip()
        print()

        if on_validi_arvaus(arvaus, bandit, arvatut_bandit, viimeinen_kirjain):
            arvatut_bandit.append(arvaus.lower())
            print(f"Hyvä! {arvaus} on oikein.")
            viimeinen_kirjain = arvaus[-1].lower()
        elif laskin < 3 and arvaus.lower() != 'lopeta':
            laskin += 1
            print(f"Virheellinen arvaus tai on jo arvattu. (Virhe {laskin-1}) Yritä uudelleen.")
        else:
            if laskin == 3:
                print("3 virhettä. Hävisit.")
            print("Kiitos pelaamisesta!")
            break

    # Tulostetaan arvatut bändit keskitettynä
    print("Arvasit seuraavat bändit:".center(shutil.get_terminal_size().columns))
    for bandi in arvatut_bandit:
        padding_width = (shutil.get_terminal_size().columns - max(len(bandi) for bandi in arvatut_bandit)) // 2
        print(' ' * padding_width + bandi.upper())
    print()
```
###### Pääfunktio `main()` hallinnoi koko "Arvaa Bändi" -pelin kulkua. Tämä funktio vastaa muuttujien alustamisesta, tietojen lataamisesta tiedostosta ja pelilogiikan hallinnasta, joka sisältää käyttäjän syötteiden vastaanottamisen, arvausten tarkistamisen ja arvattujen bändien seurannan. Lisäksi se hallitsee pelin kierroksia ja määrittää, milloin peli päättyy, joko voiton, tappion tai käyttäjän pelin lopetuksen myötä.
<br>

### Pelin suorittaminen
```python
if __name__ == "__main__":
    # Pelin toistuva suoritus kunnes käyttäjä päättää lopettaa
    while True:
        main()
        uusi_peli = ''
        while uusi_peli != 'k' and uusi_peli != 'e':
            uusi_peli = input("Haluatko pelata uudelleen? [K]yllä vai [E]i ").lower().strip()
            if uusi_peli != 'k' and uusi_peli != 'e':
                print("Syötä 'K' jatkaaksesi tai 'E' lopettaaksesi.")
        if uusi_peli == 'e':
            break
    print("Heippa!!")
    pydoc.writedoc('Arvaa_Bändi')
```
###### Koodi `if __name__ == "__main__"` varmistaa, että pääfunktio main() suoritetaan vain, kun skriptiä ajetaan suoraan, ei kun sitä tuodaan moduulina. Tämä koodiblokki mahdollistaa pelin käynnistämisen ja tarjoaa käyttäjälle mahdollisuuden pelata useita kertoja, kunnes hän päättää lopettaa pelin.



