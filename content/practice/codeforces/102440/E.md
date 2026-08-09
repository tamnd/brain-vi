---
title: "CF 102440E - Hướng dẫn về thiên hà cho người quá giang"
description: "Có sự mâu thuẫn trực tiếp giữa báo cáo vấn đề được cung cấp và kết quả mẫu của nó, vì vậy một bài xã luận chính xác không thể được viết từ báo cáo chính xác như đã đưa ra."
date: "2026-08-08T13:50:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "E"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 276
verified: true
draft: false
---

[CF 102440E - Hướng dẫn về thiên hà cho người quá giang](https://codeforces.com/problemset/problem/102440/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có sự mâu thuẫn trực tiếp giữa báo cáo vấn đề được cung cấp và kết quả mẫu của nó, vì vậy một bài xã luận chính xác không thể được viết từ báo cáo chính xác như đã đưa ra. 

Mảng`f`mô tả đồ thị hàm số: mọi ngôi sao`i`có một cạnh đi ra`i -> f_i`, và mọi`-1`là một cạnh mà đích đến của nó mà chúng ta được phép chọn. Bắt đầu từ một ngôi sao, liên tục đi theo các cạnh này, cuối cùng sẽ đi vào một chu kỳ có hướng. số`c_i`của các ngôi sao riêng biệt có thể tiếp cận được từ`i`do đó, kích thước của đuôi dẫn đến chu kỳ cộng với kích thước chu kỳ, bao gồm cả ngôi sao ban đầu vì nó có thể tiếp cận được từ chính nó bằng cách di chuyển bằng 0 và, trong biểu đồ hàm, cũng xuất hiện trở lại sau khi đi qua chu trình. 

Hãy xem xét mẫu đầu tiên:```
n = 5
f = [0, 1, 2, -1, -1]
```Ngôi sao`0`,`1`, Và`2`đã có vòng lặp tự, vì vậy mỗi vòng đóng góp`1`. 

Hai cạnh chưa biết có thể được hoàn thành một cách hợp pháp như```
f = [0, 1, 2, 4, 3]
```Rồi sao`3`Và`4`tạo thành một chu kỳ có độ dài`2`. Bắt đầu từ một trong hai, hai ngôi sao có thể tiếp cận được`3`Và`4`, vậy đóng góp của họ đều là`2`. 

Sức hấp dẫn thu được là 

1+1+1+2+2=7, 

đã lớn hơn sản lượng mẫu đã nêu`3`. Kể từ đây`3`không thể là mức tối đa theo định nghĩa được cung cấp. 

Mẫu thứ hai có vấn đề tương tự theo hướng ngược lại. Với tất cả bảy mục chưa biết, chúng ta có thể đặt```
f = [1, 2, 3, 4, 5, 6, 0]
```và thu được một chu kỳ chứa tất cả bảy ngôi sao. Mỗi ngôi sao khởi đầu có thể đạt đủ bảy sao nên sức hấp dẫn là 

7⋅7=49, 

không phải như đã nêu`42`. 

Sự khác biệt không phải là chi tiết triển khai hoặc trường hợp đặc biệt. Nó thay đổi chính vấn đề toán học. Trang Codeforces chính thức hiện hiển thị chính xác tuyên bố tương tự và các mẫu được hiển thị ở đây, đồng thời cho biết rằng tuyên bố đã được thay đổi gần đây. 

## Phương pháp tiếp cận 

Đối với tuyên bố như đã viết, vũ lực sẽ liệt kê mọi khả năng thay thế của`-1`mục nhập. Nếu có`k`vị trí chưa biết, có`n^k`sự hoàn thiện. Việc đánh giá một đồ thị hàm số đã hoàn chỉnh một cách đơn giản có thể mất`O(n^2)`bằng cách bắt đầu duyệt từ mọi đỉnh, do đó cách tiếp cận toàn diện trực tiếp là`O(n^{k+2})`. Với tất cả các mục không xác định, điều này trở thành`O(n^{n+2})`, điều này vượt xa khả năng ngay cả đối với rất nhỏ`n`. 

Trên thực tế, có một giải pháp đồ thị thời gian tuyến tính cho bài toán được mô tả bằng câu lệnh, dựa trên việc phân tách đồ thị hàm số được chỉ định một phần thành các thành phần đã chứa chu trình và các thành phần kết thúc ở một cạnh không xác định. Tuy nhiên, giải pháp đó tạo ra`7`Và`49`đối với các mẫu được cung cấp, không`3`Và`42`. 

Bởi vì bài xã luận được yêu cầu phải chứa cách triển khai được chấp nhận và các ví dụ hoạt động chính xác, nên việc trình bày thuật toán đó làm giải pháp cho vấn đề được cung cấp sẽ gây hiểu nhầm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^{k+2})`|`O(n)`| Quá chậm | 
| Giải pháp đồ thị hàm số cho định nghĩa đã nêu |`O(n)`|`O(n)`| Giải quyết vấn đề bằng văn bản nhưng không đồng ý với các mẫu | 

## Hướng dẫn thuật toán 

Để hoàn thiện, mâu thuẫn có thể được thiết lập mà không cần bất kỳ thuật toán phức tạp nào. 

1. Giữ các cạnh cố định`0 -> 0`,`1 -> 1`, Và`2 -> 2`. Mỗi ngôi sao trong số ba ngôi sao đó có chính xác một ngôi sao có thể tiếp cận được. 
2. Hoàn thành các cạnh chưa biết bằng`3 -> 4`Và`4 -> 3`. Đây là sự hoàn thành hợp lệ vì cả hai điểm đến đều là chỉ số sao hợp lệ. 
3. Bắt đầu từ ngôi sao`3`, trình tự là`3, 4, 3, 4, ...`, vì vậy các ngôi sao có thể tiếp cận khác biệt là`{3, 4}`Và`c_3 = 2`. 
4. Bắt đầu từ ngôi sao`4`, chu trình tương tự được truyền theo hướng ngược lại, vì vậy`c_4 = 2`. 
5. Sức hấp dẫn mang lại là`1 + 1 + 1 + 2 + 2 = 7`. 
6. Vì`7 > 3`, mức tối đa được yêu cầu của mẫu đầu tiên không thể tuân theo định nghĩa được cung cấp. 

Mẫu thứ hai thậm chí còn đưa ra một mâu thuẫn đơn giản hơn. Đặt bảy cạnh chưa biết thành một bảy chu kỳ khiến mọi ngôi sao đều đạt đến đủ bảy ngôi sao, tạo nên sức hấp dẫn`49`, trong khi mẫu tuyên bố`42`. 

Bất biến đằng sau mâu thuẫn chỉ đơn giản là mọi đỉnh trong một chu trình có hướng đều có thể chạm tới mọi đỉnh của chu trình đó. Không có cách giải thích nào về các quy tắc biểu đồ đã cho chỉ có thể đóng góp bảy chu kỳ`6`sao có thể tiếp cận nếu`c_i`đếm các ngôi sao riêng biệt có thể tiếp cận được từ đỉnh bắt đầu như tuyên bố đã nói. 

## Giải pháp Python 

Một bản đệ trình không thể được cung cấp một cách trung thực cho vấn đề một cách chính xác như đã được cung cấp, bởi vì bất kỳ việc triển khai đúng định nghĩa đã nêu đều phải từ chối các mẫu được cung cấp. 

Ví dụ: trình xác minh nhỏ sau đây thể hiện trực tiếp mâu thuẫn đầu tiên:```python
import sys
input = sys.stdin.readline

