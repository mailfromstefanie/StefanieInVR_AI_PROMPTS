# Kerstversie — analyse en overdracht voor LM1 en LM2

Meetdatum 10 november 2026. Dit rapport beschrijft de live Blender-toestand nadat Stef de objecten voor de gewone versie heeft uitgezet. Status: analyse en overdracht; een definitieve UV-pack en nieuwe proefbak zijn nog uit te voeren.

## Opdracht en kwaliteitsreferentie

Alle onderdelen die bij de kerstversie horen moeten passen in precies twee lightmapgroepen, **LM1 en LM2**. Stef vindt het bestaande gebakken licht in Unity prachtig en wil die kwaliteit als uitgangspunt gebruiken. De kerstspullen zijn per soort gesorteerd. Die organisatie helpt bij selecteren en beoordelen.

Het einddoel is een bruikbare lightmapverdeling met passende texeldensity, voldoende tussenruimte en een goede proefbak. De eerdere opdracht om alle krans-eilanden evenveel UV-oppervlakte te geven was een voorbereidingsstap. Een uniforme eilandoppervlakte geeft kleine onderdelen veel meer texels per meter dan grote onderdelen.

## Vastgestelde projecttoestand

- Blenderbestand: `E:/Projects/Movie_Theater/Blend/LIGHTMAPS/Christmas Version/movietheater_export.blend`.
- Unityproject: `E:/Projects/Movie_Theater/Unity/MOVIETHEATER`.
- Textuur-UV is `UVMap` (index 0). Lightmap-UV is `UVMap.001` (index 1). Controleer deze koppeling opnieuw vóór bewerkingen.
- Blender gebruikt metrische eenheden met schaal 1. De metingen gebruiken wereldtransformaties. Absolute texels per meter blijven afhankelijk van overeenkomende fysieke maat in Unity.
- LM1 bevat 46 meshes inclusief subcollecties. Nu zijn 37 zichtbaar, waarvan 13 kerstmeshes.
- LM2 bevat 56 meshes inclusief `LM2_TALK_MODE_SEATS`. Alle 56 zijn zichtbaar.
- `CHRISTMASTREE` zit momenteel onder LM1. `LM1_MIC` bevat `Control Box` en `Microphone Holder`.
- Lights On, Lights Off en Talk Show hebben elk de twee Bakery-groepen. Dat zijn twee atlasindelingen per lichtmodus. L0/L1 in een texturebestandsnaam zijn maptypen en mogen niet worden verward met de groepen LM1/LM2.

### Door Stef uitgezet voor deze versie

Deze negen meshes zijn momenteel via het oogje verborgen en horen volgens deze sessie buiten de kerstselectie:

- `Paper_Tablet_stand`
- `Kentia_leafs_1` t/m `Kentia_leafs_4`
- `Kentia_pots_and_steels_1` t/m `Kentia_pots_and_steels_4`

Hun render-vlag staat nog aan. Gebruik bovenstaande lijst als expliciete seizoensuitsluiting. Alleen een verborgen oogje garandeert geen uitsluiting uit FBX-export, Unity of de build. Maak vóór export een gecontroleerde objectlijst.

Andere verborgen collecties, zoals LIGHTMESHES, HELPERS en COLLIDERS, zijn geen toestemming om bakhulpmiddelen te verwijderen. Bakhulpmiddelen blijven deel van de bestaande bakewerkwijze en vallen buiten deze UV-opdracht.

### Opgeslagen Bakery-instellingen

De zes assets voor Group_1/Group_2 en de drie lichtmodi gebruiken:

- resolutie 2048;
- autoResolution uit;
- OriginalUV;
- identieke opgeslagen atlastransformaties (1, 1, 0, 0) in de gecontroleerde bakegegevens;
- L0 kleurimport op maximaal 2048 voor PC, Android en iPhone;
- L1 richtingimport op maximaal 1024 voor die platforms.

