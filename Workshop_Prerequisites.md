# Copilot Studio Workshop - Pre-Workshop Requirements
## Library Review Automation Workshop

**Duration:** 90 minutes  
**Format:** Hands-on, build-along workshop

---

## 📋 What You'll Need BEFORE the Workshop

### ✅ Required Accounts & Licenses

#### **1. Microsoft 365 Account**
- **What:** Work or school account with admin rights
- **Why:** Needed to access Power Platform services
- **How to verify:**
  - Go to https://portal.office.com
  - Sign in with your account
  - You should see Office apps (Outlook, Word, etc.)

**Don't have one?**
- Contact your IT department, OR
- Sign up for Microsoft 365 Developer Program (free): https://developer.microsoft.com/microsoft-365/dev-program

---

#### **2. Copilot Studio License**
- **What:** License to build AI agents
- **Included in:**
  - Microsoft 365 E3/E5
  - Office 365 E3/E5
  - Dynamics 365 licenses
  - Standalone Copilot Studio license

**How to verify:**
1. Go to https://copilotstudio.microsoft.com
2. Sign in with your Microsoft 365 account
3. If you can see the home page → You have access ✅
4. If you see "No license" error → Request from IT ❌

**Don't have access?**
- Request from your IT department
- OR sign up for free trial: https://aka.ms/TryCopilotStudio (30 days)

---

#### **3. Power Automate License**
- **What:** License to create automated workflows
- **Included in:**
  - Most Microsoft 365 plans (E3/E5, Business Premium)
  - Power Automate Per User license
  - Some Dynamics 365 licenses

**How to verify:**
1. Go to https://make.powerautomate.com
2. Sign in
3. Try creating a new flow
4. If it works → You have access ✅

**Note:** You need **Premium** connectors for Dataverse access (included in most plans)

---

#### **4. AI Builder Credits** (Important!)
- **What:** Credits to use AI capabilities (sentiment analysis, GPT prompts)
- **Included in:**
  - Power Apps/Power Automate Per User licenses (limited)
  - Dynamics 365 licenses
  - AI Builder add-on capacity

**How to check your credits:**
1. Go to https://make.powerapps.com
2. Click ⚙️ (gear icon) → **Admin center**
3. Left menu → **Resources** → **Capacity**
4. Look for "AI Builder" capacity

**Typical usage for workshop:**
- ~50-100 AI Builder credits per participant
- Sentiment analysis: ~1 credit per review
- GPT categorization: ~5 credits per review
- Workshop will process ~20 reviews

**Don't have credits?**
- Sign up for AI Builder trial: https://aka.ms/TryAIBuilder
- OR request from IT department
- Workshop can be completed without AI Builder (simplified version)

---

#### **5. Dataverse for Teams (Minimum) or Dataverse Pro**
- **What:** Database to store review data
- **Included in:**
  - Microsoft 365 E3/E5 (Dataverse for Teams - limited)
  - Power Apps Per User license (full Dataverse)
  - Dynamics 365 licenses (full Dataverse)

**How to verify:**
1. Go to https://make.powerapps.com
2. Click **Tables** in left menu
3. Try creating a new table
4. If you can import data → You have access ✅

**Don't have access?**
- Dataverse for Teams is included with most Microsoft Teams licenses
- Request from IT if blocked

---


#### **6. Azure Subscription (for AI Search demo)**
- **What:** Azure account to create AI Search service
- **Cost:** Free tier available, OR ~$75/month for Basic tier
- **Used for:** Advanced semantic search capabilities (optional module)

**How to get:**
- Azure Free Account: https://azure.microsoft.com/free (12 months free)
- Azure for Students: https://azure.microsoft.com/free/students (if eligible)
- Company Azure subscription (ask IT)


---

#### **7. Power Platform Environment**
- **What:** Isolated workspace for building solutions
- **You need:** "Maker" or "Admin" permissions in an environment

**How to verify:**
1. Go to https://make.powerapps.com
2. Top-right corner → Check environment name
3. Can you create apps/flows? → You have access ✅

**Don't have environment access?**
- Ask IT to create a "Developer" environment for you
- OR create personal developer environment (if allowed by your org)

