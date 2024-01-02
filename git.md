---
layout: page
permalink: /git
title: Git 
title_long: Gitin käyttö harjoitustyössä 
inheader: no
---

<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/">
  <img alt="Creative Commons -lisenssi" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/88x31.png"
  />
</a>

_Alkuperäinen ohje: Mika Huttunen ja Silja Polvi_

# Aloittaminen
ÄLÄ käytä Netbeansin tarjoamaa Git-lisäosaa ÄLÄKÄ graafisia ohjelmia. Tähän asti kaikki kurssilla päivän koodaustyönsä menettäneiden ongelmat ovat johtuneet näistä. Käytä aina Linuxissa/OSX:ssä terminaalia tai Windowsissa Git Bashia, ja tee Commitit sekä Pushit käsin.

Kokeile ensin onko git asennettu koneellesi, esim ajamalla komento "git -v" terminaalissa jolloin tulosteen pitäisi musitittaa jotain seuraavanlaista:
```  
jezberg@LM2-500-27156 ~ % git -v 
git version 2.39.3 (Apple Git-145)
```
Mikäli saat virheimoituksen (esim "command not found"), asenna ensin git esim [näillä sivuilla](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) olevien ohjeiden mukaisesti. 

Gitin asentamisen jälkeen, luo seuraavaksi [Github](https://github.com/) tili (personal, ei enterprise).
Tilin luomisen jälkeen voit luoda uuden repositorion ylä oikealla olevan "+" symbolin alla löytyvän "create repository" napin avulla. 

![]({{ "/images/git-0.png" | absolute_url }})

Laita repositorion nimeksi esimerkiksi “harjoitustyö”, harjoitustyösi aihe tai oma nimiehdotuksesi ohjelmalle ** Valitse “Public” ** Valitse ruutu “Initialize this repository with a README”.
Halutessasi voit myös valita omalle koodauskielellesi sopivan "gitignore" tiedoston (lisää gitigonersta tässä alla)

![]({{ "/images/git-1.png" | absolute_url }})


## SSH-avaimen luominen omalla koneella
Katso ohje [tästä](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

## Repositorion valmisteleminen käyttöä varten
**HUOMAA:** Tässä ohjeessa komentorivillä tarkoitetaan Linuxilla ja Macilla terminaalia, ja Windowsin tapauksessa Git Bashia! Windowsin omat komentorivit eivät tunnista git-alkuisia komentoja.

Hankkiudu Githubin sivulla luomasi repositorion näkymään. Sinne pääsee vaikkapa etusivulta kun olet kirjautunut, klikkaamalla repositorion nimeä.

![]({{ "/images/git-2.png" | absolute_url }})

Paina napista "code" ja avautuvasta ponnahdusikkunasta "ssh". 

![]({{ "/images/git-3.png" | absolute_url }})

Kopioi näkyviin tuleva repositorion osoite, joka on suunnilleen muotoa git@github.com:käyttäjätunnuksesi/HarjoitusRepo.git.
Osoitteen voi kopioida esimerkiksi painamalla osoitteen vieressä olevaa symbolia. 

Avaa komentorivi ja kopioi (cloonaa) reposirotio omalle koneellesi komennolla git clone "osoite"
```
jezberg@LM2-500-27156 Documents % git clone git@github.com:jezberg/harjoitustyo.git
Cloning into 'harjoitustyo'...
remote: Enumerating objects: 4, done.
remote: Counting objects: 100% (4/4), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 4 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (4/4), done.
jezberg@LM2-500-27156 Documents % 
```
Mene komentorivillä äsken kloonaamaasi kansioon komennolla ```cd Hharjoitustyo``` tms. Kaikki tiedostot voit listata komennolla ```ls```.
Nyt komennolla ls tulisi näkyä tiedosto README.
```
jezberg@LM2-500-27156 Documents % cd harjoitustyo 
jezberg@LM2-500-27156 harjoitustyo % ls
README.md
jezberg@LM2-500-27156 harjoitustyo % 
```
Tämä kansioon lisätään nyt kaikki harjoitustyöhön liittyvät tiedostot joista ne voidaan siirtää Gihubiiin. Harjoitellaan seuraavaksi sen käyttöä kolmella treenillä. 

# Repotreeni 1: Muokkaus
Varmista, että olet komentorivillä repositoriokansiossasi
Avaa README-tiedosto millä tekstieditorilla haluat.
Kirjoita tiedostoon jotakin ja tallenna se. 
![]({{ "/images/git-4.png" | absolute_url }})

Anna komentorivillä komento git status. Nyt näet luettelon tiedostoista, joita olet muokannut (modified), tässä tapauksessa README-tiedosto.
```
jezberg@LM2-500-27156 harjoitustyo % git status
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```
Tästä näet, että viimeisimmän commitin (tallennuksen) jälkeen tiedostoa "README.md" on muokattu. 
Comittaaksesi (tallentaaksesi) nämä muutokset meidän täytyy (periaattessa)) ensin lisätä muokatut tiedostot comittiin, ja sitten commitata ne jonkun kuvaavan viestin avulla. 
Kuitenkin, jos haluamme tallentaa kaikki muutokset, voimme käyttää comit kommennon "-a" vipua. Viestin voi suoraan kirjoittaa commit komennon "-m" vivulla. 
``` 
jezberg@LM2-500-27156 harjoitustyo % git commit -am "ensi muokkaus"
[main 1351813] ensi muokkaus
 1 file changed, 3 insertions(+), 1 deletion(-)
jezberg@LM2-500-27156 harjoitustyo % 
```
Jos tästä kommennosta unohtuu viesti (tässä. "ensi muokkaus") unohtuu, aukeaa editori (vim tain nano) jonne viestin voi kirjoittaa. Jos editorin käyttö ei ole tuttua, siitä voi myös poistua 
komennolla :q ja antaa commit viestin uudelleen.

Nyt tekemäsi muokkaukset (README-tiedoston muokkaus) ovat paikallisessa repositoriossasi. Sieltä ne pitää vielä työntää verkossa olevaan Githubin repoosi komennolla git push. 
```
jezberg@LM2-500-27156 harjoitustyo % git push                      
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 311 bytes | 311.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:jezberg/harjoitustyo.git
   4b835c6..1351813  main -> main

```
Jos git push sanoo “Permission denied (publickey)”, kokeile ssh-avaimen generointia uudestaan tai komentoa ssh-add

Mene Githubin sivulla olevaan repositoriosi näkymään (kuten edellisen ohjeen kohdassa 1) ja päivitä näkymä. Huomaat, että README-tiedostoon tekemäsi muokkaukset ovat nyt Githubissa asti, antamasi viestin kera!
![]({{ "/images/git-5.png" | absolute_url }})


# Repotreeni 2: Tiedoston lisäys
Hankkiudu komentorivillä repositorion kansioosi. Anna komento ```mkdir testikansio``` luodaksesi uuden kansion.
Lisää testikansioosi jokin tiedosto: etsi tietokoneeltasi luomasi kansio ja luo tai siirrä sinne vaikkapa tekstitiedosto.
```
jezberg@LM2-500-27156 harjoitustyo % mkdir testikansio 
jezberg@LM2-500-27156 harjoitustyo % ls
README.md	testikansio
jezberg@LM2-500-27156 harjoitustyo % vim testikansio/tekstitesti.txt
jezberg@LM2-500-27156 harjoitustyo % ls
README.md	testikansio
jezberg@LM2-500-27156 harjoitustyo % ls testikansio/ 
tekstitesti.txt
```
Lisää luomasi kansio versionhallinnoitavaksi antamalla komentoriville komento ```git add testikansio```. 
Nyt versionhallinta seuraa kansiotasi siinä mielessä, että se näkyy status-komennolla, ja sen voi siirtää paikalliseen versionhallintaan (commitilla). 
**Huomaa,** että typötyhjä kansio ei tule hallinnoitavaksi add-komennolla.

Anna commit-komento: 
``` 
jezberg@LM2-500-27156 harjoitustyo % git commit -m "uusi testitiedosto"
[main 96644ec] uusi testitiedosto
 1 file changed, 1 insertion(+)
 create mode 100644 testikansio/tekstitesti.txt
```
Huomaa, että nyt ei tarvita -a flagia, koska annoimme git add komennon aikaisemmin. Tärkeää on huomata, että -a flagi commit komennon yhteydessä ei lisää uusia tiedostoja, vaan tallentaa
vain jo lisättyjen tiedostojen muuutokset. TOisin sanottuna, uusien tiedostojen lisäämisen yhteydessä täytyy aina antaa add komento erikseen. 

Anna push-komento (```git push```). Tarkista Githubin nettisivulta, että uusi kansiosi ja sen sisältämä tiedosto ilmestyvät repositorionäkymääsi.
![]({{ "/images/git-5.png" | absolute_url }})

Jos haluat lisätä useita tiedostoja kerralla, se onnistuu esimerkiksi komennolla ```git add``` joka lisää nykyisen kansion (.) kaikki uudet tiedostot verisonhallintaan. 

Nyt osaat lisätä Githubiin tiedostoja ja kansioita: add - commit - push! Tee samaan malliin harjoitustyöllesi dokumentaatiokansio ja sinne ensimmäisellä viikolla tarvittava dokumentointi.

# Repotreeni 3: Pull: muutosten haku Githubista
Jos työskentelet useammalla kuin yhdellä koneella, voit versionhallinnan avulla pitää projektin ajan tasalla ilman että joudut siirtämään tiedostoja koneelta toiselle esimerkiksi muistitikulla. Sehän on yksi versionhallinnan hyödyistä! Tässä harjoituksessa pääset harjoittelemaan tiedostojen kopioimista Githubista koneellesi toisen repositoriokloonikansion avulla. Todellisessa tilanteessa harjoituksessa luotava toinen kloonikansio vastaa eri koneella olevaa kloonikansiota. Et siis tarvitse toista kloonia (varjorepo) muuhun kuin tähän harjoitukseen.

Luo uudella nimellä (esim. varjorepo) toinen klooni repositoriosta antamalla komentorivillä sama komento tyyliin ```git clone git@github.com:käyttäjätunnuksesi/harjoitustyo.git varjorepo```. 
``` 
jezberg@LM2-500-27156 Documents % git clone git@github.com:jezberg/harjoitustyo.git varjorepo
Cloning into 'varjorepo'...
remote: Enumerating objects: 11, done.
remote: Counting objects: 100% (11/11), done.
remote: Compressing objects: 100% (7/7), done.
remote: Total 11 (delta 1), reused 6 (delta 0), pack-reused 0
Receiving objects: 100% (11/11), done.
Resolving deltas: 100% (1/1), done.
```
Klooni luodaan siis muuten kuten kohdassa <a href="/git#repositorion-valmisteleminen-käyttöä-varten">Repositorion valmistelu käyttöä varten</a>, paitsi että nyt kloonikansio nimetään itse.
Muokkaa alkuperäisessä repossa olevaa tiedostoa README. Anna add-komento, commit-komento ja push-komento alkuperäisessä repositoriokloonikansiossasi, jotta äsken tehty muutos päätyy Githubiin asti.
``` 
jezberg@LM2-500-27156 harjoitustyo % echo "repoharjoitus kolmosen lisäys readmehen" >> README.md 
jezberg@LM2-500-27156 harjoitustyo % git commit -a -m "Harjoituskolme" 
[main b1ecdc0] Harjoituskolme
 1 file changed, 1 insertion(+), 1 deletion(-)
jezberg@LM2-500-27156 harjoitustyo % git push 
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 383 bytes | 383.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:jezberg/harjoitustyo.git
   96644ec..b1ecdc0  main -> main
```
Tässä ensimmäinen komento lisää rivin "repoharjoitus kolmosen lisäys readmehen" tiedostoon "README". Huomaa myös, että tässä ei tarvita erillistä add komentoa, koska README on jo versionhallinassa. 

Mene tämän harjoituksen alussa luomaasi varjorepoon. Avaa nyt README-tiedosto tässä uudessa repossa, jolloin huomaat että se ei ole muuttunut.
```
jezberg@LM2-500-27156 varjorepo % cat README.md 
# harjoitustyo

Repotreenin harjoitus%   
```
Vedä alkuperäisessä kansiossasi tekemäsi muutokset varjorepoon Githubista komennolla ```git pull```.
```
jezberg@LM2-500-27156 varjorepo % git pull     
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 363 bytes | 72.00 KiB/s, done.
From github.com:jezberg/harjoitustyo
   96644ec..b1ecdc0  main       -> origin/main
Updating 96644ec..b1ecdc0
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
jezberg@LM2-500-27156 varjorepo % cat README.md
# harjoitustyo

Repotreenin harjoitusrepoharjoitus kolmosen lisäys readmehen
``` 
Huomaat, että README-tiedosto päivittyy niiden muutosten mukaan, jotka teit alkuperäisessä kloonissa! Tarkista tämä vielä varjorepossa olevasta README-tiedostosta.


# Git & Netbeans
Git ja Netbeans ovat hyviä kavereita. Käytä Netbeans-projektin tallennuspaikkana koneellasi olevaa repositoriokloonikansiota: aseta Netbeansissa kohtaan “Project Location” koneella oleva repositorion kansio (esim. HarjoitusRepo). Nyt tallennat projektiasi koko ajan repositorion kansioon.

Anna (kun olet ensin siirtynyt komentorivillä repositoriokansioosi) komentoriville komento git status. Huomaat kaksi uutta tiedostoa: harjoitustyösi lisäksi tiedoston .gitignore. Viimeksi mainittu on Netbeansin luoma tiedosto, joka antaa listan versionhallintaan kuulumattomista tiedostoista. Se pitää lisätä versionhallinnoitavaksi. Tiedostosta ei tarvitse ymmärtää enempää, mutta jos haluat tai tiedosto ei generoitunut automaattisesti, lue tämän dokumentin lopusta lyhyt .gitignore selite.
Anna komento git add harkkatyöprojektisiNimi ja sen jälkeen komento git add .gitignore.
Nyt komennon git status pitäisi näyttää sekä harkkatyöprojektisi sisältöineen että .gitignore Anna commit-komento ja push-komento. Nyt harjoitustyösi löytyy Githubistasi!
Nyt voit aloittaa harjoitustyösi tekemisen.

## Yhteenveto

1. Uudella koneella aloittaessasi luo ssh-avain ja kloonaa (clone) haluamasi repositorio koneelle
1. Työskentelyn aluksi vedä (pull) Githubista projektisi
1. Tehdessäsi muutoksia, lisää (add) uudet tiedostot ja muutetut tiedostot, sekä tee commit kuvaavalla kommentilla.
    - Add + Commit yhdistelmää voit ajatella työn tallentamisena. *Tee mieluummin liikaa kuin liian vähän committeja*. ```git status``` komennolla voit kysellä mitä olet muuttanut ja mitä olet commitoimassa. Ongelmatilanteissa on mahdollista palata vanhaan committiin, vaikka viikon takaa
1. Viimeistään lopettaessasi työskentelyn työnnä (push) muutoksesi Githubiin.
1. Tarkista selaimesta, että KAIKKI muutokset menivät repositorioosi: Muuten ohjaajat eivät niitä näe
1. Repositorio on myös varmuuskopio työstäsi - jos tietokoneesi hajoaa tai laitoksen palvelimet reistailevat, on kaikki vaivalla tekemäsi työ tallessa Githubissa!
1. Tiedoston poistaminen repositoriosta tehdään git rm tiedostonnimi komennon avulla. HUOM ole huolellinen tämän käytössä, sillä komento poistaa tiedoston myös paikallisesta repositoriosta (siis omalta koneeltasi)

**MUISTA:** Github-repositoriosi on julkinen. Sen näkee koko maailma. ÄLÄ pistä sinne esimerkiksi opiskelijanumerosi.
Nyt voit jatkaa harjoitustyötäsi miltä tahansa tietokoneelta aina siitä mihin lopetit edellisellä kerralla.

## Lyhyt .gitignore -ohje
.gitignore on tiedosto, joka sisältää tiedon niistä tiedostoista ja kansioista, joita ei haluta versionhallintaan eikä Githubiin. Nämä esitetään sääntöinä, jokainen sääntö omalla rivillään. Yleensä nämä tiedostot ovat kehitysympäristöjen joka ajokerralla generoitavia tai arkaluontoista tietoa sisältäviä tiedostoja, joita ei haluta versionhallintaan viemään tilaa tai kaikkien nähtäville. .gitignore tiedosto itsessään halutaan Githubiin.

Netbeans luo .gitignore -tiedoston automaattisesti, kun Netbeans-projektin luo kansioon, joka on repositorio. Netbeans lisää sinne säännön /projektikansiosi_nimi/nbproject/private/, joka estää versionhallintaan menemästä tiedostoja, jotka Netbeans luo aina kun käännät ja ajat ohjelmasi. git status komento ei siis huomaa lainkaan, vaikka private/ -kansion sisällä tapahtuu muutoksia.

Jos Netbeans jostain syystä ei tehnyt tällaista tiedostoa, voit luoda sen itse.

Tee tekstitiedosto, jolle annat nimeksi “.gitignore” *Tiedostolla ei ole tiedostopäätettä*.
Lisää tekstitiedostoon rivi /projektikansiosi_nimi/nbproject/private/
Lisää myös rivi /projektikansiosi_nimi/nbproject/dist/
Vastaavasti .gitignore-tiedostoon voi lisätä omia sääntöjä. Jokainen sääntö tulee omalle rivilleen.

Säännöistä löytää lisää esimerkkejä osoitteissa:
- [http://git-scm.com/docs/gitignore](http://git-scm.com/docs/gitignore)
- [https://help.github.com/articles/ignoring-files](https://help.github.com/articles/ignoring-files)
- [http://cfmumbojumbo.com/cf/index.cfm/coding/git-giving-files-the-cold-shoulder-gitignore/](http://cfmumbojumbo.com/cf/index.cfm/coding/git-giving-files-the-cold-shoulder-gitignore/)

### Issuiden salliminen 
Kurssin vertaisarviointi annetaan Github Issuena. 
Voidaksesi saada palautetta sinun täytyy ensin sallia issueiden luomisen alla olevan ohjeen mukaisesti. 

1. Mene repositoriosi
1. Klikkaa ylhäältä Settings
    ![]({{ "/images/git-7.png" | absolute_url }})
1. Settings sivulta ruksita kohta Issues (kts. kuva alta) asetukset tallentuvat automaattisesti
   ![]({{ "/images/git-8.png" | absolute_url }})



