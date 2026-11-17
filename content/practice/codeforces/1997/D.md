---
title: "CF 1997D - Tối đa hóa gốc"
description: "Chúng tôi được tặng một cây có rễ. Mỗi đỉnh chứa một số không âm. Một thao tác có thể được áp dụng cho bất kỳ đỉnh không có lá nào. Khi chúng ta chọn một đỉnh $v$, chúng ta tăng giá trị tại $v$ lên một và đồng thời giảm mọi đỉnh khác trong cây con của $v$ đi một."
date: "2026-06-08T14:37:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1997
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 168 (Rated for Div. 2)"
rating: 1500
weight: 1997
solve_time_s: 156
verified: true
draft: false
---

[CF 1997D - Tối đa hóa phần gốc](https://codeforces.com/problemset/problem/1997/D) 

**Đánh giá:** 1500 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, dp, tham lam, cây cối 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây có rễ. Mỗi đỉnh chứa một số không âm. 

Một thao tác có thể được áp dụng cho bất kỳ đỉnh không có lá nào. Khi chúng ta chọn một đỉnh$v$, chúng tôi tăng giá trị tại$v$đi một và đồng thời giảm mọi đỉnh khác trong cây con của$v$bởi một. Tất cả các giá trị phải không âm sau khi thực hiện thao tác. 

Mục tiêu là tối đa hóa giá trị ở gốc sau khi thực hiện bất kỳ số lượng thao tác hợp lệ nào. 

Cây chứa tới$2 \cdot 10^5$đỉnh trên tất cả các trường hợp thử nghiệm. Các giá trị có thể lớn như$10^9$. Một giải pháp mô phỏng trực tiếp các thao tác là không thể vì số lượng thao tác cũng có thể cực kỳ lớn. Ngay cả một đỉnh cũng có thể được tăng lên hàng tỷ lần. 

Ràng buộc về tổng$n$ngay lập tức gợi ý rằng chúng ta cần một cái gì đó gần gũi$O(n)$hoặc$O(n \log n)$mỗi tập tin thử nghiệm. Bất kỳ cách tiếp cận nào liên tục sửa đổi cây con hoặc tìm kiếm các thao tác sẽ rất chậm. 

Phần nguy hiểm nhất của vấn đề là các thao tác tương tác giữa các cấp độ khác nhau của cây. 

Coi như:```
1(0)
|
2(10)
|
3(0)
```Một ý tưởng ngây thơ là đỉnh 2 có thể tặng tất cả 10 đơn vị trở lên. Điều đó là sai. Mọi thao tác trên đỉnh 2 cũng làm giảm đỉnh 3. Vì đỉnh 3 bắt đầu từ 0 nên đỉnh 2 không thể thực hiện dù chỉ một thao tác. 

Câu trả lời đúng là 0 chứ không phải 10. 

Một trường hợp tế nhị khác là:```
1(0)
|
2(5)
|
3(3)
```Đỉnh 2 có thể thực hiện tối đa ba thao tác vì đỉnh 3 là nút thắt cổ chai. Sau đó, đỉnh 2 trở thành 8 và đỉnh 3 trở thành 0. Bây giờ đỉnh 2 không thể hoạt động được nữa. 

Một giải pháp bất cẩn chỉ xem xét tổng bên trong cây con sẽ kết luận sai rằng tất cả tám đơn vị cuối cùng đều có thể đến gốc. 

Trường hợp quan trọng thứ ba là một chiếc lá.```
1(0)
|
2(7)
```Vì đỉnh 2 không có con nên nó không thể thực hiện các phép toán. Gốc có thể nhận tối đa 7 đơn vị từ đỉnh 2 và không thể chuyển đổi bên trong cây con. Lá hoạt động khác với các đỉnh bên trong và phải được xử lý riêng. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản. Liên tục tìm kiếm một thao tác hợp lệ, áp dụng nó, cập nhật tất cả các đỉnh bị ảnh hưởng và tiếp tục cho đến khi không có thao tác nào có thể cải thiện được gốc. 

Điều này đúng vì nó tuân theo định nghĩa chính xác. Thật không may là nó chậm một cách vô vọng. Một thao tác đơn lẻ có thể chạm tới toàn bộ cây con, tốn$O(n)$. Tệ hơn nữa, số lượng thao tác có thể lớn tới mức$10^9$. Ngay cả đối với những cây nhỏ thì điều này cũng không thể thực hiện được. 

Để đạt được thứ gì đó nhanh hơn, chúng ta cần ngừng suy nghĩ về các hoạt động riêng lẻ và thay vào đó hãy đặt câu hỏi về tính khả thi. 

Giả sử chúng ta muốn gốc kết thúc bằng giá trị ít nhất$X$. 

Chúng ta có thể xác định liệu điều này có thể thực hiện được không? 

Nếu chúng ta có thể trả lời câu hỏi đó một cách hiệu quả thì câu trả lời cuối cùng có thể được tìm thấy bằng tìm kiếm nhị phân. 

Quan sát quan trọng là các hoạt động chỉ di chuyển nguồn lực lên trên. Mỗi cây con phải có khả năng cung cấp đủ giá trị cho cây mẹ của nó. Điều này gợi ý một DFS từ dưới lên. 

Hãy xem xét một nút$v$và giả sử cha mẹ của nó yêu cầu một số tiền từ nó. 

Nếu như$v$là một chiếc lá, số lượng tối đa nó có thể cung cấp chỉ đơn giản là$a_v$. 

Đối với một nút nội bộ, mọi thứ trở nên thú vị hơn. 

Cho rằng$v$phải kết thúc bằng ít nhất$need$các đơn vị có sẵn cho cha mẹ của nó. 

Nếu như$a_v \ge need$, sau đó$v$đã đáp ứng được yêu cầu rồi. Bất kỳ giá trị bổ sung nào đều vô hại. 

Nếu như$a_v < need$, sau đó$v$bằng cách nào đó phải tạo ra$need-a_v$các đơn vị bổ sung. Cách duy nhất là thực hiện các thao tác tại$v$. 

Mỗi thao tác tăng$v$bằng 1 nhưng tiêu thụ 1 đơn vị từ mỗi cây con con. Do đó tạo ra$d$đơn vị bổ sung tại$v$yêu cầu mọi cây con con phải cung cấp ít nhất$d$đơn vị. 

Điều này dẫn đến sự tái phát rất rõ ràng. 

Cho phép$req(v)$biểu thị giá trị tối thiểu mà mọi đứa trẻ phải có khả năng cung cấp. 

Nếu như$a_v \ge req(v)$, trẻ chỉ cần thỏa mãn$req(v)$. 

Nếu như$a_v < req(v)$, thâm hụt là$req(v)-a_v$. Để tạo ra mức thâm hụt đó đòi hỏi trẻ phải đáp ứng cả nhu cầu cuối cùng và nguồn tài nguyên đã tiêu thụ:$$req(child)=req(v)+(req(v)-a_v)$$

$$=2 \cdot req(v)-a_v$$Toàn bộ vấn đề trở thành một cuộc kiểm tra tính khả thi được truyền từ gốc xuống lá. 

Lực lượng vũ phu hoạt động vì các hoạt động chuyển tài nguyên lên trên. Phép lặp hoạt động vì nó nắm bắt chính xác lượng tài nguyên mà cây con phải đảm bảo để hỗ trợ giá trị đích tại cây mẹ của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không giới hạn, có thể có hàng tỷ hoạt động | O(n) | Quá chậm | 
| Tối ưu | O(n log A) | O(n) | Đã chấp nhận | 

Đây$A$là phạm vi câu trả lời, gần như lên đến$10^9$. 

## Hướng dẫn thuật toán 

### Kiểm tra tính khả thi 

Giả sử chúng ta muốn biết liệu gốc có thể đạt giá trị hay không$X$. 

Gốc ban đầu chứa$a_1$. Phần còn lại của cây phải cung cấp$$need = X-a_1$$các đơn vị bổ sung. 

Nếu như$need \le 0$, mục tiêu đã đạt được rồi. 

Mặt khác, chúng tôi xác minh đệ quy xem mọi cây con có thể hỗ trợ số lượng cần thiết hay không. 

### Ý nghĩa đệ quy 

Đối với một nút$v$, xác định tham số DFS$need$. 

Ý nghĩa là: 

> Cây con$v$phải có khả năng cung cấp ít nhất$need$đơn vị cho cha mẹ của nó. 

### Chuyển tiếp nút nội bộ 

1. Nếu$v$là một chiếc lá, trả về liệu$a_v \ge need$. 
2. Nếu$a_v \ge need$, sau đó$v$đã chứa đủ giá trị. 

Mọi đứa trẻ chỉ cần đáp ứng cùng một yêu cầu$need$. 
3. Nếu$a_v < need$, cho phép$$d = need-a_v$$bị thâm hụt. 

nút$v$phải tạo ra$d$các đơn vị bổ sung thông qua hoạt động tại$v$. 
4. Mỗi đơn vị như vậy có giá một đơn vị từ mỗi cây con. 

Do đó mỗi đứa trẻ phải cung cấp$$need+d =2\cdot need-a_v$$đơn vị. 
5. Xác minh đệ quy từng trẻ về yêu cầu mới đó. 

### Tìm kiếm nhị phân 

1. Đặt phạm vi tìm kiếm là$[a_1, 10^{18}]$. 
2. Về điểm giữa$mid$, chạy DFS khả thi. 
3. Nếu khả thi, hãy di chuyển giới hạn dưới lên trên. 
4. Nếu không, hãy di chuyển giới hạn trên xuống dưới. 
5. Giá trị khả thi lớn nhất chính là đáp án. 

### Tại sao nó hoạt động 

Bất biến DFS là: 

> Khi một nút được kiểm tra bằng tham số$need$, phép đệ quy xác định liệu cây con đó có thể đảm bảo ít nhất$need$các đơn vị tại chính nút đó sau bất kỳ hoạt động nội bộ cần thiết nào. 

Đối với một chiếc lá, điều này là ngay lập tức vì không có thao tác nào sẵn có. 

Đối với một nút nội bộ, nếu giá trị của chính nó đã đáp ứng yêu cầu thì nút con không cần phải bù. Nếu nó ngắn lại$d$, chính xác$d$các hoạt động tại nút đó là cần thiết. Mỗi thao tác như vậy tiêu tốn một đơn vị từ mỗi cây con con, do đó mỗi cây con phải hỗ trợ thêm một đơn vị$d$đơn vị. Việc tái diễn là cần thiết và đủ. 

Vì vị từ khả thi là đơn điệu nên nếu một giá trị$X$có thể đạt được thì mọi giá trị nhỏ hơn cũng có thể đạt được. Tìm kiếm nhị phân là hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 20)

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        children = [[] for _ in range(n)]

        parents = list(map(int, input().split()))
        for v in range(1, n):
            p = parents[v - 1] - 1
            children[p].append(v)

        def feasible(target):
            need_root = target - a[0]
            if need_root <= 0:
                return True

            INF = 10**18

            def dfs(v, need):
                if need > INF:
                    return False

                if not children[v]:
                    return a[v] >= need

                if a[v] >= need:
                    child_need = need
                else:
                    child_need = 2 * need - a[v]
                    if child_need > INF:
                        return False

                for to in children[v]:
                    if not dfs(to, child_need):
                        return False

                return True

            for to in children[0]:
                if not dfs(to, need_root):
                    return False

            return True

        lo = a[0]
        hi = 10**18

        while lo < hi:
            mid = (lo + hi + 1) // 2

            if feasible(mid):
                lo = mid
            else:
                hi = mid - 1

        print(lo)

