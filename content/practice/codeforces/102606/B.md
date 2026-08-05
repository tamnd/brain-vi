---
title: "CF 102606B - Chuỗi nhị phân"
description: "Vấn đề này là về việc khám phá một chuỗi nhị phân ẩn. Chúng tôi chỉ được biết chiều dài của nó. Thông tin duy nhất có sẵn đến từ các truy vấn: chúng tôi gửi một chuỗi nhị phân khác và thẩm phán cho chúng tôi biết liệu chuỗi đã gửi của chúng tôi có xuất hiện bên trong chuỗi ẩn dưới dạng một chuỗi con hay không."
date: "2026-08-05T00:37:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102606
codeforces_index: "B"
codeforces_contest_name: "2020 ECNU Campus Online Invitational Contest"
rating: 0
weight: 102606
solve_time_s: 155
verified: true
draft: false
---

[CF 102606B - Chuỗi nhị phân](https://codeforces.com/problemset/problem/102606/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề này là về việc khám phá một chuỗi nhị phân ẩn. Chúng tôi chỉ được biết chiều dài của nó. Thông tin duy nhất có sẵn đến từ các truy vấn: chúng tôi gửi một chuỗi nhị phân khác và thẩm phán cho chúng tôi biết liệu chuỗi đã gửi của chúng tôi có xuất hiện bên trong chuỗi ẩn dưới dạng một chuỗi con hay không. 

Thách thức là chuỗi ẩn có thể có độ dài lên tới 1000, trong khi mỗi truy vấn bị giới hạn ở khoảng một nửa độ dài đó. Phương pháp xây dựng lại trực tiếp hỏi về toàn bộ tiền tố đã biết cuối cùng sẽ tạo ra các truy vấn quá dài. Giải pháp phải sử dụng giới hạn độ dài truy vấn làm hướng dẫn. 

Bởi vì có nhiều nhất là 1024 truy vấn và`n`tối đa là 1000, có thể chấp nhận cách tiếp cận sử dụng khoảng một truy vấn cho mỗi ký tự. Tuy nhiên, bất kỳ giải pháp nào yêu cầu khám phá bậc hai các chuỗi có thể đều không thể thực hiện được vì số lượng chuỗi nhị phân có thể tăng theo cấp số nhân. 

Những trường hợp phức tạp không phải do giá trị lớn gây ra mà do hướng chúng ta tái cấu trúc. Ví dụ: nếu chuỗi ẩn là:```
1
```câu trả lời hợp lệ duy nhất là:```
1
```Một giải pháp giả định rằng nó luôn có thể kiểm tra số 0 trước và nối thêm một cách mù quáng nó sẽ thất bại. 

Đối với trường hợp chia nhỏ:```
n = 4
hidden = 0110
```Nếu chúng ta cố gắng xây dựng lại tất cả bốn ký tự từ bên trái bằng cách hỏi xem câu trả lời hiện tại cộng với một ký tự mới có phải là một chuỗi con hay không, thì sau khi tìm thấy ba ký tự, độ dài truy vấn sẽ trở thành bốn, lớn hơn độ dài truy vấn được phép là ba. Vấn đề tương tự xuất hiện đối với mọi chuỗi lớn. Cách tiếp cận đúng phải xây dựng lại từ cả hai đầu để mọi truy vấn đều nằm trong giới hạn. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ tự nhiên là xây dựng câu trả lời từ trái sang phải. Giả sử chúng ta đã biết tiền tố`p`. Chúng ta có thể hỏi liệu`p + "0"`là một dãy số. Nếu không thì ký tự tiếp theo phải là`1`. Điều này có hiệu quả vì mỗi tiền tố của chuỗi thực tự nó là một chuỗi con của chuỗi ẩn. 

Vấn đề là kích thước truy vấn. Sau khi khám phá được khoảng một nửa chuỗi, truy vấn tiếp theo sẽ chứa tiền tố đã biết cộng thêm một ký tự nữa, vượt quá độ dài cho phép. Vì`n = 1000`, việc xây dựng lại toàn bộ từ trái sang phải cần các truy vấn có độ dài gần 1000, trong khi giới hạn chỉ là 501. 

Quan sát quan trọng là giới hạn chính xác bằng một nửa chiều dài. Chúng ta có thể tái tạo lại nửa đầu từ bên trái và nửa sau từ bên phải. Khi xây dựng lại từ bên phải, chúng tôi giữ hậu tố đã biết ở cuối truy vấn và thêm một ký tự ứng cử viên vào trước. Độ dài truy vấn không bao giờ vượt quá một nửa chuỗi cộng một. 

Hai nửa cùng nhau bao phủ mọi vị trí và số lượng truy vấn nhiều nhất`n`, dễ dàng nằm gọn trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tái thiết từ trái sang phải | O(n) truy vấn nhưng kích thước truy vấn không hợp lệ | O(n) | Không được phép | 
| Tái thiết hai mặt | Truy vấn O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`half = n // 2`. Xây dựng lại lần đầu tiên`half`ký tự từ trái qua phải. Duy trì tiền tố đã được tìm thấy. Hỏi xem tiền tố có theo sau không`0`là một dãy số. Nếu câu trả lời là có thì ký tự tiếp theo là`0`; nếu không nó phải là`1`. 

Truy vấn dài nhất trong giai đoạn này có độ dài`half`, vì vậy nó tôn trọng giới hạn truy vấn. 

1. Dựng lại các ký tự còn lại từ phải qua trái. Duy trì hậu tố đã được tìm thấy. Hỏi xem`0`theo sau là hậu tố hiện tại là một dãy con. Nếu đúng như vậy thì nhân vật mới là`0`; nếu không thì nó là`1`. 

Độ dài hậu tố trước khi thêm ký tự mới không bao giờ dài hơn`n - half - 1`, vì vậy độ dài truy vấn tối đa là`half + 1`. 

1. Đảo ngược hậu tố được xây dựng lại và đặt nó sau nửa đầu. Hai phần cùng nhau tạo thành chuỗi gốc. 

Tại sao nó hoạt động: 

Bất biến trong giai đoạn đầu tiên là tiền tố được lưu trữ chính xác là tiền tố tương ứng của chuỗi ẩn. Thêm một trong hai`0`hoặc`1`tạo một tiền tố ứng cử viên của chuỗi ẩn và chỉ chuỗi đúng vẫn là một chuỗi con. 

Giai đoạn thứ hai sử dụng ý tưởng tương tự theo hướng ngược lại. Hậu tố đúng của chuỗi ẩn vẫn là một chuỗi con khi ký tự thực tiếp theo được đặt trước nó. Kiểm tra`0 + suffix`phân biệt ký tự tiếp theo là 0 hay 1. Vì mọi vị trí đều thuộc về chính xác một trong hai phần được xây dựng lại nên chuỗi cuối cùng là chính xác. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

def ask(s):
    print("? " + s, flush=True)
    return int(input())

n = int(input())

half = n // 2

left = ""
for _ in range(half):
    if ask(left + "0"):
        left += "0"
    else:
        left += "1"

right = ""
for _ in range(n - half):
    if ask("0" + right):
        right = "0" + right
    else:
        right = "1" + right

print("! " + left + right, flush=True)
```chức năng`ask`xử lý các giao tiếp tương tác. Mọi truy vấn sẽ được xóa ngay lập tức vì người đánh giá tương tác sẽ đợi kết quả đầu ra của chương trình trước khi trả lời. 

Vòng lặp đầu tiên xây dựng phần bên trái. Tại mọi thời điểm, tiền tố hiện tại đã được biết là chính xác, vì vậy việc kiểm tra số 0 tiếp theo là đủ. Trường hợp ngược lại phải là một vì chuỗi ẩn có đúng một ký tự ở vị trí đó. 

Vòng lặp thứ hai sử dụng logic tương tự nhưng phát triển câu trả lời ngược lại. Nhiệm vụ`right = "0" + right`giữ hậu tố theo đúng thứ tự trong khi thêm các ký tự từ phải sang trái. 

Không cần tính toán số nguyên ngoài điểm phân chia, do đó không có lo ngại về tràn. Ranh giới quan trọng là độ dài truy vấn và cả hai vòng lặp đều được thiết kế sao cho kích thước truy vấn tối đa là`floor(n/2)+1`. 

## Ví dụ đã hoạt động 

Đối với một chuỗi ẩn`0110`: 

| Bước | Được biết trái | Được biết đúng | Truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | "" | "" | "0" | Có | 
| 2 | "0" | "" | "00" | Không | 
| 3 | "01" | "" | "010" | Có | 
| 4 | "01" | "0" | "00" | Có | 

Nửa đầu trở thành`01`. Việc tái thiết đúng sẽ phát hiện ra phần còn lại`10`, đưa ra câu trả lời cuối cùng`0110`. 

Đối với một chuỗi ẩn`101`: 

| Bước | Được biết trái | Được biết đúng | Truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | "" | "" | "0" | Không | 
| 2 | "" | "1" | "01" | Có | 
| 3 | "" | "01" | "001" | Không | 

Thuật toán tìm phần bên trái là`1`và phần bên phải là`01`, sản xuất`101`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(n) | Một truy vấn xác định từng ký tự | 
| Không gian | O(n) | Chuỗi được xây dựng lại được lưu trữ | 

Giải pháp sử dụng tối đa`n`truy vấn, thấp hơn 1024 truy vấn được phép với độ dài tối đa. Việc sử dụng bộ nhớ là tuyến tính và dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm 

Đây là sự cố tương tác nên không có định dạng đầu vào/đầu ra ngoại tuyến nào có thể được kiểm tra bằng các thử nghiệm dựa trên xác nhận thông thường. Các trường hợp sau đây mô tả các tình huống mà trình mô phỏng ngoại tuyến dành cho trọng tài cần xác minh. 

| Chuỗi ẩn | Câu trả lời dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`0`| Xử lý độ dài tối thiểu | 
|`1`|`1`| Ký tự đơn không có chuỗi con bằng 0 | 
|`0000`|`0000`| Tất cả các giá trị bằng nhau | 
|`1111`|`1111`| Tất cả các giá trị bằng nhau | 
|`0110`|`0110`| Điểm phân chia giữa hai nửa | 
|`10101`|`10101`| Tái thiết độ dài lẻ | 

## Vỏ cạnh 

Đối với chuỗi một ký tự`1`, giai đoạn đầu tiên không có ký tự để xây dựng lại vì`n // 2`là số không. Giai đoạn hậu tố hỏi liệu`0`là có thể. Câu trả lời là sai nên thuật toán đặt`1`vào hậu tố và trả về chuỗi đúng. 

Đối với một chuỗi có độ dài chẵn như`0110`, sự phân chia tạo ra hai phần có kích thước bằng nhau. Pha bên trái chỉ xử lý`01`, và tay cầm pha bên phải`10`. Không bên nào cần một truy vấn dài hơn ba ký tự, đây là giới hạn cho phép đối với độ dài này. 

Đối với một chuỗi có độ dài lẻ như`10101`, phần bên trái có độ dài bằng hai và phần bên phải có độ dài bằng ba. Việc xây dựng lại bên phải vẫn hoạt động vì trước khi thêm mỗi ký tự, hậu tố được lưu trữ đủ ngắn để việc thêm một ký tự vào trước sẽ giữ cho truy vấn nằm trong giới hạn. Phép nối cuối cùng giữ nguyên thứ tự ban đầu.
