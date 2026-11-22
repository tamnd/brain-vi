---
title: "CF 290F - Petya tham lam"
description: "Chúng ta được cho một đồ thị vô hướng có tối đa 20 đỉnh và tối đa 400 cạnh, và chúng ta cần quyết định xem có tồn tại một đường đi đơn giản đi qua mọi đỉnh đúng một lần hay không."
date: "2026-06-05T10:38:11+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 290
codeforces_index: "F"
codeforces_contest_name: "April Fools Day Contest 2013"
rating: 2800
weight: 290
solve_time_s: 68
verified: true
draft: false
---

[CF 290F - Petya tham lam](https://codeforces.com/problemset/problem/290/F) 

**Đánh giá:** 2800 
**Thẻ:** *đặc biệt, dfs và tương tự, đồ thị, tham lam 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có tối đa 20 đỉnh và tối đa 400 cạnh, và chúng ta cần quyết định xem có tồn tại một đường đi đơn giản đi qua mọi đỉnh đúng một lần hay không. Đây là bài toán đường đi Hamilton ở dạng quyết định của nó: chúng ta không được yêu cầu xây dựng đường đi mà chỉ nói liệu có tồn tại ít nhất một thứ tự các đỉnh như vậy khi các đỉnh liên tiếp được nối với nhau bằng các cạnh hay không. 

Chi tiết chính là tỷ lệ của biểu đồ. Với tối đa 20 đỉnh, các phương pháp hàm mũ trở nên khả thi, nhưng bất cứ điều gì như liệt kê trực tiếp tất cả các hoán vị của các đỉnh sẽ là đường biên vì$20!$là quá lớn. Tuy nhiên, bất kỳ giải pháp nào nén trạng thái trên các tập con đỉnh, theo thứ tự$2^n \cdot n^2$, là khả thi một cách thoải mái. 

Một điểm tinh tế là đồ thị có thể chứa các vòng tự lặp hoặc nhiều cạnh và những cạnh này không ảnh hưởng đến sự tồn tại của đường đi Hamilton. Vòng lặp tự không bao giờ giúp ích trong một đường dẫn phải di chuyển giữa các đỉnh riêng biệt và các cạnh trùng lặp là dư thừa để kết nối. 

Một sai lầm ngây thơ thường xuất hiện là coi đây là sự cố kết nối. Ví dụ, việc kiểm tra xem đồ thị có được kết nối hay không và tất cả các đỉnh có bậc ít nhất là một là chưa đủ. Một đồ thị liên thông giống như một ngôi sao có 20 đỉnh có tâm nối với tất cả các lá nhưng không có đường đi Hamilton, vì bất kỳ đường đi nào đi vào tâm chỉ có thể rời khỏi nó một lần, khiến cho việc đi qua tất cả các lá trong một chuỗi là không thể. 

Một trường hợp thất bại khác đến từ sự vượt qua tham lam. Bắt đầu từ một nút ngẫu nhiên và luôn đi đến một nút hàng xóm không được ghé thăm có thể bị kẹt sớm ngay cả khi tồn tại một đường dẫn Hamiltonian hợp lệ. Lựa chọn cục bộ không mã hóa các ràng buộc toàn cục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các hoán vị của các đỉnh và kiểm tra xem các đỉnh liên tiếp có được nối với nhau bằng các cạnh hay không. Đối với mỗi hoán vị, chúng tôi xác minh sự kề cận trong$O(n)$, và có$n!$hoán vị, tạo ra độ phức tạp tổng thể của$O(n! \cdot n)$. Thậm chí tại$n = 20$, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc của bài toán thay đổi đáng kể khi chúng ta nhận thấy rằng chúng ta chỉ quan tâm đến tập con đỉnh nào đã được thăm và đỉnh cuối cùng trên đường đi. Hai hoán vị khác nhau truy cập vào cùng một tập đỉnh và kết thúc ở cùng một đỉnh là tương đương nhau về khả năng mở rộng trong tương lai. Điều này gợi ý một công thức quy hoạch động trên các tập hợp con. 

Chúng tôi xác định trạng thái nắm bắt một tập hợp con các đỉnh đã truy cập và điểm cuối của đường dẫn. Từ bất kỳ trạng thái nào, chúng tôi cố gắng mở rộng đường dẫn đến một người hàng xóm chưa được ghé thăm. Điều này làm giảm không gian tìm kiếm từ hoán vị đến chuyển đổi tập hợp con, biến một bảng liệt kê khó hiểu thành một bảng liệt kê có thể quản lý được.$2^n \cdot n$không gian trạng thái. 

Đây là đường đi Hamiltonian DP tiêu chuẩn, và ở đây nó hoàn toàn phù hợp vì$n \le 20$, làm$2^n \approx 10^6$, đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị) |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Bitmask DP qua đường dẫn |$O(n^2 \cdot 2^n)$|$O(n \cdot 2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một bảng lập trình động trong đó chúng tôi theo dõi xem một tập hợp con các đỉnh nhất định có thể tạo thành một đường dẫn hợp lệ kết thúc tại một đỉnh cụ thể hay không. 

1. Chúng ta biểu diễn từng tập hợp con các đỉnh bằng cách sử dụng mặt nạ bit có độ dài$n$. Mặt nạ bit mã hóa chính xác những đỉnh nào đã được đưa vào đường dẫn một phần. Cách biểu diễn này là tự nhiên vì các chuyển tiếp chỉ thêm một đỉnh tại một thời điểm. 
2. Chúng tôi xác định một mảng DP trong đó`dp[mask][v]`đúng nếu tồn tại một đường đi thăm chính xác các đỉnh trong`mask`và kết thúc ở đỉnh`v`. Trạng thái này nắm bắt tất cả thông tin cần thiết để mở rộng đường dẫn hơn nữa. 
3. Chúng tôi khởi tạo DP bằng cách cài đặt`dp[1 << v][v] = True`cho mọi đỉnh`v`. Mỗi đỉnh riêng lẻ tạo thành một đường đi tầm thường có độ dài bằng một. 
4. Chúng tôi lặp lại tất cả các mặt nạ từ nhỏ đến lớn. Đối với mỗi mặt nạ, chúng tôi thử tất cả các điểm cuối có thể`v`. Nếu như`dp[mask][v]`là đúng, chúng tôi cố gắng mở rộng con đường. 
5. Để mở rộng một trạng thái`(mask, v)`, chúng tôi thử tất cả hàng xóm`to`của`v`. Nếu như`to`không có trong`mask`, chúng tôi thiết lập`dp[mask | (1 << to)][to] = True`. Bước này tương ứng chính xác với việc thêm một đỉnh mới vào đường dẫn hiện tại. 
6. Sau khi xử lý tất cả các trạng thái, chúng ta kiểm tra xem có tồn tại đỉnh nào không`v`như vậy`dp[(1 << n) - 1][v]`là đúng. Nếu vậy thì tồn tại một đường đi Hamilton. 

Tính đúng đắn phụ thuộc vào thực tế là mọi đường đi Hamilton hợp lệ đều có đỉnh cuối cùng và DP cuối cùng sẽ xây dựng chính xác chuỗi các tập hợp con dẫn đến đỉnh đó. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[mask][v]`đúng khi và chỉ nếu tồn tại một đường đi đơn đi qua chính xác các đỉnh trong`mask`và kết thúc tại`v`. Mọi chuyển đổi đều duy trì tính đơn giản vì chúng tôi chỉ di chuyển đến các đỉnh chưa được thăm và mọi phần mở rộng có thể có của đường dẫn hợp lệ đều được xem xét. Vì mọi đường đi Hamilton tương ứng với một số thứ tự của phép cộng tập hợp con, nên nó phải xuất hiện trong không gian trạng thái này và không có trạng thái không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[False] * n for _ in range(n)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        adj[u][v] = True
        adj[v][u] = True

    if n == 1:
        print("Yes")
        return

    size = 1 << n
    dp = [[False] * n for _ in range(size)]

    for i in range(n):
        dp[1 << i][i] = True

    for mask in range(size):
        for v in range(n):
            if not dp[mask][v]:
                continue
            if mask == (1 << n) - 1:
                continue
            for to in range(n):
                if not adj[v][to]:
                    continue
                if mask & (1 << to):
                    continue
                dp[mask | (1 << to)][to] = True

    full = (1 << n) - 1
    for v in range(n):
        if dp[full][v]:
            print("Yes")
            return

    print("No")

if __name__ == "__main__":
    solve()
```Ma trận kề được sử dụng sao cho việc kiểm tra cạnh có thời gian không đổi. Điều này rất quan trọng vì DP đã chạy trên tất cả các tập hợp con và điểm cuối và mọi chi phí logarit cho mỗi lần chuyển đổi sẽ quá tốn kém. 

Bảng DP được lập chỉ mục bằng bitmask trước tiên vì phép lặp tập hợp con là vòng lặp bên ngoài tự nhiên. Mỗi trạng thái chỉ mở rộng dọc theo các cạnh thực tế và chúng tôi bỏ qua rõ ràng các chuyển đổi sang các đỉnh đã truy cập để đảm bảo tính hợp lệ của đường dẫn. 

Lần quét cuối cùng kết thúc`dp[full]`kiểm tra xem có thể truy cập được bất kỳ điểm cuối nào sau khi truy cập tất cả các đỉnh hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào:```
3 2
1 2
2 3
```Chúng ta mong đợi một đường đi Hamilton như 1 → 2 → 3. 

| mặt nạ | điểm cuối v | trạng thái dp | chuyển tiếp | 
| --- | --- | --- | --- | 
| 001 | 1 | đúng | bắt đầu | 
| 010 | 2 | đúng | bắt đầu | 
| 100 | 3 | đúng | bắt đầu | 
| 011 | 2 | đúng | 1→2 | 
| 110 | 3 | đúng | 2→3 | 
| 111 | 3 | đúng | 1→2→3 | 

Ở mặt nạ cuối cùng, có thể truy cập được ít nhất một điểm cuối nên câu trả lời là “Có”. 

Dấu vết này cho thấy các đường dẫn từng phần hợp nhất một cách tự nhiên như thế nào khi các tập hợp con phát triển. 

### Ví dụ 2 

đầu vào:```
4 2
1 2
3 4
```Đồ thị bị ngắt kết nối nên không tồn tại đường đi Hamilton. 

Tất cả các trạng thái DP vẫn bị giới hạn trong các thành phần của chúng và không có quá trình chuyển đổi nào có thể chuyển từ {1,2} sang {3,4}. Do đó không có trạng thái nào đạt đến mặt nạ 1111. 

DP không bao giờ tạo ra trạng thái mặt nạ đầy đủ, xác nhận rằng chỉ kết nối bên trong các tập hợp con là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \cdot 2^n)$| Mỗi trạng thái tập hợp con cố gắng lên tới$n$điểm cuối và lên đến$n$hàng xóm | 
| Không gian |$O(n \cdot 2^n)$| Bảng DP lưu trữ trạng thái cho từng mặt nạ và điểm cuối | 

