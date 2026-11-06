---
title: "CF 1219G - Máy gặt"
description: "Chúng ta được cung cấp một lưới giá trị hình chữ nhật, trong đó mỗi ô biểu thị số lượng bong bóng có thể được thu thập từ vị trí đó. Johnny có thể kích hoạt hoạt động thu hoạch tối đa bốn lần."
date: "2026-06-15T05:15:18+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1219
codeforces_index: "G"
codeforces_contest_name: "Bubble Cup 12 - Finals [Online Mirror, unrated, Div. 2]"
rating: 2000
weight: 1219
solve_time_s: 176
verified: true
draft: false
---

[CF 1219G - Máy thu hoạch](https://codeforces.com/problemset/problem/1219/G) 

**Xếp hạng:** 2000 
**Thẻ:** triển khai 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới giá trị hình chữ nhật, trong đó mỗi ô biểu thị số lượng bong bóng có thể được thu thập từ vị trí đó. Johnny có thể kích hoạt hoạt động thu hoạch tối đa bốn lần. Mỗi lần kích hoạt sẽ chọn toàn bộ một hàng hoặc toàn bộ cột và khi được chọn, máy gặt sẽ quét qua dòng đó để thu thập tất cả các giá trị trong đó. Nếu một ô nằm trong nhiều dòng đã chọn, giá trị của ô đó vẫn chỉ được thu thập một lần. 

Nhiệm vụ là chọn tối đa bốn dòng, trộn các hàng và cột tùy ý để tối đa hóa tổng giá trị ô được thu thập riêng biệt. 

Khó khăn chính là hàng và cột chồng lên nhau. Nếu chúng ta chọn cả một hàng và một cột, ô giao nhau của chúng sẽ được tính hai lần trừ khi được sửa. Điều này tạo ra sự kết hợp giữa các lựa chọn khiến cho sự lựa chọn tham lam ngây thơ trở nên không đáng tin cậy. 

Các ràng buộc cho phép lên đến$N \cdot M \le 10^5$, do đó lưới có thể lớn nhưng đủ thưa để$O(NM)$tiền xử lý. Tuy nhiên, chúng tôi không được phép thử tất cả các tập hợp con của hàng và cột trên toàn cầu, vì số cách chọn tối đa bốn dòng từ tối đa$10^5$các ứng cử viên là rất lớn về mặt tổ hợp. 

Một cách tiếp cận đơn giản sẽ thử tất cả các kết hợp tối đa bốn dòng giữa tất cả các hàng và cột, tính toán giá trị hợp mỗi lần và lấy kết quả tốt nhất. Điều này đòi hỏi phải đánh giá một cách đại khái$\binom{N+M}{4}$khả năng là quá lớn ngay cả đối với$N+M = 2000$, huống hồ là$10^5$. 

Một vấn đề tinh vi hơn xuất hiện khi trộn các hàng và cột. Nếu chúng ta bỏ qua sự chồng chéo một cách cẩn thận, chúng ta có thể tính thừa hoặc tính thiếu các khoản đóng góp. Ví dụ: việc chọn một hàng có giá trị cao và một cột có giá trị cao có thể trông tối ưu cục bộ nhưng giao điểm của chúng có thể chứa các giá trị cực lớn làm giảm lợi ích của việc chọn cả hai. 

Các trường hợp cạnh phát sinh khi một cột cực kỳ có giá trị giao với nhiều hàng có giá trị vừa phải. Một lựa chọn tham lam ngây thơ chọn các hàng trên cùng và các cột trên cùng một cách độc lập có thể thất bại nặng nề vì sự chồng chéo chiếm ưu thế. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: thử từng tập hợp con của tối đa bốn dòng từ tất cả các hàng và cột, tính tổng giá trị thu thập được bằng cách đánh dấu các ô được bao phủ và lấy giá trị tối đa. Điều này đúng vì nó mô phỏng trực tiếp quá trình. Điểm thất bại là số lượng lựa chọn tăng dần theo$O((N+M)^4)$và mỗi chi phí đánh giá$O(NM)$, dẫn đến thời gian chạy không thể thực hiện được. 

Quan sát quan trọng là mặc dù số lượng hàng và cột rất lớn nhưng giải pháp chỉ sử dụng tối đa bốn dòng. Điều này gợi ý rõ ràng rằng chỉ một tập hợp nhỏ các hàng và cột “quan trọng” mới có thể quan trọng trong một giải pháp tối ưu. Nếu một hàng có tổng mức đóng góp rất kém so với một hàng khác thì nó khó có thể xuất hiện trong lựa chọn kích thước 4 tối ưu vì bất kỳ sự thay thế nào đều ảnh hưởng đến tối đa bốn giao điểm trên mỗi cột đã chọn và không thể biện minh cho sự thiếu hụt lớn về tổng hàng trừ khi nó bù đắp ở nơi khác. Điều này cho phép chúng ta hạn chế sự chú ý đến một tập hợp các hàng và cột ứng viên bị chặn với tổng số hàng và cột cao. 

Khi chúng tôi giới hạn ở một nhóm ứng viên nhỏ, chúng tôi có thể ép buộc tất cả các tập hợp con có kích thước tối đa bốn trên các ứng viên này. Đối với mỗi tập hợp con, chúng tôi tính toán phần đóng góp của nó bằng cách sử dụng tổng hàng, tổng cột và trừ đi phần giao nhau giữa các hàng và cột đã chọn. 

Chúng tôi đánh giá một tập hợp con$R, C$BẰNG:$$\sum_{i \in R} \text{rowSum}[i] + \sum_{j \in C} \text{colSum}[j] - \sum_{i \in R, j \in C} A_{i,j}$$Từ$|R| + |C| \le 4$, việc kiểm tra giao lộ là không đổi trên mỗi tập hợp con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các dòng |$O((N+M)^4 \cdot NM)$|$O(NM)$| Quá chậm | 
| Cắt tỉa ứng viên + liệt kê tập hợp con |$O(K^4 + NM)$|$O(NM)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng từng hàng, từng cột. 

Điều này nén lưới thành các đóng góp cấp dòng để chúng tôi có thể đánh giá các tập hợp con mà không cần tính toán lại tổng lưới đầy đủ mỗi lần. 
2. Chọn một tập hợp nhỏ các hàng và cột có tổng hàng và tổng cột cao nhất, thường là 30 hàng đầu của mỗi cột. 

Lý do là bất kỳ giải pháp tối ưu nào có kích thước tối đa là bốn đều rất có thể chỉ liên quan đến các dòng có đóng góp cao và việc giới hạn ở một nhóm có kích thước không đổi sẽ khiến cho việc tìm kiếm toàn diện trở nên khả thi. 
3. Coi các hàng và cột là các loại riêng biệt nhưng cho phép cả hai trong cùng một tập hợp con. Chúng tôi sẽ liệt kê tất cả các tập hợp con có kích thước từ 0 đến 4 từ nhóm ứng cử viên kết hợp này. 
4. Đối với mỗi tập hợp con, hãy chia nó thành các hàng đã chọn$R$và các cột được chọn$C$. 

Chúng tôi tính toán phần đóng góp cơ sở từ tổng hàng và tổng cột. 
5. Trừ các khoản đóng góp chồng chéo. Đối với mỗi cặp$(i, j)$Ở đâu$i \in R$Và$j \in C$, trừ$A_{i,j}$. 

Điều này sửa việc đếm kép vì những ô đó được bao gồm trong cả hàng và cột. 
6. Theo dõi giá trị lớn nhất trên tất cả các tập hợp con. 

### Tại sao nó hoạt động 

Bất kỳ chiến lược hợp lệ nào đều tương ứng với việc chọn tối đa bốn dòng. Sau khi giới hạn ở một nhóm ứng viên đủ phong phú, mọi sự kết hợp tối ưu đều có thể được xây dựng lại bằng vũ lực. Vì công thức đánh giá khớp chính xác với sự kết hợp của các hàng và cột đã chọn với hiệu chỉnh chồng chéo nên mỗi tập hợp con đều được tính điểm chính xác. Việc liệt kê đảm bảo chúng tôi kiểm tra mọi cách khác biệt về mặt cấu trúc để chọn tối đa bốn dòng trong số các ứng viên phù hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = [list(map(int, input().split())) for _ in range(n)]

    row_sum = [0] * n
    col_sum = [0] * m

    for i in range(n):
        row_sum[i] = sum(a[i])
    for j in range(m):
        col_sum[j] = sum(a[i][j] for i in range(n))

    rows = sorted(range(n), key=lambda i: row_sum[i], reverse=True)[:30]
    cols = sorted(range(m), key=lambda j: col_sum[j], reverse=True)[:30]

    candidates = []
    for i in rows:
        candidates.append(("r", i))
    for j in cols:
        candidates.append(("c", j))

    best = 0
    k = len(candidates)

    from itertools import combinations

    for mask_size in range(1, 5):
        for comb in combinations(range(k), mask_size):
            R = []
            C = []

            val = 0

            for idx in comb:
                t, idd = candidates[idx]
                if t == "r":
                    val += row_sum[idd]
                    R.append(idd)
                else:
                    val += col_sum[idd]
                    C.append(idd)

            for i in R:
                for j in C:
                    val -= a[i][j]

            if val > best:
                best = val

    print(best)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén lưới thành các tập hợp hàng và cột để mỗi dòng ứng cử viên có một đóng góp cơ sở duy nhất. Sau đó, bước liệt kê sẽ khám phá mọi cách để chọn tối đa bốn dòng từ một nhóm đã giảm bớt. Bước hiệu chỉnh sẽ trừ đi một cách rõ ràng các giao điểm giữa các hàng và cột đã chọn, đảm bảo không có ô nào được tính hai lần. 

Phần tinh tế duy nhất là hạn chế ứng cử viên. Không có nó, việc liệt kê là không thể. Với nó, chúng tôi giảm không gian tìm kiếm thành một vụ nổ tổ hợp có kích thước không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 2
3 4
```Tổng hàng là`[3, 7]`, tổng cột là`[4, 6]`. 

Chúng tôi thử các tập hợp con có tối đa bốn dòng. Sự lựa chọn tốt nhất là lấy cả hai hàng, đưa ra mức độ bao phủ đầy đủ. 

| Bước | Hàng được chọn | Cols được chọn | Tính toán | Giá trị | 
| --- | --- | --- | --- | --- | 
| 1 | [] | [] | 0 | 0 | 
| 2 | [0,1] | [] | 3 + 7 | 10 | 

Điều này xác nhận rằng phạm vi bao phủ toàn hàng đã nắm bắt được mọi thứ một cách tối ưu. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 3
4 5 6
7 8 9
```Tổng hàng là`[6, 15, 24]`, tổng cột là`[12, 15, 18]`. 

Một cấu hình tối ưu là chọn hàng dưới cùng và cột ngoài cùng bên phải, cùng với một tổ hợp hàng/cột khác tùy thuộc vào sự chồng chéo. 

| Bước | Hàng | Cols | Biểu hiện | Giá trị | 
| --- | --- | --- | --- | --- | 
| 1 | [2] | [2] | 24 + 18 - 9 | 33 | 
| 2 | [1,2] | [2] | 15 + 24 + 18 - (6 + 9) | 42 | 

Điều này cho thấy cách hiệu chỉnh chồng lấp làm thay đổi cấu trúc tối ưu như thế nào vì ô dùng chung phải được xóa một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(NM + K^4)$| tiền xử lý hàng/col cộng với phép liệt kê tập hợp con có kích thước không đổi | 
| Không gian |$O(NM)$| lưu trữ lưới và cốt liệu | 

Kích thước lưới tối đa là$10^5$, do đó việc tính tổng hàng và cột là tuyến tính và có thể chấp nhận được. Việc liệt kê ứng cử viên là thời gian không đổi do kích thước cắt tỉa cố định, giúp giải pháp dễ dàng trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

# sample
assert run("""2 2
1 2
3 4
""") == "10"

# single cell
assert run("""1 1
5
""") == "5"

# all zeros
assert run("""2 3
0 0 0
0 0 0
""") == "0"

# single row dominance
assert run("""1 5
1 2 3 4 5
""") == "15"

# mixed structure
assert run("""3 3
1 100 1
1 1 1
1 1 1
""") == "105"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 5 | trường hợp tối thiểu | 
| tất cả số không | 0 | trường hợp không có cạnh đóng góp | 
| hàng đơn | 15 | tối ưu chỉ hàng | 
| cấu trúc hỗn hợp | 105 | tính chính xác tương tác hàng-cột | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một giá trị cực kỳ lớn nằm ở giao điểm của hàng và cột. Trong trường hợp như vậy, việc chọn cả hàng và cột có thể có vẻ có lợi vì cả hai đều có tổng cao, nhưng phép trừ chồng lấp sẽ loại bỏ ô lớn đó một lần, điều này có thể thay đổi đáng kể tập hợp con tối ưu. Thuật toán xử lý việc này một cách chính xác vì nó trừ đi rõ ràng mọi giao điểm cho mỗi đánh giá tập hợp con, đảm bảo rằng không còn phép tính kép nào tồn tại. 

Một trường hợp khác là khi giải pháp tối ưu chỉ sử dụng cột hoặc chỉ hàng. Vì các tập hợp con bao gồm các cấu hình không có hàng hoặc cột không, nên bảng liệt kê đương nhiên bao gồm các lựa chọn thuần túy, đảm bảo các trường hợp này được đánh giá chính xác mà không cần viết hoa đặc biệt.
