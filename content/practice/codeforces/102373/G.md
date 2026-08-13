---
title: "CF 102373G - \u041d\u043e\u0436\u043d\u0438\u0446\u044b"
description: "Chúng ta có một tấm hình chữ nhật được chia thành các ô đơn vị n × m. Bill chỉ cắt dọc theo các đường lưới và đi theo một đường xoắn ốc rẽ phải cố định. Việc cắt bắt đầu cách một ô từ ranh giới bên trái, sau đó tiếp tục đi lên cho đến khi việc mở rộng nó bằng một đơn vị khác sẽ ngắt kết nối hình còn lại."
date: "2026-08-12T23:01:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102373
codeforces_index: "G"
codeforces_contest_name: "\u0426\u0438\u043a\u043b \u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434 \u0434\u043b\u044f \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102373
solve_time_s: 350
verified: true
draft: false
---

[CF 102373G - \u041d\u043e\u0436\u043d\u0438\u0446\u044b](https://codeforces.com/problemset/problem/102373/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có một tấm hình chữ nhật được chia thành`n × m`ô đơn vị. Bill chỉ cắt dọc theo các đường lưới và đi theo một đường xoắn ốc rẽ phải cố định. Việc cắt bắt đầu cách một ô từ ranh giới bên trái, sau đó tiếp tục đi lên cho đến khi việc mở rộng nó bằng một đơn vị khác sẽ ngắt kết nối hình còn lại. Hướng thay đổi theo chiều kim đồng hồ và quy tắc tương tự được áp dụng sau mỗi lượt. Chúng tôi cần tổng chiều dài của mỗi đoạn đơn vị được cắt trong toàn bộ quá trình. 

Kích thước có thể lớn như`10^9`. Điều đó ngay lập tức loại trừ bất cứ điều gì cấu trúc trang tính một cách rõ ràng, bởi vì trang tính đó có thể chứa tối đa`10^18`tế bào. Ngay cả một thuật toán thực hiện công việc liên tục cho mỗi ô cũng sẽ cần khoảng`10^18`hoạt động, vượt xa giới hạn hai giây. Giải pháp phải sử dụng hình học của đường xoắn ốc thay vì mô phỏng nó. 

Các trường hợp cạnh nguy hiểm nhất là các hình chữ nhật mỏng. Vì`2 × 2`, câu trả lời là`1`, không`2`hoặc`4`, vì chỉ có thể cắt một đơn vị trước khi hình còn lại không thể kéo dài được nữa. Vì`2 × 3`, câu trả lời là`2`, vì công thức cho`(2 - 1)(3 - 1) = 2`. Mô phỏng dựa trên việc đếm các lượt hoàn chỉnh có thể dễ dàng mắc lỗi từng lượt một ở đây vì lượt cuối cùng ngắn hơn các lượt trước đó. Phân tích chính thức xác nhận rằng câu trả lời chính xác là`(n - 1)(m - 1)`. 

Một trường hợp cạnh hữu ích khác là hình vuông như`3 × 3`. Đường xoắn ốc đối xứng, nhưng câu trả lời là`4`, không phải chu vi hoặc số lượng ô. Đại lượng liên quan là số vị trí bên trong được thể hiện bằng vết cắt sau khi biến đổi hình học được mô tả dưới đây. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể mô phỏng từng ô một. Chúng ta có thể duy trì hình chữ nhật hiện tại, đi theo đường xoắn ốc và thêm một vào đáp án cho mỗi đoạn đơn vị của vết cắt. Điều này đúng vì mỗi lần cắt bao gồm các phân đoạn lưới đơn vị, do đó, rõ ràng theo đường xoắn ốc cuối cùng sẽ tính chính xác độ dài cần thiết. 

Vấn đề là số lượng hoạt động. Tổng chiều dài có thể là`(n - 1)(m - 1)`, cái nào cho`n = m = 10^9`bằng`999999998000000001`. Do đó, một mô phỏng sẽ cần theo thứ tự`10^18`lặp lại trong trường hợp xấu nhất. Việc lưu trữ lưới thậm chí còn rõ ràng hơn là không thể, vì nó sẽ yêu cầu tới`10^18`tế bào. 

Quan sát hữu ích là ngừng suy nghĩ về các vết cắt như những đường thẳng và thay vào đó hãy nhìn vào lưới phẳng từ phối cảnh kép của nó. Thay thế các ô ban đầu bằng các ranh giới lưới tương ứng và thay thế các ranh giới ban đầu bằng các ô trong khi vẫn duy trì tính liền kề. Theo phép biến đổi này, mỗi đoạn đơn vị của phần cắt ban đầu tương ứng với chính xác một ô không biên trong hình chữ nhật được biến đổi. 

Hình chữ nhật được chuyển đổi có thêm một hàng và một cột bổ sung so với kích thước ban đầu. Các ô ranh giới của nó tương ứng với ranh giới bên ngoài của trang tính ban đầu, nơi không thể thực hiện cắt được. Những gì còn lại là một`(n - 1) × (m - 1)`hình chữ nhật nội thất. 

Đường xoắn ốc rẽ phải ban đầu trở thành đường xoắn ốc bình thường đi qua tất cả các ô bên trong đó. Mỗi ô bên trong được truy cập chính xác một lần và mỗi ô được truy cập đại diện cho một đơn vị cắt ban đầu. Do đó tổng chiều dài cắt chỉ đơn giản là số lượng tế bào bên trong,`(n - 1)(m - 1)`. Đây chính xác là đối số hình học được sử dụng trong hướng dẫn chính thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nm)`|`O(nm)`hoặc`O(1)`tùy thuộc vào mô phỏng | Quá chậm | 
| Tối ưu |`O(1)`|`O(1)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước`n`Và`m`của tấm hình chữ nhật ban đầu. 
2. Giải thích đường xoắn ốc qua lưới kép. Ranh giới bên ngoài của tấm ban đầu trở thành lớp ranh giới của hình chữ nhật được chuyển đổi, do đó các vị trí đó không thể tương ứng với các vết cắt. 
3. Loại bỏ lớp ranh giới đó khỏi phần xem xét. Vì hình chữ nhật được biến đổi có kích thước`(n + 1) × (m + 1)`, loại bỏ một ô ở mỗi bên lá`(n - 1) × (m - 1)`tế bào bên trong. 
4. Đếm các ô bên trong. Mỗi cái tương ứng một cách khách quan với một đoạn đơn vị của đường cắt xoắn ốc ban đầu, do đó tổng chiều dài cần thiết là`(n - 1) × (m - 1)`. 
5. In sản phẩm. 