---

## 💻 Technical Requirements

### **Computer Setup**

✅ **Operating System:**
- Windows 10/11
- macOS 10.15+

✅ **Browser (CRITICAL):**
- **Recommended:** Microsoft Edge (Chromium) or Google Chrome (latest version)
- **Alternative:** Firefox (latest version)
- ❌ **Not supported:** Internet Explorer, Safari (limited support)

✅ **Internet Connection:**
- Stable broadband connection (minimum 5 Mbps)
- Access to *.microsoft.com, *.azure.com domains
- No corporate firewall blocking Power Platform


---

## 📁 Files to Download

### **Workshop Materials**

**Before the workshop, download these files:**

1. **library_reviews.csv** (provided by instructor)
   - Sample data with 20 library reviews
   - You'll import this into Dataverse

2. **Workshop guide PDF** (provided by instructor)
   - Step-by-step instructions
   - Reference during the workshop


---

## 🔧 Pre-Workshop Setup Tasks (15-30 minutes)

### **Complete These BEFORE Attending:**

#### **Task 1: Verify Access (10 mins)**

✅ Sign into each portal and verify access:

1. Copilot Studio: https://copilotstudio.microsoft.com
   - Can you see the home page?
   - Can you click "Create"?

2. Power Automate: https://make.powerautomate.com
   - Can you see "My flows"?
   - Can you create a new flow?

3. Power Apps: https://make.powerapps.com
   - Can you see "Tables"?
   - Can you create a new table?


---

#### **Task 2: Create a Test Flow (10 mins)**

**Verify your Power Automate works:**

1. Go to https://make.powerautomate.com
2. Click **+ Create** → **Instant cloud flow**
3. Name: "Test Flow"
4. Trigger: **Manually trigger a flow**
5. Click **Create**
6. Add action: **Compose**
   - Inputs: `Hello Workshop!`
7. Click **Save**
8. Click **Test** → **Manually** → **Test**
9. Click **Run flow**
10. Should see "Your flow ran successfully" ✅

**If this fails:** Your Power Automate license may have issues. Contact IT.


---

#### **Task 3: Test AI Builder Access (5 mins - Optional)**

1. Go to https://make.powerapps.com
2. Left menu → **AI Builder** → **Explore**
3. Click **Sentiment Analysis**
4. Can you see the demo? → You have access ✅

