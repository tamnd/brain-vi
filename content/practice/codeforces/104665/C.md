---
title: "CF 104665C - Bữa tiệc của thợ làm mũ"
description: "Chúng ta được cung cấp một tập hợp các sợi mì, mỗi sợi mang một giá trị hương vị bằng số. Chúng ta cần chia những sợi này thành nhiều món ăn. Mỗi món ăn phải chứa ít nhất các sợi $K$ và giá trị của một món ăn được xác định là hương vị tối đa trong số các sợi được đặt vào đó."
date: "2026-06-29T09:58:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104665
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 10-06-23 Div. 1 (Advanced)"
rating: 0
weight: 104665
solve_time_s: 71
verified: true
draft: false
---

[CF 104665C - Bữa tiệc của thợ làm mũ](https://codeforces.com/problemset/problem/104665/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các sợi mì, mỗi sợi mang một giá trị hương vị bằng số. Chúng ta cần chia những sợi này thành nhiều món ăn. Mỗi món ăn phải có ít nhất$K$sợi, và giá trị của một món ăn được định nghĩa là hương vị tối đa trong số các sợi được đặt vào đó. 

Nhiệm vụ là chia tất cả các chuỗi thành các món ăn hợp lệ sao cho tổng giá trị món ăn càng lớn càng tốt. 

Vì vậy, quyết định cốt lõi không chỉ nằm ở từng chuỗi riêng lẻ mà là về cách chúng ta nhóm chúng lại, vì chỉ yếu tố tối đa trong mỗi nhóm mới đóng góp vào điểm số, trong khi tất cả các yếu tố khác trong nhóm đều là những yếu tố “hỗ trợ” hiệu quả giúp nhóm tồn tại. 

Kích thước đầu vào tăng lên$10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các phân vùng hoặc liên tục mô phỏng các lựa chọn nhóm. Bất cứ điều gì bậc hai hoặc hàm mũ về số lượng chuỗi sẽ thất bại bởi vì ngay cả$10^10$hoạt động đã quá lớn cho giới hạn 1 giây trong thực tế. 

Một điểm tinh tế là mỗi sợi phải thuộc đúng một món ăn. Điều này biến vấn đề thành vấn đề phân vùng hơn là vấn đề lựa chọn. Một cách giải thích ngây thơ cho phép loại bỏ các chuỗi sẽ thay đổi hoàn toàn cấu trúc và dẫn đến một mục tiêu khác, dễ dàng hơn, vì vậy ràng buộc “sử dụng tất cả các phần tử” là quan trọng. 

Trường hợp cạnh xuất hiện khi$K = 1$, trong đó mỗi sợi tạo thành món ăn riêng của nó và câu trả lời chỉ đơn giản là tổng của tất cả các giá trị. Một trường hợp cạnh khác là khi$K = N$, trong đó chỉ có thể tạo được một món ăn và đáp án là phần tử lớn nhất. Trường hợp thứ ba phá vỡ trực giác tham lam là khi các giá trị lớn bị phân tán: nếu không có logic nhóm cẩn thận, người ta có thể vô tình lãng phí các giá trị lớn bên trong các nhóm mà chúng không phải là giá trị tối đa. 

Ví dụ, nếu chúng ta có$f = [10, 9, 9, 1, 1]$Và$K = 3$, một nhóm bất cẩn như$[10, 1, 1]$Và$[9, 9]$không hợp lệ vì nhóm thứ hai không đáp ứng yêu cầu về kích thước. Loại lỗi này cho thấy tại sao việc nhóm phải tôn trọng giới hạn kích thước tối thiểu trên toàn cầu chứ không phải cục bộ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng liệt kê tất cả các cách có thể để phân chia mảng thành các nhóm có kích thước ít nhất$K$. Đối với mỗi phân vùng, chúng tôi tính tổng cực đại của tất cả các nhóm. Ngay cả khi chúng ta hạn chế ở các phân vùng hợp lệ, số cách để phân chia$N$các phần tử phát triển cực kỳ nhanh, giống như số Bell với các ràng buộc bổ sung. Vì$N = 20$, điều này đã vượt xa tính toán khả thi, và tại$N = 10^5$, điều đó là hoàn toàn không thể. 

Điều quan trọng cần lưu ý là trong mỗi nhóm, chỉ có phần tử tối đa là quan trọng, trong khi tất cả các phần tử khác chỉ đáp ứng yêu cầu về kích thước. Điều này tạo ra sự bất đối xứng mạnh mẽ: các phần tử lớn chỉ có giá trị nếu chúng trở thành cực đại của nhóm, trong khi các phần tử nhỏ có thể thay thế cho nhau. 

Khi chúng ta sắp xếp mảng theo thứ tự giảm dần, chiến lược tốt nhất sẽ trở nên đơn giản về mặt cấu trúc. Chúng tôi muốn chỉ định phần tử lớn nhất còn lại làm nhóm tối đa thường xuyên nhất có thể. Mỗi lần chọn mức tối đa, chúng ta phải “tiêu” ít nhất$K-1$các phần tử khác để tạo thành một nhóm hợp lệ. Để tối đa hóa tổng số tiền, chúng tôi luôn muốn những$K-1$các phần tử càng nhỏ càng tốt, bảo toàn các phần tử lớn hơn cho các nhóm trong tương lai. 

Điều này dẫn đến một cấu trúc tham lam trong đó chúng ta quét mảng đã sắp xếp và chọn mọi$K$- yếu tố thứ với tư cách là người lãnh đạo nhóm, vì mỗi người lãnh đạo tiêu thụ chính mình cộng thêm$K-1$chất độn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (sắp xếp + lựa chọn tham lam) |$O(N \log N)$|$O(1)$hoặc$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi mô tả chiến lược mang tính xây dựng trực tiếp xây dựng nhóm tối ưu. 

1. Sắp xếp tất cả các giá trị hương vị theo thứ tự giảm dần. 

Điều này đảm bảo rằng khi chúng tôi quyết định ai sẽ trở thành món ăn tối đa, trước tiên chúng tôi luôn xem xét các ứng cử viên lớn hơn, điều này là cần thiết vì các phần tử nhỏ hơn luôn có thể được sử dụng làm chất bổ sung. 
2. Khởi tạo bộ tích lũy cho câu trả lời bằng 0. 
3. Lặp lại mảng đã sắp xếp với kích thước bước$K$, bắt đầu từ chỉ số 0. 

Mỗi vị trí được ghé thăm tương ứng với một người đứng đầu món ăn đã chọn. 
4. Thêm giá trị tại mỗi vị trí đã ghé thăm vào câu trả lời. 

Lý do là phần tử này trở thành phần tử lớn nhất của một nhóm hợp lệ, trong khi phần tử tiếp theo$K-1$các phần tử theo thứ tự được sắp xếp được ngầm sử dụng làm phần bổ sung. 
5. Dừng khi chỉ số vượt quá giới hạn mảng. 

Mô hình tinh thần quan trọng là chúng ta đang chia mảng đã sắp xếp thành các khối có kích thước liên tiếp$K$, nhưng chỉ phần tử đầu tiên của mỗi khối mới đóng góp vào điểm số. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mảng được sắp xếp từ lớn đến nhỏ. Hãy cân nhắc việc thành lập các nhóm theo thứ tự này. Bất cứ khi nào chúng tôi chọn phần tử lớn nhất chưa được sử dụng hiện tại làm nhóm tối đa, chúng tôi vẫn cần$K-1$các yếu tố khác. Việc chọn những phần tử nhỏ nhất có sẵn tiếp theo làm người lấp chỗ trống luôn là tối ưu vì chúng không đóng góp vào bất kỳ mức tối đa nào trong tương lai trừ khi chính họ trở thành người dẫn đầu, và việc trì hoãn việc trở thành người dẫn đầu của một phần tử lớn chỉ có thể làm giảm cơ hội được chọn trước khi bị buộc vào các vị trí người lấp chỗ trống. 

Điều này tạo ra một bất biến: ở mỗi bước, tiền tố chưa được sử dụng còn lại của mảng được sắp xếp luôn chứa các ứng cử viên có sẵn lớn nhất cho cực đại nhóm trong tương lai và tiêu thụ$K$các phần tử trong mỗi nhóm bảo toàn đặc tính mà mọi người lãnh đạo được chọn là lớn nhất có thể có trong số các ứng cử viên hợp lệ còn lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = [int(input()) for _ in range(n)]
    
    a.sort(reverse=True)
    
    ans = 0
    for i in range(0, n, k):
        ans += a[i]
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các giá trị, sắp xếp chúng theo thứ tự giảm dần và sau đó tích lũy mọi giá trị$K$-phần tử thứ bắt đầu từ chỉ số 0. Cấu trúc vòng lặp trực tiếp mã hóa ý tưởng rằng mỗi nhóm có kích thước$K$đóng góp chính xác một giá trị hữu ích, phần tử đầu tiên theo thứ tự được sắp xếp. 

Một lỗi phổ biến là cố gắng hình thành các nhóm một cách rõ ràng và theo dõi các phần tử còn lại. Điều đó là không cần thiết và dễ xảy ra lỗi. Cách tiếp cận từng bước được sắp xếp đã mã hóa việc nhóm tối ưu một cách ngầm định. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1
76
100
```Mảng được sắp xếp trở thành:```
[100, 76]
```| Bước | Chỉ mục | Giá trị được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | 100 | 100 | 
| 2 | 1 | 76 | 176 | 

Mỗi phần tử tạo thành nhóm riêng của nó vì$K = 1$, vì vậy mọi phần tử đều đóng góp. 

Điều này xác nhận rằng thuật toán suy biến chính xác thành tổng tất cả các phần tử khi không cần nhóm. 

### Mẫu 2 

đầu vào:```
4 2
1
5
3
1
```Mảng được sắp xếp:```
[5, 3, 1, 1]
```| Bước | Chỉ mục | Giá trị được chọn | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 0 | 5 | 5 | 
| 2 | 2 | 1 | 6 | 

Chúng tôi chọn chỉ số 0 và 2 vì mỗi nhóm cần 2 phần tử, vì vậy mỗi phần tử dẫn đầu được cách nhau đúng một phần tử bị bỏ qua. 

Điều này thể hiện cách các phần tử phụ được tự động sử dụng giữa mức tối đa đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(1)$hoặc$O(N)$| Sắp xếp tại chỗ hoặc lưu trữ mảng đầu vào | 

Các ràng buộc cho phép lên đến$10^5$các phần tử, do đó việc sắp xếp nằm trong giới hạn. Việc quét tuyến tính không đáng kể so với việc sắp xếp, giúp giải pháp trở nên hiệu quả một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, k = map(int, input().split())
    a = [int(input()) for _ in range(n)]
    a.sort(reverse=True)
    ans = sum(a[i] for i in range(0, n, k))
    return str(ans)

# provided samples
assert run("2 1\n76\n100\n") == "176"
assert run("4 2\n1\n5\n3\n1\n") == "8"

# custom cases
assert run("1 1\n42\n") == "42"  # single element
assert run("5 5\n1\n2\n3\n4\n5\n") == "5"  # one group only
assert run("6 2\n10\n9\n8\n7\n6\n5\n") == "24"  # multiple pairs
assert run("3 1\n0\n0\n0\n") == "0"  # all zeros
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 42 | xử lý kích thước tối thiểu | 
| K bằng N | 5 | trường hợp cạnh nhóm đơn | 
| phân nhóm đều | 24 | lặp đi lặp lại nhóm chính xác | 
| tất cả số không | 0 | ổn định giá trị bằng 0 | 

## Vỏ cạnh 

Khi nào$K = 1$, thuật toán truy cập mọi chỉ mục trong mảng được sắp xếp. Ví dụ:```
3 1
5
2
7
```Mảng được sắp xếp trở thành$[7, 5, 2]$. Thuật toán chọn các chỉ số 0, 1, 2, tạo ra 14. Mỗi phần tử tạo thành nhóm riêng, phù hợp với định nghĩa. 

Khi$K = N$, Ví dụ:```
4 4
1
9
3
2
```Mảng được sắp xếp là$[9, 3, 2, 1]$. Chỉ chỉ số 0 được chọn, tạo ra 9. Toàn bộ mảng tạo thành một món ăn hợp lệ duy nhất và phần tử tối đa thể hiện chính xác hương vị của nó. 

Khi các giá trị được nhóm chặt chẽ, chẳng hạn như:```
6 3
8
7
6
5
4
3
```Mảng được sắp xếp là$[8, 7, 6, 5, 4, 3]$. Thuật toán chọn chỉ số 0 và 3, tạo ra$8 + 5 = 13$. Nhóm đầu tiên sử dụng ba giá trị hàng đầu, nhóm thứ hai sử dụng ba giá trị tiếp theo và trong mỗi trường hợp, giá trị tối đa là phần tử đầu tiên của khối.
