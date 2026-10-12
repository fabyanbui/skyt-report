# MODELS 2026 — Tổng hợp Vấn đề & Ý tưởng + Tool/Phát kiến mới (DOI: 10.1145/3837062)

**Hội nghị:** ACM/IEEE 29th International Conference on Model Driven Engineering Languages and Systems (MODELS 2026), Málaga, 4–9/10/2026. DOI proceedings Companion: `10.1145/3837062`.
**Nguồn:** program công khai `conf.researchr.org/home/models-2026` (đã fetch + 3 subagents trích). ACM DL chặn bot nên dùng program làm ground truth.
**Phạm vi:** 151 mục:

| Track | Số lượng |
|---|---|
| Research Papers | 30 |
| Tools and Demonstrations | 30 |
| Posters | 35 |
| NIER | 24 |
| Industry Day | 13 |
| Journal First | 8 |
| Educators Symposium | 11 |

File kèm: `MODELS2026_Danh_sach_bai_theo_track.csv` (151 dòng) + 7 file txt theo track (cùng thư mục `models2026/`).
> Mức tin cậy: danh sách tiêu đề đã xác minh từ program (cao). Phần Vấn đề/Ý tưởng suy từ tiêu đề + track (trung bình); chỉ 2 bài có trang details (Zero-Width Markers, CANcept). Không bịa abstract.

---

## 1. Vấn đề chung xuyên hội nghị
1. **MDE implementation gap:** model đẹp nhưng không ra code/hệ chạy được; LLM sinh code bỏ qua model.
2. **Traceability model↔code↔runtime đứt:** đặc biệt khi LLM sinh code, view↔model, spec (.ads-like)↔body.
3. **Biến thể + nhất quán đa model:** product line, semantic variability, multi-model consistency dưới uncertainty.
4. **Thiếu benchmark/dataset cho modeling:** OCL, class diagram, sketch→model, boilerplate đánh giá LLM làm modeling.
5. **Low-code/no-code khó trợ giúp đúng:** assistance chung chung, không theo phương pháp, khó review.
6. **An toàn/bảo mật/certification:** automotive (ASIL, COLREGS, CARLA), aerospace (ESA, Lunar I-HAB, TTEthernet), y tế (FHIR), privacy (LINDDUN).

## 2. Ý tưởng/giải pháp tiêu biểu theo cụm
- **LLM + MDE có kiểm soát:** Constrained LLMs lấp implementation gap; Json-SysML biểu diễn 2 chiều cho LLM-MBSE; Anvil tổng hợp/validate/sửa Alloy spec; ModiGen sinh Modelica; LLM sinh instance model qua code synthesis; slicing hỗ trợ completion; OCL robustness benchmark.
- **Traceability & consistency:** Zero-Width Markers cho view-to-model trace; Preserving model-to-code traceability khi LLM sinh code; Senate kiểm lan truyền thay đổi liên model; UncertainTree + VitruviusOCL cho consistency đa model; MbEx học co-evolution từ thao tác user.
- **Agentic/assistant cho modeling:** Agentic model transformation (service-mediated); AssiStarMe + empirical low-code assistance; Jjodie conversational assistant thao tác (meta)model bằng script; FM2C sinh configurator low-code từ feature model; typed edits + blended co-modeling review được.
- **Digital twin & runtime:** Data contracts cho twin services; methodology twin liên tục; Asset Administration Shell maturity; Models@Orbit (space); ontology-driven corporate twin; anaerobic reactor twin (agri-food); runtime decision models under uncertainty (surprise-based); multi-runtime models comprehension.
- **An toàn/bảo mật:** Assurance suite COLREGS cho tàu tự hành; ArchSafe (ASIL + change-impact + failure); CI/CD security evidence stage-aware; metamodel sinh security model từ CTI; LINDDUN MAESTRO privacy; PolicyDSL access control.

## 3. Tool mới (30 Tools and Demonstrations — đáng copy nhất)
- **Hiểu/tái cấu trúc:** Code2LowCode (nâng clone thành abstraction), EMFular (editor đồ họa web-native cho EMF), Fastbelt (language engineering hiệu quả), Langium (language workbench TypeScript), GLSP+Langium interweaving, Class diagram visualization 2 chiều + LSP, RAMA review model trong PR.
- **Elicitation/domain:** Interactive domain elicitation & metamodeling, Who are my users? (ngôn ngữ đặc tả end-user profile), Domain Knowledge Debt (vì sao metamodeling vẫn cần elicitation).
- **Ô tô/robot/IoT:** CANcept (sinh/bẻ lưu lượng CAN — duy nhất có details page), CARLA Scenario Editor, MoRE (rule + runtime eval trong CARLA), SceneGEMS (tàu mặt nước tự hành), RuMROS* (swarm robot), ML2++ (IoT dự báo), ForeACT (forecast actionability), PRISMA (sinh DB instance nhất quán), MDEO-Cloud (tối ưu trên cloud), Cordis Suite (điều khiển sản xuất).
- **Quy trình:** Senate (federation change propagation), ModL (ledger tiến hóa data model/contract), MIMOS, LowSheets (spreadsheet low-code).

## 4. Industry Day (13) — tín hiệu áp dụng
Agentic Engineering Stack sau MBSE/Digital Thread; trợ lý agentic cho Avionic V&V; ASCEND (safety+security aerospace digital thread); Gen-AI+MDE thực dụng cho enterprise; twin lò kỵ khí agri-food; TTEthernet cho Lunar I-HAB; ESA MBSE journey; SysV2-Repair (sửa SysML v2 bằng LLM); Your AI Agent Needs a Model; Standards for Models Interoperability.

## 5. Educators + Journal First (gợi ý VN)
- Dạy UML trong kỷ nguyên GenAI (manual + prompt + evaluation), MDE literacy cho AI, Epsilon Playground, MDE không cần modeller (Epsilon+PlantUML), low-code trong đồ án sinh viên.
- Journal First: low-code platform engineering, scenario giao lộ exhaustive, EvolveDB (tiến hóa schema quan hệ theo model), SavviDriver (test xe tự hành đa agent), khảo sát nhu cầu tool dạy modeling.

## 6. Đề xuất sáng kiến VN/Viettel từ MODELS 2026
1. **Traceability model→code cho hệ legacy:** học Zero-Width Markers + Senate + MbEx, áp vào repo billing/viễn thông: mỗi requirement/model sinh marker bền qua LLM-generation.
2. **Low-code configurator từ feature model:** học FM2C + Bridging Feature Models and Users, làm configurator gói cước/dịch vụ số.
3. **Twin + assurance cho hạ tầng:** học data contracts + SceneGEMS/MoRE, làm twin trạm/BTS với scenario assurance.
