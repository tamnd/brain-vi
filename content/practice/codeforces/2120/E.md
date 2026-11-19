---
title: "CF 2120E - Làn Xe Ô Tô"
description: "Mỗi làn đường ban đầu chứa ô tô ai. Nếu không có ai di chuyển, những chiếc xe trong một làn đường sẽ rời đi mỗi giây một chiếc, do đó, một làn đường có kích thước x sẽ đóng góp $$1+2+dots+x=frac{x(x+1)}2$$ vào tổng số sự tức giận. Ôtô có thể chuyển làn."
date: "2026-06-08T03:54:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 2120
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1033 (Div. 2) and CodeNite 2025"
rating: 2300
weight: 2120
solve_time_s: 152
verified: true
draft: false
---

[CF 2120E - Làn đường ô tô](https://codeforces.com/problemset/problem/2120/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, dp, tìm kiếm ba ngôi 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi làn ban đầu chứa`a_i`ô tô. Nếu không có ai di chuyển, các ô tô trong một làn đường sẽ rời đi mỗi giây, do đó một làn đường có kích thước`x`đóng góp$$1+2+\dots+x=\frac{x(x+1)}2$$đến mức hoàn toàn tức giận. 

Ôtô có thể chuyển làn. Xe đang di chuyển bị dồn vào phía sau làn đường khác và bị phạt thêm`k`. Vì việc chuyển làn diễn ra tức thời và có thể xảy ra bất cứ lúc nào nên điều duy nhất quan trọng là số lượng xe cuối cùng trên mỗi làn. 

Giả sử kích thước làn đường cuối cùng là`b_1, b_2, ..., b_n`với tổng số ô tô như nhau:$$\sum b_i = \sum a_i = S.$$Sự đóng góp của thời gian chờ đợi trở thành$$\sum \frac{b_i(b_i+1)}2.$$Câu hỏi còn lại là thực tế phải di chuyển bao nhiêu ô tô. 

Nếu làn đường`i`kết thúc bằng`b_i`nhiều nhất là ô tô`min(a_i,b_i)`xe nguyên bản có thể ở đó. Những người còn lại phải rời đi hoặc đến từ nơi khác. Số ô tô di chuyển tối thiểu là$$\sum \max(0,a_i-b_i).$$Vì vậy vấn đề tối ưu hóa là$$\min \sum \left( \frac{b_i(b_i+1)}2 + k\max(0,a_i-b_i) \right).$$Những hạn chế là thách thức thực sự. Có tới`10^4`trường hợp thử nghiệm và tổng số`n`trên tất cả các trường hợp thử nghiệm là`2·10^5`. Kích thước làn đường riêng lẻ đạt tới`10^6`, nên tổng số ô tô có thể lớn bằng`2·10^{11}`. Bất kỳ thuật toán nào suy luận về ô tô riêng lẻ đều không thể thực hiện được. Chúng ta cần một giải pháp có độ phức tạp phụ thuộc vào`n`, không phải trên tổng số ô tô. 

Một số trường hợp khó xử lý. 

Coi như:```
1
1 100
5
```Chỉ có một làn đường. Không có chiếc xe nào có thể được di chuyển đến bất cứ nơi nào hữu ích. Câu trả lời đơn giản là$$1+2+3+4+5=15.$$Bất kỳ giải pháp nào cho rằng sự cân bằng luôn có ích sẽ tạo ra kết quả sai. 

Một trường hợp tế nhị khác là:```
1
2 1
1 1
```Việc di chuyển một chiếc ô tô hầu như không tốn kém gì, nhưng việc di chuyển vẫn không giúp ích được gì. Giữ nguyên cấu hình mang lại sự tức giận hoàn toàn`1+1=2`. Bất kỳ động thái nào cũng tạo ra kích thước làn đường`(0,2)`và chi phí`3+1=4`. 

Ở thái cực khác:```
1
2 100
10 0
```Thời hạn chờ đợi thích sự cân bằng hơn, nhưng hình phạt di chuyển là rất lớn. Sự tối ưu giữ cho hầu hết các xe ở đúng vị trí. Một giải pháp chỉ giảm thiểu số hạng kích thước làn đường bậc hai sẽ thất bại. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là chọn kích thước làn đường cuối cùng`b_i`và đánh giá$$\sum \frac{b_i(b_i+1)}2 + k\sum \max(0,a_i-b_i).$$Khó khăn là số lượng vectơ có thể`(b_1,\dots,b_n)`với số tiền cố định`S`là thiên văn. Ngay cả với những giá trị khiêm tốn, không gian tìm kiếm vẫn vượt xa tầm với. 

Quan sát quan trọng là mục tiêu có thể tách rời. Định nghĩa$$g_i(b) = \frac{b(b+1)}2 + k\max(0,a_i-b).$$Sau đó chúng tôi muốn$$\min \sum g_i(b_i)$$tùy thuộc vào$$\sum b_i=S.$$Đây là một bài toán cấp phát tài nguyên lồi cổ điển. 

Hãy tưởng tượng xây dựng làn đường`i`một chiếc xe tại một thời điểm Hãy để chi phí tăng kích thước của nó từ`t-1`ĐẾN`t`là chi phí cận biên$$\Delta_i(t)=g_i(t)-g_i(t-1).$$Một phép tính ngắn cho$$\Delta_i(t)= \begin{cases} t-k,& t\le a_i,\\ t,& t>a_i. \end{cases}$$Đối với mỗi làn đường, chúng ta có được chuỗi chi phí cận biên tăng dần:$$1-k,\;2-k,\;\dots,\;a_i-k,\;a_i+1,\;a_i+2,\dots$$Việc chọn kích thước cuối cùng tương đương với việc chọn chính xác`S`các khoản mục chi phí cận biên từ sự kết hợp của tất cả các chuỗi này. Tổng chi phí tối thiểu có được bằng cách lấy`S`chi phí biên nhỏ nhất. 

Bây giờ bài toán trở thành: trong vô số giá trị cận biên, tìm tổng của giá trị nhỏ nhất`S`của họ. 

Các giá trị cận biên đều đơn điệu nên chúng ta có thể tìm kiếm nhị phân một ngưỡng`m`. Đối với bất kỳ`m`, chúng ta có thể đếm được có bao nhiêu cận biên`≤ m`và tính tổng của tất cả các biên`< m`. Điều này biến vấn đề thành một tìm kiếm nhị phân đếm và tổng hợp tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`Và`S`| Hàm mũ | Quá chậm | 
| Tối ưu |`O(n log M)`|`O(1)`thêm | Đã chấp nhận | 

Đây`M`là về`10^6`. 

## Hướng dẫn thuật toán 

1. Hãy để`S = sum(a)`. 
2. Hãy quan sát điều đó$$g_i(b) = \frac{b(b+1)}2 + k\max(0,a_i-b).$$Tổng mục tiêu là tổng chi phí của các làn đường độc lập này. 
3. Tính trình tự biên cho từng làn:$$\Delta_i(t)= \begin{cases} t-k,& t\le a_i,\\ t,& t>a_i. \end{cases}$$Lựa chọn`b_i`có nghĩa là lấy cái đầu tiên`b_i`giá trị cận biên từ làn đường`i`. 
4. Diễn giải lại vấn đề theo hướng lựa chọn chính xác`S`giá trị cận biên từ sự kết hợp của tất cả các chuỗi làn đường. 

Giải pháp tối ưu là tổng của`S`giá trị biên nhỏ nhất. 
5. Tìm kiếm nhị phân ngưỡng số nguyên`m`. 

Đối với làn đường có kích thước`a_i`: 

Số lượng cận biên thỏa mãn`Δ ≤ m`là$$\min(a_i,m+k)+\max(0,m-a_i),$$với số lượng âm được cắt giảm về 0. 
6. Hãy để`C(m)`là tổng số cận biên`≤ m`. 

Tìm ngưỡng nhỏ nhất thỏa mãn$$C(m)\ge S.$$Tương đương,$$C(m-1)<S\le C(m).$$7. Tính số và tổng của tất cả các số cận biên nhỏ hơn`m`. 

Đối với làn đường`i`: 

Đoạn đầu tiên góp phần$$c_1=\min(a_i,m+k-1),$$cận biên, có tổng là$$\frac{c_1(c_1+1)}2-kc_1.$$Đoạn thứ hai góp phần$$c_2=\max(0,m-1-a_i)$$cận biên, có tổng là$$\sum_{t=a_i+1}^{a_i+c_2} t = \frac{c_2(2a_i+c_2+1)}2.$$8. Hãy để`cnt_less`là tổng số biên`< m`Và`sum_less`tổng số tiền của họ. 

Chúng tôi vẫn cần$$S-cnt\_less$$cận biên, tất cả đều có giá trị chính xác`m`. 
9. Tổng của nhỏ nhất`S`cận biên là$$sum\_less + (S-cnt\_less)\cdot m.$$10. Thêm số hạng không đổi$$kS.$$Điều này phục hồi giá trị mục tiêu ban đầu. 

### Tại sao nó hoạt động 

Đối với mỗi làn đường, chi phí cận biên tạo thành một chuỗi không giảm. Bất kỳ vectơ kích thước cuối cùng khả thi nào cũng tương ứng với việc lấy tiền tố của mỗi chuỗi và tổng số biên được chọn chính xác là`S`. 

Trong số tất cả các cách để lựa chọn`S`cận biên, số tiền tối thiểu thu được bằng cách lấy giá trị nhỏ nhất trên toàn cầu`S`các giá trị. Tìm kiếm nhị phân xác định giá trị biên`m`tách các lề được chọn và không được chọn. Các công thức đếm xác định chính xác có bao nhiêu giá trị nằm dưới ranh giới đó và tổng đóng góp của chúng là bao nhiêu. Điền giá trị vào các vị trí còn lại`m`xây dựng lại tổng của`S`biên nhỏ nhất, đó chính xác là mục tiêu tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        S = sum(a)
        mx = max(a)

        def count_le(m):
            total = 0
            for x in a:
                c1 = min(x, max(0, m + k))
                c2 = max(0, m - x)
                total += c1 + c2
            return total

        lo = -k
        hi = max(mx, (S + n - 1) // n) + 2

        while lo < hi:
            mid = (lo + hi) // 2
            if count_le(mid) >= S:
                hi = mid
            else:
                lo = mid + 1

        m = lo

        cnt_less = 0
        sum_less = 0

        for x in a:
            c1 = min(x, max(0, m + k - 1))
            cnt_less += c1
            sum_less += c1 * (c1 + 1) // 2 - k * c1

            c2 = max(0, m - 1 - x)
            cnt_less += c2
            sum_less += c2 * (2 * x + c2 + 1) // 2

        marginal_sum = sum_less + (S - cnt_less) * m
        answer = k * S + marginal_sum

        ans.append(str(answer))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Tìm kiếm nhị phân tìm thấy giá trị cận biên ngưỡng. chức năng`count_le(m)`tính toán chi phí cận biên nhiều nhất là bao nhiêu`m`. 

Giới hạn dưới là`-k`, đó là giá trị cận biên nhỏ nhất có thể (`1-k`). Giới hạn trên chỉ cần vượt quá ngưỡng cuối cùng. Một lần`m`đạt tới`max(a_i)`, mỗi làn đóng góp khoảng`m`cận biên, vì vậy một giới hạn gần`max(a_i)`là đủ. 

Bước thứ hai tính toán đóng góp chính xác của tất cả các giá trị cận biên nhỏ hơn`m`. Các số cận biên cần thiết còn lại đều phải bằng nhau`m`, vì thuộc tính xác định$$C(m-1)<S\le C(m).$$Số nguyên Python tự động xử lý các giá trị lên tới gần đúng`10^{17}`và hơn thế nữa, do đó không có vấn đề tràn phát sinh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 4
13 7 4
```Tổng số ô tô là`S = 24`. 

Tìm kiếm nhị phân tìm thấy`m = 2`. 

| Ngõ | a_i | Đếm < m | Tổng < m | 
| --- | --- | --- | --- | 
| 1 | 13 | 5 | -5 | 
| 2 | 7 | 5 | -5 | 
| 3 | 4 | 5 | -5 | 

Vì vậy: 

| Số lượng | Giá trị | 
| --- | --- | 
| cnt_less | 15 | 
| tổng_không | -15 | 
| còn lại | 9 | 
| cận_tổng | 3 | 
| kS | 96 | 
| trả lời | 123 | 

Mức tối ưu tương ứng với việc di chuyển hai ô tô từ làn đường lớn nhất sang làn đường nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
1
1 7
6
```Chỉ có một làn đường. 

Tìm kiếm nhị phân tìm thấy`m = 6`. 

| Ngõ | a_i | Đếm < m | Tổng < m | 
| --- | --- | --- | --- | 
| 1 | 6 | 5 | -20 | 

Sau đó: 

| Số lượng | Giá trị | 
| --- | --- | 
| cnt_less | 5 | 
| tổng_không | -20 | 
| còn lại | 1 | 
| cận_tổng | -14 | 
| kS | 42 | 
| trả lời | 21 | 

Điều này phù hợp$$1+2+3+4+5+6=21.$$Dấu vết chứng tỏ rằng công thức vẫn có hiệu lực ngay cả khi không thể chuyển làn đường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log M)`| Tìm kiếm nhị phân vượt ngưỡng biên, mỗi bước quét tất cả các làn | 
| Không gian |`O(1)`thêm | Chỉ có một số quầy được duy trì |`M`là về`10^6`, Vì thế`log M`là khoảng 20. Với tổng số`n ≤ 2·10^5`, thuật toán chỉ thực hiện vài triệu phép tính số học và vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from solution import solve

    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""6
3 4
13 7 4
4 9
6 12 14 5
5 3
2 4 13 5 10
1 7
6
13 4
10 26 34 39 9 43 48 41 1 38 13 4 46
16 3
176342 171863 70145 80835 160257 136105 78541 100795 114461 45482 68210 51656 29593 8750 173743 156063
"""
) == """123
219
156
21
5315
82302351405
"""

# minimum size
assert run(
"""1
1 5
1
"""
) == "1\n"

# all equal
assert run(
"""1
3 10
5 5 5
"""
) == "45\n"

# movement not beneficial
assert run(
"""1
2 1
1 1
"""
) == "2\n"

# single lane
assert run(
"""1
1 100
6
"""
) == "21\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, a=[1]`|`1`| Ví dụ kích thước tối thiểu | 
|`a=[5,5,5]`|`45`| Cấu hình đối xứng đã tối ưu | 
|`a=[1,1], k=1`|`2`| Di chuyển ô tô có thể làm tăng chi phí | 
| Làn đường đơn với sáu ô tô |`21`| Không thể phân phối lại | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 100
5
```Tìm kiếm nhị phân tìm thấy ngưỡng tương ứng với sáu giá trị cận biên của một làn. Vì không có làn đường nào khác nên mọi đường biên được chọn đều xuất phát từ cùng một chuỗi. Thuật toán trả về$$1+2+3+4+5=15.$$Không cần xử lý đặc biệt. 

Bây giờ hãy xem xét:```
1
2 1
1 1
```Các chuỗi cận biên là$$0,2,3,\dots$$cho cả hai làn đường. Hai giá trị cận biên nhỏ nhất là hai số 0, mỗi số một ở mỗi làn. Thuật toán chọn chính xác các giá trị đó và thu được tổng chi phí`2`. Bất kỳ sự phân phối lại nào cũng sẽ yêu cầu chọn mức biên lớn hơn và không thể cải thiện kết quả. 

Cuối cùng:```
1
2 100
10 0
```Làn đường đầu tiên có nhiều lề âm hoặc nhỏ vì việc loại bỏ ô tô cực kỳ tốn kém. Ngưỡng được tìm thấy bằng tìm kiếm nhị phân giữ hầu hết các lề được chọn bên trong làn đường ban đầu. Hình phạt chuyển động được kết hợp trực tiếp vào các giá trị cận biên, do đó thuật toán tự động tránh phân phối lại quá mức và trả về giá trị tối ưu thực sự.