### Tại sao nó hoạt động 

Bất biến chính là sự tương ứng giữa các đoạn cắt đơn vị trong lưới ban đầu và các ô bên trong trong lưới kép. Phép toán xoắn ốc duy trì sự tương ứng này vì các phần liền kề của vết cắt ban đầu trở thành các ô liền kề trong biểu diễn được biến đổi. Quy tắc dừng ở một ngã rẽ chính là điều ngăn cản hình xoắn ốc đi vào lớp ranh giới hoặc ngắt kết nối hình còn lại. Do đó, đường xoắn ốc đã biến đổi sẽ ghé thăm mọi ô bên trong hợp lệ một lần và chỉ một lần. Có chính xác`(n - 1)(m - 1)`các ô như vậy, do đó tổng chiều dài của vết cắt ban đầu bằng nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    print((n - 1) * (m - 1))

if __name__ == "__main__":
    solve()
```Đầu vào bao gồm một cặp số nguyên, do đó không có vòng lặp test-case. biểu hiện`(n - 1) * (m - 1)`trực tiếp thực hiện kết quả thu được từ đối số lưới kép. 

Phép trừ phải xảy ra trước phép nhân. sử dụng`n * m - 1`hoặc`(n - 1) * m`sẽ tính một vị trí biên không tương ứng với một vết cắt và tạo ra các câu trả lời sai ngay cả trên các hình chữ nhật nhỏ. 

Số nguyên Python có độ chính xác tùy ý, do đó sản phẩm tối đa được xử lý mà không cần bất kỳ sự chăm sóc đặc biệt nào. Trong ngôn ngữ có loại số nguyên có chiều rộng cố định, ở đây số nguyên có dấu 64 bit là đủ vì câu trả lời tối đa là bên dưới`10^18`, nhưng Python không yêu cầu lựa chọn loại rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đối với một`3 × 3`trang tính, hình chữ nhật được biến đổi có kích thước`4 × 4`. Ranh giới bên ngoài của nó đại diện cho ranh giới trang tính ban đầu, do đó chỉ có`2 × 2`nội thất vẫn có liên quan. 

|`n`|`m`| Hàng nội thất | Cột nội thất | Trả lời | 
| --- | --- | --- | --- | --- | 
| 3 | 3 | 2 | 2 | 4 | 

Bốn ô bên trong tương ứng với bốn đoạn đơn vị của đường xoắn ốc. Vậy tổng chiều dài cắt là`4`, phù hợp với mẫu 

### Mẫu 2 

Đối với một`3 × 4`trang tính, hình chữ nhật được biến đổi có kích thước`4 × 5`. Sau khi loại bỏ lớp ranh giới của nó, có`2 × 3`các tế bào bên trong có liên quan. 

|`n`|`m`| Hàng nội thất | Cột nội thất | Trả lời | 
| --- | --- | --- | --- | --- | 
| 3 | 4 | 2 | 3 | 6 | 

Đường xoắn ốc đi qua tất cả sáu vị trí bên trong nên sáu đơn vị chiều dài bị cắt đi. Điều này mang lại`(3 - 1)(4 - 1) = 6`, một lần nữa phù hợp với mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(1)`| Chỉ cần hai phép trừ, một phép nhân và một thao tác đầu ra. | 
| Không gian |`O(1)`| Thuật toán chỉ lưu trữ hai chiều đầu vào và kết quả. | 

