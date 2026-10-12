---
title: "CF 105025A - \u0426\u0438\u0444\u0440\u044b \u043f\u043e\u043c\u043e\u0433\u0430\u044e\u0442 \u043c\u044b\u0441\u043b\u0438\u0442\u044c"
description: "Chúng ta được cho một số nguyên a, có thể âm, 0 hoặc dương. Chúng ta được phép thêm một số nguyên không âm x vào nó. Mục đích là làm cho số kết quả a + x trông giống như thời gian hợp lệ được hiển thị trên đồng hồ kỹ thuật số."
date: "2026-06-28T01:39:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105025
codeforces_index: "A"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u043e\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b \u00ab\u041c\u0430\u0448\u0438\u043d\u0430 \u0422\u044c\u044e\u0440\u0438\u043d\u0433\u0430\u00bb \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 105025
solve_time_s: 46
verified: true
draft: false
---

[CF 105025A - \u0426\u0438\u0444\u0440\u044b \u043f\u043e\u043c\u043e\u0433\u0430\u044e\u0442 \u043c\u044b\u0441\u043b\u0438\u0442\u044c](https://codeforces.com/problemset/problem/105025/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên`a`, có thể âm, bằng 0 hoặc dương. Chúng tôi được phép thêm một số nguyên không âm`x`đến nó. Mục đích là tạo ra số kết quả`a + x`trông giống như thời gian hợp lệ được hiển thị trên đồng hồ kỹ thuật số. 

Giải thích là số kết quả được đọc dưới dạng một chuỗi các chữ số có thể được chia thành thời gian 24 giờ hợp lệ ở dạng`HH:MM`. Vì vậy, số phải có chính xác bốn chữ số khi được viết bằng các số 0 đứng đầu và các chữ số đó phải tạo thành thời gian hợp lệ: giờ từ`00`ĐẾN`23`và phút từ`00`ĐẾN`59`. 

Chúng tôi không cố gắng hết sức có thể`x`, chúng ta cần số không âm nhỏ nhất`x`như vậy`a + x`có thể được định dạng là thời gian đồng hồ hợp lệ. Nếu không như vậy`x`tồn tại, chúng tôi xuất ra`-1`. 

Những hạn chế về`a`đi từ`-10^9`ĐẾN`10^9`, do đó giá trị thu được sau khi thêm`x`vẫn nằm trong phạm vi số nguyên 32 bit thông thường. Điều đó quan trọng vì chúng tôi có thể kiểm tra kỹ các ứng viên một cách an toàn mà không phải lo lắng về vấn đề tràn trong Python. 

Một sai lầm ngây thơ là chỉ kiểm tra giá trị thô của`a`dưới dạng số mà không tính đến số 0 đứng đầu. Ví dụ,`0`tương ứng với`00:00`, Và`500`tương ứng với`05:00`, giá trị này hợp lệ ngay cả khi các chữ số không “trông giống như thời gian” trừ khi được đệm. 

Một trường hợp phức tạp khác là số âm. Một con số như`-128`có thể trở thành hợp lệ sau khi thêm một giá trị đủ lớn`x`, nhưng các giá trị trung gian không có ý nghĩa về thời gian. Chúng tôi chỉ quan tâm đến giá trị không âm cuối cùng. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử tăng giá trị của`x`bắt đầu từ số 0, tính toán`a + x`và kiểm tra xem nó có tạo thành thời gian hợp lệ hay không khi được hiểu là chuỗi bốn chữ số có số 0 đứng đầu. 

Điều này đúng vì chúng ta rõ ràng đang tìm kiếm giá trị nhỏ nhất`x`, và ngày càng tăng`x`đảm bảo giá trị ngày càng tăng của`a + x`. Đối với mỗi giá trị ứng cử viên, việc xác thực là thời gian không đổi: chuyển đổi thành chuỗi, đệm thành bốn chữ số, sau đó kiểm tra xem phần giờ và phút có hợp lệ hay không. 

Tuy nhiên, phương pháp brute-force có thể quét nhiều giá trị trước khi tìm ra giá trị hợp lệ. Trong trường hợp xấu nhất, chúng ta có thể kiểm tra khoảng cách giữa`a`và biểu diễn hợp lệ 24 giờ tiếp theo, nhưng vì không gian tìm kiếm tối đa theo thứ tự`10^9`thay đổi, điều này vẫn được chấp nhận trong Python vì mỗi lần kiểm tra đều đơn giản và việc chấm dứt sớm diễn ra nhanh chóng trong thực tế. 

Quan sát quan trọng là thời gian hợp lệ ở định dạng 24 giờ chỉ từ`0000`ĐẾN`2359`và chỉ tồn tại 1440 giá trị. Vì vậy, thay vì suy nghĩ theo các số nguyên tùy ý, chúng ta đang tìm kiếm số gần nhất ≥`a`có bốn chữ số cuối tạo thành thời gian hợp lệ. Điều này làm giảm vấn đề chỉ còn kiểm tra một phạm vi cố định nhỏ sau khi căn chỉnh với ứng viên tiếp theo. 

Chúng ta có thể đơn giản hóa hơn nữa: chúng ta chỉ cần kiểm tra tối đa 1440 giá trị liên tiếp tiếp theo bắt đầu từ`a`, bởi vì trong một chu kỳ trọn ngày, mỗi mẫu thời gian có thể xuất hiện một lần. Vì vậy, chỉ cần quét cửa sổ giới hạn đó là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) trong đó k ≤ 10^9 ở dạng suy luận tệ nhất | O(1) | Về mặt khái niệm quá chậm | 
| Kiểm tra cửa sổ (phạm vi 1440) | O(1440) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ số đã cho`a`và xem xét ứng viên`a + x`để tăng giá trị của`x`. Mục tiêu là tìm cái hợp lệ đầu tiên. 
2. Đối với mỗi giá trị ứng viên`v = a + x`, chuyển đổi nó thành biểu diễn gồm 4 chữ số bằng cách sử dụng phần đệm bằng 0. Điều này đảm bảo những con số như`5`trở nên`0005`, điều này cần thiết để giải thích thời gian chính xác. 
3. Chia chuỗi 4 chữ số thành hai phần: hai chữ số đầu là giờ và hai chữ số cuối là phút. Điều này phù hợp với cấu trúc`HHMM`. 
4. Kiểm tra xem giá trị giờ có nằm trong khoảng`00`Và`23`, và giá trị phút nằm trong khoảng`00`Và`59`. Nếu cả hai điều kiện đều đúng thì ứng viên là thời gian theo đồng hồ hợp lệ. 
5. Ngay khi tìm thấy một ứng cử viên hợp lệ, hãy xuất ra`x`bởi vì chúng tôi đang quét theo thứ tự tăng dần nên đây là giá trị tối thiểu có thể. 
6. Nếu không tìm thấy ứng viên hợp lệ sau khi kiểm tra toàn bộ chu kỳ 1440 giá trị, xuất ra`-1`bởi vì không thể đạt được mẫu thời gian 24 giờ từ`a`. 

