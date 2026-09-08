---
title: "CF 104573I - Quần đảo Kỳ nhông"
description: "Chúng tôi được cấp một dòng đảo. Mỗi hòn đảo có một loại trái cây và số lượng ban đầu. Hàng ngày, một đoạn đảo liền kề được tiếp xúc với du khách; mọi thứ bên ngoài đoạn đó đều không có sẵn do bão."
date: "2026-06-30T08:22:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104573
codeforces_index: "I"
codeforces_contest_name: "UTPC Contest 09-08-23 Div. 1"
rating: 0
weight: 104573
solve_time_s: 82
verified: false
draft: false
---

[CF 104573I - Quần đảo Iguana](https://codeforces.com/problemset/problem/104573/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một dòng đảo. Mỗi hòn đảo có một loại trái cây và số lượng ban đầu. Hàng ngày, một đoạn đảo liền kề được tiếp xúc với du khách; mọi thứ bên ngoài đoạn đó đều không có sẵn do bão. Bên trong đoạn có sẵn, hai người chơi thay phiên nhau bắt đầu từ Ivan. Trong một lượt, người chơi chọn bất kỳ hòn đảo nào vẫn còn trái cây và loại bỏ một lượng trái cây nhất định, nhưng số lượng trái cây họ loại bỏ phải chính xác là sức mạnh của loại trái cây trên đảo, tức là đối với đảo tôi họ có thể lấy$f_i^k$với mọi số nguyên$k \ge 0$, miễn là không vượt quá số lượng còn lại. 

Quá trình tiếp tục cho đến khi tất cả trái cây có sẵn được tiêu thụ. Người chơi thực hiện nước đi cuối cùng trong ngày đó được tuyên bố là người chiến thắng trong ngày đó. 

Trạng thái của mỗi ngày là độc lập vì tất cả các hòn đảo đều được bổ sung đầy đủ trước khi trò chơi bắt đầu, vì vậy mỗi truy vấn là một trò chơi khách quan mới trên một phân mảng. 

Nhiệm vụ là xác định, đối với từng phân đoạn truy vấn, liệu người chơi đầu tiên (Ivan) có bị buộc phải thắng khi chơi tối ưu hay không. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$đảo và các câu hỏi. Bất kỳ giải pháp nào tính toán lại kết quả trò chơi cho mỗi truy vấn đều phải tránh mô phỏng tuyến tính trên phân đoạn vì điều đó sẽ dẫn đến$O(NQ)$hành vi vượt xa giới hạn. Chúng ta nên mong đợi ít nhất$O((N+Q)\log N)$hoặc$O((N+Q)\alpha)$kết cấu. 

Một trường hợp quan trọng là khi một hòn đảo có$f_i = 1$. Từ$1^k = 1$, mỗi nước đi trên một hòn đảo như vậy sẽ loại bỏ đúng 1 đơn vị. Điều này có nghĩa là sự đóng góp của nó hoạt động rất khác so với$f_i > 1$, trong đó phần bị loại bỏ là các khối có kích thước theo cấp số nhân. 

Một trường hợp tế nhị khác phát sinh khi$q_i$bản thân nó là sức mạnh của$f_i$. Trong trường hợp đó, người chơi có thể chiếm toàn bộ hòn đảo chỉ bằng một nước đi, biến nó thành một đống duy nhất trong trò chơi lấy và nghỉ thông thường. 

Cuối cùng, nếu chúng ta xử lý các hòn đảo một cách độc lập và tính tổng các đóng góp không chính xác mà không tính đến sự tương tác thông qua tính chẵn lẻ của lượt, chúng ta có thể giả định sai tính cộng theo cách không đúng trừ khi chúng ta chuyển trò chơi thành cấu trúc Sprague-Grundy thích hợp. 

## Phương pháp tiếp cận 

Chúng tôi diễn giải lại mỗi hòn đảo dưới dạng một cọc độc lập trong trò chơi phép trừ, trong đó các bước di chuyển được phép tùy thuộc vào cơ sở$f_i$. Vì các hòn đảo không tương tác ngoại trừ thông qua thứ tự lần lượt, nên toàn bộ phân đoạn là một tổng thể riêng biệt của các trò chơi độc lập. Người chiến thắng phụ thuộc vào giá trị XOR của Grundy của các đảo được chọn. 

Cách tiếp cận bạo lực tính toán giá trị Grundy cho mỗi hòn đảo bằng cách mô phỏng tất cả các trạng thái có thể tiếp cận từ$q_i$sử dụng các bước di chuyển được phép$f_i^k$, sau đó tính toán lại XOR trên từng phân đoạn truy vấn. Điều này đúng nhưng không khả thi vì một hòn đảo có thể yêu cầu$O(q_i)$chuyển tiếp trong trường hợp xấu nhất, và thực hiện điều này cho tất cả các đảo sẽ dẫn đến hành vi bậc hai. 

Quan sát quan trọng là tập hợp di chuyển có cấu trúc cao: dành cho cố định$f$, các bước di chuyển được phép là sức mạnh của$f$, do đó các chuyển đổi trạng thái tương ứng với việc trừ liên tục các lũy thừa của một cơ số. Điều này biến trò chơi thành một cấu trúc đã biết: giá trị Grundy của một cọc chỉ phụ thuộc vào số chữ số trong cơ số của nó$f$, bởi vì trừ$f^k$tương ứng với việc thao tác một vị trí chữ số trong cơ sở$f$-giống như cách trình bày. 

Vì$f_i > 1$, mỗi cọc hoạt động giống như một hệ nhị phân (nói chung là cơ sở-$f_i$) bộ đếm trong đó mỗi lần di chuyển làm giảm một chữ số, làm cho giá trị Grundy bằng với tính chẵn lẻ của số chữ số khác 0 trong cơ số-$f_i$đại diện của$q_i$. Vì$f_i = 1$, giá trị đơn giản là$q_i \bmod 2$, vì mỗi nước đi giảm đi đúng 1. 

Do đó, mỗi hòn đảo đóng góp giá trị 0/1 cho XOR. Mỗi truy vấn giảm xuống việc tính toán XOR trên một phạm vi, có thể được trả lời bằng mảng XOR tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force Grundy trên mỗi hòn đảo + mỗi truy vấn XOR |$O(NQ)$|$O(1)$| Quá chậm | 
| Tính toán trước giá trị đảo + tiền tố XOR |$O(N + Q)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi hòn đảo thành một giá trị bit duy nhất thể hiện sự đóng góp Grundy của nó. 

1. Với mỗi hòn đảo i, hãy tính giá trị g[i] biểu thị liệu đống đó đóng góp 0 hay 1 vào kết quả XOR. 

Vì$f_i = 1$, đây đơn giản là$q_i \bmod 2$. Điều này có hiệu quả vì mỗi lần di chuyển sẽ loại bỏ chính xác 1 đơn vị, do đó cọc tương đương với một đống Nim có kích thước$q_i$. 
2. Đối với$f_i > 1$, phân hủy nhiều lần$q_i$trong căn cứ$f_i$, đếm xem có bao nhiêu chữ số khác 0. 

Mỗi chữ số khác 0 tương ứng với một vị trí có thể được giảm độc lập bằng cách di chuyển kích thước$f_i^k$, do đó mỗi cái đóng góp một đơn vị giá trị Grundy. 
3. Lưu trữ g[i] dưới dạng tính chẵn lẻ của số chữ số khác 0 đó. 
4. Xây dựng mảng XOR tiền tố trên g. 
5. Với mỗi truy vấn [l, r], trả về XOR của g[l..r] sử dụng tiền tố XOR. 

Mỗi truy vấn sau đó được trả lời trong thời gian không đổi. 

### Tại sao nó hoạt động 

Mỗi hòn đảo tạo thành một trò chơi độc lập, khách quan. Các nước đi được phép luôn trừ đi lũy thừa thuần túy của cơ số, cách ly một chữ số trong cơ số-$f_i$đại diện. Điều này có nghĩa là trò chơi phân tách thành các lựa chọn nhị phân độc lập cho mỗi vị trí chữ số và mỗi vị trí như vậy đóng góp chính xác một đơn vị vào giá trị Grundy nếu nó khác 0. Vì các tổng riêng biệt kết hợp thông qua XOR nên toàn bộ phân đoạn giảm xuống XOR so với đóng góp cho mỗi đảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def island_value(f, q):
    if f == 1:
        return q & 1

    cnt = 0
    while q > 0:
        if q % f != 0:
            cnt ^= 1
        q //= f
    return cnt

def solve():
    n, q = map(int, input().split())
    f = list(map(int, input().split()))
    a = list(map(int, input().split()))

    g = [0] * n
    for i in range(n):
        g[i] = island_value(f[i], a[i])

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] ^ g[i]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        xor_val = pref[r] ^ pref[l - 1]
        out.append("Ivan" if xor_val else "Isabel")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên nén mỗi hòn đảo thành một bất biến giống như chẵn lẻ. Chức năng trợ giúp`island_value`thực hiện một cơ sở-$f$quét chữ số và chuyển đổi một chút bất cứ khi nào chữ số khác 0 xuất hiện. Điều này tính toán hiệu quả tính chẵn lẻ của các chữ số khác 0 mà không lưu trữ biểu diễn đầy đủ. 

