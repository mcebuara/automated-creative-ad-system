# 🚀 Quick Start Guide

Get your Automated Creative ROI System up and running in 30 minutes.

## Prerequisites Checklist

Before you begin, ensure you have:

- [ ] n8n instance (cloud or self-hosted)
- [ ] Airtable account with API access
- [ ] Meta Business Manager with ad account
- [ ] OpenAI API key with GPT-4 access
- [ ] Slack workspace (optional but recommended)

---

## Step 1: Get Your API Keys (10 mins)

### Meta Business Access Token

1. Go to [Meta Business Suite](https://business.facebook.com/)
2. Navigate to **Business Settings** → **Users** → **System Users**
3. Click **Add** and create a new system user named "n8n Automation"
4. Click **Generate New Token**
5. Select your Ad Account and grant these permissions:
   - `ads_management`
   - `ads_read`
   - `business_management`
6. Copy the access token (starts with `EAAA...`)
7. Save it securely - **you'll only see it once**

### Airtable API Key

1. Go to [Airtable Account Settings](https://airtable.com/account)
2. Click **Generate API key** or use personal access token
3. Copy the key (starts with `key...` or `pat...`)

### OpenAI API Key

1. Go to [OpenAI API Keys](https://platform.openai.com/api-keys)
2. Click **Create new secret key**
3. Name it "n8n Creative System"
4. Copy the key (starts with `sk-...`)
5. Ensure you have GPT-4 access enabled

### Slack Webhook (Optional)

1. Go to [Slack API Apps](https://api.slack.com/apps)
2. Click **Create New App** → **From scratch**
3. Name it "Creative ROI Bot"
4. Select your workspace
5. Go to **Incoming Webhooks** → Enable
6. Click **Add New Webhook to Workspace**
7. Select channel (e.g., `#marketing-automation`)
8. Copy webhook URL

---

## Step 2: Set Up Airtable (5 mins)

### Option A: Use Template (Easiest)

1. Click this link: [Airtable Template](#) *(coming soon)*
2. Click "Use this base"
3. Rename to "Creative ROI System"
4. Copy the Base ID from the URL: `airtable.com/appXXXXXXXXXXXXXX`

### Option B: Manual Setup

Create a new base with three tables:

**Table 1: Client_Briefs**
```
Fields:
- Client_ID (Single line text)
- Campaign_Name (Single line text)
- Target_Audience (Long text)
- Product_Description (Long text)
- Brand_Voice (Single select: Professional, Friendly, Urgent, Authoritative, Playful)
- Key_Benefits (Long text)
- Status (Single select: Active, Paused, Completed)
- Next_Creative_Date (Date)
```

**Table 2: Ad_Creatives**
```
Fields:
- Client_ID (Single line text)
- Campaign_Name (Single line text)
- Variant_Number (Number)
- Headline (Single line text)
- Primary_Text (Long text)
- Description (Long text)
- CTA (Single line text)
- Status (Single select: pending_approval, approved, deployed, paused, winner)
- Meta_Ad_ID (Single line text)
- Meta_Adset_ID (Single line text)
- Meta_Campaign_ID (Single line text)
- Impressions (Number)
- Clicks (Number)
- Spend (Currency, USD)
- Conversions (Number)
- CTR (Percent, 2 decimals)
- CPC (Currency, USD)
- CPM (Currency, USD)
- Conversion_Rate (Percent, 2 decimals)
- CPA (Currency, USD)
- ROAS (Number, 2 decimals)
- Quality_Score (Number, 0-100)
- Deployed_At (Date)
- Last_Updated (Date)
- Optimization_Status (Single select: scale, maintain, reduce, pause)
- Composite_Score (Number, 2 decimals)
```

**Table 3: Audiences**
```
Fields:
- Campaign_Name (Single line text)
- Source_ROAS (Number, 2 decimals)
- Source_Conversions (Number)
- Custom_Audience_ID (Single line text)
- LAL_1_Percent_ID (Single line text)
- LAL_3_Percent_ID (Single line text)
- LAL_5_Percent_ID (Single line text)
- Created_At (Date)
- Status (Single select: active, paused, archived)
```

---

## Step 3: Import Workflows to n8n (5 mins)

1. Download all workflow files from this repo:
   ```bash
   git clone https://github.com/mcebuara/automated-creative-roi-system.git
   cd automated-creative-roi-system/workflows
   ```

2. In n8n, click **Add Workflow** → **Import from File**

3. Import in this order:
   - ✅ `01-creative-generation.json`
   - ✅ `02-performance-monitoring.json`
   - ✅ `03-winner-analysis.json`
   - ✅ `04-meta-deployment.json`
   - ✅ `05-audience-intelligence.json`

4. Each workflow will appear in your workflows list (initially inactive)

---

## Step 4: Configure n8n (10 mins)

### Add Environment Variables

1. In n8n, go to **Settings** → **Environments** → **Variables**
2. Add these variables:

```bash
AIRTABLE_BASE_ID=appXXXXXXXXXXXXXX          # From Airtable URL
META_AD_ACCOUNT_ID=act_XXXXXXXXXXXXX         # From Meta Business Manager
META_ACCESS_TOKEN=EAXXXXXXXXXXXXXXXXXX       # From Step 1
META_PAGE_ID=XXXXXXXXXXXXX                   # Your Facebook Page ID
META_PIXEL_ID=XXXXXXXXXXXXX                  # Your Meta Pixel ID
LANDING_PAGE_URL=https://example.com         # Your landing page
```

**How to find IDs:**
- **Ad Account ID:** Business Settings → Accounts → Ad Accounts → Look for `act_XXXXX`
- **Page ID:** Facebook Page → About → Page ID
- **Pixel ID:** Events Manager → Data Sources → Your Pixel

### Add Credentials

1. Go to **Credentials** → **Add Credential**

2. **Airtable:**
   - Type: `Airtable Token API`
   - Personal Access Token: `[paste your token]`
   - Test connection → Save

3. **OpenAI:**
   - Type: `OpenAI API`
   - API Key: `[paste your sk-... key]`
   - Organization ID: (optional)
   - Test connection → Save

4. **Slack:**
   - Type: `Slack API`
   - Token: `[paste webhook URL or bot token]`
   - Test connection → Save

### Link Credentials to Workflows

For each workflow:
1. Open the workflow
2. Click on nodes with credential icons (Airtable, OpenAI, Slack)
3. Select your saved credentials from dropdown
4. Save workflow

---

## Step 5: Test Your Setup (5 mins)

### Test Creative Generation

1. Add a test brief to Airtable:
   ```
   Client_ID: TEST001
   Campaign_Name: Test Campaign
   Target_Audience: Tech-savvy millennials
   Product_Description: Wireless earbuds
   Brand_Voice: Friendly
   Key_Benefits: Long battery, noise cancellation, affordable
   Status: Active
   Next_Creative_Date: [Today]
   ```

2. Open **Creative Generation** workflow in n8n

3. Click **Execute Workflow** (top right)

4. Watch nodes execute (green = success, red = error)

5. Check Airtable `Ad_Creatives` table - you should see 5 new rows!

### Test Performance Monitoring

1. Manually add a Meta Ad ID to one creative in Airtable:
   ```
   Meta_Ad_ID: 120210000000000  # Use a real ad ID or test ID
   Status: deployed
   ```

2. Open **Performance Monitoring** workflow

3. Click **Execute Workflow**

4. Check if metrics are fetched (check node output)

### Activate Workflows

Once tests pass:
1. Open each workflow
2. Toggle **Active** (top right)
3. Workflows now run on schedule automatically!

---

## Step 6: Create Your First Real Campaign (5 mins)

1. **Add Real Client Brief** in Airtable:
   ```
   Client_ID: CLIENT001
   Campaign_Name: Holiday Sale 2024
   Target_Audience: Women 25-45, interested in home decor
   Product_Description: Handmade ceramic vases, modern minimalist design
   Brand_Voice: Professional
   Key_Benefits: Unique designs, sustainable materials, free shipping
   Status: Active
   Next_Creative_Date: [Today]
   ```

2. **Wait for Generation** (runs at midnight, or trigger webhook):
   ```bash
   curl -X POST https://your-n8n.com/webhook/generate-creative
   ```

3. **Review Creatives** in Airtable after 2-3 minutes

4. **Approve Winners:** Change status to `approved` for 3-5 variants

5. **Deploy to Meta:**
   ```bash
   curl -X POST https://your-n8n.com/webhook/deploy-ads
   ```

6. **Monitor in Slack:** You'll get deployment confirmation

7. **Watch Optimization:** Daily reports will show winners after 24-48 hours

---

## Verification Checklist

Before going live, verify:

- [ ] All 5 workflows are imported and active
- [ ] Environment variables are set correctly
- [ ] All credentials are linked and tested
- [ ] Airtable tables exist with correct field names
- [ ] Test creative generation succeeded
- [ ] Slack notifications are received
- [ ] Meta API calls work (test in Meta sandbox if possible)

---

## Next Steps

✅ **You're ready!** The system is now automated.

**What happens automatically:**
- ⏰ **Daily:** Creative generation for active briefs
- ⏰ **Hourly:** Performance metrics updated
- ⏰ **Daily:** Winner analysis and budget optimization
- ⏰ **Weekly:** Lookalike audience creation

**What you need to do:**
- Add new client briefs when needed
- Review and approve generated creatives
- Trigger deployment webhook after approval
- Monitor Slack for reports and alerts

---

## Getting Help

**Issues during setup?**

1. Check the [Troubleshooting Guide](README.md#-troubleshooting)
2. Review n8n execution logs (click on failed nodes)
3. Open a [GitHub Issue](https://github.com/mcebuara/automated-creative-roi-system/issues)
4. Join [n8n Community](https://community.n8n.io)

**Common first-time issues:**
- ❌ Credentials not linked → Link them in each workflow
- ❌ Wrong field names → Must match Airtable exactly (case-sensitive)
- ❌ Meta token expired → Generate new token (expires every 60 days)
- ❌ OpenAI rate limit → Wait 60 seconds or upgrade plan

---

## Video Walkthrough

*(Coming soon: Full setup video tutorial)*

---

🎉 **Congratulations!** Your automated creative system is live. Time to scale your ad campaigns effortlessly!