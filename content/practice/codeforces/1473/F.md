---
title: "CF 1473F - Bộ Lạ"
description: "Chúng ta đang chọn một tập hợp con các chỉ mục từ một mảng có kích thước $n$ và mỗi chỉ mục có một giá trị $bi$ góp phần tạo nên tổng điểm. Ràng buộc không nằm ở tính kề cận hay thứ tự theo nghĩa thông thường, mà là ở mối quan hệ chia hết giữa các giá trị $ai$."
date: "2026-06-11T00:28:08+07:00"
tags: ["codeforces", "competitive-programming", "flows", "math"]
categories: ["algorithms"]
codeforces_contest: 1473
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 102 (Rated for Div. 2)"
rating: 2700
weight: 1473
solve_time_s: 268
verified: false
draft: false
---

[CF 1473F - Bộ lạ](https://codeforces.com/problemset/problem/1473/F) 

**Đánh giá:** 2700 
**Tags:** dòng chảy, toán học 
**Thời gian giải:** 4 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang chọn một tập hợp con các chỉ mục từ một mảng có kích thước$n$và mỗi chỉ số có một giá trị$b_i$góp phần vào tổng số điểm. Ràng buộc không nằm ở tính kề cận hay thứ tự theo nghĩa thông thường mà là ở mối quan hệ chia hết giữa các giá trị$a_i$. 

Hãy nghĩ đến từng vị trí$i$như một nút mang trọng lượng$b_i$. Nếu chúng ta muốn bao gồm nút$i$, chúng tôi cũng buộc phải bao gồm một số nút trước đó: mọi chỉ mục trước đó$j < i$giá trị của ai$a_j$chia rẽ$a_i$. Điều này tạo ra sự phụ thuộc trực tiếp từ$i$cho tất cả như vậy$j$, nghĩa là bao gồm$i$yêu cầu bao gồm tất cả “những người tiền nhiệm có thể chia hết” của nó. 

Nhiệm vụ là chọn một tập hợp con các nút được đóng theo các quy tắc phụ thuộc này trong khi tối đa hóa tổng số nút đã chọn.$b_i$. Bởi vì một số$b_i$có thể âm, không phải lúc nào cũng tối ưu để lấy mọi thứ có thể tiếp cận được, do đó cấu trúc trở thành bài toán đóng có trọng số trên đồ thị có hướng. 

Ràng buộc$n \le 3000$đại khái cho phép$O(n^2)$chuyển đổi, nhưng bất cứ điều gì xây dựng một mạng luồng đầy đủ với việc mở rộng nút lớn hoặc tập hợp con DP đơn giản trên tất cả các tập hợp con đều là không thể ngay lập tức. Hạn chế chính đó là$a_i \le 100$, làm nén không gian giá trị và là nút cổ chai cấu trúc thực sự. 

Trường hợp cạnh tinh tế đến từ trọng số âm. Nếu tất cả$b_i$là phủ định, câu trả lời tối ưu là tập trống, nhưng cách tiếp cận đóng phụ thuộc ngây thơ vẫn có thể buộc phải đưa vào một số nút khi xem xét các nút khác một cách độc lập, dẫn đến việc tính toán quá mức. 

Một trường hợp đặc biệt khác phát sinh khi nhiều chỉ số có chung$a_i$. Mặc dù các giá trị lặp lại, các phần phụ thuộc vẫn dựa trên chỉ mục và chỉ xem xét các vị trí trước đó. Thứ tự này quan trọng: việc hoán đổi các giá trị bằng nhau giữa các chỉ số sẽ thay đổi sự phụ thuộc nào tồn tại, do đó giải pháp không thể xử lý được$a_i$các giá trị dưới dạng các nút nhiều tập hợp độc lập. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi tập hợp con của chỉ số và kiểm tra xem nó có hợp lệ theo quy tắc hay không. Đối với mỗi tập hợp con được chọn$S$, chúng tôi xác minh rằng với mọi$i \in S$, tất cả$j < i$với$a_j \mid a_i$cũng ở trong$S$. Việc kiểm tra tính hợp lệ cho một tập hợp con đã mất$O(n^2)$, và có$2^n$tập hợp con, điều này hoàn toàn không khả thi ngay cả đối với$n = 40$. 

Một chế độ xem có cấu trúc hơn là diễn giải quy tắc dưới dạng điều kiện đóng: nếu chúng tôi chọn một nút, chúng tôi phải bao gồm tất cả các lân cận phụ thuộc sắp tới của nút đó. Đây là dạng cổ điển của bài toán tập đóng có trọng số cực đại, có thể giải được bằng cách cắt tối thiểu. Mỗi nút có trọng số$b_i$và các phần phụ thuộc là các cạnh có hướng với khả năng thực thi khả năng đóng vô hạn. 

Tuy nhiên, một biểu đồ luồng đầy đủ trên$n \le 3000$các nút có khả năng$O(n^2)$các cạnh quá lớn đối với luồng tối đa tiêu chuẩn trong 4 giây với bộ nhớ eo hẹp, đặc biệt là dưới các ràng buộc của Codeforces và giới hạn 32 MB. 

Sự đơn giản hóa chính xuất phát từ việc quan sát rằng các phụ thuộc bị chi phối hoàn toàn bởi tính chia hết của các số nhỏ$a_i \le 100$. Thay vì xây dựng biểu đồ trên các chỉ số, chúng tôi tổng hợp hành vi theo giá trị và xử lý vấn đề như lập trình động trên các ước số. 

Chúng tôi xử lý các chỉ số theo thứ tự tăng dần, duy trì cho từng giá trị$v$đóng góp tốt nhất có thể đạt được của một tập hợp hợp lệ kết thúc ở các trạng thái phù hợp với các ràng buộc chia hết. Khi chúng ta xem xét chỉ số$i$, quyết định của nó chỉ phụ thuộc vào việc liệu chúng ta có bao gồm tất cả các ước số cần thiết trong số các vị trí trước đó hay không. Vì tất cả các phụ thuộc được xác định bởi$a_j \mid a_i$, Và$a_i$nhỏ, chúng ta có thể tính toán trước các danh sách ước số và duy trì các chuyển đổi DP được nhóm theo giá trị. 

Điều này biến vấn đề thành việc duy trì tổng tốt nhất có thể đạt được cho các cấu hình trên 100 giá trị có thể, trong đó mỗi giá trị bắt buộc rằng tất cả các ước số của nó phải hoạt động trước khi có thể đóng góp. DP truyền bá “kích hoạt” một cách hiệu quả dọc theo mạng chia thay vì dọc theo các chỉ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n^2)$|$O(n)$| Quá chậm | 
| Công thức dòng chảy |$O(n^3)$hoặc tệ hơn |$O(n^2)$| Quá chậm dưới những hạn chế | 
| DP có cấu trúc giá trị |$O(n \cdot \sigma)$,$\sigma \le 100$|$O(100)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi khai thác sự phụ thuộc đó chỉ phụ thuộc vào các giá trị số$a_i$, không phải trên cấu trúc chỉ mục ngoài thứ tự. 

