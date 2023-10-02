---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---


# Python script zonder micro:bit

De werking van OpenCV en het neuraal netwerk kan getest worden adhv volgend script. Start binnen Anaconda Navigator de Spyder IDE op. Spyder is een python editor waarbinnen python code kan worden geschreven.

Het is hier niet de bedoeling om qua python kennis alles uit de doeken te doen. Een zeer beperkte bespreking van de werking van het script zal hier worden uitgevoerd. Bij verdere interesse in de werking van python en spyder wordt hier verwezen naar aparte opleiding hieromtrent.

:::warning
Zorg in eerste instantie dat je een aparte map hebt gemaakt op uw computer waar alle bestanden omtrent dit script in zullen worden verzameld. Hierin komen al zeker de twee bestanden die je uit het ZIP bestand hebt gehaald ("labels.txt", "keras_model.h5"). Zorg er ook voor dat het nieuwe aangemaakte py-bestand binnen Spyder ook in die map wordt bewaard.
:::

Geef het py-bestand een eigen naam en sla op. Let op, zoals eerder aangegeven, sla dit bestand zeker op in de map waar ook de gedownloade Teachable Machine bestanden zijn opgeslagen. 

```python
#Imports
import numpy as np
import cv2  # Bibliotheek om webcam te gebruiken
from keras.models import load_model # Bibliotheek om NN-bestand te gebruiken

cap = cv2.VideoCapture(0)  #Variabele om webcam beeld in op te slaan

model = load_model('keras_model.h5') #Variabele waarin NN-bestand wordt gelezen

while True:
    succes, image = cap.read() #beeld opvragen aan Webcam
    if succes == True:  #indien beeld is verkregen van de webcam
        
        cv2.imshow("Frame",image) #Toon het beeld in een venster
        img=cv2.resize(image,(224,224)) #Beeld wat aanpassen
        img=np.array(img,dtype=np.float32) #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        img=np.expand_dims(img, axis=0) #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        img=img/255 #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        prediction = model.predict(img) #aangepaste beeld data aanbieden aan NN en opvangen output NN in variabele
        predicted_class=np.argmax(prediction[0], axis=-1) #variabele laden met klasse (hoogste kans) uit vorige variabele 
        
        print(predicted_class) #printen naar console van klasse met hoogste kans
   
    if cv2.waitKey(1) & 0xFF == ord('q'): #programma stoppen door q te drukken na selectie webcam venster
        
        break
    
cap.release()
cv2.destroyAllWindows()
```

Met dit script, kan al onmiddelijk worden getest. In de console van Spyder, waar printcommando's hun visualisatie hebben, kan het resultaat geïnterpreteerd worden. Onderaan het script kan men zien dat met de toets q het script kan stoppen.

***
We bemerken, hier in dit voorbeeld, dat er 3 klasses zijn. De klasse met de hoogste waarschijnlijkheid wordt geselecteerd en wordt in de variabele "predicted_class" weggeschreven.


## Python script met micro:bit

Eenmaal vorige werkt, kunnen we vanuit het python script na de predict, het resultaat doorsturen naar de micro:bit. Dit doen we door een uniek karakter door te sturen op basis van het predict resultaat van het NN. Afhankelijk van het aantal klasses doen we dit hier met een '0' of '1' of '2' door te sturen naar de micro:bit. Dit doen we via een serieële verbinding via de USB kabel waarmee de micro:bit is verbonden met de computer waarop het NN draait. 

:::tip
Om dit te kunnen doen moeten we de **COM-poort nummer** kennen waarmee de Micro:Bit is verbonden met de computer. Dit kan telkens anders zijn!!
Dit kan je controleren via het Configuratie scherm van de computer. (Hardware en geluiden => Apparaatbeheer => Poorten (COM&LPT)).
:::

In volgend voorbeelden is de Micro:Bit verbonden via COM5.

### Micro:Bit code

Natuurlijk moet er dan nog op de Micro:Bit ook code draaien die deze karakters kan binnenlezen en op basis daarvan worden specifieke LED's op de ledmatrix van de Micro:Bit aangestuurd. (theorie en praktijk omtrent de Micro:Bit, zie gedeelte micro:bit).

De Micro:Bit code ziet er zo uit:

```python
# Imports go at the top
from microbit import *

while True:     #oneindige loop maken
    print('Nummer(0,1,2)? + Enter:')
    name = input()  #een binnengekomen karakter, via de USB-kabel, opslaan in variabele name
                    #variabele name is van het type char
    if name == '0': #is binnengekomen char een 0?
        display.clear() #ja, dan clear alle leds
        display.show(0) #toon 0 op Ledmatrix
        
    if name == '1': #is binnengekomen char een 1?
        display.clear()#ja, dan clear alle leds
        display.show(1) #toon 1 op Ledmatrix
        
    if name == '2': #is binnengekomen char een 2?
        display.clear() #ja, dan clear alle leds
        display.show(2) #toon 2 op Ledmatrix
        
```

