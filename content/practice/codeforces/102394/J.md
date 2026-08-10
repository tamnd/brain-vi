---
title: "CF 102394J - Chứng minh phỏng đoán"
description: "Đối với mỗi trường hợp thử nghiệm, chúng ta cần chia số nguyên n thành hai số nguyên dương x và y sao cho x là số nguyên tố, y là hợp số và x + y = n. Cả hai số đều phải nhỏ hơn n. Nếu không có sự phân chia như vậy, chúng ta in -1."
date: "2026-08-10T19:11:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102394
codeforces_index: "J"
codeforces_contest_name: "The 2019 China Collegiate Programming Contest Harbin Site"
rating: 0
weight: 102394
solve_time_s: 87
verified: true
draft: false
---

[CF 102394J - Chứng minh phỏng đoán](https://codeforces.com/problemset/problem/102394/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng ta cần chia số nguyên`n`thành hai số nguyên dương`x`Và`y`như vậy`x`là số nguyên tố,`y`là hợp chất và`x + y = n`. Hai số đó đều phải nhỏ hơn`n`. Nếu không có sự phân chia như vậy tồn tại, chúng tôi in`-1`. 

Có thể có tới`10^5`các trường hợp thử nghiệm, trong khi mỗi trường hợp`n`có thể lớn như`10^9`. Điều đó loại trừ các thuật toán thực hiện công việc tỷ lệ thuận với`n`cho mỗi trường hợp thử nghiệm. Ngay cả việc quét tuyến tính trên tất cả các giá trị có thể có của`x`sẽ yêu cầu lên tới khoảng`10^14`lặp đi lặp lại trên toàn bộ đầu vào. Chúng ta cần khai thác tính chất toán học của các số nguyên tố và hợp số cần thiết thay vì tìm kiếm thông qua các ứng viên. 

Số tổng hợp hữu ích nhỏ nhất là`4`. Sự ngang bằng của`n`cho chúng ta một cách xây dựng thậm chí còn đơn giản hơn. Nếu như`n`đều và đủ lớn, chọn`x = 2`làm cho`y = n - 2`, tức là chẵn. Mọi số chẵn lớn hơn`2`là tổng hợp, vì vậy điều này ngay lập tức hoạt động bất cứ khi nào`n - 2 >= 4`, nghĩa`n >= 6`. 

Nếu như`n`là số lẻ và đủ lớn, chọn`x = 3`làm cho`y = n - 3`, tức là chẵn. Đối với số lẻ`n >= 7`, chúng tôi có`y >= 4`, Vì thế`y`là tổng hợp. Vì vậy mỗi lẻ`n >= 7`cũng có nhà xây dựng ngay. 

Những giá trị nhỏ`1`bởi vì`5`cần được xử lý riêng. Vì`n = 1, 2, 3`, không có đủ chỗ cho số nguyên tố cộng với số tổng hợp. Vì`n = 4`, cách phân chia duy nhất có thể sử dụng số nguyên tố là`2 + 2`, Nhưng`2`là số nguyên tố chứ không phải là hợp số. Vì`n = 5`, các phân rã nguyên tố đầu tiên có thể là`2 + 3`Và`3 + 2`, và không có số thứ hai nào là hợp số. Do đó cả năm giá trị đều tạo ra`-1`. 

Ví dụ, đầu vào`1`phải sản xuất`-1`. Việc thực hiện bất cẩn dẫn đến`1`vì hỗn hợp có thể chấp nhận không chính xác`1 + 0`hoặc một sự phân tách không hợp lệ khác. Đối với đầu vào`5`, chỉ cần tìm hai số có tổng bằng`5`cũng không đủ, bởi vì`2 + 3`gồm toàn các số nguyên tố. Sự khác biệt giữa nguyên tố và hỗn hợp phải được kiểm tra chính xác. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể thử mọi ứng cử viên chính có thể`x`từ`2`bởi vì`n - 2`, bộ`y = n - x`, và kiểm tra xem`x`là số nguyên tố và`y`là tổng hợp. Phương pháp này đúng vì mọi câu trả lời hợp lệ đều phải xuất hiện trong số những ứng viên đó. Nếu tính nguyên tố được kiểm tra bằng phép chia thử thì việc kiểm tra một số có thể mất`O(sqrt(n))`hoạt động, do đó việc quét tất cả các ứng cử viên có độ phức tạp trong trường hợp xấu nhất`O(n sqrt(n))`cho một trường hợp thử nghiệm. Với`n`lớn như`10^9`, điều đó có nghĩa là gần như`10^9`giá trị ứng cử viên và về`31623`ước số thử cho mỗi ứng cử viên, theo thứ tự`3 * 10^13`kiểm tra tính chia hết trong trường hợp xấu nhất. Lặp đi lặp lại công việc như vậy cho đến`10^5`trường hợp hoàn toàn không thể thực hiện được. 

Cách tiếp cận vũ phu có hiệu quả vì câu trả lời, khi nó tồn tại, phải nằm ở đâu đó trong số các phần tách có thể có, nhưng bài toán cho chúng ta cấu trúc mạnh hơn nhiều so với bài toán tìm kiếm tùy ý. Thực ra chúng ta không cần tìm số nguyên tố gần`n`, vì các số nguyên tố cố định`2`Và`3`tương tác hoàn hảo với tính chẵn lẻ. 

Thậm chí`n`, trừ số nguyên tố`2`để lại số chẵn. Một khi số dư đó ít nhất là`4`, nó sẽ tự động được tổng hợp. Đối với một điều kỳ lạ`n`, trừ số nguyên tố`3`cũng để lại số chẵn và đối với`n >= 7`số dư đó ít nhất là`4`. Do đó, việc tìm kiếm sẽ chỉ còn một phép kiểm tra chẵn lẻ và một phép trừ. 

Thuật toán kết quả chỉ thực hiện công việc không đổi cho mỗi trường hợp thử nghiệm. Không cần kiểm tra tính nguyên tố, sàng lọc, phân tích nhân tử hoặc tìm kiếm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n sqrt(n))`mỗi trường hợp với kiểm tra tính nguyên tố phân chia thử nghiệm |`O(1)`| Quá chậm | 
| Tối ưu |`O(1)`mỗi trường hợp |`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị`n`. Giá trị bên dưới`6`không thể biểu diễn dưới dạng số nguyên tố cộng với hợp số, vì vậy hãy in ngay`-1`vì`n < 6`. 
2. Nếu`n`chẵn, in`2`Và`n - 2`. số`2`là số nguyên tố, trong khi`n - 2`là chẵn và ít nhất`4`, vậy số thứ hai là hợp số. 
3. Nếu`n`thật kỳ lạ, hãy in`3`Và`n - 3`. số`3`là số nguyên tố, trong khi`n - 3`là chẵn và ít nhất`4`, vậy số thứ hai là hợp số. 

