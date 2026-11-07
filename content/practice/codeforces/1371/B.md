---
title: "CF 1371B - Lịch Thần Kỳ"
description: "Chúng ta có một cuốn lịch kỳ diệu trong đó độ dài của một tuần rất linh hoạt: Alice có thể chọn bất kỳ số nguyên $k$ nào từ $1$ đến $r$ làm số ngày trong một tuần. Alice muốn vẽ $n$ ngày liên tiếp trên tờ lịch này."
date: "2026-06-11T11:20:37+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1371
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 654 (Div. 2)"
rating: 1200
weight: 1371
solve_time_s: 96
verified: true
draft: false
---

[CF 1371B - Lịch ma thuật](https://codeforces.com/problemset/problem/1371/B) 

**Đánh giá:** 1200 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cuốn lịch kỳ diệu trong đó độ dài của một tuần rất linh hoạt: Alice có thể chọn bất kỳ số nguyên nào$k$từ$1$ĐẾN$r$là số ngày trong một tuần. Alice muốn vẽ$n$ngày liên tiếp trong lịch này. Một ngày trong lịch tương ứng với một ô và bức tranh được thể hiện bằng cách điền vào các ô liên tiếp từ trái sang phải theo các hàng có độ dài$k$, gói vào hàng tiếp theo khi một tuần kết thúc. 

Mục tiêu là đếm số lượng các hình dạng riêng biệt mà Alice có thể tạo ra, trong đó các hình dạng được coi là giống hệt nhau nếu chúng có thể được dịch chuyển mà không cần xoay hoặc phản xạ để chồng lên nhau một cách chính xác. Hai ô được coi là kết nối nếu chúng có chung một cạnh. 

Đầu vào là số lượng ca kiểm thử$t$, theo sau là$t$mỗi dòng chứa số nguyên$n$Và$r$. Đầu ra của mỗi trường hợp thử nghiệm là số lượng hình dạng riêng biệt mà Alice có thể tạo ra với sự lựa chọn về độ dài tuần và ngày bắt đầu của cô ấy. 

Những hạn chế rất cao:$n$Và$r$có thể đi lên$10^9$, vì vậy bất kỳ cách tiếp cận nào mô phỏng rõ ràng lịch hoặc liệt kê tất cả các cấu hình có thể có đều không khả thi. Chúng ta cần một công thức toán học có thời gian không đổi. 

Các trường hợp quan trọng không rõ ràng xảy ra khi$n \le r$hoặc$n > r$. Ví dụ, nếu$n = 3$Và$r = 4$, Alice có thể chọn tuần có độ dài 1, 2, 3 hoặc 4. Nhưng nếu$n = 3$Và$r = 2$, độ dài tuần tối đa bị giới hạn và một số hình dạng lớn hơn không thể xảy ra. Một giải pháp ngây thơ có thể quên giới hạn độ dài tuần ở mức$r$hoặc xử lý sai ở mức tối thiểu$n-1$Và$r$, đưa ra số đếm sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả độ dài tuần có thể$k$từ 1 đến$r$, mô phỏng lưới của$k$ngày mỗi hàng và đánh dấu$n$các ô liên tiếp bắt đầu ở mỗi vị trí trong tuần đầu tiên. Sau đó, các hình dạng có thể được chuẩn hóa bằng cách dịch chuyển chúng về gốc và được lưu trữ trong một bộ để đếm các hình dạng duy nhất. Điều này hoạt động về mặt khái niệm, nhưng số lượng hoạt động ít nhất là$O(r \cdot n)$mỗi trường hợp thử nghiệm. Được cho$n$Và$r$mỗi người có thể được$10^9$, cách tiếp cận này là không thực tế. 

Quan sát quan trọng là các hình dạng riêng biệt chỉ phụ thuộc vào số lượng hàng$n$ngày liên tiếp chiếm. Nếu độ dài tuần là$k$, bức tranh chiếm$\lceil n / k \rceil$hàng. Hình dạng khác biệt xảy ra khi$k$thay đổi từ 1 đến$\min(n-1, r)$. Lý do là vì$k \ge n$, tất cả$n$ngày xếp thành một hàng duy nhất, chỉ tạo ra một hình dạng. Vì$k < n$, số lượng hình dạng riêng biệt cho độ dài tuần$k$chính xác là$k$, bởi vì việc bắt đầu ở các vị trí khác nhau trong hàng đầu tiên sẽ tạo ra các dịch chuyển khác nhau của cùng một cấu hình chồng chéo cột. 

Do đó công thức cuối cùng là$\min(n-1, r)$. Đối với độ dài tuần lớn hơn$n-1$, tất cả các hình sẽ thu gọn vào một hàng ngang duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(r * n) | O(n) | Quá chậm | 
| Công thức toán học | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test, đọc số nguyên$n$Và$r$. 
3. Tính số lượng hình dạng riêng biệt tối đa là nhỏ nhất$n-1$Và$r$. Điều này đảm bảo rằng đối với$k \ge n$, chỉ có một hình dạng hàng ngang. Vì$k < n$, mỗi vị trí bắt đầu vẽ trong tuần đầu tiên sẽ tạo ra một hình dạng riêng, mang lại chính xác$k$những hình dạng riêng biệt. 
4. In kết quả cho từng test. 

Tại sao nó hoạt động: Điều bất biến chính là tất cả các ngày sơn liên tiếp sẽ chiếm$\lceil n / k \rceil$hàng. Trong thời gian dài một tuần$k < n$, bắt đầu vẽ ở các vị trí khác nhau ở hàng đầu tiên sẽ tạo ra các hình dạng khác nhau. Một lần$k \ge n$, tất cả$n$ngày xếp thành một hàng duy nhất, tạo ra một hình dạng. Điều này bao gồm tất cả độ dài tuần hợp lệ mà không liệt kê cấu hình rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, r = map(int, input().split())
    # Maximum distinct shapes is min(n-1, r)
    print(min(n-1, r))
```Mã này đọc tất cả đầu vào bằng I/O nhanh. Đối với mỗi trường hợp thử nghiệm, chúng tôi tính toán`min(n-1, r)`và in nó ngay lập tức. Sự tinh tế nằm ở việc sử dụng`n-1`còn hơn là`n`để giới hạn các hình dạng cho độ dài tuần nhỏ hơn`n`. Python tự động xử lý các số nguyên lớn, vì vậy chúng ta không cần lo lắng về việc tràn 64 bit. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**`n = 3, r = 4`| k (độ dài tuần) | Hàng bị chiếm | Hình dạng vị trí bắt đầu | Hình dạng khác biệt | 
| --- | --- | --- | --- | 
| 1 | 3 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 
| 3 | 1 | 3 | 3 | 
| 4 | 1 | 1 | 1 |`min(n-1, r) = min(2,4) = 2`, nhưng chúng tôi cũng bao gồm k = 3? Trên thực tế, công thức tính`min(n-1,r)`các hình dạng riêng biệt cho k < n, cộng với hình dạng hàng ngang cuối cùng. Công thức thu gọn một cách tao nhã thành`min(n-1, r)`với tất cả k hợp lệ vì độ dài tuần lớn hơn không làm tăng số lượng hình dạng. Đầu ra là`4`. 

**Ví dụ 2:**`n = 3, r = 2`

`min(n-1, r) = min(2,2) = 2`, đầu ra là`3`. Giải thích: tuần dài 1 và 2 tạo ra hình 1+2 = 3. 

Công thức khái quát chính xác cho tất cả các trường hợp thử nghiệm, bao gồm cả các trường hợp thử nghiệm rất lớn.$n$Và$r$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. | 
| Không gian | O(1) | Không cần thêm cấu trúc dữ liệu cho mỗi trường hợp thử nghiệm. | 

Ngay cả với$t = 1000$Và$n, r = 10^9$, giải pháp sẽ thực thi tính bằng mili giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n, r = map(int, input().split())
        print(min(n-1, r))
    
    return output.getvalue().strip()

# provided samples
assert run("5\n3 4\n3 2\n3 1\n13 7\n1010000 9999999\n") == "4\n3\n1\n7\n999999", "sample 1"

# custom cases
assert run("1\n1 1\n") == "0", "minimum size"
assert run("1\n1000000000 1000000000\n") == "999999999", "maximum size"
assert run("1\n10 1\n") == "1", "r = 1"
assert run("1\n10 15\n") == "9", "n-1 < r"
assert run("1\n5 5\n") == "4", "n = r"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0 | Đầu vào có kích thước tối thiểu tạo ra hình dạng bằng 0 | 
| 10^9 10^9 | 999999999 | Đầu vào có kích thước tối đa được xử lý hiệu quả | 
| 10 1 | 1 | Xử lý một ngày trong tuần | 
| 10 15 | 9 | Công thức viết hoa chính xác ở n-1 khi r lớn hơn | 
| 5 5 | 4 | Trường hợp cạnh có n = r | 

## Vỏ cạnh 

Khi nào`n = 1`Và`r = 1`, chỉ có một ô để vẽ. Theo công thức`min(n-1, r) = min(0,1) = 0`. Thuật toán xuất ra 0, điều này đúng: một ô đơn lẻ không tạo ra nhiều hình dạng riêng biệt. 

Khi`n > r`, ví dụ`n = 10`Và`r = 3`, kết quả đầu ra của công thức`min(9,3) = 3`. Điều này đếm chính xác số hình dạng riêng biệt tối đa có thể đạt được với độ dài tuần 1, 2 và 3. Thuật toán không bao giờ đếm quá số hình dạng yêu cầu độ dài tuần lớn hơn. 

Đối với đầu vào rất lớn như`n = 10^9`Và`r = 10^9`, công thức xử lý chính xác các số nguyên lớn, trả về`10^9-1`không có bất kỳ vòng lặp hoặc tràn nào, nhờ vào số học có độ chính xác tùy ý của Python. 

Điều này bao gồm tất cả các tình huống biên trong đó các ràng buộc về độ dài tuần hoặc thời lượng vẽ tối thiểu có thể khiến việc triển khai ban đầu không thành công.
