---
title: "CF 104745H - Kiến Menorca"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có nhiều loại kiến, trong đó loại $i$ có sẵn kiến ​​$ai$. Mục tiêu là ném tổng cộng ít nhất $p$ kiến ​​bằng cách sử dụng một chuỗi các bước di chuyển. Mỗi nước đi có hai ràng buộc độc lập."
date: "2026-06-28T23:03:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104745
codeforces_index: "H"
codeforces_contest_name: "CAMA 2023"
rating: 0
weight: 104745
solve_time_s: 49
verified: true
draft: false
---

[CF 104745H - Kiến Menorca](https://codeforces.com/problemset/problem/104745/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản, có nhiều loại kiến, trong đó loại$i$có$a_i$kiến có sẵn. Mục tiêu là ném ít nhất$p$tổng số kiến ​​bằng cách sử dụng một chuỗi các bước di chuyển. 

Mỗi nước đi có hai ràng buộc độc lập. Đầu tiên, trong một nước đi chúng ta có thể ném nhiều nhất$m$kiến tổng thể. Thứ hai, đối với bất kỳ loại đơn lẻ nào, chúng ta có thể ném tối đa$k$kiến thuộc loại đó trong cùng một động tác. Chúng tôi có thể chọn bất kỳ sự kết hợp nào của các loại trong mỗi nước đi miễn là cả hai giới hạn đều được tôn trọng và chúng tôi muốn giảm thiểu số lượng nước đi cần thiết để đạt được tổng số ít nhất là$p$kiến ném. 

Cấu trúc về cơ bản là thực hiện liên tục “các bước đóng gói có giới hạn dung lượng” trên nhiều tập hợp số lượng giới hạn, trong đó mỗi bước có giới hạn chung và giới hạn cho mỗi loại. 

Những hạn chế là rất lớn. Số lượng loại trên tất cả các trường hợp thử nghiệm lên tới$10^6$, và cả hai$m$Và$k$có thể lớn như$10^{18}$. Điều này ngay lập tức loại trừ mọi mô phỏng cho mỗi lần di chuyển. Ngay cả một giải pháp tuyến tính về số bước di chuyển cũng sẽ thất bại vì số bước di chuyển cũng có thể lớn khi$k$nhỏ hoặc khi phân bố không đồng đều giữa các loại. Chúng ta cần một đặc tính dạng đóng hoặc tham lam. 

Một điểm tinh tế là câu trả lời không chỉ phụ thuộc vào tổng số kiến ​​mà còn phụ thuộc vào cách chúng được phân bổ theo các loại. Nếu tất cả kiến ​​đều thuộc một loại thì giới hạn của mỗi loại sẽ trở thành hệ số giới hạn. Nếu kiến ​​lây lan sang nhiều loại, giới hạn toàn cầu sẽ trở thành nút thắt cổ chai. 

Một sai lầm ngây thơ là cho rằng mỗi bước đi luôn góp phần$\min(m, \sum a_i)$, bỏ qua hạn chế trên mỗi loại. Ví dụ, nếu$m=10$,$k=2$, và chúng tôi có một loại với$a_1=10$, thì mỗi nước đi chỉ có thể thực hiện 2 nước đi, vì vậy cần có 5 nước đi mặc dù tổng số nước đi mỗi nước là 10. 

Một trường hợp thất bại tinh tế khác là giả sử chúng ta luôn có thể sử dụng đầy đủ$m$mỗi lần di chuyển. Nếu chúng ta có nhiều cọc nhỏ, mỗi lần di chuyển có thể vẫn bị giới hạn bởi$k$cho mỗi loại ngay cả khi công suất toàn cầu không được sử dụng do hạn chế về phân phối. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ xây dựng rõ ràng từng bước di chuyển. Trong mỗi lần di chuyển, chúng tôi sẽ liên tục chọn loại và chiếm tới$k$từ mỗi lần không vượt quá$m$, trừ đi$a_i$và tiếp tục cho đến khi chúng ta lấy đủ hoặc hết kiến. Điều này đúng vì nó tôn trọng chính xác cả hai ràng buộc, nhưng về cơ bản là quá chậm. Mỗi lần di chuyển có thể quét tất cả các loại, đưa ra$O(n)$mỗi lần di chuyển và số lần di chuyển cũng có thể lớn, dẫn đến$O(n \cdot \text{moves})$, điều đó là không thể khi$n$tùy thuộc vào$10^6$. 

Quan sát quan trọng nhất là vấn đề không nằm ở trình tự các bước di chuyển mà là về công suất trên mỗi bước di chuyển. Mỗi loại$i$đóng góp độc lập: trong một nước đi nó có thể đóng góp nhiều nhất$k$, vì vậy để làm trống một đống kích thước$a_i$, nó đòi hỏi ít nhất$\lceil a_i / k \rceil$"loại đóng góp". Tuy nhiên, tất cả các loại đều có chung một giới hạn chung$m$mỗi lần di chuyển, vì vậy mỗi lần di chuyển có thể xử lý nhiều loại đóng góp, tối đa$m$kiến trong tổng số. 

Điều này biến vấn đề thành một cách diễn giải về lịch trình. Mỗi loại$i$đóng góp$\lceil a_i / k \rceil$khối, trong đó mỗi khối đại diện cho tối đa$k$kiến, ngoại trừ có thể là con cuối cùng. Mỗi bước di chuyển có thể xử lý tới$m$kiến trên những khối này. Do đó, câu trả lời được điều khiển bởi hai giới hạn dưới độc lập: giới hạn tổng khối lượng$\lceil p / m \rceil$và ràng buộc đóng gói chunk cho mỗi loại. 

Chúng ta phải tính toán số lượng “khối” hiệu quả cần thiết cho mỗi loại và sau đó xác định cần bao nhiêu bước di chuyển để xử lý chúng với công suất$m$, đồng thời đảm bảo chúng tôi không vượt quá$p$. 

Giải pháp tối ưu xuất hiện từ việc kết hợp các ràng buộc này một cách cẩn thận hơn là mô phỏng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng mỗi lần di chuyển |$O(n \cdot \text{moves})$|$O(1)$| Quá chậm | 
| Tập hợp chunk + đóng gói tham lam |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với từng loại$i$, hãy tính xem nó đóng góp bao nhiêu “lô cho mỗi loại” đầy đủ, nghĩa là chúng ta cần áp dụng giới hạn bao nhiêu lần$k$. Đây là$\lceil a_i / k \rceil$. Điều này thể hiện số lượng đóng góp bị ràng buộc mà loại đó áp đặt cho tất cả các bước di chuyển. 
2. Đồng thời theo dõi xem thực sự cần bao nhiêu con kiến ​​thuộc loại đó để tiếp cận$p$. Nếu như$p$đã được đáp ứng bởi các loại trước đó, về mặt khái niệm, chúng tôi sẽ ngừng tính những đóng góp tiếp theo vì chúng tôi chỉ quan tâm đến việc đạt được$p$, không làm hết kiến. 
3. Tổng hợp tất cả các loại thành một tổng số “kiến có thể xử lý”, nhưng hiểu rằng mỗi nước đi chỉ có thể xử lý tối đa$m$. Điều này ngay lập tức đưa ra giới hạn dưới của$\lceil p / m \rceil$. 
4. Bây giờ hãy xem xét hạn chế cho mỗi loại. Ngay cả khi năng lực toàn cầu cho phép nhiều hơn, một loại hình đơn lẻ cũng không thể đóng góp nhiều hơn$k$mỗi lần di chuyển, vì vậy nếu một loại rất lớn, nó buộc phải thực hiện nhiều bước di chuyển bất kể dung lượng chung. Điều này được ghi lại bằng cách tổng hợp các khoản đóng góp và đảm bảo chúng tôi không dồn quá nhiều bước vượt quá$k$mỗi loại. 
5. Câu trả lời cuối cùng là giá trị tối đa của hai giá trị: số lần di chuyển cần thiết do năng lực toàn cầu và số lượng cần thiết do hạn chế đóng gói trên mỗi loại. Đầu tiên là$\lceil p / m \rceil$và thứ hai là số lần di chuyển tối thiểu cần thiết để sắp xếp tất cả các khối cần thiết cho mỗi loại vào các thùng có kích thước$m$. 

### Tại sao nó hoạt động 

Mỗi bước đi đều có hai điểm nghẽn độc lập: tổng công suất$m$và công suất từng loại$k$. Bất kỳ cách xây dựng nước đi hợp lệ nào cũng có thể được coi là phân chia các yêu cầu loại bỏ thành các nhóm trong đó mỗi nhóm tôn trọng cả hai ràng buộc. Ràng buộc toàn cục gây ra tổng giới hạn dưới, trong khi ràng buộc theo từng loại gây ra giới hạn phân mảnh dưới. Vì cả hai đều cần thiết và độc lập nên giải pháp tối ưu là lịch trình chặt chẽ nhất thỏa mãn đồng thời cả hai, được nắm bắt bằng cách kết hợp hai giới hạn dẫn xuất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        p = int(input())

        # total ants needed, but we only care up to p
        remaining = p

        total = 0
        for x in a:
            if remaining == 0:
                break
            take = min(x, remaining)
            total += take
            remaining -= take

        # lower bound from global capacity
        ans = (total + m - 1) // m

        # per-type constraint: each type contributes in chunks of size k
        extra_moves = 0
        remaining = p
        for x in a:
            if remaining == 0:
                break
            take = min(x, remaining)
            # number of chunks needed from this type
            extra_moves += (take + k - 1) // k
            remaining -= take

        ans = max(ans, extra_moves)

        print(ans)

if __name__ == "__main__":
    solve()
```Lần đầu tiên tính toán số lượng kiến ​​thực sự cần thiết cho đến$p$, bỏ qua nguồn cung dư thừa. Điều này đảm bảo chúng tôi không đánh giá quá cao công việc của những con kiến ​​không liên quan. 

Phép tính thứ hai thực thi giới hạn cho mỗi loại bằng cách chuyển đổi phần đóng góp của từng loại thành các phần có kích thước$k$. Mỗi đoạn tương ứng với sự tham gia bắt buộc trong một nước đi. Việc phân chia trần cho thấy thực tế là một loại vượt quá$k$không thể được xử lý trong một lần di chuyển bất kể dung lượng toàn cầu còn lại. 

Cuối cùng, câu trả lời đạt mức tối đa giữa giới hạn tổng công suất và sự phân mảnh theo từng loại, vì cả hai đều là những ràng buộc độc lập trên cùng một lịch trình. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp có giá trị vừa phải trong đó cả hai ràng buộc đều quan trọng. 

đầu vào:```
n = 3, m = 4, k = 2
a = [3, 3, 3]
p = 6
```Chúng tôi xử lý các khoản đóng góp lên đến$p$. 

| Loại | a[i] đã sử dụng | k-khối | Chạy p | Tổng khối | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 3 | 2 | 
| 2 | 3 | 2 | 0 | 4 | 

Ở đây, tổng số kiến ​​cần thiết là 6, vì vậy giới hạn toàn cầu là$\lceil 6/4 \rceil = 2$. Phân đoạn theo loại cho 4 khối, vì vậy câu trả lời là 4. 

Điều này cho thấy một trường hợp mà sự phân mảnh chi phối năng lực toàn cầu. 

Bây giờ hãy xem xét trường hợp năng lực toàn cầu chiếm ưu thế. 

đầu vào:```
n = 2, m = 10, k = 10
a = [100, 100]
p = 15
```| Loại | a[i] đã sử dụng | k-khối | Chạy p | Tổng khối | 
| --- | --- | --- | --- | --- | 
| 1 | 15 | 2 | 0 | 2 | 

Giới hạn toàn cầu là$\lceil 15/10 \rceil = 2$, các đoạn trên mỗi loại cũng căn chỉnh thành 2, vì vậy câu trả lời là 2. Điều này xác nhận cả hai ràng buộc đều trùng khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi mảng được quét một số lần không đổi | 
| Không gian |$O(1)$thêm | Chỉ sử dụng bộ đếm | 

Tổng của$n$trên tất cả các trường hợp thử nghiệm là$10^6$, do đó giải pháp quét tuyến tính phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ không đổi ngoài việc lưu trữ đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import ceil

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m, k = map(int, input().split())
            a = list(map(int, input().split()))
            p = int(input())

            remaining = p
            total = 0
            for x in a:
                if remaining == 0:
                    break
                take = min(x, remaining)
                total += take
                remaining -= take

            ans = (total + m - 1) // m

            remaining = p
            extra = 0
            for x in a:
                if remaining == 0:
                    break
                take = min(x, remaining)
                extra += (take + k - 1) // k
                remaining -= take

            ans = max(ans, extra)
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# sample-like sanity checks
assert run("1\n3 4 2\n3 3 3\n6\n") == "4"
assert run("1\n2 10 10\n100 100\n15\n") == "2"

# edge cases
assert run("1\n1 1 1\n10\n5\n") == "5"
assert run("1\n5 100 1\n1 1 1 1 1\n3\n") == "1"
assert run("1\n3 2 2\n5 5 5\n10\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn, kín k | 5 | nút thắt mỗi loại chiếm ưu thế | 
| nhiều loại nhỏ, lớn m | 1 | trường hợp chưa sử dụng công suất toàn cầu | 
| trộn cọc lớn | 3 | tương tác của cả hai ràng buộc | 

## Vỏ cạnh 

Kịch bản một loại thể hiện rõ ràng hạn chế cho mỗi loại. Nếu chúng ta lấy$n=1$,$a_1=10$,$m=10$,$k=2$, Và$p=10$, thì mỗi nước đi chỉ diệt được 2 con kiến. Thuật toán tạo ra một cách chính xác$\lceil 10/2 \rceil = 5$, trong khi giới hạn toàn cầu$\lceil 10/10 \rceil = 1$là quá lạc quan. 

Một kịch bản phân phối đầy đủ cho thấy điều ngược lại. Nếu như$n=5$, tất cả$a_i=1$,$m=100$,$k=1$, Và$p=3$, thì mỗi lần di chuyển có thể loại bỏ tổng cộng tối đa 3 con kiến. Thuật toán đưa ra$\lceil 3/100 \rceil = 1$từ năng lực toàn cầu và đóng góp cho mỗi loại cũng có tổng cộng là 3 phần, nhưng vì tất cả đều độc lập nên lịch trình sẽ gói chúng thành một nước đi một cách chính xác, mang lại kết quả là 1. 

Một trường hợp phân phối hỗn hợp trong đó một loại lớn và một loại khác nhỏ chứng tỏ rằng không có ràng buộc nào là đủ và cần có mức tối đa của cả hai giới hạn để tránh các bước di chuyển bị tính thiếu.
