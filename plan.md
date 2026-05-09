# Project Plan: Kasparro AI Shopping Agent

This document outlines the end-to-end plan for building the AI Shopping Agent for the Kasparro Hackathon (Track 1). It covers the product vision, technical architecture, implementation phases, and the mandatory documentation strategy.

## 1. Product Vision (Track 1: AI Shopping Agent)
**Goal:** Replace the tedious "browse-search-filter-compare" loop with a single, intelligent conversation that moves the user from vague intent to purchase.

### Core User Journey
1. **Initial Vague Query:** User says, "I need an outfit for a summer beach wedding."
2. **Smart Clarification:** The agent doesn't just show all summer clothes. It asks 1-2 targeted questions (e.g., "Are you looking for something formal like a suit, or more smart-casual like linen pants and a button-down? What's your budget?").
3. **Intelligent Narrowing:** The agent queries the Shopify store via API, analyzing product metadata.
4. **Explainable Recommendations:** The agent presents 2-3 options. It explicitly handles tradeoffs (e.g., "Option A is 100% linen and highly breathable, but slightly over your budget. Option B is a cotton blend, strictly under budget, but might be warmer.").
5. **Clean Checkout Path:** Once the user decides, the agent generates a direct Shopify checkout link with the selected items.

## 2. System Architecture

### Tech Stack
*   **Frontend:** Next.js (React), TailwindCSS, Framer Motion (for smooth chat animations).
*   **Backend:** Next.js API Routes (Serverless functions).
*   **AI / LLM:** OpenAI API (GPT-4o) or Google Gemini 1.5 Pro, utilizing **Function Calling / Tools**.
*   **E-commerce Data:** Shopify Storefront API (GraphQL) to fetch real-time inventory, pricing, and create checkouts.

### Data Flow
1. User sends a message via the UI.
2. Next.js backend receives the message and appends it to the conversation history.
3. LLM processes the history. If it needs product data, it triggers a `search_shopify_products` function call.
4. Backend executes the deterministic Shopify GraphQL query.
5. Backend returns the structured product data (JSON) to the LLM.
6. LLM synthesizes the data into a conversational response, grounding its recommendations in the actual store data.
7. User selects a product to buy -> Backend triggers `create_shopify_checkout` and returns the URL.

### AI vs. Deterministic Boundary
*   **AI Handles:** Intent extraction, dialogue management, tradeoff explanation, and deciding *when* to search.
*   **Deterministic Code Handles:** Executing Shopify API calls, managing cart state, generating checkout URLs, and fetching exact pricing/inventory (to prevent hallucinations).

## 3. Implementation Phases (May 10 - May 20)

### Phase 1: Foundation (Days 1-2)
*   **Shopify Setup:** Create a Shopify Partner account and a Development Store. Populate it with synthetic data (e.g., a high-end apparel store or electronics shop) with rich descriptions and variants.
*   **App Scaffold:** Initialize Next.js project, setup Tailwind, and build the basic chat UI layout.
*   **API Connection:** Setup Shopify Storefront API access tokens and test basic GraphQL queries.

### Phase 2: The Agentic Core (Days 3-5)
*   **LLM Integration:** Connect the OpenAI/Gemini SDK.
*   **Tool Creation:** Define the function schemas for the LLM:
    *   `search_products(query, min_price, max_price, category)`
    *   `get_product_details(product_id)`
    *   `create_checkout(variant_ids)`
*   **Execution Loop:** Build the logic to handle the LLM's function calls, execute them against Shopify, and return the results to the LLM.

### Phase 3: UX & Tradeoff Polish (Days 6-7)
*   **Prompt Engineering:** Refine the system prompt heavily so the agent:
    *   Asks follow-up questions instead of dumping lists.
    *   Explicitly compares items (Price vs. Quality, Availability vs. Preference).
    *   Explains *why* an item was recommended.
*   **UI Enhancements:** Render products beautifully in the chat. Instead of just text, render rich "Product Cards" when the LLM recommends items.

### Phase 4: Resilience & Failure Handling (Day 8)
*   **Shopify API Down:** If Shopify fails, catch the error, log it, and have the bot gracefully say: "I'm having trouble accessing the store's inventory right now. Please try again in a moment."
*   **LLM Hallucinations:** Enforce strict grounding. The LLM must only recommend products returned by the tool calls.
*   **Unexpected Input:** If the user asks about the weather or non-store topics, gracefully pivot back: "I can only help you find products in our store. Are you looking for [Category] today?"

### Phase 5: Documentation & Submission (Days 9-10)
*   **Draft Product Document:** Focus on the "Why", user journey, and scope decisions.
*   **Draft Technical Document:** Focus on architecture, the AI/Deterministic boundary, and failure handling.
*   **Decision Log:** Document 3-4 key engineering/product choices.
*   **Record Demo Video:** 3-5 minute walkthrough explaining the agent's logic while demonstrating a purchase flow.
*   **Final GitHub Polish:** Ensure the README is pristine and all checklist items are ticked.

## 4. Documentation Strategy

### Product Document Outline
*   **Problem:** The paradox of choice in modern e-commerce. Filter-based search is broken for vague intents.
*   **Target User:** Time-poor buyers who know what they want to achieve (e.g., "look good for a party") but don't know the specific product.
*   **Core Journey:** Vague Intent -> Clarification -> Grounded Recommendation w/ Tradeoffs -> Checkout.
*   **Scope Decisions (What we DIDN'T build):** We didn't build order tracking or returns (Track 4), or checkout recovery (Track 2). We focused purely on discovery-to-cart.

### Technical Document Outline
*   **Architecture Diagram:** User -> Next.js -> LLM Router -> Shopify API.
*   **AI Boundary:** LLM is the brain, Shopify API is the source of truth. No pricing or specs are generated without an API call.
*   **Failure Handling:** Detailed retry logic for APIs and graceful degradation for LLM timeouts.

### Decision Log (Examples)
1.  **Considered:** Vector Database (RAG) for product search.
    **Chose:** Direct Shopify GraphQL search.
    **Because:** Shopify's native search API is sufficient for our synthetic catalog size, ensures 100% real-time accuracy for pricing/inventory, and reduces architectural complexity.
2.  **Considered:** Rendering standard text responses for products.
    **Chose:** Intercepting structured data to render Custom React Components (Product Cards) in the chat.
    **Because:** A purely text-based shopping experience lacks the visual trust required for e-commerce. Visual cards bridge conversational AI with traditional e-commerce UX.
