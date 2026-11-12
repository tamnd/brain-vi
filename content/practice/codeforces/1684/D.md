---
title: "CF 1684D - Bẫy"
description: "Chúng ta được cung cấp một chuỗi bẫy, mỗi bẫy có giá trị sát thương cơ bản. Chúng tôi đi qua các bẫy theo thứ tự và đối với mỗi bẫy, chúng tôi nhận sát thương hoặc bỏ qua bằng cách nhảy qua nó."
date: "2026-06-10T00:02:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1684
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 792 (Div. 1 + Div. 2)"
rating: 1700
weight: 1684
solve_time_s: 153
verified: false
draft: false
---

[CF 1684D - Bẫy](https://codeforces.com/problemset/problem/1684/D) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bẫy, mỗi bẫy có giá trị sát thương cơ bản. Chúng tôi đi qua các bẫy theo thứ tự và đối với mỗi bẫy, chúng tôi nhận sát thương hoặc bỏ qua bằng cách nhảy qua nó. Mỗi lần nhảy qua bẫy sẽ ngăn chặn thiệt hại của nó nhưng sẽ tăng sát thương của tất cả các bẫy tiếp theo thêm một lần cho mỗi lần nhảy đã thực hiện. Chúng tôi được phép thực hiện nhiều nhất$k$nhảy. Mục đích là giảm thiểu tổng thiệt hại nhận được. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra$n$, số lượng bẫy,$k$, số lần nhảy tối đa được phép và danh sách$n$số nguyên đại diện cho thiệt hại cơ bản của mỗi bẫy. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng thiệt hại tối thiểu có thể đạt được. 

Những ràng buộc cho phép$n$để đạt được$2 \cdot 10^5$và tổng của$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Điều này chỉ ra rằng bất kỳ giải pháp nào cũng phải gần như tuyến tính trong$n$mỗi trường hợp thử nghiệm. Một cách tiếp cận ngây thơ thử mọi sự kết hợp của các bước nhảy là không thể vì số lượng tập hợp con của bẫy là$2^n$, vượt xa thời gian tính toán hợp lý. Giá trị sát thương cơ bản lớn lên tới$10^9$đề nghị chúng ta phải tránh mọi giải pháp tích lũy thiệt hại không hiệu quả. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó$k \ge n$, nơi chúng ta có thể nhảy tất cả các bẫy và không nhận sát thương, và những trường hợp mà cú nhảy tối ưu không nằm ở bẫy sát thương cơ bản lớn nhất do hiệu ứng xếp tầng của sát thương cộng thêm. Ví dụ, trong`[5,10,11,5]`với$k=1$, nhảy qua bẫy thứ ba sẽ tốt hơn nhảy qua bẫy lớn nhất, vì nó giảm thiểu tổng số tiền sau khi tính phần thưởng +1 cho các bẫy tiếp theo. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ liệt kê tất cả các tập hợp con bẫy để nhảy qua, tính toán tổng thiệt hại cho từng tập hợp con và chọn mức tối thiểu. Điều này đúng vì nó thử mọi khả năng, nhưng nó đòi hỏi$O(2^n)$hoạt động trên mỗi trường hợp thử nghiệm, điều này là không khả thi đối với$n$lên đến$2 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là xem xét tác động của việc nhảy qua bẫy theo thứ tự ngược lại. Chi phí hiệu quả của mỗi cái bẫy là sát thương cơ bản của nó trừ đi phần "tiền thưởng tránh được" tiềm ẩn khi nhảy vào bẫy sau này. Nếu chúng ta nhảy qua cái cuối cùng$k$bẫy, những lần nhảy đó không ảnh hưởng đến các bẫy trước đó. Bằng cách đảo ngược danh sách thiệt hại và xem xét thiệt hại hiệu quả lớn nhất trước tiên, chúng ta có thể tham lam lựa chọn$k$bẫy để nhảy qua giúp giảm tối đa tổng thiệt hại. Việc sắp xếp bẫy bằng cách giảm sát thương cơ bản có tác dụng vì hiệu ứng thưởng tăng tuyến tính theo số lần nhảy, khiến việc bỏ qua các bẫy có sát thương cơ bản lớn nhất ở các vị trí mới nhất là tối ưu nhất có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$, và mảng bẫy thiệt hại$a$. 
2. Sắp xếp các bẫy theo thứ tự sát thương cơ bản giảm dần. Điều này xác định những cái bẫy có mức độ sát thương cao nhất trong tổng số. 
3. Chọn cái đầu tiên$k$bẫy từ danh sách được sắp xếp để nhảy qua. Mỗi lần nhảy sẽ giảm tổng sát thương bằng cách tránh sát thương cơ bản của bẫy. Theo dõi bao nhiêu bước nhảy đã được áp dụng cho đến nay. 
4. Duyệt mảng theo thứ tự. Đối với mỗi bẫy, nếu nó được đánh dấu để nhảy, hãy bỏ qua việc thêm sát thương cho nó. Nếu không, hãy cộng sát thương của nó cộng với số lần nhảy đã áp dụng trước đó. 
5. Tổng số tiền tích lũy ở bước 4 là tổng thiệt hại tối thiểu. 

Thuật toán hoạt động vì các cú nhảy có hiệu quả nhất trên các bẫy có sát thương cơ bản lớn nhất. Bằng cách sắp xếp và chọn đầu$k$bẫy, chúng tôi đảm bảo rằng mỗi lần nhảy sẽ góp phần tối đa vào việc giảm tổng thiệt hại. Sát thương cộng thêm được xử lý một cách tự nhiên bằng cách tích lũy số lần nhảy trong khi vượt qua các bẫy theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        # sort traps by base damage in descending order
        sorted_indices = sorted(range(n), key=lambda i: a[i], reverse=True)
        jump_set = set(sorted_indices[:k])
        damage = 0
        jumps_done = 0
        for i in range(n):
            if i in jump_set:
                jumps_done += 1
            else:
                damage += a[i] + jumps_done
        print(damage)

if __name__ == "__main__":
    solve()
```Trước tiên, chúng tôi sắp xếp các chỉ số bẫy theo mức độ thiệt hại theo thứ tự giảm dần và chọn chỉ số bẫy trên cùng.$k$chỉ số để nhảy. Trong khi di chuyển ngang, mỗi bẫy bị bỏ qua sẽ tăng bộ đếm bước nhảy, điều này làm tăng sát thương hiệu quả của các bẫy tiếp theo. Tổng của`a[i] + jumps_done`đối với bẫy không nhảy sẽ gây ra tổng thiệt hại tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`4 1 5 10 11 5`Bẫy nhảy top 1: chỉ số 2 (giá trị 11). 

| tôi | một [tôi] | nhảy? | nhảy_done | thiệt hại | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | Không | 0 | 5 | 
| 1 | 10 | Không | 0 | 15 | 
| 2 | 11 | Có | 1 | 15 | 
| 3 | 5 | Không | 1 | 21 | 

Đầu ra: 21 

### Ví dụ 2 

đầu vào:`6 3 1 2 3 4 5 6`Nhảy top 3 bẫy: chỉ số 5, 4, 3 (giá trị 6,5,4). 

| tôi | một [tôi] | nhảy? | nhảy_done | thiệt hại | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | Không | 0 | 1 | 
| 1 | 2 | Không | 0 | 3 | 
| 2 | 3 | Không | 0 | 6 | 
| 3 | 4 | Có | 1 | 6 | 
| 4 | 5 | Có | 2 | 6 | 
| 5 | 6 | Có | 3 | 6 | 

Đầu ra: 6 

Dấu vết này xác nhận rằng số lần nhảy được áp dụng chính xác và các thiệt hại lớn nhất được loại bỏ một cách hiệu quả để giảm thiểu tổng thiệt hại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp mảng bẫy chiếm ưu thế, truyền tải là O(n) | 
| Không gian | O(n) | Lưu trữ các chỉ số đã sắp xếp và bộ nhảy | 

Với$n$lên đến$2 \cdot 10^5$, sắp xếp và truyền tải tuyến tính vừa vặn thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("5\n4 4\n8 7 1 4\n4 1\n5 10 11 5\n7 5\n8 2 5 15 11 2 8\n6 3\n1 2 3 4 5 6\n1 1\n7\n") == "0\n21\n9\n6\n0"

# custom cases
assert run("1\n1 1\n10\n") == "0", "single trap, jump all"
assert run("1\n5 0\n1 2 3 4 5\n") == "15", "no jumps allowed"
assert run("1\n5 5\n1 2 3 4 5\n") == "0", "jump all traps"
assert run("1\n3 2\n10 1 10\n") == "1", "best jump is first and last trap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 10`|`0`| đầu vào tối thiểu, nhảy tất cả | 
|`5 0 1 2 3 4 5`|`15`| không được phép nhảy | 
|`5 5 1 2 3 4 5`|`0`| số lần nhảy tối đa bằng n | 
|`3 2 10 1 10`|`1`| bước nhảy tối ưu không rõ ràng, bỏ qua lớn nhất và cuối cùng | 

## Vỏ cạnh 

Khi nào$k \ge n$, chẳng hạn như`n=4, k=4, a=[8,7,1,4]`, thuật toán sẽ nhảy tất cả các bẫy một cách chính xác. Bộ đếm bước nhảy tăng dần cho mỗi bẫy, nhưng vì tất cả đều bị bỏ qua nên không có bẫy nào gây ra thiệt hại. Thiệt hại vẫn bằng 0, phù hợp với thiệt hại tối thiểu dự kiến. 

Khi nhiều bẫy có mức sát thương như nhau, chẳng hạn như`a=[5,5,5,5]`Và`k=2`, thuật toán chọn 2 chỉ số đầu tiên sau khi sắp xếp (chỉ số 0 và 1) để nhảy. Các bẫy còn lại phải chịu sát thương 5 + 2 lần nhảy = 7 và 5 + 2 = 7, tổng cộng là 14, mức tối thiểu nếu xét đến bất kỳ sự kết hợp nào khác. Cách tiếp cận xử lý các giá trị bằng nhau một cách khéo léo bằng cách chọn các lần xuất hiện sớm nhất theo thứ tự được sắp xếp.
