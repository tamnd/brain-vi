---
title: "CF 102769A - Lời chào từ Tần Hoàng Đảo"
description: "Bài toán mô tả một tập hợp các quả bóng màu đỏ và xanh. Có r quả bóng đỏ và b quả bóng xanh, lấy ngẫu nhiên hai quả bóng giống nhau và không thay thế."
date: "2026-07-30T04:20:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "A"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 61
verified: true
draft: false
---

[CF 102769A - Lời chào từ Tần Hoàng Đảo](https://codeforces.com/problemset/problem/102769/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một tập hợp các quả bóng màu đỏ và xanh. có`r`quả bóng màu đỏ và`b`quả bóng màu xanh và hai quả bóng được chọn ngẫu nhiên như nhau, không thay thế. Chúng ta cần tính xác suất để cả hai quả bóng được chọn đều có màu đỏ, sau đó in xác suất đó dưới dạng phân số rút gọn. Định dạng đầu ra cũng yêu cầu số trường hợp trước mỗi câu trả lời. 

Tổng số cách chọn hai quả bóng trong số các quả bóng có sẵn là số cặp trong số đó.`r + b`các đối tượng, đó là`C(r+b, 2)`. Lựa chọn có lợi là những cặp chỉ chứa bi đỏ, điều này mang lại`C(r, 2)`. Xác suất cần thiết là tỷ số giữa hai đại lượng này. 

Các ràng buộc rất nhỏ: mỗi số lượng màu tối đa là 100 và có tối đa 10 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả các phép tính tổ hợp trực tiếp cũng đủ nhanh. Không cần đến các kỹ thuật xác suất nâng cao hoặc tính toán trước lớn. Thách thức chính không phải là hiệu suất mà là tránh sai sót khi giảm phân số và xử lý chính xác các giá trị đặc biệt. 

Một số trường hợp rất dễ xử lý sai. Khi chỉ có một quả bóng màu đỏ thì việc chọn hai quả bóng màu đỏ là không thể. Đối với đầu vào:```
1
1 5
```câu trả lời là`Case #1: 0/1`. Việc thực hiện bất cẩn có thể tính toán`1 * 0 / 2 = 0`đúng nhưng quên rằng mẫu số rút gọn vẫn phải dương và khác 0. 

Một trường hợp khác là khi tất cả các quả bóng đều có màu đỏ. Đối với đầu vào:```
1
5 0
```xác suất chính xác là 1 vì mọi cặp có thể có đều màu đỏ. Câu trả lời rút gọn phải là`1/1`, không phải cái gì đó như`10/10`. 

Lỗi phổ biến cuối cùng là sử dụng các lựa chọn có thứ tự thay vì các cặp không có thứ tự. Ví dụ:```
1
2 1
```Có một cặp thuận lợi, hai bi đỏ và tổng cộng ba cặp. Câu trả lời là`1/3`. Đếm các lượt rút theo thứ tự mang lại`2 / 6`, bằng nhau về mặt toán học nhưng yêu cầu giảm thích hợp trước khi xuất. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là đếm số cặp có thể có và số cặp thành công. Vì việc chọn hai đối tượng bỏ qua thứ tự nên tổng số lần lựa chọn là:$$\binom{r+b}{2}=\frac{(r+b)(r+b-1)}{2}$$và các lựa chọn thành công là:$$\binom{r}{2}=\frac{r(r-1)}{2}$$Xác suất là giá trị đầu tiên chia cho giá trị thứ hai. Phương pháp này đã tối ưu vì giá trị đầu vào rất nhỏ. Một mô phỏng lực lượng vũ phu có thể thử từng cặp bóng, kiểm tra xem cả hai có màu đỏ hay không và đếm kết quả. Điều đó vẫn đúng nhưng nó tạo ra công việc không cần thiết. Với 200 quả bóng, việc kiểm tra tất cả các cặp có nghĩa là khoảng 20.000 phép tính cho mỗi trường hợp thử nghiệm, điều này có thể chấp nhận được ở đây nhưng bỏ qua cấu trúc toán học. 

Nhận xét rằng tất cả các cặp đều có khả năng như nhau cho phép chúng ta thay thế mô phỏng bằng cách đếm. Một khi chúng ta biết số cặp thuận lợi và tổng số cặp thì xác suất chỉ là một phần nhỏ. Chúng ta chỉ cần chia tử số và mẫu số cho ước số chung lớn nhất của chúng để thu được dạng tối giản cần thiết. 

Lực lượng vũ phu hoạt động vì nó mô hình trực tiếp mọi lựa chọn có thể, nhưng công thức đếm hoạt động vì mọi cặp đều có xác suất giống nhau và số lượng các cặp có thể có dạng đóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r+b)^2) | O(1) | Được chấp nhận cho những giới hạn này, nhưng không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và xử lý từng cặp bóng một cách độc lập. 
2. Tính số lần lựa chọn thành công. Hai quả bóng màu đỏ có thể được chọn vào`r * (r - 1) / 2`cách vì thứ tự của các quả bóng được chọn không quan trọng. 
3. Tính tổng số các lựa chọn có thể có. Bất kỳ hai quả bóng nào trong số tất cả`r + b`quả bóng có thể được lựa chọn, đưa ra`(r + b) * (r + b - 1) / 2`khả năng. 
4. Tìm ước số chung lớn nhất của số đếm thuận lợi và tổng số đếm. Chia cả hai giá trị cho gcd này để phân số giảm đi. 
5. In phân số rút gọn bằng cách sử dụng định dạng đánh số chữ được yêu cầu. 

