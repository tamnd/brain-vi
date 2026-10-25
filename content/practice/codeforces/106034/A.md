---
title: "CF 106034A - \u0422\u0435\u043b\u0435\u0444\u043e\u043d\u043d\u044b\u0435 \u043d\u043e\u043c\u0435\u0440\u0430"
description: "Vấn đề là so sánh các số điện thoại được viết bằng một số định dạng thân thiện với con người. Vasya muốn thêm một số mới vào danh bạ của mình nhưng anh ấy cần biết liệu mỗi số trong số ba bản ghi hiện có có đề cập đến cùng một số điện thoại thực tế hay không."
date: "2026-06-25T13:01:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106034
codeforces_index: "A"
codeforces_contest_name: "ICPC Central Russia Regional Qualification Round, 2024"
rating: 0
weight: 106034
solve_time_s: 45
verified: true
draft: false
---

[CF 106034A - \u0422\u0435\u043b\u0435\u0444\u043e\u043d\u043d\u044b\u0435 \u043d\u043e\u043c\u0435\u0440\u0430](https://codeforces.com/problemset/problem/106034/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Vấn đề là so sánh các số điện thoại được viết bằng một số định dạng thân thiện với con người. Vasya muốn thêm một số mới vào danh bạ của mình nhưng anh ấy cần biết liệu mỗi số trong số ba bản ghi hiện có có đề cập đến cùng một số điện thoại thực tế hay không. 

Số điện thoại bao gồm mã vùng/mã nhà mạng gồm ba chữ số và số thuê bao gồm bảy chữ số. Mã có thể được viết rõ ràng hoặc được bỏ qua, trong trường hợp đó nó phải được coi là`495`. Tiền tố quốc gia có thể xuất hiện dưới dạng`+7`hoặc`8`, và dấu phân cách với`-`có thể được chèn vào giữa bất kỳ chữ số nào. Dấu ngoặc đơn xung quanh mã cũng được cho phép. 

Đầu vào chứa một số điện thoại mà Vasya muốn thêm, theo sau là ba số điện thoại đã được lưu trữ. Đối với mỗi số được lưu trữ, chúng ta cần xuất ra`YES`nếu nó thể hiện chính xác mã và số thuê bao giống như số mới, nếu không thì xuất ra`NO`. 

Các ràng buộc rất nhỏ vì mỗi số chỉ chứa tối đa vài chục ký tự. Điều này có nghĩa là chúng ta không cần cấu trúc dữ liệu nâng cao hoặc thuật toán chuỗi nặng. Quét tuyến tính các ký tự là đủ và thậm chí thực hiện việc đó một vài lần cũng thấp hơn nhiều so với giới hạn có sẵn. Phần quan trọng không phải là hiệu suất mà là chuẩn hóa chính xác tất cả các định dạng có thể thành một biểu diễn chung. 

Các trường hợp phức tạp đến từ nhiều cách khác nhau mà cùng một con số có thể xuất hiện. Ví dụ:```
Input:
8(495)430-23-97
+7-4-9-5-43-023-97
4-3-0-2-3-9-7
```Hai số đầu tiên đại diện cho cùng một số điện thoại, bởi vì`8`Và`+7`tương đương và mã là`495`. Số thứ ba không có mã số nên mã số của nó trở thành`495`, và nó cũng tương tự. Giải pháp chỉ loại bỏ dấu gạch ngang nhưng giữ nguyên tiền tố sẽ phân tách các trường hợp này một cách không chính xác. 

Một trường hợp đặc biệt khác là khi thiếu mã:```
Input:
4302397
84954302397
```Đầu ra đúng là`YES`, vì số đầu tiên nên được hiểu là`4954302397`, và cái thứ hai cũng là`4954302397`sau khi chuyển đổi`8`tiền tố. Việc triển khai bất cẩn có thể chỉ so sánh các chữ số hiển thị và bỏ sót mã ẩn. 

Một lỗi khác xuất hiện khi số sau khi loại bỏ ký hiệu có 11 chữ số:```
Input:
84951234567
1234567
```Số thứ hai không chỉ có bảy chữ số cuối. Nó cũng có mã mặc định`495`, vậy so sánh đúng là giữa`4951234567`Và`4951234567`, không phải giữa các chuỗi không đầy đủ. Việc bỏ qua mã mặc định sẽ tạo ra kết quả âm tính giả. 

# Phương pháp tiếp cận 

Cách tiếp cận đơn giản là cố gắng so sánh trực tiếp các chuỗi sau khi loại bỏ dấu phân cách. Điều này thật hấp dẫn vì đầu vào rất nhỏ. Chúng ta có thể xóa mọi`-`, xóa dấu ngoặc đơn rồi kiểm tra xem các chuỗi còn lại có khớp không. Điều này đúng trong một số trường hợp vì định dạng ký tự không ảnh hưởng đến số thực. 

Vấn đề là biểu diễn hiển thị không phải là dữ liệu thực. Cùng một số điện thoại có thể có các tiền tố khác nhau và có thể bỏ qua mã. Cách tiếp cận bạo lực sẽ cần mô phỏng mọi cách giải thích có thể có của định dạng đầu vào. Chỉ với một vài ký tự, điều này vẫn có thể thực hiện được, nhưng nó phức tạp không cần thiết và có thể mắc lỗi. 

Điều quan trọng cần lưu ý là mọi bản ghi điện thoại hợp lệ đều mô tả chính xác hai thông tin giống nhau: mã gồm ba chữ số và số thuê bao bảy chữ số. Nếu chúng ta chuyển đổi mọi chuỗi đầu vào thành hai phần đó thì việc so sánh sẽ trở nên tầm thường. 

Sau khi loại bỏ các ký hiệu định dạng, các chữ số còn lại có một trong hai hình dạng. Nếu độ dài là bảy, mã đã bị bỏ qua và chúng tôi thêm vào trước`495`. Nếu độ dài là mười một thì chữ số đầu tiên là tiền tố quốc gia (`8`) và ba chữ số tiếp theo là mã. Bảy chữ số cuối cùng luôn là số thuê bao. Sau đó có thể sử dụng biểu diễn mười chữ số được chuẩn hóa tương tự cho tất cả các phép so sánh. 

Ý tưởng brute-force hoạt động vì đầu vào nhỏ, nhưng nó thất bại về mặt khái niệm vì nó so sánh các biểu diễn thay vì ý nghĩa. Bước chuẩn hóa chuyển đổi vấn đề từ khớp chuỗi với nhiều trường hợp thành một phép kiểm tra đẳng thức đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) mỗi lần so sánh | O(k) | Được chấp nhận nhưng dễ bị lỗi | 
| Tối ưu | O(k) mỗi số | O(k) | Đã chấp nhận | 

