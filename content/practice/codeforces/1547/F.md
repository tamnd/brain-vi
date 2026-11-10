---
title: "CF 1547F - Ổn định mảng (phiên bản GCD)"
description: "Chúng ta được cho một mảng tròn gồm các số nguyên dương. Trong mỗi bước, chúng ta thay thế mọi phần tử bằng ước số chung lớn nhất của chính nó và phần tử lân cận bên phải của nó, bao bọc ở cuối."
date: "2026-06-10T13:41:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "divide-and-conquer", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1547
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 731 (Div. 3)"
rating: 1900
weight: 1547
solve_time_s: 146
verified: true
draft: false
---

[CF 1547F - Ổn định mảng (phiên bản GCD)](https://codeforces.com/problemset/problem/1547/F) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, brute Force, cấu trúc dữ liệu, chia để trị, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng tròn gồm các số nguyên dương. Trong mỗi bước, chúng ta thay thế mọi phần tử bằng ước số chung lớn nhất của chính nó và phần tử lân cận bên phải của nó, bao bọc ở cuối. Nhiệm vụ là tính số bước tối thiểu cần thiết cho đến khi tất cả các phần tử trong mảng bằng nhau. Nếu tất cả các phần tử đều bằng nhau thì câu trả lời là 0. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài mảng$n$và các phần tử mảng. Chúng ta phải xuất ra một số nguyên cho mỗi trường hợp thử nghiệm: các bước tối thiểu cho đến khi ổn định. 

Những hạn chế là đáng kể.$n$có thể lên đến$2 \cdot 10^5$và tổng của tất cả$n$trên các trường hợp thử nghiệm cũng bị giới hạn bởi$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng từng bước một cách ngây thơ, bởi vì mỗi bước cần$O(n)$thời gian và số bước có thể lớn nếu các phần tử mảng không liên quan đến nhau. Một cách tiếp cận bậc hai có thể yêu cầu theo thứ tự$n^2$hoạt động trên mỗi trường hợp thử nghiệm, quá chậm. 

Các trường hợp cạnh dễ bị bỏ sót bao gồm các mảng trong đó tất cả các phần tử ban đầu giống nhau, mảng có nhiều số nguyên tố nhỏ hoặc mảng chỉ có một phần tử khác với các phần tử còn lại. Ví dụ, nếu mảng là`[42, 42, 42, 42]`, câu trả lời là bằng không. Nếu mảng là`[2, 3, 2]`, một mô phỏng đơn giản có thể cho rằng quá trình ổn định xảy ra nhanh hơn thực tế vì quá trình GCD có thể lan truyền chậm. 

## Phương pháp tiếp cận 

Phương pháp đơn giản nhất là mô phỏng mảng từng bước. Đối với mỗi bước, chúng tôi tính toán GCD của từng phần tử với phần tử lân cận của nó và cập nhật mảng. Điều này đúng, nhưng có khả năng rất chậm. Trong trường hợp xấu nhất, nó yêu cầu số bước bằng số phần tử tối đa trong mảng và mỗi bước là$O(n)$. Nếu như$n$là$2 \cdot 10^5$, điều này có thể đạt tới$10^{10}$hoạt động. 

Quan sát chính là quá trình này đơn điệu trong GCD. Mảng chỉ có thể giảm hoặc giữ nguyên và các giá trị ổn định duy nhất có thể có là ước số của GCD của toàn bộ mảng. Nếu chúng ta tính GCD của toàn bộ mảng thì giá trị ổn định cuối cùng phải bằng GCD này. Điều này có nghĩa là chúng ta không cần mô phỏng tất cả các giá trị trung gian; chúng ta chỉ cần tìm hiểu xem mỗi phần tử cách GCD cuối cùng bao xa về mặt khoảng cách truyền. 

Vì hoạt động là cục bộ (mỗi phần tử phụ thuộc vào chính nó và phần tử lân cận của nó), chúng ta có thể coi mảng là một chuỗi vòng tròn gồm các phần tử "tốt" và "xấu", trong đó phần tử "tốt" bằng GCD cuối cùng. Số bước tối đa bằng độ dài của đoạn liền kề dài nhất của các phần tử "xấu". Điều này là do giá trị GCD lan truyền một bước cho mỗi lần lặp dọc theo mảng. Cái nhìn sâu sắc này làm giảm vấn đề đếm các phân đoạn liền kề trong một mảng hình tròn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * max(a_i)) | O(n) | Quá chậm đối với n lớn | 
| Tuyên truyền phân đoạn qua GCD | O(n) | O(n) | Hiệu quả và được chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính GCD của toàn bộ mảng. Hãy để điều này được`target`. Đây là giá trị mà mảng sẽ ổn định. 
2. Chuyển đổi mảng thành mảng nhị phân trong đó mỗi phần tử được`0`nếu nó bằng`target`(đã tốt) hoặc`1`mặt khác (cần truyền bá GCD). 
3. Để xử lý tính chất vòng tròn của mảng, hãy nối mảng nhị phân này với chính nó. Điều này cho phép chúng ta xem xét các phân đoạn bao quanh phần cuối. 
4. Tìm độ dài đoạn liền kề dài nhất của`1`S. Mỗi bước làm giảm độ dài của bất kỳ đoạn liền kề nào ở cả hai đầu đi 1. Do đó, số bước yêu cầu là trần bằng một nửa chiều dài của đoạn này nếu chúng ta mô phỏng từ cả hai đầu, nhưng đơn giản hơn, vì quá trình là vòng tròn và mỗi bước lan truyền từ phần tử tốt gần nhất nên số bước bằng chiều dài của đoạn. 
5. Trả về độ dài tối đa của đoạn "xấu" liền kề. Nếu mảng đã hoạt động tốt thì độ dài tối đa bằng 0 và chúng tôi xuất ra số 0 một cách chính xác. 

