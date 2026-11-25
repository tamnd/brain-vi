---
title: "CF 978D - Tiến trình gần như số học"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép "tinh chỉnh" từng phần tử một cách độc lập bằng cách chọn giảm nó đi một, tăng nó lên một hoặc giữ nguyên."
date: "2026-06-17T01:23:37+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 978
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 481 (Div. 3)"
rating: 1500
weight: 978
solve_time_s: 79
verified: true
draft: false
---

[CF 978D - Tiến trình gần như số học](https://codeforces.com/problemset/problem/978/D) 

**Đánh giá:** 1500 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và chúng ta được phép "tinh chỉnh" từng phần tử một cách độc lập bằng cách chọn giảm nó đi một, tăng nó lên một hoặc giữ nguyên. Sau khi áp dụng những điều chỉnh nhỏ này, chúng tôi muốn chuỗi kết quả tạo thành một cấp số cộng, nghĩa là sự khác biệt giữa các phần tử liên tiếp sẽ không đổi trên toàn bộ mảng. 

Mục tiêu không chỉ là quyết định xem điều này có khả thi hay không mà còn là giảm thiểu số lượng vị trí mà chúng tôi thực sự sửa đổi. Một vị trí được tính là đã sửa đổi nếu chúng ta áp dụng +1 hoặc −1 cho nó. Để nó không thay đổi không tốn kém gì. 

Khó khăn chính xuất phát từ thực tế là mỗi phần tử có thể có ba giá trị cuối cùng, do đó không gian của tất cả các chuỗi được sửa đổi đều theo cấp số nhân. Với tối đa 100.000 phần tử, việc ép buộc tất cả các kết hợp điều chỉnh một cách thô bạo là không thể. 

Một cách hữu ích để suy nghĩ về các ràng buộc là mọi giải pháp hợp lệ đều được xác định đầy đủ bởi hai tham số: phần tử đầu tiên sau khi sửa đổi và hiệu chung. Khi những điều đó đã được sửa, giá trị mục tiêu của mọi phần tử sẽ bị ép buộc. Quyền tự do duy nhất còn lại là chọn xem mỗi vị trí có khớp với giá trị ban đầu của nó hay phải được dịch chuyển chính xác một vị trí để khớp với mục tiêu. 

Một trường hợp khó phát hiện khi không có cấp số cộng hợp lệ nào có thể được hình thành ngay cả sau khi sửa đổi. Ví dụ: nếu sự khác biệt bắt buộc buộc một số phần tử phải cách xa giá trị ban đầu của nó nhiều hơn một đơn vị, thì tiến trình ứng viên đó không hợp lệ. Một trường hợp cạnh khác là các chuỗi có độ dài 1 hoặc 2, trong đó bất kỳ giá trị nào luôn tạo thành một cấp số cộng, do đó câu trả lời luôn bằng 0. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi sự kết hợp điều chỉnh có thể có, có nghĩa là mọi phần tử đều ở một trong ba trạng thái. Điều đó dẫn đến 3^n khả năng, điều này hoàn toàn không khả thi với n lên tới 100.000. 

Một lực lượng vũ phu có cấu trúc hơn là để quan sát rằng một cấp số cộng được xác định bởi hai phần tử đầu tiên của nó. Nếu chúng ta đoán các giá trị đã điều chỉnh của hai vị trí đầu tiên thì phần còn lại của chuỗi sẽ bị ép buộc. Vì mỗi vị trí trong số hai vị trí đầu tiên có ba lựa chọn nên chúng ta chỉ có 9 cặp ứng cử viên xuất phát. Đối với mỗi cặp, chúng tôi truyền bá sai số chung cần thiết và kiểm tra xem mọi phần tử có thể khớp trong phạm vi ±1 giá trị ban đầu của nó hay không. 

Điều này làm giảm vấn đề xuống số lần quét tuyến tính không đổi. Thông tin chi tiết quan trọng là sau khi thuật ngữ đầu tiên và sự khác biệt được cố định, mọi thuật ngữ khác sẽ được xác định duy nhất, vì vậy chúng tôi không còn khám phá các kết hợp mà chỉ xác thực tính nhất quán. 

Trong quá trình xác thực, mỗi vị trí đóng góp 0 chi phí (nếu đã chính xác) hoặc 1 chi phí (nếu chúng tôi phải điều chỉnh theo ±1). Nếu bất kỳ vị trí nào sai lệch hơn 1 so với mục tiêu của nó thì tiến trình ứng cử viên đó không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) | O(1) | Quá chậm | 
| Hãy thử hai vị trí đầu tiên (9 trường hợp) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta dựa vào thực tế là một cấp số cộng được xác định duy nhất bởi hai giá trị đầu tiên của nó. 

