---
title: "CF 106169B - Nỗi nhớ"
description: "Chương trình đang được phân tích được viết bằng Scratch và bao gồm các lệnh đơn giản để đọc các giá trị, lưu trữ chúng trong các biến, tính toán các giá trị mới và in kết quả. Trong quá trình thử nghiệm ban đầu, các lập trình viên đã sử dụng một đầu ra gỡ lỗi bổ sung trước mỗi thao tác đầu vào."
date: "2026-06-25T11:07:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106169
codeforces_index: "B"
codeforces_contest_name: "2025-2026 ICPC NERC, Kyrgyzstan Regional Contest"
rating: 0
weight: 106169
solve_time_s: 37
verified: true
draft: false
---

[CF 106169B - Nỗi nhớ](https://codeforces.com/problemset/problem/106169/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chương trình đang được phân tích được viết bằng Scratch và bao gồm các lệnh đơn giản để đọc các giá trị, lưu trữ chúng trong các biến, tính toán các giá trị mới và in kết quả. Trong quá trình thử nghiệm ban đầu, các lập trình viên đã sử dụng một đầu ra gỡ lỗi bổ sung trước mỗi thao tác đầu vào. Đầu ra bổ sung đó chỉ đơn giản là tên của biến sắp nhận đầu vào, giúp họ hiểu giá trị mà chương trình đang chờ đợi. 

Nhiệm vụ là tính toán số lượng ký tự được in bởi các đầu ra gỡ lỗi bổ sung này. Phiên bản cuối cùng được gửi của chương trình sẽ không chứa các kết quả đầu ra đó, do đó chỉ có tên biến được kết nối với các hoạt động đầu vào mới góp phần đưa ra câu trả lời. 

Đầu vào chứa số dòng chương trình theo sau là các lệnh. Trong số tất cả các lệnh có thể, chỉ có những dòng lưu trữ giá trị tạm thời đặc biệt`answer`thành một biến đại diện cho một hoạt động đầu vào. Đối với mỗi dòng như vậy, độ dài của tên biến đích phải được thêm vào kết quả. 

Số lượng dòng có thể lên tới 100000, vì vậy giải pháp phải kiểm tra từng dòng một số lần không đổi. Cách tiếp cận mô phỏng chương trình Scratch hoặc cố gắng đánh giá mọi giá trị biến sẽ thực hiện những công việc không cần thiết. Quét tuyến tính là đủ vì câu trả lời chỉ phụ thuộc vào tên của các biến nhận đầu vào. 

Một lỗi phổ biến là đếm mọi tên biến xuất hiện trong chương trình. Ví dụ:```
6
Ask read_token and wait
Set first to answer
Set result to first + second
Say result
Ask read_token and wait
Set second to answer
```Đầu ra đúng là:```
11
```Chỉ một`first`Và`second`được sử dụng làm đầu vào, đóng góp`5 + 6 = 11`. Đếm`result`sẽ thêm sai các ký tự từ một biến chỉ được tính toán. 

Một trường hợp cạnh khác được lặp lại đầu vào vào cùng một biến:```
8
Ask read_token and wait
Set a to answer
Ask read_token and wait
Set a to answer
Set b to a + a
Say b
Ask read_token and wait
Set c to answer
```Đầu ra đúng là:```
3
```Biến`a`đóng góp hai lần và`c`đóng góp một lần nên tổng cộng là`1 + 1 + 1 = 3`. Việc triển khai bất cẩn mà đếm các tên biến duy nhất sẽ trả về`2`, điều đó không đúng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là hiểu mọi lệnh trong chương trình Scratch, theo dõi các biến và mô phỏng quá trình thực thi. Điều này sẽ đúng vì nó tuân theo hành vi chính xác của chương trình gốc. Tuy nhiên, nó giải quyết được một vấn đề khó khăn hơn nhiều so với mức cần thiết. Các giá trị được lưu trữ trong các biến, phép tính số học và kết quả được in ra không bao giờ ảnh hưởng đến câu trả lời. Nếu mọi thao tác đều được mô phỏng thì việc triển khai sẽ tốn thời gian để duy trì những thông tin không liên quan. 

Quan sát quan trọng là đầu ra bổ sung chỉ xảy ra ngay trước đầu vào. Câu lệnh đảm bảo rằng mọi yêu cầu đầu vào đều được ghép nối với chính xác một phép gán từ`answer`và mọi phép gán như vậy đều tuân theo một yêu cầu đầu vào. Điều này có nghĩa là mỗi dòng của biểu mẫu`Set name to answer`trực tiếp tiết lộ một tên biến đã được in trong quá trình thử nghiệm. 

Vấn đề được giảm xuống để xử lý chuỗi. Chúng ta chỉ cần nhận biết các dòng gán đó và cộng độ dài của tên biến xuất hiện sau`Set`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) với công việc mô phỏng không cần thiết | O(số biến) | Đúng nhưng quá phức tạp | 
| Tối ưu | O(n) | O(1) thêm không gian | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số dòng trong chương trình Scratch và xử lý từng lệnh một. Vì mỗi dòng có thể được kiểm tra độc lập nên không cần phải lưu trữ toàn bộ chương trình. 
2. Với mỗi dòng biểu thị`Set name to answer`, trích xuất`name`và thêm độ dài của nó vào câu trả lời. Đây là loại lệnh duy nhất tương ứng với đầu ra thử nghiệm cục bộ bổ sung. 
3. Bỏ qua tất cả các lệnh khác vì chúng thao tác các giá trị bên trong hoặc hiển thị đầu ra chương trình cuối cùng. Họ không tạo thêm các ký tự gỡ lỗi. 
4. In tổng số tích lũy. 

