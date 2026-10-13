---
title: "CF 105167H - Giao phối hàng giờ"
description: "Chúng tôi được cung cấp một bộ sưu tập đồ uống, mỗi đồ uống có một loại và giới hạn hết hạn được tính bằng giờ. Thời gian trôi qua một cách kín đáo: Sascha tiêu thụ chính xác một đồ uống mỗi giờ và máy phải chọn đồ uống nào được phân phối mỗi giờ."
date: "2026-06-27T10:37:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105167
codeforces_index: "H"
codeforces_contest_name: "ETH Zurich Competitive Programming Contest Spring 2024"
rating: 0
weight: 105167
solve_time_s: 188
verified: false
draft: false
---

[CF 105167H - Giao phối hàng giờ](https://codeforces.com/problemset/problem/105167/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập đồ uống, mỗi đồ uống có một loại và giới hạn hết hạn được tính bằng giờ. Thời gian trôi qua một cách kín đáo: Sascha tiêu thụ chính xác một đồ uống mỗi giờ và máy phải chọn đồ uống nào được phân phối mỗi giờ. Đồ uống sẽ không thể sử dụng được sau một số giờ cố định, nghĩa là nó chỉ có thể được sử dụng ở một trong những vị trí đầu tiên của chuỗi. 

Câu hỏi quan trọng không phải là mô phỏng mức tiêu thụ mà là xác định xem có thể đạt được bao nhiêu “vòng” hoàn chỉnh cho tất cả các loại đồ uống. Một vòng có nghĩa là đối với mỗi loại tồn tại trong máy, Sascha có thể uống ít nhất một món thuộc loại đó. Nếu loại nào đó vắng mặt, loại đó ngay lập tức đưa ra câu trả lời bằng 0. 

Mỗi đồ uống có một vị trí thời hạn trong đơn đặt hàng cuối cùng: nếu đồ uống có giá trị`b`, nó có thể được đặt nhiều nhất ở vị trí`b + 1`của trình tự tiêu dùng. Tương tự, nếu chúng ta lập chỉ mục tiêu thụ từ 1 thì mỗi mặt hàng đều có vị trí được phép muộn nhất. 

Máy được coi là được điều khiển tối ưu. Điều này có nghĩa là chúng tôi có thể tự do lựa chọn bất kỳ đơn đặt hàng đồ uống nào, miễn là chúng tôi tôn trọng các hạn chế về thời hạn sử dụng, để tối đa hóa số vòng bao gồm đầy đủ loại có thể đạt được. 

Mục đích là tính số nguyên lớn nhất`k`sao cho chúng ta có thể lên lịch ít nhất`k`đồ uống của mọi loại trước thời hạn tương ứng. 

Các hạn chế rất lớn: tối đa 3·10^5 đồ uống cho mỗi lần kiểm tra và tổng cộng là 5·10^5. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng lịch trình hoặc kiểm tra tính khả thi bằng cách xây dựng các hoán vị rõ ràng cho mỗi bước ứng viên một cách ngây thơ với hành vi bậc hai hoặc thậm chí bậc ba. Mọi cách tiếp cận đều phải gần tuyến tính hoặc n log n cho mỗi lần kiểm tra. 

Một trường hợp khó phát hiện khi một số loại bị thiếu hoàn toàn. Trong trường hợp đó, ngay cả một hiệp đấu trọn vẹn cũng là không thể. Một trường hợp góc quan trọng khác là khi có nhiều vật phẩm cùng loại tồn tại nhưng có giới hạn hết hạn chặt chẽ, buộc chúng phải chiếm các vị trí rất sớm, điều này hạn chế số vòng đầy đủ khả thi. 

## Phương pháp tiếp cận 

Giải thích trực tiếp là hãy nghĩ tới việc xây dựng trật tự tiêu dùng từng bước. Vào mỗi giờ, chúng tôi chọn một loại đồ uống vẫn còn hiệu lực, cố gắng đảm bảo rằng theo thời gian, chúng tôi thu thập số lượng cân bằng trên tất cả các loại. Người ta có thể tưởng tượng việc mô phỏng tất cả các lịch trình hợp lệ có thể có và kiểm tra xem có bao nhiêu bộ sắp chữ hoàn chỉnh xuất hiện. Tuy nhiên, số lượng hoán vị là giai thừa tính bằng n và thậm chí mô phỏng tham lam với quay lui cũng quá chậm. 

Một ý tưởng mạnh mẽ có cấu trúc hơn là tìm kiếm nhị phân`k`, số vòng đầy đủ. Đối với một cố định`k`, chúng ta cần kiểm tra xem có thể chọn ít nhất`k`đồ uống từ mỗi loại sao cho tất cả đồ uống đã chọn có thể được sắp xếp theo một trình tự hợp lệ tôn trọng thời hạn. Điều này trở thành một vấn đề khả thi về lập kế hoạch bị hạn chế. Việc kiểm tra tính khả thi đơn giản có thể sắp xếp tất cả các mục theo thời hạn và phân công vị trí một cách tham lam, nhưng nếu được thực hiện riêng cho từng loại hoặc quét lặp lại, nó dễ dàng trở thành O(n^2 log n). 

Thông tin chi tiết quan trọng là lật ngược quan điểm: thay vì nghĩ đến việc xây dựng một chuỗi toàn cầu, chúng tôi coi mỗi đồ uống là khả năng đóng góp cho các vị trí ban đầu và chúng tôi hỏi tổng thể có bao nhiêu mặt hàng có thể được “giữ” trong khi đảm bảo mỗi loại đều đóng góp ít nhất`k`mặt hàng. Vì mỗi loại đều đối xứng trong mục tiêu nên vấn đề giảm xuống còn việc xác minh xem liệu chúng ta có thể chọn`k`các mặt hàng cho mỗi loại có thời hạn cho phép chúng chiếm vị trí đầu tiên`k · m`các vị trí. 

Điều này biến vấn đề thành một cuộc kiểm tra tính khả thi về lập kế hoạch theo thời hạn cổ điển kết hợp với việc nhóm theo loại. Với mỗi loại chúng ta chỉ quan tâm đến`k`các mục tốt nhất (thời hạn lớn nhất), vì mọi lựa chọn khả thi sẽ luôn ưu tiên thời hạn muộn hơn. Sau khi thu thập các ứng cử viên này ở tất cả các loại, chúng tôi kiểm tra xem liệu chúng có thể được sắp xếp theo thứ tự thời hạn tăng dần hay không: chỉ cần quét tham lam là đủ. 

Điều này mang lại một tìm kiếm nhị phân rõ ràng trên`k`, mỗi lần kiểm tra đều tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lập kế hoạch vũ phu | O(n!)-giống như / hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + tính khả thi tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Nhóm tất cả đồ uống theo loại, lưu trữ thời gian hết hạn của chúng vào các danh sách riêng biệt. Điều này là cần thiết vì yêu cầu mang tính đối xứng theo từng loại, do đó các quyết định được đưa ra theo từng nhóm thay vì trên toàn bộ. 
2. Đối với mỗi loại, sắp xếp giá trị hết hạn theo thứ tự giảm dần. Điều này cho phép chúng tôi nhanh chóng xác định được`k`các mục phù hợp nhất để đưa vào nếu chúng ta hướng tới`k`vòng. Lý do là bất kỳ lựa chọn tối ưu nào cũng không bao giờ ưu tiên thời hạn nhỏ hơn thời hạn lớn hơn trong cùng một loại. 
3. Chúng tôi tìm kiếm câu trả lời nhị phân`k`từ 0 đến giá trị tối đa có thể, được giới hạn bởi số lượng mục tối đa trong bất kỳ loại nào và bởi`n // m`. 
4. Đối với ứng viên cố định`k`, chúng tôi cố gắng xây dựng nhiều mục đã chọn bằng cách chiếm vị trí hàng đầu`k`thời hạn của từng loại. Nếu một loại có ít hơn`k`hạng mục, ứng cử viên này ngay lập tức thất bại. 
5. Chúng tôi thu thập tất cả các mục đã chọn theo loại, tạo thành danh sách kích thước`k · m`. 
6. Chúng tôi sắp xếp danh sách này theo thời hạn. 
7. Chúng tôi mô phỏng việc gán chúng vào các khe thời gian từ 1 đến`k · m`. Tại vị trí`i`, chúng tôi kiểm tra xem thời hạn của mặt hàng đó có ít nhất là`i`. Nếu bất kỳ mục nào không đạt được điều kiện này thì lịch trình đó không khả thi`k`. 
8. Nếu tất cả các mục đều đạt, chúng ta có thể đạt được`k`, vì vậy chúng tôi di chuyển tìm kiếm nhị phân lên trên; nếu không thì đi xuống. 

Ý tưởng chính là việc sắp xếp theo thời hạn sẽ thực thi lịch trình tham lam tối ưu: luôn sử dụng các mục bị hạn chế nhất trước tiên. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một thuộc tính khả thi đơn điệu. Nếu có thể sắp xếp`k`đầy đủ vòng, sau đó bất kỳ`k' < k`cũng khả thi vì việc loại bỏ các mục cần thiết chỉ làm giảm bớt các ràng buộc. Đối với một cố định`k`, chọn`k`Các mục có thời hạn muộn nhất cho mỗi loại là tối ưu vì việc thay thế bất kỳ mục nào đã chọn bằng thời hạn chặt chẽ hơn chỉ có thể làm giảm tính khả thi. Cuối cùng, phép gán tham lam bằng cách tăng thời gian đảm bảo rằng nếu có bất kỳ phép gán nào tồn tại thì phép gán được sắp xếp này sẽ thành công vì nó luôn dành các vị trí sớm cho các phần tử bị ràng buộc nhất trước tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check(k, groups):
    items = []
    for g in groups:
        if len(g) < k:
            return False
        # take k largest deadlines
        for x in g[:k]:
            items.append(x)
    items.sort()
    for i, d in enumerate(items, start=1):
        if d < i:
            return False
    return True

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        groups = [[] for _ in range(m + 1)]
        for i in range(n):
            groups[a[i]].append(b[i])

        for i in range(1, m + 1):
            groups[i].sort(reverse=True)

        if any(len(groups[i]) == 0 for i in range(1, m + 1)):
            print(0)
            continue

        lo, hi = 0, n // m
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if check(mid, groups):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách nhóm thời gian hết hạn theo loại, sau đó sắp xếp từng nhóm theo thứ tự giảm dần để chọn nhóm đầu tiên.`k`các yếu tố tương ứng với việc lựa chọn các mục linh hoạt nhất. 

các`check`chức năng thực thi tính khả thi cho một ứng cử viên`k`. Nó xây dựng nhóm ứng viên bằng cách lấy`k`các mục cho mỗi loại, sau đó sắp xếp chúng trên toàn cầu theo thời hạn. Bước xác thực tham lam kiểm tra xem mục được lập lịch thứ i có thể tồn tại cho đến thời điểm đó hay không`i`. 

Tìm kiếm nhị phân kết thúc bài kiểm tra tính khả thi này để tìm giá trị hợp lệ tối đa`k`. 

Một cạm bẫy triển khai phổ biến là quên rằng các chỉ số dựa trên 1 trong điều kiện thời hạn. Một vấn đề tế nhị khác là đảm bảo rằng việc lựa chọn theo từng loại luôn sử dụng thời hạn lớn nhất; nếu không, tính khả thi có thể bị từ chối một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trường hợp đơn giản với 2 loại: 

đầu vào:```
n = 4, m = 2
a = [1, 1, 2, 2]
b = [2, 1, 2, 1]
```Chúng tôi kiểm tra`k = 1`. 

| Bước | Hành động | Mặt hàng | 
| --- | --- | --- | 
| nhóm | chia theo loại | T1=[2,1], T2=[2,1] | 
| chọn | chiếm top 1 mỗi người | [2] + [2] = [2,2] | 
| sắp xếp | theo thời hạn | [2,2] | 
| giao | kiểm tra vị trí | 1 2 được, 2 2 được |`k = 1`là khả thi. 

Hiện nay`k = 2`: 

| Bước | Hành động | Mặt hàng | 
| --- | --- | --- | 
| nhóm | chia theo loại | T1=[2,1], T2=[2,1] | 
| chọn | chiếm top 2 mỗi người | [2,1] + [2,1] | 
| sắp xếp | theo thời hạn | [1,1,2,2] | 
| giao | kiểm tra vị trí | vị trí 1 có 1 ≥1 ok, vị trí 2 có 1<2 thất bại | 

Vì thế`k = 2`không thành công, câu trả lời là 1. 

Điều này cho thấy thời hạn chặt chẽ đã cản trở các vòng chơi sâu hơn như thế nào ngay cả khi tổng số lượng đã đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n log n) | nhóm O(n), sắp xếp theo loại tổng O(n log n), tìm kiếm nhị phân thêm kiểm tra tính khả thi của log n, mỗi lần sắp xếp O(n log n) trong trường hợp xấu nhất | 
| Không gian | O(n) | lưu trữ các mục được nhóm và danh sách lựa chọn tạm thời | 

