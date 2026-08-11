---
title: "CF 102409J ​​- Phân chia tốt nhất"
description: "Có (N-1) vị trí cắt hợp lệ bên trong một thanh sôcôla có chiều dài (L). Cộng hai điểm cuối (0) và (L), chúng ta có (N+1) vị trí mô tả (N) phần cơ bản. Chúng ta phải chọn chính xác ba vị trí bên trong để tạo ra bốn mảnh liền kề nhau."
date: "2026-08-12T00:04:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "J"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 132
verified: true
draft: false
---

[CF 102409J - Phân chia tốt nhất](https://codeforces.com/problemset/problem/102409/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (N-1) vị trí cắt hợp lệ bên trong một thanh sôcôla có chiều dài (L). Cộng hai điểm cuối (0) và (L), chúng ta có (N+1) vị trí mô tả (N) phần cơ bản. Chúng ta phải chọn chính xác ba vị trí bên trong để tạo ra bốn mảnh liền kề nhau. 

Nếu các vết cắt được chọn ở vị trí (a<b<c), chiều dài của bốn mảnh là 

[ 
a,\qquad b-a,\qquad c-b,\qquad L-c. 
] 

Chất lượng của bộ phận này là sự khác biệt giữa phần lớn nhất và phần nhỏ nhất của nó. Chúng ta cần giá trị tối thiểu có thể có của chênh lệch đó đối với mọi lựa chọn hợp pháp của ba lần cắt. 

Kích thước đầu vào là dấu hiệu cảnh báo đầu tiên. Với (N) lớn bằng (10^5), việc thử mỗi ba lần cắt yêu cầu 

\frac{(N-1)(N-2)(N-3)}{6} 
] 

khả năng xảy ra, tức là khoảng (1,67\times10^{14}) khi (N=10^5). Giới hạn một giây loại bỏ hoàn toàn điều đó. Tọa độ có thể đạt tới (10^{15}), do đó việc triển khai cũng phải sử dụng số học số nguyên 64 bit. Số nguyên Python đã xử lý phạm vi này một cách an toàn. 

Có một số trường hợp ranh giới có thể khiến việc triển khai hợp lý trở nên sai lầm. Đầu tiên, khi (N=4), có đúng ba phép cắt hợp pháp, do đó chỉ có thể phân chia được một. Ví dụ,```
4 10
2 5 7
```tạo ra các mảnh (2,3,2,3), vì vậy câu trả lời là (1). Việc triển khai giả định có một số lựa chọn ở giữa có thể vô tình bỏ qua trường hợp này. 

Điểm cuối (0) và (L) không phải là điểm cắt. Vì```
4 10
1 5 9
```các phần hợp lệ duy nhất là (1,4,4,1), đưa ra câu trả lời (3). Việc xử lý điểm cuối như thể nó là một phần cắt có sẵn sẽ làm thay đổi vấn đề và có thể tạo ra phép chia không hợp lệ. 

Một trường hợp tinh vi khác xảy ra khi điểm giữa lý tưởng nằm chính xác giữa hai đường cắt có sẵn. Coi như```
5 10
2 4 6 8
```Để có đường cắt ở giữa phù hợp, nửa bên trái có tọa độ đích (3) và cả (2) và (4) đều gần nhau như nhau. Cả hai lựa chọn đều tạo ra hai mảnh bên trái có độ dài giống nhau theo thứ tự đảo ngược, vì vậy câu trả lời là (2). Việc tìm kiếm chỉ kiểm tra một phía của điểm chèn có thể bỏ lỡ kết quả tối ưu. 

Cuối cùng, tọa độ có thể lớn hơn nhiều so với phạm vi số nguyên máy thông thường. Vì```
4 1000000000000000
1 2 3
```phép chia duy nhất có độ dài (1,1,1,999999999999997), vì vậy câu trả lời là```
999999999999996
```Việc sử dụng dấu phẩy động để xác định điểm giữa là không cần thiết và có khả năng không an toàn ở thang đo này. Giải pháp dưới đây thực hiện tất cả các phép tính điểm giữa với số học số nguyên. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là chọn vết cắt thứ nhất, thứ hai và thứ ba. Nếu vị trí của chúng là (a<b<c), chúng ta sẽ tính bốn độ dài và cập nhật câu trả lời bằng 

[ 
\max(a,b-a,c-b,L-c)-\min(a,b-a,c-b,L-c). 
] 

