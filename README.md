# 🧠 Shelly Pro 3EM Simulation via Node-RED & Home Assistant

Ce module Node-RED simule un appareil **Shelly Pro 3EM** en envoyant ses données (phase C) vers un serveur externe compatible **StorCube**, via MQTT. Il lit les données réelles d’un capteur Home Assistant (par ex. `sensor.shellypro3em_ac01234_phase_c_active_power`) et les injecte dans le flux MQTT comme si elles provenaient d’un vrai Shelly Pro 3EM.

---

## 🚀 Fonctionnalités

- ✅ Envoi périodique des données du Shelly 3EM (phase C uniquement)
- ✅ Lecture automatique des valeurs depuis Home Assistant
- ✅ Publication MQTT vers un serveur distant (ex : `sonicpo.com`)
- ✅ Simulation du topic `/events/rpc` conforme à la structure d’un Shelly Pro 3EM
- ✅ Envoi de l’état `online` toutes les 30 secondes

---

## 📦 Prérequis

- Home Assistant avec l'intégration MQTT
- Add-on Node-RED installé et fonctionnel
- Capteur de type `sensor` existant dans Home Assistant
- Accès au broker MQTT distant avec identifiants

---

## ⚙️ Variables personnalisables

Dans le flow, vous pouvez modifier les éléments suivants :

| Variable                            | Description                                      |
|-------------------------------------|--------------------------------------------------|
| `sensor.shelly_power`               | `entity_id` du capteur à lire                   |
| `shellypro3em-ac15187b0123`         | ID MQTT simulé du Shelly Pro 3EM                |
| `sonicpo.com`                       | Adresse du broker MQTT                          |
| `shelly-pro / shelle-pro0117`       | Identifiants de connexion au broker             |

---

## 🛠 Installation

1. Ouvrez **Node-RED** dans Home Assistant.  
2. Cliquez sur **menu > Import**, puis collez le contenu du flow JSON fourni.  
3. Modifiez le capteur de référence dans le node **"capteur à modifier"**  
   - Exemple : dans **Entity ID**, mettez `sensor.votre-capteur-de-reference`  
4. Modifiez le `fake_id` dans :
   - le node **"donnée fake_id à modifier"**
   - le node **"Préparer online msg"**  
   - Exemple : `fake_id = "test1234"` et topic = `"test1234/online"`
5. Cliquez sur **Deploy**  
6. Vérifiez que vous avez des logs dans les nodes **"debug"**, confirmant que la donnée est bien lue (ex. : `function : (warn) "Valeur capteur brute : 1900.9"`, ce qui correspond à 1900.9 W).  
7. Dans l'application **StorCube**, entrez l'ID dans la catégorie **compteurs électriques intelligents**  
   - Exemple : ID `test1234` (choisissez **toujours la phase C**, faites attention aux majuscules)
8. Dans le mode de contrôle de sortie, sélectionnez **compteur intelligent**

---

## 🧪 Exemple de payload envoyé

```json
{
  "src": "shellypro3em-ac15187b0123",
  "dst": "shellypro3em-ac15187b0123/events/rpc",
  "method": "NotifyStatus",
  "params": {
    "ts": 1752783371.883,
    "events": [
      {
        "component": "sys",
        "event": "scheduled_restart",
        "time_ms": 996,
        "ts": 1752783371.883
      }
    ],
    "em:0": {
      "a_act_power": 0,
      "b_act_power": 0,
      "c_act_power": 1174.4
    }
  }
}
