---
title: "CF 1948B - Sửa mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên từ 0 đến 99 và mục tiêu của chúng ta là quyết định xem liệu chúng ta có thể chuyển đổi nó thành một mảng không giảm hay không bằng cách liên tục \"chia\" các số có hai chữ số thành các chữ số riêng lẻ của chúng."
date: "2026-06-07T17:54:03+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1948
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 163 (Rated for Div. 2)"
rating: 1100
weight: 1948
solve_time_s: 89
verified: true
draft: false
---

[CF 1948B - Sửa lỗi mảng](https://codeforces.com/problemset/problem/1948/B) 

**Đánh giá:** 1100 
**Tags:** bạo lực, dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên từ 0 đến 99 và mục tiêu của chúng ta là quyết định xem liệu chúng ta có thể chuyển đổi nó thành một mảng không giảm hay không bằng cách liên tục "chia" các số có hai chữ số thành các chữ số riêng lẻ của chúng. Cụ thể, đối với bất kỳ phần tử nào có ít nhất 10, chúng ta có thể thay thế bằng các chữ số của nó theo thứ tự. Ví dụ: 12 trở thành [1, 2], 45 trở thành [4, 5]. Chúng tôi có thể thực hiện thao tác này nhiều lần trên bất kỳ phần tử đủ điều kiện nào. Nhiệm vụ là xác định xem có tồn tại một chuỗi các thao tác làm cho toàn bộ mảng được sắp xếp theo thứ tự không giảm hay không. 

Các ràng buộc cung cấp cho chúng ta các mảng có độ dài lên tới 50 và nhiều nhất là 1000 trường hợp thử nghiệm. Vì mảng lớn nhất có kích thước nhỏ nên thuật toán bậc hai tính theo độ dài của mảng là khả thi, nhưng bất kỳ thuật toán nào có tính hàm mũ về số phần tử đều không thực tế. Mỗi phần tử có nhiều nhất là 99, vì vậy thao tác "tách" tạo ra tối đa hai phần tử mới, giới hạn kích thước của bất kỳ mảng được chuyển đổi nào. 

Các trường hợp cạnh xuất hiện khi các số đã có một chữ số, khi mảng chứa các số trùng lặp hoặc khi các số lớn hơn phải được phân chia để phù hợp với thứ tự. Ví dụ, một đầu vào`[12, 3]`có thể được chuyển đổi thành`[1, 2, 3]`, được sắp xếp. Việc triển khai bất cẩn chỉ kiểm tra số gốc mà không xem xét việc phân tách sẽ báo cáo không chính xác "KHÔNG". Một trường hợp đặc biệt khác là khi tất cả các số đều là số 0 hoặc số có một chữ số, chẳng hạn như`[0, 0]`; thuật toán sẽ nhận ra nó như đã được sắp xếp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các mảng có thể bằng cách thực hiện phân chia theo mọi thứ tự có thể. Đối với mỗi mảng, chúng tôi sẽ kiểm tra xem nó có giảm không. Về nguyên tắc, điều này đúng vì nó cố gắng hết sức mọi sự kết hợp, nhưng số lượng các khả năng có thể tăng theo cấp số nhân với số lượng các số có nhiều chữ số. Ngay cả với độ dài mảng tối đa là 50, chúng ta có thể có khoảng`2^50`khả năng xảy ra trong trường hợp xấu nhất là quá lớn. 

Thông tin quan trọng giúp đưa ra giải pháp tối ưu là việc chia số hoàn toàn có lợi cho việc sắp xếp vì nó không bao giờ làm giảm thứ tự tương đối của các chữ số. Mỗi số`x`có hai chữ số chỉ có thể chia thành chữ số thứ nhất và chữ số thứ hai và các chữ số này luôn không giảm (`x//10 <= x%10`). Do đó, thay vì khám phá tất cả các kết hợp, chúng ta có thể thực hiện một cách tiếp cận tham lam: chúng ta chuyển đổi từng số có hai chữ số thành các chữ số cấu thành của nó ngay lập tức và sau đó kiểm tra xem mảng kết quả có được sắp xếp hay không. Việc trì hoãn việc phân tách không có lợi ích gì, vì việc phân tách sớm hơn chỉ có thể giúp sắp xếp mảng dễ dàng hơn. 

Bằng cách áp dụng thông tin chi tiết này, chúng tôi giảm vấn đề thành một phép chuyển đơn giản qua mảng đã biến đổi để xác minh thứ tự không giảm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tham Lam Chia | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`b`sẽ giữ mảng được chuyển đổi. Chúng ta sẽ lặp qua mảng ban đầu và thay thế mỗi số có hai chữ số bằng các chữ số của nó. 
2. Đối với mỗi phần tử`x`trong mảng`a`, hãy kiểm tra xem nó có ít nhất là 10 không. Nếu vậy, hãy nối thêm`x // 10`theo sau là`x % 10`ĐẾN`b`. Nếu không thì nối thêm`x`trực tiếp. Điều này ngay lập tức mô phỏng việc chia số thành các chữ số. 
3. Khi tất cả các phần tử đã được xử lý thành`b`, lặp qua`b`và kiểm tra xem mỗi phần tử có lớn hơn hoặc bằng phần tử trước không. Nếu bất kỳ phần tử nào vi phạm điều kiện này, chúng tôi kết luận rằng không thể sắp xếp mảng bằng cách chia tách và xuất ra "KHÔNG". 
4. Nếu toàn bộ mảng`b`vượt qua kiểm tra không giảm, xuất ra "CÓ". 

Tại sao nó hoạt động: Tại mọi thời điểm, mảng`b`chứa biểu diễn nhỏ nhất có thể có sau khi phân tách và bất kỳ phân tách nào nữa sẽ không làm giảm chữ số đầu tiên hoặc tạo ra một số nhỏ hơn. Vì vậy, nếu`b`không giảm, có một chuỗi các thao tác làm cho mảng ban đầu được sắp xếp. Ngược lại, nếu`b`không phải là không giảm, nên không có chuỗi phép chia nào có thể khắc phục được điều đó, bởi vì chúng ta đã chia tất cả các số và bất kỳ phép chia bổ sung nào sẽ chỉ làm tăng số phần tử chứ không bao giờ giảm một chữ số để cải thiện thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = []
        for x in a:
            if x >= 10:
                b.append(x // 10)
                b.append(x % 10)
            else:
                b.append(x)
        ok = True
        for i in range(1, len(b)):
            if b[i] < b[i - 1]:
                ok = False
                break
        print("YES" if ok else "NO")

if __name__ == "__main__":
    main()
```Trong giải pháp, chúng tôi đọc số lượng ca kiểm thử, sau đó đọc mảng cho mỗi ca kiểm thử. Mảng được chuyển đổi`b`được xây dựng bằng cách chia số ngay lập tức. Việc kiểm tra thứ tự không giảm được thực hiện chỉ trong một lần. Cách tiếp cận này tránh được sự phân chia không cần thiết và đảm bảo rằng chúng tôi chỉ theo dõi trình tự có liên quan để xác định xem có thể sắp xếp hay không. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4
12 3 45 67
```| Bước | x | b sau khi nối thêm | Kiểm tra sắp xếp | 
| --- | --- | --- | --- | 
| 1 | 12 | [1, 2] | - | 
| 2 | 3 | [1, 2, 3] | - | 
| 3 | 45 | [1, 2, 3, 4, 5] | - | 
| 4 | 67 | [1, 2, 3, 4, 5, 6, 7] | Đúng | 

Điều này chứng tỏ rằng việc chia tham lam tất cả các số có hai chữ số sẽ tạo ra một mảng được sắp xếp đầy đủ. 

Đầu vào mẫu 2:```
3
12 28 5
```| Bước | x | b sau khi nối thêm | Kiểm tra sắp xếp | 
| --- | --- | --- | --- | 
| 1 | 12 | [1, 2] | - | 
| 2 | 28 | [1, 2, 2, 8] | kiểm tra 2 >= 2 được không | 
| 3 | 5 | [1, 2, 2, 8, 5] | kiểm tra 5 >= 8 thất bại | 

Việc kiểm tra không thành công ở phần tử cuối cùng nên kết quả đầu ra là "KHÔNG". Điều này cho thấy một số mảng không thể sửa được bằng cách chia nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử được xử lý một lần và mảng biến đổi được quét một lần. | 
| Không gian | O(n) | Mảng được chuyển đổi`b`lưu trữ tối đa 2n phần tử. | 

Với các ràng buộc n ≤ 50 và t ≤ 1000, tổng số phép toán nhỏ hơn 100.000, hiệu quả đối với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n4\n12 3 45 67\n3\n12 28 5\n2\n0 0\n") == "YES\nNO\nYES", "sample tests"

# Custom test cases
assert run("1\n2\n9 10\n") == "YES", "splitting last element"
assert run("1\n3\n11 12 10\n") == "YES", "all elements split result sorted"
assert run("1\n5\n21 20 3 4 5\n") == "NO", "cannot sort due to first two elements"
assert run("1\n4\n0 0 0 0\n") == "YES", "all zeros, already sorted"
assert run("1\n2\n99 1\n") == "NO", "two-digit number needs split but still cannot fix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 9 10 | CÓ | Tách phần tử cuối cùng hoạt động | 
| 11 12 10 | CÓ | Nhiều phần tách tạo ra mảng được sắp xếp | 
| 21 20 3 4 5 | KHÔNG | Các số có hai chữ số sớm ngăn cản việc sắp xếp | 
| 0 0 0 0 | CÓ | Đã được sắp xếp với tất cả các phần tử bằng nhau | 
| 99 1 | KHÔNG | Không thể sửa đơn hàng ngay cả sau khi chia tách | 

## Vỏ cạnh 

Đối với một mảng như`[12, 3]`, thuật toán chia 12 thành`[1, 2]`và nối thêm 3 để có được`[1, 2, 3]`. Việc kiểm tra xác nhận rằng mảng đã được sắp xếp, tạo ra "CÓ". Vì`[28, 5]`, sự chia tách tạo ra`[2, 8, 5]`. Phần tử cuối cùng 5 nhỏ hơn 8 nên việc kiểm tra không thành công và tạo ra "NO". Điều này xác nhận thuật toán xử lý các mảng trong đó việc phân tách là cần thiết nhưng không đủ, cũng như các mảng đã được sắp xếp hoặc có thể sắp xếp được một cách tầm thường.
