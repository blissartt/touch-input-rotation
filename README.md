#touchdev-calibration

Su GNU/Linux cambiare l'orientamento dello schermo è piuttosto semplice su desktop enviroment come GNOME o KDE,
ma non lo è altrettanto su window manager indipendenti (come dwm o i3). Il motivo? Gli strumenti grafici che permettono tale rotazione in questo
caso non sono presenti. Si può ricorrere all'utilissimo tool "Arandr", con il quale possiamo modificare questi parametri con un interfaccia
grafica, utilizzando "in sottofondo" lo strumento "xrandr", presente in quasi tutte le distribuzioni.

Andando però a cambiare l'orientamento dello nostro schermo tramite Arandr, sui laptop si possono manifestare problemi con il
trackpad o la funzione touchscreen del nostro dispositivo. Xrandr, in questa circostanza, ne invertirà in modo fallace l'input, il che
può rivelarsi piuttosto frustrante se si desidera utlizzare il nostro dispositivo in modalità 'tablet'.

Il problema è che l'utility "xrandr" ruota solo i movimenti del touchscreen e del trackpad sullo schermo, ma non l'input in sè.
Esiste una soluzione. Con il comando "xinput -set-prop " si possono modificare le proprietà dei dispositivi di input del nostro sistema,
tra le quali c'è una funzione interessante, che si chiama "Coordinate Transformation Matrix".
Questa ci permette sostanzialemnete di ricalibrare i nostri dispositivi di input senza incappare in ulteriori problemi.

Per esempio:
dato che:
		
	TOUCHPAD='DEFAULT'
	TOUCHSCREEN='DEFAULT-1'

con i comandi:

	xinput set-prop "$TOUCHPAD"    "$TRANSFORM" 0 -1 1 1 0 0 0 0 1
		
  e
		
	xinput set-prop "$TOUCHSCREEN" "$TRANSFORM" 0 -1 1 1 0 0 0 0 1
		
si possono calibrare il touchpad e il touchscreen, orientandoli verso sinistra. Assumendo che i valori DEFAULT e DEFAULT-1
vengano rimpiazzati dal nome del trackpad e del touchscreen (reperibili con 'xinput list'), questi comandi funzionano su tutti i 
sistemi GNU/Linux. Invece di utilizzare 'xinput list', abbiamo creato due bash script che reperisce i nomi dei vostri
dispositvi di input.

Per modificare in modo efficace queste variabili, l'utente GitHub 'mildmojo', ha creato uno shell script che cambia in modo automatico
l'orientamento dello schermo, del trackpad e del touchscreen, a patto che i valori TOUCHPAD e TOUCHSCREEN siano soddisfatti
correttamente. Per trovare in modo efficace il nome dei due dispositivi richesti da suddetto script, nello zip verranno acclusi due bash script che estraggono
il nome dei dispositivi dal database 'udev' del vostro sistema GNU/Linux.
 
 
 
 
TL;DR
Ecco i vari step per cambiare efficacemente l'orientamento dello schermo e dei dispositivi touch su un sistema GNU/Linux.
Le seguenti istruzioni sono applicabili a qualsiasi distribuzione. PER EVITARE PROBLEMI, È PREFERIBILE ESEGUIRE I COMANDI IN UNA SHELL
SENZA PERMESSI ROOT. Per clonare la repo Github è inoltre necessario lo strumento 'git'.

-0 Clonare la repo Github ed entrarvi all'interno:

	git clone https://github.com/blissartt/touchdev-calibration
	cd touchdev-calibration


-1 Rendere eseguibili i due script con il seguente comando:
			
	chmod a+x recognize-touchpad.sh recognize-touchscreen.sh


-2 Eseguire i due script con i seguenti comandi e salvare il risultato ottenuto per il passaggio successivo, ricordandosi quale dei due output fosse del touchpad e
quale fosse del touchscreen.
			
	./recognize-touchpad.sh
	./recognize-touchscreen.sh	


-3 Aprire lo script "rotate-desktop.sh" con un text editor e sostituire i valori DEFAULT e DEFAULT-1 negli apostrofi con gli output ricevuti nel
passaggio precedente, cioè il nome dei dispostitivi in questione. Dopodiche, salvare il file.

Esempio:

	TOUCHPAD='SynPS/2 Synaptics TouchPad'
	TOUCHSCREEN='TPPS/2 IBM TrackPoint'		


-4 Rendere eseguibile il file con il seguente comando:

	chmod a+x rotate-desktop.sh


-5 Eseguire il file, specificando l'orientamento, con questo comando:

	./rotate-desktop.sh ORIENTAMENTO
dove "ORIENTAMENTO" deve essere sostituito da una delle seguenti opzioni:
		
-left
-right
-inverted
-normal

Una volta eseguito, lo script cambierà l'orientamento dello schermo e dei dispositivi touch. Gli script usati in
precedenza possono essere eliminati.
		
		

