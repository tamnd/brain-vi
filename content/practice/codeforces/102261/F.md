---
title: "CF 102261F - \u041f\u043e\u0438\u0441\u043a \u043b\u043e\u043c\u0430\u044e\u0449\u0435\u0433\u043e \u043a\u043e\u043c\u043c\u0438\u0442\u0430"
description: "Chúng tôi có n cam kết được đánh số từ 1 đến n. Có chính xác một cam kết đầu tiên đã phá vỡ các bài kiểm tra. Điều này mang lại cho chuỗi một cấu trúc đơn điệu rất hữu ích: mọi lần xác nhận có số nhỏ hơn m là tốt, trong khi mọi lần xác nhận từ m trở đi là xấu."
date: "2026-08-17T20:43:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102261
codeforces_index: "F"
codeforces_contest_name: "\u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e - \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f (\u042f\u043d\u0434\u0435\u043a\u0441)"
rating: 0
weight: 102261
solve_time_s: 65
verified: true
draft: false
---

[CF 102261F - \u041f\u043e\u0438\u0441\u043a \u043b\u043e\u043c\u0430\u044e\u0449\u0435\u0433\u043e \u043a\u043e\u043c\u043c\u0438\u0442\u0430](https://codeforces.com/problemset/problem/102261/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`cam kết được đánh số từ`1`ĐẾN`n`. Có chính xác một cam kết đầu tiên`m`phá vỡ các bài kiểm tra. Điều này mang lại cho chuỗi một cấu trúc đơn điệu rất hữu ích: mọi cam kết có số nhỏ hơn`m`là tốt, trong khi mọi cam kết từ`m`trở đi là xấu. 

Chương trình không thể kiểm tra trước toàn bộ trình tự. Nó liên lạc với thẩm phán bằng cách in một số cam kết, nhận`1`nếu cam kết đó vượt qua các bài kiểm tra và`0`nếu nó thất bại. Sau khi xác định`m`, nó phải in`! m`và chấm dứt. Mọi truy vấn phải được xóa ngay lập tức vì trọng tài chỉ đưa ra câu trả lời tiếp theo sau khi nhận được truy vấn tương ứng. 

Đầu vào bắt đầu bằng`n`, Ở đâu`1 <= n <= 10^6`. Đầu vào còn lại mang tính tương tác: mỗi phản hồi chỉ xuất hiện sau khi chương trình thực hiện truy vấn. Giới hạn trên của`n`đủ nhỏ để tìm kiếm logarit có thể dễ dàng đủ nhanh, nhưng quét tuyến tính có thể yêu cầu một triệu truy vấn, trong khi giao thức chỉ cho phép 25. Do đó, hạn chế thực sự là số lượng tương tác thay vì thời gian CPU. 

Tính đơn điệu cũng quyết định các trường hợp biên. Nếu như`n = 1`, câu trả lời duy nhất có thể là`m = 1`, do đó chương trình không được cố gắng truy vấn điểm giữa không hợp lệ hoặc di chuyển giới hạn dưới trong quá khứ`n`. Ví dụ, với đầu vào`1`, đầu ra cuối cùng đúng là`! 1`. 

Cam kết đầu tiên có thể là cam kết vi phạm. Ví dụ, nếu`n = 5`và dãy ẩn là`0 0 0 0 0`, câu trả lời là`! 1`. Việc triển khai giả định luôn có ít nhất một cam kết thành công và bắt đầu bằng cách tìm kiếm quá trình chuyển đổi từ`1`ĐẾN`0`sử dụng giới hạn dưới của`2`có thể bỏ lỡ câu trả lời hoàn toàn. 

Cam kết cuối cùng cũng có thể là cam kết vi phạm. Với`n = 5`và phản hồi`1 1 1 1 0`, câu trả lời là`! 5`. Một tìm kiếm nhị phân bất cẩn xử lý một`1`ở điểm giữa có nghĩa là câu trả lời nằm ở`[mid, hi]`thay vì`[mid + 1, hi]`có thể bị kẹt hoặc trả lại một cam kết thành công. 

Ngoài ra còn có sự khác biệt quan trọng giữa câu trả lời truy vấn và câu trả lời cuối cùng. Truy vấn`m`chính nó trở lại`0`, nhưng điều đó không có nghĩa là chúng tôi cần một truy vấn khác để xác minh nó. Khi khoảng tìm kiếm đã thu gọn về một vị trí, thuộc tính đơn điệu đã chứng minh rằng vị trí này là lần xác nhận thất bại đầu tiên. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là truy vấn các cam kết từ trái sang phải. Chúng tôi bắt đầu với cam kết`1`, sau đó`2`và tiếp tục cho đến khi có phản hồi đầu tiên`0`xuất hiện. Cam kết thất bại đầu tiên chính xác là`m`, bởi vì tất cả các lần xác nhận trước đó đều vượt qua và mọi lần xác nhận từ`m`trở đi thất bại. Nếu như`m = n`, phương pháp này cần`n`truy vấn. 

Để tối đa`n = 10^6`, do đó trường hợp xấu nhất là một triệu truy vấn. Giao thức chỉ cho phép 25, do đó quét tuyến tính không chỉ chậm hơn mức cần thiết mà còn không hợp lệ đối với hầu hết các đầu vào có thể có. 

Phương pháp brute-force hoạt động vì phản hồi chứa đủ thông tin để xác định lỗi đầu tiên. Quan sát quan trọng là một truy vấn có thể loại bỏ toàn bộ khoảng thời gian chứ không chỉ xác định vị trí được truy vấn. Giả sử chúng ta truy vấn`mid`. Nếu câu trả lời là`1`, sau đó`mid`được biết là trước lần cam kết thất bại đầu tiên, vì vậy mọi vị trí cho đến và bao gồm`mid`có thể được loại bỏ. Nếu câu trả lời là`0`, sau đó`mid`đã thất bại, do đó vị trí thất bại đầu tiên phải bằng hoặc trước`mid`. 

Đây chính xác là cấu trúc cần thiết cho tìm kiếm nhị phân. Chúng tôi duy trì một khoảng thời gian`[lo, hi]`được đảm bảo chứa`m`. Điểm giữa thành công sẽ thay đổi nó thành`[mid + 1, hi]`, trong khi điểm giữa không thành công sẽ thay đổi nó thành`[lo, mid]`. Mỗi truy vấn xấp xỉ một nửa số câu trả lời có thể có. 

Vì`n <= 10^6`, nhiều nhất`ceil(log2(10^6)) = 20`các truy vấn là cần thiết. Đó là mức thoải mái dưới giới hạn 25. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n) | O(1) | Quá chậm | 
| Tìm kiếm nhị phân | Truy vấn O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt`lo = 1`Và`hi = n`. Bất biến là lần cam kết thất bại đầu tiên chưa biết`m`luôn ở bên trong`[lo, hi]`. 
2. Trong khi`lo < hi`, chọn`mid = (lo + hi) // 2`và in`mid`. Xả ngay đầu ra tiêu chuẩn, sau đó đọc câu trả lời của giám khảo. 
3. Nếu phản hồi là`1`, làm`mid`vượt qua các bài kiểm tra Vì mọi cam kết trước đây`m`vượt qua và mọi cam kết từ`m`trở đi thất bại,`m`phải lớn hơn`mid`. Thay thế khoảng bằng`[mid + 1, hi]`. 
4. Nếu phản hồi là`0`, làm`mid`thất bại. Lần cam kết thất bại đầu tiên không thể xảy ra sau`mid`, bởi vì`mid`bản thân nó đã thất bại rồi. Thay thế khoảng bằng`[lo, mid]`. 
5. Khi nào`lo == hi`, khoảng thời gian chứa chính xác một cam kết có thể. In`! lo`và chấm dứt. Không cần truy vấn bổ sung vì bất biến chứng minh rằng vị trí này là`m`. 

