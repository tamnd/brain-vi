---
title: "CF 1364A - XXXXX"
description: "Chúng ta được cung cấp một mảng các số nguyên và một số $x$ mà Ehab không thích. Mục tiêu là tìm độ dài của mảng con liền kề dài nhất có tổng không chia hết cho $x$. Mỗi trường hợp thử nghiệm đưa ra một mảng mới và một $x$ mới."
date: "2026-06-11T12:23:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1364
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 649 (Div. 2)"
rating: 1200
weight: 1364
solve_time_s: 114
verified: true
draft: false
---

[CF 1364A - XXXXX](https://codeforces.com/problemset/problem/1364/A) 

**Đánh giá:** 1200 
**Tags:** Brute Force, cấu trúc dữ liệu, lý thuyết số, hai con trỏ 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và một số$x$mà Ehab không thích. Mục tiêu là tìm độ dài của mảng con liền kề dài nhất có tổng không chia hết cho$x$. Mỗi trường hợp thử nghiệm đưa ra một mảng mới và một$x$. Nếu không có mảng con nào thỏa mãn điều kiện thì kết quả đầu ra sẽ là$-1$. 

Mảng đầu vào có thể lên tới$10^5$các phần tử dài và các giá trị trong mảng và$x$có thể đi lên$10^4$. Với tối đa 5 trường hợp thử nghiệm mỗi lần chạy, điều này có nghĩa là giải pháp của chúng tôi phải ở mức tuyến tính tệ nhất trong$n$cho mỗi trường hợp thử nghiệm, bởi vì cách tiếp cận bậc hai đơn giản kiểm tra tất cả các mảng con sẽ liên quan đến khoảng$5 \times 10^5 \times 10^5 / 2$hoạt động trong trường hợp xấu nhất, quá chậm trong giới hạn thời gian 1 giây. 

Các trường hợp phức tạp bao gồm các mảng trong đó tất cả các phần tử là bội số của$x$. Ví dụ, đối với$a = [2, 4, 6]$Và$x = 2$, mọi tổng của mảng con đều chia hết cho 2, do đó kết quả đầu ra phải là$-1$. Một trường hợp khác là khi tổng của toàn bộ mảng không chia hết cho$x$; trong trường hợp đó, mảng con dài nhất là toàn bộ mảng. Mảng có số 0 hoặc một phần tử bằng$x$cũng có thể cư xử khác đi và cần xử lý cẩn thận để tránh những sai lầm riêng lẻ. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ kiểm tra tất cả các mảng con một cách rõ ràng, tính tổng của chúng và kiểm tra khả năng chia hết cho$x$. Điều này hoạt động hợp lý, nhưng số lượng mảng con là$O(n^2)$và tính toán mỗi tổng sẽ mất$O(1)$với tổng tiền tố. Tổng số hoạt động vẫn$O(n^2)$, điều này là không thể thực hiện được đối với$n = 10^5$. 

Cái nhìn sâu sắc quan trọng là tính chất chia hết có tính toàn cục. Nếu tổng của toàn bộ mảng không chia hết cho$x$, thì mảng đầy đủ đã là câu trả lời. Nếu tổng đó chia hết cho$x$, thì chúng ta phải loại bỏ ít nhất một phần tử để phá vỡ tính chia hết này. Bởi vì mảng con phải tiếp giáp nhau nên loại bỏ tiền tố hoặc hậu tố cho đến khi phần tử đầu tiên không chia hết cho$x$đạt được sẽ cho mảng con dài nhất có thể. Chúng ta không cần kiểm tra tất cả các mảng con; chúng ta chỉ cần phần tử đầu tiên và cuối cùng không chia hết cho$x$. Quan sát này làm giảm giải pháp từ thời gian bậc hai sang thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$x$và sau đó đọc mảng$a$. 
2. Tính tổng của mảng. Nếu tổng này không chia hết cho$x$, quay lại ngay$n$, chiều dài đầy đủ của mảng. 
3. Nếu tổng số tiền chia hết cho$x$, quét mảng từ bên trái để tìm phần tử đầu tiên có giá trị không chia hết cho$x$. Lưu trữ chỉ mục của nó dưới dạng`left`. 
4. Quét mảng từ bên phải để tìm phần tử đầu tiên có giá trị không chia hết cho$x$. Lưu trữ chỉ mục của nó dưới dạng`right`. 
5. Nếu không có phần tử nào như vậy tồn tại thì tất cả các phần tử đều chia hết cho$x$và không có mảng con nào có tổng không chia hết cho$x$; trở lại$-1$. 
6. Mặt khác, tính toán độ dài hai mảng con ứng cử viên: loại bỏ tiền tố cho đến`left`sản lượng`n - left - 1`và xóa hậu tố sau`right`sản lượng`right`. Câu trả lời là độ dài tối đa của hai độ dài này. 

Bất biến ở đây là việc loại bỏ các phần tử khỏi một đầu của mảng có tổng chia hết cho$x$cho đến khi đạt đến phần tử không chia hết đảm bảo rằng mảng con còn lại có tổng không chia hết cho$x$. Đây là khoảng thời gian dài nhất có thể vì việc loại bỏ nhiều phần tử hơn sẽ chỉ rút ngắn mảng con một cách không cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))
    
    total = sum(a)
    if total % x != 0:
        print(n)
        continue
    
    left = 0
    while left < n and a[left] % x == 0:
        left += 1
    right = n - 1
    while right >= 0 and a[right] % x == 0:
        right -= 1
    
    if left == n:  # all elements divisible by x
        print(-1)
    else:
        print(max(n - left - 1, right))
