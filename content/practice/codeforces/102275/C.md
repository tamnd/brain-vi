---
title: "CF 102275C - Phân loại"
description: "Chúng ta có (S) ngăn xếp, mỗi ngăn chứa (H) giấy tờ từ trên xuống dưới. Đầu vào cung cấp cho các ngăn xếp theo từng hàng, do đó, chuỗi đầu vào thứ (i) mô tả các giấy tờ ở độ sâu (i) trên tất cả các ngăn xếp. Mỗi bài thuộc môn A hoặc môn B. Một bài có thể bị chấm điểm hoặc bị mất."
date: "2026-08-17T10:04:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102275
codeforces_index: "C"
codeforces_contest_name: "2019 Facebook Hacker Cup, Round 2"
rating: 0
weight: 102275
solve_time_s: 995
verified: true
draft: false
---

[CF 102275C - Chấm điểm](https://codeforces.com/problemset/problem/102275/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 16 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có (S) ngăn xếp, mỗi ngăn chứa (H) giấy tờ từ trên xuống dưới. Đầu vào cung cấp cho các ngăn xếp theo từng hàng, do đó, chuỗi đầu vào thứ (i) mô tả các giấy tờ ở độ sâu (i) trên tất cả các ngăn xếp. Mỗi bài thuộc về chủ đề A hoặc chủ đề B. 

Một bài viết có thể được chấm điểm hoặc bị mất. Giấy tờ bị mất không ảnh hưởng đến việc chuyển ngữ cảnh nhưng được tính vào ngân sách mất mát cho phép. Các giấy tờ đã được phân loại được xử lý theo một số thứ tự tôn trọng thứ tự dọc bên trong mỗi chồng giấy. Sự chuyển đổi ngữ cảnh xảy ra đối với bài viết được chấm điểm đầu tiên và bất cứ khi nào chủ đề của bài viết được chấm điểm khác với bài viết được chấm điểm trước đó. Vì vậy, nếu bài viết được chấm điểm có trình tự chủ đề```
AAAABBBBAAA
```có ba công tắc ngữ cảnh vì trình tự có ba lần chạy. 

Đối với mỗi số cho phép (L_i), chúng ta cần số lần chạy nhỏ nhất có thể trong chuỗi môn học được chấm điểm sau khi mất nhiều nhất (L_i) bài. 

Kích thước nhiều nhất là (300), do đó có nhiều nhất (HS=90.000) giấy tờ. Một thuật toán bậc hai trong tổng số bài báo đã quá lớn và bất kỳ thuật toán nào theo cấp số nhân là hoàn toàn không thể. Mục tiêu hữu ích là khoảng (O(HS^2)), vì một chiều tối đa là (300). Đầu vào chứa tối đa (K=HS) truy vấn, do đó, việc xử lý mọi truy vấn một cách độc lập bằng DP tốn kém cũng sẽ không cần thiết. Chúng ta nên tính toán câu trả lời cho mọi số lần chuyển ngữ cảnh có thể một lần, sau đó trả lời từng truy vấn từ quá trình tính toán trước đó. 

Có ba trường hợp đặc biệt có xu hướng đưa ra các giải pháp không chính xác. 

Hãy xem xét một chồng giấy có năm tờ giấy.```
1 5 2
BABAB
1 2
```Đầu ra đúng là`Case #1: 2 1`. Với một lần thua được phép, chuỗi chủ đề còn lại tốt nhất vẫn cần hai lần chạy. Với hai lần thua, chúng ta có thể giữ cả ba tờ B và mất cả hai tờ A, chỉ được chạy một lần. Một giải pháp bất cẩn tính toán mọi thay đổi của chủ đề trong ngăn xếp ban đầu mà không tính đến tổn thất sẽ bỏ lỡ câu trả lời thứ hai. 

Hãy xem xét hai ngăn xếp có chiều cao hai.```
2 2 3
AB
BA
0 1 2
```Hai ngăn xếp là`AB`Và`BA`. Không bị thua lỗ, cả hai chuỗi hai lượt chạy hoàn chỉnh đều có đối tượng bắt đầu trái ngược nhau, vì vậy hai lượt chạy toàn cầu là không đủ. Bốn? Không, ví dụ như ba lần chạy toàn cầu là đủ`A,B,A`. Như vậy các câu trả lời là`3 2 1`. Giải pháp chỉ tính toán số lần chạy tối đa cần thiết cho một ngăn xếp riêng lẻ sẽ trả về không chính xác hai lần với mức tổn thất bằng 0 và sẽ bỏ lỡ xung đột định hướng giữa các ngăn xếp. 

Cuối cùng, hãy xem xét một lưới hoàn toàn thống nhất.```
2 2 3
AA
AA
0 1 3
```Mỗi bài báo đều là A, do đó, một chuyển đổi ngữ cảnh là đủ ngay cả khi không bị lỗ và nó vẫn đủ cho mọi mức lỗ. Đầu ra đúng là`Case #1: 1 1 1`. Một giải pháp nhất quyết sử dụng đúng số lượng tổn thất cho phép, thay vì tối đa con số đó, có thể loại bỏ giấy tờ không chính xác và tạo ra kết quả tồi tệ hơn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể quyết định một cách độc lập cho mỗi bài viết xem nó được chấm điểm hay bị mất. Với (N=HS) bài thi, điều đó đã tạo ra (2^N) tập hợp con các bài được chấm điểm. Đối với mỗi tập hợp con, chúng ta sẽ phải xác định xem liệu các bài viết của nó có thể được sắp xếp thành các ngăn hay không và tìm số lần chạy chủ đề tối thiểu. Ngay cả khi việc kiểm tra đó chỉ mất (O(N)), tổng công việc sẽ là (O(N2^N)). Ở mức tối đa (N=90.000), số lượng tập hợp con là (2^{90000}), do đó phương pháp này không khả thi từ xa. 

Quan sát hữu ích là các ngăn xếp chỉ hạn chế thứ tự tương đối của các giấy tờ thuộc cùng một ngăn xếp. Sau khi chúng tôi quyết định bài nào của một ngăn xếp được xếp loại, chúng sẽ tạo thành một chuỗi con của chuỗi ngăn xếp đó. Trình tự được phân loại từ một ngăn xếp có thể được xen kẽ với các trình tự được phân loại từ tất cả các ngăn xếp khác. 

Giả sử chuỗi chủ đề chung cuối cùng có (C) chạy. Mỗi ngăn xếp chỉ cần tạo ra một chuỗi con có thể được nhúng vào các lần chạy xen kẽ (C) đó. Chúng ta không cần phải quyết định thứ tự toàn cầu chính xác của từng giấy tờ. 

Có một sự tinh tế. Một chuỗi con sử dụng ít hơn (C) lần chạy có thể bắt đầu ở một trong hai chủ đề, bởi vì nó có thể được đặt vào một trong hai tính chẵn lẻ của các lần chạy chung. Một chuỗi con sử dụng chính xác (C) các lượt chạy không có lượt chạy dự phòng ở đầu, vì vậy chủ đề đầu tiên của nó phải bằng chủ đề đầu tiên của chuỗi toàn cục. 

Điều này làm giảm vấn đề thành một chương trình động độc lập cho mọi ngăn xếp. Đối với mỗi số lần chạy có thể (r), chúng tôi tính toán số lượng giấy tờ bị xóa tối thiểu cần thiết để có được một chuỗi con có chính xác (r) lần chạy và với chủ đề đầu tiên được chỉ định. 

Đối với một chồng có chiều cao (H), DP này mất (O(H^2)) thời gian. Có (S) ngăn xếp, tạo ra tổng công việc là (O(SH^2)). Vì (H,S\le300), điều này là thực tế. 

Sau khi tính toán từng ngăn xếp, chúng ta kết hợp các ngăn xếp bằng phép cộng. Đối với mọi số lượng có thể (C) của các lần chạy toàn cầu, chúng tôi tính toán tổn thất tối thiểu cần thiết khi lần chạy toàn cầu đầu tiên là A và khi nó là B. Giá trị tốt hơn trong hai giá trị đó là tổn thất tối thiểu cần thiết cho các chuyển đổi ngữ cảnh (C). 

Yêu cầu mất mát dẫn đến đều đơn điệu: cho phép nhiều chuyển đổi ngữ cảnh hơn không bao giờ có thể yêu cầu nhiều giấy tờ bị mất hơn. Điều này cho phép chúng tôi trả lời mọi (L_i) bằng tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(HS\cdot2^{HS})) hoặc tệ hơn | (O(HS)) | Quá chậm | 
| Tối ưu | (O(SH^2 + K\log H)) | (O(H+S)) | Đã chấp nhận | 

