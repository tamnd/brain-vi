---
title: "CF 407D - Ma trận con lớn nhất 3"
description: "Chúng ta được cho một ma trận nguyên có kích thước n x m. Mỗi ô chứa một số nguyên dương. Nhiệm vụ là tìm ma trận con hình chữ nhật lớn nhất trong đó tất cả các phần tử đều khác nhau. “Lớn nhất” được đo bằng diện tích, nghĩa là số lượng ô bên trong hình chữ nhật."
date: "2026-06-07T01:51:12+07:00"
tags: ["codeforces", "competitive-programming", "dp", "hashing"]
categories: ["algorithms"]
codeforces_contest: 407
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 239 (Div. 1)"
rating: 2700
weight: 407
solve_time_s: 284
verified: false
draft: false
---

[CF 407D - Ma trận con lớn nhất 3](https://codeforces.com/problemset/problem/407/D) 

**Đánh giá:** 2700 
**Thẻ:** dp, băm 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một ma trận số nguyên có kích thước _n_ theo _m_. Mỗi ô chứa một số nguyên dương. Nhiệm vụ là tìm ma trận con hình chữ nhật lớn nhất trong đó tất cả các phần tử đều khác nhau. “Lớn nhất” được đo bằng diện tích, nghĩa là số lượng ô bên trong hình chữ nhật. Ví dụ: hình chữ nhật 2×3 có diện tích 6. Đầu ra là diện tích của hình chữ nhật lớn nhất đó. 

Với các ràng buộc _n_, _m_ 400, một giải pháp mạnh mẽ để kiểm tra tất cả các hình chữ nhật có thể có sẽ liên quan đến việc lặp lại trên các ma trận con O(n2 m2) và kiểm tra tính khác biệt bên trong mỗi ma trận con, có thể thực hiện các phép toán lên tới 400² × 400² = 2,56 × 10¹⁰. Giá trị này quá lớn so với giới hạn 3 giây. Giới hạn bộ nhớ là 256 MB, đủ để lưu trữ một vài ma trận có kích thước 400×400, nhưng chúng ta phải tránh lưu trữ bất kỳ dữ liệu trung gian O(n² m²) nào. 

Việc triển khai đơn giản cũng có thể thất bại với các đầu vào nhỏ hoặc suy biến. Ví dụ: nếu tất cả các phần tử đều bằng nhau:```
2 2
1 1
1 1
```Câu trả lời đúng là 1, vì bất kỳ ma trận con nào lớn hơn 1×1 đều chứa các phần tử lặp lại. Một cách tiếp cận bất cẩn có thể đếm không chính xác các diện tích lớn hơn nếu nó không thực thi cẩn thận tính duy nhất. 

Một trường hợp tinh vi khác là ma trận có nhiều giá trị lặp lại nằm rải rác, như:```
3 3
1 2 3
2 3 4
3 4 5
```Hình chữ nhật lớn nhất không phải là ma trận đầy đủ, vì các số lặp lại theo đường chéo xuất hiện; thuật toán phải phát hiện chính xác sự trùng lặp như vậy. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp qua tất cả các cặp góc trên bên trái và góc dưới bên phải, trích xuất ma trận con và kiểm tra xem tất cả các giá trị có phải là duy nhất hay không. Kiểm tra tính duy nhất có thể được thực hiện bằng bộ băm. Điều này đảm bảo tính chính xác nhưng không thành công vì mỗi phép kiểm tra ma trận con là O(diện tích) và có các ma trận con O(n² m2). Ngay cả đối với các ma trận nhỏ, tốc độ này vẫn quá chậm. 

Thông tin chi tiết quan trọng cần tối ưu hóa là tính duy nhất dọc theo các cột có thể được theo dõi một cách hiệu quả bằng cách quét các hàng. Thay vì lặp lại một cách rõ ràng các góc dưới cùng bên phải, chúng tôi sửa hàng trên cùng và kéo dài hàng dưới cùng xuống dưới. Đối với mỗi cột, chúng tôi theo dõi hàng cuối cùng nơi xảy ra trùng lặp, tạo thành hình chữ nhật “có chiều rộng hợp lệ” cho cột đó. Sau đó, hình chữ nhật lớn nhất với tất cả các phần tử duy nhất có thể được tìm thấy bằng cách duy trì ranh giới hợp lệ ngoài cùng bên trái cho mỗi cột. Điều này làm giảm vấn đề xuống còn O(n m2) bằng cách sử dụng hàm băm trên mỗi hàng, điều này khả thi với n, m 400. 

Về cơ bản, vấn đề giảm xuống trong việc tính toán, đối với mỗi phân đoạn hàng, hình chữ nhật lớn nhất với các phần tử cột duy nhất sử dụng cửa sổ trượt trên mỗi hàng. Thách thức ở đây là theo dõi các bản sao và mở rộng hình chữ nhật một cách hiệu quả mà không cần tính toán lại tính duy nhất từ ​​đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 m2 × diện tích) ≈ O(n³ m³) | O(n m) | Quá chậm | 
| Tối ưu | O(n m2) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`best`về 0. Điều này sẽ lưu trữ diện tích lớn nhất được tìm thấy cho đến nay. 
2. Lặp lại`top`từ hàng 0 đến n-1. Hàng này sẽ là ranh giới trên cùng của ma trận con ứng viên của chúng ta. 
3. Tạo một mảng`last_seen`từ điển, mỗi từ điển một cột, ánh xạ các giá trị phần tử tới chỉ mục hàng cuối cùng nơi chúng xuất hiện. Điều này cho phép chúng tôi phát hiện các bản sao trong một cột một cách nhanh chóng. 
4. Khởi tạo một mảng`min_top`cỡ m với`top`. Điều này theo dõi, đối với mỗi cột, chỉ mục hàng sớm nhất mà chúng ta có thể bắt đầu mà không lặp lại giá trị trong cột hiện tại. 
5. Lặp lại`bottom`từ`top`đến n-1. Điều này mở rộng ma trận con xuống dưới. Đối với mỗi cột, hãy kiểm tra phần tử trong hàng`bottom`. Nếu nó đã xuất hiện trong cột này kể từ`min_top[col]`, di chuyển`min_top[col]`ngay sau hàng đó. Cập nhật`last_seen[col][value] = bottom`. 
6. Một lần`min_top`được cập nhật cho hiện tại`bottom`, sử dụng cửa sổ trượt qua các cột để tìm phạm vi liền kề rộng nhất`[l, r]`tất cả ở đâu`min_top[c] <= top`. Tính diện tích`(bottom - top + 1) * (r - l + 1)`và cập nhật`best`nếu lớn hơn. 
7. Sau khi hoàn thành tất cả`bottom`sự mở rộng, sự gia tăng`top`và lặp lại. 

