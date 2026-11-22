---
title: "CF 268E - Danh sách phát"
description: "Mỗi bài hát có hai thuộc tính. Độ dài của nó là l, và độc lập Manao thích nó với xác suất p. Khi một bài hát được phát lần đầu tiên, một trong hai điều sẽ xảy ra. Nếu anh ấy thích, bài hát sẽ được thêm vào bộ sưu tập các bài hát được ghi nhớ."
date: "2026-06-05T01:10:50+07:00"
tags: ["codeforces", "competitive-programming", "math", "probabilities", "sortings"]
categories: ["algorithms"]
codeforces_contest: 268
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 164 (Div. 2)"
rating: 2100
weight: 268
solve_time_s: 160
verified: true
draft: false
---

[CF 268E - Danh sách phát](https://codeforces.com/problemset/problem/268/E) 

**Xếp hạng:** 2100 
**Tags:** toán, xác suất, sắp xếp 
**Thời gian giải:** 2 phút 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi bài hát có hai thuộc tính. chiều dài của nó là`l`, và độc lập Manao thích nó với xác suất`p`. 

Khi một bài hát được phát lần đầu tiên, một trong hai điều sẽ xảy ra. 

Nếu anh ấy thích, bài hát sẽ được thêm vào bộ sưu tập các bài hát được ghi nhớ. 

Nếu không thích, anh ấy sẽ ngay lập tức phát lại từng bài hát đã nhớ một lần. 

Những bài hát được ghi nhớ chính xác là những bài hát được yêu thích trong số tất cả các bài hát được xử lý trước đó. Một khi một bài hát được thích, nó sẽ được thích mãi mãi. 

Chúng tôi có thể sắp xếp các bài hát theo thứ tự bất kỳ. Trong số tất cả các hoán vị, chúng ta phải tối đa hóa tổng thời gian nghe dự kiến. 

Đầu vào chứa tới 50.000 bài hát. Một thuật toán bậc hai sẽ yêu cầu khoảng 2,5 tỷ cặp tương tác, vượt xa giới hạn. Giải pháp phải gần`O(n log n)`. 

Khó khăn chính là sự đóng góp của một bài hát phụ thuộc vào bài hát nào xuất hiện trước nó. Một bài hát có thể được phát lại nhiều lần vì những bài hát không được ưa thích trong tương lai, vì vậy sự lựa chọn tham lam của người dân địa phương không thể hiện rõ ràng ngay lập tức. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một bài hát không bao giờ được yêu thích:```
1
100 0
```Thời gian nghe dự kiến ​​chính xác`100`. Không có bài hát nào được ghi nhớ nên không có bản phát lại nào xảy ra. Bất kỳ công thức nào nhân một cách mù quáng với xác suất mà không cần xử lý`p = 0`một cách chính xác có thể tạo ra những đóng góp bổ sung sai lầm. 

Hãy xem xét một bài hát luôn được yêu thích:```
1
100 100
```Câu trả lời cũng là`100`. Bài hát được phát một lần và không bao giờ được phát lại vì không có bài hát nào sau này. Một số dẫn xuất vô tình tính số lần tự tương tác và thu được giá trị lớn hơn. 

Một trường hợp tinh tế khác là khi một xác suất bằng 0:```
2
100 100
200 0
```Nếu bài hát đầu tiên được đặt trước bài hát thứ hai thì tổng thời gian nghe là```
100 + 200 + 100 = 400
```bởi vì bài hát thứ hai chắc chắn không được thích và khiến người ta phải phát lại bài hát đầu tiên. 

Nếu đảo ngược thứ tự thì tổng số chỉ là```
200 + 100 = 300
```Vì vậy, thứ tự vẫn quan trọng ngay cả khi một xác suất mang tính quyết định. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi hoán vị của bài hát, tính toán thời gian nghe dự kiến và giữ ở mức tối đa. 

Đối với hoán vị cố định, kỳ vọng có thể được tính từ các công thức xác suất. Thật không may là có`n!`hoán vị. Ngay cả đối với`n = 15`điều này đã là vô vọng rồi, trong khi giới hạn thực tế là`50,000`. 

Quan sát quan trọng là kỳ vọng là tuyến tính. Thay vì suy nghĩ về việc thực thi hoàn chỉnh, chúng tôi có thể tính toán mức đóng góp dự kiến ​​của từng tương tác giữa các cặp bài hát. 

Giả sử bài hát`i`xuất hiện trước bài hát`j`. 

Bài hát`i`được phát lại khi bài hát`j`không thích, nhưng chỉ khi bài hát`i`đã được yêu thích trước đó. 

Xác suất xảy ra sự kiện đó là$$P(i\text{ liked}) \cdot P(j\text{ disliked})$$cái nào bằng$$\frac{p_i}{100}\left(1-\frac{p_j}{100}\right).$$Bất cứ khi nào điều đó xảy ra, một bổ sung`l_i`giây được lắng nghe. 

Vì vậy mỗi cặp đặt hàng`(i,j)`với`i`trước`j`đóng góp$$l_i \cdot \frac{p_i}{100}\left(1-\frac{p_j}{100}\right)$$đến sự mong đợi. 

Thời lượng dự kiến ​​của lần phát đầu tiên của mỗi bài hát luôn được tính chính xác một lần, bất kể thứ tự:$$\sum l_i.$$Do đó bài toán tối ưu hóa trở thành:$$\sum l_i + \sum_{i<j} l_i \cdot \frac{p_i}{100} \left(1-\frac{p_j}{100}\right).$$Số hạng không đổi không phụ thuộc vào hoán vị. Chỉ có thuật ngữ cặp mới quan trọng. 

Bất cứ khi nào một mục tiêu có thể được viết dưới dạng tổng của các đóng góp theo cặp, thì đối số trao đổi thường là công cụ phù hợp. Chúng tôi so sánh hai bài hát liền kề và xác định thứ tự nào tốt hơn. Điều này tạo ra tiêu chí sắp xếp và giảm vấn đề xuống còn sắp xếp một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n! · n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Suy ra quy tắc đặt hàng 

Hãy xem xét hai bài hát`A`Và`B`. 

Cho phép$$a=l_A,\quad x=\frac{p_A}{100}$$Và$$b=l_B,\quad y=\frac{p_B}{100}.$$Chỉ có sự tương tác lẫn nhau của chúng thay đổi khi chúng ta hoán đổi chúng. 

Nếu như`A`đến trước`B`, cặp đôi đóng góp$$a x (1-y).$$Nếu như`B`đến trước`A`, cặp đôi đóng góp$$b y (1-x).$$chúng tôi muốn`A`trước`B`khi$$a x (1-y) > b y (1-x).$$nhân với`100^2`để tránh phân số cho$$a p_A (100-p_B) > b p_B (100-p_A).$$Đây là một quy tắc so sánh đầy đủ giữa hai bài hát. 

### bước 

1. Đọc tất cả các bài hát. 
2. Sắp xếp bài hát bằng bộ so sánh$$l_i p_i (100-p_j) > l_j p_j (100-p_i).$$Nếu sự bất bình đẳng này đúng, bài hát`i`phải xuất hiện trước bài hát`j`. 

1. Sau khi sắp xếp, hãy tính kỳ vọng. 
2. Duy trì$$S=\sum_{\text{processed}} l_i \cdot \frac{p_i}{100}.$$

`S`biểu thị tổng thời lượng dự kiến ​​của các bài hát được ghi nhớ sau tất cả các bài hát được xử lý trước đó. 

1. Xử lý bài hát từ trái sang phải. 
2. Thêm lần phát đầu tiên của bài hát, cụ thể là`l_i`, để trả lời. 
3. Có khả năng bài hát hiện tại không được ưa thích$$1-\frac{p_i}{100}.$$Nếu điều đó xảy ra, tất cả các bài hát đã nhớ sẽ được phát lại. Tổng chiều dài dự kiến ​​​​của họ chính xác là`S`. 

Thêm vào$$S\left(1-\frac{p_i}{100}\right)$$để trả lời. 

1. Cập nhật$$S \leftarrow S + l_i\frac{p_i}{100}.$$1. Đưa ra kỳ vọng cuối cùng. 

### Tại sao nó hoạt động 

Mục tiêu theo cặp có tính chất cộng thêm. Đối với bất kỳ hai bài hát lân cận nào, sự khác biệt về đóng góp giữa hai thứ tự có thể chỉ phụ thuộc vào hai bài hát đó:$$a p_A (100-p_B) - b p_B (100-p_A).$$Bất cứ khi nào giá trị này là dương, đặt`A`trước`B`làm tăng sự mong đợi. Điều này xác định một mối quan hệ thứ tự theo cặp hợp lệ. Sắp xếp theo mối quan hệ đó đảm bảo rằng mọi đảo ngược liền kề đã bị loại bỏ. Theo đối số trao đổi tiêu chuẩn, không có sự hoán đổi nào nữa có thể cải thiện sự sắp xếp, do đó hoán vị thu được là tối ưu. 

Tính toán kỳ vọng cũng đúng vì tính tuyến tính của kỳ vọng cho phép chúng ta thay thế tập hợp ngẫu nhiên các bài hát được ghi nhớ bằng tổng độ dài dự kiến ​​của nó.`S`. Mỗi bài hát đóng góp độc lập`l_i p_i/100`với độ dài được ghi nhớ dự kiến. 

## Giải pháp Python```python
import sys
from functools import cmp_to_key

input = sys.stdin.readline

def solve():
    n = int(input())
    songs = []

    for _ in range(n):
        l, p = map(int, input().split())
        songs.append((l, p))

    def cmp(a, b):
        l1, p1 = a
        l2, p2 = b

        left = l1 * p1 * (100 - p2)
        right = l2 * p2 * (100 - p1)

        if left > right:
            return -1
        if left < right:
            return 1
        return 0

    songs.sort(key=cmp_to_key(cmp))

    ans = 0.0
    remembered = 0.0

    for l, p in songs:
        prob = p / 100.0

        ans += l
        ans += remembered * (1.0 - prob)

        remembered += l * prob

    print("{:.12f}".format(ans))

if __name__ == "__main__":
    solve()
```Bộ so sánh là trung tâm của giải pháp. Nó xuất phát trực tiếp từ việc so sánh hai thứ tự có thể có của một cặp bài hát. Việc sử dụng phép nhân chéo sẽ tránh được các vấn đề về độ chính xác của dấu phẩy động trong quá trình sắp xếp. 

Biến`remembered`lưu trữ tổng thời lượng dự kiến ​​của các bài hát đã được ghi nhớ. Nó không phải là độ dài ngẫu nhiên thực tế trong một lần thực hiện. Bằng tính tuyến tính của kỳ vọng, chỉ cần theo dõi giá trị kỳ vọng là đủ. 

Câu trả lời được tích lũy dần dần. Mỗi bài hát đều đóng góp thời lượng phát lại ban đầu của riêng nó. Khi đó, với xác suất`1 - p/100`, nó khiến tất cả các bài hát đã nhớ được phát lại. Độ dài dự kiến ​​của những bài hát đó chính xác là`remembered`. 

Tất cả số học bên trong bộ so sánh đều sử dụng số nguyên. Giá trị lớn nhất là khoảng$$1000 \cdot 100 \cdot 100 = 10^7,$$rất nhỏ so với phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
150 20
150 50
100 50
```Bộ so sánh sắp xếp các bài hát như sau:```
(150,50), (100,50), (150,20)
```| Bước | Bài hát`(l,p)`| nhớ trước | Đã thêm kỳ vọng phát lại | Trả lời sau | 
| --- | --- | --- | --- | --- | 
| 1 | (150,50) | 0 | 0 | 150 | 
| 2 | (100,50) | 75 | 37,5 | 287,5 | 
| 3 | (150,20) | 125 | 100 | 537,5 | 

Câu trả lời cuối cùng:```
537.500000000
```Dấu vết này cho thấy cách`remembered`đại diện cho độ dài được ghi nhớ dự kiến. Sau bài hát đầu tiên nó bằng`150 × 0.5 = 75`. Sau bài hát thứ hai nó trở thành`125`. 

### Ví dụ tùy chỉnh 

đầu vào:```
2
100 100
200 0
```Thứ tự tối ưu đã được hiển thị bởi bộ so sánh. 

| Bước | Bài hát`(l,p)`| nhớ trước | Đã thêm kỳ vọng phát lại | Trả lời sau | 
| --- | --- | --- | --- | --- | 
| 1 | (100.100) | 0 | 0 | 100 | 
| 2 | (200,0) | 100 | 100 | 400 | 

Câu trả lời cuối cùng:```
400.000000000
```Ví dụ này chứng minh tại sao các bài hát có khả năng xảy ra cao có xu hướng di chuyển sớm hơn. Một lượt không thích nhất định sau này trong danh sách phát sẽ liên tục kích hoạt các bài hát được ghi nhớ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log n)`| Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian |`O(n)`| Lưu trữ tất cả các bài hát | 

Với`n = 50,000`, MỘT`O(n log n)`sắp xếp thực hiện khoảng vài trăm nghìn so sánh, phù hợp thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io
from functools import cmp_to_key

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    songs = [tuple(map(int, input().split())) for _ in range(n)]

    def cmp(a, b):
        l1, p1 = a
        l2, p2 = b

        x = l1 * p1 * (100 - p2)
        y = l2 * p2 * (100 - p1)

        if x > y:
            return -1
        if x < y:
            return 1
        return 0

    songs.sort(key=cmp_to_key(cmp))

    ans = 0.0
    remembered = 0.0

    for l, p in songs:
        prob = p / 100.0
        ans += l
        ans += remembered * (1.0 - prob)
        remembered += l * prob

    return f"{ans:.12f}"

# provided sample
assert run(
"""3
150 20
150 50
100 50
"""
) == "537.500000000000", "sample 1"

# minimum size
assert run(
"""1
100 0
"""
) == "100.000000000000", "single song, never liked"

# always liked then always disliked
assert run(
"""2
100 100
200 0
"""
) == "400.000000000000", "replay triggered once"

# all probabilities zero
assert run(
"""3
50 0
60 0
70 0
"""
) == "180.000000000000", "no remembered songs ever"

# all probabilities one hundred
assert run(
"""3
50 100
60 100
70 100
"""
) == "180.000000000000", "no replay events"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một bài hát, xác suất 0 | 100 | Trường hợp cơ sở | 
|`(100,100)`Và`(200,0)`| 400 | Hiệu ứng đặt hàng | 
| Tất cả xác suất 0 | Tổng độ dài | Không có bài hát nào được ghi nhớ | 
| Tất cả xác suất 100 | Tổng độ dài | Không có lượt không thích xảy ra | 

## Vỏ cạnh 

###Bài hát chưa bao giờ thích 

đầu vào:```
1
100 0
```Thứ tự sắp xếp không thay đổi. Ban đầu`remembered = 0`. 

Bài hát góp phần`100`cho lần chơi đầu tiên của nó. Từ`remembered`bằng 0, thời hạn phát lại cũng bằng 0. Câu trả lời là chính xác`100`. 

###Bài hát luôn thích 

đầu vào:```
1
100 100
```Bài hát góp phần`100`. Xác suất chơi lại bằng 0 nên không có gì bổ sung thêm. Sau đó`remembered`trở thành`100`, nhưng không có bài hát nào sau này. Kết quả vẫn còn`100`. 

### Không thích xác định sau thích xác định 

đầu vào:```
2
100 100
200 0
```Bộ so sánh thích bài hát đầu tiên trước bài hát thứ hai vì$$100 \cdot 100 \cdot 100 > 200 \cdot 0 \cdot 0.$$Sau khi xử lý bài hát đầu tiên,`remembered = 100`. 

Bài hát thứ hai chắc chắn không được thích nên phần đóng góp replay của nó chính xác là`100`. 

Câu trả lời cuối cùng là$$100 + 200 + 100 = 400.$$Điều này xác nhận rằng thuật toán xử lý chính xác các xác suất ở cả hai thái cực và khai thác chính xác các cơ hội chơi lại thông qua quy tắc sắp xếp.
