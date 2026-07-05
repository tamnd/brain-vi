---
title: "CF 102896K - Lễ kỷ niệm năm 2021 của Kate"
description: "Chúng tôi được một cửa hàng tặng một số gói bóng bay. Mỗi gói hàng có giá và nhiều chữ số được viết trên bong bóng của nó."
date: "2026-07-04T11:30:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102896
codeforces_index: "K"
codeforces_contest_name: "Northern Eurasia Finals Online 2020"
rating: 0
weight: 102896
solve_time_s: 37
verified: true
draft: false
---

[CF 102896K - Lễ kỷ niệm năm 2021 của Kate](https://codeforces.com/problemset/problem/102896/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được một cửa hàng tặng một số gói bóng bay. Mỗi gói hàng có giá và nhiều chữ số được viết trên bong bóng của nó. Kate muốn mua chính xác một gói sao cho, bằng cách sử dụng các chữ số bên trong gói đó, cô ấy có thể tạo thành năm “2021”, nghĩa là cô ấy cần ít nhất hai số 0, hai số một và một hai, một hai, một một đã được tính, vì vậy tổng yêu cầu giảm xuống còn hai bản sao của chữ số 2, hai bản sao của chữ số 0 và một bản sao của chữ số 1 là không đủ vì mục tiêu cụ thể là “2 0 2 1”, vì vậy yêu cầu chính xác là số chữ số: 2 xuất hiện hai lần, 0 xuất hiện một lần và 1 xuất hiện một lần. 

Mỗi gói có thể được sử dụng như một túi chữ số và chúng ta có thể tự do chọn các chữ số từ gói đó miễn là chúng tồn tại. Nhiệm vụ là tìm chỉ số gói rẻ nhất thỏa mãn các yêu cầu về chữ số này hoặc báo cáo rằng không có gói nào có thể đáp ứng được. 

Kích thước đầu vào đủ nhỏ để có thể kiểm tra từng gói một cách độc lập. Với tối đa khoảng một nghìn gói hàng và mỗi chuỗi chữ số có độ dài lên tới một trăm, ngay cả việc quét tần số đơn giản cho mỗi gói cũng đủ nhanh. Bất kỳ cách tiếp cận nào tệ hơn khoảng 10^8 thao tác nguyên thủy sẽ vẫn an toàn, nhưng chúng tôi còn kém xa mức đó. 

Các trường hợp nghiêm trọng nhất xuất phát từ sự hiểu lầm đa dạng. Một sai lầm phổ biến là cho rằng cần phải xem “2021” dưới dạng chuỗi con nhưng các chữ số có thể được sắp xếp lại tùy ý. Một vấn đề tế nhị khác là quên rằng nhiều số 0 hoặc số 1 có thể xuất hiện và chỉ được tính là quan trọng. Cuối cùng, một gói hàng có thể chứa tất cả các chữ số bắt buộc nhưng số lượng không đủ, ví dụ chỉ có một số ‘2’. 

Ví dụ về một ý tưởng ngây thơ thất bại: nếu chúng ta chỉ kiểm tra xem chuỗi có chứa các ký tự '2', '0', '1' ít nhất một lần hay không, chúng ta sẽ chấp nhận sai một gói như “201” thiếu ký tự ‘2’ thứ hai. 

Một trường hợp khác là khi có nhiều gói hàng thỏa mãn điều kiện và chúng ta phải chọn giá tối thiểu chứ không phải giá hợp lệ đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực gần như đã là tối ưu. Đối với mỗi gói hàng, chúng tôi đếm số lần xuất hiện của các chữ số từ 0 đến 9 và xác minh xem số lượng có đáp ứng yêu cầu để tạo thành “2021” hay không. Điều này đúng vì vị trí chữ số không quan trọng, chỉ có tần số. 

Chi phí cưỡng bức rất đơn giản: đối với mỗi gói trong số n gói, chúng tôi quét một chuỗi có độ dài lên tới 100, do đó tổng công việc là khoảng 100.000 lần kiểm tra ký tự. Điều này là không đáng kể. 

Không có cấu trúc tổ hợp sâu hơn nào để khai thác vì mỗi gói là độc lập và quá trình kiểm tra diễn ra liên tục khi tần số được tính toán. “Tối ưu hóa” duy nhất là dừng sớm nếu số lượng đã vượt quá ngưỡng yêu cầu, nhưng thậm chí điều đó cũng không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (đếm chữ số trên mỗi gói) | O(n · L) | O(1) | Đã chấp nhận | 
| Tối ưu (giống như vũ phu) | O(n · L) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng gói hàng và khởi tạo một biến để lưu trữ câu trả lời tốt nhất là “không tìm thấy” và giá tốt nhất là vô cùng. Điều này đảm bảo chúng tôi có thể so sánh tất cả các ứng cử viên một cách thống nhất. 
2. Đối với mỗi gói hàng, hãy đọc giá và chuỗi chữ số của nó, sau đó tính toán dãy tần số có kích thước 10 bằng cách quét chuỗi một lần. Bước này là cần thiết vì chúng ta cần sự đa dạng chính xác chứ không chỉ sự hiện diện. 
3. Kiểm tra xem gói hàng có chứa ít nhất hai chữ số ‘2’, ít nhất một chữ số ‘0’ và ít nhất một chữ số ‘1’ hay không. Điều này trực tiếp mã hóa liệu chúng ta có thể xây dựng “2021” từ nhiều tập hợp hay không. 
4. Nếu điều kiện được thỏa mãn và giá nhỏ hơn mức giá tốt nhất được thấy cho đến nay, hãy cập nhật câu trả lời tốt nhất cho chỉ mục gói này. Điều này duy trì mức hoạt động tối thiểu đối với các ứng cử viên hợp lệ. 
5. Sau khi xử lý tất cả các gói, xuất ra chỉ mục tốt nhất nếu nó tồn tại, nếu không thì xuất ra 0. 

### Tại sao nó hoạt động

Mỗi gói là độc lập, do đó, vấn đề giảm xuống mẫu lọc và thu nhỏ trên một tập hợp các ứng cử viên. Việc kiểm tra tần số vừa cần thiết vừa đủ vì mọi hoán vị chữ số đều được cho phép. Việc duy trì chỉ mục tối thiểu trong số các gói hợp lệ sẽ nắm bắt chính xác kết quả đầu ra được yêu cầu vì chúng tôi luôn so sánh với giải pháp hợp lệ được biết đến nhiều nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ok(cnt):
    return cnt[2] >= 2 and cnt[0] >= 1 and cnt[1] >= 1

n = int(input())
best_price = float('inf')
best_idx = 0

for i in range(1, n + 1):
    parts = input().split()
    p = int(parts[0])
    s = parts[1]

    cnt = [0] * 10
    for ch in s:
        cnt[ord(ch) - 48] += 1

    if ok(cnt):
        if p < best_price:
            best_price = p
            best_idx = i

print(best_idx)
```Giải pháp được cấu trúc xung quanh bảng tần số chữ số trực tiếp. Kiểm tra trợ giúp sẽ tách biệt điều kiện để hình thành “2021”, điều này tránh lặp lại logic bên trong vòng lặp. Việc theo dõi chỉ mục được thực hiện cẩn thận để tôn trọng việc lập chỉ mục dựa trên 1 từ báo cáo vấn đề. 

Một lỗi triển khai phổ biến là quên đặt lại mảng tần số cho mỗi trường hợp thử nghiệm hoặc trộn lẫn chuyển đổi ASCII. sử dụng`ord(ch) - 48`đảm bảo ánh xạ chính xác từ ký tự sang giá trị chữ số. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào có ba gói:```
3
150 2021
200 220011
100 012345
```Gói đầu tiên đã chứa chính xác các chữ số cần thiết. 

| Trọn gói | Giá | Số chữ số (0,1,2) | hợp lệ | Chỉ số tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 150 | (0:1, 1:1, 2:1) | Không | 0 | 
| 2 | 200 | (0:2, 1:2, 2:2) | Có | 2 | 
| 3 | 100 | (0:1, 1:1, 2:0) | Không | 2 | 

Dấu vết này cho thấy tính hợp lệ phụ thuộc chặt chẽ vào số lượng hơn là thứ tự. 

Bây giờ hãy xem xét ví dụ thứ hai:```
4
300 2220011
250 2020
400 221100
100 000211
```| Trọn gói | Giá | (0,1,2 lần đếm) | hợp lệ | Chỉ số tốt nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 300 | (2,2,3) | Có | 1 | 
| 2 | 250 | (2,0,1) | Không | 1 | 
| 3 | 400 | (1,2,2) | Có | 1 | 
| 4 | 100 | (2,1,1) | Không | 1 | 

Điều quan trọng ở đây là mặc dù gói 4 rẻ nhất nhưng nó không đáp ứng được yêu cầu vì thiếu đủ '2'. Gói 1 vẫn tối ưu trong số các gói hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · L) | Mỗi chuỗi gói được quét một lần để tạo tần số chữ số | 
| Không gian | O(1) | Chỉ sử dụng mảng có kích thước cố định gồm 10 chữ số | 

Với n ≤ 1000 và L ≤ 100, tổng số thao tác là khoảng 100.000, thấp hơn nhiều so với ngưỡng giới hạn thời gian đối với các ràng buộc Codeforce điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline()

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    best_price = float('inf')
    best_idx = 0

    for i in range(1, n + 1):
        p, s = input().split()
        p = int(p)

        cnt = [0] * 10
        for ch in s:
            cnt[ord(ch) - 48] += 1

        if cnt[2] >= 2 and cnt[0] >= 1 and cnt[1] >= 1:
            if p < best_price:
                best_price = p
                best_idx = i

    return str(best_idx)

# provided samples
assert solve("""4
100 9876543210
200 00112233445566778899
160 012345678924568
150 000000123456789
""") == "3"

assert solve("""5
100 0123456789
120 0022446688
200 00224466883456789
10 0
10 1
""") == "0"

# custom cases
assert solve("""1
10 2021
""") == "1", "minimum valid single package"

assert solve("""2
5 000211
3 22100
""") == "2", "cheapest valid chosen"

assert solve("""3
10 222
20 001
30 11
""") == "0", "no valid package"

assert solve("""3
10 2021
5 2021
7 2021
""") == "2", "tie-breaking by price"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hợp lệ duy nhất | 1 | độ chính xác đầu vào tối thiểu | 
| nhiều ứng viên | 2 | lựa chọn tối thiểu đúng | 
| tất cả đều không hợp lệ | 0 | xử lý sự cố | 
| giống nhau hợp lệ | 2 | hòa theo giá | 

## Vỏ cạnh 

Một gói như “202” không thành công ngay cả khi nó trông giống mục tiêu vì nó chỉ chứa một ‘1’. Thuật toán sẽ loại bỏ nó một cách chính xác vì việc kiểm tra tần suất thực thi nghiêm ngặt tất cả các số đếm được yêu cầu. 

Gói như “2220011” chứa đủ chữ số nhưng vượt quá. Thuật toán chấp nhận nó vì các chữ số dư thừa không ảnh hưởng đến tính khả thi, chỉ có ngưỡng tối thiểu mới quan trọng. 

Trường hợp có nhiều gói hợp lệ có giá giống nhau được xử lý một cách tự nhiên vì thuật toán giữ mức tối thiểu gặp phải đầu tiên và vấn đề cho phép bất kỳ chỉ mục hợp lệ nào có quan hệ.
