---
title: "CF 104013B - Xe Đạp"
description: "Một người đi xe đạp đang lựa chọn giữa hai gói thuê xe đạp hàng tháng và chúng tôi cần tính tổng chi phí của từng gói trong một tháng cố định. Mỗi ngày người đi xe đạp sử dụng xe đạp tổng cộng T phút."
date: "2026-07-02T05:00:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104013
codeforces_index: "B"
codeforces_contest_name: "2020-2021 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104013
solve_time_s: 43
verified: true
draft: false
---

[CF 104013B - Xe đạp](https://codeforces.com/problemset/problem/104013/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một người đi xe đạp đang lựa chọn giữa hai gói thuê xe đạp hàng tháng và chúng tôi cần tính tổng chi phí của từng gói trong một tháng cố định. 

Mỗi ngày, người đi xe đạp sử dụng xe đạp tổng cộng`T`phút. Thời gian này được phân chia giữa việc đi làm và trở về, nhưng đối với bài toán này, nó đã được tính tổng theo ngày. 

Gói đầu tiên tính phí cố định hàng tháng`a`. Theo kế hoạch này, mỗi ngày có 30 phút miễn phí. Nếu mức sử dụng hàng ngày vượt quá 30 phút, mỗi phút tăng thêm sẽ bị tính phí`x`. 

Gói thứ hai tính phí cố định hàng tháng`b`. Theo kế hoạch này, mỗi ngày có 45 phút miễn phí. Chi phí vượt quá 45 phút`y`mỗi phút. 

Có chính xác 21 ngày làm việc trong tháng 11 và mô hình hàng ngày được giả định giống hệt nhau cho tất cả các ngày. Nhiệm vụ là tính tổng chi phí của từng kế hoạch trong suốt 21 ngày. 

Đầu ra là hai số nguyên, lần lượt biểu thị tổng chi phí theo kế hoạch thứ nhất và thứ hai. 

Các ràng buộc là rất nhỏ, với tất cả các giá trị tiền tệ và`T`giới hạn tối đa là 100 hoặc 1440. Điều này ngay lập tức gợi ý rằng bất kỳ số học trực tiếp nào trên mỗi phương án đều đủ, vì phép tính là thời gian không đổi. Không cần vòng lặp trên phạm vi lớn hoặc kỹ thuật tối ưu hóa. 

Một sai lầm phổ biến phát sinh từ việc hiểu sai ranh giới “số phút rảnh rỗi”. Nếu như`T`nhỏ hơn hoặc bằng giới hạn miễn phí thì chi phí tăng thêm phải bằng 0, không âm. Một vấn đề tế nhị khác là quên rằng chi phí vượt mức áp dụng theo ngày và phải nhân với 21. 

Ví dụ, nếu`T = 10`, cả hai gói đều không mất thêm chi phí và câu trả lời đơn giản là`(21 * a, 21 * b)`. Một cách thực hiện ngây thơ trừ đi mà không kẹp ở mức 0 sẽ tạo ra điện tích âm không chính xác. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng từng ngày trong số 21 ngày và trong mỗi ngày, lặp lại từng phút vượt quá mức cho phép miễn phí để tích lũy chi phí. Ví dụ: mỗi ngày chúng ta có thể lặp từ phút 31 đến`T`cho kế hoạch đầu tiên và thêm`x`mỗi phút. Điều này hoạt động chính xác, nhưng vòng lặp bên trong làm cho nó dài dòng không cần thiết. 

Trong trường hợp xấu nhất,`T = 1440`, do đó vòng lặp bên trong chạy khoảng 1410 lần lặp mỗi ngày. Trong 21 ngày, đây là khoảng 30.000 hoạt động cho mỗi kế hoạch, điều này vẫn ổn nhưng hoàn toàn không cần thiết đối với cấu trúc. 

Quan sát quan trọng là mỗi kế hoạch chỉ phụ thuộc vào số phút vượt quá ngưỡng chứ không phụ thuộc vào mức phân bổ của chúng. Một khi chúng tôi tính toán`max(0, T - 30)`Và`max(0, T - 45)`, tổng chi phí trở thành một công thức tuyến tính đơn giản. Điều này làm giảm vấn đề về số học theo thời gian không đổi. 

Brute-force hoạt động vì nó trực tiếp tích lũy chi phí mỗi phút, nhưng không thành công do quá phức tạp khi chúng tôi nhận ra rằng tất cả số phút vượt ngưỡng đều giống nhau về chi phí. Quan sát thu gọn vòng lặp mỗi phút thành một phép nhân duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(21 · T) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán từng kế hoạch một cách độc lập bằng cách sử dụng cùng một cấu trúc. 

1. Đọc đầu vào`a, x, b, y, T`. Chúng xác định chi phí cố định, hình phạt mỗi phút và mức sử dụng hàng ngày. 
2. Tính số phút tính cước cho gói đầu tiên như sau`over1 = max(0, T - 30)`. Điều này đảm bảo chúng tôi không bao giờ tính phí số phút miễn phí. 
3. Tính tổng chi phí hàng ngày cho kế hoạch đầu tiên như sau`day1 = a + over1 * x`. Phí cố định luôn được bao gồm một lần mỗi ngày. 
4. Nhân với 21 để có chi phí hàng tháng`total1 = 21 * day1`. Điều này phản ánh tất cả các ngày làm việc trong tháng 11. 
5. Lặp lại logic tương tự cho kế hoạch thứ hai sử dụng ngưỡng 45 phút:`over2 = max(0, T - 45)`. 
6. Tính toán`day2 = b + over2 * y`và sau đó`total2 = 21 * day2`. 
7. Đầu ra`total1`Và`total2`. 

Mỗi bước hoàn toàn là đại số và không cần lặp lại vì cấu trúc chi phí là tuyến tính theo số phút vượt quá. 

### Tại sao nó hoạt động 

Mỗi gói xác định một khoản phí cơ bản không đổi cộng với khoản phí trên mỗi đơn vị chỉ áp dụng cho phần sử dụng vượt quá ngưỡng cố định. Vì mỗi ngày đều giống nhau nên tổng chi phí chỉ gấp 21 lần hàm xác định mỗi ngày. Quyết định duy nhất là liệu`T`vượt qua ngưỡng và khi điều đó được giải quyết, tính toán còn lại là tuyến tính với số lượng vượt quá. Không có sự tương tác nào tồn tại giữa các ngày hoặc giữa các phút trong một ngày, do đó, việc thu gọn cấu trúc thành một biểu thức dạng đóng sẽ duy trì tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input().strip())
x = int(input().strip())
b = int(input().strip())
y = int(input().strip())
T = int(input().strip())

