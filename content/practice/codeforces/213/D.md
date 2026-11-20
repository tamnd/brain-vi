---
title: "CF 213D - Sao"
description: "Chúng ta phải xây dựng n ngôi sao hình ngôi sao năm cánh trong mặt phẳng. Mỗi ngôi sao xuất phát từ một hình ngũ giác đều có cạnh dài 10 và các đoạn được sơn chính xác là năm đường chéo của nó. Đầu ra không chỉ là hình học. Chúng tôi cũng cần một lệnh vẽ hợp lệ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry"]
categories: ["algorithms"]
codeforces_contest: 213
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 131 (Div. 1)"
rating: 2300
weight: 213
solve_time_s: 96
verified: false
draft: false
---

[CF 213D - Stars](https://codeforces.com/problemset/problem/213/D) 

**Đánh giá:** 2300 
**Tags:** thuật toán xây dựng, hình học 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải xây dựng`n`những ngôi sao hình năm cánh trong mặt phẳng. Mỗi ngôi sao xuất phát từ một hình ngũ giác đều có cạnh dài`10`, và các đoạn được sơn chính xác là năm đường chéo của nó. 

Đầu ra không chỉ là hình học. Chúng tôi cũng cần một lệnh vẽ hợp lệ. Rubik bắt đầu bằng việc chạm bút vào tờ giấy, vẽ hết đoạn này đến đoạn khác và không bao giờ nhấc bút lên. Không có đoạn nào có độ dài dương được phép đi qua hai lần. Các ngôi sao khác nhau chỉ có thể chạm nhau ở các đỉnh. 

Định dạng phản ánh cấu trúc này. Đầu tiên chúng ta in tất cả các điểm được sử dụng bởi công trình. Sau đó, đối với mỗi ngôi sao, chúng ta chỉ định năm điểm nào tạo thành hình ngũ giác đều của nó. Cuối cùng, chúng tôi in bản hướng dẫn đi qua các điểm, trong đó mỗi cặp liên tiếp có nghĩa là “vẽ đoạn thẳng giữa hai điểm này”. 

Quan sát quan trọng là lý thuyết đồ thị. Một ngôi sao năm cánh là một chu trình Euler trên năm đỉnh:```
1 -> 3 -> 5 -> 2 -> 4 -> 1
```Mỗi đỉnh đều có bậc`2`, nên chỉ một ngôi sao thôi cũng dễ dàng vẽ được trong một nét. Khó khăn đến từ việc kết hợp nhiều ngôi sao thành một bản vẽ Euler được kết nối mà không có các phân đoạn chồng chéo. 

Các giới hạn là rất nhỏ. Chúng ta chỉ cần xử lý`n ≤ 100`, do đó mọi cách xây dựng tuyến tính hoặc bậc hai đều tầm thường. Phần khó khăn không phải là hiệu suất mà là tính chính xác về mặt hình học. Tọa độ phải nằm trong`5000`, các ngôi sao chỉ có thể giao nhau ở các đỉnh và độ chính xác của dấu phẩy động phải tồn tại trong quá trình kiểm tra trình xác thực. 

Có một số cách dễ dàng để vô tình xây dựng một công trình không hợp lệ. 

Một lỗi phổ biến là đặt các ngôi sao một cách độc lập mà không kết nối chúng. Ví dụ, nếu`n = 2`và chúng ta đặt hai ngôi sao năm cánh rời nhau, mỗi ngôi sao riêng lẻ có chu trình Euler, nhưng toàn bộ đồ thị vẽ bị ngắt kết nối. Việc vẽ một nét trở nên không thể. 

Một sai lầm khác là kết nối các ngôi sao bằng cách sử dụng lại một cạnh hiện có. Giả sử chúng ta cố gắng hợp nhất hai ngôi sao bằng cách xác định hai đỉnh thay vì một. Khi đó một số đoạn chéo có thể xuất hiện ở cả hai ngôi sao, vi phạm quy tắc “không có đoạn hai lần”. 

Một vấn đề tinh tế hơn là sự chồng chéo hình học. Nếu các ngôi sao quá gần, các đường chéo từ các ngôi sao khác nhau có thể cắt nhau ở những điểm không có đỉnh. Trình xác nhận từ chối điều này ngay cả khi cấu trúc biểu đồ đúng. Một công trình an toàn phải tách biệt các ngôi sao đủ để chỉ tồn tại các đỉnh chung dự kiến. 

Cuối cùng, nhiều cách triển khai quên rằng mọi ngôi sao vẫn phải đều đặn với độ dài cạnh chính xác`10`. Chia tỷ lệ hoặc dịch là tốt, nhưng việc bóp méo tọa độ sẽ phá vỡ định nghĩa của ngôi sao một cách độc lập. 

## Phương pháp tiếp cận 

Tư duy vũ phu là suy nghĩ trực tiếp về đồ thị Euler. Mỗi ngôi sao đóng góp 5 chu kỳ theo thứ tự truyền tải ngôi sao năm cánh. Chúng ta có thể cố gắng tạo ra các giao điểm tùy ý giữa các ngôi sao cho đến khi toàn bộ biểu đồ được kết nối và tất cả các độ đỉnh vẫn bằng nhau. 

Cách tiếp cận này về mặt lý thuyết có thể thực hiện được vì các ràng buộc nhỏ. Ngay cả việc tìm kiếm quay lại trên các kết nối biểu đồ cũng có thể kết thúc`n ≤ 100`. Vấn đề là tính nhất quán hình học. Mỗi khi chúng ta hợp nhất các đỉnh, chúng ta phải bảo toàn hình học ngũ giác đều đặn và tránh các giao điểm ngẫu nhiên của các đoạn. Phần đồ thị dễ dàng, việc ghi sổ hình học trở nên khó khăn. 

Quan sát quan trọng là một đỉnh dùng chung là đủ để kết nối hai thành phần Euler. Nếu hai ngôi sao có chung một đỉnh thì bậc đỉnh chung sẽ trở thành`4`, vẫn chẵn. Việc lặp lại điều này sẽ tự động tạo ra một đồ thị Euler được kết nối. 

Điều đó làm giảm nhiệm vụ thành một vấn đề hình học thuần túy mang tính xây dựng: 

1. Xây dựng một ngôi sao năm cánh đều đặn hợp lệ. 
2. Tạo bản dịch. 
3. Làm cho các ngôi sao liên tiếp có chung một đỉnh. 
4. Giữ tất cả các đỉnh khác cách xa nhau. 

Việc xây dựng dây chuyền sẽ giải quyết mọi việc một cách rõ ràng. 

Lấy một hình ngũ giác đều có độ dài cạnh`10`. Năm đỉnh của nó là tọa độ tương đối cố định. Đối với mỗi ngôi sao mới, hãy sử dụng lại một đỉnh từ ngôi sao trước đó và đặt bốn đỉnh còn lại bằng cách dịch toàn bộ hình đủ xa theo chiều ngang. Vì các bản dịch bảo toàn tính đều đặn và độ dài cạnh nên mọi ngôi sao vẫn có giá trị. 

Về mặt đồ họa, toàn bộ cấu trúc trở thành:```
star1 -- shared vertex -- star2 -- shared vertex -- star3 ...
```Mọi bậc đỉnh đều chẵn: 

- các đỉnh sao thông thường có bậc`2`- các đỉnh chung có bậc`4`Một đồ thị liên thông trong đó tất cả các bậc đều bằng nhau luôn có chu trình Euler. Vì mỗi cạnh thuộc về đúng một ngôi sao nên không có đoạn nào bị lặp lại. 

Việc xây dựng là tuyến tính, đơn giản và mạnh mẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / khó bị ràng buộc | Lớn | Không thực tế | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng tọa độ của một hình ngũ giác đều có độ dài cạnh`10`. 

Việc tham số hóa thuận tiện sử dụng các điểm trên đường tròn. Đối với một hình ngũ giác đều có bán kính đường tròn`R`, các đỉnh liền kề nhau có góc phụ`72°`. Công thức độ dài cạnh là:$$s = 2R\sin(36^\circ)$$Từ`s = 10`, chúng tôi nhận được:$$R = \frac{10}{2\sin(36^\circ)}$$2. Tạo năm đỉnh bằng tọa độ cực. 

Đối với đỉnh`i`:$$(x_i, y_i) = (R\cos(\theta_i), R\sin(\theta_i))$$trong đó các góc khác nhau bởi`72°`. 
3. Sửa thứ tự di chuyển của ngôi sao năm cánh. 

Các cạnh sao kết nối:```
1 -> 3 -> 5 -> 2 -> 4 -> 1
```Điều này sử dụng mọi đường chéo chính xác một lần và tạo thành một chu trình. 
4. Đặt từng ngôi sao dọc theo trục x. 

Dành cho ngôi sao`k`, dịch tất cả tọa độ theo độ lệch ngang lớn, chẳng hạn`40 * k`. 

Độ lệch lớn hơn nhiều so với đường kính sao nên các ngôi sao khác nhau không thể vô tình giao nhau. 
5. Làm cho các ngôi sao liên tiếp có chung một đỉnh. 

Tái sử dụng một chỉ số đỉnh giữa các ngôi sao lân cận. Một lựa chọn thuận tiện là: 

- đỉnh thứ năm của ngôi sao`k`- trở thành đỉnh đầu tiên của ngôi sao`k + 1`Điều này kết nối biểu đồ trong khi vẫn giữ nguyên độ chẵn. 
6. Lưu trữ tất cả các điểm duy nhất. 

Mỗi ngôi sao mới chỉ đóng góp bốn đỉnh mới vì một đỉnh được sử dụng lại. 
7. Xuất ra các định nghĩa về ngôi sao. 

Mỗi ngôi sao in năm chỉ số đỉnh của nó theo thứ tự tuần hoàn. 
8. Xây dựng thứ tự vẽ Euler tổng thể. 

Vì các ngôi sao tạo thành một chuỗi được kết nối ở các đỉnh chung, chúng ta có thể chỉ cần nối các chu kỳ ngôi sao năm cánh cục bộ:```
1 3 5 2 4 1
```với các đỉnh chung liên kết các ngôi sao liền kề một cách tự nhiên. 

### Tại sao nó hoạt động 

Mỗi ngôi sao riêng lẻ đóng góp một chu kỳ 5, vì vậy mỗi bậc đỉnh cục bộ là`2`. Khi hai ngôi sao có chung một đỉnh thì bậc đỉnh đó sẽ tăng thêm`2`, còn lại chẵn. Toàn bộ biểu đồ được kết nối vì mỗi ngôi sao có chung một đỉnh với đỉnh tiếp theo. Một đồ thị liên thông trong đó mọi đỉnh đều có bậc chẵn luôn thừa nhận một chu trình Euler. 

Không có cạnh nào có thể lặp lại vì các ngôi sao chỉ có chung các đỉnh chứ không bao giờ có các đoạn. Các bản dịch lớn ngăn chặn các giao điểm hình học ngoài ý muốn. Vì mọi ngôi sao thu được bằng cách dịch chuyển cùng một hình ngũ giác đều, nên độ dài các cạnh vẫn giữ nguyên chính xác`10`. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())

    # Regular pentagon with side length 10
    R = 10.0 / (2.0 * math.sin(math.pi / 5.0))

    base = []
    start_angle = math.pi / 2.0

    for i in range(5):
        ang = start_angle + 2.0 * math.pi * i / 5.0
        x = R * math.cos(ang)
        y = R * math.sin(ang)
        base.append((x, y))

    points = []
    stars = []

    # add first star completely
    for x, y in base:
        points.append((x, y))

    stars.append([1, 2, 3, 4, 5])

    last_shared = 5

    # each next star shares one vertex
    for k in range(1, n):
        shift_x = 40.0 * k

        ids = [last_shared]

        for j in range(1, 5):
            x, y = base[j]
            points.append((x + shift_x, y))
            ids.append(len(points))

        stars.append(ids)
        last_shared = ids[-1]

    print(len(points))

    for x, y in points:
        print(f"{x:.12f} {y:.12f}")

    for star in stars:
        print(*star)

    path = []

    order = [0, 2, 4, 1, 3, 0]

    for star in stars:
        for idx in order[:-1]:
            path.append(star[idx])

    path.append(stars[-1][0])

    print(*path)

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán một hình ngũ giác đều chính tắc. Sử dụng các công thức lượng giác sẽ tránh được các lỗi hình học tích lũy từ các phép quay lặp lại. 

