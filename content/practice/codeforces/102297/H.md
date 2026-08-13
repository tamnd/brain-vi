---
title: "CF 102297H - Vươn tới các vì sao"
description: "Con tem là một hình chữ thập được căn chỉnh theo trục cố định bao gồm năm ô: ô trung tâm và bốn ô liền kề trực giao với nó. Việc dập sẽ biến năm ô giấy đó thành màu đen."
date: "2026-08-13T22:44:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102297
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2015"
rating: 0
weight: 102297
solve_time_s: 112
verified: true
draft: false
---

[CF 102297H - Vươn tới các vì sao](https://codeforces.com/problemset/problem/102297/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Con tem là một hình chữ thập được căn chỉnh theo trục cố định bao gồm năm ô: ô trung tâm và bốn ô liền kề trực giao với nó. Việc dập sẽ biến năm ô giấy đó thành màu đen. Vì mực đen vẫn đen sau khi dán tem khác nên được phép dán tem chồng lên nhau, nhưng tem không bao giờ có thể chạm vào ô được cho là vẫn giữ nguyên màu trắng. 

Nhiệm vụ là tìm số lượng nhỏ nhất của các vị trí chéo như vậy mà hợp của nó chính xác là tập hợp các ô đen. Nếu bức ảnh đã trắng hoàn toàn thì câu trả lời là không. Nếu ô đen nào đó không thể được đóng dấu bởi bất kỳ con dấu pháp lý nào thì câu trả lời là không thể. 

Mỗi hình ảnh có tối đa 9 hàng và 9 cột. Trang Codeforces hiện đưa ra giới hạn thời gian 2,5 giây và giới hạn bộ nhớ 256 MB. Lưới có kích thước tuyệt đối rất nhỏ nhưng có thể có tới 49 tâm tem trong một hình ảnh 9 x 9. Điều đó làm cho việc thử mọi tập hợp con của các vị trí tem theo cấp số nhân theo số lượng ô có thể chứa tem chứ không chỉ theo cấp số nhân theo chiều rộng lưới. 

Quan sát hữu ích đầu tiên là tâm tem chỉ có thể là một ô bên trong, vì cả bốn cánh tay phải nằm bên trong tờ giấy. Trung tâm pháp luật cũng phải có`#`tại tất cả năm ô bị chiếm giữ bởi thập tự giá. Một khi những điều kiện đó được giữ vững, việc đặt tem luôn an toàn. Điều này cho phép chúng tôi tách vấn đề thành việc lựa chọn các trung tâm tem hợp pháp và đảm bảo rằng liên minh của họ bao trùm mọi ô đen. 

Có một số trường hợp khó khăn có thể làm thất bại việc triển khai đơn giản hơn. Không thể đóng dấu hình ảnh đen một ô:```
1
1 1
#
```Đầu ra đúng là`Image #1: impossible`, vì con tem không thể vừa với tờ giấy 1x1. 

Một hình ảnh trống không cần đóng dấu gì cả:```
1
1 1
.
```Câu trả lời là`Image #1: 0`. Một tìm kiếm nhất quyết tìm một con tem sẽ tuyên bố không chính xác điều này là không thể. 

Một góc đen cũng không thể che đậy được. Ví dụ,```
1
3 3
###
###
###
```là không thể. Tâm tem duy nhất có thể có trong một tờ giấy 3 x 3 là ô ở giữa và hình chữ thập của nó không chứa cả hai góc. Một giải pháp bất cẩn chỉ kiểm tra xem mọi con tem được chọn có an toàn hay không có thể chấp nhận hình ảnh này mà không kiểm tra xem mọi ô đen có bị che hay không. 

Có một trường hợp tế nhị khác khi có tem pháp lý nhưng chưa đủ. Vì```
1
3 3
.#.
###
.#.
```chữ thập ở giữa là một con dấu hợp pháp và bao phủ mọi ô màu đen, vì vậy câu trả lời chính xác là 1. Chỉ kiểm tra sự tồn tại của các trung tâm hợp pháp sẽ không đủ đối với các hình ảnh lớn hơn, bởi vì tất cả các ô màu đen vẫn phải được che đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp nhất là xác định mọi vị trí tem hợp pháp và thử mọi tập hợp con của các vị trí đó. Đối với mỗi tập hợp con, chúng ta có thể xây dựng các ô đen thu được, so sánh chúng với mục tiêu và giữ lại tập hợp con nhỏ nhất. 

Điều này đúng vì mọi trình tự dập có thể có đều có thể được biểu diễn bằng tập hợp các vị trí được đóng dấu. Việc lặp lại cùng một vị trí không bao giờ có tác dụng vì lần dập thứ hai không làm thay đổi ô nào. Tuy nhiên, một bảng 9 x 9 có thể có 7 x 7 hoặc 49 tâm tem. Trong trường hợp xấu nhất có thể có một con dấu hợp pháp ở mỗi nơi trong số đó, mang lại`2^49 = 562,949,953,421,312`tập hợp con. Ngay cả khi việc kiểm tra một tập hợp con chỉ mất một vài thao tác thì thời gian này vẫn vượt xa thời gian sẵn có. 

Cấu trúc chính giúp giải quyết vấn đề là mỗi tem chỉ ảnh hưởng đến một hàng và hai hàng lân cận của nó. Bên trong hàng của chính nó, nó ảnh hưởng đến ba cột liên tiếp. Điều đó có nghĩa là khi xử lý từng hàng hình ảnh, phạm vi bao phủ của hàng hiện tại chỉ phụ thuộc vào các lựa chọn tem ở hàng trước, hàng hiện tại và hàng tiếp theo. 

Vì có tối đa 9 cột nên chúng ta có thể biểu thị tất cả tâm tem được chọn trong một hàng bằng mặt nạ bit. Trung tâm tem chỉ có thể xuất hiện ở một trong các`c - 2`các cột bên trong, vì vậy có nhiều nhất`2^7 = 128`mặt nạ có thể cho một hàng. 

Giả sử hàng trước sử dụng mặt nạ`prev`, hàng hiện tại sử dụng mặt nạ`cur`, và hàng tiếp theo sử dụng mặt nạ`nxt`. Hàng hiện tại nhận được phạm vi bao phủ theo chiều dọc từ`prev`Và`nxt`. Nó nhận được phạm vi bảo hiểm theo chiều ngang từ`cur`, bởi vì mọi tâm được chọn cũng tô màu ô ngay bên trái và bên phải của nó. Nếu như`horizontal[cur]`biểu thị`cur | (cur << 1) | (cur >> 1)`, 

thì phạm vi bao phủ đầy đủ của hàng hiện tại là`prev | horizontal[cur] | nxt`. 

Chúng ta có thể kiểm tra xem điều này có bao gồm chính xác những gì cần thiết trong hàng mục tiêu hay không. Vì mọi trung tâm được chọn đều được coi là tem hợp pháp nên không có tem được chọn nào có thể tô màu một chấm. Vì vậy chúng ta chỉ cần kiểm tra xem mọi mục tiêu`#`được che phủ. 

Điều này mang lại một chương trình động có trạng thái ghi nhớ hai mặt nạ hàng liên tiếp. Khi chúng ta chọn mặt nạ hàng tiếp theo, hàng cũ nhất sẽ được xác định hoàn toàn và có thể được kiểm tra và loại bỏ. Lực lượng vũ phu hoạt động vì nó xem xét rõ ràng mọi bộ tem, nhưng không thành công khi có nhiều trung tâm khả thi. Bản chất hàng cục bộ của chéo cho phép chúng tôi thay thế tìm kiếm theo cấp số nhân trên 49 vị trí bằng một chương trình động bitmask nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^((r-2)(c-2)) * rc)`|`O(rc)`| Quá chậm | 
| Tối ưu |`O(r * 2^(3(c-2)))`|`O(2^(2(c-2)))`| Đã chấp nhận | 

Với`c <= 9`, giới hạn chuyển tiếp tối ưu nhiều nhất là khoảng`9 * 2^21`, điều này rất thiết thực cho những lưới nhỏ này. 

## Hướng dẫn thuật toán 

1. Đọc lưới và xác định ô bên trong nào có thể đóng vai trò là tâm tem. Một trung tâm`(i,j)`hợp pháp chính xác khi trung tâm và bốn hàng xóm của nó đều`#`. Con tem như vậy có thể được sử dụng một cách an toàn vì nó không bao giờ tạo ra mực đen trên chấm mục tiêu. 
2. Đối với mỗi hàng, hãy tạo một bitmask`allowed[row]`chứa tất cả các trung tâm tem hợp pháp trong hàng đó. Các hàng ranh giới tự động có mặt nạ trống vì không thể căn giữa dấu ở đó. 
3. Tạo mọi mặt nạ con của`allowed[row]`. Mỗi mặt nạ con đại diện cho một lựa chọn có thể có về việc đặt tem hợp pháp vào hàng đó. Mặt nạ số 0 được đưa vào vì không đặt dấu nào liên tiếp là một lựa chọn hợp lệ. 
4. Tính toán trước phạm vi bao phủ theo chiều ngang cho mọi mặt nạ hàng có thể. Nếu như`cur`chọn một số trung tâm tem,`cur`,`cur << 1`, Và`cur >> 1`đại diện cho các ô trung tâm và cánh tay ngang của chúng. Sự kết hợp của chúng chính xác là một phần của hàng hiện tại được tô màu bằng các tem ở giữa hàng đó. 
5. Khởi tạo chương trình động với hai hàng trống. Về mặt khái niệm, hàng trước ảnh không có tem và hàng ảnh đầu tiên cũng không có tâm tem, vì vậy trạng thái ban đầu là`(prev, cur) = (0, 0)`với chi phí bằng không. 
6. Xử lý hình ảnh từ trên xuống dưới. Đối với hàng hiện tại, hãy chọn mặt nạ`nxt`từ các mặt nạ tem có thể có của hàng tiếp theo. Hàng hiện tại sau đó được xác định đầy đủ, bởi vì phạm vi bao phủ theo chiều dọc của nó đến từ`prev`Và`nxt`, trong khi phạm vi bao phủ ngang của nó đến từ`cur`. 
7. Tính toán`covered = prev | horizontal[cur] | nxt`. Chỉ giữ quá trình chuyển đổi này khi mọi ô màu đen trong hàng mục tiêu hiện tại được bao gồm trong`covered`. Ở dạng bitmask, điều kiện là`(covered & target[row]) == target[row]`. 
8. Chuyển trạng thái lập trình động từ`(prev, cur)`ĐẾN`(cur, nxt)`và thêm số lượng tem đã chọn vào`nxt`đến chi phí. Số lượng tem được chọn chỉ đơn giản là`nxt.bit_count()`. 
9. Ở hàng cuối cùng, buộc`nxt = 0`. Điều này ngăn không cho tem được đặt bên ngoài tờ giấy và giúp kiểm tra độ bao phủ hoàn chỉnh của hàng cuối cùng. 
10. Sau khi hàng cuối cùng được xử lý, hãy lấy chi phí tối thiểu trong số tất cả các trạng thái còn tồn tại. Nếu không có trạng thái nào tồn tại thì không thể có được hình ảnh mục tiêu. 

