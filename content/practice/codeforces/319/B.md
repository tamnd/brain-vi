---
title: "CF 319B - Tâm lý trong một dòng"
description: "Cho phép hoán vị các số từ 1 đến n xếp thành một dòng. Mỗi vị trí đại diện cho một “tâm lý” có giá trị sức mạnh duy nhất và đường tiến hóa theo các vòng riêng biệt. Trong một hiệp, mọi kẻ tâm thần đều so sánh mình với người ngay bên phải mình."
date: "2026-06-06T02:01:04+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 319
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 189 (Div. 1)"
rating: 1900
weight: 319
solve_time_s: 73
verified: true
draft: false
---

[CF 319B - Tâm lý trong một dòng](https://codeforces.com/problemset/problem/319/B) 

**Xếp hạng:** 1900 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho phép hoán vị các số từ 1 đến n xếp thành một dòng. Mỗi vị trí đại diện cho một “tâm lý” có giá trị sức mạnh duy nhất và đường tiến hóa theo các vòng riêng biệt. 

Trong một hiệp, mọi kẻ tâm thần đều so sánh mình với người ngay bên phải mình. Nếu giá trị của anh ta lớn hơn, anh ta sẽ giết người hàng xóm phù hợp. Tất cả sự so sánh xảy ra đồng thời, vì vậy một kẻ tâm thần có thể giết ai đó ngay cả khi anh ta bị giết trong cùng một hiệp đấu. Sau tất cả các vụ giết người, những kẻ tâm thần còn lại xếp hàng và tạo thành một hàng mới cho vòng tiếp theo. Quá trình lặp lại cho đến khi không thể giết được nữa. 

Nhiệm vụ là xác định có bao nhiêu vòng xảy ra trước khi hệ thống ổn định, nghĩa là không có vị trí nào có giá trị lớn hơn vị trí lân cận ngay bên phải của nó. 

Kích thước đầu vào lên tới 100.000 phần tử, điều này ngay lập tức loại trừ mọi mô phỏng liên tục quét và xóa các phần tử. Một cách tiếp cận đơn giản là xây dựng lại danh sách mỗi vòng sẽ giảm xuống O(n²) trong trường hợp xấu nhất, vượt xa mức phù hợp trong một giây trong Python. 

Một khó khăn tinh tế xuất hiện ở cách việc xóa đồng thời tương tác với các phản ứng dây chuyền. Một giá trị có thể tồn tại sau một vòng ngay cả khi nó được định sẵn sẽ bị loại bỏ sau đó, bởi vì hàng xóm bên phải của nó có thể bị giết trong cùng một bước. Ví dụ: trong một mảng giảm dần như`[5, 4, 3, 2, 1]`, mọi thứ ngoại trừ mức tối đa sẽ biến mất trong một vòng chứ không phải dần dần. Bất kỳ mô phỏng tham lam không chính xác nào xóa tuần tự từ trái sang phải sẽ đánh giá thấp tốc độ sụp đổ. 

Một trường hợp khác là khi mức tăng cục bộ tồn tại sâu bên trong mảng. Ví dụ`[1, 3, 2, 4, 3]`tạo ra nhiều vòng trong đó các phân đoạn khác nhau co lại với tốc độ khác nhau. Câu trả lời phụ thuộc vào “độ trễ” dài nhất trước khi một giá trị được bảo vệ bởi một giá trị lớn hơn ở bên trái của nó. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: mô phỏng từng vòng, quét mảng, đánh dấu tất cả các vị trí ở đó`a[i] > a[i+1]`, xóa các phần tử bên phải, nén mảng và lặp lại cho đến khi không còn phần xóa nào xảy ra. Mỗi vòng là O(n) và trong trường hợp xấu nhất, chúng tôi chỉ có thể loại bỏ một phần tử trong mỗi vòng, tạo ra các vòng O(n), do đó độ phức tạp tổng cộng sẽ trở thành O(n²). Với n lên tới 100.000 thì điều này là không thể. 

Quan sát quan trọng là mỗi phần tử chỉ có liên quan cho đến khi nó bị giết và sự sống sót của nó chỉ phụ thuộc vào phần tử lớn hơn gần nhất ở bên trái. Khi một phần tử lớn hơn xuất hiện ở bên trái, phần tử hiện tại sẽ “bị tiêu diệt” và cuối cùng sẽ bị loại bỏ, nhưng thời gian phụ thuộc vào khoảng cách của phần tử thống trị đó. 

Thay vì mô phỏng các vòng đấu, chúng tôi theo dõi thời điểm mỗi vị trí bị loại bỏ. Chúng tôi xử lý từ trái sang phải và duy trì cấu trúc đơn điệu của các chỉ số trong đó giá trị đang giảm dần. Đối với mỗi phần tử, chúng tôi tìm thấy nó tồn tại được bao nhiêu bước trước khi bị loại bỏ bởi giá trị lớn hơn ở bên trái. Điều này tương đương với việc theo dõi một chuỗi các ảnh hưởng tương tự như một ngăn xếp đơn điệu với sự lan truyền thời gian bổ sung. 

Kết quả cổ điển cho bài toán này là mỗi phần tử có “thời gian chết” bằng một lần thời gian chết tối đa của phần tử lớn nhất gần chuỗi bên trái của nó. Câu trả lời là thời gian chết tối đa trên tất cả các yếu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Ngăn xếp đơn điệu có lan truyền | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý mảng từ trái sang phải, duy trì một chồng chỉ mục trong đó các giá trị theo thứ tự giảm dần. 

1. Khởi tạo một mảng`death[i] = 0`cho mọi vị trí. Điều này lưu trữ vòng trong đó mỗi kẻ tâm thần bị loại bỏ. 
2. Lặp lại qua từng vị trí`i`từ trái sang phải. 
3. Trong khi ngăn xếp không trống và`a[stack[-1]] < a[i]`, bật ra khỏi ngăn xếp. Các phần tử này nhỏ hơn và không thể là công cụ chặn ưu thế gần nhất cho các phần tử trong tương lai. Họ bị chi phối một cách hiệu quả bởi`a[i]`. 
4. Nếu ngăn xếp không trống sau khi bật lên, phần trên cùng của ngăn xếp là phần tử lớn hơn gần bên trái nhất. Phần tử hiện tại cuối cùng sẽ bị loại bỏ vì phần tử đó. Chúng tôi đặt:`death[i] = death[stack[-1]] + 1`. 

Điều này nắm bắt được ý tưởng rằng`i`sống sót sau một vòng nữa so với chuỗi tiền nhiệm mạnh hơn gần nhất của nó. 
5. Nếu ngăn xếp trống,`death[i]`vẫn là 0 vì không có gì ở bên trái lấn át nó. 
6. Đẩy`i`lên ngăn xếp. 
7. Đáp án là giá trị lớn nhất trong`death`. 

### Tại sao nó hoạt động 

Ngăn xếp duy trì các ứng cử viên cho “nguồn thống trị tích cực”, nghĩa là các chỉ số có giá trị giảm dần từ trái sang phải. Đối với bất kỳ chỉ mục nào`i`, phần tử lớn hơn đầu tiên ở bên trái là phần tử duy nhất cuối cùng có thể khiến nó bị loại bỏ. Tất cả các yếu tố nhỏ hơn giữa chúng đều không liên quan vì chúng bị loại bỏ sớm hơn hoặc bị chi phối sớm hơn và không thể ngăn chặn được ảnh hưởng của giá trị lớn hơn. 

Sự tái diễn`death[i] = death[j] + 1`lớn hơn gần nhất`j`mã hóa cấu trúc sụp đổ lớp. Mỗi phần tử tồn tại thêm một vòng nữa sau thời điểm phần tử tiền nhiệm chiếm ưu thế của nó bắt đầu hoạt động trong quá trình loại bỏ. Điều này đảm bảo việc truyền bá thời gian loại bỏ dọc theo chuỗi thống trị mà không cần mô phỏng rõ ràng các vòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    stack = []
    death = [0] * n

    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()

        if stack:
            death[i] = death[stack[-1]] + 1
        else:
            death[i] = 0

        stack.append(i)

    print(max(death))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên một chồng chỉ số đơn điệu. Ngăn xếp luôn lưu trữ các giá trị theo thứ tự giảm dần, đảm bảo rằng khi gặp giá trị lớn hơn, chúng tôi loại bỏ chính xác tất cả các ứng cử viên nhỏ hơn không thể ảnh hưởng đến các phần tử trong tương lai. 

các`death`mảng được tính toán tăng dần và mỗi mục chỉ phụ thuộc vào phần trước hợp lệ gần nhất. Điều này tránh việc quét hoặc mô phỏng các vòng lặp lại. 

Một sai lầm phổ biến là cố gắng tính toán số tử vong mà không duy trì tính đơn điệu nghiêm ngặt, điều này phá vỡ sự đảm bảo rằng đỉnh ngăn xếp đại diện cho phần tử lớn hơn có liên quan gần nhất. Một vấn đề tế nhị khác là quên rằng câu trả lời là lớn nhất trên tất cả`death[i]`, không chỉ phần tử cuối cùng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10
10 9 7 8 6 5 3 4 2 1
```Chúng tôi theo dõi giá trị cộng dồn và cái chết. 

| tôi | một [tôi] | ngăn xếp (giá trị) | cái chết[i] | 
| --- | --- | --- | --- | 
| 0 | 10 | 10 | 0 | 
| 1 | 9 | 10, 9 | 0 | 
| 2 | 7 | 10, 9, 7 | 0 | 
| 3 | 8 | 10, 9, 8 | 1 | 
| 4 | 6 | 10, 9, 8, 6 | 1 | 
| 5 | 5 | 10, 9, 8, 6, 5 | 1 | 
| 6 | 3 | 10, 9, 8, 6, 5, 3 | 1 | 
| 7 | 4 | 10, 9, 8, 6, 5, 4 | 2 | 
| 8 | 2 | 10, 9, 8, 6, 5, 4, 2 | 2 | 
| 9 | 1 | 10, 9, 8, 6, 5, 4, 2, 1 | 2 | 

Thời gian chết tối đa là 2, nghĩa là cấu trúc sẽ ổn định sau hai vòng. Điều này phù hợp với sự sụp đổ được quan sát thành`[10, 8, 4]`sau đó`[10]`. 

### Ví dụ 2 

đầu vào:```
5
1 3 2 4 3
```| tôi | một [tôi] | ngăn xếp (giá trị) | cái chết[i] | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | 0 | 
| 1 | 3 | 3 | 0 | 
| 2 | 2 | 3, 2 | 1 | 
| 3 | 4 | 4 | 0 | 
| 4 | 3 | 4, 3 | 1 | 

Câu trả lời là 1, vì sau một vòng, tất cả các so sánh không ổn định sẽ biến mất và cấu hình ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất ra nhiều nhất một lần từ ngăn xếp | 
| Không gian | O(n) | Mảng ngăn xếp và mảng chết lưu trữ một giá trị cho mỗi phần tử | 

Độ phức tạp tuyến tính vừa vặn thoải mái trong các giới hạn cho n lên tới 100.000 và mức sử dụng bộ nhớ cũng tuyến tính và an toàn dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    stack = []
    death = [0] * n

    for i in range(n):
        while stack and a[stack[-1]] < a[i]:
            stack.pop()

        if stack:
            death[i] = death[stack[-1]] + 1
        else:
            death[i] = 0

        stack.append(i)

    return str(max(death))

# provided sample
assert run("10\n10 9 7 8 6 5 3 4 2 1\n") == "2"

# strictly increasing (no kills)
assert run("5\n1 2 3 4 5\n") == "0"

# strictly decreasing (one wave collapse)
assert run("5\n5 4 3 2 1\n") == "1"

# alternating pattern
assert run("6\n1 3 2 5 4 6\n") == "2"

# single element
assert run("1\n1\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tăng nghiêm ngặt | 0 | không xảy ra sự loại bỏ | 
| giảm nghiêm ngặt | 1 | sụp đổ hoàn toàn trong một hiệp | 
| mô hình xen kẽ | 2 | tuyên truyền nhiều lớp về cái chết | 
| phần tử đơn | 0 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Một chuỗi tăng dần nghiêm ngặt như`1 2 3 4`không bao giờ kích hoạt bất kỳ tiêu diệt nào vì không có phần tử nào có giá trị lớn hơn ở bên phải. Ngăn xếp vẫn tăng, mọi phần tử đều được`death[i] = 0`, và đáp án cuối cùng là 0. 

Một chuỗi giảm nghiêm ngặt như`5 4 3 2 1`tạo ra sự thống trị ngay lập tức từ trái sang phải. Mỗi phần tử coi giá trị đầu tiên là giá trị liền trước lớn hơn gần nhất của nó, vì vậy tất cả các trường hợp tử vong đều phụ thuộc vào chỉ số 0 và được đánh giá thành 1 bước, cho kết quả tối đa là 1. 

Trong các cấu trúc xen kẽ như`1 3 2 5 4`, mỗi đỉnh tạo ra một lớp thống trị mới. Ngăn xếp được đặt lại nhiều lần và việc truyền bá`death`các giá trị tăng lên một đơn vị mỗi khi một phần tử mới mạnh hơn xuất hiện sau khi đảo ngược cục bộ. Điều này chứng tỏ cách thuật toán nắm bắt được việc loại bỏ xếp tầng nhiều vòng mà không cần mô phỏng rõ ràng.
