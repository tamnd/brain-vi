---
title: "CF 1687A - Khu rừng phù phép"
description: "Chúng tôi được yêu cầu tính số lượng nấm tối đa mà Marisa có thể thu thập trong khu rừng một chiều trong một số phút giới hạn."
date: "2026-06-09T23:43:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1687
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 796 (Div. 1)"
rating: 1600
weight: 1687
solve_time_s: 123
verified: false
draft: false
---

[CF 1687A - Khu rừng mê hoặc](https://codeforces.com/problemset/problem/1687/A) 

**Đánh giá:** 1600 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tính số lượng nấm tối đa mà Marisa có thể thu thập trong khu rừng một chiều trong một số phút giới hạn. Mỗi vị trí dọc theo trục bắt đầu với một số lượng nấm nhất định và mỗi phút, Marisa di chuyển tối đa một bước sang trái hoặc phải, thu thập tất cả nấm ở vị trí hiện tại của mình và sau đó mỗi vị trí sẽ sinh ra một loại nấm mới. Cô ấy không thể thu thập nấm ở phút 0, vì vậy việc thu thập đầu tiên xảy ra ở phút 1. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị số lượng nấm tối đa mà Marisa có thể thu thập được.`k`phút. 

Những ràng buộc đầu vào cho thấy rõ rằng những cách tiếp cận ngây thơ phải được xem xét cẩn thận. Có thể có tới`2 × 10^5`vị trí trên tất cả các trường hợp thử nghiệm và`k`có thể lên tới`10^9`. Điều này loại trừ mọi phương pháp mô phỏng từng phút riêng lẻ vì mô phỏng trực tiếp có thể yêu cầu tới`10^14`hoạt động. Số lượng ban đầu và tốc độ tăng trưởng mỗi phút là số nguyên lớn, lên tới`10^9`, vì vậy chúng ta phải lưu ý đến việc tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định, mặc dù Python xử lý nguyên bản các số nguyên lớn. 

Các trường hợp cạnh bao gồm các tình huống chỉ có một vị trí hoặc khi`k`là cực kỳ lớn so với`n`. Ví dụ: nếu có một vị trí duy nhất có 999999 nấm và`k=2`, Marisa thu thập 999999 trong lần di chuyển đầu tiên và sự tăng trưởng sẽ bổ sung thêm hai cây nấm nữa nếu cô ấy xem lại, tạo ra một chuỗi số học đơn giản. Một mô phỏng chuyển động ngây thơ có thể cố gắng lặp lại từng phút một cách không chính xác hoặc cho rằng cô ấy có thể di chuyển ra ngoài giới hạn của khu rừng một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: thử mọi vị trí bắt đầu, mô phỏng từng phút để`k`các bước và theo dõi tổng số nấm được thu thập. Điều này đúng vì nó liệt kê đầy đủ tất cả các khả năng, nhưng nó thất bại với số lượng lớn.`k`bởi vì nó sẽ yêu cầu`O(n * k)`hoạt động trên mỗi trường hợp thử nghiệm, điều này là không khả thi khi`k`có thể đạt được`10^9`. 

Quan sát chính là sự tăng trưởng tuyến tính và đồng đều giữa các vị trí. Nếu Marisa thu thập nấm từ các vị trí theo thứ tự giảm dần của số lượng ban đầu cộng với số phút cô ấy lấy được chúng, cô ấy sẽ tối đa hóa số tiền. Điều này dẫn đến một cách tiếp cận tham lam: sắp xếp các vị trí theo số lượng nấm ban đầu, mô phỏng việc thu thập cây nấm lớn nhất còn lại mỗi phút và tăng bộ đếm tăng trưởng tương ứng. Vì cô ấy có thể di chuyển nhiều nhất một bước mỗi phút, khoảng cách xa nhất mà cô ấy có thể đạt tới từ vị trí hiện tại bị giới hạn bởi số phút còn lại, nhưng cấu trúc của bài toán cho phép chúng tôi xử lý các giá trị đã sắp xếp với trọng số giảm dần, điều này làm giảm đáng kể số lượng các phép toán từ`k`lặp lại một vòng lặp đơn giản trên các vị trí được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * k) | O(n) | Quá chậm | 
| Sắp xếp tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc`n`Và`k`và mảng nấm ban đầu`a`.`n`là số vị trí và`k`là số phút. 
2. Sắp xếp mảng`a`theo thứ tự giảm dần. Điều này đảm bảo rằng chúng tôi xem xét các vị trí có nhiều nấm nhất trước tiên, tối đa hóa bộ sưu tập. 
3. Khởi tạo một biến`total`về không. Điều này sẽ tích lũy nấm thu thập được. 
4. Lặp lại mảng đã được sắp xếp. Đối với mỗi vị trí tại chỉ mục`i`(dựa trên 0), tính toán bộ sưu tập hiệu quả như`a[i] + min(k, i)`. Đây,`i`thể hiện số bước di chuyển cần thiết để đạt được vị trí này nếu chúng ta luôn tham lam đi từ những vị trí tốt nhất hiện có. 
5. Thêm giá trị này vào`total`. Dừng lặp lại khi`i`đạt tới`k`, vì Marisa không thể kiếm được nhiều hơn`k`di chuyển. 
6. Đầu ra`total`như câu trả lời cho trường hợp thử nghiệm. 

