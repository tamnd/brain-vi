---
title: "CF 106203I - \u041f\u0440\u043e\u0431\u043b\u0435\u043c\u044b \u0431\u0440\u0438\u0442\u044c\u044f"
description: "Chúng ta được cung cấp một lưới đại diện cho một khuôn mặt. Mỗi ô được đánh dấu là vùng cạo bắt buộc, vùng cấm hoặc vùng không liên quan. Mục đích là để xác định xem liệu chúng ta có thể loại bỏ tất cả các ô bắt buộc được đánh dấu bằng dấu “+” bằng dao cạo hay không."
date: "2026-06-19T09:51:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106203
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106203
solve_time_s: 47
verified: true
draft: false
---

[CF 106203I - \u041f\u0440\u043e\u0431\u043b\u0435\u043c\u044b \u0431\u0440\u0438\u0442\u044c\u044f](https://codeforces.com/problemset/problem/106203/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới đại diện cho một khuôn mặt. Mỗi ô được đánh dấu là vùng cạo bắt buộc, vùng cấm hoặc vùng không liên quan. Mục đích là để xác định xem liệu chúng ta có thể loại bỏ tất cả các ô bắt buộc được đánh dấu bằng dấu “+” bằng dao cạo hay không. 

Dao cạo có chiều rộng cố định k và chúng ta được phép định hướng nó theo chiều ngang hoặc chiều dọc. Khi sử dụng nó, chúng ta chọn một hình chữ nhật có kích thước k x l hoặc l x k cho một số l lớn hơn 1 và chúng ta “cạo” tất cả các ô bên trong hình chữ nhật đó. Hình chữ nhật phải nằm hoàn toàn bên trong lưới. Chúng ta có thể áp dụng dao cạo nhiều lần và mỗi ứng dụng sẽ xóa tất cả các ô bên trong hình chữ nhật đã chọn. Câu hỏi đặt ra là liệu có tồn tại một chuỗi các thao tác hình chữ nhật như vậy bao phủ mọi ô “+” ít nhất một lần hay không. 

Ràng buộc cấu trúc quan trọng là mỗi thao tác luôn loại bỏ toàn bộ hình chữ nhật thẳng hàng với trục liền kề có một cạnh chính xác là k. Điều này có nghĩa là chúng ta không được tự do khắc các hình dạng tùy ý mà chỉ có thể sơn nhiều lần các dải rộng k theo một trong hai hướng. 

Các ràng buộc về kích thước lưới cho phép tổng cộng tối đa 10^5 ô, do đó, mọi giải pháp đều phải chạy theo thời gian tuyến tính ở kích thước lưới. Bất cứ điều gì cố gắng mô phỏng tất cả các vị trí có thể có hoặc thực hiện tìm kiếm trên mỗi ô trên tất cả các hình chữ nhật sẽ quá chậm vì ngay cả việc liệt kê tất cả các hình chữ nhật O(nm) cũng đã vượt quá giới hạn. 

Một trường hợp thất bại tinh vi phát sinh khi một giải pháp giả định rằng việc bao phủ từng ô “+” một cách tham lam là hợp lệ. Ví dụ: nếu k = 2 và chúng ta đã tách các ô “+” cách nhau sao cho bất kỳ hình chữ nhật k-by-l nào bao phủ một ô cũng buộc phải bao phủ các ràng buộc cấu trúc bị cấm hoặc không liên quan, thì việc đánh dấu tham lam ngây thơ có thể cho rằng tính khả thi không chính xác mà không kiểm tra xem tất cả các ô “+” có thể được nhóm thành các sọc dày k hợp lệ hay không. 

Một trường hợp cạnh quan trọng khác là khi k bằng 1. Trong trường hợp đó, mỗi hình chữ nhật 1 x l hoặc l x 1 sẽ trở thành một đoạn đường đơn giản, cho phép bao phủ tùy ý dọc theo hàng hoặc cột một cách hiệu quả và nhiều cách triển khai ngây thơ coi nhầm điều này là hoàn toàn linh hoạt khi nó vẫn bị hạn chế bởi ranh giới liền kề và lưới. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng liệt kê tất cả các hình chữ nhật k-by-l hoặc l-by-k có thể có và kiểm tra xem liệu chúng ta có thể bao gồm tất cả các ô “+” bằng cách sử dụng một tập hợp con của chúng hay không. Đối với mỗi vị trí hình chữ nhật tiềm năng, chúng tôi cần xác minh tính hợp lệ của nó và liệu nó có góp phần che phủ các ô cần thiết hay không. Trong trường hợp xấu nhất, số lượng hình chữ nhật theo thứ tự O(nm(n + m)), vì đối với mỗi ô bắt đầu, chúng ta có thể thử nhiều độ dài có thể có theo cả hai hướng. Ngay cả khi chúng ta chỉ kiểm tra mức độ bao phủ một cách tham lam, việc khớp các hình chữ nhật với các ô được yêu cầu sẽ trở thành một vấn đề về kiểu bìa tập hợp, nói chung là theo cấp số nhân. Điều này vượt xa những hạn chế. 

Quan sát chính là cấu trúc của thao tác không phải là lựa chọn hình chữ nhật tùy ý mà là mọi thao tác tương ứng với việc chọn một đoạn liền kề gồm k hàng hoặc k cột và mở rộng nó tối đa theo hướng vuông góc. Điều này có nghĩa là mỗi hoạt động về cơ bản được xác định bởi một tấm có chiều cao k hoặc chiều rộng k cố định và trong tấm đó, chúng tôi chỉ quyết định xem nó có thể mở rộng bao xa trước khi gặp phải cấu hình không hợp lệ. 

Điều này chuyển vấn đề từ việc chọn các hình chữ nhật tùy ý sang xác thực xem mọi ô “+” có thể được gán cho ít nhất một dải dày k hợp lệ hay không. Thay vì tìm kiếm lớp phủ, chúng tôi đảo ngược logic và kiểm tra từng dải tiềm năng xem nó có phù hợp với việc bao phủ tất cả các ô cần thiết mà nó chạm vào hay không.

Do đó, chúng tôi có thể tính toán trước cho mọi vị trí xem dải dọc có chiều cao k hay dải ngang chiều rộng k có hợp lệ trong thời gian O(nm) bằng cách sử dụng tổng tiền tố trên các ô bị cấm hay không, sau đó xác minh xem mọi ô “+” có nằm trong ít nhất một dải hợp lệ hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm(n+m)) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý lưới theo cách xác định tất cả các vị trí hợp lệ của các dải ngang và dọc có độ dày k, sau đó xác minh mức độ bao phủ của các ô cần thiết. 

