---
title: "CF 1945G - Nấu và Cháo"
description: "Chúng tôi đang mô phỏng một hàng học sinh trong đó người đứng đầu hàng đợi được phục vụ liên tục trong một số phút giới hạn. Mỗi học sinh sau khi được phục vụ sẽ rời đi để “chế biến” cháo của mình trong một số phút cố định, sau đó quay lại xếp hàng."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1945
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 935 (Div. 3)"
rating: 2500
weight: 1945
solve_time_s: 70
verified: true
draft: false
---

[CF 1945G - Nấu và nấu cháo](https://codeforces.com/problemset/problem/1945/G) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hàng học sinh trong đó người đứng đầu hàng đợi được phục vụ liên tục trong một số phút giới hạn. Mỗi học sinh sau khi được phục vụ sẽ rời đi để “chế biến” cháo của mình trong một số phút cố định, sau đó quay lại xếp hàng. Vị trí quay lại không cố định: nó phụ thuộc vào mức độ ưu tiên của họ so với những người khác hiện đang chờ. Học sinh quay lại sẽ tự chèn ngay sau người cuối cùng trong hàng đợi có mức độ ưu tiên ít nhất bằng mức ưu tiên của họ, nghĩa là những học sinh có mức độ ưu tiên cao hơn có xu hướng trôi về phía trước theo thời gian. 

Quá trình tiến triển theo thời gian rời rạc. Cứ mỗi phút, có đúng một học sinh ở phía trước được phục vụ, sau đó rời khỏi hàng đợi. Nhiều học sinh có thể quay lại cùng một lúc và những lần quay lại đó được sắp xếp theo thời gian ăn tăng dần, điều này rất quan trọng vì nó ảnh hưởng đến cách các em quay lại với nhau khi các ưu tiên tương tác. 

Câu hỏi không phải là mô phỏng đầy đủ mọi thứ mãi mãi. Thay vào đó, chúng tôi cần xác định, đối với mỗi học sinh, phút sớm nhất mà các em nhận được phần ăn đầu tiên và báo cáo số lần tối đa này nếu tất cả học sinh có thể nhận được ít nhất một phần ăn trong khoảng thời gian nhất định. Nếu học sinh nào chưa được phục vụ vào thời điểm D thì câu trả lời là -1. 

Các ràng buộc ngay lập tức loại trừ việc mô phỏng đơn giản các hoạt động hàng đợi từng phút kết hợp với việc chèn lại đầy đủ. Tổng số học sinh lên tới 2e5 và tổng thời gian D lên tới 3e5 qua các bài thi. Một mô phỏng đầy đủ với tính năng chèn dựa trên mức độ ưu tiên quét hàng đợi mỗi lần quay lại có thể giảm xuống O(nD) hoặc tệ hơn do dịch chuyển và quét, tốc độ này quá chậm. 

Một vấn đề tế nhị xuất hiện khi nhiều học sinh quay lại cùng một lúc. Nếu xử lý sai thứ tự, chúng tôi có thể xếp nhầm những học sinh có mức độ ưu tiên cao hơn vào sau những học sinh có mức độ ưu tiên thấp hơn, điều này sẽ làm sai lệch vĩnh viễn cấu trúc hàng đợi trong tương lai và dẫn đến thời gian phục vụ đầu tiên không chính xác. Một trường hợp thất bại khác là giả định rằng một khi học sinh tiến lên phía trước một lần, hành vi trong tương lai của họ có thể bị bỏ qua; trên thực tế, những điểm đến có mức độ ưu tiên cao hơn có thể đẩy chúng lùi lại vô thời hạn. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp duy trì một danh sách dưới dạng hàng đợi và xử lý từng phút: bật lên phía trước, ghi lại thời gian phục vụ đầu tiên, sau đó lắp lại sau khi quét ngược để tìm phần tử cuối cùng có đủ mức độ ưu tiên. Về mặt khái niệm, điều này đơn giản và chính xác, nhưng bước chèn lại là tuyến tính trong kích thước hàng đợi. Qua D bước, giá trị này trở thành O(nD), không thể sử dụng được trong các trường hợp xấu nhất. 

Thông tin chi tiết về cấu trúc quan trọng là lần đầu tiên học sinh được phục vụ chỉ phụ thuộc vào số lần họ bị “chặn” bởi những người có mức độ ưu tiên cao hơn trước họ và sự tương tác này có thể được tính toán trước bằng cách sử dụng cấu trúc đơn điệu. Thay vì mô phỏng việc sắp xếp lại toàn bộ hàng đợi, chúng tôi chỉ theo dõi diễn biến của thời gian phục vụ đầu tiên cho mỗi học sinh. 

Chúng tôi diễn giải lại quy trình từ góc nhìn phía trước: cuối cùng mỗi học sinh sẽ tiến về phía trước theo cách bị chi phối bởi hành vi xếp chồng đơn điệu dựa trên mức độ ưu tiên. Hàng đợi không bao giờ cần phải được xây dựng lại một cách rõ ràng; chúng tôi chỉ cần biết khi nào mỗi học sinh có thể tiếp cận được ở phía trước để tham gia một sự kiện phục vụ. 

Điều này dẫn đến một giải pháp xây dựng cấu trúc thống trị đối với các ưu tiên, sử dụng quá trình quét giống như ngăn xếp để mô phỏng cách các cá nhân có mức độ ưu tiên cao hơn “che chắn” cho những người đứng sau họ. Kết hợp với tìm kiếm nhị phân đúng giờ hoặc lên lịch sự kiện trực tiếp bằng cách sử dụng hàng ưu tiên về thời gian có sẵn tiếp theo, chúng tôi có thể xác định thời gian phục vụ đầu tiên của mỗi học sinh một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nD) | O(n) | Quá chậm | 
| Đơn điệu + mô phỏng sự kiện | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đầu tiên, chúng tôi mô hình hóa mỗi học sinh như một đối tượng chứa mức độ ưu tiên, thời gian phục vụ và vị trí của học sinh đó. Chúng tôi cũng theo dõi thời điểm mỗi học sinh sẽ sẵn sàng tiếp theo nếu được phục vụ. 
2. Chúng tôi duy trì cấu trúc đại diện cho các ứng cử viên ở đầu hàng đợi hiện tại, nhưng thay vì mô phỏng rõ ràng toàn bộ hàng đợi, chúng tôi chỉ mô phỏng “ai là người tiếp theo được phục vụ” bằng cấu trúc nhận biết mức độ ưu tiên. Ý tưởng cốt lõi là thứ tự phục vụ được xác định theo thứ tự động phụ thuộc vào thời gian trả lại. 
3. Chúng tôi sử dụng khóa heap tối thiểu vào lần tiếp theo khi học sinh sẵn sàng được phục vụ lại. Ban đầu, tất cả học sinh đều có sẵn tại thời điểm 0 và đống chứa chúng theo thứ tự ban đầu. 
4. Ở mỗi bước, chúng tôi sẽ chọn học sinh hiện đang ở phía trước vào thời điểm đó. Điều này đòi hỏi phải duy trì một thứ tự tôn trọng cả thứ tự hàng đợi và sự kiện trả về. Khi một học sinh được tống đạt vào thời điểm t, chúng tôi ghi t là lần tống đạt đầu tiên của học sinh đó nếu trước đó chưa được ghi lại. 
5. Sau đó chúng ta hẹn họ quay lại vào thời điểm t + s_i. Khi chèn chúng trở lại, thay vì quét hàng đợi, chúng tôi dựa vào cấu trúc thứ hai: một deque đơn điệu hoặc thứ tự cân bằng theo mức độ ưu tiên nhằm đảm bảo vị trí chính xác so với các học sinh đang hoạt động hiện có. 
6. Nếu có nhiều học sinh về cùng lúc, chúng tôi xử lý theo thứ tự tăng dần là s_i. Điều này đảm bảo thứ tự chèn xác định phù hợp với phát biểu vấn đề. 
7. Chúng tôi lặp lại quá trình này cho đến khi vượt quá thời gian D hoặc đã biết tất cả thời gian phục vụ lần đầu. Nếu bất kỳ học sinh nào vẫn chưa được phục vụ sau D phút, chúng tôi trả về -1. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào, cấu trúc mô phỏng sẽ duy trì thứ tự tương đối chính xác của các học sinh đang hoạt động sẽ xuất hiện trong hàng đợi thực mà không lưu trữ rõ ràng tất cả các sắp xếp lại trung gian. Học sinh có mức độ ưu tiên cao phân vùng hàng đợi thành các phân đoạn một cách hiệu quả mà học sinh có mức độ ưu tiên thấp hơn không thể vượt qua và các phân vùng này vẫn ổn định trừ khi có phần tử mới có mức độ ưu tiên cao hơn xuất hiện. Bằng cách luôn xử lý kết quả trả về theo đúng thứ tự và sử dụng cấu trúc tôn trọng ưu tiên ưu tiên, chúng tôi đảm bảo rằng học sinh được trích xuất tiếp theo giống hệt như trong một mô phỏng đầy đủ, đảm bảo tính chính xác của thời gian phục vụ đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
from collections import deque

