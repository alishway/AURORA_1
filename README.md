# AURORA Intelligence Platform - Synthetic Datasets

This repository contains comprehensive synthetic datasets designed for the AURORA Intelligence Platform, a law enforcement intelligence system that provides advanced entity resolution, network analysis, and investigative capabilities.

## 📊 Dataset Overview

### Total Records: 12,000+
- **Geographic Focus**: Canada (80%) and USA (20%)
- **Time Range**: 2019-2024
- **Data Type**: Synthetic intelligence-ready datasets
- **Purpose**: Entity resolution, network analysis, and law enforcement scenarios

## 🗂️ Data Sources (16 Files)

### 1. **Social Media & Communication**
- `facebook_data.json` - Facebook profiles, posts, connections
- `twitter_data.json` - Twitter/X profiles, tweets, interactions
- `instagram_data.json` - Instagram profiles, posts, media
- `linkedin_data.json` - LinkedIn professional profiles, connections
- `telegram_data.json` - Telegram messaging metadata
- `whatsapp_data.json` - WhatsApp communication patterns

### 2. **Financial Intelligence**
- `financial_transactions_data.json` - Banking transactions, wire transfers, cryptocurrency
- `enhanced_business_ownership_data.json` - Complex business ownership structures

### 3. **Travel & Movement**
- `travel_border_crossing_data.json` - Airport arrivals, border crossings, maritime movements

### 4. **Directory & Address Data**
- `residential_address_data.json` - Residential addresses, occupancy records
- `business_directory_data.json` - Business registrations, commercial addresses

### 5. **Law Enforcement Intelligence**
- `interpol_data.json` - International criminal databases, red notices
- `additional_intelligence_data.json` - Supplementary intelligence sources

### 6. **Environmental & Context**
- `weather_data.json` - Weather patterns, environmental conditions
- `social_vulnerability_data.json` - Social vulnerability indices

### 7. **System Metadata**
- `data_summary.json` - Dataset overview and statistics

## 🔍 Entity Resolution Capabilities

### WHO-IS-WHO Analysis
- **Entity Matching**: Identify same individuals across multiple data sources
- **Bigram Address Matching**: Advanced address comparison algorithms
- **Phone & Email Correlation**: Link entities through communication identifiers
- **Name Similarity**: Fuzzy matching for name variations and aliases

### WHO-KNOWS-WHO Analysis
- **Relationship Discovery**: Identify connections between entities
- **Communication Patterns**: Analyze messaging and call relationships
- **Financial Relationships**: Track money flows and business connections
- **Geographic Proximity**: Identify co-location patterns

## 🏛️ Law Enforcement Scenarios

### 1. **Money Laundering Investigation**
- Structured deposits under reporting thresholds
- Cross-border wire transfers
- Shell company networks
- Cryptocurrency transactions

### 2. **Organized Crime Networks**
- Criminal enterprise mapping
- Communication pattern analysis
- Asset tracking and ownership
- Associate identification

### 3. **Fugitive Tracking**
- International movement patterns
- Border crossing analysis
- Associate networks
- Social media monitoring

### 4. **Cybercrime Investigation**
- Digital footprint analysis
- Communication metadata
- Financial transaction patterns
- Social network analysis

## 🛠️ Technical Implementation

### Data Processing Pipeline
1. **Extraction**: Parse and normalize data from all sources
2. **Entity Resolution**: Identify and merge duplicate entities
3. **Relationship Discovery**: Find connections between entities
4. **Network Construction**: Build interactive relationship graphs
5. **Risk Assessment**: Calculate entity and network risk scores

### Matching Algorithms
- **Bigram Address Matching**: "99 Apple St" → ["99 Apple", "Apple St"]
- **Phone Number Correlation**: Link entities with shared contacts
- **Email Domain Analysis**: Identify organizational relationships
- **Name Similarity Scoring**: Fuzzy matching for variations

## 🎯 Key Features

- **Real-time Processing**: Handle 12,000+ records efficiently
- **Advanced Analytics**: Network centrality, clustering, pathfinding
- **Risk Scoring**: Multi-factor risk assessment models
- **Interactive Visualization**: Force-directed graphs with risk-based coloring
- **LLM Integration**: AI-powered investigation assistance

## 📈 Expected Results

When processed through the AURORA platform:
- **50+ Unique Entities** resolved from 12,000+ records
- **25+ Relationships** discovered between entities
- **Multiple Risk Levels** (Low, Medium, High, Critical)
- **Interactive Network Graphs** with real-time analytics

## 🚀 Getting Started

### Prerequisites
- Node.js and npm
- React/TypeScript environment
- AURORA Intelligence Platform

### Usage
1. Clone this repository
2. Integrate with AURORA platform
3. Run entity resolution algorithms
4. Explore interactive network visualizations
5. Use AI assistant for investigation support

## 🔒 Security & Privacy

- **Synthetic Data**: All datasets are artificially generated
- **No Real PII**: No actual personal information is included
- **Law Enforcement Training**: Designed for training and development
- **Compliance Ready**: Structured for audit and compliance requirements

## 📄 License

This dataset is provided for law enforcement training and development purposes. Please ensure compliance with local regulations and privacy laws when using in production environments.

## 🤝 Contributing

This is a curated dataset for the AURORA Intelligence Platform. For modifications or additions, please contact the development team.

## 📞 Support

For technical support or questions about the AURORA Intelligence Platform and these datasets, please refer to the main platform documentation or contact the development team.

---

**Generated**: 2024-12-22  
**Version**: 1.0.0  
**Platform**: AURORA Intelligence Platform  
**Purpose**: Law Enforcement Intelligence & Training 