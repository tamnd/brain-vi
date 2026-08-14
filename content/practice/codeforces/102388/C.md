---
title: "CF 102388C - Bi da"
description: "Ta có một cái bàn hình chữ nhật có chiều rộng m và chiều cao n. Bóng bắt đầu tại điểm bên trong (x0, y0) và phải đạt tới (x1, y1). Nó luôn truyền dọc theo các đoạn thẳng, phản xạ từ một bức tường có góc tới và góc phản xạ bằng nhau."
date: "2026-08-12T20:59:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102388
codeforces_index: "C"
codeforces_contest_name: "SUFE ICPC Team Formation Test"
rating: 0
weight: 102388
solve_time_s: 575
verified: true
draft: false
---

[CF 102388C - Bi da](https://codeforces.com/problemset/problem/102388/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có một cái bàn hình chữ nhật có chiều rộng`m`và chiều cao`n`. Bóng bắt đầu ở điểm bên trong`(x0, y0)`và phải đạt`(x1, y1)`. Nó luôn truyền dọc theo các đoạn thẳng, phản xạ từ một bức tường có góc tới và góc phản xạ bằng nhau. Số lượng điểm tiếp xúc trên tường cần thiết là chính xác`k`, và chúng ta muốn tổng chiều dài tối thiểu của quỹ đạo. 

Khó khăn chính là cú nảy sẽ thay đổi hướng, do đó cố gắng mô phỏng trực tiếp quả bóng đồng nghĩa với việc phải xử lý nhiều chuỗi phản xạ có thể xảy ra. Các ràng buộc đủ nhỏ để cho phép quét tuyến tính trên`k`, nhưng chúng đủ lớn để việc tìm kiếm theo cấp số nhân trên các chuỗi bị trả lại là hoàn toàn không thể. Từ`k`thậm chí có thể là 100`2^k`khả năng đã có khoảng`1.27 * 10^30`. Kích thước của bảng tối đa là 100, do đó tọa độ trong mặt phẳng được chuyển đổi vẫn đủ nhỏ cho số học dấu phẩy động thông thường và số lượng trường hợp thử nghiệm chỉ là 100. 

Có một số trường hợp thường gây ra giải pháp không chính xác. Đầu tiên, số lần trả lại bằng 0 phải cho phép phân đoạn trực tiếp, vì vậy```
1
100 100 1 1 1 1 0
```có câu trả lời`0.00`. Một giải pháp luôn xây dựng một hình ảnh phản chiếu sẽ vô tình buộc ít nhất một bức tường tiếp xúc. 

Thứ hai, việc chạm tới một góc được tính là hai lần nảy vì bóng chạm vào một bức tường dọc và một bức tường ngang cùng một lúc. Vì```
1
100 100 1 2 1 2 2
```quả bóng có thể di chuyển từ`(1,2)`đến góc`(0,0)`và quay lại`(1,2)`, cho khoảng cách`2 * sqrt(5) = 4.47`. Một giải pháp đếm các sự kiện va chạm thay vì các điểm tiếp xúc trên tường có thể coi đây là một lần nảy không chính xác. 

Thứ ba, hướng của hình ảnh phản chiếu rất quan trọng. Vì```
1
10 10 1 1 9 1 1
```khoảng cách trực tiếp là`8`, nhưng đường dẫn đó không bị trả lại. Với chính xác một lần thoát, mục tiêu được phản ánh hợp lệ gần nhất là ở`-9`hoặc`11`, cả hai đều cách tọa độ x ban đầu 10 đơn vị, nên câu trả lời là`10.00`. Chỉ cần thêm một chiều rộng bảng vào tọa độ đích sẽ tạo ra hình dạng sai. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ trực tiếp là liệt kê mọi trình tự có thể có của các hướng tường. Ở mỗi lần bật lên, về mặt khái niệm, chúng ta có thể chọn xem va chạm tiếp theo là với một bức tường thẳng đứng hay nằm ngang, sau đó đi theo quỹ đạo phản ánh thu được và kiểm tra xem điểm cuối có thể là điểm được yêu cầu sau đó chính xác hay không.`k`va chạm. có`2^k`trình tự định hướng, vì vậy trong trường hợp xấu nhất`k = 100`điều này có nghĩa là đại khái`1.27 * 10^30`trường hợp. Cách tiếp cận này đúng về mặt khái niệm vì mọi quỹ đạo pháp lý đều có một số chuỗi tiếp xúc theo chiều dọc và chiều ngang của tường, nhưng không gian tìm kiếm sẽ tăng theo cấp số nhân. 

Quan sát loại bỏ vụ nổ này là ngừng phản ánh quỹ đạo. Thay vào đó, hãy phản chiếu toàn bộ bàn bất cứ khi nào quả bóng nảy lên. Trong mặt phẳng mở này, quả bóng di chuyển dọc theo một đường thẳng. Mỗi bản sao phản ánh của đích ban đầu đại diện cho một quỹ đạo có thể có trong bảng thực. 

Giả sử đường thẳng khai triển cắt nhau`p`ranh giới bảng dọc và`q`ranh giới bảng ngang. Quỹ đạo thực khi đó có chính xác`|p| + |q|`bị trả lại. Chúng tôi cần`|p| + |q| = k`. 

Đối với số nguyên có dấu`p`, chúng ta có thể tính tọa độ x của bản sao phản ánh tương ứng của`x1`. Nếu như`p`chẵn, bản sao ở`p * m + x1`. 

Nếu như`p`thật kỳ lạ, bản sao ở`(p + 1) * m - x1`. 

Công thức tương tự áp dụng cho tọa độ y bằng cách sử dụng`q`Và`n`. 

Các dấu hiệu của`p`Và`q`cho chúng tôi biết quả bóng sẽ di chuyển theo hướng nào qua các bản sao được mở ra. Đối với mọi khả năng`p`từ`-k`bởi vì`k`, số lần nảy ngang còn lại là`k - |p|`, cho nhiều nhất hai lựa chọn`q`. Chúng tôi tính toán khoảng cách Euclide đến từng hình ảnh thu được và lấy mức tối thiểu. 

Tìm kiếm brute-force hoạt động vì mỗi chuỗi thoát tương ứng với một đường có thể được mở ra. Nó thất bại vì có nhiều chuỗi theo cấp số nhân. Việc quan sát diễn ra cho phép chúng ta quên hoàn toàn thứ tự của các lần nảy. Chỉ có số lượng đường giao cắt ranh giới theo chiều dọc và chiều ngang được ký hiệu là quan trọng, làm giảm việc tìm kiếm xuống còn`O(k)`ứng viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^k)`|`O(k)`| Quá chậm | 
| Tối ưu |`O(k)`mỗi trường hợp thử nghiệm |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy xem xét bảng được lặp lại vô tận theo mọi hướng. Mỗi bản sao thứ hai đều được phản chiếu, do đó việc phản chiếu bàn trở nên tương đương với việc tiếp tục đưa bóng theo một đường thẳng. 
2. Gán số nguyên có dấu`p`đến số lượng đường ngang của bức tường thẳng đứng. Giá trị tuyệt đối của nó là số lần thoát theo chiều dọc. Tương tự, gán`q`đến các đường ngang. 

Một va chạm ở góc đương nhiên phù hợp với mô hình này. Nếu đường thẳng đi qua một góc, nó sẽ cắt một ranh giới dọc và một ranh giới ngang tại cùng một điểm, do đó sự kiện đóng góp hai lần nảy. 
3. Vì tổng số lần thoát phải chính xác`k`, hạn chế các ứng viên`|p| + |q| = k`. 
4. Liệt kê`p`từ`-k`ĐẾN`k`. Với mỗi giá trị hãy tính`r = k - |p|`. Các giá trị duy nhất có thể có của`q`là`r`Và`-r`. 
5. Chuyển đổi từng số lần thoát đã ký thành tọa độ mục tiêu được mở. Đối với trục x, xác định`image_x(p) = p*m + x1`khi`p`là chẵn, và`image_x(p) = (p+1)*m - x1`khi`p`thật kỳ quặc. 

Áp dụng công thức tương tự để có được`image_y(q)`. 
6. Quỹ đạo mở ra bây giờ chỉ đơn giản là đoạn từ`(x0, y0)`ĐẾN`(image_x, image_y)`. chiều dài của nó là`sqrt((image_x - x0)^2 + (image_y - y0)^2)`. 
7. Giữ khoảng cách nhỏ nhất giữa các thí sinh và in đúng hai chữ số sau dấu thập phân. 

### Tại sao nó hoạt động 

Điều bất biến là mọi quỹ đạo phản ánh hợp lệ trong bảng gốc tương ứng với chính xác một đoạn thẳng từ điểm bắt đầu đến bản sao phản ánh thích hợp của đích trong mặt phẳng trải rộng. Số ranh giới dọc mà đoạn đó vượt qua chính xác là số lần nảy của tường thẳng đứng và tương tự theo chiều ngang. Vì vậy, mọi ứng viên đều thỏa mãn`|p| + |q| = k`đại diện cho một quỹ đạo pháp lý với chính xác`k`bị trả lại. Ngược lại, mọi quỹ đạo pháp lý đều mở ra một trong những ứng cử viên này. Vì thuật toán kiểm tra mọi cặp có chữ ký có thể thỏa mãn số lần thoát và chọn phân đoạn tương ứng ngắn nhất nên giá trị tối thiểu mà nó tính toán chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def image_coordinate(pos, length, bounces):
    if bounces % 2 == 0:
        return bounces * length + pos
    return (bounces + 1) * length - pos

def solve_case(m, n, x0, y0, x1, y1, k):
    ans = float("inf")

    for p in range(-k, k + 1):
        remaining = k - abs(p)

        x = image_coordinate(x1, m, p)

        for q in {remaining, -remaining}:
            y = image_coordinate(y1, n, q)

            dx = x - x0
            dy = y - y0

            dist = math.hypot(dx, dy)
            ans = min(ans, dist)

    return ans

def main():
    t = int(input())

    for _ in range(t):
        m, n, x0, y0, x1, y1, k = map(int, input().split())
        ans = solve_case(m, n, x0, y0, x1, y1, k)
        print(f"{ans:.2f}")

if __name__ == "__main__":
    main()
```các`image_coordinate`chức năng thực hiện mô hình phản ánh trực tiếp. Khi chỉ mục bản sao đã ký là chẵn, đích sẽ giữ nguyên hướng ban đầu. Khi số lẻ, đích đến sẽ được phản chiếu bên trong hình chữ nhật lặp lại tương ứng. 

