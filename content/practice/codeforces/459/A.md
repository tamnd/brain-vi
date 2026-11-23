---
title: "CF 459A - Pashmak và Vườn"
description: "Chúng ta được cho tọa độ của hai cây riêng biệt trên mặt phẳng Descartes và chúng ta biết rằng khu vườn tạo thành một hình vuông hoàn hảo thẳng hàng với các trục. Mỗi đỉnh của hình vuông có một cây nên có đúng 4 cây."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 459
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 261 (Div. 2)"
rating: 1200
weight: 459
solve_time_s: 63
verified: true
draft: false
---

[CF 459A - Pashmak và Garden](https://codeforces.com/problemset/problem/459/A) 

**Đánh giá:** 1200 
**Thẻ:** triển khai 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho tọa độ của hai cây riêng biệt trên mặt phẳng Descartes và chúng ta biết rằng khu vườn tạo thành một hình vuông hoàn hảo thẳng hàng với các trục. Mỗi đỉnh của hình vuông có một cây nên có đúng 4 cây. Nhiệm vụ là xác định tọa độ của hai cây còn lại. Nếu không có ô vuông nào thỏa mãn các điều kiện này thì chúng ta phải xuất ra -1. 

Đầu vào bao gồm bốn số nguyên biểu thị tọa độ x và y của hai cây đã biết. Mỗi tọa độ nằm trong khoảng từ -100 đến 100 và tọa độ đầu ra được phép lớn tới ±1000. Vì kích thước đầu vào là cố định (chỉ có hai điểm) nên chúng ta không cần lo lắng về hiệu quả ngoài các phép tính số học đơn giản. Bất kỳ giải pháp nào chỉ bao gồm một vài lần kiểm tra liên tục sẽ chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

Các trường hợp cạnh rất tinh tế. Nếu hai cây có cùng tọa độ x thì hình vuông thẳng đứng; nếu chúng có cùng tọa độ y thì nó nằm ngang. Nếu không có tọa độ nào trùng nhau thì các cây phải là các đỉnh đối diện theo đường chéo của hình vuông và khoảng cách dọc theo x phải bằng khoảng cách dọc theo y. Nếu điều kiện này không thành công thì không thể hình thành được hình vuông nào. Việc triển khai ngây thơ có thể bỏ qua những điều kiện này và cố gắng xây dựng những điểm không thể. 

Ví dụ, đầu vào`0 0 1 2`không thể tạo thành một hình vuông thẳng hàng với các trục vì khoảng cách ngang và dọc khác nhau, do đó kết quả đầu ra đúng là -1. Tương tự, nhập`0 0 0 1`đại diện cho một cạnh thẳng đứng của hình vuông và hai đỉnh còn lại phải có tọa độ x dịch chuyển một khoảng cách theo chiều dọc (1), mang lại`(1, 0)`Và`(1, 1)`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các hình vuông có thể có cho hai điểm đầu tiên và kiểm tra cấu hình nào tạo thành hình vuông hợp lệ có các cạnh song song với các trục. Điều này sẽ liên quan đến nhiều nhánh có điều kiện cho các vị trí ngang, dọc và chéo, đồng thời kiểm tra tất cả các hoán vị của hai điểm còn lại. Nó hoạt động vì hình vuông bị hạn chế rất nhiều, nhưng nó gây ra sự phức tạp không cần thiết. 

Cách tiếp cận tối ưu là phân loại mối quan hệ giữa hai điểm đã cho. Nếu chúng nằm trên cùng một đường thẳng đứng thì cạnh của hình vuông là thẳng đứng và hai đỉnh còn lại dịch chuyển theo chiều ngang một khoảng theo chiều dọc. Nếu chúng nằm trên cùng một đường ngang thì cạnh đó nằm ngang và các đỉnh còn lại dịch chuyển theo chiều dọc. Nếu chúng tạo thành một đường chéo của hình vuông thì khoảng cách theo chiều ngang và chiều dọc phải bằng nhau, hai đỉnh còn lại nằm ở`(x1, y2)`Và`(x2, y1)`. Nếu không có trường hợp nào trong số này đúng thì không có bình phương hợp lệ. 

Quan sát này làm giảm vấn đề xuống còn ba lần kiểm tra thời gian không đổi và số học đơn giản, đảm bảo thời gian và không gian O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Quá phức tạp, không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ`(x1, y1)`Và`(x2, y2)`. 
2. Kiểm tra xem hai điểm có nằm trên cùng một đường thẳng đứng không (`x1 == x2`). Nếu vậy, hãy tính khoảng cách theo phương ngang là`d = abs(y2 - y1)`. Các đỉnh còn lại là`(x1 + d, y1)`Và`(x2 + d, y2)`. In những thứ này và thoát. 
3. Nếu hai điểm nằm trên cùng một đường ngang (`y1 == y2`), tính khoảng cách theo chiều dọc`d = abs(x2 - x1)`. Các đỉnh khác là`(x1, y1 + d)`Và`(x2, y2 + d)`. In và thoát. 
4. Nếu khoảng cách tuyệt đối theo chiều ngang bằng khoảng cách tuyệt đối theo chiều dọc (`abs(x2 - x1) == abs(y2 - y1)`), các điểm tạo thành một đường chéo. Các đỉnh khác là`(x1, y2)`Và`(x2, y1)`. In và thoát. 
5. Nếu không có điều kiện nào ở trên xảy ra, hãy ghi -1, vì không thể tạo được bình phương hợp lệ. 

Tại sao nó hoạt động: Trong mỗi trường hợp, chúng tôi khai thác thuộc tính căn chỉnh theo trục của hình vuông. Các trường hợp cạnh dọc và ngang dựa vào việc dịch dọc theo trục vuông góc theo độ dài cạnh chính xác. Trường hợp đường chéo có tác dụng vì trong một hình vuông, hiệu tọa độ x bằng hiệu tọa độ y đối với các đỉnh đối diện theo đường chéo. Ba bước kiểm tra này bao gồm tất cả các vị trí có thể có của hai đỉnh trên một hình vuông thẳng hàng với trục, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

x1, y1, x2, y2 = map(int, input().split())

if x1 == x2:
    d = abs(y2 - y1)
    print(x1 + d, y1, x2 + d, y2)
elif y1 == y2:
    d = abs(x2 - x1)
    print(x1, y1 + d, x2, y2 + d)
elif abs(x2 - x1) == abs(y2 - y1):
    print(x1, y2, x2, y1)
else:
    print(-1)
```Mã trực tiếp thực hiện ba trường hợp được xác định trong thuật toán. Thứ tự kiểm tra rất quan trọng: đầu tiên chúng ta kiểm tra căn chỉnh theo chiều dọc, sau đó là chiều ngang và cuối cùng là đường chéo. các`abs`đảm bảo độ dài cạnh dương và thứ tự in khớp với yêu cầu của bài toán với tọa độ đầu ra theo bất kỳ thứ tự nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`0 0 0 1`| Biến | Giá trị | 
| --- | --- | 
| x1, y1 | 0, 0 | 
| x2, y2 | 0, 1 | 
| x1 == x2 | Đúng | 
| d | 1 | 
| Đầu ra | 1 0 1 1 | 

Điều này thể hiện trường hợp cạnh dọc. Thuật toán tính toán chính xác độ dịch chuyển theo chiều ngang và tìm ra hai đỉnh còn lại. 

### Ví dụ 2 

đầu vào:`0 0 1 1`| Biến | Giá trị | 
| --- | --- | 
| x1, y1 | 0, 0 | 
| x2, y2 | 1, 1 | 
| abs(x2 - x1) == abs(y2 - y1) | Đúng | 
| Đầu ra | 0 1 1 0 | 

Đây là một trường hợp đường chéo. Thuật toán hoán đổi tọa độ để tạo thành hai đỉnh còn lại dọc theo hình vuông. 

### Ví dụ 3 

đầu vào:`0 0 1 2`| Biến | Giá trị | 
| --- | --- | 
| x1, y1 | 0, 0 | 
| x2, y2 | 1, 2 | 
| Không có điều kiện giữ | Đúng | 
| Đầu ra | -1 | 

Điều này cho thấy một cấu hình không thể. Thuật toán phát hiện chính xác không có hình vuông hợp lệ nào tồn tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép tính số học và phép so sánh được thực hiện. | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ; không có mảng hoặc cấu trúc dữ liệu tỷ lệ thuận với đầu vào. | 

Với tọa độ tối đa là ±100 và một bước số học duy nhất, giải pháp nằm trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    x1, y1, x2, y2 = map(int, input().split())
    if x1 == x2:
        d = abs(y2 - y1)
        return f"{x1 + d} {y1} {x2 + d} {y2}"
    elif y1 == y2:
        d = abs(x2 - x1)
        return f"{x1} {y1 + d} {x2} {y2 + d}"
    elif abs(x2 - x1) == abs(y2 - y1):
        return f"{x1} {y2} {x2} {y1}"
    else:
        return "-1"

# Provided sample
assert run("0 0 0 1\n") == "1 0 1 1", "sample 1"

# Custom cases
assert run("0 0 1 1\n") == "0 1 1 0", "diagonal square"
assert run("0 0 1 2\n") == "-1", "impossible square"
assert run("2 3 2 5\n") == "4 3 4 5", "vertical side"
assert run("5 7 8 7\n") == "5 10 8 10", "horizontal side"
assert run("-100 -100 -100 -98\n") == "-98 -100 -98 -98", "negative coordinates vertical"
assert run("100 100 102 100\n") == "100 102 102 102", "positive coordinates horizontal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 1 | 1 0 1 1 | trường hợp bên dọc | 
| 0 0 1 1 | 0 1 1 0 | trường hợp chéo | 
| 0 0 1 2 | -1 | hình vuông không thể | 
| 2 3 2 5 | 4 3 4 5 | dọc với khoảng cách > 1 | 
| 5 7 8 7 | 5 10 8 10 | mặt ngang | 
| -100 -100 -100 -98 | -98 -100 -98 -98 | tọa độ âm | 
| 100 100 102 | | |
