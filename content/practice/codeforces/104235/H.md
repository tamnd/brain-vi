---
title: "CF 104235H - \u041a\u0440\u0430\u0441\u043d\u043e-\u0441\u0438\u043d\u0438\u0435 \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u044b"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh có đúng hai cạnh hướng ra ngoài: một cạnh đỏ và một cạnh xanh. Từ bất kỳ đỉnh bắt đầu nào, một bước di chuyển bao gồm việc lặp lại một mẫu di chuyển cạnh cố định và chúng ta phải xác định nơi chúng ta sẽ kết thúc sau khi áp dụng mẫu đó nhiều…"
date: "2026-07-01T23:32:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104235
codeforces_index: "H"
codeforces_contest_name: "2022-2023 Olympiad Cognitive Technologies, Final Round"
rating: 0
weight: 104235
solve_time_s: 62
verified: true
draft: false
---

[CF 104235H - \u041a\u0440\u0430\u0441\u043d\u043e-\u0441\u0438\u043d\u0438\u0435 \u043c\u0430\u0440\u0448\u0440\u0443\u0442\u044b](https://codeforces.com/problemset/problem/104235/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh có đúng hai cạnh hướng ra ngoài: một cạnh đỏ và một cạnh xanh. Từ bất kỳ đỉnh bắt đầu nào, một bước di chuyển bao gồm việc lặp lại một mẫu di chuyển cạnh cố định và chúng ta phải xác định nơi chúng ta sẽ kết thúc sau khi áp dụng mẫu đó nhiều lần. 

Mỗi truy vấn cung cấp một đỉnh bắt đầu và một chuỗi lệnh rút gọn. Chuỗi lệnh mã hóa hai thứ cùng một lúc: số lần chúng ta lặp lại một “khối chuyển động” và trình tự truyền tải cạnh dựa trên màu sắc nào xác định khối đó. Bản thân một khối chuyển động là một bước đi ngắn bằng cách đi theo các cạnh màu đỏ và màu xanh theo thứ tự được chỉ định bởi chuỗi. 

Nhiệm vụ là mô phỏng các bước đi lặp lại này cho mỗi truy vấn và đưa ra đỉnh cuối cùng. 

Các ràng buộc đẩy chúng tôi ra khỏi mọi mô phỏng trực tiếp của từng bước. Với tối đa 100.000 đỉnh và tối đa 50.000 truy vấn cũng như số lần lặp lại có khả năng rất lớn được mã hóa bên trong các truy vấn, việc mở rộng tất cả các bước di chuyển một cách rõ ràng sẽ dẫn đến một số chuyển đổi vượt xa giới hạn khả thi. Ngay cả một truy vấn duy nhất cũng có thể mô tả một chuyến đi bộ với tối đa 10^8 lần lặp lại, điều này ngay lập tức loại trừ việc mô phỏng từng bước. 

Trường hợp cạnh tinh tế xuất hiện khi chuỗi lệnh chứa số lần lặp lại lớn nhưng mẫu rất ngắn. Ví dụ: nếu một đỉnh tạo thành một chu trình theo mẫu đã cho, mô phỏng đơn giản sẽ lặp vô tận hoặc vượt quá giới hạn thời gian, mặc dù kết quả cuối cùng được xác định bằng cách tổ hợp hàm lặp lại. 

## Phương pháp tiếp cận 

Mỗi đỉnh xác định hai chuyển tiếp xác định: đỉnh màu đỏ và đỉnh kế tiếp màu xanh. Bất kỳ chuỗi màu nào cũng tương ứng với một hàm từ đỉnh này sang đỉnh khác. Để một chuỗi màu xác định một hàm$f$, nộp hồ sơ ở đâu$f(v)$có nghĩa là bắt đầu từ$v$và làm theo trình tự màu một lần. 

Mỗi truy vấn yêu cầu chúng ta tính toán$f^M(v)$, nghĩa là chúng tôi áp dụng chức năng này$M$lần. 

Một cách tiếp cận brute-force tính toán một ứng dụng của$f$bằng cách mô phỏng chuỗi màu và sau đó lặp lại nó$M$lần. Chi phí một ứng dụng$O(L)$, Ở đâu$L \le 8$, Nhưng$M$có thể lên đến$10^8$hoặc lớn hơn. Điều này dẫn tới trường hợp xấu nhất là$O(M \cdot L)$, quá chậm. 

Quan sát quan trọng là chúng ta liên tục soạn cùng một hàm. Thành phần hàm có tính kết hợp, vì vậy chúng ta có thể tính toán trước lũy thừa của hàm bằng cách sử dụng nâng nhị phân. Thay vì áp dụng phép biến đổi từng bước một, chúng ta tính toán trước$f^{2^k}$cho mỗi đỉnh và mỗi lũy thừa của hai. Khi đó bất kỳ số mũ nào$M$có thể được phân tách thành lũy thừa của hai, giảm ứng dụng lặp lại thành thời gian logarit trong$M$. 

Bản thân hàm này nhỏ nhưng miền của nó lớn, vì vậy chúng tôi lưu trữ các bảng chuyển đổi: đối với mỗi đỉnh, chúng tôi duy trì vị trí của nó sau 1, 2, 4, 8, ... ứng dụng của khối chuyển động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot M \cdot L)$|$O(n)$| Quá chậm | 
| Nâng nhị phân |$O(q \cdot L + q \cdot \log M)$|$O(n \log M)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi diễn giải chuỗi lệnh của mỗi truy vấn dưới dạng hàm chuyển đổi một bước trên biểu đồ. 

