---
# required metadata

title: Migrazione da AD RMS ad Azure Rights Management - Fase 1 | Azure RMS
description:
keywords:
author: cabailey
manager: mbaldwin
ms.date: 05/20/2016
ms.topic: article
ms.prod: azure
ms.service: rights-management
ms.technology: techgroup-identity
ms.assetid: 5a189695-40a6-4b36-afe6-0823c94993ef

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: esaggese
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# Fase 1 della migrazione: configurazione lato server per AD RMS

*Si applica a: Active Directory Rights Management Services, Azure Rights Management*

Usare le informazioni seguenti per la fase 1 della migrazione da AD RMS ad Azure Rights Management (Azure RMS). Queste procedure illustrano i passaggi da 1 a 4 di [Migrazione da AD RMS ad Azure Rights Management](migrate-from-ad-rms-to-azure-rms.md).


## Passaggio 1: Scaricare Azure Rights Management Administration Tool
Passare all'Area download Microsoft e [scaricare Azure Rights Management Administration Tool](http://go.microsoft.com/fwlink/?LinkId=257721), contenente il modulo di amministrazione di Azure RMS per Windows PowerShell.

Installare lo strumento. Per le istruzioni, vedere [Installazione di Windows PowerShell per Microsoft Azure Rights Management](../deploy-use/install-powershell.md).

## Passaggio 2: Esportare i dati di configurazione da AD RMS e importarli in Azure RMS
Questo passaggio è un processo costituito da due parti:

1.  Esportare i dati di configurazione da AD RMS esportando i domini di pubblicazione trusted in un file XML. Questo processo è identico per tutte le migrazioni.

2.  Importare i dati di configurazione in Azure RMS. Per questo passaggio esistono diversi processi, a seconda della configurazione della distribuzione corrente di AD RMS e della topologia preferita per la chiave del tenant di Azure RMS.

### Esportare i dati di configurazione da AD RMS.
Eseguire le operazioni seguenti in tutti i cluster AD RMS, per tutti i domini di pubblicazione trusted con contenuto protetto per l'organizzazione. Non è necessario eseguire questa procedura nei cluster usati per la sola gestione delle licenze.

