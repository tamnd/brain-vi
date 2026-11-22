---
title: "CF 33E - Người trợ giúp"
description: "Valera có một danh sách các môn học mà anh ấy biết cách giải quyết. Mỗi môn học đều có một lượng thời gian cố định. Học sinh đi kèm với các yêu cầu: mỗi yêu cầu có một môn học, thời hạn thi và phần thưởng. Nếu Valera hoàn thành bài giải một cách nghiêm túc trước khi kỳ thi bắt đầu, anh ấy sẽ được trả tiền."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 33
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 33 (Codeforces format)"
rating: 2600
weight: 33
solve_time_s: 177
verified: false
draft: false
---
[CF 33E - Người trợ giúp](https://codeforces.com/problemset/problem/33/E) 

**Đánh giá:** 2600 
**Thẻ:** - 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Valera có một danh sách các môn học mà anh ấy biết cách giải quyết. Mỗi môn học đều có một lượng thời gian cố định. Học sinh đi kèm với các yêu cầu: mỗi yêu cầu có một môn học, thời hạn thi và phần thưởng. Nếu Valera hoàn thành bài giải một cách nghiêm túc trước khi kỳ thi bắt đầu, anh ấy sẽ được trả tiền. 

Phần khó khăn là quản lý thời gian. Valera không hoạt động liên tục. Mỗi ngày đều có những khoảng thời gian bị chặn cho giấc ngủ và bữa ăn. Trong những khoảng thời gian rảnh rỗi, anh ta có thể làm việc, nhưng một khi đã bắt đầu giải quyết một vấn đề, anh ta phải tiếp tục cho đến khi nó hoàn thành, ngoại trừ những gián đoạn bắt buộc do giấc ngủ hoặc bữa ăn gây ra. Anh ta không thể tự nguyện tạm dừng và tiếp tục sau đó. 

Chúng ta cần chọn những học sinh nào sẽ giúp đỡ và theo thứ tự nào để tối đa hóa tổng phần thưởng. Đầu ra cũng phải xây dựng lại lịch trình chính xác, bao gồm dấu thời gian bắt đầu và kết thúc cho mọi yêu cầu được giải quyết. 

Các ràng buộc đủ nhỏ để đề xuất một giải pháp lập trình động, nhưng đủ lớn để tiêu diệt bạo lực. Có nhiều nhất 100 sinh viên và 30 ngày. Thời gian trong một ngày chỉ có 1440 phút nên toàn bộ lịch có nhiều nhất là 43200 phút. Một bảng liệt kê tập hợp con ngây thơ sẽ yêu cầu kiểm tra tới$2^{100}$tập hợp con, điều này là không thể. Ngay cả việc cố gắng sắp xếp tất cả các công việc được chấp nhận cũng là vô vọng. 

Cấu trúc quan trọng là thế này: 

1. Mọi nhiệm vụ đều có thời hạn. 
2. Mỗi công việc đều có thời gian xử lý tùy theo chủ đề. 
3. Năng lực làm việc theo thời gian mang tính quyết định. 
4. Sự gián đoạn là cố định và giống hệt nhau mỗi ngày. 

Điều này bắt đầu trông giống như việc lập kế hoạch có trọng số với thời hạn. 

Khó khăn tiềm ẩn đầu tiên là “thời gian thực tế trôi qua” và “thời gian làm việc thực tế” khác nhau. Giả sử một nhiệm vụ cần 60 phút làm việc và Valera bắt đầu lúc 08:00, nhưng bữa sáng chiếm 08:20-08:40. Thời điểm hoàn thành không phải là 09:00 mà là 09:20. 

Việc triển khai bất cẩn chỉ đơn giản là thêm khoảng thời gian theo thời gian trên đồng hồ treo tường sẽ tạo ra thời hạn sai. 

Hãy xem xét ví dụ này:```
1 1 1
math
60
00:00-00:10
00:20-00:30
12:00-12:10
18:00-18:10
math 1 01:15 100
```Valera chỉ có 50 phút làm việc trong khoảng thời gian từ 00:10 đến 01:10 vì bữa sáng có 10 phút. Anh ấy thực sự kết thúc lúc 01:20, vì vậy câu trả lời là 0. 

Một trường hợp tế nhị khác là điều kiện “đúng trước”. Hoàn thành chính xác khi bắt đầu kỳ thi không được tính.```
1 1 1
math
30
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
math 1 00:31 100
```Nếu công việc bắt đầu lúc 00:01 và kéo dài 30 phút thì thời gian hoàn thành là 00:31. Điều đó không hợp lệ vì bài thi cũng bắt đầu lúc 00:31. 

Cạm bẫy thứ ba đến từ những đối tượng không được hỗ trợ. Học sinh có thể yêu cầu các môn học ngoài danh sách của Valera. Những công việc đó phải bị bỏ qua hoàn toàn.```
1 1 1
math
10
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
physics 1 10:00 500
```Câu trả lời là 0 mặc dù có đủ thời gian. 

Phần khó khăn cuối cùng là tái thiết. Chúng tôi không chỉ tối đa hóa lợi nhuận mà còn phải in dấu thời gian bắt đầu và kết thúc chính xác. Bất kỳ DP nào mất thông tin trước đó sẽ không thể sử dụng được. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi nhóm nhỏ học sinh, hãy thử mọi thứ tự, mô phỏng lịch từng phút và kiểm tra xem tất cả thời hạn có được đáp ứng hay không. Trong số các lịch trình khả thi hãy chọn phần thưởng cao nhất. 

Tại sao điều này làm việc? Bởi vì một khi đơn hàng đã được cố định thì thời gian thực hiện sẽ mang tính quyết định. Chúng tôi luôn bắt đầu nhiệm vụ tiếp theo càng sớm càng tốt. Nếu một nhiệm vụ bị trễ thời hạn thực hiện sớm nhất, việc trì hoãn nó chỉ khiến mọi việc trở nên tồi tệ hơn. 

Vấn đề là sự bùng nổ của các quốc gia. Ngay cả đối với 20 sinh viên chúng tôi cũng đã phải đối mặt$20!$hoán vị. Với 100 sinh viên, không gian tìm kiếm rất lớn. 

Chúng ta cần một quan sát cấu trúc. 

Giả sử chúng ta chuyển dấu thời gian thực thành “số phút làm việc có thể sử dụng được”. Hãy tưởng tượng nén dòng thời gian bằng cách loại bỏ tất cả các khoảng thời gian ngủ và ăn. Khi đó mọi lịch trình khả thi sẽ được xử lý liên tục mà không bị gián đoạn. 

Cho từng khoảnh khắc thực sự$T$, định nghĩa:$$f(T) = \text{number of workable minutes strictly before } T$$Bây giờ đưa một học sinh có thời hạn thi$D$. Valera phải hoàn thành nghiêm ngặt trước$D$, do đó anh ta có thể chi tiêu tối đa:$$f(D)$$đơn vị công việc trước thời hạn đó. 

Sự chuyển đổi này là cái nhìn sâu sắc quan trọng. Sau khi nén, mọi công việc sẽ trở thành: 

1. Thời gian xử lý$p_i$2. Thời hạn$d_i = f(D_i)$3. Lợi nhuận$c_i$Bây giờ vấn đề là việc lập kế hoạch theo thời hạn theo kiểu cổ điển. 

Một quan sát quan trọng khác đến từ việc lập kế hoạch cho Thời hạn sớm nhất. Nếu một nhóm công việc đã chọn khả thi thì việc sắp xếp chúng theo thời hạn cũng khả thi. Điều đó có nghĩa là chúng ta không bao giờ cần phải xem xét các hoán vị tùy ý. 

Sau khi sắp xếp công việc theo thời hạn nén, chúng ta có thể chạy DP kiểu ba lô:$$dp[t] = \text{maximum profit achievable using exactly } t \text{ work minutes}$$Khi xử lý một công việc$(p,d,c)$, chúng ta chỉ có thể đặt nó nếu:$$t + p < d$$Bất đẳng thức chặt chẽ mã hoá “hoàn thành nghiêm ngặt trước khi thi”. 

Điều này làm giảm độ phức tạp xuống khoảng:$$O(n \cdot W)$$Ở đâu$W$là tổng số phút có thể làm được trong suốt thời gian thi, nhiều nhất là 43200. 

Điều đó dễ dàng phù hợp trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \cdot W)$|$O(W)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích các khoảng thời gian bị chặn hàng ngày và xây dựng lịch các phút khả thi trên tất cả$k$ngày. 

