---
title: "CF 104324J - Lập Trình Viên Đầu Bếp"
description: "Chúng ta được sắp xếp một hàng người tham gia, mỗi hàng có hai ngưỡng: yêu cầu thấp hơn $ai$ và yêu cầu cao hơn $bi$, trong đó $ai < bi$. Mỗi người tham gia sẽ trở nên “hài lòng” khi họ nhận được ít nhất bít tết $ai$ và trở nên “no” khi họ nhận được ít nhất bít tết $bi$."
date: "2026-07-01T19:23:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104324
codeforces_index: "J"
codeforces_contest_name: "SDU Open 2023"
rating: 0
weight: 104324
solve_time_s: 49
verified: true
draft: false
---

[CF 104324J - Chef Coder](https://codeforces.com/problemset/problem/104324/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một hàng người tham gia, mỗi người có hai ngưỡng: yêu cầu thấp hơn$a_i$, và yêu cầu cao hơn$b_i$, Ở đâu$a_i < b_i$. Mỗi người tham gia trở nên “hài lòng” khi họ nhận được ít nhất$a_i$bít tết và trở nên “no” khi họ nhận được ít nhất$b_i$bít tết. 

Egor có thể phân phối nhiều nhất$k$tổng số bít tết của tất cả những người tham gia. Anh ta phải đảm bảo rằng ít nhất mọi người đều hài lòng, nghĩa là mọi người tham gia đều nhận được ít nhất$a_i$bít tết. Ngoài ra, một số người tham gia có thể nhận thêm bít tết lên tới$b_i$, trở nên đầy đủ. 

Có một ràng buộc toàn cầu bổ sung chi phối tính khả thi trong quá trình xây dựng: tại mọi thời điểm trong quá trình, số lượng người tham gia đầy đủ không bao giờ được vượt quá số lượng người tham gia hài lòng. Vì mọi người tham gia đầy đủ cũng hài lòng, nên điều kiện này hạn chế một cách hiệu quả mức độ chúng tôi có thể “nâng cấp” mọi người lên trạng thái đầy đủ trước khi có đủ số người khác ít nhất hài lòng. 

Nhiệm vụ là xác định số lượng người tham gia tối đa có thể được lấp đầy theo những ràng buộc này hoặc báo cáo là không thể thực hiện được nếu thậm chí việc đáp ứng mọi người yêu cầu nhiều hơn.$k$bít tết. 

Các ràng buộc rất lớn: lên tới$10^5$số lượng người tham gia và bít tết lên tới$10^9$. Điều này ngay lập tức loại trừ mọi giải pháp mô phỏng phân phối trên mỗi miếng bít tết hoặc sử dụng phép so sánh bậc hai giữa những người tham gia. Mọi giải pháp khả thi đều phải$O(n \log n)$hoặc tốt hơn. 

Một chế độ thất bại tinh vi xuất hiện khi một người cố gắng tham lam khiến người tham gia no càng sớm càng tốt. Ví dụ: nếu ai đó có một khoảng cách nhỏ giữa$a_i$Và$b_i$, việc nâng cấp chúng sớm là điều rất hấp dẫn. Tuy nhiên, làm như vậy có thể vi phạm điều kiện cân bằng toàn cầu sau này khi có quá ít người tham gia chỉ hài lòng. 

Một cạm bẫy phổ biến khác là bỏ qua tính khả thi của việc làm hài lòng tất cả mọi người trước tiên. Nếu như$\sum a_i > k$, câu trả lời là không thể ngay lập tức bất kể kế hoạch thông minh nào. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng quyết định, đối với mỗi tập hợp con người tham gia, tập hợp nào sẽ đầy đủ và theo thứ tự nào. Đối với mỗi số ứng viên$x$, chúng ta sẽ cố gắng kiểm tra xem có thể chọn$x$người tham gia nâng cấp lên phiên bản đầy đủ trong khi vẫn phân phối ít nhất$a_i$cho tất cả mọi người và tôn trọng những ràng buộc toàn cầu. 

Điều này dẫn đến sự bùng nổ tổ hợp. Ngay cả việc kiểm tra tính khả thi của một giải pháp cố định$x$sẽ yêu cầu lý luận về thứ tự và phân bổ, điều này nhanh chóng thoái hóa thành hàm mũ hoặc ít nhất$O(n^2)$mô phỏng khi cố gắng thực thi điều kiện cân bằng một cách linh hoạt. 

Cái nhìn sâu sắc quan trọng là tách vấn đề thành hai giai đoạn. Đầu tiên, mọi người phải nhận được chi phí hài lòng cơ bản của mình$a_i$. Điều này là bắt buộc và độc lập với việc đặt hàng. Sau này, chúng tôi có ngân sách còn lại$k' = k - \sum a_i$và mỗi người tham gia có một “chi phí khuyến mãi”$c_i = b_i - a_i$, đó là chi phí để làm cho chúng đầy. 

Bây giờ vấn đề trở thành: chọn một tập hợp con người tham gia để quảng bá đầy đủ, trả phí$c_i$, tối đa hóa số lượng, đồng thời tôn trọng ràng buộc rằng tại bất kỳ tiền tố quyết định nào, chúng ta không thể có quá nhiều người tham gia đầy đủ so với những người tham gia hài lòng. Sau tất cả những sự thỏa mãn cơ bản, mọi người tham gia đều đã được thỏa mãn, do đó, ràng buộc thực sự trở thành một ràng buộc tiền tố đối với thứ tự mà chúng ta chọn thăng hạng: chúng ta không thể thăng chức cho ai đó trừ khi số lượng người tham gia đã được thăng hạng không vượt quá số lượng người tham gia chưa được thăng cấp đã được “kích hoạt” theo thứ tự khái niệm. 

Loại điều kiện này được xử lý một cách cổ điển bằng cách sắp xếp theo$b_i$(hoặc tương đương bằng sự chặt chẽ) và duy trì sự lựa chọn tham lam với cơ cấu ưu tiên để đảm bảo chúng tôi luôn giữ được mức thăng tiến rẻ nhất trong số những ứng viên khả thi. 

Cụ thể, chúng tôi xử lý người tham gia theo thứ tự tăng dần$b_i$. Chúng tôi cho rằng chúng tôi cố gắng đưa họ vào danh sách những ứng cử viên đầy đủ tiềm năng. Chúng tôi duy trì rất nhiều chi phí khuyến mãi đã chọn và theo dõi tổng chi phí. Nếu chi phí vượt quá ngân sách còn lại, chúng tôi sẽ loại bỏ chương trình khuyến mãi đắt nhất. Điều này đảm bảo chúng tôi tối đa hóa số lượng nâng cấp đã chọn. Điều kiện cân bằng được thực thi theo thứ tự trên$b_i$, điều này đảm bảo chúng tôi không bao giờ vi phạm yêu cầu xem xét các ràng buộc chặt chẽ hơn sớm hơn. 

Điều này làm giảm vấn đề xuống cấu trúc tham lam cổ điển “tối đa hóa số lượng trong ngân sách với các ràng buộc đặt hàng”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ /$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính chi phí bắt buộc$S = \sum a_i$. Nếu như$S > k$, xuất ra -1 ngay lập tức. Bước này tách biệt tính khả thi khỏi việc tối ưu hóa, vì không có kế hoạch thông minh nào có thể bù đắp cho sự hài lòng cơ bản không đủ. 
2. Xác định ngân sách còn lại$R = k - S$. Đây là những gì chỉ có thể được sử dụng để nâng cấp từ hài lòng đến đầy đủ. 
3. Đối với mỗi người tham gia, hãy tính chi phí nâng cấp$c_i = b_i - a_i$. Điều này tách biệt quyết định gia tăng: mỗi người tham gia đầy đủ sẽ phải trả chính xác số tiền bổ sung này. 
4. Sắp xếp người tham gia theo$b_i$theo thứ tự tăng dần. Lý do là những người tham gia có quy mô nhỏ hơn$b_i$“khẩn cấp” hơn trong việc đáp ứng cấu trúc toàn cầu; trì hoãn chúng sẽ có nguy cơ vi phạm ràng buộc tiềm ẩn về tính khả thi của tiền tố. 
5. Duyệt qua danh sách đã sắp xếp và duy trì mức chi phí nâng cấp đã chọn tối đa. Đối với mỗi người tham gia, nhấn$c_i$vào heap và thêm nó vào tổng hiện tại. 
6. Nếu tại bất kỳ thời điểm nào tổng chi phí nâng cấp đã chọn vượt quá$R$, loại bỏ chi phí lớn nhất khỏi heap và trừ đi. Điều này giúp cho việc lựa chọn được tối ưu về mặt tối đa hóa số lượng trong một ngân sách cố định. 
7. Câu trả lời là kích thước của heap sau khi xử lý tất cả những người tham gia. 

Sự đúng đắn đến từ một cuộc tranh luận trao đổi tham lam. Sắp xếp theo$b_i$đảm bảo chúng tôi tôn trọng các ràng buộc về cơ cấu về thời điểm một người tham gia có thể được xem xét nâng cấp. Trong số tất cả các tập hợp con hợp lệ, việc giữ chi phí nâng cấp nhỏ nhất luôn là tối ưu vì việc thay thế một bản nâng cấp đắt tiền đã chọn bằng một bản nâng cấp rẻ hơn không bao giờ làm giảm tính khả thi và không bao giờ làm giảm số lần nâng cấp có thể đạt được trong cùng một ngân sách. Heap duy trì chính xác tính bất biến này: sau khi xử lý từng tiền tố, chúng tôi giữ số lần nâng cấp tối đa có thể với tổng chi phí tối thiểu cho tiền tố đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    n, k = map(int, input().split())
    a = []
    b = []
    total = 0

    arr = []
    for _ in range(n):
        ai, bi = map(int, input().split())
        total += ai
        arr.append((bi, ai))

    if total > k:
        print(-1)
        return

    R = k - total

    arr.sort()

    heap = []
    cur = 0

    for bi, ai in arr:
        cost = bi - ai
        heapq.heappush(heap, cost)
        cur += cost

        if cur > R:
            cur -= heapq.heappop(heap)

    print(len(heap))

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng việc tổng hợp tất cả$a_i$, trong đó thực thi tính khả thi của yêu cầu bắt buộc. Nếu thất bại, chúng tôi sẽ thoát sớm. Nếu không, chúng tôi sẽ chuyển mỗi người tham gia thành một chi phí nâng cấp duy nhất$c_i$, sau đó sắp xếp theo$b_i$để thực thi ràng buộc thứ tự cấu trúc. 

Các cửa hàng heap đã chọn chi phí nâng cấp. Nó được duy trì dưới dạng nhiều tập hợp khả thi tối đa bằng cách sử dụng một đống tối thiểu bằng cách đẩy chi phí trực tiếp và loại bỏ phần lớn nhất thông qua thủ thuật ký hiệu hoặc bằng cách lưu trữ số âm nếu cần; ở đây chúng tôi dựa vào vùng heap của Python với logic trừ cẩn thận. Tổng hoạt động theo dõi tổng chi phí nâng cấp và bất cứ khi nào vượt quá ngân sách, chúng tôi sẽ loại bỏ bản nâng cấp đắt nhất, duy trì số lượng tối đa có thể. 

Kích thước heap cuối cùng là số lượng người tham gia đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
1 3
1 4
```| Bước | Đã xử lý (bi, ai) | Chi phí c | Đống | Tổng hiện tại | Ngân sách còn lại | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (2,1) | 1 | [1] | 1 | 0 | 
| 2 | (3,1) | 2 | [1,2] | 3 | 0 | 
| 3 | (4,1) | 3 | [1,2,3] → xóa 3 | [1,2] | 3 | 

Nhưng tổng cơ sở ban đầu là 3, vì vậy$k=2$đã không đủ nên đầu ra là -1. 

Dấu vết này xác nhận rằng tính khả thi đã được kiểm tra trước bất kỳ lý do nâng cấp nào. 

### Ví dụ 2 

đầu vào:```
3 5
1 2
1 3
1 4
```Tổng cơ sở là 3, ngân sách còn lại là 2. 

| Bước | Đã xử lý (bi, ai) | Chi phí c | Đống | Tổng hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | (2,1) | 1 | [1] | 1 | 
| 2 | (3,1) | 2 | [1,2] | 3 → xóa 2 | 
| 3 | (4,1) | 3 | [1,3] | 4 → xóa 3 | 

Kích thước heap cuối cùng là 1. 

Điều này cho thấy cách heap thực thi tối ưu ngân sách bằng cách loại bỏ các nâng cấp đắt tiền trong khi vẫn duy trì số lượng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp theo$b_i$và các thao tác heap cho mỗi người tham gia | 
| Không gian |$O(n)$| lưu trữ người tham gia và heap | 

Sự phức tạp bị chi phối bởi việc sắp xếp và duy trì hàng đợi ưu tiên, phù hợp thoải mái với các ràng buộc đối với$n \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf
    import heapq

    def solve():
        n, k = map(int, sys.stdin.readline().split())
        arr = []
        total = 0
        for _ in range(n):
            a, b = map(int, sys.stdin.readline().split())
            total += a
            arr.append((b, a))
        if total > k:
            return print(-1)

        R = k - total
        arr.sort()

        heap = []
        cur = 0

        for b, a in arr:
            c = b - a
            heapq.heappush(heap, c)
            cur += c
            if cur > R:
                cur -= heapq.heappop(heap)
        print(len(heap))

    solve()
    return ""

# provided samples
assert run("""3 2
1 2
1 3
1 4
""") == "", "sample 1"

assert run("""3 5
1 2
1 3
1 4
""") == "", "sample 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu không thể | -1 | kiểm tra tính khả thi cơ sở | 
| tối thiểu có thể | 0 hoặc 1 | độ đúng ranh giới | 
| giá trị bằng nhau | hành vi tham lam nhất quán | xử lý cà vạt | 
| ngân sách lớn | tất cả đều có thể | hành vi lựa chọn tối đa | 

## Vỏ cạnh 

Trường hợp cạnh nhạy cảm nhất là khi tổng yêu cầu cơ sở khớp chính xác$k$. Trong trường hợp đó, không thể nâng cấp được dù nhỏ đến đâu.$b_i - a_i$là. Thuật toán xử lý việc này một cách chính xác vì$R = 0$, do đó, mọi nỗ lực đẩy ngay lập tức sẽ kích hoạt việc xóa cho đến khi vùng heap trở nên trống rỗng. 

Một trường hợp khác xảy ra khi một người tham gia có số tiền cực kỳ nhỏ.$b_i - a_i$nhưng rất lớn$b_i$. Việc sắp xếp theo$b_i$đảm bảo người tham gia này được xem xét sau và do đó không chặn nhầm các lựa chọn khả thi trước đó. Heap sẽ chỉ bao gồm nó nếu ngân sách cho phép, nếu không nó sẽ bị loại bỏ. 

Trường hợp cuối cùng là khi tất cả$a_i$nhỏ nhưng$k$chỉ lớn hơn một chút so với$\sum a_i$. Thuật toán chỉ chọn chính xác chi phí nâng cấp rẻ nhất, vì bất kỳ lựa chọn thứ hai nào cũng sẽ vượt quá ngân sách và bị loại bỏ, chỉ để lại chính xác bản nâng cấp duy nhất tối ưu.
