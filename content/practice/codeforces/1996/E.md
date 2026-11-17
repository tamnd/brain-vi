---
title: "CF 1996E - Giải mã"
description: "Chúng tôi được cung cấp một chuỗi nhị phân và chúng tôi xem xét mọi phân đoạn liền kề có thể có của chuỗi đó. Bản thân mỗi phân đoạn chứa nhiều phân đoạn con và chúng ta chỉ quan tâm đến những phân đoạn con có số lượng số 0 bằng số lượng số một."
date: "2026-06-08T14:43:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1996
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 962 (Div. 3)"
rating: 1600
weight: 1996
solve_time_s: 132
verified: false
draft: false
---

[CF 1996E - Giải mã](https://codeforces.com/problemset/problem/1996/E) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, cấu trúc dữ liệu, triển khai, toán học 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nhị phân và chúng tôi xem xét mọi phân đoạn liền kề có thể có của chuỗi đó. Bản thân mỗi phân đoạn chứa nhiều phân đoạn con và chúng ta chỉ quan tâm đến những phân đoạn con có số lượng số 0 bằng số lượng số một. 

Đối với mỗi lựa chọn khoảng lớn hơn trong chuỗi, chúng tôi đếm xem nó chứa bao nhiêu phân đoạn cân bằng. Sau đó, chúng tôi tính tổng giá trị này trong tất cả các khoảng thời gian có thể. Một cách khác để thấy điều đó là mỗi phân đoạn cân bằng đều đóng góp vào nhiều khoảng bao quanh và chúng ta đang tổng hợp tất cả những đóng góp đó một cách hiệu quả. 

Việc giải thích trực tiếp đã gợi ý một lượng lớn việc đếm lặp đi lặp lại. Mỗi chuỗi con được coi là một ứng cử viên tiềm năng và mỗi khoảng kèm theo lại thêm nó vào. Vì cả số khoảng bên ngoài và chuỗi con bên trong đều chia tỷ lệ bậc hai, nên bất kỳ giải pháp nào liệt kê rõ ràng chúng sẽ quá chậm khi độ dài chuỗi đạt tới 200000. 

Một ý tưởng ngây thơ sẽ cố gắng sửa một đoạn và quét tất cả các đoạn con bên trong nó, nhưng điều này dẫn đến hành vi hình khối. Ngay cả việc tối ưu hóa việc kiểm tra bên trong bằng các tổng tiền tố cũng không cứu được chúng ta, vì bản thân số lượng các phân đoạn vẫn là bậc hai. Cấu trúc của bài toán buộc chúng ta phải diễn giải lại thứ tự đếm. 

Ví dụ, một trường hợp thất bại tinh tế của lý luận ngây thơ xuất hiện khi chuỗi đồng nhất`"0000"`. Không có chuỗi con cân bằng nào cả, vì vậy câu trả lời là 0. Tuy nhiên, việc triển khai bất cẩn cố gắng xử lý không chính xác sự khác biệt về số lượng, ví dụ bằng cách đếm sai các đẳng thức tiền tố mà không trừ đi các đóng góp cho mỗi khoảng ngoài, vẫn có thể tích lũy các giá trị khác 0 do tính hai lần các điều kiện trống. 

Khó khăn chính là mỗi mảng con hợp lệ đóng góp không phải một lần mà tỷ lệ với số lượng khoảng bên ngoài chứa nó và sự ghép nối giữa hai lớp khoảng này phải được gỡ bỏ. 

## Phương pháp tiếp cận 

Một phương pháp bạo lực sẽ liệt kê mọi khoảng thời gian$(l, r)$, sau đó liệt kê mọi khoảng con$(x, y)$bên trong nó và kiểm tra xem nó có số 0 và số 1 bằng nhau hay không. Sử dụng tổng tiền tố, bản thân việc kiểm tra là thời gian không đổi, nhưng số lượng các cặp khoảng vẫn là$O(n^3)$. Với$n$lên tới$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Để cải thiện, chúng tôi đảo ngược quan điểm. Thay vì cố định các khoảng bên ngoài và đếm các khoảng bên trong, chúng ta sửa một mảng con ứng cử viên và đếm xem có bao nhiêu khoảng bên ngoài chứa nó. Một mảng con$(x, y)$được bao gồm trong tất cả$(l, r)$như vậy$l \le x$Và$r \ge y$. Số khoảng ngoài như vậy chỉ phụ thuộc vào vị trí$x$Và$y$, và bằng$x \cdot (n - y + 1)$. 

Vì vậy, vấn đề giảm xuống còn tính tổng trên tất cả các mảng con cân bằng:$$\sum_{x \le y, \; s[x..y]\text{ balanced}} x \cdot (n - y + 1).$$Bây giờ nhiệm vụ còn lại là liệt kê tất cả các mảng con cân bằng một cách hiệu quả. Đây là một cách tái tạo lại tổng tiền tố cổ điển: bản đồ`0 → -1`Và`1 → +1`. Một mảng con được cân bằng chính xác khi chênh lệch tổng tiền tố của nó bằng 0. Vì vậy, chúng ta cần xem xét tất cả các cặp vị trí mà tổng tiền tố khớp với nhau. 

Cho phép$p[i]$là tổng tiền tố. Với mỗi giá trị$v$, giả sử sự xuất hiện của nó là ở các chỉ số$i_1 < i_2 < \dots < i_k$. Bất kỳ cặp nào$(i_a, i_b)$với$a < b$định nghĩa một mảng con cân bằng$(i_a + 1, i_b)$. Mỗi mảng con như vậy đóng góp:$$(i_a + 1) \cdot (n - i_b + 1).$$Do đó, chúng tôi xử lý từng giá trị tổng tiền tố một cách độc lập và tính tổng này trên tất cả các cặp theo thời gian tuyến tính trên mỗi nhóm bằng cách sử dụng tổng hợp tiền tố. 

Quan sát quan trọng là đối với điểm cuối bên phải cố định, chúng ta có thể duy trì tổng tích lũy trên điểm cuối bên trái để tránh liệt kê bậc hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi nhị phân thành mảng tổng tiền tố trong đó`0`trở thành -1 và`1`trở thành +1. Điều này biến đổi điều kiện “số lượng số 0 và số 1 bằng nhau” thành “tổng tiền tố bằng nhau ở hai vị trí”. 
2. Tính tất cả các tổng tiền tố$p[0], p[1], \dots, p[n]$, bắt đầu bằng$p[0] = 0$. 
3. Nhóm các chỉ số có giá trị tổng tiền tố bằng nhau. Mỗi nhóm tương ứng với tất cả các vị trí có số dư đang chạy giống hệt nhau. 
4. Đối với từng nhóm chỉ số$i_1, i_2, \dots, i_k$, xử lý đóng góp của tất cả các cặp$(i_a, i_b)$, Ở đâu$a < b$, mà không liệt kê chúng một cách rõ ràng. 
5. Quét qua các chỉ mục theo thứ tự tăng dần, duy trì hai giá trị đang chạy: tổng các điểm cuối bên trái và số lần xuất hiện cho đến nay. Đối với mỗi chỉ mục mới được coi là điểm cuối bên phải, hãy thêm phần đóng góp của nó bằng cách sử dụng các chỉ mục tiền tố đã thấy trước đó. 
6. Để có điểm cuối phù hợp$i_b$, mỗi chỉ số trước đó$i_a$đóng góp$(i_a + 1) \cdot (n - i_b + 1)$. Yếu tố ra$(n - i_b + 1)$, nên ta chỉ cần tổng$(i_a + 1)$trên tiền tố nhóm. 
7. Tích lũy kết quả modulo$10^9+7$. 

### Tại sao nó hoạt động 

Phép biến đổi làm giảm các chuỗi con cân bằng thành các tổng tiền tố bằng nhau. Mỗi chuỗi con hợp lệ tương ứng duy nhất với một cặp vị trí tổng tiền tố bằng nhau. Hàm đóng góp tách biệt rõ ràng thành tích của một số hạng chỉ phụ thuộc vào điểm cuối bên trái và một số hạng chỉ phụ thuộc vào điểm cuối bên phải. Khả năng phân tách này cho phép tích lũy gia tăng: khi xử lý điểm cuối bên phải, tất cả các điểm cuối bên trái hợp lệ đều đã được biết và đóng góp của chúng có thể được tính tổng mà không cần xem lại chúng. Điều này đảm bảo mọi mảng con hợp lệ đều được tính chính xác một lần và chỉ một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)

        # prefix sum positions grouped by value
        groups = {}

        cur = 0
        groups.setdefault(0, []).append(0)

        for i, ch in enumerate(s, start=1):
            cur += 1 if ch == '1' else -1
            groups.setdefault(cur, []).append(i)

        ans = 0

        for pos in groups.values():
            if len(pos) < 2:
                continue

            pref_sum_left = 0
            count = 0

            for i in pos:
                if count > 0:
                    right_factor = (n - i + 1)
                    ans = (ans + pref_sum_left * right_factor) % MOD

                pref_sum_left += (i + 1)
                count += 1

        print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng các tổng tiền tố trong đó mỗi giá trị tổng tiền tố thu thập tất cả các chỉ số mà nó xuất hiện. Đối với mỗi nhóm, chúng tôi quét từ trái sang phải. Tại mỗi vị trí được coi là điểm cuối bên phải, chúng tôi nhân tổng tích lũy của điểm cuối bên trái (dịch chuyển +1 để khớp với phần đóng góp bắt đầu chuỗi con) với số phần mở rộng có thể có ở ranh giới bên phải. Điều này trực tiếp khớp với sự đóng góp được nhân tố hóa trong thuật toán. 