### Tại sao nó hoạt động 

Bất biến là trạng thái DP`(prev, cur)`đại diện cho mọi lựa chọn tem có thể có trong hàng hiện tại với chính xác số lượng tem tối thiểu được lưu trữ, trong khi chỉ để lại hàng tiếp theo chưa quyết định. Khi thuật toán chọn`nxt`, mọi tem có thể ảnh hưởng đến hàng hiện tại đều đã được biết: chúng được căn giữa ở hàng trước, hiện tại hoặc tiếp theo. Vì vậy việc kiểm tra mức độ bao phủ là chính xác. Quá trình chuyển đổi được giữ chính xác khi tất cả các ô màu đen của hàng hiện tại được che phủ và bởi vì mỗi dấu được chọn đều hợp pháp riêng lẻ nên không có dấu chấm nào được tô màu. Sau khi kiểm tra hàng, hàng trước đó có thể bị lãng quên vì không tem sau này có thể chạm tới hàng đó. Mỗi cấu hình dập hợp lệ sẽ tạo ra một chuỗi mặt nạ hàng được DP xem xét và mỗi chuỗi DP tương ứng với các tem hợp pháp, do đó việc giảm thiểu số lượng tem tích lũy sẽ mang lại mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

def solve_case(r, c, grid):
    # A stamp center must be strictly inside the grid.
    # allowed[i] contains the columns where a legal stamp can be centered.
    allowed = [0] * r

    for i in range(1, r - 1):
        mask = 0
        for j in range(1, c - 1):
            if (
                grid[i][j] == '#'
                and grid[i - 1][j] == '#'
                and grid[i + 1][j] == '#'
                and grid[i][j - 1] == '#'
                and grid[i][j + 1] == '#'
            ):
                mask |= 1 << j
        allowed[i] = mask

    # Every possible set of stamp centers in a row.
    choices = []
    for mask in allowed:
        row_choices = []
        sub = mask
        while True:
            row_choices.append(sub)
            if sub == 0:
                break
            sub = (sub - 1) & mask
        choices.append(row_choices)

    # Target row as a bitmask.
    target = []
    for row in grid:
        mask = 0
        for j, ch in enumerate(row):
            if ch == '#':
                mask |= 1 << j
        target.append(mask)

    # Horizontal coverage produced by stamps centered in each row.
    full = (1 << c) - 1
    horizontal = [0] * (1 << c)
    for mask in range(1 << c):
        horizontal[mask] = (
            mask
            | ((mask << 1) & full)
            | (mask >> 1)
        )

    # dp[(prev, cur)] = minimum number of stamps selected so far.
    # Initially both rows contain no stamps.
    dp = {(0, 0): 0}

    for i in range(r):
        ndp = {}

        if i + 1 < r:
            next_choices = choices[i + 1]
        else:
            # Nothing may be centered outside the paper.
            next_choices = [0]

        for (prev, cur), cost in dp.items():
            base = prev | horizontal[cur]

            for nxt in next_choices:
                covered = base | nxt

                # Every '#' in this row must be covered.
                if (covered & target[i]) != target[i]:
                    continue

                new_cost = cost + nxt.bit_count()
                state = (cur, nxt)

                old = ndp.get(state, INF)
                if new_cost < old:
                    ndp[state] = new_cost

        dp = ndp

        if not dp:
            return None

    return min(dp.values()) if dp else None

