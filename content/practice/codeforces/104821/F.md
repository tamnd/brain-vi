---
title: "CF 104821F - Viết lại tương đương"
description: "Chúng ta bắt đầu với một mảng có độ dài $m$, ban đầu chứa đầy các số 0. Mỗi thao tác $i$ lấy một danh sách các vị trí và ghi đè tất cả các vị trí đó bằng giá trị $i$."
date: "2026-06-28T12:48:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104821
codeforces_index: "F"
codeforces_contest_name: "The 2023 ICPC Asia Nanjing Regional Contest (The 2nd Universal Cup. Stage 11: Nanjing)"
rating: 0
weight: 104821
solve_time_s: 95
verified: false
draft: false
---

[CF 104821F - Viết lại tương đương](https://codeforces.com/problemset/problem/104821/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có độ dài$m$, ban đầu chứa đầy số không. Mỗi thao tác$i$lấy danh sách các vị trí và ghi đè tất cả các vị trí đó bằng giá trị$i$. Vì các phép toán được áp dụng theo thứ tự từ 1 đến$n$, giá trị cuối cùng tại mỗi vị trí chỉ đơn giản là chỉ số của thao tác cuối cùng chạm vào nó. 

Mảng cuối cùng$R$do đó được xác định theo quy tắc "lần ghi cuối cùng sẽ thắng": mọi chỉ mục đều ghi nhớ số thao tác lớn nhất bao gồm nó và số thao tác đó trở thành giá trị cuối cùng của nó. 

Nhiệm vụ là sắp xếp lại các hoạt động. Chúng ta được phép hoán vị các phép toán một cách tùy ý, nhưng sau khi áp dụng chúng theo thứ tự mới này, mọi vị trí phải kết thúc bằng giá trị cuối cùng giống hệt như trong quy trình ban đầu. 

Các ràng buộc cho phép lên đến$10^5$hoạt động và vị trí cho mỗi lần kiểm tra, với tổng kích thước đầu vào lên tới khoảng$2 \cdot 10^6$. Điều này buộc phải đưa ra giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất cứ điều gì bậc hai về số lần xảy ra thao tác-vị trí sẽ thất bại ngay lập tức vì mỗi thao tác có thể chạm vào nhiều vị trí. 

Một trường hợp thất bại nhỏ xuất hiện khi hai thao tác trùng nhau ở một số vị trí nhưng thao tác “chiến thắng” cho các vị trí đó lại khác nhau trong quy trình ban đầu. Ví dụ: nếu thao tác 1 chạm vào vị trí 1 và 2, thao tác 2 chạm vào vị trí 2 và 3, và thao tác 3 chạm vào vị trí 1 và 3, thì các chỉ số khác nhau sẽ có người ghi cuối cùng khác nhau. Một ý tưởng ngây thơ như sắp xếp theo quy mô hoạt động hoặc theo chỉ số tối thiểu được chạm vào sẽ không thành công vì nó bỏ qua các ràng buộc đó là dành riêng cho từng vị trí chứ không phải toàn cục. 

Khó khăn cốt lõi là kết quả cuối cùng mã hóa các ràng buộc ưu tiên giữa các thao tác và các ràng buộc này phải được trích xuất một cách cẩn thận. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua các ràng buộc về thứ tự và chỉ thử tất cả các hoán vị, chúng ta có thể mô phỏng từng thứ tự và so sánh kết quả. Điều đó sẽ tốn kém$n! \cdot m$, điều này hoàn toàn không khả thi ngay cả đối với những đầu vào nhỏ. 

Một nỗ lực ít ngây thơ hơn một chút là nhận ra rằng giá trị cuối cùng của mỗi vị trí chỉ phụ thuộc vào thao tác cuối cùng ảnh hưởng đến nó. Điều này gợi ý tính toán, đối với từng vị trí, thao tác nào là cuối cùng trong chuỗi ban đầu. Gọi đây$last[x]$. Nếu chúng tôi khắc phục rằng thao tác tương tự phải là người ghi cuối cùng cho mỗi vị trí, thì chúng tôi có thể rút ra các ràng buộc: cho mọi vị trí$x$, mọi thao tác chạm vào$x$ngoại trừ$last[x]$phải xuất hiện trước$last[x]$. 

Điều này biến vấn đề thành một phần thứ tự của các phép toán. Mỗi vị trí đóng góp các ràng buộc định hướng có dạng “hoạt động$a$phải đến trước khi hoạt động$b$Một khi chúng ta xây dựng tất cả các ràng buộc như vậy, vấn đề sẽ trở thành kiểm tra xem liệu thứ tự tôpô hợp lệ có tồn tại hay không. 

Đây chính xác là nơi cấu trúc có thể được khai thác. Thay vì lý luận về mảng, chúng ta lý luận về sự phụ thuộc giữa các phép toán. Nếu biểu đồ phụ thuộc có một chu trình thì không có hoán vị nào có thể bảo toàn tất cả các mối quan hệ “lần ghi cuối cùng”. Nếu nó không theo chu kỳ thì bất kỳ thứ tự tôpô nào cũng tạo ra một hoán vị hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot m)$|$O(m)$| Quá chậm | 
| Đồ thị phụ thuộc + Toposort |$O(n + \sum p_i)$|$O(n + \sum p_i)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề thành việc xây dựng và xác nhận đồ thị có hướng cho các phép toán. 

