# Đề xuất sáng kiến VDT: Kiểm tra tuân thủ OSS trong quy trình phát triển phần mềm

> **Lĩnh vực:** Software Engineering / DevSecOps  
> **Trạng thái:** Ý tưởng đề xuất — cần khảo sát và thử nghiệm  
> **Người đề xuất:** Bùi Đình Bảo  
> **Tên làm việc:** SCCG (Source Code Compliance Gate)  
> **Tài liệu xác định vấn đề:** [Kiểm tra việc sử dụng mã nguồn mở và mã do AI hỗ trợ](./problem-definition-source-code-compliance.md)

## 1. Tóm tắt ý tưởng

Trong quá trình phát triển AI OCR, tôi nhận thấy mình có thể tập trung vào chất lượng và khả năng tích hợp của mô hình OSS mà chưa kiểm tra đầy đủ điều kiện sử dụng. Từ trải nghiệm này, tôi muốn khảo sát khả năng bổ sung một bước kiểm tra trong quy trình phát triển phần mềm.

Ý tưởng SCCG là giúp nhóm phát triển kiểm tra thông tin tuân thủ của thành phần OSS trước khi tích hợp vào dự án. MVP nên bắt đầu bằng một luồng hẹp: nhập repository/package/model cần đánh giá, thu thập bằng chứng về tên, phiên bản, giấy phép, nguồn tham khảo và dependency trong phạm vi hỗ trợ, rồi đối chiếu với chính sách do đơn vị có thẩm quyền xác định. Kết quả gồm **đạt / cần xem xét / không đạt theo quy tắc đã thống nhất**, kèm lý do, nguồn bằng chứng, ngày kiểm tra và hướng xử lý.

Chưa nên mặc định xây ngay một nền tảng CI/CD hoàn chỉnh. Trước tiên cần kiểm chứng trên một tập mẫu nhỏ có kết quả được rà soát thủ công; nếu có giá trị và phù hợp với quy trình hiện tại, mới tích hợp vào PR/CI ở chế độ cảnh báo. Kiểm tra mã do AI hỗ trợ và mở rộng sang nhiều loại tài nguyên chỉ thực hiện nếu khảo sát cho thấy cần thiết và khả thi.

Đây là đề xuất cần được kiểm chứng, không phải kết luận rằng Viettel chưa có công cụ tương tự hoặc đang có vi phạm. SCCG không thay thế tư vấn pháp lý và không tự đưa ra kết luận vi phạm bản quyền.

## 2. Vấn đề và giả thuyết

Thông tin về giấy phép, phiên bản, nguồn gốc và lỗ hổng của một thành phần có thể nằm ở repository, package registry, model card, trang phát hành trọng số hoặc hệ thống kiểm tra khác nhau. Với mô hình AI, giấy phép của mã nguồn, trọng số, dataset và tài nguyên đi kèm có thể khác nhau; không nên mặc định rằng một giấy phép ở trang chính bao quát tất cả. Nếu quy trình hiện tại chưa tập hợp bằng chứng này tại thời điểm phù hợp, nhóm phát triển có thể mất thời gian tra cứu thủ công hoặc phải xử lý lại khi vấn đề được phát hiện muộn.

**Giả thuyết cần xác minh:** Một bước kiểm tra tự động, có thể cấu hình và tích hợp vào PR/CI, sẽ giúp phát hiện sớm các trường hợp cần xem xét, giảm thao tác lặp lại và lưu lại bằng chứng xử lý.

Trước khi triển khai, cần xác nhận với nhóm kỹ thuật, DevSecOps và pháp chế/tuân thủ về quy trình hiện hành, công cụ đã có, loại rủi ro cần ưu tiên và đơn vị có quyền phê duyệt chính sách.

## 3. Giải pháp đề xuất

### 3.1. Luồng hoạt động dự kiến

**Bước kiểm chứng ban đầu (trước CI/CD):**

