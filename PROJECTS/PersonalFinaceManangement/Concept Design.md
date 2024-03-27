**Brief concept**
	is a comprehensive personal finance management app designed to empower users to take control of their financial health. The app offers a user-friendly interface with a range of features to help individuals track their income, expenses, budgets, and financial goals. With FinSavvy, users can gain valuable insights into their spending patterns, make informed financial decisions, and work towards achieving their financial objectives.

**Key Features:**

1. **Expense Tracking:**
    
    - Easily record and categorize daily expenses.
    - View transaction history and track spending trends over time.
2. **Budget Management:**
    
    - Set personalized budgets for different spending categories.
    - Receive real-time alerts when approaching or exceeding budget limits.
3. **Income Tracking:**
    
    - Log various sources of income, including salaries, bonuses, and additional earnings.
    - Visualize income sources to understand overall financial inflow.
4. **Financial Goal Setting:**
    
    - Create and manage financial goals, such as saving for a vacation or paying off a debt.
    - Track progress towards goals with interactive visualizations.
5. **Reports and Analytics:**
    
    - Generate detailed financial reports for insights into spending and income patterns.
    - Utilize customizable charts and graphs to visualize financial data.
6. **Currency Support:**
    
    - Support for multiple currencies with real-time exchange rates.
    - Effortlessly convert and track expenses in various currencies.
8. **Reminders and Notifications:**
    
    - Set reminders for upcoming bills, income receipts, and financial goal deadlines.
    - Receive notifications to stay informed about important financial events.
9. **User-Friendly Interface:**
    
    - Intuitive design for easy navigation and data entry.
    - Customizable dashboard to tailor the user experience.

**Model**s
![[Pasted image 20240124000010.png]]

- ### **Income**
	- ##### **Attributes**:
	1. **Income ID:**
	    - Unique identifier for each income source.
	2. **User ID (foreign key):**
	    - Associating income with a specific user.
	3. **Source Name:**
	    - Descriptive name for the income source (e.g., Salary, Freelance Work).
	4. **Amount:**
	    - The amount of income received in each cycle (e.g., monthly, weekly).
	5. **Frequency:**
	    - The frequency at which income is received (e.g., monthly, bi-weekly, weekly).
	6. **Tax-Related Information:**
	    - Fields for tax-related details, such as tax percentage or deductions.
	7. **Payment Method:**
	    - Specify the method through which income is received (e.g., direct deposit, check).
	8. **Payment Schedule:**
	    - For irregular income, allow users to specify a custom payment schedule.
	9. **Date of Entry:**
	    - Record the date when the income source is added to the system.
	
	- ##### **Methods**
	1. **Add a New Income Source:**
	1. **Retrieve Income Sources for a Specific User:**
	2. **Update Income Details:**
	3. **Delete an Income Source:**
	4. **Project Future Income:**
	5. **Set Reminders for Income Receipts:**
	    - Enable users to set reminders for upcoming income receipts.
	6. **Categorize Income:**
	    - Allow users to categorize income sources (e.g., Primary Income, Side Gig).
	7. **View Income History:**
	    - Provide a history of income transactions for a specific income source.
	8. **<mark style="background: #FFB86CA6;">Export Income Data</mark>:**
	    - Option to export income data for record-keeping or tax purposes.
	9. **<mark style="background: #FFB86CA6;">Income Insights:</mark>**
	    - Generate insights and trends related to income, helping users understand their earning patterns.
	10. **Support for Multiple Currencies:**
	11. **<mark style="background: #FFB86CA6;">Automatic Income Updates:</mark>**
	    - If applicable, implement a mechanism for automatic updates of income data from external sources.

- ### **Currency**
	- ##### Attributes:
	1. **Currency Code:**
	    - Unique code representing each currency (e.g., USD for US Dollar, EUR for Euro).
	2. **Currency Name:**
	    - Descriptive name for the currency (e.g., United States Dollar, Euro).
	3. **Exchange Rates:**
	    - Store current exchange rates against a base currency.
	    - Historical exchange rates for analytical purposes.
	4. **Last Updated Date:**
	    - Record the date and time when exchange rates were last updated.
	5. **Supported Currencies:**
	    - Maintain a list of supported currencies within the application.
	6. **Decimal Places:**
	    - Specify the number of decimal places for each currency to handle precision in calculations.
	      
	- ##### Methods:
	1. **Retrieve List of Supported Currencies:**
	    - Method to fetch a list of currencies supported by the application.
	2. **Update Exchange Rates:**
	    - Method to update exchange rates, either manually or through an automated service.
	3. **Convert Amounts Between Currencies:**
	    - Algorithm to convert amounts from one currency to another based on the current exchange rates.
	4. **Retrieve Exchange Rates:**
	    - Method to fetch the current exchange rates for a specific currency.
	5. **Handle Dynamic Exchange Rate Changes:**
	    - Implement a mechanism to handle dynamic changes in exchange rates over time.
	6. **Exchange Rate API Integration:**
	    - Integrate with external APIs or services to fetch real-time exchange rates.
	7. **User Preferences:**
	    - Allow users to customize currency-related preferences, such as the number of decimal places.

- #### **Report** 
	- ##### **Attributes**:
	1. **Report ID:**
	    - Unique identifier for each financial report.
	2. **User ID (foreign key):**
	    - Associating reports with a specific user.
	3. **Report Type:**
	    - Indicate the type of financial report (e.g., spending analysis, income breakdown).
	4. **Date Range:**
	    - Specify the time frame for the data included in the report.
	5. **Data Visualisation Preferences:**
	    - User preferences for how data should be presented (e.g., charts, graphs, tables).
	6. **Creation Date:**
	    - Record the date and time when the report is generated.
	      
	- ##### Methods:
	1. **Generate Financial Reports:**
	    - Method to generate a financial report based on specified parameters.
	    - Include options for different report types (spending, income, net worth, etc.).
	2. <mark style="background: #FFB86CA6;">**Save or Export Reports:**</mark>
	    - Method to save reports within the application for future reference.
	    - Export reports in various formats (PDF, CSV) for external use.
	3. **View Previously Generated Reports:**
	    - Retrieve a list of previously generated reports for a specific user.
	4. **Scheduled Reports:**
	    - Allow users to schedule automatic generation and delivery of reports via email.
	5. **<mark style="background: #FFB86CA6;">Report Insights and Recommendations:</mark>**
	    - Provide insights and recommendations based on the data presented in the report.
	6. <mark style="background: #FFB86CA6;">**Comparative Analysis:**</mark>
	    - Include features for comparing current and past financial performance.
	7. **Category Breakdowns:**
	    - Break down spending or income into specific categories for detailed analysis.
	8. **Graphical Representations:**
	    - Present data using charts, graphs, and visualizations to aid in understanding.
	9. **<mark style="background: #FFB86CA6;">Currency Conversion within Reports</mark>:**
	    - If dealing with multiple currencies, allow users to view reports in their preferred currency.
	10. **Data Filtering and Sorting:**
	    - Allow users to filter and sort data within the report for a customized view.
	11. **Print-Friendly Reports:**
	    - Design reports to be print-friendly for users who prefer physical copies.
	      
	  