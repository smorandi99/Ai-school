# Ai-school

## From grayscale to color image

Nella prima parte di questo progetto ho utilizzato un autoencoder che, data in ingesso un'immagine in grayscale, restituisse la stessa nello spazio colore RGB. 
Per permettere ciò ho allenato un autoencoder su un dataset di immagini contenete circa 1300 immagini da me creato. Le immagini contenute nel dataset sono state scaricate alcune da internet ed altre da roboflow e rappresentano differenti landscape. 
![1*nqzWupxC60iAH2dYrFT78Q](https://user-images.githubusercontent.com/112158913/200778319-c713b74a-e422-444a-bede-03b6a87c5b94.png)


Nella prima parte di codice ho collegato Google Drive al mio progetto ed assegnato il percorso alla cartella contenente il dataset alla variabile path. 

La prima cosa da fare, dopo aver caricato il dataset, è quella di effettuare un rescaling delle immagini dividendo il valore di ogni pixel di ogni canale per 255 in modo da normalizzare tutti i valori tra 0 e 1. Il motivo principale per cui è stata effettuata questa normalizzazione è che come funzione di attivazione ho scelto di utilizzare la ReLu, che assume valori tra 0 e 1.

Il passaggio successivo è quello di effettuare un resize delle immagini: dato che sono state scaricate in diversi momenti e da diverse fonti sono caratterizzate da dimensioni differenti, sono state tutte ridimensionate a 256x256 pixel.

Dato che le immagini sono tutte nello spazio RGB viene effettuata una conversione nello spazio lab che significa andare successivamente a lavorare su un layer caratterizzato dalle informazioni sulla luminosità e due valori a e b che rappresentano i quattro colori visibili all'occhio umano: rosso, verde, blu e giallo.

![lab-color-space](https://user-images.githubusercontent.com/112158913/200777154-140d6a16-e317-4b13-8183-9995f52a6e39.png)

Per effettuare la conversione dallo spazio RGB allo spazio lab, il canale che rappresenta la luminosità, ovvero il canale dell'immagine in bianco e nero, viene assegnato al vettore X; invece i canali a e b che rappresentano i colori vengono assegnati al vettore Y.
I vlori contenuti nella Y vengono normalizzati tra -1 e 1, dato che, come detto in precedenza, la ReLu assume valori compresi tra 0 e 1, nell'ultimo layer viene utilizzata la tanh come funzione di attivazione la quale presenta un dominio compreso tra -1 e 1.

Ecco la struttura del modello utilizzato:
<img width="459" alt="Screenshot 2022-11-09 at 09 41 00" src="https://user-images.githubusercontent.com/112158913/200781729-09fd09ff-0ddf-419e-a7c6-770991ed1885.png">

l'input del modello è di dimensione 256x256x1, ovvero il canale L. Dopo la prima convoluzione sono già presenti 640 parametri utilizzati nel training. Per scelta personale non ho utilizzato maxpooling ma solo layer convolutivi.

Una volta che l'immagine raggiunge la dimensione 32x32 viene passata al decoder e inizia la fase di upscaling dell'immagine in modo da avere in output un'immagine della stessa dimensione di quella in input, ovvero 256x256x2 perchè i canali a e b sono stati ricostruiti interamente.

Ultimo passaggio è quello di riconvertire l'immagine dallo spazio lab allo spazio RGB e salvare il risultato ottenuto. 

Dopo aver allenato il modello, dandogli in ingresso un'immagine in grayscale, l'output che si ottiene è l'immagine di input colorata.
