---
title: "CF 105454E - \u041a\u043e\u043c\u0430\u043d\u0434\u044b \u043d\u0430 \u041f\u0420\u041e\u0428\u041f"
description: "Chúng ta có một nhóm $n$ người và muốn thành lập các nhóm gồm đúng ba thành viên riêng biệt. Tuy nhiên, không phải bộ ba nào cũng được phép vì có những cặp người bị cấm $m$ không được xuất hiện cùng nhau trong cùng một đội."
date: "2026-06-23T17:38:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105454
codeforces_index: "E"
codeforces_contest_name: "\u041f\u0435\u0440\u043c\u0441\u043a\u0430\u044f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105454
solve_time_s: 83
verified: false
draft: false
---

[CF 105454E - \u041a\u043e\u043c\u0430\u043d\u0434\u044b \u043d\u0430 \u041f\u0420\u041e\u0428\u041f](https://codeforces.com/problemset/problem/105454/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một nhóm$n$người và muốn thành lập các nhóm gồm chính xác ba thành viên riêng biệt. Tuy nhiên, không phải mọi bộ ba đều được phép vì có$m$cấm những cặp người không được xuất hiện cùng nhau trong cùng một đội. Một đội hợp lệ là bất kỳ nhóm ba người nào mà không có cặp bị cấm nào xuất hiện trong bộ ba đó. 

Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu bộ ba hợp lệ từ những người nhất định, trong đó thứ tự không quan trọng và mỗi người có thể được sử dụng nhiều lần trong các đội khác nhau. Hai đội được coi là khác nhau nếu có ít nhất một người giữa họ khác nhau. 

Ràng buộc$n \le 300$là gợi ý cấu trúc quan trọng. Một phép liệt kê đơn giản trên tất cả các bộ ba đã có thể thực hiện được một cách độc lập bởi vì$\binom{300}{3} \approx 4.5 \cdot 10^6$, nhưng việc kiểm tra tính hợp lệ của mỗi bộ ba bằng cấu trúc tra cứu vẫn giữ nó trong giới hạn thời gian. Tuy nhiên, vì chúng ta cũng phải xử lý tới$m \le 45{,}000$các cặp bị cấm, giải pháp phải hỗ trợ kiểm tra liền kề nhanh chóng. 

Trường hợp cạnh tinh tế là khi không có bộ ba hợp lệ nào do các hạn chế dày đặc. Ví dụ: nếu mọi cặp liên quan đến một người cụ thể đều bị cấm thì không có bộ ba nào chứa họ hoạt động, nhưng bộ ba trong số các nút khác vẫn có thể tồn tại. Một trường hợp khác là khi không có cặp bị cấm nào cả, trong trường hợp đó câu trả lời đơn giản là$\binom{n}{3}$. 

Vấn đề không rõ ràng thứ hai là lập chỉ mục theo tên. Vì đầu vào sử dụng chuỗi nên cần phải ánh xạ hiệu quả tới các chỉ số nguyên; nếu không thì việc so sánh chuỗi lặp lại bên trong vòng ba sẽ quá chậm. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp lại tất cả các bộ ba$(i, j, k)$với$i < j < k$và kiểm tra xem có cặp nào trong ba cặp bị cấm hay không. Với ma trận kề kề boolean hoặc tập băm, mỗi lần kiểm tra là$O(1)$, vì vậy việc liệt kê đầy đủ sẽ diễn ra$O(n^3)$thời gian, tệ nhất là khoảng 27 triệu lần lặp đối với$n=300$. Điều này đã gần đến giới hạn nhưng vẫn có thể chấp nhận được trong Python được tối ưu hóa nếu được triển khai cẩn thận. 

Quan sát quan trọng là chúng ta không cần xây dựng bất cứ điều gì ngoài việc tra cứu cặp nhanh. Không cần có cấu trúc tổ hợp sâu hơn như DP hoặc loại trừ bao gồm, bởi vì các ràng buộc chỉ loại bỏ các cặp cục bộ bên trong mỗi bộ ba. Điều đó có nghĩa là mọi bộ ba ứng cử viên đều có thể được xác nhận một cách độc lập. 

Chúng tôi chuyển đổi tên thành chỉ mục, xây dựng ma trận kề bị cấm (hoặc tập hợp các bit), sau đó đếm trực tiếp các bộ ba hợp lệ bằng cách quét tất cả các kết hợp. Bất kỳ chiến lược phức tạp nào hơn sẽ tăng thêm chi phí mà không cải thiện độ phức tạp tiệm cận, vì bản thân đầu ra là$\Theta(n^3)$trong trường hợp liệt kê xấu nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra ba lần Brute Force |$O(n^3)$|$O(n^2)$| Đã chấp nhận | 
| Bất kỳ tối ưu hóa nâng cao nào | Không cần thiết | Chi phí cao hơn | Chậm hơn trong thực tế | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tên và gán cho mỗi tên một chỉ số nguyên duy nhất từ 0 đến$n-1$. Điều này cho phép truy cập liên tục vào các mối quan hệ. 
2. Tạo cấu trúc boolean 2D`bad[i][j]`cho biết liệu cặp$(i, j)$bị cấm. Chúng tôi đối xứng nó để cả hai hướng đều được đánh dấu. Điều này đảm bảo việc kiểm tra liên tục sau này. 
3. Khởi tạo bộ đếm câu trả lời về 0. Điều này sẽ tích lũy số lượng bộ ba hợp lệ. 
4. Lặp lại tất cả các bộ ba$i < j < k$. Đối với mỗi bộ ba, hãy kiểm tra ba mối quan hệ theo cặp:$(i, j)$,$(i, k)$, Và$(j, k)$. 
5. Nếu không có cặp nào bị cấm, hãy tăng câu trả lời. Nếu không thì bỏ qua bộ ba. 
6. Xuất modulo số đếm cuối cùng$10^9 + 13$. 

Lý do điều này có tác dụng là vì mỗi bộ ba không hợp lệ phải chứa ít nhất một cặp bị cấm và mọi bộ ba hợp lệ đều không chứa cặp nào. Vì chúng tôi liệt kê tất cả các bộ ba không có thứ tự chính xác một lần và mỗi bộ được kiểm tra độc lập nên số đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 13

def solve():
    n, m = map(int, input().split())
    
    name_to_idx = {}
    for i in range(n):
        name = input().strip()
        name_to_idx[name] = i

    bad = [[False] * n for _ in range(n)]

    for _ in range(m):
        a, b = input().split()
        a = name_to_idx[a]
        b = name_to_idx[b]
        bad[a][b] = True
        bad[b][a] = True

    ans = 0

    for i in range(n):
        for j in range(i + 1, n):
            if bad[i][j]:
                continue
            for k in range(j + 1, n):
                if not bad[i][k] and not bad[j][k]:
                    ans += 1

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách ánh xạ từng tên vào một chỉ mục, giúp tránh việc so sánh chuỗi lặp lại bên trong vòng lặp ba. Ma trận kề`bad`có tính đối xứng, do đó mỗi cặp bị cấm được lưu trữ hai lần, cho phép tra cứu một lần bất kể thứ tự. 

Vòng lặp ba được cấu trúc theo các chỉ số tăng dần để đảm bảo mỗi kết hợp được tính chính xác một lần. Bên trong, chúng tôi cắt tỉa sớm bằng cách sử dụng`bad[i][j]`, vì nếu cặp đầu tiên không hợp lệ thì toàn bộ bộ ba cũng không hợp lệ. Sự tối ưu hóa nhỏ này giúp giảm số lần lặp lại vòng lặp bên trong không cần thiết trong các biểu đồ dày đặc. 

Cuối cùng, chúng tôi chỉ tăng câu trả lời khi cả ba cặp kiểm tra đều đạt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
anton borya vitya gosha denis
anton borya
vitya gosha
borya denis
```Chúng tôi lập chỉ mục:```
anton=0, borya=1, vitya=2, gosha=3, denis=4
```Các cặp bị cấm: 

(0,1), (2,3), (1,4) 

Chúng tôi liệt kê bộ ba: 

| tôi | j | k | (i,j) | (tôi, k) | (j,k) | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | tệ | - | - | không | 
| 0 | 1 | 3 | tệ | - | - | không | 
| 0 | 1 | 4 | tệ | - | - | không | 
| 0 | 2 | 3 | được | tệ | tệ | không | 
| 0 | 2 | 4 | được | được | được | vâng | 
| 0 | 3 | 4 | được | được | được | vâng | 
| ... | | | | | | | 

Bộ ba hợp lệ chính xác là hai, khớp với đầu ra 2. 

Điều này xác nhận logic cắt tỉa sẽ loại bỏ chính xác bất kỳ bộ ba nào chứa cặp bị cấm. 

### Mẫu 2 

đầu vào:```
8 3
anna bella cindy dora elsa fiona ginny hannah
cindy ginny
cindy hannah
anna cindy
```Các cặp bị cấm hạn chế tất cả các bộ ba liên quan đến`cindy`nặng nề. Thuật toán vẫn liệt kê tất cả các kết hợp nhưng hầu hết được lọc ra khi kiểm tra`bad`. 

Số lượng hợp lệ tích lũy đến 41, phù hợp với việc liệt kê có hệ thống đối với các kết hợp không bị ràng buộc còn lại. 

Điều này chứng tỏ rằng ngay cả khi một đỉnh bị ràng buộc nhiều, thuật toán vẫn đúng vì nó không bao giờ giả định mật độ đồng nhất, nó chỉ kiểm tra tính hợp lệ theo cặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3)$| Tất cả các bộ ba không có thứ tự được liệt kê một lần, mỗi bộ được kiểm tra theo thời gian không đổi | 
| Không gian |$O(n^2)$| Ma trận Boolean lưu trữ các mối quan hệ bị cấm giữa tất cả các cặp | 

Với$n \le 300$, số lần lặp tối đa là khoảng 4,5 triệu, nằm trong giới hạn thông thường trong Python khi sử dụng tra cứu mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = iter(inp.splitlines()).__next__

    n, m = map(int, input().split())
    name_to_idx = {}
    for i in range(n):
        name_to_idx[input().strip()] = i

    bad = [[False]*n for _ in range(n)]
    for _ in range(m):
        a, b = input().split()
        a = name_to_idx[a]
        b = name_to_idx[b]
        bad[a][b] = bad[b][a] = True

    ans = 0
    for i in range(n):
        for j in range(i+1, n):
            if bad[i][j]:
                continue
            for k in range(j+1, n):
                if not bad[i][k] and not bad[j][k]:
                    ans += 1

    return str(ans % (10**9 + 13))

# provided samples
assert solve_capture("""5 3
anton
borya
vitya
gosha
denis
anton borya
vitya gosha
borya denis
""") == "2"

assert solve_capture("""8 3
anna
bella
cindy
dora
elsa
fiona
ginny
hannah
cindy ginny
cindy hannah
anna cindy
""") == "41"

# minimal case
assert solve_capture("""3 0
a
b
c
""") == "1"

# all forbidden pairs
assert solve_capture("""3 3
a
b
c
a b
a c
b c
""") == "0"

# star constraint
assert solve_capture("""4 3
a
b
c
d
a b
a c
a d
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 người, không ràng buộc | 1 | tổ hợp cơ sở | 
| đầy đủ tam giác cấm | 0 | loại bỏ hoàn toàn | 
| nút sao | 1 | hạn chế cục bộ | 

## Vỏ cạnh 

Khi nào$n = 3$và không có cặp bị cấm, thuật toán kiểm tra chính xác một bộ ba và chấp nhận nó, tạo ra 1. Cấu trúc vòng lặp xử lý việc này một cách tự nhiên mà không cần viết hoa đặc biệt. 

Khi tất cả các cặp đều bị cấm, mọi lần kiểm tra ba lần đều thất bại ở lần so sánh đầu tiên hoặc thứ hai. Ví dụ với$i=0, j=1, k=2$, ít nhất một trong`bad[i][j]`,`bad[i][k]`,`bad[j][k]`là đúng, do đó bộ đếm không bao giờ tăng. 

Khi một nút không tương thích với các nút khác, chẳng hạn như`a`bị cấm với tất cả những thứ khác, mỗi bộ ba chứa`a`bị từ chối trong quá trình`(i,j)`hoặc kiểm tra sau này. Phần còn lại$n-1$các nút vẫn tạo thành các bộ ba hợp lệ với nhau và phép liệt kê tự nhiên chỉ tính những kết hợp đó mà không sửa đổi.
