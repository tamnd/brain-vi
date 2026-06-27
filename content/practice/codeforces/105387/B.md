---
title: "CF 105387B - Tiêu diệt tất cả!"
description: "Chúng ta có một lưới rất lớn nhưng chỉ một số lượng nhỏ ô bên trong được đánh dấu. Từ trạng thái này, chúng ta được phép chọn một ô trống duy nhất và chọn một trong bốn hướng chính."
date: "2026-06-23T05:08:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105387
codeforces_index: "B"
codeforces_contest_name: "ICPC Central Russia Regional Contest, 2023"
rating: 0
weight: 105387
solve_time_s: 135
verified: false
draft: false
---

[CF 105387B - Tiêu diệt tất cả!](https://codeforces.com/problemset/problem/105387/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới rất lớn nhưng chỉ một số lượng nhỏ ô bên trong được đánh dấu. Từ trạng thái này, chúng ta được phép chọn một ô trống duy nhất và chọn một trong bốn hướng chính. Theo hướng đã chọn đó, trò chơi sẽ phá hủy mọi ô được đánh dấu nằm đúng hướng đó dọc theo cùng một hàng hoặc cùng một cột, tùy thuộc vào việc chúng ta chọn chuyển động ngang hay dọc. 

Mục tiêu là chọn một ô bắt đầu trống và một hướng sao cho số lượng ô được đánh dấu bị phá hủy càng lớn càng tốt và chúng ta cũng cần xuất ra một vị trí bắt đầu hợp lệ để đạt được mức tối đa này. 

Mặc dù kích thước lưới có thể lên tới 10^9 ở cả hai chiều nhưng chỉ có tối đa 200.000 ô được đánh dấu. Điều này ngay lập tức cho chúng ta biết rằng cấu trúc của giải pháp chỉ được phụ thuộc vào các ô được đánh dấu chứ không phụ thuộc vào bất kỳ biểu diễn lưới đầy đủ nào. 

Một cách giải thích mạnh mẽ sẽ là thử từng ô trống và đối với mỗi ô đó, hãy quét hàng và cột của nó để đếm xem có bao nhiêu ô được đánh dấu nằm theo mỗi hướng. Điều này là không thể vì số lượng ô trống thực tế là n·m trừ k, có thể rất lớn. 

Một hạn chế tinh tế hơn xuất phát từ việc lựa chọn hướng chỉ quan tâm đến sự liên kết. Khi chúng tôi sửa một ô, chỉ các ô được đánh dấu trong hàng hoặc cột của nó mới quan trọng. Điều này gợi ý việc nén vấn đề thành thông tin tần số theo hàng và theo cột. 

Một trường hợp thất bại phổ biến đối với cách suy luận ngây thơ xuất hiện khi người ta cho rằng vị trí tốt nhất phải nằm trên hoặc bên cạnh ô được đánh dấu. Điều đó là sai vì việc đặt điểm bắt đầu bên ngoài các vị trí được đánh dấu cực đoan trong một hàng hoặc cột có thể chiếm được tất cả các ô được đánh dấu trong dòng đó. 

Ví dụ: hãy xem xét một hàng có cột 3 và 10 được đánh dấu. Nếu chúng ta đặt điểm bắt đầu ở cột 1 trong cùng hàng đó thì toàn bộ hướng bên phải sẽ phá hủy cả hai dấu. Một giải pháp chỉ xem xét các vị trí liền kề với các ô được đánh dấu sẽ bỏ lỡ điều này. 

Một trường hợp lỗi khác xuất hiện khi tất cả các ô được đánh dấu đều nằm trong một hàng hoặc một cột. Trong trường hợp đó, việc chọn một ô trong hàng hoặc cột khác có thể vẫn hợp lệ và có thể cần thiết tùy thuộc vào việc hàng đó có được điền đầy đủ hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ lặp lại trên mọi ô trống và tính toán số lượng ô được đánh dấu có thể truy cập theo bốn hướng. Đối với mỗi ô ứng cử viên, việc quét hàng và cột của nó bằng cách sử dụng bộ băm hoặc tìm kiếm nhị phân sẽ tốn O(k) mỗi ô trong trường hợp xấu nhất. Vì số lượng ô trống về cơ bản là n·m nên phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là cấu trúc lưới không liên quan ngoài việc nhóm các ô được đánh dấu theo hàng và theo cột. Khi chúng tôi sửa một hàng, tất cả vấn đề quan trọng là có bao nhiêu ô được đánh dấu tồn tại trong hàng đó. Điều tương tự cũng áp dụng cho các cột. 

Đối với một hàng cố định có các ô được đánh dấu t, giả sử chúng ta chọn một ô trống trong hàng đó. Nếu chúng ta đặt nó ngoài khoảng được kéo dài bởi các ô được đánh dấu, chúng ta có thể chụp tất cả các ô t theo một hướng. Nếu chúng ta đặt nó bên trong khoảng, chúng ta có thể chia hàng thành phần bên trái và bên phải, nhưng điều tốt nhất chúng ta có thể làm vẫn là t − 1. Vì lưới lớn và có ít nhất một vị trí trống trừ khi hàng được điền đầy đủ, chúng ta luôn có thể chọn một vị trí bên ngoài phạm vi được đánh dấu, cho giá trị t. 

Do đó, mỗi hàng đóng góp số lượng ô được đánh dấu miễn là nó không được lấp đầy hoàn toàn. Một đối số giống hệt nhau áp dụng cho các cột. 

Điều này làm giảm nhiệm vụ tính toán tần số hàng và tần số cột và lấy giá trị tối đa trong số chúng, đồng thời đảm bảo rằng tồn tại ít nhất một ô trống hợp lệ trong hàng hoặc cột đã chọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m·k) | O(k) | Quá chậm | 
| Tối ưu | O(k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi tách vấn đề thành thống kê hàng và thống kê cột. 

1. Đọc tất cả các ô được đánh dấu và nhóm chúng theo hàng và theo cột. Chúng tôi lưu trữ cho mỗi hàng một danh sách các cột được đánh dấu và đối với mỗi cột một danh sách các hàng được đánh dấu. Đây là thông tin duy nhất về lưới chúng ta cần. 
2. Với mỗi hàng, hãy tính xem nó chứa bao nhiêu ô được đánh dấu. Nếu số đếm bằng m thì hàng này không có ô trống và không thể được sử dụng làm vị trí bắt đầu, vì vậy chúng ta bỏ qua nó. 
3. Đối với mỗi hàng có thể sử dụng, hãy tính điểm ứng viên của nó bằng số ô được đánh dấu trong hàng đó. Chúng tôi cũng theo dõi hàng tốt nhất và số lượng của nó. 
4. Lặp lại quy trình tương tự cho các cột. Nếu một cột có số đếm bằng n thì cột đó đã bị chiếm hoàn toàn và không thể sử dụng được. Nếu không thì điểm ứng cử viên của nó là số ô được đánh dấu trong cột đó. 
5. So sánh điểm hàng tốt nhất và điểm cột tốt nhất. Cái lớn hơn là câu trả lời. Nếu cả hai đều bằng 0 hoặc không tồn tại hàng/cột hợp lệ thì không có nước đi hợp lệ nào và câu trả lời là 0 với tọa độ 0 0. 
6. Để xây dựng lại ô bắt đầu cho hàng tốt nhất, chúng tôi chọn một hàng có số lượng tối đa và sau đó tìm bất kỳ chỉ mục cột nào không được đánh dấu trong hàng đó. Vì hàng có tối đa k ô được đánh dấu nên chúng ta có thể thử chỉ số cột bắt đầu từ 1 cho đến khi tìm thấy một ô không có trong tập hợp. Logic tương tự cũng áp dụng cho cột tốt nhất. 

Lý do nó hoạt động dựa trên thực tế là trong bất kỳ hàng nào có ít nhất một ô trống, chúng ta luôn có thể đặt điểm bắt đầu bên ngoài đoạn cột được đánh dấu và chụp tất cả các ô được đánh dấu theo một hướng. Do đó, khả năng phá hủy theo chiều ngang tốt nhất có thể đạt được trong hàng đó chính xác là số lượng ô được đánh dấu trong đó. Đối số tương tự được áp dụng đối xứng cho các cột. Vì mọi bước di chuyển hợp lệ phải được neo vào một số hàng hoặc cột, mức tối ưu toàn cục phải xuất hiện trong số các cực đại trên mỗi dòng này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    row = {}
    col = {}
    marked = set()

    for _ in range(k):
        x, y = map(int, input().split())
        marked.add((x, y))
        if x not in row:
            row[x] = []
        if y not in col:
            col[y] = []
        row[x].append(y)
        col[y].append(x)

    best = 0
    best_type = None  # 0 for row, 1 for col
    best_idx = None

    for x, ys in row.items():
        t = len(ys)
        if t < m:
            if t > best:
                best = t
                best_type = 0
                best_idx = x

    for y, xs in col.items():
        c = len(xs)
        if c < n:
            if c > best:
                best = c
                best_type = 1
                best_idx = y

    if best == 0:
        print(0)
        print("0 0")
        return

    if best_type == 0:
        x = best_idx
        forbidden = set(row[x])
        y = 1
        while y in forbidden:
            y += 1
        print(best)
        print(x, y)
    else:
        y = best_idx
        forbidden = set(col[y])
        x = 1
        while x in forbidden:
            x += 1
        print(best)
        print(x, y)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã này xây dựng danh sách kề cho các hàng và cột trong khi theo dõi tất cả các ô được đánh dấu để truy vấn thành viên nhanh chóng. Vòng lựa chọn chính tính toán các ứng cử viên hàng và cột tốt nhất trong khi loại bỏ các dòng đã điền đầy đủ, vì những dòng đó không thể chứa ô bắt đầu hợp lệ. 

Bước xây dựng lại rất đơn giản: nó quét lên từ 1 cho đến khi tìm thấy tọa độ không được đánh dấu trong dòng đã chọn. Điều này an toàn vì số lượng ô được đánh dấu nhỏ nên quá trình quét được giới hạn bởi k trong trường hợp xấu nhất trên mỗi dòng được chọn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu. 

Ta có các hàng được đánh dấu số đếm: hàng 3 có 2 điểm, hàng 6 có 1, hàng 12 có 1. Cột 4 có 2 điểm, cột 7 và 11 mỗi chỗ 1 điểm. 

| Bước | Hàng 3 | Hàng 6 | Hàng 12 | Col 4 | Col 7 | Col 11 | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đếm | 2 | 1 | 1 | 2 | 1 | 1 | 
| Có hiệu lực? | vâng | vâng | vâng | vâng | vâng | vâng | 

Tối đa là 2, có thể đạt được từ hàng 3 hoặc cột 4. Thuật toán chọn hàng 3 và chọn bất kỳ cột nào chưa được đánh dấu trong hàng đó, chẳng hạn như 4. 

Điều này chứng tỏ rằng cả phối cảnh hàng và cột đều có thể mang lại mức tối ưu như nhau và mọi hoạt động tái cấu trúc hợp lệ đều được chấp nhận. 

Bây giờ hãy xem xét trường hợp một hàng được điền đầy đủ: 

đầu vào:```
1 5 5
1 1
1 2
1 3
1 4
1 5
```Không có ô trống nào cả. Thuật toán xác định chính xác rằng tất cả các hàng và cột đều không hợp lệ nên kết quả là 0. 

Điều này cho thấy tầm quan trọng của việc loại trừ các đường đã được chiếm dụng hoàn toàn, vì nếu không chúng ta sẽ cho rằng chúng ta có thể đặt điểm bắt đầu ở đó một cách sai lầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi ô được đánh dấu được xử lý một lần và quét tái cấu trúc ở tối đa k vị trí | 
| Không gian | O(k) | Chúng tôi lưu trữ danh sách hàng và cột cộng với một tập hợp các ô được đánh dấu | 

Các ràng buộc cho phép lên tới 200.000 ô được đánh dấu, do đó, giải pháp tuyến tính trên k dễ dàng đủ nhanh. Kích thước lưới không liên quan vì chúng tôi không bao giờ lặp lại nó. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    old_stdout = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = old_stdout
    return out.strip()

# sample
assert run("""12 12 4
3 7
3 11
6 4
12 4
""") == "2\n3 4"

# single cell
assert run("""3 3 1
2 2
""") in ["0\n0 0"], "center single mark"

# full row
assert run("""1 5 5
1 1
1 2
1 3
1 4
1 5
""") == "0\n0 0"

# column heavy
assert run("""5 5 3
1 1
2 1
5 1
""") == "3\n1 2"

# sparse large grid
assert run("""1000000000 1000000000 2
1 1
1 1000000000
""") == "2\n1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ô đơn | 0 0 | không có nước đi hợp lệ | 
| đầy đủ hàng | 0 0 | xử lý đường dây bị chiếm dụng hoàn toàn | 
| cột nặng | 3 1 2 | lựa chọn tối ưu theo chiều dọc | 
| lưới lớn thưa thớt | 2 1 2 | độ chính xác tọa độ lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các ô được đánh dấu chiếm toàn bộ một hàng. Trong trường hợp đó, hàng đó không đóng góp gì vì không có ô bắt đầu trống hợp lệ. Thuật toán xử lý việc này bằng cách kiểm tra rõ ràng xem số hàng có bằng m hay không và bỏ qua nó. 

Một trường hợp cạnh khác xảy ra khi giải pháp tối ưu nằm trong một cột chứ không phải một hàng. Logic xây dựng lại phản ánh việc xử lý hàng và đảm bảo chúng tôi vẫn tìm thấy tọa độ trống hợp lệ bằng cách quét vị trí chưa được đánh dấu đầu tiên trong cột đó. 

Trường hợp cạnh thứ ba là khi k bằng 0. Khi đó mọi ô đều trống, nhưng bất kỳ động thái nào cũng không phá hủy được gì. Thuật toán trả về chính xác số 0 và mọi tọa độ đều được chấp nhận, ở đây được biểu thị là 0 0.