def solve():
    t = int(input())

    out = []

    for case in range(1, t + 1):
        r, c = map(int, input().split())
        grid = [input().strip() for _ in range(r)]

        answer = solve_case(r, c, grid)

        if answer is None:
            result = "impossible"
        else:
            result = str(answer)

        out.append(f"Image #{case}: {result}")
        out.append("")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve_case`xác định các trung tâm tem hợp pháp. Việc kiểm tra năm ô được thực hiện có chủ ý trước chương trình động, bởi vì một khi một trung tâm vượt qua nó, mọi lần sử dụng trung tâm đó trong tương lai đều được đảm bảo không tạo ra một ô đen không mong muốn. 

các`allowed`mặt nạ sử dụng số cột thực tế làm vị trí bit. Điều này làm cho các dịch chuyển ngang thẳng hàng trực tiếp với các cột lưới, do đó không có sự chuyển đổi giữa các chỉ mục cột được nén và cột gốc. Vì trung tâm pháp chỉ ở cột`1`bởi vì`c - 2`, dịch chuyển mặt nạ của họ sang trái hoặc phải vẫn ở bên trong tờ giấy. 

các`choices`xây dựng liệt kê mọi tập hợp con của trung tâm pháp lý cho mỗi hàng. Đây không phải là liệt kê các tập hợp con tùy ý của tất cả các cột. Trong trường hợp xấu nhất chỉ có thể có bảy cột ở giữa, do đó một hàng có tối đa 128 lựa chọn. 

Các hàng mục tiêu cũng được biểu diễn dưới dạng mặt nạ. biểu thức`(covered & target[i]) == target[i]`kiểm tra vùng phủ sóng mà không cần quan tâm liệu ô đen có bị che một lần hay nhiều lần hay không. Điều này phù hợp với quy trình dập vật lý, trong đó mực đen lặp đi lặp lại không thể phân biệt được với một lớp duy nhất. 

Trạng thái lập trình động chứa chính xác hai mặt nạ hàng.`prev`cung cấp nhánh hướng xuống của các tem ở giữa hàng hiện tại,`cur`cung cấp tâm và cánh tay ngang ở hàng hiện tại, và`nxt`cung cấp nhánh hướng lên của tem ở giữa bên dưới nó. Khi hàng hiện tại đã được kiểm tra,`prev`không còn cần thiết nữa. 

Không có vấn đề tràn số nguyên trong Python và mặt nạ lớn nhất chỉ sử dụng chín bit. các`INF`giá trị lớn hơn nhiều so với số lượng tem hữu ích tối đa có thể là 49. 

Quá trình chuyển đổi cuối cùng sử dụng rõ ràng`[0]`vì`nxt`. Nếu không có điều kiện biên này, về mặt khái niệm, DP có thể sử dụng một con tem ở giữa tờ giấy để che một ô ở hàng cuối cùng, điều này sẽ vi phạm yêu cầu toàn bộ con tem vẫn nằm trong tờ giấy. 

## Ví dụ đã hoạt động 

Hình ảnh mẫu đầu tiên là một tờ giấy 1 x 1 chỉ chứa một dấu chấm. Không có lý do gì để đặt một con tem, và thực tế là không có con tem nào có thể phù hợp. DP bắt đầu với mặt nạ trống và ngay lập tức xác minh rằng vùng phủ sóng trống có đáp ứng hàng mục tiêu trống hay không. 

| Hàng |`prev`|`cur`|`nxt`| Mục tiêu | Bảo hiểm | Chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`0`|`0`|`0`|`0`|`0`|`0`| 

Quá trình chuyển đổi tồn tại vì mục tiêu không chứa ô đen. Mức tối thiểu cuối cùng bằng 0, cho`Image #1: 0`. Điều này chứng tỏ tại sao một bức tranh trống phải được coi là giải pháp không tem hợp lệ. 

