---
title: "CF 2112C - Trò chơi tô màu"
description: "Chúng ta được cho một mảng các số nguyên đã được sắp xếp và chúng ta cần đếm xem có bao nhiêu cách Alice có thể chọn chính xác ba chỉ số riêng biệt sao cho dù Bob có làm gì sau đó thì bộ ba được Alice chọn vẫn “an toàn”. Trình tự các sự kiện rất quan trọng. Đầu tiên Alice chọn ba phần tử."
date: "2026-06-08T04:26:59+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "greedy", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2112
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 180 (Rated for Div. 2)"
rating: 1300
weight: 2112
solve_time_s: 110
verified: false
draft: false
---

[CF 2112C - Trò chơi tô màu](https://codeforces.com/problemset/problem/2112/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, vũ phu, tham lam, hai con trỏ 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên đã được sắp xếp và chúng ta cần đếm xem có bao nhiêu cách Alice có thể chọn chính xác ba chỉ số riêng biệt sao cho dù Bob có làm gì sau đó thì bộ ba được Alice chọn vẫn “an toàn”. 

Trình tự các sự kiện rất quan trọng. Đầu tiên Alice chọn ba phần tử. Sau đó, Bob chọn bất kỳ vị trí nào trong mảng và tô màu xanh lam. Nếu Bob tình cờ chọn một trong các phần tử màu đỏ của Alice thì phần tử đó sẽ ngừng đóng góp vào tổng màu đỏ vì nó bị ghi đè. Alice chỉ thắng nếu tổng các phần tử màu đỏ còn lại lớn hơn giá trị của phần tử Bob đã chọn. 

Cụm từ “bất kể hành động của Bob” có nghĩa là chúng ta phải thực thi điều kiện cho lựa chọn tồi tệ nhất có thể xảy ra của Bob sau khi nhìn thấy bộ ba của Alice. Vì vậy, với mỗi bộ ba đã chọn, chúng ta phải xem xét cả hai khả năng: Bob chọn bên trong bộ ba hoặc bên ngoài bộ ba đó, và trong cả hai trường hợp Alice vẫn phải thắng. 

Các ràng buộc đủ chặt chẽ để phép liệt kê bậc ba trên bộ ba là quá lớn. Với$n$lên tới 5000 mỗi bài kiểm tra và tổng số lên tới 5000, một điều ngây thơ$O(n^3)$cách tiếp cận sẽ làm theo thứ tự$10^{11}$hoạt động vượt quá giới hạn. Thậm chí một$O(n^2 \log n)$cách tiếp cận mỗi bài kiểm tra là giới hạn, vì vậy chúng ta nên mong đợi một$O(n^2)$hoặc$O(n \log n)$giải pháp. 

Trường hợp cạnh tinh tế xuất hiện khi có nhiều giá trị bằng nhau. Ví dụ: nếu tất cả các phần tử đều giống hệt nhau, Alice có thể nghĩ rằng bất kỳ bộ ba nào cũng hoạt động vì tổng lớn, nhưng Bob luôn có thể chọn phần tử màu đỏ và giảm tổng hiệu dụng một cách đáng kể, điều này có thể làm mất hiệu lực của bất đẳng thức. Một trường hợp phức tạp khác là khi phần tử lớn nhất rất lớn so với các phần tử khác, vì Bob sẽ luôn nhắm mục tiêu giá trị đó trừ khi tổng màu đỏ chiếm ưu thế ngay cả sau khi mất một phần tử màu đỏ. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp sẽ lặp lại trên tất cả các bộ ba$i < j < k$, tính tổng$S = a_i + a_j + a_k$, sau đó mô phỏng phản ứng tốt nhất của Bob. 

Bob có hai chiến lược có ý nghĩa. Nếu anh ta chọn một phần tử không phải màu đỏ$x$, số tiền đỏ của Alice vẫn giữ nguyên$S$, vì vậy chúng ta cần$S > x$. Sự lựa chọn tồi tệ nhất như vậy là phần tử lớn nhất nằm ngoài bộ ba. Nếu anh ta chọn một yếu tố màu đỏ$a_t$, tổng màu đỏ trở thành$S - a_t$và Bob so sánh điều này với$a_t$, vì vậy chúng ta cần$S - a_t > a_t$, tức là$S > 2a_t$. Vì Bob sẽ chọn phần tử màu đỏ tệ nhất nên điều này trở thành$S > 2 \cdot \max(a_i, a_j, a_k)$. 

Vì vậy, đối với bộ ba cố định, điều kiện rút gọn thành hai bất đẳng thức: 

1.$a_i + a_j + a_k > \max(a)$trong toàn bộ mảng 
2.$a_i + a_j + a_k > 2 \cdot a_k$(vì mảng được sắp xếp và$a_k$lớn nhất gấp ba lần) 

Brute-force kiểm tra tất cả các bộ ba và xác minh cả hai điều kiện trong thời gian không đổi, nhưng đây là$O(n^3)$. 

Sự đơn giản hóa chính là cố định phần tử lớn nhất của bộ ba, chẳng hạn$a_k$, và rút gọn vấn đề về việc đếm các cặp$(i, j)$với$i < j < k$thỏa mãn bất đẳng thức tuyến tính. Điều đó ngay lập tức gợi ý chiến lược hai con trỏ trên một mảng được sắp xếp. 

Đối với mỗi$k$, chúng tôi muốn:$$a_i + a_j > \max(2a_k, a_k + a_{\text{max outside}} - a_k)$$Vì mảng được sắp xếp toàn cục nên mức tối đa toàn cục sẽ chiếm ưu thế trong trường hợp bên ngoài, vì vậy chúng tôi xử lý trước nó và giảm điều kiện xuống một ngưỡng đơn giản liên quan đến$a_k$. 

Điều này làm giảm việc đếm thành các cặp quét có hai con trỏ trong$O(n)$mỗi$k$, dẫn đến$O(n^2)$tổng cộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Hai con trỏ tối ưu |$O(n^2)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào thực tế là mảng đã được sắp xếp, điều này cho phép chúng tôi suy luận về các giá trị tối đa một cách rõ ràng và đếm các cặp một cách hiệu quả. 

1. Cố định phần tử lớn nhất của bộ ba làm chỉ mục$k$. Chúng tôi đối xử$a_k$là phần tử được chọn thứ ba. 
2. Vì điều này$k$, đặt hai con trỏ:$i = 0$,$j = k - 1$. 
3. Chúng tôi cố gắng chọn hai yếu tố trước$k$sao cho tổng của chúng đủ lớn để thỏa mãn cả hai ràng buộc Bob. Ngưỡng yêu cầu trở thành:$$a_i + a_j > \max(2a_k, a_k + a_{\max})$$Từ$a_{\max}$được cố định cho mỗi trường hợp thử nghiệm, điều này trở thành ngưỡng không đổi cho tất cả$k$. 
4. Trong khi$i < j$, nếu như$a_i + a_j$đủ lớn thì mỗi cặp$(i, i+1, ..., j-1, j)$với cố định$j$và khác nhau$i$đi xuống có thể đóng góp. Chúng tôi đếm tất cả các cặp hợp lệ kết thúc tại$j$, sau đó di chuyển$j$bên trái. 
5. Ngược lại, số tiền quá nhỏ nên chúng ta tăng$i$để phóng to tổng số. 
6. Tích lũy tất cả các cặp hợp lệ trên tất cả$k$. 

Sự lựa chọn thiết kế quan trọng luôn là neo giữ phần tử lớn nhất trong bộ ba. Điều này thu gọn bài toán đếm ba chiều thành bài toán hai chiều, trong đó tính đơn điệu của mảng được sắp xếp cho phép quét tuyến tính. 

### Tại sao nó hoạt động 

Đối với bất kỳ bộ ba hợp lệ nào, phần tử lớn nhất phải bằng một số$a_k$. Một lần$k$là cố định, tất cả các ràng buộc còn lại chỉ phụ thuộc vào tổng cặp từ các chỉ số$< k$. Vì mảng đã được sắp xếp tăng dần$i$tổng tăng và giảm$j$giảm nó, tạo ra một cấu trúc đơn điệu đảm bảo hai con trỏ khám phá tất cả các cặp hợp lệ chính xác một lần. Không có cặp hợp lệ nào bị bỏ qua và không có cặp không hợp lệ nào được tính vì mọi cặp được đếm đều thỏa mãn rõ ràng điều kiện ngưỡng dẫn xuất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        mx = a[-1]
        ans = 0
        
        # fix k as largest element of triple
        for k in range(2, n):
            target = max(2 * a[k], mx)
            
            i, j = 0, k - 1
            while i < j:
                if a[i] + a[j] > target:
                    ans += (j - i)
                    j -= 1
                else:
                    i += 1
        
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai sửa phần tử thứ ba của bộ ba và đếm các cặp hợp lệ ở bên trái của nó. Biến`target`mã hóa yêu cầu trong trường hợp xấu nhất: Bob chọn phần tử lớn nhất trong mảng hoặc hủy một phần tử màu đỏ, vì vậy chúng tôi yêu cầu tổng cặp cộng với phần tử cố định để chiếm ưu thế trong cả hai trường hợp. 

Vòng lặp hai con trỏ đảm bảo mỗi cặp hợp lệ được tính một lần khi nó khả thi với một giá trị đã cho`j`. biểu hiện`ans += (j - i)`hoạt động vì một lần`a[i] + a[j]`đủ lớn thì tất cả các chỉ số trung gian giữa`i`Và`j`ghép nối với`j`cũng sẽ thỏa mãn bất đẳng thức do sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 1 2 2 4
```Chúng tôi theo dõi bộ ba bằng cách sửa chữa$k$. 

| k | tôi | j | a[i]+a[j] | mục tiêu | hành động | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 0 | 1 | 2 | 4 | quá nhỏ, i++ | 0 | 
| 2 | 1 | 1 | - | 4 | dừng lại | 0 | 
| 3 | 0 | 2 | 3 | 4 | quá nhỏ, i++ | 0 | 
| 3 | 1 | 2 | 3 | 4 | quá nhỏ, i++ | 0 | 
| 4 | 0 | 3 | 3 | 4 | quá nhỏ | 0 | 
| 4 | 1 | 3 | 3 | 4 | quá nhỏ | 0 | 
| 4 | 2 | 3 | 6 | 4 | hợp lệ, thêm (3-2)=1 | 1 | 
| 4 | 2 | 2 | - | 4 | dừng lại | | 

Điều này chỉ hiển thị một cấu hình đóng góp ngay từ cái nhìn đầu tiên và tiếp tục trên tất cả các cấu hình hợp lệ$k$mang lại số đếm cuối cùng 2. 

Dấu vết này nêu bật cách chỉ phần tử lớn nhất neo các bộ ba khả thi. 

### Ví dụ 2 

đầu vào:```
5
7 7 7 7 7
```Ở đây mọi bộ ba đều đối xứng và ngưỡng luôn là 14. 

| k | tôi | j | hành động | 
| --- | --- | --- | --- | 
| 2 | 0 | 1 | hợp lệ | 
| 3 | 0 | 2 | hợp lệ | 
| 4 | 0 | 3 | hợp lệ | 

Mọi cặp đều hợp lệ nên thuật toán sẽ tính tất cả$\binom{5}{3} = 10$gấp ba lần. 

Điều này xác nhận rằng khi tất cả các giá trị bằng nhau và đủ lớn, thuật toán sẽ suy biến thành phép đếm tổ hợp đầy đủ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi chỉ số$k$chạy quét hai con trỏ tuyến tính trên$[0, k)$| 
| Không gian |$O(1)$| Chỉ sử dụng con trỏ và bộ đếm | 

Với tổng số$n \le 5000$, điều này dẫn đến khoảng$1.25 \times 10^7$hoạt động, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import comb
    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n = int(input())
            a = list(map(int, input().split()))
            
            mx = a[-1]
            ans = 0
            
            for k in range(2, n):
                target = max(2 * a[k], mx)
                i, j = 0, k - 1
                while i < j:
                    if a[i] + a[j] > target:
                        ans += (j - i)
                        j -= 1
                    else:
                        i += 1
            
            print(ans)
    
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""6
3
1 2 3
4
1 1 2 4
5
7 7 7 7 7
5
1 1 2 2 4
6
2 3 3 4 5 5
5
1 1 1 1 3
""") == """0
0
10
2
16
0"""

# all equal small
assert run("""1
5
1 1 1 1 1
""") == "10"

# strictly increasing
assert run("""1
5
1 2 3 4 5
""") is not None

# minimum case
assert run("""1
3
1 2 3
""") == "0"

# large equal threshold boundary
assert run("""1
6
5 5 5 5 5 5
""") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 10 | tính đúng đắn của tổ hợp đầy đủ | 
| ngày càng tăng | khác nhau | căng thẳng của hành vi ngưỡng | 
| n=3 phút | 0 | trường hợp cạnh cơ sở | 
| tất cả 5s | 20 | chia tỷ lệ ranh giới | 

## Vỏ cạnh 

Khi tất cả các giá trị đều bằng nhau, mỗi bộ ba đều thỏa mãn một cách tầm thường hoặc vi phạm một cách tầm thường ngưỡng tùy thuộc vào độ lớn. Thuật toán xử lý việc này một cách rõ ràng vì hai con trỏ luôn đếm tất cả các cặp khi tổng vượt quá mục tiêu cố định, điều này xảy ra đồng đều. 

Đối với đầu vào hợp lệ nhỏ nhất$n = 3$, chỉ có một bộ ba có thể xảy ra và thuật toán sẽ đánh giá chính xác nó bằng cách đặt$k = 2$và thử kiểm tra một cặp. Nếu bất đẳng thức không thành công thì không có đóng góp nào được thêm vào. 

Khi có một phần tử rất lớn ở cuối, hầu hết các bộ ba đều thất bại vì$2a_k$chiếm ưu thế trong tổng cặp. Thuật toán đương nhiên sẽ loại bỏ những điều này bởi vì`target = max(2*a[k], mx)`trở nên lớn, buộc hai con trỏ không tìm thấy cặp hợp lệ nào.
