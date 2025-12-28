# Anime & Manga Discord Bot

A Discord bot that allows users to search for anime and manga information using slash commands. The bot fetches data from external APIs and provides detailed information about your favorite anime and manga series.

## Features

- 🎌 **Anime Search**: Search for anime by name with detailed information
- 📚 **Manga Search**: Search for manga by name with comprehensive details
- ⚡ **Slash Commands**: Modern Discord slash command interface
- 🔍 **Flexible Results**: Choose to get all results or a specific number
- 🛡️ **Rate Limiting**: Built-in protection against API rate limits
- 🎭 **Auto Role Creation**: Automatically creates roles for new servers

## Commands

### Standard Commands

- **`/anime <name> <number_search>`**  
  Search for anime information  
  • `name`: Name of the anime  
  • `number_search`: "all" for all results or a number for specific count  
  **Example**: `/anime "Attack on Titan" 3`

- **`/manga <name> <number_search>`**  
  Search for manga information  
  • `name`: Name of the manga  
  • `number_search`: "all" for all results or a number for specific count  
  **Example**: `/manga "One Piece" all`

- **`/characterinfo <character>`**  
  Search for detailed info about anime/manga characters.

- **`/randomanime`**  
  Suggest a random anime based you preference note or theme

- **`/help`**  
  Display this help message

- **`/terms`**  
  View Terms and Conditions

- **`/privacy`**  
  View Privacy Policy

- **`/setup-channel`**  
  Create and configure bot channels (Admin only)

- **`/buypremium`**  
  Upgrade to premium for exclusive features!

---

### Premium Commands (Subscription required)

- **`/recommend`**  
  Get personalized anime/manga recommendations based on your search history.

- **`/trending`**  
  Show currently trending anime/manga.

- **`/compare <title1> <title2>`**  
  Compare two anime/manga titles by popularity.

- **`/watchlist`**  
  Manage your personal anime/manga watchlist.

- **`/notify`**  
  Get notifications for new episodes or manga chapters.


- **`/advancedanime <genre> <year> <rating> <studio> <count>`**  
  Advanced search for anime.

- **`/advancedmanga <genre> <year> <rating> <author> <count>`**  
  Advanced search for manga.

- **`/advancedcharacter <name> <count>`**  
  Advanced search for anime/manga characters.

- **`/stats`**  
  View your search history and stats.

---

> Premium commands are only available to users who have purchased premium. Use `/buypremium` to unlock all features!

## Required Discord Permissions

To function correctly, the bot requires the following permissions in your server:

- **Send Messages**: Allows the bot to send messages in channels.
- **Use Slash Commands**: Enables the bot to register and use slash commands.
- **Embed Links**: Lets the bot send rich embedded messages.
- **View Channels**: Allows the bot to see and interact in channels.
- **Manage Roles**: Required for automatic role creation and management.
- **Manage Channels**: Needed to create and configure dedicated bot channels (e.g., `#anime-manga`).
- **Read Message History**: Lets the bot read previous messages for context and logging.

> Make sure to grant these permissions when inviting the bot to your server.  
> Example invite link includes all necessary permissions:  
> `https://discord.com/api/oauth2/authorize?client_id=473135420608348162&permissions=274877921280&scope=bot%20applications.commands`


## API Information

