# Customer-Satisfaction-Support-Dashboard

1. Data Preparation & Cleaning (Excel)
Handled Null Values:

Descriptive fields (e.g., Status, Issue Type): Forward-filled missing values.

Quantitative fields (e.g., CSAT_Score, Response_Time): Replaced nulls with column mean.

AI Integration: Used Excel AI tools to automate filling logic.

Power BI Load: Imported preprocessed data; only adjusted headers in Power Query (no further cleaning needed).

2. Data Modeling
Flat Schema: Single table used (no relationships or data modeling required).

Primary Key: Unique identifiers like Ticket_ID acted as implicit keys.

3. DAX Formulas
Total Ticket:  COUNT('customer_satisfaction_support_d (2)'[Ticket_ID])

Purpose: Total tickets created.

Open & In-Progress Tickets:= CALCULATE(COUNT('customer_satisfaction_support_d (2)'[Ticket_ID]),'customer_satisfaction_support_d (2)'[Status] in {"Open", "In Progress"})

Purpose: Track active workload.

Resolved & Closed Tickets:  = CALCULATE(COUNT('customer_satisfaction_support_d (2)'[Ticket_ID]),'customer_satisfaction_support_d (2)'[Status] in {"Resolved", "Closed"})

Purpose: Measure resolution efficiency.

Avg CSAT Score: = AVERAGE('customer_satisfaction_support_d (2)'[CSAT_Score])

Purpose: Overall customer satisfaction (scale: 1–5).

Issues by Type: = COUNTROWS(GROUPBY('customer_satisfaction_support_d (2)','customer_satisfaction_support_d (2)'[Issue_Type]))

Purpose: Categorize recurring problems.

Avg Response Time (HRS):  = AVERAGE('customer_satisfaction_support_d (2)'[Response_Time])


Time Format DAX: Converted hours to HH:MM:SS using AI-assisted code.
DAX CODE HH:MM:SS CONVERSION = Avg Response time HRS = VAR TotalMinutes = [Average Response time HRS] * 60  -- Convert hours to minutes
VAR Hours = INT(TotalMinutes / 60)         -- Extract hours
VAR Minutes = INT(TotalMinutes - (Hours * 60)) -- Extract remaining minutes
VAR Seconds = ROUND((TotalMinutes - INT(TotalMinutes)) * 60, 0) -- Convert fractional part to seconds
RETURN 
FORMAT(Hours, "00") & ":" & FORMAT(Minutes, "00") & ":" & FORMAT(Seconds, "00")

 Avg Resolution Time HRS (Resolved) = AVERAGEX(FILTER('customer_satisfaction_support_d (2)','customer_satisfaction_support_d (2)'[Status] ="Resolved" || 'customer_satisfaction_support_d (2)'[Status] = "Closed" ),'customer_satisfaction_support_d (2)'[Resolution_Time])



Time Format DAX: Applied similar HH:MM:SS conversion.
DAX CODE = Avg resolution(resolved) time = 
VAR TotalMinutes = [Avg Resolution Time HRS (Resolved)] * 60  -- Convert hours to minutes
VAR Hours = INT(TotalMinutes / 60)         -- Extract hours
VAR Minutes = INT(TotalMinutes - (Hours * 60)) -- Extract remaining minutes
VAR Seconds = ROUND((TotalMinutes - INT(TotalMinutes)) * 60, 0) -- Convert fractional part to seconds
RETURN 
FORMAT(Hours, "00") & ":" & FORMAT(Minutes, "00") & ":" & FORMAT(Seconds, "00")

4. Visualizations & Purpose
Line Chart

X-axis: Created Date (Month) | Y-axis: Ticket Count

Insight: Track ticket volume trends over time (e.g., spikes in specific months).

Stacked Bar Chart

Y-axis: Priority | X-axis: Ticket Count

Insight: Identify high-priority ticket distribution (e.g., % of "Urgent" tickets).

Stacked Column Chart

X-axis: Agent Name | Y-axis: Ticket Count

Insight: Compare agent workload/productivity.

Donut Chart

Legend: Status | Value: Ticket Count

Insight: Visualize resolution vs. pending tickets (e.g., 70% "Resolved").

Stacked Column Chart (Issue Type)

X-axis: Issue Type | Y-axis: Ticket Count

Insight: Highlight most frequent issues (e.g., "Billing" dominates).

Treemap

Category: Agent Name | Value: Avg CSAT Score

Insight: Rank agents by customer satisfaction (e.g., Agent X: 4.8/5).

Matrix Table

Rows: Agent Name, Customer Name | Values: Ticket ID Count

Insight: Drill down into agent-customer interactions.

Gauge

Value: Avg CSAT Score | Min/Max: DAX-calculated min/max

Insight: Quick glance at overall satisfaction (e.g., 4.2/5).

5. Design & Formatting
Canvas Background: Pink theme for visual appeal.

Transparency: 100% transparency on cards/visuals for a clean look.

6. Key Insights
Peak Ticket Volume: Highest in December (holiday season).

Agent Performance: Agent "John" resolved 120+ tickets but has a below-average CSAT (3.9).

Slow Resolution: "Technical Issues" take 15+ hours to resolve (vs. average 8hrs).

Customer Sentiment: Avg CSAT = 4.3/5, but 20% tickets have CSAT < 3 (needs root-cause analysis).

Bottlenecks: 30% of "Open" tickets are >48hrs old (process inefficiency).