Công thức lập trình động (O(SH^2)) cũng nhất quán với cuộc thảo luận tiêu chuẩn về cuộc thi, trong đó giải pháp đơn giản được mô tả là DP theo chiều dài ngăn xếp và số khối. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào (H). Ký tự ở hàng (i), cột (j) thuộc về ngăn xếp (j), vì vậy hãy xây dựng lại mọi ngăn xếp bằng cách lấy một ký tự từ mỗi hàng. 
2. Đối với một ngăn xếp, hãy tính hai mảng DP, một cho các dãy con có chủ đề được xếp loại đầu tiên là A và một mảng cho các dãy con có chủ đề được xếp loại đầu tiên là B. Giả sử`dp[r]`là số lượng giấy tờ tối đa có thể được lưu giữ trong khi sản xuất chính xác (r) chủ đề. 
3. Xử lý ngăn xếp từ trên xuống dưới. Khi ký tự hiện tại khớp với chủ đề phải kết thúc chuỗi chạy (r), nó có thể mở rộng chuỗi con chạy (r) hiện có hoặc bắt đầu lần chạy thứ (r) từ chuỗi con chạy ((r-1)). Việc xử lý số lần chạy ngược cho phép cả hai lần chuyển đổi sử dụng các giá trị của vị trí trước đó. 
4. Sau khi xử lý ngăn xếp, hãy chuyển đổi độ dài tối đa được giữ thành số lần xóa. Với mọi (r),`delA[r]`là số tổn thất tối thiểu cần thiết cho chính xác (r) lần chạy bắt đầu bằng A và`delB[r]`được định nghĩa tương tự. 
5. Xây dựng`best[r]`, tổn thất tối thiểu cần thiết để có được một chuỗi con có nhiều nhất (r) lần chạy mà không cần quan tâm đến chủ đề bắt đầu của nó. Dãy con trống được phép tính toán này và chi phí (H) tổn thất. 
6. Giả sử chuỗi toàn cục có (C) lần chạy và bắt đầu bằng A. Một ngăn xếp sử dụng ít lần chạy hơn (C) có thể bắt đầu ở một trong hai chủ đề, do đó sẽ tốn kém`best[C-1]`. Một ngăn xếp sử dụng chính xác (C) các lần chạy phải bắt đầu bằng A, vì vậy nó có giá`delA[C]`. Chi phí thực tế của nó nhỏ hơn trong hai giá trị này. 
7. Thực hiện tương tự cho chuỗi chung bắt đầu bằng B. Thêm chi phí tương ứng một cách độc lập cho mỗi ngăn xếp. Tổng nhỏ hơn là số tổn thất tối thiểu cần thiết để đạt được chính xác (C) chuyển đổi bối cảnh toàn cầu. 
8. Chuỗi toàn cục không bao giờ cần nhiều hơn (H+1) lần chạy. Mỗi ngăn xếp riêng lẻ có độ dài (H), do đó, nó sử dụng tối đa (H) lần chạy. Với các lần chạy toàn cục (H+1), mọi ngăn xếp đều có thể vừa khít bất kể chủ thể bắt đầu của nó là gì vì nó có ít nhất một lần chạy toàn cầu dự phòng để căn chỉnh. 
9. Mảng kết quả`need[C]`là đơn điệu không tăng. Đối với mọi truy vấn (L_i), tìm kiếm nhị phân cho thỏa mãn (C) nhỏ nhất`need[C] <= L_i`. 

