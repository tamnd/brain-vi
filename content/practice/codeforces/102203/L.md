---
title: "CF 102203L - \u0412 \u043f\u043e\u0438\u0441\u043a\u0430\u0445 \u0438\u0441\u0442\u0438\u043d\u044b"
description: "Chúng tôi có một mảng không xác định (s1,s2,ldots,sn). Mảng đang tăng dần đến một số vị trí và giảm dần sau đó, do đó, nó có một đỉnh duy nhất. Tất cả các giá trị đều khác biệt."
date: "2026-08-18T00:54:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "L"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 70
verified: true
draft: false
---

[CF 102203L - \u0412 \u043f\u043e\u0438\u0441\u043a\u0430\u0445 \u0438\u0441\u0442\u0438\u043d\u044b](https://codeforces.com/problemset/problem/102203/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng không xác định (s_1,s_2,\ldots,s_n). Mảng đang tăng dần đến một số vị trí và giảm dần sau đó, do đó, nó có một đỉnh duy nhất. Tất cả các giá trị đều khác biệt. Chúng tôi biết (n) và giá trị mục tiêu (k), đồng thời đánh giá tương tác cho phép chúng tôi yêu cầu giá trị ở bất kỳ chỉ mục nào (i). Nhiệm vụ là tìm một chỉ mục có giá trị chính xác là (k). 

Bản thân mảng này không có sẵn cho chương trình. Mỗi truy cập mảng là một truy vấn tương tác, được viết dưới dạng`? i`, và trọng tài trả lời bằng (s_i). Khi chỉ mục yêu cầu đã được xác định, chương trình sẽ in`! i`và chấm dứt. 

Giới hạn (n\le 2\cdot10^5) ngay lập tức loại trừ việc quét mảng. Mặc dù thuật toán (O(n)) thông thường sẽ đủ nhỏ trong nhiều bài toán không tương tác, ở đây mỗi lần truy cập mảng đều là một truy vấn đắt tiền và chỉ có 80 truy vấn có sẵn. Chúng ta cần hành vi logarit. Vì (\log_2(2\cdot10^5)<18), ngay cả một số tìm kiếm nhị phân cũng vừa vặn thoải mái trong giới hạn truy vấn. 

Bản thân các giá trị nằm trong khoảng từ 0 đến (10^9), vì vậy số nguyên Python thông thường là quá đủ. Mục tiêu đảm bảo sẽ xảy ra nên sau khi xác định chính xác phần tăng giảm, một trong hai phép tìm kiếm nhị phân phải tìm ra nó. 

Có một số trường hợp ranh giới có thể phá vỡ việc triển khai bất cẩn. Coi như`n = 2`, với các giá trị`[5, 3]`Và`k = 3`. Đỉnh nằm ở vị trí đầu tiên, do đó phần giảm chứa câu trả lời ở chỉ số 2. Việc triển khai giả định đỉnh luôn là phần tử bên trong có thể thua trường hợp này và không bao giờ tìm kiếm chính xác vị trí thứ hai. 

Một trường hợp khác là`[1, 5, 3]`với`k = 1`. Câu trả lời là phần tử đầu tiên, nằm chính xác ở ranh giới bên trái của phần tăng dần. Tìm kiếm nhị phân khởi tạo giới hạn dưới không chính xác có thể loại bỏ nó. 

Tương tự,`[1, 5, 3]`với`k = 3`kiểm tra ranh giới bên phải của phần giảm. Câu trả lời là chỉ số 3, do đó tìm kiếm nhị phân giảm dần phải cho phép điểm cuối cuối cùng của nó. 

Yêu cầu kiểm tra một mảng "hoàn toàn bằng nhau" không thể được đáp ứng theo nghĩa đen vì bài toán ban đầu đảm bảo rằng tất cả (s_i) đều khác biệt. Một mảng như`[5, 5, 5]`không phải là đầu vào hợp pháp. Thử nghiệm có ý nghĩa gần nhất là một mảng đơn điệu nghiêm ngặt, trong đó đỉnh nằm ở một trong các điểm cuối, chẳng hạn như`[1, 2, 3, 4]`hoặc`[4, 3, 2, 1]`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là truy vấn các vị trí từ 1 đến (n) cho đến khi tìm thấy giá trị (k). Điều này đúng vì mọi vị trí cuối cùng đều được kiểm tra và câu trả lời được đảm bảo tồn tại. Vấn đề của nó là ngân sách truy vấn. Trong trường hợp xấu nhất, nó cần (n) truy vấn, có thể lên tới (200.000), trong khi thẩm phán chỉ cho phép 80. Vấn đề không chỉ đơn thuần là thời gian chạy mà còn vượt quá giới hạn giao thức tương tác. 

Cấu trúc của mảng cung cấp cho chúng ta nhiều thông tin hơn một mảng tùy ý. Về phần tăng dần, nếu (s_i<k) thì mục tiêu chỉ có thể ở bên phải. Nếu (s_i>k) thì chỉ có thể ở bên trái. Ở phần giảm dần, các hướng bị đảo ngược. Vì vậy, một khi chúng ta biết đỉnh ở đâu, bài toán sẽ trở thành hai phép tìm kiếm nhị phân thông thường. 

Chúng ta cũng có thể tìm thấy đỉnh bằng tìm kiếm nhị phân. Giả sử chúng ta truy vấn vị trí ở giữa (m) và so sánh (s_m) với (s_{m+1}). Nếu (s_m<s_{m+1}), chúng ta vẫn ở phía tăng nên đỉnh nằm hoàn toàn bên phải của (m). Nếu (s_m>s_{m+1}), chúng ta đã ở phía giảm dần, do đó đỉnh nằm ở (m) hoặc đâu đó ở bên trái của nó. 

Điều này cung cấp một tìm kiếm logarit cho đỉnh. Sau đó, chúng tôi tìm kiếm nhị phân đoạn tăng và đoạn giảm. Tổng số truy vấn nhiều nhất là khoảng (3\log_2 n), dưới 80 cho (n\le2\cdot10^5). 

Cách tiếp cận bạo lực hoạt động vì mọi vị trí đều được kiểm tra rõ ràng, nhưng không thành công vì mỗi lần kiểm tra chỉ sử dụng một trong 80 truy vấn. Thứ tự hình ngọn núi cho phép chúng ta thay thế các kiểm tra riêng lẻ đó bằng các quyết định nhị phân, giảm việc tìm kiếm từ các truy vấn (O(n)) thành (O(\log n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n)) truy vấn | (O(1)) | Quá chậm | 
| Tìm đỉnh + hai tìm kiếm nhị phân | (O(\log n)) truy vấn | (O(\log n)) giá trị được lưu trong bộ nhớ cache hoặc không gian phụ (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và (k). Chúng tôi chưa biết bất kỳ giá trị mảng nào, vì vậy mọi giá trị mà thuật toán cần phải được lấy thông qua truy vấn tương tác. 
2. Xác định hàm truy vấn in`? i`, xóa đầu ra ngay lập tức, đọc câu trả lời của giám khảo và trả lại. Việc xóa là bắt buộc vì nếu không, thẩm phán có thể đợi một truy vấn vẫn còn trong bộ đệm đầu ra của Python. 
3. Tìm đỉnh bằng tìm kiếm nhị phân. Duy trì một phạm vi`[lo, hi]`được đảm bảo chứa đỉnh. Trong khi`lo < hi`, lấy`mid = (lo + hi) // 2`và truy vấn`s[mid]`Và`s[mid + 1]`. 
4. Nếu`s[mid] < s[mid + 1]`, dãy số vẫn tăng tại`mid`, do đó đỉnh phải nằm hoàn toàn bên phải. Bộ`lo = mid + 1`. 
5. Nếu không,`s[mid] > s[mid + 1]`, bởi vì tất cả các giá trị đều khác biệt. Trình tự đã bắt đầu giảm dần nên đỉnh điểm là`mid`hoặc sang trái. Bộ`hi = mid`. 
6. Khi nào`lo == hi`, chỉ số đó là đỉnh cao. Mảng bây giờ được chia thành một phân đoạn tăng dần`[1, peak]`và một đoạn giảm dần`[peak + 1, n]`. 
7. Tìm kiếm nhị phân`[1, peak]`như một mảng tăng dần. Tại vị trí`mid`, nếu giá trị của nó nhỏ hơn`k`, di chuyển sang phải. Nếu nó lớn hơn`k`, di chuyển sang trái. Nếu nó bằng`k`, xuất chỉ mục đó ngay lập tức. 
8. Nếu lần tìm kiếm đầu tiên thất bại, tìm kiếm nhị phân`[peak + 1, n]`như một mảng giảm dần. Ở đây các hướng so sánh được đảo ngược. Nếu như`s[mid] > k`, di chuyển sang phải vì các giá trị nhỏ hơn về phía bên phải. Nếu như`s[mid] < k`, di chuyển sang trái. 
9. Khi tìm thấy mục tiêu, hãy in`! index`và chấm dứt. Sự đảm bảo rằng (k) xảy ra có nghĩa là lần tìm kiếm thứ hai cuối cùng sẽ tìm thấy nó nếu lần tìm kiếm đầu tiên không tìm thấy. 

### Tại sao nó hoạt động 

Bất biến tìm kiếm đỉnh là khoảng hiện tại luôn chứa phần tử tối đa. Nếu như`s[mid] < s[mid+1]`, mức tối đa không thể bằng hoặc trước`mid`, vì dãy số vẫn đang tăng lên ở đó nên việc chuyển sang`[mid+1, hi]`bảo toàn sự bất biến. Nếu như`s[mid] > s[mid+1]`, dãy số đã bắt đầu giảm dần`mid+1`, do đó đạt cực đại tại`mid`hoặc sớm hơn, bảo toàn bất biến với`[lo, mid]`. 

Sau khi biết đỉnh, mọi chỉ số bên trái của nó thuộc về một dãy tăng chặt và mọi chỉ số bên phải của nó thuộc về một dãy giảm chặt. Tìm kiếm nhị phân tương ứng giữ chính xác các chỉ số mà (k) vẫn có thể xảy ra. Vì mục tiêu được đảm bảo tồn tại nên một trong những tìm kiếm này sẽ đạt được chỉ mục của nó. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    cache = {}

    def ask(i):
        if i not in cache:
            print("?", i, flush=True)
            value = int(input())
            if value == -1:
                sys.exit(0)
            cache[i] = value
        return cache[i]

    # Find the peak.
    lo, hi = 1, n
    while lo < hi:
        mid = (lo + hi) // 2
        left = ask(mid)
        right = ask(mid + 1)

        if left < right:
            lo = mid + 1
        else:
            hi = mid

    peak = lo

    # Binary search on the increasing part [1, peak].
    lo, hi = 1, peak
    while lo <= hi:
        mid = (lo + hi) // 2
        value = ask(mid)

        if value == k:
            print("!", mid, flush=True)
            return
        if value < k:
            lo = mid + 1
        else:
            hi = mid - 1

    # Binary search on the decreasing part [peak + 1, n].
    lo, hi = peak + 1, n
    while lo <= hi:
        mid = (lo + hi) // 2
        value = ask(mid)

        if value == k:
            print("!", mid, flush=True)
            return
        if value > k:
            lo = mid + 1
        else:
            hi = mid - 1

if __name__ == "__main__":
    solve()
```các`ask`chức năng là nơi duy nhất giao tiếp với thẩm phán. Bộ đệm rất hữu ích vì việc phát hiện đỉnh có thể truy vấn lại một vị trí mà tìm kiếm nhị phân sau này cần. Việc sử dụng lại câu trả lời sẽ tránh được việc sử dụng một truy vấn tương tác khác trên cùng một chỉ mục. 

Việc sử dụng tìm kiếm cao điểm`lo = mid + 1`khi`s[mid] < s[mid + 1]`, bởi vì`mid`bản thân nó không thể là đỉnh cao trong tình huống đó. Ở nhánh kia,`mid`vẫn có thể đạt đỉnh nên bản cập nhật chính xác là`hi = mid`, không`mid - 1`. 

Tìm kiếm nhị phân ngày càng tăng sử dụng các quy tắc so sánh thông thường. Việc tìm kiếm giảm dần sẽ đảo ngược hướng vì di chuyển sang phải làm cho các giá trị nhỏ hơn. 

Python không có vấn đề tràn số nguyên ở đây và`(lo + hi) // 2`là an toàn trong giới hạn nhất định. Chi tiết triển khai quan trọng là`flush=True`trên mọi câu hỏi và trên câu trả lời cuối cùng. Nếu không xóa, một thuật toán tương tác đúng có thể thất bại vì trọng tài không bao giờ nhận được yêu cầu. 

các`-1`xử lý là một biện pháp phòng thủ thông thường dành cho các thẩm phán tương tác sử dụng phản hồi tiêu cực để báo hiệu một truy vấn không hợp lệ hoặc lỗi giao thức. Câu lệnh ban đầu không yêu cầu sử dụng phản hồi như vậy để thực thi hợp lệ, nhưng việc chấm dứt khi nó xuất hiện sẽ ngăn chương trình tiếp tục với đầu vào vô nghĩa. 

Mã này dành cho giám khảo tương tác ban đầu. Người chạy theo đợt bình thường không thể thực hiện trực tiếp vì trọng tài chịu trách nhiệm trả lời`? i`. 

## Ví dụ đã hoạt động 

Tuyên bố có chứa một mẫu tương tác. Mảng ẩn trong mẫu đó là`[1, 3, 10, 8, 2]`, với`n = 5`Và`k = 3`. Dấu vết sau đây mô tả các quyết định được thực hiện bởi thuật toán. Bởi vì mẫu ban đầu liệt kê một chuỗi truy vấn cụ thể thay vì bắt buộc một chiến lược duy nhất, nên các truy vấn chính xác được tạo ra bởi quá trình triển khai này có thể khác nhau trong khi câu trả lời cuối cùng vẫn giống nhau. 

| Sân khấu | Phạm vi | Truy vấn | Giá trị | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tìm kiếm đỉnh cao |`[1, 5]`|`3, 4`|`10, 8`| Đỉnh điểm đang ở`[1, 3]`| 
| Tìm kiếm đỉnh cao |`[1, 3]`|`2, 3`|`3, 10`| Đỉnh điểm đang ở`[3, 3]`| 
| Tăng cường tìm kiếm |`[1, 3]`|`2`|`3`| Đã tìm thấy mục tiêu | 
| Trả lời | | | |`! 2`| 

Đỉnh là vị trí 3 vì chuỗi tăng từ 1 lên 3 đến 10 rồi giảm xuống 8 và 2. Khi tìm kiếm phía tăng dần, vị trí 2 ngay lập tức chứa giá trị được yêu cầu. 

Ví dụ thứ hai, hãy xem xét mảng ẩn`[2, 6, 11, 9, 4, 1]`với`k = 4`. Mục tiêu nằm ở phía giảm dần, vô tình thực hiện phần thuật toán dễ đảo ngược nhất. 

| Sân khấu | Phạm vi | Truy vấn | Giá trị | Quyết định | 
| --- | --- | --- | --- | --- | 
| Tìm kiếm đỉnh cao |`[1, 6]`|`3, 4`|`11, 9`| Đỉnh điểm đang ở`[1, 3]`| 
| Tìm kiếm đỉnh cao |`[1, 3]`|`2, 3`|`6, 11`| Đỉnh điểm đang ở`[3, 3]`| 
| Tăng cường tìm kiếm |`[1, 3]`|`2`|`6`| Mục tiêu còn lại là 2 | 
| Tăng cường tìm kiếm |`[1, 1]`|`1`|`2`| Bên tăng thất bại | 
| Giảm tìm kiếm |`[4, 6]`|`5`|`4`| Đã tìm thấy mục tiêu | 
| Trả lời | | | |`! 5`| 

Ở truy vấn đầu tiên của tìm kiếm giảm dần, giá trị đã bằng mục tiêu. Thay vào đó, nếu mảng được tìm kiếm bằng quy tắc so sánh mảng tăng dần thì hướng tìm kiếm có thể bị đảo ngược và câu trả lời sẽ bị mất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log n)) truy vấn | Phát hiện đỉnh và hai tìm kiếm nhị phân, mỗi tìm kiếm yêu cầu nhiều truy vấn logarit | 
| Không gian | (O(\log n)) | Bộ đệm lưu trữ các chỉ mục được truy vấn và tối đa cần có số logarit của các vị trí riêng biệt | 

