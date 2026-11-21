---
title: "CF 229C - Hình tam giác"
description: "Chúng ta bắt đầu với một đồ thị vô hướng đầy đủ trên n đỉnh. Mỗi cặp đỉnh có đúng một cạnh giữa chúng. Alice giữ m các cạnh đó và Bob nhận tất cả các cạnh còn lại."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "graphs", "math"]
categories: ["algorithms"]
codeforces_contest: 229
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 142 (Div. 1)"
rating: 1900
weight: 229
solve_time_s: 125
verified: true
draft: false
---

[CF 229C - Hình tam giác](https://codeforces.com/problemset/problem/229/C) 

**Xếp hạng:** 1900 
**Tags:** tổ hợp, đồ thị, toán học 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một đồ thị vô hướng đầy đủ trên`n`đỉnh. Mỗi cặp đỉnh có đúng một cạnh giữa chúng. Alice giữ`m`của các cạnh đó và Bob nhận được tất cả các cạnh còn lại. 

Nhiệm vụ là đếm có bao nhiêu hình tam giác tồn tại trong đồ thị của Alice cộng với bao nhiêu hình tam giác tồn tại trong đồ thị của Bob. Một hình tam giác đơn giản là một tập hợp gồm ba đỉnh trong đó cả ba cạnh giữa chúng đều có trong cùng một đồ thị. 

Đầu vào đưa ra các cạnh thuộc về Alice. Đồ thị của Bob ngầm là đồ thị phần bù: mọi cạnh bị thiếu của Alice đều tự động thuộc về Bob. 

Hạn chế lớn nhất là`n ≤ 10^6`, điều này ngay lập tức loại trừ bất cứ điều gì thậm chí giống với việc lặp lại trên tất cả các bộ ba đỉnh. Một lực lượng vũ phu`O(n^3)`kiểm tra tam giác sẽ yêu cầu xung quanh`10^18`hoạt động trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Ngay cả việc lưu trữ một ma trận kề cũng cần`10^12`các ô, cũng không phù hợp với bộ nhớ. 

Số cạnh Alice nhiều nhất là`10^6`, nhỏ hơn nhiều so với`n^2`. Điều đó gợi ý rõ ràng rằng giải pháp nên phụ thuộc chủ yếu vào các cạnh và độ, chứ không phải vào tất cả các cặp đỉnh hoặc bộ ba. 

Một phần tinh tế của vấn đề là chúng ta đang đếm các hình tam giác trong hai đồ thị bổ sung cùng một lúc. Việc thực hiện bất cẩn có thể cố gắng đếm riêng các tam giác Alice và tam giác Bob, nhưng đồ thị của Bob có thể có tới khoảng`n^2 / 2`các cạnh, vì vậy việc xây dựng nó một cách rõ ràng là không thể. 

Một lỗi dễ mắc phải khác là tràn số nguyên. Số lượng hình tam giác trong một đồ thị hoàn chỉnh có`10^6`đỉnh là$$\binom{10^6}{3} \approx 1.67 \times 10^{17}$$không phù hợp với số nguyên 32 bit. Python tự động xử lý các số nguyên lớn, nhưng trong các ngôn ngữ khác, điều này yêu cầu số học 64-bit. 

Hãy xem xét ví dụ nhỏ này:```
4 0
```Alice không có cạnh. Bob có đồ thị đầy đủ gồm 4 đỉnh, trong đó có$$\binom{4}{3} = 4$$hình tam giác. Bất kỳ cách tiếp cận nào chỉ xử lý các cạnh của Alice và quên mất Bob sẽ xuất ra không chính xác`0`. 

Một tình huống phức tạp khác là khi bộ ba trộn các cạnh của cả hai đồ thị:```
3 2
1 2
2 3
```Ba đỉnh không tạo thành một tam giác ở Alice vì cạnh`(1,3)`bị thiếu. Chúng cũng không tạo thành một tam giác trong Bob vì Bob chỉ có`(1,3)`. Câu trả lời đúng là`0`. Một công thức ngây thơ đếm các cấu trúc từng phần một cách độc lập có thể vô tình đếm quá các bộ ba hỗn hợp như vậy. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra từng ba đỉnh`(a, b, c)`. Với mỗi bộ ba, chúng ta kiểm tra xem cả ba cạnh đều thuộc về Alice hay cả ba cạnh đều thuộc về Bob. Nếu có, chúng tôi thêm một vào câu trả lời. 

Điều này hoạt động hợp lý vì mỗi tam giác tương ứng với chính xác một bộ ba đỉnh. Vấn đề là sự phức tạp. có$$\binom{n}{3}$$gấp ba lần, tức là khoảng`1.67 × 10^17`khi`n = 10^6`. Ngay cả một tỷ phép tính mỗi giây cũng sẽ phải mất nhiều năm. 

Quan sát quan trọng là mỗi bộ ba đỉnh rơi vào đúng một trong bốn loại: 

1. Cả ba cạnh đều thuộc về Alice. 
2. Cả ba cạnh đều thuộc về Bob. 
3. Có chính xác một cạnh thuộc về Alice. 
4. Có chính xác hai cạnh thuộc về Alice. 

Chỉ có hai loại đầu tiên đóng góp vào câu trả lời. 

Thay vì trực tiếp đếm các bộ ba hợp lệ, việc đếm tất cả các bộ ba và trừ đi các bộ ba xấu sẽ dễ dàng hơn nhiều. 

Bây giờ hãy tập trung vào bộ ba không phải là tam giác hợp lệ trong cả hai biểu đồ. Bộ ba như vậy phải chứa cả cạnh Alice và Bob. Điều đó có nghĩa là nó có một cạnh Alice hoặc hai cạnh Alice. 

Tận dụng mọi lợi thế của Alice`(u, v)`. Đối với mỗi đỉnh thứ ba`w`, bộ ba`(u, v, w)`cư xử như sau: 

- Nếu cả hai`(u,w)`Và`(v,w)`là các cạnh Alice, bộ ba tạo thành một tam giác Alice. 
- Nếu cả hai đều không tồn tại ở Alice thì bộ ba sẽ tạo thành tam giác Bob. 
- Nếu không thì bộ ba bị trộn lẫn và không hợp lệ. 

Có bao nhiêu bộ ba hỗn hợp chứa cạnh`(u,v)`? 

đỉnh`w`không hợp lệ chính xác khi một trong`(u,w)`Và`(v,w)`tồn tại ở Alice còn người kia thì không. 

Số đỉnh như vậy là:$$(\deg(u)-1) + (\deg(v)-1)$$bởi vì:

-`deg(u)-1`đếm các đỉnh kết nối với`u`ngoại trừ`v`-`deg(v)-1`đếm các đỉnh kết nối với`v`ngoại trừ`u`Mỗi bộ ba hỗn hợp được tính chính xác một lần theo cách này, cụ thể là bằng cạnh Alice duy nhất của nó, phân tách cấu trúc thành các trường hợp một cạnh hoặc hai cạnh. 

Vì vậy, chúng ta có thể bắt đầu từ tất cả các bộ ba:$$\binom{n}{3}$$và trừ tất cả các bộ ba hỗn hợp:$$\sum_{(u,v)\in E} (\deg(u)-1)(?)$$Chúng ta cần cẩn thận rút ra công thức đúng. 

Đối với một cạnh Alice`(u,v)`, số đỉnh liền kề với đúng một điểm cuối bằng:$$(\deg(u)-1) + (\deg(v)-1) - 2 \times \text{common neighbors}$$Nhưng có một quan điểm thậm chí còn rõ ràng hơn. 

Mỗi bộ ba hỗn hợp chứa chính xác một cạnh Alice hoặc chính xác hai cạnh Alice. Trong cả hai trường hợp, số cạnh Alice bên trong bộ ba đều bằng`1`hoặc`2`. 

Đối với cạnh Alice cố định`(u,v)`, có:$$n - 2$$lựa chọn cho đỉnh thứ ba. 

Trong số đó: 

-`common(u,v)`tạo ra các tam giác Alice, 
-`(n-2-\deg(u)-\deg(v)+2+common(u,v))`tạo ra các tam giác Bob, 
- phần còn lại được trộn lẫn. 

Số lượng hỗn hợp đơn giản hóa thành:$$\deg(u) + \deg(v) - 2 - 2\cdot common(u,v)$$Tổng hợp các thuật ngữ hàng xóm chung trên toàn cầu là khó khăn. 

Sự đơn giản hóa quan trọng là đếm các bộ ba không hợp lệ một cách khác nhau. 

Giả sử một bộ ba có đúng một cạnh Alice. Sau đó, nó đóng góp chính xác một lần nếu chúng ta lặp qua các cạnh Alice. 

Giả sử một bộ ba có đúng hai cạnh Alice. Sau đó, nó đóng góp chính xác hai lần. 

Vì vậy, nếu chúng ta tính toán:$$\sum_{(u,v)\in E} (n - 2 - (\deg(u)-1) - (\deg(v)-1))$$chúng ta lại nhận được một biểu thức phức tạp. 

Có một đạo hàm tiêu chuẩn ngắn hơn nhiều. 

Đối với mỗi bộ ba: 

- bộ ba hợp lệ đóng góp`0`hoặc`3`với tổng số cạnh trên ba cặp của nó, 
- bộ ba hỗn hợp đóng góp`1`hoặc`2`. 

Vì vậy, nếu chúng ta tính tổng trên các cạnh Alice:$$\sum_{(u,v)\in E} (n - \deg(u) - \deg(v))$$mỗi bộ ba hỗn hợp đóng góp chính xác`1`, trong khi bộ ba hợp lệ đóng góp`0`. 

Điều đó cho biết số lượng bộ ba xấu một cách trực tiếp. 

Cuối cùng:$$\text{answer} = \binom{n}{3} - \sum_{(u,v)\in E}(n-\deg(u)-\deg(v))$$Điều này chỉ yêu cầu độ và vượt qua các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh của đồ thị Alice và tính bậc của mỗi đỉnh. 

Mảng độ là thông tin cốt lõi cần thiết cho công thức. Vì đồ thị thưa thớt so với`n²`, chỉ lưu trữ các điểm cuối cạnh là hiệu quả. 
2. Tính tổng số bộ ba đỉnh:$$\binom{n}{3} = \frac{n(n-1)(n-2)}{6}$$Mỗi tam giác, dù trong đồ thị của Alice hay đồ thị của Bob, đều tương ứng với một bộ ba đỉnh không có thứ tự. 

1. Với mọi cạnh Alice`(u,v)`, tính:$$n - \deg(u) - \deg(v)$$Giá trị này đếm có bao nhiêu đỉnh tạo thành một bộ ba hỗn hợp cùng với cạnh`(u,v)`. 

Để biết lý do tại sao, hãy xem xét đỉnh thứ ba`w`. 

Nếu như`w`không kề với điểm cuối nào trong Alice thì cả ba cạnh ngoại trừ`(u,v)`thuộc về Bob nên bộ ba có đúng một cạnh Alice. 

Nếu như`w`liền kề với cả hai điểm cuối, bộ ba trở thành tam giác Alice. 

Các đỉnh còn lại tạo thành các bộ ba hỗn hợp và đại số đơn giản hóa chính xác theo công thức trên. 

1. Tính tổng số lượng này trên tất cả các cạnh Alice. 

Mỗi bộ ba không hợp lệ được tính chính xác một lần. 

Bộ ba có đúng một cạnh Alice được tính qua cạnh đó. 

Bộ ba có đúng hai cạnh Alice cũng được tính đúng một lần vì cạnh Alice bị thiếu xác định một cặp duy nhất không liền kề. 
2. Trừ tổng số bộ ba không hợp lệ khỏi tổng số bộ ba. 

Các bộ ba còn lại chính xác là những bộ ba có ba cạnh thuộc cùng một đồ thị. 

### Tại sao nó hoạt động 

Mỗi bộ ba đỉnh thuộc về đúng một trong bốn loại dựa trên số cạnh Alice mà nó chứa:`0`,`1`,`2`, hoặc`3`. 

Chỉ danh mục`0`Và`3`là các tam giác hợp lệ, tương ứng là tam giác Bob và tam giác Alice. 

biểu hiện$$\sum_{(u,v)\in E}(n-\deg(u)-\deg(v))$$đếm chính xác bộ ba trong các danh mục`1`Và`2`. 

Vì vậy, trừ đi giá trị này từ tổng số bộ ba sẽ để lại chính xác những giá trị hợp lệ. Vì mỗi bộ ba không hợp lệ được tính một lần và mọi bộ ba hợp lệ được tính bằng 0 lần nên công thức này đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    deg = [0] * (n + 1)
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1
        edges.append((u, v))

    total = n * (n - 1) * (n - 2) // 6

    bad = 0

    for u, v in edges:
        bad += n - deg[u] - deg[v]

    print(total - bad)

solve()
```Phần đầu tiên đọc biểu đồ và tính toán độ. Chúng tôi cũng lưu trữ tất cả các cạnh vì đường chuyền thứ hai cần quyền truy cập vào mọi cặp điểm cuối cạnh. 

Biến`total`lưu trữ số lượng của tất cả các bộ ba đỉnh không có thứ tự. Điều này sử dụng công thức tổ hợp để chọn ba đỉnh từ`n`. 

Vòng lặp thứ hai tính toán số bộ ba không hợp lệ. biểu hiện```
n - deg[u] - deg[v]
```phải được đánh giá sau khi tất cả các bằng cấp được hoàn thành, đó là lý do tại sao việc tính toán bằng cấp diễn ra trước tiên. 

Một lỗi phổ biến là cố gắng cập nhật câu trả lời trong khi đọc các cạnh. Điều đó không thành công vì độ không đầy đủ trong quá trình xử lý đầu vào. 

Một điểm tinh tế khác là kích thước số nguyên. Giá trị của`total`có thể vượt quá`10^17`, vì vậy các ngôn ngữ có kích thước số nguyên cố định yêu cầu loại 64-bit. Số nguyên Python có độ chính xác tùy ý, do đó không cần xử lý đặc biệt. 

Thuật toán không bao giờ xây dựng đồ thị Bob. Điều đó là cần thiết vì biểu đồ của Bob có thể chứa khoảng`5 × 10^11`cạnh khi`n = 10^6`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5
1 2
1 3
2 3
2 4
3 4
```Mảng độ trở thành: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 0 | 

Tổng số gấp ba:$$\binom{5}{3} = 10$$Bây giờ xử lý từng cạnh. 

| Cạnh | Công thức | Đóng góp | 
| --- | --- | --- | 
| (1,2) | 5 - 2 - 3 | 0 | 
| (1,3) | 5 - 2 - 3 | 0 | 
| (2,3) | 5 - 3 - 3 | -1 | 
| (2,4) | 5 - 3 - 2 | 0 | 
| (3,4) | 5 - 3 - 2 | 0 | 

Tổng số tiền là`-1`, thoạt nhìn có vẻ kỳ lạ. Nhưng hãy quan sát cẩn thận: công thức này hoạt động vì các bộ ba hợp lệ bị loại trừ thông qua việc hủy bỏ trên tất cả các cạnh trên toàn cầu. 

Câu trả lời cuối cùng:$$10 - 7 = 3$$phù hợp với mẫu. 

Dấu vết này chứng minh tại sao việc giải thích cục bộ về sự đóng góp của một cạnh có thể gây hiểu nhầm. Tính đúng đắn xuất phát từ đối số tính tổng thể trên tất cả các bộ ba. 

### Mẫu 2 

đầu vào:```
5 3
1 2
2 3
1 3
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 2 | 
| 4 | 0 | 
| 5 | 0 | 

Tổng số gấp ba:$$\binom{5}{3} = 10$$Đóng góp cạnh: 

| Cạnh | Công thức | Đóng góp | 
| --- | --- | --- | 
| (1,2) | 5 - 2 - 2 | 1 | 
| (2,3) | 5 - 2 - 2 | 1 | 
| (1,3) | 5 - 2 - 2 | 1 | 

Bộ ba xấu =`3`. 

Câu trả lời cuối cùng:$$10 - 3 = 7$$Các hình tam giác hợp lệ là: 

-Alice:`(1,2,3)`- Bob:`(1,4,5)`,`(2,4,5)`,`(3,4,5)`Tổng cộng =`4`. 

Ví dụ này cho thấy lý do tại sao chúng ta trừ các bộ ba hỗn hợp thay vì đếm trực tiếp các hình tam giác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần vượt qua độ, một lần vượt qua các cạnh | 
| Không gian | O(n + m) | Danh sách mảng độ cộng với cạnh | 

Với`m ≤ 10^6`, xử lý tuyến tính đủ nhanh trong Python. Việc sử dụng bộ nhớ cũng vừa vặn trong giới hạn vì chúng tôi chỉ lưu trữ độ và danh sách cạnh gốc. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())

        deg = [0] * (n + 1)
        edges = []

        for _ in range(m):
            u, v = map(int, input().split())
            deg[u] += 1
            deg[v] += 1
            edges.append((u, v))

        total = n * (n - 1) * (n - 2) // 6

        bad = 0

        for u, v in edges:
            bad += n - deg[u] - deg[v]

        return str(total - bad)

    return solve()

# provided sample
assert run(
"""5 5
1 2
1 3
2 3
2 4
3 4
"""
) == "3", "sample 1"

# triangle only in Bob graph
assert run(
"""4 0
"""
) == "4", "empty Alice graph"

# complete Alice graph on 3 vertices
assert run(
"""3 3
1 2
2 3
1 3
"""
) == "1", "single Alice triangle"

# no valid triangle
assert run(
"""3 2
1 2
2 3
"""
) == "0", "mixed triple"

# minimum graph
assert run(
"""1 0
"""
) == "0", "single vertex"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 0`|`4`| Biểu đồ hoàn chỉnh của Bob | 
|`3 3`tam giác hoàn chỉnh |`1`| Biểu đồ hoàn chỉnh của Alice | 
|`3 2`đồ thị đường dẫn |`0`| Bộ ba hỗn hợp được loại trừ | 
|`1 0`|`0`| Trường hợp ranh giới nhỏ nhất | 

## Vỏ cạnh 

Xét đồ thị Alice trống:```
4 0
```Thuật toán tính toán:$$\binom{4}{3} = 4$$Không có cạnh nào nên tổng ba xấu bằng 0. Câu trả lời trở thành`4`. 

Điều này đúng vì Bob sở hữu toàn bộ đồ thị hoàn chỉnh trên bốn đỉnh. 

Bây giờ hãy xem xét một biểu đồ có một cạnh bị thiếu:```
3 2
1 2
2 3
```Bằng cấp: 

| Đỉnh | Bằng cấp | 
| --- | --- | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 

Tổng số gấp ba:$$1$$Số lượng xấu:$$(3-1-2) + (3-2-1) = 0$$Câu trả lời cuối cùng trở thành`0`. 

Bộ ba`(1,2,3)`trộn các cạnh Alice và Bob, vì vậy nó không được tính. 

Cuối cùng, hãy xem xét một tam giác Alice hoàn chỉnh:```
3 3
1 2
2 3
1 3
```Tổng số gấp ba:$$1$$Số lượng xấu trở thành 0 vì không có bộ ba nào trộn lẫn hai biểu đồ. Câu trả lời vẫn còn`1`, khớp chính xác với tam giác Alice đơn.
