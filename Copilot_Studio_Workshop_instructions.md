# Copilot Studio Workshop
## Library Review Response Automation

---

##  What You'll Build

An AI agent that:
1. Finds negative library reviews
2. Categorizes issues automatically  
3. Sends personalized email responses


---

##  What You Need

- Copilot Studio access
- Power Automate license
- AI Builder credits
- Microsoft 365 email
- CSV file (provided)

---

##  PART 1: Setup

### Step 1: Import CSV to Dataverse

1. Go to **Power Apps** (make.powerapps.com)
2. Left menu → **Tables**
3. Click **Import** → **Import data**
4. Upload `library_reviews.csv`
5. Map columns (auto-detected)
6. Table name: **"Library Reviews"**
7. Click **Import** and wait

**Verify:** Table should have 20 reviews

---

##  PART 2: Build Copilot Agent

### Step 1: Environments in Copilot Studio

<img width="653" height="525" alt="image" src="https://github.com/user-attachments/assets/9e77e21b-5799-4843-a75b-4eadc0c9f039" />

<img width="454" height="526" alt="image" src="https://github.com/user-attachments/assets/29848fa0-98b5-4082-acc0-efd56d5fb45a" />


### Step 2: Create Agent

<img width="975" height="529" alt="image" src="https://github.com/user-attachments/assets/0c6d1a4b-825d-45e4-9fa8-4e0341c2aed8" />

1. Go to **Copilot Studio** (copilotstudio.microsoft.com)
2. Click **Agents** → **Create blank agent**


**Settings:**
```
Name: Public Facility Review Manager

Description: 
Analyzes library reviews and sends automated responses

Instructions:
You are a helpful AI assistant for the City Library System. Your role is to: - Analyze customer reviews from our four library branches (Downtown, Westside, Eastside, and North Branch) - Identify negative or concerning feedback that requires immediate attention - Help library management understand review trends and common issues - Facilitate quick responses to customer concerns - Maintain a professional, empathetic, and solution-oriented tone - Prioritize customer satisfaction and service quality When discussing reviews: - Always acknowledge the customer's experience - Focus on actionable insights - Suggest appropriate next steps - Escalate urgent issues (safety, major complaints) immediately Review flagging criteria: - Rating of 1-2 stars = High priority - Keywords: "unsafe", "broken", "rude", "disgusting", "unacceptable" = Flagged - Multiple complaints about same issue = Escalate
```

4. Click **Create**

---

### Step 3: Add Knowledge Source

1. Go to **Knowledge** tab
2. Click **+ Add knowledge**
3. Select **AI Search**
4. Choose **Library Reviews** Index
5. Click **Add**
6. Wait for indexing (~30 seconds)

---

### Step 4: Enable Generative Answers

1. Go to **Settings** (gear icon)
2. **Generative AI** section
3. Toggle **Generative answers** to ON
4. Click **Save**

---

### Step 5: Test Basic Q&A

In the **Test** panel, try:
```
User: "What negative reviews do we have?"
User: "Show me reviews about cleanliness"
User: "Which branch has the most complaints?"
```

✓ Agent should answer from the data

---

### Step 6: Create Topic

1. Go to **Topics** tab
2. Click **+ Add a topic** → **From blank**
3. Name: **"Analyze Flagged Reviews"**

**Trigger phrases:**
```
- Show me flagged reviews
- What negative reviews do we have
- Show concerning feedback
- Which reviews need attention
- Show me problem reviews
- Display urgent reviews
- What complaints came in today
- Show me reviews that need responses
- Find reviews with issues
- What are customers complaining about
```

---

**Topic Flow:**

**Node 1: Message**
```
I'll help you send automated responses to flagged reviews 
(reviews with 2 stars or below).
```

**Node 2: Question (Multiple Choice)**
```
Which branch would you like to process?

Options:
○ Downtown Branch
○ Westside Branch
○ Eastside Branch
○ North Branch

Save response as: Topic.BranchFilter
```
**Node 3: Message**
```
I'll analyze our flagged reviews - these are reviews with ratings of 2 stars  or below, or those containing concerning keywords.

Let me search our review database...
```

**Node 4: Call an action**
- We'll create this flow next
- For now, add placeholder

**Save the topic**

---

##  PART 3: AI Builder

### Step 1: Create GPT Categorization Prompt

1. Go to **Power Apps** (make.powerapps.com)
2. Left menu → **AI hub**
3. Choose **Prompts**
4. Name: **"Review Categorizer"**