Lý do sử dụng`lo < hi`thay vì truy vấn liên tục cho đến khi một phản hồi cụ thể xuất hiện thì chính khoảng thời gian đó mang bằng chứng về tính đúng đắn. Một khi các điểm cuối của nó trùng nhau thì không còn sự không chắc chắn nào nữa. 

### Tại sao nó hoạt động 

Tại mọi điểm,`[lo, hi]`chứa cam kết thất bại thực sự đầu tiên. Ban đầu điều này đúng vì`m`được đảm bảo nằm giữa`1`Và`n`. Nếu điểm giữa trở lại`1`, sự đơn điệu cho chúng ta biết rằng`m > mid`, do đó loại bỏ`[lo, mid]`không thể loại bỏ`m`. Nếu điểm giữa trở lại`0`, sự đơn điệu cho chúng ta biết rằng`m <= mid`, do đó loại bỏ`[mid + 1, hi]`không thể loại bỏ`m`. Do đó, bất biến tồn tại trong mọi truy vấn. 

Mỗi lần lặp lại làm giảm nghiêm ngặt kích thước của khoảng trong khi vẫn bảo toàn`m`. Sau cùng`lo`Và`hi`trở nên bình đẳng. Vì vị trí khả dĩ duy nhất còn lại là vị trí đúng`m`, in vị trí đó là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    lo = 1
    hi = n

    while lo < hi:
        mid = (lo + hi) // 2

        print(mid, flush=True)
        response = input().strip()

        if response == "1":
            lo = mid + 1
        else:
            hi = mid

    print("!", lo, flush=True)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên đọc`n`và khởi tạo khoảng thời gian tìm kiếm cho tất cả các lần xác nhận có thể. Khoảng thời gian sử dụng lập chỉ mục dựa trên một vì các số cam kết trong phạm vi giao thức từ`1`bởi vì`n`. 

