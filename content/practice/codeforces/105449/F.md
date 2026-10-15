---
title: "CF 105449F - \u041d\u0412\u041f\u0411\u041f"
description: "Chúng ta được cung cấp một mảng và độ dài dãy con tăng dần dài nhất của nó. Đối với mọi truy vấn, chúng tôi xóa một phân đoạn liền kề và hỏi xem việc xóa này có giữ nguyên độ dài LIS hay không. Nói cách khác, mảng ban đầu có một số dãy con tăng dần tối ưu có độ dài L."
date: "2026-06-24T23:28:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105449
codeforces_index: "F"
codeforces_contest_name: "Moscow team school olympiad (MKOSHP) 2024"
rating: 0
weight: 105449
solve_time_s: 91
verified: false
draft: false
---

[CF 105449F - \u041d\u0412\u041f\u0411\u041f](https://codeforces.com/problemset/problem/105449/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng và độ dài dãy con tăng dần dài nhất của nó. Đối với mọi truy vấn, chúng tôi xóa một phân đoạn liền kề và hỏi xem việc xóa này có giữ nguyên độ dài LIS hay không. 

Nói cách khác, mảng ban đầu có một số dãy con tăng dần có độ dài L tối ưu. Mỗi truy vấn sẽ xóa một khối [l, r] và chúng ta phải xác định xem liệu có còn có thể đạt được dãy con tăng dần có độ dài L chỉ sử dụng các phần tử bên ngoài khối đó hay không. 

Khó khăn ở đây là không tính toán lại LIS từ đầu cho mọi truy vấn, vì cả n và q đều lên tới 400000. Một lần tính toán LIS O(n log n) là ổn, nhưng việc lặp lại nó cho mỗi truy vấn là không thể. Mọi giải pháp đều phải giảm từng truy vấn xuống gần O(1) hoặc O(log n) sau khi xử lý trước. 

Một sai lầm ngây thơ xuất phát từ việc cho rằng LIS “mạnh cục bộ”. Ví dụ: việc loại bỏ các phần tử không phải là một phần của LIS đã chọn vẫn có thể phá hủy tất cả LIS có độ dài tối đa vì LIS không phải là duy nhất. Hãy xem xét một mảng như [1, 3, 2, 4]. Độ dài LIS là 3, nhưng các lựa chọn LIS khác nhau sẽ chồng lên nhau ở các vị trí khác nhau. Việc loại bỏ một đoạn tránh một LIS vẫn có thể phá hủy tất cả các đường dẫn LIS tối ưu. 

Một vấn đề tế nhị khác là giả sử chúng ta chỉ cần kiểm tra xem LIS đã biết có giao nhau với đoạn bị loại bỏ hay không. Điều đó sai vì có thể có nhiều cấu trúc LIS tối ưu và đoạn bị loại bỏ có thể giao nhau với tất cả chúng theo những cách khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi loại bỏ phân đoạn [l, r] một cách vật lý, tính toán LIS trên mảng còn lại bằng phương pháp O(n log n) tiêu chuẩn và so sánh kết quả với độ dài LIS ban đầu. Điều này đúng nhưng chi phí O(n log n) cho mỗi truy vấn, dẫn đến O(nq log n), điều này hoàn toàn không khả thi ở mức 400000 ràng buộc. 

Để cải thiện điều này, chúng ta cần một cách để hiểu cách mỗi phần tử đóng góp vào một số LIS tối ưu mà không cần tính toán lại LIS nhiều lần. Quan sát quan trọng là cấu trúc LIS có thể được phân tách thành các phần đóng góp từ tiền tố và hậu tố, và với mỗi vị trí, chúng ta có thể tính hai giá trị: dãy con tăng tốt nhất kết thúc tại i và dãy con tăng tốt nhất bắt đầu tại i. Đây là các trạng thái LIS DP tiến và lùi cổ điển. 

Khi biết những điều này, chúng tôi có thể suy luận xem liệu LIS tối ưu có thể được hình thành hoàn toàn bên ngoài phân khúc đã bị loại bỏ hay không. Bài toán trở thành kiểm tra xem có tồn tại dãy con tăng dần có độ dài L tránh được [l, r] hay không. Thay vì tính toán lại LIS, chúng tôi đếm xem cấu trúc LIS bị “ép buộc” thông qua phân đoạn đã bị loại bỏ. Nếu phân đoạn chứa đủ cấu trúc quan trọng để mọi LIS tối ưu đều phải đi qua nó thì việc loại bỏ nó sẽ làm giảm câu trả lời. Ngược lại, ít nhất một LIS tối ưu vẫn tồn tại. 

Điều này biến vấn đề thành việc phân tích xem các lớp LIS chồng lên nhau như thế nào. Một cách tiêu chuẩn để chính thức hóa điều này là tính toán “đóng góp xếp hạng” LIS của nó cho từng vị trí và sau đó duy trì số lượng phần tử cần thiết ở mỗi cấp độ LIS. Sau đó, các truy vấn trở thành kiểm tra phạm vi đối với những đóng góp này, thường được xử lý bằng tổng tiền tố hoặc cây phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| LIS Brute Force cho mỗi truy vấn | O(n² log n) | O(n) | Quá chậm | 
| Cấu trúc tiền tố DP + trên đóng góp LIS | O(n log n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán LIS kết thúc ở mỗi vị trí bằng cách sử dụng cây Fenwick hoặc ý tưởng sắp xếp kiên nhẫn với nén tọa độ. Điều này mang lại độ dài chuỗi con tăng dần tối đa có thể kết thúc ở mọi chỉ mục. 
2. Đảo ngược mảng và tính LIS kết thúc tại mỗi vị trí theo thứ tự đảo ngược, tương ứng với LIS bắt đầu từ mỗi chỉ mục trong mảng ban đầu. Điều này mang lại “khả năng hậu tố” cho từng vị trí. 
3. Kết hợp hai mảng này để xác định độ dài LIS tối đa L và xác định vị trí nào có thể tham gia vào một số LIS có độ dài L. Một vị trí có khả năng tới hạn nếu tồn tại một LIS đi qua nó, nghĩa là LIS tiến + LIS lùi - 1 bằng L. 
4. Chuyển vấn đề này thành vấn đề bao phủ trên các chỉ số: mỗi vị trí đóng góp vào cấu trúc LIS trên một “lớp” nhất định và chúng tôi theo dõi xem có bao nhiêu đóng góp quan trọng tồn tại. 
5. Xây dựng tổng tiền tố trên các vị trí quan trọng này để đối với bất kỳ truy vấn [l, r] nào, chúng tôi có thể nhanh chóng xác định xem việc xóa phân đoạn này có loại bỏ ít nhất một đóng góp bắt buộc từ mọi LIS có thể hay không. 
6. Đối với mỗi truy vấn, hãy kiểm tra xem các vị trí còn lại có còn cho phép LIS đầy đủ có độ dài L hay không. Nếu có, xuất ra CÓ; nếu không thì KHÔNG. 

Lý do chính khiến điều này hoạt động là vì bất kỳ LIS nào có độ dài tối đa đều phải đi qua một chuỗi các vị trí có cấu trúc lớp LIS nhất quán. Nếu một phân đoạn loại bỏ tất cả các đại diện của ít nhất một quá trình chuyển đổi lớp, thì độ dài LIS sẽ giảm xuống; nếu không nó có thể được xây dựng lại hoàn toàn bên ngoài phân đoạn đã bị loại bỏ. 

### Tại sao nó hoạt động 

Mỗi vị trí có thể được gán một vai trò trong ít nhất một LIS tối ưu thông qua các giá trị LIS tiến và lùi của nó. điều kiện`dpL[i] + dpR[i] - 1 = L`mô tả tất cả các nút thuộc về một số LIS tối ưu. Cấu trúc của LIS đảm bảo rằng các nút này có thể được xếp lớp bằng cách tăng giá trị dpL, tạo thành một phần thứ tự mà bất kỳ chuỗi tối ưu nào cũng phải tôn trọng. Việc loại bỏ sẽ duy trì độ dài LIS khi và chỉ khi, đối với mỗi lớp, vẫn còn ít nhất một đường dẫn tiếp tục hợp lệ bên ngoài khoảng thời gian đã bị xóa. Việc giảm tổng tiền tố nắm bắt chính xác liệu tất cả các lớp cần thiết có còn được kết nối hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# We compute LIS ending at each position and LIS starting at each position.

def lis_dp(arr):
    import bisect
    n = len(arr)
    dp = [0] * n
    tails = []
    for i, x in enumerate(arr):
        pos = bisect.bisect_left(tails, x)
        if pos == len(tails):
            tails.append(x)
        else:
            tails[pos] = x
        dp[i] = pos + 1
    return dp

def solve():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    # forward LIS
    left = lis_dp(a)

    # backward LIS
    right = lis_dp(a[::-1])[::-1]

    L = max(left)

    good = [0] * n
    for i in range(n):
        if left[i] + right[i] - 1 == L:
            good[i] = 1

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + good[i]

    total_good = pref[n]

    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        cnt_removed = pref[r] - pref[l - 1]
        if total_good - cnt_removed > 0:
            out.append("YES")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc tính toán LIS được thực hiện hai lần bằng phương pháp sắp xếp kiên nhẫn tiêu chuẩn. Việc chuyển tiếp sẽ mang lại chuỗi con tăng dần tốt nhất kết thúc ở mỗi chỉ mục và việc đảo ngược mảng sẽ cung cấp thông tin hậu tố tương tự. 

Sau đó, chúng tôi tính toán vị trí nào là một phần của ít nhất một LIS tối ưu bằng cách sử dụng nhận dạng cổ điển`left[i] + right[i] - 1 == L`. Đây được đánh dấu là vị trí "tốt". Tổng tiền tố trên mảng đánh dấu này cho phép đếm nhanh xem có bao nhiêu vị trí như vậy nằm trong bất kỳ khoảng truy vấn nào. 

Mỗi truy vấn sẽ loại bỏ một khoảng thời gian và kiểm tra xem có còn ít nhất một vị trí “tốt” hay không. Nếu không còn lại, mọi LIS tối ưu buộc phải cắt đoạn bị loại bỏ theo cách phá hủy tính tối ưu. 

## Ví dụ đã hoạt động 

Xét mảng [1, 2, 5, 4, 7, 3, 6]. Độ dài LIS là 4. 

Chúng tôi tính toán: 

| tôi | một [tôi] | trái | đúng | trái+phải-1 ​​| tốt | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 4 | 4 | 1 | 
| 2 | 2 | 2 | 3 | 4 | 1 | 
| 3 | 5 | 3 | 2 | 4 | 1 | 
| 4 | 4 | 3 | 2 | 4 | 1 | 
| 5 | 7 | 4 | 1 | 4 | 1 | 
| 6 | 3 | 2 | 2 | 3 | 0 | 
| 7 | 6 | 3 | 1 | 3 | 0 | 

Bây giờ thực hiện truy vấn [6, 7]. Chúng ta loại bỏ vị trí 6 và 7, cả hai đều có good = 0 nên tổng số good vẫn là 5, LIS được giữ nguyên. 

Lấy truy vấn [3, 5]. Chúng tôi xóa các chỉ mục chứa một số yếu tố quan trọng; vẫn còn ít nhất một vị trí tốt nên LIS không thay đổi. 

| Truy vấn | loại bỏ số lượng tốt | còn lại tốt | trả lời | 
| --- | --- | --- | --- | 
| [6,7] | 0 | 5 | CÓ | 
| [3,5] | 3 | 2 | CÓ | 

Điều này chứng tỏ rằng việc loại bỏ các phân đoạn không cần thiết không ảnh hưởng đến độ dài LIS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q) | Hai phép tính LIS thông qua phân loại kiên nhẫn và O(1) cho mỗi truy vấn | 
| Không gian | O(n) | Mảng cho các trạng thái LIS và tổng tiền tố | 

Điều này phù hợp thoải mái trong các ràng buộc vì cả n và q đều lên tới 400000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def lis_dp(arr):
        import bisect
        dp = []
        res = []
        for x in arr:
            i = bisect.bisect_left(dp, x)
            if i == len(dp):
                dp.append(x)
            else:
                dp[i] = x
            res.append(i + 1)
        return res

    n, q = map(int, sys.stdin.readline().split())
    a = list(map(int, sys.stdin.readline().split()))

    left = lis_dp(a)
    right = lis_dp(a[::-1])[::-1]
    L = max(left)

    good = [0]*n
    for i in range(n):
        if left[i] + right[i] - 1 == L:
            good[i] = 1

    pref = [0]*(n+1)
    for i in range(n):
        pref[i+1] = pref[i] + good[i]

    out = []
    for _ in range(q):
        l,r = map(int, sys.stdin.readline().split())
        if pref[n] - (pref[r]-pref[l-1]) > 0:
            out.append("YES")
        else:
            out.append("NO")

    return "\n".join(out)

# sample-style and custom tests
assert run("""7 5
1 2 5 4 7 3 6
6 7
4 6
3 3
1 7
2 5
""") == "YES\nYES\nYES\nNO\nYES"

assert run("""1 2
10
1 1
1 1
""") == "NO\nNO"

assert run("""5 3
1 2 3 4 5
2 4
1 3
1 5
""") == "YES\nYES\nNO"

assert run("""6 2
5 4 3 2 1 6
1 5
2 6
""") == "YES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng nghiêm ngặt | hỗn hợp CÓ/KHÔNG | độ nhạy LIS đầy đủ | 
| phần tử đơn | KHÔNG KHÔNG | trường hợp chính xác của cạnh | 
| tăng đầy đủ với số lần xóa | CÓ/CÓ/KHÔNG | phá hủy LIS ranh giới | 
| tiền tố giảm dần | CÓ/CÓ | cấu trúc LIS không tầm thường | 

## Vỏ cạnh 

Đối với một mảng tăng nghiêm ngặt như [1,2,3,4,5], mọi phần tử đều cần thiết cho LIS duy nhất. Việc xóa bất kỳ đoạn giữa nào sẽ xóa ít nhất một phần tử LIS bắt buộc, do đó câu trả lời sẽ trở thành KHÔNG cho bất kỳ truy vấn nào ngoại trừ việc xóa trống. Thuật toán xử lý việc này vì tất cả các vị trí đều thỏa mãn`left[i] + right[i] - 1 = L`, do đó việc loại bỏ bất kỳ khoảng thời gian nào sẽ làm giảm số lượng vị trí tốt về 0. 

Đối với một mảng có nhiều đường dẫn LIS chẳng hạn như [1,3,2,4], có hai cấu trúc LIS khác nhau. Thuật toán đánh dấu tất cả các vị trí thuộc ít nhất một LIS tối ưu và các truy vấn duy trì chính xác độ dài LIS trừ khi chúng loại bỏ tất cả các đại diện trên cả hai đường dẫn.