Dit is uit de bestanden op schijf vastgesteld. Live Unity-instellingen en de effectieve geïmporteerde afmetingen moeten vóór het eindresultaat worden gecontroleerd. In Original UV gebruikt Bakery de aangeleverde UV-layout. Een wijziging van UV-posities of atlasgroep vereist een bijpassende export en nieuwe bake.

Lokale bronnen, relatief aan het Unityproject:

- `Assets/Scenes/LMGroup_1 (Lights On).asset` en de overeenkomstige Group_2-, Lights Off- en Talk Show-assets.
- `Assets/Bakery/BakeryLightmapGroup.cs`, enum voor OriginalUV.
- `Assets/Scenes/MOVIETHEATER.unity`, Bakery-selectors en bakedScaleOffset.
- `Assets/BakeryLightmaps/LMGroup_2 (Lights On)_L0.hdr.meta` en `_L1.tga.meta`.

De Unity-stand-ins voor Control Box en Microphone Holder hebben eigen schaalinstellingen. Hun Blender-dichtheid is daarom nog geen geverifieerde runtime-dichtheid. Hun UV-ruimte hoort volgens de opgeslagen Unity-selectors wel bij Group_1.

## Metingen en betekenis

Voor een vierkante atlas:

```text
D = R × sqrt(Auv / Awereld)
Auv_doel = Awereld × (Ddoel / R)²
schaalfactor = sqrt(Auv_doel / Auv_huidig)
```

D is de lineaire texeldensity, R de effectieve atlasresolutie, Auv de som van UV-driehoeksoppervlakten en Awereld de oppervlakte in vierkante meters. Schaal een eiland uniform in beide UV-richtingen. Meet UV en wereldoppervlakte op dezelfde triangulatie.

Objectgemiddelden kunnen grote verschillen tussen vlakken verbergen. Controleer voor uitvoering per UV-eiland ook smalle stukken, lokale vervorming, pixeldekking en ondergrenzen. Een oppervlaktemaat alleen bewijst geen voldoende resolutie in beide richtingen.

De tabelwaarden hieronder zijn berekend bij 2048 kleurpixels. Bij 1024 richtingspixels halveert de lineaire dichtheid voor dezelfde UV-layout.

### Patroon in de bestaande bioscoop

| Referentie | Texels/m bij 2048, ongeveer |
|---|---:|
| Walls | 19 |
| Ceiling | 24 |
| Tapijt, gordijnen, frontpaneel | 30–31 |
| Houten stoelonderdelen | 36 |
| Stoelbekleding | 42–44 |
| floor_front | 48 |
| Deuren | 55 |
| Projector, speakers, wandlampen | 94–100 |
| Krijtbord | 144–146 |
| Lectern | 166 |
| Klok | 173–174 |
| Kleine schakelaars | veelal 260–445 |

De acht stoelgroepen hebben vrijwel gelijke dichtheden binnen hun soort. Dat is een bruikbare referentie voor herhaling.

De verklaring voor een klein blaadje met hogere dichtheid dan een muur is een werkhypothese: een klein eiland heeft genoeg pixels nodig om bruikbare lichtinformatie en randen over te houden, terwijl een groot vlak zachte verlichting over meer pixels kan verdelen. De benodigde lichtdetailfrequentie, kijkafstand, zichtbaarheid en minimum pixeldekking zijn samen bepalend. Alleen het materiaal of de naam van het onderdeel bepaalt geen ideale waarde.

### Huidige kerstkransen

| Onderdeel | Aantal 3D-onderdelen | Gemeten dichtheid, ongeveer |
|---|---:|---:|
| Rode ballen | 17 | 45–60 |
| Gestreepte ballen | 19 | 60–80 |
| Trommeltjes | 6 | 151 |
| Sterretjes | 55 | 165–221 |
| Ophangdopjes | 19 | 510–684 |
| Binnenkernen dennenappels | 31 | 254–459 |
| Dennenappelschubben | 639 | 215–609 |
| Warmwitte lampjes | 386 | 340–456 |
| Dennentakjes | 339 | 32–87 |

