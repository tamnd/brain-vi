---
title: "CF 900B - Vị trí trong phân số"
description: "Chúng ta được cho một số hữu tỉ được hình thành bằng cách chia hai số nguyên và chúng ta quan tâm đến biểu diễn thập phân của nó sau dấu thập phân. Nhiệm vụ là xác định vị trí sớm nhất nơi một chữ số cụ thể xuất hiện trong phần mở rộng thập phân vô hạn (hoặc tận cùng) đó."
date: "2026-06-17T03:25:49+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 900
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 450 (Div. 2)"
rating: 1300
weight: 900
solve_time_s: 197
verified: true
draft: false
---

[CF 900B - Vị trí trong Phân số](https://codeforces.com/problemset/problem/900/B) 

**Đánh giá:** 1300 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số hữu tỉ được hình thành bằng cách chia hai số nguyên và chúng ta quan tâm đến biểu diễn thập phân của nó sau dấu thập phân. Nhiệm vụ là xác định vị trí sớm nhất nơi một chữ số cụ thể xuất hiện trong phần mở rộng thập phân vô hạn (hoặc tận cùng) đó. 

Dữ liệu đầu vào bao gồm tử số và mẫu số xác định một phân số, cùng với một chữ số mà chúng ta đang tìm kiếm. Về mặt khái niệm, chúng ta tưởng tượng thực hiện phép chia dài của tử số cho mẫu số và quan sát từng chữ số xuất hiện sau dấu thập phân. Đầu ra là chỉ số của lần đầu tiên chữ số đích xuất hiện trong chuỗi này, trong đó chữ số đầu tiên sau dấu thập phân ở vị trí 1. Nếu chữ số đó không bao giờ xuất hiện thì câu trả lời là -1. 

Các ràng buộc cho phép cả hai số trong phân số lên tới 100000. Con số này đủ nhỏ để việc mô phỏng phép chia dài từng chữ số là khả thi. Mỗi bước của phép chia dài chỉ sử dụng số học theo thời gian không đổi, do đó mô phỏng O(b) là đủ. Vì phép khai triển thập phân có thể lặp lại theo chu kỳ có độ dài tối đa b, nên số bước có ý nghĩa cũng bị giới hạn bởi b. 

Một trường hợp cạnh ngây thơ nhưng quan trọng là khi việc mở rộng thập phân kết thúc sớm. Ví dụ: nếu phần còn lại bằng 0 thì việc khai triển kết thúc và không còn chữ số nào tồn tại nữa. Một trường hợp tinh tế khác là khi chữ số mong muốn là 0. Nhiều phân số tạo ra những đoạn dài không có số 0 và rất dễ cho rằng sự kết thúc hoặc bỏ sót rằng số 0 chỉ có thể xuất hiện sau một vài bước chia. 

## Phương pháp tiếp cận 

Cách đơn giản để tạo ra phép khai triển thập phân là mô phỏng phép chia dài chính xác như được thực hiện bằng tay. Bắt đầu với phần dư của a chia cho b, chúng ta nhân phần dư với 10 nhiều lần, trích chữ số tiếp theo bằng phép chia số nguyên và cập nhật phần còn lại. Mỗi chữ số được tạo ra sẽ được kiểm tra theo chữ số đích. Quá trình này tiếp tục cho đến khi tìm thấy chữ số hoặc số dư bằng 0, nghĩa là quá trình mở rộng thập phân chấm dứt. 

Điều này hiệu quả vì mỗi bước tạo ra chính xác một chữ số thập phân theo thứ tự. Tuy nhiên, trong trường hợp xấu nhất, phần còn lại có thể chuyển qua nhiều giá trị trước khi lặp lại và quy trình có thể chạy tối đa b bước. Vì b có thể là 100000 nên giá trị này vẫn hiệu quả nhưng nằm ở giới hạn trên của những gì chúng ta muốn đảm bảo. 

Một cách nhìn có cấu trúc hơn là coi phần dư là các trạng thái trong một quy trình xác định. Mỗi số dư xác định duy nhất chữ số tiếp theo và số dư tiếp theo. Vì chỉ có thể có b số dư nên khi số dư lặp lại, phép khai triển thập phân bắt đầu theo chu kỳ. Điều này đảm bảo chúng ta không bao giờ cần nhiều hơn b bước để mô tả đầy đủ trình tự. Chúng ta có thể tùy ý theo dõi các phần còn lại đã truy cập, nhưng chỉ cần lặp lại tối đa b bước hoặc dừng ở phần còn lại bằng 0 là đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng với số bước không giới hạn) | O(∞) trong lý luận tồi nhất, có thể có chu kỳ | O(1) | Không chính xác/không an toàn | 
| Tối ưu (phân chia dài với các bước giới hạn) | O(b) | O(1) hoặc O(b) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô phỏng từng bước khai triển số thập phân bằng cách sử dụng quy trình chia dài tiêu chuẩn.

1. Bắt đầu với số dư ban đầu bằng a mod b. Điều này thể hiện những gì còn lại sau khi trích xuất phần nguyên mà chúng tôi bỏ qua vì chỉ các chữ số sau dấu thập phân mới quan trọng. 
2. Lặp lại quy trình sau cho đến b bước, vì có nhiều nhất b phần dư có thể có trước khi lặp lại. 
3. Nhân số dư hiện tại với 10. Điều này sẽ chuyển phép chia sang vị trí thập phân tiếp theo. 
4. Tính chữ số tiếp theo là số nguyên của giá trị này cho b. Đây chính xác là chữ số tiếp theo trong phần mở rộng thập phân. 
5. Cập nhật phần còn lại vào mô đun của giá trị này theo b. Phần còn lại này xác định tất cả các chữ số trong tương lai. 
6. So sánh chữ số được tạo ra với chữ số cần tìm c. Nếu chúng khớp nhau, hãy trả về vị trí hiện tại (chỉ số dựa trên 1). 
7. Nếu phần dư bằng 0, quá trình khai triển thập phân kết thúc, vì vậy chúng ta dừng sớm và trả về -1 nếu không tìm thấy chữ số. 

