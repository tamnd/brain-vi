---
title: "CF 102569M - Thông báo"
description: "Mỗi thông báo đến vào một thời điểm cụ thể và chứa một video có thời lượng nhất định. Vasya luôn xem video theo thứ tự thông báo đến. Nếu anh ấy không sử dụng khi có thông báo xuất hiện, anh ấy sẽ ngay lập tức bắt đầu xem video đó."
date: "2026-08-01T06:03:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102569
codeforces_index: "M"
codeforces_contest_name: "2020, XIII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102569
solve_time_s: 70
verified: true
draft: false
---

[CF 102569M - Thông báo](https://codeforces.com/problemset/problem/102569/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thông báo đến vào một thời điểm cụ thể và chứa một video có thời lượng nhất định. Vasya luôn xem video theo thứ tự thông báo đến. Nếu anh ấy không sử dụng khi có thông báo xuất hiện, anh ấy sẽ ngay lập tức bắt đầu xem video đó. Nếu anh ấy đang xem nội dung nào đó, video mới sẽ đợi trong hàng đợi cho đến khi mọi video chưa xem hết trước đó được xem. 

Đầu vào đã đưa ra các thông báo theo thứ tự không giảm về thời gian đến, vì vậy chúng ta không bao giờ cần phải sắp xếp lại chúng. Nhiệm vụ chỉ đơn giản là xác định thời gian hoàn thành video cuối cùng sau khi xử lý mọi thông báo. 

Số lượng thông báo lên tới 200000, trong khi cả thời gian đến và thời lượng có thể lớn tới (10^9). Bất kỳ thuật toán nào liên tục mô phỏng thời gian hoặc quét các thông báo đã xử lý trước đó để tìm mỗi thông báo mới sẽ thực hiện quá nhiều thao tác. Một giải pháp tuyến tính dễ dàng đủ nhanh, trong khi các phương pháp bậc hai bị loại trừ. Vì thời gian kết thúc có thể vượt quá (2 \times 10^9), nên thuật toán cũng phải tránh giả định rằng các giá trị khớp với số nguyên 32 bit, mặc dù số nguyên Python xử lý điều này một cách tự nhiên. 

Một sai lầm dễ mắc phải là quên rằng Vasya có thể không hoạt động trước khi có thông báo tiếp theo. 

Ví dụ:```
2
1 2
10 3
```Câu trả lời đúng là`13`. Vasya đã xem hết video đầu tiên`3`, đợi đến lúc`10`, sau đó xem video thứ hai cho đến khi`13`. Chỉ cần thêm thời lượng vào thời gian kết thúc trước đó sẽ tạo ra kết quả không chính xác`6`. 

Một trường hợp tinh tế khác xảy ra khi một số thông báo đến vào cùng một thời điểm.```
3
5 2
5 4
5 1
```Câu trả lời đúng là`12`. Video đầu tiên bắt đầu ngay tại thời điểm`5`, và hai cái còn lại được thêm vào sau nó theo thứ tự đến. Việc xử lý các thông báo có cùng dấu thời gian như các sự kiện độc lập đồng thời sẽ làm mất thứ tự đó. 

Trường hợp thứ ba là khi có thông báo đúng lúc video trước đó kết thúc.```
2
1 4
5 3
```Câu trả lời đúng là`8`. Vào thời điểm`5`Vasya không còn bận nữa nên video thứ hai sẽ bắt đầu ngay lập tức. Sử dụng một so sánh nghiêm ngặt như`finish > arrival`thay vì`finish >= arrival`có thể thay đổi lịch trình không chính xác. 

## Phương pháp tiếp cận 

Một mô phỏng đơn giản sẽ giữ mọi video chờ trong hàng đợi. Bất cứ khi nào có thông báo đến, mô phỏng sẽ tăng thời gian, xóa mọi video đã hoàn thành, sau đó bắt đầu video đến ngay lập tức hoặc thêm video đó vào hàng đợi. Điều này mô hình hóa một cách trung thực các quy tắc, nhưng trong trường hợp xấu nhất, mỗi lần đến có thể xử lý lại nhiều video được xếp hàng đợi. Với (n) thông báo, điều này có thể yêu cầu (O(n^2)) hoạt động, khoảng (4 \times 10^{10}) thao tác khi (n = 200000). 

Bản thân hàng đợi thực sự không cần thiết. Tại bất kỳ thời điểm nào, thông tin duy nhất ảnh hưởng đến các thông báo trong tương lai là thời điểm Vasya sẽ rảnh rỗi. Giả sử sau khi xử lý một số thông báo, anh ta chắc chắn sẽ bận rộn cho đến khi`finish`. Khi có thông báo mới đến đúng lúc`t`, chỉ có hai trường hợp có thể xảy ra. 

Nếu như`finish <= t`, Vasya không hoạt động khi thông báo xuất hiện. Video mới bắt đầu lúc`t`và kết thúc tại`t + d`. 

Nếu như`finish > t`, Vasya vẫn đang bận. Video mới bắt đầu chính xác vào lúc`finish`và kết thúc tại`finish + d`. 

Cả hai tình huống có thể được thể hiện bằng một công thức:```
finish = max(finish, t) + d
```Biến duy nhất này nắm bắt hoàn toàn trạng thái của toàn bộ hàng đợi xem, cho phép mọi thông báo được xử lý một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`finish = 0`. Trước khi có bất kỳ thông báo nào đến, Vasya không hoạt động. 
2. Đọc từng thông báo theo thứ tự nhất định. Đầu vào đã được sắp xếp theo thời gian đến nên không cần xử lý trước. 
3. Đối với mỗi thông báo có thời gian đến`t`và thời lượng`d`, tính toán`start = max(finish, t)`. Nếu Vasya vẫn bận, video sẽ đợi cho đến khi`finish`. Nếu không thì nó bắt đầu ngay lập tức lúc`t`. 
4. Cập nhật`finish = start + d`. Đây trở thành thời điểm sớm nhất khi mọi thông báo được xử lý đều được xem đầy đủ. 
5. Sau khi tất cả các thông báo đã được xử lý, hãy xuất ra`finish`. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý lần đầu tiên`i`thông báo,`finish`bằng thời gian chính xác khi tất cả các video đó đã được xem. Khi có thông báo tiếp theo, không có video nào sớm hơn có thể bắt đầu muộn hơn`finish`, vì mọi thông báo trước đó đã được lên lịch. Nếu Vasya không hoạt động, video mới sẽ bắt đầu ngay lập tức. Nếu anh ấy bận thì mọi video trước đó phải kết thúc trước khi video mới có thể bắt đầu, vì vậy thời điểm bắt đầu sớm nhất có thể chính xác là`finish`. Đang cập nhật`finish`với`max(finish, t) + d`giữ nguyên sự bất biến và sau thông báo cuối cùng, nó thể hiện thời gian hoàn thành của video cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    finish = 0

    for _ in range(n):
        t, d = map(int, input().split())
        finish = max(finish, t) + d

    print(finish)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ giữ một biến,`finish`, đại diện cho sự kết thúc của lịch trình hiện tại. Mọi thông báo đều cập nhật giá trị này bằng công thức rút ra từ thuật toán. 

biểu thức`max(finish, t)`xử lý cả hai trạng thái có thể mà không cần phân nhánh. Nếu Vasya đã bận thì lịch trình hiện tại sẽ tiếp tục không bị gián đoạn. Nếu anh ấy đã hoàn thành mọi việc, lịch trình sẽ bắt đầu lại từ thời điểm thông báo đến. 

Việc so sánh sử dụng`max`thay vì một sự bất bình đẳng nghiêm ngặt, xử lý chính xác các thông báo đến chính xác khi video trước đó kết thúc. Số nguyên Python tự động tăng lên khi cần thiết, do đó, ngay cả thời gian hoàn thành rất lớn vẫn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 4
3 3
6 1
10 2
10 3
```| Thông báo | Đến | Thời lượng | Kết thúc trước | Bắt đầu | Kết thúc mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 0 | 1 | 5 | 
| 2 | 3 | 3 | 5 | 5 | 8 | 
| 3 | 6 | 1 | 8 | 8 | 9 | 
| 4 | 10 | 2 | 9 | 10 | 12 | 
| 5 | 10 | 3 | 12 | 12 | 15 | 

Lịch trình sẽ trống sau video thứ ba, vì vậy video thứ tư sẽ bắt đầu ngay lập tức`10`. Thông báo thứ năm đến trong khi thông báo thứ tư đang phát, vì vậy nó sẽ đợi đến thời gian`12`. 

### Ví dụ 2 

đầu vào:```
3
5 2
5 4
5 1
```| Thông báo | Đến | Thời lượng | Kết thúc trước | Bắt đầu | Kết thúc mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 0 | 5 | 7 | 
| 2 | 5 | 4 | 7 | 7 | 11 | 
| 3 | 5 | 1 | 11 | 11 | 12 | 

Mọi thông báo đều đến cùng nhau nhưng chúng vẫn được xử lý theo thứ tự đầu vào. Thời gian kết thúc chạy sẽ giữ nguyên thứ tự đó một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi thông báo được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ có thời gian kết thúc hiện tại được lưu trữ. | 

Việc xử lý 200000 thông báo với công việc liên tục trên mỗi thông báo dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ liên tục thấp hơn nhiều so với bộ nhớ khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    n = int(input())
    finish = 0
    for _ in range(n):
        t, d = map(int, input().split())
        finish = max(finish, t) + d
    print(finish)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out.getvalue().strip()

assert run("5\n1 4\n3 3\n6 1\n10 2\n10 3\n") == "15", "sample 1"

assert run("1\n7 5\n") == "12", "single notification"

assert run("2\n1 2\n10 3\n") == "13", "idle period"

assert run("3\n5 2\n5 4\n5 1\n") == "12", "same arrival time"

assert run("2\n1 4\n5 3\n") == "8", "arrival at finish"

assert run("3\n1 1000000000\n1 1000000000\n1 1000000000\n") == "3000000001", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một thông báo | 12 | Đầu vào hợp lệ tối thiểu | 
| Khoảng trống nhàn rỗi | 13 | Lên lịch khởi động lại sau khi không hoạt động | 
| Thời gian đến bằng nhau | 12 | Thứ tự đầu vào được giữ nguyên | 
| Về đích | 8 | Ranh giới nơi video kết thúc chính xác vào thời điểm thông báo | 
| Thời lượng lớn | 3000000001 | Xử lý đúng thời gian hoàn thành rất lớn | 

## Vỏ cạnh 

Hãy xem xét một khoảng trống nhàn rỗi:```
2
1 2
10 3
```Thuật toán bắt đầu với`finish = 0`. Sau thông báo đầu tiên,`finish = max(0, 1) + 2 = 3`. Đối với thông báo thứ hai,`finish = max(3, 10) + 3 = 13`. Lịch trình khởi động lại chính xác vào thời điểm`10`thay vì tiếp tục theo thời gian`3`. 

Bây giờ hãy xem xét nhiều thông báo có cùng dấu thời gian.```
3
5 2
5 4
5 1
```Các bản cập nhật là`7`, sau đó`11`, sau đó`12`. Mọi thông báo đều được xử lý theo thứ tự đầu vào vì mỗi thời gian bắt đầu mới phụ thuộc vào thời gian kết thúc do thông báo trước đó tạo ra. 

Cuối cùng, hãy xem xét thông báo đến đúng thời điểm video trước đó kết thúc.```
2
1 4
5 3
```Sau thông báo đầu tiên,`finish = 5`. Bản cập nhật thứ hai trở thành`max(5, 5) + 3 = 8`. Từ`finish`và thời gian đến bằng nhau, video mới sẽ bắt đầu ngay lập tức mà không phải chờ đợi không cần thiết, phù hợp với hành vi được yêu cầu.
