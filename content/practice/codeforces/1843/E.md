---
title: "CF 1843E - Phân đoạn theo dõi"
description: "Chúng ta bắt đầu với một mảng có độ dài n chứa đầy các số 0. Có m đoạn. Một đoạn được coi là đẹp khi số lượng số 1 bên trong nó lớn hơn số số 0 bên trong cùng một đoạn."
date: "2026-06-09T06:09:07+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1843
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 881 (Div. 3)"
rating: 1600
weight: 1843
solve_time_s: 107
verified: true
draft: false
---

[CF 1843E - Phân đoạn theo dõi](https://codeforces.com/problemset/problem/1843/E) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, brute Force, cấu trúc dữ liệu, hai con trỏ 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một mảng có độ dài`n`được lấp đầy hoàn toàn bằng số không. 

có`m`phân đoạn. Một đoạn được coi là đẹp khi số lượng số 1 bên trong nó lớn hơn số số 0 bên trong cùng một đoạn. Đối với một đoạn có độ dài`len`, điều kiện này tương đương với:$$\text{ones} > \frac{\text{len}}{2}$$Chúng ta cũng được cung cấp một chuỗi`q`cập nhật. các`i`-th cập nhật lần lượt vị trí`x_i`vào trong`1`. Các vị trí là khác nhau, vì vậy một khi một vị trí trở thành`1`, nó ở lại`1`. 

Nhiệm vụ là tìm số cập nhật sớm nhất mà sau đó ít nhất một trong các phân đoạn đã cho trở nên đẹp. Nếu không có phân đoạn nào đẹp ngay cả sau khi áp dụng tất cả các cập nhật, chúng ta sẽ xuất ra`-1`. 

Các ràng buộc ngay lập tức cho thấy rằng việc mô phỏng mọi bản cập nhật và kiểm tra lại mọi phân đoạn từ đầu là quá tốn kém. Trên tất cả các trường hợp thử nghiệm, tổng của`n`nhiều nhất là`10^5`, nhưng một trường hợp thử nghiệm duy nhất vẫn có thể chứa tới`10^5`phân đoạn và`10^5`cập nhật. Bất kỳ giải pháp nào thực hiện công việc tỷ lệ thuận với`m × q`vượt xa những gì vừa vặn trong hai giây. 

Một quan sát tinh tế là các bản cập nhật chỉ thêm nhiều cái khác. Một khi một đoạn đã trở nên đẹp thì nó không bao giờ có thể ngừng đẹp được. Hành vi đơn điệu này là chìa khóa cho giải pháp. 

Hãy xem xét một ví dụ nhỏ:```
n = 5
segment = [1,5]
updates = 5,3,1,2,4
```Sau lần cập nhật đầu tiên có một`1`, sau cái thứ hai có hai, sau cái thứ ba có ba. Độ dài đoạn là năm nên ba đoạn là đủ để làm đẹp. Câu trả lời là`3`. 

Một sai lầm dễ mắc phải khác là quên rằng điều kiện thực sự lớn hơn.```
segment length = 4
ones = 2
zeros = 2
```Điều này không đẹp. Việc thực hiện bất cẩn bằng cách sử dụng`>=`thay vì`>`sẽ chấp nhận nó một cách sai lầm. 

Một trường hợp cạnh khác xảy ra khi một đoạn bao gồm một vị trí duy nhất.```
segment = [3,3]
```Vị trí khoảnh khắc`3`trở thành`1`, đoạn chứa một số một và không có số 0 nên đẹp ngay. Câu trả lời có thể là`1`. 

Cuối cùng, một số trường hợp không bao giờ tạo ra một đoạn đẹp.```
n = 4
segments = [1,1], [4,4]
updates = 2,3
```Cả hai đoạn đều không được chạm vào nên câu trả lời đúng là`-1`. 

## Phương pháp tiếp cận 

Việc mô phỏng trực tiếp rất đơn giản. Sau mỗi lần cập nhật, đánh dấu vị trí mới là`1`, sau đó kiểm tra từng đoạn. Đối với mỗi phân đoạn, hãy đếm xem nó hiện có bao nhiêu phân đoạn và kiểm tra xem số lượng đó có vượt quá một nửa độ dài phân đoạn hay không. 

Phiên bản brute-force là chính xác vì nó thực hiện đúng định nghĩa. Vấn đề là chi phí. Với`q`cập nhật và`m`các phân đoạn, ngay cả khi việc đếm các phân đoạn bên trong một phân đoạn là thời gian không đổi, chúng tôi vẫn sẽ thực hiện`m × q`séc. Trong trường hợp xấu nhất, điều này trở nên đại khái`10^10`hoạt động hoàn toàn không thể thực hiện được. 

Bước ngoặt đến từ việc nhận thấy đặc tính “đoạn nào đó đẹp” là đơn điệu. 

Nếu sau lần đầu tiên`k`cập nhật một số phân đoạn là đẹp, sau đó sau bất kỳ số lượng cập nhật lớn hơn nào, nó vẫn đẹp vì các bản cập nhật chỉ thêm những cái đó. Điều này có nghĩa là câu trả lời là vị trí đầu tiên mà một vị từ đơn điệu trở thành đúng. 

Bất cứ khi nào một bài toán yêu cầu điểm sớm nhất mà điều kiện đơn điệu xuất hiện, tìm kiếm nhị phân sẽ trở thành một ứng cử viên đương nhiên. 

Giả sử chúng ta đoán một giá trị`mid`. Chúng tôi hỏi: 

> Sau khi áp dụng lần đầu tiên`mid`cập nhật, có đoạn nào đẹp không? 

Để trả lời câu hỏi này một cách hiệu quả, chúng tôi xây dựng một mảng đánh dấu những vị trí nào đã được kích hoạt trong số những vị trí đầu tiên.`mid`cập nhật. Sau đó, tổng tiền tố cho phép chúng ta tính số lượng đơn vị bên trong bất kỳ phân đoạn nào trong thời gian không đổi. 

Đối với một phân khúc`[l,r]`:$$\text{ones} = pref[r] - pref[l-1]$$Đoạn đẹp khi:$$\text{ones} > (r-l+1) - \text{ones}$$Kiểm tra tất cả các chi phí phân đoạn`O(m)`và xây dựng tổng chi phí tiền tố`O(n)`. 

Do đó, vị ngữ có thể được đánh giá trong`O(n+m)`thời gian. Kết hợp điều đó với tìm kiếm nhị phân trên`q`bản cập nhật đưa ra một giải pháp được chấp nhận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(qm) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phân đoạn và tất cả các vị trí cập nhật. 
2. Xác định hàm`check(k)`xác định xem một số phân đoạn có đẹp hay không sau khi áp dụng lần đầu tiên`k`cập nhật. 
3. Bên trong`check(k)`, tạo một mảng có độ dài`n`được khởi tạo bằng số không. 
4. Đánh dấu đầu tiên`k`vị trí được cập nhật như`1`. 
5. Xây dựng mảng tổng tiền tố trên mảng nhị phân này. 
6. Đối với mọi phân khúc`[l,r]`, hãy tính số lượng đơn vị bên trong nó bằng cách sử dụng tổng tiền tố. 
7. Hãy để`length = r - l + 1`. Nếu số lượng cái lớn hơn nhiều so với`length / 2`, quay lại ngay`True`. 
8. Nếu không có đoạn nào thỏa mãn điều kiện thì trả về`False`. 
9. Trước khi tìm kiếm nhị phân, hãy đánh giá`check(q)`. Nếu nó sai thì không có đoạn nào trở nên đẹp, do đó đầu ra`-1`. 
10. Nếu không thì tìm kiếm nhị phân trên câu trả lời. Duy trì số lượng cập nhật nhỏ nhất có vị từ đúng. 
11. Xuất số cập nhật hợp lệ ngoài cùng bên trái. 

### Tại sao nó hoạt động 

Vị ngữ được kiểm tra bởi`check(k)`là: 

> Ít nhất có một đoạn đẹp sau đoạn đầu tiên`k`cập nhật. 

Áp dụng nhiều bản cập nhật hơn chỉ có thể tăng số lượng bản cập nhật trong mỗi phân khúc. Một đoạn đẹp sau này không bao giờ có thể trở nên không đẹp. Do đó, vị từ có tính đơn điệu: một khi đã đúng thì nó sẽ đúng mãi mãi. 

Tổng tiền tố tính toán chính xác số lượng vị trí được kích hoạt bên trong mỗi phân đoạn, do đó`check(k)`trả về true chính xác khi điều kiện bài toán được thỏa mãn sau`k`cập nhật. 

Tìm kiếm nhị phân trên một vị từ đơn điệu luôn tìm thấy chỉ mục nhỏ nhất mà vị từ đó trở thành đúng. Chỉ mục đó chính xác là bản cập nhật đầu tiên sau đó một số phân đoạn trở nên đẹp đẽ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())

        segments = []
        for _ in range(m):
            l, r = map(int, input().split())
            segments.append((l, r))

        q = int(input())
        queries = [int(input()) for _ in range(q)]

        def check(k):
            arr = [0] * (n + 1)

            for i in range(k):
                arr[queries[i]] = 1

            pref = [0] * (n + 1)
            for i in range(1, n + 1):
                pref[i] = pref[i - 1] + arr[i]

            for l, r in segments:
                ones = pref[r] - pref[l - 1]
                length = r - l + 1

                if ones * 2 > length:
                    return True

            return False

        if not check(q):
            print(-1)
            continue

        lo, hi = 1, q
        ans = q

        while lo <= hi:
            mid = (lo + hi) // 2

            if check(mid):
                ans = mid
                hi = mid - 1
            else:
                lo = mid + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xoay quanh`check`chức năng. Nó xây dựng lại trạng thái mảng sau lần đầu tiên`k`cập nhật, sau đó sử dụng mảng tổng tiền tố để trả lời các truy vấn đếm phân đoạn trong thời gian không đổi. 

