# touchdev-calibration
A little script library to rotate effectively your screen and touch input devices in GNU/Linux. Written in Italian.
--------------------------------------------------------------------------------------------------------------------
# Cambiare l'orientamento dello schermo su GNU/Linux.

Su GNU/Linux cambiare l'orientamento dello schermo è piuttosto semplice sui maggiori desktop environment come GNOME o KDE,
ma non lo è altrettanto su window manager indipendenti(come dwm o i3). Il motivo? Gli strumenti grafici che fanno parte
dei pacchetti CORE dei suddetti DE(desktop enviroment) in questo caso non sono presenti.
Si può ricorrere all'utilissimo tool "Arandr", con il quale possiamo modificare questi parametri comodamente con un interfaccia
grafica, che usa "in sottofondo" lo strumento basico GNU "xrandr".

Se andiamo però a cambiare l'orientamento dello nostro schermo tramite Arandr, sui laptop si possono manifestare problemi con il
trackpad o la funzione touchscreen del nostro dispositivo. Xrandr, in questa circostanza, invertirà l'orientamento dei tali: il che ne rende
 l'uso davvero frustante se si desidera utilizzare il proprio dispositivo in "modalità tablet".


Il problema è che il comando "xrandr" non tiene conto della visuale dell'utente, e dunque cambia l'orinetamento solo dell'input, e non
"ruota" tutto il trackpad o il touchscreen. Esiste una soluzione. Con il comando "xinput -set-prop " si possono modificare le proprietà
dei dispositivi di input del nostro sistema, tra i quali c'è una funzione interessante, che si chiama "Coordinate Transformation Matrix".
Questa ci permette sostanzialmente di ricalibrare i nostri dispositivi di input senza incappare in ulteriori problemi.

Per esempio:
dato che:
		
		TOUCHPAD='DEFAULT'
		TOUCHSCREEN='DEFAULT-1'

con i comandi:

		xinput set-prop "$TOUCHPAD"    "$TRANSFORM" 0 -1 1 1 0 0 0 0 1
		
		e
		
		xinput set-prop "$TOUCHSCREEN" "$TRANSFORM" 0 -1 1 1 0 0 0 0 1
		
si possono calibrare il touchpad e il touchscreen, orientandoli verso sinistra. Assumendo che i valori DEFAULT e DEFAULT-1
vengano rimpiazzati dal nome del trackpad e del touchscreen (reperibili con "xinput list"), questi comandi funzionerebbero su tutti i 
sistemi GNU/Linux.



Per modificare in modo efficace queste variabili, l'utente GitHub "mildmojo", ha creato uno shell script che cambia in modo automatico
l'orientamento dello schermo, del trackpad e del touchscreen, a patto che i valori TOUCHPAD e TOUCHSCREEN siano soddisfatti
correttamente.

Per trovare in modo efficace il nome dei due dispositivi richesti da suddetto script, nello zip verranno acclusi due file ".sh" che estraggono
il nome dei dispositivi dal database UDEV del vostro sistema GNU/Linux.

 
 
 
 
 TL;DR
 
 Ecco i vari step per cambiare efficacemente l'orientamento dello schermo e dei dispositivi touch su un sistema GNU/Linux.
 (Le seguenti istruzioni sono applicabili a qualsiasi distribuzione). Per questo HOW TO, i vari passaggi devono essere eseguiti da
 terminale all'interno della cartella scaricata. NON È NECESSARIO L'USO DELL'AMMINISTRATORE, PER EVITARE PROBLEMI, È PREFERIBILE
 ESEGUIRE I COMANDI IN UNA SHELL SENZA PERMESSI ROOT.
 
 
 -1 Rendere eseguibili i due script con il seguente comando.
			
	    chmod a+x recognize-touchpad.sh recognize-touchscreen.sh
---


-2 Eseguire i due script con i seguenti comandi e salvare il risultato ottenuto per il passaggio successivo, ricordandosi quale dei due output fosse del touchpad e
quale fosse del touchscreen.
			
			./recognize-touchpad.sh
			./recognize-touchscreen.sh
---			


-3 Aprire lo script "rotate-desktop.sh" con un text editor e sostituire i valori DEFAULT e DEFAULT-1 negli apostrofi con gli output ricevuti nel
passaggio precedente, cioè il nome dei dispostitivi in questione. Dopodiche, salvare il file.

Esempio:

		TOUCHPAD='SynPS/2 Synaptics TouchPad'
		TOUCHSCREEN='TPPS/2 IBM TrackPoint'
---			


-4 Rendere eseguibile il file con il seguente comando:

		chmod a+x rotate-desktop.sh
---


-5 Eseguire il file, specificando l'orientamento, con questo comando:

		./rotate-desktop.sh ORIENTAMENTO
		"ORIENTAMENTO" deve essere sostituito da una delle seguenti opzioni:
		
		-left
		-right
		-inverted
		-normal

Una volta eseguito, lo script cambierà l'orientamento dello schermo e dei dispositivi touch. Gli script usati in
precedenza possono essere eliminati.

FINE :)


1^ BONUS TIP=
Si può creare un alias nel file .bashrc per automatizzare questo processo e non dover cercare ogni volta lo script da eseguire.

Per esempio, se si è salvata la cartella "touchdev" in /home/user/Downloads, allora l'alias sarà:

		alias QUELLOCHEVUOI='/home/user/Downloads/touchdev/./rotate-desktop.sh ORIENTAMENTO'
---

2^ BONUS TIP=
Si può creare un menù nella TRAY JWM per rendere ancora più semplice questo processo(a patto che si stia utilizzando una sessione JWM). 


		
		