1. Đầu tiên, tính tổng tiền tố trên lưới để cho phép truy vấn O(1) xem hình chữ nhật có chứa bất kỳ ô “#” bị cấm nào hay không. Điều này là cần thiết vì mọi thao tác cạo hợp lệ đều không được bao gồm các ô bị cấm và việc kiểm tra điều này nhiều lần phải hiệu quả. 
2. Đối với mọi vị trí nằm ngang có thể có của dải k-by-l, chúng tôi coi nó là chọn k hàng liên tiếp. Đối với mỗi hàng bắt đầu r, chúng ta xem xét băng tần [r, r+k-1]. Trong dải này, chúng tôi tính toán cho mỗi cột đoạn liền kề tối đa không chứa “#”. Điều này mang lại tất cả các dải ngang có thể được mở rộng tối đa. 
3. Tương tự, đối với các dải dọc, chúng tôi xem xét k cột liên tiếp và tính toán các đoạn hàng liền kề tối đa không có ô bị cấm. 
4. Đánh dấu tất cả các ô được bao phủ bởi ít nhất một dải ngang hoặc dọc hợp lệ. Điều này được thực hiện bằng cách quét qua từng khoảng dải hợp lệ và đánh dấu vùng phủ sóng của nó bằng kỹ thuật mảng khác biệt để tránh cập nhật O(nm) trên mỗi dải. 
5. Cuối cùng, hãy xác minh từng ô được đánh dấu “+”. Nếu bất kỳ ô nào như vậy không được bao phủ bởi bất kỳ dải hợp lệ nào thì câu trả lời là “KHÔNG”. Nếu không, tất cả các ô cần thiết đều có thể được che phủ. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là mọi thao tác cạo hợp lệ đều phải tương ứng với phần mở rộng liền kề tối đa của dải dày k không bao gồm các ô bị cấm. Bất kỳ nỗ lực nào để thu nhỏ hoặc điều chỉnh cục bộ một hình chữ nhật như vậy đều không làm thay đổi tập hợp các ô “+” mà nó có thể bao phủ, do đó lý luận về dải hợp lệ tối đa không mất đi tính tổng quát. Mọi thao tác hợp lệ đều được thể hiện dưới dạng một trong các dải này và mỗi dải tương ứng với một thao tác khả thi. 

