# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602156 (K04)
- Ngày / CVAT local: 17/09/2026 / CVAT local (http://localhost:8080)
- Công cụ đã dùng: Brush, Polygon, AI gợi ý tự động (Interactive / SAM)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe `car` ở sát mép bên trái ảnh (nửa dưới khung hình).
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Tôi dùng Brush/Polygon vẽ bao quanh các bộ phận nhìn thấy của thân xe; ở mép trái nơi xe bị cắt bởi khung hình thì dừng đúng mép ảnh; không tự ý vẽ bù phần xe bị khuất ra ngoài ảnh hoặc sau vật che.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Khi dùng AI gợi ý, mô hình thường tự động lấy cả phần bóng đen đổ dưới gầm xe trên mặt đường gộp vào mask xe; tôi đã dùng Brush xóa bỏ phần bóng mặt đường, chỉ giữ lại bánh xe và thân xe thật.
- Nếu không dùng gợi ý: không dùng.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`.
- Lỗi thuộc loại: sai lớp / thừa vật (xuất hiện category ngoài classes.json).
- Bằng chứng tôi nhìn thấy: Khi chạy script kiểm tra `python3 scripts/inspect_submissions.py --dir submissions`, hệ thống báo lỗi `[LỖI] medium_instance: category ngoài classes.json: building, road, sidewalk, sky, traffic light, vegetation`.
- Quy tắc và hành động sửa: Quy tắc của bài toán Instance Segmentation chỉ gán nhãn cho các vật thể đếm được (thing: `car`, `person`, `bus`, `truck`, `motorcycle`, `bicycle`), không gán nhãn cho các lớp vùng nền/stuff (`road`, `sidewalk`, `building`, `sky`, `vegetation`). Tôi đã mở lại CVAT ở Job của `medium_instance`, xóa các mask thuộc các lớp nền thừa, bấm Save và export lại định dạng `COCO 1.0` vào `submissions/medium_instance.zip`.
- Sau sửa đã Save và export lại chưa? Đã Save và export lại; chạy lại script kiểm tra `scripts/inspect_submissions.py` đã báo `[OK]` đạt chuẩn hợp lệ.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa: Đã tự chạy script `inspect_submissions.py`, tất cả 9/9 task (3 tier chính và 6 checkpoint) đều đạt trạng thái `[OK]`. Trạng thái `medium_instance` đã chuyển từ `[LỖI]` sang `[OK]` với 52 annotations hợp lệ / chưa có điểm chính thức từ coach. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. Easy semantic (`7ee6d192-89e2408b.jpg`), khu vực giáp ranh giữa lòng đường và vỉa hè | Road hay Sidewalk ở đoạn gờ bó vỉa chuyển tiếp màu xám gần giống mặt đường | Dựa vào chức năng và cao độ gờ bó vỉa (curb): phần gờ nhô cao phục vụ người đi bộ, phần mặt phẳng bên dưới là lòng đường xe chạy | Phân định phần gờ cao thuộc sidewalk, phần mặt đường phẳng thuộc road |
| 2. Medium instance (`000000181542.jpg`), người đi bộ (`person`) ở tiền cảnh bị chân xe/vật thể che ngang | Tách thành 2 instance người rời rạc hay giữ chung 1 instance | Quy tắc phân đoạn Instance: một thực thể dù bị che khuất chia cắt thành nhiều mảng thị giác rời nhau vẫn là 1 đối tượng duy nhất | Gán các phần nhìn thấy của cùng một người thành 1 instance `person` duy nhất |
| 3. Hard panoptic (`000000460147.jpg`), các cành cây/tán lá phủ đè phía trước mặt đứng tòa nhà | Gộp chung vào `building` cho liền mạch hay phân tách riêng `vegetation` | Quy tắc Panoptic segmentation: tách biệt rõ các lớp stuff khác nhau dựa trên phần nhìn thấy | Dùng Brush nhỏ phóng to viền đúng các tán lá thuộc `vegetation`, phần tường lộ ra phía sau thuộc `building` |
