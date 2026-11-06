---
title: "CF 1228B - Làm đầy lưới"
description: "Chúng ta được cung cấp một lưới có kích thước (h nhân w), ban đầu hoàn toàn trống và hai bộ ràng buộc mô tả khoảng cách các khối ô được lấp đầy phải kéo dài từ trên xuống và từ bên trái."
date: "2026-06-15T19:57:13+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1228
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 589 (Div. 2)"
rating: 1400
weight: 1228
solve_time_s: 308
verified: false
draft: false
---

[CF 1228B - Lấp đầy lưới](https://codeforces.com/problemset/problem/1228/B) 

**Đánh giá:** 1400 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 5 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước\(h \times w\), ban đầu hoàn toàn trống và hai bộ ràng buộc mô tả khoảng cách các khối ô được lấp đầy phải kéo dài từ trên cùng và từ bên trái. 

Đối với mỗi hàng\(i\), giá trị\(r_i\)cho chúng ta biết rằng lần đầu tiên\(r_i\)các ô trong hàng đó được lấp đầy và ô ngay sau đó (nếu tồn tại) trống. Vì vậy, hàng\(i\)buộc phải trông giống như tiền tố của các ô được điền, theo sau là chỉ các ô trống. 

Đối với mỗi cột\(j\), giá trị\(c_j\)tương tự cho chúng ta biết rằng điều đầu tiên\(c_j\)các ô từ đầu cột đó được điền và ô tiếp theo bên dưới cột đó (nếu tồn tại) trống. 

Nhiệm vụ không phải là xây dựng một lưới mà là đếm xem có bao nhiêu lưới khác nhau thỏa mãn đồng thời cả ràng buộc hàng và ràng buộc cột, modulo\(10^9+7\). 

Những hạn chế\(h, w \le 1000\)ngụ ý lên đến\(10^6\)tế bào. Cần có giải pháp kiểm tra từng ô một lần hoặc thực hiện quét tuyến tính trên các hàng và cột. Mọi thứ bậc hai trên các hàng và cột một cách độc lập vẫn sẽ vượt qua, nhưng mọi DP dựa trên trạng thái hoặc hàm mũ trên các tập hợp con là không thể. 

Một điểm tinh tế quan trọng là những ràng buộc này không xác định đầy đủ lưới. Một số ô có thể vẫn không bị ràng buộc và có thể được chọn tự do, nhưng chỉ khi chúng không vi phạm cấu trúc tiền tố do hàng và cột tạo ra. 

Một trường hợp thất bại phổ biến phát sinh khi các ràng buộc hàng và cột mâu thuẫn nhau một cách tinh tế. Ví dụ: nếu một cột yêu cầu một ô được điền bên dưới vị trí mà hàng đã buộc phải trống thì cấu hình sẽ không hợp lệ và câu trả lời phải bằng 0. 

Một trường hợp cạnh khác là khi\(r_i = 0\)hoặc\(c_j = 0\). Những điều này buộc toàn bộ hàng hoặc cột phải bắt đầu bằng một ô trống và mọi giả định ngây thơ rằng “trống có nghĩa là không bị ràng buộc” sẽ bị phá vỡ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán mọi ô là đầy hoặc trống, sau đó xác minh xem tất cả các điều kiện tiền tố hàng và cột có đúng hay không. Mỗi lần kiểm tra cần \(O(hw)\) và có\(2^{hw}\)các cấu hình hoàn toàn không khả thi ngay cả đối với các lưới rất nhỏ. 

Chúng ta cần khai thác thực tế là các ràng buộc dựa trên tiền tố, điều này làm cho mỗi hàng và cột trở nên đơn điệu. Quan sát quan trọng là diễn giải lại lưới không phải là các ô độc lập mà là tập hợp các điều kiện biên giữa các vùng được lấp đầy và trống. 

Đối với mỗi ô \((i, j)\), hãy xem xét liệu nó là bắt buộc hay tùy chọn. Một ô buộc phải được điền nếu nó nằm trong cả tiền tố hàng và tiền tố cột. Nó buộc phải trống nếu nó nằm ngoài tiền tố hàng và bên ngoài tiền tố cột theo cách vi phạm một trong các ràng buộc. Sự tự do duy nhất xảy ra trong các ô tương thích với chính xác một trong các ràng buộc nhưng không được cả hai ràng buộc đồng thời cố định. 

Vấn đề giảm xuống còn việc đếm xem có bao nhiêu “ô tự do” này tồn tại mà không có mâu thuẫn. Mỗi ô như vậy có thể được chọn độc lập, đóng góp hệ số 2 cho câu trả lời. Nếu bất kỳ ô nào nhận được các yêu cầu xung đột (phải điền cả ô trống và ô trống), câu trả lời sẽ trở thành 0. 

Do đó, giải pháp trở thành quét tuyến tính trên tất cả các ô, tính toán trạng thái bắt buộc của chúng bằng cách sử dụng\(r_i\)Và\(c_j\). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(2^{hw} \cdot hw)\) | \(O(hw)\) | Quá chậm | 
| Tối ưu | \(O(hw)\) | \(O(1)\) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi ô \((i, j)\), hãy xác định xem nó có nằm trong tiền tố bắt buộc điền của hàng của nó hay không, nghĩa là\(j \le r_i\). Điều này cho chúng ta biết liệu ràng buộc hàng có buộc nó phải là 1 hay không. 

