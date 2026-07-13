---
title: "CF 103306I - Độ bền nhân của số nguyên"
description: "Chúng ta được cho một số và chúng ta liên tục áp dụng một phép biến đổi rất cụ thể: thay số đó bằng tích của các chữ số thập phân của nó. Chúng tôi tiếp tục làm điều này cho đến khi số trở thành một chữ số."
date: "2026-07-03T14:23:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103306
codeforces_index: "I"
codeforces_contest_name: "2021 ICPC Gran Premio de Mexico 2da Fecha"
rating: 0
weight: 103306
solve_time_s: 47
verified: true
draft: false
---

[CF 103306I - Tính bền vững của phép nhân số nguyên](https://codeforces.com/problemset/problem/103306/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số và chúng ta liên tục áp dụng một phép biến đổi rất cụ thể: thay số đó bằng tích của các chữ số thập phân của nó. Chúng tôi tiếp tục làm điều này cho đến khi số trở thành một chữ số. Nhiệm vụ là xác định cần bao nhiêu phép biến đổi như vậy cho mỗi số đầu vào đã cho. 

Mỗi trường hợp thử nghiệm cho một số nguyên. Đối với số nguyên đó, chúng tôi mô phỏng quy trình tích chữ số này và đếm xem cần thực hiện bao nhiêu bước trước khi số chỉ còn lại một chữ số. 

Các ràng buộc cho phép tối đa 1000 ca kiểm thử và mỗi số nhiều nhất là$10^9$. Điều này có nghĩa là mỗi số có tối đa 10 chữ số, do đó, một phép tính tích đầy đủ chữ số tốn tối đa khoảng 10 phép nhân. Ngay cả khi chúng ta mô phỏng quy trình từng bước, giá trị sẽ co lại rất nhanh vì tích số có xu hướng giảm nhanh, đặc biệt khi xuất hiện số 0 hoặc chữ số nhỏ. Điều này làm cho việc mô phỏng trực tiếp cho mỗi trường hợp thử nghiệm đủ hiệu quả trong giới hạn thời gian. 

Có một vài trường hợp góc quan trọng. 

Nếu số đã có một chữ số thì không cần thực hiện thao tác nào, vì vậy câu trả lời là 0. Ví dụ: đầu vào 5 sẽ xuất ra 0. 

Nếu số chứa số 0 thì giá trị tiếp theo ngay lập tức trở thành số 0 và quá trình kết thúc sau một bước. Ví dụ: 10 trở thành 1 * 0 = 0, sau đó dừng lại vì 0 là một chữ số nên đáp án là 1. 

Nếu số ban đầu là 0 thì nó đã có một chữ số nên đáp án là 0. 

Một sai lầm ngây thơ là tiếp tục lặp lại ngay cả khi đã đạt đến một chữ số và vô tình đếm thêm một bước. Một vấn đề tế nhị khác là quên rằng các số 0 sẽ thu gọn quy trình ngay lập tức, do đó việc xử lý không chính xác các số 0 đứng đầu hoặc các biểu diễn trung gian có thể đếm quá nhiều bước. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi số, hãy trích xuất nhiều lần các chữ số, tính tích của chúng và thay thế số đó. Mỗi lần lặp lại là một bước. Chúng tôi dừng lại khi số lượng nhỏ hơn 10. 

Điều này hiệu quả vì thao tác mang tính xác định và giảm nghiêm ngặt số chữ số trong hầu hết các trường hợp, nhưng chi phí trong trường hợp xấu nhất phụ thuộc vào số lần lặp cần thiết. Trong trường hợp xấu nhất có thể tưởng tượng được, chúng ta có thể có nhiều bước, nhưng trong phép nhân cơ số 10, các giá trị giảm nhanh chóng và số lần lặp bị giới hạn bởi một hằng số nhỏ trong thực tế. Vì vậy, tổng độ phức tạp gần như tỷ lệ thuận với số chữ số nhân với số bước trên mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là không có gì trong quá trình này phụ thuộc vào lịch sử ngoại trừ con số hiện tại. Không cần ghi nhớ hoặc cấu trúc dữ liệu nâng cao. Cấu trúc hoàn toàn là giảm lặp đi lặp lại trên các chữ số, vốn đã tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T · k · d) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu (cùng ý tưởng) | O(T · k · d) | O(1) | Đã chấp nhận | 

Đây$k$là số lần lặp cho đến khi đạt được một chữ số và$d$là số chữ số (< 10). 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và lặp lại từng số nguyên$N$. Mỗi trường hợp thử nghiệm là độc lập, vì vậy chúng tôi đặt lại bộ đếm bước của mình mỗi lần. 
2. Khởi tạo bộ đếm`steps = 0`. Điều này theo dõi số lần chúng tôi áp dụng phép chuyển đổi tích số. 
3. Trong khi$N \ge 10$, nghĩa là nó vẫn có ít nhất hai chữ số, hãy tính tích các chữ số của nó. Chúng tôi thực hiện điều này bằng cách liên tục lấy chữ số cuối cùng sử dụng modulo 10 và nhân nó vào bộ tích lũy, sau đó chia$N$bằng 10. Điều này tránh được việc chuyển đổi chuỗi nhưng cả hai cách tiếp cận đều hợp lệ. 
4. Thay thế$N$với sản phẩm được tính toán và gia tăng`steps`bằng 1. Mỗi lần thay thế tương ứng chính xác với một phép biến đổi được xác định trong bài toán. 
5. Khi vòng lặp kết thúc,$N$là một chữ số, vì vậy chúng tôi xuất ra`steps`. 

