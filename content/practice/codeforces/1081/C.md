---
title: "CF 1081C - Gạch nhiều màu sắc"
description: "Chúng ta đang vẽ một hàng các viên gạch $n$, mỗi viên gạch chọn một trong số $m$ màu có sẵn. Sau khi vẽ xong, chúng ta xem xét các vị trí có màu sắc thay đổi so với viên gạch trước đó."
date: "2026-06-15T06:13:44+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1081
codeforces_index: "C"
codeforces_contest_name: "Avito Cool Challenge 2018"
rating: 1500
weight: 1081
solve_time_s: 319
verified: true
draft: false
---

[CF 1081C - Gạch đầy màu sắc](https://codeforces.com/problemset/problem/1081/C) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, dp, toán 
**Thời gian giải:** 5 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang vẽ một đường$n$gạch, mỗi viên gạch chọn một trong$m$màu sắc có sẵn. Sau khi vẽ xong, chúng ta xem xét các vị trí có màu sắc thay đổi so với viên gạch trước đó. Chính thức, chúng tôi quét từ trái sang phải và đếm xem có bao nhiêu chỉ số$i \ge 2$thỏa mãn$a_i \ne a_{i-1}$. Số lượng đó được yêu cầu phải chính xác$k$. 

Nhiệm vụ là đếm chính xác có bao nhiêu màu đầy đủ của toàn bộ chuỗi tạo ra$k$những “điểm thay đổi” như vậy và trả về kết quả theo modulo$998244353$. 

Một cách hữu ích để diễn giải cấu trúc là coi mảng như được chia thành các khối liền kề có màu bằng nhau. Mỗi lần chúng ta có một điểm thay đổi, một khối mới sẽ bắt đầu. Vì vậy nếu có$k$các vị trí có màu khác với vị trí trước đó thì mảng bao gồm chính xác$k+1$các đoạn đơn sắc. 

Những hạn chế$n, m \le 2000$đề nghị rằng một$O(n^2)$hoặc$O(n^2 m)$Phương pháp quy hoạch động là hợp lý, trong khi bất cứ điều gì theo cấp số nhân trên tất cả các màu đều không thể thực hiện được ngay lập tức vì số lượng các màu là$m^n$, tăng trưởng quá nhanh ngay cả đối với mức độ vừa phải$n$. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong tình huống này, tất cả các viên gạch phải có cùng màu, điều này mang lại chính xác$m$các cấu hình hợp lệ. Một trường hợp ranh giới khác là$k = n-1$, trong đó mỗi cặp liền kề khác nhau, buộc phải có sự xen kẽ màu sắc nghiêm ngặt, điều này hạn chế rất nhiều sự chuyển đổi nhưng vẫn cho phép nhiều lựa chọn tùy thuộc vào$m$. 

Một sai lầm ngây thơ là đối xử với từng$k$thay đổi vị trí một cách độc lập, nhưng các ràng buộc mang tính tổng thể: một khi một màu được chọn, các lựa chọn trong tương lai sẽ phụ thuộc vào màu trước đó, do đó các giả định về tính độc lập sẽ bị phá vỡ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ liệt kê tất cả$m^n$màu và đếm chính xác có bao nhiêu$k$chuyển tiếp. Điều này đơn giản về mặt khái niệm: tạo ra mọi mảng và đếm các thay đổi trong$O(n)$, cho độ phức tạp tổng thể$O(n m^n)$. Điều này thất bại ngay lập tức vì thậm chí$m=2, n=2000$có kích thước lớn về mặt thiên văn. 

Quan sát quan trọng là cấu trúc chỉ phụ thuộc vào việc chúng ta tiếp tục một phân khúc hay bắt đầu một phân khúc mới. Chúng ta không bao giờ cần theo dõi toàn bộ lịch sử tô màu, chỉ màu của viên gạch trước đó và bao nhiêu lần chuyển đổi đã xảy ra cho đến nay. Điều này dẫn đến việc lập trình động theo các vị trí một cách tự nhiên. 

Cho phép$dp[i][j]$là số cách tô màu đầu tiên$i$gạch chính xác như vậy$j$những chuyển tiếp đã xảy ra. Khi chúng ta đặt gạch$i$, chúng ta có thể khớp màu trước đó (không có chuyển tiếp mới) hoặc chọn một màu khác (một chuyển tiếp mới). Điểm quan trọng là trong trường hợp “khác màu”, chúng ta có$m-1$các lựa chọn bất kể màu trước đó là gì, điều này cho phép chúng ta thu gọn sự phụ thuộc vào giá trị màu thực tế. 

Điều này làm giảm vấn đề từ việc theo dõi các trạng thái màu đầy đủ sang chỉ theo dõi số lần chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m^n \cdot n)$|$O(n)$| Quá chậm | 
| DP qua quá trình chuyển đổi |$O(nk)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một bảng DP trong đó$dp[i][j]$biểu thị số cách tô màu đầu tiên$i$gạch với chính xác$j$chuyển tiếp. 