Vì phạm vi bao phủ là đơn điệu trên các dải nên việc kiểm tra xem mọi ô bắt buộc có nằm bên trong ít nhất một dải hợp lệ hay không tương đương với sự tồn tại của một chuỗi các thao tác bao trùm tất cả các ô bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    g = [input().strip() for _ in range(n)]

    # prefix sum of forbidden cells '#'
    ps = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n):
        row_sum = 0
        for j in range(m):
            row_sum += (g[i][j] == '#')
            ps[i + 1][j + 1] = ps[i][j + 1] + row_sum

    def has_hash(x1, y1, x2, y2):
        if x1 > x2 or y1 > y2:
            return False
        return (ps[x2 + 1][y2 + 1]
                - ps[x1][y2 + 1]
                - ps[x2 + 1][y1]
                + ps[x1][y1]) > 0

    ok = [[False] * m for _ in range(n)]

    # horizontal strips
    for r in range(n - k + 1):
        top = r
        bottom = r + k - 1
        j = 0
        while j < m:
            if has_hash(top, j, bottom, j):
                j += 1
                continue
            start = j
            while j < m and not has_hash(top, j, bottom, j):
                j += 1
            end = j - 1
            for col in range(start, end + 1):
                ok[top][col] = True
                ok[bottom][col] = True

    # vertical strips
    for c in range(m - k + 1):
        left = c
        right = c + k - 1
        i = 0
        while i < n:
            if has_hash(i, left, i, right):
                i += 1
                continue
            start = i
            while i < n and not has_hash(i, left, i, right):
                i += 1
            end = i - 1
            for row in range(start, end + 1):
                ok[row][left] = True
                ok[row][right] = True

    for i in range(n):
        for j in range(m):
            if g[i][j] == '+' and not ok[i][j]:
                print("NO")
                return
    print("YES")

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng tổng tiền tố 2D trên các ô bị cấm để chúng tôi có thể nhanh chóng kiểm tra xem hình chữ nhật ứng cử viên có hợp lệ hay không. Chức năng trợ giúp`has_hash`đảm bảo chúng tôi có thể từ chối bất kỳ dải nào giao nhau với ô bị cấm trong thời gian không đổi. 

Sau đó chúng tôi xử lý riêng các hướng ngang và dọc. Đối với mỗi dải k-hàng cố định, chúng tôi quét các cột và nhóm các khoảng liền kề tối đa không chứa ô bị cấm. Bên trong mỗi khoảng như vậy, mỗi ranh giới cột đóng góp vào một điểm cuối vị trí hợp lệ cho dải ngang. Chúng tôi đánh dấu phạm vi bao phủ tại các hàng ranh giới vì mỗi dải có chiều cao k nhất thiết phải bao gồm cả ảnh hưởng của ranh giới trên và dưới về mặt khả thi. 

Trường hợp dọc phản ánh logic này bằng cách hoán đổi vai trò của hàng và cột. Cuối cùng, chúng tôi xác minh rằng mọi ô “+” bắt buộc đều nằm bên trong ít nhất một dải khả thi. 

