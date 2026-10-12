# Đề xuất: Bảo đảm pháp lý mã nguồn trong kỷ nguyên AI

> **Mục tiêu:** Trình bày với lãnh đạo về vấn đề bảo đảm pháp lý mã nguồn khi sử dụng AI trong phát triển phần mềm, và đề xuất hướng giải quyết.
>
> **Nguồn tham khảo:** ICSE 2026, FSE 2026, ASE 2026, MODELS 2026 (các hội nghị khoa học hàng đầu về kỹ thuật phần mềm).
>
> **Cách đọc:** Phần 1–3 dành cho mọi người. Phần 4–5 dành cho intech (đội kỹ thuật).

---

## 0. Tóm tắt 1 phút (cho lãnh đạo / pháp chế / kinh doanh)

**Câu hỏi:** Khi lập trình viên dùng AI để viết code, làm sao ta chứng minh với khách hàng / kiểm toán rằng phần mềm này "sạch" — không sao chép trái phép, không vi phạm giấy phép — mà không phải phơi bày toàn bộ bí mật công ty?

**Ví dụ đời thường:** như món ăn cần "giấy chứng nhận vệ sinh an toàn thực phẩm". Khách không cần biết công thức chi tiết, chỉ cần tem kiểm định đáng tin: nguyên liệu từ đâu, có chất cấm không, ai chứng nhận.

**Rủi ro nếu không làm:**
- Bị kiện bản quyền / vi phạm license (đặc biệt GPL copyleft) → bồi thường, phải mở mã nguồn.
- Mất hợp đồng outsourcing vì không trả lời được "code này AI viết hay người viết, lấy từ đâu?".
- Lộ IP khi cố chứng minh sạch bằng cách gửi toàn bộ danh mục thư viện cho bên ngoài.

**Giải pháp tóm gọn (3 lớp):**
1. **Gắn tem từ lúc viết (ngăn):** AI viết tới đâu, gắn nhãn nguồn gốc tới đó.
2. **Soát quá khứ (dọn):** quét kho code cũ, phát hiện giấy phép biến thể và code copy không rõ nguồn.
3. **Chứng nhận chọn lọc (chứng minh):** xuất "giấy khai sinh phần mềm" (SBOM/AIBOM) và chứng minh "không chứa chất cấm" mà không lộ công thức — bằng mật mã Zero-Knowledge.

---

## 1. Vấn đề cần giải quyết

### 1.1. Tình huống hiện tại

Khi đội ngũ phát triển sử dụng AI (như GitHub Copilot, ChatGPT, Claude) để viết code, chúng ta đang đối mặt với một vấn đề pháp lý mới:

> **Làm sao chứng minh với khách hàng, kiểm toán viên, hoặc tòa án rằng phần mềm của chúng ta "sạch" về mặt pháp lý?**

Cụ thể, cần trả lời được các câu hỏi:
- Code do AI viết có sao chép trái phép từ các dự án mã nguồn mở không?
- Có vi phạm giấy phép (license) của các thư viện bên ngoài không?
- Nếu bị kiện, làm sao chứng minh nguồn gốc của từng đoạn code?
- Làm sao chia sẻ thông tin cần thiết với khách hàng mà không lộ bí mật kinh doanh?

### 1.2. Tại sao đây là vấn đề quan trọng

| Rủi ro | Hậu quả |
|--------|---------|
| Vi phạm bản quyền / license | Bị kiện, bồi thường, buộc phải mở mã nguồn |
| Không chứng minh được nguồn gốc code | Mất hợp đồng outsourcing, mất uy tín |
| Lộ bí mật khi chứng minh | Đối thủ biết chiến lược kỹ thuật, mất lợi thế cạnh tranh |
| Không tuân thủ quy định | Bị phạt hành chính, cấm kinh doanh ở một số thị trường |

### 1.3. Ví dụ đời thường

Hãy tưởng tượng như sau:

- **Nhà hàng** cần giấy chứng nhận vệ sinh an toàn thực phẩm. Khách hàng không cần biết công thức nấu ăn chi tiết, chỉ cần tem kiểm định đáng tin cậy.
- **Phần mềm** cũng vậy: khách hàng cần "giấy chứng nhận" rằng code không sao chép trái phép, không vi phạm license — mà không cần xem toàn bộ mã nguồn.

Vấn đề là: **hiện tại chưa có "giấy chứng nhận" như vậy cho phần mềm có sử dụng AI.**

