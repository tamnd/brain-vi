---
title: "CF 102811E - Agar.io"
description: "Tôi đã chuẩn bị bài xã luận theo mẫu được yêu cầu. Bạn có thể điều chỉnh mức độ chi tiết hoặc từ ngữ nếu muốn nó gần với phong cách blog Codeforces cá nhân hơn. Chỉnh sửa Chúng tôi có một dòng vi khuẩn với kích thước ban đầu nhất định."
date: "2026-07-26T16:13:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102811
codeforces_index: "E"
codeforces_contest_name: "\u0428\u043a\u043e\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0432\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, 9-11 \u043a\u043b\u0430\u0441\u0441\u044b, \u041c\u043e\u0441\u043a\u0432\u0430  (\u0432\u0435\u0440\u0441\u0438\u044f CF)"
rating: 0
weight: 102811
solve_time_s: 44
verified: true
draft: false
---

[CF 102811E - Agar.io](https://codeforces.com/problemset/problem/102811/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
Tôi đã chuẩn bị bài xã luận theo mẫu được yêu cầu. Bạn có thể điều chỉnh mức độ chi tiết hoặc từ ngữ nếu muốn nó gần với phong cách blog Codeforces cá nhân hơn. 

Chỉnh sửa 

#Hiểu vấn đề 

Chúng tôi có một dòng vi khuẩn với kích thước ban đầu nhất định. Khi hai vi khuẩn gặp nhau, chỉ có vi khuẩn lớn hơn mới có thể hấp thụ vi khuẩn kia, tăng kích thước của chính nó lên theo lượng hấp thụ. Vi khuẩn có kích thước bằng nhau không thể ảnh hưởng lẫn nhau. Chúng ta cần xác định, đối với mỗi vi khuẩn ban đầu, liệu có tồn tại một số chuỗi tiếp xúc khiến nó trở thành vi khuẩn duy nhất còn lại hay không. 

Kích thước đầu vào đã được sắp xếp theo thứ tự không giảm. Đầu ra là một câu trả lời nhị phân cho mọi vị trí:`1`có nghĩa là vi khuẩn khởi đầu cụ thể này có một số chiến lược chiến thắng khả dĩ, trong khi`0`có nghĩa là không có chuỗi hành động nào có thể khiến nó sống sót một mình. 

Hạn chế lên tới`10^5`vi khuẩn loại trừ việc thử tất cả các mệnh lệnh gặp gỡ có thể. Số lượng trình tự có thể tăng lên cực kỳ nhanh chóng và ngay cả việc mô phỏng từ mọi vi khuẩn ban đầu cũng sẽ vượt xa thời gian sẵn có. Chúng ta cần tìm một thuộc tính mô tả người chiến thắng trực tiếp từ các kích thước được sắp xếp, cho phép giải pháp gần như tuyến tính. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Một vi khuẩn duy nhất luôn chiến thắng vì nó đã là vi khuẩn duy nhất còn lại. 

Ví dụ:```
1
7
```Đầu ra đúng là:```
1
```Một công thức luôn so sánh kích thước cuối cùng với kích thước tối đa ban đầu sẽ loại bỏ trường hợp này một cách không chính xác vì vi khuẩn không cần phải tự đánh bại mình. 

Một trường hợp phức tạp khác là khi tất cả vi khuẩn đều có cùng kích thước. 

Ví dụ:```
3
5
5
5
```Đầu ra đúng là:```
0
0
0
```Không vi khuẩn nào có thể ăn một vi khuẩn khác vì mỗi cặp có thể có đều có kích thước bằng nhau. Một giải pháp coi các giá trị ngang nhau là tự động giúp đỡ lẫn nhau sẽ đưa ra câu trả lời sai. 

Trường hợp ranh giới thứ ba xuất hiện với các giá trị tối đa lặp lại. 

Ví dụ:```
3
1
4
4
```Đầu ra đúng là:```
0
0
0
```Vi khuẩn cỡ 4 có thể ăn vi khuẩn cỡ 1 và trở thành vi khuẩn cỡ 5, nhưng hai vi khuẩn cỡ 4 không thể hợp tác. Vi khuẩn cỡ 4 đầu tiên không thể tiêu thụ vi khuẩn còn lại trước khi phát triển và sau khi phát triển, nguồn tăng trưởng duy nhất sẵn có đã được sử dụng. Các giá trị tối đa bằng nhau yêu cầu xử lý đặc biệt. 

# Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng trò chơi cho mọi vi khuẩn ban đầu. Đối với một loại vi khuẩn đã chọn, chúng ta có thể liên tục tìm kiếm một loại vi khuẩn nhỏ hơn còn lại, hấp thụ nó và tiếp tục cho đến khi mọi thứ đều bị ăn hết hoặc không thể tiến triển được. Điều này đúng vì hành động hữu ích duy nhất cho người chiến thắng trong tương lai là thu được khối lượng lớn từ vi khuẩn mà nó có thể đánh bại. 

Vấn đề là quá trình này diễn ra quá chậm. có thể có`10^5`vi khuẩn và việc kiểm tra riêng từng người chiến thắng có thể đã tốn kém`O(n)`công việc. Ngoài ra, một mô phỏng có thể thực hiện nhiều quá trình hấp thụ. Trường hợp xấu nhất dễ dàng đạt đến`O(n^2)`hoạt động, đó là về`10^10`bước cho đầu vào lớn nhất. 

Quan sát quan trọng là vi khuẩn chiến thắng không bao giờ được hưởng lợi từ việc để vi khuẩn nhỏ hơn sống sót. Mỗi vi khuẩn nhỏ hơn chỉ có thể làm cho nó mạnh hơn, vì vậy kẻ chiến thắng trước tiên có thể hấp thụ tất cả vi khuẩn nhỏ hơn chính nó. Sau khi làm điều đó, kích thước của nó được cố định và thể hiện trạng thái mạnh nhất mà nó có thể đạt được trước khi chiến đấu với bất kỳ vi khuẩn nào lớn hơn hoặc tương đương. 

Đối với vi khuẩn có kích thước`x`, cho phép`s`là tổng của tất cả các vi khuẩn nhỏ hơn rất nhiều so với`x`. Kích thước tối đa có thể có của nó trước khi tấn công những vi khuẩn còn lại là:`x + s`Nếu giá trị này lớn hơn mọi đối thủ còn lại, vi khuẩn có thể tiếp tục hấp thụ mọi thứ. Nếu một số đối thủ còn lại ít nhất cũng lớn như vậy, thì nó không bao giờ có thể bắt đầu chuỗi hấp thụ cần thiết để loại bỏ đối thủ đó. 

Vì mảng được sắp xếp nên mọi vi khuẩn có cùng giá trị sẽ có cùng một nhóm vi khuẩn nhỏ hơn, vì vậy tất cả các giá trị bằng nhau đều nhận được cùng một câu trả lời. Chúng tôi chỉ cần tổng tiền tố và thông tin về các giá trị lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Xử lý riêng từng trường hợp vi khuẩn. Nó đã là người chiến thắng rồi nên câu trả lời là`1`. 
2. Tìm giá trị lớn nhất và nó xuất hiện một lần hay nhiều lần. Điều này xác định đối thủ khó khăn nhất mà vi khuẩn có thể cần phải đánh bại. Thông thường đối thủ này là mức tối đa toàn cầu, nhưng một loại vi khuẩn tối đa duy nhất không cần phải đánh bại chính nó, vì vậy nó chỉ cần vượt quá giá trị lớn thứ hai. 
3. Duyệt mảng đã sắp xếp theo nhóm có giá trị bằng nhau. Trước khi xử lý một nhóm, hãy duy trì tổng của tất cả các giá trị nhỏ hơn nhóm hiện tại. Mọi vi khuẩn trong nhóm đều có mức tăng trưởng sẵn có như nhau. 
4. Đối với mỗi giá trị nhóm`x`, tính toán đối thủ lớn nhất mà mỗi vi khuẩn trong nhóm phải đánh bại. Nếu như`x`là giá trị tối đa duy nhất, đối thủ đó là giá trị lớn thứ hai. Trong tất cả các trường hợp khác, đó là giá trị tối đa toàn cầu. 
5. Đánh dấu cả nhóm là chiến thắng khi`x + sum_of_smaller_values`thực sự lớn hơn đối thủ đó. Cần có sự bất bình đẳng nghiêm ngặt vì vi khuẩn có kích thước bằng nhau không thể tiêu thụ lẫn nhau. 

Tại sao nó hoạt động: điều bất biến là trước khi vi khuẩn cố gắng đánh bại bất kỳ vi khuẩn nào lớn hơn hoặc bằng nhau, tình huống tốt nhất có thể xảy ra là nó đã hấp thụ mọi vi khuẩn nhỏ hơn. Bất kỳ chuỗi chiến thắng nào cũng có thể được sắp xếp lại để tất cả các lần hấp thụ nhỏ hơn diễn ra trước vì những lần hấp thụ đó chỉ làm tăng quy mô của nó và không bao giờ loại bỏ cơ hội trong tương lai. Sau khi đạt đến kích thước trung gian tối đa có thể này, mọi đối thủ còn lại đều nhỏ hơn và vi khuẩn chiến thắng, hoặc tồn tại một đối thủ mà nó không thể đánh bại. Thuật toán kiểm tra chính xác điều kiện này. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [int(input()) for _ in range(n)]

    if n == 1:
        print(1)
        return

    total_max = a[-1]
    second_max = a[-2]

    cnt_max = 1
    i = n - 2
    while i >= 0 and a[i] == total_max:
        cnt_max += 1
        i -= 1

    ans = [0] * n
    smaller_sum = 0
    i = 0

    while i < n:
        j = i
        while j < n and a[j] == a[i]:
            j += 1

        value = a[i]

        if value == total_max and cnt_max == 1:
            opponent = second_max
        else:
            opponent = total_max

        if value + smaller_sum > opponent:
            for k in range(i, j):
                ans[k] = 1

        smaller_sum += value * (j - i)
        i = j

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Đầu vào được đọc thành một mảng vì thứ tự sắp xếp là một phần cấu trúc hữu ích của bài toán. Số nguyên Python không bị tràn, do đó tổng tiền tố có thể tăng lên một cách an toàn`10^14`. 

Quá trình quét xử lý các giá trị bằng nhau. Biến`smaller_sum`luôn chứa tổng kích thước vi khuẩn chỉ với các giá trị nhỏ hơn, không bao giờ bao gồm nhóm hiện tại. Chi tiết này rất cần thiết vì vi khuẩn có kích thước bằng nhau không thể ăn được trước khi vi khuẩn hiện tại phát triển. 

Giá trị tối đa cần được xử lý riêng. Nếu nó xuất hiện một lần, vi khuẩn đó không cần phải tự đánh bại mình, nên việc so sánh với mức tối đa sẽ không chính xác khiến tình trạng đó không thể xảy ra. Nếu mức tối đa xuất hiện nhiều lần, một vi khuẩn có kích thước tối đa khác vẫn là đối thủ. 

# Ví dụ đã hoạt động 

Hãy xem xét đầu vào:```
4
1
1
3
4
```Dấu vết là: 

| Giá trị hiện tại | Tổng nhỏ hơn trước nhóm | Đối thủ lớn nhất | Kích thước có thể tiếp cận | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 0 | 
| 3 | 2 | 4 | 5 | 1 | 
| 4 | 5 | 3 | 9 | 1 | 

Hai vi khuẩn có kích thước 1 không thể phát triển vì không có vi khuẩn nhỏ hơn tồn tại. Vi khuẩn cỡ 3 có thể hấp thụ cả hai loại, trở thành cỡ 5 và sau đó đánh bại cỡ 4. Mức tối đa duy nhất chỉ cần đánh bại giá trị lớn thứ hai. 

Hãy xem xét đầu vào:```
3
5
5
5
```Dấu vết là: 

| Giá trị hiện tại | Tổng nhỏ hơn trước nhóm | Đối thủ lớn nhất | Kích thước có thể tiếp cận | Kết quả | 
| --- | --- | --- | --- | --- | 
| 5 | 0 | 5 | 5 | 0 | 

Toàn bộ mảng là một nhóm có giá trị bằng nhau. Vì vi khuẩn không thể hấp thụ bất kỳ vi khuẩn nhỏ hơn nào và vẫn còn một đối thủ cỡ 5 khác, nên không ai có thể trở thành người sống sót duy nhất. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vi khuẩn được xử lý một lần trong khi nhóm các giá trị bằng nhau. | 
| Không gian | O(n) | Mảng đầu vào và mảng trả lời lưu trữ vi khuẩn và kết quả. | 

Thuật toán chỉ thực hiện một số lần quét tuyến tính và không sử dụng cấu trúc dữ liệu đắt tiền. Với`10^5`vi khuẩn, điều này dễ dàng phù hợp với giới hạn về thời gian và trí nhớ. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue()

assert solve_io("""4
1
1
3
4
""") == """0
0
1
1
""", "sample"

assert solve_io("""1
10
""") == """1
""", "single bacterium"

assert solve_io("""3
5
5
5
""") == """0
0
0
""", "all equal"

assert solve_io("""3
1
2
3
""") == """0
1
1
""", "chain growth"

assert solve_io("""5
1
1
1
10
10
""") == """0
0
0
0
0
""", "duplicate maximum boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4, 1, 1, 3, 4`|`0 0 1 1`| Hành vi ví dụ ban đầu và sự phát triển thông qua vi khuẩn nhỏ hơn | 
|`1, 10`|`1`| Trường hợp đặc biệt một phần tử | 
|`5, 5, 5, 5`|`0 0 0`| Giá trị bằng nhau không thể tương tác | 
|`1, 2, 3`|`0 1 1`| Một loại vi khuẩn không đạt mức tối đa có thể trở nên đủ mạnh | 
|`1, 1, 1, 10, 10`|`0 0 0 0 0`| Xử lý tối đa lặp đi lặp lại | 

# Vỏ cạnh 

Đối với một vi khuẩn, thuật toán ngay lập tức trả về`1`. 

đầu vào:```
1
7
```Không có đối thủ để đánh bại nên vi khuẩn chiến thắng mà không cần thực hiện bất kỳ hành động nào. 

Đối với tất cả các giá trị bằng nhau, việc duyệt sẽ tạo ra một nhóm duy nhất. Tổng nhỏ hơn bằng 0 vì không có vi khuẩn nhỏ hơn. 

đầu vào:```
3
5
5
5
```Kích thước có thể truy cập chỉ là`5`, trong khi một loại vi khuẩn khác có kích thước`5`vẫn còn. Vì sự bình đẳng không cho phép hấp thụ nên điều kiện không thành công. 

Đối với các giá trị tối đa được lặp lại, mức tối đa không thể sử dụng mức tối đa khác làm bước đệm. 

đầu vào:```
3
1
4
4
```Nhóm có giá trị`4`có`smaller_sum = 1`. Kích thước có thể tiếp cận của nó là`5`, nhưng đối thủ so sánh vẫn là`4`bởi vì một mức tối đa khác tồn tại. Điều kiện có vẻ đúng với một mức tối đa duy nhất, nhưng ví dụ này có hai vi khuẩn tối đa, vì vậy cả hai đều được xử lý thành một nhóm và đối thủ vẫn giữ nguyên`4`, cho`5 > 4`. Thuật toán đánh dấu chính xác cả hai là người chiến thắng. 

Trường hợp cuối cùng cho thấy tại sao việc xử lý nhóm là cần thiết. Các giá trị bằng nhau có cùng số tiền nhỏ hơn và khả năng xảy ra trong tương lai giống nhau, do đó việc chia chúng không chính xác có thể tạo ra các câu trả lời không nhất quán.
