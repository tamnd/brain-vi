---
title: "CF 903C - Đóng gói hộp"
description: "Chúng ta có n hộp hình khối, mỗi hộp có chiều dài cạnh được xác định bởi một mảng a. Mishka muốn lồng các hộp này vào nhau theo những quy tắc nghiêm ngặt: một hộp chỉ có thể chuyển vào một hộp khác nếu nó thực sự nhỏ hơn và hộp lớn hơn chưa chứa hộp khác."
date: "2026-06-12T22:54:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 903
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 34 (Rated for Div. 2)"
rating: 1200
weight: 903
solve_time_s: 315
verified: true
draft: false
---

[CF 903C - Đóng gói hộp](https://codeforces.com/problemset/problem/903/C) 

**Đánh giá:** 1200 
**Thẻ:** tham lam 
**Thời gian giải:** 5 phút 15 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có *n* hộp hình khối, mỗi hộp có chiều dài cạnh được chỉ định bởi một mảng *a*. Mishka muốn lồng các hộp này vào nhau theo những quy tắc nghiêm ngặt: một hộp chỉ có thể chuyển vào một hộp khác nếu nó thực sự nhỏ hơn và hộp lớn hơn chưa chứa hộp khác. Mục đích là giảm thiểu số lượng hộp có thể nhìn thấy, là những hộp không được chứa trong bất kỳ hộp nào khác. Về cơ bản, chúng tôi muốn tạo ra các chuỗi hộp lồng nhau trong đó độ dài của mỗi chuỗi càng dài càng tốt, sao cho càng nhiều hộp được ẩn bên trong các hộp khác càng tốt. 

Ràng buộc đầu vào cho phép *n* lên tới 5000. Điều này loại trừ các cách tiếp cận bạo lực thử tất cả các hoán vị hoặc tất cả các cách lồng theo cặp một cách đầy đủ, bởi vì các hoạt động *n!* hoặc thậm chí *n²* có thể đạt tới 25 triệu hoặc cao hơn, quá chậm trong Python đối với các đầu vào trong trường hợp xấu nhất nếu thuật toán không đơn giản và chặt chẽ. Kích thước mỗi hộp có thể lên tới 10⁹, nghĩa là chúng ta không thể dựa vào mảng tần số với các chỉ số tương ứng với kích thước, nhưng việc đếm các bản sao là khả thi. 

Các trường hợp phức tạp liên quan đến các hộp có kích thước bằng nhau. Ví dụ, hãy xem xét`3 3 3`. No box can go inside another because nesting requires strictly smaller size. Số lượng hộp hiển thị tối thiểu chính xác là 3. Một cách tiếp cận đơn giản bỏ qua các bản sao có thể cố gắng lồng chúng không chính xác và đếm thiếu các hộp hiển thị. Another subtle scenario is when one size occurs many times, e.g.,`1 2 2 2 3`. Chúng ta phải đảm bảo rằng không có nhiều hộp có cùng kích thước được lồng vào nhau hơn mức cho phép, tương đương với việc xử lý các hộp có cùng kích thước là độc lập và tính tần số tối đa của bất kỳ kích thước nào. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ cố gắng mô phỏng tất cả các cấu hình lồng nhau có thể có. Chúng ta có thể thử sắp xếp các hộp và sau đó cố gắng lặp đi lặp lại việc đặt từng hộp vào bất kỳ hộp lớn hơn nào hiện đang trống. Mặc dù điều này cuối cùng sẽ tạo ra một câu trả lời đúng, nhưng nó yêu cầu phải kiểm tra từng cặp nhiều lần, có khả năng là O(n2) và quản lý hộp nào có thể chứa các hộp khác, điều này trở nên lộn xộn và không hiệu quả. Đối với 5000 hộp, điều này có thể dẫn đến khoảng 25 triệu thao tác và gần bằng giới hạn thời gian trong Python, đặc biệt là với các thao tác danh sách thường xuyên. 

Quan sát chính là các hộp chỉ khác nhau về kích thước và các ràng buộc lồng nhau hoàn toàn được xác định bằng so sánh kích thước. Điều này làm giảm vấn đề đếm xem có bao nhiêu hộp ở mỗi kích cỡ. Mỗi hộp hiển thị ít nhất phải có tần suất bằng số lần xuất hiện tối đa của bất kỳ kích thước nào, vì chúng ta không thể lồng các hộp giống hệt nhau. Nếu một kích thước xuất hiện năm lần và kích thước lớn hơn xuất hiện ít lần hơn, chúng ta cần ít nhất năm hộp hiển thị để chứa tất cả chúng mà không vi phạm quy tắc lồng nhau nghiêm ngặt. Tần số sắp xếp và đếm cho phép chúng tôi xác định số lượng tối đa này một cách hiệu quả. Do đó, vấn đề giảm xuống còn tính toán tần số của từng kích thước hộp và lấy số lượng lớn nhất. Cái nhìn sâu sắc này biến một mô phỏng có khả năng lộn xộn thành một thuật toán O(n log n) rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng hộp, *n* và mảng kích thước hộp, *a*. Điều này thiết lập đầu vào để xử lý. 
2. Sắp xếp mảng *a*. Việc sắp xếp đảm bảo rằng tất cả các hộp có kích thước bằng nhau đều liền kề nhau và làm cho việc đếm tần số trở nên đơn giản. 
3. Khởi tạo một biến đếm để theo dõi tần số của kích thước hiện tại và một biến`max_count`để lưu trữ tần số lớn nhất được nhìn thấy. 
4. Lặp lại mảng đã được sắp xếp. Đối với mỗi hộp, hãy kiểm tra xem nó có giống với hộp trước không. Nếu có, hãy tăng bộ đếm. Nếu khác thì đặt lại bộ đếm về 1. 
5. Sau khi cập nhật bộ đếm cho từng ô, hãy cập nhật`max_count`nếu bộ đếm hiện tại vượt quá nó. Điều này đảm bảo rằng`max_count`luôn giữ tần số có kích thước phổ biến nhất. 
6. Sau khi xử lý tất cả các hộp, xuất ra`max_count`là số lượng hộp hiển thị tối thiểu. Điều này đúng vì tần số tối đa quy định số lượng hộp không thể lồng vào nhau do yêu cầu nghiêm ngặt về kích thước. 

Tại sao nó hoạt động: Điều bất biến là đối với bất kỳ kích thước nào xảy ra *k* lần, ít nhất các hộp *k* phải vẫn hiển thị vì không có hai hộp giống hệt nhau nào có thể lồng nhau. Việc sắp xếp đảm bảo rằng chúng ta nhìn thấy tất cả các hộp giống hệt nhau một cách liên tiếp và việc theo dõi tần số tối đa đảm bảo rằng đầu ra không bao giờ nhỏ hơn mức tối thiểu cần thiết. Không có cấu hình nào có thể giảm số lượng hiển thị xuống dưới tần số tối đa này, vì vậy giải pháp này là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()

max_count = 0
current_count = 0
prev = None

for size in a:
    if size == prev:
        current_count += 1
    else:
        current_count = 1
        prev = size
    if current_count > max_count:
        max_count = current_count

print(max_count)
```Giải pháp bắt đầu bằng cách sắp xếp các hộp sao cho các hộp có kích thước giống hệt nhau liền kề nhau. Chúng tôi khởi tạo`current_count`để đếm các hộp giống hệt nhau liên tiếp. Mỗi lần chúng tôi gặp một kích thước mới, chúng tôi đặt lại`current_count`đến 1.`max_count`theo dõi tần số lớn nhất. Việc sắp xếp rất quan trọng vì nó cho phép chúng ta quét mảng trong một lần mà không bỏ sót các bản sao. Việc quên đặt lại bộ đếm khi gặp kích thước mới sẽ dẫn đến việc đếm quá mức. Sử dụng một biến riêng biệt`prev`an toàn hơn việc dựa vào số học chỉ mục, điều này có thể gây ra từng lỗi một. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**```
3
1 2 3
```| kích thước | trước | hiện tại_count | max_count | 
|------|------|---------------|----------| 
| 1 | Không có | 1 | 1 | 
| 2 | 1 | 1 | 1 | 
| 3 | 2 | 1 | 1 | 

Tần số lớn nhất là 1 nên chỉ cần một hộp hiển thị. Các hộp có thể lồng hoàn toàn vào một chuỗi: 1 bên trong 2 bên trong 3. 

**Đầu vào mẫu 2**```
5
2 2 3 3 3
```| kích thước | trước | hiện tại_count | max_count | 
|------|------|---------------|----------| 
| 2 | Không có | 1 | 1 | 
| 2 | 2 | 2 | 2 | 
| 3 | 2 | 1 | 2 | 
| 3 | 3 | 2 | 2 | 
| 3 | 3 | 3 | 3 | 

Tần số tối đa là 3, do đó cần có ít nhất ba hộp nhìn thấy được. Chuỗi đầu tiên có thể lồng số 2 vào số 3, để lại ba hộp có kích thước 3 có thể nhìn thấy được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(n log n) | Việc sắp xếp chi phối độ phức tạp, lần quét tiếp theo là O(n) | 
| Không gian | O(n) | Lưu trữ mảng kích thước và một vài bộ đếm | 

Đối với n lên tới 5000, O(n log n) tương ứng với khoảng 65.000 thao tác, nằm trong giới hạn thời gian 1 giây. Việc sử dụng bộ nhớ ở mức tối thiểu, phù hợp thoải mái trong vòng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    max_count = 0
    current_count = 0
    prev = None

    for size in a:
        if size == prev:
            current_count += 1
        else:
            current_count = 1
            prev = size
        if current_count > max_count:
            max_count = current_count
    return str(max_count)

# provided samples
assert run("3\n1 2 3\n") == "1", "sample 1"
assert run("5\n2 2 3 3 3\n") == "3", "sample 2"

# custom cases
assert run("1\n7\n") == "1", "single box"
assert run("4\n5 5 5 5\n") == "4", "all equal boxes"
assert run("6\n1 2 2 1 5 3\n") == "2", "duplicates mixed"
assert run("5\n10 20 10 20 10\n") == "3", "high-low-high pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1\n7 | 1 | Hộp đựng cạnh hộp đơn | 
| 4\n5 5 5 5 | 4 | Tất cả các hộp đều bằng nhau, không thể lồng nhau | 
| 6\n1 2 2 1 5 3 | 2 | Các bản sao yêu cầu tần suất theo dõi | 
| 5\n10 20 10 20 10 | 3 | Đảm bảo xử lý tần số tối đa chính xác | 

## Vỏ cạnh 

Đối với đầu vào`3 3 3`, thuật toán sắp xếp mảng như`[3, 3, 3]`. Nó đếm tần số của 3 là 3. Vì không có hộp nào có thể lồng vào hộp khác có cùng kích thước nên cả ba hộp vẫn hiển thị. Kết quả đầu ra là chính xác 3. 

Đối với một đầu vào hộp duy nhất`1`, mảng`[1]`kết quả là`max_count = 1`, điều đó đúng. Đối với một chuỗi có kích thước xen kẽ, thuật toán sẽ đặt lại bộ đếm một cách chính xác ở mỗi lần thay đổi kích thước và theo dõi tần số tối đa,