1. Người thử nghiệm nhập URL repository/package/model hoặc danh sách thành phần cần đánh giá.
2. Công cụ thu thập thông tin có thể truy xuất: tên, phiên bản/commit, giấy phép được công bố, dependency nếu có, nguồn bằng chứng và thời điểm kiểm tra.
3. Với mô hình AI, ghi nhận riêng các thành phần có thể có giấy phép/điều kiện riêng như mã nguồn, trọng số, dataset và tài nguyên đi kèm; nếu không xác minh được thì đánh dấu chưa rõ, không suy đoán.
4. Đối chiếu với chính sách đã được đơn vị có thẩm quyền cung cấp và tạo báo cáo có lý do, liên kết nguồn, mức độ đầy đủ của bằng chứng và hướng xử lý.
5. So sánh kết quả với đánh giá thủ công của người rà soát để xác định mức bao phủ và các loại cảnh báo sai.

**Bước tích hợp sau khi kiểm chứng:**

1. Khi tạo hoặc cập nhật PR, CI thu thập manifest và lockfile của dự án.
2. Công cụ lập danh sách dependency trực tiếp và gián tiếp trong phạm vi hỗ trợ; thu thập thông tin giấy phép và nguồn tham khảo có sẵn.
3. Đối chiếu với chính sách do đơn vị có thẩm quyền cung cấp:
   - **Đạt:** có đủ bằng chứng theo quy tắc đã cấu hình.
   - **Cần xem xét:** thiếu hoặc mâu thuẫn thông tin, cần người phụ trách đánh giá.
   - **Không đạt:** phát hiện điều kiện đã được chính sách xác định rõ là không được chấp nhận.
4. Lưu báo cáo cùng PR; ghi nhận người duyệt và lý do nếu có ngoại lệ.

Kết quả tự động chỉ là công cụ hỗ trợ sàng lọc. Metadata có thể sai hoặc thiếu; trường hợp mơ hồ cần được con người xem xét.

### 3.2. Phạm vi MVP

- **Đầu vào:** một repository/package/model URL hoặc một danh sách thành phần; chọn một hệ sinh thái công nghệ và phạm vi nhỏ để thử nghiệm.
- **Thu thập bằng chứng:** tên và phiên bản/commit, metadata giấy phép, URL nguồn, dependency có thể truy xuất và ngày kiểm tra. Với model, ghi nhận riêng mã nguồn, trọng số, dataset/tài nguyên liên quan khi có thông tin.
- **Đánh giá theo quy tắc:** phân loại đạt / cần xem xét / không đạt theo chính sách được phê duyệt; thiếu bằng chứng phải được đánh dấu rõ, không tự kết luận pháp lý.
- **Đầu ra:** báo cáo dễ đọc và JSON gồm thành phần, phiên bản, kết quả, lý do, nguồn bằng chứng, ngày kiểm tra và hướng xử lý.
- **Đánh giá thủ công:** xây dựng tập mẫu khoảng 20–30 thành phần nếu được phép, để người có chuyên môn xác minh kết quả; số lượng cuối cùng phụ thuộc vào khả năng tiếp cận và độ đa dạng của mẫu.
- **Tích hợp CI/CD:** chưa phải yêu cầu đầu tiên. Sau khi đánh giá độ chính xác và tính hữu ích, thử tích hợp vào một repository ở chế độ cảnh báo.
- **Tái sử dụng công cụ:** đánh giá công cụ tạo SBOM/kiểm tra giấy phép hiện có thay vì tự xây dựng mọi thành phần từ đầu.

### 3.3. Chưa thuộc phạm vi ban đầu

- Tự kết luận một hành vi có vi phạm pháp luật hoặc giấy phép hay không.
- Chứng minh nguồn gốc của mọi dòng mã được sao chép hoặc sinh bởi AI.
- Bao quát mọi loại tài nguyên mô hình, dataset, binary, container và plugin ngay trong MVP.
- Gửi mã nguồn hoặc dữ liệu nội bộ tới dịch vụ bên ngoài khi chưa được phê duyệt.

## 4. Kiến trúc mức cao

```text
Pull Request
    |
    v
CI: thu thập dependency và metadata
    |
    v
Báo cáo thành phần + thông tin giấy phép
    |
    v
Đối chiếu chính sách đã được phê duyệt
    |
    +--> Đạt ------------> Lưu báo cáo
    |
    +--> Cần xem xét ----> Người phụ trách đánh giá
    |
    +--> Không đạt ------> Sửa đổi hoặc xin ngoại lệ
                               |
                               v
                       Lưu quyết định cùng PR
```

