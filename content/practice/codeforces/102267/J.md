---
title: "CF 102267J - Sở thú"
description: "Sở thú là một vòng tròn gồm (n) địa điểm. Một công dân chọn vị trí bắt đầu (a), hướng đi vòng quanh xe đạp và một đường đi đơn giản có độ dài tối đa (k). Cuộc đi bộ phải ở bên trong con đường đã chọn đó, quay trở lại (a) và ghé thăm mọi vị trí của con đường."
date: "2026-08-19T03:44:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102267
codeforces_index: "J"
codeforces_contest_name: "The 2019 University of Jordan Collegiate Programming Contest"
rating: 0
weight: 102267
solve_time_s: 436
verified: false
draft: false
---

[CF 102267J - Sở thú](https://codeforces.com/problemset/problem/102267/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 16 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Sở thú là một vòng tròn gồm (n) địa điểm. Một công dân chọn vị trí bắt đầu (a), hướng đi vòng quanh xe đạp và một đường đi đơn giản có độ dài tối đa (k). Cuộc đi bộ phải ở bên trong con đường đã chọn đó, quay trở lại (a) và ghé thăm mọi vị trí của con đường. Số lần di chuyển của nó không được vượt quá (m). Nhiệm vụ là đếm tất cả các bước đi có thể theo modulo (10^9+7). 

Cách hữu ích để tạm thời quên đi chu trình này là nhìn vào một hướng đã chọn từ (a). Dán nhãn (a) là vị trí (0), vị trí tiếp theo là (1), v.v. Khi đó, một bước đi hợp lệ sẽ trở thành một bước đi trên đoạn số nguyên từ (0) đến một vị trí tối đa nào đó (d), trong đó mỗi bước di chuyển sẽ thay đổi vị trí hiện tại theo (1) hoặc (-1). Cuộc đi bộ bắt đầu và kết thúc ở (0), không bao giờ xuống dưới (0) và vị trí tối đa của nó nhiều nhất là (k). 

Có một điểm tinh tế ở đây. Chúng ta không cần phải chọn (d) một cách rõ ràng. Nếu một bước đi đạt đến vị trí tối đa (d), thì đường đi đã chọn có độ dài chính xác bằng độ dài đó, vì vị trí cuối cùng của đường đi phải được ghé thăm. Do đó, việc đếm các bước đi nằm trong khoảng từ (0) đến (k) sẽ tự động đếm mọi đường đi có thể được chọn chính xác một lần. Hướng và vị trí bắt đầu sau đó có thể được khôi phục theo hệ số (2n). 

Ràng buộc (n\le 10^5) loại trừ mọi thứ lặp lại trên mọi vị trí bắt đầu và mọi lần đi bộ có thể. Quan trọng hơn, (m\le2000) là tham số nhỏ giúp lập trình động có thể thực hiện được. Vì một quãng đường có độ dài (i) không bao giờ có thể xa hơn (i) tính từ điểm bắt đầu của nó, nên các vị trí liên quan tại thời điểm (i) chỉ là (0,\ldots,\min(i,k)). Tính tổng trên tất cả (i\le m), cho ra (O(m^2)) trạng thái, nhiều nhất là khoảng bốn triệu khi (m=2000). 

Một sai lầm dễ mắc phải là đếm điểm cuối thay vì đếm bước di chuyển. Ví dụ: với đầu vào (4\ 3\ 3), bước đi khép kín không trống duy nhất có thể có độ dài (2), cụ thể là di chuyển đến vị trí lân cận và quay lại ngay lập tức. Có (4) vị trí xuất phát và (2) hướng đi nên đáp án là (8). Việc coi bước đi đó có chiều dài (3) sẽ loại bỏ nó một cách không chính xác. Đầu ra đúng là (8). 

Một sai lầm khác là cho rằng bước đi phải đạt chính xác khoảng cách (k). Ví dụ: với (n=5,k=4,m=4), bước đi (0\to1\to0) là hợp lệ ngay cả khi khoảng cách tối đa của nó chỉ là (1). Đường dẫn được chọn thực tế của nó có độ dài (1), được phép vì yêu cầu tối đa là (k). Do đó, DP phải tính tất cả các lần đi bộ có mức tối đa là nhiều nhất (k), không chỉ các lần đi bộ đạt đến (k). 

Trường hợp ranh giới thứ ba xảy ra khi (k=1). Chuyển động duy nhất có thể xảy ra là giữa các vị trí (0) và (1), do đó bước đi hợp lệ chỉ tồn tại trong các khoảng thời gian chẵn. Với (n=2,k=1,m=2), có chính xác một bước đi cho mỗi (2n=4) lựa chọn về điểm xuất phát và hướng, cho kết quả (4). Sự lặp lại vô tình cho phép vị trí (2) sẽ vượt quá trường hợp này. 

## Phương pháp tiếp cận 

Một giải pháp vũ lực trực tiếp có thể chọn vị trí bắt đầu, một trong hai hướng, sau đó liệt kê mọi chuỗi di chuyển có thể có chiều dài (m). Mỗi bước có nhiều nhất hai lựa chọn nên số dãy có độ dài từ (1) đến (m) là 

[ 
2+2^2+\cdots+2^m=2^{m+1}-2. 
] 

Có (2n) lựa chọn về vị trí và hướng xuất phát. Trong trường hợp xấu nhất điều này có nghĩa là đại khái 

[ 
2n(2^{m+1}-2) 
] 

trình tự ứng cử viên. Với (n=10^5) và (m=2000), điều này hoàn toàn không thể thực hiện được. Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng mọi bước đi có thể, nhưng số lần đi bộ theo cấp số nhân mới là vấn đề. 

Cấu trúc của một cuộc dạo chơi cho chúng ta một không gian trạng thái nhỏ hơn nhiều. Khi điểm bắt đầu và hướng đã được xác định, chu trình chỉ là một đoạn thẳng. Tại thời điểm (i), thông tin duy nhất cần thiết để tiếp tục chuyến đi là khoảng cách hiện tại (j) tính từ điểm xuất phát. Từ (j), vị trí tiếp theo là (j-1) hoặc (j+1). Các vị trí dưới (0) bị cấm, trong khi các vị trí trên (k) bị cấm.

Điều này dẫn trực tiếp đến sự lặp lại lập trình động. Gọi (dp[i][j]) là số bước đi có độ dài-(i) bắt đầu tại (0), không bao giờ rời khỏi ([0,k]) và kết thúc tại vị trí (j). Sự chuyển tiếp là 

[ 
dp[i][j]=dp[i-1][j-1]+dp[i-1][j+1]. 
] 

Tại vị trí (0), số hạng đầu tiên không tồn tại vì việc di chuyển đến (-1) bị cấm. Tại vị trí (k), việc chuyển từ (k) sang (k+1) bị cấm. 

Một cuộc đi bộ được đóng lại chính xác khi vị trí cuối cùng của nó là (0). Chúng ta tính tổng (dp[i][0]) trên tất cả (1\le i\le m). Vị trí xuất phát có (n) lựa chọn và hướng đi có (2) lựa chọn nên kết quả cuối cùng được nhân với (2n). 

Độ phức tạp rõ ràng (O(mk)) cũng tốt hơn so với vẻ ngoài ban đầu. Tại thời điểm (i), không thể đạt được vị trí lớn hơn (i), do đó chỉ có vị trí (\min(i,k)+1) là phù hợp. Vì (m\le2000), tổng số lần chuyển đổi là (O(m\min(m,k))=O(m^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^m)) | (O(m)) | Quá chậm | 
| DP tối ưu | (O(m\min(m,k))) | (O(\min(m,k))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n,k,m). Trước tiên, chúng tôi sẽ đếm số bước đi cho một vị trí bắt đầu cố định và một hướng cố định, sau đó khôi phục các lựa chọn đối xứng (2n) ở cuối. 
2. Biểu diễn hướng đã chọn dưới dạng hệ tọa độ một chiều. Vị trí bắt đầu có tọa độ (0), di chuyển dọc theo hướng đã chọn sẽ làm tăng tọa độ và di chuyển về phía sau sẽ làm giảm tọa độ. Vì đường đi đã chọn có độ dài tối đa là (k), nên tọa độ hợp lệ là (0,\ldots,k). 
3. Khởi tạo (dp[0][0]=1). Trước khi thực hiện bất kỳ nước đi nào, có chính xác một bước đi có độ dài bằng 0 và nó ở vị trí bắt đầu. 
4. Với mọi thời điểm (i) từ (1) đến (m), hãy tính số cách để đến mọi vị trí có thể có (j). Sự tái phát là 
[ 
dp[i][j]=dp[i-1][j-1]+dp[i-1][j+1]. 
] 
Vị trí (j) chỉ có thể đạt được từ một trong hai vị trí lân cận của nó tại thời điểm trước đó. 
5. Hạn chế (j) thành (0\le j\le\min(i,k)). Một cuộc đi bộ có chiều dài (i) không thể đạt được khoảng cách lớn hơn (i), vì vậy các vị trí ngoài (i) là không cần thiết. Giới hạn dưới (0) ngăn việc đi bộ rời khỏi đường đã chọn ở điểm cuối xuất phát. 
6. Sau khi tính toán toàn bộ lớp thời gian, hãy thêm (dp[i][0]) vào câu trả lời. Kết thúc ở (0) có nghĩa là công dân đã quay trở lại vị trí xuất phát, vì vậy mọi trạng thái như vậy đều là một chuyến đi khép kín hợp lệ. 
7. Nhân số tích lũy với (2n). Có (n) vị trí có thể bắt đầu và hai hướng xung quanh chu trình. Mỗi bước đi một chiều được tính sẽ xác định chính xác một trong những lựa chọn này và mọi lựa chọn như vậy đều có cùng số bước đi. 

### Tại sao nó hoạt động 

Đối với vị trí và hướng bắt đầu cố định, bất biến là (dp[i][j]) tính chính xác chiều dài-(i) bước đi vẫn nằm trong đoạn được phép và kết thúc tại tọa độ (j). Phép truy toán xem xét chính xác hai tọa độ có thể có trước đó, trong khi hạn chế (j\ge0) và (j\le k) loại bỏ mọi chuyển động bên ngoài đường dẫn đã chọn. Do đó, (dp[i][0]) tính chính xác các bước đi khép kín hợp lệ có độ dài (i). 

Một bước đi được tính có thể có tọa độ tối đa (d<k), nhưng điều đó đúng. Đường dẫn được chọn thực tế của nó kết thúc tại tọa độ (d), vẫn nằm trong mức tối đa cho phép (k). Vì điểm cuối của đường đi đã chọn phải được truy cập, (d) được xác định duy nhất bởi tọa độ tối đa đạt được khi đi bộ. Do đó, DP không tính cùng một bước đi cho nhiều độ dài đường đi. Cuối cùng, mỗi bước đi có thể được đặt tại bất kỳ (n) vị trí bắt đầu nào và đi theo một trong hai hướng, cho ra hệ số (2n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k, m = map(int, input().split())

    limit = min(k, m)

    prev = [0] * (limit + 2)
    prev[0] = 1

    ans = 0

    for i in range(1, m + 1):
        cur = [0] * (limit + 2)
        upper = min(i, k)

        for j in range(upper + 1):
            ways = 0

            if j > 0:
                ways += prev[j - 1]

            if j + 1 <= limit:
                ways += prev[j + 1]

            cur[j] = ways % MOD

        ans += cur[0]
        ans %= MOD
        prev = cur

    ans = ans * (2 * n) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```DP sử dụng hai mảng thay vì lưu trữ tất cả (m) lớp.`prev[j]`đại diện cho bước thời gian trước đó và`cur[j]`đại diện cho cái hiện tại. Điều này làm giảm bộ nhớ từ (O(m^2)) xuống (O(m)). 

biểu hiện`upper = min(i, k)`là tối ưu hóa ranh giới quan trọng. Tại thời điểm (i), tọa độ (j>i) không thể truy cập được, trong khi tọa độ (j>k) bị cấm bởi đường dẫn đã chọn. Không cần tính toán các vị trí khác. 

Vì`j > 0`, vị trí trước đó có thể là`j - 1`. Vì`j + 1 <= limit`, vị trí trước đó có thể là`j + 1`. Khi`j == 0`, quá trình chuyển đổi đầu tiên được cố tình bỏ qua vì việc di chuyển từ (0) sang (-1) sẽ rời khỏi đường dẫn đã chọn. 

Khe cắm mảng bổ sung tại`limit + 1`không được sử dụng như một trạng thái hợp lệ. Nó chỉ đơn giản là cho phép tái phát đọc`prev[j + 1]`an toàn khi`j == limit`, trong đó giá trị đó vẫn bằng 0. 

Số nguyên Python không bị tràn, nhưng việc giảm từng mô-đun trạng thái (10^9+7) sẽ giữ cho các giá trị trung gian ở mức nhỏ và tuân theo mô-đun đầu ra được yêu cầu. Phép nhân cuối cùng với (2n) cũng được thực hiện theo modulo (10^9+7). 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`4 3 3`. Vì chiều dài bước đi tối đa là (3), nên các vị trí có thể có tại mỗi thời điểm được hiển thị bên dưới. 

| Thời gian (i) | Vị trí có thể tiếp cận (j) | Giá trị DP | (dp[i][0]) | Số tích lũy | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | (1) | (1) | (0) | 
| 1 | 0, 1 | (0,1) | (0) | (0) | 
| 2 | 0, 1, 2 | (1,0,1) | (1) | (1) | 
| 3 | 0, 1, 2, 3 | (0,2,0,1) | (0) | (1) | 

Chỉ có thể thực hiện một lần đi bộ khép kín cho một điểm xuất phát và hướng cố định trong vòng ba lần di chuyển, cụ thể là đi đến vị trí liền kề và quay lại. Có (4\cdot2=8) lựa chọn điểm xuất phát và hướng, nên đáp án là (8). 

Đối với Mẫu 2, đầu vào là`10 5 6`. Giới hạn trên (k=5) không ảnh hưởng đến DP trong sáu bước này vì một bước đi khép kín có độ dài (6) có thể đạt tới khoảng cách tối đa (3). 

| Thời gian (i) | (dp[i][0]) | (dp[i][1]) | (dp[i][2]) | (dp[i][3]) | Số tích lũy | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 0 | 
| 1 | 0 | 1 | 0 | 0 | 0 | 
| 2 | 1 | 0 | 1 | 0 | 1 | 
| 3 | 0 | 2 | 0 | 1 | 1 | 
| 4 | 2 | 0 | 3 | 0 | 3 | 
| 5 | 0 | 5 | 0 | 4 | 3 | 
| 6 | 5 | 0 | 9 | 0 | 8 | 

Đối với một điểm xuất phát và hướng cố định, có (1+2+5=8) các bước đi khép kín có độ dài (2,4,6). Hệ số (2n=20) cho ra (8\cdot20=160), khớp với kết quả đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\min(m,k))) | Tại thời điểm (i), chỉ có thể đạt được từ (0) đến (\min(i,k)). | 
| Không gian | (O(\min(m,k))) | Chỉ các lớp DP trước đó và hiện tại được lưu trữ. | 

Vì (m\le2000), số lần chuyển đổi trạng thái nhiều nhất vào khoảng bốn triệu. Thuật toán không bao giờ lặp lại trên tất cả (n) vị trí riêng lẻ, vì vậy (n) chỉ xuất hiện trong phép nhân cuối cùng với (2n). Điều này thoải mái phù hợp với giới hạn (10^5) trên (n) và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the algorithm on an input string and return its output
import sys
import io

MOD = 10**9 + 7

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    n, k, m = data

    limit = min(k, m)
    prev = [0] * (limit + 2)
    prev[0] = 1

    ans = 0

    for i in range(1, m + 1):
        cur = [0] * (limit + 2)
        upper = min(i, k)

        for j in range(upper + 1):
            if j > 0:
                cur[j] += prev[j - 1]
            if j + 1 <= limit:
                cur[j] += prev[j + 1]
            cur[j] %= MOD

        ans = (ans + cur[0]) % MOD
        prev = cur

    return str(ans * (2 * n) % MOD)

# Provided samples
assert solve_data("4 3 3") == "8", "sample 1"
assert solve_data("10 5 6") == "160", "sample 2"

# Minimum feasible n, and an odd maximum length.
assert solve_data("2 1 1") == "0", "no nonempty closed walk of odd length"

# Maximum n with the smallest useful k and m.
assert solve_data("100000 1 2") == "400000", "maximum n boundary"

# The path limit is irrelevant here because m=4 cannot reach beyond distance 2.
assert solve_data("5 4 4") == "30", "Catalan walks of lengths 2 and 4"

# k=2 removes walks that would need to reach distance 3.
assert solve_data("3 2 6") == "42", "upper-bound restriction"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`0`| Khả thi tối thiểu (n), độ dài lẻ không thể quay lại từ đầu | 
|`100000 1 2`|`400000`| Độ rộng đường dẫn tối đa (n) và nhỏ nhất | 
|`5 4 4`|`30`| Nhiều độ dài bước đi khép kín hợp lệ và thực tế là (k) là giới hạn trên | 
|`3 2 6`|`42`| Lượt đi đạt (k+1) phải bị từ chối | 

## Vỏ cạnh 

Khi độ dài đường dẫn tối đa là (k=1), bước đi chỉ có thể xen kẽ giữa các vị trí (0) và (1). Đối với đầu vào`2 1 2`, DP có`dp[2][0] = 1`, trong khi tất cả các trạng thái có độ dài lẻ ở vị trí (0) đều bằng 0. Số tích lũy là (1) và nhân với (2n=4) sẽ cho ra kết quả`4`. Ranh giới trên được tôn trọng vì quá trình chuyển đổi từ vị trí (1) chỉ có thể quay lại vị trí (0). 

Khi quãng đường đi là số lẻ thì việc quay lại điểm xuất phát là không thể. Vì`2 1 1`, nước đi đầu tiên phải đi từ (0) đến (1), vì vậy`dp[1][0] = 0`và câu trả lời là`0`. Tổng quát hơn, mỗi bước di chuyển sẽ thay đổi tính chẵn lẻ của tọa độ hiện tại, do đó bước đi bắt đầu từ (0) chỉ có thể trở về (0) sau một số lần di chuyển chẵn. 

Khi (k) lớn hơn mọi khoảng cách có thể tiếp cận, ranh giới trên không bao giờ ảnh hưởng đến kết quả. Vì`5 4 4`, độ dài lối đi khép kín duy nhất là (2) và (4). Có một bước đi có chiều dài (2) và hai chiều dài (4), đưa ra (3) bước đi để có điểm xuất phát và hướng cố định. Hệ số (2n=10) tạo ra`30`. Điều này cũng chứng tỏ tại sao DP phải chấp nhận đi bộ có khoảng cách tối đa nhỏ hơn (k). 

Khi (k) bị hạn chế, DP phải loại bỏ các lối đi vượt qua ranh giới đó. Vì`3 2 6`, các bước đi khép kín không âm không giới hạn có độ dài (2,4,6) số (1,2,5), nhưng trong số năm bước đi có chiều dài-(6), bước đi đến vị trí (3) bị cấm vì (k=2). Do đó, chỉ còn lại (1+2+4=7) bước đi cho điểm xuất phát và hướng cố định. Có (2n=6) các lựa chọn như vậy, cho`42`. 

Cuối cùng, thực tế là (k<n) là thứ cho phép đường dẫn đã chọn được coi là một đoạn đường thông thường thay vì vô tình quấn hết chu trình. DP chỉ theo dõi khoảng cách dọc theo hướng được chọn rõ ràng và hệ số (2n) khôi phục tính đối xứng quay và hướng của chu trình mà không liệt kê (n) vị trí của nó.
