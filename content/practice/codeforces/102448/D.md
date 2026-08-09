---
title: "CF 102448D – Uống đến đỏ mặt"
description: "Chúng ta có một điểm cố định ((x,y)) nơi đặt chiếc nhẫn ma thuật. Mọi vòng thông thường đều có tâm ((Xi,Yi)) và bán kính (Ri). Chiếc nhẫn ma thuật bắt đầu với một số bán kính (r) và bất cứ khi nào nó hấp thụ một chiếc nhẫn thông thường, bán kính của nó sẽ tăng theo bán kính của chiếc nhẫn đó."
date: "2026-08-08T12:07:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "D"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 589
verified: true
draft: false
---

[CF 102448D - Uống đến đỏ](https://codeforces.com/problemset/problem/102448/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một điểm cố định ((x,y)) nơi đặt chiếc nhẫn ma thuật. Mọi vành thông thường đều có tâm ((X_i,Y_i)) và bán kính (R_i). Chiếc nhẫn ma thuật bắt đầu với một số bán kính (r) và bất cứ khi nào nó hấp thụ một chiếc nhẫn thông thường, bán kính của nó sẽ tăng theo bán kính của chiếc nhẫn đó. 

Giả sử chiếc nhẫn ma thuật hiện có bán kính (C). Một vòng thường (i) có thể bị hấp thụ chính xác khi hai vòng tròn cắt nhau hoặc chạm nhau, điều này tương đương với 

[ 
\sqrt{(X_i-x)^2+(Y_i-y)^2} \le C+R_i. 
] 

Sau khi hấp thụ nó, bán kính hiện tại trở thành (C+R_i). Nhiệm vụ là chọn (r) ban đầu nhỏ nhất có thể để cho phép mọi vòng được hấp thụ theo một thứ tự nào đó. 

Khó khăn chính là việc hấp thụ một vòng làm thay đổi bán kính, do đó các vòng không thể được xử lý độc lập một cách đơn giản. Một chiếc nhẫn ban đầu không thể tiếp cận được có thể trở nên có thể tiếp cận được sau khi hấp thụ một số vòng dễ dàng hơn. 

Có tới (10^5) vòng, trong khi tọa độ có thể có độ lớn gần (10^9). Điều này loại trừ mọi phép tính bậc hai trong (N), vì các phép toán (10^{10}) vượt xa giới hạn hai giây. Chúng ta cần một giải pháp (O(N\log N)) hoặc giải pháp nào đó tương đương hiệu quả. Giới hạn tọa độ cũng có nghĩa là khoảng cách bình phương có thể đạt tới khoảng (8\cdot10^{18}), do đó việc tính toán khoảng cách cần có đủ độ chính xác số nguyên trước khi lấy căn bậc hai. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Đầu tiên, bán kính ban đầu cần thiết thực sự có thể bằng không. Ví dụ,```
1 0 0
0 0 2
```có câu trả lời`0.0000000000`. Chiếc nhẫn ma thuật đã giao nhau với chiếc nhẫn thông thường ngay cả khi có bán kính bằng 0, bởi vì khoảng cách giữa tâm của chúng bằng 0. Việc triển khai khởi tạo câu trả lời cho bán kính bắt buộc đầu tiên mà không cho phép số 0 có thể in sai giá trị dương. 

Tiếp tuyến là một trường hợp ranh giới khác. Coi như```
1 0 0
3 0 2
```Các tâm cách nhau chính xác khoảng cách (3) và tổng bán kính bằng (3), do đó các vòng tiếp xúc và hấp thụ được cho phép. Câu trả lời là```
1.0000000000
```Sử dụng một cách nghiêm ngặt`<`thay vì`<=`sẽ từ chối sự hấp thụ hợp lệ này. 

Thứ tự hấp thụ cũng có vấn đề. Coi như```
2 0 0
100 0 99
3 0 1
```Đối với vòng đầu tiên, bán kính dòng điện yêu cầu là (100-99=1). Đối với lần thứ hai, nó là (3-1=2). Vòng đầu tiên nên được hấp thụ trước. Bắt đầu với bán kính (1), vòng ma thuật sẽ hấp thụ vòng bán kính-(99) và phát triển đến bán kính (100), sau đó vòng thứ hai sẽ dễ dàng. Câu trả lời đúng là`1.0000000000`. Thay vào đó, việc sắp xếp theo khoảng cách trung tâm sẽ xử lý vòng khoảng cách-(3) trước và kết luận sai rằng bán kính (2) là cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp là xem xét mọi thứ tự hấp thụ có thể. Đối với một đơn hàng cố định, bán kính ban đầu yêu cầu tối thiểu rất dễ tính toán. Trước khi vòng hấp thụ (i), chúng ta biết tổng bán kính của tất cả các vòng được hấp thụ trước đó, vì vậy chúng ta có thể xác định chính xác bán kính ban đầu phải lớn đến mức nào để bước đó có thể thực hiện được. Lấy mức tối đa trên toàn bộ đơn hàng sẽ cho ra bán kính bắt đầu cần thiết cho đơn hàng đó. 

Vấn đề là số lượng đơn đặt hàng. Có (N!) hoán vị và việc đánh giá một hoán vị mất (O(N)) thời gian, mang lại công việc (O(N\cdot N!)). Ngay cả (N=20) cũng sẽ yêu cầu khoảng (20\cdot20!), khoảng (4,9\cdot10^{19}), kiểm tra vòng. Với (N=10^5), việc sắp xếp đầy đủ là hoàn toàn không thể. 

Cấu trúc trở nên đơn giản hơn nhiều nếu chúng ta viết lại điều kiện hấp thụ. hãy để 

[ 
d_i=\sqrt{(X_i-x)^2+(Y_i-y)^2} 
] 

là khoảng cách từ tâm vòng tròn ma thuật đến tâm vòng (i). Vòng (i) có thể hấp thụ được khi 

[ 
d_i\le C+R_i, 
] 

hoặc tương đương, 

[ 
d_i-R_i\le C. 
] 

Xác định 

[ 
A_i=d_i-R_i. 
] 

Giờ đây, mỗi chiếc nhẫn chỉ đơn giản là một vật thể sẽ có sẵn khi bán kính ma thuật hiện tại đạt đến ngưỡng (A_i). Hấp thụ nó sẽ thêm (R_i) vào bán kính hiện tại. 

Tất cả phần thưởng (R_i) đều dương. Điều đó thay đổi hoàn toàn vấn đề đặt hàng. Nếu hai vòng có ngưỡng (A_i\le A_j) và vòng có ngưỡng (A_j) hiện có sẵn thì vòng có ngưỡng (A_i) cũng có sẵn. Việc hấp thụ vòng ngưỡng nhỏ hơn trước tiên không gây hại gì vì nó chỉ làm tăng bán kính hiện tại trước khi xem xét vòng còn lại. 

Điều này có nghĩa là chúng ta có thể sắp xếp tất cả các vành theo (A_i). Sau khi được sắp xếp, không cần phải mô phỏng các lựa chọn phức tạp. Trước vòng thứ (i) theo thứ tự này, bán kính hiện tại đã đạt được bán kính của tất cả các vòng trước đó. Nếu tổng bán kính của chúng là (S), thì chúng ta cần 

[ 
r+S\ge A_i, 
] 

vậy 

[ 
r\ge A_i-S. 
] 

Do đó, bán kính ban đầu tối thiểu là mức tối đa của các yêu cầu này, cùng với 0 vì bán kính không thể âm. 

Lực lượng vũ phu hoạt động vì một mệnh lệnh hoàn chỉnh cho phép chúng tôi kiểm tra mọi trình tự có thể. Nó thất bại vì có nhiều trình tự. Việc quan sát rằng các vòng có thể được sắp xếp theo lượng bán kính mà chúng cần trước khi có thể truy cập được sẽ giảm toàn bộ vấn đề về sắp xếp và tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N\cdot N!)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi chiếc nhẫn thông thường, hãy tính khoảng cách của nó đến tâm chiếc nhẫn ma thuật cố định: 

