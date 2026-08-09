# 🚀 Projects Deep Dive

Detailed breakdown of each project — built for interview preparation and portfolio clarity.

--- 

## 🤖 1. Agentic RAG AI Research Assistant

**Repo:** [agentic-rag-ai-research-assistant](https://github.com/adheethii/agentic-rag-ai-research-assistant)

### What Problem Does it Solve?
Most RAG systems rely on expensive paid APIs (OpenAI) and send your data to the cloud. This system runs **100% locally** — no API costs, no data privacy concerns — while still providing document Q&A, summarization, and web search.

### Tech Stack & Why

| Technology | Why I Chose It |
|------------|----------------|
| LangChain | Best framework for building LLM pipelines and agents |
| Ollama | Runs LLMs locally — free, private, no API key needed |
| FAISS | Fast similarity search — handles thousands of document chunks efficiently |
| Streamlit | Fastest way to build a clean interactive UI for ML apps |

### How it Works
```
User uploads PDF
    ↓
Documents chunked (500 tokens, 50 overlap)
    ↓
Chunks embedded → stored in FAISS index
    ↓
User asks question
    ↓
LangChain agent decides: retriever / summarizer / web search
    ↓
Retrieved context → injected into prompt → Ollama generates answer
    ↓
Response displayed in Streamlit
```

### Key Technical Decisions
- **Hybrid retrieval** — FAISS vector search + BM25 keyword search combined for better accuracy
- **Local LLM** — Ollama with llama3 avoids API costs and keeps data private
- **Agentic design** — LLM decides which tool to use rather than hardcoded flow

### Challenges Faced
- Chunk size tuning — too large gave irrelevant context, too small lost meaning
- Hallucination prevention — solved with strict grounding prompts
- Slow first load — solved by persisting FAISS index to disk

### What I'd Improve
- Add evaluation metrics (compare vector-only vs hybrid retrieval accuracy)
- Add a demo GIF showing the app in action
- Deploy with Docker so anyone can run it in one command

### Interview Talking Points
- "Why local LLMs?" → Privacy, cost, offline capability
- "Why FAISS?" → Fast nearest-neighbor search at scale
- "What is hybrid retrieval?" → Dense (semantic) + sparse (keyword) combined
- "Is it truly agentic?" → Agent dynamically selects tools based on query intent

---

## 🩺 2. MediCore AI Hospital System

**Repo:** [Medicore-AI-Hospital-System](https://github.com/adheethii/Medicore-AI-Hospital-System)

### What Problem Does it Solve?
Hospital reception is slow and error-prone — patients fill forms repeatedly, staff manually search records. MediCore automates patient identification using face recognition and auto-fills visit details — reducing duplicate entries by ~30%.

### Tech Stack & Why

| Technology | Why I Chose It |
|------------|----------------|
| face_recognition | Simple Python API wrapping dlib's deep learning model |
| OpenCV | Webcam access and real-time frame processing |
| Streamlit | Multi-page app with clean hospital-themed UI |
| Pandas + CSV | Lightweight storage — no database setup needed for prototype |

### How it Works
```
New Patient:
Fill registration form → Capture face via webcam
    ↓
face_recognition extracts 128-dim encoding
    ↓
Encoding + patient data saved to patients.csv + patient_faces/

Existing Patient:
Webcam captures face → encoding extracted
    ↓
Compare against all stored encodings (tolerance 0.6)
    ↓
Match found → auto-fill patient details → log visit
    ↓
Admin dashboard updates with new visit stats
```

### Key Technical Decisions
- **Tolerance 0.6** — balanced between false positives and false negatives
- **CSV storage** — simpler than a database for a prototype, easy to inspect
- **Admin password protection** — basic security for sensitive analytics

### Challenges Faced
- Lighting variations affected recognition accuracy — solved by preprocessing frames
- BGR vs RGB confusion — OpenCV uses BGR, face_recognition needs RGB
- Slow encoding on first run — solved by saving encodings to pickle files

### What I'd Improve
- Replace CSV with SQLite or PostgreSQL for better scalability
- Add SMS notification on patient arrival
- Deploy with Docker for easy hospital IT setup
- Add liveness detection to prevent photo spoofing

### Interview Talking Points
- "How does face recognition work?" → 128-dim embeddings, cosine distance
- "Why 0.6 tolerance?" → Trade-off between precision and recall
- "How would you scale this?" → Move to database, add load balancing
- "What's the biggest limitation?" → No liveness detection — a photo could fool it

---

## 🚗 3. Road Accident Analysis Dashboard

**Repo:** [Road-Accident-Analysis-Dashboard](https://github.com/adheethii/Road-Accident-Analysis-Dashboard)

### What Problem Does it Solve?
Road safety authorities need to understand accident patterns to allocate resources and design interventions. This dashboard transforms 195K+ raw accident records into actionable insights — showing where, when, and why accidents happen.

### Tech Stack & Why

| Technology | Why I Chose It |
|------------|----------------|
| Power BI | Industry-standard BI tool, rich interactive visuals |
| DAX | Powerful formula language for KPIs and time intelligence |
| Power Query | Data cleaning and transformation without code |

### Key Insights Found
- Cars account for **155,804 casualties** — majority of all vehicle-related accidents
- **Single carriageway roads** most dangerous — 145K casualties
- **Daytime accidents** dominate at **73.84%** — high traffic hours are key risk
- Fatal casualties dropped **33.3% year-on-year** — safety measures working

### Key Technical Decisions
- **Star schema** — Calendar dimension table connected to fact table enables time intelligence
- **YoY DAX measures** — TOTALYTD + SAMEPERIODLASTYEAR for KPI comparisons
- **Drill-down filters** — Road surface + weather slicers for interactive analysis

### What I'd Improve
- Add predictive model (which roads will have accidents next month?)
- Connect to live data feed for real-time monitoring
- Add geographic clustering to identify hotspot zones more precisely

### Interview Talking Points
- "What is DAX?" → Formula language for Power BI — like Excel formulas but for data models
- "What is a star schema?" → Fact table + dimension tables — enables fast queries
- "What insights did you find?" → Always have 2-3 specific numbers ready

---

## 📉 4. Telecom Customer Churn Prediction

**Repo:** [Telecom-customer-churn](https://github.com/adheethii/Telecom-customer-churn)

### What Problem Does it Solve?
Telecom companies lose significant revenue when customers switch providers. Predicting which customers are likely to churn allows proactive retention — targeted offers before they leave.

### Tech Stack & Why

| Technology | Why I Chose It |
|------------|----------------|
| Scikit-Learn | Best ML library for classical algorithms |
| SMOTE | Handles class imbalance — churn is rare event |
| Pandas | Data manipulation and feature engineering |
| Streamlit | Interactive prediction interface |

### How it Works
```
Raw customer data (tenure, charges, contract type)
    ↓
EDA → identify patterns (long-tenure customers churn less)
    ↓
Feature engineering → encode categoricals, scale numerics
    ↓
SMOTE → balance classes (churn = minority class)
    ↓
Train Random Forest → tune hyperparameters
    ↓
Evaluate with F1 + ROC-AUC (not accuracy — data is imbalanced)
    ↓
Streamlit app → input customer details → get churn probability
```

### Key Technical Decisions
- **SMOTE** — churn rate ~15%, so accuracy is misleading. SMOTE balances training data
- **F1 + ROC-AUC** — better metrics than accuracy for imbalanced classification
- **Random Forest** — captures non-linear relationships better than logistic regression

### Challenges Faced
- Class imbalance — 85% non-churn vs 15% churn → solved with SMOTE
- Feature encoding — multiple categorical columns → label encoding + one-hot encoding
- Overfitting — solved with cross-validation and hyperparameter tuning

### What I'd Improve
- Deploy as a FastAPI + Docker app on Render (actively working on this)
- Add SHAP values for model explainability
- Build a monitoring dashboard to track model drift over time

### Interview Talking Points
- "Why not use accuracy?" → Dataset is imbalanced — 85% accuracy by predicting "no churn" every time
- "Why SMOTE?" → Creates synthetic minority samples rather than just duplicating
- "Why F1 over precision/recall alone?" → Balanced metric when both matter
- "How would you deploy this?" → FastAPI → Docker → Render/AWS

---

*Updated: July 2026*
