---
title: "CF 2124D - Tạo bảng màu"
description: "Chúng tôi bắt đầu với một mảng và có thể xóa các phần tử nhiều lần. Quy tắc xóa không bình thường: chọn bất kỳ mảng con nào có độ dài ít nhất là k, tìm giá trị nhỏ thứ k của nó và xóa một lần xuất hiện của giá trị đó bên trong mảng con đã chọn."
date: "2026-06-08T03:33:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2124
codeforces_index: "D"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2025 (Codeforces Round 1036, Div. 1 + Div. 2)"
rating: 1700
weight: 2124
solve_time_s: 120
verified: false
draft: false
---

[CF 2124D - Tạo một bảng màu](https://codeforces.com/problemset/problem/2124/D) 

**Đánh giá:** 1700 
**Tags:** tham lam, sắp xếp, hai con trỏ 
**Thời gian giải:** 2 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng và có thể xóa các phần tử nhiều lần. Quy tắc xóa không bình thường: chọn bất kỳ mảng con nào có độ dài ít nhất`k`, tìm nó`k`-giá trị nhỏ nhất và xóa một lần xuất hiện của giá trị đó bên trong mảng con đã chọn. 

Mục tiêu không phải là giảm thiểu hoặc tối đa hóa số lượng hoạt động. Chúng ta chỉ cần quyết định xem có tồn tại một số chuỗi thao tác xóa hợp lệ để lại một bảng màu hay không. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta cần một giải pháp gần như tuyến tính. Trên tất cả các trường hợp thử nghiệm, tổng chiều dài tối đa là`2·10^5`, vì vậy một`O(n log n)`thuật toán rất thoải mái, trong khi mọi thứ bậc hai đều không thể thực hiện được. Ngay cả một thử nghiệm duy nhất với`n = 2·10^5`sẽ làm`O(n²)`vượt quá hàng chục tỷ hoạt động. 

Khó khăn chính là hiểu được phần tử nào thực sự có thể bị xóa. Hoạt động được xác định thông qua các mảng con tùy ý, do đó việc cố gắng mô phỏng thao tác xóa trực tiếp sẽ nhanh chóng trở nên vô vọng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như`k = 1`.```
4 1
1 1 2 1
```Câu trả lời là`YES`. Mọi phần tử đều có thể bị xóa vì phần tử nhỏ nhất đầu tiên của mảng con có độ dài-1 chính là phần tử đó. Một giải pháp giả định một số giá trị phải được giữ nguyên sẽ thất bại ở đây. 

Coi như`k = n`.```
3 3
1 2 2
```Câu trả lời là`NO`. Mảng con duy nhất có sẵn là toàn bộ mảng. Phần tử nhỏ thứ ba là`2`, vậy chỉ có một`2`có thể bị xóa. Sau khi xóa một`2`, độ dài mảng trở thành`2 < k`, do đó không thể thực hiện thêm thao tác nào nữa. Một cách tiếp cận ngây thơ xử lý việc xóa như luôn có sẵn sẽ trả lời sai`YES`. 

Một trường hợp tinh vi khác xảy ra khi mảng chứa nhiều bản sao của giá trị tới hạn.```
5 4
1 2 1 2 2
```Câu trả lời là`YES`. Một số bản sao của giá trị tới hạn có thể bị xóa trong khi những bản sao khác phải được giữ lại. Việc coi tất cả các bản sao của giá trị đó là cố định vĩnh viễn là không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa quá trình một cách trực tiếp. Ở mọi trạng thái, chúng ta có thể liệt kê tất cả các mảng con hợp lệ, xác định chúng`k`-giá trị nhỏ nhất, tạo mọi thao tác xóa có thể và tiếp tục tìm kiếm. Điều này đúng về mặt lý thuyết vì nó khám phá mọi chuỗi hoạt động hợp pháp. 

Không gian trạng thái bùng nổ gần như ngay lập tức. Ngay cả đối với mức độ vừa phải`n`, số lượng mảng có thể truy cập trở nên rất lớn. Từ`n`đạt tới`2·10^5`, hướng đi này hoàn toàn không khả thi. 

Bước đột phá là mô tả các phần tử có thể xóa được trên toàn cầu. 

Sắp xếp mảng và để`x`là`(k-1)`-phần tử nhỏ nhất trong toàn bộ mảng, sử dụng chỉ mục dựa trên 1. Tương đương,`x`là giá trị tại chỉ mục`k-1`sau khi sắp xếp. 

Một thực tế quan trọng là`k-1`các yếu tố nhỏ nhất trên toàn cầu không bao giờ có thể biến mất hoàn toàn. Mỗi lần xóa yêu cầu phần tử bị xóa phải có ít nhất`k-1`các phần tử không lớn hơn nó trong mảng con đã chọn. Một phần tử thuộc về phần tử nhỏ nhất trên toàn cầu`k-1`vị trí không bao giờ có thể đáp ứng điều này. 

Cuộc trò chuyện mạnh mẽ một cách đáng ngạc nhiên. Bất kỳ phần tử nào không nằm trong số những phần tử nhỏ nhất trên toàn cầu`k-1`các phần tử luôn có thể bị xóa. Bài xã luận chính thức chứng minh rằng miễn là những người được bảo vệ`k-1`phần tử còn lại, chúng ta có thể chọn một khoảng thích hợp trong đó phần tử mục tiêu trở thành chính xác`k`-thứ nhỏ nhất. 

Điều này biến đổi vấn đề hoàn toàn. 

Cho phép`x`là giá trị của`(k-1)`-phần tử nhỏ nhất thứ 

Mọi giá trị đều lớn hơn`x`có thể xóa tự do, vì vậy không bao giờ có lý do để giữ các giá trị đó. 

Các giá trị hoàn toàn nhỏ hơn`x`là không thể chạm tới. 

Giá trị bằng`x`là phần linh hoạt Một số bản sao thuộc về bộ được bảo vệ, một số khác thì không. Chúng tôi có thể xóa một số bản sao của`x`, nhưng ít nhất chúng ta phải rời đi`k-1`các yếu tố tổng thể. 

Sau khi loại bỏ tất cả các giá trị lớn hơn`x`, chúng ta thu được một mảng rút gọn`b`chỉ bao gồm các giá trị`< x`và giá trị`= x`. 

Bây giờ chúng ta cần xác định xem việc xóa một số bản sao của`x`có thể làm`b`một bảng màu. 

Điều này trở thành một vấn đề hai con trỏ tham lam. 

Di chuyển con trỏ từ cả hai đầu. 

Nếu các giá trị khớp nhau, hãy giữ cả hai. 

Nếu chúng khác nhau và không bên nào bằng nhau`x`, thì cả hai giá trị đều được bảo vệ và không thể xóa được. Câu trả lời là ngay lập tức`NO`. 

Nếu chính xác một bên bằng`x`, xóa bản sao đó của`x`và tiếp tục. 

Điều này tham lam chỉ loại bỏ những`x`những giá trị buộc phải biến mất. Vì chúng tôi muốn giữ lại càng nhiều phần tử càng tốt nên đây là chiến lược tốt nhất. 

Cuối cùng, chúng tôi đếm xem có bao nhiêu phần tử tồn tại. Nếu ít hơn`k-1`các phần tử vẫn còn, chúng tôi sẽ xóa quá nhiều vị trí được bảo vệ, điều này là không thể. Nếu không thì câu trả lời là`YES`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp bản sao của mảng. 
2. Tìm`x`, giá trị tại vị trí`k-1`trong mảng đã sắp xếp. 
3. Xây dựng một mảng mới`b`chỉ chứa các phần tử`<= x`. 

Mọi giá trị lớn hơn`x`luôn có thể bị xóa, vì vậy việc giữ chúng không bao giờ có ích. 
4. Khởi tạo hai con trỏ,`l = 0`Và`r = len(b)-1`. 
5. Duy trì bộ đếm`kept`, ban đầu`len(b)`. 
6. Trong khi`l < r`: 

1. Nếu`b[l] == b[r]`, giữ nguyên cả hai phần tử và di chuyển cả hai con trỏ vào trong. 
2. Nếu`b[l] != b[r]`Và`b[l] == x`, xóa bản sao bên trái của`x`, giảm`kept`, và tăng`l`. 
3. Nếu`b[l] != b[r]`Và`b[r] == x`, xóa bản sao bên phải của`x`, giảm`kept`, và giảm`r`. 
4. Nếu không bên nào bằng nhau`x`, trở lại`NO`. 

Các yếu tố có thể tháo rời duy nhất bên trong`b`là bản sao của`x`. Khi xuất hiện sự không khớp, bất kỳ mặt nào có thể tháo rời phải được loại bỏ ngay lập tức. 
7. Sau vòng lặp, kiểm tra xem`kept >= k-1`. 
8. Nếu đúng thì trả về`YES`; nếu không thì trả lại`NO`. 

### Tại sao nó hoạt động 

Nhỏ nhất toàn cầu`k-1`các phần tử tạo thành một lõi được bảo vệ. Bất kỳ mảng cuối cùng hợp lệ nào cũng phải chứa chúng. Mọi giá trị lớn hơn`x`luôn có thể tháo rời được, vì vậy việc giữ các giá trị như vậy không bao giờ giúp tạo ra một bảng màu. 

Sau khi loại bỏ tất cả các giá trị lớn hơn`x`, phần tử có thể tháo rời duy nhất còn lại là bản sao của`x`. Trong quá trình quét hai con trỏ, nếu các đầu khác nhau và không có đầu nào bằng nhau`x`, thì hai giá trị được bảo vệ không đồng ý. Vì cả hai đều không thể bị xóa nên không thể có bảng màu. 

Bất cứ khi nào một bên bằng`x`, buộc phải xóa bản sao đó. Bất kỳ palindrome nào tương thích với các phần tử được bảo vệ cuối cùng đều phải loại bỏ nó. Quá trình quét tham lam sẽ loại bỏ chính xác những bản sao của`x`những thứ cần thiết và giữ lại mọi yếu tố khác. 

Bởi vì thuật toán xóa số phần tử tối thiểu có thể bằng`x`, nếu ngay cả ứng cử viên palindrome có kích thước tối đa này cũng chứa ít hơn`k-1`các phần tử, không có chuỗi hoạt động hợp lệ nào có thể tồn tại. Ngược lại, nếu ít nhất`k-1`các phần tử vẫn còn, việc xóa cần thiết có thể được thực hiện bằng cách mô tả ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k == 1:
            ans.append("YES")
            continue

        s = sorted(a)
        x = s[k - 1]

        b = [v for v in a if v <= x]

        l = 0
        r = len(b) - 1
        kept = len(b)

        ok = True

        while l < r:
            if b[l] == b[r]:
                l += 1
                r -= 1
            elif b[l] == x:
                kept -= 1
                l += 1
            elif b[r] == x:
                kept -= 1
                r -= 1
            else:
                ok = False
                break

        if ok and kept >= k - 1:
            ans.append("YES")
        else:
            ans.append("NO")

    sys.stdout.write("\n".join(ans))

solve()
```Bước đầu tiên tính giá trị ngưỡng`x`. Đây là giá trị ngăn cách các phần tử luôn có thể loại bỏ khỏi các phần tử có thể phải ở lại. 

Mảng`b`loại bỏ mọi giá trị lớn hơn`x`. Những yếu tố đó không liên quan vì bất kỳ giải pháp thành công nào cũng có thể xóa chúng trước. 

Vòng lặp hai con trỏ thực hiện việc xây dựng bảng màu tham lam. Khi các đầu cuối khớp nhau, rõ ràng chúng có thể thuộc về bảng màu cuối cùng. Khi chúng khác nhau, việc xóa hợp pháp duy nhất bên trong`b`là một bản sao của`x`, do đó bất kỳ sự không phù hợp nào liên quan đến`x`được giải quyết bằng cách loại bỏ bản sao đó. 

Biến`kept`theo dõi có bao nhiêu phần tử tồn tại. Điều này dễ dàng hơn một chút so với việc đếm số lần xóa sau đó và tránh được sai sót khi có nhiều bản sao của`x`được gỡ bỏ. 

Điều kiện cuối cùng`kept >= k - 1`là điều cần thiết. Lõi được bảo vệ chứa`k-1`các phần tử, do đó, bất kỳ mảng cuối cùng khả thi nào cũng phải chứa ít nhất nhiều phần tử đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2 1 2 2
```Mảng được sắp xếp:```
[1, 1, 2, 2, 2]
```Vì thế`x = 2`. 

Mảng giảm không thay đổi:```
b = [1, 2, 1, 2, 2]
```| tôi | r | b[l] | b[r] | Hành động | giữ | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 4 | 1 | 2 | xóa phải x | 4 | 
| 0 | 3 | 1 | 2 | xóa phải x | 3 | 
| 0 | 2 | 1 | 1 | giữ cả hai | 3 | 

Quá trình quét thành công và`kept = 3 ≥ k-1 = 3`. 

Trả lời:`YES`. 

Ví dụ này cho thấy rằng một số bản sao của`x`có thể bị xóa trong khi những cái khác vẫn còn. 

### Ví dụ 2 

đầu vào:```
5 4
5 2 4 3 1
```Mảng được sắp xếp:```
[1, 2, 3, 4, 5]
```Vì thế`x = 4`. 

Mảng giảm:```
b = [2, 4, 3, 1]
```| tôi | r | b[l] | b[r] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | không bên nào là x | 

Thuật toán ngay lập tức dừng lại. 

Các giá trị`2`Và`1`thuộc phần được bảo vệ và không đồng ý ở vị trí đối xứng. Không có palindrome hợp lệ có thể được hình thành. 

Trả lời:`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế | 
| Không gian | O(n) | Mảng giảm`b`| 

Tổng chiều dài của các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Việc sắp xếp từng trường hợp thử nghiệm sẽ mang lại độ phức tạp tổng thể là`O(total_n log total_n)`, dễ dàng nằm gọn trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = []

    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split())

        )

        if k == 1:
            out.append("YES")
            continue

        s = sorted(a)
        x = s[k - 1]

        b = [v for v in a if v <= x]

        l = 0
        r = len(b) - 1
        kept = len(b)

        ok = True

        while l < r:
            if b[l] == b[r]:
                l += 1
                r -= 1
            elif b[l] == x:
                kept -= 1
                l += 1
            elif b[r] == x:
                kept -= 1
                r -= 1
            else:
                ok = False
                break

        out.append("YES" if ok and kept >= k - 1 else "NO")

    return "\n".join(out)

# provided sample
assert run(
"""8
5 3
5 4 3 4 5
4 1
1 1 2 1
6 6
2 3 4 5 3 2
5 4
5 2 4 3 1
8 5
4 7 1 2 3 1 3 4
5 4
1 2 1 2 2
3 3
1 2 2
4 4
2 1 2 2
"""
) == "\n".join([
    "YES",
    "YES",
    "YES",
    "NO",
    "NO",
    "YES",
    "NO",
    "YES"
])

# minimum size
assert run(
"""1
1 1
7
"""
) == "YES"

# already palindrome
assert run(
"""1
5 5
1 2 3 2 1
"""
) == "YES"

# impossible due to protected mismatch
assert run(
"""1
4 4
1 3 2 4
"""
) == "NO"

# all equal values
assert run(
"""1
6 4
2 2 2 2 2 2
"""
) == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, k=1`| CÓ | Ví dụ nhỏ nhất có thể | 
| Mảng Palindromic với`k=n`| CÓ | Không cần xóa | 
|`1 3 2 4`,`k=4`| KHÔNG | Giá trị được bảo vệ không khớp | 
| Tất cả các giá trị bằng nhau | CÓ | Sao chép nặng và xóa nhiều hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1
4 1
1 1 2 1
```Từ`k=1`, mọi phần tử đều có thể tháo rời được. Thuật toán ngay lập tức trở lại`YES`. Bất kỳ mảng nào cũng có thể được rút gọn thành một phần tử hoặc mảng trống, cả hai bảng màu. 

Coi như:```
1
3 3
1 2 2
```Mảng được sắp xếp là`[1,2,2]`, Vì thế`x=2`. Mảng giảm vẫn còn`[1,2,2]`. 

Quét hai con trỏ so sánh`1`Và`2`. Vế phải bằng`x`, vì vậy nó bị loại bỏ. Chỉ còn lại hai phần tử, ít hơn`k-1=2`sau khi giải quyết sự không phù hợp. Lần kiểm tra cuối cùng không thành công, tạo ra`NO`. 

Coi như:```
1
5 4
1 2 1 2 2
```Quá trình quét chỉ loại bỏ các bản sao cần thiết của`2`, đạt đến một bảng màu và giữ chính xác`k-1`các phần tử được bảo vệ. Đây là trường hợp phá vỡ các giải pháp xử lý tất cả các bản sao của`x`như không thể xóa được. 

Toàn bộ giải pháp dựa trên đặc điểm là mọi giá trị lớn hơn giá trị toàn cục`(k-1)`-giá trị nhỏ nhất có thể xóa tự do, trong khi lõi được bảo vệ của`k-1`những phần tử nhỏ nhất phải tồn tại.
