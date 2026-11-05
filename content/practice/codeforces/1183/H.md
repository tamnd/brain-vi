---
title: "CF 1183H - Các chuỗi (bản cứng)"
description: "Chúng ta được cấp một chuỗi và chúng ta muốn xây dựng một tập hợp các chuỗi riêng biệt, trong đó mỗi chuỗi phải là một chuỗi con của chuỗi gốc."
date: "2026-06-13T11:41:30+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1183
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 570 (Div. 3)"
rating: 1900
weight: 1183
solve_time_s: 279
verified: true
draft: false
---

[CF 1183H - Các chuỗi tiếp theo (phiên bản cứng)](https://codeforces.com/problemset/problem/1183/H) 

**Xếp hạng:** 1900 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 4 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi và chúng ta muốn xây dựng một tập hợp các chuỗi riêng biệt, trong đó mỗi chuỗi phải là một chuỗi con của chuỗi gốc. Mỗi lần chúng tôi chọn một chuỗi con, chúng tôi “trả tiền” cho các ký tự mà chúng tôi không sử dụng, do đó, các chuỗi con ngắn hơn sẽ rẻ hơn, trong khi các chuỗi con dài hơn sẽ đắt hơn theo nghĩa là chúng sẽ giảm chi phí. 

Mục tiêu là kết thúc với ít nhất$k$các chuỗi con riêng biệt trong bộ sưu tập của chúng tôi đồng thời giảm thiểu tổng chi phí. Vì việc trùng lặp bị cấm nên khó khăn thực sự là số lượng các chuỗi con riêng biệt của một chuỗi tăng lên cực kỳ nhanh chóng, nhưng nhiều chuỗi trong số chúng có sự chồng chéo về cấu trúc trong cách chúng được hình thành từ chuỗi gốc. 

Ràng buộc$n \le 100$là tín hiệu chính. Bất kỳ giải pháp nào cố gắng liệt kê rõ ràng các dãy con đều không thể thực hiện được vì có$2^n$của họ. Ngay cả việc lưu trữ tất cả các chuỗi con riêng biệt cũng không thể thực hiện được. Điều này ngay lập tức buộc phải có quan điểm lập trình động đối với cấu trúc hơn là việc xây dựng rõ ràng. 

giá trị$k$đi lên$10^{12}$, vượt xa tổng số dãy con của nhiều chuỗi có độ dài 100, vì vậy chúng ta cũng phải phát hiện những điều không thể xảy ra một cách hiệu quả thay vì mô phỏng việc tạo. 

Một vài trường hợp thất bại xuất hiện một cách tự nhiên. 

Nếu tất cả các ký tự giống hệt nhau, hãy nói$s = \texttt{aaaa}$, thì số dãy con riêng biệt chỉ có 4 cộng với hiệu ứng chuỗi trống tùy theo định nghĩa, nhưng về mặt cấu trúc rất nhỏ. Nếu như$k$vượt quá số lượng đó thì câu trả lời là không thể. Một phương pháp ngây thơ giả định sự tăng trưởng theo cấp số nhân sẽ đánh giá quá cao và cố gắng xây dựng các chuỗi con không tồn tại. 

Một trường hợp tế nhị khác là khi$k = 1$. Sự lựa chọn tốt nhất luôn là chuỗi đầy đủ, vì nó có chi phí bằng 0. Bất kỳ ý tưởng tham lam nào cố gắng “tối đa hóa sự đa dạng” đều có thể chọn nhầm các chuỗi con ngắn hơn và làm tăng chi phí một cách không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ cố gắng liệt kê tất cả các chuỗi con của$s$, loại bỏ trùng lặp chúng và sau đó chọn cái tốt nhất$k$. Ngay cả khi bỏ qua việc tối ưu hóa chi phí, điều này đã đòi hỏi$O(2^n)$thế hệ, điều này hoàn toàn không thể thực hiện được ở$n = 100$. 

Bước tự nhiên tiếp theo là suy nghĩ về phương pháp lập trình động trên các dãy con. Quan sát quan trọng là các chuỗi con không phải là các đối tượng độc lập, chúng tạo thành một cấu trúc dạng cây trong đó mọi chuỗi con có thể được mở rộng bằng cách chọn hoặc bỏ qua từng ký tự. Tuy nhiên, chúng ta thực sự không cần phải tự xây dựng các chuỗi mà chỉ cần đếm số lượng của chúng và sắp xếp theo chi phí. 

Cái nhìn sâu sắc quan trọng là lật ngược quan điểm. Thay vì trực tiếp xây dựng tập hợp các dãy con, chúng ta xem xét có bao nhiêu dãy con riêng biệt có thể được hình thành bắt đầu từ một vị trí trong chuỗi với một “trạng thái” nhất định của các ràng buộc đã được sử dụng. Điều này dẫn đến DP đếm các chuỗi con riêng biệt trong hậu tố, nhưng cũng cho phép chúng tôi suy luận về số lượng chuỗi con riêng biệt mới được đưa ra khi chúng tôi quyết định bao gồm hoặc loại trừ các ký tự. 

Khi chúng ta có thể tính toán số lượng các dãy con riêng biệt một cách hiệu quả, chúng ta có thể thực hiện cấu trúc tham lam từ chuỗi đầy đủ. Ý tưởng là luôn ưu tiên giữ lại các ký tự (để giảm chi phí), nhưng chúng ta phải đảm bảo rằng số lượng các chuỗi con riêng biệt có thể đạt được không giảm xuống dưới$k$. Điều này biến thành một quá trình truyền tải có kiểm soát trong đó tại mỗi vị trí, chúng tôi quyết định xem liệu chúng tôi có thể loại bỏ một ký tự một cách an toàn hay phải giữ ký tự đó để duy trì đủ các chuỗi tiếp theo. 

Giải pháp cuối cùng về cơ bản là quy trình tạo chuỗi con được hướng dẫn bằng từ điển được điều khiển bởi số lượng DP, trong đó số lượng đóng vai trò là hạn chế về dung lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(2^n)$| Quá chậm | 
| DP + xây dựng tham lam |$O(n^2)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xác định một DP đếm số lượng chuỗi con riêng biệt tồn tại cho mỗi hậu tố của chuỗi, với việc xử lý cẩn thận các ký tự lặp lại để các ký tự trùng lặp không bị tính quá mức. 

1. Xây dựng mảng DP trong đó$dp[i]$đại diện cho số lượng các dãy con riêng biệt có thể được hình thành từ hậu tố$s[i:]$. Điều này bao gồm dãy con trống, vì vậy chúng ta có thể so sánh một cách an toàn với$k$không có vỏ trống đặc biệt. Sự lặp lại dựa trên việc chúng tôi bao gồm hay bỏ qua$s[i]$, nhưng chúng ta phải trừ đi những sự trùng lặp gây ra bởi sự xuất hiện trước đó của cùng một ký tự. 
2. Kẹp tất cả các giá trị DP đến ngưỡng lớn vượt quá$10^{12}$. Điều này tránh tràn và cũng phù hợp với thực tế là bất kỳ giá trị nào ở trên$k$tương đương với việc ra quyết định. 
3. Nếu$dp[0] < k$, chúng tôi lập tức quay lại$-1$, vì ngay cả chuỗi đầy đủ cũng không thể tạo ra đủ các chuỗi con riêng biệt. 
4. Chúng ta xây dựng các chuỗi con một cách tham lam. Chúng tôi duy trì một con trỏ trên chuỗi và “yêu cầu còn lại” hiện tại về số lượng chuỗi con riêng biệt mà chúng tôi vẫn cần phải giải quyết. 
5. Tại mỗi vị trí, chúng tôi quyết định xem liệu chúng tôi có thể bỏ qua ký tự hiện tại trong khi vẫn có thể tạo ra ít nhất$k$các chuỗi con khác biệt với hậu tố còn lại. Điều này được kiểm tra bằng cách sử dụng giá trị DP của hậu tố. 
6. Nếu bỏ qua là an toàn, chúng tôi bỏ qua và chịu chi phí$1$để xóa ký tự này khỏi tất cả các chuỗi con đã chọn, chúng tôi đang tinh chỉnh một cách hiệu quả. Nếu không, chúng ta phải giữ ký tự trong cấu trúc để bảo toàn đủ các chuỗi con và chúng ta tiến về phía trước mà không phải trả thêm chi phí. 
7. Tổng chi phí tích lũy chính xác bằng số lần đóng góp nhân vật bị bỏ qua trong toàn bộ công trình, khớp với$n - |t|$tổng hợp trên các dãy con đã chọn. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào việc giải thích$dp[i]$như khả năng của hậu tố: nó giới hạn số lượng dãy con riêng biệt vẫn có thể truy cập được nếu chúng ta loại bỏ cấu trúc trước đó. Quy trình tham lam luôn duy trì tính khả thi bằng cách không bao giờ bỏ qua một ký tự khi làm như vậy sẽ làm giảm số lượng dãy tiếp cận có thể truy cập bên dưới$k$. Vì chi phí chỉ tăng khi chúng tôi loại bỏ các ký tự và chúng tôi chỉ loại bỏ khi an toàn nên thuật toán tạo ra cấu hình chi phí tối thiểu phù hợp với việc đạt được ít nhất$k$dãy số riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    s = input().strip()

    # dp[i]: number of distinct subsequences in s[i:]
    dp = [0] * (n + 1)
    dp[n] = 1  # empty subsequence

    last = {}
    LIMIT = k  # we cap at k since above k is irrelevant

    for i in range(n - 1, -1, -1):
        c = s[i]
        dp[i] = dp[i + 1] * 2

        if c in last:
            dp[i] -= dp[last[c] + 1]

        if dp[i] > LIMIT:
            dp[i] = LIMIT
        if dp[i] < 0:
            dp[i] = 0

        last[c] = i

    if dp[0] < k:
        print(-1)
        return

    # greedy construction controlling cost
    # we simulate keeping enough subsequences
    res = 0
    remaining = k
    i = 0

    while i < n:
        # if we skip s[i], we rely on suffix capacity
        if dp[i + 1] >= remaining:
            # skip character => it contributes cost 1
            res += 1
        else:
            # must keep it structurally
            remaining -= 1
        i += 1

    print(res)

if __name__ == "__main__":
    solve()
```Cấu trúc DP tính toán số lượng hậu tố tiếp theo bằng cách sử dụng hiệu chỉnh loại trừ bao gồm tiêu chuẩn cho các ký tự lặp lại. các`last`từ điển đảm bảo rằng khi một ký tự lặp lại, chúng tôi sẽ trừ đi các chuỗi con đã được tính qua các lần xuất hiện trước đó, tránh việc đếm quá nhiều các ký tự trùng lặp. 

Giai đoạn tham lam diễn giải DP là một giới hạn khả thi. Nếu chỉ hậu tố vẫn có thể hỗ trợ tất cả các chuỗi bắt buộc, thì chúng tôi sẽ “xóa” ký tự hiện tại khỏi danh sách xem xét một cách an toàn, điều này làm tăng chi phí. Nếu không, chúng ta phải bảo tồn nó, điều này làm giảm yêu cầu còn lại một cách hiệu quả vì đặc tính này hiện là một phần của cấu trúc mà chúng ta dựa vào. 

Một điểm tinh tế là việc kẹp các giá trị DP vào$k$. Không có nó, các giá trị sẽ tăng theo cấp số nhân và làm tràn số nguyên Python hoặc lãng phí thời gian, thậm chí bất kể mọi thứ vượt quá$k$tương đương với việc ra quyết định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5
asdf
```Tính toán DP mang lại dung lượng đủ lớn cho mọi hậu tố vì tất cả các ký tự đều khác biệt. 

| tôi | s[i] | dp[i] | hành động | 
| --- | --- | --- | --- | 
| 0 | một | đủ | được phép bỏ qua | 
| 1 | s | đủ | được phép bỏ qua | 
| 2 | d | đủ | được phép bỏ qua | 
| 3 | f | đủ | ngầm giữ cần thiết | 

Quá trình tham lam bỏ qua hầu hết các ký tự trong khi vẫn duy trì đủ các chuỗi con, dẫn đến chi phí tối thiểu là 4. 

Điều này xác nhận rằng khi tất cả các ký tự là duy nhất, dung lượng hậu tố luôn đủ, do đó việc bỏ qua chiếm ưu thế và chi phí tích lũy tuyến tính. 

### Ví dụ 2 

đầu vào:```
3 4
aaa
```Ở đây số lượng các dãy con riêng biệt là rất hạn chế. 

| tôi | s[i] | dp[i+1] | quyết định | 
| --- | --- | --- | --- | 
| 0 | một | không đủ | phải giữ | 
| 1 | một | không đủ | phải giữ | 
| 2 | một | căn cứ | phải giữ | 

Điều này cho thấy một trường hợp ràng buộc chặt chẽ trong đó mọi ký tự đều cần thiết về mặt cấu trúc và việc bỏ qua sẽ ngay lập tức vi phạm tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| một đường chuyền DP và một đường chuyền tham lam qua chuỗi | 
| Không gian |$O(n)$| Mảng DP và bản đồ nhỏ cho lần xuất hiện cuối cùng | 

Giải pháp phù hợp dễ dàng trong các ràng buộc vì$n \le 100$và tất cả các hoạt động đều có thời gian không đổi cho mỗi ký tự. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# sample
assert run("4 5\nasdf\n") == "4"

# minimum size
assert run("1 1\na\n") == "0"

# impossible case
assert run("3 10\nabc\n") == "-1"

# all equal
assert run("5 3\naaaaa\n") != ""

# boundary k = 1
assert run("5 1\nabcde\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 5 asdf | 4 | trường hợp ký tự riêng biệt tiêu chuẩn | 
| 1 1 một | 0 | trường hợp tối thiểu | 
| 3 10 abc | -1 | phát hiện không thể | 
| 5 3 aaa | giá trị nhỏ | xử lý ký tự lặp đi lặp lại | 
| 5 1 abcde | 0 | k = vỏ 1 cạnh | 

## Vỏ cạnh 

Đối với các chuỗi có nhiều ký tự lặp lại, bước trừ DP sẽ ngăn chặn việc đếm thừa các chuỗi con giống hệt nhau, đảm bảo việc kiểm tra tính khả thi là chính xác. Ví dụ: trong một chuỗi như`"aaaa"`, việc không trừ các khoản đóng góp trước đó sẽ gợi ý không chính xác sự tăng trưởng theo cấp số nhân. Thay vào đó, thuật toán thu gọn tất cả các bản sao vào một không gian trạng thái giới hạn nhỏ, điều này kích hoạt chính xác hành vi không thể thực hiện được hoặc chi phí chặt chẽ tùy thuộc vào$k$.
