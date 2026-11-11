---
title: "CF 1585A - Cuộc đời của một bông hoa"
description: "Chúng ta được cung cấp lịch sử tưới nước của một bông hoa trong vài ngày. Mỗi phần tử của mảng đại diện cho một ngày. Giá trị 1 có nghĩa là bông hoa đã được tưới nước, trong khi giá trị 0 có nghĩa là nó bị bỏ quên. Bông hoa bắt đầu với chiều cao 1."
date: "2026-06-10T09:30:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1585
codeforces_index: "A"
codeforces_contest_name: "Technocup 2022 - Elimination Round 3"
rating: 800
weight: 1585
solve_time_s: 217
verified: true
draft: false
---

[CF 1585A - Cuộc đời của một bông hoa](https://codeforces.com/problemset/problem/1585/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp lịch sử tưới nước của một bông hoa trong vài ngày. Mỗi phần tử của mảng đại diện cho một ngày. Một giá trị của`1`có nghĩa là hoa đã được tưới nước, trong khi`0`có nghĩa là nó đã bị bỏ quên. 

Bông hoa bắt đầu từ chiều cao`1`. Mỗi ngày tưới nước đều tăng chiều cao, nhưng số lượng phụ thuộc vào những gì đã xảy ra vào ngày hôm trước. Nếu hai ngày liên tiếp đều được tưới nước thì ngày hiện tại sẽ đóng góp`5`cm. Nếu không thì một ngày tưới nước chỉ góp phần`1`. Một ngày khô ráo chẳng đóng góp được gì. Nếu xuất hiện hai ngày khô hạn liên tiếp, bông hoa sẽ chết ngay lập tức và câu trả lời sẽ trở thành`-1`. 

Số lượng ca kiểm thử nhiều nhất là`100`và mỗi ca kiểm thử chứa tối đa`100`ngày. Ngay cả một thuật toán kiểm tra từng ngày một cũng chỉ thực hiện được khoảng mười nghìn thao tác, một con số rất nhỏ. Không cần bất cứ điều gì phức tạp hơn quét tuyến tính. 

Một số tình huống rất dễ xử lý sai. 

Một trường hợp là khi ngày đầu tiên khô ráo. Ví dụ:```
1
1
0
```Câu trả lời là`1`, không`0`. Bông hoa đã có chiều cao`1`trước khi bất kỳ ngày nào bắt đầu. 

Một trường hợp khác là một cặp ngày khô hạn liên tiếp.```
1
4
1 0 0 1
```Câu trả lời đúng là`-1`. Việc thực hiện bất cẩn có thể đơn giản bỏ qua những ngày khô ráo và tiếp tục xử lý, tạo ra chiều cao dương không chính xác. 

Những ngày tưới nước liên tục cũng cần được chú ý.```
1
3
0 1 1
```Câu trả lời là`7`. Ngày thứ hai đóng góp`1`, và đóng góp thứ ba`5`, do đó chiều cao cuối cùng trở thành`1 + 1 + 5 = 7`. Xử lý mỗi ngày tưới nước như bổ sung`1`sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là mô phỏng cuộc sống của loài hoa đúng như mô tả. Chúng tôi xử lý các ngày từ trái sang phải, duy trì chiều cao hiện tại và kiểm tra ngày hôm trước bất cứ khi nào chúng tôi cần xác định mức tăng trưởng mà ngày hiện tại tạo ra. Vì mỗi ngày chỉ được truy cập một lần nên trường hợp xấu nhất chỉ cần`100`hoạt động cho mỗi trường hợp thử nghiệm. 

Mô phỏng lực lượng vũ phu đã phù hợp thoải mái trong giới hạn. Không cần lập trình động hoặc tiền xử lý vì hiệu ứng của mỗi ngày chỉ phụ thuộc vào ngày trước đó. Bản chất cục bộ của các quy tắc có nghĩa là chúng ta có thể quyết định mọi thứ ngay lập tức trong khi quét mảng. 

Quan sát quan trọng là chỉ có ba mẫu quan trọng. một cặp`0 0`giết chết bông hoa. một cặp`1 1`thêm vào`5`cho ngày thứ hai. Một người bị cô lập`1`thêm vào`1`. Vì những điều kiện này chỉ phụ thuộc vào các phần tử lân cận nên chỉ cần duyệt qua mảng một lần là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo chiều cao của bông hoa là`1`, vì đó là chiều cao bắt đầu trước khi bất kỳ ngày nào được xử lý. 
2. Duyệt mảng từ trái sang phải. 
3. Nếu ngày hiện tại và ngày hôm trước đều giống nhau`0`, dừng lại ngay lập tức và đánh dấu câu trả lời là`-1`. Hoa tàn ngay khi xuất hiện hai ngày khô hạn liên tiếp. 
4. Nếu ngày hiện tại được tưới nước và ngày hôm trước cũng được tưới nước, hãy thêm`5`đến độ cao. Quy tắc cho biết ngày hiện tại đóng góp`5`thay vì`1`. 
5. Nếu ngày hôm nay được tưới nước nhưng ngày hôm trước khô ráo, hãy thêm`1`đến độ cao. 
6. Nếu ngày hiện tại khô, không thêm gì cả. 
7. Sau khi tất cả các ngày được xử lý, xuất chiều cao trừ khi hoa tàn sớm hơn. 

### Tại sao nó hoạt động 

Sau ngày xử lý`i`, chiều cao được lưu trữ bằng chiều cao thực tế của bông hoa sau những giá trị đó`i`ngày, miễn là bông hoa vẫn còn sống. Mọi cặp ngày liên tiếp có thể đều được xử lý theo quy định. MỘT`0 0`cặp đôi gây ra sự chấm dứt ngay lập tức, trong khi mỗi ngày tưới nước đóng góp chính xác lượng nước được chỉ định bởi vấn đề. Vì thuật toán tái tạo hiệu ứng của mỗi ngày theo trình tự thời gian nên kết quả cuối cùng phải khớp với chiều cao thật của bông hoa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        height = 1
        alive = True

        for i in range(n):
            if a[i] == 1:
                if i > 0 and a[i - 1] == 1:
                    height += 5
                else:
                    height += 1
            else:
                if i > 0 and a[i - 1] == 0:
                    alive = False
                    break

        if alive:
            ans.append(str(height))
        else:
            ans.append("-1")

    sys.stdout.write("\n".join(ans))

solve()
```Biến`height`bắt đầu từ`1`, phù hợp với trạng thái ban đầu của hoa. Vòng lặp xử lý ngày theo thứ tự thời gian. Trong một ngày đầy nước, chúng ta nhìn lại một vị trí để quyết định xem sự đóng góp có`1`hoặc`5`. 

điều kiện`i > 0`ngăn chặn truy cập`a[-1]`khi xử lý ngày đầu tiên. Trường hợp ranh giới này rất dễ bị bỏ qua. 

Ngay khi tìm thấy một cặp số 0 liên tiếp, vòng lặp sẽ dừng lại. Tiếp tục đi xa hơn sẽ vô nghĩa vì bông hoa đã tàn rồi. 

Vì chiều cao không bao giờ vượt quá vài trăm nên số nguyên thông thường là quá đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 0 1
```| Ngày | Giá trị | Giá trị trước đó | Chiều cao | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | 1 | 
| 1 | 1 | - | 2 | 
| 2 | 0 | 1 | 2 | 
| 3 | 1 | 0 | 3 | 

Bông hoa không bao giờ gặp phải hai ngày khô hạn liên tiếp. Cả hai ngày được tưới nước đều bị cô lập nên mỗi ngày chỉ đóng góp`1`. Chiều cao cuối cùng là`3`. 

### Ví dụ 2 

đầu vào:```
3
0 1 1
```| Ngày | Giá trị | Giá trị trước đó | Chiều cao | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | 1 | 
| 1 | 0 | - | 1 | 
| 2 | 1 | 0 | 2 | 
| 3 | 1 | 1 | 7 | 

Ví dụ này cho thấy tại sao những ngày tưới nước liên tiếp lại quan trọng. Ngày thứ ba đóng góp`5`, không`1`, tạo ra câu trả lời cuối cùng của`7`. 

### Ví dụ 3 

đầu vào:```
4
1 0 0 1
```| Ngày | Giá trị | Giá trị trước đó | Chiều cao | 
| --- | --- | --- | --- | 
| Ban đầu | - | - | 1 | 
| 1 | 1 | - | 2 | 
| 2 | 0 | 1 | 2 | 
| 3 | 0 | 0 | Chết | 

Hoa tàn vào ngày thứ ba vì hai ngày khô liên tiếp. Những ngày còn lại không còn quan trọng nữa và câu trả lời là`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngày được xử lý đúng một lần | 
| Không gian | O(1) | Chỉ có một số biến được duy trì | 

Ngay cả với kích thước đầu vào tối đa, thuật toán chỉ thực hiện vài trăm thao tác cho mỗi trường hợp thử nghiệm. Cả thời gian và mức sử dụng bộ nhớ đều thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    import sys
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        height = 1
        alive = True

        for i in range(n):
            if a[i] == 1:
                if i > 0 and a[i - 1] == 1:
                    height += 5
                else:
                    height += 1
            else:
                if i > 0 and a[i - 1] == 0:
                    alive = False
                    break

        ans.append(str(height) if alive else "-1")

    return "\n".join(ans)

# provided sample
assert run(
"""4
3
1 0 1
3
0 1 1
4
1 0 0 1
1
0
"""
) == "3\n7\n-1\n1"

# minimum size
assert run(
"""1
1
1
"""
) == "2"

# all zeros
assert run(
"""1
3
0 0 0
"""
) == "-1"

# all ones
assert run(
"""1
4
1 1 1 1
"""
) == "18"

# consecutive zeros in the middle
assert run(
"""1
5
1 1 0 0 1
"""
) == "-1"

# alternating pattern
assert run(
"""1
6
1 0 1 0 1 0
"""
) == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`2`| Kích thước tối thiểu có tưới nước | 
|`1 / 3 / 0 0 0`|`-1`| Tử vong ngay lập tức do những ngày khô hạn liên tiếp | 
|`1 / 4 / 1 1 1 1`|`18`| Tăng trưởng tiền thưởng lặp đi lặp lại | 
|`1 / 5 / 1 1 0 0 1`|`-1`| Cái chết xảy ra ở giữa | 
|`1 / 6 / 1 0 1 0 1 0`|`4`| Mô hình xen kẽ không có tiền thưởng | 

## Vỏ cạnh 

Xem xét đầu vào```
1
1
0
```Thuật toán bắt đầu với chiều cao`1`. Ngày duy nhất khô ráo nên không có sự tăng trưởng nào xảy ra. Vì không có ngày trước nên điều kiện tử vong không thể kích hoạt. Đầu ra trở thành`1`. Bắt đầu từ độ cao`0`sẽ đưa ra câu trả lời sai. 

Coi như```
1
4
1 0 0 1
```Sau ngày đầu tiên, chiều cao trở nên`2`. Ngày thứ hai không đóng góp được gì. Vào ngày thứ ba, thuật toán thông báo rằng cả ngày hiện tại và ngày trước đó đều`0`, đánh dấu bông hoa đã chết và dừng lại. Đầu ra là`-1`. 

Coi như```
1
3
0 1 1
```Ngày đầu tiên không đóng góp được gì. Ngày thứ hai bổ sung`1`, tăng chiều cao lên`2`. Ngày thứ ba thấy giá trị trước đó là`1`, vì vậy nó thêm`5`, tạo ra chiều cao cuối cùng là`7`. Thuật toán phân biệt chính xác việc tưới nước riêng biệt với việc tưới nước liên tiếp.