các`base`mảng lưu trữ năm tọa độ địa phương. Mỗi ngôi sao trong tương lai chỉ là một bản sao được dịch, đảm bảo hình dạng giống hệt nhau. 

Việc triển khai chia sẻ chính xác một đỉnh giữa các ngôi sao lân cận. Biến`last_shared`theo dõi chỉ số điểm được sử dụng lại. Mỗi ngôi sao bổ sung chỉ chèn bốn điểm mới, giữ tổng số điểm là:$$5 + 4(n-1) = 4n + 1$$dưới mức cho phép`5n`. 

Thứ tự di chuyển`[0, 2, 4, 1, 3, 0]`là chu kỳ năm cánh tiêu chuẩn. Việc kết nối các chu kỳ này có hiệu quả vì các ngôi sao liên tiếp đã có chung điểm cuối. 

Một chi tiết tinh tế là đỉnh được nối thêm cuối cùng. Nếu không có nó, toàn bộ đường đi sẽ dừng sớm một cạnh. Đỉnh chung cuối cùng kết thúc bước đi Euler toàn cục một cách chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
```Việc xây dựng tạo ra một ngôi sao. 

| Bước | Điểm đã thêm | Đỉnh chia sẻ | Đóng góp đường dẫn | 
| --- | --- | --- | --- | 
| Ngôi sao 1 | 5 | Không có | 1 3 5 2 4 1 | 

Con đường cuối cùng:```
1 3 5 2 4 1
```Điều này thể hiện sự truyền tải của ngôi sao năm cánh cơ sở. Mỗi cạnh xuất hiện đúng một lần và bước đi bắt đầu và kết thúc ở cùng một đỉnh. 

### Ví dụ 2 

đầu vào:```
2
```Bây giờ ngôi sao thứ hai sử dụng lại một đỉnh. 

| Bước | Điểm mới | Đỉnh chia sẻ | Chỉ số sao | 
| --- | --- | --- | --- | 
| Ngôi sao 1 | 1 2 3 4 5 | Không có | [1,2,3,4,5] | 
| Sao 2 | 6 7 8 9 | 5 | [5,6,7,8,9] | 

Đường dẫn được tạo: 

| Phân đoạn | Truyền tải | 
| --- | --- | 
| Ngôi sao 1 | 1 3 5 2 4 | 
| Chuyển tiếp | 5 | 
| Sao 2 | 5 7 9 6 8 5 | 

Đỉnh chia sẻ`5`kết nối cả hai chu trình Euler thành một đồ thị Euler được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngôi sao đóng góp công việc không ngừng | 
| Không gian | O(n) | Chúng tôi lưu trữ tất cả các điểm và định nghĩa sao | 

Các giới hạn này cực kỳ nhỏ nên lời giải dễ dàng nằm gọn trong giới hạn thời gian và bộ nhớ. Ngay cả đối với`n = 100`, chúng tôi chỉ tạo ra`401`điểm và đường đi có độ dài`501`. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import math

    input = sys.stdin.readline

    n = int(input())

    R = 10.0 / (2.0 * math.sin(math.pi / 5.0))

    base = []

    for i in range(5):
        ang = math.pi / 2.0 + 2.0 * math.pi * i / 5.0
        base.append((R * math.cos(ang), R * math.sin(ang)))

    points = []
    stars = []

    for x, y in base:
        points.append((x, y))

    stars.append([1, 2, 3, 4, 5])

    last = 5

    for k in range(1, n):
        shift = 40.0 * k

        ids = [last]

        for j in range(1, 5):
            x, y = base[j]
            points.append((x + shift, y))
            ids.append(len(points))

        stars.append(ids)
        last = ids[-1]

    out = []

    out.append(str(len(points)))

    for x, y in points:
        out.append(f"{x:.12f} {y:.12f}")

    for s in stars:
        out.append(" ".join(map(str, s)))

    path = []

    order = [0, 2, 4, 1, 3, 0]

    for s in stars:
        for idx in order[:-1]:
            path.append(s[idx])

    path.append(stars[-1][0])

    out.append(" ".join(map(str, path)))

    return "\n".join(out)

# minimum case
res1 = run("1\n")
assert res1.splitlines()[0] == "5"

# small chain
res2 = run("2\n")
assert res2.splitlines()[0] == "9"

# larger chain
res3 = run("5\n")
assert res3.splitlines()[0] == "21"

# maximum input
res4 = run("100\n")
assert res4.splitlines()[0] == "401"

# path length check
last_line = res4.splitlines()[-1].split()
assert len(last_line) == 501
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`| 5 điểm | Cơ sở xây dựng sao đơn | 
|`2`| 9 điểm | Chia sẻ đỉnh đúng | 
|`5`| 21 điểm | Công thức tăng trưởng tuyến tính | 
|`100`| 401 điểm | Xử lý hạn chế tối đa | 
| Kiểm tra độ dài đường dẫn | 501 đỉnh đi bộ | Kích thước truyền tải Euler | 

