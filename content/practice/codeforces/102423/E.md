---
title: "CF 102423E - Hoán vị điểm cố định"
description: "Một hoán vị có kích thước n sắp xếp lại các số 1,…,n sao cho mỗi số xuất hiện đúng một lần. Vị trí i là một điểm cố định khi giá trị đặt ở đó cũng là i."
date: "2026-08-12T01:11:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102423
codeforces_index: "E"
codeforces_contest_name: "North American Southeast Regional 2019 (Div 1)"
rating: 0
weight: 102423
solve_time_s: 115
verified: true
draft: false
---

[CF 102423E - Hoán vị điểm cố định](https://codeforces.com/problemset/problem/102423/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một hoán vị có kích thước n sắp xếp lại các số 1,…,n sao cho mỗi số xuất hiện đúng một lần. Vị trí i là một điểm cố định khi giá trị đặt ở đó cũng là i. Nhiệm vụ đưa ra n, số lượng yêu cầu m điểm cố định và hạng k, đồng thời yêu cầu hoán vị thứ k theo thứ tự từ điển trong số tất cả các hoán vị có chính xác m điểm cố định. Nếu tồn tại ít hơn k hoán vị như vậy, chúng ta sẽ in`-1`. Giới hạn chính thức là 1<n<50, 0<m<n, và 1<k<1018, với giới hạn thời gian một giây. 

Giá trị nhỏ của n là sai lầm. Ngay cả ở mức n=50, vẫn có 50!≈3,04×10 64 hoán vị, do đó việc liệt kê các hoán vị là hoàn toàn không thể. Hạng k được giới hạn ở 10 18, có nghĩa là chúng ta không bao giờ cần phân biệt giữa các số lớn hơn 10 18. Một nghiệm quanh O(n 3 ) dễ dàng đủ nhỏ, trong khi mọi giai thừa hoặc hàm mũ trong n đều bị loại trừ. 

Có một số trường hợp ranh giới trong đó việc triển khai hợp lý bề ngoài có thể thất bại. Vì`1 1 1`, hoán vị duy nhất là`1`, vậy câu trả lời là`1`. Việc triển khai giả định phải có ít nhất hai vị trí để sắp xếp lại có thể từ chối trường hợp này một cách không chính xác. 

Vì`3 2 1`, câu trả lời là`-1`. Hai điểm cố định sẽ chỉ để lại một vị trí và một giá trị, buộc vị trí cuối cùng đó cũng phải cố định. Do đó, chính xác n−1 điểm cố định là không thể với n>1. Một công thức chỉ chọn m vị trí cố định và hoán vị mọi thứ khác có thể vô tình tính một sự sắp xếp có thêm một điểm cố định. 

Vì`3 0 3`, câu trả lời là`-1`. Hai sự biến dạng của ba yếu tố là`231`Và`312`, vì vậy yêu cầu cấp ba nằm ngoài bộ có sẵn. Một cấu trúc tham lam không đếm toàn bộ khối trước khi chọn giá trị có thể đi vào một nhánh không hợp lệ và cuối cùng thất bại mà không biết rằng thứ hạng được yêu cầu chưa bao giờ tồn tại. 

Mẫu thứ ba,`5 3 7`, là một trường hợp ranh giới hữu ích khác. Câu trả lời của nó là`2 1 3 4 5`. Sáu hoán vị hợp lệ đầu tiên bắt đầu bằng`1`, trong khi phần thứ bảy bắt đầu bằng`2`. Thuật toán tham lam chỉ kiểm tra xem một ứng viên có thể được hoàn thành hay không, thay vì đếm xem ứng viên đó có bao nhiêu lần hoàn thành thì không thể xác định chính xác thứ hạng này. Các mẫu chính thức là`3 1 1 -> 1 3 2`,`3 2 1 -> -1`, Và`5 3 7 -> 2 1 3 4 5`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Tạo mọi hoán vị của 1,…,n, đếm các điểm cố định của nó, giữ những điểm đó có chính xác m, sắp xếp chúng theo từ điển và lấy điểm thứ k. Điều này đúng vì mọi hoán vị có thể đều được xem xét và thứ tự cuối cùng chính xác là thứ tự được yêu cầu. Vấn đề là số lượng hoán vị. Trong trường hợp xấu nhất, việc tạo và kiểm tra chúng mất khoảng n⋅n! kiểm tra vị trí cơ bản. Với n=50, tức là khoảng 50⋅50!≈1,52×10 66 lần kiểm tra, vượt xa mọi giới hạn thực tế. 

Quan sát hữu ích là chúng ta không cần phải xây dựng tất cả các hoán vị hợp lệ. Để xác định phần tử tiếp theo của câu trả lời, chúng ta chỉ cần biết mỗi giá trị tiếp theo có thể có bao nhiêu lần hoàn thành hợp lệ. Nếu ứng cử viên nhỏ nhất chiếm ít hơn k hoán vị hợp lệ, chúng ta có thể bỏ qua toàn bộ khối đó và giảm k. Nếu không, ứng viên đó phải là một phần của câu trả lời. 

Câu hỏi còn lại là làm thế nào để đếm những lần hoàn thành đó một cách hiệu quả. Khi tiền tố đã được sửa, một số vị trí còn lại vẫn có thể nhận giá trị riêng, trong khi những vị trí khác thì không thể vì giá trị đó đã được sử dụng. Gọi q là số vị trí còn lại và gọi r là số vị trí còn lại mà giá trị riêng của chúng vẫn có sẵn. Những vị trí r này là những nơi duy nhất có thể xuất hiện các điểm cố định mới. Chúng ta có thể đếm các hoán vị của trạng thái rút gọn này bằng phép lặp quy hoạch động ba chiều. 

Sự truy hồi dựa trên việc chọn một trong r vị trí vẫn còn giá trị riêng. Nếu nó nhận được giá trị riêng, chúng ta tạo một điểm cố định. Nếu không, giá trị của nó phải đến từ nơi khác. Giá trị thay thế đó thuộc về một vị trí đặc biệt khác, loại bỏ khả năng trở thành cố định của vị trí đó hoặc thuộc về một vị trí không đặc biệt. 

Brute-force hoạt động vì việc kiểm tra mọi hoán vị cuối cùng sẽ tìm ra câu trả lời, nhưng không thành công vì về cơ bản có nhiều hoán vị. Nhận xét rằng chỉ số vị trí còn lại và các cặp điểm cố định có thể còn lại mới làm giảm vấn đề đếm xuống O(n 3 ), sau đó việc hủy xếp hạng từ điển sẽ thực hiện một chuyển tiếp trạng thái O(n 2 ) khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n⋅n!) | O(n) bên cạnh các câu trả lời được lưu trữ | Quá chậm | 
| Tối ưu | O(n 3 ) | O(n 3 ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[q][r][t]`là số cách để hoàn thành hoán vị một phần khi vẫn còn q vị trí và giá trị q, chính xác r trong số các vị trí đó vẫn có sẵn giá trị riêng và chính xác t điểm cố định được yêu cầu trong số các vị trí còn lại. 

Tham số thứ ba là số điểm cố định còn cần thiết chứ không phải tổng số điểm đã được thi công. Điều này làm cho DP độc lập với tiền tố cụ thể. 
2. Khởi tạo trạng thái trống bằng`dp[0][0][0] = 1`. Nếu không còn vị trí nào thì sẽ có chính xác một lần hoàn thành hợp lệ khi không cần thêm điểm cố định nào. 
3. Xử lý các trạng thái bằng`r = 0`riêng. Không có vị trí còn lại nào có thể cố định được nên mục tiêu duy nhất có thể là`t = 0`. Mọi hoán vị của các giá trị q còn lại đều hợp lệ, cho`q!`sự hoàn thiện. 
4. Đối với một tiểu bang có`r > 0`, hãy chọn một trong các vị thế vẫn còn giá trị riêng. Có ba khả năng. 

Nếu vị trí đó nhận được giá trị riêng thì một điểm cố định sẽ được tạo. Trạng thái mới là`(q-1, r-1, t-1)`. 

Nếu nó nhận được giá trị riêng của một vị trí đặc biệt khác, thì có`r-1`sự lựa chọn. Giá trị được chọn biến mất nên vị trí tương ứng của nó không còn khả năng cố định nữa, đồng thời bản thân vị trí đã chọn cũng biến mất. Trạng thái mới là`(q-1, r-2, t)`. 

Nếu nó nhận được một giá trị không đặc biệt, thì có`q-r`sự lựa chọn. Chỉ vị trí đặc biệt đã chọn mới biến mất, mang lại`(q-1, r-1, t)`. 

Như vậy sự tái diễn là 

dp[q][r][t]=dp[q−1][r−1][t−1]+(r−1)dp[q−1][r−2][t]+(q−r)dp[q−1][r−1][t]. 
5. Giới hạn mọi giá trị DP ở mức 10 18. Chúng tôi chỉ so sánh số đếm với k, có giá trị tối đa là 10 18, do đó việc phân biệt giữa 10 18 và một số lớn hơn nhiều không ảnh hưởng đến câu trả lời. Giới hạn cũng giữ cho số học nhỏ. 
6. Bắt đầu xây dựng câu trả lời từ vị trí 1. Ban đầu mọi giá trị đều không được sử dụng, vì vậy tất cả n vị trí đều có thể là vị trí điểm cố định. Bộ`r = n`Và`fixed = 0`. 
7. Tại vị trí hiện tại i, thử từng giá trị v chưa sử dụng theo thứ tự tăng dần. Thứ tự này chính xác là thứ mà thứ tự từ điển yêu cầu. 
8. Tạm thời hãy tưởng tượng việc chọn v. Nếu v=i, một điểm cố định bổ sung được tạo ra và số cặp cố định có thể có giảm đi một. 

Nếu v  =i, vị trí hiện tại biến mất, do đó một cặp cố định có thể bị mất. Nếu v>i, giá trị v thuộc về một vị trí trong tương lai và cũng bị loại bỏ, làm mất đi một cặp cố định có thể có khác. Do đó 

r ′ =r−1−[v>i] 

với v  =i, trong khi r ′ =r−1 khi v=i. 
9. Hãy để`need`là số điểm cố định vẫn cần thiết sau khi chọn v. Truy vấn trạng thái DP cho n-i vị trí còn lại. Nếu số đó nhỏ hơn k thì mọi hoán vị bắt đầu bằng ứng cử viên này sẽ xuất hiện trước câu trả lời mong muốn, vì vậy hãy trừ số đó khỏi k và thử giá trị tiếp theo. 
10. Nếu số lượng ít nhất là k, xác nhận v, cập nhật tập giá trị đã sử dụng, số điểm cố định và`r`, sau đó tiếp tục với vị trí tiếp theo. 
11. Nếu không có ứng viên nào có thể chứa thứ hạng mong muốn, hãy xuất ra`-1`. Trong thực tế, điều này có thể được phát hiện ngay từ đầu`dp[n][n][m]`đếm. 

Bất biến chính là trước khi xử lý vị trí i,`r`đếm chính xác các vị trí còn lại mà giá trị riêng của chúng vẫn còn, do đó, những vị trí đó và chỉ những vị trí đó mới có thể trở thành điểm cố định. DP đếm mỗi lần hoàn thành trạng thái rút gọn đó chính xác một lần bằng cách xem xét giá trị được gán cho một vị trí đặc biệt. Trong quá trình xây dựng tham lam, mọi ứng cử viên bị bỏ qua đại diện cho một khối từ điển hoàn chỉnh có kích thước được biết chính xác, do đó, việc trừ đi kích thước của nó sẽ giữ nguyên cách diễn giải k là thứ hạng mong muốn bên trong hậu tố còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def solve_case(n, m, k):
    # dp[q][r][t]:
    # q remaining positions,
    # r positions whose own values are still available,
    # t fixed points still required.
    dp = [[[0] * (n + 1) for _ in range(n + 1)]
          for _ in range(n + 1)]

    dp[0][0][0] = 1

    # States with r = 0 have no possible fixed points.
    for q in range(1, n + 1):
        dp[q][0][0] = min(LIMIT, q * dp[q - 1][0][0])

    for q in range(1, n + 1):
        for r in range(1, q + 1):
            for t in range(0, q + 1):
                value = 0

                # The chosen special position becomes fixed.
                if t >= 1:
                    value += dp[q - 1][r - 1][t - 1]

                # It receives the own value of another special position.
                if r >= 2:
                    value += (r - 1) * dp[q - 1][r - 2][t]

                # It receives a value belonging to a nonspecial position.
                if q > r:
                    value += (q - r) * dp[q - 1][r - 1][t]

                dp[q][r][t] = min(LIMIT, value)

    if dp[n][n][m] < k:
        return "-1"

    used = [False] * (n + 1)
    answer = []

    r = n
    fixed = 0

    for i in range(1, n + 1):
        remaining = n - i

        for v in range(1, n + 1):
            if used[v]:
                continue

            is_fixed = (v == i)
            new_fixed = fixed + is_fixed

            if is_fixed:
                new_r = r - 1
            else:
                new_r = r - 1 - (v > i)

            need = m - new_fixed

            if need < 0 or need > remaining:
                ways = 0
            elif new_r < 0 or new_r > remaining:
                ways = 0
            else:
                ways = dp[remaining][new_r][need]

            if ways < k:
                k -= ways
                continue

            answer.append(v)
            used[v] = True
            fixed = new_fixed
            r = new_r
            break

    return " ".join(map(str, answer))

def main():
    n, m, k = map(int, input().split())
    print(solve_case(n, m, k))

if __name__ == "__main__":
    main()
```DP được xây dựng trước khi hoán vị được xây dựng. các`r = 0`hàng được xử lý trực tiếp vì khi không thể có điểm cố định, mọi hoán vị còn lại đều có thể chấp nhận được miễn là không yêu cầu thêm điểm cố định, tạo ra sự lặp lại giai thừa. 

Vì`r > 0`, ba số hạng tương ứng chính xác với ba trường hợp trong phép truy toán. Giới hạn xung quanh`r - 2`,`t - 1`và các vị trí còn lại ngăn chặn các trạng thái không hợp lệ được truy cập hoặc tính. 

Trong quá trình xây dựng,`used[v]`ghi lại những giá trị đã xuất hiện. Bản cập nhật của`r`là tinh tế. Khi`v == i`, cặp cố định hiện tại chỉ biến mất một lần. Khi`v != i`, vị trí hiện tại sẽ mất cơ hội được cố định và nếu`v > i`, vị thế tương lai của chính nó cũng mất đi cơ hội vì giá trị của nó vừa được tiêu hao. Đây là lý do tại sao biểu thức là`r - 1 - (v > i)`. 

Số nguyên Python không bị tràn, nhưng giới hạn ở mức 10 18 vẫn hữu ích vì tất cả số đếm trên ngưỡng đó đều tương đương để so sánh thứ hạng. DP sử dụng bộ lưu trữ dựa trên số 0 cho số lượng vị trí còn lại, trong khi các vị trí trong hoán vị chính là dựa trên một. Việc tách biệt hai khái niệm đó sẽ tránh được lỗi thường gặp nhất trong giải pháp này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho`n = 3`,`m = 1`,`k = 1`, chúng ta cần hoán vị đầu tiên với đúng một điểm cố định. 

Lúc đầu cả ba vị trí đều có thể được cố định, do đó trạng thái là`(q,r,t) = (3,3,1)`. 

| Vị trí | Ứng viên | Trạng thái còn lại`(q,r,t)`| Hoàn thành | Quyết định | 
| --- | --- | --- | --- | --- | 
| 1 | 1 |`(2,2,0)`| 1 | Bỏ qua, xếp hạng trở thành 0? | 
| 1 | 2 |`(2,1,1)`| 1 | Chọn | 

Ứng viên đầu tiên thực sự có một lần hoàn thành hợp lệ,`123`với ba điểm cố định không được tính vì mục tiêu còn lại bằng 0 và cả hai vị trí còn lại phải tránh bị cố định. Việc hoàn thành hợp lệ là`132`. Vì cấp bậc được yêu cầu là một, ứng viên`1`nên được lựa chọn, sản xuất`132`. 

Dấu vết rõ ràng hơn về chi nhánh thành công thực sự là: 

| Vị trí | Giá trị được chọn | Đã sửa cho đến nay |`r`| Yêu cầu còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 0 | 
| 2 | 3 | 1 | 1 | 0 | 
| 3 | 2 | 1 | 0 | 0 | 

Kết quả là`1 3 2`, phù hợp với mẫu chính thức. 

### Mẫu 2 

cho`n = 3`,`m = 2`, không hoán vị nào có thể có đúng hai điểm cố định. Nếu hai vị trí được cố định thì vị trí cuối cùng còn lại chỉ còn lại giá trị riêng của nó, buộc nó cũng phải cố định. 

| Vị trí | Lý luận của ứng viên | Kết quả | 
| --- | --- | --- | 
| 1 | Bất kỳ tiền tố hợp lệ nào cũng sẽ để lại hai vị trí | Mục tiêu yêu cầu hai điểm cố định | 
| 2 | Sửa vị trí này để lại một vị trí | Vị trí cuối cùng đó bị buộc phải cố định | 
| 3 | Tổng trở thành ba điểm cố định | Mâu thuẫn | 

Trạng thái DP ban đầu`dp[3][3][2]`bằng 0 nên thuật toán in ngay`-1`. Đây chính xác là mẫu chính thức thứ hai. 

### Mẫu 3 

cho`n = 5`,`m = 3`, có mười hoán vị hợp lệ vì chúng ta chọn ba vị trí cố định và sắp xếp trật tự hai vị trí còn lại. 

Ở vị trí một, việc thử giá trị một cho sáu lần hoàn thành hợp lệ, tất cả đều bắt đầu bằng`1`. Từ`k=7`, toàn bộ khối đó bị bỏ qua và thứ hạng trở thành một. Đang thử giá trị hai lá các cặp cố định`3,3`,`4,4`, Và`5,5`có sẵn và vẫn cần có chính xác ba điểm cố định. Chỉ có một sự hoàn thành,`2 1 3 4 5`. 

| Vị trí | Ứng viên |`r`sau khi lựa chọn | Điểm cố định vẫn cần thiết | Hoàn thành | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 3 | 6 | Bỏ qua, k=1 | 
| 1 | 2 | 3 | 3 | 1 | Chọn | 
| 2 | 1 | 3 | 3 | 1 | Chọn | 
| 3 | 3 | 2 | 2 | 1 | Chọn | 
| 4 | 4 | 1 | 1 | 1 | Chọn | 
| 5 | 5 | 0 | 0 | 1 | Chọn | 

Hoán vị kết quả là`2 1 3 4 5`, hoán vị hợp lệ thứ bảy theo thứ tự từ điển, khớp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n 3 ) | Có O(n 3 ) trạng thái DP, mỗi trạng thái được tính toán trong thời gian không đổi, trong khi việc xây dựng tham lam kiểm tra tối đa O(n 2 ) ứng viên | 
| Không gian | O(n 3 ) | DP chứa tối đa 51 3 trạng thái nguyên | 