### 1.4. Vấn đề cụ thể cho đội kỹ thuật

**Phiên bản intech:**
- Input: repo + commit/PR + log sinh code LLM/agent + dependencies (PyPI/npm/C-C++ copy-vendor) + model/dataset kèm theo.
- Output: (1) báo cáo tương thích license, (2) provenance từng file/hunk (`human` / `ai` / `oss-reuse` / `unknown`), (3) SBOM/AIBOM verify có chọn lọc.
- Đối tượng: doanh nghiệp outsourcing/sản phẩm phải ký cam kết IP, đang dùng coding agent nội bộ.

**Vì sao khác SCA cổ điển:**
1. **AI làm mờ nguồn.** LLM có thể viết lại code bản quyền chỉ bằng cách đổi tên biến — detector thường bị qua mặt.
2. **Giấy phép ngoài đời là bản biến thể, không chuẩn.** Sai khác một câu đã đổi nghĩa pháp lý. Dev cũng lúng túng về copyright khi dùng GenAI.
3. **Code + AI trộn vào nhau.** License model/dataset trên Hugging Face đã hỗn loạn; chuẩn SPDX cho AI (AIBOM) vẫn đang xây dở; code C/C++ copy-thủ công mất nguồn gốc.
4. **Muốn chứng minh sạch lại sợ lộ.** Khách muốn xem hết SBOM, ta sợ lộ chiến lược kỹ thuật. `VeriSBOM` giải đúng mâu thuẫn này bằng Zero-Knowledge.

---

## 2. Bối cảnh kỹ thuật (đơn giản hóa)

### 2.1. Các khái niệm chính

| Thuật ngữ | Giải thích đơn giản |
|-----------|---------------------|
| **License (Giấy phép)** | Quy định "được phép làm gì" khi dùng lại code người khác. Ví dụ: MIT cho phép dùng thoải mái, GPL yêu cầu phải mở lại mã nguồn. |
| **Provenance (Nguồn gốc)** | Tem truy xuất "đoạn code này từ đâu": người viết, AI viết, hay copy từ thư viện X. |
| **SBOM (Software Bill of Materials)** | Bảng kê "nguyên liệu" của phần mềm: danh sách các thư viện, phiên bản, giấy phép. Giống như thành phần trên bao bì thực phẩm. |
| **AIBOM** | SBOM mở rộng, bao gồm cả thông tin về AI: model nào được dùng, dữ liệu huấn luyện từ đâu. |
| **SCA (Software Composition Analysis)** | Máy quét nguyên liệu — quét xem có thư viện dính lỗi/cấm không. |
| **Zero-Knowledge Proof** | Kỹ thuật mật mã cho phép chứng minh một điều gì đó mà không cần lộ thông tin bí mật. Ví dụ: chứng minh "tôi không dùng GPL" mà không cần khai toàn bộ danh sách thư viện. |
| **Membership inference / Watermark** | Truy xem AI có "học lén" code ai không / đóng dấu chìm. Kiểm tra đoạn code có phải AI nhớ từ đâu đó. |

### 2.2. Tại sao công cụ hiện tại chưa đủ

Các công cụ quét license truyền thống (SCA) chỉ hoạt động tốt khi code được copy nguyên văn. Nhưng AI có thể:
- **Viết lại** code bản quyền chỉ bằng cách đổi tên biến, thay đổi cấu trúc
- **Trộn lẫn** nhiều nguồn khác nhau thành một đoạn code mới
- **Tạo ra** code giống với code trong tập dữ liệu huấn luyện (memorization)

Kết quả: công cụ quét thông thường bỏ qua nhiều trường hợp vi phạm tiềm ẩn.

---

## 3. So sánh với cách làm hiện tại của các đối thủ

| Tiêu chí | Cách làm truyền thống | Cách làm mới đề xuất |
|----------|------------------------|----------------------|
| **Phát hiện vi phạm** | Chỉ phát hiện code copy nguyên văn | Phát hiện cả code đã được AI viết lại |
| **Nguồn gốc code** | Không có thông tin về AI | Ghi rõ code do AI hay người viết |
| **Chứng minh với khách hàng** | Phải mở toàn bộ mã nguồn | Chứng minh chọn lọc, không lộ bí mật |
| **Cập nhật khi luật thay đổi** | Phải sửa thủ công | Tự động cập nhật tiêu chí kiểm tra |
| **Chi phí tuân thủ** | Cao (nhiều công sức thủ công) | Thấp hơn (tự động hóa nhiều bước) |