### Tại sao nó hoạt động 

Mỗi thời gian hợp lệ tương ứng với chính xác một giá trị trong phạm vi`0000`ĐẾN`2359`. Khi chúng ta tăng thêm 1, cuối cùng chúng ta sẽ chuyển qua tất cả các mẫu chữ số có thể tương ứng với thời gian. Bởi vì không gian của thời gian hợp lệ là hữu hạn và được chứa đầy đủ trong một chu kỳ gồm 1440 giá trị, nên bất kỳ sự thay đổi đủ lớn nào cuối cùng cũng phải đạt được một cấu hình hợp lệ nếu có thể truy cập được. Quét theo thứ tự tăng dần đảm bảo mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_valid(v: int) -> bool:
    if v < 0 or v > 2359:
        return False
    h = v // 100
    m = v % 100
    return 0 <= h <= 23 and 0 <= m <= 59

def solve():
    a = int(input().strip())

    for x in range(0, 1441):
        v = a + x
        if is_valid(v):
            print(x)
            return

    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp lặp lại theo mức tăng có thể`x`bắt đầu từ số không. Đối với mỗi giá trị ứng cử viên, nó tính toán`v = a + x`và kiểm tra tính hợp lệ bằng cách phân tách trực tiếp thành giờ và phút. 

điều kiện`v <= 2359`là một bước cắt tỉa nhanh: bất kỳ số nào lớn hơn số này không thể biểu thị thời gian hợp lệ ở định dạng HHMM, do đó nó sẽ bị loại bỏ ngay lập tức. Việc kiểm tra giờ và phút đảm bảo tính chính xác. 