def attractiveness(f):
    n = len(f)
    ans = 0

    for start in range(n):
        seen = set()
        v = start

        while v not in seen:
            seen.add(v)
            v = f[v]

        ans += len(seen)

    return ans

f = [0, 1, 2, 4, 3]
print(attractiveness(f))
```Nó in:```
7
```Việc thực hiện tuân theo chính xác định nghĩa của`c_i`: theo dõi nhiều lần`f`, ghi lại từng ngôi sao riêng biệt gặp phải và đếm tập hợp kết quả. 

Đối với mẫu hoàn toàn chưa biết, việc chọn bảy chu kỳ tương tự sẽ tạo ra`49`. 

Vấn đề triển khai quan trọng ở đây không phải là tràn số nguyên, độ sâu đệ quy hoặc truyền tải đồ thị. Vấn đề là không có cách triển khai nào có thể đồng thời đáp ứng được định nghĩa bằng văn bản và kết quả đầu ra dự kiến ​​được cung cấp. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, sự hoàn thành quyết định là`f = [0, 1, 2, 4, 3]`. 

| Ngôi sao khởi đầu | Truyền tải | Những ngôi sao có thể tiếp cận khác biệt |`c_i`| 
| --- | --- | --- | --- | 
| 0 |`0 -> 0 -> ...`|`{0}`| 1 | 
| 1 |`1 -> 1 -> ...`|`{1}`| 1 | 
| 2 |`2 -> 2 -> ...`|`{2}`| 1 | 
| 3 |`3 -> 4 -> 3 -> ...`|`{3, 4}`| 2 | 
| 4 |`4 -> 3 -> 4 -> ...`|`{3, 4}`| 2 | 

Tổng cộng là`7`. Vì đây là lần hoàn thành hợp lệ nên mức tối đa ít nhất phải là`7`. 

Đối với mẫu thứ hai, chọn hoàn thành```
f = [1, 2, 3, 4, 5, 6, 0]
```| Ngôi sao khởi đầu | Đã đạt đến chu kỳ | Những ngôi sao có thể tiếp cận khác biệt |`c_i`| 
| --- | --- | --- | --- | 
| 0 |`0 -> 1 -> ... -> 6 -> 0`| toàn 7 sao | 7 | 
| 1 | cùng chu kỳ | toàn 7 sao | 7 | 
| 2 | cùng chu kỳ | toàn 7 sao | 7 | 
| 3 | cùng chu kỳ | toàn 7 sao | 7 | 
| 4 | cùng chu kỳ | toàn 7 sao | 7 | 
| 5 | cùng chu kỳ | toàn 7 sao | 7 | 
| 6 | cùng chu kỳ | toàn 7 sao | 7 | 

Tổng cộng là`49`, mâu thuẫn`42`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Xác minh trực tiếp việc hoàn thành |`O(n^2)`| Bắt đầu duyệt từ mọi đỉnh | 
| Tìm kiếm hoàn thành đầy đủ |`O(n^{k+2})`|`n^k`nhiệm vụ của`k`cạnh chưa biết | 
| Giải bài toán bằng đồ thị hàm số |`O(n)`| Mỗi đỉnh và cạnh đã biết có thể được xử lý một số lần không đổi | 

Ràng buộc đã nêu rằng tổng`n`trong tất cả các bài kiểm tra nhiều nhất là`5 * 10^5`rõ ràng loại trừ bất cứ điều gì bậc hai hoặc tệ hơn. Một giải pháp thực sự được chấp nhận về cơ bản phải là tuyến tính hoặc`O(n log n)`. Tuy nhiên, tuyên bố và mẫu được cung cấp không xác định được vấn đề nhất quán, do đó chỉ riêng độ phức tạp không thể giải quyết được sự khác biệt. 

## Trường hợp thử nghiệm 

Các khẳng định sau đây thể hiện sự mâu thuẫn hơn là kiểm tra một bài nộp được chấp nhận.```
def attractiveness(f):
    n = len(f)
    ans = 0

    for start in range(n):
        seen = set()
        v = start

        while v not in seen:
            seen.add(v)
            v = f[v]

        ans += len(seen)

    return ans

