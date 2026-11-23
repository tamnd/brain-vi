---
title: "CF 463A - Caisa và Đường"
description: "Chúng ta được cấp một số tiền cố định tính bằng đô la và một danh sách các loại đường có thể lựa chọn trong siêu thị. Mỗi tùy chọn có giá được viết bằng đô la và xu."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 463
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 264 (Div. 2)"
rating: 1200
weight: 463
solve_time_s: 60
verified: true
draft: false
---

[CF 463A - Caisa và Đường](https://codeforces.com/problemset/problem/463/A) 

**Đánh giá:** 1200 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một số tiền cố định tính bằng đô la và một danh sách các loại đường có thể lựa chọn trong siêu thị. Mỗi tùy chọn có giá được viết bằng đô la và xu. Caisa có thể chọn chính xác một loại đường, thanh toán bằng số tiền sẵn có của mình và nhận tiền lẻ nếu anh ta có nhiều tiền hơn yêu cầu. Chi tiết bất thường là cửa hàng trả lại một phần số kẹo thay vì xu, và số kẹo anh ta nhận được chính xác bằng số xu trong số tiền lẻ của anh ta, bị giới hạn bởi thực tế là xu luôn được chuẩn hóa sao cho nhiều nhất là 99 xu còn lại sau khi đổi 100 xu thành một đô la. 

Nhiệm vụ là chọn một loại đường duy nhất mà Caisa có thể mua được và tối đa hóa số lượng kẹo mà anh ta nhận được khi đổi lại. Nếu không có loại đường nào có giá cả phải chăng thì câu trả lời là -1. 

Kích thước đầu vào nhỏ: tối đa 100 tùy chọn. Điều này ngay lập tức loại trừ mọi lo ngại về hiệu suất; thậm chí một vòng lặp kép trên tất cả các mục là chuyện nhỏ. Quét tuyến tính trên danh sách là đủ. 

Một trường hợp phức tạp xuất hiện khi tổng cộng tất cả các mặt hàng đều quá đắt. Ví dụ: nếu Caisa có 5 đô la và mỗi đường có giá ít nhất là 6 đô la thì không thể mua được và câu trả lời phải là -1. Một trường hợp khác là khi nhiều mặt hàng có giá cả phải chăng nhưng một mặt hàng có giá thấp hơn trong khi mặt hàng khác mang lại nhiều xu hơn; sự lựa chọn tối ưu không phải là mặt hàng rẻ nhất mà là mặt hàng tối đa hóa phần còn lại tính bằng xu sau khi trừ đi. 

Một ví dụ cụ thể có thể gây nhầm lẫn cho cách tiếp cận ngây thơ là việc chọn hạng mục có chi phí tối thiểu thay vì tối đa hóa sự thay đổi. Giả sử Caisa có 10 đô la. Một mặt hàng có giá 9 đô la 70 xu và một mặt hàng khác có giá 7 đô la 0 xu. Cái đầu tiên chỉ có 30 xu tiền lẻ, trong khi cái thứ hai có giá trị thay đổi 300 xu, tương ứng với 3 đô la và 0 xu, tức là 0 cái kẹo. Nếu không tính toán kỹ phần còn lại tính bằng cent thì rất dễ so sánh sai. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực đương nhiên tuân theo các ràng buộc. Chúng tôi lặp lại từng loại đường, kiểm tra xem Caisa có đủ khả năng chi trả hay không, tính toán sự thay đổi bằng xu và theo dõi phần còn lại tối đa. Vì có tối đa 100 mục nên đây chỉ là 100 phép kiểm tra số học, con số này không đáng kể. 

Quan sát chính là vấn đề hoàn toàn mang tính cục bộ đối với từng hạng mục. Không có sự tương tác giữa các lựa chọn, không có cấu trúc tổ hợp và không cần tối ưu hóa trên nhiều lựa chọn. Mỗi loại đường độc lập tạo ra một giá trị dự kiến: còn lại bao nhiêu xu sau khi trừ đi giá của nó khỏi số tiền hiện có. 

Chúng tôi chuyển đổi mọi thứ thành xu để tránh suy luận dấu phẩy động. Nếu Caisa có s đô la, số tiền đó sẽ trở thành 100 xu. Mỗi mặt hàng có giá 100xᵢ + yᵢ xu. Nếu chi phí vượt quá số tiền sẵn có, chúng tôi sẽ bỏ qua. Ngược lại, chúng tôi tính toán phần còn lại và cập nhật câu trả lời đúng nhất. 

Cách tiếp cận bạo lực đã là tối ưu vì kích thước đầu vào có giới hạn không đổi và nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chuyển đổi số tiền hiện có thành xu bằng cách tính 100 * s. Điều này đảm bảo tất cả các so sánh diễn ra trong một hệ thống đơn vị duy nhất, loại bỏ mọi sự mơ hồ giữa đô la và xu. 
2. Khởi tạo một biến tốt nhất thành -1. Điều này sẽ lưu trữ số tiền lẻ tối đa trong số tất cả các lựa chọn hợp lý. 
3. Đối với mỗi loại đường, quy đổi giá thành xu bằng cách sử dụng 100 * xᵢ + yᵢ. Điều này mang lại một số lượng tương đương trong cùng một đơn vị với ngân sách. 
4. Nếu giá vượt quá số tiền hiện có, hãy bỏ qua tùy chọn này vì không thể mua được. 
5. Nếu không thì hãy tính thay đổi dưới dạng (ngân sách - giá). Trích xuất phần xu bằng modulo 100, vì kẹo tương ứng với xu trong phần thay đổi còn lại sau khi chuẩn hóa. 
6. Cập nhật tốt nhất nếu giá trị này lớn hơn giá trị tốt nhất hiện tại. 
7. Sau khi xử lý tất cả các mục, kết quả đầu ra là tốt nhất. Nếu không tìm thấy mục hợp lệ, tốt nhất vẫn là -1. 

### Tại sao nó hoạt động 

Mỗi loại đường tạo ra chính xác một giá trị ứng cử viên độc lập, đó là số xu còn lại sau khi mua. Vì Caisa mua chính xác một mặt hàng nên vấn đề giảm xuống còn việc chọn giá trị lớn nhất trong một tập hợp hữu hạn các đánh giá độc lập. Chuyển đổi mọi thứ thành xu sẽ duy trì trật tự và đảm bảo không mất độ chính xác. Thuật toán duy trì tính bất biến tốt nhất luôn là số xu thay đổi tối đa được thấy trong số tất cả các mặt hàng giá cả phải chăng được xử lý cho đến nay, do đó giá trị cuối cùng là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, s = map(int, input().split())
    budget = s * 100

    best = -1

    for _ in range(n):
        x, y = map(int, input().split())
        cost = x * 100 + y

        if cost <= budget:
            change = budget - cost
            sweets = change % 100
            if sweets > best:
                best = sweets

    print(best)

if __name__ == "__main__":
    main()
```Giải pháp trước tiên bình thường hóa ngân sách thành xu để các so sánh được nhất quán. Mỗi mục cũng được chuyển đổi thành xu trước khi thực hiện bất kỳ phép tính nào, tránh các lỗi đơn vị hỗn hợp. 

Chi tiết triển khai chính là sử dụng modulo 100 cho thay đổi còn lại. Vì đồ ngọt tương ứng với xu trong số tiền còn lại sau khi bình thường hóa toàn bộ đô la, nên chỉ phần còn lại sau khi chia cho 100 mới là vấn đề. Chỉ theo dõi giá trị đó để tránh tính toán không cần thiết. 

Xử lý cạnh là ngầm định: nếu không có mục nào đáp ứng chi phí <= ngân sách, tốt nhất không bao giờ cập nhật từ -1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 10
3 90
12 0
9 70
5 50
7 0
```Chúng tôi theo dõi ngân sách = 1000 xu. 

| Mục | Chi phí (xu) | Giá cả phải chăng | Thay đổi (xu) | Kẹo | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 3 90 | 390 | vâng | 610 | 10 | 10 | 
| 12 0 | 1200 | không | - | - | 10 | 
| 9 70 | 970 | vâng | 30 | 30 | 30 | 
| 5 50 | 550 | vâng | 450 | 50 | 50 | 
| 7 0 | 700 | vâng | 300 | 0 | 50 | 

Câu trả lời cuối cùng là 50. 

Dấu vết này cho thấy mức tối đa không tương ứng với mặt hàng rẻ nhất mà tương ứng với phần còn lại tốt nhất theo modulo 100. 

### Ví dụ 2 

đầu vào:```
3 2
3 0
2 50
5 99
```Ngân sách = 200 xu. 

| Mục | Chi phí | Giá cả phải chăng | Thay đổi | Kẹo | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 3 0 | 300 | không | - | - | -1 | 
| 2 50 | 250 | không | - | - | -1 | 
| 5 99 | 599 | không | - | - | -1 | 

Câu trả lời cuối cùng là -1. 

Điều này khẳng định việc xử lý trường hợp “mua hàng không hợp lệ”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi loại đường được xử lý một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giới hạn đầu vào n ở mức 100, do đó, ngay cả quá trình quét tuyến tính cũng có hiệu quả tức thời. Việc sử dụng bộ nhớ là không đổi bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, s = map(int, input().split())
    budget = s * 100

    best = -1

    for _ in range(n):
        x, y = map(int, input().split())
        cost = x * 100 + y
        if cost <= budget:
            best = max(best, (budget - cost) % 100)

    return str(best)

# provided sample
assert run("5 10\n3 90\n12 0\n9 70\n5 50\n7 0\n") == "50"

# minimum case, no affordable item
assert run("1 1\n2 0\n") == "-1"

# exact budget match, zero change
assert run("2 5\n5 0\n3 0\n") == "0"

# multiple options, best is not cheapest
assert run("3 10\n9 99\n9 50\n8 0\n") == "50"

# all affordable, check modulo behavior
assert run("3 10\n1 10\n1 20\n1 30\n") == "90"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có mặt hàng giá cả phải chăng | -1 | xử lý đúng đắn những điều không thể | 
| khớp chính xác | 0 | trường hợp không thay đổi | 
| giá hỗn hợp | 50 | lựa chọn theo số dư tối đa, không tính phí | 
| tất cả giá cả phải chăng | 90 | trích xuất modulo chính xác | 

## Vỏ cạnh 

Trường hợp một bên là khi tất cả các mặt hàng đều quá đắt. Trong trường hợp đó, vòng lặp không bao giờ cập nhật giá trị ban đầu -1 và đầu ra vẫn là -1. Ví dụ: với ngân sách 100 xu và một mặt hàng có giá 200 xu, việc kiểm tra không thành công và không có tính toán thay đổi nào được thực hiện, bảo toàn chính xác trọng điểm. 

Một trường hợp khác là khi nhiều mặt hàng có giá cả phải chăng nhưng lại tạo ra số dư giống hệt nhau. Ví dụ: hai mặt hàng đều có thể để lại 30 xu. Thuật toán xử lý việc này một cách tự nhiên vì nó chỉ theo dõi giá trị tối đa và các giá trị bằng nhau không làm thay đổi kết quả. 

Trường hợp cuối cùng là khi thay đổi là bội số của 100 xu. Trong trường hợp đó, phép toán modulo mang lại kết quả 0, biểu thị chính xác số kẹo bằng 0.