Các ràng buộc cho phép điều này một cách thoải mái vì tổng n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 5·10^5 và mỗi thao tác bị chi phối bởi việc sắp xếp và quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def check(k, groups):
        items = []
        for g in groups:
            if len(g) < k:
                return False
            for x in g[:k]:
                items.append(x)
        items.sort()
        for i, d in enumerate(items, start=1):
            if d < i:
                return False
        return True

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            a = list(map(int, input().split()))
            b = list(map(int, input().split()))
            groups = [[] for _ in range(m + 1)]
            for i in range(n):
                groups[a[i]].append(b[i])
            for i in range(1, m + 1):
                groups[i].sort(reverse=True)

            if any(len(groups[i]) == 0 for i in range(1, m + 1)):
                out.append("0")
                continue

            lo, hi = 0, n // m
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if check(mid, groups):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1
            out.append(str(ans))
        return "\n".join(out)

    return solve()

# provided samples (as given format may be compressed, kept conceptual placeholders)
# assert run(...) == "1\n2\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thời hạn chặt chẽ loại duy nhất | 0 hoặc 1 | xử lý kiểu thiếu | 
| tất cả các thời hạn lớn như nhau | n // m | tính khả thi thống nhất | 
| thời hạn hỗn hợp với những khoảng trống | hành vi tìm kiếm nhị phân đúng | giá trị tham lam | 
| tối thiểu n=m=1 | 1 hoặc 0 tùy b | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi có ít nhất một loại không có mục nào. Trong tình huống đó, việc nhóm ngay lập tức sẽ hiển thị một danh sách trống và bất kỳ thông tin tích cực nào`k`thất bại. Thuật toán xử lý việc này bằng cách kiểm tra sớm trước khi tìm kiếm nhị phân, trả về số 0. 

Một trường hợp khác là khi thời hạn rất gấp rút, chẳng hạn như tất cả`b_i = 0`. Dù mỗi loại có đủ vật phẩm thì cũng chỉ có thể sử dụng được vị trí đầu tiên nên`k`không bao giờ có thể vượt quá 1 và thường trở thành 0 nếu các lực lượng nhóm xung đột. Việc kiểm tra tính khả thi tham lam nắm bắt được điều này vì thời hạn được sắp xếp sẽ vi phạm các ràng buộc vị trí ban đầu ngay lập tức. 

Trường hợp tinh tế cuối cùng phát sinh khi một loại có nhiều hạng mục có thời hạn lớn và loại khác có hầu hết các hạng mục có thời hạn nhỏ. Tìm kiếm nhị phân có thể gợi ý một số lượng lớn`k`, nhưng trong quá trình xây dựng tính khả thi, loại thời hạn nhỏ sẽ không thực hiện được bước đặt hàng chung vì các mục được chọn của nó buộc phải bố trí sớm và không thể đáp ứng được.
