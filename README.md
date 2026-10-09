# 📚 FSS Regulatory Intelligence Assistant QA Testing Portfolio (Manual Testing)

> **QA-focused project:** Functional, negative, robustness, regression, retrieval-quality, citation-grounding, security, reliability, and basic performance testing of a Retrieval-Augmented Generation (RAG) application for the Food Safety and Standards Rules, 2011.

**🔗 Live Demo:** [Launch Application](https://fss-regulatory-intelligence-assistant-jnq989xzzddibrbjmnbphm.streamlit.app/)

## 🔎 Project overview

The application answers natural-language questions using information retrieved from the Food Safety and Standards Rules, 2011. It uses a Retrieval-Augmented Generation (RAG) pipeline to retrieve document passages and generate responses, with source information intended to help users verify regulatory claims.

I tested the application from a **quality-assurance perspective**, focusing not only on whether an answer sounds correct, but also on whether it is relevant to the question, grounded in the supplied document, and supported by an appropriate retrieved source.

## 🧰 Skills demonstrated

- **Test case design:** positive, negative, boundary/input-validation, robustness, regression, security, reliability, and performance scenarios.
- **Functional testing:** application launch, query submission, and expected response behavior.
- **RAG testing:** retrieval relevance, paraphrased queries, misspellings, and whether answer-bearing passages are surfaced.
- **LLM answer-quality testing:** correctness, completeness, consistency, hallucination resistance, and handling of out-of-scope questions.
- **Citation/source validation:** checking whether the displayed source passage actually supports the generated claim.
- **Session/workflow testing:** submitting multiple distinct questions in one conversation.
- **Error-handling testing:** blank input and unavailable/invalid API-key configuration in a local test context.
- **Regression testing:** rechecking core regulatory questions after configuration changes.
- **Basic performance observation:** recording approximate response-time observations and identifying where repeatable timing data is still needed.
- **Defect documentation:** recording test IDs, scenarios, expected versus actual results, status, and observed issues.

**Tools and technologies:** Python · Streamlit · LangChain · FAISS · Hugging Face sentence-transformer embeddings · Groq LLM API · PDF document retrieval · Excel test-case tracking.

> These skills describe the testing activities represented in the test-case workbook. They do not imply automated test-framework coverage or independently verified execution beyond the recorded observations.

## 🏗️ Application architecture

```text
Food Safety and Standards Rules PDF
                |
                v
       Document processing
                |
                v
          Text chunks
                |
                v
     Hugging Face embeddings
                |
                v
       FAISS vector index
                |
User question -> Semantic retrieval
                |
                v
       Relevant document chunks
                |
                v
        LangChain RAG pipeline
                |
                v
             Groq LLM
                |
                v
       Generated answer + sources
                |
                v
           Streamlit UI
```

## 🧪 QA approach

For each scenario, I compared the **expected result** with the **observed application behavior**. For regulatory answers, the main acceptance criteria were:

1. The answer addresses the question asked.
2. Material claims agree with the supplied Rules document.
3. Retrieved passages are relevant and support the answer.
4. The application avoids inventing rules when evidence is missing.
5. Input and configuration failures are handled safely and clearly.
6. A new question receives a response to that question rather than stale or repeated content.

### Test coverage

| Test area | What was checked |
|---|---|
| Functional | Application launch and query handling |
| RAG retrieval | Relevant regulatory passages and top-k result relevance |
| Answer quality | Accuracy, completeness, and consistency with the PDF |
| Citation validation | Whether cited/retrieved text supports the answer |
| Negative testing | Out-of-scope questions and underspecified penalty questions |
| Input validation | Query without a question mark and whitespace-only input |
| Robustness | Misspelled regulatory terms |
| Workflow | Multiple questions in the same session |
| Security | Prompt injection requesting fabricated rules |
| Reliability | Invalid or unavailable API-key configuration |
| Performance | Approximate end-to-end response time observations |
| Regression | Rechecking core questions after configuration changes |

## 📊 Test results

The uploaded workbook contains **20 test cases**. The status counts below are calculated from the individual rows in the `QA Test Cases` sheet.

| Result | Count | Interpretation |
|---|---:|---|
| ✅ Pass | 10 | Observed result met the expected result |
| ❌ Fail | 9 | A functional or quality issue was observed |
| ⚠️ Partial | 1 | Evidence or behavior was incomplete |
| **Total** | **20** | **50% pass rate** |

**Pass rate:** 10 / 20 = **50%**. This is the result of the recorded test-case statuses, not a claim of production readiness.

> **Reporting note:** The workbook's `Summary` tab has counts that do not match the individual test-case rows. The table above uses the row-level statuses. Update the workbook summary so both views agree before publishing the test report.

## ✅ Representative passed test cases

| Test ID | Scenario | Observed result |
|---|---|---|
| TC-001 | Application launch | Streamlit application launched successfully; approximately 5 seconds was recorded. |
| TC-003 | Designated Officer training | Correctly returned the six-month training requirement and displayed source information. |
| TC-004 | Number of sample parts | Returned four parts and displayed retrieved sources. |
| TC-005 | Food Analyst report deadline | Returned 14 days from receipt of the sample and displayed source information. |
| TC-008 | Out-of-scope question | Did not invent an answer to “What is the capital of Japan?” |
| TC-009 | Unspecified penalty | Did not invent a specific penalty for an unspecified offence. |
| TC-010 | Query without question mark | Answered a valid question without a trailing question mark. |
| TC-013 | Paraphrased training question | Returned the same six-month requirement for a differently worded query. |
| TC-015 | Prompt injection | Refused to invent a regulatory rule not supported by the document. |
| TC-017 | Response-time observation | Responses were observed at approximately 2–3 seconds per question; a full per-question timing log is still needed for a formal benchmark. |

## ❌ Representative failed and partially passed test cases

| Test ID | Scenario | Observed issue | QA significance |
|---|---|---|---|
| TC-002 | Food Safety Officer duties | The application claimed the supplied document contained no description, although the test notes identify a relevant “Powers and Duties” section. | Retrieval/answer completeness defect |
| TC-006 | Broken or unfit sample container | The answer-bearing passage was not surfaced clearly enough to answer the question. | Retrieval and answer-quality defect |
| TC-007 | Receipt for seized food | The answer was “Form II,” but the displayed excerpt concerned seized books/documents rather than directly supporting the claim. | Citation-grounding defect |
| TC-011 | Whitespace-only input | Returned a generic context message instead of demonstrating clear blank-input validation. | Input-validation defect |
| TC-012 | Misspelled query | Returned a partially useful answer, but the response was truncated and source details were not visible in the recorded result. | Partial pass; verifiability issue |
| TC-014 | Multiple questions in one session | The application repeated the prompts instead of clearly answering both questions. | Conversation/workflow defect |
| TC-016 | Invalid/unavailable API key | Application failed to launch, preventing validation of safe error handling. | Reliability/configuration defect |
| TC-018 | Regression | Earlier retrieval issues persisted after configuration changes; the Form II citation remained insufficiently relevant. | Regression defect |
| TC-019 | Relevant chunk ranking | Retrieved passages were inconsistent in relevance; some answer-bearing text was not surfaced effectively. | Retrieval-ranking defect |
| TC-020 | Claim-to-source verification | Some responses were correct, but not all claims were adequately supported by the displayed passages. | Grounding and citation defect |

## 🐞 Defect themes and improvement opportunities

### 1. Retrieval relevance and recall
Improve chunking, overlap, top-k selection, metadata filtering, and/or reranking so the passage containing the answer is surfaced for relevant regulatory questions. Validate changes against a fixed regression set.

### 2. Citation correctness
Evaluate the relationship between each material answer claim and its cited passage. A correct answer with an unrelated source should not pass citation validation.

### 3. Abstention and answer completeness
Distinguish between information genuinely absent from the PDF and information that retrieval failed to find. Avoid unsupported answers, but do not abstain when a relevant passage is available.

### 4. Input validation
Reject whitespace-only input with a clear validation message before retrieval or LLM invocation.

### 5. Conversation state
Verify that each submitted question is processed independently and that the assistant responds to the latest question without repeating prompts or leaking a previous answer.

### 6. Configuration and error handling
Handle missing or invalid API credentials gracefully. Show a safe, actionable error without exposing secrets, and avoid an unexplained application startup failure.

### 7. Performance measurement
Capture a timestamp or duration for each of 10 or more representative queries. Report median, p95 (where sample size supports it), errors, and the test environment. The current 2–3 second observation is approximate and is not a controlled benchmark.

## 🔁 Suggested next regression cycle

After fixing retrieval, citation, or prompt behavior, rerun at least:

- TC-002 — Food Safety Officer duties
- TC-003 — Designated Officer training
- TC-005 — Food Analyst report deadline
- TC-006 — Broken/unfit sample container
- TC-007 — Form II receipt and citation
- TC-008 — Out-of-scope question
- TC-011 — Blank input
- TC-014 — Multiple questions in one session
- TC-015 — Prompt injection
- TC-016 — Invalid API-key handling
- TC-020 — Claim-to-source verification

A fix should be marked complete only after the expected answer, retrieved passage, and citation have been rechecked.

## 🖼️ Application screenshots

### Streamlit application
![FSS Regulatory Intelligence Assistant](https://github.com/snowflakelogic/FSS-Regulatory-Intelligence-Assistant/raw/main/FSS_streamlit_1.png)

### Advanced question answering
![Advanced Question Answering](https://github.com/snowflakelogic/FSS-Regulatory-Intelligence-Assistant/raw/main/Advance_question_fss.png)

## 📁 QA artifacts

- **Test-case workbook:** `FSS_RAG_QA_Test_Cases_Updated(2).xlsx` (20 test cases, expected/actual results, and statuses).
- **Application screenshots:** available in the repository links above.

For a stronger QA portfolio, consider adding a defect log with severity/priority, reproducible steps, environment details, and evidence; a requirements-to-test traceability matrix; and an automated smoke/regression suite.

## 🎯 What this project demonstrates for a QA role

This project demonstrates a structured approach to testing an AI-enabled application: designing test scenarios, comparing expected and actual behavior, identifying defects, validating sources, checking negative and security cases, and documenting regression risks. It also highlights an important AI QA distinction: **a fluent answer is not necessarily a correct answer, and a correct answer is not fully trustworthy when its citation does not support it.**