Chúng tôi đánh dấu mỗi phút là có sẵn hoặc bị chặn. Tổng chiều dài dòng thời gian tối đa là$30 \times 1440 = 43200$, vì vậy tiền xử lý rõ ràng là rẻ. 
2. Xây dựng hai mảng chuyển đổi. 

Mảng đầu tiên ánh xạ từng phút thực tới số phút có thể thực hiện được trước đó. 

Mảng thứ hai ánh xạ từng chỉ số phút khả thi trở lại phút thực tương ứng. 

Ánh xạ đầu tiên nén thời hạn. Ánh xạ thứ hai sẽ tái tạo lại dấu thời gian thực tế sau đó. 
3. Đọc tất cả các yêu cầu của học sinh. 

Nếu không rõ chủ đề, hãy loại bỏ yêu cầu ngay lập tức vì Valera không thể giải quyết được. 
4. Chuyển từng dấu thời gian thi thành thời hạn rút gọn. 

Giả sử kỳ thi bắt đầu vào phút thực$R$. Khi đó thời hạn công việc sẽ trở thành:$$d = f(R)$$Một lịch trình sử dụng chính xác$d$số phút khả thi kết thúc vào hoặc sau kỳ thi, vì vậy chỉ những trạng thái hoàn toàn nhỏ hơn$d$là hợp pháp. 
5. Sắp xếp công việc theo thời hạn nén. 

