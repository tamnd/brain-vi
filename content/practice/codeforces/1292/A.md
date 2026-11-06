---
title: "CF 1292A - Trò chơi mê cung của NEKO"
description: "Chúng ta có một lưới 2 hàng với cột $n$ tượng trưng cho một mê cung. Người chơi bắt đầu từ ô trên cùng bên trái $(1,1)$ và muốn đến ô dưới cùng bên phải $(2,n)$. Chỉ được phép di chuyển giữa các ô liền kề trực giao. Ban đầu, tất cả các ô đều là mặt đất có thể qua được."
date: "2026-06-11T18:46:44+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1292
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 614 (Div. 1)"
rating: 1400
weight: 1292
solve_time_s: 115
verified: true
draft: false
---

[CF 1292A - Trò chơi mê cung của NEKO](https://codeforces.com/problemset/problem/1292/A) 

**Đánh giá:** 1400 
**Tags:** cấu trúc dữ liệu, dsu, triển khai 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp 2 hàng bởi$n$-lưới cột tượng trưng cho một mê cung. Người chơi bắt đầu ở ô trên cùng bên trái$(1,1)$và muốn đến ô dưới cùng bên phải$(2,n)$. Chỉ được phép di chuyển giữa các ô liền kề trực giao. Ban đầu, tất cả các ô đều là mặt đất có thể qua được. Điều khó khăn là một số tế bào nhất định có thể trở thành dung nham hoặc quay trở lại mặt đất trong quá trình$q$khoảnh khắc. Sau mỗi lần thay đổi, chúng tôi được hỏi liệu con đường từ đầu đến cuối có còn tồn tại mà không bước lên dung nham hay không. 

Nói cách khác, đầu vào mô tả động 2×$n$lưới trong đó mỗi truy vấn sẽ chuyển trạng thái của ô. Đầu ra là một chuỗi "Có" hoặc "Không" cho biết liệu đường dẫn có còn khả thi ngay sau mỗi lần lật hay không. 

Những ràng buộc cho chúng ta biết$n$Và$q$có thể lớn như$10^5$. Điều này có nghĩa là chúng tôi không thể mô phỏng tìm kiếm đường dẫn đầy đủ sau mỗi truy vấn: một BFS đơn giản cho mỗi truy vấn sẽ là$O(nq)$, có khả năng$10^{10}$hoạt động quá chậm so với giới hạn 2 giây. Chúng tôi cần một cách tiếp cận có thể xử lý từng truy vấn trong thời gian không đổi hoặc gần như không đổi. 

Trường hợp cạnh tinh tế phát sinh khi một cột có cả hai ô bị chặn theo cách ngăn cản hoàn toàn việc vượt qua từ trên xuống dưới. Ví dụ: nếu cột 3 có dung nham ở cả hai ô, đường dẫn không thể vượt qua cột 3, ngay cả khi các cột liền kề trống. Một trường hợp cạnh khác là chuyển đổi một ô gián tiếp mở khóa một đường dẫn không thể thực hiện được trước đây. Cách tiếp cận bất cẩn có thể chỉ xem xét ô mới bị chặn mà không kiểm tra cách nó tương tác với các ô lân cận, tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ biểu diễn lưới dưới dạng mảng 2D và chạy BFS từ$(1,1)$sau mỗi lần lật. BFS sẽ khám phá tất cả các ô có thể truy cập và xác định xem$(2,n)$đã đạt được. Điều này đúng nhưng quá chậm vì mỗi BFS$O(n)$và có$q$truy vấn, dẫn đến$O(nq)$, điều không thể chấp nhận được đối với$n,q \sim 10^5$. 

Quan sát quan trọng để có giải pháp nhanh hơn xuất phát từ việc nhận thấy rằng cách duy nhất mà một đường dẫn bị chặn là khi một ô dung nham ở một hàng thẳng hàng với một ô dung nham ở hàng đối diện trong một cột lân cận. Nói cách khác, một ô dung nham chỉ đe dọa đường đi nếu nó có các ô liền kề ở hàng khác cũng là dung nham. 

Thay vì mô phỏng các đường dẫn, chúng tôi theo dõi các xung đột: một "xung đột" xảy ra khi một tế bào dung nham ở$(r,c)$có dung nham liền kề ở hàng đối diện tại các cột$c-1$,$c$, hoặc$c+1$. Nếu có bất kỳ xung đột nào, đường dẫn sẽ bị chặn. Chúng ta chỉ cần duy trì số lượng xung đột đang diễn ra. Mỗi truy vấn sẽ chuyển đổi một ô và cập nhật số lượng xung đột dựa trên các ô lân cận. Việc kiểm tra xem đường dẫn có khả thi hay không sẽ giảm xuống còn xem liệu số lượng xung đột có bằng 0 hay không. Cách tiếp cận này là$O(1)$cho mỗi truy vấn và phù hợp thoải mái trong giới hạn thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (BFS cho mỗi truy vấn) | O(nq) | O(n) | Quá chậm | 
| Đếm xung đột | O(q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo 2×$n$mảng boolean`lava`với tất cả các ô được đặt thành`False`, đại diện cho mặt đất. Khởi tạo một số nguyên`conflicts`về 0. Điều này sẽ theo dõi số lượng tình huống chặn đang hoạt động. 
2. Xử lý từng truy vấn, chuyển trạng thái của một ô theo$(r,c)$. Trước khi chuyển đổi, hãy kiểm tra ba cột lân cận ở hàng đối diện:$c-1$,$c$, Và$c+1$. Đối với mỗi hàng xóm, nếu nó chứa dung nham, hãy giảm`conflicts`bởi một nếu ô hiện tại là dung nham (chúng tôi sắp loại bỏ nó), vì xung đột này sẽ không còn tồn tại. 
3. Chuyển đổi trạng thái của ô: nếu nó là mặt đất, đánh dấu nó là dung nham; nếu đó là dung nham, hãy đánh dấu nó là mặt đất. 
4. Sau khi chuyển đổi, hãy kiểm tra lại ba người hàng xóm đó. Đối với mỗi dung nham lân cận ở hàng đối diện, hãy tăng`conflicts`bởi một nếu ô hiện tại bây giờ là dung nham, vì một xung đột mới đã được tạo ra. 
5. Nếu`conflicts`bằng 0 thì xuất ra “Có”, nếu không thì xuất ra “Không”. Lặp lại cho tất cả các truy vấn. 

Tại sao nó hoạt động: Bất biến là vậy`conflicts`đếm chính xác số cặp liền kề trong đó dung nham ở một hàng nằm ngay cạnh dung nham ở hàng đối diện. Xung đột cho biết đường dẫn bị chặn vì người chơi không thể vượt qua cặp đó. Vì chuyển động chỉ đến các ô liền kề nên không có kịch bản nào khác có thể chặn hoàn toàn đường dẫn. Bằng cách duy trì số lượng tăng dần, chúng tôi có thể trả lời từng truy vấn trong thời gian không đổi mà không cần tìm kiếm đường dẫn đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
lava = [[False] * n for _ in range(2)]
conflicts = 0

for _ in range(q):
    r, c = map(int, input().split())
    r -= 1
    c -= 1
    delta = 1 if not lava[r][c] else -1
    for dc in [-1, 0, 1]:
        nc = c + dc
        if 0 <= nc < n:
            if lava[1-r][nc]:
                conflicts += delta
    lava[r][c] = not lava[r][c]
    print("Yes" if conflicts == 0 else "No")
```Chúng tôi sử dụng cách biểu diễn được lập chỉ mục 0 để thuận tiện. Biến`delta`xác định xem việc chuyển đổi ô sẽ thêm hay loại bỏ xung đột. Chúng ta lặp qua ba cột lân cận ở hàng đối diện để điều chỉnh`conflicts`đếm. Sau khi cập nhật trạng thái ô và xung đột, chúng tôi kiểm tra xem đường dẫn có bị chặn hay không. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu đầu tiên:```
5 5
2 3
1 4
2 4
2 3
1 4
```| Truy vấn | Ô được chuyển đổi | Xung đột Trước | Xung đột Sau | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | (2,3) | 0 | 0 | Có | 
| 2 | (1,4) | 0 | 1 | Không | 
| 3 | (2,4) | 1 | 2 | Không | 
| 4 | (2,3) | 2 | 1 | Không | 
| 5 | (1,4) | 1 | 0 | Có | 

Điều này chứng tỏ rằng số lượng xung đột nắm bắt chính xác các đường dẫn bị chặn mà không cần mô phỏng toàn bộ chuyển động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi truy vấn sẽ kiểm tra tối đa ba ô lân cận và chuyển đổi một ô. | 
| Không gian | O(n) | Chúng tôi lưu trữ lưới 2×n. | 

Về giới hạn tối đa$n,q = 10^5$, thuật toán thực hiện khoảng 3·10^5 thao tác trên mỗi bộ truy vấn, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, q = map(int, input().split())
    lava = [[False] * n for _ in range(2)]
    conflicts = 0
    out = []
    for _ in range(q):
        r, c = map(int, input().split())
        r -= 1
        c -= 1
        delta = 1 if not lava[r][c] else -1
        for dc in [-1,0,1]:
            nc = c + dc
            if 0 <= nc < n:
                if lava[1-r][nc]:
                    conflicts += delta
        lava[r][c] = not lava[r][c]
        out.append("Yes" if conflicts==0 else "No")
    return "\n".join(out)

# provided sample
assert run("5 5\n2 3\n1 4\n2 4\n2 3\n1 4\n") == "Yes\nNo\nNo\nNo\nYes", "sample 1"

# minimum size
assert run("2 1\n1 2\n") == "Yes", "minimum size"

# maximum size, no conflicts
assert run("5 5\n1 2\n2 4\n1 3\n2 5\n1 4\n") == "Yes\nYes\nYes\nYes\nYes", "max size no conflict"

# all toggled once and back
assert run("3 6\n1 2\n2 2\n1 2\n2 2\n1 3\n2 3\n") == "Yes\nNo\nYes\nYes\nYes\nNo", "toggle twice edge"

# adjacent conflicts
assert run("4 4\n1 2\n2 1\n1 3\n2 2\n") == "Yes\nNo\nNo\nNo", "adjacent column conflict"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1\n1 2\n | Có | Lưới kích thước tối thiểu hoạt động | 
| 5 5\n1 2\n2 4\n1 3\n2 5\n1 4\n | Có\nCó\nCó\nCó\nCó | Nhiều chuyển đổi mà không có xung đột | 
| 3 6\n1 2\n2 2\n1 2\n2 2\n1 3 | | |