Tại sao nó hoạt động: đối với mỗi ngăn xếp, DP xem xét mọi chuỗi con có thể xảy ra theo số lần chạy và chủ đề bắt đầu, do đó, DP sẽ tìm thấy tổn thất tối thiểu cho mọi cấu hình cục bộ có liên quan. Một chuỗi con cục bộ có ít lần chạy hơn (C) luôn có thể được chuyển thành một tập hợp con tương thích của các lần chạy toàn cục xen kẽ (C). Một chuỗi con có chính xác (C) lần chạy không có quyền tự do như vậy, vì vậy chủ thể bắt đầu của nó phải đồng ý với lần chạy đầu tiên toàn cục. Khi các điều kiện này được giữ cho mọi ngăn xếp, độ dài chạy của chuỗi chung có thể được chọn đủ lớn cho lần chạy được chỉ định của mỗi ngăn xếp. Sau đó, các ngăn xếp có thể được xen kẽ một cách độc lập và các giấy tờ bị bỏ qua sẽ bị mất. Do đó, tổng chi phí DP vừa có thể đạt được vừa là giới hạn thấp hơn cho mọi chiến lược hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -10**9

def stack_costs(s):
    h = len(s)

    dp_a = [NEG] * (h + 1)
    dp_b = [NEG] * (h + 1)

    for ch in s:
        for r in range(h, 0, -1):
            # A-starting subsequence.
            last_a = 'A' if r & 1 else 'B'
            if ch == last_a:
                cand = NEG

                if dp_a[r] != NEG:
                    cand = dp_a[r] + 1

                if r == 1:
                    cand = max(cand, 1)
                elif dp_a[r - 1] != NEG:
                    cand = max(cand, dp_a[r - 1] + 1)

                dp_a[r] = max(dp_a[r], cand)

            # B-starting subsequence.
            last_b = 'B' if r & 1 else 'A'
            if ch == last_b:
                cand = NEG

                if dp_b[r] != NEG:
                    cand = dp_b[r] + 1

                if r == 1:
                    cand = max(cand, 1)
                elif dp_b[r - 1] != NEG:
                    cand = max(cand, dp_b[r - 1] + 1)

                dp_b[r] = max(dp_b[r], cand)

    inf = h + 1
    del_a = [inf] * (h + 1)
    del_b = [inf] * (h + 1)

    for r in range(1, h + 1):
        if dp_a[r] != NEG:
            del_a[r] = h - dp_a[r]
        if dp_b[r] != NEG:
            del_b[r] = h - dp_b[r]

    # best[r] = minimum losses for at most r runs,
    # with arbitrary starting subject.
    best = [h] * (h + 1)

    for r in range(1, h + 1):
        best[r] = min(best[r - 1], del_a[r], del_b[r])

    return del_a, del_b, best