[ 
d_i=\sqrt{(X_i-x)^2+(Y_i-y)^2}. 
] 

Sau đó tính bán kính hiện tại yêu cầu của nó 

[ 
A_i=d_i-R_i. 
] 

Giá trị này là bán kính vòng ma thuật nhỏ nhất cho phép vòng (i) được hấp thụ ngay lập tức. 

1. Lưu mỗi vòng dưới dạng cặp ((A_i,R_i)), sau đó sắp xếp tất cả các cặp theo (A_i). 

Thứ tự sắp xếp là hợp lệ vì vòng có ngưỡng nhỏ hơn không bao giờ khó hấp thụ hơn vòng có ngưỡng lớn hơn. Nếu có sẵn một vòng ngưỡng lớn hơn thì mọi vòng ngưỡng nhỏ hơn cũng có sẵn và việc hấp thụ vòng ngưỡng nhỏ hơn chỉ làm tăng bán kính. 

1. Đặt`added = 0`, biểu thị tổng bán kính thu được từ các vòng đã được hấp thụ và đặt`answer = 0`. 

Trước khi xử lý một chiếc nhẫn, bán kính hiện tại của chiếc nhẫn ma thuật là`answer + added`chỉ về mặt khái niệm. Trực tiếp hơn,`answer`đại diện cho bán kính ban đầu mà chúng tôi đang cố gắng tạo ra đủ, trong khi`added`đại diện cho bán kính đạt được sau những lần hấp thụ trước đó. 

