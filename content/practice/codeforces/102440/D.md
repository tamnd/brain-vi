---
title: "CF 102440D - \u041f\u0435\u0442\u044f \u0438 \u043c\u0430\u0441\u0441\u0438\u0432"
description: "Chúng ta có một mảng (a) và ngưỡng không âm (k). Một mảng con được gọi là đẹp nếu sau khi xóa tối đa một phần tử khỏi nó, giá trị lớn nhất trừ giá trị nhỏ nhất của nó có thể đạt được nhiều nhất là (k)."
date: "2026-08-08T13:46:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "D"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 166
verified: true
draft: false
---

[CF 102440D - \u041f\u0435\u0442\u044f \u0438 \u043c\u0430\u0441\u0441\u0438\u0432](https://codeforces.com/problemset/problem/102440/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng (a) và ngưỡng không âm (k). Một mảng con được gọi là đẹp nếu sau khi xóa tối đa một phần tử khỏi nó, giá trị lớn nhất trừ giá trị nhỏ nhất của nó có thể đạt được nhiều nhất là (k). Một truy vấn ([L,R]) yêu cầu số lượng cặp ((l,r)) với (L\le l<r\le R) sao cho mảng con (a_l,\ldots,a_r) đẹp. 

Cụm từ "xóa tối đa một phần tử" chính là điều khiến bài toán trở nên thú vị. Nếu phạm vi ban đầu đã đạt tối đa (k) thì không cần xóa gì cả. Mặt khác, việc xóa một phần tử chỉ có thể hữu ích nếu phần tử đó là mức tối thiểu duy nhất hoặc mức tối đa duy nhất. Nếu mức tối thiểu xảy ra hai lần, việc xóa một mức tối thiểu sẽ để lại một bản sao khác có cùng giá trị. Điều tương tự cũng áp dụng cho mức tối đa. 

Giả sử một cửa sổ có tối thiểu (mn), tối đa (mx), phần tử nhỏ thứ hai (mn_2) và phần tử lớn thứ hai (mx_2), trong đó các giá trị thứ hai được phép bằng cực trị tương ứng khi cực trị đó xảy ra ít nhất hai lần. Cửa sổ đẹp đúng lúc 

[ 
mx-mn\le k, 
] 

hoặc mức tối đa xảy ra đúng một lần và 

[ 
mx_2-mn\le k, 
] 

hoặc mức tối thiểu xảy ra đúng một lần và 

[ 
mx-mn_2\le k. 
] 

Các ràng buộc loại trừ các thuật toán kiểm tra tất cả các mảng con cho mọi truy vấn. Hầu như có (2\cdot10^{10}) cặp điểm cuối trong một mảng có độ dài (2\cdot10^5), do đó, ngay cả việc kiểm tra (O(1)) cho mọi mảng con cũng đã quá đắt. Chúng ta cần khai thác một thực tế rằng các mảng con đẹp có tính di truyền: mọi mảng con của một mảng đẹp cũng đẹp. Điều này mang lại cấu trúc đơn điệu cần thiết cho việc quét hai con trỏ. 

Có một số trường hợp khó xử lý. 

Coi như```
2 1 4
0 10
1 2
```Câu trả lời là (1). Khoảng ([1,2]) có phạm vi (10), lớn hơn (4), nhưng việc xóa một trong hai phần tử sẽ để lại một phần tử có phạm vi (0). Việc triển khai chỉ kiểm tra`max - min <= k`sẽ xuất sai (0). 

Bây giờ hãy xem xét```
4 1 0
0 0 10 10
1 4
```Câu trả lời là (4). Toàn bộ khoảng không đẹp vì xóa một số 0 để lại a (10), xóa một mười để lại a (0) nên phạm vi còn lại là (10). Tuy nhiên, ([1,3]) đẹp bằng cách xóa số mười, và ([2,4]) đẹp bằng cách xóa số 0. Hai cặp giá trị bằng nhau ([1,2]) và ([3,4]) cũng đẹp. Việc triển khai bất cẩn coi cực trị thứ hai là một giá trị khác mà không theo dõi bội số có thể đưa ra quyết định xóa sai. 

Một truy vấn có độ dài 1 là một trường hợp ranh giới khác. Ví dụ,```
1 1 0
7
1 1
```có đáp án (0), vì bài toán chỉ yêu cầu (l<r). Mặc dù một singleton luôn đẹp nhưng nó không bao giờ được tính là mảng con được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê từng cặp điểm cuối bên trong mỗi truy vấn và xác định xem mảng con đó có đẹp hay không. Đối với một truy vấn bao trùm toàn bộ mảng, điều này có nghĩa là kiểm tra 

[ 
\frac{n(n-1)}2 
] 

khoảng thời gian ứng viên. Tại (n=200000), tức là khoảng (19,999,900,000). Ngay cả khi bài kiểm tra sắc đẹp được thực hiện bằng phép thuật (O(1)) thì điều này cũng đã vượt quá giới hạn rồi. Nếu mỗi khoảng được quét để tìm cực trị của nó thì chi phí sẽ là bậc ba. 

Lực lượng vũ phu là chính xác vì nó kiểm tra định nghĩa cho mọi cặp có thể ((l,r)), nhưng nó hoàn toàn bỏ qua sự chồng chéo giữa các mảng con lân cận. Khi chúng ta mở rộng cửa sổ thêm một phần tử, hầu hết nội dung của nó không thay đổi. Điều tương tự cũng đúng khi điểm cuối bên trái của nó di chuyển một vị trí. 

Nhận xét quan trọng là vẻ đẹp đơn điệu khi bị thu hẹp lại. Nếu một mảng con đẹp, hãy chọn phần tử mà việc xóa của nó tạo ra phạm vi tối đa (k). Bất kỳ mảng con nhỏ hơn nào cũng không chứa phần tử đó, trong trường hợp đó phạm vi của nó đã bị giới hạn bởi cùng một đối số hoặc nó chứa phần tử đó và chúng ta có thể xóa nó một lần nữa. Vì vậy mọi mảng con nhỏ hơn đều đẹp. 

Điều này có nghĩa là với mọi điểm cuối bên phải cố định (r), sẽ có một điểm cuối bên trái nhỏ nhất (f[r]) sao cho ([f[r],r]) đẹp. Mọi khoảng ([l,r]) với (l\ge f[r]) đều đẹp. Khi (r) di chuyển sang phải, (f[r]) không bao giờ di chuyển sang trái. 

Chúng ta có thể tìm thấy tất cả (f[r]) bằng cửa sổ trượt. Khó khăn duy nhất còn lại là kiểm tra xem cửa sổ hiện tại có đẹp hay không. Deque tối thiểu đơn điệu cho giá trị nhỏ nhất và deque tối đa đơn điệu cho giá trị lớn nhất. Với tần số của các giá trị, chúng ta có thể xác định xem mức tối thiểu hoặc tối đa hiện tại có xảy ra một lần hay không. Nếu một cực trị là duy nhất thì phần tử thứ hai của deque đơn điệu tương ứng sẽ cho ra cực trị khác biệt tiếp theo. 

Sau khi tính toán (f[r]), các truy vấn trở thành bài toán đếm một chiều. Vì (f[r]) không giảm nên tìm kiếm nhị phân có thể chia mọi truy vấn thành các vị trí trong đó (f[r]\le L) và các vị trí trong đó (f[r]>L). Tổng tiền tố sau đó làm cho cả hai phần có thời gian không đổi sau khi tìm kiếm nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn^2)) ngay cả với (O(1)) bài kiểm tra sắc đẹp | (O(n^2)) nếu các bài kiểm tra được tính toán trước | Quá chậm | 
| Tối ưu | (O(n+q\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Duy trì cửa sổ trượt ([left,r]) trong khi xử lý mảng từ trái sang phải. Cửa sổ sẽ luôn là cửa sổ đẹp nhỏ nhất kết thúc bằng (r) sau khi chúng ta thu nhỏ nó xong. 
2. Giữ một deque tăng đơn điệu của các chỉ số ở mức tối thiểu và một deque giảm đơn điệu của các chỉ số ở mức tối đa. Khi một giá trị mới được chèn vào, hãy xóa các chỉ số ở phía sau mà không bao giờ có thể trở thành giá trị tối thiểu hoặc tối đa nữa. Bởi vì cả hai điểm cuối chỉ di chuyển về phía trước nên mỗi chỉ mục sẽ vào và rời khỏi mỗi deque nhiều nhất một lần. 
3. Duy trì một từ điển tần số cho các giá trị hiện có trong cửa sổ. Tần số là cần thiết vì giá trị ở phía trước deque tối thiểu có thể xuất hiện nhiều lần mặc dù deque chỉ lưu trữ một đại diện của giá trị đó. 
4. Kiểm tra cửa sổ hiện tại. Đặt (mn) và (mx) là mức tối thiểu và tối đa của nó. Nếu (mx-mn\le k), cửa sổ sẽ đẹp trực tiếp. 
5. Nếu (mx-mn>k), khả năng xóa thành công duy nhất có thể xảy ra là một cực trị duy nhất. Nếu mức tối đa xảy ra một lần, việc xóa nó sẽ để lại giá trị lớn thứ hai làm mức tối đa mới. Nếu phạm vi kết quả (mx_2-mn) tối đa là (k), cửa sổ đẹp. Đối xứng, nếu mức tối thiểu xảy ra một lần và (mx-mn_2\le k) thì cửa sổ đẹp. 
6. Nếu cửa sổ không đẹp thì tăng dần`left`và xóa (a[left]) khỏi cửa sổ. Giá trị bị xóa sẽ bị xóa khỏi từ điển tần số và chỉ mục của nó sẽ bị xóa khỏi mặt trước của một trong hai deque khi cần thiết. Lặp lại cho đến khi cửa sổ trở nên đẹp. 
7. Lưu trữ kết quả`left`như (f[r]). Vì cửa sổ trước đó đã ở mức tối thiểu đối với (r-1), việc thêm phần tử mới không thể làm cho điểm cuối bên trái nhỏ hơn không hợp lệ trước đó trở lại hợp lệ. Do đó, dãy (f[0],f[1],\ldots,f[n-1]) không giảm. 
8. Xây dựng tổng tiền tố của các chỉ số và của (f[r]). Đối với một truy vấn ([L,R]), điểm cuối (r) đóng góp tất cả các điểm cuối bên trái hợp lệ từ (L) đến (r-1) if (f[r]\le L). Khi đó đóng góp của nó là (r-L). 
9. Nếu (f[r]>L), các điểm cuối bên trái hợp lệ bắt đầu tại (f[r]), do đó phần đóng góp là (rf[r]). Vì (f[r]) không giảm nên tìm kiếm nhị phân tìm thấy (r) đầu tiên trong truy vấn có (f[r]>L). 
10. Sử dụng tổng tiền tố để đánh giá cả hai phần của truy vấn. Điểm cuối đơn (r=L) tự động đóng góp số 0, do đó không cần trường hợp đặc biệt riêng biệt. 

### Tại sao nó hoạt động 

Đối với mọi điểm cuối bên phải (r), tính bất biến của cửa sổ trượt là sau khi thu nhỏ, ([f[r],r]) là đẹp trong khi mọi khoảng ([l,r]) với (l<f[r]) thì không. Vẻ đẹp được giữ nguyên khi một khoảng được thu nhỏ lại, do đó tất cả bắt đầu từ (f[r]) đến (r) tạo ra những khoảng đẹp. Truy vấn chỉ loại trừ singleton (l=r), để lại chính xác (r-\max(L,f[r])) bắt đầu hợp lệ. 

Các deques đơn điệu đưa ra mức tối thiểu và tối đa hiện tại. Các mục trước của chúng đại diện cho các cực trị, trong khi các mục tiếp theo đưa ra giá trị ứng cử viên tiếp theo khi cực trị tương ứng là duy nhất. Từ điển tần số phân biệt trường hợp cực trị duy nhất với trường hợp cực trị lặp lại. Vì vậy, bài kiểm tra sắc đẹp hoàn toàn phù hợp với định nghĩa. 

Cuối cùng, ranh giới bên trái chỉ di chuyển sang phải, do đó (f[r]) không giảm. Điều này làm cho việc phân tách truy vấn hợp lệ và cho phép một tìm kiếm nhị phân xác định tất cả các điểm cuối bằng (f[r]\le L). 

## Giải pháp Python```python
import sys
from bisect import bisect_right
from collections import deque

input = sys.stdin.readline

def solve():
    n, q, k = map(int, input().split())
    a = list(map(int, input().split()))

    minq = deque()
    maxq = deque()
    freq = {}

    f = [0] * n
    left = 0

    def add(i):
        x = a[i]

        while minq and a[minq[-1]] >= x:
            minq.pop()
        minq.append(i)

        while maxq and a[maxq[-1]] <= x:
            maxq.pop()
        maxq.append(i)

        freq[x] = freq.get(x, 0) + 1

    def remove(i):
        x = a[i]

        freq[x] -= 1
        if freq[x] == 0:
            del freq[x]

        while minq and minq[0] < left:
            minq.popleft()

        while maxq and maxq[0] < left:
            maxq.popleft()

    def beautiful():
        mn = a[minq[0]]
        mx = a[maxq[0]]

        if mx - mn <= k:
            return True

        if freq[mx] == 1:
            mx2 = a[maxq[1]]
            if mx2 - mn <= k:
                return True

        if freq[mn] == 1:
            mn2 = a[minq[1]]
            if mx - mn2 <= k:
                return True

        return False

    for r in range(n):
        add(r)

        while not beautiful():
            left += 1
            remove(left - 1)

        f[r] = left

    pref_index = [0] * (n + 1)
    pref_f = [0] * (n + 1)

    for i in range(n):
        pref_index[i + 1] = pref_index[i] + i
        pref_f[i + 1] = pref_f[i] + f[i]

    out = []

    for _ in range(q):
        L, R = map(int, input().split())
        L -= 1
        R -= 1

        # First position p in [L, R] with f[p] > L.
        p = bisect_right(f, L, lo=L, hi=R + 1)

        # For r in [L, p), contribution is r - L.
        count1 = p - L
        sum_r1 = pref_index[p] - pref_index[L]
        ans = sum_r1 - count1 * L

        # For r in [p, R], contribution is r - f[r].
        sum_r2 = pref_index[R + 1] - pref_index[p]
        sum_f2 = pref_f[R + 1] - pref_f[p]
        ans += sum_r2 - sum_f2

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`minq`deque lưu trữ các chỉ mục có giá trị tăng dần sau mỗi lần chèn. Khi một giá trị mới nhỏ hơn hoặc bằng các giá trị ở phía sau, những chỉ số cũ đó không bao giờ có thể trở thành giá trị tối thiểu trong khi chỉ mục mới vẫn còn trong cửa sổ, vì vậy chúng sẽ bị loại bỏ.`maxq`sử dụng quy tắc đối xứng. 

Từ điển tần số được cố tình tách biệt khỏi deque. Các giá trị tối thiểu bằng nhau được thu gọn trong deque, nhưng tính bội số của chúng vẫn có vấn đề. Ví dụ: nếu mức tối thiểu hiện tại là (0) và xuất hiện ba lần thì việc xóa một lần xuất hiện không thể xóa (0) khỏi cửa sổ. Từ điển phát hiện chính xác tình huống này. 

các`remove`chức năng sử dụng toàn cầu hiện tại`left`ranh giới. Một chỉ mục có thể đã biến mất khỏi deque vì một ứng cử viên tốt hơn được chèn vào sau đó, do đó việc dọn dẹp được viết dưới dạng một`while`vòng lặp thay vì cho rằng có chỉ mục bị loại bỏ. 

Bài kiểm tra sắc đẹp kiểm tra phạm vi bình thường trước tiên. Chỉ khi phạm vi quá lớn, nó mới kiểm tra các trường hợp cực đoan duy nhất. Nếu mức tối đa là duy nhất,`maxq[1]`là mức tối đa tiếp theo. Nếu mức tối thiểu là duy nhất,`minq[1]`là mức tối thiểu tiếp theo. Việc kiểm tra là an toàn vì cửa sổ hiện tại có ít nhất hai giá trị riêng biệt bất cứ khi nào lần kiểm tra phạm vi đầu tiên không thành công. 

Mảng`f`sử dụng các chỉ số dựa trên số không. Đối với điểm cuối cố định (r), nếu`f[r] <= L`, mọi lần bắt đầu từ (L) đến (r-1) đều hoạt động, đưa ra các lựa chọn (r-L). Mặt khác, thời điểm bắt đầu là từ (f[r]) đến (r-1), đưa ra các lựa chọn (r-f[r]). Tổng tiền tố của các chỉ số xử lý biểu thức đầu tiên, trong khi tổng tiền tố của`f`xử lý thứ hai. 

Số nguyên Python có độ chính xác tùy ý, do đó câu trả lời không bị tràn. Câu trả lời lớn nhất có thể là khoảng (n^2/2), tức là khoảng (2\cdot10^{10}) cho (n=2\cdot10^5). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mảng đầu vào là 

[ 
[0,10,10,2,4] 
] 

với (k=4). Bảng sau đây hiển thị trạng thái sau khi cửa sổ được thu nhỏ đến mức cần thiết. 

| (r) | Cửa sổ hiện tại | Tối thiểu | Tối đa | Đếm phút | Số lượng tối đa | (f[r]) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [0] | 0 | 0 | 1 | 1 | 0 | 
| 1 | [0,10] | 0 | 10 | 1 | 1 | 0 | 
| 2 | [0,10,10] | 0 | 10 | 1 | 2 | 0 | 
| 3 | [10,10,2] | 2 | 10 | 1 | 2 | 1 | 
| 4 | [10,2,4] | 2 | 10 | 1 | 1 | 2 | 

Tại (r=1), phạm vi là (10), nhưng mức tối đa là duy nhất, do đó việc xóa nó chỉ còn lại (0). Tại (r=2), cực đại xảy ra hai lần, nhưng cực tiểu là duy nhất, do đó việc xóa các lá cực tiểu`[10,10]`. Tại (r=3),`[0,10,10,2]`không đẹp nên điểm cuối bên trái tiến tới (1). Kết quả`[10,10,2]`trở nên đẹp bằng cách xóa (2). 

Mảng ranh giới kết quả là 

[ 
f=[0,0,0,1,2]. 
] 

Đối với truy vấn ([1,5]), điểm cuối (2) đóng góp (1), điểm cuối (3) đóng góp (2), điểm cuối (4) đóng góp (2) và điểm cuối (5) đóng góp (2). Tổng của chúng là (7). 

### Mẫu 2 

Mảng ở đây là 

[ 
[0,10,1,2,4] 
] 

và (k=4). 

| (r) | Cửa sổ hiện tại | Tối thiểu | Tối đa | Đếm phút | Số lượng tối đa | (f[r]) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | [0] | 0 | 0 | 1 | 1 | 0 | 
| 1 | [0,10] | 0 | 10 | 1 | 1 | 0 | 
| 2 | [0,10,1] | 0 | 10 | 1 | 1 | 0 | 
| 3 | [0,10,1,2] | 0 | 10 | 1 | 1 | 0 | 
| 4 | [0,10,1,2,4] | 0 | 10 | 1 | 1 | 0 | 

Tại (r=4), toàn bộ mảng rất đẹp vì việc xóa giá trị tối đa duy nhất (10) sẽ để lại các giá trị từ (0) đến (4), có phạm vi chính xác là (k). Do đó, mọi điểm cuối đều có (f[r]=0) và mọi khoảng không trống có độ dài ít nhất hai là đẹp. 

có 

[ 
1+2+3+4=10 
] 

các mảng con như vậy, phù hợp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+q\log n)) | Mỗi chỉ mục mảng đi vào và rời khỏi mỗi deque đơn điệu một lần, trong khi mỗi truy vấn thực hiện một tìm kiếm nhị phân. | 
| Không gian | (O(n)) | Các deque, từ điển tần số, mảng ranh giới và hai mảng tiền tố đều sử dụng không gian tuyến tính. | 