def solve_case(h, s, rows, queries):
    # There are S stacks, each of height H.
    stacks = []
    for col in range(s):
        stacks.append(''.join(rows[row][col] for row in range(h)))

    max_runs = h + 1

    total_a = [0] * (max_runs + 1)
    total_b = [0] * (max_runs + 1)

    for stack in stacks:
        del_a, del_b, best = stack_costs(stack)

        for c in range(1, h + 1):
            # Global sequence starts with A.
            total_a[c] += min(best[c - 1], del_a[c])

            # Global sequence starts with B.
            total_b[c] += min(best[c - 1], del_b[c])

        # With H+1 runs, every stack has at most H runs,
        # so its starting subject can always be aligned.
        total_a[h + 1] += best[h]
        total_b[h + 1] += best[h]

    need = [0] * (max_runs + 1)
    for c in range(1, max_runs + 1):
        need[c] = min(total_a[c], total_b[c])

    answers = []

    for loss in queries:
        lo = 1
        hi = max_runs

        while lo < hi:
            mid = (lo + hi) // 2
            if need[mid] <= loss:
                hi = mid
            else:
                lo = mid + 1

        answers.append(str(lo))

    return answers

def solve():
    t = int(input())

    out = []

    for case_id in range(1, t + 1):
        h, s, k = map(int, input().split())

        rows = [input().strip() for _ in range(h)]
        queries = list(map(int, input().split()))

        answers = solve_case(h, s, rows, queries)
        out.append(f"Case #{case_id}: {' '.join(answers)}")

    sys.stdout.write('\n'.join(out))

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được lưu trữ dưới dạng hàng vì đó là cách biểu diễn các ngăn xếp. biểu thức`rows[row][col]`là tờ giấy ở độ sâu cụ thể trong một ngăn xếp cụ thể, vì vậy việc nối một cột sẽ tạo lại một ngăn xếp hoàn chỉnh từ trên xuống dưới. 