Điều này là đủ vì thứ tự EDF đảm bảo tính khả thi. 
6. Chạy DP ba lô trong thời gian khả thi. 

Cho phép:$$dp[t] = \text{maximum profit with total work } t$$Ban đầu chỉ$dp[0] = 0$là hợp lệ. 

Đối với mọi công việc, hãy lặp lại thời gian ngược lại. Nếu việc thêm công việc khiến tổng công việc thấp hơn thời hạn, hãy cập nhật trạng thái. 
7. Lưu trữ con trỏ cha trong quá trình chuyển đổi. 

Chúng ta cần xây dựng lại những công việc nào đã được chọn và thời gian làm việc tích lũy mà mỗi công việc kết thúc. 
8. Khôi phục các công việc đã chọn theo thứ tự ngược lại. 

Việc tái thiết tự nhiên tạo ra các công việc theo thứ tự thời hạn không giảm, là thứ tự thực hiện thực tế. 
9. Chuyển đổi khoảng thời gian làm việc đã nén thành dấu thời gian thực. 

Giả sử một công việc chiếm số phút khả thi$[L, R)$. Sử dụng mảng ánh xạ nghịch đảo, chúng ta có thể thu được: 

- bắt đầu từ phút thực 
- kết thúc phút thực 

Sau đó chuyển đổi số phút đó thành chỉ mục ngày và định dạng HH:MM. 

### Tại sao nó hoạt động 

Dòng thời gian được nén sẽ loại bỏ tất cả các khoảng thời gian không có sẵn. Bên trong không gian được biến đổi này, công việc tiến triển liên tục và mọi công việc đều tiêu tốn thời gian xử lý. 

Bất kỳ lịch trình khả thi nào cũng có thể được sắp xếp lại theo thời hạn không giảm mà không vi phạm tính khả thi, đó là đặc tính EDF cổ điển. Sau khi sắp xếp theo thời hạn, lựa chọn duy nhất còn lại là nên thực hiện tập hợp con công việc nào. 

Bất biến DP là:$$dp[t]$$lưu trữ lợi nhuận có thể đạt được tốt nhất trong số các công việc đã xử lý có tổng thời gian làm việc được nén bằng$t$. 

Việc chuyển đổi chỉ được phép nếu thời gian hoàn thành đạt được đúng trước thời hạn công việc. Vì thời gian nén tương ứng chính xác với số phút khả thi trong đời thực nên mọi trạng thái DP được chấp nhận đều tương ứng với một lịch trình thực tế hợp lệ. 

Ánh xạ nghịch đảo tái tạo lại dòng thời gian thực hiện thực tế sớm nhất duy nhất cho các công việc đã chọn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -(10 ** 18)