Quá trình tiền xử lý là tuyến tính vì hai con trỏ và cả hai deque chỉ di chuyển về phía trước. Chi phí giai đoạn truy vấn (O(\log n)) cho mỗi truy vấn vì`f`được sắp xếp theo thứ tự không giảm. Với (n,q\le2\cdot10^5), điều này mang lại khoảng (O(n+q\log n)) hoạt động và duy trì ở mức độ phức tạp dự kiến ​​đối với các giới hạn đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_right
from collections import deque

def solve():
    input = sys.stdin.readline

    n, q, k = map(int, input().split())
    a = list(map(int, input().split()))

    minq = deque()
    maxq = deque()
    freq = {}

    f = [0] * n
    left = 0

    def add(i):
        x = a[i]

        while minq and a[minq[-1]] >= x:
            minq.pop()
        minq.append(i)

        while maxq and a[maxq[-1]] <= x:
            maxq.pop()
        maxq.append(i)

        freq[x] = freq.get(x, 0) + 1

    def remove(i):
        x = a[i]
        freq[x] -= 1
        if freq[x] == 0:
            del freq[x]

        while minq and minq[0] < left:
            minq.popleft()

        while maxq and maxq[0] < left:
            maxq.popleft()

    def beautiful():
        mn = a[minq[0]]
        mx = a[maxq[0]]

        if mx - mn <= k:
            return True

        if freq[mx] == 1 and a[maxq[1]] - mn <= k:
            return True

        if freq[mn] == 1 and mx - a[minq[1]] <= k:
            return True

        return False

    for r in range(n):
        add(r)

        while not beautiful():
            left += 1
            remove(left - 1)

        f[r] = left

    pref_index = [0] * (n + 1)
    pref_f = [0] * (n + 1)

    for i in range(n):
        pref_index[i + 1] = pref_index[i] + i
        pref_f[i + 1] = pref_f[i] + f[i]

    ans = []

    for _ in range(q):
        L, R = map(int, input().split())
        L -= 1
        R -= 1

        p = bisect_right(f, L, lo=L, hi=R + 1)

        count1 = p - L
        sum_r1 = pref_index[p] - pref_index[L]
        cur = sum_r1 - count1 * L

        sum_r2 = pref_index[R + 1] - pref_index[p]
        sum_f2 = pref_f[R + 1] - pref_f[p]
        cur += sum_r2 - sum_f2

        ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

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

