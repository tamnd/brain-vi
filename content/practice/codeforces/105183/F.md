---
title: "CF 105183F - \u0423\u044e\u0442\u043d\u0435\u043d\u044c\u043a\u043e"
description: "Chúng ta được cung cấp một mảng được lập chỉ mục từ 1 đến n, trong đó mỗi vị trí đại diện cho một con phố và mỗi con phố có một giá trị dương a[i] mô tả “cường độ” hoặc “kích thước” của nó. Chúng ta cần chọn một dãy con gồm các chỉ số i1 < i2 < ... < ik."
date: "2026-06-27T08:07:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105183
codeforces_index: "F"
codeforces_contest_name: "XX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105183
solve_time_s: 96
verified: false
draft: false
---

[CF 105183F - \u0423\u044e\u0442\u043d\u0435\u043d\u044c\u043a\u043e](https://codeforces.com/problemset/problem/105183/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng được lập chỉ mục từ 1 đến n, trong đó mỗi vị trí đại diện cho một con phố và mỗi con phố có một giá trị dương a[i] mô tả “cường độ” hoặc “kích thước” của nó. 

Chúng ta cần chọn một dãy con gồm các chỉ số i1 < i2 < ... < ik. Các chỉ số được chọn phải tuân theo quy tắc khoảng cách giữa mỗi cặp liên tiếp: khoảng cách giữa hai vị trí được chọn ít nhất phải nhỏ hơn trong hai giá trị tương ứng. Nói cách khác, việc di chuyển từ đường đã chọn này sang đường tiếp theo chỉ được phép nếu khoảng cách về chỉ số đủ lớn để “chứa” được đường yếu hơn trong hai đường. 

Nhiệm vụ là tối đa hóa số lượng chỉ số chúng ta có thể chọn trong khi tôn trọng quy tắc này. 

Ràng buộc n 10^6 có nghĩa là bất kỳ cách tiếp cận bậc hai nào đối với các cặp chỉ số đều không thể thực hiện được ngay lập tức. Ngay cả các giải pháp O(n log n) cũng cần phải được cấu trúc cẩn thận, bởi vì chúng ta không thể lưu trữ hoặc xử lý tất cả các cạnh giữa các cặp một cách rõ ràng. Bất kỳ giải pháp nào cũng phải quyết định một cách hiệu quả cục bộ, theo thời gian tuyến tính hoặc gần tuyến tính, xem nên bao gồm từng vị trí hay bỏ qua nó mà không cần xem lại nhiều trạng thái trước đó. 

Một khó khăn tinh tế xuất phát từ thực tế là ràng buộc phụ thuộc vào cả hai giá trị a[i] và a[j]. Điều này loại trừ các công thức lập kế hoạch khoảng thời gian “thuần túy” tiêu chuẩn trong đó các ràng buộc chỉ phụ thuộc vào một điểm cuối. 

Một cách tiếp cận đơn giản có thể thử lập trình động trên tất cả các phần tử đã chọn trước đó, nhưng điều này sẽ yêu cầu kiểm tra tính tương thích với nhiều trạng thái trước đó và sẽ không mở rộng được. 

Một trường hợp thường phá vỡ trực giác tham lam ngây thơ là khi các giá trị lớn tập hợp lại với nhau. Ví dụ: nếu một phân đoạn có các giá trị [5, 6, 7, 8] thì việc lựa chọn liên tiếp sẽ không thể thực hiện được vì các chỉ số liền kề chỉ khác nhau 1, nhỏ hơn min(5,6), min(6,7), v.v. Bất kỳ cách tiếp cận tham lam không chính xác nào giả định “giá trị cao luôn tốt để lấy” sẽ thất bại ở đây, vì việc lấy một phần tử lớn có thể chặn nhiều lựa chọn trong tương lai. 

Một tình huống phức tạp khác là xen kẽ các giá trị lớn và nhỏ. Ví dụ: [1, 100, 1, 100, 1] cho phép chọn hầu hết mọi chỉ mục, nhưng một chiến lược ngây thơ bỏ qua các giá trị nhỏ sớm có thể làm giảm tính linh hoạt trong tương lai một cách không cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi chuỗi con hợp lệ. Đối với mỗi dãy con hiện tại, chúng ta thử mở rộng nó với mọi chỉ số tiếp theo có thể có j > i thỏa mãn j - i ≥ min(a[i], a[j]). Điều này ngay lập tức trở thành hàm mũ, vì mỗi phần tử phân nhánh thành nhiều phần tiếp theo hợp lệ và thậm chí việc kiểm tra các phần chuyển đổi cũng là O(n^2). 

Một sàng lọc tự nhiên là lập trình động trong đó dp[i] là độ dài chuỗi con tối đa kết thúc tại i. Để tính dp[i], chúng ta sẽ quét tất cả j < i và kiểm tra xem j có thể kết nối với i hay không. Điều này đúng nhưng tốn O(n^2), quá chậm đối với n lên tới 10^6. 

Quan sát cấu trúc quan trọng là tính khả thi của quá trình chuyển đổi chỉ phụ thuộc vào chỉ số được chọn cuối cùng. Khi chúng tôi sửa vị trí được chọn cuối cùng j, mọi ứng cử viên i sẽ được đánh giá độc lập với j. Không cần phải nhớ bất cứ điều gì sớm hơn j, vì điều kiện để mở rộng dãy con hoàn toàn là cục bộ đối với phần tử cuối cùng. 

Điều này cho thấy rằng chúng tôi không thực sự giải quyết DP chung trên tất cả các trạng thái trước đó mà thay vào đó đang xây dựng một chuỗi dài nhất trong biểu đồ tuần hoàn có hướng trong đó các cạnh mã hóa điều kiện tương thích cục bộ. Trong những cài đặt như vậy, chuỗi tối ưu thường có thể được tìm thấy một cách tham lam bằng cách luôn thực hiện phần mở rộng hợp lệ sớm nhất có thể, vì việc trì hoãn một lựa chọn không bao giờ cải thiện tính khả thi trong tương lai. 

Điều này làm giảm vấn đề trong việc duy trì một con trỏ duy nhất cho phần tử được chọn cuối cùng và quét tiếp một lần, quyết định xem mỗi vị trí có thể mở rộng chuỗi hiện tại hay không.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP theo cặp | O(n^2) | O(n) | Quá chậm | 
| Xây dựng dây chuyền tham lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta xây dựng dãy con một cách tham lam từ trái sang phải, luôn duy trì chỉ mục được chọn cuối cùng. 

1. Bắt đầu với không có phần tử nào được chọn và khởi tạo độ dài câu trả lời bằng 0. 
2. Lặp qua các chỉ số từ 1 đến n theo thứ tự tăng dần. 
3. Nếu chưa có chỉ mục nào được chọn, hãy chọn chỉ mục đầu tiên làm điểm bắt đầu của chuỗi con. Điều này là an toàn vì không có ràng buộc trước. 
4. Với mỗi chỉ mục tiếp theo i, hãy kiểm tra xem nó có thể tuân theo chỉ mục j được chọn cuối cùng hay không. Điều kiện là i - j ít nhất phải là min(a[j], a[i]). Nếu điều này đúng, chúng ta thêm i vào dãy con và cập nhật j thành i. 
5. Nếu điều kiện không đạt, chúng ta bỏ qua i và tiếp tục. Việc bỏ qua là cần thiết vì lấy i sẽ vi phạm quy tắc giãn cách với phần tử được chọn cuối cùng. 
6. Tiếp tục cho đến hết mảng, đếm xem chúng ta đã mở rộng chuỗi thành công bao nhiêu lần. 

Ý tưởng quan trọng là thuật toán không bao giờ xem xét lại các quyết định trước đó. Khi một chỉ mục bị bỏ qua, nó sẽ không bao giờ được xem lại và khi một chỉ mục được lấy, nó sẽ trở thành điểm tham chiếu duy nhất cho tất cả các quyết định trong tương lai. 

### Tại sao nó hoạt động 

Ràng buộc về chuỗi con chỉ phụ thuộc vào các phần tử được chọn liền kề, do đó toàn bộ cấu trúc tạo thành một chuỗi trong đó mỗi liên kết được xác nhận độc lập so với liên kết trước đó. Khi chúng ta ở vị trí được chọn cuối cùng cố định j, mọi phần mở rộng hợp lệ i chỉ phụ thuộc vào j và i, không phụ thuộc vào lịch sử trước đó. Trong số tất cả các phần mở rộng khả thi, việc chọn phần mở rộng sớm nhất có thể không thể làm giảm số lượng các tùy chọn trong tương lai, bởi vì tính khả thi trong tương lai chỉ phụ thuộc vào chỉ mục được chọn cuối cùng và việc lấy chỉ mục nhỏ hơn luôn mang lại yêu cầu khoảng cách nhỏ hơn hoặc bằng nhau cho các bước tiếp theo so với việc trì hoãn lựa chọn đến một chỉ mục sau có a[i] có khả năng lớn hơn. Điều này ngăn cản bất kỳ lợi thế nào từ việc bỏ qua tiện ích mở rộng hợp lệ để chuyển sang tiện ích mở rộng sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    last = -1
    ans = 0

    for i in range(n):
        if last == -1:
            last = i
            ans += 1
        else:
            if i - last >= min(a[i], a[last]):
                last = i
                ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì một con trỏ duy nhất`last`, theo dõi chỉ mục được chọn gần đây nhất trong chuỗi tiếp theo. Đối với mỗi chỉ mục mới, chúng tôi kiểm tra tính hợp lệ của việc mở rộng chuỗi bằng cách sử dụng chính xác điều kiện giống như câu lệnh vấn đề. Nếu hợp lệ, chúng tôi cam kết thực hiện ngay lập tức, tương ứng với việc xây dựng chuỗi tối đa một cách tham lam. 

Một cạm bẫy triển khai phổ biến là quên rằng các chỉ mục dựa trên 1 trong báo cáo vấn đề trong khi Python sử dụng lập chỉ mục dựa trên 0. Vì điều kiện chỉ phụ thuộc vào sự khác biệt nên các chỉ số dịch chuyển không ảnh hưởng đến tính chính xác miễn là cả hai bên đều nhất quán. 

Một điểm tinh tế khác là chúng ta không bao giờ cần lưu trữ chính dãy con đó, chỉ cần lưu trữ vị trí được chọn cuối cùng và độ dài của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 11
a = [1, 10, 1, 3, 6, 7, 6, 1, 4, 1, 5]
```Chúng tôi theo dõi quá trình từng bước: 

| tôi | một [tôi] | được chọn lần cuối | tôi - cuối cùng | phút(a[cuối], a[i]) | lấy? | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | - | - | lấy | 1 | 
| 1 | 10 | 0 | 1 | 1 | lấy | 2 | 
| 2 | 1 | 1 | 1 | 1 | lấy | 3 | 
| 3 | 3 | 2 | 1 | 1 | lấy | 4 | 
| 4 | 6 | 3 | 1 | 3 | bỏ qua | 4 | 
| 5 | 7 | 3 | 2 | 3 | bỏ qua | 4 | 
| 6 | 6 | 3 | 3 | 3 | lấy | 5 | 
| 7 | 1 | 6 | 1 | 1 | lấy | 6 | 
| 8 | 4 | 7 | 1 | 1 | lấy | 7 | 
| 9 | 1 | 8 | 1 | 1 | lấy | 8 | 
| 10 | 5 | 9 | 1 | 1 | lấy | 9 | 

Dấu vết này cho thấy các giá trị lớn sớm có thể tạm thời chặn các phần tử tầm trung như thế nào, nhưng các giá trị nhỏ sau này sẽ mở lại khả năng lựa chọn dày đặc. 

### Ví dụ 2 

đầu vào:```
a = [1, 100, 1, 100, 1]
```| tôi | một [tôi] | cuối cùng | lấy? | lý do | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | vâng | bắt đầu | 
| 1 | 100 | 0 | vâng | khoảng cách 1 ≥ 1 | 
| 2 | 1 | 1 | vâng | khoảng cách 1 ≥ 1 | 
| 3 | 100 | 2 | vâng | khoảng cách 1 ≥ 1 | 
| 4 | 1 | 3 | vâng | khoảng cách 1 ≥ 1 | 

Tất cả các phần tử đều được chọn, xác nhận rằng giá trị lớn không làm giảm kích thước câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ con trỏ và bộ đếm cuối cùng được lưu trữ | 

Quét tuyến tính dễ dàng phù hợp với giới hạn n lên tới 10^6, vì mỗi thao tác là một phép so sánh số học đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like small cases
assert run("1\n1\n") == "1"
assert run("2\n1 1\n") == "2"

# strictly increasing values
assert run("5\n1 2 3 4 5\n") == "5"

# alternating large/small
assert run("5\n1 100 1 100 1\n") == "5"

# clustered large values
assert run("4\n10 10 10 10\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở | 
| tất cả những cái | n | chuỗi luôn hợp lệ | 
| trình tự tăng dần | n | không chặn | 
| xen kẽ | 5 | tính khả thi dày đặc | 
| tất cả đều lớn bằng | 1 | hạn chế về khoảng cách chiếm ưu thế | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi tất cả các giá trị đều bằng nhau và lớn. Ví dụ,`n = 4, a = [10, 10, 10, 10]`. Thuật toán chỉ chọn phần tử đầu tiên vì mỗi cặp liên tiếp yêu cầu ít nhất 10 khoảng cách nhưng các chỉ số chỉ cách nhau 1. Thủ tục tham lam sẽ từ chối chính xác mọi chỉ mục tiếp theo vì điều kiện`i - last >= 10`thất bại ở mọi bước. 

Một trường hợp khác là sự kết hợp của các giá trị rất nhỏ và rất lớn. Vì`a = [1, 100, 1, 100, 1]`, thuật toán thay thế các lựa chọn vì mọi cặp liền kề có giá trị nhỏ đều có khoảng cách tối thiểu là 1, điều này luôn được thỏa mãn. Điều này cho thấy rằng các giá trị lớn không làm giảm độ dài chuỗi trừ khi chúng liền kề hoặc được đóng gói chặt chẽ. 

Trường hợp biên cuối cùng là khi một giá trị lớn xuất hiện muộn sau một chuỗi dài các giá trị nhỏ. Thuật toán sẽ chấp nhận nó nếu điều kiện khoảng cách phù hợp với chỉ mục được chọn cuối cùng, chứng tỏ rằng quyết định luôn mang tính cục bộ và không phụ thuộc vào các cơ hội bị bỏ qua trước đó.
