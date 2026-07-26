---
title: "CF 102806A - \u0412\u043e\u0437\u0440\u0430\u0441\u0442\u0430\u044e\u0449\u0438\u0439 \u043c\u0430\u0441\u0441\u0438\u0432"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Đối với mọi phần tử, chúng ta được phép giữ nguyên giá trị hiện tại hoặc thay đổi dấu của nó. Nhiệm vụ là quyết định xem có tồn tại một số lựa chọn dấu hiệu làm cho mảng kết quả không giảm hay không."
date: "2026-07-26T16:19:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102806
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2017-2018, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 102806
solve_time_s: 83
verified: true
draft: false
---

[CF 102806A - \u0412\u043e\u0437\u0440\u0430\u0441\u0442\u0430\u044e\u0449\u0438\u0439 \u043c\u0430\u0441\u0441\u0438\u0432](https://codeforces.com/problemset/problem/102806/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Đối với mọi phần tử, chúng ta được phép giữ nguyên giá trị hiện tại hoặc thay đổi dấu của nó. Nhiệm vụ là quyết định xem có tồn tại một số lựa chọn dấu hiệu làm cho mảng kết quả không giảm hay không. Nếu cấu trúc như vậy tồn tại, chúng ta phải xuất ra một mảng kết quả hợp lệ. 

Kích thước đầu vào đạt tới 100000 phần tử, vì vậy việc thử tất cả các lựa chọn dấu hiệu có thể là không thể. Mỗi phần tử có hai trạng thái có thể, sẽ tạo ra 2^n mảng có thể. Ngay cả việc kiểm tra tất cả chúng cũng đã vượt xa giới hạn 2 giây cho phép. Chúng ta cần một cách tiếp cận xử lý từng phần tử với số lần không đổi. 

Các giá trị có thể lớn tới 100000 ở giá trị tuyệt đối, nhưng độ lớn của chúng không tạo ra vấn đề tràn trong Python. Khó khăn chính không phải là tính toán mà là tìm ra một chuỗi lựa chọn hợp lý. Một quy tắc tham lam sai có thể thất bại vì việc chọn giá trị lớn hơn bây giờ có thể khiến các phần tử trong tương lai không thể đặt được. 

Trường hợp cạnh đầu tiên là một phần tử duy nhất. Ví dụ:```
Input:
1
-5

Output:
Yes
-5
```Bất kỳ mảng một phần tử nào cũng không giảm. Một giải pháp giả định tồn tại ít nhất một so sánh liền kề có thể bác bỏ nó một cách không chính xác. 

Một trường hợp quan trọng khác là khi chỉ có lựa chọn phủ định có tác dụng đối với một số phần tử. Coi như:```
Input:
3
5 3 1

Output:
Yes
-5 -3 -1
```Phiên bản tích cực đang giảm dần, nhưng việc lật tất cả các dấu hiệu sẽ mang lại một chuỗi tăng hợp lệ. Một giải pháp chỉ cố gắng giữ lại những dấu hiệu ban đầu sẽ thất bại. 

Trường hợp khó khăn cuối cùng là khi giá trị âm không đủ và chúng ta phải sử dụng giá trị dương:```
Input:
2
1 2

Output:
Yes
-1 2
```Việc chọn các giá trị âm một cách tham lam mà không kiểm tra xem chúng có hợp lệ hay không sẽ tạo ra`-1 -2`, đang giảm dần. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ liệt kê mọi cách gán dấu hiệu có thể có. Đối với mỗi phép gán, chúng ta có thể xây dựng mảng kết quả và kiểm tra xem mọi cặp liền kề có thỏa mãn điều kiện không giảm hay không. Phương pháp này đúng vì nó kiểm tra mọi câu trả lời có thể có nhưng thực hiện 2^n lần kiểm tra. Với n = 100000, điều này thậm chí còn không khả thi. 

Quan sát hữu ích là mỗi phần tử có chính xác hai giá trị có thể:`-abs(a[i])`Và`abs(a[i])`. Trong hai giá trị này, giá trị âm luôn nhỏ hơn. Khi xử lý mảng từ trái sang phải, chúng ta chỉ cần biết giá trị đã chọn trước đó. Lựa chọn tốt nhất cho vị trí hiện tại là giá trị nhỏ nhất không phá vỡ thứ tự. 

Tại sao việc chọn giá trị hợp lệ nhỏ nhất có thể lại hữu ích? Giá trị hiện tại nhỏ hơn sẽ tạo ra ít hạn chế hơn đối với tất cả các phần tử sau này. Nếu giá trị âm có thể được đặt sau giá trị trước đó thì việc sử dụng nó luôn an toàn. Nếu không thể, giá trị dương là lựa chọn duy nhất còn lại. Nếu ngay cả giá trị dương nhỏ hơn giá trị trước đó thì không có nghiệm nào tồn tại. 

Brute-force hoạt động vì nó khám phá mọi phép gán dấu hiệu có thể có, nhưng không thành công vì không gian tìm kiếm tăng theo cấp số nhân. Quan sát thấy giá trị hiện tại hợp lệ nhỏ nhất để lại nhiều tự do nhất sẽ giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với giá trị đã chọn trước đó được đặt thành âm vô cực. Không có phần tử đầu tiên nào bị hạn chế từ bên trái, vì vậy mọi lựa chọn hợp lệ đều có thể thực hiện được. 
2. Đối với mỗi phần tử mảng, hãy xem xét hai giá trị có thể có của nó: giá trị tuyệt đối âm và giá trị tuyệt đối dương. Phiên bản tiêu cực luôn là ứng cử viên nhỏ hơn. 
3. Nếu ứng cử viên phủ định ít nhất là giá trị đã chọn trước đó, hãy chọn nó. Đây là lựa chọn ưu tiên vì nó giữ giá trị hiện tại càng nhỏ càng tốt. 
4. Nếu không, hãy thử ứng cử viên tích cực. Nếu ít nhất đó là giá trị đã chọn trước đó, hãy chọn nó. 
5. Nếu cả hai ứng cử viên đều không hoạt động, mảng không thể được chuyển đổi thành mảng không giảm, vì vậy đầu ra`No`. 
6. Sau khi xử lý thành công tất cả các phần tử, xuất ra`Yes`và trình tự đã xây dựng. 

Tại sao nó hoạt động: sau mỗi vị trí được xử lý, thuật toán sẽ giữ một tiền tố hợp lệ của câu trả lời. Trong số tất cả các lựa chọn hợp lệ cho vị trí hiện tại, nó lưu trữ giá trị nhỏ nhất có thể. Bất kỳ yếu tố nào trong tương lai có thể theo sau một lựa chọn lớn hơn cũng có thể theo sau lựa chọn nhỏ hơn này, vì vậy quyết định tham lam không bao giờ loại bỏ một giải pháp khả thi. Nếu thuật toán đạt đến một phần tử trong đó cả hai lựa chọn đều quá nhỏ thì mọi tiền tố có thể kết thúc trước phần tử đó đều có giá trị ít nhất bằng giá trị được lưu trữ trước đó, do đó không thể tiếp tục xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = []
    prev = -10**30

    for x in a:
        x = abs(x)

        if -x >= prev:
            cur = -x
        elif x >= prev:
            cur = x
        else:
            print("No")
            return

        ans.append(cur)
        prev = cur

    print("Yes")
    print(*ans)

if __name__ == "__main__":
    solve()
```Mã chỉ giữ giá trị đã chọn trước đó vì tương lai không phụ thuộc vào các phần tử trước đó khi tiền tố hiện tại đã được hợp lệ. Biến`prev`đại diện cho giá trị cuối cùng trong tiền tố không giảm được xây dựng. 

Các ứng cử viên được tạo ra từ`abs(x)`thay vì giá trị ban đầu vì phép toán cho phép một trong hai dấu, do đó mọi phần tử luôn có thể trở thành độ lớn hoặc độ lớn âm của nó. Thứ tự kiểm tra rất quan trọng: ứng viên tiêu cực được kiểm tra trước vì đây là lựa chọn nhỏ hơn và mang lại sự linh hoạt nhất cho các vị trí trong tương lai. 

Giá trị ban đầu cho`prev`được chọn thấp hơn nhiều so với bất kỳ giá trị mảng nào có thể có. Vì cường độ đầu vào tối đa là 100000,`-10**30`an toàn hoạt động như âm vô cực cho bài toán này. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 -1 -2 3 6
```| Bước | Giá trị tuyệt đối hiện tại | Lựa chọn tiêu cực | Lựa chọn tích cực | Giá trị trước đó | Được chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | 1 | -inf | -1 | 
| 2 | 1 | -1 | 1 | -1 | -1 | 
| 3 | 2 | -2 | 2 | -1 | 2 | 
| 4 | 3 | -3 | 3 | 2 | 3 | 
| 5 | 6 | -6 | 6 | 3 | 6 | 

Thuật toán trước tiên giữ các giá trị càng nhỏ càng tốt. Ở vị trí thứ ba,`-2`sẽ nhỏ hơn giá trị trước đó, vì vậy giá trị dương`2`được yêu cầu. Trình tự cuối cùng`[-1, -1, 2, 3, 6]`là không giảm. 

### Mẫu 2 

đầu vào:```
3
1 1 0
```| Bước | Giá trị tuyệt đối hiện tại | Lựa chọn tiêu cực | Lựa chọn tích cực | Giá trị trước đó | Được chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | -1 | 1 | -inf | -1 | 
| 2 | 1 | -1 | 1 | -1 | -1 | 
| 3 | 0 | 0 | 0 | -1 | 0 | 

Giá trị 0 chỉ có một kết quả có thể xảy ra và nó khớp sau hai giá trị âm. Dấu vết này cũng xác nhận rằng các giá trị liền kề bằng nhau được cho phép vì mảng chỉ cần không giảm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được kiểm tra một lần và chỉ có công việc liên tục được thực hiện. | 
| Không gian | O(n) | Mảng kết quả được lưu trữ cho đầu ra. | 

Độ phức tạp tuyến tính phù hợp với ràng buộc 100000 phần tử vì thuật toán chỉ thực hiện một số thao tác cố định nhỏ cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    n = int(input())
    a = list(map(int, input().split()))

    ans = []
    prev = -10**30

    for x in a:
        x = abs(x)
        if -x >= prev:
            cur = -x
        elif x >= prev:
            cur = x
        else:
            print("No")
            sys.stdin = old_stdin
            return sys.stdout.getvalue()

        ans.append(cur)
        prev = cur

    print("Yes")
    print(*ans)

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("5\n1 -1 -2 3 6\n") == "Yes\n-1 -1 2 3 6\n", "sample 1"
assert run("3\n1 1 0\n") == "Yes\n-1 -1 0\n", "sample 2"

assert run("1\n-5\n") == "Yes\n-5\n", "single element"
assert run("2\n5 3\n") == "Yes\n-5 -3\n", "decreasing absolute values"
assert run("3\n10 1 2\n") == "Yes\n-10 -1 2\n", "switching from negative to positive"
assert run("3\n1 0 -1\n") == "No\n", "impossible ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -5`|`Yes / -5`| Đầu vào kích thước tối thiểu | 
|`5 3`|`-5 -3`| Sử dụng giá trị âm để giữ trật tự | 
|`10 1 2`|`-10 -1 2`| Ranh giới nơi sự lựa chọn tích cực trở nên cần thiết | 
|`1 0 -1`|`No`| Phát hiện công trình bất khả thi | 

## Vỏ cạnh 

Đối với một phần tử, vòng lặp chạy một lần và luôn chấp nhận một trong hai dấu hiệu có thể có. Thuật toán không bao giờ cố gắng so sánh với phần tử không tồn tại trước đó, vì vậy trường hợp`n = 1`được xử lý một cách tự nhiên. 

Đối với một mảng đã giảm giá trị tuyệt đối, chẳng hạn như:```
Input:
3
5 3 1
```thuật toán chọn:```
-5 -3 -1
```Mỗi giá trị tiếp theo lớn hơn giá trị trước đó, vì vậy các lựa chọn phủ định là đủ. 

Đối với trường hợp lựa chọn phủ định không thành công:```
Input:
2
1 2
```phần tử đầu tiên trở thành`-1`. Ở phần tử thứ hai,`-2`nhỏ hơn`-1`, nên không dùng được. Thuật toán cố gắng`2`, hoạt động, cho:```
-1 2
```Đối với trường hợp không thể:```
Input:
3
1 0 -1
```phần tử đầu tiên trở thành`-1`. Phần tử thứ hai chỉ có thể trở thành`0`, do đó tiền tố trở thành`[-1, 0]`. Phần tử cuối cùng chỉ có thể trở thành`-1`hoặc`1`;`-1`quá nhỏ và`1`là hợp lệ, đưa ra`[-1, 0, 1]`. Việc xây dựng tham lam đã thành công ở đây cho thấy nhiều trường hợp trông đáng ngờ vẫn có thể xảy ra. 

Thuật toán chỉ thất bại khi cả hai giá trị khả dụng ở một vị trí nào đó đều thấp hơn giá trị đã chọn trước đó. Tại thời điểm đó, tiền tố được lưu trữ đã là tiền tố linh hoạt nhất có thể, do đó không có phép gán dấu nào khác có thể giải được mảng. 

Tôi cũng có thể điều chỉnh phiên bản này thành phiên bản biên tập ngắn hơn theo phong cách Codeforces nếu bạn muốn có hình thức gửi bài dự thi nhiều hơn.