**Kết luận:** Các công ty đi trước (như Meta, Google, Microsoft) đang đầu tư mạnh vào lĩnh vực này. Nếu chúng ta không hành động, sẽ bị động khi khách hàng bắt đầu yêu cầu chứng nhận pháp lý.

---

## 4. Bằng chứng từ hội nghị (đối chiếu cho đội kỹ thuật)

| Nguồn | Bài | Dùng cho gì |
|---|---|---|
| ICSE26 Research | Small Changes, Big Trouble (PyPI License Variants) | Parser license-variant, lõi check tương thích |
| ICSE26 Journal-first | Developer Perspectives on Licensing/Copyright + GenAI | Yêu cầu disclose AI, phân định trách nhiệm |
| ICSE26 Research | Same Same But Different (anti-refactoring plagiarism) | Detector chịu được tấn công refactor |
| ICSE26 Research | Membership Inference vs Semantic Transforms | Cơ sở truy nguồn memorization |
| ICSE26 NIER | TraceCaps: Inline Provenance + Risk Enforcement | Gắn provenance inline khi agent viết code |
| ICSE26 SEIP | Open AIBOM (mở rộng SPDX SBOM ISO/IEC 5962:2021) | Chuẩn BOM cho AI supply chain |
| ASE26 Research | VeriSBOM (ZK-proofs chia sẻ SBOM) | Chứng minh chọn lọc không lộ hết |
| ASE26 Research | GANADI (C/C++ reuse genealogy) | Dựng phả hệ reuse khi mất origin |
| ASE26 Research | Safeguarding Agents via Provenance Analysis | Dùng provenance để audit/misalignment |
| ASE26 NIER | Regulatory Compliance Pipeline: Legal → Tests | Biến luật thành test tự tiến hóa |
| ASE26 Industry | IP-Preserving Modernization via Spec Extraction | Mẫu giữ IP khi dùng LLM |
| ASE26 Tools | AIGen (AIBOM via MLOps), AIRBORNE (fine-grained SBOM) | Sinh BOM tự động từ pipeline |
| FSE26 IVR | Watermark Paradox | Cảnh báo: verification có thể bị lợi dụng giấu backdoor |
| FSE26 IVR | AST-PAC (AST-guided Membership Inference) | Kiểm tra code có trong training set |
| FSE26 IVR | Attack Chains from SBOM Graphs | Tăng giá trị SBOM: từ kê khai sang dự đoán rủi ro |
| FSE26 Journal-first | Classifier or Prompt: Legal Requirements Traceability | So sánh phương pháp trace yêu cầu luật |
| FSE26 Doctoral | Democratizing Legal Requirements | Đưa yêu cầu luật tới dev thường |

Chi tiết đầy đủ xem trong các file tổng hợp ICSE/ASE/FSE/MODELS 2026 trong repo.

---

## 5. Các hướng giải quyết đề xuất

Tôi đề xuất **ba hướng giải quyết** bổ sung cho nhau, từ ngăn ngừa, dọn dẹp, đến chứng minh. Mỗi hướng có 2 lớp diễn giải: non-tech và intech.

### Hướng A: Gắn tem từ lúc viết (Ngăn ngừa)

**Ý tưởng:** Khi AI viết code, hệ thống tự động gắn nhãn nguồn gốc ngay lập tức. Như camera + nhật ký bếp: món nào nấu bởi ai, nguyên liệu nào, máy tự báo khi dùng nguyên liệu cấm.

**Cách hoạt động (non-tech):**
- Mỗi đoạn code được AI tạo ra sẽ có "tem" ghi lại: model nào, prompt gì, có chỉnh sửa bởi người không
- Hệ thống cảnh báo ngay khi phát hiện code giống với mã nguồn mở có giấy phép nghiêm ngặt
- Như có camera an ninh trong bếp: mọi thứ đều được ghi lại

**Cách hoạt động (intech):**
- Học **TraceCaps** + **Safeguarding via Provenance**: mỗi hunk gắn `{model, prompt-hash, retrieved-context, human-edit-ratio, risk}`
- Pre-commit hook chặn nếu similarity với OSS copyleft cao
- Log append-only
- Kết hợp **AST-PAC**/membership inference đánh dấu memorization
- Đo: % hunk có provenance, precision/recall chặn PR, overhead latency

**Lợi ích:**
- Phát hiện vấn đề ngay khi xảy ra, không phải đi quét lại sau
- Tạo bằng chứng truy xuất từ đầu