Đối với (n\le2\cdot10^5), (\log_2 n) dưới 18. Tìm kiếm tối đa mất ít hơn 18 lần lặp và mỗi lần lặp yêu cầu tối đa hai vị trí. Mỗi một trong hai tìm kiếm mục tiêu mất ít hơn 18 lần lặp. Do đó, tổng số có thể thoải mái dưới giới hạn 80 truy vấn. Việc sử dụng bộ nhớ rất nhỏ so với giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Vì nhiệm vụ ban đầu có tính tương tác nên thông thường`run(input) -> output`mẫu thử nghiệm đơn vị không thể được áp dụng trực tiếp. Bộ khai thác kiểm tra phải mô phỏng người đánh giá bằng cách cung cấp các giá trị cho các chỉ số được yêu cầu. Các thử nghiệm sau đây sử dụng phiên bản hàng loạt của cùng một thuật toán, trong đó mảng ẩn hiển thị rõ ràng cho mã thử nghiệm. Điều này cũng làm cho các khẳng định mang tính quyết định.```python
import io
import sys

def find_target(a, k):
    n = len(a)
    queries = 0

    def ask(i):
        nonlocal queries
        queries += 1
        return a[i - 1]

    lo, hi = 1, n

    while lo < hi:
        mid = (lo + hi) // 2
        left = ask(mid)
        right = ask(mid + 1)

        if left < right:
            lo = mid + 1
        else:
            hi = mid

    peak = lo

    lo, hi = 1, peak
    while lo <= hi:
        mid = (lo + hi) // 2
        value = ask(mid)

        if value == k:
            return mid, queries
        if value < k:
            lo = mid + 1
        else:
            hi = mid - 1

    lo, hi = peak + 1, n
    while lo <= hi:
        mid = (lo + hi) // 2
        value = ask(mid)

        if value == k:
            return mid, queries
        if value > k:
            lo = mid + 1
        else:
            hi = mid - 1

    raise AssertionError("Target is guaranteed to exist")

# Provided sample.
assert find_target([1, 3, 10, 8, 2], 3)[0] == 2

# Minimum-size input, peak at the first position.
assert find_target([5, 3], 3)[0] == 2

# Minimum-size input, peak at the second position.
assert find_target([3, 5], 3)[0] == 1

# Target is exactly the peak.
assert find_target([1, 4, 9, 7, 2], 9)[0] == 3

# Target is at the right boundary.
assert find_target([1, 5, 8, 6, 3], 3)[0] == 5

# Strictly increasing array, peak at the right boundary.
assert find_target([1, 2, 3, 4, 5], 1)[0] == 1

# Strictly decreasing array, peak at the left boundary.
assert find_target([5, 4, 3, 2, 1], 1)[0] == 5

# Large legal instance.
a = list(range(100000))
a += list(range(100000, 0, -1))
expected = 150000
assert find_target(a, a[expected - 1])[0] == expected
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1, 3, 10, 8, 2], k=3`|`2`| Mẫu được cung cấp | 
|`[5, 3], k=3`|`2`| Kích thước tối thiểu, đỉnh ở ranh giới bên trái | 
|`[3, 5], k=3`|`1`| Kích thước tối thiểu, đỉnh ở ranh giới bên phải | 
|`[1, 4, 9, 7, 2], k=9`|`3`| Mục tiêu bằng đỉnh | 
|`[1, 5, 8, 6, 3], k=3`|`5`| Nhắm mục tiêu ở vị trí cuối cùng | 
|`[1,2,3,4,5], k=1`|`1`| Toàn bộ mảng đang tăng | 
|`[5,4,3,2,1], k=1`|`5`| Toàn bộ mảng đang giảm | 
| mảng núi lớn |`150000`| Đầu vào lớn và hành vi logarit | 

