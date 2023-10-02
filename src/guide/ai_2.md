---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---

# Hands on AI in STEM

## Intro

Hier zal gebruik gemaakt worden van AI om een servomotor op de micro:bit aan te sturen. Er zal eerst een neuraal netwerk getraind worden met beelden. Beelden die we verzamelen via de webcam van de computer. Hoe meer beelden we nemen, hoe preciezer het neuraal netwerk zal werken. Binnen het neuraal netwerk (NN) zullen we een aantal klasse's opnemen (classificatie). Hoeveel klasses je nodig hebt hangt af van de toepassing. De aangeboden train beelden zullen we zelf toekennen aan een klasse. We werken hier dus met een **Supervised learning AI model** werken. De data geven we aan het NN en bij die data zeggen we (per beeld hier) het NN tot welk een klasse de data behoort. 

Het neuraal netwerk (NN) zal patronen zoeken en onderscheiden die specifiek zijn voor een bepaalde klasse.

Eenmaal het neuraal netwerk getraind is kunnen we dan een nieuw beeld aanbieden aan het NN die dan een beoordeling zal maken en een procentuele kanswaarde zal genereren voor iedere klasse. De hoogste procentuele waarde zal bepalend zijn om het beeld tot die klasse onder te brengen. Let wel het nieuwe beeld levert geen meerwaarde aan het leerproces van het NN. Indien er geen voldoening is door de gebruiker dringt zich een nieuwe leerfase op met betere/meer traindata. 

> Het is dus duidelijk dat we zelf het NN niet zullen bouwen, maar we gebruiken een leeg bestaand NN om met onze specifieke data dit NN te trainen volgens onze toepassing. Wil je een nieuwe toepassing, dan kan je dit opnieuw doen met andere data.

Eenmaal we tevreden zijn van het getrainde NN (we zullen het ook testen, wat ook een belangrijke stap is in het gebruik van AI) gaan we dit volledige NN downloaden als een bestand op onze computer. Aan dit bestand kunnen we dan nieuwe data aanbieden, waarop het NN dan zijn werk zal doen en een output (procentuele classificatie).

Om dit te kunnen doen zullen we een beetje moeten programmeren. En python is hiervoor uitstekend geschikt. In dit programma zullen we toelaten dat we nieuwe data kunnen binnenhalen (in ons geval webcam beelden) en dat we die nieuwe data kunnen aanbieden aan het bestand met daarin ons getraind NN. Dit zal dus een output generen volgens de klasses die we in het NN hebben opgenomen. Daaruit zullen we de klasse halen die de hoogste 'confidence score' bezit. 

Ons python programma zal dan ook in staat moeten zijn om te kunnen communiceren met de microcontroller. In ons geval de Micro:Bit. Het python programma kan dan een unieke waarde doorsturen naar de microcontroller. De microcontroller kan die waarde binnen lezen en interpreteren. Volgens de interpretatie kan de microcontroller dan een actuator aansturen, in ons geval zullen dit enkele led's zijn op de ledmatrix van de Micro:Bit. Andere actuatoren zijn dan natuurlijk ook mogelijk.


## Stappenplan

Om een overzicht te houden volgt hier een opsomming van de te nemen stappen:
<ol>
<li>beelden verzamelen om het NN te trainen = train data (via webcam)</li>
<li>beelden classificeren</li>
<li>NN trainen</li>
<li>NN downloaden als bestand</li>
<li>python script schrijven met volgend stappenplan: </li>
<ol>
    <li>NN bestand importeren en gebruiken</li>
    <li>nieuwe webcam beelden kan aanleveren aan NN</li>
    <li>output van NN kan lezen en interpreteren</li>
    <li>communiceren met een microcontroller</li>
    <li>output van NN doorsturen naar microcontroller</li>
</ol>
<li>microcontroller leest data binnen</li>
<li>microcontroller stuurt actuator aan</li>
</ol>

We bespreken in volgende hoofdstukken deze stappen in detail. We gaan hier een voorbeeld uitwerken waarbij we met 3 klasses zullen werken:

<ul>
<li>klasse1 : beelden met een duim omhoog</li>
<li>klasse2 : beelden met een duim omlaag</li>
<li>klasse0 : beelden zonder zichtbare duim (of iets die niet klasse1 en niet klasse2 is)</li>
</ul>