1. Khởi tạo trường hợp cơ sở cho viên gạch đầu tiên. có$m$cách tô màu và không có sự chuyển tiếp nào xảy ra, vì vậy$dp[1][0] = m$. Mỗi màu hợp lệ phải bắt đầu bằng một số lựa chọn màu sắc. 
2. Đối với từng vị trí$i$từ$2$ĐẾN$n$, chúng tôi xem xét việc mở rộng tất cả các cấu hình hợp lệ từ$i-1$. 
3. Nếu chúng ta gán gạch$i$cùng màu với gạch$i-1$thì số lần chuyển tiếp không thay đổi. Đối với mọi cấu hình được tính trong$dp[i-1][j]$, có chính xác một cách để mở rộng nó mà không làm tăng số lần chuyển đổi, vì vậy điều này góp phần$dp[i-1][j]$ĐẾN$dp[i][j]$. 
4. Nếu chúng ta gán gạch$i$một màu khác với gạch$i-1$, sau đó chúng ta tăng số lần chuyển tiếp lên 1. Vì màu trước đó được cố định nên có chính xác$m-1$lựa chọn hợp lệ cho màu mới. Điều này góp phần$(m-1) \cdot dp[i-1][j-1]$ĐẾN$dp[i][j]$. 
5. Chúng tôi tính toán tất cả các trạng thái lên đến$dp[n][k]$, đó là câu trả lời cuối cùng. 

Sự tái phát là:$$dp[i][j] = dp[i-1][j] + (m-1)\cdot dp[i-1][j-1]$$với việc xử lý cẩn thận các chỉ số không hợp lệ. 

### Tại sao nó hoạt động 

Trạng thái DP là đủ vì bất kỳ màu một phần nào cũng được đặc trưng hoàn toàn, nhằm mục đích mở rộng trong tương lai, bằng hai thông tin: có bao nhiêu lần chuyển đổi đã xảy ra và màu cuối cùng là gì. Sự phụ thuộc màu cuối cùng biến mất trong công thức chuyển đổi vì tất cả các màu đều đối xứng và việc chuyển sang bất kỳ màu nào khác luôn mang lại kết quả chính xác$m-1$sự lựa chọn. Tính đối xứng này đảm bảo rằng việc tổng hợp tất cả các màu cuối cùng không làm mất thông tin và không nhân đôi cấu hình đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n, m, k = map(int, input().split())

# dp[j] = ways for current prefix with j transitions
dp = [0] * (k + 1)

# base: first brick, 0 transitions, m choices
dp[0] = m

for i in range(2, n + 1):
    ndp = [0] * (k + 1)
    for j in range(0, k + 1):
        # same color transition count unchanged
        ndp[j] = (ndp[j] + dp[j]) % MOD

        # different color increases transitions
        if j + 1 <= k:
            ndp[j + 1] = (ndp[j + 1] + dp[j] * (m - 1)) % MOD

    dp = ndp