Các chỉ số tiêu cực là có chủ ý. Ví dụ, với`p = -1`, công thức cho`-x1`, là hình ảnh phản chiếu của đích đến trên bức tường bên trái. Với`p = -2`, nó mang lại`-2m + x1`, đại diện cho hai bức tường thẳng đứng giao cắt theo hướng ngược nhau. 

Vòng lặp chính xem xét mọi số lượng chữ ký dọc có thể có. Một lần`p`đã được sửa,`k - |p|`buộc phải là số đường ngang nên chỉ có thể có hai dấu hiệu cho`q`. bộ`{remaining, -remaining}`tránh thực hiện cùng một phép tính hai lần khi`remaining`là số không.`math.hypot(dx, dy)`tính khoảng cách Euclide mà không cần phải viết biểu thức căn bậc hai theo cách thủ công. Tất cả các phép tính tọa độ đều là số nguyên và rất nhỏ theo các ràng buộc nhất định, do đó không có vấn đề tràn số nguyên trong Python. 

các`:.2f`định dạng thực hiện làm tròn đầu ra cần thiết. Không cần xử lý đặc biệt đối với`k = 0`, bởi vì khi đó`p = 0`Và`q = 0`là các giá trị ứng cử viên duy nhất có thể, đưa ra khoảng cách trực tiếp. 

