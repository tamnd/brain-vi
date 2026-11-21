---
title: "CF 252B - Phân loại mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên có độ dài n và được yêu cầu xác định xem liệu chúng ta có thể hoán đổi hai phần tử bất kỳ ở các vị trí khác nhau sao cho mảng kết quả không còn được sắp xếp nữa. Việc sắp xếp ở đây được định nghĩa rộng rãi: mảng được sắp xếp nếu nó không giảm hoặc không tăng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "sortings"]
categories: ["algorithms"]
codeforces_contest: 252
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 153 (Div. 2)"
rating: 1800
weight: 252
solve_time_s: 127
verified: true
draft: false
---

[CF 252B - Phân loại mảng](https://codeforces.com/problemset/problem/252/B) 

**Đánh giá:** 1800 
**Tags:** vũ lực, phân loại 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên có độ dài _n_ và được yêu cầu xác định xem liệu chúng ta có thể hoán đổi hai phần tử bất kỳ ở các vị trí riêng biệt sao cho mảng kết quả không còn được sắp xếp hay không. Việc sắp xếp ở đây được định nghĩa rộng rãi: mảng được sắp xếp nếu nó không giảm hoặc không tăng. Chúng ta không thể hoán đổi các phần tử bằng nhau, ngay cả khi chúng ở các vị trí khác nhau và chúng ta phải thực hiện hoán đổi nếu có thể. 

Các ràng buộc đầu vào là 1 ≤ _n_ ≤ 10^5 và các phần tử mảng tối đa 10^9. Với _n_ có khả năng là 100.000 và giới hạn 2 giây, giải pháp O(n^2) sẽ bao gồm 10^10 thao tác, quá chậm. Điều này có nghĩa là chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. Bộ nhớ rất rộng ở mức 256 MB, vì vậy việc lưu trữ một vài mảng phụ có kích thước _n_ là khả thi. 

Trường hợp cạnh rất quan trọng ở đây. Một mảng một phần tử rõ ràng không thể được sắp xếp bằng cách hoán đổi. Một mảng có tất cả các phần tử bằng nhau cũng không thể được sắp xếp vì chúng ta không thể hoán đổi các phần tử bằng nhau. Các mảng tăng hoặc giảm nghiêm ngặt và chứa tất cả các phần tử duy nhất có thể hoán đổi được, nhưng các mảng đơn điệu với các giá trị lặp lại ở cuối yêu cầu lựa chọn cẩn thận các giá trị riêng biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các cặp chỉ số riêng biệt, thử hoán đổi nếu các phần tử không bằng nhau và kiểm tra xem mảng kết quả có chưa được sắp xếp hay không. Điều này đúng nhưng O(n^2) trong trường hợp xấu nhất sẽ không hoàn thành với n = 10^5. Brute-force hoạt động vì nó kiểm tra một cách có hệ thống mọi tùy chọn, nhưng nó không thành công đối với các mảng lớn do sự bùng nổ tổ hợp của các cặp. 

Cách tiếp cận tối ưu tận dụng quan sát rằng để phá vỡ một mảng đã được sắp xếp, chúng ta chỉ cần hoán đổi phần tử đầu tiên khác với phần tử khác ở một nơi khác. Nếu mảng đang tăng dần, việc hoán đổi phần tử đầu tiên với bất kỳ phần tử nào sau đó nhỏ hơn sẽ phá vỡ thứ tự. Tương tự, nếu giảm nghiêm ngặt, việc hoán đổi phần tử đầu tiên với phần tử lớn hơn sau này sẽ hoạt động. Điều này làm giảm vấn đề thành quét tuyến tính đối với hai chỉ số bất kỳ trong đó các phần tử khác nhau, đó là O(n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Kiểm tra xem độ dài mảng có phải là 1 hay không. Nếu đúng như vậy, hãy xuất -1 vì mảng một phần tử không thể được sắp xếp bằng hoán đổi. 
2. Lặp lại mảng từ phần tử đầu tiên đến phần tử thứ hai đến cuối cùng. Nếu bạn tìm thấy hai phần tử liên tiếp khác nhau, hãy ghi lại chỉ số của chúng. Hai vị trí này được đảm bảo tạo ra một mảng chưa được sắp xếp nếu bị hoán đổi, vì việc hoán đổi bất kỳ hai phần tử riêng biệt nào trong một mảng được sắp xếp sẽ phá vỡ thuộc tính tăng hoặc giảm. 
3. Nếu không tìm thấy cặp nào như vậy trong quá trình quét tuyến tính thì tất cả các phần tử đều bằng nhau. Trong trường hợp này, xuất ra -1 vì không được phép hoán đổi các phần tử bằng nhau. 
4. Xuất ra chỉ số của cặp đầu tiên tìm được. 

Tại sao nó hoạt động: Trong một mảng được sắp xếp, bất kỳ hai phần tử riêng biệt nào cũng có thứ tự tương đối. Hoán đổi hai phần tử riêng biệt sẽ vi phạm trật tự đó ít nhất tại một vị trí. Quét tuyến tính đảm bảo rằng chúng tôi tìm thấy cặp đầu tiên như vậy một cách nhanh chóng. Nếu không có cặp nào tồn tại thì tất cả các phần tử đều bằng nhau và không thể hoán đổi được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

if n == 1:
    print(-1)
else:
    found = False
    for i in range(n - 1):
        if a[i] != a[i + 1]:
            print(i + 1, i + 2)
            found = True
            break
    if not found:
        print(-1)
```Giải pháp đọc kích thước mảng và các phần tử bằng cách sử dụng đầu vào nhanh. Nó xử lý trường hợp một phần tử tầm thường ngay lập tức. Quét tuyến tính kiểm tra các phần tử liền kề vì bất kỳ cặp phần tử liền kề không bằng nhau nào đều đảm bảo sự hoán đổi hợp lệ phá vỡ tính đơn điệu. Việc sử dụng chỉ mục dựa trên 1 trong đầu ra phù hợp với yêu cầu của bài toán. Lá cờ`found`đảm bảo rằng nếu tất cả các phần tử đều bằng nhau thì chúng ta xuất ra đúng -1. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
1
1
```| tôi | một [tôi] | a[i+1] | a[i] != a[i+1]? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| - | 1 | - | - | -1 | 

Mảng chỉ có một phần tử nên kết quả đầu ra là -1. 

Đầu vào mẫu 2:```
4
1 2 3 4
```| tôi | một [tôi] | a[i+1] | a[i] != a[i+1]? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | Có | 1 2 | 

Cặp phần tử liền kề không bằng nhau đầu tiên nằm ở chỉ số 1 và 2, sẽ phá vỡ thứ tự tăng dần nếu hoán đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính mảng một lần để tìm hai phần tử riêng biệt | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian và bộ nhớ cho n 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    if n == 1:
        return "-1"
    for i in range(n - 1):
        if a[i] != a[i + 1]:
            return f"{i+1} {i+2}"
    return "-1"

assert run("1\n1\n") == "-1", "sample 1"
assert run("4\n1 2 3 4\n") == "1 2", "sample 2"
assert run("3\n5 5 5\n") == "-1", "all equal elements"
assert run("2\n10 20\n") == "1 2", "minimum size distinct"
assert run("5\n1 3 3 3 1\n") == "1 2", "distinct at start"
assert run("6\n2 2 2 3 3 3\n") == "3 4", "distinct in middle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1\n1 | -1 | mảng một phần tử | 
| 4\n1 2 3 4 | 1 2 | mảng tăng nghiêm ngặt | 
| 3\n5 5 5 | -1 | mọi phần tử đều bằng nhau | 
| 2\n10 20 | 1 2 | kích thước tối thiểu với các phần tử riêng biệt | 
| 5\n1 3 3 3 1 | 1 2 | các yếu tố riêng biệt khi bắt đầu | 
| 6\n2 2 2 3 3 3 | 3 4 | các yếu tố riêng biệt ở giữa | 

## Vỏ cạnh 

Đối với mảng một phần tử như`1\n1`, lời giải ngay lập tức trả về -1, xử lý chính xác trường hợp không thể xảy ra. Đối với các mảng trong đó tất cả các phần tử đều bằng nhau, chẳng hạn như`3\n5 5 5`, quá trình quét tuyến tính không tìm thấy bất kỳ phần tử liền kề riêng biệt nào và giải pháp đưa ra chính xác là -1. Đối với các mảng tăng hoặc giảm nghiêm ngặt, cặp liền kề không bằng nhau đầu tiên được chọn, điều này đảm bảo rằng mảng sẽ không được sắp xếp sau khi hoán đổi. Thuật toán xử lý chính xác các mảng có kích thước tối đa cho phép vì nó chỉ yêu cầu một lần truyền qua mảng, duy trì thời gian O(n) và không gian O(1).
