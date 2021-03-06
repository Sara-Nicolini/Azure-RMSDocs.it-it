---
# required metadata

title: Modalità di iscrizione per RMS per utenti singoli | Azure RMS
description:
keywords:
author: cabailey
manager: mbaldwin
ms.date: 04/28/2016
ms.topic: article
ms.prod: azure
ms.service: rights-management
ms.technology: techgroup-identity
ms.assetid: a60731bd-f78d-4f00-bb3e-354637b312ab

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Modalità di iscrizione per RMS per utenti singoli

*Si applica a: Azure Rights Management*

Per iscriversi per ottenere l'account gratuito, gli utenti possono visitare la [pagina di Microsoft Rights Management](https://portal.aadrm.com/) e specificare il proprio indirizzo di posta elettronica aziendale o dell'istituto di istruzione. 

Il modo più comune con cui gli utenti verranno indirizzati a questa pagina per l'iscrizione è quando ricevono un messaggio di posta elettronica con un allegato protetto, che contiene istruzioni su come iscriversi. Gli utenti riceveranno un messaggio di posta elettronica di risposta da Microsoft e potranno quindi completare la procedura di iscrizione inserendo i dettagli per creare l'account. Quando riceveranno una conferma tramite posta elettronica da Microsoft, questo messaggio di posta elettronica finale li indirizzerà a una pagina in cui potranno scaricare l'applicazione di condivisione per dispositivi diversi e un collegamento alla Guida dell'utente.

## Per iscriversi per RMS per utenti singoli

1.  Usando un computer Windows o Mac, passare alla [pagina di Microsoft Rights Management](https://portal.aadrm.com).

2.  Digitare l'indirizzo di posta elettronica usato come indirizzo aziendale, ad esempio **janetm@contoso.com** o **p.dover@fabrikam.com**.

    > [!IMPORTANT]
    > Gli account di posta elettronica personali non sono supportati, pertanto non immettere un account Microsoft (definito in precedenza account Microsoft Live ID) né un altro account personale privato fornito dal proprio provider Internet.

3.  Fare clic sul pulsante **Introduzione**.

    Microsoft usa l'indirizzo di posta elettronica per verificare se l'organizzazione ha già una [sottoscrizione pagata che include Azure RMS](../get-started/requirements-subscriptions.md). In tal caso, non è necessario RMS per utenti singoli quindi si verrà connessi immediatamente e l’iscrizione self-service per RMS per utenti singoli verrà annullata. Se non viene trovata una sottoscrizione a pagamento per Azure RMS, si procederà al passaggio successivo.

4.  Attendere un messaggio di posta elettronica di conferma proveniente da Microsoft e inviato all'indirizzo di posta indicato Sarà proveniente da Microsoft (DoNotReply@microsoft.com) e riporterà l'oggetto **Microsoft RMS**..

5.  Quando si riceve il messaggio di posta elettronica, fare clic sul collegamento presente nelle istruzioni per completare il processo di iscrizione.

6.  Il collegamento consente di accedere a una nuova pagina di **Microsoft Rights Management** in cui indicare i dettagli relativi al proprio account. Digitare il nome, il cognome, immettere e confermare una password di propria scelta, selezionare il paese (o il paese più vicino al proprio se non è elencato) nell'elenco a discesa e quindi fare clic su **Crea**.

7.  Attendere un altro messaggio di posta elettronica di Microsoft che conferma che l'account è pronto per essere usato.

8.  Quando si riceve il messaggio, fare clic sul collegamento per accedere e leggere le istruzioni per scaricare e installare l'applicazione di condivisione oppure fare clic sul collegamento relativo alla guida per leggere la guida dell'utente dell'applicazione di condivisione.

Dopo avere creato l'account, è possibile iniziare a proteggere i file e a leggere quelli protetti da altri utenti. Quando viene chiesto di accedere per proteggere i file o leggere quelli protetti, immettere l'indirizzo di posta elettronica e la password usati per creare l'account per RMS per utenti singoli.

## Panoramica tecnica del processo di registrazione
RMS per utenti singoli usa un processo di iscrizione self-service impiegato anche da altri servizi che usano la tecnologia basata su Microsoft Cloud per autenticare gli utenti.

Questo è ciò che avviene in background quando un utente effettua l'iscrizione a RMS per utenti singoli e la sua organizzazione non dispone di un abbonamento a Office 365 o di una sottoscrizione di Azure e quindi non dispone di una directory in Azure per autenticare gli utenti:

1.  Quando il primo utente di un'organizzazione richiede una sottoscrizione di RMS per utenti singoli, il nome di dominio specificato nell'indirizzo di posta elettronica viene controllato per verificare se è già associato a un tenant Azure. Se non è presente alcun tenant esistente, viene creato automaticamente un nuovo tenant e una directory di Azure per l'organizzazione, che contiene un account per il primo utente. A differenza di una sottoscrizione di Azure a pagamento, questo primo account non è un amministratore globale, ma un utente standard. Il nuovo account usa l'indirizzo di posta elettronica e la password che l'utente ha specificato.

    > [!NOTE]
    > Alcuni nomi di dominio non possono essere usati per creare la directory e quindi neanche per RMS per utenti singoli. L'elenco dei nomi di dominio bloccati può essere visualizzato dal seguente file JavaScript Object Notation: [http://portal.aadrm.com/content/blocked_domains.json](http://portal.aadrm.com/content/blocked_domains.json)

    Se viene trovato un tenant esistente, viene controllato per sapere se dispone già di una sottoscrizione per Azure RMS. Quando non viene trovata alcuna sottoscrizione, è possibile aggiungere la sottoscrizione gratuita a RMS per utenti singoli.

2.  L'organizzazione riceve gratuitamente la sottoscrizione di RMS per utenti singoli. Ora, questo utente può essere autenticato da Azure e può proteggere i file e leggere i file protetti da altri utenti tramite Azure Rights Management. Per proteggere i file e leggere i file protetti, l'utente deve avere un'applicazione abilitata per RMS, ad esempio l'[applicazione di condivisione Rights Management](../rms-client/sharing-app-windows.md) gratuita.

3.  Quando un secondo utente della stessa organizzazione richiede una sottoscrizione di RMS per utenti singoli, viene aggiunto un nuovo account utente alla directory di Azure creata in precedenza, usando la sottoscrizione di RMS per utenti singoli dell'organizzazione. Questo secondo utente può eseguire tutte le operazioni che può eseguire il primo (proteggere file e leggere file protetti), ma i due utenti possono ora collaborare più facilmente in modo sicuro perché sono in grado di applicare rapidamente modelli predefiniti ai file per limitare l'accesso agli account presenti nella directory di Azure dell'organizzazione.

4.  Gli utenti della stessa organizzazione che successivamente richiedono la sottoscrizione seguono lo stesso schema, ovvero l'aggiunta di account utente (quando nuovi utenti eseguono l'iscrizione) alla directory di Azure dell'organizzazione. Maggiore è il numero di account aggiunto alla directory, maggiore è il numero di utenti che possono collaborare in modo sicuro con colleghi e partner e in grado di impedire con semplicità a persone non autorizzate di leggere i file qualora non dispongano dell'accesso ai file stessi.

