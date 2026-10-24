---
title: "CF 106020B - Sự cố miễn phí"
description: "Vấn đề này chỉ là một thủ thuật đầu ra nhỏ ẩn bên trong một dạng cuộc thi lập trình thông thường. Ban giám khảo hỏi bạn có muốn một bài toán miễn phí không. Phản hồi duy nhất được chấp nhận là chuỗi chính xác Yee. Bất kỳ văn bản nào khác, bao gồm các xác nhận thông thường như Có, đều bị từ chối."
date: "2026-06-25T13:10:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106020
codeforces_index: "B"
codeforces_contest_name: "The 2025 Damascus University Collegiate Programming Contest"
rating: 0
weight: 106020
solve_time_s: 46
verified: true
draft: false
---

[CF 106020B - Sự cố miễn phí](https://codeforces.com/problemset/problem/106020/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề này chỉ là một thủ thuật đầu ra nhỏ ẩn bên trong một dạng cuộc thi lập trình thông thường. Ban giám khảo hỏi bạn có muốn một bài toán miễn phí không. Phản hồi duy nhất được chấp nhận là chuỗi chính xác`Yee`. Bất kỳ văn bản nào khác, bao gồm các xác nhận thông thường như`Yes`, bị từ chối. 

Phần đầu vào thực sự không liên quan đến giải pháp. Ngay cả khi thẩm phán cung cấp một số luồng đầu vào, câu trả lời bắt buộc không phụ thuộc vào nội dung của nó. Chương trình chỉ cần in từ cần thiết. 

Các ràng buộc ngụ ý rằng không cần phải phân tích dữ liệu, lưu trữ mảng, xây dựng biểu đồ hoặc thực hiện bất kỳ tính toán nào. Một giải pháp đọc toàn bộ dữ liệu đầu vào hoặc thực hiện những công việc không cần thiết vẫn sẽ đạt giới hạn bình thường, nhưng nó giải quyết được một vấn đề khó hơn vấn đề được yêu cầu. Số lượng hoạt động dự định là không đổi, đây là cách tiếp cận hợp lý duy nhất cho sự cố trong đó đầu ra được cố định. 

Các trường hợp chính không liên quan đến thuật toán mà là về kết quả đầu ra chính xác. Một lỗi thường gặp là in`Yes`bởi vì nó có vẻ là câu trả lời đúng. Ví dụ: đầu ra của vấn đề là:```
Yes
```Đầu ra đúng không phải là chuỗi này nên việc gửi bị từ chối. 

Một sai lầm khác là thêm văn bản bổ sung. Ví dụ:```
Yee!
```khác với kết quả đầu ra được yêu cầu vì dấu chấm than làm thay đổi chuỗi. Thẩm phán so sánh kết quả đầu ra một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng hiểu câu chuyện, đọc các giá trị đầu vào, mô phỏng sự tương tác hoặc tìm kiếm một điều kiện ẩn quyết định câu trả lời. Chiến lược này thất bại vì không có điều kiện thay đổi để tính toán. Số lượng đầu vào có thể là không liên quan vì mọi trường hợp đều có cùng một câu trả lời. 

Quan sát chính là vấn đề đang kiểm tra sự chú ý đến yêu cầu đầu ra chính xác hơn là kỹ năng thuật toán. Khi chúng tôi nhận ra rằng câu trả lời là một chuỗi cố định, toàn bộ giải pháp sẽ chuyển sang in chuỗi đó. 

Ý tưởng bạo lực thực sự không bị giới hạn trong những công việc không cần thiết, trong khi giải pháp dự định sử dụng một thao tác đầu ra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(kích thước đầu vào) | O(kích thước đầu vào) | Tinh thần quá chậm chạp và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bỏ qua đầu vào vì nó không ảnh hưởng đến câu trả lời. Đầu ra yêu cầu được xác định hoàn toàn bởi câu lệnh. 
2. In chính xác chuỗi`Yee`. Chữ viết hoa và chính tả phải khớp nhau vì giám khảo sẽ trực tiếp kiểm tra văn bản đầu ra. 

Tại sao nó hoạt động: 

Điều kiện duy nhất cần thiết để được chấp nhận là tạo ra phản hồi cần thiết. Vì cùng một phản hồi có giá trị cho mọi trường hợp kiểm tra có thể xảy ra nên việc in trực tiếp nó luôn làm hài lòng giám khảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

print("Yee")
```Giải pháp chỉ chứa một thao tác quan trọng: in câu trả lời được yêu cầu. Không có điều kiện biên, vòng lặp, cấu trúc dữ liệu hoặc phép toán số học nào có thể mắc sai sót. 

Chương trình không cần tiêu thụ đầu vào. Python tự động thoát sau khi in và giám khảo chỉ kiểm tra kết quả đầu ra. Cách viết chính xác của`Yee`là chi tiết triển khai duy nhất quan trọng. 

## Ví dụ đã hoạt động 

Vì mọi đầu vào có thể đều tạo ra cùng một đầu ra nên các dấu vết đều giống hệt nhau. 

Mẫu 1: 

| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Chương trình bắt đầu | trống | 
| 2 | In đáp án cố định | Yee | 

Dấu vết này cho thấy thuật toán không phụ thuộc vào bất kỳ giá trị nào được cung cấp. 

Mẫu 2: 

| Bước | Hành động | Đầu ra | 
| --- | --- | --- | 
| 1 | Chương trình bắt đầu | trống | 
| 2 | In đáp án cố định | Yee | 

Điều này chứng tỏ rằng ngay cả một đầu vào hoàn toàn khác cũng không thể thay đổi kết quả. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chương trình thực hiện một thao tác in. | 
| Không gian | O(1) | Không có biến hoặc cấu trúc dữ liệu nào được lưu trữ. | 

Giải pháp này phù hợp với mọi giới hạn thời gian và bộ nhớ hợp lý vì nó hoạt động liên tục bất kể kích thước đầu vào. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    print("Yee")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

# provided samples style
assert run("") == "Yee\n", "empty input"
assert run("1\n") == "Yee\n", "arbitrary input"

# custom cases
assert run("100 200 300\n") == "Yee\n", "input should not matter"
assert run("abc\n") == "Yee\n", "non numeric input should not matter"
assert run("999999999999999999\n") == "Yee\n", "large input should not matter"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào trống | Yee | Xác nhận không cần nhập liệu | 
|`1`| Yee | Xác nhận đầu vào bình thường bị bỏ qua | 
| Số lượng lớn | Yee | Xác nhận không có sự phụ thuộc vào giá trị | 
| Văn bản tùy ý | Yee | Xác nhận giải pháp chỉ in câu trả lời cố định | 

## Vỏ cạnh 

Đối với trường hợp ai đó in`Yes`thay vì`Yee`, thuật toán thất bại vì chuỗi đầu ra khác. Việc thực hiện đúng chỉ đơn giản là in`Yee`, khớp chính xác với phản hồi được yêu cầu. 

Đối với trường hợp ai đó in các ký tự bổ sung, chẳng hạn như`Yee!`, thuật toán cũng thất bại vì so sánh đầu ra dựa trên ký tự. Dấu vết chính xác là một thao tác in duy nhất chỉ tạo ra`Yee`. 

Đối với bất kỳ đầu vào nào, chẳng hạn như số lượng lớn hoặc văn bản không liên quan, thuật toán sẽ thực hiện cùng một hành động. Nó không phân tích cú pháp đầu vào, do đó không có vấn đề tràn, lỗi lập chỉ mục hoặc giả định không hợp lệ về dữ liệu.
