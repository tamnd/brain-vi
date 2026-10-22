---
title: "CF 105887A - \u9898\u76ee\u80cc\u666f\u662f GPT \u7ed9\u7684"
description: "Chúng ta có một mảng hình tròn có độ dài $n$, trong đó mỗi vị trí chứa một giá trị nguyên biểu thị một số mặt nạ bit. Mảng phát triển theo từng vòng riêng biệt."
date: "2026-06-21T15:05:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105887
codeforces_index: "A"
codeforces_contest_name: "\u7b2c\u5341\u4e09\u5c4a\u91cd\u5e86\u5e02\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b"
rating: 0
weight: 105887
solve_time_s: 58
verified: true
draft: false
---

[CF 105887A - \u9898\u76ee\u80cc\u666f\u662f GPT \u7ed9\u7684](https://codeforces.com/problemset/problem/105887/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng hình tròn có chiều dài$n$, trong đó mỗi vị trí giữ một giá trị số nguyên đại diện cho một số mặt nạ bit. Mảng phát triển theo từng vòng riêng biệt. trong$i$-vòng thứ, mọi vị trí$j$đồng thời cập nhật giá trị của nó bằng cách lấy một bit HOẶC với một vị trí khác được dịch chuyển bởi$i$bước về phía trước trên vòng tròn. Về mặt hình thức, mỗi$A_j$trở thành$A_j \,\mathrm{OR}\, A_{(j+i)\bmod n}$. 

Tất cả các cập nhật trong một vòng đều diễn ra đồng thời và các vòng sau sẽ hoạt động trên mảng đã được cập nhật. 

Sau khi thực hiện một số vòng như vậy theo thứ tự, các giá trị bắt đầu lan truyền xung quanh chu kỳ thông qua việc hợp nhất OR lặp đi lặp lại của các độ lệch khác nhau. Quá trình này dần dần lan truyền thông tin (bit) trên các vị trí. Nhiệm vụ là xác định số vòng nhỏ nhất$T$sau đó tất cả các phần tử trong mảng trở nên bằng nhau. 

Đầu ra không phải là mảng kết quả mà chỉ có số vòng tối thiểu cần thiết cho đến khi mọi vị trí đều chứa cùng một giá trị. 

Những ràng buộc cho phép$n \le 10^5$, điều này ngay lập tức loại trừ mọi mô phỏng thực hiện rõ ràng$O(n)$làm việc mỗi vòng nếu có thể$T$, vì thậm chí$O(n^2)$tổng hành vi sẽ là quá chậm. Bất kỳ giải pháp đúng nào cũng phải giảm quy trình về dạng tính toán đóng hoặc gần như không đổi cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi$n$là rất nhỏ. Ví dụ, nếu$n = 2$, một vòng duy nhất đã buộc cả hai vị trí HOẶC lẫn nhau, làm cho chúng bằng nhau ngay lập tức. Vì$n = 3$, hành vi khác nhau: các giá trị lan truyền chậm hơn vì mỗi vòng chỉ giới thiệu một kích thước bước cố định và việc chồng chéo yêu cầu nhiều vòng. Điều này cho thấy câu trả lời phụ thuộc vào tốc độ truyền bá thông tin trong chu kỳ chứ không phụ thuộc vào bản thân các giá trị. 

Một sai lầm phổ biến khác là cho rằng vì OR là lũy đẳng và giao hoán nên một vòng đơn hoặc một số vòng cố định nhỏ là đủ. Điều đó thất bại khi$n$lớn và khoảng cách truyền bị giới hạn trong mỗi vòng. 

## Phương pháp tiếp cận 

Một cách giải thích ngây thơ là mô phỏng quá trình theo nghĩa đen. Ở mỗi vòng$i$, chúng ta tính toán một mảng mới bằng cách lặp qua tất cả các vị trí$j$, lấy$A_j$HOẶC$A_{j+i}$, với chỉ số modulo$n$. Lặp đi lặp lại điều này cho$T$chi phí vòng$O(nT)$. Trong trường hợp xấu nhất, nếu$T$theo thứ tự của$n$, điều này trở thành$O(n^2)$, vượt xa giới hạn chấp nhận được đối với$n = 10^5$. 

Quan sát quan trọng là các giá trị của mảng không ảnh hưởng đến cấu trúc của quy trình. Chỉ có việc truyền bá “khả năng tiếp cận” mới quan trọng: khi một giá trị từ vị trí$x$đạt đến vị trí$y$, nó có thể đóng góp vĩnh viễn tất cả các bit của nó ở đó do tính chất đơn điệu của OR. 

Vì vậy, thay vì theo dõi các giá trị, chúng tôi theo dõi vị trí nào ảnh hưởng đến vị trí nào khác theo sau$T$vòng. 

Sau khi suy nghĩ về mặt truyền bá, quá trình này trở thành một vấn đề về khả năng tiếp cận đồ thị trên một chu kỳ. trong vòng$i$, chúng tôi thêm các cạnh có chiều dài một cách hiệu quả$i$. Sau đó$T$vòng, bất kỳ vị trí nào cũng có thể ảnh hưởng đến các vị trí có thể tiếp cận bằng tổng kích thước bước được chọn từ$1$ĐẾN$T$, bởi vì các vòng lặp lại tạo nên những ca này. 

Vì vậy, tập hợp các khoảng cách có thể tới được là tổng các phần tử từ$\{1,2,\dots,T\}$, nghĩa là bất kỳ khoảng cách nào lên đến$$1 + 2 + \dots + T = \frac{T(T+1)}{2}.$$Vì vậy sau$T$vòng, mỗi vị trí có thể thu thập thông tin tối đa từ tất cả các vị trí trong khoảng cách$\frac{T(T+1)}{2}$dọc theo chu kỳ. Khi khoảng này bao trùm toàn bộ chu trình, mọi vị trí sẽ nhận được sự đóng góp từ tất cả những vị trí khác và tất cả các giá trị trở nên giống hệt với OR chung của mảng. 

Vậy bài toán quy về việc tìm giá trị nhỏ nhất$T$như vậy:$$\frac{T(T+1)}{2} \ge n - 1.$$Đây là một ngưỡng bậc hai đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng |$O(nT)$|$O(n)$| Quá chậm | 
| Công thức khả năng tiếp cận bậc hai |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tính toán số vòng tối thiểu cần thiết để khoảng cách truyền bao phủ toàn bộ chu kỳ. 

1. Đọc$n$cho từng trường hợp thử nghiệm. Các giá trị mảng không liên quan đến câu trả lời cuối cùng vì OR chỉ tích lũy thông tin và không ảnh hưởng đến tốc độ lan truyền. 
2. Giải thích quá trình này như bài toán bán kính trải rộng trên một chu trình. Sau đó$T$vòng, thông tin có thể di chuyển một khoảng cách tối đa bằng tổng kích thước bước được giới thiệu trong mỗi vòng. 
3. Tính số tam giác$S(T) = \frac{T(T+1)}{2}$, đại diện cho khoảng cách xa nhất có thể đạt được sau$T$vòng. 
4. Tìm số nhỏ nhất$T$như vậy$S(T) \ge n-1$. Điều này đảm bảo rằng mọi vị trí có thể tiếp cận mọi vị trí khác trong thời gian tối đa$T$vòng. 
5. Giải bất đẳng thức trực tiếp bằng công thức bậc hai. Chúng tôi cần:$$T^2 + T - 2(n-1) \ge 0.$$Căn dương cho:$$T = \left\lceil \frac{-1 + \sqrt{1 + 8(n-1)}}{2} \right\rceil.$$6. Xuất giá trị này cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Quá trình này chỉ mở rộng thông tin thông qua các dịch chuyển theo chu kỳ và mỗi vòng sẽ thêm một kích thước bước cố định mới vào tập hợp các chuyển động có thể có. Thành phần của các vòng tương ứng với việc tính tổng các kích thước bước này. Bởi vì OR là đơn điệu và không thể đảo ngược, nên khi một vị trí nhận được một bit, nó sẽ không bao giờ mất đi, do đó yếu tố hạn chế duy nhất là liệu đồ thị của các dịch chuyển có thể tiếp cận có được kết nối hoàn toàn hay không. Giới hạn hình tam giác đặc trưng chính xác khi phạm vi dịch chuyển tích lũy bao trùm toàn bộ chu kỳ, đảm bảo rằng mọi nút đều nhận được sự đóng góp từ tất cả các nút khác, buộc phải có sự bình đẳng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        # We only need n
        if n <= 1:
            print(0)
            continue
        
        target = n - 1
        disc = 1 + 8 * target
        root = int(math.isqrt(disc))
        if root * root < disc:
            root += 1
        
        T = (-1 + root) // 2
        if T * (T + 1) // 2 < target:
            T += 1
        
        print(T)

if __name__ == "__main__":
    solve()
```Việc triển khai hoàn toàn bỏ qua các giá trị mảng vì thời gian đồng bộ hóa cuối cùng chỉ phụ thuộc vào tốc độ các vị trí có thể trao đổi thông tin chứ không phụ thuộc vào thông tin chúng mang theo. 

Việc tính toán sử dụng căn bậc hai số nguyên để tránh các vấn đề về độ chính xác của dấu phẩy động. Sau khi ước tính$T$từ công thức bậc hai, chúng ta sửa nó nếu cần để đảm bảo điều kiện tam giác được thỏa mãn chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2
```Sau một vòng, mỗi vị trí HOẶC với vị trí còn lại. Cả hai trở nên giống hệt nhau ngay lập tức. 

| T | T(T+1)/2 | Tình trạng | 
| --- | --- | --- | 
| 0 | 0 | không | 
| 1 | 1 | vâng | 

Đầu ra là 1. 

Điều này cho thấy trường hợp lan truyền tối thiểu trong đó một cạnh đã kết nối toàn bộ chu trình. 

### Ví dụ 2 

đầu vào:```
n = 7
```Chúng tôi tính toán phạm vi bảo hiểm cần thiết$n-1 = 6$. 

| T | T(T+1)/2 | Tình trạng | 
| --- | --- | --- | 
| 1 | 1 | không | 
| 2 | 3 | không | 
| 3 | 6 | vâng | 

Vậy đáp án là 3. 

Điều này chứng tỏ mức độ lan truyền được tích lũy dần dần và sự bình đẳng chỉ đạt được khi phạm vi tiếp cận tích lũy kéo dài toàn bộ chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$mỗi trường hợp thử nghiệm | Chỉ sử dụng căn bậc hai số học và số nguyên | 
| Không gian |$O(1)$| Không có công trình phụ trợ nào tỷ lệ thuận với$n$| 

Giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với$n = 10^5$và nhiều trường hợp thử nghiệm, vì mỗi trường hợp được giải quyết trong thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        
        if n <= 1:
            out.append("0")
            continue
        
        target = n - 1
        disc = 1 + 8 * target
        root = int(math.isqrt(disc))
        if root * root < disc:
            root += 1
        
        T = (-1 + root) // 2
        if T * (T + 1) // 2 < target:
            T += 1
        
        out.append(str(T))
    
    return "\n".join(out)

# provided samples (illustrative placeholders)
assert solve("1\n2\n1 2\n") == "1", "sample 1"
assert solve("1\n7\n6 5 4 3 2 1\n") == "3", "sample 2"

# custom cases
assert solve("1\n2\n5 7\n") == "1", "min size cycle"
assert solve("1\n3\n1 2 3\n") == "2", "small propagation"
assert solve("1\n6\n1 1 1 1 1 1\n") == "3", "uniform values"
assert solve("1\n10\n0 0 0 0 0 0 0 0 0 0\n") == "4", "larger chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=2 | 1 | hành vi chu kỳ nhỏ nhất | 
| n=3 | 2 | truyền chậm trên vòng nhỏ | 
| tất cả các mảng bằng nhau | đúng T độc lập với các giá trị | giá trị không liên quan | 
| n=10 | 4 | tăng trưởng bậc hai của phạm vi tiếp cận | 

## Vỏ cạnh 

cho$n = 2$, biểu đồ lan truyền đã được kết nối đầy đủ sau một vòng. Thuật toán tính toán$n-1 = 1$, và điều kiện$T(T+1)/2 \ge 1$sản lượng$T = 1$, phù hợp với mô phỏng trực tiếp. 

Vì$n = 3$, chúng tôi cần$n-1 = 2$. Số tam giác nhỏ nhất bằng 2 là$3$, tương ứng với$T = 2$. Điều này phản ánh rằng một vòng chỉ cung cấp khả năng lân cận, trong khi việc trộn hoàn toàn cần có lớp lan truyền thứ hai. 

Đối với lớn$n$, chẳng hạn như$n = 10^5$, được tính toán$T$ở xung quanh$\sqrt{2n}$, vẫn còn nhỏ so với$n$, đảm bảo thuật toán luôn hoạt động hiệu quả và tránh mọi cạm bẫy mô phỏng tuyến tính.