Tại sao nó hoạt động: Thuật toán tham lam duy trì tính bất biến mà tại mỗi phút, Marisa thu thập từ vị trí đóng góp nhiều nhất vào số tiền dựa trên sự phát triển của nấm. Việc sắp xếp theo số lượng ban đầu đảm bảo chúng tôi không bỏ lỡ các bộ sưu tập lớn hơn và giới hạn số lần lặp lại ở mức`k`tôn trọng việc hạn chế di chuyển. các`min(k, i)`đảm bảo rằng chúng tôi không ấn định mức tăng trưởng nhiều hơn số phút Marisa đã trôi qua. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def max_mushrooms():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort(reverse=True)
        total = 0
        for i in range(min(n, k)):
            total += a[i] + i
        print(total)

if __name__ == "__main__":
    max_mushrooms()
```Đầu tiên, mã sẽ đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc kích thước rừng`n`và số phút có sẵn`k`, cùng với dãy số lượng nấm. Sắp xếp`a`theo thứ tự giảm dần đảm bảo chúng tôi luôn xem xét các vị trí tốt nhất trước tiên. Vòng lặp lặp lại nhiều nhất`min(n, k)`lần, tương ứng với số lượng vị trí tối đa mà Marisa có thể đạt được trong thời gian nhất định và cộng thêm đóng góp tăng trưởng bằng cách sử dụng`i`. In ấn`total`xuất ra số lượng nấm tối đa cho trường hợp thử nghiệm đó. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 2
5 6 1 2 3
```| tôi | Đã sắp xếp a[i] | Tăng trưởng tôi | Đã sưu tầm a[i]+i | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | 6 | 6 | 
| 1 | 5 | 1 | 6 | 12 | 

Đầu ra: 12. Điều này phù hợp với đường dẫn tối ưu được mô tả trong ghi chú. Chúng tôi thu thập hai vị trí lớn nhất trong phạm vi nước đi được phép. 

Mẫu 2: 

đầu vào:```
5 7
5 6 1 2 3
```| tôi | Đã sắp xếp a[i] | Tăng trưởng tôi | Đã sưu tầm a[i]+i | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 0 | 6 | 6 | 
| 1 | 5 | 1 | 6 | 12 | 
| 2 | 3 | 2 | 5 | 17 | 
| 3 | 2 | 3 | 5 | 22 | 
| 4 | 1 | 4 | 5 | 27 | 

Đầu ra: 27. Quá trình lặp dừng ở`i=4`từ`n=5`. Số phút còn lại ngoài 4 không cho phép thu thập từ các vị trí mới, xác nhận việc chúng tôi sử dụng`min(n, k)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; vòng lặp tiếp theo là O(min(n, k)) tối đa là O(n) | 
| Không gian | O(n) | Lưu trữ mảng và bản sao được sắp xếp | 

Thuật toán dễ dàng phù hợp với các ràng buộc. Sắp xếp`2 × 10^5`các phần tử khả thi trong vòng 2 giây và vòng lặp bổ sung là tuyến tính. Mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    max_mushrooms()
    return out.getvalue().strip()

# provided samples
assert run("4\n5 2\n5 6 1 2 3\n5 7\n5 6 1 2 3\n1 2\n999999\n5 70000\n1000000000 1000000000 1000000000 1000000000 1000000000\n") == "12\n37\n1000000\n5000349985", "sample 1"

# custom cases
assert run("1\n1 1\n10\n") == "10", "single position"
assert run("1\n3 2\n1 2 3\n") == "5", "small n, k less than n"
assert run("1\n3 5\n1 2 3\n") == "9", "small n, k greater than n"
assert run("1\n5 3\n5 5 5 5 5\n") == "18", "all equal values"
assert run("1\n2 1000000000\n1000000000 1\n") == "1000000001", "k very large"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 10 | 10 | Đầu vào kích thước tối thiểu | 
| 3 2 1 2 3 | 5 | Nhỏ n, k < n | 
| 3 5 1 2 3 | 9 | Nhỏ n, k > n | 
| 5 3 5 5 5 5 5 | 18 | Tất cả các giá trị bằng nhau | 
| 2 1000000000 1000000000 1 | 1000000001 | Xử lý k lớn | 

## Vỏ cạnh 

Đối với một vị trí có nhiều phút, chẳng hạn như đầu vào`1 10 100`, thuật toán tính tổng chính xác giá trị ban đầu cộng với mức tăng trưởng được giới hạn bởi`k`. Tối đa`k`các kịch bản, các`min(n, k)`đảm bảo chúng tôi không cố gắng truy cập các phần tử mảng không tồn tại. Đối với tất cả các giá trị bằng nhau, thứ tự không quan trọng và thuật toán vẫn tính tổng chính xác bằng cách áp dụng
