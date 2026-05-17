### 文件檢索系統: AI 旅平險專員，2026.5.17更新

這是一個利用RAG的文件檢索系統，主要在做"AI旅平險專員"的任務

系統能夠讀取並解析旅行綜合保險條款 PDF，根據使用者描述的具體情境（如：班機延誤、行李被竊等），檢索相關條文，並判斷是否符合理賠資格，
具備「主動防坑」的特性，若使用者的情境觸發了不保事項（除外責任），AI 會強烈提醒使用者，打破錯誤期待

特色：

```

1. 法規文本切塊：優先針對法規的「第 X 章」、「第 X 條」進行切塊，確保保險條文的語意完整性，不被隨意截斷。

2. 突破雙欄 PDF 解析限制：採用 PyMuPDF 的 blocks 模式抓取文字區塊，有效解決保單常見的雙欄排版導致閱讀順序混亂的問題

3. 向量資料庫建置：先在 Colab 本地端建置 ChromaDB，再備份至 Google Drive，大幅提升寫入穩定度

4. 系統提示詞：設定系統提示詞，要求 系統 做到「客觀」、「精準引用條款名稱與第幾條」，並具備「主動防坑」警示功能

```

架構：

```

1. 語言模型: llama-3.3-70b-versatile - 提供極速且精準的推論

2. 嵌入模型 (Embedding): HuggingFace (BAAI/bge-m3) - 支援多語言且效能優異的開源 Embedding 模型

3. 向量資料庫 (Vector DB): Chroma

4. 文件解析: PyMuPDF

5. 核心框架: LangChain

```

快速開始：

```

1. 建議使用 Google Colab 執行本專案，

2. 專案所需套件：
!pip install -q langchain langchain-community langchain-chroma langchain-huggingface pypdf pdfplumber sentence-transformers chromadb groq streamlit langchain_groq
!pip install -q torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121  # Colab GPU 版本
!pip install -q pymupdf

3. 設定 Google Drive：程式會自動掛載 Google Drive，請確保你的雲端硬碟中有 /MyDrive/rag_travel_insurance/data/raw_pdfs/ 目錄

4. 設定 API 金鑰：在程式碼中填入你的 Groq API Key

5. 依序執行 Notebook 中的儲存格

```

範例：

```

問題：我的回程班機原本預計下午 2 點起飛，因為颱風延誤到晚上 7 點才起飛。但我因為不想等，在下午 5 點就自行退票，改買另一家航空公司的機票回台灣。請問這樣符合『班機延誤險』的理賠資格嗎？

系統回復：根據第二十七條承保範圍，班機延誤期間的計算，應符合下列情事之一，且若為同一事故時，下列各款得合併計算。然而，在您的案例中，您在下午 5 點自行退票，改買另一家航空公司的機票回台灣...以下略

```
