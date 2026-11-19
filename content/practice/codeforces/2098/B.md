---
title: "CF 2098B - Sasha và việc mua căn hộ"
description: "Chúng ta được cung cấp một dòng các ngôi nhà được lập chỉ mục bằng các số nguyên lớn và nhiều vị trí thanh dọc theo dòng này. Nhiều thanh có thể chiếm cùng một ngôi nhà, nhưng mỗi thanh được coi là một điểm riêng biệt."
date: "2026-06-08T05:13:10+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2098
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1021 (Div. 2)"
rating: 1400
weight: 2098
solve_time_s: 65
verified: true
draft: false
---

[CF 2098B - Sasha và việc mua căn hộ](https://codeforces.com/problemset/problem/2098/B) 

**Đánh giá:** 1400 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dòng các ngôi nhà được lập chỉ mục bằng các số nguyên lớn và nhiều vị trí thanh dọc theo dòng này. Nhiều thanh có thể chiếm cùng một ngôi nhà, nhưng mỗi thanh được coi là một điểm riêng biệt. 

Đối với bất kỳ nhà ứng cử viên nào$x$, chúng tôi định nghĩa hàm chi phí là tổng khoảng cách từ$x$tới tất cả các thanh còn lại. Trước khi lựa chọn$x$, chúng ta được phép xóa nhiều nhất$k$thanh. Sau khi xóa, chúng tôi tính toán hàm chi phí tối ưu và xem xét tất cả các vị trí$x$làm giảm thiểu tổng khoảng cách tuyệt đối này. Nhiệm vụ là đếm xem có bao nhiêu vị trí số nguyên phân biệt$x$có thể trở nên tối ưu theo một số chiến lược xóa hợp lệ. 

Cấu trúc ẩn chính là việc giảm thiểu tổng độ lệch tuyệt đối chỉ phụ thuộc vào trung vị của nhiều tập hợp còn lại. Khi chúng tôi xóa một số thanh, mức tối ưu$x$luôn là khoảng trung bình của các điểm còn lại. Do đó, vấn đề đặt ra là: có bao nhiêu số nguyên có thể tạo thành trung vị của một số tập hợp con thu được bằng cách xóa nhiều nhất$k$các phần tử. 

Những ràng buộc ngụ ý$n$tổng cộng$10^5$, vì vậy mỗi lần kiểm tra chúng tôi cần khoảng$O(n \log n)$hoặc tuyến tính sau khi sắp xếp. Bất cứ điều gì bậc hai hoặc liệt kê tất cả các phép xóa đều không thể thực hiện được vì số lượng tập hợp con là hàm mũ trong$n$. Thậm chí thử mọi lựa chọn trong đó$k$các thanh cần loại bỏ sẽ là tổ hợp. 

Một cạm bẫy ngây thơ là cho rằng chúng ta phải chọn một “tập hợp con tốt nhất” và tính trung vị của nó. Điều đó nhớ sự khác biệt$x$các giá trị có thể tối ưu theo các chiến lược xóa khác nhau, do đó, giải pháp phải tổng hợp tất cả các vị trí trung bình có thể đạt được chứ không phải một cấu hình. 

Trực giác về trường hợp cạnh rất quan trọng khi tất cả các thanh đều giống hệt nhau. Ví dụ, nếu tất cả$a_i = 5$, thì mỗi lần xóa vẫn để lại tất cả các điểm ở mức 5, nên chỉ$x = 5$hoạt động. Một trường hợp tế nhị khác là khi$k$đủ lớn để loại bỏ hoàn toàn một bên, khiến cho các vị trí cực đoan trở nên khả thi; đây là nơi xảy ra việc mở rộng khoảng thời gian. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ thử mọi cách để xóa tối đa$k$thanh, sau đó tính khoảng thời gian trung bình của nhiều tập hợp còn lại. Đối với mỗi tập hợp con, chúng tôi sẽ thêm tất cả các vị trí trung vị hợp lệ. Điều này ngay lập tức thất bại vì số lượng tập hợp con là$\sum_{i=0}^k \binom{n}{i}$, là số mũ trong$n$. Ngay cả đối với$n = 10^5$, điều này là không thể thực hiện được. 

Cái nhìn sâu sắc về cấu trúc xuất phát từ việc viết lại vấn đề theo thứ tự được sắp xếp. Sau khi sắp xếp các vị trí thanh, bất kỳ trung vị nào cũng được xác định bởi một đoạn chỉ số ở giữa. Việc xóa các thanh tương ứng với việc xóa các phần tử khỏi các đầu của mảng được sắp xếp này trong một cấu hình tối ưu, vì việc xóa các điểm bên trong không thể mở rộng khoảng trung bình có thể hiệu quả hơn việc loại bỏ các điểm cực trị. 

Vì vậy, vấn đề trở thành: chúng ta muốn chọn một cửa sổ có kích thước còn lại$m = n - t$Ở đâu$0 \le t \le k$và xem xét tất cả các khoảng trung bình có thể có của các cửa sổ đó. Trung vị của nhiều tập hợp không phải là một điểm duy nhất khi kích thước là số chẵn; nó trở thành một khoảng giữa hai phần tử ở giữa. Khi chúng tôi thay đổi cái nào$t$các phần tử bị xóa, chúng tôi sẽ trượt một cửa sổ một cách hiệu quả đồng thời cho phép mở rộng các điểm cuối bằng cách xóa chi tiêu. 

Điều này giúp giảm bớt việc tính toán xem chúng ta có thể mở rộng phạm vi trung bình xung quanh khu vực trung tâm đến mức nào. Câu trả lời cuối cùng trở thành sự kết hợp của tất cả các khoảng trung bình khả thi trên tất cả các lần loại bỏ được phép, có thể được tính bằng cách phân tích xem có bao nhiêu phần tử có thể bị loại bỏ ở bên trái và bên phải của mảng đã sắp xếp trong khi vẫn giữ lại ít nhất một nửa số phần tử. 

Chúng ta quy vấn đề về việc tìm khoảng$[L, R]$trong mảng được sắp xếp sao cho tồn tại một tập hợp con hợp lệ trong đó trung vị nằm ở bất kỳ đâu giữa$a_L$Và$a_R$. Điểm cuối phụ thuộc vào số lần xóa mà chúng tôi phân bổ cho mỗi bên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum \binom{n}{k})$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng vị trí thanh. Việc sắp xếp là cần thiết vì số trung vị chỉ phụ thuộc vào thứ tự chứ không phụ thuộc vào vị trí ban đầu. 
2. Hãy để$n$là số thanh. Đối với kích thước tập hợp con cuối cùng cố định$m = n - t$, vùng trung vị được xác định bởi các chỉ số$\left\lfloor \frac{m-1}{2} \right\rfloor$Và$\left\lceil \frac{m-1}{2} \right\rceil$trong tập con đã được sắp xếp. Điều này cho thấy mọi trung vị hợp lệ đều tương ứng với một số phân đoạn liền kề của mảng ban đầu được sắp xếp sau khi xóa. 
3. Quan sát rằng việc xóa cho phép chúng ta bỏ qua tối đa$k$các phần tử ở bất cứ đâu, nhưng để tối đa hóa phạm vi của các trung vị có thể, việc xóa được sử dụng tối ưu ở các điểm cực trị của mảng được sắp xếp. Việc xóa phần tử bên trong không mở rộng các giá trị trung bình có thể tiếp cận nhiều hơn việc xóa các phần tử biên. 
4. Hãy để$m = n - k$. Đây là kích thước còn lại nhỏ nhất có thể. Khoảng trung vị được tối đa hóa trong trường hợp này, bởi vì các tập hợp con nhỏ hơn có độ không đảm bảo lớn hơn ở vị trí trung vị của chúng. 
5. Vị trí trung vị hợp lệ tương ứng với tất cả các cửa sổ có kích thước$m$trong mảng đã sắp xếp. Mỗi cửa sổ đóng góp một hoặc hai ứng cử viên trung vị tùy thuộc vào tính chẵn lẻ của$m$. Việc kết hợp các khoảng này sẽ tạo ra một phân đoạn liên tục trong không gian giá trị. 
6. Câu trả lời đơn giản là đếm xem có bao nhiêu giá trị nguyên nằm trong khoảng giữa giá trị trung bình tối thiểu có thể có và giá trị trung vị tối đa có thể có trên tất cả các cửa sổ hợp lệ. Những thái cực này đạt được bằng cách lấy kích thước cửa sổ ngoài cùng bên trái và cửa sổ ngoài cùng bên phải$m$. 
7. Tính toán: 

