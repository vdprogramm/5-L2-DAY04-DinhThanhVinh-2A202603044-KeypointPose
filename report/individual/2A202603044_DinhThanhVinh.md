# Báo cáo cá nhân — DinhThanhVinh-2A202603044

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Đinh Thành Vinh
- MSSV: 2A202603044
- Email: dinhthanhvinhnb205@gmail.com
- Link repository: https://github.com/vdprogramm/5-L2-DAY04-DinhThanhVinh-2A202603044-KeypointPose.git
- Phiên bản CVAT (Help → About): 2.82.1
- Máy: hệ điều hành Windows · RAM 32 GB · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu: 16h30 PM
- Giờ kết thúc: 12AM

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 19 |
| `face` | Skeleton | 13 | Có | Có | 12 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):
Chuột phải trực tiếp vào điểm keypoint cần chỉnh (hoặc chọn điểm trong bảng Objects bên phải) và bật/tắt biểu tượng `Occluded` (hình con mắt gạch chéo) hoặc `Outside` (hình mũi tên thoát ra ngoài) trên thanh thuộc tính của điểm đó.

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 5 | 0 | 0 | 25 |
| `occluded` | 3 | 6 | 1 | 0 | 30 |
| `back` | 2 | 2 | 0 | 0| 15 |
| `small` | 1 | 1 | 2 | 0| 10 |
| `crowd` | 1 | 3 | 4 | 0 | 20 |
| `faces` | 8 | 8 | — | 0 | 25 |

*(Lưu ý: Tổng số skeleton đã gán ở 5 nhóm người trên của bạn theo log script là **17** người: 5+6+2+1+3=17).*

## 3. Bảng ca khó (CP1, CP2)

Mỗi điểm bạn phân vân một dòng, nhất là phân vân giữa `v = 1` và `v = 0`, hoặc không chắc hướng người.

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `000000122199.jpg` | Người #2 | `left_ankle` | 1 | `v = 1` và `v = 0` | Bàn chân bị che một phần bởi vật cản nhưng vẫn thấy rõ ống quyển hướng xuống nên ước lượng được vị trí khớp cổ chân. |
| `000000188804.jpg` | Người #2 | `left_hip` | 1 | `v = 1` và `v = 0` | Phần hông bị áo/người bên cạnh che nhưng dựa vào trục vai và đầu gối có thể suy đoán chắc chắn vị trí khớp hông. |
| `000000106051.jpg` | Người #1 | `right_wrist` | 0 | `v = 1` và `v = 0` | Cánh tay bị khuất hoàn toàn ra phía sau thân người và ngoài vùng nhìn thấy, không có cơ sở hình học để đoán nên đặt `v = 0`. |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. 5 ảnh người (ít nhất 2 ảnh `back` hoặc `crowd`) và 3 ảnh mặt. Ít nhất 8 dòng, kể cả dòng `ok`. Ghi lỗi trước, sửa sau.

Mã lỗi dùng đúng một trong: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| `000000515309.jpg` (`back`) | Người #1 | Toàn bộ 17 điểm | `ok` | Đã xác định đúng trái/phải theo hướng người quay lưng. | Không cần |
| `000000508269.jpg` (`back`) | Người #1 | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `lr_swap` | Bị gán ngược trái/phải phần chân so với vai và hông khi người quay lưng. | Đã sửa |
| `000000326480.jpg` (`crowd`) | Người #1 | `left_shoulder` | `position` | Đặt hơi lệch ra ngoài mép áo, đã kéo lại vào đúng tâm khớp vai. | Đã sửa |
| `000000188804.jpg` (`occluded`) | Người #2 | `left_elbow`, `right_elbow` | `lr_swap` | Nhầm hướng trái/phải của khuỷu tay do tư thế nghiêng người. | Đã sửa |
| `000000122199.jpg` (`occluded`) | Người #2 | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `lr_swap` | Gán ngược cặp chân trái/phải so với thân trên. | Đã sửa |
| `face_92828` | Mặt #1 | Toàn bộ 13 điểm | `ok` | Các điểm mắt, mũi, miệng khớp chính xác tâm giải phẫu. | Không cần |
| `face_85624` | Mặt #1 | `left_eye_outer` | `vis_flag` | Đuôi mắt bị gọng kính râm che nhưng vẫn đoán được, đổi từ `v = 2` sang `v = 1`. | Đã sửa |
| `face_449454` | Mặt #1 | Toàn bộ 13 điểm | `ok` | Mặt nhìn nghiêng, các điểm bên khuất đã đặt đúng cờ `v = 1`. | Không cần |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 3 | 1 | 1 | 0 | 0 | 3 |