Hình ảnh mẫu thứ hai là một ô màu đen. Một lần nữa chỉ có một hàng nên không thể có trung tâm tem hợp pháp. Mặt nạ bit đích chứa một bit, nhưng phạm vi bao phủ duy nhất có thể là 0. 

| Hàng |`prev`|`cur`|`nxt`| Mục tiêu | Bảo hiểm | Chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`0`|`0`|`0`|`1`|`0`| không hợp lệ | 

Việc kiểm tra mức độ bao phủ không thành công vì`(0 & 1) != 1`. Không có trạng thái DP nào tồn tại nên kết quả là`impossible`. Điều này xác nhận rằng thuật toán phân biệt mục tiêu trống với mục tiêu chứa ô đen không được che phủ. 

Đối với mẫu thứ ba, hình chữ thập 3 x 3 có đúng một tâm pháp lý.```
.#.
###
.#.
```Mặt nạ tem duy nhất có thể là bit trung tâm. Hàng đầu tiên không thể có trung tâm nên DP chọn trung tâm làm`nxt`trong khi kiểm tra hàng trên cùng. Cánh tay hướng lên của trung tâm bao phủ ô màu đen duy nhất trong hàng đó. 

| Hàng |`prev`|`cur`|`nxt`| Mục tiêu | Bảo hiểm | Chi phí | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`0`|`0`| trung tâm | trung tâm | trung tâm |`1`| 
| 1 |`0`| trung tâm |`0`| trái, giữa, phải | trái, giữa, phải |`1`| 
| 2 | trung tâm |`0`|`0`| trung tâm | trung tâm |`1`| 

