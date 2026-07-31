# Transparante afbeeldingen maken voor Unity UI

## De vraag

Waarom lukt het ChatGPT soms wel en soms niet om afbeeldingen met echte transparantie te maken, en welke instructie moet ik geven voor een betrouwbare Unity-ready sprite met alpha-kanaal?

## Wat is echte transparantie?

Een gewone afbeelding gebruikt drie kleurkanalen: rood, groen en blauw. Een transparante afbeelding heeft daarnaast een **alpha-kanaal**.

- `Alpha 255`: volledig zichtbaar
- `Alpha 0`: volledig transparant
- Tussenwaarden: gedeeltelijk transparant

Voor een Unity UI-sprite moet de lege achtergrond werkelijk uit pixels met `alpha 0` bestaan.

## Waarom gaat het soms fout?

Wanneer je alleen vraagt om een “transparante achtergrond”, kan een beeldgenerator:

- een echte transparante PNG maken;
- een witte of gekleurde achtergrond maken;
- een schaakbordpatroon tekenen;
- transparantie alleen visueel nabootsen.

Een schaakbordpatroon in de afbeelding is geen echte transparantie. Echte transparantie vereist een PNG-bestand met een alpha-kanaal.

Een duidelijke prompt helpt, maar het gebruikte beeldmodel moet transparantie technisch ook ondersteunen.

## Aanbevolen prompt

> Maak een vrijstaande Unity UI-sprite als PNG met echte native alpha-transparantie. Gebruik RGBA. Alle achtergrondpixels moeten alpha 0 zijn. Geen witte, zwarte of gekleurde achtergrond en geen getekend schaakbordpatroon. Plaats alleen het icoon gecentreerd in beeld, met transparante marge rondom. Geen kader, tegel, scène of achtergrondschaduw.

## Korte afgesproken prompt

> Maak dit als een Unity-ready transparante sprite.

Daarmee wordt bedoeld:

- PNG met RGBA-kanalen;
- echte alpha-transparantie;
- achtergrond volledig `alpha 0`;
- geen achtergrondkleur;
- geen getekend schaakbord;
- alleen het icoon of object;
- transparante marge rondom;
- geschikt voor Unity UI.

## Voorbeeld

> Maak een strak wit lijnicoon van een filmcamera gecombineerd met een microfoon. Maak het als een Unity-ready transparante sprite. Gebruik een vierkante RGBA-PNG. Alle achtergrondpixels moeten alpha 0 zijn. Geen achtergrond, kader, tegel, schaduw of schaakbordpatroon.

## Controleren in GIMP

Een correct transparant bestand:

- toont in GIMP een schaakbord achter het object;
- heeft geen zichtbare achtergrondlaag;
- heeft transparante hoekpixels;
- bevat een alpha-kanaal.

Het schaakbord van GIMP is alleen de weergave van transparantie. Een schaakbord dat onderdeel is van de afbeelding is fout.

## Unity-importinstellingen

Selecteer de PNG in Unity en gebruik:

- **Texture Type:** `Sprite (2D and UI)`
- **Sprite Mode:** `Single`
- **Alpha Source:** `Input Texture Alpha`
- **Alpha Is Transparency:** aan
- **Generate Mip Maps:** meestal uit voor UI-iconen

Klik daarna op **Apply**.

## Belangrijkste instructie

> Maak een RGBA-PNG met echte alpha-transparantie. Alle achtergrondpixels moeten alpha 0 zijn. Teken geen achtergrond of schaakbordpatroon.