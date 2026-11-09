--- 
title: "CF 1490C - Tổng các khối" 
mô tả: "Chúng ta được cho một số số, và với mỗi số$x$, chúng ta phải xác định xem nó có thể được viết dưới dạng tổng của hai lập phương dương hay không. Nói cách khác, chúng ta muốn biết liệu có tồn tại số nguyên dương hay không$a$Và$b$như vậy$$a^3 + b^3 = x." date: "2026-06-10T22:37:19+07:00" tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "math"] categories: ["algorithms"] codeforces_contest: 1490 codeforces_index: "C" codeforces_contest_name: "Codeforces Round 702 (Div. 3)" rating: 1100 weight: 1490 solve_time_s: 150 verified: true draft: false --- [CF 1490C - Sum of Cubes](https://codeforces.com/problemset/problem/1490/C) **Rating:** 1100   **Tags:** binary search, brute force, math   **Solve time:** 2m 30s   **Verified:** yes   ## Solution ## Problem Understanding We are given several numbers, and for each number $x$, we must determine whether it can be written as the sum of two positive cubes. In other words, we want to know whether there exist positive integers $a$ and $b$ such that$$a^3 + b^3 = x.$$
The answer for each test case is simply "YES" if such a pair exists and "NO" otherwise.
The largest possible value of $x$ is $10^{12}$. That bound immediately tells us something useful about the size of the search space. Since
$$10000^3 = 10^{12},$$
any cube participating in the sum must come from an integer no larger than about 10,000.
A naive search over all pairs $(a,b)$ would require checking roughly $10^4 \times 10^4 = 10^8$ combinations for a single test case. With up to 100 test cases, that becomes completely impractical.
The constraints suggest that an algorithm around $10^4$ operations per test case is perfectly reasonable, while $10^8$ operations per test case is not.
There are a few easy-to-miss edge cases.
Consider:
⟦PROTECT_0⟧
The correct answer is:
⟦PROTECT_1⟧
Although $1 = 1^3$, the problem requires a sum of two positive cubes. The smallest possible sum is $1^3 + 1^3 = 2$.
Consider:
⟦PROTECT_2⟧
The correct answer is:
⟦PROTECT_3⟧
because $16 = 2^3 + 2^3$. A careless implementation that only checks distinct values of $a$ and $b$ would incorrectly reject this case.
Consider:
⟦PROTECT_4⟧
The correct answer is:
⟦PROTECT_5⟧
because $2 = 1^3 + 1^3$. This is the smallest valid representation and is a useful boundary test.
Another common mistake is using floating-point cube roots. For values near $10^{12}$, rounding errors can cause an exact cube to be missed. Integer arithmetic avoids that problem entirely.
## Approaches
The most direct solution is to try every possible pair $(a,b)$. Since every relevant cube root is at most about 10,000, we could iterate through all values of $a$ and $b$, compute $a^3+b^3$, and check whether it equals $x$.
This approach is correct because it explicitly examines every candidate pair. The problem is the running time. Roughly $10^8$ pairs must be checked for a single test case, which is far beyond what fits within the time limit.
The key observation is that once we choose one cube, the other cube is completely determined.
Suppose we fix $a$. Then we need
$$b^3 = x - a^3.$$ 
Thay vì cố gắng mọi cách có thể$b$, chúng ta chỉ cần xác định xem giá trị$x-a^3$bản thân nó là một khối lập phương hoàn hảo. 
Vì chỉ có khoảng 10.000 giá trị khối có thể, nên chúng ta có thể tính toán trước tất cả các khối lên đến$10^{12}$và lưu trữ chúng trong một bộ băm. Sau đó với mỗi ứng viên$a$, chúng tôi tính toán$x-a^3$và thực hiện một$O(1)$kiểm tra tư cách thành viên trong tập hợp. 
Điều này làm giảm việc tìm kiếm từ việc kiểm tra tất cả các cặp thành chỉ kiểm tra tất cả các phần tử đầu tiên có thể có. 
| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(M^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(M)$|$O(M)$| Đã chấp nhận | 
Đây$M \approx 10^4$, căn bậc ba lớn nhất có thể. 
## Hướng dẫn thuật toán 
1. Tính toán trước tất cả các hình khối$i^3$cho số nguyên dương$i$trong khi$i^3 \le 10^{12}$. 
2. Lưu trữ các khối đó trong bộ băm. Điều này cho phép kiểm tra liên tục xem một số có phải là một khối lập phương hoàn hảo trong phạm vi được tính toán trước của chúng tôi hay không. 
3. Với mỗi test case, hãy đọc$x$. 
4. Lặp lại từng giá trị khối được tính toán trước$c=a^3$. 
5. Tính giá trị còn lại$x-c$. 
6. Kiểm tra xem$x-c$tồn tại trong tập khối. 
7. Nếu nó tồn tại thì$x=a^3+b^3$với một số nguyên dương$b$, do đó đầu ra`"YES"`và ngừng xử lý trường hợp thử nghiệm này. 
8. Nếu vòng lặp kết thúc mà không tìm được kết quả phù hợp, xuất ra`"NO"`. 
### Tại sao nó hoạt động 
Thuật toán kiểm tra mọi giá trị có thể có của khối đầu tiên$a^3$. Đối với bất kỳ đại diện hợp lệ$x=a^3+b^3$, việc lặp lại cuối cùng sẽ đạt được điều đó$a^3$. Tại thời điểm đó, số dư tính toán bằng$b^3$, có trong tập khối nên thuật toán trả lời`"YES"`. 
Ngược lại, bất cứ khi nào thuật toán tìm thấy$x-a^3$thuộc tập lập phương nên tồn tại số nguyên dương$b$khối lập phương của nó bằng số dư đó. phương trình$x=a^3+b^3$sau đó hài lòng, nên trả lời`"YES"`là đúng. 
Vì mọi khối đầu tiên có thể đều được kiểm tra và bài kiểm tra thành viên nhận ra chính xác các khối được tính toán trước nên không thể bỏ sót biểu diễn hợp lệ nào và không thể chấp nhận biểu diễn không hợp lệ nào. 
## Giải pháp Python```python
import sys
input = sys.stdin.readline
LIMIT = 10 ** 12
cubes = []
cube_set = set()
i = 1
while i ** 3 <= LIMIT:
    c = i ** 3
    cubes.append(c)
    cube_set.add(c)
    i += 1
t = int(input())
for _ in range(t):
    x = int(input())
    found = False
    for c in cubes:
        if c >= x:
            break
        if x - c in cube_set:
            found = True
            break
    print("YES" if found else "NO")
```Phần đầu tiên tính toán trước tất cả các hình khối lên đến$10^{12}$. Chỉ có 10.000 trong số đó, vì vậy cả việc sử dụng bộ nhớ và thời gian xử lý trước đều rất nhỏ. 
Bộ băm là cấu trúc dữ liệu quan trọng. Không có nó, kiểm tra xem$x-c$là một khối lập phương sẽ yêu cầu tìm kiếm khác, làm cho giải pháp chậm hơn. 
Trong mỗi trường hợp thử nghiệm, chúng tôi lặp qua mọi khối đầu tiên có thể. Đối với mỗi người, chúng tôi tính toán số tiền còn lại cần thiết để đạt được$x$. Nếu số dư đó nằm trong tập lập phương thì chúng ta đã tìm được một phân tách hợp lệ. 
điều kiện`if c >= x: break`là một tối ưu hóa nhỏ. Một khi khối hiện tại ít nhất là$x$, tất cả các lập phương sau này thậm chí còn lớn hơn nên phần dư sẽ bằng 0 hoặc âm. Vì cả hai số nguyên đều phải dương nên không có nghiệm nào có thể xuất hiện ngoài điểm đó. 
Số nguyên Python tự động xử lý các giá trị lớn hơn 32 bit, do đó không có vấn đề tràn ngay cả khi khối tiếp cận$10^{12}$. 
## Ví dụ đã hoạt động 
### Ví dụ 1 
đầu vào:```
35
```| khối hiện tại$c$| còn lại$35-c$| Trong bộ khối lập phương? | 
| --- | --- | --- | 
| 1 | 34 | Không | 
| 8 | 27 | Có | 
Thuật toán đạt$c=8=2^3$. Số dư là 27, bằng$3^3$. Vì 27 nằm trong bộ lập phương nên câu trả lời là`"YES"`. 
Ví dụ này cho thấy cách kiểm tra một khối đơn lẻ sẽ tự động xác định khối đối tác duy nhất có thể có. 
### Ví dụ 2 
đầu vào:```
34
```| khối hiện tại$c$| còn lại$34-c$| Trong bộ khối lập phương? | 
| --- | --- | --- | 
| 1 | 33 | Không | 
| 8 | 26 | Không | 
| 27 | 7 | Không | 
Khối tiếp theo sẽ là 64, vượt quá 34, do đó vòng lặp sẽ dừng lại. 
Không tìm thấy số dư trong bộ lập phương nên đáp án là`"NO"`. 
Ví dụ này chứng minh rằng mọi khối đầu tiên ứng cử viên đều được kiểm tra, nhưng không có khối nào tạo ra một khối khác làm phần còn lại. 
## Phân tích độ phức tạp 
| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M)$mỗi trường hợp thử nghiệm | Lặp lại qua tất cả các khối được tính toán trước | 
| Không gian |$O(M)$| Lưu trữ các khối và bộ băm | 
Đây$M \approx 10^4$, bởi vì$10000^3 = 10^{12}$. 
Tối đa 10.000 giá trị khối được kiểm tra cho mỗi trường hợp thử nghiệm. Ngay cả với 100 trường hợp thử nghiệm, tổng công việc chỉ khoảng một triệu lần lặp, vừa vặn trong giới hạn. 
## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    LIMIT = 10 ** 12
    cubes = []
    cube_set = set()
    i = 1
    while i ** 3 <= LIMIT:
        c = i ** 3
        cubes.append(c)
        cube_set.add(c)
        i += 1
    t = int(input())
    ans = []
    for _ in range(t):
        x = int(input())
        found = False
        for c in cubes:
            if c >= x:
                break
            if x - c in cube_set:
                found = True
                break
        ans.append("YES" if found else "NO")
    return "\n".join(ans) + "\n"
# provided sample
assert run(
"""7
1
2
4
34
35
16
703657519796
"""
) == """NO
YES
NO
NO
YES
YES
YES
"""
# minimum value
assert run(
"""1
1
"""
) == """NO
"""
# smallest valid representation
assert run(
"""1
2
"""
) == """YES
"""
# equal cubes
assert run(
"""1
16
"""
) == """YES
"""
# large boundary cube sum
assert run(
"""1
2000000000000
"""
) == """YES
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NO`| Giá trị nhỏ nhất có thể | 
|`2`|`YES`| Phân rã hợp lệ nhỏ nhất | 
|`16`|`YES`| Trường hợp cả hai khối đều bằng nhau | 
|`2000000000000`|`YES`| Các giá trị lớn gần ranh giới căn bậc ba | 
## Vỏ cạnh 
Hãy xem xét đầu vào:```
1
1
```Thuật toán bắt đầu với khối nhỏ nhất,$1$. Từ$1 \ge x$, vòng lặp lập tức dừng lại. Không có cặp hợp lệ nào tồn tại vì cả hai khối đều phải dương. Đầu ra là`"NO"`. 
Hãy xem xét đầu vào:```
1
2
```Khối đầu tiên là$1$. Phần còn lại là$2-1=1$. Vì 1 có mặt trong tập khối nên thuật toán tìm biểu diễn$1^3+1^3$và đầu ra`"YES"`. 
Hãy xem xét đầu vào:```
1
16
```Thuật toán kiểm tra$1$, sau đó$8$. Vì$c=8$, phần còn lại cũng là$8$. Vì tập lập phương chứa 8 nên thuật toán chấp nhận phân tách$2^3+2^3$. Điều này xác nhận rằng các hình khối bằng nhau được xử lý một cách tự nhiên. 
Hãy xem xét đầu vào:```
1
34
```Thuật toán kiểm tra mọi khối đầu tiên có thể nhỏ hơn 34. Các phần còn lại là 33, 26 và 7, không có khối nào là khối lập phương. Việc tìm kiếm kết thúc và câu trả lời là`"NO"`. Điều này xác minh rằng thuật toán không chấp nhận sai các số gần với tổng lập phương hợp lệ nhưng thực tế không thể biểu diễn được.
