---
title: "CF 1363A - Lựa chọn lẻ"
description: "Chúng ta được cho một mảng các số nguyên và phải chọn chính xác x phần tử từ nó. Các phần tử được chọn có thể đến từ bất kỳ vị trí nào trong mảng. Câu hỏi đặt ra là liệu có tồn tại một tập hợp chính xác x phần tử có tổng là số lẻ hay không."
date: "2026-06-11T12:28:14+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1363
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 646 (Div. 2)"
rating: 1200
weight: 1363
solve_time_s: 134
verified: true
draft: false
---

[CF 1363A - Lựa chọn số lẻ](https://codeforces.com/problemset/problem/1363/A) 

**Đánh giá:** 1200 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và phải chọn chính xác`x`các yếu tố từ nó. Các phần tử được chọn có thể đến từ bất kỳ vị trí nào trong mảng. Câu hỏi đặt ra là liệu có tồn tại sự lựa chọn chính xác`x`các phần tử có tổng là số lẻ. 

Giá trị thực tế của các con số không quan trọng ngoài tính chẵn lẻ của chúng. Mọi số nguyên đều là số lẻ hoặc số chẵn và tính chẵn lẻ của tổng chỉ phụ thuộc vào số lượng số lẻ được bao gồm. Một tổng là số lẻ khi và chỉ khi nó chứa một số lẻ các phần tử lẻ. 

Những hạn chế là nhỏ. Mỗi test chứa tối đa 1000 số và có tối đa 100 test. Thậm chí một`O(n²)`lời giải có thể chấp nhận được, nhưng việc quan sát tính chẵn lẻ cho phép một lời giải tuyến tính thậm chí còn đơn giản hơn. Không cần lập trình động hoặc liệt kê tập hợp con. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một mảng chỉ chứa các số chẵn:```
1
4 3
2 4 6 8
```Câu trả lời đúng là`No`. Bất kỳ tổng số chẵn nào vẫn là số chẵn. Một giải pháp bất cẩn chỉ kiểm tra xem`x`thật kỳ quặc sẽ thất bại ở đây. 

Hãy xem xét một mảng chỉ chứa các số lẻ:```
1
4 2
1 3 5 7
```Câu trả lời đúng là`No`. Chọn hai số lẻ sẽ được số chẵn. Một số triển khai cho rằng không chính xác rằng có sẵn số lẻ là đủ. 

Hãy xem xét trường hợp chúng ta phải lấy tất cả các yếu tố:```
1
3 3
1 2 3
```Tổng số tiền là`6`, là số chẵn nên đáp án là`No`. Khi`x = n`, chỉ có một lựa chọn khả thi. 

Một trường hợp tinh vi khác xảy ra khi có đủ số lẻ để tạo thành tổng số lẻ, nhưng không đủ phần tử tổng để lấp đầy các ô còn lại bằng một số chẵn lẻ hợp lệ:```
1
3 3
1 2 2
```Chúng ta phải có cả ba yếu tố. Tổng số tiền là`5`, vậy câu trả lời là`Yes`. Tính khả thi phụ thuộc vào cả số phần tử lẻ và yêu cầu chọn chính xác`x`các phần tử. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các tập hợp con có kích thước`x`và kiểm tra xem có số nào trong số chúng có tổng lẻ hay không. Điều này đúng vì nó xem xét mọi lựa chọn có thể. Vấn đề là số lượng tập hợp con như vậy. Trong trường hợp xấu nhất chúng ta sẽ phải kiểm tra$$\binom{1000}{500}$$các lựa chọn khác nhau, có quy mô lớn về mặt thiên văn và hoàn toàn không thể thực hiện được trong thời hạn. 

Quan sát quan trọng là chỉ có tính chẵn lẻ mới quan trọng. Cho phép`odd`là số phần tử lẻ trong mảng và`even`là số phần tử chẵn. 

Tổng được chọn là số lẻ chính xác khi số phần tử lẻ được chọn là số lẻ. Giả sử chúng ta chọn`k`các yếu tố lẻ. Sau đó:$$k + \text{chosen even elements} = x$$Vì các phần tử được chọn còn lại phải là phần tử chẵn nên chúng ta cần:$$x - k \le even$$và cả$$k \le odd$$giá trị`k`phải là số lẻ vì tổng cuối cùng phải là số lẻ. 

Vấn đề trở thành: có tồn tại số nguyên lẻ không`k`như vậy:$$1 \le k \le odd$$Và$$x-k \le even$$Và$$k \le x$$Có nhiều nhất`x`các giá trị có thể có của`k`, vì vậy việc kiểm tra tất cả các giá trị lẻ là chuyện nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(n,x)) | O(x) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu phần tử trong mảng là số lẻ và bao nhiêu phần tử là số chẵn. 
2. Thử mọi giá trị lẻ`k`từ`1`lên tới`min(odd, x)`. 
3. Điều trị`k`là số phần tử lẻ mà chúng ta muốn chọn. Từ`k`thật kỳ lạ, mọi lựa chọn hợp lệ đều sử dụng chính xác`k`các phần tử lẻ sẽ có tổng lẻ. 
4. Tính toán`need_even = x - k`. Đây là những vị trí còn lại phải được lấp đầy bằng các phần tử chẵn. 
5. Kiểm tra xem`need_even`là không âm và liệu`need_even <= even`. Nếu cả hai điều kiện đều đúng, chúng ta có thể xây dựng một lựa chọn chính xác`x`các phần tử. 
6. Nếu có gì kỳ lạ`k`thỏa mãn điều kiện, in`Yes`. 
7. Nếu không có ứng viên nào làm việc thì in`No`. 

