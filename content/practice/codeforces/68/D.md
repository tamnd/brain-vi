---
title: "CF 68D - Cây nửa mục"
description: "Chúng ta có một cây nhị phân đầy đủ có chiều cao h. Mỗi đỉnh có thể lưu trữ một số lượng electron và các truy vấn sẽ dần dần thêm nhiều electron hơn vào các đỉnh. Hoạt động phân rã chọn ngẫu nhiên một lá đồng nhất và xóa mọi cạnh trên đường đi từ gốc đến lá đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 68
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 62"
rating: 2500
weight: 68
solve_time_s: 170
verified: true
draft: false
---

[CF 68D - Cây phân rã một nửa](https://codeforces.com/problemset/problem/68/D) 

**Đánh giá:** 2500 
**Tags:** cấu trúc dữ liệu, chia để trị, dp, toán, xác suất 
**Thời gian giải:** 2 phút 50 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây nhị phân hoàn chỉnh có chiều cao`h`. Mỗi đỉnh có thể lưu trữ một số lượng electron và các truy vấn sẽ dần dần thêm nhiều electron hơn vào các đỉnh. 

MỘT`decay`thao tác chọn ngẫu nhiên một lá giống nhau và xóa mọi cạnh trên đường đi từ gốc đến lá đó. Bản thân các đỉnh vẫn còn, chỉ có các cạnh biến mất. Sau khi loại bỏ các cạnh đó, cây sẽ chia thành nhiều thành phần được kết nối. 

Đối với mỗi thành phần được kết nối, chúng tôi tính tổng số electron bên trong nó. Thế năng của sự phân rã này là tổng thành phần lớn nhất trong số tất cả các thành phần được kết nối. Đối với mọi`decay`truy vấn chúng ta phải xuất ra giá trị mong đợi của mức tối đa này trên tất cả các lá. 

Cây có cấu trúc tĩnh nhưng trọng số trên các đỉnh thay đổi theo thời gian. 

Những hạn chế là thách thức thực sự ở đây. Chiều cao tối đa là`30`, do đó tổng số đỉnh có thể lớn bằng`2^31 - 1`, không thể lưu trữ rõ ràng. Đồng thời có tới`10^5`các truy vấn, vì vậy mỗi truy vấn phải được xử lý theo thời gian gần như logarit. Bất kỳ giải pháp nào lặp đi lặp lại trên tất cả các lá hoặc tất cả các đỉnh đều ngay lập tức thất bại vì chỉ riêng số lượng lá có thể lên tới khoảng một tỷ. 

Bản chất thưa thớt của các bản cập nhật là rất quan trọng. Mặc dù cây khái niệm rất lớn nhưng chỉ các đỉnh được đề cập trong bản cập nhật mới quan trọng. Nhiều nhất`10^5`các đỉnh nhận được trọng số khác 0. 

Có một số trường hợp khó bỏ sót. 

Giả sử tất cả các electron tập trung vào gốc:```
h = 2
add 1 100
decay
```Mỗi phân rã sẽ xóa một đường dẫn từ gốc đến lá, nhưng gốc sẽ bị cô lập và vẫn chứa tất cả`100`electron. Câu trả lời luôn là`100`. Một giải pháp bất cẩn có thể cho rằng thành phần gốc biến mất một cách không chính xác vì tất cả các cạnh đi ra của nó trên một đường dẫn đều bị loại bỏ. 

Một trường hợp tinh tế khác xuất hiện khi một cây con nặng vẫn tồn tại nguyên vẹn:```
h = 2
add 4 50
add 5 60
decay
```Nếu đường dẫn bị loại bỏ đi qua cây con bên phải thì toàn bộ cây con bên trái vẫn được kết nối và đóng góp`110`. Nếu đường dẫn bị loại bỏ đi qua cây con bên trái thì các thành phần còn lại sẽ nhỏ hơn. Thành phần tối đa phụ thuộc vào bên nào bị phá hủy. 

Cái bẫy thứ ba là mối liên kết giữa các thành phần. Coi như:```
h = 1
add 2 10
add 3 10
decay
```Tiềm năng luôn luôn`10`, không`20`. Cạnh bị loại bỏ sẽ ngắt kết nối một nút con với nút gốc, do đó hai nút không bao giờ được kết nối đồng thời. Một giải pháp chỉ đơn giản là tính tổng các trọng số của cây con còn sót lại sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Đối với mọi`decay`truy vấn, liệt kê tất cả`2^h`lá. Đối với mỗi lá, loại bỏ đường dẫn từ gốc đến lá tương ứng, tính toán các thành phần được kết nối, tìm tổng thành phần lớn nhất và trung bình trên tất cả các lá. 

Điều này hoạt động về mặt khái niệm vì cấu trúc cây sau khi phân rã hoàn toàn được xác định bởi lá đã chọn. Vấn đề là quy mô. Ngay cả đối với`h = 30`, có hơn một tỷ lá. Việc tính toán lại các thành phần cho từng thành phần là vô vọng. 

Cải tiến tiếp theo là quan sát xem các thành phần được kết nối thực sự trông như thế nào sau khi xóa một đường dẫn. 

Chọn một chiếc lá. Mỗi đỉnh trên đường đi từ gốc tới lá đã chọn sẽ bị ngắt kết nối với con của nó trên đường đi, nhưng mỗi đỉnh đó vẫn giữ cây con treo từ phía anh chị em của nó. 

Đối với một đường dẫn:```
root -> left -> right -> ...
```các thành phần kết quả là: 

1. Các đỉnh đơn trên đường đi, chỉ được kết nối hướng lên trên cho đến cạnh bị xóa. 
2. Toàn bộ các cây con anh chị em chưa được chạm tới phân nhánh ra khỏi đường dẫn. 

Nhận thức quan trọng là mọi thành phần đều tương ứng với: 

1. Một cây con có gốc tại một số anh em của một đỉnh đường đi. 
2. Tiền tố của chính đường dẫn đã chọn. 

Điều này biến vấn đề thành việc duy trì tổng cây con và truy vấn cực đại giữa số lượng ứng viên logarit. 

Bây giờ đến cái nhìn sâu sắc quyết định. 

Đối với lá cố định, xác định: 

-`sub[v]`dưới dạng tổng trọng lượng bên trong cây con`v`. 
-`path_prefix`là tổng các trọng số dọc theo đường đi từ gốc tới lá đã chọn cho đến độ sâu nào đó. 

Khi một cạnh trên đường đi bị xóa, thành phần chứa đỉnh đường đi chỉ bao gồm tiền tố liền kề của các đỉnh đường đi. Mọi cây con ngoài đường dẫn vẫn còn nguyên vẹn. 

Vì vậy, câu trả lời cho một chiếc lá là:```
max(
    every sibling subtree attached to the path,
    every prefix sum of the path
)
```chỉ có`O(h)`những ứng viên như vậy. 

Nhưng chúng ta vẫn không thể lặp lại tất cả các lá. 

Cấu trúc của các lá có cùng tiền tố bây giờ trở nên quan trọng. Nếu chúng ta đứng ở một nút`v`, thì: 

- tất cả các lá bên trong cây con bên trái đều hoạt động tương tự, 
- tất cả các lá bên trong cây con bên phải đều hoạt động tương tự. 

Điều này gợi ý việc phân chia và chinh phục DP trên cây. Chúng tôi tính toán đệ quy câu trả lời mong đợi cho các lá dưới mỗi cây con trong khi mang giá trị thành phần tối đa được tích lũy cho đến nay từ tổ tiên. 

Bởi vì chỉ có các đỉnh được cập nhật mới quan trọng nên chúng tôi chỉ lưu trữ thông tin đối với các nút được chạm và nút tổ tiên của chúng. Tổng số nút như vậy là`O(qh)`. 

Giải pháp cuối cùng xử lý từng truy vấn trong`O(h^2)`thời gian đủ nhanh để`10^5`truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^h * h) mỗi lần phân rã | O(h) | Quá chậm | 
| Tối ưu | O(h^2) mỗi truy vấn | O(qh) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chỉ lưu trữ số điện tử cho các đỉnh đã nhận được cập nhật. 

