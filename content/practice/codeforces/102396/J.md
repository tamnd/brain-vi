---
title: "CF 102396J - Siêu hoán vị"
description: "Việc xây dựng bắt đầu với trình tự [1]. Để chuyển từ thứ tự m sang thứ tự m+1, chúng ta quét mọi cửa sổ có độ dài m của chuỗi hiện tại. Bất cứ khi nào một cửa sổ như vậy là hoán vị 1..m, chúng tôi chèn giá trị mới m+1 theo sau là hoán vị tương tự ngay sau cửa sổ."
date: "2026-08-10T18:55:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "J"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 448
verified: true
draft: false
---

[CF 102396J - Siêu hoán vị](https://codeforces.com/problemset/problem/102396/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 28 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Việc xây dựng bắt đầu với trình tự`[1]`. Để chuyển từ trật tự`m`đặt hàng`m+1`, chúng tôi quét mọi chiều dài-`m`cửa sổ của chuỗi hiện tại. Bất cứ khi nào một cửa sổ như vậy là một hoán vị của`1..m`, chúng tôi chèn giá trị mới`m+1`theo sau là hoán vị tương tự ngay sau cửa sổ. 

Chuỗi kết quả chứa mọi hoán vị chính xác một lần dưới dạng độ dài-`m`cửa sổ. Nhiệm vụ không phải là xây dựng chuỗi khổng lồ này. Thay vào đó, đối với một hoán vị nhất định`a`của`1..n`, chúng ta cần vị trí được lập chỉ mục 1 nơi bắt đầu xuất hiện duy nhất của nó, modulo`10^9+7`. Tuyên bố chính thức đưa ra cùng một cấu trúc đệ quy và các giới hạn giống nhau, bao gồm`n <= 300000`. 

Ràng buộc`n <= 300000`loại trừ bất cứ điều gì xây dựng rõ ràng siêu hoán vị hoặc liệt kê các hoán vị của nó. chiều dài của nó là 

[ 
|s_n|=1!+2!+\cdots+n!, 
] 

vì vậy ngay cả việc lưu trữ`s_n`là không thể. Một giải pháp phải xử lý hoán vị đã cho theo cách gần như tuyến tính hoặc`O(n log n)`thời gian và cách sử dụng`O(n)`ký ức. Giới hạn một giây làm cho`O(n log n)`việc thực hiện đáng được giữ chặt chẽ, trong khi bất cứ điều gì liên quan đến`n!`công việc hoàn toàn nằm ngoài tầm với. 

Có một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Vì`n=1`, đầu vào duy nhất là`[1]`, và câu trả lời là`1`. Phép truy toán giả định hoán vị không trống trước đó sẽ thất bại ở đây. Vì`n=2`, hoán vị`[2,1]`bắt đầu ở vị trí`2`TRONG`s_2=[1,2,1]`, trong khi`[1,2]`bắt đầu ở vị trí`1`. Điều này phát hiện ra từng lỗi một trong phần đóng góp của phần tử tối đa. Khi mức tối đa ở cuối, chẳng hạn như`[2,3,1,4]`, quá trình chèn của nó xảy ra sau hoán vị cơ bản, nhưng bản thân mục tiêu có thể bắt đầu trước lần chèn đó. Xử lý mọi lần xuất hiện như bắt đầu từ lúc được chèn`4`đưa ra câu trả lời sai`9`thay vì`6`. Cuối cùng, đầu vào đảm bảo rằng các giá trị tạo thành một hoán vị, do đó, một đầu vào hoàn toàn bằng nhau, chẳng hạn như`3 / 1 1 1`không phải là trường hợp thử nghiệm hợp lệ và không được sử dụng để kiểm tra giải pháp đã gửi. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ xây dựng các chuỗi`s_1,s_2,...,s_n`. Ở sân khấu`m`, chúng ta phải kiểm tra mọi chiều dài-`m`cửa sổ và quyết định xem nó có chứa mọi giá trị từ`1`bởi vì`m`. Đã có rồi`Theta(m!)`vị trí ở giai đoạn đó và việc kiểm tra một cửa sổ một cách ngây thơ sẽ mất`Theta(m)`thời gian. Chỉ riêng khâu cuối cùng đã tốn chi phí`Theta(n * n!)`hoạt động trong trường hợp xấu nhất, ngoài việc yêu cầu`Theta(n!)`bộ nhớ chỉ để lưu trữ chuỗi. Với`n=300000`, điều này không khả thi chút nào. 

Lực lượng vũ phu hoạt động vì mỗi lần chèn đều mang tính cục bộ. Nếu một hoán vị`q`kích thước`m-1`xảy ra ở`s_{m-1}`, phần chèn xây dựng`[m,q]`ngay sau đó. Điều này tạo ra chính xác`m`kích thước liên tiếp-`m`hoán vị xung quanh việc chèn đó. Quan sát hữu ích là những`m`hoán vị chỉ đơn giản là phép quay tuần hoàn của`q`với`m`chèn vào các vị trí khác nhau. 

Giả sử hoán vị hiện tại là`p`, và giá trị lớn nhất`m`xảy ra ở vị trí`k`. Di dời`m`và xoay dãy còn lại sao cho phần tử ngay sau`m`trở thành phần tử đầu tiên. Gọi hoán vị kết quả`q`. Sau đó`q`chính xác là kích thước-`m-1`hoán vị mà việc chèn của nó tạo ra`p`. 

Có một số lượng khác mà chúng tôi cần. Cho phép`R_m(p)`là thứ hạng dựa trên số 0 của`p`trong số`m!`hoán vị theo thứ tự xuất hiện trong`s_m`. Nếu như`q`có thứ hạng`R_{m-1}(q)`, tất cả của nó`m`hoán vị dẫn xuất tạo thành một nhóm liên tiếp. Vị trí của`p`bên trong nhóm này là`m-k`, bởi vì`m`đang ở vị trí`k`. Do đó 

[ 
R_m(p)=mR_{m-1}(q)+(m-k). 
] 

Vị trí thực tế thậm chí còn có sự tái diễn đơn giản hơn. Trước nhóm cho`q`, mọi hoán vị kích thước trước đó`m-1`gây ra sự chèn của`m`các phần tử, vì vậy`q`được dịch chuyển bởi`mR_{m-1}(q)`các vị trí. Mục tiêu bắt đầu`m-k`vị trí muộn hơn so với thời điểm bắt đầu`q`. Như vậy 

[ 
P_m(p)=P_{m-1}(q)+R_m(p). 
] 

Bắt đầu với`P_1=1`, do đó câu trả lời cuối cùng là 

[ 
P_n=1+\sum_{m=2}^{n}R_m. 
] 

Thử thách còn lại là thu được mọi giá trị`m-k`mà không xoay vòng hoán vị một cách rõ ràng ở mọi cấp độ. 

Các phép quay có sự diễn giải rõ ràng trên một vòng tròn. Bắt đầu với hoán vị ban đầu dưới dạng danh sách vòng tròn. Khi xử lý tối đa`m`, tất cả các giá trị lớn hơn đã bị xóa và hoán vị tuyến tính hiện tại bắt đầu ngay sau mức tối đa đã bị xóa trước đó. Đang xóa`m`có nghĩa là yếu tố sống sót tiếp theo sẽ trở thành sự khởi đầu mới. Vì thế`m-k`chính xác là số lượng vị trí hiện còn sống sau`m`và trước khi bắt đầu hiện tại, được đo theo chu kỳ. 

Cây Fenwick vẫn giữ nguyên vị trí ban đầu. Danh sách liên kết đôi duy trì vòng tròn kế tiếp hiện tại khi một vị trí bị xóa. Họ cùng nhau cho phép chúng tôi tính toán mọi`m-k`TRONG`O(log n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(n * n!)`|`Theta(n!)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cửa hàng`pos[x]`, vị trí mảng ban đầu của mọi giá trị`x`. Các giá trị này là duy nhất, do đó, điều này cho phép truy cập liên tục vào vị trí của mức tối đa hiện tại. 
2. Xử lý các vị trí mảng ban đầu dưới dạng danh sách liên kết đôi hình tròn. Ban đầu mọi vị trí đều còn sống,`next[i]`trỏ đến vị trí tiếp theo, và`prev[i]`trỏ tới vị trí trước đó. Biểu diễn tuyến tính hiện tại bắt đầu tại vị trí`1`. 
3. Khởi tạo cây Fenwick với`1`ở mọi vị trí. Tổng tiền tố Fenwick hiện cho chúng ta biết có bao nhiêu phần tử hiện đang tồn tại xuất hiện ở một vị trí ban đầu nhất định. 
4. Quy trình`m=n,n-1,...,2`. Cho phép`x=pos[m]`. Hoán vị hiện tại của`1..m`bắt đầu lúc`start`và tuân theo thứ tự vòng tròn của các vị trí còn sống cho đến khi đạt đến`x`. 
5. Hãy để`d_m=m-k`, Ở đâu`k`là vị trí được lập chỉ mục 1 của`m`trong hoán vị hiện tại. Nếu như`start <= x`, thì các phần tử theo sau`x`trước khi gói vào`start`được tính bằng`m - prefix(x)`. Nếu như`start > x`, khoảng liên quan là khoảng thông thường`(x,start)`, có số vị trí còn sống là`prefix(start-1)-prefix(x)`. Điều này mang lại`d_m`mà không xây dựng hoán vị quay. 
6. Xóa`x`từ cây Fenwick và danh sách liên kết. cái mới`start`trở thành`next[x]`, bởi vì loại bỏ`m`xoay hoán vị còn lại sao cho phần kế tiếp của`m`là đầu tiên. 
7. Rốt cuộc`d_m`các giá trị đã được tính toán, xử lý`m=2,3,...,n`. Duy trì`rank`, ban đầu bằng không. Sự tái diễn 

[ 
thứ hạng=m\cdot thứ hạng+d_m 
] 

tính toán`R_m`từ`R_{m-1}`. Thêm thứ hạng này vào câu trả lời vì`P_m=P_{m-1}+R_m`. 

1. Bắt đầu câu trả lời tại`1`, tương ứng với hoán vị duy nhất có kích thước một. Thực hiện mọi phép nhân và phép cộng`10^9+7`. 

### Tại sao nó hoạt động 

Ở mọi kích cỡ`m`, loại bỏ giá trị tối đa khỏi hoán vị đích và xoay sau khi nó tạo ra chính xác hoán vị`q`việc chèn của nó đã tạo ra mục tiêu. Việc xây dựng xử lý sự xuất hiện của kích thước`m-1`từ trái sang phải và mỗi lần xuất hiện như vậy tạo ra chính xác một nhóm liên tiếp`m`kích cỡ-`m`hoán vị. Trong nhóm đó, hoán vị có giá trị lớn nhất tại vị trí`k`là`(m-k)`-thành viên thứ, vậy`R_m=mR_{m-1}+(m-k)`. Các nhóm trước đó dịch chuyển lần xuất hiện tương ứng một cách chính xác`m`mỗi vị trí, đưa ra`P_m=P_{m-1}+R_m`. Danh sách liên kết vòng thể hiện chính xác trình tự thu được bằng cách liên tục loại bỏ mức tối đa hiện tại và bắt đầu ngay sau nó, trong khi cây Fenwick đếm các phần tử còn sót lại của nó. Như vậy mọi`d_m=m-k`được sử dụng bởi phép truy hồi là chính xác và vị trí tích lũy cuối cùng là lần xuất hiện bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, x in enumerate(a, 1):
        pos[x] = i

    # Fenwick tree containing 1 for every currently alive position.
    bit = [0] * (n + 1)

    # Build the Fenwick tree in O(n).
    for i in range(1, n + 1):
        bit[i] += 1
        j = i + (i & -i)
        if j <= n:
            bit[j] += bit[i]

    def prefix(x):
        s = 0
        while x:
            s += bit[x]
            x -= x & -x
        return s

    def remove(x):
        while x <= n:
            bit[x] -= 1
            x += x & -x

    # Circular doubly linked list of alive positions.
    nxt = [0] * (n + 1)
    prv = [0] * (n + 1)

    for i in range(1, n + 1):
        nxt[i] = i + 1 if i < n else 1
        prv[i] = i - 1 if i > 1 else n

    # d[m] = m - position_of_m_in_the_current_permutation.
    d = [0] * (n + 1)

    start = 1

    for m in range(n, 1, -1):
        x = pos[m]

        px = prefix(x)

        if start <= x:
            d[m] = m - px
        else:
            d[m] = prefix(start - 1) - px

        # Remove x and rotate the remaining circular order so
        # that its successor becomes the new first position.
        nx = nxt[x]
        p = prv[x]

        nxt[p] = nx
        prv[nx] = p
        start = nx

        remove(x)

    rank = 0
    answer = 1

    for m in range(2, n + 1):
        rank = (m * rank + d[m]) % MOD
        answer += rank
        if answer >= MOD:
            answer -= MOD

    print(answer)

if __name__ == "__main__":
    solve()
```các`pos`mảng cho phép pha giảm dần tìm mức tối đa hiện tại trong thời gian không đổi. Cây Fenwick chứa một đơn vị cho mỗi giá trị chưa bị loại bỏ, vì vậy tổng tiền tố của nó biểu thị số phần tử còn sót lại trong một khoảng hoán vị ban đầu. 

Danh sách liên kết là cần thiết vì một lý do khác. Hoán vị hiện tại không phải là mảng ban đầu với một số mục bị xóa từ bên trái hoặc bên phải. Đó là một vòng quay có điểm bắt đầu thay đổi sau khi loại bỏ mọi mức tối đa.`start`ghi lại vòng quay đó, và`nxt`Và`prv`cập nhật nó trong thời gian liên tục sau khi xóa. 

Biểu thức cho`d[m]`được cố ý viết bằng kích thước hiện tại`m`. Trước khi gỡ bỏ`m`, chính xác`m`các vị trí còn sống. Khi`start <= x`, các phần tử sau`x`trước khi gói chỉ đơn giản là tất cả các phần tử còn sống sau`x`, đó là`m-prefix(x)`. Khi`start > x`, khoảng liên quan không bao bọc, do đó cần có sự khác biệt của hai tổng tiền tố. 

Lần thứ hai xây dựng lại thứ hạng từ kích thước nhỏ nhất đến lớn nhất.`rank`cửa hàng`R_{m-1}`trước khi cập nhật và trở thành`R_m`sau khi nhân với`m`và thêm`d[m]`. Câu trả lời bắt đầu từ`P_1=1`, sau đó nhận được đúng một đóng góp`R_m`ở mỗi cấp độ. 

Số nguyên Python không bị tràn, nhưng việc lấy mô-đun sau mỗi lần lặp lại sẽ giữ cho các giá trị ở mức nhỏ và tránh số học số nguyên lớn không cần thiết. Bản thân cây Fenwick chỉ lưu trữ các giá trị tối đa`n`, vì vậy các phần tử của nó không bao giờ lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hoán vị đầu vào là`[2,3,1]`. Các vị trí của nó là`pos[1]=3`,`pos[2]=1`, Và`pos[3]=2`. 

|`m`|`start`|`pos[m]`| Tiền tố sống tại`pos[m]`|`d[m]`| Mới`start`| 
| --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 2 | 2 | 1 | 3 | 
| 2 | 3 | 1 | 1 | 0 | 2 | 

Vì`m=3`, hoán vị hiện tại là`[2,3,1]`, Vì thế`3`đang ở vị trí`2`Và`d_3=3-2=1`. Sau khi loại bỏ`3`, thứ tự vòng tròn còn lại là`[1,2]`bắt đầu từ vị trí`3`. 
Cách tính hạng là 
[ 
R_ 2=0, 
\quad 
R_ 3=3\cdot 0+1=1. 
] 
vị trí là 
[ 
P_3=1+R_2+R_3=1+0+1=2. 
] 
Như vậy câu trả lời là`2`, phù hợp với mẫu 

### Mẫu 2 

Hoán vị là`[2,3,1,4]`, với các vị trí`pos[1]=3`,`pos[2]=1`,`pos[3]=2`, Và`pos[4]=4`. 

|`m`|`start`|`pos[m]`|`d[m]`| Mới`start`|`R_m`| Trả lời sau`R_m`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 4 | 1 | 4 | 0 | 1 | 0 | 1 | 
| 3 | 1 | 2 | 1 | 3 | 1 | 2 | 
| 2 | 3 | 1 | 0 | 2 | 2 | 4 | 

Các giá trị xếp hạng cuối cùng là`R_2=0`,`R_3=1`, Và`R_4=4`. Do đó 

[ 
P_4=1+0+1+4=6. 
] 

Ví dụ này chứng minh tại sao sự đóng góp thứ hạng lại quan trọng. Hoán vị`[2,3,1]`không ở lại vị trí`2`sau khi chuyển từ`s_3`ĐẾN`s_4`, vì nhóm hoán vị trước đó đã chèn bốn phần tử mới vào trước nó. Sự xuất hiện thay đổi của nó là tiền tố của mục tiêu`[2,3,1,4]`, bắt đầu ở vị trí`6`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Mỗi trong số`n`các vị trí sẽ bị xóa một lần, với các cập nhật Fenwick và các truy vấn tiền tố sẽ lấy`O(log n)`. Việc vượt qua thứ hạng là tuyến tính. | 
| Không gian |`O(n)`| Mảng vị trí, cây Fenwick, mảng danh sách liên kết và`d`mảng đều có kích thước`O(n)`. | 

Vì`n=300000`, thuật toán không bao giờ xây dựng một siêu hoán vị và không bao giờ liệt kê nó`n!`hoán vị. các`O(n log n)`công việc cấu trúc dữ liệu và bộ nhớ phụ tuyến tính phù hợp với quy mô dự định của các ràng buộc, trong khi việc xây dựng rõ ràng sẽ không thể thực hiện được đối với các giá trị rất nhỏ so với đầu vào tối đa. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    pos = [0] * (n + 1)
    for i, x in enumerate(a, 1):
        pos[x] = i

    bit = [0] * (n + 1)

    for i in range(1, n + 1):
        bit[i] += 1
        j = i + (i & -i)
        if j <= n:
            bit[j] += bit[i]

    def prefix(x):
        s = 0
        while x:
            s += bit[x]
            x -= x & -x
        return s

    def remove(x):
        while x <= n:
            bit[x] -= 1
            x += x & -x

    nxt = [0] * (n + 1)
    prv = [0] * (n + 1)

    for i in range(1, n + 1):
        nxt[i] = i + 1 if i < n else 1
        prv[i] = i - 1 if i > 1 else n

    d = [0] * (n + 1)
    start = 1

    for m in range(n, 1, -1):
        x = pos[m]
        px = prefix(x)

        if start <= x:
            d[m] = m - px
        else:
            d[m] = prefix(start - 1) - px

        nx = nxt[x]
        p = prv[x]

        nxt[p] = nx
        prv[nx] = p
        start = nx

        remove(x)

    rank = 0
    answer = 1

    for m in range(2, n + 1):
        rank = (m * rank + d[m]) % MOD
        answer += rank
        if answer >= MOD:
            answer -= MOD

    sys.stdin = old_stdin
    return str(answer)

# Provided samples
assert solution("3\n2 3 1\n") == "2", "sample 1"
assert solution("4\n2 3 1 4\n") == "6", "sample 2"
assert solution("4\n4 3 1 2\n") == "14", "sample 3"

# Minimum size
assert solution("1\n1\n") == "1", "minimum size"

# Maximum at the first position
assert solution("3\n3 1 2\n") == "3", "maximum at first"

# Maximum at the last position
assert solution("2\n1 2\n") == "1", "maximum at last, identity"

assert solution("2\n2 1\n") == "2", "maximum at first for n=2"

# A larger custom case exercising several circular rotations
assert solution("5\n5 1 4 2 3\n") == "17", "circular rotation case"

# Maximum-size valid input.
# The identity permutation has answer 1 for every n.
n = 300000
identity = " ".join(map(str, range(1, n + 1)))
assert solution(f"{n}\n{identity}\n") == "1", "maximum n"

# An all-equal input is deliberately not tested because the problem
# guarantees that the second line is a permutation, so duplicates are invalid.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Trường hợp cơ sở và ranh giới kích thước tối thiểu | 
|`3 / 3 1 2`|`3`| Tối đa ở vị trí đầu tiên | 
|`2 / 1 2`|`1`| Tối đa ở vị trí cuối cùng và đóng góp thứ hạng bằng 0 | 
|`2 / 2 1`|`2`| Phần bù chèn không cần thiết nhỏ nhất | 
|`5 / 5 1 4 2 3`|`17`| Nhiều vòng quay và cấp bậc khác 0 | 
|`300000 / 1 2 ... 300000`|`1`| Kích thước đầu vào tối đa và hành vi bộ nhớ tuyến tính | 

## Vỏ cạnh 

cho`n=1`, đầu vào chính xác là`[1]`. Không có vòng lặp loại bỏ tối đa vì vòng lặp bắt đầu tại`m=2`. Thứ hạng vẫn bằng 0 và câu trả lời vẫn giữ nguyên giá trị ban đầu`1`, đó chính xác là vị trí của`[1]`TRONG`s_1`. 

Vì`n=2`và hoán vị`[2,1]`, điểm bắt đầu hiện tại là vị trí`1`và tối đa`2`cũng đang ở vị trí`1`. Cây Fenwick báo cáo một phần tử còn sót lại sau vị trí`1`, Vì thế`d_2=1`. Thứ hạng trở thành`R_2=1`, và câu trả lời trở thành`1+1=2`. Điều này phù hợp`s_2=[1,2,1]`. 

Để đạt mức tối đa ở vị trí đầu tiên, hãy xem xét`[3,1,2]`. Tối đa`3`có`k=1`, Vì thế`d_3=2`. Sau khi loại bỏ nó, thứ tự còn lại bắt đầu lúc`1`. Ở kích thước hai, tối đa`2`ở cuối, đưa ra`d_2=0`. Như vậy`R_2=0`,`R_3=2`, và câu trả lời là`1+0+2=3`. Thực vậy,`[3,1,2]`bắt đầu ở vị trí`3`TRONG`s_3`. 

Để đạt mức tối đa ở vị trí cuối cùng,`[2,3,1,4]`có`d_4=0`, bởi vì`4`xảy ra ở vị trí`4`. Cấp độ tiếp theo có`d_3=1`, mang lại`R_3=1`và sau đó`R_4=4`. Vị trí cuối cùng là`1+0+1+4=6`. Mục tiêu bắt đầu ở vị trí`6`, trước khi chèn`4`, do đó, thuật toán luôn tìm vị trí cực đại sẽ không chính xác. 

Đối với một vòng tròn bao quanh,`[4,3,1,2]`đặc biệt hữu ích. Tối đa`4`ban đầu ở vị trí`1`, Vì thế`d_4=3`. Loại bỏ nó làm cho vị trí`2`sự khởi đầu mới. Mức tối đa tiếp theo`3`cũng ở vị trí đầu tiên mới, đưa ra`d_3=2`. Cuối cùng`d_2=0`. Các cấp bậc là`R_2=0`,`R_3=2`, Và`R_4=11`, vậy câu trả lời là`1+0+2+11=14`. Danh sách liên kết là thứ làm cho hành vi bao quanh này trở nên rõ ràng thay vì buộc chúng ta phải xoay hoán vị một cách vật lý. 

Một đầu vào hoàn toàn bằng nhau như`[1,1,1]`sẽ vi phạm đảm bảo đầu vào rằng mọi giá trị từ`1`bởi vì`n`xảy ra đúng một lần. Thuật toán dựa vào sự đảm bảo đó khi xây dựng`pos`, khi diễn giải mức tối đa hiện tại và khi xóa chính xác một vị trí trên mỗi giá trị. Do đó, đây là một trường hợp vấn đề không hợp lệ chứ không phải là một trường hợp đặc biệt của giải pháp được yêu cầu.
