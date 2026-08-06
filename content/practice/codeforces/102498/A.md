---
title: "CF 102498A - \u041f\u0435\u0440\u0435\u0440\u044b\u0432 \u043d\u0430 \u043e\u0431\u0435\u0434"
description: "Chúng ta có điểm xuất phát, điểm đến và một số nhà hàng có thể có được đặt trên mặt phẳng tọa độ. Người du hành di chuyển với tốc độ không đổi một đơn vị khoảng cách mỗi giây và phải ghé thăm đúng một nhà hàng trước khi đến đích."
date: "2026-08-05T18:19:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102498
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u041f\u0435\u0440\u0432\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102498
solve_time_s: 199
verified: true
draft: false
---

[CF 102498A - \u041f\u0435\u0440\u0435\u0440\u044b\u0432 \u043d\u0430 \u043e\u0431\u0435\u0434](https://codeforces.com/problemset/problem/102498/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có điểm xuất phát, điểm đến và một số nhà hàng có thể có được đặt trên mặt phẳng tọa độ. Người du hành di chuyển với tốc độ không đổi một đơn vị khoảng cách mỗi giây và phải ghé thăm đúng một nhà hàng trước khi đến đích. Việc ghé thăm một nhà hàng sẽ thêm thời gian chờ đợi cố định vì việc ăn uống mất một số giây đã biết. Nhiệm vụ là chọn nhà hàng có tổng thời gian di chuyển nhỏ nhất. 

Đối với một nhà hàng ở`(x_i, y_i)`, tổng thời gian là quãng đường từ điểm xuất phát đến nhà hàng đó, cộng với thời gian ăn uống, cộng với khoảng cách từ nhà hàng đó tới điểm đến. Vì tốc độ di chuyển là một nên khoảng cách Euclide biểu thị trực tiếp thời gian di chuyển. Câu trả lời là mức tối thiểu của giá trị này đối với tất cả các nhà hàng. 

Các tọa độ được giới hạn bởi 1000 về giá trị tuyệt đối, do đó khoảng cách tối đa là vài nghìn đơn vị. Số lượng nhà hàng có thể lên tới 1000. Đây là kích thước đầu vào đủ nhỏ để có thể dễ dàng kiểm tra từng nhà hàng một lần. Một giải pháp thử từng cặp nhà hàng sẽ không cần thiết vì lựa chọn duy nhất là ghé thăm nhà hàng nào. 

Một sai lầm phổ biến là chỉ so sánh khoảng cách trực tiếp tới điểm đến mà bỏ qua thời gian ăn uống. Một nhà hàng xa hơn với thời gian chờ đợi ngắn hơn có thể là lựa chọn tốt nhất. 

Ví dụ:```
0 0 10 0
2
5 0 100
8 0 1
```Nhà hàng đầu tiên cung cấp`5 + 100 + 5 = 110`, trong khi cái thứ hai cho`8 + 1 + 2 = 11`. Chọn nhà hàng gần nhất sẽ cho kết quả sai. 

Một trường hợp khác là khi nhà hàng tốt nhất không nằm giữa điểm xuất phát và điểm đến. Một tuyến đường có thể di chuyển đi trước nếu nhà hàng có thời gian ăn uống đủ nhỏ.```
0 0 5 0
1
0 5 1
```thời gian là`sqrt(25) + 1 + sqrt(50) = 8.071067811865476`. Một giải pháp chỉ xem xét các nhà hàng nằm trên phân khúc trực tiếp sẽ từ chối nhà hàng duy nhất có thể một cách không chính xác. 

Độ chính xác cũng có vấn đề. Tọa độ là số nguyên, nhưng khoảng cách thường không hợp lý. In với định dạng dấu phẩy động thông thường là đủ miễn là phép tính sử dụng số dấu phẩy động và in đủ chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra từng nhà hàng một cách độc lập. Với mỗi người hãy tính khoảng cách từ điểm xuất phát đến nhà hàng, cộng thêm thời gian ăn của nhà hàng, sau đó cộng khoảng cách từ nhà hàng đến đích. Giá trị nhỏ nhất trong số tất cả các nhà hàng chính là câu trả lời. Phương pháp này đúng vì mỗi hành trình hợp lệ phải chọn chính xác một nhà hàng, vì vậy việc kiểm tra mọi lựa chọn có thể bao trùm toàn bộ không gian giải pháp. 

Cách tiếp cận vũ phu đã có cấu trúc giống như giải pháp tối ưu vì không có sự tương tác ẩn giữa các nhà hàng. Không có lý do gì để ghé thăm hai nhà hàng và chi phí để ghé thăm một nhà hàng không phụ thuộc vào những nhà hàng khác. Với`n = 1000`, điều này chỉ cần 1000 phép tính khoảng cách, điều này không đáng kể. 

Quan sát cho thấy mọi nhà hàng đều có thể được đánh giá độc lập cho phép chúng tôi giảm vấn đề xuống một tìm kiếm tối thiểu đơn giản. Phần quan trọng là nhận ra rằng không có vấn đề tìm đường ở đây. Tọa độ của thành phố không chứa chướng ngại vật hoặc hạn chế, vì vậy đường đi ngắn nhất giữa hai điểm bất kỳ luôn là đoạn thẳng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc điểm bắt đầu và điểm đích. Lưu trữ chúng vì mỗi phép tính nhà hàng đều cần hai điểm cuối giống nhau. 
2. Khởi tạo câu trả lời với giá trị rất lớn. Đây là tuyến đường tốt nhất được tìm thấy cho đến nay trước khi bất kỳ nhà hàng nào được kiểm tra. 
3. Đối với mỗi nhà hàng, hãy tính khoảng cách từ điểm bắt đầu đến nhà hàng bằng công thức khoảng cách Euclide. Thêm thời gian ăn của nhà hàng và khoảng cách từ nhà hàng đến điểm đến. 
4. So sánh độ dài tuyến đường này với câu trả lời hiện tại và thay thế câu trả lời nếu nhà hàng này đưa ra tổng thời gian nhỏ hơn. 
5. In giá trị tối thiểu cuối cùng với đủ số thập phân để đáp ứng yêu cầu về độ chính xác. 

Lý do điều này có tác dụng là vì quyết định duy nhất trong toàn bộ vấn đề là lựa chọn nhà hàng. Khi đã chọn được một nhà hàng, cách tối ưu để đến đó và rời đi luôn là đi một đường thẳng vì việc di chuyển không bị hạn chế. Thuật toán kiểm tra mọi lựa chọn có thể chính xác một lần, vì vậy giá trị nhỏ nhất mà nó giữ được là tuyến đường tối ưu thực sự. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    xs, ys, xt, yt = map(int, input().split())
    n = int(input())

    ans = float("inf")

    for _ in range(n):
        x, y, t = map(int, input().split())

        first = math.hypot(xs - x, ys - y)
        second = math.hypot(xt - x, yt - y)

        ans = min(ans, first + t + second)

    print("{:.15f}".format(ans))

if __name__ == "__main__":
    solve()
```Giải pháp trực tiếp theo thuật toán.`math.hypot`được sử dụng thay vì tính toán căn bậc hai theo cách thủ công vì nó thể hiện rõ ràng công thức khoảng cách Euclide và tránh mã trung gian không cần thiết. 

Câu trả lời bắt đầu là vô cùng để nhà hàng đầu tiên luôn thay thế nó. Mỗi nhà hàng được xử lý độc lập và không cần lưu trữ thông tin nhà hàng sau khi tính giá trị của nó, điều này giúp duy trì mức sử dụng bộ nhớ không đổi. 

Thời gian ăn là một số nguyên nhưng được cộng vào khoảng cách dấu phẩy động. Python tự động chuyển đổi kết quả thành giá trị dấu phẩy động, duy trì độ chính xác cần thiết. Không có nguy cơ tràn vì giới hạn tọa độ làm cho tất cả các khoảng cách bình phương trở nên rất nhỏ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
0 0 10 0
1
5 0 3
```| Nhà hàng | Khoảng cách xuất phát | Giờ ăn | Khoảng cách cuối | Tổng cộng | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| (5, 0) | 5 | 3 | 5 | 13 | 13 | 

Con đường khả thi duy nhất là đến nhà hàng ở giữa, dành ba giây ở đó và tiếp tục đến đích. Dấu vết cho thấy thuật toán chỉ cần đánh giá một phương án độc lập. 

Đối với mẫu thứ hai:```
0 -5 0 -3
1
0 5 10
```| Nhà hàng | Khoảng cách xuất phát | Giờ ăn | Khoảng cách cuối | Tổng cộng | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | --- | 
| (0, 5) | 10 | 10 | 8 | 28 | 28 | 

Nhà hàng không nằm giữa điểm bắt đầu và điểm đến. Thuật toán vẫn đánh giá nó vì nó không giả định bất cứ điều gì về vị trí nhà hàng. Điều này xác nhận rằng con đường ngắn nhất có thể liên quan đến việc di chuyển theo bất kỳ hướng nào trước khi đến đích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhà hàng đều được ghé thăm một lần và yêu cầu một lượng số học không đổi. | 
| Không gian | O(1) | Chỉ có nhà hàng hiện tại và câu trả lời tốt nhất hiện tại mới được lưu trữ. | 

Với tối đa 1000 nhà hàng, việc quét tuyến tính thấp hơn nhiều so với giới hạn sẵn có. Giải pháp chỉ thực hiện vài nghìn phép tính số học và sử dụng một lượng bộ nhớ cố định. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import math
    input = sys.stdin.readline

    xs, ys, xt, yt = map(int, input().split())
    n = int(input())

    ans = float("inf")

    for _ in range(n):
        x, y, t = map(int, input().split())
        ans = min(ans,
                  math.hypot(xs - x, ys - y) +
                  t +
                  math.hypot(xt - x, yt - y))

    result = "{:.15f}".format(ans)
    sys.stdin = old_stdin
    return result

assert abs(float(solve_data("""0 0 10 0
1
5 0 3
""")) - 13.0) < 1e-9

assert abs(float(solve_data("""0 -5 0 -3
1
0 5 10
""")) - 28.0) < 1e-9

assert abs(float(solve_data("""0 0 5 5
2
3 3 2
3 4 1
""")) - 8.23606797749979) < 1e-9

assert abs(float(solve_data("""0 0 0 0
1
0 0 1
""")) - 1.0) < 1e-9

assert abs(float(solve_data("""0 0 10 0
2
5 0 100
8 0 1
""")) - 11.0) < 1e-9

assert abs(float(solve_data("""1000 1000 -1000 -1000
1
1000 -1000 1000
""")) - (2000 * math.sqrt(2) + 1000)) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bắt đầu bằng đích đến và nhà hàng là cùng một điểm | 1 | Xử lý khoảng cách di chuyển bằng 0 một cách chính xác. | 
| Hai nhà hàng càng xa càng tốt | 11 | Ngăn chặn việc chỉ chọn theo khoảng cách hình học. | 
| Tọa độ ở giới hạn tối đa | Giá trị dấu phẩy động lớn | Xác nhận xử lý chính xác tại tọa độ biên. | 

## Vỏ cạnh 

Trường hợp lợi ích đầu tiên từ việc hiểu vấn đề là sự hiện diện của một nhà hàng gần đó nhưng hoạt động chậm. Vì:```
0 0 10 0
2
5 0 100
8 0 1
```thuật toán đánh giá cả hai lựa chọn. Cái đầu tiên cho`110`, thứ hai cho`11`, do đó mức tối thiểu được lưu trữ trở thành`11`. Vì thời gian ăn uống được bao gồm trong biểu thức giống như khoảng cách nên thuật toán không rơi vào bẫy chọn nhà hàng gần nhất. 

Trường hợp cạnh thứ hai là một nhà hàng nằm ngoài đường dẫn trực tiếp:```
0 0 5 0
1
0 5 1
```Thuật toán tính khoảng cách đến nhà hàng là`5`, thêm thời gian ăn`1`, và thêm khoảng cách cuối cùng`sqrt(50)`. Nó không bao giờ lọc các nhà hàng theo vị trí của họ, do đó nó trả về chính xác khoảng`8.071067811865476`. 

Trường hợp cạnh cuối cùng là khi khoảng cách không phải là số nguyên. Ví dụ: mẫu có chuyển động chéo chứa căn bậc hai. Thuật toán giữ tất cả các phép tính dưới dạng giá trị dấu phẩy động và in mười lăm chữ số sau dấu thập phân, đủ để đáp ứng dung sai lỗi yêu cầu.
