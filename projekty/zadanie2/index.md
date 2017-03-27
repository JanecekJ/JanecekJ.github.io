---
layout: post
title: Zadanie 2 - Transformácia vybraného dokumentu do formátu DocBook
categories: Webové publikovanie
---
Predmetom 2. zadania je spracovanie vybraného dokumentu (ideálne bakalárskeho projektu) z pôvodného ľubovoľného (Word, OpenOffice, LaTeX, …) formátu do formátu DocBook a vygenerovanie cieľového tvaru v PDF. Výsledný dokument bude mať rozsah minimálne 10 a maximálne 15 strán. Do rozsahu sa nezapočítavajú úvodné strany (obsah, zoznamy obrázkov a tabuliek), použitá literatúra a prílohy.

# Riešenie

Vypracované zadanie obsahuje všetky požadované kontrolné konštrukcie. 

+ Text je členený na kapitoly a podkapitoly. Obsahuje tiež prílohu a generovaný obsah (obsah, zoznam obrázkov, zoznam tabuliek)
+ Na členenie textu som použil odrážky aj číslovanie, pričom pri číslovaní som zároveň využil aj zvýraznenie textu
+ V texte sa odkazujem na ďalšie časti dokumentu, konkrétne podkapitoly
+ Poznámka pod čiarou je použitá pri niektorých obrázkoch, kde pridáva menej zaujímavé informácie.
+ Zoznam použitej literatúry sa nachádza na konci dokumentu, a citácie sa nachádzajú na viacerých miestach dokumentu.
+ V dokumente sa nachádzajú obrázky a tabulky, na ktoré sa odkazujem v texte. Na začiatku dokumentu sa nachádzajú zoznamy tabuliek a obrázkov.
+ Na konci dokumentu sa nachádza viacúrovňový register pojmov.

Kapitoly a podkapitoly som vytváral pomocou tagov:
```xml
<chapter>
...
<section>...</section>
...
</chapter>
```

Na vygenerovanie zoznamu obrazkov a tabuliek som pozmenil subor **thesis.xsl** :
```xml
<xsl:param name="generate.toc">
book      title,toc,figure,table
</xsl:param>
```

Na vytvorenie číslovania so zvýraznením som použil:
```xml
<orderedlist>
	<listitem>
		<para>
			<emphasis role='bold' >čistenie dát</emphasis>
		</para>
	</listitem>
	<listitem>
		<para>
			<emphasis role='bold' >normalizácia</emphasis>
		</para>
	</listitem>
	<listitem>
		<para>
			<emphasis role='bold' >transformácia</emphasis>
		</para>
	</listitem>
	<listitem>
		<para>
			<emphasis role='bold' >extrakcia atribútov</emphasis>
		</para>
	</listitem>
	<listitem>
		<para>
			<emphasis role='bold' >selekcia atribútov</emphasis>
		</para>
	</listitem>
</orderedlist>
```

Odkaz na podkapitolu:
```xml
Tomu prečo je tento spôsob spracovania užitočný sa venujme v <xref linkend='variance'/>
...
<section id='variance'>
```

Obrázok s poznámkou pod čiarou:
```xml
<figure id="imgBiasVariance">
	<title>
		Vzťah medzi bias, variance a komplexnosťou modelu 
		<footnote>
			<para>Čím komplikovanejší model, tým väčšia variancia.
			 Čím jednoduchší model, tým väčšia vyvhýlenosť. Treba nájsť správny pomer.</para>
		</footnote>
	</title>
	<mediaobject>
	  	<imageobject>
	   	 	<imagedata width='10cm' fileref="images/biasvariance.png"/>
	  	</imageobject>
	  	<textobject>
	  		<phrase>Čím komplikovanejší model, tým väčšia variancia.
	  		 Čím jednoduchší model, tým väčšia vyvhýlenosť. Treba nájsť správny pomer.</phrase>
	  	</textobject>
	  	<ulink url="http://scott.fortmann-roe.com/docs/BiasVariance.html"/>	
	</mediaobject>
</figure>
```

Tabuľka:
```xml
<table id='tabulkaBagging' frame='all'><title>Tréningové podmnožiny pre bagging</title>
	<tgroup cols='2' align='left' colsep='1' rowsep='1'>
		<colspec colname='mnozina'/>
		<colspec colname='prvky' />
		<thead>
			<row>
	  			<entry>Množina</entry>
	  			<entry>Pozorovanie v množine</entry>
			</row>
		</thead>
		<tbody>
			<row>
			  	<entry>T (pôvodná množina)</entry>
			  	<entry>1, 2, 3, 4, 5, 6, 7, 8, 9, 10</entry>
			</row>
			<row>
			  	<entry>T1</entry>
			  	<entry>1, 2, 2, 4, 5, 6, 6, 8, 9, 10</entry>
			</row>
			<row>
			  	<entry>T2</entry>
			  	<entry>1, 1, 3, 4, 5, 5, 7, 8, 10, 10</entry>
			</row>
			<row>
			  	<entry>T3</entry>
			  	<entry>3, 3, 3, 4, 5, 6, 7, 7, 9, 10</entry>
			</row>
			<row>
			  	<entry>T4</entry>
			  	<entry>1, 2, 4, 4, 5, 6, 7, 8, 9, 9</entry>
			</row>
		</tbody>
	</tgroup>
</table>
```

Položka v použitej literatúre:
```xml
<biblioentry id="Breiman1996">
  	<authorgroup>
    	<author><firstname>Leo</firstname><surname>Breiman</surname></author>
  	</authorgroup>
  	<isbn>1573-0565</isbn>
  	<copyright>
  		<year>1996</year>
  	</copyright>
  	<publisher>
   		<publishername>Machine Learning</publishername>
  	</publisher>
  	<title>Bagging predictors</title>
  	<pagenums>123-140</pagenums>
</biblioentry>
```

Zoznam pojmov 
```xml
<!-- prvá uroveň -->
<envar>Klasifikácia</envar><indexterm><primary>Klasifikácia</primary></indexterm>
<!-- druhá uroveň -->
<envar>Predpríprava dát</envar><indexterm><primary>Klasifikácia</primary><secondary>Predpríprava dát</secondary></indexterm>
<!-- vygenerovanie zoznamu -->
<index></index>
```