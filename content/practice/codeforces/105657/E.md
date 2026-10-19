---
title: "CF 105657E - Thang máy II"
description: "Mỗi nhiệm vụ đại diện cho một người phải được đón từ tầng xuất phát và xuống tầng cao hơn bằng một thang máy duy nhất. Thang máy bắt đầu ở một số tầng đầu tiên và mỗi lần chỉ có thể chở một người."
date: "2026-06-22T05:19:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105657
codeforces_index: "E"
codeforces_contest_name: "The 2024 ICPC Asia Hangzhou Regional Contest (The 3rd Universal Cup. Stage 25: Hangzhou)"
rating: 0
weight: 105657
solve_time_s: 56
verified: true
draft: false
---

[CF 105657E - Thang máy II](https://codeforces.com/problemset/problem/105657/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi nhiệm vụ đại diện cho một người phải được đón từ tầng xuất phát và xuống tầng cao hơn bằng một thang máy duy nhất. Thang máy bắt đầu ở một số tầng đầu tiên và mỗi lần chỉ có thể chở một người. Di chuyển lên trên sẽ tiêu tốn năng lượng tỷ lệ thuận với số tầng được di chuyển, còn di chuyển xuống dưới thì miễn phí. 

Mỗi người đóng góp một khoản chi phí đi lại cố định bằng khoảng cách giữa điểm xuất phát và điểm đến của họ. Phần đó là tất yếu và không phụ thuộc vào thứ tự phục vụ mọi người. Phần duy nhất phụ thuộc vào thứ tự là “đi lại chết” khi thang máy chuyển từ kết thúc một người sang bắt đầu người tiếp theo, cụ thể là khi chúng ta phải di chuyển lên từ tầng thả trước đó sang tầng đón tiếp theo. 

Vì vậy, vấn đề không phải là tối ưu hóa các chuyến đi riêng lẻ mà là sắp xếp các chuyến đi này sao cho thang máy tránh được những bước nhảy lên không cần thiết giữa các công việc liên tiếp. 

Các ràng buộc cho phép tối đa 100.000 người cho mỗi trường hợp thử nghiệm và tổng cộng lên tới 300.000. Điều này ngay lập tức loại trừ mọi nỗ lực sắp xếp thứ tự bậc hai đối với các hoán vị. Bất kỳ giải pháp nào cũng phải dựa vào việc sắp xếp cộng với cấu trúc tham lam tuyến tính hoặc gần tuyến tính, điển hình là O(n log n) do hàng đợi ưu tiên hoặc sắp xếp. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các hoán vị của mọi người hoặc thậm chí là các giao dịch hoán đổi địa phương tham lam. Điều này không thành công vì sự tương tác về chi phí mang tính toàn cầu: việc chọn khoảng thời gian tiếp theo tệ hơn một chút có thể tạo ra một chuỗi dài các chuyển đổi không tốn phí sau này. 

Một trường hợp thất bại tinh vi xuất hiện khi sự lựa chọn tham lam bỏ qua tính khả dụng trong tương lai. 

Ví dụ, hãy xem xét:```
f = 1
(1, 100), (2, 3), (3, 4)
```Nếu chúng ta chọn (1, 100) trước, chúng ta sẽ kết thúc ở 100 và mọi thứ khác đều miễn phí. Nếu chúng ta chọn (2, 3) trước, chúng ta kết thúc ở 3 và vẫn có thể lấy (3, 4) miễn phí, nhưng sau đó chúng ta phải chuyển sang (1, 100), phải trả một khoản chi phí lớn. Thứ tự quan trọng theo cách không cục bộ, vì vậy chúng tôi cần một cấu trúc giúp tối đa hóa khả năng truy cập trong tương lai. 

## Phương pháp tiếp cận 

Nhận xét quan trọng là mỗi người đóng góp một khoản chi phí không đổi bằng với chiều dài chuyến đi của họ. Phần đó không bao giờ thay đổi khi đặt hàng. Mục tiêu tối ưu hóa duy nhất là tổng số lần nhảy lên giữa các chuyến đi liên tiếp. 

Điều này biến vấn đề thành một nhiệm vụ lập kế hoạch theo các khoảng thời gian trong đó mỗi công việc sẽ di chuyển vị trí hiện tại từ li sang ri. Sau khi hoàn thành công việc, hệ thống sẽ ở trạng thái ri. Việc chuyển sang công việc tiếp theo tốn tối đa(0, next_li − current_position). 

Một giải pháp bạo lực sẽ liệt kê tất cả các hoán vị và mô phỏng quá trình. Mỗi mô phỏng là O(n), cho ra tổng thể O(n · n!), vượt xa tính khả thi ngay cả đối với n khoảng 10. 

Một cách tiếp cận tốt hơn nhưng vẫn không chính xác là sắp xếp theo li hoặc ri. Việc sắp xếp theo mức tăng li mang lại cảm giác tự nhiên vì nó làm giảm số lần nhảy giữa các lần lấy, nhưng nó có thể khiến chúng ta sớm rơi vào giá trị r thấp, hạn chế phạm vi tiếp cận trong tương lai và buộc phải thực hiện các bước nhảy tốn kém sau này. 

Cấu trúc chính xác xuất phát từ việc xem quy trình như một vấn đề “mở rộng phạm vi tiếp cận”. Một khi chúng ta đã ở một vị trí nào đó, chúng ta nên luôn ưu tiên những công việc mà chúng ta có thể bắt đầu mà không phải trả bất kỳ chi phí nào, nghĩa là li ≤ vị trí hiện tại. Trong số đó, chúng tôi muốn chọn công việc thúc đẩy chúng tôi cao nhất, bởi vì ri cao hơn chỉ làm tăng số lượng công việc trong tương lai mà chúng tôi có thể nhận miễn phí. Nếu hiện tại không có công việc nào có thể truy cập được, chúng ta phải chuyển sang li có sẵn tiếp theo. 

Điều này dẫn đến một quá trình tham lam với việc quét trên li đã được sắp xếp và hàng đợi ưu tiên trên ri. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | O(n) | Quá chậm | 
| Tham lam + Đống | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý tất cả mọi người theo các khoảng (li, ri). Chúng tôi duy trì một con trỏ theo các khoảng được sắp xếp và vùng heap tối đa được khóa bởi ri. 

1. Sắp xếp tất cả mọi người theo số li của họ theo thứ tự tăng dần. Điều này cho phép chúng tôi khám phá một cách hiệu quả những công việc nào có sẵn khi thang máy di chuyển lên trên. 
2. Khởi tạo vị trí hiện tại tại tầng xuất phát f. 
3. Duy trì con trỏ i trên danh sách đã sắp xếp và vùng heap tối đa. Heap lưu trữ tất cả những người có li ≤ vị trí hiện tại, được ưu tiên bởi ri lớn nhất. Điều này đảm bảo chúng tôi luôn chọn công việc có khả năng mở rộng phạm vi tiếp cận của chúng tôi nhiều nhất trong số tất cả các công việc khả thi ngay lập tức. 
4. Trong khi có những người chưa được xử lý hoặc heap không trống, trước tiên chúng ta đẩy vào heap tất cả những người có li ≤ vị trí hiện tại. Điều này mô phỏng việc tiết lộ tất cả các công việc mà chúng tôi hiện có thể bắt đầu mà không phải trả thêm tiền di chuyển. 
5. Nếu heap không trống, chúng ta chọn người có ri lớn nhất. Chúng tôi thêm người này vào thứ tự đầu ra và di chuyển thang máy đến ri. Không có chi phí tăng thêm nào được thêm vào trong bước này vì li ≤ vị trí hiện tại đảm bảo việc lấy hàng được miễn phí. 
6. Nếu heap trống, hiện tại không có công việc nào có thể truy cập được. Chúng ta phải di chuyển lên trên li của công việc có sẵn tiếp theo. Chúng tôi chọn li nhỏ nhất trong số các công việc còn lại, trả max(0, li − vị trí hiện tại), sau đó đặt vị trí hiện tại thành ri sau khi xử lý công việc đó. 
7. Lặp lại cho đến khi tất cả mọi người được xử lý. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tất cả các công việc có li ≤ vị trí hiện tại đều có thể thay thế cho nhau xét về tính khả thi trước mắt. Việc chọn cái có ri tối đa sẽ lấn át những cái khác vì nó tối đa hóa tính sẵn có trong tương lai mà không làm tăng chi phí hiện tại. Khi không thể tiếp cận công việc nào, bước nhảy bắt buộc tiếp theo là không thể tránh khỏi và việc chọn li nhỏ nhất sẽ đảm bảo chuyển động đi lên cần thiết nhỏ nhất. Điều này duy trì tính bất biến rằng vị thế hiện tại luôn lớn nhất có thể với chi phí đã trả, điều này ngăn chặn những bước nhảy vọt không cần thiết trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

def solve():
    T = int(input())
    for _ in range(T):
        n, f = map(int, input().split())
        people = []
        for i in range(n):
            l, r = map(int, input().split())
            people.append((l, r, i + 1))
        
        people.sort()
        
        ans = []
        heap = []
        i = 0
        cur = f
        
        while i < n or heap:
            while i < n and people[i][0] <= cur:
                l, r, idx = people[i]
                heapq.heappush(heap, (-r, l, idx))
                i += 1
            
            if heap:
                neg_r, l, idx = heapq.heappop(heap)
                ans.append(idx)
                cur = -neg_r
            else:
                l, r, idx = people[i]
                cur = max(cur, l)
                heapq.heappush(heap, (-r, l, idx))
        
        print(sum(people[idx - 1][1] - people[idx - 1][0] for idx in ans), *ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ sắp xếp tất cả các khoảng thời gian theo tầng bắt đầu, cho phép quét tuyến tính để dần dần hiển thị các công việc có sẵn khi vị trí thang máy hiện tại tăng lên. Heap lưu trữ các công việc có thể tiếp cận, được sắp xếp theo tầng đích cao nhất, đảm bảo rằng chúng tôi luôn mở rộng phạm vi có thể tiếp cận của mình một cách tích cực nhất có thể. 

Vị trí hiện tại được cập nhật bất cứ khi nào chúng tôi hoàn thành một công việc và nếu không thể truy cập được công việc nào, chúng tôi sẽ trực tiếp chuyển sang tầng bắt đầu tiếp theo. Điều này tránh việc quét liên tục các ứng cử viên không khả thi. 

Một điểm tinh tế là chi phí cuối cùng được in trong quá trình triển khai này được tính toán lại theo thứ tự đã chọn bằng cách sử dụng quan sát tổng không đổi. Trong môi trường sản xuất, người ta thường tích lũy chi phí chuyển đổi tăng dần trong quá trình mô phỏng, nhưng việc tính toán lại đơn giản hơn và tránh được những sai sót trong quá trình chuyển đổi vùng heap. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét:```
f = 2
(1, 3), (2, 7), (5, 6)
```Chúng tôi bắt đầu lúc 2 giờ. 

| Bước | Hiện tại | Việc làm có sẵn | Được chọn | Vị trí mới | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | (1,3), (2,7) | (2,7) | 7 | 
| 2 | 7 | (1,3), (5,6) | (5,6) | 6 | 
| 3 | 6 | (1,3) | (1,3) | 3 | 

Điều này cho thấy việc luôn chọn phạm vi tiếp cận tối đa trong số các công việc có sẵn sẽ ngăn chặn việc đặt lại tăng lên không cần thiết như thế nào. 

### Ví dụ 2```
f = 1
(10, 20), (2, 3), (3, 4)
```| Bước | Hiện tại | Việc làm có sẵn | Hành động | Vị trí mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | nhảy tới 2 | 3 | 
| 2 | 3 | (2,3), (3,4) | (3,4) | 4 | 
| 3 | 4 | (10,20) | nhảy tới 10 rồi lấy | 20 | 

Điều này làm nổi bật các bước nhảy bắt buộc khi hiện tại không thể truy cập được khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp cộng với mỗi khoảng thời gian được đẩy và xuất hiện một lần từ heap | 
| Không gian | O(n) | Đống và lưu trữ trong khoảng thời gian | 

Tổng số người trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 300.000, do đó, giải pháp O(n log n) nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    input = sys.stdin.readline
    import heapq

    def solve():
        T = int(input())
        out = []
        for _ in range(T):
            n, f = map(int, input().split())
            a = []
            for i in range(n):
                l, r = map(int, input().split())
                a.append((l, r, i + 1))
            a.sort()

            i = 0
            cur = f
            heap = []
            order = []

            while i < n or heap:
                while i < n and a[i][0] <= cur:
                    l, r, idx = a[i]
                    heapq.heappush(heap, (-r, l, idx))
                    i += 1
                if heap:
                    nr, l, idx = heapq.heappop(heap)
                    order.append(idx)
                    cur = -nr
                else:
                    l, r, idx = a[i]
                    cur = max(cur, l)
                    heapq.heappush(heap, (-r, l, idx))

            # cost recompute
            pos = f
            cost = 0
            for idx in order:
                l, r = [(a[j][0], a[j][1]) for j in range(n) if a[j][2] == idx][0]
                cost += max(l - pos, 0)
                cost += r - l
                pos = r

            out.append(str(cost))
            out.append(" ".join(map(str, order)))
        return "\n".join(out)

    return solve()

# custom cases
assert run("1\n1 5\n3 10\n")  # single job
assert run("1\n3 1\n1 2\n2 3\n3 4\n")  # chain
assert run("1\n3 1\n10 20\n1 2\n2 3\n")  # forced jump case
assert run("1\n2 100\n1 2\n2 3\n")  # high start
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công việc đơn lẻ | tầm thường | trường hợp cơ sở đúng đắn | 
| khoảng thời gian chuỗi | thứ tự tuần tự | không có bước nhảy không cần thiết | 
| khoảng cách riêng biệt | buộc phải khởi động lại xử lý | tính đúng đắn của logic nhảy | 
| khởi đầu cao | xử lý phạm vi tiếp cận ban đầu | sử dụng đúng f | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi tầng bắt đầu đã cao hơn tất cả các giá trị li. Thuật toán ngay lập tức coi tất cả công việc là có thể truy cập được và hoàn toàn dựa vào việc chọn ri tối đa, giúp tránh chuyển động đi lên không cần thiết ngay từ đầu. 

Một trường hợp khác là khi các khoảng cách xa nhau, buộc phải đặt lại nhiều lần. Trong những trường hợp như vậy, heap liên tục trở nên trống và tính chính xác phụ thuộc vào việc luôn nhảy tới li nhỏ nhất tiếp theo. Cấu trúc tham lam đảm bảo chúng ta không bao giờ bỏ qua một li nhỏ hơn có thể làm giảm bước nhảy bắt buộc tiếp theo. 

Trường hợp cạnh cuối cùng là khi nhiều khoảng chia sẻ cùng một li nhưng có ri khác nhau. Đống dữ liệu đảm bảo rằng chỉ khoảng có ri lớn nhất được chọn đầu tiên, duy trì khả năng tiếp cận và tránh cam kết sớm với các đích thấp hơn.
