---
title: "CF 105977M - \u81f4\u8c22"
description: "Bài toán đưa ra một bảng lịch sử cố định mô tả 11 phiên bản của chuỗi cuộc thi lập trình ở Phúc Kiến. Mỗi phiên bản có một trường đại học tổ chức và mỗi trường đại học được liên kết với một từ viết tắt tiếng Anh ngắn gọn."
date: "2026-06-22T16:29:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105977
codeforces_index: "M"
codeforces_contest_name: "2025 National Invitational of CCPC (Fujian), The 12th Fujian Collegiate Programming Contest"
rating: 0
weight: 105977
solve_time_s: 46
verified: true
draft: false
---

[CF 105977M - \u81f4\u8c22](https://codeforces.com/problemset/problem/105977/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một bảng lịch sử cố định mô tả 11 phiên bản của chuỗi cuộc thi lập trình ở Phúc Kiến. Mỗi phiên bản có một trường đại học tổ chức và mỗi trường đại học được liên kết với một từ viết tắt tiếng Anh ngắn gọn. Đầu vào là một số nguyên duy nhất`n`, đại diện cho phiên bản nào của cuộc thi mà chúng ta quan tâm. Nhiệm vụ là xuất ra tên viết tắt của trường đại học tổ chức cuộc thi`n`-ấn bản thứ. 

Mặc dù câu lệnh chứa nhiều ký tự nhiễu và bị cắt xén, cấu trúc thực tế rất đơn giản: có một danh sách tĩnh gồm 11 mục và câu trả lời là tra cứu trực tiếp theo chỉ mục. 

Ràng buộc`1 ≤ n ≤ 11`là cực kỳ nhỏ, điều này ngay lập tức loại trừ mọi nhu cầu xử lý trước, tìm kiếm hoặc tối ưu hóa thuật toán. Bất kỳ giải pháp nào thực hiện ngay cả thao tác lập chỉ mục mảng theo thời gian không đổi là đủ. 

Không có trường hợp ẩn nào liên quan đến kích thước đầu vào lớn hoặc nhiều truy vấn. Vấn đề tinh tế duy nhất mà việc triển khai bất cẩn có thể gặp phải là lập chỉ mục theo từng thứ một, vì các vấn đề cạnh tranh như thế này thường đánh số phiên bản bắt đầu từ 1 trong khi các ngôn ngữ lập trình lập chỉ mục mảng từ 0. 

Ví dụ: nếu ai đó coi nhầm ấn bản đầu tiên là chỉ mục 0 thì hãy nhập`1`sẽ ánh xạ không chính xác tới trường đại học thứ hai thay vì trường đại học đầu tiên, tạo ra một từ viết tắt sai mặc dù logic có vẻ đúng. 

## Phương pháp tiếp cận 

Cấu trúc của bài toán về cơ bản là ánh xạ tĩnh từ số nguyên sang chuỗi. Cách giải thích thô bạo sẽ là tái tạo lại bản đồ một cách linh hoạt bằng cách phân tích bảng đầy đủ các trường đại học và trích xuất các chữ viết tắt mỗi lần, nhưng đó là chi phí không cần thiết. Bảng được cố định và rất nhỏ, vì vậy việc tính toán lại hoặc phân tích cú pháp nó nhiều lần sẽ chỉ làm tăng thêm độ phức tạp mà không mang lại lợi ích gì. 

Quan sát quan trọng là không có gì thay đổi trong các trường hợp thử nghiệm và không cần tính toán ngoài việc chọn giá trị chính xác đã biết trước. Khi chúng tôi nhận ra rằng vấn đề là một nhiệm vụ lập chỉ mục trực tiếp, giải pháp sẽ giảm xuống việc lưu trữ 11 chữ viết tắt trong một mảng và trả về`(n-1)`-phần tử thứ. 

Ý tưởng brute-force thất bại về mặt tinh tế hơn là tốc độ: nó sẽ liên quan đến việc đọc lại văn bản có cấu trúc hoặc mô phỏng logic phân tích cú pháp, điều này rất dễ bị ảnh hưởng do nhiễu mã hóa trong câu lệnh. Cách tiếp cận lập chỉ mục trực tiếp là mạnh mẽ và tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích cú pháp Brute Force | O(1) cho mỗi truy vấn nhưng việc triển khai phức tạp | O(1) | Không cần thiết | 
| Tra cứu trực tiếp | O(1) | O(11) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách`abbr`chứa 11 từ viết tắt đã biết theo thứ tự các phiên bản cuộc thi. Danh sách này được lấy trực tiếp từ bảng bài toán, trong đó mỗi vị trí tương ứng với một phiên bản. 
2. Đọc số nguyên`n`từ đầu vào. 
3. Đầu ra`abbr[n - 1]`, chuyển đổi từ lập chỉ mục dựa trên 1 trong câu lệnh vấn đề sang lập chỉ mục dựa trên 0 trong biểu diễn mảng. 

### Tại sao nó hoạt động 

Các phiên bản cuộc thi tạo thành một trình tự cố định không có sự biến đổi hoặc phụ thuộc giữa các mục. Mỗi truy vấn chỉ đơn giản là yêu cầu giá trị được lưu trữ ở một vị trí đã biết trong chuỗi đó. Bởi vì ánh xạ là tĩnh và tổng thể trên miền`1..11`, lưu trữ nó một cách rõ ràng đảm bảo tính chính xác. Việc chuyển đổi duy nhất cần có là điều chỉnh việc lập chỉ mục và khi việc đó được xử lý, việc tra cứu sẽ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

abbr = [
    "FZU",
    "FNU",
    "FZU",
    "FZU",
    "FAFU",
    "HQU",
    "MJU",
    "XMUT",
    "QZNU",
    "JMU",
    "FZU"
]

n = int(input().strip())
print(abbr[n - 1])
```Giải pháp mã hóa chuỗi viết tắt theo thứ tự chính xác của các phiên bản cuộc thi. Điều này là có chủ ý vì tập dữ liệu đã được cố định bởi chính bản báo cáo vấn đề. 

Chi tiết triển khai duy nhất quan trọng là`n - 1`điều chỉnh. Vì danh sách Python không được lập chỉ mục nhưng vấn đề xác định phiên bản đầu tiên là`1`, nếu không trừ đi một câu trả lời sẽ dịch chuyển mọi câu trả lời đi một vị trí. 

## Ví dụ đã hoạt động 

Xem xét đầu vào`1`. Thuật toán đọc`n = 1`và truy cập`abbr[0]`, đó là`"FZU"`. Điều này phù hợp với người tổ chức phiên bản đầu tiên. 

Bây giờ hãy xem xét`2`. Thuật toán truy cập`abbr[1]`, trở về`"FNU"`. Điều này tương ứng với phiên bản thứ hai trong bảng. 

| Bước | n | Chỉ mục được sử dụng | Giá trị | 
| --- | --- | --- | --- | 
| Ví dụ 1 | 1 | 0 | FZU | 
| Ví dụ 2 | 2 | 1 | FNU | 

Những ví dụ này xác nhận rằng ánh xạ hoàn toàn là vị trí và việc lập chỉ mục là phép biến đổi duy nhất được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ tra cứu một mảng duy nhất sau khi đọc đầu vào | 
| Không gian | O(1) | Đã sửa danh sách 11 chuỗi | 

Các ràng buộc đảm bảo rằng việc tra cứu theo thời gian liên tục là đủ. Ngay cả khi đầu vào được mở rộng một chút, cách tiếp cận này vẫn tối ưu vì không cần tính toán ngoài việc lập chỉ mục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    abbr = [
        "FZU","FNU","FZU","FZU","FAFU",
        "HQU","MJU","XMUT","QZNU","JMU","FZU"
    ]
    n = int(sys.stdin.readline().strip())
    return abbr[n - 1]

# provided samples
assert run("1\n") == "FZU"
assert run("2\n") == "FNU"

# boundary cases
assert run("11\n") == "FZU"
assert run("5\n") == "FAFU"
assert run("7\n") == "MJU"
assert run("9\n") == "QZNU"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | FZU | Tính chính xác của phần tử đầu tiên | 
| 2 | FNU | Tính chính xác của phần tử thứ hai | 
| 11 | FZU | Lập chỉ mục ranh giới trên | 
| 5 | FAFU | Tính chính xác của bản đồ giữa | 

## Vỏ cạnh 

Trường hợp cạnh có ý nghĩa duy nhất là lập chỉ mục ranh giới ở cuối danh sách. Đối với đầu vào`1`, thuật toán tính chỉ số`0`, lấy chính xác chữ viết tắt đầu tiên. Đối với đầu vào`11`, nó tính chỉ số`10`, để lấy mục cuối cùng. 

Ví dụ, khi`n = 11`, phép tính là: 

Việc truy cập danh sách trở thành`abbr[10]`, trả về`"FZU"`. Vì danh sách được xây dựng rõ ràng theo đúng thứ tự nên không xảy ra truy cập ngoài phạm vi và không cần xử lý đặc biệt ngoài việc thay đổi chỉ mục.