def parse_time(s):
    h, m = map(int, s.split(':'))
    return h * 60 + m

def parse_segment(s):
    a, b = s.split('-')
    return parse_time(a), parse_time(b)

def minute_to_output(x):
    day = x // 1440 + 1
    rem = x % 1440
    h = rem // 60
    m = rem % 60
    return day, f"{h:02d}:{m:02d}"

def solve():
    m, n, k = map(int, input().split())

    subjects = [input().strip() for _ in range(m)]
    times = list(map(int, input().split()))

    subj_time = {}
    for s, t in zip(subjects, times):
        subj_time[s] = t

    blocked = [False] * (k * 1440)

    segments = [parse_segment(input().strip()) for _ in range(4)]

    for day in range(k):
        base = day * 1440
        for l, r in segments:
            for x in range(l, r + 1):
                blocked[base + x] = True

    total_minutes = k * 1440

    pref = [0] * (total_minutes + 1)
    work_to_real = []

    for i in range(total_minutes):
        pref[i + 1] = pref[i]
        if not blocked[i]:
            pref[i + 1] += 1
            work_to_real.append(i)

    jobs = []

    for idx in range(1, n + 1):
        parts = input().split()

        subj = parts[0]

        if subj not in subj_time:
            continue

        day = int(parts[1]) - 1
        tm = parse_time(parts[2])
        money = int(parts[3])

        real_deadline = day * 1440 + tm

        deadline = pref[real_deadline]

        p = subj_time[subj]

        jobs.append((deadline, p, money, idx))

    jobs.sort()

    W = len(work_to_real)

    dp = [INF] * (W + 1)
    dp[0] = 0

    parent = [None] * (W + 1)

    for j, (deadline, p, money, idx) in enumerate(jobs):
        for t in range(W - p, -1, -1):
            if dp[t] == INF:
                continue

            nt = t + p

            if nt >= deadline:
                continue

            val = dp[t] + money

            if val > dp[nt]:
                dp[nt] = val
                parent[nt] = (t, j)

    best_t = 0
    best_profit = 0

    for t in range(W + 1):
        if dp[t] > best_profit:
            best_profit = dp[t]
            best_t = t

    chosen = []

    cur = best_t

    while cur != 0:
        prev, j = parent[cur]
        chosen.append((cur, jobs[j]))
        cur = prev

    chosen.reverse()

    print(best_profit)
    print(len(chosen))

    current_work = 0

    for finish_work, (deadline, p, money, idx) in chosen:
        start_work = current_work
        end_work = finish_work - 1

        start_real = work_to_real[start_work]
        end_real = work_to_real[end_work]

        sd, st = minute_to_output(start_real)
        ed, et = minute_to_output(end_real)

        print(idx, sd, st, ed, et)

        current_work = finish_work

solve()
```Giai đoạn tiền xử lý là trung tâm của việc thực hiện. Thay vì liên tục mô phỏng sự gián đoạn cho mọi tác vụ, mã sẽ nén toàn bộ lịch một lần.`pref[t]`lưu trữ bao nhiêu phút khả thi tồn tại trước phút thực`t`. Điều này biến việc kiểm tra thời hạn thành các so sánh số nguyên thông thường. 

Mảng`work_to_real`thực hiện ánh xạ nghịch đảo. Nếu số phút khả thi là 500 tương ứng với số phút thực là 900 thì một nhiệm vụ chiếm khoảng thời gian được nén`[500, 560)`tự động ánh xạ trở lại dấu thời gian thực tế. 

Bất đẳng thức nghiêm ngặt đáng được chú ý:```
if nt >= deadline:
    continue