Công trình luôn cho ra các số dương nhỏ hơn`n`. Vì`n >= 6`, số nguyên tố được chọn nhiều nhất là`3`, và phần hợp thành là`n - 2`hoặc`n - 3`, vì vậy cả hai đều nằm giữa`0`Và`n`. 

### Tại sao nó hoạt động 

Bất biến đằng sau cách xây dựng là số thứ hai luôn là số nguyên chẵn lớn hơn`2`. Mọi số nguyên chẵn lớn hơn`2`là tổng hợp. Thậm chí`n`, trừ số nguyên tố`2`đưa ra một con số như vậy bởi vì`n >= 6`ngụ ý`n - 2 >= 4`. Đối với số lẻ`n`, trừ số nguyên tố`3`đưa ra một con số như vậy bởi vì`n >= 7`ngụ ý`n - 3 >= 4`. Các giá trị duy nhất còn lại là`1`bởi vì`5`và không có số nào có thể chứa cả số nguyên tố và số nguyên dương tổng hợp, do đó trả về`-1`đối với họ là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        if n < 6:
            print(-1)
        elif n % 2 == 0:
            print(2, n - 2)
        else:
            print(3, n - 3)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý mọi giá trị nhỏ không thể thực hiện được. Ngưỡng là`6`còn hơn là`4`bởi vì`n = 4`Và`n = 5`vẫn không thể chứa cả số nguyên tố và số tổng hợp. 

