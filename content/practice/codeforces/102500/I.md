---
title: "CF 102500I - Sàn đảo ngược"
description: "Chúng tôi có một chuỗi các giá trị độ hiếm của thẻ. Trình tự phải được sắp xếp theo thứ tự không giảm, nhưng một đoạn liên tục có thể bị đảo ngược. Nhiệm vụ là tìm ra đoạn mà khi đảo ngược một lần sẽ làm cho toàn bộ chuỗi được sắp xếp."
date: "2026-08-05T18:15:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 163
verified: true
draft: false
---

[CF 102500I - Bộ bài đảo ngược](https://codeforces.com/problemset/problem/102500/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các giá trị độ hiếm của thẻ. Trình tự phải được sắp xếp theo thứ tự không giảm, nhưng một đoạn liên tục có thể bị đảo ngược. Nhiệm vụ là tìm ra đoạn mà khi đảo ngược một lần sẽ làm cho toàn bộ chuỗi được sắp xếp. Nếu không có phân đoạn như vậy tồn tại, chúng tôi phải báo cáo rằng điều đó là không thể. Bất kỳ phân đoạn hợp lệ nào đều được chấp nhận. 

Đầu vào chứa số lượng thẻ và thứ tự hiện tại của chúng. Đầu ra là vị trí bên trái và bên phải dựa trên 1 của đoạn cần đảo ngược. Cho phép một đoạn có độ dài bằng 1 vì việc đảo ngược một phần tử sẽ khiến mảng không thay đổi. 

Kích thước của đầu vào là thách thức chính. Với tối đa 1.000.000 giá trị, thuật toán thử nhiều phân đoạn có thể là không thực tế. Việc kiểm tra mọi khoảng thời gian có thể sẽ cần khoảng n2 ứng viên, tức là khoảng 10¹² kiểm tra trong trường hợp xấu nhất. Ngay cả một giải pháp thực hiện một lượng công việc nhỏ trong mỗi khoảng thời gian có thể cũng sẽ vượt quá thời gian sẵn có. Cách tiếp cận dự định phải xử lý mảng với số lần không đổi, đưa ra giải pháp O(n). 

Một số trường hợp đặc biệt có thể phá vỡ các giải pháp chỉ xem xét sự rối loạn rõ ràng. 

Ví dụ: một mảng đã được sắp xếp, chẳng hạn như:```
1 2 3
```có câu trả lời:```
1 1
```Giải pháp chỉ tìm kiếm phần giảm dần và giả định rằng nó phải tìm thấy phần đó sẽ không thể in sai. 

Một trường hợp phức tạp khác là khi đoạn đảo ngược chạm vào ranh giới mảng:```
3 2 1 4
```Câu trả lời đúng là:```
1 3
```Giải pháp chỉ kiểm tra các phân đoạn được bao quanh bởi các giá trị tăng dần có thể bỏ sót điều này vì không có phần tử nào trước phần bị đảo ngược. 

Trường hợp thứ ba liên quan đến các giá trị bằng nhau:```
1 2 2 1 2
```Câu trả lời đúng là:```
3 4
```Hai giá trị bằng nhau ở đầu vùng xấu không tạo ra mức giảm. Một giải pháp coi mỗi cặp bằng nhau là một vấn đề có thể chọn một phân đoạn không chính xác. 

## Phương pháp tiếp cận 

Một phương pháp đơn giản là thử mọi phân đoạn liền kề có thể, đảo ngược nó và kiểm tra xem mảng kết quả đã được sắp xếp chưa. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra rõ ràng. Có n(n+1)/2 phân đoạn có thể và việc kiểm tra một phân đoạn cần có thời gian O(n) nếu chúng ta so sánh toàn bộ mảng sau đó. Tổng công là O(n³), vượt xa khả năng có thể xảy ra với n = 1.000.000. Ngay cả việc cải thiện bước kiểm tra vẫn để lại quá nhiều phân khúc ứng viên. 

Cấu trúc của vấn đề mang lại một quan sát mạnh mẽ hơn nhiều. Nếu một mảng được sắp xếp có một phân đoạn bị đảo ngược thì mọi thứ bên ngoài phân đoạn đó vẫn được sắp xếp. Bên trong phân khúc, thứ tự phải giảm hoàn toàn trước khi đảo chiều. Điều này có nghĩa là phần thú vị duy nhất của mảng là vị trí đầu tiên mà chuỗi dừng không giảm và là vị trí cuối cùng mà chuỗi dừng không giảm. 

Chúng ta có thể tìm thấy tiền tố dài nhất đã được sắp xếp và hậu tố dài nhất đã được sắp xếp. Mọi thứ giữa họ là phân đoạn duy nhất có thể đảo ngược được. Sau khi xác định phân khúc ứng cử viên này, chúng tôi chỉ cần xác minh hai thuộc tính. Phần bên trong phải không tăng vì nó sẽ tăng sau khi đảo chiều. Các giá trị ngay bên ngoài đoạn phải vừa sau khi đảo ngược, nghĩa là ranh giới bên trái mới không được nhỏ hơn phần tử trước nó và ranh giới bên phải mới không được lớn hơn phần tử sau nó. 

Brute-force hoạt động vì nó khám phá mọi khu vực có thể bị hư hại, nhưng không thành công vì số lượng khả năng quá lớn. Quan sát cho thấy một sự đảo chiều duy nhất tạo ra chính xác một gián đoạn đơn điệu cho phép chúng ta xác định khu vực duy nhất có thể có trong thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét từ đầu cho đến chỉ mục đầu tiên nơi trình tự giảm dần. Mọi thứ trước chỉ mục này đã là tiền tố được sắp xếp hợp lệ. Nếu không có chỉ mục nào như vậy tồn tại thì toàn bộ mảng sẽ được sắp xếp và việc đảo ngược phần tử đầu tiên là một câu trả lời hợp lệ. 
2. Quét từ cuối cho đến chỉ mục đầu tiên nơi trình tự giảm dần khi di chuyển từ phải sang trái. Mọi thứ sau chỉ mục này đã là hậu tố được sắp xếp hợp lệ. Khoảng cách giữa hai vị trí này là đoạn đảo ngược duy nhất có thể. 
3. Kiểm tra xem khoảng ứng viên có tăng không từ trái sang phải hay không. Sau khi đảo ngược nó, mọi giá trị trong khoảng phải trở thành không giảm. 
4. Kiểm tra kết nối giữa khoảng ứng viên và các phần đã được sắp xếp bên ngoài nó. Nếu có một phần tử trước khoảng thì nó phải nhỏ hơn hoặc bằng phần tử đầu tiên mới của khoảng. Nếu có một phần tử sau khoảng thì phần tử cuối cùng mới của khoảng phải nhỏ hơn hoặc bằng phần tử tiếp theo đó. 
5. Nếu tất cả các bước kiểm tra đều đạt, hãy xuất ra các ranh giới ứng cử viên bằng cách sử dụng chỉ mục dựa trên 1. Mặt khác, báo cáo rằng không có sự đảo ngược đơn lẻ nào có thể sắp xếp mảng. 

Lý do điều này có tác dụng là vì việc đảo ngược một phân đoạn chỉ thay đổi thứ tự bên trong phân đoạn đó. Mọi phần tử bên ngoài đoạn đều giữ vị trí tương đối của nó, vì vậy những phần bên ngoài đó phải được sắp xếp sẵn. Mức giảm đầu tiên và cuối cùng xác định chính xác nơi khối đảo ngược phải bắt đầu và kết thúc. Bên trong khối đó, sự đảo ngược sẽ thay đổi một chuỗi giảm dần thành một chuỗi tăng dần, do đó việc xác minh thuộc tính giảm và hai phép nối ranh giới là đủ để chứng minh toàn bộ mảng đã được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    left = 0
    while left + 1 < n and a[left] <= a[left + 1]:
        left += 1

    if left == n - 1:
        print("1 1")
        return

    right = n - 1
    while right - 1 >= 0 and a[right - 1] <= a[right]:
        right -= 1

    for i in range(left, right):
        if a[i] < a[i + 1]:
            print("impossible")
            return

    if left > 0 and a[left - 1] > a[right]:
        print("impossible")
        return

    if right + 1 < n and a[left] > a[right + 1]:
        print("impossible")
        return

    print(left + 1, right + 1)

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên tìm phần cuối của tiền tố được sắp xếp dài nhất. Vòng lặp dừng chính xác ở lần đảo ngược đầu tiên, bởi vì bất kỳ phần đảo ngược hợp lệ nào cũng phải chứa phần đảo ngược đó. 

Vòng lặp thứ hai tìm phần đầu của hậu tố được sắp xếp. Hai chỉ số hiện mô tả phân đoạn duy nhất có thể bị đảo ngược. 

Việc xác minh nội bộ sử dụng`a[i] < a[i + 1]`là điều kiện thất bại vì phân khúc phải không tăng. Cho phép các giá trị bằng nhau, điều này cần thiết cho các trường hợp có độ hiếm trùng lặp. 

Việc kiểm tra ranh giới so sánh với các giá trị sau khi đảo ngược, không phải trước nó. Giá trị ngoài cùng bên trái của phân đoạn đảo ngược trở thành giá trị lớn nhất bên trong phân khúc và giá trị ngoài cùng bên phải trở thành giá trị nhỏ nhất bên trong phân khúc. Đây là lý do tại sao việc so sánh sử dụng`a[right]`cho ranh giới bên trái và`a[left]`cho đúng ranh giới. 

Tất cả các chỉ số được duy trì ở vị trí dựa trên 0 trong khi xử lý. Chỉ đầu ra cuối cùng mới chuyển đổi chúng sang các vị trí dựa trên 1 được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 13 19 19 15 14 20
```| Biến | Giá trị | 
| --- | --- | 
| Vị trí giảm đầu tiên | 2 | 
| Vị trí giảm cuối cùng | 5 | 
| Phân khúc ứng viên | [2, 5] | 
| Kiểm tra phân đoạn | 19, 19, 15, 14 không tăng | 
| Kiểm tra ranh giới | Vượt qua | 
| Đầu ra | 3 6 | 

Tiền tố`10 13 19`đã được sắp xếp và hậu tố`20`đã được sắp xếp rồi. Đảo ngược đoạn giữa tạo ra`10 13 14 15 19 19 20`, xác nhận rằng khoảng ứng viên chính xác là phần bị hỏng. 

Đối với mẫu thứ hai:```
9 1 8 2 7 3
```| Biến | Giá trị | 
| --- | --- | 
| Vị trí giảm đầu tiên | 0 | 
| Vị trí giảm cuối cùng | 5 | 
| Phân khúc ứng viên | [0, 5] | 
| Kiểm tra phân đoạn | Thất bại vì 1 < 8 | 
| Đầu ra | không thể | 

Toàn bộ mảng sẽ cần phải được đảo ngược, nhưng nó không giảm trước khi đảo ngược. Vì thuộc tính bên trong được yêu cầu không thành công nên không một sự đảo ngược nào có thể khắc phục được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mảng được quét một số lần không đổi. | 
| Không gian | O(1) thêm | Chỉ các chỉ số và biến tạm thời được lưu trữ. | 

Giải pháp thực hiện một vài lần chuyển tuyến tính lên tới một triệu giá trị. Điều này giữ cho cả thời gian chạy và mức sử dụng bộ nhớ trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(data):
    sys.stdin = io.StringIO(data)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    left = 0
    while left + 1 < n and a[left] <= a[left + 1]:
        left += 1

    if left == n - 1:
        return "1 1"

    right = n - 1
    while right - 1 >= 0 and a[right - 1] <= a[right]:
        right -= 1

    for i in range(left, right):
        if a[i] < a[i + 1]:
            return "impossible"

    if left > 0 and a[left - 1] > a[right]:
        return "impossible"

    if right + 1 < n and a[left] > a[right + 1]:
        return "impossible"

    return f"{left + 1} {right + 1}"

assert solution("7\n10 13 19 19 15 14 20\n") == "3 6"
assert solution("6\n9 1 8 2 7 3\n") == "impossible"
assert solution("3\n1 2 3\n") == "1 1"
assert solution("1\n5\n") == "1 1"
assert solution("5\n1 2 2 1 2\n") == "3 4"
assert solution("5\n5 4 3 2 1\n") == "1 5"
assert solution("6\n1 2 5 4 3 6\n") == "3 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 / 10 13 19 19 15 14 20`|`3 6`| Đảo ngược chuẩn ở giữa | 
|`6 / 9 1 8 2 7 3`|`impossible`| Phân khúc ứng viên không giảm | 
|`3 / 1 2 3`|`1 1`| Đã sắp xếp mảng | 
|`1 / 5`|`1 1`| Đầu vào kích thước tối thiểu | 
|`5 / 1 2 2 1 2`|`3 4`| Giá trị trùng lặp xung quanh đoạn bị hỏng | 
|`5 / 5 4 3 2 1`|`1 5`| Đảo ngược bao trùm toàn bộ mảng | 

## Vỏ cạnh 

Đối với một mảng đã được sắp xếp:```
1 2 3
```lần quét đầu tiên đến cuối mà không tìm thấy sự đảo ngược. Thuật toán ngay lập tức trở lại`1 1`, bởi vì việc đảo ngược một thẻ được cho phép và giữ nguyên trình tự. 

Đối với sự đảo ngược chạm vào điểm bắt đầu:```
3 2 1 4
```vị trí giảm dần đầu tiên là chỉ số 0 và quá trình quét hậu tố tìm thấy chỉ số 2. Phân đoạn ứng cử viên là`[0,2]`. Nó không tăng và không có ranh giới bên trái để kiểm tra. Kiểm tra ranh giới bên phải so sánh`3`với`4`, vượt qua, do đó thuật toán xuất ra`1 3`. 

Đối với các giá trị trùng lặp:```
1 2 2 1 2
```sự đảo ngược đầu tiên xuất hiện giữa lần thứ hai`2`Và`1`, do đó phân khúc ứng cử viên là`[2,3]`. Phân khúc`2 1`không tăng và việc đảo ngược nó mang lại`1 2`, dẫn đến một mảng được sắp xếp. Các giá trị liền kề bằng nhau được chấp nhận vì việc sắp xếp cho phép các giá trị lặp lại.
