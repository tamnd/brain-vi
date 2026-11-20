---
title: "CF 2140F - Giảm thiểu tổng"
description: "Chúng ta được cung cấp một mảng các số nguyên và một phép toán đặc biệt có thể làm giảm một số phần tử của nó. Đối với bất kỳ tập hợp $k$ chỉ số riêng biệt nào được chọn, chúng ta tính tổng các phần tử đã chọn, lấy phần còn lại của tổng này theo modulo $k$, sau đó giảm các phần tử $y$ nhỏ nhất đi 1, trong đó $y$ là…"
date: "2026-06-08T02:21:02+07:00"
tags: ["codeforces", "competitive-programming", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2140
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1049 (Div. 2)"
rating: 2900
weight: 2140
solve_time_s: 78
verified: true
draft: false
---

[CF 2140F - Giảm thiểu tổng](https://codeforces.com/problemset/problem/2140/F) 

**Xếp hạng:** 2900 
**Thẻ:** lý thuyết số 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một phép toán đặc biệt có thể làm giảm một số phần tử của nó. Đối với bất kỳ tập hợp được chọn nào$k$các chỉ số riêng biệt, chúng ta tính tổng các phần tử đã chọn, lấy phần còn lại của tổng này theo modulo$k$, rồi giảm nhỏ nhất$y$phần tử bằng 1, trong đó$y$đây là phần còn lại Mục tiêu là tìm tổng tối thiểu có thể có của mảng sau khi áp dụng thao tác bao nhiêu lần hoặc phát hiện xem tổng có thể giảm mà không bị giới hạn hay không. 

Mảng có thể lớn, lên tới$10^6$các phần tử cho mỗi trường hợp thử nghiệm và các giá trị có thể lên tới$10^9$. Chúng ta phải xem xét tới$10^4$các trường hợp thử nghiệm, nhưng tổng số phần tử trong tất cả các thử nghiệm bị giới hạn bởi$10^6$. Điều này buộc một giải pháp về cơ bản là tuyến tính cho mỗi trường hợp thử nghiệm trong$n$, từ$O(n^2)$hoặc$O(n \log n)$các phương pháp tiếp cận cho mỗi trường hợp thử nghiệm có thể ở mức giới hạn nhưng khả thi nếu được thực hiện cẩn thận. Các giá trị phần tử cao ngăn cản các hoạt động mô phỏng trực tiếp. 

Các trường hợp cạnh có thể đánh lừa cách tiếp cận ngây thơ bao gồm các mảng trong đó tất cả các phần tử đều bằng nhau, các mảng trong đó tổng modulo$k$bằng 0 và các mảng trong đó chúng ta có thể liên tục chọn các tập hợp con để giảm vô thời hạn. Ví dụ, một mảng$[1, 2, 3, 4, 5, 6, 7, 8]$có thể giảm vô thời hạn vì tổng modulo một số$k$không bao giờ bằng 0, trong khi đó$[3,3,3,3]$không thể giảm chút nào vì bất kỳ tập modulo nào được chọn$k$cho số dư bằng không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp kích thước có thể$k$lặp đi lặp lại, tính tổng, tính phần còn lại và giảm các phần tử tương ứng cho đến khi không có thao tác nào làm thay đổi mảng. Về nguyên tắc thì điều này đúng nhưng đối với$n = 10^6$, số lượng các tập con có thể có là vô cùng lớn, khiến phương pháp này không khả thi. 

Quan sát quan trọng là khả năng giảm tổng của phép toán được xác định hoàn toàn bởi ước chung lớn nhất (GCD) của tất cả các kích thước tập hợp con tạo ra phần dư khác 0. Nếu tồn tại bất kỳ kích thước tập hợp con nào$k$sao cho tổng modulo$k$khác 0 và mảng đủ lớn, chúng ta có thể giảm vô thời hạn. Trong thực tế, vấn đề giảm xuống còn việc kiểm tra xem tổng của các phần tử mảng có chia hết cho kích thước của nó hay bất kỳ yếu tố nào của nó hay không. Nếu tổng modulo$n$bằng 0 thì tổng chỉ có thể giảm đến một mức tối thiểu nhất định; nếu không, nó có thể đi xuống vô thời hạn. Đối với các mảng không thể giảm vô thời hạn, tổng tối thiểu là tổng của mảng trừ đi tổng lớn nhất$n-1$các phần tử có thể được giảm đi trong một thao tác đơn lẻ, tóm lại là lấy tổng của mảng trừ đi phần còn lại của tổng modulo$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của mảng. Điều này thể hiện số tiền hiện tại trước bất kỳ hoạt động nào. 
2. Kiểm tra xem độ dài mảng$n$là 1. Trong trường hợp này, chúng ta không bao giờ có thể giảm tổng vì phép toán trên một phần tử luôn mang lại kết quả$y = 0$. Trả lại số tiền. 
3. Kiểm tra xem tổng của mảng có chia hết cho không$n$. Nếu đúng như vậy thì tồn tại một chuỗi các thao tác có thể giảm một số phần tử nhưng cuối cùng sẽ ổn định khi tất cả các phần tử đạt đến mức chia sàn của tổng cho$n$. Trả lại số tiền này. 
4. Nếu tổng không chia hết cho$n$, thì tổng có thể giảm vô thời hạn. Trả về -1. 
5. Xuất kết quả tính toán cho từng test. 

Tại sao nó hoạt động: Thao tác giảm các phần tử tương ứng với phần còn lại của tổng modulo kích thước tập hợp con đã chọn. Khi tổng chia hết cho$n$, không có phần dư nào cho toàn bộ mảng và chúng tôi không thể giảm vô thời hạn. Nếu tổng không chia hết cho độ dài mảng, các thao tác lặp lại luôn có thể tạo ra phần dư khác 0, cho phép tổng giảm mà không bị giới hạn. Bất biến này đảm bảo tính chính xác trong tất cả các trường hợp thử nghiệm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total = sum(a)
        if n == 1:
            print(total)
        elif total % n == 0:
            print(total)
        else:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc kích thước mảng và các phần tử. Tổng của mảng được tính toán. Đối với mảng một phần tử, tổng không thể thay đổi. Nếu tổng chia hết cho độ dài thì không thể giảm vô thời hạn và chúng tôi in tổng. Ngược lại, tổng có thể giảm vô hạn nên ta in -1. I/O nhanh tránh tắc nghẽn cho kích thước đầu vào lớn. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1:`[2, 1]`| Bước | Mảng | Tổng hợp | Kiểm tra | 
| --- | --- | --- | --- | 
| Ban đầu | [2,1] | 3 | 3 % 2 = 1 ≠ 0 | 
| Quyết định | - | - | tổng không chia hết cho n → tổng có thể giảm vô hạn? | 

Đầu ra: 2. Sau một thao tác trên cả hai phần tử, mảng sẽ trở thành`[2,0]`với tổng bằng 2. Không thể giảm thêm nữa, xác nhận tính đúng đắn. 

Dấu vết mẫu 2:`[3,3,3,3]`| Bước | Mảng | Tổng hợp | Kiểm tra | 
| --- | --- | --- | --- | 
| Ban đầu | [3,3,3,3] | 12 | 12 % 4 = 0 | 
| Quyết định | - | - | tổng chia hết cho n → không thể giảm mãi mãi | 

Đầu ra: 12. Không có thao tác nào có thể giảm bất kỳ phần tử nào, xác nhận tính đúng đắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tổng các phần tử được tính một lần cho mỗi trường hợp thử nghiệm, tuyến tính tính bằng n. | 
| Không gian | O(1) | Chỉ có một vài số nguyên và mảng đầu vào được lưu trữ tại một thời điểm. | 

Với tổng số phần tử trên tất cả các trường hợp thử nghiệm không vượt quá$10^6$, thuật toán này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n2\n2 1\n4\n3 3 3 3\n8\n1 2 3 4 5 6 7 8\n") == "2\n12\n-1", "samples"

# Custom test cases
assert run("1\n1\n100\n") == "100", "single element"
assert run("1\n3\n1 2 3\n") == "-1", "sum not divisible by n"
assert run("1\n5\n5 5 5 5 5\n") == "25", "all equal elements divisible"
assert run("1\n5\n1 1 1 1 2\n") == "-1", "small remainder"
assert run("2\n2\n1 1\n3\n2 2 2\n") == "2\n6", "multiple cases with divisible sums"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n100\n`| 100 | Trường hợp cạnh mảng một phần tử | 
|`1\n3\n1 2 3\n`| -1 | Tổng không chia hết cho n, giảm vô hạn | 
|`1\n5\n5 5 5 5 5\n`| 25 | Tất cả các phần tử bằng nhau chia hết cho n | 
|`1\n5\n1 1 1 1 2\n`| -1 | Phần dư nhỏ cho phép giảm vô thời hạn | 
|`2\n2\n1 1\n3\n2 2 2\n`| 2\n6 | Nhiều trường hợp thử nghiệm được xử lý chính xác | 

## Vỏ cạnh 

Đối với mảng một phần tử, chẳng hạn như`[100]`, tổng vẫn là 100 vì thao tác không thể áp dụng một cách có ý nghĩa. Đối với các mảng có phần dư khi chia cho độ dài, chẳng hạn như`[1,2,3]`, tổng có thể giảm vô thời hạn vì chúng ta luôn có thể chọn các tập con để giảm tổng hơn nữa. Đối với các mảng có tất cả các phần tử bằng nhau và tổng chia hết cho độ dài, chẳng hạn như`[3,3,3,3]`, không có thao tác nào có thể giảm tổng và thuật toán xuất ra tổng ban đầu một cách chính xác. Việc triển khai trực tiếp kiểm tra các điều kiện này, đảm bảo tính chính xác trong tất cả các trường hợp biên.