### Tại sao nó hoạt động 

Tính chẵn lẻ của một tổng chỉ phụ thuộc vào số lượng số lẻ có trong đó. Một tổng là số lẻ khi số đó là số lẻ. 

Thuật toán kiểm tra mọi số lẻ có thể`k`có thể xuất hiện trong một lựa chọn hợp lệ. Đối với mỗi như vậy`k`, nó xác minh xem có tồn tại đủ các phần tử lẻ để cung cấp những phần tử đó hay không`k`số lượng và các phần tử chẵn tồn tại đủ để điền vào phần còn lại`x-k`các vị trí. 

Nếu thuật toán tìm thấy một phương án khả thi`k`, một lựa chọn thực tế có thể được xây dựng trực tiếp từ những số đếm đó. Nếu mỗi lẻ`k`không thành công thì không có lựa chọn nào có thể có cả kích thước yêu cầu và tính chẵn lẻ lẻ. Vì mọi nghiệm hợp lệ đều phải tương ứng với một giá trị lẻ nào đó của`k`, thuật toán hoàn tất và không thể bỏ sót lời giải nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        odd = sum(v % 2 for v in a)
        even = n - odd

        possible = False

        for k in range(1, min(odd, x) + 1, 2):
            need_even = x - k
            if need_even <= even:
                possible = True
                break

        ans.append("Yes" if possible else "No")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của giải pháp tính các phần tử lẻ và chẵn. Không có gì khác về các giá trị quan trọng. 

Vòng lặp kết thúc`k`kiểm tra mọi số lẻ có thể có của các số lẻ đã chọn. Kích thước bước là`2`, điều này đảm bảo rằng mọi ứng viên`k`thật kỳ quặc. 

điều kiện`need_even <= even`xác minh rằng có đủ số chẵn để hoàn thành việc lựa chọn. Chúng tôi không cần kiểm tra riêng`need_even >= 0`bởi vì vòng lặp chỉ xem xét`k <= x`. 

Ngay khi tìm thấy một cấu hình khả thi, câu trả lời sẽ là`Yes`, do đó vòng lặp kết thúc sớm. 

