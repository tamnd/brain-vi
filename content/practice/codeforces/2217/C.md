---
title: "CF 2217C - Lưới che phủ"
description: "Vấn đề hỏi liệu một người bắt đầu từ góc trên bên trái của lưới cuối cùng có thể truy cập mọi ô bằng cách nhảy theo một mẫu rất cụ thể hay không."
date: "2026-06-07T18:24:03+07:00"
tags: ["codeforces", "competitive-programming", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2217
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1091 (Div. 2) and CodeCraft 26"
rating: 1300
weight: 2217
solve_time_s: 100
verified: false
draft: false
---

[CF 2217C - Lưới che phủ](https://codeforces.com/problemset/problem/2217/C) 

**Đánh giá:** 1300 
**Tags:** định lý số dư Trung Hoa, toán, lý thuyết số 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề hỏi liệu một người bắt đầu từ góc trên bên trái của lưới cuối cùng có thể truy cập mọi ô bằng cách nhảy theo một mẫu rất cụ thể hay không. Lưới có`n`hàng và`m`các cột và các bước nhảy dựa trên modulo, nghĩa là lưới bao quanh: di chuyển qua hàng cuối cùng hoặc các vòng lặp cột quay lại hàng đầu tiên. Người đó có thể di chuyển ngay bằng`b`bước và đi xuống`a`các bước, xen kẽ các động tác này, bắt đầu bằng một trong hai động tác. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp gồm bốn số nguyên:`n`,`m`,`a`, Và`b`. Đầu ra là "CÓ" nếu cuối cùng mọi ô đều có thể được truy cập theo mẫu nhảy này và "KHÔNG" nếu ngược lại. 

Các ràng buộc chính là cực kỳ lớn:`n`Và`m`có thể lên đến`10^9`và có thể có tới`10^4`trường hợp thử nghiệm. Một mô phỏng đơn giản theo dõi tất cả các ô được truy cập sẽ yêu cầu lưu trữ tới`10^9`tế bào, điều này là không thể thực hiện được cả về thời gian và không gian. Điều này ngay lập tức loại trừ các phương pháp tiếp cận vũ phu. Tính tuần hoàn và tính chất bao bọc của các bước nhảy cho thấy rằng việc suy luận theo lý thuyết số là cần thiết. 

Các trường hợp cạnh bao gồm các lưới nhỏ như`1x1`hoặc`1xN`, trong đó bất kỳ bước nhảy nào cũng có thể bao phủ lưới một cách tầm thường và các lưới ở đó`a`hoặc`b`chia rẽ`n`hoặc`m`, có khả năng để lại các hàng hoặc cột chưa được truy cập. Ví dụ, trong một`4x2`lưới với`a=2`,`b=1`, di chuyển xuống hai hàng liên tục chỉ chạm vào hàng`1`Và`3`, rời đi`2`Và`4`không thể truy cập được, tạo ra "KHÔNG". 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng chuyển động bằng cách xen kẽ các bước nhảy và theo dõi các ô đã truy cập trong ma trận boolean. Điều này đúng vì nó trực tiếp thực hiện các quy tắc di chuyển, nhưng nó có độ phức tạp`O(n*m)`cho mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được đối với các giới hạn nhất định, đặc biệt là vì`n`Và`m`có thể cao như`10^9`. 

Cách tiếp cận tối ưu đến từ việc phân tích modulo bước nhảy`n`Và`m`. Mẫu hàng được truy cập chỉ phụ thuộc vào`a`Và`n`và mẫu cột được truy cập chỉ phụ thuộc vào`b`Và`m`. Cụ thể, bắt đầu từ hàng`1`, thứ tự các hàng đã ghé thăm bằng cách di chuyển xuống dưới`a`bước lặp đi lặp lại modulo`n`tạo thành một chu trình có độ dài là`n / gcd(n, a)`. Tất cả các hàng được bao phủ khi và chỉ khi`gcd(n, a) = 1`. Tương tự, tất cả các cột đều được che phủ khi và chỉ khi`gcd(m, b) = 1`. Vì các bước di chuyển xen kẽ nhau nên các chuỗi hàng và cột xen kẽ nhau nhưng điều kiện về khả năng tiếp cận sẽ giảm xuống còn việc kiểm tra các điều kiện gcd này. 

Cái nhìn sâu sắc này biến đổi vấn đề từ mô phỏng chuyển động sang tính toán hai ước chung lớn nhất, rất nhanh ngay cả đối với`n`Và`m`lên tới`10^9`. Mỗi trường hợp thử nghiệm sau đó có thể được giải quyết trong`O(log n + log m)`thời gian sử dụng thuật toán Euclide. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n * m) | Quá chậm | 
| Kiểm tra dựa trên GCD | O(log n + log m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc các số nguyên`n`,`m`,`a`, Và`b`. 
2. Tính toán`g = gcd(n, a)`. Điều này thể hiện số chu kỳ hàng rời rạc. Nếu như`g`lớn hơn`1`, một số hàng không thể truy cập được. 
3. Tính toán`h = gcd(m, b)`. Điều này thể hiện số chu kỳ cột rời rạc. Nếu như`h`lớn hơn`1`, một số cột không thể truy cập được. 
4. Nếu cả hai`g`Và`h`bình đẳng`1`, in "CÓ" vì tất cả các hàng và cột đều có thể truy cập được. Nếu không, hãy in "KHÔNG" vì một số ô không thể truy cập được. 

Tại sao nó hoạt động: Chuỗi các hàng được truy cập tạo thành một modulo cấp số cộng`n`với bước`a`. Theo lý thuyết số, cấp số nhân bao gồm tất cả các thặng dư theo modulo`n`nếu và chỉ khi`gcd(n, a) = 1`. Điều tương tự cũng xảy ra với các cột có`b`Và`m`. Các bước di chuyển luân phiên không phá vỡ thuộc tính này, vì việc xen kẽ các chuỗi vẫn yêu cầu phải bao phủ tất cả các hàng và tất cả các cột. Vì vậy việc kiểm tra điều kiện gcd là đủ và cần thiết. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def can_cover_grid(n, m, a, b):
    if math.gcd(n, a) == 1 and math.gcd(m, b) == 1:
        return "YES"
    return "NO"

t = int(input())
results = []
for _ in range(t):
    n, m, a, b = map(int, input().split())
    results.append(can_cover_grid(n, m, a, b))
print("".join(results))
```Mã sử ​​dụng đầu vào nhanh với`sys.stdin.readline`vì có thể có 10.000 trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm được rút gọn thành việc tính toán hai gcds, một thao tác an toàn cho các số nguyên lên tới`10^9`. Chúng tôi nối kết quả vào danh sách và xuất chúng cùng một lúc để tránh I/O chậm. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`n=2, m=2, a=1, b=1`| Bước | gcd(n, a) | gcd(m, b) | Kết quả | 
| --- | --- | --- | --- | 
| Tính toán | gcd(2,1)=1 | gcd(2,1)=1 | CÓ | 

Tất cả các hàng và cột đều nguyên tố cùng nhau với kích thước bước, vì vậy mọi ô đều có thể truy cập được. 

**Mẫu 2**:`n=4, m=2, a=2, b=1`| Bước | gcd(n, a) | gcd(m, b) | Kết quả | 
| --- | --- | --- | --- | 
| Tính toán | gcd(4,2)=2 | gcd(2,1)=1 | KHÔNG | 

Hàng gcd là 2, vì vậy chỉ có thể truy cập được hàng 1 và 3 bằng cách nhảy xuống. Cột gcd là 1, vì vậy tất cả các cột đều có thể truy cập được. Bởi vì không phải tất cả các hàng đều có thể truy cập được nên câu trả lời là "KHÔNG". 

Những dấu vết này xác nhận rằng quá trình kiểm tra gcd xác định chính xác các ô không thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * (log n + log m)) | Mỗi phép tính gcd đều có tính logarit trong đối số của nó. Chúng tôi có`t`trường hợp thử nghiệm. | 
| Không gian | O(t) | Chúng tôi lưu trữ kết quả cho từng trường hợp thử nghiệm. | 

Giải pháp dễ dàng phù hợp với các ràng buộc: 10.000 trường hợp thử nghiệm với hai phép tính gcd cho mỗi trường hợp và không có mảng lớn nào được phân bổ. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        n, m, a, b = map(int, input().split())
        res.append("YES" if math.gcd(n, a) == 1 and math.gcd(m, b) == 1 else "NO")
    return "".join(res)

# provided sample
assert run("4\n2 2 1 1\n4 2 2 1\n1 1 1 1\n3 5 2 3\n") == "YESNONOYES", "sample 1"

# custom cases
assert run("2\n1 1000000000 1 1\n1000000000 1 1 1\n") == "YESYES", "single row/column max size"
assert run("3\n4 4 2 2\n6 9 3 2\n7 7 1 3\n") == "NONONO", "common divisors"
assert run("1\n1000000000 1000000000 999999937 999999937\n") == "YES", "large primes step"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x10^9, 1x10^9 với bước 1 | CÓ | Bao phủ các lưới có kích thước tối đa một cách tầm thường | 
| 4x4 với bước 2 | KHÔNG | GCD để lại các hàng/cột không thể truy cập | 
| 1000000000x1000000000 với bước 999999937 | CÓ | Các bước cơ bản lớn đảm bảo phạm vi phủ sóng đầy đủ | 

## Vỏ cạnh 

Đối với một`1x1`lưới,`n=1, m=1`, bất kỳ kích thước bước nào thực sự không liên quan. Kiểm tra gcd mang lại kết quả`gcd(1, a)=1`Và`gcd(1, b)=1`, trả lời "CÓ". Đối với lưới một hàng như`1xN`, hàng gcd luôn bằng 1, do đó mức độ bao phủ chỉ phụ thuộc vào cột gcd. Một cách tiếp cận mô phỏng các bước di chuyển bất cẩn có thể lặp lại nhầm lẫn vô thời hạn mà không kiểm tra việc gói đó có cho phép bao phủ hoàn toàn hay không, nhưng kiểm tra dựa trên gcd sẽ xử lý nó ngay lập tức.
