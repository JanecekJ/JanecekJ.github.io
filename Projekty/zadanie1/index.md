---
layout: post
title: Zadanie 1 - Osobná webová prezentácia na GitHub pages
categories: Webové publikovanie
---
Vytvorte webovú prezentáciu (webové sídlo) o sebe. Zamerajte sa jednak na vaše profesné záujmy (napr. projekty, ktoré riešite/riešili ste, čo vás v informatike najviac baví, fascinuje = váš developerský profil) a jednak vaše osobné záujmy, hobby.

V rámci developerského profilu vytvorte sekciu Webové publikovanie, kde budete publikovať všetky tri vaše vypracované zadania z predmetu.

Využite pritom technológie Git + GitHub Pages + Jekyll + Markdown. Využite potenciál statického generátora Jekyll a jeho templatovacích možností.

# Riešenie
Zadanie som vyvíjal na lokálnej verzií pomocou Jekyll. Následne som zmeny zverejňoval na GitHube. Na vizuálnu časť stránky bol použitý bootstrap.

## 5 podstránok a využitie layoutov

+ default.html	 
+ picture.html
+ projects.html
+ blog.html
+ post.html
+ basic.html

Ich využitie:
- **Úvod** -> default.html
- **O mne**	-> profil.htm -> basic.html	
- **Príroda** -> picture.html -> basic.html
- **Programovanie** ->	picture.html -> basic.html
- **Projekty** -> projects.html -> basic.html
- **Blog** -> blog.html -> basic.html
- **Post** -> post.html -> basic.html

## 5 premenných
Premenné využívam na viacerých stránkach. Buď ako parameter pre layout aby vedel ktorý dátový súbor má použiť 
[picture.html](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/_layouts/picture.html){:target="_blank"} + 
[stránka o prírode](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/priroda/index.md){:target="_blank"} 
(počet premenných 1).

Alebo priamo na posunutie údajov aby sa v prípade potreby dali ľahko zmeniť s inou hodnotou 
[profil.html](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/_layouts/profil.html){:target="_blank"} + 
[stránka o mne](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/o_mne/index.md){:target="_blank"} 
(počet premenných 3). 

Tak isto aj pri
[postoch](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/_posts/2017-03-10-testovaci-post.md){:target="_blank"} 
 (počet premenných 2).

## Dátové súbory
Ako zdroj dát som využíval **JSON** súbory. 

- **Príroda** -> priroda.json
- **Porgramovanie** -> programovanie.json
- **Úvod** -> polozky_menu.json
- **Projekty** -> projekty.json

## Filtre a tagy
Využívam niekoľko konštrukcí jazyka Liquid.
Napríklad:
**for**, **if**, **sort_natural**, **assign**, **break** v 
[picture.html](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/_layouts/picture.html){:target="_blank"}

**size** v 
[projects.html](https://github.com/JanecekJ/JanecekJ.github.io/blob/master/_layouts/projects.html){:target="_blank"}

## Pluginy
Pluginy som použil 3.

### 1.
Prvým je **jemoji**. Ten konvertuje emoji z textovej formy : smile :  : +1 : na obrázky :smile: :+1: . Funguje aj na GitHub pages.

### 2.
Druhým je **jekyll-email-protect**. Ten zabraňuje crawler botom získať emailovú adresu zo stránky. Footer -> Mailový kontakt

**pokus@gmail.com** == **%70%6F%6B%75%73@%67%6D%61%69%6C.%63%6F%6D**. 

Nefunguje na GitHub pages.

### 3.
Tretím je **liquid-reading-time**. Ten pomocou algoritmu odhadne ako dlho bude trvať préčítanie textu. Stránka Blog

Text: Ahoj ako sa máš?

Trvanie v minútach: 1

Nefunguje na GitHub pages.