Tại sao nó hoạt động: 

Bất biến được duy trì trong quá trình quét là sau khi xử lý bất kỳ tiền tố nào của chương trình, câu trả lời được lưu trữ bằng số ký tự phụ được tạo ra bởi tất cả các thao tác nhập bên trong tiền tố đó. Một dòng chỉ đóng góp khi nó lưu trữ`answer`thành một biến và trong trường hợp đó, đầu ra thử nghiệm cục bộ sẽ chính xác là tên biến đó. Mỗi dòng như vậy được tính một lần và không dòng nào khác có thể đóng góp. Sau khi quét hoàn tất, bất biến sẽ đưa ra tổng số cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    ans = 0

    for _ in range(n):
        line = input().strip()
        if line.startswith("Set ") and line.endswith(" to answer"):
            name = line[4:line.index(" to answer")]
            ans += len(name)

    print(ans)

if __name__ == "__main__":
    solve()
```Mã đọc mỗi lệnh Scratch chính xác một lần. Điều kiện kiểm tra cả hai phần xác định nhiệm vụ đầu vào: lệnh phải bắt đầu bằng`Set`và giá trị được gán phải là từ dành riêng`answer`. 

Tên biến nằm giữa tiền tố cố định`Set `và hậu tố` to answer`. Việc sử dụng vị trí của hậu tố sẽ tránh phụ thuộc vào độ dài thay đổi và xử lý chính xác tất cả các tên hợp lệ. 

Việc triển khai không lưu trữ các lệnh hoặc biến vì không cần thông tin từ các dòng trước đó. Câu trả lời số nguyên có thể đạt tới`10^9`, Python xử lý mà không lo tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
Ask read_token and wait
Set first to answer
Ask read_token and wait
Set second to answer
Set result to first + second
Say result
```| Bước | Lệnh | Biến đầu vào được công nhận | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | Hỏi read_token và chờ đợi | không | 0 | 
| 2 | Đặt câu trả lời đầu tiên | đầu tiên | 5 | 
| 3 | Hỏi read_token và chờ đợi | không | 5 | 
| 4 | Đặt giây để trả lời | thứ hai | 11 | 
| 5 | Đặt kết quả thành thứ nhất + thứ hai | không | 11 | 
| 6 | Nói kết quả | không | 11 | 

Dấu vết cho thấy chỉ những biến nhận được dữ liệu đầu vào của người dùng mới quan trọng. Các biến được tính toán như`result`bị bỏ qua. 

### Ví dụ 2 

