---
title: "CF 1133A - Giữa cuộc thi"
description: "Nhiệm vụ là tìm ra điểm giữa chính xác của cuộc thi với thời gian bắt đầu và kết thúc tính bằng giờ và phút. Đầu vào cho biết thời gian bắt đầu là h1:m1 và thời gian kết thúc là h2:m2."
date: "2026-06-12T04:03:52+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1133
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 544 (Div. 3)"
rating: 1000
weight: 1133
solve_time_s: 67
verified: true
draft: false
---

[CF 1133A - Giữa cuộc thi](https://codeforces.com/problemset/problem/1133/A) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm ra điểm giữa chính xác của cuộc thi với thời gian bắt đầu và kết thúc tính bằng giờ và phút. Đầu vào cho biết thời gian bắt đầu là`h1:m1`và thời gian kết thúc như`h2:m2`. Chúng tôi đảm bảo rằng cuộc thi kéo dài số phút chẵn và cuộc thi diễn ra trong vòng một ngày. Đầu ra phải là điểm giữa trong cùng`hh:mm`định dạng. 

Trong thực tế, điều này có nghĩa là chúng ta cần tính tổng thời lượng của cuộc thi tính bằng phút, chia cho hai và cộng vào thời gian bắt đầu. Các ràng buộc rất nhỏ: giờ nằm ​​trong khoảng từ 0 đến 23 và số phút nằm trong khoảng từ 0 đến 59. Bởi vì tất cả các phép toán đều là số học đơn giản trên các số nguyên và chúng tôi chỉ tính toán một điểm giữa duy nhất nên hiệu suất không phải là vấn đề đáng lo ngại. Một cách tiếp cận ngây thơ tính toán thời gian từng bước sẽ luôn nhanh chóng. 

Ví dụ: các trường hợp phức tạp bao gồm các cuộc thi bắt đầu hoặc kết thúc trong nửa giờ hoặc gần nửa đêm`23:58`ĐẾN`23:59`không được phép vì cuộc thi kéo dài ít nhất 2 phút, nhưng`23:58`ĐẾN`00:00`là hợp lệ nếu ranh giới ngày được xem xét (nhưng vấn đề đảm bảo một ngày duy nhất). Một cái bẫy tiềm ẩn khác là việc quên chuyển số phút sang số giờ khi điểm giữa cộng thêm 59 phút đã qua. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng cuộc thi từng phút, đếm nửa chừng, sau đó in giờ và phút tương ứng. Điều này hiệu quả vì chúng tôi được đảm bảo một khoảng thời gian cố định nhỏ (tối đa 1440 phút trong một ngày). Cách tiếp cận này đúng nhưng chậm một cách không cần thiết đối với phạm vi lớn hơn hoặc nhiều truy vấn vì nó thực hiện một vòng lặp cho mỗi phút của cuộc thi. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể biểu thị thời gian hoàn toàn bằng phút kể từ nửa đêm. Chuyển đổi thời gian bắt đầu`h1:m1`ĐẾN`start_minutes = h1 * 60 + m1`và thời gian kết thúc`h2:m2`ĐẾN`end_minutes = h2 * 60 + m2`. Trung điểm tính bằng phút là`(start_minutes + end_minutes) // 2`. Cuối cùng, chuyển đổi ngược lại thành giờ và phút bằng phép chia số nguyên và modulo. Điều này hiệu quả vì tất cả số học đều tuyến tính và chúng tôi được đảm bảo tổng số phút là số chẵn, do đó phép chia số nguyên mang lại điểm giữa chính xác mà không gặp vấn đề làm tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(thời lượng tính bằng phút) | O(1) | Đúng nhưng không cần thiết cho vấn đề này | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian bắt đầu`h1:m1`và thời gian kết thúc`h2:m2`. 
2. Chuyển đổi cả hai thời gian thành tổng số phút kể từ nửa đêm. Tính toán`start_minutes = h1 * 60 + m1`Và`end_minutes = h2 * 60 + m2`. Điều này cho phép tính toán tuyến tính mà không phải lo lắng về việc gói hàng giờ. 
3. Tính điểm giữa theo phút là`(start_minutes + end_minutes) // 2`. Phép chia số nguyên có tác dụng vì tổng thời gian của cuộc thi được đảm bảo bằng nhau. 
4. Chuyển đổi điểm giữa về giờ và phút bằng`mid_h = mid_minutes // 60`Và`mid_m = mid_minutes % 60`. 
5. In kết quả ở định dạng hai chữ số sử dụng khoảng đệm 0 cho giờ và phút để khớp với`hh:mm`yêu cầu. 

Tại sao nó hoạt động: Việc chuyển đổi sang tổng số phút đảm bảo chúng tôi tự động xử lý mọi chuyển đổi từ phút sang giờ. Phép chia số nguyên tính toán chính xác một nửa thời lượng và modulo 60 và phép chia số nguyên cho 60 sẽ tái tạo lại chính xác giờ và phút. Sự đảm bảo trong vấn đề ngăn ngừa lỗi làm tròn hoặc tràn ngày. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

h1, m1 = map(int, input().strip().split(':'))
h2, m2 = map(int, input().strip().split(':'))

start_minutes = h1 * 60 + m1
end_minutes = h2 * 60 + m2

mid_minutes = (start_minutes + end_minutes) // 2

mid_h = mid_minutes // 60
mid_m = mid_minutes % 60

print(f"{mid_h:02d}:{mid_m:02d}")
```Giải pháp đầu tiên phân tích đầu vào bằng cách sử dụng`split(':')`và chuyển đổi giờ và phút thành số nguyên. Tính tổng số phút đơn giản hóa mọi phép tính. Chia tổng số phút bắt đầu và kết thúc cho hai sẽ tính trực tiếp điểm giữa. Cuối cùng, phép chia số nguyên và modulo chuyển số phút ngược lại thành giờ và phút, với định dạng để duy trì yêu cầu hai chữ số. Sự đảm bảo của vấn đề có nghĩa là không cần kiểm tra ranh giới ngoài phần đệm bằng 0. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
10:00
11:00
```| Biến | Giá trị | 
| --- | --- | 
| h1, m1 | 10, 0 | 
| h2, m2 | 11, 0 | 
| bắt đầu_phút | 600 | 
| cuối_phút | 660 | 
| giữa_phút | 630 | 
| giữa_h | 10 | 
| giữa_m | 30 | 

Dấu vết cho thấy việc chuyển đổi sang phút sẽ đơn giản hóa việc tìm điểm giữa: 600 + 660 = 1260, chia cho 2 được 630 phút, tương ứng với 10 giờ 30 phút. 

Đầu vào tùy chỉnh 2:```
11:10
11:12
```| Biến | Giá trị | 
| --- | --- | 
| h1, m1 | 11, 10 | 
| h2, m2 | 11, 12 | 
| bắt đầu_phút | 670 | 
| cuối_phút | 672 | 
| giữa_phút | 671 | 
| giữa_h | 11 | 
| giữa_m | 11 | 

Điều này cho thấy thuật toán xử lý chính xác độ chính xác ở cấp độ phút, tạo ra`11:11`cho một cuộc thi kéo dài hai phút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Cách tiếp cận là thời gian và không gian không đổi, nằm trong giới hạn. Vì kích thước đầu vào được cố định thành hai lần nên không có vấn đề gì về hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    h1, m1 = map(int, input().strip().split(':'))
    h2, m2 = map(int, input().strip().split(':'))
    start_minutes = h1 * 60 + m1
    end_minutes = h2 * 60 + m2
    mid_minutes = (start_minutes + end_minutes) // 2
    mid_h = mid_minutes // 60
    mid_m = mid_minutes % 60
    return f"{mid_h:02d}:{mid_m:02d}"

# provided sample
assert run("10:00\n11:00\n") == "10:30", "sample 1"

# minimum duration
assert run("00:00\n00:02\n") == "00:01", "minimum duration"

# even duration with minutes overflow
assert run("23:58\n23:59\n") == "23:58", "edge minutes overflow not allowed, guaranteed >=2 minutes"

# odd/even alignment
assert run("11:10\n11:12\n") == "11:11", "2-minute contest in mid hour"

# full day split
assert run("00:00\n23:58\n") == "11:59", "halfway through full day minus two minutes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 10:00 11:00 | 10:30 | cuộc thi tiêu chuẩn 1 giờ | 
| 00:00 00:02 | 00:01 | thời lượng cuộc thi tối thiểu | 
| 11:10 11:12 | 11:11 | cuộc thi rất ngắn không đúng giờ | 
| 00:00 23:58 | 11:59 | cuộc thi kéo dài gần như cả ngày | 

## Vỏ cạnh 

Thuật toán xử lý chính xác các cuộc thi kéo dài đúng hai phút. Ví dụ, đầu vào`00:00`Và`00:02`chuyển đổi thành phút 0 và 2, điểm giữa là 1 phút, chuyển đổi trở lại thành`00:01`. Nó cũng xử lý những khoảng thời gian đòi hỏi phải kéo dài hàng phút đến hàng giờ mà không có bất kỳ trường hợp đặc biệt nào. Các đảm bảo về khoảng thời gian chẵn sẽ ngăn ngừa lỗi làm tròn và định dạng đảm bảo đầu ra luôn có hai chữ số cho giờ và phút.
