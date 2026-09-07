---
title: "CF 104555M - Tối đa hóa hiệu quả chuyến bay"
description: "Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh trong đó mỗi đỉnh đại diện cho một thành phố và mỗi cặp thành phố đều có đường bay thẳng với chi phí đã biết. Ma trận chi phí có tính đối xứng nên việc di chuyển giữa hai thành phố có chi phí như nhau ở cả hai hướng."
date: "2026-06-30T08:52:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104555
codeforces_index: "M"
codeforces_contest_name: "2023-2024 ICPC Brazil Subregional Programming Contest"
rating: 0
weight: 104555
solve_time_s: 62
verified: true
draft: false
---

[CF 104555M - Tối đa hóa hiệu quả chuyến bay](https://codeforces.com/problemset/problem/104555/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có trọng số hoàn chỉnh trong đó mỗi đỉnh đại diện cho một thành phố và mỗi cặp thành phố đều có đường bay thẳng với chi phí đã biết. Ma trận chi phí có tính đối xứng nên việc di chuyển giữa hai thành phố có chi phí như nhau ở cả hai hướng. Mục đích là để quyết định xem liệu giá chuyến bay thẳng này có phù hợp nội bộ với lý luận về đường đi ngắn nhất hay không, và nếu có, để xác định có bao nhiêu chuyến bay thẳng là không cần thiết vì đường bay gián tiếp không bao giờ đắt hơn chúng. 

Một bảng được coi là nhất quán khi, đối với mỗi cặp thành phố, chi phí chuyến bay trực tiếp đã là cách rẻ nhất có thể để đi lại giữa chúng. Theo thuật ngữ đồ thị, điều này có nghĩa là ma trận kề đã cho phải đáp ứng thuộc tính đường đi ngắn nhất tất cả các cặp. 

Nếu bảng không nhất quán thì sự hiện diện của tuyến đường gián tiếp rẻ hơn sẽ làm mất hiệu lực mô hình định giá và chúng tôi phải xuất -1. 

Nếu nó nhất quán, chúng ta được phép loại bỏ càng nhiều cạnh trực tiếp càng tốt, nhưng chỉ những cạnh dư thừa theo nghĩa tồn tại một tuyến đường thay thế có chi phí chính xác bằng cạnh trực tiếp. Việc loại bỏ không được làm tăng chi phí đường đi ngắn nhất giữa bất kỳ cặp thành phố nào. 

Các ràng buộc cho phép tối đa 100 thành phố, vì vậy chúng tôi đang xử lý tối đa 10.000 mục trong ma trận. Thuật toán bậc ba trong N có thể chấp nhận được. Bất cứ điều gì tệ hơn O(N^3) sẽ là không cần thiết, trong khi O(N^4) đã gần đạt đến giới hạn thoải mái trên. 

Trường hợp cạnh tinh tế phát sinh khi cạnh trực tiếp kém hơn hẳn so với đường dẫn hai bước nhảy. Ví dụ: nếu chúng ta có:```
0 5 10
5 0 4
10 4 0
```Ở đây, đường 1 → 2 → 3 có chi phí là 9, rẻ hơn chi phí trực tiếp 1 → 3 là 10. Điều này có nghĩa là bảng không mạch lạc và phải trả về -1. Một cách tiếp cận đơn giản chỉ kiểm tra sự bất đẳng thức của tam giác theo một hướng nhưng bỏ qua các so sánh trung gian có thể thất bại nếu không được áp dụng một cách có hệ thống trên tất cả các bộ ba. 

Một trường hợp cạnh khác xảy ra khi tồn tại nhiều đường dẫn có chi phí bằng nhau. Ví dụ:```
0 2 2
2 0 2
2 2 0
```Tất cả các cạnh trực tiếp đều đã tối ưu, nhưng không thể loại bỏ bất kỳ cạnh nào vì việc loại bỏ bất kỳ cạnh nào sẽ làm tăng chi phí đường đi ngắn nhất. Cách tiếp cận ngây thơ “loại bỏ nếu tồn tại một đường dẫn” sẽ loại bỏ các cạnh một cách không chính xác ngay cả khi đường đi thay thế không hoàn toàn ngắn hơn nhưng bằng nhau, nhưng vẫn phải duy trì sự bằng nhau về đường đi ngắn nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tính toán các đường đi ngắn nhất giữa mỗi cặp thành phố bằng cách liên tục nới lỏng các cạnh hoặc chạy Dijkstra từ mỗi nút, sau đó so sánh khoảng cách đường đi ngắn nhất được tính toán với ma trận đã cho. Nếu bất kỳ cặp nào có sự không khớp trong đó cạnh đã cho không bằng đường đi ngắn nhất được tính toán thì bảng đó sẽ không mạch lạc. 

Cách tiếp cận này đúng vì nó trực tiếp kiểm tra xem ma trận đã mã hóa giải pháp đường đi ngắn nhất cho tất cả các cặp hay chưa. Tuy nhiên, việc chạy Dijkstra từ mỗi nút tốn O(N^3 log N) hoặc O(N^3) khi tối ưu hóa và Floyd-Warshall cũng tốn O(N^3), do đó, bạo lực đã ở mức giới hạn nhưng vẫn có thể chấp nhận được. Sự kém hiệu quả thực sự xuất hiện khi chúng tôi cố gắng kiểm tra bổ sung việc loại bỏ cạnh riêng lẻ, điều này sẽ làm tăng độ phức tạp lên gấp bội với hệ số N^2 khác. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng việc loại bỏ. Sau khi tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall, chúng tôi có thể xác thực đồng thời tính mạch lạc và tính dự phòng. Một cạnh giữa i và j có thể tháo rời được nếu tồn tại một số nút trung gian k sao cho i → k → j đạt được chi phí chính xác như cạnh trực tiếp. Nếu bất kỳ đường dẫn trung gian nào nhỏ hơn cạnh trực tiếp thì bảng đó không hợp lệ. 

Điều này làm giảm vấn đề xuống còn một lần chạy Floyd-Warshall, sau đó là quét ba lần trên tất cả các cặp và sản phẩm trung gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đường dẫn ngắn nhất cho mỗi cặp + kiểm tra) | O(N^3) đến O(N^4) | O(N^2) | Quá chậm | 
| Kiểm tra tối ưu hóa Floyd-Warshall | O(N^3) | O(N^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc ma trận và lưu nó dưới dạng dist. Điều này thể hiện cả các cạnh trực tiếp đầu vào và bảng đường đi ngắn nhất đang hoạt động của chúng tôi. Chúng tôi giữ nó không thay đổi ban đầu vì chúng tôi sẽ dần dần tinh chỉnh nó bằng cách sử dụng các đỉnh trung gian. 
2. Chạy Floyd-Warshall trên tất cả các bộ ba (k, i, j), cập nhật dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]). Bước này tính toán đường đi ngắn nhất thực sự giữa mỗi cặp bằng cách sử dụng bất kỳ thành phố trung gian nào. Lý do điều này có hiệu quả là vì bất kỳ đường đi ngắn nhất nào cũng có thể được phân tách thành các đường dẫn con có các đỉnh trung gian cuối cùng sẽ được coi là khi k tăng. 
3. Sau khi tính toán các đường đi ngắn nhất, hãy xác minh tính nhất quán bằng cách kiểm tra xem liệu với mỗi cặp (i, j), chi phí trực tiếp ban đầu có bằng chi phí đường đi ngắn nhất được tính toán hay không. Nếu bất kỳ cạnh trực tiếp nào lớn hơn đường đi ngắn nhất thì bảng đó không nhất quán và chúng tôi ngay lập tức xuất ra -1. Điều này đảm bảo không có đường bay gián tiếp nào rẻ hơn đường bay thẳng. 
4. Nếu sự mạch lạc được giữ vững, chúng ta sẽ đếm các cạnh có thể tháo rời được. Với mỗi cặp (i, j), chúng ta kiểm tra xem có tồn tại một số nút trung gian k khác với i và j sao cho dist[i][j] bằng dist[i][k] + dist[k][j]. Nếu k như vậy tồn tại thì cạnh trực tiếp là dư thừa vì một đường đi thay thế đạt được cùng chi phí tối ưu. 
5. Để tránh tính hai lần, chúng ta chỉ xét cặp i < j vì đồ thị là vô hướng. Mỗi cạnh có thể tháo rời đóng góp chính xác một cạnh cho câu trả lời. 
6. Xuất ra tổng số cạnh có thể tháo rời. 

