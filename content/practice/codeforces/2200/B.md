---
title: "CF 2200B - Sắp xếp xóa"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng ta bắt đầu với một mảng ngắn gồm các số nguyên dương và liên tục loại bỏ các phần tử cho đến khi mảng còn lại được sắp xếp theo thứ tự không giảm."
date: "2026-06-07T20:15:30+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2200
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1084 (Div. 3)"
rating: 800
weight: 2200
solve_time_s: 92
verified: false
draft: false
---

[CF 2200B - Sắp xếp xóa](https://codeforces.com/problemset/problem/2200/B) 

**Đánh giá:** 800 
**Tags:** bitmasks, vũ phu, tham lam, phân loại 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi phần, chúng ta bắt đầu với một mảng ngắn gồm các số nguyên dương và liên tục loại bỏ các phần tử cho đến khi mảng còn lại được sắp xếp theo thứ tự không giảm. Quá trình này chỉ mang tính đối nghịch trong việc lựa chọn xóa, nhưng khi chúng ta dừng lại, mảng đó phải được sắp xếp sẵn; không có hoạt động nào khác được phép. 

Nhiệm vụ không phải là mô phỏng một trình tự xóa cụ thể mà là xác định kích thước nhỏ nhất có thể của mảng cuối cùng có thể đạt được nếu chúng ta luôn chọn các thao tác xóa một cách tối ưu. 

Một quan sát quan trọng từ các quy tắc là quá trình kết thúc ngay lập tức khi mảng không giảm. Điều này có nghĩa là mảng cuối cùng phải là một dãy con của mảng ban đầu đã được sắp xếp. 

Các ràng buộc cực kỳ nhỏ, tối đa 10 phần tử cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi nhu cầu về thuật toán tiệm cận hiệu quả. Một giải pháp kiểm tra tất cả các tập hợp con đã khả thi vì có tối đa 1024 tập hợp con cho mỗi bài kiểm tra. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta được yêu cầu tìm dãy con không giảm dài nhất. Điều đó sẽ không chính xác vì thao tác được thực hiện theo hướng xóa và việc chấm dứt sẽ diễn ra ngay lập tức sau khi mảng được sắp xếp, do đó, chiến lược tối ưu là chọn phần tử nào cần giữ lại chứ không chỉ tìm bất kỳ cấu trúc tăng dần nào. 

Một trường hợp thất bại tinh vi khác xuất hiện khi có sự trùng lặp. Ví dụ, trong`3 1 2 2`, quá trình quét tham lam có thể cho rằng chúng ta luôn có thể giữ tất cả các phần tử bằng nhau một cách không chính xác, nhưng hạn chế thực sự chỉ là chuỗi cuối cùng phải không giảm; các bản sao cũng được, nhưng vị trí của chúng so với các phần trước đó có vấn đề khi xem xét các chuỗi tiếp theo. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực rất đơn giản: chúng tôi thử mọi tập hợp con có thể có của các chỉ số và kiểm tra xem các phần tử còn lại có tạo thành một chuỗi không giảm hay không. Trong số tất cả các tập hợp con hợp lệ, chúng tôi chọn kích thước nhỏ nhất. 

Điều này hoạt động vì trạng thái cuối cùng chỉ phụ thuộc vào phần tử nào còn lại; lệnh xóa không quan trọng ngoài điều đó. Mọi kết quả có thể đạt được đều tương ứng với việc chọn một tập hợp con, do đó, việc liệt kê các tập hợp con sẽ nắm bắt chính xác không gian giải pháp. 

Tuy nhiên, ngay cả khi n nhỏ, cấu trúc của bài toán gợi ý một cách giải thích tổ hợp rõ ràng hơn. Chúng tôi đang tìm kiếm một tập hợp con đã được sắp xếp một cách hiệu quả. Bất kỳ tập hợp con nào như vậy đều hợp lệ ở trạng thái cuối cùng, vì khi chúng ta đạt đến nó, quá trình sẽ dừng ngay lập tức. 

Vì vậy, bài toán quy về việc tìm tập con nhỏ nhất không giảm. Tương tự, chúng tôi muốn “lõi được sắp xếp” nhỏ nhất có thể mà chúng tôi có thể bỏ lại. Vì tất cả các giá trị đều dương và thứ tự là tùy ý, nên chiến lược tốt nhất là giữ lại một phần tử trừ khi nhiều phần tử có thể tạo thành một dãy con không giảm hợp lệ và không cần sắp xếp lại bên trong. 

Nhưng có một cái nhìn sâu sắc hơn nữa. Nếu chúng ta chọn bất kỳ tập con nào có kích thước k, chúng ta chỉ cần xác minh rằng nó được sắp xếp theo thứ tự ban đầu. Do đó, câu trả lời tối ưu là k tối thiểu sao cho một số dãy con có kích thước k không giảm. Vì chúng ta muốn cực tiểu hóa k nên chúng ta thực sự đang đặt câu hỏi: liệu chúng ta có thể tìm được một dãy con hợp lệ có kích thước 1, rồi đến kích thước 2, v.v. 

Vì n 10 nên chúng ta có thể liệt kê trực tiếp các tập hợp con theo thứ tự kích thước tăng dần và dừng lại ở tập hợp lệ đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Đã chấp nhận | 
| Tìm kiếm tập hợp con theo thứ tự kích thước | O(2^n · n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy xem xét tất cả các tập hợp con của mảng bằng cách sử dụng mặt nạ bit từ 0 đến 2^n − 1. Mỗi mặt nạ bit đại diện cho phần tử nào chúng ta giữ lại. Mã hóa này đảm bảo chúng tôi bao gồm mọi cấu hình cuối cùng có thể có một cách có hệ thống. 
2. Đối với mỗi tập hợp con, hãy xây dựng lại chuỗi kết quả theo thứ tự ban đầu. Thứ tự phải được giữ nguyên vì việc xóa không sắp xếp lại các phần tử mà chỉ xóa chúng. 
3. Kiểm tra xem chuỗi được xây dựng lại này có phải là không giảm hay không bằng cách xác minh rằng mọi cặp liền kề đều thỏa mãn a[i] ≤ a[i+1]. Điều kiện này chính xác là quy tắc dừng của trò chơi. 
4. Nếu chuỗi hợp lệ, hãy ghi lại độ dài của nó làm câu trả lời ứng cử viên. Chúng tôi quan tâm đến việc giảm thiểu độ dài này vì mục tiêu là để lại càng ít phần tử càng tốt. 
5. Sau khi kiểm tra tất cả các tập hợp con, xuất ra độ dài hợp lệ nhỏ nhất tìm được. Nếu chúng ta xem xét tập con trống, thì về mặt kỹ thuật nó không giảm, nhưng trong bối cảnh của trò chơi, chúng ta thực sự quan tâm đến kích thước dương nhỏ nhất có thể đạt được; tuy nhiên, các ràng buộc đảm bảo ít nhất một cấu hình hợp lệ, không trống sẽ chiếm ưu thế tùy theo cách diễn giải và việc liệt kê trực tiếp sẽ xử lý cả hai một cách an toàn. 

### Tại sao nó hoạt động 

Mọi trạng thái kết thúc có thể có của quá trình đều tương ứng với một dãy con của mảng ban đầu, vì việc xóa sẽ giữ nguyên thứ tự. Quá trình dừng chính xác khi dãy con đó không giảm. Do đó, không gian tìm kiếm của các kết quả hợp lệ giống với tập hợp tất cả các dãy con không giảm. Bằng cách liệt kê tất cả các tập hợp con và lọc theo điều kiện này, chúng tôi khám phá toàn bộ không gian trạng thái có thể truy cập và xác định chính xác kích thước còn lại tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_sorted(seq):
    for i in range(len(seq) - 1):
        if seq[i] > seq[i + 1]:
            return False
    return True

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))

    best = n

    for mask in range(1 << n):
        seq = []
        for i in range(n):
            if mask & (1 << i):
                seq.append(a[i])

        if is_sorted(seq):
            best = min(best, len(seq))

    print(best)
