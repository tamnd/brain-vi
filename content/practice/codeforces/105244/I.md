---
title: "CF 105244I - Tổng độ dài đường dẫn"
description: "Đầu vào mô tả một cây có các đỉnh được đánh số từ 1 đến n. Mọi đỉnh ngoại trừ đỉnh đầu tiên đều có chính xác một đỉnh cha cho trước, điều này ngầm xác định một cạnh vô hướng giữa mỗi nút i và đỉnh pi gốc của nó."
date: "2026-06-24T07:03:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105244
codeforces_index: "I"
codeforces_contest_name: "Dynamic Programming, SPbSU 2024, Training 2"
rating: 0
weight: 105244
solve_time_s: 47
verified: true
draft: false
---

[CF 105244I - Tổng độ dài đường dẫn](https://codeforces.com/problemset/problem/105244/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một cây có các đỉnh được đánh số từ 1 đến n. Mọi đỉnh ngoại trừ đỉnh đầu tiên đều có chính xác một đỉnh cha đã cho, điều này ngầm xác định một cạnh vô hướng giữa mỗi nút i và đỉnh p_i gốc của nó. Bởi vì mỗi nút trỏ đến một nút cha có chỉ số nhỏ hơn nên cấu trúc được đảm bảo là một cây hợp lệ có gốc từ 1. 

Nhiệm vụ là xem xét mọi cặp đỉnh u và v, kể cả các cặp trong đó u bằng v, tính khoảng cách giữa chúng trong các cạnh và tính tổng tất cả các khoảng cách đó. Một cặp đóng góp bằng 0 khi cả hai điểm cuối đều có cùng một đỉnh và đóng góp số cạnh trên đường đơn giản duy nhất nếu không. 

Ràng buộc n lên tới 300000 buộc chúng ta phải tránh xa mọi giải pháp cố gắng tính khoảng cách theo từng cặp. Một ý tưởng kiểu đường dẫn ngắn nhất cho tất cả các cặp ngây thơ sẽ ngầm kiểm tra các cặp O(n^2), vốn đã có khoảng 9e10 hoạt động trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. Ngay cả việc lưu trữ hoặc lặp lại tất cả các cặp cũng không thể thực hiện được cả về thời gian và bộ nhớ. 

Một vấn đề phức tạp hơn phát sinh với BFS hoặc DFS đơn giản từ mọi nút. Mặc dù mỗi lần truyền tải là tuyến tính, nhưng việc lặp lại nó n lần sẽ nhân chi phí lên O(n^2), chi phí này lại bị thu gọn theo các ràng buộc. 

Các trường hợp cạnh chủ yếu là cấu trúc hơn là số. Khi n bằng 1, không có cạnh nào và do đó không có đường đi nào khác 0, nên đáp án phải bằng 0. Một trường hợp góc khác là cây có độ lệch cao, nhấn mạnh việc triển khai đệ quy đơn giản do độ sâu lên tới 300000, yêu cầu duyệt qua lặp lại hoặc xử lý đệ quy cẩn thận. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tính toán rõ ràng khoảng cách giữa mỗi cặp đỉnh. Đối với mỗi đỉnh bắt đầu u, chúng tôi chạy BFS hoặc DFS để tính khoảng cách đến tất cả các đỉnh v khác, sau đó tích lũy các khoảng cách đó. Mỗi lần truyền tải tốn O(n), lặp lại cho n điểm bắt đầu mang lại tổng công việc là O(n^2). Với n ở mức 300000, điều này sẽ yêu cầu thứ tự 10^10 thao tác, điều này không khả thi từ xa. 

Quan sát quan trọng là chúng ta thực sự không cần khoảng cách riêng lẻ. Chúng ta chỉ cần tổng đóng góp của mỗi cạnh trên tất cả các đường đi. Mọi đường đi đơn giữa hai đỉnh đều đi qua một tập cạnh cụ thể đúng một lần. Điều này cho phép chúng ta chuyển phối cảnh từ các cặp đỉnh sang các cạnh. 

Nếu chúng ta cố định một cạnh, chẳng hạn giữa một nút và nút cha của nó, thì việc loại bỏ cạnh này sẽ chia cây thành hai thành phần được kết nối. Bất kỳ đường đi nào đi từ nút trong thành phần này đến nút trong thành phần khác đều phải đi qua cạnh này đúng một lần. Do đó, số cặp có đường đi ngắn nhất sử dụng cạnh này chính xác là tích của kích thước của hai thành phần thu được. 

Điều này biến vấn đề thành tính toán kích thước cây con. Khi chúng ta root cây ở nút 1, mỗi cạnh sẽ kết nối một nút với nút gốc của nó và kích thước của cây con bắt nguồn từ một nút sẽ trực tiếp cho chúng ta biết có bao nhiêu đỉnh nằm trên một phía của cạnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng việc diễn giải mảng cha như một danh sách kề. Mỗi nút i từ 2 đến n được kết nối với p_i, tạo thành cây vô hướng.

1. Xây dựng danh sách kề từ biểu diễn gốc bằng cách thêm một cạnh giữa i và p_i với mọi i từ 2 đến n. Điều này cho phép truyền tải theo cả hai hướng, điều này cần thiết để tính toán kích thước cây con một cách rõ ràng. 
2. Gốc cây tại nút 1 và tính toán kích thước cây con bằng cách duyệt theo chiều sâu. Chúng tôi duy trì một mảng kích thước trong đó size[v] biểu thị số lượng nút tồn tại trong cây con của v, bao gồm cả chính v. 
3. Trong DFS, chúng tôi truy cập một nút và tính toán đệ quy kích thước của tất cả các nút con của nó. Sau khi xử lý c, chúng ta thêm size[c] vào size[v]. Bước tổng hợp này là bước tích lũy thông tin cây con trở lên. 
4. Sau khi tính toán tất cả kích thước cây con, chúng ta đánh giá từng cạnh trong danh sách gốc ban đầu. Đối với cạnh giữa i và p_i, giả sử cây con có gốc tại i là “phía con” của cạnh đó. Số cặp có đường đi qua cạnh này là size[i] nhân với n trừ size[i]. 
5. Tổng hợp các đóng góp này trên tất cả i từ 2 đến n để có được câu trả lời cuối cùng. 

Điểm tinh tế là chúng ta không bao giờ tính toán khoảng cách một cách rõ ràng. Mỗi cạnh độc lập đóng góp vào tổng số và kích thước cây con xác định đầy đủ có bao nhiêu đường dẫn sử dụng cạnh đó. 

### Tại sao nó hoạt động 

Mỗi cặp đỉnh có một đường đi đơn duy nhất trên cây. Đường dẫn đó có thể được phân tách thành các cạnh và mỗi cạnh được tính chính xác một lần cho mỗi cặp sử dụng nó. Khi một cạnh bị loại bỏ, cây sẽ tách thành hai tập hợp đỉnh rời nhau. Một cặp đóng góp vào tổng khoảng cách khi và chỉ khi điểm cuối của nó nằm trong các thành phần khác nhau của phần tách đó. Tính toán DFS đảm bảo chúng ta biết kích thước của một thành phần cho mỗi cạnh, đủ để đếm tất cả các cặp như vậy chính xác một lần trên mỗi cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n = int(input().strip())
    if n == 1:
        print(0)
        return

    p = list(map(int, input().split()))

    g = [[] for _ in range(n + 1)]
    for i in range(2, n + 1):
        par = p[i - 2]
        g[i].append(par)
        g[par].append(i)

    size = [0] * (n + 1)

    def dfs(v, parent):
        size[v] = 1
        for to in g[v]:
            if to == parent:
                continue
            dfs(to, v)
            size[v] += size[to]

    dfs(1, -1)

    ans = 0
    for i in range(2, n + 1):
        s = size[i]
        ans += s * (n - s)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng danh sách kề từ biểu diễn gốc. Điều này rất cần thiết vì tính toán cây con yêu cầu truyền tải hai chiều, mặc dù đầu vào mã hóa mối quan hệ cha có hướng. 

DFS tính toán kích thước cây con trong một lần chuyển bắt đầu từ nút 1. Mỗi nút khởi tạo kích thước của nó thành 1 và tích lũy kích thước từ các nút con của nó. Tham số gốc ngăn việc xem lại nút trước đó. 

Vòng tích lũy cuối cùng sử dụng thực tế là mỗi cạnh (i, p_i) được liên kết duy nhất với nút i là phía con, do đó size[i] chính xác là một thành phần của phần tách cạnh đó. 

Một cạm bẫy triển khai phổ biến là sử dụng đệ quy mà không tăng giới hạn đệ quy, điều này sẽ thất bại đối với cây dạng chuỗi. Một điều tinh tế khác là đảm bảo mảng cha được lập chỉ mục chính xác, vì nó bắt đầu từ p2. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản gồm bốn nút: 1 được kết nối với 2, 2 đến 3 và 3 đến 4. 

Đối với đầu vào này, mảng cha là`1 2 3`. Kích thước cây con được tính từ nút 1 được hiển thị bên dưới. 

| Nút | Phụ huynh | Kích thước cây con | 
| --- | --- | --- | 
| 1 | - | 4 | 
| 2 | 1 | 3 | 
| 3 | 2 | 2 | 
| 4 | 3 | 1 | 

Mỗi đóng góp của cạnh được tính bằng size[i] nhân n trừ size[i]. 

Với i=2, khoản đóng góp là 3 × 1 = 3. Đối với i=3, khoản đóng góp là 2 × 2 = 4. Với i=4, khoản đóng góp là 1 × 3 = 3. Tổng cộng là 10. 

Điều này phù hợp với việc liệt kê trực tiếp tất cả các khoảng cách theo cặp trong một chuỗi, trong đó khoảng cách tích lũy tự nhiên qua các cạnh trung gian. 

Bây giờ hãy xem xét một ngôi sao có tâm ở nút 1 với các nút 2, 3, 4 đều được kết nối trực tiếp với 1. 

| Nút | Kích thước cây con | 
| --- | --- | 
| 1 | 4 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Mỗi lá đóng góp 1 × 3 = 3, vì vậy tổng câu trả lời là 9. Điều này tương ứng với mọi cặp lá có khoảng cách 2 và mọi cặp từ lá đến tâm có khoảng cách 1, khớp với tổng được tính toán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được xử lý một số lần không đổi trong DFS và tích lũy cuối cùng | 
| Không gian | O(n) | Danh sách kề và mảng cây con lưu trữ các cấu trúc có kích thước tuyến tính | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho n lên tới 300000. Việc sử dụng bộ nhớ cũng tuyến tính và bị chi phối bởi danh sách kề, có thể chấp nhận được trong giới hạn 512 MiB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

assert run("1\n") == "0", "single node"

assert run("4\n1 2 3\n") == "10", "chain of 4 nodes"

assert run("4\n1 1 1\n") == "9", "star centered at 1"

assert run("5\n1 2 3 4\n") == "20", "long chain check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | cây có kích thước tối thiểu | 
| chuỗi 4 | 10 | tính đúng đắn của sự tích lũy đường dẫn | 
| cây sao | 9 | tính đúng đắn của việc tách thành phần | 
| chuỗi 5 | 20 | tính nhất quán của cấu trúc tuyến tính | 

## Vỏ cạnh 

Với n bằng 1, thuật toán ngay lập tức trả về 0 trước khi xây dựng bất kỳ cấu trúc nào. Điều này phù hợp với thực tế là không có cặp đỉnh phân biệt. 

Trong chuỗi tuyến tính, độ sâu đệ quy đạt tới n. DFS được viết đệ quy, do đó việc tăng giới hạn đệ quy là cần thiết để tránh tràn ngăn xếp. Trên đầu vào`1 2 3 ... n-1`, kích thước của cây con giảm dần từ gốc đến lá và mỗi phần đóng góp của cạnh vẫn được ghi lại chính xác dưới dạng size[i] lần n trừ size[i]. 

Trong cây hình ngôi sao, mọi nút ngoại trừ nút trung tâm đều có kích thước cây con là 1. Mỗi cạnh như vậy đóng góp n trừ 1 và tính tổng tất cả các lá sẽ tạo ra tổng số chính xác mà không cần đếm kép, vì mỗi cạnh được xác định duy nhất bởi nút con của nó trong biểu diễn gốc.
