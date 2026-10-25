# MF-portfolio-analyser
AI mutual fund portfolio reviewer built with LangGraph. It reads an Indian CAS statement, computes returns, risk, overlap and tax in code, then Claude writes a fund-by-fund review and a least-cost action plan. A rules-based critic checks each step against tax and exit-load costs. Personal data is stripped and amounts masked before any cloud call.


The problem
Reviewing a mutual fund portfolio properly takes a lot of work. You need returns against the right benchmark, risk-adjusted performance, peer rankings, overlap between funds, tax on each purchase lot and exit loads. Only then can you decide what to switch and in what order. Most investors skip it. Advisers charge for it. A general chatbot makes up numbers and would need the investor's full statement.

I built a tool that follows a written review framework, the way I would review a portfolio myself, and keeps the investor's data private.

<img width="572" height="289" alt="image" src="https://github.com/user-attachments/assets/86f7efcd-ccc1-409e-8b40-c47aeaf24886" />


<img width="501" height="404" alt="image" src="https://github.com/user-attachments/assets/94852f14-72f0-44c2-a582-87024a8fbbd7" />

<img width="502" height="295" alt="image" src="https://github.com/user-attachments/assets/02ed2400-2ace-49ed-905d-e11c663668af" />





What changed in v2
1.Benchmark data now loads from niftyindices, so information ratio, R², rolling win-rates, closet-indexer flags and the market valuation view appear in the report.
2.Benchmark per fund from its SEBI category (for example small cap → Nifty Smallcap 250) instead of one default index for every fund.
3.Secrets out of the code: CAS path, password and K are read from .env, and the run stops if the password is missing. More AMC websites were added to the config.


Known limitations and what's next
I keep these visible on purpose; they are the next iteration's backlog.

1.Masking gap: when the critic sends a draft back, its break-even notes quote real rupee amounts. Next: send them as scaled tags or percentages.
2.Benchmark resolver: it runs before AMFI data is fetched, so AMFI's reported benchmark is not used yet, and a few keyword rules map to the wrong index (for example "Nasdaq 100").
3.Data matching: expense ratios still come back missing, some AMFI returns match the wrong scheme by name, and peer quartiles are empty. Next: match on scheme code / ISIN.
4.Macro data: CPI and FII/DII fetchers are written but not yet wired in.
5.Evaluation: build a labelled set of CAS files and track XIRR against the registrar's figure, missing-data rate, critic pass rate and re-drafts per run.



How to Run this?
1.Install Ollama and pull both models: ollama pull qwen2.5:3b and ollama pull qwen2.5:7b.
pip install casparser amfipy langgraph claude-agent-sdk ollama exa-py pdfplumber pandas numpy scipy requests python-dotenv beautifulsoup4 lxml openpyxl
2.Sign the Claude Agent SDK's bundled CLI into your Claude account and create a long-lived token (claude setup-token).
3.Create a .env file next to the notebook. It is git-ignored and never committed:
4.CAS_PDF_PATH=path/to/your_cas.pdf
CAS_PASSWORD=your_cas_password
PRIVACY_K=choose_any_number
CLAUDE_CODE_OAUTH_TOKEN=your_token
EXA_API_KEY=your_exa_key        # only needed if ENABLE_AMC_DOCS = True
5.Fill in the PROFILE block (age, horizon, risk tolerance) in the config cell, then run the notebook top to bottom.