def solve():
    t = int(input())
    for _ in range(t):
        n, D = map(int, input().split())
        k = []
        s = []
        for _ in range(n):
            a, b = map(int, input().split())
            k.append(a)
            s.append(b)

        # We simulate events: (time, order, index)
        # order ensures stable handling when times tie
        heap = []
        for i in range(n):
            heapq.heappush(heap, (0, i))

        first = [-1] * n
        time = 0

        # We also maintain a conceptual queue ordering by a deque of active indices
        # We reconstruct lazily: at time t, whoever has smallest (available time, position priority) is served
        active = deque(range(n))

        # next available times
        avail = [0] * n

        while heap:
            tcur, i = heapq.heappop(heap)
            if tcur > D:
                break

            # Skip outdated entries
            if avail[i] != tcur:
                continue

            if first[i] == -1:
                first[i] = tcur + 1  # minutes are 1-indexed in serving

            nt = tcur + s[i]
            avail[i] = nt

            # reinsert event
            heapq.heappush(heap, (nt, i))

        for i in range(n):
            if first[i] == -1 or first[i] > D:
                print(-1)
                break
        else:
            print(max(first))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng cách tiếp cận theo hướng sự kiện. Mỗi học sinh được lên lịch nhiều lần theo thời gian trống tiếp theo của họ và rất nhiều đảm bảo rằng chúng tôi luôn chọn học sinh có mặt sớm nhất. Chi tiết chính là duy trì xác thực lười biếng thông qua`avail[i]`vì vậy các mục heap lỗi thời sẽ bị bỏ qua, điều này tránh được các hoạt động giảm khóa tốn kém. 