điều kiện```
ones * 2 > length
```tương đương với việc kiểm tra xem số lượng số một có vượt quá số lượng số không hay không. Sử dụng phép nhân sẽ tránh được mọi vấn đề với phép chia số nguyên. 

Tìm kiếm nhị phân tìm kiếm giá trị đầu tiên của`k`vì cái gì`check(k)`trả về đúng. Vì vị từ là đơn điệu nên tìm kiếm nhị phân đúng ngoài cùng bên trái tiêu chuẩn sẽ áp dụng trực tiếp. 

Một chi tiết triển khai dễ mắc sai lầm là việc lập chỉ mục. Các phân đoạn được lập chỉ mục 1, do đó mảng tổng tiền tố cũng được xây dựng dưới dạng 1 chỉ mục. Điều này làm cho công thức```
pref[r] - pref[l - 1]
```làm việc một cách tự nhiên. 

Một điểm tinh tế khác là kiểm tra`check(q)`trước khi tìm kiếm nhị phân. Nếu vị từ sai ngay cả sau khi cập nhật tất cả, tìm kiếm nhị phân sẽ không có câu trả lời hợp lệ. Việc xử lý riêng trường hợp này giúp logic tìm kiếm trở nên đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
segments = [1,2], [4,5], [1,5], [1,3], [2,4]
updates = 5,3,1,2,4
```Đánh giá tìm kiếm nhị phân: 

| k | Vị trí hoạt động | Phân đoạn đẹp tồn tại? | 
| --- | --- | --- | 
| 3 | {1,3,5} | Có | 
| 1 | {5} | Không | 
| 2 | {3,5} | Không | 

Việc tìm kiếm đầu tiên phát hiện ra rằng`k=3`hoạt động. Sau đó nó xác minh rằng không`1`cũng không`2`hoạt động. Giá trị hợp lệ nhỏ nhất là`3`. 

Ví dụ này thể hiện tính chất đơn điệu của vị ngữ. Một lần`k=3`trở nên hợp lệ, tất cả các giá trị lớn hơn vẫn hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 4
segments = [1,1], [4,4]
updates = 2,3
```Sự phát triển của trạng thái: 