1. Với mỗi cặp được sắp xếp ((A_i,R_i)), hãy tính 

[ 
A_i-đã thêm. 
] 

Bán kính ban đầu ít nhất phải bằng giá trị này vì các vòng trước đó đã tăng bán kính của vòng ma thuật lên`added`. Cập nhật câu trả lời với giá trị tối đa hiện tại và yêu cầu này. 

1. Thêm (R_i) vào`added`và tiếp tục. 

Mỗi vòng chỉ đóng góp bán kính của nó sau khi đã đạt được thành công, đó chính xác là cách hoạt động của quá trình hấp thụ thực sự. 

1. In kết quả tối đa với độ chính xác thập phân vừa đủ. 

Tính toán khoảng cách sử dụng hiệu số nguyên trước khi lấy căn bậc hai, do đó khoảng cách bình phương được tính chính xác bằng Python. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý các vòng (i) đầu tiên theo thứ tự ngưỡng được sắp xếp,`added`chính xác là tổng bán kính của chúng, và`answer`là bán kính ban đầu nhỏ nhất giúp cho mọi quá trình hấp thụ được xử lý đều có thể thực hiện được. 

Đối với vòng tiếp theo, ngưỡng của nó là (A_i). Các vòng trước đã tăng bán kính ma thuật lên`added`, vì vậy sự hấp thụ tiếp theo có thể xảy ra chính xác khi 

[ 
câu trả lời+đã thêm\ge A_i. 
] 

Do đó thuật toán phải thực thi (answer\ge A_i- Added). Lấy mức tối đa trên tất cả các vòng được xử lý sẽ thỏa mãn mọi điều kiện như vậy. 

