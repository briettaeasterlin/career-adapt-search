Career Adapt Search
A Learning & Demo Project Using MongoDB Atlas Search + O*NET for Personalized Career Pathways

Built by Brietta Easterlin — 2025

🌟 Overview

Career Adapt Search is a hands-on demo and learning project exploring:

MongoDB Atlas

Atlas Search (text search)

Atlas Vector Search (future enhancement)

O*NET skills & occupation data ingestion

Prototype career recommendation logic

This project supports both:

🎓 My UW AI Product Management Capstone (Career Adapt)

…by providing a real backend for analyzing skill gaps and recommending new career paths.

💼 My interview preparation for MongoDB Search PM

…by demonstrating practical experience with:

Search indexes

Developer workflows

Data modeling

API design patterns

Activation & DX improvements

End-to-end product thinking

🧠 What This Project Does

This repo contains:

1. Seeded Job & Profile Data

Located at:

seed/jobs.json
seed/courses.json
seed/profiles.json


Ingested into MongoDB Atlas using:

npm run seed

2. O*NET Occupation & Skills Dataset (sample + expandable)

A small subset of O*NET was ingested from:

data/onet/onet_sample.json


Ingested using:

npm run ingest:onet


This creates a collection:

career_adaptø.onet_occupations

3. Atlas Search Indexes

Indexes used:

jobs_search → on jobs collection

onet_search → on onet_occupations collection

Configured using Atlas UI (Dynamic Index for ease of iteration).

4. Search Pipelines

Includes demo pipelines showing:

Text search

Autocomplete

Skills search across O*NET

Career pathway matching

Example (run in mongosh):

db.onet_occupations.aggregate([
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

🧱 Architecture
┌──────────────────────────┐
│     Lovable Prototype    │  (UI — optional future integration)
└──────────────▲───────────┘
               │ fetch /api/search
               │
┌──────────────┴──────────────┐
│       Node.js Scripts        │
│  (queries, ingestion, demo)  │
└──────────────▲──────────────┘
               │ MongoDB driver
               │
┌──────────────┴──────────────────────────┐
│           MongoDB Atlas Cluster         │
│    career_adaptø.jobs                   │
│    career_adaptø.profiles               │
│    career_adaptø.onet_occupations       │
│─────────────────────────────────────────│
│        Atlas Search Indexes             │
│      - jobs_search                      │
│      - onet_search                      │
└─────────────────────────────────────────┘


This mirrors the real developer flow MongoDB PMs manage:

Load data

Create search index

Query

Iterate

Integrate into code

🚀 Local Developer Workflow
1. Clone the repo
git clone https://github.com/briettaeasterlin/career-adapt-search.git
cd career-adapt-search

2. Install dependencies
npm install

3. Add your credentials

Create a .env file:

ATLAS_URI=<your connection string>
DB_NAME=career_adaptø

4. Seed Jobs + Profiles
npm run seed

5. Ingest O*NET sample
npm run ingest:onet

6. Run example search
npm run onet:search

🔍 Example Search Output
🔎 O*NET search results:
• Software Developers — score 7.92
• Data Scientists — score 7.11

🧩 Future Extensions
🔹 Hybrid “Career Match” Pipeline

Match user skills → O*NET → jobs:

skills = ["critical thinking", "programming"]


Pipeline returns:

Closest occupations

Suggested learning paths

Job roles aligned to those occupations

🔹 Atlas Vector Search

Use embeddings for:

Resume semantic search

Course alignment

Transferable skills discovery

🔹 UI Integration (Lovable or Next.js)

Simple API route:

GET /api/search?q=product manager


Returns results from Atlas Search.

🧠 PM Reflections (DX, PLG, Developer Empathy)

Working through this project highlighted several Developer Experience moments:

IP Access Lists can confuse new users

.env setup is friction-heavy

Atlas Search onboarding assumes familiarity with index naming

Switching between career_adapt and career_adaptø creates unexpected failure modes

Dynamic Index is great for exploration, but discoverability is low

Developers need “sample pipelines” templates in-product

Very strong opportunity for guided setup flows for Search indexes

I’ll discuss these insights in my interview to highlight:

developer empathy

understanding of onboarding challenges

PLG activation barriers

opportunities for experimentation

🏁 Summary

This repo demonstrates:

✔ Atlas Search fundamentals
✔ Text + autocomplete search
✔ Practical ingestion flows
✔ Career pathway modeling using O*NET
✔ Product thinking & developer empathy
✔ Search-centric architecture ready for UI integration

It's both:

A meaningful backend for my Career Adapt AI capstone, and 
A practical demonstration of my ability to understand and build with MongoDB Search
