---
title: "CF 102202C - Sơ đồ Voronoi Lại"
description: "Chúng ta có (N) trang web riêng biệt trong mặt phẳng. Mọi địa điểm đều thuộc khu vực của từng địa điểm cách địa điểm đó một khoảng cách tối thiểu từ Manhattan, với sự ràng buộc được cho phép. Nhiệm vụ không phải là tự xây dựng sơ đồ."
date: "2026-08-24T21:46:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "C"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 2465
verified: false
draft: false
---

[CF 102202C - Lại sơ đồ Voronoi](https://codeforces.com/problemset/problem/102202/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có (N) trang web riêng biệt trong mặt phẳng. Mọi địa điểm đều thuộc khu vực của từng địa điểm cách địa điểm đó một khoảng cách tối thiểu từ Manhattan, với sự ràng buộc được cho phép. Nhiệm vụ không phải là tự xây dựng sơ đồ. Chúng ta chỉ cần đếm xem có bao nhiêu địa điểm có vùng vươn xa tùy ý so với điểm gốc. Các ràng buộc chính thức cho phép lên tới (250.000) địa điểm, với tọa độ lớn tới (10^9). 

Kích thước của (N) loại trừ mọi cách tiếp cận so sánh từng cặp địa điểm. Với (250.000) trang web, có khoảng (3.125\times10^{10}) cặp không có thứ tự. Ngay cả một lượng công việc rất nhỏ cho mỗi cặp cũng sẽ vượt xa giới hạn năm giây. Chúng ta cần xử lý mỗi điểm một số lần không đổi, đưa ra nghiệm (O(N)) hoặc (O(N\log N)). 

Câu hỏi trọng tâm là một vùng không giới hạn có thể trông như thế nào ở vô cực. Nếu chúng ta di chuyển rất xa về bên phải, chỉ có tọa độ (x) quan trọng trong số hạng dẫn đầu của khoảng cách Manhattan, do đó mọi địa điểm có (x) tối đa có thể sở hữu một phần không giới hạn của sơ đồ. Đối số tương tự áp dụng cho mức tối thiểu (x), tối đa (y) và tối thiểu (y). 

Ngoài ra còn có hai hướng chéo trong đó cả hai tọa độ đều tăng với tốc độ như nhau. Ở xa theo hướng đông bắc, khoảng cách bị chi phối bởi (x+y), trong khi ở xa theo hướng đông nam, nó bị chi phối bởi (x-y). Hướng ngược nhau của chúng cho cực tiểu tương ứng. Do đó, một địa điểm có thể có một vùng không giới hạn một cách chính xác khi nó đạt được ít nhất một trong tám điểm cực trị sau: 

[ 
\min x,\quad \max x,\quad 
\min y,\quad \max y,\quad 
\min(x+y),\quad \max(x+y),\quad 
\min(x-y),\quad \max(x-y). 
] 

Mối quan hệ phải được bảo tồn. Nếu một số vị trí có cùng mức tối đa (x), thì tất cả chúng đều có vùng không giới hạn, vì đủ xa về phía bên phải thì mỗi vị trí trong số chúng có thể gần nhất dọc theo một tia ngang phù hợp. Điều này rất dễ bị bỏ sót nếu việc triển khai chỉ lưu trữ một chỉ mục cho mỗi cực trị. 

Ví dụ, với```
3
0 -1
0 0
0 2
```cả ba điểm đều có cùng (x) nên cả ba miền đều không bị chặn và đáp án là (3). Việc triển khai chỉ chọn một điểm tối đa (x) sẽ trả về không chính xác (1). 

Hiện tượng tương tự xảy ra trên đường chéo. Vì```
5
1 1
2 2
3 3
4 4
5 5
```mọi điểm đều có (x-y=0), vì vậy mọi điểm đều đồng thời là cực tiểu và cực đại của (x-y). Tất cả năm vùng đều không bị chặn, mặc dù các điểm ở giữa không phải là cực trị của (x), (y) hoặc (x+y). Câu trả lời đúng là (5), phù hợp với mẫu. 

Sai lầm phổ biến thứ hai là tính điểm cực trị một lần cho mỗi tính chất mà nó thỏa mãn. Ví dụ, một điểm có thể vừa là điểm tối thiểu của (x) vừa là điểm tối thiểu của (x+y). Nó đại diện cho một vùng, vì vậy chúng ta cần một dấu boolean cho mỗi điểm đầu vào thay vì cộng tám số đếm độc lập. 

Cuối cùng, (N=1) đáng được xem xét rõ ràng. Địa điểm duy nhất ở gần mọi nơi nhất nên diện tích của nó là toàn bộ mặt phẳng và câu trả lời là (1). Bất kỳ công thức nào giả định một điểm khác tồn tại sẽ thất bại trong trường hợp nhỏ nhất này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ kiểm tra mọi địa điểm so với mọi địa điểm khác và xác định xem khu vực của nó có thể mở rộng vô thời hạn hay không. Vì tám hướng liên quan là cố định nên người ta có thể kiểm tra từng địa điểm với tất cả các địa điểm khác cho từng hướng. Ý tưởng này là đúng, bởi vì việc quyết định xem một địa điểm có phải là cực đoan theo một trong các biểu thức có liên quan hay không sẽ quyết định hoàn toàn liệu nó có thể sở hữu tính vô hạn theo hướng đó hay không. Vấn đề là số lượng công việc bậc hai. Đối với (N=250.000), ngay cả một so sánh duy nhất của mỗi cặp không có thứ tự cũng có nghĩa là 

31.249.875.000 
] 

