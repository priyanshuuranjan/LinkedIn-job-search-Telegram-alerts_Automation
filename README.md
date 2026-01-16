---

# 🚀 Automated LinkedIn Job Search Workflow (n8n)

This project is an **n8n workflow** designed for job seekers and developers who want to **automate their LinkedIn job search**. It automatically fetches job listings based on your custom filters, scores each opportunity using AI, logs all results into **Google Sheets**, and sends **Telegram alerts** for the most relevant matches — making your job application process **smarter, faster, and more efficient**. 🔍✨

---
![WorkFlow](https://github.com/user-attachments/assets/5c7b1d9f-0df5-4b1e-a2b0-c8f51701cc3c)

## ⭐ Features

- 🔎 **Automated LinkedIn Job Fetching**  
  Pulls new job listings using your defined filters (keywords, location, seniority, etc.).

- 🤖 **AI-Powered Job Scoring**  
  Each job posting is enriched and ranked using AI for relevance and quality.

- 📊 **Google Sheets Logging**  
  All job records — including scores, timestamps, and metadata — are stored neatly for tracking.

- 📱 **Telegram Notifications**  
  Sends real-time alerts directly to your Telegram account for high-scoring job opportunities.

- ⚙️ **Fully Customizable n8n Workflow**  
  Modify any step — scoring logic, notification rules, or data pipelines — to suit your workflow.

---

## 🧩 How It Works

1. **LinkedIn Query Node** → Fetches job posts using filters you define.  
2. **AI Evaluation Node** → Scores relevance based on your profile or criteria.  
3. **Decision Logic** → Flags top-scoring opportunities.  
4. **Google Sheets Node** → Logs each job with details + score.  
5. **Telegram Node** → Sends alerts for top picks.

---

## 🛠️ Requirements

- n8n instance (self-hosted or cloud)
- LinkedIn data source or API integration
- Google Cloud credentials for Sheets API
- Telegram bot token + chat ID
- AI provider key (OpenAI, Claude, etc.)

---

## 📥 Installation

1. Clone or download this repository  
2. Import the workflow JSON into your n8n instance  
3. Add your environment credentials  
4. Activate the workflow and let automation do the job hunting 💼⚡

---

## 📈 Example Output

- **Google Sheets logs:** job title, company, link, AI score, timestamp  
- **Telegram:** “🔥 New high-score job found: Senior Backend Engineer at X (Score: 92)”

---

- **Google Sheets & Telegram logs:**
  
![Sheet](https://github.com/user-attachments/assets/3e1cd400-87a4-46db-9717-0772a2e96ed1)
![Telegram Job Update](https://github.com/user-attachments/assets/cb87427b-e510-4993-8c1f-074ca66d4356)

---

## 🧪 Test Workflow Example: LinkedIn Job Automation

### Step 1: Create a New Workflow
1. Open your **n8n instance**.
2. Click on **New Workflow**.
3. Name it: `LinkedIn Job Automation Test`.

### Step 2: Add **HTTP Request** Node  
**Purpose**: Fetch sample job data (or test data via an API).  
1. In your workflow, click on **Add Node**.
2. Select **HTTP Request**.
3. Set the method to **GET** and the URL to:  
   `https://jsonplaceholder.typicode.com/posts/1`  
   (This URL returns a test job post, which will simulate the response we expect from LinkedIn).
4. Click **Execute Node** to fetch the test data and verify the response. You should see something like:

```json
{
  "userId": 1,
  "id": 1,
  "title": "Test Job Post Title",
  "body": "Details about the job post."
}
```

### Step 3: Add **Set** Node  
**Purpose**: Map the response data to workflow fields for further processing.  
1. Click **Add Node** and choose **Set**.
2. In the **Set** node:
   - Add the fields to be mapped:  
     - **title**: `{{$node["HTTP Request"].json["title"]}}`
     - **company**: `Sample Company` (replace this with actual data when working with LinkedIn API).
     - **location**: `Sample Location` (same as above).
     - **score**: Define your scoring logic (for now, use a fixed value like `80` or generate it dynamically based on AI).

3. This node will now output these variables for the next steps.

### Step 4: Add **Google Sheets** Node  
**Purpose**: Log the job data (with score) into a Google Sheet.  
1. Click **Add Node** and search for **Google Sheets**.
2. Select **Append Row** as the operation.
3. Connect the **Set** node to the **Google Sheets** node.
4. In **Google Sheets** settings:
   - **Spreadsheet ID**: Enter your Google Sheets document’s ID.
   - **Range**: Define the range, such as `Sheet1!A:D` (adjust depending on your sheet layout).
   - Map the fields:
     - **title** → `{{$node["Set"].json["title"]}}`
     - **company** → `{{$node["Set"].json["company"]}}`
     - **location** → `{{$node["Set"].json["location"]}}`
     - **score** → `{{$node["Set"].json["score"]}}`

### Step 5: Add **Telegram** Node (Optional)  
**Purpose**: Send a message via Telegram when a job is added.  
1. Click **Add Node** and search for **Telegram**.
2. Select **Send Message** as the operation.
3. Connect the **Set** node to the **Telegram** node.
4. Configure your **Telegram** bot settings:
   - **Chat ID**: Add your Telegram chat ID (you can get this from your bot or by sending a message to your bot).
   - **Message**:  
     Use the following format to send a message:
     ```
     New Job: {{$node["Set"].json["title"]}} at {{$node["Set"].json["company"]}} located in {{$node["Set"].json["location"]}}.
     Score: {{$node["Set"].json["score"]}}
     ```
   - This message will notify you whenever a new job is logged.

### Step 6: Connect Nodes & Execute Workflow  
1. Ensure your nodes are connected in this order:  
   `HTTP Request → Set → Google Sheets → Telegram`  
2. Click **Execute Workflow** to run the test.

After running the workflow:
- You should see the test job data in your Google Sheet.
- You should receive a Telegram notification with the job details.

---

## 📝 Step-by-Step Tutorial: Setting Up Your LinkedIn Job Automation

Follow these steps to set up the LinkedIn Job Automation workflow on n8n.

### 1. **Create Your n8n Workflow**
   - Open your n8n instance and create a new workflow. Name it something relevant, such as “LinkedIn Job Automation”.
   
### 2. **Set Up LinkedIn Data Fetching**
   - Add the **HTTP Request** node to fetch LinkedIn job data (use LinkedIn API or any other service to get job data).
     - Set method: **GET**.
     - Enter the URL for fetching job data (e.g., LinkedIn API endpoint).
     - Test the request to ensure data is being pulled correctly.

### 3. **Process Job Data**
   - Add a **Set** node to process the job data.
     - Map fields such as:
       - **title**
       - **company**
       - **location**
       - **score**
     - The score can be set based on AI scoring or other custom logic.

### 4. **Log Data to Google Sheets**
   - Add the **Google Sheets** node with the **Append Row** operation.
   - Connect your Google Sheets credentials and map the job data fields from the **Set** node to the Google Sheets columns (title, company, location, score).
   
### 5. **Set Up Telegram Alerts**
   - If you want real-time job alerts, add the **Telegram** node.
   - Enter your **Telegram bot token** and **Chat ID** to send messages.
   - Customize the message to include relevant job details from the **Set** node.

### 6. **Connect the Nodes & Test**
   - Connect all nodes in sequence.
   - Execute the workflow to test if the data flows correctly through the nodes.

### 7. **Customize & Automate**
   - Now that your basic automation is set up, you can:
     - Add filters to prioritize specific job types or titles.
     - Adjust AI scoring logic for better relevance.
     - Add multiple notification channels (email, Slack, etc.) if needed.

---

## 📋 Conclusion
This workflow will significantly speed up your job application process by automatically logging job posts into Google Sheets and alerting you via Telegram. It’s a great way for job seekers and developers to stay on top of the latest job opportunities without manual searching.

Feel free to adjust the **Set** and **AI Scoring** nodes as needed to fine-tune job relevance or integrate with other services!



## 🤝 Contributions

Pull requests and suggestions are welcome! Feel free to add:  
✨ better scoring logic  
📊 new data destinations  
🔔 more notification channels  

---

## 📄 License

MIT License – free for personal and commercial use.

---