Op basis van die beelden (classificaties) willen we de positie van een servo motor aansturen in drie posities: 

<ul>
<li>klasse1 : Led2 aan</li>
<li>klasse2 : Led3 aan</li>
<li>klasse0 : Led1 aan</li>
</ul>

<hr>

## Beelden verzamelen om het NN te trainen = train data (via webcam)

Hiervoor gebruiken we een online platform van Google. Dit platform voorziet lege neurale netwerken die je met uw eigen data kan trainen. Nadien kan je ook testen of uw NN geschikt is voor gebruik. Het platform is 'Teachable Machine' en is te vinden op:

<https://teachablemachine.withgoogle.com/>

![example image](./images/tm0.png "Teachable Machine")

Je hebt hiervoor een Google-account nodig. Log met die account in op 'Teachable Machine'. Nu kan je kiezen wat voor soort train-, test- en verwerk data je wenst te gebruiken. 

Klik op de knop "Aan de slag"



## Beelden classificeren

De gemaakte beelden zijn onmiddelijk in drie klasses gestopt volgens inhoud van de foto's:

<ul>
<li>klasse1 : beelden met een duim omhoog</li>
<li>klasse2 : beelden met een duim omlaag</li>
<li>klasse0 : beelden zonder zichtbare duim (of iets die niet klasse1 en niet klasse2 is)</li>
</ul>

![example image](./images/tm1.png "Teachable Machine")

![example image](./images/tm2.png "Teachable Machine")

![example image](./images/tm3.png "Teachable Machine")

![example image](./images/tm4.png "Teachable Machine")

## Neuraal netwerk trainen

![example image](./images/tm5.png "Teachable Machine")

Na het train proces kan er op de website onmidellijk worden getest met nieuwe data (nieuwe beelden). Ga hier na of in alle omstandigheden het neuraal netwerk goed reageert en classificeert. Indien dit niet ok is kan er opnieuw worden getraind met nieuwe / extra data. 

![example image](./images/tm6.png "Teachable Machine")

Eenmaal dit goed bevonden, kan dit worden opgeslagen in uw Google account. Dit is handig als er achteraf toch nog nieuwe trainingsdata zou moeten bijkomen. Je kan verschillende van die getrainde NN op de website opslaan.


## Volledig getraind Neuraal Netwerk downloaden als bestand

Het volledige getraind neuraal netwerk kan nu als bestand worden gedownload. Let wel, niet alle getrainde fotodata is hierin opgeslagen. Wel de herkende patronen die het NN toelaten om er een classificatie van te maken. Vandaar dat dit bestand niet zo groot is als je op eerste zicht zou verwachten.

Klik op Model exporteren

![example image](./images/tm7.png "Teachable Machine")

Er zijn verschillende manieren om van dit NN een bestand te maken. Het Google model die hier gebruikt wordt is een Keras model van Tensorflow. Tensorflow is een Google onderdeel die zich specialiseert in AI. Op de website kunnen we dit NN als een keras model van Tensorflow downloaden. 

Klik hiervoor op het middelste tabblad "Tensorflow" en zet de radiobutton op Keras.

![example image](./images/tm8.png "Teachable Machine")

Klik nu op de knop "Mijn model downloaden". Teachable Machine zal het model converteren naar een Keras model en het inpakken in een ZIP bestand.

![example image](./images/tm9.png "Teachable Machine")

Dit levert een ZIP bestand (converted_keras.zip) op dat je op uw lokale computer kan bewaren en uitpakken.

![example image](./images/tm10.png "Teachable Machine")

In het ZIP bestand zitten twee bestanden:

![example image](./images/tm11.png "Teachable Machine")

Het ene bestand is het NN 'keras_model.h5' bestand. Het volledige getrainde NN zit vervat in dit bestand.
Het andere bestand "labels.txt" bevat enkel de namen van de klasses.


## Python script schrijven

Nu moet er op de lokale computer een Python script wordt geschreven die volgende zaken doet:

<ol>
    <li>NN bestand importeren en gebruiken</li>
    <li>nieuwe webcam beelden kan aanleveren aan NN</li>
    <li>output van NN kan lezen en interpreteren</li>
    <li>communiceren met een microcontroller</li>
    <li>output van NN doorsturen naar microcontroller</li>
</ol>

