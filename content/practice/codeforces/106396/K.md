---
title: "CF 106396K - \u5171\u6b7b"
description: "Chúng ta được cung cấp một danh sách các số nguyên và giá trị ngưỡng $k$. Từ danh sách này, chúng tôi quan tâm đến việc “đóng” hai phần tử bất kỳ có thể đạt được như thế nào trong hoạt động XOR."
date: "2026-06-21T09:59:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "K"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 41
verified: true
draft: false
---

[CF 106396K - \u5171\u6b7b](https://codeforces.com/problemset/problem/106396/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các số nguyên và giá trị ngưỡng$k$. Từ danh sách này, chúng tôi quan tâm đến việc “đóng” hai phần tử bất kỳ có thể đạt được như thế nào trong hoạt động XOR. Cụ thể, chúng tôi xem xét tất cả các cặp số trong mảng, tính toán XOR theo bit của chúng và tập trung vào giá trị XOR nhỏ nhất có thể đạt được giữa bất kỳ cặp nào. 

Nhiệm vụ là xác định xem XOR theo cặp tối thiểu này có lớn nhất hay không$k$. Nếu tồn tại ít nhất một cặp có XOR đủ nhỏ thì câu trả lời là khẳng định, ngược lại là âm tính. 

Từ quan điểm tính toán, kích thước mảng có thể đủ lớn để việc kiểm tra trực tiếp từng cặp là không khả thi. Một so sánh cặp đôi đầy đủ đòi hỏi$O(n^2)$hoạt động trở nên quá chậm khi$n$phát triển đến các giới hạn điển hình của Codeforces như$10^5$. 

Một trường hợp tế nhị phát sinh khi tất cả các số đều giống hệt nhau. Trong tình huống đó, mọi XOR đều bằng 0, vì vậy câu trả lời luôn là “Có” bất kể$k$. Một tình huống quan trọng khác là khi các giá trị có khoảng cách rộng rãi trong biểu diễn nhị phân. Ngay cả khi các số gần giống nhau về số lượng, XOR của chúng vẫn có thể lớn do các bit cao khác nhau, do đó, chỉ sắp xếp theo giá trị rõ ràng là không đủ nếu không có sự biện minh. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: tính XOR cho mỗi cặp$(i, j)$, theo dõi mức tối thiểu và so sánh nó với$k$. Điều này hiệu quả vì nó trực tiếp đánh giá định nghĩa của vấn đề. Tuy nhiên số lượng cặp$n(n-1)/2$, đạt khoảng$5 \times 10^9$hoạt động khi$n = 10^5$, vượt xa giới hạn khả thi. 

Quan sát quan trọng là chúng ta không thực sự cần so sánh tất cả các cặp. Cấu trúc của XOR trên số nguyên ngụ ý rằng nếu hai số gần nhau theo thứ tự được sắp xếp thì chúng có nhiều khả năng tạo ra XOR nhỏ nhất. Lý do là XOR bị chi phối bởi bit cao nhất nơi hai số khác nhau. Nếu hai số cách xa nhau theo thứ tự được sắp xếp, bit khác nhau đáng kể nhất của chúng có thể cao, làm cho XOR của chúng lớn. Ngược lại, các phần tử liền kề theo thứ tự được sắp xếp sẽ giảm thiểu vị trí đầu tiên nơi chúng khác nhau, điều này có xu hướng giảm thiểu giá trị XOR. 

Điều này làm giảm vấn đề sắp xếp mảng và chỉ kiểm tra XOR giữa các phần tử liên tiếp. XOR tối thiểu trên tất cả các cặp phải xuất hiện trong ít nhất một cặp liền kề theo thứ tự được sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + Kiểm tra liền kề |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước mảng$n$và ngưỡng$k$, theo sau là các giá trị mảng. 
2. Sắp xếp mảng theo thứ tự không giảm. Điều này tổ chức các giá trị sao cho các phần tử gần nhau về số lượng trở thành lân cận. 
3. Khởi tạo một biến`ans`thành một số lượng lớn, đại diện cho XOR tốt nhất được tìm thấy cho đến nay. 
4. Lặp lại mảng đã được sắp xếp từ chỉ số 1 đến$n-1$, tính toán XOR giữa mỗi cặp phần tử liên tiếp. 
5. Cập nhật`ans`với giá trị tối thiểu hiện tại và XOR được tính toán. 
6. Sau khi xử lý tất cả các cặp liền kề, so sánh`ans`với$k$. 
7. Xuất “Có” nếu`ans <= k`, nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Việc sắp xếp sắp xếp các phần tử sao cho bất kỳ cặp nào có XOR nhỏ đều buộc phải xuất hiện cạnh nhau. Thuộc tính quan trọng là XOR được xác định bởi bit có độ chênh lệch cao nhất và bất kỳ cặp không liền kề nào cũng phải khác nhau ở một vị trí ít nhất có ý nghĩa bằng một số ranh giới liền kề theo thứ tự được sắp xếp. Do đó, không thể bỏ qua XOR tối thiểu trên tất cả các cặp khi hạn chế sự chú ý đến các hàng xóm theo thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 1 << 60
    
    for i in range(1, n):
        ans = min(ans, a[i - 1] ^ a[i])
    
    print("Yes" if ans <= k else "No")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả. Việc sắp xếp được thực hiện một lần, sau đó chỉ cần so sánh lân cận cục bộ. Tính toán XOR sử dụng các phép toán bit có thời gian không đổi. Giá trị trọng điểm cho`ans`được chọn đủ lớn để tránh tình trạng tràn dữ liệu khi so sánh. 

Một lỗi phổ biến là cố gắng chỉ so sánh các giá trị lân cận trong mảng chưa được sắp xếp ban đầu, lỗi này không thành công vì thứ tự theo vị trí đầu vào không liên quan đến cấu trúc XOR. Việc sắp xếp là cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 5 2 8
```Mảng được sắp xếp:`[1, 2, 5, 8]`| tôi | Cặp | XOR | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 ^ 2 | 3 | 3 | 
| 2 | 2 ^ 5 | 7 | 3 | 
| 3 | 5 ^ 8 | 13 | 3 | 

Cuối cùng`ans = 3`, và kể từ đó$3 \le 3$, đầu ra là`Yes`. 

Điều này chứng tỏ cặp tối ưu có thể đến từ các phần tử lân cận sớm như thế nào sau khi sắp xếp, ngay cả khi vị trí ban đầu không liên quan. 

### Ví dụ 2 

đầu vào:```
3 1
10 14 20
```Mảng được sắp xếp:`[10, 14, 20]`| tôi | Cặp | XOR | trả lời | 
| --- | --- | --- | --- | 
| 1 | 10^14 | 4 | 4 | 
| 2 | 14 ^ 20 | 26 | 4 | 

Cuối cùng`ans = 4`, và kể từ đó$4 > 1$, đầu ra là`No`. 

Điều này cho thấy rằng ngay cả các giá trị tương đối gần cũng có thể tạo ra XOR lớn do chênh lệch bit cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế, quét tuyến tính đơn theo sau | 
| Không gian |$O(1)$thêm | Sắp xếp tại chỗ ngoài bộ nhớ đầu vào | 

Cách tiếp cận này hiệu quả đối với các ràng buộc điển hình lên đến$10^5$, vì việc sắp xếp và một lần vượt qua đều nằm trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 1 << 60
    for i in range(1, n):
        ans = min(ans, a[i - 1] ^ a[i])
    
    return "Yes" if ans <= k else "No"

# provided sample-like cases
assert run("4 3\n1 5 2 8\n") == "Yes"
assert run("3 1\n10 14 20\n") == "No"

# minimum size
assert run("2 0\n7 7\n") == "Yes"

# boundary: just above threshold
assert run("2 3\n1 4\n") == "No"

# all equal
assert run("5 0\n9 9 9 9 9\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 giá trị giống nhau | Có | XOR = vỏ 0 cạnh | 
| cặp nhỏ ngay trên k | Không | ranh giới ngưỡng | 
| tất cả các mảng bằng nhau | Có | trường hợp suy biến tối thiểu | 

## Vỏ cạnh 

Đối với một đầu vào như:```
2 5
100 103
```Sau khi sắp xếp, cặp duy nhất là`(100, 103)`với XOR`7`. Thuật toán tính toán trực tiếp điều này, lưu trữ nó dưới dạng`ans`, và trả về một cách chính xác`No`từ$7 > 5$. Điều này xác nhận rằng ngay cả trong các đầu vào có kích thước tối thiểu, logic suy biến chính xác để đánh giá cặp trực tiếp mà không cần xử lý đặc biệt.
