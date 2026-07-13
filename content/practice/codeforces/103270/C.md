---
title: "CF 103270C - Chó của Abhilash"
description: "Chúng ta được cung cấp một đường dẫn được tạo thành từ các vị trí rời rạc được sắp xếp theo một đường thẳng và mỗi vị trí có một giá trị độ cao mà chúng ta có thể tự do lựa chọn."
date: "2026-07-03T14:39:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103270
codeforces_index: "C"
codeforces_contest_name: "UTPC Contest 09-03-21 Div. 1 (Advanced)"
rating: 0
weight: 103270
solve_time_s: 45
verified: true
draft: false
---

[CF 103270C - Con chó của Abhilash](https://codeforces.com/problemset/problem/103270/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đường dẫn được tạo thành từ các vị trí rời rạc được sắp xếp theo một đường thẳng và mỗi vị trí có một giá trị độ cao mà chúng ta có thể tự do lựa chọn. Đường dẫn phải bắt đầu ở độ cao bằng 0 và kết thúc ở độ cao bằng 0 và giữa hai vị trí liền kề bất kỳ, độ cao chỉ có thể thay đổi tối đa một đơn vị lên hoặc xuống. Trong số tất cả các cấu hình chiều cao hợp lệ như vậy, nhiệm vụ là xác định chiều cao tối đa có thể xuất hiện ở bất kỳ đâu dọc theo đường dẫn. 

Một cách khác để xem điều này là chúng tôi đang xây dựng một “mặt cắt địa hình” có chiều dài L. Chúng tôi muốn đỉnh cao nhất có thể, nhưng chúng tôi bị hạn chế bởi hai điều kiện: địa hình phải chạm mặt đất ở cả hai đầu và nó không thể có độ dốc lớn hơn một đơn vị trên mỗi bậc thang. Câu hỏi đặt ra là một ngọn núi có thể được định hình cao bao nhiêu dưới những ràng buộc này. 

Ràng buộc L lên tới 100000 có nghĩa là chúng ta cần một giải pháp tuyến tính hoặc logarit kém nhất trong L. Bất kỳ công trình hoặc mô phỏng nào thử tất cả các cấu hình độ cao có thể đều không thể thực hiện được vì số lượng chuỗi hợp lệ tăng theo cấp số nhân. Ngay cả cách tiếp cận bậc hai đối với các phân đoạn cũng sẽ quá chậm. 

Trường hợp cạnh khóa xuất hiện khi độ dài nhỏ. Đối với L bằng 1 hoặc 2, địa hình buộc phải duy trì ở độ cao 0 trong suốt hoặc ngay lập tức trở về 0, do đó không thể đạt được đỉnh. Đối với các giá trị lớn hơn một chút như L bằng 3 hoặc 4, chỉ có thể hình thành một đỉnh hình tam giác nhỏ và rất dễ cho rằng có thể có các đỉnh lớn hơn nếu bỏ qua ràng buộc biên là cả hai đầu phải bằng 0. 

Ví dụ: khi L bằng 4, cấu hình hợp lệ là 0, 0, 1, 0 và chiều cao tối đa là 1. Một trực giác ngây thơ có thể gợi ý chiều cao bằng 2 là có thể bằng cách tăng quá nhanh, nhưng ràng buộc chênh lệch liền kề sẽ chặn điều đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử xây dựng tất cả các chuỗi chiều cao hợp lệ có độ dài L bắt đầu và kết thúc bằng 0, xác minh ràng buộc kề và tính chiều cao tối đa trong mỗi chuỗi. Về nguyên tắc, điều này đúng vì nó liệt kê tất cả các địa hình khả thi, nhưng số lượng trình tự tăng lên giống như một bước đi ngẫu nhiên bị hạn chế. Ngay cả đối với L vừa phải, số lượng các khả năng có thể trở nên vô cùng lớn, khiến điều này hoàn toàn không thể thực hiện được nếu chỉ có đầu vào rất nhỏ. 

Cấu trúc của bài toán là một bước đi ràng buộc cổ điển trên các số nguyên. Mỗi bước thay đổi chiều cao tối đa một bước và chúng ta bắt đầu và kết thúc ở mức 0. Đỉnh cao nhất có thể tương ứng với việc đẩy lối đi lên trên càng nhiều càng tốt trước khi nó phải trở về số 0. Yếu tố hạn chế là khi chúng ta tăng chiều cao lên một bước cho mỗi bước, chúng ta cũng cần đủ số bước còn lại để giảm về 0 với cùng giới hạn độ dốc. 

Điều này dẫn đến một cách giải thích hình học đơn giản. Nếu muốn đạt đến độ cao H, chúng ta cần ít nhất H bước để leo từ 0, vì chúng ta có thể tăng tối đa một bước cho mỗi lần di chuyển. Sau khi đạt H, chúng ta cần thêm H bước nữa để quay về 0. Vì vậy, một đỉnh có độ cao H cần ít nhất 2H bước cho chuyển động lên xuống, cộng thêm một vị trí cho chính đỉnh đó nếu chúng ta nghĩ về các vị trí. Điều này kết nối trực tiếp chiều cao tối đa có thể với chiều dài L có sẵn. 

Do đó, bài toán quy về việc tìm số nguyên H lớn nhất sao cho một ngọn núi đối xứng có chiều cao H nằm gọn trong các vị trí L. Cấu trúc đó là một hình tam giác rời rạc. Nếu chúng ta đặt đỉnh ở giữa thì tổng chiều dài cần thiết là 2H + 1. Do đó, chúng ta muốn H tối đa sao cho 2H + 1 ≤ L. 

Từ bất đẳng thức này, câu trả lời trở thành sàn((L - 1) / 2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (liệt kê tất cả các cấu hình chiều cao hợp lệ) | Hàm mũ | O(L) | Quá chậm | 
| Tối ưu (rút ra giới hạn khả thi cao nhất) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số nguyên L biểu thị số vị trí trên đường dẫn. Mục đích là để xác định độ cao của đỉnh hợp lệ cao nhất có thể chịu các ràng buộc về chuyển động và ranh giới. 
2. Quan sát rằng bất kỳ đỉnh cao H nào cũng phải được hình thành bằng cách tăng đơn điệu từ 0 đến H và sau đó giảm đơn điệu trở về 0. Mỗi bước tăng hoặc giảm sẽ thay đổi chiều cao chính xác bằng 1 trong trường hợp tốt nhất, vì không được phép nhảy lớn hơn. 
3. Tính số vị trí tối thiểu cần thiết để tạo thành một đỉnh như vậy. Đi từ 0 lên H cần H bước và đi từ H trở xuống 0 cần thêm H bước nữa, điều này đã tính đến chuyển tiếp 2H. Vì vị trí đỉnh được tính một lần nên tổng số vị trí cần có là 2H + 1. 
4. Đảm bảo tính khả thi bằng cách yêu cầu 2H + 1 ≤ L. Sự bất bình đẳng này thể hiện thực tế là toàn bộ ngọn núi phải vừa với chiều dài đoạn có sẵn. 
5. Giải bất đẳng thức cho H, thu được H ≤ (L - 1)/2. Số nguyên H lớn nhất thỏa mãn ràng buộc này là đáp án cuối cùng. 

### Tại sao nó hoạt động 

Bất kỳ chuỗi chiều cao hợp lệ nào đạt đến giá trị tối đa H đều phải tăng ít nhất H bước và giảm H bước vì chiều cao thay đổi tối đa một bước cho mỗi lần di chuyển. Điều này tạo ra một giới hạn dưới nghiêm ngặt về độ dài cần thiết để hỗ trợ đỉnh cao H. Cấu trúc đối xứng 0, 1, 2, ..., H, ..., 2, 1, 0 đạt được giới hạn này một cách chính xác, do đó không tồn tại cấu trúc hiệu quả nào hơn. Vì cả đối số cần thiết và cấu trúc phù hợp đều tồn tại nên giới hạn chặt chẽ và công thức là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

L = int(input().strip())

print((L - 1) // 2)
```Toàn bộ giải pháp xoay quanh việc chuyển giới hạn về chiều cao thành yêu cầu về độ dài tối thiểu đối với một ngọn núi rời rạc. Mã này trực tiếp áp dụng công thức dẫn xuất mà không cần mô phỏng, vì bất kỳ cách xây dựng lặp lại nào cũng sẽ là chi phí không cần thiết đối với một bài toán dẫn đến một bất đẳng thức duy nhất. 

Sự tinh tế duy nhất là phép chia số nguyên. Sử dụng (L - 1) // 2 làm sàn chính xác giá trị thực của (L - 1) / 2, phù hợp với yêu cầu H phải là số nguyên thỏa mãn ràng buộc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

L = 4 

Giá trị được tính toán là (4 - 1) // 2 = 3 // 2 = 1. 

| Bước | L | Tính H | 
| --- | --- | --- | 
| Bắt đầu | 4 | - | 
| Áp dụng công thức | 4 | 1 | 

Chiều cao thu được tương ứng với cấu trúc hợp lệ như 0, 0, 1, 0. Điều này cho thấy rằng ngay cả khi có chỗ để tăng, việc quay trở lại 0 sẽ tiêu tốn không gian cần thiết, hạn chế chiều cao tối đa. 

### Ví dụ 2 

đầu vào: 

L = 5 

Giá trị được tính toán là (5 - 1) // 2 = 4 // 2 = 2. 

| Bước | L | Tính H | 
| --- | --- | --- | 
| Bắt đầu | 5 | - | 
| Áp dụng công thức | 5 | 2 | 

Địa hình hợp lệ là 0, 1, 2, 1, 0. Địa hình này sử dụng chính xác mọi vị trí có sẵn và xác nhận rằng giới hạn là chặt chẽ. 

Ví dụ này chứng minh rằng độ dài lẻ cho phép đỉnh ở giữa hoàn hảo, trong khi độ dài chẵn buộc cấu trúc hơi dẹt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ thực hiện một phép tính số học duy nhất | 
| Không gian | O(1) | Không cần cấu trúc phụ trợ | 

Các ràng buộc cho phép lên tới 100000, nhưng giải pháp không lặp lại đầu vào theo bất kỳ cách có ý nghĩa nào ngoài việc đọc một số nguyên. Điều này làm cho nó nhanh chóng và trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline
    L = int(input().strip())
    return str((L - 1) // 2)

# provided samples
assert run("4\n") == "1"
assert run("5\n") == "2"

# custom cases
assert run("1\n") == "0", "minimum length"
assert run("2\n") == "0", "too short for any peak"
assert run("3\n") == "1", "smallest non-trivial peak"
assert run("10\n") == "4", "larger symmetric structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | trường hợp ranh giới tối thiểu | 
| 2 | 0 | không còn chỗ cho đỉnh cao | 
| 3 | 1 | núi hợp lệ nhỏ nhất | 
| 10 | 4 | độ chính xác trên chiều dài chẵn lớn hơn | 

## Vỏ cạnh 

Đối với L bằng 1, chuỗi chỉ chứa một vị trí duy nhất phải là cả vị trí bắt đầu và kết thúc, buộc chiều cao bằng 0 ở mọi nơi. Thuật toán trả về (1 - 1) // 2 = 0, khớp với thực tế là không thể đi lên được. 

Đối với L bằng 2, không có chỗ để tăng và giảm dưới giới hạn ±1 trong khi bắt đầu và kết thúc ở số 0. Công thức cho (2 - 1) // 2 = 0, ngăn chặn chính xác mọi đỉnh sai. 

Đối với L bằng 3, cấu trúc hợp lệ duy nhất là một đỉnh đơn có chiều cao 1, cụ thể là 0, 1, 0. Phép tính mang lại (3 - 1) // 2 = 1, khớp chính xác với cấu trúc tối ưu. 

Đối với L lớn hơn, thuật toán ngầm xây dựng hình dạng núi đối xứng tối đa mặc dù nó chưa bao giờ được xây dựng một cách rõ ràng. Tính đúng đắn xuất phát từ thực tế là bất kỳ nỗ lực nào vượt quá độ cao này đều sẽ phải vi phạm ràng buộc bước hoặc ràng buộc biên.