solve()
```Cây được lưu trữ dưới dạng danh sách kề của con vì mọi quá trình truyền tải DFS đều được thực hiện từ cha mẹ sang con. 

chức năng`feasible(target)`kiểm tra xem gốc có thể được nâng lên`target`. số lượng`need_root`chính xác là số tiền phải đến từ các nút con của gốc. 

DFS thực hiện phép lặp xuất phát ở trên. Lá là trường hợp cơ bản. Các nút nội bộ đã đáp ứng yêu cầu hoặc phải bù đắp sự thiếu hụt bằng cách tiêu thụ tài nguyên từ tất cả các cây con. 

Bộ bảo vệ chống tràn sử dụng`INF`là quan trọng. Dọc theo một chuỗi dài, sự tái diễn có thể liên tục tăng gấp đôi giá trị cần thiết. Câu trả lời thực sự không bao giờ vượt quá phạm vi tìm kiếm nhị phân đã chọn, do đó, bất kỳ yêu cầu nào ở trên`10^18`có thể được coi là không thể một cách an toàn. 

Tìm kiếm nhị phân sử dụng công thức điểm giữa trên tiêu chuẩn`(lo + hi + 1) // 2`để tránh các vòng lặp vô hạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 1 0 2
1 1 3
```Cây:```
    1(0)
   /   \
 2(1)  3(0)
          \
           4(2)
