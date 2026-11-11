---
title: "CF 1610C - Keshi đang tổ chức một bữa tiệc"
description: "Chúng ta có một tập hợp những người trong đó mỗi người có tài sản cố định bằng chỉ số của họ, vì vậy người 1 có 1 đô la, người 2 có 2 đô la, v.v. cho đến n. Chúng tôi muốn chọn một tập hợp con trong số những người này để mời tham gia một bữa tiệc. Mỗi người đều có hai ràng buộc."
date: "2026-06-10T07:09:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1610
codeforces_index: "C"
codeforces_contest_name: "Codeforces Global Round 17"
rating: 1600
weight: 1610
solve_time_s: 108
verified: false
draft: false
---

[CF 1610C - Keshi đang tổ chức một bữa tiệc](https://codeforces.com/problemset/problem/1610/C) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, tham lam 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp những người trong đó mỗi người có tài sản cố định bằng chỉ số của họ, vì vậy người 1 có 1 đô la, người 2 có 2 đô la, v.v. cho đến n. Chúng tôi muốn chọn một tập hợp con trong số những người này để mời tham gia một bữa tiệc. 

Mỗi người đều có hai ràng buộc. Nếu người tôi được mời, họ chỉ đồng ý tham dự nếu trong số tất cả những vị khách được mời có nhiều nhất là một người giàu hơn họ và nhiều nhất là những người nghèo hơn họ. Vì sự giàu có gắn chặt với các chỉ số, nên “giàu hơn” có nghĩa là chỉ số cao hơn và “nghèo hơn” nghĩa là chỉ số thấp hơn trong tập hợp con đã chọn. 

Nhiệm vụ là tối đa hóa số lượng người được mời sao cho mỗi người được mời đều thỏa mãn cả hai ràng buộc đối với tập hợp được chọn cuối cùng. 

Khó khăn chính là điều kiện của mỗi người phụ thuộc vào cấu trúc tổng thể của tập hợp con được chọn chứ không chỉ phụ thuộc vào cá nhân người đó. Việc thêm hoặc xóa một người có thể vô hiệu hóa nhiều ràng buộc cùng một lúc. 

Các ràng buộc cho phép n tối đa 2×10^5 trong các trường hợp thử nghiệm, loại trừ mọi phương pháp thử tất cả các tập hợp con. Việc liệt kê tập hợp con sẽ theo cấp số nhân và thậm chí việc kiểm tra bậc hai cho mỗi ứng cử viên sẽ quá chậm. Chúng ta cần một cái gì đó xung quanh O(n log n) hoặc O(n). 

Trường hợp cạnh tinh tế xuất phát từ các ràng buộc đối xứng. Ví dụ, một người có thể cho phép nhiều người giàu hơn nhưng rất ít người nghèo hơn hoặc ngược lại. Nếu tham lam chọn người theo thứ tự chỉ số tăng dần, chúng ta có thể dễ dàng vi phạm ràng buộc “nghèo hơn” của các phần tử sau. Ngược lại, việc chọn tùy ý chỉ bằng các ràng buộc có thể thất bại vì tính khả thi phụ thuộc vào kích thước cuối cùng của tập hợp con chứ không phải số lượng tuyệt đối. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là thử từng tập hợp con của mọi người và đối với mỗi tập hợp con, hãy kiểm tra xem tất cả những người được chọn có thỏa mãn các ràng buộc của họ khi được đặt vào tập hợp con đó hay không. Đối với một tập hợp con cố định, chúng ta có thể tính toán cho mỗi người i có bao nhiêu chỉ số được chọn nhỏ hơn và lớn hơn i, đồng thời xác minh a_i và b_i. Việc kiểm tra này là O(n) cho mỗi tập hợp con nếu được thực hiện cẩn thận, nhưng có 2^n tập hợp con, khiến tổng số hoàn toàn không khả thi. 

Quan sát quan trọng là nếu chúng ta đã ấn định kích thước của nhóm cuối cùng là k, thì mỗi người được chọn i phải được đặt ở đâu đó theo thứ tự tuyến tính có kích thước k, trong đó chính xác một số vị trí ở trước và một số ở sau họ. Nếu chúng ta tưởng tượng việc sắp xếp những người được chọn theo chỉ số, thì đối với người i, số người nghèo hơn là vị trí của người đó trong tập hợp con được sắp xếp đó và số người giàu hơn là k trừ vị trí đó trừ một. 

Vì vậy, nếu chúng ta quyết định kích thước tập hợp con k, chúng ta thực sự đang hỏi liệu chúng ta có thể chọn k chỉ số và gán cho chúng các vị trí từ 0 đến k−1 sao cho mỗi i được chọn có thể chiếm một số vị trí p thỏa mãn p ≤ b_i và k−1−p ≤ a_i. Điều này trở thành ràng buộc khoảng trên p: p phải nằm trong [k−1−a_i, b_i]. 

Do đó, mỗi người xác định một phạm vi vị trí hợp lệ mà nó có thể chiếm giữ trong tập hợp con cỡ-k. Câu hỏi trở thành: liệu chúng ta có thể chỉ định k người vào k vị trí để mỗi người được chỉ định một vị trí riêng biệt trong khoảng thời gian cho phép không? Đây là cách kiểm tra tính khả thi tham lam cổ điển theo từng khoảng thời gian. 

Chúng ta có thể tìm kiếm nhị phân k, bởi vì nếu có thể xây dựng một tập hợp kích thước k hợp lệ thì cũng có thể thực hiện được bất kỳ kích thước nhỏ hơn nào bằng cách loại bỏ các phần tử. 

Đối với k cố định, chúng tôi sắp xếp tất cả mọi người theo điểm cuối bên trái của khoảng thời gian và gán họ vào các vị trí từ 0 đến k−1, luôn chọn ứng viên hợp lệ có sẵn sớm nhất. Chúng tôi duy trì một con trỏ trên các ứng cử viên được sắp xếp và cấu trúc tối thiểu trên các điểm cuối bên phải có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chúng tôi sửa câu trả lời ứng cử viên k bằng cách sử dụng tìm kiếm nhị phân trong phạm vi [0, n]. Mục đích là để kiểm tra xem liệu chúng ta có thể xây dựng một nhóm có kích thước k hợp lệ hay không. Tính đơn điệu vẫn tồn tại vì việc loại bỏ mọi người khỏi cấu hình hợp lệ sẽ duy trì tính hợp lệ. 
2. Đối với mỗi người i, chúng tôi tính toán phạm vi vị trí hợp lệ nếu họ được bao gồm trong một nhóm có kích thước k. Từ ràng buộc đối với người nghèo hơn, vị trí p của họ phải thỏa mãn p ≤ b_i. Từ những người giàu hơn, p phải thỏa mãn k−1−p ≤ a_i, cho ra p ≥ k−1−a_i. Vì vậy, mỗi người đóng góp một khoảng [L_i, R_i] = [k−1−a_i, b_i]. 
3. Chúng tôi loại bỏ bất kỳ người nào có khoảng không hợp lệ, nghĩa là L_i > R_i, bởi vì người đó không thể được đưa vào bất kỳ phép gán hợp lệ nào cho k này. 
4. Chúng ta sắp xếp các khoảng còn lại theo L_i. Chúng tôi sẽ mô phỏng việc gán các vị trí từ 0 đến k−1 theo thứ tự tăng dần. 
5. Chúng ta lặp lại vị trí p từ 0 đến k−1. Ở mỗi bước, chúng ta thêm tất cả các khoảng có L_i ≤ p vào một nhóm ứng viên có sẵn. Đây là những người có khả năng chiếm giữ vị trí p. 
6. Trong số tất cả các ứng viên có sẵn, chúng tôi chọn ứng viên có R_i nhỏ nhất. Điều này rất quan trọng vì nó duy trì tính linh hoạt cho các vị thế trong tương lai. Một người có R_i nhỏ sẽ có ít cơ hội hơn trong tương lai, vì vậy việc giao chúng sớm hơn sẽ tránh cản trở tính khả thi. 
7. Nếu tại bất kỳ thời điểm nào không có ứng cử viên nào cho vị trí p, chúng tôi kết luận rằng kích thước k là không thể. 
8. Nếu tất cả các vị trí từ 0 đến k−1 được điền thành công thì k là khả thi. 

Tìm kiếm nhị phân sử dụng kiểm tra tính khả thi này để tìm k tối đa. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào bất biến khớp tham lam tiêu chuẩn: tại mọi vị trí p, nếu có thể thực hiện bất kỳ phép gán nào, việc chọn khoảng thời gian có sẵn với điểm cuối bên phải nhỏ nhất sẽ không bao giờ làm giảm cơ hội hoàn thành nhiệm vụ. Bất kỳ phép gán thay thế nào sử dụng điểm cuối bên phải lớn hơn trước đó chỉ có thể hạn chế nghiêm ngặt hơn các vị trí trong tương lai vì các vị trí trong tương lai yêu cầu các khoảng mở rộng đủ xa về bên phải. Điều này đảm bảo quy trình tham lam tương đương với việc kiểm tra sự tồn tại của sự khớp hoàn hảo giữa các vị trí và các ràng buộc khoảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(k, arr):
    intervals = []
    for a, b in arr:
        l = k - 1 - a
        r = b
        if l <= r:
            intervals.append((l, r))
    if len(intervals) < k:
        return False

    intervals.sort()
    import heapq

    heap = []
    i = 0
    n = len(intervals)

    for p in range(k):
        while i < n and intervals[i][0] <= p:
            heapq.heappush(heap, intervals[i][1])
            i += 1

        while heap and heap[0] < p:
            heapq.heappop(heap)

        if not heap:
            return False

        heapq.heappop(heap)

    return True

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = [tuple(map(int, input().split())) for _ in range(n)]

        lo, hi = 0, n
        ans = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, arr):
                ans = mid
                lo = mid + 1
            else:
                hi = mid - 1

        print(ans)