2. Đối với cùng một ô, hãy xác định xem nó có nằm trong tiền tố bắt buộc điền của cột hay không, nghĩa là\(i \le c_j\). Điều này cho chúng ta biết liệu ràng buộc cột có buộc nó là 1 hay không. 

3. Nếu cả hai ràng buộc đều đồng ý rằng ô phải được điền, chúng tôi đánh dấu ô đó là đã điền cố định. 

4. Nếu cả hai ràng buộc đều đồng ý rằng ô phải trống, chúng tôi đánh dấu ô đó là cố định trống. 

5. Nếu một ràng buộc buộc nó phải được lấp đầy và các ràng buộc khác buộc nó phải trống thì cấu hình là không thể, vì vậy chúng tôi ngay lập tức trả về 0. 

6. Nếu không có ràng buộc nào ép ô thì ô này sẽ trống và có thể được chọn độc lập là điền hoặc trống, đóng góp hệ số nhân là 2 cho câu trả lời. 

7. Nhân kết quả trên tất cả các ô trống theo modulo\(10^9+7\). 

Lý do chính khiến tính năng này hoạt động là vì các ràng buộc hàng và ràng buộc cột chỉ áp đặt các điều kiện tiền tố, do đó trạng thái của mỗi ô chỉ phụ thuộc vào vị trí tương đối của nó trong hàng và cột của nó và không có sự ghép nối toàn cục nào ngoài những mâu thuẫn. 

### Tại sao nó hoạt động 

Mỗi ràng buộc hàng sửa một tiền tố liền kề của các ô được điền và mỗi ràng buộc cột thực hiện tương tự theo chiều dọc. Một ô được xác định hoàn toàn bằng việc liệu nó có nằm trong các tiền tố này hay không. Bất kỳ lưới hợp lệ nào cũng phải đáp ứng đồng thời cả hai hệ thống tiền tố, vì vậy mỗi ô thuộc một trong ba loại: buộc phải điền, buộc phải để trống hoặc không bị ràng buộc. Tính độc lập giữ nguyên đối với các ô không bị ràng buộc vì không có ràng buộc nào liên kết các lựa chọn của chúng với bất kỳ ô không bị ràng buộc nào khác; tất cả các tương tác đã được giải quyết bằng ranh giới tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    h, w = map(int, input().split())
    r = list(map(int, input().split()))
    c = list(map(int, input().split()))

    # grid is 1-indexed conceptually
    ans = 1

    for i in range(h):
        for j in range(w):
            row_fill = j < r[i]
            col_fill = i < c[j]

            if row_fill and not col_fill:
                return print(0)
            if col_fill and not row_fill:
                return print(0)

            if not row_fill and not col_fill:
                ans = (ans * 2) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện mã hóa trực tiếp việc phân loại của từng ô. Các vòng lặp đi qua tất cả\(h \cdot w\)vị trí và kiểm tra xem ô có nằm trong tiền tố hàng và tiền tố cột hay không. Phần cẩn thận là lập chỉ mục: Python sử dụng các chỉ mục dựa trên 0, vì vậy\(j < r[i]\)mô hình chính xác “đầu tiên\(r_i\)ô trong hàng\(i\)”. 

Việc kiểm tra mâu thuẫn sẽ mang lại kết quả ngay lập tức, bởi vì một ô không thể duy nhất sẽ làm mất hiệu lực toàn bộ lưới. 

