# ICSE 2026 — Tổng hợp Vấn đề & Ý tưởng (DOI proceedings: 10.1145/3744916)

**Hội nghị:** 48th IEEE/ACM International Conference on Software Engineering (ICSE 2026), Rio de Janeiro, 12–18/04/2026.
**Nguồn:** ACM DL `doi/proceedings/10.1145/3744916` (bị Cloudflare 403, đã kiểm chứng) + program công khai tại `conf.researchr.org` (đã fetch toàn văn, 7144 dòng).
**Phạm vi:** Tất cả tracks — 543 mục đã trích xuất:

| Track | Số lượng |
|---|---|
| Research | 259 |
| Journal-first | 77 |
| SEIP (In Practice) | 59 |
| SEET (Education) | 45 |
| NIER | 34 |
| SEIS (In Society) | 22 |
| Demo | 19 |
| Plenary/Panel/Industry-Challenge | 28 |

File kèm: `ICSE2026_Danh_sach_bai_theo_track.csv` (543 dòng Track + Tiêu đề, trích máy từ program, đã lọc social/catering).

> Phương pháp & mức tin cậy: ~12 abstracts đã đọc trực tiếp qua trang `details` researchr / arXiv / ACM snippet (ghi rõ từng bài); phần còn lại suy từ tiêu đề + program listing (tin cậy trung bình). Không bịa abstract. Chi tiết xem từng cụm dưới đây.

---

## Cụm 1 — LLM & Agentic AI for SE (lớn nhất, ~70+ bài Research + SEIP)

**Vấn đề chung:**
1. LLM nhạy prompt / suy giảm khi gặp code sai: nhiễu diễn đạt nhỏ làm sụt correctness; code sai khiến test sinh ra kém 57% accuracy, 12% coverage, 24% bug-detection (bài *Measuring the Influence of Incorrect Code on Test Generation* — đã đọc abstract researchr).
2. Bảo mật: LLM tái tạo pattern lỗ hổng, review chung bỏ sót lỗi bảo mật, thiếu dataset + metric bảo mật.
3. Giới hạn snippet-level: task repo/project-level (issue Rust, dịch C→Rust cả dự án, root-cause microservices) đòi context xuyên file, call-chain, dependency.
4. Agent đơn lẻ kẹt cục bộ, thiếu đa dạng suy luận.
5. Chi phí agent (nhiều turn/tool-call) và thiếu grounding thực thi (hallucination, patch fail exploit).

**Ý tưởng tiêu biểu:**
- **Model editing & fine-tune chuyên biệt:** CREME (causal-tracing định vị layer nhạy-robustness rồi edit V-projection; +63% Pass@1 trên prompt-nhiễu, giữ nguyên input sạch; đã đọc abstract arXiv:2507.16407); SecureReviewer (dataset secure-review + secure-aware fine-tune + RAG + metric SecureBLEU; đã đọc abstract arXiv:2510.26457); Repairing LLM Executions for Secure Programming.
- **Multi-agent debate/ensemble/phản biện:** SWE-Debate (sinh fault-propagation trace trên dependency graph + debate 3 vòng + MCTS; SOTA agent open-source trên SWE-bench; đã đọc abstract arXiv:2507.23348); Agent-Based Ensemble Reasoning; Mock-Court (xử vulnerability kiểu tòa án); More with Less (nghiên cứu turn-control cho coding agent hiệu quả).
- **Execution-aware feedback loop:** CodeRover-S cho OSS-Fuzz (dùng exploit input + sanitizer report + dynamic/static call-graph thay issue-text; patch plausible 61–72%; đã đọc abstract arXiv:2411.03346); TestWeaver (sinh regression test theo feedback thực thi); Heterogeneous Prompting + Execution Feedback (chọn repro-test, ~63% fail-to-pass).
- **Repo-aware context:** RepoScope (call-chain multi-view); EvoC2Rust (skeleton-guided C→Rust project-level); SmartC2Rust (iterative feedback); Agentic Memory Recursive Reasoning (root-cause microservices).
- **Reasoning có cấu trúc:** HoarePrompt (lý luận Hoare bằng ngôn ngữ tự nhiên, Distinguished Paper); ADARULE (NL→LTL bằng pattern-adaptive rule induction); Cobblestone (divide-and-conquer cho formal verification); CASCADE (deobfuscate JS triển khai tại Google).

## Cụm 2 — Testing, Fuzzing, Flaky, CPS/Autonomous, Performance (~60+ bài)

