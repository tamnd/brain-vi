---
title: "CF 102348K - Hướng về mặt trăng"
description: "Chúng ta cần xây dựng một bàn cờ (n lần n), trong đó ô ((i,j)) phải chứa đá nếu (i+j) chẵn và nếu không là cát. Giá trị (n) là chẵn và nhiều nhất là (50). Khó khăn không phải là chọn màu cuối cùng. Khó khăn là thứ tự các ô có thể được tiếp cận."
date: "2026-08-13T01:15:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102348
codeforces_index: "K"
codeforces_contest_name: "ICPC 2019-2020 NERC (NEERC), Southern and Volga Russia Qualifier"
rating: 0
weight: 102348
solve_time_s: 158
verified: true
draft: false
---

[CF 102348K - Lên mặt trăng](https://codeforces.com/problemset/problem/102348/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một bàn cờ (n \times n), trong đó ô ((i,j)) phải chứa đá nếu (i+j) chẵn và nếu không thì là cát. Giá trị (n) là chẵn và nhiều nhất là (50). 

Khó khăn không phải là chọn màu cuối cùng. Khó khăn là thứ tự các ô có thể được tiếp cận. Một ô trống chỉ có thể được điền riêng lẻ khi nó nằm trên đường viền hoặc chạm vào một ô đã được điền. Phép toán (2 \times 2) mạnh hơn, nhưng mọi ô hiện trống bên trong ô vuông đó đều nhận được cùng một màu. Vì mỗi bàn cờ (2 \times 2) có hai ô đá và hai ô cát nên phép tính hình vuông chỉ có thể được sử dụng sau khi các ô cùng màu bên trong ô vuông đó đã được lấp đầy. 

Đầu ra là một chuỗi các hoạt động hợp lệ. Mọi thao tác chỉ được đặt đúng màu, mọi vị trí được chọn phải đáp ứng điều kiện tiếp cận bắt buộc và tổng số thao tác không được vượt quá (3n^2/4). 

Ràng buộc (n \le 50) có nghĩa là ngay cả cấu trúc trực tiếp (O(n^2)) cũng rất nhỏ về mặt tính toán, vì có nhiều nhất (2500) ô. Hạn chế thực sự là số lần sử dụng thao tác. Việc điền từng ô riêng lẻ sẽ yêu cầu (n^2) thao tác, luôn lớn hơn mức cho phép (3n^2/4). Do đó, chúng ta cần khai thác thao tác (2 \times 2) thay vì chỉ tìm phép duyệt hợp lệ trên tất cả các ô. 

Thực tế là (n) chẵn chính xác là cái cho phép chúng ta chia toàn bộ bức tường thành các ô vuông rời rạc (2 \times 2). Có (n^2/4) các ô vuông như vậy và nếu mỗi ô vuông có thể được hoàn thành sau ba thao tác thì tổng số chính xác là (3n^2/4), khớp với giới hạn. 

Có hai trường hợp nhỏ thường phá vỡ các công trình xây dựng bất cẩn. Với (n=2), toàn bộ bức tường là một (2 \times 2) hình vuông. Một thao tác trực tiếp (2 \times 2) không thể là thao tác đầu tiên, vì cả bốn ô đều trống và nó sẽ tô chúng bằng một màu, tạo ra các ô không chính xác. Ví dụ, với đầu vào`2`, một cấu trúc hợp lệ bao gồm ba thao tác: điền riêng hai ô của một đường chéo, sau đó sử dụng một thao tác hình vuông cho đường chéo còn lại. 

Đối với một hình vuông bên trong (2 \times 2), chỉ cần điền vào một ô và sau đó cố gắng điền vào đối tác đường chéo cùng màu của nó cũng không thành công. Hai ô đó nằm cạnh nhau theo đường chéo, do đó ô thứ hai không được giải phóng bằng thao tác đầu tiên. Công trình xây dựng phải chọn hai ô giống sao cho mỗi ô đều nằm trên ranh giới của khu vực được xây dựng. 

## Phương pháp tiếp cận 

Cách xây dựng trực tiếp nhất là lấp đầy từng ô một cách độc lập. Một ô có thể được chọn khi nó có thể truy cập được và việc xử lý bức tường từ đường viền của nó hướng vào bên trong sẽ đưa ra một thứ tự hợp lệ. Màu sắc cuối cùng rõ ràng là chính xác vì mọi thao tác đều chọn được màu yêu cầu. Tuy nhiên, điều này sử dụng chính xác (n^2) các phép toán trong trường hợp xấu nhất, trong khi vấn đề chỉ cho phép (3n^2/4). Đối với (n=50), đó là (2500) hoạt động thay vì mức tối đa được phép (1875). Vấn đề là ngân sách hoạt động chứ không phải là thời gian chạy tính toán. 

Quan sát quan trọng là bức tường có thể được chia thành (n^2/4) hình vuông rời rạc (2 \times 2). Xét một hình vuông có góc trên bên trái ((x,y)), trong đó cả (x) và (y) đều là số lẻ. Tế bào của nó 

[ 
(x,y),\quad (x,y+1),\quad (x+1,y),\quad (x+1,y+1). 
] 

Các ô ((x,y+1)) và ((x+1,y)) có cùng màu. Quan trọng hơn, khi (2 \times 2) hình vuông được xử lý từ trên xuống dưới và từ trái sang phải, cả hai ô này đều có thể truy cập được trước khi chúng ta xử lý hình vuông của chúng. Đầu tiên là ở đường viền trên cùng hoặc chạm vào hình vuông đã được xây dựng hoàn chỉnh ở trên. Cái thứ hai nằm ở viền trái hoặc chạm vào hình vuông đã được xây dựng hoàn chỉnh ở bên trái. 

Chúng ta có thể lấp đầy hai ô cùng màu đó bằng hai thao tác đơn lẻ. Khi đó, hai ô còn lại trong hình vuông có màu đối lập nhau, do đó, một thao tác (2 \times 2) với màu đối diện đó sẽ lấp đầy chính xác hai ô trống đó. Không có ô nào được sơn sai. 

Như vậy mỗi (2 \times 2) bình phương có đúng ba phép tính. Vì có (n^2/4) ô vuông nên tổng số là 

[ 
3 \cdot \frac{n^2}{4}=\frac{3n^2}{4}, 
] 

đó chính xác là mức tối đa cho phép. 

Cấu trúc brute-force hoạt động hiệu quả vì các hoạt động riêng lẻ cung cấp quyền kiểm soát hoàn toàn cho mọi ô, nhưng nó chỉ dành một thao tác cho mỗi ô. Nhận xét rằng mọi hình vuông trên bàn cờ (2 \times 2) bao gồm hai đường chéo cùng màu cho phép chúng ta thay thế bốn phép tính đơn bằng hai hạt đơn và một phép tính hình vuông. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Điền vào từng ô riêng lẻ | (O(n^2)) | (O(n^2)) | Hợp lệ nhưng vượt quá giới hạn hoạt động | 
| Phân chia thành (2 \times 2) ô vuông | (O(n^2)) | (O(n^2)) cho các hoạt động được lưu trữ | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chia bức tường thành các ô vuông rời rạc (2 \time 2) có góc trên bên trái là ((x,y)), trong đó (x) và (y) lấy các giá trị lẻ (1,3,5,\ldots,n-1). Mỗi ô trên tường thuộc về đúng một ô vuông như vậy. 
2. Xử lý các ô vuông này theo thứ tự hàng tăng dần và bên trong mỗi hàng, thứ tự cột tăng dần. Lệnh này đảm bảo rằng mọi hình vuông ngoại trừ những hình vuông chạm vào đường viền bên ngoài đều có một hình vuông đã hoàn thành ngay phía trên nó hoặc ngay bên trái nó. 
3. Đối với hình vuông hiện tại, trước tiên hãy điền riêng ((x,y+1)). Màu yêu cầu của nó là cát, vì (x+y+1) là số lẻ. Ô này trống vì (x=1) đặt nó ở viền trên cùng, trong khi (x>1) đặt nó ở cạnh được lấp đầy ở trên. 
4. Đổ cát vào ((x+1,y)) riêng lẻ. Nếu (y=1), nó nằm ở viền trái. Ngược lại, hình vuông ngay bên trái của nó đã được hoàn thành, do đó ((x+1,y)) có một ô lân cận được lấp đầy. 
5. Sử dụng phép toán (2 \times 2) tại ((x,y)) với hòn đá. Hai ô cát được đặt trong các thao tác trước đã bị chiếm dụng, trong khi hai ô còn lại, ((x,y)) và ((x+1,y+1)), chính xác là hai ô đá của hình vuông bàn cờ này. Do đó thao tác chỉ điền vào các ô chính xác. 
6. Tiếp tục cho đến khi mọi (2 \times 2) ô vuông được xử lý. Có (n/2) lựa chọn của (x) và (n/2) lựa chọn của (y), do đó có (n^2/4) bình phương và có chính xác ba phép tính trên mỗi bình phương. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý một hình vuông, mọi hình vuông được xử lý trước đó đều hoàn toàn chính xác và hình vuông hiện tại vẫn trống. Hạt giống đầu tiên ((x,y+1)) luôn miễn phí vì nó nằm ở đường viền trên cùng hoặc chạm vào hình vuông đã hoàn thành ở trên. Hạt giống thứ hai ((x+1,y)) luôn trống vì nó nằm ở viền bên trái hoặc chạm vào hình vuông đã hoàn thành ở bên trái. Cả hai hạt đều là tế bào cát. Sau khi chúng được đặt, ô trống duy nhất trong hình vuông là hai ô đá, do đó thao tác cuối cùng (2 \times 2) có thể sử dụng đá một cách an toàn. Do đó, mọi thao tác đều hợp lệ và mọi ô bị chiếm đều có màu được yêu cầu. Vì các hình vuông rời rạc và bao phủ toàn bộ bức tường nên bức tường cuối cùng chính xác là bàn cờ cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = []

    for x in range(1, n, 2):
        for y in range(1, n, 2):
            # (x, y + 1) and (x + 1, y) are both sand cells.
            ans.append((1, x, y + 1, 2))
            ans.append((1, x + 1, y, 2))

            # The remaining two cells of this 2x2 square are stone.
            ans.append((2, x, y, 1))

    print(len(ans))
    for t, x, y, b in ans:
        print(t, x, y, b)

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài chọn hàng trên cùng của mỗi khối (2 \times 2). Bởi vì nó tăng lên hai, nên nó truy cập chính xác các hàng (1,3,\ldots,n-1). Vòng lặp bên trong thực hiện tương tự với các cột. 

Mục tiêu hoạt động đơn lẻ đầu tiên ((x,y+1)). Tính chẵn lẻ của nó rất lẻ nên chắc hẳn là cát. Mục tiêu thứ hai ((x+1,y)), có cùng tính chẵn lẻ và do đó cũng cần cát. 

Phép toán bình phương bắt đầu tại ((x,y)). Bốn ô của nó nằm bên trong bức tường vì (x,y \le n-1). Hai ô cát đã bị chiếm dụng nên thao tác chỉ lấp đầy hai ô đá trống. Chọn đá làm màu vuông chính là điều giúp cho quá trình vận hành được an toàn. 

Không cần mô phỏng bức tường. Bản thân trình tự xây dựng chứng minh rằng các ô được yêu cầu là miễn phí. Số nguyên Python cũng không cần xử lý đặc biệt vì tất cả tọa độ và số phép toán nhiều nhất là vài nghìn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đối với đầu vào được cung cấp (n=2), chỉ có một hình vuông (2 \times 2). 

| Hình vuông ((x,y)) | Hạt giống đầu tiên | Hạt giống thứ hai | Hoạt động vuông | Hoạt động được sử dụng | 
| --- | --- | --- | --- | --- | 
| ((1,1)) | ((1,2)), cát | ((2,1)), cát | ((1,1)), đá | 3 | 

Bức tường kết quả là 

| Vị trí | ((1,1)) | ((1,2)) | 
| --- | --- | --- | 
| Hàng 1 | đá | cát | 
| Hàng 2 | cát | đá | 

Mẫu sử dụng một thứ tự khác nhưng có giá trị như nhau, cụ thể là hai thao tác đơn đá theo sau là một thao tác cát vuông. Vấn đề chấp nhận mọi cách xây dựng hợp lệ trong giới hạn hoạt động. Công trình của chúng tôi cũng sử dụng chính xác các phép toán (3=3\cdot2^2/4). 

### Ví dụ 2 

Với (n=4), có bốn hình vuông rời nhau (2 \times 2). 

| Hình vuông ((x,y)) | Hạt giống đầu tiên | Hạt giống thứ hai | Hoạt động vuông cuối cùng | Tổng số hoạt động | 
| --- | --- | --- | --- | --- | 
| ((1,1)) | ((1,2)), cát | ((2,1)), cát | đá | 3 | 
| ((1,3)) | ((1,4)), cát | ((2,3)), cát | đá | 3 | 
| ((3,1)) | ((3,2)), cát | ((4,1)), cát | đá | 3 | 
| ((3,3)) | ((3,4)), cát | ((4,3)), cát | đá | 3 | 

Sau khi ô vuông đầu tiên hoàn thành, ô ((2,3)) ở ô vuông thứ hai liền kề với ô vuông đã hoàn thành ở bên trái của nó. Ô ((3,2)) ở hình vuông thứ ba cũng liền kề với ô vuông đã hoàn thành ở trên. Đây chính xác là lý do tại sao việc xử lý hàng chính lại hiệu quả đối với các ô vuông bên trong. 

Số cuối cùng là (4\cdot3=12), trong khi 

[ 
\frac{3n^2}{4}=\frac{3\cdot16}{4}=12. 
] 

Do đó, ví dụ đạt đến giới hạn hoạt động một cách chính xác mà không vượt quá nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Có (n^2/4) ô vuông và mỗi ô tạo ra ba phép tính. | 
| Không gian | (O(n^2)) | Các thao tác đầu ra được lưu trữ trước khi in, với tối đa (3n^2/4) thao tác. | 

Với (n\le50), có nhiều nhất (2500) ô và (1875) thao tác được tạo. Việc xây dựng chỉ thực hiện một lượng công việc không đổi trên mỗi ô, do đó, nó dễ dàng nằm trong giới hạn thời gian một giây và thấp hơn nhiều so với giới hạn bộ nhớ. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên các thử nghiệm sẽ xác nhận chuỗi thao tác được tạo thay vì so sánh từng ký tự với một đầu ra hợp lệ cụ thể.```python
# This test harness validates the construction itself.
# It can be used with the solve() implementation above.

import sys
import io

def build(n):
    ans = []

    for x in range(1, n, 2):
        for y in range(1, n, 2):
            ans.append((1, x, y + 1, 2))
            ans.append((1, x + 1, y, 2))
            ans.append((2, x, y, 1))

    out = [str(len(ans))]
    out.extend(f"{t} {x} {y} {b}" for t, x, y, b in ans)
    return "\n".join(out) + "\n"

def validate(inp: str) -> str:
    n = int(inp.strip())
    output = build(n)

    data = list(map(int, output.split()))
    k = data[0]
    assert 1 <= k <= 3 * n * n // 4

    board = [[0] * n for _ in range(n)]
    p = 1

    def wanted(i, j):
        # i and j are zero-based here.
        return 1 if (i + j) % 2 == 0 else 2

    def free(i, j):
        if board[i][j] != 0:
            return False

        if i == 0 or i == n - 1 or j == 0 or j == n - 1:
            return True

        return (
            board[i - 1][j] != 0
            or board[i + 1][j] != 0
            or board[i][j - 1] != 0
            or board[i][j + 1] != 0
        )

    for _ in range(k):
        t, x, y, b = data[p:p + 4]
        p += 4

        x -= 1
        y -= 1

        if t == 1:
            assert 0 <= x < n
            assert 0 <= y < n
            assert free(x, y)
            assert wanted(x, y) == b
            board[x][y] = b

        else:
            assert t == 2
            assert 0 <= x < n - 1
            assert 0 <= y < n - 1

            cells = [
                (x, y),
                (x, y + 1),
                (x + 1, y),
                (x + 1, y + 1),
            ]

            assert any(free(i, j) for i, j in cells)

            for i, j in cells:
                if board[i][j] == 0:
                    assert wanted(i, j) == b

            for i, j in cells:
                if board[i][j] == 0:
                    board[i][j] = b

    for i in range(n):
        for j in range(n):
            assert board[i][j] == wanted(i, j)

    return output

# Provided sample.
sample1 = validate("2")
assert sample1.startswith("3\n"), "sample 1"

# Minimum size, exercising the single 2x2 square.
case2 = validate("2")
assert case2.splitlines()[0] == "3"

# A small interior case, where reachability depends on previously
# completed squares.
case3 = validate("4")
assert case3.splitlines()[0] == "12"

# Another even size, catching row and column progression errors.
case4 = validate("6")
assert case4.splitlines()[0] == "27"

# Maximum allowed n.
case5 = validate("50")
assert case5.splitlines()[0] == str(3 * 50 * 50 // 4)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| 3 thao tác | Kích thước tối thiểu và thực tế là hình vuông đầu tiên cần có hai hạt giống trước khi hoạt động hình vuông | 
|`4`| 12 hoạt động | Hình vuông bên trong và lan truyền từ trên xuống bên trái | 
|`6`| 27 hoạt động | Chuyển đổi hàng và cột lặp đi lặp lại | 
|`50`| Hoạt động 1875 | Kích thước tối đa và giới hạn hoạt động chính xác | 

## Vỏ cạnh 

Với (n=2), đầu vào là```
2
```Hình vuông duy nhất là ((1,1)). Thuật toán đầu tiên lấp đầy ((1,2)) bằng cát và sau đó ((2,1)) bằng cát. Cả hai đều là ô viền, vì vậy cả hai phép toán đơn lẻ đều hợp lệ. Phép toán cuối cùng (2 \times 2) sẽ lấp đầy ((1,1)) và ((2,2)) bằng đá. Đầu ra sử dụng chính xác ba thao tác. 

Đối với một hình vuông bên trong, hãy xem xét (n=4) và hình vuông bắt đầu từ ((3,3)). Trước khi đạt được nó, các ô vuông bắt đầu tại ((1,1)), ((1,3)) và ((3,1)) đã được hoàn thành. Ô ((3,4)) trống vì nó liền kề với ô vuông đã hoàn thành ở trên, trong khi ((4,3)) trống vì nó liền kề với ô vuông đã hoàn thành ở bên trái. Cả hai đều là ô cát nên có thể đặt riêng lẻ. Các ô còn lại ((3,3)) và ((4,4)) đều là đá và thao tác hình vuông cuối cùng sẽ lấp đầy chính xác các ô đó. 

Các trường hợp ranh giới trong đó (x=1) hoặc (y=1) được xử lý mà không cần mã đặc biệt. Nếu (x=1), hạt đầu tiên nằm ở viền trên. Nếu (y=1) thì hạt thứ hai nằm ở viền trái. Khi công trình di chuyển xa hơn vào trong, hình vuông đã hoàn thành trước đó tương ứng sẽ cung cấp khối lân cận cần thiết. 

Trường hợp tối đa (n=50) chứa (25) hàng khối và (25) cột khối, cho (625) hình vuông rời nhau (2 \times 2). Mỗi hình vuông cần ba phép tính, do đó, việc xây dựng tạo ra các phép tính (625\cdot3=1875), chính xác bằng (3n^2/4). Không có số lượng hoạt động hoặc tọa độ vượt quá giới hạn đã nêu.