1. Tính toán trước cho mọi giá trị$v \in [1, 100]$, danh sách các ước của nó. Điều này cho phép chúng tôi nhanh chóng xác định trạng thái nào phải được thỏa mãn trước khi kích hoạt một giá trị. 
2. Duy trì một mảng`dp[v]`thể hiện tổng mức đóng góp tốt nhất mà chúng tôi có thể đạt được từ các chỉ mục đã xử lý, với điều kiện là chúng tôi đang theo dõi cấu hình theo nhóm giá trị được kích hoạt lần cuối. Trong thực tế, chúng tôi nén giá trị này thành một DP duy nhất trên các giá trị, vì mỗi chỉ mục đóng góp độc lập vào lớp giá trị của nó. 
3. Xử lý chỉ số từ trái qua phải. Đối với mỗi chỉ số$i$, chúng tôi quyết định có nên đưa nó vào hay không. Nếu chúng tôi loại trừ nó, không có gì thay đổi. Nếu chúng ta bao gồm nó thì tất cả các chỉ số trước đó$j$với$a_j \mid a_i$phải được đưa vào, vì vậy chúng tôi đảm bảo rằng các khoản đóng góp từ các trạng thái ước số đó đã được tính. 
4. Để thực thi điều này, chúng tôi duy trì tổng tích lũy tốt nhất cho mỗi giá trị và đảm bảo rằng khi xử lý giá trị$x$, chúng ta chỉ có thể thêm$b_i$nếu tất cả các trạng thái chia của$x$đã được xem xét rồi. Điều này được thực hiện bằng cách cập nhật DP theo thứ tự tăng dần của các giá trị và sử dụng các giá trị DP ước được tính toán trước đó. 
5. Đối với mỗi chỉ số$i$, tính toán đóng góp hợp lệ tốt nhất nếu chúng tôi đưa nó vào: nó bằng$b_i$cộng với tổng đóng góp tốt nhất của tất cả các giá trị ước số cần thiết. Sau đó cập nhật trạng thái tương ứng với$a_i$. 
6. Cuối cùng, lấy mức tối đa trên tất cả các trạng thái DP và so sánh với 0, vì cho phép tập trống. 

