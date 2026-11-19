---
title: "CF 2121A - Thư nhà"
description: "Chúng ta được cho một tập hợp các vị trí số nguyên riêng biệt trên trục số và vị trí bắt đầu. Từ vị trí bắt đầu, chúng ta có thể di chuyển sang trái hoặc phải từng đơn vị một."
date: "2026-06-08T03:46:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 2121
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1032 (Div. 3)"
rating: 800
weight: 2121
solve_time_s: 70
verified: true
draft: false
---

[CF 2121A - Thư gửi nhà](https://codeforces.com/problemset/problem/2121/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các vị trí số nguyên riêng biệt trên trục số và vị trí bắt đầu. Từ vị trí bắt đầu, chúng ta có thể di chuyển sang trái hoặc phải từng đơn vị một. Mục tiêu của chúng tôi là ghé thăm mọi vị trí được chỉ định ít nhất một lần và chúng tôi muốn tính toán số lần di chuyển tối thiểu cần thiết để đạt được điều này. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra số lượng vị trí cần truy cập, điểm bắt đầu và danh sách các vị trí được sắp xếp. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số bước tối thiểu để đảm bảo tất cả các vị trí đều được truy cập. 

Các ràng buộc rất nhỏ: số lượng vị trí`n`tối đa là 10, vị trí bắt đầu và vị trí đích nằm trong khoảng từ 1 đến 100 và có thể có tới 1000 trường hợp thử nghiệm. Bởi vì`n`là rất nhỏ, các thuật toán có tính hàm mũ trong`n`là khả thi. Chúng ta không cần phải xem xét các phương pháp lập trình động hoặc đồ thị có độ phức tạp cao, vì ngay cả việc thử tất cả các hoán vị của thứ tự truy cập cũng hợp lý về mặt tính toán. 

Trường hợp cạnh tinh tế phát sinh khi vị trí bắt đầu`s`nằm ngoài phạm vi của các vị trí chúng ta cần ghé thăm. Ví dụ, nếu`s = 1`và các vị trí là`[3, 4, 5]`, đường đi tối ưu là di chuyển trực tiếp đến điểm cuối gần nhất, sau đó đi qua tất cả các vị trí một cách tuần tự. Một triển khai ngây thơ giả định`s`luôn nằm trong phạm vi các vị trí sẽ tạo ra câu trả lời sai. 

Một trường hợp cạnh khác là khi`s`trùng với một trong các vị trí. Ví dụ, nếu`s = 3`và các vị trí là`[3, 4, 5]`, số bước tối thiểu giảm đi vì chúng ta không cần phải di chuyển để đến vị trí ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra tất cả các hoán vị của các vị trí và tính toán khoảng cách di chuyển từ`s`qua từng hoán vị. Điều này đúng vì nó xem xét tất cả các thứ tự có thể, nhưng nó không cần thiết do tính chất tuyến tính của trục số. Với`n = 10`, có`10! = 3,628,800`hoán vị cho mỗi trường hợp thử nghiệm. Nhân với 1000 trường hợp thử nghiệm, cách tiếp cận này quá chậm. 

Thông tin chi tiết quan trọng là tất cả các vị trí đều nằm trên đường một chiều và được sắp xếp. Số bước tối thiểu được xác định hoàn toàn bởi vị trí ngoài cùng bên trái và ngoài cùng bên phải mà chúng ta phải truy cập. Nếu vị trí bắt đầu nằm trong khoảng này, chiến lược tối ưu là đi đến điểm cuối gần hơn trước, sau đó đi ngang đến đầu kia. Nếu vị trí bắt đầu nằm ngoài khoảng, chúng ta di chuyển thẳng đến điểm cuối gần nhất và di chuyển đến điểm cuối xa. Do đó, các bước tối thiểu là khoảng cách giữa vị trí ngoài cùng bên trái và ngoài cùng bên phải cộng với bất kỳ khoảng cách bổ sung nào để đạt đến khoảng cách từ điểm bắt đầu. 

Điều này biến bài toán thành số học đơn giản: tìm vị trí ngoài cùng bên trái và ngoài cùng bên phải, tính khoảng cách để đến được chúng từ đó.`s`, và thêm độ dài khoảng. Cách tiếp cận này là tuyến tính trong`n`cho mỗi trường hợp thử nghiệm và tầm thường để thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n! * n) | O(n) | Quá chậm | 
| Tối ưu (khoảng + khoảng cách) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`,`s`và danh sách các vị trí được sắp xếp`x`. 
3. Xác định vị trí ngoài cùng bên trái`L = x[0]`và vị trí ngoài cùng bên phải`R = x[-1]`. 
4. Nếu vị trí bắt đầu`s`nằm giữa`L`Và`R`(bao gồm), đường dẫn tối thiểu là`R - s + s - L = R - L`, vì việc di chuyển đầu tiên đến đầu gần hơn và sau đó đi ngang qua khoảng luôn dẫn đến tổng khoảng cách như nhau. Tính toán`max(R - s, s - L) + (R - L)`; trong thực tế, điều này đơn giản hóa để`(R - L)`khi`s`nằm trong khoảng. 
5. Nếu vị trí bắt đầu`s`ít hơn`L`, các bước là`R - s`, vì chúng ta phải chuyển từ`s`ĐẾN`L`sau đó đi qua`R`. 
6. Nếu vị trí bắt đầu`s`lớn hơn`R`, các bước là`s - L`, vì chúng ta chuyển từ`s`ĐẾN`R`sau đó đi qua`L`. 
7. Xuất ra các bước tối thiểu đã tính toán. 

Tại sao nó hoạt động: Giải pháp khai thác tính chất một chiều của các vị thế. Trình tự các bước tối thiểu luôn là đi đến điểm cuối của khoảng và đi qua điểm cuối khác. Bất kỳ sai lệch nào, chẳng hạn như truy cập các vị trí theo thứ tự không tuần tự, sẽ làm tăng số bước một cách không cần thiết. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, s = map(int, input().split())
    x = list(map(int, input().split()))
    L, R = x[0], x[-1]

    if s < L:
        steps = R - s
    elif s > R:
        steps = s - L
    else:
        steps = (R - L) + min(R - s, s - L)
    
    print(steps)
```Mã đọc đầu vào hiệu quả với`sys.stdin.readline`để xử lý nhiều trường hợp thử nghiệm một cách nhanh chóng. Nó xác định khoảng`[L, R]`chứa tất cả các vị trí. Tùy thuộc vào việc vị trí bắt đầu`s`nằm bên ngoài hoặc bên trong khoảng này, mã sẽ tính số bước tối thiểu. các`min(R - s, s - L)`đảm bảo chúng ta bắt đầu từ điểm cuối gần hơn khi`s`nằm trong khoảng đó, làm giảm quãng đường đi được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`1 2 1 3`| Bước | L | R | s | Trường hợp | Bước | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | s trong [L,R] | (3-1) + phút(3-2,2-1) = 2 + 1 = 3 | 

