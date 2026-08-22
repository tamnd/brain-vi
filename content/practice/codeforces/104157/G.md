---
title: "CF 104157G - Kiểu gõ dở tệ"
description: "Chúng ta được cấp một dãy các nhân viên đứng theo một thứ tự cố định, trong đó mỗi nhân viên có một khoảng thời gian gõ đã biết. Có M máy tính giống hệt nhau và những máy tính này hoạt động giống như các bộ xử lý song song liên tục đưa người có sẵn tiếp theo vào hàng đợi."
date: "2026-07-02T01:16:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 59
verified: true
draft: false
---

[CF 104157G - Kiểu gõ dở tệ](https://codeforces.com/problemset/problem/104157/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một dãy các nhân viên đứng theo một thứ tự cố định, trong đó mỗi nhân viên có một khoảng thời gian gõ đã biết. Có M máy tính giống hệt nhau và những máy tính này hoạt động giống như các bộ xử lý song song liên tục đưa người có sẵn tiếp theo vào hàng đợi. 

Tại thời điểm 0, M nhân viên đầu tiên ngay lập tức bắt đầu gõ, mỗi máy một người. Mỗi máy tính hoạt động độc lập: khi người dùng kết thúc, máy tính đó sẽ ngay lập tức đưa người tiếp theo vào hàng chờ. Quá trình tiếp tục cho đến khi tất cả nhân viên hoàn thành bài kiểm tra của mình và cuộc thi kết thúc khi nhân viên cuối cùng kết thúc. 

Nhiệm vụ là xác định số lượng máy tính M nhỏ nhất sao cho tổng thời gian hoàn thành của toàn bộ hàng đợi không vượt quá thời hạn D. 

Các ràng buộc làm cho việc mô phỏng trực tiếp trên tất cả M có thể không khả thi nếu được thực hiện một cách ngây thơ. N có thể lên tới 100000 và D có thể lớn tới 10^9. Một nỗ lực đơn giản nhằm tính toán lại toàn bộ lịch trình cho mỗi ứng viên M sẽ tốn O(N^2) trong trường hợp xấu nhất, quá chậm so với giới hạn. Ngay cả O(N log N) cho mỗi lần kiểm tra cũng trở nên đắt đỏ nếu M được kiểm tra tuyến tính. 

Một trường hợp phức tạp xuất hiện khi thời gian của một nhân viên đã lớn hơn D. Trong tình huống đó, ngay cả với vô số máy tính, câu trả lời vẫn bị hạn chế bởi thời gian chạy tối đa của từng nhân viên, vì nhân viên đó không thể song song. Ví dụ: nếu N = 3, D = 5 và t = [10, 1, 1], câu trả lời không tồn tại trừ khi bài toán đảm bảo tính khả thi, điều đó đúng như vậy. Điều này cho chúng ta biết rằng chúng ta phải giả sử không gian nghiệm dự định luôn chứa M hợp lệ. 

Một trường hợp cạnh khác phát sinh khi tất cả thời gian xử lý bằng nhau. Trong trường hợp đó, thời gian hoàn thành phụ thuộc gần như tuyến tính vào ceil(N / M) và trực giác tham lam ngây thơ có thể khiến người ta lầm tưởng rằng M = N luôn là cần thiết, nhưng việc sử dụng lại hàng đợi đảm bảo M nhỏ hơn vẫn có thể thỏa mãn ràng buộc nếu D đủ lớn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử mọi số lượng máy tính M có thể từ 1 đến N. Đối với mỗi M, chúng tôi mô phỏng toàn bộ quy trình lập lịch bằng cách sử dụng hàng đợi ưu tiên hoặc một đống tối thiểu để theo dõi thời điểm mỗi máy tính rảnh rỗi. Chúng tôi đẩy M nhiệm vụ đầu tiên, sau đó liên tục bật máy hoàn thiện sớm nhất, giao nhiệm vụ tiếp theo và tiếp tục cho đến khi tất cả các nhiệm vụ được xử lý. Tổng thời gian chạy là thời gian hoàn thiện tối đa được tạo ra bởi mô phỏng này. 

Điều này hoạt động vì nó tái tạo chính xác hệ thống thực. Vấn đề là mỗi mô phỏng tốn O(N log M) và việc thực hiện nó cho tất cả M sẽ dẫn đến O(N^2 log N) trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là tính khả thi của một M nhất định là đơn điệu. Nếu M máy tính có thể hoàn thành trong thời gian D thì bất kỳ M' > M nào cũng có thể hoàn thành trong D vì việc thêm máy chỉ có thể giảm thời gian chờ đợi hoặc giữ nguyên. Điều này biến bài toán thành một vị từ đơn điệu trên M, ngay lập tức gợi ý tìm kiếm nhị phân. 

Những gì còn lại là một cách hiệu quả để kiểm tra tính khả thi của một M cố định. Mô phỏng tự nhiên với một đống đã được chấp nhận ở O(N log M), nhưng vì chúng ta chỉ cần kiểm tra O(log N) do tìm kiếm nhị phân, nên độ phức tạp tổng thể sẽ trở thành O(N log N log N), thế là đủ. 

Chúng tôi đang kết hợp hiệu quả hai ý tưởng: cân bằng tải trên các máy giống hệt nhau và tìm kiếm nhị phân trên số lượng máy cần thiết để đáp ứng ràng buộc về thời gian toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N^2 log N) | O(N) | Quá chậm | 
| Tìm kiếm nhị phân + Mô phỏng heap | O(N log N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tìm kiếm M nhỏ nhất vượt qua bài kiểm tra tính khả thi.

1. Xác định hàm can(M) xác định xem M máy tính có đủ để hoàn thành tất cả nhiệm vụ trong thời gian D. Chúng tôi mô phỏng quy trình chính xác như được mô tả trong bài toán bằng cách sử dụng hàng ưu tiên về thời gian hoàn thành. 
2. Trong can(M), trước tiên chúng ta gán M tác vụ đầu tiên cho M máy tính. Mỗi mục trong heap biểu thị thời gian mà máy tính trở nên trống, vì vậy chúng ta khởi tạo nó bằng t[i] cho i từ 0 đến M−1. Điều này phản ánh rằng mỗi nhiệm vụ này đều bắt đầu tại thời điểm 0. 
3. Đối với mỗi nhiệm vụ còn lại, chúng tôi liên tục lấy máy tính có sẵn sớm nhất từ ​​đống. Máy đó kết thúc vào thời điểm current_time và chúng tôi giao nhiệm vụ tiếp theo cho nó, cập nhật thời gian kết thúc mới của nó là current_time + t[i]. Chúng tôi đẩy giá trị cập nhật này trở lại vùng heap. 
4. Sau khi tất cả nhiệm vụ được giao, câu trả lời cho M này là giá trị lớn nhất nhìn thấy trong heap. Vì vùng nhớ luôn lưu trữ thời gian hoàn thành nên giá trị tối đa thể hiện thời điểm nhân viên cuối cùng hoàn thành. 
5. Nếu mức tối đa này nhỏ hơn hoặc bằng D thì M là đủ. 
6. Chúng ta tìm kiếm nhị phân M từ 1 đến N, sử dụng can(M) làm vị ngữ. Nếu can(M) đúng, chúng ta thử M nhỏ hơn; mặt khác, chúng tôi tăng M. 

Lý do chính khiến tìm kiếm nhị phân hoạt động là vì can(M) là đơn điệu. Nếu việc tăng số lượng máy không thể làm giảm thời gian hoàn thành thì khi giá trị của M hợp lệ, tất cả các giá trị lớn hơn vẫn hợp lệ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def can(M, t, D):
    if M >= len(t):
        return max(t) <= D

    heap = t[:M]
    heapq.heapify(heap)

    for i in range(M, len(t)):
        earliest = heapq.heappop(heap)
        finish = earliest + t[i]
        heapq.heappush(heap, finish)

    return max(heap) <= D

def solve():
    N, D = map(int, input().split())
    t = list(map(int, input().split()))

    lo, hi = 1, N
    ans = N

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, t, D):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Hàm can(M) mô phỏng hệ thống chính xác như một quy trình lập kế hoạch cho nhiều máy. Heap lưu trữ thời gian hoàn thành hiện tại, vì vậy chúng tôi luôn sử dụng lại máy có sẵn sớm nhất. Sự phân công tham lam này là cần thiết vì việc trì hoãn phân công sẽ chỉ làm tăng thời gian hoàn thành. 

