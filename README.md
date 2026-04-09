# Skrimpers Platform

Built and operated by a solo founder with 25+ years leading large-scale technology programs at UBS, Credit Suisse, Bank of America, etc.

**AI-powered Swiss price intelligence platform**

Live at [skrimpers.com](https://skrimpers.com)

---

## What Is This?

Skrimpers is a production consumer platform that answers a simple question: **where should I shop in Switzerland to get the best prices?**

It tracks prices across 9 major Swiss grocery retailers, monitors 6,000+ fuel stations across Switzerland and 100km into neighboring countries (Germany, France, Italy, Austria), and maps 17,000+ EV charging stations — all updated daily through automated data pipelines.

I designed, architected, and built the entire platform solo, using AWS and AI-native development practices with Claude as my engineering team. The platform is live in production with real users today.

---

## The Numbers

| Metric | Value |
|--------|-------|
| Grocery retailers tracked | 9 (Migros, Coop, Lidl, Aldi, Denner, Aligro, Prodega, Ottos, TopCC) |
| Products monitored daily | 130,000+ |
| Fuel stations | 6,000+ (CH + 100km cross-border DE/FR/IT/AT) |
| EV charging stations | 17,000+ (CH + 100km cross-border DE/FR/IT/AT) |
| Languages | 4 (German, French, English, Italian) |
| Lambda functions | 22 |
| Consumer app components | 55+ |
| Admin dashboard pages | 30 |
| Data pipeline stages | 4-layer medallion (Raw, Bronze, Silver, Gold) |

---

## Architecture

```
                                    SKRIMPERS PLATFORM

    DATA ACQUISITION                    ETL PIPELINE                      SERVING
    ================                    ============                      =======

    +------------------+          +-------------------+            +------------------+
    | 9 Store Scrapers |          |   Raw (S3)        |            |   DynamoDB       |
    | (Scrapy on ECS   |  -----+  |   Bronze (clean)  |  -------+  |   (products,     |
    |  Fargate)        |          |   Silver (enrich) |            |    prices,       |
    +------------------+          |   Gold (serve)    |            |    promotions)   |
                                  +-------------------+            +------------------+
    +------------------+                    |                              |
    | Fuel & EV Data   |          +-------------------+            +------------------+
    | (API ingestion)  |          | 22 Lambda Functions|            | REST API         |
    +------------------+          | (Python, ARM64)   |            | (API Gateway)    |
                                  +-------------------+            +------------------+
    +------------------+                    |                              |
    | Flyer Scanner    |          +-------------------+            +------------------+
    | (OCR pipeline)   |          | Step Functions    |            | Consumer App     |
    +------------------+          | (orchestration)   |            | (React 19, PWA)  |
                                  +-------------------+            +------------------+
                                            |
                                  +-------------------+            +------------------+
                                  | AI Classification |            | Admin Dashboard  |
                                  | (Bedrock / Claude)|            | (React 18)       |
                                  +-------------------+            +------------------+
```

---

## Tech Stack

### Data Acquisition
- **Scrapers**: Python 3.11+, Scrapy framework
- **Infrastructure**: ECS Fargate (containerized), ECR for image registry
- **Schedule**: EventBridge + Step Functions, daily automated runs

### Data Platform (ETL)
- **Architecture**: Medallion pattern (Raw > Bronze > Silver > Gold)
- **Storage**: S3 (tiered by processing stage)
- **Compute**: 22 Lambda functions (Python, ARM64 for cost optimization)
- **Orchestration**: AWS Step Functions
- **AI/ML**: Amazon Bedrock (Claude) for product classification, category resolution, brand matching
- **Serving**: DynamoDB (single-digit millisecond reads)

### Consumer App
- **Framework**: React 19 + TypeScript
- **Build**: Vite 7
- **Styling**: Tailwind CSS 4
- **Auth**: AWS Cognito + Google OAuth
- **i18n**: react-i18next (DE, EN, FR, IT)
- **Hosting**: S3 + CloudFront (PWA-ready)

### Admin Dashboard
- **Framework**: React 18 + TypeScript
- **Visualization**: Recharts
- **Auth**: AWS Amplify (separate Cognito pool)

### Infrastructure
- **IaC**: AWS CDK (TypeScript)
- **Region**: eu-central-1 (Frankfurt)
- **Monitoring**: CloudWatch, custom health monitor Lambda
- **Cost tracking**: Automated billing notifier

---

## Consumer App Features

**Price Intelligence**
- Cross-store price comparison for 130K+ products
- Price history tracking with trend visualization
- Daily deal aggregation across all 9 retailers
- Basket optimizer (find the cheapest store for your shopping list)
- Hot deals ticker with real-time promotions

**Fuel & Energy**
- 6,000+ fuel station prices (CH + cross-border DE/FR/IT/AT)
- Canton-level price rankings
- Fuel price trend charts
- 17,000+ EV charging station map
- Community-driven price reporting  (CH + cross-border DE/FR/IT/AT)

**Product Detail**
- Nutritional information panels
- Ingredient lists
- Dietary badges (vegan, gluten-free, organic, etc.)
- Allergen information
- Store availability and pricing

**Shopping Tools**
- Shopping lists with cross-store price optimization
- Price alerts (get notified when products drop)
- Category browser (600+ product categories)
- Multi-store search

**Discovery**
- Weekly flyer viewer (digital flyer aggregation)
- AI-powered chat assistant
- Store locator with maps
- Opening hours

**Platform**
- 4 languages (DE, EN, FR, IT)
- PWA (installable, offline-capable)
- Cognito authentication + Google OAuth
- Mobile-first responsive design

---

## Admin Dashboard (Internal Operations)

30 operational pages covering:

- **Pipeline Monitoring**: Job scheduling, execution history, kill switch
- **Data Quality**: AI validation queue, category QC, deduplication tools
- **Master Data**: Brand management, category taxonomy, UOM normalization
- **Business Ops**: Deal management, flyer operations, fuel operations
- **Infrastructure**: Billing dashboard, cost tracking, store audits
- **Communications**: Email templates, share analytics

---

## Why I Built This

For 6 years I had a nagging question: with 9 grocery stores to choose from in Switzerland, where should I actually shop on any given day?

I had the problem defined and the solution designed. What I lacked was the execution bandwidth. AI changed that equation entirely.

Using Claude as my development partner, I went from a solo professional with an idea to a fully mobilized engineering operation — architect, developers, testers, infrastructure, documentation, AI team — with a few keystrokes.

**What AI provided**: Execution bandwidth. The ability to move from concept to production at a pace that would have required a team of 8-10 engineers.

**What AI didn't provide**: Architecture judgment, cost discipline, product sense, quality standards, and the decision-making that comes from 25 years of building and leading complex technology programs in regulated industries.

The combination is what shipped.

---

## Contact

- **Live app**: [skrimpers.com](https://skrimpers.com)
- **LinkedIn**: [Connect with me](https://www.linkedin.com/in/steed-kevin/)

---

*Solo-built with AI-native development practices. Designed, architected, and shipped by one experienced professional with a Mac Studio and Claude.*
