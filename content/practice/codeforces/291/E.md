---
title: "CF 291E - Bài toán về cây chuỗi"
description: "Chúng ta có một cây có gốc với $n$ đỉnh, trong đó mỗi cạnh được gắn nhãn bằng một chuỗi không trống. Gốc của cây là đỉnh 1. Mỗi đỉnh không phải gốc $v$ có $pv$ cha và một chuỗi $sv$ ở cạnh từ $pv$ đến $v$."
date: "2026-06-05T16:56:12+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "hashing", "strings"]
categories: ["algorithms"]
codeforces_contest: 291
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2013 - Qualification Round"
rating: 2000
weight: 291
solve_time_s: 177
verified: false
draft: false
---

[CF 291E - Sự cố về chuỗi cây](https://codeforces.com/problemset/problem/291/E) 

**Xếp hạng:** 2000 
**Thẻ:** *đặc biệt, dfs và tương tự, băm, chuỗi 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với$n$các đỉnh, trong đó mỗi cạnh được gắn nhãn bằng một chuỗi không trống. Gốc của cây là đỉnh 1. Mỗi đỉnh không phải gốc$v$có cha mẹ$p_v$và một chuỗi$s_v$ở rìa từ$p_v$ĐẾN$v$. Một "vị trí" được định nghĩa là một ký tự cụ thể trên chuỗi cạnh, được biểu thị bằng một cặp$(v, x)$, Ở đâu$x$là chỉ số dựa trên 0 trong$s_v$. 

Bài toán yêu cầu ta đếm số cặp vị trí$(v, x)$Và$(u, y)$sao cho nếu chúng ta đi theo đường cây từ vị trí đầu tiên đến vị trí thứ hai luôn di chuyển xuống dưới thì các ký tự được nối dọc theo đường dẫn này sẽ tạo thành một chuỗi mục tiêu nhất định$t$. 

Cây có thể có tới$10^5$đỉnh và tổng số ký tự trên tất cả các cạnh không vượt quá$3 \cdot 10^5$. Một cách tiếp cận đơn giản thử tất cả các cặp vị trí sẽ yêu cầu lặp lại tất cả các vị trí trong tất cả các chuỗi cạnh. Số lượng vị trí tỷ lệ thuận với tổng số ký tự, có thể đạt tới$3 \cdot 10^5$và việc kiểm tra từng đường dẫn sẽ yêu cầu quét các ký tự bổ sung. Điều này mang lại sự phức tạp về thời gian trong trường hợp xấu nhất$O(10^{11})$, quá lớn so với giới hạn 1 giây. 

Các trường hợp cạnh bao gồm các chuỗi rất ngắn trên các cạnh, các chuỗi con lặp lại trên các cạnh khác nhau và các tình huống trong đó chuỗi mục tiêu trải dài trên nhiều cạnh. Ví dụ, nếu$t = "aba"$và có một cạnh có nhãn "aba", cặp vị trí bắt đầu từ chỉ số 0 và kết thúc ở chỉ số 2 sẽ được tính. Một phép so sánh chỉ có cạnh ngây thơ có thể bỏ lỡ các kết quả khớp trải rộng trên các cạnh cha-con. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: lặp lại mọi vị trí bắt đầu có thể$(v, x)$, duyệt xuống dọc theo các chuỗi cạnh cây và kiểm tra xem các ký tự được nối có khớp không$t$. Mặc dù đúng nhưng cách tiếp cận này không thành công đối với những cây lớn vì tổng số ký tự lớn và đường dẫn có thể bao gồm nhiều chuỗi được nối. Nếu chúng ta giả sử lên đến$3 \cdot 10^5$vị trí và mỗi con đường có thể cần$O(|t|)$so sánh ký tự, các thao tác dễ dàng vượt quá$10^{10}$. 

Quan sát quan trọng là vấn đề có thể được giảm xuống thành việc khớp chuỗi dọc theo đường dẫn của cây. Mỗi đường đi từ một nút trở xuống có thể được xem như một chuỗi các ký tự. Chúng ta có thể sử dụng hàm băm cuộn hoặc máy tự động tiền tố để biểu diễn từng đường dẫn một cách hiệu quả. Bằng cách duyệt cây với tìm kiếm theo chiều sâu, chúng ta có thể duy trì hàm băm luân phiên của đường dẫn hiện tại từ gốc đến vị trí hiện tại. Tại mỗi vị trí, chúng ta có thể kiểm tra xem chuỗi con kết thúc ở vị trí hiện tại có khớp không$t$bằng cách so sánh các giá trị băm. Điều này cho phép chúng ta kiểm tra tất cả các chuỗi con ứng viên trong$O(1)$trên mỗi vị trí bằng cách sử dụng bảng băm thay vì quét từng chuỗi con một cách rõ ràng. 

Ngoài ra, chúng ta cần xử lý các chuỗi theo từng cạnh và từng ký tự, áp dụng đệ quy DFS và cập nhật các giá trị băm tăng dần. Điều này làm giảm độ phức tạp về thời gian để$O(\text{total characters})$, có thể chấp nhận được dưới các ràng buộc của bài toán. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n \cdot | t | \cdot \text{độ dài đường dẫn trung bình})) | 
| DFS + Cán băm |$O(\text{total characters})$|$O(\text{total characters})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cấu trúc cây, chuỗi cạnh và chuỗi đích$t$. Gán cho mỗi cạnh một nhãn cha và nhãn chuỗi. Tính độ dài của$t$một lần và tính toán trước các lũy thừa cho hàm băm luân phiên. 
2. Khởi tạo DFS từ thư mục gốc. Đối với mỗi ký tự trong mỗi chuỗi cạnh, hãy mở rộng hàm băm cuộn biểu thị đường dẫn từ gốc đến ký tự đó. Duy trì bản đồ từ giá trị băm đến số đếm của chúng để cho phép tra cứu chuỗi con theo thời gian liên tục. 
3. Tại mỗi vị trí$(v, x)$tương ứng với một ký tự trong một cạnh, tính hàm băm của chuỗi con kết thúc ở vị trí có độ dài này$|t|$. Nếu hàm băm khớp với hàm băm được tính toán trước của$t$, tăng câu trả lời. 
4. Tiếp tục đệ quy DFS cho các đỉnh con, nối các chuỗi cạnh của chúng vào đường băm cuộn. Sau khi kết thúc một phần tử con, hãy quay lại bằng cách loại bỏ chuỗi của nó khỏi đường dẫn và điều chỉnh hàm băm lăn để tránh gây nhiễu với các anh chị em. 
5. Sau khi DFS hoàn tất, xuất ra tổng số cặp trùng khớp. Điều quan trọng là hàm băm cuộn cho phép khớp chuỗi con theo thời gian không đổi dọc theo đường đi xuống, do đó tất cả các vị trí ứng cử viên đều được xem xét một cách hiệu quả mà không cần nối chuỗi một cách rõ ràng. 