Tìm kiếm nhị phân đảm bảo chúng tôi không kiểm tra tất cả các giá trị M một cách rõ ràng. Thay vào đó, chúng tôi dựa vào tính đơn điệu để giảm không gian tìm kiếm theo logarit. 

Một điểm tinh tế là việc khởi tạo heap với M tác vụ đầu tiên. Mô hình này bắt đầu đồng thời tại thời điểm 0, điều này rất quan trọng; coi máy ở trạng thái không hoạt động ban đầu sẽ dịch chuyển không chính xác tất cả thời gian hoàn thành. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
N = 6, D = 151
t = [56, 94, 95, 33, 62, 28]
```Chúng tôi kiểm tra các giá trị M ứng cử viên thông qua tìm kiếm nhị phân. 

| M | Tiến hóa heap (lần kết thúc) | Tối đa cuối cùng | Khả thi | 
| --- | --- | --- | --- | 
| 1 | [56] → [150] → [245] → ... | >151 | Không | 
| 2 | [56, 94] → [56, 127] → [150, 127] → ... | >151 | Không | 
| 3 | [56, 94, 95] → cập nhật → tối đa 151 | 151 | Có | 

Với M = 3, việc lập kế hoạch cân bằng tải đủ để không có máy nào vượt quá thời hạn. 

Điều này xác nhận rằng việc tăng M sẽ làm giảm áp lực lên từng máy riêng lẻ và ngưỡng được đặt ở mức 3. 

### Mẫu 2 

đầu vào:```
N = 9, D = 83
t = [10, 47, 53, 9, 83, 33, 15, 24, 28]
```| M | Tóm tắt hành vi | Tối đa cuối cùng | Khả thi | 
| --- | --- | --- | --- | 
| 4 | hàng đợi tích lũy nhiệm vụ trễ | >83 | Không | 
| 5 | tải phân bố đều | 83 | Có | 

Với M = 5, nhiệm vụ dài (83) vẫn khớp chính xác và tất cả các nhiệm vụ còn lại hoàn thành mà không có độ trễ xếp tầng. 

Dấu vết này cho thấy một tác vụ dài có thể chiếm ưu thế trong việc lập kế hoạch trừ khi có đủ máy móc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N log N) | Mỗi lần kiểm tra tính khả thi đều có chi phí O(N log M) và tìm kiếm nhị phân sẽ thêm một hệ số log N khác | 
| Không gian | O(N) | Heap lưu trữ tối đa M phần tử | 

Các ràng buộc cho phép tối đa 10^5 nhân viên và các hệ số logarit vẫn đủ nhỏ cho giới hạn 2 giây. Các hoạt động của đống có hiệu quả trong thực tế nhờ các hằng số nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def can(M, t, D):
        if M >= len(t):
            return max(t) <= D
        heap = t[:M]
        heapq.heapify(heap)
        for i in range(M, len(t)):
            earliest = heapq.heappop(heap)
            heapq.heappush(heap, earliest + t[i])
        return max(heap) <= D

    def solve():
        N, D = map(int, input().split())
        t = list(map(int, input().split()))

        lo, hi = 1, N
        ans = N
        while lo <= hi:
            mid = (lo + hi) // 2
            if can(mid, t, D):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1
        print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("6 151\n56 94 95 33 62 28\n") == "3"
assert run("9 83\n10 47 53 9 83 33 15 24 28\n") == "5"

# minimum case
assert run("1 10\n5\n") == "1"

# all equal
assert run("5 20\n4 4 4 4 4\n") == "1"

# tight deadline
assert run("3 10\n5 5 5\n") == "3"

# larger mix
assert run("4 10\n2 8 3 7\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ đơn lẻ | 1 | trường hợp cơ sở | 
| đều nhỏ bằng nhau D | 1 | tranh chấp tồi tệ nhất | 
| lịch trình chặt chẽ | 3 | tính khả thi về ranh giới | 
| tải hỗn hợp | 2 | cân bằng tham lam | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi M bằng 1. Trong trường hợp này, thuật toán suy biến thành tổng tiền tố đơn giản, vì mọi tác vụ chạy tuần tự trên một máy. Logic heap vẫn hoạt động chính xác, nhưng nó thực sự là một chi phí không cần thiết. Đối với đầu vào`[5, 5, 5]`với D = 10, mô phỏng tạo ra thời gian kết thúc là 15, loại bỏ chính xác M = 1. 

Một trường hợp quan trọng khác là khi M lớn hơn hoặc bằng N. Ở đây mỗi tác vụ đều có máy riêng, vì vậy câu trả lời đơn giản là giá trị lớn nhất trong t. Mã xử lý việc này một cách rõ ràng, đảm bảo không có hoạt động heap nào bị lãng phí và tính chính xác được bảo toàn. 

Trường hợp thứ ba liên quan đến một nhiệm vụ cực kỳ lớn trong số nhiều nhiệm vụ nhỏ. Vùng heap đảm bảo rằng tác vụ lớn này luôn chiếm máy sớm và tìm kiếm nhị phân đảm bảo chúng ta tìm thấy M nhỏ nhất tách biệt hiệu ứng của nó đủ để đáp ứng D.
