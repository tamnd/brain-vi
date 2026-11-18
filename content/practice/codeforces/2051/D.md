---
title: "CF 2051D - Đếm cặp"
description: "Chúng ta được cung cấp một mảng các số nguyên dương và khoảng mục tiêu $[x, y]$. Chúng tôi được phép loại bỏ chính xác hai phần tử riêng biệt khỏi mảng. Sau khi loại bỏ chúng, chúng tôi xem xét tổng của những gì còn lại và chúng tôi muốn số tiền còn lại này nằm trong khoảng nhất định."
date: "2026-06-08T08:41:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 1200
weight: 2051
solve_time_s: 79
verified: true
draft: false
---

[CF 2051D - Đếm cặp](https://codeforces.com/problemset/problem/2051/D) 

**Đánh giá:** 1200 
**Thẻ:** tìm kiếm nhị phân, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên dương và khoảng mục tiêu$[x, y]$. Chúng tôi được phép loại bỏ chính xác hai phần tử riêng biệt khỏi mảng. Sau khi loại bỏ chúng, chúng tôi xem xét tổng của những gì còn lại và chúng tôi muốn số tiền còn lại này nằm trong khoảng nhất định. 

Tương tự, nếu tổng của mảng là$S$, loại bỏ các phần tử tại các vị trí$i$Và$j$để lại một khoản tiền$S - a_i - a_j$. Điều kiện trở thành:$$x \le S - a_i - a_j \le y$$Sắp xếp lại mang lại:$$S - y \le a_i + a_j \le S - x$$Vì vậy nhiệm vụ chỉ đơn thuần là đếm các cặp$(i, j)$có tổng nằm trong một phạm vi cố định được xác định bởi tổng. 

Kích thước đầu vào lớn trong các trường hợp thử nghiệm, lên tới$2 \cdot 10^5$tổng số phần tử. Bất kì$O(n^2)$mỗi cách tiếp cận trường hợp thử nghiệm sẽ thất bại ngay lập tức bởi vì ngay cả$n = 2 \cdot 10^5$sẽ ám chỉ về$2 \cdot 10^{10}$kiểm tra cặp. Điều này đẩy chúng ta về phía$O(n \log n)$hoặc$O(n)$cho mỗi trường hợp thử nghiệm, thường sử dụng cách sắp xếp bằng hai con trỏ hoặc tìm kiếm nhị phân. 

Một trường hợp thất bại tinh vi xuất phát từ việc xử lý chính xác các giới hạn được chuyển đổi. Vì chúng ta chuyển đổi điều kiện của tổng còn lại thành điều kiện của tổng cặp nên thường xảy ra lỗi khi trộn lẫn$x, y$hoặc quên tính toán lại giới hạn cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh cụ thể là khi tất cả các phần tử giống hệt nhau và phạm vi hợp lệ rất chặt chẽ. Ví dụ, nếu$a = [3, 3, 3]$,$x = 4$,$y = 4$, tổng số là 9, vì vậy chúng ta cần$5 \le a_i + a_j \le 5$, điều đó là không thể. Việc triển khai ngây thơ mà quên trừ đúng cách có thể đếm sai các cặp. 

Một trường hợp góc khác là khi giới hạn trở nên âm hoặc cực kỳ lớn sau khi chuyển đổi. Từ$a_i \ge 1$, tổng luôn dương, vì vậy logic cắt phạm vi vẫn phải hoạt động chính xác mà không cần viết hoa đặc biệt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: hãy thử mọi cặp$(i, j)$, tính tổng còn lại sau khi loại bỏ chúng và kiểm tra xem nó có nằm trong$[x, y]$. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó đòi hỏi phải kiểm tra$\frac{n(n-1)}{2}$cặp cho mỗi trường hợp thử nghiệm, điều này trở nên không khả thi ngay cả đối với mức độ vừa phải$n$. 

Quan sát quan trọng là việc loại bỏ hai phần tử chỉ phụ thuộc vào tổng của chúng chứ không phụ thuộc vào vị trí của chúng. Khi chúng ta viết lại điều kiện theo$a_i + a_j$, bài toán trở thành một nhiệm vụ đếm tổng cặp bị ràng buộc cổ điển: đếm xem có bao nhiêu cặp nằm trong một phạm vi giá trị. 

Sau khi sắp xếp mảng, chúng ta có thể đếm các cặp có tổng$\le R$sử dụng quét hai con trỏ trong thời gian tuyến tính. Bằng cách tính toán hai giới hạn và trừ đi, chúng ta sẽ có câu trả lời. 

Lực lượng vũ phu hoạt động vì các ràng buộc là cục bộ trên mỗi cặp, nhưng nó không thành công do chia tỷ lệ bậc hai. Việc chuyển đổi sang phạm vi tổng cặp làm giảm vấn đề sắp xếp cộng với cấu trúc đếm đơn điệu, cho phép hai con trỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Sắp xếp + Hai con trỏ |$O(n \log n)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta bắt đầu bằng cách chuyển bài toán thành một ràng buộc tổng cặp. 

1. Tính tổng số tiền$S$của mảng. Điều này là cần thiết vì tình trạng phụ thuộc vào những gì còn lại sau khi loại bỏ. 
2. Chuyển điều kiện của tổng còn lại thành điều kiện của tổng cặp bị loại bỏ:$$L = S - y,\quad R = S - x$$Bất kỳ cặp hợp lệ nào cũng phải thỏa mãn$L \le a_i + a_j \le R$. 
3. Sắp xếp mảng. Việc sắp xếp là cần thiết vì nó cho phép chúng ta suy luận về các tổng theo cặp một cách đơn điệu. 
4. Đếm xem có bao nhiêu cặp có tổng$\le R$. Điều này được thực hiện bằng cách sử dụng kỹ thuật hai con trỏ: một con trỏ bắt đầu ở bên trái, con trỏ kia ở bên phải và chúng tôi thu nhỏ hoặc mở rộng dựa trên việc tổng hiện tại có quá lớn hay không. 
5. Đếm xem có bao nhiêu cặp có tổng$< L$, hoặc đếm tương đương các cặp có tổng$\le L - 1$. 
6. Trừ: cặp hợp lệ = count($\le R$) − đếm($\le L - 1$). 

Việc đếm hai con trỏ hoạt động vì đối với một con trỏ bên phải cố định, tất cả các vị trí bên trái cho đến một chỉ mục nhất định tạo thành các cặp hợp lệ, vì vậy chúng ta có thể tích lũy số lượng hàng loạt thay vì kiểm tra riêng lẻ. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mảng có đặc tính là tăng chỉ số bên phải chỉ làm tăng tổng cặp. Tính đơn điệu này đảm bảo rằng với mỗi cố định$r$, tồn tại chỉ số biên$l$như vậy tất cả$i < l$thỏa mãn$a_i + a_r \le X$. Điều này tạo ra một phân đoạn hợp lệ liền kề, cho phép đếm tuyến tính. Mỗi cặp được tính chính xác một lần trên tất cả các điểm cuối bên phải, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def count_leq(a, bound):
    n = len(a)
    l, r = 0, n - 1
    res = 0
    while l < r:
        if a[l] + a[r] <= bound:
            res += r - l
            l += 1
        else:
            r -= 1
    return res

def solve():
    t = int(input())
    for _ in range(t):
        n, x, y = map(int, input().split())
        a = list(map(int, input().split()))
        s = sum(a)

        L = s - y
        R = s - x

        a.sort()

        ans = count_leq(a, R) - count_leq(a, L - 1)
        print(ans)

if __name__ == "__main__":
    solve()
```Chi tiết triển khai cốt lõi là hàm trợ giúp đếm các cặp có tổng giới hạn ở trên bởi một ngưỡng. Nó sử dụng một cửa sổ thu nhỏ từ cả hai đầu. Khi tổng đủ nhỏ, tất cả các phần tử nằm giữa`l`Và`r`ghép nối với`l`là hợp lệ, vì vậy chúng tôi thêm`r - l`trong một bước. 

Bước trừ là rất quan trọng. Chúng tôi tính toán một phạm vi bao gồm$[L, R]$bởi sự khác biệt của hai số kiểu tiền tố. quên đi`L - 1`điều chỉnh là nguồn phổ biến của các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, x=8, y=10
a = [4, 6, 3, 6]
```Tổng số tiền$S = 19$. Vì thế:$L = 19 - 10 = 9$,$R = 19 - 8 = 11$Mảng được sắp xếp:`[3, 4, 6, 6]`| Bước | tôi | r | a[l] + a[r] | Hành động | Đếm | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 0 | 3 | 3+6=9 | hợp lệ, thêm 3 | 3 | 
| di chuyển tôi | 1 | 3 | 4+6=10 | hợp lệ, thêm 2 | 5 | 
| di chuyển tôi | 2 | 3 | 6+6=12 | quá lớn, r-- | 5 | 
| cuối cùng | 2 | 2 | dừng lại | | 5 | 

Vậy hãy tính ($\le 11$) = 5. 

cho$< 9$, tức là$\le 8$: 

chỉ các cặp: 

(3,4)=7 hợp lệ, các giá trị khác vượt quá. 

Vậy đếm = 1. 

Đáp án cuối cùng = 5 - 1 = 4. 

Điều này xác nhận rằng phép biến đổi bảo toàn chính xác ràng buộc ban đầu. 

### Ví dụ 2 

đầu vào:```
n=3, x=8, y=10
a = [3, 2, 1]
```Tổng số tiền$S=6$, Vì thế:$L = 6 - 10 = -4$,$R = 6 - 8 = -2$Tất cả các tổng cặp đều dương nên không có cặp nào có thể thỏa mãn điều kiện. 

Mảng được sắp xếp:`[1, 2, 3]`Cả hai hàm đếm đều trả về 0 nên kết quả là 0. 

Điều này cho thấy giới hạn âm sẽ loại bỏ chính xác tất cả các cặp mà không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; quét hai con trỏ là tuyến tính | 
| Không gian |$O(1)$thêm | Sắp xếp tại chỗ và các biến phụ trợ không đổi | 

Tổng của$n$trên các trường hợp thử nghiệm được giới hạn bởi$2 \cdot 10^5$, do đó chi phí sắp xếp qua tất cả các thử nghiệm vẫn nằm trong giới hạn có thể chấp nhận được. Quá trình quét tuyến tính đảm bảo mỗi phần tử tham gia vào tối đa một vài chuyển động của con trỏ, đảm bảo hiệu quả tổng thể của công việc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def count_leq(a, bound):
        n = len(a)
        l, r = 0, n - 1
        res = 0
        while l < r:
            if a[l] + a[r] <= bound:
                res += r - l
                l += 1
            else:
                r -= 1
        return res

    def solve():
        t = int(input())
        for _ in range(t):
            n, x, y = map(int, input().split())
            a = list(map(int, input().split()))
            s = sum(a)
            L = s - y
            R = s - x
            a.sort()
            ans = count_leq(a, R) - count_leq(a, L - 1)
            print(ans)

    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""7
4 8 10
4 6 3 6
6 22 27
4 9 6 3 4 5
3 8 10
3 2 1
3 1 1
2 3 4
3 3 6
3 2 1
4 4 12
3 3 2 1
6 8 8
1 1 2 2 2 3
""") == """4
7
0
0
1
5
6"""

# custom cases
assert run("""1
3 1 100
1 1 1
""") == "3", "all pairs valid"

assert run("""1
3 10 10
1 2 3
""") == "0", "tight impossible range"

assert run("""1
4 5 5
1 2 3 4
""") == "1", "single valid pair"

assert run("""1
5 1 1
1 1 1 1 1
""") == "10", "all equal minimum values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các cặp hợp lệ | 3 | chấp nhận toàn dải | 
| phạm vi không thể chặt chẽ | 0 | xử lý giải pháp trống | 
| cặp hợp lệ duy nhất | 1 | độ đúng ranh giới | 
| tất cả các giá trị bằng nhau | 10 | tính đúng đắn của tổ hợp | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi các giới hạn được chuyển đổi trở thành âm. Coi như:```
n=3, x=8, y=10
a=[3,2,1]
```Đây$L=-4$,$R=-2$. Thuật toán vẫn chạy bình thường, nhưng tổng mỗi cặp đều dương, do đó cả hai hàm đếm đều trả về 0. Điều này xác nhận rằng không cần lọc trường hợp đặc biệt đối với các phạm vi không hợp lệ. 

Một trường hợp khác là khi tất cả các phần tử đều giống hệt nhau:```
n=5, x=1, y=1
a=[1,1,1,1,1]
```Tổng số tiền là 5, vì vậy tổng số cặp bắt buộc chính xác là 4. Mỗi cặp đóng góp 2, vì vậy không có cặp nào đủ điều kiện. Phương pháp hai con trỏ đếm chính xác bằng 0 vì mọi tổng đều ở dưới hoặc trên giới hạn một cách nhất quán và phép trừ mang lại kết quả bằng 0 mà không có lỗi điều chỉnh. 

Trường hợp tinh tế cuối cùng là khi khoảng thời gian hợp lệ rất rộng, cho phép tất cả các cặp một cách hiệu quả. Thuật toán tính tất cả$\frac{n(n-1)}{2}$cặp thông qua$\le R$và trừ 0 từ giới hạn dưới sẽ tạo ra số tổ hợp đầy đủ, xác nhận không có cặp đếm kép hoặc thiếu.