Đây`k`là độ dài của chuỗi điện thoại. Từ`k`bị giới hạn bởi định dạng đầu vào nhỏ, cả hai đều nhanh, nhưng chuẩn hóa là giải pháp rõ ràng. 

## Hướng dẫn thuật toán 

1. Đọc số điện thoại mà Vasya muốn thêm và chuyển đổi nó thành dạng chuẩn hóa. Dạng chuẩn hóa sẽ là một chuỗi mười chữ số chứa mã ba chữ số, theo sau là số thuê bao bảy chữ số. 
2. Để chuẩn hóa một số, trước tiên hãy xóa tất cả các ký tự không có chữ số. Sau bước này, chỉ còn lại các chữ số có ý nghĩa. 
3. Nếu chuỗi đã làm sạch có bảy chữ số, hãy thêm`495`đến đầu. Điều này xử lý các số mà mã không được viết rõ ràng. 
4. Nếu chuỗi đã làm sạch có 11 chữ số, hãy bỏ phần đầu`8`hoặc`7`đại diện cho quốc gia và giữ mười chữ số còn lại. Ba chữ số đầu tiên trong số đó là mã và bảy chữ số cuối cùng là số thuê bao. 
5. Chuẩn hóa từng số điện thoại trong số ba số điện thoại được lưu theo cách tương tự. 
6. So sánh mọi số được lưu trữ đã chuẩn hóa với số mục tiêu đã chuẩn hóa. In`YES`khi chúng bằng nhau và`NO`nếu không thì. 

Lý do điều này có tác dụng là vì việc chuẩn hóa sẽ loại bỏ mọi khác biệt chỉ liên quan đến vấn đề định dạng. Sau khi chuyển đổi, hai số bằng nhau khi mã thực và số thuê bao của chúng bằng nhau. 

Điều bất biến là mỗi chuỗi chuẩn hóa đại diện cho một và chỉ một số điện thoại thực tế. Trong quá trình thuật toán, chúng tôi không bao giờ so sánh các cách biểu diễn không đầy đủ hoặc được định dạng, do đó các cách viết khác nhau của cùng một số luôn thu gọn về cùng một giá trị. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(s):
    digits = ''.join(c for c in s if c.isdigit())

    if len(digits) == 7:
        return '495' + digits

    if digits[0] in '78':
        digits = digits[1:]

    return digits

def solve():
    target = normalize(input().strip())

    ans = []
    for _ in range(3):
        cur = normalize(input().strip())
        ans.append("YES" if cur == target else "NO")

    print('\n'.join(ans))

if __name__ == "__main__":
    solve()