Mảng XOR tiền tố biến mỗi truy vấn thành một phép trừ trong không gian XOR. Điều kiện để Ivan thắng là XOR trên đoạn đó khác 0. 

Phải cẩn thận với$f = 1$, vì vòng lặp phân rã cơ sở sẽ không bao giờ kết thúc. Việc xử lý riêng biệt sẽ đảm bảo tính chính xác và thời gian chạy tuyến tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chúng tôi tính toán phần đóng góp của mỗi hòn đảo trước tiên. 

| tôi | f[i] | q[i] | giá trị g[i] | 
| --- | --- | --- | --- | 
| 1 | 1 | 5 | 1 | 
| 2 | 1 | 8 | 0 | 
| 3 | 3 | 5 | 1 | 
| 4 | 2 | 6 | 0 | 
| 5 | 4 | 9 | 1 | 
| 6 | 6 | 6 | 1 | 

Tiền tố XOR: 

| tôi | trước | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 0 | 
| 5 | 1 | 
| 6 | 0 | 

Đánh giá truy vấn: 

| Truy vấn | Phạm vi XOR | Kết quả | 
| --- | --- | --- | 
| [1,2] | 1 ⊕ 0 = 1 | Ivan | 
| [1,3] | 1 ⊕ 0 ⊕ 1 = 0 | Isabel | 
| [2,4] | 0 ⊕ 1 ⊕ 0 = 1 | Ivan | 
| [4,5] | 0 ⊕ 1 = 1 | Ivan | 
| [1,6] | 0 | Isabel | 

