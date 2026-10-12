# ASE 2026 — Tổng hợp Vấn đề & Ý tưởng + Tool/Dataset mới (DOI: 10.1145/3832783)

**Hội nghị:** 41st IEEE/ACM International Conference on Automated Software Engineering (ASE 2026), Munich, 12–16/10/2026.
**Nguồn:** ACM DL `doi/proceedings/10.1145/3832783` (bị 403 với bot, đã kiểm chứng là open-access qua trang chủ) + program công khai `conf.researchr.org/home/ase-2026` (đã fetch + 4 subagents trích Accepted Papers). Trang chủ xác nhận 15 proceedings (1 chính + 14 workshops) đều open-access.
**Phạm vi:** Proceedings chính (không gồm 14 workshops AgenticDev/AISM/APR/... có DOI riêng). Tổng **498 mục**:

| Track | Số lượng |
|---|---|
| Research | 263 |
| Industry Showcase | 75 |
| Tools and Datasets | 63 |
| Journal-first | 43 |
| NIER | 23 |
| Student Research Competition (SRC) | 21 |
| Doctoral Symposium | 7 |
| Keynotes | 3 |
| Posters | 0 (chưa công bố, slot trống) |

File kèm: `ASE2026_Danh_sach_bai_theo_track.csv` (498 dòng) + 8 file txt theo track (cùng thư mục `ase2026/`).
> Mức tin cậy: danh sách tiêu đề đã xác minh từ program (cao; Research 100% có DOI 10.1145/3832783.*, Tools 62/63 có DOI, SRC 21/21 có DOI, Industry 10/75 có DOI, NIER 1/23 có DOI, Journal-first toàn DOI journal gốc). Phần Vấn đề/Ý tưởng suy từ tiêu đề + track (trung bình); ~4 abstracts đã đọc trực tiếp qua arXiv/PDF snippet (ghi rõ). Không bịa abstract.

**Abstracts đã đọc trực tiếp:**
- Certified Program Synthesis with a Multi-modal Verifier (ASE26 Research, DOI …3837559 — PDF snippet: pipeline LeetProof trên Velvet/Lean, benchmark 50 problems imperative).
- WiseSpec: Requirements-Driven Agents for Code Generation (SRC, DOI …3844573 — arXiv snippet: requirements-driven agents).
- Loop Engineering: Building Blocks, Adoption, and Impact (JAWs@ASE26 workshop — scan 36.645 repos, chỉ 2 state files đạt chuẩn).
- Agentic Verification of Software Systems (FSE26 liên quan, đã đọc ở báo cáo FSE — bổ trợ cho cụm verification ASE).

---

## 1. Vấn đề chung xuyên hội nghị

1. **Agentic AI bùng nổ nhưng thiếu an toàn/đánh giá:** ~80+ tiêu đề chứa Agent/Agentic/Multi-agent. Vấn đề lặp lại: hallucination package, credential leak qua skills (How Your Credentials Are Leaked by Agent Skills), malicious skills (MalSkills), backdoor instruction, compression thành attack surface, misalignment cần provenance, cần runtime monitoring (ProbGuard, RefineAct).
2. **Repair/test ở quy mô repo:** issue resolution cần nén context, multi-hunk bugs, flaky timing-dependent, oracle yếu (dual-context, TORI), benchmark lệch (PAIChecker về PR-issue misalignment trong SWE-Bench-like).
3. **Fuzzing bão hòa chẩn đoán:** cần directed/path-difficulty/boundary-oriented (ExcavatorFuzz, Planner-Fuzz cho UAV), multi-fault interaction (Beyond the First Crash, Phantom Fixes — NIER), config-aware cho cloud (ConfFuzz).
4. **Supply-chain/SBOM:** reuse genealogy C/C++ (GANADI), conditional dependencies C/C++, VeriSBOM bằng ZK-proofs, AIBOM (AIGen), backdoor lúc commit/release (Not In My Git Yard), multiple patches cho 1 CVE.
5. **Di trú legacy:** COBOL (COBug, K-COBAgent NIER), C→Rust (Towards Reliable C-to-Rust + RL, SafeDep, VARIES harness), enterprise modernization IP-preserving.
6. **Con người + năng suất:** 11.579 IDE sessions (Programming by Chat), eye-tracking khi review code LLM-labelled, toxicity GitHub, GenAI governance (To Ban or Not to Ban), dyslexia-inclusive support.
7. **Quantum + CPS/autonomous:** lifecycle bugs quantum, testability metric (Quantum Squeeziness), QUTest/QDoctor/KQFuzz; ADS testing (PCLA-Bridge CARLA, camera-perturbation prioritization, falsifiable oracle).

