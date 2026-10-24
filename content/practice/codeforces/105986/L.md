---
title: "CF 105986L - \u7b49\u4ef7\u4ea4\u6362"
description: "Chúng ta được cung cấp một hệ thống vật phẩm, trong đó mỗi vật phẩm cuối cùng có thể trở thành “được cung cấp năng lượng” nếu nó được chọn trực tiếp ngay từ đầu hoặc có thể được sản xuất thông qua công thức nấu ăn."
date: "2026-06-22T16:35:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105986
codeforces_index: "L"
codeforces_contest_name: "2025 Wuhan University of Technology Programming Contest"
rating: 0
weight: 105986
solve_time_s: 62
verified: true
draft: false
---

[CF 105986L - \u7b49\u4ef7\u4ea4\u6362](https://codeforces.com/problemset/problem/105986/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống vật phẩm, trong đó mỗi vật phẩm cuối cùng có thể trở thành “được cung cấp năng lượng” nếu nó được chọn trực tiếp ngay từ đầu hoặc có thể được sản xuất thông qua công thức nấu ăn. Mỗi công thức lấy một số mục đầu vào và tạo ra một mục đầu ra, với quy tắc là đầu ra chỉ được cấp điện khi tất cả các đầu vào của nó đã được cấp điện. 

Ban đầu, không có vật nào được cấp năng lượng. Chúng tôi được phép cấp năng lượng thủ công cho một số hạng mục. Sau khi hoàn thành việc đó, chúng tôi liên tục áp dụng tất cả các công thức có đầu vào được cung cấp năng lượng đầy đủ và các đầu ra đó cũng được cung cấp năng lượng. Quá trình tiếp tục cho đến khi không thể kích hoạt thêm mục nào nữa. 

Điều khó khăn là chúng ta không thực sự biết cấu trúc của công thức nấu ăn. Chúng ta chỉ biết rằng có n vật phẩm và m công thức nấu ăn, và chúng ta phải giả định cách sắp xếp các công thức nấu ăn tốt nhất có thể để giảm thiểu số lượng vật phẩm mà chúng ta cung cấp năng lượng ban đầu. Mục tiêu là tính toán con số tối thiểu có thể này. 

Ràng buộc n lên tới 2×10^5 và m lên đến n ngụ ý rằng chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ phép tính bậc hai nào trong n sẽ ngay lập tức thất bại vì thậm chí 10^10 phép tính cũng vượt xa giới hạn thời gian. 

Trường hợp cạnh tinh tế xuất hiện khi m bằng 0. Không có công thức nấu ăn thì không thể sản xuất được gì nên mọi vật dụng đều phải được cung cấp năng lượng bằng tay, cho ra câu trả lời n. Một trường hợp đặc biệt khác là khi các công thức nấu ăn đủ phong phú để có khả năng kết nối mọi thứ thành một chuỗi dẫn xuất duy nhất, trong trường hợp đó, câu trả lời sẽ giảm xuống còn 1. 

Khó khăn chính là hiểu được các công thức nấu ăn có thể cung cấp bao nhiêu “sức mạnh hợp nhất” theo thiết kế tối ưu. 

## Phương pháp tiếp cận 

Chúng tôi bắt đầu từ quan điểm bi quan nhất: nếu không có công thức nấu ăn, mọi món đồ đều bị cô lập. Cách duy nhất để tiếp thêm năng lượng cho mọi thứ là chọn thủ công tất cả n mục, vì vậy câu trả lời là n. 

Bây giờ hãy xem xét công thức thực sự có tác dụng gì trong trường hợp tốt nhất có thể. Nếu chúng tôi thiết kế các công thức nấu ăn một cách tự do, chúng tôi luôn mong muốn chúng càng đơn giản càng tốt về các điều kiện tiên quyết. Một công thức có nhiều đầu vào thực sự khó đáp ứng hơn vì nó yêu cầu tất cả chúng đều phải hoạt động. Ngược lại, một công thức có một đầu vào duy nhất hoạt động giống như một cạnh được định hướng từ mục này sang mục khác: khi mục đầu tiên được kích hoạt thì mục thứ hai sẽ hoạt động ngay lập tức. 

Quan sát này là sự đơn giản hóa quan trọng. Trong một cấu trúc tối ưu, mọi công thức đều có thể được coi như một cạnh định hướng từ mục này sang mục khác. Vấn đề giảm xuống còn việc chọn m cạnh có hướng trên n nút để giảm thiểu số lượng nút bắt đầu cần thiết để tất cả các nút đều có thể truy cập được trong khả năng tiếp cận dọc theo các cạnh có hướng. 

Bây giờ cấu trúc trở nên quen thuộc. Chúng tôi đang cố gắng giảm thiểu số lượng nguồn cần thiết để bao phủ tất cả các nút trong cấu trúc giống như khu rừng được định hướng. Mỗi cạnh được định hướng có thể giảm tối đa một số lượng thành phần khởi đầu độc lập, bởi vì nó có thể kết nối một nút độc lập trước đó với một chuỗi có thể truy cập hiện có. 

Vì vậy, nếu chúng ta bắt đầu từ n nút riêng biệt, mọi công thức có thể giảm số lượng điểm bắt đầu cần thiết xuống một điểm trong trường hợp tốt nhất. Sau khi sử dụng m mức giảm như vậy, số điểm bắt đầu cần thiết sẽ trở thành n - m. Nếu m đủ lớn để xuống dưới 1, chúng ta vẫn cần ít nhất một nút khởi đầu, vì ban đầu một số nút phải được cấp điện. 

Điều này dẫn trực tiếp đến công thức max(1, n - m). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng xây dựng Brute Force | O(2^n) | O(n) | Quá chậm | 
| Lý luận giảm cạnh tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc n và m từ đầu vào. Chúng mô tả số lượng mặt hàng tồn tại và số lượng công thức nấu ăn mà chúng tôi được phép thiết kế một cách tối ưu. 
2. Nếu không có công thức nào, trả về n. Không có khả năng biến đổi, mỗi vật phẩm ban đầu phải được cung cấp năng lượng. 
3. Mặt khác, hãy diễn giải từng công thức ở dạng hiệu quả nhất dưới dạng sự phụ thuộc trực tiếp duy nhất giữa hai mục. Đây là cấu hình giúp tối đa hóa hiệu quả truyền bá. 
4. Bắt đầu với ý tưởng rằng ban đầu mỗi mục hình thành yêu cầu độc lập của riêng nó, do đó có n điểm bắt đầu bắt buộc. 
5. Mỗi công thức có thể hợp nhất chính xác một mục độc lập trước đó thành một cấu trúc có thể truy cập được, giảm số lượng điểm bắt đầu cần thiết xuống một. Sau khi sử dụng tối ưu tất cả m công thức, số đếm sẽ trở thành n - m. 
6. Nếu giá trị này giảm xuống dưới 1, hãy kẹp nó ở mức 1 vì cần ít nhất một vật phẩm được cấp điện ban đầu để kích hoạt bất kỳ sự lan truyền nào. 

### Tại sao nó hoạt động 

Quá trình này có thể được xem là giảm dần số lượng “nguồn kích hoạt” độc lập cần thiết để bao phủ biểu đồ. Mọi công thức, khi được thiết kế tối ưu dưới dạng một quy tắc tiên quyết duy nhất, có thể đính kèm một thành phần trước đây không thể truy cập vào cấu trúc có thể truy cập hiện có. Vì các công thức nấu ăn không thể tạo ra sự kích hoạt từ con số không, nên chúng chỉ có thể truyền năng lượng hiện có chứ không loại bỏ hoàn toàn nhu cầu về điểm xuất phát. Điều này có nghĩa là tổng số thành phần khởi đầu độc lập giảm tối đa một thành phần cho mỗi công thức và giới hạn này có thể đạt được bằng cách xây dựng cấu trúc phụ thuộc giống như chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    ans = n - m
    if ans < 1:
        ans = 1
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên thực tế là chúng ta không bao giờ cần xây dựng biểu đồ công thức một cách rõ ràng. Câu trả lời chỉ phụ thuộc vào số lần giảm độc lập mà m có thể đạt được so với n mục riêng biệt ban đầu. Phép trừ ghi lại số lượng mục có thể được hấp thụ vào chuỗi phụ thuộc và kẹp cuối cùng đảm bảo chúng tôi không loại bỏ hoàn toàn nhu cầu về hạt giống ban đầu. 

Một lỗi phổ biến là cố gắng mô phỏng các công thức nấu ăn hoặc xây dựng một biểu đồ thực tế, điều này là không cần thiết và sẽ gây ra chi phí chung mà không thay đổi cấu trúc tổ hợp cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
```Chúng tôi bắt đầu với 3 mục riêng biệt. 

| Bước | Thành phần độc lập | Công thức nấu ăn còn lại được sử dụng | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 3 | 2 | 3 | 
| Sau 1 công thức | 2 | 1 | 2 | 
| Sau 2 công thức | 1 | 0 | 1 | 

Câu trả lời cuối cùng là 1. 

Điều này cho thấy hai phần phụ thuộc tối ưu là đủ để thu gọn ba mục riêng biệt thành một chuỗi kích hoạt duy nhất. 

### Ví dụ 2 

đầu vào:```
5 1
```| Bước | Thành phần độc lập | Công thức nấu ăn còn lại được sử dụng | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Ban đầu | 5 | 1 | 5 | 
| Sau 1 công thức | 4 | 0 | 4 | 

Câu trả lời cuối cùng là 4. 

Điều này chứng tỏ rằng một công thức duy nhất chỉ có thể làm giảm nhu cầu về một mặt hàng có năng lượng ban đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ số học trên n và m | 
| Không gian | O(1) | Không cần cấu trúc phụ trợ | 

Các ràng buộc cho phép tính toán trực tiếp này một cách dễ dàng, vì ngay cả kích thước đầu vào tối đa cũng chỉ yêu cầu xử lý thời gian không đổi cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    ans = n - m
    if ans < 1:
        ans = 1
    return str(ans)

# provided sample
assert run("3 2\n") == "1"

# minimum case
assert run("1 0\n") == "1"

# no recipes, many items
assert run("5 0\n") == "5"

# enough recipes to fully connect
assert run("6 10\n") == "1"

# exact boundary
assert run("5 4\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 | 1 | ranh giới kích thước tối thiểu | 
| 5 0 | 5 | không có trường hợp công thức | 
| 6 10 | 1 | hành vi kẹp khi m ≥ n | 
| 5 4 | 1 | trường hợp bão hòa chính xác | 

## Vỏ cạnh 

Khi n bằng 1 thì chỉ có một vật phẩm và nó phải được cấp điện bất kể m. Công thức cho giá trị tối đa (1, 1 - m), luôn đánh giá là 1, phù hợp với yêu cầu. 

Khi m bằng 0 thì không thể lan truyền được và mọi mục ban đầu phải được chọn. Công thức cho max(1, n), trả về chính xác n cho mọi n ≥ 1. 

Khi m rất lớn so với n, phép trừ n - m trở nên âm, nhưng kẹp đảm bảo câu trả lời không bao giờ giảm xuống dưới một, phản ánh thực tế rằng luôn cần ít nhất một mục bắt đầu để bắt đầu bất kỳ chuỗi dẫn xuất nào.
