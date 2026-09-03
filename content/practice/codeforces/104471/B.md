---
title: "CF 104471B - Vấn đề 2 bộ"
description: "Chúng ta có hai tập hợp, mỗi tập hợp có kích thước $n$. Hãy coi chúng như hai hàng số, $s$ và $t$, mỗi hàng chứa chính xác các phần tử $n$, trong đó cho phép lặp lại. Trong một lần di chuyển, chúng ta chọn một phần tử từ $s$ và một phần tử từ $t$, nhưng chỉ khi các giá trị khác nhau."
date: "2026-06-30T12:50:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104471
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #20 (7-Problems-Forces)"
rating: 0
weight: 104471
solve_time_s: 101
verified: true
draft: false
---

[CF 104471B - Sự cố 2 bộ](https://codeforces.com/problemset/problem/104471/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp hai bộ nhiều bộ, mỗi bộ có kích thước$n$. Hãy coi chúng như hai hàng số,$s$Và$t$, mỗi cái chứa chính xác$n$các yếu tố, trong đó sự lặp lại được cho phép. 

Trong một lần di chuyển, chúng ta chọn một phần tử từ$s$và một phần tử từ$t$, nhưng chỉ khi các giá trị khác nhau. Chúng tôi xóa cả hai yếu tố. Chúng tôi lặp lại thao tác này cho đến khi xóa được mọi thứ khỏi cả hai bộ nhiều bộ hoặc bị kẹt. 

Nhiệm vụ là quyết định xem có thể làm trống hoàn toàn cả hai bộ nhiều tập bằng các thao tác như vậy hay không. 

Vì mọi thao tác đều loại bỏ chính xác một phần tử ở mỗi bên nên mọi quy trình thành công đều phải bao gồm chính xác$n$hoạt động, ghép nối mọi phần tử trong$s$có đúng một phần tử trong$t$. Hạn chế duy nhất là chúng ta không được phép ghép các giá trị bằng nhau. 

Các ràng buộc rất lớn: tổng số phần tử trong tất cả các trường hợp thử nghiệm có thể đạt tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ mọi thứ bậc hai cho mỗi trường hợp thử nghiệm và thậm chí việc sắp xếp cho mỗi thử nghiệm chỉ được chấp nhận nếu được giới hạn cẩn thận. Giải pháp phải giảm thiểu từng trường hợp thử nghiệm một cách hiệu quả thành phép đếm tuyến tính hoặc tuyến tính. 

Một cách tiếp cận ngây thơ sẽ cố gắng mô phỏng việc so khớp một cách tham lam hoặc thậm chí xây dựng một sự so khớp hai bên rõ ràng. Điều đó không thành công vì nhiều tập hợp trong trường hợp xấu nhất có nhiều giá trị lặp lại sẽ buộc phải kiểm tra quá nhiều khả năng tương thích và việc khớp chung quá chậm. 

Một trường hợp thất bại tinh vi đối với việc so khớp tham lam ngây thơ xuất hiện khi các lựa chọn cục bộ chặn các cặp ghép trong tương lai. Ví dụ: nếu trước tiên chúng ta luôn ghép các giá trị trông giống hệt nhau mà không xem xét các ràng buộc về tần số chung, thì chúng ta có thể gặp khó khăn ngay cả khi tồn tại một kết quả khớp hoàn toàn hợp lệ. 

Khó khăn cốt lõi mang tính toàn cầu: một giá trị xuất hiện quá thường xuyên trên cả hai bộ nhiều tập hợp có thể “tiêu tốn” quá nhiều cơ hội ghép nối, khiến sau này không thể tránh khỏi việc ghép các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là coi đây như một bài toán đồ thị lưỡng cực. Chúng tôi có$n$các nút ở bên trái (các phần tử của$s$) Và$n$bên phải (các phần tử của$t$). Mỗi cặp$(x, y)$được phép trừ khi$x = y$. Chúng tôi muốn một sự kết hợp hoàn hảo. 

Một giải pháp đơn giản sẽ xây dựng biểu đồ đầy đủ và chạy thuật toán đối sánh hai bên tối đa. Đồ thị dày đặc, có hầu hết các cạnh, do đó độ phức tạp xấp xỉ$O(n^2)$mỗi thử nghiệm trong trường hợp xấu nhất. Với tổng số$n$lên đến$2 \cdot 10^5$, tốc độ này quá chậm. 

Quan sát quan trọng là điều duy nhất ngăn cản việc ghép đôi là sự bình đẳng về giá trị. Vì vậy, toàn bộ cấu trúc chỉ phụ thuộc vào số lượng tần số của từng giá trị chứ không phụ thuộc vào vị trí. Nếu một giá trị$v$xuất hiện nhiều lần trong cả hai tập hợp, những lần xuất hiện đó sẽ hạn chế lẫn nhau rất nhiều vì chúng không thể ghép nối với nhau. 

Thay vì suy nghĩ theo từng yếu tố riêng lẻ, chúng tôi nén mọi thứ thành số lượng tần số. Với mỗi giá trị$v$, cho phép$a_v$được tính vào$s$, Và$b_v$số lượng của nó trong$t$. 

Bây giờ hãy xem xét điều gì có thể xảy ra sai sót. Nếu một giá trị cụ thể xuất hiện quá nhiều lần trên cả hai mảng, giả sử$a_v + b_v$là rất lớn thì hầu hết những lần xuất hiện đó phải được ghép nối với các phần tử có giá trị khác. Nhưng chỉ có$n$các phần tử ở phía đối diện, do đó điều này tạo ra hạn chế về dung lượng cứng. 

Điều kiện đúng hóa ra lại đơn giản: với mọi giá trị$v$, tổng số lần xuất hiện trên cả hai mảng không được vượt quá$n$, nghĩa$a_v + b_v \le n$. Nếu điều này đúng với tất cả các giá trị thì luôn tồn tại một cặp hợp lệ; nếu không thì không thể vì một số giá trị sẽ buộc có quá nhiều cặp chéo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Kiểm tra tần số |$O(n)$mỗi bài kiểm tra |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta quy vấn đề về việc đếm tần số và kiểm tra một ràng buộc đơn giản. 

1. Đọc cả nhiều tập hợp và đếm số lần xuất hiện của mọi giá trị trong$s$Và$t$. 

Bước này thay thế lý luận vị trí bằng lý luận tần số, vì chỉ có bội số mới quan trọng đối với tính khả thi của việc ghép đôi. 
2. Với mỗi giá trị riêng biệt$v$, tính tổng$a_v + b_v$. 

Điều này đo lường số lần giá trị này xuất hiện trên cả hai mặt. 
3. Kiểm tra xem tổng số này có vượt quá không$n$. 

Nếu đúng như vậy, hãy kết luận ngay rằng một chuỗi các thao tác hợp lệ không thể tồn tại. Nguyên nhân là chỉ có$n$các phần tử ở phía đối diện để khớp với nhau, vì vậy quá nhiều bản sao của một giá trị sẽ dẫn đến xung đột không thể tránh khỏi. 
4. Nếu không có giá trị nào vi phạm điều kiện, hãy xuất ra giá trị đó. 

Trong trường hợp này, sự phân bổ đủ cân bằng để chúng ta luôn có thể sắp xếp các cặp mà không bị buộc phải xếp vào các trận đấu có giá trị bằng nhau. 

### Tại sao nó hoạt động 

Hãy nghĩ đến từng giá trị$v$như một “loại” không thể ghép nối với chính nó. Mỗi lần xuất hiện của$v$TRONG$s$phải được kết hợp với một số không$v$phần tử trong$t$, và ngược lại. Vì chỉ có$n$tổng các phần tử ở mỗi bên, nhu cầu kết hợp được tạo ra bởi một giá trị duy nhất không thể vượt quá khả năng đáp ứng sẵn có. điều kiện$a_v + b_v \le n$ngăn chặn chính xác bất kỳ giá trị nào khỏi việc độc quyền quá nhiều vị trí ghép nối, điều này đảm bảo rằng luôn có thể sắp xếp một kết hợp đầy đủ tránh các cặp bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import Counter

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        s = list(map(int, input().split()))
        t = list(map(int, input().split()))
        
        cs = Counter(s)
        ct = Counter(t)
        
        ok = True
        for v in set(cs.keys()) | set(ct.keys()):
            if cs[v] + ct[v] > n:
                ok = False
                break
        
        print("YES" if ok else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp được xây dựng hoàn toàn xung quanh bản đồ tần số. Lựa chọn triển khai chính là hợp nhất các bộ khóa của cả hai bộ đếm, đảm bảo mọi giá trị xuất hiện trong nhiều bộ đều được kiểm tra chính xác một lần. Việc thoát sớm là rất quan trọng để tránh việc lặp lại không cần thiết khi phát hiện thấy vi phạm. 

Logic không cố gắng xây dựng các cặp một cách rõ ràng. Nó chỉ xác minh tính khả thi, điều này là đủ do ràng buộc về cấu trúc hoàn toàn là trên mỗi giá trị. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
s = [1, 1, 2]
t = [3, 3, 2]
n = 3
```Chúng tôi tính toán tần số: 

| giá trị | số lượng | đếm t | tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 
| 2 | 1 | 1 | 2 | 
| 3 | 0 | 2 | 2 | 

Tất cả các khoản tiền đều$\le 3$, do đó thuật toán xuất ra CÓ. 

Điều này xác nhận rằng mặc dù các giá trị được nhóm lại nhưng không có giá trị đơn lẻ nào lấn át khả năng khớp. 

### Mẫu 2 

đầu vào:```
s = [1, 1, 1]
t = [1, 1, 1]
n = 3
```| giá trị | số lượng | đếm t | tổng hợp | 
| --- | --- | --- | --- | 
| 1 | 3 | 3 | 6 | 

Ở đây tổng vượt quá$n$, do đó thuật toán xuất ra NO. 

Điều này ghi lại trường hợp lỗi trong đó mọi phần tử đều giống hệt nhau, khiến không thể tránh việc ghép các giá trị bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi phần tử được tính một lần và mỗi giá trị riêng biệt được kiểm tra một lần | 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ hầu hết tất cả các giá trị riêng biệt | 

Tổng cộng$n$trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó nghiệm tuyến tính phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        s = list(map(int, input().split()))
        t = list(map(int, input().split()))
        
        cs = Counter(s)
        ct = Counter(t)
        
        ok = True
        for v in set(cs.keys()) | set(ct.keys()):
            if cs[v] + ct[v] > n:
                ok = False
                break
        
        out.append("YES" if ok else "NO")
    return "\n".join(out)

# provided samples
assert run("""2
3
1 1 2
3 3 2
3
1 1 1
1 1 1
""") == "YES\nNO"

# all distinct values
assert run("""1
4
1 2 3 4
5 6 7 8
""") == "YES"

# impossible heavy overlap
assert run("""1
3
1 1 1
1 1 1
""") == "NO"

# boundary minimal
assert run("""1
1
1
2
""") == "YES"

# mixed tight case
assert run("""1
5
1 1 2 2 3
3 3 4 4 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chồng chéo hoàn toàn giống hệt nhau | KHÔNG | phát hiện sự tập trung không thể | 
| bộ rời rạc | CÓ | kết hợp hợp lệ dễ dàng | 
| trường hợp tối thiểu | CÓ | xử lý đầu vào nhỏ nhất | 
| trường hợp cân bằng hỗn hợp | CÓ | xác minh tình trạng tần số | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cả hai tập hợp giống hệt nhau và bao gồm một giá trị lặp lại duy nhất. Ví dụ,$s = t = [1,1,\dots,1]$. Ở đây điều kiện không thành công ngay lập tức vì tổng tần số là$2n$, vượt quá$n$. Thuật toán từ chối chính xác điều này mà không cần thử bất kỳ ghép nối nào. 

Một trường hợp cạnh khác là khi các giá trị hoàn toàn tách biệt giữa hai tập hợp. Ví dụ,$s = [1,2,3]$Và$t = [4,5,6]$. Mọi ghép nối đều hợp lệ vì không có ràng buộc bình đẳng nào được kích hoạt. Việc kiểm tra tần số diễn ra suôn sẻ vì mỗi$a_v + b_v = 1$. 

Một trường hợp tinh vi hơn là khi một giá trị chiếm ưu thế trong một tập hợp nhiều tập hợp nhưng lại không có ở tập hợp kia. Ví dụ,$s = [1,1,1,1]$,$t = [2,2,2,2]$. Mặc dù số lượng bị sai lệch nhưng tổng của mỗi giá trị vẫn chính xác$n$, do đó thuật toán chấp nhận và tồn tại một cặp chéo hợp lệ.