| k | Vị trí hoạt động | Đoạn [1,1] | Đoạn [4,4] | Tồn tại? | 
| --- | --- | --- | --- | --- | 
| 1 | {2} | 0 cái | 0 cái | Không | 
| 2 | {2,3} | 0 cái | 0 cái | Không | 

Ngay cả sau khi thực hiện tất cả các cập nhật, không vị trí được giám sát nào trở thành`1`. 

sơ bộ`check(q)`thất bại, vì vậy câu trả lời là`-1`. 

Ví dụ này cho thấy lý do tại sao thuật toán thực hiện kiểm tra toàn bộ trước khi bắt đầu tìm kiếm nhị phân. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log q) | Mỗi đánh giá vị ngữ quét mảng và tất cả các phân đoạn, tìm kiếm nhị phân thực hiện đánh giá O(log q) | 
| Không gian | O(n) | Mảng cho trạng thái kích hoạt và tổng tiền tố | 

Tổng số tiền của`n`trên nhiều trường hợp thử nghiệm là nhiều nhất`10^5`. Mỗi phép lặp tìm kiếm nhị phân thực hiện công việc tuyến tính trong`n + m`và có nhiều nhất khoảng 17 lần lặp vì`q ≤ 10^5`. Điều này thoải mái phù hợp trong giới hạn thời gian và chỉ sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from io import StringIO

    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = StringIO(inp)
    sys.stdout = StringIO()

    def solve():
        input = sys.stdin.readline

        t = int(input())

        for _ in range(t):
            n, m = map(int, input().split())

            segments = [tuple(map(int, input().split())) for _ in range(m)]

            q = int(input())
            queries = [int(input()) for _ in range(q)]

            def check(k):
                arr = [0] * (n + 1)

                for i in range(k):
                    arr[queries[i]] = 1

                pref = [0] * (n + 1)

                for i in range(1, n + 1):
                    pref[i] = pref[i - 1] + arr[i]

                for l, r in segments:
                    ones = pref[r] - pref[l - 1]
                    if ones * 2 > (r - l + 1):
                        return True

                return False

            if not check(q):
                print(-1)
                continue

            lo, hi = 1, q
            ans = q

            while lo <= hi:
                mid = (lo + hi) // 2

                if check(mid):
                    ans = mid
                    hi = mid - 1
                else:
                    lo = mid + 1

            print(ans)

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run(
"""6
5 5
1 2
4 5
1 5
1 3
2 4
5
5
3
1
2
4
4 2
1 1
4 4
2
2
3
5 2
1 5
1 5
4
2
1
3
4
5 2
1 5
1 3
5
4
1
2
3
5
5 5
1 5
1 5
1 5
1 5
1 4
3
1
4
3
3 2
2 2
1 3
3
2
3
1
"""
) == """3
-1
3
3
3
1
"""

