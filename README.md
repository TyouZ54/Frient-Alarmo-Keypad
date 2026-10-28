# Frient Keypad with Alarmo for Home Assistant (+ Tamper)

🇬🇧 [English](#-english) · 🇫🇷 [Français](#-français)

> **Fork notice / Avis de fork**
> This project is a fork of [Bygood91/frient_keypad_alarmo](https://github.com/Bygood91/frient_keypad_alarmo). All credit for the original blueprint goes to **Bygood91**.
> Ce projet est un fork de [Bygood91/frient_keypad_alarmo](https://github.com/Bygood91/frient_keypad_alarmo). Tout le mérite du blueprint d'origine revient à **Bygood91**.

---

# 🇬🇧 English

This project provides automation to link a **Frient Keypad** to the **Alarmo** module (or any alarm control panel) in Home Assistant, through **Zigbee2MQTT**. It enables seamless communication between the keypad and Home Assistant's Alarm Control Panel, offering advanced functionality for home security management.

This fork adds **tamper handling** (keypad enclosure opened or removed) and a French-translated interface.

## Import the blueprint

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2F%3Cyour-username%3E%2F%3Cyour-repo%3E%2Fblob%2Fmain%2Ffrient_keypad_alarmo_tamper.yaml)

> Replace `<your-username>` and `<your-repo>` in the link with your own GitHub account and repository name.

Requires Home Assistant **2024.10.0** or newer.

## Features

- **Arm All Zones**: Activate all zones with the **Lock** button.
- **Arm Day Zones**: Activate day zones with the **Home** button.
- **Arm Night Zone**: Activate night zones with the **Moon** button.
- **Disarm**: Deactivate the alarm with the **Unlock** button.
- **Emergency Mode**: Trigger an action when the **SOS** button is pressed.
- **Entry Delay**: The keypad emits beeps and flashes a green light during the entry delay.
- **Exit Delay**: The keypad emits beeps and flashes a red light during the exit delay.
- **Invalid Code**: Flashes an orange light when an incorrect PIN or RFID tag is used.
- **Alarm in Progress**: Flashes a red light when the alarm is active.
- **RFID**: Support for multiple PIN codes and RFID tags.
- **Custom actions**: Up to 3 special PIN codes can run arbitrary actions.

### What this fork adds

- **Tamper handling**: reacts when the keypad reports `tamper: true`.
  - Optionally **triggers the alarm** (`alarm_control_panel.alarm_trigger`) when the tamper is detected **and the alarm is armed** (home, night, away...). Nothing is triggered while the alarm is disarmed.
  - Separate configurable actions for tamper while **armed** and tamper while **disarmed** (e.g. a simple notification).
- **Armed Home fix**: the `armed_home` state was never forwarded to the keypad because its trigger was missing. It is now handled.
- **French translation** of the blueprint interface.

### RFID Tips:

- Use tools like [NFC Tools](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc&hl=en&gl=US) (on Android) to read your RFID tags. Simply copy the tag ID (preceded by `+`) into the list of allowed PIN codes.

## Setup

1. **Pair the keypad** with Zigbee2MQTT.
2. **Import the blueprint** and fill in:
   - `Keypad MQTT State Topic` → e.g. `zigbee2mqtt/Keypad`
   - `Keypad MQTT Set Topic` → e.g. `zigbee2mqtt/Keypad/set`
   - `Keypad Pincode List` → valid PINs and `+RFID` tags, separated by `;`
   - `Control Panel` → your Alarmo / manual alarm control panel
   - `Control Panel Pincode` → the code used to control the panel
3. Create the same PIN codes in Alarmo (Alarmo also supports RFID tags).
4. *(Optional)* In the **Tamper** section, enable the alarm trigger and/or choose your actions.

## Tamper notes and limitations

- The blueprint listens to the `tamper` field of the keypad's Zigbee2MQTT message. Check in the Z2M **State** tab that your keypad exposes it.
- Whether `alarm_trigger` is accepted depends on your alarm control panel. If it does not work with Alarmo, add the keypad's tamper `binary_sensor` in Alarmo as a sensor of type **Tamper** and let Alarmo handle it.
- While the enclosure stays open, the keypad may keep sending messages: the *disarmed* action can therefore run several times. The *armed* path does not repeat once the alarm has been triggered, because the panel is no longer in an armed state.
- The keypad runs on batteries and may take a few seconds to wake up and play the entry-delay beeps. This is a Zigbee sleepy-device behavior, not a blueprint delay.

## Credits

- Original blueprint: [Bygood91/frient_keypad_alarmo](https://github.com/Bygood91/frient_keypad_alarmo)

## License

This project is licensed under the MIT License, like the original. See the `LICENSE` file for details.

---

# 🇫🇷 Français

Ce projet fournit une automatisation pour relier un **clavier Frient** au module **Alarmo** (ou à toute centrale d'alarme) dans Home Assistant, via **Zigbee2MQTT**. Il permet une communication fluide entre le clavier et la centrale d'alarme de Home Assistant, avec des fonctionnalités avancées pour la gestion de la sécurité du domicile.

Ce fork ajoute la **gestion du tamper** (ouverture ou arrachement du boîtier du clavier) et une interface traduite en français.

## Importer le blueprint

[![Ouvrir votre instance Home Assistant et afficher la fenêtre d'import du blueprint.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2F%3Cyour-username%3E%2F%3Cyour-repo%3E%2Fblob%2Fmain%2Ffrient_keypad_alarmo_tamper.yaml)

> Remplacez `<your-username>` et `<your-repo>` dans le lien par votre compte GitHub et le nom de votre dépôt.

Nécessite Home Assistant **2024.10.0** ou plus récent.

## Fonctionnalités

- **Armement total** : active toutes les zones avec le bouton **Cadenas fermé**.
- **Armement jour** : active les zones de jour avec le bouton **Maison**.
- **Armement nuit** : active les zones de nuit avec le bouton **Lune**.
- **Désarmement** : désactive l'alarme avec le bouton **Cadenas ouvert**.
- **Mode urgence** : déclenche une action lorsque le bouton **SOS** est pressé.
- **Délai d'entrée** : le clavier émet des bips et clignote en vert pendant le délai d'entrée.
- **Délai de sortie** : le clavier émet des bips et clignote en rouge pendant le délai de sortie.
- **Code invalide** : clignotement orange lorsqu'un code PIN ou un badge RFID incorrect est utilisé.
- **Alarme en cours** : clignotement rouge lorsque l'alarme est active.
- **RFID** : prise en charge de plusieurs codes PIN et badges RFID.
- **Actions personnalisées** : jusqu'à 3 codes PIN spéciaux peuvent lancer des actions au choix.

### Ce que ce fork ajoute

- **Gestion du tamper** : réagit lorsque le clavier signale `tamper: true`.
  - Peut **déclencher l'alarme** (`alarm_control_panel.alarm_trigger`) lorsque le tamper est détecté **et que l'alarme est armée** (maison, nuit, absence...). Rien n'est déclenché lorsque l'alarme est désarmée.
  - Actions distinctes et configurables pour le tamper **centrale armée** et **centrale désarmée** (ex : simple notification).
- **Correction « Armée maison »** : l'état `armed_home` n'était jamais transmis au clavier car son trigger manquait. Il est désormais pris en charge.
- **Traduction en français** de l'interface du blueprint.

### Astuces RFID :

- Utilisez des outils comme [NFC Tools](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc&hl=fr&gl=FR) (sous Android) pour lire vos badges RFID. Copiez simplement l'identifiant du badge (précédé de `+`) dans la liste des codes PIN autorisés.

## Installation

1. **Appairez le clavier** avec Zigbee2MQTT.
2. **Importez le blueprint** et renseignez :
   - `Topic d'état MQTT du clavier` → ex : `zigbee2mqtt/Keypad`
   - `Topic « set » MQTT du clavier` → ex : `zigbee2mqtt/Keypad/set`
   - `Liste des codes du clavier` → codes PIN et badges `+RFID` valides, séparés par `;`
   - `Centrale d'alarme` → votre centrale Alarmo / manuelle
   - `Code de la centrale d'alarme` → le code utilisé pour piloter la centrale
3. Créez les mêmes codes PIN dans Alarmo (Alarmo prend aussi en charge les badges RFID).
4. *(Facultatif)* Dans la section **Tamper**, activez le déclenchement de l'alarme et/ou choisissez vos actions.

## Notes et limites du tamper

- Le blueprint lit le champ `tamper` du message Zigbee2MQTT du clavier. Vérifiez dans l'onglet **État** de Z2M que votre clavier l'expose bien.
- L'acceptation de `alarm_trigger` dépend de votre centrale. Si cela ne fonctionne pas avec Alarmo, ajoutez le `binary_sensor` tamper du clavier dans Alarmo comme capteur de type **Tamper** et laissez Alarmo le gérer.
- Tant que le boîtier reste ouvert, le clavier peut continuer à envoyer des messages : l'action *désarmée* peut donc s'exécuter plusieurs fois. Le chemin *armée* ne se répète pas une fois l'alarme déclenchée, car la centrale n'est plus dans un état armé.
- Le clavier fonctionne sur piles et peut mettre quelques secondes à se réveiller pour émettre les bips du délai d'entrée. C'est un comportement normal des appareils Zigbee en veille, pas un délai du blueprint.

## Crédits

- Blueprint d'origine : [Bygood91/frient_keypad_alarmo](https://github.com/Bygood91/frient_keypad_alarmo)

## Licence

Ce projet est sous licence MIT, comme l'original. Voir le fichier `LICENSE` pour plus de détails.
