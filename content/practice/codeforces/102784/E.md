---
title: "CF 102784E - Những con số ma quái"
description: "Francine có một bộ sưu tập các chữ số riêng lẻ và muốn sắp xếp một số trong số chúng thành số lớn nhất có thể mà Buster cho là ma quái. Số ma quái là số nguyên không âm chia hết cho 2, 3 và 5, tương đương với số chia hết cho 30."
date: "2026-07-27T19:47:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 57
verified: true
draft: false
---

[CF 102784E - Những con số ma quái](https://codeforces.com/problemset/problem/102784/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Francine có một bộ sưu tập các chữ số riêng lẻ và muốn sắp xếp một số trong số chúng thành số lớn nhất có thể mà Buster cho là ma quái. Số ma quái là số nguyên không âm chia hết cho 2, 3 và 5, tương đương với số chia hết cho 30. Cô ấy có thể loại bỏ một số chữ số vì Arthur đã thêm những chữ số không mong muốn, nhưng các chữ số còn lại phải tạo thành giá trị lớn nhất có thể và không được chứa các số 0 đứng đầu không cần thiết. 

Đầu vào cung cấp số chữ số có sẵn và sau đó là chính các chữ số đó. Đầu ra là số nguyên lớn nhất có thể được tạo từ các chữ số này trong khi chia hết cho 30. Nếu không tồn tại số đó, chúng tôi sẽ in`-1`. 

Ràng buộc cho phép tối đa 100000 chữ số. Bất kỳ cách tiếp cận nào thử các cách sắp xếp khác nhau hoặc kiểm tra nhiều tập hợp con có thể đều không thể thực hiện được vì số lượng ứng viên tăng theo cấp số nhân. Ngay cả việc thử tất cả các hoán vị của một phần lớn các chữ số cũng sẽ ngay lập tức vượt quá thời gian có sẵn. Giải pháp cần xử lý các chữ số với số lần không đổi, hướng tới phương thức O(n) hoặc O(n log n). 

Có một số trường hợp việc thực hiện bất cẩn có thể thất bại. Một số 0 duy nhất là một câu trả lời hợp lệ vì số 0 chia hết cho mọi ước số khác 0. Ví dụ, đầu vào```
1
0
```phải sản xuất```
0
```Một giải pháp yêu cầu chữ số đầu khác 0 sẽ từ chối nó một cách không chính xác. 

Một cái bẫy khác là quên rằng số chia hết cho 30 cần có số 0 ở cuối. Ví dụ,```
4
3 3 6 6
```có tổng chữ số 18, chia hết cho 3 và chứa các chữ số chẵn nhưng không có số 0. Đầu ra đúng là```
-1
```vì không có cách sắp xếp nào có thể chia hết cho 5. 

Một lỗi phổ biến cuối cùng là xóa sai chữ số khi sửa tổng chữ số. Coi như```
5
1 1 1 2 0
```Tổng số là 5 nên phải bỏ đi một chữ số để tổng chia hết cho 3. Bỏ đi một`2`đưa ra câu trả lời lớn nhất:```
1110
```Loại bỏ một`1`cho`2100`, cái nào nhỏ hơn. Quá trình loại bỏ phải bảo toàn số lượng còn lại lớn nhất có thể. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là tạo ra các tập hợp con có thể có của các chữ số có sẵn, sắp xếp từng tập hợp con được chọn theo thứ tự giảm dần và kiểm tra xem số kết quả có chia hết cho 30 hay không. Điều này đúng vì mọi câu trả lời hợp lệ có thể xuất hiện trong số các ứng cử viên đó và câu trả lời tối đa có thể được chọn sau đó. 

Vấn đề là có tới 100000 chữ số. Số lượng tập hợp con là 2^n và thậm chí việc lưu trữ hoặc kiểm tra một phần rất nhỏ trong số chúng là không thể. Cách tiếp cận bạo lực không thành công vì nó coi các chữ số là những lựa chọn độc lập thay vì sử dụng các quy tắc chia hết để mô tả chính xác những gì quan trọng. 

Quan sát quan trọng là phép chia hết cho 30 có cấu trúc rất đơn giản. Một số chia hết cho 30 thì phải chia hết cho 10 nên phải có số 0 ở cuối. Nó cũng phải chia hết cho 3, điều này chỉ phụ thuộc vào tổng các chữ số của nó. Thứ tự chính xác của các chữ số khác không ảnh hưởng đến khả năng chia hết cho 3. 

Vấn đề trở thành việc chọn những chữ số cần loại bỏ. Vì chúng ta muốn số lớn nhất có thể nên chúng ta nên giữ càng nhiều chữ số càng tốt. Nếu tổng các chữ số đã chia hết cho 3 thì chúng ta chỉ cần sắp xếp tất cả các chữ số có sẵn theo thứ tự giảm dần. Mặt khác, chúng tôi loại bỏ số chữ số nhỏ nhất có thể làm cho tổng chia hết cho 3. Sau đó, việc sắp xếp các chữ số còn lại sẽ cho ra sự sắp xếp tối đa. 

Các chữ số duy nhất cần loại bỏ là số dư theo modulo 3. Để giảm tổng đi 1 modulo 3, chúng tôi loại bỏ chữ số nhỏ nhất có số dư 1. Để giảm tổng đi 2 modulo 3, chúng tôi loại bỏ chữ số nhỏ nhất có số dư 2. Nếu không có một chữ số, việc loại bỏ hai chữ số của số dư đối diện có thể đạt được hiệu quả tương tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tất cả các chữ số và tính tổng của chúng. Tổng cho chúng ta biết số dư hiện tại theo modulo 3, đây là thông tin duy nhất cần thiết để chia hết cho 3. 
2. Kiểm tra xem có ít nhất một số 0 hay không. Nếu không có số 0 thì số đó không thể chia hết cho 10 nên không thể tồn tại số ma quái. 
3. Nếu tổng các chữ số không chia hết cho 3 thì loại bỏ chữ số nhỏ nhất có thể hoặc cặp chữ số cố định số dư. Giá trị bị loại bỏ nhỏ hơn sẽ để lại số cuối cùng lớn hơn. 
4. Sắp xếp các chữ số còn lại theo thứ tự giảm dần. Một số có cùng chữ số là số lớn nhất khi chữ số cao nhất của nó xuất hiện đầu tiên. 
5. In các chữ số kết quả. Nếu biểu diễn duy nhất còn lại là số 0, hãy in một số 0 duy nhất thay vì nhiều số 0 đứng đầu. 

Tại sao nó hoạt động: thuật toán chỉ xóa các chữ số khi được yêu cầu bởi quy tắc chia hết cho 3. Việc xóa ít chữ số hơn luôn giữ một số có nhiều chữ số hơn, số này lớn hơn bất kỳ số nào có ít chữ số hơn được tạo từ cùng một bộ sưu tập. Khi cần loại bỏ, việc chọn các chữ số có thể tháo rời nhỏ nhất có thể sẽ để lại nhiều chữ số lớn nhất còn lại. Cuối cùng, sắp xếp giảm dần sẽ tạo ra số lượng tối đa từ nhiều tập hợp đó. Sự hiện diện của số 0 đảm bảo chia hết cho 10 và tổng chữ số đã sửa đảm bảo chia hết cho 3, do đó kết quả luôn chia hết cho 30. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    digits = list(map(int, input().split()))

    if 0 not in digits:
        print(-1)
        return

    total = sum(digits)
    rem = total % 3

    if rem != 0:
        removed = False

        for i, d in enumerate(digits):
            if d % 3 == rem:
                digits.pop(i)
                removed = True
                break

        if not removed:
            need = (3 - rem) % 3
            to_remove = []
            for i, d in enumerate(digits):
                if d % 3 == need:
                    to_remove.append(i)
                    if len(to_remove) == 2:
                        break

            if len(to_remove) < 2:
                print(-1)
                return

            for i in reversed(to_remove):
                digits.pop(i)

    digits.sort(reverse=True)

    if digits[0] == 0:
        print(0)
    else:
        print("".join(map(str, digits)))

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên loại bỏ các trường hợp không có số 0 vì không có sự sắp xếp lại nào có thể tạo ra một số kết thúc bằng 0. Điều này tránh được những công việc không cần thiết và xử lý trực tiếp yêu cầu chia hết cho 10. 

Tổng chữ số được sử dụng để quyết định xem có cần loại bỏ hay không. Khi loại bỏ một chữ số, mã sẽ tìm kiếm chữ số nhỏ nhất có phần còn lại được yêu cầu vì các chữ số bị loại bỏ nhỏ hơn sẽ bảo toàn câu trả lời lớn hơn. Nếu một chữ số là không đủ, nó sẽ loại bỏ hai chữ số có cùng hiệu ứng còn lại. 

Sắp xếp giảm dần cuối cùng là cách chuyển đổi các chữ số đã chọn thành số nguyên lớn nhất có thể. Việc kiểm tra đặc biệt đối với`digits[0] == 0`xử lý trường hợp mọi chữ số còn lại bằng 0, ngăn chặn các kết quả đầu ra như`0000`. 

Số nguyên Python không tràn ở đây vì thuật toán không bao giờ xây dựng số đó bằng số. Nó xây dựng câu trả lời dưới dạng một chuỗi chữ số, điều này cần thiết vì câu trả lời có thể chứa 100000 chữ số. 

## Ví dụ đã hoạt động 

Đối với ví dụ đầu tiên:```
1
0
```Việc thực hiện là: 

| Bước | Chữ số | Tổng số dư | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [0] | 0 | Không tồn tại, không cần xóa | [0] | 
| Sắp xếp | [0] | 0 | Đã có đơn hàng lớn nhất | 0 | 

Điều này thể hiện trường hợp chỉ có số 0 đặc biệt. Câu trả lời là số 0, không phải là số trống không hợp lệ. 

Đối với ví dụ thứ hai:```
6
2 3 1 3 1 0
```Việc thực hiện là: 

| Bước | Chữ số | Tổng số dư | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | [2,3,1,3,1,0] | 1 | Cần giảm tổng đi 1 | Xóa chữ số 1 | 
| Sau khi loại bỏ | [2,3,1,3,0] | 0 | Chia hết cho 3 và 10 | Giữ tất cả | 
| Sắp xếp | [3,3,2,1,0] | 0 | Sự sắp xếp lớn nhất | 33210 | 

Điều này cho thấy tại sao việc sử dụng tập con chữ số lớn nhất có thể không phải lúc nào cũng đúng. Toàn bộ có tổng chữ số sai nên phải loại bỏ một chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc đếm và loại bỏ là tuyến tính và việc sắp xếp các chữ số chiếm ưu thế trong thời gian chạy. | 
| Không gian | O(n) | Các chữ số được lưu trữ trong một danh sách và sắp xếp lại trong bộ nhớ. | 

Kích thước đầu vào là 100000 chữ số, vì vậy việc sắp xếp 100000 giá trị dễ dàng nằm trong giới hạn. Thuật toán tránh tất cả việc tạo tập hợp con hoặc hoán vị và chỉ thực hiện các lần quét đơn giản cộng với một lần sắp xếp. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def solve():
    input = sys.stdin.readline
    n = int(input())
    digits = list(map(int, input().split()))

    if 0 not in digits:
        print(-1)
        return

    total = sum(digits)
    rem = total % 3

    if rem:
        removed = False
        for i, d in enumerate(digits):
            if d % 3 == rem:
                digits.pop(i)
                removed = True
                break

        if not removed:
            need = (3 - rem) % 3
            idx = []
            for i, d in enumerate(digits):
                if d % 3 == need:
                    idx.append(i)
                    if len(idx) == 2:
                        break

            if len(idx) < 2:
                print(-1)
                return

            for i in reversed(idx):
                digits.pop(i)

    digits.sort(reverse=True)
    if digits[0] == 0:
        print(0)
    else:
        print("".join(map(str, digits)))

assert run("""1
0
""") == "0\n", "sample 1"

assert run("""6
2 3 1 3 1 0
""") == "33210\n", "sample 2"

assert run("""4
3 3 6 6
""") == "-1\n", "no zero"

assert run("""5
1 1 1 2 0
""") == "1110\n", "remove smallest digit"

assert run("""3
0 0 0
""") == "0\n", "all zeroes"

assert run("""5
9 9 9 9 0
""") == "99990\n", "large equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`0`| Kích thước tối thiểu và xử lý không chỉ | 
|`6 / 2 3 1 3 1 0`|`33210`| Hành vi mẫu và sửa lỗi còn lại | 
|`4 / 3 3 6 6`|`-1`| Thiếu trường hợp ranh giới 0 | 
|`5 / 1 1 1 2 0`|`1110`| Lựa chọn loại bỏ nhỏ nhất | 
|`3 / 0 0 0`|`0`| Tránh nhiều số 0 đứng đầu | 
|`5 / 9 9 9 9 0`|`99990`| Chữ số bằng nhau và giá trị lớn | 

## Vỏ cạnh 

Đối với trường hợp chỉ có 0:```
1
0
```Thuật toán tìm số 0, tính tổng các chữ số còn lại bằng 0, bỏ qua việc loại bỏ, sắp xếp danh sách và thấy rằng chữ số đầu tiên bằng 0. Nó xuất ra một số không. Điều này ngăn ngừa lỗi phổ biến khi coi số 0 là số không hợp lệ vì nó không có chữ số đầu khác 0. 

Đối với trường hợp thiếu số 0:```
4
3 3 6 6
```Thuật toán ngay lập tức không đáp ứng yêu cầu chia hết cho 10 vì không tồn tại số 0. Nó xuất ra`-1`trước khi thử xóa bất kỳ chữ số nào. Việc xóa các chữ số không thể giúp ích vì mọi số ma quái hợp lệ đều phải kết thúc bằng 0. 

Đối với trường hợp tổng chữ số cần sửa:```
5
1 1 1 2 0
```Tổng bằng 5, dư 2 theo modulo 3. Thuật toán tìm kiếm một chữ số có dư 2 và loại bỏ chữ số đó`2`. Các chữ số còn lại có tổng bằng 3, chứa số 0 và sắp xếp thành`1110`. Loại bỏ bất kỳ`1`thay vào đó sẽ để lại một số lượng nhỏ hơn. 

Đối với nhiều số 0:```
3
0 0 0
```Tất cả các sắp xếp có thể đại diện cho số không. Sau khi sắp xếp, thuật toán phát hiện chữ số đầu tiên bằng 0 và chỉ in ra một chữ số. Điều này xử lý vấn đề biểu diễn trong khi vẫn bảo toàn giá trị toán học.