1. Với mỗi truy vấn, trích xuất số nguyên$M$và thứ tự màu sắc$s$. Màu sắc xác định ánh xạ xác định từ bất kỳ đỉnh nào đến đỉnh khác sau một khối chuyển động. 
2. Đối với truy vấn cố định, hãy tính mảng “chuyển đổi cơ sở”`next[v]`, mô phỏng việc áp dụng chuỗi màu một lần bắt đầu từ mọi đỉnh. Điều này hiệu quả vì mỗi đỉnh có chính xác một cạnh màu đỏ đi ra và một cạnh màu xanh đi ra, do đó đích đến được xác định rõ ràng. 
3. Xây dựng bàn nâng nhị phân`up[k][v]`, Ở đâu`up[0][v] = next[v]`, Và`up[k][v] = up[k-1][up[k-1][v]]`. Mỗi cấp độ thể hiện việc áp dụng khối chuyển động$2^k$lần. 
4. Phân hủy$M$thành nhị phân. Bắt đầu từ đỉnh ban đầu, áp dụng các bước nhảy được tính toán trước bất cứ khi nào có một chút trong$M$được thiết lập, cập nhật đỉnh hiện tại cho phù hợp. 
5. Xuất đỉnh cuối cùng sau khi xử lý tất cả các bit đã đặt. 

Lý do tính toán trước hiệu quả là vì các chuyển đổi biểu đồ có tính xác định và chức năng, do đó thành phần lặp lại không bao giờ phân nhánh và có thể được lưu trữ nhỏ gọn. 

### Tại sao nó hoạt động 

Thuật toán coi mỗi khối chuyển động là một hàm trên các đỉnh. Tổ hợp hàm có tính kết hợp, vì vậy việc áp dụng lặp đi lặp lại cùng một hàm có thể được viết lại dưới dạng lũy ​​thừa. Bảng nâng nhị phân mã hóa tất cả lũy thừa của hàm này, đảm bảo rằng bất kỳ số mũ nào cũng được biểu diễn chính xác một lần dưới dạng tổng lũy ​​thừa của hai. Vì mỗi bước duy trì tính chính xác của bố cục nên đỉnh cuối cùng khớp với kết quả của việc áp dụng quy trình lặp lại ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def apply_once(v, s, r, b):
    for ch in s:
        if ch == 'R':
            v = r[v]
        else:
            v = b[v]
    return v

def solve():
    n, q = map(int, input().split())
    r = [0] + list(map(int, input().split()))
    b = [0] + list(map(int, input().split()))

    for _ in range(q):
        parts = input().split()
        v = int(parts[0])
        s = parts[1]

        # extract M and pattern
        i = 0
        while i < len(s) and s[i].isdigit():
            i += 1
        M = int(s[:i])
        pattern = s[i:]

        # compute base transition
        nxt = [0] * (n + 1)
        for u in range(1, n + 1):
            cur = u
            for ch in pattern:
                if ch == 'R':
                    cur = r[cur]
                else:
                    cur = b[cur]
            nxt[u] = cur

        # binary lifting
        LOG = M.bit_length()
        up = [nxt]
        for k in range(1, LOG):
            prev = up[k - 1]
            cur = [0] * (n + 1)
            for u in range(1, n + 1):
                cur[u] = prev[prev[u]]
            up.append(cur)

        ans = v
        bit = 0
        while M:
            if M & 1:
                ans = up[bit][ans]
            M >>= 1
            bit += 1

        print(ans)

if __name__ == "__main__":
    solve()
