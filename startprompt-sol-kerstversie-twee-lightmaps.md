# Startprompt — kerstversie passend maken in LM1 en LM2

## Zo gebruik je deze prompt

Geef Sol dit bestand samen met `kerstversie-lm1-lm2-analyse.md` uit dezelfde map. Open de juiste Blender-scene en verbind de Blender-koppeling. GPT-6.1 Sol Medium is een praktische start voor deze afgebakende opdracht. Beoordeel het resultaat aan de controles hieronder.

## Opdracht om te kopiëren

Werk in mijn open Blender-bestand aan de kerstversie van Movie Theater. Lees eerst het bijgevoegde analysedocument volledig en controleer daarna de live toestand.

Het bestaande licht in Unity ziet er prachtig uit. Ik wil alle onderdelen van de kerstversie met passende lightmap-texeldensity binnen precies **twee atlasgroepen, LM1 en LM2**, krijgen. Gebruik de gemeten bestaande verdeling als kwaliteitsreferentie.

Mijn bestand hoort bij `E:/Projects/Movie_Theater/Blend/LIGHTMAPS/Christmas Version/movietheater_export.blend`. Textuur-UV is `UVMap` en lightmap-UV is `UVMap.001`. Controleer bestand en UV-laag voordat je begint.

### 1. Inventarisatie en herstelpunt

- Werk binnen LM1 en LM2, inclusief hun bedoelde subcollecties en kerstobjecten.
- Mijn acht Kentia-objecten en `Paper_Tablet_stand` zijn voor deze versie uitgezet. Sluit die expliciet uit van de UV-opdracht en vermeld ze in de exportlijst.
- Controleer objectidentiteit, gekoppelde meshdata, eenheden, wereldtransformaties, modifiers en de fysieke maat na export naar Unity.
- Leg de actuele groepsindeling en begintoestand vast. Maak een nieuwe werkkopie plus een herstelbare UV1-snapshot voordat je schaalt of pakt.
- Behoud UV0, meshgeometrie, materialen, bestaande bakehulpmiddelen en lichtinstellingen.
- Controleer of de bekende nuloppervlakte-UV's in de kerstboomobjecten nog bestaan en beoordeel waarvoor die vlakken dienen.

### 2. Meten en een concrete verdeling kiezen

Meet de UV- en wereldoppervlakte per eiland, plus de pixeldichtheid en smalle afmetingen. Controleer de effectieve resoluties. De geverifieerde projectbasis is 2048 voor kleur en 1024 voor richting.

Gebruik eerst de huidige dichtheden van de bestaande bioscooponderdelen. Probeer de huidige kerstschalen door de beschikbare ruimte over beide groepen te verdelen. Het analysedocument beschrijft een kandidaat waarbij kerstobjecten naar LM2 gaan. Meet opnieuw of die kandidaat nog past bij de huidige toestand.

Geef een korte voortgangsmelding met het gekozen plan en voer de omkeerbare, benodigde Blender-stappen uit. Als je moet kiezen tussen een vast kwaliteitsdoel en passend krijgen binnen twee atlassen, leg die concrete keuze aan mij voor.

Behoud objecten per geheel geëxporteerde mesh/renderer in één atlasgroep. Houd een duidelijke lijst bij van gewijzigde groepstoewijzingen en de bijbehorende Unity/Bakery-aanpassingen per lichtmodus. Een Blender-collectiewijziging alleen is onvoldoende voor een correcte bake.

### 3. Texeldensity afstemmen

Gebruik deze berekening voor een vierkante atlas:

```text
D = resolutie × sqrt(UV-oppervlakte / wereldoppervlakte)
doel-UV-oppervlakte = wereldoppervlakte × (doeldichtheid / resolutie)²
uniforme schaalfactor = sqrt(doel-UV-oppervlakte / huidige UV-oppervlakte)
```

Laat de bekende kwaliteit van bestaande onderdelen leidend zijn. Beoordeel voor kerstonderdelen de zichtbaarheid, normale kijkafstand, fijne lichtovergangen en minimum pixeldekking per eiland. Gebruik de proefbereiken uit het rapport alleen wanneer gerichte aanpassing nodig blijkt.

Kleine schubben en dopjes kunnen door een minimum pixelgrootte een hogere texeldensity nodig hebben dan grote oppervlakken. Controleer ook de smalste bruikbare delen op de 1024-richtingskaart. Benoem welke dichtheid of pixelondergrens je kiest en waarom.

### 4. Pakken en selecteerbaar houden

Pak iedere groep afzonderlijk binnen haar eigen 0–1-tegel. Behoud de afgesproken schaal per eiland en controleer automatische schaalfuncties in de packer. Gebruik uniforme schaling voor eventuele gerichte dichtheidsaanpassingen.

Bepaal en noteer de tussenruimte en atlasrand in pixels én UV-eenheden, passend bij de effectieve resoluties en bakeketen. Controleer de betekenis van de gebruikte packer-marge.

De soorten moeten gemakkelijk opnieuw te selecteren zijn. Bewaar daarom hun selectie-informatie per object of eiland. De eindpacking mag een compactere ruimtelijke indeling krijgen dan de tijdelijke blokken.

### 5. Werk in korte, controleerbare stappen

Lees meshgegevens eenmaal per fase, bereken het plan vooraf en pas per object of korte batch toe. Houd Blender responsief. Gebruik actuele face-/loopgegevens. Controleer na een onderbreking eerst de toestand voordat je dezelfde bewerking herhaalt.

### 6. Eindcontrole en oplevering

Controleer en rapporteer:

- de inbegrepen en uitgesloten objecten, met precies twee atlasgroepen;
- alle bedoelde eilanden volledig binnen de eigen 0–1-tegel, met de afgesproken atlasrand;
- overlap binnen eilanden en tussen alle objecten van dezelfde atlas;
- daadwerkelijk gemeten tussenruimte en atlasranden;
- eindige UV-coördinaten, nuloppervlakten en smalle/problematische delen;
- dichtheden en pixeldekking vóór en na, inclusief de gekozen minimumgrootten;
- behoud van UV0, geometrie en uitgesloten objecten;
- gewijzigde groepstoewijzingen en de benodigde Unity/Bakery-mapping;
- de locatie van de nieuwe werkkopie en een herstelmogelijkheid.

Lever twee gecontroleerde UV-layouts en een korte uitleg van de gekozen verdeling. Markeer resterende beperkingen duidelijk.

Deze opdracht autoriseert de voorbereiding en uitvoering van de Blender-UV-indeling. Lever de export-/Unity-mapping op voor de volgende stap. De definitieve lichtkwaliteit beoordelen we daarna met een proefbak in Unity, in de bestaande drie lichtmodi en vanaf normale VR-kijkafstanden.