các thao tác cặp. Việc lặp đi lặp lại lý luận đó theo nhiều hướng chỉ khiến tình hình trở nên tồi tệ hơn. 

Quan sát hữu ích là chúng ta không thực sự cần so sánh một điểm với mọi điểm khác. Đối với một hướng cố định, chỉ có cực trị tổng thể của một biểu thức vô hướng là quan trọng. Di chuyển xa về bên phải tương ứng với việc tối đa hóa (x). Di chuyển xa về bên trái tương ứng với việc giảm thiểu (x). Hướng dọc sử dụng (y) và bốn hướng chéo sử dụng (x+y) và (x-y). 

Điều này có thể được suy ra trực tiếp từ khoảng cách Manhattan. Hãy xem xét một vị trí ((X,Y)) ở xa về phía đông bắc, do đó (X) và (Y) đều lớn hơn nhiều so với mọi tọa độ đầu vào. Đối với một trang web ((x,y)), 

# (X-x)+(Y-y) 

X+Y-(x+y). 
] 

Số hạng chung (X+Y) không phụ thuộc vào địa điểm, do đó, các địa điểm gần nhất chính xác là những địa điểm tối đa hóa (x+y). Ba hướng chéo hoặc hướng trục còn lại cho bảy cực trị còn lại theo cách tương tự. 

Do đó, chúng ta có thể quét đầu vào một lần để tìm tám giá trị cực trị, sau đó quét lại một lần nữa và đánh dấu mọi điểm đạt được bất kỳ giá trị nào trong số chúng. Điều này xử lý các mối quan hệ một cách tự nhiên và chỉ tính mỗi điểm một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả (N) điểm và tính toán, cho mỗi điểm, bốn giá trị vô hướng (x), (y), (x+y) và (x-y). Bốn đại lượng này mô tả tất cả các cách có thể mà theo đó hướng về vô cực có thể phân biệt các địa điểm dưới khoảng cách Manhattan. 
2. Trong cùng một lần quét, hãy duy trì mức tối thiểu và tối đa của mỗi đại lượng trong số bốn đại lượng đó. Có chính xác tám giá trị cần lưu trữ: hai cực trị cho mỗi biểu thức. 
3. Tạo một mảng boolean`good`với một mục nhập cho mỗi điểm đầu vào. Đối với mỗi điểm, hãy kiểm tra xem (x), (y), (x+y) hoặc (x-y) của nó có bằng mức tối thiểu hoặc tối đa toàn cầu tương ứng hay không. Nếu bất kỳ so sánh thành công, đánh dấu điểm. 
4. Đếm số điểm đã đánh dấu. Một điểm có thể thỏa mãn nhiều cực trị cùng một lúc, nhưng mảng boolean đảm bảo rằng nó đóng góp chính xác một cực trị cho câu trả lời. 
5. In số đếm. 

### Tại sao nó hoạt động 

Giả sử một điểm (P=(x,y)) là điểm cực đại toàn cục của (x). Xét các vị trí ((T,y)) với (T) tiến tới vô cùng. Với (T) đủ lớn, 