Om vorige code beter te begrijpen kunnen we binnen de Micro:Bit Python programmeeromgeving dit al eens testen. Binnen die programmeeromgeving zit namelijk een terminal programma. Dit is eigenlijk een programma die op de laptop draait en kan communiceren via COM-poorten (bidirectioneel). Eenmaal de vorige code gedownload is in de Micro:Bit (dan voert deze het programma onmiddelijk uit), staat de Micro:Bit te wachten tot er een karakter via de USB kabel binnenkomt. 

Open dan het terminal venster :

![example image](./images/mb_term.png "Terminal venster van Micro:Bit")

Klik nu in het zwarte kader en geef een getal (0, 1 of 2) in via het toetsenbord. Er gebeurt niets tot je op ENTER drukt. Dan zal de respectievelijke LED oplichten. 

:::tip
Er moet dus een ENTER volgen op een doorgestuurd karakter!!!
:::

Nu zal dit getal moeten afkomstig zijn van het NN (predicted_class) via een Spyder python script.

:::warning
Sluit nu zeker het online programma van Micro:Bit af. Want een COM-poort kan slechts één keer door een programma worden gebruikt!!!
:::

Om dit al eens te testen binnen Syder zonder het NN kan volgend nieuw python script worden geschreven. Probeer dit te begrijpen:

```python
import serial  # Bibliotheek om communicatie te maken met de Micro:Bit

ser=serial.Serial('COM5',baudrate=115200) #instellingen voor de communicatie met de Micro:Bit

ser.write(bytearray('0\r\n','ascii')) #stuur een 0 naar de Micro:Bit

ser.close() #Sluiten van de seriële poort!!!

```
:::tip
Kijk naar de Micro:Bit. Wat zie je? Welke LED licht op? Pas bovenstaande code aan zodat een andere LED oplicht.
:::

:::warning
Let op het statement ser.close(). Dit is noodzakelijk omdat door het schrijven naar de COM-poort, die poort wordt geopend. Echter kan die slechts éénmaal worden geopend. Indien dit statement er niet zou staan, dan zou het programma de eerste keer werken, maar de volgende keren niet meer omdat de poort nog geopend staat. Oplossing is dan de Micro:Bit te disconnecteren en terug te connecteren via de USB kabel in en uit te trekken.
:::


### Laptop Python code

Het python script op de computer ziet er dan als volgt uit en is bijna hetzelfde als het vorige script. Enkel zijn er nu commando's bijgekomen die de communicatie verzorgen met de Micro:Bit.

:::warning
Let op, de manier om dit programma te beëindigen is van groot belang. Sluit het programma steeds af door in het venster met het webcam beeld de toets 'q' in te drukken!!! Hierdoor wordt ook de COM-poort met de Micro:Bit terug gesloten, zodat de volgende uitvoer van het programma de poort terug kan worden geopend, zie ook vorige Warning.
:::


```python
#Imports
import numpy as np
import cv2  # Bibliotheek om webcam te gebruiken
from keras.models import load_model # Bibliotheek om NN-bestand te gebruiken

import serial  # Bibliotheek om communicatie te maken met de Micro:Bit

cap = cv2.VideoCapture(0)  #Variabele om webcam beeld in op te slaan

model = load_model('keras_model.h5') #Variabele waarin NN-bestand wordt gelezen
ser=serial.Serial('COM5',baudrate=115200) #instellingen voor de communicatie met de Micro:Bit
while True:
    succes, image = cap.read() #beeld opvragen aan Webcam
    if succes == True:  #indien beeld is verkregen van de webcam
        
        cv2.imshow("Frame",image) #Toon het beeld in een venster
        img=cv2.resize(image,(224,224)) #Beeld wat aanpassen
        img=np.array(img,dtype=np.float32) #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        img=np.expand_dims(img, axis=0) #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        img=img/255 #Beeld data omvormen naar een formaat die NN-bestand kan lezen
        prediction = model.predict(img) #aangepaste beeld data aanbieden aan NN en opvangen output NN in variabele
        predicted_class=np.argmax(prediction[0], axis=-1) #variabele laden met klasse (hoogste kans) uit vorige variabele 
        
        print(predicted_class) #printen naar console van klasse met hoogste kans
        if predicted_class == 0: #is die klasse 0? 
            ser.write(bytearray('0\r\n','ascii')) #ja, stuur dan een 0 naar de Micro:Bit
        if predicted_class == 1: #is die klasse 1? 
            ser.write(bytearray('1\r\n','ascii')) #ja, stuur dan een 1 naar de Micro:Bit
        if predicted_class == 2: #is die klasse 2? 
            ser.write(bytearray('2\r\n','ascii')) #ja, stuur dan een 2 naar de Micro:Bit
            
        
    if cv2.waitKey(1) & 0xFF == ord('q'): #programma stoppen door q te drukken na selectie webcam venster
        ser.close() #sluiten van de verbinding met de Micro:Bit
        break
    
cap.release()
cv2.destroyAllWindows()
```






<hr>

Voila, dit is de volledige werking van AI Powered STEM
	
***
