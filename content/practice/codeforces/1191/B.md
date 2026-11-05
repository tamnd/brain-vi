---
title: "CF 1191B - Tokitsukaze và mạt chược"
description: "Chúng tôi được cấp chính xác ba quân mạt chược. Mỗi ô bao gồm một số từ 1 đến 9 và một bộ, trong đó bộ đó là một trong các m, p hoặc s. Một ván bài chiến thắng trong trò chơi đơn giản hóa này chỉ cần có sự tồn tại của một mentsu duy nhất."
date: "2026-06-12T00:30:12+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1191
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 573 (Div. 2)"
rating: 1200
weight: 1191
solve_time_s: 162
verified: true
draft: false
---

[CF 1191B - Tokitsukaze và Mạt chược](https://codeforces.com/problemset/problem/1191/B) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp chính xác ba quân mạt chược. Mỗi ô bao gồm một số từ 1 đến 9 và một bộ đồ, trong đó bộ đồ đó là một trong`m`,`p`, hoặc`s`. 

Một ván bài chiến thắng trong trò chơi đơn giản hóa này chỉ cần có sự tồn tại của một mentsu duy nhất. Mentsu có thể là bộ ba ô giống hệt nhau hoặc một chuỗi ba số liên tiếp trong cùng một bộ. 

Chúng tôi có thể vẽ bất kỳ ô bổ sung nào mà chúng tôi muốn và không có giới hạn về số lượng bản sao của ô đó. Nhiệm vụ là tìm số lượng ô bổ sung tối thiểu cần thiết để trong số các ô chúng ta có tồn tại ít nhất một bộ ba hoặc dãy hợp lệ. 

Kích thước đầu vào nhỏ bất thường. Chúng tôi luôn nhận được chính xác ba ô. Chỉ có 27 loại ô riêng biệt trong trò chơi, do đó, ngay cả những cách tiếp cận khá bạo lực cũng khả thi. Thách thức không phải là hiệu suất mà là xác định chính xác mức độ gần của ván bài hiện tại để hình thành một kết hợp hợp lệ. 

Một số trường hợp đặc biệt có thể dễ dàng dẫn đến việc triển khai không chính xác. 

Coi như:```
1m 1m 2m
```Câu trả lời đúng là:```
1
```Vẽ cái khác`1m`hoàn thành một bộ ba. Giải pháp chỉ kiểm tra trình tự sẽ trả về sai 2. 

Hãy xem xét:```
1m 2m 4m
```Câu trả lời đúng là:```
1
```Vẽ`3m`tạo ra trình tự`1m 2m 3m`. Việc triển khai bất cẩn chỉ tìm kiếm các số liền kề đã có sẵn có thể bỏ sót điều này. 

Coi như:```
1m 3m 5m
```Câu trả lời đúng là:```
2
```Không có cặp ô nào đủ gần để hoàn thành chuỗi chỉ bằng một lần rút. Bất kỳ sự kết hợp hợp lệ nào cũng cần có ít nhất hai ô bổ sung. 

Coi như:```
9m 9m 9m
```Câu trả lời đúng là:```
0
```Ván bài đã có sẵn bộ ba nên không cần rút bài. 

Quan sát quan trọng là câu trả lời chỉ có thể là 0, 1 hoặc 2. Bắt đầu với ba ô, nếu chúng ta chọn một ô bất kỳ, hai bản sao bổ sung luôn tạo thành một bộ ba. Vì vậy, hai trận hòa luôn là đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các ô có thể rút được. Vì có 27 loại ô, chúng tôi có thể kiểm tra mọi phần mở rộng một ô có thể và mọi phần mở rộng hai ô có thể có, sau đó kiểm tra xem liệu một phần kết hợp có tồn tại hay không. Tổng công việc là nhỏ: 

- 27 khả năng cho một lần rút thăm. 
-$27^2 = 729$khả năng cho hai trận hòa. 

Đối với mỗi ván bài của ứng cử viên, chúng tôi chỉ kiểm tra một số ô. Ngay cả cách tiếp cận này cũng dễ dàng nằm trong giới hạn. 

Tuy nhiên, vấn đề có cấu trúc đơn giản hơn nhiều. Chúng ta không bao giờ cần phải xây dựng bàn tay tương lai thực tế. Chúng ta chỉ cần biết ván bài hiện tại đã gần với một số kết hợp đến mức nào. 

Một meld bao gồm ba ô. Vì chúng tôi đã có ba ô nên mọi kết hợp tiềm năng phải sử dụng một, hai hoặc cả ba ô đó. 

Nếu cả ba ô hiện tại đã tạo thành một khối thì câu trả lời là 0. 

Nếu hai ô hiện tại có thể trở thành một phần của một khối sau khi thêm một ô thì câu trả lời là 1. 

Nếu không thì câu trả lời là 2. 

Điều này làm giảm nhiệm vụ đo số lượng ô lớn nhất đã góp phần tạo ra một số kết hợp. 

Đối với bộ ba, các ô giống hệt nhau sẽ đóng góp cùng nhau. Hai ô bằng nhau cần thêm một bản sao. Ba ô bằng nhau không cần thiết. 

Đối với trình tự, chỉ có các ô có cùng chất liệu mới quan trọng. Hai viên gạch giống nhau có thể là: 

- liên tiếp, chẳng hạn như`4m`Và`5m`, cần một ô điểm cuối; 
- cách nhau một khoảng trống, chẳng hạn như`4m`Và`6m`, cần gạch ở giữa; 
- giống hệt nhau, không hữu ích cho trình tự nhưng có thể giúp ích cho bộ ba. 

Chúng tôi đánh giá từng cặp ô và xác định có bao nhiêu ô của một khối đã có. Giá trị tốt nhất được tìm thấy sẽ xác định câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(27²) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba ô. 
2. Khởi tạo đáp án là 2. 

Hai lần rút luôn là đủ vì bất kỳ ô nào cũng có thể biến thành bộ ba bằng cách vẽ thêm hai bản sao. 
3. Kiểm tra từng ô riêng lẻ. 

Một ô duy nhất đã đóng góp một ô cho một số kết hợp trong tương lai, vì vậy câu trả lời không thể vượt quá 2. 
4. Kiểm tra từng cặp gạch. 

Nếu hai ô giống hệt nhau thì chúng đã tạo thành hai phần ba bộ ba. Cập nhật câu trả lời lên tối đa 1. 
5. Đối với mỗi cặp có cùng bộ đồ, hãy so sánh số lượng của chúng. 

Hãy để sự khác biệt giữa các số là`d`. 
6. Nếu`d = 0`, cặp này đóng góp hai ô để tạo thành một bộ ba. 

Cập nhật câu trả lời lên tối đa 1. 
7. Nếu`d = 1`hoặc`d = 2`, cặp này có thể trở thành một chuỗi sau khi thêm một ô phù hợp. 

Ví dụ: 

-`3m, 4m`nhu cầu`2m`hoặc`5m`. 
-`3m, 5m`nhu cầu`4m`. 

Cập nhật câu trả lời lên tối đa 1. 
8. Kiểm tra xem cả ba ô đã tạo thành bộ ba chưa. 

Nếu vậy thì câu trả lời là 0. 
9. Kiểm tra xem cả ba ô có tạo thành một chuỗi giống nhau hay không. 

Sắp xếp các số và xác minh chúng liên tiếp. 

Nếu vậy thì câu trả lời là 0. 
10. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Mỗi meld chứa chính xác ba ô. Bàn tay đã có ba ô, vì vậy điều quan trọng duy nhất là có bao nhiêu ô của một số khối đã có sẵn. 

Nếu có cả ba ô của một ô thì không cần phải rút thăm. 

Nếu có hai ô của một ô ghép, một lần rút sẽ hoàn thành ô đó. 

Nếu không có tổ hợp nào chứa nhiều hơn một ô hiện tại thì cần ít nhất hai lần rút. Vì hai lần rút luôn là đủ bằng cách hoàn thành bộ ba từ bất kỳ ô hiện có nào, nên câu trả lời chính xác là hai. 

Thuật toán kiểm tra rõ ràng mọi cách để hai ô có thể thuộc cùng một bộ ba hoặc chuỗi và mọi cách mà cả ba ô có thể tạo thành một khối. Do đó, nó luôn trả về số lần rút tối thiểu được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

tiles = input().split()

ans = 2

for i in range(3):
    for j in range(i + 1, 3):
        a, sa = int(tiles[i][0]), tiles[i][1]
        b, sb = int(tiles[j][0]), tiles[j][1]

        if tiles[i] == tiles[j]:
            ans = min(ans, 1)

        if sa == sb:
            d = abs(a - b)
            if d <= 2:
                ans = min(ans, 1)

# triplet
if tiles[0] == tiles[1] == tiles[2]:
    ans = 0

# sequence
nums = []
suit = tiles[0][1]

same_suit = True
for t in tiles:
    if t[1] != suit:
        same_suit = False
        break
    nums.append(int(t[0]))

if same_suit:
    nums.sort()
    if nums[0] + 1 == nums[1] and nums[1] + 1 == nums[2]:
        ans = 0

print(ans)
```Giải pháp bắt đầu bằng việc quan sát rằng câu trả lời không bao giờ vượt quá 2. Chúng tôi khởi tạo`ans = 2`và sau đó tìm kiếm bằng chứng cho thấy chỉ cần một lần rút hoặc không cần rút. 

Vòng lặp lồng nhau sẽ kiểm tra từng cặp ô. Vì chỉ có ba ô nên tồn tại chính xác ba cặp. Nếu hai ô giống hệt nhau thì chúng đã là hai phần ba của bộ ba. Nếu hai ô có cùng chất và số của chúng khác nhau nhiều nhất là hai, thì một ô có thể hoàn thành một chuỗi. 

Sau khi xử lý các cặp, chúng tôi kiểm tra xem ván bài đã hoàn thành chưa. Bộ ba rất dễ được phát hiện vì cả ba chuỗi ô vuông đều phải bằng nhau. 

Đối với một trình tự, cả ba bộ quần áo phải phù hợp. Sau đó chúng tôi sắp xếp các số và xác minh rằng chúng tạo thành các giá trị liên tiếp. Sắp xếp tránh sự phụ thuộc vào thứ tự đầu vào. 

Một lỗi phổ biến là quên trường hợp khoảng cách chính xác là hai, chẳng hạn như`3m`Và`5m`. Một ô, cụ thể là`4m`, hoàn thành chuỗi. Kiểm tra`d <= 2`xử lý chính xác cả cặp liền kề và một khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1s 2s 3s
```| Bước | Cặp/Trạng thái | Trả lời | 
| --- | --- | --- | 
| Ban đầu | bắt đầu | 2 | 
| Cặp (1s,2s) | khác biệt = 1 | 1 | 
| Cặp (1s,3s) | khác biệt = 2 | 1 | 
| Cặp (2s,3s) | khác biệt = 1 | 1 | 
| Kiểm tra trình tự | đã 1,2,3 bộ đồ giống nhau | 0 | 

Đầu ra:```
0
```Ví dụ này cho thấy rằng việc kiểm tra theo cặp có thể cho thấy một lần rút là đủ, nhưng lần kiểm tra ba ô cuối cùng có thể làm giảm câu trả lời xuống 0. 

### Ví dụ 2 

đầu vào:```
7m 7m 7m
```| Bước | Cặp/Trạng thái | Trả lời | 
| --- | --- | --- | 
| Ban đầu | bắt đầu | 2 | 
| Đôi (7m,7m) | giống hệt nhau | 1 | 
| Đôi (7m,7m) | giống hệt nhau | 1 | 
| Đôi (7m,7m) | giống hệt nhau | 1 | 
| Kiểm tra ba lần | tất cả đều bình đẳng | 0 | 

Đầu ra:```
0
```Ví dụ này thể hiện điều kiện bộ ba
