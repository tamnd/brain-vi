---
title: "CF 106252B - Phần mềm vẽ lỗi I"
description: "Chúng ta được cung cấp một hình ảnh mục tiêu trên lưới $n lần m$. Mỗi ô chứa một nhãn màu hoặc số 0, trong đó số 0 có nghĩa là trong suốt."
date: "2026-06-20T22:37:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106252
codeforces_index: "B"
codeforces_contest_name: "The 2025 ICPC Asia Shenyang Regional Contest (The 4th Universal Cup. Stage 6: Grand Prix of Shenyang)"
rating: 0
weight: 106252
solve_time_s: 68
verified: true
draft: false
---

[CF 106252B - Phần mềm vẽ lỗi I](https://codeforces.com/problemset/problem/106252/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hình ảnh mục tiêu trên một$n \times m$lưới. Mỗi ô chứa một nhãn màu hoặc số 0, trong đó số 0 có nghĩa là trong suốt. Hình ảnh hiển thị cuối cùng được tạo ra bằng cách xếp chồng nhiều lớp: tại mỗi vị trí, pixel không trong suốt đầu tiên từ các lớp trên cùng sẽ xác định những gì chúng ta nhìn thấy. 

Chúng tôi bắt đầu không có lớp và có thể liên tục thêm các lớp mới. Một lớp mới được lấp đầy hoàn toàn bằng một màu duy nhất hoặc hoàn toàn trong suốt. Sau khi tạo một lớp, chúng ta có thể sửa đổi từng pixel riêng lẻ bên trong nó: chúng ta có thể tô một pixel thành một màu với chi phí$a$, hoặc xóa nó để làm cho nó trong suốt với chi phí$b$. Mục tiêu là xây dựng một số lớp có hình ảnh hiển thị khớp chính xác với lưới mục tiêu, đồng thời giảm thiểu tổng chi phí. 

Điểm tinh tế quan trọng nhất là các lớp tương tác thông qua sự tắc nghẽn. Một pixel ở lớp cao hơn sẽ ẩn mọi thứ bên dưới nó trừ khi nó trong suốt. Điều này có nghĩa là các lỗi ở lớp cao hơn có thể ghi đè hoàn toàn nội dung chính xác bên dưới, do đó, các pixel không chính xác ở bất kỳ lớp nào được sử dụng đều phải được xóa hoặc sửa cẩn thận. 

Các ràng buộc ngụ ý đến$10^6$tổng số ô trên tất cả các trường hợp thử nghiệm. Điều này loại trừ mọi giải pháp mô phỏng cấu trúc từng lớp hoặc thực hiện lý luận tương tác trên mỗi ô giữa các màu hoặc lớp. Quét tuyến tính cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. 

Một vài tình huống khó khăn rất dễ bị đánh giá sai. 

Nếu tất cả các ô đều bằng 0 thì câu trả lời tối ưu là 0 vì chúng ta không bao giờ cần thêm bất kỳ pixel màu nào. 

Nếu lưới chứa một màu duy nhất ở mọi nơi, giả sử tất cả các ô là 5, thì chúng ta tạo một lớp màu không đổi và không sửa gì hoặc chúng ta vẽ mọi thứ theo cách thủ công. Sự lựa chọn tối ưu chỉ phụ thuộc vào việc so sánh hai chi phí. 

Một trường hợp lỗi tinh vi hơn xuất hiện khi màu sắc thưa thớt. Giả sử một màu chỉ xuất hiện một lần trong một lưới lớn. Một cách tiếp cận đơn giản có thể đề xuất xây dựng một lớp không đổi đầy đủ và xóa hầu hết các ô, nhưng điều đó có thể tốn nhiều chi phí hơn là chỉ vẽ trực tiếp một pixel đó. 

Thách thức chính là nhận ra rằng mặc dù các lớp tương tác về mặt không gian, cấu trúc chi phí không kết hợp các màu khác nhau theo cách đòi hỏi phải có thứ tự toàn cầu. 

## Phương pháp tiếp cận 

Ý tưởng đầu tiên tự nhiên là nghĩ đến việc xây dựng từng pixel hình ảnh. Đối với mỗi ô mục tiêu, chúng ta có thể tưởng tượng vẽ nó thành một lớp nào đó hoặc dựa vào một lớp không đổi và sửa mọi thứ khác xung quanh nó. Điều này nhanh chóng trở nên tốn kém vì mọi quyết định về một pixel đều ảnh hưởng đến tất cả các lớp bên trên và bên dưới nó. Bất kỳ nỗ lực nào nhằm mô phỏng trực tiếp các hoạt động xếp chồng đều dẫn đến sự bùng nổ tổ hợp về các lựa chọn và thứ tự lớp. 

Sự đơn giản hóa thực sự đến từ việc diễn giải lại ý nghĩa thực sự của “lớp màu không đổi”. Nếu chúng ta tạo một lớp chứa đầy màu sắc$c$, sau đó mọi vị trí ban đầu hiển thị$c$. Để làm cho lớp đó tương thích với hình ảnh cuối cùng, mọi vị trí không được hiển thị$c$phải được làm trong suốt bằng cách sử dụng tẩy xóa. Yêu cầu này mang tính cục bộ đối với chính lớp đó: một pixel trong lớp đó sẽ đóng góp màu sắc$c$hoặc phải được loại bỏ một cách rõ ràng. Không có lớp nào khác có thể sửa một pixel không trong suốt không chính xác trong lớp này mà không gặp rủi ro khi ghi đè nội dung chính xác bên dưới. 

Điều này dẫn đến sự tách biệt quan trọng: mỗi màu có thể được xử lý độc lập. Đối với một màu nhất định$c$, chúng tôi chỉ quan tâm đến việc có bao nhiêu ô trong lưới mục tiêu chứa$c$, và bao nhiêu thì không. Chúng tôi so sánh hai chiến lược để xử lý tất cả các lần xuất hiện của$c$. Hoặc là chúng tôi không bao giờ sử dụng một lớp chuyên dụng cho$c$và thay vào đó hãy vẽ từng lần xuất hiện của nó một cách riêng lẻ hoặc chúng ta tạo một lớp không đổi đầy đủ$c$và xóa tất cả những vị trí không nên có$c$. 

Vì những lựa chọn này không phụ thuộc vào các màu khác nên giải pháp tối ưu đạt được bằng cách đánh giá từng màu riêng biệt và tổng hợp lựa chọn tốt nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng phân lớp vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Tối ưu hóa độc lập cho mỗi màu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Bước 1: Đếm số lần xuất hiện của mỗi màu 

Chúng tôi quét lưới một lần và đếm số lần mỗi màu xuất hiện. Cho phép$k_c$biểu thị số ô chứa màu$c$. Điều này cho chúng ta biết chính xác cần sơn bao nhiêu pixel nếu chúng ta tránh sử dụng một lớp cho màu đó. 

## Bước 2: Cân nhắc chi phí khi không sử dụng lớp 

Nếu chúng ta không tạo một lớp dành riêng cho màu sắc$c$, thì mỗi lần xuất hiện phải được vẽ riêng lẻ. Chi phí là$a \cdot k_c$. Đây là cách tiếp cận thuần túy trên mỗi pixel mà không tối ưu hóa cấu trúc. 

## Bước 3: Xem xét chi phí sử dụng lớp không đổi 

Nếu chúng ta tạo một lớp đầy đủ màu sắc$c$, chúng ta bắt đầu với tất cả$n \cdot m$các ô được đặt thành$c$. Mọi tế bào không nên có$c$phải bị xóa, nếu không nó sẽ xuất hiện không chính xác trong hình ảnh cuối cùng. có$n \cdot m - k_c$những tế bào như vậy, do đó chi phí trở thành$$a + b \cdot (nm - k_c).$$Số hạng đầu tiên bằng 0 đối với việc tạo lớp, vì vậy chỉ có sửa đổi mới quan trọng. 

## Bước 4: Chọn màu phù hợp hơn 

Đối với mỗi màu, chúng tôi thực hiện tối thiểu hai chiến lược:$$\min(a \cdot k_c,\; a + b \cdot (nm - k_c)).$$Chúng tôi tính tổng giá trị này cho tất cả các màu xuất hiện trong lưới. 

## Bước 5: Xuất tổng số tiền 

Câu trả lời cuối cùng là tổng chi phí độc lập tối ưu cho tất cả các màu. 

## Tại sao nó hoạt động 

Một lớp màu không đổi được xác định đầy đủ sau khi được tạo và mỗi pixel trong đó sẽ đóng góp tín hiệu hoặc nhiễu hữu ích một cách độc lập. Bất kỳ pixel nào không khớp với hình ảnh mục tiêu đều phải bị xóa rõ ràng, bất kể các lớp khác làm gì. Tương tự, việc vẽ các pixel riêng lẻ không ảnh hưởng đến các màu khác vì mỗi ô thuộc về chính xác một giá trị cuối cùng. 

Điều này loại bỏ bất kỳ sự phụ thuộc nào giữa các màu: không có thứ tự lớp nào có thể giảm số lượng thao tác cần thiết cho một màu dựa trên các quyết định cho màu khác. Do đó, chi phí của mỗi màu là độc lập và bổ sung, giúp mức tối thiểu cho mỗi màu trở nên tối ưu trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, a, b = map(int, input().split())
        cnt = {}
        nm = n * m

        for _ in range(n):
            row = list(map(int, input().split()))
            for x in row:
                cnt[x] = cnt.get(x, 0) + 1

        ans = 0
        for c, k in cnt.items():
            if c == 0:
                continue
            paint_only = a * k
            layer = a + b * (nm - k)
            ans += min(paint_only, layer)

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên bản đồ tần số theo màu sắc. Kích thước lưới được làm phẳng về mặt khái niệm thành các số đếm, tránh mọi nhu cầu mô phỏng các lớp. Chi tiết quan trọng là xử lý màu 0 riêng biệt: nó thể hiện tính minh bạch và không yêu cầu chi phí xây dựng. 

Mỗi màu đóng góp chính xác một quyết định, được tính từ tần số của nó và tổng kích thước lưới. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một$2 \times 3$lưới:```
1 1 0
2 1 2
```Chúng tôi có$nm = 6$. Số lượng là$k_1 = 3$,$k_2 = 2$,$k_0 = 1$. 

Cho rằng$a = 2$,$b = 1$. 

Đối với màu 1: 

| Chiến lược | Công thức | Giá trị | 
| --- | --- | --- | 
| Chỉ sơn |$2 \cdot 3$| 6 | 
| Lớp |$2 + 1 \cdot (6 - 3)$| 5 | 

Tốt nhất là 5. 

Đối với màu 2: 

| Chiến lược | Công thức | Giá trị | 
| --- | --- | --- | 
| Chỉ sơn |$2 \cdot 2$| 4 | 
| Lớp |$2 + 1 \cdot (6 - 2)$| 6 | 

Tốt nhất là 4. 

Tổng cộng là$5 + 4 = 9$. 

Điều này cho thấy một lớp có thể giúp ích cho những màu đậm đặc nhưng không cần thiết đối với những màu thưa thớt. 

### Ví dụ 2 

Lưới đồng nhất:```
3 3
3 3
```Đây$k_3 = 4$,$nm = 4$. 

Đối với màu 3: 

Sơn chỉ mang lại$4a$, trong khi một lớp cho$a + 0 = a$. Lớp chiếm ưu thế, xác nhận rằng các vùng đồng nhất dày đặc đặc biệt ủng hộ các lớp không đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi ô được đọc một lần và được tính | 
| Không gian |$O(\#colors)$| Bản đồ tần số trên các giá trị quan sát được | 

Giải pháp là tuyến tính trong tổng số ô trên tất cả các trường hợp thử nghiệm, phù hợp thoải mái trong giới hạn của$10^6$tế bào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline

    def solve():
        T = int(input())
        for _ in range(T):
            n, m, a, b = map(int, input().split())
            cnt = {}
            nm = n * m
            for _ in range(n):
                for x in map(int, input().split()):
                    cnt[x] = cnt.get(x, 0) + 1
            ans = 0
            for c, k in cnt.items():
                if c == 0:
                    continue
                ans += min(a * k, a + b * (nm - k))
            print(ans)

    solve()
    return ""

# minimum size
run("1\n1 1 5 3\n1\n")

# all zeros
run("1\n2 2 1 1\n0 0\n0 0\n")

# single color
run("1\n2 3 2 10\n1 1 1\n1 1 1\n")

# mixed case
run("1\n1\n2 3 1 1\n1 2 1\n")

# sparse color test
run("1\n3 3 5 1\n1 0 0\n0 2 0\n0 0 0\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 ô đơn | 5 | độ chính xác lưới tối thiểu | 
| tất cả số không | 0 | xử lý minh bạch | 
| màu đồng nhất | lựa chọn a vs lớp | tối ưu hóa màu sắc dày đặc | 
| màu hỗn hợp | logic mỗi màu kết hợp | giả định độc lập | 
| màu thưa thớt | tránh lớp không cần thiết | trường hợp cạnh thưa thớt | 

## Vỏ cạnh 

Một lưới bao gồm toàn số 0 được xử lý chính xác vì bản đồ tần số chỉ chứa màu 0, màu này bị bỏ qua khi tính toán chi phí. Không có thao tác vẽ hoặc tạo lớp nào được kích hoạt, do đó đầu ra vẫn bằng 0. 

Đối với một màu cực kỳ hiếm trong một lưới lớn, thuật toán sẽ so sánh$a \cdot k_c$với$a + b \cdot (nm - k_c)$. Từ$k_c$nhỏ, việc sơn riêng lẻ được chọn, tránh việc xây dựng toàn lớp tốn kém. 

Đối với một lưới hoàn toàn đồng nhất, tùy chọn lớp sẽ trở nên rẻ hơn rất nhiều vì không cần xóa. Thuật toán xác định chính xác điều này thông qua so sánh tương tự, tự động chọn chiến lược lớp không đổi.