**Lưu ý:** Đừng tin watermark tuyệt đối — **Watermark Paradox** (FSE26 IVR) chỉ ra verification có thể thành chỗ giấu backdoor; cần verification độc lập + review người.

---

### Hướng B: Soát xét kho code cũ (Dọn dẹp)

**Ý tưởng:** Quét toàn bộ kho code hiện có, phát hiện các vấn đề tiềm ẩn về license và nguồn gốc. Như tổng kiểm kho: phát hiện tem mờ, nhãn biến thể, hàng không rõ xuất xứ rồi dán lại nhãn đúng.

**Cách hoạt động (non-tech):**
- Quét tất cả các thư viện, dependencies đang sử dụng
- Phát hiện code copy từ mã nguồn mở không rõ nguồn gốc
- Đánh dấu các giấy phép "biến thể" (variant) có thể gây hiểu lầm
- Tạo báo cáo tổng thể về tình trạng pháp lý của codebase

**Cách hoạt động (intech):**
- Nối **Small Changes, Big Trouble** (variant parser PyPI) + **Foiegras/GANADI** (SCA copy-based, pivotal-function clustering) + **Package Dashboard/DependaFix** thành CI check đa hệ sinh thái
- Kỹ thuật: normalize license text → variant graph; dựng reuse genealogy
- Vá bằng rule/template (tránh LLM sinh thêm rủi ro)
- Đo: incompatibility mới vs ScanCode/FOSSology, FP, lead-time fix

**Lợi ích:**
- Biết rõ "hàng tồn kho" hiện tại có gì
- Sửa chữa các vấn đề trước khi bị phát hiện bởi bên ngoài

---

### Hướng C: Chứng nhận chọn lọc (Chứng minh)

**Ý tưởng:** Tạo "giấy chứng nhận pháp lý" cho phần mềm, cho phép chứng minh các yêu cầu mà không lộ bí mật. Như tem kiểm định: khách quét QR thấy "đạt", không cần xem công thức. Luật đổi thì bài kiểm tra tự cập nhật.

**Cách hoạt động (non-tech):**
- Tạo SBOM/AIBOM tự động từ quy trình phát triển
- Sử dụng kỹ thuật Zero-Knowledge Proof để chứng minh "không chứa GPL" mà không cần khai toàn bộ danh sách
- Chuyển đổi các điều khoản hợp đồng thành bài kiểm tra tự động
- Khi luật thay đổi, hệ thống tự cập nhật tiêu chí kiểm tra

**Cách hoạt động (intech):**
- Học **Open AIBOM/SPDX** + **AIGen/AIRBORNE** sinh BOM tự động từ MLOps
- **VeriSBOM** (ZK-circuit cho predicate allow/deny-list, e.g. "không chứa GPL-3.0?")
- **Regulatory Compliance Pipeline** + **Classifier or Prompt** biến điều khoản hợp đồng thành test
- Trace Requirement→File→Commit→Issue kiểu `ADATracer`
- Đo: thời gian chuẩn bị audit, % yêu cầu có test mapping, số dòng SBOM phải mở
- Mẫu giữ IP: **IP-Preserving Modernization** (chỉ trích spec, không gửi code thô ra ngoài)

**Lợi ích:**
- Chứng minh với khách hàng mà không lộ bí mật kinh doanh
- Tự động hóa quy trình tuân thủ pháp lý
- Tạo lợi thế cạnh tranh khi khách hàng ngày càng quan tâm đến vấn đề này

---

## 6. Lộ trình và ước tính chi phí

### Giai đoạn 1: Khảo sát (2 tuần)
- Đánh giá tình trạng hiện tại: có bao nhiêu repo, dùng AI ở mức độ nào
- Xác định các rủi ro pháp lý chính
- Ước tính chi phí và nguồn lực cần thiết

**Chi phí ước tính:** 1-2 kỹ sư × 2 tuần = **2-4 người-tuần**

---

### Giai đoạn 2: Thử nghiệm (4 tuần)
- Chọn 1-2 repo để thử nghiệm Hướng A (gắn tem)
- Quét thử 1 repo lớn với Hướng B (soát kho)
- Đánh giá hiệu quả và chi phí

**Chi phí ước tính:** 2-3 kỹ sư × 4 tuần = **8-12 người-tuần**

---

### Giai đoạn 3: Triển khai (8 tuần)
- Áp dụng Hướng A cho toàn bộ đội phát triển
- Triển khai Hướng B định kỳ (hàng quý)
- Xây dựng Hướng C cho các dự án quan trọng

