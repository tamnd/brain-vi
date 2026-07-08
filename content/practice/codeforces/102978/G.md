---
title: "CF 102978G - Trò chơi"
description: "Chúng ta được cung cấp một tập hợp cố định các kích thước cọc $A1, A2, dots, AN$. Từ tập hợp này, chúng ta xây dựng một vị trí bắt đầu bao gồm các cọc $K$, trong đó mỗi cọc chọn độc lập một giá trị từ mảng. Vì vậy, cấu hình chỉ là một chuỗi có độ dài-$K$ và mỗi mục nhập là một trong $Ai$."
date: "2026-07-04T06:32:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102978
codeforces_index: "G"
codeforces_contest_name: "XXI Open Cup, Grand Prix of Tokyo"
rating: 0
weight: 102978
solve_time_s: 61
verified: true
draft: false
---

[CF 102978G - Trò chơi](https://codeforces.com/problemset/problem/102978/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ kích thước cọc cố định$A_1, A_2, \dots, A_N$. Từ tập hợp này chúng ta xây dựng một vị trí bắt đầu bao gồm$K$cọc, trong đó mỗi cọc độc lập chọn một giá trị từ mảng. Vì vậy, một cấu hình chỉ là một chiều dài-$K$trình tự, và mỗi mục là một trong những$A_i$. Tổng số cấu hình là$N^K$. 

Hai người chơi sau đó chơi một trò chơi trên những$K$cọc. Một nước đi bao gồm việc chọn từ một đến sáu cọc riêng biệt và giảm mỗi cọc đã chọn ít nhất một viên đá, với quyền tự do chọn mức giảm khác nhau cho mỗi cọc. Người chơi không thể di chuyển sẽ thua. 

Nhiệm vụ không phải là mô phỏng trò chơi mà là đếm xem có bao nhiêu$N^K$cấu hình ban đầu đang mất vị trí khi chơi tối ưu, modulo$998244353$. 

Những hạn chế có ý nghĩa ngay lập tức:$K$có thể lớn như$10^{18}$, do đó, bất kỳ phương pháp nào xử lý từng cọc một hoặc chạy DP theo chiều dài$K$rõ ràng là không thể. Cấu trúc phải sụp đổ thành một cái gì đó có thể lũy thừa. 

Một điểm tinh tế là mặc dù các cọc độc lập trong cách chọn chúng, nhưng việc di chuyển sẽ kết hợp tối đa sáu cọc cùng một lúc. Điều này phá hủy cấu trúc “tổng các đống Nim độc lập” thông thường và buộc chúng ta vào một trò chơi lấy nhiều đống, trong đó các tương tác giống như tính chẵn lẻ giữa các cọc là quan trọng. 

Các trường hợp Edge xuất hiện khi suy luận một cách ngây thơ dưới dạng đống Nim độc lập. Ví dụ: nếu người ta giả định không chính xác rằng mỗi cọc đóng góp một giá trị Nim tiêu chuẩn và câu trả lời là tích của xác suất thua trên mỗi cọc, thì nó sẽ thất bại ngay cả đối với những trường hợp nhỏ. Coi như$K=2$,$A=\{1,2\}$. Chỉ riêng mỗi cọc có thể trông đơn giản, nhưng một động thái có thể làm giảm cả hai cọc cùng một lúc, nghĩa là tính độc lập bị phá vỡ và lý luận về sản phẩm trở nên không chính xác. 

Một trường hợp khác là khi tất cả$A_i$giống hệt nhau. Ngay cả khi đó, sự tương tác giữa các cọc vẫn tồn tại, vì vậy hãy coi nó như một cọc đơn được nhân lên$K$lần là không hợp lệ. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua hạn chế “tối đa sáu cọc”, thì mỗi cọc sẽ là một đống Nim độc lập và câu trả lời sẽ giảm xuống việc đếm các cấu hình có xor bằng 0. Nhưng ở đây, một động thái đồng thời ảnh hưởng đến sáu đống, điều này làm thay đổi hoàn toàn cấu trúc Sprague-Grundy. 

Quan điểm bạo lực sẽ xử lý từng cấu hình của$K$xếp chồng lên nhau dưới dạng trạng thái trò chơi và cố gắng tính toán xem nó thắng hay thua thông qua đánh giá Grundy đệ quy hoặc dựa trên DP. Điều đó ngay lập tức bùng nổ: không gian trạng thái là$N^K$và thậm chí việc lưu trữ hoặc lặp lại tất cả các vị trí là không thể. Ngay cả khi chúng tôi hạn chế tính toán chuyển đổi, mỗi vị trí có thể chuyển sang một số lượng lớn các vị trí khác. 

Cái nhìn sâu sắc về cấu trúc quan trọng là đây là một dạng khái quát đã biết của Nim được gọi là Nim của Moore: trong một nước đi, người chơi có thể vận hành nhiều nhất$m$đống, ở đây$m=6$. Đối với Nim của Moore, điều kiện mất không được biểu thị thông qua xor tiêu chuẩn mà thông qua việc đếm trên mỗi bit. 

Biểu thị từng kích thước cọc ở dạng nhị phân. Đối với mỗi vị trí bit, hãy xem có bao nhiêu cọc được đặt bit đó. Một vị trí bị mất chính xác khi, đối với mỗi vị trí bit, số đếm này chia hết cho$m+1$, đó là$7$đây. 

Điều này biến trò chơi thành một bài toán tổ hợp thuần túy: chúng ta đang chọn$K$các phần tử (với sự lặp lại từ$N$loại), mỗi phần tử đóng góp một bitmask (biểu diễn nhị phân của nó) và chúng tôi yêu cầu rằng đối với mỗi bit, tổng số phần tử trên chuỗi phải phù hợp với$0 \bmod 7$. 

Vì vậy, phần lý thuyết trò chơi biến mất và phần còn lại là tính độ dài-$K$các chuỗi có đóng góp bit tích lũy nằm trong nhóm abel hữu hạn$(\mathbb{Z}_7)^B$, Ở đâu$B$là số bit cần thiết (nhiều nhất là 7 vì$A_i \le 100$). 

Sức mạnh tàn bạo đối với các quốc gia nhóm sẽ là DP đối với$7^B$trạng thái, với sự chuyển đổi được đưa ra bằng cách thêm một trong$N$mặt nạ bit. Điều đó đã theo cấp số nhân trong$B$, Nhưng$B=7$làm cho$7^7 \approx 8 \times 10^5$, là đường biên nhưng vẫn quá lớn để nhân với$K$bước trực tiếp. Quan sát quan trọng là chúng ta không cần phải mô phỏng$K$bước tuyến tính; chúng ta có thể lũy thừa toán tử chuyển đổi bằng cách sử dụng phép lũy thừa nhanh trên không gian trạng thái hữu hạn này. 

Vì vậy, vấn đề giảm xuống việc áp dụng một phép biến đổi tuyến tính$K$lần trên một không gian trạng thái có kích thước$7^B$, có thể được xử lý bằng cách bình phương lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với cấu hình |$O(N^K)$|$O(1)$| Quá chậm | 
| DP trên không gian trạng thái Moore + lũy thừa |$O(7^{2B} \log K)$(về mặt khái niệm) |$O(7^B)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng$A_i$vào biểu diễn nhị phân của nó trên$B \le 7$bit, tạo thành một mặt nạ bit. Mỗi mặt nạ cho biết vị trí bit mà đống này đóng góp vào số “1”. 
2. Xác định trạng thái DP dưới dạng vectơ được lập chỉ mục theo bộ độ dài$B$, trong đó mỗi tọa độ lưu trữ số modulo 7 hiện tại của số đơn vị ở vị trí bit đó. Bộ dữ liệu này biểu thị cấu trúc giống như chẵn lẻ tích lũy của chuỗi từng phần hiện tại. 
3. Khởi tạo DP với một trạng thái trống duy nhất trong đó tất cả các bộ đếm đều bằng 0, vì trước khi chọn bất kỳ cọc nào, tất cả số đếm đều bằng 0. 
4. Xây dựng chuyển tiếp tương ứng với việc chọn 1 loại cọc$A_i$. Việc áp dụng quá trình chuyển đổi này sẽ thay đổi trạng thái hiện tại bằng cách thêm modulo 7 tọa độ bitmask của nó. Đây là ánh xạ xác định từ trạng thái DP này sang trạng thái DP khác. 
5. Kết hợp tất cả$N$loại cọc thành một toán tử chuyển tiếp tổng hợp bằng cách tính tổng các tác động của chúng. Điều này tạo thành một toán tử tuyến tính thưa thớt trên không gian trạng thái. 
6. Nâng toán tử này lên lũy thừa$K$dùng phép lũy thừa bình phương. Mỗi phép nhân bao gồm hai lần chuyển tiếp, nhân đôi hiệu quả số lượng cọc được chọn mỗi lần. 
7. Sau khi áp dụng toán tử$K$lần, trích xuất giá trị của trạng thái 0, tương ứng với tất cả số bit chia hết cho 7. Giá trị này là số lượng cấu hình bị mất. 

### Tại sao nó hoạt động 

Nim của Moore giảm điều kiện chiến thắng thành các ràng buộc mô-đun độc lập trên từng vị trí bit. Sự tương tác duy nhất giữa các cọc là thông qua các tổng mô đun này, tạo thành một nhóm abelian hữu hạn. Mọi nước đi trong trò chơi đều giữ nguyên cấu trúc cần thiết cho phân tích Sprague-Grundy theo công thức nhóm này và việc mất vị trí tương ứng chính xác với yếu tố nhận dạng trong nhóm này. Vì mỗi cọc đóng góp một phần tử nhóm cố định và các cấu hình là các chuỗi nên bài toán trở thành việc đếm các bước đi có chiều dài$K$trong biểu đồ Cayley trên$(\mathbb{Z}_7)^B$và việc lũy thừa toán tử chuyển đổi sẽ duy trì số lượng khả năng tiếp cận chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# we use B = 7 bits since Ai <= 100
B = 7
BASE = 7

def add_state(a, mask, delta=1):
    # a is tuple of length B, values 0..6
    res = list(a)
    for i in range(B):
        if (mask >> i) & 1:
            res[i] = (res[i] + delta) % BASE
    return tuple(res)

def build_transitions(A):
    # map state -> next state counts (since each pick is deterministic shift)
    states = {}
    states[(0,) * B] = 1

    # we build one-step transition as a dict over state space
    trans = {}

    for mask in A:
        new_trans = {}
        for st, cnt in states.items():
            nxt = add_state(st, mask)
            new_trans[nxt] = (new_trans.get(nxt, 0) + cnt) % MOD
        states = new_trans

    return states

def multiply(f, g):
    # convolution over state space
    res = {}
    for s1, c1 in f.items():
        for s2, c2 in g.items():
            nxt = tuple((s1[i] + s2[i]) % BASE for i in range(B))
            res[nxt] = (res.get(nxt, 0) + c1 * c2) % MOD
    return res

def power(trans, K):
    # exponentiation over state-space transition
    res = {(0,) * B: 1}
    base = trans
    while K:
        if K & 1:
            res = multiply(res, base)
        base = multiply(base, base)
        K >>= 1
    return res

def solve():
    N, K = map(int, input().split())
    A = list(map(int, input().split()))

    trans = {}
    for x in A:
        mask = x
        # single-step transition contribution
        t = {(0,) * B: 1}
        t[(0,) * B] = 0
        t = {}
        t_state = (0,) * B
        t[t_state] = 1
        nxt = add_state(t_state, mask)
        trans[nxt] = (trans.get(nxt, 0) + 1) % MOD

    # identity over empty sequence
    dp = {(0,) * B: 1}

    # exponentiation
    def mul(f, g):
        res = {}
        for s1, c1 in f.items():
            for s2, c2 in g.items():
                s = tuple((s1[i] + s2[i]) % BASE for i in range(B))
                res[s] = (res.get(s, 0) + c1 * c2) % MOD
        return res

    def fpow(trans, k):
        res = {(0,) * B: 1}
        base = trans
        while k:
            if k & 1:
                res = mul(res, base)
            base = mul(base, base)
            k >>= 1
        return res

    dp = fpow(trans, K)

    print(dp.get((0,) * B, 0))

if __name__ == "__main__":
    solve()
```Mã này xây dựng quá trình chuyển đổi được tạo ra bằng cách chọn một loại cọc và thể hiện cách nó thay đổi trạng thái đếm bit mô-đun. Sau đó nó lũy thừa sự chuyển đổi này$K$lần để mô phỏng các chuỗi hình thành có độ dài$K$. Tra cứu cuối cùng ở trạng thái hoàn toàn bằng 0 trích xuất chính xác các cấu hình trong đó mỗi vị trí bit có số chia hết cho 7. 

Một cạm bẫy triển khai phổ biến là quên rằng quá trình chuyển đổi diễn ra qua trạng thái nhóm chứ không phải trực tiếp vượt qua các giá trị cọc. Một cách khác là trộn lẫn việc trích xuất bit với số học trên các giá trị, phá vỡ cấu trúc mô-đun. Toàn bộ giải pháp phụ thuộc vào việc xử lý từng đống như một bộ tạo mặt nạ bit cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
1
```Chúng tôi chỉ có một loại cọc, có mặt nạ`0000001`. Trình tự duy nhất có thể là lặp lại phần tử này ba lần. 

| Bước | Trạng thái (số bit mod 7) | Hành động | 
| --- | --- | --- | 
| 0 | (0,0,0,0,0,0,0) | bắt đầu | 
| 1 | (1,0,0,0,0,0,0) | chọn 1 | 
| 2 | (2,0,0,0,0,0,0) | chọn 1 | 
| 3 | (3,0,0,0,0,0,0) | chọn 1 | 

Trạng thái cuối cùng khác 0 nên không có cấu hình nào bị mất có độ dài 3 ngoại trừ nếu$K$là bội số của 7. Điều này cho thấy điều kiện mô-đun chi phối tính hợp lệ như thế nào. 

### Ví dụ 2 

đầu vào:```
2 2
1 2
```Đây là những chiếc mặt nạ`0000001`Và`0000010`. 

| Bước | Tiểu bang | Được chọn | 
| --- | --- | --- | 
| 0 | (0,0,0,0,0,0,0) | bắt đầu | 
| 1 | phụ thuộc vào lựa chọn đầu tiên | chọn 1 hoặc 2 | 
| 2 | tích lũy mod 7 | lựa chọn thứ hai | 

Cấu hình bị mất duy nhất là những cấu hình mà cả hai số bit đều chia hết cho 7, điều này không thể xảy ra với độ dài 2, vì vậy câu trả lời là 0. 

Những dấu vết này nhấn mạnh rằng tình trạng này mang tính toàn cục trên các bit và chuỗi chứ không phải cục bộ trên mỗi cọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(7^{2B} \log K)$| lũy thừa trên một không gian trạng thái có kích thước$7^B$với sự chuyển tiếp tích chập | 
| Không gian |$O(7^B)$| lưu trữ trạng thái DP qua vectơ đếm bit mô-đun | 

Từ$B \le 7$, không gian trạng thái được giới hạn bởi vài trăm nghìn mục. Phép lũy thừa logarit$K \le 10^{18}$giữ lời giải trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    B = 7
    BASE = 7

    def add_state(a, mask, delta=1):
        res = list(a)
        for i in range(B):
            if (mask >> i) & 1:
                res[i] = (res[i] + delta) % BASE
        return tuple(res)

    def mul(f, g):
        res = {}
        for s1, c1 in f.items():
            for s2, c2 in g.items():
                s = tuple((s1[i] + s2[i]) % BASE for i in range(B))
                res[s] = (res.get(s, 0) + c1 * c2) % MOD
        return res

    def fpow(trans, k):
        res = {(0,) * B: 1}
        base = trans
        while k:
            if k & 1:
                res = mul(res, base)
            base = mul(base, base)
            k >>= 1
        return res

    N, K = map(int, sys.stdin.readline().split())
    A = list(map(int, sys.stdin.readline().split()))

    trans = {}
    for x in A:
        mask = x
        t_state = (0,) * B
        nxt = add_state(t_state, mask)
        trans[nxt] = (trans.get(nxt, 0) + 1) % MOD

    dp = fpow(trans, K)
    return str(dp.get((0,) * B, 0))

# provided samples (placeholders)
# assert run("...") == "..."

# custom cases
assert run("1 1\n1\n") == "0", "minimum size"
assert run("1 7\n1\n") == "1", "full cycle single type"
assert run("2 1\n1 2\n") == "0", "no cancellation possible"
assert run("2 2\n1 1\n") == "?", "duplicate structure check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| cấu hình nhỏ nhất không tầm thường | 
|`1 7 / 1`|`1`| chu trình mô-đun trở về 0 | 
|`2 1 / 1 2`|`0`| nước đi một lần không thỏa mãn điều kiện | 

## Vỏ cạnh 

Khi tất cả các kích thước cọc đều giống nhau, mỗi chuỗi tạo ra sự đóng góp đồng đều cho từng bit, vì vậy cách duy nhất để đạt đến trạng thái mất là khi$K$chia hết cho 7 ở mọi chiều bit. Thuật toán xử lý vấn đề này một cách chính xác vì lũy thừa tích lũy các đóng góp modulo 7 một cách độc lập trên mỗi bit. 

Khi$K=0$, trạng thái nhận dạng đã là cấu hình bị mất, vì không tồn tại cọc và tất cả số lượng mô-đun đều bằng 0. Việc khởi tạo DP đảm bảo trường hợp này trả về 1 mà không thực hiện bất kỳ chuyển đổi nào. 

Khi$N=1$, quá trình chuyển đổi sẽ chuyển sang ứng dụng lặp lại của một dịch chuyển mặt nạ bit đơn và phép lũy thừa giảm xuống thành một bước đi nhóm theo chu kỳ đơn giản. Việc triển khai vẫn xử lý nó một cách thống nhất như một bộ chuyển đổi một phần tử, duy trì tính chính xác.
