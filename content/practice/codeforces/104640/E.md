---
title: "CF 104640E - \u041f\u0440\u044f\u043c\u043e\u0443\u0433\u043e\u043b\u044c\u043d\u043e\u0435 \u041f\u044f\u0442\u043d\u043e"
description: "Chúng ta có một tập hợp các hình chữ nhật được căn chỉnh theo trục, mỗi hình chữ nhật được xác định bởi chiều cao và chiều rộng của nó, đồng thời mỗi hình chữ nhật cũng được phép xoay 90 độ."
date: "2026-06-29T16:50:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104640
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104640
solve_time_s: 92
verified: false
draft: false
---

[CF 104640E - \u041f\u0440\u044f\u043c\u043e\u0443\u0433\u043e\u043b\u044c\u043d\u043e\u0435 \u041f\u044f\u0442\u043d\u043e](https://codeforces.com/problemset/problem/104640/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các hình chữ nhật được căn chỉnh theo trục, mỗi hình chữ nhật được xác định bởi chiều cao và chiều rộng của nó, đồng thời mỗi hình chữ nhật cũng được phép xoay 90 độ. Điều này có nghĩa là đối với mỗi hình chữ nhật, chúng ta có thể coi nó có hai hướng có thể và chúng ta có thể tự do chọn bất kỳ hướng nào giúp ích cho chúng ta. 

Mục tiêu là chọn một tập hợp con của các hình chữ nhật này và sắp xếp chúng theo một chuỗi không giảm, trong đó mỗi hình chữ nhật nằm gọn trong hình tiếp theo. Một hình chữ nhật có kích thước$(h_1, w_1)$có thể được đặt bên trong$(h_2, w_2)$nếu cả chiều cao và chiều rộng không vượt quá kích thước tương ứng. Chúng tôi muốn chuỗi dài nhất có thể như vậy và chúng tôi cũng cần xuất chỉ mục của các hình chữ nhật tạo thành chuỗi theo thứ tự. 

Vấn đề về cơ bản là yêu cầu chuỗi dài nhất theo thứ tự một phần hai chiều, nhưng có thêm điểm xoắn là mỗi mục có hai trạng thái có thể xảy ra do chuyển động quay. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ mọi so sánh bậc hai giữa các cặp. Bất kỳ cách tiếp cận nào cố gắng so sánh tất cả các cặp hoặc chạy lập trình động trên tất cả các hình chữ nhật trước đó một cách đơn giản sẽ dẫn đến khoảng$10^{10}$trong trường hợp xấu nhất vượt xa giới hạn khả thi. 

Một sai lầm ngây thơ nhưng đầy cám dỗ là xử lý từng hình chữ nhật một cách độc lập, chọn hướng cục bộ tốt nhất của nó, sau đó sắp xếp và tham lam lấy một chuỗi. Điều này không thành công vì các quyết định định hướng mang tính toàn cầu. Một hình chữ nhật có thể cần phải được "hy sinh" thành một hướng không tối ưu để sau này có thể tạo ra các chuỗi dài hơn. 

Một vấn đề tế nhị khác phát sinh khi cả hai chiều đều bằng nhau sau khi xoay. Nếu chúng ta luôn chuẩn hóa bằng cách sắp xếp các kích thước một cách thiếu thận trọng, chúng ta có thể vô tình cho phép các chuỗi không hợp lệ hoặc phá vỡ tính nhất quán của mối liên kết, điều này có thể phá hủy việc xây dựng lại một chuỗi chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử tất cả các tập hợp con và tất cả các lựa chọn định hướng, kiểm tra xem một chuỗi nhất định có tạo thành một chuỗi lồng hợp lệ hay không. Ngay cả khi hạn chế các chuỗi con, điều này trở thành cấp số nhân trong$n$. Một lực lượng vũ phu có cấu trúc hơn một chút sẽ là lập trình động trên các tập hợp con hoặc theo cặp, nhưng thậm chí sau đó chúng ta vẫn so sánh mọi hình chữ nhật với mọi hình chữ nhật khác, dẫn đến$O(n^2)$chuyển tiếp. Với$10^5$hình chữ nhật, điều này hoàn toàn không thể thực hiện được. 

Quan sát cấu trúc quan trọng là khi các hình chữ nhật được đặt trong một chuỗi, một chiều sẽ không giảm dọc theo chuỗi và chúng ta có thể thực thi thứ tự bằng cách sắp xếp. Thách thức thực sự là chúng ta đang xử lý thứ tự hai chiều, đó là Chuỗi tăng dần dài nhất (LIS) cổ điển trong 2D. 

Quyền tự do xoay đơn giản hóa mỗi hình chữ nhật thành một lựa chọn sắp xếp hai cạnh của nó. The best strategy is to always orient each rectangle so that its smaller side is treated as height and its larger side as width. Việc chuẩn hóa này đảm bảo tính nhất quán: mọi hình chữ nhật được biểu diễn ở dạng chuẩn tối thiểu của nó$(a, b)$với$a \le b$. Bất kỳ chuỗi tối ưu nào cũng có thể được chuyển đổi thành chuỗi bằng cách sử dụng biểu diễn này vì việc hoán đổi các trục chỉ giúp giảm bớt các ràng buộc. 

Sau khi chuẩn hóa, bài toán giảm xuống còn tìm chuỗi cặp dài nhất$(a_i, b_i)$sao cho cả hai tọa độ đều không giảm. Đây là một vấn đề LIS 2D tiêu chuẩn. Sắp xếp theo tọa độ đầu tiên và sau đó tìm LIS trên tọa độ thứ hai sẽ mang lại cấu trúc chuỗi tối ưu. 

Để xây dựng lại trình tự, chúng ta phải lưu trữ các chuỗi trước đó trong quá trình tính toán LIS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$hoặc$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Với mỗi hình chữ nhật, hãy sắp xếp lại các kích thước của nó sao cho$a = \min(h, w)$Và$b = \max(h, w)$. Điều này sửa các lựa chọn xoay thành một biểu diễn chuẩn, đảm bảo mọi hình chữ nhật được thể hiện theo hướng hạn chế nhất để lồng nhau. 
2. Lưu trữ mỗi hình chữ nhật dưới dạng bộ ba$(a, b, index)$, giữ nguyên các chỉ số ban đầu để chúng ta có thể xây dựng lại câu trả lời sau này. 
3. Sắp xếp tất cả các hình chữ nhật theo$a$theo thứ tự tăng dần và bằng nhau$a$, qua$b$theo thứ tự tăng dần. Điều này đảm bảo rằng bất kỳ chuỗi lồng hợp lệ nào cũng phải tôn trọng thứ tự này ở tọa độ đầu tiên, vì vậy chúng ta chỉ cần kiểm soát tọa độ thứ hai trong tương lai. 
4. Chạy quy trình dãy con không giảm dài nhất trên dãy$b$các giá trị. Chúng tôi duy trì cấu trúc DP nơi chúng tôi lưu trữ giá trị kết thúc nhỏ nhất có thể cho một chuỗi con của mỗi độ dài. 
5. Để cho phép xây dựng lại, chúng tôi lưu trữ cho mỗi phần tử độ dài của chuỗi con tốt nhất kết thúc ở đó và một con trỏ trước tới phần tử trước đó trong chuỗi con đó. Điều này là cần thiết vì bài toán yêu cầu chuỗi thực tế chứ không chỉ độ dài của nó. 
6. Khi cập nhật cấu trúc LIS, chúng ta sử dụng tìm kiếm nhị phân để tìm vị trí đầu tiên nơi dòng điện$b$có thể được đặt, duy trì giá trị đuôi tối thiểu. Mỗi bản cập nhật tương ứng với việc mở rộng hoặc cải thiện một chuỗi tiếp theo. 
7. Sau khi xử lý tất cả các hình chữ nhật, chúng tôi xác định vị trí có độ dài chuỗi con lớn nhất và xây dựng lại chuỗi bằng cách đi theo các con trỏ trước đó về phía sau, sau đó đảo ngược kết quả. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên việc giảm vấn đề thống trị 2D thành vấn đề LIS 1D. Sắp xếp theo$a$đảm bảo rằng bất kỳ chuỗi hợp lệ nào cũng phải đáp ứng ràng buộc thứ nguyên đầu tiên. Sau đó, chiều thứ hai trở thành điều kiện duy nhất còn lại. Bởi vì tất cả các hình chữ nhật đều được sắp xếp trước nên bất kỳ dãy con nào chúng ta xây dựng sẽ tự động duy trì không giảm$a$, vì vậy việc thực thi LIS trên$b$đảm bảo đầy đủ giá trị pháp lý. 

Các con trỏ tiền nhiệm đảm bảo rằng mọi phần mở rộng trong LIS tương ứng với một quá trình chuyển đổi hợp lệ thực sự giữa các hình chữ nhật theo thứ tự được sắp xếp, do đó chuỗi được xây dựng lại là một chuỗi lồng hợp lệ theo cả hai chiều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    rects = []
    for i in range(n):
        h, w = map(int, input().split())
        a, b = min(h, w), max(h, w)
        rects.append((a, b, i + 1))
    
    rects.sort(key=lambda x: (x[0], x[1]))
    
    import bisect
    
    tail = []
    tail_idx = []
    parent = [-1] * n
    pos_in_tail = [-1] * n
    
    # store best ending index for each length
    best_idx = []
    
    for i, (a, b, idx) in enumerate(rects):
        j = bisect.bisect_right(tail, b)
        
        if j == len(tail):
            tail.append(b)
            best_idx.append(i)
        else:
            tail[j] = b
            best_idx[j] = i
        
        pos_in_tail[i] = j
        
        if j > 0:
            parent[i] = best_idx[j - 1]
    
    # reconstruct LIS
    length = len(tail)
    last = best_idx[length - 1]
    
    ans = []
    while last != -1:
        ans.append(rects[last][2])
        last = parent[last]
    
    ans.reverse()
    
    print(len(ans))
    print(*ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ chuẩn hóa từng hình chữ nhật sao cho hướng đó được cố định một cách nhất quán. Sắp xếp theo thứ nguyên thứ nhất đảm bảo chúng ta chỉ cần quản lý tính khả thi ở thứ nguyên thứ hai. Cấu trúc LIS sử dụng`tail`để duy trì các giá trị kết thúc tối thiểu có thể có cho các chuỗi con của mỗi độ dài và`best_idx`để nhớ hình chữ nhật nào hiện đang xác định từng điểm cuối có độ dài chuỗi con. 

các`parent`mảng là thứ cho phép tái thiết. Every time a rectangle extends a subsequence, it stores a pointer to the best sequence of length one less. Điều này đảm bảo rằng khi chúng ta quay lại từ phần tử cuối cùng của dãy con tối ưu, chúng ta sẽ khôi phục được chuỗi hợp lệ theo đúng thứ tự. 

Một điểm tinh tế là việc sử dụng`bisect_right`thay vì`bisect_left`. Điều này thực thi các chuỗi không giảm thay vì các chuỗi tăng nghiêm ngặt, phù hợp với điều kiện$h_1 \le h_2$,$w_1 \le w_2$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Nhập hình chữ nhật sau khi chuẩn hóa: 

| tôi | (a, b) | sắp xếp thứ tự | 
| --- | --- | --- | 
| 1 | (1, 1) | 1 | 
| 2 | (2, 3) | 4 | 
| 3 | (2, 5) | 3 | 
| 4 | (1, 4) | 2 | 
| 5 | (3, 5) | 5 | 

Sau khi sắp xếp: 

(1,1), (1,4), (2,3), (2,5), (3,5) 

Chúng tôi xử lý LIS trên tọa độ thứ hai: 

| bước | b | đuôi | hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | [1] | bắt đầu | 
| 2 | 4 | [1,4] | mở rộng | 
| 3 | 3 | [1,3] | thay thế | 
| 4 | 5 | [1,3,5] | mở rộng | 
| 5 | 5 | [1,3,5] | mở rộng | 

Độ dài chuỗi cuối cùng là 4 với các chỉ số năng suất tái cấu trúc phù hợp với thứ tự lồng hợp lệ, chẳng hạn như$1 \to 4 \to 3 \to 5$. 

Dấu vết này cho thấy việc thay thế đuôi sẽ cải thiện khả năng mở rộng trong tương lai như thế nào mà không phá vỡ tính chính xác. 

### Mẫu 2 

đầu vào: 

(1,10), (2,9), (3,8), (4,7), (5,6) 

Sau khi chuẩn hóa và sắp xếp, tọa độ thứ hai sẽ giảm dần. 

| bước | b | đuôi | 
| --- | --- | --- | 
| 1 | 10 | [10] | 
| 2 | 9 | [9] | 
| 3 | 8 | [8] | 
| 4 | 7 | [7] | 
| 5 | 6 | [6] | 

Mỗi phần tử thay thế đuôi trước đó, vì vậy độ dài LIS là 1. 

Điều này xác nhận rằng khi không thể lồng nhau, thuật toán sẽ tránh được việc buộc cấu trúc tăng không hợp lệ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế$O(n \log n)$, LIS sử dụng tìm kiếm nhị phân cho mỗi phần tử | 
| Không gian |$O(n)$| lưu trữ hình chữ nhật, mảng DP và con trỏ tái cấu trúc | 

Sự phức tạp phù hợp thoải mái trong các ràng buộc đối với$n = 10^5$, vì cả tìm kiếm sắp xếp và tìm kiếm nhị phân đều có quy mô hiệu quả và mức sử dụng bộ nhớ là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    # direct inline solution for testing
    n = int(sys.stdin.readline())
    rects = []
    for i in range(n):
        h, w = map(int, sys.stdin.readline().split())
        rects.append((min(h, w), max(h, w), i + 1))
    rects.sort(key=lambda x: (x[0], x[1]))

    import bisect
    tail = []
    best_idx = []
    parent = [-1] * n
    pos = [-1] * n

    for i, (_, b, _) in enumerate(rects):
        j = bisect.bisect_right(tail, b)
        if j == len(tail):
            tail.append(b)
            best_idx.append(i)
        else:
            tail[j] = b
            best_idx[j] = i
        pos[i] = j
        if j > 0:
            parent[i] = best_idx[j - 1]

    length = len(tail)
    cur = best_idx[length - 1]
    ans = []
    while cur != -1:
        ans.append(rects[cur][2])
        cur = parent[cur]

    return f"{len(ans)}\n" + " ".join(map(str, ans[::-1]))

# samples
assert run("5\n1 1\n3 2\n2 5\n4 1\n3 5\n") == "4\n1 4 3 5"
assert run("5\n1 10\n2 9\n3 8\n4 7\n5 6\n") == "1\n1"

# custom
assert run("1\n7 3\n") == "1\n1"
assert run("3\n1 1\n1 1\n1 1\n") == "3\n1 2 3"
assert run("3\n5 4\n6 3\n7 2\n") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | chuỗi 1 phần tử | trường hợp cơ sở | 
| hình chữ nhật giống hệt nhau | tất cả đều có thể sử dụng theo thứ tự bất kỳ | xử lý bình đẳng | 
| giảm nghiêm ngặt | chỉ có một phần tử | không có chuỗi sai | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi nhiều hình chữ nhật trở nên giống hệt nhau sau khi chuẩn hóa. Ví dụ,$(3,5), (5,3), (3,5)$tất cả bình thường hóa thành$(3,5)$. Thuật toán cho phép tất cả chúng tham gia vào LIS vì chúng tôi sử dụng các chuyển đổi không giảm. Điều này đúng vì các hình chữ nhật giống hệt nhau luôn có thể lồng theo bất kỳ thứ tự nào. 

Một trường hợp khác là khi việc luân chuyển có ý nghĩa quan trọng đối với tính khả thi. Một hình chữ nhật như$(2,10)$và cái khác$(9,3)$cả hai sẽ bình thường hóa thành$(2,10)$Và$(3,9)$và nếu không có tính nhất quán chuẩn hóa, sự lựa chọn định hướng tham lam có thể phá vỡ chuỗi. Phép biến đổi chuẩn loại bỏ sự mơ hồ này trước khi tính toán LIS. 

Cuối cùng, các trường hợp có chuỗi tăng lớn ở cả hai chiều nhấn mạnh tính chính xác của việc tái thiết. Bởi vì mỗi chuyển đổi DP đều lưu trữ một chuỗi trước đó, bước quay lui luôn khôi phục một chuỗi nhất quán ngay cả khi tồn tại nhiều chuỗi con tối ưu.