**Chi phí ước tính:** 3-4 kỹ sư × 8 tuần = **24-32 người-tuần**

---

### Giai đoạn 4: Vận hành (liên tục)
- Giám sát và cập nhật quy trình
- Đào tạo nhân viên
- Chứng nhận định kỳ

**Chi phí ước tính:** 0.5-1 kỹ sư × 12 tháng = **6-12 người-tuần/năm**

---

### Tổng chi phí ước tính

| Giai đoạn | Thời gian | Chi phí (người-tuần) |
|-----------|-----------|----------------------|
| Khảo sát | 2 tuần | 2-4 |
| Thử nghiệm | 4 tuần | 8-12 |
| Triển khai | 8 tuần | 24-32 |
| Vận hành | 12 tháng | 6-12 |
| **Tổng cộng** | **~6 tháng** | **40-60 người-tuần** |

*Lưu ý: Chi phí này chưa bao gồm chi phí công cụ bên ngoài (nếu cần) và chi phí đào tạo.*

### Bảng lộ trình tối thiểu (6–8 tuần, on-premise)

| Tuần | Non-tech hiểu là | Intech làm gì |
|---|---|---|
| 1–2 | Chụp hiện trạng kho | Dataset 50–100 repo nội bộ; baseline ScanCode + Foiegras/JDetect |
| 3–4 | Thử gắn tem 1 đội | Prototype Hướng A: header + commit trailer `Provenance:` cho 1 coding agent |
| 5–6 | Tổng kiểm 1 kho điểm | Prototype Hướng B: variant parser + clustering cho 1 monorepo |
| 7–8 | Demo tem kiểm định | Demo Hướng C: SPDX/AIBOM + 1 ZK-proof "có GPL-3.0 không?" Yes/No có chứng minh |

---

## 7. Kết luận

### Tóm tắt vấn đề
Khi AI tham gia viết code, chúng ta cần cách để chứng minh phần mềm "sạch" về mặt pháp lý mà không lộ bí mật kinh doanh.

### Giải pháp đề xuất
Ba hướng bổ sung nhau:
1. **Ngăn ngừa:** Gắn tem nguồn gốc từ lúc AI viết code
2. **Dọn dẹp:** Quét kho code cũ, phát hiện vấn đề tiềm ẩn
3. **Chứng minh:** Tạo giấy chứng nhận pháp lý chọn lọc bằng kỹ thuật mật mã

### Lợi ích kỳ vọng
- Giảm rủi ro pháp lý khi sử dụng AI trong phát triển
- Tăng uy tín với khách hàng và đối tác
- Tạo lợi thế cạnh tranh trong bối cảnh khách hàng ngày càng quan tâm đến tuân thủ pháp lý

### Cần quyết định
1. Phê duyệt khảo sát ban đầu (2 tuần)?
2. Phân bổ nguồn lực cho giai đoạn thử nghiệm?
3. Xác định các repo/dự án ưu tiên triển khai trước?

---

## 8. Giới hạn & câu hỏi mở

- Adversary chủ động vẫn qua được watermark/provenance — cần threat model rõ.
- Variant parsing mới tốt trên PyPI; C/C++ copy-vendor và Hugging Face model/dataset còn thiếu chuẩn.
- ZK-SBOM hay về lý thuyết, chi phí chấp nhận của doanh nghiệp cần kiểm chứng (bài học governance/disclosure từ `Demystifying CVE`, ICSE26).
- Diễn giải suy từ tiêu đề + program; muốn chắc cần fetch từng `details` researchr / DOI `10.1145/3744916.*`, `10.1145/3832783.*`, `10.1145/3803437.*`.

---

## Phụ lục: Tài liệu tham khảo chi tiết

Đối với những ai muốn tìm hiểu sâu hơn về kỹ thuật, các tài liệu sau trong repo cung cấp thông tin chi tiết:

- `icse2026/ICSE2026_Tong_hop_Van_de_Y_tuong.md` — Tổng hợp ICSE 2026
- `ase2026/ASE2026_Tong_hop_Van_de_Y_tuong.md` — Tổng hợp ASE 2026
- `fse2026/FSE2026_Tong_hop_Van_de_Y_tuong.md` — Tổng hợp FSE 2026
- `models2026/MODELS2026_Tong_hop_Van_de_Y_tuong.md` — Tổng hợp MODELS 2026
