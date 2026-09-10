---
title: "CF 104598H - Đánh giá mô hình"
description: "Nhiệm vụ đưa ra hai lưới số vuông, cả hai đều có kích thước $N nhân N$, biểu thị cường độ điểm ảnh của hai hình ảnh. Đối với mỗi truy vấn, chúng ta có một tiểu vùng hình chữ nhật bên trong các lưới này, được xác định bởi hai góc đối diện."
date: "2026-06-30T03:07:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104598
codeforces_index: "H"
codeforces_contest_name: "GPL 2023 Advanced"
rating: 0
weight: 104598
solve_time_s: 86
verified: true
draft: false
---

[CF 104598H - Đánh giá mô hình](https://codeforces.com/problemset/problem/104598/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ đưa ra hai lưới số vuông, cả hai đều có kích thước$N \times N$, biểu thị cường độ pixel của hai hình ảnh. Đối với mỗi truy vấn, chúng ta có một tiểu vùng hình chữ nhật bên trong các lưới này, được xác định bởi hai góc đối diện. Đối với hình chữ nhật đó, chúng ta tính tổng các giá trị trong ảnh$A$và tổng các giá trị trong hình ảnh$B$, sau đó xuất ra chênh lệch tuyệt đối giữa hai tổng đó. 

Mỗi truy vấn là độc lập, vì vậy chúng ta được yêu cầu nhiều lần tính tổng hình chữ nhật trong hai ma trận và so sánh chúng. 

Những ràng buộc làm cho ý tưởng ngây thơ không thể thực hiện được. Với$N \le 800$, lưới có tới$6.4 \times 10^5$tế bào và có thể có tới$7 \times 10^4$truy vấn. Nếu mỗi truy vấn tính lại tổng hình chữ nhật bằng cách quét tất cả các ô trong vùng, hình chữ nhật trong trường hợp xấu nhất là toàn bộ lưới, dẫn đến khoảng$800^2 \cdot 70000$hoạt động vượt xa giới hạn thời gian. 

Trường hợp cạnh chung liên quan đến các hình chữ nhật lớn trong đó$r_1 > r_2$hoặc$c_1 > c_2$. Báo cáo vấn đề cho phép tọa độ theo bất kỳ thứ tự nào, do đó, việc triển khai đơn giản giả sử các góc có thứ tự sẽ âm thầm tính toán các tiểu vùng không chính xác trừ khi nó chuẩn hóa tọa độ trước. 

Một cạm bẫy khác là tràn. Mỗi ô có thể lên tới$10^9$, vậy là đầy đủ$800 \times 800$tổng đạt$6.4 \times 10^{14}$, không phù hợp với số nguyên 32 bit và yêu cầu số học 64 bit. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn bằng cách lặp qua từng ô bên trong hình chữ nhật và tính tổng các giá trị trong cả hai lưới. Điều này đơn giản và chính xác nhưng chi phí cho mỗi truy vấn phụ thuộc vào diện tích hình chữ nhật. Trong trường hợp xấu nhất, mỗi truy vấn chạm vào$O(N^2)$tế bào, dẫn đến$O(N^2 Q)$, quá chậm đối với kích thước lớn$Q$. 

Quan sát quan trọng là tổng hình chữ nhật có thể được tính toán trước bằng cách sử dụng bảng tổng tiền tố. Thay vì tính toán lại các tổng cho mỗi truy vấn, chúng tôi xử lý trước từng lưới thành một mảng tổng tiền tố 2D để có thể thu được bất kỳ tổng hình chữ nhật phụ nào trong thời gian không đổi bằng cách sử dụng loại trừ bao gồm. Vì cần tính tổng cho cả hai hình ảnh nên chúng tôi xây dựng hai mảng tổng tiền tố và trả lời từng truy vấn bằng cách trừ chúng và lấy giá trị tuyệt đối. 

Điều này làm giảm mỗi truy vấn khỏi việc quét$O(N^2)$tế bào để$O(1)$, làm thay đổi vấn đề từ không khả thi thành hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2 Q)$|$O(1)$| Quá chậm | 
| Tổng tiền tố |$O(N^2 + Q)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng tổng tiền tố 2D tiêu chuẩn cho mỗi hình ảnh. 

