# Un seul ESP32 pour deux BMS JK dans Home Assistant

La plupart des tutos utilisent un ESP32 par batterie. Ici, **un seul ESP32** se connecte en Bluetooth à **deux BMS JK** en même temps et envoie tout à Home Assistant par wifi : tension des 2 × 16 cellules, résistance de chaque cellule, températures, courant, puissance, état de charge, cycles, alarmes…

Vidéo pas à pas (chaîne **Alf Fait Maison**) : *lien à venir*

## Matériel
- 1 × **ESP32-WROOM-32U**, la version **avec antenne externe** (prise IPEX / u.FL), livrée avec son antenne.
  ⚠️ Bien prendre la **32U**, pas la 32D ou 32E à antenne intégrée : la portée change tout si les batteries sont loin de la box, et le boîtier 3D est prévu pour elle.
- 2 × BMS JK (testé avec JK_PB2A16S20P, matériel V19A, protocole `JK02_32S`)
- 1 chargeur USB pour alimenter l'ESP32
- En option : le boîtier imprimé en 3D (dossier `boitier_3d`)

## Installation rapide
1. Installe **ESPHome Device Builder** dans Home Assistant (Paramètres > Modules complémentaires > Boutique).
2. Crée un nouvel appareil ESP32, puis clique sur **Modifier** et colle le contenu de `esp32_2_jk_bms.yaml`.
3. Remplace `XX:XX:XX:XX:XX:01` et `XX:XX:XX:XX:XX:02` par les adresses Bluetooth de tes deux BMS (visibles avec l'appli gratuite nRF Connect : appareils dont le nom commence par JK).
4. Renseigne tes secrets (voir `secrets.exemple.yaml`).
5. **Installer** > Brancher sur cet ordinateur pour la première fois, puis les mises à jour se font sans fil.

## Ce qui rend deux batteries possibles
- Deux `ble_client` (client0, client1) et deux `jk_bms_ble` (bms0, bms1).
- Chaque capteur est déclaré deux fois ; ceux de la batterie 2 sont préfixés **B2**.
- Surveillance : si un BMS est hors ligne 3 minutes, sa connexion Bluetooth est relancée automatiquement.
- Capteurs « signal Bluetooth » de chaque BMS pour placer l'antenne (au-dessus de −85 dBm, c'est bon).

## Boîtier imprimé en 3D
`boitier_3d/WiRex-Bottom-plus5mm.stl` et `WiRex-Top-plus5mm.stl` : version **rallongée de 5 mm** du boîtier
« Boîtier ESP32-WROOM-32U » de **Loiseau créatif** : https://www.printables.com/model/762529-esp32-wroom-32u-casing
Licence **CC BY-NC-SA 4.0** (attribution, pas d'usage commercial, partage dans les mêmes conditions), comme l'original.

## Crédits
- Composant ESPHome : [syssi/esphome-jk-bms](https://github.com/syssi/esphome-jk-bms)
- Inspiration : vidéo 126 du Profes'Solaire, « JK BMS dans Home Assistant en Bluetooth » : https://www.youtube.com/watch?v=ZjxKqJwwoc8
- Boîtier d'origine : Loiseau créatif (Printables)

## Licence / License
- `esp32_2_jk_bms.yaml` : Apache 2.0 (voir / see `LICENSE`), version modifiée de / modified version of syssi/esphome-jk-bms `esp32-ble-example.yaml`.
- `boitier_3d/` : CC BY-NC-SA 4.0 (voir / see `boitier_3d/LICENCE.txt`).