## 2. Ý tưởng/giải pháp tiêu biểu theo cụm

- **Agent cho SE có kiểm soát:** Compressing Code Context cho issue resolution; Prism (multi-solution reasoning cho repo-issues); Free Is More (issue-free trajectory + entropy-aware RLVR); LaMAR (collab qua KV-cache cho APR); MultiFixer (coordinator-proposer cho multi-hunk); Agent-HAnS (giữ traceability feature↔code); TraceGate (policy-controlled failure evidence); Graphectory Viewer (phân tích trajectory theo process); RELY-SE (dashboard độ tin cậy agent); FlightRecorder (trace fine-grained dev với AI assistants).
- **Verification + synthesis:** Certified Program Synthesis (LeetProof/Velvet trên Lean); DafnyRAG + DSpec2Test + ProofPulse (bộ ba Dafny); VerusSeek (RAG proof synthesis cho Rust); Kani/KVerus (model checker + proof gen cho Rust, Industry); Barista (synth typestate specs bằng agents); TLCrafter (NL→temporal logic); RFC2TLA+ (RFC→formal models bằng LLM feedback liên tục); Spec2V (spec→Verilog + verification-driven debug).
- **Fuzzing thế hệ mới:** CtxFuzz (fuzzer-agent cooperation), ExcavatorFuzz (path-difficulty), RamFuzz (spatial memory via range violation), ConfFuzz (configurable cloud), Planner-Fuzz (boundary cho UAV), WiDepFuzz (Wi-Fi hotspot field-dependency), KQFuzz (quantum libs), IcFuzz (Isaac Sim semantic guidance); BanditFuzz/Beyond First Crash (NIER, differential + multi-fault).
- **Security/supply-chain:** MalSkills (neuro-symbolic phát hiện malicious skills); S2CAgent (knowledge-graph cho supply-chain); GANADI (pivotal-function clustering cho C/C++ reuse); VeriSBOM (ZK-proofs chia sẻ SBOM); SmartPoC + Trace-Driven PoC Synthesis (validate vuln bằng PoC thực thi); SecChain (operationalize guidelines cho multi-agent smart-contract gen); Not In My Git Yard (bắt backdoor lúc commit/release).
- **AIOps/microservices:** ARMOR (RCA khi thiếu modality, self-supervised); GALA (graph-augmented agents cho RCA + incident response); HERO (hypothesis-centered RCA); Panorama (meta-learning latent dependencies cho autoscaling); OpenStack-RCA-Bench (dataset IaaS tái lập được); Smart Brain (semantic anomaly cho time series).
- **Mobile/Android/Web:** GraphDroid (async LLM GUI testing), UIXPOSE (NIER, screen-conditioned discrepancy cho security-aware exploration), LiteScope (security paradox của Lite apps), Mobile Bug Reproduction (global-state reprioritization + trajectory), ReproBreak (dataset web locator breaks), Testing IFrame Rendering.
- **Benchmarks/datasets đáng dùng lại:** RACE-Bench (repo-level feature addition), RepoProbe (architecture-aware comprehension checklists), JavaVulBench (leakage-aware), CppPerf (perf-improving commits), OpenStack-RCA-Bench, EVerest (secure SE), CAS2UML (sketch→PlantUML), PITMuS (sinh bug dataset bằng mutant reconstruction).
- **IVR/NIER (tầm nhìn):** Distributional Program Analysis (coi LLM như program sampler); ECLAIR (causal AI cho ESE discovery); Specifying Delegated-Autonomy Boundary (RE cho agentic AI); Do Not Copy/Paste (soft barriers khi copy trong AI-assisted programming); EU AI Act artifacts/patterns; The Ground Is Shifting (nền tảng measurement).
- **Industry Showcase (tín hiệu áp dụng):** Meta RADAR (low-risk review + risk calibration), Google Low-Latency Agentic APR, SAP HANA disassembly-augmented RCA, WeChat Pay PBT ở billion-user scale, Xiaomi Vela Li-WaC (WASM container cho IoT), Prime Video diff-aware deployment risk, Kani/KVerus cho Rust, Model-Guided PBT, MISRA C violation detection bằng multi-agent + runtime data.
- **Journal-first (đọc lại):** Byam (fix breaking deps bằng LLM), PoCo (agentic PoC cho smart contracts), Hallucination to Consensus (multi-agent JUnit gen), UntrustVul (lọc alert thiếu tin cậy), PrevaRank (rank patches bằng historic features).