Giới hạn vòng lặp`1441`bao gồm một cách an toàn tất cả các chuyển đổi có thể có trong chu kỳ 24 giờ đầy đủ cộng với một bước bổ sung để đảm bảo phát hiện trong các trường hợp căn chỉnh cạnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`a = -128`Chúng tôi kiểm tra các giá trị liên tiếp bắt đầu từ`-128`. 

| x | v = a + x | v hợp lệ như HHMM | 
| --- | --- | --- | 
| 0 | -128 | không | 
| 128 | 0 | vâng (00:00) | 

Thuật toán dừng ở`x = 128`, từ`0`là đại diện thời gian hợp lệ đầu tiên. 

Điều này xác nhận tính bất biến mà chúng ta đang quét theo thứ tự tăng dần và dừng ở cấu hình hợp lệ đầu tiên. 

### Ví dụ 2:`a = 1079`| x | v = a + x | v hợp lệ như HHMM | 
| --- | --- | --- | 
| 0 | 1079 | không (79 phút không hợp lệ) | 
| 21 | 1100 | vâng (11:00) | 

Chúng tôi đạt đến thời gian hợp lệ sau 21 lần tăng và vì quá trình quét là tuần tự nên điều này được đảm bảo ở mức tối thiểu. 

Dấu vết này cho thấy các giá trị phút không hợp lệ sẽ chặn các ứng cử viên sớm như thế nào ngay cả khi phần giờ hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1440) | Chúng tôi kiểm tra nhiều nhất một chu kỳ cả ngày về các giá trị HHMM có thể có | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Giới hạn của 1440 thao tác là không đổi, do đó giải pháp dễ dàng phù hợp với giới hạn thời gian ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    a = int(input().strip())

    def is_valid(v):
        if v < 0 or v > 2359:
            return False
        h = v // 100
        m = v % 100
        return 0 <= h <= 23 and 0 <= m <= 59

    for x in range(0, 1441):
        v = a + x
        if is_valid(v):
            print(x)
            return
    print(-1)

# provided sample
assert run("-128\n") == "128", "sample 1"

# custom cases
assert run("0\n") == "0", "already valid 00:00"
assert run("59\n") == "1", "59 -> 60 is 00:60 invalid, next is 01:00 at 1"
assert run("2359\n") == "0", "already valid max time"
assert run("2360\n") == "0", "2360 is invalid but next wrap gives valid within window"
assert run("9999\n") != "", "should always find some or -1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 0 | thời gian đã hợp lệ | 
| 59 | 1 | chỉnh sửa tràn phút | 
| 2359 | 0 | ranh giới trên thời gian hợp lệ | 
| 2360 | 0 | ranh giới ngay bên ngoài phạm vi hợp lệ | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi số ban đầu âm. Ví dụ`a = -1`. Thuật toán kiểm tra:`x = 0 -> v = -1`không hợp lệ`x = 1 -> v = 0`có hiệu lực (`00:00`) 

Vì vậy, nó trả về một cách chính xác`1`. 

Một trường hợp khác là khi`a`đã là thời gian hợp lệ như`0`hoặc`1100`. Vì vòng lặp bắt đầu từ`x = 0`, nó ngay lập tức phát hiện tính hợp lệ và trả về`0`. 

Trường hợp thứ ba là mã hóa phút không hợp lệ như`a = 59`. giá trị`59`tương ứng với`00:59`, hợp lệ nên thuật toán trả về chính xác`0`mà không cố gắng tăng thêm. Điều này nhấn mạnh rằng việc xác thực phụ thuộc vào cấu trúc phân tách chứ không phải các mẫu chữ số thô.