# minimum size
assert run(
"""1
1 1
1 1
1
1
"""
) == """1
"""

# never becomes beautiful
assert run(
"""1
4 2
1 1
4 4
2
2
3
"""
) == """-1
"""

# strict inequality check
assert run(
"""1
4 1
1 4
2
1
2
"""
) == """-1
"""

# answer occurs at final update
assert run(
"""1
3 1
1 3
3
1
2
3
"""
) == """2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phân đoạn phần tử đơn | 1 | Hạn chế tối thiểu | 
| Vị trí giám sát chưa được chạm tới | -1 | Chưa từng có đoạn đẹp nào xuất hiện | 
| Dài 4 có đúng 2 cái | -1 | Hoàn toàn lớn hơn, không lớn hơn hoặc bằng | 
| Đoạn [1,3] được kích hoạt dần dần | 2 | Bản cập nhật hợp lệ sớm nhất được tìm thấy chính xác | 

## Vỏ cạnh 

Xét một đoạn có độ dài chẵn.```
1
4 1
1 4
2
1
2
```Sau cả hai lần cập nhật, phân đoạn chứa hai số một và hai số không. Thuật toán tính toán:```
ones = 2
length = 4
ones * 2 = 4
```Từ`4 > 4`là sai, đoạn đó không đẹp. Đầu ra là`-1`. Điều này xử lý chính xác yêu cầu bất bình đẳng nghiêm ngặt. 

Hãy xem xét một phân khúc vị trí duy nhất.```
1
1 1
1 1
1
1
```Vì`k = 1`, đoạn này chứa một số một và số không. Séc trở thành:```
1 * 2 > 1
```điều này đúng, vì vậy tìm kiếm nhị phân trả về`1`. 

Hãy xem xét trường hợp không có phân đoạn nào bị ảnh hưởng.```
1
4 2
1 1
4 4
2
2
3
```Ngay cả sau tất cả các bản cập nhật, cả hai phân đoạn được theo dõi đều không có phân đoạn nào. Cuộc gọi sơ bộ`check(q)`trả về sai và thuật toán ngay lập tức đưa ra`-1`. 

Cuối cùng, hãy xem xét các phân đoạn lớn lặp đi lặp lại:```
n = 5
segments = [1,5], [1,5], [1,5]
updates = 2,1,3,4
```Thuật toán kiểm tra từng phân đoạn một cách độc lập. Các phân đoạn trùng lặp không thay đổi tính chính xác vì mỗi lần kiểm tra đều sử dụng cùng một truy vấn tổng tiền tố. Ngay khi một bản sao trở nên đẹp, vị từ sẽ trả về true.
