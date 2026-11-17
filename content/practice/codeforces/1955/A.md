---
title: "CF 1955A - Bán Sữa Chua"
description: "Maxim muốn mua chính xác n hộp sữa chua từ một cửa hàng nơi mỗi hộp sữa chua có giá một bao, nhưng có một chương trình khuyến mãi cung cấp hai hộp sữa chua với giá b bao. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tính số tiền tối thiểu anh ta có thể chi để mua đúng n sữa chua."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1955
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 938 (Div. 3)"
rating: 800
weight: 1955
solve_time_s: 54
verified: true
draft: false
---

[CF 1955A - Bán sữa chua](https://codeforces.com/problemset/problem/1955/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Maxim muốn mua chính xác`n`sữa chua từ một cửa hàng nơi một loại sữa chua có giá`a`burles, nhưng có một chương trình khuyến mãi cung cấp hai loại sữa chua cho`b`burles. Với mỗi test case, chúng ta phải tính toán chính xác số tiền tối thiểu anh ta có thể bỏ ra để mua`n`sữa chua. Đầu vào chỉ định nhiều trường hợp thử nghiệm, mỗi trường hợp cung cấp số lượng sữa chua`n`, giá đơn thông thường`a`, và giá khuyến mãi mua 2 tặng 1`b`. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên duy nhất biểu thị tổng chi phí tối thiểu. 

Các hạn chế là nhỏ:`n`lên tới 100 và giá lên tới 30. Với tối đa`10^4`các trường hợp kiểm thử, chúng ta cần mỗi trường hợp kiểm thử được xử lý trong thời gian không đổi. Điều này loại trừ các thuật toán lặp đi lặp lại`n`cho mỗi trường hợp thử nghiệm nhiều lần hoặc thực hiện các tìm kiếm phức tạp. 

Các trường hợp đặc biệt bao gồm các tình huống mua riêng lẻ hai hộp sữa chua sẽ rẻ hơn so với khuyến mãi. Ví dụ, nếu`a = 5`Và`b = 11`, thì việc mua riêng hai loại sữa chua với giá 10 chiếc sẽ tốt hơn so với việc sử dụng khuyến mãi cho 11 chiếc. Một trường hợp tế nhị khác là khi`n`là kỳ quặc; chúng ta có thể mua càng nhiều cặp càng tốt và cần xử lý riêng sữa chua cuối cùng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ thử mọi sự kết hợp có thể có của các cặp đơn lẻ và cặp quảng cáo có tổng giá trị`n`sữa chua và tính tổng chi phí cho mỗi sự kết hợp. Điều này hiệu quả vì chúng ta có thể lặp từ 0 đến`n//2`cặp, và với mỗi số cặp, hãy tính tổng chi phí bằng cách cộng số sữa chua đơn lẻ còn lại. Để tối đa`n = 100`, chúng tôi sẽ thực hiện tối đa 50 lần lặp cho mỗi trường hợp thử nghiệm. Với`10^4`trường hợp thử nghiệm, điều này dẫn đến 500.000 lần lặp trong trường hợp xấu nhất, điều này khả thi nhưng không cần thiết. 

Cái nhìn sâu sắc quan trọng là nhận thấy rằng vấn đề được rút gọn thành một so sánh đơn giản: đối với mỗi cặp sữa chua, mua hai hộp với giá khuyến mại`b`chỉ có giá trị nếu`b`ít hơn`2 * a`. Nếu không, chúng ta nên mua sữa chua riêng lẻ. Quan sát này cho phép chúng ta tính chi phí tối thiểu bằng một công thức duy nhất: tính số cặp (`n // 2`) và các đĩa đơn còn sót lại (`n % 2`) và đối với các cặp, hãy sử dụng giá trị rẻ hơn của`b`Và`2 * a`. Những người độc thân luôn`a`mỗi. Điều này làm giảm giải pháp số học đơn giản cho mỗi trường hợp thử nghiệm, điều này là tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Có thể chấp nhận được nhưng không cần thiết | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Chúng tôi sẽ xử lý từng cái một cách độc lập. 
2. Với mỗi test case, hãy đọc`n`,`a`,`b`. Chúng đại diện cho tổng số sữa chua cần mua, giá sữa chua đơn và giá khuyến mãi cho hai chiếc. 
3. Tính số cặp sữa chua chúng ta có thể mua được`n // 2`và số lượng sữa chua đơn còn lại là`n % 2`. 
4. Đối với các cặp, hãy tính chi phí bằng cách sử dụng giá trị nhỏ hơn của`b`Và`2 * a`, bởi vì mua một cặp không bao giờ được vượt quá chi phí mua hai cái riêng lẻ. 
5. Nhân số cặp với giá cặp và số sữa chua đơn còn lại với`a`và tính tổng chúng để có được tổng chi phí. 
6. In kết quả cho test case. 

Tại sao nó hoạt động: Điều bất biến là đối với hai loại sữa chua bất kỳ, chúng tôi luôn chọn tùy chọn rẻ hơn giữa khuyến mãi và mua hai lần. Vì mỗi loại sữa chua đều phải được tính toán nên những sữa chua đơn lẻ còn sót lại sẽ được xử lý riêng. Không có sự kết hợp thay thế nào có thể tạo ra tổng chi phí nhỏ hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, a, b = map(int, input().split())
    pairs = n // 2
    singles = n % 2
    cost = pairs * min(2 * a, b) + singles * a
    print(cost)
```Mã đọc số lượng trường hợp kiểm thử và xử lý từng trường hợp kiểm thử trong thời gian không đổi. Đối với mỗi trường hợp thử nghiệm, nó tính toán số lượng cặp và số lượng sữa chua đơn lẻ rồi sử dụng`min`chức năng để chọn tùy chọn rẻ hơn cho các cặp. Cách tiếp cận này tránh được từng lỗi một bằng cách xử lý các đĩa đơn còn sót lại bằng cách sử dụng`n % 2`. Việc tính toán đơn giản và tránh các vòng lặp không cần thiết. 

## Ví dụ đã hoạt động 

Dấu vết của mẫu đầu vào 1: 

đầu vào:`2 5 9`Biến:`n = 2`,`a = 5`,`b = 9`Cặp:`2 // 2 = 1`Đơn:`2 % 2 = 0`Giá cặp:`min(2*5, 9) = 9`Tổng chi phí:`1*9 + 0*5 = 9`đầu vào:`3 5 11`Biến:`n = 3`,`a = 5`,`b = 11`Cặp:`3 // 2 = 1`Đơn:`3 % 2 = 1`Giá cặp:`min(2*5, 11) = 10`Tổng chi phí:`1*10 + 1*5 = 15`Những dấu vết này cho thấy thuật toán chọn chính xác tùy chọn rẻ hơn cho các cặp và xử lý các đĩa đơn còn sót lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm được xử lý trong thời gian không đổi bằng cách sử dụng so sánh số học và so sánh tối thiểu | 
| Không gian | O(1) | Chỉ một vài biến cho mỗi trường hợp thử nghiệm được sử dụng, không có cấu trúc dữ liệu bổ sung | 

Với những hạn chế, phương pháp này dễ dàng phù hợp với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        pairs = n // 2
        singles = n % 2
        cost = pairs * min(2 * a, b) + singles * a
        print(cost)
    return output.getvalue().strip()

# provided samples
assert run("4\n2 5 9\n3 5 9\n3 5 11\n4 5 11\n") == "9\n14\n15\n20", "sample 1"

# custom cases
assert run("3\n1 3 5\n2 2 4\n5 5 9\n") == "3\n4\n23", "custom cases"
assert run("2\n10 1 1\n7 2 5\n") == "10\n14", "mix of cheap and expensive promotion"
assert run("1\n1 30 1\n") == "30", "single yogurt only"
assert run("1\n100 30 60\n") == "3000", "large n, b equal to 2*a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 5`|`3`| Chỉ mua 1 sữa chua, bỏ qua khuyến mãi | 
|`2 2 4`|`4`| Khuyến mãi bằng hai đơn, thuật toán xử lý bằng nhau | 
|`5 5 9`|`23`| Mix đôi và lẻ còn dư, khuyến mãi rẻ hơn 2*a | 
|`10 1 1`|`10`| Khuyến mại rẻ, đúng kịch bản 2*a = b | 
|`1 30 1`|`30`| tối thiểu`n`trường hợp cạnh | 
|`100 30 60`|`3000`| Tối đa`n`, b chính xác là 2*a | 

## Vỏ cạnh 

cho`n = 1`và bất kỳ`b`, thuật toán tính toán chính xác`singles = 1`và in`a`, bỏ qua chương trình khuyến mãi. Đối với số lẻ`n`giống`n = 3`,`pairs = 1`Và`singles = 1`đảm bảo một sữa chua được mua riêng lẻ. Khi khuyến mại`b`đắt hơn hai lần mua một lần,`min(2*a, b)`chọn cái rẻ hơn`2*a`. Khi`b`bằng`2*a`, nó vẫn chọn`2*a`, điều đó đúng. Những tình huống này xác nhận tính đúng đắn của các kịch bản giá khuyến mãi có kích thước tối thiểu, lẻ và giới hạn.
