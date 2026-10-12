# Xác định vấn đề: Kiểm tra việc sử dụng mã nguồn mở và mã do AI hỗ trợ

> **Liên quan:** [Đề xuất sáng kiến SCCG](./vdt-source-code-compliance-gate.md)

## 1. Góc nhìn và bối cảnh

Khi xây dựng pipeline AI OCR, tôi đã thử nghiệm nhiều mô hình, repository và công cụ mã nguồn mở để so sánh chất lượng, tốc độ và khả năng tích hợp. Tôi nhận ra mình tập trung nhiều vào khía cạnh kỹ thuật và có thể chưa kiểm tra đầy đủ giấy phép cũng như điều kiện sử dụng của một số thành phần.

Từ trải nghiệm cá nhân đó, tôi đặt câu hỏi rộng hơn: trong quy trình phát triển phần mềm, việc kiểm tra giấy phép và nguồn gốc của các thành phần OSS, cũng như mã được tạo hoặc chỉnh sửa bằng AI, đã được thực hiện ở bước nào và có nhất quán hay không?

Đây là **quan sát và giả thuyết ban đầu của cá nhân tôi**, không phải kết luận rằng Viettel hiện chưa có quy trình kiểm soát, hoặc một thành phần cụ thể nào đã vi phạm. Cần trao đổi với các nhóm liên quan để xác minh thực trạng trước khi xác định phạm vi giải pháp.

## 2. Vấn đề cần tìm hiểu

Khi lựa chọn thư viện, mô hình hoặc công cụ OSS, người phát triển thường cần đánh giá tính năng, độ chính xác, hiệu năng và khả năng tích hợp. Tuy nhiên, các thông tin về giấy phép, điều kiện sử dụng, nguồn gốc, bảo mật và yêu cầu của dự án có thể nằm ở nhiều nơi khác nhau.

Tương tự, khi dùng AI để sinh hoặc chỉnh sửa mã, cần xác định liệu quy trình review hiện tại đã bao quát các rủi ro liên quan hay chưa. Nếu thông tin và bước kiểm tra chưa rõ ràng hoặc chưa thống nhất, nhóm phát triển có thể phát hiện vấn đề muộn, phải sửa đổi thành phần đã tích hợp hoặc mất thời gian kiểm tra lại.

**Vấn đề giả định:** Làm thế nào để giúp nhóm phát triển nhận biết sớm những trường hợp cần kiểm tra giấy phép, nguồn gốc và chính sách sử dụng OSS/AI, đồng thời lưu lại kết quả xử lý mà không tạo thêm nhiều thao tác thủ công?

## 3. Một số điểm cần xác minh

- **Giấy phép và điều kiện sử dụng:** Có xác định được giấy phép của thư viện, mã nguồn mô hình, trọng số mô hình, dataset và tài nguyên đi kèm trước khi đưa vào sản phẩm không? Các thông tin này có nguồn bằng chứng rõ ràng không?
- **Mã được AI hỗ trợ:** Quy trình review hiện tại có hướng dẫn kiểm tra dependency mới, nguồn gốc khi có thể xác định, và các yêu cầu về bảo mật hoặc chính sách không?
- **Dependency gián tiếp:** Nhóm có theo dõi được các thư viện phụ thuộc mà một package hoặc công cụ kéo theo không?
- **Bằng chứng kiểm tra:** Có lưu được thành phần/phiên bản đã dùng, kết quả kiểm tra, người quyết định và lý do ngoại lệ không?
- **Thời điểm kiểm tra:** Các vấn đề thường được phát hiện khi thử nghiệm, tạo pull request, trước khi release hay sau khi triển khai?
- **Mức độ phù hợp với dự án:** Yêu cầu có khác nhau giữa thử nghiệm nội bộ, triển khai production và bàn giao cho khách hàng không?

Các điểm trên là câu hỏi khảo sát, không phải khẳng định rằng mọi dự án đều gặp các vấn đề này.

## 4. Hướng cải thiện có thể xem xét