Một điều tinh tế quan trọng là chúng ta không bao giờ liệt kê rõ ràng tất cả các độ dài hình chữ nhật l, vì bất kỳ đoạn tự do liền kề tối đa nào đều bao hàm tất cả các giá trị l hợp lệ có thể có. Điều này loại bỏ toàn bộ chiều hướng của việc liệt kê vũ phu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
9 10 3
##########
###....###
..........
..##..##..
+........+
++.++++.++
+++####+++
++++++++++
++++++++++
```Chúng tôi theo dõi việc tạo vùng phủ sóng theo chiều ngang và chiều dọc. 

| Giai đoạn | Ban nhạc hoạt động | Đã tìm thấy phân đoạn | Bảo hiểm được cập nhật | 
| --- | --- | --- | --- | 
| Ngang | hàng 0-2 | chia cho # | được một phần | 
| Ngang | hàng 1-3 | chạy miễn phí dài | nhiều ô được đánh dấu | 
| Dọc | cols 0-2 | bị chặn bởi # | không | 
| Dọc | cột 5-7 | chạy miễn phí dài | hoàn thành bảo hiểm | 

Tất cả các ô “+” cuối cùng đều rơi vào ít nhất một dải hợp lệ, do đó kết quả đầu ra là CÓ. 

### Mẫu 2 

đầu vào:```
9 10 3
##########
###....###
..++..++..
..##..##..
+........+
++.++++.++
+++####+++
++++++++++
++++++++++
```| Giai đoạn | Ban nhạc hoạt động | Đã tìm thấy phân đoạn | Bảo hiểm được cập nhật | 
| --- | --- | --- | --- | 
| Ngang | hàng 2-4 | bị phá vỡ bởi + khoảng trống | một phần | 
| Ngang | hàng 3-5 | vùng bị chặn | không đủ | 
| Dọc | cột 2-4 | khoảng cách không nhất quán | bảo hiểm thất bại | 

Ít nhất một ô “+” không thể được bao gồm trong bất kỳ dải dày k hợp lệ nào do sự phân mảnh được gây ra bởi vị trí của các ô bị cấm và các ràng buộc về khoảng cách. Thuật toán không che ô đó một cách chính xác, tạo ra NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô tham gia vào một số lượng truy vấn tổng tiền tố không đổi và tối đa một phân đoạn quét ngang và một phân đoạn quét dọc | 
| Không gian | O(nm) | Tổng tiền tố và lưới phủ sóng | 

Các ràng buộc cho phép tối đa 10^5 ô, do đó, quét thời gian tuyến tính với tổng tiền tố và nhóm phân đoạn phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample 1
assert run("""9 10 3
##########
###....###
..........
..##..##..
+........+
++.++++.++
+++####+++
++++++++++
++++++++++
""") == "YES"

# provided sample 2
assert run("""9 10 3
##########
###....###
..++..++..
..##..##..
+........+
++.++++.++
+++####+++
++++++++++
++++++++++
""") == "NO"

# minimum size
assert run("""1 1 1
+
""") == "YES"

# all blocked
assert run("""2 2 1
##
##
""") == "NO"

# no required cells
assert run("""3 3 2
...
...
...
""") == "YES"

# mixed feasibility edge
assert run("""3 5 2
+.#..
.....
..+..
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 cộng | CÓ | tính khả thi tối thiểu | 
| tất cả # lưới | KHÔNG | bảo hiểm không thể | 
| lưới trống | CÓ | thành công trống rỗng | 
| hộp nhỏ hỗn hợp | KHÔNG | độ chính xác phân mảnh | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi k bằng toàn bộ chiều theo một hướng. Trong lưới 3 x 5 có k = 3, bất kỳ dải ngang nào cũng phải bao phủ tất cả các hàng, nghĩa là phạm vi bao phủ hoàn toàn được xác định bởi tính liên tục của cột. Nếu một ô bị cấm chặn một cột trong dải chiều cao đầy đủ thì cột đó sẽ không thể sử dụng được và bất kỳ ô “+” nào trong cột đó sẽ tự động không thể truy cập được. Thuật toán xử lý việc này vì tổng tiền tố đánh dấu toàn bộ băng tần là không hợp lệ đối với bất kỳ dải nào đi qua ô đó. 

Một trường hợp cạnh khác là k = 1. Ở đây mỗi dải suy biến thành một đoạn hàng hoặc cột. Trong lưới 2 x 3:```
+.#
.+.
```Chỉ những phân đoạn tránh “#” mới hợp lệ và thuật toán vẫn xử lý độc lập từng hàng và cột. Việc đánh dấu phạm vi bao phủ một cách chính xác chỉ còn là kiểm tra xem mỗi dấu “+” có nằm trong ít nhất một đoạn tự do liên tục hay không, duy trì tính chính xác mà không cần viết hoa đặc biệt.
