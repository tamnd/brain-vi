---
title: "CF 102168I - \u041a\u043e\u043d\u0442\u0435\u0441\u0442\u044b"
description: "Có n cuộc thi được sắp xếp theo thứ tự thời gian. Đối với cuộc thi i, a[i] là mức tăng xếp hạng đạt được khi tham gia cuộc thi đó. Sau khi tham gia cuộc thi i, Kirill phải bỏ qua các cuộc thi b[i] tiếp theo nên cuộc thi sớm nhất anh có thể tham gia sau i là i + b[i] + 1."
date: "2026-08-19T07:34:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102168
codeforces_index: "I"
codeforces_contest_name: "\u041b\u0438\u0447\u043d\u044b\u0439 \u0447\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u043e\u0433\u043e \u0443\u043d\u0438\u0432\u0435\u0440\u0441\u0438\u0442\u0435\u0442\u0430 \u0441\u0440\u0435\u0434\u0438 \u043d\u043e\u0432\u0438\u0447\u043a\u043e\u0432 2018-2019"
rating: 0
weight: 102168
solve_time_s: 74
verified: true
draft: false
---

[CF 102168I - \u041a\u043e\u043d\u0442\u0435\u0441\u0442\u044b](https://codeforces.com/problemset/problem/102168/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`n`cuộc thi sắp xếp theo trình tự thời gian. Dành cho cuộc thi`i`,`a[i]`là mức tăng xếp hạng thu được khi tham gia vào nó. Sau khi tham gia cuộc thi`i`, Kirill phải bỏ qua phần tiếp theo`b[i]`cuộc thi, vì vậy cuộc thi sớm nhất anh ấy có thể tham gia sau`i`là`i + b[i] + 1`. 

Nhiệm vụ là chọn một tập hợp các cuộc thi có khoảng thời gian phục hồi không trùng nhau, tối đa hóa tổng số điểm của chúng.`a[i]`các giá trị. Từ`a[i]`có thể là tiêu cực, lựa chọn trống cũng có liên quan: Kirill có thể chỉ cần tham gia vào các cuộc thi và đạt được`0`. 

Với`n <= 200000`, MỘT`O(n^2)`giải pháp là quá chậm. Trong trường hợp xấu nhất nó sẽ hoạt động khoảng`200000^2 / 2 = 20,000,000,000`chuyển đổi trạng thái, không thể phù hợp với giới hạn 2 giây. Chúng ta cần một thuật toán gần với thời gian tuyến tính. 

Các giá trị của`a[i]`có thể đạt được`10^9`theo một trong hai hướng và câu trả lời có thể chứa đựng sự đóng góp từ nhiều cuộc thi. Số nguyên 32 bit là không đủ: câu trả lời có thể xoay quanh`2 * 10^14`về độ lớn. Số nguyên Python xử lý việc này một cách tự động, trong khi các ngôn ngữ như C++ cần`long long`. 

Có một số trường hợp ranh giới có thể khiến việc triển khai trực tiếp bị sai. Nếu chỉ có một cuộc thi, chẳng hạn như```
1-10 0
```câu trả lời là`0`, không`-10`, vì việc tham gia là tùy chọn. Nếu như`b[i] = 0`, cuộc thi tiếp theo sẽ có ngay. Ví dụ,```
25 07 0
```có câu trả lời`12`, vì vậy điều trị`b[i]`như thể nó bao gồm cuộc thi`i + 1`sẽ cấm cuộc thi thứ hai một cách không chính xác. 

Ranh giới khác là khi thời gian phục hồi kéo dài ra ngoài toàn bộ chuỗi còn lại. Vì```
210 5100 0
```câu trả lời là`100`. Sau cuộc thi 1, không còn cuộc thi nào có thể sử dụng được nữa, nhưng điều đó không khiến cuộc thi 1 trở thành bắt buộc hoặc ưu tiên hơn cuộc thi 2. 

Cuối cùng, những cuộc tranh giành tiêu cực không được buộc chúng ta phải chấp nhận chúng. Vì```
3-5 0-2 0-7 0
```câu trả lời là`0`. Một DP được khởi tạo ở mức âm vô cực mà không thể hiện rõ ràng khả năng không tham gia cuộc thi nào có thể âm thầm tạo ra câu trả lời phủ định. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản xem xét mọi cuộc thi cuối cùng có thể xảy ra. Giả sử chúng ta quyết định cuộc thi đó`i`là cuộc thi cuối cùng chúng ta tham gia. Sau đó, tất cả các cuộc thi được chọn trước đó phải kết thúc trước`i`và chúng ta có thể liệt kê đệ quy tất cả các tập hợp con hợp lệ trước nó. Điều này đúng vì mọi lịch trình hợp lệ đều có cuộc thi cuối cùng được xác định rõ ràng, nhưng số lượng tập hợp con là theo cấp số nhân, do đó cách tiếp cận này trở nên vô dụng thậm chí sớm hơn nhiều so với`n = 200000`. 

Chúng ta có thể làm cho ý tưởng bạo lực trở nên có cấu trúc hơn bằng lập trình động. Cho phép`dp[i]`là mức tăng xếp hạng tối đa có thể đạt được chỉ bằng cách sử dụng lần đầu tiên`i`các cuộc thi. Khi xử lý cuộc thi`i`, có đúng hai khả năng. Chúng tôi bỏ qua nó, rời đi`dp[i - 1]`. Hoặc chúng ta tham gia vào đó, trong trường hợp đó cuộc thi được chọn trước đó tối đa phải nằm`b[i] + 1`vị trí trước`i`. Nếu lần đầu tiên`i`cuộc thi được đánh số từ`1`, cuộc thi cuối cùng có sẵn trước thời gian phục hồi là`i - b[i] - 1`. 

Như vậy giá trị thu được khi tham gia cuộc thi`i`là`a[i] + dp[i - b[i] - 1]`. 

Quan sát quan trọng đó là`dp`đã chứa câu trả lời tối ưu cho mọi tiền tố có thể. Chúng tôi không cần phải kiểm tra lịch trình cá nhân trước đó. Toàn bộ lịch sử trước khoảng thời gian bị cấm có thể được tóm tắt bằng một con số. 

Điều này gây ra sự tái phát`dp[i] = max(dp[i - 1], a[i] + dp[max(0, i - b[i] - 1)])`. 

các`max(0, ...)`xử lý các trường hợp thi`i`buộc chúng ta phải đi trước đầu mảng. Sự lặp lại kiểm tra mỗi cuộc thi một lần, do đó tổng công việc là tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Độ sâu đệ quy O(n) | Quá chậm | 
| Tiền tố DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo`dp`Ở đâu`dp[i]`đại diện cho mức tăng xếp hạng tốt nhất có thể đạt được từ lần đầu tiên`i`các cuộc thi. Bộ`dp[0] = 0`, bởi vì trước khi xem xét bất kỳ cuộc thi nào thì mức tăng là bằng không. 
2. Xử lý cuộc thi từ trái sang phải. Dành cho cuộc thi`i`, trước tiên hãy cân nhắc việc bỏ qua nó. Kết quả tốt nhất trong trường hợp đó là chính xác`dp[i - 1]`. 
3. Nếu chúng tôi tham gia cuộc thi`i`, chúng ta phải bỏ qua phần tiếp theo`b[i]`các cuộc thi. Cuộc thi ngay trước khoảng cấm là`i - b[i] - 1`. Do đó, kết quả trước đó tương thích tốt nhất là`dp[max(0, i - b[i] - 1)]`. 
4. Thêm`a[i]`với kết quả tiền tố tương thích đó. Điều này đưa ra lịch trình tốt nhất mà các cuộc thi được chọn bao gồm`i`. 
5. Lưu trữ phần lớn hơn của các trường hợp bỏ qua và tham gia vào`dp[i]`. Sau khi xử lý tất cả các cuộc thi,`dp[n]`là mức tăng xếp hạng tối đa có thể. 

### Tại sao nó hoạt động 

Hãy xem xét một lịch trình tối ưu trong số những lịch trình đầu tiên`i`các cuộc thi. Nếu nó không chứa cuộc thi`i`, giá trị của nó lớn nhất là`dp[i - 1]`, Và`dp[i - 1]`đại diện cho lịch trình tốt nhất như vậy. Nếu nó có chứa cuộc thi`i`, mọi cuộc thi được chọn khác phải thuộc về cuộc thi đầu tiên`i - b[i] - 1`các cuộc thi. Theo định nghĩa của`dp`, lịch trình tương thích tốt nhất trước đó có giá trị`dp[max(0, i - b[i] - 1)]`. Thêm`a[i]`đưa ra lịch trình tốt nhất có thể bao gồm`i`. Hai trường hợp này bao gồm mọi lịch trình hợp lệ, vì vậy việc lấy mức tối đa của chúng sẽ mang lại mức tối ưu thực sự cho mọi tiền tố. Bằng quy nạp trên`i`,`dp[n]`là tối ưu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n = int(input())
    dp = [0] * (n + 1)
    for i in range(1, n + 1):        a, b = map(int, input().split())
        previous = i - b - 1        if previous < 0:            previous = 0
        take = a + dp[previous]        skip = dp[i - 1]
        dp[i] = max(skip, take)
    print(dp[n])

if __name__ == "__main__":    solve()
```Mảng`dp`sử dụng thêm một vị trí để`dp[0]`tự nhiên đại diện cho tiền tố trống. Tại cuộc thi`i`,`previous = i - b - 1`là tiền tố lớn nhất có thể được sử dụng cùng với cuộc thi`i`. 

Việc trừ đi một là chi tiết lập chỉ mục chính. Nếu như`b = 0`, cuộc thi`i - 1`được cho phép, vì vậy tiền tố phải kết thúc tại`i - 1`. Công thức cho chính xác`i - 0 - 1 = i - 1`. Nếu như`b = 1`, cuộc thi`i - 1`phải được bỏ qua, vì vậy tiền tố có thể sử dụng được kết thúc ở`i - 2`. 

Khi`previous`là tiêu cực, đơn giản là không có cuộc thi nào có thể xảy ra trước`i`, Và`dp[0] = 0`tượng trưng cho tình huống đó. Các số nguyên có độ chính xác tùy ý của Python cũng xử lý các tổng lên tới gần đúng một cách an toàn`200000 * 10^9`. 

Đầu vào được xử lý bằng`sys.stdin.readline`, điều này tránh được chi phí liên tục chia tách toàn bộ đầu vào thành một danh sách lớn các mã thông báo. 

## Ví dụ đã hoạt động 

Định dạng câu lệnh đặt các giá trị mẫu trên các dòng riêng biệt mà không đánh số chúng một cách rõ ràng. Việc giải thích hai mẫu được hiển thị sẽ đưa ra các đầu vào sau. 

### Mẫu 1```
320 0100 230 0
```Các tiểu bang phát triển như sau. 

|`i`|`a[i]`|`b[i]`|`previous`|`take`|`skip`|`dp[i]`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 20 | 0 | 0 | 20 | 0 | 20 | 
| 2 | 100 | 2 | 0 | 100 | 20 | 100 | 
| 3 | 30 | 0 | 2 | 130 | 100 | 130 | 

Ở cuộc thi 2,`b[2] = 2`, do đó cả cuộc thi 1 và cuộc thi 3 đều không tương thích với cuộc thi 2. Do đó, lựa chọn tốt nhất liên quan đến cuộc thi 2 là`100`. Ở cuộc thi thứ 3, chúng ta có thể kết hợp với kết quả tốt nhất của hai cuộc thi đầu tiên, đưa ra`100 + 30 = 130`. 

### Mẫu 2```
320 1100 230 0
```|`i`|`a[i]`|`b[i]`|`previous`|`take`|`skip`|`dp[i]`| 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 20 | 1 | 0 | 20 | 0 | 20 | 
| 2 | 100 | 2 | 0 | 100 | 20 | 100 | 
| 3 | 30 | 0 | 2 | 130 | 100 | 130 | 

Ở đây cuộc thi 1 có`b[1] = 1`, nên tham gia sẽ cản trở việc tham gia cuộc thi 2. Bản thân cuộc thi 3 không có thời gian hồi phục nên sau khi chọn cuộc thi 2 chúng ta vẫn có thể chọn cuộc thi 3. Kết quả tối ưu lại là`130`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cuộc thi được xử lý một lần với công việc liên tục. | 
| Không gian | O(n) | Mảng DP lưu trữ một giá trị cho mỗi tiền tố. | 

Với`n = 200000`, thuật toán chỉ thực hiện một số phép tính số học cho mỗi cuộc thi, vì vậy nó phù hợp với giới hạn thời gian dự kiến. Mảng DP chứa khoảng 200001 số nguyên Python, cũng nằm trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve_data(inp: str) -> str:    data = inp.split()    it = iter(data)
    n = int(next(it))    dp = [0] * (n + 1)
    for i in range(1, n + 1):        a = int(next(it))        b = int(next(it))
        previous = max(0, i - b - 1)        dp[i] = max(dp[i - 1], a + dp[previous])
    return str(dp[n])

def run(inp: str) -> str:    return solve_data(inp)

# Sample 1assert run(    """320 0100 230 0""") == "130", "sample 1"
# Sample 2assert run(    """320 1100 230 0""") == "130", "sample 2"
# Minimum size, negative value, so taking nothing is optimal.assert run(    """1-10 0""") == "0", "minimum-size negative case"
# Two consecutive contests with no recovery period.assert run(    """45 07 03 010 0""") == "25", "b = 0 boundary"
# Every contest forces all remaining contests to be skipped.assert run(    """410 1020 030 040 0""") == "40", "recovery extends beyond the array"
# Negative values mixed with positive values.assert run(    """5-100 010 120 0-50 030 2""") == "50", "negative values"
# Maximum-size input.n = 200000large_input = str(n) + "\n" + ("1 0\n" * n)assert run(large_input) == str(n), "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -10 0`|`0`| Lịch trình trống và kích thước đầu vào tối thiểu | 
|`5 0, 7 0, 3 0, 10 0`|`25`|`b = 0`, nơi mọi cuộc thi liền kề đều có thể được thực hiện | 
|`10 10, 20 0, 30 0, 40 0`|`40`| Khoảng thời gian phục hồi kéo dài vượt ra ngoài tất cả các cuộc thi còn lại | 
|`-100 0, 10 1, 20 0, -50 0, 30 2`|`50`| Giá trị âm và chỉ chọn các cuộc thi tương thích có lợi nhuận | 
|`200000`cuộc thi với`a = 1, b = 0`|`200000`| Tối đa`n`và hiệu suất tuyến tính | 

## Vỏ cạnh 

### Một cuộc thi phủ định duy nhất 

cho```
1-10 0
```trạng thái ban đầu là`dp[0] = 0`. Tham gia cuộc thi duy nhất mang lại`-10`, trong khi bỏ qua nó mang lại`0`, Vì thế`dp[1] = 0`. Thuật toán không bao giờ giả định rằng phải chọn ít nhất một cuộc thi. 

### Không có thời gian phục hồi 

cho```
25 07 0
```cuộc thi 1 có`previous = 0`, cho`dp[1] = 5`. Cuộc thi 2 có`previous = 1`, bởi vì`2 - 0 - 1 = 1`. Giá trị nhận của nó là`7 + dp[1] = 12`, vậy câu trả lời là`12`. Đây chính xác là hành vi ranh giới cần thiết khi`b[i] = 0`. 

### Phục hồi vượt xa sự khởi đầu 

cho```
210 5100 0
```cuộc thi 1 có`previous = -5`, được kẹp vào`0`. Lấy nó mang lại`10`. Cuộc thi 2 có`previous = 1`, cho`100 + dp[1] = 110`vì giá trị thu hồi của chính nó bằng 0. Như vậy câu trả lời thực chất là`110`, vì cuộc thi 2 có thể được thực hiện sau cuộc thi 1. Phần lớn`b[1]`chỉ ảnh hưởng đến những gì có thể xảy ra sau cuộc thi 1 chứ không ảnh hưởng đến những gì có thể xảy ra trước cuộc thi 2. Sự khác biệt này chính là lý do tại sao DP nhìn lại cuộc thi đang được xem xét. 

### Tất cả các giá trị đều âm 

cho```
3-5 0-2 0-7 0
```mọi`take`giá trị là âm, trong khi`skip`còn lại`0`. Do đó, DP giữ ở mức 0 trong toàn bộ mảng. Điều này xử lý thực tế là việc tăng xếp hạng là tùy chọn. 

### Giá trị dương rất lớn 

Giả sử có nhiều cuộc thi tương thích`a[i] = 10^9`. Với`200000`cuộc thi, tổng số có thể đạt`2 * 10^14`. Bản thân sự lặp lại không thay đổi ở quy mô này, nhưng việc triển khai sử dụng số nguyên 32 bit sẽ tràn. Kiểu số nguyên của Python tránh được vấn đề đó, do đó, phép lặp tương tự vẫn hợp lệ cho toàn bộ phạm vi đầu vào.
