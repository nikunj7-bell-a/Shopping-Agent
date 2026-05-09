# Kasparro AI Shopping Agent

An autonomous digital assistant that replaces the traditional browse-search-filter-compare loop with an intelligent, conversational e-commerce experience. Built for the Kasparro Agentic Commerce Hackathon (Track 1: AI Shopping Agent).

## 🚀 Overview

Consumers are moving away from traditional search engines toward AI-powered, conversational search ("agentic commerce"). Traditional e-commerce forces users into a tedious loop of filtering and comparing, leading to decision fatigue and high cart abandonment. 

This project solves this by introducing an intelligent, 24/7 in-store expert that:
1. **Understands Context:** Interprets vague queries (e.g., "outfit for a summer wedding") instead of just matching keywords.
2. **Narrows Options Intelligently:** Asks smart follow-up questions to refine preferences and budget.
3. **Explains Tradeoffs:** Compares products explicitly (e.g., price vs. quality, availability vs. preference) so the user understands *why* a product is recommended.
4. **Streamlines Checkout:** Supports a clean, direct path from intent to purchase by generating tailored checkout links.

## 📋 Hackathon Submission Checklist

- [x] **Product Document:** [docs/product_document.md](./docs/product_document.md) *(Pending implementation)*
- [x] **Technical Document:** [docs/technical_document.md](./docs/technical_document.md) *(Pending implementation)*
- [x] **Decision Log:** [docs/decision_log.md](./docs/decision_log.md) *(Pending implementation)*
- [x] **Demo Video:** [YouTube Link Here] *(Placeholder)*
- [x] **Screenshots:** [See below or link to assets] *(Placeholder)*

## 🛠️ Setup Instructions

### Prerequisites
- Node.js (v18+)
- A Shopify Partner Account & Development Store
- OpenAI API Key (or alternative LLM provider)

### 1. Clone the Repository
```bash
git clone https://github.com/nikunj7-bell-a/Shopping-Agent.git
cd Shopping-Agent
```

### 2. Install Dependencies
```bash
npm install
```

### 3. Environment Variables
Create a `.env.local` file in the root directory and add the following:
```env
OPENAI_API_KEY=your_openai_api_key
SHOPIFY_STORE_DOMAIN=your-dev-store.myshopify.com
SHOPIFY_STOREFRONT_ACCESS_TOKEN=your_storefront_access_token
```

### 4. Run the Development Server
```bash
npm run dev
```
Navigate to `http://localhost:3000` to interact with the AI Shopping Agent.

## 🤝 Contribution Note

**[Solo Participant Name / Team Members]**

*If Solo:*
- **Product Thinking (50%):** Defined the user journey, identified key friction points in traditional e-commerce search, and mapped out the tradeoffs the AI needs to handle to build trust.
- **Engineering (50%):** Implemented the Next.js frontend, integrated the OpenAI function calling loop, and connected the Shopify Storefront API for real-time data grounding and checkout generation.

*If Team:*
- **[Name 1]:** Led product framing, UX design, and wrote the Product Document.
- **[Name 2]:** Led technical architecture, LLM integration, Shopify API connecting, and wrote the Technical Document.
- **Joint Effort:** Designing the core prompt logic and decision handling for product tradeoffs.

## 📸 Screenshots / Walkthrough
*(Add screenshots of the chat interface demonstrating: a vague query, the agent asking a clarifying question, the agent explaining a tradeoff, and the final checkout link).*