---
title: "CF 105831A - \u0424\u0438\u043a\u0441\u0438\u043a\u0438 \u0438 \u043f\u0430\u0440\u043e\u0432\u043e\u0439 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044c"
description: "Chúng ta có một số chẵn người tham gia, mỗi người có một giá trị nguyên khác 0 thể hiện hiệu quả của họ. Chúng ta phải ghép chúng lại để mỗi người tham gia được sử dụng đúng một cặp."
date: "2026-06-21T04:21:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105831
codeforces_index: "A"
codeforces_contest_name: "4inazezContest"
rating: 0
weight: 105831
solve_time_s: 39
verified: true
draft: false
---

[CF 105831A - \u0424\u0438\u043a\u0441\u0438\u043a\u0438 \u0438 \u043f\u0430\u0440\u043e\u0432\u043e\u0439 \u0434\u0432\u0438\u0433\u0430\u0442\u0435\u043b\u044c](https://codeforces.com/problemset/problem/105831/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chẵn người tham gia, mỗi người có một giá trị nguyên khác 0 thể hiện hiệu quả của họ. Chúng ta phải ghép chúng lại để mỗi người tham gia được sử dụng đúng một cặp. Điểm của một cặp được xác định là tích của hai giá trị trong cặp đó và mục tiêu là chọn cặp sao cho tổng tổng của tất cả các sản phẩm trong cặp sẽ lớn nhất. 

Khó khăn chính là các quyết định ghép nối tương tác trên toàn cầu. Một phần tử duy nhất có thể được ghép nối với bất kỳ phần tử nào khác và lựa chọn ban đầu kém có thể cản trở cấu hình toàn cầu tốt hơn. Vì các giá trị có thể âm hoặc dương nên sự tương tác giữa các dấu hiệu cũng quan trọng như độ lớn. 

Kích thước đầu vào có thể lên tới 10^6 phần tử. Bất kỳ giải pháp nào tệ hơn O(n log n) đều có nguy cơ hết thời gian và thậm chí các công trình O(n^2) cũng hoàn toàn không khả thi. Hạn chế về bộ nhớ cũng buộc chúng ta tránh lưu trữ các cấu trúc phụ trợ lớn ngoài không gian tuyến tính. 

Trường hợp cạnh tinh tế phát sinh khi có liên quan đến số âm. Ví dụ: ghép hai số âm sẽ tạo ra đóng góp dương, trong khi ghép số âm với số dương sẽ tạo ra đóng góp âm làm giảm tổng số tiền. Một cách tiếp cận tham lam ngây thơ ghép các phần tử liền kề hoặc ghép đôi một cách tùy tiện có thể thất bại nặng nề. 

Hãy xem xét đầu vào:```
-5 -1 2 4
```Một cặp đơn giản như (-5, 2) và (-1, 4) cho -10 + -4 = -14. 

Nhưng ghép nối (-5, -1) và (2, 4) cho kết quả 5 + 8 = 13, là mức tối ưu. 

Điều này cho thấy việc phân nhóm nhận biết dấu hiệu là cần thiết. 

Một trường hợp khác là khi tất cả các số đều dương hoặc tất cả đều âm. Trong cả hai trường hợp, cấu trúc ghép nối tối ưu đều đơn giản hóa, nhưng thuật toán chung vẫn phải xử lý chúng một cách thống nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cấu hình ghép nối có thể có. Điều này có thể được coi là tạo ra tất cả các kết quả khớp hoàn hảo của n phần tử và tính tổng sản phẩm cho mỗi phần tử. Số lượng các kết quả khớp như vậy là (n - 1) × (n - 3) × ... × 1, tăng theo cấp số nhân. Ngay cả với n = 20, điều này vẫn không thể thực hiện được. 

Lý do vũ lực là chính xác là vì nó khám phá tất cả các phân vùng hợp lệ thành từng cặp và đánh giá mục tiêu một cách trực tiếp. Tuy nhiên, nó thất bại vì số lượng cặp kết hợp bùng nổ theo kiểu tổ hợp và không có sự cắt tỉa nào tránh được việc xem xét lại các lựa chọn cấu trúc tương đương. 

Quan sát quan trọng là sự đóng góp của một cặp chỉ phụ thuộc vào chính các giá trị chứ không phụ thuộc vào vị trí của chúng. Điều này cho thấy chúng ta nên sắp xếp lại các phần tử một cách tự do. Sau khi được sắp xếp, cấu trúc ghép nối tối ưu sẽ trở nên mang tính quyết định: các số dương lớn phải được ghép với nhau, các số âm lớn phải được ghép với nhau và các dấu hiệu trộn lẫn thường có hại trừ khi bị ép buộc. 

Để chính thức hóa điều này, việc sắp xếp mảng cho thấy chiến lược tối ưu là ghép các phần tử liền kề theo thứ tự được sắp xếp. Điều này có hiệu quả vì việc sắp xếp lại bất kỳ cặp chéo nào thành các cặp được sắp xếp không chéo sẽ không bao giờ làm giảm tổng số tiền, một đối số trao đổi tiêu chuẩn. Theo trực giác, việc ghép đôi các thái cực giúp ổn định các đóng góp: các cực âm khuếch đại lẫn nhau một cách tích cực và các cực dương bảo toàn cường độ khi được nhóm lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n-1)!!) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. 