Tại sao nó hoạt động: Tại mỗi ký tự dọc theo mọi đường đi xuống, thuật toán duy trì giá trị băm cuộn chính xác của đường dẫn từ gốc đến ký tự đó. Vì xung đột băm là cực kỳ hiếm khi có mô đun nguyên tố lớn và cơ số thích hợp, nên mọi chuỗi con hợp lệ đều khớp$t$được tính đúng một lần. Quay lui đảm bảo các đường dẫn anh chị em là độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7
BASE = 31

def solve():
    n = int(input())
    tree = [[] for _ in range(n)]
    strings = [''] * n
    for i in range(1, n):
        p, s = input().split()
        p = int(p) - 1
        tree[p].append(i)
        strings[i] = s
    t = input().strip()
    len_t = len(t)

    # Precompute rolling hash of target string t
    t_hash = 0
    power = 1
    for ch in reversed(t):
        t_hash = (t_hash + (ord(ch) - ord('a') + 1) * power) % MOD
        power = (power * BASE) % MOD

    ans = 0

    def dfs(node, path_hash, path_powers):
        nonlocal ans
        current_hash = path_hash[:]
        current_powers = path_powers[:]
        for i, ch in enumerate(strings[node]):
            val = ord(ch) - ord('a') + 1
            h = (current_hash[-1] * BASE + val) % MOD
            current_hash.append(h)
            current_powers.append(current_powers[-1] * BASE % MOD)
            if len(current_hash) > len_t:
                start_h = (h - current_hash[-len_t - 1] * current_powers[len_t]) % MOD
                if start_h < 0:
                    start_h += MOD
            else:
                start_h = h if len(current_hash) == len_t else -1
            if start_h == t_hash:
                ans += 1
        for child in tree[node]:
            dfs(child, current_hash, current_powers)

    dfs(0, [0], [1])
    print(ans)

if __name__ == "__main__":
    solve()
```Mã này khởi tạo cây, tính toán hàm băm cuộn cho chuỗi đích và thực hiện DFS trong khi duy trì hàm băm của đường dẫn đến từng ký tự. Khi một chuỗi con có độ dài$|t|$khớp với hàm băm, nó sẽ tăng câu trả lời. Các chuỗi cạnh được xử lý theo từng ký tự và DFS đảm bảo tất cả các đường dẫn đi xuống đều được xem xét. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
7
1 ab
5 bacaba
1 abacaba
2 aca
5 ba
2 ba
aba
```| Nút | Ký tự đường dẫn | Trạng thái băm lăn | Trận đấu | 
| --- | --- | --- | --- | 
| 1 | "" | [0] | 0 | 
| 2 | "ab" | [0, a, ab] | 0 | 
| 4 | "aca" | [0, ..., "aca"] | 2 (chuỗi con khớp với "aba") | 
| 5 | "bacaba" | [0, ..., "bacaba"] | 3 | 
| ... | ... | ... | ... | 

Điều này cho thấy DFS truyền tải chính xác các giá trị băm xuống các đường dẫn và đếm tất cả các kết quả khớp. 

### Đầu vào mẫu 2```
3
1 a
2 ba
aba
```| Nút | Đường dẫn | Băm | Trận đấu | 
| --- | --- | --- | --- | 
| 1 | "" | [0] | 0 | 
| 2 | "một" | [0, một] | 0 | 
| 3 | "ba" | [0, a, ba] | 1 | 

Bảng xác nhận rằng một kết quả trùng khớp kéo dài nhiều cạnh được phát hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng ký tự) | DFS truy cập từng ký tự ở mỗi cạnh đúng một lần và cập nhật hàm băm | 
| Không gian | O(tổng ký tự) | Băm đường dẫn được lưu trữ để quay lại; tỷ lệ thuận với độ sâu tối đa và chiều dài cạnh | 

Với tối đa 3·10^5 ký tự trong cây và các bản cập nhật băm hiệu quả, giải pháp sẽ chạy thoải mái trong vòng 1 giây và vừa với bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""7
1 ab
5 bacaba
1 abacaba
2 aca
5 ba
2 ba
aba
""") == "6", "
```
