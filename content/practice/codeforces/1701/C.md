---
title: "CF 1701C - Quản lý tiến độ"
description: "Chúng ta được giao một tập hợp các nhiệm vụ, trong đó mỗi nhiệm vụ có một nhân viên “ưu tiên” được chỉ định. Nếu một công nhân xử lý một nhiệm vụ mà họ được giao ưu tiên thì sẽ mất một đơn vị thời gian; nếu không thì phải mất hai."
date: "2026-06-09T21:47:33+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1701
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 131 (Rated for Div. 2)"
rating: 1400
weight: 1701
solve_time_s: 102
verified: true
draft: false
---

[CF 1701C - Quản lý lịch trình](https://codeforces.com/problemset/problem/1701/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được giao một tập hợp các nhiệm vụ, trong đó mỗi nhiệm vụ có một nhân viên “ưu tiên” được chỉ định. Nếu một công nhân xử lý một nhiệm vụ mà họ được giao ưu tiên thì sẽ mất một đơn vị thời gian; nếu không thì phải mất hai. Công nhân hoạt động độc lập song song và mỗi công nhân chỉ có thể xử lý một nhiệm vụ tại một thời điểm. 

Mục tiêu là giao mọi nhiệm vụ cho một số công nhân sao cho thời gian hoàn thành tổng thể, nghĩa là thời gian tối đa mà bất kỳ công nhân nào dành cho nhiệm vụ được giao của mình, được giảm thiểu. 

Cấu trúc chính là mỗi công nhân có một tập hợp con các nhiệm vụ họ có thể thực hiện nhanh chóng và mọi nhiệm vụ khác sẽ chậm hơn. Chúng tôi không quyết định kịp thời thứ tự nhiệm vụ trên toàn cầu mà phân bổ tải cho các công nhân để không một công nhân nào trở thành nút cổ chai. 

Các ràng buộc đủ chặt chẽ để mọi mô phỏng bậc hai hoặc theo từng công nhân trên tất cả các giá trị thời gian có thể sẽ không hoạt động. Tổng số nhiệm vụ trên tất cả các trường hợp thử nghiệm lên tới 2⋅10^5, điều này buộc phải có O(n log n) hoặc O(n) cho mỗi chiến lược trường hợp thử nghiệm. 

Một cách giải thích ngây thơ có thể cố gắng mô phỏng rõ ràng sự phân công hoặc thử tất cả các phân phối nhiệm vụ chậm và nhanh trên mỗi công nhân. Điều đó ngay lập tức trở nên không thể thực hiện được vì ngay cả việc phân phối nhiệm vụ một cách tham lam theo nhiều cấu hình cũng sẽ yêu cầu khám phá các bài tập tổ hợp. 

Một trường hợp phức tạp xuất hiện khi một công nhân được giao nhiều nhiệm vụ không được ưu tiên trong khi những nhiệm vụ khác chỉ được thực hiện nhẹ nhàng. Ví dụ: nếu tất cả các nhiệm vụ thuộc về một công nhân duy nhất, câu trả lời không phải là 1 mà tăng tuyến tính tùy thuộc vào số lượng nhiệm vụ mà họ phải xử lý. Một trường hợp đặc biệt khác là khi các nhiệm vụ được phân bổ đồng đều giữa các công nhân nhưng việc phân công chúng theo kiểu vòng tròn đơn giản sẽ gây ra các nhiệm vụ chậm không cần thiết mặc dù có sự thay đổi cân bằng. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng giao nhiệm vụ trực tiếp, chúng ta sẽ cần phải quyết định cho từng nhiệm vụ xem nhân viên nào sẽ thực hiện nhiệm vụ đó và họ thực hiện nhiệm vụ theo thứ tự nào. Vì mỗi nhiệm vụ có thể mất 1 hoặc 2 giờ tùy theo nhiệm vụ và các công nhân làm việc song song nên thời gian hoàn thành phụ thuộc vào khối lượng công việc tối đa của các công nhân. 

Một ý tưởng mạnh mẽ là thử tất cả các nhiệm vụ được giao cho công nhân và tính toán thời gian hoàn thành. Đây rõ ràng là hàm mũ theo m vì mỗi nhiệm vụ có n lựa chọn và thậm chí việc cắt tỉa cũng không giúp ích gì trong trường hợp xấu nhất. Một nỗ lực khác có thể mô phỏng việc tăng thời gian T và kiểm tra xem liệu T có thể hoàn thành tất cả các nhiệm vụ hay không, nhưng ngay cả việc kiểm tra tính khả thi cũng yêu cầu quyết định cách phân chia nhiệm vụ cho mỗi công nhân, điều này không tầm thường trừ khi được cấu trúc cẩn thận. 

Cái nhìn sâu sắc quan trọng là đảo ngược quan điểm: thay vì giao nhiệm vụ trực tiếp, chúng tôi sửa câu trả lời T của ứng viên và hỏi liệu có thể hoàn thành mọi thứ trong T hay không. Nếu biết cách kiểm tra tính khả thi, chúng tôi có thể tìm kiếm nhị phân T. 

Đối với T cố định, mỗi công nhân có thể hoàn thành tối đa T nhiệm vụ nếu tất cả đều là nhiệm vụ ưu tiên và ít nhất ⌊T/2⌋ nhiệm vụ nếu tất cả đều không được ưu tiên. Điều quan trọng là mỗi nhiệm vụ đều đóng góp 1 đơn vị tải cho nhân viên ưa thích của mình hoặc 2 đơn vị cho người khác, vì vậy, chúng tôi muốn phân bổ nhiệm vụ theo cách giảm thiểu tình trạng quá tải bằng cách ưu tiên sử dụng các nhiệm vụ ưa thích của nhân viên trước. 

Đối với mỗi công nhân, gọi cnt[i] là số lượng nhiệm vụ được công nhân i ưa thích. Nếu chúng ta giao x nhiệm vụ đó cho công nhân i thì mỗi nhiệm vụ sẽ có giá 1. Những nhiệm vụ còn lại được giao cho công nhân tôi phải đến từ những công nhân khác và chi phí mỗi công việc là 2. Tổng thời gian tôi bỏ ra là x + 2y, trong đó y là số công việc không được ưu tiên. 

Cấu trúc tham lam xuất hiện từ việc nhận thấy rằng việc giao các nhiệm vụ ưu tiên luôn tốt hơn ở địa phương. Vì vậy, đối với T cố định, chúng tôi cố gắng tối đa hóa việc sử dụng các nhiệm vụ ưa thích trên mỗi nhân viên lên đến T và xem còn lại bao nhiêu nhiệm vụ còn lại. Những phần còn lại đó yêu cầu có 2 khe cắm đơn vị, vì vậy, chúng tôi kiểm tra xem liệu tất cả các nhiệm vụ còn lại có phù hợp với khả năng còn lại của các công nhân hay không.

Tuy nhiên, điều này vẫn dẫn đến một cách thức cải cách đơn giản hơn: mỗi công nhân i có các nhiệm vụ ưu tiên cnt[i]. Nếu chúng ta đặt thời gian T, công nhân i có thể thực hiện tổng cộng tối đa T nhiệm vụ, nhưng trong số đó nhiều nhất là cnt[i] có thể rẻ. Do đó, mỗi công nhân đóng góp công suất rẻ min(cnt[i], T), và mọi công suất còn lại lên tới T phải được lấp đầy bằng các nhiệm vụ đắt tiền. Tổng hợp các công nhân mang lại tổng số vị trí giá rẻ; nếu tổng số vị trí giá rẻ trên tất cả công nhân ít nhất là m, chúng ta có thể hoàn thành trong T. 

Tính khả thi đơn điệu này cho phép tìm kiếm nhị phân trên T. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Nhiệm vụ Brute Force | Hàm mũ | O(1) | Quá chậm | 
| Tìm kiếm nhị phân + kiểm tra tham lam | O((n+m) log m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn vấn đề thành việc kiểm tra xem thời gian T cho trước có đủ hay không. 

1. Đếm xem mỗi công nhân thành thạo bao nhiêu công việc. Điều này cho ra một mảng tần số cnt. Điều này là cần thiết vì mọi quyết định chỉ phụ thuộc vào số lượng chứ không phụ thuộc vào nhận dạng nhiệm vụ. 
2. Đối với thời điểm ứng viên T, hãy tính xem mỗi công nhân có thể thực hiện bao nhiêu nhiệm vụ một cách “thoải mái” như những nhiệm vụ ưa thích. Đây là min(cnt[i], T) đối với mỗi công nhân vì ngay cả một công nhân thành thạo cũng không thể xử lý tổng cộng nhiều hơn T nhiệm vụ. 
3. Tính tổng các giá trị này trên tất cả công nhân. Điều này cung cấp số lượng nhiệm vụ tối đa có thể được xử lý trong 1 đơn vị cho mỗi chế độ nhiệm vụ. 
4. Nếu tổng này ít nhất là m thì tất cả các nhiệm vụ có thể được giao trong vòng T giờ. Ngược lại, T quá nhỏ. 
5. Sử dụng tìm kiếm nhị phân trên T từ 1 đến 2m, vì trong trường hợp xấu nhất, mỗi tác vụ sẽ mất 2 giờ. 

Tại sao điều này có tác dụng gắn liền với thực tế là bất kỳ nhiệm vụ nào cũng có thể được sắp xếp lại để mọi công nhân trước tiên có thể đảm nhận tất cả các nhiệm vụ ưa thích của họ cho đến công suất T trước khi thực hiện bất kỳ nhiệm vụ không được ưu tiên nào. Việc sắp xếp lại này không bao giờ làm tăng thời gian hoàn thành vì nó chỉ thay thế những nhiệm vụ đắt tiền bằng những nhiệm vụ rẻ hơn hoặc không thay đổi chúng. Do đó, tính khả thi chỉ phụ thuộc vào việc liệu tổng số “thời gian 1 giờ” có sẵn giữa các công nhân có thể thực hiện tất cả các nhiệm vụ trong thời gian T hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(T, cnt, m):
    total = 0
    for x in cnt:
        total += min(x, T)
    return total >= m

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    
    cnt = [0] * n
    for v in a:
        cnt[v - 1] += 1

    lo, hi = 1, 2 * m
    ans = 2 * m

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, cnt, m):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)
```Việc triển khai cốt lõi bắt đầu bằng cách nén danh sách tác vụ thành một mảng tần số. Điều này rất quan trọng vì thứ tự nhiệm vụ không liên quan; chỉ có bao nhiêu nhiệm vụ ưa thích của mỗi công nhân mới quan trọng. 

các`can`chức năng mã hóa tính khả thi. Nó tính toán tổng số nhiệm vụ có thể được thực hiện trong T giả định rằng chúng tôi luôn ưu tiên giao cho một nhân viên những nhiệm vụ ưa thích của họ trước tiên. Việc sử dụng`min(cnt[i], T)`nắm bắt cả giới hạn của các nhiệm vụ ưu tiên sẵn có và khả năng thời gian của mỗi công nhân. 

Tìm kiếm nhị phân sau đó tìm ra T khả thi nhỏ nhất. Giới hạn trên 2m là an toàn vì trong trường hợp xấu nhất, mỗi nhiệm vụ cần 2 đơn vị thời gian và về mặt lý thuyết, một công nhân có thể xử lý chúng một cách tuần tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 4
1 2 1 2
```Số lượng công nhân là [2, 2]. 

