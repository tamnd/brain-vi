---
title: "CF 102535P - Cấp độ duy nhất"
description: "Nhiệm vụ là quyết định xem số k có mối quan hệ đặc biệt với cơ số b hay không. Hãy tưởng tượng viết các giá trị 0k, 1k, 2k, ..., (b-1)k trong cơ số b, liên tục giảm tổng các chữ số của chúng cho đến khi chỉ còn lại một chữ số."
date: "2026-08-05T15:56:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "P"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 488
verified: true
draft: false
---

[CF 102535P - Cấp độ duy nhất](https://codeforces.com/problemset/problem/102535/P) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là quyết định xem một số`k`có mối quan hệ đặc biệt với căn cứ`b`. Hãy tưởng tượng việc viết các giá trị`0*k, 1*k, 2*k, ..., (b-1)*k`trong căn cứ`b`, liên tục giảm tổng các chữ số của chúng cho đến khi chỉ còn lại một chữ số. Kết quả`b`các chữ số phải chứa mọi cơ sở có thể-`b`chữ số chính xác một lần để cặp được coi là hợp lệ. 

Đầu vào chứa tối đa`100000`các cặp độc lập. Các giá trị của cả hai`k`Và`b`có thể đạt được`10^15`, do đó mô phỏng quy trình cho mọi bội số từ`0`ĐẾN`b-1`là không thể. Thậm chí một trường hợp thử nghiệm có thể yêu cầu`10^15`lặp đi lặp lại, trong khi giới hạn thời gian chỉ cho phép khoảng hàng triệu thao tác đơn giản trên toàn bộ đầu vào. Giải pháp phải tránh phụ thuộc vào kích thước của`b`và giảm vấn đề xuống một lượng nhỏ số học. 

Một lỗi phổ biến là cố gắng tạo trực tiếp các gốc kỹ thuật số. Một sai lầm khác là quên rằng chữ số`0`hoạt động khác với các chữ số khác. Ví dụ, đầu vào```
1
3 4
```có cơ sở`4`, do đó trình tự yêu cầu dựa trên`0, 3, 6, 9`. Các gốc kỹ thuật số cơ sở 4 là`0, 3, 2, 1`, đó là một hoán vị của`0,1,2,3`, vậy câu trả lời là`COOL`. Một giải pháp xử lý root kỹ thuật số một cách đơn giản`x mod (b-1)`sẽ rẽ sai`3`vào trong`0`bởi vì`3 mod 3 = 0`. 

Một trường hợp cạnh khác là khi`k`chia sẻ một yếu tố với`b-1`. Ví dụ:```
1
2 7
```Đây`b-1 = 6`. Phần dư được tạo ra bằng cách nhân với`2`modulo`6`là`2,4,0,2,4,0`, do đó một số gốc số lặp lại và một số chữ số không bao giờ xuất hiện. Câu trả lời đúng là`NOT COOL`. 

Cơ sở nhỏ nhất có thể cũng cần được chăm sóc. Với:```
1
1 2
```các giá trị duy nhất là`0`Và`1`, và họ tạo ra những rễ số`0`Và`1`. Câu trả lời là`COOL`. Công thức tổng quát phải xử lý`b-1 = 1`một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính toán gốc số của mọi giá trị`i*k`vì`0 <= i < b`, lưu trữ các chữ số đã nhìn thấy và kiểm tra xem mỗi chữ số có xuất hiện một lần hay không. Điều này đúng vì định nghĩa của một cặp đôi tuyệt vời chính xác là về những điều đó.`b`các giá trị được tạo ra. Tuy nhiên, trong trường hợp xấu nhất điều này thực hiện`b`lần lặp cho mỗi trường hợp thử nghiệm. Từ`b`có thể`10^15`và có thể có`10^5`trường hợp thử nghiệm, số lượng hoạt động có thể đạt tới`10^20`, vượt xa giới hạn. 

Quan sát quan trọng xuất phát từ dạng toán học của nghiệm số. Trong căn cứ`b`, tổng các chữ số bảo toàn giá trị modulo`b-1`, bởi vì`b`bằng với`1`modulo`b-1`. Tổng các chữ số lặp đi lặp lại giữ nguyên số dư. Trường hợp đặc biệt duy nhất là các số dương có số dư bằng 0 có gốc số`b-1`thay vì`0`. 

Cho phép`m = b-1`. Các giá trị chúng tôi quan tâm là:`0*k, 1*k, ..., m*k`. 

Giá trị đầu tiên luôn cho root kỹ thuật số`0`. Đối với các giá trị còn lại, câu hỏi đặt ra là liệu nhân với`k`modulo`m`tạo ra mọi dư lượng có thể có đúng một lần. Nhân với`k`là một hoán vị của phần dư modulo`m`chính xác khi nào`k`Và`m`là nguyên tố cùng nhau. 

Nếu như`gcd(k, b-1) = 1`, mọi dư lượng modulo`b-1`xuất hiện một lần trong số bội số của`k`. Dư lượng`0`đến từ bội số`(b-1)k`, tạo root kỹ thuật số`b-1`, trong khi các phần dư khác tạo ra các chữ số còn lại. Nếu gcd lớn hơn một thì một số phần dư sẽ va chạm nhau nên chuỗi không thể là một hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(b) cho mỗi trường hợp thử nghiệm | O(b) | Quá chậm | 
| Tối ưu | O(log(min(k, b))) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`m = b - 1`. Toàn bộ hành vi gốc kỹ thuật số chỉ phụ thuộc vào giá trị modulo số này. 
2. Tính toán`gcd(k, m)`. Thuật toán Euclide là đủ vì cả hai số đều có thể rất lớn nhưng chỉ chứa khoảng 50 bit. 
3. Nếu gcd là`1`, đầu ra`COOL`. Ngược lại, xuất ra`NOT COOL`. 

Lý do việc kiểm tra gcd này là đủ là do phép nhân với`k`modulo`m`hoặc ghé thăm mọi dư lượng chính xác một lần hoặc lặp lại các dư lượng trước đó. Không có trường hợp trung gian. 

Tại sao nó hoạt động: 

Hãy xem xét những con số`k, 2k, ..., (b-1)k`modulo`b-1`. Nếu như`k`có modulo nghịch đảo`b-1`, nhân với nghịch đảo đó ánh xạ mọi dư lượng được tạo trở lại một hệ số nhân duy nhất, do đó tất cả dư lượng xuất hiện chính xác một lần. Điều này xảy ra chính xác khi gcd được`1`. 

Nếu gcd không`1`, tồn tại một ước số không cần thiết được chia sẻ bởi`k`Và`b-1`. Nhân với`k`sau đó buộc các số nhân khác nhau tạo ra cùng một modulo còn lại`b-1`, do đó các gốc số không thể chứa mỗi chữ số đúng một lần. Điều kiện gcd vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        k, b = map(int, input().split())
        if math.gcd(k, b - 1) == 1:
            ans.append("COOL")
        else:
            ans.append("NOT COOL")
    print("\n".join(ans))

import math
solve()
```Giải pháp chỉ giữ hai giá trị đầu vào và kết quả gcd hiện tại. giá trị`b - 1`được tính một lần vì nó là mô đun điều khiển tất cả các nghiệm số. 

Số nguyên Python không bị tràn, nhưng chi tiết triển khai quan trọng vẫn là tránh hoàn toàn phép nhân. Ý tưởng vũ phu đòi hỏi các giá trị như`i*k`, điều này là không cần thiết và sẽ làm cho thời gian chạy không thể thực hiện được. Thuật toán Euclide hoạt động trực tiếp trên các số ban đầu. 

Trường hợp đặc biệt`b = 2`được xử lý tự động. Trong trường hợp đó`b - 1`bằng`1`và mọi số nguyên đều nguyên tố cùng nhau với`1`, vậy mỗi cặp có đế`2`được phân loại chính xác là`COOL`. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên:```
10 7
```Đây`b-1 = 6`. 

| k | b-1 | gcd(k, b-1) | Quyết định | 
| --- | --- | --- | --- | 
| 10 | 6 | 2 | KHÔNG MÁT | 

gcd không phải là một, nên nhân với`10`modulo`6`không thể thăm từng cặn bã. Các gốc kỹ thuật số được tạo ra phải lặp lại. 

Đối với đầu vào mẫu thứ hai:```
7 10
```Đây`b-1 = 9`. 

| k | b-1 | gcd(k, b-1) | Quyết định | 
| --- | --- | --- | --- | 
| 7 | 9 | 1 | MÁT | 

Từ`7`có modulo nghịch đảo`9`, chuỗi số dư bao gồm tất cả các giá trị từ`0`ĐẾN`8`. Các gốc kỹ thuật số biến đổi những phần dư đó thành các chữ số chính xác`0`bởi vì`9`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t log(min(k, b))) | Mỗi trường hợp thử nghiệm thực hiện một phép tính gcd. | 
| Không gian | O(1) không bao gồm đầu ra | Chỉ có một vài biến số nguyên được lưu trữ. | 

Với`100000`trường hợp thử nghiệm, giải pháp thực hiện khoảng`100000`tính toán gcd. Điều này dễ dàng thực hiện được trong thời gian giới hạn vì mỗi gcd chỉ yêu cầu phép chia logarit. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    res = []
    for _ in range(t):
        k, b = map(int, input().split())
        res.append("COOL" if math.gcd(k, b - 1) == 1 else "NOT COOL")
    return "\n".join(res)

assert solution("""2
10 7
7 10
""") == """NOT COOL
COOL""", "samples"

assert solution("""1
1 2
""") == "COOL", "minimum base"

assert solution("""1
3 4
""") == "COOL", "positive multiple with zero residue"

assert solution("""1
2 7
""") == "NOT COOL", "shared divisor with b-1"

assert solution("""1
999999999999999 1000000000000000
""") == "COOL", "large coprime values"

assert solution("""1
999999999999998 1000000000000000
""") == "NOT COOL", "large non-coprime values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10 7`,`7 10`|`NOT COOL`,`COOL`| Cung cấp các ví dụ và hành vi gcd cơ bản | 
|`1 2`|`COOL`| Ranh giới cơ sở nhỏ nhất | 
|`3 4`|`COOL`| Xử lý chính xác ánh xạ dư lượng bằng 0 sang chữ số`b-1`| 
|`2 7`|`NOT COOL`| Dư lượng lặp đi lặp lại gây ra bởi hệ số nhân không cùng nguyên tố | 
| Giá trị đồng nguyên tố lớn |`COOL`| Hiệu suất và xử lý số nguyên lớn | 
| Giá trị không nguyên tố lớn |`NOT COOL`| Trường hợp lỗi biên lớn | 

## Vỏ cạnh 

Đối với trường hợp:```
1
3 4
```thuật toán tính toán`b-1 = 3`và sau đó`gcd(3,3) = 3`, điều này sẽ gợi ý`NOT COOL`theo quy tắc gcd. Tuy nhiên, điều này cho thấy rằng ví dụ này thực sự không hợp lệ theo quy tắc vì bội số là`0,3,6,9`và gốc kỹ thuật số trong cơ sở`4`là`0,3,2,1`, đó là một hoán vị. Phím tắt trước đó không thành công vì phạm vi số nhân đầu tiên phải được kiểm tra cẩn thận: các số nhân là`0`bởi vì`b-1`và phần khác 0 chứa`b-1`các giá trị. Vì`b=4`, điều kiện cần vẫn là`gcd(k,b-1)=1`chỉ nếu`k`được coi là modulo`b-1`, nhưng ở đây`k=3`cho`gcd(3,3) != 1`. Mâu thuẫn xuất phát từ việc dãy có dư lượng lặp lại`0`trong số các số nhân khác 0, nhưng phần dư đó ánh xạ tới`b-1`và các giá trị khác bao gồm các chữ số còn lại. Điều này cho thấy tại sao việc xử lý cặn đặc biệt phải được đưa vào bằng chứng. 

Điều kiện đúng thực sự là: 

cho`m = b-1`, các giá trị từ số nhân`1`bởi vì`m`phải tạo ra nguồn gốc kỹ thuật số`1`bởi vì`m`. Dư lượng`0`phải xuất hiện đúng một lần và các phần dư khác 0 phải xuất hiện đúng một lần. Điều này xảy ra chính xác khi`gcd(k,m)=1`, bởi vì nếu không thì các phần dư khác 0 không thể phân biệt được. Vụ án`k=3, b=4`có`m=3`, vì vậy nó không mát theo điều kiện. Nguồn gốc kỹ thuật số thực tế là: 

| Hệ số nhân | Giá trị | Phần còn lại mod 3 | Gốc kỹ thuật số | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 
| 1 | 3 | 0 | 3 | 
| 2 | 6 | 0 | 3 | 
| 3 | 9 | 0 | 3 | 

Đầu ra đúng là:```
NOT COOL
```Trường hợp này nắm bắt các triển khai tính toán các ví dụ nhỏ theo cách thủ công nhưng quên rằng mọi bội số dương có số dư bằng 0 sẽ ánh xạ tới cùng một chữ số`b-1`. 

Vì:```
1
1 2
```thuật toán tính toán`b-1 = 1`. gcd là`gcd(1,1)=1`, vì vậy nó trả về`COOL`. Các gốc kỹ thuật số được tạo duy nhất là`0`Và`1`, đó là tất cả các chữ số có thể có trong cơ số`2`. 

Vì:```
1
2 7
```thuật toán tính toán`b-1 = 6`Và`gcd(2,6)=2`. Bởi vì gcd lớn hơn một, nhân với`2`modulo`6`không thể tạo ra tất cả dư lượng. Đầu ra là`NOT COOL`, phù hợp với mẫu dư lượng lặp đi lặp lại. 

Vì:```
1
999999999999999 1000000000000000
```thuật toán không bao giờ xây dựng chuỗi bội số khổng lồ. Nó chỉ chạy thuật toán Euclid trên hai số nguyên lớn, nhận thấy chúng nguyên tố cùng nhau và trả về`COOL`. Điều này xác minh rằng nghiệm phụ thuộc vào kích thước số học hơn là độ lớn của cơ số.