> [!NOTE] Se si usa Windows Server 2003 Rights Management, invece di queste istruzioni seguire la procedura [Export SLC, TUD, TPD and RMS private key (Esportare SLC, TUD, TPD e chiave privata RMS)](http://technet.microsoft.com/library/jj835767%28v=ws.10%29.aspx) nell'articolo [Migrating from Windows RMS to AD RMS in a Different Infrastructure (Migrazione da Windows RMS ad AD RMS in un'infrastruttura diversa)](http://technet.microsoft.com/library/jj835767%28v=ws.10%29.aspx).

#### Per esportare i dati di configurazione (informazioni su domini di pubblicazione trusted)

1.  Accedere al cluster AD RMS come utente con autorizzazioni di amministratore di AD RMS.

2.  Dalla console di gestione di AD RMS (**Active Directory Rights Management Services**), espandere il nome del cluster AD RMS, espandere **Criteri di attendibilità**e quindi fare clic su **Domini di pubblicazione trusted**.

3.  Nel riquadro dei risultati selezionare il dominio di pubblicazione trusted e quindi nel riquadro Azioni fare clic su **Esporta dominio di pubblicazione Trusted**.

4.  Nella finestra di dialogo **Esporta dominio di pubblicazione trusted** :

    -   Fare clic su **Salva con nome** e salvare in un percorso e con un nome file a propria scelta. Assicurarsi di specificare **.xml** come estensione di file (non viene aggiunta automaticamente).

    -   Specificare e confermare una password complessa. Prendere nota della password, perché sarà necessaria in seguito per l'importazione dei dati di configurazione in Azure RMS.

    -   Non selezionare la casella di controllo per salvare il file di dominio trusted in RMS versione 1.0.

Dopo l'esportazione di tutti i domini di pubblicazione trusted, sarà possibile avviare la procedura di importazione dei dati nel modulo di protezione hardware (HSM) di Azure RMS da Thales. Per ulteriori informazioni, 

### Importare i dati di configurazione in Azure RMS
Le procedure esatte per questo passaggio dipendono dalla configurazione della distribuzione corrente di AD RMS e dalla topologia preferita per la chiave del tenant di Azure RMS.

La distribuzione corrente di AD RMS userà una delle seguenti configurazioni per la chiave del certificato concessore di licenze server (SLC):

-   Password di protezione nel database AD RMS. Questa è la configurazione predefinita.

-   Protezione tramite un modulo di protezione hardware Thales.

-   Modulo di protezione hardware tramite un modulo di protezione hardware di un fornitore diverso da Thales.

-   Protezione con password tramite un provider del servizio di crittografia esterno.

> [!NOTE] Per altre informazioni sull'uso di moduli di protezione hardware con AD RMS, vedere [Uso di AD RMS con moduli di protezione hardware](http://technet.microsoft.com/library/jj651024.aspx).

Per la topologia di chiave del tenant di Azure RMS esistono due opzioni: la chiave viene gestita da Microsoft (**gestione di Microsoft**) oppure dall'utente (**gestione del cliente**). Quando la chiave del tenant di Azure RMS è gestita dall'utente, viene a volte definita BYOK ("bring your own key") e richiede un modulo di protezione hardware di Thales. Per altre informazioni, vedere l’articolo [Pianificazione e implementazione della chiave del tenant di Azure Rights Management](plan-implement-tenant-key.md).

> [!IMPORTANT]
> Exchange Online non è attualmente compatibile con la modalità BYOK di Azure RMS.  Se si vuole usare la modalità BYOK dopo la migrazione e si prevede di usare Exchange Online, verificare come questa configurazione riduce la funzionalità IRM per Exchange Online. Rivedere le informazioni fornite in [Prezzi e restrizioni della modalità BYOK](byok-price-restrictions.md) per poter scegliere la migliore topologia di chiave del tenant di Azure RMS per la migrazione.

Usare la tabella seguente per identificare la procedura da eseguire per la migrazione. Le combinazioni non elencate non sono supportate.

|Distribuzione di AD RMS corrente|Topologia di chiave del tenant di Azure RMS scelta|Istruzioni relative alla migrazione|
|-----------------------------|----------------------------------------|--------------------------|
|Password di protezione nel database AD RMS|Gestione di Microsoft|Vedere la procedura **Migrazione da una chiave protetta tramite software a un'altra** dopo questa tabella.<br /><br />Questo è il percorso di migrazione più semplice e richiede solo il trasferimento dei dati di configurazione in Azure RMS.|
|Modulo di protezione hardware tramite un modulo di protezione hardware nShield di Thales|Gestione del cliente (scenario BYOK)|Vedere la procedura **Migrazione da una chiave protetta tramite HSM a un'altra** dopo questa tabella.<br /><br />Sono necessari il set di strumenti BYOK e due procedure per trasferire la chiave dal modulo di protezione hardware locale ai moduli di protezione hardware di Azure RMS e quindi trasferire i dati di configurazione in Azure RMS.|
|Password di protezione nel database AD RMS|Gestione del cliente (scenario BYOK)|Vedere la procedura di migrazione **Da una chiave protetta tramite software a una chiave protetta tramite HSM** dopo questa tabella.<br /><br />Sono necessari il set di strumenti BYOK e tre procedure, innanzitutto per estrarre la chiave software e importarla nel modulo di protezione hardware locale, quindi trasferire la chiave dal modulo di protezione hardware locale ai moduli di protezione hardware di Azure RMS e infine trasferire i dati di configurazione in Azure RMS.|
|Modulo di protezione hardware tramite un modulo di protezione hardware di un fornitore diverso da Thales.|Gestione del cliente (scenario BYOK)|Contattare il fornitore del modulo di protezione hardware per istruzioni sul trasferimento della chiave da questo modulo di protezione hardware a un modulo di protezione hardware nShield di Thales. Seguire quindi le istruzioni per la procedura di migrazione **Da una chiave protetta tramite HSM a un’altra** dopo questa tabella.|
|Protezione con password tramite un provider del servizio di crittografia esterno|Gestione del cliente (scenario BYOK)|Contattare il fornitore del provider del servizio di crittografia per istruzioni sul trasferimento della chiave a un modulo di protezione hardware nShield di Thales. Seguire quindi le istruzioni per la procedura di migrazione **Da una chiave protetta tramite HSM a un’altra** dopo questa tabella.|
Prima di iniziare queste procedure, assicurarsi che sia possibile accedere ai file con estensione XML creati in precedenza durante l'esportazione dei domini di pubblicazione trusted. Ad esempio, è possibile salvarli su una chiavetta USB che può essere spostata dal server AD RMS alla workstation connessa a Internet.

> [!NOTE] Indipendentemente dalla modalità di archiviazione di questi file, per proteggerli attenersi alle procedure consigliate per la sicurezza, poiché tali dati includono la chiave privata.


Per completare il passaggio 2, scegliere e selezionare le istruzioni per il percorso di migrazione: 


- [Da una chiave protetta tramite software a un’altra](migrate-softwarekey-to-softwarekey.md)
- [Da una chiave protetta tramite HSM a un’altra](migrate-hsmkey-to-hsmkey.md)
- [Da una chiave protetta tramite software a una chiave protetta tramite HSM](migrate-softwarekey-to-hsmkey.md)


## Passaggio 3. Attivare il tenant di RMS
Le istruzioni per questo passaggio sono descritte in dettaglio nell’articolo [Attivazione di Azure Rights Management](../deploy-use/activate-service.md).


> [!TIP]
> Se si ha una sottoscrizione a Office 365, è possibile attivare Azure RMS dall'interfaccia di amministrazione di Office 365 o nel portale di Azure classico. È consigliabile usare il portale di Azure classico perché verrà usato per completare il passaggio successivo.

**Cosa accade se il tenant di Azure RMS è già attivato?** Se il servizio Azure RMS è già attivato per l'organizzazione, gli utenti potrebbero avere già usato Azure RMS per proteggere il contenuto con una chiave del tenant generata automaticamente (e con i modelli predefiniti) invece che con le chiavi (e i modelli) esistenti da AD RMS. È improbabile che ciò accada su computer ben gestiti nella Intranet, perché verranno automaticamente configurati per l'infrastruttura AD RMS. Può però verificarsi su computer di un gruppo di lavoro o su computer che non si connettono spesso alla Intranet. Dal momento che sfortunatamente è anche difficile identificare questi computer, si consiglia di non attivare il servizio prima di importare i dati di configurazione da AD RMS.

Se il tenant di Azure RMS è già attivato ed è possibile identificare questi computer, assicurarsi di eseguire lo script CleanUpRMS_RUN_Elevated.cmd su questi computer, come descritto nel passaggio 5. L'esecuzione dello script li obbliga a reinizializzare l'ambiente utente e quindi a scaricare la chiave del tenant aggiornata e i modelli importati.

## Passaggio 4. Configurare i modelli importati
Poiché i modelli importati hanno uno stato predefinito **Archiviato**, è necessario impostare questo stato su **Pubblicato** se si vuole consentire agli utenti di usarli con Azure RMS.

Se i modelli in AD RMS usavano il gruppo **ANYONE**, questo gruppo viene rimosso automaticamente quando si importano i modelli in Azure RMS. È necessario aggiungere manualmente il gruppo o gli utenti equivalenti e gli stessi diritti ai modelli importati. Il gruppo equivalente per Azure RMS viene denominato **AllStaff-7184AB3F-CCD1-46F3-8233-3E09E9CF0E66@<nome_tenant>.onmicrosoft.com**. Ad esempio, questo gruppo potrebbe essere simile al seguente per Contoso: **AllStaff-7184AB3F-CCD1-46F3-8233-3E09E9CF0E66@contoso.onmicrosoft.com**.

Se non si è certi che i modelli AD RMS includano il gruppo ANYONE, è possibile usare lo script di Windows PowerShell di esempio per identificare questi modelli. Per altre informazioni sull'uso di Windows PowerShell con AD RMS, vedere l'articolo relativo all'[uso di Windows PowerShell per amministrare AD RMS](https://technet.microsoft.com/library/ee221079%28v=ws.10%29.aspx).

È possibile vedere il gruppo creato automaticamente dell'organizzazione copiando uno dei modelli di criteri per i diritti predefiniti nel portale di Azure classico e quindi identificando il **NOME UTENTE** nella pagina **DIRITTI**. Non è tuttavia possibile usare il portale di Azure classico per aggiungere questo gruppo a un modello creato manualmente o importato. È invece necessario usare una delle opzioni di Azure RMS PowerShell seguenti:

-   Usare il cmdlet di PowerShell [New-AadrmRightsDefinition](https://msdn.microsoft.com/library/azure/dn727080.aspx) per definire il gruppo "AllStaff" e i diritti come un oggetto di definizione dei diritti ed eseguire di nuovo questo comando per ognuno degli altri gruppi o utenti a cui sono già stati concessi diritti nel modello originale, in aggiunta al gruppo ANYONE. Aggiungere quindi tutti questi oggetti di definizione dei diritti ai modelli con il cmdlet [Set-AadrmTemplateProperty](https://msdn.microsoft.com/en-us/library/azure/dn727076.aspx).

-   Usare il cmdlet [Export-AadrmTemplate](https://msdn.microsoft.com/library/azure/dn727078.aspx) per esportare il modello in un file con estensione XML che è possibile modificare per aggiungere il gruppo "AllStaff" e i diritti ai gruppi e ai diritti esistenti. Usare quindi il cmdlet [Import-AadrmTemplate](https://msdn.microsoft.com/library/azure/dn727077.aspx) per importare di nuovo le modifiche in Azure RMS.

> [!NOTE] Questo gruppo equivalente "AllStaff" non corrisponde esattamente al gruppo ANYONE in AD RMS: il gruppo "AllStaff" include tutti gli utenti nel tenant di Azure, mentre il gruppo ANYONE comprende tutti gli utenti autenticati che potrebbero essere esterni all'organizzazione.
> 
> A causa di questa differenza tra i due gruppi, potrebbe essere necessario aggiungere anche gli utenti esterni in aggiunta al gruppo "AllStaff". Gli indirizzi email esterni per i gruppi attualmente non sono supportati.

I modelli importati da AD RMS hanno lo stesso aspetto e comportamento dei modelli personalizzati che è possibile creare nel portale di Azure classico. Per modificare i modelli importati pubblicandoli in modo che gli utenti possano visualizzarli e selezionarli dalle applicazioni o per apportarvi altre modifiche, vedere [Configurazione di modelli personalizzati per Azure Rights Management](../deploy-use/configure-custom-templates.md).

> [!TIP]
> Per un'esperienza più coerente per gli utenti durante il processo di migrazione, non apportare modifiche ai modelli importati oltre a queste due. Non pubblicare i due modelli predefiniti forniti con Azure RMS, né creare nuovi modelli in questa fase. Attendere invece il completamento del processo di migrazione e la rimozione delle autorizzazioni dei server AD RMS.

### Esempio di script di Windows PowerShell per identificare modelli AD RMS che includono il gruppo ANYONE
Questa sezione include lo script di esempio per facilitare l'identificazione dei modelli AD RMS per i quali è definito un gruppo ANYONE, come descritto nella sezione precedente.

**Dichiarazione di non responsabilità:** questo script di esempio non è supportato in alcun programma o servizio di supporto standard Microsoft. Questo script di esempio viene fornito "nello stato in stato in cui si trova" senza garanzia di alcun tipo.*

```
import-module adrmsadmin 

New-PSDrive -Name MyRmsAdmin -PsProvider AdRmsAdmin -Root https://localhost -Force 

$ListofTemplates=dir MyRmsAdmin:\RightsPolicyTemplate

foreach($Template in $ListofTemplates) 
{ 
                $templateID=$Template.id

                $rights = dir MyRmsAdmin:\RightsPolicyTemplate\$Templateid\userright

     $templateName=$Template.DefaultDisplayName 

        if ($rights.usergroupname -eq "anyone")

                         {
                           $templateName = $Template.defaultdisplayname

                           write-host "Template " -NoNewline

                           write-host -NoNewline $templateName -ForegroundColor Red

                           write-host " contains rights for " -NoNewline

                           write-host ANYONE  -ForegroundColor Red
                         }
 } 
Remove-PSDrive MyRmsAdmin -force
```


## Passaggi successivi
Passare alla [fase 2: configurazione lato client](migrate-from-ad-rms-phase2.md).



<!--HONumber=May16_HO3-->


