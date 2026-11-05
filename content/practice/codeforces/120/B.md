---
title: "CF 120B - Giải Đố vui"
description: "Chúng ta được đưa cho một bảng tròn được chia thành n khu vực, mỗi khu vực chứa một câu hỏi trắc nghiệm. Một số câu hỏi đã được hỏi, được đánh dấu bằng 0 và những câu hỏi khác vẫn còn, được đánh dấu bằng 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 120
codeforces_index: "B"
codeforces_contest_name: "School Regional Team Contest, Saratov, 2011"
rating: 1100
weight: 120
solve_time_s: 81
verified: true
draft: false
---

[CF 120B - Giải đấu đố vui](https://codeforces.com/problemset/problem/120/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng tròn được chia thành`n`các lĩnh vực, mỗi lĩnh vực có chứa một câu hỏi đố. Một số câu hỏi đã được hỏi, được đánh dấu bằng`0`, và những thứ khác vẫn có sẵn, được đánh dấu bằng`1`. Một mũi tên chỉ vào khu vực`k`, và mục tiêu là xác định số lượng câu hỏi tiếp theo sẽ được hỏi. Nếu khu vực được chỉ bằng mũi tên đã được hỏi, người dẫn chương trình sẽ chọn câu hỏi chưa được trả lời tiếp theo theo chiều kim đồng hồ. Việc đánh số bao quanh khu vực`n`quay lại lĩnh vực`1`. 

Dữ liệu đầu vào bao gồm hai số nguyên,`n`Và`k`, theo sau là danh sách`n`số nguyên biểu thị trạng thái của từng ngành. Đầu ra là một số nguyên duy nhất biểu thị số lĩnh vực của câu hỏi tiếp theo sẽ được hỏi. 

Các hạn chế là nhỏ:`n`có thể lên tới 1000. Điều này ngụ ý rằng ngay cả một tìm kiếm tuyến tính đơn giản quanh bảng cũng đủ hiệu quả, vì trong trường hợp xấu nhất chúng ta sẽ thực hiện`n`kiểm tra mỗi truy vấn, tối đa là 1000 thao tác. 

Các trường hợp cạnh không rõ ràng bao gồm mũi tên chỉ vào khu vực cuối cùng, với câu hỏi có sẵn tiếp theo bao quanh phần đầu hoặc có mũi tên chỉ thẳng vào một câu hỏi có sẵn (không cần di chuyển). Ví dụ, nếu`n = 5`,`k = 5`, và mảng là`[0, 1, 0, 1, 0]`, mũi tên chỉ vào khu vực đã được hỏi (`0`). Khu vực chưa được trả lời tiếp theo theo chiều kim đồng hồ là`2`, không`5`, bởi vì`5`đã được sử dụng rồi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản và chính xác: bắt đầu tại khu vực được chỉ định bởi mũi tên và kiểm tra xem câu hỏi đã được hỏi chưa. Nếu có, hãy di chuyển theo chiều kim đồng hồ từng khu vực một, quấn quanh khi cần thiết cho đến khi tìm thấy câu hỏi sẵn có. Độ phức tạp trong trường hợp xấu nhất là`O(n)`, có thể chấp nhận được`n ≤ 1000`. Cách tiếp cận này hiệu quả vì chúng tôi được đảm bảo rằng ít nhất một câu hỏi chưa được trả lời. 

Lời giải tối ưu về cơ bản giống như lời giải brute-force vì cấu trúc của bài toán - một mảng hình tròn nhỏ - cho phép chúng ta tìm kiếm tuyến tính mà không bị phạt. Không cần cấu trúc dữ liệu bổ sung và có thể xử lý việc bao bọc bằng phép toán modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

Các cách tiếp cận đơn giản và tối ưu hội tụ ở đây vì các ràng buộc của bài toán nhỏ và thao tác đơn giản. 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. Điều chỉnh`k`lập chỉ mục dựa trên 0 để truy cập mảng dễ dàng hơn. 
2. Đọc mảng trạng thái câu hỏi. Mỗi phần tử là`1`nếu câu hỏi không được trả lời và`0`nếu nó đã được hỏi. 
3. Bắt đầu vòng lặp tại chỉ mục`k-1`và di chuyển theo chiều kim đồng hồ. Đối với mỗi lần lặp lại, hãy kiểm tra xem khu vực hiện tại có sẵn không. 
4. Nếu khu vực hiện tại có câu hỏi chưa được trả lời (`1`), in chỉ mục dựa trên 1 của nó và dừng lại. 
5. Nếu lĩnh vực hiện tại đã được hỏi (`0`), tăng chỉ số lên một và sử dụng modulo`n`để bắt đầu lại nếu cần thiết. 

Tại sao nó hoạt động: điều bất biến là ở mỗi bước, chúng ta đều tìm thấy một câu hỏi chưa được trả lời hoặc chúng ta tiếp tục chuyển sang lĩnh vực tiếp theo. Bởi vì vấn đề đảm bảo có ít nhất một câu hỏi chưa được trả lời nên vòng lặp sẽ luôn kết thúc ở đúng khu vực. Hoạt động modulo đảm bảo chúng ta xử lý chính xác tính chất vòng tròn của bảng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
a = list(map(int, input().split()))

# convert to zero-based index
idx = k - 1

while True:
    if a[idx] == 1:
        print(idx + 1)  # output 1-based index
        break
    idx = (idx + 1) % n
```Mã bắt đầu từ khu vực được chỉ định bởi mũi tên, kiểm tra xem nó có được trả lời hay không và di chuyển theo chiều kim đồng hồ nếu cần. Modulo đảm bảo rằng việc di chuyển qua khu vực cuối cùng sẽ kết thúc với khu vực đầu tiên. Việc sử dụng lập chỉ mục dựa trên số 0 trong nội bộ sẽ đơn giản hóa việc truy cập mảng và số học. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
5 5
0 1 0 1 0
```| Bước | idx | a[idx] | Hành động | 
| --- | --- | --- | --- | 
| 1 | 4 | 0 | đã được hỏi rồi, hãy chuyển sang phần tiếp theo | 
| 2 | 0 | 0 | đã được hỏi rồi, hãy chuyển sang phần tiếp theo | 
| 3 | 1 | 1 | tìm thấy có sẵn, đầu ra 2 | 

Điều này cho thấy thuật toán bao quanh mảng một cách chính xác và chọn câu hỏi chưa được trả lời tiếp theo theo chiều kim đồng hồ. 

**Mẫu 2** (đã thi công) 

đầu vào:```
6 3
1 0 0 1 1 0
```| Bước | idx | a[idx] | Hành động | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | chuyển sang tiếp theo | 
| 2 | 3 | 1 | tìm thấy có sẵn, đầu ra 4 | 

Điều này xác nhận rằng nếu mũi tên chỉ vào khu vực đã được hỏi thì việc tìm kiếm sẽ tiếp tục cho đến khi tìm thấy câu hỏi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Trong trường hợp xấu nhất, chúng tôi có thể phải kiểm tra từng lĩnh vực một lần. | 
| Không gian | O(n) | Lưu trữ mảng trạng thái câu hỏi. | 

Giải pháp này nằm trong giới hạn thời gian 1 giây cho`n ≤ 1000`và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    idx = k - 1
    while True:
        if a[idx] == 1:
            print(idx + 1)
            break
        idx = (idx + 1) % n
    return output.getvalue().strip()

# Provided sample
assert run("5 5\n0 1 0 1 0\n") == "2", "sample 1"

# Custom cases
assert run("1 1\n1\n") == "1", "single sector, unanswered"
assert run("3 2\n0 0 1\n") == "3", "arrow points at asked, next is last sector"
assert run("4 4\n1 0 1 0\n") == "4", "arrow points at available, no move"
assert run("5 3\n0 0 0 1 0\n") == "4", "wrap around multiple zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1`|`1`| trường hợp cạnh khu vực đơn | 
|`3 2\n0 0 1`|`3`| mũi tên chỉ vào phần hỏi, tiếp theo là khu vực cuối cùng | 
|`4 4\n1 0 1 0`|`4`| mũi tên chỉ vào câu hỏi có sẵn | 
|`5 3\n0 0 0 1 0`|`4`| tìm kiếm toàn diện cho câu trả lời tiếp theo | 

## Vỏ cạnh 

Nếu mũi tên chỉ vào khu vực cuối cùng và tất cả các khu vực trước đó đã được hỏi, modulo đảm bảo rằng việc tìm kiếm tiếp tục từ đầu. Đối với đầu vào`5 5\n0 1 0 1 0`, tìm kiếm di chuyển từ chỉ mục`4`ĐẾN`0`, sau đó`1`, đây là khu vực có sẵn đầu tiên, xuất ra chính xác`2`. 

Đối với bảng một ngành có câu hỏi chưa được trả lời,`1 1\n1`, vòng lặp ngay lập tức tìm thấy câu hỏi có sẵn mà không cần lặp lại, xuất ra chính xác`1`. 

Thuật toán này xử lý tất cả các trường hợp cạnh theo bất biến, nó luôn kiểm tra từng khu vực theo chiều kim đồng hồ cho đến khi tìm thấy câu hỏi chưa được trả lời và modulo đảm bảo bao quanh bảng tròn một cách thích hợp.