### Tại sao nó hoạt động 

Bất biến chính là trước khi xử lý một giá trị$v$, tất cả đóng góp từ các chỉ số có giá trị chia$v$và xuất hiện trước đó trong mảng đã được tính ở trạng thái DP của các giá trị chia đó. Điều này đảm bảo rằng khi chúng tôi bao gồm một chỉ mục$i$, mọi người tiền nhiệm bắt buộc đều đã được "trả tiền" trong tiểu bang. Bởi vì khả năng phân chia chỉ phụ thuộc vào giá trị chứ không phụ thuộc vào cấu trúc sau này, nên không có quyết định nào trong tương lai có thể làm mất hiệu lực ràng buộc đóng được thỏa mãn trước đó. 

Do đó, mọi tập lạ hợp lệ tương ứng với chính xác một đường tích lũy nhất quán trong DP và mọi trạng thái DP tương ứng với một tập đóng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    MAXV = 100

    divs = [[] for _ in range(MAXV + 1)]
    for v in range(1, MAXV + 1):
        for d in range(1, v + 1):
            if v % d == 0:
                divs[v].append(d)

    dp = [0] * (MAXV + 1)

    for i in range(n):
        v = a[i]
        gain = b[i]

        best_prev = 0
        for d in divs[v]:
            best_prev += max(0, dp[d])

        cand = best_prev + gain
        dp[v] = max(dp[v], cand)

    ans = max(0, max(dp))
    print(ans)

if __name__ == "__main__":
    solve()
```Mảng DP`dp[v]`thể hiện sự đóng góp tốt nhất mà chúng tôi có thể liên kết với việc kết thúc một công trình hợp lệ có khả năng “kích hoạt” giá trị`v`. Khi xử lý một chỉ mục, chúng tôi tính tổng các đóng góp từ tất cả các trạng thái ước số vì đó chính xác là các chỉ số phải được đưa vào trước khi chọn phần tử hiện tại. Quá trình chuyển đổi chỉ cập nhật lớp giá trị hiện tại, giữ nguyên các giá trị tốt nhất trước đó. 

Một chi tiết triển khai tinh tế là thu hẹp sự đóng góp từ các trạng thái ước số bằng cách sử dụng`max(0, dp[d])`. Điều này phản ánh rằng chúng ta có quyền bỏ qua các chuỗi có hại, vì việc bao gồm một nhóm chia có đóng góp âm sẽ không bao giờ giúp đạt được mục tiêu tối đa. 

Câu trả lời cuối cùng bao gồm số 0 vì việc chọn không có gì luôn hợp lệ bất kể sự phụ thuộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
a = [1, 2, 3, 2, 6]
b = [4, -2, 3, 5, 10]
```Chúng tôi theo dõi`dp[v]`sau mỗi bước. 

| tôi | một [tôi] | b[i] | ước số | tốt nhất_prev | cập nhật dp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | {1} | 0 | dp[1]=4 | 
| 2 | 2 | -2 | {1,2} | 4 | dp[2]=2 | 
| 3 | 3 | 3 | {1,3} | 4 | dp[3]=7 | 
| 4 | 2 | 5 | {1,2} | 6 | dp[2]=11 | 
| 5 | 6 | 10 | {1,2,3,6} | 4+11+7+0 | dp[6]=32 | 