### Tại sao nó hoạt động

Bước Floyd-Warshall đảm bảo dist chứa khoảng cách đường đi ngắn nhất thực sự trong tất cả các tuyến đường có thể. Nếu bất kỳ cạnh trực tiếp nào lớn hơn giá trị này thì nó không thể là một phần của bất kỳ cấu trúc tối ưu nào và đầu vào không nhất quán. Nếu đẳng thức giữ nguyên thì cạnh trực tiếp đã là tối ưu, nhưng nó có thể là duy nhất hoặc không. Việc kiểm tra k trung gian bảo toàn đẳng thức sẽ xác định chính xác khi nào cạnh không được yêu cầu duy nhất. Bởi vì bất kỳ đường đi ngắn nhất nào có chi phí bằng nhau đều đủ để duy trì khoảng cách tất cả các cặp, việc loại bỏ cạnh đó không làm thay đổi bất kỳ giá trị đường đi ngắn nhất nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    dist = [list(map(int, input().split())) for _ in range(n)]

    # Floyd–Warshall
    for k in range(n):
        for i in range(n):
            dik = dist[i][k]
            if dik == 10**18:
                continue
            for j in range(n):
                nd = dik + dist[k][j]
                if nd < dist[i][j]:
                    dist[i][j] = nd

    # Check coherence
    for i in range(n):
        for j in range(n):
            if dist[i][j] != dist[i][j]:
                pass
    # Actually we need original matrix, so recompute carefully
    # Store original
    # (Fix approach: re-read logic cleanly)

