---
title: "CF 939B - Trang trại Hamster"
description: "Dima có một số lượng chuột đồng cố định sẵn sàng để vận chuyển và một bộ sưu tập các loại hộp, mỗi loại có sức chứa khác nhau. Mỗi hộp được sử dụng phải được lấp đầy hoàn toàn và tất cả các hộp được chọn phải thuộc một loại duy nhất vì mua nhiều loại sẽ được giảm giá."
date: "2026-06-17T02:35:46+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 939
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 464 (Div. 2)"
rating: 1000
weight: 939
solve_time_s: 66
verified: true
draft: false
---

[CF 939B - Trang trại Hamster](https://codeforces.com/problemset/problem/939/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Dima có một số lượng chuột đồng cố định sẵn sàng để vận chuyển và một bộ sưu tập các loại hộp, mỗi loại có sức chứa khác nhau. Mỗi hộp được sử dụng phải được lấp đầy hoàn toàn và tất cả các hộp được chọn phải thuộc một loại duy nhất vì mua nhiều loại sẽ được giảm giá. 

Vì vậy, quyết định là: chọn một giá trị dung lượng từ danh sách, giả sử nó giữ`a`chuột đồng vào mỗi hộp, sau đó đóng gói càng nhiều hộp đầy đủ có kích thước đó càng tốt bằng cách sử dụng`N`chuột đồng. Bất kỳ chuột hamster nào còn sót lại không vừa với hộp đầy đủ sẽ bị bỏ lại và không đóng góp vào kết quả. 

Đối với công suất đã chọn`a`, số hộp dùng được là`N // a`, và số lượng chuột đồng được vận chuyển là`a * (N // a)`. Mục tiêu là chọn chỉ số tối đa hóa tổng số vận chuyển này. 

Những hạn chế là vô cùng lớn:`N`có thể lên tới 10^18 và mỗi dung lượng cũng có thể lên tới 10^18, trong khi số lượng loại có thể lên tới 100.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng việc đóng gói hoặc lặp lại từng con chuột đồng hoặc hộp riêng lẻ. Ngay cả suy luận bậc hai về năng lực cũng không thể thực hiện được. Hướng khả thi duy nhất là đánh giá từng loại một cách độc lập trong thời gian không đổi. 

Một vài trường hợp đáng chú ý. Khi`N = 0`, không có chuột đồng nào tồn tại nên mọi khả năng đều không tạo ra chuột đồng được vận chuyển và bất kỳ loại nào đều hợp lệ, nhưng số lượng hộp cũng phải bằng 0. Khi dung lượng hộp lớn hơn`N`, loại đó cũng không góp phần vận chuyển chuột đồng vì không thể hình thành hộp đầy đủ. Những trường hợp này có thể âm thầm phá vỡ logic tham lam không chính xác giả định rằng luôn tồn tại ít nhất một hộp đầy đủ. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng suy luận về số lượng chuột đồng còn lại sau khi chọn từng loại và có thể tạo được bao nhiêu hộp đầy đủ. Với mỗi công suất`a_i`, chúng tôi tính xem có bao nhiêu hộp hoàn chỉnh vừa vặn và nhân ngược lại để có được tổng số hàng đã vận chuyển. Điều này đã gần đạt mức tối ưu vì mỗi loại có thể được đánh giá độc lập mà không cần tương tác. 

Một sai lầm ngây thơ là cố liệt kê số hộp cần lấy cho mỗi loại từ`1`lên đến`N / a_i`, tính toán lựa chọn tốt nhất cho mỗi loại. Điều đó sẽ dẫn đến tổng độ phức tạp tỷ lệ thuận với tổng số hộp có thể có, trong trường hợp xấu nhất đạt tới 10^18 và hoàn toàn không khả thi. 

Nhận xét quan trọng là cấu trúc của bài toán loại bỏ mọi tương tác tổ hợp. Sau khi đã cố định dung lượng, chiến lược tốt nhất sẽ được áp dụng: lấy càng nhiều hộp đầy càng tốt. Không có lợi ích gì khi lấy ít hộp hơn vì những chú chuột hamster không sử dụng sẽ không thể cải thiện được điều gì sau này. Điều này làm giảm vấn đề đánh giá một công thức cho mỗi loại và chọn mức tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực về số lượng hộp trên mỗi loại | O(NK) | O(1) | Quá chậm | 
| Đánh giá từng loại một lần | O(K) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng loại hộp một cách độc lập và tính toán kết quả tốt nhất có thể đạt được nếu loại đó được chọn. 

1. Đọc`N`và danh sách năng lực. Những thông tin này xác định tổng số chuột đồng có sẵn và tất cả các kích cỡ hộp có thể có. 
2. Khởi tạo các biến để lưu trữ câu trả lời tốt nhất được tìm thấy cho đến nay, bao gồm chỉ mục tốt nhất và giá trị được vận chuyển tốt nhất. 
3. Đối với từng loại`i`, hãy tính xem có thể tạo được bao nhiêu hộp đầy đủ, đó là`N // a[i]`. Điều này trực tiếp thực thi yêu cầu các hộp phải được lấp đầy hoàn toàn. 
4. Tính toán số chuột hamster được vận chuyển là`a[i] * (N // a[i])`. Điều này đo lường tổng sản lượng có thể sử dụng cho lựa chọn đó. 
5. Nếu giá trị này lớn hơn giá trị tốt nhất được thấy cho đến nay, hãy cập nhật chỉ mục tốt nhất và lưu trữ cả chỉ mục lẫn số lượng hộp. 
6. Sau khi quét tất cả các loại, xuất ra chỉ mục tốt nhất được lưu trữ và số lượng hộp tương ứng. 

Ý tưởng trung tâm là mỗi loại ứng viên sẽ tạo ra một kết quả xác định, do đó vấn đề sẽ trở thành một quá trình quét tuyến tính trên các lựa chọn độc lập. 

### Tại sao nó hoạt động 

Đối với bất kỳ công suất cố định nào, chiến lược tối ưu được xác định đầy đủ vì các hộp một phần bị cấm và chuột đồng không sử dụng không thể được phân phối lại giữa các loại. Do đó, mỗi loại xác định một kết quả có thể xảy ra. Vì không có sự kết hợp giữa các lựa chọn nên việc so sánh các kết quả này một cách độc lập là đủ để đảm bảo rằng tìm được mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    best_idx = 1
    best_val = -1
    best_boxes = 0

    for i, x in enumerate(a, start=1):
        boxes = n // x
        total = boxes * x

        if total > best_val:
            best_val = total
            best_idx = i
            best_boxes = boxes

    print(best_idx, best_boxes)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo công thức dẫn xuất cho từng loại. Phép chia số nguyên xử lý an toàn tất cả các giá trị lớn vì số nguyên Python không bị tràn. Việc so sánh sử dụng tổng số được vận chuyển làm số liệu chính, trong khi số lượng hộp chỉ được lưu trữ cho đầu ra sau khi xác định được loại tốt nhất. Chỉ mục được theo dõi ở dạng dựa trên 1 để phù hợp với thông số đầu vào. 

Một điểm tinh tế là khởi tạo: thiết lập`best_val`ĐẾN`-1`đảm bảo rằng ngay cả khi`N = 0`, loại đầu tiên vẫn được chọn, tạo ra câu trả lời hợp lệ không có ô nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
19 3
5 4 10
```Chúng tôi đánh giá từng loại một cách độc lập. 

| Loại | Công suất | Hộp = 19 // a | Đã vận chuyển | 
| --- | --- | --- | --- | 
| 1 | 5 | 3 | 15 | 
| 2 | 4 | 4 | 16 | 
| 3 | 10 | 1 | 10 | 

Giá trị vận chuyển tối đa là 16 từ loại 2, với 4 hộp. 

Điều này xác nhận rằng mặc dù loại 1 có dung lượng khá, loại 2 tạo ra mức sử dụng đầy đủ hơn dưới sự ràng buộc của việc đóng gói số nguyên. 

### Ví dụ 2 

đầu vào:```
7 3
8 3 2
```| Loại | Công suất | Hộp = 7 // a | Đã vận chuyển | 
| --- | --- | --- | --- | 
| 1 | 8 | 0 | 0 | 
| 2 | 3 | 2 | 6 | 
| 3 | 2 | 3 | 6 | 

Cả loại 2 và loại 3 đều đạt được giá trị vận chuyển tối đa như nhau. Hoặc là hợp lệ. 

Điều này chứng tỏ rằng công suất lớn hơn`N`tự nhiên trở nên không liên quan, và sự ràng buộc có thể được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(K) | Mỗi khả năng K được đánh giá chính xác một lần bằng cách sử dụng số học thời gian không đổi | 
| Không gian | O(1) | Chỉ một số biến được duy trì ngoài mảng đầu vào | 

Các ràng buộc cho phép lên tới 100.000 loại, do đó, một lần quét tuyến tính cũng nằm trong giới hạn. Mỗi lần lặp chỉ thực hiện phép chia và phép nhân số nguyên, là các phép toán có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    best_idx = 1
    best_val = -1
    best_boxes = 0

    for i, x in enumerate(a, start=1):
        boxes = n // x
        total = boxes * x
        if total > best_val:
            best_val = total
            best_idx = i
            best_boxes = boxes

    return f"{best_idx} {best_boxes}"

# provided sample
assert run("19 3\n5 4 10\n") == "2 4"

# minimum N = 0
assert run("0 3\n5 4 10\n") in ["1 0", "2 0", "3 0"]

# all capacities larger than N
assert run("3 3\n10 11 12\n") in ["1 0", "2 0", "3 0"]

# all equal capacities
assert run("20 3\n5 5 5\n") in ["1 4", "2 4", "3 4"]

# mixed case
assert run("17 4\n3 4 5 6\n") == "2 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| N=0 trường hợp | bất kỳ chỉ số nào 0 | hành vi cạnh zero-hamster | 
| tất cả ai > N | bất kỳ chỉ số nào 0 | không có hộp đầy đủ hợp lệ | 
| năng lực ngang nhau | bất kỳ chỉ số tối đa nào | xử lý cà vạt | 
| trường hợp hỗn hợp | lựa chọn tốt nhất | tối đa hóa chính xác | 

## Vỏ cạnh 

Khi nào`N = 0`, tính toán`N // a[i]`luôn bằng 0, vì vậy mọi loại đều không mang lại chuột đồng được vận chuyển. Thuật toán vẫn chọn loại đầu tiên vì so sánh sử dụng kiểm tra lớn hơn nghiêm ngặt bắt đầu từ`-1`, và tất cả các ứng cử viên hòa nhau ở mức 0. Đầu ra báo cáo chính xác các hộp không. 

Khi tất cả năng lực vượt quá`N`, mỗi ứng cử viên tạo ra các hộp bằng 0. Thuật toán không cố gắng loại bỏ các loại này sớm, điều này tránh được hành vi không chính xác khi lọc logic. Loại đầu tiên được trả về dưới dạng câu trả lời hợp lệ. 

Khi nhiều loại đạt được cùng một giá trị vận chuyển tối đa, thuật toán sẽ giữ nguyên mức tối đa gặp phải đầu tiên. Vì bài toán cho phép bất kỳ câu trả lời đúng nào, nên sự ràng buộc này là hợp lệ và nhất quán.
