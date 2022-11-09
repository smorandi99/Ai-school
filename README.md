# Ai-school

## From grayscale to color image

Nella prima parte di questo progetto ho utilizzato un autoencoder che, data in ingesso un'immagine in grayscale, restituisse la stessa nello spazio colore RGB. 
Per permettere ciò ho allenato un autoencoder su un dataset di immagini contenete circa 1300 immagini da me creato. Le immagini contenute nel dataset sono state scaricate alcune da internet ed altre da roboflow e rappresentano differenti landscape. 

Nella prima parte di codice ho collegato Google Drive al mio progetto ed assegnato il percorso alla cartella contenente il dataset alla variabile path. 

La prima cosa da fare, dopo aver caricato il dataset, è quella di effettuare un rescaling delle immagini dividendo il valore di ogni pixel di ogni canale per 255 in modo da normalizzare tutti i valori tra 0 e 1. Il motivo principale per cui è stata effettuata questa normalizzazione è che come funzione di attivazione ho scelto di utilizzare la ReLu, che assume valori tra 0 e 1.

Il passaggio successivo è quello di effettuare un resize delle immagini: dato che sono state scaricate in diversi momenti e da diverse fonti sono caratterizzate da dimensioni differenti, sono state tutte ridimensionate a 256x256 pixel.

Dato che le immagini sono tutte nello spazio RGB viene effettuata una conversione nello spazio lab che significa andare successivamente a lavorare su un layer caratterizzato dalle informazioni sulla luminosità e due valori a e b che rappresentano i quattro colori visibili all'occhio umano: rosso, verde, blu e giallo.

![lab-color-space](https://user-images.githubusercontent.com/112158913/200777154-140d6a16-e317-4b13-8183-9995f52a6e39.png)