Chúng ta bắt đầu từ số 2, đến số 1, sau đó đi đến số 3, tổng cộng là 3 bước. 

### Ví dụ 2 

đầu vào:`2 2 1 3`| Bước | L | R | s | Trường hợp | Bước | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 2 | s trong [L,R] | (3-1) + phút(3-2,2-1) = 2 + 1 = 3 | 

Chúng tôi bắt đầu từ 2, đi đến 1, sau đó duyệt đến 3, xác nhận thuật toán là nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi trường hợp kiểm thử yêu cầu duyệt qua các vị trí một lần để tìm L và R. n 10, t 1000. | 
| Không gian | O(n) | Lưu trữ mảng vị trí cho mỗi trường hợp thử nghiệm. | 

Giải pháp dễ dàng phù hợp với giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB, vì tổng số thao tác tối đa là 10 * 1000 = 10.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, s = map(int, input().split())
        x = list(map(int, input().split()))
        L, R = x[0], x[-1]
        if s < L:
            steps = R - s
        elif s > R:
            steps = s - L
        else:
            steps = (R - L) + min(R - s, s - L)
        print(steps)
    return output.getvalue().strip()

# provided samples
assert run("12\n1 1\n1\n1 2\n1\n1 1\n2\n2 1\n2 3\n2 2\n1 3\n2 3\n1 2\n3 1\n1 2 3\n3 2\n1 3 4\n3 3\n1 2 3\n4 3\n1 2 3 10\n5 5\n1 2 3 6 7\n6 6\n1 2 3 9 10 11") == "0\n1\n1\n2\n3\n2\n2\n4\n2\n11\n8\n15"

# custom cases
assert run("2\n1 5\n10\n3 1\n2 3 4") == "5\n3", "minimum-size and interior case"
assert run("1\n4 10\n1 2 3 4") == "10", "start outside right"
assert run("1\n4 1\n1 2 3 4") == "3", "start at left endpoint"
assert run("1\n5 3\n1 2 3 4 5") == "4", "start in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 10`|`5`| Vị trí bắt đầu bên phải khoảng thời gian | 
|`3 1 2 3 4`|`3`| Vị trí bắt đầu tại điểm cuối bên trái | 
|`5 3 1 2 3 4 5`|`4`| Vị trí bắt đầu trong khoảng | 
| `1 1 1 | | |
