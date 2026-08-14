---
title: "CF 102318H - Phần phụ NOI tối đa"
description: "Bài toán yêu cầu chúng ta xử lý một mảng số nguyên và với mỗi giá trị có thể có của (k), hãy xác định có bao nhiêu phần tử mảng có thể được chọn vào một tập hợp các dãy con tăng dần. Mỗi dãy con được chọn phải chứa ít nhất (k) phần tử."
date: "2026-08-13T05:24:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "H"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 240
verified: true
draft: false
---

[CF 102318H - NOI tối đa](https://codeforces.com/problemset/problem/102318/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu chúng ta xử lý một mảng số nguyên và với mỗi giá trị có thể có của (k), hãy xác định có bao nhiêu phần tử mảng có thể được chọn vào một tập hợp các dãy con tăng dần. 

Mỗi dãy con được chọn phải chứa ít nhất (k) phần tử. Hai chuỗi con được chọn phải không trùng nhau trong mảng ban đầu. Nếu một dãy con sử dụng các vị trí từ (i) đến (j), dãy con khác không thể sử dụng bất kỳ vị trí nào giữa (i) và (j), ngay cả khi vị trí đó không được chọn. Mục tiêu là tối đa hóa tổng số phần tử được chọn trên tất cả các chuỗi con. Đầu ra được yêu cầu chứa một câu trả lời cho mọi (k) từ (1) đến (n). Đây là công thức chính xác được sử dụng bởi bài toán UCF Locals ban đầu, trong đó (n\le100) và có thể có tới 50 trường hợp thử nghiệm. 

Giá trị nhỏ (n\le100) làm thay đổi đáng kể mục tiêu thuật toán. Thuật toán bậc ba thực hiện khoảng (100^3=10^6) thao tác cơ bản cho một trường hợp thử nghiệm, điều này hoàn toàn hợp lý, ngay cả khi lặp lại cho tất cả 50 trường hợp trong quá trình triển khai được biên dịch. Thuật toán hàm mũ đã vô vọng ở mức (n=100), vì (2^{100}) là khoảng (1,27\times10^{30}). Do đó, giải pháp dự định sử dụng lập trình động với công việc (O(n^3)). Đánh giá cuộc thi chính thức cũng mô tả giai đoạn tiền xử lý (O(n^3)) cho tất cả các giá trị LIS theo khoảng thời gian, theo sau là một chương trình động (O(n^3)) khác. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai đơn giản hơn không chính xác. Ví dụ: nếu mảng có một phần tử`1 5`, câu trả lời hợp lệ duy nhất là`1`, bởi vì (k=1) phần tử đó tự nó tạo thành một dãy con. Với (k>1) không có dãy con hợp lệ, nhưng không có giá trị nào như vậy của (k) khi (n=1), do đó kết quả đầu ra chỉ đơn giản là`1`. Việc triển khai khởi tạo mọi câu trả lời về 0 mà không xử lý các khoảng phần tử đơn sẽ thất bại ở đây. 

Các giá trị lặp lại là một trường hợp biên khác vì các dãy con phải tăng một cách nghiêm ngặt. Đối với đầu vào`3 / 1 1 1`, đầu ra đúng là`3 0 0`. Với (k=1), mỗi phần tử riêng lẻ có thể tạo thành dãy con riêng của nó, tạo ra ba phần tử được chọn. Tuy nhiên, với (k=2), không có hai phần tử bằng nhau nào tạo thành một dãy con tăng chặt. Quá trình chuyển đổi LIS bất cẩn bằng cách sử dụng`<=`thay vì`<`sẽ tuyên bố không chính xác rằng mảng chứa dãy con tăng dần có độ dài ba. 

Trường hợp cạnh thứ ba xảy ra khi bộ sưu tập tốt nhất không sử dụng phần tử mảng cuối cùng. Coi như`5 / 2 9 1 3 4`. Với (k=2), các dãy con`[2, 9]`Và`[3, 4]`chọn bốn phần tử, trong khi phần tử cuối cùng đã là một phần của`[3,4]`đây. Tổng quát hơn, giải pháp tối ưu cho tiền tố có thể không sử dụng vị trí cuối cùng của nó. Do đó, tiền tố DP phải cho phép chuyển đổi`dp[i] = dp[i-1]`. Việc triển khai nhấn mạnh rằng phần tử cuối cùng thuộc về dãy con cuối cùng có thể làm mất các giải pháp hợp lệ. 

Cuối cùng, các dãy con khác nhau không thể tách rời nhau trong các chỉ số đã chọn của chúng. Toàn bộ phạm vi chỉ số của chúng phải rời rạc. TRONG`2 1 9 3 4 4 5 6`với (k=2), nghiệm tối ưu là`[2,9]`,`[3,4]`,`[4,5,6]`, đưa ra bảy phần tử được chọn. Hai lần xuất hiện của`4`thuộc về các dãy con khác nhau nhưng phạm vi chỉ số của chúng không trùng nhau. Đây là lý do tại sao DP phải chia mảng thành các vùng liền kề và lấy một dãy con tăng dần từ mỗi vùng được chọn thay vì chọn độc lập các bộ chỉ mục rời rạc tùy ý. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp có thể liệt kê mọi tập hợp con của các vị trí mảng, sau đó xác định xem liệu các vị trí đã chọn đó có thể được chia thành các chuỗi con tăng dần hợp lệ có độ dài ít nhất (k) hay không, đồng thời vẫn tôn trọng điều kiện không chồng chéo. Có (2^n) tập hợp con trước khi chúng tôi kiểm tra xem một tập hợp con cụ thể có hợp lệ hay không. Nếu quá trình kiểm tra tính hợp lệ kiểm tra các vị trí đã chọn và các ranh giới có thể có thì phải mất thời gian đa thức, do đó tổng công việc ít nhất là (O(2^n n^2)). Tại (n=100), riêng số lượng tập hợp con đã xấp xỉ (1,27\times10^{30}), khiến cho việc tìm kiếm toàn diện là không thể. 

Lực lượng vũ phu thất bại vì nó coi mọi lựa chọn vị trí đều không liên quan đến mọi lựa chọn khác. Cấu trúc hữu ích là tính không chồng chéo mang lại cho chúng ta sự phân tách từ trái sang phải một cách tự nhiên. Khi dãy con cuối cùng được cố định, mọi thứ trước vị trí bắt đầu của nó là một thể hiện nhỏ hơn độc lập. 

Nhận xét tiếp theo là, nếu chúng ta quyết định rằng một dãy con chiếm khoảng từ vị trí (l) đến vị trí (r), thì không bao giờ có lý do để chọn bất kỳ dãy con nào nhỏ hơn dãy con tăng dài nhất trong khoảng đó. Nếu khoảng đó có LIS có độ dài (L) và (L\ge k), chúng ta có thể sử dụng tất cả các phần tử (L). Việc sử dụng ít phần tử hơn sẽ chỉ làm giảm mục tiêu và không làm cho khoảng tương thích hơn với một dãy con khác, bởi vì dù sao thì không có dãy con nào khác được phép ở trong khoảng đó. 

Điều này làm giảm vấn đề xuống còn hai lớp lập trình động. Đầu tiên, tính toán`lis[l][r]`, độ dài của dãy con tăng dài nhất nằm hoàn toàn trong khoảng liền kề từ (l) đến (r). Có các khoảng (O(n^2)) và LIS DP đơn giản tính toán tất cả các khoảng trong thời gian (O(n^3)). Đây chính xác là chiến lược tiền xử lý được mô tả trong bài đánh giá chính thức. 

Sau đó sửa (k). Cho phép`dp[r]`là số lượng phần tử được chọn tối đa chỉ sử dụng các vị trí`0..r`. Có hai khả năng. Chúng ta có thể để vị trí (r) không được sử dụng, đưa ra`dp[r-1]`. Hoặc dãy con cuối cùng bắt đầu ở vị trí (l) nào đó, chiếm toàn bộ khoảng`[l,r]`, và đóng góp`lis[l][r]`các phần tử. Điều này chỉ được phép khi`lis[l][r] >= k`. Mọi thứ trước (l) đóng góp`dp[l-1]`. Như vậy quá trình chuyển đổi là 

[ 
dp[r]=\max\left(dp[r-1],\max_{0\le l\le r,;lis[l][r]\ge k} 
\left(dp[l-1]+lis[l][r]\right)\right). 
] 

Có (n) giá trị có thể có của (k), (n) điểm cuối bên phải có thể có và (n) điểm bắt đầu có thể có, vì vậy giai đoạn thứ hai này cũng mất (O(n^3)) thời gian. Đánh giá theo phong cách biên tập chính thức trình bày cách phân tách tương tự, xem chuỗi con được chọn cuối cùng dưới dạng LIS của hậu tố sau một số điểm dừng trước đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n^3)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tạo bảng hai chiều`lis`, Ở đâu`lis[l][r]`cuối cùng sẽ chứa độ dài LIS của khoảng`a[l:r+1]`. Chúng ta cần thông tin này vì mỗi dãy con được chọn đều chiếm một phạm vi vị trí liền kề nhau, mặc dù các phần tử được chọn bên trong phạm vi đó không nhất thiết phải liền kề nhau. 
2. Sửa điểm cuối bên trái`l`và kéo dài khoảng thời gian một vị trí tại một thời điểm. Đối với mỗi điểm cuối bên phải mới`r`, tính dãy con tăng dài nhất kết thúc chính xác tại`r`chỉ sử dụng các vị trí`l..r`. Đối với mọi vị trí trước đó`p`, phần tử tại`p`có thể đi trước`a[r]`chính xác khi nào`a[p] < a[r]`. Lấy cực đại so với các số trước đó và thêm một số sẽ cho dãy con tăng dần tốt nhất kết thúc tại`r`. 
3. Giữ độ dài LIS tối đa cho đến nay trong khi kéo dài khoảng thời gian. Điều này mang lại`lis[l][r]`, bởi vì LIS của`[l,r]`hoặc kết thúc tại`r`hoặc kết thúc sớm hơn. 
4. Lặp lại tính toán khoảng thời gian cho mọi trường hợp có thể`l`. Có các khoảng (O(n^2)) và tìm kiếm tiền nhiệm trên mỗi khoảng sẽ đưa ra giai đoạn tiền xử lý (O(n^3)). 
5. Với mọi (k) từ`1`bởi vì`n`, tạo một mảng DP tiền tố. Cho phép`dp[r]`đại diện cho số lượng phần tử tối đa có thể được chọn từ các vị trí`0..r`khi mọi dãy con được chọn có độ dài ít nhất`k`. 
6. Khởi tạo tiền tố DP bằng cách cho phép vị trí hiện tại không được sử dụng. Vì`r > 0`, bắt đầu với`dp[r] = dp[r-1]`. Điều này xử lý các giải pháp có chuỗi con cuối cùng kết thúc trước vị trí`r`. 
7. Hãy thử mọi vị trí bắt đầu có thể`l`cho dãy con cuối cùng kết thúc tại`r`. Nếu như`lis[l][r] >= k`, khoảng có thể cung cấp một dãy con cuối cùng hợp lệ. Đóng góp của nó là`lis[l][r]`, trong khi tiền tố trước nó đóng góp`dp[l-1]`, hoặc bằng 0 khi`l=0`. 
8. Lấy giá trị lớn nhất trong tất cả các lựa chọn của`l`. Sau khi xử lý`r`,`dp[r]`là câu trả lời tối ưu cho tiền tố thông qua`r`. 
9. Cửa hàng`dp[n-1]`như câu trả lời cho điều này`k`, sau đó lặp lại cho giá trị tiếp theo của`k`. 