```các`normalize`chức năng là toàn bộ ý tưởng của giải pháp. Đầu tiên nó loại bỏ dấu gạch ngang và dấu ngoặc đơn bằng cách chỉ giữ lại các ký tự chữ số. Định dạng điện thoại đảm bảo rằng chuỗi còn lại có bảy hoặc mười một chữ số. 

Trường hợp bảy chữ số được xử lý bằng cách chèn mã mặc định. Đối với trường hợp 11 chữ số, chữ số đầu tiên chỉ là tiền tố quốc gia nên được loại bỏ trước khi so sánh. 

Giai đoạn so sánh được cố ý đơn giản. Tất cả các quyết định định dạng đã được giải quyết, vì vậy sự bình đẳng của chuỗi cũng là sự bình đẳng của số điện thoại. 

Không có rủi ro lập chỉ mục từ các vị trí cố định trong chuỗi gốc vì dấu phân cách có thể xuất hiện ở bất kỳ đâu. Mã chỉ dựa vào chuỗi chữ số đã được làm sạch, trong đó định dạng được đảm bảo. 

# Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8(495)430-23-97
+7-4-9-5-43-023-97
4-3-0-2-3-9-7
8-495-430
```| Bước | Số | Chữ số sạch | Chuẩn hóa | So sánh | 
| --- | --- | --- | --- | --- | 
| Mục tiêu | 8(495)430-23-97 | 84954302397 | 4954302397 | Giá trị cơ bản | 
| 1 | +7-4-9-5-43-023-97 | 74954302397 | 4954302397 | CÓ | 
| 2 | 4-3-0-2-3-9-7 | 4302397 | 4954302397 | CÓ | 
| 3 | 8-495-430 | 8495430 | 4954302397 | KHÔNG | 

Điều này chứng tỏ tại sao mã mặc định phải được thêm vào khi nó bị thiếu. 

### Mẫu 2 

đầu vào:```
4302397
84954302397
84954302397
+7(495)4302397
```| Bước | Số | Chữ số sạch | Chuẩn hóa | So sánh | 
| --- | --- | --- | --- | --- | 
| Mục tiêu | 4302397 | 4302397 | 4954302397 | Giá trị cơ bản | 
| 1 | 84954302397 | 84954302397 | 4954302397 | CÓ | 
| 2 | 84954302397 | 84954302397 | 4954302397 | CÓ | 
| 3 | +7(495)4302397 | 74954302397 | 4954302397 | CÓ | 

Điều này xác nhận rằng các tiền tố và định dạng khác nhau tạo ra cùng một biểu diễn chuẩn hóa. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi số điện thoại được quét một lần để xóa ký tự định dạng | 
| Không gian | O(k) | Một chuỗi được làm sạch và chuỗi chuẩn hóa được lưu trữ | 

Độ dài tối đa của chuỗi đầu vào là rất nhỏ, do đó giải pháp dễ dàng nằm gọn trong giới hạn. Thuật toán chỉ thực hiện một số bước tuyến tính và tránh việc xử lý trường hợp không cần thiết. 

# Trường hợp thử nghiệm```python
import sys
import io

def solve(inp):
    data = inp.strip().split('\n')

    def normalize(s):
        digits = ''.join(c for c in s if c.isdigit())
        if len(digits) == 7:
            return '495' + digits
        if digits[0] in '78':
            digits = digits[1:]
        return digits

    target = normalize(data[0])
    res = []

    for s in data[1:4]:
        res.append("YES" if normalize(s) == target else "NO")

    return '\n'.join(res)

# sample 1
assert solve("""8(495)430-23-97
+7-4-9-5-43-023-97
4-3-0-2-3-9-7
8-495-430""") == "YES\nYES\nNO"

# sample 2
assert solve("""4302397
84954302397
84954302397
+7(495)4302397""") == "YES\nYES\nYES"

# all default code cases
assert solve("""1234567
4951234567
8(495)1234567
+7-495-123-4567""") == "YES\nYES\nYES"

# different codes
assert solve("""84991234567
84981234567
84991234567
91234567""") == "NO\nYES\nNO"

# minimal formatting
assert solve("""1234567
1-2-3-4-5-6-7
8(495)1234567
+7-495-1234567""") == "YES\nYES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thiếu mã có dấu phân cách | CÓ CÓ CÓ CÓ | Mặc định`495`xử lý | 
| Mã rõ ràng khác nhau | Câu trả lời hỗn hợp | So sánh mã | 
| Chỉ chữ số | CÓ | Xử lý đầu vào sạch sẽ | 
| Các biến thể tiền tố | CÓ |`8`Và`+7`tương đương | 

# Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên, hãy xem xét:```
4302397
4954302397
```Số đầu tiên được làm sạch thành`4302397`. Thuật toán phát hiện bảy chữ số và tạo`4954302397`. Số thứ hai được làm sạch và tiền tố bị loại bỏ, để lại cùng một giá trị chuẩn hóa, vì vậy câu trả lời là`YES`. 

Đối với vấn đề tiền tố:```
+7(495)4302397
84954302397
```Số đầu tiên trở thành`74954302397`sau khi loại bỏ các ký hiệu và cái thứ hai trở thành`84954302397`. Trong cả hai trường hợp, chữ số quốc gia đứng đầu đều bị xóa, để lại`4954302397`, vậy là so sánh thành công. 

Đối với mã sai:```
84991234567
84981234567
```Đầu tiên bình thường hóa thành`4991234567`, trong khi cái thứ hai trở thành`4981234567`. Phần thuê bao giống nhau nhưng mã khác nhau nên thuật toán in đúng`NO`.
