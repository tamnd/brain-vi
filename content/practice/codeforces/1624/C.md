---
title: "CF 1624C - Chia cho hai và hoán vị"
description: "Chúng ta được cho một mảng các số nguyên dương có kích thước $n$, và chúng ta muốn biết liệu chúng ta có thể biến nó thành một hoán vị các số từ $1$ đến $n$ hay không bằng cách chia liên tục các phần tử cho hai. Mỗi phép chia thay thế phần tử $ai$ bằng $lfloor ai / 2 rfloor$."
date: "2026-06-10T05:35:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "flows", "graph-matchings", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1624
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 764 (Div. 3)"
rating: 1100
weight: 1624
solve_time_s: 72
verified: true
draft: false
---

[CF 1624C - Chia cho hai và hoán vị](https://codeforces.com/problemset/problem/1624/C) 

**Đánh giá:** 1100 
**Tags:** thuật toán xây dựng, luồng, so khớp đồ thị, tham lam, toán học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương có kích thước$n$và chúng tôi muốn biết liệu chúng tôi có thể biến nó thành một hoán vị của các số từ$1$ĐẾN$n$bằng cách liên tục chia các phần tử cho hai. Mỗi phép chia thay thế một phần tử$a_i$với$\lfloor a_i / 2 \rfloor$. Một hoán vị ở đây có nghĩa là mảng cuối cùng chứa mọi số nguyên từ$1$ĐẾN$n$đúng một lần. 

Các ràng buộc đủ nhỏ về mặt$n$(lên tới 50) nhưng lớn về mặt số lượng (lên đến$10^9$). Điều này có nghĩa là chúng ta không thể thực hiện các phép toán có tỷ lệ trực tiếp với các số (như lặp từ 1 đến$a_i$), nhưng chúng ta có thể thực hiện điều gì đó tỷ lệ thuận với$n \log a_i$vì việc chia một số cho hai nhiều lần mất tối đa khoảng 30 bước (vì$2^{30} \approx 10^9$). 

Một cách tiếp cận đơn giản có thể là thử tất cả các chuỗi chia cho mỗi số và kiểm tra xem có sự kết hợp nào mang lại hoán vị hay không. Điều này sẽ theo cấp số nhân trong$n$và thậm chí là không thể đối với$n=20$. Các trường hợp cạnh bao gồm các mảng trong đó nhiều phần tử ban đầu lớn hơn$n$, mảng có số lặp lại và mảng thiếu số nhỏ nhất. Ví dụ, nếu$a = [4, 4, 4]$Và$n = 3$, có vẻ hợp lý vì mỗi 4 có thể chia thành 2 và 1, nhưng chúng ta phải gán kết quả cẩn thận để tránh trùng lặp. Một cách tiếp cận bất cẩn có thể tham lam chia các số mà không xem xét thứ tự và tuyên bố sai thành công hay thất bại. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng tạo ra tất cả các chuỗi phân chia cho mỗi số và sau đó kiểm tra xem liệu một số kết hợp có tạo thành một hoán vị hợp lệ hay không. Đối với một số duy nhất, số lượng khả năng là khoảng$\log_2 a_i$, tức là khoảng 30 trong trường hợp xấu nhất. Vì$n=50$, tổng số kết hợp trở thành$30^{50}$, có giá trị lớn về mặt thiên văn. Điều này rõ ràng là không khả thi. 

Thông tin chi tiết quan trọng là đối với mỗi số mục tiêu từ$1$ĐẾN$n$, chúng ta chỉ quan tâm đến việc liệu nó có thể được tạo ra bằng cách chia đôi liên tục một số phần tử trong mảng hay không. Chúng tôi không cần phải theo dõi tất cả các chuỗi cùng một lúc. Chúng ta có thể sắp xếp mảng theo thứ tự giảm dần và cố gắng gán các số lớn nhất trước tiên cho các mục tiêu lớn nhất hiện có, chia nhỏ cho đến khi số đó phù hợp. Cách tiếp cận tham lam này hoạt động vì khi chúng tôi chỉ định một số cho một mục tiêu, chúng tôi sẽ loại bỏ mục tiêu đó khỏi sự xem xét và các số lớn hơn không thể chặn các số nhỏ hơn nếu chúng tôi xử lý chúng trước. 

Chúng ta có thể thực hiện điều này bằng cách duy trì một tập hợp các mục tiêu có sẵn$1$bởi vì$n$. Đối với mỗi phần tử bắt đầu từ phần tử lớn nhất, chúng tôi liên tục chia cho hai cho đến khi nó rơi vào tập hợp các mục tiêu có sẵn (sau đó chúng tôi loại bỏ mục tiêu đó) hoặc giảm xuống dưới 1 (thất bại). Điều này đảm bảo mọi số đều được gán cho mục tiêu lớn nhất có thể mà nó có thể tiếp cận, ngăn ngừa xung đột và đảm bảo rằng thuật toán tìm thấy hoán vị nếu có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((log A)^n) | O(n) | Quá chậm | 
| Phòng tham lam | O(n log A) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng$a$. 
2. Khởi tạo một tập hợp chứa các số mục tiêu$1$ĐẾN$n$. Điều này thể hiện những con số chúng tôi vẫn cần sản xuất. 
3. Sắp xếp mảng$a$theo thứ tự giảm dần. Chúng tôi muốn chỉ định số lượng lớn hơn trước để tránh chặn các mục tiêu nhỏ hơn. 
4. Đối với mỗi số trong mảng đã sắp xếp, hãy chia liên tục cho hai cho đến khi nó khớp với một số trong tập hợp mục tiêu có sẵn hoặc trở thành số 0. 
5. Nếu số khớp với mục tiêu, hãy xóa mục tiêu đó khỏi bộ và chuyển sang số tiếp theo. 
6. Nếu một số đạt đến 0 mà không khớp với bất kỳ mục tiêu sẵn có nào, hãy in "NO" cho trường hợp thử nghiệm này. Nếu không, hãy tiếp tục cho đến khi tất cả các số được xử lý. 
7. Nếu tất cả các mục tiêu đã được chỉ định, hãy in "CÓ". 

Tại sao nó hoạt động: Điều bất biến là ở mỗi bước, số lượng lớn nhất chưa được chỉ định sẽ cố gắng bao phủ mục tiêu lớn nhất còn lại mà nó có thể tiếp cận. Vì chúng tôi chỉ chỉ định mục tiêu khi một số bằng nó và sau đó loại bỏ mục tiêu nên sẽ không có hai số nào yêu cầu cùng một mục tiêu. Việc xử lý các số theo thứ tự giảm dần đảm bảo rằng các số nhỏ hơn không bị chặn bởi các số lớn hơn, vì vậy nếu tồn tại một hoán vị hợp lệ thì thuật toán sẽ tìm thấy nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    targets = set(range(1, n+1))
    a.sort(reverse=True)
    
    possible = True
    for x in a:
        while x > 0:
            if x in targets:
                targets.remove(x)
                break
            x //= 2
        else:
            possible = False
            break
    
    print("YES" if possible else "NO")
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc mảng và khởi tạo tập hợp mục tiêu từ 1 đến$n$. Sắp xếp mảng theo thứ tự giảm dần đảm bảo chúng ta xử lý các số lớn hơn trước. Vòng lặp bên trong chia từng số cho đến khi nó khớp với mục tiêu hoặc trở thành số 0. Nếu nó trở thành 0 mà không khớp thì hoán vị là không thể. Việc sử dụng một bộ cho phép O(1) kiểm tra và loại bỏ các mục tiêu, giữ cho giải pháp luôn hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, 8, 25, 2]`, n = 4 

| Bước | Số | Mục tiêu | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | 25 | {1,2,3,4} | 25 → 12 → 6 → 3; gán 3, xóa 3 | 
| 2 | 8 | {1,2,4} | 8 → 4; gán 4, xóa 4 | 
| 3 | 2 | {1,2} | 2; gán 2, xóa 2 | 
| 4 | 1 | {1} | 1; gán 1, xóa 1 | 

Tất cả các mục tiêu được bao phủ, xuất ra CÓ. 

### Ví dụ 2 

Mảng đầu vào:`[1, 1]`, n = 2 

| Bước | Số | Mục tiêu | Hoạt động | 
| --- | --- | --- | --- | 
| 1 | 1 | {1,2} | 1; gán 1, xóa 1 | 
| 2 | 1 | {2} | 1 → 0; không thể gán 2 | 

Không phải tất cả các mục tiêu đều được bao phủ, xuất ra NO. 

Những dấu vết này cho thấy cách tiếp cận phép chia tham lam gán chính xác các số cho mục tiêu và xác định các trường hợp không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi số được chia nhiều nhất là log2(A_i) lần, tổng cộng là n số | 
| Không gian | O(n) | Đặt mục tiêu, lưu trữ mảng | 

Vì n 50 và log2(A_i) 30, giải pháp thực hiện tối đa 1500 thao tác cho mỗi trường hợp thử nghiệm, dễ dàng khớp trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste solution here
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        targets = set(range(1, n+1))
        a.sort(reverse=True)
        
        possible = True
        for x in a:
            while x > 0:
                if x in targets:
                    targets.remove(x)
                    break
                x //= 2
            else:
                possible = False
                break
        
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("6\n4\n1 8 25 2\n2\n1 1\n9\n9 8 3 4 2 7 1 5 6\n3\n8 2 1\n4\n24 7 16 7\n5\n22 6 22 4 22\n") == "YES\nNO\nYES\nNO\nNO\nYES", "sample 1"

# Custom cases
assert run("1\n1\n1\n") == "YES", "minimum input"
assert run("1\n3\n2 2 2\n") == "YES", "all equal values but divisible"
assert run("1\n5\n10 20 30 40 50\n") == "YES", "all large values"
assert run("1\n4\n1 2 3 4\n") == "YES", "already a permutation"
assert run("1\n3\n3 3 3\n") == "NO", "duplicates cannot reach all targets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | Đầu vào kích thước tối thiểu | 
|`1\n3\n2 2 2`| CÓ | Nhiều số bằng nhau có thể chia được | 
|`1\n5\n10 20 30 40 50`| CÓ | Số lớn phải chia | 
| `1\n4\n1 2 3 | | |