**Prompt text:**
```
You are analyzing customer reviews for a public library.

Review Text: {ReviewText}
Rating: {Rating} stars
Branch: {BranchName}

Analyze and respond with ONLY this JSON (no markdown, no explanation):
{
  "category": "[Staff|Cleanliness|Resources|Hours|Technology|Safety|Other]",
  "urgency": "[Low|Medium|High|Critical]",
  "primaryIssue": "[one sentence summary]",
  "shouldRespond": "[true|false]"
}

Categorization rules:
- Rating 1-2 = shouldRespond: true
- Safety/health issues = urgency: Critical
- Staff complaints = urgency: High
- Facility issues = urgency: High
- Hours/resources = urgency: Medium
```

**Add dynamic inputs:**
- ReviewText (Text)
- Rating (Number)
- BranchName (Text)

**Test with sample:**
```
ReviewText: "The bathrooms were absolutely disgusting!"
Rating: 1
BranchName: "Downtown Branch"
```

**Expected output:**
```json
{
  "category": "Cleanliness",
  "urgency": "High",
  "primaryIssue": "Unsanitary bathroom conditions",
  "shouldRespond": true
}
```

5. Click **Save**

---

##  PART 4: Power Automate Flows

### Flow 1: GetFilteredReviews

1. Go to **Power Automate** (make.powerautomate.com)
2. Click **Create** → **Automated cloud flow**
3. Name: **"GetFilteredReviews"**
4. Trigger: **When an agent calls the flow**

**Trigger inputs:**
- BranchName (String)

---

**Step 1: List rows**
- Action: **Dataverse - List rows**
- Table: Library Reviews
- Filter rows: 
```
cr5fc_branchname eq '@{triggerBody()?['BranchName']}' and cr5fc_rating le 2
```
- Row limit: 20

---

**Step 2: Initialize variable**
- Name: ReviewsArray
- Type: Array
- Value: `[]`

---


**Step 3: Initialize variable**
- Name: DisplayText
- Type: String
- Value: ""

---

**Step 4: Apply to each**
- Select output: value (from List rows)

**Inside loop:**

**Action: Append to array variable**
- Name: ReviewsArray
- Value:
```json
{
  "ReviewID": @{items('Apply_to_each')?['cr977_reviewid']},
  "ReviewerName": @{items('Apply_to_each')?['cr977_reviewername']},
  "ReviewerEmail": @{items('Apply_to_each')?['cr977_revieweremail']},
  "ReviewText": @{items('Apply_to_each')?['cr977_reviewtext']},
  "BranchName": @{items('Apply_to_each')?['cr977_branchname']},
  "Rating": @{items('Apply_to_each')?['cr977_rating']}
}
```

*Note: Replace cr977_ with your actual Dataverse column prefix*

**Action: Append to string variable**
- Name: DisplayText
- Value:

```json
Review # @{items('Apply_to_each')?['cr5fc_reviewid']}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Name:     @{items('Apply_to_each')?['cr5fc_reviewername']} 
Email:    @{items('Apply_to_each')?['cr5fc_revieweremail']} 
Branch:   @{items('Apply_to_each')?['cr5fc_branchname']} 
Rating:    @{items('Apply_to_each')?['cr5fc_rating']}⭐ 
Comment:  @{items('Apply_to_each')?['cr5fc_reviewtext']}  
```
---

**Step 5: Return to Copilot**
- ReviewCount (Number): `@{length(variables('ReviewsArray'))}`
- ReviewsJSON (String): `@{string(variables('ReviewsArray'))}`
- Summary (String): `Found @{length(variables('ReviewsArray'))} flagged reviews`
- DisplayText (String): `@{string(variables('DisplayText'))}`

**Save and test flow**

---

### Flow 2: SendAutomatedResponses

1. **Create** → **Automated cloud flow**
2. Name: **"SendAutomatedResponses"**
3. Trigger: **When an agent calls the flow**

**Trigger inputs:**
- ReviewsJSON (String)

---

**Step 1: Parse JSON**
- Content: `@{triggerBody()?['ReviewsJSON']}`
- Schema:
```json
{
    "type": "array",
    "items": {
        "type": "object",
        "properties": {
            "ReviewID": {"type": "integer"},
            "ReviewerName": {"type": "string"},
            "ReviewerEmail": {"type": "string"},
            "ReviewText": {"type": "string"},
            "BranchName": {"type": "string"},
            "Rating": {"type": "integer"}
        }
    }
}
```

---

**Step 2: Initialize counters**
- Name: SuccessCount
- Type: Integer
- Value: 0

