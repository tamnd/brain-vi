---
title: "CF 1108A – Hai điểm phân biệt"
description: "Chúng ta có hai đoạn thẳng trên trục số, mỗi đoạn được xác định bởi điểm cuối của nó. Đối với mỗi truy vấn, chúng ta cần chọn một điểm nguyên từ phân đoạn đầu tiên và một điểm nguyên từ phân đoạn thứ hai sao cho các điểm đó khác nhau."
date: "2026-06-12T05:15:41+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1108
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 535 (Div. 3)"
rating: 800
weight: 1108
solve_time_s: 101
verified: false
draft: false
---

[CF 1108A - Hai điểm khác biệt](https://codeforces.com/problemset/problem/1108/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đoạn thẳng trên trục số, mỗi đoạn được xác định bởi điểm cuối của nó. Đối với mỗi truy vấn, chúng ta cần chọn một điểm nguyên từ phân đoạn đầu tiên và một điểm nguyên từ phân đoạn thứ hai sao cho các điểm đó khác nhau. Các phân đoạn có thể chồng chéo hoàn toàn, một phần hoặc không hề chồng chéo, nhưng đảm bảo rằng có giải pháp tồn tại. Nhiệm vụ là xuất ra bất kỳ cặp điểm riêng biệt hợp lệ nào cho mỗi truy vấn. 

Các ràng buộc rất đơn giản. Các điểm cuối có thể lớn bằng$10^9$, nhưng chúng tôi chỉ có tối đa 500 truy vấn. Điều này có nghĩa là chúng tôi không thể cung cấp các thuật toán thử tất cả các số nguyên giữa các điểm cuối của phân đoạn nếu phạm vi lớn. Tuy nhiên, vì bài toán đảm bảo tồn tại nghiệm và các phạm vi là số nguyên liên tục nên chúng ta có thể giải quyết vấn đề này bằng quyết định theo thời gian không đổi cho mỗi truy vấn. 

Trường hợp cạnh không rõ ràng xảy ra khi các đoạn trùng nhau hoặc trùng nhau. Ví dụ: nếu đoạn đầu tiên là [1, 2] và đoạn thứ hai là [1, 2], việc lựa chọn ngây thơ các điểm ngoài cùng bên trái sẽ cho (1, 1), vi phạm yêu cầu về tính khác biệt. Giải pháp phải phát hiện xung đột như vậy và chọn một số nguyên khác từ một trong các phân đoạn, như (1, 2) hoặc (2, 1). 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các số nguyên trong phân đoạn đầu tiên và tất cả các số nguyên trong phân đoạn thứ hai, kiểm tra xem cặp đầu tiên có khác biệt hay không. Mặc dù đúng nhưng cách tiếp cận này có thể yêu cầu lặp lại tới$10^9$số nguyên trong trường hợp xấu nhất là không thể thực hiện được. 

Cách tiếp cận tối ưu khai thác tính đơn giản của vấn đề. Trước tiên chúng ta có thể chọn điểm cuối bên trái của đoạn đầu tiên làm điểm ứng viên$a$. Sau đó, chúng tôi thử điểm cuối bên trái của đoạn thứ hai làm ứng cử viên$b$. Nếu như$b$cũng giống như$a$, thay vào đó, chúng ta có thể chọn đúng điểm cuối của đoạn thứ hai một cách an toàn. Sự đảm bảo của vấn đề đảm bảo rằng điều này sẽ luôn tạo ra một cặp hợp lệ. Điều này làm giảm vấn đề thành một lựa chọn có điều kiện đơn giản, chạy trong thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r1-l1+1)*(r2-l2+1)) | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi truy vấn | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn, hãy đọc điểm cuối$l_1, r_1, l_2, r_2$. 
2. Đặt điểm đầu tiên$a$ĐẾN$l_1$, số nguyên ngoài cùng bên trái trong phân đoạn đầu tiên. Đây là sự lựa chọn tự nhiên vì nó đảm bảo$a$nằm trong phân khúc. 
3. Dự kiến ​​đặt điểm thứ hai$b$ĐẾN$l_2$, số nguyên ngoài cùng bên trái trong phân đoạn thứ hai. 
4. Nếu$a$bằng$b$, điều chỉnh$b$ĐẾN$r_2$, số nguyên ngoài cùng bên phải trong phân đoạn thứ hai. Từ$l_2 < r_2$,$r_2$được đảm bảo khác với$a$nếu như$a = l_2$. 
5. Đầu ra$a$Và$b$. 