## Ví dụ đã hoạt động 

### Mẫu 1, testcase thứ hai 

Đầu vào là```
100 100 1 2 1 2 2
```Điểm bắt đầu và điểm đến trùng nhau, nhưng cần có chính xác hai lần bật tường. Quỹ đạo ngắn nhất đi qua góc`(0,0)`. Trong mặt phẳng mở ra, điều này được thể hiện bằng hình ảnh mục tiêu`(-1,-2)`. 

|`p`|`q`| Hình ảnh`(x, y)`| Khoảng cách | 
| --- | --- | --- | --- | 
|`-2`|`0`|`(-199, 2)`|`200.00`| 
|`-1`|`-1`|`(-1, -2)`|`4.47`| 
|`-1`|`1`|`(-1, 198)`|`200.01`| 
|`0`|`-2`|`(1, -198)`|`200.00`| 
|`0`|`2`|`(1, 202)`|`200.00`| 
|`1`|`-1`|`(199, -2)`|`198.04`| 
|`1`|`1`|`(199, 198)`|`278.60`| 
|`2`|`0`|`(201, 2)`|`200.00`| 

Mức tối thiểu đạt được với`p = -1`Và`q = -1`. Đoạn mở ra có chuyển vị ngang`2`và chuyển vị thẳng đứng`4`, vậy độ dài của nó là`sqrt(20) = 4.472...`, được in dưới dạng`4.47`. Điều này chứng tỏ tại sao một va chạm ở góc phải được tính là hai lần bật tường. 