---

**Step 3: Apply to each**
- Select: body('Parse_JSON')

**Inside loop:**

---

**3a. AI Builder - Create text with GPT**
- Prompt: Select "Review Categorizer"
- ReviewText: `@{items('Apply_to_each')?['ReviewText']}`
- Rating: `@{items('Apply_to_each')?['Rating']}`
- BranchName: `@{items('Apply_to_each')?['BranchName']}`

---

**3b. Parse JSON** (GPT output)
- Content: `@{outputs('AI_Builder_-_Create_text_with_GPT')?['body/text']}`
- Schema:
```json
{
    "type": "object",
    "properties": {
        "category": {"type": "string"},
        "urgency": {"type": "string"},
        "primaryIssue": {"type": "string"},
        "shouldRespond": {"type": "string"}
    }
}
```

---

**3c. Condition**
```
@or(
  equals(body('Parse_JSON_GPT')?['shouldRespond'], "true")
)
```

**If YES:**

---

**3d. Compose email body**
```
Dear @{items('Apply_to_each')?['ReviewerName']},

Thank you for taking the time to share your feedback about your recent 
experience at @{items('Apply_to_each')?['BranchName']}.

We sincerely apologize that your visit did not meet the high standards 
we strive to maintain. Your concerns regarding @{body('Parse_JSON_GPT')?['category']} 
are very important to us.

Issue identified: @{body('Parse_JSON_GPT')?['primaryIssue']}
Priority level: @{body('Parse_JSON_GPT')?['urgency']}

Our team is reviewing your feedback and will take appropriate action to 
address these concerns. We value your input and appreciate you helping 
us improve our services.

If you would like to discuss this further, please feel free to contact 
our main office at (555) 123-4567.

Warm regards,
City Library System Customer Care Team

---
This response was generated using AI analysis:
• Category: @{body('Parse_JSON_GPT')?['category']}
• Urgency: @{body('Parse_JSON_GPT')?['urgency']}
```

---

**3e. Send email**
- Action: **Office 365 Outlook - Send an email (V2)**
- To: `@{items('Apply_to_each')?['ReviewerEmail']}`
- Subject: `Re: Your Feedback on @{items('Apply_to_each')?['BranchName']}`
- Body: Output from Compose
- Importance: Normal

---

**3f. Increment variable**
- Name: SuccessCount
- Value: 1

---