Dit zijn 3D-onderdelen. Eén onderdeel kan meerdere UV-eilanden hebben. De drie kransen bevatten samen 1640 UV-eilanden. Hun gezamenlijke vlakoppervlakte is 1,6105% van één atlas, exclusief tussenruimte.

De gelijke eilandoppervlakte van circa 0,00000982016 UV² komt overeen met circa 41 kleurtexels per eiland bij 2048, of een equivalent vierkant van circa 6,4 × 6,4 pixels. Bij 1024 is dat circa 3,2 × 3,2. Dit is een oppervlakteequivalent; langwerpige eilanden hebben andere afmetingen en kunnen plaatselijk te smal zijn.

## Beschikbaar budget na het uitzetten

| Situatie | Opgetelde UV-vlakoppervlakte |
|---|---:|
| LM1 bestaande zichtbare onderdelen, zonder kerstspullen | 74,1208% van één atlas |
| Kerstspullen, alle 13 meshes | 19,1500% |
| LM1 inclusief kerstspullen in huidige toewijzing | 93,2708% |
| LM2 bestaande zichtbare onderdelen | 61,6836% |
| Beide groepen samen | 154,9544% van twee keer 100% |

Dit zijn sommen van driehoeksoppervlakten. De getallen bewijzen geen overlapvrije bezetting en meten geen vrije aaneengesloten packruimte. Padding, eilandvormen, gaten, dunne delen, eventuele overlaps en objecten die per lichtmodus wisselen moeten afzonderlijk worden gecontroleerd.

**Eerste te onderzoeken kandidaat:** verdeel de kerstspullen over de twee groepen, voordat bestaande kwaliteit wordt verlaagd. Als alle 13 kerstmeshes naar LM2 zouden gaan, wordt de ruwe verdeling circa **74,12% / 80,83%**. Dit is een rekenscenario, geen uitgevoerde verplaatsing of bewezen packresultaat. Een gedeeltelijke verdeling kan praktischer zijn.

Wijs hele geëxporteerde meshes/renderers toe aan een groep. Controleer de Bakery-selectors in alle drie lichtmodi en eventuele expliciete overrides op kinderen. LM1 en LM2 mogen dezelfde 0–1-coördinaten gebruiken wanneer hun renderers in Unity aan twee afzonderlijke Bakery-atlasgroepen zijn toegewezen. Alleen twee Blender-collecties maken die atlaskoppeling niet. Een gedeeld object krijgt niet automatisch twee atlasindices doordat enkele eilanden in een andere collectie lijken te liggen.

## Uit te voeren werkwijze

1. Lees de live toestand opnieuw en leg een objectmanifest vast met groepsindeling, seizoensuitsluitingen, UV-laag en exportidentiteit. Houd rekening met linked meshdata en per-modus objecten.
2. Maak een nieuwe werkkopie en een herstelbare snapshot van UV1 en de groepsindeling. Leg ook controlesommen van UV0 en meshgeometrie vast.
3. Meet per eiland Awereld, Auv, dichtheid, korte afmetingen in pixels en geldige UV-coördinaten. Neem schaal, modifiers en de uiteindelijke Unity-maten mee.
4. Gebruik de bestaande verdeling als kwaliteitsreferentie. Probeer eerst de huidige kerstschalen met een betere verdeling over LM1/LM2. Positioneren en roteren zijn mogelijk, waarbij de afgesproken dichtheden behouden blijven.
5. Test de packing met vaste schaal en expliciete atlasgrenzen. Disable een eventuele automatische normalisatie of fit die alle eilanden stilzwijgend verkleint. Controleer de werkelijke UVPackmaster-instellingen en versie.
6. Als de eerste verdeling onvoldoende ruimte geeft, onderzoek gerichte aanpassingen aan kerstcategorieën. Toon per soort de oorspronkelijke en voorgestelde dichtheid, het ruimte-effect en de reden. Stel gevestigde bioscooponderdelen alleen bij als dat nodig is en maak kwaliteitsconcessies zichtbaar.
7. Bewaar de soortselecties als herbruikbare groepsinformatie. Tijdens de eindpacking mag de ruimtelijke blokindeling wijken voor efficiënte plaatsing, terwijl de onderdelen per soort selecteerbaar blijven. Gebruik een passende face- of eilandregistratie bij gedeelde vertices.
8. Valideer de eindlayout op afzonderlijke LM1- en LM2-atlassen. Rapporteer aantal eilanden, minimaal gemeten tussenruimte, dichtheden, wijzigingen en eventuele resterende twijfelgevallen.
9. Lever een mapping voor Unity/export op. De daadwerkelijke bake en VR-controle vormen een volgende controlefase met dezelfde bestaande lichtinstellingen.