# First supplied sample, with a valid completion of the unknown edges.
assert attractiveness([0, 1, 2, 4, 3]) == 7

# All seven stars can form one cycle.
assert attractiveness([1, 2, 3, 4, 5, 6, 0]) == 49

# Minimum-size case.
assert attractiveness([0]) == 1

# Three independent self-loops.
assert attractiveness([0, 1, 2]) == 3

# A three-cycle.
assert attractiveness([1, 2, 0]) == 9
```| Kiểm tra đầu vào/hoàn thành | Sức hấp dẫn dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[0, 1, 2, 4, 3]`|`7`| Trực tiếp mâu thuẫn với mẫu được cung cấp 1 | 
|`[1, 2, 3, 4, 5, 6, 0]`|`49`| Trực tiếp mâu thuẫn với mẫu được cung cấp 2 | 
|`[0]`|`1`| Đồ thị hàm số kích thước tối thiểu | 
|`[0, 1, 2]`|`3`| Vòng tự độc lập | 
|`[1, 2, 0]`|`9`| Tất cả các đỉnh trong một chu kỳ ba | 

## Vỏ cạnh 

Trường hợp cạnh không rõ ràng đầu tiên chính xác là mẫu đầu tiên. Các cạnh không xác định không bị cấm trỏ vào nhau. Một lần`3 -> 4`Và`4 -> 3`được chọn thì các đỉnh đó tạo thành một hai chu trình hợp lệ, do đó đóng góp của chúng đều là`2`. Bất kỳ thuật toán nào âm thầm rời đi`-1`các đỉnh nằm ngoài biểu đồ sẽ tạo ra mẫu`3`, nhưng nó sẽ không giải quyết được vấn đề hoàn thành đã nêu. 

Trường hợp cạnh thứ hai là một mảng hoàn toàn chưa xác định. Với bảy đỉnh, bảy chu kỳ là sự hoàn thành hợp pháp. Mọi đỉnh đều đạt đến đủ bảy đỉnh nên độ hấp dẫn là`49`. Mẫu của`42`sẽ tương ứng với việc chỉ đếm sáu đỉnh cho mỗi điểm bắt đầu, đây là một định nghĩa khác về`c_i`. 

Trường hợp ranh giới hữu ích thứ ba là`n = 1`với`f = [-1]`. Sự hoàn thành duy nhất có thể là`f_0 = 0`, mang lại một ngôi sao có thể tiếp cận và sự hấp dẫn`1`. Bất kỳ cách giải thích nào theo đó câu trả lời là`0`một lần nữa sẽ thay đổi ý nghĩa của khả năng tiếp cận. 

Bản thân trang vấn đề xác nhận kết quả đầu ra mẫu được hiển thị là`3`Và`42`, vì vậy đây không phải là sự khác biệt về phiên âm giữa dấu nhắc và Codeforces. 

**Vui lòng cung cấp định nghĩa gốc/cập nhật của`c_i`hoặc điều kiện còn thiếu về cách thức`-1`các giá trị có thể được hoàn thành.** Với sự điều chỉnh đó, toàn bộ biên tập, bằng chứng, triển khai Python, dấu vết và bộ thử nghiệm được chấp nhận có thể được rút ra một cách nhất quán.