Nếu quá trình khảo sát xác nhận có khoảng trống, nên bắt đầu bằng một MVP hẹp: nhập URL repository/package/model, thu thập phiên bản, giấy phép, dependency và nguồn bằng chứng có thể truy xuất; sau đó xuất báo cáo nêu rõ kết quả, lý do, liên kết nguồn, ngày kiểm tra và hướng xử lý. Với mô hình AI, cần ghi nhận riêng mã nguồn, trọng số, dataset hoặc tài nguyên liên quan khi có thông tin; thiếu bằng chứng phải được đánh dấu chưa rõ, không tự suy đoán.

Trước khi tích hợp CI/CD, nên đánh giá công cụ trên một tập mẫu nhỏ đã được người có chuyên môn rà soát, so sánh độ bao phủ, cảnh báo sai/bỏ sót và thời gian xử lý với cách làm thủ công. Nếu kết quả hữu ích và quy trình hiện tại phù hợp, mới thử tích hợp vào một repository ở chế độ cảnh báo. Chỉ xem xét chặn merge khi quy tắc đã được thống nhất, có độ tin cậy phù hợp và có quy trình ngoại lệ. Công cụ không thay thế quyết định pháp lý hay chính sách nội bộ.

## 5. Liên hệ với tiêu chí đánh giá sáng kiến VDT

- **Hiệu quả kinh tế:** Cần đo thời gian kiểm tra thủ công, công sức xử lý lại khi phát hiện muộn và chi phí vận hành công cụ. Chưa nên đưa ra tỷ lệ tiết kiệm khi chưa có số liệu nền.
- **Tính mới:** Cần khảo sát các công cụ và quy trình Viettel đang sử dụng để xác định điểm khác biệt thực sự. Không mặc định rằng giải pháp hoặc công nghệ này chưa tồn tại.
- **Tính ứng dụng:** Có thể bắt đầu bằng một thử nghiệm nhỏ trong CI/CD nếu phù hợp với công nghệ, hạ tầng và chính sách hiện có.
- **Tính hoàn thiện:** Cần có bản thử nghiệm chạy được, kết quả kiểm tra trên repository mẫu, đánh giá độ chính xác/cảnh báo sai, hướng dẫn sử dụng và kết quả đo trước khi đề xuất mở rộng.

## 6. Kết luận

Từ một trải nghiệm khi phát triển AI OCR, tôi muốn kiểm tra xem quy trình sử dụng OSS và mã được AI hỗ trợ có cần một bước kiểm tra rõ ràng, sớm và có thể truy vết hơn hay không. Bước tiếp theo là xác minh nhu cầu với các nhóm kỹ thuật, DevSecOps và pháp chế/tuân thủ; sau đó mới chốt yêu cầu, phạm vi thử nghiệm và cách đo hiệu quả.


## 7. Các điểm cần làm rõ khi kiểm chứng

Ngoài việc xác định có tìm được metadata hay không, cần đánh giá **chất lượng của bằng chứng**. Một thành phần có trường license không đồng nghĩa thông tin đó đã được xác minh hoặc đủ để đưa ra quyết định. Bản thử nghiệm nên phân biệt tối thiểu: có bằng chứng phù hợp; bằng chứng thiếu hoặc mâu thuẫn; và có dấu hiệu không phù hợp với chính sách đã được phê duyệt.

Cũng cần phân biệt rủi ro của một thành phần với mức độ chắc chắn của kết quả kiểm tra. Chẳng hạn, chưa tìm thấy giấy phép không tự động chứng minh thành phần vi phạm; trường hợp này nên được chuyển sang trạng thái cần xem xét. Với dependency gián tiếp, cần chỉ ra đường dẫn phụ thuộc dẫn đến thành phần cần kiểm tra để người phát triển có thể xác định phạm vi ảnh hưởng.

Các nội dung trên là giả thuyết thiết kế cần được thử nghiệm trên tập mẫu, không phải kết luận về chất lượng của công cụ hay quy trình hiện có tại Viettel.