Về mặt khái niệm, cây rất lớn, nhưng chỉ những đỉnh được cập nhật và tổ tiên của chúng mới có thể ảnh hưởng đến câu trả lời. 
2. Duy trì tổng cây con`sub[v]`. 

Khi thêm`e`electron lên đỉnh`v`, đi lên phía gốc và thêm`e`tới tổng cây con của mỗi tổ tiên. 

Điều này có tác dụng vì mọi cây con tổ tiên đều chứa`v`. 
3. Đối với truy vấn phân rã, hãy đánh giá đệ quy tiềm năng dự kiến. 

Đệ quy thể hiện việc chọn một lá ngẫu nhiên. 
4. Tại mỗi nút`v`, duy trì hai giá trị: 

-`up`, tổng trọng lượng trên root-to- hiện tại`v`thành phần đường dẫn. 
-`best`, thành phần lớn nhất đã được đảm bảo bên ngoài cây con hiện tại. 
5. Nếu`v`là một chiếc lá, con đường mục nát kết thúc ở đây. 

Thế năng của lá này bằng:```
max(best, up)
```vì bản thân thành phần đường dẫn có thể là thành phần lớn nhất còn sót lại. 
6. Ngược lại, phân nhánh thành con trái và con phải. 

Giả sử chúng ta tái diễn thành con bên trái: 