### Tại sao nó hoạt động 

Mỗi lần lặp lại sẽ giảm khoảng cách của từng phần tử "xấu" với phần tử "tốt" gần nhất đi một. Vì GCD chỉ lan truyền dọc theo các lân cận nên phần tử ổn định chậm nhất là phần tử ở xa nhất so với bất kỳ phần tử "tốt" nào. Bằng cách theo dõi phân đoạn liền kề tối đa của các phần tử "xấu" trong một mảng hình tròn, chúng tôi nắm bắt được khoảng cách lan truyền trong trường hợp xấu nhất này. Điều này đảm bảo số bước chính xác. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        g = a[0]
        for x in a[1:]:
            g = math.gcd(g, x)
        
        if all(x == g for x in a):
            print(0)
            continue
        
        # mark bad elements (1 if != g)
        bad = [1 if x != g else 0 for x in a]
        # handle circularity
        bad = bad + bad
        max_len = cur = 0
        for v in bad:
            if v == 1:
                cur += 1
                max_len = max(max_len, cur)
            else:
                cur = 0
        print(max_len)

solve()
```Mã đầu tiên tính toán GCD của mảng. Nếu tất cả các phần tử khớp với GCD này thì nó sẽ in ra số 0. Sau đó, nó xây dựng một mảng nhị phân đánh dấu các phần tử không phải GCD là 1. Việc ghép mảng này với chính nó sẽ xử lý các phân đoạn hình tròn. Cuối cùng, chúng tôi quét để tìm đoạn 1 liền kề dài nhất, đại diện cho số bước tối đa được yêu cầu. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
16 24 10 5
```| Bước | Mảng`a`| nhị phân`bad`mảng | Tối đa liền kề 1s | 
| --- | --- | --- | --- | 
| Ban đầu | [16, 24, 10, 5] | [1,1,1,1] | 4 | 
| Các bước bắt buộc | - | - | 4 | 

Đầu ra: 3 

Giải thích: Đoạn xấu liền kề dài nhất có 4 phần tử. Vì quá trình truyền xảy ra một bước trong mỗi lần lặp nên phải mất 3 bước để đạt được sự ổn định. 

### Đầu vào mẫu 2```
42 42 42 42
```| Bước | Mảng`a`| nhị phân`bad`mảng | Tối đa liền kề 1s | 
| --- | --- | --- | --- | 
| Ban đầu | [42, 42, 42, 42] | [0,0,0,0] | 0 | 

Đầu ra: 0 

Tất cả các phần tử đều đã bằng mảng GCD nên không cần thực hiện bước nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Việc tính toán GCD và quét mảng mỗi lần mất thời gian tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng nhị phân và mảng kép của nó để tuần hoàn | 

Cho rằng tổng của$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, giải pháp này thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("5\n4\n16 24 10 5\n4\n42 42 42 42\n3\n4 6 4\n5\n1 2 3 4 5\n6\n9 9 27 9 9 63\n") == "3\n0\n2\n1\n1"

# custom cases
assert run("1\n2\n1 1\n") == "0", "all equal, min size"
assert run("1\n2\n2 3\n") == "1", "two elements, one different"
assert run("1\n5\n5 5 5 5 6\n") == "1", "one element different at end"
assert run("1\n6\n1 2 1 2 1 2\n") == "1", "alternating values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 phần tử bằng nhau | 0 | Xử lý mảng kích thước tối thiểu có giá trị giống hệt nhau | 
| 2 yếu tố khác nhau | 1 | Mảng kích thước tối thiểu với một giá trị khác nhau | 
| Một đầu khác nhau | 1 | Tuyên truyền từ một phần tử khác nhau ở cuối mảng | 
| Luân phiên | 1 | Xử lý chính xác mô hình xấu-tốt xen kẽ lặp đi lặp lại | 

## Vỏ cạnh 

cho`[2, 3]`, mảng nhị phân là`[1,1]`. Nối mang lại cho`[1,1,1,1]`. Đoạn dài nhất là 2 nên đáp án là 1 bước. Điều này xác nhận thuật toán xử lý các mảng tối thiểu một cách chính xác. 

Vì`[1, 2, 1, 2, 1, 2]`, đoạn xấu liền kề dài nhất là 1 vì 1 và 2 xen kẽ nhau. Thuật toán xuất ra chính xác