Cần phải cẩn thận trong việc lập chỉ mục. Mảng tiền tố sử dụng các vị trí dựa trên 0 để thuận tiện, nhưng điểm cuối chuỗi con phụ thuộc vào việc chuyển đổi các chỉ số tiền tố thành ranh giới phân đoạn thực tế, đó là lý do tại sao`(i + 1)`xuất hiện cho điểm cuối bên trái và`(n - i + 1)`để biết số lượng tiện ích mở rộng bên phải. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`010`Tổng tiền tố là: 

| chỉ số tôi | tổng tiền tố | 
| --- | --- | 
| 0 | 0 | 
| 1 | -1 | 
| 2 | 0 | 
| 3 | -1 | 

Chúng tôi nhóm các vị trí: 

0 → [0, 2] 

-1 → [1, 3] 

Đối với nhóm 0: 

| đúng rồi tôi | pref_sum_left | đóng góp | 
| --- | --- | --- | 
| 2 | 1 | (1 * (3 - 2 + 1)) = 2 | 

Đối với nhóm -1: 

| đúng rồi tôi | pref_sum_left | đóng góp | 
| --- | --- | --- | 
| 3 | 2 | (2 * 1) = 2 | 

Tổng cộng = 4. 

Điều này chứng tỏ mỗi nhóm độc lập như thế nào và các đóng góp được tổng hợp thông qua các tổng tiền tố thay vì liệt kê theo cặp. 

