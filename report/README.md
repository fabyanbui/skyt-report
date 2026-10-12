# Báo cáo SCCG — Source Code Compliance Gate

Bản báo cáo này được tổ chức theo cấu trúc của [VDT2026_MiniProject_report](https://github.com/fabyanbui/VDT2026_MiniProject_report): trang bìa, lời mở đầu, mục lục, các chương giới thiệu/nội dung/kết quả-kế hoạch đánh giá/kết luận và tài liệu tham khảo.

## Biên dịch

Yêu cầu LaTeX có hỗ trợ tiếng Việt và Biber. Từ thư mục `report/`, chạy:

```bash
latexmk -pdf -pdflatex="xelatex %O %S" -use-biber main.tex
```

Hoặc chạy lần lượt XeLaTeX, Biber, XeLaTeX hai lần trên `main.tex`.

## Trạng thái nội dung

Đây là **báo cáo đề xuất và thiết kế pilot**, không phải báo cáo kết quả triển khai hoàn tất. Những mục chưa có số liệu thực nghiệm được ghi là kế hoạch/tiêu chí cần kiểm chứng; không được hiểu là kết quả đã đạt. Trước khi dùng làm hồ sơ chính thức, cần cập nhật người hướng dẫn, đơn vị, ngày nộp, phạm vi được phép thử nghiệm và kết quả đo thực tế.
