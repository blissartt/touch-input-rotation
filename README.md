# touch-input-rotation
This file describe the same process both in English and in Italian.

## Italian
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
dispositivi di input.

Per modificare in modo efficace queste variabili, l'utente GitHub 'mildmojo', ha creato uno shell script che cambia in modo automatico
l'orientamento dello schermo, del trackpad e del touchscreen, a patto che i valori TOUCHPAD e TOUCHSCREEN siano soddisfatti
correttamente. Per trovare in modo efficace il nome dei due dispositivi richesti da suddetto script, nello zip verranno acclusi due bash script che estraggono
il nome dei dispositivi dal database 'udev' del vostro sistema GNU/Linux.
 
 

Ecco i vari step per cambiare efficacemente l'orientamento dello schermo e dei dispositivi touch su un sistema GNU/Linux.
Le seguenti istruzioni sono applicabili a qualsiasi distribuzione. PER EVITARE PROBLEMI, È PREFERIBILE ESEGUIRE I COMANDI IN UNA SHELL
SENZA PERMESSI ROOT. Per clonare la repo Github è inoltre necessario lo strumento 'git'.

-0 Clonare la repo Github ed entrarvi all'interno:

	git clone https://github.com/blissartt/touchdev-calibration
	cd touchdev-calibration


-1 Rendere eseguibile lo script per riconoscere i device con il seguente comando:
			
	chmod a+x recognize-devices.sh


-2 Eseguire lo script e salvare il risultato ottenuto per il passaggio successivo:
			
	./recognize-devices.sh


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

## English
On GNU/Linux, changing screen orientation is rather simple on desktop enviroments such as GNOME or KDE,
but not so easy on stand-alone window managers (such as dwm or i3). The reason? The graphical tools that allow such rotation
are not present in this case. We can resort to the "Arandr" tool, which we can use to modify these parameters with a
graphical interface, using 'in the background' the 'xrandr' tool, which is present in almost all distributions.

However, if we go to change the orientation of our screen via Arandr, on laptops there may be problems with the
trackpad or the touchscreen function of our device. Xrandr, in this circumstance, will fallaciously reverse the input, which
can be quite frustrating if we want to use our device in 'tablet' mode.

The problem is that the 'xrandr' utility only rotates the movements of the touchscreen and trackpad on the screen, but not the input itself.
There is a solution. With the command 'xinput -set-prop' we can change the properties of our system's input devices,
among which is an interesting function called "Coordinate Transformation Matrix".
This essentially allows us to recalibrate our input devices without running into further problems.

For example...

if:
		
	TOUCHPAD='DEFAULT'
	TOUCHSCREEN='DEFAULT-1'

running the commands:

	xinput set-prop "$TOUCHPAD"    "$TRANSFORM" 0 -1 1 1 0 0 0 0 1
		
and
		
	xinput set-prop "$TOUCHSCREEN" "$TRANSFORM" 0 -1 1 1 0 0 0 0 1

it's possible to calibrate the touchpad and touchscreen, orienting them to the left. Assuming that the values DEFAULT and DEFAULT-1
are replaced by the name of the trackpad and touchscreen (these can be found with 'xinput list'). These commands will work on all 
GNU/Linux systems. Instead of using 'xinput list', we have created two bash scripts that retrieve the names of your
input devices.

To effectively change these variables, GitHub user 'mildmojo', has created a shell script that automatically changes
the orientation of the screen, trackpad and touchscreen, provided that the values TOUCHPAD and TOUCHSCREEN are met
correctly. In order to efficiently find the name of the two devices requested by the above script, two bash scripts are included in the repo.
These extract the name of the devices from the 'udev' database of your GNU/Linux system.

Here are the various steps to effectively change the orientation of the screen and touch devices.
The following instructions are applicable to any distribution. TO AVOID PROBLEMS, IT IS PREFERABLE TO RUN THE COMMANDS IN A SHELL
WITHOUT ROOT PERMISSIONS. The 'git' tool is also required to clone the Github repo.

-0 Clone the Github repo and enter it:

	git clone https://github.com/blissartt/touchdev-calibration
	cd touchdev-calibration


-1 Make the script "recognize-devices.sh" executable with the following command:
			
	chmod a+x recognize-devices.sh

-2 Run the script with the following command and save the result for the next step:
			
	./recognize-devices.sh

-3 Open the 'rotate-desktop.sh' script with a text editor and replace the values DEFAULT and DEFAULT-1 in the apostrophes with the output received in the
previous step, i.e. the name of the devices in question. Afterwards, save the file.

Example:

	TOUCHPAD='SynPS/2 Synaptics TouchPad'.
	TOUCHSCREEN='TPPS/2 IBM TrackPoint'		

-4 Make the file executable with the following command:

	chmod a+x rotate-desktop.sh

-5 Run the file, specifying the orientation, with this command:

	./rotate-desktop.sh ORIENTATION

where 'ORIENTATION' must be replaced by one of the following options:
		
-left
-right
-inverted
-normal

Once executed, the script will change the orientation of the screen and touch devices. Previously used scripts are no longer needed and they can be deleted.


		
		

