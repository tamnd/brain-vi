---
title: "CF 1722E - Đếm hình chữ nhật"
description: "Chúng ta có một tập hợp các hình chữ nhật được căn chỉnh theo trục, mỗi hình chữ nhật được xác định bởi chiều cao và chiều rộng của nó. Đối với mỗi truy vấn, chúng ta cũng có hai hình chữ nhật bao quanh: hình chữ nhật “ràng buộc bên trong nhỏ” và hình chữ nhật “ràng buộc bên ngoài lớn”."
date: "2026-06-15T01:29:56+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1722
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 817 (Div. 4)"
rating: 1600
weight: 1722
solve_time_s: 294
verified: false
draft: false
---

[CF 1722E - Đếm hình chữ nhật](https://codeforces.com/problemset/problem/1722/E) 

**Đánh giá:** 1600 
**Tags:** bạo lực, cấu trúc dữ liệu, dp, triển khai 
**Thời gian giải:** 4 phút 54 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình chữ nhật được căn chỉnh theo trục, mỗi hình chữ nhật được xác định bởi chiều cao và chiều rộng của nó. Đối với mỗi truy vấn, chúng ta cũng có hai hình chữ nhật bao quanh: hình chữ nhật “ràng buộc bên trong nhỏ” và hình chữ nhật “ràng buộc bên ngoài lớn”. 

Đối với truy vấn có tham số$(h_s, w_s, h_b, w_b)$, chúng ta chỉ phải xét những hình chữ nhật đó$(h_i, w_i)$sao cho hình chữ nhật hoàn toàn lớn hơn ràng buộc nhỏ ở cả hai chiều và nhỏ hơn hoàn toàn so với ràng buộc lớn ở cả hai chiều. Trong số các hình chữ nhật hợp lệ đó, chúng ta tính tổng diện tích của chúng$h_i \cdot w_i$. 

Vì vậy, mỗi truy vấn đều yêu cầu bộ lọc phạm vi 2D trên các hình chữ nhật, sau đó là tổng hợp các khu vực của chúng. 

Khó khăn chính là cả hai$n$Và$q$có thể lớn như$10^5$, và có tới 100 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào kiểm tra từng hình chữ nhật cho mỗi truy vấn, vì điều đó sẽ yêu cầu tối đa$10^{10}$hoạt động trong trường hợp xấu nhất. 

Ràng buộc cấu trúc bổ sung rất quan trọng: chiều cao và chiều rộng đều bị giới hạn bởi 1000. Điều này có nghĩa là không gian tọa độ nhỏ và rời rạc, mặc dù số lượng hình chữ nhật lớn. Điều đó gợi ý rõ ràng về việc tính toán trước trên một lưới cố định. 

Một sai lầm ngây thơ nhưng tinh tế là thử sắp xếp các hình chữ nhật và thực hiện tìm kiếm nhị phân một cách độc lập theo chiều cao và chiều rộng. Cách tiếp cận đó không thành công vì các ràng buộc không thể tách rời: một hình chữ nhật có thể thỏa mãn điều kiện về chiều cao nhưng không thỏa mãn điều kiện về chiều rộng và ngược lại. Ví dụ: một hình chữ nhật có$h=10, w=1$và cái khác với$h=1, w=10$cả hai đều vượt qua bộ lọc một chiều nhưng không vượt qua bộ lọc 2D kết hợp. Bất kỳ cách tiếp cận nào xử lý các kích thước một cách độc lập mà không có cấu trúc khớp sẽ tạo ra kết quả không chính xác. 

Một trường hợp khác là xử lý bình đẳng. Vì yêu cầu bất đẳng thức nghiêm ngặt nên hình chữ nhật có chiều cao bằng$h_s$hoặc$h_b$phải được loại trừ. Một lỗi phổ biến là vô tình biến điều này thành giới hạn bao gồm, đặc biệt là khi dịch sang tổng tiền tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: đối với mỗi truy vấn, lặp lại trên tất cả các hình chữ nhật và kiểm tra xem cả chiều cao và chiều rộng có nằm đúng trong giới hạn truy vấn hay không. Nếu vậy, hãy thêm diện tích của nó vào câu trả lời. Điều này đúng vì nó trực tiếp tuân theo định nghĩa về tính hợp lệ. 

Tuy nhiên, chi phí này$O(n)$mỗi truy vấn, dẫn đến$O(nq)$tổng thể. Với$n = q = 10^5$, điều này trở nên quá lớn. 

Quan sát quan trọng là kích thước hình chữ nhật nhỏ. Cả chiều cao và chiều rộng đều nằm trong$[1, 1000]$, chỉ cho$10^6$các cặp có thể. Thay vì coi hình chữ nhật là một danh sách phi cấu trúc, chúng ta có thể tổng hợp chúng thành bảng tần số 2D. 

Khi chúng tôi thực hiện điều đó, mỗi truy vấn sẽ trở thành truy vấn tổng hình chữ nhật trên lưới 2D. Đây là một bài toán tổng tiền tố cổ điển: nếu chúng ta tính toán trước tổng tiền tố 2D của tổng đóng góp diện tích thì mỗi truy vấn có thể được trả lời trong thời gian không đổi bằng cách sử dụng loại trừ bao gồm. 

Ý tưởng là để lưu trữ cho mỗi ô$(h, w)$, tổng diện tích được đóng góp bởi tất cả các hình chữ nhật có chính xác kích thước đó. Sau đó xây dựng tổng tiền tố 2D trên lưới này để chúng ta có thể truy vấn tổng trên bất kỳ vùng hình chữ nhật nào trong O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Tổng tiền tố 2D |$O(n + 1000^2 + q)$|$O(1000^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác thực tế là chiều cao và chiều rộng được giới hạn bởi 1000, vì vậy chúng tôi xây dựng một lưới cố định. 

1. Khởi tạo mảng 2D`grid[h][w]`có kích thước 1001 × 1001 với số không. Mỗi mục sẽ tích lũy tổng diện tích được đóng góp bởi các hình chữ nhật có chiều cao và chiều rộng chính xác đó. Điều này biến đổi danh sách đầu vào thành bản đồ tần số có cấu trúc trên một miền nhỏ. 
2. Đối với mỗi hình chữ nhật$(h_i, w_i)$, thêm vào$h_i \cdot w_i$ĐẾN`grid[h_i][w_i]`. Bước này nén tất cả các hình chữ nhật giống hệt nhau thành một phần đóng góp duy nhất, bảo toàn chính xác tổng diện tích. 
3. Xây dựng mảng tổng tiền tố 2D`pref`, trong đó mỗi ô lưu trữ tổng của tất cả các giá trị trong hình chữ nhật$[1..h][1..w]$. Điều này được thực hiện bằng cách sử dụng phép lặp loại trừ bao gồm tiêu chuẩn trên một lưới. Lý do điều này có tác dụng là vì bất kỳ truy vấn hình chữ nhật nào cũng có thể được phân tách thành các vùng tiền tố chồng chéo. 
4. Đối với mỗi truy vấn$(h_s, w_s, h_b, w_b)$, tính tổng trên hình chữ nhật nghiêm ngặt:$$(h_s, h_b) \times (w_s, w_b)$$bằng cách chuyển đổi các bất đẳng thức nghiêm ngặt thành các chỉ số tiền tố bao gồm:$$h \in [h_s+1, h_b-1], \quad w \in [w_s+1, w_b-1]$$5. Trả lời truy vấn bằng cách sử dụng công thức bao gồm-loại trừ trên bảng tổng tiền tố. Điều này cung cấp tổng diện tích của tất cả các hình chữ nhật hợp lệ trong thời gian không đổi cho mỗi truy vấn. 

Quyết định cấu trúc quan trọng là thay vì truy vấn danh sách hình chữ nhật, chúng tôi truy vấn qua lưới nén trong đó mỗi tọa độ đại diện cho một cặp thứ nguyên. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, mỗi hình chữ nhật đóng góp chính xác một lần vào ô lưới tương ứng của nó, do đó không có thông tin nào bị mất trong quá trình nén. Thứ hai, phép biến đổi tổng tiền tố bảo toàn tổng chính xác trên các hình chữ nhật con được căn chỉnh theo trục. Vì mọi điều kiện truy vấn chính xác là một vùng hình chữ nhật trong lưới chiều rộng chiều cao sau khi dịch chuyển theo giới hạn nghiêm ngặt nên mọi hình chữ nhật hợp lệ sẽ được bao gồm và mọi hình chữ nhật không hợp lệ sẽ bị loại trừ. Không có trường hợp trùng lặp hoặc thiếu nào xảy ra do phép chuyển đổi có tính chất khách quan giữa các điều kiện truy vấn và các vùng con lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 1000

def solve():
    n, q = map(int, input().split())
    
    grid = [[0] * (MAX + 1) for _ in range(MAX + 1)]
    
    for _ in range(n):
        h, w = map(int, input().split())
        grid[h][w] += h * w
    
    pref = [[0] * (MAX + 1) for _ in range(MAX + 1)]
    
    for h in range(1, MAX + 1):
        row_sum = 0
        for w in range(1, MAX + 1):
            row_sum += grid[h][w]
            pref[h][w] = pref[h - 1][w] + row_sum
    
    def get(h1, w1, h2, w2):
        if h1 > h2 or w1 > w2:
            return 0
        return (
            pref[h2][w2]
            - pref[h1 - 1][w2]
            - pref[h2][w1 - 1]
            + pref[h1 - 1][w1 - 1]
        )
    
    out = []
    for _ in range(q):
        hs, ws, hb, wb = map(int, input().split())
        h1, h2 = hs + 1, hb - 1
        w1, w2 = ws + 1, wb - 1
        out.append(str(get(h1, w1, h2, w2)))
    
    print("\n".join(out))

t = int(input())
for _ in range(t):
    solve()
```Giải pháp bắt đầu bằng cách nén tất cả các hình chữ nhật vào một lưới cố định được lập chỉ mục theo chiều cao và chiều rộng. Điều này tránh việc xử lý các hình chữ nhật riêng lẻ trong quá trình truy vấn. 

Việc xây dựng tổng tiền tố được thực hiện theo từng hàng để đảm bảo mỗi ô tích lũy tất cả các đóng góp từ ma trận con trên cùng bên trái. Trung gian`row_sum`biến là cần thiết để tránh tính toán lại tổng theo chiều ngang nhiều lần, nếu không sẽ thêm hệ số bổ sung 1000 một cách không cần thiết. 

Hàm truy vấn xử lý cẩn thận các phạm vi không hợp lệ trong đó các ràng buộc nghiêm ngặt thu gọn thành một khoảng trống. Điều này ngăn ngừa các lỗi lập chỉ mục tiêu cực và đảm bảo tính chính xác ở các ranh giới. 

Mỗi truy vấn được chuyển đổi từ bất đẳng thức nghiêm ngặt thành giới hạn bao gồm trước khi áp dụng công thức tính tổng tiền tố. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, q = 1
rectangles: (2,3), (3,2)
query: (1,1,3,4)
```Chúng tôi xây dựng lưới: 

| h\w | 2 | 3 | 
| --- | --- | --- | 
| 2 | 6 | 0 | 
| 3 | 0 | 6 | 

Tổng tiền tố tích lũy các giá trị này trên mặt phẳng 2D. Truy vấn dịch sang:$h \in [2,2], w \in [2,3]$Chỉ có ô (2,3) nằm trong phạm vi này, đóng góp 6. 

Đầu ra:```
6
```Điều này chứng tỏ sự bất bình đẳng nghiêm ngặt được chuyển đổi chính xác thành các giới hạn lưới bao trùm như thế nào. 

### Ví dụ 2 

Hãy xem xét một cấu hình lớn hơn một chút:```
rectangles:
(1,2)=2, (2,2)=4, (2,3)=6, (4,4)=16
query: (1,1,3,3)
```Hình chữ nhật hợp lệ phải thỏa mãn:$h \in [2,2], w \in [2,2]$Chỉ có (2,2) đóng góp. 

| Bước | Các ô hợp lệ | Tổng hợp | 
| --- | --- | --- | 
| lọc | (2,2) | 4 | 
| kết quả | 4 | 4 | 

Điều này xác nhận rằng các hình chữ nhật bên ngoài cửa sổ truy vấn, ngay cả khi khớp một phần với một thứ nguyên, sẽ bị loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + 10^6 + q)$| Mỗi bài kiểm tra xây dựng một lưới 1000×1000 cố định và trả lời các truy vấn trong O(1) | 
| Không gian |$O(10^6)$| Lưu trữ tổng lưới và tiền tố | 

Thuật ngữ chủ đạo là cấu trúc lưới cố định, độc lập với kích thước đầu vào cho mỗi trường hợp thử nghiệm. Từ$10^6$các hoạt động trên mỗi trường hợp thử nghiệm có thể được chấp nhận theo các ràng buộc và tổng số$n, q$bị giới hạn, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAX = 1000

    def solve():
        n, q = map(int, input().split())
        grid = [[0] * (MAX + 1) for _ in range(MAX + 1)]

        for _ in range(n):
            h, w = map(int, input().split())
            grid[h][w] += h * w

        pref = [[0] * (MAX + 1) for _ in range(MAX + 1)]

        for h in range(1, MAX + 1):
            row_sum = 0
            for w in range(1, MAX + 1):
                row_sum += grid[h][w]
                pref[h][w] = pref[h - 1][w] + row_sum

        def get(h1, w1, h2, w2):
            if h1 > h2 or w1 > w2:
                return 0
            return (
                pref[h2][w2]
                - pref[h1 - 1][w2]
                - pref[h2][w1 - 1]
                + pref[h1 - 1][w1 - 1]
            )

        out = []
        for _ in range(q):
            hs, ws, hb, wb = map(int, input().split())
            h1, h2 = hs + 1, hb - 1
            w1, w2 = ws + 1, wb - 1
            out.append(str(get(h1, w1, h2, w2)))

        return "\n".join(out)

    t = int(input())
    return "\n".join(solve() for _ in range(t))

# provided sample
assert run("""3
2 1
2 3
3 2
1 1 3 4
5 5
1 1
2 2
3 3
4 4
5 5
3 3 6 6
2 1 4 5
1 1 2 10
1 1 100 100
1 1 3 3
3 1
999 999
999 999
999 998
1 1 1000 1000
""") == """6
41
9
0
54
4
2993004"""

# edge: empty answer
assert run("""1
2 2
1 1
2 2
1 1 2 2
""") == "0"

# edge: single cell valid
assert run("""1
1 1
5 7
0 0 6 8
""") == "35"

# edge: full range
assert run("""1
1 1
1 1
0 0 1000 1000
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vùng bên trong trống | 0 | bất bình đẳng nghiêm ngặt loại trừ ranh giới | 
| hình chữ nhật hợp lệ duy nhất | 35 | bao gồm chính xác khi giới hạn khớp chính xác | 
| đầy đủ | 1 | sự tích lũy toàn cầu đúng đắn | 

## Vỏ cạnh 

Một trường hợp tinh tế xuất hiện khi cửa sổ truy vấn thu gọn sau khi áp dụng giới hạn nghiêm ngặt. Ví dụ, nếu$h_b = h_s + 1$, thì khoảng chiều cao hợp lệ sẽ trở nên trống. Thuật toán xử lý việc này bằng cách tạo ra$h_1 = h_s + 1$Và$h_2 = h_b - 1$, dẫn đến$h_1 > h_2$. các`get`Hàm kiểm tra rõ ràng điều kiện này và trả về 0 ngay lập tức, ngăn chặn việc lập chỉ mục tổng tiền tố không hợp lệ. 

Một trường hợp khác là khi hình chữ nhật nằm chính xác trên ranh giới của truy vấn. Vì cả hai bất đẳng thức đều nghiêm ngặt nên một hình chữ nhật có chiều cao bằng$h_s$hoặc$h_b$không được đóng góp. Việc biểu diễn dựa trên lưới đảm bảo điều này một cách tự nhiên vì những ô đó đơn giản được loại trừ khỏi phạm vi tiền tố$[h_s+1, h_b-1]$, do đó không cần xử lý đặc biệt ngoài việc chuyển chỉ mục.