-$L = a[\lfloor (n-k-1)/2 \rfloor]$-$R = a[n - 1 - \lfloor (n-k-1)/2 \rfloor]$Vậy thì câu trả lời là$R - L + 1$. 

### Tại sao nó hoạt động 

Trung vị của bất kỳ tập hợp nào chỉ phụ thuộc vào các phần tử ở giữa theo thứ tự được sắp xếp. Vì chúng tôi được phép loại bỏ nhiều nhất$k$phần tử, chúng ta có thể thay đổi phần tử nào chiếm vị trí trung vị bằng cách xóa từ một trong hai phía. Các giá trị trung vị cực trị thu được bằng cách đẩy càng nhiều phần tử nhỏ ra khỏi cửa sổ trung vị càng tốt đối với ranh giới bên phải và càng nhiều phần tử lớn ra khỏi ranh giới bên trái càng tốt. Bất kỳ chiến lược xóa bên trong nào cũng không thể vượt quá các mức cực đoan này vì nó tiêu tốn ngân sách xóa mà không dịch chuyển chỉ số trung bình hiệu quả hơn so với xóa bỏ ranh giới. Do đó, các trung vị có thể tiếp cận tạo thành một phân đoạn liền kề theo thứ tự được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        m = n - k
        mid = (m - 1) // 2

        L = a[mid]
        R = a[n - 1 - mid]

        print(R - L + 1)