Không cần xử lý đặc biệt đối với các giá trị lớn vì chỉ có số lượng được thao tác. Tràn số nguyên là không thể trong Python và dù sao thì số lượng cũng không bao giờ vượt quá 1000. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
2 2
51 50
```| Bước | lẻ | thậm chí | k | cần_thậm chí | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| Đếm số chẵn lẻ | 1 | 1 | - | - | - | 
| Hãy thử k=1 | 1 | 1 | 1 | 1 | Có | 

Thuật toán nhận thấy rằng có thể chọn một số lẻ và một số chẵn. Tổng kết quả có tính chẵn lẻ lẻ, nên câu trả lời là`Yes`. 

### Ví dụ 2 

đầu vào:```
1
3 3
101 102 103
```| Bước | lẻ | thậm chí | k | cần_thậm chí | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| Đếm số chẵn lẻ | 2 | 1 | - | - | - | 
| Hãy thử k=1 | 2 | 1 | 1 | 2 | Không | 
| Không còn giá trị k lẻ | 2 | 1 | - | - | - | 

Để chọn ba phần tử chỉ có một phần tử lẻ được chọn, chúng ta cần hai phần tử chẵn, nhưng chỉ tồn tại một phần tử. Chọn ba phần tử lẻ là không thể vì chỉ có hai số lẻ. Không có cấu hình khả thi nào tồn tại, vì vậy câu trả lời là`No`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm số chẵn lẻ chiếm ưu thế trong công việc | 
| Không gian | O(1) | Chỉ có một vài quầy được lưu trữ | 

Mảng được quét một lần cho mỗi trường hợp thử nghiệm. Với tối đa 1000 phần tử cho mỗi trường hợp thử nghiệm và tối đa 100 trường hợp thử nghiệm, tổng khối lượng công việc rất nhỏ so với giới hạn sẵn có. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))

        odd = sum(v % 2 for v in a)
        even = n - odd

        ok = False
        for k in range(1, min(odd, x) + 1, 2):
            if x - k <= even:
                ok = True
                break

        out.append("Yes" if ok else "No")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""5
1 1
999
1 1
1000
2 1
51 50
2 2
51 50
3 3
101 102 103
"""
) == """Yes
No
Yes
Yes
No
"""

# minimum size, odd element
assert run(
"""1
1 1
1
"""
) == "Yes\n", "single odd number"

# minimum size, even element
assert run(
"""1
1 1
2
"""
) == "No\n", "single even number"

# all even values
assert run(
"""1
5 3
2 4 6 8 10
"""
) == "No\n", "all sums remain even"

# all odd values, even selection size
assert run(
"""1
4 2
1 3 5 7
"""
) == "No\n", "two odd numbers give even sum"

# boundary case x=n
assert run(
"""1
3 3
1 2 2
"""
) == "Yes\n", "must take all elements"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`| Có | Trường hợp lẻ hợp lệ nhỏ nhất | 
|`1 1 / 2`| Không | Trường hợp nhỏ nhất không thể | 
| Tất cả các số chẵn | Không | Tổng lẻ không thể hình thành | 
| Tất cả các số lẻ có số chẵn x | Không | Số chẵn sẽ cho tổng chẵn | 
|`x=n`trường hợp | Có | Xử lý lựa chọn toàn mảng bắt buộc | 

## Vỏ cạnh 

### Tất cả các phần tử đều là số chẵn 

đầu vào:```
1
4 3
2 4 6 8
```Thuật toán tính`odd = 0`Và`even = 4`. Vì không có giá trị lẻ của`k`để thử, vòng lặp không bao giờ tìm thấy cấu hình hợp lệ. Đầu ra là`No`. 

Điều này phù hợp với thực tế toán học rằng bất kỳ tổng nào của các số chẵn đều là số chẵn. 

### Tất Cả Các Phần Tử Là Lẻ, Kích Thước Lựa Chọn Là Chẵn 

đầu vào:```
1
4 2
1 3 5 7
```Thuật toán tính`odd = 4`Và`even = 0`. 

Nó cố gắng`k = 1`, yêu cầu một phần tử chẵn, nhưng không tồn tại. 

Không có số lẻ lớn hơn`k`được cho phép bởi vì`k > x`. 

Đầu ra là`No`. 

Mọi lựa chọn có đúng hai số lẻ đều có tổng chẵn nên kết quả là đúng. 

### Phải chọn mọi phần tử 

đầu vào:```
1
3 3
1 2 3
```Thuật toán tính`odd = 2`Và`even = 1`. 

Đang cố gắng`k = 1`đòi hỏi hai phần tử chẵn, nhưng chỉ tồn tại một phần tử. 

Không có giá trị lẻ nào khác có thể. 

Đầu ra là`No`. 

Vì cả ba phần tử phải được chọn và tổng của chúng là`6`, một số chẵn, không có lựa chọn hợp lệ nào tồn tại. 

### Có Chính Xác Một Số Lẻ 

đầu vào:```
1
5 5
1 2 2 2 2
```Thuật toán tính`odd = 1`Và`even = 4`. 

Đang cố gắng`k = 1`cho`need_even = 4`, có sẵn. 

Đầu ra là`Yes`. 

Việc chọn một phần tử lẻ cùng với cả bốn phần tử chẵn sẽ tạo ra một tổng lẻ, xác nhận rằng logic đếm xử lý chính xác trường hợp biên này.