Điều này đúng vì mỗi phép chia hợp pháp tương ứng với chính xác một bộ ba phần cắt bên trong tăng dần, vì vậy việc kiểm tra tất cả các bộ ba sẽ kiểm tra mọi phép chia có thể có. Vấn đề là số lượng gấp ba. Đối với (N=10^5), có khoảng (1,67\time10^{14}) trong số đó, vượt xa giới hạn thời gian cho phép. 

Cấu trúc hữu ích xuất hiện khi chúng ta tạm thời sửa phần cắt thứ hai hoặc phần giữa ở vị trí (b). Khi (b) được khắc phục, vấn đề sẽ tách thành hai cặp độc lập. 

Hai mảnh bên trái là 

[ 
a,\qquad b-a, 
] 

và tổng của chúng được cố định tại (b). Hai mảnh bên phải là 

[ 
c-b,\qquad L-c, 
] 

và tổng của chúng được cố định tại (L-b). 

Đối với hai số có tổng cố định, việc làm cho chúng gần nhau hơn không bao giờ có thể làm cho tổng cực đại-trừ-nhỏ nhất trở nên tồi tệ hơn. Giả sử cặp hiện tại là (x,S-x). Di chuyển (x) về phía (S/2) đồng thời di chuyển bộ phận lớn hơn đi xuống và bộ phận nhỏ hơn đi lên. Bất kỳ quân nào khác ngoài cặp này đều không bị ảnh hưởng nên phạm vi toàn cầu không thể tăng lên. 

Điều này đưa ra quan sát trọng tâm: đối với một đường cắt ở giữa cố định (b), đường cắt đầu tiên tốt nhất là vị trí cắt có sẵn gần nhất với (b/2). Đường cắt thứ ba tốt nhất là vị trí cắt có sẵn gần nhất với 

[ 
\frac{b+L}{2}. 
] 

Mục tiêu đầu tiên là điểm tại đó (a=b-a) và mục tiêu thứ hai là điểm tại đó (c-b=L-c). Vì các vị trí cắt được sắp xếp nên mỗi vị trí gần nhất có thể được tìm thấy bằng tìm kiếm nhị phân. 

Lực lượng vũ phu hoạt động vì nó kiểm tra rõ ràng từng bộ ba, nhưng không thành công vì có quá nhiều bộ ba. Quan sát cho thấy đường cắt ở giữa cố định cho phép mỗi bên được tối ưu hóa độc lập sẽ giảm vấn đề xuống còn đường cắt ở giữa (O(N)), với hai lần tìm kiếm nhị phân (O(\log N)) cho mỗi đường cắt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^3)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\log N)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ các vị trí cắt cùng với hai điểm cuối. Xác định 

[ 
p_0=0,\quad p_1=x_1,\ldots,p_{N-1}=x_{N-1},\quad p_N=L. 
] 

Mảng có các vị trí (N+1), trong khi chỉ các chỉ số (1) đến (N-1) là các phần cắt hợp pháp. 
2. Lặp lại mọi đường cắt giữa có thể có (j), vì vậy (2\le j\le N-2). Đường cắt ở giữa cần ít nhất một đường cắt hợp pháp trước nó và một đường cắt hợp pháp sau nó. 

Đặt tọa độ của nó là (p_j). Hai mảnh bên trái có tổng chiều dài (p_j), do đó sự phân chia lý tưởng của chúng xảy ra tại 

[ 
\frac{p_j}{2}. 
] 
3. Tìm đường cắt hợp lệ (i<j) có tọa độ gần nhất với (p_j/2). Vì các vị trí được sắp xếp nên tìm kiếm nhị phân sẽ đưa ra điểm chèn ngay xung quanh mục tiêu này. Chúng ta chỉ cần kiểm tra vị trí tại điểm chèn và điểm tiền thân của nó. 

Các phần còn lại thu được là (p_i) và (p_j-p_i). Việc chọn vị trí gần điểm giữa nhất sẽ làm cho hai mảnh này cân bằng trong mức độ cắt sẵn có cho phép. 
4. Đối với bên phải, hai mảnh có tổng chiều dài (L-p_j). Nếu lần cắt thứ ba là (p_k), chúng là 

[ 
p_k-p_j,\qquad L-p_k. 
] 

Chúng bằng nhau khi 

[ 
p_k=\frac{p_j+L}{2}. 
] 

Tìm kiếm nhị phân cho vị trí hợp pháp (k>j) gần tọa độ này nhất, kiểm tra lại điểm chèn và điểm trước nó. 
5. Tính toán bốn độ dài được tạo ra bởi hai lựa chọn cân bằng cục bộ này và cập nhật phạm vi tối thiểu tổng thể. 
6. Sau khi tất cả các phần cắt ở giữa có thể đã được xử lý, hãy xuất ra phạm vi nhỏ nhất được tìm thấy. 