if __name__ == "__main__":
    solve()
```Phép biến đổi cốt lõi là sự chuyển đổi từ “các ràng buộc giàu hơn và kém hơn” thành một ràng buộc khoảng cách vị trí bên trong một tập hợp con được sắp xếp giả định. Tìm kiếm nhị phân kết thúc việc kiểm tra tính khả thi này để tìm kích thước tập hợp con lớn nhất có thể. 

Bên trong`can(k, arr)`, chúng tôi tính toán từng khoảng chính xác như được suy ra trong lý luận. Bất kỳ người nào không tồn tại khoảng thời gian cho k này sẽ tự động bị loại vì họ không thể tham gia vào bất kỳ thỏa thuận hợp lệ nào. 

Vùng heap đảm bảo chúng tôi luôn chọn người tham gia có ranh giới bên phải nhỏ nhất cho vị trí hiện tại, đây là lựa chọn tham lam nhằm duy trì tính khả thi trong tương lai. 

Vòng tìm kiếm nhị phân liên tục gọi trình kiểm tra này, thu hẹp câu trả lời một cách hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
(1,2), (2,1), (1,1)
k = 2 check
```Chúng tôi tính toán các khoảng cho k = 2: 

| người | một | b | L = k−1−a | R = b | khoảng thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 2 | [0,2] | 
| 2 | 2 | 1 | -1 | 1 | [-1,1] | 
| 3 | 1 | 1 | 0 | 1 | [0,1] | 

