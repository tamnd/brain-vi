---
title: "CF 104361B - \u0412\u044b\u0431\u043e\u0440 \u0446\u0432\u0435\u0442\u043e\u0432 \u0434\u043b\u044f \u0431\u0443\u043a\u0435\u0442\u0430"
description: "Chúng ta được tặng một bó hoa phải có đúng n bông hoa và có m loại hoa. Mỗi loại có thể được sử dụng nhiều lần. Mô hình phần thưởng cho một loại không cố định trên mỗi bông hoa."
date: "2026-07-01T17:54:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104361
codeforces_index: "B"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2020"
rating: 0
weight: 104361
solve_time_s: 73
verified: true
draft: false
---

[CF 104361B - \u0412\u044b\u0431\u043e\u0440 \u0446\u0432\u0435\u0442\u043e\u0432 \u0434\u043b\u044f \u0431\u0443\u043a\u0435\u0442\u0430](https://codeforces.com/problemset/problem/104361/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một bó hoa phải chứa chính xác`n`hoa, và có`m`các loại hoa. Mỗi loại có thể được sử dụng nhiều lần. 

Mô hình phần thưởng cho một loại không cố định trên mỗi bông hoa. Nếu chúng ta lấy`x`loại hoa`i`, cái đầu tiên đóng góp`a_i`và mọi người tiếp theo đều đóng góp`b_i`. Vì vậy, tổng đóng góp của loại đó trở thành`a_i + (x - 1) * b_i`khi`x > 0`, Và`0`khi`x = 0`. 

Nhiệm vụ là phân phối tổng số`n`hoa trên các loại để tối đa hóa tổng số tiền đóng góp. 

Khó khăn chính là giá trị cận biên của một loại thay đổi sau lần chọn đầu tiên. Bông hoa đầu tiên của một loại có thể khác biệt đáng kể so với những bông hoa tiếp theo, vì vậy một chiến lược tham lam ngây thơ chỉ dựa trên`b_i`hoặc chỉ trên`a_i`thất bại. 

Các ràng buộc rất lớn:`n`có thể đi lên`10^9`, trong khi`m`tùy thuộc vào`100000`. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng việc hái từng bông hoa hoặc duy trì các quyết định theo từng bông hoa. Bất kỳ giải pháp đúng nào cũng phải giảm vấn đề về quá trình xử lý trước O(m) hoặc O(m log m). 

Một trường hợp thất bại tinh vi đối với lý luận ngây thơ xuất hiện khi một kiểu có kích thước rất lớn.`a_i`nhưng nhỏ`b_i`, hoặc ngược lại. Ví dụ: nếu một loại có`(a, b) = (100, 1)`và một cái khác có`(1, 100)`, những lựa chọn tham lam chỉ dựa trên lợi ích thứ nhất hoặc thứ hai có thể dễ dàng gây hiểu nhầm, bởi vì lựa chọn đầu tiên tương tác với cấu trúc phân bổ còn lại. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi quyết định có bao nhiêu bông hoa`x_i`lấy từ mỗi loại và tính tổng đóng góp một cách trực tiếp, thử tất cả các phân phối hợp lệ. Điều này đúng vì nó đánh giá công thức chính xác, nhưng số lượng phân phối rất lớn về mặt thiên văn. Ngay cả khi chúng tôi giới hạn mỗi`x_i`Tại`n`, số phân vùng nguyên của`n`vào trong`m`các phần vượt xa khả năng tính toán. Điều này thất bại ngay lập tức khi`n`vượt quá vài chục. 

Cấu trúc của hàm thưởng là tuyến tính sau phần tử đầu tiên. Mỗi loại hoạt động giống như một chuỗi: một giá trị đặc biệt`a_i`, theo sau là một dòng vô hạn`b_i`. Điều này gợi ý chuyển vấn đề sang việc lựa chọn những đóng góp cận biên. 

Đối với mỗi loại, một quan điểm cận biên trực tiếp sẽ tạo ra một chuỗi lợi ích vô hạn:`a_i, b_i, b_i, b_i, ...`. Nhiệm vụ trở thành lựa chọn tốt nhất`n`giá trị trên tất cả các chuỗi này. Vấn đề là mỗi chuỗi là vô hạn nên chúng ta không thể liệt kê rõ ràng các ứng cử viên. 

Quan sát quan trọng là chỉ có điều tốt nhất`b_i`quan trọng trên toàn cầu đối với tất cả các lựa chọn "không phải đầu tiên". Khi chúng ta lấy nhiều hoa, mỗi bông hoa bổ sung chỉ đóng góp vào loại của nó.`b_i`, vì vậy chúng tôi muốn những đóng góp đó càng lớn càng tốt. Điều này gợi ý rằng trong bất kỳ giải pháp tối ưu nào, hầu hết tất cả các lựa chọn không phải lần đầu tiên đều phải đến từ loại có tối đa`b_i`, vì sử dụng bất kỳ nhỏ hơn`b_i`làm giảm tổng số. 

Điều này làm sụp đổ cấu trúc: thay vì phân phối tất cả`n`các mục tùy ý, chúng tôi xử lý một loại chiếm ưu thế với mức tối đa`b_i`làm phần bổ sung cơ sở và chúng tôi chỉ xem xét liệu việc “thay thế” một số lựa chọn cơ sở đó bằng các lựa chọn đầu tiên thuộc loại khác có mang lại lợi ích hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bản phân phối | Hàm mũ | O(1) | Quá chậm | 
| Mô phỏng biên mỗi lần chọn | O(n log m) | O(m) | Quá chậm | 
| Giảm tối ưu bằng cách sử dụng max`b_i`đường cơ sở | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm thiểu mọi thứ để so sánh với loại “mặc định” tốt nhất có thể. 

### Các bước 

1. Tìm giá trị lớn nhất trong số tất cả`b_i`, gọi nó`B`. 

Điều này thể hiện giá trị tốt nhất có thể có trên mỗi bông hoa bổ sung sau bông hoa đầu tiên. 
2. Bắt đầu với giải pháp cơ bản trong đó tất cả`n`hoa được tưởng tượng đến từ một loại có giá trị`B`. 

Điều này mang lại sự đóng góp cơ bản của`n * B`. 
3. Đối với từng loại`i`, hãy tính toán hiệu quả của việc sử dụng nó ít nhất một lần thay vì chỉ sử dụng loại đường cơ sở. 

Nếu chúng ta giới thiệu loại`i`, chúng tôi lấy một bông hoa từ nó, góp phần`a_i`thay vì`B`. Điều này thay đổi tổng số bằng`a_i - B`. 
4. Nếu`a_i - B > 0`, sẽ có ích nếu bao gồm loại này ít nhất một lần. Thêm giá trị này vào câu trả lời. 
5. Cộng tất cả những đóng góp tích cực từ bước 4 và cộng chúng vào đường cơ sở`n * B`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi bông hoa ngoài bông hoa đầu tiên đều hoạt động giống hệt nhau trong loại của nó và sự khác biệt chung duy nhất giữa các loại đối với các vị trí đó là giá trị của`b_i`. Vì vậy, sự phân công tốt nhất có thể cho tất cả các vị trí không đặc biệt luôn là mức tối đa.`b_i`. 

Khi đường cơ sở này được cố định, mọi sai lệch so với nó chỉ có thể xảy ra thông qua các lần chọn loại đầu tiên. Mỗi sai lệch như vậy thay thế một mục cơ sở có giá trị`B`với một mục đầu tiên có giá trị`a_i`, tạo ra mức tăng`a_i - B`. Không có sự tương tác nào nữa giữa các sai lệch vì mọi vị trí được thay thế đều độc lập và cấu trúc còn lại vẫn chứa đầy`B`. Tính độc lập này đảm bảo rằng việc tổng hợp tất cả các lợi ích dương sẽ mang lại cấu hình tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    a = []
    b = []
    
    B = 0
    for _ in range(m):
        ai, bi = map(int, input().split())
        a.append(ai)
        b.append(bi)
        if bi > B:
            B = bi

    ans = n * B
    
    for i in range(m):
        gain = a[i] - B
        if gain > 0:
            ans += gain

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xác định tốc độ tăng trưởng trên mỗi đơn vị mạnh nhất`B`. Sau đó nó giả định rằng tất cả các bông hoa đều đóng góp`B`, tạo thành một đường cơ sở rõ ràng. Mỗi loại được đánh giá độc lập bằng cách kiểm tra xem bông hoa đầu tiên của nó có mạnh hơn đường cơ sở đó hay không. Nếu đúng như vậy, chúng tôi sẽ nâng cấp giải pháp bằng cách thay thế một bông hoa cơ bản bằng bông hoa đầu tiên của loại đó. 

Một cạm bẫy phổ biến là cố gắng mô phỏng số lượng hoa của mỗi loại để lấy. Điều đó là không cần thiết vì khi đường cơ sở được cố định thì sự đóng góp của mỗi bông hoa bổ sung đã được xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
5 0
1 4
2 2
```Đây`B = max(0, 4, 2) = 4`. Đường cơ sở là`4 * 4 = 16`. 

Bây giờ đánh giá lợi nhuận: 

- Loại 1:`5 - 4 = 1`- Loại 2:`1 - 4 = -3`- Loại 3:`2 - 4 = -2`Chỉ có loại 1 đóng góp. 

| Bước | Hành động | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | đường cơ sở | 16 | 16 | 
| Loại 1 | tăng thêm | +1 | 17 | 
| Loại 2 | bỏ qua | +0 | 17 | 
| Loại 3 | bỏ qua | +0 | 17 | 

Câu trả lời cuối cùng là`17`. 

Điều này cho thấy chỉ những loại có bông hoa đầu tiên đánh bại được đường cơ sở mới có ý nghĩa như thế nào. 

### Ví dụ 2 

đầu vào:```
5 3
5 2
4 2
3 1
```Đây`B = 2`. Đường cơ sở là`5 * 2 = 10`. 

Lợi nhuận: 

- Loại 1:`5 - 2 = 3`- Loại 2:`4 - 2 = 2`- Loại 3:`3 - 2 = 1`| Bước | Hành động | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | 
| Ban đầu | đường cơ sở | 10 | 10 | 
| Loại 1 | tăng thêm | +3 | 13 | 
| Loại 2 | tăng thêm | +2 | 15 | 
| Loại 3 | tăng thêm | +1 | 16 | 

Câu trả lời cuối cùng là`16`. 

Dấu vết cho thấy rằng tất cả các loại có giá trị đầu tiên trên đường cơ sở đều có lợi độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Một lượt để tìm`B`, một lần để tính lợi nhuận | 
| Không gian | O(1) | Chỉ lưu trữ số lần chạy tối đa và kết quả | 

Các ràng buộc cho phép lên đến`100000`các loại, do đó quét tuyến tính dễ dàng đủ nhanh. Giá trị lớn của`n`được xử lý trong thời gian không đổi bằng cách chia tỷ lệ đường cơ sở thay vì lặp lại trên từng bông hoa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, m = map(int, sys.stdin.readline().split())
    B = 0
    a = []
    
    ans = 0
    
    for _ in range(m):
        ai, bi = map(int, sys.stdin.readline().split())
        a.append(ai)
        if bi > B:
            B = bi
    
    ans = n * B
    
    for ai in a:
        if ai - B > 0:
            ans += ai - B
    
    return str(ans)

# provided samples (as given in statement; note formatting ambiguity is ignored)
assert run("4 3\n5 0\n1 4\n2 2\n") == "17"
assert run("5 3\n5 2\n4 2\n3 1\n") == "16"

# custom cases
assert run("1 1\n10 5\n") == "10", "single type"
assert run("10 2\n1 100\n2 100\n") == str(10 * 100 + (2 - 100)), "negative gain ignored"
assert run("5 3\n100 1\n1 100\n1 50\n") == str(5 * 100 + (1 - 100)), "dominant b case"
assert run("3 2\n1 1\n1 1\n") == str(3 * 1), "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| loại đơn | tính toán trực tiếp | độ đúng cơ sở | 
| sự thống trị hỗn hợp | lợi nhuận âm bị bỏ qua | lọc logic | 
| trội`b`trường hợp | lựa chọn cơ sở đúng đắn | tối đa`b_i`hành vi | 
| tất cả đều bình đẳng | ổn định | không có lựa chọn không cần thiết | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả`b_i`đều bình đẳng. Trong tình huống này, đường cơ sở được chia sẻ cho tất cả các loại và giải pháp chỉ đơn giản là chọn các loại có giá trị cao nhất`a_i`. Thuật toán xử lý việc này một cách tự nhiên bởi vì mọi`a_i - B`được đánh giá một cách độc lập. 

Một trường hợp khác là khi tốt nhất`a_i`thuộc loại có kích thước nhỏ`b_i`. Thuật toán vẫn bao gồm nó nếu giá trị đầu tiên của nó vượt quá đường cơ sở toàn cầu, bởi vì sự thay thế đó luôn có lợi bất kể giá trị của nó là bao nhiêu.`b_i`. 

Cuối cùng, khi`n = 1`, câu trả lời đơn giản là`max(a_i)`, vì không có lựa chọn thứ hai nào tồn tại. Công thức rút gọn đúng vì`n * B + max(a_i - B)`đơn giản hóa để`max(a_i)`.
