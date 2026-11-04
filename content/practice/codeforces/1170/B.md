---
title: "CF 1170B - Những Ngày Xấu"
description: "Chúng tôi được cung cấp một chuỗi số lượt truy cập trang web hàng ngày. Mỗi vị trí đại diện cho một ngày và giá trị tại vị trí đó thể hiện số lượt truy cập đã xảy ra vào ngày đó. Chúng ta cần kiểm tra mỗi ngày và quyết định xem nó có “xấu” hay không."
date: "2026-06-12T02:01:58+07:00"
tags: ["codeforces", "competitive-programming", "*special", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1170
codeforces_index: "B"
codeforces_contest_name: "Kotlin Heroes: Episode 1"
rating: 0
weight: 1170
solve_time_s: 192
verified: true
draft: false
---

[CF 1170B - Những ngày tồi tệ](https://codeforces.com/problemset/problem/1170/B) 

**Đánh giá:** - 
**Tags:** *đặc biệt, thực hiện 
**Thời gian giải:** 3 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi số lượt truy cập trang web hàng ngày. Mỗi vị trí đại diện cho một ngày và giá trị tại vị trí đó thể hiện số lượt truy cập đã xảy ra vào ngày đó. Chúng ta cần kiểm tra mỗi ngày và quyết định xem nó có “xấu” hay không. 

Một ngày trở nên tồi tệ nếu, khi chỉ nhìn vào những ngày trước đó, có ít nhất hai ngày trước đó có số lượt truy cập lớn hơn giá trị của ngày đó. Những ngày trước đó không cần phải liền kề nhau, chỉ có giá trị của chúng mới quan trọng. 

Nhiệm vụ là đếm xem có bao nhiêu chỉ số thỏa mãn điều kiện này. 

Hạn chế về số ngày lên tới 200.000. Điều đó ngay lập tức loại trừ bất kỳ giải pháp nào so sánh mỗi ngày với tất cả các ngày trước đó trong một vòng lặp lồng nhau, vì điều đó sẽ yêu cầu theo thứ tự so sánh n2, quá lớn so với giới hạn 3 giây. Cần phải quét tuyến tính hoặc gần tuyến tính, có thể với một lượng công việc bổ sung cố định nhỏ cho mỗi phần tử. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất phát từ việc quên rằng chúng ta chỉ quan tâm đến việc có bao nhiêu giá trị trước đó lớn hơn chứ không phải vị trí của chúng. Ví dụ: trong một chuỗi như [10, 1, 9, 8], phần tử thứ hai nhìn thấy hai giá trị lớn hơn trước nó, trong khi các phần tử sau có thể có ít giá trị hơn mặc dù mảng không đơn điệu. Một trường hợp cạnh khác là các giá trị lặp lại. Vì điều kiện hoàn toàn lớn hơn nên không được tính các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Phương pháp tiếp cận trực tiếp sẽ kiểm tra độc lập mỗi ngày. Đối với chỉ mục i cố định, chúng tôi quét tất cả j < i và đếm có bao nhiêu a[j] > a[i]. Nếu số lượng này ít nhất là hai, chúng tôi đánh dấu ngày đó là ngày tồi tệ. Điều này đúng vì nó tuân theo đúng định nghĩa. Tuy nhiên, mỗi vị trí có thể yêu cầu quét tối đa i-1 phần tử và trên tất cả i, điều này trở thành khoảng n(n-1)/2 so sánh. Với n = 200.000 thì điều này không thể thực hiện được. 

Quan sát quan trọng là trong mỗi ngày, chúng ta chỉ quan tâm đến việc liệu ngày đó có ít nhất hai giá trị lớn hơn trước đó hay không. Đây là một điều kiện ngưỡng, không phải là con số chính xác vượt quá hai. Điều đó có nghĩa là chúng tôi không cần thông tin đầy đủ về tất cả các yếu tố trước đó, chỉ liệu chúng tôi có thể xác định được hai giá trị lớn nhất được thấy cho đến nay trong số những ngày trước đó hay không. 

Một khi chúng ta nhận ra điều này, cấu trúc sẽ trở nên đơn giản. Khi lặp lại từ trái sang phải, chúng tôi duy trì các giá trị lớn nhất và lớn thứ hai cho đến nay. Đối với mỗi ngày mới, chúng tôi so sánh giá trị của nó với hai cực đại được theo dõi này. Nếu cả hai giá trị cực đại được lưu trữ đều lớn hơn giá trị hiện tại thì phải có ít nhất hai ngày trước đó vượt quá giá trị đó, điều đó thật tệ. 

Điều này có hiệu quả vì bất kỳ phần tử nào lớn hơn giá trị hiện tại đều phải nằm trong số các giá trị hàng đầu toàn cầu được thấy cho đến nay. Nếu có ít nhất hai phần tử như vậy, chúng nhất thiết sẽ chiếm vị trí tối đa thứ nhất và thứ hai trong tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Duy trì hai cực đại | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai biến,`first`Và`second`, để theo dõi các giá trị lớn nhất và lớn thứ hai được thấy cho đến nay. Chúng tôi đặt chúng ở mức vô cực âm để mọi giá trị đầu vào sẽ thay thế chúng một cách chính xác. 
2. Khởi tạo bộ đếm`bad = 0`để tích lũy số ngày tồi tệ. 
3. Lặp lại mảng từ trái sang phải. 
4. Đối với giá trị hiện tại`x`, kiểm tra xem cả hai`first > x`Và`second > x`giữ. Nếu đúng như vậy thì ít nhất hai ngày trước đó có giá trị lớn hơn rất nhiều, vì vậy chúng tôi tăng dần`bad`. 
5. Sau khi xử lý điều kiện, hãy cập nhật giá trị cực đại được theo dõi bằng`x`. Nếu như`x >= first`, sự thay đổi`first`vào trong`second`và chỉ định`first = x`. Bằng không, nếu`x >= second`, cập nhật`second = x`. 

Thứ tự cập nhật rất quan trọng vì chúng ta phải đánh giá điều kiện chỉ bằng cách sử dụng các phần tử trước đó. Cập nhật sau khi kiểm tra đảm bảo ngày hiện tại không được tính vào lịch sử của chính ngày đó. 

### Tại sao nó hoạt động 

Ở mọi vị trí,`first`Và`second`lưu trữ các giá trị lớn nhất và lớn thứ hai từ tất cả các chỉ số trước đó. Vì vậy, điều kiện`first > x and second > x`tương đương với việc nói rằng ít nhất hai phần tử trước đó vượt quá`x`. Không có giá trị nào khác có thể vi phạm sự tương đương này vì bất kỳ giá trị nào lớn hơn`x`phải nằm trong số cực đại tiền tố và nếu có ít nhất hai giá trị như vậy thì chúng phải chiếm hai vị trí được theo dõi này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    first = float("-inf")
    second = float("-inf")
    
    bad = 0
    
    for x in a:
        if first > x and second > x:
            bad += 1
        
        if x >= first:
            second = first
            first = x
        elif x >= second:
            second = x
    
    print(bad)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý mảng trong một lần duy nhất. Việc kiểm tra điều kiện chỉ sử dụng thông tin tiền tố được lưu trữ trong`first`Và`second`, đảm bảo tính đúng đắn Bước cập nhật cẩn thận duy trì thứ tự để các ràng buộc được xử lý chính xác: sử dụng`>=`đảm bảo các bản sao được theo dõi theo cách duy trì khả năng đếm các so sánh lớn hơn một cách chính xác trong các bước sau. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng mẫu [3, 1, 4, 1, 5, 9, 2, 6]. 

Chúng tôi theo dõi`first`,`second`, và liệu ngày hiện tại có xấu không. 

| tôi | x | đầu tiên | thứ hai | xấu? | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | -inf | không | 
| 2 | 1 | 3 | 1 | không | 
| 3 | 4 | 4 | 3 | không | 
| 4 | 1 | 4 | 3 | vâng | 
| 5 | 5 | 5 | 4 | không | 
| 6 | 9 | 9 | 5 | không | 
| 7 | 2 | 9 | 5 | vâng | 
| 8 | 6 | 9 | 6 | không | 

Bảng cho thấy ngày 4 và 7 được tính là ngày xấu, phù hợp với yêu cầu rằng mỗi ngày phải có ít nhất hai giá trị lớn hơn trước đó. 

Bây giờ hãy xem xét một ví dụ về cạnh nhỏ [10, 8, 7, 9, 6]. 

| tôi | x | đầu tiên | thứ hai | xấu? | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | -inf | không | 
| 2 | 8 | 10 | 8 | không | 
| 3 | 7 | 10 | 8 | vâng | 
| 4 | 9 | 10 | 9 | vâng | 
| 5 | 6 | 10 | 9 | vâng | 

Điều này chứng tỏ rằng một khi hai giá trị lớn tồn tại trong tiền tố thì nhiều giá trị nhỏ sau này có thể trở thành giá trị không hợp lệ và thuật toán sẽ phát hiện một cách nhất quán giá trị đó mà không cần quét lịch sử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần với các bản cập nhật và kiểm tra liên tục | 
| Không gian | O(1) | Chỉ có hai biến và một bộ đếm được duy trì | 

Quá trình quét tuyến tính xử lý thoải mái n lên tới 200.000 trong giới hạn thời gian vì mỗi lần lặp chỉ thực hiện một số phép so sánh và phép gán. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf
    
    data = inp.strip().split()
    n = int(data[0])
    a = list(map(int, data[1:]))

    first = float("-inf")
    second = float("-inf")
    bad = 0

    for x in a:
        if first > x and second > x:
            bad += 1
        
        if x >= first:
            second = first
            first = x
        elif x >= second:
            second = x

    return str(bad)

# provided sample
assert run("8\n3 1 4 1 5 9 2 6") == "2"

# minimum size
assert run("1\n5") == "0"

# all equal
assert run("5\n7 7 7 7 7") == "0"

# strictly decreasing
assert run("5\n9 8 7 6 5") == "3"

# increasing sequence
assert run("5\n1 2 3 4 5") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | ranh giới tối thiểu | 
| tất cả đều bình đẳng | 0 | điều kiện nghiêm ngặt hơn | 
| giảm dần | 3 | tích lũy nhiều giá trị lớn ban đầu | 
| ngày càng tăng | 0 | không có phần tử nào có hai phần tử liền trước lớn hơn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi mảng giảm nghiêm ngặt, ví dụ [9, 7, 5, 3, 1]. Thuật toán cập nhật`first`Và`second`sao cho sau hai phần tử đầu tiên, cả hai phần tử đều lớn. Bắt đầu từ phần tử thứ ba, mọi giá trị mới đều nhỏ hơn cả giá trị cực đại được lưu trữ, do đó mọi vị trí tiếp theo đều bị tính là xấu. Điều này phù hợp với định nghĩa vì mọi phần tử như vậy đều có ít nhất hai giá trị lớn hơn trước đó. 

Một trường hợp cạnh khác là các giá trị lặp lại như [5, 5, 5, 5]. Ở đây, mặc dù có nhiều phần tử trước đó nhưng không có phần tử nào lớn hơn bất kỳ phần tử đã cho nào. điều kiện`first > x and second > x`không bao giờ trở thành sự thật bởi vì`first`Và`second`không bao giờ lớn hơn giá trị hiện tại một khi đẳng thức chiếm ưu thế. Điều này mang lại chính xác số ngày tồi tệ.