### bước 

1. Trích xuất các giá trị ứng viên cho phần tử đầu tiên:$b_0 - 1, b_0, b_0 + 1$. Làm tương tự cho phần tử thứ hai. Chúng xác định tối đa 9 cấu hình bắt đầu. 

Mỗi cấu hình đại diện cho một lựa chọn có thể có của các giá trị được điều chỉnh cho vị trí 0 và 1. 
2. Đối với mỗi cặp ứng viên$(x_0, x_1)$, tính hiệu chung$d = x_1 - x_0$. 

Bước này là cần thiết vì khi hai số hạng đầu tiên được cố định, tất cả các số hạng sau đó phải tuân theo chính xác sự khác biệt này. 
3. Khởi tạo bộ đếm cho những sửa đổi cần thiết. Nếu như$x_0 \neq b_0$, thêm một. Nếu như$x_1 \neq b_1$, thêm một. 

Điều này giải thích cho việc liệu chúng tôi có sử dụng điều chỉnh ±1 ở hai vị trí đầu tiên hay không. 
4. Lặp lại phần còn lại của mảng. Đối với mỗi chỉ số$i \ge 2$, tính giá trị cần tìm:$$target_i = x_0 + i \cdot d$$5. Kiểm tra tính khả thi cho từng vị trí: 

Nếu$|b_i - target_i| > 1$, ứng cử viên này là không thể và phải bị loại bỏ ngay lập tức. 

Ngược lại, nếu$b_i \neq target_i$, tăng bộ đếm sửa đổi. 
6. Theo dõi chi phí tối thiểu trên tất cả các cặp ứng viên hợp lệ. 
7. Nếu không có cặp ứng cử viên nào hợp lệ, trả về -1. Nếu không thì trả lại chi phí tối thiểu. 

### Tại sao nó hoạt động 

Mỗi dãy cuối cùng hợp lệ phải là một cấp số cộng, do đó nó phải có một số hạng đầu tiên và sai phân chung. Vì mỗi phần tử có thể thay đổi nhiều nhất một phần tử nên hai phần tử đúng đầu tiên phải nằm trong tập hợp$\{b_0-1, b_0, b_0+1\}$Và$\{b_1-1, b_1, b_1+1\}$. Do đó, mọi giải pháp hợp lệ đều được bao phủ bởi một trong 9 cặp khởi đầu ứng cử viên. 

Khi một cặp được cố định, phần còn lại của chuỗi không còn tự do. Bất kỳ sai lệch nào so với mảng ban đầu đều đóng góp chính xác một chi phí và bất kỳ sai lệch nào lớn hơn một sẽ khiến ứng cử viên không thể thực hiện được. Điều này đảm bảo chúng tôi không bỏ lỡ các giải pháp hợp lệ cũng như không chấp nhận những giải pháp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = list(map(int, input().split()))

    if n <= 2:
        print(0)
        return

    INF = 10**18
    ans = INF

    for a0_delta in (-1, 0, 1):
        for a1_delta in (-1, 0, 1):
            x0 = b[0] + a0_delta
            x1 = b[1] + a1_delta

            d = x1 - x0
            cost = 0

            if x0 != b[0]:
                cost += 1
            if x1 != b[1]:
                cost += 1

            ok = True

            for i in range(2, n):
                expected = x0 + i * d
                diff = b[i] - expected

                if abs(diff) > 1:
                    ok = False
                    break

                if diff != 0:
                    cost += 1

            if ok:
                ans = min(ans, cost)

    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Việc triển khai liệt kê rõ ràng tất cả các nhiễu loạn ban đầu hợp lệ cho hai phần tử đầu tiên. Phần còn lại của trình tự được xác thực trong một lần duyệt cho mỗi ứng viên. Việc tính toán chi phí được liên kết trực tiếp với việc mỗi phần tử đã bằng mục tiêu hay cần điều chỉnh ±1. 

