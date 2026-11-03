---
title: "CF 111B - Petya và các ước số"
description: "Chúng ta được yêu cầu xử lý một chuỗi truy vấn, mỗi truy vấn được xác định bởi hai số nguyên x và y. Với mỗi truy vấn, chúng ta cần đếm xem có bao nhiêu ước dương của x không chia bất kỳ số y nào ngay trước x trong dãy, tức là các số x-y, x-y+1, ..., x-1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 111
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 85 (Div. 1 Only)"
rating: 1900
weight: 111
solve_time_s: 170
verified: true
draft: false
---

[CF 111B - Petya và Số chia](https://codeforces.com/problemset/problem/111/B) 

**Xếp hạng:** 1900 
**Tas:** tìm kiếm nhị phân, cấu trúc dữ liệu, lý thuyết số 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xử lý một chuỗi truy vấn, mỗi truy vấn được xác định bởi hai số nguyên x và y. Đối với mỗi truy vấn, chúng ta cần đếm xem có bao nhiêu ước dương của x **không** chia bất kỳ số y nào ngay trước x trong dãy, tức là các số x-y, x-y+1, ..., x-1. Nếu y bằng 0, câu trả lời đơn giản là số ước của x, vì không có số nào đứng trước để kiểm tra. 

Đầu vào cho phép tối đa 100.000 truy vấn và x có thể lớn tới 100.000. Điều này có nghĩa là bất kỳ giải pháp nào lặp lại một cách ngây thơ trên tất cả các ước số của x và kiểm tra chúng với tối đa x số cho mỗi truy vấn sẽ quá chậm. Một cách tiếp cận mạnh mẽ là lặp lại mọi ước số cho mỗi truy vấn dựa trên tất cả các số có liên quan trước đó có thể đạt tới 10^10 phép toán, điều này không thể quản lý được. Chúng ta cần một giải pháp tính toán trước hoặc giảm bớt công việc bằng cách sử dụng các thuộc tính lý thuyết số. 

Các trường hợp biên không rõ ràng bao gồm các truy vấn có y = 0, là một tình huống rút gọn và các giá trị x nhỏ, đặc biệt là x = 1 hoặc số nguyên tố, trong đó các ước số là tầm thường nhưng các điều kiện biên có thể thất bại nếu chúng ta không xử lý chúng một cách chính xác. Một trường hợp cạnh khác là khi y vượt quá x, nhưng các ràng buộc đảm bảo 0 ≤ y ≤ i-1, vì vậy chúng ta sẽ không bao giờ truy cập các chỉ số âm của các giá trị x trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các ước của x cho mỗi truy vấn và sau đó kiểm tra từng ước số với các số y trước đó để xem liệu nó có chia hết bất kỳ số nào trong số chúng hay không. Đối với mỗi truy vấn, điều này có thể liên quan đến tối đa O(sqrt(x)) ước số lên đến O(y) số trước đó, trong trường hợp xấu nhất đạt đến O(n * sqrt(x) * n) ≈ 10^10 hoạt động. Điều này rõ ràng là quá chậm. 

Quan sát quan trọng là chúng ta chỉ cần biết liệu ước d của x có xuất hiện dưới dạng ước số của bất kỳ số y nào trước đó hay không. Thay vì kiểm tra từng ước số với từng số trước đó, chúng ta có thể duy trì cho mỗi ước số một bản ghi chỉ mục truy vấn cuối cùng nơi nó xuất hiện dưới dạng ước số. Sau đó, đối với truy vấn hiện tại, chúng tôi chỉ cần kiểm tra xem lần xuất hiện cuối cùng này có nằm trong y truy vấn cuối cùng hay không. Nếu đúng như vậy thì chúng ta bỏ qua việc đếm d; nếu không, chúng tôi tính d. Điều này làm giảm công việc liệt kê các ước của x và thực hiện tra cứu đơn giản trên mỗi ước số, điều này khả thi vì mỗi x có nhiều nhất là O(sqrt(x)) ước. 

Cách tiếp cận này tận dụng tính toán trước và bộ nhớ đệm, chuyển đổi một cách hiệu quả việc kiểm tra bậc hai có khả năng thành một tập hợp kiểm tra thời gian không đổi cho mỗi ước số. Nó là một ứng dụng cổ điển của quá trình tiền xử lý theo lý thuyết số kết hợp với việc theo dõi trạng thái thông minh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * sqrt(x) * n) | O(n) | Quá chậm | 
| Theo dõi số chia | O(n * sqrt(x)) | O(max_x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các ước số của tất cả các số có giá trị x lớn nhất có thể. Chúng ta có thể làm điều này bằng cách lặp từ 1 đến max_x và thêm i làm ước số cho tất cả các bội số của i. Điều này mang lại khả năng xử lý trước O(max_x * log(max_x)). Đối với mỗi truy vấn, chúng ta có thể truy xuất các ước của x ngay lập tức. 
2. Khởi tạo một mảng hoặc từ điển,`last_seen`, để lưu trữ chỉ mục truy vấn cuối cùng trong đó mỗi ước số xuất hiện. 
3. Xử lý các truy vấn một cách tuần tự. Đối với truy vấn i có giá trị x và y, lặp qua tất cả các ước d của x. 
4. Với mỗi ước số d, hãy kiểm tra`last_seen[d]`. Nếu nó nhỏ hơn i-y, điều này có nghĩa là d không xuất hiện trong các số y trước đó và chúng ta sẽ tăng số đếm. 
5. Cập nhật`last_seen[d]`tới i cho tất cả các ước của x, ghi lại chỉ mục truy vấn hiện tại. 
6. In số lượng truy vấn hiện tại. 

Điều này hoạt động vì sự bất biến`last_seen[d]`luôn lưu trữ chỉ mục truy vấn gần đây nhất trong đó d là ước số để đảm bảo rằng mọi ước số xuất hiện trong phạm vi x-y đến x-1 sẽ bị bỏ qua một cách chính xác. Bằng cách theo dõi các chỉ số thay vì các số đầy đủ trước đó, chúng ta tránh được việc kiểm tra số chia lặp đi lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX_X = 100000

# Precompute divisors for all numbers up to MAX_X
divisors = [[] for _ in range(MAX_X + 1)]
for i in range(1, MAX_X + 1):
    for j in range(i, MAX_X + 1, i):
        divisors[j].append(i)

def main():
    n = int(input())
    last_seen = [0] * (MAX_X + 1)
    
    for idx in range(1, n + 1):
        x_str, y_str = input().split()
        x = int(x_str)
        y = int(y_str)
        
        count = 0
        for d in divisors[x]:
            if last_seen[d] <= idx - y - 1:
                count += 1
            last_seen[d] = idx
        print(count)

if __name__ == "__main__":
    main()
```các`divisors`array cho phép chúng ta truy xuất ngay lập tức tất cả các ước của x, tránh phải tính toán lại. Chúng tôi sử dụng lập chỉ mục truy vấn dựa trên 1 để`idx - y - 1`xác định chính xác liệu lần xuất hiện cuối cùng của số chia có nằm ngoài y truy vấn trước đó hay không. Đang cập nhật`last_seen[d]`sau khi kiểm tra đảm bảo tính đúng đắn cho các truy vấn tiếp theo. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu:```
6
4 0
3 1
5 2
6 2
18 4
10000 3
```| Truy vấn | x | y | Ước của x | Đã tính | lần cuối sau truy vấn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 0 | 1,2,4 | 3 | 1→1, 2→1, 4→1 | 
| 2 | 3 | 1 | 1,3 | 1 | 1→2, 3→2 | 
| 3 | 5 | 2 | 1,5 | 1 | 1→3, 5→3 | 
| 4 | 6 | 2 | 1,2,3,6 | 2 | 1→4,2→4,3→4,6→4 | 
| 5 | 18 | 4 | 1,2,3,6,9,18 | 2 | 1→5,2→5,3→5,6→5,9→5,18→5 | 
| 6 | 10000 | 3 | 1,2,4,5,... | 22 | cập nhật tương ứng | 

Dấu vết này chứng tỏ rằng`last_seen`bỏ qua một cách chính xác các ước số xuất hiện quá gần đây và đếm tất cả các ước số khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(MAX_X)) | Mỗi truy vấn xử lý các ước của x, tối đa là O(sqrt(x)) cho mỗi truy vấn. | 
| Không gian | O(MAX_X) | Lưu trữ`last_seen`và danh sách ước số cho tất cả các số lên tới MAX_X. | 

Với n 10^5 và x 10^5, O(n * sqrt(MAX_X)) ≈ 10^7 hoạt động, nằm trong giới hạn 5 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided sample
assert run("6\n4 0\n3 1\n5 2\n6 2\n18 4\n10000 3\n") == "3\n1\n1\n2\n2\n22", "sample 1"

# minimum x
assert run("1\n1 0\n") == "1", "minimum x"

# all primes
assert run("3\n2 0\n3 1\n5 2\n") == "2\n1\n1", "primes"

# y=0 edge
assert run("2\n12 0\n7 0\n") == "6\n2", "y zero case"

# large x
assert run("1\n100000 0\n") == "36", "large x with all divisors counted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 0 | 1 | giá trị x nhỏ nhất, ước số tầm thường | 
| 3\n2 0\n3 1\n5 2 | 2\n1\n1 | số nguyên tố và y > 0 bỏ qua | 
| 2\n12 0\n7 0 | 6\n2 | y = 0 xử lý phím tắt | 
| 1\n100000 0 | 36 | tính đúng đắn trên x lớn | 

## Vỏ cạnh 

Ví dụ: đối với truy vấn có y = 0`x = 12, y = 0`, thuật toán lấy tất cả các ước số [1,2,3,4,6,12] và đếm chúng mà không cần kiểm tra các truy vấn trước đó.`last_seen`cập nhật chỉ mục truy vấn hiện tại. Điều này tạo ra đầu ra 6, điều này đúng. 

Ví dụ: đối với các truy vấn có y > 0`x = 5, y = 2`sau các truy vấn 4 và 3 trước đó,`last_seen[1] = 3`Và`last_seen[5] = 2`. Vì `idx - y - 1 = 3 - 2 - 1 =
