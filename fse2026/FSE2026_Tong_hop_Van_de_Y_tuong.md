# FSE 2026 Companion — Tổng hợp Vấn đề & Ý tưởng + Tool mới (DOI: 10.1145/3803437)

**Hội nghị:** 34th ACM International Conference on the Foundations of Software Engineering (FSE 2026), Montreal, 5–9/07/2026.
**Nguồn:** ACM DL `doi/proceedings/10.1145/3803437` (Companion, 1962 trang, ISBN 9798400726361, bị Cloudflare 403, đã kiểm chứng qua websearch) + program công khai tại `conf.researchr.org/home/fse-2026` (đã fetch + 4 subagents trích Accepted Papers).
**Phạm vi:** Chỉ Companion (không gồm Research Papers chính — track này xuất bản ở PACMSE `dl.acm.org/toc/pacmse/2026/3/FSE`). Tổng **261 mục**:

| Track | Số lượng |
|---|---|
| Industry Papers | 65 |
| Journal-first | 59 |
| Ideas, Visions and Reflections (IVR) | 43 |
| Tool Demonstrations | 37 |
| Software Engineering Education | 21 |
| Doctoral Symposium | 17 |
| Student Research Competition (SRC) | 11 |
| Plenary/Keynotes/Awards | 8 (3 Accepted + 5 keynotes/award talks) |
| Posters | 0 (chưa công bố tại thời điểm fetch) |

File kèm: `FSE2026_Danh_sach_bai_theo_track.csv` (261 dòng) + 8 file txt theo track (cùng thư mục `fse2026/`).
> Mức tin cậy: danh sách tiêu đề đã xác minh từ program (cao). Phần Vấn đề/Ý tưởng suy từ tiêu đề + track (trung bình); ~6 abstracts đã đọc trực tiếp qua trang `details` researchr / ACM snippet (ghi rõ từng bài). Không bịa abstract.

**Abstracts đã đọc trực tiếp:**
- Agentic Verification of Software Systems (details FSE26-Research, NUS: agent chứng minh + Rocq/Coq loop, thử SV-COMP + Linux kernel modules).
- Generalizing Test Cases for Comprehensive Test Scenario Coverage (details: TestGeneralizer vs EvoSuite/gpt-o4-mini/ChatTester trên 506 methods, 1637 scenarios; 16/27 tests được merge).
- ScanCoder: Leveraging Human Attention Patterns to Enhance LLMs for Code (details snippet: mô phỏng attention kiểu eye-tracking ở quy mô lớn).
- TraceSynth: Generating Production-Quality Kernel Traces with Constraint-Guided Diffusion Models (ACM snippet: diffusion Transformer + constraint-repair cho event/timestamp/CPU/thread).
- A Comprehensive Study of ML Techniques for Log-Based Anomaly Detection (Journal-first details: deep learning vs ML truyền thống cho LAD).
- From Syntactic Mimicry to Semantic Correctness / Reliable and Developer-Aligned Evaluation of Agents (Doctoral: xác nhận hướng đánh giá agent theo developer-aligned, behavioral understanding).

---

## 1. Vấn đề chung xuyên Companion

1. **Agent hóa nhưng thiếu tin cậy:** agent sửa code/sinh test/triage incident hallucinate, thiếu grounding thực thi, khó đánh giá theo chuẩn developer (Doctoral có 2 bài liền về reliable/developer-aligned evaluation, behavioral understanding).
2. **Code review ở quy mô công nghiệp:** Meta/Atlassian/ByteDance/EA cùng hỏi: comment có hại? specificity? hallucination? reviewer cho high-risk diff? (5 bài Industry liền kề Code Review 1).
3. **AIOps/microservices/cloud:** batch failure, incident triage, root-cause, auto-remediation, anomaly detection trên trace/log quy mô lớn, LLM training fault localization (Reflex cho LLM training).
4. **Supply-chain & SCA:** C/C++ binary TPL, Java source SCA (JDetect), SBOM → attack-chain, patch porting inconsistency, backport decisions.
5. **Di trú ngôn ngữ:** C→Rust (project-level + RL alignment), Python lib migration (Pig), COBOL→hiện đại (COBook, Path-Guider notebook/agentic phân tích), Rust trong AOSP.
6. **Giáo dục SE trong kỷ nguyên GenAI:** gian lận/plagiarism (Criminal Minds), deliverables ≠ understanding, peer-evaluation, emotion-aware capstone, static analysis giáo dục.
7. **Sustainability/energy:** CodeGreen đo năng lượng, carbon-taxed transformers, energy flow graph, energy-aware test prioritization cho HPC.

