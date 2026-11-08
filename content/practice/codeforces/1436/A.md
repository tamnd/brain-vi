---
title: "CF 1436A - Sắp xếp lại"
description: "Bài toán trình bày một mảng các số nguyên và hỏi liệu nó có thể được sắp xếp lại sao cho tổng trọng số cụ thể khớp với một giá trị mục tiêu nhất định hay không."
date: "2026-06-11T04:52:08+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1436
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 678 (Div. 2)"
rating: 800
weight: 1436
solve_time_s: 206
verified: true
draft: false
---

[CF 1436A - Sắp xếp lại](https://codeforces.com/problemset/problem/1436/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một mảng các số nguyên và hỏi liệu nó có thể được sắp xếp lại sao cho tổng trọng số cụ thể khớp với một giá trị mục tiêu nhất định hay không. Tổng được định nghĩa là tổng trên tất cả các vị trí bắt đầu$i$tổng từ vị trí đó đến cuối mỗi phần tử chia cho chỉ số dựa trên một của nó. Về mặt khái niệm, mỗi phần tử đóng góp vào nhiều số hạng của tổng thể, trong đó các phần tử trước ảnh hưởng đến nhiều số hạng hơn các phần tử sau. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài của mảng, tổng mục tiêu và chính mảng đó. Kết quả đầu ra phải là "CÓ" nếu tồn tại bất kỳ hoán vị nào của mảng tạo ra tổng chính xác và "KHÔNG" nếu ngược lại. 

Với những ràng buộc$1 \le n \le 100$Và$0 \le a_i, m \le 10^6$, chúng ta có thể thực hiện các phép tính một cách an toàn trong$O(n \log n)$hoặc$O(n^2)$cho mỗi trường hợp thử nghiệm, vì$100^2$hoạt động có thể chấp nhận được. Một giải pháp ngây thơ cố gắng tất cả$n!$hoán vị là không thể thực hiện được. Các trường hợp cạnh cần theo dõi bao gồm mảng số 0, mảng có một phần tử hoặc mục tiêu nhỏ hơn phần tử nhỏ nhất. Ví dụ, với mảng$[0, 0, 0]$và mục tiêu$1$, đầu ra đúng là "KHÔNG". Nếu mảng là$[5]$và mục tiêu là$5$, câu trả lời là "CÓ". 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ tạo ra tất cả$n!$hoán vị và tính tổng của mỗi hoán vị. Mặc dù đúng nhưng điều này không thực tế vì ngay cả đối với$n = 10$, số lượng hoán vị vượt quá 3 triệu. 

Quan sát chính là tổng tuyến tính trong các phần tử mảng. Viết lại công thức cho:$$\sum_{i=1}^{n}\sum_{j=i}^{n} \frac{a_j}{j} = \sum_{j=1}^{n} a_j \sum_{i=1}^{j} \frac{1}{j} = \sum_{j=1}^{n} a_j$$từ$\sum_{i=1}^{j} \frac{1}{j} = 1$. Do đó, tổng có trọng số giảm xuống còn tổng đơn giản của các phần tử mảng. Sự đơn giản hóa đáng chú ý này có nghĩa là tổng không thay đổi khi sắp xếp lại. Sau đó, vấn đề giảm xuống còn việc kiểm tra xem tổng của tất cả các phần tử có bằng mục tiêu hay không. Cái nhìn sâu sắc này biến một vấn đề tổ hợp có vẻ phức tạp thành một phép kiểm tra số học đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | O(n! * n) | O(n) | Quá chậm | 
| Kiểm tra tổng (tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$Và$m$, sau đó đọc mảng độ dài$n$. 
3. Tính tổng của tất cả các phần tử mảng. 
4. So sánh số tiền này với$m$. Nếu chúng bằng nhau thì in ra "CÓ"; nếu không thì in "KHÔNG". 

Tại sao nó hoạt động: công thức đưa ra trong bài toán thu gọn về tổng của các phần tử mảng bất kể thứ tự của chúng. Do đó, nếu tổng khớp với mục tiêu thì mọi hoán vị đều thỏa mãn điều kiện một cách tầm thường. Nếu không thì không có hoán vị nào có thể thỏa mãn điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        if sum(a) == m:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc đầu vào hiệu quả với`sys.stdin.readline`, tính tổng theo thời gian tuyến tính và so sánh trực tiếp với mục tiêu. Các biến được đặt trong phạm vi vòng lặp để tránh sử dụng lại trong các trường hợp thử nghiệm. Việc kiểm tra tổng là chính xác vì tất cả các số đều là số nguyên, tránh các vấn đề về dấu phẩy động. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 8 | 
| một | [2, 5, 1] ​​| 
| tổng(a) | 8 | 
| So sánh | 8 == 8 → CÓ | 

**Mẫu 2** 

| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| m | 4 | 
| một | [0, 1, 2, 3] | 
| tổng(a) | 6 | 
| So sánh | 6 != 4 → KHÔNG | 

Những dấu vết này chứng tỏ rằng yêu cầu duy nhất là tổng của mảng. Tổng bên trong và tổng bên ngoài là bất biến khi sắp xếp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi trường hợp thử nghiệm có tổng tối đa 100 phần tử. | 
| Không gian | O(n) | Lưu trữ mảng cho mỗi trường hợp thử nghiệm. | 

Với$t \le 100$Và$n \le 100$, các phép toán trong trường hợp xấu nhất là 10.000, nằm trong giới hạn. 

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
assert run("2\n3 8\n2 5 1\n4 4\n0 1 2 3\n") == "YES\nNO", "sample 1"

# custom cases
assert run("1\n1 5\n5\n") == "YES", "single element equal"
assert run("1\n1 5\n4\n") == "NO", "single element not equal"
assert run("1\n3 0\n0 0 0\n") == "YES", "all zeros target zero"
assert run("1\n3 1\n0 0 1\n") == "YES", "sum equals target with zero elements"
assert run("1\n3 2\n1 0 0\n") == "NO", "sum does not equal target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 5\n5\n | CÓ | Mảng một phần tử trong đó phần tử bằng đích | 
| 1\n1 5\n4\n | KHÔNG | Mảng một phần tử trong đó phần tử khác với mục tiêu | 
| 1\n3 0\n0 0 0\n | CÓ | Tất cả số không, mục tiêu số không | 
| 1\n3 1\n0 0 1\n | CÓ | Tổng khớp với mục tiêu có số 0 | 
| 1\n3 2\n1 0 0\n | KHÔNG | Tổng không khớp mục tiêu | 

## Vỏ cạnh 

Trường hợp cạnh tối thiểu là một mảng có độ dài bằng một. Việc kiểm tra tổng xử lý việc này một cách tầm thường vì phần tử đơn lẻ phải khớp với mục tiêu. Mảng toàn số 0 hoạt động miễn là mục tiêu bằng 0 và mảng có các số 0 hỗn hợp hoạt động chính xác vì tổng của tất cả các phần tử chính xác là điều quan trọng, bỏ qua thứ tự. Không cần số học dấu phẩy động vì tất cả các phép toán đều là tổng số nguyên.