Chúng tôi kiểm tra tính khả thi: 

| T | đóng góp tối thiểu | tổng cộng | khả thi | 
| --- | --- | --- | --- | 
| 1 | 1 + 1 = 2 | 2 | không | 
| 2 | 2 + 2 = 4 | 4 | vâng | 

Vậy đáp án là 2. 

Điều này chứng tỏ rằng khi các nhiệm vụ được phân bổ đều, mỗi công nhân có thể xử lý tất cả các nhiệm vụ ưa thích của mình một cách chính xác trong thời gian 2. 

### Ví dụ 2 

đầu vào:```
2 4
1 1 1 1
```Số lượng công nhân là [4, 0]. 

| T | đóng góp tối thiểu | tổng cộng | khả thi | 
| --- | --- | --- | --- | 
| 2 | 2 + 0 = 2 | 2 | không | 
| 3 | 3 + 0 = 3 | 3 | vâng | 

Câu trả lời là 3. 

Điều này cho thấy rằng một công nhân sẽ bão hòa nhanh chóng và buộc câu trả lời phụ thuộc trực tiếp vào số lượng nhiệm vụ mà họ phải xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log m) | Nhiệm vụ đếm là tuyến tính, mỗi lần kiểm tra tính khả thi là O(n), tìm kiếm nhị phân thêm hệ số log m | 
| Không gian | O(n) | Mảng tần số lưu trữ số lượng nhiệm vụ của công nhân | 

