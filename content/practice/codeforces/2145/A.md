---
title: "CF 2145A - Kẹo cho cháu trai"
description: "Monocarp muốn chia đều một số kẹo nhất định cho ba đứa cháu của mình. Anh ta bắt đầu với n chiếc kẹo và cần tính xem anh ta phải mua thêm bao nhiêu chiếc nữa, nếu có, để tổng số kẹo chia hết cho ba."
date: "2026-06-08T01:31:06+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 2145
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 183 (Rated for Div. 2)"
rating: 800
weight: 2145
solve_time_s: 79
verified: true
draft: false
---

[CF 2145A - Kẹo cho cháu trai](https://codeforces.com/problemset/problem/2145/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Monocarp muốn chia đều một số kẹo nhất định cho ba đứa cháu của mình. Anh ấy bắt đầu với`n`kẹo và cần tính xem nếu có thì phải mua thêm bao nhiêu kẹo nữa để tổng số kẹo chia hết cho ba. Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp cho một số nguyên duy nhất`n`. Đầu ra cho mỗi trường hợp thử nghiệm là số nguyên không âm nhỏ nhất`k`như vậy`n + k`có thể chia hết cho ba. 

Các hạn chế là nhỏ:`n`có thể lên tới 100 và có tối đa 100 trường hợp thử nghiệm. Điều này có nghĩa là chúng ta có thể thực hiện các phép tính số học rất đơn giản cho mỗi trường hợp thử nghiệm mà không phải lo lắng về hiệu suất. Không cần cấu trúc dữ liệu hoặc thuật toán phức tạp. 

Các trường hợp cạnh không rõ ràng phát sinh khi`n`đã chia hết cho ba, trong trường hợp đó Monocarp không cần mua thêm kẹo. Ví dụ, nếu`n = 24`, câu trả lời đúng là`0`. Nếu như`n = 7`, Monocarp cần`2`kẹo bổ sung vì`7 + 2 = 9`, chia hết cho ba. Giải pháp bất cẩn, mù quáng thêm số cố định hoặc quên xử lý vụ việc`n % 3 == 0`sẽ tạo ra những câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Đối với mỗi`n`, chúng ta có thể bắt đầu từ số không có kẹo bổ sung và kiểm tra dần dần`n + k`cho đến khi chia hết cho ba. Điều này hiệu quả vì cuối cùng chúng ta sẽ đạt được một số chia hết cho ba, nhưng ngay cả với những ràng buộc nhỏ thì điều này cũng không cần thiết. Nó bao gồm tối đa hai lần kiểm tra bổ sung cho mỗi trường hợp thử nghiệm, điều này không quan trọng đối với vấn đề này, nhưng hiểu biết sâu sắc về số học cho phép chúng tôi giải quyết nó một cách trực tiếp. 

Quan sát quan trọng là để thực hiện`n`chia hết cho 3 thì ta chỉ cần xét số dư`r = n % 3`. Nếu như`r`bằng 0, không cần kẹo. Nếu như`r`là một, chúng ta cần`2`nhiều kẹo hơn, và nếu`r`là hai, chúng ta cần`1`nhiều kẹo hơn. Điều này xuất phát từ số học mô-đun đơn giản: về cơ bản chúng ta đang giải`(n + k) % 3 == 0`cho số không âm nhỏ nhất`k`. 

Do đó, cách tiếp cận tối ưu là tính phần còn lại của`n`modulo ba và trừ nó từ ba, trừ khi phần còn lại bằng 0. Điều này tránh hoàn toàn việc lặp lại không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`, số kẹo Monocarp hiện có. 
3. Tính số dư`r = n % 3`. 
4. Nếu`r`bằng 0, in`0`. Ngược lại, in`3 - r`. Điều này đưa ra số lượng kẹo tối thiểu cần thêm để làm cho tổng số chia hết cho ba. 
5. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: điều bất biến là chúng ta luôn muốn tổng số kẹo chia hết cho ba. Phép toán modulo đo trực tiếp khoảng cách giữa chúng ta và bội số gần nhất của ba. Thêm`3 - r`sửa phần còn lại mà không vượt quá. Điều này đảm bảo rằng tổng số kẹo sẽ chia hết cho ba với số lượng kẹo bổ sung tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    r = n % 3
    print(0 if r == 0 else 3 - r)
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó tính toán phần còn lại của`n`chia cho ba. Nếu phần còn lại bằng 0, nó sẽ xuất ra số 0; nếu không, nó sẽ xuất ra`3 - r`. Không có vấn đề riêng biệt nào vì`n % 3`luôn tạo ra 0, 1 hoặc 2. Sử dụng`3 - r`đảm bảo chúng tôi luôn thêm số lượng kẹo tối thiểu cần thiết. 

## Ví dụ đã hoạt động 

Đối với đầu vào`7`: 

| n | n % 3 | r | 3 - r | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 7 | 1 | 1 | 2 | 2 | 

Thêm 2 viên kẹo sẽ được`9`kẹo, chia hết cho ba, nên mỗi cháu trai được`3`. 

Đối với đầu vào`24`: 

| n | n % 3 | r | 3 - r | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 24 | 0 | 0 | - | 0 | 

Không cần kẹo vì`24`đã chia hết cho ba rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm yêu cầu một thao tác modulo duy nhất và kiểm tra có điều kiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được sử dụng. | 

Cho rằng`t`nhiều nhất là 100 và`n`tối đa là 100, giải pháp này dễ dàng chạy trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    t = int(input())
    for _ in range(t):
        n = int(input())
        r = n % 3
        print(0 if r == 0 else 3 - r)
    
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples
assert run("2\n7\n24\n") == "2\n0", "sample 1"

# custom cases
assert run("3\n1\n2\n3\n") == "2\n1\n0", "small numbers"
assert run("2\n100\n99\n") == "2\n0", "large divisible cases"
assert run("1\n0\n") == "0", "zero candies"
assert run("1\n5\n") == "1", "remainder 2 case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 2, 3 | 2, 1, 0 | số nhỏ và logic dư | 
| 100, 99 | 2, 0 | tính đúng đắn cho số lớn | 
| 0 | 0 | trường hợp kẹo không | 
| 5 | 1 | phần còn lại điều chỉnh 2 | 

## Vỏ cạnh 

cho`n = 0`,`0 % 3`bằng 0, do đó thuật toán đưa ra chính xác`0`. Vì`n = 1`hoặc`n = 2`, thuật toán tính toán`3 - r`BẰNG`2`Và`1`tương ứng, phù hợp với lượng kẹo bổ sung tối thiểu cần thiết. Không cần xử lý đặc biệt ngoài việc tính toán modulo đơn giản. Vì`n`đã chia hết cho ba, thuật toán cho kết quả bằng 0, đảm bảo chúng ta không bao giờ vượt quá. Mỗi trường hợp đặc biệt này đều xác nhận rằng giải pháp dựa trên modulo là mạnh mẽ.