các`stack_costs`chức năng là DP cốt lõi. Đối với chủ đề bắt đầu cố định, chủ đề cuối cùng của chuỗi con chạy chính xác (r) đã được xác định bằng tính chẵn lẻ. Nếu dãy con bắt đầu bằng A thì lần chạy cuối cùng của nó là A cho số lẻ (r) và B cho số chẵn (r). Điều đó loại bỏ một chiều DP. 

Khi một ký tự khớp với chủ đề cuối cùng được yêu cầu, nó có thể kéo dài thời gian chạy hiện tại. Nó cũng có thể bắt đầu một lần chạy mới nếu trạng thái trước đó có (r-1) lần chạy, vì lần chạy trước đó nhất thiết phải kết thúc ở chủ đề ngược lại. Vòng lặp kết thúc`r`chạy ngược lại nên`dp[r - 1]`giá trị vẫn thuộc về vị trí đầu vào trước đó. 

Các mảng lưu trữ số bài được giữ lại tối đa thay vì số bài bị xóa tối thiểu vì việc tối đa hóa số lượng bài được xếp loại sẽ mang lại kết quả tương tự và làm cho quá trình chuyển tiếp trở nên bổ sung. Cuối cùng,`h - dp[r]`chuyển đổi kết quả thành số lượng tổn thất cần thiết. 

các`best`mảng bao gồm dãy con trống với giá`h`. Điều này là cần thiết vì một ngăn xếp có thể bị bỏ qua hoàn toàn khi mức tổn thất cho phép. Nó cũng xử lý trường hợp các giấy tờ của ngăn xếp không liên quan đến chủ đề toàn cầu đã chọn. 

Bước kết hợp là nơi cấu trúc toàn cầu đi vào. Vì`total_a[c]`, một ngăn xếp có thể sử dụng ít hơn`c`chạy và bắt đầu ở bất cứ nơi nào nó cần, hoặc nó có thể sử dụng tất cả`c`chạy và phải bắt đầu bằng A. Đó chính xác là hai trường hợp được biểu thị bởi`min(best[c - 1], del_a[c])`. 

Không có vấn đề tràn số nguyên trong Python. Số lượng tổn thất tối đa là (HS) và giá trị DP nhiều nhất là (H). Vòng lặp chạy lùi cũng rất cần thiết, vì việc thay đổi nó theo thứ tự tăng dần sẽ cho phép cùng một bài viết tham gia vào nhiều lần chạy mới được tạo trong một lần lặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có một chồng có chiều cao năm:```
BABAB
```Các truy vấn là một và hai tổn thất được phép. 

Đối với một ngăn xếp, số lần xóa chính xác là: 

| Chạy | Bắt đầu A, xóa | Bắt đầu B, xóa | Tốt nhất với nhiều nhất số lần chạy này | 
| --- | --- | --- | --- | 
| 0 | 5 | 5 | 5 | 
| 1 | 3 | 2 | 2 | 
| 2 | 2 | 2 | 2 | 
| 3 | 2 | 2 | 2 | 
| 4 | 1 | 1 | 1 | 
| 5 | 0 | 0 | 0 | 

Với một kỳ thi toàn cầu, lựa chọn tốt nhất là chấm cả ba bài B và thua cả hai bài A, vì vậy`need[1] = 2`. 

Với hai lần chạy toàn cầu, toàn bộ ngăn xếp có thể giảm xuống thành chuỗi hai lần chạy sau hai lần thua, nhưng không được sau một lần thua. Như vậy`need[2] = 2`. 

Bốn lần chạy, một lần thua là đủ, vậy nên`need[4] = 1`. 

Do đó, hai truy vấn được trả lời như sau. 

| Khoản lỗ được phép | Số lần chạy khả thi đầu tiên | Trả lời | 
| --- | --- | --- | 
| 1 | 4? | 4 | 

Bảng này sẽ gây hiểu nhầm nếu diễn giải trực tiếp vì mẫu đầu tiên thực tế có năm ngăn xếp có chiều cao bằng một, được biểu thị bằng hàng`BABAB`. Trong cách giải thích ma trận thực tế, năm ngăn xếp chứa B, A, B, A, B. Sau đó`need[1] = 2`Và`need[2] = 0`, đưa ra câu trả lời cần thiết`2 1`. 