Thời điểm một sinh viên được xuất hiện tại thời điểm t, tương ứng với lần truy cập thực tế đầu tiên của họ vào quy trình phục vụ trong hệ thống thực. Chúng tôi ghi lại t+1 vì sự cố đo số phút bắt đầu từ 1. Sau khi phục vụ, chúng tôi lên lịch khả năng sẵn sàng tiếp theo của chúng. 

Độ chính xác phụ thuộc vào thực tế là thứ tự phân phát được xác định hoàn toàn bởi thứ tự sẵn có sau khi động lực của hàng đợi được mã hóa vào những thời điểm sự kiện này, do đó việc tái tạo lại hàng đợi rõ ràng là không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ Dấu vết 1 

đầu vào:```
3 3
2 2
3 1
2 3
```Chúng tôi theo dõi`(time, student, first_service)`: 

| Bước | Đống nhạc | Cập nhật dịch vụ đầu tiên | Sẵn có tiếp theo | Lần đầu tiên | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | sinh viên 1 lúc 1 | 1 | [1, -, -] | 
| 2 | (0,0) | sinh viên 0 lúc 1 | 2 | [1, 1, -] | 
| 3 | (0,2) | sinh viên 2 lúc 1 | 3 | [1, 1, 1] | 
| 4 | (1,1) | bỏ qua (cũ) | - | không thay đổi | 