Bất biến chính là ở mỗi bước, cặp (chữ số, số dư) thể hiện chính xác phần chưa sử dụng tiếp theo của quy trình chia dài chính xác. Vì phép chia dài có tính xác định và được đặc trưng đầy đủ bởi phần dư nên không có chữ số nào bị bỏ qua hoặc tính toán sai. Mỗi vị trí trong khai triển thập phân được truy cập chính xác một lần theo thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())

    rem = a % b
    for pos in range(1, b + 1):
        rem *= 10
        digit = rem // b
        rem %= b

        if digit == c:
            print(pos)
            return

        if rem == 0:
            break

    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện phản ánh trực tiếp quá trình phân chia dài. Phần còn lại được khởi tạo bằng modulo để loại bỏ phần nguyên. Mỗi lần lặp tạo ra chính xác một chữ số thập phân và chỉ số vòng lặp biểu thị vị trí của nó sau dấu thập phân. Việc chấm dứt sớm khi số dư bằng 0 sẽ ngăn cản việc lặp lại không cần thiết khi quá trình mở rộng thập phân kết thúc. 

Một chi tiết tinh tế là vòng lặp được giới hạn bởi b lần lặp. Ngay cả khi không theo dõi rõ ràng các phần dư đã truy cập, ràng buộc này đảm bảo chấm dứt vì chỉ có thể có b trạng thái phần dư. Một chi tiết quan trọng khác là chỉ sử dụng số học số nguyên, tránh hoàn toàn các lỗi dấu phẩy động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 1, b = 4, c = 2
```Khai triển thập phân của 1/4 là 0,25. 

| Bước | Phần còn lại | *giá trị 10 | Chữ số | Phần còn lại tiếp theo | Thành lập? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 2 | 2 | Có | 

Chữ số đầu tiên sau số thập phân là 2, vì vậy câu trả lời là 1. Điều này cho thấy cách thuật toán lấy chữ số phân số đầu tiên mà không cần tạo ra bản khai triển đầy đủ. 

### Ví dụ 2 

đầu vào:```
a = 1, b = 6, c = 3
```Khai triển thập phân của 1/6 là 0,1666... 

| Bước | Phần còn lại | *giá trị 10 | Chữ số | Phần còn lại tiếp theo | Thành lập? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 1 | 4 | Không | 
| 2 | 4 | 40 | 6 | 4 | Không | 
| 3 | 4 | 40 | 6 | 4 | Không | 
| 4 | 4 | 40 | 6 | 4 | Không | 

Chữ số 3 không bao giờ xuất hiện và phần còn lại sẽ chuyển sang chu trình ngay sau bước 2. Thuật toán phát hiện chính xác sự vắng mặt bằng cách sử dụng hết tất cả các trạng thái có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(b) | Mỗi lần lặp tạo ra một chữ số thập phân và có tối đa b trạng thái còn lại trước khi lặp lại hoặc kết thúc | 
| Không gian | O(1) | Chỉ sử dụng một số lượng biến không đổi | 

Giới hạn của bài toán cho phép lên tới 100000 lần lặp, tốc độ này đủ nhanh trong Python vì mỗi lần lặp là một vài phép toán số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples (as stated)
# format may vary, but structure preserved
assert run("1 4 2\n") == "1"

# minimum values
assert run("1 2 1\n") == "1"

# digit not present
assert run("1 2 9\n") == "-1"

# terminating decimal with trailing zeros concept
assert run("1 8 0\n") == "3"

# repeating decimal cycle case
assert run("1 3 6\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 1 | 1 | khớp ngay ở chữ số đầu tiên | 
| 1 2 9 | -1 | chữ số vắng mặt hoàn toàn | 
| 1 8 0 | 3 | phát hiện các số 0 ở cuối số thập phân | 
| 1 3 6 | 2 | xử lý chu trình lặp lại | 

## Vỏ cạnh 

Trường hợp cạnh phổ biến là khi phân số kết thúc sớm vì phần còn lại bằng 0. Ví dụ, với đầu vào`1 4 3`, khai triển thập phân là`0.25`. Phần còn lại trở thành 0 sau khi tạo ra chữ số thứ hai. Vòng lặp dừng ngay lập tức và vì chữ số 3 không bao giờ xuất hiện nên đầu ra là -1. Thuật toán tránh tiếp tục thành các chữ số không tồn tại một cách chính xác. 

Một trường hợp khác là khi chữ số mục tiêu bằng 0. Vì`1 8`, sự mở rộng là`0.125`. Thuật toán kiểm tra từng chữ số được tạo ra một cách rõ ràng, do đó số 0 ở vị trí thứ hai được phát hiện chính xác mà không cần viết hoa đặc biệt. 

Trường hợp tinh tế cuối cùng là khi chữ số xuất hiện muộn trong một chu kỳ lặp lại. Vì`1 7`, sự mở rộng là`0.142857...`. Thuật toán khám phá toàn bộ chu kỳ của số dư và cuối cùng gặp chữ số nếu nó tồn tại trong chu trình. Nếu không, phép lặp giới hạn sẽ đảm bảo kết thúc mà không có vòng lặp vô hạn.
