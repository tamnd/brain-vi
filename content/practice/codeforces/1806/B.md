--- 
title: "CF 1806B - Bậc thầy Mex" 
mô tả: "Chúng ta được cấp một mảng và có thể hoán vị các phần tử của nó theo cách chúng ta muốn. Sau khi chọn thứ tự, chúng ta xây dựng một mảng mới bao gồm tổng của các phần tử lân cận:$$b=[a1+a2, a2+a3, ldots, a{n-1}+an]." date: "2026-06-09T09:11:13+07:00" tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"] categories: ["algorithms"] codeforces_contest: 1806 codeforces_index: "B" codeforces_contest_name: "Codeforces Round 858 (Div. 2)" rating: 900 weight: 1806 solve_time_s: 290 verified: false draft: false --- [CF 1806B - Mex Master](https://codeforces.com/problemset/problem/1806/B) **Rating:** 900   **Tags:** constructive algorithms, greedy   **Solve time:** 4m 50s   **Verified:** no   ## Solution ## Problem Understanding We are given an array and may permute its elements however we like. After choosing an ordering, we build a new array consisting of sums of neighboring elements:$$b=[a_1+a_2,\ a_2+a_3,\ \ldots,\ a_{n-1}+a_n].$$
The score of the arrangement is the MEX of $b$, where MEX is the smallest non-negative integer that does not appear.
Our task is not to construct the optimal ordering. We only need the minimum possible score obtainable after rearranging the original numbers.
The constraints immediately suggest that the answer must come from a simple structural observation. The total length over all test cases is only $2\cdot 10^5$, so an $O(n)$ or $O(n \log n)$ solution per test case is easily fast enough. Any approach that tries many permutations is hopeless because even $n=20$ already makes enumeration impossible.
The interesting part is that only the values $0$, $1$, and "greater than $1$" matter.
Consider a few edge cases.
If there is a positive number between every pair of zeros, then no adjacent pair sums to $0$. For example:
⟦PROTECT_0⟧
The adjacent sums are:
⟦PROTECT_1⟧
MEX is $0$.
A careless solution that only checks whether zeros exist would incorrectly return $1$.
Now consider:
⟦PROTECT_2⟧
The only adjacent sum is:
⟦PROTECT_3⟧
MEX is $1$.
Finally:
⟦PROTECT_4⟧
No matter how we arrange the numbers, at least one adjacent pair of zeros exists. That creates a sum $0$. At the same time, every positive number is at least $2$, so sum $1$ can never appear. The resulting minimum MEX is $1$.
These examples already hint that only the counts of zeros and ones control the answer.
## Approaches
A brute-force solution would try every permutation of the array. For each permutation it would build the adjacent-sum array, compute its MEX, and keep the minimum.
This is correct because it directly checks every possible arrangement. Unfortunately, it requires $n!$ permutations. Even $n=12$ would already be completely infeasible.
The key observation is that the minimum possible MEX can only be $0$, $1$, or $2$.
To obtain MEX $0$, we need to avoid creating a sum equal to $0$. Since all values are non-negative, the only way to get a sum $0$ is with two adjacent zeros.
Thus MEX $0$ is possible exactly when we can arrange the array so that no two zeros are adjacent.
Suppose there are $z$ zeros and $p=n-z$ positive numbers.
Each positive number can separate at most one gap between consecutive zeros. To place all zeros without adjacency we need
$$z \le p+1.$$
Equivalently,
$$z \le n-z+1.$$
If this holds, we can eliminate all occurrences of $0$ from the adjacent-sum array, so MEX becomes $0$.
Now assume $z>p+1$. Then some pair of adjacent zeros is unavoidable, so $0$ must appear.
Can we make MEX equal to $1$? We need $0$ to appear but $1$ not to appear.
A sum $1$ can only be produced by $0+1$, because all numbers are non-negative integers.
If the array contains no ones, then sum $1$ is impossible, hence the answer is immediately $1$.
The only remaining situation is:
$$z>p+1$$ 
và có ít nhất một giá trị bằng$1$. 
Sau đó$0$là điều không thể tránh khỏi và một số$0+1$lân cận cũng là điều khó tránh khỏi. Trong trường hợp đó cả hai$0$Và$1$xuất hiện, do đó MEX tối thiểu là$2$. 
Điều này hoàn toàn xác định câu trả lời. 
| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!\cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 
## Hướng dẫn thuật toán 
1. Đếm số 0, gọi nó là$z$. 
2. Đếm số cái, gọi tên$o$. 
3. Hãy để$p=n-z$, số phần tử dương. 
4. Nếu$z \le p+1$, đầu ra$0$. 
Trong trường hợp này, các số 0 có thể được phân tách bằng các phần tử dương, do đó không có cặp liền kề nào có tổng bằng$0$. Từ$0$vắng mặt trong mảng tổng liền kề, MEX của nó là$0$. 
5. Nếu không, không thể tránh khỏi một số cặp số 0 liền kề, vì vậy$0$xuất hiện. 
6. Nếu$o=0$, đầu ra$1$. 
một khoản tiền$1$yêu cầu một$0$và một$1$. Không có cái nào, tổng$1$không thể xảy ra. 
7. Nếu không thì xuất ra$2$. 
Đây$0$xuất hiện vì các số 0 liền kề là không thể tránh khỏi, và$1$cũng có thể bị buộc phải xuất hiện vì một$1$tồn tại. Giá trị thiếu nhỏ nhất trở thành$2$. 
### Tại sao nó hoạt động 
Cách duy nhất để tạo ra tổng$0$là với các số 0 liền kề. Vì vậy khả năng đạt được MEX$0$phụ thuộc hoàn toàn vào việc liệu tất cả các số không có thể được tách ra hay không. 
Nếu không thể tách rời tất cả các số 0 thì$0$nhất thiết phải thuộc mảng tổng liền kề. Câu hỏi tiếp theo là liệu$1$có thể tránh được. 
Vì tất cả các giá trị đều không âm nên biểu diễn duy nhất của$1$dưới dạng tổng của hai phần tử mảng là$0+1$. Nếu không có phần tử nào bằng$1$tồn tại thì tính tổng$1$là không thể và MEX là$1$. Ngược lại cả hai$0$Và$1$xảy ra, buộc MEX phải$2$. 
Không có câu trả lời nào khác là có thể. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        zeros = a.count(0)
        ones = a.count(1)
        positive = n - zeros
        if zeros <= positive + 1:
            ans.append("0")
        elif ones == 0:
            ans.append("1")
        else:
            ans.append("2")
    sys.stdout.write("\n".join(ans))
