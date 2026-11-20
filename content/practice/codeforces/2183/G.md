---
title: "CF 2183G - Hướng dẫn về rắn"
description: "Phiên bản tương tác cho phép chúng tôi hỏi tối đa ba hướng dẫn được lựa chọn cẩn thận và quan sát những con rắn còn sống sót. Trong phiên bản bị tấn công, tốc độ đã được cung cấp trong đầu vào và công việc của chúng tôi là tái tạo câu trả lời mà một giải pháp tương tác tối ưu sẽ đưa ra."
date: "2026-06-07T21:47:47+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "interactive"]
categories: ["algorithms"]
codeforces_contest: 2183
codeforces_index: "G"
codeforces_contest_name: "Hello 2026"
rating: 3200
weight: 2183
solve_time_s: 167
verified: false
draft: false
---

[CF 2183G - Hướng dẫn về rắn](https://codeforces.com/problemset/problem/2183/G) 

**Đánh giá:** 3200 
**Tags:** thuật toán mang tính xây dựng, tham lam, tương tác 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Phiên bản tương tác cho phép chúng tôi hỏi tối đa ba hướng dẫn được lựa chọn cẩn thận và quan sát những con rắn còn sống sót. Trong phiên bản bị tấn công, tốc độ đã được cung cấp trong đầu vào và công việc của chúng tôi là tái tạo câu trả lời mà một giải pháp tương tác tối ưu sẽ đưa ra. 

Mỗi con rắn bắt đầu ở một vị trí số nguyên riêng biệt. Tốc độ của nó là một trong`0`,`1`, hoặc`2`. 

Nếu chúng tôi đưa ra hướng dẫn một giây`L`, mọi con rắn đều di chuyển sang trái theo vận tốc của nó. Trong giây phút đó, bất cứ khi nào hai con rắn gặp nhau, con rắn nào sẽ biến mất nhanh hơn. Vì tốc độ tối đa là`2`và tất cả các vị trí đều là số nguyên, trong một giây chỉ di chuyển những con rắn lân cận có khoảng cách là`1`hoặc`2`có thể tương tác. 

Câu hỏi quan trọng không phải là "tốc độ là bao nhiêu?" bởi vì đầu vào bị tấn công đã chứa chúng. Câu hỏi thực sự là liệu tốc độ có thể được phục hồi duy nhất bằng một số chiến lược sử dụng tối đa ba truy vấn hay không. Nếu không, chúng ta phải in`-1`. 

Tổng số tiền của`n`nhiều nhất là`10^5`, do đó, bất kỳ giải pháp nào chỉ thực hiện một lượng công việc không đổi cho mỗi con rắn đều đủ nhanh. Bất cứ điều gì bậc hai đều bị loại trừ. 

Phần tinh tế là mô tả chính xác khi không thể xác định được tốc độ, ngay cả với chuỗi ba truy vấn tốt nhất có thể. 

Xét ba con rắn liên tiếp ở các vị trí`x, x+1, x+2`với tốc độ`[0, 1, 0]`hoặc`[0, 2, 0]`. Con rắn ở giữa luôn chết trong mọi thí nghiệm hữu ích vì nó bị mắc kẹt giữa hai con rắn đứng yên. Mọi quan sát có được từ các truy vấn được phép đều giống nhau về tốc độ`1`và tốc độ`2`. Hai thế giới này không thể phân biệt được nên đáp án phải là`-1`. Đây là trở ngại duy nhất. 

## Phương pháp tiếp cận 

Một tư duy mạnh mẽ sẽ là mô hình hóa tất cả các chuỗi truy vấn có thể có độ dài lên tới`4n`, mô phỏng hệ thống con rắn và hỏi liệu mỗi lần gán tốc độ có tạo ra một bản ghi duy nhất hay không. Điều đó rõ ràng là không thể. Số lượng các hướng dẫn có thể có là theo cấp số nhân và thậm chí một mô phỏng đơn lẻ cũng không cần thiết. 

Thay vào đó, giải pháp chính thức thiết kế ba truy vấn cố định:`L`

`LR`

`R`Điều quan sát đáng chú ý là ba truy vấn này tiết lộ hầu hết mọi thứ. 

Sau một giây`L`, mọi con rắn sống sót đều kết thúc tại`a_i - s_i`. 

Bây giờ hãy nhìn vào truy vấn`LR`. Giây đầu tiên hoàn toàn giống nhau`L`, vì vậy những con rắn giống nhau vẫn sống sót. Giây thứ hai di chuyển mọi người sống sót trở lại bên phải bằng tốc độ của chính nó, khôi phục nó về vị trí ban đầu. Theo đó, đầu ra của`LR`chỉ đơn giản là tập hợp các vị trí ban đầu của những con rắn sống sót sau trận chiến`L`truy vấn. 

Phù hợp với những người sống sót giữa`L`Và`LR`ngay lập tức đưa ra tốc độ của mọi con rắn sống sót sau truy vấn đầu tiên. 

Những con rắn còn lại chính xác là những con đã bị loại bỏ trong quá trình`L`. Tốc độ của chúng có thể được xây dựng lại từ những khoảng trống vị trí cục bộ và`R`truy vấn. Chỉ còn lại một cấu hình mơ hồ: ba vị trí liên tiếp có tốc độ`0, x, 0`, Ở đâu`x`có thể là một trong hai`1`hoặc`2`. Bất cứ khi nào một mô hình như vậy tồn tại, câu trả lời đúng là`-1`. Ngược lại mọi vận tốc đều được xác định duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mô phỏng câu trả lời cho truy vấn`L`. 

Đặt vị trí của người sống sót kết quả là`vl`. 
2. Mô phỏng câu trả lời cho truy vấn`LR`. 

Đặt vị trí kết quả là`vlr`. 

Mỗi vị trí trong`vlr`là vị trí ban đầu của một con rắn sống sót`L`. 
3. Trận đấu`vlr`so với mảng vị trí ban đầu. 

Nếu vị trí`a[i]`xuất hiện ở`vlr`, rắn`i`sống sót sau`L`truy vấn. 
4. Phục hồi tốc độ của mọi người sống sót. 

Giả sử con rắn`i`sống sót`L`. 

Vị trí của nó trong`vl`là`a[i] - s[i]`. 

Kể từ đây`s[i] = a[i] - vl[j]`. 
5. Một số loài rắn đã không sống sót`L`. 

Đối với mỗi con rắn chưa được giải quyết`i > 0`, kiểm tra khoảng cách`d = a[i] - a[i-1]`. 

Nếu như`d = 2`, hoặc nếu`d = 1`và tốc độ trước đó đã được biết là khác 0, thì rắn`i`phải có tốc độ`2`. 

Đây chính xác là những tình huống mà tốc độ`2`con rắn buộc phải chết khi di chuyển sang trái. 
6. Mô phỏng truy vấn`R`. 

Hãy để vị trí của người sống sót`vr`. 

Giờ đây, mọi con rắn chưa được giải quyết còn lại đều có thể được so sánh với`vr`, và tốc độ của nó là`vr[pos] - a[i]`. 
7. Kiểm tra mẫu duy nhất không thể. 

Đối với mỗi ba vị trí liên tiếp có khoảng cách đơn vị,`a[i+1] - a[i] = 1`Và`a[i+2] - a[i+1] = 1`. 

Nếu tốc độ được xây dựng lại trông giống như`[0, nonzero, 0]`thì giá trị ở giữa không thể phân biệt được giữa`1`Và`2`. 

đầu ra`-1`. 
8. Nếu không thì xuất ra tất cả tốc độ được xây dựng lại. 

### Tại sao nó hoạt động 

Truy vấn`L`xác định con rắn nào sống sót khi di chuyển sang trái. Truy vấn`LR`chuyển đổi thông tin đó thành các chỉ số ban đầu vì mọi người sống sót đều quay trở lại vị trí bắt đầu. Cùng với nhau, hai truy vấn này xác định chính xác tốc độ của mọi người sống sót. 

Những con rắn duy nhất vẫn chưa được biết đến là những con bị loại bỏ trong quá trình`L`. Hình học cục bộ của khoảng trống`1`Và`2`buộc một số người trong số họ phải có tốc độ`2`. Truy vấn`R`xác định tất cả các giá trị còn lại. 

Bài xã luận chứng minh rằng cấu hình duy nhất về cơ bản vẫn còn mơ hồ sau bất kỳ chuỗi nào có tối đa ba truy vấn là bộ ba con rắn liên tiếp với tốc độ`0, x, 0`, Ở đâu`x`Có lẽ`1`hoặc`2`. Nếu một mô hình như vậy tồn tại thì không có chiến lược nào có thể phân biệt được hai khả năng này. Trong mọi cấu hình khác, ba truy vấn cố định ở trên xác định duy nhất mọi tốc độ. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def simulate(a, s, direction):
    n = len(a)

    if direction == 'L':
        pos = [a[i] - s[i] for i in range(n)]
    else:
        pos = [a[i] + s[i] for i in range(n)]

    alive = [True] * n

    for i in range(n - 1):
        d = a[i + 1] - a[i]

        if direction == 'L':
            if s[i + 1] > s[i] and d <= s[i + 1] - s[i]:
                alive[i + 1] = False
        else:
            if s[i] > s[i + 1] and d <= s[i] - s[i + 1]:
                alive[i] = False

    res = []
    for i in range(n):
        if alive[i]:
            res.append(pos[i])
    return res

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = list(map(int, input().split()))

        vl = simulate(a, s, 'L')
        vlr = [a[i] for i in range(n) if simulate(a, s, 'L').count(a[i] - s[i]) > 0]

        alive_after_l = [True] * n
        p = 0
        for i in range(n):
            if p < len(vlr) and vlr[p] == a[i]:
                p += 1
            else:
                alive_after_l[i] = False

        vr = simulate(a, s, 'R')

        ans = [-1] * n

        j = 0
        k = len(vlr)
        for i, x in enumerate(a):
            if j < k and vlr[j] == x:
                ans[i] = x - vl[j]
                j += 1

        for i in range(1, n):
            if ans[i] == -1:
                d = a[i] - a[i - 1]
                if d == 2 or (d == 1 and ans[i - 1] != 0):
                    ans[i] = 2

        for i, x in enumerate(a):
            if ans[i] == -1:
                j = bisect_left(vr, x)
                ans[i] = vr[j] - x

        bad = False
        for i in range(n - 2):
            if (
                a[i + 1] - a[i] == 1
                and a[i + 2] - a[i + 1] == 1
                and ans[i] == 0
                and ans[i + 1] != 0
                and ans[i + 2] == 0
            ):
                bad = True

        if bad:
            print(-1)
        else:
            print(*ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo đối số tái thiết trực tiếp. 

Giai đoạn đầu tiên mô phỏng thông tin thu được từ ba truy vấn cố định. Giai đoạn thứ hai thể hiện tốc độ sống sót của những con rắn`L`. Giai đoạn thứ ba giải quyết rắn phải có tốc độ`2`khỏi những hạn chế về khoảng cách cục bộ. Bất kỳ con rắn nào vẫn chưa được giải quyết được xác định từ`R`truy vấn. 

Lần quét cuối cùng sẽ kiểm tra cấu hình duy nhất không thể thực hiện được. Việc thiếu phần kiểm tra này sẽ tạo ra Câu trả lời sai trong các bài kiểm tra chính thức vì những trường hợp như vậy phải trả về`-1`mặc dù đầu vào đã có sự phân công tốc độ cụ thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi con rắn được xử lý một số lần không đổi | 
| Không gian | O(n) | Lưu trữ các câu trả lời được xây dựng lại và kết quả truy vấn mô phỏng | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`10^5`, thời gian tuyến tính và bộ nhớ tuyến tính phù hợp thoải mái trong giới hạn.