Điểm giữa được tính như`(lo + hi) // 2`. Số nguyên Python không bị tràn, mặc dù biểu thức tương tự ở đây cũng an toàn trong các ngôn ngữ có kiểu số nguyên đủ rộng vì`n`chỉ là`10^6`. 

Truy vấn được in với`flush=True`. Điều này rất cần thiết trong một vấn đề tương tác. Nếu không xóa, Python có thể giữ truy vấn trong bộ đệm đầu ra của nó, khiến người đánh giá phải chờ trong khi chương trình chờ phản hồi. 

Một phản hồi của`"1"`di chuyển`lo`ĐẾN`mid + 1`, bởi vì lần cam kết thất bại đầu tiên phải hoàn toàn ở bên phải. Một phản hồi của`"0"`di chuyển`hi`ĐẾN`mid`, bởi vì`mid`chính nó có thể là lần cam kết thất bại đầu tiên. sử dụng`hi = mid - 1`đây sẽ là một lỗi riêng lẻ và có thể loại bỏ câu trả lời đúng. 

Vòng lặp dừng khi`lo == hi`. Tại thời điểm đó, câu trả lời đã được biết nên chương trình sẽ in ra kết quả cần thiết`!`tiền tố và xóa lại trước khi kết thúc. 

Không có nhiều trường hợp thử nghiệm. Mỗi lệnh gọi của chương trình tương tác với một chuỗi xác nhận ẩn. 

## Ví dụ đã hoạt động 

Tuyên bố chính thức được cung cấp không chứa bản ghi đầu vào và đầu ra mẫu có thể sử dụng được. Hiển thị`! 5`chỉ là một phần của ví dụ đầu ra tương tác, vì vậy các dấu vết sau đây sử dụng hai trường hợp ẩn cụ thể để minh họa giao thức. 

### Ví dụ 1 

hãy để`n = 10`và giả sử cam kết`7`là lần cam kết thất bại đầu tiên. Do đó, chuỗi phản hồi ẩn là`1 1 1 1 1 1 0 0 0 0`. 