Tại sao nó hoạt động được rút ra từ sự phân rã chuỗi tiếp theo cuối cùng. Hãy xem xét một giải pháp tối ưu cho tiền tố kết thúc tại`r`. Nếu nó không sử dụng vị trí`r`, giải pháp đã được đại diện bởi`dp[r-1]`. Ngược lại, để dãy con cuối cùng của nó chiếm các vị trí từ`l`bởi vì`r`. Không có dãy con nào được chọn trước đó có thể sử dụng bất kỳ vị trí nào trong khoảng đó, vì vậy tất cả các phần tử được chọn trước đó đều nằm hoàn toàn bên trong`0..l-1`và được đại diện một cách tối ưu bởi`dp[l-1]`. Bên trong`[l,r]`, việc thay thế chuỗi con cuối cùng bằng LIS không thể gây tổn hại gì vì khoảng không có sẵn cho mọi chuỗi con khác và chuỗi con tăng dài hơn sẽ đóng góp nhiều phần tử hơn. Quá trình chuyển đổi xem xét chính xác các khả năng này, do đó nó chứa giải pháp tối ưu và không bao giờ tạo ra sự chồng chéo không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(a):
    n = len(a)

    # lis[l][r] = LIS length inside a[l..r].
    lis = [[0] * n for _ in range(n)]

    for l in range(n):
        ending = [0] * n
        best = 0

        for r in range(l, n):
            cur = 1
            ar = a[r]

            for p in range(l, r):
                if a[p] < ar and ending[p] + 1 > cur:
                    cur = ending[p] + 1

            ending[r] = cur
            if cur > best:
                best = cur

            lis[l][r] = best

    answer = [0] * n

    # Solve the non-overlapping interval problem independently for
    # every required minimum subsequence length k.
    for k in range(1, n + 1):
        dp = [0] * n

        for r in range(n):
            # Leave position r unused.
            if r > 0:
                best = dp[r - 1]
            else:
                best = 0

            # Make [l, r] the interval occupied by the last subsequence.
            for l in range(r + 1):
                length = lis[l][r]

                if length >= k:
                    before = dp[l - 1] if l > 0 else 0
                    value = before + length

                    if value > best:
                        best = value

            dp[r] = best

        answer[k - 1] = dp[n - 1]

    return answer