Trường hợp "tất cả các giá trị bằng nhau" được yêu cầu trong đặc tả thử nghiệm đã cố tình vắng mặt. Mảng như vậy vi phạm điều kiện ban đầu là mọi (s_i) đều khác biệt. Việc thay thế nó bằng các mảng đơn điệu nghiêm ngặt sẽ kiểm tra hành vi biên tương ứng mà không kiểm tra trạng thái không thể. 

## Vỏ cạnh 

Để có đỉnh ở vị trí đầu tiên, hãy xem xét`n = 2`,`k = 3`và mảng ẩn`[5, 3]`. Trong quá trình phát hiện đỉnh, việc so sánh vị trí 1 và 2 cho`5 > 3`, do đó phạm vi cực đại trở thành`[1, 1]`. Tìm kiếm tăng dần kiểm tra vị trí 1 và thất bại vì giá trị của nó là 5. Tìm kiếm giảm dần kiểm tra vị trí 2 và tìm thấy 3, tạo ra`! 2`. Trường hợp này hoạt động vì khoảng thời gian đỉnh được phép thu gọn trực tiếp về điểm cuối bên trái. 

Để có đỉnh ở vị trí cuối cùng, hãy xem xét`[3, 5]`với`k = 3`. Sự so sánh`3 < 5`di chuyển tìm kiếm cao nhất đến vị trí 2. Tìm kiếm tăng dần bao trùm cả hai vị trí và tìm thấy mục tiêu ở vị trí 1. Không có phân đoạn giảm nào cần chứa bất kỳ phần tử nào. Điều này xác nhận việc sử dụng`[peak + 1, n]`cho lần tìm kiếm thứ hai thay vì buộc phạm vi không hợp lệ. 

