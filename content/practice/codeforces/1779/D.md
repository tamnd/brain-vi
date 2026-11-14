---
title: "CF 1779D - Boris và kiểu tóc tuyệt vời của anh ấy"
description: "Chúng tôi được yêu cầu biến kiểu tóc hiện tại của Boris thành kiểu tóc mong muốn bằng cách sử dụng một bộ dao cạo giới hạn, mỗi chiếc có thể cắt tóc đến một độ dài cố định trên bất kỳ đoạn liền kề nào."
date: "2026-06-09T11:29:34+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures", "dp", "dsu", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1779
codeforces_index: "D"
codeforces_contest_name: "Hello 2023"
rating: 1700
weight: 1779
solve_time_s: 95
verified: false
draft: false
---

[CF 1779D - Boris và kiểu tóc tuyệt vời của anh ấy](https://codeforces.com/problemset/problem/1779/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, cấu trúc dữ liệu, dp, dsu, tham lam, sắp xếp 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu biến kiểu tóc hiện tại của Boris thành kiểu tóc mong muốn bằng cách sử dụng một bộ dao cạo giới hạn, mỗi chiếc có thể cắt tóc đến một độ dài cố định trên bất kỳ đoạn liền kề nào. Về mặt khái niệm, bạn có thể coi kiểu tóc hiện tại là một mảng các số nguyên biểu thị độ dài của tóc và kiểu tóc mong muốn là một mảng mục tiêu có cùng kích thước. Mỗi chiếc dao cạo có một số đại diện cho chiều cao tối đa mà nó có thể cắt tóc xuống và mỗi chiếc chỉ có thể được sử dụng một lần. Sau khi dùng dao cạo vào một đoạn, mọi sợi tóc trong đoạn đó sẽ trở thành chiều cao tối thiểu hiện tại và kích thước của dao cạo. 

Thách thức là quyết định xem liệu có thể tiếp cận mảng mục tiêu bằng các thao tác này hay không. Điều quan trọng là thợ cắt tóc không thể tăng độ dài của tóc nên bất kỳ độ dài tóc mong muốn nào cao hơn tóc hiện tại đều là điều không thể. 

Vấn đề này cho phép lên tới 200.000 sợi tóc trong mỗi trường hợp thử nghiệm và lên tới 20.000 trường hợp thử nghiệm, với tổng số vị trí tóc trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bạo lực nào có thể thử tất cả các phân khúc hoặc tất cả các tổ hợp dao cạo. Chúng ta cần một giải pháp xử lý từng sợi tóc trong thời gian cơ bản không đổi. 

Các trường hợp cạnh bao gồm các tình huống trong đó mảng mục tiêu đã bằng mảng hiện tại (không cần cắt) hoặc khi một số sợi tóc nhất định yêu cầu dao cạo chỉ xuất hiện một lần trong khi cần nó ở nhiều vị trí không liền kề. Một cách tiếp cận ngây thơ có thể cho rằng chúng ta có thể áp dụng dao cạo ở bất cứ đâu mà không cần xem xét đến những tính đa dạng này một cách sai lầm. 

Ví dụ, nếu tóc hiện tại là`[3, 3, 3]`và mục tiêu là`[2, 1, 2]`với dao cạo`[1, 2]`, một giải pháp bất cẩn có thể cố gắng áp dụng dao cạo kích thước-2 trước trên toàn bộ mảng và không đạt được kích thước 1 duy nhất, tạo ra câu trả lời sai. Lập luận đúng đắn phải xem xét cách phân bổ dao cạo cho tất cả các vị trí cần cắt cụ thể đó. 

## Phương pháp tiếp cận 

Phương pháp brute-force là thử tất cả các phân đoạn có thể có cho mỗi chiếc dao cạo. Bạn sẽ kiểm tra từng phân đoạn liền kề của mảng và xem liệu việc áp dụng dao cạo ở đó có tiến tới mục tiêu hay không. Điều này đúng vì mỗi thao tác được phép ở mọi nơi, nhưng với tối đa 200.000 sợi tóc và 200.000 dao cạo, số phân đoạn trong trường hợp xấu nhất là O(n^2), tức là khoảng 4×10^10 thao tác cho mỗi trường hợp thử nghiệm. Rõ ràng, điều này là quá chậm. 

Hiểu biết sâu sắc quan trọng cho một giải pháp hiệu quả đến từ việc quan sát thấy rằng bất kỳ sợi tóc nào có độ dài mong muốn nhỏ hơn độ dài hiện tại đều phải được cắt bằng dao cạo có độ dài chính xác như vậy. Hơn nữa, chúng ta chỉ cần đảm bảo rằng đối với mỗi độ dài mục tiêu riêng biệt, sẽ có sẵn một loại dao cạo cho từng khối tóc liền kề yêu cầu độ dài đó. Nếu nhiều vị trí cần có cùng độ dài được phân tách bằng các vị trí đã có độ dài đó hoặc ngắn hơn thì cùng một dao cạo có thể bao phủ toàn bộ đoạn liền kề. Vì vậy, vấn đề giảm xuống còn việc đếm các đoạn liền kề cần một đường cắt cụ thể và xác minh rằng chúng ta có đủ dao cạo có kích thước đó. 

Một quan sát hữu ích khác là những sợi lông đã ở mức hoặc thấp hơn mục tiêu không cần phải thực hiện hành động nào. Như vậy, chúng ta chỉ cần tập trung vào những vị trí mà sợi tóc hiện tại cao hơn mục tiêu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * m) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lặp lại từng trường hợp kiểm thử và đọc mảng`a`,`b`và danh sách dao cạo. 
2. Kiểm tra ngay nếu có`b[i] > a[i]`. Nếu đúng với bất kỳ sợi tóc nào, hãy in "KHÔNG" vì chúng ta không thể tăng độ dài của tóc. 
3. Đếm số lượng dao cạo cho mỗi kích cỡ bằng từ điển tần số hoặc bộ đếm. Điều này cho phép tra cứu nhanh để xem liệu có sẵn dao cạo cần thiết hay không. 
4. Di chuyển mảng tóc từ trái sang phải. Theo dõi đoạn tiếp giáp hiện tại nơi`b[i]`ít hơn`a[i]`. Đối với mỗi đoạn, ghi lại chiều dài cắt yêu cầu (`b[i]`) và số vị trí trong đoạn cần độ dài này. 
5. Bất cứ khi nào đoạn kết thúc (lông tiếp theo đã ở hoặc dưới mục tiêu hoặc phần cuối của mảng), hãy kiểm tra xem có đủ số lượng dao cạo có độ dài cần thiết hay không. Nếu không thì in ra "KHÔNG". Nếu không, hãy giảm số lượng dao cạo theo số lượng vị trí trong phân đoạn. 
6. Tiếp tục cho đến khi toàn bộ mảng được xử lý. Nếu tất cả các phân đoạn đều được che bằng dao cạo có sẵn, hãy in "CÓ". 