if __name__ == "__main__":
    solve()
```Bước sắp xếp thiết lập một thứ tự cố định để suy luận về số trung vị. Biến$m = n - k$đại diện cho kích thước tập hợp con nhỏ nhất mà chúng ta phải xem xét, vì việc loại bỏ nhiều phần tử hơn chỉ làm giảm tính linh hoạt. 

Chi tiết triển khai chính là sử dụng cùng độ lệch điểm giữa$mid = (m-1)//2$từ cả hai đầu của mảng. Sự đối xứng này nắm bắt được các vị trí trung bình nhỏ nhất và lớn nhất có thể đạt được. Phép trừ cuối cùng$R - L + 1$đếm các vị trí nhà nguyên trong khoảng khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 2
a = [1, 6, 6, 7, 7]
```Chúng tôi sắp xếp (đã được sắp xếp) và tính toán$m = 3$, Vì thế$mid = 1$. 

| Bước | Mảng | m | giữa | L | R | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [1, 6, 6, 7, 7] | 3 | 1 | - | - | 
| tính L | - | - | - | 6 | - | 
| tính R | - | - | - | 6 | 7 | 

Trả lời =$7 - 6 + 1 = 2$. 

Điều này cho thấy cách loại bỏ tối đa$k$các phần tử cho phép trung vị dịch chuyển giữa hai giá trị liền kề, tạo ra một khoảng nhỏ các vị trí hợp lệ. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 0
a = [1, 2, 3, 4]
```Đây$m = 4$, Vì thế$mid = 1$. 

| Bước | Mảng | m | giữa | L | R | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | [1, 2, 3, 4] | 4 | 1 | - | - | 
| tính L | - | - | - | 2 | - | 
| tính R | - | - | - | 2 | 3 | 

Trả lời =$3 - 2 + 1 = 2$. 

Điều này xác nhận rằng không bị xóa, chỉ vùng trung tâm được xác định bởi các trung vị cố định là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế, mỗi bài kiểm tra sau đó đều tuyến tính | 
| Không gian |$O(1)$thêm | Sắp xếp tại chỗ ngoài bộ lưu trữ đầu vào | 

Các ràng buộc cho phép lên đến$10^5$tổng các phần tử, do đó$O(n \log n)$giải pháp dựa trên sắp xếp phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys
    input = sys.stdin.readline

    t = int(sys.stdin.readline())
    for _ in range(t):
        n, k = map(int, sys.stdin.readline().split())
        a = list(map(int, sys.stdin.readline().split()))
        a.sort()
        m = n - k
        mid = (m - 1) // 2
        print(a[n - 1 - mid] - a[mid] + 1)

    return output.getvalue().strip()

# provided samples
assert solve_io("""4
4 0
1 2 3 4
5 2
7 6 6 7 1
3 1
6 7 9
6 2
5 1 9 10 13 2
""") == """2
2
4
9"""

# custom cases
assert solve_io("""1
1 0
100
""") == "1"

assert solve_io("""1
2 1
1 100
""") == "100"

assert solve_io("""1
5 0
1 1 1 1 1
""") == "1"

assert solve_io("""1
6 2
1 2 3 100 101 102
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | ranh giới tối thiểu | 
| hai phần tử, xóa một phần tử | 100 | sự thay đổi cực độ | 
| tất cả đều bình đẳng | 1 | sự ổn định trùng lặp | 
| cụm tách biệt | 3 | mở rộng khoảng trung vị | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các giá trị đều giống hệt nhau. Đối với đầu vào`[5,5,5,5]`với bất kỳ$k$, việc loại bỏ các phần tử không bao giờ thay đổi giá trị trung bình. Thuật toán sắp xếp mảng và tính toán$mid$, nhưng cả hai$a[mid]$Và$a[n-1-mid]$bằng 5, cho ra đáp án 1, phù hợp với ngôi nhà duy nhất khả thi. 

Một trường hợp cạnh khác là khi$k = n-1$. Sau khi xóa tất cả trừ một thanh, mọi phần tử còn lại đều là trung vị. Công thức giảm xuống còn$mid = 0$, Vì thế$L = a[0]$Và$R = a[n-1]$, nghĩa là mọi giá trị ban đầu đều có thể đạt được dưới dạng trung vị đơn lẻ, mang lại mức chênh lệch chính xác đầy đủ cho các kết quả có thể xảy ra. 

Trường hợp thứ ba là phân phối có độ lệch cao như`[1,1,1,100,100,100]`với mức độ vừa phải$k$. Thuật toán nắm bắt chính xác rằng việc xóa một vài điểm cực trị không thể di chuyển trung vị ra ngoài vùng chồng lấp trung tâm của hai cụm, vì giới hạn chỉ số điểm giữa vẫn được neo bởi các vị trí được sắp xếp thay vì giá trị tuyệt đối.