Sau khi chọn cả ba hàng, tâm được chọn duy nhất sẽ bao phủ mọi ô màu đen. Câu trả lời là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(r * 2^(3(c-2)))`| Nhiều nhất`2^(c-2)`mặt nạ cho mỗi hàng trong số ba hàng liên tiếp được xem xét trong trường hợp xấu nhất | 
| Không gian |`O(2^(2(c-2)))`| Một tiểu bang lưu trữ hai mặt nạ hàng, với tối đa`2^(c-2)`khả năng cho mỗi | 

Vì`r,c <= 9`, có tối đa bảy cột tâm tem có thể có trong một hàng. Do đó, DP hoạt động với tối đa 128 mặt nạ mỗi hàng và nhiều nhất là 16.384 trạng thái hai hàng. Ngay cả giới hạn chuyển tiếp tồi tệ nhất cũng đủ nhỏ cho giới hạn 2,5 giây và chỉ sử dụng một phần nhỏ trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

INF = 10**9

def solve_case(r, c, grid):
    allowed = [0] * r

    for i in range(1, r - 1):
        mask = 0
        for j in range(1, c - 1):
            if (
                grid[i][j] == '#'
                and grid[i - 1][j] == '#'
                and grid[i + 1][j] == '#'
                and grid[i][j - 1] == '#'
                and grid[i][j + 1] == '#'
            ):
                mask |= 1 << j
        allowed[i] = mask

    choices = []
    for mask in allowed:
        row_choices = []
        sub = mask
        while True:
            row_choices.append(sub)
            if sub == 0:
                break
            sub = (sub - 1) & mask
        choices.append(row_choices)

    target = []
    for row in grid:
        mask = 0
        for j, ch in enumerate(row):
            if ch == '#':
                mask |= 1 << j
        target.append(mask)

    full = (1 << c) - 1
    horizontal = [0] * (1 << c)
    for mask in range(1 << c):
        horizontal[mask] = (
            mask
            | ((mask << 1) & full)
            | (mask >> 1)
        )

    dp = {(0, 0): 0}

    for i in range(r):
        ndp = {}
        next_choices = choices[i + 1] if i + 1 < r else [0]

        for (prev, cur), cost in dp.items():
            base = prev | horizontal[cur]

            for nxt in next_choices:
                covered = base | nxt

                if (covered & target[i]) != target[i]:
                    continue

                state = (cur, nxt)
                new_cost = cost + nxt.bit_count()

                if new_cost < ndp.get(state, INF):
                    ndp[state] = new_cost

        dp = ndp

        if not dp:
            return None

    return min(dp.values()) if dp else None

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case in range(1, t + 1):
        r, c = map(int, input().split())
        grid = [input().strip() for _ in range(r)]

        ans = solve_case(r, c, grid)
        value = "impossible" if ans is None else str(ans)

        out.append(f"Image #{case}: {value}")
        out.append("")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

sample_input = """5
1 1
.
1 1
#
3 3
.#.
###
.#.
3 5
.#.#.
#####
.#.#.
4 7
.##.#..
######.
.######
..#..#.
"""

sample_output = """Image #1: 0

Image #2: impossible

Image #3: 1

Image #4: 2

Image #5: 5

"""

assert run(sample_input) == sample_output, "provided samples"

assert run("""1
1 1
.
""") == """Image #1: 0

""", "minimum-size empty image"

assert run("""1
1 1
#
""") == """Image #1: impossible

""", "minimum-size black image"

assert run("""1
3 3
###
###
###
""") == """Image #1: impossible

""", "corner cells cannot be covered"

assert run("""1
3 3
.#.
###
.#.
""") == """Image #1: 1

""", "single legal stamp"

max_empty = "9 9\n" + "\n".join(["........."] * 9) + "\n"
assert run("1\n" + max_empty) == """Image #1: 0

""", "maximum-size empty image"
```Trường hợp tùy chỉnh đầu tiên kiểm tra loại giấy nhỏ nhất có thể và xác minh rằng một hình ảnh hoàn toàn màu trắng không cần tem. Cái thứ hai sử dụng cùng kích thước với một ô màu đen, kiểm tra sự khác biệt giữa mục tiêu trống và mục tiêu không thể thực hiện được. Cách thứ ba mắc phải sai lầm phổ biến là chỉ kiểm tra xem bản thân tem có hợp pháp hay không, bởi vì lưới 3 x 3 toàn màu đen có tâm hợp lệ nhưng lại để hở các góc của nó. Thứ tư là việc dập hợp lệ không tầm thường nhỏ nhất và kiểm tra các điều kiện biên trung tâm. Bài tập thứ năm thực hiện tối đa 9 x 9 chiều trong khi vẫn giữ câu trả lời ở mức 0, đồng thời kiểm tra việc tạo mặt nạ và hành vi DP ở kích thước đầu vào lớn nhất được phép. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`với`.`|`Image #1: 0`| Kích thước tối thiểu và mục tiêu trống | 
|`1 1`với`#`|`Image #1: impossible`| Tem không vừa | 
|`3 3`với tất cả`#`|`Image #1: impossible`| Góc không thể che phủ | 
|`3 3`chéo |`Image #1: 1`| Giải pháp tem đơn chính xác | 
|`9 9`với tất cả`.`|`Image #1: 0`| Kích thước tối đa và xử lý không mặt nạ | 

