
- Based on  [[System Analysis (PaoSatang)]] #PaoSatang-System-Analysis that we just analysed. We can writing our **System Design** by using **UML** Lang by following concept requirement.

1. Actors
	- **User** : the **actor** would interact with the system
	- **BudgetAccount** : The **System** **actor** that would  interact with the system


2. Use-case
	#Use-case-PaoSatang #Use-case-Diagram 
	Understanding and Analyzing. we can make diagram following #Key-Features-PaoSatang 
```merm
mindmap
    root((App))
        SettingFinancialGoals[1.Setting Financial Goals]
            "PDCA Cycle  Plan"
            "App prompts goal setup"
        TrackingDailyExpenses[2.Tracking Daily Expenses]
            "PDCA Cycle  Do"
            "App prompts expense logging"
        AnalyzingFinancialHealth[3.Analyzing Then Adjusting Financial Health]
            "PDCA Cycle  Check & Do"
            "Insights gained from the analysis then prediction the Financial Health"
        AutomatingFinancialTasks[4.Automating Financial Tasks]
            "App prompts automation setup"
        ManagingMultipleUserBudgets[5.Managing Multiple User Budgets]
            "App prompts shared account setup"
```


1. **Setting Financial Goals (PDCA Cycle - Plan)**
    - A user opens the app and navigates to the "Goals" section.
    - The user creates a new goal, such as "Save $10,000 for a down payment on a house."
    - The user sets a target date and breaks down the goal into smaller, achievable monthly or weekly targets.
    - The app guides the user through allocating a portion of their income or existing funds towards this goal.
```merm
sequenceDiagram
	autonumber
    actor User
    participant GoalManager
    actor B as BudgetAccount(system)
    participant S as Server
    participant DB as Database
    
    User->>GoalManager: Create New Goal
    GoalManager->>User: Prompt Goal Details
    User->>GoalManager: Provide Goal Details
    GoalManager->>S: POST Request
    note over S: Allocate Funds
    S->>DB: Create
    DB-->>S: Data
    S-->>B: Responed 
    note over B: Update Local State
    B-->>User: Show Detail
```

2. **Tracking Daily Expenses (PDCA Cycle - Do)**
    - A user logs their daily expenses in the "Diary" section, categorizing each transaction (e.g., groceries, transportation, entertainment).
    - The user attaches receipts or photos of bills for better record-keeping.
    - The app automatically calculates the total expenses for the day/month and provides insights into spending patterns.
```merm
sequenceDiagram
    autonumber
    actor User
    participant ExpenseTracker
    actor App as BudgetAccount(system)
    participant S as Server
    participant DB as Database

    User->>ExpenseTracker: Log Expense
    ExpenseTracker->>User: Prompt Expense Details
    User->>ExpenseTracker: Provide Expense Details
    ExpenseTracker->>S: POST Request
    note over S: Record Transaction Data
    S->>DB: Create
    DB-->>S: Data Respond
    S-->>App: Respond
    note over App: Update Local State
    App-->>User: Display Spending Insights
``` 

3. **Analyzing Then Adjusting Financial Health  (PDCA Cycle - Check & Act)**
    - The app generates detailed reports on the user's expenses, income, budget adherence, and category breakdowns for a specified period.
    - The user can view interactive charts and visualizations to identify areas of overspending or underspending.
    - The app highlights any deviations from the user's financial plans or goals.
    - Based on the insights gained from the analysis, the user can adjust their budgets, reallocate funds, or modify their financial goals within the app.
```merm
sequenceDiagram
    autonumber
    actor User
    participant ReportGenerator
    Participant RA as ReportAnalysis
    actor App as  Budget(system)
    participant S as Server
    participant DB as Database

    rect rgb(191, 223, 255,0.4)
	par User get Report
    User->>ReportGenerator: Request Financial Insights
    ReportGenerator->>S: GET Request
    note over S: Fetch Financial Data
    S->>DB: Retrieve
    DB-->>S: Data
    S-->>ReportGenerator: Provide Financial Data
    ReportGenerator-->>ReportGenerator: Generate Reports
    ReportGenerator-->>User: Display Financial Report
	end
	end

    rect rgb(191, 210, 230,0.4)
	par Analysis report from Report
	ReportGenerator-->>+RA: Sending Report
	RA-->>+RA: Adjusting financial health
	RA-->>-User: Display Financial Health Insights
	end
	end
```
    
