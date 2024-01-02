---
layout: page
title: Gradle
inheader: no
permalink: /gradle/
---
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/3.0/">
  <img alt="Creative Commons -lisenssi" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/3.0/88x31.png"
  />
</a>


[Gradle](https://en.wikipedia.org/wiki/Gradle) on ns. [build automation](https://en.wikipedia.org/wiki/Build_automation) työkalu jonka tarkoituksena on automatisoida
projektinhallintaan liittyviä tehtäviä, kuten sen kääntaminen ja testaus. 


Gradlea käytetään pääsääntöisesti java projektien kehitykseen, sille löytyy kuitenkin myös 
(epävirallisia) [python plugineja](https://github.com/PrzemyslawSwiderski/python-gradle-plugin), jotka mahdollistavat käytön myös python projektien kehitykseen. 
Python plugineja saa käyttää omassa harjoitustyössä, mutta niiden toimivuutta ei ole kurssinhenkilökunnan puolelta testattu. 
Toisaalta, koska python projeteja ei tarvitse (eikä helposti voikkaan) kääntää, gradle on tavallaan turhan monimutkainen pythonin kanssa käytettäväksi. 
Suosittelemme pythonille ennemmin [Poetrya](/poetry/).


## Johdatus Gradlen konfigurointiin (Javalle)

_Nämä ohjeet ovat melkein suora kopio kurssin [Ohjelmistotuotanto](https://ohjelmistotuotanto-hy.github.io/gradle/) kurssin sivuilta_ 

Tehdään gradle-projekti alusta asti itse. Tee [palautusrepositorioosi](/tehtavat1#teht%C3%A4vien-palautusrepositoriot) uusi hakemisto ja mene hakemistoon. 

Kokeile toimiiko koneessasi komento `gradle`. Huomaa, että esimerkiksi fuksiläppäreissä on asennettuna erittäin vanha gradlen versio. Komennon suorittaminen näyttää mikä versio on kyseessä

```
mluukkai@melkki:~$ gradle
Starting a Gradle Daemon (subsequent builds will be faster)

> Task :help

Welcome to Gradle 4.4.1.
```


Jos komento ei toimi **tai versio on vanhempi kuin 6.7**, kopioi hakemistoon jostain aiemmasta gradle-projektistasi (edellisen viikon tehtävistä) tiedosto _gradlew_ jos käytät Linux tai OSX tai _gradlew.bat_ jos käytät Windowsia ja käytä jatkossa komentoa _./gradlew_ tai _gradlew_. Tämän lisäksi joudut kopioimaan aiemmasta projektista myös hakemiston _gradle_ ja sen alla olevan hakemiston _gradle/wrapper_.

Mikäli edelleen ei toimi voit ottaa lähtökohdaksesi [täällä olevan](https://github.com/ohjelmistotuotanto-hy/syksy2021-java/tree/main/koodi/viikko2/TyhjaProjeki) Matti Luukkaisen alustaman tyhjän gradle-projektin.

Aloita antamalla komento _gradle_:

```
> Task :help

Welcome to Gradle 6.7.

To run a build, run gradle <task> ...

To see a list of available tasks, run gradle tasks

To see a list of command-line options, run gradle --help

To see more detail about a task, run gradle help --task <task>

For troubleshooting, visit https://help.gradle.org
...
```

Ohje neuvoo meitä seuraavasti: "To see a list of available tasks, run gradle tasks", eli kokeillaan komentoa _gradle tasks_:

```
> Task :tasks

------------------------------------------------------------
Tasks runnable from root project
------------------------------------------------------------

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'intro_gradle'.
components - Displays the components produced by root project 'intro_gradle'. [incubating]
dependencies - Displays all dependencies declared in root project 'intro_gradle'.
dependencyInsight - Displays the insight into a specific dependency in root project 'intro_gradle'.
dependentComponents - Displays the dependent components of components in root project 'intro_gradle'. [incubating]
help - Displays a help message.
model - Displays the configuration model of root project 'intro_gradle'. [incubating]
outgoingVariants - Displays the outgoing variants of root project 'intro_gradle'.
projects - Displays the sub-projects of root project 'intro_gradle'.
properties - Displays the properties of root project 'intro_gradle'.
tasks - Displays the tasks runnable from root project 'intro_gradle'.
```

Komento listaa käytettävissä olevat _taskit_. Gradlen [dokumentaatio](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html) kuvaa taskeja seuraavasti:

> Each project is made up of one or more tasks. A task represents some atomic piece of work which a build performs. This might be compiling some classes, creating a JAR, generating Javadoc, or publishing some archives to a repository.

Taskit ovat siis "komentoja", joita voimme suorittaa gradle-projekteille.

Gradle-projekti määritellään projektihakemiston juureen sijoitettavan tiedoston _build.gradle_ avulla.  

**Saat luotua tiedoston** suorittamalla taskin _init_ (eli antamalla komennon _gradle init_).

Valitse _basic_ (type of project), _Groovy_ (build script DSL) ja anna projektille nimi.

Huomaat että operaation jälkeen hakemistoon on tullut tiedoston _build.gradle_ lisäksi muutakin:

<pre>
$ ls -la
-rw-r--r--  1 mluukkai  984178727   198 Oct 27 18:00 build.gradle
drwxr-xr-x  3 mluukkai  984178727    96 Oct 27 18:00 gradle
-rwxr-xr-x  1 mluukkai  984178727  5766 Oct 27 18:00 gradlew
-rw-r--r--  1 mluukkai  984178727  2763 Oct 27 18:00 gradlew.bat
-rw-r--r--  1 mluukkai  984178727   359 Oct 27 18:00 settings.gradle
</pre>

Näistä hakemisto _.gradle_ kannattaa gitignoroida. Gradle-projekteissa tulee gitignoroida aina myös hakemisto _build_ mihin kaikki gradle-taskien generoimat tiedostot sijoitetaan. Gradle luokin valmiiksi tilanteeseen sopivan gitignore-tiedoston. 

Tavoitteenamme on lisätä projektiin Java-koodia ja JUnit-testejä. Oletusarvoisesti gradle ei ymmärrä Javasta mitään, mutta ottamalla käyttöön _java-pluginin_, se lisää projektille uusia, Javan kääntämiseen liittyviä taskeja.

Otetaan nyt käyttöön java-plugin lisäämällä tiedostoon _build.gradle_ rivi:

<pre>
plugins {
    id 'java'
}
</pre>

Kun nyt suoritetaan komento _gradle tasks_ huomataan että listalla on uusia, java-pluginin lisäämiä taskeja:

<pre>
Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.

Build Setup tasks
-----------------
init - Initializes a new Gradle build.
wrapper - Generates Gradle wrapper files.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

...

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.
</pre>

Voimme nyt siis suorittaa projektille esim. viime viikoilta tutut komennot _gradle build_ ja _gradle test_.

Jos suoritamme esimerkiksi taskin _build_ eli komennon _gradle build_, on tulostus seuraava

<pre>
BUILD SUCCESSFUL in 885ms
</pre>

Tämä ei vielä oikein kerro mitään. Java-pluginin [dokumentaatio selventää asiaa](https://docs.gradle.org/current/userguide/java_plugin.html#sec:java_tasks):

![]({{ "/images/lh2-1.png" | absolute_url }})

_Build_ siis riippuu kahdesta pluginista _check_ ja _assemble_:

![]({{ "/images/lh2-2.png" | absolute_url }})

Myös nämä riippuvat muutamasta taskista. Lopulta käy niin, että _gradle build_ suorittaa kaikki seuraavat taskit

<pre>
compileJava
processResources 
classes
jar
assemble
compileTestJava
processTestResources
testClasses
test
check
</pre>

Eli _build_ suorittaa koodin käännöksen, paketoinnin jar-tiedostoksi sekä projektiin liittyvät testit. 

Ennen kun siirryt eteenpäin, suorita _gradle clean_, joka poistaa kaikki edellisen komennon luomat tiedostot.

## Järkevä editori

Älä käytä tällä kertaa NetBeansia tai muutakaan IDE:ä vaan tee kaikki koodi ja konfiguraatiot tekstieditorilla. 

Hyvä vaihtoehto editoriksi on laitoksen koneilta ja fuksikannettavista löytyvä [Visual Studio Code](https://code.visualstudio.com)

## Koodin lisääminen projektiin

Gradle olettaa, että ohjelman koodi sijaitsee projektin juuren alla olevassa hakemistossa _src/main/java_. Luo hakemisto(t) ja tiedosto _src/main/java/Main.java_ ja sille esim. seuraava sisältö:

<pre>
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello gradle!");
    }
}
</pre>

Suorita sitten task  _compileJava_ ja tarkastele projektihakemiston sisältöä komennolla _tree_:

<pre>
$ tree
.
├── build
│   ├── classes
│   │   └── java
│   │       └── main
│   │           └── Main.class
...
</pre>

Task _compileJava_ on siis luonut hakemiston _build_ ja sen sisälle käännöksen tuloksena olevan _class_-tiedoston.

Suorita käännetty koodi menemällä hakemistoon ja antamalla komento _java Main_:

<pre>
$ cd build/classes/java/main/
$ java Main
Hello gradle!
</pre>

Yleensä Java-koodia ei suoriteta käyttämällä suoraan _class_-tiedostoja. Parempi tapa on pakata koodi  _jar_-tiedostoksi.

Jar-tiedosto muodostetaan gradlen taskilla _jar_. Help kertoo seuraavaa:

<pre>
$ gradle help --task jar
Detailed task information for jar

Path
     :jar

Type
     Jar (org.gradle.api.tasks.bundling.Jar)

Description
     Assembles a jar archive containing the main classes.
</pre>

**HUOM** komento _gradle_ tulee suorittaa aina projektihakemiston juuressa, eli hakemistossa missä tiedosto _build.gradle_ sijaitsee.

Määritellään taskia varten _pääohjelman sijainti_ lisäämällä seuraava tiedoston _build.gradle_ loppuun:

<pre>
jar {
    manifest {
        attributes 'Main-Class': 'Main'
    }
}
</pre>

Huomaa, että tiedoston _build.gradle_ pitää alkaa _plugins_-määrittelyllä. Jos se ei ole alussa, törmäät seuraavaan virheilmoitukseen:

<pre>
FAILURE: Build failed with an exception.

* Where:
Build file '/Users/mluukkai/dev/intro_gradle/build.gradle' line: 7

...

@ line 7, column 1.
  plugins {
</pre>

Palaa nyt projektihakemistoon ja suorita jar-tiedoston generoiva task _jar_ (eli anna komento _gradle_ jar). 

Voit suorittaa syntyneen jar-tiedoston seuraavasti (huomaa että tiedoston nimi riippuu projektisi nimestä ja todennäköisesti ei ole _gradle-test.jar_):

<pre>
$ java -jar build/libs/gradle-test.jar
Hello gradle!
</pre>

## application-plugin

Aiemmissa tehtävissä olemme pystyneet suorittamaan koodin myös komennolla _gradle run_. 

Komento aiheuttaa kuitenkin nyt virheilmoituksen _Task 'run' not found in root project_.

Syynä tälle on se, että task _run_ ei ole java-pluginin vaan [application-pluginin](https://docs.gradle.org/current/userguide/application_plugin.html) määrittelemä. Otetaan tämä käyttöön  muuttamalla tiedoston _build.gradle_ alku muotoon 

<pre>
plugins {
    id 'java'
    id 'application'
}
</pre>

Itseasiassa _java_-pluginin määrittelevää riviä ei nyt edes tarvita, sillä _application_ sisältää myös sen määrittelevät taskit.

Komento  _gradle run_ aiheuttaa nyt virheen _No main class specified_.

Pluginin [dokumentaatio](https://docs.gradle.org/current/userguide/application_plugin.html) kertoo, että pääohjelman sisältävä luokka, eli _main class_ tulee määritellä lisäämällä tiedostoon _build.gradle_ seuraava rivi:

<pre>
application {
    mainClass = 'Main'
}
</pre>

Nyt ohjelman suorittaminen taskin avulla toimii:

<pre>
$ gradle run
> Task :compileJava UP-TO-DATE
> Task :processResources NO-SOURCE
> Task :classes UP-TO-DATE

> Task :run
Hello gradle!

BUILD SUCCESSFUL in 1s
</pre>

Suorittaminen kannattanee tehdä optiota _-q_ käyttäen, jolloin gradle jättää omat tulosteensa tekemättä:

<pre>
$ gradle run -q
Hello gradle!
</pre>

Laajennetaan ohjelmaa siten, että se kysyy Scanner-olion avulla käyttäjän nimeä:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        System.out.println("kuka olet: ");
        String nimi = scanner.nextLine();

        System.out.println("Hello " + nimi);
    }
}
```

Jos ohjelmasta tehdään _jar-tiedosto_ (suorittamalla komento _gradle jar)_, toimii se odotetulla tavalla:

<pre>
$ java -jar build/libs/gradle-test.jar
kuka olet:
mluukkai
Hello mluukkai
</pre>

Jos ohjelma suoritetaan gradlen _run_-taskin avulla, seurauksena on virhe:

<pre>
$ gradle run
kuka olet:
Exception in thread "main" java.util.NoSuchElementException: No line found
        at java.util.Scanner.nextLine(Scanner.java:1540)
        at Main.main(Main.java:7)

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':run'.
> Process 'command '/Library/Java/JavaVirtualMachines/jdk1.11.0_101.jdk/Contents/Home/bin/java'' finished with non-zero exit value 1

</pre>

Syynä tälle on se, että oletusarvoisesti gradlen task _run_ ei liitä terminaalia syötevirtaan _System.in_. Asia saadaan korjautumaan lisäämällä tiedostoon _build.gradle_ seuraava: 

<pre>
run {
    standardInput = System.in
}
</pre>

Nyt komento _gradle run_ toimii. 

## Toinen luokka

Lisätään ohjelmalle luokka, jonka avulla on mahdollista laskea kertolaskuja. Sijoitetaan luokka pakkaukseen _ohtu_ eli tiedostoon _src/main/java/ohtu/Multiplier.java_

```java
package ohtu;

public class Multiplier {
    private int value;
    public Multiplier(int value) {
        this.value = value;
    }

    public int multipliedBy(int other) {
        return value * other;
    }

}
```

Käytetään luokkaa pääohjelmasta käsin. Huomaa, että koska luokka on eri pakkauksessa kuin pääohjelma, tulee pakkaus importata:

```java
import java.util.*;
import ohtu.Multiplier;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Multiplier kolme = new Multiplier(3);
        System.out.println("anna luku ");
        int luku = scanner.nextInt();

        System.out.println("luku kertaa kolme on " + kolme.multipliedBy(luku) );
    }
}
```

## Testit

Tehdään nyt luokalle JUnit-testi. Gradle olettaa, että JUnit-testit sijoitetaan hakemiston _src/test/java_ alle. Sijoitamme testin samaan pakkaukseen kuin testattavan luokan, eli tiedostoon _src/test/java/ohtu/MultiplierTest.java_

```java
package ohtu;

import static org.junit.Assert.*;
import org.junit.Test;

public class MultiplierTest {

    @Test
    public void kertominenToimii() {
        Multiplier viisi = new Multiplier(5);

        assertEquals(5, viisi.multipliedBy(1));
        assertEquals(35, viisi.multipliedBy(7));
    }

}
```

Yritetään suorittaa testit komennolla _gradle test_. Seurauksena on suuri määrä virheilmoituksia. Virheet tapahtuvat taskin _compileTestJava_ eli testien kääntämisen aikana:

<pre>
$ gradle test

> Task :compileTestJava FAILED
/Users/mluukkai/dev/intro_gradle/src/test/java/MultiplierTest.java:3: error: package org.junit does not exist
import static org.junit.Assert.*;
                       ^
/Users/mluukkai/dev/intro_gradle/src/test/java/MultiplierTest.java:4: error: package org.junit does not exist
import org.junit.Test;
                ^
/Users/mluukkai/dev/intro_gradle/src/test/java/MultiplierTest.java:8: error: cannot find symbol
    @Test
     ^
  symbol:   class Test
  location: class MultiplierTest
/Users/mluukkai/dev/intro_gradle/src/test/java/MultiplierTest.java:12: error: cannot find symbol
        assertEquals(5, viisi.multipliedBy(1));
        ^
  symbol:   method assertEquals(int,int)
  location: class MultiplierTest
/Users/mluukkai/dev/intro_gradle/src/test/java/MultiplierTest.java:13: error: cannot find symbol
        assertEquals(35, viisi.multipliedBy(7));
        ^
  symbol:   method assertEquals(int,int)
  location: class MultiplierTest
5 errors
</pre>

Syynä virheille on se, että projektimme ei tunne testien importtaamaa koodia:

<pre>
import static org.junit.Assert.*;
import org.junit.Test;
</pre>

JUnit-kirjasto on siis ohjelmamme testien käännöksen aikainen _riippuvuus_. 

## Riippuvuudet

Käytännössä riippuvuudet ovat jar-tiedostoja, jotka sisältävät käytettävien apukirjastojen, eli tässä tapauksessa JUnitin koodin. Gradlen samoin kuin Mavenin hyvä puoli on se, että ohjelmoijan ei tarvitse itse latailla riippuvuuksia, riittää kun projektin riippuvuudet määritellään tiedostossa _build.gradle_ ja gradle hoitaa automaattisesti riippuvuuksien lataamisen, jos niitä ei koneelta löydy.

Tarvittava määrittely on seuraava:

<pre>
repositories {
    jcenter()
}

dependencies {
    testImplementation group: 'junit', name: 'junit', version: '4.13'
}
</pre>

Ensimmäinen osa _repositories_ kertoo gradlelle mistä sen tulee etsiä riippuvuuksia. [jcenter](https://bintray.com/bintray/jcenter) on eräs niistä paikoista, johon on talletettu suuri määrä gradlen ja mavenin käyttämiä kirjastoja. Toinen vaihtoehtoinen paikka riippuvuuksien etsimiseen on [mavenCentral](https://search.maven.org). _repositories_-osassa voidaan määritellä myös useita paikkoja joista gradle käy etsimässä projektiin määriteltyjä riippuvuuksia.

Toinen osa määrittelee, että _testImplementation_-vaiheeseen otetaan käyttöön JUnit-kirjaston versio 4.13. Käytännössä tämä tarkoittaa, että kääntäessään testien koodia gradle liittää JUnitin _classpathiin_.

Lisättävän riippuvuuden erittelevälle riville voidaan käyttää myös vaihtoehtoista syntaksia, missä riippuvuus, ja sen versio ilmaistaan yhtenä merkkijonona:

<pre>
dependencies {
    testImplementation 'junit:junit:4.13'
}
</pre>

Kun suoritamme uudelleen komennon _gradle test_ kaikki toimii. 

Rikotaan vielä testi ja varmistetaan että testit huomaavat virheen.

JUnitin uusi versio [JUnit5](http://junit.org/junit5/) on ilmestynyt vuosien odotuksen jälkeen jo jonkin aikaa sitten. JUnit5:ssä on monia mielenkiintoisia uudistuksia, mutta valitettavasti työkalutuki on edelleen vielä niin keskeneräinen, että joudumme kurssilla käyttämään vielä vanhaa JUnitia.
