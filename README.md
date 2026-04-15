# Nutritional Insights — Diet Analysis Cloud Dashboard

**Live app:** https://witty-pond-0e08a300f.6.azurestaticapps.net/

This project is a cloud-based data analytics dashboard designed to process raw dietary datasets and transform them into meaningful insights.

The system simulates a real-world data pipeline where raw data is ingested, cleaned, aggregated, and served through APIs to support user decision-making (e.g., understanding diet patterns and nutritional distribution).

Built using Azure serverless technologies, the platform focuses on scalability, performance optimization, and efficient data processing.

---

## Table of contents

- Architecture
- Features
- Repository layout
- Backend (Azure Functions)
- Frontend (React + Vite)
- Environment variables
- Supabase configuration
- Deployment
- Local development
- API reference
- Troubleshooting
- License

---

## Architecture

This system follows a cloud-native data pipeline architecture:

Raw Data (Blob Storage)  
→ Data Processing (Azure Functions)  
→ Cached Results (JSON in Blob Storage)  
→ API Layer (HTTP endpoints)  
→ Frontend Dashboard (React)

| Layer | Service | Role |
|--------|---------|------|
| UI | Azure Static Web Apps | Hosts the React SPA with CI/CD via GitHub Actions |
| API | Azure Functions (Python) | REST endpoints and blob-triggered processing |
| Data | Azure Blob Storage | Stores raw CSV, cleaned data, and cached results |
| Auth | Supabase | Email/password and OAuth authentication |

### Design Considerations

- Azure Functions are used for serverless execution to support scalability and reduce infrastructure management
- Blob-triggered processing ensures data is recomputed only when the dataset changes
- Caching results as JSON avoids repeated heavy computations and improves response time
- Separation of frontend and backend improves maintainability and flexibility

---

## Features

### Data Processing & Insights

- Processes raw CSV datasets using Python (pandas) for data cleaning and aggregation
- Extracts structured insights such as diet distribution and nutritional metrics
- Enables users to explore dietary patterns and make informed decisions through visualization

### Performance Optimization

- Implements caching using precomputed JSON stored in Blob Storage
- Reduces redundant computation and improves API response performance
- Uses blob-triggered updates so processing only occurs when source data changes

### Cloud Architecture

- Built on Azure serverless infrastructure for scalable and event-driven processing
- Uses Blob Storage for raw data, processed outputs, and cached results
- Frontend deployed via Azure Static Web Apps with CI/CD pipeline

### Data Interaction

- Supports filtering by diet type
- Provides keyword search (recipe name / cuisine)
- Implements paginated API endpoints for efficient data retrieval

### Authentication & Security

- Uses Supabase Auth (email + OAuth)
- Restricts dashboard access to authenticated users
- Sensitive data (passwords) handled securely by the auth provider

---

## Repository layout
