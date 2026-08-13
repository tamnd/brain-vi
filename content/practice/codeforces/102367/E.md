---
title: "CF 102367E - Ghép nối XOR"
description: "Chúng ta có một số chẵn các viên đá được lập chỉ mục và mỗi viên đá mang một số nguyên từ 0 đến 1000. Chúng ta phải phân chia tất cả các viên đá thành từng cặp."
date: "2026-08-12T23:43:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102367
codeforces_index: "E"
codeforces_contest_name: "Fall 2019 ICPC-style Waterloo Local Contest"
rating: 0
weight: 102367
solve_time_s: 411
verified: true
draft: false
---

[CF 102367E - Ghép nối XOR](https://codeforces.com/problemset/problem/102367/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số chẵn các viên đá được lập chỉ mục và mỗi viên đá mang một số nguyên từ 0 đến 1000. Chúng ta phải phân chia tất cả các viên đá thành từng cặp. Một cặp chứa đá`i`Và`j`đóng góp`x[i] ^ x[j]`với tổng chi phí và chúng tôi muốn cả tổng chi phí tối thiểu có thể và số lượng các cặp khác nhau đạt được mức tối thiểu đó, modulo`10^9 + 7`. 

Các chỉ số quan trọng ngay cả khi hai viên đá có cùng giá trị. Ví dụ, bốn viên đá chứa`5 5 5 5`có ba cặp khác nhau, vì bản thân các viên đá là khác nhau mặc dù giá của tất cả các cặp đều bằng 0. 

Mẫu chính thức có`N = 6`, theo sau là`7 14 17 4 2 1`, và câu trả lời của nó là`31 3`. 

Giới hạn trên`N <= 74`đủ nhỏ cho các thuật toán xung quanh`O(N^2)`hoặc`O(N^2 log V)`, nhưng hoàn toàn loại trừ việc liệt kê tất cả các cặp. có`(N-1)!!`sự kết hợp hoàn hảo, vì vậy đối với`N = 74`một cuộc tìm kiếm vũ phu có`(73)!!`lá và mỗi lá yêu cầu 37 phép tính XOR. Số lượng đánh giá chi phí theo cặp chính xác là`37 * 73!!`, theo thứ tự của`10^55`. Giá trị ràng buộc`x[i] <= 1000`đặc biệt hữu ích vì chỉ có bit`0`bởi vì`9`có thể xảy ra, mang lại cho chúng ta sự phân rã nhị phân chỉ với 10 cấp độ. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Với hai viên đá, chẳng hạn như```
2
0 1000
```cặp đôi duy nhất có giá`1000`, vậy câu trả lời là`1000 1`. Việc triển khai giả định mọi nút đệ quy đều có hai nút con không trống ở đây có thể thất bại ở đây vì một bên của phần phân tách nhị phân có thể trống. 

Các giá trị trùng lặp yêu cầu đếm các đá được lập chỉ mục riêng biệt. Vì```
4
5 5 5 5
```câu trả lời là`0 3`, bởi vì cả ba cặp hoàn hảo đều có giá bằng 0. Chỉ đếm các sắp xếp giá trị riêng biệt sẽ trả về một giá trị không chính xác. 

Một trường hợp phức tạp hơn xảy ra khi cả hai phía của một bit được chia đều có kích thước lẻ. Đối với mẫu chính thức,```
6
7 14 17 4 2 1
```bit cao nhất tách biệt`{17}`từ`{7,14,4,2,1}`, nên cả hai vế đều lẻ. Chính xác một cặp phải vượt qua phần phân chia này, nhưng có nhiều lựa chọn khả thi cho cặp đó. Một lựa chọn tham lam chỉ dựa trên XOR ngay lập tức có thể bỏ lỡ việc ghép đôi tối ưu toàn cầu và thậm chí tệ hơn có thể bỏ lỡ một số cặp tối ưu khi đếm. 

Giá trị biên`1000`cũng đáng được quan tâm. Vì```
4
0 1000 511 512
```các cặp tối ưu là`(0, 511)`Và`(1000, 512)`, với chi phí`511`Và`488`, cho`999 1`. Xử lý các giá trị như thể chúng chỉ chiếm chín bit thấp nhất sẽ đặt`1000`trong phân vùng sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tạo ra mọi kết quả khớp hoàn hảo theo cách đệ quy. Chọn một viên đá hiện chưa được sử dụng, ghép nó với mọi viên đá chưa sử dụng khác và lặp lại. Điều này đúng vì mỗi cặp hoàn hảo đều có chính xác một đối tác cho viên đá được chọn đầu tiên, do đó, phép đệ quy cuối cùng sẽ ghé thăm mỗi cặp đúng một lần. Vấn đề là số lượng lá. Với`N`có đá ở đó`(N-1)!!`các cặp và mỗi cặp chứa`N/2`các cạnh. Tại`N = 74`, điều đó có nghĩa chính xác`37 * 73!!`tính toán chi phí theo cặp, vượt xa những gì mà bất kỳ việc triển khai nào cũng có thể thực hiện được. 

Cấu trúc hữu ích xuất phát từ việc xem xét bit khác nhau cao nhất của hai giá trị. Giả sử một nút trie nhị phân biểu thị các giá trị có các bit cao hơn giống hệt nhau và giả sử hai nút con của nó khác nhau ở một bit.`b`. Bất cứ cạnh nào giao nhau giữa hai con đều có bit`b`được thiết lập, vì vậy nó đóng góp ít nhất`2^b`. 

Lập luận trao đổi quan trọng là một kết quả khớp tối ưu có thể chứa nhiều nhất một giao điểm cạnh giữa hai phần tử con. Giả sử hai cặp giao nhau là`(a,b)`Và`(c,d)`, Ở đâu`a,c`thuộc về đứa trẻ bên trái và`b,d`thuộc về đúng đứa trẻ. Hai cạnh của chúng cùng nhau đóng góp ít nhất`2^(b+1)`chỉ từ bit hiện tại. Thay thế chúng bằng`(a,c)`Và`(b,d)`. Cả hai cạnh mới đều có bit hiện tại bằng 0 và mỗi cạnh có giá trị hoàn toàn bên dưới`2^b`, do đó chi phí kết hợp của họ thấp hơn rất nhiều`2^(b+1)`. Sự thay thế luôn tốt hơn. 

Điều này mang lại một cấu trúc đệ quy rất mạnh mẽ. Nếu cả hai con đều có kích thước chẵn thì giải pháp tối ưu không sử dụng cặp chéo. Nếu cả hai con đều có kích thước lẻ thì giải pháp tối ưu sử dụng chính xác một cặp chéo. Nếu toàn bộ nút có kích thước lẻ, thì hai nút con của nó có tính chẵn lẻ đối lập nhau và giải pháp tối ưu không sử dụng cặp giao nhau, để lại một viên đá không khớp trong nút con lẻ. 

Câu hỏi còn lại là cây con có kích thước lẻ phải trả về thông tin gì. Chúng ta không thể đơn giản trả về một số, bởi vì phụ huynh cần biết viên đá nào còn lại chưa khớp. Thay vào đó, chúng tôi tính toán cho mỗi viên đá`i`trong một cây con lẻ, chi phí tối thiểu để ghép tất cả các viên đá khác và để lại`i`chưa từng có, cùng với số cách để đạt được chi phí đó. Vì có tối đa 74 viên đá nên việc lưu trữ một trạng thái cho mỗi viên đá sẽ rẻ. 

Tại một nút có hai con lẻ, chúng tôi thử mọi viên đá có thể chưa từng có`i`từ đứa trẻ bên trái và`j`từ đứa trẻ bên phải. Hai viên đá đó trở thành cặp giao nhau duy nhất, trong khi những viên đá còn lại được kết hợp tối ưu bên trong các viên đá con tương ứng của chúng. Đây là nơi duy nhất cần chuyển đổi bậc hai. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O((N/2) * (N-1)!!)`|`O(N)`| Quá chậm | 
| Tối ưu |`O(N^2 log V)`|`O(N log V)`| Đã chấp nhận | 

Đây`V <= 1000`, Vì thế`log V <= 10`. 

## Hướng dẫn thuật toán 

1. Tính toán trước`ways[k]`, số cặp hoàn hảo của`k`đá được lập chỉ mục. Thậm chí`k`, sự tái diễn là`ways[k] = (k - 1) * ways[k - 2]`, với`ways[0] = 1`. Điều này cũng đưa ra số cách để ghép tất cả trừ một viên đá trong một bộ có kích thước lẻ, bởi vì`ways[k - 1]`là số lượng cần thiết. 
2. Xử lý đệ quy một tập hợp các chỉ số đá từ bit quan trọng nhất đến bit ít quan trọng nhất. Chia các chỉ số theo bit hiện tại. Nếu tất cả các giá trị trong tập hợp có cùng một bit thì chỉ có một con không trống, vì vậy chúng ta chỉ cần tiếp tục đến bit tiếp theo. 
3. Đối với một nút có kích thước chẵn có hai nút con đều chẵn, hãy kết hợp các kết quả khớp tối ưu độc lập của chúng. Không cần cặp chéo nào, bởi vì hai cặp chéo sẽ tệ hơn rất nhiều và khả thi là không có cặp chéo nào. Chi phí là tổng của hai chi phí con và số cách là tích của chúng. 
4. Đối với một nút chẵn có hai nút con đều là số lẻ, cần có chính xác một cặp giao nhau. Đối với mỗi viên đá`i`ở đứa trẻ bên trái và mọi hòn đá`j`ở con bên phải, sử dụng trạng thái cây con lẻ của chúng và tính toán`dp_left[i] + dp_right[j] + (x[i] ^ x[j])`. 

Giữ giá trị nhỏ nhất và tính tổng số lượng bất cứ khi nào một lựa chọn khác đạt đến mức tối thiểu tương tự. Mỗi cặp kết quả đều có một cặp chéo duy nhất, do đó không có sự tính trùng lặp. 
5. Đối với một nút có kích thước lẻ, có chính xác một nút con là số lẻ và nút còn lại là số chẵn. Giải pháp tối ưu không có cặp giao nhau. Do đó, viên đá vô song phải đến từ đứa trẻ lẻ loi. Cộng chi phí khớp hoàn toàn của con chẵn với mọi trạng thái đá chưa từng có của con lẻ và nhân số lượng tương ứng. 
6. Khi đệ quy đạt đến bit cuối cùng, tất cả các giá trị trong tập hợp hiện tại đều bằng nhau. Mỗi cặp đều có XOR bằng 0. Nếu tập hợp có kích thước chẵn`m`, chi phí trả lại bằng 0 và`ways[m]`các cặp đôi. Nếu nó có kích thước lẻ, đối với mỗi viên đá có thể không khớp được thì chi phí trả lại bằng 0 và`ways[m - 1]`các cặp đôi. 
7. Bắt đầu đệ quy với tất cả các chỉ số và bit`9`, vì 1000 nhỏ hơn`2^10`. Chi phí ở trạng thái chẵn được trả về là tổng XOR tối thiểu và số lượng của nó là số cặp tối thiểu theo modulo`10^9 + 7`. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái đệ quy đều chứa giải pháp tối ưu trong điều kiện chính xác được biểu thị bởi trạng thái đó. Trạng thái chẵn đại diện cho một cặp đá hoàn chỉnh của nó, trong khi trạng thái lẻ được lập chỉ mục bởi`i`đại diện cho một cặp của mỗi viên đá ngoại trừ`i`. 

Đối số trao đổi chứng minh rằng một kết quả khớp tối ưu không bao giờ cần hai cạnh cắt ngang hai nút con của nút trie hiện tại. Do đó, một nút chẵn hoặc có 0 cạnh giao nhau khi cả hai nút con đều chẵn hoặc có chính xác một cạnh giao nhau khi cả hai đều là số lẻ. Một nút lẻ không thể sử dụng một cạnh giao nhau vì các chẵn lẻ con của nó khác nhau và việc sử dụng hai hoặc nhiều hơn sẽ thực sự tệ hơn so với cấu trúc giao nhau bằng 0 khả thi. Do đó, mọi kết quả khớp tối ưu đều có chính xác một trong các dạng đệ quy được xem xét bởi quá trình chuyển đổi. 

Tại một lá, tất cả các giá trị XOR đều bằng 0, do đó các trường hợp cơ sở sẽ tính mọi cặp tối ưu. Tại các nút bên trong, các quá trình chuyển đổi liệt kê mọi kết quả phù hợp tối ưu về mặt cấu trúc có thể có chính xác một lần. Về mặt quy nạp, chi phí và số lượng tối thiểu được trả về là chính xác cho thư mục gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
MAX_BIT = 9

def solve_case(a):
    n = len(a)

    # ways[k] = number of perfect pairings of k indexed elements,
    # for even k.
    ways = [0] * (n + 1)
    ways[0] = 1
    for k in range(2, n + 1, 2):
        ways[k] = ways[k - 2] * (k - 1) % MOD

    def dfs(ids, bit):
        m = len(ids)

        # All remaining bits are equal, so every XOR is zero.
        if bit < 0:
            if m & 1:
                cnt = ways[m - 1]
                return [(0, cnt) for _ in ids]
            return (0, ways[m])

        mask = 1 << bit
        left = []
        right = []

        for i in ids:
            if a[i] & mask:
                right.append(i)
            else:
                left.append(i)

        # The current bit does not distinguish these values.
        if not left or not right:
            return dfs(ids, bit - 1)

        dl = dfs(left, bit - 1)
        dr = dfs(right, bit - 1)

        nl = len(left)
        nr = len(right)

        if m & 1:
            # Exactly one child is odd and the other is even.
            if nl & 1:
                odd_dp = dl
                even_cost, even_count = dr
            else:
                odd_dp = dr
                even_cost, even_count = dl

            result = []
            for cost, count in odd_dp:
                result.append(
                    (cost + even_cost, count * even_count % MOD)
                )
            return result

        # The current node is even.
        if (nl & 1) == 0:
            # Both children are even, so no crossing pair is needed.
            left_cost, left_count = dl
            right_cost, right_count = dr
            return (
                left_cost + right_cost,
                left_count * right_count % MOD
            )

        # Both children are odd, so exactly one crossing pair is needed.
        best_cost = None
        best_count = 0

        for i, (cost_i, count_i) in zip(left, dl):
            for j, (cost_j, count_j) in zip(right, dr):
                cand_cost = cost_i + cost_j + (a[i] ^ a[j])
                cand_count = count_i * count_j % MOD

                if best_cost is None or cand_cost < best_cost:
                    best_cost = cand_cost
                    best_count = cand_count
                elif cand_cost == best_cost:
                    best_count = (best_count + cand_count) % MOD

        return best_cost, best_count

    return dfs(list(range(n)), MAX_BIT)

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    cost, count = solve_case(a)
    print(cost, count)

if __name__ == "__main__":
    solve()
```các`ways`mảng xử lý các trường hợp cơ sở tổ hợp. Ví dụ,`ways[4] = 3`, tương ứng với ba cách chia bốn viên đá được lập chỉ mục thành hai cặp. Giữ các giá trị này theo modulo`10^9 + 7`là đủ vì mỗi lần đếm sau này đều được hình thành từ tổng và tích của các lần đếm này. 

Hàm đệ quy trả về hai loại trạng thái khác nhau. Một cây con có kích thước chẵn sẽ trả về một`(cost, count)`ghép đôi vì tất cả các viên đá của nó đều giống nhau. Một cây con có kích thước lẻ trả về một`(cost, count)`cặp cho mọi chỉ số chưa từng có có thể. Sự khác biệt này cho phép cha mẹ quyết định hai viên đá nào sẽ tạo thành cặp giao nhau duy nhất của nó. 

Phân vùng sử dụng`1 << bit`, với chút`9`như bit bắt đầu. Từ`1000 < 1024`, không có bit cao hơn có thể xảy ra. Cuộc gọi đệ quy với`bit - 1`cuối cùng đạt tới`-1`, trong đó tất cả các giá trị còn lại đều giống hệt nhau đối với mọi bit có thể có. 

Vòng lặp bậc hai chỉ được thực hiện khi cả hai phần tử con đều lẻ. Mỗi cặp chéo ứng cử viên sử dụng một trạng thái từ mỗi đứa trẻ. Phép nhân các số đếm kết hợp các lựa chọn tối ưu độc lập từ hai cây con, trong khi tổng cuối cùng tích lũy các lựa chọn khác nhau của cạnh giao nhau. Số lượng được giảm modulo`MOD`sau mỗi lần nhân hoặc cộng. 

Số nguyên Python không bị tràn nên việc tính toán chi phí không cần loại số nguyên đặc biệt. Chi phí thực tế tối đa nhiều nhất là`37 * 1023`, dù sao thì nó cũng nhỏ thôi. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
6
7 14 17 4 2 1
```Tại bit 4, các giá trị được chia thành`{7,14,4,2,1}`Và`{17}`. Cả hai nhóm đều lẻ nên phải chọn đúng một cặp chéo. Nhóm đầu tiên cần một viên đá chưa từng có và bảng hiển thị chi phí tối ưu để kết hợp bốn viên đá còn lại cho mỗi chỉ số có thể chưa từng có. 

| Chưa từng có trong`{7,14,4,2,1}`| Chi phí nội bộ | Số cách | XOR với`17`| Tổng cộng | 
| --- | --- | --- | --- | --- | 
|`7`| 13 | 1 | 22 | 35 | 
|`14`| 6 | 1 | 31 | 37 | 
|`4`| 12 | 1 | 21 | 33 | 
|`2`| 14 | 1 | 19 | 33 | 
|`1`| 15 | 3 | 16 | 31 | 

Mức tối thiểu có được bằng cách để lại`1`chưa từng có trong nhóm thấp hơn và ghép nối nó với`17`. Có ba cách tối ưu để kết hợp`{7,14,4,2}`với chi phí 15, vì vậy câu trả lời cuối cùng là`31 3`. Ba khả năng này là khác nhau vì các viên đá được lập chỉ mục. 

Đối với ví dụ thứ hai, hãy xem xét```
4
0 1 2 3
```Tại bit 1, hai đứa trẻ là`{0,1}`Và`{2,3}`, cả hai đều chẵn. Do đó, cấu trúc tối ưu không chứa cặp giao nhau ở bit này. 

| Chút | Nhóm trái | Đúng nhóm | Trạng thái trái | Đúng trạng thái | Kết quả tổng hợp | 
| --- | --- | --- | --- | --- | --- | 
|`1`|`0,1`|`2,3`| trị giá`1`, đếm`1`| trị giá`1`, đếm`1`| trị giá`2`, đếm`1`| 
|`0`| bên trong`{0,1}`| bên trong`{2,3}`| đôi`0,1`| đôi`2,3`| không thay đổi | 

Sự ghép đôi tối ưu duy nhất là`(0,1)`Và`(2,3)`, với chi phí XOR`1`Và`1`. Câu trả lời là`2 1`. Ví dụ này thể hiện sự chuyển đổi chẵn-chẵn, trong đó hai nửa có thể được giải độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N^2 log V)`| Tại mỗi bit, các cặp phần tử chỉ được xem xét khi chúng nằm ở các nút con đối diện của cùng một nút trie. Trên một cấp độ có`O(N^2)`những cặp như vậy. | 
| Không gian |`O(N log V)`| Mỗi phần tử tham gia vào một trạng thái đệ quy trên mỗi bit và mỗi trạng thái lẻ lưu trữ một cặp giá trị. | 

Đây`V <= 1000`, Vì thế`log V`nhiều nhất là 10. Với`N <= 74`, hệ số bậc hai chỉ là vài nghìn phép tính trên mỗi bit, cộng với chi phí đệ quy nhỏ. Thuật toán thoải mái trong giới hạn 3 giây và 256 MB. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp được gửi có sẵn dưới dạng`solution.py`và phơi bày`solve_case(a)`.```python
import sys
import io

from solution import solve_case

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]
    cost, count = solve_case(a)
    return f"{cost} {count}"

# Provided sample
assert run("""
6
7 14 17 4 2 1
""") == "31 3", "sample 1"

# Minimum-size input
assert run("""
2
0 1000
""") == "1000 1", "minimum size"

# All values equal, with four indexed stones
assert run("""
4
5 5 5 5
""") == "0 3", "all equal"

# Boundary values and a highest-bit split
assert run("""
4
0 1000 511 512
""") == "999 1", "boundary values"

# A slightly larger case where every optimal pair has XOR 1
assert run("""
6
0 1 2 3 4 5
""") == "3 1", "adjacent XOR pairs"

# Maximum-size input. For equal values the minimum cost is zero,
# and every perfect pairing is optimal.
MOD = 10**9 + 7
ways = [0] * 75
ways[0] = 1
for k in range(2, 75, 2):
    ways[k] = ways[k - 2] * (k - 1) % MOD

max_input = "74\n" + " ".join(["1000"] * 74) + "\n"
assert run(max_input) == f"0 {ways[74]}", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 1000`|`1000 1`| tối thiểu`N`và sự phân chia với một mặt đệ quy trống | 
|`4 / 5 5 5 5`|`0 3`| Các bản sao được lập chỉ mục và đếm tổ hợp | 
|`4 / 0 1000 511 512`|`999 1`| Giá trị cao nhất được phép và hành vi ranh giới bit | 
|`6 / 0 1 2 3 4 5`|`3 1`| Nhiều cấp độ thử nghiệm với cấu trúc tối ưu độc đáo | 
|`74 / 1000 ... 1000`|`0 73!! mod MOD`| Tối đa`N`và số lượng cặp đôi tối ưu lớn nhất có thể | 

## Vỏ cạnh 

Đối với trường hợp hai viên đá```
2
0 1000
```phần gốc cuối cùng chỉ chứa hai viên đá đó. Các chuyển đổi lẻ/chẵn giảm xuống còn một lá chẵn chứa hai giá trị đệ quy tương ứng với hiện tại chỉ sau khi tất cả các bit phân biệt đã được xử lý. Hai viên đá phải tạo thành một cặp, đưa ra chi phí`0 ^ 1000 = 1000`và đúng một cặp. Đầu ra là`1000 1`. 

Đối với các giá trị trùng lặp,```
4
5 5 5 5
```cả bốn chỉ số đều đạt đến cùng một lá. Chiếc lá có bốn viên đá nên giá của nó bằng 0 và`ways[4] = 3`. Thuật toán không bao giờ hợp nhất các viên đá có giá trị bằng nhau vào một đối tượng, vì vậy cả ba cặp được lập chỉ mục đều được tính. Đầu ra là`0 3`. 

Đối với mẫu chính thức,```
6
7 14 17 4 2 1
```bit cao nhất tách một viên đá khỏi năm viên đá. Cả hai bên đều lẻ, buộc đúng một cặp chéo. Trạng thái động của mặt năm viên đá giữ bản sắc của viên đá chưa từng có của nó và việc thử từng chỉ số chưa từng có sẽ cho thấy rằng`1`ghép nối với`17`đưa ra tổng số tối thiểu của`31`. Có ba cách kết hợp nội tại tối ưu cho bốn viên đá còn lại, tạo ra`31 3`. 

Đối với trường hợp biên,```
4
0 1000 511 512
```sự ghép đôi tốt nhất là`(0,511)`Và`(1000,512)`. Giá trị XOR của chúng là`511`Và`488`, tương ứng, vậy tổng số là`999`. Các cặp khác có thể có tổng số lớn hơn. Quá trình thử bắt đầu ở bit 9, trong đó`1000`Và`512`đang ở mức cao trong khi`0`Và`511`ở phía thấp, do đó thuật toán xử lý ranh giới mà không có bất kỳ trường hợp số đặc biệt nào. Đầu ra là`999 1`. 

Để có đầu vào tối đa,```
74
1000 1000 ... 1000
```mọi XOR đều bằng 0. Quá trình đệ quy đạt đến một lá duy nhất chứa tất cả 74 viên đá được đánh chỉ mục. Chi phí bằng 0 và mọi sự kết hợp hoàn hảo đều là tối ưu. Số đếm chính xác`73!!`, modulo được tính toán`10^9 + 7`bởi`ways`tái phát. Trường hợp này thực hiện cả kích thước trạng thái đệ quy tối đa và logic đếm mô-đun.