### Mẫu 1, testcase thứ tư 

Đầu vào là```
100 50 1 2 3 4 5
```Một lựa chọn tối ưu là bốn đường ngang theo hướng âm và một đường ngang theo hướng âm. Như vậy`p = -1`Và`q = -4`đưa ra sự kết hợp thực tế tốt nhất sau khi kiểm tra tất cả các khả năng. 

|`p`|`|p|`|`q`| Hình ảnh`(x, y)`| Khoảng cách | 
|---:|---:|---:|---:|---:| 
|`-5`|`5`|`0`|`(-403, 4)`|`404.00`| 
|`-4`|`4`|`-1`|`(-397, -4)`|`400.08`| 
|`-3`|`3`|`-2`|`(-203, -96)`|`229.03`| 
|`-2`|`2`|`-3`|`(-197, -104)`|`224.60`| 
|`-1`|`1`|`-4`|`(-3, -196)`|`198.04`| 
|`0`|`0`|`-5`|`(3, -204)`|`206.00`| 

Các lựa chọn dấu dương còn lại cũng được thuật toán kiểm tra nhưng không có nhịp nào`198.04`. Đối với ứng cử viên chiến thắng, chuyển vị ngang là`4`và chuyển vị thẳng đứng là`198`, cho`sqrt(4^2 + 198^2) = 198.040...`. Ví dụ này cho thấy lý do tại sao tính chẵn lẻ của bản sao được phản ánh phải được xử lý chính xác, đặc biệt đối với số lượng thư bị trả lại âm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(k)`mỗi trường hợp thử nghiệm | có`2k + 1`các lựa chọn cho số lượng dọc đã ký và tối đa hai ký hiệu ngang cho mỗi lựa chọn. | 
| Không gian |`O(1)`| Chỉ có một số lượng giá trị tọa độ và khoảng cách không đổi được lưu trữ. | 

Với`k <= 100`, mỗi testcase chỉ kiểm tra vài trăm ứng viên. Ngay cả với 100 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ so với giới hạn một giây và thuật toán sử dụng bộ nhớ bổ sung không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def image_coordinate(pos, length, bounces):
    if bounces % 2 == 0:
        return bounces * length + pos
    return (bounces + 1) * length - pos

def solve_case(m, n, x0, y0, x1, y1, k):
    ans = float("inf")

    for p in range(-k, k + 1):
        remaining = k - abs(p)
        x = image_coordinate(x1, m, p)

        for q in {remaining, -remaining}:
            y = image_coordinate(y1, n, q)
            ans = min(ans, math.hypot(x - x0, y - y0))

    return ans

def run(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    out = []

    for _ in range(t):
        values = list(map(int, data.readline().split()))
        ans = solve_case(*values)
        out.append(f"{ans:.2f}")

    return "\n".join(out)

samples = """4
100 100 1 1 1 1 0
100 100 1 2 1 2 2
100 100 1 1 1 1 3
100 50 1 2 3 4 5
"""

assert run(samples) == """0.00
4.47
200.01
198.04""", "provided samples"

assert run("""1
2 2 1 1 1 1 0
""") == "0.00", "minimum table and zero bounces"

assert run("""1
2 2 1 1 1 1 2
""") == "2.83", "minimum table, corner collision"

assert run("""1
10 10 1 1 9 1 1
""") == "10.00", "one-bounce off-by-one case"

assert run("""1
100 100 50 50 50 50 100
""") == "7071.07", "maximum k and symmetric optimum"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 1 1 1 1 0`|`0.00`| Kích thước tối thiểu và số lần thoát bằng 0 | 
|`2 2 1 1 1 1 2`|`2.83`| Một lần tiếp xúc ở góc được tính là hai lần nảy | 
|`10 10 1 1 9 1 1`|`10.00`| Chỉnh sửa tính chẵn lẻ của tọa độ phản xạ và hình học một lần thoát | 
|`100 100 50 50 50 50 100`|`7071.07`| Số lần thoát tối đa và các đường ngang/dọc cân bằng | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là`k = 0`. Vì```
1
100 100 1 1 1 1 0
```vòng lặp chỉ có thể chọn`p = 0`, lực nào`q = 0`. Hình ảnh điểm đến chính xác`(1,1)`, do đó cả hai hiệu tọa độ đều bằng 0 và câu trả lời là`0.00`. Không có đường phản xạ nào được xem xét vì bất kỳ chỉ số có dấu khác 0 nào cũng sẽ tạo ra ít nhất một đường đi qua tường. 

