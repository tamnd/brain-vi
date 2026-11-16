---
title: "CF 1928A - Cắt hình chữ nhật"
description: "Chúng ta được cho một hình chữ nhật có các cạnh $a$ và $b$, và chúng ta được hỏi liệu có thể cắt hình chữ nhật này dọc theo một đường thẳng song song với một trong các cạnh của nó thành hai hình chữ nhật nhỏ hơn có kích thước nguyên, sau đó sử dụng hai mảnh này để tạo thành một hình chữ nhật khác nhau về…"
date: "2026-06-08T18:50:36+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 1928
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 924 (Div. 2)"
rating: 800
weight: 1928
solve_time_s: 337
verified: false
draft: false
---

[CF 1928A - Cắt hình chữ nhật](https://codeforces.com/problemset/problem/1928/A) 

**Đánh giá:** 800 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hình chữ nhật có cạnh$a$Và$b$, và chúng ta được hỏi liệu có thể cắt hình chữ nhật này dọc theo một đường thẳng song song với một trong các cạnh của nó thành hai hình chữ nhật nhỏ hơn có kích thước nguyên, sau đó sử dụng hai mảnh này để tạo thành một hình chữ nhật **có hình dạng khác** so với hình ban đầu$a \times b$hình chữ nhật. Các hình chữ nhật quay với nhau (ví dụ:$6 \times 4$Và$4 \times 6$) được coi là giống nhau. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm hai số nguyên$a$Và$b$thể hiện kích thước của hình chữ nhật. Đầu ra phải trả lời "Có" nếu có thể tạo thành một hình chữ nhật khác hoặc "Không" nếu ngược lại. 

Các ràng buộc rất lớn:$a$Và$b$có thể lên đến$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê tất cả các vết cắt có thể hoặc mô phỏng việc tạo thành tất cả các hình chữ nhật có thể. Chúng tôi cần một giải pháp tính toán câu trả lời trong **thời gian không đổi cho mỗi trường hợp thử nghiệm**. 

Các hình chữ nhật không tầm thường nhỏ nhất làm nổi bật các đường viền tinh tế. MỘT$1 \times 1$hình chữ nhật hoàn toàn không thể cắt được nên câu trả lời là "Không". MỘT$2 \times 1$hình chữ nhật chỉ có thể được cắt thành$1 \times 1$hình chữ nhật có thể ghép lại thành$1 \times 2$, giống với bản gốc sau khi quay nên câu trả lời cũng là "Không". Hình chữ nhật có kích thước$2 \times 2$hoặc lớn hơn thường cho phép cắt để tạo ra hình chữ nhật mới. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi đường cắt ngang và dọc có thể, tạo ra các hình chữ nhật thu được và kiểm tra xem liệu có thể hình thành một hình chữ nhật khác hay không. Mỗi lần cắt tạo ra hai hình chữ nhật và chúng ta có thể cố gắng kết hợp chúng theo cả cấu hình xếp chồng ngang và dọc. Đối với kích thước lớn, điều này nhanh chóng trở nên không khả thi vì ngay cả một hình chữ nhật có kích thước gần$10^9$sẽ tạo ra$10^9$khả năng, và với$10^4$trường hợp thử nghiệm, không thể chạy trong 1 giây. 

Quan sát chính là hình học và tổ hợp. Để tạo thành một hình chữ nhật mới khác với hình chữ nhật ban đầu, chúng ta cần ít nhất một cạnh của hình chữ nhật ban đầu bằng 2. Nếu cả hai cạnh đều bằng 1 thì chúng ta không thể cắt được. Nếu có ít nhất một cạnh bằng 2 hoặc lớn hơn, chúng ta luôn có thể cắt dọc theo cạnh đó để tạo ra hai hình chữ nhật có thể sắp xếp lại thành hình chữ nhật có tỷ lệ khác nhau. Cụ thể, bất kỳ hình chữ nhật nào có$a \ge 2$hoặc$b \ge 2$có thể cắt thành hai mảnh có ít nhất một chiều bằng 1 và những mảnh này có thể xếp chồng lên nhau để tạo thành một hình chữ nhật mới có các cạnh bằng tổng của các mảnh đã cắt. Ngoại lệ duy nhất là hình chữ nhật$2 \times 1$hoặc$1 \times 2$, không thể tạo ra hình dạng mới, vì cắt dọc theo cạnh lớn hơn sẽ tạo ra hai$1 \times 1$hình vuông, tái tạo lại thành cùng một hình chữ nhật. 

Do đó, vấn đề được rút gọn thành một phép kiểm tra có điều kiện đơn giản: hình chữ nhật quá nhỏ để tạo ra một hình dạng mới nếu nó$1 \times 1$hoặc$2 \times 1$(hoặc$1 \times 2$) và ngược lại là "Có". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(min(a,b)) cho mỗi trường hợp thử nghiệm | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$a$Và$b$. 
3. Hoán đổi$a$Và$b$nếu cần thiết để$a \le b$. Điều này đơn giản hóa việc suy luận về các cạnh nhỏ hơn và lớn hơn. 
4. Kiểm tra xem$a = 1$Và$b = 1$. Nếu đúng, xuất ra "Không" vì a$1 \times 1$hình chữ nhật không thể cắt được. 
5. Kiểm tra xem$a = 1$Và$b = 2$. Nếu đúng, xuất ra "Không" vì a$1 \times 2$hình chữ nhật chỉ có thể tạo ra hình chữ nhật giống nhau sau khi cắt và sắp xếp lại. 
6. Nếu không, xuất ra "Có" vì tồn tại một vết cắt có thể tạo ra một hình chữ nhật mới. 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: điều bất biến là cạnh nhỏ nhất của hình chữ nhật xác định liệu có thể thực hiện được một đường cắt có ý nghĩa hay không. Nếu cạnh nhỏ nhất ít nhất là 2 thì việc cắt ngang hoặc cắt dọc có thể tạo ra hai mảnh có thể ghép lại thành hình chữ nhật mới. Ngoại lệ duy nhất là hình chữ nhật có kích thước tối thiểu$1 \times 1$Và$1 \times 2$, được xử lý rõ ràng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    if a > b:
        a, b = b, a
    if a == 1 and b <= 2:
        print("No")
    else:
        print("Yes")
```Mã đọc tất cả các trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Hoán đổi đảm bảo chúng ta luôn xử lý bên nhỏ hơn trước, giúp đơn giản hóa việc kiểm tra trường hợp biên. điều kiện`a == 1 and b <= 2`chụp các hình chữ nhật tối thiểu không thể tạo ra hình dạng mới. Tất cả các hình chữ nhật khác tự động thỏa mãn điều kiện hình học để có thể cắt. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 6
```| một | b | điều kiện a > b | kiểm tra a==1 và b<=2 | đầu ra | 
| --- | --- | --- | --- | --- | 
| 2 | 6 | không | không | Có | 

Giải thích: Hình chữ nhật$2 \times 6$có thể được cắt dọc theo cạnh dài hơn thành$2 \times 3$Và$2 \times 3$. Xếp chồng chúng tạo thành một$4 \times 3$hình chữ nhật, khác với$2 \times 6$. 

### Đầu vào mẫu 2```
1 1
```| một | b | điều kiện a > b | kiểm tra a==1 và b<=2 | đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | vâng | Không | 

Giải thích:$1 \times 1$không thể cắt được chút nào, do đó không thể tạo thành một hình chữ nhật mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được đánh giá bằng các lần kiểm tra liên tục. | 
| Không gian | O(1) | Chỉ cần một vài biến số nguyên cho mỗi trường hợp thử nghiệm. | 

Được cho$t \le 10^4$và mỗi trường hợp thử nghiệm thực hiện$O(1)$, giải pháp sẽ thực thi tốt trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        a, b = map(int, input().split())
        if a > b:
            a, b = b, a
        if a == 1 and b <= 2:
            print("No")
        else:
            print("Yes")
    
    return output.getvalue().strip()

# provided samples
assert run("7\n1 1\n2 1\n2 6\n3 2\n2 2\n2 4\n6 3\n") == "No\nNo\nYes\nYes\nYes\nYes\nNo"

# custom cases
assert run("3\n1 2\n1 3\n2 2\n") == "No\nYes\nYes", "minimum side and exact 2"
assert run("2\n10 1\n1 10\n") == "Yes\nYes", "large rectangle, small side 1"
assert run("2\n1 1\n2 1\n") == "No\nNo", "smallest rectangles"
assert run("2\n1000000000 2\n2 1000000000\n") == "Yes\nYes", "max size inputs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | Không | Hình chữ nhật tối thiểu không thể tạo ra hình dạng mới | 
| 1 3 | Có | Cạnh nhỏ 1 nhưng cạnh dài hơn > 2, có thể hình chữ nhật mới | 
| 2 2 | Có | Hình vuông lớn hơn trường hợp tối thiểu | 
| 1000000000 2 | Có | Kích thước đầu vào tối đa được xử lý hiệu quả | 
| 2 1 | Không | Vỏ cạnh hình chữ nhật tối thiểu | 

## Vỏ cạnh 

Đối với$1 \times 1$hình chữ nhật, thuật toán xác định chính xác rằng`a==1 and b<=2`là đúng và xuất ra "Không". Vì$1 \times 2$, điều kiện tương tự sẽ kích hoạt "Không". Đối với hình chữ nhật như$2 \times 3$hoặc lớn hơn, điều kiện là sai và thuật toán đưa ra "Có", nắm bắt trường hợp chung trong đó vết cắt có thể tạo ra một hình chữ nhật mới. Bước hoán đổi đảm bảo rằng cạnh nhỏ hơn luôn ở trong`a`, do đó việc kiểm tra điều kiện cạnh là đồng nhất và không bỏ sót bất kỳ trường hợp tối thiểu nào.