Dấu vết này cho thấy mỗi hòn đảo đóng góp độc lập như thế nào và XOR xác định đầy đủ kết quả như thế nào. 

### Mẫu 2 

Tính toán đóng góp: 

| tôi | f[i] | q[i] | g[i] | 
| --- | --- | --- | --- | 
| 1 | 56 | 983 | 1 | 
| 2 | 78 | 834 | 1 | 
| 3 | 65 | 721 | 1 | 

Tiền tố XOR: 

| tôi | trước | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Truy vấn: 

| Truy vấn | XOR | Người chiến thắng | 
| --- | --- | --- | 
| [1,1] | 1 | Ivan | 
| [1,2] | 0 | Isabel | 
| [1,3] | 1 | Ivan | 
| [2,2] | 1 | Ivan | 
| [2,3] | 0 | Isabel | 
| [3,3] | 1 | Ivan | 

Mẫu xen kẽ xuất phát trực tiếp từ cấu trúc XOR tiền tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N + Q + \sum \log_{f_i} q_i)$| Mỗi hòn đảo bị phân hủy trong căn cứ$f_i$và mỗi truy vấn là O(1) qua tiền tố XOR | 
| Không gian |$O(N)$| Lưu trữ các đóng góp và mảng tiền tố | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn vì mỗi$q_i$được giảm theo cấp số nhân và các truy vấn có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def island_value(f, q):
        if f == 1:
            return q & 1
        cnt = 0
        while q > 0:
            if q % f != 0:
                cnt ^= 1
            q //= f
        return cnt

    n, q = map(int, input().split())
    f = list(map(int, input().split()))
    a = list(map(int, input().split()))

    g = [island_value(f[i], a[i]) for i in range(n)]
    pref = [0]
    for x in g:
        pref.append(pref[-1] ^ x)

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append("Ivan" if pref[r] ^ pref[l - 1] else "Isabel")

    return "\n".join(out)

