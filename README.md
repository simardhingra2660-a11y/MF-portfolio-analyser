# MF-portfolio-analyser
AI mutual fund portfolio reviewer built with LangGraph. It reads an Indian CAS statement, computes returns, risk, overlap and tax in code, then Claude writes a fund-by-fund review and a least-cost action plan. A rules-based critic checks each step against tax and exit-load costs. Personal data is stripped and amounts masked before any cloud call.
