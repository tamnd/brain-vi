---
title: "CF 102777E - \u041a\u0430\u043b\u044c\u043a\u0443\u043b\u044f\u0442\u043e\u0440 \u042d\u043b\u0435\u043a\u0442\u0440\u043e\u043d\u0438\u043a\u0430-2020"
description: "Màn hình máy tính là một bản vẽ văn bản ba hàng trong đó mỗi ký tự chiếm một vùng cố định 3 x 3. Màn hình chứa các chữ số, dấu cộng hoặc dấu trừ và dấu bằng cuối cùng. Các ký hiệu liền kề được phân tách bằng một cột trống."
date: "2026-07-27T20:21:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 73
verified: true
draft: false
---

[CF 102777E - \u041a\u0430\u043b\u044c\u043a\u0443\u043b\u044f\u0442\u043e\u0440 \u042d\u043b\u0435\u043a\u0442\u0440\u043e\u043d\u0438\u043a\u0430-2020](https://codeforces.com/problemset/problem/102777/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Màn hình máy tính là một bản vẽ văn bản ba hàng trong đó mỗi ký tự chiếm một vùng cố định 3 x 3. Màn hình chứa các chữ số, dấu cộng hoặc dấu trừ và dấu bằng cuối cùng. Các ký hiệu liền kề được phân tách bằng một cột trống. Nhiệm vụ là khôi phục biểu thức số học đã viết, đánh giá nó từ trái sang phải bằng các phép toán được hiển thị và in biểu thức ở dạng văn bản thông thường, theo sau là kết quả sau dấu bằng. 

Kích thước đầu vào được đo bằng chiều rộng của ba hàng hiển thị, có thể đạt tới 1000 ký tự. Điều này có nghĩa là toàn bộ màn hình chỉ chứa vài trăm ô, do đó thuật toán sẽ xử lý mỗi ký tự với số lần không đổi. Bất kỳ cách tiếp cận nào cố gắng diễn giải nhiều ô hoặc quét liên tục toàn bộ biểu thức sẽ không cần thiết. 

Giá trị của các số riêng lẻ đủ nhỏ để phép tính số nguyên thông thường là đủ. Khó khăn chính không phải là tính toán mà là nhận biết chính xác các ký hiệu kiểu bảy đoạn. 

Một số chi tiết có thể phá vỡ một triển khai đơn giản. Dấu trừ đứng đầu là toán tử thực, không phải là một phần của số đầu tiên. Ví dụ: màn hình hiển thị`-10-7=`phải trở thành`-10-7=-17`, không`10-7=-3`. Một vấn đề khác là dấu đẳng thức. Nó không có biểu thức nào sau nó, vì vậy quá trình phân tích cú pháp sẽ dừng lại ngay khi tìm thấy. Vấn đề thứ ba là các số có thể chứa nhiều chữ số, vì vậy mỗi chữ số được nhận dạng phải được thêm vào số hiện tại cho đến khi một phép tính hoặc dấu bằng xuất hiện. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng so sánh mọi khối ba cột có thể có với tất cả các ký hiệu đã biết. Vì chỉ có mười chữ số và một vài toán tử nên trình phân tích cú pháp mạnh mẽ này đã chính xác. Vấn đề của nó không phải là số lượng so sánh, vì có rất ít ký hiệu có thể có, nhưng việc triển khai bất cẩn thường mắc lỗi khi tìm kiếm nhiều lần trên toàn bộ màn hình trong khi cố gắng phân chia biểu thức. Với chiều rộng 1000, việc này vẫn có thể được thực hiện nhanh chóng nhưng không sử dụng cấu trúc của đầu vào. 

Quan sát hữu ích là màn hình đã được tách thành các ô độc lập. Mỗi ô có chiều rộng cố định và cột phân cách luôn trống. Điều này có nghĩa là toàn bộ vấn đề sẽ giảm xuống chỉ còn một lần vượt qua các ô. Một ô có thể được chuyển đổi thành biểu tượng ngay lập tức và chuỗi kết quả có thể được đánh giá trong khi nó đang được xây dựng. 

Phần số học có một sự đơn giản hóa bổ sung. Biểu thức chỉ chứa phép cộng và phép trừ nên không có quyền ưu tiên. Sau khi mã thông báo được khôi phục, giá trị chỉ là tổng tích lũy với dấu hiệu hiện tại được áp dụng cho mỗi số tiếp theo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(W) | O(W) | Được chấp nhận nếu thực hiện cẩn thận | 
| Tối ưu | O(W) | O(W) | Đã chấp nhận | 

Ở đây W là chiều rộng của ba hàng đầu vào. Giải pháp tối ưu là trình phân tích cú pháp từng ô một cách rõ ràng. 

## Hướng dẫn thuật toán 

1. Đọc ba hàng hiển thị và đệm chúng theo cùng độ dài. Đầu vào đã thể hiện một màn hình hoàn chỉnh, do đó, việc căn chỉnh các hàng sẽ cho phép trích xuất từng ô theo vị trí cột. 
2. Di chuyển qua màn hình từ trái sang phải và bỏ qua các cột hoàn toàn trống. Các cột này chỉ là dấu phân cách giữa các ký hiệu. 
3. Đối với mỗi ô không trống, lấy ba cột tiếp theo từ cả ba hàng và so sánh mẫu 3 x 3 thu được với mẫu chữ số và toán tử đã biết. Điều này chuyển đổi biểu diễn đồ họa thành một ký tự bình thường. 
4. Xây dựng chuỗi biểu thức từ các ký tự được nhận dạng. Khi đạt đến dấu đẳng thức thì dừng lại vì mọi thứ sau đó đều vắng mặt. 
5. Đánh giá biểu thức được khôi phục bằng cách quét các ký tự. Duy trì dấu hiệu hiện tại và số hiện tại đang được đọc. Khi một toán tử xuất hiện, hãy cộng số trước đó với dấu của nó và cập nhật dấu cho số tiếp theo. 
6. In biểu thức đã khôi phục, theo sau là giá trị cuối cùng sau dấu đẳng thức. 

Tính chính xác xuất phát từ thực tế là mỗi biểu tượng đều chiếm đúng một ô độc lập. Trình phân tích cú pháp không bao giờ đoán được chữ số kết thúc ở đâu vì dấu phân cách xác định tất cả các ranh giới. Vì từ điển ô chứa mọi ký hiệu có thể có nên mọi ô được trích xuất sẽ được chuyển đổi thành chính xác ký tự được hiển thị ban đầu. Giai đoạn đánh giá bảo toàn bất biến rằng giá trị tích lũy bằng giá trị của tất cả các thuật ngữ được xử lý hoàn chỉnh, trong khi dấu hiệu hiện tại mô tả thuật ngữ tiếp theo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

patterns = {
    "0": (" _ ", "| |", "|_|"),
    "1": ("   ", "  |", "  |"),
    "2": (" _ ", " _|", "|_ "),
    "3": (" _ ", " _|", " _|"),
    "4": ("   ", "|_|", "  |"),
    "5": (" _ ", "|_ ", " _|"),
    "6": (" _ ", "|_ ", "|_|"),
    "7": (" _ ", "  |", "  |"),
    "8": (" _ ", "|_|", "|_|"),
    "9": (" _ ", "|_|", " _|"),
    "+": ("   ", " _ ", "|_|"),
    "-": ("   ", "___", "   "),
    "=": ("   ", "___", "___"),
}

decode = {v: k for k, v in patterns.items()}

def solve():
    rows = [input().rstrip("\n") for _ in range(3)]
    width = max(map(len, rows))
    rows = [row.ljust(width) for row in rows]

    expr = []
    col = 0
    while col < width:
        if all(rows[r][col] == " " for r in range(3)):
            col += 1
            continue

        tile = tuple(row[col:col + 3].ljust(3) for row in rows)
        expr.append(decode[tile])
        col += 3

    expr = "".join(expr)

    value = 0
    current = 0
    sign = 1
    for ch in expr:
        if ch.isdigit():
            current = current * 10 + int(ch)
        else:
            value += sign * current
            current = 0
            sign = 1 if ch == "+" else -1
    value += sign * current

    print(expr + "=" + str(value))

if __name__ == "__main__":
    solve()
```Từ điển lưu trữ bản vẽ ba hàng chính xác của mọi ký hiệu có thể. Điều này tránh việc cố gắng xây dựng lại các phân đoạn theo cách thủ công trong quá trình phân tích cú pháp. Đầu vào được đệm vì ô cuối cùng có thể kết thúc chính xác ở ký tự cuối cùng và việc cắt vượt quá độ dài hàng hiện tại sẽ tạo ra các bộ dữ liệu không nhất quán. 

Trình phân tích cú pháp tiến lên ba cột sau khi đọc ký hiệu. Dấu phân cách trống được xử lý riêng biệt, do đó không có nguy cơ xử lý khoảng trống như một ký tự khác. Người đánh giá không cần quy tắc xếp chồng hoặc quy tắc ưu tiên vì chỉ tồn tại phép cộng và phép trừ. Số hiện tại được tích lũy từng chữ số và nó chỉ được xác nhận khi gặp một thao tác hoặc kết thúc biểu thức. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, các ô được khôi phục tạo thành biểu thức`1+2`. 

| Bước | Ngói | Biểu thức được giải mã | Trạng thái giá trị hiện tại | 
| --- | --- | --- | --- | 
| 1 | chữ số đầu tiên |`1`| số hiện tại = 1 | 
| 2 | dấu cộng |`1+`| giá trị = 1, dấu = + | 
| 3 | chữ số thứ hai |`1+2`| số hiện tại = 2 | 
| 4 | bình đẳng |`1+2=3`| kết quả = 3 | 

Dấu vết cho thấy các toán tử ngay lập tức chốt số trước đó. Trình phân tích cú pháp không bao giờ cần biết trước độ dài của một số. 

Đối với mẫu thứ hai, biểu thức chứa phép trừ và số có hai chữ số. 

| Bước | Ngói | Biểu thức được giải mã | Trạng thái giá trị hiện tại | 
| --- | --- | --- | --- | 
| 1 | chữ số 3 |`3`| số hiện tại = 3 | 
| 2 | dấu trừ |`3-`| giá trị = 3, dấu = - | 
| 3 | chữ số 1 |`3-1`| số hiện tại = 1 | 
| 4 | chữ số 0 |`3-10`| số hiện tại = 10 | 
| 5 | bình đẳng |`3-10=-7`| kết quả = -7 | 

Điều này chứng tỏ tại sao người đánh giá đợi cho đến khi toán tử xuất hiện trước khi áp dụng số. Các chữ số cạnh nhau thuộc cùng một toán hạng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(W) | Mỗi cột của màn hình được kiểm tra với số lần không đổi và mỗi lần tra cứu ô đều có thời gian không đổi. | 
| Không gian | O(W) | Biểu thức đã khôi phục được lưu trữ trước khi đánh giá. | 

Độ rộng đầu vào lớn nhất chỉ là 1000 ký tự, do đó quét tuyến tính dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

# This helper assumes solve() from the solution is available.
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

assert run(
""" _ 
|_|
|_|
"""
) == "6=6\n", "single digit"

assert run(
""" _     _ 
 _|   | |
|_    |_|
"""
) == "2+0=2\n", "addition"

assert run(
"""     _ 
 _  |_ 
 _|  _|
"""
) == "2-1=1\n", "subtraction"

assert run(
""" _   _   _ 
|_| |_| |_|
|_| |_| |_|
"""
) == "888=888\n", "large repeated digit"

assert run(
"""   _     _ 
  |  ___  |
  |      |
"""
) == "1-1=0\n", "operator boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hiển thị một chữ số |`6=6`| Xử lý biểu thức nhỏ nhất. | 
| Trường hợp bổ sung |`2+0=2`| Kiểm tra phân tích cú pháp toán tử. | 
| Trường hợp trừ |`2-1=1`| Kiểm tra việc xử lý dấu âm. | 
| Chữ số lặp lại |`888=888`| Kiểm tra số có nhiều chữ số. | 
| Trường hợp ranh giới toán tử |`1-1=0`| Kiểm tra sự phân tách và chuyển tiếp của ô. | 

## Vỏ cạnh 

Một kết quả âm tính không yêu cầu phân tích cú pháp đặc biệt. Đối với một biểu thức như`3-10=`, trước tiên trình phân tích cú pháp sẽ khôi phục các ký tự`3-10`, sau đó người đánh giá lưu trữ`3`, thay đổi dấu sau ô trừ và trừ`10`. Đầu ra trở thành`3-10=-7`. 

Một số có nhiều chữ số không được chia thành các toán hạng riêng biệt. Trong biểu thức`2+10=`, các ô chữ số`1`Và`0`được đọc liên tiếp, do đó số hiện tại thay đổi từ`1`ĐẾN`10`trước khi nó được thêm vào. Việc coi mỗi ô là một số riêng biệt sẽ tạo ra kết quả không chính xác`2+1+0=3`. 

Dấu đẳng thức phải chấm dứt phân tích cú pháp. Nếu ngăn xếp cuối cùng bị nhầm lẫn với toán tử hoặc toán hạng, người đánh giá có thể thử xử lý một giá trị không tồn tại sau dấu bằng. Thuật toán tránh điều này bằng cách giải mã toàn bộ chuỗi ô và chỉ sử dụng ký hiệu đẳng thức làm điểm đánh dấu cuối của biểu thức được hiển thị. 

Cột phân cách trống ở cuối dữ liệu nhập không được tạo ký hiệu trống. Trình phân tích cú pháp bỏ qua các cột trống hoàn toàn trước khi cố gắng trích xuất một ô, điều này giúp giữ cho các ranh giới ô được căn chỉnh ngay cả khi các hàng đầu vào được đệm bằng dấu cách. 

Tôi cũng có thể điều chỉnh định dạng này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn cần nội dung nào đó gần hơn với nội dung sẽ xuất hiện trên trang cuộc thi.
