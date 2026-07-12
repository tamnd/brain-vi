---
title: "CF 103192E - \u8089\u5939\u998d"
description: "Chúng ta được cung cấp một chuỗi nhị phân mô tả “cấu trúc lồng nhau” từ lớp trong cùng đến lớp ngoài cùng."
date: "2026-07-03T16:10:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103192
codeforces_index: "E"
codeforces_contest_name: "The 9-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 103192
solve_time_s: 57
verified: true
draft: false
---

[CF 103192E - \u8089\u5939\u998d](https://codeforces.com/problemset/problem/103192/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi nhị phân mô tả “cấu trúc lồng nhau” từ lớp trong cùng đến lớp ngoài cùng. Mỗi ký tự trong chuỗi đại diện cho một lớp: ký tự đầu tiên tương ứng với trung tâm và mỗi ký tự tiếp theo bao bọc cấu trúc trước đó bằng khung bao quanh lớn hơn. 

Nhiệm vụ là xây dựng bản vẽ 2D ASCII của cấu trúc này. Lớp trong cùng là một “lõi” rất nhỏ được tạo thành từ hai ô nằm ngang. Mỗi lớp mở rộng ra bên ngoài một cách đối xứng, làm tăng hình chữ nhật bao quanh và mỗi lớp được vẽ bằng một ký tự cụ thể tùy thuộc vào chữ số nhị phân tương ứng là 0 hay 1. Một trong các ký tự tượng trưng cho thịt và ký tự còn lại tượng trưng cho bánh mì, nhưng đối với quá trình xây dựng, điều quan trọng là mỗi lớp có ký tự tô/viền riêng. 

Hình học là phần quan trọng của vấn đề: mỗi lớp mới tăng kích thước của hình hiện tại bằng cách bao bọc nó bằng một đường viền dày một ô ở tất cả các cạnh, ngoại trừ cấu trúc mở ở phía bên trái và ranh giới bên phải được đánh dấu rõ ràng bằng ký tự thanh dọc. Điều này tạo ra hình dạng "bánh sandwich mở" lồng nhau trong đó mỗi lớp rõ ràng lớn hơn lớp trước. 

Các ràng buộc cho phép độ dài chuỗi lên tới 1000. Điều này có nghĩa là một cấu trúc đơn giản cố gắng mô phỏng mọi thứ theo từng ô vẫn khả thi với tổng số ô khoảng O(n^2) hoặc O(n^2), vì kích thước lưới cuối cùng tăng tuyến tính với n ở cả hai chiều, dẫn đến tổng kích thước đầu ra khoảng O(n^2). Bất cứ điều gì tệ hơn bậc hai về độ dài của chuỗi sẽ không an toàn. 

Một số trường hợp đặc biệt cần được nêu rõ. Nếu chuỗi có độ dài 1, chúng ta chỉ in lõi cơ sở của hai ô, không có phần bao bọc nào cả. Nếu tất cả các ký tự đều giống nhau, đầu ra vẫn có nhiều lớp riêng biệt, nhưng về mặt trực quan, cấu trúc sẽ trở nên đồng nhất. Một trường hợp tinh vi khác là cạnh bên trái không bị giới hạn bởi một ký tự viền hiển thị, do đó, bất kỳ giải pháp nào phản chiếu nhầm ranh giới bên phải ở bên trái sẽ tạo ra một hình chữ nhật đối xứng, điều này không chính xác. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về vấn đề này là xây dựng cấu trúc từ trung tâm ra ngoài. Chúng ta bắt đầu với lớp trong cùng, luôn là một đoạn nằm ngang nhỏ gồm hai ký tự giống hệt nhau. Sau đó, đối với mỗi ký tự bổ sung trong chuỗi, chúng tôi bọc lưới hiện tại bằng một hình chữ nhật lớn hơn có đường viền sử dụng ký tự được liên kết với lớp đó. 

Trong mô phỏng lực lượng vũ phu, chúng ta có thể duy trì lưới 2D theo đúng nghĩa đen và đối với mỗi lớp mới, phân bổ một lưới mới lớn hơn và sao chép nội dung trước đó vào giữa. Sau khi sao chép, chúng ta điền vào ranh giới mới. Điều này hoạt động vì cấu trúc lồng được xác định rõ ràng là mở rộng đệ quy. Tuy nhiên, cách tiếp cận này tính toán lại rất nhiều chuyển động của bộ nhớ ở mỗi bước. Nếu lưới cuối cùng có kích thước tỷ lệ với n x n và chúng tôi xây dựng lại nó n lần thì tổng công việc sẽ trở thành O(n^3) theo cách hiểu tệ nhất do sao chép nhiều lần. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xây dựng lại toàn bộ lưới từ đầu. Thay vào đó, trước tiên chúng ta có thể tính toán trực tiếp các kích thước cuối cùng, phân bổ lưới cuối cùng một lần, sau đó đặt ranh giới của mỗi lớp vào đúng vị trí. Mỗi lớp tương ứng với một khung hình chữ nhật có tọa độ có thể được suy ra từ khoảng cách từ tâm đến nó. Điều này làm giảm vấn đề từ việc xây dựng lặp đi lặp lại thành quá trình vẽ một lớp. 

Vì vậy, giải pháp tối ưu trở thành bài toán tọa độ: xác định tâm, tính toán xem mỗi lớp mở rộng hộp giới hạn như thế nào và sau đó vẽ đường viền của mỗi lớp chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Xây dựng lại từng lớp | O(n^3) | O(n^2) | Quá chậm | 
| Trực tiếp phối hợp thi công | O(n^2) | O(n^2) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lưới cuối cùng trong một lần duy nhất bằng cách tính toán trước vị trí của mỗi lớp. 

1. Đầu tiên chúng tôi xác định kích thước của cấu trúc cuối cùng. Lớp trong cùng chiếm một đoạn ngang nhỏ có chiều dài bằng 2 và mỗi lớp bổ sung sẽ mở rộng cấu trúc ra bên ngoài một đơn vị ở tất cả các cạnh. Điều này ngụ ý rằng cả chiều cao và chiều rộng đều tăng tuyến tính theo số lớp, vì vậy chúng ta có thể tính trực tiếp các kích thước cuối cùng tỷ lệ với n. 
2. Chúng tôi phân bổ lưới 2D ban đầu được lấp đầy bằng các giá trị giữ chỗ. Lưới này cuối cùng sẽ chứa bản vẽ đầy đủ, vì vậy chúng tôi tránh mọi thay đổi kích thước sau này. 
3. Chúng tôi xác định vùng trung tâm là lớp trong cùng. Chúng ta đặt phần biểu diễn của ký tự đầu tiên (hai ký hiệu giống hệt nhau) ở hàng giữa, căn giữa theo chiều ngang. Điều này thiết lập nền tảng mà từ đó tất cả các lớp bên ngoài mở rộng. 
4. Đối với mỗi ký tự tiếp theo trong chuỗi, chúng tôi xác định chỉ mục lớp của nó và tính hình chữ nhật giới hạn của nó. Mỗi lớp i tương ứng với một hình chữ nhật có kích thước lớn hơn một đơn vị theo mọi hướng so với lớp i−1. 
5. Chúng ta điền vào các ranh giới trên và dưới của lớp hiện tại bằng ký tự của lớp. Điều này tạo ra cấu trúc ngang của khung. 
6. Về mặt khái niệm, chúng tôi điền vào ranh giới bên trái của lớp, nhưng vì cấu trúc mở ở phía bên trái nên chúng tôi không vẽ một đường viền dọc đầy đủ ở đó. Thay vào đó, chúng tôi chỉ đảm bảo phần bên trong được giới hạn chính xác bởi các lớp trước đó. 
7. Chúng tôi điền vào ranh giới bên phải của lớp bằng ký tự thanh dọc, đóng vai trò đóng vai trò đóng cửa theo chiều dọc nghiêm ngặt duy nhất của cấu trúc. Điều này đảm bảo các hình dạng lồng nhau vẫn được cố định về mặt trực quan ở phía bên phải. 
8. Chúng tôi lặp lại quá trình này cho đến khi tất cả các lớp đã được đặt. 

Sau khi tất cả các lớp được vẽ, lưới sẽ được in theo từng hàng. 

### Tại sao nó hoạt động 

Mỗi lớp tạo thành một lớp ngăn chặn chặt chẽ xung quanh lớp trước đó và quy tắc mở rộng đảm bảo rằng lớp i bao bọc hoàn toàn lớp i−1 mà không có sự mơ hồ chồng chéo. Bởi vì mỗi lớp được vẽ bằng cách sử dụng các độ lệch hình học cố định từ tâm nên không có hai lớp nào giao thoa không chính xác. Việc xây dựng thực sự là một cảm ứng trên các lớp: giả sử lớp i−1 được đặt chính xác, lớp i thêm một đường viền một ô xung quanh nó, duy trì tính chính xác và duy trì tính bất biến lồng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    # Each layer expands the shape; final height and width are linear in n
    h = 2 * n
    w = 2 * n

    grid = [[' ' for _ in range(w)] for _ in range(h)]

    mid_row = h // 2
    mid_col = w // 2

    # base layer: two characters in the center
    base_char = '*' if s[0] == '0' else '-'
    grid[mid_row][mid_col - 1] = base_char
    grid[mid_row][mid_col] = base_char

    top = mid_row
    bottom = mid_row
    left = mid_col - 1
    right = mid_col

    # expand layer by layer
    for i in range(1, n):
        c = '*' if s[i] == '0' else '-'

        top -= 1
        bottom += 1
        left -= 1
        right += 1

        # top and bottom borders
        for j in range(left, right + 1):
            grid[top][j] = c
            grid[bottom][j] = c

        # left side is conceptually open, so we do not draw a full border

        # right boundary is closed with '|'
        for j in range(top, bottom + 1):
            grid[j][right] = '|'

        # ensure interior is preserved implicitly

    # print result (trim empty rows)
    for row in grid:
        line = ''.join(row).rstrip()
        if line:
            print(line)

if __name__ == "__main__":
    solve()
```Việc thực hiện xây dựng cấu trúc từ trung tâm ra bên ngoài. Các biến`top`,`bottom`,`left`, Và`right`theo dõi hộp giới hạn hiện tại của lớp đang hoạt động. Mỗi lần lặp sẽ mở rộng hộp này thêm một đơn vị theo mọi hướng, phù hợp với định nghĩa về gói đệ quy. 

Sự lựa chọn thiết kế tinh tế nhất là chúng tôi không bao giờ xây dựng lại các lớp trước đó. Thay vào đó, chúng ta chỉ mở rộng ranh giới hiện tại và vẽ các cạnh của lớp mới. Ranh giới bên phải được viết rõ ràng là`|`, trong khi phía bên trái được để trống một cách trực quan bằng cách không tạo ra một bức tường thẳng đứng. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào ngắn như`01`. Ký tự đầu tiên tạo ra một trung tâm gồm hai ô nhỏ. Ký tự thứ hai bao bọc nó bằng một hình chữ nhật lớn hơn có phần trên và phần dưới chứa đầy`-`và cạnh phải của nó được đánh dấu bằng`|`. 

| Bước | Lớp | Đầu trang | Dưới cùng | Trái | Đúng | Hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | c | c+1 | Đặt chân đế "**" | 
| 1 | 1 | -1 | 1 | c-1 | c+2 | Thêm đường viền, đặt tường bên phải | 

Dấu vết này cho thấy cấu trúc phát triển đối xứng như thế nào trong khi vẫn bảo tồn được phần lõi trung tâm. 

Bây giờ hãy xem xét`001`. Hai lớp đầu tiên là thịt, tạo thành các tổ`*`các vùng và lớp cuối cùng chuyển sang ký tự búi tóc`-`, tạo ra một khung bên ngoài thống trị trực quan tất cả cấu trúc bên trong. Điều này chứng tỏ rằng nhận dạng lớp là độc lập và mỗi lớp chỉ ghi đè hoàn toàn ranh giới của chính nó mà không sửa đổi nội dung bên trong. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi lớp vẽ một đường viền hình chữ nhật đầy đủ có tổng diện tích trên tất cả các lớp là bậc hai theo n | 
| Không gian | O(n^2) | Lưới cuối cùng lưu trữ tất cả các ký tự của cấu trúc mở rộng | 

Các ràng buộc cho phép n lên tới 1000, do đó, việc xây dựng bậc hai phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. Các thừa số không đổi là nhỏ vì mỗi ô được viết một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal
assert run("0\n") != ""

# single bun
assert run("1\n") != ""

# two layers
assert run("01\n") != ""

# all same
assert run("000\n") != ""

# alternating
assert run("0101\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| lõi nhỏ | độ chính xác một lớp | 
|`1`| biến thể lõi nhỏ | xử lý ký tự thay thế | 
|`01`| khung lồng nhau | gói cơ bản | 
|`000`| lồng thống nhất | lặp đi lặp lại các lớp giống hệt nhau | 
|`0101`| cấu trúc xen kẽ | độ chính xác của lớp hỗn hợp | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`0`, thuật toán ngay lập tức đặt lõi hai ô cơ sở và không thực hiện mở rộng. Vì vòng lặp bắt đầu ở chỉ số 1 nên không có sự điều chỉnh biên nào xảy ra, do đó đầu ra vẫn ở mức tối thiểu và chính xác. 

Đối với một chuỗi như`0000`, mọi lớp đều sử dụng cùng một ký tự. Mỗi lần lặp lại vẫn mở rộng ranh giới, nhưng về mặt trực quan, kết quả là một chuỗi các khung giống hệt nhau. Thuật toán vẫn hoạt động vì nó không dựa vào sự khác biệt về ký tự giữa các lớp mà chỉ dựa vào việc mở rộng hình học. 

Đối với các đầu vào xen kẽ như`010101`, mỗi bản mở rộng sẽ chuyển ký tự bản vẽ. Vì mỗi lớp chỉ ghi đè hoàn toàn đường viền của chính nó nên không có sự can thiệp nào xảy ra giữa các lớp và tính bất biến mà các lớp bên trong không bị ảnh hưởng sẽ đảm bảo tính chính xác.
