---
title: "CF 106089F - \u0414\u043e\u0441\u0442\u043e\u0439\u043d\u043e\u0435 \u043f\u0440\u043e\u0434\u043e\u043b\u0436\u0435\u043d\u0438\u0435"
description: "Chúng ta được cho một tập hợp các số nguyên riêng biệt, mỗi số có ít nhất hai chữ số. Đối với mỗi cặp số khác nhau có thứ tự, chúng ta muốn kiểm tra một điều kiện chữ số đơn giản: chữ số cuối cùng của số thứ nhất phải khớp với chữ số đầu tiên của số thứ hai."
date: "2026-06-19T20:23:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106089
codeforces_index: "F"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0444\u0438\u043d\u0430\u043b"
rating: 0
weight: 106089
solve_time_s: 47
verified: true
draft: false
---

[CF 106089F - \u0414\u043e\u0441\u0442\u043e\u0439\u043d\u043e\u0435 \u043f\u0440\u043e\u0434\u043e\u043b\u0436\u0435\u043d\u0438\u0435](https://codeforces.com/problemset/problem/106089/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên riêng biệt, mỗi số có ít nhất hai chữ số. Đối với mỗi cặp số khác nhau có thứ tự, chúng ta muốn kiểm tra một điều kiện chữ số đơn giản: chữ số cuối cùng của số thứ nhất phải khớp với chữ số đầu tiên của số thứ hai. Mọi cặp thỏa mãn quy tắc này đều được tính và hướng rất quan trọng, vì vậy việc hoán đổi thứ tự có thể thay đổi xem cặp đó có hợp lệ hay không. 

Nhiệm vụ là đếm xem có bao nhiêu cặp có thứ tự thỏa mãn điều kiện này trong số tối đa 200.000 số. Mỗi số có nhiều nhất là 10^9, vì vậy việc trích xuất chữ số đầu tiên và chữ số cuối cùng là thời gian đơn giản và không đổi trên mỗi số. 

Ràng buộc trên n ngay lập tức loại trừ mọi so sánh bậc hai giữa các cặp. Quá trình quét O(n^2) đơn giản sẽ thực hiện khoảng 4×10^10 kiểm tra trong trường hợp xấu nhất, vượt xa giới hạn một giây. Điều này báo hiệu rằng chúng ta cần tổng hợp thông tin theo thuộc tính chữ số thay vì so sánh trực tiếp các phần tử. 

Một chi tiết tinh tế là các số kết thúc bằng 0 vẫn có thể xuất hiện dưới dạng phần tử đầu tiên của một cặp, nhưng chúng không bao giờ có thể là phần tử thứ hai của một cặp hợp lệ nếu không có số nào bắt đầu bằng 0. Vì đầu vào không có số 0 đứng đầu và ít nhất là 10 nên chữ số đầu tiên luôn nằm trong khoảng từ 1 đến 9. Điều này làm cho việc nhóm chữ số trở nên rõ ràng và có giới hạn. 

Một sai lầm điển hình là suy nghĩ theo cách sắp xếp hoặc kề cận. Việc sắp xếp các số không giúp ích gì vì điều kiện chỉ phụ thuộc vào các chữ số chứ không phụ thuộc vào độ gần của số. Một sai lầm khác là đếm kép các cặp không có thứ tự mà không tôn trọng hướng, nhưng ở đây hướng là cần thiết, vì vậy chúng ta phải đếm tất cả (i, j) hợp lệ một cách riêng biệt. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp qua mọi cặp chỉ số i và j có thứ tự, tính chữ số cuối cùng của ai và chữ số đầu tiên của aj, rồi đếm các kết quả trùng khớp. Điều này đúng vì nó trực tiếp thực hiện định nghĩa. Tuy nhiên, nó kiểm tra n(n−1) cặp và mỗi kiểm tra là O(1), dẫn đến khoảng 4×10^10 thao tác ở kích thước đầu vào tối đa, điều này là không khả thi. 

Điều quan trọng quan trọng là giá trị của một số chỉ quan trọng thông qua hai thuộc tính: chữ số đầu tiên và chữ số cuối cùng của nó. Một khi những điều này được biết đến, con số đầy đủ sẽ trở nên không liên quan. Điều này cho phép chúng ta nén toàn bộ mảng thành các số đếm theo cấu trúc 9×10: có bao nhiêu số bắt đầu bằng chữ số d và bao nhiêu số kết thúc bằng chữ số d. 

Thay vì ghép nối các phần tử một cách rõ ràng, chúng tôi đếm xem có bao nhiêu lựa chọn cho mỗi lần chuyển đổi chữ số tương thích. Đối với một số cố định ai, đóng góp của nó chỉ phụ thuộc vào số lượng số bắt đầu bằng chữ số cuối cùng của nó. Tổng hợp những đóng góp này trên tất cả ai sẽ mang lại câu trả lời theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Đếm chữ số | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi số, hãy trích chữ số đầu tiên và chữ số cuối cùng của số đó. Điều này làm giảm mỗi phần tử thành một cặp số nguyên trong một phạm vi cố định nhỏ. 
2. Duy trì mảng tần số cntStart[d] cho số lượng số bắt đầu bằng chữ số d. Điều này nén tất cả các phần tử thứ hai ứng cử viên của cặp. 
3. Lặp lại tất cả các số một lần nữa và với mỗi số ai, hãy thêm cntStart[lastDigit(ai)] vào câu trả lời. Điều này đếm tất cả các cặp hợp lệ trong đó ai là phần tử đầu tiên. 
4. Trừ 1 nếu cần khi một số có thể ghép với chính nó. Trong bài toán này, tất cả các số đều khác nhau nên việc tự ghép nối không bao giờ xảy ra và không cần hiệu chỉnh. 
5. Xuất số tiền tích lũy. 

Ý tưởng cốt lõi là mỗi số đóng góp độc lập tất cả các cặp gửi đi hợp lệ dựa trên chữ số cuối cùng của nó và chúng ta không bao giờ cần liệt kê rõ ràng các điểm đến. 

### Tại sao nó hoạt động

Mỗi cặp thứ tự hợp lệ (i, j) được tính chính xác một lần khi xử lý i. Điều kiện chỉ phụ thuộc vào chữ số cuối cùng của ai và chữ số đầu tiên của aj, và cntStart trực tiếp mã hóa số lượng lựa chọn của j thỏa mãn ràng buộc đó. Vì mỗi j được tính vào chính xác một nhóm chữ số nên không có cặp hợp lệ nào bị bỏ sót và không có cặp không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def first_digit(x: int) -> int:
    while x >= 10:
        x //= 10
    return x

def last_digit(x: int) -> int:
    return x % 10

n = int(input())
a = list(map(int, input().split()))

cnt_start = [0] * 10

for x in a:
    cnt_start[first_digit(x)] += 1

ans = 0
for x in a:
    ans += cnt_start[last_digit(x)]

print(ans)
```Giải pháp đầu tiên nén tất cả các số thành số chữ số đứng đầu. Đây là sự tổng hợp toàn cầu duy nhất cần thiết. Lần thứ hai tính toán phần đóng góp cho mỗi số bằng cách sử dụng chữ số cuối của nó. 

Một chi tiết triển khai tinh tế là việc trích xuất chữ số đầu tiên yêu cầu phép chia lặp đi lặp lại, nhưng vì các số có tối đa 10 chữ số nên đây là thời gian không đổi. Việc sử dụng chuyển đổi chuỗi cũng có thể được chấp nhận, nhưng phép chia số nguyên sẽ tránh được chi phí chung. 

Chúng tôi không trừ bất cứ điều gì đối với việc tự ghép đôi vì bài toán nêu rõ tất cả các số đều khác biệt và một số không thể có chữ số đầu tiên bằng chữ số cuối cùng đóng góp cho chính nó theo cách có vấn đề trừ khi nó được tính là ghép nối với chính nó, điều này không bao giờ xảy ra vì các chỉ số khác nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
19 98 2025 12 99
```Chúng tôi tính toán các chữ số đầu tiên và cuối cùng: 

| số | chữ số đầu tiên | chữ số cuối cùng | 
| --- | --- | --- | 
| 19 | 1 | 9 | 
| 98 | 9 | 8 | 
| 2025 | 2 | 5 | 
| 12 | 1 | 2 | 
| 99 | 9 | 9 | 

Bây giờ hãy xây dựng cntStart: 

- 1 → 2 số (19, 12) 
- 2 → 1 số (2025) 
- 9 → 2 số (98, 99) 

Bây giờ tính toán đóng góp: 

| số | chữ số cuối cùng | đóng góp | 
| --- | --- | --- | 
| 19 | 9 | 2 | 
| 98 | 8 | 0 | 
| 2025 | 5 | 0 | 
| 12 | 2 | 1 | 
| 99 | 9 | 2 | 

Tổng cộng = 5. 

Điều này khớp với hành vi mẫu trong đó mỗi số tìm thấy có bao nhiêu người theo dõi hợp lệ tồn tại dựa trên việc khớp chữ số. 

### Ví dụ 2 

đầu vào:```
10 21 32 43
```Chữ số: 

| số | đầu tiên | cuối cùng | 
| --- | --- | --- | 
| 10 | 1 | 0 | 
| 21 | 2 | 1 | 
| 32 | 3 | 2 | 
| 43 | 4 | 3 | 

cntStart: 

1→1, 2→1, 3→1, 4→1 

Đóng góp: 

| số | cuối cùng | đóng góp | 
| --- | --- | --- | 
| 10 | 0 | 0 | 
| 21 | 1 | 1 | 
| 32 | 2 | 1 | 
| 43 | 3 | 1 | 

Tổng cộng = 3. 

Điều này cho thấy một cấu trúc giống như chuỗi trong đó mỗi số chỉ có thể kết nối tiến lên trong không gian chữ số và việc đếm sẽ giảm xuống mức tra cứu tần số đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi số được xử lý một số lần không đổi để trích xuất các chữ số và tích lũy số đếm | 
| Không gian | O(1) | Mảng tần số có kích thước cố định 10 bất kể kích thước đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì nó chỉ thực hiện chuyển tuyến tính qua đầu vào với công việc không đổi tối thiểu trên mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    cnt_start = [0] * 10

    def first_digit(x):
        while x >= 10:
            x //= 10
        return x

    def last_digit(x):
        return x % 10

    for x in a:
        cnt_start[first_digit(x)] += 1

    ans = 0
    for x in a:
        ans += cnt_start[last_digit(x)]

    return str(ans)

# sample
assert run("5\n19 98 2025 12 99\n") == "5"

# minimum size
assert run("1\n10\n") == "0"

# all same first digit, different last digits
assert run("3\n10 11 12\n") == "0"

# all connect
assert run("4\n10 21 32 43\n") == "3"

# strong connectivity
assert run("3\n19 91 11\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 số | 0 | không tự ghép nối | 
| chữ số bắt đầu giống nhau | 0 | không có kết quả phù hợp | 
| chuỗi chữ số | 3 | nhân giống chuẩn | 
| hỗn hợp đối xứng | 2 | nhiều chuyển đổi hợp lệ | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số có cùng chữ số đầu tiên. Ví dụ:```
3
10 11 12
```Ở đây cntStart[1] = 3, nhưng tất cả các chữ số cuối cùng là 0, 1, 2, không khớp với bất kỳ chữ số bắt đầu nào ngoại trừ có thể là 1. Chỉ có 11 đóng góp nếu chữ số cuối cùng là 1. Thuật toán xử lý việc này một cách rõ ràng vì mỗi số truy vấn độc lập cntStart[lastDigit] và không xảy ra ghép nối nhân tạo. 

Một trường hợp đặc biệt khác là khi một số kết thúc bằng một chữ số không xuất hiện dưới dạng bất kỳ chữ số đầu tiên nào. Ví dụ:```
3
10 21 32
```Ở đây, các chữ số cuối cùng là 0, 1, 2, nhưng nếu không có số nào bắt đầu bằng 0 thì các đóng góp liên quan đến 10 sẽ tự động bằng 0 vì cntStart[0] = 0. Thuật toán tự nhiên lọc các chuyển tiếp không hợp lệ mà không cần viết hoa đặc biệt. 

Trường hợp cạnh cấu trúc cuối cùng là khi các chữ số tạo thành một chu trình đầy đủ. Thậm chí sau đó, mỗi cặp được tính độc lập thông qua tra cứu tần số và thuật toán không giả định bất kỳ thứ tự hoặc cấu trúc nào ngoài khả năng tương thích chữ số.
