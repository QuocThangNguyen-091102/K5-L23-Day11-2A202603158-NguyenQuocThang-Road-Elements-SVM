# Phiếu QC 1 — Road v1.0 (tự kiểm lô của chính mình)

> Phiếu này là **tự kiểm**: tôi không tìm được bạn để đổi lô review chéo, nên kiểm chính lô của mình theo đúng mẫu và luật của Phiếu QC 1. Người kiểm và người được kiểm là cùng một người, nên kết quả không có tính độc lập như review chéo.

Lô được kiểm (học viên): NguyenQuocThang   2A202603158     Người kiểm (học viên): NguyenQuangThuc (tự kiểm)  
Ngày: 20/11/2026                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `nguyenquocthang-front.zip`, `nguyenquocthang-fisheye.zip` (export từ CVAT 2.82.0), revision 1  
Số ảnh kiểm: 6 (A01, A04, A07, B01, B05, B08) (+ 0 ảnh thêm)   Số nhãn đã kiểm (shape + tag): 44  

Cách kiểm:
- Mỗi ảnh mở overlay nhãn trên ảnh gốc, phóng to từng polyline / box, đối chiếu guideline Road v1.0 và kế hoạch ảnh.
- Đo vị trí vệt sơn theo từng hàng pixel để kiểm độ lệch ≤ 5 px, màu, liền/đứt.
- Chạy `road_check lint`.
- Mẫu gồm 3 ảnh front: nhiều vạch (A01), vàng đôi + direct/alternative (A04), đèn + vỉa (A07). Thêm 3 ảnh fisheye: front (B01), camera trái (B05), camera phải (B08).

Số nhãn từng ảnh (shape + tag):

| Ảnh | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A01 | 4 | 4 | 8 |
| A04 | 5 | 4 | 9 |
| A07 | 5 | 4 | 9 |
| B01 | 1 | 4 | 5 |
| B05 | 2 | 4 | 6 |
| B08 | 3 | 4 | 7 |
| **Tổng** | **20** | **24** | **44** |

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 24 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 8 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 10 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 10 | 0 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 10 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 10 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 8 | 0 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 2 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 2 | 0 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 0 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-12 file | lỗi file | 44 | 0 | nhẹ / nặng |

Cách đếm "Số mục kiểm":
- **QC1-01:** 4 tag × 6 ảnh.
- **QC1-02:** vị trí L1 và R1 ở A01, A04, A07 và B01 (fisheye_front). B05, B08 là camera hai bên, không dùng L1/R1.
- **QC1-03 → QC1-06:** 10 polyline lane (A01: 4, A04: 3, A07: 2, B08: 1).
- **QC1-07:** drivable + crosswalk + road_curb (A04: 2, A07: 1, B01: 1, B05: 2, B08: 2).
- **QC1-08:** đèn (A07: 2).
- **QC1-09:** đèn + biển.
- **QC1-10:** biển. Không ảnh nào trong mẫu có biển trong kế hoạch.
- **QC1-11:** mọi hình trên 3 ảnh fisheye.
- **QC1-12:** mọi nhãn.

## Danh sách lỗi (mỗi lỗi một dòng)
Không còn lỗi trong revision 1 ở 6 ảnh mẫu.

Lỗi tìm thấy trong lúc tự kiểm và **đã sửa trước khi tạo revision 1** (ghi lại để truy vết):

| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-03 | A07 | lane L1 → road_curb | nặng | Polyline "L1" nằm trên dải sáng mảnh (2–8 px) ở chân dải phân cách bê tông, không phải vệt sơn. Đã đổi thành `road_curb` |
| 2 | QC1-04 | A04 | lane L1 (vàng đôi) | chính | Đầu xa dừng sớm khoảng 25 px (ở y=345, vệt sơn còn tới y≈335). Đã kéo tới (546,335) |
| 3 | QC1-07 | A01 | drivable × 3 (direct + 2 alternative) | chính | Thừa: kế hoạch A01 ("3+ vạch làn") không yêu cầu vùng lái. Đã gỡ |
| 4 | QC1-10 | A01 | traffic_sign × 2 | chính | Thừa: kế hoạch A01 không yêu cầu biển. Đã gỡ |
| 5 | QC1-08 | A04 | traffic_light × 2 | chính | Thừa: kế hoạch A04 ("vàng đôi + direct/alternative") không yêu cầu đèn. Đã gỡ |
| 6 | QC1-10 | A04 | traffic_sign × 3 | chính | Thừa: kế hoạch A04 không yêu cầu biển. Đã gỡ |
| 7 | QC1-07 | A07 | drivable × 2 | chính | Thừa: kế hoạch A07 ("đèn giao thông") không yêu cầu vùng lái. Đã gỡ |
| 8 | QC1-10 | A07 | traffic_sign × 3 | chính | Thừa: kế hoạch A07 không yêu cầu biển. Đã gỡ |

## Ảnh thiếu L1/R1 vì không thấy vạch (miễn QC1-02, chờ coach xác nhận)
`road_check lint` báo QC1-02 ở các ảnh dưới đây. Ở các ảnh này, phía đó thật sự không có vạch sơn nên tôi không vẽ, theo hướng dẫn của SUBMISSION và CHECKPOINTS.

| Ảnh | Thiếu | Lý do |
| --- | --- | --- |
| A05 | L1, R1 | Đường phố không kẻ vạch; hai bên là dãy xe đỗ. Đã gán `road_curb` và drivable theo kế hoạch "vùng lái + mép lề" |
| A06 | R1 | Bên phải làn là hàng cột đường sắt trên cao, không có vệt sơn |
| A07 | L1 | Bên trái là dải phân cách bê tông, không có vạch sơn mép đường; đã gán `road_curb` |
| A08 | L1 | Bên trái làn đang đi là vùng mặt đường rộng, không có vạch dọc |
| A09 | R1 | Mép phải là mép đường không sơn; đã gán `road_curb` |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A01–A16 | Bảng kế hoạch ảnh (slide Task 1 của deck) không có trong repo. Tôi lấy cột trọng tâm của từng slot (`select_bdd_road.py`, bảng SLOTS) làm kế hoạch: lane + road_curb ở mọi ảnh, các nhóm khác chỉ khi trọng tâm có. Cách hiểu này có đúng không? | chưa có |
| A07 | Vạch sát dải phân cách bê tông bên trái: vệt sơn (lane L1) hay chân vỉa (road_curb)? Tôi chọn road_curb nên lint báo QC1-02 | chưa có |
| B01 | Hai vạch trắng song song bên phải xe là vạch làn xe hay vạch làn xe đạp? Tôi không vẽ R1. Nếu là vạch làn xe thì thiếu R1 (QC1-02, nặng) | chưa có |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 0 / 44 nhãn = 0 %   Lỗi nhẹ: 0   Ca chờ lead: 3  
Quyết định (Release / Hold / Rework): **Hold**  
Lý do (theo luật của phiếu):
- Không có lỗi nặng, và tỉ lệ lỗi chính 0 % ≤ 5 %, nên nếu chỉ xét lỗi thì đạt Release.
- Tuy vậy còn ca mơ hồ chưa có lời đáp có thể đổi kết luận. Ở A07 và B01, nếu lead xác định vạch thấy được thì đó là thiếu L1/R1, tức lỗi nặng, và quyết định sẽ thành Rework. Cách hiểu kế hoạch ảnh cũng quyết định nhãn nào là thừa.
- Theo luật: "còn câu hỏi chưa có lời đáp ảnh hưởng tới kết luận → Hold".

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](../docs/qc_severity_road_v1.0.md).