```Một công việc hoàn thành đúng vào thời điểm thi là không hợp lệ nên sự bình đẳng phải bị bác bỏ. 

Một chi tiết tinh tế khác là tái thiết. DP chỉ lưu trữ giá trị tốt nhất cho tổng số lượng công việc, nhưng con trỏ cha sẽ bảo toàn khối lượng công việc trước đó và công việc đã chọn. Bởi vì quá trình chuyển đổi chạy lùi nên mỗi công việc chỉ được sử dụng tối đa một lần. 

Việc xây dựng lại lịch trình sử dụng phạm vi công việc đã được nén. Giả sử công việc tích lũy hiện tại trước khi một nhiệm vụ được thực hiện`start_work`và sau đó là`finish_work`. Nhiệm vụ chiếm số phút khả thi:```
[start_work, finish_work)
```Vì số phút là riêng biệt và dấu thời gian kết thúc được in là phút làm việc cuối cùng nên mã sử dụng:```
end_work = finish_work - 1
```Nếu không có phép trừ này, mỗi khoảng thời gian được in sẽ bị lệch đi một phút. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3 4
calculus
algebra
history
58 23 15
00:00-08:15
08:20-08:35
09:30-10:25
19:00-19:45
calculus 1 09:36 100
english 4 21:15 5000
history 1 19:50 50
```Dòng thời gian khả thi được nén vào ngày 1: 

| Khoảng thời gian thực | Số phút khả thi tích lũy | 

|---|---|---| 

| 08:16-08:19 | 4 | 

| 08:36-09:29 | 58 | 

| 10:26-19:49 | nhiều | 

Nhiệm vụ tính toán cần 58 phút có thể thực hiện được. Bắt đầu từ phút khả thi 0 kết thúc chính xác ở phút khả thi thứ 57, tương ứng với thời gian thực 09:29. 

Nhiệm vụ lịch sử cần 15 phút khả thi. Nó bắt đầu sau nhiệm vụ đầu tiên và kết thúc lúc 10:40. 

| Nhiệm vụ | Đang xử lý | Hạn chót | Đã chọn | 
| --- | --- | --- | --- | 
| phép tính | 58 | trước 09:36 | vâng | 
| lịch sử | 15 | trước 19:50 | vâng | 

Tổng lợi nhuận trở thành 150. 

Dấu vết này cho thấy lý do tại sao tính năng nén dòng thời gian lại hoạt động. Sự gián đoạn bữa sáng sẽ tự động biến mất khỏi lịch trình nén. 