Thậm chí`n`, mã chọn`2`. Kết quả`n - 2`là chẵn và ít nhất`4`, do đó không cần phải kiểm tra xem nó có phải là hợp số một cách rõ ràng hay không. 

Đối với một điều kỳ lạ`n`, mã chọn`3`. Kết quả`n - 3`một lần nữa lại chẵn và ít nhất`4`. sử dụng`3`còn hơn là`2`là cần thiết vì số lẻ trừ`2`là số lẻ và số lẻ không tự động là hợp số. 

Không có vấn đề tràn số nguyên trong Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, giá trị lớn nhất được in ở đây là bên dưới`10^9`, do đó, số nguyên có dấu 32 bit tiêu chuẩn cũng đủ. 

Thứ tự của các điều kiện quan trọng về mặt khái niệm. Trước tiên, chúng tôi loại bỏ các giá trị nhỏ không thể thực hiện được, sau đó áp dụng cấu trúc chẵn lẻ. Nếu không có`n < 6`kiểm tra, các giá trị như`n = 5`sẽ nhập sai nhánh lẻ và tạo ra`3 2`, Ở đâu`2`là số nguyên tố chứ không phải là hợp số. 

## Ví dụ đã hoạt động 

Mẫu có thể được xem như ba trường hợp thử nghiệm,`n = 1`,`n = 6`, Và`n = 7`. 

|`n`|`n < 6`| Chẵn lẻ | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | đúng | chưa đạt |`-1`| 
| 6 | sai | thậm chí |`2 4`| 
| 7 | sai | lẻ |`3 4`| 

Vì`n = 1`, điều kiện giá trị nhỏ ngay lập tức xác định rằng không tồn tại sự phân tách hợp lệ. Vì`n = 6`, số đó là số chẵn nên`2 + 4`được xây dựng, với`2`số nguyên tố và`4`tổng hợp. Vì`n = 7`, số đó là số lẻ nên`3 + 4`được xây dựng. Những dấu vết này bao gồm vùng bất khả thi, cấu trúc chẵn và cấu trúc lẻ. 

Một ví dụ thậm chí lớn hơn,`n = 10`, tuân theo quy luật tương tự. 

|`n`|`n < 6`| Chẵn lẻ | Xuất sắc`x`| tổng hợp`y`| Kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 10 | sai | thậm chí | 2 | 8 |`2 + 8 = 10`| 

Phần còn lại`8`chẵn và lớn hơn`2`, do đó nó tự động được tổng hợp. Không cần phân tích nhân tử. 

Một ví dụ kỳ lạ lớn hơn,`n = 11`, chứng minh tại sao trường hợp lẻ sử dụng`3`. 

|`n`|`n < 6`| Chẵn lẻ | Xuất sắc`x`| tổng hợp`y`| Kiểm tra | 
| --- | --- | --- | --- | --- | --- | 
| 11 | sai | lẻ | 3 | 8 |`3 + 8 = 11`| 

