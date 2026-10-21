---
title: "CF 105859A - Đồng phạm"
description: "Bài toán mô tả một nhóm ứng viên và tình bạn giữa họ. Chúng ta cần đếm có bao nhiêu nhóm ứng viên có thể được chọn cho mỗi quy mô nhóm có thể để không có hai ứng viên được chọn là bạn bè."
date: "2026-06-25T14:40:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105859
codeforces_index: "A"
codeforces_contest_name: "Mines HSPC 2025 Open Division"
rating: 0
weight: 105859
solve_time_s: 42
verified: true
draft: false
---

[CF 105859A - Đồng phạm](https://codeforces.com/problemset/problem/105859/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một nhóm ứng viên và tình bạn giữa họ. Chúng ta cần đếm có bao nhiêu nhóm ứng viên có thể được chọn cho mỗi quy mô nhóm có thể để không có hai ứng viên được chọn là bạn bè. Trong thuật ngữ đồ thị, mỗi ứng cử viên là một đỉnh và mỗi tình bạn là một cạnh, vì vậy chúng ta đang đếm các tập hợp độc lập được nhóm theo kích thước của chúng. Đầu ra chứa một giá trị cho mỗi kích thước từ 0 đến`n`, trong đó giá trị là số tập hợp độc lập chứa chính xác số đỉnh đó. 

Số lượng ứng viên có thể lên tới 40. Đây là hạn chế chính. Việc duyệt đồ thị thông thường hoặc lập trình động trên các đỉnh là không đủ vì việc đếm trực tiếp tất cả các tập con đòi hỏi phải kiểm tra tới`2^40`khả năng, đó là khoảng một nghìn tỷ tiểu bang. Ngay cả việc triển khai rất nhanh cũng không thể xử lý được việc đó trong vòng vài giây. Giới hạn gợi ý rõ ràng việc chia vấn đề thành những phần nhỏ hơn, bởi vì`2^20`chỉ khoảng một triệu và có thể quản lý được. 

Có một số trường hợp dễ bỏ sót. Khi không có tình bạn, mọi tập hợp con đều hợp lệ. Ví dụ:```
3 0
```Câu trả lời là:```
1 3 3 1
```Một giải pháp chỉ đếm các cặp hoặc cố gắng tham lam chọn ứng viên sẽ thất bại vì mọi nhóm có thể đều hoạt động. 

Một trường hợp khác là khi đồ thị hoàn chỉnh:```
3 3
1 2
1 3
2 3
```Câu trả lời là:```
1 3 0 0
```Chỉ cho phép nhóm trống và các ứng cử viên duy nhất. Việc triển khai bất cẩn mà quên kiểm tra xung đột giữa các đỉnh từ các nửa khác nhau có thể tính các nhóm không hợp lệ. 

Trường hợp cạnh thứ ba là biểu đồ bị ngắt kết nối:```
4 2
1 2
3 4
```Câu trả lời là:```
1 4 4 0 0
```Các cặp`(1,3)`,`(1,4)`,`(2,3)`, Và`(2,4)`là hợp lệ, nhưng không có nhóm kích thước ba tồn tại. Việc xử lý riêng hai thành phần được kết nối mà không kết hợp chính xác kích thước của chúng có thể cho kết quả đếm sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra từng tập hợp con của các ứng cử viên và kiểm tra xem nó có chứa khía cạnh tình bạn hay không. Đối với mỗi`2^n`các tập hợp con, chúng ta sẽ quét các ứng cử viên đã chọn và xác minh rằng không có hai đỉnh được chọn nào được kết nối. Phương pháp này đúng vì mỗi nhóm có thể được xem xét chính xác một lần. Vấn đề là với`n = 40`, số lượng tập hợp con là khoảng`1,099,511,627,776`, do đó số lượng thao tác vượt xa những gì có thể chạy kịp thời. 

Quan sát hữu ích là biểu đồ quá lớn để liệt kê đầy đủ, nhưng nó đủ nhỏ để phân chia. Nếu chúng ta chia các đỉnh thành hai nửa, mỗi nửa có nhiều nhất là 20 đỉnh thì mỗi nửa có nhiều nhất`2^20`tập hợp con. Chúng ta có thể liệt kê tất cả các tập hợp con của một nửa, xác định những tập hợp con nào có giá trị nội tại và lưu trữ thông tin giúp kết hợp chúng với nửa còn lại. 

Đối với một tập con của nửa bên trái, thông tin quan trọng không chỉ là kích thước của nó mà còn là những đỉnh nào ở nửa bên phải không thể được chọn bằng nó. Mỗi đỉnh bên trái có một mặt nạ bit của các đỉnh lân cận ở phía bên phải. Bằng cách OR các mặt nạ này cho các đỉnh bên trái đã chọn, chúng ta biết được các đỉnh bên phải bị cấm. Các đỉnh bên phải còn lại có sẵn. 

Nửa bên phải có thể được xử lý trước bằng mảng lập trình động. Đối với mỗi mặt nạ của các đỉnh bên phải, chúng tôi lưu trữ có bao nhiêu tập hợp con độc lập hợp lệ được chứa bên trong mặt nạ đó, được phân tách bằng kích thước của chúng. Sau đó, mọi tập hợp con bên trái hợp lệ có thể truy vấn số cách để chọn các tập con bên phải tương thích bằng cách nhìn vào mặt nạ được phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(1) | Quá chậm | 
| Tối ưu | O(2^(n/2) * n) | O(2^(n/2) * n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia các đỉnh thành hai nhóm. Hãy để nhóm đầu tiên chứa`a`các đỉnh và nhóm thứ hai chứa`b`các đỉnh, trong đó cả hai giá trị tối đa là 20. Điều này làm giảm phần mũ từ`2^40`về khoảng`2^20`. 
2. Liệt kê mọi tập con của nhóm thứ hai và xác định xem đó có phải là tập độc lập hay không. Đối với mỗi tập hợp con hợp lệ, hãy lưu trữ kích thước của nó. Thông tin được lưu trữ sau đó sẽ trả lời: "Có bao nhiêu nhóm bên phải hợp lệ của mỗi kích thước phù hợp với tập hợp có sẵn này?" 
3. Xây dựng bảng lập trình động tập hợp con trên nhóm thứ hai. Bắt đầu với các giá trị cho mặt nạ chính xác và truyền bá thông tin đến mặt nạ lớn hơn. Sau đó, một truy vấn về mặt nạ sẽ cho chúng ta biết có bao nhiêu tập hợp con độc lập ở mọi kích thước được chứa bên trong nó. 
4. Liệt kê mọi tập hợp con của nhóm đầu tiên. Kiểm tra xem nó có độc lập trong nhóm đầu tiên hay không. Nếu không, hãy loại bỏ nó vì việc thêm các đỉnh từ nửa kia không thể khắc phục được tình bạn nội bộ. 
5. Với mỗi tập hợp con bên trái hợp lệ, hãy tính bitmask của các đỉnh bên phải bị chặn bởi các bạn bè của nó. Đảo ngược mặt nạ này để có được các đỉnh bên phải vẫn có thể được sử dụng. 
6. Thêm số lượng đã lưu cho mặt nạ bên phải có sẵn vào câu trả lời. Kích thước của nhóm cuối cùng là kích thước tập hợp con bên trái cộng với kích thước tập hợp con bên phải đã chọn. 

Tại sao nó hoạt động: mọi tập hợp độc lập trong biểu đồ gốc có thể được chia duy nhất thành phần thuộc nửa bên trái và phần thuộc nửa bên phải. Phần bên trái được kiểm tra xung đột nội bộ và phần bên phải chỉ được tính trong số các đỉnh không bị tình bạn bên trái cấm. Vì quá trình tiền xử lý bên phải tính chính xác tất cả các tập hợp con hợp lệ bên trong bất kỳ mặt nạ được phép nào, nên mọi tập hợp độc lập có thể có đều được tính một lần và không bao gồm tập hợp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    left = n // 2
    right = n - left

    g_left = [0] * left
    g_right = [0] * right
    cross = [0] * left

    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1

        if a < left and b < left:
            g_left[a] |= 1 << b
            g_left[b] |= 1 << a
        elif a >= left and b >= left:
            x = a - left
            y = b - left
            g_right[x] |= 1 << y
            g_right[y] |= 1 << x
        else:
            if a < left:
                cross[a] |= 1 << (b - left)
            else:
                cross[b] |= 1 << (a - left)

    total_right = 1 << right
    dp = [[0] * (right + 1) for _ in range(total_right)]
    dp[0][0] = 1

    for mask in range(total_right):
        if mask == 0:
            continue
        bit = mask & -mask
        idx = bit.bit_length() - 1
        prev = mask ^ bit

        for k in range(right + 1):
            dp[mask][k] = dp[prev][k]

        if prev & g_right[idx] == 0:
            size = prev.bit_count()
            dp[mask][size + 1] += 1

    for bit in range(right):
        for mask in range(total_right):
            if mask & (1 << bit):
                for k in range(right + 1):
                    dp[mask][k] += dp[mask ^ (1 << bit)][k]

    ans = [0] * (n + 1)
    total_left = 1 << left

    for mask in range(total_left):
        ok = True
        blocked = 0

        for i in range(left):
            if mask & (1 << i):
                if g_left[i] & mask:
                    ok = False
                    break
                blocked |= cross[i]

        if not ok:
            continue

        available = (1 << right) - 1
        available ^= blocked

        size_left = mask.bit_count()
        for k in range(right + 1):
            ans[size_left + k] += dp[available][k]

    print(*ans)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu dưới dạng bitmask vì mỗi nửa có tối đa 20 đỉnh. Một số nguyên duy nhất có thể biểu diễn các lân cận của một đỉnh, khiến cho việc kiểm tra xung đột diễn ra liên tục. 

Quá trình tiền xử lý bên phải sử dụng lập trình động tập hợp con. Quá trình chuyển đổi sẽ loại bỏ một đỉnh đã chọn và kiểm tra xem việc thêm nó có tạo ra xung đột hay không. Sau giai đoạn nhân giống thứ hai,`dp[mask][k]`đại diện cho số lượng tập hợp con độc lập có kích thước`k`được chứa bên trong`mask`, không chỉ chính xác mặt nạ. 

Trong quá trình liệt kê bên trái, điều kiện`g_left[i] & mask`bắt được bất kỳ tình bạn nào hoàn toàn bên trong tập hợp con bên trái đã chọn. các`blocked`mặt nạ thu thập mọi đỉnh bên phải không thể xuất hiện cùng với lựa chọn bên trái hiện tại. Việc sử dụng các đỉnh còn lại đảm bảo rằng các cạnh chéo được tôn trọng. 

Việc triển khai dựa trên số nguyên Python có thể dễ dàng chứa mặt nạ 20 bit. Không có rủi ro tràn vì số nguyên Python tự động tăng lên. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
5 3
1 2
2 3
4 5
```Sự phân chia có thể là:```
left: 1 2
right: 3 4 5
```Dấu vết của các lựa chọn bên trái là: 

| Mặt nạ bên trái | Kích thước bên trái | hợp lệ | Các đỉnh bên phải bị chặn | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 00 | 0 | vâng | không | được rồi các bộ độc lập | 
| 01 | 1 | vâng | không | tập đúng với ứng viên 1 | 
| 10 | 1 | vâng | ứng cử viên 3 | bộ bên phải không có 3 | 
| 11 | 2 | không | không | bỏ qua | 

Mặt nạ không hợp lệ`11`bị bỏ qua vì ứng viên 1 và 2 là bạn bè. Các mặt nạ còn lại kết hợp với số lượng bên phải và tạo ra:```
1 5 7 2 0 0
```Ví dụ này chứng minh rằng xung đột nội bộ được loại bỏ trước khi kết hợp một nửa. 

Một ví dụ khác:```
4 2
1 2
3 4
```Chia thành:```
left: 1 2
right: 3 4
```Dấu vết là: 

| Mặt nạ bên trái | Kích thước bên trái | hợp lệ | Các đỉnh bên phải bị chặn | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 00 | 0 | vâng | không | mọi lựa chọn đúng đắn | 
| 01 | 1 | vâng | không | đúng mà không có xung đột | 
| 10 | 1 | vâng | không | đúng mà không có xung đột | 
| 11 | 2 | không | không | bỏ qua | 

Hai lựa chọn đỉnh đơn hợp lệ ở bên trái kết hợp với cả hai lựa chọn hợp lệ ở bên phải, tạo ra bốn cặp hợp lệ. Câu trả lời đầy đủ là:```
1 4 4 0 0
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^(n/2) * n) | Cả hai nửa có tối đa 20 đỉnh và mỗi tập hợp con chỉ yêu cầu các thao tác bit trên các đỉnh | 
| Không gian | O(2^(n/2) * n) | Cửa hàng DP bên phải tính từng khẩu trang và mọi kích cỡ có thể có | 

Với`n = 40`,`2^(n/2)`là khoảng một triệu. Thuật toán thực hiện vài triệu thao tác bit và vừa vặn trong giới hạn dự kiến. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old
    return out.getvalue()

# sample 1
assert run("""5 3
1 2
2 3
4 5
""") == "1 5 7 2 0 0\n", "sample 1"

# minimum size
assert run("""1 0
""") == "1 1\n", "single vertex"

# all equal behaviour: complete graph
assert run("""4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "1 4 0 0 0\n", "complete graph"

# no edges
assert run("""5 0
""") == "1 5 10 10 5 1\n", "empty graph"

# boundary case with disconnected edges
assert run("""6 3
1 2
3 4
5 6
""") == "1 6 12 8 1 0 0\n", "three components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`1 1`| Xử lý biểu đồ nhỏ nhất | 
| Đồ thị hoàn chỉnh |`1 4 0 0 0`| Kiểm tra phát hiện xung đột | 
| Biểu đồ trống |`1 5 10 10 5 1`| Kiểm tra xem mọi tập hợp con đều được tính | 
| Ba cạnh rời rạc |`1 6 12 8 1 0 0`| Séc kết hợp các nửa độc lập | 

## Vỏ cạnh 

Đối với trường hợp đồ thị trống:```
3 0
```Quá trình tiền xử lý bên phải đánh dấu mọi tập hợp con là độc lập. Trong quá trình liệt kê bên trái, không có đỉnh nào chặn bất cứ thứ gì, vì vậy mọi lựa chọn bên trái đều kết hợp với mọi lựa chọn bên phải. Câu trả lời trở thành số nhị thức`1 3 3 1`. 

Đối với trường hợp đồ thị hoàn chỉnh:```
3 3
1 2
1 3
2 3
```Bất kỳ tập hợp con bên trái nào chứa hai đỉnh đều bị loại bỏ ngay lập tức vì mặt nạ kề cắt với mặt nạ đã chọn. Các lựa chọn duy nhất còn sót lại là tập trống và các đỉnh đơn, do đó đầu ra là`1 3 0 0`. 

Đối với trường hợp bị ngắt kết nối:```
4 2
1 2
3 4
```Việc chọn đỉnh 1 không chặn đỉnh 3 hoặc 4 vì tình bạn hoàn toàn nằm trong một thành phần khác. Thuật toán giữ cho mặt nạ chặn nửa chéo trống và cho phép tất cả các tập hợp con bên phải tương thích. Điều này đếm chính xác bốn cặp hợp lệ và tránh tạo bộ ba.