## 2. Ý tưởng/giải pháp tiêu biểu theo cụm

- **Agentic SE có kiểm chứng:** Agentic Verification (proof agent ↔ Rocq, học on-the-fly, SV-COMP); AutoCodeRover cho SonarQube Issues (DOI 10.1145/3803437.3805209); RACER (perfective maintenance at scale); dynamic co-generation of repro-test trong APR; accountable APR (Who Wrote This Patch? DOI …3805552).
- **Review/test công nghiệp:** AI-Assisted Fixes at Scale (Meta), HalluJudge (reference-free hallucination detection), Hydra-Reviewer (multi-agent), Just-in-Time Catching Test Generation at Meta, Understanding/Mitigating Hallucinations in industrial unit-test gen, TestAgent (multi-agent repo-level unit test gen).
- **AIOps & runtime:** Aloha (contrast + human-in-loop cho batch failures), Agentic triage framework, Bridging the Delay (lag-aware causal inference), E2E-REME (experience-simulation RL fine-tune auto-remediation), TraceSynth (diffusion sinh kernel trace), ScalerEval (testbed auto-scaler), Reflex (event-driven FL cho LLM training).
- **Phân tích tĩnh/bảo mật khả mở rộng:** YASA (multi-language taint trên Unified AST tại Ant Group), JDetect (behavioral+code features cho Java SCA), AllocScope (allocation manipulation trong smart contracts), FISTS (security testing cho SDN updates), SCAR (mine audit reports), Cerberus (endpoint threat detection), JSProtect (obfuscation cho WeChat mini-games).
- **Di trú & dependencies:** Pig (LLM Python migration), Dependency-Guided C-to-Rust + RL, C2SaferRust (neurosymbolic, Journal-first), DependaFix (GitHub App sửa breaking updates cho Java), BackportCheck, Package Dashboard (dual-perspective cross-ecosystem).
- **IVR (tầm nhìn mới):** Watermark Paradox (provenance verification → camouflaged backdoors, DOI …3805553); Stylistic Blind Spot (bias coding-style lên LLM evaluation, …3805590); LLM-Assisted Repo-Level Generation with Spec-Driven Engineering (…3805567); Causal SE roadmap; Energy Flow Graph; Compilomorphic Fuzzing (dùng compiler chống chính nó); QuantumCode2Vec; AgentReputation (decentralized reputation cho agent).
- **Journal-first (đã qua journal, đáng đọc lại):** Galápagos (N-version programming bằng LLM), LPB-Gen (log-parsing benchmarks), HAFix (history-augmented bug fixing), CodeUltraFeedback (LLM-as-judge cho code preferences), MCP landscape/security, VLM-Fuzz cho Android GUI, SMARLA (safety monitoring cho DRL agents).
- **Giáo dục:** playbook GenAI-supported learning (From Domain Understanding to Design Readiness), BlocksDeFix (hints cho block-based), Refactoria (game học refactoring), emotion-aware capstone, Scrum simulation cho process-control threshold, prompt-engineering demand/supply/assessment.

## 3. Tool mới (37 Demonstrations — đáng copy nhất)

