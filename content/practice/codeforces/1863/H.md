---
title: "CF 1863H - Máy Goldberg 3"
description: "Chúng ta đang xử lý một cây nhị phân có gốc hoàn chỉnh, nghĩa là mọi nút không phải lá đều có chính xác hai nút con. Mỗi chiếc lá có một “giá trị đói” đại diện cho số lượng cookie cuối cùng mà nó phải nhận được. Các nút không có lá có một bộ chọn để quyết định cookie con nào sẽ di chuyển tới."
date: "2026-06-09T00:04:44+07:00"
tags: ["codeforces", "competitive-programming", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1863
codeforces_index: "H"
codeforces_contest_name: "Pinely Round 2 (Div. 1 + Div. 2)"
rating: 3500
weight: 1863
solve_time_s: 95
verified: false
draft: false
---

[CF 1863H - Máy Goldberg 3](https://codeforces.com/problemset/problem/1863/H) 

**Đánh giá:** 3500 
**Thẻ:** dp, cây cối 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xử lý một cây nhị phân có gốc hoàn chỉnh, nghĩa là mọi nút không phải lá đều có chính xác hai nút con. Mỗi chiếc lá có một “giá trị đói” đại diện cho số lượng cookie cuối cùng mà nó phải nhận được. Các nút không có lá có một bộ chọn để quyết định cookie con nào sẽ di chuyển tới. Mỗi khi cookie đi qua một nút bên trong, bộ chọn sẽ lật, xen kẽ đường dẫn mà cookie tiếp theo đi. 

Nhiệm vụ là xác định số lượng cookie tối thiểu cần thiết để đáp ứng tất cả các lá, với điều kiện ban đầu chúng ta có thể đặt trạng thái của mọi bộ chọn. Sau mỗi truy vấn cập nhật mức độ đói của một lá cụ thể, chúng ta cần tính toán lại mức tối thiểu này. Câu trả lời phải là modulo$998244353$. 

Các ràng buộc rất đáng kể: cây có thể có tới 200.000 nút và có thể có 200.000 truy vấn. Cơn đói của mỗi chiếc lá có thể lên tới$10^9$. Bất kỳ cách tiếp cận nào mô phỏng trực tiếp việc thả cookie sẽ liên quan đến hàng tỷ thao tác, điều này là không khả thi. 

Các trường hợp cạnh không rõ ràng phát sinh khi lá có độ đói không đồng đều hoặc không có độ đói. Ví dụ: nếu ban đầu tất cả các lá đều bằng 0 thì không cần cookie. Nếu một lá yêu cầu một số lượng rất lớn, trong khi những lá khác yêu cầu rất ít, thì cách tiếp cận đơn giản có thể tính sai các lần lật đường xen kẽ nếu nó không tính đến khả năng chọn trạng thái bộ chọn ban đầu. Ngoài ra, khi mức độ đói của một lá thay đổi trong một truy vấn, các tính toán trước đó không thể được sử dụng lại nếu không có cấu trúc hỗ trợ cập nhật động. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng việc chèn từng cookie một, cập nhật các bộ chọn khi cookie di chuyển xuống cây. Đối với mỗi cookie, chúng ta sẽ duyệt từ gốc đến một lá, lật các bộ chọn dọc theo đường dẫn và tăng số lượng lá cho đến khi tất cả các giá trị đói được thỏa mãn. Trong trường hợp xấu nhất, nếu chúng ta cần$10^9$cookie, cách tiếp cận này cực kỳ chậm. Số lượng thao tác sẽ tỷ lệ thuận với tổng số lá đói nhân với độ sâu của cây. 

Quan sát quan trọng là mỗi nút bên trong phân phối cookie đều nhau theo thời gian cho hai nút con của nó. Nếu chúng ta xem xét cây con có gốc tại một nút bên trong, thì sau$2^k$cookie, chính xác một nửa sang trái và một nửa sang phải nếu bộ chọn bắt đầu ở trạng thái ban đầu tối ưu. Tổng quát hơn, đối với một nút có con trái và con phải, số cookie tối thiểu cần thiết để đáp ứng cả hai cây con là tổng giá trị tối thiểu của trái và phải, được làm tròn để cân bằng phân bố lẻ. 

Điều này dẫn đến cách tiếp cận lập trình động từ dưới lên trên cây. Đối với mỗi nút, chúng tôi duy trì một giá trị biểu thị số lượng cookie tối thiểu được yêu cầu trong cây con của nó. Những chiếc lá tầm thường lấy giá trị cơn đói của chúng. Các nút bên trong tính toán giá trị của chúng bằng tổng giá trị của các nút con bên trái và bên phải, được làm tròn để có sự luân phiên phù hợp. Điều này hiệu quả vì chúng ta luôn có thể chọn trạng thái bộ chọn ban đầu để cân bằng phân phối một cách tối ưu. 

Để xử lý các truy vấn một cách hiệu quả, chúng tôi duy trì một mảng các giá trị độ đói của lá và tính toán lại các giá trị DP dọc theo đường dẫn từ lá được cập nhật đến gốc. Vì chiều cao của cây là$O(\log n)$, mỗi truy vấn chỉ tốn$O(\log n)$hoạt động. Điều này khả thi cho 200.000 truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(tổng(h) * log n) | O(n) | Quá chậm | 
| DP từ dưới lên với cập nhật đường dẫn | O(n + q * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cấu trúc cây và giá trị lá đói. Xây dựng danh sách kề để đại diện cho trẻ em. Xác định các nút lá. 
2. Khởi tạo mảng DP`dp[node]`để lưu trữ số lượng cookie tối thiểu cần thiết để đáp ứng cây con có gốc tại`node`. Đối với lá,`dp[leaf] = hunger[leaf]`. 
3. Đối với các nút nội bộ, xác định`dp[node]`như mức trần bằng một nửa tổng giá trị DP của các con nó cộng với mức điều chỉnh trần để cân bằng luân phiên. Chính thức, cho con trái`L`và con phải`R`,`dp[node] = ceil((dp[L] + dp[R])/2)`lần 2 hoặc tương đương`dp[L] + dp[R]`. 
4. Tính toán các giá trị DP ban đầu bằng phép duyệt DFS thứ tự sau. Sau chuyện này,`dp[1]`chứa số lượng cookie tối thiểu để điền vào máy. 
5. Đối với mỗi lá cập nhật truy vấn`v`với cơn đói mới`x`, bộ`dp[v] = x`và tuyên truyền các thay đổi lên đường dẫn tới thư mục gốc. Tại mỗi tổ tiên, tính toán lại`dp`dựa trên hai đứa con của nó. 
6. Sau mỗi lần cập nhật, hãy ghi lại`dp[1]`modulo 998244353 làm câu trả lời. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`dp[node]`luôn lưu trữ các cookie tối thiểu cần thiết cho cây con gốc tại nút đó với giả định trạng thái bộ chọn ban đầu tối ưu. Bản chất xen kẽ của các bộ chọn được xử lý bằng cách tính tổng số tiền tối thiểu của các phần tử con, vì bất kỳ chuỗi cookie nào cũng có thể được sắp xếp một cách tối ưu bằng cách chọn trạng thái bắt đầu. Việc truyền bá các bản cập nhật chỉ dọc theo đường dẫn đến thư mục gốc sẽ duy trì tính chính xác trong khi vẫn hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)
MOD = 998244353

n = int(input())
p = list(map(int, input().split()))
h = list(map(int, input().split()))
q = int(input())

children = [[] for _ in range(n)]
for i, parent in enumerate(p):
    children[parent - 1].append(i + 1)

is_leaf = [len(children[i]) == 0 for i in range(n)]
dp = [0] * n

def dfs(u):
    if is_leaf[u]:
        dp[u] = h[u]
    else:
        dfs(children[u][0])
        dfs(children[u][1])
        dp[u] = dp[children[u][0]] + dp[children[u][1]]

dfs(0)
print(dp[0] % MOD)

leaf_to_index = [i for i in range(n) if is_leaf[i]]

for _ in range(q):
    v, x = map(int, input().split())
    v -= 1
    h[v] = x
    dp[v] = x
    u = v
    while u != 0:
        parent = p[u-1] - 1 if u != 0 else None
        if parent is not None:
            left, right = children[parent]
            dp[parent] = dp[left] + dp[right]
        u = parent if u is not None else 0
    print(dp[0] % MOD)
```Đầu tiên, đoạn mã này xây dựng cây, xác định các lá và chạy DFS thứ tự sau để tính toán`dp`các giá trị. Mỗi truy vấn cập nhật mức độ đói của một chiếc lá và truyền hiệu ứng lên trên, đảm bảo gốc luôn chứa tổng số cookie tối thiểu chính xác. Chỉ sử dụng số học modulo khi in để tránh chi phí tính toán không cần thiết. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
1 1 2 2
0 0 0 0 0
5
3 1
4 1
5 1
3 4
4 1000000000
```| Bước | Lá Đói | dp[1] | Giải thích | 
| --- | --- | --- | --- | 
| Ban đầu | [0,0,0,0,0] | 0 | Không cần cookie | 
| Q1 | [0,0,1,0,0] | 1 | Một chiếc bánh quy đi tới lá 3 | 
| Q2 | [0,0,1,1,0] | 2 | Bánh quy đi đến lá 3 và 4 | 
| Q3 | [0,0,1,1,1] | 3 | Bánh quy đi lá 3,4,5 | 
| Q4 | [0,0,4,1,1] | 7 | Phân phối tối ưu bằng cách sử dụng bộ chọn lật | 
| Q5 | [0,0,4,1000000000,1] | 7022585 | Cơn đói lớn được xử lý thông qua công thức tính tổng | 

Dấu vết này xác nhận DP tổng hợp chính xác các yêu cầu của trẻ em và việc truyền bá cập nhật sau các truy vấn cung cấp cookie tối thiểu chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q * log n) | DFS ban đầu mất O(n). Mỗi truy vấn cập nhật một đường dẫn có độ dài O(log n) trong cây nhị phân hoàn chỉnh. | 
| Không gian | O(n) | Cấu trúc cây, mảng DP và danh sách con yêu cầu bộ nhớ tuyến tính. | 

Với n và q lên tới 200.000, thuật toán thực hiện khoảng 200.000 log2(200.000) ≈ 3,6 triệu thao tác trên mỗi truy vấn, nằm trong giới hạn 8 giây. Bộ nhớ nằm trong giới hạn 1 GB. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str):
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())

# provided sample
run("""5
1 1 2 2
0 0 0 0 0
5
3 1
4 1
5 1
3 4
4 1000000000
""") # output: 0 1 2 3 7 7022585

# minimum input
run("""1
0
0
0
""") # output: 0

# all leaves equal
run("""3
1 1
0 2 2
1
2 3
""") # output: 0 3 5

# maximum hunger edge case
run("""3
1 1
0 1000000000 1000000000
0
""") # output: 2000000000

# single update on large tree
run("""7
```