def main():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        ans = solve_case(a)
        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Phần lồng nhau đầu tiên xây dựng bảng LIS khoảng. Đối với một cố định`l`,`ending[r]`lưu trữ chuỗi con tăng dài nhất kết thúc chính xác tại vị trí`r`. Khi`a[p] < a[r]`, một dãy con tăng dần kết thúc tại`p`có thể được mở rộng bởi`a[r]`. Biến`best`là mức tối đa của tất cả các độ dài kết thúc như vậy gặp phải cho đến nay, chính xác là LIS của khoảng hiện tại. 

Phần chính thứ hai xử lý một giá trị của`k`tại một thời điểm. Sự phân công từ`dp[r - 1]`không phải là việc ghi sổ tùy chọn. Nó thể hiện khả năng việc thu thập tối ưu sẽ dừng lại trước`r`, đó là nguồn phổ biến của các giải pháp không chính xác. 

biểu hiện`dp[l - 1] if l > 0 else 0`xử lý khoảng đầu tiên mà không yêu cầu phần tử trọng điểm. Điều này tránh được trường hợp đặc biệt riêng biệt trong`lis`table trong khi vẫn giữ phép truy toán gần với dạng toán học của nó. 

Sự so sánh`a[p] < ar`phải nghiêm khắc. Các giá trị bằng nhau không thể là các phần tử liên tiếp của một dãy con tăng dần. Số nguyên Python cũng có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên. 