Ví dụ này rất hữu ích vì nó giải thích tại sao dữ liệu đầu vào phải được diễn giải theo từng cột. Hàng đầu vào duy nhất đại diện cho năm ngăn xếp khác nhau, không phải một ngăn xếp chứa năm tờ giấy. 

### Mẫu 2 

Mẫu thứ hai là```
2 3 3
ABA
AAB
1 0 5
```Có ba ngăn xếp, mỗi ngăn xếp có chiều cao hai. Đọc các cột cho:```
AA
BA
AB
```Ngăn xếp đầu tiên đã bao gồm một lần chạy A. Người thứ hai là B theo sau là A, và người thứ ba là A theo sau là B. 

Để không bị tổn thất, hai ngăn xếp hai lần có đối tượng bắt đầu trái ngược nhau. Hai lần chạy toàn cầu không thể chứa cả hai chuỗi hoàn chỉnh, vì vậy cần có ba lần chạy toàn cầu. 

| Chạy toàn cầu | Bắt đầu Một sự mất mát hoàn toàn | Bắt đầu thua tổng B | Mất tối thiểu | 
| --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 
| 2 | 1 | 1 | 1 | 
| 3 | 0 | 0 | 0 | 

Do đó, tổn thất bằng 0 cần có ba công tắc. Với một tổn thất cho phép, hai công tắc là đủ. Với năm lần thua được phép, một lần chạy là đủ. 

Các câu trả lời kết quả là`2 3 1`cho các truy vấn`1 0 5`. Ví dụ này thể hiện điều kiện định hướng: số lần chạy bên trong một ngăn xếp riêng lẻ không đủ để xác định câu trả lời chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(SH^2 + K\log H)) | Mỗi ngăn xếp (S) có độ dài (H) và DP chạy của nó sẽ kiểm tra trạng thái (O(H^2)). Mỗi truy vấn sử dụng tìm kiếm nhị phân trong tối đa (H+1) lần chạy toàn cầu. | 
| Không gian | (O(H+S)) | DP cho một ngăn xếp sử dụng trạng thái (O(H)), trong khi mảng toàn cục chứa các giá trị (O(H)) và các hàng đầu vào sử dụng ký tự (O(HS)). | 

Bản thân đầu vào đã chứa các ký tự (HS), do đó việc lưu trữ ma trận là điều không thể tránh khỏi khi triển khai đơn giản. Với (H,S\le300), DP thực hiện tối đa vài chục triệu cập nhật trạng thái đơn giản trong trường hợp lớn nhất, trong khi giai đoạn truy vấn là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

# The solution above is assumed to be defined in the same file:
# solve()

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
sample = """\
5
1 5 2
BABAB
1 2
2 3 3
ABA
AAB
1 0 5
3 2 4
AB
BA
AB
0 1 2 3
5 5 6
BBABA
ABAAB
AAABA
BABBA
BBBAB
5 0 8 12 10 1
10 10 15
AABAAABBAB
BAABAAAABB
AABABBBABB
BAAABAAAAB
BBBBAAABAA
ABAABBBABA
BABAABABBA
AAABAAABAA
BAAAABBBBA
ABABBAAABA
14 2 99 33 3 8 43 4 12 1 21 24 17 32 10
"""

sample_expected = """\
Case #1: 2 1
Case #2: 2 3 1
Case #3: 4 3 2 1
Case #4: 3 5 2 1 2 4
Case #5: 5 8 1 2 8 7 1 7 5 9 4 3 4 3 6
"""

assert run(sample) == sample_expected, "provided samples"

# Minimum-size input.
minimum = """\
1
1 1 1
A
0
"""

assert run(minimum) == "Case #1: 1\n", "minimum-size case"

# Every paper has the same subject.
uniform = """\
1
2 2 3
AA
AA
0 1 3
"""

assert run(uniform) == "Case #1: 1 1 1\n", "all-equal case"

# Opposite orientations force an extra global run.
opposite = """\
1
2 2 3
AB
BA
0 1 2
"""

assert run(opposite) == "Case #1: 3 2 1\n", "orientation boundary case"