### Schaalkeuzes als proefwaarden

Als gerichte aanpassing nodig blijkt, zijn 32–45 texels/m voor dennenvlakken, 60–80 voor ballen en 100–160 voor goed zichtbare sterren/trommeltjes bruikbare proefbereiken in deze wereld. Grote papieren sterren zitten momenteel rond 157–160 texels/m en gebruiken circa 6,48% van een atlas. Een test met 80–100 kan ruimte besparen, maar vereist een visuele vergelijking. Dit is geen opdracht om dat direct toe te passen.

Kleine dopjes, schubben en lampjes krijgen een pixelondergrens afgeleid van bruikbare bestaande details, het uiteindelijke bakresultaat en beide mapresoluties. Een hoge dichtheid op een klein onderdeel kan door die ondergrens terecht zijn. Vermijd een universeel doel voor alle soorten.

### Padding en controles

Leg vast of een ingestelde marge een afstand tussen eilanden of een rand per eiland betekent. Reken pixels om via de effectieve resolutie. Een tussenruimte van bijvoorbeeld 4 pixels op 1024 is 8 pixels op 2048, of 0,00390625 UV. Dit is een conversievoorbeeld, geen reeds gevalideerde projectinstelling. Kies de marge vanuit de bestaande bakeketen en controleer haar in de packuitvoer, inclusief de atlasrand.

Controleer overlaps tussen alle objecten binnen dezelfde groep en binnen ieder eiland, eindposities binnen 0–1, nuloppervlakten, self-overlaps, eindige coördinaten en dunne delen. Overlap tussen de twee afzonderlijke groepen is toegestaan omdat ze verschillende atlassen gebruiken.

Eerdere metingen vonden 108, 120 en 241 driehoeken met nagenoeg nul UV-oppervlakte in respectievelijk `kerstboom_stam_back_l`, `kerstboom_stam_back_r` en `kerstboom_stam_front`. Onderzoek de betrokken vlakken en hun zichtbaarheid voordat de UV's als bakeklaar worden gemeld. Dit is nog geen vastgestelde oorzaak of opdracht om geometrie te verwijderen.

### Blender responsief houden

De eerdere vastloper ontstond tijdens een grote mutatiepoging met veel herhaalde toegang tot edit-meshes. De precieze oorzaak is niet bewezen. Een latere batchbewerking per object werkte wel.

Lees elke mesh een keer per fase. Bereken een plan buiten de mutatielus, valideer de invoer vóór toepassen en werk per mesh of in korte batches. Gebruik een BMesh per object en update de mesh eenmaal per batch. Behoud een snapshot die ook na een onderbreking bruikbaar is. Baseer toekomstige stappen op nieuwe face-/loopgegevens; indices kunnen na meshwijzigingen veranderen. Een timeout is reden om de actuele toestand te controleren vóór een retry.

## Acceptatie en grenzen van deze analyse

Een technisch geslaagde eindindeling bevat precies twee afgesproken atlasgroepen, verwerkt alle inbegrepen meshes, heeft passende padding en meetbaar gecontroleerde UV's, en houdt de afgesproken kwaliteitsreferenties aan.

Een mooie bake is pas bevestigd na vergelijking in Unity en VR, bij de normale kijkafstanden en in Lights On, Lights Off en Talk Show. Controleer vooral ballen, fijne schubben, dunne takken, contactschaduwen en lichte randjes. Een zichtbaar patroon in de bestaande UV-verdeling bewijst op zichzelf geen optimale lichtkwaliteit.