| Bước |`lo`|`hi`|`mid`| Phản hồi | Khoảng thời gian mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 5 | 1 |`[6, 10]`| 
| 2 | 6 | 10 | 8 | 0 |`[6, 8]`| 
| 3 | 6 | 8 | 7 | 0 |`[6, 7]`| 
| 4 | 6 | 7 | 6 | 1 |`[7, 7]`| 

Truy vấn đầu tiên chứng minh rằng cam kết`1`bởi vì`5`không phải là câu trả lời. Truy vấn thứ hai chứng minh rằng câu trả lời nhiều nhất là`8`, và điều thứ ba chứng tỏ rằng nhiều nhất là`7`. Cuối cùng là truy vấn`6`thành công, vì vậy thất bại đầu tiên phải là`7`. Thuật toán in`! 7`. 

### Ví dụ 2 

hãy để`n = 8`và giả sử lần cam kết thất bại đầu tiên là`1`. Mọi truy vấn đều trả về`0`. 

| Bước |`lo`|`hi`|`mid`| Phản hồi | Khoảng thời gian mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 8 | 4 | 0 |`[1, 4]`| 
| 2 | 1 | 4 | 2 | 0 |`[1, 2]`| 
| 3 | 1 | 2 | 1 | 0 |`[1, 1]`| 

Việc tìm kiếm không bao giờ cho rằng có một cam kết thành công tồn tại. Mỗi phản hồi không thành công chỉ đơn giản là di chuyển giới hạn trên sang trái. Khoảng thời gian cuối cùng trở thành`[1, 1]`, do đó thuật toán in`! 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(log n) | Mỗi truy vấn sẽ giảm khoảng một nửa khoảng thời gian ứng viên | 
| Không gian | O(1) | Chỉ có hai điểm cuối khoảng và một điểm giữa được lưu trữ | 

Vì`n <= 10^6`, tìm kiếm nhị phân yêu cầu tối đa 20 truy vấn, vì`2^20 = 1,048,576`. Đây là dưới giới hạn cho phép là 25 và chương trình sử dụng bộ nhớ bổ sung liên tục. 

Thời gian chạy thực tế bị chi phối bởi các truy vấn tương tác thay vì tính toán Python. Số lượng phép tính số học cục bộ rất nhỏ và thuật toán vẫn nằm trong giới hạn bộ nhớ đã nêu là 256 MB. 

## Trường hợp thử nghiệm 

Vì đây là sự cố tương tác nên luồng đầu vào chính thức không thể được sao chép bằng phương pháp thông thường`run(input_string)`người giúp đỡ. Thẩm phán không cung cấp trước tất cả các câu trả lời. Đối với thử nghiệm ngoại tuyến, phần khai thác sau đây mô phỏng việc đánh giá bằng cách chọn một cam kết thất bại đầu tiên bị ẩn và cung cấp các câu trả lời tương ứng cho cùng một logic tìm kiếm nhị phân.```python
import sys
import io

def simulated_solution(n: int, first_bad: int):
    lo = 1
    hi = n
    queries = []

    while lo < hi:
        mid = (lo + hi) // 2
        queries.append(mid)

        response = "1" if mid < first_bad else "0"

        if response == "1":
            lo = mid + 1
        else:
            hi = mid

    return queries, f"! {lo}"

def run(inp: str) -> str:
    data = inp.split()
    n = int(data[0])
    first_bad = int(data[1])

    _, answer = simulated_solution(n, first_bad)
    return answer

# The statement's displayed sample is incomplete because the original task is interactive.
# These tests use explicit hidden answers for offline verification.

assert run("1 1") == "! 1", "minimum n"

assert run("10 1") == "! 1", "first commit is broken"

assert run("10 10") == "! 10", "last commit is broken"

assert run("10 5") == "! 5", "middle transition"

assert run("1000000 999999") == "! 999999", "maximum n and near-right boundary"