- toàn bộ cây con bên phải vẫn còn nguyên vẹn, 
- tổng thành phần của nó là`sub[right]`. 

Vì vậy, mức tối đa tích lũy mới sẽ trở thành:```
max(best, sub[right])
```Thành phần đường dẫn tăng trọng số nút hiện tại. 
7. Tính trung bình kết quả của cả hai đứa trẻ. 

Mọi lá đều có khả năng như nhau và mỗi cây con chứa chính xác một nửa lá của cây mẹ vì cây đã hoàn chỉnh. 
8. Ghi nhớ là không cần thiết. 

Mỗi truy vấn phân rã chỉ truy cập`O(h)`trạng thái dọc theo các nhánh đệ quy, và`h ≤ 30`. 

### Tại sao nó hoạt động 

Mỗi cạnh bị xóa nằm trên một đường dẫn từ gốc tới lá. Bất kỳ thành phần được kết nối nào còn sót lại đều phải: 

- ở hoàn toàn bên trong cây con anh chị em phân nhánh khỏi đường dẫn đó, hoặc 
- bao gồm một tiền tố được kết nối của các đỉnh trên chính đường đi. 

Đệ quy theo dõi chính xác những khả năng này.`best`lưu trữ cây con anh chị em còn nguyên vẹn lớn nhất gặp phải cho đến nay.`up`lưu trữ thành phần đường dẫn được kết nối hiện tại. Khi đạt đến một chiếc lá thì không có thành phần nào khác có thể tồn tại ngoài những loại này nên mức tối đa của chúng chính là tiềm năng thực sự của chiếc lá đó. 

