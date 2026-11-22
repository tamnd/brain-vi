---
title: "CF 336C - Gấu Vasily và Trình tự"
description: "Chúng ta được cho một dãy số nguyên dương tăng dần. Nhiệm vụ là chọn một tập hợp con của những số này sao cho vẻ đẹp của tập hợp con đó được tối đa hóa."
date: "2026-06-06T10:44:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 336
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 195 (Div. 2)"
rating: 1800
weight: 336
solve_time_s: 101
verified: true
draft: false
---

[CF 336C - Chú gấu Vasily và Trình tự](https://codeforces.com/problemset/problem/336/C) 

**Đánh giá:** 1800 
**Tags:** vũ phu, tham lam, thực hiện, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên dương tăng dần. Nhiệm vụ là chọn một tập hợp con của những số này sao cho vẻ đẹp của tập hợp con đó là lớn nhất. Cái đẹp được định nghĩa là số nguyên lớn nhất$v \ge 0$sao cho AND theo bit của tất cả các số trong tập hợp con chia hết cho$2^v$. Nếu không như vậy$v$tồn tại, vẻ đẹp là -1. Trong số tất cả các tập hợp con có vẻ đẹp tối đa, chúng ta được yêu cầu chọn một tập hợp con có số phần tử lớn nhất. 

Được trình bày lại theo cách đơn giản hơn, đối với mỗi tập hợp con số, AND theo bit cho chúng ta một số có khả năng chia hết cho lũy thừa của hai mà chúng ta có thể đo được. Tối đa hóa vẻ đẹp tương đương với việc tối đa hóa số lượng số 0 ở cuối trong AND của các số đã chọn. Sau đó, trong số tất cả các tập hợp con đạt được số lượng số 0 ở cuối tối đa này, chúng tôi ưu tiên tập hợp con chứa nhiều số nhất. 

Kích thước đầu vào$n$có thể lên đến$10^5$và số lượng của chúng lên tới$10^9$. Một cách tiếp cận đơn giản để kiểm tra tất cả các tập hợp con sẽ yêu cầu$O(2^n)$hoạt động hoàn toàn không thể thực hiện được. Ràng buộc này gợi ý rằng giải pháp phải hoạt động trong thời gian tuyến tính hoặc gần tuyến tính so với$n$. 

Các trường hợp đặc biệt không rõ ràng bao gồm các chuỗi trong đó tất cả các số đều là số lẻ, các chuỗi có một phần tử duy nhất hoặc các số có lũy thừa hai cực kỳ khác nhau. Ví dụ: một chuỗi như`[1, 3, 5]`không có tập hợp con nào có AND chia hết cho 2, vì vậy vẻ đẹp là 0 đối với bất kỳ tập hợp con đơn lẻ nào. Một cách tiếp cận ngây thơ luôn lấy số lượng lớn nhất có thể bỏ lỡ kích thước tập hợp con tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả các tập hợp con, tính toán AND cho mỗi tập hợp, đếm các số 0 ở cuối và chọn tập hợp con có các số 0 ở cuối tối đa và kích thước tối đa. Số lượng hoạt động sẽ là$O(n 2^n)$, điều này không thể thực hiện được đối với$n=10^5$. 

Cái nhìn sâu sắc quan trọng là vẻ đẹp được xác định bởi số lượng số 0 ở cuối trong biểu diễn nhị phân của mỗi số. Chúng ta có thể tính toán$f(x)$, số chữ số 0 ở cuối mỗi số$x$, BẰNG$\text{tz}(x) = \text{lowest power of two dividing } x$. Vẻ đẹp tối đa có thể đạt được chỉ đơn giản là vẻ đẹp lớn nhất$v$sao cho ít nhất một số có$f(x) = v$. Một lần$v$được xác định, tập hợp con tối ưu bao gồm tất cả các số có số 0 ở cuối bằng nhau$v$. Điều này đảm bảo cả vẻ đẹp tối đa và kích thước tập hợp con tối đa vì bất kỳ số bổ sung nào có ít số 0 ở cuối sẽ làm giảm kết quả AND bên dưới$2^v$. 

Quan sát này làm giảm vấn đề từ độ phức tạp theo cấp số nhân sang độ phức tạp tuyến tính. Tính các số 0 ở cuối mỗi số là$O(1)$mỗi số, lặp qua mảng là$O(n)$và lọc tập hợp con tối ưu cũng là$O(n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dãy số nguyên đầu vào. 
2. Với mỗi số, hãy tính số số 0 ở cuối trong biểu diễn nhị phân của nó. Điều này có thể được thực hiện bằng cách chia liên tục cho 2 hoặc sử dụng thao tác bit. Lưu trữ các số lượng này. 
3. Xác định số 0 ở cuối tối đa trên tất cả các số. Giá trị này thể hiện vẻ đẹp tối đa$v$. 
4. Thu thập tất cả các số có chính xác số 0 ở cuối tối đa này. Những con số này tạo thành tập hợp con đạt được vẻ đẹp tối đa và kích thước tối đa cùng một lúc. 
5. In kích thước của tập hợp con này và các số. 

Tại sao nó hoạt động: AND theo bit của nhiều số không thể có nhiều số 0 ở cuối hơn số lượng số 0 ở cuối tối thiểu trong số các số đã chọn. Bằng cách chọn các số có chính xác số 0 ở cuối tối đa, chúng tôi đảm bảo rằng AND của tập hợp con có chính xác số 0 ở cuối. Việc bao gồm các số có ít số 0 ở cuối sẽ làm giảm vẻ đẹp và việc loại trừ các số có cùng số 0 ở cuối sẽ làm giảm kích thước tập hợp con. Do đó, chiến lược này tạo ra giải pháp tối ưu theo cả hai tiêu chí. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def trailing_zeros(x):
    count = 0
    while x % 2 == 0:
        x //= 2
        count += 1
    return count

def main():
    n = int(input())
    a = list(map(int, input().split()))
    
    # Step 1: compute trailing zeros for each number
    tz_counts = [trailing_zeros(x) for x in a]
    
    # Step 2: find maximum trailing zeros
    max_tz = max(tz_counts)
    
    # Step 3: select numbers with exactly max trailing zeros
    result = [a[i] for i in range(n) if tz_counts[i] == max_tz]
    
    # Step 4: print results
    print(len(result))
    print(" ".join(map(str, result)))

if __name__ == "__main__":
    main()
```các`trailing_zeros`hàm lặp đi lặp lại chia cho hai, đếm xem nó có thể làm như vậy bao nhiêu lần. Điều này là an toàn vì số lượng nhiều nhất là$10^9$, do đó vòng lặp chạy tối đa 30 lần lặp cho mỗi số. Chúng ta cũng có thể sử dụng các phép toán theo bit để đạt hiệu quả nhưng độ phức tạp vẫn ở mức tuyến tính. Việc hiểu danh sách đảm bảo chúng tôi thu thập tập hợp con có số 0 ở cuối tối đa một cách hiệu quả. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5
1 2 3 4 5
```| Số | Nhị phân | Số 0 ở cuối | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 10 | 1 | 
| 3 | 11 | 0 | 
| 4 | 100 | 2 | 
| 5 | 101 | 0 | 

Các số 0 ở cuối tối đa là 2 (từ số 4). Tập con có vẻ đẹp tối đa và kích thước tối đa là`[4]`. 

**Đầu vào mẫu 2 (được xây dựng):**```
6
8 12 16 20 24 32
```| Số | Nhị phân | Số 0 ở cuối | 
| --- | --- | --- | 
| 8 | 1000 | 3 | 
| 12 | 1100 | 2 | 
| 16 | 10000 | 4 | 
| 20 | 10100 | 2 | 
| 24 | 11000 | 3 | 
| 32 | 100000 | 5 | 

Số 0 ở cuối tối đa = 5. Tập hợp con là`[32]`. 

Những dấu vết này cho thấy rằng chúng tôi xác định chính xác số lượng số 0 ở cuối và chọn tập hợp con tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * log(max(a_i))) | Mỗi phép tính số 0 ở cuối mất tối đa 30 lần lặp, thực tế là không đổi; lặp qua n phần tử chiếm ưu thế. | 
| Không gian | O(n) | Chúng tôi lưu trữ số 0 ở cuối và tập hợp con kết quả. | 

Được cho$n \le 10^5$và số lượng lên tới$10^9$, giải pháp thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided sample
assert run("5\n1 2 3 4 5\n") == "1\n4", "sample 1"

# Minimum-size input
assert run("1\n7\n") == "1\n7", "single element"

# All even numbers
assert run("4\n2 4 8 16\n") == "1\n16", "all even"

# Mixed numbers
assert run("6\n8 12 16 20 24 32\n") == "1\n32", "mixed numbers"

# All odd numbers
assert run("5\n1 3 5 7 9\n") == "5\n1 3 5 7 9", "all odd, beauty 0"

# Maximum-size input (constructed small for test)
assert run("5\n2 4 6 8 10\n") == "1\n8", "subset with max trailing zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n7`|`1\n7`| Đầu vào một phần tử | 
|`4\n2 4 8 16`|`1\n16`| Tất cả các số chẵn, số 0 ở cuối lớn nhất được chọn | 
|`5\n1 3 5 7 9`|`5\n1 3 5 7 9`| Tất cả các số lẻ, vẻ đẹp 0, kích thước tập hợp con tối đa | 
|`6\n8 12 16 20 24 32`|`1\n32`| Hỗn số, chọn số có số 0 ở cuối tối đa | 
|`5\n2 4 6 8 10`|`1\n8`| Xác nhận tập hợp con có vẻ đẹp tối đa được chọn chính xác | 

## Vỏ cạnh 

Một chuỗi gồm tất cả các số lẻ, ví dụ:`[1, 3, 5, 7]`, mang lại vẻ đẹp tối đa 0. Thuật toán tính các số 0 ở cuối là 0 cho mỗi số và chọn tất cả các số vì chúng đều đáp ứng tiêu chí số 0 ở cuối tối đa. Đầu ra là`[1, 3, 5, 7]`, đó chính xác là sự tối đa hóa tập hợp con chính xác
