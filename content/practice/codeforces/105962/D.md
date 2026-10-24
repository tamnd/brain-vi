---
title: "CF 105962D - MA141"
description: "Chúng ta có bốn điểm trong mặt phẳng theo một thứ tự cố định A, B, C, D. Nhiệm vụ là xác định xem việc nối chúng theo thứ tự đó, kể cả cạnh từ D trở lại A, có tạo thành một hình vuông hay không. Đây không phải là bài toán “tập hợp bốn điểm” mà chúng ta có thể sắp xếp lại một cách tùy ý."
date: "2026-06-22T16:15:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105962
codeforces_index: "D"
codeforces_contest_name: "UNICAMP Freshman Contest 2025"
rating: 0
weight: 105962
solve_time_s: 60
verified: true
draft: false
---

[CF 105962D - MA141](https://codeforces.com/problemset/problem/105962/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn điểm trong mặt phẳng theo một thứ tự cố định A, B, C, D. Nhiệm vụ là xác định xem việc nối chúng theo thứ tự đó, kể cả cạnh từ D trở lại A, có tạo thành một hình vuông hay không. 

Đây không phải là bài toán “tập hợp bốn điểm” mà chúng ta có thể sắp xếp lại một cách tùy ý. Thứ tự quan trọng vì các cạnh rõ ràng là AB, BC, CD và DA. Vì vậy, chúng tôi đang kiểm tra xem liệu chu trình chính xác này có mô tả một hình vuông hay không. 

Mỗi điểm có tọa độ nguyên giới hạn trong một phạm vi vừa phải, vì vậy tất cả các phép tính hình học có thể được thực hiện một cách an toàn bằng cách sử dụng số nguyên. Vì tọa độ có độ lớn lên tới 10^4 nên khoảng cách bình phương vừa khít với số nguyên 64 bit và chúng ta không bao giờ cần số học dấu phẩy động. 

Một sai lầm ngây thơ là coi bốn điểm như một tập hợp không có thứ tự và cố gắng đoán một hình vuông bằng cách sắp xếp khoảng cách hoặc góc. Điều đó không thành công vì hình vuông hợp lệ phụ thuộc vào thứ tự tuần hoàn. Ví dụ, bốn điểm giống nhau có thể tạo thành hình vuông nếu được sắp xếp đúng thứ tự, nhưng trở thành hình tự giao nhau hoặc hình tứ giác không vuông nếu được hoán vị. 

Một cạm bẫy phổ biến khác là dựa vào hình học dấu phẩy động, đặc biệt là so sánh góc. Điều đó là không cần thiết và rủi ro do sai sót về độ chính xác. Mọi thứ có thể được biểu diễn chính xác bằng cách sử dụng tích số chấm và bình phương độ dài. 

Trường hợp cạnh tinh tế là khi các điểm tạo thành một hình thoi không phải là hình vuông. Tất cả các cạnh có thể bằng nhau, nhưng các góc không phải là 90 độ. Một cái khác là một hình chữ nhật không phải là hình vuông, trong đó các cạnh đối diện bằng nhau nhưng các cạnh liền kề khác nhau. Cả hai đều phải bị từ chối. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là xem xét tất cả các hoán vị của bốn điểm, diễn giải chúng như một chu trình và kiểm tra xem liệu có thứ tự nào tạo thành một hình vuông hay không. Đối với mỗi hoán vị, chúng ta sẽ tính cả bốn độ dài cạnh và góc. Vì có 4! = 24 hoán vị, đây là công việc không đổi cho mỗi trường hợp thử nghiệm và vẫn khả thi. 

Tuy nhiên, cách tiếp cận đó lãng phí về mặt khái niệm vì vấn đề đã khắc phục được thứ tự. Chúng tôi không được phép sắp xếp lại điểm; chúng ta phải xác nhận trực tiếp chu trình đã cho. 

Quan sát quan trọng là một tứ giác là hình vuông khi và chỉ khi có hai điều kiện thỏa mãn: cả bốn cạnh đều bằng nhau và ít nhất một góc là góc vuông. Theo thuật ngữ vectơ, nếu AB, BC, CD, DA là các cạnh thì chúng ta yêu cầu độ dài bình phương bằng nhau và độ trực giao giữa các cạnh liền kề. 

Điều này làm giảm toàn bộ vấn đề thành việc tính toán bốn vectơ, bốn chiều dài bình phương và một vài tích số chấm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu | O(1) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra hình học (tối ưu) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta làm việc trực tiếp với các vectơ hình thành bởi các điểm liên tiếp. 

1. Vẽ các vectơ 4 cạnh AB, BC, CD, DA. Mỗi vectơ được tính bằng cách trừ tọa độ, ví dụ AB = B − A. Điều này mã hóa hướng và độ dài ở dạng phù hợp với số học số nguyên. 
2. Tính bình phương độ dài của cả bốn cạnh. Độ dài bình phương tránh căn bậc hai và giữ mọi thứ chính xác, vì việc so sánh độ dài chỉ yêu cầu sự bằng nhau. 
3. Xác minh rằng tất cả bốn chiều dài bình phương đều bằng nhau. Nếu bất kỳ cạnh nào khác nhau thì hình đó không thể là hình vuông vì hình vuông có bốn cạnh bằng nhau. 
4. Chọn một đỉnh, ví dụ điểm B, và tính tích số chấm của các cạnh liền kề AB và BC. Tích số chấm bằng 0 tương đương với một góc 90 độ tại B. 
5. Kiểm tra xem AB và BC có vuông góc không. Nếu không, hình này không phải là hình vuông ngay cả khi tất cả các cạnh bằng nhau, vì nó sẽ chỉ là hình thoi. 
6. Nếu cả hai điều kiện đều giữ nguyên, xuất ra “SIM”, nếu không thì xuất ra “NAO”. 

Lý do đằng sau việc chỉ kiểm tra một góc là vì trong một tứ giác có tất cả các cạnh bằng nhau, việc có một góc vuông sẽ buộc tất cả các góc đều là góc vuông, điều này đặc trưng đầy đủ cho hình vuông. 

### Tại sao nó hoạt động

Thuật toán thực thi rằng tất cả các cạnh đều bằng nhau, điều này hạn chế hình dạng thành hình thoi. Trong hình thoi, tất cả các đỉnh đều đối xứng khi quay 180 độ và tất cả các góc đều bằng nhau. Nếu một góc bằng 90 độ thì tất cả đều phải bằng 90 độ, biến hình thoi thành hình vuông. Do đó, độ dài các cạnh bằng nhau cộng với một điều kiện kề cận trực giao duy nhất là đủ để đảm bảo cấu trúc hình vuông đầy đủ và thứ tự cố định đảm bảo chúng ta đang xác nhận đúng chu trình thay vì tìm kiếm một chu trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def sq_len(ax, ay, bx, by):
    dx = bx - ax
    dy = by - ay
    return dx * dx + dy * dy

def dot(ax, ay, bx, by, cx, cy):
    # AB · BC where A->B->C
    return (bx - ax) * (cx - bx) + (by - ay) * (cy - by)

def solve():
    pts = [tuple(map(int, input().split())) for _ in range(4)]
    A, B, C, D = pts

    ax, ay = A
    bx, by = B
    cx, cy = C
    dx, dy = D

    d1 = sq_len(ax, ay, bx, by)
    d2 = sq_len(bx, by, cx, cy)
    d3 = sq_len(cx, cy, dx, dy)
    d4 = sq_len(dx, dy, ax, ay)

    if not (d1 == d2 == d3 == d4):
        print("NAO")
        return

    if dot(ax, ay, bx, by, cx, cy) != 0:
        print("NAO")
        return

    print("SIM")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tải bốn điểm và tính khoảng cách bình phương cho mỗi cạnh liên tiếp theo thứ tự đã cho. Việc kiểm tra đẳng thức đảm bảo chúng ta đang xử lý một ứng cử viên hình thoi. Kiểm tra tích số chấm ở đỉnh B tạo thành một góc vuông giữa AB và BC. Chúng ta không cần kiểm tra rõ ràng các góc hoặc tính chất đường chéo khác vì chúng được ngụ ý bởi sự kết hợp của các cạnh bằng nhau và một góc trực giao trong một chu trình kín. 

Tất cả các tính toán chỉ là số nguyên, tránh hoàn toàn các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
0 1
1 1
1 0
```| Bước | AB² | BC² | CD² | DA² | AB·BC | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán các cạnh | 1 | 1 | 1 | 1 | -1 | tiếp tục | 
| Bình đẳng bên | bằng | bằng | bằng | bằng | - | vượt qua | 
| Kiểm tra góc phải | - | - | - | - | 0 | SIM | 

Đây là hình vuông căn chỉnh theo trục tiêu chuẩn. Tất cả các cạnh đều bằng nhau và góc ở B là 90 độ nên thuật toán chấp nhận điều đó. 

### Ví dụ 2 

đầu vào:```
0 0
0 1
1 2
1 1
```| Bước | AB² | BC² | CD² | DA² | AB·BC | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Tính toán các cạnh | 1 | 2 | 1 | 2 | -1 | thất bại | 
| Bình đẳng bên | không bằng | | | | | NAO | 

Ở đây, hình dạng giống như một tứ giác lệch. Ngay cả khi không kiểm tra các góc, độ dài các cạnh không bằng nhau sẽ ngay lập tức loại bỏ nó. 

Dấu vết cho thấy đẳng thức độ dài cạnh là bộ lọc chiếm ưu thế, ngăn chặn việc tính toán góc không cần thiết trong các trường hợp không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định trên bốn điểm | 
| Không gian | O(1) | Lưu trữ liên tục tọa độ và giá trị trung gian | 

Các ràng buộc đủ nhỏ để thậm chí một phương pháp hình học nặng hơn cũng có thể dễ dàng vượt qua, nhưng giải pháp này tối ưu cả về tính đơn giản lẫn chi phí thực hiện. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import isclose

    # re-run solution inline
    input = sys.stdin.readline

    def sq_len(ax, ay, bx, by):
        dx = bx - ax
        dy = by - ay
        return dx * dx + dy * dy

    def dot(ax, ay, bx, by, cx, cy):
        return (bx - ax) * (cx - bx) + (by - ay) * (cy - by)

    pts = [tuple(map(int, input().split())) for _ in range(4)]
    A, B, C, D = pts
    ax, ay = A
    bx, by = B
    cx, cy = C
    dx, dy = D

    d1 = sq_len(ax, ay, bx, by)
    d2 = sq_len(bx, by, cx, cy)
    d3 = sq_len(cx, cy, dx, dy)
    d4 = sq_len(dx, dy, ax, ay)

    if not (d1 == d2 == d3 == d4):
        return "NAO\n"

    if dot(ax, ay, bx, by, cx, cy) != 0:
        return "NAO\n"

    return "SIM\n"

# provided samples
assert run("0 0\n0 1\n1 1\n1 0\n") == "SIM\n"
assert run("0 0\n0 1\n1 2\n1 1\n") == "NAO\n"

# custom cases
assert run("1 0\n2 1\n1 2\n0 1\n") == "SIM\n", "rotated square"
assert run("0 0\n1 0\n2 1\n1 1\n") == "NAO\n", "rectangle not square"
assert run("0 0\n1 1\n2 2\n3 3\n") == "NAO\n", "collinear points"
assert run("0 0\n0 2\n2 2\n2 0\n") == "SIM\n", "axis aligned square"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình vuông xoay | SIM | hình vuông thẳng hàng không có trục | 
| hình chữ nhật không vuông | NAO | các cạnh liền kề không bằng nhau | 
| điểm thẳng hàng | NAO | từ chối thoái hóa | 
| trục vuông góc | SIM | cấu hình chuẩn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi hình vuông được xoay. Đối với đầu vào như (1,0), (2,1), (1,2), (0,1), thuật toán vẫn tính các bình phương có độ dài bằng nhau và tích số 0 ở điểm thứ hai nên trả về chính xác “SIM”. Điều này xác nhận rằng phương pháp này là bất biến xoay. 

Một trường hợp khác là khi các điểm tạo thành một hình chữ nhật không phải là hình vuông, chẳng hạn như (0,0), (1,0), (2,1), (1,1). Ở đây độ dài các cạnh khác nhau ngay lập tức nên thuật toán sẽ loại bỏ nó mà không cần kiểm tra góc. 

Trường hợp có vẻ suy biến là thứ tự cộng tuyến hoặc gần cộng tuyến như (0,0), (1,1), (2,2), (3,3). Mặc dù khoảng cách có thể thể hiện sự đối xứng một phần, nhưng ít nhất một bên so sánh không thành công hoặc tích số chấm khác 0, đảm bảo bị loại bỏ.