This bot uses the [Jikan API](https://jikan.moe/) to fetch anime and manga data from MyAnimeList. The API is free and doesn't require authentication, but has rate limiting in place.

```mermaid
graph LR
    %% User Interactions Region
    subgraph USER_REGION["👤 User Interactions"]
        U[Discord User] 
        DC[Discord Client]
        U --> |Slash Commands| DC
        U --> |Message Mentions| DC
    end
    
    %% Bot Initialization Region
    subgraph INIT_REGION["🚀 Bot Initialization"]
        START[Bot Start] 
        INIT_DB[Initialize Database]
        LIMITED[Limited Functionality Mode]
        INIT_NOTIFY[Initialize Notification System]
        READY[Bot Ready Event]
        REG_CMD[Register Slash Commands]
        SET_ACTIVITY[Set Bot Activity]
        
        START --> INIT_DB
        INIT_DB --> |Success| INIT_NOTIFY
        INIT_DB --> |Failure| LIMITED
        INIT_NOTIFY --> READY
        READY --> REG_CMD
        READY --> SET_ACTIVITY
    end
    
    %% Command Processing Region
    subgraph CMD_PROCESS_REGION["⚙️ Command Processing Pipeline"]
        INT_CREATE[Interaction Create Event]
        CMD_CHECK{Command Type?}
        CHANNEL_CHECK{Allowed Channel?}
        FIND_CHANNEL[Find/Create anime-manga Channel]
        REDIRECT[Redirect User to Correct Channel]
        PREMIUM_CHECK{Premium Command?}
        
        INT_CREATE --> CMD_CHECK
        CMD_CHECK --> |Other Commands| CHANNEL_CHECK
        CHANNEL_CHECK --> |No| FIND_CHANNEL
        FIND_CHANNEL --> REDIRECT
        CHANNEL_CHECK --> |Yes| PREMIUM_CHECK
    end
    
    %% Premium Purchase Flow Region
    subgraph PAYMENT_REGION["💳 Payment & Premium Management"]
        BUY_PREMIUM[Buy Premium Command]
        STRIPE_CHECKOUT[Create Stripe Checkout]
        PAYMENT[User Payment]
        WEBHOOK[Stripe Webhook]
        VERIFY_SIG[Verify Webhook Signature]
        PROCESS_EVENT{Event Type?}
        WEBHOOK_ERROR[Return 400 Error]
        UPDATE_PREMIUM[Update Premium Status]
        REMOVE_PREMIUM[Remove Premium Status]
        ASSIGN_ROLES[Assign Premium Roles Across Servers]
        REMOVE_ROLES[Remove Premium Roles Across Servers]
        SEND_WELCOME[Send Welcome DM]
        SEND_GOODBYE[Send Cancellation DM]
        
        CMD_CHECK --> |buypremium| BUY_PREMIUM
        BUY_PREMIUM --> STRIPE_CHECKOUT
        STRIPE_CHECKOUT --> PAYMENT
        PAYMENT --> WEBHOOK
        WEBHOOK --> VERIFY_SIG
        VERIFY_SIG --> |Valid| PROCESS_EVENT
        VERIFY_SIG --> |Invalid| WEBHOOK_ERROR
        PROCESS_EVENT --> |checkout.session.completed| UPDATE_PREMIUM
        PROCESS_EVENT --> |customer.subscription.deleted| REMOVE_PREMIUM
        UPDATE_PREMIUM --> ASSIGN_ROLES
        ASSIGN_ROLES --> SEND_WELCOME
        REMOVE_PREMIUM --> REMOVE_ROLES
        REMOVE_ROLES --> SEND_GOODBYE

    %% Replaced: Simplified Payment Flow (ref. top diagram)
    %% Top snippet (used elsewhere in README) updated to show Stripe Checkout → Stripe infra → Azure Function → MongoDB Atlas
    %% New concise flow is documented in the Payment Tunnel & Synchronization section as a dedicated mermaid block.
    end
    
    %% Rate Limiting Region
    subgraph RATE_LIMIT_REGION["🛡️ Rate Limiting System"]
        IS_PREMIUM{User Premium?}
        PREMIUM_DENY[Deny Access - Show Upgrade Message]
        RATE_LIMIT_PREMIUM[Rate Limit Check for Premium]
        RATE_LIMIT_CHECK[Rate Limit Check]
        GLOBAL_CHECK[Check Global API Limit]
        GLOBAL_DENY[Deny - Global Limit Message]
        SERVER_CHECK[Check Server API Limit]
        SERVER_DENY[Deny - Server Limit Message]
        USER_CHECK{User Type?}
        PREMIUM_QUOTA[Check Premium Quota - 50/hour]
        FREE_QUOTA[Check Dynamic Free Quota]
        PREMIUM_LIMIT[Deny - Premium Limit Message]
        DAILY_CHECK[Check Daily Limit]
        DAILY_DENY[Deny - Daily Limit + Upgrade Message]
        HOURLY_CHECK[Check Hourly Limit]
        HOURLY_DENY[Deny - Hourly Limit + Upgrade Message]
        
        PREMIUM_CHECK --> |Yes| IS_PREMIUM
        IS_PREMIUM --> |No| PREMIUM_DENY
        IS_PREMIUM --> |Yes| RATE_LIMIT_PREMIUM
        PREMIUM_CHECK --> |No| RATE_LIMIT_CHECK
        RATE_LIMIT_PREMIUM --> RATE_LIMIT_CHECK
        RATE_LIMIT_CHECK --> GLOBAL_CHECK
        GLOBAL_CHECK --> |Exceeded| GLOBAL_DENY
        GLOBAL_CHECK --> |OK| SERVER_CHECK
        SERVER_CHECK --> |Exceeded| SERVER_DENY
        SERVER_CHECK --> |OK| USER_CHECK
        USER_CHECK --> |Premium| PREMIUM_QUOTA
        USER_CHECK --> |Free| FREE_QUOTA
        PREMIUM_QUOTA --> |Exceeded| PREMIUM_LIMIT
        FREE_QUOTA --> DAILY_CHECK
        DAILY_CHECK --> |Exceeded| DAILY_DENY
        DAILY_CHECK --> |OK| HOURLY_CHECK
        HOURLY_CHECK --> |Exceeded| HOURLY_DENY
    end
    
    %% Command Execution Region
    subgraph EXEC_REGION["🎯 Command Execution"]
        EXECUTE_CMD[Execute Command]
        CMD_TYPE{Command Type?}
        CONTENT_FILTER[Content Filter Check]
        FILTER_DENY[Deny - Inappropriate Content]
        
        PREMIUM_QUOTA --> |OK| EXECUTE_CMD
        HOURLY_CHECK --> |OK| EXECUTE_CMD
        EXECUTE_CMD --> CMD_TYPE
        CMD_TYPE --> |anime/manga| CONTENT_FILTER
        CONTENT_FILTER --> |Blocked| FILTER_DENY
    end
    
    %% API Operations Region
    subgraph API_REGION["🌐 External API Operations"]
        API_CALL[Jikan API Call]
        CHAR_API[Character API Call]
        INFO_API[Info API Call]
        TREND_API[Trending API Call]
        POLL_API[Poll for New Releases]
        JIKAN[Jikan API v4]
        
        CONTENT_FILTER --> |OK| API_CALL
        CMD_TYPE --> |character| CHAR_API
        CMD_TYPE --> |trending| TREND_API
        API_CALL --> JIKAN
        CHAR_API --> JIKAN
        INFO_API --> JIKAN
        TREND_API --> JIKAN
        POLL_API --> JIKAN
    end
    
    %% Data Processing Region
    subgraph DATA_REGION["📊 Data Processing & Results"]
        PROCESS_RESULTS[Process & Filter Results]
        CHAR_RESULTS[Process Character Results]
        INFO_CACHE{Cache Hit?}
        SEND_CACHED[Send Cached Info]
        CACHE_INFO[Cache Info]
        SEND_RESULTS[Send Results to User]
        SEND_CHAR[Send Character Info]
        SEND_INFO[Send Info]
        SEND_TREND[Send Trending Results]
        
        API_CALL --> PROCESS_RESULTS
        CHAR_API --> CHAR_RESULTS
        CMD_TYPE --> |info| INFO_CACHE
        INFO_CACHE --> |Yes| SEND_CACHED
        INFO_CACHE --> |No| INFO_API
        INFO_API --> CACHE_INFO
        CACHE_INFO --> SEND_INFO
        PROCESS_RESULTS --> SEND_RESULTS
        CHAR_RESULTS --> SEND_CHAR
        TREND_API --> SEND_TREND
    end
    
    %% Premium Features Region
    subgraph PREMIUM_FEATURES_REGION["💎 Premium Features"]
        REC_HISTORY[Get User Search History]
        REC_ALGO[Recommendation Algorithm]
        SEND_REC[Send Recommendations]
        WL_DB[Watchlist Database Operations]
        SEND_WL[Send Watchlist Response]
        NOTIFY_DB[Update Notification Preferences]
        SEND_NOTIFY[Send Notification Confirmation]
        
        CMD_TYPE --> |recommend| REC_HISTORY
        REC_HISTORY --> REC_ALGO
        REC_ALGO --> SEND_REC
        CMD_TYPE --> |watchlist| WL_DB
        WL_DB --> SEND_WL
        CMD_TYPE --> |notify| NOTIFY_DB
        NOTIFY_DB --> SEND_NOTIFY
    end
    
    %% Admin Commands Region
    subgraph ADMIN_REGION["👑 Admin Commands"]
        ADMIN_CHECK{Admin Permission?}
        ADMIN_DENY[Deny - Admin Required]
        CREATE_CHANNELS[Create anime-manga Channel]
        CREATE_ROLES[Create Premium Role]
        SETUP_COMPLETE[Send Setup Complete Message]
        QUOTA_ADMIN{Admin Permission?}
        QUOTA_DENY[Deny - Admin Required]
        QUOTA_CALC[Calculate Current Quotas]
        SEND_QUOTA[Send Quota Status]
        SYNC_ADMIN{Admin Permission?}
        SYNC_DENY[Deny - Admin Required]
        SYNC_DB[Get Premium Users from DB]
        SYNC_ROLES[Sync Premium Roles]
        SYNC_COMPLETE[Send Sync Complete]
        
        CMD_TYPE --> |setup-channel| ADMIN_CHECK
        ADMIN_CHECK --> |No| ADMIN_DENY
        ADMIN_CHECK --> |Yes| CREATE_CHANNELS
        CREATE_CHANNELS --> CREATE_ROLES
        CREATE_ROLES --> SETUP_COMPLETE
        CMD_TYPE --> |quotastatus| QUOTA_ADMIN
        QUOTA_ADMIN --> |No| QUOTA_DENY
        QUOTA_ADMIN --> |Yes| QUOTA_CALC
        QUOTA_CALC --> SEND_QUOTA
        CMD_TYPE --> |syncpremium| SYNC_ADMIN
        SYNC_ADMIN --> |No| SYNC_DENY
        SYNC_ADMIN --> |Yes| SYNC_DB
        SYNC_DB --> SYNC_ROLES
        SYNC_ROLES --> SYNC_COMPLETE
    end
    
    %% Database Operations Region
    subgraph DATABASE_REGION["🗄️ Database Operations"]
        DB_UPDATE[Database Update]
        DB_REMOVE[Database Update]
        LOG_SEARCH[Log Search to Database]
        SAFE_DB[Safe Database Operation]
        ENSURE_CONNECTION[Ensure DB Connection]
        DB_OPERATION[Execute DB Operation]
        RECONNECT_DB[Reconnect Database]
        DB_ERROR[Return Database Error]
        MONGODB[(MongoDB Database)]
        
        UPDATE_PREMIUM --> DB_UPDATE
        REMOVE_PREMIUM --> DB_REMOVE
        PROCESS_RESULTS --> LOG_SEARCH
        DB_UPDATE --> SAFE_DB
        LOG_SEARCH --> SAFE_DB
        REC_HISTORY --> SAFE_DB
        WL_DB --> SAFE_DB
        NOTIFY_DB --> SAFE_DB
        SYNC_DB --> SAFE_DB
        SAFE_DB --> ENSURE_CONNECTION
        ENSURE_CONNECTION --> |Connected| DB_OPERATION
        ENSURE_CONNECTION --> |Disconnected| RECONNECT_DB
        RECONNECT_DB --> |Success| DB_OPERATION
        RECONNECT_DB --> |Failed| DB_ERROR
        SAFE_DB --> MONGODB
    end
    
    %% Background Processes Region
    subgraph BACKGROUND_REGION["🔄 Background Processes"]
        CLEANUP_TIMER[Start Cleanup Timer - 10min]
        CLEANUP[Clean Expired Rate Limits]
        HEALTH_TIMER[Start Health Check - 5min]
        DB_HEALTH[Database Health Check]
        RECONNECT[Attempt Reconnection]
        POLL_TIMER[Start Polling Timer - 5min]
        CHECK_SUBSCRIPTIONS[Check User Subscriptions]
        MATCH_CRITERIA[Match Release Criteria]
        SEND_NOTIFICATIONS[Send Notifications to Users]
        
        READY --> CLEANUP_TIMER
        CLEANUP_TIMER --> CLEANUP
        CLEANUP --> CLEANUP_TIMER
        READY --> HEALTH_TIMER
        HEALTH_TIMER --> DB_HEALTH
        DB_HEALTH --> |Failed| RECONNECT
        DB_HEALTH --> |Success| HEALTH_TIMER
        RECONNECT --> HEALTH_TIMER
        INIT_NOTIFY --> POLL_TIMER
        POLL_TIMER --> POLL_API
        POLL_API --> CHECK_SUBSCRIPTIONS
        CHECK_SUBSCRIPTIONS --> MATCH_CRITERIA
        MATCH_CRITERIA --> SEND_NOTIFICATIONS
        SEND_NOTIFICATIONS --> POLL_TIMER
    end
    
    %% Info Commands Region
    subgraph INFO_REGION["ℹ️ Information Commands"]
        SEND_HELP[Send Help Embed]
        SEND_TERMS[Send Terms & Conditions]
        SEND_PRIVACY[Send Privacy Policy]
        SEND_PING[Send Pong Response]
        
        CMD_TYPE --> |help| SEND_HELP
        CMD_TYPE --> |terms| SEND_TERMS
        CMD_TYPE --> |privacy| SEND_PRIVACY
        CMD_TYPE --> |ping| SEND_PING
    end
    
    %% External Services Region
    subgraph EXTERNAL_REGION["🔗 External Services"]
        STRIPE[Stripe Payment Service]
        
        BUY_PREMIUM --> STRIPE
        STRIPE --> STRIPE_CHECKOUT
    end
    
    %% Error Handling Region
    subgraph ERROR_REGION["❌ Error Handling"]
        LOG_ERROR[Log Error]
        
        DB_ERROR --> LOG_ERROR
        WEBHOOK_ERROR --> LOG_ERROR
        GLOBAL_DENY --> LOG_ERROR
    end
    
    %% Guild Events Region
    subgraph GUILD_REGION["🏰 Guild Events"]
        GUILD_JOIN[New Guild Joined]
        REG_GUILD_CMD[Register Guild Commands]
        WELCOME_MSG[Send Welcome Message]
        
        GUILD_JOIN --> REG_GUILD_CMD
        REG_GUILD_CMD --> WELCOME_MSG
    end
    
    %% Main Flow Connections
    DC --> INT_CREATE
    CHECK_SUBSCRIPTIONS --> SAFE_DB
    
    %% Styling
    classDef userAction fill:#e1f5fe,stroke:#01579b,stroke-width:2px
    classDef botProcess fill:#f3e5f5,stroke:#4a148c,stroke-width:2px
    classDef database fill:#e8f5e8,stroke:#1b5e20,stroke-width:2px
    classDef external fill:#fff3e0,stroke:#e65100,stroke-width:2px
    classDef premium fill:#fff8e1,stroke:#ff6f00,stroke-width:2px
    classDef error fill:#ffebee,stroke:#c62828,stroke-width:2px
    classDef admin fill:#f1f8e9,stroke:#33691e,stroke-width:2px
    
    class U,DC userAction
    class INIT_DB,READY,INT_CREATE,EXECUTE_CMD botProcess
    class MONGODB,SAFE_DB,DB_UPDATE,LOG_SEARCH database
    class JIKAN,STRIPE external
    class IS_PREMIUM,PREMIUM_QUOTA,UPDATE_PREMIUM premium
    class WEBHOOK_ERROR,DB_ERROR,LOG_ERROR error
    class ADMIN_CHECK,QUOTA_ADMIN,SYNC_ADMIN admin
```


## Architecture & Business Logic

To guarantee the reliability of a large-scale freemium service, the bot relies on an event-driven and serverless architecture. This structure isolates the payment logic from the search logic for maximum security.

1. **Payment Tunnel & Synchronization (Stripe Integration)**

   Subscription management (€4.99 excl. VAT/month) is fully automated to avoid any manual intervention:

   - **Checkout Flow**: The bot generates a unique Stripe Checkout session linked to the user's Discord ID.
   - **Webhooks Security**: A dedicated Azure Function listens for Stripe events. Each request is authenticated via validation of the Stripe signature header to prevent any fraudulent payments.
   - **Reconciliation Process**: To mitigate potential network failures, a Cron Job (Azure Timer Trigger) compares the status of Stripe subscriptions daily with the `subscriptionStatus` entries in MongoDB Atlas.

2. **Performance Optimization (API & Caching)**

   The bot interacts with external APIs (MyAnimeList/Jikan, AniList) while adhering to high availability requirements:

   - **Caching Strategy**: Implementation of an in-memory cache for lightweight responses and a long-term store in MongoDB Atlas for subscription-related records.
   - **Payment Flow Diagram**: See the simplified flow below (Stripe Checkout → Stripe Infrastructure → Azure Function → MongoDB Atlas). The diagram is included as code and will not be rendered in preview.

```mermaid
graph TD
    %% User Action
    U[👤 Utilisateur Discord] -->|Commande /buypremium| B[🤖 Bot Discord]

    %% Payment Flow
    B -->|Génère lien| S[💳 Stripe Checkout]
    S -->|Paiement Validé| SP[🏦 Infrastructure Stripe]

    %% Asynchronous Processes
    SP -->|Envoi Facture PDF| U
    SP -->|Webhook Event| AF[⚡ Azure Function]

    %% Business Logic
    AF -->|Validation Signature| AF
    AF -->|Update subscriptionStatus| DB[(🗄️ MongoDB Atlas)]
    DB -->|Notification Succès| B
    B -->|Confirmation & Rôles| U

    %% Styling
    style SP fill:#6772e5,color:#fff
    style AF fill:#0078d4,color:#fff
    style DB fill:#47a248,color:#fff
```

   - **Rate Limit Management**: Implementation of an intelligent fallback system: if the Jikan API approaches its limit (30 requests/min), the service automatically switches to the AniList GraphQL API to ensure service continuity.

3. **Security & Confidentiality (Privacy by Design)**

   - **Secrets Management**: All API keys and Stripe secrets are encrypted and stored in Azure Key Vault, never in plain text in the runtime environment.
   - **Data Minimization (GDPR)**: The data schema is limited to the bare minimum (Discord IDs and payment status). Data is automatically purged when the bot is removed from a server.

## 📜 Legal notices

### Disclaimer
**See [`DISCLAIMER.md`](DISCLAIMER.md) for the full text.**  
In short, the bot retrieves data from third‑party APIs; the developers are **not** liable for:

1. Accuracy of the retrieved information,  
2. Offensive, inappropriate, or illegal content that may be returned,  
3. Damage or consequences arising from the use of the search commands,  
4. Service interruptions or failures of external APIs.  

By using any search command you accept full responsibility for the results obtained.

### License
The **source code** is released under the **MIT License**.  
See [`LICENSE.md`](LICENSE.md) for the complete license text.

### Trademark notice
The individual project names (**News Nintendo**, **SearchAnimeManga**, **Anime & Manga Discord Bot**, etc.) are **not** registered trademarks on their own.  
They are covered by the registered trademark **SivaGames** (INPI N° 5183463, classes 9 35 38 41 42).  

> Use of these names outside the scope of operating the bots described here requires prior written permission from the owner of the SivaGames trademark.

For the exact wording, consult [`TRADEMARK.md`](TRADEMARK.md).




## 🙋‍♂️ Support
If you encounter any issues or have questions, please open an issue on GitHub or contact the bot administrator at contact@sivagames.com.