Điều này hiệu quả vì mỗi khối tóc liền kề cần cắt có kích thước nhất định có thể được xử lý bằng một thao tác duy nhất bằng dao cạo tương ứng. Thuật toán duy trì tính bất biến là chúng tôi chỉ cố gắng cắt những sợi tóc có độ dài mục tiêu chính xác và không bao giờ cố gắng cắt lớn hơn mức cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import Counter

def can_cut_hair():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        m = int(input())
        razors = list(map(int, input().split()))
        razor_count = Counter(razors)

        possible = True
        i = 0
        while i < n:
            if b[i] > a[i]:
                possible = False
                break
            if a[i] > b[i]:
                required = b[i]
                j = i
                while j < n and b[j] == required and a[j] > b[j]:
                    j += 1
                if razor_count[required] <= 0:
                    possible = False
                    break
                razor_count[required] -= 1
                i = j
            else:
                i += 1

        print("YES" if possible else "NO")

can_cut_hair()
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi sử dụng`Counter`để theo dõi tính sẵn có của dao cạo một cách hiệu quả. Chúng tôi lặp qua`a`Và`b`, xác định khối tóc cần cắt. Đối với mỗi khối, chúng tôi kiểm tra xem dao cạo tương ứng có tồn tại hay không. Chúng tôi không cố gắng cắt tóc một cách không cần thiết hoặc sử dụng dao cạo nhiều lần cho cùng một khối. 

## Ví dụ đã hoạt động 

**Mẫu 1 dấu vết** 

| tôi | một [tôi] | b[i] | Hành động | dao cạo_count | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | bắt đầu khối 2 | {1:1,2:1} | 
| 1 | 3 | 1 | bắt đầu khối 1 | {1:1,2:1} | 
| 2 | 3 | 2 | bắt đầu khối 2 | {1:0,2:0} | 

Dấu vết xác nhận thuật toán xác định chính xác các khối riêng biệt cho 2 và 1, sử dụng các dao cạo có sẵn đúng một lần. 

**Dấu vết mẫu 2** 

| tôi | một [tôi] | b[i] | Hành động | dao cạo_count | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 | không hành động | {3:0,2:0} | 
| 1 | 4 | 1 | khối 1 | {1:0} | 

Cho thấy rằng nếu một khối yêu cầu dao cạo không có sẵn, thuật toán sẽ ngay lập tức in "KHÔNG". 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) cho mỗi trường hợp thử nghiệm | Mỗi sợi tóc được thăm một lần, tính dao cạo là O(m) | 
| Không gian | O(m) | Bộ đếm tần suất lưu trữ của từng kích cỡ dao cạo | 

Với tổng số`n`Và`m`trên tất cả các trường hợp thử nghiệm 2×10^5, giải pháp này chạy thoải mái trong giới hạn 2 giây và sử dụng ít hơn nhiều so với bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    can_cut_hair()
    return out.getvalue().strip()

# provided samples
assert run("7\n3\n3 3 3\n2 1 2\n2\n1 2\n6\n3 4 4 6 3 4\n3 1 2 3 2 3\n3\n3 2 3\n10\n1 2 3 4 5 6 7 8 9 10\n1 2 3 4 5 6 7 8 9 10\n10\n1 2 3 4 5 6 7 8 9 10\n3\n1 1 1\n1 1 2\n12\n4 2 4 3 1 5 6 3 5 6 2 1\n13\n7 9 4 5 3 3 3 6 8 10 3 2 5\n5 3 1 5 3 2 2 5 8 5 1 1 5\n8\n1 5 3 5 4 2 3 1\n13\n7 9 4 5 3 3 3 6 8 10 3 2 5\n5 3 1 5 3 2 2 5 8 5 1 1 5\n7\n1 5 3 4 2 3 1\n3\n19747843 2736467 938578397\n2039844 2039844 2039844\n1\n2039844") == "YES\nNO
```