Các công cụ như SPDX, Syft hoặc ScanCode Toolkit có thể được đánh giá trong bước thử nghiệm. Đây là ứng viên để khảo sát, chưa phải lựa chọn công nghệ cuối cùng. Việc lựa chọn cần xét độ bao phủ, độ chính xác, khả năng chạy nội bộ, chi phí vận hành và giấy phép của chính công cụ.

## 5. Cách đánh giá thử nghiệm

Cần đo số liệu nền trước khi đặt mục tiêu. Các chỉ số đề xuất:

- **Độ bao phủ bằng chứng:** tỷ lệ thành phần trong tập mẫu có thể thu thập được thông tin giấy phép và nguồn tham khảo.
- **Độ đúng so với rà soát thủ công:** tỷ lệ kết quả phân loại khớp với đánh giá của người có chuyên môn; ghi riêng trường hợp chưa thể kết luận.
- **Cảnh báo sai và bỏ sót:** số lượng/tỷ lệ false positive và false negative trên tập mẫu đã được xác minh.
- **Thời gian xử lý:** thời gian trung vị để kiểm tra thủ công so với dùng công cụ.
- **Khả năng truy vết:** tỷ lệ kết quả có phiên bản, nguồn bằng chứng, lý do và ngày kiểm tra.
- **Ảnh hưởng đến phát triển:** nếu tích hợp CI ở giai đoạn sau, đo thời gian CI tăng thêm và tác động tới PR.
- **Chi phí:** công sức triển khai, bảo trì chính sách và vận hành.

Không cam kết trước tỷ lệ tiết kiệm hoặc giảm rủi ro khi chưa có dữ liệu thực nghiệm.

## 6. Kế hoạch thử nghiệm dự kiến

| Giai đoạn | Công việc | Kết quả |
|---|---|---|
| 1. Khảo sát | Xác minh quy trình/công cụ hiện tại, chọn phạm vi và thống nhất người rà soát có chuyên môn | Khoảng trống cần giải quyết, quy tắc và số liệu nền |
| 2. Lập tập mẫu | Chọn khoảng 20–30 package/model/thành phần nếu được phép; ghi nhận kết quả rà soát thủ công và nguồn bằng chứng | Tập mẫu đối chiếu có thể kiểm tra lại |
| 3. Làm MVP | Thu thập metadata, phân loại theo quy tắc, xuất báo cáo và JSON; chưa bắt buộc tích hợp CI | Bản thử nghiệm chạy được |
| 4. Đánh giá | So sánh kết quả tự động với tập mẫu đã xác minh; đo độ bao phủ, cảnh báo sai/bỏ sót và thời gian xử lý | Báo cáo đánh giá có số liệu |
| 5. Tích hợp thử | Nếu MVP hữu ích, tích hợp một repository ở chế độ cảnh báo; chỉ cân nhắc chặn merge sau khi kiểm chứng | Kết luận thử nghiệm và đề xuất tiếp theo |

Thời lượng từng giai đoạn sẽ được chốt sau khi biết độ phức tạp repository và khả năng hỗ trợ của các bên liên quan.

## 7. Rủi ro và cách giảm thiểu

- **Metadata không chính xác hoặc thiếu:** hiển thị nguồn và mức độ chắc chắn; cho phép xác minh thủ công.
- **Cảnh báo sai gây cản trở:** bắt đầu ở chế độ cảnh báo, đo độ nhiễu trước khi bật chặn.
- **Lộ mã nguồn nội bộ:** ưu tiên công cụ chạy trong hạ tầng được phê duyệt; đánh giá luồng dữ liệu trước khi dùng dịch vụ bên ngoài.
- **Chính sách chưa rõ hoặc lỗi thời:** cần chủ sở hữu chính sách, quản lý phiên bản và quy trình cập nhật.
- **Hiểu nhầm kết quả công cụ là kết luận pháp lý:** nêu rõ giới hạn và chuyển trường hợp mơ hồ đến người có thẩm quyền.
- **Trùng lặp công cụ hiện có:** khảo sát trước khi xây dựng; nếu đã có giải pháp phù hợp, cân nhắc tích hợp hoặc cải tiến thay vì làm lại.

