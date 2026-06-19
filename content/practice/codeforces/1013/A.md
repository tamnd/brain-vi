---
title: "CF 1013A - Cọc Đá"
description: "Chúng ta được cung cấp hai bức ảnh chụp nhanh của cùng một hệ thống cọc đá. Trong ảnh chụp nhanh đầu tiên, mỗi cọc có một số lượng đá và trong ảnh chụp nhanh thứ hai, các cọc có số lượng khác nhau."
date: "2026-06-16T22:32:08+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1013
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 500 (Div. 2) [based on EJOI]"
rating: 800
weight: 1013
solve_time_s: 75
verified: true
draft: false
---

[CF 1013A - Cọc có đá](https://codeforces.com/problemset/problem/1013/A) 

**Đánh giá:** 800 
**Thẻ:** toán 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai bức ảnh chụp nhanh của cùng một hệ thống cọc đá. Trong ảnh chụp nhanh đầu tiên, mỗi cọc có một số lượng đá và trong ảnh chụp nhanh thứ hai, các cọc có số lượng khác nhau. Giữa hai thời điểm đó, trong đêm, hai loại hoạt động có thể xảy ra nhiều lần: một hòn đá có thể được lấy ra khỏi một đống hoàn toàn, hoặc một hòn đá có thể được di chuyển từ đống này sang đống khác. Không có hạn chế về số lượng hành động như vậy xảy ra hoặc số lượng người khác nhau thực hiện chúng, vì vậy chúng tôi chỉ quan tâm đến việc liệu có thể chuyển đổi cấu hình đầu tiên thành cấu hình thứ hai bằng cách sử dụng hai thao tác nguyên thủy này hay không. 

Vấn đề không phải là xây dựng một chuỗi các nước đi mà chỉ là quyết định tính khả thi. Chúng tôi đang kiểm tra một cách hiệu quả xem một mảng số nguyên có thể được chuyển đổi thành một mảng khác hay không bằng cách sử dụng các thao tác “xóa 1 khỏi bất kỳ chỉ mục nào” và “chuyển 1 đơn vị giữa các chỉ mục”. 

Các hạn chế là nhỏ, tối đa là 50 cọc. Điều đó ngay lập tức loại trừ mọi nhu cầu về cấu trúc dữ liệu nặng hoặc tối ưu hóa ngoài lý luận tuyến tính hoặc bậc hai. Bất kỳ giải pháp nào là O(n²) hoặc thậm chí O(n) đều đủ nhanh và vấn đề thực sự là hiểu được những hoạt động này bảo toàn những gì và chúng có thể thay đổi những gì. 

Một trường hợp thất bại tinh tế xuất hiện khi tổng số viên đá thay đổi theo cách không thể giải thích được bằng cách loại bỏ hoặc khi việc phân bổ lại không đủ để phù hợp với hình dạng mục tiêu. 

Ví dụ: nếu chúng ta có:```
x = [1, 0]
y = [0, 2]
```Điều này là không thể, vì chúng ta không thể tạo thêm đá. Di chuyển chỉ phân phối lại hoặc xóa đá. 

Một ví dụ khác:```
x = [0, 0]
y = [1, 1]
```Điều này cũng không thể thực hiện được vì lý do tương tự: không có thao tác nào làm tăng tổng số đá. 

Những ví dụ này cho thấy khó khăn cốt lõi không phải là hành vi trên mỗi cọc mà là cấu trúc bất bình đẳng và bảo tồn toàn cầu. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng tất cả các chuỗi di chuyển và loại bỏ có thể xảy ra. Mỗi bước sẽ giảm tổng số đá hoặc chuyển một đơn vị giữa các vị trí. Mặc dù hệ số phân nhánh lớn về mặt khái niệm nhưng không gian trạng thái vẫn rất lớn vì các cọc có thể nhận nhiều giá trị trung gian. Điều này ngay lập tức trở nên khó giải quyết, vì ngay cả những giá trị nhỏ của n và kích thước cọc cũng tạo ra nhiều trạng thái có thể tiếp cận được theo cấp số nhân. 

Quan sát chính là các hoạt động có thể được hiểu trên toàn cầu hơn là cục bộ. Việc di chuyển một viên đá không làm thay đổi tổng số viên đá, trong khi việc loại bỏ một viên đá sẽ làm giảm tổng số đúng một viên. Điều này có nghĩa là tác động không thể đảo ngược duy nhất trong hệ thống là làm giảm tổng của tất cả các phần tử. 

Từ đó, chúng ta có được sự đơn giản hóa mạnh mẽ: bắt đầu từ mảng ban đầu, chúng ta có thể phân phối lại các viên đá một cách tùy ý bằng cách sử dụng các bước di chuyển, do đó hạn chế thực sự duy nhất là liệu chúng ta có đủ số lượng đá tổng thể để phù hợp với cấu hình mục tiêu hay không. Nếu mục tiêu yêu cầu nhiều đá hơn chúng ta có ban đầu thì điều đó là không thể. Nếu chúng ta có ít nhất nhiều viên đá, chúng ta luôn có thể loại bỏ những phần bổ sung và sắp xếp lại phần còn lại bằng cách chuyển. 

Do đó, toàn bộ vấn đề giảm xuống việc so sánh tổng của cả hai mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Cao | Quá chậm | 
| So sánh tổng | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số viên đá trong cấu hình ban đầu. Điều này đại diện cho tất cả các viên đá chúng ta bắt đầu trước bất kỳ hoạt động nào. 
2. Tính tổng số đá trong cấu hình cuối cùng. Đây là khối lượng đá phải còn lại sau mọi hoạt động. 
3. So sánh hai tổng. Nếu tổng ban đầu nhỏ hơn tổng cuối cùng, ghi “Không”, vì chúng ta không thể tạo đá mới. 
4. Nếu không, hãy ghi “Có”, vì bất kỳ viên đá dư thừa nào cũng có thể được loại bỏ và mọi khác biệt về phân bổ đều có thể được khắc phục bằng cách chuyển đổi. 

### Tại sao nó hoạt động 

Bất biến quan trọng là các lần chuyển tiền sẽ bảo toàn tổng số tiền, trong khi việc xóa sẽ làm giảm chính xác một số tiền cho mỗi thao tác. Do đó, hạn chế duy nhất mà quy trình áp đặt là tổng cuối cùng không thể vượt quá tổng ban đầu. Vì chúng ta có thể tự do di chuyển đá giữa các cọc nên mọi sự phân phối lại đều có thể đạt được khi chúng ta có đủ tổng khối lượng. Không có cấu trúc ẩn như ràng buộc chẵn lẻ hoặc ràng buộc trên mỗi chỉ mục, bởi vì không có thao tác nào giới hạn cách sắp xếp lại các khối ngoại trừ thực tế là chúng ta không thể tạo các khối mới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    if sum(x) >= sum(y):
        print("Yes")
    else:
        print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đọc cả hai mảng và tính tổng của chúng một cách trực tiếp. Sự so sánh là đủ vì tất cả các thao tác được phép đều bảo toàn hoặc giảm tổng số đá, không bao giờ tăng thêm. 

Một lỗi triển khai phổ biến là cố gắng khớp từng cọc riêng lẻ. Điều đó là không cần thiết và gây hiểu nhầm, bởi vì việc chuyển giao cho phép định hình lại một cách tùy ý. Chỉ có tổng số tiền toàn cầu mới quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5
x = [1, 2, 3, 4, 5]
y = [2, 1, 4, 3, 5]
```| Bước | tổng(x) | tổng (y) | Quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 15 | 15 | so sánh | 
| cuối cùng | 15 | 15 | bằng | 

Chúng tôi thấy cả hai tổng số đều bằng nhau nên không cần phải loại bỏ viên đá nào. Vì việc chuyển giao cho phép phân phối lại tùy ý nên chúng ta có thể biến x thành y bằng cách di chuyển các viên đá giữa các cọc. 

Đầu ra là “Có”. 

### Ví dụ 2 

đầu vào:```
n = 4
x = [1, 1, 1, 1]
y = [3, 1, 1, 1]
```| Bước | tổng(x) | tổng (y) | Quyết định | 
| --- | --- | --- | --- | 
| ban đầu | 4 | 6 | so sánh | 

Ở đây mục tiêu cần 6 viên đá trong khi ban đầu chỉ có 4 viên tồn tại. Vì chúng ta không thể tạo ra đá nên việc biến đổi là không thể dù có phân phối lại hay không. 

Đầu ra là “Không”. 

Những ví dụ này cho thấy sự bình đẳng cho phép sự linh hoạt hoàn toàn, trong khi sự thiếu hụt ngay lập tức cản trở tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi tổng hợp hai mảng có kích thước n một lần | 
| Không gian | O(1) | Chỉ tổng số đang chạy mới được lưu trữ | 

Các ràng buộc cho phép tối đa 50 cọc, do đó, một đường chuyền tuyến tính duy nhất là không đáng kể. Ngay cả khi n lớn hơn nhiều thì cách tiếp cận này vẫn tối ưu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    x = list(map(int, input().split()))
    y = list(map(int, input().split()))

    return "Yes" if sum(x) >= sum(y) else "No"

# provided sample
assert run("5\n1 2 3 4 5\n2 1 4 3 5\n") == "Yes"

# custom: insufficient total
assert run("3\n1 1 1\n2 2 2\n") == "No"

# custom: exact match but different distribution
assert run("4\n0 5 0 0\n1 1 1 2\n") == "Yes"

# custom: all zeros
assert run("3\n0 0 0\n0 0 0\n") == "Yes"

# custom: single pile decrease
assert run("1\n5\n3\n") == "Yes"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị bằng nhau | Có | chỉ phân phối lại | 
| số tiền không đủ | Không | không thể thực hiện được do yêu cầu sáng tạo | 
| phân phối lại thưa thớt | Có | chuyển nhượng có thể định hình lại tùy ý | 
| tất cả số không | Có | trường hợp cạnh tầm thường | 
| cọc đơn | Có | hành vi n tối thiểu | 

## Vỏ cạnh 

Khi tất cả các cọc ban đầu bằng 0 và cuối cùng cũng bằng 0, cả hai tổng đều bằng 0, do đó thuật toán sẽ đưa ra chính xác “Có” ngay lập tức. 

Đối với trường hợp như:```
x = [2, 0]
y = [1, 1]
```thuật toán tính sum(x) = 2 và sum(y) = 2 nên trả về “Có”. Mặc dù sự phân bổ khác nhau, một thao tác chuyển duy nhất sẽ di chuyển một viên đá từ đống 1 sang đống 2, khớp chính xác với mục tiêu. 

Đối với trường hợp như:```
x = [1, 0]
y = [0, 2]
```chúng ta nhận được tổng (x) = 1 và tổng (y) = 2. Thuật toán trả về chính xác “Không”, bởi vì ngay cả số lần chuyển không giới hạn cũng không thể bù cho tổng số đá bị thiếu. 

Mỗi trường hợp cạnh đều xác nhận một bất biến giống nhau: chỉ có tổng số lượng đá là quan trọng và việc phân phối lại không có ràng buộc bổ sung nào.
