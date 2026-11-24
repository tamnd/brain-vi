---
title: "CF 86D - Mảng mạnh mẽ"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và nhiều truy vấn khoảng. Đối với mỗi truy vấn [l, r], chúng tôi chỉ xem xét mảng con giữa các chỉ số đó và tính giá trị gọi là lũy thừa của nó. Nếu một số x xuất hiện k lần trong mảng con thì x đóng góp k2 x vào câu trả lời."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "math", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 86
codeforces_index: "D"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 2"
rating: 2200
weight: 86
solve_time_s: 226
verified: true
draft: false
---

[CF 86D - Mảng mạnh mẽ](https://codeforces.com/problemset/problem/86/D) 

**Đánh giá:** 2200 
**Tags:** cấu trúc dữ liệu, cách triển khai, toán học, hai con trỏ 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và nhiều truy vấn khoảng. Đối với mỗi truy vấn`[l, r]`, chúng ta chỉ nhìn vào mảng con giữa các chỉ số đó và tính giá trị gọi là lũy thừa của nó. 

Nếu một số`x`xuất hiện`k`lần bên trong mảng con, sau đó`x`đóng góp`k² * x`để trả lời. Kết quả cuối cùng là tổng của những đóng góp này trên tất cả các giá trị khác biệt trong khoảng. 

Ví dụ, trong mảng con`[1, 2, 1, 1, 2]`, giá trị`1`xuất hiện`3`lần và đóng góp`3² * 1 = 9`. giá trị`2`xuất hiện`2`lần và đóng góp`2² * 2 = 8`. Tổng cộng là`17`. 

Độ dài mảng và số lượng truy vấn đều đạt`200000`. Một giải pháp đơn giản xử lý mọi truy vấn một cách độc lập ngay lập tức là quá chậm. Ngay cả việc lặp qua một mảng con đầy đủ cho mỗi truy vấn cũng sẽ tốn kém`O(n * q)`trong trường hợp xấu nhất, điều đó trở thành khoảng`4 * 10^10`hoạt động. Đó là một số bậc độ lớn vượt quá mức phù hợp trong 5 giây. 

Bản thân các giá trị nhiều nhất là`10^6`, đủ nhỏ cho một mảng tần số được lập chỉ mục theo giá trị. Quan sát đó trở nên rất hữu ích khi chúng tôi xử lý các truy vấn theo thứ tự thông minh hơn. 

Một chi tiết tinh tế là câu trả lời có thể trở nên lớn. Giả sử tất cả`200000`các phần tử bằng`10^6`. Khi đó phần đóng góp là:$$(200000)^2 \cdot 10^6 = 4 \cdot 10^{16}$$Điều này không phù hợp với số nguyên 32 bit, do đó việc triển khai phải sử dụng số học 64 bit. 

Một sai lầm dễ mắc phải khác là cập nhật phần đóng góp không chính xác khi tần số thay đổi. Hãy xem xét mảng:```
[5, 5]
```Đối với khoảng thời gian`[1, 2]`, đáp án đúng là:```
2² * 5 = 20
```Nếu chúng ta đã có một cái`5`trong khoảng thời gian hiện tại, thêm một khoảng thời gian khác`5`không tăng câu trả lời chỉ bằng`5`. Khoản đóng góp thay đổi từ:```
1² * 5 = 5
```ĐẾN```
2² * 5 = 20
```vì vậy mức tăng thực sự là`15`. 

Một lỗi phổ biến cuối cùng xuất hiện ở ranh giới khoảng thời gian. Giả sử khoảng thời gian hiện tại là`[2, 4]`và truy vấn tiếp theo là`[3, 4]`. Nếu chúng ta xóa điểm cuối bên trái sau khi di chuyển con trỏ, chúng ta vô tình giữ một phần tử phụ bên trong cửa sổ. Thứ tự di chuyển con trỏ và cập nhật tần số phải khớp chính xác với khoảng thời gian đã định. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý mọi truy vấn một cách độc lập. Đối với mỗi khoảng thời gian`[l, r]`, chúng tôi đếm tần số bên trong mảng con và tính toán:$$\sum cnt[x]^2 \cdot x$$Bản đồ băm hoặc mảng tần số giúp việc đếm trở nên đơn giản. Phương pháp này đúng vì nó tuân theo định nghĩa về công suất. 

Vấn đề là toàn bộ công việc. Một truy vấn có thể bao trùm toàn bộ mảng, yêu cầu`O(n)`hoạt động. Với`200000`các truy vấn, tổng độ phức tạp sẽ trở thành`O(nq)`, nó quá lớn. 

Quan sát chính là các truy vấn lân cận thường chồng chéo rất nhiều. Nếu hai khoảng chỉ khác nhau một vài phần tử thì việc tính lại toàn bộ câu trả lời sẽ lãng phí gần như toàn bộ công việc trước đó. 

Cấu trúc này chính xác là những gì thuật toán của Mo khai thác. 

Thay vì trả lời các truy vấn theo thứ tự đầu vào, chúng tôi sắp xếp chúng sao cho các truy vấn liên tiếp có phạm vi tương tự nhau. Sau đó, chúng tôi duy trì một khoảng thời gian hiện tại và điều chỉnh nó dần dần. Khi khoảng mở rộng hoặc thu hẹp bởi một phần tử, chúng tôi sẽ cập nhật câu trả lời theo thời gian không đổi. 

Giả sử tần số hiện tại của giá trị`x`là`f`. 

Trước khi thêm cái khác`x`, đóng góp của nó là:$$f^2 \cdot x$$Sau khi thêm nó, đóng góp sẽ trở thành:$$(f+1)^2 \cdot x$$Mức tăng là:$$((f+1)^2 - f^2) \cdot x = (2f+1)\cdot x$$Chúng ta có thể cập nhật câu trả lời ngay lập tức mà không cần tính toán lại bất cứ điều gì khác. 

Việc xóa một phần tử hoạt động tương tự:$$f^2 \cdot x - (f-1)^2 \cdot x = (2f-1)\cdot x$$Vì chi phí di chuyển của mỗi con trỏ`O(1)`, tổng độ phức tạp trở thành xấp xỉ`O((n + q)\sqrt n)`, đủ nhanh cho các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(max(a)) | Quá chậm | 
| Thuật toán tối ưu Mo | O((n + q)√n) | O(max(a) + q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tất cả các truy vấn. 
2. Chuyển đổi truy vấn sang lập chỉ mục dựa trên 0 vì danh sách Python sử dụng các vị trí dựa trên 0. 
3. Thông thường, chọn kích thước khối`int(sqrt(n))`. 
4. Sắp xếp truy vấn theo: 

đầu tiên, khối chứa điểm cuối bên trái, 

thứ hai, điểm cuối bên phải. 

Thứ tự này giảm thiểu khoảng cách di chuyển của con trỏ khoảng giữa các truy vấn liên tiếp. 
5. Duy trì khoảng thời gian hiện tại`[cur_l, cur_r]`và giá trị công suất hiện tại của nó. 

Ban đầu khoảng trống. 
6. Duy trì mảng tần số`freq[x]`lưu trữ giá trị bao nhiêu lần`x`xuất hiện bên trong khoảng thời gian hiện tại. 
7. Xác định một`add(x)`hoạt động. 

Nếu tần số hiện tại là`f`, sau đó:$$answer -= f^2 \cdot x$$tăng tần số,$$answer += (f+1)^2 \cdot x$$Điều này cập nhật sự đóng góp của chỉ giá trị bị ảnh hưởng. 
8. Xác định một`remove(x)`hoạt động tương tự. 

Nếu tần số hiện tại là`f`, xóa phần đóng góp cũ của nó, giảm tần suất, sau đó thêm phần đóng góp mới. 
9. Xử lý các truy vấn theo thứ tự được sắp xếp. 

Di chuyển`cur_l`Và`cur_r`cho đến khi khoảng thời gian duy trì khớp chính xác với khoảng thời gian truy vấn mục tiêu. 
10. Sau khi khoảng thời gian khớp, hãy lưu câu trả lời hiện tại vào mảng kết quả bằng cách sử dụng chỉ mục gốc của truy vấn. 
11. Sau khi tất cả các truy vấn được xử lý, hãy in câu trả lời theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, khoảng thời gian được duy trì khớp chính xác với phạm vi truy vấn hiện tại. Mảng tần số luôn lưu trữ số lần xuất hiện thực sự của mọi giá trị trong khoảng đó. 

Biến trả lời bằng:$$\sum freq[x]^2 \cdot x$$bởi vì mọi thao tác thêm hoặc xóa sẽ cập nhật phần đóng góp của chính xác một giá trị trong khi giữ nguyên tất cả các giá trị khác. 

Vì thứ tự của Mo đảm bảo rằng mọi truy vấn đều đạt được thông qua một chuỗi mở rộng và thu gọn khoảng thời gian hợp lệ, trạng thái được duy trì luôn chính xác khi truy vấn được trả lời. 

## Giải pháp Python```python
import sys
from math import sqrt

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    queries = []

    for idx in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        queries.append((l, r, idx))

    block_size = int(sqrt(n)) + 1

    queries.sort(
        key=lambda x: (
            x[0] // block_size,
            x[1]
        )
    )

    freq = [0] * (10**6 + 1)
    answers = [0] * q

    current_answer = 0
    cur_l = 0
    cur_r = -1

    def add(value):
        nonlocal current_answer

        f = freq[value]
        current_answer -= f * f * value

        freq[value] += 1
        f += 1

        current_answer += f * f * value

    def remove(value):
        nonlocal current_answer

        f = freq[value]
        current_answer -= f * f * value

        freq[value] -= 1
        f -= 1

        current_answer += f * f * value

    for l, r, idx in queries:

        while cur_l > l:
            cur_l -= 1
            add(arr[cur_l])

        while cur_r < r:
            cur_r += 1
            add(arr[cur_r])

        while cur_l < l:
            remove(arr[cur_l])
            cur_l += 1

        while cur_r > r:
            remove(arr[cur_r])
            cur_r -= 1

        answers[idx] = current_answer

    print("\n".join(map(str, answers)))

solve()
```Giải pháp bắt đầu bằng cách đọc tất cả các truy vấn và đính kèm các chỉ mục ban đầu của chúng. Sau khi sắp xếp các truy vấn cho thuật toán của Mo, chúng tôi không còn xử lý chúng theo thứ tự đầu vào nữa, vì vậy chúng tôi phải nhớ từng câu trả lời thuộc về đâu. 

Khoảng thời gian hiện tại bắt đầu trống vì`[0, -1]`. Cách biểu diễn này tránh các trường hợp đặc biệt vì việc thêm phần tử đầu tiên chỉ đơn giản là tăng`cur_r`. 

Mảng tần số có kích thước`10^6 + 1`bởi vì các giá trị mảng được giới hạn bởi`10^6`. Sử dụng danh sách nhanh hơn nhiều so với từ điển cho vấn đề này. 

các`add`Và`remove`chức năng là cốt lõi của việc thực hiện. Một lỗi phổ biến là cập nhật tần suất trước khi trừ phần đóng góp cũ. Mã đầu tiên loại bỏ thuật ngữ cũ`f² * value`, thay đổi tần số, sau đó thêm thuật ngữ được cập nhật. 

Thứ tự di chuyển của con trỏ cũng có vấn đề. Khi thu nhỏ từ bên trái, ta loại bỏ phần tử bên trái hiện tại trước khi tăng`cur_l`. Đảo ngược thứ tự đó sẽ loại bỏ giá trị sai. 

Tất cả số học đều sử dụng số nguyên Python, tự động xử lý các giá trị lớn một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2 1
1 2
1 3
```Sau khi chuyển đổi dựa trên số 0, các truy vấn sẽ`[0,1]`Và`[0,2]`. 

| Bước | Khoảng thời gian | Hành động | Tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [] | trống | {} | 0 | 
| Thêm 1 | [0,0] | thêm 1 | {1:1} | 1 | 
| Thêm 2 | [0,1] | thêm 2 | {1:1,2:1} | 3 | 
| Trả lời câu hỏi 1 | [0,1] | cửa hàng | {1:1,2:1} | 3 | 
| Thêm 1 | [0,2] | thêm 1 | {1:2,2:1} | 6 | 
| Trả lời câu hỏi 2 | [0,2] | cửa hàng | {1:2,2:1} | 6 | 

Dấu vết cho thấy câu trả lời tiến triển dần dần như thế nào. Khi thứ hai`1`được thêm vào, sự đóng góp của`1`thay đổi từ`1`ĐẾN`4`, tăng tổng số câu trả lời lên`3`. 

### Ví dụ tùy chỉnh 

đầu vào:```
5 2
1 1 2 2 2
1 5
2 4
```| Bước | Khoảng thời gian | Hành động | Tần số | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | [] | trống | {} | 0 | 
| Thêm 1 | [0,0] | thêm 1 | {1:1} | 1 | 
| Thêm 1 | [0,1] | thêm 1 | {1:2} | 4 | 
| Thêm 2 | [0,2] | thêm 2 | {1:2,2:1} | 6 | 
| Thêm 2 | [0,3] | thêm 2 | {1:2,2:2} | 12 | 
| Thêm 2 | [0,4] | thêm 2 | {1:2,2:3} | 22 | 
| Trả lời câu hỏi 1 | [0,4] | cửa hàng | {1:2,2:3} | 22 | 
| Xóa 1 | [1,4] | bỏ trái | {1:1,2:3} | 19 | 
| Xóa 2 | [1,3] | xóa ngay | {1:1,2:2} | 9 | 
| Trả lời câu hỏi 2 | [1,3] | cửa hàng | {1:1,2:2} | 9 | 

Dấu vết này thể hiện cả sự giãn nở và co lại của khoảng thời gian. Chỉ có giá trị bị ảnh hưởng mới thay đổi mức đóng góp mỗi lần, đó là lý do tại sao các bản cập nhật luôn được cập nhật liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q)√n) | Mỗi con trỏ di chuyển tối đa tổng số vị trí O(q√n) | 
| Không gian | O(max(a) + q) | Mảng tần số cộng với lưu trữ truy vấn | 