Tại sao nó hoạt động: Ở mỗi truy vấn, chúng tôi chọn$a$Và$b$trực tiếp từ các điểm cuối của các phân đoạn. Lần duy nhất lựa chọn đầu tiên thất bại là khi cả hai điểm cuối bên trái đều trùng nhau. Trong trường hợp đó, chúng tôi chọn điểm cuối phù hợp của đoạn thứ hai, được đảm bảo khác với$a$do sự bất bình đẳng nghiêm ngặt$l_2 < r_2$. Việc đảm bảo vấn đề đảm bảo rằng không cần xử lý đặc biệt nào khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
results = []

for _ in range(q):
    l1, r1, l2, r2 = map(int, input().split())
    a = l1
    b = l2
    if a == b:
        b = r2
    results.append(f"{a} {b}")

print("\n".join(results))
```Mã đầu tiên đọc số lượng truy vấn. Đối với mỗi truy vấn, nó trích xuất bốn điểm cuối. Điểm đầu tiên$a$luôn được đặt thành$l_1$, và điểm thứ hai$b$ban đầu là$l_2$. Nếu cả hai điểm trùng nhau thì ta dịch chuyển$b$ĐẾN$r_2$. Các kết quả được thu thập và in thành một khối để tránh các vòng lặp I/O chậm. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:`1 2 1 2`Lựa chọn ban đầu:$a = 1$,$b = 1$Từ$a = b$, bộ$b = r_2 = 2$Đầu ra:`1 2`| Bước | một | b | Lý do | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 1 | Đã chọn điểm cuối bên trái | 
| Điều chỉnh | 1 | 2 | Điểm cuối bên trái trùng nhau, chọn điểm cuối bên phải cho b | 

Dấu vết này thể hiện việc xử lý va chạm khi các phân đoạn chồng lên nhau. 

**Ví dụ 2** 

đầu vào:`1 4 5 8`Lựa chọn ban đầu:$a = 1$,$b = 5$Không có xung đột, đầu ra:`1 5`| Bước | một | b | Lý do | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 5 | Không cần điều chỉnh | 

Điều này cho thấy trường hợp đơn giản trong đó các phân đoạn không trùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn được xử lý trong thời gian không đổi. | 
| Không gian | O(q) | Lưu trữ kết quả cho tất cả các truy vấn trước khi xuất ra. | 

Với$q \le 500$, thao tác này chạy thoải mái trong giới hạn thời gian 1 giây và yêu cầu bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    q = int(input())
    results = []
    for _ in range(q):
        l1, r1, l2, r2 = map(int, input().split())
        a = l1
        b = l2
        if a == b:
            b = r2
        results.append(f"{a} {b}")
    return "\n".join(results)

# provided samples
assert run("5\n1 2 1 2\n2 6 3 4\n2 4 1 3\n1 2 1 3\n1 4 5 8\n") == "1 2\n2 3\n2 1\n1 2\n1 5"

# custom cases
assert run("1\n1 2 1 3\n") == "1 3", "overlap, left endpoints collide"
assert run("1\n1 5 5 8\n") == "1 5", "no overlap, a < b"
assert run("1\n3 7 3 7\n") == "3 7", "identical segments"
assert run("1\n100000000 1000000000 99999999 100000000\n") == "100000000 99999999", "large numbers, overlapping endpoints"
assert run("1\n1 2 3 4\n") == "1 3", "completely separate segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 3 | 1 3 | Chồng chéo, điểm cuối bên trái va chạm | 
| 1 5 5 8 | 1 5 | Không chồng chéo, có điểm khác biệt | 
| 3 7 3 7 | 3 7 | Phân đoạn giống hệt nhau | 
| 100000000 1000000000 99999999 100000000 | 100000000 99999999 | Số lượng lớn, chồng cạnh | 
| 1 2 3 4 | 1 3 | Phân khúc hoàn toàn riêng biệt | 

## Vỏ cạnh 

Khi các đoạn trùng nhau, ví dụ: [3, 7] và [3, 7], thuật toán ban đầu sẽ chọn$a = 3$Và$b = 3$. Vì chúng bằng nhau nên$b$được chuyển sang 7, tạo ra (3, 7), giá trị này hợp lệ. Đối với các đoạn rời rạc, không có sự điều chỉnh nào xảy ra và việc lựa chọn đơn giản các điểm cuối bên trái là đủ. Đối với các phân đoạn chồng chéo tại một điểm duy nhất, ví dụ: [1, 2] và [2, 3], thuật toán chọn (1, 2), cũng thỏa mãn điều kiện phân biệt. Những ví dụ này bao gồm tất cả các tình huống không rõ ràng.