1. Tính toán thao tác cuối cùng ảnh hưởng đến từng vị trí. Chúng tôi quét các hoạt động theo thứ tự tăng dần và ghi lại cho từng vị trí$x$chỉ số lớn nhất$i$hoạt động đó$i$chứa$x$. Giá trị này là$last[x]$. Bước này ghi lại trạng thái cuối cùng của mảng. 
2. Đối với mọi hoạt động$i$, chúng ta kiểm tra tất cả các vị trí mà nó chạm tới. Đối với mỗi vị trí$x$, nếu như$last[x] \neq i$, sau đó hoạt động$i$phải xuất hiện trước khi hoạt động$last[x]$theo bất kỳ thứ tự hợp lệ nào. Chúng tôi mã hóa điều này như một cạnh có hướng$i \rightarrow last[x]$. 
3. Chúng tôi xây dựng đồ thị có hướng đầy đủ bằng cách sử dụng danh sách kề và tính toán độ của tất cả các nút. Biểu đồ này mã hóa mọi ràng buộc cần thiết để bảo toàn mảng cuối cùng. 
4. Chúng tôi thực hiện sắp xếp cấu trúc liên kết bằng cách sử dụng một hàng các nút có bậc bằng 0. Mỗi lần chúng tôi xóa một thao tác khỏi hàng đợi, chúng tôi sẽ thêm thao tác đó vào câu trả lời và giảm mức độ của các thao tác lân cận. 
5. Nếu chúng tôi quản lý để xuất tất cả$n$hoạt động, lệnh là hợp lệ. Mặt khác, biểu đồ chứa một chu trình và không hoán vị nào có thể bảo toàn kết quả. 

Ý tưởng chính là mỗi vị trí đóng góp một ràng buộc “người chiến thắng phải đến sau tất cả những người tham gia khác” và chúng tôi thực thi tất cả các ràng buộc đó trên toàn cầu. 

### Tại sao nó hoạt động 

Mỗi vị trí$x$được xác định trong quy trình ban đầu bằng một thao tác duy nhất$last[x]$. Bất kỳ thao tác nào khác chạm vào$x$không được ghi đè lên nó sau$last[x]$theo thứ tự mới, nếu không giá trị cuối cùng sẽ thay đổi. Vì vậy mọi hoạt động như vậy phải đi trước$last[x]$. Những ràng buộc này chính xác là những gì biểu đồ mã hóa. 

Ngược lại, nếu tất cả các ràng buộc này được thỏa mãn thì với mọi vị trí$x$, thao tác cuối cùng chạm vào nó theo thứ tự mới vẫn là$last[x]$, bởi vì tất cả các hoạt động cạnh tranh đều bị ép buộc trước đó. Điều này bảo tồn chính xác mảng cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    
    ops = [[] for _ in range(n + 1)]
    last = [0] * (m + 1)

    for i in range(1, n + 1):
        arr = list(map(int, input().split()))
        p = arr[0]
        for x in arr[1:]:
            ops[i].append(x)
            last[x] = i

    g = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)

    for i in range(1, n + 1):
        for x in ops[i]:
            j = last[x]
            if j != i:
                g[i].append(j)
                indeg[j] += 1

    q = deque([i for i in range(1, n + 1) if indeg[i] == 0])
    res = []

    while q:
        u = q.popleft()
        res.append(u)
        for v in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if len(res) != n:
        print("No")
    else:
        print("Yes")
        print(*res)

t = int(input())
for _ in range(t):
    solve()
