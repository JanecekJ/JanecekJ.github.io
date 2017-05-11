---
layout: post
title: Zadanie 3 - XML prezentácia
categories: Webové publikovanie
---
Analyzujte možnosti zápisu jednoduchej prezentácie v jazyku XML. Identifikujte základné súčasti prezentácie a navrhnite XML elementy pre ich označkovanie (metadátové, štrukturálne, inline). Dbajte na znovupoužiteľnosť a vyvarujte sa redundancií. Návrh elementov zrealizujte opísaním typu dokumentu pomocou vybraného jazyka (DTD, XSD, RELAX NG) spolu s vysvetlením účelu jednotlivých elementov. Vo vami navhrnutom jazyku vytvorte ukážkovú prezentáciu, ktorá bude demonštrovať možnosti tvorby prezentácií podľa definície typu dokumentu.

Navrhnite a vytvorte XSLT šablóny pre konverziu prezentácie z XML do XHTML+CSS a pre konverziu prezentácie z XML do PDF. Klaďte dôraz na znovupoužitie jednotlivých šablon pre viaceré výstupné formáty. Umožnite zadávanie parametrov transformácií.

# Riešenie
+ Na opis dokumentu som sa rozhodol použiť variant XML schémy. Využil som ju na definíciu elementov, ktoré sú stavebnými prvkami XML prezentácie. Definoval som komplexné typy, ale použil som aj preddefinované typy. V komentároch som vysvetlil význam jednotlivých elementov pre prezentáciu.

+ Následne som vytvoril XML dokument reprezentujúci prezentáciu, ktorý využíval prvky zadefinované pomocou XML schémy. Prezentácia demonštruje možnosti definície. Ako tému prezentácie som si zvolil počítačovú hru, ktorú radi hráme s kamarátmi.

+ Na účel XSLT transformácií som vytvoril 3 súbory:
	- Prvým súborom je **basic.xsl**. Obsahuje zákaldné transformácie a šablony, z ktorých niektoré sa využívajú aj pri transformácií XML prezentácie do PDF aj HTML.
	- Druhým je **html.xsl**. Tento súbor slúži na transformáciu XML prezentáciu do HTML formátu. Každý slide prezentácie sa vygeneruje ako samostatný HTML súbor. Jednotlivé súbory sú medzi sebou prepojené odkazmi. Každý tento súbor využíva CSS stylesheet, ktorý definuje vizuálnu stránku prezentácie vo formáte HTML.
	- Tretím je **pdf.xsl**. Posledný súbor s transformáciami definuje šablony na vytvorenie **fo** súboru, ktorý je následne transformovaný na PDF prezentáciu.

+ Snažil som sa dosiahnuť čo najväčšiu podobnosť medzi HTML a PDF formátmi prezetácií.

+ Isté parametre transformácie je možné zadať aj priamo pri transformácií. Konkrétne sa jedná o číslovanie slidov a zmena fontu prezentácie.

+ Na transformáciu XML do HTML a FO som využil knižnicu **Saxon**, konkrétne *java -jar C:\Saxon\saxon9he.jar*.

+ Na transformáciu FO do PDF som využil DocBook, konkrétne *xep.bat*.

# Ukážky transformácií
### Načítanie externých parametrov a bat súbor pre HTML transformáciu:
```bat
@ECHO OFF

SET /A c=1
SET f="Arial" 

:Y1
IF "%1"=="" GOTO :END
IF "%1"=="cislovanie-0" SET /A c=0
IF "%1"=="font-times" SET f="Times New Roman"

SHIFT
GOTO :Y1

:END
call java -jar C:\Saxon\saxon9he.jar -s:prezentacia.xml -xsl:html.xsl cislovanie=%c% font=%f%
```
```xml
<xsl:param name="cislovanie" select="cislovanie"/>
<xsl:param name="font" select="font"/>
```

### Šablona na vytvorenie popisu obrázka zo základných transformácií :
```xml
<!-- Template pre nájdenie čísla obrázka v prezentácií -->
<xsl:template name="obr-cislo">
	<!-- zistí číslo obrázka -->
	<xsl:variable name="cislo" select="count(preceding::*//obrazok)+1"/>
 	<!-- nájde popis obrázka podľa toho o aký slide sa jedná -->
 	<xsl:if test=".[x:obrazok]">
		<xsl:variable name="popis" select="x:obrazok/x:popis"/>
		Obr.<xsl:value-of select="$cislo"/> : <xsl:value-of select="$popis"/>
	</xsl:if>
	<xsl:if test=".[x:mix]">
		<xsl:variable name="popis" select="x:mix/x:obrazok/x:popis"/>
		Obr.<xsl:value-of select="$cislo"/> : <xsl:value-of select="$popis"/>
	</xsl:if>
</xsl:template>
```

### Šablona pre vytvorenie automaticky generovaného slidu so zhrnutím pre HTML:
```xml
<!-- template pre spracovanie slidu so zhrnutím -->
<xsl:template name="zhrnutie">
	<div class="nadpis">
		<center>
			<h1>Zhrnutie</h1>
		</center>
	</div>
	<div class="obsah">
		<div class="text">
			<p>
				Kontakt: <xsl:value-of select="//x:metadata/x:email"/>
			</p>
			<ul>
			<xsl:for-each select="//x:key">
				<li>
					<xsl:apply-templates/>
				</li>
			</xsl:for-each>
			</ul>
		</div>
	</div>
</xsl:template>
```
