---
title: "CF 102680D-Một"
description: "Nhiệm vụ là phân loại từng số tự nhiên đã cho. Một số là số nguyên tố nếu ước số dương duy nhất của nó là 1 và chính nó. Nó là Hợp số nếu nó có thêm ít nhất một ước số. Giá trị đặc biệt 1 không thuộc loại nào vì nó chỉ có một ước số dương."
date: "2026-08-01T23:31:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102680
codeforces_index: "D"
codeforces_contest_name: "Brookfield Computer Programming Challenge 1"
rating: 0
weight: 102680
solve_time_s: 62
verified: true
draft: false
---

[CF 102680D - Một](https://codeforces.com/problemset/problem/102680/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân loại từng số tự nhiên đã cho. Một số là số nguyên tố nếu ước số dương duy nhất của nó là 1 và chính nó. Nó là Hợp số nếu nó có thêm ít nhất một ước số. Giá trị đặc biệt 1 không thuộc loại nào vì nó chỉ có một ước số dương. 

Đầu vào chứa một số số độc lập. Đối với mỗi số, chúng tôi in chính xác một từ mô tả phân loại của nó. 

Khó khăn chính là kích thước của các giá trị chứ không phải số lượng đầu vào. Có thể chỉ có 100 số, nhưng mỗi số có thể lớn tới 2.000.000.000. Việc kiểm tra mọi ước số có thể có từ 2 cho đến chính số đó sẽ yêu cầu tới 200 tỷ lần kiểm tra cho một giá trị, vượt xa thời gian sẵn có. Ngay cả việc kiểm tra tuyến tính nhiều trường hợp lớn cũng không thực tế. Chúng ta cần khai thác cấu trúc toán học của ước số. 

Giới hạn tìm kiếm số chia hữu ích lớn nhất là căn bậc hai của số. Nếu một số có ước số lớn hơn căn bậc hai của nó thì ước số tương ứng sẽ nhỏ hơn căn bậc hai. Điều này làm giảm không gian tìm kiếm xuống còn khoảng 44.721 lượt kiểm tra cho giá trị tối đa, có thể dễ dàng quản lý đối với 100 trường hợp kiểm thử. 

Trường hợp cạnh đầu tiên là số 1. Đối với đầu vào:```
1
1
```đầu ra đúng là:```
Neither
```Việc triển khai bất cẩn chỉ kiểm tra xem nó có tìm thấy ước số hay không có thể gắn nhãn sai 1 là số nguyên tố vì nó không tìm thấy ước số nào từ 2 trở đi. 

Một trường hợp cạnh khác là một hình vuông hoàn hảo. Đối với đầu vào:```
1
49
```đầu ra đúng là:```
Composite
```Một chương trình chỉ kiểm tra các ước số trong khi bình phương nhỏ hơn căn bậc hai của số đó có thể vô tình bỏ sót ước số 7 và phân loại 49 không chính xác. 

Trường hợp cạnh cuối cùng là số nguyên tố lớn gần với giới hạn:```
1
2000000000
```Đây không phải là số nguyên tố vì nó có nhiều yếu tố. Chỉ kiểm tra một vài ước số nhỏ hoặc sử dụng bảng nguyên tố không đầy đủ có thể cho kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi ước số có thể có từ 2 đến (q-1). Nếu bất kỳ giá trị nào chia hết cho (q), số đó là hợp số. Nếu không thì nó là số nguyên tố. Điều này đúng vì mọi số không phải số nguyên tố ngoại trừ 1 đều có ước số khác 1 và chính nó. 

Vấn đề là thời gian chạy. Với (q = 2.000.000.000), phương pháp brute-force có thể thực hiện gần hai tỷ phép tính modulo cho một số. Với tối đa 100 đầu vào, trường hợp xấu nhất sẽ xảy ra trong khoảng (2 \cdot 10^{11}) thao tác. 

Quan sát quan trọng là các ước số xuất hiện theo cặp. Nếu (a \times b = q), thì ít nhất một trong (a) và (b) lớn nhất là (\sqrt q). Khi tìm ước số, chúng ta chỉ cần kiểm tra các ứng cử viên đến căn bậc hai. Tìm một có nghĩa là số đó là hợp số và đạt đến điểm cuối có nghĩa là số đó là số nguyên tố. 

Phương pháp vũ lực hoạt động vì nó kiểm tra tất cả các yếu tố có thể, nhưng nó lặp lại nhiều lần kiểm tra không cần thiết. Quan sát căn bậc hai sẽ loại bỏ tất cả các ứng cử viên không thể là thành viên nhỏ hơn của cặp số chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q) mỗi số | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(q)) mỗi số | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng số một cách độc lập và xử lý trường hợp đặc biệt trong đó giá trị là 1. Vì 1 có chính xác một ước số dương nên ngay lập tức nó phải được phân loại là Không. 
2. Thử mọi ước số nguyên từ 2 đến căn bậc hai của số đó. Nếu bất kỳ ước số nào cũng chia đều thì số đó có cặp thừa số khác 1 và chính nó nên là Số tổng hợp. 
3. Nếu vòng lặp kết thúc mà không tìm thấy ước số, hãy phân loại số đó là Số nguyên tố. Mỗi cặp yếu tố có thể có sẽ chứa một yếu tố nhỏ hơn đã được thử nghiệm. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến rằng sau khi kiểm tra mọi ước số ứng viên cho đến thời điểm hiện tại, không có hệ số nhỏ hơn nào của số bị bỏ sót. Bất kỳ số tổng hợp nào cũng có một cặp thừa số. Phần tử nhỏ hơn của cặp đó không thể vượt quá căn bậc hai, do đó việc tìm kiếm luôn tìm ra ước số cho hợp số. Các số nguyên tố không có ước số như vậy nên chúng tồn tại trong quá trình tìm kiếm hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def classify(x):
    if x == 1:
        return "Neither"

    d = 2
    while d * d <= x:
        if x % d == 0:
            return "Composite"
        d += 1

    return "Prime"