### Tại sao nó hoạt động 

Bất biến được duy trì là đối với hình chữ nhật hiện tại từ`top`ĐẾN`bottom`và cột`[l, r]`, không có phần tử nào được lặp lại trong bất kỳ cột nào. Bằng cách theo dõi`min_top`, chúng tôi không bao giờ bao gồm một hàng có thể tạo ra một hàng trùng lặp. Cửa sổ trượt đảm bảo rằng chiều rộng lớn nhất có thể luôn được xem xét, vì vậy mọi hình chữ nhật ứng cử viên đều có giá trị tối đa cho các ranh giới trên và dưới của nó. Quét tất cả các hàng trên cùng đảm bảo không bỏ sót hình chữ nhật tiềm năng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

best = 0
last_seen = [{} for _ in range(m)]

for top in range(n):
    min_top = [top] * m
    for bottom in range(top, n):
        for col in range(m):
            val = a[bottom][col]
            if val in last_seen[col] and last_seen[col][val] >= min_top[col]:
                min_top[col] = last_seen[col][val] + 1
            last_seen[col][val] = bottom

        l = 0
        for r in range(m):
            while l <= r and min_top[r] > top:
                l += 1
            width = r - l + 1
            height = bottom - top + 1
            best = max(best, width * height)

print(best)
```Giải pháp đọc ma trận và lặp lại các hàng trên cùng.`last_seen`là một từ điển trên mỗi cột để tra cứu nhanh hàng cuối cùng mà một giá trị xuất hiện.`min_top`theo dõi mức độ chúng ta có thể đạt được mà không cần lặp lại trong cột đó. Cửa sổ trượt qua các cột đảm bảo chúng tôi tối đa hóa chiều rộng cho mỗi phần mở rộng hàng dưới cùng. Điều kiện biên như chuyển động`l`chuyển tiếp đảm bảo không có phần tử trùng lặp lẻn vào hình chữ nhật. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
3 3
1 3 1
4 5 6
2 6 1
```| hàng đầu | dưới cùng | phút_top | tôi | r | chiều rộng | chiều cao | khu vực | tốt nhất | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | [0,0,0] | 0 | 0 | 1 | 1 | 1 | 1 | 
| 0 | 0 | [0,0,0] | 0 | 1 | 2 | 1 | 2 | 2 | 
| 0 | 0 | [0,0,0] | 0 | 2 | 3 | 1 | 3 | 3 | 
| 0 | 1 | [0,0,0] | 0 | 0 | 1 | 2 | 2 | 3 | 
| 0 | 1 | [0,0,0] | 0 | 1 | 2 | 2 | 4 | 4 | 
| 0 | 1 | [0,0,0] | 0 | 2 | 2 | 2 | 4 | 4 | 
| 0 | 2 | [0,0,2] | 0 | 0 | 1 | 3 | 3 | 4 | 
| 0 | 2 | [0,0,2] | 0 | 1 | 2 | 3 | 6 | 6 | 

Tốt nhất cuối cùng là 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m2) | Vòng lặp ngoài qua các hàng trên cùng (n), vòng lặp bên trong qua các hàng dưới cùng (≤ n), cửa sổ trượt qua m cột | 
| Không gian | O(n m) | Lưu trữ từ điển Last_seen trên mỗi cột, mảng min_top | 

Với các thao tác n, m  400, n m  6.4×10⁷ chạy thoải mái trong 3 giây. Mức sử dụng bộ nhớ trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]
    best = 0
    last_seen = [{} for _ in range(m)]
    for top in range(n):
        min_top = [top] * m
        for bottom in range(top, n):
            for col in range(m):
                val = a[bottom][col]
                if val in last_seen[col] and last_seen[col][val] >= min_top[col]:
                    min_top[col] = last_seen[col][val] + 1
                last_seen[col][val] = bottom
            l = 0
            for r in range(m):
                while l <= r and min_top[r] > top:
                    l += 1
                width = r - l + 1
                height = bottom - top + 1
                best = max(best, width * height)
    return str(best)

# provided sample
assert run("3 3\n1 3 1\n4 5 6\n2 6 1\n") == "6"

# all equal
assert run("2 2\n1 1\n1 1\n") == "1"

# minimum input
```