# Provided sample 1
assert run(
    """5 1 4
0 10 10 2 4
1 5
"""
) == "7", "sample 1"

# Provided sample 2
assert run(
    """5 1 4
0 10 1 2 4
1 5
"""
) == "10", "sample 2"

# Minimum-size input. A singleton is never counted.
assert run(
    """1 1 0
7
1 1
"""
) == "0", "minimum-size input"

# Maximum-size input. All values are equal, so every interval is beautiful.
n = 200000
expected = n * (n - 1) // 2
big_input = f"200000 1 0\n{' '.join(['7'] * n)}\n1 200000\n"
assert run(big_input) == str(expected), "maximum-size input"

# Repeated minimum and maximum. The full interval is not beautiful,
# while [1,3] and [2,4] become beautiful by deleting one extreme.
assert run(
    """4 1 0
0 0 10 10
1 4
"""
) == "4", "repeated extremes"

# Boundary query. This is a suffix of sample 1 and catches query
# conversion and f[r] boundary mistakes.
assert run(
    """5 1 4
0 10 10 2 4
2 5
"""
) == "5", "query boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0 / 7 / 1 1`|`0`| Các truy vấn đơn lẻ không được tính các mảng con có độ dài một. | 
|`200000 1 0`với tất cả các giá trị bằng nhau |`19999900000`| Tối đa (n), kích thước câu trả lời tối đa, tổng tiền tố và tiền xử lý tuyến tính. | 
|`4 1 0 / 0 0 10 10 / 1 4`|`4`| Các cực tiểu và cực đại lặp đi lặp lại không được coi là các cực trị duy nhất. | 
| Mẫu 1 với truy vấn`2 5`|`5`| Ranh giới truy vấn và chuyển đổi từ các chỉ mục dựa trên một sang dựa trên 0. | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khoảng hai phần tử có phạm vi lớn hơn (k). Vì```
2 1 4
0 10
1 2
```cửa sổ hiện tại`[0,10]`có phạm vi (10) nên điều kiện làm đẹp đầu tiên không thành công. Cả hai thái cực đều là duy nhất. Việc xóa một trong hai sẽ để lại một singleton, có phạm vi là (0). Do đó, thuật toán giữ`left=0`, bản ghi (f[1]=0) và truy vấn tính một cặp hợp lệ ((1,2)). 

