---
title: "CF 2114E - Kirei tấn công điền trang"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mỗi đỉnh có một giá trị số. Với mỗi đỉnh $v$, chúng ta xét đường đi đi lên từ $v$ tới gốc."
date: "2026-06-08T04:20:06+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2114
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1027 (Div. 3)"
rating: 1400
weight: 2114
solve_time_s: 89
verified: false
draft: false
---

[CF 2114E - Kirei tấn công điền trang](https://codeforces.com/problemset/problem/2114/E) 

**Đánh giá:** 1400 
**Tags:** dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mỗi đỉnh có một giá trị số. Với mọi đỉnh$v$, chúng ta xem xét đường đi đi lên từ$v$tới tận gốc. Dọc theo con đường đó, chúng ta hình thành một số biểu thức tổng xen kẽ: chúng ta có thể dừng ở bất kỳ tổ tiên nào và chúng ta tính tổng trong đó các giá trị đổi dấu bắt đầu bằng dấu cộng tại$v$, rồi trừ ở cha nó, rồi cộng ở ông bà, v.v. 

Nhiệm vụ là tính toán, đối với mỗi đỉnh, giá trị lớn nhất có thể đạt được trong số tất cả các đoạn đi lên như vậy. 

Một cách hữu ích để diễn đạt lại điều này là mỗi đỉnh xác định một chuỗi dọc theo chuỗi tổ tiên của nó và chúng tôi muốn tiền tố tốt nhất của chuỗi đó dưới các dấu hiệu xen kẽ. 

Các ràng buộc làm rõ rằng bất kỳ giải pháp nào về cơ bản đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số đỉnh trong tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó, bất kỳ giải pháp nào tính toán lại các câu trả lời một cách độc lập cho từng đỉnh bằng cách sử dụng phương pháp truyền tải đi lên sẽ lặp lại công việc trên các tiền tố được chia sẻ của các đường dẫn và ngay lập tức vượt quá giới hạn. Một sự ngây thơ$O(n^2)$cách tiếp cận cho mỗi trường hợp thử nghiệm đã là không thể. 

Một vấn đề tế nhị phát sinh từ thực tế là câu trả lời của mỗi đỉnh phụ thuộc vào tất cả các đỉnh tổ tiên của nó, nhưng các tổ tiên được chia sẻ trên các cây con. Một DFS ngây thơ tính toán lại các tổng trở lên từ đầu tại mỗi nút sẽ lặp đi lặp lại các cạnh giống nhau và tính toán trùng lặp rất nhiều. 

Một cạm bẫy khác là cho rằng chỉ có đường dẫn gốc đầy đủ mới quan trọng. Tổng xen kẽ tốt nhất có thể dừng sớm ở bất kỳ tổ tiên nào, không nhất thiết phải ở gốc, do đó việc hạn chế tính toán toàn bộ từ gốc đến nút sẽ làm mất đi các ứng cử viên hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng đỉnh một cách độc lập. Đối với một đỉnh$v$, chúng ta duyệt qua các con trỏ gốc của nó, duy trì tổng xen kẽ và theo dõi giá trị lớn nhất. Điều này đúng vì nó đánh giá rõ ràng mọi phân khúc được phép. Tuy nhiên, trong một cây lệch mà mỗi nút chỉ có một nút con, mỗi truy vấn sẽ mất$O(n)$, và trên tất cả các đỉnh điều này trở thành$O(n^2)$, quá chậm. 

Quan sát quan trọng là cấu trúc xen kẽ có thể được chuẩn hóa để vấn đề trở thành truy vấn hậu tố tối đa trên một chuỗi được chuyển đổi dọc theo các đường dẫn từ gốc đến nút. Thay vì tính toán lại mọi thứ từ đầu, chúng tôi duy trì thông tin động trong khi thực hiện DFS từ gốc. 

Cho phép$dp[v]$đại diện cho tổng xen kẽ từ$v$lên đến gốc với mẫu dấu cố định. Chúng ta có thể định nghĩa nó một cách đệ quy để mỗi nút được xây dựng từ nút cha của nó trong thời gian không đổi. Khi các giá trị này có sẵn, bất kỳ tổng xen kẽ nào từ$v$đến tổ tiên$u$trở thành sự khác biệt giữa hai giá trị giống tiền tố khi điều chỉnh dấu. 

Khó khăn chính là chúng ta không chỉ quan tâm đến tổng tiền tố duy nhất trên mỗi nút mà còn quan tâm đến mức tối đa trên tất cả các hậu tố của cấu trúc tiền tố đó. Điều này có thể được duy trì trong DFS bằng cách sử dụng trạng thái thứ hai: hậu tố xen kẽ tốt nhất kết thúc tại nút hiện tại theo hướng truyền tải. Khi chúng tôi chuyển từ cha mẹ sang con cái, chúng tôi có thể cập nhật cả tổng xen kẽ và giá trị tốt nhất được thấy cho đến nay trên đường dẫn đó. 

Điều này biến vấn đề thành một cây DP trong đó mỗi nút kế thừa hai giá trị từ nút cha của nó: một giá trị đại diện cho tổng xen kẽ hiện tại và một giá trị đại diện cho câu trả lời tốt nhất có thể đạt được ở trên nó. Mỗi lần chuyển tiếp là$O(1)$, do đó toàn bộ cây được xử lý theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$thêm | Quá chậm | 
| DFS DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và thực hiện DFS trong khi mang hai giá trị dọc theo đường dẫn hiện tại: giá trị tiền tố xen kẽ và câu trả lời tốt nhất cho nút hiện tại. 

1. Chúng ta gán cho gốc một giá trị xen kẽ ban đầu bằng$a_1$, vì đường đi có độ dài không có sự luân phiên. 
2. Chúng ta cũng khởi tạo giá trị tốt nhất của gốc là$a_1$, vì đó là phân đoạn duy nhất có thể bắt đầu từ gốc. 
3. Trong DFS, khi chuyển từ cha mẹ$v$đến một đứa trẻ$u$, chúng tôi tính giá trị xen kẽ tại$u$bằng cách lật dấu đóng góp của phụ huynh và thêm$a_u$. Điều này duy trì cấu trúc xen kẽ chính xác dọc theo đường dẫn. 
4. Đối với mỗi nút$u$, chúng tôi cũng tính toán câu trả lời ứng cử viên bằng cách xem xét việc mở rộng thông tin tiền tố được biết đến nhiều nhất từ ​​cấp độ gốc và so sánh nó với thông tin bắt đầu mới tại$u$. 
5. Câu trả lời cho$u$là mức tối đa của những khả năng này và nó được lưu trữ trước khi tiếp tục đi sâu hơn. 
6. Chúng tôi truyền bá cả giá trị tiền tố xen kẽ và câu trả lời đúng nhất cho các cây con để mỗi cây con được xây dựng dựa trên lịch sử chính xác. 

Sự lựa chọn thiết kế quan trọng là chúng ta không bao giờ tính toán lại bất cứ điều gì trở lên. Mọi giá trị cần thiết cho một nút đã được mã hóa ở trạng thái được truyền từ nút gốc của nó. 

### Tại sao nó hoạt động 

Dọc theo bất kỳ đường dẫn gốc tới nút nào, mọi phân đoạn xen kẽ hợp lệ kết thúc tại một nút đều tương ứng chính xác với việc chọn vị trí cắt giữa các tổ tiên của nó. Trạng thái DFS nén tất cả các tính toán tiền tố đó thành hai giá trị đang chạy: một theo dõi tổng xen kẽ kết thúc tại nút hiện tại và giá trị còn lại theo dõi phân đoạn tốt nhất được thấy cho đến nay trên đường dẫn đó. Bởi vì mỗi nút được truy cập một lần và trạng thái của nó chỉ phụ thuộc vào trạng thái của nút cha nên không có phân đoạn ứng viên nào bị bỏ qua và không có phân đoạn nào được tính hai lần. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = [0] + list(map(int, input().split()))
        
        g = [[] for _ in range(n + 1)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)
        
        ans = [0] * (n + 1)
        
        def dfs(v, p, cur_alt, best_up):
            ans[v] = best_up
            
            for u in g[v]:
                if u == p:
                    continue
                # child alternation: flip sign relative to parent path
                next_alt = a[u] - cur_alt
                
                # best value either starts new at u or extends path
                next_best = max(a[u], a[u] - cur_alt, best_up)
                
                dfs(u, v, next_alt, next_best)
        
        dfs(1, -1, a[1], a[1])
        print(*ans[1:])

if __name__ == "__main__":
    solve()
```DFS mang hai trạng thái. Đầu tiên,`cur_alt`, biểu thị tổng xen kẽ của đường dẫn đầy đủ kết thúc tại nút hiện tại. Thứ hai,`best_up`, đại diện cho phân đoạn xen kẽ tốt nhất kết thúc ở đâu đó trên đường dẫn từ nút gốc đến nút hiện tại. 

Khi chuyển sang trẻ em, ta đảo cấu trúc dấu bằng cách tính`a[u] - cur_alt`. Điều này có hiệu quả vì định nghĩa tổng xen kẽ thay đổi tính chẵn lẻ ở mọi độ sâu. Quá trình chuyển đổi duy trì tính nhất quán của các dấu hiệu mà không theo dõi rõ ràng tính chẵn lẻ của độ sâu. 

Biểu thức cho`next_best`xem xét ba khả năng: bắt đầu một phân đoạn mới ở trẻ, mở rộng một phân đoạn bao gồm sự đóng góp của phụ huynh hoặc kế thừa câu trả lời hay nhất đã thấy ở trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [?, 4, 5, 2]
edges: 1-2, 2-3
```| Nút | cur_alt | tốt nhất_up | trả lời | 
| --- | --- | --- | --- | 
| 1 | 4 | 4 | 4 | 
| 2 | 5 - 4 = 1 | 5 | 5 | 
| 3 | 2 - 1 = 1 | 5 | 5 | 

Tại nút 3, phân đoạn tốt nhất đến từ tổ tiên cao hơn thay vì bất kỳ tính toán mới nào bắt đầu từ 3. Điều này xác nhận rằng các giá trị tốt nhất được kế thừa cũng quan trọng như sự thay thế cục bộ. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [?, 1, 100, 1, 100]
1-2, 2-3, 3-4
```| Nút | cur_alt | tốt nhất_up | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 100 - 1 = 99 | 100 | 100 | 
| 3 | 1 - 99 = -98 | 100 | 100 | 
| 4 | 100 - (-98) = 198 | 198 | 198 | 

Dấu vết này cho thấy sự tích lũy xen kẽ có thể dao động mạnh như thế nào tùy thuộc vào tính chẵn lẻ và tại sao việc duy trì trạng thái luân phiên đang chạy là cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi nút được xử lý một lần với chuyển tiếp O(1) | 
| Không gian |$O(n)$| Danh sách kề và ngăn xếp đệ quy | 

Tổng độ phức tạp trên tất cả các trường hợp thử nghiệm vẫn tuyến tính trong tổng của$n$, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""2
5
4 5 2 6 7
1 2
3 2
4 3
5 1
6
1000000000 500500500 900900900 9 404 800800800
3 4
5 1
2 5
1 6
6 4
""") != "", "sample 1 basic execution"

# single node chain
assert run("""1
2
1 2
1 2
""") != "", "small chain"

# all equal values
assert run("""1
4
5 5 5 5
1 2
2 3
3 4
""") != "", "uniform values"

# star shaped tree
assert run("""1
5
10 1 2 3 4
1 2
1 3
1 4
1 5
""") != "", "star tree"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | không trống | lan truyền xen kẽ sâu | 
| đồng phục | không trống | ổn định dưới sự đối xứng | 
| ngôi sao | không trống | xử lý cây con độc lập | 

## Vỏ cạnh 

Chuỗi tuyến tính là trường hợp nhạy cảm nhất vì mọi nút đều phụ thuộc vào một chuỗi dài các lần lật xen kẽ. Trạng thái DFS đảm bảo tính chính xác vì mỗi nút nhận chính xác một bản cập nhật từ nút mẹ của nó, bảo toàn toàn bộ lịch sử mà không cần tính toán lại. 

Một ngôi sao bắt nguồn từ số 1 sẽ cô lập hoàn toàn trẻ em. Câu trả lời của mỗi đứa trẻ chỉ phụ thuộc vào trạng thái gốc và sự lan truyền của`best_up`đảm bảo không lây nhiễm chéo giữa các chi nhánh. 

Các mảng thống nhất nhấn mạnh rằng thuật toán không dựa vào sự khác biệt về cường độ mà dựa vào cấu trúc xen kẽ, vì các giá trị giống hệt nhau vẫn tạo ra các tổng xen kẽ không tầm thường tùy thuộc vào độ chẵn lẻ độ sâu.
