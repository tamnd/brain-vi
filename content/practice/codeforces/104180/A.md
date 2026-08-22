---
title: "CF 104180A - Dự báo thời tiết"
description: "Chúng ta được cấp một chuỗi có độ dài cố định gồm 28 số thực, mỗi số biểu thị xác suất có mưa vào một ngày cụ thể trong tháng Hai. Mỗi giá trị nằm trong khoảng từ 0 đến 1. Một ngày chỉ được coi là “mưa” nếu xác suất của ngày đó đáp ứng hoặc vượt quá 0,8."
date: "2026-07-02T00:42:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104180
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 2 (Beginner)"
rating: 0
weight: 104180
solve_time_s: 56
verified: true
draft: false
---

[CF 104180A - Dự báo thời tiết](https://codeforces.com/problemset/problem/104180/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi có độ dài cố định gồm 28 số thực, mỗi số biểu thị xác suất có mưa vào một ngày cụ thể trong tháng Hai. Mỗi giá trị nằm trong khoảng từ 0 đến 1. Một ngày chỉ được coi là “mưa” nếu xác suất của ngày đó đáp ứng hoặc vượt quá 0,8. Nhiệm vụ chỉ đơn giản là đếm xem có bao nhiêu trong số 28 giá trị này thỏa mãn điều kiện ngưỡng đó. 

Cấu trúc của đầu vào loại bỏ gần như toàn bộ quyền tự do về mặt thuật toán. Không có hành vi động, không có sự phụ thuộc giữa các ngày và không có sự tổng hợp nào ngoài việc đếm. Đầu ra là một số nguyên duy nhất, số lượng giá trị đủ điều kiện. 

Trong thực tế, các ràng buộc là cực kỳ nhỏ vì kích thước đầu vào được cố định ở mức 28. Ngay cả khi điều này được khái quát hóa, thao tác chúng ta cần cho mỗi phần tử là so sánh theo thời gian không đổi, do đó, ngay cả đối với đầu vào rất lớn, đây sẽ là thời gian tuyến tính và nhanh chóng. 

Các vấn đề nhỏ duy nhất đến từ việc phân tích cú pháp đầu vào dấu phẩy động một cách chính xác. Các giá trị là biểu diễn thập phân, do đó việc triển khai mã hóa đầu vào không chính xác hoặc dựa vào phân tích cú pháp số nguyên sẽ không thành công. Một trường hợp cạnh khác là xử lý ranh giới ở mức chính xác 0,8, phải được bao gồm chứ không được loại trừ. 

Một sai lầm ngây thơ sẽ là coi ngưỡng này lớn hơn 0,8 thay vì lớn hơn hoặc bằng. Ví dụ: giá trị như 0,8 sẽ được tính. Một vấn đề tiềm ẩn khác là phân tách đầu vào không chính xác nếu xuất hiện nhiều dấu cách hoặc biến thể định dạng, nhưng tính năng phân tích cú pháp dựa trên mã thông báo tiêu chuẩn sẽ tránh được điều này. 

## Phương pháp tiếp cận 

Phương pháp brute-force cũng là phương pháp tối ưu trong bài toán này. Chúng tôi lặp qua tất cả 28 giá trị, kiểm tra từng giá trị theo ngưỡng 0,8 và duy trì bộ đếm đang chạy xem có bao nhiêu giá trị thỏa mãn điều kiện. Mỗi lần kiểm tra là thời gian không đổi, do đó tổng công việc tỷ lệ thuận với số lượng giá trị. 

Không có cấu trúc nào để khai thác ngoài quá trình quét trực tiếp này. Quan sát chính là vấn đề về cơ bản là hoạt động lọc trên danh sách có kích thước cố định. Vì không có quá trình tiền xử lý hoặc chuyển đổi nào sẽ làm giảm công việc trong tương lai nên bất kỳ thuật toán nào cũng phải kiểm tra từng phần tử ít nhất một lần, làm cho một lần vượt qua trở nên tối ưu. 

Chế độ xem brute-force trở nên tối ưu vì kích thước đầu vào không đổi và nhỏ, đồng thời vì quy tắc quyết định độc lập với mỗi phần tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(28) | O(1) | Đã chấp nhận | 
| Bộ đếm một lần | O(28) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc toàn bộ dòng đầu vào và chia nó thành các mã thông báo đại diện cho 28 giá trị dấu phẩy động. Điều này đảm bảo chúng tôi tách biệt chính xác từng xác suất bất kể khoảng cách. 
2. Khởi tạo bộ đếm về 0. Bộ đếm này sẽ theo dõi có bao nhiêu giá trị đáp ứng điều kiện ngày mưa. 
3. Lặp lại từng giá trị được phân tích cú pháp. Đối với mỗi giá trị, hãy chuyển đổi nó thành số dấu phẩy động. 
4. So sánh giá trị với ngưỡng 0,8. Nếu giá trị lớn hơn hoặc bằng 0,8, hãy tăng bộ đếm. Điều kiện bình đẳng quan trọng vì bản thân 0,8 được định nghĩa là mưa. 
5. Sau khi xử lý tất cả 28 giá trị, xuất bộ đếm cuối cùng. 

Tính chính xác phụ thuộc vào thực tế là mỗi ngày đều được đánh giá độc lập. Không có tính toán trung gian nào ảnh hưởng đến các quyết định trong tương lai. 

### Tại sao nó hoạt động 

Mỗi phần tử được phân loại chỉ dựa trên một vị từ cố định: nó có ít nhất là 0,8 hay không. Thuật toán đánh giá vị từ này chính xác một lần cho mỗi giá trị đầu vào và tích lũy số lượng kết quả đúng. Vì phép cộng vượt quá số lượng có tính kết hợp và độc lập giữa các phần tử nên tổng cuối cùng khớp chính xác với số ngày đủ điều kiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().split()
    count = 0
    for x in data:
        if float(x) >= 0.8:
            count += 1
    print(count)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả mã thông báo trong một lần, tránh mọi logic phân tích cú pháp thủ công. Mỗi mã thông báo được chuyển đổi độc lập thành số float và việc so sánh được áp dụng ngay lập tức. 

Một cạm bẫy phổ biến là sử dụng sự bất bình đẳng nghiêm ngặt`>`thay vì`>=`, điều này sẽ loại trừ không chính xác các giá trị ngưỡng chính xác. Một vấn đề tế nhị khác là quên chuyển đổi chuỗi thành số float, điều này sẽ dẫn đến so sánh từ điển thay vì so sánh số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1 0.25 0.22 0.64 0.43 0 0.99 0.87 0.28 0.83 0.77 0.92 0.45 0.60 0.887 0.935 0.353 0.182
```Chúng tôi quét từng giá trị và đánh dấu những giá trị đó ≥ 0,8. 

| Ngày | Giá trị | ≥ 0,8? | Đếm | 
| --- | --- | --- | --- | 
| 1 | 0 | Không | 0 | 
| 9 | 0,8 | Có | 1 | 
| 10 | 0,9 | Có | 2 | 
| 11 | 1 | Có | 3 | 
| 17 | 0,99 | Có | 4 | 
| 18 | 0,87 | Có | 5 | 
| 20 | 0,83 | Có | 6 | 
| 22 | 0,92 | Có | 7 | 
| 25 | 0,887 | Có | 8 | 
| 26 | 0,935 | Có | 9 | 

Đầu ra cuối cùng là 9. 

Điều này xác nhận rằng các giá trị ngưỡng chính xác như 0,8 được bao gồm và các giá trị đủ điều kiện rải rác được tích lũy chính xác. 

### Ví dụ 2 

đầu vào:```
0.8 0.8 0.8 0.79 0.81 0.7 0.9 0.6 0.85 0.2 0.8 0.1 0.8 0.8 0.3 0.4 0.8 0.8 0.8 0.05 0.95 0.8 0.79 0.8 0.8 0.8 0.8 0.8
```| Ngày | Giá trị | ≥ 0,8? | Đếm | 
| --- | --- | --- | --- | 
| 1 | 0,8 | Có | 1 | 
| 2 | 0,8 | Có | 2 | 
| 4 | 0,79 | Không | 2 | 
| 5 | 0,81 | Có | 3 | 
| 7 | 0,9 | Có | 4 | 
| 9 | 0,85 | Có | 5 | 
| 11 | 0,8 | Có | 6 | 
| 13 | 0,8 | Có | 7 | 
| 21 | 0,95 | Có | 8 | 
| 22 | 0,8 | Có | 9 | 

Đầu ra cuối cùng là 9. 

Ví dụ này nhấn mạnh việc so sánh ngưỡng lặp đi lặp lại và xác nhận rằng nhiều giá trị biên giống hệt nhau được xử lý một cách nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(28) | Mỗi giá trị trong số 28 giá trị được xử lý chính xác một lần với các phép toán có thời gian không đổi | 
| Không gian | O(1) | Chỉ sử dụng một bộ đếm nhỏ và các biến tạm thời | 

Việc tính toán trong thực tế là thời gian không đổi vì kích thước đầu vào là cố định. Ngay cả khi được khái quát hóa, việc quét tuyến tính đơn lẻ trên các giá trị dấu phẩy động vẫn dễ dàng phù hợp với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    data = _sys.stdin.readline().split()
    count = 0
    for x in data:
        if float(x) >= 0.8:
            count += 1
    return str(count)

# provided sample
assert run("0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9 1 0.25 0.22 0.64 0.43 0 0.99 0.87 0.28 0.83 0.77 0.92 0.45 0.60 0.887 0.935 0.353 0.182\n") == "9"

# all below threshold
assert run("0 " * 28) == "0"

# all above threshold
assert run("0.8 " * 28) == "28"

# alternating boundary values
assert run("0.8 0.79 " * 14) == "14"

# mixed precision case
assert run("0.799999 0.8000001 " * 14) == "14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | 0 | không có kết quả dương tính giả | 
| tất cả 0,8 | 28 | tính đúng đắn của ranh giới bao gồm | 
| xen kẽ 0,8/0,79 | 14 | xử lý ranh giới lặp đi lặp lại | 
| trường hợp cạnh chính xác nổi | 14 | sự mạnh mẽ của sự so sánh | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi giá trị chính xác là 0,8. Đối với đầu vào như:```
0.8 0.8 0.8 ... (28 times)
```thuật toán đánh giá từng giá trị, tìm thấy tất cả thỏa mãn`>= 0.8`và tăng bộ đếm mỗi lần. Đầu ra cuối cùng là 28, xác nhận việc bao gồm đúng các giá trị biên. 

Một trường hợp cạnh khác liên quan đến các giá trị cực kỳ gần với 0,8 do biểu diễn dấu phẩy động, chẳng hạn như 0,799999 hoặc 0,8000001. Việc so sánh vẫn được thực hiện trực tiếp trên các chuyển đổi thả nổi và thuật toán chỉ phân loại chính xác những chuyển đổi đáp ứng hoặc vượt quá ngưỡng sau khi phân tích cú pháp. 

Trường hợp thứ ba là khi không có giá trị nào đủ tiêu chuẩn, chẳng hạn như tất cả số không. Bộ đếm vẫn ở mức 0 trong suốt quá trình lặp và đầu ra chính xác là 0, cho thấy thuật toán xử lý các tình huống tích lũy trống mà không có cách viết hoa đặc biệt.