1. Đọc cả hai lưới$A$Và$B$. Chúng được coi là ma trận được lập chỉ mục từ 1 đến$N$. Điều này đơn giản hóa việc xử lý ranh giới trong tổng tiền tố. 
2. Xây dựng tổng tiền tố$SA$Và$SB$, trong đó mỗi mục lưu trữ tổng của ma trận con từ$(1,1)$ĐẾN$(i,j)$. Mỗi giá trị được tính toán bằng cách sử dụng các tiền tố được tính toán trước đó, đảm bảo mỗi ô được xử lý một lần. 
3. Đối với mỗi truy vấn, chuẩn hóa tọa độ sao cho$r_1 \le r_2$Và$c_1 \le c_2$. Điều này tránh phạm vi không chính xác khi các góc đầu vào được đưa ra theo thứ tự ngược lại. 
4. Tính tổng hình chữ nhật trong$A$sử dụng loại trừ bao gồm:$$SA(r_2,c_2) - SA(r_1-1,c_2) - SA(r_2,c_1-1) + SA(r_1-1,c_1-1)$$Tính toán tương tự được áp dụng cho$SB$. 
5. In ra sự khác biệt tuyệt đối giữa hai kết quả. 

Mỗi truy vấn hiện chỉ sử dụng số học theo thời gian không đổi. 

### Tại sao nó hoạt động 

Tổng tiền tố 2D mã hóa tổng diện tích tích lũy để bất kỳ hình chữ nhật nào cũng có thể được phân tách thành bốn vùng tiền tố. Loại trừ bao gồm hủy bỏ các khu vực chồng chéo chính xác một lần. Vì mỗi ô đóng góp chính xác vào một tổ hợp các thuật ngữ tiền tố nên giá trị được tính toán khớp với tổng hình chữ nhật thực. Phép trừ giữa hai tổng hình chữ nhật đúng độc lập duy trì tính đúng đắn của hiệu tuyệt đối cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_prefix(grid, n):
    ps = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        row_sum = 0
        for j in range(1, n + 1):
            row_sum += grid[i-1][j-1]
            ps[i][j] = ps[i-1][j] + row_sum
    return ps

def rect_sum(ps, r1, c1, r2, c2):
    return (
        ps[r2][c2]
        - ps[r1-1][c2]
        - ps[r2][c1-1]
        + ps[r1-1][c1-1]
    )