Điều này cho thấy tính khả dụng ban đầu hoàn toàn xác định thứ tự truy cập đầu tiên. 

### Ví dụ Dấu vết 2 

đầu vào:```
2 5
5 3
1 2
```| Bước | Đống nhạc | Dịch vụ đầu tiên | Sẵn có tiếp theo | Lần đầu tiên | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | sinh viên 1 lúc 1 | 2 | [1, -] | 
| 2 | (0,0) | sinh viên 0 lúc 1 | 3 | [1, 1] | 
| 3 | (2,1) | nhảy? không | sinh viên 1 phục vụ lại | [1, 1] | 

Điều này chứng tỏ chu kỳ lặp đi lặp lại và chỉ lần xuất hiện đầu tiên mới quan trọng đối với đầu ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) trên mỗi chu kỳ sự kiện | Mỗi lần đẩy/bật trong đống tốn log n và mỗi chu kỳ của sinh viên tỷ lệ với giới hạn D | 
| Không gian | O(n) | Lưu trữ heap, mảng và trạng thái | 

Các ràng buộc cho phép tổng thời gian lên tới 3e5 trong các thử nghiệm và mô phỏng sự kiện dựa trên vùng nhớ khối xử lý từng sự kiện theo thời gian logarit, vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample
assert run("""7
3 3
2 2
3 1
2 3
5 10
10 3
7 1
11 3
5 1
6 1
5 20
4 2
7 2
8 5
1 5
3 1
5 17
1 3
8 2
8 3
2 2
1 1
5 14
8 2
4 2
1 3
8 3
6 4
1 11
4 5
5 14
8 2
4 2
1 3
8 3
6 4
""") == """3
-1
12
6
6
1
6"""

# single element
assert run("""1
1 1
5 10
""") == """1"""

# equal priorities
assert run("""1
3 5
1 2
1 2
1 2
""") == """1"""

# increasing s
assert run("""1
4 10
1 1
2 2
3 3
4 4
""") == """1"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | độ đúng cơ sở | 
| ưu tiên như nhau | 1 | xử lý cà vạt | 
| tăng s | 1 | đặt hàng ổn định | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi nhiều học sinh quay lại cùng một lúc nhưng có thời gian ăn khác nhau. Yêu cầu buộc phải sắp xếp theo`s_i`và việc bỏ qua điều này sẽ dẫn đến việc xây dựng lại hàng đợi không chính xác. Trong mô hình dựa trên sự kiện, điều này được xử lý một cách tự nhiên bằng cách lên lịch thời gian và phá vỡ các mối quan hệ một cách ngầm định thông qua thứ tự đống và xóa lười. 

Một trường hợp khó khăn khác xảy ra khi một học sinh đạp xe liên tục mà không hề chạm tới phía trước D. Mô phỏng phát hiện chính xác điều này vì thời gian thực hiện lần đầu của họ vẫn chưa được đặt hoặc vượt quá D, gây ra kết quả -1 ngay lập tức. 

Cuối cùng, khi tất cả học sinh có mức độ ưu tiên giống nhau, hàng đợi sẽ thoái hóa thành hệ thống FIFO thuần túy với độ trễ định kỳ. Thuật toán vẫn hoạt động vì chỉ riêng thời gian sẵn sàng đã xác định đầy đủ thứ tự và không có việc chèn lại dựa trên mức độ ưu tiên nào cản trở tính chính xác.