Câu hỏi còn lại là tại sao thứ tự ngưỡng được sắp xếp là đủ. Giả sử một số thứ tự hấp thụ khả thi chứa hai vòng liên tiếp có ngưỡng (A_j>A_i), trong đó (j) được hấp thụ trước (i). Vì (j) có thể truy cập được nên bán kính hiện tại ít nhất là (A_j) và do đó cũng ít nhất là (A_i). Chúng ta có thể hoán đổi chúng, hấp thụ (i) trước, lấy bán kính dương của nó và vẫn có đủ bán kính để hấp thụ (j). Việc loại bỏ nhiều lần các phép đảo ngược như vậy sẽ biến đổi bất kỳ thứ tự khả thi nào thành thứ tự ngưỡng không giảm mà không làm tăng bán kính ban đầu cần thiết. Do đó thứ tự sắp xếp chứa một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n, x, y = map(int, input().split())

    rings = []

    for _ in range(n):
        X, Y, R = map(int, input().split())

        dx = X - x
        dy = Y - y

        distance = math.hypot(dx, dy)
        need = distance - R

        rings.append((need, R))

    rings.sort()

    answer = 0.0
    added = 0

    for need, radius in rings:
        answer = max(answer, need - added)
        added += radius

    print(f"{answer:.10f}")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào trước tiên sẽ chuyển đổi mọi vòng tròn thành cặp quan trọng đối với thuật toán tham lam. Tọa độ trung tâm ban đầu không còn cần thiết sau khi tính toán khoảng cách.`math.hypot(dx, dy)`tính toán (\sqrt{dx^2+dy^2}). Sự khác biệt về tọa độ là số nguyên và số nguyên Python có độ chính xác tùy ý, do đó không có hiện tượng tràn số nguyên khi khoảng cách bình phương được hình thành bên trong. 

Phép trừ`distance - R`được thực hiện một cách có chủ ý trước khi phân loại. Sắp xếp theo`distance`riêng thì không đúng vì một vòng lớn có thể dễ hấp thụ hơn một vòng nhỏ hơn khi bán kính của nó lớn. Số lượng liên quan luôn luôn`distance - radius`. 

Vòng lặp giữ`added`dưới dạng số nguyên. Điều này rất hữu ích vì tổng bán kính lên tới (10^5) tối đa là (10^{10}), chính xác là trong Python. Chỉ ngưỡng hình học chứa căn bậc hai và cần số học dấu phẩy động. 

Quá trình cập nhật diễn ra trước khi thêm bán kính của vòng hiện tại. Thứ tự này phù hợp với quá trình vật lý: vòng hiện tại không thể đóng góp bán kính của nó cho đến khi nó được hấp thụ. Việc thêm đầu tiên sẽ gây ra lỗi sai từng bước và có thể làm cho vòng không thể truy cập ban đầu có vẻ như có thể truy cập được. 

Việc so sánh trong điều kiện toán học là không nghiêm ngặt. Một vòng tiếp tuyến có thể được hấp thụ, do đó ngưỡng được thỏa mãn khi bán kính hiện tại chính xác bằng`need`. Công thức xử lý sự bình đẳng một cách tự nhiên mà không yêu cầu trường hợp đặc biệt. 

Cuối cùng,`answer`bắt đầu từ 0 vì bán kính yêu cầu có thể không dương. Nếu mọi vòng đã giao với một vòng ma thuật có bán kính bằng 0 sau khi tính bán kính của chính nó thì không cần bán kính bắt đầu dương. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, trung tâm kỳ diệu là ((1,1)). Hai vành thường ở ((1,7)) có bán kính (3) và ((5,1)) có bán kính (3). 

Khoảng cách và ngưỡng của chúng là: 

| Nhẫn | Khoảng cách | Bán kính | (A_i=d_i-R_i) |`added`trước | Bán kính ban đầu bắt buộc | 
| --- | --- | --- | --- | --- | --- | 
| ((5,1)) | 4 | 3 | 1 | 0 | 1 | 
| ((1,7)) | 6 | 3 | 3 | 3 | 0 | 

Thứ tự sắp xếp bắt đầu từ vòng đầu vào thứ hai vì ngưỡng của nó chỉ là (1). Bắt đầu từ bán kính (1), chiếc nhẫn ma thuật có thể hấp thụ nó và phát triển đến bán kính (4). Vòng còn lại chỉ cần bán kính (3) nên bị hấp thụ ngay lập tức. 

Bán kính ban đầu được yêu cầu tối đa là (1), tạo ra đầu ra mẫu. 

Đối với Mẫu 2, bốn ngưỡng xấp xỉ: 

