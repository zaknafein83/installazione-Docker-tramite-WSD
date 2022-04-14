# Installazione Docker tramite WSL!

![](https://www.docker.com/wp-content/uploads/2021/09/Docker-build-900x551.png.webp)


## Introduzione

Nel 2021 **Docker** ha rilasciato la notizia della limitazione nell'uso della versione gratuita della sua utility desktop alle aziende, introducendo un abbonamento mensile per le aziende. Questa nuova disposizione  **entrerà in vigore il 1° marzo del 2022**  e fa parte di un cambiamento più ampio ai termini di licenza di Docker.

## Cosa vuol dire e cosa bisogna fare

Un alternativa è usare il *Sottosistema Windows per Linux* (**WSL**) consente agli sviluppatori di eseguire un ambiente GNU/Linux, inclusi la maggior parte degli strumenti da riga di comando, delle utilità e delle applicazioni, direttamente in Windows, senza modifiche e senza il sovraccarico di una macchina virtuale tradizionale o di una configurazione di avvio doppio. Vediamo come fare

### Installazione WSL


![](https://github.com/zaknafein83/installazione-Docker-tramite-WSL/blob/master/powershell-admin.png)


Avviare la **PowerShell** di Windows con i privilegi di amministratore e digitare il seguente comando

    wsl --install

Se tutto è configurato **correttamente**, sulla vostra *PowerShell*  dovreste avere configurato la *bash di Ubuntu* senza alcun problema. Nel caso in cui doveste avere problemi, è invece necessario seguire la seguente procedura.
#### Abilitare Windows Subsystem for Linux (WSL)

    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
#### Abilitare WSL

    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

#### Aggiornamento del Kernel Linux
Scaricare ed installare uno dei seguenti aggiornamenti a seconda della vostra architettura della macchina.

    https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

    https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi
#### Impostare WSL come versione di Default

    wsl --set-default-version 2
#### Installare una versione di Linux 
In questo step finale basterà scaricare ed installare Ubuntu dal Microsoft Store e create la vostra user esempio come **username** e poi scegliete una **password**.

> Usate dei dati che ricordate, essendo queste le credenziali che vi permetteranno di accedere a questo ambiente virtuale installato sulla vostra macchina.


![](https://github.com/zaknafein83/installazione-Docker-tramite-WSL/blob/master/get-ubuntu.png)


### Installazione Docker tramite WSL/Bash Linux

Avviate la versione di Ubuntu precedentemente installata e lanciate i seguenti comandi:
#### Impostare la repo di docker

     sudo apt-get update
     sudo apt-get install \
        ca-certificates \
        curl \
        gnupg \
        lsb-release

> N.B. i comandi sono inviati in sequenza, quindi attenzione che la sequenza venga rispettata 
#### Inserire la chiave GPG KEY ufficiale di Docker

     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    
> Usare la GPG di riferimento della vostra distribuzione precedentemente installata. Nel nostro caso, la ubunt/gpg è quella corretta.
#### Impostare la repository per scaricare Docker

      echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
      $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

#### Installare Docker Engine

    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io
#### Avviare il processo di Docker

    sudo /etc/init.d/docker start
Infine per fare un test di verifica :

    sudo docker run hello-world
###	Installazione Docker-Compose 

Adesso bisogna installare docker-compose per fare la build dei file docker compose yml. Eseguire i seguenti comandi dalla bash linux.
#### Scaricare l'ultima versione di docker-compose

    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
#### Cambiare i permessi dei file

    sudo chmod +x /usr/local/bin/docker-compose
#### Controllare la versione del Compose

    sudo docker–compose --version

A questo punto possiamo utilizzare i comandi di **linux** direttamente da **Powershell** senza passare per la **bash ubuntu**, basterà iniziare il qualsiasi riga di linux con **WSL**.
Ecco come lancio il compose tramite WSL

    wsl sudo docker-compose -f docker-compose.yml up


siamo pronti per usare **docker** tramite **wsl** senza **docker desktop**.

![enter image description here](https://www.docker.com/wp-content/uploads/2021/09/Moby-run.png)


## Credit

Questo documento nasce da una esigenza aziendale e prende spunto da un documento redatto in prima istanza da [Francesco Lombardi](https://github.com/Francesco-Lombardi). Tutte le informazioni sono reperibili sul web in autonomia, con il presente documento si vuole dare una guidelines.