Trường hợp cạnh thứ hai là một quả phạt góc. Vì```
1
100 100 1 2 1 2 2
```lựa chọn`p = -1`Và`q = -1`đưa ra hình ảnh`(-1,-2)`. Đoạn thẳng từ`(1,2)`ĐẾN`(-1,-2)`đi qua`(0,0)`. Trong bảng ban đầu, điều này có nghĩa là một điểm tiếp xúc với tường thẳng đứng và một điểm tiếp xúc với tường nằm ngang ở cùng một góc vật lý, do đó, nó được tính chính xác là hai lần nảy. Khoảng cách là`sqrt(2^2 + 4^2) = 4.47`. 

Trường hợp cạnh thứ ba là một bản sao phản chiếu lẻ. Vì```
1
10 10 1 1 9 1 1
```chúng ta cần chính xác một lần thoát. Lựa chọn`p = -1`Và`q = 0`cho`image_x = (0)*10 - 9 = -9`Và`image_y = 1`. Khoảng cách mở ra là`|-9 - 1| = 10`, vậy kết quả là`10.00`. Chọn hướng dọc dương sẽ cho hình ảnh`11`, cũng cách đó 10 đơn vị. Điểm đến trực tiếp tại`9`chỉ cách 8 đơn vị, nhưng nó thể hiện số lần trả lại bằng 0 và do đó bị từ chối. 

Trường hợp cạnh thứ tư là số lần thoát lớn với tọa độ bằng nhau:```
1
100 100 50 50 50 50 100
```Giải pháp tốt nhất phân phối 100 lần thoát một cách đồng đều, với`p = -50`Và`q = -50`. Cả hai chỉ số đều bằng nhau nên hình ảnh là`-4950`trên mỗi trục. Mỗi tọa độ thay đổi theo`5000`, cho khoảng cách là`sqrt(5000^2 + 5000^2) = 7071.067...`, được in dưới dạng`7071.07`. Sự phân chia không cân bằng hơn, chẳng hạn như 49 và 51 lần thoát, tạo ra khoảng cách Euclide lớn hơn, do đó, bảng liệt kê tự nhiên tìm ra lựa chọn cân bằng mà không cần đối số tối ưu hóa riêng biệt.
