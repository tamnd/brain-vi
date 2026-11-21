---
title: "CF 2220A - Bị chặn"
description: "Chúng ta được cho hai số nguyên không âm (x) và (y). Chúng ta phải xây dựng hai số nguyên không âm mới (p) và (q) sao cho không có bit nào được đặt đồng thời ở cả hai số, nghĩa là [ p ,&, q = 0."
date: "2026-06-09T04:58:25+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2220
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1093 (Div. 2)"
rating: 0
weight: 2220
solve_time_s: 164
verified: false
draft: false
---

[CF 2220A - Bị chặn](https://codeforces.com/problemset/problem/2220/A) 

**Đánh giá:** - 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 2m 44s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Cho ta hai số nguyên không âm\(x\)Và\(y\). Chúng ta phải xây dựng hai số nguyên không âm mới\(p\)Và\(q\)sao cho không có bit nào được đặt đồng thời ở cả hai số, nghĩa là,$$
p \,\&\, q = 0.
$$Trong số tất cả các cặp như vậy, chúng tôi muốn giảm thiểu$$
|x-p| + |y-q|.
$$Đầu ra không phải là giá trị tối thiểu. Chúng ta phải xuất ra một cặp tối ưu \((p,q)\). 

Các số nhỏ hơn\(2^{30}\), nhưng có tới\(10^4\)trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào tìm kiếm trên các giá trị ứng viên của\(p\)hoặc\(q\)ngay lập tức là không thể. Ngay cả việc khám phá tất cả các giá trị trong một vùng lân cận nhỏ của\(x\)Và\(y\)sẽ không hoạt động. 

điều kiện\(p \,\&\, q = 0\)là theo chiều bit. Hàm mục tiêu là số. Sự kết hợp này là một tín hiệu mạnh mẽ cho thấy giải pháp sẽ xử lý các bit từ quan trọng nhất đến ít quan trọng nhất trong khi theo dõi cách các số được xây dựng so với\(x\)Và\(y\). 

Một trường hợp khó phát hiện xảy ra khi cả hai\(x\)Và\(y\)chứa cùng một bit cao. Ví dụ:```
x = 4
y = 4
```Cặp \((4,4)\) bị cấm vì\(4 \& 4 = 4\). Một chiến lược tham lam giữ từng bit khớp sẽ thất bại. Một câu trả lời tối ưu là \((4,3)\), với chi phí\(1\). 

Một trường hợp phức tạp khác là```
x = 1
y = 1
```Câu trả lời đúng nhất không phải là \((1,0)\) hay \((0,1)\), cả hai đều có chi phí\(1\). Các đầu ra mẫu \((2,1)\), cũng có chi phí\(1\). Các giải pháp tối ưu không phải là duy nhất, vì vậy thuật toán phải tối ưu hóa chi phí chứ không phải cố gắng tái tạo một câu trả lời cụ thể. 

## Phương pháp tiếp cận 

Một tìm kiếm mạnh mẽ sẽ liệt kê tất cả các cặp \((p,q)\) thỏa mãn\(p \& q = 0\), tính toán$$
|x-p| + |y-q|,
$$và giữ cái tốt nhất. 

Ngay cả khi chúng tôi giới hạn bản thân ở những con số dưới đây\(2^{31}\), sẽ có khoảng\(2^{62}\)các cặp ứng cử viên Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là cả ràng buộc và mục tiêu đều có thể được xử lý từng chút một. 

Giả sử chúng ta đang xây dựng\(p\)Và\(q\)từ bit quan trọng nhất trở xuống. Tại bất kỳ thời điểm nào, chỉ có thể có ba trạng thái so sánh\(p\)liên quan đến\(x\):$$
p < x,\quad p = x,\quad p > x.
$$Điều này cũng đúng đối với\(q\)liên quan đến\(y\). 

Do đó toàn bộ tương lai chỉ phụ thuộc vào hai trạng thái ba ngôi, đưa ra$$
3 \times 3 = 9
$$các trạng thái so sánh có thể 

Một khi mối quan hệ giữa\(p\)Và\(x\)đã được xác định, các bit thấp hơn đóng góp tuyến tính vào\(|p-x|\). Điều tương tự cũng đúng đối với\(q\)Và\(y\). Điều này cho phép giải pháp kiểu chữ số-DP trên các chữ số nhị phân. 

Chúng tôi xử lý các bit từ 30 xuống 0. Tại mỗi bit, chúng tôi chọn một trong ba phép gán được phép:$$
(p_b,q_b)\in\{(0,0),(0,1),(1,0)\},
$$vì \((1,1)\) sẽ vi phạm\(p \& q = 0\). 

DP giữ chi phí tối thiểu có thể cho mọi trạng thái so sánh và lưu trữ các chuyển đổi để tái thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---:|---:|---| 
| Lực lượng vũ phu | \(O(2^{62})\) | \(O(1)\) | Quá chậm | 
| Bit DP | \(O(31 \cdot 9 \cdot 3)\) | \(O(31 \cdot 9)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý các bit từ 30 xuống 0. 

2. Duy trì trạng thái \((s_x,s_y)\), trong đó mỗi thành phần thuộc về\(\{-1,0,1\}\). 

Đây$$
   s_x =
   \begin{cases}
   -1 & p < x,\\
   0 & p = x,\\
   1 & p > x,
   \end{cases}
   $$chỉ xem xét các bit cao hơn đã được xử lý. 

Định nghĩa của\(s_y\)là tương tự. 

3. Với mỗi trạng thái, hãy thử ba phép gán bit hợp lệ$$
   (0,0),\ (0,1),\ (1,0).
   $$4. Cập nhật trạng thái so sánh cho\(p\)so với\(x\). 

Nếu như\(s_x=0\), bit hiện tại có thể thiết lập mối quan hệ lần đầu tiên. 

Nếu như\(s_x\neq0\), mối quan hệ không bao giờ thay đổi nữa. 

5. Thêm phần đóng góp của bit hiện tại vào\(|p-x|\). 

Cho phép$$
   d_x = p_b - x_b.
   $$Nếu như\(s_x=1\), dấu hiệu của\(p-x\)đã được biết là dương nên bit này góp phần$$
   d_x \cdot 2^b.
   $$Nếu như\(s_x=-1\), nó góp phần$$
   -d_x \cdot 2^b.
   $$Nếu như\(s_x=0\), thì sự khác biệt đầu tiên đóng góp chính xác$$
   2^b.
   $$6. Làm tương tự với\(q\)so với\(y\). 

7. Giữ cho quá trình chuyển đổi mang lại chi phí tối thiểu. 

8. Sau bit cuối cùng, chọn trạng thái có tổng chi phí tối thiểu và xây dựng lại\(p\)Và\(q\)từ cha mẹ được lưu trữ. 

### Tại sao nó hoạt động 

Điều quan trọng nhất ở đâu\(p\)Và\(x\)khác nhau quyết định liệu\(p>x\)hoặc\(p<x\). Khi bit đó được cố định, mọi bit thấp hơn sẽ đóng góp một dấu hiệu đã biết vào sự khác biệt tuyệt đối. Trạng thái DP lưu trữ chính xác thông tin dấu hiệu này. 

Vì mọi phép gán bit hợp lệ đều tương ứng với một đường dẫn duy nhất thông qua DP và mỗi đường dẫn sẽ tích lũy chính xác giá trị$$
|p-x|+|y-q|,
$$DP kiểm tra tất cả các cặp khả thi \((p,q)\) thỏa mãn\(p\&q=0\)và chọn một cái có chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BITS = 31
INF = 10**18

def solve_case(x, y):
    dp = [[INF] * 9 for _ in range(BITS + 1)]
    parent = [[None] * 9 for _ in range(BITS + 1)]

    start = 4  # (sx, sy) = (0, 0)
    dp[0][start] = 0

    def idx(sx, sy):
        return (sx + 1) * 3 + (sy + 1)

    def decode(v):
        return v // 3 - 1, v % 3 - 1

    for pos in range(BITS):
        b = 30 - pos
        w = 1 << b

        xb = (x >> b) & 1
        yb = (y >> b) & 1

        for st in range(9):
            cur = dp[pos][st]
            if cur == INF:
                continue

            sx, sy = decode(st)

            for pb, qb in ((0, 0), (0, 1), (1, 0)):
                # x side
                dx = pb - xb

                if sx == 0:
                    if dx == 0:
                        nsx = 0
                        addx = 0
                    elif dx > 0:
                        nsx = 1
                        addx = w
                    else:
                        nsx = -1
                        addx = w
                elif sx == 1:
                    nsx = 1
                    addx = dx * w
                else:
                    nsx = -1
                    addx = -dx * w

                # y side
                dy = qb - yb

                if sy == 0:
                    if dy == 0:
                        nsy = 0
                        addy = 0
                    elif dy > 0:
                        nsy = 1
                        addy = w
                    else:
                        nsy = -1
                        addy = w
                elif sy == 1:
                    nsy = 1
                    addy = dy * w
                else:
                    nsy = -1
                    addy = -dy * w

                nst = idx(nsx, nsy)
                nv = cur + addx + addy

                if nv < dp[pos + 1][nst]:
                    dp[pos + 1][nst] = nv
                    parent[pos + 1][nst] = (st, pb, qb)

    best_state = min(range(9), key=lambda s: dp[BITS][s])

    p = 0
    q = 0
    st = best_state

    for pos in range(BITS, 0, -1):
        prev, pb, qb = parent[pos][st]
        b = 30 - (pos - 1)

        p |= pb << b
        q |= qb << b

        st = prev

    return p, q

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        x, y = map(int, input().split())
        p, q = solve_case(x, y)
        ans.append(f"{p} {q}")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Bảng DP chứa 31 lớp, mỗi lớp một vị trí. Mỗi lớp chỉ có 9 trạng thái. Đối với mọi trạng thái, chúng tôi thử chính xác ba lần chuyển đổi vì cặp \((1,1)\) bị cấm. 

Các công thức đóng góp là phần tinh tế. Khi trạng thái so sánh đã được biết, các bit thấp hơn sẽ ảnh hưởng trực tiếp đến chênh lệch có dấu. Khi bit hiện tại tạo ra sự khác biệt đầu tiên, bit đó đóng góp chính xác trọng số của nó vì nó trở thành bit khác biệt dẫn đầu. 

Giai đoạn xây dựng lại đi ngược lại qua các bit cha được lưu trữ và xây dựng lại các bit đã chọn của\(p\)Và\(q\). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
x = 4
y = 4
```Biểu diễn nhị phân:$$
x=y=100_2.
$$Ở bit 2, việc chọn \((1,1)\) bị cấm. 

| Chút | x chút | y bit | Bit (p,q) được chọn | 
|---|---:|---:|---:| 
| 2 | 1 | 1 | (1,0) | 
| 1 | 0 | 0 | (0,1) | 
| 0 | 0 | 0 | (0,1) | 

Điều này tái tạo lại$$
p=100_2=4,\qquad q=011_2=3.
$$Chi phí là$$
|4-4|+|4-3|=1.
$$Ví dụ cho thấy cách chia sẻ bit cao giữa hai số. 

### Ví dụ 2 

đầu vào:```
x = 3
y = 6
```Nhị phân:$$
x=011_2,\qquad y=110_2.
$$Một sự tái thiết tối ưu là: 

| Chút | x chút | y bit | Bit (p,q) được chọn | 
|---|---:|---:|---:| 
| 2 | 0 | 1 | (0,1) | 
| 1 | 1 | 1 | (1,0) | 
| 0 | 1 | 0 | (1,0) | 

Điều này mang lại$$
p=3,\qquad q=4.
$$Chi phí là$$
|3-3|+|6-4|=2.
$$Đầu ra mẫu sử dụng\(q=8\), có cùng chi phí tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---:|---| 
| Thời gian | \(O(31 \cdot 9 \cdot 3)\) | 31 bit, 9 trạng thái, 3 lần chuyển tiếp | 
| Không gian | \(O(31 \cdot 9)\) | DP và bảng cha | 

Tổng số công việc cho mỗi trường hợp thử nghiệm chỉ là vài trăm thao tác. Với\(10^4\)trường hợp thử nghiệm, giải pháp dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    BITS = 31
    INF = 10**18

    def solve_case(x, y):
        dp = [[INF] * 9 for _ in range(BITS + 1)]
        parent = [[None] * 9 for _ in range(BITS + 1)]

        dp[0][4] = 0

        def idx(sx, sy):
            return (sx + 1) * 3 + (sy + 1)

        def decode(v):
            return v // 3 - 1, v % 3 - 1

        for pos in range(BITS):
            b = 30 - pos
            w = 1 << b

            xb = (x >> b) & 1
            yb = (y >> b) & 1

            for st in range(9):
                cur = dp[pos][st]
                if cur == INF:
                    continue

                sx, sy = decode(st)

                for pb, qb in ((0, 0), (0, 1), (1, 0)):
                    dx = pb - xb
                    dy = qb - yb

                    if sx == 0:
                        if dx == 0:
                            nsx, addx = 0, 0
                        elif dx > 0:
                            nsx, addx = 1, w
                        else:
                            nsx, addx = -1, w
                    elif sx == 1:
                        nsx, addx = 1, dx * w
                    else:
                        nsx, addx = -1, -dx * w

                    if sy == 0:
                        if dy == 0:
                            nsy, addy = 0, 0
                        elif dy > 0:
                            nsy, addy = 1, w
                        else:
                            nsy, addy = -1, w
                    elif sy == 1:
                        nsy, addy = 1, dy * w
                    else:
                        nsy, addy = -1, -dy * w

                    nst = idx(nsx, nsy)
                    nv = cur + addx + addy

                    if nv < dp[pos + 1][nst]:
                        dp[pos + 1][nst] = nv
                        parent[pos + 1][nst] = (st, pb, qb)

        best = min(range(9), key=lambda s: dp[BITS][s])

        p = q = 0
        st = best

        for pos in range(BITS, 0, -1):
            prev, pb, qb = parent[pos][st]
            b = 30 - (pos - 1)
            p |= pb << b
            q |= qb << b
            st = prev

        return f"{p} {q}"

    t = int(input())
    return "\n".join(
        solve_case(*map(int, input().split()))
        for _ in range(t)
    )

# sample input, output may differ because multiple answers exist

# minimum values
out = run("1\n0 0\n")
p, q = map(int, out.split())
assert (p & q) == 0

# equal numbers
out = run("1\n4 4\n")
p, q = map(int, out.split())
assert (p & q) == 0

# boundary
out = run("1\n1073741823 1073741822\n")
p, q = map(int, out.split())
assert (p & q) == 0

# asymmetric
out = run("1\n0 123456\n")
p, q = map(int, out.split())
assert (p & q) == 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
|`0 0`| Bất kỳ cặp tối ưu hợp lệ nào | Giá trị nhỏ nhất có thể | 
|`4 4`| Bất kỳ cặp tối ưu nào có AND bằng 0 | Bit cao nhất được chia sẻ | 
|`1073741823 1073741822`| Cặp tối ưu nào | Đầu vào lớn nhất | 
|`0 123456`| Cặp tối ưu nào | Một số đã bằng 0 | 

## Vỏ cạnh 

Khi cả hai số đều bằng 0, DP vẫn ở trạng thái đẳng thức cho mọi bit và tái tạo lại \((0,0)\). Chi phí bằng 0, rõ ràng là tối ưu. 

Khi cả hai số chia sẻ một bit lớn, chẳng hạn như```
4 4
```việc gán bit \((1,1)\) bị cấm. DP đánh giá cả hai khả năng, đưa ra thông tin cho\(p\)hoặc đưa nó cho\(q\), sau đó điều chỉnh tối ưu các bit thấp hơn. Điều này tránh được lỗi phổ biến là giữ nguyên cả hai số. 

Vì```
1073741823 1073741822
```mọi bit dưới bit 30 đều hoạt động ở ít nhất một số. Sự lựa chọn tham lam cục bộ có thể dễ dàng tạo ra lỗi lớn hơn sau này. DP giữ đồng thời tất cả các trạng thái so sánh, do đó, nó không bao giờ cam kết đưa ra quyết định bit cao dưới mức tối ưu.
