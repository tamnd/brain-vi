---
title: "CF 106339F - Đường Frosted (Cứng)"
description: "Chúng ta có một mảng được lập chỉ mục từ 1 đến n, trong đó mỗi vị trí i chứa một giá trị h[i]. Bên cạnh mảng này, có nhiều truy vấn."
date: "2026-06-19T08:51:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106339
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 1-28-2026"
rating: 0
weight: 106339
solve_time_s: 51
verified: true
draft: false
---

[CF 106339F - Đường cao tốc băng giá (Cứng)](https://codeforces.com/problemset/problem/106339/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng được lập chỉ mục từ 1 đến n, trong đó mỗi vị trí i chứa một giá trị h[i]. Bên cạnh mảng này, có nhiều truy vấn. Mỗi truy vấn đưa ra một phân đoạn [L, R] và một giá trị K, đồng thời yêu cầu chúng ta đếm xem có bao nhiêu chỉ số i trong phân đoạn này thỏa mãn điều kiện chia hết cho cả i và h[i]. 

Điều kiện là gcd(h[i], i) chia hết cho K. Điều này mạnh hơn việc chỉ kiểm tra đẳng thức hoặc ngưỡng gcd đơn giản, vì nó liên kết cấu trúc của chỉ mục và giá trị được lưu trữ tại chỉ mục đó. 

Hệ quả cấu trúc quan trọng xuất phát từ bản chất của gcd. Nếu K chia gcd(h[i], i), thì K phải chia cả h[i] và i riêng lẻ. Điều này biến đổi những gì ban đầu trông giống như một điều kiện lý thuyết số thành một vấn đề lọc tính chia hết thuần túy trên các chỉ số. 

Các ràng buộc trong các vấn đề thuộc loại này thường đẩy n lên khoảng 10^5 hoặc hơn với số lượng truy vấn tương tự. Điều đó ngay lập tức loại trừ việc tính toán lại gcd cho mọi chỉ mục cho mỗi truy vấn, vì đó sẽ là O(n) cho mỗi truy vấn và sẽ bùng nổ thành 10^10 thao tác trong trường hợp xấu nhất. Ngay cả một giải pháp O(n log n) đầy đủ cho mỗi truy vấn cũng sẽ quá chậm. 

Khó khăn tinh tế là điều kiện phụ thuộc vào cả chỉ mục và giá trị mảng, do đó, việc tính toán trước một cái gì đó thuần túy về h[i] là không đủ. Chúng ta phải hỗ trợ việc đếm phạm vi nhanh trên một tập hợp con các chỉ số được xác định động cho nhiều giá trị K khác nhau. 

Việc triển khai đơn giản cũng có thể bỏ lỡ một trường hợp quan trọng: K lớn hơn n hoặc lớn hơn tất cả h[i]. Trong trường hợp đó, câu trả lời luôn bằng 0, vì không có chỉ mục nào có thể thỏa mãn i chia hết cho K. Ví dụ: nếu n = 5, h = [2, 4, 6, 8, 10] và K = 7, mọi truy vấn đều phải trả về 0. Bất kỳ cách tiếp cận nào quét một cách mù quáng hoặc lưu trữ danh sách chia hết không đầy đủ mà không xử lý giới hạn chính xác vẫn có thể hoạt động chính xác, nhưng quá trình tiền xử lý không hiệu quả có thể lãng phí thời gian lặp lại các giá trị không cần thiết. 

Một trường hợp lỗi khác xuất hiện khi K = 1. Ở đây mọi chỉ số đều thỏa mãn điều kiện vì gcd(h[i], i) luôn chia hết cho 1. Một giải pháp đúng sẽ xử lý vấn đề này một cách tự nhiên mà không cần viết hoa đặc biệt, nhưng các vòng tiền xử lý không chính xác chỉ lặp bội số bắt đầu từ K có thể bỏ qua hoặc xử lý sai K = 1 nếu không được thiết kế cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi lặp lại tất cả các chỉ mục i trong [L, R], tính gcd(h[i], i) và kiểm tra xem nó có chia hết cho K hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, nó yêu cầu tính toán gcd cho mỗi chỉ mục cho mỗi truy vấn, dẫn đến O(n log n) cho mỗi truy vấn, vì gcd mất thời gian logarit. Với nhiều truy vấn, điều này trở nên quá chậm. 

Nút thắt cổ chai xuất phát từ việc lặp lại các bước kiểm tra cấu trúc giống nhau trên các truy vấn. Điều kiện không phụ thuộc vào L và R một cách phức tạp; nó chỉ phụ thuộc vào việc một chỉ mục có thuộc về một tập hợp có thể tính toán trước được xác định bởi K hay không. Khi chúng ta nhận ra rằng K chia gcd(h[i], i) khi và chỉ khi K chia cả i và h[i], chúng ta có thể tách tiền xử lý khỏi truy vấn. 

Quan sát này cho phép chúng ta đảo ngược quan điểm. Thay vì kiểm tra từng truy vấn trên mảng, chúng tôi tính toán trước mọi K có thể danh sách các chỉ số i sao cho cả i và h[i] đều chia hết cho K. Mỗi danh sách như vậy được sắp xếp tự nhiên vì chúng tôi xử lý các chỉ mục theo thứ tự tăng dần. Sau đó, mỗi truy vấn sẽ giảm xuống thành một vấn đề đếm phạm vi trên một danh sách đã được sắp xếp, có thể được trả lời bằng cách sử dụng tìm kiếm nhị phân. 

Tối ưu hóa còn lại là trong quá trình tiền xử lý. Chúng ta không cần kiểm tra mọi i cho mọi K. Thay vào đó, đối với một K cố định, chúng ta chỉ lặp qua bội số của K, vì chỉ những chỉ số đó mới có thể thỏa mãn i chia hết cho K. Điều này làm giảm chi phí xây dựng chuỗi hài trên n, tức là O(n log n).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · n log n) | O(1) | Quá chậm | 
| Tối ưu | O(n log n + q log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng giải pháp bằng cách tổ chức các chỉ số theo cấu trúc chia hết. 

1. Chúng tôi tạo một cấu trúc dữ liệu, thường là một từ điển hoặc mảng vectơ, trong đó với mỗi K có thể, chúng tôi lưu trữ một danh sách các chỉ mục hợp lệ. Cấu trúc này sẽ tích lũy đáp án cho tất cả các ước số tiềm năng xuất hiện trong không gian bài toán. 
2. Chúng ta lặp K từ 1 đến n. Đối với mỗi K, chúng tôi chỉ quét bội số i của nó, nghĩa là i = K, 2K, 3K, v.v. cho đến n. Hạn chế này là hợp lệ vì mọi chỉ số không chia hết cho K không bao giờ có thể thỏa mãn điều kiện. 
3. Đối với mỗi chỉ số ứng cử viên i = mK, chúng tôi kiểm tra xem h[i] có chia hết cho K hay không. Nếu h[i] mod K bằng 0 thì cả hai điều kiện bắt buộc đều đúng, vì vậy chúng tôi nối i vào chỉ số[K]. Điều này đảm bảo chúng tôi chỉ lưu trữ các vị trí hợp lệ. 
4. Sau khi xử lý trước tất cả các giá trị K, mọi danh sách chỉ số[K] đã được sắp xếp vì chúng tôi thêm i theo thứ tự tăng dần trong quá trình quét nhiều lần. 
5. Đối với mỗi truy vấn (L, R, K), chúng tôi muốn đếm xem có bao nhiêu giá trị trong chỉ số [K] nằm trong khoảng. Vì danh sách đã được sắp xếp nên chúng tôi tìm vị trí đầu tiên >= L bằng cách sử dụng giới hạn dưới và vị trí đầu tiên > R bằng cách sử dụng giới hạn trên và trừ các vị trí của chúng để lấy số đếm. 
6. Chúng tôi xuất số lượng này cho mỗi truy vấn. 

Tại sao nó hoạt động dựa trên sự chuyển đổi tương đương trực tiếp của điều kiện. Yêu cầu K | gcd(h[i], i) tương đương với việc K chia cả hai đối số của gcd. Điều này loại bỏ hoàn toàn gcd khỏi tính toán thời gian chạy. Quá trình xử lý trước đảm bảo rằng mọi chỉ mục hợp lệ i được lưu trữ chính xác trong danh sách các giá trị K chia cả i và h[i] và không có chỉ mục không hợp lệ nào được chèn vào. Vì các chỉ mục được chèn theo thứ tự tăng dần trên mỗi K nên mỗi danh sách vẫn được sắp xếp, điều này đảm bảo tính chính xác của việc đếm phạm vi tìm kiếm nhị phân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    h = [0] + list(map(int, input().split()))

    indices = [[] for _ in range(n + 1)]

    for k in range(1, n + 1):
        for i in range(k, n + 1, k):
            if h[i] % k == 0:
                indices[k].append(i)

    for _ in range(q):
        l, r, k = map(int, input().split())
        if k > n:
            print(0)
            continue

        arr = indices[k]

        # binary search
        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] >= l:
                hi = mid
            else:
                lo = mid + 1
        left = lo

        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] > r:
                hi = mid
            else:
                lo = mid + 1
        right = lo

        print(right - left)