def solve():
    n = int(input())
    ans = []

    for _ in range(n):
        x = int(input())
        ans.append(classify(x))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`classify`hàm cô lập quyết định toán học. Điều kiện đầu tiên loại bỏ số duy nhất không phải là số nguyên tố hay hợp số. 

Vòng lặp sử dụng`d * d <= x`thay vì tính căn bậc hai dấu phẩy động. Điều này tránh được các vấn đề về độ chính xác đối với số nguyên lớn và cũng bao gồm các hình vuông hoàn hảo một cách tự nhiên. 

Khi tìm thấy số chia, hàm sẽ trả về ngay lập tức vì không cần kiểm tra thêm nữa cũng có thể thay đổi câu trả lời. Nếu vòng lặp kết thúc, không có ước số nào đến căn bậc hai, điều này chứng tỏ tính nguyên tố. 

Số nguyên Python không bị tràn nên phép nhân trong điều kiện vòng lặp là an toàn. Giá trị tối đa chỉ là hai tỷ, thấp hơn nhiều so với giới hạn số nguyên của Python. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
4
9
1
2017
1000000007
```Việc thực hiện trông như thế này: 

| Số | Kiểm tra số chia hiện tại | Kết quả | 
| --- | --- | --- | 
| 9 | 2 thất bại, 3 chia 9 | Tổng hợp | 
| 1 | Trường hợp đặc biệt | Không | 
| 2017 | Kiểm tra các ước số lên tới sqrt(2017), không chia | Thủ tướng | 
| 1000000007 | Kiểm tra các ước số lên tới sqrt(1000000007), không chia | Thủ tướng | 

Trường hợp đầu tiên cho thấy sự kết thúc sớm khi tìm thấy số chia. Thứ hai xác nhận rằng 1 được xử lý riêng. 

Đối với đầu vào:```
3
2
49
25
```| Số | Kiểm tra số chia hiện tại | Kết quả | 
| --- | --- | --- | 
| 2 | Không cần ứng cử viên chia | Thủ tướng | 
| 49 | 2,3,4,5,6 thất bại, 7 chia | Tổng hợp | 
| 25 | 2,3,4 thất bại, 5 chia | Tổng hợp | 

Dấu vết này chứng tỏ tại sao ranh giới căn bậc hai phải bao gồm sự đẳng thức. Hình vuông hoàn hảo tiết lộ liệu việc triển khai có xử lý chính xác ước số cuối cùng có thể hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(q)) | Mỗi số trong n số chỉ được kiểm tra đến căn bậc hai của nó | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ bên cạnh danh sách đầu ra | 

Tìm kiếm căn bậc hai tối đa là khoảng 44.721 lần lặp cho giá trị đầu vào lớn nhất có thể. Chỉ với 100 số, con số này tương đương khoảng 4,5 triệu séc, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def classify(x):
    if x == 1:
        return "Neither"
    d = 2
    while d * d <= x:
        if x % d == 0:
            return "Composite"
        d += 1
    return "Prime"

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    n = int(input())
    out = []
    for _ in range(n):
        out.append(classify(int(input())))
    sys.stdin = old
    return "\n".join(out)

assert run("""4
9
1
2017
1000000007
""") == """Composite
Neither
Prime
Prime""", "sample"

assert run("""3
2
49
25
""") == """Prime
Composite
Composite""", "perfect squares"

assert run("""1
1
""") == "Neither", "smallest value"

assert run("""3
3
4
16
""") == """Prime
Composite
Composite""", "boundary cases"

assert run("""2
999983
1000000007
""") == """Prime
Prime""", "large primes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | Không | Xử lý đặc biệt số tự nhiên nhỏ nhất | 
| 49, 25 | Tổng hợp | Phát hiện số chia bình phương hoàn hảo | 
| 3, 4, 16 | Hỗn hợp | Giá trị biên nhỏ | 
| Số nguyên tố lớn | Thủ tướng | Hiệu quả và tìm kiếm ước số đầy đủ | 

## Vỏ cạnh 

Giá trị 1 được xử lý trước khi vòng chia số bắt đầu. Đối với đầu vào:```
1
1
```thuật toán ngay lập tức trả về Không. Nó không bao giờ đi vào logic kiểm tra số nguyên tố, ngăn ngừa lỗi phổ biến khi coi một số không có ước số nào được tìm thấy là số nguyên tố. 

Đối với các hình vuông hoàn hảo như:```
1
49
```vòng lặp tiếp tục trong khi`d * d <= x`, cho phép`d = 7`để được thử nghiệm. Số chia được tìm thấy và câu trả lời trở thành Tổng hợp. Sử dụng bất đẳng thức nghiêm ngặt sẽ bỏ qua trường hợp này. 

Đối với các giá trị lớn như:```
1
2000000000
```thuật toán không cố gắng quét mọi số nhỏ hơn. Nó chỉ kiểm tra các ứng viên đến căn bậc hai, nhanh chóng tìm ra ước số và giữ cho thời gian chạy có thể dự đoán được. 

Điều này cũng có thể được điều chỉnh thành định dạng biên tập theo phong cách Codeforces ngắn hơn nếu bạn muốn một phiên bản gần giống với những gì xuất hiện trong hướng dẫn chính thức.