Voor een andere wereld moeten schaal, resoluties, atlasbudget, objectcategorieën en proefwaarden opnieuw worden bepaald. Dit rapport bevat de projectgegevens voor deze kerstversie.

## Volledige meetlijst van de momentopname

Alle waarden hieronder gebruiken 2048 als rekenbasis en Blender-wereldoppervlakte. `Uit` verwijst naar de expliciete seizoensuitsluiting op de meetdatum. Het UV-percentage bevat de volledige mesh, ook wanneer deze nog buiten de 0–1-tegel ligt.

| Groep | Object | Meenemen | Wereldoppervlak m² | UV-vlak % | Texels/m |
|---|---|---|---:|---:|---:|
| LM1 | `EmergencyExitLight_case` | Ja | 0.7245 | 0.0883 | 71.5 |
| LM1 | `Beanbags` | Ja | 21.2842 | 0.6816 | 36.6 |
| LM1 | `carpet_floor_Fabric028` | Ja | 633.7966 | 14.5385 | 31.0 |
| LM1 | `Ceiling` | Ja | 743.3300 | 10.3575 | 24.2 |
| LM1 | `Curtains` | Ja | 85.0817 | 2.0011 | 31.4 |
| LM1 | `doors` | Ja | 20.6025 | 1.5038 | 55.3 |
| LM1 | `EmergencyExitLight_glass` | Ja | 0.2083 | 0.0367 | 85.9 |
| LM1 | `steps_2` | Ja | 34.1308 | 0.5524 | 26.1 |
| LM1 | `front_panel_Fabric028` | Ja | 307.4181 | 6.5081 | 29.8 |
| LM1 | `Walls` | Ja | 783.6944 | 6.7969 | 19.1 |
| LM1 | `Wooden_Steps` | Ja | 5.6311 | 0.7091 | 72.7 |
| LM1 | `PictureFrames` | Ja | 12.8063 | 0.1659 | 23.3 |
| LM1 | `Picture1` | Ja | 4.3073 | 0.1446 | 37.5 |
| LM1 | `Picture2` | Ja | 4.3200 | 0.1599 | 39.4 |
| LM1 | `floor_front` | Ja | 291.8230 | 16.1697 | 48.2 |
| LM1 | `steps_1` | Ja | 34.1308 | 0.5551 | 26.1 |
| LM1 | `Theater_Stage_Lamps` | Ja | 9.0913 | 1.2666 | 76.4 |
| LM1 | `Display` | Ja | 7.8329 | 1.1933 | 79.9 |
| LM1 | `Paper_Tablet_stand` | Uit | 0.1124 | 0.0283 | 102.8 |
| LM1 | `Concert Crate_SM_Wheels.001` | Ja | 12.9392 | 1.9718 | 79.9 |
| LM1 | `Concert Crate_SM_Wheels.002` | Ja | 11.3272 | 1.7277 | 80.0 |
| LM1 | `Hanging_lamp_lightbulb_holder_L` | Ja | 0.0452 | 0.0067 | 79.1 |
| LM1 | `Hanging_lamp_lightbulb_holder_R` | Ja | 0.0452 | 0.0067 | 79.1 |
| LM1 | `Kentia_leafs_1` | Uit | 2.0516 | 0.9576 | 139.9 |
| LM1 | `Kentia_leafs_2` | Uit | 2.0516 | 0.9576 | 139.9 |
| LM1 | `Kentia_leafs_3` | Uit | 2.0516 | 0.9576 | 139.9 |
| LM1 | `Kentia_leafs_4` | Uit | 2.0516 | 0.9576 | 139.9 |
| LM1 | `Kentia_pots_and_steels_1` | Uit | 1.9237 | 0.7305 | 126.2 |
| LM1 | `Kentia_pots_and_steels_2` | Uit | 1.9237 | 0.7305 | 126.2 |
| LM1 | `Kentia_pots_and_steels_3` | Uit | 1.9237 | 0.7305 | 126.2 |
| LM1 | `Kentia_pots_and_steels_4` | Uit | 1.9237 | 0.7305 | 126.2 |
| LM1 | `Microphone Holder` | Ja | 0.0425 | 0.8512 | 916.2 |
| LM1 | `Control Box` | Ja | 0.3062 | 6.1277 | 916.2 |
| LM1 | `pine_leafs_back_l` | Ja | 30.9153 | 0.7661 | 32.2 |
| LM1 | `kerstboom_stam_back_l` | Ja | 3.0765 | 1.1018 | 122.6 |
| LM1 | `pine_leafs_back_r` | Ja | 30.7935 | 0.7625 | 32.2 |
| LM1 | `kerstboom_stam_back_r` | Ja | 3.0765 | 1.0269 | 118.3 |
| LM1 | `pine_leafs_front_l` | Ja | 44.6536 | 1.0996 | 32.1 |
| LM1 | `pine_leafs_front_r` | Ja | 74.1187 | 1.8364 | 32.2 |
| LM1 | `stars_paper` | Ja | 11.0713 | 6.4833 | 156.7 |
| LM1 | `kerstkrans_deur_l` | Ja | 4.1567 | 0.6894 | 83.4 |
| LM1 | `kerststerren_front` | Ja | 5.1076 | 0.6402 | 72.5 |
| LM1 | `kerststerren_back` | Ja | 7.7172 | 0.8374 | 67.5 |
| LM1 | `kerstboom_stam_front` | Ja | 8.9283 | 2.9854 | 118.4 |
| LM1 | `kerstkransen_tafel` | Ja | 0.3566 | 0.1542 | 134.7 |
| LM1 | `kerstkransen_deur_r` | Ja | 4.3354 | 0.7670 | 86.1 |
| LM2 | `_ad_SCRIPT` | Ja | 0.7612 | 0.3095 | 130.6 |
| LM2 | `_Projector_round_ANIM_1` | Ja | 0.8878 | 0.1862 | 93.8 |
| LM2 | `_Projector_round_ANIM_2` | Ja | 0.3868 | 0.0825 | 94.6 |
| LM2 | `Chair_Fabric002_1` | Ja | 75.0497 | 3.4604 | 44.0 |
| LM2 | `Chair_Fabric002_2` | Ja | 79.6966 | 3.4420 | 42.6 |
| LM2 | `Chair_Fabric002_3` | Ja | 79.6966 | 3.4078 | 42.3 |
| LM2 | `Chair_Fabric002_4` | Ja | 75.0497 | 3.4537 | 43.9 |
| LM2 | `Chair_Fabric002_5` | Ja | 65.6500 | 2.9262 | 43.2 |
| LM2 | `Chair_Fabric002_6` | Ja | 65.6500 | 2.9355 | 43.3 |
| LM2 | `Chair_Fabric002_7` | Ja | 65.6500 | 2.9295 | 43.3 |
| LM2 | `Chair_Fabric002_8` | Ja | 65.6500 | 2.9426 | 43.4 |
| LM2 | `Chair_wood_1` | Ja | 50.6221 | 1.5447 | 35.8 |
| LM2 | `Chair_wood_2` | Ja | 53.8897 | 1.6457 | 35.8 |
| LM2 | `Chair_wood_3` | Ja | 53.8897 | 1.6428 | 35.8 |
| LM2 | `Chair_wood_4` | Ja | 50.6221 | 1.5471 | 35.8 |
| LM2 | `Chair_wood_5` | Ja | 44.3383 | 1.3537 | 35.8 |
| LM2 | `Chair_wood_6` | Ja | 44.3383 | 1.3571 | 35.8 |
| LM2 | `Chair_wood_7` | Ja | 44.3383 | 1.3494 | 35.7 |
| LM2 | `Chair_wood_8` | Ja | 44.3383 | 1.3498 | 35.7 |
| LM2 | `ChalkBillBoard_Back` | Ja | 1.6416 | 0.8281 | 145.5 |
| LM2 | `ChalkBillBoard_blackbchalk` | Ja | 1.5879 | 0.7816 | 143.7 |
| LM2 | `ChalkBillBoard_Frame` | Ja | 2.0042 | 1.0108 | 145.4 |
| LM2 | `ChalkBillBoard_lamp` | Ja | 0.0306 | 0.2178 | 546.1 |
| LM2 | `clock_face` | Ja | 0.2781 | 0.2012 | 174.2 |
| LM2 | `clock_frame` | Ja | 0.6383 | 0.4534 | 172.6 |
| LM2 | `frame_down` | Ja | 33.4674 | 0.1945 | 15.6 |
| LM2 | `frame_top` | Ja | 38.4475 | 0.1915 | 14.5 |
| LM2 | `lamps_pictures` | Ja | 0.2749 | 0.1540 | 153.3 |
| LM2 | `lamps_wall` | Ja | 7.7777 | 1.8402 | 99.6 |
| LM2 | `Paperlamps` | Ja | 21.4082 | 0.6653 | 36.1 |
| LM2 | `Projector_body` | Ja | 3.4451 | 0.7482 | 95.4 |
| LM2 | `Chair_frames_center` | Ja | 44.3529 | 0.3475 | 18.1 |
| LM2 | `Chair_frames_left` | Ja | 18.4308 | 0.8212 | 43.2 |
| LM2 | `Chair_frames_right` | Ja | 18.4097 | 0.8321 | 43.5 |
| LM2 | `curtain-hinge` | Ja | 4.4007 | 0.1131 | 32.8 |
| LM2 | `doorhandles` | Ja | 0.1519 | 0.1341 | 192.4 |
| LM2 | `Lectern` | Ja | 4.3666 | 2.8838 | 166.4 |
| LM2 | `Lectern_Lamp` | Ja | 0.0306 | 0.0619 | 291.2 |
| LM2 | `Speakers` | Ja | 12.4388 | 2.9353 | 99.5 |
| LM2 | `lamps_ceiling_back` | Ja | 1.6774 | 0.0420 | 32.4 |
| LM2 | `lamps_ceiling_front` | Ja | 2.3484 | 0.0588 | 32.4 |
| LM2 | `wires` | Ja | 0.4527 | 0.0690 | 80.0 |
| LM2 | `truss` | Ja | 63.3832 | 1.8265 | 34.8 |
| LM2 | `Hanging_Lamps_L` | Ja | 1.4245 | 0.1205 | 59.6 |
| LM2 | `Hanging_Lamps_R` | Ja | 1.4245 | 0.1205 | 59.6 |
| LM2 | `Lamps_Podium_Ground` | Ja | 0.2761 | 0.6465 | 313.4 |
| LM2 | `Ropes` | Ja | 3.4199 | 0.5214 | 80.0 |
| LM2 | `switch_base` | Ja | 0.1507 | 0.4751 | 363.6 |
| LM2 | `switch_cases` | Ja | 0.1294 | 0.4035 | 361.7 |
| LM2 | `switch_mode_TALK` | Ja | 0.0047 | 0.0162 | 379.0 |
| LM2 | `switch_mode_OFF` | Ja | 0.0047 | 0.0162 | 379.1 |
| LM2 | `switch_mode_ON` | Ja | 0.0047 | 0.0163 | 379.6 |
| LM2 | `switch_wire` | Ja | 0.0354 | 0.1669 | 444.7 |
| LM2 | `switch_Lamp` | Ja | 0.0377 | 0.0606 | 259.7 |
| LM2 | `talkmode_seats` | Ja | 34.5525 | 2.1955 | 51.6 |
| LM2 | `talkmode_wood` | Ja | 3.5881 | 1.6461 | 138.7 |

## Externe bron

De betekenis van Original UV en de Bakery-groepsresolutie is gecontroleerd in de officiële handleiding. [Bakery Lightmap Group Selector](https://geom.io/bakery/wiki/index.php?title=Manual#Bakery_Lightmap_Group_Selector)

