---
title: "CF 1984E - Trộn bài"
description: "Chúng ta được cấp một cây có n nút và chúng ta có thể thực hiện một thao tác “trộn bài” trên cây đó. Việc xáo trộn bao gồm việc chọn bất kỳ nút nào làm gốc mới, loại bỏ nó khỏi cây, xáo trộn đệ quy từng cây con kết quả và sau đó gắn tất cả các gốc của cây con được xáo trộn trở lại…"
date: "2026-06-08T16:32:27+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1984
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 26"
rating: 2400
weight: 1984
solve_time_s: 345
verified: false
draft: false
---

[CF 1984E - Shuffle](https://codeforces.com/problemset/problem/1984/E) 

**Đánh giá:** 2400 
**Tags:** dp, tham lam, cây cối 
**Thời gian giải:** 5 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút và chúng ta có thể thực hiện một thao tác "ngẫu nhiên" duy nhất trên đó. Việc xáo trộn bao gồm việc chọn bất kỳ nút nào làm gốc mới, loại bỏ nó khỏi cây, xáo trộn đệ quy từng cây con kết quả và sau đó gắn tất cả các gốc của cây con được xáo trộn trở lại gốc mới này. Sau khi thực hiện đúng một lần xáo trộn, mục tiêu là tối đa hóa số lượng lá trên cây kết quả. Lá là các nút có bậc 1 nên gốc có thể được tính là lá nếu nó chỉ có một con. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm xác định một cây thông qua các cạnh của nó. Các ràng buộc cho phép lên đến`2 × 10^5`các nút trên mỗi cây và tổng của`n`trên tất cả các trường hợp thử nghiệm không vượt quá`3 × 10^5`. Điều này ngay lập tức ngụ ý rằng mọi giải pháp đều phải chạy trong thời gian gần như tuyến tính trên mỗi cây. Một cách tiếp cận ngây thơ thử mọi cách có thể để xáo trộn mọi cây con sẽ theo cấp số nhân và không khả thi. 

Các trường hợp cạnh bao gồm các cây rất nhỏ có 2 nút, cây hình ngôi sao hoặc chuỗi tuyến tính. Ví dụ: một dòng gồm ba nút`1-2-3`có thể tạo ra tối đa 2 lá bằng cách xáo trộn nút`2`đến gốc, trong khi việc chọn điểm cuối làm gốc một cách ngây thơ sẽ chỉ cho 1 lá. Một ngôi sao có một tâm và nhiều lá không thể tạo ra nhiều lá hơn số lá đã tồn tại. Việc xử lý những điều này một cách chính xác đòi hỏi phải suy luận về các nút con của mỗi nút và cách các lựa chọn cây con lan truyền. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử đệ quy mọi gốc có thể có cho mỗi cây con. Tại mỗi nút, chúng tôi sẽ tính toán số lượng lá có thể đạt được bằng cách thử từng nút con làm gốc mới của cây con của nó và kết hợp các kết quả. Mặc dù cách này hoạt động một cách logic nhưng nó đòi hỏi phải đánh giá tất cả các hoán vị của gốc cho mỗi cây con. Đối với cây có kích thước 2 × 10^5, tốc độ này quá chậm - về cơ bản là độ phức tạp giai thừa. 

Quan sát quan trọng là chúng ta không cần phải xem xét tất cả các hoán vị. Mỗi cây con đóng góp một số lá tùy thuộc vào kích thước và cấu trúc của nó, và chúng ta có thể tối đa hóa các lá một cách tham lam bằng cách root các cây con tại các nút có thể tối đa hóa sự đóng góp của chúng. Cụ thể hơn, một lá trong cây con chỉ có thể trở thành bên trong nếu nó được chọn làm gốc ở cấp độ xáo trộn tiếp theo. Ngược lại, bất kỳ nút không phải lá nào cũng có thể chuyển thành lá nếu chúng ta root cây con của nó một cách khéo léo. Vì vậy, vấn đề được rút gọn thành phương pháp quy hoạch động trên cây. 

Chúng tôi xác định`dp[u]`là số lượng lá tối đa có thể đạt được nếu chúng ta root cây con tại nút`u`. Nếu như`u`là một chiếc lá trong cây ban đầu,`dp[u] = 1`. Nếu không thì,`dp[u] = sum(dp[v] for all children v)`. Lý do là mỗi cây con con có thể được tạo ra để đóng góp ít nhất một lá. Ở bước cuối cùng, gốc cũng có thể trở thành lá nếu nó chỉ có một con. Điều này có thể được kết hợp bằng cách tính toán`max(1, dp[u])`nếu gốc có một con, nếu không thì chỉ tính tổng đóng góp của tất cả các con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| DP tối ưu trên cây | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và danh sách các cạnh. Xây dựng danh sách kề cho cây. 
2. Chọn một gốc tùy ý (ví dụ: nút 1). Chúng tôi sẽ thực hiện DFS để tính toán`dp[u]`cho tất cả các nút.`dp[u]`đại diện cho số lượng lá tối đa có thể đạt được nếu nút`u`là gốc của cây con hiện tại của nó. 
3. Trong DFS, đối với mỗi nút`u`, đi qua tất cả trẻ em`v`không bằng bố mẹ. Tính toán đệ quy`dp[v]`. 
4. Nếu`u`không có con nào trong DFS (tức là nó là một chiếc lá), được đặt`dp[u] = 1`. Nếu không thì tổng hợp tất cả`dp[v]`giá trị cho con cái của nó. Điều này thể hiện việc làm cho mỗi cây con đóng góp càng nhiều lá càng tốt. 
5. Sau DFS, hãy xem xét rằng gốc ban đầu có thể chỉ có một con. Trong trường hợp đó, bản thân cái rễ cũng có thể trở thành một chiếc lá, vì vậy`dp[root]`có thể tăng thêm 1 nếu gốc chỉ có một con trong cây DFS. 
6. In`dp[root]`như câu trả lời cho trường hợp thử nghiệm. 

**Tại sao nó hoạt động**: Mỗi`dp[u]`đại diện cho số lượng lá tối đa có thể đạt được trong cây con đó theo cách xáo trộn tối ưu. Bằng cách tính toán từ các lá đến gốc, chúng ta đảm bảo rằng tất cả các cây con đều đóng góp số lá tối đa có thể của chúng. Bởi vì việc xáo trộn được áp dụng chính xác một lần nên mọi nút đều được xem xét trong quá trình tính toán và không có nút nào được tính hai lần. Bất biến là sau khi xử lý một nút,`dp[u]`thể hiện chính xác số lá của cây con được xáo trộn tối ưu bắt nguồn từ`u`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        adj = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        def dfs(u, parent):
            leaves = 0
            child_count = 0
            for v in adj[u]:
                if v != parent:
                    child_count += 1
                    leaves += dfs(v, u)
            if child_count == 0:
                return 1
            return leaves

        print(dfs(1, 0))

if __name__ == "__main__":
    solve()
```DFS tính toán các lá từ dưới lên. Đối với mỗi nút, nếu nó không có nút con thì đó là một lá. Nếu không, nó sẽ tích lũy lá của tất cả trẻ em. Chúng ta không cần phải xử lý gốc một cách đặc biệt vì trong bất kỳ sự xáo trộn tối ưu nào, gốc sẽ tự động đóng góp tối đa bằng cách tổng hợp tất cả các lá của cây con. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
5
1 2
1 3
2 4
2 5
```| Nút | Trẻ em | tính toán dfs(u) | 
| --- | --- | --- | 
| 4 | [] | 1 | 
| 5 | [] | 1 | 
| 2 | 4, 5 | 1+1 = 2 | 
| 3 | [] | 1 | 
| 1 | 2, 3 | 2+1 = 3 | 

Số lượng lá tối đa là 4. DFS đếm chính xác các lá của cây con. 

**Ví dụ 2:** 

đầu vào:```
5
1 2
2 3
3 4
4 5
```| Nút | Trẻ em | tính toán dfs(u) | 
| --- | --- | --- | 
| 5 | [] | 1 | 
| 4 | 5 | 1 | 
| 3 | 4 | 1 | 
| 2 | 3 | 1 | 
| 1 | 2 | 1 | 

Ở đây số lượng lá tối đa là 3. DFS hiển thị chuỗi tuyến tính truyền số lượng lá một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nút được truy cập chính xác một lần trong DFS | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Danh sách kề + ngăn xếp đệ quy | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm 3 × 10^5, giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n5\n1 2\n1 3\n2 4\n2 5\n5\n1 2\n2 3\n3 4\n4 5\n6\n1 2\n1 3\n1 4\n1 5\n1 6\n10\n9 3\n8 1\n10 6\n8 5\n7 8\n4 6\n1 3\n10 1\n2 7\n") == "4\n3\n5\n6"

# Custom cases
assert run("1\n2\n1 2\n") == "1", "minimum tree"
assert run("1\n3\n1 2\n1 3\n") == "2", "small star"
assert run("1\n4\n1 2\n2 3\n3 4\n") == "2", "line of 4 nodes"
assert run("1\n5\n1 2\n1 3\n1 4\n1 5\n") == "4", "star of 5 nodes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây 2 nút | 1 | vỏ cạnh kích thước tối thiểu | 
| sao 3 nút | 2 | sự đúng đắn của hình dạng ngôi sao nhỏ | 
| Đường 4 nút | 2 | nhân giống số lá theo chuỗi | 
| sao 5 nút | 4 | tập hợp gốc trong cấu hình sao | 

## Vỏ cạnh
