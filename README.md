# n8n_Chatbot
🤖 AI-powered customer service automation for sports shops using n8n, OpenAI GPT-4, and Supabase Vector Store. Automatically answers customer emails with accurate product information, promotions, and inventory details.
An intelligent, fully automated customer service system for sports shops that uses RAG (Retrieval-Augmented Generation) to provide accurate, contextual responses to customer inquiries via email.
🎯 Problem It Solves
Sports shops receive hundreds of repetitive customer emails daily asking about:

Product availability and specifications
Pricing and promotions
Size and stock information
General sports equipment advice

Manually responding to each email is:

⏰ Time-consuming
💸 Expensive (requires dedicated staff)
😴 Inconsistent (human error, varying response quality)
🌙 Limited to business hours

✨ Solution
Genie is an AI-powered agent that automatically:

Monitors your Gmail inbox for customer inquiries
Searches your product knowledge base using semantic vector search
Generates accurate, friendly responses using GPT-4
Sends personalized replies instantly - 24/7

🚀 Features

🤖 Fully Automated Email Responses - No human intervention required
📚 Smart Knowledge Base - Uses Supabase vector store for semantic search
🧠 Context-Aware AI - GPT-4 powered responses with conversation memory
📁 Auto-Sync Product Data - Google Drive integration for easy knowledge updates
💬 Natural Conversations - PostgreSQL chat memory maintains context across emails
🎯 Accurate & Factual - Only responds with verified information from your database
⚡ Real-Time Updates - Add new products/FAQs and they're instantly available

🏗️ Architecture
┌─────────────────┐
│  Gmail Trigger  │ ← Monitors inbox every minute
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│   AI Agent      │ ← Processes customer query
│   (GPT-4)       │
└────────┬────────┘
         │
         ├──────────────────┐
         │                  │
         ▼                  ▼
┌─────────────────┐  ┌──────────────────┐
│ Supabase Vector │  │ PostgreSQL       │
│ Store (RAG)     │  │ Chat Memory      │
└─────────────────┘  └──────────────────┘
         │
         ▼
┌─────────────────┐
│ Gmail Reply     │ ← Sends response
└─────────────────┘

┌──────────────────────┐
│ Google Drive Trigger │ ← Watches FAQ folder
└──────────┬───────────┘
           │
           ▼
┌──────────────────────┐
│ Download & Embed     │ ← Auto-updates knowledge base
└──────────────────────┘
🛠️ Tech Stack
ComponentTechnologyPurposeWorkflow Automationn8nOrchestrates the entire workflowAI ModelOpenAI GPT-4.1-miniGenerates intelligent responsesVector DatabaseSupabaseStores and retrieves product knowledgeEmbeddingsOpenAI EmbeddingsConverts text to semantic vectorsEmail IntegrationGmail APIReceives and sends emailsKnowledge SourceGoogle DriveStores FAQ documentsMemoryPostgreSQLMaintains conversation context
📋 Prerequisites

n8n instance (self-hosted or cloud)
OpenAI API key
Supabase account and project
Google Account with Gmail and Drive access
PostgreSQL database (for chat memory)

⚙️ Setup Instructions
1. Clone the Workflow

Copy the workflow JSON from this repository
Open your n8n instance
Click "Import from File" or paste the JSON directly

2. Configure Credentials
Set up the following credentials in n8n:
OpenAI API:

Get your API key from OpenAI Platform
Add to n8n credentials as "OpenAI API"

Gmail:

Enable Gmail API in Google Cloud Console
Create OAuth 2.0 credentials
Add to n8n as Gmail credentials

Google Drive:

Use the same Google Cloud project
Enable Google Drive API
Add credentials to n8n

Supabase:

Create a new Supabase project
Create a table named documents with vector extension
Get your project URL and API key
Add to n8n credentials

3. Set Up Supabase Vector Store
Run this SQL in your Supabase SQL editor:
sql-- Enable the pgvector extension
create extension if not exists vector;

-- Create the documents table
create table documents (
  id bigserial primary key,
  content text,
  metadata jsonb,
  embedding vector(1536)
);

-- Create an index for faster similarity search
create index on documents using ivfflat (embedding vector_cosine_ops)
  with (lists = 100);
4. Configure Google Drive Folder

Create a folder in Google Drive for your FAQs (e.g., "FAQs")
Copy the folder ID from the URL
Update the "Google Drive Trigger" node with your folder ID
Upload your product FAQs, inventory lists, and promotion documents

5. Customize the AI Agent
Edit the AI Agent node system message to match your:

Shop name and branding
Product categories
Tone and personality
Specific business rules

6. Test the Workflow

Activate the workflow
Send a test email to your connected Gmail
Verify the AI response
Check Supabase to ensure knowledge is being retrieved

📝 Usage
Adding New Products/FAQs

Create a document (PDF, DOCX, or TXT) with product information
Upload to your Google Drive "FAQs" folder
The workflow automatically:

Downloads the file
Extracts and embeds the content
Stores it in Supabase vector store


Information is immediately available for customer queries

Monitoring Performance

Check n8n execution logs for successful email handling
Review AI responses in Gmail
Monitor Supabase for vector store queries

🎨 Customization Ideas

Multi-language support: Add translation nodes
Sentiment analysis: Route angry customers to human support
Product recommendations: Enhance AI with recommendation logic
Analytics dashboard: Track common questions and response times
Slack notifications: Alert team for high-priority inquiries
WhatsApp/SMS integration: Expand beyond email

🤝 Contributing
Contributions are welcome! Please feel free to submit a Pull Request.

Fork the repository
Create your feature branch (git checkout -b feature/AmazingFeature)
Commit your changes (git commit -m 'Add some AmazingFeature')
Push to the branch (git push origin feature/AmazingFeature)
Open a Pull Request

📊 Example Use Cases

Inventory Inquiries: "Do you have size 10 Nike Air Zoom?"
Pricing Questions: "What's the price of Wilson basketballs?"
Promotions: "Are there any deals on running shoes?"
Product Recommendations: "What's the best tennis racket for beginners?"
Availability: "When will the Adidas tracksuit be back in stock?"

⚠️ Limitations

Requires active n8n instance (consider hosting costs)
OpenAI API usage costs (monitor token consumption)
Gmail API has rate limits (check Google quotas)
Response quality depends on knowledge base completeness

🔒 Security Considerations

Never commit API keys or credentials
Use environment variables for sensitive data
Implement email filtering to prevent spam
Review AI responses periodically for accuracy
Set up rate limiting to prevent API abuse

📄 License
This project is licensed under the MIT License - see the LICENSE file for details.
🙏 Acknowledgments

n8n - Workflow automation platform
OpenAI - GPT-4 language model
Supabase - Vector database and backend
Community contributors and testers

📧 Support
For questions or support:

Open an issue in this repository
Check n8n community forums
Review n8n documentation

🗺️ Roadmap

 Add support for multiple languages
 Implement advanced analytics dashboard
 Create web widget for website integration
 Add voice message support
 Integrate with e-commerce platforms
 Implement A/B testing for responses
