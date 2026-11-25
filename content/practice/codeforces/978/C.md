---
title: "CF 978C - Chữ cái"
description: "Chúng tôi được cấp một số ký túc xá được sắp xếp thành một hàng. Mỗi ký túc xá có một số phòng đã biết và tất cả các phòng trong tất cả các ký túc xá đều được nối thành một chuỗi dài về mặt khái niệm."
date: "2026-06-17T01:22:17+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 1000
weight: 978
solve_time_s: 72
verified: true
draft: false
---

[CF 978C - Thư](https://codeforces.com/problemset/problem/978/C) 

**Đánh giá:** 1000 
**Tags:** tìm kiếm nhị phân, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số ký túc xá được sắp xếp thành một hàng. Mỗi ký túc xá có một số phòng đã biết và tất cả các phòng trong tất cả các ký túc xá đều được nối thành một chuỗi dài về mặt khái niệm. Ký túc xá thứ nhất đóng góp số phòng trước, tiếp theo là phòng thứ hai, v.v., tạo thành một số liên tục duy nhất từ ​​1 đến tổng số phòng. 

Nhiệm vụ là trả lời các truy vấn trong đó mỗi truy vấn đưa ra chỉ mục phòng chung theo chuỗi kết hợp này. Đối với mỗi chỉ số như vậy, chúng ta cần xác định hai điều: ký túc xá nào chứa phòng đó và chỉ số cục bộ của phòng bên trong ký túc xá đó là gì. 

Kích thước đầu vào làm cho cấu trúc trở nên quan trọng. Cả số lượng ký túc xá và truy vấn có thể lên tới 200.000 và mỗi ký túc xá có thể có tới 10¹⁰ phòng. Điều này ngay lập tức loại trừ mọi cách tiếp cận cố gắng mô phỏng toàn bộ mảng phẳng hoặc quét tuyến tính cho từng truy vấn. Ngay cả việc quét ký túc xá cho mỗi truy vấn cũng dẫn đến các hoạt động 2×10⁵ × 2×10⁵ trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một trường hợp khó phát hiện xuất phát từ các giá trị biên của tổng tích lũy. Ví dụ, nếu ký túc xá có kích thước`[3, 5]`, khi đó các chỉ số toàn cầu từ 1 đến 3 ánh xạ tới ký túc xá 1 và 4 đến 8 ánh xạ tới ký túc xá 2. Một sai lầm ngây thơ là coi tổng tiền tố là các khoảng mở không nhất quán, dẫn đến các lỗi sai lệch như ánh xạ chỉ số 3 không chính xác vào ký túc xá thứ hai nếu điều kiện biên không được xử lý cẩn thận. 

Một trường hợp đặc biệt khác là xử lý các truy vấn đến phòng cuối cùng của ký túc xá. Ví dụ: nếu số tiền tích lũy là`[10, 25, 37]`, sau đó truy vấn`10`phải sơ đồ tới ký túc xá thứ nhất chứ không phải ký túc xá thứ hai. Đây là nơi mà sự bất bình đẳng nghiêm ngặt và bất bình đẳng không nghiêm ngặt trở nên quan trọng. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ xử lý từng truy vấn một cách độc lập bằng cách đi qua các ký túc xá, trừ đi số phòng cho đến khi chỉ số còn lại vừa với một ký túc xá. Đối với một truy vấn duy nhất, đây là O(n). Qua m truy vấn, điều này trở thành O(nm), trong trường hợp xấu nhất đạt tới các hoạt động 4×10¹⁰ và không khả thi. 

Cấu trúc của vấn đề gợi ý một quan điểm tích lũy. Nếu chúng ta tính tổng tiền tố của kích thước ký túc xá thì mỗi ký túc xá tương ứng với một khoảng liền kề trên trục số. Mỗi truy vấn sau đó sẽ trở thành một bài toán vị trí điểm: tìm tổng tiền tố đầu tiên lớn hơn hoặc bằng giá trị truy vấn. 

Đây chính xác là một tìm kiếm nhị phân trên một mảng đơn điệu. Vì tổng tiền tố ngày càng tăng nên chúng ta có thể xác định vị trí ký túc xá chính xác trong O(log n) cho mỗi truy vấn. Sau khi tìm chỉ số ký túc xá, số phòng địa phương có được bằng cách trừ đi tổng tiền tố trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tổng tiền tố + Tìm kiếm nhị phân | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của kích thước ký túc xá sao cho mỗi vị trí đại diện cho chỉ số toàn cầu cuối cùng của ký túc xá đó. Điều này chuyển vấn đề thành phạm vi tìm kiếm thay vì lặp lại cấu trúc. 
2. Với mỗi giá trị truy vấn`x`, thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố để tìm chỉ số nhỏ nhất`i`như vậy`prefix[i] >= x`. Điều này xác định ký túc xá chứa phòng vì`x`nằm trong khoảng tích lũy của nó. 
3. Từng là ký túc xá`i`được tìm thấy, hãy tính chỉ số phòng cục bộ. Nếu như`i`là ký túc xá đầu tiên, câu trả lời đơn giản là`x`. Ngược lại thì trừ`prefix[i-1]`từ`x`. 
4. Đầu ra`(i + 1, local_index)`vì ký túc xá được lập chỉ mục 1. 

Bước suy luận chính là mỗi ký túc xá chiếm một đoạn liền kề trên trục số, do đó việc xác định vị trí của một phòng tương đương với việc xác định đoạn nào chứa một điểm. 

### Tại sao nó hoạt động 

Mảng tổng tiền tố phân chia việc đánh số toàn cầu thành các khoảng rời rạc, liền kề trong đó mỗi khoảng tương ứng với chính xác một ký túc xá. Vì tổng tiền tố ngày càng tăng nên mỗi giá trị truy vấn rơi vào đúng một khoảng. Tìm kiếm nhị phân tìm thấy khoảng đó bằng cách khai thác tính đơn điệu và trừ đi ranh giới trước đó sẽ mang lại độ lệch chính xác bên trong khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    prefix = []
    s = 0
    for x in a:
        s += x
        prefix.append(s)

    import bisect

    out = []
    for x in b:
        i = bisect.bisect_left(prefix, x)
        prev = prefix[i - 1] if i > 0 else 0
        out.append(f"{i + 1} {x - prev}")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng tổng tiền tố trong một lần chuyển. Chi tiết triển khai quan trọng đang sử dụng`bisect_left`, tìm tổng tiền tố đầu tiên không nhỏ hơn truy vấn. Điều này xử lý chính xác các giá trị biên: nếu một truy vấn bằng tổng tiền tố một cách chính xác, thì truy vấn đó vẫn ánh xạ tới ký túc xá tương ứng chứ không phải ký túc xá tiếp theo. 

Bước trừ sử dụng tổng tiền tố trước đó một cách cẩn thận, kèm theo điều kiện cho ký túc xá đầu tiên để tránh lập chỉ mục âm. Đây là nơi nhiều giải pháp không chính xác thất bại do luôn trừ hoặc quên hoàn toàn trường hợp biên. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

đầu vào:```
n=3, m=6
a = [10, 15, 12]
b = [1, 9, 12, 23, 26, 37]
```Tổng tiền tố là`[10, 25, 37]`. 

| truy vấn x | kết quả tìm kiếm nhị phân i | tiền tố[i-1] | ký túc xá | chỉ mục địa phương | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 1 | 
| 9 | 0 | 0 | 1 | 9 | 
| 12 | 1 | 10 | 2 | 2 | 
| 23 | 1 | 10 | 2 | 13 | 
| 26 | 2 | 25 | 3 | 1 | 
| 37 | 2 | 25 | 3 | 12 | 

Dấu vết này cho thấy cách mỗi truy vấn được phân loại vào khoảng chính xác được xác định bằng tổng tiền tố. Nó cũng thể hiện việc xử lý chính xác các giá trị biên chính xác như 10, 25 và 37 một cách ngầm định thông qua hành vi tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Cấu trúc tiền tố là tuyến tính, mỗi truy vấn sử dụng tìm kiếm nhị phân trên mảng tiền tố | 
| Không gian | O(n) | Chỉ tổng tiền tố được lưu trữ | 

Điều này phù hợp thoải mái trong các ràng buộc vì n và m lên tới 2×10⁵ và tìm kiếm logarit trên phạm vi này là hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    prefix = []
    s = 0
    for x in a:
        s += x
        prefix.append(s)

    import bisect

    out = []
    for x in b:
        i = bisect.bisect_left(prefix, x)
        prev = prefix[i - 1] if i > 0 else 0
        out.append(f"{i + 1} {x - prev}")

    return "\n".join(out)

# provided sample
assert run("""3 6
10 15 12
1 9 12 23 26 37
""") == """1 1
1 9
2 2
2 13
3 1
3 12"""

# minimum input
assert run("""1 3
5
1 3 5
""") == """1 1
1 3
1 5"""

# boundary at exact prefix
assert run("""2 3
4 6
4 5 10
""") == """1 4
2 1
2 6"""

# all equal sizes
assert run("""4 4
2 2 2 2
1 2 3 8
""") == """1 1
1 2
2 1
4 2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký túc xá đơn | bản đồ trực tiếp | tính đúng đắn của trường hợp cơ sở | 
| lượt truy cập tiền tố chính xác | xử lý ranh giới đúng | an toàn từng người một | 
| kích thước bằng nhau | phân khúc thống nhất | logic khoảng nhất quán | 

## Vỏ cạnh 

Một trường hợp đặc biệt quan trọng là khi một truy vấn bằng tổng tiền tố một cách chính xác. Ví dụ: nếu kích thước ký túc xá là`[4, 6]`, tiền tố là`[4, 10]`và truy vấn là`4`. Tìm kiếm nhị phân trả về chỉ mục`0`và chỉ mục cục bộ trở thành`4 - 0 = 4`, ánh xạ chính xác đến phòng cuối cùng của ký túc xá đầu tiên. 

Một trường hợp đặc biệt khác là khi truy vấn rơi vào ký túc xá cuối cùng. Với tiền tố`[4, 10]`và truy vấn`9`, tìm kiếm nhị phân trả về chỉ mục`1`, tiền tố trước nó là`4`và chỉ mục cục bộ trở thành`5`, xác định chính xác ký túc xá thứ hai. 

Những trường hợp này xác nhận rằng việc coi mỗi ký túc xá là một khoảng đóng trên ranh giới tiền tố kết hợp với`bisect_left`mang lại sự phân loại chính xác và ổn định.
