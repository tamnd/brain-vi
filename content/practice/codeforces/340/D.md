---
title: "CF 340D - Đồ thị sắp xếp bong bóng"
description: "cho một hoán vị của bài toán Hiểu Chúng ta được cho một hoán vị của các số từ 1 đến n. các số từ 1 đến n. Hãy tưởng tượng Hãy tưởng tượng việc chạy sắp xếp bong bóng trên hoán vị này đang chạy sắp xếp bong bóng thông thường trên này."
date: "2026-06-06T17:17:43+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 340
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 198 (Div. 2)"
rating: 1500
weight: 340
solve_time_s: 125
verified: true
draft: false
---

[CF 340D - Biểu đồ sắp xếp nổi bọt](https://codeforces.com/problemset/problem/340/D) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
đưa ra một hoán vị của ## Hiểu vấn đề 

Chúng ta được cho một hoán vị các số từ`1`ĐẾN`n`. 

các số từ 1 đến n. Hãy tưởng tượng Hãy tưởng tượng việc chạy sắp xếp bong bóng trên hoán vị này đang chạy sắp xếp bong bóng thông thường trên này. Mỗi lần sắp xếp bong bóng sẽ tìm thấy hai phần tử liền kề trong hoán vị. Mỗi khi sắp xếp bong bóng thực hiện hoán đổi giữa hai thứ tự sai liền kề và hoán đổi chúng, chúng ta thêm một cạnh vô hướng giữa các phần tử, chúng ta thêm một cạnh vô hướng giữa hai giá trị được hoán đổi. Các giá trị biểu đồ đang được hoán đổi. 

Sau khi sắp xếp nổi bong bóng kết thúc, các đỉnh của chúng ta là các giá trị`1...n`. 

Sau khi sắp xếp nổi bọt, thu được một đồ thị có các đỉnh là các số từ 1 đến n. Nhiệm vụ kết thúc, chúng ta thu được một biểu đồ có các cạnh ghi lại mọi cạnh không phải để mô phỏng chính biểu đồ đó mà để tìm sự hoán đổi đã xảy ra. Nhiệm vụ là tìm kích thước lớn nhất của tập độc lập tối đa của nó, nghĩa là tập độc lập lớn nhất trong biểu đồ đó, nghĩa là lớn nhất trong số các đỉnh không có cạnh giữa bất kỳ tập hợp cặp đỉnh nào không có cạnh nào. 

Thử thách đầu tiên giữa bất kỳ cặp nào trong số họ. 

Đầu tiên là hiểu cách sắp xếp bong bóng trên đồ thị thực sự tạo ra điều gì. Một thách thức trực tiếp là hiểu được mô phỏng đồ thị nào của sắp xếp bong bóng có thể thực hiện việc sắp xếp bong bóng thực sự tạo ra. Mô phỏng trực tiếp việc sắp xếp bong bóng lên tới O(n2) giao dịch hoán đổi. Với n như có thể yêu cầu`O(n² large as 100000, that would be roughly 10¹⁰ operations in)`trao đổi. Với`n`lên đến` the worst case, which is completely impossible within a100000`, đó sẽ là giới hạn thời gian khoảng một giây. 

Sự hạn chế đó`10^10`các phép toán trong trường hợp xấu nhất mà đầu vào là một hoán vị là cực kỳ quan trọng. Mọi thứ đều hoàn toàn không thể xảy ra trong vòng một giây. 

Giá trị ràng buộc mạnh xuất hiện chính xác một lần, điều này tạo ra gợi ý rằng biểu đồ có một số cấu trúc ẩn. cấu trúc mạnh mẽ trong các giao dịch hoán đổi. 

Điểm chung Chúng ta cần một thuật toán xung quanh`O(n log n)`. 

Một sai lầm khi nghĩ rằng sai lầm phổ biến là mọi sự đảo ngược đều trở thành suy nghĩ rằng mọi sự đảo ngược đều trở thành một cạnh. Coi như:```text
3
3 1 2
```Các nghịch đảo là (3, một cạnh. Hãy xem xét:```text
3
3 1 2
```Những sự đảo ngược là`(3,1)`Và`(3,1) and (3,2). Bubble sort indeed2)`, và thực sự cả hai đều tạo ra các cạnh (3,1) và (3,2), do đó trở thành các cạnh. Nhưng đây là ví dụ này không đúng vì không bộc lộ được sự khác biệt. 

Một ví dụ rõ ràng hơn là:``` of inversions in general,text
4
4 1 3  it is true because bubble sort eventually2
```Sự đảo ngược hoán đổi các cặp đó một cách trực tiếp. 

Một cặp hấp dẫn khác (4,2) tồn tại, nhưng sai lầm này là việc xây dựng biểu đồ một cách rõ ràng các giá trị không bao giờ được hoán đổi trực tiếp. Sắp xếp bong bóng chỉ hoán đổi các phần tử liền kề và sau đó chạy thuật toán tập hợp độc lập tối đa. Bộ độc lập tối đa. Việc coi mọi nghịch đảo là một cạnh là NP-hard trên các biểu đồ tùy ý, do đó sẽ xây dựng biểu đồ sai. 

Một cách tiếp cận dễ dàng khác như vậy sẽ ngay lập tức có vẻ sai lầm là xây dựng biểu đồ một cách rõ ràng và sau đó vô vọng. Quan sát quan trọng là biểu đồ này giải được bài toán tập hợp độc lập cực đại trên nó. Tổng hợp độc lập tối đa rất đặc biệt. 

Coi như:```text
4
 is NP-hard, so if the graph were arbitrary this4 1 2 3
```Bubble sẽ là vô vọng. Cấu trúc ẩn của việc sắp xếp tạo ra các cạnh`(4,1)`,`(4,2)`,`(4,3)`. đồ thị là toàn bộ Tập độc lập tối đa có kích thước`3`, cụ thể là điểm của vấn đề. 

Một sự tinh tế cuối cùng`{1,2,3}`. Trường hợp biểu đồ chung là một hoán vị đã được sắp xếp: 

thuật toán sẽ bỏ lỡ sự thật là biểu đồ này xuất hiện```text
5
1 2 3 4 5
 from a very structured process.

A useful edge case is an```Không có sự hoán đổi nào xảy ra, vì vậy biểu đồ đã sắp xếp hoán vị:```text
5
1 2 3 4  has no edges. The answer is 55
```Không có sự hoán đổi nào xảy ra nên đồ thị không có cạnh. Tập độc lập lớn nhất chứa mọi đỉnh vì mọi đỉnh đều có thể thuộc về tập độc lập. Bất kỳ giải pháp nào giả định, và câu trả lời là`5`. 

Ở thái cực ngược lại:```text
5
 at least one edge exists will fail here.

## Approaches

5 4 3 2 1
```Mỗi cặp Ý tưởng trực tiếp nhất là mô phỏng các dạng bong bóng đảo ngược. Sắp xếp bong bóng chính xác như sắp xếp được mô tả cuối cùng sẽ hoán đổi chính xác từng cặp đảo ngược. Cứ hai lần một lần, tạo ra một biểu đồ hoàn chỉnh. Kích thước tập hợp độc lập tối đa là`1`. 

Hiểu các giá trị liền kề được hoán đổi, chúng tôi thêm một cạnh giữa chúng. Sau khi thu thập lý do tại sao hai thái cực này tương ứng với tất cả các cạnh trống, chúng ta có thể thử tính toán các biểu đồ hoàn chỉnh và dẫn trực tiếp đến cái nhìn sâu sắc chính. 

## Đạt đến tập độc lập tối đa. 

Sự mô phỏng 

Việc giải thích bạo lực rất đơn giản. Bản thân mô phỏng sắp xếp bong bóng đã có chi phí O(n2) chính xác như được mô tả, thêm mọi cạnh, trường hợp xấu nhất vì sắp xếp bong bóng có thể thực hiện Θ xây dựng biểu đồ kết quả và sau đó tính toán các giao dịch hoán đổi (n2) của nó. Với n = 100000 tập độc lập tối đa. 

Ngay cả việc xây dựng biểu đồ này cũng vượt xa khả thi. 

Cách tệ hơn nữa là quá chậm. Sắp xếp bong bóng thực hiện, việc tìm tập độc lập tối đa trong biểu đồ chung là tính toán`O(n²)`so sánh và khó khăn. Vì vậy, lực lượng vũ phu đều thất bại vì đồ thị hoán đổi. Vì`n =  construction is too expensive and because the resulting optimization problem appears in100000`, số lượng hoạt động có thể kéo dài được. 

Quan sát quan trọng là đồ thị do bong bóng tạo ra vượt quá giới hạn thực tế. 

Quan sát tiếp theo là sự sắp xếp rất đặc biệt quan trọng: in. 

Xét hai giá trị x và y với x > y. 

sắp xếp bong bóng, hai giá trị được hoán đổi Nếu x xuất hiện trước y khi và chỉ khi chúng tạo thành một sự đảo ngược trong hoán vị ban đầu. 

Thực hiện hoán vị, sau đó chúng tạo thành một sự đảo ngược. hai giá trị`x > y`. Nếu như`x`Trong quá trình sắp xếp bong bóng, x cuối cùng phải di chuyển qua y. Vì xuất hiện trước đó`y`, chúng tạo thành một sự đảo ngược. Hoán đổi bong bóng liền kề nhau, có đúng một thời điểm khi x và y trở nên liền kề và được hoán đổi sắp xếp cuối cùng sẽ di chuyển`x`Phải. Tại thời điểm đó một cạnh (x,y)ward và`y`sang trái cho đến khi chúng liền kề nhau, at được thêm vào. 

Nếu x xuất hiện sau y ban đầu thì chúng đã được đổi chỗ đúng một lần. Nếu chúng theo đúng thứ tự tương đối và không bao giờ hoán đổi. 

Đây không phải là sự đảo ngược ban đầu, thứ tự tương đối của chúng không bao giờ thay đổi có nghĩa là: 

Hai đỉnh được kết nối và chúng không bao giờ bị hoán đổi. 

Điều này có nghĩa là nếu và chỉ nếu chúng tạo thành một sự đảo ngược trong hoán vị ban đầu. 

đồ thị chứa chính xác một cạnh cho mỗi cặp nghịch đảo. 

Bây giờ giả sử chúng ta chọn Bây giờ hãy viết lại điều kiện nghịch đảo theo các đỉnh tạo thành một tập hợp độc lập. Không có vị trí. Cho phép`pos[v]`là vị trí của giá trị`v`trong hoán vị. 

Đối với các giá trị`u < two selected vertices may form an inversion. Thus, among the selected v`, chúng liền kề nhau trong các giá trị đồ thị, thứ tự của chúng trong hoán vị phải chính xác khi```text
pos[u] > pos[v]
`` increasing.

That is exactly the definition of an`

In other words, the graph is a increasing subsequence.

Conversely, every increasing subsequence contains no permutation graph.

An independent set cannot inversion pair, so its vertices form an independent set.

We have established a bijection:

Maximum independent set contain two vertices connected by an edge. Therefore, if we choose values size = Longest Increasing Subsequence length.

Once

```văn bản 
x1 < x2 < ... < x vấn đề được giảm xuống còn LIS, tuần```

inside an independent set, we must have

```văn bản 
có thể giải nó trong O(n log n) bằng cách sử dụng tiêu chuẩnpos[x1] < pos[x2] < ... < pos[xk]```

Otherwise patience-sorting technique with binary search.

| Approach | Time Complexity some pair would form an inversion and create an edge.

So | Space Complexity | Verdict |
|---|---|---|---|
| Brute Force an independent set corresponds exactly to a subsequence of values whose positions are bubble-sort simulation + graph reasoning increasing.

If we list the positions in | O(n²) or worse | O(n² value order:

```văn bản 
pos[1], pos[2], ...,) | Quá chậm | 
| Vị trí giảm LIS[n]```

then the | O(n log n) | O(n) | Accepted |

## Algorithm answer becomes the length of the Longest Increasing Subsequence Walkthrough

1. Read the permutation.

2. Maintain an of this sequence.

Computing LIS in ` array `tails`.

   `tails[i]`O(n log n)` is a standard technique will store the smallest possible ending value of and easily fits the constraints.

| Approach | Time Complexity | an increasing subsequence of length `i + 1`.

3. Process Space Complexity | Verdict |
|---|---|---|---|
| Brute Force | the permutation from left to right.

4. O(n²) or worse | O(n² For the current value `x`,) | Too slow |
| Optimal (LIS) | O(n log n) | find the first position in `tails` whose O(n) | Accepted |

## Algorithm Walkthrough value is greater than or equal to `x`.

   This

1. Read the permutation.

2. Build an array ` position can be found with binary search.

5. If nopos` where `pos[v]` stores the position such position exists, append `x` to `tails`.

   This of value `v` in the permutation.

3. Construct the sequence:

   means we have found a longer increasing subsequence than any ```văn bản 
pos[1], pos[2], đã thấy trước đây. 

6. Ngược lại, thay thế ..., pos[n]```

   This sequence describes position with `x`.

   A smaller ending value is where each value appears.

4. Compute the Long always better because it leaves more room forest Increasing Subsequence of this sequence using the standard patience future elements to extend the subsequence.

7. After-sorting method.

5. Maintain an all values are processed, the array `tails`.

   `tails[len-1]` stores length of `tails` equals the length of the the smallest possible ending value of Longest Increasing Subsequence.

8. Output that length.

### Why it works an increasing subsequence of length `len`.

6. Process the

An edge exists exactly between pairs of position sequence from left to right.

   For each position value ` values that form inversions in the original permutation.

x`, find the first element in `tails` thatAn independent set cannot contain two adjacent vertices of the graph is greater than or equal to `x`.

7. If. Therefore it cannot contain an inversion pair. The no such element exists, append `x` to `tails`.

   This selected values must appear in increasing order within the permutation.

Convers means we found a longer increasing subsequence.

8. Otherwise replace that element with `x`.

ely, every increasing subsequence contains no inversion pair. Since   A smaller tail is always no inversion pair exists, no edge exists between any two preferable because it leaves more room for future extensions.

9. After processing all elements, the chosen vertices, making it an independent set.

Thus independent sets and increasing subsequences length of `tails` is the LIS length.

10. Output are exactly the same objects viewed that length.

### Why it works

Every graph from two different perspectives. Maximizing one is edge corresponds to an inversion pair.

Take any independent set and equivalent to maximizing the other.

The patience-sorting LIS algorithm is sort its vertices by value:

```văn bản 
v đã biết tính toán độ dài chuỗi con tăng tối đa vì1 < v2 < ... < vk```

Since no pair `tails[k]` always stores the smallest achievable ending value is connected, none of these pairs forms of a subsequence of length `k + 1`. Binary an inversion. Hence

```văn bản 
pos[v1] < pos[v2] < ... < search duy trì sự bất biến này sau mỗi lần cập nhật. 

## Giải pháp Python``` pos[vk]
```Các vị trí tạo thành một chuỗi con tăng dần 
hệ thống nhập khẩu 
từ bisect nhập bisect_left 

đầu vào =. 

Ngược lại, giả sử giá trị```text
 sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int,v1 < v2 < ... < vk
```có đầu vào().split())) ngày càng tăng 

đuôi = [] 

cho x trong a: 
pos = vị trí bisect_left. Khi đó không có cặp nào tạo thành sự đảo ngược, do đó không tồn tại cạnh nào giữa bất kỳ cặp nào.(Đuôi, x) 

if pos == len(tails): 
tails.append(x) 
Họ tạo thành một tập hợp độc lập. 

Vì vậy, có một cái khác: 
đuôi[pos] = x 

in(len(đuôi)) 

nếu __name__ == "__main__": 
-sự tương ứng một-một giữa các tập độc lập và các dãy con tăng dần của```text
pos[1], solve()
```Giải pháp không bao giờ pos[2], ..., pos[n].```

The maximum independent set size is constructs the graph.

The crucial observation is the equivalence between exactly the LIS length of that sequence.

## Python Solution

```trăn 
nhập các tập đồ thị độc lập và các chuỗi con tăng dần. Một khi hệ thống giảm đó 
từ bisect nhập bisect_left 

input = được thiết lập, phần còn lại là LIS sys.stdin.readline tiêu chuẩn 

giải quyết chắc chắn(): 
n = int(đầu vào()) 
a = list(map(int, input().split())) 

tính toán. 

Mảng`tails`được duy trì theo thứ tự sắp xếp. vị trí = [0] * (n + 1) 
sử dụng`bisect_left`tìm thấy for i, v trong enumerate(a): 
pos[v] = tôi 

đuôi = [] 

vị trí đầu tiên nơi giá trị hiện tại có thể thay thế đuôi hiện có. Vì đối với v trong phạm vi (1, n + 1): 
x = vị trí[v] 
đầu vào là một hoán vị, tất cả các giá trị đều khác biệt, nhưng idx = bisect_left(tails, x) 

if idx == len(đuôi):`bisect_left`vẫn là lựa chọn chính xác cho tails.append(x) 
khác: 
đuôi[idx] = x 

thuật toán LIS tăng chặt tiêu chuẩn. 

Khi `pos == print(len(tails)) 

nếu __name__ == "__main__": 
giải len(tails)`, giá trị hiện tại kéo dài chuỗi con dài nhất được tìm thấy cho đến nay()```

The first part builds the inverse. Otherwise we replace an existing tail. permutation `pos`. Instead of asking which value sits at each position, The replacement does not reduce any achievable subsequence length, it we ask where each value sits.

The sequence `pos[1], pos[ only improves future extension opportunities by making the ending2], ..., pos[n]` is exactly value smaller.

No overflow concerns the object whose LIS we need.

The ` exist because all values are at most 100000.

## Worked Examples

###tails` array implements the standard ` Example 1

Input:

```văn bản 
3 
3 1 2 
O(n log n)` LIS algorithm. Using `chia đôi_trái````

| Current value | is the correct choice because LIS requires strictly increasing values tails before | Position. Replacing the first tail | tails after |
|---|---|---|---|
| 3 | [] greater than or equal to the current value | 0 | [3] |
| 1 | [ preserves the optimal invariant that3] | 0 | [1] |
| 2 | [ each subsequence length keeps the smallest possible ending position1] | 1 | [1, 2] |

Final.

No integer-overflow issues exist because all values are at LIS length = 2.

Output:

```văn bản 
nhất`100000`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```text
3
3 2
```Điều này tương ứng với tập độc lập {1 2```

Positions:

| Value | Position1,2}. The graph contains edges (3,1) and (3,2), |
|---|---|
| 1 | 1 |
|  so vertex 3 cannot coexist with either2 | 2 |
| 3 | 0 |

Position sequence:

```của họ. 

### Ví dụ 2 

đầu vào:```text
5
1 2 3 text
[1, 2, 0]
```Dấu vết LIS: 

| Hiện tại x | đuôi trước | đuôi sau | 
|---|4 5```

| Current value | tails before | Position | tails after |
|---|---|---|---|
| 1 | [] | 0 |---|---|
| 1 | [] | [1] |
| [1] |
| 2 | [1] | 1 | [ 2 | [1] | [1, 2] |
| 0 | [1, 2] |
| 3 | [1, 2] | 2 | [1, 2] | [0, 2] |

Final LIS1, 2, 3] |
| 4 | [1, 2, length = 2.

Answer:

```văn bản 
2```

This corresponds 3] | 3 | [1, 2, 3, 4] |
| 5 | [1, 2, 3, 4] | 4 | [ to choosing values `{1,2}`. Their1, 2, 3, 4, 5] |

Final LIS positions are increasing, so no inversion exists between them.

### Example 2

Input:

```văn bản 
5 
5 4 3 2 1```

 length = 5.

Output:

```văn bản 
5```

No inversPositions:

| Value | Position |
|---|---|
| 1 | 4 |
ions exist, so bubble sort performs no swaps and the graph has no edges. Every| 2 | 3 |
| 3 | 2 |
| 4 | 1 |
|  vertex belongs to the maximum independent set.

## Complexity Analysis

| Measure | Complexity | Explanation |
|---5 | 0 |

Position sequence:

```văn bản 
[4, 3, |---|---| 
| Thời gian | O(n log n) | Một2, 1, 0]```

LIS trace:

| Current binary search for each of n elements |
| Space | x | tails before | tails after |
|---|---|---|
|  O(n) | The `tails` array may contain up to n values |

With n4 | [] | [4] |
| 3 | [4] | [3] |
| ≤ 100000, O(n log n) performs roughly a 2 | [3] | [2] |
| 1 | [2] | few million primitive operations, which easily fits within the time [1] |
| 0 | [1] | [0] |

Final LIS limit. The memory usage is linear and well below length = 1.

Answer:

```Văn bản 1```

This the 256 MB limit.

## Test Cases

```trăn 
# thể hiện trường hợp đồ thị hoàn chỉnh. Mỗi cặp trợ giúp: chạy giải pháp trên chuỗi đầu vào, trả về chuỗi đầu ra 
hệ thống nhập khẩu 
là một nghịch đảo nên chỉ có thể chọn một đỉnhimport io 
từ bisect nhập bisect_left 

chắc chắn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|--- run(inp: str) -> str: 
sys|---|---| 
| Thời gian | O(n log n) | One.stdin = io.StringIO(inp) 

n = int(sys.stdin.readline()) 
a = danh sách chèn LIS cho mỗi giá trị | 
|(map(int, sys.stdin.readline().split())) 

đuôi = [] 

Không gian | O(n) | Mảng vị trí và cấu trúc LIS | 

cho x trong a: 
pos = bisect_left(đuôi,Với`n = 100000`, `nx) 
if pos == len(tails): 
tails.append(x) 
log n` khoảng vài triệu nguyên thủy khác: 
đuôi[pos] = x 

trả về str(len(t hoạt động, thoải mái trong giới hạn thời gian. Theails)) 

# mẫu được cung cấp 
khẳng định run("3\n3 1 mức sử dụng bộ nhớ là tuyến tính và thấp hơn mức 2\n") == "2", "sample 1" 

# kích thước tối thiểu, đã được sắp xếp 
khẳng định có sẵn 256 MB. 

## Trường hợp thử nghiệm```python
# run("2\n1 2\n") == "2", "minimum sorted helper: run solution on input string, return output string
import sys, io
"

# minimum size, reversed
assert run("2\n2 1\n") == "1", "from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdinminimum reversed"

# fully increasing
assert run("5\n1 2 3  = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int,4 5\n") == "5", "all vertices independent"

# fully sys.stdin.readline().split()))

    pos = [0] * (n + 1)
    decreasing
assert run("5\n5 4 3 2 1\n") for i, v in enumerate(a):
        pos[v] = i

    tails = []

    == "1", "maximum inversion case"

# common for v in range(1, n + 1):
        x = pos[v]
        idx = bisect LIS example
assert run("6\n3 1 _left(tails, x)

        if idx == len(tails):
            tails5 2 6 4\n") == "3",.append(x)
        else:
            tails[idx] = x

    return str(len(tails))

# "general case"
```| Kiểm tra đầu vào | Mẫu được cung cấp đầu ra dự kiến 
khẳng định run("3\n3 1 2\n") == "2", " | Những gì nó xác nhận | 
|---|---|---| 
| `2 / mẫu 1" 

# kích thước tối thiểu, được sắp xếp 
khẳng định chạy ("2\n1 21 2` | `2` | Nhỏ nhất\n") == "2", "được sắp xếp tối thiểu" 

# kích thước tối thiểu, đảo ngược 
khẳng định run(" hoán vị được sắp xếp | 
|`2 / 2 1`|`1`| Đảo ngược không cần thiết nhỏ nhất | 
| `5 / 2\n2 1\n") == "1", "đảo ngược tối thiểu" 

# đã được sắp xếp 
khẳng định1 2 3 4 5` | `5` | Chạy biểu đồ("5\n1 2 3 4 5\n") == "5", "biểu đồ trống" 

# không có cạnh | 
|`5 / 5 4 3 2 1`| ` hoàn toàn đảo ngược 
khẳng định run("5\n5 4 3 2 1\n") == "1", "đồ thị hoàn chỉnh" 

#trường hợp hỗn hợp 
khẳng định run("4\n1` | Mật độ đảo ngược tối đa | 
|`6 / 3 1 5 2 6 4`|`2 1 4 3\n") == "2", "two independent3`| Hành vi LIS chung | 

## Vỏ cạnh 

Hãy xem xét các cặp"```

| Test input | Expected output | the already sorted permutation:

```văn bản 
5 
1 Nó xác nhận điều gì | 
|---|---|---| 
| `2 / 2 3 4 5```

Bubble sort performs no1 2` | `2` | Minimum swaps. The graph contains five size, no inversions |
| `2 / 2  isolated vertices. The LIS algorithm1` | `1` | Minimum size, one inversion |
 produces length 5 because every| `5 / 1 2 3 4 5` element extends the current subsequence. | `5` | Empty graph case |
| `5 / 5  The answer is correctly4 3 2 1` | `1` | 5.

Consider the reverse permutation:

```văn bản 
5 
Trường hợp đồ thị hoàn chỉnh | 
| `4 / 2 1 4 5 4 3 2 1```

Every3` | `2` | Mixed inversion structure |

## Edge Cases

 pair forms an inversion. The graph becomes complete, meaningConsider the sorted permutation:

```văn bản 
5 
1 2 mọi cặp đỉnh đều kề nhau.3 4 5```

The Any independent set can contain only position sequence is:

```văn bản 
[0,1, một đỉnh. Độ dài LIS cũng là2,3,4]```

Its LIS 1, which the algorithm returns length is `5`..

 Bubble sort performs no swaps, so theConsider a permutation where not every inversion corresponds to graph has no edges. Every vertex belongs to the an obvious adjacent pair:

```văn bản 
4 
4 1 3 2 
`` maximum independent set. The algorithm returns`LIS là`[1, 3] `5`. 

Bây giờ hãy xem xét hoán vị ngược` or `[1, 2]`, cả hai:```text
5
5 4 3 2 1
 of length 2. The algorithm computes```Trình tự vị trí là:```text
[4,3, 2. This demonstrates why the reduction is2,1,0]
```Độ dài LIS của nó là`1`. dựa trên các cặp đảo ngược thay vì Mỗi cặp giá trị tạo thành một sự đảo ngược, tạo ra thay vì cố gắng theo dõi từng loại bong bóng riêng lẻ sẽ vượt qua một biểu đồ hoàn chỉnh. Bất kỳ tập hợp độc lập nào cũng chứa nhiều nhất một đỉnh. Mọi sự đảo ngược đều tạo ra chính xác. Thuật toán trả về chính xác`1`. 

Một ví dụ tinh tế hơn về một cạnh, bất kể việc hoán đổi xảy ra khi nào. 

là:```text
4
4 1 2 3
```TheConsider:```text
6
2 1 4 3 6 5
```Trình tự vị trí là:```text
[1,2,3,0]
```Các nghịch đảo là các cặp cục bộ độc lập. Độ dài LIS là`3`. 

Sắp xếp bong bóng tạo ra các cạnh`(4,1)`, độ dài LIS là 3 chẳng hạn`[2,4,6]`hoặc`[ `(4,2)`, `(4,3)` and nothing else. Values `{1,2,3}`1,3,5]`. Thuật toán trả về 3, không có cạnh nào giữa chúng, khớp với mức tối đa để tạo ra một tập hợp kích thước độc lập`3`. Kích thước tập hợp độc lập của thuật toán. Điều này xác nhận rằng nhiều cấu trúc đảo ngược bị ngắt kết nối nhận thấy chính xác rằng thông qua việc tăng dần được xử lý một cách tự nhiên bởi dãy con LIS`[1,2,3]`. 

Những ví dụ này bao gồm những cạm bẫy chính và minh họa lý do tại sao lại áp dụng công thức này. cấu trúc đồ thị tương đương với một bài toán LIS.