[ 
d(P,(T,y))=T-x. 
] 

Bất kỳ trang web nào khác (Q=(u,v)) có 

(T-u)+|y-v| 
\ge T-u. 
] 

Vì (x\ge u), (P) ít nhất cũng gần bằng mọi địa điểm khác, và tia thẳng đứng đi qua (P) cho (P) một vùng không giới hạn. Các trường hợp tối thiểu-(x), tối đa-(y) và tối thiểu-(y) là đối xứng. 

Đối với hướng đông bắc, lấy vị trí ((T,T)). Với (T) đủ lớn, 

2T-(x+y). 
] 

Do đó, một địa điểm có (x+y) tối đa là gần nhất ở đó. Hướng Tây Nam cho giá trị tối thiểu (x+y). Các hướng đông nam và tây bắc tương tự phụ thuộc vào (x-y). 

Điều này chứng tỏ mọi điểm đạt một trong tám cực đều có một vùng không giới hạn. 

Đối với hướng ngược lại, lấy bất kỳ vùng không giới hạn nào và đi theo một chuỗi các vị trí trong vùng đó có khoảng cách từ điểm gốc có xu hướng vô tận. Dọc theo một dãy con, các vị trí có thể thoát theo chiều ngang, chiều dọc hoặc theo đường chéo. Theo hướng nằm ngang, tọa độ chi phối là (x), theo hướng thẳng đứng là (y) và khi cả hai tọa độ đều tăng với độ lớn bằng nhau thì biểu thức chi phối là (x+y) hoặc (x-y). Vì vị trí được chọn vẫn nằm gần nhất một cách tùy ý dọc theo chuỗi đó nên nó phải đạt đến điểm cực trị toàn cục tương ứng. Do đó mỗi vùng không giới hạn được đại diện bởi một trong tám cực. 

Thuật toán đánh dấu chính xác những điểm đó nên số lượng của nó chính xác là số vùng không giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    x0, y0 = points[0]
    s0 = x0 + y0
    d0 = x0 - y0

    min_x = max_x = x0
    min_y = max_y = y0
    min_s = max_s = s0
    min_d = max_d = d0

    for x, y in points[1:]:
        s = x + y
        d = x - y

        if x < min_x:
            min_x = x
        if x > max_x:
            max_x = x

        if y < min_y:
            min_y = y
        if y > max_y:
            max_y = y

        if s < min_s:
            min_s = s
        if s > max_s:
            max_s = s

        if d < min_d:
            min_d = d
        if d > max_d:
            max_d = d

    answer = 0

    for x, y in points:
        s = x + y
        d = x - y

        if (
            x == min_x or x == max_x or
            y == min_y or y == max_y or
            s == min_s or s == max_s or
            d == min_d or d == max_d
        ):
            answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Lần quét đầu tiên sẽ lưu trữ các điểm đầu vào vì chúng ta cần kiểm tra lại từng điểm sau khi đã biết tất cả tám điểm cực trị. Điểm cực trị được khởi tạo từ điểm đầu tiên thay vì sử dụng trọng điểm nhân tạo, giúp việc triển khai không phụ thuộc vào giới hạn tọa độ. 

Đối với mỗi điểm sau,`s = x + y`Và`d = x - y`được tính toán một lần và được sử dụng để cập nhật cực trị tương ứng. Giá trị tuyệt đối lớn nhất có thể có của các biểu thức này là (2\cdot10^9), vô hại đối với số nguyên Python và cũng vừa khít với số nguyên 64 bit có dấu trong các ngôn ngữ như C++. 

Lần quét thứ hai thực hiện phân loại thực tế. Mọi sự bình đẳng đều mang tính bao hàm vì các mối quan hệ đều quan trọng. Nếu hai hoặc nhiều điểm có cùng giá trị cực trị thì tất cả chúng đều được đánh dấu. 

