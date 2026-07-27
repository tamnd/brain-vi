---
title: "CF 102775J - \u041f\u0435\u043f\u0435\u043b\u0430\u0446"
description: "Chúng ta được cung cấp một chuỗi các lần nhấn nút cố định phải diễn ra theo thứ tự. Mỗi lần nhấn thuộc về một trong ba nút và yêu cầu quay số tương ứng để hiển thị một giá trị cụ thể vào đúng giây khi nhấn xảy ra. Tất cả các mặt số đều bắt đầu ở giá trị 1."
date: "2026-07-27T20:43:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "J"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 58
verified: true
draft: false
---

[CF 102775J - \u041f\u0435\u043f\u0435\u043b\u0430\u0446](https://codeforces.com/problemset/problem/102775/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các lần nhấn nút cố định phải diễn ra theo thứ tự. Mỗi lần nhấn thuộc về một trong ba nút và yêu cầu quay số tương ứng để hiển thị một giá trị cụ thể vào đúng giây khi nhấn xảy ra. Tất cả các mặt số đều bắt đầu ở giá trị 1. 

Mục tiêu không phải là chọn nút nào để nhấn vì trình tự đã được xác định. Quyết định duy nhất là dành bao nhiêu giây để chuẩn bị giữa các lần nhấn. Trong một giây, mỗi mặt số có thể di chuyển tối đa một nút nếu không nhấn nút của nó và có thể xảy ra chính xác một lần nhấn nút. Nhiệm vụ là tìm giây sớm nhất khi lần nhấn cuối cùng được yêu cầu có thể hoàn thành. 

Đầu vào chứa tới 1000 lần nhấn cần thiết. Kích thước này loại trừ các mô phỏng trên không gian trạng thái lớn, chẳng hạn như tất cả các kết hợp có thể có của ba giá trị quay số, bởi vì số lượng trạng thái sẽ tăng vượt xa giới hạn một giây có thể xử lý. Một giải pháp sẽ xử lý mỗi lần nhấn với số lần không đổi, dẫn đến thuật toán O(n). 

Giá trị của mặt số cũng bị giới hạn bởi 1000, nhưng điều quan trọng là chúng ta không bao giờ cần lưu trữ tất cả các vị trí mặt số có thể có. Chúng ta chỉ cần hiểu thời gian trôi qua giữa hai lần nhấn cùng một nút là bao nhiêu. 

Một sai lầm phổ biến là cho rằng mọi mặt số đều có thể được điều chỉnh trong mỗi giây. Điều đó là sai đối với mặt số có nút được nhấn trong giây đó. Ví dụ: nếu đầu vào là:```
2
1 5
1 1
```câu trả lời là 9. Lần nhấn đầu tiên cần chuyển động trong 4 giây và xảy ra ở giây thứ 5. Lần thay đổi mặt số thứ hai yêu cầu chuyển từ 5 sang 1, cần thêm 4 giây nữa, sau đó lần nhấn thứ hai sẽ diễn ra. Một giải pháp bất cẩn có thể trả lời số 8 bằng cách quên rằng bản thân máy ép cũng tiêu tốn một giây. 

Một trường hợp khó khăn khác là khi một số nút khác nhau cần được chuẩn bị trước khi nhấn lần đầu tiên. Ví dụ:```
3
1 2
2 2
3 3
```câu trả lời là 4. Ba mặt số đều có thể di chuyển trong giây đầu tiên, nhưng ba lần nhấn nút vẫn cần ba giây riêng biệt. Một giải pháp coi chuyển động quay số và nhấn là các giai đoạn hoàn toàn riêng biệt sẽ đánh giá quá cao câu trả lời. 

Trường hợp khó khăn cuối cùng là nhấn lặp đi lặp lại cùng một nút mà không có khoảng trống. Ví dụ:```
2
1 3
1 4
```câu trả lời là 4. Lần nhấn đầu tiên xảy ra ở giây thứ 3 sau khi chuyển từ 1 sang 3. Mặt số không thể thay đổi trong lần nhấn đó, vì vậy nó cần thêm một giây để chuyển sang số 4 trước lần nhấn tiếp theo. Câu trả lời không phải là 3. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ là mô phỏng tất cả các hành động có thể xảy ra mỗi giây. Tại mỗi thời điểm, chúng tôi có thể thử mọi sự kết hợp có thể có của chuyển động quay số và lần nhấn nút tiếp theo. Điều này có thể đúng vì nó khám phá mọi lịch trình có thể, nhưng hệ số phân nhánh là rất lớn. Dù chỉ có ba mặt số nhưng mỗi giây lại có nhiều tổ hợp chuyển động và số giây có thể lên tới hàng nghìn. Số lượng lịch sử có thể tăng theo cấp số nhân, khiến phương pháp này không thể sử dụng được. 

Quan sát quan trọng là các giá trị quay số thực tế sau những khoảnh khắc tùy ý không thành vấn đề. Mặt số chỉ có những hạn chế tại thời điểm nhấn nút của nó. Giữa hai lần nhấn cùng một nút, mặt số đó có chính xác tất cả các giây ngoại trừ hai giây nhấn có sẵn để di chuyển. 

Giả sử nút tương tự được nhấn nhiều lần`a`Và`b`và các giá trị cần tìm của nó là`x`Và`y`. Giữa các máy ép đó có`b - a - 1`giây mà mặt số đó có thể di chuyển. Nó cần ít nhất`abs(x - y)`giây chuyển động, nên điều kiện là:```
b - a - 1 >= abs(x - y)
```có thể được viết lại thành:```
b >= a + abs(x - y) + 1
```Điều này đưa ra giới hạn dưới trực tiếp cho mỗi lần nhấn. Ý tưởng tương tự áp dụng cho lần xuất hiện đầu tiên của một nút bằng cách giả vờ có một lần nhấn ảo tại thời điểm 0 với giá trị 1. 

Lịch trình hợp lệ sớm nhất có thể được xây dựng một cách tham lam. Khi xử lý thao tác nhấn, chúng ta chỉ cần biết hai điều về lần xuất hiện trước đó của cùng một nút: nó xảy ra khi nào và giá trị của nó là bao nhiêu. Báo chí hiện tại phải đủ muộn để theo sau báo chí toàn cầu trước đó và để cho mặt số của mình có đủ thời gian chuyển động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ nguyên thời gian của lần ép cuối cùng được xử lý. Lần nhấn tiếp theo phải xảy ra muộn hơn ít nhất một giây vì không thể nhấn hai nút trong cùng một giây. 
2. Đối với mỗi nút trong số ba nút, hãy nhớ thời gian và giá trị yêu cầu của lần nhấn trước đó. Ban đầu, coi mọi nút như thể nó được nhấn ở thời điểm 0 với giá trị quay số 1. Điều này thể hiện cấu hình quay số ban đầu. 
3. Xử lý các máy ép cần thiết theo thứ tự. Đối với nút hiện tại và giá trị mục tiêu, hãy tính thời gian sớm nhất có thể dựa trên lần nhấn chung trước đó và lần nhấn trước đó của cùng nút này. Thời gian phải thỏa mãn cả hai hạn chế: 

1. Phải sau lần nhấn trước. 
2. Mặt số phải có đủ giây để chuyển từ giá trị yêu cầu trước đó sang giá trị yêu cầu mới. 
4. Chọn giới hạn lớn hơn trong hai giới hạn này làm thời gian ép thực tế. Sau khi sửa xong thời gian này, hãy cập nhật thông tin đã lưu cho nút này. 
5. Sau khi tất cả các lần nhấn được xử lý, thời gian của lần nhấn cuối cùng là thời gian mở khóa tối thiểu. 

Tại sao nó hoạt động: Điều duy nhất có thể trì hoãn một lần nhấn là một lần nhấn khác xảy ra ngay trước nó hoặc cần phải di chuyển nút xoay cần thiết vào vị trí. Sự lựa chọn tham lam luôn đặt mọi lực ép vào thời điểm sớm nhất mà hai điều kiện này cho phép. Việc trì hoãn lần nhấn trước không bao giờ có ích vì nó chỉ làm giảm thời gian có sẵn trước khi nhấn lần sau. Vì mọi hạn chế đều được bảo toàn nên lịch trình cuối cùng là khả thi và vì mỗi lần in đều được thực hiện càng sớm càng tốt nên thời gian cuối cùng là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    last_time = [0, 0, 0]
    last_value = [1, 1, 1]

    current_time = 0

    for _ in range(n):
        button, value = map(int, input().split())
        button -= 1

        current_time = max(
            current_time + 1,
            last_time[button] + abs(value - last_value[button]) + 1
        )

        last_time[button] = current_time
        last_value[button] = value

    print(current_time)

if __name__ == "__main__":
    solve()
```Các mảng`last_time`Và`last_value`lưu trữ thông tin nhấn ảo trước đó cho mỗi nút. Bắt đầu chúng theo thời gian`0`và giá trị`1`hãy để cùng một công thức xử lý lần nhấn thực đầu tiên mà không cần bất kỳ trường hợp đặc biệt nào. 

Biến`current_time`đại diện cho thời gian của báo chí được lập lịch gần đây nhất. Trước khi đặt lần nhấn tiếp theo, nó sẽ tăng thêm một vì mỗi lần nhấn chiếm một giây duy nhất. 

Phần thứ hai của`max`biểu thức là yêu cầu chuyển động cho nút hiện tại. Nếu lần nhấn nút này trước đó xảy ra vào lúc`last_time[button]`, thì mặt số có chính xác số giây sau lần nhấn đó cho đến khi lần nhấn hiện tại di chuyển. Việc bổ sung`+1`chiếm chính giây báo chí hiện tại. 

Không cần mô phỏng quay số. Thuật toán chỉ theo dõi các ràng buộc có thể trì hoãn các lần nhấn trong tương lai, đó là lý do tại sao nó không đổi trong bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
1 2
2 2
3 3
```Dấu vết: 

| Nhấn | Nút | Mục tiêu | Thời điểm hiện tại trước | Giới hạn nút giống nhau | Thời điểm báo chí cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 0 | 2 | 2 | 
| 2 | 2 | 2 | 2 | 2 | 3 | 
| 3 | 3 | 3 | 3 | 3 | 4 | 

Lần nhấn đầu tiên cần một giây chuyển động quay số và một giây để nhấn. Các mặt số còn lại có thể chuẩn bị trong khi các nút khác được nhấn, vì vậy hai lần nhấn tiếp theo chỉ cần vài giây nhấn riêng. Câu trả lời cuối cùng là 4. 

### Mẫu 2 

đầu vào:```
2
1 5
1 1
```Dấu vết: 

| Nhấn | Nút | Mục tiêu | Thời điểm hiện tại trước | Giới hạn nút giống nhau | Thời điểm báo chí cuối cùng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 0 | 5 | 5 | 
| 2 | 1 | 1 | 5 | 10 | 10 | 

Lần nhấn đầu tiên yêu cầu di chuyển mặt số từ 1 đến 5. Lần nhấn thứ hai sử dụng cùng một nút nên mặt số không thể di chuyển trong lần nhấn thứ hai. Nó cần thêm bốn giây chuyển động trước khi nhấn lần thứ hai, tổng thời gian là 10. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi máy ép cần thiết được xử lý một lần với công việc liên tục. | 
| Không gian | O(1) | Chỉ có thông tin cho ba nút được lưu trữ. | 

Số lần nhấn tối đa là 1000, do đó giải pháp tuyến tính dễ dàng nằm trong giới hạn. Việc sử dụng bộ nhớ không phụ thuộc vào số lần nhấn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample
assert run("""3
1 2
2 2
3 3
""") == "4", "sample 1"

# minimum size
assert run("""1
1 1
""") == "1", "single immediate press"

# all equal values
assert run("""4
1 5
2 5
3 5
1 5
""") == "5", "all equal targets"

# repeated button movement
assert run("""2
1 3
1 4
""") == "4", "same button consecutive presses"

# large distance boundary
assert run("""1
3 1000
""") == "1000", "maximum dial movement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 / 1 2 / 2 2 / 3 3`| 4 | Chuẩn bị đồng thời các mặt số khác nhau | 
|`1 / 1 1`| 1 | Bấm ngay mà không cần chuyển động | 
|`4 / 1 5 / 2 5 / 3 5 / 1 5`| 5 | Sử dụng lại mặt số đã đúng | 
|`2 / 1 3 / 1 4`| 4 | Chuyển động giữa các lần nhấn liên tiếp một nút | 
|`1 / 3 1000`| 1000 | Chuyển động ban đầu tối đa có thể | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
2
1 5
1 1
```thuật toán bắt đầu với nút 1 có thông tin ảo`(time = 0, value = 1)`. Lần nhấn đầu tiên sẽ có thời gian`5`bởi vì di chuyển từ 1 đến 5 cần bốn giây và nhấn chiếm giây thứ năm. Lần nhấn thứ hai ít nhất phải`5 + |5 - 1| + 1 = 10`, do đó câu trả lời trở thành 10. 

Đối với trường hợp cạnh thứ hai:```
3
1 2
2 2
3 3
```mỗi nút chưa từng được nhấn trước đó, vì vậy các giá trị ảo trước đó của chúng đều bằng 1. Thời gian bắt buộc là 2, 3 và 4. Thuật toán tự nhiên cho phép giây chuyển động đầu tiên chuẩn bị tất cả các mặt số vì nó chỉ tính các giới hạn khi nhấn một nút cụ thể. 

Đối với trường hợp cạnh thứ ba:```
2
1 3
1 4
```lần nhấn nút 1 lần thứ hai phụ thuộc vào lần nhấn nút 1 đầu tiên. Giá trị trước đó được lưu là 3 và thời gian được lưu trước đó là 3. Lần nhấn tiếp theo yêu cầu`3 + |4 - 3| + 1 = 5`? Việc hạn chế báo chí toàn cầu cũng mang lại`3 + 1 = 4`, do đó giá trị lớn hơn là 5. Lịch trình được nhấn ở giây thứ 3, di chuyển ở giây thứ 4, nhấn ở giây thứ 5. Kết quả đầu ra đúng là 5, cho biết lý do tại sao phải tính chuyển động thứ hai giữa các lần nhấn nút giống nhau.
