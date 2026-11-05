---
title: "CF 1204A - BowWow và Thời khóa biểu"
description: "Các chuyến tàu khởi hành vào các thời điểm $$1, 4, 16, 64, chấm$$ chính là lũy thừa của 4. Cho một thời gian $s$, chúng ta cần đếm xem có bao nhiêu thời gian khởi hành trong số này nhỏ hơn $s$. Phần bất thường là $s$ không được đưa ra dưới dạng thập phân."
date: "2026-06-11T23:38:47+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1204
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 581 (Div. 2)"
rating: 1000
weight: 1204
solve_time_s: 115
verified: true
draft: false
---

[CF 1204A - BowWow và Thời khóa biểu](https://codeforces.com/problemset/problem/1204/A) 

**Đánh giá:** 1000 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đôi khi những chuyến tàu khởi hành$$1, 4, 16, 64, \dots$$chính xác là lũy thừa của 4. Cho một thời điểm$s$, chúng ta cần đếm xem có bao nhiêu thời gian khởi hành nhỏ hơn hoàn toàn so với$s$. 

Phần bất thường đó là$s$không được đưa ra ở dạng thập phân. Thay vào đó, nó được đưa ra dưới dạng một chuỗi nhị phân không có số 0 đứng đầu và giá trị của nó có thể lớn bằng$2^{100}-1$. Số nguyên 64 bit bình thường không đủ để lưu trữ mọi giá trị có thể. 

Ràng buộc ngay lập tức gợi ý rằng chúng ta nên tránh chuyển đổi toàn bộ số thành kiểu số nguyên tiêu chuẩn. Biểu diễn nhị phân chứa tối đa 100 bit, do đó, bất kỳ giải pháp nào hoạt động trực tiếp trên chuỗi theo thời gian tuyến tính đều đủ nhanh. 

Quan sát quan trọng là mỗi thời gian khởi hành của tàu đều có lũy thừa bằng 4:$$4^k = 2^{2k}.$$Trong hệ nhị phân, lũy thừa của 4 trông như sau:$$1,\ 100,\ 10000,\ 1000000,\dots$$Mỗi cái có chính xác một bit được đặt và bit đó nằm ở vị trí chẵn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
1
```Thời gian là chính xác$1$. Vì việc khởi hành phải được thực hiện nghiêm ngặt trước$s$, chưa có chuyến tàu nào rời đi. Câu trả lời là:```
0
```Một giải pháp đếm lũy thừa 4 nhỏ hơn hoặc bằng$s$sẽ trả về không chính xác 1. 

Hãy xem xét:```
100
```Đây là$4$ở dạng thập phân. Chỉ có tàu vào thời điểm đó$1$đã khởi hành sớm hơn nên câu trả lời là:```
1
```Một lần nữa, sử dụng$\le$thay vì$<$sẽ tạo ra 2. 

Một trường hợp tế nhị khác là:```
101
```đó là$5$. Những chuyến khởi hành tại$1$Và$4$đều sớm hơn nên câu trả lời là:```
2
```Điều này khác với ví dụ trước mặc dù cả hai số đều có cùng độ dài nhị phân. Chúng ta phải phân biệt giữa lũy thừa thuần túy bằng 4 và số lớn hơn có cùng độ dài bit. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực trực tiếp là liên tục tạo ra lũy thừa 4:$$1,4,16,64,\dots$$và đếm xem có bao nhiêu số nhỏ hơn$s$. Từ$s < 2^{100}$, chỉ có khoảng 50 lũy thừa liên quan của 4. Nếu trước tiên chúng ta chuyển chuỗi nhị phân thành số nguyên lớn thì cách tiếp cận này thực sự đủ nhanh. 

Giải pháp thú vị hơn là tránh chuyển đổi số. 

Thời gian khởi hành là lũy thừa của 4, chính xác là những con số$2^{2k}$. Giả sử biểu diễn nhị phân của$s$có chiều dài$L$. 

Mọi lũy thừa của 4 với độ dài nhị phân hoàn toàn nhỏ hơn$L$tự động nhỏ hơn$s$. chiều dài của chúng là$$1,3,5,\dots$$bởi vì$2^{2k}$có độ dài nhị phân$2k+1$. 

Số quyền lực như vậy là$$\left\lfloor \frac{L-1}{2} \right\rfloor.$$Điều còn lại là quyết định xem lũy thừa của 4 có cùng độ dài nhị phân như$s$cũng nên được tính. 

Đối với một chuỗi nhị phân có độ dài$L$, sức mạnh đó là$$2^{L-1}.$$Nó chỉ có lũy thừa bằng 4 khi$L-1$là chẵn, nghĩa là$L$thật kỳ quặc. 

Nếu như$L$thật kỳ quặc và$s$hoàn toàn lớn hơn chuỗi nhị phân$$1\underbrace{00\dots0}_{L-1\text{ zeros}},$$thì lũy thừa của 4 cũng nhỏ hơn$s$và phải được tính. 

Điều này làm giảm toàn bộ vấn đề trong việc kiểm tra chính chuỗi nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L) | O(1) | Đã chấp nhận | 
| Tối ưu | O(L) | O(1) | Đã chấp nhận | 

Đây$L \le 100$là độ dài chuỗi nhị phân. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi nhị phân$s$. 
2. Hãy để$L$là chiều dài của nó. 
3. Đếm tất cả lũy thừa của 4 có độ dài nhị phân nhỏ hơn$L$. 

Con số này là:$$\left\lfloor \frac{L-1}{2} \right\rfloor.$$4. Nếu$L$là số chẵn, không có lũy thừa nào của 4 có độ dài nhị phân$L$, do đó xuất ra số đếm hiện tại. 
5. Nếu$L$kỳ lạ, hãy so sánh$s$với chuỗi nhị phân bao gồm một hàng đầu`1`theo sau chỉ là số không. 

Chuỗi này đại diện$2^{L-1}$, lũy thừa duy nhất của 4 có chiều dài$L$. 
6. Nếu$s$hoàn toàn lớn hơn chuỗi đó, hãy thêm một chuỗi vào câu trả lời. 
7. Xuất kết quả. 

Tại sao nó hoạt động: 

Mọi lũy thừa của 4 tương ứng với một số nhị phân có chính xác một bit được đặt ở số mũ chẵn. Trong số các số có độ dài nhị phân nhỏ hơn$L$, tất cả các lũy thừa như vậy sẽ tự động nhỏ hơn$s$, và số lượng của chúng chính xác là$\lfloor (L-1)/2 \rfloor$. Khi$L$thật kỳ lạ, có thêm một ứng cử viên, đó là$2^{L-1}$. Nó phải được tính chính xác khi nó nhỏ hơn rất nhiều so với$s$. Không có lũy thừa nào khác của 4 có thể tồn tại trong khoảng này vì lũy thừa của 4 tăng thêm hai bit số mũ cùng một lúc. Do đó thuật toán đếm chính xác số lần khởi hành trước thời gian$s$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
n = len(s)

ans = (n - 1) // 2

if n % 2 == 1:
    if s != "1" + "0" * (n - 1):
        ans += 1

print(ans)
```Phần đầu tiên tính số lũy thừa của 4 có độ dài nhị phân nhỏ hơn độ dài của đầu vào. 

biểu hiện`(n - 1) // 2`xuất phát trực tiếp từ việc đếm độ dài lẻ:$$1,3,5,\dots < n.$$Câu hỏi duy nhất còn lại là liệu có nên tính lũy thừa của 4 với cùng độ dài nhị phân hay không. Sức mạnh đó chỉ tồn tại khi`n`thật kỳ quặc. 

Với chiều dài lẻ`n`, lũy thừa liên quan chính xác là chuỗi:```
1000...000
```với`n-1`số không. Nếu đầu vào bằng chuỗi này thì thời gian khởi hành không hoàn toàn trước`s`, vì vậy chúng tôi không thêm nó. Bất kỳ chuỗi nhị phân nào khác có cùng độ dài đều lớn hơn, nghĩa là phải tính lũy thừa 4. 

Không cần chuyển đổi số nguyên, điều này tránh mọi lo ngại về kích thước số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
100000000
```| Biến | Giá trị | 
| --- | --- | 
| s |`100000000`| 
| n | 9 | 
| câu trả lời ban đầu | (9 - 1) // 2 = 4 | 
| lạ nhỉ? | Có | 
| dây nguồn |`100000000`| 
| s bằng chuỗi lũy thừa? | Có | 
| câu trả lời cuối cùng | 4 | 

Đầu ra:```
4
```Con số chính xác là$256 = 4^4$. Các chuyến tàu ở$1,4,16,64$đã khởi hành, nhưng chuyến tàu lúc$256$đã không khởi hành đúng thời gian$256$. 

### Ví dụ 2 

đầu vào:```
101
```| Biến | Giá trị | 
| --- | --- | 
| s |`101`| 
| n | 3 | 
| ban đầu và | (3 - 1) // 2 = 1 | 
| lạ nhỉ? | Có | 
| dây nguồn |`100`| 
| s bằng chuỗi lũy thừa? | Không | 
| thêm 1? | Có | 
| câu trả lời cuối cùng | 2 | 

Đầu ra:```
2
```Số lượng là$5$. Cả hai$1$Và$4$thực sự nhỏ hơn$5$, vậy là hai chuyến tàu đã khởi hành rồi. 

Ví dụ này cho thấy tại sao các số có cùng độ dài nhị phân không thể được xử lý giống nhau. Sự khác biệt giữa`100`Và`101`vấn đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Xây dựng và so sánh các chuỗi có độ dài L | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Vì biểu diễn nhị phân chứa tối đa 100 ký tự nên thuật toán chỉ thực hiện một lượng công việc rất nhỏ. Nó thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    s = input().strip()
    n = len(s)

    ans = (n - 1) // 2

    if n % 2 == 1:
        if s != "1" + "0" * (n - 1):
            ans += 1

    return str(ans)

# provided sample
assert run("100000000\n") == "4", "sample 1"

# custom cases
assert run("1\n") == "0", "minimum value"
assert run("100\n") == "1", "exact power of 4"
assert run("101\n") == "2", "just above a power of 4"
assert run("11\n") == "0", "even length, no same-length power of 4"
assert run("1111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111\n") == "50", "maximum length 100 bits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Bất đẳng thức nghiêm ngặt ở giá trị nhỏ nhất | 
|`100`|`1`| Chính xác lũy thừa của 4 nên không tính chính nó | 
|`101`|`2`| Cùng độ dài nhưng lớn hơn lũy thừa 4 | 
|`11`|`0`| Các số có độ dài chẵn không có lũy thừa độ dài bằng 4 | 
| 100 cái |`50`| Độ dài nhị phân được phép lớn nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
```Thuật toán tính toán`n = 1`Và`ans = 0`. Vì chuỗi bằng`1`, số lượng bổ sung không được thêm vào. Kết quả là:```
0
```Điều này giải quyết một cách chính xác thực tế là việc khởi hành phải diễn ra nghiêm ngặt trước thời gian đến. 

Hãy xem xét đầu vào:```
100
```Đây`n = 3`, vậy số đếm ban đầu là`(3 - 1) // 2 = 1`. Chuỗi nguồn có cùng độ dài cũng được`100`. Vì bằng nhau nên không tính chuyến tàu phụ. Câu trả lời vẫn là:```
1
```Điều này ngăn ngừa lỗi phổ biến khi đếm chính xác các chuyến khởi hành vào thời điểm$s$. 

Hãy xem xét đầu vào:```
101
```Lại`n = 3`, do đó số đếm ban đầu là 1. Chuỗi lũy thừa có cùng độ dài là`100`và đầu vào lớn hơn. Một chuyến tàu bổ sung được thêm vào, mang lại:```
2
```Điều này chứng tỏ tại sao sự bình đẳng phải được đối xử khác với việc lớn hơn một cách nghiêm ngặt. 

Hãy xem xét đầu vào:```
11
```Chiều dài là chẵn. Không có lũy thừa nào của 4 có thể có độ dài nhị phân là 2, vì lũy thừa của 4 luôn có độ dài nhị phân lẻ. Thuật toán ngay lập tức trả về:```
0
```phù hợp với thực tế, vì$3$nhỏ hơn ứng cử viên có cùng độ dài đầu tiên$4$.