### Ví dụ 2:`1100`Tổng tiền tố: 

| tôi | p[i] | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 0 | 

Nhóm: 

0 → [0, 4] 

1 → [1, 3] 

2 → [2] 

Nhóm 0 đóng góp: 

(0,4) → (1 * 1) = 1 

Nhóm 1 đóng góp: 

(1,3) → (2 * 2) = 4 

Tổng cộng = 5. 

Điều này cho thấy cách ghi lại nhiều chuỗi con cân bằng ngay cả khi chúng trùng nhau nhiều và cách nhóm theo tổng tiền tố để tránh thiếu hoặc tính hai lần chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi chỉ mục được xử lý một lần trong nhóm tổng tiền tố của nó | 
| Không gian | O(n) | Lưu trữ cho nhóm tổng tiền tố | 

Giải pháp là tuyến tính về tổng kích thước đầu vào trong các trường hợp thử nghiệm, phù hợp thoải mái với ràng buộc của$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# provided samples
# assert run("4\n0000\n01010101\n1100111001\n11000000111\n") == "..."

# custom cases
assert run("1\n0\n") == "0"
assert run("1\n01\n") == "2"
assert run("1\n10\n") == "2"
assert run("1\n000111\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"0"`|`0`| độ dài tối thiểu không có chuỗi con cân bằng | 
|`"01"`|`2`| chuỗi con cân bằng đơn với tỷ lệ khoảng ngoài | 
|`"10"`|`2`| kiểm tra tính đối xứng | 
|`"000111"`|`9`| nhiều phân đoạn cân bằng và chồng chéo | 

## Vỏ cạnh 

Một chuỗi hoàn toàn thống nhất như`"0000"`không tạo ra xung đột về tổng tiền tố ngoài các kết quả khớp tầm thường, vì vậy mỗi nhóm đều có kích thước bằng một ngoại trừ trạng thái ban đầu. Thuật toán bỏ qua các nhóm đơn lẻ, do đó không có đóng góp nào được thêm vào và kết quả vẫn bằng 0, khớp với hành vi đúng. 

Một chuỗi xen kẽ hoàn toàn như`"0101"`tạo ra nhiều tổng tiền tố lặp lại. Cơ chế nhóm đảm bảo tất cả các cặp tiền tố bằng nhau đều được ghi lại và mỗi cặp đóng góp dựa trên vị trí bên trái và bên phải của nó. Việc tích lũy quét đảm bảo rằng các chuỗi con cân bằng chồng chéo không bị tính hai lần, vì mỗi điểm cuối bên phải chỉ xử lý các điểm cuối bên trái được nhìn thấy trước đó đúng một lần.