Điều này đảm bảo rằng các phần tử có dấu và cường độ tương tự nằm liền kề nhau, điều này cần thiết để xuất hiện cấu trúc ghép nối tối ưu. 
2. Khởi tạo tổng hiện có về 0. 
3. Lặp lại mảng đã sắp xếp theo từng bước hai, lấy các cặp liên tiếp. 

Mỗi cặp (a[i], a[i+1]) được thêm dưới dạng a[i] × a[i+1]. 

Quy tắc ghép nối này là an toàn vì bất kỳ sai lệch nào so với lân cận sẽ tạo ra một cấu trúc giao nhau có thể được trao đổi mà không làm giảm tổng số tiền. 
4. Tích lũy tất cả các sản phẩm theo cặp vào câu trả lời cuối cùng. 
5. Xuất số tiền tích lũy. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên một đối số trao đổi qua các cặp. Giả sử hai cặp được hình thành bằng cách sử dụng các phần tử a ∼ b ₫ c ₫ d nhưng theo kiểu giao nhau: (a, c) và (b, d). Cặp thay thế (a, b) và (c, d) không bao giờ mang lại tổng nhỏ hơn, vì: 

(a·b + c·d) − (a·c + b·d) = (a − d)(b − c) ≥ 0 theo thứ tự sắp xếp. 

Điều này cho thấy rằng bất kỳ cặp chéo nào cũng có thể được cải thiện cục bộ thành cặp liền kề mà không làm giảm tổng số tiền. Việc áp dụng nhiều lần phép biến đổi này sẽ loại bỏ tất cả các giao cắt, dẫn đến việc ghép các phần tử liên tiếp theo thứ tự được sắp xếp. Do đó, việc ghép cặp liền kề tham lam là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    a.sort()
    
    ans = 0
    for i in range(0, n, 2):
        ans += a[i] * a[i + 1]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện theo thuật toán trực tiếp. Sắp xếp là bước tiền xử lý quan trọng cho phép ghép nối tham lam. Vòng lặp tiến lên theo gia số hai để đảm bảo mỗi phần tử được sử dụng chính xác một lần. Vì phép nhân số nguyên trong Python là không giới hạn nên không có vấn đề tràn số. 

