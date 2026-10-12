---
title: "CF 105055E - Email"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự a và b. Chuỗi này không phải là tùy ý: nó được coi là mã hóa mật khẩu nhị phân trong đó mỗi số 0 được thay thế bằng khối ab và mỗi số 1 được thay thế bằng khối aba."
date: "2026-06-28T00:23:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105055
codeforces_index: "E"
codeforces_contest_name: "UDESC Selection Contest 2023-2"
rating: 0
weight: 105055
solve_time_s: 69
verified: true
draft: false
---

[CF 105055E - Email](https://codeforces.com/problemset/problem/105055/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chỉ gồm các ký tự`a`Và`b`. Chuỗi này không phải là tùy ý: nó được coi là mã hóa mật khẩu nhị phân trong đó mỗi chuỗi`0`đã được thay thế bởi khối`ab`và mỗi`1`đã được thay thế bởi khối`aba`. Mật khẩu ban đầu là một chuỗi các bit và mã hóa nối các khối cố định này theo thứ tự. 

Nhiệm vụ của chúng ta là khôi phục bất kỳ chuỗi bit gốc hợp lệ nào có thể tạo ra chuỗi ký tự đã cho. Nếu chuỗi không thể được phân đoạn thành một chuỗi các khối hợp lệ`ab`Và`aba`, chúng tôi phải báo cáo thất bại. 

Vì vậy, vấn đề thực sự là vấn đề phân tích cú pháp ngữ pháp chuỗi với hai quy tắc đầu cuối. Chúng ta phải quyết định xem chuỗi đầu vào có thuộc về ngôn ngữ được tạo bằng cách nối hay không`ab`Và`aba`và nếu có, hãy xây dựng lại một chuỗi lựa chọn hợp lệ. 

Độ dài giới hạn lên tới 100.000 ký tự. Điều này ngay lập tức loại trừ mọi tìm kiếm theo cấp số nhân trên tất cả các phân đoạn có thể. Một cách tiếp cận quay lui ngây thơ cố gắng thực hiện mọi sự phân chia giữa`ab`Và`aba`có thể phân nhánh ở nhiều vị trí và trong trường hợp xấu nhất, số lượng phân tách tăng theo cấp số nhân, bởi vì mỗi lần xuất hiện của`aba`giới thiệu sự lựa chọn giữa việc tiêu thụ hai hoặc ba ký tự. 

Do đó chúng ta cần một chiến lược phân tích tuyến tính hoặc gần tuyến tính. 

Một trường hợp lỗi tinh vi xuất hiện bất cứ khi nào tiền tố`aba`xảy ra. Ở vị trí đó, cả hai cách giải thích đều có thể xảy ra: hoặc lấy`aba`BẰNG`1`, hoặc lấy`ab`BẰNG`0`và để lại ký tự tiếp theo được phân tích cú pháp. Một chiến lược tham lam luôn thích thời gian nghỉ trận đấu dài hơn đối với các đầu vào như`ababa`, lấy ở đâu`aba`đầu tiên để lại một hậu tố không hợp lệ mặc dù tồn tại sự phân tách hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ cố gắng phân tách chuỗi bắt đầu từ vị trí 0, phân nhánh bất cứ khi nào chúng tôi thấy mã thông báo hợp lệ. Từ bất kỳ vị trí nào`i`, nếu như`s[i:i+2] = ab`, chúng tôi thử diễn giải nó như`0`, và nếu`s[i:i+3] = aba`, chúng tôi thử diễn giải nó như`1`. Điều này tạo thành một cây đệ quy trong đó mỗi`aba`có thể phân nhánh thành hai phần tiếp theo. 

Tính đúng đắn của phương pháp này rất đơn giản vì nó khám phá tất cả các phân đoạn có thể có. Tuy nhiên, trong trường hợp xấu nhất, chẳng hạn như một chuỗi như`ababababa...`, hầu hết mọi vị trí đều cho phép cả hai lựa chọn và số lượng trạng thái đệ quy tăng theo cấp số nhân theo độ dài. Điều này vượt xa những gì có thể thực hiện được`n = 10^5`. 

Điều quan trọng cần lưu ý là đây là một vấn đề kinh điển về khả năng tiếp cận trên một dòng: tại mỗi chỉ mục`i`, chúng tôi chỉ quan tâm liệu hậu tố`i..n`có thể được phân tích cú pháp. Mỗi vị trí chỉ phụ thuộc vào tối đa hai vị trí tiền đạo,`i+2`Và`i+3`. Cấu trúc này cho phép lập trình động từ phải sang trái trong thời gian tuyến tính. 

Thay vì khám phá các đường dẫn, chúng tôi tính toán một mảng khả thi boolean`dp[i]`cho biết liệu hậu tố bắt đầu từ`i`có thể được phân tích đầy đủ. Khi đã biết điều này, chúng tôi sẽ xây dựng lại bất kỳ giải pháp hợp lệ nào một cách tham lam từ trái sang phải bằng bảng DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quay lại vũ phu | O(2^n) | O(n) | Quá chậm | 
| Lập trình động + Tái thiết | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xác định trạng thái của vấn đề 

Chúng tôi xác định`dp[i]`như thể chuỗi con bắt đầu ở vị trí`i`có thể được phân chia đầy đủ thành các khối hợp lệ. Chức vụ`n`(cuối chuỗi) luôn hợp lệ vì hậu tố trống không yêu cầu mã thông báo. 

### 2. Tính các phép chuyển từ phải sang trái 

Chúng tôi xử lý các chỉ số từ`n-1`xuống tới`0`. Tại mỗi vị trí`i`, chúng tôi kiểm tra xem liệu chúng tôi có thể lấy mã thông báo hợp lệ hay không: 

Nếu chuỗi con`s[i:i+2]`bằng`ab`, sau đó chúng ta có thể chuyển đến`i+2`nếu như`dp[i+2]`là đúng. 

Nếu chuỗi con`s[i:i+3]`bằng`aba`, sau đó chúng ta có thể chuyển đến`i+3`nếu như`dp[i+3]`là đúng. 

Chúng tôi lưu trữ`dp[i] = true`nếu một trong hai lựa chọn thành công. 

### 3. Xây dựng lại câu trả lời hợp lệ 

Nếu`dp[0]`là sai, không tồn tại phân tích cú pháp hợp lệ và chúng tôi xuất ra lỗi. Nếu không chúng ta bắt đầu từ chỉ mục`0`và liên tục chọn một quá trình chuyển đổi hợp lệ. Chúng tôi thích lấy`ab`đầu tiên (đại diện`0`), nhưng chỉ khi nó dẫn đến một hậu tố hợp lệ theo`dp`. Nếu không, chúng tôi lấy`aba`(đại diện cho`1`). 

Việc tái cấu trúc này đảm bảo chúng ta xây dựng một chuỗi bit nhất quán. 

### Tại sao nó hoạt động 

DP đảm bảo rằng`dp[i]`đúng khi tồn tại ít nhất một phân đoạn hợp lệ của hậu tố bắt đầu từ`i`. Mỗi lần chuyển đổi sẽ giảm vấn đề xuống một hậu tố nhỏ hơn hoàn toàn, do đó không tồn tại chu kỳ. Trong quá trình tái thiết, chúng tôi luôn chuyển sang trạng thái được biết là có thể giải quyết được, vì vậy chúng tôi không bao giờ bị mắc kẹt sớm. Điều này đảm bảo đầu ra tương ứng với sự phân tách hợp lệ đầy đủ của chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

dp = [False] * (n + 1)
dp[n] = True

for i in range(n - 1, -1, -1):
    if i + 2 <= n and s[i:i+2] == "ab" and dp[i+2]:
        dp[i] = True
    if i + 3 <= n and s[i:i+3] == "aba" and dp[i+3]:
        dp[i] = True

if not dp[0]:
    print(":(")
    sys.exit()

i = 0
ans = []

while i < n:
    if i + 2 <= n and s[i:i+2] == "ab" and dp[i+2]:
        ans.append('0')
        i += 2
    else:
        ans.append('1')
        i += 3

print("".join(ans))
```Mảng DP được điền ngược lại để khi chúng ta đánh giá`dp[i]`, cả hai`dp[i+2]`Và`dp[i+3]`đã được biết đến. Giai đoạn tái thiết dựa trên tính bất biến rằng ở mỗi bước, tồn tại ít nhất một chuyển đổi hợp lệ vì`dp[0]`đã được xác nhận là đúng. 

Sự lựa chọn ưa thích`ab`đầu tiên là tùy ý; mọi sự ràng buộc nhất quán đều có tác dụng vì DP đảm bảo rằng vẫn còn ít nhất một đường dẫn hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`ababa`Tính toán DP: 

| tôi | kiểm tra chuỗi con | dp[i+2] / dp[i+3] | dp[i] | 
| --- | --- | --- | --- | 
| 5 | kết thúc | đúng | đúng | 
| 4 | một | không hợp lệ | sai | 
| 3 | ba | không hợp lệ | sai | 
| 2 | aba | dp[5]=true | đúng | 
| 1 | em yêu | không hợp lệ | sai | 
| 0 | ab / aba | dp[2]=true | đúng | 

Tái thiết: 

| tôi | sự lựa chọn | đầu ra | tiếp theo tôi | 
| --- | --- | --- | --- | 
| 0 | ab | 0 | 2 | 
| 2 | aba | 1 | 5 | 

Đầu ra là`01`. 

Điều này xác nhận rằng sự mơ hồ cục bộ ở vị trí 0 được giải quyết chính xác bằng tính khả thi toàn cầu. 

### Ví dụ 2 

đầu vào:`ababaabab`DP xác nhận đầy đủ tính khả thi và tiến độ tái thiết: 

| tôi | hành động | 
| --- | --- | 
| 0 | ab → 0 | 
| 2 | aba → 1 | 
| 5 | ab → 0 | 
| 7 | ab → 0 | 

Đầu ra là`0100`, khớp với phân đoạn nhất quán của toàn bộ chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được xử lý một lần và mỗi lần chuyển đổi sẽ kiểm tra tối đa hai chuỗi con có độ dài cố định | 
| Không gian | O(n) | Mảng DP lưu trữ một boolean cho mỗi vị trí | 

Thuật toán tuyến tính theo độ dài của chuỗi, tối ưu vì mỗi ký tự phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ cũng tuyến tính và dễ dàng phù hợp với các ràng buộc đối với`n = 10^5`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import deque

    s = _sys.stdin.readline().strip()
    n = len(s)

    dp = [False] * (n + 1)
    dp[n] = True

    for i in range(n - 1, -1, -1):
        if i + 2 <= n and s[i:i+2] == "ab" and dp[i+2]:
            dp[i] = True
        if i + 3 <= n
```