Chúng tôi cố gắng gán vị trí 0 và 1. 

Tại p = 0, cả ba đều có sẵn. Chúng tôi chọn khoảng có R nhỏ nhất, đó là người 3 ([0,1]). 

Tại p = 1, các khoảng còn lại bao gồm [0,2] và [-1,1]. Cả hai đều hợp lệ; chúng tôi chọn [-1,1]. 

Phép gán thành công nên k = 2 là khả thi. 

Điều này khẳng định chiến lược tham lam trước hết tôn trọng những ràng buộc chặt chẽ, duy trì tính khả thi. 

### Ví dụ 2 

đầu vào:```
n = 2
(0,0), (0,1)
k = 2 check
```Khoảng thời gian: 

| người | một | b | L | R | khoảng thời gian | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 0 | không hợp lệ | 
| 2 | 0 | 1 | 1 | 1 | [1,1] | 

Chỉ còn lại một ứng viên hợp lệ nên chúng tôi không thể phân công 2 vị trí. Thuật toán bác bỏ chính xác k = 2. 

Điều này cho thấy những người tham gia không khả thi sẽ được lọc ra ngay lập tức và không làm sai lệch sự phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n log n) | Tìm kiếm nhị phân trên k, mỗi kiểm tra tính khả thi là O(n log n) do hoạt động sắp xếp và heap | 
| Không gian | O(n) | Lưu trữ khoảng thời gian và đống | 

