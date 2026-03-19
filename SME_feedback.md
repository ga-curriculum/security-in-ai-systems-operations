# SME Feedback: Security in AI Systems Operations
**Module:** Week 6, Class 12 — Security in AI Systems Operations  
**Reviewer:** Sumit Rahman  
**Documents Reviewed:** README.md (student-facing), Instructor Guide

---

## Overall Assessment

This is the most well-scoped security module that could realistically fit in 120 minutes for a non-security-specialist audience. The AI-specific framing (prompt injection, data
poisoning) is accurate and meaningfully differentiated from traditional web security. The "shift-left" mental model is correctly introduced. The OWASP ZAP and GitHub Security Scan
toolchain is a credible production stack. The three-section structure (identify → apply → evaluate) mirrors real DevSecOps workflows.

The primary gap is structural: the lab placeholder is the eleventh consecutive unresolved instance. There are also two technical issues worth addressing — OWASP ZAP is presented
as a tool students will actively run (impossible without a running service and 20+ minutes of scan time), and the distinction between OWASP ZAP findings and Dependabot CVE findings
is not clearly separated in the lesson narrative. The lab materials resolve both.

---

## 1. Structural Issues

**Issue 1.1 — Security Pipeline Diagram Appears in Both README and Instructor Guide**

The Mermaid diagram in Section 3 (`Developer Commit → GitHub Security Scan → Build Container → OWASP ZAP → Deploy`) is reproduced verbatim in the instructor guide's
delivery tips section. One copy is sufficient. Recommend keeping it in the README and removing it from the instructor guide (or simplifying the instructor guide version
to a reference: "show the diagram from the README").

---

## 2. Alignment with Syllabus and Competency Map

| Learning Objective | In README? | In Instructor Guide? | Notes |
|---|---|---|---|
| Describe common vulnerabilities in AI systems and data pipelines | ✅ Section 1 | ✅ Yes | Prompt injection + data poisoning framing is AI-specific and accurate |
| Apply access control and encryption practices | ✅ Section 2 | ✅ Yes | RBAC + secret scanning example is concrete |
| Evaluate system security posture using vulnerability scans and compliance checks | ✅ Section 3 | ✅ Yes | DevSecOps pipeline diagram is the clearest visualisation in this section |

All three LOs are covered. The weakest is LO 3 — the lesson names the tools but does
not show what their outputs look like. A student who has never seen a ZAP report or a
Dependabot alert will struggle to "evaluate system security posture" without example
output to interpret. The lab materials provide both.

---

## 3. Technical Content

**Issue 3.1 — OWASP ZAP Presented as a Lab Tool Without Setup Guidance**

Section 1 and the lab objective describe OWASP ZAP as a tool students will "configure to actively attack the endpoint." Running a ZAP active scan requires:
1. A running target API service
2. ZAP installed (250MB+) or Docker image pulled
3. 15–20 minutes of active scan time
4. ZAP report interpretation

This is not achievable within a 60-minute lab alongside the other objectives. The lab materials resolve this by providing a pre-collected ZAP JSON report that students parse
and triage in the notebook — giving the same analytical skill without the setup overhead.

**Issue 3.2 — GitHub Secret Scanning Requires GitHub Advanced Security (Paid)**

Section 2 presents GitHub Secret Scanning as a tool students will use. However, GitHub Secret Scanning is only available on public repositories or repositories with GitHub
Advanced Security (a paid feature not available on free plans). Students with private repos on free plans will not see the feature. The lab materials use `truffleHog`
(open-source) as the equivalent in the CI workflow, and include a Python implementation of the same logic for the notebook. This is worth noting in the lesson with a one-sentence
clarification.

**Issue 3.3 — Data Pipeline Poisoning Not Operationalised**

Section 1 correctly names data poisoning as an AI-specific threat, but gives no practical guidance on how to detect or prevent it. This is appropriate for an
introductory module, but the lesson would benefit from one sentence connecting data poisoning to the observability tools already covered: a sudden drop in model confidence
score (visible in Lab 5's Prometheus Gauge and Lab 9's canary AnalysisTemplate) can be an indicator of training data tampering.

**Issue 3.4 — Prompt Injection Defence Not Distinguished from Input Validation**

Section 1 describes prompt injection but does not distinguish it from standard input validation. Traditional input validation (SQL injection defence) sanitises inputs to
prevent them from being interpreted as commands by a database. Prompt injection is fundamentally different: the input IS the command (that's how LLMs work). Defence
requires semantic analysis of intent, not syntactic sanitisation. The lab's Stretch Problem 4.1 implements a basic pattern-based defence and correctly notes this is a
simplified approach — a one-sentence note in the lesson body would frame this for students before they encounter it.

---

## 4. Instructor Guide

**Issue 4.1 — ZAP Demo is Impractical in a 40-Minute Slot**

The pacing guide allocates 40 minutes to "DevSecOps & Scanning Demo" with instructions to "Demonstrate using OWASP ZAP to fuzz an AI model's API wrapper." A ZAP active scan
typically takes 15–20 minutes to run. Combined with service startup, ZAP setup, and result interpretation, this easily exceeds the 40-minute window. Recommend replacing
the live ZAP demo with a walkthrough of the pre-collected ZAP JSON report — which teaches the same interpretation skill in 10 minutes.

**Issue 4.2 — Common Issue: "Confusion between Prompt Injection and SQL Injection"**

The troubleshooting table entry on this confusion is the most valuable teaching note in the instructor guide. It should be elevated to a delivery tip rather than buried in
the troubleshooting table, since it is a conceptual confusion that will arise during the lecture, not just during the lab.

---

## 5. Theory-to-Lab Alignment Recommendations

**Recommendation 5.1 — Show a Sample ZAP Finding in Section 1**  
One example ZAP alert JSON (5–10 lines) in Section 1 or 3 would make the "evaluate security posture" LO actionable — students would know what format to expect before
parsing the full report in the notebook.

**Recommendation 5.2 — Add truffleHog as Open-Source Secret Scanning Alternative**  
A parenthetical noting that `truffleHog` is the open-source equivalent of GitHub Secret Scanning (and usable in any CI system including free-tier GitHub Actions)
would give students a tool that actually works in their environment.

**Recommendation 5.3 — Connect Data Poisoning to Confidence Score Monitoring**  
One sentence connecting data poisoning detection to the confidence score Gauge from Lab 5 would tie the AI-specific threat to tooling students have already built:
"A sudden unexplained drop in model confidence score can be an early indicator of training data compromise — exactly the metric your Lab 5 Prometheus Gauge monitors."

---

## Summary of Action Items

| Priority | Issue | Type | Owner |
|---|---|---|---|
| High | Clarify that OWASP ZAP cannot be run live in the lab; use pre-collected report | Fix | Content Author |
| High | Note GitHub Secret Scanning requires paid plan; recommend truffleHog alternative | Fix | Content Author |
| Medium | Replace live ZAP demo with pre-collected report walkthrough in instructor pacing | Fix | Content Author |
| Medium | Elevate prompt injection vs. SQL injection distinction to a delivery tip | Fix | Content Author |
| Medium | Remove duplicate security pipeline diagram from instructor guide | Fix | Content Author |
| Low | Add sample ZAP finding JSON to Section 1 or 3 | Recommendation | Content Author |
| Low | Connect data poisoning detection to confidence score monitoring (Lab 5 link) | Recommendation | Content Author |
| Low | Note prompt injection requires semantic defence, not syntactic sanitisation | Recommendation | Content Author |
