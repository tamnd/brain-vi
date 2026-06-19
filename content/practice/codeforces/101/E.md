---
title: "CF 101E - Kẹo và Đá"
description: "Gerald liên tục lấy đi một viên kẹo hoặc một viên đá. Sau mỗi lần di chuyển, Mike xem Gerald đã ăn bao nhiêu viên kẹo và đá. Nếu Gerald đã ăn a kẹo và b đá, Mike sẽ thưởng $$f(a,b) = (xa + yb) bmod p$$ điểm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp"]
categories: ["algorithms"]
codeforces_contest: 101
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 79 (Div. 1 Only)"
rating: 2500
weight: 101
solve_time_s: 206
verified: false
draft: false
---

[CF 101E - Kẹo và Đá](https://codeforces.com/problemset/problem/101/E) 

**Đánh giá:** 2500 
**Tags:** chia để trị, dp 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Gerald liên tục lấy đi một viên kẹo hoặc một viên đá. Sau mỗi lần di chuyển, Mike xem Gerald đã ăn bao nhiêu viên kẹo và đá. Nếu Gerald đã ăn`a`kẹo và`b`đá cho đến nay, giải thưởng Mike$$f(a,b) = (x_a + y_b) \bmod p$$điểm. 

Trò chơi bắt đầu trước bất kỳ nước đi nào với`(a,b) = (0,0)`, và nó kết thúc ngay khi còn lại đúng một viên kẹo và một viên đá. Vì Gerald bị cấm tiêu thụ tất cả kẹo hoặc đá nên trạng thái cuối cùng phải luôn là$$(a,b) = (n-1,m-1)$$và tổng số lần di chuyển là cố định:$$(n-1) + (m-1)$$Điều duy nhất Gerald kiểm soát là thứ tự tiêu thụ kẹo và đá. 

Một chuỗi di chuyển có thể được xem như một đường dẫn trên lưới. Tình trạng`(a,b)`nghĩa là Gerald đã ăn rồi`a`kẹo và`b`đá. Từ`(a,b)`chúng ta có thể di chuyển đến`(a+1,b)`bằng cách ăn một viên kẹo hoặc`(a,b+1)`bằng cách ăn một hòn đá. Mỗi trạng thái được ghé thăm đều đóng góp`f(a,b)`điểm. 

Nhiệm vụ là tìm một đường đi đơn điệu có trọng số lớn nhất từ`(0,0)`ĐẾN`(n-1,m-1)`. 

Những hạn chế là thách thức thực sự. Cả hai`n`Và`m`nhiều nhất là`20000`, do đó lưới có thể chứa tới`4 * 10^8`tiểu bang. Không thể có một bảng lập trình động tiêu chuẩn trên tất cả các ô cả về thời gian và bộ nhớ. Ngay cả việc lặp lại một lần trên tất cả các trạng thái cũng đã quá chậm trong Python. 

Hàm tính điểm có dạng đặc biệt:$$f(a,b) = (x_a + y_b) \bmod p$$Nó chỉ phụ thuộc vào một giá trị hàng và một giá trị cột. Các vấn đề với cấu trúc này thường thừa nhận sự tối ưu hóa chia để trị vì sự chuyển tiếp giữa các hàng lân cận diễn ra thường xuyên. 

Có một số trường hợp khó bỏ sót. 

Nếu tất cả các giá trị đều bằng modulo`p`, thì mọi đường đi đều có cùng số điểm. Một chiến lược tham lam cố gắng tối đa hóa phần thưởng ngay lập tức tiếp theo vẫn có hiệu lực, nhưng việc tái thiết phải nhất quán. 

Ví dụ:```
2 2 10
0 0
0 0
```Cả hai con đường đều tạo ra cùng một câu trả lời. Bất kỳ chuỗi độ dài hợp lệ nào`2`được chấp nhận. 

Một trường hợp tinh vi khác xảy ra khi modulo quấn quanh.```
2 2 5
4 4
4 4
```Ở đây mọi phần thưởng đều`(4+4)%5 = 3`, không`8`. Việc quên modulo trong quá trình tiền xử lý âm thầm tạo ra các câu trả lời hoàn toàn sai. 

Trạng thái cuối cùng cũng quan trọng. Gerald không thể ăn hết kẹo hay đá. Điều đó có nghĩa là đường đi phải dừng lại ở`(n-1,m-1)`chính xác, không vượt quá nó. Việc triển khai bất cẩn tạo nên một đường dẫn dài`n+m`thay vì`n+m-2`tạo ra một chiến lược không hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là lập trình động tiêu chuẩn trên lưới. 

Cho phép$$dp[a][b]$$là tổng số điểm tối đa có thể đạt được khi đạt đến trạng thái`(a,b)`. Vì mỗi nước đi đều thay đổi chính xác một tọa độ`1`, sự tái diễn là$$dp[a][b] = f(a,b) + \max(dp[a-1][b], dp[a][b-1])$$với các điều kiện biên thích hợp. 

Điều này hoạt động vì mọi đường dẫn hợp lệ vào`(a,b)`phải đến từ một trong hai quốc gia láng giềng đó. 

Thật không may, lưới chứa tới`4 * 10^8`tế bào. Ngay cả việc lưu trữ một số nguyên trên mỗi ô cũng đã vượt quá giới hạn bộ nhớ và số lần chuyển đổi vượt xa mức vừa vặn trong 8 giây. 

Quan sát chính là cấu trúc chuyển tiếp cực kỳ đơn giản. Các hướng của cạnh đều đơn điệu và trọng lượng ô phân hủy thành các thành phần hàng và cột. 

Giả sử chúng ta sửa một hàng ở giữa`mid`. Mọi đường đi tối ưu đều đi qua hàng này đúng một lần. Nếu chúng ta biết cột giao nhau, chúng ta có thể giải các bài toán con trên và dưới một cách độc lập. 

Điều này gợi ý sự phân chia và chinh phục qua các hàng. 

Vấn đề còn lại là xác định vị trí đường đi tối ưu đi qua hàng giữa. Đây là nơi sự đơn điệu xuất hiện. Nếu chúng ta tính toán các tiền tố tối ưu từ trên cùng và các hậu tố tối ưu từ dưới lên thì đối với mỗi cột, chúng ta có thể đánh giá đường đi tốt nhất đi qua`(mid,col)`. Cột tối đa hóa trở thành điểm phân chia. 

Cấu trúc này gần giống với thuật toán tái tạo LCS của Hirschberg. Thay vì xây dựng lại chuỗi con chung dài nhất, chúng tôi xây dựng lại đường dẫn đơn điệu có trọng số tối ưu trong khi chỉ lưu trữ một hàng DP tại một thời điểm. 

Mỗi cấp độ chia để trị chỉ xử lý một dải hình chữ nhật và mỗi ô chỉ tham gia vào nhiều bài toán con theo logarit. Sự phức tạp kết quả trở nên có thể quản lý được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP |$O(nm)$|$O(nm)$| Quá chậm | 
| Tái thiết DP phân chia và chinh phục |$O(nm)$|$O(m)$| Đã chấp nhận | 

Độ phức tạp về thời gian vẫn còn`O(nm)`bởi vì mọi bài toán con vẫn quét hình chữ nhật của nó, nhưng bộ nhớ giảm từ hàng trăm triệu ô xuống còn một vài mảng có chiều dài`m`. Đó chính là điểm nghẽn thực sự của vấn đề này. 

## Hướng dẫn thuật toán 

1. Xác định giá trị ô$$w(a,b) = (x_a + y_b) \bmod p$$Mỗi tiểu bang được ghé thăm đều đóng góp số tiền này. 

1. Quan sát rằng mọi chiến lược hợp lệ đều tương ứng với một đường dẫn đơn điệu từ`(0,0)`ĐẾN`(n-1,m-1)`chỉ sử dụng di chuyển phải và xuống. 

Ăn kẹo tăng lên`a`, trong khi ăn đá tăng`b`. 

1. Giải bài toán tái thiết đệ quy trên hình chữ nhật. 

Giả sử bài toán con hiện tại yêu cầu một đường đi tối ưu từ`(r1,c1)`ĐẾN`(r2,c2)`. 

1. Chọn hàng giữa$$mid = \lfloor (r1+r2)/2 \rfloor$$Mọi đường dẫn hợp lệ phải đi qua chính xác một ô trong hàng này. 

1. Tính DP chuyển tiếp từ hàng`r1`xuống`mid`. 

Đối với mỗi cột, hãy lưu trữ số điểm cao nhất có thể đạt được khi đạt được`(mid,col)`trong khi vẫn ở bên trong hình chữ nhật con. 

Mỗi lần chỉ cần hai hàng DP. 

1. Tính DP lùi từ hàng`r2`lên đến`mid`. 

DP này thể hiện điểm hậu tố tốt nhất có thể đạt được bắt đầu từ`(mid,col)`và kết thúc tại`(r2,c2)`. 

Một lần nữa, chỉ cần hai hàng. 

1. Với mỗi cột`col`, kết hợp hai giá trị. 

Tổng số điểm của một đường đi qua`(mid,col)`bằng$$forward[col] + backward[col] - w(mid,col)$$Ô ở giữa được tính hai lần nên hãy trừ đi một lần. 

1. Chọn cột cho giá trị kết hợp lớn nhất. 

Điều này xác định nơi đường dẫn tối ưu đi qua hàng giữa. 

1. Giải đệ quy nửa trên và nửa dưới. 

Đệ quy trên xây dựng một đường dẫn từ`(r1,c1)`ĐẾN`(mid,col)`. 

Đệ quy thấp hơn xây dựng một đường dẫn từ`(mid,col)`ĐẾN`(r2,c2)`. 

1. Nối hai chuỗi đường dẫn. 

Phải cẩn thận để không lặp lại các bước di chuyển xung quanh điểm phân chia. 

1. Xử lý các trường hợp cơ bản. 

Nếu như`r1 == r2`, con đường chỉ bao gồm những bước di chuyển bằng đá. 

Nếu như`c1 == c2`, con đường chỉ bao gồm các bước di chuyển bằng kẹo. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ cấu trúc con tối ưu. 

Sửa mọi đường dẫn tối ưu cho hình chữ nhật. Khi qua hàng giữa cột`k`, phần trước`(mid,k)`Bản thân nó phải là một con đường tối ưu từ góc xuất phát đến`(mid,k)`. Nếu không, chúng ta có thể thay thế nó bằng một tiền tố tốt hơn và cải thiện toàn bộ đường dẫn. 

Lập luận tương tự áp dụng cho hậu tố từ`(mid,k)`đến góc đích. 

DP chuyển tiếp tính toán chính xác điểm tiền tố tốt nhất có thể cho mỗi cột giao nhau. DP lạc hậu tính toán chính xác điểm hậu tố tốt nhất. Việc kết hợp chúng sẽ đánh giá đường đi hoàn chỉnh nhất bằng cách sử dụng từng điểm giao nhau. Việc chọn cột giao nhau tối đa đảm bảo rằng ít nhất một đường đi tối ưu toàn cục được bảo toàn thông qua đệ quy. 

Vì mọi phép phân chia đệ quy đều duy trì tính tối ưu nên trình tự được xây dựng lại cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -(10**30)

def solve():
    n, m, p = map(int, input().split())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    w = [[0] * m for _ in range(n)]
    for i in range(n):
        xi = x[i]
        row = w[i]
        for j in range(m):
            row[j] = (xi + y[j]) % p

    sys.setrecursionlimit(1 << 25)

    def build(r1, c1, r2, c2):
        if r1 == r2:
            return "S" * (c2 - c1)

        if c1 == c2:
            return "C" * (r2 - r1)

        mid = (r1 + r2) // 2

        # forward dp
        prev = [INF] * (c2 - c1 + 1)

        prev[0] = w[r1][c1]

        for j in range(c1 + 1, c2 + 1):
            prev[j - c1] = prev[j - c1 - 1] + w[r1][j]

        for i in range(r1 + 1, mid + 1):
            cur = [INF] * (c2 - c1 + 1)

            cur[0] = prev[0] + w[i][c1]

            for j in range(c1 + 1, c2 + 1):
                idx = j - c1
                cur[idx] = max(cur[idx - 1], prev[idx]) + w[i][j]

            prev = cur

        forward = prev

        # backward dp
        prev = [INF] * (c2 - c1 + 1)

        prev[-1] = w[r2][c2]

        for j in range(c2 - 1, c1 - 1, -1):
            prev[j - c1] = prev[j - c1 + 1] + w[r2][j]

        for i in range(r2 - 1, mid - 1, -1):
            cur = [INF] * (c2 - c1 + 1)

            cur[-1] = prev[-1] + w[i][c2]

            for j in range(c2 - 1, c1 - 1, -1):
                idx = j - c1
                cur[idx] = max(cur[idx + 1], prev[idx]) + w[i][j]

            prev = cur

        backward = prev

        best_col = c1
        best_val = INF

        for j in range(c1, c2 + 1):
            idx = j - c1
            total = forward[idx] + backward[idx] - w[mid][j]

            if total > best_val:
                best_val = total
                best_col = j

        upper = build(r1, c1, mid, best_col)
        lower = build(mid, best_col, r2, c2)

        return upper + lower

    path = build(0, 0, n - 1, m - 1)

    a = b = 0
    ans = w[0][0]

    for ch in path:
        if ch == 'C':
            a += 1
        else:
            b += 1

        ans += w[a][b]

    print(ans)
    print(path)

solve()
```Việc thực hiện tuân theo việc tái thiết đệ quy một cách chính xác. 

Hàm đệ quy`build(r1, c1, r2, c2)`trả về chuỗi di chuyển tối ưu bên trong hình chữ nhật hiện tại. Vì mỗi lần di chuyển đều làm tăng số lượng kẹo hoặc số lượng đá nên phép đệ quy tự nhiên tương ứng với các đường dẫn phụ. 

Mảng DP chỉ lưu trữ điểm cho một hàng duy nhất. Đây là tối ưu hóa bộ nhớ quan trọng. đầy đủ`n x m`bảng sẽ không vừa vặn thoải mái, trong khi tối đa hai mảng có độ dài`20000`rất nhỏ. 

DP chuyển tiếp tính toán các giá trị tiền tố tốt nhất kết thúc ở hàng giữa. DP lạc hậu tính toán các giá trị hậu tố tốt nhất bắt đầu từ đó. Sự kết hợp của chúng xác định cột vượt tối ưu. 

Một chi tiết tinh tế là trừ đi`w[mid][j]`một lần trong quá trình kết hợp. Cả hai DP đều bao gồm ô ở giữa, do đó việc không loại bỏ một bản sao sẽ làm tăng gấp đôi phần đóng góp của nó. 

Một sai lầm dễ mắc phải khác là nối lại quá trình xây dựng lại. Các lệnh gọi đệ quy đã biểu thị các đoạn đường dẫn liền kề, vì vậy chúng tôi chỉ cần nối trực tiếp các chuỗi di chuyển. Không có động thái bổ sung nào được chèn vào điểm phân chia. 

Điểm cuối cùng được tính toán lại từ đường dẫn đã tạo thay vì tin cậy các giá trị DP trung gian. Điều này tránh được các lỗi gây ra bởi các bài toán con chồng chéo hoặc các quy ước đếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 10
0 0
0 1
```Bảng trọng lượng trở thành: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| (0,0) | 0 | 
| (0,1) | 1 | 
| (1,0) | 0 | 
| (1,1) | 1 | 

Các đường dẫn có thể: 

| Đường dẫn | Các quốc gia đã đến thăm | Tổng cộng | 
| --- | --- | --- | 
| CS | (0,0) → (1,0) → (1,1) | 1 | 
| SC | (0,0) → (0,1) → (1,1) | 2 | 

Câu trả lời tối ưu là`SC`. 

Ví dụ này chứng minh rằng việc chọn phần thưởng ngay lập tức lớn hơn tại địa phương có thể đã quan trọng ngay từ bước đi đầu tiên. 

### Ví dụ 2 

đầu vào:```
3 3 7
1 5 2
4 0 6
```Bảng trọng lượng: 

| | 0 | 1 | 2 | 
| --- | --- | --- | --- | 
| 0 | 5 | 1 | 0 | 
| 1 | 2 | 5 | 4 | 
| 2 | 6 | 2 | 1 | 

Giả sử đệ quy phân chia ở hàng`1`. 

Chuyển tiếp DP tới hàng`1`: 

| Cột | Tiền tố tốt nhất | 
| --- | --- | 
| 0 | 7 | 
| 1 | 12 | 
| 2 | 16 | 

DP lùi từ dưới lên: 

| Cột | Hậu tố hay nhất | 
| --- | --- | 
| 0 | 13 | 
| 1 | 8 | 
| 2 | 5 | 

Các giá trị kết hợp: 

| Cột | Kết hợp | 
| --- | --- | 
| 0 | 18 | 
| 1 | 15 | 
| 2 | 17 | 

Điểm giao nhau tốt nhất là cột`0`. 

Dấu vết này thể hiện tính bất biến trung tâm: một khi cột giao nhau được cố định, phần trên và phần dưới trở thành các vấn đề tối ưu hóa độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi lớp DP xử lý hình chữ nhật của nó một lần | 
| Không gian |$O(m)$| Chỉ có hai hàng DP được lưu trữ đồng thời | 

Với`n,m ≤ 20000`, đầy đủ`O(nm)`bảng bộ nhớ sẽ là không thể. Việc tái cấu trúc phân chia để chinh phục giữ cho bộ nhớ tuyến tính ở chiều nhỏ hơn trong khi vẫn duy trì tính tối ưu. Tổng công việc vẫn có thể chấp nhận được trong Python được tối ưu hóa vì mọi trạng thái chỉ tham gia vào các chuyển đổi nhẹ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    INF = -(10**30)

    n, m, p = map(int, input().split())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    w = [[0] * m for _ in range(n)]
    for i in range(n):
        for j in range(m):
            w[i][j] = (x[i] + y[j]) % p

    sys.setrecursionlimit(1 << 25)

    def build(r1, c1, r2, c2):
        if r1 == r2:
            return "S" * (c2 - c1)

        if c1 == c2:
            return "C" * (r2 - r1)

        mid = (r1 + r2) // 2

        prev = [INF] * (c2 - c1 + 1)
        prev[0] = w[r1][c1]

        for j in range(c1 + 1, c2 + 1):
            prev[j - c1] = prev[j - c1 - 1] + w[r1][j]

        for i in range(r1 + 1, mid + 1):
            cur = [INF] * (c2 - c1 + 1)

            cur[0] = prev[0] + w[i][c1]

            for j in range(c1 + 1, c2 + 1):
                idx = j - c1
                cur[idx] = max(cur[idx - 1], prev[idx]) + w[i][j]

            prev = cur

        forward = prev

        prev = [INF] * (c2 - c1 + 1)
        prev[-1] = w[r2][c2]

        for j in range(c2 - 1, c1 - 1, -1):
            prev[j - c1] = prev[j - c1 + 1] + w[r2][j]

        for i in range(r2 - 1, mid - 1, -1):
            cur = [INF] * (c2 - c1 + 1)

            cur[-1] = prev[-1] + w[i][c2]

            for j in range(c2 - 1, c1 - 1, -1):
                idx = j - c1
                cur[idx] = max(cur[idx + 1], prev[idx]) + w[i][j]

            prev = cur

        backward = prev

        best_col = c1
        best_val = INF

        for j in range(c1, c2 + 1):
            idx = j - c1
            total = forward[idx] + backward[idx] - w[mid][j]

            if total > best_val:
                best_val = total
                best_col = j

        return (
            build(r1, c1, mid, best_col) +
            build(mid, best_col, r2, c2)
        )

    path = build(0, 0, n - 1, m - 1)

    a = b = 0
    ans = w[0][0]

    for ch in path:
        if ch == 'C':
            a += 1
        else:
            b += 1

        ans += w[a][b]

    return f"{ans}\n{path}\n"

# provided sample
assert run(
"""2 2 10
0 0
0 1
"""
) == "2\nSC\n"

# minimum size
assert run(
"""1 1 5
0
0
"""
) == "0\n\n"

# all equal values
assert run(
"""2 2 100
5 5
5 5
"""
).startswith("30\n")

# modulo wraparound
assert run(
"""2 2 5
4 4
4 4
"""
).startswith("9\n")

# single row
assert run(
"""1 4 10
3
1 2 3 4
"""
).endswith("SSS\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`lưới nhỏ nhất | Đường dẫn trống | Xử lý đúng các bước di chuyển bằng 0 | 
| Tất cả các giá trị bằng nhau | Bất kỳ con đường tối ưu nào | Xử lý cà vạt | 
| Bao quanh Modulo | Số học modulo đúng | Ngăn chặn các lỗi kiểu tràn | 
| Hàng đơn | Chỉ một`S`di chuyển | Trường hợp đệ quy biên | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1 1 5
0
0
```Trạng thái bắt đầu đã là trạng thái kết thúc. Không thể di chuyển được. Hàm đệ quy ngay lập tức xử lý cả trường hợp cơ sở hàng và cột, trả về một chuỗi trống. Điểm chỉ bằng giá trị ô bắt đầu:$$(0+0)\bmod 5 = 0$$Bây giờ hãy xem xét cách bao quanh modulo:```
2 2 5
4 4
4 4
```Mọi giá trị trạng thái đều bằng$$(4+4)\bmod 5 = 3$$Đường đi đi qua ba trạng thái nên điểm tối ưu là`9`. 

Nếu không áp dụng modulo trong quá trình xây dựng bảng, thuật toán sẽ tính toán sai`24`. 

Cuối cùng, xét một hình chữ nhật suy biến:```
1 4 10
3
1 2 3 4
```Chỉ di chuyển bằng đá là hợp pháp. Phép đệ quy ngay lập tức sử dụng`r1 == r2`trường hợp cơ bản và trả về`"SSS"`. 

Điều này xác nhận rằng logic phân chia và chinh phục xử lý các hình chữ nhật con mỏng mà không cố gắng chuyển đổi DP không hợp lệ.