Với`n`Và`q`cả hai lên đến`200000`, độ phức tạp nằm trong giới hạn đối với giải pháp thuật toán Mo được triển khai cẩn thận trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import sqrt

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))

    queries = []

    for idx in range(q):
        l, r = map(int, input().split())
        queries.append((l - 1, r - 1, idx))

    block = int(sqrt(n)) + 1

    queries.sort(key=lambda x: (x[0] // block, x[1]))

    freq = [0] * (10**6 + 1)
    ans = [0] * q

    cur = 0
    L = 0
    R = -1

    def add(x):
        nonlocal cur
        f = freq[x]
        cur -= f * f * x
        freq[x] += 1
        f += 1
        cur += f * f * x

    def remove(x):
        nonlocal cur
        f = freq[x]
        cur -= f * f * x
        freq[x] -= 1
        f -= 1
        cur += f * f * x

    for l, r, idx in queries:

        while L > l:
            L -= 1
            add(arr[L])

        while R < r:
            R += 1
            add(arr[R])

        while L < l:
            remove(arr[L])
            L += 1

        while R > r:
            remove(arr[R])
            R -= 1

        ans[idx] = cur

    return "\n".join(map(str, ans))

# provided sample
assert run(
"""3 2
1 2 1
1 2
1 3
"""
) == "3\n6"

# minimum size
assert run(
"""1 1
7
1 1
"""
) == "7"

# all equal values
assert run(
"""5 2
4 4 4 4 4
1 5
2 4
"""
) == "100\n36"

# boundary intervals
assert run(
"""5 3
1 2 3 4 5
1 1
5 5
1 5
"""
) == "1\n5\n15"

# off-by-one stress
assert run(
"""4 3
1 2 1 2
1 2
2 3
3 4
"""
) == "3\n3\n3"

print("All tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | Cùng giá trị | Xử lý đúng các khoảng thời gian tối thiểu | 
| Tất cả các giá trị bằng nhau | Đóng góp bậc hai lớn | Cập nhật tần suất là chính xác | 
| Truy vấn chạm ranh giới | Tính chính xác của tiền tố và hậu tố | Chuyển động của con trỏ ở các cạnh | 
| Các giá trị lặp lại xen kẽ | Câu trả lời giống nhau qua các ca | Độ chính xác từng cái một | 

## Vỏ cạnh 

Hãy xem xét khoảng thời gian nhỏ nhất có thể:```
1 1
9
1 1
```Khoảng chứa một lần xuất hiện của`9`, vậy đáp án là:```
1² * 9 = 9
```Thuật toán bắt đầu với một khoảng trống và thêm chính xác một phần tử. Tần số trở thành`1`và câu trả lời được duy trì sẽ trở thành`9`. 

Bây giờ hãy xem xét tất cả các giá trị bằng nhau:```
5 1
3 3 3 3 3
1 5
```Câu trả lời đúng là:```
5² * 3 = 75
```Khi các yếu tố được thêm vào, sự đóng góp sẽ tăng lên:```
1²*3 = 3
2²*3 = 12
3²*3 = 27
4²*3 = 48
5²*3 = 75
```Công thức cập nhật nắm bắt chính xác sự tăng trưởng phi tuyến tính. 

Cuối cùng, hãy xem xét việc thu hẹp một khoảng:```
4 2
1 2 1 2
1 4
2 3
```Khoảng đầu tiên có tần số`{1:2, 2:2}`và trả lời:```
4 + 8 = 12
```Khi chuyển đến`[2,3]`, thuật toán loại bỏ bên trái`1`và đúng`2`. 

Sau khi loại bỏ bên trái`1`:```
{1:1, 2:2}
1 + 8 = 9
```Sau khi loại bỏ quyền`2`:```
{1:1, 2:1}
1 + 2 = 3
```Câu trả lời cuối cùng là đúng vì việc xóa luôn cập nhật các đóng góp trước khi tần suất được thay đổi thêm.
