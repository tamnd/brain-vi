---
title: "CF 106500J - Giải thưởng giá trị"
description: "Ban tổ chức cuộc thi có thẻ quà tặng và muốn tặng đúng bảy phiếu quà tặng cho bảy thí sinh đứng đầu. Sau khi sắp xếp các lá bài được chọn từ giải nhất đến giải bảy, các giá trị phải thỏa mãn hai quy tắc công bằng."
date: "2026-06-25T08:37:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106500
codeforces_index: "J"
codeforces_contest_name: "XXVIII Interregional Programming Olympiad, Vologda SU, 2026"
rating: 0
weight: 106500
solve_time_s: 45
verified: true
draft: false
---

[CF 106500J - Giải thưởng có giá trị](https://codeforces.com/problemset/problem/106500/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ban tổ chức cuộc thi có thẻ quà tặng và muốn tặng đúng bảy phiếu quà tặng cho bảy thí sinh đứng đầu. Sau khi sắp xếp các lá bài được chọn từ giải nhất đến giải bảy, các giá trị phải thỏa mãn hai quy tắc công bằng. Giải nhất không thể quá lớn so với giải nhì và giải ba cộng lại. Ngoài ra, ba giải đầu cộng lại phải nhỏ hơn bốn giải cuối cộng lại. Trong số tất cả các lựa chọn công bằng có thể có, chúng ta cần tổng giá trị tối đa của bảy lá bài được chọn. 

Đầu vào cung cấp số lượng thẻ có sẵn và giá trị của chúng. Kết quả đầu ra là tổng lớn nhất có thể của một nhóm bảy thẻ hợp lệ, hoặc`-1`khi không có nhóm như vậy tồn tại. 

Số lượng thẻ có thể lên tới 500000. Điều này ngay lập tức loại trừ việc kiểm tra tất cả các nhóm bảy thẻ, vì điều đó sẽ yêu cầu khoảng$N^7$những lựa chọn vượt xa giới hạn thời gian bình thường cho phép. Thậm chí$O(N^2)$quá lớn so với kích thước đầu vào tối đa. Chúng ta cần một giải pháp gần với độ phức tạp sắp xếp, chẳng hạn như$O(N \log N)$hoặc$O(N)$sau khi sắp xếp. 

Phần khó khăn là bảy giá trị lớn nhất không phải lúc nào cũng là câu trả lời. Một giải nhất rất lớn có thể phá vỡ sự bất bình đẳng thứ nhất, và việc phân bổ bảy giá trị có ý nghĩa quan trọng. Ví dụ: với:```
7
1 2 3 4 5 6 7
```nhóm duy nhất có thể là tất cả các thẻ. Đầu ra là:```
-1
```Giải pháp bất cẩn chỉ kiểm tra tổng điểm của bảy lá bài lớn nhất sẽ trả về 28, nhưng giải 7 lớn nhất không nhỏ hơn 6 cộng 5 nên việc chia không hợp lệ. 

Một trường hợp khác xuất hiện khi có những lá bài cực lớn cần được bỏ qua. Vì:```
10
5 5 5 5 5 5 10 5 5 5
```câu trả lời là:```
35
```Một cách tiếp cận tham lam ngây thơ có thể bao gồm số 10 vì nó làm tăng tổng số tiền. Tuy nhiên, sau khi sắp xếp các giá trị đã chọn thành`5 5 5 5 5 5 10`, bất đẳng thức đầu tiên trở thành`10 < 5 + 5`, điều đó là sai. Giải pháp đúng sẽ để lại 10 không được sử dụng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng bộ bảy thẻ, sắp xếp bảy giá trị đó và kiểm tra hai điều kiện. Nó đúng vì mọi câu trả lời có thể đều được xem xét. Vấn đề là số lượng khả năng. Việc chọn bảy lá bài từ 500000 sẽ tạo ra một số lượng lớn các kết hợp, vì vậy phương pháp này không thể kết thúc. 

Quan sát quan trọng là một nhóm bảy hợp lệ có cấu trúc rất cụ thể. Sắp xếp tất cả các giá trị thẻ ngày càng tăng. Hãy xem xét bất kỳ lựa chọn hợp lệ nào:```
a0 <= a1 <= a2 <= a3 <= a4 <= a5 <= a6
```Hai giá trị trung gian`a4`Và`a5`tách biệt các điều kiện. Giá trị lớn nhất chỉ cần nhỏ hơn`a4 + a5`, trong khi bốn giá trị đầu tiên chỉ cần có tổng lớn hơn`a4 + a5`. 

Nếu một nhóm hợp lệ chưa có bảy phần tử liên tiếp trong mảng được sắp xếp, chúng ta có thể thay thế các phần tử đã chọn bằng một cửa sổ liên tiếp tốt hơn kết thúc ở cùng phần tử được chọn lớn nhất. Mọi giá trị được thay thế ít nhất cũng lớn bằng giá trị cũ và hai bất đẳng thức không trở nên khó thỏa mãn hơn vì phần giữa và phần dưới chỉ tăng lên. Việc lặp lại phép biến đổi này sẽ cho một câu trả lời tối ưu là khối bảy liên tiếp sau khi sắp xếp. 

Bây giờ vấn đề trở thành việc kiểm tra mọi cửa sổ có kích thước bảy. Vì kích thước cửa sổ là cố định nên mỗi lần kiểm tra đều có thời gian không đổi bằng cách sử dụng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^7) | O(7) | Quá chậm | 
| Tối ưu | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả giá trị thẻ theo thứ tự tăng dần. Câu trả lời tối ưu có thể được biểu diễn dưới dạng bảy giá trị liên tiếp sau khi sắp xếp, vì vậy việc sắp xếp cho phép chúng ta chỉ kiểm tra những ứng viên đó. 
2. Xây dựng tổng tiền tố của mảng đã sắp xếp. Chúng ta cần tính toán nhanh tổng của bất kỳ nhóm bốn hoặc bảy giá trị nào trong khi quét. 
3. Đối với mọi chỉ số bắt đầu`i`của cửa sổ có kích thước bảy, đặt các giá trị là:`a[i], a[i+1], ..., a[i+6]`. 