Tìm kiếm điểm giữa sử dụng mục tiêu số nguyên thay vì dấu phẩy động. Đối với điểm giữa (S/2), việc sử dụng ((S+1)//2) sẽ cho điểm giữa là số nguyên phía trên. Kiểm tra vị trí đó và vị trí liền trước của nó bao trùm cả hai vế khi (S) là số lẻ, trong khi đối với (S) thì điểm giữa chính xác được tìm thấy trực tiếp. 

### Tại sao nó hoạt động

Sửa bất kỳ vết cắt nào ở giữa (b). Hãy xem xét bất kỳ lần cắt đầu tiên hợp lệ nào (a). Cặp bên trái có tổng cố định (b), do đó việc thay thế (a) bằng một vị trí sẵn có gần hơn (b/2) làm cho hai độ dài bên trái ít dàn trải hơn. Thành viên nhỏ hơn của nó chỉ có thể tăng lên và thành viên lớn hơn của nó chỉ có thể giảm đi. Đối số tương tự áp dụng độc lập cho cặp bên phải, có tổng là (L-b). Do đó, đối với đường cắt ở giữa cố định này, cặp đường cắt ở điểm giữa gần nhất tạo ra một phép chia có phạm vi tổng thể không lớn hơn bất kỳ phép chia nào khác sử dụng cùng một đường cắt ở giữa. Vì thuật toán kiểm tra mọi đường cắt giữa có thể có, nên ít nhất một lần lặp xem xét đường cắt giữa của một phép chia tối ưu và phép lặp đó sẽ tái tạo lại một phép chia không tệ hơn mức tối ưu. Do đó, nó phải tạo ra mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())
    cuts = list(map(int, input().split()))

    # p[0] = 0, p[1..n-1] are legal cuts, p[n] = L.
    p = [0] + cuts + [L]

    ans = L

    # j is the middle cut. There must be one legal cut
    # on each side of it.
    for j in range(2, n - 1):
        pj = p[j]

        # Left pair: p[i], pj - p[i].
        # Its balance is best when p[i] is closest to pj / 2.
        left_target = (pj + 1) // 2
        i = bisect_left(p, left_target, 1, j)

        left_candidates = []
        if i < j:
            left_candidates.append(i)
        if i - 1 >= 1:
            left_candidates.append(i - 1)

        best_i = min(
            left_candidates,
            key=lambda idx: abs(2 * p[idx] - pj)
        )

        # Right pair: p[k] - pj, L - p[k].
        # Balance is best when p[k] is closest to (pj + L) / 2.
        right_target = (pj + L + 1) // 2
        k = bisect_left(p, right_target, j + 1, n)

        right_candidates = []
        if k < n:
            right_candidates.append(k)
        if k - 1 > j:
            right_candidates.append(k - 1)

        best_k = min(
            right_candidates,
            key=lambda idx: abs(2 * p[idx] - (pj + L))
        )

        a = p[best_i]
        b = pj
        c = p[best_k]

        pieces = (
            a,
            b - a,
            c - b,
            L - c
        )

        current = max(pieces) - min(pieces)
        ans = min(ans, current)

    print(ans)

if __name__ == "__main__":
    solve()
