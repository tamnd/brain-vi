---
title: "CF 105689F - Nhảy đèn lồng"
description: "Lễ hội có một hàng đèn lồng và mỗi chiếc đèn lồng có chiều cao hiện tại. Con rồng bắt đầu trên một chiếc đèn lồng đã chọn và cuối cùng muốn đứng trên mỗi chiếc đèn lồng. Chỉ có thể nhảy giữa các đèn lồng lân cận khi độ cao của chúng khớp với nhau tại thời điểm đó."
date: "2026-06-26T09:46:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105689
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 1-29-25 Div. 2 (Beginner)"
rating: 0
weight: 105689
solve_time_s: 50
verified: true
draft: false
---

[CF 105689F - Nhảy lồng đèn](https://codeforces.com/problemset/problem/105689/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lễ hội có một hàng đèn lồng và mỗi chiếc đèn lồng có chiều cao hiện tại. Con rồng bắt đầu trên một chiếc đèn lồng đã chọn và cuối cùng muốn đứng trên mỗi chiếc đèn lồng. Chỉ có thể nhảy giữa các đèn lồng lân cận khi độ cao của chúng khớp với nhau tại thời điểm đó. 

Con rồng có thể đổi chiều cao lấy năng lượng ở chiếc đèn lồng mà nó đang đứng. Hạ một chiếc đèn lồng sẽ mang lại một năng lượng, trong khi tăng một chiếc đèn lồng sẽ tốn một năng lượng. Nhiệm vụ là trả lời nhiều câu hỏi: đối với vị trí bắt đầu, lượng năng lượng ban đầu cần thiết nhỏ nhất là bao nhiêu và sau một số câu hỏi, chiều cao của đèn lồng có thể thay đổi. 

Đầu vào mô tả số lượng đèn lồng và hoạt động, theo sau là độ cao ban đầu. Một bản cập nhật thay đổi chiều cao của một chiếc đèn lồng. Một câu hỏi yêu cầu năng lượng ban đầu tối thiểu từ một chỉ số đèn lồng nhất định. 

Giới hạn rất lớn, lên tới$2 \cdot 10^5$đèn lồng và$2 \cdot 10^5$hoạt động. Điều này loại trừ việc quét tất cả các đèn lồng cho mỗi câu hỏi vì điều đó sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cấu trúc trong đó cả cập nhật và truy vấn đều mất thời gian logarit hoặc không đổi. 

Phần khó khăn là nhận ra rằng con rồng không thể tạo ra năng lượng tự do bằng cách liên tục hạ và nâng cùng một chiếc đèn lồng. Việc hạ thấp một chiếc đèn lồng sẽ tạo ra năng lượng, nhưng sau đó đưa chiếc đèn lồng đó trở lại độ cao cần thiết cho lần nhảy tiếp theo sẽ tiêu tốn một lượng năng lượng tương đương. Điều duy nhất quan trọng là độ cao tương đối của những chiếc đèn lồng được viếng thăm. 

Một sai lầm phổ biến là chỉ xem xét đèn lồng tiếp theo theo một hướng. Con rồng có thể cần đến thăm một chiếc đèn lồng rất cao ở phía đối diện, vì vậy mọi vị trí xuất phát đều phải tính đến chiếc đèn lồng cao nhất trong toàn hàng. 

Ví dụ: với đầu vào:```
3 1
5 1 2
1 2
```câu trả lời là:```
0
```Bắt đầu từ chiếc đèn lồng đầu tiên đã có chiều cao tối đa nên không cần năng lượng ban đầu. 

Một trường hợp khác là khi đèn xuất phát không cao nhất:```
3 1
5 1 2
1 2
```Nếu truy vấn dành cho đèn lồng 2, câu trả lời sẽ là:```
4
```Giải pháp chỉ kiểm tra các đèn lồng liền kề có thể bỏ sót chiều cao 5 ở đầu bên kia và trả về giá trị nhỏ hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ mô phỏng chuyển động của con rồng. Bắt đầu từ chiếc đèn lồng đã chọn, chúng ta có thể cố gắng di chuyển sang trái và phải, theo dõi lượng năng lượng cần thiết ở mỗi bước nhảy. Điều này hiệu quả vì quy tắc chuyển động có thể được mô phỏng chính xác. Tuy nhiên, đối với một truy vấn, nó có thể yêu cầu xem xét tất cả$n$đèn lồng. Với$q$truy vấn, trường hợp xấu nhất sẽ trở thành$O(nq)$, vượt xa phạm vi cho phép. 

Quan sát quan trọng xuất phát từ việc xem xét sự thay đổi năng lượng trong một bước nhảy. Giả sử con rồng di chuyển từ một chiếc đèn lồng có chiều cao$x$đến một chiếc đèn lồng có chiều cao lân cận$y$. Nếu như$y$cao hơn thì rồng tiêu$y-x$năng lượng. Nếu như$y$thấp hơn, rồng tăng$x-y$năng lượng. Trong cả hai trường hợp, năng lượng thay đổi chính xác:$$x-y$$Sau một chuỗi các bước nhảy, tất cả các độ cao trung gian đều bị hủy bỏ. Bắt đầu từ đèn lồng$p$và với tới một chiếc đèn lồng có chiều cao$h$, năng lượng trở thành:$$E + a_p - h$$Năng lượng thấp nhất xảy ra khi con rồng chạm tới chiếc đèn lồng cao nhất mà nó từng ghé thăm. Vì mục tiêu là đi thăm từng chiếc đèn lồng nên chiếc đèn lồng cao nhất chỉ đơn giản là chiều cao tối đa trong toàn bộ dãy đèn. 

Do đó, năng lượng ban đầu cần thiết từ vị trí$p$là:$$\max(0, \text{maximum height} - a_p)$$Thông tin động duy nhất chúng ta cần là chiều cao tối đa hiện tại. Các bản cập nhật có thể thay đổi mức tối đa đó, vì vậy chúng tôi duy trì nhiều tập hợp độ cao và truy xuất giá trị lớn nhất sau mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi truy vấn, tổng O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(log n) mỗi lần cập nhật, O(1) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi chiều cao của đèn lồng trong cấu trúc dữ liệu có thể chèn, xóa và truy xuất giá trị tối đa. Một đống tối đa với tính năng xóa lười biếng là đủ cho vấn đề này. 
2. Đối với thao tác cập nhật, hãy thay thế chiều cao cũ bằng chiều cao mới. Không thể xóa giá trị cũ trực tiếp khỏi một đống, vì vậy hãy lưu trữ chiều cao hiện tại của mỗi đèn lồng một cách riêng biệt và bỏ qua các mục nhập đã lỗi thời khi kiểm tra mức tối đa. 
3. Đối với câu hỏi tại đèn lồng$p$, trước tiên hãy xóa các mục heap lỗi thời, sau đó lấy chiều cao tối đa hiện tại. Câu trả lời là sự khác biệt giữa chiều cao tối đa này và chiều cao của đèn lồng ban đầu nếu chênh lệch là dương, nếu không thì bằng 0. 
4. Xuất giá trị tính toán. 

Tại sao nó hoạt động: trong bất kỳ bước nhảy nào, năng lượng của rồng thay đổi theo sự chênh lệch giữa chiều cao của đèn lồng hiện tại và chiều cao của đèn lồng tiếp theo. Trên một tuyến đường hoàn chỉnh, tất cả các độ cao trung gian đã ghé thăm sẽ bị loại bỏ. Điểm nguy hiểm duy nhất là tham quan chiếc đèn lồng cao nhất, bởi đó là nơi con rồng tiêu tốn nhiều năng lượng nhất so với độ cao ban đầu. Có đủ năng lượng để đạt đến độ cao đó có nghĩa là mọi chiếc đèn lồng khác cũng có thể được viếng thăm. Công thức đo chính xác lượng năng lượng còn thiếu cần thiết để làm cho chiếc đèn ban đầu cao bằng chiếc đèn lồng cao nhất. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    heap = []
    for i, x in enumerate(a):
        heapq.heappush(heap, (-x, i))

    out = []

    for _ in range(q):
        query = input().split()
        if query[0] == '1':
            p = int(query[1]) - 1
            while -heap[0][0] != a[heap[0][1]]:
                heapq.heappop(heap)
            out.append(str(max(0, -heap[0][0] - a[p])))
        else:
            p = int(query[1]) - 1
            x = int(query[2])
            a[p] = x
            heapq.heappush(heap, (-x, p))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng`a`lưu trữ chiều cao hiện tại của mỗi đèn lồng. Nó cần thiết vì các mục heap có thể trở nên cũ sau khi cập nhật. 

Vùng heap lưu trữ chiều cao âm vì vùng heap của Python là vùng heap tối thiểu. Do đó, chiều cao lớn nhất ở trên cùng là số âm nhỏ nhất. 

Trước khi trả lời một truy vấn, mã sẽ loại bỏ các mục heap không còn khớp với chiều cao hiện tại trong`a`. Việc xóa lười biếng này giúp tránh nhu cầu về cấu trúc dữ liệu phức tạp hơn trong khi vẫn giữ mọi thao tác nhanh chóng. 

Câu trả lời truy vấn sử dụng công thức dẫn xuất trực tiếp. các`max`cuộc gọi xử lý trường hợp đèn lồng bắt đầu đã có chiều cao tối đa toàn cầu, vì rồng không cần bất kỳ năng lượng ban đầu nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
3 1 3
1 1
1 2
```| Hoạt động | Chiều cao ban đầu | Chiều cao tối đa | Trả lời | 
| --- | --- | --- | --- | 
| Truy vấn 1 | 3 | 3 | 0 | 
| Truy vấn 2 | 1 | 3 | 2 | 

Chiếc đèn lồng đầu tiên đã ở độ cao cao nhất nên rồng có thể hấp thụ năng lượng từ những chiếc đèn lồng thấp hơn và di chuyển mà không cần bất kỳ năng lượng ban đầu nào. Chiếc đèn lồng thứ hai cần đủ năng lượng để đạt được độ cao 3. 

### Mẫu 2 

đầu vào:```
2 5
1000000000 0
1 2
2 2 10
1 2
2 1 10
1 2
```| Hoạt động | Độ cao | Chiều cao tối đa | Câu trả lời truy vấn | 
| --- | --- | --- | --- | 
| Đèn lồng truy vấn 2 | [1000000000, 0] | 1000000000 | 1000000000 | 
| Cập nhật đèn lồng 2 | [1000000000, 10] | 1000000000 | | 
| Đèn lồng truy vấn 2 | [1000000000, 10] | 1000000000 | 999999990 | 
| Cập nhật đèn lồng 1 | [10, 10] | 10 | | 
| Đèn lồng truy vấn 2 | [10, 10] | 10 | 0 | 

Dấu vết cho thấy chỉ có chiều cao tối đa hiện tại mới quan trọng. Những thay đổi đối với các đèn lồng khác chỉ quan trọng khi chúng thay đổi ở mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi lần cập nhật, O(1) khấu hao cho mỗi truy vấn | Mỗi bản cập nhật chèn một mục nhập heap và các mục nhập cũ sẽ bị xóa tối đa một lần | 
| Không gian | O(n + q) | Vùng heap có thể chứa các mục cũ từ các bản cập nhật | 

Những ràng buộc cho phép$2 \cdot 10^5$hoạt động, do đó cần có cấu trúc cập nhật logarit. Giải pháp heap vẫn nằm trong giới hạn vì mỗi phần tử heap được đẩy một lần và bị xóa một lần. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    heap = []
    for i, x in enumerate(a):
        heapq.heappush(heap, (-x, i))

    ans = []

    for _ in range(q):
        query = input().split()
        if query[0] == "1":
            p = int(query[1]) - 1
            while -heap[0][0] != a[heap[0][1]]:
                heapq.heappop(heap)
            ans.append(str(max(0, -heap[0][0] - a[p])))
        else:
            p = int(query[1]) - 1
            x = int(query[2])
            a[p] = x
            heapq.heappush(heap, (-x, p))

    return "\n".join(ans)

assert solve("""3 2
3 1 3
1 1
1 2
""") == """0
2""", "sample 1"

assert solve("""2 5
1000000000 0
1 2
2 2 10
1 2
2 1 10
1 2
""") == """1000000000
999999990
0""", "sample 2"

assert solve("""1 3
7
1 1
2 1 0
1 1
""") == """0
0""", "single lantern"

assert solve("""4 3
1 1 1 1
1 3
2 4 100
1 1
""") == """0
99""", "new maximum after update"

assert solve("""5 4
0 5 2 9 1
1 1
1 4
2 2 20
1 3
""") == """9
0
18""", "maximum movement and update"

assert solve("""3 2
5 5 5
1 2
1 3
""") == """0
0""", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đèn lồng đơn |`0`| Hộp có kích thước tối thiểu không cần di chuyển | 
| Cập nhật lên mức tối đa mới |`0`,`99`| Xử lý đúng các thay đổi tối đa | 
| Độ cao hỗn hợp |`9`,`0`,`18`| Truy vấn từ đèn lồng thấp và cao sau khi cập nhật | 
| Tất cả các giá trị bằng nhau |`0`,`0`| Trường hợp không năng lượng | 

## Vỏ cạnh 

Khi chỉ có một chiếc đèn lồng thì con rồng đã ghé thăm từng chiếc đèn lồng. Đối với đầu vào:```
1 1
7
1 1
```chiều cao tối đa và chiều cao bắt đầu đều là 7, vì vậy câu trả lời là:```
0
```Thuật toán xử lý vấn đề này vì năng lượng cần thiết là chênh lệch dương giữa chiều cao tối đa và chiều cao ban đầu. 

Khi đèn xuất phát đã cao nhất thì không cần năng lượng ban đầu. Vì:```
3 1
5 2 4
1 1
```chiều cao tối đa là 5 và chiều cao ban đầu cũng là 5. Đáp án là:```
0
```Rồng có thể thu hoặc tiêu hao năng lượng trong khi điều chỉnh các đèn lồng khác. 

Khi bản cập nhật chiều cao loại bỏ giá trị tối đa trước đó, các giá trị heap cũ không được ảnh hưởng đến câu trả lời. Vì:```
2 3
100 0
2 1 10
1 2
1 1
```sau khi cập nhật chiều cao là`[10, 0]`. Giá trị cũ`100`vẫn tạm thời ở trong heap, nhưng bước xóa lười sẽ loại bỏ nó trước khi trả lời. Câu trả lời là:```
10
0
```vì mức tối đa hiện tại là 10, không phải giá trị lỗi thời 100.