Điều kiện đầu tiên trở thành:`a[i+6] < a[i+4] + a[i+5]`. 

Giải lớn nhất được so sánh với hai giải phía dưới nên ba vị trí này được cố định bên trong cửa sổ. 
4. Kiểm tra điều kiện thứ hai:`a[i+4] + a[i+5] < a[i] + a[i+1] + a[i+2] + a[i+3]`. 

Bốn giải cuối cùng phải cao hơn ba giải đầu. Tổng tiền tố cho phép chúng ta tính tổng bốn phần tử ngay lập tức. 
5. Nếu cả hai điều kiện đều đúng, hãy cập nhật câu trả lời bằng tổng của bảy giá trị trong cửa sổ này. 
6. Nếu không có cửa sổ nào hoạt động, hãy xuất`-1`. 

Tại sao nó hoạt động: đối số chuyển đổi cho thấy rằng một lựa chọn hợp lệ tối ưu luôn có thể được chuyển sang một cửa sổ được sắp xếp liên tiếp mà không làm giảm tổng giá trị của nó hoặc phá vỡ các bất đẳng thức. Quá trình quét sẽ kiểm tra mọi dạng tối ưu có thể có, vì vậy cửa sổ hợp lệ tối đa được tìm thấy là cửa sổ tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    pref = [0] * (n + 1)
    for i, x in enumerate(a):
        pref[i + 1] = pref[i] + x

    ans = -1

    for i in range(n - 6):
        middle = a[i + 4] + a[i + 5]

        if a[i + 6] < middle:
            first_four = pref[i + 4] - pref[i]
            if middle < first_four:
                total = pref[i + 7] - pref[i]
                if total > ans:
                    ans = total

    print(ans)

if __name__ == "__main__":
    solve()
```Bước sắp xếp đặt các thẻ theo thứ tự cần thiết cho đối số cửa sổ. Sau khi sắp xếp, có thể kiểm tra phân phối hợp lệ mà không cần thử hoán vị vì thứ tự sắp xếp sẽ xác định duy nhất thứ hạng của thí sinh. 

Mảng tổng tiền tố lưu trữ tổng của mọi tiền tố của các thẻ đã được sắp xếp. biểu hiện`pref[i + 7] - pref[i]`đưa ra tổng của một cửa sổ bảy lá bài hoàn chỉnh, trong khi`pref[i + 4] - pref[i]`đưa ra bốn thẻ nhỏ nhất bên trong cửa sổ. 

Vòng lặp dừng ở`n - 7`bởi vì mỗi đoạn được kiểm tra phải chứa chính xác bảy phần tử. Các bất đẳng thức sử dụng các chỉ số liên quan đến cửa sổ, vì vậy cặp ở giữa luôn là vị trí`i + 4`Và`i + 5`. Không có vấn đề tràn tồn tại trong Python vì số nguyên có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
8
1 2 3 4 5 6 7 8
```mảng được sắp xếp đã có:```
1 2 3 4 5 6 7 8
```Quá trình quét kiểm tra: 