## Vỏ cạnh 

Để có một hình ảnh hoàn toàn trắng, mỗi hàng mục tiêu đều có mặt nạ bằng 0. DP luôn có thể chọn không có tem và mỗi hàng đều vượt qua bài kiểm tra mức độ bao phủ vì`(0 & 0) == 0`. Ví dụ,```
1
1 1
.
```sản xuất`Image #1: 0`. Không có trường hợp đặc biệt nào được yêu cầu trong quá trình triển khai vì bộ tem trống được biểu thị một cách tự nhiên bằng mặt nạ số 0. 

Đối với hình ảnh màu đen nhỏ hơn tem thì không có vị trí trung tâm hợp pháp. Coi như```
1
1 1
#
```các`allowed`mảng chỉ chứa số không. Trạng thái DP duy nhất có vùng phủ sóng bằng 0, nhưng mục tiêu chứa một bit màu đen, do đó việc kiểm tra vùng phủ sóng sẽ loại bỏ nó. Tập trạng thái cuối cùng trống và câu trả lời trở thành`impossible`. 

Góc đen không bao giờ bị chữ thập che mất vì tem không có ô chéo. TRONG```
1
3 3
###
###
###
```trung tâm là hợp pháp, nhưng phạm vi bảo hiểm của nó là```
.#.
###
.#.
```Các bit góc vẫn vắng mặt. Khi DP kiểm tra hàng đầu tiên, tâm được chọn chỉ có thể che phần giữa, do đó mặt nạ mục tiêu không được chứa trong mặt nạ che phủ. Cấu hình bị từ chối trước khi nó có thể đến hàng cuối cùng. 

