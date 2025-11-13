# Career Adapt Search
A Learning & Demo Project Using MongoDB Atlas Search + O*NET for Personalized Career Pathways
Built by Brietta Easterlin — 2025

## 🌟 Overview

Career Adapt Search is a hands-on demo and learning project exploring MongoDB Atlas, Atlas Search (text search), O*NET skills data, and prototype career recommendation logic. It supports both my UW AI Product Management capstone (Career Adapt) and my interview preparation for MongoDB Search PM by demonstrating practical experience with indexes, developer workflows, data modeling, and product thinking.

## Why MongoDB Atlas Search?

Career Adapt needs flexible, fast search across messy, varied data: job postings, user skills, learning content, and O*NET’s hierarchical skill taxonomy. MongoDB Atlas Search is a strong fit because it provides:

- Native full-text search without running Elasticsearch
- Dynamic schemas that match the shape of real-world job data
- Text, autocomplete, and vector search in one system
- A single query pipeline for blending multiple data sources
- A fast developer feedback loop while experimenting

This lets the project move quickly from prototype to real, personalized recommendations.

## Quick Start

```bash
git clone https://github.com/briettaeasterlin/career-adapt-search.git
cd career-adapt-search
npm install
cp .env.example .env   # create env file
# then add your ATLAS_URI
npm run seed           # seed jobs, courses, profiles
npm run ingest:onet    # load O*NET sample
npm run onet:search    # test search
```

### 🧠 What This Project Does

This repo includes:

Seeded Job & Profile Data
(seed/jobs.json, seed/courses.json, seed/profiles.json)
Loaded via: npm run seed

O*NET Occupation & Skills Dataset (sample set)
(data/onet/onet_sample.json)
Loaded via: npm run ingest:onet

Collections Created
- career_adaptø.jobs
- career_adaptø.courses
- career_adaptø.profiles
- career_adaptø.onet_occupations

Atlas Search Indexes
- jobs_search on jobs
- onet_search on onet_occupations (Created using Dynamic Index templates in Atlas)

Search Examples
Pipelines demonstrating text search, skill matching, and occupation lookup.

Example search (run in mongosh):

```db.onet_occupations.aggregate([
  {
    $search: {
      index: "onet_search",
      text: {
        query: "programming",
        path: ["skills.name", "tasks.description", "title", "altTitles"]
      }
    }
  },
  { $limit: 5 },
  { $project: { title: 1, score: { $meta: "searchScore" } } }
]).pretty()
```

### 🧱 Architecture
 Lovable Prototype (UI - future)
           ▲
           │
     /api/search (future)
           │
 Node.js scripts (queries, ingestion)
           ▲
           │
 MongoDB Atlas Cluster
   - jobs
   - courses
   - profiles
   - onet_occupations
   - Atlas Search Indexes

This mirrors a realistic developer workflow: load data → configure index → query → iterate → integrate.

### 🚀 Local Developer Workflow
1. Clone the repo
```git clone https://github.com/briettaeasterlin/career-adapt-search.git
cd career-adapt-search
```
2. Install dependencies
```
npm install
3. Add credentials (.env)
ATLAS_URI=<your connection string>
DB_NAME=career_adaptø
```
4. Seed core data
```npm run seed
5. Ingest O*NET sample
npm run ingest:onet=
```
6. Run a sample search
```
npm run onet:search
```

🔍 Example Search Output
🔎 O*NET search results:
• Software Developers — score 7.92
• Data Scientists — score 7.11

### 🧩 Future Extensions

Hybrid Career Match Pipeline
Match user skills → O*NET → jobs.

Atlas Vector Search Integration
Resume embeddings, semantic matching, transferable skills scoring.

UI Integration (Lovable or Next.js)
Add a simple REST endpoint:
```
GET /api/search?q=product manager
```

Course Recommendations
Suggest upskilling paths based on skill gaps.

### 🧠 PM Reflections (DX, PLG, Developer Empathy)

Working through this project revealed several onboarding and DX pain points:
- IP Access List setup creates early friction.
- .env and connection string formatting can break silently.
- Index naming and selection are not intuitive for beginners.
- Switching between databases (career_adapt vs career_adaptø) can cause unexpected failures.
- Dynamic Index templates are powerful but hidden.
- Developers would benefit from built-in sample pipelines.

## 🏁 Summary

Career Adapt Search demonstrates Atlas Search fundamentals, data ingestion, O*NET integration, and a clear path toward personalized career recommendations. It strengthens both my capstone project and my interview preparation by showcasing practical search tooling, developer workflows, and PM-level insight into improving the developer experience.