| Nhẫn | Khoảng cách | Bán kính | (A_i=d_i-R_i) |`added`trước | Bán kính ban đầu bắt buộc | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 160.015625 | 41 | 119.015625 | 0 | 119.015625 | 
| 4 | 245.943083 | 78 | 167.943083 | 41 | 126.943083 | 
| 1 | 836.244584 | 6 | 830.244584 | 119 | 711.244584 | 
| 2 | 1025.353110 | 30 | 995.353110 | 125 | 870.353110 | 

Vòng đầu vào thứ ba dễ tiếp cận nhất nên nó được hấp thụ trước và tăng bán kính ma thuật lên (41). Sau đó, vòng thứ tư có thể truy cập được, thêm một vòng khác (78). Sau hai lần hấp thụ này, chiếc nhẫn ma thuật đã đạt được bán kính (119). 

Vòng đầu tiên yêu cầu đóng góp ban đầu chỉ khoảng (711,245) sau khi tính đến (119) đơn vị đó. Vòng cuối cùng là nút cổ chai thực sự. Vào thời điểm nó được xem xét, bán kính (125) đã đạt được nhưng ngưỡng của nó là khoảng (995,353110), do đó bán kính ban đầu ít nhất phải bằng 

[ 
995,353110-125=870,353110. 
] 

Điều đó mang lại đầu ra mẫu`870.3531099090`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Tính toán tất cả các khoảng cách mất (O(N)) và ngưỡng sắp xếp (N) mất (O(N\log N)). | 
| Không gian | (O(N)) | Cặp ngưỡng và bán kính cho mỗi vòng được lưu trữ trước khi sắp xếp. | 

