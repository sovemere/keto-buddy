### Step-by-Step Action Plan: Keto-Friendly Food Rating System MVP  
**Timeframe:** 1 Month  
**Tools:** Python, LLMs (e.g., GPT-4, Claude), SQLite, APIs (OpenAI, Reddit/Google Trends), GitHub  
**Key Components:**  
1. Data Collection (Popular Foods)  
2. Keto Analysis Engine (LLM Customization)  
3. Database & Automation Pipeline  
4. Basic User Interface (CLI or Web)  

---

### **Phase 1: Setup & Research (Days 1-3)**  
**Goal:** Define scope, gather resources, and validate feasibility.  
1. **Research Keto Guidelines**  
   - Compile rules from trusted sources (e.g., [DietDoctor](https://www.dietdoctor.com), [Ruled.Me](https://www.ruled.me)).  
   - Example criteria: Net carbs ≤5g/100g, high fat, no added sugars.  
   - *Output:* `keto_guidelines.md` with thresholds and sources.  

2. **Set Up Development Environment**  
   - Install Python, VS Code, SQLite.  
   - Create a GitHub repo: `keto-food-ratings`.  
   - *Learning:* Basic Git commands (`git clone`, `commit`, `push`).  

3. **Plan LLM Workflow**  
   - Decide between API (OpenAI) vs. open-source models (Llama 3).  
   - *MVP Recommendation:* Use OpenAI API for speed; budget ~$50 for API costs.  

---

### **Phase 2: Data Collection (Days 4-7)**  
**Goal:** Automatically identify popular food items to analyze.  
1. **Option 1: LLM-Generated List**  
   - Prompt: *“Generate 200 trending food items in 2024, including snacks, meals, and ingredients.”*  
   - Save output as `food_list.csv`.  

2. **Option 2: API-Based Scraping**  
   - Use Reddit API (r/keto, r/food) or Google Trends.  
   - *Learning:* Python `requests` library, API authentication.  
   - *Code Snippet:*  
     ```python  
     import requests  
     response = requests.get("https://api.reddit.com/r/keto/top?limit=100", headers={"User-Agent": "keto-bot"})  
     ```  

3. **Merge & Clean Data**  
   - Remove duplicates, standardize names (e.g., “avocado” vs. “avocados”).  

---

### **Phase 3: Keto Analysis Engine (Days 8-14)**  
**Goal:** Build an LLM prompt that rates foods on a keto scale (1-10) with explanations.  
1. **Design the Prompt**  
   - Example:  
     ```  
     Analyze if [food] is keto-friendly. Consider net carbs, ingredients, and processing.  
     Output JSON: {"rating": 1-10, "description": "...", "sources": ["url1", "url2"]}.  
     ```  
   - Test with 10 foods manually via ChatGPT/Claude.  

2. **Automate with Python**  
   - Use OpenAI API to process `food_list.csv`.  
   - *Code Snippet:*  
     ```python  
     import openai  
     openai.api_key = "YOUR_KEY"  
     response = openai.ChatCompletion.create(  
         model="gpt-4",  
         messages=[{"role": "user", "content": f"Keto analysis prompt: {food_name}"}]  
     )  
     ```  

3. **Handle Output**  
   - Parse JSON responses; flag errors (e.g., invalid formats).  
   - Save raw responses in `responses/` folder.  

---

### **Phase 4: Database & Automation (Days 15-20)**  
**Goal:** Store results and build a repeatable pipeline.  
1. **Create SQLite Database**  
   - Schema:  
     ```sql  
     CREATE TABLE foods (  
         id INTEGER PRIMARY KEY,  
         name TEXT,  
         rating INTEGER,  
         description TEXT,  
         sources TEXT,  
         last_updated DATE  
     );  
     ```  
   - *Learning:* SQL basics (`INSERT`, `SELECT`).  

2. **Build Python Pipeline**  
   - Scripts:  
     1. `scraper.py` – Collect food names.  
     2. `analyzer.py` – LLM analysis.  
     3. `database.py` – Store results.  
   - Use Python’s `sqlite3` library.  

3. **Add Error Handling**  
   - Retry failed API calls; log errors to `errors.log`.  

---

### **Phase 5: Testing & Refinement (Days 21-25)**  
**Goal:** Ensure accuracy and reliability.  
1. **Validate LLM Outputs**  
   - Manually check 20% of entries against keto guidelines.  
   - Adjust prompts if ratings are inconsistent (e.g., “Cauliflower: 10/10 ✅”, “Quinoa: 2/10 ✅”).  

2. **Optimize Costs**  
   - Cache responses to avoid reprocessing the same food.  

3. **Performance Testing**  
   - Benchmark how many foods/minute the pipeline processes.  

---

### **Phase 6: Basic UI & Documentation (Days 26-30)**  
**Goal:** Enable users to query the database.  
1. **Command-Line Interface (CLI)**  
   - *Code Snippet:*  
     ```python  
     food_name = input("Enter food: ")  
     cursor.execute("SELECT * FROM foods WHERE name=?", (food_name,))  
     print(cursor.fetchone())  
     ```  

2. **Documentation**  
   - Write `README.md` with setup instructions and example outputs.  

3. **Future-Proofing**  
   - Structure code for scalability (e.g., swapping SQLite for PostgreSQL later).  

---

### **Additional Considerations**  
- **Legal:** Avoid scraping restricted websites; use public APIs/synthetic data.  
- **Costs:** OpenAI API (~$0.06/request for GPT-4; ~$12 for 200 foods).  
- **Next Steps:**  
  - Web interface (Flask/Django).  
  - User submissions/voting.  
  - Mobile app integration.  

---

### **How to Use This Plan**  
1. Save this as `action_plan.md` in your project folder.  
2. For each phase, ask follow-ups like:  
   - *“How to design a better prompt for accurate ratings?”*  
   - *“Show me a Python script for batch API calls.”*  
3. Refer to the `Learning` sections for gaps in knowledge.  

Let’s start with **Phase 1**. What would you like to prioritize first?