Một chi tiết triển khai tinh tế là kích thước đầu vào có thể đạt tới một triệu phần tử, vì vậy việc sử dụng sys.stdin.readline là cần thiết. Việc sắp xếp chi phối độ phức tạp, do đó vòng lặp vẫn duy trì chi phí tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
-5 -1 2 4
```Mảng được sắp xếp:```
-5 -1 2 4
```| Bước | Cặp | Sản phẩm | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | (-5, -1) | 5 | 5 | 
| 2 | (2, 4) | 8 | 13 | 

Dấu vết này cho thấy rằng việc nhóm các cực trị cùng dấu sẽ tối đa hóa mức tăng. Cặp âm chuyển thành đóng góp dương, trong khi cặp dương duy trì độ lớn. 

### Ví dụ 2 

đầu vào:```
-3 -2 -1 1 2 3
```Mảng được sắp xếp:```
-3 -2 -1 1 2 3
```| Bước | Cặp | Sản phẩm | Tổng Chạy | 
| --- | --- | --- | --- | 
| 1 | (-3, -2) | 6 | 6 | 
| 2 | (-1, 1) | -1 | 5 | 
| 3 | (2, 3) | 6 | 11 | 

Ví dụ này chứng minh rằng ngay cả khi việc trộn các dấu hiệu ở giữa là điều không thể tránh khỏi sau khi sắp xếp, tính liền kề vẫn thực thi cấu trúc tốt nhất có thể đạt được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; ghép nối là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng và hoạt động tại chỗ | 

Các ràng buộc lên tới 10^6 phần tử khiến cho việc sắp xếp chỉ được chấp nhận trong 2 giây trong Python nếu được triển khai với Timsort tích hợp sẵn và chi phí tối thiểu. Việc quét tuyến tính sau đó không đáng kể so với việc sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    
    import builtins
    orig = builtins.input
    builtins.input = lambda: sys.stdin.readline().rstrip("\n")
    
    try:
        solve()
    finally:
        builtins.input = orig
    
    return output.getvalue().strip()

# provided sample
assert run("4\n-5 -1 2 4\n") == "13"

# all positive
assert run("2\n1 2\n") == "2"

# all negative
assert run("2\n-1 -2\n") == "2"

# mixed
assert run("6\n-3 -2 -1 1 2 3\n") == "11"

# minimum case
assert run("2\n-5 7\n") == "-35"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 -5 -1 2 4 | 13 | nhóm tối ưu cơ bản | 
| 2 1 2 | 2 | cặp tích cực đơn giản | 
| 2 -1 -2 | 2 | hành vi ghép đôi tiêu cực | 
| 6 -3 -2 -1 1 2 3 | 11 | sự đúng đắn của cấu trúc hỗn hợp | 
| 2 -5 7 | -35 | đầu vào hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Đối với đầu vào chỉ chứa số âm như`-4 -3 -2 -1`, sắp xếp sản lượng`-4 -3 -2 -1`và ghép nối các phần tử liền kề mang lại`( -4, -3 ) = 12`Và`( -2, -1 ) = 2`, tổng cộng 14. Bất kỳ sự ghép cặp thay thế nào chẳng hạn như các cặp giao nhau sẽ làm giảm tổng tích vì việc ghép các cường độ gần nhau hơn luôn tạo ra ít tương tác tiêu cực hơn. 

Đối với một đầu vào dấu hiệu hỗn hợp như`-10 -1 2 3`, việc ghép nối được sắp xếp sẽ tạo ra`(-10, -1) = 10`Và`(2, 3) = 6`, tổng cộng 16. Một cặp chéo như`(-10, 2)`Và`(-1, 3)`sản lượng`-20 + -3 = -23`, cho thấy việc trộn không chính xác sẽ gây hại cho mục tiêu mạnh đến mức nào. 

Đối với đầu vào có các dấu hiệu xen kẽ như`-2 1 -1 2`, sắp xếp thành`-2 -1 1 2`đảm bảo các nhóm thuật toán phủ định trước, tránh sự mất ổn định của việc luân phiên dấu.
