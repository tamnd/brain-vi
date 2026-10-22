---
title: "CF 105862B - Công bằng và Quảng trường"
description: "Bài toán yêu cầu chúng ta lấy một lưới hình chữ nhật có chiều cao n và chiều rộng m, tạo một vài bản sao giống hệt nhau và đính kèm các bản sao đó mà không xoay chúng. Hình dạng kết hợp cuối cùng phải là hình vuông. Chúng tôi cần số lượng bản sao nhỏ nhất cần thiết để đạt được điều đó."
date: "2026-06-25T14:33:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105862
codeforces_index: "B"
codeforces_contest_name: "ACPC Kickoff 2025"
rating: 0
weight: 105862
solve_time_s: 37
verified: true
draft: false
---

[CF 105862B - Công bằng và Bình đẳng](https://codeforces.com/problemset/problem/105862/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán yêu cầu ta lấy một lưới hình chữ nhật có chiều cao`n`và chiều rộng`m`, tạo một số bản sao giống hệt nhau và đính kèm các bản sao đó mà không xoay chúng. Hình dạng kết hợp cuối cùng phải là hình vuông. Chúng tôi cần số lượng bản sao nhỏ nhất cần thiết để đạt được điều đó. Vấn đề ban đầu là từ Codeforces và có tới`10^5`trường hợp thử nghiệm với kích thước lớn như`10^9`. 

Đầu vào mô tả nhiều hình chữ nhật độc lập. Đối với mỗi hình chữ nhật, đầu ra là số lượng tối thiểu của những hình chữ nhật này có thể được xếp thành hình vuông lớn hơn trong khi vẫn giữ nguyên hướng của mỗi hình chữ nhật. Các bản sao được sắp xếp theo dạng lưới, vì vậy nếu chúng ta đặt`a`sao chép theo chiều dọc và`b`sao chép theo chiều ngang, kích thước cuối cùng trở thành`(a * n) × (b * m)`. Để đây là một hình vuông, hai giá trị đó phải bằng nhau. 

Giới hạn lớn ngay lập tức loại trừ mô phỏng. Độ dài các cạnh có thể là một tỷ, do đó, việc thử các kích thước hình vuông có thể có hoặc liên tục thêm các hình chữ nhật là không thể. Với`10^5`các trường hợp thử nghiệm, chúng ta cần một cách tiếp cận chỉ thực hiện một lượng nhỏ số học không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh chính là khi hình chữ nhật đã là hình vuông. Đối với đầu vào`1 1`, câu trả lời là`1`. Một cách tiếp cận bất cẩn khi tìm kiếm sự sắp xếp lớn hơn có thể trả về một số lớn hơn một cách không chính xác, nhưng việc sử dụng một bản sao đã thỏa mãn điều kiện. 

Một trường hợp cạnh khác là khi một bên chia cho bên kia. Đối với đầu vào:```
1
2 4
```đầu ra đúng là:```
2
```Hai bản sao của một`2 × 4`hình chữ nhật có thể được đặt cạnh nhau để tạo thành một`2 × 8`hình chữ nhật nhưng không phải là hình vuông. Thay vào đó, việc đặt chúng chồng lên nhau sẽ mang lại một`4 × 4`quảng trường. Cách tiếp cận chỉ kiểm tra các vị trí nằm ngang sẽ thất bại. 

Một trường hợp tinh tế hơn là khi các kích thước không liên quan. Đối với đầu vào:```
1
2 3
```câu trả lời là:```
6
```Hình vuông nhỏ nhất có độ dài cạnh`6`. Chúng ta cần ba bản sao theo một hướng và hai bản sao theo hướng khác, cho`6`hình chữ nhật. Giải pháp chỉ tính tỷ lệ diện tích sẽ bỏ lỡ yêu cầu ốp lát thực tế. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử số lượng bản sao có thể có. Giả sử chúng ta đặt`x`sao chép theo chiều dọc và`y`sao chép theo chiều ngang. Hình chữ nhật cuối cùng có kích thước`x * n`Và`y * m`, nên ta cần tìm số nhỏ nhất`x * y`như vậy:```
x * n = y * m
```Ý tưởng vũ phu sẽ thử các giá trị của`x`Và`y`cho đến khi đẳng thức giữ nguyên. Điều này đúng vì mọi sự sắp xếp có thể đều được biểu diễn bằng một cặp nào đó`(x, y)`. Tuy nhiên, với kích thước lên tới`10^9`, thậm chí việc lặp lại một phần nhỏ các giá trị có thể là quá chậm. Qua nhiều trường hợp thử nghiệm, số lượng hoạt động sẽ bùng nổ. 

Quan sát quan trọng là chúng ta thực sự không quan tâm đến sự sắp xếp mà chỉ quan tâm đến chiều dài cạnh hình vuông nhỏ nhất mà cả hai kích thước hình chữ nhật có thể chia được. Độ dài cạnh hình vuông cuối cùng phải là bội số chung của`n`Và`m`. Cạnh nhỏ nhất có thể là bội số chung nhỏ nhất của chúng. 

Nếu hình vuông cuối cùng có độ dài cạnh`L`, thì số lượng bản sao dọc theo chiều cao là`L / n`và dọc theo chiều rộng là`L / m`. Tổng số hình chữ nhật là:```
(L / n) * (L / m)
```Nhỏ nhất có thể`L`, ta chọn:```
L = lcm(n, m)
```Sử dụng mối quan hệ:```
lcm(n, m) = n / gcd(n, m) * m
```số bản sao trở thành:```
(n / gcd(n, m)) * (m / gcd(n, m))
```mà đơn giản hóa thành:```
n * m / gcd(n, m)^2
```Điều này đưa ra câu trả lời trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(cạnh hình vuông tối thiểu) | O(1) | Quá chậm | 
| Tối ưu | O(log(min(n, m))) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước hình chữ nhật`n`Và`m`. 

Chúng ta chỉ cần hai giá trị này vì mọi sự sắp xếp cuối cùng có thể xảy ra đều phụ thuộc vào số lượng bản sao được đặt theo mỗi hướng. 
2. Tính toán`g = gcd(n, m)`. 

Ước chung lớn nhất cho chúng ta biết hai chiều đã chia sẻ bao nhiêu. Việc loại bỏ yếu tố chia sẻ này sẽ để lại những phần không tương thích nhỏ nhất quyết định số lượng bản sao cần thiết. 
3. Tính số bản sao trên hai trục. 

Số bản sao theo chiều dọc là`m / g`, và số theo chiều ngang là`n / g`. Nhân chúng sẽ có tổng số hình chữ nhật. 

Lý do điều này có hiệu quả là vì cạnh hình vuông nhỏ nhất là bội số chung nhỏ nhất. Hai trục phải chia tỷ lệ cho đến khi các kích thước gặp nhau ở giá trị đó. 
4. Đầu ra:```
(n / g) * (m / g)
```Đây là số lượng tối thiểu có thể. 

Tại sao nó hoạt động: mọi cách xây dựng hợp lệ đều tạo ra một hình vuông có độ dài cạnh là bội số chung của`n`Và`m`. Độ dài nhỏ nhất như vậy là bội số chung nhỏ nhất, do đó, bất kỳ cách xây dựng hình vuông nào khác đều phải sử dụng ít nhất nhiều bản sao. Công thức sử dụng chiều dài cạnh nhỏ nhất đó, nghĩa là nó đạt được mức tối thiểu. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        g = gcd(n, m)
        ans.append(str((n // g) * (m // g)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mã đọc tất cả các trường hợp thử nghiệm và xử lý từng hình chữ nhật một cách độc lập. các`gcd`là đủ vì toàn bộ bài toán quy về việc tìm mối quan hệ bội chung nhỏ nhất. 

Biểu thức được viết là:```
(n // g) * (m // g)
```thay vì:```
n * m // (g * g)
```bởi vì phép chia đầu tiên giữ cho các giá trị trung gian nhỏ hơn. Số nguyên Python không bị tràn, nhưng dạng này cũng phản ánh lý luận toán học rõ ràng hơn. 

Sai lầm duy nhất có thể xảy ra là sử dụng sai ước số chung lớn nhất. Phần được chia sẻ phải được loại bỏ khỏi cả hai chiều trước khi nhân lên, bởi vì phần được chia sẻ đó thể hiện sự tiến bộ theo cả chiều dọc và chiều ngang. 

## Ví dụ đã hoạt động 

Đối với hình chữ nhật`2 × 3`: 

| Bước | n | m | gcd | bản sao | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 3 | 1 | 6 | 
| Cuối cùng | 2 | 3 | 1 | 2 × 3 | 

gcd là`1`, nghĩa là không có yếu tố chung. Cạnh hình vuông nhỏ nhất là`6`, yêu cầu ba bản sao theo một hướng và hai bản sao theo hướng khác, tổng cộng là`6`. 

Đối với hình chữ nhật`6 × 8`: 

| Bước | n | m | gcd | bản sao | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 6 | 8 | 2 | 6 | 
| Cuối cùng | 6 | 8 | 2 | 3 × 4 | 

Yếu tố chung`2`làm giảm vấn đề xuống`3 × 4`. Cạnh hình vuông nhỏ nhất là`24`, vì vậy chúng ta cần`4`hình chữ nhật theo một hướng và`3`trong cái khác. 

Những dấu vết này cho thấy tính bất biến: sau khi chia cả hai vế cho gcd, các thừa số còn lại biểu thị tỷ lệ chính xác cần thiết để đạt được kích thước hình vuông chung đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log(min(n, m))) | Thuật toán Euclid tính gcd hiệu quả | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giải pháp thực hiện một phép tính gcd cho mỗi trường hợp thử nghiệm. Vì số lượng ca kiểm thử có thể lớn nên cần có giải pháp số học theo thời gian không đổi cho mỗi trường hợp và điều này dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solution(data):
    input = io.StringIO(data).readline
    t = int(input())
    res = []
    for _ in range(t):
        n, m = map(int, input().split())
        g = gcd(n, m)
        res.append(str((n // g) * (m // g)))
    return "\n".join(res)

def run(inp: str) -> str:
    return solution(inp)

assert run("""2
2 3
1 1
""") == """6
1""", "samples"

assert run("""1
2 4
""") == "2", "dividing side"

assert run("""1
999999999 1000000000
""") == "999999999000000000", "large values"

assert run("""3
5 5
10 20
7 11
""") == """1
2
77""", "basic cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`|`6`| Kích thước đồng nguyên | 
|`2 4`|`2`| Một chiều chia rẽ chiều kia | 
|`999999999 1000000000`|`999999999000000000`| Giới hạn rất lớn | 
| Các cạnh bằng nhau như`5 5`|`1`| Đã là hình vuông | 

## Vỏ cạnh 

Đối với trường hợp đã vuông:```
1
5 5
```gcd là`5`. Công thức trở thành:```
(5 / 5) * (5 / 5) = 1
```Chỉ cần một hình chữ nhật là đúng. 

Đối với một bên chia được:```
1
3 12
```gcd là`3`. Câu trả lời là:```
(3 / 3) * (12 / 3) = 4
```Cạnh hình vuông cuối cùng là`12`, được hình thành bằng cách đặt bốn`3 × 12`hình chữ nhật xếp chồng lên nhau theo chiều dọc. 

Đối với các bên nguyên tố cùng nhau:```
1
4 7
```gcd là`1`, vậy đáp án là:```
4 * 7 = 28
```Cạnh hình vuông phải là`28`, yêu cầu bảy bản sao dọc theo một trục và bốn bản sao dọc theo trục kia. Vì không tồn tại bội số chung nhỏ hơn nên không thể sắp xếp nhỏ hơn được.