def solve():
    n = int(input())
    orig = [list(map(int, input().split())) for _ in range(n)]
    dist = [row[:] for row in orig]

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    # coherence check
    for i in range(n):
        for j in range(n):
            if dist[i][j] != orig[i][j]:
                print(-1)
                return

    removable = 0

    for i in range(n):
        for j in range(i + 1, n):
            for k in range(n):
                if k != i and k != j:
                    if dist[i][j] == dist[i][k] + dist[k][j]:
                        removable += 1
                        break

    print(removable)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sao chép ma trận đầu vào để chúng tôi duy trì chi phí chuyến bay trực tiếp ban đầu trong khi tính toán riêng các đường đi ngắn nhất. Floyd-Warshall sau đó biến hình`dist`vào ma trận đường đi ngắn nhất tất cả các cặp thực sự. 

Kiểm tra tính mạch lạc sẽ so sánh từng cặp với ma trận ban đầu. Bất kỳ sự không khớp nào có nghĩa là đầu vào chứa chuyến bay trực tiếp dưới mức tối ưu, do đó cấu trúc không hợp lệ. 

Vòng lặp cuối cùng đếm các cạnh có sự phân tách chi phí bằng nhau thay thế thông qua một số nút trung gian. các`break`đảm bảo mỗi cạnh chỉ được tính một lần, vì chúng ta chỉ cần tồn tại một nút chứng kiến ​​như vậy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
0 1 2
1 0 1
2 1 0
```Sau Floyd-Warshall, những con đường ngắn nhất vẫn giữ nguyên: 

| tôi | j | bản gốc | ngắn nhất | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 
| 0 | 2 | 2 | 2 | 
| 1 | 2 | 1 | 1 | 

Không có cặp nào có lộ trình gián tiếp tốt hơn. Tuy nhiên, cạnh (0,2) là dư thừa vì 0 → 1 → 2 có giá trị 1 + 1 = 2, khớp với cạnh trực tiếp. 

Vì vậy, chúng ta có thể loại bỏ chính xác một cạnh. 

Đầu ra:```
1
```### Ví dụ 2 

đầu vào:```
3
0 2 2
2 0 2
2 2 0
```Floyd-Warshall không cải thiện bất kỳ giá trị nào. Mỗi cặp đã có cạnh trực tiếp là chi phí đường đi ngắn nhất duy nhất. 

Kiểm tra khả năng tháo rời: 

Đối với (0,1), bất kỳ đường đi nào qua 2 đều cho 2 + 2 = 4, tệ hơn 2. Tương tự cho tất cả các cặp. 

Không có cạnh nào có thể tháo rời được. 

Đầu ra:```
0
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N^3) | Floyd-Warshall thống trị với ba vòng lặp lồng nhau trên các thành phố | 
| Không gian | O(N^2) | Hai ma trận lưu trữ khoảng cách đường đi ban đầu và ngắn nhất | 