| Cửa sổ | Tổng cặp giữa | Bốn tổng đầu tiên | hợp lệ | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 2 3 4 5 6 7 | 11 | 10 | Không | 28 | 
| 2 3 4 5 6 7 8 | 13 | 14 | Có | 35 | 

Cửa sổ thứ hai hoạt động vì 8 nhỏ hơn 13 và 13 nhỏ hơn 14. Câu trả lời là 35. 

Đối với đầu vào:```
10
5 5 5 5 5 5 10 5 5 5
```mảng được sắp xếp là:```
5 5 5 5 5 5 5 5 5 10
```Các cửa sổ hoạt động như sau: 

| Bắt đầu cửa sổ | Giá trị lớn nhất | Tổng cặp giữa | Bốn tổng đầu tiên | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 10 | 20 | Có | 
| 1 | 5 | 10 | 20 | Có | 
| 2 | 5 | 10 | 20 | Có | 
| 3 | 5 | 10 | 20 | Có | 

Các cửa sổ chứa giá trị 10 không thành công vì giá trị lớn nhất quá lớn so với cặp ở giữa. Tổng hợp lệ tốt nhất là 35. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Việc sắp xếp chiếm ưu thế trong quá trình quét tuyến tính đơn | 
| Không gian | O(N) | Mảng tổng tiền tố lưu trữ N + 1 giá trị | 

Các ràng buộc cho phép sắp xếp 500000 giá trị một cách thoải mái. Sau khi sắp xếp, mỗi cửa sổ sẽ được kiểm tra một lần nên công việc còn lại là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    a.sort()

    pref = [0] * (n + 1)
    for i, x in enumerate(a):
        pref[i + 1] = pref[i] + x

    ans = -1
    for i in range(n - 6):
        mid = a[i + 4] + a[i + 5]
        if a[i + 6] < mid and mid < pref[i + 4] - pref[i]:
            ans = max(ans, pref[i + 7] - pref[i])

    return str(ans)

assert solution("7\n1 2 3 4 5 6 7\n") == "-1"
assert solution("8\n1 2 3 4 5 6 7 8\n") == "35"
assert solution("10\n5 5 5 5 5 5 10 5 5 5\n") == "35"

assert solution("7\n10 1 1 1 1 1 1\n") == "-1"
assert solution("7\n1 1 1 1 1 1 1\n") == "7"
assert solution("9\n1 2 3 4 5 6 7 8 100\n") == "35"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảy giá trị ngày càng tăng | -1 | Bảy lá bài lớn nhất có thể không tuân theo quy tắc công bằng | 
| Các giá trị liên tiếp từ 1 đến 8 | 35 | Lựa chọn cửa sổ hợp lệ cơ bản | 
| Nhiều giá trị bằng nhau với một giá trị lớn | 35 | Từ chối giải thưởng lớn hấp dẫn nhưng không hợp lệ | 
| Một quân bài thống trị | -1 | Xử lý bất bình đẳng đầu tiên | 
| Tất cả các giá trị bằng nhau | 7 | Trường hợp ranh giới có giá trị bằng nhau | 
| Ngoại lệ lớn sau các giá trị hợp lệ | 35 | Bỏ qua các giá trị cao không sử dụng được một cách chính xác | 

## Vỏ cạnh 

Đối với trường hợp:```
7
1 2 3 4 5 6 7
```thuật toán chỉ có một cửa sổ. Cặp ở giữa là 6 và 5, cho kết quả 11. Giá trị lớn nhất 7 thỏa mãn bất đẳng thức thứ nhất, nhưng bốn giá trị đầu tiên có tổng bằng 10, không lớn hơn 11. Thuật toán loại bỏ cửa sổ và in`-1`. 

Đối với trường hợp:```
10
5 5 5 5 5 5 10 5 5 5
```mảng được sắp xếp đặt số 10 ở cuối. Bất kỳ cửa sổ nào chứa nó đều có giá trị lớn nhất là 10 và tổng cặp giữa là 10, do đó bất đẳng thức nghiêm ngặt`10 < 10`thất bại. Các cửa sổ còn lại chỉ chứa 5 giây, tổng cộng hợp lệ là 35. 

Đối với trường hợp:```
7
1 1 1 1 1 1 1
```tất cả các giá trị đều bằng nhau. Giá trị lớn nhất là 1 và tổng của cặp ở giữa là 2. Bốn giá trị đầu tiên có tổng bằng 4, do đó cả hai bất đẳng thức đều đúng. Thuật toán trả về tổng đầy đủ, 7.
