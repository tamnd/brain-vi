---
title: "CF 104451B - \u041e\u0431\u0441\u043b\u0443\u0436\u0438\u0432\u0430\u043d\u0438\u0435"
description: "Chúng ta được cung cấp một mảng các số nguyên riêng biệt và chúng ta được phép chọn chính xác một đoạn liền kề và đảo ngược nó."
date: "2026-06-30T14:49:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104451
codeforces_index: "B"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2023"
rating: 0
weight: 104451
solve_time_s: 44
verified: true
draft: false
---

[CF 104451B - \u041e\u0431\u0441\u043b\u0443\u0436\u0438\u0432\u0430\u043d\u0438\u0435](https://codeforces.com/problemset/problem/104451/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên riêng biệt và chúng ta được phép chọn chính xác một đoạn liền kề và đảo ngược nó. Nhiệm vụ là xác định xem có tồn tại một số phân đoạn mà sự đảo ngược của nó biến toàn bộ mảng thành một chuỗi tăng dần hay không và nếu có thì xuất ra một phân đoạn như vậy. 

Đầu vào chỉ đơn giản là độ dài của mảng theo sau là chính mảng đó. Đầu ra là một xác nhận rằng có thể thực hiện được cùng với các ranh giới phân đoạn hoặc một tuyên bố rằng không có sự đảo ngược đơn lẻ nào có thể sắp xếp mảng. 

Hạn chế chính về cấu trúc là kích thước mảng có thể lên tới 100.000, do đó, bất kỳ giải pháp nào thử tất cả các phân đoạn có thể đều không khả thi ngay lập tức. Cách tiếp cận O(n³) hoặc thậm chí O(n²) ngây thơ sẽ hết thời gian và thậm chí mô phỏng đảo ngược O(n³) cũng quá chậm nếu được thực hiện nhiều lần. Điều này buộc chúng ta phải hướng tới lý luận O(n) hoặc O(n log n) dựa trên các đặc tính cấu trúc của các mảng gần như được sắp xếp. 

Trường hợp cạnh tinh tế phát sinh khi mảng đã được sắp xếp. Trong trường hợp này, việc đảo ngược đoạn có độ dài 1 là hợp lệ và phải được chấp nhận. Một trường hợp góc quan trọng khác là khi phần chưa được sắp xếp rất nhỏ, ví dụ như một phép đảo ngược đơn lẻ như`[2, 1, 3, 4]`, trong đó chỉ tồn tại một phân đoạn tương đương trao đổi. Cuối cùng, có những trường hợp mảng trông “gần như được sắp xếp” cục bộ nhưng không thể sửa được bằng một lần đảo ngược, chẳng hạn như`[3, 1, 2, 4]`, trong đó bất kỳ sự đảo ngược nào sẽ phá vỡ các phần khác của lệnh. 

## Phương pháp tiếp cận 

Chiến lược brute-force là thử mọi cặp chỉ số có thể`l, r`, đảo ngược phân đoạn đó và kiểm tra xem mảng có được sắp xếp hay không. Mỗi lần kiểm tra lấy O(n) và có các phân đoạn O(n2), cho kết quả tổng thể là O(n³). Ngay cả việc tối ưu hóa việc kiểm tra bằng tính toán trước cũng không loại bỏ được số ứng viên bậc hai, do đó quá trình này quá chậm đối với n = 100.000. 

Cái nhìn sâu sắc quan trọng là chuyển từ “thử tất cả các lần đảo chiều” sang “mô tả đặc điểm của một lần đảo chiều hợp lệ”. Nếu đảo ngược một phân đoạn sẽ sắp xếp mảng thì bên ngoài phân đoạn đó, mảng đó phải được sắp xếp theo đúng thứ tự. Bên trong phân đoạn, thứ tự phải được đảo ngược chính xác so với mảng được sắp xếp, nghĩa là phân đoạn phải xuất hiện giảm dần trong mảng ban đầu. 

Điều này dẫn đến một quan sát về cấu trúc: nếu một giải pháp tồn tại, mảng sẽ khác với phiên bản được sắp xếp của nó ở chính xác một khối liền kề và khối đó đang giảm dần. Bên ngoài khối đó, mảng phải khớp với thứ tự đã sắp xếp. 

Vì vậy, thay vì thử các thao tác, chúng tôi so sánh mảng với phiên bản đã sắp xếp của nó và xác định các ranh giới không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các phân đoạn) | O(n³) | O(1) | Quá chậm | 
| So sánh với xác thực được sắp xếp + phân đoạn đơn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một bản sao đã được sắp xếp của mảng. Điều này thể hiện cấu hình mục tiêu cuối cùng sau khi đảo ngược thành công. 
2. Tìm chỉ số đầu tiên`l`trong đó mảng ban đầu khác với mảng được sắp xếp. Đây là vị trí sớm nhất phải được đưa vào đoạn đảo ngược, vì mọi thứ trước nó đều đúng. 
3. Tìm chỉ số cuối cùng`r`trong đó mảng ban đầu khác với mảng được sắp xếp. Đây là ranh giới xa nhất của đoạn phải được đảo ngược, vì mọi thứ sau nó đều đúng. 
4. Đảo ngược mảng con từ`l`ĐẾN`r`trong một bản sao của mảng ban đầu. 
5. Kiểm tra xem mảng đã sửa đổi này có khớp chính xác với mảng đã sắp xếp hay không. Nếu đúng như vậy thì đoạn`[l, r]`là hợp lệ. Nếu không, không một sự đảo ngược nào có thể sửa được mảng. 

