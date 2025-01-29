**Robotic Enterprise Framework**

built on top of Transactional Business Process template
using State Machine layout for the phases of automation project
offering high level exception handling and application recovery
keeps external settings in Config.xlsx file and Orchestrator assets
pulls credentials from Credential Manager and Orchestrator assets
gets transaction data from Orchestrator queue and updates back status
takes screenshots in case of application exceptions
provides extra utility workflows like sending a templated email
runs sample Notepad application with dummy Excel input data

	**How It Works:**
 
**INITIALIZE PROCESS**
InitiAllSettings - Load config data from file and from assets
InitiAllApplications - Login to applications as per Config("OpenApps") field
GetAppCredentials - From Orchestrator assets or local Credential Manager
If failing, retry a few times as per Config("ProcessRetries")

**GET TRANSACTION DATA**
./Framework/GetTransactionData - Fetches from Orchestrator queue as per Config("TransactionQueue")
./GetTransactionData - Sample for working with Excel input files

**PROCESS TRANSACTION**
Try ProcessTransaction
If application exceptions happen
SaveErrorScreen - In Config("ErrorsFolder") with the exception message
Going to re/INITIALIZE
SetTransactionStatus - As Success, Failed or Rejected with reason
./Framework/SetTransactionStatus - Updates the Orchestrator queue item
./SetTransactionStatus - Sample for updating Excel input file

**END PROCESS**
CloseAllApplications - As listed in Config("CloseApps")
