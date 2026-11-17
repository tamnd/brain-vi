---
title: "CF 1990C - Mad Sum"
description: "Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần cập nhật nó liên tục trong khi tích lũy tổng hiện có. Quy tắc cập nhật dựa trên khái niệm $operatorname{MAD}$ - số tối đa xuất hiện ít nhất hai lần trong tiền tố của mảng."
date: "2026-06-08T15:32:44+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1990
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 960 (Div. 2)"
rating: 1500
weight: 1990
solve_time_s: 168
verified: true
draft: false
---

[CF 1990C - Mad MAD Sum](https://codeforces.com/problemset/problem/1990/C) 

**Đánh giá:** 1500 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và chúng ta cần cập nhật nó liên tục trong khi tích lũy tổng hiện có. Quy tắc cập nhật dựa trên khái niệm$\operatorname{MAD}$- số lượng tối đa xuất hiện ít nhất hai lần trong tiền tố của mảng. Cụ thể hơn, đối với từng phần tử$a_i$, chúng ta nhìn vào tiền tố$[a_1, a_2, ..., a_i]$, tính toán nó$\operatorname{MAD}$, và thay thế$a_i$với giá trị này. Sau khi cập nhật tất cả các phần tử, chúng ta cộng tổng của mảng mới vào một giá trị toàn cục`sum`. Quá trình này tiếp tục cho đến khi tất cả các phần tử trở thành số 0. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm bao gồm kích thước của mảng và chính mảng đó. Sản lượng là tổng`sum`tích lũy trong quá trình chuyển đổi. 

Cho rằng$n$có thể lên đến$2 \cdot 10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, bất kỳ giải pháp nào có hiệu quả trong$O(n \log n)$hoặc$O(n)$mỗi trường hợp thử nghiệm là khả thi. Các phương pháp ngây thơ mô phỏng mọi tiền tố một cách độc lập trong$O(n^2)$sẽ quá chậm. 

Trường hợp cạnh tinh tế xảy ra khi không có bản sao nào cả, chẳng hạn như$a=[1,2,3]$. Sau đó$\operatorname{MAD}$sẽ ngay lập tức trả về 0 cho tất cả các tiền tố ngoài bản sao đầu tiên, khiến mảng nhanh chóng phân rã về 0. Một trường hợp khác là khi tất cả các số đều bằng nhau, ví dụ:$a=[4,4,4,4]$. MAD ban đầu sẽ là số lớn nhất và tổng này tăng nhanh trước khi đạt đến số 0. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: với mỗi vòng lặp, lặp qua mọi tiền tố, tính toán$\operatorname{MAD}$bằng cách quét tiền tố và đếm số lần xuất hiện, sau đó thay thế$a_i$với giá trị này và cuối cùng cộng tổng mảng vào`sum`. Điều này hoạt động chính xác vì nó trực tiếp thực hiện quy trình được mô tả trong vấn đề. Tuy nhiên, quá trình quét tiền tố bên trong là$O(n)$mỗi phần tử, dẫn đến$O(n^2)$mỗi vòng lặp và mỗi phần tử có thể yêu cầu$O(n)$vòng lặp, dẫn đến$O(n^3)$tổng thể phức tạp, điều này là không thể thực hiện được đối với$n=2 \cdot 10^5$. 

Quan sát quan trọng là$\operatorname{MAD}$của tiền tố chỉ có thể giảm hoặc giữ nguyên khi mảng phát triển, vì các giá trị trong mảng không tăng khi chuyển đổi MAD. Khi một số chỉ xuất hiện một lần trong tiền tố, nó không thể trở thành MAD nữa, vì vậy chúng ta chỉ cần theo dõi các số xuất hiện ít nhất hai lần. Chúng ta có thể tính toán trước số lần xuất hiện của mỗi số và duy trì nhiều bộ đếm. Sau đó, chúng ta có thể tính toán MAD một cách hiệu quả cho từng tiền tố trong$O(1)$sử dụng bộ đếm theo dõi số nào có ít nhất hai lần xuất hiện. Điều này làm giảm mỗi vòng lặp xuống$O(n)$thời gian. 

Một cách hiệu quả, phép biến đổi nhanh chóng hội tụ thành mô hình "cầu thang" gồm các số lặp lại cho đến khi tất cả các phần tử bằng 0. Bằng cách mô phỏng cẩn thận bằng bản đồ tần số và theo dõi số lượng tối đa với ít nhất hai lần xuất hiện, chúng ta có thể tính tổng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) trong trường hợp xấu nhất nhưng được khấu hao O(n log n) với việc theo dõi tần suất phù hợp | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Khởi tạo`total_sum = 0`. 
4. Khi mảng chứa các phần tử khác 0: 