Lý do chúng ta có thể tập trung an toàn vào phân đoạn ứng viên duy nhất này là vì mọi giải pháp hợp lệ đều phải căn chỉnh chính xác với khoảng không khớp giữa mảng và phiên bản được sắp xếp của nó. Nếu tồn tại một sự đảo ngược hợp lệ nhưng khác với khoảng này, điều đó có nghĩa là các phần bên ngoài sự không khớp đang bị thay đổi, điều này sẽ ngay lập tức phá vỡ thứ tự được sắp xếp ở nơi khác. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào thực tế là việc sắp xếp thông qua một đảo ngược không thể đưa ra các phần tử mới được sắp xếp chính xác bên ngoài phân đoạn đảo ngược. Mọi vị trí bên ngoài phân khúc đã chọn vẫn cố định. Do đó, tất cả các vị trí đã khớp với mảng đã sắp xếp phải nằm ngoài phân đoạn và tất cả các vị trí không khớp phải nằm bên trong phân đoạn đó. Điều này buộc đoạn đảo ngược phải chính xác là khoảng liền kề bao gồm tất cả các phần không khớp. Khi khoảng thời gian này được xác định, chỉ có một lần đảo ngược ứng cử viên để kiểm tra và nó có hợp lệ hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    b = sorted(a)

    l = 0
    while l < n and a[l] == b[l]:
        l += 1

    if l == n:
        print("yes")
        print(1, 1)
        return

    r = n - 1
    while r >= 0 and a[r] == b[r]:
        r -= 1

    segment = a[:]
    segment[l:r+1] = reversed(segment[l:r+1])

    if segment == b:
        print("yes")
        print(l + 1, r + 1)
    else:
        print("no")

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ xây dựng mảng tham chiếu đã được sắp xếp, sau đó tách biệt các vị trí không khớp đầu tiên và cuối cùng. Các chỉ số đó xác định phân đoạn hợp lý duy nhất có thể sửa được mảng. Đảo ngược phân đoạn đó trong một bản sao là đủ vì mọi giải pháp hợp lệ đều phải trùng với cửa sổ không khớp này. 

Một lỗi phổ biến là cho rằng bất kỳ phân đoạn giảm nào đều hợp lệ mà không xác minh tính nhất quán toàn cầu. Việc kiểm tra sự bằng nhau cuối cùng đối với mảng đã được sắp xếp là cần thiết vì tính đơn điệu cục bộ không đảm bảo tính chính xác bên ngoài phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Mảng được sắp xếp là`[1, 2, 3]`. Sự không khớp đầu tiên là ở chỉ số 0 và sự không khớp cuối cùng là ở chỉ mục 2. 