4. **Automating Financial Tasks**
    - The user sets up rules to automatically categorize transactions based on predefined criteria (e.g., merchant names, transaction amounts).
    - The app automatically allocates incoming funds to designated budget categories based on the user's rules.
    - The user schedules recurring transactions (e.g., rent, utility bills) to be automatically recorded in the app.
```merm
sequenceDiagram
	actor User
    participant App
    participant RuleManager
    participant TransactionManager
    User->>App: Set Automation Rules
    App->>RuleManager: Define Rules
    RuleManager-->>User: Prompt Rule Details
    User->>RuleManager: Provide Rule Details
    RuleManager-->>App: Rules Defined
    App->>TransactionManager: Apply Rules
    TransactionManager-->>App: Transactions Automated
    App-->>User: Confirm Automation Setup
```
5. **Managing Multiple User Budgets**
    - A family creates a shared account within the app, allowing them to manage their finances collectively.
    - Each family member can log their individual expenses and income, while the app consolidates the data into a unified view.
    - The app enables family members to allocate funds towards shared goals or budgets, such as a family vacation or a new car purchase.
```merm
sequenceDiagram
	autonumber
    actor U as User
    actor BA as BudgetAccount
    participant AccountManager
    participant S as Server
    participant DB as Database

	par Get BudgetAccounts
	U->>+AccountManager: Check BudgetAccount List 
	AccountManager->>+S: GET Reqeust
	note over S: Fecthing BudgetAccount List 
	S->>+DB: Retrived Data
	DB-->>-S: Return Data
    S-->>+AccountManager: Data Response
    AccountManager-->>+BA: Set State
    AccountManager-->>-U: Show the result
	    rect rgb(191, 223, 255,0.4)
	    par display BudgetAccounts info
	    U->>+AccountManager: Get Specific BudgetAccount
	    AccountManager->>+BA: Find BudgetAccount by ID
	    BA-->>-AccountManager: Return BA state
	    AccountManager-->>+U: Provide BA Details
	    AccountManager-->>+U: Provide Log Expenses/Income
	    AccountManager-->>-U: Provide Record Transactions
	    end
	    end
	end
	
    
```


3. Class Diagram
	#Class-Diagrams-PaoSatang #Class-Diagrams #Diagrams
```merm
%%{init: 
 {'themeCSS': '.node rect { stroke-width: 3px; padding:2px; }'}, 
 {'flowchart':{'nodeSpacing': 80, 'rankSpacing': 30}}
}%%

erDiagram
    USER {
        string name
        string email
        string password
    }
    BUDGET_ACCOUNT {
        int amount
    }
    TRANSACTION {
        string uuid PK
        int amount
        enum operationType "OUTFLOW | INFLOW"
        enum transactionType "BUDGET | EXPENSE" 
        string categoryLabel
        string memo 
    }
    FINANCE_REPORT {
        string title
        date created_at
        enum reportType "EXPENSE | INCOME | ETC"
        string details
    }
    FINANCE_REPORT_DATA {
        string data "JSON using with chart.js"
    }
    CATEGORY {
        string label
        enum categoryType "EXPENSE | BUDGET"
    }
    NOTIFICATION {
        string label
        string message
    }
    TARGET_GOAL {
        int targetAmount
        string description
    }
    RULE {
        string criteria
        string action
    }
    SCHEDULED_TRANSACTION {
        string uuid PK
        int amount
        string categoryLabel
        string memo 
        date nextOccurrence
    }

    INCOME_SOURCE {
        string name
        int amount
    }


    USER ||--|{ NOTIFICATION : has
    USER ||--|{ TARGET_GOAL : has
    USER ||--|{ RULE : sets
  

    USER ||--|{ SCHEDULED_TRANSACTION : schedules
    TRANSACTION ||--|{ CATEGORY : has
    TRANSACTION }|--|{ BUDGET_ACCOUNT : effects
    TRANSACTION ||--o{ INCOME_SOURCE : originates_from
    TRANSACTION }|--|| USER : belongs

    FINANCE_REPORT ||--|{ TRANSACTION : analyzes
    FINANCE_REPORT ||--|{ FINANCE_REPORT_DATA : contains

    BUDGET_ACCOUNT }|--|| USER : belongs

    
    RULE ||--|{ TRANSACTION : applies_to
```



