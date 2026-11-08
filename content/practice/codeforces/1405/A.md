---
title: "CF 1405A - Giả mạo hoán vị"
description: "Chúng ta được yêu cầu thao tác hoán vị các số nguyên từ 1 đến n. Mỗi hoán vị có một “dấu vân tay”, là danh sách được sắp xếp tổng của mỗi cặp phần tử liên tiếp."
date: "2026-06-11T08:04:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1405
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 668 (Div. 2)"
rating: 800
weight: 1405
solve_time_s: 114
verified: false
draft: false
---

[CF 1405A - Giả mạo hoán vị](https://codeforces.com/problemset/problem/1405/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu thao tác hoán vị các số nguyên từ 1 đến n. Mỗi hoán vị có một “dấu vân tay”, là danh sách được sắp xếp tổng của mỗi cặp phần tử liên tiếp. Nhiệm vụ là lấy một hoán vị nhất định và tạo ra một hoán vị khác, khác với cùng một dấu vân tay. Các hoán vị phải hợp lệ, nghĩa là tất cả các số nguyên từ 1 đến n xuất hiện đúng một lần. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp độ dài của hoán vị và chính hoán vị đó. Đầu ra là một hoán vị cho mỗi trường hợp thử nghiệm khác với đầu vào nhưng có cùng dấu vân tay. Các ràng buộc ở mức khiêm tốn: n nhiều nhất là 100 và số lượng trường hợp thử nghiệm nhiều nhất là 668. Các giới hạn này cho thấy rằng một thuật toán có độ phức tạp bậc hai hoặc thậm chí kém hơn một chút cho mỗi trường hợp thử nghiệm sẽ chạy hiệu quả, vì 100² chỉ có 10.000 phép tính cho mỗi trường hợp và 668×10.000 nói chung là dưới 10 triệu phép tính. 

Một điểm tinh tế phát sinh với những hoán vị rất nhỏ. Với n = 2, hoán vị duy nhất khác với [1, 2] là [2, 1], nhưng hoán vị đó vẫn hợp lệ vì nó tạo ra cùng một tổng cho cặp duy nhất của nó. Với n > 2, hoán vị đầu vào có thể được sắp xếp, đảo ngược hoặc ngẫu nhiên, nhưng chúng ta cần một cách có hệ thống đảm bảo để tạo ra một hoán vị khác mà không vi phạm đặc tính dấu vân tay. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử tất cả n! hoán vị có độ dài n và tính toán dấu vân tay cho mỗi hoán vị, kiểm tra xem nó có khớp với bản gốc hay không. Về nguyên tắc thì điều này đúng nhưng hoàn toàn không thực tế. Với n = 10, đó là 3.628.800 khả năng; với n = 100, n! có kích thước lớn về mặt thiên văn. Ngay cả với n = 8, số lượng kiểm tra sẽ quá cao để chạy nhiều trường hợp kiểm thử một cách hiệu quả. 

Điểm mấu chốt là dấu vân tay chỉ phụ thuộc vào tổng các phần tử liền kề. Đảo ngược hoán vị bảo toàn mọi tổng liền kề, chỉ theo một thứ tự khác. Ví dụ: tổng của [a, b, c, d] là [a+b, b+c, c+d] và với [d, c, b, a] chúng trở thành [d+c, c+b, b+a], chứa các số giống hệt nhau, có thể theo một thứ tự khác. Vì dấu vân tay được sắp xếp nên thứ tự không quan trọng. Đảo ngược bất kỳ hoán vị nào có độ dài ít nhất là 2 đảm bảo một hoán vị khác với cùng một dấu vân tay. 

Quan sát này làm giảm vấn đề từ độ phức tạp giai thừa sang thời gian tuyến tính. Chúng ta chỉ cần đọc hoán vị, đảo ngược và xuất ra. Điều này xử lý tất cả các kích thước n ≥ 2, tôn trọng các ràng buộc và dễ thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Hoán vị ngược | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. Điều này xác định có bao nhiêu hoán vị chúng ta cần xử lý. 
2. Với mỗi test, đọc số nguyên n và hoán vị p có độ dài n. 
3. Đảo ngược mảng p để tạo ra hoán vị mới p'. Điều này có tác dụng vì việc đảo ngược sẽ bảo toàn tất cả các tổng liền kề theo một thứ tự nào đó, đủ cho dấu vân tay được sắp xếp. 
4. Xuất ra hoán vị ngược. Không cần kiểm tra thêm vì n ≥ 2 đảm bảo rằng hoán vị ngược lại khác với hoán vị ban đầu. 

Tại sao nó hoạt động: Đảo ngược bảo toàn mọi tổng các phần tử liền kề trong hoán vị. Bước sắp xếp trong dấu vân tay sẽ loại bỏ độ nhạy thứ tự, do đó dấu vân tay của mảng đảo ngược khớp với bản gốc. Với n ≥ 2, phép nghịch đảo luôn khác với hoán vị ban đầu, thỏa mãn điều kiện phân biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    p = list(map(int, input().split()))
    p.reverse()
    print(" ".join(map(str, p)))
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và xử lý nhiều trường hợp thử nghiệm. Hoán vị được đảo ngược tại chỗ bằng cách sử dụng`list.reverse()`, giúp tránh việc cấp phát thêm bộ nhớ. Cuối cùng, danh sách đảo ngược được in cách nhau bằng dấu cách. Tránh được các lỗi xảy ra bởi vì`reverse()`xử lý toàn bộ mảng và thao tác nối sẽ chuyển đổi chính xác số nguyên thành chuỗi. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên: 

| Bước | p | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1, 2] | Đảo ngược | [2, 1] | 
| 2 | Tính F(p') ngầm | [2+1] = [3] | Phù hợp với bản gốc | 

Thuật toán tạo thành công [2, 1], vân tay [3], khác với đầu vào. 

Đối với đầu vào mẫu thứ hai: 

| Bước | p | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [2, 1, 6, 5, 4, 3] | Đảo ngược | [3, 4, 5, 6, 1, 2] | 
| 2 | Tính F(p') ngầm | [3+4,4+5,5+6,6+1,1+2] = [7,9,11,7,3] | Đã sắp xếp: [3,7,7,9,11] | 

Dấu vân tay trùng khớp với bản gốc. Sự đảo ngược đảm bảo sự khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * n) | Mỗi lần đảo ngược và đầu ra đều mất thời gian tuyến tính cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Lưu trữ mảng hoán vị và phiên bản đảo ngược của nó. | 

Các ràng buộc t ≤ 668 và n ≤ 100 đảm bảo tối đa 66.800 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        p.reverse()
        print(" ".join(map(str, p)))
    return out.getvalue().strip()

# Provided samples
assert run("3\n2\n1 2\n6\n2 1 6 5 4 3\n5\n2 4 3 1 5\n") == "2 1\n3 4 5 6 1 2\n5 1 3 4 2", "samples"

# Custom test cases
assert run("1\n2\n2 1\n") == "1 2", "minimum n=2"
assert run("1\n3\n1 2 3\n") == "3 2 1", "small n=3"
assert run("1\n4\n4 3 2 1\n") == "1 2 3 4", "reversed sorted"
assert run("1\n5\n5 4 3 2 1\n") == "1 2 3 4 5", "max n=5 small test"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2\n1 2 | 2 1 | Độ dài hoán vị tối thiểu | 
| 3\n1 2 3 | 3 2 1 | Hoán vị nhỏ, đảo ngược đúng | 
| 4\n4 3 2 1 | 1 2 3 4 | Đã đảo ngược, đảm bảo đầu ra khác nhau | 
| 5\n5 4 3 2 1 | 1 2 3 4 5 | Đảo ngược kích thước vừa phải | 

## Vỏ cạnh 

Với n = 2, đầu vào [1,2] được đảo ngược thành [2,1]. Dấu vân tay [3] giống hệt nhau và hoán vị khác nhau, thỏa mãn yêu cầu bài toán. Đối với một hoán vị lớn hơn, được sắp xếp như [1,2,3,4,5], phép đảo ngược [5,4,3,2,1] tạo ra cùng một dấu vân tay được sắp xếp [3,5,7,9] và khác với bản gốc. Trong mọi trường hợp, việc đảo ngược là đủ để thỏa mãn cả ràng buộc về dấu vân tay và tính khác biệt.
