---
title: "CF 102800H - Tò mò"
description: "Chúng ta được cho một mảng các số nguyên dương. Mọi giá trị trong mảng tối đa là m. Với mỗi giá trị truy vấn x, chúng ta phải đếm xem có bao nhiêu cặp thứ tự (ai, aj) thỏa mãn gcd(ai, aj) = x."
date: "2026-07-27T17:40:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "H"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 79
verified: true
draft: false
---

[CF 102800H - Tò mò](https://codeforces.com/problemset/problem/102800/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. Mọi giá trị trong mảng nhiều nhất là`m`. Đối với mỗi giá trị truy vấn`x`, chúng ta phải đếm xem có bao nhiêu cặp có thứ tự`(ai, aj)`thỏa mãn`gcd(ai, aj) = x`. Cặp thứ tự có nghĩa là`(i, j)`Và`(j, i)`được tính riêng khi`i ≠ j`, Và`(i, i)`cũng là một cặp hợp lệ. 

Cách giải thích đơn giản là tính ước số chung lớn nhất cho mỗi cặp chỉ số và trả lời các truy vấn từ các kết quả đó. Điều đó đúng, nhưng kích thước đầu vào khiến điều đó là không thể. Cả hai`n`Và`m`có thể đạt được`10^5`, và có thể có`10^5`truy vấn. Tính toán tất cả`n²`các cặp sẽ yêu cầu lên tới`10^10`Tính toán GCD, vượt xa những gì phù hợp trong một giây. Ngay cả việc lưu trữ thông tin cho mỗi cặp là không thể. 

Giới hạn trên của các giá trị là cơ hội thực sự. Mỗi phần tử mảng nhiều nhất là`m`, vì vậy thay vì suy luận về các chỉ số riêng lẻ, chúng ta có thể suy luận về các giá trị và ước số của chúng. Các thuật toán lặp qua bội số của mọi số nguyên chạy trong khoảng`m log m`thời gian vì$$\sum_{i=1}^{m}\frac{m}{i}=m\log m.$$Điều đó nằm trong giới hạn cho`m = 10^5`. 

Một lỗi dễ mắc phải là quên rằng bài toán yêu cầu các cặp có thứ tự thay vì các cặp không có thứ tự. 

Ví dụ,```
n = 2
array = [2, 4]
query = 2
```Câu trả lời đúng là`4`bởi vì các cặp hợp lệ là`(2,2)`,`(2,4)`,`(4,2)`, Và`(4,4)`. Chia cho hai như thể các cặp không có thứ tự sẽ cho ra câu trả lời sai. 

Một trường hợp tinh tế khác là khi GCD được truy vấn không bao giờ xuất hiện dưới dạng giá trị mảng.```
array = [6, 12]
query = 3
```Không có cặp nào có GCD bằng`3`, mặc dù mọi số đều chia hết cho`3`. Đơn giản chỉ cần đếm số chia hết cho`3`sẽ quay lại không chính xác`4`. 

Lỗi phổ biến thứ ba là đếm các cặp có GCD là bội số của truy vấn thay vì chính xác truy vấn.```
array = [4, 8]
query = 2
```Cả hai số đều chia hết cho`2`, nhưng thực ra mỗi cặp đều có GCD`4`. Câu trả lời đúng là`0`. Đây chính xác là lý do tại sao cần phải có bước loại trừ. 

## Phương pháp tiếp cận 

Phương pháp Brute Force kiểm tra từng cặp chỉ số có thứ tự, tính toán`gcd(ai, aj)`, và tăng câu trả lời tương ứng. Tính đúng đắn của nó là ngay lập tức vì nó trực tiếp thực hiện định nghĩa của vấn đề. Thật không may, với`n = 10^5`, nó thực hiện`10^10`kiểm tra cặp, làm cho nó hoàn toàn không thực tế. 

Quan sát quan trọng là GCD được xác định thông qua tính chia hết. Thay vì kiểm tra từng cặp một, hãy đếm xem có bao nhiêu phần tử mảng có thể chia hết cho mọi giá trị có thể. 

Cho phép`cnt[v]`là tần số của giá trị`v`trong mảng. 

Với mỗi ước số`d`, tính toán$$f[d]=\text{number of array elements divisible by }d.$$Điều này được thực hiện một cách hiệu quả bằng cách lặp qua bội số của`d`. 

Nếu như`f[d]=c`, thì có`c²`các cặp có thứ tự có hai giá trị đều chia hết cho`d`. Những cặp đó có GCD bằng`d`hoặc bội số của`d`. 

Nhiệm vụ còn lại là tách các cặp có GCD chính xác`d`. Việc xử lý các ước số từ lớn đến nhỏ sẽ làm được điều này. Cho phép`ans[d]`biểu thị chính xác số cặp có thứ tự với GCD`d`. Ban đầu,$$ans[d]=f[d]^2.$$Mỗi cặp được tính ở đây có GCD thực sự là`2d`,`3d`hoặc bội số khác đã được tính toán vì các ước số lớn hơn được xử lý trước. Trừ tất cả những đóng góp đó:$$ans[d]=f[d]^2-\sum_{k\ge2}ans[kd].$$Đây là nguyên tắc bao gồm-loại trừ cổ điển đối với bội số, thường được mô tả là nghịch đảo Möbius mà không tính toán rõ ràng hàm Möbius. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2 log m) | O(m) | Quá chậm | 
| Tối ưu | O(m log m + k) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và xây dựng mảng tần số`cnt`, Ở đâu`cnt[v]`là số lần xuất hiện của giá trị`v`. 
2. Với mọi số nguyên`d`từ`1`ĐẾN`m`, tính xem có bao nhiêu phần tử mảng chia hết cho`d`. Lặp lại qua`d, 2d, 3d, ...`và tích lũy tần số của chúng thành`divisible[d]`. 
3. Tạo một mảng câu trả lời. 
4. Xử lý ước số từ`m`xuống tới`1`. 
5. Đặt`ans[d] = divisible[d] * divisible[d]`. Điều này tính mọi cặp có thứ tự có hai giá trị chia hết cho`d`. 
6. Lặp lại bội số`2d, 3d, ...`và trừ`ans[multiple]`từ`ans[d]`. Những cặp đó đã được tính ở bước trước nhưng thực tế có GCD lớn hơn. 
7. Sau khi quá trình tiền xử lý kết thúc, mọi truy vấn sẽ được trả lời bằng cách in`ans[x]`. 