**Vấn đề:** LLM sinh test nhưng flaky/oracle kém (đặc biệt DBMS công nghiệp SAP HANA); bùng nổ tổ hợp nhiễu khi test DL; thiếu oracle/coverage cho CPS, xe tự hành, robot (hệ liên tục, state-transition); test thực tốn kém/nguy hiểm, khó tái hiện; hệ phân tán/quy mô lớn (RPC black-box, microservices, GUI perf, cross-browser, chaos, dependencies) + reachability gap trong security testing.

**Ý tưởng:**
- LLM/VLM + MARL sinh & lọc test: AutoOracle (data quality-driven + filtering cho oracle C++); RPC-API testing bằng MARL+LLM; XBIDetective (VLM phát hiện cross-browser visual inconsistency); RVSG sinh hành vi người đa dạng test robot AMR trong simulator (đã đọc abstract arXiv:2508.02338); Scaling Mobile Chaos Testing with AI.
- Tomography/học low-order đoán high-order: TestifAI (partial model tomography, chỉ chạy 1–2 nhiễu để đoán 3–4 nhiễu, giảm 60–80% inference, lỗi <7%; đã đọc abstract arXiv:2608.18900); Predicting Failures in Smart EcoSystems; Misbehavior Forecasting cho ADS testing.
- Metamorphic + search-based/fuzzing cho CPS: MR từ design assumptions + genetic programming; boundary input generation cho CPS công nghiệp; fuzzing state-transition cho sUAS.
- Coverage/metric mới: Signal Feature Coverage cho CPS dataflow; Addressing Reachability Gap; PerFrame (GUI loading perf via semantic distinguish); Network-Level Fault Injection cho microservices.
- Nghiên cứu thực nghiệm công nghiệp dẫn hướng: Hamster (1.7M dev tests Java vs ATG; đã đọc abstract IBM Research); Flakiness of LLM-tests trên DBMS; Parallelization quản lý test dependencies.

## Cụm 3 — Security, Vulnerability, Privacy, Static Analysis (~60+ bài)

**Vấn đề:** SAST/taint nhiều false positive, khó sensemaking (model lib mặc định sai, view list/tree); LLM phát hiện vuln không tổng quát (label nhiễu 20–71%, ID cao nhưng OOD thấp); supply-chain/SCA/license khó (C/C++ copy-vendor mất origin, license variant PyPI, abandonment, pipeline poisoning, proxy smart-contract); hạ tầng CVE yếu (thiếu verification, thiên vị vendor, dispute mờ); mặt tấn công mới (jailbreak code-gen, scam token multimodal, PII unlearning, pentest AD lưỡng dụng); phân tích liên-ngôn ngữ/domain đặc thù (Android production, xe kết nối).

**Ý tưởng (nhiều bài đã đọc abstract — tin cậy cao):**
- Interrogative debugging: WhyFlow (debugger Q&A why/why-not/what-if cho taint + model lib thứ 3 + graph toàn cục; +21% accuracy vs CodeQL, −45% mental demand; ACM 10.1145/3744916.3773145, arXiv:2508.07198).
- Dataset curated + sinh tổng hợp có kiểm: BenchVul/TitanVul (38.548 hàm, dedup/validate multi-agent) + RVG; OOD 0.881 (+5.3%), +RVG 0.932 (arXiv:2507.21817); ground-truth SCA 28 app C/C++ verify tay trong Foiegras (P~0.7/R~0.5, vượt SCA thương mại, deploy Endor Labs).
- Kết hợp signature + embedding + chuyên gia (Foiegras: cắt function/type/license, index 1.700 dự án chuẩn); license-variant parsing PyPI.
- Empirical + human study: Demystifying CVE (51 post + 1.337 comment + survey 77 người; 4 nhóm vấn đề governance/động cơ/disclosure/data quality; ACM 10.1145/3744916.3773241); SAST Python; user study NASA-TLX.
- LLM bổ trợ phân tích cổ điển: SpecGuru (sinh points-to spec có self-validation); LLM-Aided Partial PDA; lọc FP trong công nghiệp.
- Multi-agent tấn công/phòng thủ: MazeBreaker (RL jailbreak động); PenForge/Can LLMs Hack AD (pentest tự động); XTrace (dynamic tracing non-invasive cho Android production).

## Cụm 4 — Human, Society, Education, Fairness, Open Source (~90+ bài SEIS/SEET/Research)