## 8. Đối chiếu với tiêu chí đánh giá sáng kiến VDT

### 8.1. Hiệu quả kinh tế — 30 điểm

Giá trị kinh tế có thể đến từ việc giảm thời gian tra cứu thủ công, hạn chế công sức sửa đổi khi phát hiện vấn đề muộn và tái sử dụng quy trình kiểm tra giữa các repository.

Cần thu thập số liệu nền và đo trong pilot: thời gian kiểm tra trước/sau, số cảnh báo có thể xử lý, thời gian xử lý lại và chi phí vận hành. Chưa đủ căn cứ để khẳng định mức tác động lợi nhuận hoặc xếp hạng hiệu quả kinh tế.

### 8.2. Tính mới — 20 điểm

Tính mới hiện chưa được xác nhận. Các công cụ kiểm tra license, SBOM và bảo mật chuỗi cung ứng phần mềm đã tồn tại; do đó không nên coi bản thân việc quét giấy phép là điểm mới.

Cần khảo sát công cụ và quy trình đang được Viettel áp dụng để tìm khoảng trống cụ thể. Khả năng khác biệt có thể nằm ở cách tích hợp với quy trình nội bộ, chính sách phù hợp từng loại dự án, luồng phê duyệt ngoại lệ và khả năng truy vết — nhưng chỉ có thể xác nhận sau khảo sát.

### 8.3. Tính ứng dụng — 20 điểm

MVP có thể thử nghiệm trên một repository nếu phù hợp với CI/CD, hạ tầng và chính sách hiện hành. Khả năng áp dụng ngay phụ thuộc vào quyền truy cập, hệ sinh thái công nghệ, người phụ trách chính sách và yêu cầu bảo mật. Cần xác minh các điều kiện này trước khi đánh giá mức độ sẵn sàng triển khai.

### 8.4. Tính hoàn thiện — 30 điểm

Mức hoàn thiện phải dựa trên sản phẩm và bằng chứng thực tế, không chỉ trên tài liệu thiết kế. Kết quả tối thiểu cần hướng tới gồm:

- MVP tích hợp và chạy được trên repository thử nghiệm.
- Báo cáo kiểm tra có thể truy vết.
- Bộ mẫu được rà soát thủ công để đánh giá độ bao phủ và cảnh báo sai.
- Quy trình xử lý cảnh báo, ngoại lệ và giới hạn sử dụng được mô tả.
- Báo cáo pilot có số liệu về hiệu quả, chi phí và ảnh hưởng đến quy trình phát triển.

Chỉ đánh giá phần trăm hoàn thiện sau khi có tiêu chí nghiệm thu và kết quả kiểm thử.

## 9. Quyết định cần xin ý kiến

1. Viettel hiện có công cụ hoặc quy trình tương đương nào, và khoảng trống còn lại là gì?
2. Repository và hệ sinh thái nào phù hợp cho thử nghiệm?
3. Ai có thẩm quyền xác định chính sách và duyệt ngoại lệ?
4. Công cụ có bắt buộc chạy hoàn toàn trong hạ tầng Viettel không?
5. Những số liệu nào có thể dùng để đo hiệu quả kinh tế và mức hoàn thiện?

## 10. Kiến nghị

Đề xuất thực hiện bước khảo sát ngắn trước khi cam kết xây dựng đầy đủ. Nếu xác nhận có nhu cầu chưa được đáp ứng, trước tiên xây dựng một MVP hẹp để kiểm tra repository/package/model, thu thập bằng chứng và xuất báo cáo; so sánh với một tập mẫu đã được rà soát thủ công. Chỉ sau khi có bằng chứng về độ hữu ích, độ bao phủ, cảnh báo sai và thời gian xử lý mới tích hợp thử vào một repository ở chế độ cảnh báo. Chỉ đề xuất mở rộng khi kết quả cho thấy lợi ích rõ ràng so với công cụ và quy trình hiện có.

Đề xuất này ưu tiên giải quyết một vấn đề thực tế có thể kiểm chứng; tính mới, hiệu quả kinh tế, mức độ ứng dụng và độ hoàn thiện đều cần được chứng minh bằng khảo sát và thử nghiệm.
