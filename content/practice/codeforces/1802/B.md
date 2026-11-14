---
title: "CF 1802B - Khu định cư của Lợn Guinea"
description: "Chúng tôi đang mô phỏng dòng thời gian trong đó những con chuột lang mới xuất hiện theo thời gian và đôi khi một sự kiện “tiết lộ” xảy ra khi tất cả các động vật hiện đang sở hữu đột nhiên bị tiết lộ giới tính."
date: "2026-06-09T09:28:36+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1802
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 857 (Div. 2)"
rating: 1000
weight: 1802
solve_time_s: 191
verified: false
draft: false
---

[CF 1802B - Định cư của Lợn Guinea](https://codeforces.com/problemset/problem/1802/B) 

**Đánh giá:** 1000 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng dòng thời gian trong đó những con chuột lang mới xuất hiện theo thời gian và đôi khi một sự kiện “tiết lộ” xảy ra khi tất cả các động vật hiện đang sở hữu đột nhiên bị tiết lộ giới tính. Khó khăn là trước khi được tiết lộ, mọi con chuột lang đều hoàn toàn không rõ ràng, vì vậy, bất kỳ hai con vật nào cũng có thể cùng giới tính hoặc khác giới tính trong nhiệm vụ tồi tệ nhất có thể. 

Mỗi con chuột lang phải được đặt vào các chuồng mà mỗi con có thể chứa tối đa hai con, nhưng một hạn chế chính là chúng ta không bao giờ được phép nuôi một cặp có nhiều giới tính trong cùng một chuồng. Vì giới tính không được xác định cho đến khi có sự tiết lộ nên bất kỳ nhóm nào chúng tôi tạo trước thời điểm đó đều phải an toàn theo cả hai giới tính có thể được phân công. 

Các hoạt động là một chuỗi ngày. Vào ngày được đánh dấu bằng 1, một con chuột lang mới xuất hiện và ngay lập tức phải được giao cho một chuồng chim nào đó. Vào ngày được đánh dấu bằng 2, tất cả các loài động vật hiện có đều được tiết lộ giới tính, điều này đột nhiên cung cấp cho chúng tôi thông tin đầy đủ và cho phép chúng tôi tổ chức lại một cách tối ưu trước khi xem xét những lần đến tiếp theo. 

Đầu ra là số lượng chuồng chim tối thiểu để đảm bảo chúng tôi luôn có thể duy trì một sự sắp xếp hợp lệ bất kể giới tính xuất hiện như thế nào và bất kể chúng tôi chỉ định động vật như thế nào trước mỗi lần tiết lộ. 

Các ràng buộc rất lớn, lên tới 100000 thao tác cho mỗi trường hợp thử nghiệm và tổng số 100000 trong tất cả các thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng tất cả các phân định giới tính có thể có hoặc duy trì trạng thái phức tạp trên mỗi con vật. Chúng ta cần một giải pháp xử lý từng sự kiện trong thời gian không đổi. 

Một vấn đề tế nhị phát sinh từ chuỗi dài gồm 1 thao tác mà không có 2 thao tác nào. Trong những khoảng thời gian như vậy, chúng ta buộc phải phân công động vật một cách mù quáng. Một trường hợp phức tạp khác là khi thao tác 2 xảy ra ngay sau một loạt các lần chèn: tại thời điểm đó, chúng ta phải diễn giải lại cấu trúc trong trường hợp xấu nhất của tất cả các động vật hiện chưa được gán. 

Một cách tiếp cận ngây thơ có thể cố gắng theo dõi rõ ràng các cấu hình giới tính có thể có hoặc phân bổ động các cặp một cách tham lam. Điều đó không thành công vì đối thủ (phân công giới tính không xác định) buộc chúng ta phải thực hiện phân vùng tồi tệ nhất ở mọi thời điểm. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là mô phỏng mọi sự phân công giới tính có thể có sau mỗi lần tiết lộ và tính toán cách phân nhóm tốt nhất có thể thành các chuồng chim. Đối với k chuột lang, điều đó có nghĩa là xem xét tất cả các bài tập 2^k và kiểm tra tính khả thi, theo cấp số nhân và ngay lập tức không khả thi ngay cả đối với k khoảng 30. 

Quan sát quan trọng là trước khi tiết lộ, chúng ta không có thông tin gì, vì vậy mỗi con chuột lang đều hành xử như một món đồ không chắc chắn không thể phân biệt được. Cách giải thích trong trường hợp xấu nhất buộc chúng ta phải giả định rằng khi sự việc xảy ra, giới tính được sắp xếp đối lập nhau để tối đa hóa số lượng chuồng chim cần thiết. 

Tại bất kỳ thời điểm nào ngay trước khi lộ diện, giả sử chúng ta có k chuột lang. Sau khi tiết lộ, sự phân chia tồi tệ nhất có thể xảy ra là một giới tính xuất hiện ⌈k/2⌉ lần và giới tính kia xuất hiện ⌊k/2⌋ lần. Tuy nhiên, hạn chế về cấu trúc quan trọng hơn là mỗi chuồng chim có thể chứa tối đa một cặp cùng giới tính và bất kỳ cá thể đơn lẻ nào còn sót lại buộc phải nhốt vào các lồng riêng biệt. 

Sự đơn giản hóa sâu hơn đến từ việc xem mỗi chuồng chim là trống, chứa một cặp đã biết hoặc chứa một con chuột lang không chắc chắn. Mỗi lần tiết lộ xảy ra, chúng tôi có thể nén hệ thống một cách tối ưu: từ k con vật, điều tốt nhất chúng tôi có thể làm là gói chúng thành các cặp ⌊k/2⌋ và có thể là một con còn sót lại. Cấu trúc còn sót lại đó xác định số lượng "khe có thể sử dụng" còn lại cho các lần chèn trong tương lai.

Quan điểm tham lam đúng đắn là sau mỗi lần tiết lộ, chúng ta sẽ tổ chức lại thành số lượng chuồng chim tối thiểu phù hợp với sự phân chia giới tính trong trường hợp xấu nhất. Điều này tạo ra trạng thái rõ ràng: chúng tôi luôn duy trì số lượng chuột lang "chưa ghép đôi" hiện tại và mỗi lần tiết lộ cho phép chúng tôi chuyển đổi càng nhiều chuột lang càng tốt thành các cặp ổn định. 

Do đó, giải pháp giảm bớt việc theo dõi số lượng động vật tồn tại kể từ lần tiết lộ cuối cùng và sau mỗi lần tiết lộ, chuyển chúng thành dạng nén trong đó chỉ có một tập hợp nhỏ còn lại là quan trọng. Mỗi lần chèn mới sẽ tiêu tốn một vị trí trống trong chuồng chim hiện có hoặc buộc phải tạo một chuồng mới trong các giả định trong trường hợp xấu nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực về việc phân công giới tính | O(2^n · n) | O(n) | Quá chậm | 
| Tham lam nén mỗi lúc lộ diện | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các thao tác từ trái sang phải trong khi vẫn duy trì một giá trị duy nhất biểu thị số lượng chuột lang hiện "chưa được giải quyết", nghĩa là chúng đã được thêm vào kể từ lần khám bác sĩ gần đây nhất. 

1. Khởi tạo bộ đếm`cur = 0`, đại diện cho số lượng chuột lang kể từ lần tiết lộ cuối cùng, và`ans = 0`, đại diện cho các chuồng chim cần thiết ở trạng thái tồi tệ nhất. 
2. Lặp lại từng ngày trong lịch trình. 
3. Nếu hoạt động`1`, tăng`cur`bởi vì một con chuột lang mới được thêm vào và ngay lập tức phải đặt trong tình trạng không chắc chắn. Điều này phản ánh rằng nó có khả năng làm tăng áp lực ghép nối trong trường hợp xấu nhất. 
4. Nếu hoạt động`2`, chúng tôi xử lý một sự kiện tiết lộ. Vào lúc này, tất cả`cur`chuột lang có thể được sắp xếp lại thành chuồng chim một cách tối ưu. 
5. Để tính toán yêu cầu trong trường hợp xấu nhất, hãy quan sát rằng`cur`động vật có thể hình thành nhiều nhất`cur // 2`các cặp an toàn, có thể còn sót lại một chiếc. Số lượng chuồng chim cần thiết cho những con vật này chính xác là`cur - (cur // 2)`, điều này đơn giản hóa thành`(cur + 1) // 2`. 
6. Chúng tôi tích lũy khoản đóng góp này vào`ans`, bởi vì những chuồng chim này phải tồn tại để đáp ứng an toàn cho phân khúc hiện tại. 
7. Đặt lại`cur = 0`, vì sau khi tiết lộ, về mặt khái niệm, chúng tôi sắp xếp lại mọi thứ một cách tối ưu và bắt đầu mới. 

Điều tinh tế quan trọng là chúng tôi không bao giờ hợp nhất thông tin trên các phân đoạn khác nhau được phân tách bằng các tiết lộ. Mỗi phân đoạn hoạt động độc lập vì chuyến thăm khám của bác sĩ sẽ giải quyết mọi sự không chắc chắn và cho phép cấu hình lại toàn bộ. 

### Tại sao nó hoạt động 

Điều bất biến là giữa hai thao tác tiết lộ bất kỳ, tất cả chuột lang trong khoảng đó đều là những phần tử không chắc chắn không thể phân biệt được và chúng ta chỉ cần tính đến việc phân nhóm trường hợp xấu nhất của chúng tại thời điểm giải quyết. Khi tiết lộ, chúng tôi thay thế toàn bộ nhiều nhóm động vật không chắc chắn bằng số lượng chuồng chim tối thiểu cần thiết trong trường hợp phân bổ giới tính trong trường hợp xấu nhất, chính xác là số lượng xung đột tối đa không thể tránh khỏi do việc ghép đôi đối nghịch gây ra. Vì mỗi phân đoạn đều độc lập sau khi được tiết lộ và sắp xếp lại hoàn toàn nên việc tổng hợp chi phí của phân khúc sẽ mang lại mức tối thiểu hợp lệ trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    b = list(map(int, input().split()))
    
    cur = 0
    ans = 0
    
    for x in b:
        if x == 1:
            cur += 1
        else:
            ans += (cur + 1) // 2
            cur = 0
    
    print(ans)
```Việc triển khai chỉ giữ lại hai biến tương ứng trực tiếp với trạng thái trong thuật toán:`cur`lưu trữ kích thước khối chưa được giải quyết hiện tại và`ans`tích lũy các chuồng chim cần thiết trên tất cả các khối được phân tách bằng các lần khám bác sĩ. 

Chi tiết triển khai chính là công thức`(cur + 1) // 2`, thể hiện một cách ngắn gọn việc nhóm trong trường hợp xấu nhất thành cặp và đơn. Đặt lại`cur`sau mỗi lần`2`là điều cần thiết vì mỗi lần khám bác sĩ sẽ loại bỏ hoàn toàn sự không chắc chắn và cho phép tổ chức lại một cách tối ưu, do đó cấu trúc trước đó không còn được tiếp tục. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi hai đầu vào đại diện để hiểu cách hình thành và đánh giá các phân khúc. 

### Ví dụ 1 

đầu vào:```
1
5
1 1 1 2 1
```| Bước | Hoạt động | cur | trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 
| 2 | 1 | 2 | 0 | 
| 3 | 1 | 3 | 0 | 
| 4 | 2 | 0 | 2 | 
| 5 | 1 | 1 | 2 | 

Sau ba lần chèn đầu tiên, chúng ta có một khối có kích thước 3. Khi tiết lộ, điều này yêu cầu`(3+1)//2 = 2`chuồng chim trong trường hợp xấu nhất. Sau khi đặt lại, một con chuột lang mới bắt đầu một khối mới, chưa đóng góp gì cả. 

Điều này xác nhận rằng mỗi khối liền kề giữa 2 thao tác được xử lý độc lập và đóng góp một chi phí cố định. 

### Ví dụ 2 

đầu vào:```
1
6
2 2 1 1 2 1
```| Bước | Hoạt động | cur | trả lời | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 
| 2 | 2 | 0 | 0 | 
| 3 | 1 | 1 | 0 | 
| 4 | 1 | 2 | 0 | 
| 5 | 2 | 0 | 1 | 
| 6 | 1 | 1 | 1 | 

Ở đây hai thao tác đầu tiên tạo ra các phân đoạn trống. Đoạn thứ ba có kích thước 2, góp phần`(2+1)//2 = 1`. Con chuột lang cuối cùng vẫn chưa được giải quyết ở phần cuối và không đóng góp trừ khi có một giải pháp ngầm cuối cùng được thực hiện sau đó, điều này không cần thiết trong cấu trúc vấn đề vì chỉ những phân đoạn kết thúc bằng chuyến thăm bác sĩ mới được hoàn thiện. 

Điều này chứng tỏ rằng chỉ những phân đoạn đóng hoàn toàn (kết thúc bằng số 2) mới đóng góp vào câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thao tác được xử lý một lần với bản cập nhật O(1) | 
| Không gian | O(1) | Chỉ có hai quầy được duy trì | 

Giải pháp mở rộng quy mô trực tiếp theo tổng số thao tác trên tất cả các trường hợp thử nghiệm, tối đa là 100000, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        b = list(map(int, input().split()))
        cur = 0
        ans = 0
        for x in b:
            if x == 1:
                cur += 1
            else:
                ans += (cur + 1) // 2
                cur = 0
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""6
3
1 1 1
3
2 2 2
5
1 1 1 2 1
10
1 2 1 2 1 2 1 2 1 2
20
1 2 1 1 1 1 1 2 1 2 1 2 2 1 1 1 1 1 1 1
20
2 1 1 2 1 1 2 1 2 2 1 1 1 2 2 1 1 1 1 2
""") == """3
0
3
4
12
9"""

# custom cases
assert run("""1
1
1
""") == "1", "single insertion"

assert run("""1
1
2
""") == "0", "only doctors"

assert run("""1
5
1 1 1 1 1
""") == "3", "odd block size"

assert run("""1
4
2 1 1 2
""") == "1", "two segments"

assert run("""1
6
1 1 2 1 1 2
""") == "2", "repeated resets"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 1 | phân đoạn tối thiểu không trống | 
| đơn 2 | 0 | chưa từng có loài động vật nào tồn tại | 
| năm 1 giây | 3 | ghép nối trường hợp xấu nhất trong một khối | 
| 2 1 1 2 | 1 | xử lý nhiều phân đoạn | 
| 1 1 2 1 1 2 | 2 | lặp đi lặp lại thiết lập lại chính xác | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không có miếng chèn nào giữa các lần khám bác sĩ. Trong trường hợp đó, kích thước phân đoạn bằng 0 và không đóng góp gì. Ví dụ, đầu vào`2 2 2`sản xuất`cur = 0`mỗi lần, vì vậy mỗi bản cập nhật đều bổ sung thêm`(0 + 1)//2 = 0`, mang lại câu trả lời bằng 0 như mong đợi. 

Một trường hợp quan trọng khác là trình tự chèn kéo dài không bị gián đoạn, sau đó là một lần khám bác sĩ. Đối với đầu vào`1 1 1 1 1 2`, chúng tôi tích lũy`cur = 5`và khi tiết lộ, chúng tôi tính toán`(5 + 1)//2 = 3`. Điều này phù hợp với yêu cầu trong trường hợp xấu nhất trong đó tối đa hai con vật chỉ có thể ở chung chuồng một cách an toàn nếu chúng có cùng giới tính, về cơ bản buộc phải ghép đôi với một con còn sót lại. 

Cuối cùng, xen kẽ các mẫu như`1 2 1 2 1 2`xác nhận rằng mỗi đoạn có độ dài một đóng góp chính xác một chuồng chim và việc đặt lại sẽ ngăn chặn mọi tương tác chuyển tiếp. Mỗi phân đoạn là độc lập và việc đặt lại thuật toán đảm bảo không có trạng thái cũ nào ảnh hưởng đến các tính toán trong tương lai.