Một cạm bẫy phổ biến là chúng ta quên rằng chúng ta phải kiểm tra tính khả thi bằng cách sử dụng sai số tuyệt đối. Một cách khác là tính toán lại sai lệch tăng dần mà không sửa hai giá trị đầu tiên, điều này có thể dẫn đến việc truyền bá không nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
24 21 14 10
```Chúng tôi kiểm tra một cặp ứng cử viên bắt đầu. Giả sử chúng ta chọn (24, 21), cho d = -3. 

| tôi | b[i] | dự kiến ​​| khác biệt | hợp lệ | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 24 | 24 | 0 | vâng | 0 | 
| 1 | 21 | 21 | 0 | vâng | 0 | 
| 2 | 14 | 18 | -4 | không | - | 

Ứng cử viên này thất bại. Thử các kết hợp khác, một cấp số hợp lệ là [25, 20, 15, 10], tương ứng với (25, 20) với d = -5. 

| tôi | b[i] | dự kiến ​​| khác biệt | hợp lệ | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 24 | 25 | -1 | vâng | 1 | 
| 1 | 21 | 20 | 1 | vâng | 2 | 
| 2 | 14 | 15 | -1 | vâng | 3 | 
| 3 | 10 | 10 | 0 | vâng | 3 | 

Chi phí tốt nhất có thể đạt được là 3. 

### Ví dụ 2 

đầu vào:```
5
1 3 5 7 9
```Đây đã là một cấp số cộng có chênh lệch 2. 

Chọn (1, 3) được d = 2. 

| tôi | b[i] | dự kiến ​​| khác biệt | hợp lệ | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | vâng | 0 | 
| 1 | 3 | 3 | 0 | vâng | 0 | 
| 2 | 5 | 5 | 0 | vâng | 0 | 
| 3 | 7 | 7 | 0 | vâng | 0 | 
| 4 | 9 | 9 | 0 | vâng | 0 | 

Câu trả lời là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(9n) | Chúng tôi thử 9 cặp bắt đầu và xác thực từng cặp trong một lần duyệt qua mảng | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Quét tuyến tính trên tối đa chín ứng cử viên phù hợp thoải mái trong các ràng buộc cho n lên tới 100.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n = int(input())
    b = list(map(int, input().split()))

    if n <= 2:
        return "0"

    INF = 10**18
    ans = INF

    for a0 in (-1,0,1):
        for a1 in (-1,0,1):
            x0 = b[0] + a0
            x1 = b[1] + a1
            d = x1 - x0

            cost = (a0 != 0) + (a1 != 0)
            ok = True

            for i in range(2, n):
                expected = x0 + i*d
                diff = b[i] - expected
                if abs(diff) > 1:
                    ok = False
                    break
                if diff != 0:
                    cost += 1

            if ok:
                ans = min(ans, cost)

    return "-1" if ans == INF else str(ans)

# provided sample
assert run("4\n24 21 14 10\n") == "3"

# all already AP
assert run("5\n1 3 5 7 9\n") == "0"

# minimum size
assert run("1\n100\n") == "0"

# impossible case
assert run("3\n1 100 1\n") == "-1"

# near-valid with small fixes
assert run("4\n2 4 6 9\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | AP tầm thường | 
| đã AP | 0 | trường hợp chi phí bằng không | 
| không thể | -1 | tuyên truyền không khả thi | 
| gần hợp lệ | 1 | chỉnh sửa một lần là đủ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi giải pháp tối ưu không sử dụng giá trị thứ nhất hoặc thứ hai ban đầu. Ví dụ: nếu dịch chuyển cả hai ±1 tạo ra một cấp số hợp lệ nhưng việc giữ chúng cố định thì không, thuật toán vẫn phải khám phá các ứng cử viên đó. Việc liệt kê tất cả 9 trạng thái bắt đầu đảm bảo điều này được bao gồm, vì (b0±1, b1±1) luôn được bao gồm. 

Một trường hợp cạnh khác xảy ra khi các vị trí đầu có vẻ hợp lệ nhưng các vị trí sau vượt quá dung sai ±1. Trong những trường hợp như vậy, thuật toán phải từ chối toàn bộ ứng viên ngay lập tức thay vì cố gắng “sửa” những điểm không khớp sau này một cách tham lam. Việc kiểm tra sự khác biệt tuyệt đối thực thi điều này một cách nghiêm ngặt, ngăn chặn việc chấp nhận một phần không chính xác. 

Cuối cùng, các chuỗi có độ dài 1 hoặc 2 bỏ qua tất cả logic, vì hai số bất kỳ luôn tạo thành một cấp số cộng.