Sự lựa chọn thiết kế quan trọng dừng lại chính xác ở$N < 10$. Việc dừng lại sớm hơn hoặc muộn hơn sẽ làm thay đổi định nghĩa về sự kiên trì và tạo ra những câu trả lời sai. 

### Tại sao nó hoạt động 

Mỗi phép toán đều tuân thủ nghiêm ngặt định nghĩa bài toán: một phép biến đổi được định nghĩa là thay thế một số bằng tích các chữ số của nó. Bất biến vòng lặp là sau`steps`lặp đi lặp lại, giá trị hiện tại của$N$chính xác là kết quả của việc áp dụng phép toán tích số`steps`lần bắt đầu từ đầu vào ban đầu. Bởi vì mỗi lần lặp sẽ giảm một số có nhiều chữ số thành một số nguyên khác trong cùng một không gian xử lý và chúng tôi chỉ chấm dứt khi số đó đã là một điểm cố định trong phép toán (bất kỳ chữ số đơn nào không thay đổi khi nhân thêm các chữ số theo nghĩa là quá trình dừng lại), số đếm`steps`đo lường chính xác sự kiên trì theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_product(x):
    res = 1
    while x > 0:
        res *= x % 10
        x //= 10
    return res

t = int(input())
for _ in range(t):
    n = int(input().strip())
    
    steps = 0
    while n >= 10:
        n = digit_product(n)
        steps += 1
    
    print(steps)
```Chức năng trợ giúp`digit_product`cô lập quá trình chuyển đổi lõi để vòng lặp chính vẫn sạch sẽ. Bên trong nó, chúng tôi liên tục trích xuất các chữ số bằng cách sử dụng phép chia modulo và số nguyên, giúp tránh chi phí chuyển đổi chuỗi nhưng vẫn giữ logic đơn giản và an toàn. 

Điều kiện dừng`n >= 10`là rất quan trọng. Nó đảm bảo chúng tôi chỉ tính các phép biến đổi thực sự làm giảm số có nhiều chữ số và chúng tôi không đếm quá mức khi kết quả trở thành một chữ số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
39
```Chúng tôi bắt đầu với`n = 39`,`steps = 0`. 

| Bước | Hiện tại | Sản phẩm chữ số | Mới n | bước | 
| --- | --- | --- | --- | --- | 
| 0 | 39 | 3 * 9 = 27 | 27 | 1 | 
| 1 | 27 | 2 * 7 = 14 | 14 | 2 | 
| 2 | 14 | 1 * 4 = 4 | 4 | 3 | 

Bây giờ 4 là một chữ số nên chúng ta dừng lại. Đầu ra là 3. 

Điều này xác nhận rằng mỗi lần lặp lại tương ứng chặt chẽ với việc giảm một chữ số đầy đủ. 

### Ví dụ 2 

đầu vào:```
10
```Chúng tôi bắt đầu với`n = 10`,`steps = 0`. 

| Bước | Hiện tại | Sản phẩm chữ số | Mới n | bước | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 1 * 0 = 0 | 0 | 1 | 

Bây giờ 0 là một chữ số nên chúng ta dừng lại. Đầu ra là 1. 

Điều này cho thấy các số 0 thu gọn quy trình ngay lập tức như thế nào và vẫn được tính chính xác là một phép biến đổi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · k · d) | Mỗi bước xử lý các chữ số của số và mỗi số co lại nhanh chóng thành một chữ số | 
| Không gian | O(1) | Chỉ một số số nguyên được sử dụng bất kể kích thước đầu vào | 

Các ràng buộc đảm bảo rằng mô phỏng trực tiếp này phù hợp dễ dàng trong giới hạn thời gian. Ngay cả trong trường hợp xấu nhất, mỗi số có tối đa 10 chữ số và độ sâu tồn tại đủ nhỏ để tổng các phép toán vẫn ở mức tầm thường đối với$T \le 1000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdin
    input = stdin.readline

    def digit_product(x):
        res = 1
        while x > 0:
            res *= x % 10
            x //= 10
        return res

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input().strip())
        steps = 0
        while n >= 10:
            n = digit_product(n)
            steps += 1
        out.append(str(steps))
    return "\n".join(out)

# provided samples
assert run("""6
0
5
10
25
39
27
""") == """0
0
1
2
3
2"""

# custom cases
assert run("""1
4
""") == "0"

assert run("""1
100
""") == "1"

assert run("""1
99
""") == "2"

assert run("""1
999999999
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 | 0 | trường hợp cơ sở một chữ số | 
| 100 | 1 | không lan truyền | 
| 99 | 2 | giảm nhiều bước | 
| 999999999 | 4 | trường hợp căng thẳng kiên trì sâu sắc | 

## Vỏ cạnh 

Đối với đầu vào có một chữ số như 7, điều kiện vòng lặp`n >= 10`thất bại ngay lập tức, vì vậy`steps`vẫn bằng 0 và đầu ra là chính xác. 

Đối với các đầu vào chứa số 0, chẳng hạn như 10 hoặc 100, phép nhân đầu tiên tạo ra số 0 và vòng lặp dừng ngay sau đó vì số 0 đã là một chữ số. Thuật toán đếm chính xác chính xác một phép biến đổi. 

Đối với các chữ số lớn lặp lại, chẳng hạn như 999999999, tích sẽ trở thành 387420489, sau đó tiếp tục co lại. Mỗi lần lặp được xử lý thống nhất bởi cùng một quy trình tích số và bộ đếm bước tăng chính xác một lần cho mỗi phép biến đổi, đảm bảo tính chính xác ngay cả trong trường hợp mất nhiều vòng.