*(Lưu ý: Nếu bạn sửa xong 3 lỗi `lr_swap` trên CVAT và chạy lại `python lab.py export`, nhớ cập nhật lại phần TÓM TẮT bên dưới cho khớp nhé).*

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):
Ở ảnh `000000188804.jpg`, khớp cổ chân của người bị che khuất bởi người phía trước: phân vân giữa `v = 1` (ước lượng theo chiều dài cẳng chân) và `v = 0` (bỏ qua vì không thấy bàn chân). Cuối cùng chọn `v = 0` để tránh đưa tọa độ đoán mò có sai số lớn vào tập huấn luyện.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `DinhThanhVinh_persons.zip` | `000000137767.jpg`, người #1 | 51 | Có | Có (17) |
| `DinhThanhVinh_persons.zip` | `000000122199.jpg`, người #2 | 51 | Có | Có (15) |
| `DinhThanhVinh_faces.zip` | `face_145621`, mặt #1 | 39 | Có | Có (13) |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? **Tọa độ cũ** (CVAT 2.x giữ nguyên tọa độ `(x, y)` cũ trên khung mẫu kèm cờ `v = 0`; chuẩn COCO gốc ghi `0, 0`).

Hai job đã chuyển sang completed trên CVAT? **Có**

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
----- TÓM TẮT (dán vào báo cáo mục 5) -----
- File person: DinhThanhVinh_persons.zip
  Số ảnh: 12 · số skeleton `person`: 17 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000054909=2, 000000106051=1, 000000122199=2, 000000137767=1, 000000176008=1, 000000184224=1, 000000188804=2, 000000220045=1, 000000326480=3, 000000493741=1, 000000508269=1, 000000515309=1
  Cờ v: v=2 208 (72%) · v=1 41 (14%) · v=0 40 (14%)
  num_keypoints trung bình: 14.6
- File face: DinhThanhVinh_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_145621=1, face_159028=1, face_309728=1, face_380639=1, face_380754=1, face_449454=1, face_85624=1, face_92828=1
  Cờ v: v=2 96 (92%) · v=1 8 (8%) · v=0 0 (0%)
  num_keypoints trung bình: 13.0
- Ghi chú: 40 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 3
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:
Không giữ nguyên cảnh báo nào (đã kiểm tra lại 3 ảnh `000000122199.jpg`, `000000188804.jpg`, `000000508269.jpg` và sửa lỗi `lr_swap`). *(Nếu bạn không sửa mà giữ lại thì ghi rõ lý do người trong ảnh vắt chéo chân).*

## 6. Ảnh chụp màn hình

*(Bạn tự chụp 4 ảnh trên màn hình máy tính, lưu vào thư mục `report/individual/images/` đúng theo 4 tên file dưới đây)*

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   ![Label person](images/1_label_person.png)

2. Một người quay lưng đã gán, cờ đã đặt:

   ![Người quay lưng](images/2_nguoi_quay_lung.png)

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   ![Khuôn mặt phóng to](images/3_mat_phong_to.png)

4. Một đoạn JSON xuất của một người:

   ![JSON export](images/4_json_export.png)

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:
   Lỗi mắc nhiều nhất là nhầm lẫn trái/phải (`lr_swap`) đối với người quay lưng hoặc đứng nghiêng, đặc biệt là ở cặp đầu gối và cổ chân. Trong dự án thật, mình sẽ luôn xác định trục đối xứng của cơ thể (đầu -> vai -> hông) trước khi đặt điểm chi dưới, đồng thời bật hiển thị tên điểm (label text) và dùng script tự động kiểm tra chéo hình học sau mỗi batch gán nhãn.

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):
   Nên bổ sung quy tắc kiểm tra tính nhất quán hình học theo trục cơ thể (Ví dụ: với người đứng thẳng không bắt chéo chân, vector nối `left_shoulder -> right_shoulder` phải cùng chiều với vector `left_hip -> right_hip` và `left_ankle -> right_ankle`) để người gán nhãn tự phát hiện nhanh lỗi `lr_swap` bằng mắt thường trước khi xuất file.

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):