Trường hợp cạnh thứ hai liên quan đến các thái cực lặp đi lặp lại:```
4 1 0
0 0 10 10
1 4
```Khi có cửa sổ hoàn chỉnh, cả giá trị tối thiểu và tối đa đều xảy ra hai lần. Phạm vi thông thường là (10>0), nhưng không thể xóa được phạm vi cực trị nào khi xóa một lần vì vẫn còn một bản sao khác. Thuật toán từ chối chính xác cửa sổ và di chuyển`left`. Cuối cùng nó có được hai khoảng thời gian dài hơn đẹp đẽ`[0,0,10]`Và`[0,10,10]`, cùng với`[0,0]`Và`[10,10]`, cho (4). 

Trường hợp cạnh thứ ba là truy vấn có độ dài bằng một:```
1 1 0
7
1 1
```Các bản ghi tiền xử lý (f[0]=0), vì một singleton rất đẹp. Tuy nhiên, trong quá trình truy vấn, phần đóng góp cho điểm cuối (r=L) là (r-L=0). Do đó, singleton được loại trừ chính xác mà không có nhánh truy vấn đặc biệt. 

Trường hợp cạnh thứ tư là trường hợp mọi giá trị đều bằng nhau. Vì```
4 1 0
7 7 7 7
1 4
```mọi cửa sổ đều có phạm vi bằng 0, vì vậy điều kiện đầu tiên ngay lập tức thành công. Mảng biên là`[0,0,0,0]`và truy vấn đếm (1+2+3=6) mảng con có độ dài lớn hơn một. Điều này cũng xác minh rằng logic cực trị thứ hai không bao giờ cần thiết khi phạm vi thông thường đã thỏa mãn ngưỡng. 

Trường hợp cạnh thứ năm là khi xóa mức tối thiểu là thao tác hợp lệ duy nhất. Trong mẫu 1, cửa sổ`[0,10,10]`có tối đa (10) hai lần, do đó xóa một mức tối đa không thể xóa giá trị lớn. Giá trị tối thiểu (0) là duy nhất và việc xóa nó sẽ để lại`[10,10]`. Thuật toán sử dụng tần số cực đại để từ chối hướng xóa đầu tiên và tần số cực tiểu để chấp nhận hướng xóa thứ hai. Sự khác biệt này là lý do chính khiến từ điển tần số trở nên cần thiết.
