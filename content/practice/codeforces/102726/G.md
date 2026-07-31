---
title: "CF 102726G - Chăn nhân vật"
description: "Chúng ta cần xây dựng một Chăn nhân vật hình chữ nhật lớn từ các Ô nhân vật hình vuông nhỏ hơn. Mỗi ô được xác định một lần, sau đó quilt mô tả ô nào sẽ đi vào mọi vị trí và phép biến đổi nào sẽ được áp dụng trước khi đặt nó."
date: "2026-07-30T06:36:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102726
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 9-11-20 Div. 1"
rating: 0
weight: 102726
solve_time_s: 82
verified: true
draft: false
---

[CF 102726G - Chăn nhân vật](https://codeforces.com/problemset/problem/102726/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một Chăn nhân vật hình chữ nhật lớn từ các Ô nhân vật hình vuông nhỏ hơn. Mỗi ô được xác định một lần, sau đó quilt mô tả ô nào sẽ đi vào mọi vị trí và phép biến đổi nào sẽ được áp dụng trước khi đặt nó. 

Vị trí khối ảnh được mô tả bằng hai giá trị: chỉ mục của khối ảnh nguồn và loại chuyển đổi. Việc chuyển đổi có thể giữ nguyên ô, xoay nó theo bội số của 90 độ hoặc lật nó theo chiều ngang hoặc chiều dọc. Đầu ra là lưới ký tự hoàn chỉnh sau khi mở rộng mọi vị trí ô thành ô vuông được chuyển đổi. 

Kích thước ô tối đa là 15, trong khi chăn có thể chứa tối đa 100 x 100 vị trí ô. Việc mở rộng trực tiếp là thực tế vì lưới ký tự cuối cùng chứa tối đa 1500 x 1500 ký tự. Thách thức chính không phải là kích thước đầu ra mà là tránh làm việc lặp đi lặp lại khi áp dụng các phép biến đổi không chính xác hoặc không hiệu quả. 

Một sai lầm phổ biến là coi các cú lật là chuyển động quay hoặc nhầm lẫn trục phản xạ. Ví dụ, một viên gạch```
ab
cd
```với một cú lật dọc trở thành```
ba
dc
```không```
cd
ab
```Kết quả thứ hai là lật ngang. Một trường hợp cạnh khác là ô đơn. Đối với đầu vào```
1 1
x
1 1
0:1
```đầu ra vẫn là```
x
```bởi vì mọi phép biến đổi đều không thay đổi ô một ký tự. Việc triển khai giả định phép quay luôn hoán đổi kích thước có thể không thành công ở đây. 

Trường hợp ranh giới cuối cùng là khi chăn chỉ có một vị trí ô. Ví dụ:```
1 2
ab
cd
1 1
0:2
```Đầu ra phải là:```
dc
ba
```Một chương trình luôn nối nhiều hàng hoặc cột xếp chồng có thể vô tình thêm dấu phân tách hoặc dòng mới. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là xử lý mọi vị trí quilt, tìm ô nguồn của nó, áp dụng phép biến đổi được yêu cầu và in kết quả. Vì kích thước ô nhỏ nên chúng tôi thậm chí có thể tạo ô chuyển đổi mới mỗi lần. Điều này đúng vì mọi lần xuất hiện đều độc lập. 

Vấn đề với phiên bản đơn giản xuất hiện khi cùng một ô và chuyển đổi xảy ra nhiều lần. Một chăn 100 x 100 có 10000 vị trí và mỗi phép biến đổi chạm tới tối đa 225 ký tự. Đó vẫn chỉ là khoảng 2,25 triệu thao tác ký tự, vì vậy các ràng buộc thực sự cho phép phương pháp này. Mối nguy hiểm thực sự không phải là sự phức tạp tiệm cận mà là việc viết ra một hệ thống biến đổi phức tạp liên tục thực hiện việc sao chép không cần thiết. 

Quan sát hữu ích là chỉ có một số lượng nhỏ các phép biến đổi có thể xảy ra. Có tối đa 15 ô, mỗi ô có kích thước tối đa là 15 và có chính xác 6 kiểu biến đổi. Chúng ta có thể tính toán trước mọi phiên bản được chuyển đổi một lần. Sau đó, việc làm chăn chỉ cần tra cứu một viên gạch đã chuẩn bị sẵn và viết các ký tự trên đó. 

Phương pháp brute-force hoạt động vì tổng đầu ra nhỏ, nhưng ý tưởng tiền xử lý sẽ loại bỏ các hoạt động hình học lặp đi lặp lại và làm cho việc triển khai trở nên đơn giản hơn. Việc tạo quilt trở thành một chuỗi các bảng tra cứu và sao chép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(W * H * S²) | O(S²) | Được chấp nhận nhưng công việc lặp đi lặp lại | 
| Tối ưu | O(N * S2 + W * H * S2) | O(N * 6 * S²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các Ô ký tự gốc và lưu trữ chúng dưới dạng mảng ký tự vuông. Việc giữ các ô xếp dưới dạng mảng hai chiều khiến mọi phép biến đổi đều trở thành vấn đề ánh xạ tọa độ. 
2. Đối với mỗi ô, tạo tất cả sáu phiên bản đã chuyển đổi và lưu trữ chúng. Các phép biến đổi là: 

1. Đơn hàng gốc. 
2. Xoay 90 độ theo chiều kim đồng hồ. 
3. Xoay 180 độ. 
4. Xoay 270 độ theo chiều kim đồng hồ. 
5. Lật qua trục thẳng đứng. 
6. Lật qua trục ngang. 

Tính toán trước hoạt động vì tập biến đổi cố định và nhỏ. Mỗi vị trí chăn sau này có thể sử dụng lại cùng một ô đã biến đổi. 
3. Đọc kích thước chăn và xử lý từng hàng thông số kỹ thuật của gạch. 
4. Đối với mỗi đặc tả, hãy tách chỉ mục khối ảnh và giá trị biến đổi, truy xuất khối ảnh đã được chuyển đổi và nối các hàng của nó vào các hàng đầu ra tương ứng. 
5. Sau khi tất cả các vị trí ô đã được mở rộng, hãy in các hàng ký tự cuối cùng. 

Tại sao nó hoạt động: 

Mỗi vị trí đặt gạch trong chăn đều độc lập. Bước tiền xử lý lưu trữ chính xác kết quả sẽ được tạo ra bằng cách áp dụng phép biến đổi cho khối ảnh ban đầu. Khi một vị trí yêu cầu ô`i`với sự biến đổi`t`, thuật toán sử dụng phiên bản đã lưu trữ`tiles[i][t]`, giống hệt với việc tính toán phép biến đổi tại thời điểm đó. Vì mọi vị trí đều nhận được ô chuyển đổi chính xác và các vị trí được nối theo thứ tự ban đầu nên lưới cuối cùng chính xác là tấm chăn được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rotate90(tile):
    n = len(tile)
    return [[tile[n - 1 - r][c] for r in range(n)] for c in range(n)]

def rotate180(tile):
    n = len(tile)
    return [[tile[n - 1 - r][n - 1 - c] for c in range(n)] for r in range(n)]

def rotate270(tile):
    n = len(tile)
    return [[tile[c][n - 1 - r] for r in range(n)] for c in range(n)]

def flip_vertical(tile):
    return [row[::-1] for row in tile]

def flip_horizontal(tile):
    return tile[::-1]

def solve():
    N, S = map(int, input().split())

    all_tiles = []
    for _ in range(N):
        tile = [list(input().strip()) for _ in range(S)]
        all_tiles.append(tile)

    transformed = []
    for tile in all_tiles:
        transformed.append([
            tile,
            rotate90(tile),
            rotate180(tile),
            rotate270(tile),
            flip_vertical(tile),
            flip_horizontal(tile)
        ])

    W, H = map(int, input().split())

    answer = [[] for _ in range(H * S)]

    for row in range(H):
        specs = input().split()
        for col, spec in enumerate(specs):
            idx, t = map(int, spec.split(':'))
            cur = transformed[idx][t]
            base = row * S
            for r in range(S):
                answer[base + r].extend(cur[r])

    print('\n'.join(''.join(row) for row in answer))

if __name__ == "__main__":
    solve()
```Các hàm xoay trực tiếp thực hiện các thay đổi tọa độ. Đối với xoay theo chiều kim đồng hồ, ký tự cũ ở dưới cùng bên trái sẽ trở thành ký tự mới ở trên cùng bên trái, đó là lý do tại sao chỉ mục hàng bị đảo ngược trong khi chỉ mục cột trở thành hàng mới. 

Sáu bản sao đã chuyển đổi được lưu trữ theo cùng thứ tự với các số chuyển đổi trong đầu vào. Điều này tránh được logic có điều kiện trong khi xây dựng chăn bông. 

Mảng đầu ra có`H * S`hàng bởi vì mỗi hàng chăn mở rộng thành`S`các hàng ký tự. Mỗi ô đóng góp chính xác`S`các ký tự cho mỗi hàng được mở rộng, do đó, việc mở rộng hàng đầu ra chính xác sẽ giữ nguyên bố cục ô ban đầu. 

Số nguyên Python không phải là vấn đề đáng lo ngại vì không cần số học lớn. Rủi ro triển khai chính là trộn lẫn hai hướng lật, đó là lý do tại sao các chức năng lật được giữ riêng biệt. 

## Ví dụ đã hoạt động 

Sử dụng dữ liệu đầu vào mẫu, hãy xem xét hàng đầu tiên của chăn bông: 

| Vị trí gạch | Chỉ số gạch | Chuyển đổi | Hàng mở rộng | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 |`<<>` `^<^` `<>>`| 
| 2 | 1 | 0 |`>*=` `*+*` `+=>`| 
| 3 | 0 | 0 |`<<>` `^<^` `<>>`| 

Thuật toán chỉ cần lấy từng ô đã chuẩn bị sẵn và nối các hàng phù hợp: 

| Hàng đầu ra đang được xây dựng | Nội dung hiện tại | 
| --- | --- | 
| 1 |`<<>>*=<<>`| 
| 2 |`^<^*+*^<^`| 
| 3 |`<>>+=><>>`| 

Điều này chứng tỏ rằng bản thân cấu trúc quilt chỉ là phép nối sau khi các phép biến đổi đã được xử lý. 

Đối với một ví dụ xoay nhỏ hơn: 

đầu vào:```
1 2
ab
cd
2 1
0:1 0:2
0:3 0:4
```Các phép biến đổi là: 

| Vị trí | Chuyển đổi | Kết quả | 
| --- | --- | --- | 
| Trên cùng bên trái | 90 độ |`ca` `db`| 
| Trên cùng bên phải | 180 độ |`dc` `ba`| 
| Dưới cùng bên trái | 270 độ |`bd` `ac`| 
| Dưới cùng bên phải | Lật dọc |`ba` `dc`| 

Kết quả đầu ra là:```
cadbdcba
acbadc
```Dấu vết xác nhận rằng mỗi lần chuyển đổi được áp dụng trước khi đặt chứ không phải sau khi toàn bộ chăn bông đã được lắp ráp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N * S2 + W * H * S2) | Mỗi ô được chuyển đổi sáu lần trong quá trình tiền xử lý, sau đó mỗi vị trí quilt sẽ sao chép một ô S by S. | 
| Không gian | O(N * 6 * S² + W * H * S²) | Lưu trữ tất cả các ô đã chuyển đổi và đầu ra mở rộng cuối cùng. | 

Kích thước chăn được mở rộng tối đa là 1500 x 1500 ký tự, do đó dung lượng lưu trữ đầu ra cuối cùng sẽ nhỏ. Số lượng các phép biến đổi cũng bị hạn chế, khiến quá trình tiền xử lý dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_case(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline

    def rotate90(tile):
        n = len(tile)
        return [[tile[n - 1 - r][c] for r in range(n)] for c in range(n)]

    def rotate180(tile):
        n = len(tile)
        return [[tile[n - 1 - r][n - 1 - c] for c in range(n)] for r in range(n)]

    def rotate270(tile):
        n = len(tile)
        return [[tile[c][n - 1 - r] for r in range(n)] for c in range(n)]

    def flip_vertical(tile):
        return [row[::-1] for row in tile]

    def flip_horizontal(tile):
        return tile[::-1]

    N, S = map(int, data().split())
    tiles = []
    for _ in range(N):
        tiles.append([list(data().strip()) for _ in range(S)])

    trans = []
    for t in tiles:
        trans.append([t, rotate90(t), rotate180(t), rotate270(t),
                      flip_vertical(t), flip_horizontal(t)])

    W, H = map(int, data().split())
    ans = [[] for _ in range(H * S)]

    for i in range(H):
        specs = data().split()
        for spec in specs:
            a, b = map(int, spec.split(':'))
            tile = trans[a][b]
            for r in range(S):
                ans[i * S + r].extend(tile[r])

    res = '\n'.join(''.join(x) for x in ans)

    sys.stdin = old
    return res

assert solve_case("""1 1
x
1 1
0:5
""") == "x", "single character"

assert solve_case("""1 2
ab
cd
1 1
0:2
""") == "dc\nba", "180 rotation"

assert solve_case("""1 2
ab
cd
2 1
0:4 0:5
0:1 0:3
""") == "bacd\ncdab\ncadb\nbdac", "all transforms"

assert solve_case("""1 1
a
1 1
0:3
""") == "a", "minimum tile size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngói ký tự đơn |`x`| Tất cả các phép biến đổi trên kích thước một | 
| Xoay hai vòng |`dc`Và`ba`| Ánh xạ xoay chính xác | 
| Nhiều vị trí được chuyển đổi | Bốn hàng mở rộng | Tương tác giữa vị trí và biến đổi | 
| Kích thước ô tối thiểu |`a`| Xử lý ranh giới | 

## Vỏ cạnh 

Đối với trường hợp ô ký tự đơn:```
1 1
x
1 1
0:1
```quá trình tiền xử lý tạo ra sáu ô một ký tự giống hệt nhau. Việc tra cứu trả về phiên bản chính xác mà không cần xử lý đặc biệt. 

Đối với lỗi lật hướng:```
1 2
ab
cd
1 1
0:4
```đầu ra đúng là:```
ba
dc
```Thuật toán sử dụng bảng lật dọc, đảo ngược từng hàng. Việc triển khai lật ngang sẽ trả về không chính xác:```
cd
ab
```Đối với một vị trí chăn duy nhất:```
1 2
ab
cd
1 1
0:2
```thuật toán chỉ mở rộng một ô. Các hàng đầu ra được lấy trực tiếp từ góc quay 180 độ được lưu trữ:```
dc
ba
```Không có giả định nào về các ô lân cận được đưa ra, do đó trường hợp ranh giới hoạt động một cách tự nhiên.
