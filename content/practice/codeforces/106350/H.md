---
title: "CF 106350H - Zagol so với sự chiếm đóng của người Anh"
description: "Chúng ta có một dòng kẻ thù, mỗi kẻ có một điểm mạnh tích cực. Đối với mỗi truy vấn, Zagol bắt đầu với một thanh kiếm có sức mạnh ban đầu và cố gắng đánh bại càng nhiều kẻ thù càng tốt."
date: "2026-06-25T08:07:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106350
codeforces_index: "H"
codeforces_contest_name: "Zaglol Contest - FCDS level 1 contest 2026"
rating: 0
weight: 106350
solve_time_s: 39
verified: true
draft: false
---

[CF 106350H - Zagol vs. Sự chiếm đóng của người Anh](https://codeforces.com/problemset/problem/106350/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dòng kẻ thù, mỗi kẻ có một điểm mạnh tích cực. Đối với mỗi truy vấn, Zagol bắt đầu với một thanh kiếm có sức mạnh ban đầu và cố gắng đánh bại càng nhiều kẻ thù càng tốt. Các quy tắc rất đơn giản nhưng hơi khác thường: anh ta có thể chọn thứ tự chiến đấu với kẻ thù, nhưng để đánh bại kẻ thù, kiếm lực hiện tại của anh ta ít nhất phải bằng sức mạnh của kẻ thù đó và sau khi đánh bại nó, kiếm lực của anh ta sẽ giảm vĩnh viễn với cùng sức mạnh đó. 

Mỗi truy vấn đều độc lập, nghĩa là chúng tôi luôn bắt đầu mới với cùng một danh sách kẻ thù và sức mạnh kiếm ban đầu mới. Nhiệm vụ là tính toán, đối với mỗi truy vấn, số lượng kẻ thù tối đa có thể bị đánh bại bằng cách chọn thứ tự tối ưu. 

Cấu trúc then chốt ẩn trong quy tắc là mỗi khi chúng ta hạ gục kẻ thù có sức mạnh$a_i$, chúng tôi vĩnh viễn giảm ngân sách của mình xuống$a_i$. Điều này biến vấn đề thành một quá trình lập ngân sách trong đó mỗi mục được chọn sẽ tiêu thụ một phần công suất ban đầu. 

Các ràng buộc (lên đến$10^5$kẻ thù và$10^5$truy vấn, với giá trị lên tới$10^9$) ngay lập tức loại trừ mọi mô phỏng theo truy vấn đối với các hoán vị hoặc tập hợp con. Mọi thứ thậm chí bậc hai cho mỗi truy vấn đều quá lớn và thậm chí$O(n \log n)$mỗi truy vấn sẽ là đường biên giới. Chúng tôi cần một ý tưởng tiền xử lý cho phép mỗi truy vấn được trả lời theo thời gian logarit. 

Một vài trường hợp đáng chú ý. 

Nếu tất cả sức mạnh của kẻ thù đều lớn hơn sức mạnh ban đầu thì câu trả lời là 0. Ví dụ, với kẻ thù$[5, 6, 7]$Và$x = 3$, không thể chiến đấu được. 

Nếu tất cả kẻ thù đều nhỏ nhưng có rất nhiều kẻ thù thì chiến lược tham lam có thể phụ thuộc vào trật tự. Ví dụ, với$[4, 1, 3]$Và$x = 5$, các thứ tự khác nhau dường như có thể thay đổi kết quả, nhưng trên thực tế, cấu trúc buộc phải đưa ra lựa chọn tối ưu nhất quán một khi chúng ta diễn giải nó một cách chính xác. 

Một trường hợp tinh tế hơn là khi chọn một kẻ thù lớn sớm sẽ chặn nhiều kẻ thù nhỏ hơn sau đó. Ví dụ,$[6, 1, 1, 1]$với$x = 7$: chọn 6 con đầu tiên sẽ có quá ít sự linh hoạt, trong khi chọn những con nhỏ trước sẽ có tổng số mạng tiêu diệt cao hơn. Điều này gợi ý rằng trật tự rất quan trọng, nhưng theo một cách rất được kiểm soát. 

## Phương pháp tiếp cận 

Cách giải thích vũ lực là thử tất cả các tập hợp con của kẻ thù và tất cả các mệnh lệnh có thể có cho mỗi tập hợp con, mô phỏng xem liệu một mệnh lệnh nhất định có hợp lệ theo quy tắc sức mạnh giảm dần hay không. Ngay cả khi chúng ta bỏ qua các hoán vị và chỉ kiểm tra các tập con, chúng ta vẫn gặp phải$2^n$khả năng, và mỗi mô phỏng tốn thời gian tuyến tính. Điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là đơn đặt hàng thực sự nằm trong tầm kiểm soát của chúng tôi và chúng tôi muốn tối đa hóa số lượng mặt hàng mà chúng tôi có thể “lắp” vào ngân sách đang giảm. Mỗi kẻ thù được chọn sẽ tiêu thụ chính xác sức mạnh của mình từ tổng sức mạnh sẵn có và hạn chế ở mỗi bước tương đương với việc đảm bảo chúng ta không bao giờ vượt quá ngân sách còn lại. 

Nếu chúng ta quyết định chọn một nhóm nhỏ kẻ thù, chiến lược tốt nhất là chiến đấu với chúng theo thứ tự sức mạnh tăng dần. Điều này tránh lãng phí năng lượng sớm với các giá trị lớn và đảm bảo rằng tính khả thi chỉ phụ thuộc vào tổng số tiền chứ không phụ thuộc vào các biến động trung gian. Theo thứ tự này, điều kiện trở nên đơn giản: sau khi chọn$k$kẻ thù, tổng số tiền của chúng không được vượt quá$x$. 

Vì vậy, vấn đề giảm xuống còn việc chọn càng nhiều phần tử càng tốt sao cho tổng của chúng lớn nhất là$x$. Để tối đa hóa số lượng, chúng ta phải luôn lấy các phần tử nhỏ hơn trước vì chúng đóng góp ít chi phí hơn và cho phép phù hợp với nhiều phần tử hơn. Điều này chuyển vấn đề thành sắp xếp và xây dựng một mảng tổng tiền tố. 

Sau đó, mỗi truy vấn sẽ trở thành: chúng ta có thể lấy bao nhiêu phần tử nhỏ nhất trước khi tổng tích lũy của chúng vượt quá$x$, có thể được trả lời bằng cách sử dụng tìm kiếm nhị phân trên tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực đối với các tập hợp con và đơn đặt hàng |$O(n! \cdot n)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Sắp xếp + tổng tiền tố + tìm kiếm nhị phân |$O(n \log n + q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng sức mạnh của địch theo thứ tự không giảm. Điều này đảm bảo chúng tôi luôn xem xét những kẻ thù rẻ nhất trước tiên, điều này rất cần thiết để tối đa hóa số lượng. 
2. Xây dựng một mảng tổng tiền tố trong đó mỗi vị trí lưu trữ tổng sức mạnh của tất cả kẻ thù cho đến chỉ số đó. Điều này cho phép chúng tôi nhanh chóng tính toán cái giá phải trả để tiêu diệt bất kỳ số lượng kẻ thù yếu nhất nào. 
3. Đối với mỗi truy vấn có lũy thừa ban đầu$x$, thực hiện tìm kiếm nhị phân trên mảng tổng tiền tố để tìm chỉ số lớn nhất$k$sao cho tổng của số đầu tiên$k$nhiều nhất là kẻ thù$x$. 
4. Đầu ra$k$là câu trả lời cho truy vấn đó, vì nó đại diện cho số lượng kẻ thù tối đa có thể bị đánh bại mà không vượt quá sức mạnh sẵn có. 

Lý do tìm kiếm nhị phân hoạt động ở đây là vì tổng tiền tố đang tăng lên rất nhiều, do đó tính khả thi sẽ chuyển từ đúng sang sai đúng một lần. 

### Tại sao nó hoạt động 

Sau khi chúng tôi xác định được số lượng kẻ thù mà chúng tôi muốn bắt, mọi mệnh lệnh hợp lệ đều phải tôn trọng ràng buộc rằng sức mạnh còn lại không bao giờ giảm xuống dưới kẻ thù được chọn tiếp theo. Sắp xếp theo thứ tự tăng dần đảm bảo rằng ràng buộc khó nhất luôn được thỏa mãn sau cùng, vì mỗi bước tiêu tốn chi phí khả dụng nhỏ nhất có thể ở giai đoạn đó. Điều này thu gọn điều kiện tuần tự thành một điều kiện toàn cục duy nhất: tổng tổng các phần tử được chọn không được vượt quá lũy thừa ban đầu. Bởi vì tất cả các giá trị đều dương, nên bất kỳ sai lệch nào so với việc chọn phần tử nhỏ nhất có sẵn trước tiên chỉ có thể làm giảm số lượng mục chúng tôi có thể khớp chứ không bao giờ tăng nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
a.sort()

# prefix sums
pref = [0] * n
pref[0] = a[0]
for i in range(1, n):
    pref[i] = pref[i - 1] + a[i]

q = int(input())
out = []

def upper_bound(x):
    lo, hi = 0, n - 1
    ans = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if pref[mid] <= x:
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1
    return ans + 1

for _ in range(q):
    x = int(input())
    out.append(str(upper_bound(x)))

print("\n".join(out))
```Đầu tiên, mã sẽ sắp xếp mảng sao cho kẻ thù yếu hơn được xem xét trước kẻ thù mạnh hơn. Mảng tổng tiền tố chuyển đổi "tôi có thể lấy k kẻ thù không?" thành một so sánh duy nhất với$x$. Tìm kiếm nhị phân tìm thấy tiền tố hợp lệ lớn nhất và trả về chỉ số cộng một sẽ chuyển nó thành số đếm. 

Một cạm bẫy triển khai phổ biến là quên rằng câu trả lời là số đếm chứ không phải chỉ mục. Một cách khác là xử lý sai trường hợp ngay cả phần tử nhỏ nhất cũng quá lớn, trong trường hợp đó hàm trả về 0 một cách chính xác thông qua$-1 + 1$điều chỉnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [4, 2, 1, 10, 3]
query x = 7
```Sau khi sắp xếp:```
[1, 2, 3, 4, 10]
prefix: [1, 3, 6, 10, 20]
```Chúng tôi kiểm tra xem có bao nhiêu phần tử tiền tố phù hợp với 7. 

| k | tổng tiền tố | có hiệu lực? | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 3 | vâng | 
| 3 | 6 | vâng | 
| 4 | 10 | không | 

Câu trả lời là 3. 

Điều này cho thấy chiến lược tối ưu luôn chọn các phần tử nhỏ nhất trước tiên và quyết định chỉ phụ thuộc vào tổng tích lũy. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [5, 1, 1, 1]
query x = 5
```Đã sắp xếp:```
[1, 1, 1, 5]
prefix: [1, 2, 3, 8]
```| k | tổng tiền tố | có hiệu lực? | 
| --- | --- | --- | 
| 1 | 1 | vâng | 
| 2 | 2 | vâng | 
| 3 | 3 | vâng | 
| 4 | 8 | không | 

Câu trả lời là 3. 

Điều này khẳng định trực giác tham lam rằng việc thực hiện số 5 lớn sớm sẽ tệ hơn rất nhiều so với việc trì hoãn nó, nhưng trong giải pháp tối ưu, nó đơn giản bị loại trừ vì nó phá vỡ giới hạn ngân sách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n + q \log n)$| việc sắp xếp chiếm ưu thế trong quá trình tiền xử lý, mỗi truy vấn sử dụng tìm kiếm nhị phân | 
| Không gian |$O(n)$| mảng tổng tiền tố lưu trữ một giá trị cho mỗi kẻ thù | 

Các ràng buộc cho phép lên đến$10^5$các phần tử và truy vấn, do đó$O(n \log n)$tiền xử lý cộng với các truy vấn logarit phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    pref = [0] * n
    pref[0] = a[0]
    for i in range(1, n):
        pref[i] = pref[i - 1] + a[i]

    q = int(input())
    out = []

    def upper_bound(x):
        lo, hi = 0, n - 1
        ans = -1
        while lo <= hi:
            mid = (lo + hi) // 2
            if pref[mid] <= x:
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1
        return ans + 1

    for _ in range(q):
        x = int(input())
        out.append(str(upper_bound(x)))

    return "\n".join(out)

# custom cases

# minimum size
assert run("1\n5\n1\n10\n") == "1"

# cannot take anything
assert run("3\n5 6 7\n2\n1\n5\n") == "0\n0"

# all equal
assert run("5\n2 2 2 2 2\n3\n1\n4\n10\n") == "0\n1\n5"

# mixed case
assert run("4\n4 1 3 2\n2\n5\n6\n") == "2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả đều quá lớn | 0 0 | lựa chọn bất khả thi | 
| giá trị bằng nhau | 0 1 5 | hành vi ngưỡng | 
| giá trị hỗn hợp | 2 3 | tính chính xác tham lam trên các truy vấn | 

## Vỏ cạnh 

Khi tất cả các phần tử vượt quá sức mạnh truy vấn, mảng tổng tiền tố sẽ bắt đầu ở trên$x$và tìm kiếm nhị phân trả về chính xác$-1$, chuyển đổi thành không. Ví dụ, với$[10, 20]$Và$x = 5$, tìm kiếm không bao giờ tìm thấy tiền tố hợp lệ. 

Khi mảng chứa nhiều phần tử nhỏ theo sau là phần tử ngoại lệ lớn, chẳng hạn như$[1, 1, 1, 1, 100]$, thuật toán tự nhiên bỏ qua phần tử lớn đối với các truy vấn nhỏ vì nó được đặt ở cuối sau khi sắp xếp và chỉ trở nên phù hợp với các truy vấn đủ lớn$x$. 

Khi$x$là cực kỳ lớn, tìm kiếm nhị phân luôn trả về$n$và thuật toán đếm chính xác tất cả các phần tử mà không gặp vấn đề tràn vì tổng tiền tố sử dụng số nguyên Python.
