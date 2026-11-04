---
title: "CF 1145B - Số Kanban"
description: "Chúng ta được yêu cầu xác định xem một số đã cho có thể được biểu diễn dưới dạng tổng của bất kỳ số nguyên nào bằng 4 hoặc 7 hay không. Đầu vào là một số nguyên $a$ trong khoảng từ 1 đến 99 và đầu ra là \"CÓ\" nếu biểu diễn như vậy tồn tại hoặc \"KHÔNG\" nếu ngược lại."
date: "2026-06-12T03:26:01+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1145
codeforces_index: "B"
codeforces_contest_name: "April Fools Day Contest 2019"
rating: 0
weight: 1145
solve_time_s: 96
verified: true
draft: false
---

[CF 1145B - Số Kanban](https://codeforces.com/problemset/problem/1145/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, vũ phu 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem một số đã cho có thể được biểu diễn dưới dạng tổng của bất kỳ số nguyên nào bằng 4 hoặc 7 hay không. Đầu vào là một số nguyên duy nhất$a$trong khoảng từ 1 đến 99 và đầu ra là "CÓ" nếu tồn tại biểu thị như vậy hoặc "KHÔNG" nếu ngược lại. Về mặt khái niệm, chúng ta có thể coi đây là một loại vấn đề đặc biệt về “đổi tiền” chỉ với hai mệnh giá tiền xu: 4 và 7. 

Hạn chế chính đó là$a$nhiều nhất là 99. Giới hạn trên nhỏ này có nghĩa là chúng ta có thể xem xét tất cả các kết hợp có thể có của 4 và 7 mà không cần quan tâm đến hiệu suất. Một cách tiếp cận ngây thơ thử mọi cặp$(x, y)$Ở đâu$4x + 7y = a$là khả thi vì số lượng xu tối đa cho một trong hai mệnh giá là dưới 25 và tổng số kết hợp là rất nhỏ. 

Các trường hợp cạnh xuất hiện khi$a$nhỏ hơn 4 hoặc từ 4 đến 7 nhưng không bằng 4 hoặc 7. Ví dụ: nếu$a = 3$, không có sự kết hợp nào của 4 và 7 có thể tổng bằng 3, vì vậy kết quả đầu ra đúng là "KHÔNG". Nếu như$a = 11$, số có thể được biểu thị dưới dạng$4 + 7$, vì vậy đầu ra là "CÓ". Một cách tiếp cận bất cẩn có thể chỉ thử bội số của 4 hoặc 7 một cách độc lập, thiếu các kết hợp như$4 + 7$. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tất cả các số không âm 4 và 7 có tổng không vượt quá$a$. Với mỗi số có thể là 4, chúng ta kiểm tra xem phần còn lại có$a - 4x$chia hết cho 7. Cách tiếp cận này hiệu quả vì tổng số lần lặp bị giới hạn bởi$\frac{a}{4} \approx 24$trong trường hợp xấu nhất. Mặc dù về mặt kỹ thuật, một vòng lặp đôi có thể được sử dụng, nhưng một vòng lặp đơn trên 4 giây là đủ vì một khi số 4 đã được cố định, việc kiểm tra khả năng chia hết cho 7 là thời gian không đổi. 

Cách tiếp cận tối ưu dựa vào cái nhìn sâu sắc mà chúng ta chỉ cần xem xét$x$từ 0 đến$\lfloor a / 4 \rfloor$. Đối với mỗi$x$, nếu như$(a - 4x) \% 7 == 0$, sau đó$a$có thể được biểu diễn dưới dạng tổng của 4 và 7. Điều này làm giảm việc khám phá không cần thiết về số lượng âm hoặc không thể đếm được trong 7 giây và đảm bảo tìm ra giải pháp nếu có. Không cần phương pháp nhanh hơn vì phạm vi đầu vào nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(a^2) | O(1) | Được chấp nhận do đầu vào nhỏ | 
| Kiểm tra vòng lặp đơn | O(a/4) = O(1) | O(1) | Được chấp nhận và đơn giản hơn | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên$a$từ đầu vào. Đây là tổng mục tiêu mà chúng tôi muốn kiểm tra. 
2. Lặp lại$x$từ 0 đến$\lfloor a / 4 \rfloor$. Mỗi$x$đại diện cho số 4 được sử dụng. 
3. Đối với mỗi$x$, tính số dư$r = a - 4x$. Đây là số tiền phải được trả trong 7 giây. 
4. Kiểm tra xem$r$chia hết cho 7 bằng cách sử dụng$r \% 7 == 0$. Nếu đúng, xuất "CÓ" và dừng. 
5. Nếu không$x$thỏa mãn điều kiện này sau vòng lặp, xuất ra "NO". 

Tại sao nó hoạt động: Mọi sự kết hợp tiềm năng của số 4 và số 7 đều có thể được biểu thị dưới dạng$4x + 7y = a$. Bằng cách lặp lại tất cả các số đếm khả thi là 4 giây, chúng tôi đảm bảo kiểm tra mọi giá trị có thể có của$y$đó là không tiêu cực. Việc kiểm tra tính chia hết đảm bảo rằng$y$là số nguyên thỏa mãn phương trình ban đầu. Không có sự kết hợp hợp lệ có thể được bỏ qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
for x in range(a // 4 + 1):
    if (a - 4 * x) % 7 == 0:
        print("YES")
        break
else:
    print("NO")
```Mã đọc số nguyên$a$và lặp đi lặp lại$x$từ 0 đến$a // 4$. biểu hiện$(a - 4 * x) \% 7$trực tiếp kiểm tra xem phần còn lại có thể được hình thành hoàn toàn bằng 7 giây hay không. Sử dụng một`for`vòng lặp với một`else`mệnh đề đảm bảo chúng tôi chỉ in "KHÔNG" nếu không hợp lệ$x$được tìm thấy. Một lỗi phổ biến là lỗi từng lỗi một trong giới hạn vòng lặp; đây,`a // 4 + 1`bao gồm chính xác trường hợp$x = a // 4$. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`5`| x | r = a - 4*x | r% 7 == 0? | 
| --- | --- | --- | 
| 0 | 5 | Sai | 
| 1 | 1 | Sai | 

Không hợp lệ$x$tồn tại, vì vậy đầu ra là "KHÔNG". Điều này xác nhận thuật toán xử lý các số nhỏ một cách chính xác. 

Đầu vào mẫu 2:`11`| x | r = a - 4*x | r% 7 == 0? | 
| --- | --- | --- | 
| 0 | 11 | Sai | 
| 1 | 7 | Đúng | 

Thuật toán in ra "CÓ", chứng tỏ nó xác định chính xác các kết hợp như 4 + 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a/4) = O(1) | Tối đa 25 lần lặp cho a ≤ 99 | 
| Không gian | O(1) | Chỉ bộ đếm vòng lặp và phần còn lại được lưu trữ | 

Giải pháp vừa vặn thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a = int(input())
    for x in range(a // 4 + 1):
        if (a - 4 * x) % 7 == 0:
            return "YES"
    return "NO"

# provided sample
assert run("5\n") == "NO", "sample 1"

# custom cases
assert run("11\n") == "YES", "4 + 7"
assert run("3\n") == "NO", "too small"
assert run("28\n") == "YES", "4*7 or 7*4"
assert run("99\n") == "YES", "largest input, multiple solutions"
assert run("1\n") == "NO", "minimum input, impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 11 | CÓ | kết hợp 4 + 7 | 
| 3 | KHÔNG | quá nhỏ, không có giải pháp | 
| 28 | CÓ | nhiều giải pháp khả thi | 
| 99 | CÓ | đầu vào lớn nhất, kiểm tra căng thẳng | 
| 1 | KHÔNG | đầu vào tối thiểu, không thể | 

## Vỏ cạnh 

cho`a = 1`, vòng lặp chạy với x = 0, r = 1, không chia hết cho 7. Đầu ra là "NO", xử lý giới hạn dưới một cách chính xác. Vì`a = 28`, x = 0 mang lại r = 28 chia hết cho 7, do đó thuật toán sẽ in ngay "CÓ", cho thấy nó tìm đúng các giải pháp chỉ sử dụng 7 giây. Những ví dụ này xác nhận rằng tất cả các kết hợp trong phạm vi đầu vào đều được kiểm tra hiệu quả.