def solve():
    n, q = map(int, input().split())

    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(n)]

    psa = build_prefix(A, n)
    psb = build_prefix(B, n)

    out = []
    for _ in range(q):
        r1, c1, r2, c2 = map(int, input().split())
        if r1 > r2:
            r1, r2 = r2, r1
        if c1 > c2:
            c1, c2 = c2, c1

        sa = rect_sum(psa, r1, c1, r2, c2)
        sb = rect_sum(psb, r1, c1, r2, c2)
        out.append(str(abs(sa - sb)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```After reading the input, both matrices are stored explicitly so that indexing matches the prefix construction. The prefix arrays are 1-indexed to avoid repeated boundary checks inside queries.

 The rectangle sum function applies the standard inclusion-exclusion identity. The normalization step for query coordinates is essential because the input does not guarantee ordering of corners.

 ## Ví dụ đã hoạt động 

Consider the first sample.

 We build prefix sums for both matrices, then process query$(1,1,1,3)$. Sau khi bình thường hóa nó vẫn không thay đổi. Tổng hình chữ nhật được tính theo thời gian không đổi từ các mảng tiền tố, tạo ra 11 cho$A$và 9 cho$B$, do đó đầu ra là 2. 

Đối với truy vấn thứ hai$(3,3,1,2)$, chuẩn hóa tạo ra$(1,2,3,3)$. Tổng tiền tố cho tổng số bằng nhau cho cả hai ma trận, do đó hiệu là 0. 

Những ví dụ này xác nhận rằng cả đầu vào tọa độ có thứ tự và tọa độ đảo ngược đều được xử lý chính xác và tổng tiền tố đó trả về các tập hợp hình chữ nhật nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 + Q)$| xây dựng tiền tố cộng với các truy vấn thời gian không đổi | 
| Không gian |$O(N^2)$| lưu trữ hai bảng tiền tố | 

Giới hạn cho phép xử lý trước lên tới 800×800, nằm trong giới hạn. Mỗi truy vấn trong số lên tới 70000 truy vấn được trả lời trong thời gian không đổi, đảm bảo giải pháp phù hợp thoải mái trong điều kiện hạn chế về thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    A = [list(map(int, input().split())) for _ in range(n)]
    B = [list(map(int, input().split())) for _ in range(n)]

    def build(ps):
        n = len(ps)
        for i in range(n):
            for j in range(n):
                ps[i][j] += (ps[i-1][j] if i else 0) + (ps[i][j-1] if j else 0) - (ps[i-1][j-1] if i and j else 0)
        return ps

    psa = build(A)
    psb = build(B)

    def get(ps, r1, c1, r2, c2):
        res = ps[r2][c2]
        if r1: res -= ps[r1-1][c2]
        if c1: res -= ps[r2][c1-1]
        if r1 and c1: res += ps[r1-1][c1-1]
        return res

    out = []
    for _ in range(q):
        r1, c1, r2, c2 = map(int, input().split())
        r1, r2 = sorted([r1-1, r2-1])
        c1, c2 = sorted([c1-1, c2-1])
        out.append(str(abs(get(psa, r1, c1, r2, c2) - get(psb, r1, c1, r2, c2))))

    return "\n".join(out)

# provided sample
assert run("""3 2
3 1 7
2 5 2
5 8 4
5 2 2
1 3 7
4 9 4
1 1 1 3
3 3 1 2
""") == "2\n0"

# custom cases
assert run("""1 1
5
3
1 1 1 1
""") == "2", "single cell"

assert run("""2 1
1 2
3 4
4 3
2 1
1 1 2 2
""") == "0", "equal sums"

assert run("""2 1
1 1
1 1
2 2
2 2
1 1 2 2
""") == "0", "all equal"

assert run("""3 1
1 2 3
4 5 6
7 8 9
9 8 7
6 5 4
3 2 1
1 1 3 3
""") == "0", "full symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 2 | tính chính xác của phép trừ đơn ô | 
| 2x2 giá trị hoán đổi | 0 | hủy trên toàn lưới | 
| ma trận giống hệt nhau | 0 | tính đúng đắn cơ bản | 
| lưới có cấu trúc đảo ngược | 0 | tập hợp hình chữ nhật nhất quán | 

## Vỏ cạnh 

Một truy vấn một ô như$(i,j,i,j)$kiểm tra xem phép trừ tiền tố có suy biến chính xác hay không. Trong trường hợp đó, tất cả các thuật ngữ tiền tố bên ngoài đều bị hủy và chỉ còn lại một ô duy nhất, do đó cả hai tổng đều giảm xuống mục nhập đó và chênh lệch được tính toán chính xác. 

Một truy vấn tọa độ đảo ngược, chẳng hạn như$(r_2,c_2,r_1,c_1)$kiểm tra xem việc chuẩn hóa có được áp dụng hay không. Nếu không sắp xếp, phép trừ tiền tố sẽ truy cập vào các vùng không hợp lệ và tạo ra kết quả không chính xác, nhưng sau khi hoán đổi tọa độ, hình chữ nhật sẽ trở thành hợp lệ và loại trừ bao gồm được áp dụng rõ ràng. 

Truy vấn toàn lưới kiểm tra xem tổng tiền tố lớn có vượt quá giới hạn 32 bit hay không. Sử dụng số nguyên Python tránh tràn và duy trì tính chính xác ngay cả khi đạt đến tổng$10^{14}$tỉ lệ.
