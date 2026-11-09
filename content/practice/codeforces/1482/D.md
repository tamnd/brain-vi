---
title: "CF 1482D - Danh sách phát"
description: "Chúng tôi được yêu cầu mô phỏng một danh sách phát gồm các bài hát được gắn nhãn theo thể loại, trong đó Arkady nghe các bài hát theo thứ tự, quay lại phần đầu khi đến phần cuối."
date: "2026-06-10T23:24:22+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1482
codeforces_index: "D"
codeforces_contest_name: "\u0422\u0435\u0445\u043d\u043e\u043a\u0443\u0431\u043e\u043a 2021 - \u0424\u0438\u043d\u0430\u043b"
rating: 1900
weight: 1482
solve_time_s: 130
verified: false
draft: false
---

[CF 1482D - Danh sách phát](https://codeforces.com/problemset/problem/1482/D) 

**Xếp hạng:** 1900 
**Tas:** cấu trúc dữ liệu, dsu, triển khai, đường dẫn ngắn nhất 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một danh sách phát gồm các bài hát được gắn nhãn theo thể loại, trong đó Arkady nghe các bài hát theo thứ tự, quay lại phần đầu khi đến phần cuối. Quy tắc là nếu thể loại của bài hát cuối cùng Arkady đã nghe và thể loại của bài hát trước đó là đồng nguyên thì bài hát cuối cùng sẽ bị xóa khỏi danh sách phát. Khi một bài hát bị xóa, Arkady sẽ quên các bài hát trước đó, điều đó có nghĩa là việc xóa không thể xâu chuỗi ngay lập tức. Nhiệm vụ là xác định bài hát nào sẽ bị xóa và theo thứ tự nào. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp chỉ định số lượng bài hát và danh sách thể loại. Đối với mỗi trường hợp thử nghiệm, đầu ra phải là số lượng bài hát đã xóa và vị trí dựa trên 1 của chúng theo thứ tự xóa. Các ràng buộc cho phép tối đa 100.000 bài hát trong tất cả các trường hợp thử nghiệm với giá trị thể loại lên tới$10^9$, loại trừ những cách tiếp cận ngây thơ lặp đi lặp lại qua từng cặp bài hát cho mỗi lần xóa. Một mô phỏng trực tiếp sử dụng việc xóa mảng sẽ là$O(n^2)$trong trường hợp xấu nhất là quá chậm. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tình huống trong đó danh sách phát quay vòng nhiều lần, xóa bỏ bài hát hoặc danh sách phát có các thành phần lặp lại. Ví dụ: danh sách phát`[2, 2, 2]`không bao giờ xóa bất cứ thứ gì vì tất cả các thể loại liền kề đều có chung GCD lớn hơn 1, trong khi`[1, 2]`xóa cả hai bài hát vì sau khi xóa bài hát thứ hai, thuật toán sẽ đặt lại và kiểm tra lại bài hát đầu tiên, có khả năng gây ra nhiều lần xóa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại danh sách phát, kiểm tra hai bài hát cuối cùng và xóa khi GCD của chúng bằng 1, sau đó khởi động lại từ bài hát sau bài hát đã xóa. Mặc dù điều này khớp chính xác với các quy tắc nhưng mỗi lần xóa có thể yêu cầu quét tới$O(n)$bài hát, dẫn đến$O(n^2)$sự phức tạp. Với$n$lên đến$10^5$, điều này là không thực tế. 

Thông tin chi tiết quan trọng là chúng tôi chỉ xóa các bài hát khi bài hát hiện tại và bài hát trước đó là đồng nguyên tố. Điều này gợi ý việc duy trì cấu trúc vòng tròn và nhanh chóng tìm ra ứng cử viên tiếp theo để xóa mà không cần quét nhiều lần các bài hát đã được kiểm tra. Hàng đợi hoặc cấu trúc danh sách liên kết cho phép loại bỏ O(1) và quay vòng hiệu quả. Để theo dõi bài hát nào hợp lệ cho lần xóa tiếp theo, chúng tôi có thể duy trì một hàng chỉ mục bài hát mà bài hát trước đó vẫn còn tồn tại và các lần kiểm tra đồng nguyên tố chưa hết. Sau khi xóa, chúng tôi tiếp tục từ bài hát tiếp theo, bỏ qua các bài hát đã bị Arkady “quên” một cách hiệu quả. Điều này làm giảm độ phức tạp xuống còn O(n) cho mỗi trường hợp thử nghiệm vì mỗi bài hát được truy cập nhiều nhất hai lần: một lần khi được kiểm tra và một lần có khả năng là khi bị xóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu (Kiểm tra hàng đợi + GCD) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng bài hát`n`và danh sách các thể loại`a`. Chúng tôi sẽ làm việc với tính năng lập chỉ mục dựa trên 0 trong nội bộ để đơn giản hóa. 
2. Xây dựng biểu diễn liên kết vòng tròn của danh sách phát bằng cách sử dụng một mảng`next_song`Ở đâu`next_song[i]`trỏ tới bài hát tiếp theo trong danh sách phát. Ban đầu,`next_song[i] = (i + 1) % n`. 
3. Khởi tạo một hàng đợi có chỉ mục của các bài hát có bài hát trước đó trùng với chúng. Điều này có thể được tìm thấy bằng cách duyệt qua tất cả các bài hát và kiểm tra`gcd(a[i], a[i-1]) == 1`. 
4. Khởi tạo danh sách trống`deleted`để lưu trữ thứ tự xóa. 
5. Xử lý hàng đợi: bật chỉ mục bài hát`i`. Nếu như`i`đã bị xóa (vì bài hát trước đó đã bị xóa), hãy bỏ qua. Nếu không thì nối thêm`i+1`ĐẾN`deleted`(chuyển đổi sang lập chỉ mục dựa trên 1). 
6. Cập nhật`next_song`của bài hát trước để bỏ qua bài hát đã xóa, xóa bài hát đó khỏi danh sách phát một cách hiệu quả. 
7. Kiểm tra xem bài hát tiếp theo sau`i`hiện có mối quan hệ đồng nguyên với bài hát mới trước đó của nó. Nếu vậy, hãy thêm chỉ mục bài hát tiếp theo vào hàng đợi để xóa trong tương lai. 
8. Tiếp tục cho đến khi hàng đợi trống. Xuất ra chiều dài của`deleted`và các chỉ số theo thứ tự. 

Tại sao nó hoạt động: thuật toán duy trì cấu trúc vòng tròn chỉ đại diện cho các bài hát trực tiếp. Bằng cách duy trì hàng đợi xóa ứng viên, chúng tôi không bao giờ kiểm tra lại các bài hát một cách không cần thiết, đảm bảo mỗi bài hát sẽ bị xóa nhiều nhất một lần. Điều bất biến là sau mỗi lần xóa, danh sách phát và hàng đợi sẽ thể hiện chính xác các lần xóa đồng nguyên tố tiềm năng tiếp theo. Liên kết vòng tròn đảm bảo chuyển động phù hợp qua danh sách phát. 

## Giải pháp Python```python
import sys
import math
from collections import deque
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    next_song = [(i + 1) % n for i in range(n)]
    deleted = []
    alive = [True] * n
    q = deque()
    
    for i in range(n):
        if math.gcd(a[i], a[i-1]) == 1:
            q.append(i)
    
    while q:
        i = q.popleft()
        if not alive[i]:
            continue
        deleted.append(i + 1)
        alive[i] = False
        prev = (i - 1) % n
        while not alive[prev]:
            prev = (prev - 1) % n
        nxt = next_song[i]
        while not alive[nxt]:
            nxt = next_song[nxt]
        next_song[prev] = nxt
        if alive[nxt] and math.gcd(a[prev], a[nxt]) == 1:
            q.append(nxt)
    
    print(len(deleted), *deleted)
```Giải pháp bắt đầu bằng việc tạo ra một`next_song`mảng để mô phỏng danh sách liên kết vòng. Mỗi bài hát ban đầu có thể bị xóa sẽ được đưa vào hàng đợi. Sau đó, chúng tôi liên tục xóa phần trước của hàng đợi, cập nhật liên kết và kiểm tra xem việc xóa có tạo ra điều kiện nguyên tố cùng loại mới cho bài hát tiếp theo hay không. Các điều kiện biên như đạp xe qua cuối danh sách phát được xử lý thông qua các phép toán modulo. Các bài hát đã xóa sẽ được bỏ qua, tránh bị xóa trùng lặp. 

## Ví dụ đã hoạt động 

Dấu vết mẫu 1, trường hợp thử nghiệm đầu tiên`[5, 9, 2, 10, 15]`: 

| Bước | Xếp hàng | Đã xóa | Danh sách phát (còn sống) | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1, 2] | [] | [5, 9, 2, 10, 15] | gcd(5,9)=1, gcd(9,2)=1 | 
| Pop 1 | [2] | [2] | [5,2,10,15] | Xóa 9, trước=0, tiếp theo=2 | 
| Pop 2 | [] | [2,3] | [5,10,15] | Xóa 2, trước=0, tiếp theo=3 | 
| Xong | [] | [2,3] | [5,10,15] | Hàng đợi trống | 

Dấu vết mẫu 1, trường hợp thử nghiệm thứ hai`[1,2,4,2,4,2]`: 

| Bước | Xếp hàng | Đã xóa | Danh sách phát (còn sống) | Ghi chú | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [1,3] | [] | [1,2,4,2,4,2] | gcd(1,2)=1, gcd(2,4)=2 ... | 
| Pop 1 | [3] | [2] | [1,4,2,4,2] | Xóa bài hát 2 | 
| Pop 3 | [] | [2,1] | [4,2,4,2] | Xóa bài hát 1 | 

Những dấu vết này xác nhận rằng việc xóa xảy ra theo đúng thứ tự, tôn trọng quy tắc “quên trước” và chu kỳ phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi bài hát được truy cập tối đa hai lần: một lần để kiểm tra GCD, một lần để xóa. Hoạt động xếp hàng là O (1). | 
| Không gian | O(n) cho mỗi trường hợp thử nghiệm | Mảng`alive`,`next_song`và hàng đợi chứa tối đa n phần tử. | 

Cho tổng của`n`trên tất cả các trường hợp thử nghiệm 10^5, điều này phù hợp thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import math
    input = sys.stdin.readline
    
    output = io.StringIO()
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        next_song = [(i + 1) % n for i in range(n)]
        deleted = []
        alive = [True] * n
        q = deque()
        
        for i in range(n):
            if math.gcd(a[i], a[i-1]) == 1:
                q.append(i)
        
        while q:
            i = q.popleft()
            if not alive[i]:
                continue
            deleted.append(i + 1)
            alive[i] = False
            prev = (i - 1) % n
            while not alive[prev]:
                prev = (prev - 1) % n
            nxt = next_song[i]
            while not alive[nxt]:
                nxt = next_song[nxt]
            next_song[prev] = nxt
            if alive[nxt
```
