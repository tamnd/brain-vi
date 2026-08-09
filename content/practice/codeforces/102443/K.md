---
title: "CF 102443K - Xoay gần như sắp xếp"
description: "Chúng ta có một lưới (n lần n) các số tùy ý. Chúng tôi không được cung cấp những con số. Thay vào đó, chúng ta phải in một chương trình cố định sẽ hoạt động chính xác cho mọi lưới ban đầu có thể. Một lệnh chương trình so sánh hai ô."
date: "2026-08-09T14:02:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102443
codeforces_index: "K"
codeforces_contest_name: "2019-2020 Russia Team Open, High School Programming Contest (VKOSHP 19)"
rating: 0
weight: 102443
solve_time_s: 897
verified: true
draft: false
---

[CF 102443K - RotationAlmostSort](https://codeforces.com/problemset/problem/102443/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (n\times n) các số tùy ý. Chúng tôi không được cung cấp những con số. Thay vào đó, chúng ta phải in một chương trình cố định sẽ hoạt động chính xác cho mọi lưới ban đầu có thể. 

Một lệnh chương trình so sánh hai ô. Nếu giá trị đầu tiên lớn hơn, nó sẽ quay một khối (2\times2) được chỉ định ngược chiều kim đồng hồ. Nếu so sánh sai thì không có gì xảy ra. Mọi ô được lệnh đề cập phải tồn tại và góc trên bên trái của khối được xoay không được ở hàng cuối cùng hoặc cột cuối cùng. 

Sau khi toàn bộ chương trình kết thúc, chỉ có hàng (3) đến (n) quan trọng. Chúng được đọc từng hàng, từ trái sang phải và toàn bộ chuỗi này phải không giảm. Hàng (1) và (2) là không gian làm việc tạm thời một cách hiệu quả. 

Đầu vào duy nhất là (n), với (2\le n\le9). Giới hạn trên rất nhỏ nên nhiệm vụ không phải là xử lý mảng đầu vào một cách hiệu quả. Đó là một vấn đề về xây dựng: chúng ta có thể đủ khả năng chi trả cho hàng nghìn hoặc thậm chí hàng chục nghìn hướng dẫn được tạo ra. Hạn chế cứng là giới hạn đầu ra của (100.000) lệnh. Một cấu trúc với các lệnh (O(n^3)) dễ dàng đủ nhỏ khi (n\le9). Giới hạn một giây cũng có nghĩa là chúng ta nên tạo ra câu trả lời một cách trực tiếp thay vì thực hiện bất kỳ tìm kiếm tốn kém nào trên các chương trình khả thi. Vấn đề ban đầu xác nhận các giới hạn này và giới hạn lệnh (100.000). 

Trường hợp cạnh đầu tiên là (n=2). Không có hàng nào từ (3) đến (n), do đó chuỗi bắt buộc có độ dài bằng 0 và được sắp xếp tự động. Việc triển khai bất cẩn vẫn có thể cố gắng thực thi hàng truy cập và xây dựng chung (n-2=0), tạo ra các ô không hợp lệ. Đối với đầu vào`2`, chương trình đúng thậm chí có thể trống. Thay vào đó, mẫu sử dụng ba lệnh hợp lệ và cũng an toàn. 

Trường hợp cạnh thứ hai là khối (2\times2) chứa các giá trị bằng nhau. Một cách xây dựng dựa trên những so sánh chặt chẽ thông thường không được cho rằng một số so sánh luôn luôn đúng. Ví dụ, nếu khối là 

[ 
\bắt đầu{ma trận} 
5&5\ 
5&5 
\end{ma trận}, 
] 

cả ba so sánh trong nguyên hàm trích xuất tối đa của chúng tôi đều sai. Điều đó không sao cả vì mọi vị trí đều có mức tối đa như nhau. Việc triển khai xoay vòng vô điều kiện sẽ không chính xác vì nguyên thủy được cho là chỉ phụ thuộc vào so sánh. 

Trường hợp cạnh thứ ba là mức tối đa nằm trên ranh giới của hình chữ nhật đang hoạt động. Ví dụ, khi hình chữ nhật hiện hành kết thúc ở cột (n), chúng ta không bao giờ được tạo một lệnh có hình vuông được xoay bắt đầu ở cột (n). Cấu trúc của chúng tôi chỉ sử dụng tối đa các cột phía trên bên trái (n-1) và tọa độ hàng của nó nhiều nhất là (n-1). Việc cẩn thận hơn xung quanh cột cuối cùng là một lý do khiến quá trình quét đi từ (n-1) xuống (1). 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ tự nhiên là mô phỏng thuật toán sắp xếp trên các vị trí tượng trưng và cố gắng khám phá một chuỗi các phép quay thực hiện các phép hoán đổi thông thường. Cách tiếp cận đó khó khăn một cách không cần thiết vì một phép quay sẽ thay đổi bốn ô cùng một lúc và chương trình phải hoạt động mà không biết các giá trị. Tìm kiếm trên các phép quay có thể cũng tăng theo cấp số nhân với số lượng hướng dẫn. 

Có một cách đơn giản hơn nhiều để xem lệnh. Ba phép quay có điều kiện là đủ để di chuyển tối đa khối (2\times2) tới bất kỳ góc nào đã chọn. Một khi chúng ta có được nguyên hàm đó, bài toán hai chiều sẽ trở thành một quá trình giống như sắp xếp lựa chọn. Đây là quan sát trung tâm đằng sau việc xây dựng. 

Xét khối (2\times2) 

[ 
\bắt đầu{ma trận} 
A&B\ 
C&D 
\end{ma trận} 
] 

và giả sử chúng ta muốn nó đạt cực đại tại (A). Thực hiện so sánh với (A) theo thứ tự (C), (D), (B). Bất cứ khi nào giá trị so sánh lớn hơn giá trị hiện tại tại (A), hãy xoay ngược chiều kim đồng hồ. 

Nếu (C>A), sau khi quay thì (C) trước sẽ chuyển sang (A). Lập luận tương tự áp dụng cho (D) và sau đó cho (B). Do đó, sau ba lệnh, (A) chứa giá trị tối đa của cả bốn giá trị. Nếu dòng điện (A) đã cực đại thì không có phép quay nào xảy ra. 

Ý tưởng tương tự cũng áp dụng cho ba góc còn lại bằng cách thay đổi góc bắt đầu trong chu trình so sánh. Cụ thể, chúng ta sẽ sử dụng một nguyên hàm đặt giá trị tối đa ở góc dưới bên trái và một nguyên hàm khác đặt giá trị tối đa ở góc dưới bên phải. 

Nguyên thủy phía dưới bên trái sau đó được sử dụng làm quét. Đối với (i cố định), việc quét tất cả các khối (2\times2) có các hàng phía trên bên trái (1,\ldots,i-2) và các cột (n-1,\ldots,1) đẩy tối đa các hàng (1,\ldots,i-1) thành hàng (i-1). Sau đó, chúng ta có thể quét theo chiều ngang qua các hàng (i-1) và (i) để đặt tối đa các ô hiện hoạt còn lại vào vị trí đã chọn của hàng (i). 

Lặp lại quá trình đó từ phải sang trái sẽ làm đầy hàng (i) với các giá trị (n) lớn nhất của các hàng hiện hoạt, theo thứ tự tăng dần. Sau đó chúng tôi giảm (i), giữ nguyên hàng đã được sắp xếp. Về cơ bản, đây là cách sắp xếp lựa chọn được thực hiện bằng cách sử dụng nguyên hàm trích xuất tối đa. 

Ý tưởng brute-force thất bại vì nó coi các phép quay riêng lẻ là thao tác cơ bản. Quan sát rằng một chuỗi các phép quay có kích thước không đổi thực hiện một nguyên hàm so sánh hữu ích cho phép chúng ta suy luận về toàn bộ hình chữ nhật thay vì chuyển động của từng ô riêng lẻ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ theo độ dài chương trình | Có khả năng theo cấp số nhân | Quá chậm và khó xây dựng | 
| Thi công khai thác tối đa | (O(n^3)) lệnh được tạo | (O(1)) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định nguyên thủy`work(x, y, c)`cho khối (2\times2) có góc trên bên trái là ((x,y)). Bốn vị trí được di chuyển theo chu kỳ xung quanh khối. Chúng tôi đưa ra ba so sánh với góc đã chọn (c). Nếu tìm thấy giá trị lớn hơn, khối sẽ được quay ngược chiều kim đồng hồ. Sau ba lần so sánh, góc được chọn chứa tối đa bốn ô. 

Lý do điều này có hiệu quả là sau mỗi lần xoay thành công, giá trị lớn hơn được so sánh sẽ di chuyển vào góc đã chọn. Sau đó, so sánh tiếp theo sẽ so sánh với giá trị tốt nhất được thấy cho đến nay. 
2. Xử lý các hàng theo thứ tự (i=n,n-1,\ldots,3). Khi bắt đầu lặp lại (i), các hàng bên dưới (i) đã được sửa và không bao giờ được chạm vào nữa. 
3. Đối với mỗi cột mong muốn (j=n,n-1,\ldots,1), quét liên tục hình chữ nhật gồm các hàng (1) đến (i-1). Nguyên hàm tối đa phía dưới bên trái được áp dụng cho mọi hàng phía trên bên trái từ (1) đến (i-2) và mọi cột từ (n-1) xuống đến (1). 

Quá trình quét này di chuyển giá trị lớn nhất trong các hàng (1,\ldots,i-1) vào hàng (i-1). Việc lặp lại quá trình quét trước mỗi lựa chọn sẽ mang lại cho chúng ta mức tối đa mới trong số các giá trị chưa được đặt vào hàng (i). 
4. Bắt đầu từ cột (1), áp dụng nguyên hàm tối đa phía dưới bên phải thông qua các cột (1,\ldots,j-1) trên các hàng (i-1) và (i). Giá trị tối đa của tiền tố hai hàng này kết thúc ở vị trí ((i,j)). 

Lúc này, giá trị đặt ở cột (j) là giá trị lớn nhất vẫn có thể chiếm giữ vị trí đó. Vì (j) giảm từ (n) xuống (1), giá trị được chọn lớn nhất sẽ di chuyển về bên phải xa nhất và các giá trị được chọn nhỏ dần sẽ di chuyển về bên trái. 
5. Sau lần lặp (j=1), một giá trị vẫn nằm ở hàng (i-1) chứ không phải ở hàng (i). Chuỗi lệnh ngắn cuối cùng chuẩn hóa khối (2\times2) trong cột (1) và (2), trích xuất mức tối đa cần thiết từ các hàng ở trên và hoàn tất việc chèn vào cột (1). 
6. Khi quá trình lặp (i) kết thúc, hàng (i) chứa (n) giá trị lớn nhất trong số các hàng (1,\ldots,i), được sắp xếp tăng dần. Mọi giá trị còn lại phía trên nó không lớn hơn giá trị đầu tiên của hàng (i). Do đó, chúng ta có thể chuyển đến (i-1) mà không cần chạm vào hàng (i) nữa. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi kết thúc phép lặp (i), hàng (i) được sắp xếp và chứa (n) giá trị lớn nhất trong số (i) hàng đầu tiên. Quá trình lựa chọn đặt các giá trị này từ phải sang trái, do đó thứ tự của chúng trong hàng không giảm. Mọi giá trị còn lại trong hàng (1,\ldots,i-1) tối đa là mọi giá trị đã được đặt trong hàng (i). 

Khi lần lặp tiếp theo xử lý (i-1), nó chỉ sử dụng các hàng (1,\ldots,i-1), do đó, tính bất biến của tất cả các hàng đã hoàn thành được giữ nguyên. Cuối cùng, các hàng (3,\ldots,n) được sắp xếp riêng lẻ và mọi giá trị ở hàng đầu ra trước đó nhiều nhất là mọi giá trị ở hàng đầu ra sau. Do đó, sự kết hợp của chúng không giảm. 

Việc xây dựng chính xác là chiến lược phân loại cộng với trích xuất tối đa được mô tả trong giải pháp đã biết cho vấn đề này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

dx = (0, 1, 1, 0)
dy = (0, 0, 1, 1)

answer = []

def emit(x1, y1, x2, y2, x3, y3):
    answer.append(
        f"{chr(ord('a') + y1 - 1)}{x1} > "
        f"{chr(ord('a') + y2 - 1)}{x2} ? "
        f"{chr(ord('a') + y3 - 1)}{x3}"
    )

def work(x, y, c):
    # The four cells of the block are indexed cyclically:
    #
    # 0: top-left
    # 1: bottom-left
    # 2: bottom-right
    # 3: top-right
    #
    # Comparing the other three positions with c and rotating
    # counterclockwise puts the maximum at position c.
    for i in range(c + 1, c + 4):
        p = i & 3
        emit(
            x + dx[p],
            y + dy[p],
            x + dx[c],
            y + dy[c],
            x,
            y
        )

def solve():
    n = int(input())

    if n == 2:
        # There are no output rows at all.
        # These three valid commands also match the sample.
        for _ in range(3):
            emit(2, 1, 2, 2, 1, 1)
        print("\n".join(answer))
        return

    for i in range(n, 2, -1):
        for j in range(n, 0, -1):
            # Push the maximum of rows 1..i-1 down into row i-1.
            for x in range(1, i - 1):
                for y in range(n - 1, 0, -1):
                    work(x, y, 1)

            # Move that maximum through rows i-1 and i
            # until it reaches column j.
            for x in range(1, j):
                work(i - 1, x, 2)

        # Finish the last element in column 1 and restore
        # the workspace invariant for the next outer iteration.
        emit(i, 2, i, 1, i - 1, 1)
        emit(i - 1, 2, i, 2, i - 1, 1)
        emit(i - 1, 1, i - 1, 2, i - 1, 1)

        work(i - 2, 1, 1)

        emit(i, 1, i - 1, 1, i - 1, 1)

    print("\n".join(answer))

if __name__ == "__main__":
    solve()
```các`emit`hàm chuyển đổi một cột số thành chữ cái được yêu cầu. Các hàng được giữ dựa trên một hàng vì nó khớp trực tiếp với câu lệnh, điều này tránh được một lớp chuyển đổi bổ sung khi xây dựng tọa độ. 

các`work`chức năng là phần quan trọng. Các mảng`dx`Và`dy`mô tả bốn ô theo thứ tự tuần hoàn ngược chiều kim đồng hồ bắt đầu từ góc trên bên trái. biểu thức`i & 3`bao bọc các chỉ số xung quanh chu kỳ bốn ô. Ví dụ,`c = 1`chọn ô phía dưới bên trái, trong khi`c = 2`chọn ô phía dưới bên phải. 

Mỗi cuộc gọi đến`work`tạo ra chính xác ba lệnh. Nó không bao giờ tạo ra một phép quay không hợp lệ vì góc trên bên trái của nó luôn ở nhiều nhất là hàng (n-1) và nhiều nhất là cột (n-1). Hàng lớn nhất có thể được sử dụng làm gốc khối là (i-1) và cột lớn nhất có thể là (n-1). 

Các vòng lặp lồng nhau thực hiện quá trình lựa chọn từ bằng chứng. Vòng lặp bên ngoài giảm (i), vì vậy khi hàng (i) đã hoàn thành, nó sẽ không bao giờ được chạm nữa. Vòng lặp tiếp theo giảm dần (j), đặt các giá trị được chọn nhỏ dần về phía bên trái. 

Trường hợp (n=2) được xử lý riêng vì cấu trúc chung giả định rằng có ít nhất ba hàng. Ba lệnh mẫu có giá trị về mặt cú pháp và chỉ xoay hình vuông duy nhất (2\times2). 

## Ví dụ đã hoạt động 

### Ví dụ 1: (n=2) 

Đối với đầu vào```
2
```chương trình sử dụng ba lệnh của mẫu. 

| Bước | Lệnh | Hiệu ứng | 
| --- | --- | --- | 
| 1 |`a2 > b2 ? a1`| Chỉ xoay nếu (a_2>b_2) | 
| 2 |`a2 > b2 ? a1`| Áp dụng lại phép so sánh tương tự | 
| 3 |`a2 > b2 ? a1`| Áp dụng nó một lần nữa | 

Phần dưới cùng chứa (n-2=0) hàng nên không có điều kiện thứ tự nào cần thỏa mãn. Yêu cầu duy nhất là mọi lệnh đều hợp lệ. Cả ba lệnh đều sử dụng các ô hiện có và xoay khối hợp lệ (2\times2) bắt đầu từ`a1`. 

### Ví dụ 2: (n=3) 

Đối với đầu vào```
3
```chỉ có một hàng đầu ra, hàng (3). Do đó, việc xây dựng phải sắp xếp ba giá trị trong hàng đó theo thứ tự không giảm. 

Vòng ngoài chỉ có (i=3). Đối với mỗi (j), lần quét đầu tiên sẽ xem xét các hàng (1) và (2), đẩy mức tối đa của chúng vào hàng (2). Phần thứ hai kết hợp hàng (2) với hàng (3) và đặt giá trị sẵn có lớn nhất tại cột (j). 

| Giai đoạn | Hàng hoạt động | Cột mục tiêu | Ý nghĩa | 
| --- | --- | --- | --- | 
| Lựa chọn đầu tiên | 1, 2, 3 | 3 | Đặt giá trị lớn nhất vào`c3`| 
| Lựa chọn thứ hai | các ô còn lại | 2 | Đặt số lớn nhất tiếp theo vào`b3`| 
| Lựa chọn cuối cùng | các ô còn lại | 1 | Đặt số nhỏ nhất trong ba số đã chọn vào`a3`| 

Như vậy hàng cuối cùng có dạng 

[ 
a_3\le b_3\le c_3. 
] 

Ví dụ này chứng minh tại sao công trình không cần biết bất kỳ giá trị thực tế nào. Mọi quyết định được thực hiện bằng các so sánh được tạo trước và nguyên hàm trích xuất tối đa sẽ chuyển đổi các so sánh đó thành một hoạt động lựa chọn xác định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) lệnh | Có (O(n^2)) lệnh gọi nguyên thủy ở mỗi cấp độ bên ngoài và (n\le9) | 
| Không gian | (O(n^3)) | Chương trình được tạo sẽ được lưu trữ trước khi in | 
| Sản lượng tối đa | Dưới đây (100.000) | Với (n=9), việc xây dựng chỉ tạo ra vài nghìn lệnh | 

Chính xác hơn, phần lồng nhau chính tạo ra 

[ 
3\sum_{i=3}^{n} n(n-1)(i-2) 
] 

các lệnh, chỉ cộng với các lệnh (O(n^2)) từ lần dọn dẹp cuối cùng của mỗi hàng. Với (n=9), giá trị này nằm dưới giới hạn (100.000). Do đó, cấu trúc phù hợp với giới hạn một giây và 512 MB với biên độ lớn. 

## Trường hợp thử nghiệm 

Bởi vì đây là một vấn đề mang tính xây dựng nên việc so sánh kết quả đầu ra được tạo ra với một chuỗi cố định không phải là phép thử phù hợp. Một thử nghiệm hợp lệ sẽ phân tích các lệnh được tạo, mô phỏng chúng trên nhiều lưới và kiểm tra thứ tự cuối cùng được yêu cầu. 

Bộ khai thác thử nghiệm sau đây nhúng cấu trúc tương tự vào một hàm có thể gọi được. Nó xác nhận trường hợp tối thiểu, trường hợp giá trị nhỏ đầy đủ cho (n=3), một số trường hợp ngẫu nhiên (n=4), giá trị bằng nhau và kích thước tối đa.```python
import io
import random
from itertools import product

dx = (0, 1, 1, 0)
dy = (0, 0, 1, 1)

def build(n):
    ans = []

    def emit(x1, y1, x2, y2, x3, y3):
        ans.append((x1, y1, x2, y2, x3, y3))

    def work(x, y, c):
        for i in range(c + 1, c + 4):
            p = i & 3
            emit(
                x + dx[p],
                y + dy[p],
                x + dx[c],
                y + dy[c],
                x,
                y
            )

    if n == 2:
        for _ in range(3):
            emit(2, 1, 2, 2, 1, 1)
        return ans

    for i in range(n, 2, -1):
        for j in range(n, 0, -1):
            for x in range(1, i - 1):
                for y in range(n - 1, 0, -1):
                    work(x, y, 1)

            for x in range(1, j):
                work(i - 1, x, 2)

        emit(i, 2, i, 1, i - 1, 1)
        emit(i - 1, 2, i, 2, i - 1, 1)
        emit(i - 1, 1, i - 1, 2, i - 1, 1)
        work(i - 2, 1, 1)
        emit(i, 1, i - 1, 1, i - 1, 1)

    return ans

def rotate_ccw(a, x, y):
    a[x][y], a[x][y + 1], a[x + 1][y + 1], a[x + 1][y] = (
        a[x][y + 1],
        a[x + 1][y + 1],
        a[x + 1][y],
        a[x][y]
    )

def simulate(n, program, values):
    a = [list(values[i * n:(i + 1) * n]) for i in range(n)]

    for x1, y1, x2, y2, x3, y3 in program:
        x1 -= 1
        y1 -= 1
        x2 -= 1
        y2 -= 1
        x3 -= 1
        y3 -= 1

        assert 0 <= x1 < n
        assert 0 <= x2 < n
        assert 0 <= x3 < n - 1
        assert 0 <= y1 < n
        assert 0 <= y2 < n
        assert 0 <= y3 < n - 1

        if a[x1][y1] > a[x2][y2]:
            rotate_ccw(a, x3, y3)

    result = []
    for r in range(2, n):
        result.extend(a[r])

    assert all(
        result[i] <= result[i + 1]
        for i in range(len(result) - 1)
    )

    return a

def run(inp: str) -> str:
    n = int(inp.strip())
    program = build(n)

    # Return the actual program in the problem's textual format.
    out = []
    for x1, y1, x2, y2, x3, y3 in program:
        out.append(
            f"{chr(ord('a') + y1 - 1)}{x1} > "
            f"{chr(ord('a') + y2 - 1)}{x2} ? "
            f"{chr(ord('a') + y3 - 1)}{x3}"
        )

    return "\n".join(out)

# Provided sample.
sample = run("2")
expected = "\n".join([
    "a2 > b2 ? a1",
    "a2 > b2 ? a1",
    "a2 > b2 ? a1",
])
assert sample == expected, "sample 1"

# Minimum-size input.
assert len(build(2)) == 3, "minimum n"

# Exhaustive ternary-value testing for n = 3.
# 3^9 = 19683 grids, small enough for a local correctness test.
program3 = build(3)
for values in product(range(3), repeat=9):
    simulate(3, program3, values)

# All values equal.
program4 = build(4)
simulate(4, program4, [7] * 16)

# Random boundary-heavy cases for n = 4.
random.seed(1)
for _ in range(200):
    values = [random.choice([-10, 0, 1, 10]) for _ in range(16)]
    simulate(4, program4, values)

# Maximum-size input and output bound.
program9 = build(9)
assert len(program9) <= 100000, "command limit"

for _ in range(100):
    values = [random.randint(-10**9, 10**9) for _ in range(81)]
    simulate(9, program9, values)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| Ba lệnh mẫu | Kích thước tối thiểu và xử lý ranh giới hợp lệ | 
|`3`với mọi giá trị từ`{0,1,2}`| Hàng cuối cùng được sắp xếp cho tất cả (3^9) lưới | Xác minh toàn diện việc xây dựng cốt lõi | 
|`4`với mọi giá trị bằng`7`| Hàng cuối cùng không thay đổi và được sắp xếp | So sánh có giá trị bằng nhau khi không bị ép xoay | 
|`4`với các giá trị ngẫu nhiên từ`{-10,0,1,10}`| Hai hàng dưới cùng được sắp xếp trên toàn cầu | Giá trị trùng lặp và ranh giới so sánh | 
|`9`với các giá trị ngẫu nhiên | Bảy hàng dưới cùng được sắp xếp trên toàn cầu | Kích thước tối đa và giới hạn số lượng lệnh | 

## Vỏ cạnh 

Với (n=2), đầu vào chính xác là```
2
```Chương trình bao gồm ba bản sao của`a2 > b2 ? a1`. Mọi ô được tham chiếu đều tồn tại và`a1`là góc trên bên trái hợp pháp. Vì đầu ra được yêu cầu chứa 0 hàng nên điều kiện sắp xếp là trống. Đây là lý do vì sao việc xây dựng tổng quát (n\ge3) là không cần thiết. 

Đối với các giá trị bằng nhau, hãy xem xét khối (2\times2) chứa 

[ 
\bắt đầu{ma trận} 
5&5\ 
5&5 
\end{ma trận}. 
] 

Mọi so sánh trong`work`là sai. Không có vòng quay nào xảy ra nhưng góc được chọn đã chứa giá trị tối đa. Do đó, kiểu nguyên thủy vẫn đúng ngay cả khi phép so sánh chặt chẽ không bao giờ thành công. 

Đối với khối ranh giới, hãy xem xét khối hợp lệ ngoài cùng bên phải (2\times2). Cột phía trên bên trái của nó là (n-1), không phải (n). Việc sử dụng các vòng lặp của công trình`y in range(n - 1, 0, -1)`, do đó, gốc khối được tạo lớn nhất chính xác là cột (n-1). Tương tự như vậy, nguồn gốc hàng không bao giờ đạt tới (n). Điều này giúp robot không bị hỏng. 

Đối với kích thước đầu vào tối đa (n=9), tất cả các tham chiếu hàng và cột vẫn ở dạng một chữ số, do đó biểu diễn ô văn bản vẫn ở định dạng mà vấn đề cho phép. Chương trình được tạo ra chỉ có vài nghìn lệnh, thấp hơn nhiều so với giới hạn (100.000). 

Trường hợp cạnh chính xác quan trọng nhất là một giá trị đã nằm trong vùng chính xác. Nguyên hàm trích xuất tối đa không buộc phải thực hiện các phép quay không cần thiết. Nếu góc được chọn đã lớn nhất thì cả ba phép so sánh đều thất bại. Trong giai đoạn lựa chọn, khi vị trí hàng đã được cố định, các thao tác sau này chỉ xem xét tiền tố vẫn hoạt động, do đó các giá trị đã đặt sẽ không thể thay thế được. Đó là thuộc tính cho phép cấu trúc hoạt động giống như sắp xếp lựa chọn mặc dù hoạt động vật lý của nó là xoay bốn ô.
