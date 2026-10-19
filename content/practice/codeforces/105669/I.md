---
title: "CF 105669I - Phân vùng"
description: "Chúng ta được cho một số nguyên duy nhất ( n ). Nhiệm vụ là đếm xem có bao nhiêu cách khác nhau mà chúng ta có thể biểu diễn ( n ) dưới dạng tổng của các số nguyên dương, trong đó thứ tự là quan trọng."
date: "2026-06-26T09:56:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105669
codeforces_index: "I"
codeforces_contest_name: "Combinatorics Contest - Brazilian ICPC Summer School 2025"
rating: 0
weight: 105669
solve_time_s: 37
verified: true
draft: false
---

[CF 105669I - Phân vùng](https://codeforces.com/problemset/problem/105669/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên duy nhất\( n \). Nhiệm vụ là đếm xem chúng ta có thể diễn đạt được bao nhiêu cách khác nhau\( n \)dưới dạng tổng của các số nguyên dương, trong đó thứ tự là quan trọng. Ví dụ, viết\( 3 \)BẰNG\( 1+2 \)được coi là khác với\( 2+1 \), và thậm chí cả thuật ngữ đơn\( 3 \)bản thân nó cũng là một đại diện hợp lệ. Mọi dãy số nguyên dương có tổng bằng chính xác\( n \)đóng góp một cách. 

Đây là bài toán kinh điển về việc đếm các tập hợp số nguyên có thứ tự của\( n \). Câu trả lời phát triển nhanh chóng và kết quả cuối cùng phải được báo cáo theo modulo\( 10^9+7 \). 

Ràng buộc\( n \le 10^6 \)là tín hiệu quan trọng ở đây. Bất kỳ cách tiếp cận nào liệt kê các phân vùng hoặc xây dựng tất cả các thành phần một cách rõ ràng đều không thể thực hiện được vì ngay cả đối với\( n \), số lượng các tác phẩm là theo cấp số nhân trong\( n \). Do đó, cấu trúc của bài toán phải được rút gọn thành một dạng đệ quy đơn giản hoặc dạng đóng có thể tính toán được trong thời gian tuyến tính. 

Một trường hợp cạnh tinh tế xuất hiện tại\( n = 1 \). Có chính xác một thành phần:\([1]\). Một cách giải thích ngây thơ không cho phép sử dụng chính số đó sẽ tạo ra số 0 không chính xác. Một lỗi phổ biến khác là nhầm lẫn giữa các bố cục có thứ tự với các phân vùng không có thứ tự; ví dụ, đối với\( n=3 \), câu trả lời đúng là\(4\), không\(3\), vì đơn hàng được tính. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng tạo ra mọi chuỗi số nguyên dương có thể có tổng bằng\( n \). Từ quan điểm đệ quy, ở mỗi bước chúng ta chọn phép triệu tiếp theo\( k \ge 1 \)và tái diễn\( n-k \). Điều này tạo ra hệ số phân nhánh lên tới\( n \)thì ở cấp độ cao nhất\( n-1 \), vân vân. Tổng số trạng thái đệ quy mở rộng gần bằng số lượng các hợp phần, tức là\( 2^{n-1} \). Vì\( n = 10^6 \), điều này hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng là bài toán không phụ thuộc vào các giá trị thực được chọn mà chỉ phụ thuộc vào tổng còn lại. Đặt \(f(n) \) là số thành phần hợp lệ của\( n \). Bất kỳ thành phần nào của\( n \)hoặc bắt đầu bằng\( 1 \), hoặc\( 2 \), hoặc\( 3 \), v.v. cho đến\( n \). Điều này gây ra sự tái phát\[
f(n) = f(n-1) + f(n-2) + \dots + f(0),
\]với quy ước \( f(0) = 1 \), biểu thị phân tách rỗng. 

Việc tính toán trực tiếp cho phép tái diễn này vẫn là \( O(n^2) \) nếu được thực hiện một cách đơn giản, nhưng nó có thể được đơn giản hóa bằng cách nhận thấy mối quan hệ kính thiên văn. Trừ đi sự lặp lại của \( f(n-1) \) từ \( f(n) \) mang lại\[
f(n) - f(n-1) = f(n-1),
\]đơn giản hóa để\[
f(n) = 2 \cdot f(n-1).
\]Điều này thu gọn toàn bộ cấu trúc thành một quá trình nhân đôi đơn giản: mỗi thành phần số nguyên của\( n-1 \)có thể không thay đổi với số dẫn đầu\( 1 \), hoặc được mở rộng theo cách bảo toàn song ánh với các thành phần của\( n \). Điều này thiết lập sự tương ứng một-một để nhân đôi số lượng ở mỗi bước. 

Do đó chúng ta có được dạng đóng:\[
f(n) = 2^{n-1}.
\]Điều này có thể được tính theo thời gian tuyến tính thông qua phép truy hồi đơn giản hoặc theo thời gian logarit thông qua phép lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Liệt kê lực lượng vũ phu | \( O(2^n) \) | \( O(n) \) | Quá chậm | 
| DP tổng tiền tố trên các trạng thái | \( O(n^2) \) | \( O(n) \) | Quá chậm | 
| dạng đóng\( 2^{n-1} \)| \( O(\log n) \) | \( O(1) \) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi thừa nhận rằng mọi cách xây dựng hợp lệ của\( n \)tương ứng với một chuỗi cắt giữa các số nguyên\( 1 \)bởi vì\( n \). Giữa mỗi cặp vị trí liền kề, chúng ta đặt một vết cắt hoặc không, điều này sẽ xác định xem các số được nhóm hay tách riêng. Điều này chuyển đổi vấn đề thành một quá trình quyết định nhị phân qua\( n-1 \)những khoảng trống. 

2. Vì mỗi\( n-1 \)các vị trí độc lập có hai lựa chọn, tổng số cấu hình là\( 2^{n-1} \). Đây là phép biến đổi tổ hợp trung tâm giúp tránh hoàn toàn đệ quy. 

3. Chúng tôi tính toán \( 2^{n-1} \bmod (10^9+7) \) bằng cách sử dụng lũy ​​thừa nhanh. Số mũ lớn nên chúng ta liên tục bình phương cơ số và giảm số mũ đi một nửa ở mỗi bước. 

4. Giá trị cuối cùng được trả về dưới dạng câu trả lời. 

### Tại sao nó hoạt động 

Điều bất biến là mọi thành phần của\( n \)tương ứng duy nhất với một tập con của\( n-1 \)dấu phân cách có thể có giữa các số nguyên liên tiếp. Việc chọn một tập hợp con sẽ xác định chính xác vị trí kết thúc của phân đoạn và do đó xác định duy nhất một phân vùng có thứ tự hợp lệ. Không có hai tập hợp con khác nhau tạo ra cùng một thành phần và mỗi thành phần tương ứng với chính xác một tập hợp con. Lời khẳng định này đảm bảo tính đúng đắn của\( 2^{n-1} \)đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n = int(input().strip())

# number of compositions of n is 2^(n-1)
if n == 0:
    print(1)
else:
    print(pow(2, n - 1, MOD))
```Việc triển khai áp dụng trực tiếp phép lũy thừa mô-đun. Trường hợp đặc biệt\( n = 0 \)được đưa vào để đảm bảo tính đầy đủ, mặc dù vấn đề đảm bảo\( n \ge 1 \). 

Sự đơn giản hóa chính là tránh bất kỳ bảng DP hoặc đệ quy nào. Toàn bộ cấu trúc của bài toán quy về việc đếm các lựa chọn nhị phân độc lập qua các khoảng trống giữa các số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 3 

Chúng tôi theo dõi cách các tác phẩm phát sinh từ việc giải thích khoảng cách nhị phân. 

| Quyết định khoảng cách (giữa 1 và 2, 2 và 3) | Thành phần kết quả | 
|---|---| 
| cắt, cắt | 1+1+1 | 
| cắt, không cắt | 1 + 2 | 
| không cắt, cắt | 2 + 1 | 
| không cắt, không cắt | 3 | 

Điều này tạo ra 4 tác phẩm hợp lệ, phù hợp\( 2^{2} = 4 \). 

Dấu vết này xác nhận rằng mọi tập hợp con của khoảng trống đều tương ứng với chính xác một phân tách hợp lệ. 

### Ví dụ 2: n = 4 

| Quyết định khoảng cách | Thành phần | 
|---|---| 
| 1,1,1 | 1+1+1+1 | 
| 1,1,0 | 1+1+2 | 
| 1,0,1 | 1+2+1 | 
| 1,0,0 | 1+3 | 
| 0,1,1 | 2+1+1 | 
| 0,1,0 | 2+2 | 
| 0,0,1 | 3+1 | 
| 0,0,0 | 4 | 

Có 8 kết quả phù hợp\( 2^{3} = 8 \). Cấu trúc của các quyết định về khoảng cách độc lập được thể hiện đầy đủ ở đây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \( O(\log n) \) | tính lũy thừa nhanh\( 2^{n-1} \)theo các bước logarit | 
| Không gian | \( O(1) \) | chỉ có một số số nguyên được duy trì | 

Ràng buộc\( n \le 10^6 \)được xử lý dễ dàng vì phép lũy thừa theo mô đun cực kỳ nhanh ngay cả đối với số mũ lớn. Giải pháp chạy tốt trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline().strip())
    if n == 0:
        return "1"
    return str(pow(2, n - 1, MOD))

# provided samples
assert run("1") == "1"
assert run("3") == "4"

# custom cases
assert run("2") == "2", "1+1, 2"
assert run("4") == "8", "check small correctness"
assert run("10") == str(pow(2, 9, MOD)), "power correctness"
assert run("1000000") == str(pow(2, 999999, MOD)), "large stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1 | 1 | ranh giới tối thiểu | 
| 2 | 2 | thành phần không tầm thường nhỏ nhất | 
| 4 | 8 | tính nhất quán tăng trưởng theo cấp số nhân | 
| 1000000 | 2^999999 mod | hiệu suất và số mũ lớn | 

## Vỏ cạnh 

### n = 1 

Đối với đầu vào`1`, thuật toán tính toán\( 2^{0} = 1 \). Thành phần duy nhất là phần tử duy nhất`[1]`, vậy kết quả đúng Giải thích khoảng cách có khoảng cách bằng 0, nghĩa là tồn tại chính xác một cấu hình. 

### n rất lớn (lên tới 10^6) 

cho`n = 1000000`, bước lũy thừa sẽ chạy trong \( O(\log n) \). Phép tính liên tục bình phương cơ số 2 và giảm số mũ, không bao giờ lưu trữ các giá trị trung gian lớn ngoài mô đun. Điều này đảm bảo thuật toán vẫn ổn định và nhanh chóng ngay cả ở kích thước đầu vào tối đa.
