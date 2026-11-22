---
title: "CF 39D - Hành tinh khối"
description: "Chúng ta có hai đỉnh của một khối lập phương đơn vị. Mỗi đỉnh được mô tả bằng ba tọa độ và mỗi tọa độ là 0 hoặc 1. Vì mỗi tọa độ chỉ có thể nhận hai giá trị nên các tọa độ này đại diện cho tám góc của khối lập phương. Hai con ruồi đứng ở hai đỉnh khác nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 39
codeforces_index: "D"
codeforces_contest_name: "School Team Contest 1 (Winter Computer School 2010/11)"
rating: 1100
weight: 39
solve_time_s: 87
verified: true
draft: false
---

[CF 39D - Hành tinh khối](https://codeforces.com/problemset/problem/39/D) 

**Đánh giá:** 1100 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai đỉnh của một khối lập phương đơn vị. Mỗi đỉnh được mô tả bằng ba tọa độ và mỗi tọa độ đều là`0`hoặc`1`. Vì mỗi tọa độ chỉ có thể nhận hai giá trị nên các tọa độ này đại diện cho tám góc của khối lập phương. 

Hai con ruồi đứng ở hai đỉnh khác nhau. Chúng có thể nhìn thấy nhau nếu cả hai đỉnh đều thuộc ít nhất một mặt chung của hình lập phương. Nhiệm vụ là in`"YES"`khi khuôn mặt như vậy tồn tại, nếu không thì in`"NO"`. 

Kích thước đầu vào rất nhỏ. Chúng tôi chỉ đọc tổng cộng sáu số nguyên, vì vậy hiệu suất không liên quan ở đây. Ngay cả một phép kiểm tra hình học mạnh mẽ trên tất cả các mặt khối cũng sẽ chạy ngay lập tức. Thách thức là nhận ra thuộc tính hình học ẩn trong tọa độ. 

Quan sát quan trọng là cách các mặt khối được biểu diễn trong tọa độ. Mỗi mặt cố định chính xác một tọa độ. Ví dụ như mặt`x = 0`chứa tất cả các đỉnh có tọa độ đầu tiên là`0`. Hai đỉnh nằm trên cùng một mặt nếu chúng có chung ít nhất một giá trị tọa độ ở cùng một vị trí. 

Có một vài trường hợp dễ bỏ sót. 

Xét các đỉnh đối diện:```
0 0 0
1 1 1
```Câu trả lời đúng là`"NO"`. Các đỉnh này không có chung bất kỳ mặt nào vì mọi tọa độ đều khác nhau. Một cách tiếp cận bất cẩn chỉ kiểm tra khoảng cách Euclide hoặc kết nối biên có thể thất bại ở đây. 

Bây giờ hãy xem xét các đỉnh chỉ được kết nối theo đường chéo trên một mặt:```
0 0 0
0 1 1
```Câu trả lời đúng là`"YES"`. Các đỉnh này không kề nhau bởi một cạnh nhưng vẫn thuộc mặt`x = 0`. Một giải pháp sai có thể cho rằng chỉ các đỉnh lân cận mới có thể nhìn thấy nhau. 

Một trường hợp tinh tế khác là khi có chính xác một tọa độ khớp:```
1 0 1
1 1 0
```Câu trả lời vẫn là`"YES"`vì cả hai điểm đều thuộc mặt`x = 1`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ mô hình hóa rõ ràng sáu mặt của khối lập phương. Mỗi mặt có bốn đỉnh. Chúng ta có thể tạo ra tất cả các đỉnh của khối lập phương, nhóm chúng theo các mặt, sau đó kiểm tra xem cả hai đỉnh đã cho có xuất hiện cùng nhau trong bất kỳ mặt nào hay không. 

Cách tiếp cận đó đúng vì định nghĩa về khả năng hiển thị chính xác là “chia sẻ một khuôn mặt”. Khối lập phương chỉ có sáu mặt, vì vậy ngay cả việc mô phỏng theo nghĩa đen cũng sẽ mất thời gian không đổi. 

Hình học trở nên đơn giản hơn nhiều khi chúng ta mô tả các khuôn mặt bằng tọa độ. Mỗi mặt khối cố định một tọa độ:```
x = 0
x = 1
y = 0
y = 1
z = 0
z = 1
```Hai đỉnh nằm trên cùng một mặt một cách chính xác khi có ít nhất một tọa độ trùng khớp ở cùng một vị trí. Nếu tọa độ đầu tiên bằng nhau thì cả hai đỉnh đều thuộc một trong hai`x = 0`hoặc`x = 1`. Logic tương tự áp dụng cho`y`Và`z`. 

Điều này loại bỏ tất cả hình học khỏi quá trình triển khai. Chúng ta chỉ cần so sánh tọa độ theo cặp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ đỉnh thứ nhất vào`(x1, y1, z1)`. 
2. Đọc tọa độ đỉnh thứ 2 vào`(x2, y2, z2)`. 
3. So sánh tọa độ tương ứng. 

Nếu như`x1 == x2`, cả hai đỉnh đều nằm trên cùng một`x`khuôn mặt. 

Nếu như`y1 == y2`, cả hai đỉnh đều nằm trên cùng một`y`khuôn mặt. 

Nếu như`z1 == z2`, cả hai đỉnh đều nằm trên cùng một`z`khuôn mặt. 
4. Nếu có ít nhất một so sánh khớp, hãy in`"YES"`. 
5. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Mỗi mặt của khối lập phương được xác định bằng cách cố định một tọa độ. Ví dụ: tất cả các đỉnh trên mặt`z = 1`có tọa độ thứ ba bằng`1`. 

Nếu hai đỉnh có chung bất kỳ giá trị tọa độ nào ở cùng một vị trí thì cả hai đỉnh đó đều thuộc về mặt tương ứng. Ngược lại, nếu cả ba tọa độ khác nhau thì một đỉnh nằm ở góc đối diện của khối lập phương với đỉnh kia và không có mặt nào chứa cả hai điểm. 

Thuật toán kiểm tra chính xác điều kiện này nên vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x1, y1, z1 = map(int, input().split())
x2, y2, z2 = map(int, input().split())

if x1 == x2 or y1 == y2 or z1 == z2:
    print("YES")
else:
    print("NO")
```Chương trình đọc hai bộ ba tọa độ và so sánh từng tọa độ một cách độc lập. 

Điều kiện sử dụng logic`or`bởi vì việc chia sẻ dù chỉ một tọa độ cũng đủ để đặt cả hai đỉnh trên một mặt chung. 

Một lỗi phổ biến là yêu cầu hai tọa độ trùng nhau. Điều đó sẽ chỉ phát hiện các đỉnh được kết nối bởi một cạnh và sẽ loại bỏ không chính xác các đường chéo của mặt như`(0,0,0)`Và`(0,1,1)`. 

Một lỗi khác có thể xảy ra là kiểm tra vùng lân cận theo khoảng cách Manhattan. Khả năng hiển thị rộng hơn mức kề cận, do đó cách tiếp cận đó bỏ lỡ các trường hợp hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 0
0 1 0
```| x1 | y1 | z1 | x2 | y2 | z2 | Tọa độ trùng khớp | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 1 | 0 | x và z | CÓ | 

Các đỉnh chia sẻ cả hai`x = 0`Và`z = 0`, nên rõ ràng chúng nằm trên các mặt chung. 

### Ví dụ 2 

đầu vào:```
0 0 0
1 1 1
```| x1 | y1 | z1 | x2 | y2 | z2 | Tọa độ trùng khớp | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | 1 | 1 | không | KHÔNG | 

Tất cả các tọa độ đều khác nhau. Đây là các góc lập phương đối diện nhau nên không có mặt nào chứa cả hai đỉnh. 

Ví dụ này xác nhận bất biến khóa: ít nhất một tọa độ phải khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba phép so sánh tọa độ được thực hiện | 
| Không gian | O(1) | Thuật toán lưu trữ một số số nguyên cố định | 

Cả thời gian chạy và mức sử dụng bộ nhớ đều không đổi, thấp hơn nhiều so với giới hạn của sự cố. Chương trình kết thúc ngay lập tức ngay cả trên môi trường chậm nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    x1, y1, z1 = map(int, input().split())
    x2, y2, z2 = map(int, input().split())

    if x1 == x2 or y1 == y2 or z1 == z2:
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("0 0 0\n0 1 0\n") == "YES\n", "sample 1"

# opposite corners
assert run("0 0 0\n1 1 1\n") == "NO\n", "opposite vertices"

# face diagonal
assert run("0 0 0\n0 1 1\n") == "YES\n", "same face but not adjacent"

# exactly one coordinate equal
assert run("1 0 1\n1 1 0\n") == "YES\n", "single matching coordinate"

# another opposite pair
assert run("1 0 0\n0 1 1\n") == "NO\n", "all coordinates different"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 0 / 1 1 1`|`NO`| Góc khối đối diện | 
|`0 0 0 / 0 1 1`|`YES`| Khả năng hiển thị đường chéo của khuôn mặt | 
|`1 0 1 / 1 1 0`|`YES`| Chính xác một tọa độ khớp | 
|`1 0 0 / 0 1 1`|`NO`| Không có khuôn mặt chung | 

## Vỏ cạnh 

Xét các đỉnh đối diện:```
0 0 0
1 1 1
```Thuật toán so sánh tọa độ từng cái một:```
0 == 1 → false
0 == 1 → false
0 == 1 → false
```Không có tọa độ trùng khớp nên chương trình sẽ in`"NO"`. Điều này xác định chính xác rằng các góc đối diện không bao giờ có chung một khuôn mặt. 

Bây giờ hãy xem xét một đường chéo của khuôn mặt:```
0 0 0
0 1 1
```Tọa độ đầu tiên khớp:```
0 == 0 → true
```Thuật toán biết ngay cả hai đỉnh đều thuộc mặt`x = 0`, vì vậy nó in`"YES"`. 

Cuối cùng, hãy xem xét trường hợp chỉ có một tọa độ chung:```
1 0 1
1 1 0
```Kết quả so sánh là:```
1 == 1 → true
0 == 1 → false
1 == 0 → false
```Ít nhất một tọa độ trùng khớp, đủ để hiển thị. Đầu ra là`"YES"`. 

Những trường hợp này cho thấy tại sao điều kiện “ít nhất một tọa độ bằng nhau” khớp chính xác với hình dạng của các mặt khối.
