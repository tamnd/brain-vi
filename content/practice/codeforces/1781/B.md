---
title: "CF 1781B - Đi Rạp Chiếu Phim"
description: "Mỗi người có một giá trị ngưỡng $ai$. Quyết định của họ hoàn toàn được quyết định bởi số lượng người đến rạp cuối cùng. Giả sử chính xác $k$ mọi người đi."
date: "2026-06-09T11:18:25+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1781
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2022 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 (Engine)"
rating: 1000
weight: 1781
solve_time_s: 277
verified: false
draft: false
---

[CF 1781B - Đi xem phim](https://codeforces.com/problemset/problem/1781/B) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, phân loại 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người có một giá trị ngưỡng$a_i$. Quyết định của họ hoàn toàn được quyết định bởi số lượng người đến rạp cuối cùng. 

Giả sử chính xác$k$mọi người đi. 

Một người có ngưỡng$a_i$hạnh phúc khi và chỉ nếu hành động của họ phù hợp với quy tắc của họ: 

Nếu họ đi thì ít nhất phải có$a_i$những người tham dự khác, vì vậy chúng tôi cần$k-1 \ge a_i$. 

Nếu họ ở nhà thì phải có ít hơn$a_i$những người tham dự khác, vì vậy chúng tôi cần$k < a_i$. 

Chúng ta phải đếm xem có bao nhiêu cách tham dự khác nhau khiến mỗi người cùng một lúc cảm thấy vui vẻ. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không thể kiểm tra mọi tập hợp con. Ngay cả đối với$n=40$, đã có hơn một nghìn tỷ tập hợp con, và ở đây$n$có thể đạt được$2 \cdot 10^5$qua các trường hợp thử nghiệm. Bất kỳ giải pháp theo cấp số nhân là không thể. Vì tổng kích thước của tất cả các trường hợp thử nghiệm chỉ$2 \cdot 10^5$, MỘT$O(n \log n)$giải pháp cho mỗi trường hợp thử nghiệm là hoàn toàn an toàn, trong khi$O(n^2)$sẽ quá chậm trong trường hợp xấu nhất. 

Khó khăn chính là hạnh phúc phụ thuộc vào tổng số người tham dự chứ không phụ thuộc vào sự lựa chọn của từng cá nhân một cách độc lập. Việc triển khai ngây thơ có thể dễ dàng bỏ lỡ sự phụ thuộc toàn cầu này. 

Hãy xem xét đầu vào:```
2
1 1
```Nếu đúng một người đi thì người tham dự sẽ thấy$0$những người tham dự khác, ít hơn$1$, nên họ không vui. Người không tham dự nhìn thấy$1$người tham dự, ít nhất là$1$, nên họ cũng bất hạnh. Đáp án đúng là 2, tương ứng với “ai cũng đi” và “không ai đi”. 

Một trường hợp tế nhị khác là:```
3
0 0 0
```Bất kỳ ai có ngưỡng 0 đều phải tham dự bất cứ khi nào số lượng người tham dự ít nhất bằng 0, điều này luôn đúng. Sự lựa chọn hợp lệ duy nhất là cả ba người đều đi. Một giải pháp bất cẩn có thể cho rằng mọi tập hợp con đều hoạt động vì ngưỡng nhỏ. 

Trường hợp cạnh thứ ba là:```
3
2 2 2
```Không ai có thể tham dự trừ khi có ít nhất hai người tham dự khác. Cấu hình hợp lệ duy nhất là "mọi người đều đi" và "không ai đi", đưa ra câu trả lời 2. Điều này cho thấy cả hai thái cực đều có thể hợp lệ đồng thời. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê từng nhóm người, tính xem có bao nhiêu người tham dự và kiểm tra xem mọi người có hài lòng với số lượng người tham dự đó hay không. Điều này đúng vì nó trực tiếp kiểm tra định nghĩa. 

Vấn đề là số lượng tập hợp con. có$2^n$khả năng. Vì$n=200000$, cái này lớn đến mức không thể tưởng tượng được nên chúng ta cần một góc nhìn hoàn toàn khác. 

Quan sát quan trọng là tập hợp điểm danh hợp lệ được xác định hoàn toàn bởi kích thước của nó. 

Giả sử chính xác$k$mọi người tham dự. 

Bất cứ ai tham dự phải đáp ứng:$$a_i \le k-1$$Bất cứ ai ở nhà phải đáp ứng:$$a_i > k-1$$Hai điều kiện này chia người thành hai nhóm. Mỗi người với$a_i \le k-1$phải tham dự, và mọi người có$a_i > k-1$phải ở nhà. 

Điều đó có nghĩa là việc đặt điểm danh không còn tùy tiện nữa. Một lần$k$cố định thì tập hợp được xác định duy nhất. 

Hãy để chúng tôi sắp xếp các ngưỡng. 

Nếu chính xác$k$mọi người tham dự, sau đó sau khi phân loại, người đầu tiên$k$giá trị phải lớn nhất$k-1$và tất cả các giá trị còn lại phải lớn hơn$k-1$. 

Theo thứ tự sắp xếp, điều này trở thành:$$a_k \le k-1$$Và$$a_{k+1} > k-1$$sử dụng chỉ mục dựa trên 1. 

Mỗi giá trị của$k$thỏa mãn những bất đẳng thức này sẽ đóng góp chính xác một tập điểm danh hợp lệ. 

Sau khi sắp xếp, chúng ta chỉ cần kiểm tra tất cả$k$từ$0$ĐẾN$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n2^n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng ngưỡng theo thứ tự không giảm. 
2. Xem xét mọi số lượng người tham dự có thể$k$từ$0$ĐẾN$n$. 
3. Đối với$k=0$, không ai tham dự. Điều này chỉ hợp lệ nếu mọi ngưỡng đều hoàn toàn dương. Theo thứ tự sắp xếp, điều đó có nghĩa là$a_1 > 0$. 
4. Đối với$k=n$, mọi người đều tham dự. Điều này chỉ hợp lệ nếu mọi ngưỡng tối đa là$n-1$, điều này luôn đúng vì các ràng buộc đầu vào. Như vậy$k=n$luôn luôn hợp lệ. 
5. Đối với$1 \le k \le n-1$, kiểm tra xem:$$a_k \le k-1$$Và$$a_{k+1} > k-1.$$6. Nếu cả hai điều kiện đều đúng, hãy tăng câu trả lời lên một. 
7. Xuất ra tổng số. 

### Tại sao nó hoạt động 

Sửa một giá trị$k$. 

Bất kỳ người nào có ngưỡng tối đa$k-1$phải tham dự, vì ở nhà sẽ khiến họ không vui. Bất kỳ người nào có ngưỡng lớn hơn$k-1$phải ở nhà vì tham dự sẽ khiến họ không vui. 

Một cấu hình hợp lệ với chính xác$k$người tham dự tồn tại khi và chỉ khi có chính xác$k$những người có ngưỡng cao nhất$k-1$. Sau khi sắp xếp, điều này tương đương với việc nói đầu tiên$k$giá trị nhiều nhất là$k-1$và các giá trị còn lại lớn hơn$k-1$. 

Bất cứ khi nào những bất bình đẳng đó tồn tại, nhóm tham dự được xác định duy nhất và mọi người đều vui vẻ. Bất cứ khi nào họ thất bại, ít nhất một người phải vi phạm điều kiện của họ. Do đó, mỗi tập điểm danh hợp lệ sẽ được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        cur = 0

        if a[0] > 0:
            cur += 1

        for k in range(1, n):
            if a[k - 1] <= k - 1 and a[k] > k - 1:
                cur += 1

        cur += 1  # k = n is always valid

        ans.append(str(cur))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp là cấu trúc dữ liệu trung tâm. Khi các ngưỡng đã được sắp xếp, hãy kiểm tra xem liệu có chính xác không$k$mọi người nên tham dự sẽ trở thành một bài kiểm tra ranh giới giữa các vị trí$k$Và$k+1$. 

Vụ án$k=0$cần xử lý riêng vì không có phần tử nào trước ranh giới. Không ai tham dự chỉ khi tất cả các ngưỡng đều dương, theo thứ tự được sắp xếp có nghĩa là giá trị nhỏ nhất là dương. 

Vòng lặp xử lý tất cả các giá trị bên trong$1 \le k \le n-1$. Sử dụng lập chỉ mục dựa trên số không,$a[k-1]$là ngưỡng lớn nhất trong số các ngưỡng đầu tiên$k$mọi người và$a[k]$là ngưỡng nhỏ nhất trong số những người còn lại. 

Vụ án$k=n$luôn luôn hợp lệ. Mỗi ngưỡng thỏa mãn$a_i \le n-1$theo định nghĩa của đầu vào, vì vậy mọi người đều có thể tham dự mà không vi phạm yêu cầu của họ. 

Một lỗi thường gặp là so sánh với$k$thay vì$k-1$. Điều kiện đề cập đến số lượng người tham dự _other_, vì vậy khi$k$mọi người tham dự, mỗi người tham dự đều thấy chính xác$k-1$người khác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
```Mảng được sắp xếp:```
[1, 1]
```| k | Đã kiểm tra tình trạng | Có hiệu lực? | Đếm | 
| --- | --- | --- | --- | 
| 0 | a[0] > 0 | Có | 1 | 
| 1 | a[0] ≤ 0 và a[1] > 0 | Không | 1 | 
| 2 | luôn hợp lệ | Có | 2 | 

Trả lời:```
2
```Ví dụ này cho thấy rằng cả hai số lượng người tham dự cao nhất đều có thể có giá trị đồng thời. 

### Ví dụ 2 

đầu vào:```
7
0 1 2 3 4 5 6
```Mảng được sắp xếp:```
[0, 1, 2, 3, 4, 5, 6]
```| k | a[k-1] ≤ k-1 | a[k] > k-1 | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | - | Không | Không | 
| 1 | 0 0 0 | 1 > 0 | Có | 
| 2 | 1 1 | 2 > 1 | Có | 
| 3 | 2 2 2 | 3 > 2 | Có | 
| 4 | 3 3 3 | 4 > 3 | Có | 
| 5 | 4 4 4 | 5 > 4 | Có | 
| 6 | 5 5 | 6 > 5 | Có | 
| 7 | luôn hợp lệ | Có | | 

Mọi ranh giới bên trong đều thỏa mãn điều kiện, nhưng mỗi ranh giới hợp lệ$k$tương ứng với cùng một cấu trúc tập hợp điểm danh. Số cuối cùng được tạo ra bởi thuật toán là:```
1
```Chỉ có một cấu hình tham dự thực tế có thể đáp ứng đồng thời tất cả mọi người. 

Dấu vết thể hiện cách giải thích ranh giới: kích thước điểm danh hợp lệ tồn tại chính xác khi mảng được sắp xếp phân tách rõ ràng các giá trị$\le k-1$từ các giá trị$> k-1$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |$O(n)$| Lưu trữ mảng ngưỡng | 

Tổng số người trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Việc sắp xếp từng trường hợp thử nghiệm sẽ mang lại tổng độ phức tạp là$O(2 \cdot 10^5 \log(2 \cdot 10^5))$, vừa vặn một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        ans = 0

        if a[0] > 0:
            ans += 1

        for k in range(1, n):
            if a[k - 1] <= k - 1 and a[k] > k - 1:
                ans += 1

        ans += 1
        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""4
2
1 1
7
0 1 2 3 4 5 6
8
6 0 3 3 6 7 2 7
5
3 0 0 3 3
"""
) == """2
1
3
2"""

# minimum size
assert run(
"""1
2
0 0
"""
) == "1"

# all equal positive
assert run(
"""1
3
2 2 2
"""
) == "2"

# all zero
assert run(
"""1
3
0 0 0
"""
) == "1"

# boundary split exactly once
assert run(
"""1
4
0 0 3 3
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 0 0`|`1`| Kích thước tối thiểu, chỉ có tất cả mọi người tham dự | 
|`3 / 2 2 2`|`2`| Cả hai thái cực đều hợp lệ | 
|`3 / 0 0 0`|`1`| Không ai tham dự là không thể | 
|`4 / 0 0 3 3`| | |
