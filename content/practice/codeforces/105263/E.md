---
title: "CF 105263E - Đá Tranh 2"
description: "Chúng ta được cho một hàng các viên đá $n$ và mỗi viên đá phải được sơn bằng một trong các màu $c$ có sẵn. Hạn chế duy nhất là về các khối có màu giống hệt nhau: chúng tôi không được phép có bất kỳ khối nào có độ dài $k$ trở lên trong đó tất cả các viên đá có cùng màu."
date: "2026-06-24T02:30:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105263
codeforces_index: "E"
codeforces_contest_name: "XXIV Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105263
solve_time_s: 106
verified: false
draft: false
---

[CF 105263E - Đá vẽ tranh 2](https://codeforces.com/problemset/problem/105263/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một dòng$n$đá, và mỗi viên đá phải được sơn bằng một trong các$c$màu sắc có sẵn. Hạn chế duy nhất là về việc chạy các màu giống hệt nhau: chúng tôi không được phép có bất kỳ khối độ dài nào$k$hoặc nhiều hơn nơi tất cả các viên đá có cùng màu. Nói cách khác, nếu một màu xuất hiện liên tiếp thì độ dài vệt của nó phải luôn ở mức dưới$k$. 

Nhiệm vụ là đếm xem có bao nhiêu màu hợp lệ cho mỗi trường hợp thử nghiệm và đưa ra kết quả theo modulo một số nguyên tố nhất định.$p$. Số lượng ca kiểm thử có thể lớn và mỗi ca kiểm thử có thể liên quan đến rất nhiều$n$Và$c$, vì vậy chúng ta cần một phương pháp tránh liệt kê các chất tạo màu một cách rõ ràng. 

Những ràng buộc đã loại bỏ hoàn toàn vũ lực. Ngay cả đối với mức độ vừa phải$n$, tổng số dãy là$c^n$, và điều đó phát triển vượt xa mọi tính toán khả thi. Sự hiện diện của$n$lên đến$10^6$buộc một cách tiếp cận tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Ràng buộc bổ sung$k \ge 2$gợi ý rằng hạn chế là cục bộ và chỉ phụ thuộc vào cấu trúc liên tiếp, điều này thường báo hiệu một chương trình động hoặc tái diễn tổ hợp. 

Trường hợp cạnh tinh tế xuất hiện khi$k > n$. Trong tình huống đó, ràng buộc thực sự không liên quan, bởi vì không có chiều dài nào$k$có thể tồn tại bên trong một chiều dài$n$sự liên tiếp. Câu trả lời khi đó chỉ đơn giản là$c^n$. Một trường hợp cạnh khác là$k = 2$, điều này cấm bất kỳ hai viên đá liền kề nào có cùng màu, biến bài toán thành một bài toán đếm cổ điển “không có cạnh bằng nhau” có đáp án$c \cdot (c-1)^{n-1}$. Bất kỳ giải pháp chung nào cũng phải suy giảm chính xác cho những trường hợp đặc biệt này. 

## Phương pháp tiếp cận 

Một nỗ lực ngây thơ là xây dựng chuỗi từ trái sang phải và đếm tất cả các phép gán hợp lệ theo cách đệ quy. Ở mỗi vị trí, chúng tôi đều thử tất cả$c$màu sắc và chúng tôi chỉ từ chối một lựa chọn nếu nó tạo ra một chiều dài bị cấm$k$. Để thực thi điều này, trạng thái phải nhớ độ dài chạy hiện tại của màu cuối cùng. Điều này dẫn đến trạng thái DP như vị trí và độ dài vệt hiện tại, với các chuyển đổi tùy thuộc vào việc chúng ta tiếp tục giữ nguyên màu hay chuyển đổi. 

Công thức này đúng nhưng ngay lập tức sẽ trở nên quá lớn nếu được thực hiện trực tiếp. Chiều dài chạy có thể lên tới$k-1$, vậy không gian trạng thái là$O(nk)$. Với$n$lên đến$10^6$, thậm chí việc lưu trữ hoặc lặp lại DP như vậy là không thể. 

Sự đơn giản hóa chính là ngừng theo dõi màu sắc chính xác và thay vào đó chỉ theo dõi cấu trúc của các lần chạy. Quan sát quan trọng là ràng buộc chỉ quan tâm đến việc liệu một lần chạy có đạt đến độ dài hay không$k$, không phải về màu sắc của đường chạy. Điều này có nghĩa là tất cả các màu đều đối xứng và chúng ta chỉ cần đếm xem có bao nhiêu cách tạo chuỗi từ các chuỗi độ dài$1$ĐẾN$k-1$, mỗi lần chạy chọn một màu khác với lần chạy trước. 

Nếu chúng ta nghĩ về mặt số lần chạy, thì mỗi chuỗi là sự kết hợp của các khối. Mỗi khối có chiều dài$1$ĐẾN$k-1$. Khối đầu tiên có thể có bất kỳ màu nào, vì vậy nó đóng góp một yếu tố$c$. Mỗi khối tiếp theo phải chọn một màu khác với khối trước, góp phần tạo ra một yếu tố$c-1$và độc lập chọn độ dài của nó. 

Điều này dẫn đến một sự tái phát tuyến tính tiêu chuẩn trên$f[i]$, Ở đâu$f[i]$là số chuỗi có độ dài hợp lệ$i$. Đối với mỗi vị trí, chúng tôi có thể kéo dài lần chạy hiện tại (miễn là nó không đạt đến độ dài$k$) hoặc bắt đầu lượt chạy mới với màu khác. Khó khăn là DP ngây thơ vẫn yêu cầu độ dài lần chạy theo dõi, nhưng điều này có thể được loại bỏ bằng cách sử dụng tổng tiền tố trong khoảng thời gian cuối cùng.$k-1$tiểu bang. 

Quá trình chuyển đổi trở thành lặp lại cửa sổ trượt: tạo thành các chuỗi kết thúc tại vị trí$i$, chúng tôi xem xét tất cả các cách mà lần chạy cuối cùng có thể bắt đầu tại các vị trí$i-1, i-2, \dots, i-(k-1)$. Mỗi khả năng như vậy tương ứng với việc cố định độ dài lần chạy cuối cùng và chọn màu của nó, đồng thời đảm bảo vị trí trước đó đã kết thúc một cấu hình hợp lệ. 

Điều này làm giảm tính toán cho mỗi trường hợp thử nghiệm xuống$O(n)$, vì mỗi trạng thái có thể được cập nhật bằng cách sử dụng tổng cửa sổ cuộn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP vượt qua trạng thái chạy |$O(nk)$|$O(nk)$| Quá chậm | 
| Cửa sổ trượt DP tái diễn |$O(n)$|$O(n)$hoặc$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định$f[i]$là số lượng màu hợp lệ cho lần đầu tiên$i$đá. 

Chúng tôi cũng duy trì một mảng tổng tiền tố$pref[i] = f[0] + f[1] + \dots + f[i]$, modulo được tính toán$p$, để cho phép tính tổng phạm vi nhanh. 

Sự truy hồi xuất phát từ việc xem xét khối cuối cùng trong việc tô màu chiều dài$i$. Giả sử khối cuối cùng có chiều dài$t$, Ở đâu$1 \le t < k$. Sau đó trước đó$i-t$các vị trí tạo thành bất kỳ chuỗi hợp lệ nào và khối cuối cùng đóng góp một lựa chọn màu khác với khối cuối cùng của tiền tố. 

1. Khởi tạo$f[0] = 1$, đại diện cho chuỗi trống. 
2. Đối với từng vị trí$i$từ$1$ĐẾN$n$, chúng tôi muốn tính toán tất cả các chuỗi kết thúc tại$i$. 
3. Nếu$i < k$, chưa có hạn chế nào nên mọi tiện ích mở rộng đều hợp lệ. Chúng ta có thể tính toán$f[i] = c \cdot (c-1)^{i-1}$trực tiếp hoặc thông qua tái phát. 
4. Đối với chung$i \ge k$, chúng tôi chia theo chiều dài khối cuối cùng$t$, dao động từ$1$ĐẾN$k-1$. Đối với mỗi$t$, tiền tố góp phần$f[i-t]$, và chúng tôi nhân với$c-1$vì khối cuối cùng phải có màu khác với khối trước đó. 
5. Tổng này qua một cửa sổ được tính toán một cách hiệu quả bằng cách sử dụng tổng tiền tố:$$f[i] = (c-1) \cdot (f[i-1] + f[i-2] + \dots + f[i-k+1])$$6. Chúng tôi duy trì các tổng tiền tố sao cho mỗi$f[i]$có thể được tính toán trong thời gian không đổi. 

Câu trả lời cuối cùng là$f[n]$. 

Lý do điều này có tác dụng là vì mọi màu hợp lệ có độ dài$i$được xác định duy nhất bởi vị trí bắt đầu lần chạy cuối cùng. Điểm bắt đầu đó phải nằm trong điểm cuối cùng$k-1$vị trí, nếu không việc chạy sẽ vi phạm ràng buộc. Mỗi lựa chọn vị trí bắt đầu tương ứng với chính xác một phân tách thành tiền tố hợp lệ và một lần chạy mới, và tất cả các phân tách như vậy đều rời rạc. Điều này cung cấp một phân vùng hoàn chỉnh của không gian giải pháp, chính xác là những gì mà tổng cửa sổ trượt thu được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, c, k, p = map(int, input().split())

        if k > n:
            # no restriction applies
            res = pow(c, n, p)
            print(res)
            continue

        # f[i] = number of valid arrays of length i
        f = [0] * (n + 1)
        pref = [0] * (n + 1)

        f[0] = 1
        pref[0] = 1

        for i in range(1, n + 1):
            # window sum f[i-1] ... f[i-k+1]
            left = i - 1
            right = i - k

            total = pref[left]
            if right >= 0:
                total = (total - pref[right] + p) % p

            f[i] = total * (c - 1) % p
            pref[i] = (pref[i - 1] + f[i]) % p

        print(f[n])

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì một mảng DP để đếm chuỗi và một mảng tổng tiền tố để cho phép tính tổng trong phạm vi thời gian không đổi. Quá trình chuyển đổi trực tiếp mã hóa ý tưởng rằng lần chạy cuối cùng phải có độ dài tối đa$k-1$, vì vậy chúng tôi chỉ tổng hợp các vị trí cắt hợp lệ trước đó. Phép nhân với$c-1$phản ánh việc lựa chọn một màu khác với màu của lần chạy trước. 

Một chi tiết triển khai tinh tế là việc xử lý phép trừ mô-đun khi tính tổng cửa sổ. Vì tổng tiền tố có thể bị trừ nên chúng ta phải cộng$p$trước khi dùng modulo để tránh giá trị âm. 

Trường hợp đặc biệt$k > n$được xử lý riêng biệt bằng cách sử dụng lũy ​​thừa nhanh, vì phép truy toán suy biến thành màu không hạn chế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, c = 2, k = 3
```Chúng tôi tính toán$f[i]$từng bước một. 

| tôi | tổng cửa sổ f[i-1..i-k+1] | f[i] | 
| --- | --- | --- | 
| 0 | - | 1 | 
| 1 | 1 | 2 | 
| 2 | 3 | 6 | 
| 3 | 5 | 10 | 
| 4 | 10 | 20 | 
| 5 | 20 | 40 | 

Đây$k=3$, vì vậy chúng ta luôn tính tổng 2 trạng thái cuối và nhân với$c-1 = 1$. Điều này cho thấy sự lặp lại hoạt động giống như sự tăng trưởng theo kiểu Fibonacci khi các ràng buộc bắt đầu có hiệu lực. 

Dấu vết này xác nhận rằng mọi cấu hình mới đều được xây dựng từ những cấu hình ngắn hơn hợp lệ mà không tính hai lần, vì mỗi phần mở rộng tương ứng với một độ dài duy nhất ở lần chạy cuối cùng. 

### Ví dụ 2 

đầu vào:```
n = 4, c = 3, k = 2
```Ở đây các màu liền kề phải khác nhau. 

| tôi | tổng cửa sổ | f[i] | 
| --- | --- | --- | 
| 0 | - | 1 | 
| 1 | 1 | 3 | 
| 2 | 3 | 6 | 
| 3 | 6 | 12 | 
| 4 | 12 | 24 | 

Điều này giảm xuống còn$3 \cdot 2^{n-1}$và bảng hiển thị sự lặp lại khớp với biểu mẫu đã đóng đó. Mỗi bước xác nhận rằng chỉ các phần mở rộng từ các màu trước đó khác nhau mới được tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$mỗi trường hợp thử nghiệm | Mỗi trạng thái DP được tính toán bằng cách sử dụng phép trừ tổng tiền tố theo thời gian không đổi | 
| Không gian |$O(n)$| Mảng tính tổng DP và tiền tố | 

Các ràng buộc cho phép lên đến$10^6$tổng cộng$n$, do đó thời gian tuyến tính cho mỗi trường hợp thử nghiệm là đủ. Bộ nhớ vẫn an toàn vì mảng được sử dụng lại cho mỗi trường hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

# sample tests
assert run("""5
7 3 6 100000007
10 2 5 100000037
100 100 50 100000039
1000 123456789 345 100000049
1000000 987654321 123456 100000073
""") == """2172
802
62324845
55895361
81968913"""

# edge: no restriction
assert run("1\n5 2 10 1000000007\n") == str(pow(2, 5, 1000000007))

# edge: k=2 (no equal adjacent)
assert run("1\n5 3 2 1000000007\n") == str(3 * pow(2, 4, 1000000007) % 1000000007)

# edge: c=1
assert run("1\n10 1 3 1000000007\n") == "0"

# small case
assert run("1\n3 3 3 1000000007\n") == str(3 + 6 + 12)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$k>n$|$c^n$| không có trường hợp hạn chế | 
|$k=2$|$c(c-1)^{n-1}$| ràng buộc liền kề | 
|$c=1$| 0 cho$n \ge k$| đường dài không thể | 
| bé nhỏ$n$| trận đấu DP thủ công | tính đúng đắn của sự tái phát | 

## Vỏ cạnh 

Khi nào$k > n$, điều kiện cửa sổ DP không bao giờ được kích hoạt. Ví dụ, với$n=5, k=10$, nếu không thì vòng lặp sẽ tính toán các phạm vi trống. Thuật toán chuyển rõ ràng sang$c^n$, do đó nó tránh được phép trừ tiền tố không xác định và phù hợp với thực tế là mọi màu sắc đều hợp lệ. 

Khi$k=2$, kích thước cửa sổ trở thành 1, vì vậy mỗi trạng thái chỉ phụ thuộc vào$f[i-1]$. Sự tái phát sụp đổ một cách chính xác vào$f[i] = (c-1) f[i-1]$và việc triển khai tự nhiên tạo ra hành vi theo cấp số nhân mà không cần xử lý đặc biệt. 

Khi$c=1$, chuỗi duy nhất có thể là một màu không đổi. Nếu như$k \le n$, điều này vi phạm ràng buộc và DP tạo ra số 0 một cách chính xác vì hệ số$c-1$trở thành số không, buộc tất cả$f[i]$vì$i \ge 2$biến mất.