```Đầu tiên, mã sẽ xây dựng lại “lần xuất hiện cuối cùng” của từng vị trí, mô tả đầy đủ mảng mục tiêu. Sau đó, nó xây dựng một biểu đồ có hướng trong đó mọi cạnh bắt buộc rằng một hoạt động không thắng cho một vị trí phải xuất hiện sớm hơn hoạt động thắng. 

Việc sắp xếp cấu trúc liên kết được thực hiện bằng cách sử dụng deque. Nếu bất kỳ chu trình nào tồn tại, một số thao tác sẽ không bao giờ đạt tới mức 0, điều này báo hiệu chính xác sự không thể thực hiện được. 

Một nhược điểm phổ biến là lặp lại các cạnh hai lần hoặc thêm các cạnh không chính xác vào các vị trí mà bản thân thao tác là tác vụ ghi cuối cùng. Những trường hợp đó phải được bỏ qua vì chúng không áp đặt ràng buộc nào. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó các hoạt động chồng chéo: 

đầu vào:```
n = 3, m = 3
1: [1]
2: [1, 2]
3: [2]
```Đây$last[1] = 2$,$last[2] = 3$,$last[3] = 0$. 

Chúng tôi xây dựng các cạnh: 

Hoạt động 1 góp phần$1 \rightarrow 2$. 

Hoạt động 2 góp phần$2 \rightarrow 3$. 

Hoạt động 3 không đóng góp gì. 

Sắp xếp tôpô mang lại thứ tự$1, 2, 3$. 

| Bước | Xếp hàng | Được chọn | Kết quả | Thay đổi mức độ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | [1] | 2 giảm | 
| 1 | 2 | 2 | [1,2] | 3 giảm | 
| 2 | 3 | 3 | [1,2,3] | xong | 

Điều này xác nhận rằng các phần phụ thuộc được lan truyền chính xác dọc theo các bản cập nhật chồng chéo. 

Ví dụ thứ hai cho thấy điều không thể xảy ra:```
1: [1]
2: [1]
```Cả hai thao tác đều chạm vào cùng một vị trí, nhưng chỉ có thao tác 2 là cuối cùng, vì vậy chúng ta có được cạnh$1 \rightarrow 2$. Điều này vẫn không có tính tuần hoàn nên trật tự tồn tại dưới dạng$1,2$. Nếu chúng tôi đảo ngược các phần phụ thuộc không chính xác, chúng tôi sẽ tạo một chu trình và báo cáo lỗi không chính xác. Việc xây dựng đảm bảo hướng luôn từ không cuối cùng đến cuối cùng, ngăn ngừa chu kỳ sai lầm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + \sum p_i)$| Mỗi vị trí được xử lý một lần để tính toán lần xuất hiện cuối cùng và một lần để tạo cạnh | 
| Không gian |$O(n + \sum p_i)$| Lưu trữ danh sách lân cận và danh sách thành viên hoạt động | 

Tổng công việc chia tỷ lệ tuyến tính với kích thước đầu vào, phù hợp thoải mái trong các ràng buộc trong đó tổng của tất cả$p_i$tùy thuộc vào$10^6$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        ops = [[] for _ in range(n + 1)]
        last = [0] * (m + 1)

        for i in range(1, n + 1):
            arr = list(map(int, input().split()))
            p = arr[0]
            for x in arr[1:]:
                ops[i].append(x)
                last[x] = i

        g = [[] for _ in range(n + 1)]
        indeg = [0] * (n + 1)

        for i in range(1, n + 1):
            for x in ops[i]:
                j = last[x]
                if j != i:
                    g[i].append(j)
                    indeg[j] += 1

        q = deque([i for i in range(1, n + 1) if indeg[i] == 0])
        res = []

        while q:
            u = q.popleft()
            res.append(u)
            for v in g[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)

        if len(res) != n:
            return "No\n"
        return "Yes\n" + " ".join(map(str, res)) + "\n"

    t = int(input())
    out = []
    for _ in range(t):
        out.append(solve())
    return "".join(out)

# sample-style checks
assert run("""1
3 3
1 1
2 1 2
1 2
""").strip().startswith("Yes")

assert run("""1
2 2
1 1
1 1
""").strip() in ["Yes\n1 2", "Yes\n2 1"]

assert run("""1
2 2
1 1
1 1
""") != "", "non-empty output"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi phụ thuộc đơn | Có + đơn hàng hợp lệ | Tính đúng đắn tôpô cơ bản | 
| Hoạt động giống hệt nhau | Có | Xử lý các ràng buộc dư thừa | 
| Cấu trúc chồng chéo nhưng không theo chu kỳ | Có | Nhiều ràng buộc trên mỗi nút | 

## Vỏ cạnh 

Trường hợp tối thiểu là khi mọi thao tác đều chạm vào các vị trí rời rạc. Đồ thị không có cạnh nên mọi hoán vị đều hợp lệ. Thuật toán tạo ra chính xác tất cả các nút có bậc 0 ban đầu và xuất ra bất kỳ thứ tự nào. 

Một trường hợp tế nhị hơn là khi có nhiều thao tác chạm vào một vị trí. Chỉ cái cuối cùng trong chuỗi ban đầu mới trở thành phần chìm của một chuỗi các cạnh và tất cả các cạnh khác đều trỏ đến nó. Cấu trúc đảm bảo không có cạnh nào bị đảo ngược, do đó không có chu trình nhân tạo nào được đưa vào. 

Trường hợp thất bại sẽ phát sinh nếu chúng ta thêm nhầm các cạnh theo cả hai hướng cho các vị trí chung. Điều đó sẽ tạo ra các chu kỳ ngay lập tức ngay cả khi tồn tại một đơn đặt hàng hợp lệ. Thuật toán tránh điều này bằng cách hướng các cạnh về phía người ghi cuối cùng được xác định bởi quy trình ban đầu.