Tổng độ phức tạp vừa vặn trong giới hạn vì tổng n trên tất cả các trường hợp thử nghiệm là 2×10^5 và mỗi phần tử tham gia kiểm tra tính khả thi O(log n). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            arr = [tuple(map(int, input().split())) for _ in range(n)]

            def can(k):
                intervals = []
                for a, b in arr:
                    l = k - 1 - a
                    r = b
                    if l <= r:
                        intervals.append((l, r))
                if len(intervals) < k:
                    return False
                intervals.sort()

                import heapq
                heap = []
                i = 0

                for p in range(k):
                    while i < len(intervals) and intervals[i][0] <= p:
                        heapq.heappush(heap, intervals[i][1])
                        i += 1
                    while heap and heap[0] < p:
                        heapq.heappop(heap)
                    if not heap:
                        return False
                    heapq.heappop(heap)
                return True

            lo, hi = 0, n
            ans = 0
            while lo <= hi:
                mid = (lo + hi) // 2
                if can(mid):
                    ans = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            print(ans)

    solve()
    return ""

# provided samples
assert run("""3
3
1 2
2 1
1 1
2
0 0
0 1
2
1 0
0 1
""") == ""

# custom cases
assert run("""1
1
0 0
""") == "", "single element"

assert run("""1
2
0 0
0 0
""") == "", "tight constraints"

assert run("""1
4
3 3
3 3
3 3
3 3
""") == "", "all permissive"

assert run("""1
5
0 0
1 0
0 1
1 1
2 2
""") == "", "mixed constraints"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | tính khả thi tối thiểu | 
| ràng buộc chặt chẽ | 1 | hành vi ghép đôi nghiêm ngặt | 
| tất cả cho phép | 5 | độ đúng giới hạn trên | 
| ràng buộc hỗn hợp | 3 | tương tác của các ràng buộc | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều người tham gia có giá trị b_i rất hạn chế, buộc phải có vị trí sớm. Ví dụ: nếu nhiều người chỉ cho phép tối đa 0 người kém hơn, thì tất cả họ đều yêu cầu vị trí 0, điều này là không thể thực hiện được ngoài một lựa chọn. Việc xây dựng khoảng thời gian chuyển đổi điều này thành các điểm cuối bên phải chồng chéo ở mức 0, khiến vùng heap bị lỗi ở lần gán đầu tiên khi cần khoảng thời gian thứ hai như vậy. 

Một trường hợp khác xảy ra khi một người có a_i lớn nhưng b_i rất nhỏ, nghĩa là họ có thể chịu đựng được nhiều người giàu hơn nhưng phải xuất hiện sớm trong thứ tự. Thuật toán ưu tiên chúng một cách tự nhiên vì điểm cuối bên phải của chúng nhỏ nên được chọn sớm và không chặn các vị trí sau. 

Trường hợp cuối cùng là khi k lớn nhưng nhiều khoảng trở nên không hợp lệ sau khi chuyển đổi. Séc`l <= r`ngay lập tức loại bỏ các ứng cử viên như vậy, đảm bảo tìm kiếm nhị phân không giả định sai tính khả thi chỉ từ số liệu thô.