# Maximum-size case. Every paper is A, so one run is always enough.
H = 300
S = 300
maximum = ["1", f"{H} {S} 2"]
maximum.extend(["A" * S for _ in range(H)])
maximum.append(f"0 {H * S - 1}")
maximum_input = "\n".join(maximum) + "\n"

assert run(maximum_input) == "Case #1: 1 1\n", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 1 / A / 0`|`Case #1: 1`| Kích thước tối thiểu và chuyển đổi ngữ cảnh đầu tiên bắt buộc | 
|`2 2 / AA / AA`|`Case #1: 1 1 1`| Môn học thống nhất và thực tế là thua lỗ là tùy chọn | 
|`2 2 / AB / BA`|`Case #1: 3 2 1`| Hướng xuất phát đối diện và ranh giới (H+1) | 
| Ma trận 300 x 300 toàn A |`Case #1: 1 1`| Kích thước tối đa, đầu vào lớn và truy vấn bị mất tối đa | 

## Vỏ cạnh 

Trường hợp xen kẽ một ngăn xếp```
1
1 5 2
BABAB
1 2
```chứa năm ngăn xếp có chiều cao một, không phải một ngăn xếp có chiều cao năm. Với một mất mát được phép, năm đối tượng có thể được sắp xếp theo thứ tự B, B, B, A, A bằng cách mất một B, do đó, hai chuyển ngữ cảnh là đủ. Với hai lần thua, tất cả các bài B đều có thể được xếp loại và tất cả các bài A đều bị mất, chỉ có một lần chuyển đổi. Việc xây dựng cột trong quá trình triển khai sẽ tự động xử lý việc này vì một hàng đầu vào sẽ trở thành năm ngăn xếp một ký tự. 

Trường hợp ngược hướng```
1
2 2 3
AB
BA
0 1 2
```tạo ra ngăn xếp`AB`Và`BA`. Với mức tổn thất bằng 0, một ngăn xếp yêu cầu A rồi B ​​trong khi ngăn xếp kia yêu cầu B rồi A. Chuỗi toàn cục hai lần chạy không thể chứa cả hai chuỗi hoàn chỉnh, vì vậy cần có ba lần chạy. Với một lần mất, một trong các ngăn xếp có thể được giảm xuống còn một lần chạy, cho phép ngăn xếp hai lần chạy còn lại phù hợp với chuỗi toàn cầu hai lần chạy. Với hai lần thua, chỉ cần chấm điểm một môn, nên chỉ cần một lần thi là đủ. DP nắm bắt được điều này thông qua`del_a[c]`Và`del_b[c]`, thay vì coi số lần chạy cục bộ là đủ. 

Trường hợp đồng phục```
1
2 2 3
AA
AA
0 1 3
```có hai ngăn xếp, cả hai đều chỉ chứa A. DP không tìm thấy thao tác xóa nào trong một lần chạy bắt đầu bằng A. Do đó`need[1]`bằng 0 và mọi khoản lỗ lớn hơn đều có cùng một câu trả lời. Việc triển khai không làm mất bất kỳ giấy tờ nào, đó là lý do tại sao cả ba truy vấn đều trả về một. 

Ranh giới tổn thất tối đa cũng được xử lý bởi trạng thái chạy bổ sung (H+1). Với (H) giấy trong một chồng, không có chồng nào có thể chứa nhiều hơn (H) số lần chạy. Nếu các ngăn xếp khác nhau yêu cầu các hướng khác nhau và tất cả các bài viết phải được xếp loại, thì một lần chạy toàn cầu bổ sung là đủ để dịch chuyển một hướng theo một vị trí. Do đó (H+1) là giới hạn trên phổ quát an toàn và việc triển khai tính toán rõ ràng trạng thái đó thay vì vô tình lập chỉ mục`delA[H+1]`hoặc`delB[H+1]`. 

Cuối cùng, bài viết được chấm điểm đầu tiên luôn được tính là một lần chuyển ngữ cảnh. Thuật toán tính số lần chạy chủ đề chứ không tính số lần thay đổi chủ đề, do đó, giải pháp chấm điểm chỉ có bài luận A có câu trả lời là một chứ không phải không. Vì mọi câu hỏi đều cho phép mất nhiều nhất (HS-1), nên ít nhất một bài phải được chấm điểm, vì vậy câu trả lời không bao giờ bằng 0.
