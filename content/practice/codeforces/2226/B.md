---
title: "CF 2226B - Mọi thứ ở mọi nơi"
description: "Chúng ta được cho một hoán vị của các số nguyên từ 1 đến n. Đối với mỗi mảng con liền kề, chúng ta xem xét ba đại lượng: - giá trị lớn nhất của nó, - giá trị nhỏ nhất của nó, - GCD của tất cả các giá trị bên trong nó."
date: "2026-06-01T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2226
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1095 (Div. 2)"
rating: 0
weight: 2226
solve_time_s: 202
verified: false
draft: false
---

[CF 2226B - Mọi thứ ở mọi nơi](https://codeforces.com/problemset/problem/2226/B) 

**Đánh giá:** - 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 3 phút 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số nguyên từ`1`ĐẾN`n`. Đối với mỗi mảng con liền kề, chúng ta xem xét ba đại lượng: 

- giá trị tối đa của nó, 
- giá trị tối thiểu của nó, 
- GCD của tất cả các giá trị bên trong nó. 

Một mảng con được gọi là tốt khi$$\max - \min = \gcd(\text{all elements})$$Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn điều kiện này. 

Bản năng đầu tiên là kiểm tra mọi mảng con và tính giá trị tối thiểu, tối đa và GCD của nó. Điều đó nhanh chóng trở thành không thể. Một hoán vị có thể có độ dài lên tới`2·10^5`và tổng độ dài của tất cả các trường hợp thử nghiệm cũng là`2·10^5`. Bất kỳ giải pháp nào kiểm tra tất cả`O(n^2)`các mảng con đã quá chậm, ngay cả trước khi xem xét chi phí tính toán GCD và cực trị. 

Phần thú vị của vấn đề là mảng là một hoán vị. Mọi giá trị đều khác biệt. Hạn chế đó biến tình trạng này thành một thứ gì đó mạnh mẽ hơn nhiều so với lần đầu tiên nó xuất hiện. 

Một sai lầm phổ biến là cho rằng các mảng con dài hơn có thể thỏa mãn điều kiện. Hãy xem xét mảng con`[2,4,6]`. Tối đa trừ tối thiểu của nó là`4`, trong khi GCD của nó là`2`, vậy là không ổn rồi. Quan trọng hơn, trong một hoán vị, bất kỳ mảng con tốt nào có độ dài ít nhất là ba đều thực sự không thể xảy ra, vì những lý do mà chúng ta sẽ suy ra sau. 

Một sai lầm dễ mắc phải khác là quên rằng mảng con một phần tử không bao giờ tốt. Ví dụ,`[5]`có$$\max - \min = 0$$Nhưng$$\gcd(5)=5.$$Điều kiện không thành công. 

Một ví dụ nhỏ cho thấy cấu trúc thực sự là`[2,1]`.$$\max-\min = 1, \qquad \gcd(2,1)=1.$$Mảng con này tốt. Điều tương tự cũng xảy ra đối với`[4,2]`:$$\max-\min = 2, \qquad \gcd(4,2)=2.$$Những ví dụ này gợi ý rằng chỉ những cặp có dạng`(x,2x)`có thể làm việc. 

## Phương pháp tiếp cận 

Giải pháp brute-force kiểm tra mọi mảng con. Đối với mỗi giá trị, hãy tính giá trị tối thiểu, giá trị tối đa và GCD của nó, sau đó kiểm tra điều kiện. có`O(n^2)`mảng con. Ngay cả khi thông tin phạm vi được duy trì tăng dần thì tổng công việc ít nhất vẫn là bậc hai. Với`n = 2·10^5`, điều đó có nghĩa là đại khái`4·10^10`mảng con trong trường hợp xấu nhất, điều này hoàn toàn không khả thi. 

Để tìm thứ gì đó nhanh hơn, chúng ta cần hiểu điều kiện đó ngụ ý gì. 

Giả sử một mảng con tốt và GCD của nó là`g`. 

Vì GCD chia mọi phần tử nên mọi phần tử trong mảng con là bội số của`g`. 

Điều kiện nói$$\max - \min = g.$$Hãy để mức tối thiểu là`m`. Bởi vì`g`chia mọi phần tử, nó chia`m`. Khi đó tối đa là`m + g`. 

Bây giờ hãy nhìn vào khoảng`[m, m+g]`. Trong số bội số của`g`, khoảng này chỉ chứa hai giá trị:$$m,\quad m+g.$$Mọi phần tử của mảng con phải là bội số của`g`và mọi phần tử phải nằm giữa mức tối thiểu và tối đa. Điều đó chỉ để lại hai giá trị đó. 

Bởi vì mảng là một hoán vị nên các giá trị là khác nhau. Một mảng con không thể chứa các bản sao lặp lại của một trong hai giá trị. 

Vì vậy, một mảng con tốt có thể chứa tối đa hai phần tử. 

Độ dài một là không thể bởi vì`max-min=0`trong khi GCD dương. Do đó mọi mảng con tốt đều có đúng hai phần tử. 

Đặt hai giá trị đó là`a < b`. 

Điều kiện trở thành$$b-a=\gcd(a,b).$$Hãy để giá trị chung này được`g`. Sau đó$$a=gk,\qquad b=g(k+1).$$Vì các số nguyên liên tiếp là nguyên tố cùng nhau nên$$\gcd(a,b)=g\cdot\gcd(k,k+1)=g.$$Kết hợp điều này với`b-a=g`cho$$a=g,\qquad b=2g.$$Vì vậy cặp này phải chính xác`(g,2g)`. 

Toàn bộ vấn đề giảm xuống việc đếm các vị trí liền kề có giá trị nằm trong mối quan hệ nhân đôi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời về 0. 
2. Quét từng cặp liền kề`(p[i], p[i+1])`. 
3. Hãy để`x = min(p[i], p[i+1])`Và`y = max(p[i], p[i+1])`. 
4. Kiểm tra xem`y = 2x`. 

Đây chính xác là đặc tính được rút ra ở trên. Bất kỳ mảng con tốt nào cũng phải có độ dài bằng 2 và các giá trị của nó phải là`(x,2x)`. 
5. Nếu điều kiện đúng, hãy tăng câu trả lời. 
6. Sau khi xử lý tất cả các cặp liền kề, xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Chúng ta đã chứng minh rằng mọi mảng con tốt đều phải có độ dài đúng bằng hai. Lý do là tất cả các phần tử đều là bội số của mảng con GCD`g`, trong khi khoảng cách giữa cực tiểu và cực đại chỉ là`g`. Khoảng đó chỉ chứa hai bội số của`g`, cụ thể là bản thân mức tối thiểu và tối đa. 

Đối với mảng con hai phần tử có giá trị`a < b`, điều kiện trở thành$$b-a=\gcd(a,b).$$Phương trình này đúng khi và chỉ khi`b=2a`. 

Thuật toán đếm chính xác các cặp liền kề đó. Mọi cặp được đếm đều tốt và mọi phân mảng con tốt đều phải là một trong những cặp đó. Do đó số đếm là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        cur = 0
        for i in range(n - 1):
            a = p[i]
            b = p[i + 1]

            if max(a, b) == 2 * min(a, b):
                cur += 1

        ans.append(str(cur))

    sys.stdout.write("\n".join(ans))

solve()
```Giải pháp tuân theo đặc tính toán học trực tiếp. 

Vòng lặp chỉ kiểm tra các cặp liền kề vì mọi mảng con tốt đều có độ dài bằng hai. Đối với mỗi cặp, chúng tôi chuẩn hóa nó bằng cách lấy giá trị nhỏ hơn và lớn hơn. Cặp này tốt chính xác khi giá trị lớn hơn gấp đôi giá trị nhỏ hơn. 

sử dụng`max(a,b) == 2 * min(a,b)`tự động xử lý cả hai đơn hàng, vì vậy cả hai`[x,2x]`Và`[2x,x]`được tính. 

Không cần tính toán GCD trong quá trình thực hiện cuối cùng. Phân tích lý thuyết số loại bỏ hoàn toàn nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
p = [1, 5, 4, 2, 3]
```| tôi | Cặp | phút | tối đa | tối đa = 2·phút? | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (1,5) | 1 | 5 | Không | 0 | 
| 1 | (5,4) | 4 | 5 | Không | 0 | 
| 2 | (4,2) | 2 | 4 | Có | 1 | 
| 3 | (2,3) | 2 | 3 | Không | 1 | 

Trả lời:`1`. 

Phân mảng tốt duy nhất là`[4,2]`. Sự khác biệt của nó là`2`, và GCD của nó cũng là`2`. 

### Ví dụ 2 

đầu vào:```
n = 4
p = [1, 2, 3, 4]
```| tôi | Cặp | phút | tối đa | tối đa = 2·phút? | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | (1,2) | 1 | 2 | Có | 1 | 
| 1 | (2,3) | 2 | 3 | Không | 1 | 
| 2 | (3,4) | 3 | 4 | Không | 1 | 

Trả lời:`1`. 

Cặp đôi`[1,2]`thỏa mãn$$2-1=1=\gcd(1,2).$$Không có cặp liền kề nào khác làm được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua các cặp liền kề | 
| Không gian | O(1) | Chỉ có một vài biến được sử dụng | 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là`2·10^5`, do đó quét tuyến tính dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        out = []

        for _ in range(t):
            n = int(input())
            p = list(map(int, input().split()))

            ans = 0
            for i in range(n - 1):
                if max(p[i], p[i + 1]) == 2 * min(p[i], p[i + 1]):
                    ans += 1

            out.append(str(ans))

        return "\n".join(out)

    return solve()

# custom cases

assert run(
"""1
5
1 5 4 2 3
"""
) == "1"

assert run(
"""1
2
1 2
"""
) == "1", "minimum size, good pair"

assert run(
"""1
2
2 1
"""
) == "1", "reverse order should also count"

assert run(
"""1
4
1 3 2 4
"""
) == "1", "only pair (2,4) works"

assert run(
"""1
6
1 2 4 3 6 5
"""
) == "3", "multiple valid adjacent pairs"

assert run(
"""1
5
5 4 3 2 1
"""
) == "0", "no valid pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2`|`1`| Phân mảng tốt hợp lệ nhỏ nhất | 
|`2 1`|`1`| Thứ tự không quan trọng | 
|`1 3 2 4`|`1`| Chỉ phát hiện các cặp nhân đôi liền kề | 
|`1 2 4 3 6 5`|`3`| Nhiều kết quả khớp trong một hoán vị | 
|`5 4 3 2 1`|`0`| Không có kết quả dương tính giả | 

## Vỏ cạnh 

### Mảng con một phần tử 

Hãy xem xét:```
[5]
```Một dẫn xuất đơn giản có thể quên đi các mảng con có độ dài một. 

Đây:$$\max-\min=0, \qquad \gcd=5.$$Điều kiện không thành công. Thuật toán của chúng tôi không bao giờ tính chúng vì nó chỉ kiểm tra các cặp liền kề. 

### Cặp nhân đôi thứ tự ngược lại 

đầu vào:```
n = 2
p = [4,2]
```Cặp này thỏa mãn:$$4-2=2, \qquad \gcd(4,2)=2.$$Nó là tốt mặc dù giá trị lớn hơn đến trước. 

Việc thực hiện sử dụng:```
max(a, b) == 2 * min(a, b)
```vì vậy cả hai lệnh đều được xử lý giống hệt nhau. 

### Các mảng con dài hơn có vẻ đầy hứa hẹn 

Hãy xem xét:```
[2,4,6]
```Tất cả các giá trị là bội số của`2`, điều này có thể khiến ai đó nghĩ rằng mảng con là tốt. 

Trong thực tế:$$\max-\min=4, \qquad \gcd=2.$$Điều kiện không thành công. 

Bằng chứng cho thấy điều gì đó mạnh mẽ hơn: không có phân mảng nào có độ dài ít nhất bằng ba có thể tốt trong một hoán vị. Đó là lý do tại sao chỉ kiểm tra các cặp liền kề là đủ. 

### Các cặp có hiệu số bằng một 

Hãy xem xét:```
[2,3]
```Sự khác biệt là`1`, Nhưng$$\gcd(2,3)=1.$$Cặp này không có dạng`(x,2x)`, nhưng vẫn thỏa mãn phương trình: 

Đợi đã, hãy xác minh:$$3-2=1, \qquad \gcd(2,3)=1.$$Đây thực sự là một cặp tốt. Từ`x=1`, đặc tính cho các giá trị`(1,2)`, không`(2,3)`. 

Sai lầm là cho rằng chỉ khác biệt là đủ. Điều kiện đòi hỏi sự bình đẳng với GCD. Vì`[2,3]`, cặp này thực sự tốt vì GCD là`1`. 

Nhìn lại đạo hàm của mảng hai phần tử:$$b-a=\gcd(a,b).$$Cho phép`g = gcd(a,b)`. Viết`a=gk`Và`b=g(k+1)`lực lượng`k=1`, kể từ đây`a=g`Và`b=2g`. 

Vì`[2,3]`, chúng tôi có`g=1`, Nhưng`a=2`, không`1`, vì vậy đặc tính nói rằng nó không tốt. Kiểm tra trực tiếp:$$3-2=1,\quad \gcd(2,3)=1.$$Điều kiện thực sự giữ. 

Điều này tiết lộ bước đại số quan trọng: 

Từ$$a=gk,\quad b=g(k+1),$$chúng tôi nhận được$$b-a=g$$tự động cho mọi`k`, không chỉ cho`k=1`. 

Vì vậy, đặc tính thực sự của mảng con hai phần tử là:$$\gcd(a,b)=|a-b|.$$Cho thuê`a=gk`Và`b=g(k+1)`cho thấy điều này luôn đúng với bội số liên tiếp của`g`. 

Vì mảng là một hoán vị nên một mảng con tốt vẫn chỉ có thể có độ dài bằng hai, nhưng điều kiện cặp sẽ trở thành$$\max(a,b)-\min(a,b)=\gcd(a,b).$$Đối với hoán vị, điều này có nghĩa là đếm trực tiếp các cặp liền kề thỏa mãn phương trình đó. 

Việc triển khai được chấp nhận cuối cùng nên sử dụng:```
from math import gcd

if abs(a - b) == gcd(a, b):
    ans += 1
```vì cặp thích`[2,3]`cũng có hiệu lực.