- **Hiểu/duy trì:** Package Dashboard (đã có PDF local `packagedashboard.pdf` — đối chiếu được), DependaFix (PDF local `DependaFix.pdf`), BackportCheck, SmartPatchLinker (linked-changes cho review), CoRenameAgent (multi-agent rename có human-in-loop), TokenScope (token-level explainability cho code-LLM).
- **Test/fuzz:** TestAgent (multi-agent repo-level unit test), Kea2 (property-based cho mobile), Argus (mutation testing có trace), Python Runtime Fuzzer, SQLynx (mutation fuzz DBMS đa dialect), Shark2Pit (template protocol fuzzing từ packet parser), MIMIC-Py (personality-driven game testing bằng LLM), TRACE/CapCo (CARLA accident reconstruction + co-simulation fuzzing).
- **Bảo mật/vận hành:** FISTS, IssueGuard (chặn leak secret trong issue reports, DOI …3806406), SCAR, IDSLab (low-code IDS dataset), PyMOP (runtime verification cho Python), HarnessForge (tách verification tasks từ dự án công nghiệp), ScalerEval.
- **Legacy & đặc thù:** COBook (notebook COBOL tương tác + visualization), Path-Guider (agentic COBOL path analysis trong IDE), ADATracer (traceability cho ADA — PDF local `ADATracer.pdf`), Foiegras-liên quan? (lưu ý: Foiegras là ICSE-SEIP, không thuộc FSE này; JDetect mới là SCA của FSE).
- **Năng lượng/giải thích:** CodeGreen, ProfOlaf/AISysRev (systematic review bằng LLM), SpecPylot (sinh Python spec bằng LLM), ZION (system exploration bằng class contours).

## 4. Industry Day thực dụng (65 bài — tín hiệu áp dụng)

Cụm Meta (review/test): AI-Assisted Fixes, Code Reviewer Recommendation cho high-risk diffs, Just-in-Time Catching Tests. Cụm ByteDance/Tencent/Ant (cloud/microservices): Aloha, Reflex, Bridging the Delay, YASA, RepoFuse (repo-level completion dual-context). Cụm xe/game (EA, automotive): AstraGame (VLM agent cho game testing), Dependency-Aware OTA cho SDV, VLM visual bug detection (19.738 keyframes/41h video). Cụm AIOps/K8s: LLM Agents for AIOps với Red Hat OpenShift, TSGen (troubleshooting guide gen), Eagle (benchmark question gen từ ops docs). Cụm developer experience: BayesInsights (Bloomberg Bayesian nets), NES (next-edit suggestion không cần instruction), LACY (mentoring onboarding bằng code tours).

## 5. Doctoral + SRC (hướng nghiên cứu mới)

- Doctoral (17): đánh giá agent theo developer, behavioral understanding (syntactic→semantic), trustworthiness (helpful→trustworthy pair programming), GitHub Actions evolution, legal requirements, quantum oracle verification, repo-level vulnerability datasets, PR-based secure development.
- SRC (11): log-centric overlay trên call graphs, rare logs ảnh hưởng log parsing, Docker reproducibility (timestamps), AI-authored PR merge prediction, declarative fault injection, spec refinement cho code gen.

## 6. Đề xuất sáng kiến VN/Viettel từ FSE Companion 2026

1. **Review/test ở quy mô Viettel:** học HalluJudge + Hydra-Reviewer + Meta Just-in-Time tests, áp vào repo nội bộ: reference-free hallucination check cho comment/sửa do LLM sinh trước khi tạo MR.
2. **AIOps cho cloud/5G:** học Aloha + E2E-REME + TraceSynth: contrast-analysis triage + diffusion sinh trace khi thiếu dữ liệu production (privacy/overhead), thử trên BTS/core logs.
3. **Di trú legacy:** học COBook + Path-Guider + DependaFix/Package Dashboard: notebook tương tác + agentic path analysis cho COBOL/Java cũ, kèm dashboard dual-perspective + auto-fix breaking updates trong CI.

---

## Hạn chế & cách lấy tiếp full abstract

1. ACM DL chặn bot (403) nên không crawl toàn văn Companion tự động. Researchr gắn `details` + abstract cho subset (ước tính ~1/3); Posters chưa có Accepted list; Research chính nằm ở PACMSE, ngoài DOI này.
2. File CSV kèm theo là danh sách đầy đủ đã xác minh từ program (không bịa). Cột Vấn đề/Ý tưởng chi tiết từng bài trong báo cáo này ở mức cụm chủ đề + ví dụ đã kiểm chứng; muốn chi tiết 261 dòng cần duyệt từng DOI/preprint.
3. Đề xuất tiếp: cho mình 1 sub-list (ví dụ 20–30 bài bạn quan tâm nhất từ file CSV), mình sẽ fetch từng abstract (researchr details/arXiv/DOI) và điền bảng chi tiết [Tên bài | Vấn đề | Ý tưởng | Phương pháp | Kết quả chính].