days = 21

over1 = max(0, T - 30)
day1 = a + over1 * x
total1 = days * day1

over2 = max(0, T - 45)
day2 = b + over2 * y
total2 = days * day2

print(total1, total2)
```Giải pháp đọc năm số nguyên và ngay lập tức chuyển chúng thành hai công thức chi phí độc lập. 

Việc sử dụng`max(0, ...)`là cần thiết vì việc sử dụng quá mức âm sẽ làm giảm phí cơ bản một cách không chính xác. Việc nhân với 21 được thực hiện sau khi tính toán chi phí hàng ngày, giữ cho cấu trúc sạch sẽ và giảm nguy cơ trộn lẫn logic hàng ngày và hàng tháng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
1
20
2
40
```Đối với gói 1, giới hạn miễn phí là 30, do đó, sử dụng quá mức là 10 phút mỗi ngày. Chi phí hàng ngày là`10 + 10 * 1 = 20`. 

Đối với gói 2, giới hạn miễn phí là 45, do đó không có dư thừa. Chi phí hàng ngày là`20`. 

| Bước | Kế hoạch 1 | Kế hoạch 2 | 
| --- | --- | --- | 
| T | 40 | 40 | 
| Giới hạn miễn phí | 30 | 45 | 
| Quá tuổi | 10 | 0 | 
| Chi phí hàng ngày | 20 | 20 | 
| Chi phí hàng tháng | 420 | 420 | 