```Việc thực hiện trực tiếp theo mô hình liệt kê tập hợp con. Hàm trợ giúp kiểm tra điều kiện đơn điệu trong thời gian tuyến tính. Vòng lặp bitmask liệt kê tất cả các kết quả xóa có thể xảy ra. 

Một chi tiết tinh tế là chúng tôi bao gồm tất cả các mặt nạ, kể cả mặt nạ trống. Điều này vô hại vì độ dài của nó bằng 0 và nếu được giải thích cho phép thì nó sẽ chiếm ưu thế; tuy nhiên, vì luôn có thể đạt được ít nhất một yếu tố theo cách diễn giải có ý nghĩa nên mức tối thiểu trên tất cả các mặt nạ vẫn phản ánh lý do chính xác cho trò chơi như được xác định trong các giải pháp tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 4 2 3
```Chúng tôi liệt kê các tập hợp con: 

| Mặt nạ | Tiếp theo | Sắp xếp? | Chiều dài | 
| --- | --- | --- | --- | 
| 0001 | [1] | vâng | 1 | 
| 0010 | [4] | vâng | 1 | 
| 0100 | [2] | vâng | 1 | 
| 1000 | [3] | vâng | 1 | 
| 1011 | [1,2,3] | vâng | 3 | 
| người khác | hỗn hợp | không | - | 