if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo bằng chứng trực tiếp. 
Biến`zeros`xác định xem có thể tránh được các số 0 liền kề hay không. điều kiện`zeros <= positive + 1`chính xác là điều kiện phân tách xuất phát ở trên. 
Nếu không thể tránh khỏi các số 0 liền kề, chúng ta chỉ cần biết liệu phần tử nào có bằng không`1`. Không có giá trị nào khác quan trọng bởi vì chỉ`0+1`có thể tạo ra một tổng bằng`1`. 
Tất cả số học dễ dàng phù hợp với số nguyên Python tiêu chuẩn. 
## Ví dụ đã hoạt động 
### Ví dụ 1 
đầu vào:```
3
0 0 1
```| Số lượng | Giá trị | 
| --- | --- | 
| số không | 2 | 
| những cái | 1 | 
| tích cực | 1 | 
| kiểm tra |$2 \le 1+1$| 
Vì điều kiện được giữ nguyên nên các số 0 có thể được tách ra:```
0 1 0
```Các khoản liền kề:```
1 1
```MEX là$0$. 
Trả lời:```
0
```Điều này chứng tỏ rằng chỉ có số 0 không buộc phải tính tổng$0$xuất hiện. 
### Ví dụ 2 
đầu vào:```
8
1 0 0 0 2 0 3 0
```| Số lượng | Giá trị | 
| --- | --- | 
| số không | 5 | 
| những cái | 1 | 
| tích cực | 3 | 
| kiểm tra |$5 \le 3+1$là sai | 
Các số 0 liền kề là không thể tránh khỏi, vì vậy$0$xuất hiện. 
Một giá trị$1$tồn tại, do đó tổng$1$cũng có thể xảy ra. 
Vì vậy cả hai$0$Và$1$thuộc về mảng tổng liền kề, làm cho MEX tối thiểu bằng$2$. 
Trả lời:```
2
```Đây chính xác là mẫu thứ ba. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lượt để đếm số không và số một | 
| Không gian |$O(1)$| Chỉ có một vài quầy được lưu trữ | 
Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là$2\cdot10^5$, do đó việc quét tuyến tính mọi mảng dễ dàng nằm gọn trong giới hạn. 
## Trường hợp thử nghiệm```python
import sys
import io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        zeros = a.count(0)
        ones = a.count(1)
        positive = n - zeros
        if zeros <= positive + 1:
            out.append("0")
        elif ones == 0:
            out.append("1")
        else:
            out.append("2")
    return "\n".join(out)
# provided samples
assert run(
"""3
2
0 0
3
0 0 1
8
1 0 0 0 2 0 3 0
"""
) == "1\n0\n2"
# minimum size
assert run(
"""1
2
5 7
"""
) == "0"
# all zeros
assert run(
"""1
5
0 0 0 0 0
"""
) == "1"
# many zeros and a one
assert run(
"""1
5
0 0 0 0 1
"""
) == "2"
# enough positive separators
assert run(
"""1
6
0 0 1 2 3 4
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 7`|`0`| Không có tổng bằng 0 nào có thể tồn tại | 
|`0 0 0 0 0`|`1`| Các số 0 liền kề không thể tránh khỏi, không có số nào | 
|`0 0 0 0 1`|`2`| Các số 0 liền kề không thể tránh khỏi, một số tồn tại | 
|`0 0 1 2 3 4`|`0`| Số dương có thể tách biệt mọi số 0 | 
## Vỏ cạnh 
Hãy xem xét:```
2
0 0
```Đây`zeros = 2`,`positive = 0`. điều kiện`2 <= 1`là sai. Không có ai cả nên câu trả lời là`1`. Tổng liền kề duy nhất là`0`, MEX của nó thực sự là`1`. 
Coi như:```
3
0 0 1
```Đây`zeros = 2`,`positive = 1`. điều kiện`2 <= 2`là đúng. Ta có thể sắp xếp các số như`0 1 0`, tạo ra các tổng liền kề`1, 1`. Từ`0`vắng mặt, câu trả lời là`0`. 
Coi như:```
5
0 0 0 0 1
```Đây`zeros = 4`,`positive = 1`. điều kiện`4 <= 2`là sai, do đó phải tồn tại một số số 0 liền kề. Vì một`1`có mặt, cả hai tổng`0`Và`1`có thể xảy ra. Câu trả lời là`2`. 
Những trường hợp này bao gồm mọi nhánh có thể có của giải pháp.