Questo processo non prevede costi aggiuntivi per l'organizzazione né attività da parte dei membri del reparto IT, sebbene questi ultimi possano scegliere di effettuare una delle operazioni seguenti:

-   **Gestire gli account e il processo di iscrizione**: gli amministratori IT possono diventare proprietari della directory e degli account creati automaticamente in Azure. e quindi gestire gli account implementando le soluzioni di integrazione delle directory, ad esempio la sincronizzazione delle password e l'accesso Single Sign-On. In alternativa, possono impedire agli utenti di creare account o i iscriversi a RMS per utenti singoli.

    Per altre informazioni, vedere [Modalità di controllo da parte degli amministratori degli account creati per RMS per utenti singoli](rms-for-individuals-take-control.md).

-   **Gestire Rights Management**: gli amministratori IT possono convertire la sottoscrizione di RMS per utenti singoli per l'organizzazione in una sottoscrizione a pagamento che include Azure Rights Management. In questo caso, la directory e gli account Azure esistenti vengono mantenuti per consentire una semplice transizione agli utenti esistenti che usavano RMS per utenti singoli. Tutti i file protetti in precedenza dagli utenti rimarranno protetti in base agli stessi criteri e le persone cui era stato concesso di usare i file saranno ancora in grado di usarli nello stesso modo.

    Quando si sceglie questo tipo di comportamento, l'organizzazione è in grado di integrare Rights Management nei propri flussi di lavoro, servizi e archivi dati. In questo scenario è inoltre possibile gestire Rights Management perché si dispone del controllo sulla chiave del tenant dell'organizzazione per Azure Rights Management. A questo punto è possibile eseguire le operazioni seguenti:

    -   Configurare Exchange e SharePoint per supportare Azure Rights Management, anche se le due applicazioni sono eseguite in locale. Exchange e SharePoint sono supportate in modalità nativa per i servizi online e tramite un connettore per i server locali. Per altre informazioni, vedere i seguenti articoli:

        -   Le sezioni relative a Exchange Online e SharePoint Online in [Office 365: configurazione di client e servizi online](../deploy-use/configure-office365.md)

        -   [Distribuzione del connettore di Azure Rights Management](../deploy-use/deploy-rms-connector.md)

    -   Eseguire procedure di e-discovery sui dati di proprietà dell'azienda in modo che sia possibile, se richiesto, di decrittografare i file protetti tramite Rights Management. Per altre informazioni, vedere [Configuring super users for Azure Rights Management and discovery services or data eecovery (Configurazione degli utenti con privilegi avanzati per Azure Rights Management e servizi di individuazione o ripristino dei dati)](../deploy-use/configure-super-users.md).

    -   Registrare tutte le attività correlate all'uso di Rights Management nell'organizzazione. Questa operazione è estremamente utile perché consente non solo di monitorare i file protetti e gli utenti che vi hanno effettuato l'accesso, ma anche di identificare potenziali comportamenti sospetti di utenti non autorizzati che tentano di accedere a tali file. Per altre informazioni, vedere [Registrazione e analisi dell'utilizzo di Azure Rights Management](../deploy-use/log-analyze-usage.md).

    -   Fornire agli utenti la possibilità di tenere traccia e revocare i documenti protetti, se queste funzionalità sono supportate dalla [sottoscrizione Azure RMS](https://technet.microsoft.com/dn858608). Per altre informazioni, vedere la sezione [Rilevare e revocare i documenti quando si utilizza l'applicazione di condivisione RMS](../rms-client/sharing-app-track-revoke.md) in [Guida dell'utente dell'applicazione di condivisione Rights Management](../rms-client/sharing-app-user-guide.md).

    -   Implementare una soluzione BYOK in modo da generare la chiave del tenant per Azure Rights Management in locale in base ai propri criteri IT e di trasferirla in modo sicuro a Microsoft tramite un modulo di protezione hardware. Per altre informazioni, vedere [Pianificazione e implementazione della chiave del tenant di Azure Rights Management](../plan-design/plan-implement-tenant-key.md).


## Passaggi successivi
Vedere [Modalità di controllo da parte degli amministratori degli account creati per RMS per utenti singoli](rms-for-individuals-take-control.md).




<!--HONumber=Apr16_HO4-->


