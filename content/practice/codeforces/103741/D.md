---
title: "CF 103741D - Sự khác biệt"
description: "Chúng ta được cung cấp một mảng có độ dài $n$. Đối với mỗi mảng con liền kề $[l, r]$, chúng ta xác định một giá trị bằng chênh lệch giữa phần tử lớn nhất và phần tử nhỏ nhất bên trong mảng con đó."
date: "2026-07-02T09:04:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103741
codeforces_index: "D"
codeforces_contest_name: "HUSTPC 2022"
rating: 0
weight: 103741
solve_time_s: 50
verified: true
draft: false
---

[CF 103741D - Sự khác biệt](https://codeforces.com/problemset/problem/103741/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng có độ dài$n$. Đối với mọi mảng con liền kề$[l, r]$, chúng ta xác định một giá trị bằng hiệu giữa phần tử lớn nhất và phần tử nhỏ nhất bên trong mảng con đó. Nhiệm vụ không phải là tính toán rõ ràng tất cả các giá trị này mà là xác định giá trị nào được xếp hạng$k$-th khi tất cả sự khác biệt của mảng con được sắp xếp theo thứ tự giảm dần. 

Vì vậy, về mặt khái niệm, mỗi khoảng đều đóng góp một con số: các giá trị của nó “phân tán” như thế nào. Một mảng con hằng số đóng góp bằng 0, trong khi một mảng con chứa cả phần tử rất lớn và phần tử rất nhỏ đóng góp một giá trị lớn. 

Các ràng buộc rất lớn:$n \le 5 \cdot 10^5$, ngụ ý$O(n^2)$mảng con trong trường hợp xấu nhất, xung quanh$10^{11}$. Bất kỳ giải pháp nào liệt kê các khoảng đều là không thể ngay lập tức. Thậm chí$O(n \log n)$tính toán theo khoảng thời gian được loại trừ. 

Khó khăn chính là chúng tôi được yêu cầu thống kê thứ tự trên tất cả các phạm vi mảng con chứ không phải một giá trị tổng hợp duy nhất. Điều đó thường báo hiệu rằng chúng ta cần đếm xem có bao nhiêu mảng con thỏa mãn điều kiện có dạng “hiệu ít nhất là X”, sau đó tìm kiếm nhị phân trên X. 

Một cạm bẫy tinh tế là sự hiểu lầm về sự đơn điệu. Nếu chúng ta ấn định một ngưỡng$X$, điều kiện “max trừ min ≥ X” hoạt động đơn điệu trên các mảng con: nếu một mảng con thỏa mãn nó thì bất kỳ cửa sổ lớn hơn nào chứa nó cũng thỏa mãn nó. Cấu trúc này cho phép đếm bằng hai con trỏ và hàng đợi đơn điệu. 

Các trường hợp cần lưu ý bao gồm các mảng có tất cả các phần tử bằng nhau, trong đó mọi mảng con đều có giá trị bằng 0 và các mảng có các giá trị cực kỳ xen kẽ, trong đó hầu hết các mảng con đều có sự khác biệt lớn. Một mô phỏng xếp hạng ngây thơ sẽ thất bại ngay cả đối với$n = 10^4$bởi vì nó sẽ cần khoảng$5 \cdot 10^7$khoảng thời gian. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi mảng con, tính giá trị tối thiểu và tối đa của nó và lưu trữ kết quả. Điều này đúng nhưng về cơ bản là bậc hai về số khoảng. Với$n = 5 \cdot 10^5$, điều này có nghĩa là khoảng$1.25 \cdot 10^{11}$mảng con, vượt xa mọi tính toán khả thi. 

Quan sát quan trọng là đảo ngược vấn đề. Thay vì cố gắng liệt kê tất cả những khác biệt, chúng tôi yêu cầu: với một giá trị cố định$X$, có bao nhiêu mảng con thỏa mãn$$\max(a[l..r]) - \min(a[l..r]) \ge X?$$Nếu chúng ta có thể tính toán số lượng này một cách nhanh chóng, chúng ta có thể tìm kiếm nhị phân trên$X$. Câu trả lời chúng tôi muốn là lớn nhất$X$ít nhất như vậy$k$mảng con có sự khác biệt ít nhất$X$. Điều này hoạt động vì số lượng mảng con có sự khác biệt ít nhất$X$giảm đơn điệu như$X$tăng lên. 

Để tính toán số lượng một cách hiệu quả cho một giá trị cố định$X$, chúng tôi sử dụng cửa sổ trượt hai con trỏ với các deque đơn điệu duy trì mức tối đa và tối thiểu hiện tại. Chúng tôi mở rộng điểm cuối bên phải và duy trì điểm cuối bên trái tốt nhất sao cho cửa sổ vẫn hợp lệ theo ràng buộc “max − min < X”. Mỗi khi cửa sổ phá vỡ ràng buộc, chúng ta di chuyển con trỏ trái về phía trước. 

Điều này cung cấp một lần quét tuyến tính cho mỗi lần kiểm tra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + hai con trỏ |$O(n \log V)$|$O(n)$| Đã chấp nhận | 

Đây$V$là phạm vi của những khác biệt có thể có, được giới hạn bởi khoảng$2 \cdot 10^9$. 

## Hướng dẫn thuật toán 

1. Xác định hàm$\text{count}(X)$trả về có bao nhiêu mảng con thỏa mãn$\max - \min \ge X$. Chúng ta sẽ tính toán điều này thông qua việc đếm phần bù thay vì tính trực tiếp. 
2. Đối với cố định$X$, thay vào đó hãy đếm các mảng con trong đó$\max - \min < X$. Điều này dễ dàng hơn để duy trì dần dần. 
3. Duy trì cửa sổ trượt$[l, r]$và hai deque đơn điệu: một giảm cho giá trị tối đa và một tăng cho giá trị tối thiểu. 
4. Mở rộng$r$từ trái sang phải, chèn$a[r]$vào cả hai deques trong khi vẫn giữ được tính đơn điệu. Điều này đảm bảo mặt trước của mỗi deque luôn thể hiện mức tối đa và tối thiểu của cửa sổ hiện tại. 
5. Trong khi cửa sổ hiện tại vi phạm$\max - \min < X$, di chuyển$l$chuyển tiếp và loại bỏ các phần tử lỗi thời khỏi deque. Bước thu nhỏ này đảm bảo cửa sổ luôn thỏa mãn ràng buộc. 
6. Đối với mỗi$r$, tất cả các mảng con kết thúc tại$r$và bắt đầu từ bất cứ đâu trong$[l, r]$hợp lệ theo ràng buộc, vì vậy chúng tôi thêm$r - l + 1$đến số lượng mảng con hợp lệ. 
7. Tổng số mảng con là$n(n+1)/2$. Trừ những giá trị hợp lệ sẽ cho số lượng mảng con có chênh lệch ít nhất$X$. 
8. Tìm kiếm nhị phân$X$trong phạm vi khác biệt có thể có. Với mỗi phần giữa, hãy tính xem có bao nhiêu mảng con có sự chênh lệch ít nhất ở phần giữa và so sánh với$k$để điều chỉnh tìm kiếm. 

Ý tưởng chính là thay vì xếp hạng trực tiếp các giá trị mảng con, chúng ta chuyển bài toán thành bài toán đếm ngưỡng, bài toán này đơn điệu và có thể kiểm tra hiệu quả. 

### Tại sao nó hoạt động 

Đối với bất kỳ cố định$X$, tập hợp các mảng con thỏa mãn$\max - \min \ge X$là đơn điệu theo nghĩa cần thiết cho tìm kiếm nhị phân: tăng$X$chỉ có thể xóa các mảng con hợp lệ, không bao giờ thêm mảng con mới. Cửa sổ trượt đếm chính xác tập phần bù vì trong mỗi điểm cuối bên phải, giá trị thực thi ranh giới bên trái tối thiểu được xác định duy nhất bởi ràng buộc đơn điệu ở mức tối đa và tối thiểu. Điều này đảm bảo mỗi mảng con được tính chính xác một lần trong chế độ hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def count_less_than_x(a, x):
    n = len(a)
    maxdq = deque()
    mindq = deque()
    l = 0
    res = 0

    for r in range(n):
        while maxdq and a[maxdq[-1]] <= a[r]:
            maxdq.pop()
        maxdq.append(r)

        while mindq and a[mindq[-1]] >= a[r]:
            mindq.pop()
        mindq.append(r)

        while maxdq and mindq and a[maxdq[0]] - a[mindq[0]] >= x:
            if maxdq[0] == l:
                maxdq.popleft()
            if mindq[0] == l:
                mindq.popleft()
            l += 1

        res += r - l + 1

    return res

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    total = n * (n + 1) // 2

    lo, hi = 0, max(a) - min(a)

    ans = 0
    while lo <= hi:
        mid = (lo + hi) // 2
        # count subarrays with diff >= mid
        less = count_less_than_x(a, mid)
        ge = total - less

        if ge >= k:
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã được cấu trúc xung quanh một trình trợ giúp đếm các mảng con có chênh lệch nhỏ hơn ngưỡng. Cửa sổ trượt duy trì hai deque đơn điệu, một theo dõi các ứng cử viên tối đa và một theo dõi các ứng cử viên tối thiểu. Điểm chính xác quan trọng là bất biến cửa sổ luôn thực thi một điều kiện bất đẳng thức nghiêm ngặt, do đó phần bù tương ứng chính xác với vị từ ngưỡng mong muốn. 

Tìm kiếm nhị phân được thực hiện trên các giá trị câu trả lời có thể có và mỗi lần kiểm tra sẽ chuyển đổi điều kiện thành một bài toán đếm. Câu trả lời cuối cùng là ngưỡng lớn nhất vẫn còn ít nhất$k$mảng con hợp lệ ở trên nó. 

Một lỗi triển khai phổ biến là trộn lẫn hướng bất đẳng thức bên trong điều kiện cửa sổ trượt. Việc kiểm tra phải nhất quán với định nghĩa “nhỏ hơn x”, nếu không tính đơn điệu của tìm kiếm nhị phân sẽ bị phá vỡ và kết quả sẽ không ổn định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 1 2
```Tất cả các mảng con: 

| tôi | r | mảng con | tối đa-min | 
| --- | --- | --- | --- | 
| 1 | 1 | [3] | 0 | 
| 2 | 2 | [1] | 0 | 
| 3 | 3 | [2] | 0 | 
| 1 | 2 | [3,1] | 2 | 
| 2 | 3 | [1,2] | 1 | 
| 1 | 3 | [3,1,2] | 2 | 

Sắp xếp chênh lệch giảm dần:```
2, 2, 1, 0, 0, 0
```Số lớn thứ 2 là 2. 

Điều này xác nhận thuật toán phải ưu tiên các mảng con có khoảng cách lớn hơn ngay cả khi chúng ít hơn. 

### Ví dụ 2 

đầu vào:```
4 1
1 1 1 1
```Mỗi mảng con có chênh lệch 0, vì vậy tất cả các giá trị đều giống hệt nhau. Giá trị lớn thứ k luôn bằng 0 bất kể k. 

Điều này cho thấy tìm kiếm nhị phân phải xử lý các phạm vi suy biến một cách chính xác và không cho rằng tồn tại sự khác biệt tích cực. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$| Mỗi bước tìm kiếm nhị phân chạy quét hai con trỏ tuyến tính; phạm vi giá trị được giới hạn bởi max-min | 
| Không gian |$O(n)$| Deques lưu trữ các chỉ số của cửa sổ hiện tại | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì$n = 5 \cdot 10^5$và cần khoảng 31-32 lần lặp tìm kiếm nhị phân. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def count_less_than_x(a, x):
        n = len(a)
        maxdq = deque()
        mindq = deque()
        l = 0
        res = 0

        for r in range(n):
            while maxdq and a[maxdq[-1]] <= a[r]:
                maxdq.pop()
            maxdq.append(r)

            while mindq and a[mindq[-1]] >= a[r]:
                mindq.pop()
            mindq.append(r)

            while maxdq and mindq and a[maxdq[0]] - a[mindq[0]] >= x:
                if maxdq[0] == l:
                    maxdq.popleft()
                if mindq[0] == l:
                    mindq.popleft()
                l += 1

            res += r - l + 1

        return res

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    total = n * (n + 1) // 2

    lo, hi = 0, max(a) - min(a)
    ans = 0

    while lo <= hi:
        mid = (lo + hi) // 2
        less = count_less_than_x(a, mid)
        ge = total - less

        if ge >= k:
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    return str(ans)

# provided sample
assert run("3 2\n3 1 2\n") == "2"

# custom cases
assert run("4 1\n1 1 1 1\n") == "0"
assert run("2 1\n1 100\n") == "99"
assert run("5 3\n1 2 3 4 5\n") == "3"
assert run("3 3\n5 1 5\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 / 3 1 2 | 2 | tính đúng đắn của mẫu và logic xếp hạng | 
| 4 1 / tất cả đều bằng nhau | 0 | trường hợp thoái hóa | 
| 2 1/1 100 | 99 | khoảng cách tối đa khoảng đơn | 
| 5 3 / 1..5 | 3 | phân phối các phạm vi mảng con | 
| 3 3 / 5 1 5 | 4 | thái cực hỗn hợp và trùng lặp | 

## Vỏ cạnh 

Đối với một mảng hoàn toàn bằng nhau như`1 1 1 1`, mọi mảng con đều có chênh lệch bằng 0. Cửa sổ trượt không bao giờ vi phạm các ràng buộc đối với bất kỳ$X > 0$, do đó tìm kiếm nhị phân sẽ thu gọn chính xác thành câu trả lời 0. Hàm đếm trả về tất cả các mảng con cho$X = 0$, đặt chính xác số 0 làm giá trị ứng cử viên duy nhất. 

Đối với trường hợp cực đoan hai yếu tố như`1 100`, chỉ có ba mảng con và chênh lệch tối đa xuất hiện đúng một lần. Tìm kiếm nhị phân ngay lập tức tách 99 thành ngưỡng khác 0 duy nhất được hỗ trợ bởi một mảng con. 

Đối với các mẫu xen kẽ như`5 1 5`, sự khác biệt lớn xảy ra trong nhiều mảng con chồng chéo và hàng đợi đơn điệu đảm bảo việc đếm chính xác ngay cả khi giá trị tối đa và tối thiểu liên tục dịch chuyển tại các ranh giới.
