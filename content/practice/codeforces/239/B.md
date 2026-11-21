---
title: "CF 239B - Lập trình băng dễ dàng"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ số, < và ký tự. Mỗi truy vấn chọn một chuỗi con và coi nó như một chương trình độc lập bằng ngôn ngữ băng nhỏ. Trình thông dịch giữ hai phần trạng thái."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 239
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 148 (Div. 2)"
rating: 1500
weight: 239
solve_time_s: 212
verified: false
draft: false
---

[CF 239B - Lập trình băng dễ dàng](https://codeforces.com/problemset/problem/239/B) 

**Đánh giá:** 1500 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 3 phút 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi gồm các chữ số,`<`, Và`>`nhân vật. Mỗi truy vấn chọn một chuỗi con và coi nó như một chương trình độc lập bằng ngôn ngữ băng nhỏ. 

Trình thông dịch giữ hai phần trạng thái. Vị trí hiện tại trỏ đến một ký tự của chuỗi và hướng hiện tại là trái hoặc phải. Ban đầu chúng ta bắt đầu ở ký tự ngoài cùng bên trái và di chuyển sang bên phải. 

Các chữ số hoạt động giống như bộ đếm. Khi trình thông dịch truy cập một chữ số, nó sẽ in giá trị hiện tại, sau đó giảm chữ số đó đi một. Nếu chữ số đã có`0`, nó biến mất hoàn toàn khỏi chương trình. Sau khi xử lý chữ số, con trỏ sẽ di chuyển một bước theo hướng hiện tại. 

Các biểu tượng`<`Và`>`chỉ đổi hướng. Sau khi cập nhật hướng, con trỏ sẽ di chuyển một bước. Có một quy tắc bổ sung: nếu con trỏ chạm vào biểu tượng chỉ hướng khác ngay sau đó, biểu tượng chỉ hướng trước đó sẽ biến mất. 

Đối với mỗi truy vấn, chúng ta phải đếm số lần mỗi chữ số từ`0`ĐẾN`9`được in trong quá trình thực hiện đầy đủ. 

Những hạn chế là rất nhỏ. Cả độ dài chuỗi và số lượng truy vấn nhiều nhất là`100`. Ngay cả một mô phỏng khá nặng cho mỗi truy vấn cũng có thể chấp nhận được. Một giải pháp xung quanh`O(n^2)`hoặc thậm chí`O(n^3)`tổng số hoạt động dễ dàng phù hợp bên trong giới hạn. 

Phần khó khăn không phải là hiệu suất, mà là việc triển khai trình thông dịch chính xác như mô tả. Một số chi tiết rất dễ xử lý sai. 

Một lỗi phổ biến là cập nhật một chữ số trước khi in. Coi như:```
1
```Hành vi đúng là:```
print 1
digit becomes 0
print 0
digit disappears
```Số lượng đầu ra là:```
0 -> 1 time
1 -> 1 time
```Nếu chúng ta giảm trước và in sau, chúng ta đã bỏ sót không chính xác`1`. 

Một trường hợp tinh vi khác là xóa các ký hiệu chỉ hướng. Giả sử chương trình là:```
><
```Chúng tôi bắt đầu lúc`>`, hướng trở nên đúng và chúng ta chuyển sang`<`. Vì ký tự mới cũng là ký hiệu chỉ hướng nên ký tự trước đó`>`biến mất ngay lập tức. Việc quên việc xóa này sẽ thay đổi các chỉ số trong tương lai và phá vỡ mô phỏng. 

Sự thay đổi chỉ mục sau khi xóa là một nguồn lỗi khác. Coi như:```
0<
```Chúng tôi in`0`, xóa nó và chương trình còn lại sẽ trở thành`<`. Con trỏ hiện tại bây giờ phải tham chiếu đến ký tự tiếp theo chính xác trong mảng rút gọn. Việc triển khai bất cẩn thường tăng con trỏ trước khi điều chỉnh để xóa và vô tình bỏ qua các ký tự. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng trình thông dịch theo nghĩa đen. 

Đối với mỗi truy vấn, chúng tôi trích xuất chuỗi con và lưu trữ chuỗi đó trong cấu trúc có thể thay đổi, chẳng hạn như danh sách các ký tự. Chúng tôi duy trì vị trí và hướng đi hiện tại. Ở mỗi bước, chúng tôi thực thi các quy tắc chính xác từ câu lệnh, cập nhật chương trình nếu một ký tự biến mất và đếm các chữ số được in. 

Mô phỏng lực lượng vũ phu này đã đủ nhanh. Một chữ số chỉ có thể giảm đi một số lần nhất định trước khi biến mất. Các ký hiệu chỉ đường cũng chỉ có thể biến mất một lần. Vì độ dài chuỗi con nhiều nhất là`100`, tổng số bước thông dịch cho mỗi truy vấn là rất nhỏ. 

Một giới hạn trên thô rất dễ rút ra. Mỗi chữ số đóng góp nhiều nhất`10`in trước khi biến mất. Mỗi biểu tượng hướng biến mất nhiều nhất một lần. Ngay cả trong trường hợp xấu nhất, một truy vấn chỉ thực hiện vài nghìn thao tác. 

Thách thức là tính đúng đắn khi đột biến. Độ dài chuỗi thay đổi linh hoạt nên các chỉ mục phải được cập nhật cẩn thận sau khi xóa. Quan sát quan trọng là các ràng buộc đủ nhỏ để chúng ta không cần bất kỳ tối ưu hóa phức tạp nào. Một mô phỏng trung thực sẽ đơn giản và an toàn hơn so với việc cố gắng nén các trạng thái hoặc tính toán trước các chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(q · n²) | O(n) | Đã chấp nhận | 
| Mô phỏng có thể thay đổi cẩn thận | O(q · n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi truy vấn, hãy trích xuất chuỗi con và chuyển đổi nó thành danh sách các ký tự có thể thay đổi. 

Chúng ta cần khả năng thay đổi vì các chữ số và ký hiệu hướng có thể biến mất trong quá trình thực thi. 
2. Khởi tạo vị trí hiện tại`pos = 0`và phương hướng`dir = 1`. 

Chúng tôi sử dụng`1`cho quyền và`-1`cho bên trái. 
3. Lặp lại trong khi`0 <= pos < len(program)`. 

Việc thực thi sẽ dừng ngay khi con trỏ rời khỏi ranh giới chương trình hiện tại. 
4. Nếu ký tự hiện tại là chữ số: 

Tăng bộ đếm câu trả lời cho chữ số đó. 

Nếu chữ số là`0`, xóa ký tự khỏi danh sách. Nếu không thì giảm nó đi một. 

Sau đó di chuyển một bước theo hướng hiện tại. 

Việc xóa phải xảy ra sau khi in vì trình thông dịch sẽ in giá trị hiện tại trước. 
5. Nếu ký tự hiện tại là`<`hoặc`>`: 

Cập nhật hướng đi cho phù hợp. 

Lưu trữ chỉ mục hiện tại vì biểu tượng này có thể biến mất. 

Di chuyển một bước theo hướng mới. 
6. Sau khi di chuyển từ ký hiệu chỉ hướng, hãy kiểm tra xem vị trí mới có còn nằm trong chương trình không và ký tự mới có còn nằm trong chương trình không?`<`hoặc`>`. 

Nếu cả hai điều kiện đều đúng, hãy xóa ký hiệu chỉ hướng trước đó. 
7. Khi xóa một ký tự trước con trỏ hiện tại, hãy giảm chỉ số con trỏ đi một. 

Điều này bù đắp cho sự dịch chuyển trái trong mảng sau khi xóa. 
8. Tiếp tục cho đến khi con trỏ thoát khỏi chương trình. 

### Tại sao nó hoạt động 

Mô phỏng duy trì trạng thái chính xác giống như định nghĩa của trình thông dịch: chương trình có thể thay đổi hiện tại, vị trí con trỏ hiện tại và hướng di chuyển. 

Mọi quy tắc thông dịch được áp dụng theo thứ tự giống như câu lệnh. Các chữ số được in trước khi sửa đổi, các thay đổi hướng xảy ra trước khi di chuyển và các ký hiệu hướng chỉ bị xóa sau khi chạm vào ký hiệu hướng khác. 

Phần khó khăn duy nhất là bảo trì chỉ mục sau khi xóa. Bất cứ khi nào một ký tự trước con trỏ hiện tại biến mất, tất cả các chỉ số sau đó sẽ dịch chuyển sang trái một. Điều chỉnh con trỏ giữ nguyên bất biến`pos`luôn đề cập đến cùng một ký tự logic mà trình thông dịch sẽ truy cập tiếp theo. 

Vì mỗi lần lặp vòng lặp phản ánh chính xác một bước thông dịch nên số lượng chữ số được tạo ra là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(sub):
    s = list(sub)
    ans = [0] * 10

    pos = 0
    direction = 1

    while 0 <= pos < len(s):
        cur = s[pos]

        if cur.isdigit():
            d = int(cur)
            ans[d] += 1

            if d == 0:
                s.pop(pos)

                if direction == -1:
                    pos -= 1
            else:
                s[pos] = str(d - 1)
                pos += direction

        else:
            direction = -1 if cur == '<' else 1

            old_pos = pos
            pos += direction

            if 0 <= pos < len(s) and s[pos] in '<>':
                s.pop(old_pos)

                if old_pos < pos:
                    pos -= 1

    return ans

def solve():
    n, q = map(int, input().split())
    s = input().strip()

    out = []

    for _ in range(q):
        l, r = map(int, input().split())

        res = simulate(s[l - 1:r])
        out.append(' '.join(map(str, res)))

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo các quy tắc thông dịch trực tiếp. 

Chuỗi con cho mỗi truy vấn sẽ trở thành một danh sách vì chuỗi Python là bất biến. Việc xóa xảy ra thường xuyên, vì vậy các thao tác trên danh sách giúp việc triển khai trở nên rõ ràng hơn nhiều. 

Vòng lặp chính kết thúc khi con trỏ rời khỏi phạm vi hợp lệ hiện tại. Vì chương trình co lại linh hoạt nên chúng tôi luôn so sánh với độ dài hiện tại. 

Xử lý chữ số có hai nhánh riêng biệt. Đối với chữ số`1`bởi vì`9`, ta giảm ký tự tại chỗ và di chuyển bình thường. Vì`0`, chúng tôi xóa hoàn toàn ký tự. Việc cập nhật con trỏ sau khi xóa rất tinh tế. Nếu chúng ta di chuyển sang trái, mảng sẽ co lại trước vị trí logic tiếp theo hiện tại, do đó chúng ta giảm dần`pos`một lần nữa. 

Các ký hiệu chỉ hướng yêu cầu một sự điều chỉnh cẩn thận khác. Đầu tiên chúng ta di chuyển theo hướng đã cập nhật, sau đó kiểm tra xem đích đến có phải là biểu tượng hướng khác hay không. Nếu vậy, chúng tôi xóa cái trước đó. Khi chỉ số bị loại bỏ nằm trước vị trí hiện tại, vị trí hiện tại sẽ dịch chuyển sang trái một, do đó chúng ta bù bằng`pos -= 1`. 

Những sửa đổi chỉ mục này là sự khác biệt giữa một mô phỏng chính xác và một giải pháp âm thầm bỏ qua các ký tự sau khi xóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chương trình đầu vào:```
1>3
```| Bước | Chương trình | Vị trí | Hướng | Hành động | In | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`1>3`| 0 | Đúng | In`1`, chữ số trở thành`0`| 1 | 
| 2 |`0>3`| 1 | Đúng | Đọc`>`| | 
| 3 |`0>3`| 2 | Đúng | In`3`, chữ số trở thành`2`| 3 | 
| 4 |`0>2`| 3 | Đúng | Con trỏ thoát | | 

Số đếm cuối cùng:```
0 1 0 1 0 0 0 0 0 0
```Dấu vết này cho thấy các chữ số được in trước khi giảm. ban đầu`1`vẫn đóng góp vào câu trả lời mặc dù sau này nó trở thành`0`. 

### Ví dụ 2 

Chương trình đầu vào:```
><<
```| Bước | Chương trình | Vị trí | Hướng | Hành động | In | 
| --- | --- | --- | --- | --- | --- | 
| 1 |`><<`| 0 | Đúng | Đọc`>`, di chuyển sang phải | | 
| 2 |`><<`| 1 | Đúng | Đã hạ cánh`<`, xóa trước đó`>`| | 
| 3 |`<<`| 0 | Đúng | Đọc`<`, di chuyển sang trái | | 
| 4 |`<<`| -1 | Trái | Con trỏ thoát | | 

Số đếm cuối cùng:```
0 0 0 0 0 0 0 0 0 0
```Ví dụ này thể hiện quy tắc xóa đặc biệt cho các ký hiệu hướng liên tiếp. đầu tiên`>`biến mất ngay sau khi con trỏ chạm vào`<`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q · n²) | Mỗi truy vấn thực hiện một số lượng đột biến giới hạn và con trỏ di chuyển trên một chuỗi có độ dài tối đa 100 | 
| Không gian | O(n) | Bản sao có thể thay đổi của chuỗi con lưu trữ tối đa n ký tự | 

Với`n, q ≤ 100`, thậm chí mô phỏng bậc hai cho mỗi truy vấn cũng nhanh chóng. Tổng số lượng công việc vẫn ở mức dưới một triệu hoạt động. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    import sys
    input = sys.stdin.readline

    def simulate(sub):
        s = list(sub)
        ans = [0] * 10

        pos = 0
        direction = 1

        while 0 <= pos < len(s):
            cur = s[pos]

            if cur.isdigit():
                d = int(cur)
                ans[d] += 1

                if d == 0:
                    s.pop(pos)

                    if direction == -1:
                        pos -= 1
                else:
                    s[pos] = str(d - 1)
                    pos += direction

            else:
                direction = -1 if cur == '<' else 1

                old_pos = pos
                pos += direction

                if 0 <= pos < len(s) and s[pos] in '<>':
                    s.pop(old_pos)

                    if old_pos < pos:
                        pos -= 1

        out.append(' '.join(map(str, ans)))

    n, q = map(int, input().split())
    s = input().strip()

    out = []

    for _ in range(q):
        l, r = map(int, input().split())
        simulate(s[l - 1:r])

    return '\n'.join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run(
"""7 4
1>3>22<
1 3
4 7
7 7
1 7
"""
) == (
"""0 1 0 1 0 0 0 0 0 0
2 2 2 0 0 0 0 0 0 0
0 0 0 0 0 0 0 0 0 0
2 3 2 1 0 0 0 0 0 0"""
)

# single zero
assert run(
"""1 1
0
1 1
"""
) == (
"""1 0 0 0 0 0 0 0 0 0"""
)

# repeated digit decrements
assert run(
"""1 1
2
1 1
"""
) == (
"""1 1 1 0 0 0 0 0 0 0"""
)

# consecutive direction symbols
assert run(
"""2 1
><
1 2
"""
) == (
"""0 0 0 0 0 0 0 0 0 0"""
)

# boundary movement to the left
assert run(
"""2 1
<1
1 2
"""
) == (
"""0 0 0 0 0 0 0 0 0 0"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`| một in số không | Xử lý đúng việc xóa chữ số | 
|`2`| bản in`2,1,0`| Chữ số được in trước khi giảm | 
|`><`| không có đầu ra | Xóa biểu tượng hướng liên tiếp | 
|`<1`| không có đầu ra | Thoát ngay sau khi di chuyển sang trái | 

## Vỏ cạnh 

Hãy xem xét chương trình:```
0
```Việc thực thi bắt đầu trên chữ số`0`. Trình thông dịch in`0`, sau đó xóa ký tự vì không thể giảm thêm. Con trỏ bây giờ trỏ ra ngoài chương trình trống, do đó việc thực thi sẽ dừng lại. 

Thuật toán xử lý việc này một cách chính xác vì`d == 0`nhánh loại bỏ ký tự ngay sau khi đếm bản in. 

Bây giờ hãy xem xét:```
><
```Con trỏ bắt đầu vào`>`, đổi hướng sang phải và di chuyển lên`<`. Vì đích đến cũng là biểu tượng chỉ đường nên điểm đến trước đó`>`biến mất. Con trỏ bây giờ nằm ​​trên`<`, nó sẽ gửi nó sang bên trái và ngay lập tức bên ngoài chương trình. 

Việc triển khai tái tạo hành vi này bằng cách sử dụng`old_pos`và chỉ xóa biểu tượng trước đó sau khi di chuyển. 

Cuối cùng, hãy xem xét:```
1<
```Con trỏ in`1`, thay đổi chữ số thành`0`, và di chuyển ngay vào`<`. các`<`chuyển hướng sang trái, di chuyển trở lại`0`. các`0`được in và xóa, sau đó con trỏ thoát ra bên trái. 

Chi tiết quan trọng ở đây là điều chỉnh chỉ mục sau khi xóa trong khi di chuyển sang trái. Không có`pos -= 1`ở nhánh xóa, quá trình mô phỏng sẽ tiếp tục ở sai vị trí sau khi mảng thu nhỏ lại.