Với (N\le10^5), việc sắp xếp các phần tử (10^5) trở nên dễ dàng thực tế trong giới hạn hai giây. Thuật toán chỉ thực hiện một lần truyền tuyến tính sau khi sắp xếp và mức sử dụng bộ nhớ của nó tỷ lệ thuận với số vòng, trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve(data: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(data)

    input = sys.stdin.readline

    n, x, y = map(int, input().split())
    rings = []

    for _ in range(n):
        X, Y, R = map(int, input().split())

        dx = X - x
        dy = Y - y

        distance = math.hypot(dx, dy)
        need = distance - R

        rings.append((need, R))

    rings.sort()

    answer = 0.0
    added = 0

    for need, radius in rings:
        answer = max(answer, need - added)
        added += radius

    sys.stdin = old_stdin
    return f"{answer:.10f}"

def run(inp: str) -> str:
    return solve(inp)

# Provided sample 1
out = float(run("""\
2 1 1
1 7 3
5 1 3
"""))
assert abs(out - 1.0) < 1e-9, "sample 1"

# Provided sample 2
out = float(run("""\
4 211 -458
335 369 6
-771 -753 30
193 -617 41
-27 -396 78
"""))
assert abs(out - 870.3531099090) < 1e-6, "sample 2"

# Minimum-size input, zero initial radius
out = float(run("""\
1 0 0
0 0 2
"""))
assert abs(out - 0.0) < 1e-9, "zero-radius answer"

# Tangency must be accepted
out = float(run("""\
1 0 0
3 0 2
"""))
assert abs(out - 1.0) < 1e-9, "tangency"

# Sorting must use distance - radius, not distance
out = float(run("""\
2 0 0
100 0 99
3 0 1
"""))
assert abs(out - 1.0) < 1e-9, "threshold ordering"

# Boundary coordinates and a large distance
out = float(run("""\
1 -999999999 1000000000
1000000000 -999999999 2
"""))
assert abs(out - (1999999999 * math.sqrt(2) - 2)) < 1e-6, "coordinate boundary"

# Maximum-size input, all equal values
n = 100000
lines = [f"{n} 0 0"]
lines.extend(["0 0 2"] * n)
out = float(run("\n".join(lines) + "\n"))
assert abs(out - 0.0) < 1e-9, "maximum-size equal input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 0 / 0 0 2`|`0.0000000000`| Trường hợp kích thước tối thiểu và khả năng bán kính ban đầu bằng 0 | 
|`1 0 0 / 3 0 2`|`1.0000000000`| Tiếp tuyến và điều kiện hấp thụ không nghiêm ngặt | 
|`2 0 0 / 100 0 99 / 3 0 1`|`1.0000000000`| Sắp xếp theo`distance - radius`thay vì khoảng cách | 
| Trường hợp tọa độ biên | khoảng`2828427123.746...`| Chênh lệch tọa độ lớn và độ chính xác về số | 
| 100000 chiếc nhẫn giống hệt nhau ở trung tâm |`0.0000000000`| Tối đa (N), giá trị lặp lại, phân loại và hiệu suất quét tuyến tính | 

## Vỏ cạnh 

Trường hợp bán kính bằng 0 được xử lý vì câu trả lời bắt đầu từ`0.0`và mọi yêu cầu đều được xem xét tương ứng với giá trị đó. Vì```
1 0 0
0 0 2
```khoảng cách là (0), do đó ngưỡng là (0-2=-2). Do đó, bán kính ban đầu được yêu cầu là (-2), nhưng bán kính vật lý không thể âm, do đó giá trị cực đại bằng 0 vẫn giữ nguyên (0). Chiếc nhẫn có thể được hấp thụ ngay lập tức và thuật toán in`0.0000000000`. 

Trường hợp tiếp tuyến sử dụng đẳng thức một cách trực tiếp. Vì```
1 0 0
3 0 2
```ngưỡng là (3-2=1). Với bán kính ban đầu chính xác (1), hai đường tròn có khoảng cách tâm (3) và bán kính tổng hợp (1+2=3) nên chúng chạm nhau. Điều kiện được thỏa mãn chính xác và câu trả lời là`1.0000000000`. Không cần điều chỉnh epsilon hoặc so sánh nghiêm ngặt. 

Trường hợp cạnh thứ tự giải thích tại sao ngưỡng lại bao gồm bán kính của chính vòng. Vì```
2 0 0
100 0 99
3 0 1
```các ngưỡng lần lượt là (1) và (2). Thứ tự sắp xếp lấy vòng bán kính-(99) trước. Bắt đầu với bán kính (1), chiếc nhẫn ma thuật tiếp cận chính xác nó, hấp thụ nó và tăng lên bán kính (100). Sau đó, chiếc nhẫn thứ hai có thể tiếp cận được một cách tầm thường. Thuật toán ghi lại các yêu cầu (1) và (2-99=-97), do đó đáp án cuối cùng vẫn là (1). Sắp xếp theo khoảng cách trung tâm sẽ đảo ngược thứ tự và tạo ra câu trả lời sai. 

Tọa độ lớn không gây tràn số nguyên trong Python. Ví dụ: trường hợp ranh giới sử dụng tâm gần ((-10^9,10^9)) và vòng gần ((10^9,-10^9)). Chênh lệch tọa độ gần bằng (2\cdot10^9), do đó bình phương khoảng cách là khoảng (8\cdot10^{18}). Python tính toán số nguyên đó một cách chính xác trước đó`math.hypot`chuyển đổi hình học thành dấu phẩy động. Lỗi dấu phẩy động thu được thấp hơn nhiều so với dung sai bắt buộc (10^{-6}) cho câu trả lời cuối cùng. 

Cuối cùng, nhiều vòng có thể có cùng một ngưỡng. Việc sắp xếp giữ chúng liền kề và thứ tự tương đối của chúng không thành vấn đề vì tất cả đều có thể truy cập được ở cùng bán kính hiện tại. Vì mọi bán kính đều dương nên việc hấp thụ bất kỳ một trong số chúng chỉ có thể làm cho các vòng còn lại dễ tiếp cận hơn. Thử nghiệm kích thước tối đa với (10^5) vòng giống hệt nhau thực hiện trường hợp này đồng thời xác nhận rằng việc triển khai (O(N\log N)) tỷ lệ với giới hạn đầu vào. 

Nếu bạn muốn, tôi cũng có thể biến nó thành một bài xã luận kiểu Codeforces nhỏ gọn hơn, phù hợp để xuất bản ngay sau phần trình bày vấn đề.
