---
title: "CF 16A - Cờ"
description: "Chúng ta được cung cấp một lưới hình chữ nhật tượng trưng cho lá cờ của một quốc gia. Mỗi ô trong lưới có một màu, được mã hóa dưới dạng chữ số từ 0 đến 9. Kích thước của cờ là các hàng theo cột. Mục đích là để xác định xem cờ có bị “sọc” theo tiêu chuẩn ISO mới hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 16
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 16 (Div. 2 Only)"
rating: 800
weight: 16
solve_time_s: 68
verified: true
draft: false
---
[CF 16A - Cờ](https://codeforces.com/problemset/problem/16/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới hình chữ nhật tượng trưng cho lá cờ của một quốc gia. Mỗi ô trong lưới có một màu, được mã hóa dưới dạng chữ số từ 0 đến 9. Kích thước của lá cờ là$n$hàng theo$m$cột. Mục đích là để xác định xem cờ có bị “sọc” theo tiêu chuẩn ISO mới hay không. Cờ bị sọc nếu mỗi hàng chứa các ô cùng màu và các hàng liên tiếp không có cùng màu. 

Đầu vào nhỏ, với$n, m \leq 100$. Điều này có nghĩa là ngay cả một$O(n \cdot m)$giải pháp có thể chấp nhận được vì tổng số ô nhiều nhất là 10.000, con số này không đáng kể đối với các CPU hiện đại. 

Một số trường hợp cạnh không rõ ràng bao gồm các cờ có một hàng duy nhất, trong đó không có hàng trước đó để so sánh và các cờ có một cột duy nhất, trong đó mỗi hàng vẫn cần phải đồng nhất. Ví dụ: một lá cờ có một hàng gồm ba ô`111`nên quay lại`YES`bởi vì tất cả các ô trong hàng đó đều giống hệt nhau, mặc dù không có hàng trên hoặc dưới để so sánh màu sắc. Một tình huống phức tạp khác là khi hai hàng liền kề vô tình có cùng màu, chẳng hạn như`111`theo sau là`111`. Điều này sẽ trở lại`NO`vì các sọc không rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên mọi ô, kiểm tra hai điều kiện: mỗi hàng đều đồng nhất và mỗi hàng liên tiếp khác với hàng trước. Điều này đúng vì bài toán xác định rõ ràng các thuộc tính của một lá cờ hợp lệ. Sự phức tạp là$O(n \cdot m)$, có thể chấp nhận được đối với kích thước bài toán này. 

Thông tin chi tiết quan trọng là chúng ta có thể đơn giản hóa việc kiểm tra: thay vì liên tục xác minh từng ô riêng lẻ với mọi ô khác trong hàng, chúng ta chỉ cần so sánh từng ô với ô đầu tiên của hàng. Nếu bất kỳ ô nào khác, hàng đó không hợp lệ. Để kiểm tra các hàng liên tiếp, chúng ta chỉ cần so sánh ô đầu tiên của hàng hiện tại với ô đầu tiên của hàng trước đó. Điều này làm giảm những so sánh không cần thiết nhưng không làm thay đổi độ phức tạp tiệm cận. 

Brute-force hoạt động vì nó kiểm tra mọi yêu cầu một cách rõ ràng, nhưng việc quan sát so sánh ô đầu tiên làm giảm độ phức tạp trong nhận thức và triển khai, giúp mã sạch hơn và ít xảy ra lỗi hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(1) | Đã chấp nhận | 
| Kiểm tra hàng đầu tiên được tối ưu hóa | O(n·m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước$n$Và$m$từ đầu vào và lưu cờ dưới dạng danh sách các chuỗi, trong đó mỗi chuỗi đại diện cho một hàng. Điều này bảo tồn cấu trúc hàng để dễ dàng kiểm tra. 
2. Lặp lại từng hàng bắt đầu từ hàng đầu tiên. Đối với mỗi hàng, hãy kiểm tra xem tất cả các ô có giống nhau không. So sánh từng ký tự trong hàng với ký tự đầu tiên. Nếu có bất kỳ ký tự nào khác, xuất ra`NO`và dừng lại. 
3. Nếu hàng hiện tại không phải là hàng đầu tiên, hãy so sánh ký tự đầu tiên của nó với ký tự đầu tiên của hàng trước đó. Nếu chúng giống nhau thì xuất ra`NO`và dừng lại. 
4. Nếu tất cả các hàng đều vượt qua cả hai lần kiểm tra, hãy xuất`YES`. 

**Tại sao nó hoạt động:** Thuật toán đảm bảo rằng mọi hàng đều đồng nhất bằng cách so sánh tất cả các ô với ô đầu tiên của hàng. Bằng cách kiểm tra các ô đầu tiên của các hàng liên tiếp, nó đảm bảo rằng các hàng liền kề là khác nhau. Hai séc này thỏa mãn định nghĩa về cờ sọc. Không cần kiểm tra khác vì chữ số màu được đảm bảo là 0-9. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
flag = [input().strip() for _ in range(n)]

for i in range(n):
    row = flag[i]
    if any(c != row[0] for c in row):
        print("NO")
        sys.exit(0)
    if i > 0 and row[0] == flag[i - 1][0]:
        print("NO")
        sys.exit(0)

print("YES")
```Đầu tiên, mã đọc cờ thành danh sách các chuỗi. các`any(c != row[0] for c in row)`xây dựng kiểm tra tính đồng nhất của hàng một cách hiệu quả. Điều kiện thứ hai đảm bảo các sọc khác nhau. sử dụng`sys.exit(0)`ngay lập tức chấm dứt chương trình khi phát hiện thấy vi phạm, điều này tránh việc lặp lại không cần thiết trên các hàng còn lại. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 3
000
111
222
```| Bước | Hàng | Đồng phục? | Ô đầu tiên so với hàng trước? | 
| --- | --- | --- | --- | 
| 0 | 000 | Có | Không áp dụng | 
| 1 | 111 | Có | 1 != 0, được | 
| 2 | 222 | Có | 2 != 1, được | 

Đầu ra:`YES`Giải thích: Mỗi hàng đều đồng nhất và không có hai hàng liên tiếp nào có cùng màu. 

**Đầu vào tùy chỉnh 2** 

đầu vào:```
2 3
111
111
```| Bước | Hàng | Đồng phục? | Ô đầu tiên so với hàng trước? | 
| --- | --- | --- | --- | 
| 0 | 111 | Có | Không áp dụng | 
| 1 | 111 | Có | 1 == 1, THẤT BẠI | 

Đầu ra:`NO`Giải thích: Các hàng liên tiếp có cùng màu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·m) | Chúng tôi quét từng hàng hoàn toàn một lần. | 
| Không gian | O(n·m) | Việc lưu trữ lưới dưới dạng danh sách các chuỗi yêu cầu một chuỗi trên mỗi hàng. | 

Vì$n, m \leq 100$, thuật toán xử lý tối đa 10.000 ô, trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    n, m = map(int, input().split())
    flag = [input().strip() for _ in range(n)]
    for i in range(n):
        row = flag[i]
        if any(c != row[0] for c in row):
            print("NO")
            return sys.stdout.getvalue().strip()
        if i > 0 and row[0] == flag[i - 1][0]:
            print("NO")
            return sys.stdout.getvalue().strip()
    print("YES")
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3 3\n000\n111\n222\n") == "YES", "sample 1"

# Custom cases
assert run("1 5\n33333\n") == "YES", "single row"
assert run("5 1\n1\n2\n3\n4\n5\n") == "YES", "single column, distinct"
assert run("2 4\n7777\n7777\n") == "NO", "repeated color"
assert run("3 3\n123\n456\n789\n") == "NO", "row not uniform"
assert run("3 2\n11\n22\n22\n") == "NO", "last two rows same"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5\n33333 | CÓ | Hàng đơn hợp lệ | 
| 5 1\n1\n2\n3\n4\n5 | CÓ | Cờ một cột có màu sắc riêng biệt | 
| 2 4\n7777\n7777 | KHÔNG | Các hàng liền kề chia sẻ màu sắc | 
| 3 3\n123\n456\n789 | KHÔNG | Hàng không đồng nhất | 
| 3 2\n11\n22\n22 | KHÔNG | Hai hàng cuối cùng cùng màu | 

## Vỏ cạnh 

Cờ có một hàng, chẳng hạn như:```
1 4
4444
```Thuật toán kiểm tra tính đồng nhất của hàng:`all(c == row[0] for c in row)`vượt qua. Vì không có hàng trước nên việc kiểm tra hàng liền kề bị bỏ qua. Đầu ra là`YES`. 

Cờ có một cột:```
3 1
1
2
2
```Tính đồng nhất của hàng trôi qua một cách tầm thường vì mỗi hàng có một ô. Việc kiểm tra hàng liền kề không thành công đối với hai hàng cuối cùng vì`2 == 2`. Đầu ra là`NO`. 

Những ví dụ này xác nhận rằng thuật toán xử lý các kích thước tối thiểu và so sánh cạnh một cách chính xác.