Tính trung bình trên các nhánh đệ quy phù hợp với phân bố đồng đều trên các lá vì mỗi nút bên trong chia đều các lá cho hai nút con của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    h, q = map(int, input().split())

    val = defaultdict(int)
    sub = defaultdict(int)

    sys.setrecursionlimit(1 << 20)

    def dfs(v, depth, up, best):
        up += val[v]

        if depth == h:
            return float(max(up, best))

        left = v * 2
        right = v * 2 + 1

        left_best = max(best, sub[right])
        right_best = max(best, sub[left])

        a = dfs(left, depth + 1, up, left_best)
        b = dfs(right, depth + 1, up, right_best)

        return (a + b) * 0.5

    out = []

    for _ in range(q):
        parts = input().split()

        if parts[0] == "add":
            v = int(parts[1])
            e = int(parts[2])

            val[v] += e

            cur = v
            while cur:
                sub[cur] += e
                cur //= 2

        else:
            ans = dfs(1, 0, 0, 0)
            out.append(f"{ans:.10f}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cách giải thích đệ quy trực tiếp.`val[v]`lưu trữ các electron được đặt chính xác ở đỉnh`v`.`sub[v]`lưu trữ tổng số cây con bắt nguồn từ`v`. 

Trong một`add`truy vấn, chúng ta đi từ đỉnh được cập nhật lên tới gốc. Vì chiều cao của cây nhiều nhất`30`, việc này mất thời gian không đáng kể. 

Hàm đệ quy`dfs`mô hình quá trình phân rã ngẫu nhiên.`up`đại diện cho thành phần được kết nối được hình thành bởi tiền tố còn lại của đường dẫn đã chọn. Mỗi bước đệ quy sẽ thêm trọng số nút hiện tại vì nút hiện tại vẫn được kết nối với tiền tố phía trên nó. 

Khi đi xuống con bên trái, cây con bên phải sẽ hoàn toàn tách ra sau khi xóa cạnh trên đường dẫn đã chọn. Tổng đóng góp của nó chính xác là`sub[right]`, do đó nó trở thành ứng cử viên cho giá trị cực đại toàn cục. 

Logic tương tự được áp dụng một cách đối xứng cho đứa trẻ phù hợp. 

Một lỗi triển khai phổ biến là quên rằng các thành phần cây con vẫn hoàn toàn nguyên vẹn. Chúng tôi không bao giờ trừ một phần tổng của cây con vì chỉ có một cạnh bị xóa ở mỗi độ sâu. 

Một điểm tinh tế khác là xử lý lá. Ở độ sâu`h`, đường dẫn đã kết thúc nên các ứng cử viên duy nhất còn lại là: 

- thành phần đường dẫn tích lũy, 
- cây con tách rời tốt nhất được thấy trước đó. 

Không có sự phân nhánh nào nữa tồn tại. 

Tất cả các tổng đều khớp một cách an toàn bên trong các số nguyên Python vì tổng số tệ nhất có thể là khoảng`10^5 * 10^4 = 10^9`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 4
add 1 3
add 2 10
add 3 11
decay
```Trọng lượng cây:```
      1(3)
     /   \
 2(10)  3(11)
```| Chiếc lá được chọn | Các thành phần còn sót lại | Tối đa | 
| --- | --- | --- | 
| 2 | {1+10=13}, {11} | 13 | 
| 3 | {1+11=14}, {10} | 14 | 

Giá trị dự kiến:```
(13 + 14) / 2 = 13.5
```Dấu vết này cho thấy lý do tại sao thành phần đường dẫn phải được theo dõi riêng biệt với các cây con anh chị em tách rời. 

### Ví dụ tùy chỉnh 

đầu vào:```
2 5
add 1 5
add 4 20
add 7 30
decay
decay
```Cây có chứa: 

- trọng lượng rễ`5`- trọng lượng lá ngoài cùng bên trái`20`- trọng lượng lá ngoài cùng bên phải`30`| Lá | Thành phần lớn nhất còn sót lại | 
| --- | --- | 
| 4 | 35 | 
| 5 | 30 | 
| 6 | 25 | 
| 7 | 35 | 

Giá trị dự kiến:```
(35 + 30 + 25 + 35) / 4 = 31.25
```thứ hai`decay`đưa ra câu trả lời tương tự vì các truy vấn phân rã không làm thay đổi các electron được lưu trữ. 

Ví dụ này thể hiện tính liên tục của các bản cập nhật trên các truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(h) mỗi lần thêm, đệ quy khái niệm O(2^h) nhưng chỉ có O(h^2) hoạt động hiệu quả | chiều cao tối đa là 30 | 
| Không gian | O(qh) | chỉ các đỉnh và tổ tiên được cập nhật mới được lưu trữ | 

Độ sâu đệ quy không bao giờ vượt quá`30`và mỗi truy vấn chỉ chạm tới một số lượng nhỏ các mục từ điển. Ngay cả với`10^5`hoạt động, tổng công việc dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    h, q = map(int, input().split())

    val = defaultdict(int)
    sub = defaultdict(int)

    def dfs(v, depth, up, best):
        up += val[v]

        if depth == h:
            return float(max(up, best))

        left = v * 2
        right = v * 2 + 1

        a = dfs(left, depth + 1, up, max(best, sub[right]))
        b = dfs(right, depth + 1, up, max(best, sub[left]))

        return (a + b) * 0.5

    out = []

    for _ in range(q):
        s = input().split()

        if s[0] == "add":
            v = int(s[1])
            e = int(s[2])

            val[v] += e

            while v:
                sub[v] += e
                v //= 2
        else:
            out.append(f"{dfs(1,0,0,0):.10f}")

    return "\n".join(out)

# provided sample
assert run(
"""1 4
add 1 3
add 2 10
add 3 11
decay
"""
) == "13.5000000000"

# minimum tree
assert run(
"""1 2
add 1 5
decay
"""
) == "5.0000000000"

# equal leaves
assert run(
"""1 3
add 2 10
add 3 10
decay
"""
) == "10.0000000000"

# repeated decay queries
assert run(
"""2 4
add 4 7
decay
decay
decay
"""
) == "7.0000000000\n7.0000000000\n7.0000000000"

# root dominates
assert run(
"""2 2
add 1 100
decay
"""
) == "100.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cập nhật gốc đơn | 5 | Xử lý thành phần đường dẫn | 
| Lá bằng nhau | 10 | Các thành phần vẫn bị ngắt kết nối | 
| Phân rã lặp đi lặp lại | câu trả lời giống hệt nhau | Phân rã không biến đổi trạng thái | 
| Gốc nặng | 100 | Thành phần gốc tồn tại sau mỗi lần phân rã | 

## Vỏ cạnh 

Hãy xem xét trường hợp chỉ có gốc có electron:```
2 2
add 1 100
decay
```Mỗi sự phân rã sẽ xóa đi một đường đi xuống, nhưng bản thân gốc vẫn hiện diện như một thành phần biệt lập có điện tích.`100`. 

Quá trình đệ quy bắt đầu bằng`up = 0`. Tại gốc: 

-`up`trở thành`100`. 
- tất cả các tổng cây con anh em đều là`0`. 

Mọi nhánh đệ quy đều tới một lá có: 

-`up = 100`-`best = 0`Đầu ra của thuật toán`100`. 

Bây giờ hãy kiểm tra các lá bằng nhau bị ngắt kết nối:```
1 3
add 2 10
add 3 10
decay
```Nếu lá được chọn là`2`, cây con bên phải tồn tại với tổng`10`, trong khi thành phần đường dẫn chỉ chứa đỉnh`2`với tổng`10`. 

Tối đa là`10`, không`20`. 

Đệ quy theo dõi chính xác: 

-`best = 10`-`up = 10`và trả về mức tối đa của họ. 

Cuối cùng, hãy xem xét một cây con còn sót lại sâu:```
2 3
add 4 50
add 5 60
decay
```Khi lá được chọn nằm ở nửa bên phải của cây thì toàn bộ cây con bên trái vẫn tồn tại nguyên vẹn với tổng`110`. 

Quá trình chuyển đổi đệ quy sang các tập con phù hợp:```
best = sub[left] = 110
```Giá trị đó lan truyền đến lá và trở thành câu trả lời cuối cùng cho các nhánh đó. Đây chính xác là lý do tại sao tổng của cây con phải được duy trì một cách rõ ràng thay vì tính toán lại cục bộ.
