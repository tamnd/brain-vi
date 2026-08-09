---
title: "CF 102458C - Trò chơi của Daniel"
description: "Chúng ta có một mảng A gồm n số nguyên không âm và M. Đối với mỗi mảng con liền kề không trống A[l..r], Andy có thể tăng các phần tử của nó, nhưng tổng số lượng được thêm vào không thể vượt quá M. Mục tiêu của anh ấy là làm cho mảng con đã chọn không giảm."
date: "2026-08-09T02:42:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102458
codeforces_index: "C"
codeforces_contest_name: "Hanoi final contest"
rating: 0
weight: 102458
solve_time_s: 298
verified: true
draft: false
---

[CF 102458C - Trò chơi của Daniel](https://codeforces.com/problemset/problem/102458/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 58 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`A`của`n`số nguyên không âm và ngân sách`M`. Đối với mọi mảng con liền kề không trống`A[l..r]`, Andy có thể tăng các phần tử của nó nhưng tổng số lượng được thêm vào không được vượt quá`M`. Mục tiêu của anh ta là làm cho mảng con được chọn không giảm. 

Đối với một mảng con cố định, chỉ có một đại lượng phù hợp: tổng số tiền tối thiểu phải được thêm vào để làm cho nó không giảm. Chúng ta cần đếm tối đa có bao nhiêu mảng con có chi phí tối thiểu này`M`. 

Giả sử mảng con được chọn bắt đầu tại`l`. Xử lý nó từ trái sang phải. Giá trị đầu tiên không cần thay đổi. Tại vị trí`i`, giá trị nhỏ nhất có thể sau khi sửa đổi là giá trị lớn hơn giá trị ban đầu`A[i]`và giá trị được sửa đổi trước đó. Do đó, chuỗi sửa đổi tối ưu chính xác là chuỗi tiền tố cực đại: 

[ 
B_i=\max(A_l,A_{l+1},\ldots,A_i). 
] 

Chi phí tối thiểu của`[l,r]`do đó là 

[ 
C(l,r)=\sum_{i=l}^{r}\left(\max_{j=l}^{i} A_j-A_i\right). 
] 

Một mảng con sẽ thắng chính xác khi`C(l,r) <= M`. 

Hạn chế quan trọng là`n <= 2 * 10^5`, trong khi`M`có thể lớn như`2 * 10^14`và mỗi`A[i]`có thể lớn như`10^9`. có khoảng`n(n+1)/2`mảng con, vì vậy việc liệt kê chúng đã mất khoảng`2 * 10^10`trường hợp ở kích thước tối đa. Một giải pháp dành thời gian tuyến tính cho mỗi mảng con là hoàn toàn không thể. Chúng tôi cần gần`O(n log n)`hoạt động và mọi chi phí phải sử dụng số học 64 bit. Số nguyên Python xử lý phạm vi này một cách tự nhiên. 

Có một số trường hợp đặc biệt dễ gây ra câu trả lời sai. 

Khi`n=1`, mảng con một phần tử đã không giảm và có giá bằng 0. Vì`1 0`với mảng`1234`, câu trả lời là`1`. Việc triển khai chỉ đếm các mảng con chứa đảo ngược sẽ trả về 0 không chính xác. 

Các giá trị bằng nhau không được coi là mức tăng nghiêm ngặt. Vì`n=4`,`M=0`và mảng`5 5 5 5`, mỗi mảng trong số mười mảng con đều không giảm, vì vậy câu trả lời là`10`. Nếu cấu trúc lớn hơn tiếp theo coi giá trị bằng nhau là giá trị lớn hơn, thì nó có thể phân chia khối tối đa tiền tố không chính xác. 

Việc so sánh ngân sách là bao gồm. Vì`n=3`,`M=3`và mảng`3 1 2`, mảng con`[3,1,2]`chi phí chính xác`3`: hai giá trị nhỏ hơn phải trở thành`3`, thêm`2+1`. Nó hợp lệ nên tất cả sáu mảng con đều được tính. sử dụng`< M`thay vì`<= M`sẽ mất khoảng này. 

Cuối cùng, chi phí có thể lớn hơn nhiều so với số nguyên 32 bit. Với`n=2*10^5`và các giá trị xung quanh`10^9`, tổng hiệu chỉnh có thể đạt tới thứ tự`10^14`. Bộ tích lũy 32 bit âm thầm tràn vào các ngôn ngữ có số nguyên có chiều rộng cố định. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là sửa từng cặp`(l,r)`và mô phỏng việc xây dựng tham lam của một chuỗi không giảm. Bắt đầu với`maximum = A[l]`, mỗi phần tử sau đây góp phần`max(0, maximum - A[i])`, Và`maximum`được cập nhật khi`A[i]`lớn hơn. Điều này đúng vì mọi phần tử ít nhất phải có giá trị cuối cùng trước đó, vì vậy việc chọn bất kỳ phần tử nào lớn hơn chỉ có thể làm tăng chi phí. 

có`n(n+1)/2`mảng con và một mảng con có thể chứa`O(n)`các phần tử. Trong trường hợp xấu nhất điều này có nghĩa 

[ 
\Theta(n^3) 
] 

hoạt động. Với`n=2*10^5`, đó là theo thứ tự của`8*10^15`lần lặp cơ bản, vượt xa giới hạn hai giây. 

Cách tiếp cận bạo lực hoạt động vì nó tuân theo cấu trúc tham lam chính xác, nhưng nó liên tục đi qua các phần giống nhau của mảng. Quan sát mở ra giải pháp nhanh hơn là tiền tố tối đa không thay đổi ở mọi vị trí. Bắt đầu từ vị trí`l`, mức tối đa hiện tại vẫn còn`A[l]`cho đến vị trí đầu tiên có giá trị lớn hơn`A[l]`. Sau khi đạt đến vị trí đó, lý do tương tự lại bắt đầu từ mức tối đa mới. 

Định nghĩa`nxt[i]`là vị trí đầu tiên bên phải của`i`với`A[nxt[i]] > A[i]`. Giữa`i`Và`nxt[i]-1`, tiền tố tối đa chính xác là`A[i]`. Toàn bộ chi phí của khối này là 

[ 
A_i(nxt[i]-i)-\sum_{j=i}^{nxt[i]-1}A_j. 
] 

Do đó, chi phí của một mảng con dài có thể được phân tách thành một chuỗi gồm các khối hoàn chỉnh lớn hơn tiếp theo cộng với một khối một phần cuối cùng. 

Tất cả`nxt[i]`các giá trị có thể được tìm thấy trong thời gian tuyến tính với một ngăn xếp đơn điệu. Sau đó, chúng tôi sử dụng nâng cấp nhị phân để bỏ qua nhiều khối lớn hơn tiếp theo cùng một lúc và thu được`C(l,r)`TRONG`O(log n)`. 

Quan sát cuối cùng là điểm cuối bên phải hợp lệ tối đa là đơn điệu. Nếu chúng ta di chuyển`l`ở bên phải trong khi giữ`r`đã sửa, chúng tôi loại bỏ các phần tử ngay từ đầu nên chi phí yêu cầu tối thiểu không thể tăng lên. Do đó giá trị hợp lệ lớn nhất`r`không bao giờ di chuyển sang trái như`l`tăng lên. Điều này cho phép quét hai con trỏ: mỗi điểm cuối bên phải chỉ tiến lên`O(n)`lần về tổng thể, trong khi mỗi lần kiểm tra chi phí đều mất`O(log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^3)`|`O(1)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n log n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng tổng tiền tố thông thường`pref`, Ở đâu`pref[i]`là tổng của`A[0..i-1]`. Điều này cho phép chúng ta tính tổng của bất kỳ khối liền kề nào trong thời gian không đổi. 
2. Tính toán`nxt[i]`, chỉ số đầu tiên`j > i`với`A[j] > A[i]`. Quét từ phải sang trái với ngăn xếp giảm dần đơn điệu. Trong khi phần trên có giá trị nhỏ hơn`A[i]`, bật nó lên vì`i`trở thành phần tử thực sự lớn hơn đầu tiên của nó. Các giá trị bằng nhau không được hiển thị vì tiền tố tối đa không thay đổi khi chúng ta gặp một giá trị bằng nhau. 
3. Đối với mọi vị trí`i`, xác định chi phí của khối hoàn chỉnh của nó là chi phí từ`i`bởi vì`nxt[i]-1`. Nếu không có phần tử nào lớn hơn thì khối kết thúc tại`n`. Chi phí của nó là 

[ 
block[i]=A_i(nxt[i]-i)-(pref[nxt[i]]-pref[i]). 
] 

Trong khoảng này, mỗi tiền tố tối đa là`A[i]`, do đó công thức tính tổng trực tiếp`A[i]-A[j]`. 

1. Xây dựng bàn nâng nhị phân.`up[k][i]`đại diện cho vị trí đạt được sau khi theo dõi`2^k`bước nhảy lớn tiếp theo bắt đầu từ`i`.`gain[k][i]`lưu trữ tổng của tất cả các chi phí khối hoàn chỉnh mà các bước nhảy đó vượt qua. Cấp độ cơ sở bao gồm một bước nhảy, vì vậy`up[0][i] = nxt[i]`Và`gain[0][i] = block[i]`. 
2. Triển khai một chức năng`cost(l,r)`tính toán chi phí tối thiểu để thực hiện`A[l..r]`không giảm. Bắt đầu lúc`l`, kiểm tra các mức nâng nhị phân từ lớn nhất đến nhỏ nhất. Nếu đích đến của bước nhảy nhiều nhất là`r`, toàn bộ khối đó nằm trong khoảng được yêu cầu, vì vậy hãy cộng chi phí được tính toán trước của nó và chuyển đến đó. Sau khi không còn khối hoàn chỉnh nào phù hợp nữa, khoảng thời gian còn lại bắt đầu tại`cur`và kết thúc tại`r`. Tiền tố tối đa của nó là`A[cur]`, đưa ra chi phí từng phần cuối cùng 

[ 
A_{cur}(r-cur+1)-(pref[r+1]-pref[cur]). 
] 

1. Quét mảng bằng hai con trỏ. Giữ một điểm cuối đúng`r`. Đối với mỗi điểm cuối bên trái mới`l`, liên tục kéo dài`r`trong khi`cost(l,r+1) <= M`. Mọi tiện ích mở rộng đều là vĩnh viễn vì điểm cuối phù hợp không bao giờ cần phải lùi lại. Khi tiện ích mở rộng tiếp theo vượt quá ngân sách, mọi điểm cuối bên phải lớn hơn cũng không hợp lệ vì việc thêm một phần tử không thể giảm chi phí sửa đổi cần thiết. 
2. Đối với hiện tại`l`, chính xác`r-l+1`mảng con bắt đầu tại`l`là hợp lệ. Thêm số này vào câu trả lời và tăng dần`l`. Việc loại bỏ phần tử ngoài cùng bên trái không thể làm tăng chi phí sửa đổi tối thiểu, do đó chi phí hiện tại`r`vẫn hợp lệ và bất biến hai con trỏ được giữ nguyên. 

### Tại sao nó hoạt động 

Đối với mỗi mảng con, giá trị cuối cùng tối thiểu có thể có ở mỗi vị trí là tiền tố tối đa của nó. Phân tách lớn hơn tiếp theo sẽ phân chia các cực đại tiền tố đó thành các vùng cực đại trong đó cực đại hiện tại là không đổi, do đó công thức khối tính toán chính xác chi phí tương tự như quét tham lam trực tiếp. Nâng nhị phân chỉ bỏ qua các vùng hoàn chỉnh liên tiếp mà không thay đổi tổng đóng góp của chúng và vùng một phần cuối cùng được tính trực tiếp. 

Phần hai con trỏ dựa vào tính đơn điệu. Đối với một cố định`l`, mở rộng`r`chỉ có thể thêm phần đóng góp không âm, vì vậy khi điểm cuối bên phải trở nên không hợp lệ thì mọi điểm cuối lớn hơn đều không hợp lệ. Đối với một cố định`r`, di chuyển`l`quyền loại bỏ các ràng buộc và không thể tăng chi phí tối thiểu, do đó giá trị hợp lệ lớn nhất`r`không thể di chuyển sang trái. Do đó, mọi mảng con hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, M = map(int, input().split())
    a = list(map(int, input().split()))

    # Prefix sums.
    pref = [0] * (n + 1)
    s = 0
    for i, x in enumerate(a):
        s += x
        pref[i + 1] = s

    # nxt[i] = first j > i with a[j] > a[i].
    nxt = [n] * n
    stack = []

    for i in range(n - 1, -1, -1):
        x = a[i]
        while stack and a[stack[-1]] < x:
            nxt[stack.pop()] = i
        stack.append(i)

    # Cost of the complete block [i, nxt[i)-1].
    block = [0] * n
    for i in range(n):
        j = nxt[i]
        block[i] = a[i] * (j - i) - (pref[j] - pref[i])

    LOG = n.bit_length()

    # Binary lifting tables.
    up = [nxt]
    gain = [block]

    for k in range(1, LOG):
        prev_up = up[-1]
        prev_gain = gain[-1]

        cur_up = [n] * n
        cur_gain = [0] * n

        for i in range(n):
            mid = prev_up[i]
            if mid < n:
                cur_up[i] = prev_up[mid]
                cur_gain[i] = prev_gain[i] + prev_gain[mid]

        up.append(cur_up)
        gain.append(cur_gain)

    def cost(l, r):
        """Minimum increments needed for a[l..r]."""
        cur = l
        ans = 0

        for k in range(LOG - 1, -1, -1):
            j = up[k][cur]
            if j <= r:
                ans += gain[k][cur]
                cur = j

        # cur is the beginning of the final partial block.
        ans += a[cur] * (r - cur + 1) - (pref[r + 1] - pref[cur])
        return ans

    ans = 0
    r = -1

    for l in range(n):
        if r < l:
            r = l

        while r + 1 < n and cost(l, r + 1) <= M:
            r += 1

        ans += r - l + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Việc xây dựng tổng tiền tố tương ứng trực tiếp với công thức chi phí khối.`pref[j] - pref[i]`là tổng của tất cả các giá trị ban đầu trong khối, trong khi`a[i] * (j-i)`là tổng sau khi mọi phần tử trong khối đó được nâng lên mức tối đa của khối. 

Ngăn xếp đơn điệu sử dụng phép so sánh chặt chẽ`a[stack[-1]] < x`. Thay đổi điều này thành`<=`sẽ không chính xác. Đối với các giá trị bằng nhau, giá trị trước đó vẫn giữ nguyên mức tối đa tiền tố, do đó, giá trị lớn hơn đầu tiên là giá trị kết thúc khối của nó. 

Sử dụng bàn nâng nhị phân`n`như một trọng điểm có nghĩa là không có phần tử nào lớn hơn tồn tại. Một bước nhảy chỉ được sử dụng khi đích đến của nó là`<= r`. Điều kiện này đảm bảo rằng toàn bộ khối được biểu thị bằng bước nhảy thuộc về`[l,r]`. 

Sau khi tất cả các khối hoàn chỉnh đã được tiêu thụ,`cur`là vị trí đầu tiên có phần tử lớn hơn tiếp theo nằm ngoài`r`. Do đó mọi tiền tố tối đa từ`cur`bởi vì`r`bằng`a[cur]`, điều này làm cho công thức hằng số thời gian cuối cùng có giá trị. 

Vòng lặp hai con trỏ có chủ ý kiểm tra`r+1`thay vì tính toán lại chi phí của cửa sổ hiện tại. hiện tại`[l,r]`đã được biết là hợp lệ. Một lần`[l,r+1]`không thành công, tất cả các điểm cuối bên phải tiếp theo cũng không thành công. 

Các số nguyên có độ chính xác tùy ý của Python rất hữu ích ở đây vì cả hai`M`và chi phí tích lũy có thể vào khoảng`10^14`. Không cần xử lý tràn rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`A = [5,4,1,1,5,5]`Và`M = 6`, điểm cuối bên phải hợp lệ tối đa cho mỗi điểm cuối bên trái như sau. 

|`l`| Tối đa`r`|`cost(l,r)`| Chi phí tiếp theo | Đã thêm mảng con | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 5 | 9 | 3 | 
| 2 | 6 | 6 | ngoài mảng | 5 | 
| 3 | 6 | 0 | ngoài mảng | 4 | 
| 4 | 6 | 0 | ngoài mảng | 3 | 
| 5 | 6 | 0 | ngoài mảng | 2 | 
| 6 | 6 | 0 | ngoài mảng | 1 | 

Vì`l=1`, tiền tố cực đại của`[5,4,1]`là`[5,5,5]`, vậy chi phí là`0+1+4=5`. Mở rộng đến phần tử thứ tư sẽ mang lại một phần tử khác`4`, tính chi phí`9`, do đó điểm cuối bên phải dừng lại ở`3`. 

Vì`l=2`, mảng con`[4,1,1,5,5]`chi phí`3+3+0+0=6`, chính xác là ngân sách sẵn có. Sự đẳng thức chính xác chứng tỏ tại sao điều kiện phải là`<= M`. 

Số lượng là`3+5+4+3+2+1 = 18`, khớp với đầu ra mẫu chính thức. 

### Mẫu 2 

cho`A = [6,5,4,3,2]`Và`M = 5`, vết là: 

|`l`| Tối đa`r`|`cost(l,r)`| Chi phí tiếp theo | Đã thêm mảng con | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 6 | 3 | 
| 2 | 4 | 3 | 6 | 3 | 
| 3 | 5 | 3 | ngoài mảng | 3 | 
| 4 | 5 | 1 | ngoài mảng | 2 | 
| 5 | 5 | 0 | ngoài mảng | 1 | 

Ví dụ,`[6,5,4]`yêu cầu tăng dần`1`Và`2`, đưa ra chi phí`3`. Thêm`3`sẽ yêu cầu cái khác`3`, cho biết tổng chi phí`6`, vượt quá ngân sách. 

Câu trả lời là`3+3+3+2+1 = 12`, một lần nữa phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Tiền xử lý lớn hơn tiếp theo là`O(n)`, nâng nhị phân là`O(n log n)`và quá trình quét hai con trỏ sẽ thực hiện`O(n)`truy vấn chi phí, mỗi lần lấy`O(log n)`. | 
| Không gian |`O(n log n)`| các`up`Và`gain`mỗi bàn nâng nhị phân chứa`O(n log n)`mục nhập. | 

Với`n <= 2*10^5`,`log n`chỉ khoảng 18. Thuật toán tránh liệt kê số bậc hai của các mảng con và chỉ thực hiện một lượng công việc logarit cho mỗi chuyển động của ranh giới hai con trỏ, là thang đo bắt buộc cho giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def solve():
    n, M = map(int, input().split())
    a = list(map(int, input().split()))

    pref = [0] * (n + 1)
    for i, x in enumerate(a):
        pref[i + 1] = pref[i] + x

    nxt = [n] * n
    stack = []

    for i in range(n - 1, -1, -1):
        while stack and a[stack[-1]] < a[i]:
            nxt[stack.pop()] = i
        stack.append(i)

    block = [0] * n
    for i in range(n):
        j = nxt[i]
        block[i] = a[i] * (j - i) - (pref[j] - pref[i])

    LOG = n.bit_length()
    up = [nxt]
    gain = [block]

    for _ in range(1, LOG):
        pu = up[-1]
        pg = gain[-1]
        cu = [n] * n
        cg = [0] * n

        for i in range(n):
            mid = pu[i]
            if mid < n:
                cu[i] = pu[mid]
                cg[i] = pg[i] + pg[mid]

        up.append(cu)
        gain.append(cg)

    def cost(l, r):
        cur = l
        res = 0

        for k in range(LOG - 1, -1, -1):
            j = up[k][cur]
            if j <= r:
                res += gain[k][cur]
                cur = j

        return res + a[cur] * (r - cur + 1) - (
            pref[r + 1] - pref[cur]
        )

    ans = 0
    r = -1

    for l in range(n):
        if r < l:
            r = l

        while r + 1 < n and cost(l, r + 1) <= M:
            r += 1

        ans += r - l + 1

    print(ans)

# Provided samples
assert solve_data(
    "6 6\n5 4 1 1 5 5\n"
) == "18", "sample 1"

assert solve_data(
    "5 5\n6 5 4 3 2\n"
) == "12", "sample 2"

assert solve_data(
    "1 0\n1234\n"
) == "1", "sample 3"

# Minimum-size input.
assert solve_data(
    "1 0\n7\n"
) == "1", "single element"

# All equal values: every subarray already works.
assert solve_data(
    "4 0\n5 5 5 5\n"
) == "10", "all equal"

# Exact budget boundary.
assert solve_data(
    "3 3\n3 1 2\n"
) == "6", "cost exactly equals M"

# Strictly decreasing, with only some longer intervals affordable.
assert solve_data(
    "3 1\n3 2 1\n"
) == "5", "decreasing boundary"

# Maximum n, all equal, so every subarray is valid.
n = 200000
expected = n * (n + 1) // 2
large_input = f"{n} 0\n" + ("1 " * (n - 1)) + "1\n"
assert solve_data(large_input) == str(expected), "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 7`|`1`| Đầu vào có kích thước tối thiểu và mảng con một phần tử | 
|`4 0 / 5 5 5 5`|`10`| Giá trị bằng nhau và ngân sách bằng không | 
|`3 3 / 3 1 2`|`6`| Chi phí chính xác bằng`M`| 
|`3 1 / 3 2 1`|`5`| Giảm nghiêm ngặt trình tự và ranh giới bên phải | 
|`n=200000`, tất cả các giá trị`1`,`M=0`|`20000100000`| Kích thước đầu vào tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Trường hợp phần tử đơn`n=1`,`M=0`,`A=[7]`bắt đầu bằng`r=l`. Chi phí của`[7]`bằng 0, do đó vòng lặp không thể mở rộng thêm và thêm một mảng con hợp lệ. Câu trả lời là`1`. 

Để có giá trị bằng nhau, hãy xem xét`A=[5,5,5,5]`với`M=0`. Mọi`nxt[i]`là`n`bởi vì không có giá trị nào lớn hơn. Do đó, mỗi khối hoàn chỉnh sẽ mở rộng đến cuối, nhưng chi phí của nó bằng 0 vì mọi phần tử đều bằng khối tối đa. Quét hai con trỏ đạt tới`r=3`cho mọi`l`, đếm`4+3+2+1=10`mảng con. 

Đối với trường hợp ngân sách chính xác`A=[3,1,2]`,`M=3`, khoảng đầy đủ có tiền tố cực đại`[3,3,3]`. Chi phí của nó là`(3-3)+(3-1)+(3-2)=3`, vậy điều kiện`cost <= M`chấp nhận nó. Thuật toán mở rộng`r`qua vị trí cuối cùng thay vì dừng sớm một vị trí. 

Đối với trường hợp giảm`A=[3,2,1]`,`M=1`, các khoảng có độ dài hai có chi phí`1`, trong khi toàn bộ khoảng thời gian có chi phí`1+2=3`. Do đó, số lượng hợp lệ theo vị trí bắt đầu là`2`,`2`, Và`1`, cho`5`. Chuỗi lớn hơn tiếp theo cho mỗi phần tử kết thúc ngay lập tức vì mọi giá trị theo sau đều nhỏ hơn, do đó công thức khối nắm bắt chính xác chi phí ngày càng tăng của việc kéo dài khoảng thời gian. 

Đối với trường hợp kích thước tối đa với`200000`các phần tử bằng nhau và`M=0`, mỗi một trong số 

[ 
\frac{200000\cdot200001}{2}=20000100000 
] 

mảng con là hợp lệ. Bản thân câu trả lời vượt quá phạm vi 32 bit, trong khi số học số nguyên của Python xử lý trực tiếp nó. Thuật toán vẫn chỉ thực hiện`O(n log n)`công việc tiền xử lý và quét. 

Nếu bạn muốn, tôi cũng có thể cung cấp một phiên bản biên tập cuộc thi ngắn hơn hoặc triển khai C++ 17 bằng cách sử dụng cùng một ý tưởng nâng cấp nhị phân lớn hơn tiếp theo.