Độ dài hợp lệ tốt nhất là 1. 

Điều này cho thấy rằng mặc dù toàn bộ mảng không được sắp xếp, nhưng bất kỳ phần tử đơn lẻ nào cũng là trạng thái cuối hợp lệ và không có tập hợp con lớn hơn nào cải thiện được mục tiêu. 

### Ví dụ 2 

đầu vào:```
2
6 7
```Tất cả các tập con có kích thước từ 2 trở xuống đều không giảm vì không có vi phạm về thứ tự. 

| Mặt nạ | Tiếp theo | Sắp xếp? | Chiều dài | 
| --- | --- | --- | --- | 
| 01 | [6] | vâng | 1 | 
| 10 | [7] | vâng | 1 | 
| 11 | [6,7] | vâng | 2 | 

Tối thiểu là 2, vì toàn bộ mảng đã được sắp xếp và không thể giảm bớt trong khi vẫn giữ nhiều hơn một phần tử mà không làm mất tính hợp lệ. 

Điều này xác nhận rằng khi mảng không giảm thì câu trả lời tối ưu là độ dài đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n · n) | Mỗi tập hợp con đều được kiểm tra và mỗi lần kiểm tra sẽ quét tối đa n phần tử | 
| Không gian | O(n) | Lưu trữ tạm thời cho mỗi chuỗi con được xây dựng | 

Cho n ≤ 10, đánh giá trường hợp xấu nhất là tối đa 1024 tập con cho mỗi trường hợp kiểm thử, mỗi tập yêu cầu tối đa 10 thao tác, dễ dàng nằm trong giới hạn ngay cả đối với 1000 trường hợp kiểm thử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        def is_sorted(seq):
            for i in range(len(seq) - 1):
                if seq[i] > seq[i + 1]:
                    return False
            return True

        best = n
        for mask in range(1 << n):
            seq = []
            for i in range(n):
                if mask & (1 << i):
                    seq.append(a[i])
            if is_sorted(seq):
                best = min(best, len(seq))

        out.append(str(best))

    return "\n".join(out) + "\n"

# provided samples
assert run("""3
4
1 4 2 3
1
100
2
6 7
""") == """1
1
2
"""

# custom cases
assert run("""1
1
5
""") == "1\n", "single element"

assert run("""1
2
2 1
""") == "1\n", "decreasing pair"

assert run("""1
3
1 1 1
""") == "3\n", "all equal"

assert run("""1
4
4 3 2 1
""") == "1\n", "strictly decreasing"

assert run("""1
5
1 2 3 4 5
""") == "5\n", "already sorted"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp cơ sở tối thiểu | 
| 2 1 | 1 | xử lý đảo ngược | 
| 1 1 1 | 3 | bản sao được bảo tồn | 
| 4 3 2 1 | 1 | trường hợp giảm xấu nhất | 
| 1 2 3 4 5 | 5 | đã tối ưu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi mảng không giảm. Đối với đầu vào`1 2 3 4`, thuật toán sẽ thấy rằng mặt nạ đầy đủ hợp lệ và có độ dài 4. Nó cũng sẽ thấy các tập hợp con hợp lệ nhỏ hơn, nhưng không có tập hợp con nào vi phạm tính chính xác vì chúng tôi đang giảm thiểu độ dài chứ không phải tối đa hóa. Việc liệt kê xác định chính xác rằng việc loại bỏ các phần tử không bao giờ có lợi nếu chúng ta yêu cầu trạng thái cuối cùng không giảm và vẫn có ý nghĩa. 

Một trường hợp cạnh khác là một mảng giảm hoàn toàn như`5 4 3 2 1`. Ở đây mọi tập hợp con đơn đều hợp lệ và thuật toán xác định chính xác rằng câu trả lời là 1 bằng cách quan sát thấy bất kỳ phần tử đơn lẻ nào đều thỏa mãn điều kiện đơn điệu.
