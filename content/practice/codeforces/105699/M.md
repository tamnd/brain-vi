---
title: "CF 105699M - Meta"
description: "Chúng tôi được giao một tập hợp nhỏ các vấn đề trong cuộc thi lập trình và với mỗi vấn đề, chúng tôi biết mỗi người trong số ba đồng đội sẽ cần thực hiện nó trong bao lâu nếu họ là người được giao nhiệm vụ đó. Giá trị -1 có nghĩa là một đồng đội cụ thể không thể thực hiện được vấn đề đó."
date: "2026-06-22T04:54:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105699
codeforces_index: "M"
codeforces_contest_name: "OCPC 2024 Winter, Day 8: Borys Minaiev Contest 1 (The 3rd Universal Cup. Stage 27: London)"
rating: 0
weight: 105699
solve_time_s: 45
verified: true
draft: false
---

[CF 105699M - Meta](https://codeforces.com/problemset/problem/105699/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một tập hợp nhỏ các vấn đề trong cuộc thi lập trình và với mỗi vấn đề, chúng tôi biết mỗi người trong số ba đồng đội sẽ cần thực hiện nó trong bao lâu nếu họ là người được giao nhiệm vụ đó. Một giá trị của`-1`có nghĩa là một đồng đội cụ thể không thể thực hiện được vấn đề đó. 

Nhóm chỉ có một máy tính dùng chung nên không thể làm việc song song. Họ cũng có thời lượng cuộc thi cố định là 300 phút. Mỗi bài toán nếu được chọn sẽ được giao cho đúng một đồng đội và thời gian đóng góp chính là thời gian thực hiện tương ứng của đồng đội đó cho bài toán đó. Mục tiêu là chọn ra càng nhiều vấn đề riêng biệt càng tốt sao cho mỗi vấn đề được chọn sẽ được giao cho một đồng đội có thể giải quyết nó và tổng thời gian thực hiện đã chọn không vượt quá 300. 

Cấu trúc của đầu vào về cơ bản là một tập hợp tối đa 14 mặt hàng, mỗi mặt hàng có giá gấp ba lần trên ba “ba lô”, trong đó mỗi mặt hàng có thể được lấy với một trong các chi phí hợp lệ hoặc được bỏ qua hoàn toàn. Chúng tôi không được yêu cầu tối đa hóa tổng giá trị mà thay vào đó, chúng tôi phải tối đa hóa số lượng mục được chọn trong một ngân sách thời gian toàn cầu. 

Ràng buộc`n ≤ 14`là tín hiệu quan trọng. Với tối đa 14 bài toán, bất kỳ giải pháp nào khám phá các tập hợp con của bài toán đều khả thi, vì`2^14 = 16384`, đủ nhỏ để cho phép kiểm tra rõ ràng mọi tập hợp con. Chiều bổ sung của ba lựa chọn cho mỗi mục không làm thay đổi tính khả thi đó vì nó có thể được giải quyết trong mỗi tập hợp con bằng một quy tắc quyết định đơn giản. 

Trường hợp cạnh tinh tế duy nhất đến từ`-1`các giá trị. Một cách giải thích ngây thơ có thể coi`-1`bằng 0 hoặc bỏ qua tính khả thi không chính xác, điều này sẽ cho phép thực hiện các nhiệm vụ không thể thực hiện được. 

Ví dụ: nếu một vấn đề là:```
A -1 20 10
```thì việc giao nó cho đồng đội đầu tiên là điều không thể. Một cách tiếp cận tham lam sai lầm vẫn có thể cố gắng “chọn thời gian tối thiểu” và coi nó là 0, dẫn đến việc đếm quá mức các tập hợp khả thi. Cách xử lý đúng là tùy chọn này bị loại trừ hoàn toàn. 

Một trường hợp cạnh khác là khi cả ba giá trị đều`-1`. Trong trường hợp đó, bài toán không bao giờ có thể được chọn và luôn phải được bỏ qua, ngay cả khi tất cả các bài toán khác đều nhỏ. 

## Phương pháp tiếp cận 

Một cách trực tiếp để suy nghĩ về vấn đề này là hãy tưởng tượng việc thử mọi cách có thể để lựa chọn và giao bài toán. Đối với mỗi tập hợp con của các vấn đề, chúng tôi sẽ quyết định xem đồng đội nào sẽ giải được từng vấn đề đã chọn và tính tổng thời gian. Nếu tổng số tối đa là 300, chúng tôi sẽ cập nhật câu trả lời đúng nhất theo kích thước của tập hợp con đó. 

Quan điểm bạo lực này là đúng nhưng có hai lớp tổ hợp. Đầu tiên, có`2^n`tập hợp con của vấn đề. Đối với mỗi tập hợp con, mỗi bài toán được chọn có tối đa 3 lựa chọn gán, do đó một khai triển đơn giản sẽ như sau`O(3^n)`. Ngay cả với`n = 14`, đây đã là ranh giới nếu được triển khai mà không cẩn thận, mặc dù vẫn khả thi về mặt kỹ thuật trong mã được tối ưu hóa. Tuy nhiên, không cần thiết phải xem xét tất cả các nhiệm vụ một cách độc lập. 

Sự đơn giản hóa chính là đối với bất kỳ tập hợp con cố định nào của các bài toán, việc gán tối ưu là không đáng kể: đối với mỗi bài toán trong tập hợp con, chúng ta chỉ quan tâm đến thời gian hợp lệ tối thiểu giữa ba người cùng nhóm. Không có sự tương tác giữa các vấn đề về mặt lựa chọn bài tập, bởi vì mỗi vấn đề đều tiêu tốn cùng một nguồn tài nguyên thời gian được chia sẻ bất kể ai giải quyết nó. Điều này loại bỏ hoàn toàn lớp tổ hợp thứ hai. 

Vì vậy, vấn đề tập trung vào việc lựa chọn một tập hợp con các hạng mục trong đó mỗi hạng mục có một chi phí hiệu quả duy nhất, được xác định là thời gian thực hiện hợp lệ tối thiểu của nó. Sau đó, chúng tôi muốn số lượng vật phẩm tối đa có tổng chi phí không vượt quá 300. Đây là một biến thể ba lô cổ điển với mục tiêu là tối đa hóa số lượng vật phẩm với số lượng vật phẩm nhỏ. 

Bây giờ chúng ta chỉ cần đánh giá tất cả các tập hợp con, tính tổng chi phí và kích thước của chúng, đồng thời theo dõi kích thước khả thi nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê đầy đủ | O(3^n) | O(n) | Quá chậm | 
| Liệt kê tập hợp con với mức giảm chi phí tối thiểu | O(2^n · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xử lý trước từng vấn đề bằng cách chuyển đổi ba giá trị thời gian của nó thành một chi phí hiệu quả duy nhất. Chi phí này là nhỏ nhất trong số các giá trị hợp lệ, bỏ qua mọi giá trị`-1`mục nhập. Nếu cả ba giá trị đều`-1`, chúng tôi đánh dấu sự cố là không thể sử dụng được. 

Tiếp theo, chúng tôi liệt kê tất cả các tập hợp con của vấn đề bằng cách sử dụng mặt nạ bit. Mỗi tập hợp con đại diện cho một tập hợp các vấn đề mà chúng ta cố gắng giải quyết. 

1. Đối với mỗi vấn đề, hãy tính chi phí hiệu quả của nó bằng số thời gian tối thiểu mà đồng đội có được. Nếu tất cả đều`-1`, chúng tôi coi nó là chi phí vô hạn và đảm bảo nó không bao giờ có thể được chọn. 
2. Lặp lại tất cả các mặt nạ từ`0`ĐẾN`2^n - 1`. Mỗi mặt nạ mã hóa một tập hợp con các vấn đề. 
3. Đối với một mặt nạ nhất định, hãy tính hai giá trị: số lượng bài toán đã chọn và tổng thời gian cần thiết. 
4. Trong khi tính tổng thời gian, nếu nó vượt quá 300, chúng tôi sẽ dừng sớm đối với tập hợp con đó vì nó không thể được cải thiện bằng cách tiếp tục. 
5. Nếu tổng thời gian trong vòng 300, chúng tôi sẽ cập nhật câu trả lời với số lượng vấn đề tối đa được thấy cho đến nay. 

Việc dừng sớm bên trong mỗi tập hợp con rất quan trọng vì nó tránh được việc tính tổng không cần thiết khi tính khả thi đã bị phá vỡ, giúp giải pháp luôn hiệu quả ngay cả trong Python. 

### Tại sao nó hoạt động 

Đối với mỗi vấn đề, việc giao nó cho một đồng đội là độc lập với tất cả các vấn đề khác vì không có ràng buộc về năng lực của mỗi người, chỉ có một quỹ thời gian toàn cầu duy nhất. Điều này loại bỏ bất kỳ sự kết hợp nào giữa các quyết định phân công. Vì vậy, việc thay thế mỗi bài toán bằng chi phí phân công khả thi tối thiểu của nó sẽ bảo toàn tính khả thi của mỗi tập hợp con và không bao giờ làm tăng chi phí của một nhiệm vụ hợp lệ. Bất kỳ giải pháp tối ưu nào cũng phải tương ứng với một tập hợp con nào đó và trong tập hợp con đó, việc chọn ấn định chi phí tối thiểu cho mỗi bài toán luôn là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    cost = []

    for _ in range(n):
        parts = input().split()
        name = parts[0]
        t = list(map(int, parts[1:]))

        best = 10**9
        for x in t:
            if x != -1:
                best = min(best, x)

        cost.append(best)

    ans = 0

    for mask in range(1 << n):
        total = 0
        cnt = 0

        for i in range(n):
            if mask & (1 << i):
                if cost[i] == 10**9:
                    total = 10**9
                    break
                total += cost[i]
                cnt += 1
                if total > 300:
                    break

        if total <= 300:
            ans = max(ans, cnt)

    print(ans)

if __name__ == "__main__":
    solve()
```Bước tiền xử lý sẽ nén từng vấn đề thành một chi phí vô hướng duy nhất, điều này rất cần thiết để giảm không gian tìm kiếm. Vòng lặp bitmask sau đó khám phá trực tiếp tất cả các tập hợp con. Các điều kiện kết thúc sớm bên trong vòng lặp tập hợp con đảm bảo chúng ta không lãng phí thời gian tích lũy chi phí khi chắc chắn không thể thực hiện được. 

Chi tiết triển khai tinh tế thể hiện các vấn đề không thể xảy ra với một số lượng rất lớn thay vì 0. Việc sử dụng số 0 không chính xác sẽ cho phép lựa chọn miễn phí các vấn đề không thể giải được, làm tăng câu trả lời một cách giả tạo. Sử dụng một trọng điểm như`10**9`đảm bảo các tập hợp con như vậy tự động vượt quá ngân sách. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào đơn giản hóa với ba vấn đề:```
P1: 50 60 70
P2: -1 40 90
P3: 100 20 80
```Sau khi tiền xử lý, chúng tôi nhận được chi phí: 

P1 = 50, P2 = 40, P3 = 20 

Bây giờ chúng ta liệt kê các tập hợp con. 

| Mặt nạ | Được chọn | Tổng chi phí | Đếm | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 000 | {} | 0 | 0 | vâng | 
| 001 | P1 | 50 | 1 | vâng | 
| 010 | P2 | 40 | 1 | vâng | 
| 011 | P1,P2 | 90 | 2 | vâng | 
| 100 | P3 | 20 | 1 | vâng | 
| 101 | P1,P3 | 70 | 2 | vâng | 
| 110 | P2,P3 | 60 | 2 | vâng | 
| 111 | tất cả | 110 | 3 | vâng | 

Tập hợp con tốt nhất là cả ba bài toán có tổng chi phí là 110, cho câu trả lời 3. Dấu vết này xác nhận rằng việc giảm chi phí cho mỗi bài toán xuống mức tối thiểu sẽ duy trì tất cả những sự đánh đổi có ý nghĩa. 

Bây giờ hãy xem xét một trường hợp với một vấn đề không thể:```
P1: -1 -1 -1
P2: 200 200 200
```Chi phí trở thành: 

P1 = không thể, P2 = 200 

| Mặt nạ | Được chọn | Tổng chi phí | hợp lệ | 
| --- | --- | --- | --- | 
| 01 | P1 | không hợp lệ | không | 
| 10 | P2 | 200 | vâng | 
| 11 | P1,P2 | không hợp lệ | không | 

Câu trả lời tốt nhất là 1, vì P1 không thể đóng góp gì cả mặc dù có mặt trong đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n · n) | Mỗi tập hợp con được đánh giá bằng cách quét tối đa n mục | 
| Không gian | O(n) | Chỉ lưu trữ chi phí cho mỗi vấn đề và một vài bộ đếm | 

Với`n ≤ 14`,`2^n`nhiều nhất là 16384 và nhân với 14 vẫn mang lại kết quả dưới vài trăm nghìn thao tác, vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    input = sys.stdin.readline
    n = int(input())
    cost = []

    for _ in range(n):
        parts = input().split()
        t = list(map(int, parts[1:]))

        best = 10**9
        for x in t:
            if x != -1:
                best = min(best, x)
        cost.append(best)

    ans = 0
    for mask in range(1 << n):
        total = 0
        cnt = 0
        for i in range(n):
            if mask & (1 << i):
                if cost[i] == 10**9:
                    total = 10**9
                    break
                total += cost[i]
                cnt += 1
                if total > 300:
                    break
        if total <= 300:
            ans = max(ans, cnt)

    print(ans)

# sample
assert run("""3
A -1 20 -1
B 80 90 60
C 40 50 30
""") == "3"

# all impossible
assert run("""2
A -1 -1 -1
B -1 -1 -1
""") == "0"

# tight budget
assert run("""3
A 200 200 200
B 200 200 200
C 200 200 200
""") == "1"

# mix
assert run("""4
A 100 1 100
B 100 100 1
C -1 200 200
D 150 150 150
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều không thể | 0 | xử lý các sự cố không sử dụng được | 
| ngân sách eo hẹp | 1 | tham lam đóng gói dưới sự ràng buộc | 
| chi phí hỗn hợp | 3 | sự lựa chọn tập hợp con chính xác | 

## Vỏ cạnh 

Trường hợp một bên là khi mọi vấn đề đều không thể đối với ít nhất một đồng đội nhưng vẫn có thể thực hiện được thông qua một đồng đội khác. Ví dụ, một vấn đề có thể trông giống như`-1 10 200`. Thuật toán lấy chính xác giá trị là 10 và bỏ qua giá trị`-1`, đảm bảo vấn đề vẫn có thể sử dụng được. 

Một trường hợp khó khăn khác là khi tất cả đồng đội không thể gặp vấn đề gì. Trong trường hợp đó, chi phí của nó trở nên vô hạn và bất kỳ tập hợp con nào chứa nó sẽ tự động bị từ chối. Việc liệt kê vẫn xem xét các tập hợp con như vậy, nhưng chúng sẽ được lọc ra ngay lập tức sau khi phát hiện thấy chi phí trọng điểm. 

Trường hợp khó phát hiện cuối cùng là khi tồn tại nhiều vấn đề về chi phí nhỏ và giải pháp tối ưu đòi hỏi phải thực hiện hầu hết các vấn đề đó cho đến khi ngân sách bão hòa chính xác. Việc liệt kê mặt nạ bit đảm bảo các kết hợp này được kiểm tra rõ ràng và việc dừng sớm chỉ ảnh hưởng đến việc cắt bớt các tập hợp con không khả thi chứ không bỏ qua các kết hợp hợp lệ.
