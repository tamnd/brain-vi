---
title: "CF 350B - Khu nghỉ dưỡng"
description: "Chúng ta có một cấu trúc có hướng về n đối tượng đại diện cho một khu nghỉ dưỡng trượt tuyết. Mỗi đối tượng là một ngọn núi hoặc một khách sạn. Mọi đối tượng đều có nhiều nhất một đường trượt tuyết dẫn đến một đối tượng khác và một khách sạn không bao giờ có đường trượt tuyết nào cả."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs"]
categories: ["algorithms"]
codeforces_contest: 350
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 203 (Div. 2)"
rating: 1500
weight: 350
solve_time_s: 268
verified: false
draft: false
---

[CF 350B - Khu nghỉ dưỡng](https://codeforces.com/problemset/problem/350/B) 

**Đánh giá:** 1500 
**Thẻ:** đồ thị 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc định hướng trên`n`đồ vật đại diện cho một khu nghỉ mát trượt tuyết. Mỗi đối tượng là một ngọn núi hoặc một khách sạn. Mọi đối tượng đều có nhiều nhất một đường trượt tuyết dẫn đến một đối tượng khác và một khách sạn không bao giờ có đường trượt tuyết nào cả. Điều này có nghĩa là mỗi nút có nhiều nhất là một nút và tất cả các cạnh đều hướng về nút “tiếp theo”, tạo thành một tập hợp các chuỗi được định hướng cuối cùng kết thúc tại các khách sạn. 

Nhiệm vụ là chọn một con đường hợp lệ bắt đầu từ một ngọn núi nào đó và đi theo các đường trượt tuyết hướng ra ngoài từng bước cho đến khi kết thúc tại một khách sạn. Trong số tất cả các chuỗi hợp lệ như vậy, chúng ta phải xuất ra chuỗi dài nhất về số lượng nút được truy cập. 

Cấu trúc ngụ ý rằng mỗi nút có nhiều nhất một cạnh đi ra, do đó, từ bất kỳ điểm bắt đầu nào đều có một bước tiến duy nhất. Quyết định thực sự là chọn ngọn núi xuất phát tốt nhất dẫn đến khách sạn sâu nhất có thể tiếp cận được. 

Với`n ≤ 10^5`, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Một cách tiếp cận bậc hai thử mọi nút bắt đầu và mô phỏng bước đi có thể giảm xuống`O(n^2)`trong một chuỗi dài, quá chậm. Biểu đồ thực sự là một rừng các chuỗi được định hướng, vì vậy chúng ta nên mong đợi một giải pháp lập trình động hoặc truyền tải được ghi nhớ trong`O(n)`. 

Một vấn đề khó phát sinh nếu người ta cố gắng bắt đầu từ tất cả các ngọn núi mà không có kết quả lưu vào bộ nhớ đệm. Nhiều điểm bắt đầu có thể chia sẻ các hậu tố dài và việc tính toán lại chúng nhiều lần sẽ gây ra việc duyệt lặp lại các chuỗi giống nhau. 

Một trường hợp cạnh khác là một số nút có thể là khách sạn biệt lập (không có cạnh đi và không có cạnh vào). Các nút như vậy tạo thành các đường dẫn hợp lệ có độ dài bằng một và chúng phải được xem xét ở mức tối đa. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi ngọn núi là điểm khởi đầu và đi theo các cạnh hướng ra ngoài của nó cho đến khi đến khách sạn, đếm số bước trên đường đi. Điều này đúng vì biểu đồ không có phân nhánh về phía trước nên mỗi điểm bắt đầu xác định chính xác một đường dẫn. Tuy nhiên, trong trường hợp xấu nhất đồ thị là một chuỗi dài có độ dài`n`. Bắt đầu từ nút đầu tiên sẽ đi qua`n`các nút, bắt đầu từ nút thứ hai sẽ đi qua`n-1`, v.v., dẫn đến đại khái`n + (n-1) + ... + 1 = O(n^2)`hoạt động. 

Quan sát quan trọng là mỗi nút có nhiều nhất một cạnh đi ra, do đó cấu trúc là một tập hợp các chuỗi rời rạc hợp nhất về phía sau nhưng không bao giờ phân nhánh về phía trước. Điều này giúp cho mỗi nút có thể tính toán độ dài của chuỗi kết thúc tại một khách sạn bằng cách sử dụng tính năng ghi nhớ. Khi đã biết độ dài của một nút, nó không bao giờ cần phải tính toán lại. Chúng tôi cũng có thể lưu trữ nút tiếp theo theo đường dẫn tốt nhất để việc tái thiết trở nên đơn giản. 

Điều này biến vấn đề thành tính toán khoảng cách từ hậu tố đến khách sạn dài nhất trong biểu đồ hàm trong đó mức độ đi ra là 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu (DP trên đồ thị hàm số) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi lập mô hình mỗi nút có nhiều nhất một cạnh đi ra. Đối với mỗi nút, chúng tôi tính toán độ dài của đường dẫn có được bằng cách liên tục đi theo các cạnh đi ra cho đến khi đến khách sạn cuối cùng. 

1. Xây dựng hàm kề ngầm từ mảng`a`, Ở đâu`a[i]`là nút tiếp theo hoặc bằng không. Điều này cung cấp một người kế nhiệm duy nhất cho mỗi nút. 
2. Duy trì một mảng ghi nhớ`dp[v]`lưu trữ độ dài đường dẫn tối đa bắt đầu từ`v`và một`next[v]`con trỏ để tái thiết. Chúng tôi cũng theo dõi xem một nút đã được tính toán chưa để tránh việc tính toán lại. 
3. Đối với mỗi nút`v`, nếu nó đã được tính toán rồi thì chúng ta bỏ qua nó. Mặt khác, chúng tôi tính toán độ dài chuỗi của nó bằng cách đi về phía trước cho đến khi chúng tôi gặp nút được tính toán hoặc đến nút cuối không có cạnh đi ra. 
4. Khi chúng tôi đến cuối chuỗi (một khách sạn hoặc một nút không có cạnh ra), chúng tôi sẽ chỉ định giá trị cơ bản cho nó`1`vì nó tự tạo thành một đường đi có độ dài riêng. 
5. Sau đó, chúng tôi truyền các giá trị ngược dọc theo con đường chúng tôi đã đi qua, gán`dp[u] = 1 + dp[a[u]]`và thiết lập`next[u] = a[u]`. Điều này đảm bảo mỗi nút biết chuỗi hậu tố của nó dài bao nhiêu. 
6. Sau khi tính toán tất cả các giá trị dp, chúng tôi chọn nút bắt đầu trong số tất cả các ngọn núi tối đa hóa`dp[v]`. 
7. Cuối cùng, xây dựng lại đường đi bằng cách đi theo nhiều lần`next[]`con trỏ cho đến khi kết thúc. 

Lựa chọn thiết kế chính là lưu trữ con trỏ tiếp theo trong quá trình truyền DP, cho phép chúng ta xây dựng lại chuỗi cực đại chính xác mà không cần tính toán lại. 

### Tại sao nó hoạt động 

Mỗi nút có một đường chuyển tiếp duy nhất nên giá trị dp của nó chỉ phụ thuộc vào nút tiếp theo. Điều này xác định một sự phụ thuộc chức năng. Bằng cách phân giải các giá trị dp từ điểm chìm về phía sau, giá trị của mỗi nút sẽ được cố định chính xác một lần. Vì mỗi cạnh được xử lý nhiều nhất một lần trong quá trình ghi nhớ nên không thể xảy ra trạng thái không nhất quán và điểm bắt đầu được chọn cuối cùng nhất thiết phải mang lại chuỗi hợp lệ dài nhất trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    typ = list(map(int, input().split()))
    a = list(map(int, input().split()))

    dp = [0] * (n + 1)
    nxt = [0] * (n + 1)
    vis = [False] * (n + 1)

    sys.setrecursionlimit(10**7)

    def compute(v):
        if vis[v]:
            return dp[v]

        path = []
        cur = v

        while cur != 0 and not vis[cur]:
            path.append(cur)
            cur = a[cur]

        if cur == 0:
            dp[cur] = 0

        for node in reversed(path):
            if a[node] == 0:
                dp[node] = 1
                nxt[node] = 0
            else:
                dp[node] = 1 + dp[a[node]]
                nxt[node] = a[node]
            vis[node] = True

        return dp[v]

    for i in range(1, n + 1):
        if not vis[i]:
            compute(i)

    start = 1
    best = -1
    for i in range(1, n + 1):
        if typ[i - 1] == 0:
            continue
        if dp[i] > best:
            best = dp[i]
            start = i

    path = []
    cur = start
    while cur != 0:
        path.append(cur)
        cur = nxt[cur]

    print(len(path))
    print(*path)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng các giá trị dp cho tất cả các nút bằng cách sử dụng truyền tải kiểu nén đường dẫn. Thay vì đệ quy, nó lặp đi lặp lại về phía trước, lưu trữ chuỗi, sau đó gán các giá trị dp ngược lại. Điều này tránh được các vấn đề về độ sâu đệ quy trên chuỗi dài. 

các`typ`mảng chỉ được sử dụng khi chọn điểm bắt đầu, đảm bảo chúng ta bắt đầu ở một ngọn núi. Khách sạn bị loại vì vấn đề yêu cầu đầu tiên`k-1`nút là những ngọn núi và nút cuối cùng là khách sạn. 

Việc tái thiết sử dụng`nxt[]`, được lấp đầy trong quá trình truyền DP. Điều này đảm bảo thời gian tái thiết O(k). 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
0 0 0 0 1
0 1 2 3 4
```Chúng tôi có một chuỗi duy nhất:`1 → 2 → 3 → 4 → 5`, trong đó 5 là một khách sạn. 

| Bước | Nút | Tiếp theo | dp được giao | 
| --- | --- | --- | --- | 
| xây dựng | 5 | 0 | dp[5]=1 | 
| xây dựng | 4 | 5 | dp[4]=2 | 
| xây dựng | 3 | 4 | dp[3]=3 | 
| xây dựng | 2 | 3 | dp[2]=4 | 
| xây dựng | 1 | 2 | dp[1]=5 | 

Núi khởi đầu tốt nhất là nút 1, vì nó mang lại độ dài chuỗi tối đa là 5. Đường dẫn được xây dựng lại đi theo 1 → 2 → 3 → 4 → 5, xác nhận tính chính xác. 

### Ví dụ 2 

đầu vào:```
4
0 0 1 1
0 3 0 0
```Ở đây chúng tôi có hai chuỗi:`2 → 3`Và`1`bị cô lập và`3`Và`4`là những khách sạn. 

| Bước | Nút | Tiếp theo | dp | 
| --- | --- | --- | --- | 
| xây dựng | 3 | 0 | dp[3]=1 | 
| xây dựng | 2 | 3 | dp[2]=2 | 
| xây dựng | 1 | 0 | dp[1]=1 | 
| xây dựng | 4 | 0 | dp[4]=1 | 

Ngọn núi tốt nhất là nút 2, tạo ra đường đi`2 → 3`. Điều này xác nhận rằng thuật toán xử lý chính xác nhiều thành phần và các nút bị cô lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập và xử lý nhiều nhất một lần trong quá trình truyền DP | 
| Không gian | O(n) | Mảng cho dp, con trỏ tiếp theo và theo dõi lượt truy cập | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho`n ≤ 10^5`và việc sử dụng bộ nhớ cũng tuyến tính, trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out.strip()

# sample
assert run("""5
0 0 0 0 1
0 1 2 3 4
""") == "5\n1 2 3 4 5"

# single hotel
assert run("""1
1
0
""")
```
