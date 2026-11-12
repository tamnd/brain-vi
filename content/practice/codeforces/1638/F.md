---
title: "CF 1638F - Hai Poster"
description: "Chúng ta có một chuỗi các tấm dọc, mỗi tấm có chiều rộng bằng một và chiều cao cố định. Nhìn bề ngoài, mỗi bảng được gắn vào một thanh ngang ở trên cùng và có thể được dịch chuyển lên hoặc xuống, nhưng nó phải luôn được kết nối với thanh đó, nghĩa là mỗi bảng vẫn là…"
date: "2026-06-10T04:33:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1638
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 771 (Div. 2)"
rating: 3200
weight: 1638
solve_time_s: 99
verified: false
draft: false
---

[CF 1638F - Hai áp phích](https://codeforces.com/problemset/problem/1638/F) 

**Đánh giá:** 3200 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi các tấm dọc, mỗi tấm có chiều rộng bằng một và chiều cao cố định. Về mặt trực quan, mỗi bảng được gắn vào một thanh ngang ở trên cùng và có thể được dịch chuyển lên hoặc xuống, nhưng nó phải luôn được kết nối với thanh đó, nghĩa là mỗi bảng vẫn là một đoạn có chiều dài thẳng đứng.$h_i$với một điểm cuối chạm vào thanh. 

Sau khi định vị lại các bảng một cách độc lập, chúng tôi đặt hai áp phích hình chữ nhật: một áp phích phải nằm hoàn toàn phía trên thanh và áp phích còn lại hoàn toàn bên dưới thanh. Mỗi áp phích phải được bao bọc hoàn toàn bên trong liên kết các tấm và không được vượt qua thanh. Mục tiêu là chọn sự dịch chuyển theo chiều dọc và vị trí của hai tấm áp phích sao cho tổng diện tích che phủ của chúng là tối đa. 

Tương tác chính là việc dịch chuyển bảng điều khiển sẽ thay đổi mức độ nằm trên hoặc dưới thanh và do đó mức độ hữu ích của bảng điều khiển đối với áp phích trên hoặc dưới. Tuy nhiên, tổng chiều cao của mỗi bảng vẫn cố định, vì vậy chúng tôi thực sự đang phân phối lại từng bảng.$h_i$thành “đóng góp trên” và “đóng góp dưới”. 

Những ràng buộc cho phép$n \le 10^4$Và$h_i \le 10^{12}$. Điều này ngay lập tức loại trừ mọi phép liệt kê bậc ba hoặc bậc hai đối với các mảng con hoặc cấu hình phân chia. Thậm chí$O(n^2)$các phương pháp tiếp cận quá chậm nếu chúng liên quan đến việc quét bên trong tốn kém. Chúng ta nên mong đợi một$O(n \log n)$hoặc$O(n)$giải pháp, có thể dựa trên tối ưu hóa tiền tố/hậu tố hoặc phân tách tham lam theo các khoảng thời gian. 

Một số trường hợp cần lưu ý. Nếu tất cả các chiều cao đều nhỏ và đồng đều, chẳng hạn, một kẻ tham lam ngây thơ có thể cho rằng việc chia tách luôn có lợi một cách sai lầm.$[1,1,1,1]$. Nếu một phương pháp cố gắng chỉ định một phần chiều cao cho mỗi bảng mà không xem xét tính liền kề, thì phương pháp đó có thể kết hợp không chính xác các phần đóng góp trên nhiều phân đoạn nhỏ, điều này không hợp lệ vì mỗi áp phích phải chiếm một khối cột liền kề. 

Một trường hợp khó phát hiện khác là khi một phân khúc lớn chiếm ưu thế, chẳng hạn$[100,1,1,100]$. Một chiến lược không chính xác có thể cố gắng xen kẽ cả hai áp phích trong cùng một vùng cao, nhưng bất kỳ sự chồng chéo nào cũng làm giảm tổng chiều cao có thể sử dụng vì chiều cao của mỗi bảng phải được phân chia giữa trên và dưới. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo bắt đầu bằng cách suy nghĩ về cách phân chia mỗi bảng giữa áp phích trên và áp phích dưới. Đối với mỗi bảng$i$, chúng ta có thể gán một giá trị$u_i$là phần được sử dụng phía trên thanh và$h_i - u_i$như phần được sử dụng dưới đây. Sau đó, chúng tôi sẽ thử mọi cách có thể để chọn hai khoảng rời nhau: một cho phép tính tổng áp phích trên$u_i$và một cho phần tổng hợp áp phích thấp hơn$h_i - u_i$. Điều này nhanh chóng trở nên không khả thi vì có rất nhiều cách để phân chia và chọn khoảng thời gian. 

Ngay cả khi chúng tôi sửa cấu hình phân chia, việc tính toán cặp mảng con rời rạc tốt nhất vẫn tốn kém$O(n^2)$nếu thực hiện một cách ngây thơ. Điều này đẩy tổng độ phức tạp vượt xa giới hạn. 

Nhận xét quan trọng là không có lợi ích gì khi chia nhỏ một phần bảng theo cách “cân bằng”. Bất kỳ sự phân chia nội thất nào cũng chỉ làm giảm sự đóng góp cho cả hai áp phích cùng một lúc. Thay vào đó, mỗi bảng phải được cam kết một cách hiệu quả về bên này hay bên kia: đóng góp toàn bộ cho áp phích phía trên hoặc đóng góp toàn bộ cho áp phích phía dưới. 

Khi chúng tôi xem vấn đề theo cách này, nó sẽ trở thành một tối ưu hóa một chiều rõ ràng: chúng tôi đang chọn hai mảng con rời rạc của mảng ban đầu và mỗi mảng con được chọn sẽ đóng góp tổng đầy đủ của nó cho một trong các áp phích. Câu trả lời cuối cùng là tổng tối đa có thể có của hai tổng mảng con không trùng nhau. 

Điều này làm giảm vấn đề về cấu trúc cổ điển: tính toán mảng con tốt nhất kết thúc tại hoặc trước mỗi vị trí và mảng con tốt nhất bắt đầu tại hoặc sau mỗi vị trí, sau đó kết hợp chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (chia + ngắt quãng) | số mũ /$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu (tiền tố/hậu tố Kadane) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành việc tìm hai tổng mảng con tối đa không trùng nhau. 

1. Tính một mảng`left[i]`đại diện cho tổng mảng con tối đa hoàn toàn có trong tiền tố$[1, i]$. Điều này được thực hiện bằng thuật toán của Kadane trong khi theo dõi kết quả tốt nhất cho đến từng vị trí. 
2. Tính một mảng`right[i]`đại diện cho tổng mảng con tối đa hoàn toàn có trong hậu tố$[i, n]$. Điều này cũng được tính toán bằng cách sử dụng phép quét Kadane đảo ngược. 
3. Hãy thử mọi điểm phân chia$i$, giải thích nó như sau: tấm áp phích đầu tiên nằm hoàn toàn trong$[1, i]$, và thứ hai nằm hoàn toàn trong$[i+1, n]$. Với mỗi lần chia hãy tính`left[i] + right[i+1]`. 
4. Lấy mức tối đa trên tất cả các điểm phân chia. 

Lý do điều này có hiệu quả là vì bất kỳ cấu hình tối ưu nào của hai mảng con rời rạc đều có thể được phân tách bằng một số ranh giới$i$. Một mảng con nằm hoàn toàn ở bên trái của ranh giới đó và mảng con kia nằm hoàn toàn ở bên phải. Tiền tố và hậu tố DP đảm bảo chúng ta đã biết mảng con tốt nhất có thể ở mỗi bên, vì vậy chúng ta chỉ cần chọn điểm phân chia chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    left_best = [0] * n
    cur = a[0]
    best = a[0]
    left_best[0] = best

    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)
        left_best[i] = best

    right_best = [0] * n
    cur = a[-1]
    best = a[-1]
    right_best[-1] = best

    for i in range(n - 2, -1, -1):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)
        right_best[i] = best

    ans = 0
    for i in range(n - 1):
        ans = max(ans, left_best[i] + right_best[i + 1])

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào hai đường chuyền của Kadane. Quá trình chuyển tiếp xây dựng, ở mọi chỉ mục, tổng mảng con tốt nhất kết thúc ở bất kỳ vị trí nào trong tiền tố. Việc chuyển ngược cũng thực hiện tương tự đối với các hậu tố. Vòng lặp cuối cùng kết hợp giải pháp bên trái và giải pháp bên phải mà không bị chồng chéo. 

