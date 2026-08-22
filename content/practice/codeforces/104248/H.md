---
title: "CF 104248H - Nội tiếp tam giác 3"
description: "Chúng ta được cho ba số được cho là đại diện cho độ dài của ba đoạn của một đường đa tuyến khép kín được vẽ bên trong hoặc trên ranh giới của một tam giác cố định."
date: "2026-07-01T22:10:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104248
codeforces_index: "H"
codeforces_contest_name: "Udmurt SU Contest 2010"
rating: 0
weight: 104248
solve_time_s: 45
verified: true
draft: false
---

[CF 104248H - Tam giác nội tiếp 3](https://codeforces.com/problemset/problem/104248/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba số được cho là đại diện cho độ dài của ba đoạn của một đường đa tuyến khép kín được vẽ bên trong hoặc trên ranh giới của một tam giác cố định. Đường đa tuyến có chính xác ba đoạn và mọi đỉnh của đường đa tuyến này phải nằm trên một trong các cạnh của tam giác. Ngoài ra, mỗi cạnh của tam giác phải chứa ít nhất một trong các đỉnh này, do đó đường đa giác buộc phải “chạm” vào tất cả các cạnh của tam giác. 

Trong số tất cả các đường polyline hợp lệ như vậy, trước tiên chúng tôi giảm thiểu tổng chiều dài của cả ba đoạn. Nếu nhiều cấu trúc đạt được tổng chiều dài tối thiểu như nhau thì chúng ta sẽ chọn cấu trúc có chiều dài tối thiểu hóa tích của các đoạn. Nhiệm vụ không yêu cầu chúng ta xây dựng bất cứ điều gì. Thay vào đó, chúng ta chỉ được hỏi liệu có tồn tại một số tam giác mà đa tuyến tối ưu thu được có độ dài các đoạn thẳng bằng bộ ba a, b, c đã cho hay không. 

Các ràng buộc đầu vào rất nhỏ, với mỗi giá trị nằm trong khoảng từ âm một nghìn đến một nghìn. Điều này ngay lập tức chỉ ra rằng việc tìm kiếm hình học mạnh mẽ trên các hình tam giác là không thể, nhưng cũng gợi ý rằng câu trả lời có thể được xác định bởi một điều kiện đại số đơn giản trên ba số chứ không phải bất kỳ hình học rõ ràng nào. 

Một điểm tinh tế quan trọng là các giá trị âm được cho phép trong đầu vào. Vì độ dài các đoạn trong hình học luôn không âm nên mọi cấu hình hợp lệ đều phải có a, b, c đều không âm. Chỉ điều đó thôi đã loại bỏ được nhiều trường hợp. 

Một trường hợp ẩn khác là sự suy biến của tam giác hỗ trợ việc xây dựng. Mặc dù tam giác ban đầu được yêu cầu là không suy biến, nhưng ràng buộc đa tuyến chỉ quan tâm đến các điểm nằm trên các cạnh của nó và việc tối ưu hóa có hiệu quả giảm xuống việc suy luận về cách ba điểm trên ranh giới tam giác kết nối trong một chuỗi khép kín ngắn nhất. Nhiều cách giải thích ngây thơ giả định không chính xác hình học tùy ý, nhưng câu trả lời thực sự rút gọn thành một điều kiện đơn giản hơn nhiều. 

Một cách tiếp cận bất cẩn có thể cố gắng liệt kê các hình tam giác hoặc vị trí các đỉnh, nhưng điều này sẽ không cần thiết và gây hiểu nhầm vì mức độ tự do có ý nghĩa duy nhất sẽ biến mất sau bước cực tiểu hóa. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu sẽ là xem xét một tam giác có tọa độ, đặt ba điểm trên các cạnh của nó, kết nối chúng theo tất cả các thứ tự tuần hoàn có thể có và tính toán độ dài đa tuyến thu được trong khi thực thi rằng mỗi cạnh được chạm ít nhất một lần. Đối với mỗi hình dạng và vị trí tam giác, chúng tôi sẽ kiểm tra xem cấu hình tối ưu có mang lại độ dài đoạn a, b, c hay không. 

Cách tiếp cận này đúng về mặt lý thuyết nhưng thất bại ngay lập tức vì không gian của các hình tam giác là liên tục. Ngay cả khi chúng ta rời rạc hóa tọa độ, số lượng cấu hình vẫn tăng theo cấp số nhân. Mỗi vị trí của ba điểm trên ba cạnh đã có sự tự do liên tục và việc giảm thiểu tất cả các vị trí như vậy dẫn đến một không gian tìm kiếm không thể đếm được. 

Quan sát quan trọng là sau khi giảm thiểu tổng chiều dài, đường đa tuyến tối ưu luôn suy biến thành cấu hình chỉ phụ thuộc vào số lần mỗi cạnh được “sử dụng” làm đoạn thẳng trong biểu diễn ranh giới mở rộng. Trên thực tế, các ràng buộc hình học buộc đường đa tuyến hoạt động giống như một chuỗi khép kín ngắn nhất trên cấu trúc giống cây được hình thành bởi các cạnh tam giác. Điều này làm giảm vấn đề về một điều kiện đại số thuần túy: độ dài ba đoạn phải có khả năng tạo thành một tam giác hợp lệ theo nghĩa bất đẳng thức sau khi sắp xếp thích hợp và tất cả chúng phải không âm.

Theo trực giác, bước tối thiểu hóa sẽ loại bỏ sự phụ thuộc vào hình dạng tam giác thực tế. Những gì còn lại là một cấu hình tương đương với việc kết nối ba điểm trên một ranh giới lồi theo thứ tự tuần hoàn ngắn nhất, điều này luôn đạt được bằng các đoạn thẳng hoạt động giống như khoảng cách Euclide dọc theo một ranh giới phẳng. Điều này dẫn đến điều kiện cổ điển là bộ ba phải thỏa mãn bất đẳng thức tam giác sau khi sắp xếp. 

Do đó, toàn bộ thiết lập hình học tập trung vào việc kiểm tra xem a, b, c có thể được hiểu là độ dài các cạnh của một tam giác không suy biến hay không, với ràng buộc bổ sung là tất cả đều phải dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm hình học Brute Force | O(vô hạn) | O(1) | Quá chậm | 
| Kiểm tra bất đẳng thức tam giác tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi muốn xác định xem có tồn tại cấu hình tạo ra độ dài đoạn chính xác bằng a, b, c hay không. Vì chúng đại diện cho độ dài nên hạn chế đầu tiên là tính không âm. 

Ràng buộc thứ hai xuất phát từ cấu trúc của chu trình 3 đoạn khép kín tối thiểu: bất kỳ chu trình nào như vậy phải hoạt động giống như một tam giác trong không gian Euclide sau khi tối ưu hóa, do đó độ dài đoạn phải có khả năng tạo thành một tam giác hợp lệ. 

### Các bước 

1. Đọc ba số nguyên a, b, c. Đây là độ dài đoạn ứng cử viên của đa tuyến. 
2. Kiểm tra xem a, b, c có âm hay không. Nếu ít nhất một giá trị âm, ngay lập tức kết luận rằng không thể cấu hình được. Độ dài âm không thể đại diện cho bất kỳ phân đoạn hình học nào. 
3. Sắp xếp các giá trị sao cho x ≤ y ≤ z. Việc sắp xếp cho phép chúng ta biểu diễn tất cả các điều kiện cần thiết trong một bất đẳng thức duy nhất thay vì kiểm tra các hoán vị. 
4. Xác minh xem x + y ≥ z có đúng hay không. Đây là điều kiện bất đẳng thức tam giác để đảm bảo ba đoạn có thể khép lại thành một vòng mà không tạo ra một hình học “ngắt” hoặc không thể thực hiện được. 
5. Nếu cả không âm và bất đẳng thức tam giác đều giữ nguyên, xuất ra “Có”. Nếu không thì xuất ra “Không”. 

### Tại sao nó hoạt động 

Việc tối ưu hóa trong cấu trúc ban đầu buộc đường đa tuyến hoạt động giống như đường dẫn khép kín ngắn nhất nối ba điểm biên của một hình lồi. Một đường đi như vậy không thể có một “góc thâm hụt” ngăn chặn việc đóng cửa, góc này được nắm bắt chính xác do vi phạm bất đẳng thức tam giác. Nếu đoạn lớn nhất dài hơn tổng của hai đoạn còn lại thì không có cách thực hiện hình học nào có thể đóng vòng lặp mà không tăng tổng chiều dài vượt quá mức tối ưu, mâu thuẫn với mức tối thiểu. Ngược lại, bất cứ khi nào bất đẳng thức tam giác xảy ra, chúng ta có thể hiểu các đoạn này tạo thành một cấu hình ranh giới tam giác hợp lệ và tồn tại sự nhúng suy biến trên một số tam giác đạt được độ dài đoạn chính xác này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c = map(int, input().split())

# all lengths must be nonnegative
if a < 0 or b < 0 or c < 0:
    print("No")
    sys.exit()

x, y, z = sorted([a, b, c])

# triangle inequality
if x + y >= z:
    print("Yes")
else:
    print("No")
```Giải pháp trước tiên lọc các đầu vào âm không hợp lệ vì hình học cấm độ dài phân đoạn âm. Sau đó, nó giảm điều kiện xuống một kiểm tra bất đẳng thức được sắp xếp duy nhất. Việc sắp xếp đảm bảo chúng ta chỉ cần kiểm tra một trường hợp bất đẳng thức tam giác thay vì tất cả các hoán vị. 

Chi tiết triển khai chính đang sử dụng`>=`còn hơn là`>`. Sự bình đẳng được cho phép vì tam giác suy biến vẫn tương ứng với cấu hình giới hạn hợp lệ theo quy tắc cực tiểu hóa của bài toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3 4
```Chúng tôi theo dõi các giá trị được sắp xếp và kiểm tra sự bất bình đẳng. 

| Bước | x | y | z | x + y ≥ z | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 3 | 4 | - | - | 
| Đã sắp xếp | 2 | 3 | 4 | 5 ≥ 4 | Có | 

Điều này thể hiện cấu hình hợp lệ tiêu chuẩn trong đó phân đoạn lớn nhất không quá lớn so với hai phân đoạn còn lại, do đó có thể đóng. 

### Ví dụ 2 

đầu vào:```
-1 -1 -1
```| Bước | x | y | z | x + y ≥ z | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | -1 | -1 | -1 | - | - | 

Vì có ít nhất một giá trị âm nên cấu hình sẽ bị từ chối ngay lập tức. 

Điều này cho thấy rằng cách giải thích hình học của độ dài đoạn thẳng nghiêm cấm các đầu vào âm bất kể cấu trúc bất bình đẳng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Sắp xếp ba số và kiểm tra một bất đẳng thức là công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến vô hướng được sử dụng | 

Các ràng buộc cho phép giải pháp theo thời gian không đổi và không cần vòng lặp hoặc cấu trúc dữ liệu. Thuật toán phù hợp một cách tầm thường trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    a, b, c = map(int, input().split())

    if a < 0 or b < 0 or c < 0:
        return "No"

    x, y, z = sorted([a, b, c])

    if x + y >= z:
        return "Yes"
    return "No"

# provided sample
assert run("2 3 4\n") == "Yes"
assert run("-1 -1 -1\n") == "No"

# custom cases
assert run("0 0 0\n") == "Yes", "degenerate zero triangle"
assert run("1 2 3\n") == "Yes", "degenerate equality case"
assert run("1 2 4\n") == "No", "violates triangle inequality"
assert run("10 1 1\n") == "No", "large imbalance case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | Có | trường hợp biên hợp lệ suy biến | 
| 1 2 3 | Có | được phép bình đẳng | 
| 1 2 4 | Không | vi phạm bất đẳng thức tam giác | 
| 10 1 1 | Không | từ chối mất cân bằng cực độ | 

## Vỏ cạnh 

Đối với đầu vào tiêu cực như`-1 5 5`, thuật toán sẽ ngay lập tức loại bỏ chúng trước khi sắp xếp. Điều này tránh việc diễn giải sai chúng theo chiều dài hình học sau khi đặt hàng. 

Đối với các trường hợp thoái hóa như`0 0 0`, sắp xếp tạo ra`(0, 0, 0)`và sự bất bình đẳng`0 + 0 ≥ 0`giữ, do đó kết quả đầu ra chính xác là “Có”. Điều này phù hợp với cách giải thích rằng một tam giác thu gọn vẫn thỏa mãn điều kiện về độ dài tối thiểu. 

Đối với sự bình đẳng ranh giới như`1 2 3`, sắp xếp cho`(1, 2, 3)`và bất đẳng thức đúng. Điều này xác nhận rằng đẳng thức được chấp nhận trong điều kiện đóng, phù hợp với việc thực hiện tam giác suy biến. 

Đối với các đầu vào không cân bằng mạnh như`10 1 1`, sắp xếp cho`(1, 1, 10)`và bất đẳng thức không thành công. Điều này phản ánh chính xác rằng không có cấu hình nào có thể đóng một vòng lặp có một đoạn dài hơn tổng của hai đoạn còn lại, điều này sẽ buộc tổng chiều dài tăng vượt quá cấu trúc tối ưu được cho là.
