---
title: "CF 103492D - Kiểm tra tính nguyên thủy"
description: "Chúng ta được cho một số nguyên dương x và chúng ta xây dựng một giá trị bằng cách sử dụng các số nguyên tố xung quanh nó. Đầu tiên, chúng ta định nghĩa f(x) là số nguyên tố nhỏ nhất lớn hơn x, do đó nó là số nguyên tố tiếp theo sau x."
date: "2026-07-03T06:12:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103492
codeforces_index: "D"
codeforces_contest_name: "China Collegiate Programming Contest 2021, Qualification Round (Online), Rematch"
rating: 0
weight: 103492
solve_time_s: 42
verified: true
draft: false
---

[CF 103492D - Kiểm tra tính nguyên thủy](https://codeforces.com/problemset/problem/103492/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương x và chúng ta xây dựng một giá trị bằng cách sử dụng các số nguyên tố xung quanh nó. Đầu tiên, chúng ta định nghĩa f(x) là số nguyên tố nhỏ nhất lớn hơn x, do đó nó là số nguyên tố tiếp theo sau x. Sau đó, chúng ta áp dụng lại ý tưởng tương tự cho f(x), cho ra f(f(x)), nó đơn giản là số nguyên tố sau số nguyên tố tiếp theo. 

Từ hai số nguyên tố này, chúng ta tạo thành một số g(x) bằng cách lấy trung bình cộng của chúng, nghĩa là chúng ta cộng chúng và chia cho 2. Nhiệm vụ là quyết định xem bản thân số g(x) này có phải là số nguyên tố hay không. 

Dữ liệu đầu vào bao gồm tối đa 100.000 truy vấn độc lập và mỗi x có thể lớn tới 10^18. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng kiểm tra tính nguyên tố hoặc tìm kiếm các số nguyên tố xung quanh x cho từng truy vấn riêng biệt. Bất kỳ giải pháp nào cũng phải tránh tính toán các số nguyên tố lớn cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện ở các giá trị rất nhỏ. Với x = 1, các số nguyên tố tiếp theo là 2 và 3, do đó g(x) trở thành 2, là số nguyên tố. Với x = 2, các số nguyên tố tiếp theo là 3 và 5, do đó g(x) trở thành 4, không phải là số nguyên tố. Những đầu vào nhỏ bé này đã gợi ý rằng câu trả lời có thể chỉ phụ thuộc vào cấu trúc của một vài số nguyên tố đầu tiên chứ không phụ thuộc vào độ lớn của x. 

Khó khăn chính là x có thể cực kỳ lớn, do đó mọi suy luận đều phải phụ thuộc vào tính chất của các số nguyên tố liên tiếp hơn là các giá trị rõ ràng của chúng. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để tính g(x) là tính f(x) bằng cách tìm kiếm lên trên cho đến khi tìm thấy số nguyên tố, sau đó tính f(f(x)) theo cách tương tự, và cuối cùng kiểm tra xem giá trị trung bình của chúng có phải là số nguyên tố hay không. Điều này đúng về mặt định nghĩa, nhưng nó ngay lập tức gặp phải giới hạn tính toán. Đối với x lên tới 10^18, việc tìm số nguyên tố tiếp theo yêu cầu phải kiểm tra tính nguyên tố chặt chẽ và có khả năng quét các khoảng trống lớn và việc thực hiện việc này đối với 100.000 truy vấn trở nên không khả thi. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến giá trị số của các số nguyên tố mà chỉ quan tâm đến mối quan hệ cấu trúc của chúng. Đặt p1 = f(x) và p2 = f(f(x)). Đây là những số nguyên tố liên tiếp. Giá trị g(x) chỉ đơn giản là trung điểm giữa hai số nguyên tố liên tiếp. 

Bây giờ chúng ta hỏi một câu hỏi có cấu trúc đơn giản hơn: trung điểm của hai số nguyên tố liên tiếp có thể là số nguyên tố không? Nếu trung điểm m như vậy là số nguyên tố thì chúng ta sẽ có p1 < m < p2, nghĩa là tồn tại một số nguyên tố hoàn toàn nằm giữa hai số nguyên tố liên tiếp. Điều này mâu thuẫn với định nghĩa về tính liên tục trừ khi khoảng đó cực kỳ nhỏ. Ngoại lệ duy nhất xảy ra ở phần đầu của dãy số nguyên tố, trong đó các số nguyên tố 2 và 3 liên tiếp có điểm giữa là 2. 

Vì vậy, toàn bộ vấn đề tập trung vào việc kiểm tra xem cặp số nguyên tố liên tiếp có phải là (2, 3) hay không, điều này chỉ xảy ra khi x đủ nhỏ để số nguyên tố tiếp theo là 2. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm Brute Force Prime | O(T · sqrt(N) hoặc tệ hơn) | O(1) | Quá chậm | 
| Quan sát cấu trúc | O(T) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi x, hãy xác định xem số nguyên tố tiếp theo sau x có phải là 2 hay không. Điều này chỉ xảy ra khi x = 1, vì 2 là số nguyên tố nhỏ nhất và không có số nguyên dương nào nhỏ hơn 2 có số nguyên tố tiếp theo khác. 
2. Nếu x = 1 thì ta xác định ngay hai số nguyên tố liên tiếp là 2 và 3 nên trung điểm là 2, là số nguyên tố. 
3. Với mọi x ≥ 2, số nguyên tố tiếp theo f(x) ít nhất là 3, số nguyên tố tiếp theo f(f(x)) ít nhất là 5, tạo thành các số nguyên tố lẻ liên tiếp. 
4. Trung điểm của hai số nguyên tố lẻ liên tiếp lớn hơn (2, 3) không thể là số nguyên tố vì nó nằm giữa hai số nguyên tố liên tiếp, điều này không thể xảy ra trừ khi nó bằng một trong hai số đó. 
5. Chỉ xuất ra CÓ khi x = 1, nếu không thì xuất ra NO. 

### Tại sao nó hoạt động

Đặt p1 = f(x) và p2 = f(f(x)). Đây là những số nguyên tố liên tiếp. Giả sử g(x) = (p1 + p2) / 2 là số nguyên tố. Khi đó g(x) nằm hoàn toàn giữa p1 và p2. Điều đó có nghĩa là có một số nguyên tố tồn tại giữa hai số nguyên tố liên tiếp, mâu thuẫn với định nghĩa của chúng. Trường hợp duy nhất khiến lý do này bị phá vỡ là khi khoảng bắt đầu ở 2 và 3, trong đó điểm giữa trùng với một điểm cuối về mặt hành vi do khoảng cách nhỏ nhất có thể có. Như vậy chỉ có x = 1 sống sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        x = int(input())
        if x == 1:
            out.append("YES")
        else:
            out.append("NO")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh sự sụp đổ của toàn bộ lý luận cơ bản thành một điều kiện cấu trúc duy nhất. Hàm f(x) và số nguyên tố thứ hai f(f(x)) không bao giờ được tính toán rõ ràng, bởi vì đặc tính liên quan duy nhất của chúng là chúng tạo thành các số nguyên tố liên tiếp. Quyết định rút gọn lại việc kiểm tra xem chúng ta có đang ở trong trường hợp duy nhất trong đó các số nguyên tố đó là (2, 3), tương ứng chính xác với x = 1 hay không. 

Không cần xử lý cạnh ngoài điều kiện này vì tất cả x ≥ 2 đều tạo ra kết quả cấu trúc giống nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1: x = 1 

| Bước | f(x) | f(f(x)) | g(x) | 
| --- | --- | --- | --- | 
| 1 | 2 | 3 | (2 + 3) / 2 = 2 | 

Điểm giữa bằng 2, là số nguyên tố, nên đầu ra là CÓ. Đây là cấu hình duy nhất có các số nguyên tố liên tiếp là (2, 3), cho phép điểm giữa là số nguyên tố. 

### Ví dụ 2: x = 2 

| Bước | f(x) | f(f(x)) | g(x) | 
| --- | --- | --- | --- | 
| 1 | 3 | 5 | (3 + 5) / 2 = 4 | 

Kết quả là 4, là tổng hợp, nên đầu ra là NO. Điều này phản ánh một thực tế là khi các số nguyên tố bắt đầu từ số 3 trở đi thì điểm giữa giữa các số nguyên tố liên tiếp không thể là số nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi truy vấn là một so sánh duy nhất | 
| Không gian | O(1) | Chỉ sử dụng một lượng lưu trữ không đổi | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện công việc liên tục trên mỗi trường hợp thử nghiệm ngay cả khi T đạt 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    input = _sys.stdin.readline
    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            x = int(input())
            out.append("YES" if x == 1 else "NO")
        print("\n".join(out))

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    res = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return res

# provided samples (interpreted)
assert run("2\n1\n2\n") == "YES\nNO"

# minimum input
assert run("1\n1\n") == "YES"

# small non-trivial
assert run("1\n3\n") == "NO"

# large value
assert run("1\n1000000000000000000\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 → 1 | CÓ | trường hợp hợp lệ nhỏ nhất | 
| 1 → 2 | KHÔNG | trường hợp từ chối ngay lập tức | 
| 1 → lớn | KHÔNG | tính đúng đắn cho giới hạn trên | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là khi x = 1, trong đó chuỗi nguyên tố bắt đầu bằng 2 và 3. Đối với x = 1, thuật toán trực tiếp đưa ra CÓ, khớp với điểm giữa được tính toán (2 + 3) / 2 = 2. 

Với x ≥ 2, việc thực thi luôn rơi vào nhánh NO. Ví dụ, với x = 2, f(x) = 3 và f(f(x)) = 5, cho điểm giữa 4, điểm này bị bác bỏ một cách chính xác. 

Không có vấn đề ranh giới nào phát sinh nữa vì quyết định không phụ thuộc vào tính chất số học của các số nguyên tố lớn, mà chỉ phụ thuộc vào danh tính của cấu hình nguyên tố nhỏ nhất có thể.
