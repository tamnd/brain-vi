---
title: "CF 102623B - Lá Tre Rhapsody"
description: "Nhiệm vụ là tìm xem một tin nhắn được gửi từ nguồn có thể đạt được ít nhất một sao nhanh đến mức nào. Các ngôi sao là những điểm trong không gian ba chiều và thông điệp truyền theo đường thẳng với tốc độ một đơn vị khoảng cách mỗi năm."
date: "2026-08-02T14:06:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102623
codeforces_index: "B"
codeforces_contest_name: "2020 Lenovo Cup USST Campus Online Invitational Contest"
rating: 0
weight: 102623
solve_time_s: 191
verified: true
draft: false
---

[CF 102623B - Rhapsody Lá Tre](https://codeforces.com/problemset/problem/102623/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tìm xem một tin nhắn được gửi từ nguồn có thể đạt được ít nhất một sao nhanh đến mức nào. Các ngôi sao là những điểm trong không gian ba chiều và thông điệp truyền theo đường thẳng với tốc độ một đơn vị khoảng cách mỗi năm. Vì đích đến có thể là bất kỳ một trong những ngôi sao nhất định nên câu trả lời đơn giản là khoảng cách nhỏ nhất từ ​​điểm gốc đến bất kỳ ngôi sao nào. 

Mỗi điểm đầu vào mô tả một điểm đến có thể. Đối với một ngôi sao ở tọa độ`(x, y, z)`, thời gian di chuyển cần thiết là khoảng cách Euclide từ`(0, 0, 0)`đến thời điểm đó, đó là`sqrt(x^2 + y^2 + z^2)`. Đầu ra là khoảng cách tối thiểu trong số này, được làm tròn đến ba chữ số sau dấu thập phân. 

Số lượng ngôi sao nhiều nhất là 1000. Con số này đủ nhỏ để chúng ta có thể kiểm tra từng ngôi sao một lần. Một thuật toán có hành vi bậc hai sẽ thực hiện khoảng một triệu phép tính, điều này có thể chấp nhận được ở đây, nhưng cấu trúc của bài toán cho phép quét tuyến tính thậm chí còn đơn giản hơn. Các giá trị tọa độ được giới hạn ở 1000 ở giá trị tuyệt đối, do đó, khoảng cách bình phương vừa khít với phạm vi số nguyên thông thường trong Python. 

Một số chi tiết triển khai có thể gây ra câu trả lời sai. Một lỗi phổ biến là so sánh khoảng cách sau khi lấy căn bậc hai, điều này không cần thiết và có thể đưa ra công việc chính xác không giúp ích được gì. Một sai lầm khác là in quá ít chữ số. Ví dụ:```
1
1 1 1
```Đầu ra đúng là:```
1.732
```Khoảng cách là khoảng`1.73205`, do đó cần làm tròn số. Một chương trình cắt bớt thay vì làm tròn sẽ in`1.732`chỉ vô tình trong trường hợp này và có thể thất bại ở các giá trị trong đó chữ số thập phân thứ tư thay đổi câu trả lời. 

Một trường hợp cạnh khác là một ngôi sao ở gốc tọa độ:```
1
0 0 0
```Đầu ra đúng là:```
0.000
```Nếu quá trình triển khai khởi tạo khoảng cách tối thiểu về 0 thay vì giá trị rất lớn thì mọi khoảng cách dương sẽ bị bỏ qua một cách không chính xác. Giá trị ban đầu phải cho phép ngôi sao đầu tiên thay thế nó. 

Các ngôi sao trùng lặp cũng có thể xảy ra:```
2
5 0 0
5 0 0
```Câu trả lời là:```
5.000
```Một giải pháp phải coi mọi ngôi sao là một ứng cử viên nhưng không cần lưu trữ bất kỳ thông tin bổ sung nào vì chỉ có khoảng cách nhỏ nhất mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính khoảng cách cho mỗi ngôi sao và giữ giá trị nhỏ nhất tìm được. Điều này hiệu quả vì mọi ngôi sao đều độc lập. Không có sự tương tác giữa các điểm đến, vì vậy việc kiểm tra tất cả các ứng viên là đủ để đảm bảo tìm được điểm đến tốt nhất. 

Một cách giải thích mạnh mẽ hơn có thể so sánh từng cặp sao hoặc xây dựng các cấu trúc hình học không cần thiết, nhưng không có thao tác nào trong số đó góp phần đưa ra câu trả lời. Mối quan hệ liên quan duy nhất là giữa mỗi ngôi sao và nguồn gốc. Do đó, số tính toán hữu ích chính xác là một phép tính khoảng cách cho mỗi sao. 

Nhận xét quan trọng là bài toán yêu cầu điểm gần nhất với một điểm cố định. Vì gốc không bao giờ thay đổi nên không cần phải sắp xếp hay tìm kiếm. Một đường chuyền duy nhất là đủ. Chúng ta cũng có thể so sánh khoảng cách bình phương thay vì khoảng cách thực tế vì hàm căn bậc hai đang tăng lên. Nếu như`a < b`, sau đó`sqrt(a) < sqrt(b)`, do đó ngôi sao có khoảng cách bình phương nhỏ nhất cũng có khoảng cách thực nhỏ nhất. 

Lực lượng vũ phu hoạt động vì việc kiểm tra mọi đích đến đảm bảo tính chính xác, nhưng sẽ trở nên lãng phí nếu số lượng sao tăng lên nhiều. Việc quan sát rằng câu trả lời chỉ phụ thuộc vào khoảng cách của mỗi điểm đến gốc sẽ làm giảm giải pháp thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Được chấp nhận, nếu được thực hiện dưới dạng quét | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng sao và chuẩn bị một biến chứa khoảng cách bình phương rất lớn. Giá trị này đại diện cho câu trả lời tốt nhất được tìm thấy cho đến nay trước khi kiểm tra bất kỳ ngôi sao nào. 
2. Với mỗi ngôi sao, hãy tính`x * x + y * y + z * z`. Đây là khoảng cách di chuyển bình phương từ điểm gốc. So sánh các giá trị bình phương sẽ tránh được các phép tính căn bậc hai không cần thiết trong khi vẫn giữ nguyên thứ tự khoảng cách. 
3. Nếu khoảng cách bình phương hiện tại nhỏ hơn mức tối thiểu được lưu trữ, hãy thay thế mức tối thiểu được lưu trữ bằng giá trị này. Giá trị được lưu trữ luôn đại diện cho ngôi sao gần nhất trong số tất cả các ngôi sao được xử lý cho đến nay. 
4. Sau khi tất cả các ngôi sao đã được xử lý, lấy căn bậc hai của khoảng cách bình phương tối thiểu và in ra đúng ba chữ số sau dấu thập phân. 

Tại sao nó hoạt động: sau khi xử lý bất kỳ tiền tố nào của đầu vào, mức tối thiểu được lưu trữ là khoảng cách bình phương nhỏ nhất trong số tất cả các sao trong tiền tố đó. Ngôi sao tiếp theo sẽ trở thành ngôi sao mới gần nhất hoặc giữ nguyên câu trả lời hiện tại. Bất biến này vẫn đúng cho đến khi mọi ngôi sao được kiểm tra, do đó giá trị được lưu trữ cuối cùng tương ứng với tổng thể ngôi sao gần nhất. Chỉ lấy căn bậc hai ở cuối sẽ chuyển khoảng cách bình phương trở lại thời gian di chuyển cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n_line = input().strip()
    if not n_line:
        return

    n = int(n_line)
    best = float('inf')

    for _ in range(n):
        x, y, z = map(int, input().split())
        dist2 = x * x + y * y + z * z
        if dist2 < best:
            best = dist2

    print(f"{math.sqrt(best):.3f}")

if __name__ == "__main__":
    solve()
```Chương trình chỉ giữ một giá trị,`best`, bởi vì danh tính của ngôi sao gần nhất là không liên quan. Vòng lặp cập nhật nó bất cứ khi nào một ứng cử viên tốt hơn xuất hiện, khớp với bất biến từ hướng dẫn thuật toán. 

Việc so sánh sử dụng khoảng cách bình phương. Điều này tránh lặp đi lặp lại các phép tính căn bậc hai dấu phẩy động và giữ cho quá trình quyết định được chính xác vì tất cả các khoảng cách bình phương đều là số nguyên. Hoạt động căn bậc hai xảy ra một lần, sau khi đã tìm thấy mức tối thiểu. 

Định dạng đầu ra sử dụng`:.3f`, thực hiện yêu cầu làm tròn đến ba chữ số thập phân. Việc khởi tạo với vô cực xử lý trường hợp ngôi sao đầu tiên có bất kỳ khoảng cách nào có thể, kể cả số 0. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3
0 1 1
2 0 0
0 -2 0
```việc thực hiện là: 

| Bước | Ngôi sao | Khoảng cách bình phương | Tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | vô cực | 
| 1 | (0,1,1) | 2 | 2 | 
| 2 | (2,0,0) | 4 | 2 | 
| 3 | (0,-2,0) | 4 | 2 | 

Khoảng cách bình phương tối thiểu là 2, vì vậy khoảng cách cuối cùng là`sqrt(2)`, làm tròn thành`1.414`. Điều này chứng tỏ rằng thuật toán giữ lại ứng cử viên tốt nhất và bỏ qua các ngôi sao xa hơn. 

Một ví dụ thứ hai:```
2
0 0 0
10 10 10
```| Bước | Ngôi sao | Khoảng cách bình phương | Tối thiểu hiện tại | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | vô cực | 
| 1 | (0,0,0) | 0 | 0 | 
| 2 | (10,10,10) | 300 | 0 | 

Ngôi sao gần nhất chính là gốc tọa độ, vì vậy đầu ra là`0.000`. Điều này xác nhận rằng khoảng cách bằng 0 được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ngôi sao được viếng thăm đúng một lần và yêu cầu số học theo thời gian không đổi. | 
| Không gian | O(1) | Chỉ có giá trị tối thiểu hiện tại được lưu trữ. | 

Với tối đa 1000 sao, quá trình quét tuyến tính dễ dàng nằm gọn trong giới hạn nhất định. Thuật toán cũng có quy mô tốt vì việc sử dụng bộ nhớ của nó không phụ thuộc vào số lượng sao. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    best = float('inf')

    for _ in range(n):
        x, y, z = map(int, input().split())
        best = min(best, x * x + y * y + z * z)

    return f"{math.sqrt(best):.3f}"

assert solution("""3
0 1 1
2 0 0
0 -2 0
""") == "1.414", "sample 1"

assert solution("""1
0 0 0
""") == "0.000", "origin case"

assert solution("""3
1000 1000 1000
-1000 -1000 -1000
1 1 1
""") == "1.732", "large coordinates and minimum selection"

assert solution("""4
5 0 0
0 5 0
0 0 5
5 0 0
""") == "5.000", "duplicate equal distances"

assert solution("""2
-1000 0 0
0 999 0
""") == "999.000", "boundary coordinate values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngôi sao đơn tại`(0,0,0)`|`0.000`| Khoảng cách tối thiểu có thể bằng 0 | 
| Tọa độ tại`1000`Và`-1000`|`1.732`| Giá trị lớn và so sánh chính xác | 
| Một số khoảng cách gần nhất giống hệt nhau |`5.000`| Thí sinh trùng lặp không ảnh hưởng đến kết quả | 
| Tọa độ âm và biên |`999.000`| Vị trí tuyệt đối quan trọng thông qua khoảng cách bình phương | 

## Vỏ cạnh 

Đối với trường hợp khoảng cách bằng không:```
1
0 0 0
```thuật toán bắt đầu với vô cùng, tính khoảng cách bình phương bằng 0 và thay thế mức tối thiểu được lưu trữ. Căn bậc hai cuối cùng bằng 0 nên đáp án in ra là`0.000`. 

Đối với các ngôi sao trùng lặp:```
2
5 0 0
5 0 0
```ngôi sao đầu tiên đặt khoảng cách bình phương tối thiểu là 25. Ngôi sao thứ hai cũng có khoảng cách bình phương tối thiểu là 25, do đó khoảng cách tối thiểu không thay đổi. Câu trả lời cuối cùng là`5.000`. 

Để làm tròn độ chính xác:```
1
1 1 1
```thuật toán lưu trữ khoảng cách bình phương`3`và tính toán`sqrt(3)`cuối cùng chỉ có một lần. Đầu ra được định dạng làm tròn giá trị một cách chính xác thành`1.732`. 

Đối với tọa độ âm:```
1
-1000 0 0
```khoảng cách bình phương là`1000000`, tương tự như đối với`(1000,0,0)`. Dấu của tọa độ không ảnh hưởng đến khoảng cách vì giá trị được bình phương trước khi so sánh. 

Phiên bản này cũng có thể được điều chỉnh thành ghi chú gửi Codeforces ngắn hơn hoặc định dạng biên tập cuộc thi trang trọng hơn nếu cần.
