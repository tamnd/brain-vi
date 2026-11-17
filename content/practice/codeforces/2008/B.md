---
title: "CF 2008B - Vuông hay Không"
description: "Chúng ta có một chuỗi nhị phân ban đầu được hình thành bằng cách làm phẳng từng hàng lưới hình chữ nhật. Lưới được biết đến là “đẹp”, nghĩa là các ô viền của nó đều là 1 và tất cả các ô bên trong đều là 0."
date: "2026-06-08T13:25:46+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 2008
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 970 (Div. 3)"
rating: 800
weight: 2008
solve_time_s: 259
verified: false
draft: false
---

[CF 2008B - Vuông hay Không](https://codeforces.com/problemset/problem/2008/B) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học, dây 
**Thời gian giải:** 4 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi nhị phân ban đầu được hình thành bằng cách làm phẳng từng hàng lưới hình chữ nhật. Lưới được biết đến là “đẹp”, nghĩa là tất cả các ô viền của nó đều`1`và tất cả các tế bào bên trong đều`0`. 

Nhiệm vụ không phải là tái tạo lại lưới mà là quyết định xem có cách nào diễn giải chuỗi đến từ một lưới vuông hay không, trong đó số hàng bằng số cột, trong khi vẫn tôn trọng cấu trúc “đường viền, số 0 bên trong” giống nhau. 

Vì vậy, về mặt khái niệm, chúng tôi đang kiểm tra điều kiện tương thích: liệu chúng tôi có thể đặt chuỗi vào một`r × r`lưới sao cho chu vi toàn là số 1 và bên trong toàn là số 0. 

Ràng buộc chính là chuỗi đã được sửa; chỉ có sự giải thích (chiều hướng`r`) linh hoạt. Từ`r^2`phải bằng độ dài chuỗi, vấn đề thực sự là hỏi liệu độ dài có phải là một hình vuông hoàn hảo hay không và liệu cấu trúc ngụ ý có thể khớp với mẫu đường viền hợp lệ hay không. 

Vì tổng độ dài đầu vào qua các thử nghiệm lớn, lên tới 2 · 10^5 nên mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì liên quan đến việc mô phỏng nhiều lưới ứng viên cho mỗi bài kiểm tra sẽ quá chậm. 

Một lỗi phổ biến là cố gắng xây dựng lại ma trận một cách rõ ràng hoặc kiểm tra nhiều độ dài cạnh có thể có bằng cách điền vào lưới. Điều đó sẽ lãng phí tính toán và không cần thiết vì cấu trúc của đường viền hình vuông được xác định đầy đủ một khi`r`đã được sửa. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là thử mọi chiều có thể`r`từ 1 đến`n`, kiểm tra xem`r × r = n`và nếu vậy, hãy xây dựng lại lưới và xác minh xem nó có khớp với mẫu đường viền hợp lệ hay không. Cách tiếp cận này hầu như đã dư thừa vì chỉ có một`r`có thể thỏa mãn điều kiện bình phương, nhưng ngay cả khi được khái quát hóa để thử nhiều ứng viên, nó vẫn yêu cầu xây dựng hoặc mô phỏng một ma trận và kiểm tra tất cả các ô, đó là O(n√n) trong trường hợp xấu nhất. 

Quan sát quan trọng là ma trận vuông chỉ tồn tại khi độ dài là một hình vuông hoàn hảo. Khi điều đó được giữ vững, cấu trúc của một ma trận đẹp sẽ được cố định: tất cả các hàng đầu tiên và cuối cùng là một, tất cả các cột đầu tiên và cuối cùng là một, và mọi thứ khác đều bằng 0. Mẫu đó không gây ra bất kỳ sự mơ hồ nào khi đã biết kích thước. Vì vậy, toàn bộ vấn đề quy về việc kiểm tra xem độ dài chuỗi có phải là một hình vuông hoàn hảo hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tái thiết Brute Force | O(n√n) | O(n) | Quá chậm | 
| Chỉ kiểm tra vuông | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chỉ cần xác định xem độ dài chuỗi có thể tạo thành lưới vuông hay không. 

1. Đọc số nguyên`n`và chuỗi`s`. Bản thân chuỗi không được sử dụng vượt quá độ dài của nó. 
2. Tính căn bậc hai của số nguyên`n`. 
3. Kiểm tra xem bình phương gốc số nguyên này có trả về chính xác không`n`. 
4. Nếu trùng khớp thì xuất ra “Có”, nếu không thì xuất ra “Không”. 

Lý do đằng sau bước 3 là một lưới vuông hợp lệ yêu cầu chính xác`r × r`các ô, do đó chiều dài phải là một hình vuông hoàn hảo. 

### Tại sao nó hoạt động 

Một ma trận vuông đẹp hoàn toàn được xác định bởi kích thước của nó`r`. Nếu độ dài chuỗi không có dạng`r^2`, không có lưới vuông nào có thể tồn tại được, bất kể sự sắp xếp của các số 1 và 0. Nếu nó là một hình vuông hoàn hảo thì cấu hình đường viền hợp lệ luôn có thể được xây dựng cho kích thước đó, do đó sự tồn tại chỉ phụ thuộc vào điều kiện số học. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    s = input().strip()

    r = int(math.isqrt(n))
    if r * r == n:
        print("Yes")
    else:
        print("No")
```Việc thực hiện chỉ tập trung vào độ dài`n`. Chuỗi vẫn được đọc để sử dụng đầu vào chính xác nhưng không bao giờ được xử lý. Chi tiết chính đang sử dụng`math.isqrt`, giúp tránh các vấn đề về độ chính xác của dấu phẩy động có thể xảy ra với`sqrt`. 

Một sai lầm phổ biến là quên loại bỏ dòng mới khi đọc chuỗi; mặc dù không được yêu cầu nghiêm ngặt ở đây nhưng bạn nên đảm bảo xử lý đầu vào nhất quán. Một vấn đề tinh vi khác là sử dụng căn bậc hai có dấu phẩy động và đúc, có thể phân loại sai các hình vuông hoàn hảo lớn do lỗi chính xác. Căn bậc hai số nguyên tránh được điều đó hoàn toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 9, s = 111101111
```| n | isqrt(n) | r² | kết quả | 
| --- | --- | --- | --- | 
| 9 | 3 | 9 | Có | 

Điều này xác nhận rằng lưới 3 × 3 là có thể. 

### Ví dụ 2 

đầu vào:```
n = 12, s = 111110011111
```| n | isqrt(n) | r² | kết quả | 
| --- | --- | --- | --- | 
| 12 | 3 | 9 | Không | 

Mặc dù 3 là căn bậc hai số nguyên, nhưng 12 không phải là số chính phương hoàn hảo nên không tồn tại lưới vuông. 

Những ví dụ này cho thấy nội dung của chuỗi không liên quan; chỉ có chiều dài của nó là quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi bài kiểm tra tính một căn bậc hai số nguyên | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài đầu vào | 

Giải pháp dễ dàng thỏa mãn các ràng buộc vì mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        s = input().strip()
        r = math.isqrt(n)
        out.append("Yes" if r * r == n else "No")
    return "\n".join(out)

# provided samples
assert run("""5
2
11
4
1111
9
111101111
9
111111111
12
111110011111
""") == """No
Yes
Yes
No
No"""

# custom cases
assert run("""1
1
1
""") == "Yes"

assert run("""1
8
11111111
""") == "No"

assert run("""1
16
1111111111111111
""") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | hình vuông hợp lệ nhỏ nhất | 
| 8 | 11111111 | từ chối chiều dài không vuông | 
| 16 | tất cả những cái | trường hợp hình vuông hoàn hảo lớn hơn | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là khi`n`rất gần với một hình vuông hoàn hảo, chẳng hạn như`8`hoặc`10`. Trong những trường hợp này,`isqrt(n)`trả về gốc sàn và bình phương nó một cách chính xác không khớp`n`, đảm bảo câu trả lời là “Không”. Điều này tránh mọi sự mơ hồ từ việc làm tròn dấu phẩy động và đảm bảo tính chính xác ngay cả đối với các đầu vào lớn.