Hiervoor wordt best gebruik gemaakt van een Python IDE (ontwikkelomgeving). Er wordt hier gebruik gemaakt van Spyder. Dit wordt best geïnstalleerd onder het pakket van Anaconda.

![example image](./images/ana.png "Installer Anaconda")

Ga naar de website van Anaconda en download de installer van Anaconda. Na het downloaden voer de installer uit:

![example image](./images/ana1.png "Installer Anaconda")

Volg daarna standaard stappen (§zorg dat je Administrator rechten hebt op de computer!!):

![example image](./images/ana2.png "Installer Anaconda")

![example image](./images/ana3.png "Installer Anaconda")

![example image](./images/ana4.png "Installer Anaconda")

![example image](./images/ana5.png "Installer Anaconda")

![example image](./images/ana6.png "Installer Anaconda")
![example image](./images/ana6a.png "Installer Anaconda")

![example image](./images/ana7.png "Installer Anaconda")

![example image](./images/ana8.png "Installer Anaconda")

![example image](./images/ana9.png "Installer Anaconda")

Sluit de browser

Laat Anaconda Navigator opstarten

![example image](./images/ana9a.png "Installer Anaconda")

![example image](./images/ana9b.png "Installer Anaconda")

Kies voor No, don't show again

![example image](./images/ana9c.png "Installer Anaconda")
Het is niet nodig om een account te hebben. Sluit venster in het kruisje.

![example image](./images/ana9d.png "Installer Anaconda")

## Installatie van Tensorflow, OpenCV en PySerial python bibliotheken

Deze bibliotheek is nodig om het gedownloade NN van Teachable Machine te kunnen verwerken binnen een lokaal python omgeving (environment).
De installatie gaat als volgt: Klik op de CMD.exe Prompt binnen de Anaconda Navigator:

![example image](./images/ana9e.png "Installer Tensorflow")

![example image](./images/ana9f.png "Installer Tensorflow")

Op de prompt typen we de regel gevolgd door Enter:

```prompt
conda create -n tf tensorflow
```

![example image](./images/ana9g.png "Installer Tensorflow")

![example image](./images/ana9h.png "Installer Tensorflow")

![example image](./images/ana9i.png "Installer Tensorflow")

Type y en druk op Enter

![example image](./images/ana9j.png "Installer Tensorflow")

![example image](./images/ana9k.png "Installer Tensorflow")

![example image](./images/ana9l.png "Installer Tensorflow")

Type dan op de prompt gevolgd door Enter: 

```prompt
conda activate tf
```
![example image](./images/ana9m.png "Installer Tensorflow")

![example image](./images/ana9n.png "Installer Tensorflow")


:::warning
Tensorflow heeft nu binnen python een nieuw environment aangemaakt. Alle andere bibliotheken en ook het opstarten van Spyder moet nu binnen dit environment gebeuren. Je ziet nu trouwens dat uw prompt niet meer begint met base maar met tf, wat de naam van het environment is.
:::

Type dan op de prompt gevolgd door Enter: 

```prompt
pip install opencv-python
```

![example image](./images/ana9o.png "Installer Tensorflow")

![example image](./images/ana9p.png "Installer Tensorflow")

![example image](./images/ana9q.png "Installer Tensorflow")

Installeer nu nog eens volgende bibliotheek. Deze is nodig om later een communicatie te leggen met de microprocessor (verbonden met een USB kabel aan de computer)

```prompt
conda install -c anaconda pyserial
```

Bevestig tussendoor met een y.

***

Sluit het prompt venster en selecteer in de Anaconda Navigator het tf environment:

![example image](./images/ana9r.png "Installer Tensorflow")

Install Spyder onder de tf environment:

![example image](./images/ana9s.png "Installer Spyder")

![example image](./images/ana9t.png "Installer Spyder")

En Launch Spyder na installatie na opnieuw het sluiten van het update venster.

![example image](./images/ana9u.png "Installer Spyder")

![example image](./images/ana9v.png "Installer Spyder")

Sluit venstertje met Tour optie


Voila, de computer is klaar om een python script, met daarin het gebruik van een Keras Tensorflow AI Neuraal Netwerk bestand en via Opencv kan het script de webcam gebruiken, te schrijven en te testen. De bibliotheek PySerial is nodig om verder een communicatie te verzorgen tussen de computer en de Micro:Bit.

