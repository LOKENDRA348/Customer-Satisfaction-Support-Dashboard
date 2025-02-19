# Customer-Satisfaction-Support-Dashboard
##Customer Satisfaction & Support Dashboard
Step 1: Data Preparation in Excel
Handled missing values using AI-integrated Excel:
Descriptive values were forward-filled.
Quantitative values were replaced with their mean.
Step 2: Data Loading in Power BI
Imported the clean and preprocessed dataset into Power BI.
Used Power Query to:
Set the first row as headers.
Close and apply changes.
Step 3: Data Modeling
The dataset contained only one table (flat schema).
No need for data modeling since:
The table had a primary key.
All values were self-contained descriptive attributes.
Step 4: DAX Formulas for KPI Cards
Total Tickets

DAX
Copy
Edit
Total Ticket = COUNT('customer_satisfaction_support_d (2)'[Ticket_ID])
Measures the total number of tickets received.
Total Open & In-Progress Tickets

DAX
Copy
Edit
Total Open & Progress Tickets = CALCULATE(COUNT('customer_satisfaction_support_d (2)'[Ticket_ID]), 'customer_satisfaction_support_d (2)'[Status] IN {"Open", "In Progress"})
Shows the count of unresolved tickets.
Total Resolved & Closed Tickets

DAX
Copy
Edit
Total Resolved & Closed Tickets = CALCULATE(COUNT('customer_satisfaction_support_d (2)'[Ticket_ID]), 'customer_satisfaction_support_d (2)'[Status] IN {"Resolved", "Closed"})
Displays the count of successfully closed tickets.
Average CSAT Score (Customer Satisfaction Score)

DAX
Copy
Edit
Avg CSAT Score = AVERAGE('customer_satisfaction_support_d (2)'[CSAT_Score])
Shows the average customer satisfaction rating.
Issues by Type

DAX
Copy
Edit
Issues by Type = COUNTROWS(GROUPBY('customer_satisfaction_support_d (2)', 'customer_satisfaction_support_d (2)'[Issue_Type]))
Counts different types of issues reported.
Average Response Time (Hrs)

DAX
Copy
Edit
Average Response Time HRS = AVERAGE('customer_satisfaction_support_d (2)'[Response_Time])
Finds the average time taken to respond to tickets.

Since time format was incorrect, we converted it to HH:MM:SS format using:

DAX
Copy
Edit
Avg Response Time HRS = 
VAR TotalMinutes = [Average Response Time HRS] * 60  
VAR Hours = INT(TotalMinutes / 60)         
VAR Minutes = INT(TotalMinutes - (Hours * 60))  
VAR Seconds = ROUND((TotalMinutes - INT(TotalMinutes)) * 60, 0)  
RETURN FORMAT(Hours, "00") & ":" & FORMAT(Minutes, "00") & ":" & FORMAT(Seconds, "00")
Average Resolution Time (Hrs) for Resolved Tickets

DAX
Copy
Edit
Avg Resolution Time HRS (Resolved) = 
AVERAGEX(
    FILTER('customer_satisfaction_support_d (2)', 
    'customer_satisfaction_support_d (2)'[Status] = "Resolved" || 'customer_satisfaction_support_d (2)'[Status] = "Closed"), 
    'customer_satisfaction_support_d (2)'[Resolution_Time])
Shows the average time taken to resolve tickets.

Converted it to HH:MM:SS format using:

DAX
Copy
Edit
Avg Resolution (Resolved) Time = 
VAR TotalMinutes = [Avg Resolution Time HRS (Resolved)] * 60  
VAR Hours = INT(TotalMinutes / 60)         
VAR Minutes = INT(TotalMinutes - (Hours * 60))  
VAR Seconds = ROUND((TotalMinutes - INT(TotalMinutes)) * 60, 0)  
RETURN FORMAT(Hours, "00") & ":" & FORMAT(Minutes, "00") & ":" & FORMAT(Seconds, "00")
Step 5: Visualizations and Data Representation
Line Chart (Trend of Tickets Over Time)

X-axis: Created Date (by Month)
Y-axis: Count of Ticket ID
Shows ticket volume trends over time.
Stacked Bar Chart (Ticket Count by Priority)

Y-axis: Priority (High, Medium, Low)
X-axis: Count of Ticket ID
Helps understand ticket distribution by priority.
Stacked Column Chart (Tickets Assigned to Agents)

X-axis: Agent Name
Y-axis: Count of Ticket ID
Highlights workload distribution among agents.
Donut Chart (Ticket Distribution by Status)

Legend: Status (Open, In Progress, Resolved, Closed)
Value: Count of Ticket ID
Shows the proportion of tickets in different statuses.
Stacked Column Chart (Issue Types and Ticket Count)

X-axis: Issue Type
Y-axis: Count of Ticket ID
Helps identify the most common issue categories.
Treemap (CSAT Score by Agent)

Category: Agent Name
Value: Average CSAT Score
Indicates which agents receive the best customer satisfaction ratings.
Matrix Table (Ticket Details by Agent and Customer)

Rows: Agent Name, Customer Name
Values: Ticket ID
Displays ticket details in a structured format.
Gauge Chart (Overall CSAT Score Indicator)

Value: Average CSAT Score
Minimum Value: Calculated using DAX for the lowest CSAT score.
Maximum Value: Calculated using DAX for the highest CSAT score.
Provides a visual representation of overall customer satisfaction.
Step 6: Final Customizations
Canvas Background: Set to pink.
Visual and Card Effects: Transparency set to 100%.
Step 7: Key Insights
Customer Satisfaction Trends

The average CSAT score provides insights into customer experiences.
Certain agents consistently have higher/lower scores, indicating performance differences.
Ticket Volume Analysis

The line chart reveals periods of high or low ticket creation.
If spikes occur, it may indicate issues with services/products.
Ticket Resolution Efficiency

Gauge chart and treemap help measure agent effectiveness.
Agents with high average resolution time may need additional support/training.
Common Issue Types

The stacked column chart identifies the most frequently reported issues.
This helps prioritize improvements in customer service processes.
Response & Resolution Time

Long response times can highlight inefficiencies in support workflows.
The resolution time metric helps assess how quickly issues are being fixed.