Đối với mục tiêu bằng mức đỉnh, hãy xem xét`[1, 4, 9, 7, 2]`với`k = 9`. Phát hiện đỉnh xác định vị trí 3. Tìm kiếm nhị phân tăng dần bao gồm chính đỉnh đó, truy vấn vị trí 3 và trả về ngay lập tức. Việc loại trừ đỉnh khỏi tìm kiếm nhị phân đầu tiên sẽ là một thay đổi ranh giới không cần thiết và có thể không chính xác. 

Đối với mục tiêu ở vị trí ngoài cùng bên phải, hãy xem xét`[1, 5, 8, 6, 3]`với`k = 3`. Đỉnh cao là vị trí thứ 3 nên lượng tìm kiếm giảm dần bao gồm`[4, 5]`. Truy vấn vị trí 5 trả về 3. Việc tìm kiếm phải sử dụng`hi = n`bao gồm, nếu không thì mục tiêu ở chỉ mục cuối cùng sẽ bị loại bỏ một cách âm thầm. 

Đối với một mảng tăng hoàn toàn như`[1, 2, 3, 4, 5]`, đỉnh là vị trí 5. Mọi so sánh trong quá trình phát hiện đỉnh đều tìm thấy`s[mid] < s[mid+1]`, do đó giới hạn dưới sẽ di chuyển sang phải cho đến khi đạt đến 5. Sau đó, tìm kiếm nhị phân đầu tiên sẽ bao phủ toàn bộ mảng. Điều này hợp pháp vì định nghĩa cho phép điểm ngoặt là (n). 

Đối với một mảng giảm hoàn toàn như`[5, 4, 3, 2, 1]`, đỉnh là vị trí 1. Mọi so sánh đỉnh đều tìm thấy`s[mid] > s[mid+1]`, do đó giới hạn trên di chuyển sang trái cho đến khi đạt đến 1. Phần tăng chỉ chứa vị trí 1 và phần tìm kiếm giảm xử lý các vị trí từ 2 đến 5. Đây là trường hợp biên đối xứng. 

Cuối cùng, không được xử lý các giá trị trùng lặp như thể chúng có thể thực hiện được. Ví dụ,`[1, 4, 4, 2]`sẽ làm cho sự so sánh giữa vị trí 2 và 3 bằng nhau, nhưng vấn đề cấm một mảng như vậy. Tìm kiếm đỉnh dựa trên thực tế là mọi so sánh đều tăng hoặc giảm. Việc thêm một nhánh bình đẳng vào quá trình triển khai sẽ giải quyết được một vấn đề khác và không cần thiết ở đây.
