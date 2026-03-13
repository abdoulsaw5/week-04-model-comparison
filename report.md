# Model Comparison Report  Week 4
**Name:** Abdoul Sawadogo
**Date:** 3/12/2026
**Capstone Project:** Cybersecurity Alert Analysis Pipeline
**My Component:** Anomaly Detection Workflow
## Test Setup
**Input dataset:** 5 cybersecurity text samples covering:
- 2 clearly concerning/high-severity records (Records 3 and 4)
- 1 ambiguous/edge case record
- 2 routine/benign records
**Models tested:**
1. distilbert-base-uncased-finetuned-sst-2-english (sentiment)
2. facebook/bart-large-mnli (zero-shot classification)
3. dslim/bert-large-NER (named entity recognition)
4. Groq Llama 3 8B (LLM classification)
**Evaluation criteria:** label accuracy, confidence score, speed, ease of
integration in n8n
## Results Summary

| Record | Input Summary | Sentiment | Zero-Shot | NER Entities | Groq |
|--------|--------------|-----------|-----------|--------------|------|
| 1 | Unauthorized login from IP 198.51.100.4 at 3:47 AM  multiple failed attempts | POSITIVE (0.7481) | possible anomaly (0.9033) | None | MEDIUM — potential brute-force attempt, not a confirmed breach, requires investigation |
| 2 | Routine firewall rule update on fw-01 during scheduled maintenance | POSITIVE (0.7481) | routine activity (0.9961) | None | INFORMATIONAL  routine expected maintenance, no security issue |
| 3 | Phishing email targeting finance@company.com  spoofed sender domain | POSITIVE (0.7481) | possible anomaly (0.7573) | None | HIGH  phishing targeting finance email warrants immediate investigation |
| 4 | Multiple failed SSH authentication attempts  47 attempts in 5 minutes | POSITIVE (0.7481) | possible anomaly (0.8701) | MISC: "SS" (score: 0.994) | HIGH  potential brute-force attack on SSH server, possible unauthorized access attempt |
| 5 | System resource utilization normal across all hosts — no anomalies detected | POSITIVE (0.7481) | routine activity (0.9668) | None | INFORMATIONAL  normal healthy system utilization, no action required |
## Analysis
**Where models agreed:** Records 2 and 5 showed full agreement across all models
**Where models disagreed:** The Sentiment model disagreed with every other model on every record. It returned POSITIVE with the same score (0.7481) for all 5 records
**Most accurate model overall:** Groq Llama 3 8B provided the most accurate and actionable results. It correctly escalated Records 3 and 4 to HIGH, appropriately classified Record 1 as MEDIUM (recognizing it as suspicious but unconfirmed), and correctly labeled Records 2 and 5 as INFORMATIONAL. Critically, it also provided a one-sentence reasoning for each classification, making it directly usable for analyst review.
**Fastest/most practical:** facebook/bart-large-mnli (Zero-Shot) is the most practical at scale. It produces structured label + confidence score outputs, integrates cleanly into n8n field mappings, requires no prompt engineering
week-04-lab-instructions.md 2026-03-09
10 / 15
## Recommended Models for My Capstone Component
**Component:** Anomaly Detection Workflow
**Primary model:** Groq Llama 3 8B — its CRITICAL/HIGH/MEDIUM/LOW/INFORMATIONAL classification with human-readable reasoning makes it directly usable for alert triage, enabling analysts to act on output without additional interpretation.
**Secondary model (if applicable):** Zero-Shot — provides a fast, confidence-scored first pass to pre-filter clearly routine records before sending to Groq, reducing API usage and latency at scale.
**Rejected models and why:**
- distilbert-sst-2 (Sentiment): Returned POSITIVE for all 5 records with an identical score of 0.7481, regardless of actual threat level. The model detects linguistic tone rather than security risk and is completely unsuitable for anomaly detection in a cybersecurity context.
## Failure Cases and Limitations
The most significant failure was the Sentiment model assigning POSITIVE to every record, including a brute-force SSH attack (Record 4) and a phishing email (Record 3). This demonstrates that general-purpose NLP models trained on movie reviews or social media text are not transferable to security operations
## Next Steps
With more time, I would explore the following: Larger dataset (20–50 records): Five records is insufficient to evaluate model reliability. A larger set with more ambiguous edge cases would reveal where Zero-Shot and Groq diverge and which handles gray-area alerts better. 2. Confidence-based routing: Add an IF node to send low-confidence Zero-Shot results (score < 0.80) to Groq for a second opinion, creating a cost-efficient hybrid pipeline.
Part 6: Upload to GitHub (20 minutes)
Step 6.1: Set Up Your Week 4 Folder
In your individual GitHub repository (or in your capstone group
