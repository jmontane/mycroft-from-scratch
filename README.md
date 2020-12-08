# mycroft des de zero
# Creació d'una màquina virtual

Baixeu una ISO d'un Linux basat en Debian. En aquesta guia usarem [https://ubuntu.com/download/desktop](Ubuntu 20.04 LTS per a escriptori), però la versió de servidor és millor si no necessitem interfície gràfica


# Instal·lació de Mycroft
En la màquina virtual anterior, només cal executar aquestes ordres des del terminal:
```
sudo apt-get update
sudo apt-get install -y alsa pulseaudio git
git clone https://github.com/MycroftAI/mycroft-core.git
cd mycroft-core
./dev_setup.sh -fmDo you want to run on 'master' or against a dev branch? 
```

- A la pregunta ```Do you want to run on 'master' or against a dev branch?```, respongueu Y. Així usareu la branca estable del nucli de Mycroft.
- A la pregunta ```Would you like to automatically update whenever launching Mycroft?``` respongueu Y. Aixi Mycroft s'actualitzarà automàticament.
- A la pregunta ```Would you like this to be added to your PATH in the .profile?````, respongueu Y. Així podreu executar algunes ordres de Mycroft direcgtament.
- L'instal·lador us demanarà la contrasenya per a crear la carpeta que contindrà les habilitats ```/opt/mycroft/skills```
- A la pregunta ```Do you want to automatically check code-style when submitting code.````, respongueu Y.
- Es possible que us demani confirmació per a instal·lar els paquests necessaris ```Do you want to continue?```, respongueu Y.

# Configuració de Mycort en català
Teniu una guia molt completa per a configurar Mycroft en català [https://github.com/JarbasLingua/mycroft-catalan.conf/blob/main/readme-ca.md](aquí), que us recomanem llegir, però el resum és:

## Instal·leu el mòdul STT
```
mycroft-pip install jarbas-stt-plugin-chromium
```
## Instal·leu festival
```
sudo apt-get -y install festival festvox-ca-ona-hts lame
```

## Instal·leu l'extensió snowboy
Si poseu Mycroft a dormir amb l'habilitat ```snpatime```, ens caldrà personalitzar la paraula d'activació "wake up", per a pofer-ho fer, instal·lem l'extensió ```snowboy```

```
mycroft-pip install jarbas-wake-word-plugin-snowboy
```

## Instal·leu l'habilitat alternativa a Wolfram Alpha

Si Mycroft no "captura" l'ordre que li envia l'usuari, l'envia a l'habilitat Wolfram Alpha, però aquesta habilitat té força errors i no funciona correctament en català. Així que és millor usar auqesta habilitat alternativa:

```msm install https://github.com/JarbasSkills/skill-wolfie```

## Instal·leu l'habilittat de notícies alternativa


## Editeu el fitxer ```~/.mycroft/mycroft.conf```
Amb aquesta configuració al fitxer mycroft.conf, Mycroft us hauria de funciona en català:
```
{
  "lang": "ca-es",
  "stt": {
      "module": "chromium_stt_plug",
      "chromium_stt_plug": {
          "lang": "ca-ES"
      }
   },
  "tts": {
      "module": "festival",
      "festival": {
          "lang": "catalan",
          "encoding": "ISO-8859-15//TRANSLIT"
      }
   },
  "skills": {
    "blacklisted_skills": [
          "mycroft-npr-news.mycroftai", 
          "mycroft-fallback-duck-duck-go.mycroftai", 
          "mycroft-joke.mycroftai"
    ]
  },
  "listener": {
      "stand_up_word": "desperta"
  },
  "hotwords": {
     "desperta": {
        "module": "snowboy_ww_plug",
        "models": [
            {"sensitivity": 0.5, "model_path": "desperta_jm.pmdl"},
           {"sensitivity": 0.5, "model_path": "desperta_jm2.pmdl"}
         ]
      }
   }
}
```