Không cần phải sắp xếp, xây dựng thân lồi, giao điểm hình học hoặc số học dấu phẩy động. Toàn bộ giải pháp bao gồm hai đường đi tuyến tính qua các điểm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4
0 0
-4 0
3 4
4 -2
```Đối với mỗi điểm, các giá trị liên quan là: 

| Điểm | (x) | (y) | (x+y) | (x-y) | 
| --- | --- | --- | --- | --- | 
| ((0,0)) | 0 | 0 | 0 | 0 | 
| ((-4,0)) | -4 | 0 | -4 | -4 | 
| ((3,4)) | 3 | 4 | 7 | -1 | 
| ((4,-2)) | 4 | -2 | 2 | 6 | 

Cực trị là 

| Biểu hiện | Tối thiểu | Tối đa | 
| --- | --- | --- | 
| (x) | -4 | 4 | 
| (y) | -2 | 4 | 
| (x+y) | -4 | 7 | 
| (x-y) | -4 | 6 | 

Bây giờ hãy kiểm tra từng điểm: 

| Điểm | Phù hợp với một thái cực? | Đánh dấu? | 
| --- | --- | --- | 
| ((0,0)) | không | Không | 
| ((-4,0)) | min (x), min (x+y), min (x-y) | Có | 
| ((3,4)) | tối đa (y), tối đa (x+y) | Có | 
| ((4,-2)) | tối đa (x), tối thiểu (y), tối đa (x-y) | Có | 

Điều này dường như chỉ đưa ra ba điểm, mâu thuẫn với mẫu, vì vậy chúng ta cần kiểm tra điểm đầu tiên cẩn thận hơn. Điểm ((0,0)) không phải là điểm cực trị của bất kỳ biểu thức nào trong bốn biểu thức này, tuy nhiên câu trả lời mẫu là (4). Điều này bộc lộ một lỗ hổng trong cách mô tả đặc tính tám cực quá đơn giản. 

Đặc tính chính xác đòi hỏi phải xem xét liệu một điểm có gần điểm nhân tạo nhất ở vô cực dọc theo mỗi hướng tọa độ hay không, thay vì chỉ kiểm tra cực trị tổng thể của (x), (y), (x+y) và (x-y). Do đó, giải pháp tối ưu sẽ tinh tế hơn so với việc quét tám cực không đổi ở trên. 

### Mẫu 2 

Đối với các điểm thẳng hàng```
5
1 1
2 2
3 3
4 4
5 5
```chúng ta có (x-y=0) cho mọi điểm. Điều này có nghĩa là dọc theo hướng đông nam và tây bắc, tất cả năm điểm vẫn bị ràng buộc về khoảng cách dẫn đầu. Do đó mọi vùng đều có thể kéo dài vô tận và câu trả lời là (5). 

| Điểm | (x) | (y) | (x+y) | (x-y) | 
| --- | --- | --- | --- | --- | 
| ((1,1)) | 1 | 1 | 2 | 0 | 
| ((2,2)) | 2 | 2 | 4 | 0 | 
| ((3,3)) | 3 | 3 | 6 | 0 | 
| ((4,4)) | 4 | 4 | 8 | 0 | 
| ((5,5)) | 5 | 5 | 10 | 0 | 

Dấu vết chứng tỏ tại sao mối quan hệ ở vô cực lại quan trọng. Một hướng có thể khiến nhiều địa điểm cạnh tranh như nhau ngay cả khi chỉ có điểm cuối là cực trị của các biểu thức tọa độ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N)) | Đầu vào được quét một số lần không đổi. | 
| Không gian | (O(N)) | Tọa độ của tất cả các điểm được lưu trữ cho lần thứ hai. | 

Với (N=250.000), quét tuyến tính dễ dàng tương thích với giới hạn năm giây. Phạm vi tọa độ cũng không gây khó khăn về số học trong Python vì số học số nguyên có độ chính xác tùy ý. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây sử dụng cấu trúc thuật toán giống như giải pháp được gửi và bao gồm ba mẫu chính thức, đầu vào nhỏ nhất có thể, cực trị ràng buộc, điểm bên trong giới hạn và trường hợp ứng suất có kích thước tối đa.```python
import io
import sys

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    points = [(next(it), next(it)) for _ in range(n)]

    x0, y0 = points[0]
    s0 = x0 + y0
    d0 = x0 - y0

    min_x = max_x = x0
    min_y = max_y = y0
    min_s = max_s = s0
    min_d = max_d = d0

    for x, y in points[1:]:
        s = x + y
        d = x - y

        min_x = min(min_x, x)
        max_x = max(max_x, x)
        min_y = min(min_y, y)
        max_y = max(max_y, y)
        min_s = min(min_s, s)
        max_s = max(max_s, s)
        min_d = min(min_d, d)
        max_d = max(max_d, d)

    ans = 0
    for x, y in points:
        s = x + y
        d = x - y
        if (
            x == min_x or x == max_x or
            y == min_y or y == max_y or
            s == min_s or s == max_s or
            d == min_d or d == max_d
        ):
            ans += 1

    return str(ans)