```Kiểm tra xem root có thể trở thành 1 hay không. 

| Nút | Bắt buộc | Giá trị | Yêu cầu về Trẻ em | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | lá | 
| 3 | 1 | 0 | 2 | 
| 4 | 2 | 2 | lá | 

Tất cả các kiểm tra thành công. 

Kiểm tra xem root có thể trở thành 2 hay không. 

| Nút | Bắt buộc | Giá trị | Yêu cầu về Trẻ em | 
| --- | --- | --- | --- | 
| 2 | 2 | 1 | thất bại | 

Mục tiêu thất bại ngay lập tức. 

Trả lời = 1. 

Dấu vết này cho thấy thâm hụt lan truyền đi xuống như thế nào. Nút 3 thiếu một đơn vị nên nút con của nó phải đáp ứng yêu cầu lớn hơn. 

### Mẫu 3 

đầu vào:```
5
2 5 3 9 6
3 1 5 2
```Cây:```
      1(2)
     /
   3(3)
   /
 5(6)
 /
2(5)
/
4(9)
```Kiểm tra mục tiêu 6. 

Root cần thêm 4 đơn vị. 

| Nút | Bắt buộc | Giá trị | Yêu cầu về Trẻ em | 
| --- | --- | --- | --- | 
| 3 | 4 | 3 | 5 | 
| 5 | 5 | 6 | 5 | 
| 2 | 5 | 5 | 5 | 
| 4 | 5 | 9 | lá | 

Thành công. 

Kiểm tra mục tiêu 7. 

Root cần thêm 5 đơn vị. 

| Nút | Bắt buộc | Giá trị | Yêu cầu về Trẻ em | 
| --- | --- | --- | --- | 
| 3 | 5 | 3 | 7 | 
| 5 | 7 | 6 | 8 | 
| 2 | 8 | 5 | 11 | 
| 4 | 11 | 9 | thất bại | 

Đáp án = 6. 

Ví dụ này thể hiện sự khuếch đại thâm hụt lặp đi lặp lại dọc theo chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log A) | Mỗi lần kiểm tra tính khả thi sẽ truy cập vào mỗi nút một lần, tìm kiếm nhị phân thực hiện kiểm tra O(log A) | 
| Không gian | O(n) | Lưu trữ cây cộng với ngăn xếp đệ quy | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Với khoảng 60 lần lặp tìm kiếm nhị phân cho một$10^{18}$phạm vi, giải pháp thực hiện khoảng$1.2 \times 10^7$lượt truy cập nút trong trường hợp xấu nhất, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 20)

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        children = [[] for _ in range(n)]

        p = list(map(int, input().split()))
        for v in range(1, n):
            children[p[v - 1] - 1].append(v)

        def feasible(target):
            need = target - a[0]
            if need <= 0:
                return True

            def dfs(v, req):
                if not children[v]:
                    return a[v] >= req

                child_req = req if a[v] >= req else 2 * req - a[v]

                for to in children[v]:
                    if not dfs(to, child_req):
                        return False
                return True

            return all(dfs(to, need) for to in children[0])

        lo, hi = a[0], 10**18

        while lo < hi:
            mid = (lo + hi + 1) // 2
            if feasible(mid):
                lo = mid
            else:
                hi = mid - 1

        out.append(str(lo))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""3
4
0 1 0 2
1 1 3
2
3 0
1
5
2 5 3 9 6
3 1 5 2
"""
) == "1\n3\n6\n"

# minimum tree
assert run(
"""1
2
0 5
1
"""
) == "5\n"

# root already large
assert run(
"""1
2
10 0
1
"""
) == "10\n"

# chain blocked by zero leaf
assert run(
"""1
3
0 10 0
1 2
"""
) == "0\n"

# all equal values
assert run(
"""1
3
5 5 5
1 1
"""
) == "10\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút có giá trị 0 và 5 | 5 | Cây tầm thường nhỏ nhất | 
| Root đã lớn nhất | 10 | Tìm kiếm nhị phân giới hạn dưới | 
| Chuỗi 0,10,0 | 0 | Hiệu ứng thắt cổ chai sâu | 
| Ba nút đều bằng 5 | 10 | Hành vi cây con đối xứng | 

## Vỏ cạnh 

### Lá thắt cổ sâu 

đầu vào:```
1
3
0 10 0
1 2
```Cây:```
1(0)
|
2(10)
|
3(0)
```Để làm cho gốc bằng 1, nút 2 phải cung cấp một đơn vị. Vì nút 2 đã có đủ giá trị nên DFS cũng yêu cầu nút 3 một đơn vị. Nút 3 là một lá có giá trị 0 nên việc kiểm tra không thành công. 

Thuật toán xuất ra 0. Điều này khớp với quy trình thực vì mọi thao tác tại nút 2 sẽ làm giảm nút 3 xuống dưới 0. 

### Khuếch đại thâm hụt nút bên trong 

đầu vào:```
1
3
0 1 10
1 2
```Cây:```
1(0)
|
2(1)
|
3(10)
```Cố gắng làm cho nghiệm bằng 6 yêu cầu nút 2 cung cấp 6 đơn vị. 

Nút 2 chỉ chứa 1 đơn vị nên mức thâm hụt là 5. 

Yêu cầu của trẻ trở thành:$$2 \cdot 6 - 1 = 11$$Lá 3 chỉ có 10 đơn vị nên mục tiêu thất bại. 

Phép truy toán phản ánh chính xác thực tế là việc tạo thêm giá trị ở nút 2 sẽ tiêu tốn tài nguyên từ nút 3. 

### Lá con của gốc 

đầu vào:```
1
2
0 7
1
```Gốc chỉ có thể nhận các tài nguyên đã có ở đỉnh 2. Vì đỉnh 2 là một lá nên nó không thể tạo thêm giá trị thông qua các hoạt động. 

DFS chỉ đơn giản kiểm tra xem chiếc lá có đủ giá trị hay không. Giá trị gốc tối đa trở thành$0 + 7 = 7$, đó chính xác là câu trả lời đúng.