Với$n \le 20$,$2^n \approx 10^6$, và hệ số không đổi xung quanh$n^2$, giải pháp vừa vặn thoải mái trong giới hạn trong Python với tối ưu hóa ma trận kề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    output = []
    def fake_print(*args):
        output.append(" ".join(map(str, args)))

    global print
    old_print = print
    print = fake_print
    try:
        solve()
    finally:
        print = old_print

    return "\n".join(output) + ("\n" if output else "")

# provided sample
assert run("2 3\n1 2\n2 1\n1 1\n") == "Yes\n", "sample 1"

# simple path
assert run("3 2\n1 2\n2 3\n") == "Yes\n"

# disconnected graph
assert run("4 2\n1 2\n3 4\n") == "No\n"

# single node
assert run("1 0\n") == "Yes\n"

# star graph (no Hamiltonian path)
assert run("5 4\n1 2\n1 3\n1 4\n1 5\n") == "No\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | Có | trường hợp cơ bản tầm thường | 
| chuỗi tuyến tính | Có | lan truyền DP bình thường | 
| cặp bị ngắt kết nối | Không | cách ly thành phần | 
| đồ thị sao | Không | trường hợp cạm bẫy tham lam | 

## Vỏ cạnh 

Trường hợp một cạnh là đồ thị một đỉnh. DP khởi tạo`dp[1<<0][0] = true`, và vì điều này đã tương ứng với mặt nạ đầy đủ nên thuật toán ngay lập tức thành công, xuất ra chính xác “Có”. 

Một trường hợp khác là đồ thị có các đỉnh cô lập. Ví dụ: nếu đỉnh 5 không có cạnh thì nó chỉ có thể xuất hiện dưới dạng một đường dẫn độc lập. DP xử lý chính xác điều này vì các trạng thái bắt đầu từ 5 không thể mở rộng và không thể truy cập được trạng thái mặt nạ đầy đủ. 

Một trường hợp tinh tế hơn là các biểu đồ dày đặc thiếu các cạnh quan trọng làm cản trở thứ tự đầy đủ. Ngay cả khi hầu hết các đỉnh đều có tính kết nối cao, DP vẫn tôn trọng các ràng buộc kề cận chính xác, do đó nó không bao giờ kết hợp sai các đường dẫn từng phần không tương thích.