Một trung tâm hợp lệ cũng yêu cầu tất cả năm ô tem phải có màu đen. Ví dụ,```
1
3 3
...
.#.
...
```có một trung tâm màu đen nhưng không có tem hợp pháp, bởi vì đặt cây thánh giá sẽ tô màu bốn chấm. Trung tâm không được thêm vào`allowed`, nên DP không có cách nào sử dụng và báo cáo chính xác`impossible`. 

Tem chồng chéo không yêu cầu xử lý đặc biệt. Trong mẫu 3 x 5,```
.#.#.
#####
.#.#.
```hai chữ thập có thể được căn giữa ở hai cột màu đen bên trong. Một số ô được bao phủ bởi cả hai tem, nhưng mục tiêu chỉ quan tâm liệu ô đó có màu đen ít nhất một lần hay không. Liên kết bitwise được DP sử dụng sẽ tự nhiên loại bỏ mọi sự khác biệt giữa một và nhiều lớp phủ và mức tối thiểu là 2. 

Ranh giới hàng cuối cùng được xử lý bằng cách buộc`nxt`về không. Nếu không có hạn chế đó, quá trình chuyển đổi có thể sử dụng tâm giả định bên dưới tờ giấy có nhánh hướng lên bao phủ ô mục tiêu ở hàng cuối cùng. Nguyên tắc tương tự đã được áp dụng ở trên cùng cho đến trạng thái 0 ban đầu, vì vậy không con tem nào được phép vượt ra ngoài ranh giới dọc.