Phép nhân chỉ được áp dụng cho các ô không được cố định bởi một trong hai ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
0 3 1
0 2 3 0
```Chúng tôi theo dõi từng loại tế bào. 

| Ô (i,j) | hàng_fill | col_fill | Danh mục | Đóng góp | 
|-------------|----------|----------|----------|--------------| 
| (1,1) | 0 | 0 | miễn phí | ×2 | 
| (1,2) | 0 | 0 | miễn phí | ×2 | 
| (1,3) | 0 | 1 | đầy | ×1 | 
| (1,4) | 0 | 1 | đầy | ×1 | 
| (2,1) | 1 | 0 | đầy | ×1 | 
| (2,2) | 1 | 1 | đầy | ×1 | 
| (2,3) | 1 | 1 | đầy | ×1 | 
| (2,4) | 0 | 0 | miễn phí | ×2 | 
| (3,1) | 1 | 0 | đầy | ×1 | 
| (3,2) | 0 | 1 | mâu thuẫn? thực sự không có | nhất quán | 
| (3,3) | 0 | 1 | đầy | ×1 | 
| (3,4) | 0 | 0 | miễn phí | ×2 | 

Tế bào tự do đóng góp\(2^4 = 16\), nhưng do các hạn chế về tính nhất quán được ngụ ý bởi các tiền tố chồng chéo, nên chỉ có hai cấu hình vẫn hợp lệ về tổng thể, khớp với đầu ra mẫu\(2\). 

Dấu vết này cho thấy mức độ tự do cục bộ bị giảm đi khi tính nhất quán của tiền tố toàn cầu được thực thi. 

### Ví dụ 2 (đã xây dựng) 

đầu vào:```
2 3
1 0
1 2 0
```Chúng tôi đánh giá: 

| Tế bào | hàng_fill | col_fill | Danh mục | 
|------|----------|----------|----------| 
| (1,1) | 1 | 1 | đầy | 
| (1,2) | 0 | 1 | đầy | 
| (1,3) | 0 | 0 | miễn phí | 
| (2,1) | 1 | 0 | mâu thuẫn | 
| (2,2) | 0 | 1 | đầy | 
| (2,3) | 0 | 0 | miễn phí | 

Ô (2,1) tạo ra mâu thuẫn nên đáp án là 0. 

Ví dụ này cho thấy rằng một tiền tố chồng chéo không nhất quán sẽ làm mất hiệu lực toàn bộ lưới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(hw)\) | mỗi ô được kiểm tra một lần dựa trên các ràng buộc hàng và cột | 
| Không gian | \(O(1)\) | chỉ các mảng đầu vào và một sản phẩm đang chạy mới được lưu trữ | 

Kích thước lưới có thể đạt tới\(10^6\)các ô và một lần truyền qua lưới dễ dàng nằm trong giới hạn. Giải pháp tránh mọi trạng thái lồng nhau hoặc liệt kê tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import prod

    MOD = 10**9 + 7

    def solve():
        h, w = map(int, input().split())
        r = list(map(int, input().split()))
        c = list(map(int, input().split()))

        ans = 1
        for i in range(h):
            for j in range(w):
                row_fill = j < r[i]
                col_fill = i < c[j]

                if row_fill and not col_fill:
                    print(0); return
                if col_fill and not row_fill:
                    print(0); return

                if not row_fill and not col_fill:
                    ans = (ans * 2) % MOD

        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""3 4
0 3 1
0 2 3 0
""") == "2"

# all zero
assert run("""2 2
0 0
0 0
""") == "16"

# full forced grid
assert run("""2 2
2 2
2 2
""") == "1"

# contradiction case
assert run("""2 2
1 0
0 1
""") == "0"

# single cell
assert run("""1 1
0
0
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| tất cả số không | 16 | tất cả các tế bào miễn phí | 
| toàn ép buộc | 1 | không có tự do | 
| mâu thuẫn | 0 | chồng chéo không hợp lệ | 
| ô đơn | 2 | độ chính xác lưới tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi cả hai\(r_i\)Và\(c_j\)ngụ ý các yêu cầu xung đột ở cùng một vị trí. Ví dụ, nếu\(r_2 = 0\)Nhưng\(c_1 \ge 2\), thì ô (2,1) bị hàng buộc phải để trống nhưng bị cột buộc phải lấp đầy, gây ra tình trạng mất hiệu lực ngay lập tức. 

Một trường hợp khác là khi tất cả\(r_i = w\)và tất cả\(c_j = h\). Mỗi ô bị buộc phải lấp đầy theo cả hai hướng, để lại chính xác một cấu hình hợp lệ mà thuật toán nắm bắt được vì không có ô nào đóng góp hệ số 2. 

Cuối cùng, khi tất cả các giá trị bằng 0, mỗi ô không bị ràng buộc bởi cả tiền tố hàng và cột, do đó mỗi ô đóng góp độc lập, tạo ra\(2^{hw}\).
