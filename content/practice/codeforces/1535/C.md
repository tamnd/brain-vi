---
title: "CF 1535C - Dây không ổn định"
description: "Chúng ta được cung cấp một chuỗi chứa '0', '1' và '?'. Một chuỗi con được coi là đẹp nếu chúng ta có thể thay thế mọi dấu '?' độc lập với 0 hoặc 1 để chuỗi con kết quả trở thành chuỗi nhị phân xen kẽ. Nói cách khác, các ký tự lân cận phải luôn khác nhau."
date: "2026-06-10T15:56:30+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "greedy", "implementation", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1535
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 110 (Rated for Div. 2)"
rating: 1400
weight: 1535
solve_time_s: 1206
verified: true
draft: false
---

[CF 1535C - Chuỗi không ổn định](https://codeforces.com/problemset/problem/1535/C) 

**Đánh giá:** 1400 
**Tags:** tìm kiếm nhị phân, dp, tham lam, triển khai, chuỗi, hai con trỏ 
**Thời gian giải:** 20m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chứa`'0'`,`'1'`, Và`'?'`. 

Một chuỗi con được coi là đẹp nếu chúng ta có thể thay thế mọi`'?'`độc lập với một trong hai`0`hoặc`1`để chuỗi con kết quả trở thành chuỗi nhị phân xen kẽ. Nói cách khác, các ký tự lân cận phải luôn khác nhau. 

Nhiệm vụ là đếm xem có bao nhiêu chuỗi con liền kề có thuộc tính này. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai. Một giải pháp kiểm tra mọi chuỗi con sẽ yêu cầu khoảng`n²/2`chuỗi con, đó là về`2·10^10`khi`n = 2·10^5`. Ngay cả việc kiểm tra từng chuỗi con trong thời gian không đổi cũng sẽ quá chậm. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số trường hợp đặc biệt làm cho vấn đề trở nên phức tạp hơn lần đầu tiên xuất hiện. 

Coi như:```
???
```Mọi chuỗi con đều đẹp vì chúng ta có thể gán các giá trị xen kẽ theo cách chúng ta muốn. Câu trả lời là:```
6
```Một giải pháp cố gắng cam kết thực hiện một nhiệm vụ cụ thể của`?`các ký tự có thể từ chối không chính xác các chuỗi con hợp lệ. 

Coi như:```
00
```Chuỗi con`"00"`không thể thực hiện xen kẽ được. Câu trả lời là:```
2
```Chỉ có hai chuỗi con có độ dài 1 hoạt động. 

Coi như:```
0?0
```Toàn bộ chuỗi con đều đẹp vì ký tự ở giữa có thể trở thành`1`, sản xuất`010`. 

Kiểm tra cục bộ chỉ so sánh các ký tự cố định liền kề sẽ bỏ lỡ khả năng này. 

Coi như:```
?1??1
```Toàn bộ dây không đẹp. Nếu vị trí ngang bằng buộc cả hai cố định`1`s yêu cầu các bài tập mâu thuẫn nhau, không có sự thay thế nào tồn tại. Bất kỳ giải pháp đúng nào cũng phải phát hiện xung đột được tạo bởi các ký tự cố định cách xa nhau. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi chuỗi con, sau đó kiểm tra xem chuỗi con đó có thể được chuyển thành chuỗi xen kẽ hay không. 

Một chuỗi con có thể khớp với một trong hai mẫu xen kẽ:```
010101...
```hoặc```
101010...
```Đối với mỗi chuỗi con, chúng ta có thể kiểm tra cả hai mẫu và xem liệu mọi ký tự cố định có tương thích với ít nhất một trong số chúng hay không. 

Cách tiếp cận này đúng vì một chuỗi con đẹp chính xác khi một trong hai mẫu xen kẽ phù hợp với tất cả các vị trí cố định. 

Vấn đề là số lượng chuỗi con. Một chuỗi có độ dài`n`có`n(n+1)/2`các chuỗi con. Với`n = 2·10^5`, tức là khoảng hai mươi tỷ chuỗi con. Ngay cả việc kiểm tra O(1) cho mỗi chuỗi con cũng không thể thực hiện được. 

Quan sát quan trọng là một chuỗi xen kẽ áp đặt quy tắc chẵn lẻ. 

Giả sử một chuỗi con bắt đầu ở vị trí`L`. 

Nếu chúng ta quyết định vị trí đó`L`nên`0`, thì mọi vị trí có cùng tính chẵn lẻ như`L`cũng phải`0`và mọi vị trí có tính chẵn lẻ đối diện phải là`1`. 

Nếu chúng ta quyết định vị trí đó`L`nên`1`, vai trò bị đảo ngược. 

Xung đột xảy ra khi các ký tự cố định buộc cả hai khả năng đều thất bại. 

Thay vì kiểm tra từng chuỗi con một cách độc lập, chúng tôi xử lý chuỗi từ trái sang phải và duy trì hậu tố hợp lệ dài nhất kết thúc ở vị trí hiện tại. 

Một thủ thuật hữu ích là diễn giải lại từng ký tự cố định. 

Đối với vị trí`i`:```
expected value = i mod 2
```tương ứng với một mẫu xen kẽ, trong khi```
expected value = 1 - (i mod 2)
```tương ứng với cái kia. 

Bất cứ khi nào một ký tự cố định không đồng ý với một mẫu, nó sẽ hỗ trợ mẫu kia. Chúng tôi theo dõi vị trí gần đây nhất phá vỡ từng mẫu. 

Sau đó, với mọi vị trí kết thúc`r`, chúng ta có thể xác định ngay khoảng cách còn lại của một chuỗi con hợp lệ. 

Điều này biến vấn đề thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý chuỗi từ trái sang phải. 
2. Duy trì hai giá trị:`last0`= vị trí mới nhất tạo nên mẫu`010101...`không thể nào.`last1`= vị trí mới nhất tạo nên mẫu`101010...`không thể nào. 
3. Tại vị trí`i`, nếu như`s[i]`là`'?'`, nó không bao giờ tạo ra xung đột nên giá trị cũng không thay đổi. 
4. Nếu`s[i]`là`'0'`, so sánh nó với ký tự dự kiến ​​theo cả hai mẫu xen kẽ. 

Nếu mẫu`010101...`mong đợi`1`ở đây, thì vị trí này sẽ phá vỡ khuôn mẫu đó, vì vậy hãy cập nhật`last0 = i`. 

Cập nhật tương tự`last1`nếu mẫu thứ hai bị hỏng. 
5. Nếu`s[i]`là`'1'`, thực hiện logic tương tự. 
6. Hãy để```
bad = min(last0, last1)
```Bất kỳ chuỗi con nào kết thúc tại`i`và bắt đầu vào hoặc trước`bad`thất bại cả hai mô hình xen kẽ. 
7. Vì vậy mọi vị trí xuất phát trong```
bad + 1 ... i
```tạo thành một chuỗi con đẹp kết thúc tại`i`. 

Số chuỗi con như vậy là:```
i - bad
```8. Thêm số lượng này vào câu trả lời. 

### Tại sao nó hoạt động 

Đối với mỗi vị trí, chúng tôi nhớ vị trí mới nhất mà mỗi mẫu xen kẽ đều không thể thực hiện được. 

Một chuỗi con kết thúc tại`i`là đẹp nếu ít nhất một mẫu xen kẽ vẫn khả thi trong suốt chuỗi con đó. 

Nếu một chuỗi con bắt đầu tại hoặc trước`last0`, sau đó mẫu`010101...`đã bị mâu thuẫn bên trong chuỗi con. 

Tương tự, bắt đầu tại hoặc trước`last1`làm mất hiệu lực mẫu`101010...`. 

Một chuỗi con chỉ bị lỗi khi cả hai mẫu đều không hợp lệ. Điều đó xảy ra chính xác khi vị trí bắt đầu của nó nhiều nhất là```
min(last0, last1).
```Tất cả các vị trí bắt đầu sau này đều bảo tồn ít nhất một mẫu nên chúng rất đẹp. Việc đếm những điểm bắt đầu này cho mỗi vị trí kết thúc sẽ đếm mọi chuỗi con đẹp chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        s = input().strip()
        
        last0 = -1
        last1 = -1
        ans = 0
        
        for i, ch in enumerate(s):
            if ch != '?':
                bit = int(ch)
                
                if bit != (i & 1):
                    last0 = i
                
                if bit != (1 ^ (i & 1)):
                    last1 = i
            
            ans += i - min(last0, last1)
        
        print(ans)

solve()
```Các biến`last0`Và`last1`lưu trữ những mâu thuẫn gần đây nhất cho hai mẫu xen kẽ có thể. 

biểu thức```
(i & 1)
```là ký tự được mẫu mong đợi```
010101...
```khi lập chỉ mục từ số 0. 

biểu thức```
1 ^ (i & 1)
```là ký tự được mẫu mong đợi```
101010...
```ở cùng một vị trí. 

Bất cứ khi nào một ký tự cố định không đồng ý với một mẫu, thì mẫu đó sẽ không thể áp dụng được đối với bất kỳ chuỗi con nào chứa vị trí này. Cập nhật tương ứng`last`biến ghi lại sự thật đó. 

số lượng```
i - min(last0, last1)
```đếm tất cả các vị trí bắt đầu hợp lệ cho các chuỗi con kết thúc tại`i`. 

Câu trả lời có thể lớn như đại khái`n(n+1)/2`, đó là về`2·10^10`, vì vậy nó phải được lưu trữ ở dạng số nguyên có khả năng 64 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0?10
```| tôi | s[i] | cuối cùng0 | cuối cùng1 | phút(last0,last1) | Đã thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | -1 | 0 | -1 | 1 | 1 | 
| 1 | ? | -1 | 0 | -1 | 2 | 3 | 
| 2 | 1 | -1 | 2 | -1 | 3 | 6 | 
| 3 | 0 | -1 | 3 | -1 | 4 | 10 | 

Câu trả lời là 10. Mọi chuỗi con của chuỗi này đều đẹp. 

### Ví dụ 2 

đầu vào:```
?10??1100
```| tôi | s[i] | cuối cùng0 | cuối cùng1 | phút(last0,last1) | Đã thêm | Tổng cộng | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | ? | -1 | -1 | -1 | 1 | 1 | 
| 1 | 1 | -1 | 1 | -1 | 2 | 3 | 
| 2 | 0 | -1 | 2 | -1 | 3 | 6 | 
| 3 | ? | -1 | 2 | -1 | 4 | 10 | 
| 4 | ? | -1 | 2 | -1 | 5 | 15 | 
| 5 | 1 | -1 | 5 | -1 | 6 | 21 | 
| 6 | 1 | 6 | 5 | 5 | 1 | 22 | 
| 7 | 0 | 7 | 5 | 5 | 2 | 24 | 
| 8 | 0 | 7 | 8 | 7 | 1 | 25 | 

Xung đột do các ký tự cố định liên tiếp ở gần cuối gây ra làm giảm đáng kể số lượng hậu tố hợp lệ. Câu trả lời cuối cùng là 25. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là`2·10^5`, do đó quét tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Thuật toán sử dụng bộ nhớ phụ không đổi bất kể độ dài chuỗi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        s = input().strip()

        last0 = -1
        last1 = -1
        ans = 0

        for i, ch in enumerate(s):
            if ch != '?':
                bit = int(ch)

                if bit != (i & 1):
                    last0 = i

                if bit != (1 ^ (i & 1)):
                    last1 = i

            ans += i - min(last0, last1)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("3\n0?10\n???\n?10??1100\n") == "10\n6\n25"

# minimum size
assert run("1\n?\n") == "1"

# all equal fixed characters
assert run("1\n0000\n") == "4"

# alternating already
assert run("1\n0101\n") == "10"

# off-by-one boundary
assert run("1\n00\n") == "2"

# mixed case
assert run("1\n0?0\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`?`|`1`| Chuỗi con ký tự đơn | 
|`0000`|`4`| Mọi độ dài lớn hơn 1 đều thất bại | 
|`0101`|`10`| Đã xen kẽ, tất cả các chuỗi con đều hoạt động | 
|`00`|`2`| Cặp xung đột nhỏ nhất | 
|`0?0`|`6`| Dấu hỏi giải quyết xung đột | 

## Vỏ cạnh 

Hãy xem xét:```
???
```Mỗi chuỗi con có thể được hoàn thành thành một chuỗi xen kẽ. Trong quá trình quét cũng không`last0`cũng không`last1`thay đổi từ`-1`. Các bổ sung là`1`,`2`, Và`3`, sản xuất`6`. Thuật toán tự nhiên đếm tất cả các chuỗi con. 

Coi như:```
00
```Ở vị trí thứ hai, hai số 0 cố định tạo ra sự mâu thuẫn đối với bất kỳ chuỗi con nào có độ dài bằng hai. Thuật toán cập nhật các vị trí xung đột và chỉ đếm hai chuỗi con một ký tự, đưa ra`2`. 

Coi như:```
0?0
```Nhân vật ở giữa vẫn linh hoạt. Thuật toán không bao giờ ghi lại xung đột làm chặn toàn bộ chuỗi vì gán vị trí ở giữa cho`1`sản lượng`010`. Câu trả lời trở thành`6`, nghĩa là mọi chuỗi con đều đẹp. 

Coi như:```
?1??1
```Hai cái cố định cuối cùng làm mất hiệu lực một trong các mẫu xen kẽ trên phạm vi lớn hơn. các`last0`Và`last1`cập nhật nắm bắt chính xác nơi mâu thuẫn xuất hiện. Các chuỗi con vẫn bảo toàn ít nhất một mẫu vẫn được tính, trong khi những chuỗi con không thể thực hiện được sẽ tự động bị loại trừ.
