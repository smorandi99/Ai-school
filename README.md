# Ai-school

## From grayscale to color image

Nella prima parte di questo progetto ho utilizzato un autoencoder che, data in ingesso un'immagine in grayscale, restituisse la stessa a colori. 
Per permettere ciò ho allenato un autoencoder su un dataset di immagini contenete circa 1300 immagini da me creato. Le immagini contenute nel dataset sono state scaricate alcune da internet ed altre da roboflow e rappresentano differenti landscape. 
![1*nqzWupxC60iAH2dYrFT78Q](https://user-images.githubusercontent.com/112158913/200778319-c713b74a-e422-444a-bede-03b6a87c5b94.png)


Nella prima parte di codice ho collegato Google Drive al mio progetto ed assegnato il percorso alla cartella contenente il dataset alla variabile path. 

La prima cosa da fare, dopo aver caricato il dataset, è quella di effettuare un rescaling delle immagini dividendo il valore di ogni pixel di ogni canale per 255 in modo da normalizzare tutti i valori tra 0 e 1. Il motivo principale per cui è stata effettuata questa normalizzazione è che come funzione di attivazione ho scelto di utilizzare la ReLu, che assume valori tra 0 e 1.

Il passaggio successivo è quello di effettuare un resize delle immagini: dato che sono state scaricate in diversi momenti e da diverse fonti sono caratterizzate da dimensioni differenti, sono state tutte ridimensionate a 256x256 pixel.

Dato che le immagini sono tutte nello spazio RGB viene effettuata una conversione nello spazio lab che significa andare successivamente a lavorare su un layer caratterizzato da un canale contenente le informazioni sulla luminosità e due canali a e b che rappresentano i quattro colori visibili all'occhio umano: rosso, verde, blu e giallo.

![lab-color-space](https://user-images.githubusercontent.com/112158913/200777154-140d6a16-e317-4b13-8183-9995f52a6e39.png)

Per effettuare la conversione dallo spazio RGB allo spazio lab, il canale che rappresenta la luminosità, ovvero il canale dell'immagine in bianco e nero, viene assegnato al vettore X; invece i canali a e b che rappresentano i colori vengono assegnati al vettore Y.
I valori contenuti nella Y vengono normalizzati tra -1 e 1, dato che, come detto in precedenza, la ReLu assume valori compresi tra 0 e 1, nell'ultimo layer viene utilizzata la tanh come funzione di attivazione la quale presenta un dominio compreso tra -1 e 1.

Ecco la struttura del modello utilizzato:

<img width="459" alt="Screenshot 2022-11-09 at 09 41 00" src="https://user-images.githubusercontent.com/112158913/200781729-09fd09ff-0ddf-419e-a7c6-770991ed1885.png">

L'input del modello è di dimensione 256x256x1, 256 pixel per 256 pixel per un canale, ovvero il canale L. Dopo la prima convoluzione sono già presenti 640 parametri utilizzati nel training. Per scelta personale non ho utilizzato maxpooling ma solo layer convolutivi.

Una volta che l'immagine raggiunge la dimensione 32x32 viene passata al decoder e inizia la fase di upscaling dell'immagine in modo da avere in output un'immagine della stessa dimensione di quella in input, ovvero 256x256x2 perchè i canali a e b sono stati ricostruiti interamente.

Ultimo passaggio è quello di riconvertire l'immagine dallo spazio lab allo spazio RGB e salvare il risultato ottenuto. 

Dopo aver allenato il modello, dandogli in ingresso un'immagine in grayscale, l'output che si ottiene è l'immagine di input colorata.

![00000033_(2)](https://user-images.githubusercontent.com/112158913/200800627-4484c62a-f892-49a5-9e7f-e0277990d2e8.jpg)

![Unknown](https://user-images.githubusercontent.com/112158913/200799815-cd15e5bb-3044-4a28-b3d8-3f445bc4a390.png)


## Sea animal detection

In questa seconda parte del progetto, attraverso l'utilizzo di YOLOv5, ho fatto un confronto tra effettuare object detection riallenando interamente la rete oppure attraverso il fine tuning. 
Come soggetti da riconoscere ho scelto 7 specie marine: pesci, meduse, pinguini, pulcinelle di mare, squali, stelle marine e le mante.

Per quanto riguarda il dataset ne ho creato uno su roboflow unendone di differenti, uno per ogni specie, già lebellati. Per aumentare la quantità di dati disponibili ho effettuato della data augmentation. 

In primo luogo ho definito gli iperparametri del modello definiendo il numero di epoche. In seguito ho scaricato e preparato i dati: i dati originali scaricati da roboflow sono caratterizzati da due istanze per ogni immagine, di conseguenza ho eliminato il duplicato e la corrispondente label all'interno del file di testo contenente tutte le label. 

Scaricando il dataset da roboflow in automatico è stato creato il file data.yaml il quale contiene il percorso alle immagini del training e validation set e le corrispondenti label.
La struttura del file data.yaml è la seguente:

<img width="643" alt="Screenshot 2022-11-10 at 14 42 58" src="https://user-images.githubusercontent.com/112158913/201107768-41fdd8a9-05b6-4ba2-b8ae-22335395a95c.png">


Prima di iniziare ad allenare il modello ho controllato un paio di immagini ground truth e per farlo ho riportato le annotazioni delle bounding box da [x_center, y_center, width, height] a [x_min, y_min, x_max, y_max] stampandole poi a video. 

<img width="706" alt="Screenshot 2022-11-10 at 11 06 04" src="https://user-images.githubusercontent.com/112158913/201107839-2cc9c0fa-1598-4454-a69f-cfd488cf13cf.png">

Dopo aver creato una directory per salvare i risultati del modello ho clonato il repository di YOLO e installato tutte le relative dipendenze. 

In primo luogo ho allenato interamente la rete ottenendo i seguenti risultati:

<img width="851" alt="Screenshot 2022-11-10 at 12 01 20" src="https://user-images.githubusercontent.com/112158913/201075305-94795fed-1397-47c4-96ef-e64f22fb472b.png">

In seguito ho valutato le predizioni delle immagini appartenenti al validation set e successivamente ho effettuato il processo di inferenza sulle immagini appartenenti al test set. Ecco mostrate le predizioni sul test set:

<img width="872" alt="Screenshot 2022-11-10 at 12 07 05" src="https://user-images.githubusercontent.com/112158913/201075604-5d902e61-d2d1-4f7b-ae51-80b78c27383e.png">

<img width="1061" alt="Screenshot 2022-11-10 at 13 31 48" src="https://user-images.githubusercontent.com/112158913/201103964-5a130b40-2ab0-4497-8507-71f4b97a4b0d.png">

<img width="360" alt="Screenshot 2022-11-10 at 14 33 29" src="https://user-images.githubusercontent.com/112158913/201105292-24151e84-b5c1-4fd4-8d8e-aab4335466e7.png">

Come si può osservare il modello è stato allenato in maniera corretta e, nonostante non abbia raggiunto un'accuratezza troppo elevata, il valore di map_05 è sempre cresciuto, così come si può osservare che sia la train loss che la val loss sono diminuite durante la fase di training.

In secondo luogo ho effettuato finetuning per vedere se si ottenessero risultati migliori, peggiori o uguali. 
Per farlo ho utilizzato il modello YOLO di dimensioni medie che contiene 25 blocchi di layers caratterizzati da più di 20 milioni di parametri. In questo modo ho allenato solo gli ultimi layer freezzando i primi 15 e quindi mantenendo i parametri dei primi 15 blocchi fissi.  

I risultato ottenuti al termine della fase di train sono i seguenti: 

<img width="884" alt="Screenshot 2022-11-10 at 14 22 46" src="https://user-images.githubusercontent.com/112158913/201105550-d0b0f84d-7a51-47e2-9f40-f91a69e344da.png">

Ho poi confrontato questi con i risultati ottenuti precedentemente: 

<img width="1041" alt="Screenshot 2022-11-10 at 17 16 27" src="https://user-images.githubusercontent.com/112158913/201149724-068bc58b-e4e6-4d05-a12c-f00a465c073e.png">


<img width="367" alt="Screenshot 2022-11-10 at 14 32 52" src="https://user-images.githubusercontent.com/112158913/201108026-b6bb9e47-3a3e-42b6-8d94-7b684a068c75.png">

Quello che si può osservare è che il modello rappresentato dalla curva arancione (ovvero il primo) è migliore e si può osservare la stessa cosa nei dati che rappresentano la training loss, di fatto la curva arancione scende in maniera più veloce e questo sta a rappresentare che il modello sia in grado di imparare più velocemente ottenendo risultati migliori. 