Các ràng buộc cho phép kích thước lên tới`10^9`, vì vậy một`O(nm)`việc xây dựng có thể yêu cầu khoảng`10^18`hoạt động. Công thức thời gian không đổi hoàn toàn tránh được sự phụ thuộc vào kích thước của trang tính và phù hợp thoải mái với giới hạn thời gian hai giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    n, m = map(int, data)
    return str((n - 1) * (m - 1))

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Provided samples
assert run("3 3\n") == "4", "sample 1"
assert run("3 4\n") == "6", "sample 2"

# Minimum-size rectangle
assert run("2 2\n") == "1", "minimum valid dimensions"

# Thin rectangle, catches final-turn off-by-one errors
assert run("2 3\n") == "2", "2 by 3 boundary case"

# Equal large values
assert run("1000000000 1000000000\n") == "999999998000000001", \
    "maximum equal dimensions"

# Maximum dimension combined with the minimum dimension
assert run("2 1000000000\n") == "999999999", \
    "maximum thin rectangle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`|`1`| Tờ pháp lý nhỏ nhất và đường xoắn ốc không tầm thường đầu tiên | 
|`2 3`|`2`| Hình chữ nhật mỏng và xử lý ranh giới ở ngã rẽ cuối cùng | 
|`1000000000 1000000000`|`999999998000000001`| Giá trị tối đa và số học số nguyên lớn | 
|`2 1000000000`|`999999999`| Kích thước tối đa kết hợp với tấm mỏng nhất có thể | 

## Vỏ cạnh 

cho`2 × 2`, công thức cho`(2 - 1)(2 - 1) = 1`. Hình chữ nhật được biến đổi có`1 × 1`bên trong, do đó chính xác một ô bên trong tương ứng với một đơn vị cắt. Một mô phỏng giả định mọi hướng luôn có ít nhất một phân đoạn bổ sung đầy đủ có thể đếm không chính xác nhiều hơn một phân đoạn. 

Vì`2 × 3`, kết quả là`(2 - 1)(3 - 1) = 2`. Hình chữ nhật được biến đổi có`1 × 2`nội thất. Vòng xoắn ốc có thể ghé thăm cả hai vị trí hợp lệ, nhưng không có vị trí thứ ba sau lượt cuối cùng. Đây là trường hợp điển hình cho thấy từng lỗi một trong mô phỏng từng chặng. 

Vì`3 × 3`, kết quả là`(3 - 1)(3 - 1) = 4`. Hình chữ nhật được biến đổi có`2 × 2`bên trong, do đó bốn vị trí tương ứng với các đoạn cắt. Điều này xác nhận rằng câu trả lời tính các vị trí kép bên trong chứ không phải các ô giấy gốc hoặc chu vi. 

Vì`1000000000 × 1000000000`, câu trả lời là`999999999 × 999999999 = 999999998000000001`. Kết quả lớn nhưng vẫn được thể hiện chính xác bằng kiểu số nguyên của Python. Thuật toán không bao giờ phân bổ một lưới và thực hiện cùng một lượng công việc không đổi như đối với một lưới.`2 × 2`đầu vào.