## 3. Tool & Dataset mới (63 Tools and Datasets — đáng copy nhất)

- **Traceability/reliability:** Agent-HAnS, ARDoCo (REST API + TraceView/TraceViz), RELY-SE, FlightRecorder, TraceGate, Graphectory Viewer.
- **Spec/verification:** ACSLBench (C/ACSL corpus + call chains), DafnyRAG/DSpec2Test/ProofPulse, StarFuse (multimodal interactive theorem proving), VerusSeek, TLCrafter, SAF (static analysis framework cho LLVM IR, agent-ready + browser-accessible).
- **Test/quality:** TORI (oracle quality), Marv (visualize mutation), multiplex (modular LLM mutation), FlakeFighters (pytest flaky plugin), SplitCA (4-wise covering arrays), QUTest (quantum), UltraInstinctVR (VR system testing), Cam2Sim/DT-Drive/E-CoDrive/MASSIV (bộ tứ autonomous driving: neural reconstruction, deterministic replay, energy-critical co-sim, safety-case defeater RAG).
- **Bills of materials:** AIGen (AIBOM qua MLOps), AIRBORNE (fine-grained SBOM cho stack overflow), IoTutorMine (mine HBOM từ tutorial videos).
- **Năng lượng/công bằng:** EnergyTrackr, Baseliner (GPU benchmarking hardware-agnostic), FairLint-DL (IDE-native fairness debugging).
- **Dịch mã/di trú:** UCT (translation qua unified IR), FeatX (edit software bằng cách edit features), AutoFLC (flowchart cho legacy repo).

## 4. Keynotes (3 — khung tư duy)

- Michael Pradel — Trust the Delta: Rethinking Validation in the AI Era.
- Joost Noppen — Beyond the Hype: What AI Really Did to SE.
- Myra Cohen — Lost in Automated Specifications: Time and Space Odyssey in System Testing.

## 5. Đề xuất sáng kiến VN/Viettel từ ASE 2026

1. **Agent guardrails cho coding agents nội bộ:** học ProbGuard + RefineAct + MalSkills + TraceGate: runtime monitor + typed tool mediation + provenance, chặn credential leak/package hallucination trước khi merge.
2. **RCA cho cloud/microservices:** học ARMOR (thiếu modality) + GALA/HERO + OpenStack-RCA-Bench: dựng bench tái lập được từ logs/traces Viettel, thử hypothesis-centered RCA thay vì rule-based.
3. **Hiện đại hóa legacy + SBOM:** học K-COBAgent/COBug + UCT/FeatX + GANADI/VeriSBOM: pipeline COBOL/C→hiện đại có verification harness (VARIES-style) + chia sẻ SBOM bằng ZK-proofs cho đối tác.

---

## Hạn chế & cách lấy tiếp full abstract

1. ACM DL chặn bot (403) kể cả PDF open-access nên không crawl toàn văn tự động. Researchr có `details` + abstract cho subset lớn (Research/Tools/SRC có DOI đầy đủ; Industry/NIER phần lớn chưa gắn DOI).
2. File CSV kèm theo là danh sách đầy đủ đã xác minh từ program (không bịa). Vấn đề/Ý tưởng ở mức cụm + ví dụ kiểm chứng; muốn chi tiết 498 dòng cần duyệt từng DOI/preprint (vượt một lượt).
3. Đề xuất tiếp: cho mình 1 sub-list (20–30 bài từ CSV), mình sẽ fetch từng abstract (researchr details/arXiv/DOI 10.1145/3832783...) và điền bảng [Tên bài | Vấn đề | Ý tưởng | Phương pháp | Kết quả chính]. Workshops (14 DOI riêng) chưa gồm — cần thì làm tiếp theo cùng mẫu.
