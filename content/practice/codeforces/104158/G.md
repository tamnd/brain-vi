---
title: "CF 104158G - Kiểu gõ dở tệ"
description: "Chúng tôi được cấp một hàng nhân viên, mỗi nhân viên được liên kết với thời lượng nhập cố định. Có những nhân viên $N$ đang đứng theo thứ tự và chúng ta có thể đặt những chiếc máy tính $M$ trước mặt họ. Tại thời điểm 0, mỗi nhân viên $M$ đầu tiên chiếm một máy tính và bắt đầu gõ."
date: "2026-07-02T01:10:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104158
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 1 (Advanced)"
rating: 0
weight: 104158
solve_time_s: 69
verified: true
draft: false
---

[CF 104158G - Kiểu gõ dở tệ](https://codeforces.com/problemset/problem/104158/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một hàng nhân viên, mỗi nhân viên được liên kết với thời lượng nhập cố định. có$N$nhân viên đang đứng theo thứ tự và chúng tôi có thể đặt$M$máy tính ở phía trước của họ. Tại thời điểm 0, lần đầu tiên$M$mỗi nhân viên chiếm một máy tính và bắt đầu gõ. Bất cứ khi nào có một máy tính trống, nhân viên tiếp theo trong hàng sẽ ngay lập tức lấy chiếc máy tính đó và bắt đầu bài kiểm tra của họ. Điều này tiếp tục cho đến khi tất cả nhân viên đã hoàn thành. Thời điểm kết thúc toàn bộ quá trình là thời điểm nhân viên cuối cùng hoàn thành việc đánh máy. 

Nhiệm vụ là xác định số lượng máy tính nhỏ nhất$M$sao cho toàn bộ hàng đợi kết thúc trong thời hạn nhất định$D$. 

Khía cạnh quan trọng là việc phân công hoàn toàn theo FIFO: máy tính không chọn công việc ngắn nhất còn lại, nó luôn phục vụ người tiếp theo trong hàng. Mỗi máy hoạt động độc lập và liên tục xử lý một chuỗi công việc. 

Những hạn chế$N \le 10^5$Và$t_i \le 10^4$chỉ ra rằng bất kỳ giải pháp nào mô phỏng toàn bộ quy trình cho mỗi ứng viên$M$phải có hiệu quả. Một mô phỏng đơn giản liên tục tăng thời gian theo từng bước hoặc quét hàng đợi theo cách không hiệu quả sẽ quá chậm. Ngay cả việc mô phỏng mỗi sự kiện hoàn thành của nhân viên mà không có đống dữ liệu cũng có thể giảm xuống$O(N^2)$trong trường hợp xấu nhất là không khả thi. 

Một trường hợp cạnh tinh tế phát sinh khi$M = 1$. Khi đó thời gian hoàn thành chỉ đơn giản là tổng của tất cả$t_i$và bất kỳ trực giác lập kế hoạch tham lam nào cũng phải phù hợp với ranh giới này. Một trường hợp cạnh khác là khi$M \ge N$, nơi mỗi nhân viên đều có một máy tính chuyên dụng và câu trả lời sẽ trở thành$\max(t_i)$. Bất kỳ triển khai nào không xử lý rõ ràng hoặc bao gồm các trường hợp này một cách tự nhiên đều có nguy cơ xảy ra hành vi ranh giới không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra một số lượng máy tính cố định$M$và mô phỏng toàn bộ quá trình. Chúng tôi chỉ định đầu tiên$M$công việc, sau đó liên tục chọn máy trống sớm nhất, giao cho công việc tiếp theo và cập nhật tính khả dụng của nó. Sử dụng hàng đợi ưu tiên, mỗi nhiệm vụ sẽ tốn chi phí$O(\log M)$, do đó mô phỏng tất cả$N$chi phí việc làm$O(N \log M)$. Điều này đúng vì nó mô hình hóa quy trình một cách trung thực, nhưng nó chỉ hữu ích một lần cho mỗi ứng viên$M$. 

Để giải bài toán thực tế ta cần tìm giá trị nhỏ nhất$M$thỏa mãn điều kiện thời hạn. Quan sát quan trọng là nếu một cấu hình với$M$máy móc hoàn thành trong thời gian$D$, thì số lượng máy lớn hơn chỉ có thể làm giảm thời gian chờ đợi. Không có công việc nào trở nên chậm hơn khi chúng ta thêm nhiều máy chủ song song, vì vậy tính khả thi là đơn điệu$M$. 

Cấu trúc đơn điệu này cho phép tìm kiếm nhị phân trên$M$. Đối với một cố định$M$, chúng tôi mô phỏng quy trình bằng cách sử dụng rất nhiều thời gian khả dụng của máy. Mỗi công việc được giao cho máy hoàn thiện sớm nhất và chúng tôi theo dõi thời gian hoàn thành tối đa. Tìm kiếm nhị phân làm giảm số lượng mô phỏng xuống$O(\log N)$, mang lại tổng độ phức tạp của$O(N \log N \log N)$, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu cho mỗi M |$O(N^2 \log N)$|$O(N)$| Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng đống |$O(N \log N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta coi vấn đề này như một vấn đề quyết định: đã cho$M$, chúng ta có thể hoàn thành trong thời gian được không$D$? 

1. Cố định ứng viên$M$và mô phỏng quá trình lập kế hoạch bằng cách sử dụng một đống kích thước tối thiểu$M$. Mỗi phần tử heap biểu thị thời điểm máy tính rảnh rỗi. Ban đầu, tất cả$M$máy tính rảnh vào thời điểm 0. 
2. Lặp lại các nhân viên theo thứ tự. Đối với mỗi nhân viên$i$, trích xuất giá trị nhỏ nhất từ ​​heap, đại diện cho máy tính có sẵn sớm nhất. Đây là máy sẽ hoàn thành tải hiện tại trước tiên. 
3. Phân công nhân viên$i$vào máy tính đó bằng cách thêm$t_i$đến thời điểm sẵn sàng của nó và đẩy nó trở lại heap. Điều này duy trì tính bất biến là vùng heap luôn chứa thời gian rảnh tiếp theo của mỗi máy. 
4. Sau khi xử lý tất cả nhân viên, câu trả lời cuối cùng cho cấu hình này là giá trị tối đa trong số tất cả các phần tử heap, biểu thị thời gian hoàn thành cuối cùng. 
5. Nếu thời gian hoàn thành này nhiều nhất$D$, sau đó$M$là khả thi. 
6. Tìm kiếm nhị phân trên$M$từ 1 đến$N$, sử dụng kiểm tra tính khả thi để hướng dẫn tìm kiếm theo các giá trị hợp lệ nhỏ hơn. 

Tại sao nó hoạt động dựa trên đặc tính phân phối tải đơn điệu. Tăng dần$M$chỉ có thể giảm hoặc duy trì tải trên mỗi máy vì các công việc trước đây xếp sau các máy bận rộn giờ đây có cơ hội thực hiện sớm hơn. Mô phỏng heap luôn chỉ định từng công việc cho máy có sẵn sớm nhất, đây là lựa chọn tối ưu cục bộ duy nhất phù hợp với ràng buộc FIFO. Vì mỗi nhiệm vụ chỉ phụ thuộc vào thời gian sẵn sàng chứ không phụ thuộc vào các quyết định trong tương lai nên mô phỏng sẽ xác định đầy đủ lịch trình cho một nhiệm vụ nhất định.$M$, và tính khả thi được xác định rõ ràng và đơn điệu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def can(M, t, D):
    # initialize M machines, all free at time 0
    heap = [0] * M
    heapq.heapify(heap)

    for x in t:
        cur = heapq.heappop(heap)
        cur += x
        heapq.heappush(heap, cur)
        if cur > D:
            # optional early exit
            pass

    return max(heap) <= D

def solve():
    N, D = map(int, input().split())
    t = list(map(int, input().split()))

    lo, hi = 1, N

    while lo < hi:
        mid = (lo + hi) // 2
        if can(mid, t, D):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Mô phỏng cốt lõi sử dụng một vùng nhớ tối thiểu trong đó mỗi giá trị sẽ theo dõi thời điểm máy khả dụng. Sự phân công tham lam bị ép buộc bởi mô hình: nhân viên tiếp theo luôn lấy chiếc máy nào rảnh sớm nhất. 

Tìm kiếm nhị phân thu hẹp phạm vi khả thi nhỏ nhất$M$. Chức năng kiểm tra tính toán thời gian hoàn thành thực tế; chúng tôi so sánh nó với$D$. 

Một chi tiết triển khai tinh tế là chúng tôi theo dõi giá trị heap tối đa ở cuối thay vì cố gắng duy trì thời gian kết thúc đang chạy một cách cẩn thận. Điều này tránh được những sai lầm khi suy luận từng phần về các trạng thái trung gian. Một chi tiết khác là chúng ta không thể dựa vào việc dừng sớm bên trong vòng lặp mà không duy trì cẩn thận tính chính xác, vì vậy phiên bản sạch chỉ cần hoàn thành mô phỏng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 151
56 94 95 33 62 28
```Chúng tôi kiểm tra các giá trị ứng viên của$M$. Để minh họa, hãy xem xét$M = 3$. 

| Bước | Trạng thái heap trước | Đã giao$t_i$| Xuất hiện | Trạng thái heap sau | 
| --- | --- | --- | --- | --- | 
| 1 | [0,0,0] | 56 | 0 | [0,0,56] | 
| 2 | [0,0,56] | 94 | 0 | [0,56,94] | 
| 3 | [0,56,94] | 95 | 0 | [56,94,95] | 
| 4 | [56,94,95] | 33 | 56 | [89,94,95] | 
| 5 | [89,94,95] | 62 | 89 | [94,94,95] | 
| 6 | [94,94,95] | 28 | 94 | [94,122,95] | 

Thời gian hoàn thành cuối cùng là 122, nằm trong khoảng 151, vì vậy$M=3$là khả thi. Đang cố gắng$M=2$sẽ khiến máy bị quá tải nặng hơn và vượt quá giới hạn nên đáp án là 3. 

Dấu vết này cho thấy cách heap luôn chỉ định công việc tiếp theo cho máy có sẵn sớm nhất và cách tích lũy tồn đọng khi$M$là nhỏ. 

### Mẫu 2 

đầu vào:```
9 83
10 47 53 9 83 33 15 24 28
```Kiểm tra$M = 5$: 

| Bước | Trạng thái heap trước | Đã giao$t_i$| Xuất hiện | Trạng thái heap sau | 
| --- | --- | --- | --- | --- | 
| 1 | [0,0,0,0,0] | 10 | 0 | [0,0,0,0,10] | 
| 2 | [0,0,0,0,10] | 47 | 0 | [0,0,0,10,47] | 
| 3 | [0,0,0,10,47] | 53 | 0 | [0,0,10,47,53] | 
| 4 | [0,0,10,47,53] | 9 | 0 | [0,9,10,47,53] | 
| 5 | [0,9,10,47,53] | 83 | 0 | [9,10,47,53,83] | 
| 6 | [9,10,47,53,83] | 33 | 9 | [10,33,47,53,83] | 
| 7 | [10,33,47,53,83] | 15 | 10 | [15,33,47,53,83] | 
| 8 | [15,33,47,53,83] | 24 | 15 | [24,33,47,53,83] | 
| 9 | [24,33,47,53,83] | 28 | 24 | [28,33,47,53,83] | 

Thời gian cuối cùng là 83, trùng với thời hạn nên 5 là khả thi. Bất kỳ số nào nhỏ hơn sẽ làm trì hoãn nhiệm vụ 83 lớn hơn nữa, phá vỡ ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N \log N)$|$O(N \log M)$mô phỏng bên trong tìm kiếm nhị phân | 
| Không gian |$O(N)$| đống kích thước$M \le N$| 

Giải pháp phù hợp thoải mái vì$N = 10^5$, do đó thậm chí vài triệu thao tác heap vẫn nằm trong giới hạn. Tìm kiếm nhị phân làm giảm số lượng mô phỏng đầy đủ xuống còn khoảng 17 và mỗi mô phỏng là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def can(M, t, D):
        heap = [0] * M
        heapq.heapify(heap)
        for x in t:
            cur = heapq.heappop(heap)
            cur += x
            heapq.heappush(heap, cur)
        return max(heap) <= D

    def solve():
        N, D = map(int, input().split())
        t = list(map(int, input().split()))
        lo, hi = 1, N
        while lo < hi:
            mid = (lo + hi) // 2
            if can(mid, t, D):
                hi = mid
            else:
                lo = mid + 1
        print(lo)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6 151\n56 94 95 33 62 28\n") == "3"
assert run("9 83\n10 47 53 9 83 33 15 24 28\n") == "5"

# custom cases
assert run("1 10\n7\n") == "1", "single employee"
assert run("5 100\n1 1 1 1 1\n") == "1", "all equal small times"
assert run("5 3\n1 1 1 1 1\n") == "2", "tight deadline forces split"
assert run("4 10\n10 1 1 1\n") == "2", "heavy first job dominates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| công việc đơn lẻ | 1 | trường hợp cơ sở | 
| nhiệm vụ nhỏ thống nhất | 1 | máy tối thiểu đủ | 
| thời hạn chặt chẽ | 2 | tính đúng đắn dưới sự ràng buộc | 
| công việc đầu tiên nặng nề | 2 | xử lý mất cân bằng tải | 

## Vỏ cạnh 

cho$N = 1$, heap có một máy duy nhất và thuật toán ngay lập tức trả về 1, vì thời gian hoàn thành duy nhất là$t_1$, phù hợp với yêu cầu 

Khi tất cả$t_i$bằng nhau và nhỏ, heap phân phối nhiệm vụ đồng đều trên các máy. Ví dụ, với$N = 4$,$t = [2,2,2,2]$, và lớn$D$, mô phỏng cho thấy một máy là đủ nếu$D \ge 8$, nhưng tìm kiếm nhị phân sẽ tìm ra giá trị hợp lệ nhỏ nhất một cách chính xác$M$mà không đánh giá quá cao. 

Khi một nhiệm vụ lớn hơn nhiều so với những nhiệm vụ khác, chẳng hạn như$t = [100,1,1,1]$, vùng heap đảm bảo rằng tác vụ dài sẽ chiếm một máy sớm, trong khi các tác vụ ngắn sẽ lấp đầy các tác vụ khác. Với nhỏ$M$, nhiệm vụ kéo dài sẽ trở thành nút thắt cổ chai và tính khả thi sẽ không thành công trừ khi có đủ máy móc.
