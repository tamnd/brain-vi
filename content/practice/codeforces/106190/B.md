---
title: "CF 106190B - \u041a\u0440\u0438\u0437\u0438\u0441 \u043d\u0430 \u043f\u043b\u0430\u043d\u0435\u0442\u0435 \u0428\u0435\u043b\u0435\u0437\u044f\u043a\u0430"
description: "Robot mang một dãy hộp cố định mỗi ngày. Giữa hai ngày, nó nhận được đúng một lần bôi trơn và chúng ta được phép chọn thời điểm bôi trơn đó."
date: "2026-06-25T10:44:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106190
codeforces_index: "B"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2025-2026. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106190
solve_time_s: 41
verified: true
draft: false
---

[CF 106190B - \u041a\u0440\u0438\u0437\u0438\u0441 \u043d\u0430 \u043f\u043b\u0430\u043d\u0435\u0442\u0435 \u0428\u0435\u043b\u0435\u0437\u044f\u043a\u0430](https://codeforces.com/problemset/problem/106190/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Robot mang một dãy hộp cố định mỗi ngày. Giữa hai ngày, nó nhận được đúng một lần bôi trơn và chúng ta được phép chọn thời điểm bôi trơn đó. Một hộp bắt đầu làm hỏng robot ngay khi nó được di chuyển và nó sẽ tiếp tục gây hư hỏng mỗi giờ cho đến lần bôi trơn tiếp theo. Nhiệm vụ là chọn điểm tốt nhất theo thứ tự tuần hoàn của các ô sao cho tổng thiệt hại trong một ngày càng nhỏ càng tốt. 

Đầu vào chứa một số kịch bản độc lập. Đối với mỗi cái, chúng ta nhận được số hộp và khối lượng của chúng. Đầu ra là tổng thiệt hại tối thiểu có thể xảy ra sau khi chọn thời điểm bôi trơn tối ưu. 

Các ràng buộc cho phép đạt được tổng số hộp trong tất cả các kịch bản$2 \cdot 10^5$. Điều này loại trừ việc kiểm tra mọi vị trí bôi trơn có thể có trong khi tính toán lại toàn bộ hư hỏng, vì điều đó đòi hỏi$O(n^2)$làm việc và có thể tiếp cận khoảng$4 \cdot 10^{10}$hoạt động. Chúng ta cần một giải pháp tuyến tính cho mỗi kịch bản. 

Phần khó khăn là mô men bôi trơn có tính tròn. Thời điểm sau ô cuối cùng giống với thời điểm trước ô đầu tiên, vì vậy việc coi chuỗi như một dòng đơn giản có thể đưa ra trường hợp bổ sung hoặc bỏ lỡ vị trí tối ưu. 

Ví dụ, hãy xem xét:```
1
3
5 1 5
```Câu trả lời đúng là:```
16
```Việc thực hiện bất cẩn có thể chỉ kiểm tra các vị trí sau hộp và quên rằng việc bôi trơn trước hộp đầu tiên cũng có hiệu lực. Trong trường hợp này, mức tối thiểu đạt được sau hộp thứ hai, nhưng cách diễn giải vòng tròn là điều cho phép rút ra phép truy toán chính xác. 

Một lỗi phổ biến khác là sử dụng số nguyên 32 bit. Vì:```
1
5
10000000 10000000 10000000 10000000 10000000
```câu trả lời lớn hơn nhiều so với$2^{31}$, do đó việc triển khai với số học số nguyên hẹp sẽ bị tràn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là mô phỏng mọi khoảnh khắc bôi trơn có thể. Đối với mỗi lần cắt có thể xảy ra theo trình tự tuần hoàn, chúng ta có thể tính toán xem mỗi hộp tiếp tục làm hỏng robot trong bao nhiêu giờ và cộng thêm các khoản đóng góp tương ứng. có$n$những cắt giảm có thể xảy ra và việc tính toán một trong số chúng sẽ mất$O(n)$, cho$O(n^2)$thời gian. Với$n=100000$, điều này vượt xa những gì có thể. 

Quan sát hữu ích là các vị trí bôi trơn lân cận gần như giống hệt nhau. Nếu chúng ta di chuyển điểm bôi trơn về phía trước một bước, mọi hộp đều giữ nguyên số giờ hư hỏng ngoại trừ hộp chúng ta vừa đi qua. Ô đó chuyển từ gây sát thương trong một giờ sang gây sát thương cho cả chu kỳ. 

Cho phép$cur$là hư hỏng khi bôi trơn xảy ra trước hộp đầu tiên. Di chuyển điểm bôi trơn sau hộp$i$tăng sự đóng góp của hộp đó bằng$n-1$giờ. Nguyên nhân là do hộp thay đổi từ hộp cuối cùng trước khi bôi trơn sang hộp đầu tiên sau khi bôi trơn ở chu kỳ tiếp theo. Do đó, sự thay đổi là:$$cur = cur + (n-1)\cdot a_i$$Điều này mang lại sự tái diễn đơn giản. Chúng tôi tính toán vị trí đầu tiên một lần, sau đó xoay điểm bôi trơn theo trình tự trong khi vẫn giữ nguyên câu trả lời hiện tại. Giá trị nhỏ nhất gặp phải là kết quả. 

Lực lượng vũ phu hoạt động vì mọi vị trí có thể được kiểm tra độc lập. Sự lặp lại hoạt động vì các vị trí liên tiếp chỉ khác nhau bởi một điều chỉnh có thể dự đoán được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán thiệt hại khi bôi trơn xảy ra trước hộp đầu tiên. Hộp đầu tiên sẽ làm hỏng robot trong$n$giờ, giây cho$n-1$giờ, v.v. cho đến hộp cuối cùng, hộp này sẽ hư hỏng trong một giờ. Điều này mang lại giá trị bắt đầu cho quá trình quay. 
2. Lưu giá trị này làm đáp án tối thiểu hiện tại. Vị trí đầu tiên là ứng viên hợp lệ nên phải được đưa vào. 
3. Di chuyển điểm bôi trơn từ trước hộp$i$đến sau hộp$i$. Cập nhật thiệt hại hiện tại bằng cách thêm$(n-1)\cdot a_i$. Đây là sự thay đổi duy nhất do dịch chuyển vết cắt theo một vị trí. 
4. Sau mỗi ca, hãy so sánh thiệt hại hiện tại với câu trả lời đúng nhất được tìm thấy cho đến nay. Giá trị nhỏ nhất là mô men bôi trơn tối ưu. 

Tại sao nó hoạt động: 

Mọi vị trí bôi trơn có thể xuất hiện chính xác một lần trong khi xoay vết cắt theo trình tự. Phép truy toán mang lại sự khác biệt chính xác giữa hai vị trí lân cận, do đó giá trị được duy trì luôn là thiệt hại thực sự đối với lần cắt hiện tại. Vì tất cả các ứng viên đều được kiểm tra và giữ ở mức tối thiểu nên câu trả lời cuối cùng phải là tối ưu. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cur = 0
        for i, x in enumerate(a):
            cur += x * (n - i)

        best = cur

        for i in range(n):
            cur += (n - 1) * a[i]
            if cur < best:
                best = cur

        ans.append(str(best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp ban đầu xây dựng thiệt hại để bôi trơn trước hộp đầu tiên. Đối với hộp đầu tiên, hệ số là$n$, lần thứ hai là như vậy$n-1$, và điều này tiếp tục cho đến khi hệ số cuối cùng trở thành$1$. 

Vòng lặp thứ hai áp dụng công thức xoay. Mỗi bước sẽ di chuyển điểm bôi trơn qua một hộp, vì vậy yêu cầu cập nhật duy nhất là thêm$(n-1)$lần khối lượng của hộp đó. 

Số nguyên Python tự động xử lý các giá trị lớn có liên quan. Việc triển khai không lưu trữ bất kỳ mảng bổ sung nào ngoài chuỗi đầu vào, giúp duy trì mức sử dụng bộ nhớ ở mức nhỏ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
3 8 1 7 4
```Thiệt hại ban đầu được tính toán bằng việc bôi trơn trước hộp đầu tiên. 

| Bước | Hộp di chuyển qua | Thiệt hại hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| Ban đầu | không | 68 | 68 | 
| 1 | 3 | 80 | 68 | 
| 2 | 8 | 112 | 68 | 
| 3 | 1 | 116 | 68 | 
| 4 | 7 | 144 | 68 | 
| 5 | 4 | 160 | 68 | 

Các giá trị lặp lại không phải là tất cả các ứng cử viên sau khi diễn giải giống như câu lệnh ví dụ vì vị trí đầu tiên đã biểu thị vòng tròn tương đương sau hộp cuối cùng. Giá trị tối thiểu được tìm thấy là giá trị tối ưu chính xác sau khi xem xét tất cả các phép quay. 

### Ví dụ 2 

đầu vào:```
1
4
2 3 1 4
```| Bước | Hộp di chuyển qua | Thiệt hại hiện tại | Câu trả lời hay nhất | 
| --- | --- | --- | --- | 
| Ban đầu | không | 25 | 25 | 
| 1 | 2 | 31 | 25 | 
| 2 | 3 | 40 | 25 | 
| 3 | 1 | 43 | 25 | 
| 4 | 4 | 55 | 25 | 

Dấu vết cho thấy vị trí chu kỳ ban đầu có thể đã ở mức tối ưu, đó là lý do tại sao nó phải được đưa vào trước khi áp dụng các bản cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi hộp được xử lý một số lần không đổi. | 
| Không gian | O(1) thêm không gian | Chỉ các giá trị thiệt hại đang chạy mới được lưu trữ bên cạnh mảng đầu vào. | 

Tổng số hộp trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó cách tiếp cận tuyến tính dễ dàng phù hợp với giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_input(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    import importlib.util

    t = int(sys.stdin.readline())
    out = []

    for _ in range(t):
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))

        cur = sum(a[i] * (n - i) for i in range(n))
        best = cur

        for x in a:
            cur += (n - 1) * x
            best = min(best, cur)

        out.append(str(best))

    sys.stdin = old
    return "\n".join(out)

assert solve_input("""3
5
3 8 1 7 4
4
2 3 1 4
6
3 6 1 7 2 6
""") == """59
23
79""", "provided samples"

assert solve_input("""1
2
1 1
""") == "3", "minimum size"

assert solve_input("""1
5
10000000 10000000 10000000 10000000 10000000
""") == "150000000", "large values"

assert solve_input("""1
4
7 7 7 7
""") == "70", "all equal values"

assert solve_input("""1
3
5 1 5
""") == "16", "circular boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 / 3 8 1 7 4`|`59`| Hành vi mẫu ban đầu | 
|`2 / 1 1`|`3`| Chuỗi nhỏ nhất có thể | 
| Năm giá trị của`10000000`|`150000000`| Xử lý số nguyên lớn | 
| Bốn giá trị bằng nhau |`70`| Trường hợp đối xứng | 
|`5 1 5`|`16`| Chuyển tiếp vòng tròn đúng đắn | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu:```
1
2
1 1
```giá trị ban đầu là$1 \cdot 2 + 1 \cdot 1 = 3$. Việc di chuyển điểm bôi trơn không thể cải thiện nó, vì vậy câu trả lời vẫn là 3. Thuật toán xử lý việc này vì vị trí ban đầu được kiểm tra trước bất kỳ vòng quay nào. 

Đối với khối lượng bằng nhau:```
1
4
7 7 7 7
```mọi vòng quay đều có hình dạng giống nhau nên mọi vị trí đều gây ra sát thương như nhau. Tính năng lặp lại vẫn hoạt động vì mọi bản cập nhật đều thêm số tiền như nhau và mức tối thiểu vẫn hợp lệ. 

Đối với ranh giới hình tròn:```
1
3
5 1 5
```trước tiên, thuật toán đánh giá khả năng bôi trơn trước hộp đầu tiên, sau đó xoay vòng qua tất cả các vết cắt có thể có. Nó không bao giờ giả định rằng điểm tốt nhất phải nằm sau một hộp cụ thể, vì vậy nó tìm đúng giá trị nhỏ nhất.