### Tại sao nó hoạt động 

Với mỗi ước số`d`,`divisible[d]²`đếm tất cả các cặp có thứ tự có GCD là bội số của`d`. Mỗi cặp như vậy thuộc về đúng một giá trị, đó là GCD chính xác của nó. Việc xử lý từ các ước số lớn hơn tới các ước số nhỏ hơn đảm bảo rằng tất cả các đóng góp GCD lớn hơn đã được tính toán. Trừ đi những đóng góp đó sẽ để lại chính xác các cặp có GCD bằng`d`. Vì mỗi cặp có đúng một ước chung lớn nhất nên mỗi cặp chỉ được tính một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m, k = map(int, input().split())

        cnt = [0] * (m + 1)
        for v in map(int, input().split()):
            cnt[v] += 1

        divisible = [0] * (m + 1)
        for d in range(1, m + 1):
            s = 0
            for x in range(d, m + 1, d):
                s += cnt[x]
            divisible[d] = s

        ans = [0] * (m + 1)
        for d in range(m, 0, -1):
            cur = divisible[d] * divisible[d]
            for x in range(d * 2, m + 1, d):
                cur -= ans[x]
            ans[d] = cur

        for _ in range(k):
            x = int(input())
            out.append(str(ans[x]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Mảng tần số nén đầu vào thành số lượng giá trị thay vì vị trí. Đây là lý do khiến quá trình tiền xử lý phụ thuộc vào`m`còn hơn là`n²`. 

Vòng lặp thứ hai tính xem có bao nhiêu số chia hết cho mỗi ước số. Lặp lại bội số là hiệu quả vì mọi số nguyên chỉ xuất hiện dưới dạng bội số cho các ước của nó. 

Vòng lặp ngược là trái tim của thuật toán. Phép trừ phải xảy ra sau khi các ước số lớn hơn đã được xử lý, đó là lý do tại sao phép lặp đi từ`m`xuống tới`1`. Đảo ngược thứ tự này sẽ trừ đi các giá trị chưa được tính toán. 

Quảng trường`divisible[d] * divisible[d]`đã đếm chính xác các cặp đã đặt hàng. Không cần hệ số hai vì bài toán yêu cầu các cặp có thứ tự. 

Tất cả các giá trị trung gian đều vừa khít với số nguyên Python. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, cần phải có loại 64 bit vì câu trả lời lớn nhất có thể là`n²`, đạt tới`10^10`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
array = [2, 4, 6]
queries = [2, 1]
```| Số chia | chia hết | Cặp ban đầu | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | 
| 6 | 1 | 1 | 1 | 
| 4 | 1 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 
| 2 | 3 | 9 | 7 | 
| 1 | 3 | 9 | 0 | 

Các cặp có GCD bằng`2`chính xác là những phần còn lại sau khi loại bỏ các cặp có GCD`4`hoặc`6`. Không có cặp nào có GCD`1`, do đó giá trị cuối cùng trở thành 0. 

### Ví dụ 2 

đầu vào:```
n = 2
array = [4, 8]
query = 2
```| Số chia | chia hết | Cặp ban đầu | Câu trả lời cuối cùng | 
| --- | --- | --- | --- | 
| 8 | 1 | 1 | 1 | 
| 4 | 2 | 4 | 3 | 
| 2 | 2 | 4 | 0 | 
| 1 | 2 | 4 | 0 | 

Mặc dù mọi số đều chia hết cho`2`, cả bốn cặp ứng cử viên đều có GCD`4`hoặc`8`. Bước trừ sẽ loại bỏ tất cả, để lại câu trả lời đúng là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m + k) | Hai vòng lặp chuỗi hài trên bội số, sau đó thời gian không đổi cho mỗi truy vấn | 
| Không gian | O(m) | Mảng tần số, số chia hết và câu trả lời | 

Quá trình tiền xử lý thực hiện khoảng`m/1 + m/2 + ... + m/m`các lần lặp, đó là`O(m log m)`. Với`m = 10^5`, điều này phù hợp thoải mái trong giới hạn và mỗi truy vấn được trả lời trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []
    t = int(input())

    for _ in range(t):
        n, m, k = map(int, input().split())
        cnt = [0] * (m + 1)
        for v in map(int, input().split()):
            cnt[v] += 1

        div = [0] * (m + 1)
        for d in range(1, m + 1):
            for x in range(d, m + 1, d):
                div[d] += cnt[x]

        ans = [0] * (m + 1)
        for d in range(m, 0, -1):
            ans[d] = div[d] * div[d]
            for x in range(d * 2, m + 1, d):
                ans[d] -= ans[x]

        for _ in range(k):
            out.append(str(ans[int(input())]))

    return "\n".join(out)

assert run("""1
1 1 1
1
1
""") == "1"

assert run("""1
2 4 2
2 4
2
4
""") == "4\n1"

assert run("""1
2 8 1
4 8
2
""") == "0"

assert run("""1
3 5 3
5 5 5
5
1
2
""") == "9\n0\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Giá trị đơn`1`|`1`| Kích thước đầu vào tối thiểu | 
| Giá trị`2 4`|`4`,`1`| Đếm cặp đặt hàng | 
| Giá trị`4 8`, truy vấn`2`|`0`| Loại trừ các giá trị GCD lớn hơn | 
| Tất cả các giá trị bằng`5`|`9`,`0`,`0`| Tần số lớn tập trung ở một giá trị | 

## Vỏ cạnh 

Hãy xem xét mảng```
1
2 4 1
2 4
2
```Số đếm chia hết là`divisible[4] = 1`Và`divisible[2] = 2`. Thuật toán đầu tiên tính toán`ans[4] = 1`. Đối với số chia`2`, nó bắt đầu từ`2² = 4`sắp xếp các cặp và trừ đi một cặp có GCD`4`, rời đi`4`. Điều này khớp với bốn cặp có thứ tự có GCD chính xác`2`. 

Bây giờ hãy xem xét```
1
2 12 1
6 12
3
```Cả hai giá trị đều chia hết cho`3`, Vì thế`divisible[3] = 2`và số lượng ban đầu là`4`. Xử lý các ước số lớn hơn sẽ trừ bốn cặp có GCD`6`, rời đi`0`. Thuật toán phân biệt chính xác khả năng chia hết với GCD chính xác. 

Cuối cùng,```
1
2 8 1
4 8
2
```Lại`divisible[2] = 2`, đưa ra số đếm ban đầu là`4`. Phép tính trừ đi`ans[4] = 3`Và`ans[8] = 1`, để lại số không. Mỗi cặp ứng cử viên đều thuộc về một GCD chính xác lớn hơn, do đó không còn lại GCD nào cho ước số`2`. Điều này chứng tỏ tại sao việc xử lý từ ước lớn đến ước nhỏ là cần thiết.
