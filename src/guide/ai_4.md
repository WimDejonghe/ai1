---
mathjax:
  presets: '\def\lr#1#2#3{\left#1#2\right#3}'
---


# Micro:Bit Blokken


De procedure wordt hier besproken. Het leerproces binnen Teachable Machine is reeds gedaan. We vatten deze procedure aan net na het leerproces met zijn classificaties binnen Teachable Machine. Het getrainde NN zullen we hier bewaren in de cloud met een toegankelijke link.
Deze link wordt dan verder gebruikt binnen een website die in staat is om nieuwe webcam beelden aan te bieden aan het getrainde neuraal netwerk. En daarbij is de website in staat om de classificatie (output van het neurale netwerk) door te sturen naar een geconnecteerde Micro:Bit.

## Teachable Machine

![example image](./images/SL1.png "Teachable Machine")

Klik op Model trainen

![example image](./images/SL2.png "Teachable Machine")

Eénmaal getraind kan het model getest worden.

![example image](./images/SL3.png "Teachable Machine")

Eenmaal dit ok is, kan het exporteren plaatsvinden. Klik hiervoor op Model exporteren.
Nu wordt hier echter gekozen voor een deelbare link. Dwz dat het neuraal netwerk in de cloud zal worden bewaard en is bereikbaar via een internet link.

![example image](./images/SL4.png "Teachable Machine")

Kies voor Upload Mijn Model

![example image](./images/SL5.png "Teachable Machine")

Als de link ter beschikking is, klik op Kopiëren.

## Code Micro:Bit met blokken
Open in de browser een nieuw venster en surf naar de programmeeromgeving van Micro:Bit. En programmeer volgende scratch code:

![example image](./images/SL6.png "Teachable Machine")


:::warning
Let op bij de verschillende selecties naar de verschillende strings (omhoog, omlaag, niets). Vervang deze strings door de juiste naamclassificaties die je hebt gebruikt binnen teachable machine.
:::

Programmeer vorige code in de Micro:Bit.

Sluit de Browser en start die opnieuw op, en ga naar volgend linkadres:

[https://ai-training.glitch.me/](https://ai-training.glitch.me/)

![example image](./images/SL7.png "Teachable Machine")

Pair (verbinding maken) de Micro:bit door op de knop te klikken.

![example image](./images/SL8.png "Teachable Machine")

Plak de eerder gekopîeerde link vanuit teachable machine in dit venster.

Daarna zal via de webcam nieuwe beelden worden aangeboden aan het neuraal netwerk die op zijn beurt een communicatie met de Micro:Bit zal verzorgen door de namen van de verschillende classificaties door te sturen naar de Micro:Bit.

Voila, aan de slag ermee.