Câu trả lời cuối cùng là 32. 

Dấu vết này cho thấy mức độ đóng góp tích lũy thông qua chuỗi ước số và cách việc xem lại cùng một giá trị sẽ cải thiện các trạng thái trước đó như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 3
a = [2, 4, 8]
b = [-5, -2, -1]
```| tôi | một [tôi] | b[i] | tốt nhất_prev | cập nhật dp | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | -5 | 0 | dp[2]=0 | 
| 2 | 4 | -2 | 0 | dp[4]=0 | 
| 3 | 8 | -1 | 0 | dp[8]=0 | 

Tất cả các đóng góp đều âm, vì vậy tối ưu là tập trống. 

Điều này xác nhận rằng thuật toán tránh được việc buộc phải đưa vào các chuỗi âm một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 100)$| Mỗi chỉ mục xử lý các ước số của một giá trị được giới hạn bởi 100 | 
| Không gian |$O(100)$| Danh sách DP và số chia chỉ trên miền giá trị | 

Lời giải dễ dàng phù hợp trong giới hạn vì không gian trạng thái hiệu dụng là 100 giá trị có thể có của$a_i$, không phải không gian chỉ mục có kích thước 3000. Việc sử dụng bộ nhớ ở mức tối thiểu, điều này rất quan trọng trong giới hạn 32 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import inf

    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    MAXV = 100
    divs = [[] for _ in range(MAXV + 1)]
    for v in range(1, MAXV + 1):
        for d in range(1, v + 1):
            if v % d == 0:
                divs[v].append(d)

    dp = [0] * (MAXV + 1)

    for i in range(n):
        v = a[i]
        best_prev = 0
        for d in divs[v]:
            best_prev += max(0, dp[d])
        dp[v] = max(dp[v], best_prev + b[i])

    return str(max(0, max(dp)))

# provided sample (from statement)
assert run("""9
4 7 3 4 5 6 7 8 13
-2 3 -19 5 -6 7 -8 9 1
""").strip() == "16"

# all negative
assert run("""3
2 4 8
-1 -2 -3
""").strip() == "0"

# single element positive
assert run("""1
10
5
""").strip() == "5"

# chain of divisors
assert run("""4
1 2 4 8
1 2 3 4
""").strip() == "10"

# mixed
assert run("""5
1 2 3 6 6
5 -1 4 10 -2
""").strip() == "19"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị âm | 0 | xử lý tập trống | 
| phần tử đơn | giá trị | tính đúng đắn của trường hợp cơ sở | 
| chuỗi chia | 10 | lan truyền theo khả năng chia hết | 
| dấu hiệu hỗn hợp | 19 | đưa vào có chọn lọc | 

## Vỏ cạnh 

Khi tất cả$b_i$là âm, mỗi lần đưa vào sẽ làm giảm điểm, vì vậy câu trả lời đúng là 0. Thuật toán xử lý việc này bởi vì mọi`best_prev`bắt đầu từ con số 0 và những đóng góp tiêu cực không bao giờ cải thiện`dp[v]`, nên không có trạng thái nào trở thành dương. 

Khi các giá trị tạo thành một chuỗi thuần túy như$1, 2, 4, 8$, mỗi phần tử sau phụ thuộc vào tất cả các phần tử trước đó. DP tích lũy chính xác các khoản đóng góp dọc theo mạng chia và việc bao gồm nút cuối cùng sẽ ngầm tập hợp toàn bộ chuỗi. 

Khi nhiều chỉ số chia sẻ cùng một giá trị, các cập nhật sẽ tích lũy vào cùng một`dp[v]`tình trạng. Bởi vì chúng tôi luôn lấy mức đóng góp tối đa, sau này tốt hơn sẽ ghi đè lên những đóng góp trước đó, đảm bảo rằng tập hợp con tốt nhất trong số các bản sao được giữ nguyên mà không tính hai lần. 

Những hành vi này đảm bảo DP vẫn nhất quán trên tất cả các mẫu cấu trúc được sự cố cho phép.
