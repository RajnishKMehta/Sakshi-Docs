# Architecture Flow

This flowchart illustrates the detailed sequence of interactions between a client application, the Sākṣī SDK, and the Vault service during a typical operation. The same calling application (Camera or Portal) sends the request and receives the resulting `SakshiResult` back through the SDK.

```mermaid
---
config:
  theme: base
  look: classic
  themeVariables:
    background: "#FFFFFF"
    primaryColor: "#EEF2FF"
    primaryTextColor: "#312E81"
    primaryBorderColor: "#6366F1"
    secondaryColor: "#CCFBF1"
    secondaryTextColor: "#134E4A"
    tertiaryColor: "#DBEAFE"
    lineColor: "#718096"
    textColor: "#334155"
    edgeLabelBackground: "#F1F5F9"
    clusterBkg: "#F8FAFC"
    clusterBorder: "#CBD5E1"
    fontSize: "14px"
    fontFamily: "Inter, Arial, sans-serif"
  flowchart:
    nodeSpacing: 52
    rankSpacing: 68
    diagramPadding: 24
    curve: basis
    htmlLabels: false
---
flowchart TB

    %% THE SAME CLIENT APP STARTS THE REQUEST AND RECEIVES THE RESULT
    ClientApp(["01 · CALLING CLIENT APP<br/>Sākṣī Camera / Portal<br/>Request caller and result receiver"])

    %% SĀKṢĪ SDK
    SakshiClient["02 · SĀKṢĪ SDK<br/>SakshiClientImpl"]
    CheckConn{"Vault Service<br/>Connected?"}
    BindService["bindService()<br/>Connect to Vault"]
    DispatchIO["withContext<br/>(Dispatchers.IO)"]
    Unpack["Unpack Bundle<br/>Map to Data Models"]


    %% ANDROID IPC
    BinderReq[("03 · ANDROID IPC LAYER<br/>AIDL Request<br/>ISakshiVaultService")]
    BinderRes[("Bundle Callback<br/>Response")]

    %% VAULT SERVICE
    VaultBinder["04 · VAULT SERVICE LAYER<br/>SakshiVaultServiceBinder"]
    Validate{"Validation<br/>& PathValidator"}
    Processing["CoroutineScope<br/>Request Processing"]
    PackBundle["Build Response Bundle<br/>Result / Error"]

    %% VAULT CORE STORAGE
    DB[("05 · VAULT CORE STORAGE<br/>Room3 Database<br/>MediaRecord")]
    FileStore[("Private File Storage")]
    WorkManager["WorkManager<br/>Background Tasks"]

    %% REQUEST FLOW
    ClientApp -->|"1 · SDK API Request"| SakshiClient
    SakshiClient -->|"2 · Check connection"| CheckConn

    CheckConn -->|"No"| BindService
    BindService -->|"Service Connected"| CheckConn
    CheckConn -->|"Yes"| DispatchIO

    DispatchIO -->|"3 · AIDL Method Call"| BinderReq
    BinderReq -->|"4 · IPC Request"| VaultBinder

    %% VAULT PROCESSING
    VaultBinder --> Validate
    Validate -->|"Valid"| Processing
    Validate -->|"Invalid"| PackBundle

    Processing <-->|"4a · Query / Update"| DB
    Processing <-->|"4b · Read / Write"| FileStore
    Processing -.->|"4c · Schedule Tasks"| WorkManager
    Processing -->|"5 · Processing Complete"| PackBundle

    %% RESPONSE FLOW
    PackBundle -->|"6 · Return Bundle"| BinderRes
    BinderRes -->|"7 · IPC Response"| Unpack
    Unpack -->|"8 · SakshiResult returned to the SAME caller"| ClientApp

    %% COLOR SYSTEM: INDIGO (CLIENT), VIOLET (SDK), AMBER (DECISIONS),
    %% ORANGE (IPC), TEAL (VAULT), BLUE (DATABASE), GREEN (BACKGROUND TASKS)
    classDef client fill:#E8EDFF,stroke:#6478E8,stroke-width:2.5px,color:#293B83
    classDef sdk fill:#EDE9FE,stroke:#8B78DB,stroke-width:2px,color:#4C3B91
    classDef sdkCore fill:#5145CD,stroke:#352AA5,stroke-width:3px,color:#FFFFFF
    classDef decision fill:#FFF2CC,stroke:#E6A23C,stroke-width:2.5px,color:#85520B
    classDef os fill:#FFF0E3,stroke:#F08A45,stroke-width:2.5px,color:#994516
    classDef vault fill:#DDF7F1,stroke:#36A995,stroke-width:2.5px,color:#17695E
    classDef storage fill:#E2EEFF,stroke:#6B9CEB,stroke-width:2.5px,color:#294F91
    classDef background fill:#E2F7E8,stroke:#59B879,stroke-width:2.5px,color:#24643A

    class ClientApp client
    class SakshiClient sdkCore
    class BindService,DispatchIO,Unpack sdk
    class CheckConn,Validate decision
    class BinderReq,BinderRes os
    class VaultBinder,Processing,PackBundle vault
    class DB,FileStore storage
    class WorkManager background

    %% Keep every connection legible against the pastel node palette.
    linkStyle default stroke:#718096,stroke-width:1.8px,color:#334155
```

## Key Interactions

1. **Client Request**: The client application (like Portal or Camera) calls a high-level `suspend` function on [`SakshiClient`](../../developer-reference/api/README.md).
2. **Connection Management**: The SDK's `VaultServiceConnection` checks if it's already bound to the Vault. If not, it uses Android's `bindService` to establish the connection before proceeding.
3. **Cross-Process Execution**: The SDK moves the actual AIDL execution to the `Dispatchers.IO` thread to avoid blocking the client's main thread during the IPC transaction.
4. **Vault Processing**: Once `SakshiVaultServiceBinder` receives the request inside the Vault process, it validates the inputs. It then performs the necessary operations using Room (database) and local file storage, either synchronously (`runBlocking` for fast database reads) or asynchronously (`CoroutineScope` for I/O).
5. **Response Translation**: The result from the Vault is packed into an Android `Bundle` and sent back over the Binder. The SDK unpacks this, mapping it into a clean [`SakshiResult.Success`](../../developer-reference/data-models.md#sakshiresultt) or [`SakshiResult.Failure`](../../developer-reference/data-models.md#sakshiresultt). That result is returned to the **same client application that initiated the request** (Camera or Portal), not to a separate application.