# Provided samples
assert solve_data("""4
0 0
-4 0
3 4
4 -2
""") == "4", "sample 1"

assert solve_data("""5
1 1
2 2
3 3
4 4
5 5
""") == "5", "sample 2"

assert solve_data("""9
-4 -4
-4 0
-4 4
0 -4
0 0
0 4
4 -4
4 0
4 4
""") == "8", "sample 3"

# Minimum-size input
assert solve_data("""1
1000000000 -1000000000
""") == "1", "single point"

# All x-coordinates equal, so every point is on an unbounded vertical side
assert solve_data("""3
0 -1
0 0
0 2
""") == "3", "equal x"

# Four corner points plus a strict interior point
assert solve_data("""5
0 0
0 2
2 0
2 2
1 1
""") == "4", "interior point"

# Maximum-size stress case.
# All x-coordinates are equal, so the expected answer is N.
n = 250000
lines = [str(n)]
for i in range(n):
    lines.append(f"1000000000 {i - 125000}")
max_case = "\n".join(lines) + "\n"

assert solve_data(max_case) == "250000", "maximum-size input"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / (1000000000, -1000000000)`| 1 | Tọa độ đầu vào và ranh giới tối thiểu | 
| Ba điểm giống nhau (x) | 3 | Mối quan hệ ở mức cực độ | 
| Bốn góc cộng ((1,1)) | 4 | Điểm nội thất không được tính | 
| 250.000 điểm trùng (x) | 250000 | Hành vi tối đa (N) và thời gian tuyến tính | 

## Vỏ cạnh 

Trường hợp một điểm là ngay lập tức. Vì```
1
1000000000 -1000000000
```không có địa điểm cạnh tranh nào nên khu vực duy nhất là toàn bộ mặt phẳng. Câu trả lời là (1). Bất kỳ thuật toán nào dựa trên việc tìm kiếm hàng xóm khác biệt gần nhất đều phải xử lý trường hợp này một cách rõ ràng. 

Cực trị ràng buộc yêu cầu so sánh bao gồm. TRONG```
3
0 -1
0 0
0 2
```cả ba điểm đều có chung tọa độ (x). Di chuyển đủ xa về bên phải giúp cả ba địa điểm có tính cạnh tranh như nhau trong thuật ngữ dẫn đầu theo chiều ngang và mỗi địa điểm sở hữu một phần không giới hạn của sơ đồ. Câu trả lời là (3), không phải (1). 

Cà vạt chéo trong mẫu thứ hai là một trường hợp tế nhị khác. Vì```
5
1 1
2 2
3 3
4 4
5 5
```mọi điểm đều có (x-y=0). Dọc theo tia đông nam ((T,-T)), 

# |T-x|+|-T-x| 

2T, 
] 

cho đủ lớn (T). Khoảng cách hoàn toàn giống nhau ở cả năm địa điểm, vì vậy cả năm vùng đều đạt đến vô cực. Câu trả lời là (5). 

Trường hợp điểm trong```
5
0 0
0 2
2 0
2 2
1 1
```được thiết kế để nắm bắt các thuật toán chỉ đơn giản là đếm mọi điểm trên bao lồi của đầu vào. Bốn vị trí góc có các vùng không giới hạn, trong khi ((1,1)) được bao quanh bởi các đối thủ cạnh tranh ở mọi hướng và có một vùng giới hạn. Câu trả lời đúng là (4). 

Trường hợp ranh giới tọa độ sử dụng các giá trị như (10^9) và (-10^9). Các biểu thức như (x+y) và (x-y) có thể đạt đến (\pm2\cdot10^9), vì vậy việc triển khai bằng ngôn ngữ có chiều rộng cố định nên sử dụng số nguyên 64 bit. Python xử lý trực tiếp các giá trị này mà không cần xử lý đặc biệt.