| Bước | tôi | r | mảng sau khi đảo ngược | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 2 | 3 2 1 | 
| đảo ngược | 0 | 2 | 1 2 3 | 

Sau khi đảo ngược, mảng khớp với phiên bản đã sắp xếp, vì vậy câu trả lời là`yes 1 3`. 

Điều này xác nhận rằng toàn bộ mảng có thể được đảo ngược khi nó giảm dần. 

### Ví dụ 2 

đầu vào:```
4
3 1 2 4
```Mảng được sắp xếp là`[1 2 3 4]`. Khoảng không khớp là từ chỉ số 0 đến 2. 

| Bước | tôi | r | mảng sau khi đảo ngược | 
| --- | --- | --- | --- | 
| ban đầu | 0 | 2 | 3 1 2 4 | 
| đảo ngược | 0 | 2 | 2 1 3 4 | 

Kết quả không được sắp xếp nên không có sự đảo ngược đơn lẻ nào có tác dụng. 

Điều này chứng tỏ rằng ngay cả khi tất cả các điểm không khớp đều liền kề nhau, cấu trúc bên trong vẫn phải căn chỉnh hoàn hảo sau khi đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, tất cả các hoạt động khác đều tuyến tính | 
| Không gian | O(n) | Mảng bổ sung cho bản sao được sắp xếp | 

Điều này phù hợp thoải mái trong các ràng buộc cho n lên đến 100.000, vì việc sắp xếp và quét một lần đều hiệu quả ở quy mô này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf

    n = int(input())
    a = list(map(int, input().split()))
    b = sorted(a)

    l = 0
    while l < n and a[l] == b[l]:
        l += 1

    if l == n:
        return "yes\n1 1"

    r = n - 1
    while r >= 0 and a[r] == b[r]:
        r -= 1

    segment = a[:]
    segment[l:r+1] = reversed(segment[l:r+1])

    if segment == b:
        return f"yes\n{l+1} {r+1}"
    return "no"

# provided samples
assert run("3\n3 2 1\n") == "yes\n1 3"
assert run("4\n3 1 2 4\n") == "no"

# custom cases
assert run("2\n1 2\n") == "yes\n1 1"
assert run("4\n2 1 3 4\n") == "yes\n1 2"
assert run("5\n5 4 3 2 1\n") == "yes\n1 5"
assert run("5\n1 3 2 5 4\n") == "no"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mảng được sắp xếp | đảo ngược phần tử đơn | trường hợp cạnh đã được sắp xếp | 
| một lần trao đổi | vâng đoạn | trường hợp đảo ngược tối thiểu | 
| đảo ngược hoàn toàn | toàn bộ phân đoạn | mảng giảm tối đa | 
| hai nghịch đảo rời nhau | không | sửa chữa không liền kề không thể | 

## Vỏ cạnh 

Khi mảng đã được sắp xếp, khoảng không khớp sẽ trống và thuật toán coi nó như một trường hợp hợp lệ tầm thường. Trở về`[1, 1]`là đúng vì việc đảo ngược đoạn có độ dài 1 sẽ khiến mảng không thay đổi. 

Khi mảng giảm hoàn toàn, khoảng thời gian không khớp sẽ kéo dài toàn bộ mảng. Đảo ngược nó sẽ tạo ra một dãy tăng dần và việc kiểm tra mảng đã sắp xếp sẽ xác nhận tính đúng đắn. 

Khi sự không khớp tạo thành một khối liền kề nhưng cấu trúc bên trong không bị đảo ngược hoàn toàn, chẳng hạn như`[3, 1, 2, 4]`, thuật toán vẫn chọn khoảng thời gian chính xác nhưng không thực hiện được lần xác minh cuối cùng. Điều này ngăn chặn việc chấp nhận không chính xác các đảo ngược một phần không thể sắp xếp mảng trên toàn cầu.