if __name__ == "__main__":
    solve()
```Vòng tiền xử lý là bước chuyển đổi cốt lõi. Thay vì xử lý từng chỉ mục một cách độc lập, nó nhóm các chỉ mục theo cấu trúc ước số, điều này cho phép tái sử dụng trên các truy vấn. 

Tìm kiếm nhị phân được triển khai thủ công để tránh sự phụ thuộc vào các thư viện bên ngoài và giữ toàn quyền kiểm soát các điều kiện biên. Tìm kiếm đầu tiên tìm thấy chỉ số sớm nhất không nhỏ hơn L và tìm kiếm thứ hai tìm thấy chỉ mục đầu tiên lớn hơn R. 

Một chi tiết triển khai tinh tế là phần bảo vệ`if k > n`. Vì không có chỉ mục nào i có thể thỏa mãn i chia hết cho k khi k vượt quá n, việc bỏ qua việc tra cứu sẽ tránh được những công việc không cần thiết và ngăn chặn hành vi cạnh mảng trống bị hiểu sai. 

## Ví dụ đã hoạt động 

Xét n = 6, h = [2, 3, 6, 8, 12, 9] và một truy vấn (2, 6, 2). 

Chúng tôi xây dựng chỉ số[2] bằng cách kiểm tra bội số của 2. 

| tôi | h[i] | tôi % 2 | h[i] % 2 | bao gồm | 
| --- | --- | --- | --- | --- | 
| 2 | 3 | 0 | 1 | không | 
| 4 | 8 | 0 | 0 | vâng | 
| 6 | 9 | 0 | 1 | không | 

Vậy chỉ số[2] = [4]. 

Đối với truy vấn (2, 6, 2), chúng tôi tìm kiếm các giá trị trong [4] trong khoảng từ 2 đến 6, mang lại 1 phần tử. 

Bây giờ hãy xem xét một truy vấn khác (1, 5, 1). 

Vì k = 1 chia hết mọi thứ nên mọi chỉ số đều hợp lệ vì cả i và h[i] đều chia hết cho 1. 

| tôi | được đưa vào chỉ số[1] | 
| --- | --- | 
| 1 | vâng | 
| 2 | vâng | 
| 3 | vâng | 
| 4 | vâng | 
| 5 | vâng | 
| 6 | vâng | 

Truy vấn (1, 5, 1) trả về 5, vì tất cả các chỉ số từ 1 đến 5 đều được bao gồm. 

Các ví dụ này xác nhận rằng quá trình tiền xử lý nắm bắt chính xác các ràng buộc về khả năng chia hết và phạm vi đếm trên các danh sách được sắp xếp hoạt động như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Mỗi k xử lý n/k bội số, tổng thành chuỗi hài trên n | 
| Không gian | O(n log n) | Mỗi chỉ số có thể xuất hiện trong nhiều danh sách ước số | 

Chi phí tiền xử lý nằm trong giới hạn có thể chấp nhận được vì mỗi chỉ mục chỉ được truy cập thông qua các ước số của nó và tổng số lượt truy cập như vậy được giới hạn bởi cấu trúc hài hòa của tính chia hết. Quá trình xử lý truy vấn là logarit do tìm kiếm nhị phân trên các danh sách được sắp xếp trước được tính toán trước, đủ hiệu quả cho q lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    h = [0] + list(map(int, input().split()))
    indices = [[] for _ in range(n + 1)]

    for k in range(1, n + 1):
        for i in range(k, n + 1, k):
            if h[i] % k == 0:
                indices[k].append(i)

    out = []
    for _ in range(q):
        l, r, k = map(int, input().split())
        if k > n:
            out.append("0")
            continue
        arr = indices[k]

        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] >= l:
                hi = mid
            else:
                lo = mid + 1
        left = lo

        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] > r:
                hi = mid
            else:
                lo = mid + 1
        right = lo

        out.append(str(right - left))

    return "\n".join(out)

# minimum size
assert run("1 1\n5\n1 1 1") == "1"

# all equal values
assert run("5 2\n2 2 2 2 2\n1 5 2\n1 5 3") == "2\n0"

# boundary K > n
assert run("4 1\n1 2 3 4\n1 4 10") == "0"

# single valid index
assert run("5 1\n1 2 3 4 5\n2 5 2") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1, 5, 1 1 1 | 1 | tính đúng đắn của trường hợp tối thiểu | 
| tất cả 2s | 2/0 | hành vi nhiều K | 
| K > n | 0 | xử lý số chia ngoài phạm vi | 
| hỗn hợp | 2 | lọc theo khả năng chia hết | 

## Vỏ cạnh 

Đối với K lớn hơn n, cấu trúc tiền xử lý không bao giờ chứa các chỉ mục hợp lệ, do đó các truy vấn trả về 0 một cách chính xác mà không cần quét bất kỳ mảng nào. 

Với K = 1, mọi chỉ mục đều hợp lệ vì cả i và h[i] luôn chia hết cho 1. Vòng lặp tiền xử lý tự nhiên bao gồm tất cả các chỉ mục trong chỉ mục[1], do đó các truy vấn giảm xuống một phạm vi đơn giản trên toàn bộ danh sách chỉ mục. 

Đối với các giá trị của h[i] nhỏ hơn K, việc kiểm tra tính chia hết sẽ tự động lọc chúng ra trong quá trình tiền xử lý vì h[i] % K sẽ không bằng 0. Điều này ngăn chặn việc đưa vào không chính xác ngay cả khi các chỉ số là bội số của K.
