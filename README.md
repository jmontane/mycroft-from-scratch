# mycroft des de zero
# Creació d'una màquina virtual

- Baixeu una ISO d'un Linux basat en Debian. En aquesta guia usarem [https://ubuntu.com/download/desktop](Ubuntu 20.04 LTS per a escriptori), però la versió de servidor és millor si no necessitem interfície gràfica
- Baixeu i instal·leu [htps://www.virtualbox.org/](Virtualbox) per al vostre sistema operaitu. Hauria de ser la versió 6.010 o superior.
- Obrei el VirtualBox application i feu clic a 'Nova'
- Poseu un nom a la màquina (Mycroft-Ubuntu) i trieu Linux i Ubuntu (64-bit)
- Continueu per la resta de la de la configuració de la màquina virtual amb les opcions predetermionades.
- Més RAM no va mai mal, però amb 4 GB si useu Ubuntu d'escriptori o 1 GB si Ubuntu per a servidor, n'hauríeu de tenir prou.
- Copieu la ISO d'Ubuntu que heu baixat a la carpeta que s'ha creat per a la vostra màquina virtual (p. ex. ~/VirtualBox VMs/Mycroft-Ubuntu)
- Seleccioneu la màquina virtual que heu creat i inicieu-la!
It will prompt you to select a virtual disk file. Press the folder icon to select the Ubuntu Server ISO downloaded earlier.

Engegarà Ubuntu i podreu fer una instal·lació predeterminada.

La màquina virtual es reiniciarà al final del procès. Si heu instal·lat Ubuntu per a servidors cal que pitgeu la tecla retorn un parell de vegades.
- Inicieu sessió amb les credencials que heu definit durant la instal·lació.
- Ja teniu una màquina virtual Ubuntu! Ara cal instal·lar-hi Mycroft.

# Instal·lació de Mycroft
En la màquina virtual anterior, només cal executar aquestes ordres des del terminal:
```
sudo apt-get update
sudo apt-get install -y alsa pulseaudio git
git clone https://github.com/MycroftAI/mycroft-core.git
cd mycroft-core
./dev_setup.sh -fm
```

- A la pregunta ```Do you want to run on 'master' or against a dev branch?```, respongueu N. Així usareu la branca de desenvolupament del nucli de Mycroft.
- A la pregunta ```Would you like to automatically update whenever launching Mycroft?``` respongueu Y. Aixi Mycroft s'actualitzarà automàticament.
- A la pregunta ```Would you like this to be added to your PATH in the .profile?````, respongueu Y. Així podreu executar algunes ordres de Mycroft direcgtament.
- L'instal·lador us demanarà la contrasenya per a crear la carpeta que contindrà les habilitats ```/opt/mycroft/skills```
- A la pregunta ```Do you want to automatically check code-style when submitting code.````, respongueu Y.
- Es possible que us demani confirmació per a instal·lar els paquests necessaris ```Do you want to continue?```, respongueu Y.
- Ara trigarà una mica en instal·lar Mycroft. Una vegada hagi acabat, és millor reniciar la màquina virtual on heu instal·lat Mycroft.
- Una vegada heu reniciat, obriu una finestra de terminal, aneu a ~/mycroft-core i activeu l'entern de desenvolupament: ```source venv.sh```
- Podeu iniciar Mycroft amb ```./start-mycroft.sh debug```
- Si tot ha funcionat bé, us demanarà de registrar el vostre aparell a home.mycroft.ai. Feu-ho. Només cal crear-vos un compte i registar l0aparell amb el codi de 6 dígits que us diu.
- Per a sortir de Mycrot poder fer ```:quit``` i, una vegada en el terminal indique ```./stop-mycroft.sh``` per a aturar els serveis de Mycroft.
- Ja teniu un Mycroft funcional, en anglès. Ara manca configurar-lo en català.


# Configuració de Mycroft en català
Teniu una guia molt completa per a configurar Mycroft en català [https://github.com/JarbasLingua/mycroft-catalan.conf/blob/main/readme-ca.md](aquí), que us recomanem llegir, però el resum és:

## Apedaceu lingua franca
Cal apedaçar la biblioteca lingua franca mentre el català no arrriba a la branca estable. Per a fer-ho, baixem i instal·lem la versió de lingua franca del repositori de Softcatalà:
```
cd ~
git clone https://github.com/softcatala/lingua-franca
cd mycroft-core
pip install ../lingua-franca/
```
## Apedaceu les les traduccions de les habilitats
Ara per ara, cal apedaçar les traduccions de les habilitats.
```
cd ~
git clone https://github.com/jmontane/mycroft-update-translations
cd mycroft-update-translations
pip install -r requirements.txt
./mycroft-update-translations.py
```

## Instal·leu el mòdul STT
```
cd ~/mycroft-core
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
L'habilitat de notícies té un problema amb els fluxos d'àudio http. Mentre es corregeix, millor usar aquesta habilitat alternativa.

```msm install https://github.com/JarbasLingua/skill-news``` 


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