Việc thực hiện tính toán mọi`k`riêng vì (n\le100). Điều này giữ cho định nghĩa trạng thái đơn giản và làm cho đối số chính xác trở nên minh bạch. Tổng số lần chuyển tiếp tiền tố là (O(n^3)), trong khi khoảng thời gian xử lý trước LIS đóng góp một lần khác (O(n^3)). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
8
2 1 9 3 4 4 5 6
2
1 1
3
1 2 3
```Đối với trường hợp thử nghiệm đầu tiên, mảng là`2 1 9 3 4 4 5 6`. Hãy xem xét (k=2). Tiền tố hữu ích DP phát triển như sau. 

|`r`| Khoảng thời gian kết thúc tại`r`được sử dụng làm dãy con cuối cùng |`lis[l][r]`|`dp[r-1]`| Tốt nhất`dp[r]`| 
| --- | --- | --- | --- | --- | 
| 0 |`[2]`| 1 | 0 | 0 | 
| 1 |`[2,9]`| 2 | 0 | 2 | 
| 2 |`[1,9]`| 2 | 2 | 2 | 
| 3 |`[3,4]`| 2 | 2 | 4 | 
| 4 |`[4,5]`| 2 | 4 | 4 | 
| 5 |`[4,5]`hoặc khoảng thời gian hợp lệ khác | 2 | 4 | 4 | 
| 6 |`[4,5]`-loại khoảng | 3 | 4 | 7 | 
| 7 |`[4,5,6]`| 3 | 4 | 7 | 

Giá trị cuối cùng là`7`, thu được bởi`[2,9]`,`[3,4]`, Và`[4,5,6]`. Điều này chứng tỏ tại sao chỉ tính toán một LIS cho toàn bộ mảng là không đủ. LIS toàn cục ngắn hơn tổng số phần tử có thể đạt được từ một số chuỗi con không chồng chéo. Đầu ra hoàn chỉnh cho trường hợp thử nghiệm này là`8 7 6 5 5 0 0 0`. 

### Mẫu 2 

Trường hợp thử nghiệm thứ hai là```
2
1 1
```Đối với (k=1), mỗi phần tử riêng lẻ là một dãy con tăng hợp lệ, do đó cả hai phần tử có thể được chọn riêng biệt. 

|`r`|`dp[r-1]`| Khoảng thời gian cuối cùng hợp lệ |`lis[l][r]`|`dp[r]`| 
| --- | --- | --- | --- | --- | 
| 0 | 0 |`[1]`| 1 | 1 | 
| 1 | 1 |`[1]`| 1 | 2 | 

Đối với (k=2), khoảng duy nhất chứa hai giá trị bằng nhau, do đó LIS nghiêm ngặt của nó có độ dài bằng một. Không tồn tại dãy con hợp lệ và câu trả lời là 0. 

|`r`|`dp[r-1]`| Khoảng thời gian hợp lệ có độ dài ít nhất là 2 |`dp[r]`| 
| --- | --- | --- | --- | 
| 0 | 0 | không | 0 | 
| 1 | 0 | không | 0 | 

Kết quả đầu ra là`2 0`. Trường hợp này xác nhận một cách cụ thể rằng sự bình đẳng không được coi là sự chuyển đổi ngày càng tăng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) | Quá trình tiền xử lý LIS theo khoảng thời gian thực hiện (O(n^3)) và tất cả (n) các vấn đề về tiền tố-DP cùng nhau thực hiện một vấn đề khác (O(n^3)). | 
| Không gian | (O(n^2)) | Bảng LIS khoảng chứa (n^2) giá trị; các mảng DP còn lại chỉ có (O(n)). | 

Với (n\le100), giới hạn bậc ba đủ nhỏ cho nghiệm dự định. Phân tích chính thức xác định rõ ràng quá trình tiền xử lý khoảng thời gian (O(n^3))-LIS là khả thi đối với các giới hạn này. Việc triển khai Python giữ cho các vòng lặp bên trong đơn giản và tránh đệ quy, các cấu trúc tạm thời lớn và tính toán lại nhiều lần các giá trị LIS. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_case(a):
    n = len(a)

    lis = [[0] * n for _ in range(n)]

    for l in range(n):
        ending = [0] * n
        best = 0

        for r in range(l, n):
            cur = 1
            ar = a[r]

            for p in range(l, r):
                if a[p] < ar and ending[p] + 1 > cur:
                    cur = ending[p] + 1

            ending[r] = cur
            if cur > best:
                best = cur

            lis[l][r] = best

    answer = [0] * n

    for k in range(1, n + 1):
        dp = [0] * n

        for r in range(n):
            best = dp[r - 1] if r > 0 else 0

            for l in range(r + 1):
                length = lis[l][r]

                if length >= k:
                    before = dp[l - 1] if l > 0 else 0
                    value = before + length

                    if value > best:
                        best = value

            dp[r] = best

        answer[k - 1] = dp[n - 1]

    return answer

def solution(inp):
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            out.append(" ".join(map(str, solve_case(a))))

        print("\n".join(out))
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
sample1 = """3
8
2 1 9 3 4 4 5 6
2
1 1
3
1 2 3
"""

assert solution(sample1) == """8 7 6 5 5 0 0 0
2 0
3 3 3
""", "provided samples"

# Minimum-size input
assert solution("""1
1
42
""") == "1\n", "single element"

# All equal values
assert solution("""1
3
1 1 1
""") == "3 0 0\n", "strictly increasing requirement"

# Boundary case where the best collection uses separate intervals
assert solution("""1
8
2 1 9 3 4 4 5 6
""") == "8 7 6 5 5 0 0 0\n", "non-overlapping subsequences"

# Maximum-size input, strictly increasing
a = list(range(1, 101))
expected = " ".join(["100"] * 100) + "\n"

assert solution(
    "1\n100\n" + " ".join(map(str, a)) + "\n"
) == expected, "maximum n and fully increasing array"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 42`|`1`| Kích thước tối thiểu và một dãy con hợp lệ | 
|`3 / 1 1 1`|`3 0 0`| Bất bình đẳng nghiêm ngặt trong quá trình chuyển đổi LIS | 
|`8 / 2 1 9 3 4 4 5 6`|`8 7 6 5 5 0 0 0`| Nhiều chuỗi con không chồng chéo | 
|`100 / 1 2 ... 100`| 100 bản sao của`100`| Tối đa (n), trạng thái DP lớn và đầu vào tăng tối đa | 

