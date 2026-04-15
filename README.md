# Nutritional Insights — Diet Analysis Cloud Dashboard

**Live App:** https://witty-pond-0e08a300f.6.azurestaticapps.net/

A cloud-based data analytics dashboard that processes dietary datasets and presents them as interactive insights.

This project focuses on building a simple but realistic data pipeline in the cloud. Raw data is ingested from storage, processed using Python, and exposed through APIs to support visualization and analysis.

The goal is to demonstrate how data flows through a system — from ingestion to transformation to serving — while keeping performance and maintainability in mind.

---

## Table of Contents

- Overview  
- Architecture  
- Design Decisions  
- Data Pipeline  
- Features  
- Backend  
- Frontend  
- Environment Variables  
- Deployment  
- API Reference  
- Performance  

---

## Overview

This project demonstrates an end-to-end workflow for processing and serving analytical data:

- Load raw CSV data from cloud storage  
- Clean and transform data using pandas  
- Precompute aggregated results  
- Store processed output as JSON  
- Serve data via API endpoints  
- Display insights through a web dashboard  

---

## Architecture

The system follows a cloud-native, event-driven design:

Raw Data (Blob Storage)  
→ Data Processing (Azure Function)  
→ Cached Results (JSON in Blob Storage)  
→ API Layer (Azure Functions)  
→ Frontend Dashboard (React)

| Layer     | Technology                   | Responsibility                    |
|----------|------------------------------|----------------------------------|
| Storage  | Azure Blob Storage           | Store raw and processed data     |
| Compute  | Azure Functions (Python)     | Data processing and API          |
| API      | Azure Functions (HTTP)       | Serve processed data             |
| Frontend | React + TypeScript           | Visualization layer              |
| Hosting  | Azure Static Web Apps        | Frontend deployment              |
| Auth     | Supabase                     | User authentication              |

---

## Design Decisions

### Cache-first approach

Instead of processing the dataset on every request, results are precomputed and stored as JSON.

This reduces repeated computation and significantly improves API response time.

### Event-driven processing

A Blob trigger is used to detect changes in the dataset.

Data is processed only when the source file changes, avoiding unnecessary execution.

### Separation of concerns

- Data processing logic is isolated in backend functions  
- API layer serves only processed data  
- Frontend focuses on visualization  

This makes the system easier to maintain and extend.

### Serverless architecture

Azure Functions are used to handle both processing and API layers.

This removes the need to manage infrastructure and allows the system to scale automatically.

---

## Data Pipeline

The pipeline follows a simple ETL flow:

### Extract

- Read raw CSV data from Blob Storage  

### Transform

- Clean data (handle missing values, normalize fields)  
- Aggregate data by diet type  
- Compute metrics (protein, carbs, fat)  

### Load

- Store processed results as JSON in Blob Storage  
- API reads from cached JSON instead of recomputing  

---

## Features

### Data Processing

- Cleans and transforms raw datasets using pandas  
- Aggregates data for analysis  
- Handles inconsistent or missing data  

### Analytics

- Displays diet distribution  
- Shows nutritional breakdown  
- Supports comparison between diet types  

### API

- RESTful endpoints for accessing processed data  
- Supports filtering, search, and pagination  

### Frontend

- Interactive dashboard built with React  
- Data visualization using charts  
- Responsive UI with filtering options  

### Authentication

- Supabase authentication  
- Restricts access to authorized users  

---

## Backend (Azure Functions)

**Stack:** Python, Azure Functions, pandas  

Responsibilities:

- ETL processing  
- Data aggregation  
- API endpoints  

Blob storage structure:

- Input: `datasets/All_Diets.csv`
- Output:
  - `cleaned/cleaned_diets.csv`
  - `cache/dashboard_summary.json`

Run locally:

```bash
cd backend
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
func start
Frontend

Stack:

React
TypeScript
Vite
Chart.js

Run locally:

cd frontend
npm install
npm run dev
Environment Variables
VITE_API_BASE_URL=http://localhost:7071/api
VITE_SUPABASE_URL=your_project_url
VITE_SUPABASE_ANON_KEY=your_key
Deployment

Frontend:

Azure Static Web Apps (GitHub Actions)

Backend:

func azure functionapp publish <FUNCTION_APP_NAME>
API Reference

Base URL:
https://diet-chart-dashboard-318.azurewebsites.net/api

Method	Endpoint	Description
GET	/dashboard-summary	Returns aggregated data
GET	/recipes	Supports filtering and paging
Performance
Precomputed cache reduces repeated computation
Event-driven processing avoids unnecessary execution
API response is significantly faster than on-demand processing
License

Built as part of a cloud computing project using Azure serverless technologies.