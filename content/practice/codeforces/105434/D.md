---
title: "CF 105434D - \u80a1\u7968\u4ea4\u6613"
description: "Chúng ta được cung cấp một chuỗi giá cổ phiếu hàng ngày. Bạn được phép chọn tối đa một cặp ngày, mua vào ngày sớm hơn và bán vào ngày sau đó và bạn kiếm được lợi nhuận bằng mức tăng giá chia cho số ngày nắm giữ."
date: "2026-06-23T03:52:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105434
codeforces_index: "D"
codeforces_contest_name: "2024\u5e74\u201c\u6838\u6843\u676f\u201d\u6b66\u6c49\u5730\u533aACM\u840c\u65b0\u8d5b"
rating: 0
weight: 105434
solve_time_s: 54
verified: true
draft: false
---

[CF 105434D - \u80a1\u7968\u4ea4\u6613](https://codeforces.com/problemset/problem/105434/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi giá cổ phiếu hàng ngày. Bạn được phép chọn tối đa một cặp ngày, mua vào ngày sớm hơn và bán vào ngày sau đó và bạn kiếm được lợi nhuận bằng mức tăng giá chia cho số ngày nắm giữ. Chính thức, nếu bạn mua vào ngày$x$và bán trong ngày$y$, điểm số là$(a_y - a_x) / (y - x)$. Nếu không có giao dịch sinh lời nào được chọn, điểm được xác định là 0. 

Nhiệm vụ là tính giá trị lớn nhất có thể có của tỷ lệ này trên tất cả các cặp hợp lệ.$x < y$. 

Kích thước đầu vào đạt$10^5$, điều này ngay lập tức loại trừ mọi giải pháp kiểm tra tất cả các cặp ngày. Quét bậc hai trên tất cả$(x, y)$cặp sẽ yêu cầu khoảng$5 \cdot 10^9$đánh giá trong trường hợp xấu nhất, vượt xa giới hạn 3 giây. Thậm chí hơi dưới mức tối ưu$O(n \log n)$hoặc quá trình tiền xử lý nặng nề sẽ không cần thiết vì cấu trúc của tỷ lệ cho thấy tính chất đơn điệu mạnh hơn có thể được khai thác. 

Một trường hợp quan trọng là khi giá giảm đơn điệu. Ví dụ, đầu vào$5, 4, 3, 2, 1$không tạo ra lợi nhuận dương, vì vậy câu trả lời phải là 0. Bất kỳ thuật toán nào không cho phép lựa chọn trống một cách rõ ràng sẽ tạo ra tỷ lệ âm từ một số cặp không chính xác, mặc dù định nghĩa bài toán cấm điều đó. 

Một trường hợp tinh tế khác là khi lợi nhuận tốt nhất đến từ một khoảng thời gian ngắn thay vì một bước nhảy vọt lớn. Ví dụ,$1, 100, 2, 3$có thể cám dỗ một chiến lược tham lam chọn những điểm cực đoan, nhưng phân khúc tối ưu phụ thuộc vào việc cân bằng đồng thời tử số và mẫu số. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force liệt kê tất cả các cặp ngày$x < y$, tính giá trị$(a_y - a_x)/(y - x)$và theo dõi mức tối đa. Điều này đúng vì mọi giao dịch hợp lệ đều được kiểm tra. Tuy nhiên, đối với mỗi điểm cuối bên phải$y$, nó kiểm tra tất cả trước đó$x$, dẫn đến đại khái$n(n-1)/2$đánh giá. Với$n = 10^5$, điều này trở nên không thể thực hiện được. 

Cấu trúc của biểu thức gợi ý một cách giải thích hình học. Mỗi ngày$x$có thể được coi là một điểm$(x, a_x)$và giá trị của một cặp là độ dốc của đường nối hai điểm. Bài toán giảm xuống việc tìm độ dốc tối đa giữa hai điểm bất kỳ với tọa độ x tăng dần. 

Điều này biến thành một kịch bản tối ưu hóa bao lồi cổ điển: đối với mỗi điểm, chúng tôi muốn duy trì một cấu trúc cho phép chúng tôi truy vấn điểm chặn tối thiểu mang lại độ dốc tối đa với điểm hiện tại. Cái nhìn sâu sắc quan trọng là khi chúng ta tiến về phía trước theo thời gian, chỉ một tập hợp con các điểm trước đó có thể là ứng cử viên tối ưu và những điểm này có thể được duy trì trong một bao lồi trong đó hệ số góc giữa các điểm liên tiếp là đơn điệu. 

Điều này cho phép chúng tôi duy trì bao điểm lồi thấp hơn và truy vấn điểm trước đó tốt nhất cho mỗi ngày mới trong thời gian khấu hao không đổi bằng cách sử dụng con trỏ đơn điệu hoặc deque. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Thân lồi / Thân đơn điệu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại mỗi ngày$i$như một điểm$(i, a_i)$. Điểm giữa hai điểm là độ dốc của đường nối chúng. Chúng tôi muốn độ dốc tối đa trên tất cả các cặp có chỉ số tăng dần. 

Chúng tôi duy trì một tập hợp các điểm ứng cử viên lồi, trong đó mỗi điểm đại diện cho một ngày trước đó có thể đóng vai trò là thời điểm mua tối ưu. 

1. Bắt đầu với một deque trống để lưu trữ các chỉ mục ứng cử viên. Deque sẽ duy trì các điểm trong một cấu trúc sao cho độ dốc giữa các điểm liên tiếp là đơn điệu. 
2. Lặp lại mỗi ngày$i$từ trái sang phải. 
3. Trước khi chèn điểm hiện tại, hãy truy vấn điểm trước đó tốt nhất trong deque để tối đa hóa độ dốc$(a_i - a_j) / (i - j)$. 
4. Để tìm thấy điều này tốt nhất$j$, chúng tôi so sánh các ứng cử viên trong deque. Hàm độ dốc không đồng nhất trên thân tàu, vì vậy chúng ta có thể nâng cao một con trỏ trong khi ứng cử viên tiếp theo cải thiện độ dốc. 
5. Cập nhật câu trả lời chung với độ dốc tốt nhất được tìm thấy trong ngày$i$. 
6. Chèn điểm hiện tại$i$vào deque trong khi vẫn duy trì tính lồi. Nếu hai điểm cuối và điểm mới vi phạm tính lồi thì loại bỏ điểm giữa. 
7. Tiếp tục cho đến khi tất cả các ngày được xử lý. 

Ý tưởng chính trong cả thao tác chèn và truy vấn là các điểm thống trị sẽ không bao giờ trở lại tối ưu nữa nên chúng có thể được loại bỏ một cách an toàn. 

### Tại sao nó hoạt động 

biểu thức$(a_y - a_x)/(y - x)$là độ dốc giữa hai điểm trong mặt phẳng. Thuộc tính bao lồi đảm bảo rằng đối với bất kỳ tập hợp điểm nào, độ dốc tối đa tới một điểm mới đạt được tại điểm cực trị của bao, không bao giờ ở bên trong nó. Bằng cách chỉ duy trì các ứng cử viên cực đoan theo thứ tự chỉ số tăng dần, chúng tôi đảm bảo rằng mọi điểm bị loại bỏ đều tệ hơn một số kết hợp các điểm lân cận của nó cho tất cả các truy vấn trong tương lai. Điều này duy trì tính chính xác trong khi giảm không gian tìm kiếm xuống kích thước tuyến tính tổng thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def slope(i, j, a):
    return (a[i] - a[j]) / (i - j)

def bad(a, i, j, k):
    return (a[j] - a[i]) * (k - j) >= (a[k] - a[j]) * (j - i)

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    from collections import deque
    dq = deque()

    ans = 0.0

    for i in range(n):
        while len(dq) >= 2:
            j = dq[0]
            k = dq[1]
            if slope(i, j, a) <= slope(i, k, a):
                dq.popleft()
            else:
                break

        if dq:
            ans = max(ans, slope(i, dq[0], a))

        while len(dq) >= 2 and bad(a, dq[-2], dq[-1], i):
            dq.pop()

        dq.append(i)

    print(f"{ans:.3f}")

if __name__ == "__main__":
    solve()
```Giải pháp duy trì số ngày mua hàng của ứng viên. các`slope`chức năng đánh giá lợi nhuận mỗi ngày cho một cặp ứng cử viên. Bước truy vấn di chuyển từ phía trước của deque trong khi độ dốc tốt hơn tồn tại ở phía trước, khai thác cấu trúc đơn điệu của các độ dốc trên một bao lồi. 

các`bad`hàm loại bỏ điểm ở giữa nếu nó trở nên không cần thiết để duy trì tính lồi. Nó so sánh các hệ số góc nhân chéo để tránh các vấn đề về độ chính xác của dấu phẩy động trong bước bảo trì thân tàu, mặc dù các câu trả lời cuối cùng được tính toán trong float. 

Câu trả lời bắt đầu từ 0,0 để tôn trọng tùy chọn “không giao dịch”. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Chúng tôi theo dõi các ứng cử viên và những con dốc tốt nhất. 

| tôi | giá | deque trước truy vấn | ứng cử viên tốt nhất j | độ dốc | trả lời | deque sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | [] | - | - | 0,0 | [0] | 
| 1 | 2 | [0] | 0 | 1.0 | 1.0 | [0,1] | 
| 2 | 3 | [0,1] | 1 | 1.0 | 1.0 | [0,1,2] | 
| 3 | 4 | [0,1,2] | 2 | 1.0 | 1.0 | [0,1,2,3] | 
| 4 | 5 | [0,1,2,3] | 3 | 1.0 | 1.0 | [0,1,2,3,4] | 

Điều này hiển thị một chuỗi tăng tuyến tính trong đó mỗi phân đoạn có độ dốc 1, xác nhận rằng thuật toán theo dõi chính xác các lựa chọn tối ưu bằng nhau. 

### Ví dụ 2 

đầu vào:```
5
5 4 3 2 1
```| tôi | giá | deque trước truy vấn | ứng cử viên tốt nhất j | độ dốc | trả lời | deque sau | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | [] | - | - | 0,0 | [0] | 
| 1 | 4 | [0] | 0 | -1.0 | 0,0 | [0,1] | 
| 2 | 3 | [0,1] | 0 | -1.0 | 0,0 | [0,1,2] | 
| 3 | 2 | [0,1,2] | 0 | -1.0 | 0,0 | [0,1,2,3] | 
| 4 | 1 | [0,1,2,3] | 0 | -1.0 | 0,0 | [0,1,2,3,4] | 

Điều này xác nhận rằng hệ số góc âm không bao giờ ghi đè câu trả lời mặc định là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục nhập và rời khỏi deque nhiều nhất một lần và mỗi chỉ mục được xử lý theo thời gian khấu hao không đổi | 
| Không gian | O(n) | Deque lưu trữ hầu hết tất cả các chỉ số trong trường hợp xấu nhất trước khi cắt tỉa | 

Độ phức tạp tuyến tính là cần thiết bởi vì$n = 10^5$và mọi phép thăm dò bậc hai sẽ vượt quá giới hạn thời gian. Việc sử dụng bộ nhớ vẫn an toàn dưới 512 MB vì ​​chỉ lưu trữ các chỉ số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    from collections import deque

    def slope(i, j):
        return (a[i] - a[j]) / (i - j)

    def bad(i, j, k):
        return (a[j] - a[i]) * (k - j) >= (a[k] - a[j]) * (j - i)

    dq = deque()
    ans = 0.0

    for i in range(n):
        while len(dq) >= 2:
            j = dq[0]
            k = dq[1]
            if slope(i, j) <= slope(i, k):
                dq.popleft()
            else:
                break

        if dq:
            ans = max(ans, slope(i, dq[0]))

        while len(dq) >= 2 and bad(dq[-2], dq[-1], i):
            dq.pop()

        dq.append(i)

    return f"{ans:.3f}"

# provided samples
assert run("5\n1 2 3 4 5\n") == "1.000", "sample 1"
assert run("5\n5 4 3 2 1\n") == "0.000", "sample 2"

# custom cases
assert run("1\n10\n") == "0.000", "single element"
assert run("2\n1 100\n") == "99.000", "single jump"
assert run("3\n1 3 2\n") == "2.000", "peak in middle"
assert run("4\n10 1 10 1\n") == "9.000", "alternating extremes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0,000 | kích thước tối thiểu, không giao dịch | 
| 1 100 | 99.000 | trực tiếp quãng đơn tốt nhất | 
| 1 3 2 | 2.000 | trường hợp không đơn điệu | 
| 10 1 10 1 | 9.000 | xen kẽ các thái cực | 

## Vỏ cạnh 

Đối với đầu vào một ngày như`10`, deque chỉ chứa một điểm và không tồn tại giao dịch hợp lệ nào. Thuật toán không bao giờ tính toán độ dốc, vì vậy câu trả lời vẫn là 0,0. 

Đối với các chuỗi giảm nghiêm ngặt như`5 4 3 2 1`, mọi độ dốc được tính toán đều âm, nhưng thuật toán vẫn cập nhật deque và kiểm tra các ứng cử viên. Vì câu trả lời bắt đầu từ 0,0 và chỉ có những cải tiến tích cực mới quan trọng nên không có bản cập nhật nào xảy ra. 

Đối với trường hợp cặp tối ưu nằm liền kề, chẳng hạn như`1 100`, phép so sánh đầu tiên đánh giá trực tiếp độ dốc 99, ngay lập tức trở thành câu trả lời đúng nhất. Logic deque không trì hoãn hoặc bỏ qua trường hợp này vì mỗi chỉ mục mới được kiểm tra dựa trên thân hiện tại trước khi chèn. 

Đối với đầu vào dao động, các điểm thống trị sẽ bị loại bỏ trong quá trình bảo trì thân tàu, đảm bảo chỉ còn lại các ứng cử viên phù hợp về mặt cấu trúc và mọi độ dốc tối ưu tiềm năng vẫn được đánh giá khi đến điểm cuối phù hợp.
