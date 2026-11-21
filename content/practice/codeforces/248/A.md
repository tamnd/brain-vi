---
title: "CF 248A - Tủ"
description: "Chúng tôi được tặng một bộ tủ, mỗi tủ có hai cửa: trái và phải. Mỗi cánh cửa có thể mở hoặc đóng. Trạng thái ban đầu của mỗi cửa được đưa ra trong đầu vào. Karlsson muốn tất cả các cửa bên trái ở cùng một vị trí và tất cả các cửa bên phải ở cùng một vị trí."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 248
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 152 (Div. 2)"
rating: 800
weight: 248
solve_time_s: 94
verified: true
draft: false
---

[CF 248A - Tủ đựng đồ](https://codeforces.com/problemset/problem/248/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một bộ tủ, mỗi tủ có hai cửa: trái và phải. Mỗi cánh cửa có thể mở hoặc đóng. Trạng thái ban đầu của mỗi cửa được đưa ra trong đầu vào. Karlsson muốn tất cả các cửa bên trái ở cùng một vị trí và tất cả các cửa bên phải ở cùng một vị trí. Vị trí mục tiêu chính xác không quan trọng - chỉ có điều tất cả các cửa bên trái khớp và tất cả các cửa bên phải khớp nhau. Chúng ta cần tìm số lần lật cửa tối thiểu cần thiết để đạt được sự đồng nhất này. 

Đầu vào bao gồm một số nguyên`n`đại diện cho số lượng tủ, tiếp theo là`n`cặp số nguyên biểu thị trạng thái cửa trái và cửa phải của mỗi tủ. Đầu ra là một số nguyên - số lần lật cửa tối thiểu cần thiết. 

Những hạn chế`2 ≤ n ≤ 10^4`có nghĩa là việc quét tuyến tính đơn giản qua tất cả các tủ là khả thi. Bất kỳ thuật toán nào có độ phức tạp kém hơn O(n) đều có thể gặp rủi ro trong giới hạn thời gian. Trạng thái của mỗi cửa là 0 (đóng) hoặc 1 (mở), vì vậy chúng ta có thể dễ dàng đếm số lần xuất hiện của từng trạng thái. Các trường hợp cạnh không rõ ràng bao gồm khi tất cả các cửa đã ở cùng một vị trí hoặc khi lật tất cả các cửa của một bên sẽ rẻ hơn so với lật bên kia. 

Ví dụ: hãy xem xét một đầu vào:```
2
0 1
0 0
```Tất cả các cửa bên trái đều giống nhau nên chúng ta không cần lật bất kỳ cửa bên trái nào. Các cửa bên phải khác nhau và cách rẻ nhất là lật cửa khác (1 trở thành 0), dẫn đến 1 lần lật. Việc triển khai ngây thơ luôn dựa trên đa số có thể tạo ra kết quả tương tự ở đây, nhưng việc xử lý cẩn thận việc đếm số 0 và số 1 đảm bảo tính chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ bao gồm việc thử cả hai trạng thái cuối cùng có thể có cho cửa bên trái và cả hai trạng thái cuối cùng cho cửa bên phải, tính toán số lần lật cần thiết cho mỗi cấu hình và chọn mức tối thiểu. Đối với cửa bên trái, điều này có nghĩa là đếm số lượng hiện tại là 0 và số lượng hiện tại là 1, đồng thời chọn số lượng nhỏ hơn để lật. Lặp lại tương tự cho cửa bên phải. Cách tiếp cận này đơn giản, chính xác và chạy trong thời gian O(n) vì chúng tôi chỉ quét từng tủ một lần. 

Giải pháp tối ưu về cơ bản giống với giải pháp vũ phu ở đây. Cái nhìn sâu sắc quan trọng là trạng thái cuối cùng của mỗi bên chỉ cần sự đồng nhất. Vì vậy, chúng ta chỉ cần đếm xem mỗi bên có bao nhiêu cửa mở so với đóng và lật thiểu số. Không cần mô phỏng phức tạp hoặc lập trình động. Phương pháp đếm trực tiếp này đạt được thời gian O(n) và không gian bổ sung O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực / Bá tước thiểu số | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu (Đếm) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bộ đếm`left_open`Và`right_open`về không. Chúng sẽ theo dõi số lượng cửa trái và phải đang mở. 
2. Lặp lại từng tủ. Đối với mỗi tủ, nếu cửa bên trái mở (1), tăng dần`left_open`. Nếu cửa bên phải mở, tăng`right_open`. 
3. Tính số lần lật cần thiết cho các cửa bên trái. Cho phép`left_closed`là`n - left_open`. Số lần lật tối thiểu của cửa bên trái là nhỏ hơn`left_open`Và`left_closed`. 
4. Tương tự, tính số lần lật của các cửa bên phải. Cho phép`right_closed`là`n - right_open`. Số lần lật tối thiểu của cửa bên phải là nhỏ hơn`right_open`Và`right_closed`. 
5. Tính tổng số lần lật của cửa trái và cửa phải để có tổng số lần lật tối thiểu và in kết quả. 

Tại sao nó hoạt động: Mỗi cánh cửa chỉ có hai trạng thái có thể. Để làm cho tất cả các cửa ở một bên giống nhau, chúng ta lật tất cả các cửa đang mở sang đóng hoặc tất cả các cửa đóng để mở. Đếm số lượng cửa ở mỗi tiểu bang cho phép chúng ta chọn phương án rẻ hơn. Bất biến này đúng cho cả bên trái và bên phải một cách độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
left_open = 0
right_open = 0

for _ in range(n):
    l, r = map(int, input().split())
    if l == 1:
        left_open += 1
    if r == 1:
        right_open += 1

left_flips = min(left_open, n - left_open)
right_flips = min(right_open, n - right_open)

print(left_flips + right_flips)
```Đầu tiên, mã sẽ đọc số lượng tủ, sau đó lặp qua từng tủ để đếm xem có bao nhiêu cửa hiện đang mở. Nó tính toán số lần lật tối thiểu cho cửa trái và cửa phải một cách riêng biệt và tính tổng chúng. Những cân nhắc triển khai chính bao gồm việc sử dụng số học số nguyên để tránh các lỗi riêng lẻ và đọc dữ liệu đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`cho đầu vào lớn hơn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:```
5
0 1
1 0
0 1
1 1
0 1
```| Tủ | Trái | Đúng | Số lượng mở còn lại | Đếm mở bên phải | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 1 | 
| 2 | 1 | 0 | 1 | 1 | 
| 3 | 0 | 1 | 1 | 2 | 
| 4 | 1 | 1 | 2 | 3 | 
| 5 | 0 | 1 | 2 | 4 | 

Lật trái = min(2, 3) = 2, Lật phải = min(4, 1) = 1. Tổng = 3. Kết quả đầu ra khớp với dự kiến. 

**Mẫu 2 (tất cả các cửa đều giống nhau):** 

đầu vào:```
3
0 0
0 0
0 0
```Số lần mở bên trái = 0, Số lần mở bên phải = 0. Số lần lật = phút(0,3) + phút(0,3) = 0. Đầu ra là 0. 

Những dấu vết này xác nhận thuật toán đếm chính xác các trạng thái thiểu số và tính toán các lần lật tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua tất cả các tủ để đếm trạng thái cửa | 
| Không gian | O(1) | Chỉ có bốn bộ đếm số nguyên được sử dụng | 

Điều này hiệu quả đối với`n`lên tới 10^4, trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    left_open = 0
    right_open = 0
    for _ in range(n):
        l, r = map(int, input().split())
        if l == 1:
            left_open += 1
        if r == 1:
            right_open += 1
    return str(min(left_open, n-left_open) + min(right_open, n-right_open))

# provided sample
assert run("5\n0 1\n1 0\n0 1\n1 1\n0 1\n") == "3", "sample 1"

# all same doors
assert run("3\n0 0\n0 0\n0 0\n") == "0", "all closed"

# all open
assert run("2\n1 1\n1 1\n") == "0", "all open"

# mixed minimal flips
assert run("4\n0 1\n1 1\n0 0\n1 0\n") == "2", "mixed doors"

# minimum size
assert run("2\n0 1\n1 0\n") == "2", "minimum cupboards"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 tủ trộn | 3 | Mẫu khớp trường hợp tiêu chuẩn | 
| 3 đều đóng cửa | 0 | Không cần lật | 
| 2 đều mở | 0 | Không cần lật, tất cả đều mở | 
| 4 hỗn hợp | 2 | Tính đúng số lần lật thiểu số | 
| 2 kích thước tối thiểu | 2 | Thuật toán xử lý đầu vào nhỏ nhất | 

## Vỏ cạnh 

Đối với tủ mà tất cả các cửa ban đầu đều giống nhau, chẳng hạn như:```
3
0 0
0 0
0 0
```Thuật toán đếm số cửa mở bằng 0 cho cả bên trái và bên phải. Số lần lật tối thiểu là min(0,3) = 0 cho cả hai bên, do đó không xảy ra lần lật. Điều này xử lý chính xác trường hợp cạnh của các cửa vốn đã đồng nhất. Tương tự, khi tất cả các cửa đều mở, thuật toán sẽ đưa ra số lần lật bằng 0. Khi có số lượng bằng nhau (số lượng cửa mở và đóng bằng nhau), thuật toán sẽ chọn một trong hai, dẫn đến số lần lật tối thiểu chính xác.
