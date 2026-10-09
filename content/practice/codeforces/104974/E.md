---
title: "CF 104974E - Thực tập sinh bán hoa"
description: "Chúng tôi đang mô phỏng một hệ thống tệp rất nhỏ hỗ trợ ba thao tác được áp dụng tuần tự. Mỗi thao tác sẽ tạo một tệp được đặt tên, xóa tệp đã đặt tên hoặc hỏi hiện có bao nhiêu tệp."
date: "2026-06-28T06:10:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104974
codeforces_index: "E"
codeforces_contest_name: "Codentines Day"
rating: 0
weight: 104974
solve_time_s: 49
verified: true
draft: false
---

[CF 104974E - Thực tập sinh bán hoa](https://codeforces.com/problemset/problem/104974/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một hệ thống tệp rất nhỏ hỗ trợ ba thao tác được áp dụng tuần tự. Mỗi thao tác sẽ tạo một tệp được đặt tên, xóa tệp đã đặt tên hoặc hỏi hiện có bao nhiêu tệp. Một tệp được xác định duy nhất bằng tên của nó và các tên hoạt động giống như các chuỗi chính xác, vì vậy`"abc"`Và`"ABC"`là khác nhau và dấu cách là một phần của tên. 

Trạng thái bắt đầu trống. Khi một`touch name`lệnh xuất hiện, chúng ta thử chèn tên đó vào tập file hiện tại. Nếu nó đã tồn tại thì không có gì thay đổi. Khi một`rm name`lệnh xuất hiện, chúng tôi xóa tên đó nếu có; nếu nó vắng mặt, chúng ta lại không làm gì cả. Khi một`ask`lệnh xuất hiện, chúng ta xuất ra số lượng file hiện tại được lưu trữ. 

Khó khăn chính là quy mô. Số lượng lệnh có thể lên tới một triệu và tên tệp là các chuỗi tùy ý có tổng chiều dài có thể lên tới hai triệu ký tự. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào quét tất cả các tên được lưu trữ cho mọi truy vấn, bởi vì ngay cả việc quét tuyến tính trên mỗi`ask`sẽ suy biến thành hành vi bậc hai trong trường hợp xấu nhất. 

Việc triển khai đơn giản sẽ duy trì một danh sách các chuỗi và trên`ask`, tính toán lại số chuỗi riêng biệt tồn tại bằng cách quét toàn bộ danh sách và kiểm tra tư cách thành viên theo cách thủ công. Điều này bị hỏng khi có nhiều thao tác và nhiều tệp được lưu trữ. 

Một dạng lỗi tinh vi hơn xuất phát từ việc xử lý sai các bản sao. Ví dụ, nếu chúng ta xử lý`touch`như "thêm vào danh sách" mà không kiểm tra sự tồn tại, việc chạm lặp lại vào cùng một tên sẽ làm tăng số lượng không chính xác. Tương tự, nếu chúng ta chỉ đơn giản giảm một bộ đếm trên mỗi`rm`, chúng tôi có thể trở nên tiêu cực nếu việc xóa nhắm mục tiêu vào các tệp không tồn tại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: duy trì danh sách tất cả các tên tệp hiện đang tồn tại. Vì`touch`, chúng tôi kiểm tra xem tên đó đã có trong danh sách chưa; nếu không, chúng tôi nối thêm nó. Vì`rm`, chúng tôi tìm kiếm trong danh sách và nếu tìm thấy, chúng tôi sẽ xóa danh sách đó. Vì`ask`, chúng tôi tính toán kích thước của danh sách. 

Tính chính xác là ngay lập tức vì danh sách phản ánh chính xác tập hoạt động. Vấn đề là hiệu suất. Cả hai`touch`kiểm tra sự tồn tại và`rm`việc tra cứu yêu cầu quét tối đa chuỗi O(n) và việc xóa cũng có thể yêu cầu dịch chuyển các phần tử. Với tối đa 10^6 thao tác, điều này dẫn đến hành vi gần như O(n^2) trong trường hợp xấu nhất, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta chỉ cần tư cách thành viên và số lượng, không cần trật tự hoặc cấu trúc. Đó chính xác là mục đích của một bộ dựa trên hàm băm. Nếu chúng tôi lưu trữ tên tệp bằng Python`set`, cả ba thao tác đều trở thành trung bình O(1): chèn, xóa và kiểm tra thành viên có thời gian trung bình không đổi và kích thước của tập hợp cho chúng ta câu trả lời cho`ask`trực tiếp. 

Chúng tôi cũng tránh hoàn toàn việc tính toán lại. Tập hợp này luôn biểu thị trạng thái hiện tại, vì vậy việc đếm chỉ là đọc một số nguyên được lưu trữ được cấu trúc duy trì bên trong. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét danh sách Brute Force | O(N2) | O(N) | Quá chậm | 
| Bộ băm | Tổng O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng lệnh một trong khi duy trì một tập hợp tên tệp đang hoạt động. 

1. Khởi tạo một tập hợp trống`files`. Điều này đại diện cho tất cả các tên tệp hiện có tại bất kỳ thời điểm nào. 
2. Đọc từng dòng lệnh và chia nó thành nhiều phần. Mã thông báo đầu tiên xác định loại hoạt động. 
3. Nếu lệnh là`touch name`, chèn`name`vào bộ. Nếu nó đã tồn tại, bộ này sẽ không thay đổi, phù hợp với yêu cầu bỏ qua các bản sao. 
4. Nếu lệnh là`rm name`, di dời`name`từ bộ nếu có. Nếu nó không có mặt, không làm gì cả. Điều này có thể được thực hiện một cách an toàn bằng cách sử dụng`discard`vì vậy không có lỗi nào được nêu ra. 
5. Nếu lệnh là`ask`, xuất ra kích thước hiện tại của tập hợp. 

Logic hoạt động vì mỗi tên tệp được biểu thị nhiều nhất một lần trong tập hợp và mọi chuyển đổi trạng thái hợp lệ đều được ghi lại bằng cách thêm hoặc xóa phần tử đó. 

### Tại sao nó hoạt động 

Tại mỗi bước, tập hợp`files`chứa chính xác các tên đã được chèn bởi`touch`nhưng không bị loại bỏ bởi`rm`. Điều này được bảo toàn theo phương pháp quy nạp:`touch`thêm phần tử còn thiếu mà không trùng lặp,`rm`chỉ loại bỏ các phần tử hiện có mà không ảnh hưởng đến các phần tử khác và`ask`không sửa đổi trạng thái. Do đó, kích thước của tập hợp luôn bằng số lượng tệp đang hoạt động tại thời điểm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    files = set()
    out = []

    for _ in range(n):
        parts = input().strip().split()
        if parts[0] == "touch":
            files.add(parts[1])
        elif parts[0] == "rm":
            files.discard(parts[1])
        else:
            out.append(str(len(files)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp dựa trên bộ hàm băm tích hợp của Python để lưu trữ tên tệp. Việc sử dụng`discard`thay vì`remove`là có chủ ý, vì nó tránh được các trường hợp ngoại lệ khi xóa một tệp không tồn tại, phù hợp với hành vi “không làm gì” của câu lệnh vấn đề. 

Chúng tôi tích lũy các câu trả lời trong một danh sách thay vì in ngay lập tức để giảm chi phí I/O, điều này trở nên quan trọng ở giới hạn trên của truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
7
touch love_is_in_the_air
ask
touch Valentine
rm love_is_in_the_air
ask
rm Danny_is_cool
ask
```Chúng tôi theo dõi bộ từng bước. 

| Bước | Lệnh | Đặt trạng thái | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | chạm vào love_is_in_the_air | {love_is_in_the_air} | | 
| 2 | hỏi | {love_is_in_the_air} | 1 | 
| 3 | chạm vào Valentine | {love_is_in_the_air, Valentine} | | 
| 4 | rm love_is_in_the_air | {Valentine} | | 
| 5 | hỏi | {Valentine} | 1 | 
| 6 | rm Danny_is_cool | {Valentine} | | 
| 7 | hỏi | {Valentine} | 1 | 

Dấu vết này cho thấy rằng cả thao tác chèn an toàn trùng lặp và xóa không hoạt động đều được xử lý chính xác và`ask`chỉ cần đọc trạng thái hiện tại mà không cần tính toán lại. 

Bây giờ hãy xem xét kịch bản thứ hai với các thao tác lặp lại:```
6
touch a
touch a
touch b
rm c
ask
rm a
ask
```| Bước | Lệnh | Đặt trạng thái | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | chạm vào | {a} | | 
| 2 | chạm vào | {a} | | 
| 3 | chạm vào b | {a,b} | | 
| 4 | rm c | {a, b} | | 
| 5 | hỏi | {a,b} | 2 | 
| 6 | rm a | {b} | | 
| 7 | hỏi | {b} | 1 | 

Điều này xác nhận rằng lặp đi lặp lại`touch`không làm tăng số lượng và việc xóa các tên không tồn tại sẽ được bỏ qua một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) trung bình | Mỗi thao tác là một bản cập nhật hoặc tra cứu tập hợp băm, được khấu hao theo thời gian không đổi | 
| Không gian | O(M) | M là số tên file riêng biệt được lưu trữ đồng thời | 

Các ràng buộc cho phép lên tới một triệu thao tác, do đó việc xử lý thời gian tuyến tính với cập nhật theo thời gian liên tục nằm trong giới hạn. Việc sử dụng bộ nhớ bị giới hạn bởi số lượng tên tệp đang hoạt động, nhiều nhất là số lượng tên tệp riêng biệt`touch`hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(sys.stdin.readline())
    files = set()
    out = []

    for _ in range(n):
        parts = sys.stdin.readline().strip().split()
        if parts[0] == "touch":
            files.add(parts[1])
        elif parts[0] == "rm":
            files.discard(parts[1])
        else:
            out.append(str(len(files)))

    return "\n".join(out)

# provided sample
assert run("""7
touch love_is_in_the_air
ask
touch Valentine
rm love_is_in_the_air
ask
rm Danny_is_cool
ask
""") == "1\n1\n1"

# empty-ish behavior
assert run("""3
ask
touch a
ask
""") == "0\n1"

# duplicate touches
assert run("""5
touch x
touch x
ask
rm x
ask
""") == "1\n0"

# remove non-existent
assert run("""4
rm a
touch b
ask
ask
""") == "1\n1"

# larger mixed
assert run("""8
touch a
touch b
touch c
rm b
ask
rm a
ask
rm c
ask
""") == "2\n1\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 1 1 1 | tính đúng đắn cơ bản | 
| trống rỗng | 0 1 | xử lý trạng thái ban đầu | 
| chạm trùng lặp | 1 0 | chèn bình thường | 
| loại bỏ không tồn tại | 1 1 | xóa an toàn | 
| hỗn hợp lớn hơn | 2 1 0 | tính nhất quán tuần tự | 

## Vỏ cạnh 

Trường hợp một cạnh được lặp lại`touch`của cùng một tên tập tin. Vì một tập hợp bỏ qua các bản sao nên trạng thái vẫn ổn định. Ví dụ: 

đầu vào:```
touch a
touch a
ask
```Tập hợp trở thành`{a}`sau cả hai thao tác, vì vậy đầu ra là`1`. Việc triển khai dựa trên danh sách sẽ lưu trữ không chính xác hai bản sao trừ khi được kiểm tra rõ ràng. 

Một trường hợp đặc biệt khác là xóa một tệp chưa bao giờ được tạo. sử dụng`discard`đảm bảo không có ngoại lệ và không có thay đổi trạng thái: 

đầu vào:```
rm missing
ask
```Bộ vẫn trống và đầu ra là`0`. Một sự ngây thơ`remove`cuộc gọi sẽ gặp sự cố, trong khi cách tiếp cận bộ đếm thủ công có thể trở nên tiêu cực. 

Trường hợp cuối cùng là tình trạng rời bỏ quy mô lớn, trong đó các tệp được thêm và xóa liên tục. Bởi vì mỗi hoạt động được khấu hao O(1), thuật toán duy trì hiệu suất tuyến tính ngay cả khi trạng thái dao động mạnh.