print(dp[k])
```Việc triển khai nén DP thành một chiều duy nhất vì mỗi lớp chỉ phụ thuộc vào lớp trước đó. Chi tiết quan trọng là cả hai quá trình chuyển đổi đều được áp dụng từ cùng một trạng thái trước đó, vì vậy chúng tôi không được cập nhật tại chỗ, nếu không chúng tôi sẽ làm ảnh hưởng đến các trạng thái trong tương lai trong cùng một lần lặp. 

Khởi tạo cơ sở`dp[0] = m`nắm bắt được thực tế là viên gạch đầu tiên được tự do lựa chọn màu sắc của nó. Mỗi bước tiếp theo được xây dựng dựa trên đó mà không cần theo dõi nhận dạng màu thực tế. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 0
```Chúng tôi muốn tất cả các viên gạch giống hệt nhau. 

| tôi | dp[0] | dp[1] | 
| --- | --- | --- | 
| 1 | 3 | 0 | 
| 2 | 3 | 0 | 
| 3 | 3 | 0 | 

Câu trả lời cuối cùng là 3. 

Điều này xác nhận rằng DP bảo toàn tính bất biến mà không có chuyển tiếp nào được đưa ra khi$k=0$và mỗi bước chỉ đơn giản là truyền bá các màu đồng nhất hiện có. 

### Ví dụ 2 

đầu vào:```
3 3 1
```Chúng tôi muốn chính xác một sự chuyển đổi. 

| tôi | dp[0] | dp[1] | 
| --- | --- | --- | 
| 1 | 3 | 0 | 
| 2 | 3 | 6 | 
| 3 | 3 | 12 | 

Câu trả lời cuối cùng là 12. 

Điều này cho thấy yếu tố$m-1 = 2$liên tục mở rộng cấu hình bất cứ khi nào quá trình chuyển đổi được thực hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nk)$| Mỗi vị trí cập nhật nhiều nhất$k$trạng thái chuyển tiếp | 
| Không gian |$O(k)$| Chỉ lớp DP hiện tại được lưu trữ | 

Giới hạn$n, k \le 2000$làm$n \cdot k \le 4 \cdot 10^6$, nằm trong giới hạn thoải mái đối với Python với số học số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 998244353
    n, m, k = map(int, input().split())

    dp = [0] * (k + 1)
    dp[0] = m

    for i in range(2, n + 1):
        ndp = [0] * (k + 1)
        for j in range(k + 1):
            ndp[j] = (ndp[j] + dp[j]) % MOD
            if j + 1 <= k:
                ndp[j + 1] = (ndp[j + 1] + dp[j] * (m - 1)) % MOD
        dp = ndp

    return str(dp[k])

# provided samples
assert run("3 3 0") == "3"

# custom cases
assert run("1 5 0") == "5", "single brick all colors"
assert run("2 2 1") == "2", "must differ once"
assert run("4 1 0") == "1", "only one color available"
assert run("4 3 3") == "6", "maximum transitions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 0 | 5 | trường hợp cạnh phần tử đơn | 
| 2 2 1 | 2 | độ chính xác chuyển tiếp đơn | 
| 4 1 0 | 1 | thoái hóa không chọn màu | 
| 4 3 3 | 6 | đếm luân phiên đầy đủ | 

## Vỏ cạnh 

cho$n=1$, không thể chuyển đổi được, vì vậy câu trả lời luôn là$m$khi$k=0$Và$0$nếu không thì. DP xử lý việc này một cách ngầm định vì các vị trí vòng lặp không bao giờ chạy và trạng thái cơ sở không thay đổi. 

Vì$m=1$, tất cả các viên gạch phải cùng màu. Cấu hình hợp lệ duy nhất không có sự chuyển tiếp nào. Theo thuật ngữ DP,$m-1 = 0$, do đó tất cả các đường dẫn tạo chuyển tiếp đều biến mất, chỉ còn lại$dp[n][0] = 1$, điều đó đúng. 

Vì$k = n-1$, mọi cặp liền kề đều phải khác nhau. DP buộc một chuỗi trong đó mỗi bước nhân với$m-1$, dẫn đến$m \cdot (m-1)^{n-1}$, phù hợp với cách giải thích kết hợp về việc chọn màu đầu tiên một cách tự do và sau đó luôn tránh màu trước đó.