Lựa chọn`2`ở đây sẽ rời đi`9`, điều này xảy ra là hợp số, nhưng điều đó không được đảm bảo cho mọi số lẻ`n`. Lựa chọn`3`luôn để lại một số chẵn, tạo nên một cấu trúc đồng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(T)`| Mỗi trường hợp thử nghiệm yêu cầu một lần so sánh và nhiều nhất là một lần kiểm tra tính chẵn lẻ. | 
| Không gian |`O(1)`phụ trợ | Thuật toán chỉ lưu trữ giá trị đầu vào hiện tại và không sử dụng cấu trúc dữ liệu phụ thuộc vào`n`hoặc`T`. | 

Với nhiều nhất`10^5`trường hợp thử nghiệm, thuật toán chỉ thực hiện một số phép toán nguyên không đổi cho mỗi trường hợp. Mặc dù`n`có thể đạt được`10^9`, độ lớn của nó không làm tăng số lượng tính toán. Do đó, giải pháp phù hợp thoải mái trong các ràng buộc đã nêu. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())

    for _ in range(t):
        n = int(input())

        if n < 6:
            print(-1)
        elif n % 2 == 0:
            print(2, n - 2)
        else:
            print(3, n - 3)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run("""3
1
6
7
""") == """-1
2 4
3 4
""", "provided sample"

# Minimum-size input
assert run("""1
1
""") == """-1
""", "minimum n"

# Boundary between impossible and possible values
assert run("""4
4
5
6
7
""") == """-1
-1
2 4
3 4
""", "boundary values"

# Repeated equal values
assert run("""4
6
6
6
6
""") == """2 4
2 4
2 4
2 4
""", "repeated values"

# Maximum-size input
assert run("""1
1000000000
""") == """2 999999998
""", "maximum n"

# Large odd value
assert run("""1
999999999
""") == """3 999999996
""", "large odd n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`-1`| Đầu vào nhỏ nhất có thể | 
|`4, 5, 6, 7`|`-1, -1, 2 4, 3 4`| Chuyển đổi chính xác từ không thể thành có thể | 
|`6, 6, 6, 6`| Bốn bản sao của`2 4`| Các trường hợp thử nghiệm giống hệt nhau lặp đi lặp lại | 
|`1000000000`|`2 999999998`| Tối đa`n`và thậm chí cả xây dựng | 
|`999999999`|`3 999999996`| số lẻ lớn`n`và xây dựng kỳ lạ | 

Bộ thử nghiệm có chủ ý kiểm tra`n = 5`Và`n = 6`với nhau vì đây là lỗi ranh giới rất có thể xảy ra. Một triển khai sử dụng`n <= 5`đối với phạm vi không thể là chính xác, trong khi một phạm vi sử dụng`n < 5`sẽ chấp nhận sai`5`. 

## Vỏ cạnh 

cho`n = 1`, thuật toán đi vào`n < 6`chi nhánh và bản in`-1`. Không có sẵn số tổng hợp dương nào cả, vì vậy điều này nhất thiết là không thể. 

Vì`n = 4`, thuật toán cũng in`-1`. Cách duy nhất để sử dụng số nguyên tố làm triệu hồi đầu tiên là`2 + 2`, nhưng thứ hai`2`là số nguyên tố, không phải hợp số. Sự khác biệt quan trọng vì chỉ tìm thấy hai lệnh triệu tập tích cực hợp lệ là không đủ. 

Vì`n = 5`, kết quả lại là`-1`. Các phân rã có thể có liên quan đến một số nguyên tố là`2 + 3`Và`3 + 2`, và cả hai mệnh đề thứ hai đều là số nguyên tố. Điều này bắt được những triển khai mà quên mất điều đó`2`Và`3`không phải là hợp chất. 

Vì`n = 6`, thuật toán lấy trường hợp chẵn hợp lệ đầu tiên và kết quả đầu ra`2 4`. Đây`2`là số nguyên tố và`4`là hợp số và tổng chính xác là`6`. Đây là số nguyên đầu tiên tồn tại sự phân tích giả định. 

Vì`n = 7`, thuật toán sử dụng cấu trúc lẻ và kết quả đầu ra`3 4`. giá trị`4`là hợp số nhỏ nhất, đây là phép thử đầu tiên của nhánh lẻ. 

Để có giá trị lớn nhất`n = 10^9`, đầu ra của thuật toán`2 999999998`. Giá trị thứ hai chẵn và lớn hơn`2`, vậy nó là hợp số. Kích thước của`n`không ảnh hưởng đến số lượng thao tác, đó chính xác là lý do tại sao cấu trúc thời gian không đổi xử lý giới hạn trên một cách thoải mái.
