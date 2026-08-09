---
title: "CF 102465I - Dấu Mason"
description: "Chúng tôi có một lưới pixel đen trắng tượng trưng cho một số viên đá. Các pixel màu đen có thể có ba vai trò. Một số thuộc về vùng màu đen được kết nối bên ngoài tất cả các viên đá, một số tạo thành dấu vết thợ xây thực sự bên trong viên đá và một số là các pixel nhiễu tách biệt."
date: "2026-08-08T09:27:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102465
codeforces_index: "I"
codeforces_contest_name: "2018-2019 ICPC Southwestern European Regional Programming Contest (SWERC 2018)"
rating: 0
weight: 102465
solve_time_s: 259
verified: true
draft: false
---

[CF 102465I - Dấu ấn của Mason](https://codeforces.com/problemset/problem/102465/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một lưới pixel đen trắng tượng trưng cho một số viên đá. Các pixel màu đen có thể có ba vai trò. Một số thuộc về vùng màu đen được kết nối bên ngoài tất cả các viên đá, một số tạo thành dấu vết thợ xây thực sự bên trong viên đá và một số là các pixel nhiễu tách biệt. Mỗi viên đá chứa chính xác một dấu hiệu và dấu hiệu đó được bao quanh bởi các pixel màu trắng. 

Kích thước của lưới tối đa là 1000 x 1000, do đó có thể có tới một triệu pixel. Một giải pháp quét liên tục một phần lớn hình ảnh cho mọi đối tượng được phát hiện có thể dễ dàng đạt tới hàng tỷ hoặc thậm chí hàng nghìn tỷ thao tác. Giới hạn bốn giây ủng hộ mạnh mẽ một thuật toán có tổng công việc tỷ lệ thuận với số lượng pixel, chỉ với một lượng công việc không đổi nhỏ trên mỗi pixel. Lưới đủ lớn nên việc lấp lũ đệ quy cũng không an toàn trong Python vì độ sâu đệ quy có thể tuyến tính theo kích thước lưới. 

Quan sát trọng tâm là ba điểm này khác nhau về mặt cấu trúc liên kết. Nếu chúng ta nhìn vào các vùng màu trắng được bao quanh bởi một dấu, thì A bao quanh chính xác một vùng màu trắng, B bao gồm chính xác hai vùng và C không bao quanh. Kích thước của các dấu đó có thể khác nhau do các tham số x và y là tùy ý, do đó việc đo hộp giới hạn hoặc số lượng pixel đen chính xác là không đáng tin cậy. Số thành phần màu trắng kèm theo không phụ thuộc vào x hoặc y. 

Có một số bẫy ẩn trong công thức này. Đầu tiên là vùng màu đen bên ngoài. Coi như```
#######
#.....#
#..#..#
#.....#
#######
```Sự cô lập`#`trông giống như một dấu hiệu có thể xảy ra nhưng thực ra nó là tiếng ồn. Quan trọng hơn, các pixel màu đen thuộc vùng bên ngoài có thể giống chữ C hoặc một dấu khác. Chúng phải được loại bỏ trước khi thực hiện bất kỳ phân loại nào. 

Cái bẫy thứ hai là kết nối chéo có ý nghĩa quan trọng đối với khu vực bên ngoài. Ví dụ,```
#######
#.....#
#.#...#
##....#
#######
```Hai vùng màu đen tiếp xúc theo đường chéo thuộc cùng một vùng bên ngoài dưới kết nối 8. Việc coi vùng bên ngoài là liên kết 4 có thể để lại một số pixel của nó và phân loại sai chúng thành nhãn hiệu. 

Cái bẫy thứ ba là tiếng ồn. Pixel nhiễu là thành phần màu đen bao gồm chính xác một pixel. Các pixel màu trắng xung quanh của nó tạo thành một phần bình thường của bề mặt đá, không phải là phần bên trong kín của nhãn hiệu. Một giải pháp bất cẩn chỉ đơn giản là đếm các thành phần màu trắng liền kề với mọi thành phần màu đen có thể nhầm một pixel đó với dấu giống chữ C hoặc thậm chí đếm một lỗ sai. 

Bẫy thứ tư là bề mặt trắng sử dụng kết nối 4 chứ không phải kết nối 8. Khoảng cách chéo không kết nối hai vùng màu trắng. Ví dụ,```
#######
#..#..#
#.#.#.#
#..#..#
#######
```phải được diễn giải bằng cách sử dụng tính liền kề theo chiều dọc và chiều ngang khi quyết định các pixel màu trắng nào thuộc cùng một vùng. Việc sử dụng kết nối 8 sẽ hợp nhất các vùng được phân tách bằng tiếp điểm chéo và có thể thay đổi số lượng lỗ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp trước tiên sẽ tìm thấy mọi thành phần màu đen và sau đó, đối với từng thành phần ứng cử viên, sẽ kiểm tra các pixel màu trắng xung quanh để xác định xem nó bao gồm bao nhiêu vùng. Điều này đúng về mặt khái niệm vì loại nhãn hiệu hoàn toàn được xác định bởi các vùng màu trắng kèm theo của nó. Vấn đề là việc thực hiện việc lấp đầy vùng lưới xung quanh riêng biệt cho mỗi ứng cử viên liên tục xử lý các pixel giống nhau. 

Trong trường hợp xấu nhất có thể có các thành phần nhỏ Θ(WH). Nếu mọi thành phần gây ra một lần quét Θ(WH) khác thì tổng công việc sẽ trở thành Θ((WH)^2). Với một triệu pixel, tức là ở mức 10^12 lượt truy cập pixel, vượt xa giới hạn 4 giây. Lực lượng vũ phu có tác dụng vì mỗi phân loại riêng lẻ đều dễ dàng, nhưng nó thất bại vì cùng một bề mặt màu trắng được khám phá đi khám phá lại. 

Quan sát hữu ích là các vùng màu trắng có thể được tính toán trên toàn cầu. Mỗi pixel màu trắng thuộc về chính xác một thành phần màu trắng được kết nối 4, vì vậy chúng ta có thể lấp đầy mọi thành phần màu trắng chính xác một lần. Trong khi thực hiện việc lấp lũ đó, chúng tôi kiểm tra các thành phần màu đen chạm vào nó. 

Bề mặt bên ngoài của viên đá chạm vào vùng màu đen xung quanh viên đá. Vùng bên trong thuộc về nhãn hiệu không chạm vào vùng màu đen bên ngoài đó. Bởi vì mỗi viên đá có chính xác một dấu hiệu, thành phần màu trắng bên trong như vậy có thể được liên kết với chính xác một thành phần màu đen không gây tiếng ồn. Tiếng ồn có thể được bỏ qua vì thành phần màu đen của nó có kích thước bằng một. 

Vấn đề sau đó trở thành một cặp tính toán thành phần được kết nối toàn cầu. Đầu tiên, chúng tôi xác định vùng màu đen bên ngoài được kết nối 8. Tiếp theo, chúng tôi dán nhãn cho mọi thành phần màu đen còn lại và ghi lại kích thước của nó. Cuối cùng, chúng tôi lấp đầy mọi thành phần màu trắng bằng 4 kết nối. Đối với mỗi thành phần màu trắng không chạm vào vùng màu đen bên ngoài, chúng tôi sẽ tăng số lượng lỗ của thành phần màu đen không có tiếng ồn duy nhất chạm vào nó. 

Số lỗ kết quả sẽ trực tiếp đưa ra loại điểm. Không lỗ có nghĩa là C, một lỗ có nghĩa là A và hai lỗ có nghĩa là B. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((WH)^2) | O(WH) | Quá chậm | 
| Tối ưu | O(WH) | O(WH) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ hình ảnh dưới dạng danh sách các chuỗi và coi mỗi pixel là một đỉnh của biểu đồ lưới. Các pixel đen sau này sẽ được kết nối bằng cách sử dụng lân cận 8 lân cận, trong khi các pixel trắng sẽ sử dụng lân cận 4 lân cận. 
2. Bắt đầu lấp đầy từ pixel góc`(0, 0)`sử dụng tất cả tám hướng lân cận. Toàn bộ đường viền có màu đen và tuyên bố đảm bảo rằng mọi pixel đen thuộc vùng bên ngoài đều được kết nối với đường viền bằng kết nối 8. Do đó, lũ lụt này dán nhãn chính xác cho vùng màu đen bên ngoài. 
3. Quét tất cả các pixel màu đen còn lại. Bất cứ khi nào tìm thấy một pixel đen không được gắn nhãn, hãy chạy tràn 8 kết nối và cung cấp cho thành phần đó một ID mới. Lưu trữ kích thước của nó. Các thành phần có kích thước một là tiếng ồn, trong khi mọi thành phần lớn hơn đều là dấu hiệu của thợ xây chính hãng. 
4. Quét tất cả các pixel màu trắng. Đối với mỗi pixel màu trắng chưa được truy cập, hãy chạy tràn 4 kết nối. Trong quá trình lấp đầy vùng lũ, hãy kiểm tra tất cả các pixel đen lân cận và ghi nhớ ba thông tin: liệu thành phần màu trắng này có chạm vào vùng màu đen bên ngoài hay không, thành phần màu đen không nhiễu mà nó chạm vào và liệu nó có chạm vào nhiều thành phần như vậy hay không. 
5. Sau khi khám phá xong thành phần màu trắng, hãy phân loại vai trò của nó. Nếu chạm vào vùng đen bên ngoài thì đó là một phần của bề mặt đá thông thường và không thể là điểm đánh dấu bên trong. Nếu nó không chạm vào vùng bên ngoài và liền kề với chính xác một thành phần màu đen không nhiễu thì đó là vùng khép kín thuộc về nhãn hiệu đó, do đó hãy tăng số lượng lỗ của thành phần đó. Các thành phần tiếng ồn được cố tình bỏ qua. 
6. Sau khi tất cả các thành phần màu trắng đã được xử lý, hãy kiểm tra số lượng lỗ của từng thành phần màu đen không có tiếng ồn. Thành phần không có lỗ tượng trưng cho C, một lỗ tượng trưng cho A và hai lỗ tượng trưng cho B. Tăng dần câu trả lời tương ứng. 

Điều bất biến chính là sau khi quá trình lấp lũ thành phần màu trắng kết thúc, mọi thành phần màu trắng đã được phân loại chính xác một lần là bề mặt đá thông thường hoặc nội thất khép kín. Vì bề mặt đá có 4 mối nối và mỗi viên đá chỉ có đúng một dấu ấn nên một thành phần kèm theo chỉ có thể thuộc về dấu hiệu của viên đá đó. Vì mọi thành phần nhiễu đều có kích thước một, nên việc bỏ qua các thành phần màu đen có kích thước một sẽ ngăn tiếng ồn tạo ra các lỗ trống. Do đó, mọi nhãn hiệu chính hãng đều nhận được chính xác số lượng vùng màu trắng kèm theo thực sự của nó. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    W, H = map(int, input().split())
    grid = [input().strip() for _ in range(H)]
    n = W * H

    # comp[idx] == -1: not a black component yet
    # comp[idx] == 0: outside black region
    # comp[idx] > 0: a genuine/noise black component
    comp = array('i', [-1]) * n

    # 1. Flood-fill the outside black region using 8-connectivity.
    stack = [0]
    comp[0] = 0

    while stack:
        p = stack.pop()
        r = p // W
        c = p - r * W

        r0 = max(0, r - 1)
        r1 = min(H - 1, r + 1)
        c0 = max(0, c - 1)
        c1 = min(W - 1, c + 1)

        for nr in range(r0, r1 + 1):
            base = nr * W
            for nc in range(c0, c1 + 1):
                if nr == r and nc == c:
                    continue
                q = base + nc
                if comp[q] == -1 and grid[nr][nc] == '#':
                    comp[q] = 0
                    stack.append(q)

    # sizes[component_id] is the number of black pixels.
    # Component 0 is the outside region.
    sizes = [0]
    sizes[0] = sum(1 for p in range(n) if comp[p] == 0)

    # 2. Label every remaining black component.
    for r in range(H):
        base = r * W
        for c in range(W):
            p = base + c
            if grid[r][c] != '#' or comp[p] != -1:
                continue

            cid = len(sizes)
            sizes.append(0)
            comp[p] = cid
            stack = [p]
            size = 0

            while stack:
                q = stack.pop()
                size += 1
                qr = q // W
                qc = q - qr * W

                r0 = max(0, qr - 1)
                r1 = min(H - 1, qr + 1)
                c0 = max(0, qc - 1)
                c1 = min(W - 1, qc + 1)

                for nr in range(r0, r1 + 1):
                    nbase = nr * W
                    for nc in range(c0, c1 + 1):
                        if nr == qr and nc == qc:
                            continue
                        nq = nbase + nc
                        if grid[nr][nc] == '#' and comp[nq] == -1:
                            comp[nq] = cid
                            stack.append(nq)

            sizes[cid] = size

    # 3. Flood-fill all white components with 4-connectivity.
    seen = bytearray(n)
    holes = [0] * len(sizes)

    for r in range(H):
        base = r * W
        for c in range(W):
            start = base + c
            if grid[r][c] != '.' or seen[start]:
                continue

            seen[start] = 1
            stack = [start]

            touches_outside = False
            candidate = -1
            multiple_marks = False

            while stack:
                p = stack.pop()
                pr = p // W
                pc = p - pr * W

                # Up
                if pr > 0:
                    q = p - W
                    if grid[pr - 1][pc] == '.':
                        if not seen[q]:
                            seen[q] = 1
                            stack.append(q)
                    else:
                        cid = comp[q]
                        if cid == 0:
                            touches_outside = True
                        elif sizes[cid] > 1:
                            if candidate == -1:
                                candidate = cid
                            elif candidate != cid:
                                multiple_marks = True

                # Down
                if pr + 1 < H:
                    q = p + W
                    if grid[pr + 1][pc] == '.':
                        if not seen[q]:
                            seen[q] = 1
                            stack.append(q)
                    else:
                        cid = comp[q]
                        if cid == 0:
                            touches_outside = True
                        elif sizes[cid] > 1:
                            if candidate == -1:
                                candidate = cid
                            elif candidate != cid:
                                multiple_marks = True

                # Left
                if pc > 0:
                    q = p - 1
                    if grid[pr][pc - 1] == '.':
                        if not seen[q]:
                            seen[q] = 1
                            stack.append(q)
                    else:
                        cid = comp[q]
                        if cid == 0:
                            touches_outside = True
                        elif sizes[cid] > 1:
                            if candidate == -1:
                                candidate = cid
                            elif candidate != cid:
                                multiple_marks = True

                # Right
                if pc + 1 < W:
                    q = p + 1
                    if grid[pr][pc + 1] == '.':
                        if not seen[q]:
                            seen[q] = 1
                            stack.append(q)
                    else:
                        cid = comp[q]
                        if cid == 0:
                            touches_outside = True
                        elif sizes[cid] > 1:
                            if candidate == -1:
                                candidate = cid
                            elif candidate != cid:
                                multiple_marks = True

            if not touches_outside and candidate != -1 and not multiple_marks:
                holes[candidate] += 1

    # 4. Translate the number of holes into A, B, or C.
    ans = [0, 0, 0]

    for cid in range(1, len(sizes)):
        if sizes[cid] == 1:
            continue

        if holes[cid] == 1:
            ans[0] += 1       # A
        elif holes[cid] == 2:
            ans[1] += 1       # B
        elif holes[cid] == 0:
            ans[2] += 1       # C

    print(*ans)

if __name__ == "__main__":
    solve()
```Lần lấp lũ đầu tiên sử dụng 8 hướng vì vấn đề xác định khu vực bên ngoài có kết nối chéo cũng như kết nối ngang và dọc. Bắt đầu lúc`(0, 0)`là đủ vì mọi pixel viền đều thuộc về vùng đó. 

Dòng lũ màu đen thứ hai dán nhãn cho mọi thành phần còn lại. Kích thước của nó là thông tin duy nhất cần thiết để phân biệt một dấu hiệu có thể có với nhiễu. Một thành phần màu đen đơn lẻ không thể là nhãn hiệu vì mỗi nhãn hiệu chính hãng đều chứa nhiều hơn một pixel màu đen. 

Phần lấp đầy màu trắng được cố tình kết nối 4. Trong khi xử lý thành phần màu trắng, mã sẽ ghi lại xem nó có chạm vào thành phần 0 hay không, tức là vùng màu đen bên ngoài. Bề mặt đá thông thường luôn có sự kết nối như vậy. Một nội thất có nhãn hiệu kèm theo thì không. 

các`candidate`biến ghi lại thành phần màu đen không nhiễu duy nhất có thể sở hữu vùng màu trắng. Các pixel nhiễu bị bỏ qua khi đưa ra quyết định này. Điều này quan trọng vì pixel nhiễu có thể nằm bên trong bề mặt màu trắng thông thường hoặc thậm chí bên trong phần bên trong màu trắng của nhãn hiệu mà không làm thay đổi nhận dạng của nhãn hiệu. 

Không có đệ quy được sử dụng. Việc lấp lũ đệ quy có thể vượt quá giới hạn đệ quy của Python trên lưới có hành lang dài. Ngăn xếp rõ ràng cũng làm cho việc sử dụng bộ nhớ có thể dự đoán được. Các số nguyên Python chỉ được sử dụng trong ngăn xếp DFS tạm thời, trong khi các nhãn thành phần được lưu trữ dưới dạng nhỏ gọn.`array('i')`. 

Không có vấn đề tràn số nguyên trong Python. Chỉ số liên quan lớn nhất là dưới một triệu và tất cả các kích thước thành phần tối đa là một triệu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đã cho chứa hai nhãn hiệu chính hãng sau khi loại bỏ vùng màu đen 8 nối bên ngoài. Một thành phần màu đen có hai thành phần màu trắng kèm theo, trong khi thành phần khác có một thành phần màu trắng kèm theo. Các pixel đen bị cô lập gây nhiễu và mẫu hình chữ C được đề cập trong câu lệnh được kết nối với vùng bên ngoài nên sẽ bị loại bỏ. 

| Sân khấu | Đối tượng | Kích thước màu đen | Các thành phần màu trắng kèm theo | Phân loại | 
| --- | --- | --- | --- | --- | 
| Lũ lụt đen | Vùng ngoài | nhiều | không được xem xét | Bên ngoài | 
| Thành phần màu đen | Dấu trái | lớn hơn 1 | 2 | B | 
| Thành phần màu đen | Nhãn hiệu chính hãng khác | lớn hơn 1 | 1 | A | 
| Thành phần màu đen | Tiếng ồn bị cô lập | 1 | bỏ qua | Tiếng ồn | 
| Thành phần màu đen | Hoa văn bên ngoài hình chữ C | một phần bên ngoài | không được xem xét | Bên ngoài | 

Số đếm cuối cùng là`A = 1`,`B = 1`, Và`C = 0`, cho`1 1 0`. 

Phần hữu ích của dấu vết này là việc phân loại không phụ thuộc vào chiều rộng hoặc chiều cao của nhãn hiệu. Hai lỗ này đủ để xác định B ngay cả khi các thông số của nó khác với dấu khác. 

### Xây dựng ví dụ 2 

Hãy xem xét một hình ảnh có chứa một dấu hình chữ C và một pixel nhiễu:```
#########
#.......#
#.......#
#.#####.#
#.#.....#
#.#####.#
#.......#
#.......#
#########
```Sau khi loại bỏ thành phần màu đen bên ngoài, thành phần màu đen hình chữ C không còn thành phần màu trắng kèm theo. Pixel đen bị cô lập bên trong phần mở là nhiễu và có kích thước thành phần bằng một. 

| Sân khấu | Đối tượng | Kích thước màu đen | Linh kiện màu trắng không tiếp xúc với bên ngoài | Phân loại | 
| --- | --- | --- | --- | --- | 
| Lũ lụt đen | Vùng biên giới | nhiều | không được xem xét | Bên ngoài | 
| Thành phần màu đen | Dấu hình chữ C | lớn hơn 1 | 0 | C | 
| Thành phần màu đen | Tiếng ồn bị cô lập | 1 | bỏ qua | Tiếng ồn | 

Kết quả là`0 0 1`. 

Ví dụ này giải thích tại sao kích thước bộ phận phải được kiểm tra trước khi đếm lỗ. Nếu mọi thành phần màu đen được coi là một dấu hiệu thì pixel nhiễu sẽ tạo ra câu trả lời sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(WH) | Mỗi pixel đen trắng được truy cập với số lần không đổi, với tối đa tám lần kiểm tra hàng xóm trong quá trình lấp đầy lũ đen và bốn lần kiểm tra trong khi lấp đầy lũ trắng. | 
| Không gian | O(WH) | Hình ảnh, nhãn thành phần, mảng đã truy cập, số lỗ và ngăn xếp lấp đầy yêu cầu bộ nhớ tuyến tính. | 

Với tối đa một triệu pixel, thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi pixel. Mức tiêu thụ bộ nhớ cũng tuyến tính và duy trì trong giới hạn 256 MB. Việc triển khai tránh đệ quy Python và lưu trữ cấu trúc nhãn có kích thước lưới lớn nhất trong một mảng số nguyên nhỏ gọn. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng logic thành phần và số lỗ tương tự như giải pháp được gửi. Trường hợp đầu tiên kiểm tra mẫu được cung cấp, trường hợp thứ hai kiểm tra kích thước tối thiểu không có dấu, trường hợp thứ ba đặt một số dấu hình chữ B trong cùng một hình ảnh và trường hợp thứ tư kiểm tra hình ảnh toàn màu đen có kích thước tối đa.```python
import io
import sys
from array import array

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    W, H = map(int, input().split())
    grid = [input().strip() for _ in range(H)]
    n = W * H

    comp = array('i', [-1]) * n

    stack = [0]
    comp[0] = 0

    while stack:
        p = stack.pop()
        r, c = divmod(p, W)

        for nr in range(max(0, r - 1), min(H - 1, r + 1) + 1):
            for nc in range(max(0, c - 1), min(W - 1, c + 1) + 1):
                if nr == r and nc == c:
                    continue
                q = nr * W + nc
                if comp[q] == -1 and grid[nr][nc] == '#':
                    comp[q] = 0
                    stack.append(q)

    sizes = [sum(1 for x in comp if x == 0)]

    for r in range(H):
        for c in range(W):
            p = r * W + c
            if grid[r][c] != '#' or comp[p] != -1:
                continue

            cid = len(sizes)
            comp[p] = cid
            stack = [p]
            size = 0

            while stack:
                q = stack.pop()
                size += 1
                qr, qc = divmod(q, W)

                for nr in range(max(0, qr - 1), min(H - 1, qr + 1) + 1):
                    for nc in range(max(0, qc - 1), min(W - 1, qc + 1) + 1):
                        if nr == qr and nc == qc:
                            continue
                        nq = nr * W + nc
                        if grid[nr][nc] == '#' and comp[nq] == -1:
                            comp[nq] = cid
                            stack.append(nq)

            sizes.append(size)

    seen = bytearray(n)
    holes = [0] * len(sizes)

    for r in range(H):
        for c in range(W):
            start = r * W + c
            if grid[r][c] != '.' or seen[start]:
                continue

            seen[start] = 1
            stack = [start]
            outside = False
            candidate = -1
            multiple = False

            while stack:
                p = stack.pop()
                pr, pc = divmod(p, W)

                for nr, nc in (
                    (pr - 1, pc),
                    (pr + 1, pc),
                    (pr, pc - 1),
                    (pr, pc + 1),
                ):
                    if not (0 <= nr < H and 0 <= nc < W):
                        continue

                    q = nr * W + nc

                    if grid[nr][nc] == '.':
                        if not seen[q]:
                            seen[q] = 1
                            stack.append(q)
                    else:
                        cid = comp[q]
                        if cid == 0:
                            outside = True
                        elif sizes[cid] > 1:
                            if candidate == -1:
                                candidate = cid
                            elif candidate != cid:
                                multiple = True

            if not outside and candidate != -1 and not multiple:
                holes[candidate] += 1

    ans = [0, 0, 0]
    for cid in range(1, len(sizes)):
        if sizes[cid] == 1:
            continue
        if holes[cid] == 1:
            ans[0] += 1
        elif holes[cid] == 2:
            ans[1] += 1
        elif holes[cid] == 0:
            ans[2] += 1

    sys.stdin = old_stdin
    return " ".join(map(str, ans))

sample1 = """\
26 15
##########################
##........######......#..#
#...###....#####..#......#
#...#.#....####.........##
#...###.....##....#####..#
#...#.#.....#.....#####..#
#...###.....#.....##.##..#
#........#..#.#...#####..#
#..###......#.....#####..#
#..#........#...#.##.##..#
#..#........#.....##.##..#
#..#...#.#..#...#.##.##..#
#..###......#............#
###....#....##....##.....#
##########################
"""
assert run(sample1) == "1 1 0", "sample 1"

minimum = """\
7 9
#######
#######
#######
#######
#######
#######
#######
#######
#######
"""
assert run(minimum) == "0 0 0", "minimum dimensions"

two_b = """\
15 9
###############
#.............#
#..###........#
#..#.#........#
#..###........#
#........###..#
#........#.#..#
#........###..#
###############
"""
assert run(two_b) == "0 2 0", "two B marks"

maximum = "1000 1000\n" + ("#" * 1000 + "\n") * 1000
assert run(maximum) == "0 0 0", "maximum all-black grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp 26 x 15 mẫu |`1 1 0`| Dấu A và B chính hãng, tiếng ồn và vùng hình chữ C kết nối với bên ngoài | 
| Lưới 7 x 9 toàn màu đen |`0 0 0`| Kích thước tối thiểu và phát hiện hoàn toàn khu vực bên ngoài | 
| 15 x 9 với hai điểm B |`0 2 0`| Nhiều dấu cùng loại và đếm lỗ độc lập | 
| Lưới toàn màu đen 1000 x 1000 |`0 0 0`| Kích thước đầu vào tối đa và hành vi thời gian tuyến tính | 

## Vỏ cạnh 

Vùng màu đen bên ngoài được xử lý bằng đợt lũ đầu tiên. Bởi vì quá trình lấp đầy bắt đầu ở đường viền và sử dụng tất cả tám hướng, nên mọi pixel màu đen được kết nối với đường viền thông qua tiếp xúc ngang, dọc hoặc chéo sẽ nhận được ID thành phần bằng 0. Hình dạng màu đen trông giống C nhưng thuộc vùng này không bao giờ đạt đến giai đoạn phân loại sau này. 

Một pixel nhiễu đơn được xử lý bằng cách kiểm tra kích thước thành phần màu đen. Giả sử một pixel nhiễu được bao quanh bởi một bề mặt lớn màu trắng. Thành phần màu trắng liền kề của nó bị bỏ qua một cách có chủ ý khi đếm các lỗ vì thành phần màu đen có kích thước bằng một. Nhãn hiệu chính hãng có thành phần lớn hơn nên vùng trắng kèm theo vẫn được tính. 

Dấu không có vùng trắng kèm theo được xếp vào loại C. Phần màu trắng xung quanh dấu đó chạm vào vùng đen bên ngoài qua bề mặt đá thông thường nên không được tính là lỗ. Bản thân dấu này vẫn là thành phần màu đen không có tiếng ồn và nhận được số lỗ bằng 0. 

Dấu có một vùng màu trắng kèm theo được phân loại là A. Trong quá trình lấp đầy màu trắng, vùng kèm theo không thể chạm tới thành phần màu đen bên ngoài, do đó`touches_outside`vẫn sai. Nó chạm vào thành phần A, trở thành`candidate`và số lỗ của thành phần đó tăng lên một. 

Dấu có hai vùng màu trắng kèm theo được phân loại là B. Hai phần bên trong là 4 thành phần màu trắng được kết nối riêng biệt nên quét trắng sẽ gặp chúng một cách độc lập. Mỗi cái sẽ tăng số lượng lỗ của cùng một thành phần màu đen, để nó bằng hai. Do đó, việc phân loại cuối cùng tạo ra B. 

Tiếp xúc màu trắng theo đường chéo không được phép hợp nhất các vùng vì vùng lấp đầy màu trắng chỉ sử dụng bốn hướng. Điều này phù hợp với định nghĩa của bề mặt đá và ngăn chặn một cú chạm chéo phá hủy một lỗ thật. 

Hình ảnh có thể chứa một số lượng lớn các pixel nhiễu. Ngay cả khi mọi pixel khác bị nhiễu riêng biệt thì mỗi thành phần màu đen vẫn được phát hiện một lần, mỗi thành phần có kích thước một và không có thành phần nào đóng góp vào câu trả lời. Tổng công việc vẫn tuyến tính về số lượng pixel. 

Lưới có thể có màu đen hoàn toàn. Trong trường hợp đó, lần lấp đầy đầu tiên sẽ sử dụng toàn bộ hình ảnh, không có thành phần màu đen nào khác tồn tại và không có thành phần màu trắng nào để xử lý. Câu trả lời là chính xác`0 0 0`. 

Lưới cũng có thể chứa nhiều viên đá và nhãn hiệu riêng biệt. Mỗi thành phần màu trắng vẫn được xử lý chính xác một lần và mỗi dấu chỉ được tính phí cho các thành phần màu trắng kèm theo thuộc về nó. Không cần quét từng điểm trên toàn bộ lưới, đây là thuộc tính giữ thuật toán ở O(WH).