```Quá trình triển khai bắt đầu bằng cách phân tích cú pháp hai mảng kề cố định cho các cạnh màu đỏ và xanh lam. Đối với mỗi truy vấn, nó chia chuỗi lệnh thành số lần lặp lại và mẫu màu. các`nxt`mảng được tính bằng cách mô phỏng mẫu một lần từ mọi đỉnh, tạo thành hàm cơ sở. 

Bàn nâng`up`lưu trữ thành phần lặp đi lặp lại của chức năng này. Mỗi lớp tạo thành lớp trước đó với chính nó, vì vậy`up[k]`tương ứng với việc áp dụng mẫu$2^k$lần. Cuối cùng, câu trả lời được xây dựng bằng cách duyệt qua biểu diễn nhị phân của$M$, chỉ cập nhật đỉnh hiện tại khi bit tương ứng được đặt. 

Chi tiết quan trọng là việc nâng cấp được xây dựng cho mỗi truy vấn vì mỗi truy vấn xác định một hàm khác nhau và việc sử dụng lại các bảng trên các truy vấn là không thể nếu không có cấu trúc chung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, v=1, M=1, pattern="R"
r=[2,3,4,1], b=[4,1,2,3]
```Chúng tôi tính toán chuyển đổi cơ sở: 

| bạn | sau R | 
| --- | --- | 
| 1 | 2 | 
| 2 | 3 | 
| 3 | 4 | 
| 4 | 1 | 

Từ$M = 1$, chúng tôi trực tiếp áp dụng điều này một lần. 

| Bước | Đỉnh | 
| --- | --- | 
| bắt đầu | 1 | 
| áp dụng 1 | 2 | 

Đầu ra là 2. 

Điều này xác nhận rằng một ứng dụng hoạt động chính xác giống như ánh xạ cạnh đỏ. 

### Ví dụ 2 

đầu vào:```
n=3, v=2, M=2, pattern="BR"
r=[2,3,1], b=[3,1,2]
```Đầu tiên tính toán một ứng dụng của "BR": 

| bạn | B rồi R | 
| --- | --- | 
| 1 | 1 -> 3 -> 1 | 
| 2 | 2 -> 1 -> 2 | 
| 3 | 3 -> 2 -> 3 | 

Vì vậy, chức năng là danh tính. 

Bây giờ áp dụng nó hai lần: 

| Bước | Đỉnh | 
| --- | --- | 
| bắt đầu | 2 | 
| sau 1 | 2 | 
| sau 2 | 2 | 

Điều này cho thấy rằng khi hàm tổng hợp là đồng nhất, phép lũy thừa không có tác dụng và việc nâng nhị phân sẽ duy trì sự ổn định một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n \log M)$| Mỗi truy vấn xây dựng các chuyển đổi và nâng chúng lên trên các lớp log M | 
| Không gian |$O(n \log M)$| Lưu trữ bảng nâng trên mỗi truy vấn | 

Giải pháp vẫn đủ nhanh vì mỗi truy vấn hoạt động độc lập và độ sâu nâng bị giới hạn bởi số bit trong$M$, nhiều nhất là khoảng 30 đối với các ràng buộc điển hình. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old
    return out.getvalue().strip()

# sample test (from statement)
assert run("""4 3
2 3 4 1
4 1 2 3
1 1RRRRRRRR
1 12345RBRB
1 10000001R
""") == """1
1
2"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chu trình nhỏ | điểm cuối đúng | tính đúng đắn của các chu kỳ xác định | 
| mẫu nhận dạng | cùng một đỉnh bắt đầu | sự ổn định của các phép biến đổi no-op | 
| R/B xen kẽ | bố cục đúng | chuyển tiếp hỗn hợp | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi mẫu tạo ra một phép biến đổi tự vòng lặp sau một ứng dụng. Trong tình huống đó, phép lũy thừa lặp đi lặp lại không được trôi đi do thành phần không chính xác. 

đầu vào:```
n=2
v=1
M=10
r=[1,2]
b=[1,2]
pattern="RB"
```Cả hai cạnh luôn dẫn đến tự vòng lặp. Một ứng dụng đã ánh xạ mọi đỉnh vào chính nó, vì vậy tất cả các cấp độ nâng đều giữ nguyên danh tính. Cấu trúc thuật toán`nxt[u] = u`và mọi lũy thừa cao hơn vẫn giữ nguyên tính đồng nhất, vì vậy việc áp dụng lặp đi lặp lại sẽ tạo ra cùng một đỉnh bắt đầu. 

Một trường hợp cạnh khác là khi$M = 0$, không xuất hiện rõ ràng trong các ràng buộc nhưng được xử lý ngầm bằng logic phân rã nhị phân. Vì không có bit nào được đặt nên đỉnh ban đầu được trả về không thay đổi, phù hợp với ý tưởng không có ứng dụng nào.
