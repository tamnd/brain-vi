---
title: "CF 1236B - Alice và Danh sách Quà tặng"
description: "Chúng tôi được cung cấp một số loại mặt hàng, trong đó mỗi loại có số lượng cung cấp không giới hạn. Chúng tôi cũng có một số hộp riêng biệt, mỗi hộp thuộc về một người bạn khác nhau, vì vậy các hộp được gắn nhãn và không thể hoán đổi. Đối với mỗi loại vật phẩm, Alice chọn một tập hợp con các hộp để đặt loại đó vào."
date: "2026-06-13T19:17:37+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1236
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 593 (Div. 2)"
rating: 1500
weight: 1236
solve_time_s: 371
verified: true
draft: false
---

[CF 1236B - Alice và Danh sách Quà tặng](https://codeforces.com/problemset/problem/1236/B) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 6 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số loại mặt hàng, trong đó mỗi loại có số lượng cung cấp không giới hạn. Chúng tôi cũng có một số hộp riêng biệt, mỗi hộp thuộc về một người bạn khác nhau, vì vậy các hộp được gắn nhãn và không thể hoán đổi. 

Đối với mỗi loại vật phẩm, Alice chọn một tập hợp con các hộp để đặt loại đó vào. Bên trong bất kỳ hộp nào, cô ấy không được phép đặt nhiều hơn một vật phẩm cùng loại, nhưng hạn chế đó sẽ tự động được thỏa mãn vì một loại được đặt vào hộp hoặc không được đặt ở đó. Hạn chế chính là mỗi loại phải xuất hiện trong ít nhất một hộp, vì vậy không có loại nào hoàn toàn không được sử dụng. 

Vì vậy, vấn đề giảm xuống còn việc đếm xem có bao nhiêu cách chúng ta có thể gán cho mỗi loại trong số n loại một tập con không trống của các hộp có nhãn m. 

Mỗi loại đều độc lập về mặt lựa chọn, nhưng yêu cầu chung “mọi loại đều được sử dụng ít nhất một lần ở một nơi nào đó” đã được áp dụng cho từng loại, do đó không có sự kết hợp giữa các loại khác nhau. Sự tinh tế không phải là sự tương tác giữa các loại, mà là diễn giải chính xác rằng mỗi loại chọn một tập hợp con các hộp và tất cả các tập hợp con phải không trống. 

Kích thước đầu vào lên tới 10^9, do đó, bất kỳ cách tiếp cận nào liệt kê các tập hợp con, lặp lại các hộp hoặc sử dụng lập trình động trên n hoặc m đều không thể thực hiện được. Ngay cả O(n) hoặc O(m) trên mỗi trường hợp thử nghiệm cũng quá lớn. Giải pháp phải giảm mọi thứ thành biểu thức dạng đóng có thể tính toán theo thời gian logarit bằng cách sử dụng phép lũy thừa mô-đun. 

Một trường hợp lỗi phổ biến là diễn giải điều kiện không chính xác khi gán cho mỗi hộp một tập hợp con các loại độc lập, dẫn đến việc đếm (2^n - 1)^m hoặc các biểu thức không chính xác tương tự. Một sai lầm khác là quên rằng lựa chọn trống cho mỗi loại không được phép, điều này sẽ bao gồm không có phép gán nào. 

## Phương pháp tiếp cận 

Bắt đầu bằng cách tập trung vào một loại duy nhất. Đối với một loại cố định, chúng tôi quyết định nó xuất hiện trong hộp nào. Vì có m hộp riêng biệt nên mỗi hộp đều được chọn hoặc không được chọn cho loại này. Điều đó mang lại 2^m tập hợp con có thể. Tuy nhiên, tập hợp con trống bị cấm vì mỗi loại phải xuất hiện trong ít nhất một hộp, vì vậy số lựa chọn hợp lệ cho một loại là 2^m - 1. 

Bây giờ hãy xem xét nhiều loại. Mỗi loại tự lựa chọn một tập hợp con các hộp không trống. Vì các lựa chọn cho các loại khác nhau không ảnh hưởng lẫn nhau nên chúng ta nhân số lượng các lựa chọn trên tất cả n loại. Điều đó mang lại (2^m - 1)^n. 

Đây là sự đơn giản hóa cấu trúc quan trọng: vấn đề không phải là việc phân phối các đối tượng trên các hộp mà là việc gán cho mỗi loại một tập hợp con các vùng chứa. Sự độc lập của các loại thu gọn tổ hợp thành một biểu thức lũy thừa đơn giản. 

Ý tưởng mạnh mẽ sẽ là lặp lại tất cả các tập hợp con của các hộp cho từng loại và sau đó thực thi các ràng buộc toàn cục. Đó sẽ là O(2^m * n), điều này là không thể ngay cả với m = 20, chứ đừng nói đến 10^9. Việc quan sát rằng mỗi loại là độc lập sẽ loại bỏ mọi nhu cầu liệt kê. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của các tập hợp con | O(n · 2^m) | O(1) | Quá chậm | 
| Dạng đóng với lũy thừa mô-đun | O(log n + log m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Quan sát rằng đối với một loại cố định, mỗi hộp sẽ độc lập quyết định xem nó có chứa loại đó hay không. Điều này mang lại 2^m bài tập có thể có cho một loại. 
2. Loại trừ trường hợp không có ô nào chứa loại đó, vì mỗi loại phải xuất hiện ít nhất một lần. Điều này làm giảm số đếm xuống còn 2^m − 1. 
3. Hãy xử lý từng loại một cách độc lập vì việc lựa chọn các loại khác nhau không ảnh hưởng lẫn nhau. Sự độc lập này cho phép nhân lên nhiều khả năng trên tất cả các loại. 
4. Nâng số lượng mỗi loại lên lũy thừa n, cho kết quả (2^m − 1)^n. 
5. Tính 2^m modulo MOD bằng cách sử dụng lũy ​​thừa nhị phân nhanh. 
6. Trừ 1 một cách cẩn thận theo số học modulo để tránh các giá trị âm. 
7. Nâng kết quả lên lũy thừa n bằng cách sử dụng lũy ​​thừa mô-đun một lần nữa. 

Tại sao nó hoạt động: mỗi cấu hình hợp lệ được xác định duy nhất bằng cách chọn độc lập, đối với mỗi loại, một tập hợp con các hộp không trống. Có sự song ánh giữa các cấu hình toàn cục và các bộ gồm n tập con khác rỗng của một tập hợp phần tử m. Vì cấu trúc tích Descartes giữ chính xác nên phép nhân số đếm là chính xác và không xảy ra hiện tượng đếm quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modexp(a, e):
    res = 1
    a %= MOD
    while e > 0:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

n, m = map(int, input().split())

# number of non-empty subsets of boxes for one kind
base = modexp(2, m) - 1
base %= MOD

# total ways
ans = modexp(base, n)

print(ans)
```Giải pháp đầu tiên tính toán 2^m modulo MOD bằng cách sử dụng lũy ​​thừa nhanh. Phép trừ 1 sẽ loại bỏ tập con trống và phải được thực hiện modulo MOD để tránh các giá trị âm. Cơ sở kết quả đại diện cho các lựa chọn hợp lệ cho một loại. Sau đó, phép lũy thừa thứ hai nâng cơ số này lên n, phản ánh việc gán các tập con độc lập cho mỗi loại. 

Một cạm bẫy triển khai phổ biến là quên lấy modulo sau khi trừ, điều này có thể tạo ra các giá trị trung gian âm và phá vỡ lũy thừa tiếp theo. Một vấn đề tế nhị khác là giả sử chỉ riêng hàm pow tích hợp của Python là đủ mà không cần xử lý dấu trừ một cách cẩn thận theo số học modulo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
```Ở đây n = 1 và m = 3. Đối với loại đơn, tất cả các tập con không trống của 3 hộp đều hợp lệ. 

| Bước | Giá trị | 
| --- | --- | 
| 2^m | 8 | 
| 2^m - 1 | 7 | 
| (2^m - 1)^n | 7 | 

Đầu ra là 7. 

Điều này xác nhận cách giải thích rằng chúng ta chỉ đơn giản chọn một tập hợp con các hộp không trống cho loại duy nhất. 

### Ví dụ 2 

đầu vào:```
2 2
```Bây giờ n = 2, m = 2. 

| Bước | Giá trị | 
| --- | --- | 
| 2^m | 4 | 
| 2^m - 1 | 3 | 
| (2^m - 1)^n | 9 | 

Mỗi loại trong hai loại chọn độc lập một trong ba tập con không trống của {box1, box2}. 

Điều này khẳng định tính độc lập giữa các loại và cho thấy cấu trúc nhân rộng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n + log m) | Hai phép lũy thừa mô-đun sử dụng lũy ​​thừa nhị phân | 
| Không gian | O(1) | Chỉ có một số lượng biến không đổi được duy trì | 

Các ràng buộc n, m 10^9 làm cho việc lặp trực tiếp không thể thực hiện được, nhưng phép lũy thừa theo thời gian logarit dễ dàng đủ nhanh trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def modexp(a, e):
    res = 1
    a %= MOD
    while e > 0:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    base = modexp(2, m) - 1
    base %= MOD
    return str(modexp(base, n))

# provided samples
assert solve("1 3\n") == "7"

# minimum edge case
assert solve("1 1\n") == "1"

# symmetric case
assert solve("2 2\n") == "9"

# larger small case
assert solve("3 2\n") == str(pow((pow(2,2,MOD)-1),3,MOD))

# large exponent sanity check
assert solve("10 10\n") == str(pow((pow(2,10,MOD)-1),10,MOD))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | độ chính xác cấu hình tối thiểu | 
| 2 2 | 9 | độc lập giữa các loại | 
| 3 2 | tính toán | tính lũy thừa đúng đắn | 
| 10 10 | tính toán | ổn định giá trị lớn | 

## Vỏ cạnh 

Khi m = 1, mỗi loại chỉ có thể được đặt vào một hộp duy nhất, do đó có đúng một tập hợp con hợp lệ. Công thức cho 2^1 − 1 = 1 và nâng lên n giữ nó bằng 1, phù hợp với trực giác rằng tất cả các loại phải được xếp vào ô duy nhất có sẵn. 

Khi n = 1, vấn đề trở thành việc chọn bất kỳ tập con hộp không trống nào cho một loại. Công thức đúng mang lại 2^m − 1. Ví dụ: với m = 3, kết quả là 7, khớp với tất cả các vị trí có thể. 

Một trường hợp thất bại tiềm ẩn là quên phép trừ 1 cho các tập hợp con trống. Nếu sử dụng sai 2^(m·n), nó sẽ đếm quá mức các cấu hình trong đó một số loại không bao giờ được đặt. Cấu trúc chính xác thực thi tính không trống theo từng loại, đó là lý do tại sao phép trừ phải xảy ra trước phép lũy thừa.