### Ví dụ tùy chỉnh```
1 2 1
math
30
00:00-00:10
12:00-12:10
18:00-18:10
23:00-23:10
math 1 00:41 100
math 1 01:20 200
```Công việc có sẵn bắt đầu lúc 00:11. 

| Việc làm | Thời lượng | Thời hạn thực sự | Thời hạn nén | 
| --- | --- | --- | --- | 
| 1 | 30 | 00:41 | 30 | 
| 2 | 30 | 01:20 | 69 | 

Công việc 1 là không thể vì việc hoàn thành sẽ diễn ra đúng thời hạn. 

Công việc 2 là khả thi. 

| thời gian DP | Lợi nhuận | 
| --- | --- | 
| 0 | 0 | 
| 30 | 200 | 

Câu trả lời chỉ có học sinh thứ hai. 

Điều này chứng tỏ điều kiện bất đẳng thức chặt chẽ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot W)$| DP trên tất cả các công việc và số phút khả thi | 
| Không gian |$O(W)$| DP và mảng cha mẹ | 

Đây$W$là số phút có thể làm việc trong tất cả các ngày. Vì toàn bộ lịch chứa tối đa 43200 phút nên số lần chuyển đổi DP trong trường hợp xấu nhất là khoảng$100 \times 43200$, thoải mái trong giới hạn dành cho Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from contextlib import redirect_stdout
    out = io.StringIO()

    sys.stdin = io.StringIO(inp)

    def parse_time(s):
        h, m = map(int, s.split(':'))
        return h * 60 + m

    def parse_segment(s):
        a, b = s.split('-')
        return parse_time(a), parse_time(b)

    def minute_to_output(x):
        day = x // 1440 + 1
        rem = x % 1440
        h = rem // 60
        m = rem % 60
        return day, f"{h:02d}:{m:02d}"

    with redirect_stdout(out):
        INF = -(10 ** 18)

        input = sys.stdin.readline

        m, n, k = map(int, input().split())

        subjects = [input().strip() for _ in range(m)]
        times = list(map(int, input().split()))

        subj_time = dict(zip(subjects, times))

        blocked = [False] * (k * 1440)

        segments = [parse_segment(input().strip()) for _ in range(4)]

        for day in range(k):
            base = day * 1440
            for l, r in segments:
                for x in range(l, r + 1):
                    blocked[base + x] = True

        total_minutes = k * 1440

        pref = [0] * (total_minutes + 1)
        work_to_real = []

        for i in range(total_minutes):
            pref[i + 1] = pref[i]
            if not blocked[i]:
                pref[i + 1] += 1
                work_to_real.append(i)

        jobs = []

        for idx in range(1, n + 1):
            parts = input().split()

            subj = parts[0]

            if subj not in subj_time:
                continue

            day = int(parts[1]) - 1
            tm = parse_time(parts[2])
            money = int(parts[3])

            real_deadline = day * 1440 + tm
            deadline = pref[real_deadline]

            jobs.append((deadline, subj_time[subj], money, idx))

        jobs.sort()

        W = len(work_to_real)

        dp = [INF] * (W + 1)
        dp[0] = 0

        for deadline, p, money, idx in jobs:
            for t in range(W - p, -1, -1):
                if dp[t] == INF:
                    continue
                nt = t + p
                if nt >= deadline:
                    continue
                dp[nt] = max(dp[nt], dp[t] + money)

        print(max(dp))

    return out.getvalue().strip()

# sample 1
assert run("""3 3 4
calculus
algebra
history
58 23 15
00:00-08:15
08:20-08:35
09:30-10:25
19:00-19:45
calculus 1 09:36 100
english 4 21:15 5000
history 1 19:50 50
""").splitlines()[0] == "150"

# minimum size
assert run("""1 1 1
math
1
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
math 1 00:02 5
""").splitlines()[0] == "5"

# unsupported subject
assert run("""1 1 1
math
10
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
physics 1 10:00 100
""").splitlines()[0] == "0"

# strict deadline equality
assert run("""1 1 1
math
30
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
math 1 00:31 100
""").splitlines()[0] == "0"

# choose better subset
assert run("""1 3 1
math
10
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
math 1 00:20 10
math 1 00:30 100
math 1 00:40 50
""").splitlines()[0] == "150"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu | 5 | Trường hợp pháp lý nhỏ nhất | 
| Chủ đề không được hỗ trợ | 0 | Bỏ qua những công việc không thể | 
| Thời hạn bình đẳng | 0 | Điều kiện nghiêm ngặt trước | 
| Lựa chọn tập hợp con tốt hơn | 150 | DP chọn sự kết hợp tối ưu | 

## Vỏ cạnh 

Hãy xem xét ví dụ nhạy cảm với sự gián đoạn:```
1 1 1
math
60
00:00-00:10
00:20-00:30
12:00-12:10
18:00-18:10
math 1 01:15 100
```Công việc có thể làm trước 01:15 là: 

- 00:11-00:19, 9 phút 
- 00:31-01:14, 44 phút 

Tổng số: 53 phút khả thi. 

Thời hạn nén trở thành 53. Vì nhiệm vụ yêu cầu 60 phút nên quá trình chuyển đổi DP bị từ chối ngay lập tức. 

Một người bổ sung đồng hồ treo tường ngây thơ sẽ nghĩ sai 00:11 + 60 phút = 01:11 và chấp nhận nhiệm vụ. 

Bây giờ hãy xem xét ranh giới đẳng thức:```
1 1 1
math
30
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
math 1 00:31 100
```Công việc bắt đầu lúc 00:01. Ba mươi phút khả thi chiếm từ 00:01 đến 00:30, do đó quá trình hoàn thành diễn ra lúc 00:31. 

Thời hạn nén bằng 30. Quá trình chuyển đổi kiểm tra:```
if nt >= deadline:
```Đây`nt = 30`, do đó nhiệm vụ bị từ chối một cách chính xác. 

Cuối cùng, các chủ đề không được hỗ trợ:```
1 2 1
math
10
00:00-00:00
12:00-12:00
18:00-18:00
23:00-23:00
physics 1 10:00 100
math 1 10:00 50
```Yêu cầu vật lý không bao giờ đi vào mảng công việc. DP chỉ nhìn thấy nhiệm vụ toán học và trả về lợi nhuận 50. 

Điều này ngăn lịch trình không hợp lệ làm ảnh hưởng đến trạng thái tối ưu hóa.
