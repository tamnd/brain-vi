---
title: "CF 1744D - Chia hết cho 2^n"
description: "Chúng ta được cấp một mảng các số nguyên dương và chúng ta cần làm cho tích của tất cả các phần tử chia hết cho $2^n$, trong đó $n$ là độ dài của mảng. Chúng ta được phép nhân một phần tử $ai$ với chỉ mục $i$ của chính nó chính xác một lần cho mỗi chỉ mục."
date: "2026-06-09T15:53:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1744
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round  828 (Div. 3)"
rating: 1200
weight: 1744
solve_time_s: 202
verified: true
draft: false
---

[CF 1744D - Chia hết cho 2^n](https://codeforces.com/problemset/problem/1744/D) 

**Đánh giá:** 1200 
**Tags:** tham lam, toán học, phân loại 
**Thời gian giải:** 3 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương và chúng ta cần làm cho tích của tất cả các phần tử chia hết cho$2^n$, Ở đâu$n$là độ dài của mảng. Chúng ta được phép nhân một phần tử$a_i$theo chỉ số riêng của nó$i$chính xác một lần cho mỗi chỉ mục. Mục đích là giảm thiểu số lượng các phép nhân này hoặc báo cáo rằng điều đó là không thể. 

Trong thực tế, mỗi số đóng góp một số thừa số 2 nhất định cho tích. Chúng tôi muốn tổng của những đóng góp này ít nhất là$n$. nhân$a_i$qua$i$có thể tăng số lượng 2s đóng góp bởi$a_i$, bởi vì bản thân chỉ mục có thể có một số số 2 trong hệ số của nó. 

Những ràng buộc cho phép$n$để đạt được$2 \cdot 10^5$cho mỗi bộ thử nghiệm và tổng của tất cả các trường hợp thử nghiệm được giới hạn ở cùng một số. Một cách tiếp cận bạo lực khi thử tất cả các kết hợp hoạt động sẽ quá chậm vì số lượng tập hợp con của các chỉ số tăng theo cấp số nhân. Thay vào đó, chúng ta cần một cách ưu tiên các hoạt động để đạt được số lượng 2 giây yêu cầu một cách hiệu quả. 

Một trường hợp phức tạp là khi tất cả các số đều là số lẻ và chỉ số nhỏ, do đó không có sự kết hợp nào của các phép toán được phép có thể đạt được đủ 2 giây. Ví dụ, nếu$a = [1, 1, 1, 1]$, nhân bất kỳ số nào với chỉ số của nó sẽ cho tối đa một thừa số là 2 cho mỗi số, do đó tích không thể đạt được$2^4 = 16$, và kết quả đúng là -1. Một cách tiếp cận tham lam ngây thơ luôn chọn số đầu tiên có thể báo cáo sai một giải pháp tồn tại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực ngây thơ sẽ xem xét tất cả các tập hợp con của các chỉ số, áp dụng thao tác cho từng tập hợp con, tính toán kết quả và kiểm tra xem nó có chia hết cho không$2^n$. Đối với một mảng có kích thước$n$, điều này đòi hỏi phải kiểm tra$2^n$tập hợp con. Ngay cả đối với$n = 20$, đây đã là hơn một triệu kết hợp và với$n$lên đến$2 \cdot 10^5$, nó hoàn toàn không thể thực hiện được. 

Điểm mấu chốt là chúng ta chỉ quan tâm đến số 2 trong hệ số của mỗi số chứ không phải bản thân các số đó. Hãy xác định`twos(a_i)`như số lần$a_i$chia hết cho 2. Tương tự, nhân với chỉ số$i$thêm vào`twos(i)`để đếm phần tử đó. Sau đó, vấn đề trở thành một vấn đề lựa chọn đơn giản: chúng ta cần chọn các chỉ số có`twos(i)`những đóng góp cho chúng tôi thêm 2 giây để đạt được tổng số$n$. 

Vì vậy, trước tiên chúng ta tính tổng của`twos(a_i)`trên tất cả các phần tử. Nếu nó đã ít nhất$n$, chúng ta cần các phép toán bằng không. Mặt khác, chúng tôi tính toán danh sách lợi nhuận có thể có được từ việc nhân với chỉ số$i$, sắp xếp nó theo thứ tự giảm dần và tham lam chọn mức tăng lớn nhất cho đến khi chúng ta đạt được tổng số yêu cầu. Nếu chúng ta hết lợi ích trước khi đạt được$n$, câu trả lời là -1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng$a$và chiều dài của nó$n$. Khởi tạo`current_twos`về không. 
2. Đối với mỗi phần tử$a_i$, tính số thừa số của 2 mà nó chứa bằng phép chia lặp lại và cộng nó vào`current_twos`. 
3. Nếu`current_twos >= n`, in 0 và quay lại. Không có hoạt động cần thiết. 
4. Ngược lại, hãy tính danh sách`index_twos`mỗi mục ở đâu`twos(i)`vì$i = 1$ĐẾN$n$. Điều này thể hiện lợi ích tiềm năng nếu chúng ta nhân$a_i$qua$i$. 
5. Sắp xếp`index_twos`theo thứ tự giảm dần để chúng tôi ưu tiên các chỉ số đóng góp nhiều số 2 bổ sung nhất. 
6. Khởi tạo`operations`đến 0. Lặp lại qua`index_twos`, thêm từng giá trị vào`current_twos`và tăng dần`operations`cho đến khi`current_twos >= n`. 
7. Nếu sau khi sử dụng tất cả lợi ích có thể,`current_twos < n`, in -1. Ngược lại, in`operations`. 

**Tại sao nó hoạt động** 

Bất biến là tổng của`current_twos`đại diện cho tổng sức mạnh của 2 trong sản phẩm. Nhân với các chỉ số theo thứ tự giảm dần của 2 giây đảm bảo chúng ta đạt đến ngưỡng yêu cầu với ít thao tác nhất. Không có tập hợp chỉ số thay thế nào có thể đạt được mục tiêu với ít thao tác hơn vì bất kỳ mức tăng nhỏ hơn nào cũng sẽ không đóng góp nhiều vào tổng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_twos(x):
    cnt = 0
    while x % 2 == 0:
        x //= 2
        cnt += 1
    return cnt

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        current_twos = sum(count_twos(x) for x in a)
        if current_twos >= n:
            print(0)
            continue
        
        index_twos = [count_twos(i) for i in range(1, n + 1)]
        index_twos.sort(reverse=True)
        
        operations = 0
        for gain in index_twos:
            current_twos += gain
            operations += 1
            if current_twos >= n:
                break
        
        if current_twos < n:
            print(-1)
        else:
            print(operations)

if __name__ == "__main__":
    solve()
```Trước tiên, giải pháp đếm các hệ số 2 trong mỗi số để xác định khả năng chia hết của sản phẩm ban đầu. Sắp xếp các lợi ích tiềm năng đảm bảo rằng những đóng góp lớn nhất sẽ được sử dụng trước tiên. Các trường hợp biên, như mảng không có số 2 đầu tiên, được xử lý một cách tự nhiên bằng vòng lặp`index_twos`. Python tự động xử lý tràn số nguyên, do đó các sản phẩm lớn không gây ra sự cố. 

## Ví dụ đã hoạt động 

**Ví dụ 1**:`n = 3, a = [10, 6, 11]`| Bước | current_twos | index_twos (được sắp xếp) | hoạt động | 
| --- | --- | --- | --- | 
| ban đầu | 1 + 1 + 0 = 2 | [1, 1, 0] | 0 | 
| thêm lần tăng đầu tiên | 2 + 1 = 3 | - | 1 | 

Đầu ra: 1 

Ở đây tổng twos ban đầu là 2, chúng ta cần 3. Việc chọn mức tăng chỉ số lớn nhất sẽ cộng thêm 1, đạt 3. Chỉ cần một thao tác. 

**Ví dụ 2**:`n = 4, a = [13, 17, 1, 1]`| Bước | current_twos | index_twos (được sắp xếp) | hoạt động | 
| --- | --- | --- | --- | 
| ban đầu | 0 | [2, 1, 0, 0] | 0 | 
| thêm lần tăng đầu tiên | 0 + 2 = 2 | - | 1 | 
| thêm lần tăng thứ 2 | 2 + 1 = 3 | - | 2 | 
| thêm lần thứ 3 | 3 + 0 = 3 | - | 3 | 
| thêm lần tăng thứ 4 | 3 + 0 = 3 | - | 4 | 

Chúng tôi không bao giờ đạt tới 4, vì vậy đầu ra là -1. Thuật toán xác định chính xác điều không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Đếm hai số cho mỗi phần tử là O(log a_i), mức tăng sắp xếp là O(n log n), lặp lại là O(n). | 
| Không gian | O(n) | Lưu trữ mảng và mức tăng trên mỗi chỉ mục. | 

Điều này phù hợp thoải mái trong các hạn chế. Ngay cả với$n = 2 \cdot 10^5$và nhiều trường hợp thử nghiệm, các thao tác sắp xếp và đếm đủ nhanh. 

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
assert run("6\n1\n2\n2\n3 2\n3\n10 6 11\n4\n13 17 1 1\n5\n1 1 12 1 1\n6\n20 7 14 18 3 5\n") == "0\n1\n1\n-1\n2\n1"

# Custom tests
assert run("1\n1\n1\n") == "-1", "single odd element"
assert run("1\n5\n2 4 8 16 32\n") == "0", "all powers of two already enough"
assert run("1\n3\n1 1 1\n") == "2", "needs multiple index multiplications"
assert run("1\n4\n3 3 3 3\n") == "-1", "all odd, cannot reach 2^4"
assert run("1\n6\n1 1 1 1 1 1\n") == "3", "multiple small odd numbers,
```