```Mảng vị trí bắt đầu bằng (0) và kết thúc bằng (L). Điều này thuận tiện vì mỗi độ dài đoạn có thể được viết dưới dạng hiệu của hai phần tử. Ba vết cắt được chọn vẫn chỉ đến từ các chỉ số (1) đến (N-1). 

Vòng lặp sử dụng`range(2, n - 1)`, tương ứng chính xác với những vết cắt ở giữa có ít nhất một vết cắt hợp lệ ở cả hai bên. Đối với (N=4), vòng lặp này chứa chính xác một chỉ mục, (j=2), đây là đường cắt ở giữa duy nhất có thể. 

Đối với tìm kiếm bên trái,`bisect_left`bị giới hạn ở các chỉ số`[1, j)`. Điều đó ngăn việc tìm kiếm nhị phân chọn chính đường cắt ở giữa hoặc điểm cuối bên trái. Việc tìm kiếm phù hợp cũng bị hạn chế tương tự đối với`[j+1, n)`, ngăn không cho đường cắt ở giữa và điểm cuối (L) được chọn. 

biểu thức`abs(2 * p[idx] - pj)`so sánh khoảng cách của ứng viên với (p_j/2) mà không sử dụng dấu phẩy động. Bên phải sử dụng`abs(2 * p[idx] - (pj + L))`vì lý do tương tự. Tất cả các phép tính vẫn chính xác ngay cả khi tọa độ ở khoảng (10^{15}). 

Chỉ có hai chỉ số ứng cử viên xung quanh mỗi điểm chèn tìm kiếm nhị phân có thể là tối ưu. Vì các vị trí được sắp xếp nên mọi vị trí trước đó đều xa mục tiêu hơn vị trí trước đó và mọi vị trí sau đều xa hơn điểm chèn. 

## Ví dụ đã hoạt động 

Chỉ có một mẫu chính thức được cung cấp nên dấu vết thứ hai sử dụng đầu vào được xây dựng nhỏ. 

Đối với Mẫu 1, mảng vị trí hoàn chỉnh là 

[ 
[0,3,4,5,8,10,12,15,18]. 
] 

Đối với mỗi lần cắt ở giữa có thể, thuật toán sẽ cân bằng các cặp trái và phải một cách độc lập. 

| Chỉ số giữa (j) | (p_j) | Lần cắt đầu tiên (p_i) | Lần cắt thứ ba (p_k) | Bốn mảnh | Phạm vi | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 3 | 10 | (3,1,6,8) | 7 | 
| 3 | 5 | 3 | 12 | (3,2,7,6) | 5 | 
| 4 | 8 | 4 | 12 | (4,4,4,6) | 2 | 
| 5 | 10 | 5 | 15 | (5,5,5,3) | 2 | 
| 6 | 12 | 8 | 15 | (8,4,3,3) | 5 | 

Phạm vi tốt nhất là (2), phù hợp với đầu ra mẫu. Hai hàng có phạm vi (2) tương ứng chính xác với hai cách chia tối ưu được mô tả bởi mẫu: (4,4,4,6) và (5,5,5,3). 

Đối với ví dụ thứ hai,```
5 10
2 4 7 9
```mảng vị trí là 

[ 
[0,2,4,7,9,10]. 
] 

Có thể có ba đường cắt ở giữa. 

| Chỉ số giữa (j) | (p_j) | Lần cắt đầu tiên (p_i) | Lần cắt thứ ba (p_k) | Bốn mảnh | Phạm vi | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 2 | 7 | (2,2,3,3) | 1 | 
| 3 | 7 | 4 | 9 | (4,3,2,1) | 3 | 

Thuật toán thực sự chỉ cần hai lần lặp ở giữa vì (j=2,3) là các chỉ số hợp lệ cho (N=5). Câu đầu tiên cho (2,2,3,3), nên câu trả lời là (1). 

Ví dụ này chứng minh tại sao cân bằng mỗi bên là đủ. Với đường cắt ở giữa (4), đường chia bên trái tốt nhất là (2,2) và đường chia bên phải tốt nhất là (3,3). Không có sự thay thế không cân bằng nào có thể cải thiện phạm vi toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log N)) | Có (O(N)) phần cắt ở giữa và mỗi phần sử dụng hai tìm kiếm nhị phân. | 
| Không gian | (O(N)) | Mảng vị trí được sắp xếp chứa (N+1) số nguyên. | 

Đối với (N=10^5), thuật toán chỉ thực hiện (O(N)) lần lặp và hai lần tìm kiếm logarit trên mỗi lần lặp. Điều này nằm trong phạm vi dự kiến ​​của vấn đề, trong khi phương pháp bạo lực sẽ yêu cầu tối đa khoảng (1,67\times10^{14}) gấp ba lần (N). Các giá trị vị trí tối đa là khoảng (10^{15}), được Python xử lý chính xác mà không bị tràn. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_left

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n, L = map(int, sys.stdin.readline().split())
        cuts = list(map(int, sys.stdin.readline().split()))

        p = [0] + cuts + [L]
        ans = L

        for j in range(2, n - 1):
            pj = p[j]

            left_target = (pj + 1) // 2
            i = bisect_left(p, left_target, 1, j)

            left_candidates = []
            if i < j:
                left_candidates.append(i)
            if i - 1 >= 1:
                left_candidates.append(i - 1)

            best_i = min(
                left_candidates,
                key=lambda idx: abs(2 * p[idx] - pj)
            )

            right_target = (pj + L + 1) // 2
            k = bisect_left(p, right_target, j + 1, n)

            right_candidates = []
            if k < n:
                right_candidates.append(k)
            if k - 1 > j:
                right_candidates.append(k - 1)

            best_k = min(
                right_candidates,
                key=lambda idx: abs(2 * p[idx] - (pj + L))
            )

            pieces = (
                p[best_i],
                p[j] - p[best_i],
                p[best_k] - p[j],
                L - p[best_k],
            )

            ans = min(ans, max(pieces) - min(pieces))

        print(ans)
        return sys.stdout.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    "8 18\n"
    "3 4 5 8 10 12 15\n"
) == "2\n", "sample 1"

# Minimum N, only one possible division
assert run(
    "4 10\n"
    "2 5 7\n"
) == "1\n", "minimum-size case"

# Very large coordinates, checks integer arithmetic and boundary handling
assert run(
    "4 1000000000000000\n"
    "1 2 3\n"
) == "999999999999996\n", "large-coordinate boundary case"

# Designed to catch midpoint and insertion-point errors
assert run(
    "5 10\n"
    "2 4 7 9\n"
) == "1\n", "midpoint search case"

# Maximum N, every elementary piece has length 1.
# Cuts are 1, 2, ..., 99999 and L = 100000.
max_n = 100000
max_input = (
    f"{max_n} {max_n}\n"
    + " ".join(map(str, range(1, max_n)))
    + "\n"
)
assert run(max_input) == "0\n", "maximum-N equal-piece case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 10 / 2 5 7`|`1`| Tối thiểu (N), chính xác là một phép chia có thể | 
|`4 1000000000000000 / 1 2 3`|`999999999999996`| Tọa độ lớn và số học số nguyên | 
|`5 10 / 2 4 7 9`|`1`| Lựa chọn điểm giữa tìm kiếm nhị phân và ranh giới chỉ mục | 
|`100000 100000 / 1 2 ... 99999`|`0`| Tối đa (N), phân chia cân bằng, hiệu suất | 