**Vấn đề:** cộng tác human-AI thiếu tin cậy/tri thức ngầm; đo năng suất bỏ qua executive function/cảm xúc/hybrid-remote; bias/fairness của ML/LLM chưa đo hệ thống; OSS thiếu bao trùm, dễ toxicity, rào cản newcomer, CoC hình thức, nữ giới ít; giáo dục SE lạc hậu so với kỷ nguyên LLM; yêu cầu đạo đức/XAI/RE cho agile lớn còn chung chung. (Phần này chủ yếu suy từ tiêu đề — tin cậy trung bình; websearch không trả abstract, ACM 403.)

**Ý tưởng:**
- Tín hiệu ngầm thay feedback tường minh: *Reading Between the Lines* (sentiment trong prompt dev làm feedback quy mô lớn; Distinguished Paper).
- Khung trust/fairness: TACO (trust assessment khi hỗ trợ code); CAFFE (counterfactual fairness); tuning correlation giảm bias giữ performance.
- Sức khỏe cộng đồng: *Toxicity Ahead* (forecast derailment GitHub); *Beyond Adoption* (tiến hóa/impact của Codes of Conduct lên OSS).
- Giáo dục lấy con người làm trung tâm: giảm tải executive-function của LMS; học qua YouTube/capstone/hybrid retrospective; khóa *AI-Driven Development* + mô hình đánh giá mới; *Lessons from Space Engineers* (Distinguished Paper, SEET).
- Kết nối tri thức tacit-hệ thống (*Dark Matter*); mở rộng bao trùm (newcomer OSS, remote cho người khuyết tật, women's participation, great QA engineer, empathy từ grey literature).
- Novice-AI interaction: *Beyond the Prompt* (cách novice đọc/kiểm tra/sửa output khi tóm tắt code; Distinguished Paper).

## Cụm 5 — Các luồng đáng chú ý khác

- **Requirements/Modeling:** SAT solver có theo kịp feature model của Linux kernel? (Distinguished Paper); XAI evaluative requirements từ clinical longitudinal study; RE principles cho large-scale agile (industrial longitudinal); goal modeling data-dependent cho ML law-enforcement.
- **Maintenance/Evolution/Architecture:** Remediating React re-render (Distinguished Paper); technical lag Java; CRUD semantics inference cho REST; third-party lib detection Android; 2 decades of software architecture education.
- **Mobile/Web/Performance/Energy:** PerFrame; GPU-accelerated web; WebAssembly in Node.js; Green SE (carbon reduction enterprise); RAG energy consumption experiment; LLM inference carbon estimation (SEALing the Gap, NIER).
- **NIER (ý tưởng mới):** Build Code is Still Code (pipeline poisoning); Undecidability of Overfitting in APR; Data-aware static analysis cho ML code; MAJIT (compat issues Android/iOS bằng multi-agent); Chaos engineering gateway (Distinguished Paper NIER); Diffusion LLM cho SE tasks.
- **SEIP công nghiệp:** WhatsCode (GenAI tại WhatsApp); SWE-Bench Illusion (LLM nhớ thay vì reason); R-Log (reasoning RL cho log analysis); TerraFormer (IaC với policy-guided verifier feedback, Distinguished Paper); DAT tại Meta; TypeJinja tại dbt Labs; test generation multi-agent industry-scale (Distinguished Paper).
- **Demo/Tool:** Chatbot Dōjō; fuzzing template từ protocol reverse engineering; GDSynth (graph DB testing); GUISpector (MLLM verify GUI requirements); JUnitGenie (path-sensitive unit test với LLM); Prophecy (infer formal properties từ neuron activations).

---

## Hạn chế & cách lấy tiếp full abstract

1. ACM DL chặn bot (403 Cloudflare) nên không crawl toàn văn proceedings tự động. Researchr chỉ gắn `details` + abstract cho subset (~172/543 links trích được; SEIS 0 link, NIER 10, SEIP 16, SEET 3, Demo 6, Journal-first 6).
2. File CSV kèm theo là danh sách đầy đủ đã xác minh từ program (không bịa). Cột Vấn đề/Ý tưởng chi tiết từng bài trong báo cáo này ở mức cụm chủ đề + ví dụ đã kiểm chứng; muốn chi tiết 543 dòng cần duyệt từng DOI/preprint (ước tính ~400+ abstracts, vượt một lượt trả lời).
3. Đề xuất tiếp: cho mình 1 sub-list (ví dụ 20–30 bài bạn quan tâm nhất từ file CSV), mình sẽ fetch từng abstract (researchr details/arXiv/DOI) và điền bảng chi tiết [Tên bài | Vấn đề | Ý tưởng | Phương pháp | Kết quả chính].