```merm
classDiagram
    class USER {
        +string firstName
        +string lastName
        +string email
        +string password
    }
    
    class BUDGET_ACCOUNT {
        +int amount
        +addFunds()
        +removeFunds()
        +checkBalance()
    }
    class TRANSACTION {
        +string uuid
        +int amount
        +enum operationType
        +enum transactionType
        +string categoryLabel
        +string memo 
        +execute()
        +cancel()
    }
    class FINANCE_REPORT {
        +string title
        +date created_at
        +enum reportType
        +string details
        +generate()
        +view()
    }
    class FINANCE_REPORT_DATA {
        +string data
        +generateData()
        +viewData()
    }


    class CATEGORY {
        +string label
        +enum categoryType
        +createCategory()
        +deleteCategory()
        +updateCategory()
    }
    class NOTIFICATION {
        +string label
        +string message
        +send()
        +cancel()
    }
    class SAVING_GOAL {
        +int targetAmount
        +string description
        +createGoal()
        +deleteGoal()
        +updateGoal()
    }
    class RULE {
        +string criteria
        +string action
        +apply()
        +cancel()
    }
    class SCHEDULED_TRANSACTION {
        +string uuid
        +int amount
        +string categoryLabel
        +string memo 
        +date nextOccurrence
        +schedule()
        +cancel()
    }

    class INCOME_SOURCE {
        +string name
        +int amount
        +createSource()
        +deleteSource()
        +updateSource()
    }

    class UserService {
        +createUser()
        +deleteUser()
        +updateUser()
    }
    class TransactionService {
        +createTransaction()
        +deleteTransaction()
        +updateTransaction()
    }
    class ReportService {
        +createReport()
        +viewReport()
    }
    class NotificationService {
        +sendNotification()
        +cancelNotification()
    }

    class AuthService {
        +register(email, password)
        +login(email, password)
        +logout(user)
        +changePassword(user, oldPassword, newPassword)
    }
    class Session {
        +user
        +token
        +createdAt
        +expiresAt
    }


    USER "1" --o "0..*" BUDGET_ACCOUNT : has
    USER "1" --o "0..*" TRANSACTION : has
    USER "1" --o "0..*" NOTIFICATION : has
    USER "1" --o "0..*" SAVING_GOAL : has
    USER "1" --> "1" NotificationService : used
    USER "1" --o "0..*" RULE : sets
    USER "1" --o "0..*" SCHEDULED_TRANSACTION : schedules

    TRANSACTION "1" --o "0..*" CATEGORY : has
    TRANSACTION "1" --o "0..*" BUDGET_ACCOUNT : effects
    TRANSACTION "1" --o "0..*" INCOME_SOURCE : originates_from
    FINANCE_REPORT "1" --o "0..*" TRANSACTION : analyzes
    FINANCE_REPORT "1" --o "0..*" FINANCE_REPORT_DATA : contains
    RULE "1" --o "0..*" TRANSACTION : applies_to


    UserService --> USER : operates_on
    ReportService --> FINANCE_REPORT : operates_on
    NotificationService --> NOTIFICATION : operates_on
    TransactionService --> TRANSACTION : operates_on
    AuthService --> USER : authenticates
    AuthService --> Session : manages
```

- State Diagram
	#State-Diagram-PaoSatang #State-Diagram #Diagrams
```merm
stateDiagram
	direction LR
    [*] --> Plan
    Plan --> Do : Implement Plan
    Do --> Check : Measure Results
    Check --> Act : Analyze Results
    Act --> Plan : Improve Plan
    Act --> [*] : Goal Achieved
```