## Vỏ cạnh 

Khi (N=4), ba điểm cắt được cung cấp là những điểm cắt duy nhất có thể thực hiện được. Vì```
4 10
2 5 7
```mảng vị trí là (0,2,5,7,10). Bốn mảnh duy nhất có thể là (2,3,2,3), cho phạm vi (3-2=1). Vòng lặp xử lý (j=2), tìm (i=1) và (k=3) và trả về (1). 

Khi điểm giữa lý tưởng nằm giữa hai vị trí pháp lý thì cả hai ứng cử viên lân cận đều phải được xem xét. TRONG```
5 10
2 4 6 8
```xét đường cắt ở giữa tại (6). Mục tiêu bên trái là (3), và các vị trí hợp pháp (2) và (4) cách đều nhau. Việc chọn một trong hai sẽ tạo ra cùng một cặp (2,4), chỉ theo thứ tự đảo ngược. Ở bên phải, mục tiêu là (8), là một đường cắt có sẵn, cho các quân cờ (2,4,2,2) và phạm vi (2). Các bước kiểm tra điểm chèn và tiền thân của thuật toán sẽ bao phủ chính xác mối ràng buộc. 

Điểm cuối không bao giờ được trở thành điểm cắt được chọn. TRONG```
4 10
1 5 9
```mảng vị trí là (0,1,5,9,10). Phép chia hợp lệ duy nhất là (1,4,4,1), với câu trả lời (3). Tìm kiếm nhị phân bên trái bị giới hạn ở các chỉ số cắt hợp pháp trước phần cắt giữa và tìm kiếm bên phải bị giới hạn ở các chỉ số cắt hợp pháp sau nó, do đó cả (0) và (L) đều không thể nhập bộ ba đã chọn. 

Tọa độ lớn yêu cầu xử lý điểm giữa chính xác. Vì```
4 1000000000000000
1 2 3
```bốn độ dài là (1,1,1,9999999999999997), vì vậy câu trả lời là (999999999999996). Việc triển khai không bao giờ chuyển đổi các giá trị này thành dấu phẩy động. Việc so sánh điểm giữa được thực hiện thông qua các biểu thức như (2p_i-p_j), duy trì độ chính xác cho mọi giá trị đầu vào được phép. 

Trường hợp phần bằng nhau có kích thước tối đa cũng thực hiện các ranh giới chỉ mục. Với (N=100000), (L=100000) và cắt ở mọi số nguyên từ (1) đến (99999), thanh có thể được chia ở (25000,50000,75000). Mỗi mảnh kết quả có độ dài (25000), vì vậy câu trả lời là (0). Thuật toán xử lý tất cả (99998) vị trí cắt giữa có thể có mà không liệt kê bộ ba, chứng minh tại sao cấu trúc (O(N\log N)) lại cần thiết cho đầu vào lớn nhất.
