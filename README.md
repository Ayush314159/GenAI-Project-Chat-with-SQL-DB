# 🦜 Talk2DB: GenAI Chat with SQL Databases

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg?logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.35%2B-FF4B4B.svg?logo=streamlit&logoColor=white)](https://streamlit.io/)
[![LangChain](https://img.shields.io/badge/LangChain-0.2%2B-1C3C3C.svg?logo=chainlink&logoColor=white)](https://www.langchain.com/)
[![Groq](https://img.shields.io/badge/Groq-LPU%20Inference-f55036.svg)](https://groq.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

An enterprise-ready **Generative AI Text-to-SQL Assistant** that enables users to query relational databases (SQLite, MySQL) using natural language. Powered by **LangChain**, **Groq (Llama-3)**, and **Streamlit**, the application translates conversational queries into precise SQL commands, executes them securely, and returns synthesized answers with live agent reasoning.

---

## 📸 Demo Preview

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🦜 LangChain: Chat with SQL DB                                         │
│ ────────────────────────────────────────────────────────────────────── │
│ 👤 User: "Who has the highest marks in the Data Science class?"       │
│                                                                        │
│ 🤖 Assistant:                                                          │
│   ┌─ Agent Reasoning ────────────────────────────────────────────────┐ │
│   │ > Action: sql_db_list_tables                                     │ │
│   │ > Observation: STUDENT                                           │ │
│   │ > Action: sql_db_schema (STUDENT)                                │ │
│   │ > Action: sql_db_query                                           │ │
│   │   SELECT NAME, MARKS FROM STUDENT                                │ │
│   │   WHERE CLASS = 'Data Science' ORDER BY MARKS DESC LIMIT 1;      │ │
│   └──────────────────────────────────────────────────────────────────┘ │
│                                                                        │
│   John has the highest marks in the Data Science class with a score   │
│   of 100.                                                             │
└────────────────────────────────────────────────────────────────────────┘
```

---

## ✨ Key Features

- **⚡ Sub-Second LLM Inference:** Powered by Groq LPUs running Llama-3 for ultra-low latency Text-to-SQL generation.
- **🧠 Autonomous ReAct Agent Loop:** Self-inspects table schemas, checks dialect constraints, detects syntax errors, and self-corrects queries before execution.
- **🗄️ Multi-Database Support:** Out-of-the-box support for embedded **SQLite** databases and cloud/remote **MySQL** servers.
- **👁️ Live Observability & Reasoning:** Interactive `StreamlitCallbackHandler` displays step-by-step agent thoughts, tools used, and generated SQL queries in real time.
- **🔒 Secure Execution:** Configured with read-only connection parameters to safeguard databases against unauthorized modifications.
- **💬 Chat History & Memory:** Session state chat history management with one-click conversation reset.

---

## 🏗️ Architecture & Workflow

```
┌──────────────┐         Natural Language Query        ┌────────────────────────┐
│     User     │ ────────────────────────────────────► │ Streamlit Web Frontend │
└──────────────┘                                        └───────────┬────────────┘
                                                                    │
                                                                    ▼
                                                       ┌────────────────────────┐
                                                       │  LangChain SQL Agent   │
                                                       │   (ReAct Framework)    │
                                                       └─────┬────────────▲─────┘
                                                             │            │
                         Prompt & Schema                     │            │ Generated SQL /
                                                             ▼            │ Reasoning
                                                      ┌─────────────┐     │
                                                      │  Groq LLM   │ ────┘
                                                      │  (Llama-3)  │
                                                      └─────────────┘
                                                             │
                                                             ▼
                                                    ┌─────────────────┐
                                                    │ SQLAlchemy Core │
                                                    └────────┬────────┘
                                                             │
                                          ┌──────────────────┴──────────────────┐
                                          ▼                                     ▼
                                ┌───────────────────┐                 ┌───────────────────┐
                                │  SQLite Database  │                 │  MySQL Database   │
                                │   (student.db)    │                 │   (Remote/Cloud)  │
                                └───────────────────┘                 └───────────────────┘
```

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
| :--- | :--- | :--- |
| **Frontend** | [Streamlit](https://streamlit.io/) | Interactive UI, streaming callbacks, and session state |
| **LLM Orchestration** | [LangChain](https://www.langchain.com/) | Agent toolkit, tool execution, and prompt management |
| **LLM Provider** | [Groq](https://groq.com/) | High-speed LPU inference engine with Llama-3 |
| **Database Engine** | [SQLAlchemy](https://www.sqlalchemy.org/) | Database connection abstraction and dialect handling |
| **Databases** | SQLite 3, MySQL | Relational data stores |
| **Environment** | Python 3.10+ / `python-dotenv` | Runtime and configuration |

---

## 📁 Repository Structure

```
GenAI-Project-Chat-with-SQL-DB/
├── app.py              # Main Streamlit application and agent workflow
├── sqlite.py           # Database seeder script for sample student records
├── student.db          # Sample SQLite database
├── requirements.txt    # Clean, lightweight production dependencies
└── README.md           # Project documentation
```

---

## 🚀 Quickstart Guide

### 1. Clone the Repository
```bash
git clone https://github.com/Ayush314159/GenAI-Project-Chat-with-SQL-DB.git
cd GenAI-Project-Chat-with-SQL-DB
```

### 2. Set Up Virtual Environment
```bash
# Windows
python -m venv .venv
.venv\Scripts\activate

# macOS / Linux
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Initialize the Sample Database
Seed the local `student.db` SQLite database with sample records:
```bash
python sqlite.py
```

### 5. Launch the Streamlit App
```bash
streamlit run app.py
```

---

## 🔑 Configuration & Usage

1. **Obtain Groq API Key:** Get your free API key from the [Groq Console](https://console.groq.com/).
2. **Launch Application:** Open `http://localhost:8501` in your browser.
3. **Connect to Database:**
   - **SQLite:** Select `Use SQLite 3 Database - Student.db` from the sidebar.
   - **MySQL:** Select `Connect to your MySQL Database` and provide Host, User, Password, and Database name.
4. **Input API Key:** Enter your Groq API key in the sidebar.
5. **Start Querying!**

---

## 💡 Example Queries to Try

- *"Show all student records in the database."*
- *"Who are the students enrolled in the Data Science class?"*
- *"What is the average marks of students in the DEVOPS section?"*
- *"Find the student with the highest marks in Section A."*
- *"How many total students scored above 80 marks?"*

---

## 🌐 Deployment

### Deploy to Streamlit Community Cloud (Free)
1. Fork or push this repository to your GitHub account.
2. Visit [share.streamlit.io](https://share.streamlit.io/) and log in with GitHub.
3. Click **New App**, select your repository, set the branch to `main`, and the main file path to `app.py`.
4. Add your secrets in **Advanced Settings > Secrets**:
   ```toml
   GROQ_API_KEY = "gsk_your_groq_api_key_here"
   ```
5. Click **Deploy!**

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve the project:
1. Fork the Project (`gh repo fork Ayush314159/GenAI-Project-Chat-with-SQL-DB`)
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

---

## 📜 License

Distributed under the **MIT License**. See `LICENSE` for more information.

---

## 👨‍💻 Author

**Ayush**
- GitHub: [@Ayush314159](https://github.com/Ayush314159)
- Repository: [GenAI-Project-Chat-with-SQL-DB](https://github.com/Ayush314159/GenAI-Project-Chat-with-SQL-DB)