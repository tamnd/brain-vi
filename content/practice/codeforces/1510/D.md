---
title: "CF 1510D - Chữ số"
description: "Chúng ta có n thẻ. Mỗi thẻ chứa một số nguyên dương. Chúng tôi có thể chọn bất kỳ tập hợp con thẻ không trống nào và nhân tất cả các số đã chọn. Mục tiêu không chỉ đơn thuần là thu được tích có chữ số thập phân cuối cùng bằng d."
date: "2026-06-10T19:43:55+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1510
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1510
solve_time_s: 1394
verified: false
draft: false
---

[CF 1510D - Chữ số](https://codeforces.com/problemset/problem/1510/D) 

**Đánh giá:** 2100 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 23m 14s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`thẻ. Mỗi thẻ chứa một số nguyên dương. Chúng tôi có thể chọn bất kỳ tập hợp con thẻ không trống nào và nhân tất cả các số đã chọn. 

Mục tiêu không chỉ đơn thuần là thu được sản phẩm có chữ số thập phân cuối cùng bằng`d`. Trong số tất cả các tập con như vậy, chúng ta muốn có tập con có giá trị tích càng lớn càng tốt. 

Quan sát đầu tiên là chỉ có chữ số cuối cùng của tích mới quan trọng đối với ràng buộc. Nếu một số kết thúc bằng chữ số`x`, nhân với nó sẽ thay đổi chữ số cuối cùng hiện tại`r`vào trong`(r * x) mod 10`. 

Quan sát thứ hai là sản phẩm thực tế có thể trở nên lớn về mặt thiên văn. Với`n = 100000`, thậm chí việc lưu trữ các sản phẩm ứng cử viên là không thể. Chúng ta cần một cách khác để so sánh sản phẩm. 

Sự ràng buộc`a_i ≤ 1000`là cực kỳ hữu ích. Mỗi số chỉ đóng góp một trong mười chữ số cuối cùng có thể. Do đó, không gian trạng thái cho chữ số cuối cùng chỉ là 10. Mặt khác,`n`là rất lớn, do đó mọi giải pháp phụ thuộc vào kích thước tập con hoặc độ lớn của tích đều bị loại trừ. Chúng ta cần thời gian gần như tuyến tính. 

Một trường hợp cạnh tinh tế xuất hiện khi chữ số mong muốn có thể đạt được bằng một số tập hợp con có tích khác nhau rất nhiều. 

Ví dụ:```
3 6
2 3 8
```Cả hai`{8}`Và`{2,3}`tạo ra chữ số cuối cùng của`8`Và`6`tương ứng. Một chiến lược tham lam chỉ dựa trên những chữ số cuối cùng sẽ bỏ lỡ tầm quan trọng của tầm quan trọng của sản phẩm. 

Một trường hợp phức tạp khác liên quan đến số`1`.```
3 4
1 4 11
```nhân với`1`không thay đổi chữ số cuối cùng nhưng nó vẫn có thể thuộc tập hợp con tối ưu. Bất kỳ quy trình tái thiết nào cũng phải cho phép các phần tử đó xuất hiện. 

Trường hợp cạnh cuối cùng xảy ra khi không tồn tại tập hợp con hợp lệ.```
2 1
2 4
```Mọi tích đều chẵn nên chữ số cuối cùng không bao giờ bằng`1`. Đầu ra đúng là`-1`. 

## Phương pháp tiếp cận 

Giải pháp Brute Force kiểm tra mọi tập hợp con, tính toán sản phẩm của nó, kiểm tra chữ số cuối cùng và giữ lại sản phẩm hợp lệ lớn nhất. 

Điều này đúng vì nó đánh giá rõ ràng mọi khả năng. Thật không may là có`2^n`tập hợp con. Vì`n = 100000`điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là ràng buộc chỉ phụ thuộc vào chữ số cuối cùng của tích. Chỉ có mười dư lượng có thể có theo modulo 10. 

Giả sử chúng ta biết chữ số cuối cùng của tích tập hợp con. Để so sánh hai tập con có cùng chữ số cuối cùng, chúng ta chỉ cần biết tích nào lớn hơn. Từ`log(ab) = log(a) + log(b)`, 

tối đa hóa sản phẩm tương đương với tối đa hóa tổng logarit. 

Điều này biến bài toán thành một bài toán quy hoạch động chỉ có 10 trạng thái. 

Cho phép`dp[r]`là tổng logarit tối đa có thể có trong số tất cả các tập hợp con được xử lý có tích kết thúc bằng chữ số`r`. 

Khi chúng tôi xử lý một số mới`a`, chữ số cuối cùng của nó là`x`, chúng ta có thể bắt đầu một tập hợp con mới chỉ chứa`a`, hoặc nối thêm`a`với bất kỳ tập con nào đã biết trước đó. Chữ số cuối cùng kết quả trở thành`(r * x) mod 10`. 

Vì chỉ có 10 dư lượng nên mỗi phần tử chỉ thực hiện một lượng công không đổi. Toàn bộ DP chạy theo thời gian tuyến tính. 

Để xuất tập hợp con thực tế, chúng tôi lưu trữ thông tin gốc bất cứ khi nào trạng thái được cải thiện. Sau khi DP kết thúc, chúng tôi xây dựng lại các chỉ mục đã chọn bằng cách đi ngược lại các con trỏ cha. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| DP ở chữ số cuối cùng | O(10n) | O(10n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`w_i = log(a_i)`. 
2. Duy trì`dp[10]`, Ở đâu`dp[r]`lưu trữ giá trị logarit tối đa của tập hợp con được xử lý có sản phẩm kết thúc bằng chữ số`r`. 
3. Khởi tạo mọi trạng thái về âm vô cực. 
4. Xử lý từng số một. 
5. Đối với số hiện tại`a`, cho phép`x = a mod 10`. 
6. Tạo bản sao của DP hiện tại vì quá trình chuyển đổi chỉ được sử dụng các trạng thái từ lần lặp trước đó. 
7. Xét tập con chỉ gồm`a`. Nó tạo ra dư lượng`x`và giá trị`log(a)`. 
8. Đối với mọi dư lượng`r`đã tồn tại, hãy tạo một tập hợp con mới bằng cách thêm vào`a`. Phần dư mới trở thành`(r * x) mod 10`, và giá trị mới trở thành`dp[r] + log(a)`. 
9. Bất cứ khi nào quá trình chuyển đổi cải thiện trạng thái, hãy ghi lại đủ thông tin để xây dựng lại giải pháp sau này. Lưu trữ phần dư trước đó và chỉ mục trước đó. 
10. Sau khi xử lý xong tất cả các số, kiểm tra cặn`d`. 
11. Nếu còn sót lại`d`không thể truy cập được, hãy in`-1`. 
12. Mặt khác, hãy xây dựng lại tập hợp con bằng cách theo sau các con trỏ cha ngược lại từ trạng thái cuối cùng. 
13. Xuất ra các số tương ứng. 

### Tại sao nó hoạt động 

Đối với mỗi dư lượng`r`, DP lưu trữ giá trị logarit lớn nhất trong số tất cả các tập hợp con kết thúc bằng phần dư`r`sau khi xử lý tiền tố của mảng. 

Quá trình chuyển đổi đơn đảm bảo rằng mọi tập hợp con đều có thể bắt đầu ở phần tử được chọn đầu tiên. Quá trình chuyển đổi mở rộng đảm bảo rằng mọi tập hợp con lớn hơn có thể được hình thành từ một tập hợp con nhỏ hơn cộng với một phần tử mới. 

Bởi vì logarit tăng nghiêm ngặt nên tập hợp con có tổng logarit lớn hơn luôn có tích lớn hơn. Do đó, DP giữ chính xác tập hợp con có tích tối đa cho mọi loại dư lượng. 

Bằng cách quy nạp trên các phần tử đã xử lý, mọi tập hợp con có thể tiếp cận đều được xem xét và đối với mỗi phần dư, phần tử tốt nhất sẽ được giữ lại. Do đó trạng thái dư lượng`d`sau khi xử lý tất cả các số tương ứng với tích lớn nhất có thể có chữ số cuối cùng bằng`d`. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

NEG = -1e100

n, d = map(int, input().split())
a = list(map(int, input().split()))

dp = [NEG] * 10

take = [[False] * 10 for _ in range(n)]
prev_digit = [[-1] * 10 for _ in range(n)]
prev_index = [[-1] * 10 for _ in range(n)]

for i, val in enumerate(a):
    x = val % 10
    w = math.log(val)

    ndp = dp[:]

    if w > ndp[x]:
        ndp[x] = w
        take[i][x] = True
        prev_digit[i][x] = -1
        prev_index[i][x] = -1

    for r in range(10):
        if dp[r] <= NEG / 2:
            continue

        nr = (r * x) % 10
        cand = dp[r] + w

        if cand > ndp[nr]:
            ndp[nr] = cand
            take[i][nr] = True
            prev_digit[i][nr] = r
            prev_index[i][nr] = i - 1

    dp = ndp

if dp[d] <= NEG / 2:
    print(-1)
    sys.exit()

res = []
cur_digit = d
idx = n - 1

while idx >= 0:
    if take[idx][cur_digit]:
        res.append(a[idx])
        pd = prev_digit[idx][cur_digit]
        pi = prev_index[idx][cur_digit]

        cur_digit = pd
        idx = pi
    else:
        idx -= 1

print(len(res))
print(*res)
```Mảng DP chỉ lưu trữ giá trị logarit tốt nhất cho mỗi phần dư. Các bảng xây dựng lại ghi nhớ quá trình chuyển đổi nào đã tạo ra trạng thái. 

Việc so sánh sử dụng logarit thay vì sản phẩm. Điều này tránh được số nguyên lớn trong khi vẫn giữ nguyên thứ tự. 

Cấu trúc tái thiết là phần tinh tế nhất. Bất cứ khi nào trạng thái được cải thiện, chúng tôi ghi lại chính xác dư lượng trước đó đã tạo ra trạng thái đó. Các trạng thái không được cải tiến phải giữ nguyên ý nghĩa cũ của chúng, đó là lý do tại sao quá trình chuyển đổi được thực hiện từ mảng DP được sao chép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4
4 11 8 2 1 13
```Trạng thái DP chính sau mỗi bước: 

| Giá trị đã xử lý | Dư lượng tốt nhất 4 | Dư lượng tốt nhất 8 | Dư lượng tốt nhất 2 | 
| --- | --- | --- | --- | 
| 4 | nhật ký(4) | - | - | 
| 11 | nhật ký(44) | - | - | 
| 8 | nhật ký(44) | nhật ký(88) | - | 
| 2 | nhật ký(88) | nhật ký(88) | nhật ký(22) | 
| 1 | không thay đổi | không thay đổi | không thay đổi | 
| 13 | nhật ký(1144) | dư lượng lớn hơn-8 trạng thái | dư lượng lớn hơn-2 trạng thái | 

Trạng thái dư 4 cuối cùng tương ứng với sản phẩm 1144, là trạng thái tối ưu. 

### Ví dụ 2 

đầu vào:```
2 1
2 4
```| Giá trị đã xử lý | Dư lượng có thể tiếp cận | 
| --- | --- | 
| 2 | {2} | 
| 4 | {2,4,8} | 

Dư lượng 1 không bao giờ có thể truy cập được, vì vậy câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(10n) | Mười chuyển tiếp dư lượng cho mỗi phần tử | 
| Không gian | O(10n) | Thông tin tái thiết cho mọi chỉ số và dư lượng | 

Với`n = 100000`, thuật toán thực hiện khoảng một triệu chuyển đổi DP, dễ dàng phù hợp với giới hạn thời gian. Các bảng xây dựng lại chỉ chiếm vài triệu mục và vừa vặn trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```
# impossible target digit
2 1
2 4

# single element works
1 7
7

# single element fails
1 7
3

# many ones
5 1
1 1 1 1 1

# target digit zero
4 0
10 3 7 9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 2 4`|`-1`| Dư lượng không thể tiếp cận | 
|`1 7 / 7`| Một yếu tố được chọn | Tập hợp con đơn | 
|`1 7 / 3`|`-1`| Thất bại đơn lẻ | 
| Năm cái | Bất kỳ tập con nào không trống | Xử lý nhận dạng nhân | 
| Chữ số mục tiêu bằng 0 | Tập hợp con hợp lệ kết thúc bằng 0 | Chuyển đổi dư lượng liên quan đến 0 | 

## Vỏ cạnh 

Khi tất cả các số đều là số chẵn và chữ số đích là số lẻ thì không có chuỗi phép nhân nào có thể tạo ra chữ số cuối cùng là số lẻ. DP đơn giản là không bao giờ đạt được lượng cặn tương ứng và in chính xác`-1`. 

Khi một số tập hợp con tạo ra cùng một chữ số cuối cùng, DP sẽ so sánh tổng logarit thay vì kích thước tập hợp con. Ví dụ:```
3 6
2 3 16
```Tập hợp con`{2,3}`sản xuất`6`, trong khi`{16}`sản xuất`16`. Phần dư là như nhau, nhưng giá trị logarit của`16`lớn hơn nên DP giữ tập con thứ hai. 

Khi mảng chứa nhiều mảng, chẳng hạn như```
4 4
1 1 1 4
```phần dư không thay đổi khi thêm một số vào, nhưng giá trị logarit cũng không đổi. Lựa chọn nào cũng hợp lệ vì cả hai đều sản xuất cùng một sản phẩm. DP vẫn xây dựng lại một tập hợp con tối ưu chính xác.