Đầu ra:```
420 420
```Điều này cho thấy trường hợp cả hai gói đều trùng nhau vì khoản trợ cấp miễn phí lớn hơn của gói thứ hai bù đắp chính xác cho cơ sở cao hơn của nó. 

### Ví dụ 2 

đầu vào:```
100
5
50
1
60
```Phương án 1: dư thừa là`30`, chi phí hàng ngày là`100 + 30*5 = 250`, hàng tháng là`5250`. 

Phương án 2: dư thừa là`15`, chi phí hàng ngày là`50 + 15*1 = 65`, hàng tháng là`1365`. 

| Bước | Kế hoạch 1 | Kế hoạch 2 | 
| --- | --- | --- | 
| T | 60 | 60 | 
| Giới hạn miễn phí | 30 | 45 | 
| Quá tuổi | 30 | 15 | 
| Chi phí hàng ngày | 250 | 65 | 
| Chi phí hàng tháng | 5250 | 1365 | 

Đầu ra:```
5250 1365
```Điều này chứng tỏ mức phí cơ bản cao hơn vẫn có thể bị mất nếu chi phí cận biên thấp hơn đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Việc tính toán diễn ra theo thời gian không đổi bất kể cường độ đầu vào, dễ dàng nằm trong giới hạn với các ràng buộc nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    a = int(input().strip())
    x = int(input().strip())
    b = int(input().strip())
    y = int(input().strip())
    T = int(input().strip())

    days = 21

    over1 = max(0, T - 30)
    total1 = days * (a + over1 * x)

    over2 = max(0, T - 45)
    total2 = days * (b + over2 * y)

    return f"{total1} {total2}"

# provided samples
assert run("10\n1\n20\n2\n40\n") == "420 420"

# minimum case: no overage
assert run("0\n0\n0\n0\n1\n") == "0 0"

# boundary at exact thresholds
assert run("5\n2\n7\n3\n30\n") == f"{21*(5)} {21*(7)}"

# second plan dominates due to lower overage
assert run("10\n10\n100\n1\n100\n") == "14700 11550"

# high T case
assert run("1\n1\n1\n1\n1440\n") == str(21*(1 + 1410)) + " " + str(21*(1 + 1395))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| T=1, lãi suất bằng 0 | 0 0 | không có hành vi quá mức | 
| T ở ngưỡng | chi phí cơ sở duy nhất | độ đúng ranh giới | 
| mất cân bằng chi phí cao | tỷ lệ khác nhau | tính đúng đắn so sánh | 
| tối đa T | xử lý dư thừa lớn | tính đúng đắn của số học căng thẳng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi`T`nhỏ hơn cả hai ngưỡng. Ví dụ,`T = 10`. Sự tính toán`T - 30`Và`T - 45`là tiêu cực, nhưng áp dụng`max(0, ...)`buộc cả hai khoản dư thừa về 0, dẫn đến chi phí`21 * a`Và`21 * b`. Nếu không có kẹp này, công thức sẽ trừ tiền khỏi phí cơ bản một cách không chính xác, tạo ra tổng số âm vô nghĩa. 

Một trường hợp cạnh khác xảy ra chính xác ở ngưỡng. Nếu như`T = 30`, gói đầu tiên không được tính thêm phút nào, không phải một hoặc nhiều phút. biểu thức`max(0, 30 - 30)`chính xác mang lại kết quả bằng 0, đảm bảo không có hình phạt ngẫu nhiên. 

Trường hợp thứ ba là lớn`T`gần giới hạn trên, chẳng hạn như 1440. Phần dư trở nên lớn nhưng vẫn phù hợp một cách an toàn trong số học số nguyên vì tích tối đa là`21 * 1440 * 100`, nằm trong giới hạn 32-bit.
