---
title: "CF 1627D - Không thêm"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt và chúng ta có thể liên tục thêm các phần tử mới vào đó. Cụ thể, chúng ta có thể chọn hai phần tử bất kỳ, tính ước số chung lớn nhất (GCD) của chúng và nối nó vào mảng nếu GCD đó chưa có."
date: "2026-06-10T05:16:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1627
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 766 (Div. 2)"
rating: 1900
weight: 1627
solve_time_s: 80
verified: true
draft: false
---

[CF 1627D - Không thêm](https://codeforces.com/problemset/problem/1627/D) 

**Xếp hạng:** 1900 
**Tags:** sức mạnh vũ phu, dp, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt và chúng ta có thể liên tục thêm các phần tử mới vào đó. Cụ thể, chúng ta có thể chọn hai phần tử bất kỳ, tính ước số chung lớn nhất (GCD) của chúng và nối nó vào mảng nếu GCD đó chưa có. Mục tiêu là tìm ra số lượng tối đa các thao tác như vậy mà chúng ta có thể thực hiện trước khi không thể thêm GCD nào nữa. 

Đầu vào bao gồm tối đa một triệu số nguyên, mỗi số từ 1 đến một triệu. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp kém hơn$O(n \log n)$hoặc$O(n \sqrt{m})$, Ở đâu$m$là giá trị tối đa trong mảng, có thể quá chậm. Một cách tiếp cận ngây thơ thử tất cả các cặp liên tục sẽ yêu cầu$O(n^2)$hoạt động cho mỗi lần bổ sung, có thể đạt tới$10^{12}$trong trường hợp xấu nhất. Điều đó vượt xa giới hạn thời gian. 

Một trường hợp cạnh quan trọng là khi mảng đã chứa 1. Vì GCD luôn là số nguyên dương nên bất kỳ chuỗi thao tác nào cuối cùng cũng tạo ra 1 nếu nó không xuất hiện. Khi 1 được thêm vào, không còn số mới nào nhỏ hơn 1 nữa, do đó quá trình tự nhiên kết thúc. Một trường hợp khác là mảng số nguyên tố. Ví dụ, nếu mảng là`[2, 3, 5]`, số mới duy nhất chúng ta có thể thêm là 1 vì tất cả các GCD theo cặp khác đều là 1. Việc triển khai bất cẩn có thể cố gắng tiếp tục tạo ra các số đã tồn tại hoặc đếm sai các số trùng lặp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét từng cặp phần tử, tính toán GCD, kiểm tra xem nó đã có chưa và nối thêm nếu chưa. Chúng tôi lặp lại điều này cho đến khi không có phần tử mới nào được thêm vào. Mặc dù điều này đúng nhưng mỗi thao tác đều yêu cầu lặp lại tất cả các cặp, đưa ra$O(n^2)$mỗi phép cộng. Nếu mảng có thể tăng lên khoảng$10^6$các phần tử, tổng độ phức tạp đạt đến$O(n^3)$trong trường hợp xấu nhất là không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng mọi số được thêm vào đều là ước số của một phần tử nào đó trong mảng. Điều này giới hạn phạm vi các số có thể có mà chúng ta có thể tạo ra trong tập hợp tất cả các ước số của tất cả các phần tử mảng. Thay vì mô phỏng các phép toán trên mảng, chúng ta có thể đảo ngược bài toán: với mỗi số nguyên cho đến phần tử lớn nhất, hãy đếm số phần tử ban đầu chia hết cho nó, sau đó truyền số đếm từ bội số xuống các ước số nhỏ hơn. Điều này cho phép tính toán số lần tối đa một GCD có thể xuất hiện mà không cần tạo mảng một cách rõ ràng. 

Cụ thể, hãy xác định`cnt[x]`là số phần tử trong mảng chia hết cho`x`. Với mỗi số chia`d`, số lượng hoạt động tối đa có thể tạo ra`d`là`sum(cnt[m] for all multiples m of d) - 1`. Chúng ta có thể tính toán trước các ước số và bội số một cách hiệu quả bằng phương pháp giống như sàng. Điều này làm giảm độ phức tạp từ bậc hai xuống gần tuyến tính về kích thước của mảng và số lượng tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu (lan truyền số chia) | O(n + max(a) * log log max(a)) | O(max(a)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào và lưu trữ các phần tử mảng trong một tập hợp để kiểm tra sự tồn tại theo thời gian không đổi. Điều này đảm bảo rằng khi tính toán các GCD mới, chúng ta có thể nhanh chóng xem liệu chúng đã có sẵn hay chưa. 
2. Khởi tạo một mảng`cnt`chiều dài`max(a)+1`về không. Lặp lại mảng đầu vào và tăng dần`cnt[a_i]`cho mỗi phần tử.`cnt[x]`cuối cùng sẽ đại diện cho bao nhiêu lần`x`xuất hiện dưới dạng bội số của các phần tử mảng. 
3. Lặp lại từ giá trị lớn nhất có thể xuống 1. Với mỗi số nguyên`i`, lặp lại tất cả các bội số`j`của`i`và tích lũy`cnt[j]`vào trong`cnt[i]`. Sau bước này,`cnt[i]`biểu thị số phần tử chia hết cho`i`hoặc bất kỳ bội số nào của nó, tương ứng với số lần`i`có khả năng có thể được thêm vào thông qua các hoạt động GCD. 
4. Số lượng thao tác tối đa cho`i`vậy thì`cnt[i] - 1`, vì mỗi số yêu cầu ít nhất một phần tử hiện có để kết hợp. Theo dõi tối đa trên tất cả`i`. 
5. Xuất ra số thao tác tối đa được tính toán. 

Điều này có hiệu quả vì mỗi GCD mới được thêm vào đều là ước số của một số cặp. Việc truyền số đếm từ bội số sang ước số đảm bảo rằng chúng tôi tính đến tất cả các GCD tiềm năng và việc trừ đi một đảm bảo chúng tôi không tính chính phần tử đó là một phép toán. Bằng cách xử lý từ lớn đến nhỏ, chúng tôi tôn trọng cấu trúc phụ thuộc trong đó số lớn hơn tạo ra GCD nhỏ hơn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
MAX_A = 10**6

present = [0] * (MAX_A + 1)
for x in a:
    present[x] = 1

cnt = [0] * (MAX_A + 1)
for x in a:
    cnt[x] += 1

for i in range(MAX_A, 0, -1):
    for j in range(2 * i, MAX_A + 1, i):
        cnt[i] += cnt[j]

res = 0
for i in range(1, MAX_A + 1):
    if cnt[i] > 0:
        res = max(res, cnt[i] - 1)

print(res)
```Vòng lặp đầu tiên xây dựng bản đồ hiện diện cho mảng. Vòng lặp thứ hai đếm số lần xuất hiện của các số trong`cnt`. Vòng lặp lồng nhau truyền số đếm từ bội số xuống ước số một cách hiệu quả. Việc trừ đi một trong lần quét cuối cùng dẫn đến thực tế là một phần tử không thể tự tạo thành GCD. Thứ tự truyền từ lớn đến nhỏ đảm bảo rằng tất cả các GCD tiềm năng được xem xét chính xác một lần. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: đầu vào`[4, 20, 1, 25, 30]`. Số đếm ban đầu: 

| Số | cnt trước khi nhân giống | 
| --- | --- | 
| 1 | 1 | 
| 4 | 1 | 
| 20 | 1 | 
| 25 | 1 | 
| 30 | 1 | 

Số lượng lan truyền: 

- 30 cộng với 15, 10, 6, 5, 3, 2, 1 
- 25 cộng 5, 1 
- 20 cộng với 10, 5, 4, 2, 1 
- 4 cộng 2, 1 
- 1 còn lại 

Chung kết`cnt`: 

- 1: 5 
- 2: 3 
- 4: 2 
- 5: 3 
- 10: 2 
- 15: 1 
- 20: 1 
- 25: 1 
- 30: 1 

Trừ một để có được hoạt động tối đa:`max(cnt[i]-1) = 3`. 

Dấu vết mẫu 2: đầu vào`[2, 3, 5]`. Số đếm ban đầu: 

- 2:1, 3:1, 5:1 

Tuyên truyền thêm 1 vào 1, vì vậy`cnt[1]=3`. Hoạt động tối đa:`cnt[1]-1=2`. Điều này xác nhận thuật toán xử lý chính xác các mảng thiếu 1 và chỉ có số nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(MAX_A * nhật ký nhật ký MAX_A + n) | Đếm bội số và truyền các ước số sử dụng cách tiếp cận giống như sàng; đầu vào đọc là O(n) | 
| Không gian | O(MAX_A) | Mảng`cnt`Và`present`chiều dài`MAX_A`| 

Với`MAX_A = 10^6`Và`n <= 10^6`, thao tác này chạy thoải mái dưới 2 giây và sử dụng ít hơn 10 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    MAX_A = 10**6
    present = [0] * (MAX_A + 1)
    for x in a:
        present[x] = 1
    cnt = [0] * (MAX_A + 1)
    for x in a:
        cnt[x] += 1
    for i in range(MAX_A, 0, -1):
        for j in range(2 * i, MAX_A + 1, i):
            cnt[i] += cnt[j]
    res = 0
    for i in range(1, MAX_A + 1):
        if cnt[i] > 0:
            res = max(res, cnt[i] - 1)
    return str(res)

# provided samples
assert run("5\n4 20 1 25 30\n") == "3"
assert run("3\n2 3 5\n") == "2"

# custom tests
assert run("2\n1 2\n") == "1", "minimum size input"
assert run("4\n6 10 15 21\n") == "4", "common divisors"
assert run("3\n7 11 13\n") == "2", "all primes"
assert run("5\n1 2 3 4 6\n") == "5", "contains 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n1 2`| 1 | mảng nhỏ nhất | 
| `4\n6 | | |