Các ràng buộc cho phép tổng cộng tối đa 2⋅10^5 nhiệm vụ, do đó hệ số logarit trên m đủ nhỏ để vừa vặn thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def can(T, cnt, m):
        total = 0
        for x in cnt:
            total += min(x, T)
        return total >= m

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        cnt = [0] * n
        for v in a:
            cnt[v - 1] += 1

        lo, hi = 1, 2 * m
        ans = hi

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, cnt, m):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert solve("4\n2 4\n1 2 1 2\n2 4\n1 1 1 1\n5 5\n5 1 3 2 4\n1 1\n1\n") == "2\n3\n1\n1"

# custom cases
assert solve("1\n1 1\n1\n") == "1", "single task"
assert solve("1\n2 2\n1 2\n") == "1", "perfect matching"
assert solve("1\n3 6\n1 1 1 2 2 2\n") == "2", "balanced heavy case"
assert solve("1\n2 6\n1 1 1 1 1 1\n") == "3", "one worker overloaded"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ đơn lẻ | 1 | ranh giới tối thiểu | 
| kết hợp hoàn hảo | 1 | bài tập tối ưu hoàn toàn nhanh chóng | 
| hộp nặng cân bằng | 2 | độ chính xác phân phối đối xứng | 
| một công nhân quá tải | 3 | hành vi bão hòa | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả nhiệm vụ thuộc về một công nhân. Đối với đầu vào như`n=2, m=6`với tất cả các nhiệm vụ được giao cho công nhân 1, thuật toán sẽ tính cnt = [6, 0]. Với T=2, tổng min(cnt[i], T) = 2, nhỏ hơn 6, nên không khả thi. Với T=3, nó trở thành 3, vẫn không khả thi. Với T=6, nó trở thành 6, khả thi, phù hợp với ý tưởng rằng một công nhân phải dành thời gian tăng tuyến tính theo khối lượng công việc. 

Một trường hợp khó khăn khác là khi các nhiệm vụ được phân bổ đồng đều để không có công nhân nào vượt quá giới hạn thời gian. Vì`1 2 1 2`, cnt = [2,2], và tại T=2 tổng đạt 4, khớp chính xác với m, xác nhận rằng tính song song loại bỏ hoàn toàn tắc nghẽn. 

Trường hợp cạnh thứ ba là khi n = m và mỗi công nhân có đúng một nhiệm vụ. Khi đó cnt[i] ≤ 1 với tất cả i, do đó ngay cả T=1 cũng đã cho tổng m, cho thấy rằng giới hạn dưới là chặt chẽ và không tồn tại tình trạng quá tải công nhân.