**If blocked:** You may need AI Builder credits (see #4 above)

---

## 📧 Information to Bring

**Write down these details (you'll need them):**

1. **Your Microsoft 365 email:** _______________________
2. **Your environment name:** ________________________
3. **Your tenant name:** ____________________________
4. **Do you have AI Builder credits?** Yes / No / Don't Know
5. **Do you have Azure subscription?** Yes / No / Not Needed

---

## 🚨 Common Issues & Solutions

### **Issue 1: "You don't have access to Copilot Studio"**
**Solution:**
- Request Copilot Studio license from IT
- Sign up for free trial: https://aka.ms/TryCopilotStudio

---

### **Issue 2: "Premium connector required"**
**Solution:**
- You need Power Automate Per User license
- Request from IT department
- Trial license: https://aka.ms/TryPowerAutomate

---

### **Issue 3: "AI Builder capacity exceeded"**
**Solution:**
- Request AI Builder capacity from IT
- Sign up for AI Builder trial
- Workshop has non-AI fallback option

---

### **Issue 4: "Can't create Dataverse table"**
**Solution:**
- Ask IT for environment with Dataverse
- Use Dataverse for Teams (limited but sufficient)
- Verify you have "Environment Maker" role

---

### **Issue 5: Corporate firewall blocking Power Platform**
**Solution:**
- Request IT to whitelist:
  - *.microsoft.com
  - *.powerapps.com
  - *.dynamics.com
  - *.azure.com
- Use personal laptop with mobile hotspot (last resort)

---

## 📚 Optional: Pre-Reading (20 mins)

**Familiarize yourself with these concepts:**

1. **What is Copilot Studio?**
   - Video: https://aka.ms/LearnCopilotStudio (5 mins)

2. **What is Power Automate?**
   - Video: https://aka.ms/LearnPowerAutomate (5 mins)

3. **What is AI Builder?**
   - Article: https://aka.ms/WhatIsAIBuilder (10 mins)

**Not required, but helps you get more from the workshop!**

---

## ✅ Pre-Workshop Checklist

**Complete this checklist 24-48 hours before the workshop:**

- [ ] Microsoft 365 account verified (can sign in)
- [ ] Copilot Studio access confirmed
- [ ] Power Automate access confirmed
- [ ] AI Builder credits available (or trial signed up)
- [ ] Dataverse access verified (can create tables)
- [ ] Test flow created successfully
- [ ] Browser updated to latest version
- [ ] Workshop files downloaded
- [ ] Workshop folder created on computer
- [ ] Read optional pre-reading materials (optional)
- [ ] Laptop fully charged / power adapter ready
- [ ] Stable internet connection verified
- [ ] Noted down any access issues to report

---

**Common questions:**
- "I can't access Copilot Studio" → Contact IT for license
- "I don't have AI Builder credits" → Sign up for trial
- "My firewall blocks Power Platform" → Contact IT for access
- "I'm completely new to this" → Perfect! Workshop assumes no prior experience

---

## 📅 Workshop Day

**What to bring:**
- ✅ Laptop with charger
- ✅ Notebook and pen for notes
- ✅ Downloaded workshop files
- ✅ Login credentials ready

**What NOT to bring:**
- ❌ Tablet or iPad (not sufficient for workshop)
- ❌ Chromebook (limited Power Platform support)
- ❌ Outdated browser (must be latest version)

---

## 🎯 What You'll Build

By the end of the workshop, you'll have:

✅ A working AI agent that:
- Analyzes customer reviews
- Detects negative sentiment
- Categorizes issues automatically
- Sends personalized email responses
- Loops through multiple reviews

✅ Skills you'll learn:
- Building Copilot Studio agents
- Creating Power Automate flows
- Using AI Builder for sentiment analysis
- Integrating AI with automation
- Processing data from Dataverse

✅ Take-home materials:
- Complete working solution
- Workshop guide
- Extension ideas

---

## 💡 Tips for Success

1. **Come with a beginner's mindset** - No prior experience needed!
2. **Ask questions** - There are no dumb questions
3. **Follow along** - Hands-on practice is key
4. **Take screenshots** - Helps you remember steps
5. **Connect with peers** - Learn from each other
6. **Think of your own use cases** - How can you apply this?

---

## 📞 Day-Before Checklist

**24 hours before workshop, verify:**

- [ ] All accounts accessible
- [ ] Laptop fully functional
- [ ] Power adapter packed
- [ ] Environment permissions confirmed
- [ ] AI Builder credits available
- [ ] No conflicting meetings scheduled
- [ ] Workshop location/link saved
- [ ] Ready to learn! 🚀

---

## 🎉 See You at the Workshop!

You're all set! If you've completed this checklist, you're ready for an amazing hands-on experience building intelligent automation.

**Workshop starts:** 12:40 PM PST
**Duration:** 90 minutes  

Looking forward to building with you! 💪


---

## 📎 Quick Links Reference

**Portals:**
- Copilot Studio: https://copilotstudio.microsoft.com
- Power Automate: https://make.powerautomate.com
- Power Apps: https://make.powerapps.com
- Azure Portal: https://portal.azure.com

**Free Trials:**
- Copilot Studio: https://aka.ms/TryCopilotStudio
- Power Automate: https://aka.ms/TryPowerAutomate
- AI Builder: https://aka.ms/TryAIBuilder
- Azure Free: https://azure.microsoft.com/free

**Learning Resources:**
- Copilot Studio Docs: https://learn.microsoft.com/copilot-studio
- Power Automate Docs: https://learn.microsoft.com/power-automate
- AI Builder Docs: https://learn.microsoft.com/ai-builder

**Support:**
- Power Platform Community: https://powerusers.microsoft.com
- Microsoft Q&A: https://aka.ms/PowerPlatformQA

---

*Version 1.0 | Last Updated: [Feb 18,2026]*