```Việc thực hiện theo thuật toán trực tiếp. Trước tiên, chúng tôi xử lý trường hợp toàn mảng. Quét trái và quét phải là các lần quét tuyến tính đơn giản dừng lại ở phần tử không chia hết đầu tiên. Phép tính tối đa xem xét cả hai tùy chọn cho mảng con dài nhất. Điều kiện biên, chẳng hạn như tất cả các phần tử chia hết cho$x$, được xử lý rõ ràng bằng một kiểm tra riêng. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1: 

đầu vào:`3 3` `1 2 3`- Tổng số = 6, chia hết cho 3. 
- Quét trái: phần tử đầu tiên 1 % 3 != 0 tại chỉ số 0. 
- Quét sang phải: phần tử cuối cùng 3 % 3 == 0, thứ hai đến cuối 2 % 3 != 0 tại chỉ số 1. 
- Chiều dài ứng viên:`n - left - 1 = 3 - 0 - 1 = 2`,`right = 1`. Tối đa = 2. 

đầu vào:`3 4` `1 2 3`- Tổng số = 6, không chia hết cho 4. 
- Đáp án = 3, là độ dài đầy đủ của mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi mảng được quét tối đa hai lần, một lần từ trái sang phải và một lần từ phải sang trái. | 
| Không gian | O(1) | Chỉ có một vài chỉ số và tổng số tiền được lưu trữ. | 

Đối với các ràng buộc nhất định, điều này đảm bảo hoàn thành dưới 1 giây ngay cả ở kích thước đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # solution code inline
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        
        total = sum(a)
        if total % x != 0:
            print(n)
            continue
        
        left = 0
        while left < n and a[left] % x == 0:
            left += 1
        right = n - 1
        while right >= 0 and a[right] % x == 0:
            right -= 1
        
        if left == n:
            print(-1)
        else:
            print(max(n - left - 1, right))
    return output.getvalue().strip()

# provided samples
assert run("3\n3 3\n1 2 3\n3 4\n1 2 3\n2 2\n0 6\n") == "2\n3\n-1", "sample 1"

# custom cases
assert run("1\n1 1\n1\n") == "-1", "single element divisible by x"
assert run("1\n1 2\n1\n") == "1", "single element not divisible by x"
assert run("1\n5 2\n2 4 6 8 10\n") == "-1", "all even, x = 2"
assert run("1\n5 3\n3 3 3 3 1\n") == "5", "last element breaks divisibility"
assert run("1\n6 5\n5 5 5 5 5 1\n") == "6", "last element not divisible by x"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | -1 | Phần tử đơn chia hết cho x | 
| 1 2 1 | 1 | Phần tử đơn không chia hết cho x | 
| 5 2 2 4 6 8 10 | -1 | Tất cả các phần tử chia hết cho x | 
| 5 3 3 3 3 3 1 | 5 | Phần tử cuối cùng phá vỡ tính chia hết | 
| 6 5 5 5 5 5 5 1 | 6 | Phần tử cuối cùng không chia hết cho x | 

## Vỏ cạnh 

Khi tất cả các phần tử đều chia hết cho$x$, thuật toán xác định chính xác điều này bằng`left == n`kiểm tra. Đối với mảng chỉ có một phần tử không chia hết cho$x$, thuật toán tính toán`n - left - 1`Và`right`một cách chính xác, chọn mảng con liền kề tối đa thỏa mãn điều kiện. Đối với các mảng mà tổng không chia hết, thuật toán sẽ tránh việc quét không cần thiết và trả về toàn bộ chiều dài mảng.
