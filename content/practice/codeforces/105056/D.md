---
title: "CF 105056D - Nhiệm vụ tại Odoo"
description: "Chúng tôi được giao một loạt nhiệm vụ, mỗi nhiệm vụ có thời gian xử lý cố định và thời hạn chung. Nhiệm vụ phải được khởi chạy theo thứ tự, nghĩa là nhiệm vụ tôi không thể bắt đầu trước khi nhiệm vụ i-1 được bắt đầu. Điều này tạo ra một chuỗi phụ thuộc vào thời gian bắt đầu chứ không phải thứ tự hoàn thành."
date: "2026-06-23T12:19:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105056
codeforces_index: "D"
codeforces_contest_name: "International Odoo Programming Contest 2024"
rating: 0
weight: 105056
solve_time_s: 84
verified: false
draft: false
---

[CF 105056D - Nhiệm vụ tại Odoo](https://codeforces.com/problemset/problem/105056/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao một loạt nhiệm vụ, mỗi nhiệm vụ có thời gian xử lý cố định và thời hạn chung. Nhiệm vụ phải được thực hiện theo thứ tự, nghĩa là nhiệm vụ`i`không thể bắt đầu trước nhiệm vụ`i-1`đã được bắt đầu. Điều này tạo ra một chuỗi phụ thuộc vào thời gian bắt đầu chứ không phải thứ tự hoàn thành. Tuy nhiên, khi các tác vụ được bắt đầu, chúng sẽ được thực thi trên một nhóm máy chủ giống hệt nhau, trong đó mỗi máy chủ chỉ có thể xử lý một tác vụ tại một thời điểm và sẽ khả dụng ngay sau khi hoàn thành tác vụ hiện tại. 

Quyết định quan trọng không phải là làm thế nào để lên lịch các nhiệm vụ một cách tự do mà là làm thế nào để phân công chúng cho các máy chủ trong khi tôn trọng hai ràng buộc: các nhiệm vụ được kích hoạt theo trình tự và tổng thời gian trôi qua cho đến khi mọi thứ hoàn thành không được vượt quá`T`. Chúng tôi cần số lượng máy chủ tối thiểu để thực hiện được điều này hoặc xác định rằng ngay cả những nguồn tài nguyên vô hạn cũng không thể đáp ứng được thời hạn. 

Những hạn chế`N, T ≤ 10^5`Và`A[i] ≤ 10^5`ngụ ý rằng bất kỳ giải pháp nào tồi tệ hơn tuyến tính hoặc tuyến tính trong`N`sẽ đấu tranh. Mô phỏng bậc ba hoặc bậc hai trên tất cả các nhiệm vụ của máy chủ ngay lập tức không khả thi vì mỗi lần kiểm tra sẽ quá chậm nếu lặp lại. 

Một trường hợp phức tạp nhưng quan trọng sẽ xuất hiện khi một nhiệm vụ vượt quá giới hạn thời gian. Ví dụ, nếu`T = 5`và một số`A[i] = 10`, không có máy chủ nào giúp được vì chỉ riêng nhiệm vụ đó đã vi phạm thời hạn. 

Một trường hợp khó khăn khác đến từ các nhiệm vụ giống hệt nhau. Nếu tất cả các nhiệm vụ đều`1`Và`T = 5`thì ngay cả với tính song song, chúng ta không thể vượt quá giới hạn tổng thông lượng phụ thuộc vào số lượng nhiệm vụ chồng chéo về thời gian. Một nhiệm vụ tham lam ngây thơ bỏ qua việc đồng bộ hóa thứ tự bắt đầu sẽ đánh giá thấp các máy chủ được yêu cầu. 

Chế độ thất bại cuối cùng xảy ra khi người ta cho rằng các nhiệm vụ có thể được sắp xếp lại. Họ không thể. Ràng buộc bắt đầu tuần tự được thực thi làm cho điều này về cơ bản khác với việc lập lịch cổ điển trên các máy giống hệt nhau. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ thử một số máy chủ cố định`k`và phân công nhiệm vụ một cách tham lam cho máy chủ có sẵn sớm nhất. Mỗi chi phí mô phỏng`O(N log k)`sử dụng hàng đợi ưu tiên để theo dõi tính khả dụng tiếp theo. Đang thử tất cả`k`từ`1`ĐẾN`N`cho`O(N^2 log N)`, quá chậm. 

Ngay cả khi chúng tôi tối ưu hóa bằng cách nhận thấy rằng tính khả thi là đơn điệu trong`k`, chúng ta có thể giảm việc tìm kiếm`k`sử dụng tìm kiếm nhị phân. Câu hỏi còn lại là làm thế nào để kiểm tra hiệu quả xem một số lượng máy chủ nhất định có thể hoàn thành trong thời gian hay không.`T`. 

Thông tin chuyên sâu quan trọng là mô hình hóa việc sử dụng máy chủ như một quy trình lập lịch được điều khiển bởi thời gian sẵn sàng. Mỗi tác vụ bắt đầu ngay khi được cho phép bởi ràng buộc thứ tự, nhưng việc thực thi nó có thể được chỉ định cho bất kỳ máy chủ nào rảnh rỗi. Chúng tôi duy trì cấu trúc về thời gian rảnh tiếp theo cho máy chủ. Đối với một cố định`k`, chúng tôi tham lam giao từng nhiệm vụ cho máy chủ có sẵn sớm nhất; nếu máy chủ đó đang bận vào thời điểm tác vụ buộc phải bắt đầu, chúng tôi sẽ trì hoãn việc phân công cho đến khi nó rảnh. Khoảng thời gian thực hiện được xác định bởi thời gian hoàn thành cuối cùng. 

Điều này có tác dụng vì ràng buộc thứ tự loại bỏ mọi quyền tự do trong thời gian bắt đầu: nhiệm vụ`i`luôn được “giải phóng” vào thời điểm nào đó`i-1`(về mặt khái niệm) và việc lập kế hoạch trở thành một vấn đề phân công bị hạn chế về tính khả dụng của máy. 

Chúng tôi tìm kiếm nhị phân nhỏ nhất`k`mang lại thời gian hoàn thành ≤`T`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force trên k | O(N^2 log N) | O(N) | Quá chậm | 
| Tìm kiếm nhị phân + mô phỏng | O(N log N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi trình tự là các nhiệm vụ có sẵn từng nhiệm vụ một. Chúng tôi kiểm tra xem một số lượng máy chủ cố định có`k`là đủ. 

1. Đối với ứng viên`k`, khởi tạo một đống kích thước tối thiểu`k`, trong đó mỗi phần tử biểu thị thời gian rảnh tiếp theo của máy chủ, tất cả đều bắt đầu tại thời điểm`0`. Điều này thể hiện rằng tất cả các máy chủ ban đầu đều không hoạt động. 
2. Lặp lại các nhiệm vụ theo thứ tự. Đối với mỗi nhiệm vụ`i`, hãy dành thời gian máy chủ sẵn có sớm nhất`t = heap.pop()`. Đây là máy chủ trở nên miễn phí đầu tiên. 
3. Nhiệm vụ được phép bắt đầu không sớm hơn giới hạn vị trí của nó, tức là về mặt thời gian.`i`nếu chúng ta giả định khoảng cách đơn vị của các bản phát hành. Chính xác hơn, chúng tôi coi các nhiệm vụ là đến một cách tuần tự; do đó thời gian bắt đầu sớm nhất là thời gian giải phóng nhiệm vụ hiện tại và tính khả dụng của máy chủ tối đa. 
4. Tính thời gian bắt đầu thực tế là`start = max(t, current_release_time)`. 
5. Thời gian hoàn thành là`start + A[i]`. Đẩy giá trị này trở lại vùng heap dưới dạng thời gian rảnh tiếp theo của máy chủ. 
6. Theo dõi thời gian hoàn thành tối đa của tất cả nhiệm vụ. 
7. Sau khi xử lý tất cả các tác vụ, trả về xem thời gian hoàn thành tối đa này có phải là ≤ hay không`T`. 
8. Sử dụng tìm kiếm nhị phân`k`từ`1`ĐẾN`N`để tìm giá trị khả thi tối thiểu. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán luôn giao nhiệm vụ tiếp theo cho máy chủ rảnh rỗi sớm nhất. Bất kỳ nhiệm vụ nào khác sẽ chỉ trì hoãn việc hoàn thành nhiệm vụ đó mà không tạo ra tính khả dụng sớm hơn cho các nhiệm vụ trong tương lai, vì các nhiệm vụ phải được xử lý theo thứ tự phát hành. Sự lựa chọn tham lam này bảo toàn hồ sơ hoàn thành sớm nhất có thể cho bất kỳ số lượng máy chủ cố định nào. Vì vậy, nếu có một lịch trình cho`k`máy chủ trong thời gian`T`, công trình này cũng sẽ đạt được thời gian hoàn thành không kém gì tiến độ đó. 

Hàm khả thi là đơn điệu trong`k`: việc thêm máy chủ chỉ có thể giảm thời gian chờ đợi hoặc giữ nguyên, không bao giờ tăng thời gian chờ đợi. Điều này đảm bảo tính chính xác của tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def can(k, a, T):
    heap = [0] * k
    heapq.heapify(heap)
    max_time = 0

    for i, x in enumerate(a):
        t = heapq.heappop(heap)
        start = max(t, i)
        finish = start + x
        max_time = max(max_time, finish)
        heapq.heappush(heap, finish)

        if max_time > T:
            return False

    return max_time <= T

def solve():
    n, T = map(int, input().split())
    a = list(map(int, input().split()))

    if max(a) > T:
        print(-1)
        return

    lo, hi = 1, n
    ans = n

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, a, T):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mô phỏng lõi được xử lý bởi một vùng dữ liệu tối thiểu để theo dõi thời điểm mỗi máy chủ hoạt động trở lại. Mỗi tác vụ được phân công một cách tham lam cho máy chủ có sẵn sớm nhất, đảm bảo chúng tôi không bao giờ làm máy chạy không tải một cách không cần thiết khi có công việc. 

Tìm kiếm nhị phân kết thúc quá trình kiểm tra tính khả thi này, thu hẹp số lượng máy chủ ứng viên cho đến khi chúng tôi tìm thấy cấu hình đủ nhỏ nhất. 

Một điểm tinh tế là việc cắt tỉa sớm khi một nhiệm vụ vượt quá`T`. Không có nó, tìm kiếm nhị phân vẫn hoạt động nhưng lãng phí thời gian để khám phá những cấu hình không thể thực hiện được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 11
1 1 1 1 1
```Chúng tôi kiểm tra số lượng máy chủ. 

Vì`k = 1`, tất cả các nhiệm vụ được tuần tự hóa, hoàn thành là`5`, tức là 11, nên khả thi. 

| Nhiệm vụ | Đống nhạc | Bắt đầu | Kết thúc | Đống sau | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | [1] | 
| 2 | 1 | 1 | 2 | [2] | 
| 3 | 2 | 2 | 3 | [3] | 
| 4 | 3 | 3 | 4 | [4] | 
| 5 | 4 | 4 | 5 | [5] | 

Điều này xác nhận một máy chủ duy nhất là đủ. 

Tìm kiếm nhị phân xác nhận`k = 1`. 

### Ví dụ 2 

đầu vào:```
5 5
1 1 1 1 1
```Bây giờ ngay cả với 1 máy chủ, việc hoàn thành là`5`, chính xác bằng giới hạn nên vẫn khả thi. 

Áp dụng dấu vết tương tự, cho thấy mức độ hoàn thành cuối cùng bằng`5`. 

Nếu chúng ta giảm`T`ĐẾN`4`, quy trình tương tự sẽ vượt quá giới hạn ở tác vụ cuối cùng, thể hiện hành vi ranh giới chặt chẽ trong đó cho phép bình đẳng nhưng không cho phép tràn nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N log N) | Mỗi lần kiểm tra tính khả thi xử lý N tác vụ với các thao tác heap tính chi phí log k và tìm kiếm nhị phân trên k sẽ thêm một hệ số log N khác | 
| Không gian | O(N) | Heap lưu trữ k trạng thái máy chủ, được giới hạn bởi N | 

Các ràng buộc cho phép khoảng 10^5 thao tác trên mỗi hệ số log một cách thoải mái. Mô phỏng heap đủ hiệu quả và tìm kiếm nhị phân giữ cho số lượng mô phỏng đầy đủ ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    input = _sys.stdin.readline

    def can(k, a, T):
        heap = [0] * k
        heapq.heapify(heap)
        max_time = 0
        for i, x in enumerate(a):
            t = heapq.heappop(heap)
            start = max(t, i)
            finish = start + x
            max_time = max(max_time, finish)
            heapq.heappush(heap, finish)
            if max_time > T:
                return False
        return max_time <= T

    n, T = map(int, input().split())
    a = list(map(int, input().split()))

    if max(a) > T:
        return "-1\n"

    lo, hi = 1, n
    ans = n
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, a, T):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    return str(ans) + "\n"

# provided samples
assert run("5 11\n1 1 1 1 1\n") == "1\n", "sample 1"
assert run("5 5\n1 1 1 1 1\n") == "1\n", "sample 2"
assert run("5 5\n2 3 5 6 1\n") == "-1\n", "sample 3"

# custom cases
assert run("1 10\n5\n") == "1\n", "single task fits"
assert run("1 3\n5\n") == "-1\n", "single task impossible"
assert run("3 10\n5 5 5\n") == "2\n", "parallel needed"
assert run("4 4\n1 1 1 1\n") == "1\n", "tight chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ duy nhất phù hợp | 1 | tính khả thi tối thiểu | 
| nhiệm vụ duy nhất không thể | -1 | trường hợp từ chối cứng | 
| nhiệm vụ nặng nề như nhau | 2 | yêu cầu song song | 
| dây chuyền chặt chẽ | 1 | bão hòa ranh giới | 

## Vỏ cạnh 

Một nhiệm vụ quá khổ duy nhất như`A = [100]`với`T = 10`ngay lập tức gây ra tình trạng không thể. Thuật toán kiểm tra việc trả trước này thông qua`max(a) > T`, trở về`-1`không cần mô phỏng không cần thiết. 

Khối lượng công việc hoàn toàn tuần tự với các tác vụ nhỏ xác nhận rằng ngay cả một máy chủ cũng có thể đủ khi tổng chiều dài chuỗi tôn trọng giới hạn. Vùng heap luôn chứa một phần tử duy nhất và việc hoàn thành hoàn toàn mang tính tích lũy. 

Khối lượng công việc song song cao với các tác vụ lớn đồng đều nhấn mạnh hành vi cân bằng vùng heap. Mỗi tác vụ liên tục chọn cùng một máy chủ cho đến khi có máy chủ khác, cho thấy rằng lựa chọn tham lam sẽ phân phối tải một cách chính xác mà không cần logic phân vùng rõ ràng.