## Vỏ cạnh 

Đối với đầu vào một phần tử```
1
42
```bảng khoảng chỉ chứa`lis[0][0] = 1`. Đối với (k=1), DP xem xét`[0,0]`, nhìn thấy LIS có độ dài bằng một và thu được`dp[0] = 1`. Đầu ra là`1`. Không có chuỗi con có độ dài bằng 0 nhân tạo nào liên quan. 

Đối với đầu vào hoàn toàn bằng nhau```
3
1 1 1
```mỗi khoảng có độ dài ít nhất hai có độ dài LIS bằng một vì việc so sánh hoàn toàn nghiêm ngặt`<`. Khi (k=1), DP có thể chọn từng khoảng thời gian một cách độc lập, đưa ra`3`. Khi (k=2`or`k=3`, every interval has LIS shorter than the required threshold, so the answer is zero. The output is `3 0 0`. 

Đối với ví dụ không chồng chéo```
8
2 1 9 3 4 4 5 6
```với (k=2), DP trước tiên có thể lấy`[2,9]`, đóng góp hai yếu tố. Sau đó nó có thể bắt đầu sau khoảng thời gian đó và mất`[3,4]`, đóng góp hai cái khác. Cuối cùng,`[4,5,6]`đóng góp ba. Tổng cộng là bảy. Trạng thái tiền tố của DP ghi lại kết quả tốt nhất trước mỗi vị trí bắt đầu, do đó các khoảng thời gian này kết hợp với nhau mà không bao giờ cho phép chuỗi con trước đó xâm nhập vào khoảng thời gian sau. 

Đối với đầu vào tăng kích thước tối đa```
100
1 2 3 ... 100
```toàn bộ mảng đang tăng lên, do đó LIS của nó là 100. Với mỗi (k\le100), toàn bộ mảng đó là một dãy con hợp lệ vì độ dài của nó ít nhất là (k). Vì không có giải pháp nào có thể chọn nhiều hơn tất cả 100 phần tử nên mọi câu trả lời đều chính xác là 100. Trường hợp này thực hiện các kích thước DP lớn nhất đồng thời cung cấp một kiểm tra giới hạn trên đơn giản về tính chính xác.