**If NO:** (skip, don't send)

---

**Step 4: Return to Copilot**
- TotalProcessed (Number): `@{length(body('Parse_JSON'))}`
- SuccessfullySent (Number): `@{variables('SuccessCount')}`
- Message (String): `Successfully sent @{variables('SuccessCount')} personalized responses`

**Save the flow**

---

## 🔗 Connect Flows to Copilot Topic

### Update the Topic

Go back to Copilot Studio → Your topic

**After Node 2 (branch selection):**

**Node 3: Call an action**
- Action: GetFilteredReviews
- Input: BranchName = Topic.BranchFilter
- Save outputs as:
  - Topic.ReviewCount
  - Topic.ReviewsJSON
  - Topic.Summary
  - Topic.DisplayText

---

**Node 4: Condition**
- Topic.ReviewCount is greater than 0

**If YES:**

**Node 5: Question**
```
{Topic.Summary}

Would you like me to send automated responses to these reviews?
Each email will be personalized based on AI analysis.

Options:
○ Yes, send personalized responses
○ No, just show me details

Save as: Topic.UserConfirm
```

---

**Node 6: Condition**
- Topic.UserConfirm equals "Yes, send personalized responses"

**If YES:**

**Node 7: Message**
```
Processing reviews... This will take a moment.
```

---

**Node 8: Call an action**
- Action: SendAutomatedResponses
- Input: ReviewsJSON = Topic.ReviewsJSON
- Save outputs as:
  - Topic.TotalProcessed
  - Topic.SuccessfullySent
  - Topic.ResultMessage
  

---

**Node 9: Message**
```
{Topic.ResultMessage}

Each reviewer received a personalized email based on:

• Issue categorization
• Urgency assessment

All emails have been sent successfully!
```

**If NO (don't send):**

**Node 10: Message**
```
{Topic.DisplayText}
```

**If ReviewCount = 0:**

**Node 11: Message**
```
Good news! No flagged reviews found for {Topic.BranchFilter}.
All recent reviews meet our quality standards.
```

---

##  Testing

### Test Scenario 1: End-to-End

```
User: "Process flagged reviews"

Agent: "I'll help you send automated responses..."

Agent: "Which branch would you like to process?"

User: "Downtown Branch"

Agent: [calls GetFilteredReviews flow]

Agent: "Found 5 flagged reviews. Would you like me to send 
       automated responses to these reviews?"

User: "Yes, send personalized responses"

Agent: [calls SendAutomatedResponses flow]

Agent: "Successfully sent 5 personalized responses
       
       Each reviewer received a personalized email based on:

       • Issue categorization
       • Urgency assessment"
```

---

### Test Scenario 2: Gen AI Q&A

```
User: "What are the most common complaints?"

Agent: [uses generative AI to analyze and summarize]

User: "Show me reviews about staff"

Agent: [searches and returns relevant reviews]
```

---

##  What You've Built

### Architecture:

```
CSV Data
    ↓
Dataverse Table
    ↓
Copilot Studio Agent
    ├─ Gen AI Q&A (knowledge base)
    └─ Custom Topic (automation)
         ↓
    Flow 1: Get Reviews
         ↓
    Flow 2: AI Analysis + Email
         ├─ Sentiment Analysis
         ├─ GPT Categorization
         └─ Personalized Emails
```

---

##  Key Capabilities Demonstrated

1. **Natural Language Interface**
   - Users ask questions in plain English
   - Agent understands intent

2. **AI-Powered Analysis**
   - Automatic categorization
   - Urgency assessment

3. **Intelligent Automation**
   - Loops through multiple reviews
   - Personalizes each response
   - Based on AI insights

4. **No Code/Low Code**
   - Built without programming
   - Visual flow design
   - Drag-and-drop configuration

---

##  Extensions (Post-Workshop)

Want to add more? Here's how:

### Extension 1: Add Support Tickets
- Create Dataverse table for tickets
- Add "Create ticket" step in Flow 2
- Track resolution status

### Extension 2: Add Manager Notifications
- Send summary email to manager
- Include all flagged reviews
- Daily digest option

### Extension 3: Add Status Tracking
- Update review status to "Responded"
- Track response timestamp
- Dashboard of processed reviews

### Extension 4: Add AI Search
- Index Dataverse in Azure AI Search
- Better semantic search
- Natural language queries

### Extension 5: Add Teams Integration
- Post flagged reviews to Teams channel
- Get approvals before sending
- Team collaboration

---

## 📚 Key Takeaways

**What makes this powerful:**

1. **Speed:** Responds in minutes vs. days
2. **Consistency:** Professional tone every time
3. **Intelligence:** AI categorizes and prioritizes
4. **Scalability:** Handles 1 or 100 reviews
5. **Personalization:** Each email customized to issue type

**Traditional process:**
- Employee checks reviews daily (30 mins)
- Manually drafts responses (15 mins each)
- 5 reviews = 1.5 hours of work
- Inconsistent quality
- Easy to miss urgent issues

**Your automated process:**
- AI monitors 24/7
- Analyzes and responds in < 5 minutes
- Consistent professional quality
- Never misses a flagged review
- Frees staff for higher-value work

---

## ❓ Common Questions

**Q: How much does this cost?**
A: Included in many Microsoft 365 licenses. AI Builder has consumption pricing.

**Q: Can this work with other data sources?**
A: Yes! SharePoint, SQL, Excel, Dynamics 365, custom APIs, etc.

**Q: What if someone replies to the automated email?**
A: Set up a monitored inbox or add flow to handle replies.

**Q: Can I customize the email templates?**
A: Yes! Edit the Compose step in Flow 2.

**Q: How do I prevent sending duplicate responses?**
A: Add status tracking (Extension 1) or check sent history.

**Q: Can I use this for other review sources?**
A: Yes! Google Reviews, Yelp, internal feedback forms, etc.

---

## 📞 Next Steps

1. **Practice:** Build this in your own environment
2. **Customize:** Adjust for your actual use case
3. **Extend:** Add features from Extensions section
4. **Share:** Show colleagues and stakeholders
5. **Deploy:** Move to production with real data

---

## 🎓 Resources

- Copilot Studio Docs: https://learn.microsoft.com/copilot-studio
- AI Builder Docs: https://learn.microsoft.com/ai-builder
- Power Automate Docs: https://learn.microsoft.com/power-automate
- Community Forums: https://powerusers.microsoft.com

---

**Workshop Complete! 🎉**

You've built an intelligent automation system that demonstrates the power of:
- Conversational AI
- AI Builder
- Power Automate
- Low-code development

*Workshop created: February 2025*