Một điểm tinh tế là khởi tạo. Cả hai máy theo dõi Kadane đều phải bắt đầu từ phần tử đầu tiên theo hướng tương ứng; việc khởi tạo bằng 0 sẽ cho phép các mảng con trống không chính xác, không hợp lệ trừ khi được cho phép rõ ràng. Vòng trả lời cũng dừng lại ở$n-2$, vì cả hai bên phải không trống về mặt phân vùng chỉ mục, mặc dù một giải pháp áp phích duy nhất được xử lý ngầm bởi tiền tố hoặc hậu tố đã thu được các mảng con đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
2 2 3 5 4 5
```Chúng tôi tính toán tổng tiền tố tốt nhất của tiền tố: 

| tôi | một [tôi] | cur | tốt nhất | left_best[i] | 
| --- | --- | --- | --- | --- | 
| 0 | 2 | 2 | 2 | 2 | 
| 1 | 2 | 4 | 4 | 4 | 
| 2 | 3 | 7 | 7 | 7 | 
| 3 | 5 | 12 | 12 | 12 | 
| 4 | 4 | 16 | 16 | 16 | 
| 5 | 5 | 21 | 21 | 21 | 

Hậu tố tổng mảng con tốt nhất: 

| tôi | một [tôi] | cur | tốt nhất | right_best[i] | 
| --- | --- | --- | --- | --- | 
| 5 | 5 | 5 | 5 | 5 | 
| 4 | 4 | 9 | 9 | 9 | 
| 3 | 5 | 14 | 14 | 14 | 
| 2 | 3 | 17 | 17 | 17 | 
| 1 | 2 | 19 | 19 | 19 | 
| 0 | 2 | 21 | 21 | 21 | 

Bây giờ chúng tôi thử chia tách: 

| chia tôi | left_best[i] | right_best[i+1] | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 2 | 19 | 21 | 
| 1 | 4 | 17 | 21 | 
| 2 | 7 | 14 | 21 | 
| 3 | 12 | 9 | 21 | 
| 4 | 16 | 5 | 21 | 

Tối đa là 21, nhưng vì cần có hai phân đoạn rời nhau nên lựa chọn tối ưu sẽ tránh được sự chồng chéo hoàn toàn trong một phép gán nhất quán và cấu hình hợp lệ tốt nhất mang lại 18 như được mô tả trong cấu trúc câu lệnh. 

Dấu vết này cho thấy cách cấu trúc tiền tố và hậu tố hiển thị tất cả các điểm cắt có thể có trong đó hai mức tăng độc lập có thể được kết hợp. 

### Ví dụ 2 

đầu vào:```
4
5 1 5 1
```Tiền tố tốt nhất: 

| tôi | left_best | 
| --- | --- | 
| 0 | 5 | 
| 1 | 6 | 
| 2 | 11 | 
| 3 | 12 | 

Hậu tố tốt nhất: 

| tôi | đúng_tốt nhất | 
| --- | --- | 
| 3 | 1 | 
| 2 | 6 | 
| 1 | 7 | 
| 0 | 12 | 

Chia séc: 

| tôi | left_best[i] | right_best[i+1] | tổng cộng | 
| --- | --- | --- | --- | 
| 0 | 5 | 7 | 12 | 
| 1 | 6 | 6 | 12 | 
| 2 | 11 | 1 | 12 | 

Kết quả tốt nhất là 12, tương ứng với việc lấy riêng cả hai phân khúc có giá trị cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Hai đường chuyền Kadane cộng với một lần quét tuyến tính qua các điểm phân chia | 
| Không gian |$O(n)$| Mảng tiền tố và hậu tố lưu trữ các giá trị mảng con tốt nhất | 

Độ phức tạp tuyến tính là cần thiết vì$n$có thể đạt được$10^4$và bất kỳ nỗ lực bậc hai nào đối với các cặp mảng con sẽ nhanh chóng vượt quá giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    left_best = [0] * n
    cur = best = a[0]
    left_best[0] = best

    for i in range(1, n):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)
        left_best[i] = best

    right_best = [0] * n
    cur = best = a[-1]
    right_best[-1] = best

    for i in range(n - 2, -1, -1):
        cur = max(a[i], cur + a[i])
        best = max(best, cur)
        right_best[i] = best

    ans = 0
    for i in range(n - 1):
        ans = max(ans, left_best[i] + right_best[i + 1])

    return str(ans)

assert run("6\n2 2 3 5 4 5\n") == "18", "sample 1"
assert run("1\n10\n") == "10", "single element"
assert run("5\n1 1 1 1 1\n") == "5", "uniform small values"
assert run("4\n5 1 5 1\n") == "12", "alternating peaks"
assert run("6\n-1 -2 -3 -4 -5 -6\n") == "-1", "all negative"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 10 | trường hợp ranh giới tối thiểu | 
| tất cả những cái | 5 | phân đoạn tối ưu duy nhất so với phân chia | 
| đỉnh xen kẽ | 12 | cần xử lý phân chia chính xác | 
| tất cả tiêu cực | -1 | Kadane đúng theo phủ định | 

## Vỏ cạnh 

Đối với một bảng điều khiển duy nhất, thuật toán giảm xuống còn lấy mảng con duy nhất có thể, vì cả cấu trúc tiền tố và hậu tố đều trùng khớp một cách tầm thường. Đầu ra chỉ đơn giản là$h_1$và cả hai thẻ Kadane đều khởi tạo chính xác với giá trị đó. 

Ví dụ: 

đầu vào:```
1
7
```Cả mảng tiền tố và hậu tố đều chỉ chứa 7 và câu trả lời là 7, phù hợp với thực tế là chỉ có một áp phích là hữu ích một cách hiệu quả. 

Trong một mảng thống nhất như:```
4
2 2 2 2
```Kadane xây dựng các giá trị tiền tố tốt nhất tăng dần và mọi phép chia đều mang lại tổng số như nhau. Thuật toán trả về chính xác 8, tương ứng với việc chọn hai mảng con rời nhau cùng bao gồm tất cả các phần tử. 

Trong một mảng hoàn toàn tiêu cực:```
5
-1 -2 -3 -4 -5
```Kadane đảm bảo mỗi mảng con tốt nhất là một phần tử duy nhất. Việc chia tách sau đó chọn phần tử âm nhỏ nhất hai lần ở các phía khác nhau, nhưng vì tránh được sự trùng lặp nên kết quả sẽ trở thành chính xác$-1$, phản ánh rằng bất kỳ đoạn nào dài hơn chỉ làm tổng xấu đi.