đầu vào:```
15
Ask read_token and wait
Set a to answer
Ask read_token and wait
Set bb to answer
Set ccc to a * bb
Set dddd to a / bb
Set dddd to ccc + dddd
Say dddd
Ask read_token and wait
Set bb to answer
Ask read_token and wait
Set ccc to answer
Set x to bb * dddd
Set x to ccc - x
Say x
```| Bước | Lệnh | Biến đầu vào được công nhận | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| 1 | Hỏi read_token và chờ đợi | không | 0 | 
| 2 | Đặt a để trả lời | một | 1 | 
| 3 | Hỏi read_token và chờ đợi | không | 1 | 
| 4 | Đặt bb để trả lời | bb | 3 | 
| 5 | Đặt ccc thành a * bb | không | 3 | 
| 6 | Đặt dddd thành a /bb | không | 3 | 
| 7 | Đặt dddd thành ccc + dddd | không | 3 | 
| 8 | Nói dddd | không | 3 | 
| 9 | Hỏi read_token và chờ đợi | không | 3 | 
| 10 | Đặt bb để trả lời | bb | 5 | 
| 11 | Hỏi read_token và chờ đợi | không | 5 | 
| 12 | Đặt ccc để trả lời | ccc | 8 | 
| 13 | Đặt x thành bb * dddd | không | 8 | 
| 14 | Đặt x thành ccc - x | không | 8 | 
| 15 | Nói x | không | 8 | 

Ví dụ này chứng minh rằng các biến lặp lại được tính mỗi lần chúng nhận được đầu vào. Bản thân tên biến không phải là đơn vị được tính mà là các sự kiện đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi dòng chương trình được kiểm tra một lần. | 
| Không gian | O(1) | Chỉ dòng hiện tại và câu trả lời tích lũy được lưu trữ. | 

Các ràng buộc cho phép tối đa 100000 dòng, do đó quét tuyến tính dễ dàng phù hợp với giới hạn yêu cầu. Giải pháp chỉ thực hiện kiểm tra chuỗi và bổ sung độ dài mà không cần cấu trúc dữ liệu đắt tiền. 

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

# provided sample 1
assert run("""6
Ask read_token and wait
Set first to answer
Ask read_token and wait
Set second to answer
Set result to first + second
Say result
""") == "11\n", "sample 1"

# provided sample 2
assert run("""15
Ask read_token and wait
Set a to answer
Ask read_token and wait
Set bb to answer
Set ccc to a * bb
Set dddd to a / bb
Set dddd to ccc + dddd
Say dddd
Ask read_token and wait
Set bb to answer
Ask read_token and wait
Set ccc to answer
Set x to bb * dddd
Set x to ccc - x
Say x
""") == "8\n", "sample 2"

# minimum number of lines
assert run("""2
Ask read_token and wait
Set x to answer
""") == "1\n", "single character variable"

# repeated input to the same variable
assert run("""6
Ask read_token and wait
Set abc to answer
Ask read_token and wait
Set abc to answer
Say abc
Say abc
""") == "6\n", "repeated inputs"

# many non-input commands
assert run("""5
Set a to answer
Set b to a + a
Set c to b * b
Say c
Say a
""") == "1\n", "only answer assignments count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào duy nhất vào`x`| 1 | Xử lý độ dài biến tối thiểu | 
| Hai đầu vào vào`abc`| 6 | Các lần xuất hiện lặp lại được tính riêng | 
| Chương trình chỉ tính số học sau một lần nhập | 1 | Các bài tập không đầu vào bị bỏ qua | 
| Cung cấp mẫu | 11 và 8 | Phù hợp với các ví dụ chính thức | 

## Vỏ cạnh 

Một chương trình có thể chứa nhiều tên biến, nhưng chỉ có các biến đầu vào mới được đóng góp. Đối với đầu vào:```
6
Ask read_token and wait
Set first to answer
Set result to first + first
Say result
Ask read_token and wait
Set second to answer
```thuật toán chỉ nhận ra`first`Và`second`, sản xuất`5 + 6 = 11`. Dòng số học bao gồm`result`bị bỏ qua vì nó không chứa`answer`. 

Đầu vào lặp đi lặp lại yêu cầu đếm mỗi lần xuất hiện. Vì:```
8
Ask read_token and wait
Set a to answer
Ask read_token and wait
Set a to answer
Set b to a + a
Say b
Ask read_token and wait
Set c to answer
```quá trình quét thêm`1`cho mỗi lần xuất hiện của`a`Và`1`vì`c`, cho`3`. Thuật toán xử lý việc này một cách tự nhiên vì nó xử lý các lệnh thay vì duy trì một tập hợp các tên biến. 

Các lệnh đề cập đến biến nhưng không phải là lệnh gán đầu vào sẽ bị bỏ qua. Một dòng như`Set value to other + value`thay đổi trạng thái chương trình, nhưng không có đầu ra gỡ lỗi bổ sung nào được tạo ra trước nó, vì vậy nó không được ảnh hưởng đến câu trả lời.