assert run("1000000 1") == "! 1", "maximum n and left boundary"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`! 1`| Đầu vào có kích thước tối thiểu và câu trả lời duy nhất có thể | 
|`10 1`|`! 1`| Cam kết đầu tiên đã bị hỏng | 
|`10 10`|`! 10`| Cam kết cuối cùng là cam kết đầu tiên bị hỏng | 
|`10 5`|`! 5`| Chuyển tiếp bình thường ở giữa | 
|`1000000 999999`|`! 999999`| Tối đa`n`và một sự chuyển tiếp gần ranh giới bên phải | 
|`1000000 1`|`! 1`| Tối đa`n`và sự chuyển tiếp ở ranh giới bên trái | 

Trình mô phỏng sử dụng các cập nhật theo khoảng thời gian giống như giải pháp tương tác. Giá trị thứ hai của nó đại diện cho ẩn`m`, điều mà thẩm phán thực sự không bao giờ tiết lộ trực tiếp. Để có một bài nộp có tính tương tác thực sự,`simulated_solution`chức năng này không được sử dụng vì các câu trả lời phải đến từ giám khảo sau mỗi truy vấn được xóa. 

## Vỏ cạnh 

Khi nào`n = 1`, khoảng ban đầu là`[1, 1]`, do đó vòng lặp bị bỏ qua và chương trình in ngay lập tức`! 1`. Đối với đầu vào bê tông`1`, không có câu trả lời thay thế hợp lệ vì cam kết duy nhất phải là cam kết thất bại đầu tiên. 

Khi cam kết đầu tiên bị hỏng, hãy xem xét`n = 8`Và`m = 1`. Truy vấn đầu tiên là`4`, trả về`0`, thay đổi khoảng thời gian thành`[1, 4]`. Truy vấn`2`cũng trở lại`0`, cho`[1, 2]`, và truy vấn`1`trả lại`0`, cho`[1, 1]`. Câu trả lời cuối cùng là`! 1`. Không cần giả định về một cam kết thành công trước đó. 

Khi cam kết cuối cùng bị hỏng, hãy xem xét`n = 8`Và`m = 8`. Truy vấn đầu tiên là`4`, trả về`1`, do đó khoảng trở thành`[5, 8]`. Truy vấn`6`trả lại`1`, sản xuất`[7, 8]`, và truy vấn`7`trả lại`1`, sản xuất`[8, 8]`. Chương trình in`! 8`. bản cập nhật`lo = mid + 1`là điều cho phép vị trí cuối cùng vẫn là ứng cử viên. 

Lỗi từng lỗi phổ biến nhất xảy ra khi cam kết được truy vấn không thành công. Với`n = 5`Và`m = 3`, truy vấn`3`trả lại`0`. Khoảng thời gian mới chính xác là`[1, 3]`, không`[1, 2]`, bởi vì cam kết`3`bản thân nó có thể là thất bại đầu tiên. Nhiệm vụ`hi = mid`bảo toàn ứng cử viên đó. 

Một trường hợp ranh giới khác là khi quá trình chuyển đổi xảy ra ngay sau điểm giữa. Với`n = 10`Và`m = 6`, truy vấn`5`trả lại`1`, vì vậy câu trả lời phải có trong`[6, 10]`. Xóa cam kết`5`và mọi thứ trước đó đều an toàn vì tất cả những cam kết đó đều được biết là đã vượt qua. Nhiệm vụ`lo = mid + 1`nắm bắt chính xác lý do này. 

Cuối cùng, bản thân giới hạn truy vấn là một phần của tính chính xác. Vì`n = 10^6`, tìm kiếm nhị phân cần tối đa 20 truy vấn, trong khi mức tối đa được phép là 25. Không thể sửa chữa một phương pháp tạo một truy vấn cho mỗi lần xác nhận bằng một tối ưu hóa nhỏ, vì số lượng truy vấn trong trường hợp xấu nhất của nó về cơ bản là tuyến tính. Việc giảm logarit của khoảng ứng cử viên là lý do chính khiến giải pháp đáp ứng giao thức tương tác.