Với N ≤ 100, 10^6 lần lặp cho mỗi cấp độ vòng lặp là có thể chấp nhận được. Các hệ số không đổi là nhỏ và nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve_output(inp)).strip()

# Re-define safe runner since solve prints
def solve_output(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)

    out = StringIO()
    backup_out = sys.stdout
    sys.stdout = out

    solve()

    sys.stdin = backup
    sys.stdout = backup_out
    return out.getvalue()

# provided samples
assert solve_output("""3
0 1 2
1 0 1
2 1 0
""") == "1\n"

assert solve_output("""3
0 2 2
2 0 2
2 2 0
""") == "0\n"

# custom cases
assert solve_output("""2
0 5
5 0
""") == "0\n", "minimum non-trivial graph"

assert solve_output("""3
0 1 10
1 0 1
10 1 0
""") == "-1\n", "incoherent triangle violation"

assert solve_output("""4
0 1 2 3
1 0 1 2
2 1 0 1
3 2 1 0
""") == "3\n", "chain redundancy"

assert solve_output("""1
0
""") == "0\n", "single node"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đối xứng 2 nút | 0 | cấu trúc tối thiểu, không cần xóa | 
| vi phạm tam giác | -1 | phát hiện không mạch lạc | 
| đồ thị chuỗi | 3 | trường hợp dự phòng tối đa | 
| nút đơn | 0 | điều kiện biên | 

## Vỏ cạnh 

Đối với một thành phố, không có cạnh nào để xác thực hoặc xóa và thuật toán ngay lập tức tạo ra số 0 sau khi bỏ qua cả kiểm tra cặp và cải tiến Floyd-Warshall. 

Đối với một tam giác trong đó một cạnh tệ hơn rất nhiều so với đường dẫn hai bước, bước Floyd-Warshall sẽ giảm thiểu mục nhập đó một cách nghiêm ngặt, gây ra sự không khớp ngay lập tức với ma trận ban đầu và trả về -1. Điều này ngăn cản mọi nỗ lực đếm các cạnh có thể tháo rời trên dữ liệu không hợp lệ. 

Đối với các đồ thị hoàn chỉnh có chi phí bằng nhau, mỗi cạnh đều bằng trực tiếp với tất cả các đường dẫn hai bước nhảy thay thế, do đó mỗi cạnh được đánh dấu có thể tháo rời một lần. Thuật toán đếm chính xác từng cặp vô hướng chính xác một lần vì vòng lặp bên trong chỉ tăng khi tồn tại ít nhất một đẳng thức trung gian.
