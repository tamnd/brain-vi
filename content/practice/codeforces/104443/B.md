---
title: "CF 104443B - Nhỏ hơn 100"
description: "Nhiệm vụ là cố ý thoái hóa. Bạn được cung cấp một chuỗi đầu vào cố định duy nhất và cho dù nó được đọc hay xử lý như thế nào, yêu cầu duy nhất là xuất ra một số nguyên. Không có sự thay đổi về nội dung đầu vào giữa các trường hợp thử nghiệm."
date: "2026-06-30T18:02:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104443
codeforces_index: "B"
codeforces_contest_name: "TheForces Round #18 (JuneIsApril-Forces)"
rating: 0
weight: 104443
solve_time_s: 44
verified: true
draft: false
---

[CF 104443B - Nhỏ hơn 100](https://codeforces.com/problemset/problem/104443/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là cố ý thoái hóa. Bạn được cung cấp một chuỗi đầu vào cố định duy nhất và cho dù nó được đọc hay xử lý như thế nào, yêu cầu duy nhất là xuất ra một số nguyên. 

Không có sự thay đổi về nội dung đầu vào giữa các trường hợp thử nghiệm. Đầu vào không mã hóa các tham số, cấu trúc hoặc bất kỳ trường hợp tính toán nào. Thay vào đó, nó đóng vai trò như một trình kích hoạt liên tục: bất cứ khi nào chương trình chạy, nó đảm bảo sẽ nhìn thấy chính xác cùng một câu. 

Điều này thay đổi hoàn toàn bản chất của vấn đề. Không có trích xuất thuật toán để thực hiện, không có logic phân tích cú pháp nào ảnh hưởng đến kết quả và không có sự phụ thuộc giữa đầu vào và đầu ra. Do đó, đầu ra là một giá trị không đổi được xác định bởi định nghĩa bài toán thay vì được tính toán từ đầu vào. 

Vì kích thước đầu vào rất nhỏ và cố định nên các ràng buộc không tạo ra bất kỳ áp lực tính toán nào. Bất kỳ giải pháp hợp lệ nào, ngay cả giải pháp bỏ qua hoàn toàn đầu vào, đều chạy trong thời gian không đổi và thỏa mãn một cách tầm thường các giới hạn hiệu suất. 

Trường hợp chính có thể gây nhầm lẫn cho việc triển khai là kỹ thuật quá mức. Người đọc có thể cố gắng diễn giải chuỗi theo cấu trúc hoặc tính toán các thuộc tính dẫn xuất như độ dài, phân bố ký tự hoặc số được nhúng. Tất cả những điều này đều không liên quan vì chúng không ảnh hưởng đến đầu ra dưới bất kỳ hình thức nào. Ví dụ: coi đầu vào là văn bản có ý nghĩa và tính toán`len("We're growing alone!")`sẽ mang lại 22, kết quả này không chính xác nếu đầu ra dự định là giá trị cố định mà bài toán yêu cầu. 

Giải thích đúng là đầu vào là cá trích đỏ và câu trả lời là hằng số. 

## Phương pháp tiếp cận 

Việc giải thích thô bạo sẽ bắt đầu bằng cách đọc chuỗi và cố gắng trích xuất một số ý nghĩa số từ nó. Người ta có thể tưởng tượng việc quét các ký tự, đếm mã thông báo hoặc ánh xạ các chữ cái thành các giá trị. Điều này vẫn đúng theo quan điểm lập trình theo nghĩa là nó xử lý đầu vào, nhưng nó sẽ không dẫn đến kết quả nhất quán hoặc có ý nghĩa vì bài toán không xác định bất kỳ chuyển đổi nào từ chuỗi sang đầu ra. 

Quan sát chính là không có mối quan hệ chức năng giữa đầu vào và đầu ra. Chuỗi được cố định và đầu ra được xác định trước. Điều này thu gọn toàn bộ vấn đề thành in ấn liên tục. 

Cách tiếp cận bạo lực không thành công về mặt khái niệm vì nó giả định cấu trúc không tồn tại. Khi chúng tôi nhận ra rằng đầu vào không mang thông tin, giải pháp sẽ chuyển sang trả về một hằng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào từ đầu vào tiêu chuẩn. Bước này chỉ được yêu cầu để đáp ứng định dạng đầu vào nhưng giá trị không được sử dụng trong bất kỳ tính toán nào. 
2. Xuất ngay số nguyên không đổi`17`. Lựa chọn 17 được xác định theo định nghĩa bài toán và không phụ thuộc vào nội dung đầu vào. 

### Tại sao nó hoạt động 

Tính chính xác xuất phát từ thực tế là mọi phiên bản đầu vào hợp lệ đều giống hệt nhau và do đó phải ánh xạ tới cùng một đầu ra. Vì không có logic phân nhánh hoặc trích xuất tham số nên hàm từ đầu vào đến đầu ra là không đổi. Bất kỳ triển khai nào luôn in 17 đều thỏa mãn chính xác ánh xạ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    _ = input().strip()
    print(17)

if __name__ == "__main__":
    solve()
```Giải pháp chỉ đọc dòng đầu vào để tuân theo hành vi I/O dự kiến. Biến bị loại bỏ ngay lập tức vì nó không ảnh hưởng đến kết quả. 

Quyết định mã hóa cứng đầu ra là sự đơn giản hóa trung tâm. Không có logic điều kiện, không phân tích cú pháp và không tính toán ngoài việc in. 

## Ví dụ đã hoạt động 

Vì đầu vào luôn là cùng một chuỗi nên cả hai dấu vết mẫu đều giống hệt nhau. 

### Ví dụ 1 

| Bước | Đầu vào | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Chúng tôi đang phát triển một mình! | Đọc đầu vào | - | 
| 2 | Chúng tôi đang phát triển một mình! | Bỏ qua giá trị | - | 
| 3 | - | In liên tục | 17 | 

Dấu vết này cho thấy đầu vào không bao giờ ảnh hưởng đến luồng điều khiển sau khi được đọc. 

### Ví dụ 2 

| Bước | Đầu vào | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | Chúng tôi đang phát triển một mình! | Đọc đầu vào | - | 
| 2 | Chúng tôi đang phát triển một mình! | Bỏ qua giá trị | - | 
| 3 | - | In liên tục | 17 | 

Điều này xác nhận rằng các lần thực thi lặp lại là giống hệt nhau, củng cố rằng ánh xạ là không đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Một lần đọc đầu vào và một thao tác in | 
| Không gian | O(1) | Không có cấu trúc dữ liệu nào được lưu trữ | 

Giải pháp này có hiệu quả tương đối và phù hợp với mọi ràng buộc hợp lý vì nó thực hiện công việc liên tục bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample (implied single input)
assert run("We're growing alone!\n") == "17", "sample 1"

# custom cases
assert run("We're growing alone!\n") == "17", "repeated input"
assert run("We're growing alone!\n") == "17", "minimal variability check"
assert run("We're growing alone!\n") == "17", "stress constant behavior"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chúng tôi đang phát triển một mình! | 17 | Trường hợp bắt buộc cơ sở | 
| cùng một chuỗi nữa | 17 | Bất lực | 
| chạy lặp đi lặp lại | 17 | Tính nhất quán giữa các lần thực thi | 

## Vỏ cạnh 

Không có trường hợp biên nào có ý nghĩa ngoài định dạng đầu vào cố định bắt buộc. Thuật toán đọc chính xác một dòng và bỏ qua nội dung của nó, do đó, các biến thể như khoảng trắng ở cuối hoặc xử lý dòng mới không ảnh hưởng đến độ chính xác miễn là dòng đầu vào được sử dụng đúng cách. 

Ngay cả khi chuỗi đầu vào bao gồm các biến thể dấu câu hoặc khoảng trắng, chương trình sẽ không diễn giải chuỗi đó, do đó những khác biệt này không thể thay đổi kết quả đầu ra.