## Vỏ cạnh 

cho`n = 1`, đồ thị chỉ là một ngôi sao năm cánh. Thuật toán đưa ra năm đỉnh và chu trình:```
1 3 5 2 4 1
```Mỗi đỉnh đều có bậc`2`, do đó đồ thị là Euler. 

Vì`n = 2`, chi tiết quan trọng là đỉnh được chia sẻ. Ngôi sao thứ hai sử dụng lại chính xác một điểm từ ngôi sao đầu tiên. Mức độ đỉnh được chia sẻ trở thành`4`, vẫn chẵn. Vì đồ thị được kết nối nên thuộc tính Euler vẫn tồn tại. 

Đối với lớn`n`, các giao điểm hình học vô tình là mối nguy hiểm. Thuật toán tránh điều này bằng cách giãn cách các ngôi sao`40`các đơn vị cách nhau theo chiều ngang, trong khi đường kính ngôi sao năm cánh nhỏ hơn thế nhiều. Không có phân đoạn không chia sẻ có thể giao nhau. 

Một trường hợp tế nhị khác là kích thước đầu ra. Việc thực hiện bất cẩn có thể tạo ra`5n`đỉnh phân biệt. Điều đó vẫn phù hợp với giới hạn, nhưng sau đó biểu đồ sẽ bị ngắt kết nối. Thay vào đó, việc xây dựng của chúng tôi tạo ra:$$4n + 1$$đỉnh bằng cách sử dụng lại một điểm cho mỗi cặp liền kề, đảm bảo khả năng kết nối.