Phép chia cho gcd là cần thiết vì bài toán yêu cầu một phân số tối giản. Bản thân xác suất không thay đổi khi chia tử số và mẫu số cho cùng một giá trị. 

Tại sao nó hoạt động: mọi cặp bóng có thể có đều có xác suất được chọn như nhau. Tử số đếm chính xác các cặp chứa hai quả bóng màu đỏ, trong khi mẫu số đếm mọi cặp có thể. Tỉ số của chúng chính xác là xác suất cần thiết. Việc giảm theo ước số chung lớn nhất chỉ làm thay đổi cách biểu diễn của phân số chứ không làm thay đổi giá trị của nó, do đó kết quả cuối cùng tương đương về mặt toán học và không thể rút gọn. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for case in range(1, t + 1):
        r, b = map(int, input().split())

        numerator = r * (r - 1) // 2
        denominator = (r + b) * (r + b - 1) // 2

        g = gcd(numerator, denominator)
        numerator //= g
        denominator //= g

        ans.append(f"Case #{case}: {numerator}/{denominator}")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo phương pháp đếm trực tiếp.`numerator`lưu trữ số lượng lựa chọn thành công và`denominator`lưu trữ tất cả các lựa chọn có thể. 

Phép nhân được thực hiện trước khi chia vì công thức kết hợp yêu cầu tích của hai giá trị liên tiếp. Các giá trị đủ nhỏ để việc tràn số nguyên không phải là vấn đề đáng lo ngại trong Python. 

Hoạt động gcd cũng xử lý các trường hợp đặc biệt một cách tự nhiên. Nếu tử số bằng 0 thì gcd bằng 0 và mẫu số chính là mẫu số, do đó kết quả trở thành`0/1`. Nếu tử số và mẫu số bằng nhau thì gcd sẽ giảm phân số thành`1/1`. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
1
2 1
```việc thực hiện là: 

| Bước | r | b | Cặp đỏ | Tổng số cặp | gcd | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 1 | 1 | 3 | 1 | 1/3 | 
| Giảm | 2 | 1 | 1 | 3 | 1 | 1/3 | 

Có đúng một cặp chứa hai bi đỏ, còn ba cặp có thể có là`{red, red}`,`{red, blue}`, Và`{red, blue}`. Tính toán xác nhận rằng xác suất là`1/3`. 

Đối với đầu vào:```
1
8 8
```việc thực hiện là: 

| Bước | r | b | Cặp đỏ | Tổng số cặp | gcd | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 8 | 8 | 28 | 120 | 4 | 30/7 | 
| Giảm | 8 | 8 | 7 | 30 | 1 | 30/7 | 

Bước giảm phân số có thể nhìn thấy ở đây. Xác suất thô là`28/120`, nhưng chia cho gcd sẽ thu được dạng tối giản cần thiết`7/30`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi trường hợp thử nghiệm sử dụng một số phép tính số học cố định và một phép tính gcd | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ bên cạnh danh sách đầu ra | 

Với tối đa 10 trường hợp thử nghiệm và giá trị không lớn hơn 100, giải pháp này chạy dưới giới hạn rất nhiều. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case in range(1, t + 1):
        r, b = map(int, input().split())

        num = r * (r - 1) // 2
        den = (r + b) * (r + b - 1) // 2

        g = gcd(num, den)
        out.append(f"Case #{case}: {num // g}/{den // g}")

    print("\n".join(out))

assert run("""3
1 1
2 1
8 8
""") == """Case #1: 0/1
Case #2: 1/3
Case #3: 7/30
""", "samples"

assert run("""1
1 100
""") == """Case #1: 0/1
""", "one red ball"

assert run("""1
100 0
""") == """Case #1: 1/1
""", "all red balls"

assert run("""1
3 3
""") == """Case #1: 1/5
""", "balanced small case"

assert run("""1
100 100
""") == """Case #1: 99/398
""", "maximum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 100`|`0/1`| Không thể có cặp bóng đỏ | 
|`100 0`|`1/1`| Xác suất chắc chắn | 
|`3 3`|`1/5`| Giảm phân số với màu bằng nhau | 
|`100 100`|`99/398`| Giá trị tối đa được phép | 

## Vỏ cạnh 

Trường hợp chỉ có một bi đỏ:```
1
1 5
```thuật toán tính toán`1 * 0 / 2 = 0`cặp thành công. Tổng số cặp là dương nên phân số trở thành`0/6`và giảm gcd thay đổi nó thành`0/1`. 

Đối với trường hợp mọi quả bóng đều có màu đỏ:```
1
5 0
```số lượng thuận lợi và tổng số đều là`10`. gcd là`10`, sản xuất`1/1`. Điều này tránh in ra một xác suất không giảm. 

Đối với trường hợp đếm sai đơn hàng:```
1
2 1
```mô hình đúng đếm các cặp không có thứ tự. Có một cặp thành công và ba cặp tổng cộng. Thuật toán không bao giờ phân biệt giữa việc vẽ màu đỏ đầu tiên rồi đến màu đỏ thứ hai và vẽ màu đỏ thứ hai rồi đến màu đỏ đầu tiên, do đó nó phù hợp với quá trình lựa chọn được mô tả bởi bài toán.
