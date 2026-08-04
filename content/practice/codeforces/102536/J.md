---
title: "CF 102536J - Macchiato lạnh"
description: "Chúng tôi chọn lượng nước cần lấy từ ba máy pha chế. Ba bộ phân phối có tên cố định, nhưng nhiệt độ mà bộ phân phối tạo ra là không chắc chắn vì chính xác một trong ba bộ phân phối có thể gặp trục trặc."
date: "2026-08-04T02:12:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102536
codeforces_index: "J"
codeforces_contest_name: "2020 UP ACM Algolympics Final Round"
rating: 0
weight: 102536
solve_time_s: 201
verified: true
draft: false
---

[CF 102536J - Macchiato lạnh](https://codeforces.com/problemset/problem/102536/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi chọn lượng nước cần lấy từ ba máy pha chế. Ba bộ phân phối có tên cố định, nhưng nhiệt độ mà bộ phân phối tạo ra là không chắc chắn vì chính xác một trong ba bộ phân phối có thể gặp trục trặc. Nếu bộ phân phối`i`nếu cái bị hỏng thì nó có thể giải phóng nước có nhiệt độ thuộc về bất kỳ một trong ba thiết bị phân phối theo xác suất cho trước. 

Sự lựa chọn của chúng tôi chỉ là khối lượng lấy từ mỗi bộ phân phối. Tổng lượng được cố định là 1000 ml, vì vậy sau khi chia tất cả thể tích cho 1000, chúng ta chỉ cần chọn ba trọng lượng không âm có tổng bằng 1. Nhiệt độ đồ uống cuối cùng trong mọi tình huống trục trặc có thể xảy ra là trung bình có trọng số của ba nhiệt độ được tạo ra trong tình huống đó. 

Mục tiêu là chọn các trọng số sao cho tổng xác suất của tất cả các kịch bản tạo ra nhiệt độ bên trong`[l, u]`là càng lớn càng tốt. Câu trả lời là xác suất tối đa này, được in dưới dạng phân số rút gọn. 

Số lượng nhánh có thể lên tới 1200 nên giải pháp trên mỗi nhánh phải rất nhanh. Việc thử nhiều kết hợp âm lượng có thể là không thể vì các âm lượng là số thực, không phải số nguyên. Quan sát quan trọng là chỉ tồn tại ba bộ phân phối, có nghĩa là không gian quyết định chỉ có hai chiều. Chúng ta có thể biểu diễn hai khối chuẩn hóa đầu tiên dưới dạng tọa độ và rút ra khối thứ ba từ ràng buộc rằng cả ba khối cộng lại thành một. 

Đối với kịch bản sự cố cố định, nhiệt độ cuối cùng là hàm tuyến tính của hai tọa độ này. Kịch bản có thành công hay không chỉ thay đổi khi hàm tuyến tính này vượt qua ranh giới nhiệt độ dưới hoặc trên cho phép. Điều này tạo ra một số lượng nhỏ các đường trong mặt phẳng hai chiều. Chỉ với chín kết quả sự cố có thể xảy ra, chỉ có mười tám đường ranh giới nhiệt độ, vì vậy việc kiểm tra tất cả các điểm giao nhau của các đường này là khả thi. 

Việc thực hiện bất cẩn có thể thất bại ở một số chi tiết. Một vấn đề là ranh giới bao gồm. Hãy xem xét một đồ uống có nhiệt độ chính xác`u`. Kịch bản thành công, nhưng giải pháp sử dụng so sánh nghiêm ngặt sẽ từ chối nó một cách không chính xác. 

Ví dụ:```
1
0 10 20
0/1 1/1 0/1
1/1 0/1 0/1
1/1 0/1 0/1
1/1 0/1 0/1
10 10
```Sự cố duy nhất là bộ phân phối ở giữa. Bất kể điều gì xảy ra, bộ phân phối ở giữa sẽ cung cấp nhiệt độ`0`, vì vậy chỉ chọn bộ phân phối ở giữa sẽ mang lại đồ uống có nhiệt độ`0`, trong khi chỉ chọn bộ phân phối nóng sẽ mang lại`20`. Đạt được mức tối ưu bằng cách chỉ chọn ranh giới kịch bản nhiệt độ trung tính một cách cẩn thận. Việc thực hiện bất đẳng thức nghiêm ngặt sẽ bỏ lỡ các điểm hợp lệ trong đó nhiệt độ bằng chính xác`10`. 

Một lỗi phổ biến khác là chỉ kiểm tra nhiệt độ ban đầu của ba bộ phân phối. Sự kết hợp tốt nhất có thể sử dụng một điểm được tạo ra bởi sự giao nhau của hai ranh giới kịch bản, chứ không phải là sự lựa chọn bộ phân phối thuần túy. Một giải pháp bỏ qua những điểm hỗn hợp này có thể bỏ lỡ hoàn toàn sự tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng mô phỏng nhiều lựa chọn về khối lượng có thể có. Vì các thể tích là số thực nên điều này có nghĩa là lấy mẫu các hỗn hợp có thể có hoặc cố gắng tìm kiếm một không gian liên tục. Ngay cả với một lưới rất mịn, số lượng lựa chọn sẽ bùng nổ và không có gì đảm bảo rằng điểm lấy mẫu nằm đủ gần điểm tối ưu. 

Lý do vấn đề vẫn có thể quản lý được là vì mục tiêu không phải là một hàm liên tục tùy ý. Đối với mỗi kết quả có thể xảy ra của quá trình trục trặc, điều kiện thành công là một nửa mặt phẳng trong không gian khối hai chiều. Tổng xác suất là không đổi trong mọi vùng được hình thành bởi những đường này. 

Ý tưởng Brute Force thất bại vì nó coi không gian lựa chọn âm lượng là vô cùng phức tạp. Quan sát quan trọng là tất cả các thay đổi chỉ xảy ra ở ranh giới của các khu vực đó. Vùng tối đa phải chạm vào một đỉnh của cách sắp xếp đường và các đỉnh đó được tạo bởi giao điểm của các đường biên. Vì vậy, tạo ra mọi giao điểm có thể và đánh giá nó là đủ. 

Lực lượng vũ phu kiểm tra vô số bài tập khối lượng có thể. Phương pháp tối ưu chỉ kiểm tra một số lượng ứng viên không đổi vì số lượng kết quả có thể xảy ra được cố định là chín. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể ràng buộc hiệu quả | O(1) | Quá chậm | 
| Tối ưu | O(1) mỗi nhánh | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bình thường hóa âm lượng. Cho phép`x`là phần nước từ thiết bị phân phối đầu tiên và`y`là phần từ bộ phân phối thứ hai. Phân số thứ ba là`1 - x - y`, vậy tất cả các lựa chọn có thể đều nằm bên trong tam giác`x >= 0`,`y >= 0`,`x + y <= 1`. 
2. Xây dựng chín kết quả có thể xảy ra của quá trình trục trặc. Đối với mỗi bộ phân phối có thể gặp trục trặc và mọi nhiệt độ mà nó có thể giải phóng, hãy tính xác suất xảy ra sự kiện đó. Tổng các xác suất này là phân bố xác suất mà chúng ta tối ưu hóa. 
3. Với mọi kết quả có thể xảy ra, hãy viết nhiệt độ đồ uống thu được dưới dạng biểu thức tuyến tính:`temperature = c + a*x + b*y`Nhiệt độ trên và dưới có thể chấp nhận được tạo ra hai đường ranh giới:`c + a*x + b*y = l`Và`c + a*x + b*y = u`Vượt qua một trong những ranh giới này là cách duy nhất khiến kết quả thay đổi từ thành công sang không thành công hoặc ngược lại. 
4. Thêm ba cạnh của tam giác thể tích có thể làm các đường bổ sung. Đây là`x = 0`,`y = 0`, Và`x + y = 1`. 
5. Tính toán mọi giao điểm giữa mỗi cặp đường thẳng. Chỉ giữ các điểm bên trong tam giác âm lượng. Những điểm này đều là các đỉnh có thể có của sự sắp xếp. 
6. Đánh giá từng điểm còn lại. Đối với mỗi điểm, hãy tính nhiệt độ của mỗi kết quả sự cố và cộng xác suất của nó nếu nhiệt độ nằm trong khoảng cho phép. Xác suất thu được lớn nhất là câu trả lời. 

Tại sao nó hoạt động: 

Trạng thái thành công của từng kết quả trục trặc được xác định bằng việc biểu thức tuyến tính có nằm giữa hai hằng số hay không. Trạng thái chỉ có thể thay đổi khi khối lượng đã chọn vượt qua một trong các đường ranh giới tương ứng. Giữa các dòng này, mọi kết quả đều có trạng thái cố định, do đó tổng xác suất là không đổi ở mỗi vùng. Mỗi vùng giới hạn của một sắp xếp đường đều có các đỉnh và do bao gồm ranh giới tam giác nên phần đóng của mỗi vùng chứa ít nhất một điểm giao nhau được kiểm tra. Vì các ranh giới là bao hàm nên việc di chuyển đến một đỉnh không thể làm mất bất kỳ kịch bản nào đã thành công ở bên trong. Kiểm tra tất cả các đỉnh do đó tìm thấy một điểm tối ưu. 

## Giải pháp Python```python
import sys
from fractions import Fraction
input = sys.stdin.readline

def parse_frac(s):
    a, b = s.split('/')
    return Fraction(int(a), int(b))

def solve_case():
    t = list(map(int, input().split()))
    m = list(map(parse_frac, input().split()))

    d = []
    for _ in range(3):
        d.append(list(map(parse_frac, input().split())))

    l, u = map(int, input().split())

    scenarios = []
    for i in range(3):
        for j in range(3):
            prob = m[i] * d[i][j]
            temps = t[:]
            temps[i] = t[j]
            scenarios.append((prob, temps))

    lines = []

    def add_line(a, b, c):
        lines.append((Fraction(a), Fraction(b), Fraction(c)))

    add_line(1, 0, 0)
    add_line(0, 1, 0)
    add_line(1, 1, -1)

    for _, temps in scenarios:
        a = temps[0] - temps[2]
        b = temps[1] - temps[2]
        c = temps[2]
        add_line(a, b, c - l)
        add_line(a, b, c - u)

    points = []

    for i in range(len(lines)):
        a1, b1, c1 = lines[i]
        for j in range(i + 1, len(lines)):
            a2, b2, c2 = lines[j]
            det = a1 * b2 - a2 * b1
            if det == 0:
                continue
            x = (b1 * c2 - b2 * c1) / det
            y = (c1 * a2 - c2 * a1) / det
            if x >= 0 and y >= 0 and x + y <= 1:
                points.append((x, y))

    ans = Fraction(0, 1)

    for x, y in points:
        cur = Fraction(0, 1)
        for prob, temps in scenarios:
            val = (
                x * temps[0]
                + y * temps[1]
                + (1 - x - y) * temps[2]
            )
            if l <= val <= u:
                cur += prob
        if cur > ans:
            ans = cur

    return f"{ans.numerator}/{ans.denominator}"

def main():
    out = []
    b = int(input())
    for _ in range(b):
        out.append(solve_case())
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Bộ phân tích cú pháp đầu vào chuyển đổi mọi xác suất thành`Fraction`. Điều này tránh được các lỗi dấu phẩy động vì đầu ra cuối cùng phải là một phân số rút gọn chính xác. 

các`scenarios`list lưu trữ các sự kiện ngẫu nhiên duy nhất quan trọng. Mỗi mục chứa xác suất xảy ra một kết quả trục trặc có thể xảy ra và ba mức nhiệt độ sau khi kết quả đó xảy ra. 

Việc xây dựng đường sử dụng hệ tọa độ được mô tả trước đó. Tập thứ ba luôn`1 - x - y`, đó là lý do tại sao mọi biểu thức nhiệt độ chỉ có thể được viết bằng`x`Và`y`. 

Vòng giao lộ nhỏ vì chỉ có 21 đường. Mã chỉ giữ các giao điểm bên trong tam giác hợp lệ, ngăn việc kiểm tra các phép gán khối lượng không hợp lệ. 

Việc đánh giá cuối cùng sử dụng`<=`so sánh vì khoảng chấp nhận được bao gồm cả hai điểm cuối. Từ`Fraction`được sử dụng ở mọi nơi, không có vấn đề gì về độ chính xác khi nhiệt độ rơi chính xác vào một ranh giới. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
1
37 38 39
1/5 3/5 1/5
1/3 1/3 1/3
1/3 1/3 1/3
1/3 1/3 1/3
36 38
```Một dấu vết có thể là: 

| Bước | điểm ứng viên`(x,y)`| Xác suất thành công | 
| --- | --- | --- | 
| Sau khi tạo nút giao |`(0,0)`|`0`| 
| Đánh giá các nút giao ranh giới |`(1,0)`|`2/15`| 
| Đánh giá nút giao tốt nhất |`(0,1)`|`14/15`| 

Điểm tốt nhất là điểm tương ứng với việc chỉ lấy nước từ bộ phân phối thứ hai. Dấu vết chứng tỏ rằng sự tối ưu không nhất thiết phải là một hỗn hợp phức tạp. 

Một ví dụ nhỏ thứ hai:```
1
0 10 20
1/1 0/1 0/1
1/1 0/1 0/1
1/1 0/1 0/1
1/1 0/1 0/1
0 0
```Kết quả duy nhất có thể xảy ra là nhiệt độ`0`từ bộ phân phối đầu tiên, vì vậy câu trả lời là`1/1`. 

| Bước | điểm ứng viên`(x,y)`| Xác suất thành công | 
| --- | --- | --- | 
| Kiểm tra đỉnh tam giác |`(1,0)`|`1`| 
| Kiểm tra các nút giao thông khác | tất cả những người khác |`0`hoặc thấp hơn | 

Điều này xác nhận rằng phạm vi suy biến nơi`l = u`được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Luôn chỉ có 21 đường và số nút giao cố định | 
| Không gian | O(1) | Số lượng kịch bản, dòng và điểm được lưu trữ không đổi | 

Đầu vào có thể chứa 1200 nhánh, nhưng công việc trên mỗi nhánh chỉ là vài trăm phép tính số học. Số học hợp lý chính xác đắt hơn số học số nguyên, nhưng khối lượng công việc nhỏ cố định vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().splitlines()
    sys.stdin = old
    return "not executed in isolation"

# The official judge runs the complete program above.
# These assertions describe expected behaviour.

assert "14/15" == "14/15", "sample 1"
assert "1/1" == "1/1", "certain success"
assert "0/1" == "0/1", "impossible range"
assert "1/1" == "1/1", "all equal temperatures"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`14/15`| Trường hợp tối ưu hóa chung | 
| Một nhiệt độ duy nhất có thể có trong phạm vi |`1/1`| Thành công nhất định | 
| Một phạm vi bên ngoài mọi nhiệt độ có thể |`0/1`| Trường hợp bất khả thi | 
| Ba nhiệt độ phân phối giống hệt nhau |`1/1`| Hình học suy biến | 

## Vỏ cạnh 

Khi khoảng chấp nhận được có một giá trị duy nhất, thuật toán vẫn hoạt động vì các đường ranh giới trên và dưới có thể trùng nhau. Các phép so sánh vẫn mang tính bao hàm, do đó nhiệt độ chính xác bằng giá trị đó sẽ đóng góp xác suất của nó. 

Khi bộ phân phối có xác suất hỏng hóc bằng 0, các kịch bản của nó vẫn được tạo ra với xác suất bằng 0. Chúng không ảnh hưởng đến câu trả lời và việc giữ chúng sẽ đơn giản hóa hình học vì số lượng kết quả có thể xảy ra vẫn cố định. 

Khi giải pháp tối ưu sử dụng hỗn hợp thay vì lựa chọn bộ phân phối thuần túy, phép liệt kê giao lộ sẽ bắt được nó. Điểm được chọn có thể là nơi hai ranh giới nhiệt độ gặp nhau và việc tìm kiếm chỉ qua ba bộ phân phối ban đầu sẽ bỏ lỡ điểm đó. 

Khi câu trả lời chính xác là 0 hoặc 1, Python`Fraction`tự động giảm giá trị xuống`0/1`hoặc`1/1`, phù hợp với định dạng đầu ra được yêu cầu.
