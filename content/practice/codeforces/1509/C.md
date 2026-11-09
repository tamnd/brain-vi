---
title: "CF 1509C - Đại hội thể thao"
description: "Chúng tôi được cung cấp tốc độ chạy của tất cả các thành viên hội đồng. Chúng tôi có thể chọn bất kỳ thứ tự nào mà chúng chạy. Đối với mỗi tiền tố của thứ tự đã chọn, chúng tôi xem xét tất cả các vận động viên đã tham gia."
date: "2026-06-10T19:46:17+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1509
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 715 (Div. 2)"
rating: 1800
weight: 1509
solve_time_s: 132
verified: true
draft: false
---

[CF 1509C - Đại hội thể thao](https://codeforces.com/problemset/problem/1509/C) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp tốc độ chạy của tất cả các thành viên hội đồng. Chúng tôi có thể chọn bất kỳ thứ tự nào mà chúng chạy. 

Đối với mỗi tiền tố của thứ tự đã chọn, chúng tôi xem xét tất cả các vận động viên đã tham gia. Sự khác biệt của tiền tố đó là sự khác biệt giữa tốc độ lớn nhất và nhỏ nhất được thấy cho đến nay. Người chạy đầu tiên luôn đóng góp bằng 0 vì mức tối đa và tối thiểu bằng nhau. 

Mục tiêu của chúng tôi là sắp xếp các đường chạy sao cho tổng của tất cả các khác biệt về tiền tố càng nhỏ càng tốt. 

Đầu vào chỉ đơn giản là một mảng tốc độ. Đầu ra là tổng sai lệch tối thiểu có thể đạt được sau khi chọn thứ tự tốt nhất. 

Quan sát quan trọng nhất xuất phát từ thực tế là sự khác biệt chỉ phụ thuộc vào mức tối thiểu và tối đa hiện tại giữa những người chạy đã được chọn. Thứ tự nội bộ chính xác của những người chạy trong phạm vi đó không thành vấn đề một khi cả hai thái cực đều xuất hiện. 

Hạn chế là$n \le 2000$. Việc tìm kiếm mạnh mẽ trên tất cả các hoán vị là hoàn toàn không thể bởi vì có$n!$các đơn đặt hàng có thể. Ngay cả việc lập trình động trên các tập hợp con cũng sẽ yêu cầu$2^n$những trạng thái cũng nằm ngoài tầm với. Với$n=2000$, một giải pháp xung quanh$O(n^2)$là thực tế, trong khi$O(n^3)$sẽ là quá chậm. 

Có một số trường hợp khó bỏ sót. 

Coi như:```
1
7
```Câu trả lời là:```
0
```Chỉ có một người chạy nên mọi sự khác biệt đều bằng không. 

Coi như:```
3
5 5 5
```Câu trả lời là:```
0
```Tất cả tốc độ đều giống nhau. Mọi tiền tố đều có mức tối đa bằng mức tối thiểu. 

Coi như:```
3
1 2 100
```Một chiến lược ngây thơ có thể sắp xếp các vận động viên theo thứ tự được sắp xếp. Sự khác biệt trở thành:```
0 + 1 + 99 = 100
```Nhưng thứ tự tối ưu là:```
2, 1, 100
```cho:```
0 + 1 + 99 = 100
```Trong ví dụ này cả hai đều trùng khớp, nhưng trong trường hợp lớn hơn, thứ tự tốt nhất thường không đơn giản là thứ tự được sắp xếp. Chúng ta cần một phương pháp có thể giảm thiểu chi phí tích lũy trên toàn cầu. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi thứ tự có thể có của người chạy. Đối với mỗi hoán vị, chúng tôi có thể quét từ trái sang phải, duy trì tốc độ tối thiểu và tối đa hiện tại, tính toán mọi sai lệch và giữ tổng nhỏ nhất. 

Điều này hiệu quả vì nó trực tiếp kiểm tra mọi sự sắp xếp khả thi. 

Vấn đề là số lượng hoán vị. Với$n=2000$, có$2000!$những đơn đặt hàng có thể, một con số lớn về mặt thiên văn. Ngay cả đối với$n=15$, việc tìm kiếm toàn diện sẽ không thể thực hiện được. 

Để tìm ra cách tiếp cận tốt hơn, chúng ta hãy sắp xếp tốc độ. 

Giả sử tốc độ được sắp xếp là:$$a_0 < a_1 < \cdots < a_{n-1}$$Hãy suy nghĩ về việc xây dựng nhóm người chạy đã chọn dần dần. Sau khi sắp xếp, bất kỳ tập hợp các phần chạy đã được chọn nào cũng phải chiếm một khoảng nào đó trong mảng đã sắp xếp. Nếu chúng ta biết rằng các vận động viên được chọn chính xác là những người ở giữa các vị trí$l$Và$r$, thì sự khác biệt hiện tại của chúng chỉ đơn giản là:$$a_r - a_l$$Bây giờ hãy tưởng tượng việc xây dựng thứ tự cuối cùng ngược lại. 

Ở một số giai đoạn, chúng ta vẫn cần đặt tất cả các vận động viên chạy từ quãng đường$[l,r]$. Sự khác biệt lớn nhất cuối cùng sẽ xuất hiện trong khoảng này chính xác là:$$a_r - a_l$$Khi chúng tôi quyết định điểm cuối nào được thêm vào cuối cùng, vấn đề còn lại sẽ trở thành một trong những khoảng thời gian nhỏ hơn$[l+1,r]$hoặc$[l,r-1]$. 

Cấu trúc khoảng này gợi ý lập trình động trên các phạm vi. 

Cho phép$$dp[l][r]$$là sự khác biệt tối thiểu được đóng góp khi xây dựng tất cả các vận động viên có vị trí được sắp xếp nằm trong$[l,r]$. 

Khi khoảng thời gian$[l,r]$chứa nhiều hơn một đường chạy, độ rộng phạm vi hiện tại$$a_r-a_l$$phải được thanh toán đúng một lần và sau đó chúng tôi tiếp tục với khoảng thời gian nhỏ hơn. 

Điều đó mang lại sự tái phát:$$dp[l][r] = (a_r-a_l) + \min(dp[l+1][r], dp[l][r-1])$$Trường hợp cơ bản là:$$dp[i][i]=0$$bởi vì một người chạy đơn lẻ không đóng góp gì vào sự khác biệt. 

Câu trả lời là$dp[0][n-1]$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| DP tối ưu theo khoảng thời gian |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tốc độ và sắp xếp chúng. 
2. Tạo bảng DP hai chiều trong đó`dp[l][r]`thể hiện tổng đóng góp chênh lệch tối thiểu cần thiết cho khoảng thời gian được sắp xếp từ`l`ĐẾN`r`. 
3. Khởi tạo tất cả các khoảng phần tử đơn bằng 0 vì một đường chạy đơn có độ chênh lệch bằng 0. 
4. Khoảng thời gian xử lý theo thứ tự độ dài tăng dần. 
5. Trong một khoảng thời gian`[l, r]`, tính chiều rộng của nó:$$a_r-a_l$$Chiều rộng này phải xuất hiện một lần khi khoảng được thể hiện đầy đủ. 
6. Sau khi thanh toán chiều rộng này, điểm cuối bên trái hoặc điểm cuối bên phải sẽ được thêm vào cuối cùng. 

Nếu điểm cuối bên trái được thêm vào cuối cùng thì trạng thái trước đó là`[l+1,r]`. 

Nếu điểm cuối bên phải được thêm vào cuối cùng thì trạng thái trước đó là`[l,r-1]`. 
7. Chọn cái tốt hơn trong hai khả năng đó:$$dp[l][r] = (a_r-a_l) + \min(dp[l+1][r],dp[l][r-1])$$8. Tiếp tục cho đến hết khoảng thời gian`[0,n-1]`đã được tính toán. 
9. Đầu ra`dp[0][n-1]`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mỗi tập hợp các đường dẫn đã được kết hợp sẽ tương ứng với một khoảng liền kề của mảng đã được sắp xếp. 

Đối với khoảng thời gian$[l,r]$, sự khác biệt góp phần khi toàn bộ khoảng thời gian này bắt đầu hoạt động chính xác là$a_r-a_l$. Tại thời điểm đó, một trong hai điểm cuối phải được thêm vào cuối cùng. Việc xóa điểm cuối được thêm lần cuối đó sẽ để lại khoảng thời gian$[l+1,r]$hoặc khoảng$[l,r-1]$. 

Sự tái diễn khám phá cả hai khả năng và giữ cái rẻ hơn. Vì mọi trình tự xây dựng hợp lệ cuối cùng sẽ giảm xuống còn một khoảng phần tử duy nhất và mỗi quá trình chuyển đổi đều tính đến sự khác biệt chính xác được đưa ra ở giai đoạn đó, nên DP sẽ tính toán tổng sai lệch tối thiểu có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = sorted(map(int, input().split()))

    dp = [[0] * n for _ in range(n)]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            dp[l][r] = (a[r] - a[l]) + min(
                dp[l + 1][r],
                dp[l][r - 1]
            )

    print(dp[0][n - 1])

solve()
```Bước đầu tiên sắp xếp tốc độ. Sau khi sắp xếp, mọi trạng thái liên quan sẽ trở thành một khoảng liền kề, đây là thuộc tính cấu trúc chính của giải pháp. 

Bảng DP lưu trữ câu trả lời cho tất cả các khoảng thời gian. Các khoảng đơn phần tử vẫn bằng 0 vì không có sự khác biệt khi chỉ có một tốc độ. 

Vòng lặp bên ngoài xử lý các khoảng bằng cách tăng chiều dài. Điều này đảm bảo rằng khi tính toán`dp[l][r]`, cả hai`dp[l+1][r]`Và`dp[l][r-1]`đã được tính toán rồi. 

Sự truy hồi phù hợp trực tiếp với bằng chứng. Độ rộng khoảng`a[r] - a[l]`được thanh toán một lần và sau đó khoảng thời gian tốt hơn trong hai khoảng thời gian trước đó sẽ được chọn. 

Tất cả các giá trị vừa vặn thoải mái trong số nguyên Python. Câu trả lời tối đa có thể là theo thứ tự$n \cdot 10^9$, thấp hơn nhiều so với giới hạn của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
3 1 2
```Mảng được sắp xếp:```
[1, 2, 3]
```| Khoảng thời gian | Chiều rộng | DP trước | Kết quả | 
| --- | --- | --- | --- | 
| [0,1] | 1 | phút(0,0) | 1 | 
| [1,2] | 1 | phút(0,0) | 1 | 
| [0,2] | 2 | phút(1,1) | 3 | 

Câu trả lời cuối cùng:```
dp[0][2] = 3
```Đầu ra:```
3
```Ví dụ này cho thấy mỗi khoảng lớn hơn được xây dựng từ các khoảng nhỏ hơn như thế nào trong khi trả chiều rộng của nó chính xác một lần. 

### Mẫu 2 

đầu vào:```
1
100
```Mảng được sắp xếp:```
[100]
```| Khoảng thời gian | Giá trị | 
| --- | --- | 
| [0,0] | 0 | 

Câu trả lời cuối cùng:```
0
```Đầu ra:```
0
```Điều này chứng tỏ trường hợp cơ bản. Một người chạy đơn lẻ không bao giờ tạo ra sự khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| có$O(n^2)$khoảng thời gian, mỗi khoảng được xử lý trong$O(1)$thời gian | 
| Không gian |$O(n^2)$| Bảng DP lưu trữ một giá trị cho mỗi trạng thái khoảng | 

Với$n \le 2000$, DP chứa khoảng bốn triệu tiểu bang. Mỗi trạng thái chỉ yêu cầu một lượng công việc không đổi, giúp giải pháp đủ nhanh trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = sorted(map(int, input().split()))

    dp = [[0] * n for _ in range(n)]

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length - 1
            dp[l][r] = (a[r] - a[l]) + min(
                dp[l + 1][r],
                dp[l][r - 1]
            )

    return str(dp[0][n - 1]) + "\n"

# provided sample
assert run("3\n3 1 2\n") == "3\n", "sample 1"

# minimum size
assert run("1\n7\n") == "0\n", "single element"

# all equal
assert run("4\n5 5 5 5\n") == "0\n", "all equal"

# simple increasing
assert run("2\n1 10\n") == "9\n", "two elements"

# off-by-one interval handling
assert run("3\n1 2 10\n") == "10\n", "small interval DP"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Vỏ đế một phần tử | 
|`4 / 5 5 5 5`|`0`| Giá trị lặp lại | 
|`2 / 1 10`|`9`| Khoảng không tầm thường nhỏ nhất | 
|`3 / 1 2 10`|`10`| Chuyển đổi chính xác giữa các khoảng thời gian | 

## Vỏ cạnh 

Hãy xem xét:```
1
7
```Mảng được sắp xếp chỉ chứa một phần tử. DP không bao giờ đi vào vòng lặp mở rộng khoảng thời gian.`dp[0][0]`vẫn bằng 0, được trả về ngay lập tức. Đầu ra là:```
0
```Coi như:```
3
5 5 5
```Mỗi chiều rộng khoảng bằng không. 

Ví dụ:```
dp[0][1] = 0
dp[1][2] = 0
dp[0][2] = 0
```Câu trả lời cuối cùng vẫn là 0, phản ánh chính xác rằng mọi tiền tố đều có tốc độ tối đa và tối thiểu giống hệt nhau. 

Coi như:```
2
1 10
```Khoảng không tầm thường duy nhất là`[0,1]`. 

Chiều rộng của nó là:```
10 - 1 = 9
```Cả hai khoảng nhỏ hơn đều là các phần tử đơn lẻ có chi phí bằng 0, vì vậy:```
dp[0][1] = 9
```Câu trả lời chính xác là sự khác biệt của cả bộ. 

Coi như:```
4
1 2 3 100
```DP khám phá cả hai cách thu hẹp từng khoảng thời gian. Nó không cam kết một thứ tự cố định như thứ tự được sắp xếp. Việc lặp lại tự động chọn cách loại bỏ điểm cuối rẻ hơn ở mỗi khoảng thời gian, đảm bảo sự khác biệt tổng thể tối ưu trên toàn cầu thay vì một thỏa thuận hấp dẫn cục bộ.