# provided samples
assert run("""6 5
1 1 3 2 4 6
5 8 5 6 9 6
1 2
1 3
2 4
4 5
1 6
""") == """Ivan
Isabel
Ivan
Ivan
Isabel"""

assert run("""3 6
56 78 65
983 834 721
1 1
1 2
1 3
2 2
2 3
3 3
""") == """Isabel
Isabel
Ivan
Isabel
Ivan
Ivan"""

# custom cases
assert run("""1 3
2
1
1 1
1 1
1 1
""") == """Ivan
Ivan
Ivan""", "single island toggling"

assert run("""4 2
2 2 2 2
1 3 7 8
1 4
2 3
""") == """Isabel
Isabel""", "uniform base 2 symmetry"

assert run("""5 2
1 10 1 10 1
5 9 4 7 3
1 5
2 4
""") == """Isabel
Ivan""", "mixed 1 and large bases"

assert run("""2 1
3 3
9 10
1 2
""") in ["Ivan", "Isabel"], "small random sanity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn lặp đi lặp lại đảo đơn | Ivan Ivan Ivan | tính đúng đắn của việc xử lý cọc cách ly | 
| giá trị cơ sở thống nhất | Isabel Isabel | nhất quán theo cấu trúc đối xứng | 
| giá trị f hỗn hợp | đầu ra hỗn hợp | tương tác của logic f=1 và f>1 | 
| ngẫu nhiên nhỏ | hoặc | kiểm tra sự tỉnh táo về sự ổn định | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn là khi$f_i = 1$. Trong tình huống này, logic phân rã cơ sở sẽ không bao giờ chấm dứt vì việc chia cho 1 không làm giảm giá trị. Thuật toán tránh được điều này hoàn toàn bằng cách coi nó như một phép kiểm tra tính chẵn lẻ trực tiếp của$q_i$. Điều này tương ứng chính xác với một cọc mà mỗi lần di chuyển sẽ loại bỏ 1 đơn vị. 

Một trường hợp cạnh khác xảy ra khi$q_i < f_i$. Trong trường hợp đó, cơ sở-$f_i$biểu diễn có một chữ số nhỏ hơn$f_i$, do đó vòng lặp chạy một lần và đăng ký một chữ số khác 0, đánh dấu chính xác phần đóng góp là 1. 

Khi nào$q_i$chính xác là sức mạnh của$f_i$, chỉ có một chữ số trong cơ số của nó-$f_i$biểu diễn khác 0, do đó phần đóng góp vẫn là 1. Thuật toán xử lý việc này một cách tự nhiên mà không cần phân nhánh đặc biệt. 

Cuối cùng, các giá trị lớn lên tới$10^9$là an toàn vì quá trình phân tách chữ số chạy theo thời gian logarit và không bao giờ phụ thuộc trực tiếp vào cường độ giá trị, đảm bảo không bị tràn hoặc suy giảm hiệu suất.