Với n<50, DP chỉ có khoảng 132.651 trạng thái và việc xây dựng kiểm tra tối đa 1.275 giá trị ứng cử viên. Giới hạn một giây dễ dàng tương thích với lượng công việc này và mức sử dụng bộ nhớ rất nhỏ so với giới hạn 512 MB. 

## Trường hợp thử nghiệm 

Mã kiểm tra sau đây độc lập và sử dụng cùng một mã`solve_case`thực hiện như giải pháp được đệ trình.```python
import sys
import io

LIMIT = 10**18

def solve_case(n, m, k):
    dp = [[[0] * (n + 1) for _ in range(n + 1)]
          for _ in range(n + 1)]

    dp[0][0][0] = 1

    for q in range(1, n + 1):
        dp[q][0][0] = min(LIMIT, q * dp[q - 1][0][0])

    for q in range(1, n + 1):
        for r in range(1, q + 1):
            for t in range(q + 1):
                value = 0

                if t >= 1:
                    value += dp[q - 1][r - 1][t - 1]

                if r >= 2:
                    value += (r - 1) * dp[q - 1][r - 2][t]

                if q > r:
                    value += (q - r) * dp[q - 1][r - 1][t]

                dp[q][r][t] = min(LIMIT, value)

    if dp[n][n][m] < k:
        return "-1"

    used = [False] * (n + 1)
    answer = []
    r = n
    fixed = 0

    for i in range(1, n + 1):
        remaining = n - i

        for v in range(1, n + 1):
            if used[v]:
                continue

            is_fixed = v == i
            new_fixed = fixed + is_fixed

            if is_fixed:
                new_r = r - 1
            else:
                new_r = r - 1 - (v > i)

            need = m - new_fixed

            if need < 0 or need > remaining:
                ways = 0
            elif new_r < 0 or new_r > remaining:
                ways = 0
            else:
                ways = dp[remaining][new_r][need]

            if ways < k:
                k -= ways
            else:
                answer.append(v)
                used[v] = True
                fixed = new_fixed
                r = new_r
                break

    return " ".join(map(str, answer))

def run(inp: str) -> str:
    n, m, k = map(int, inp.split())
    return solve_case(n, m, k)

# Provided samples
assert run("3 1 1") == "1 3 2", "sample 1"
assert run("3 2 1") == "-1", "sample 2"
assert run("5 3 7") == "2 1 3 4 5", "sample 3"

# Minimum-size inputs
assert run("1 1 1") == "1", "only permutation with one fixed point"
assert run("1 0 1") == "-1", "one element cannot be a derangement"

# Smallest nontrivial derangement
assert run("2 0 1") == "2 1", "unique derangement of size 2"

# Rank just beyond the available permutations
assert run("3 0 3") == "-1", "there are only two derangements of size 3"

# Maximum-size input with all positions fixed
assert run("50 50 1") == " ".join(map(str, range(1, 51))), \
    "identity permutation at maximum n"

# Exactly n-1 fixed points is impossible
assert run("5 4 1") == "-1", "cannot have exactly n-1 fixed points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Kích thước tối thiểu và ranh giới nhận dạng | 
|`1 0 1`|`-1`| Trường hợp không thể có kích thước tối thiểu | 
|`2 0 1`|`2 1`| Biến dạng nhỏ nhất | 
|`3 0 3`|`-1`| Xếp hạng vượt quá số lượng hoán vị hợp lệ | 
|`50 50 1`|`1 2 ... 50`| Tối đa n, tất cả các vị trí cố định | 
|`5 4 1`|`-1`| Không thể có chính xác n−1 điểm cố định | 

## Vỏ cạnh 

cho`1 1 1`, trạng thái ban đầu là`dp[1][1][1] = 1`. Ứng cử viên duy nhất là giá trị`1`, tạo điểm cố định cần thiết và rời khỏi trạng thái`(0,0,0)`. DP báo cáo một lần hoàn thành, vì vậy câu trả lời là`1`. Không có cách xử lý đặc biệt nào cần thiết cho một hoán vị đơn lẻ ngoài trường hợp cơ sở chính xác. 

Vì`3 2 1`, trạng thái ban đầu`dp[3][3][2]`là số không. Phép truy toán tự động nắm bắt lý do cấu trúc: khi hai điểm cố định được tạo, vị trí còn lại không có giá trị thay thế và cũng trở nên cố định. Do đó, thuật toán in`-1`trước khi cố gắng xây dựng một hoán vị. 

Vì`3 0 3`, số DP ban đầu là hai, tương ứng với`2 3 1`Và`3 1 2`. Vì cấp bậc được yêu cầu là ba và`2 < 3`, thuật toán báo cáo ngay`-1`. Ranh giới này đặc biệt hữu ích để kiểm tra xem sự so sánh có`count < k`, không`count <= k`. 

Vì`5 3 7`, ứng cử viên đầu tiên ở vị trí một là`1`. Có sáu lần hoàn thành hợp lệ bắt đầu bằng giá trị đó, vì vậy thuật toán thay đổi k từ bảy thành một và loại bỏ toàn bộ tiền tố. Ứng viên`2`có đúng một lần hoàn thành hợp lệ nên nó được chọn. Lực lượng nhà nước còn lại`1`ở vị trí hai và`3,4,5`vào vị trí của mình, tạo ra`2 1 3 4 5`. Trường hợp này thực hiện cả việc bỏ qua khối từ điển và cập nhật số lượng điểm cố định có thể có trong tương lai. 

Vì`50 50 1`, mọi vị trí đều phải cố định. DP có chính xác một lần hoàn thành hợp lệ và thủ tục tham lam chấp nhận giá trị khả dụng nhỏ nhất ở mọi vị trí. Kết quả là sự hoán vị danh tính từ`1`bởi vì`50`, xác nhận rằng n lớn nhất được phép không gây ra bất kỳ hành vi kích thước giai thừa nào. 

Vì`5 4 1`, chính xác bốn điểm cố định sẽ buộc điểm thứ năm cũng phải cố định. DP trả về 0 cho mục tiêu này, do đó thuật toán sẽ từ chối nó mà không thử đặt vị trí cuối cùng không đúng định dạng. Đây là trường hợp không thể xảy ra tổng quát m=n−1 với mọi n>1.