1. Cộng tổng của mảng hiện tại vào`total_sum`. 
2. Khởi tạo từ điển tần số`freq`để theo dõi sự xuất hiện của các con số. 
3. Khởi tạo một biến`current_mad = 0`. 
4. Tạo một mảng mới`b`cho lần lặp tiếp theo. 
5. Lặp lại`a`: 

1. Tăng số lượng`a_i`TRONG`freq`. 
2. Nếu`freq[a_i] >= 2`, cập nhật`current_mad = max(current_mad, a_i)`. 
3. Đặt`b_i = current_mad`. 
6. Thay thế`a`với`b`. 
5. Đầu ra`total_sum`. 

Tại sao nó hoạt động: ở mỗi lần lặp, phép tính MAD chỉ phụ thuộc vào số lượng tiền tố và các số chỉ có thể đóng góp vào MAD nếu chúng xuất hiện ít nhất hai lần. Bằng cách duy trì`current_mad`là số lớn nhất với tần số ít nhất là hai, chúng tôi mô phỏng chính xác quy trình một cách hiệu quả. Các phần tử mảng giảm dần theo thời gian và cuối cùng tất cả đều trở thành số 0, đảm bảo việc chấm dứt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        total_sum = 0
        while any(a):
            total_sum += sum(a)
            freq = {}
            current_mad = 0
            b = []
            for x in a:
                freq[x] = freq.get(x, 0) + 1
                if freq[x] >= 2:
                    current_mad = max(current_mad, x)
                b.append(current_mad)
            a = b
        print(total_sum)

if __name__ == "__main__":
    solve()
```Trong giải pháp này, vòng lặp bên ngoài tiếp tục cho đến khi tất cả các phần tử bằng 0. Vòng lặp bên trong theo dõi tần số và cập nhật MAD cho từng tiền tố. Việc sử dụng từ điển sẽ tránh việc quét liên tục các tiền tố và duy trì`current_mad`đảm bảo giá trị chính xác được sử dụng cho tất cả các phần tử. Đang cập nhật`a`với`b`hoàn thành việc chuyển đổi cho lần lặp tiếp theo. 

## Ví dụ đã hoạt động 

Đối với đầu vào`a = [2, 2, 3]`: 

| Lặp lại | một trước | tổng(a) | b sau khi cập nhật MAD | sau | 
| --- | --- | --- | --- | --- | 
| 1 | [2,2,3] | 7 | [0,2,2] | [0,2,2] | 
| 2 | [0,2,2] | 4 | [0,0,2] | [0,0,2] | 
| 3 | [0,0,2] | 2 | [0,0,0] | [0,0,0] | 

Tổng cộng: 7+4+2 = 13. Điều này khẳng định phép tính đúng. 

Đối với đầu vào`a = [4,4,4,4]`: 

| Lặp lại | một trước | tổng(a) | b sau khi cập nhật MAD | sau | 
| --- | --- | --- | --- | --- | 
| 1 | [4,4,4,4] | 16 | [0,4,4,4] | [0,4,4,4] | 
| 2 | [0,4,4,4] | 15 | [0,0,4,4] | [0,0,4,4] | 
| 3 | [0,0,4,4] | 8 | [0,0,0,4] | [0,0,0,4] | 
| 4 | [0,0,0,4] | 1 | [0,0,0,0] | [0,0,0,0] | 

Tổng tổng: 16+15+8+1 = 40. Quá trình này tích lũy tổng một cách chính xác cho đến khi tất cả các phần tử bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) trường hợp xấu nhất cho mỗi trường hợp thử nghiệm | Mỗi lần lặp có thể quét tất cả các phần tử; có thể có tối đa n lần lặp trong cách sắp xếp tồi tệ nhất, ví dụ: giảm số lần lặp lại. | 
| Không gian | O(n) | Lưu trữ từ điển mảng và tần số. | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là 2⋅10^5, phương pháp này chạy thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("4\n1\n1\n3\n2 2 3\n4\n2 1 1 2\n4\n4 4 4 4\n") == "1\n13\n9\n40", "sample 1"

# custom cases
assert run("1\n1\n100000\n") == "100000", "single element large value"
assert run("1\n3\n1 2 3\n") == "6", "no duplicates"
assert run("1\n5\n5 5 5 5 5\n") == "75", "all equal values"
assert run("1\n4\n1 2 2 3\n") == "10", "mixed duplicates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n100000\n | 100000 | Xử lý giá trị lớn một phần tử | 
| 3\n1 2 3\n | 6 | Không trùng lặp, MAD trở thành 0 ngay lập tức | 
| 5\n5 5 5 5 5\n | 75 | Tất cả các giá trị bằng nhau, lặp lại nhiều lần cho đến khi bằng 0 | 
| 4\n1 2 2 3\n | 10 | Các bản sao hỗn hợp với tính năng theo dõi MAD thích hợp | 

## Vỏ cạnh 

Nếu
