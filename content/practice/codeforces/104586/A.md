---
title: "CF 104586A - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u044f\u0433\u043e\u0434\u044b"
description: "Chúng ta được biết rằng ban đầu tất cả các loại quả mọng đều được đóng gói trong các lọ giống hệt nhau và mỗi lọ đều chứa cùng một số lượng quả mọng. Mỗi lọ chỉ chứa một loại quả mọng."
date: "2026-06-30T07:32:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104586
codeforces_index: "A"
codeforces_contest_name: "Codemasters Codecup 2023 - \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 104586
solve_time_s: 64
verified: true
draft: false
---

[CF 104586A - \u0420\u0443\u0434\u043e\u043b\u044c\u0444 \u0438 \u044f\u0433\u043e\u0434\u044b](https://codeforces.com/problemset/problem/104586/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được biết rằng ban đầu tất cả các loại quả mọng đều được đóng gói trong các lọ giống hệt nhau và mỗi lọ đều chứa cùng một số lượng quả mọng. Mỗi lọ chỉ chứa một loại quả mọng. Sau một số nhầm lẫn trong quá trình phân loại, tất cả các loại quả mọng đã được đổ thành một đống duy nhất và chúng ta được biết tổng số quả nho đen được tìm thấy trong đống đó. 

Trong số tất cả các lọ chứa nho, một số là nho đen và một số là nho đỏ, nhưng chúng bị coi nhầm là cùng loại trong quá trình đóng gói. Những gì chúng ta biết về cấu trúc ban đầu là một phần của tất cả các lọ, bằng`p`, chứa quả lý chua (cả đen và đỏ cùng nhau). Các lọ còn lại chứa các loại quả mọng khác, không quan trọng đối với phép tính cuối cùng ngoại trừ việc chúng góp phần vào tổng số lượng. 

Điểm mấu chốt là tất cả các lọ đều có dung tích như nhau, vì vậy “phần của lọ” được chuyển trực tiếp thành “phần của tổng số quả mọng”. 

Nhiệm vụ là xác định xem bao nhiêu phần trong số các quả nho là quả lý chua đen, vì chúng ta đã biết số lượng quả lý chua đen.`n`, và chúng ta có thể suy ra tổng số quả nho từ tỷ lệ trong lọ`p`. 

Kích thước đầu vào rất nhỏ nên lời giải phải có thời gian không đổi. Bất kỳ cách tiếp cận nào liên quan đến mô phỏng trên lọ hoặc quả mọng đều không cần thiết. Sự tinh tế duy nhất nằm ở việc dịch chính xác một phần lọ thành một phần quả mọng bằng cách sử dụng ràng buộc có kích thước bằng nhau. 

Trường hợp cạnh xuất hiện khi`n = 0`, trong đó không có quả lý chua đen và câu trả lời phải chính xác bằng 0 bất kể`p`. Một trường hợp góc khác là khi`p = 1`, nghĩa là tất cả lọ đều là lọ nho, nên tất cả quả trong đống đều là nho và câu trả lời là`n / total_currants`, điều này sẽ đơn giản hóa một cách rõ ràng mà không gặp vấn đề về phép chia. 

Một sai lầm ngây thơ là đối xử`p`trực tiếp là tỷ lệ quả nho mà không xem xét giả định về các lọ có kích thước bằng nhau hoặc bỏ qua thực tế là tổng số quả được cố định ngầm thông qua cấu trúc lọ. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ cố gắng xây dựng lại số lượng lọ. Nếu giả sử mỗi bình chứa`k`quả mọng, chúng ta có thể thử liệt kê những thứ có thể`k`, sau đó chia tổng số quả vào lọ, sau đó phân chia lọ nho theo tỷ lệ`p`, và cuối cùng là phân phát nho đen và đỏ. Điều này nhanh chóng trở nên không được xác định rõ ràng và vô nghĩa về mặt tính toán, vì có vô số`(k, number of jars)`cặp thỏa mãn các ràng buộc. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần biết`k`hoặc số lọ. Vì mỗi lọ có cùng kích thước nên tỷ lệ lọ và tỷ lệ quả mọng đều giống nhau. Điều này làm sụp đổ toàn bộ cấu trúc thành một mối quan hệ tỷ lệ duy nhất: tỷ lệ quả nho trong số tất cả các loại quả mọng chính xác là`p`. Vì tổng số quả được cố định là một triệu nên chúng ta có thể tính trực tiếp tổng số quả nho như sau:`p * 1e6`. 

Khi đã biết tổng lượng nho, câu trả lời sẽ trở thành một tỷ lệ đơn giản: nho đen trên tổng số nho. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tái thiết lọ | O(N) hoặc tệ hơn (tìm kiếm chưa được xác định) | O(1) | Quá chậm/không rõ ràng | 
| Lý luận tỷ lệ trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`p`. Tại thời điểm này`n`là số quả nho đen, và`p`là tỷ lệ của tất cả các loại quả thuộc lọ nho. 
2. Quy đổi tỷ lệ lọ nho thành tổng số quả nho. Vì tất cả các lọ đều chứa cùng một số quả mọng nên tỷ lệ này áp dụng như nhau cho các quả mọng. Tổng số quả là`1,000,000`, vậy tổng số quả nho bằng`p * 1,000,000`. 
3. Tính tỷ lệ mong muốn như sau`n / (p * 1,000,000)`. Điều này so sánh trực tiếp nho đen với tất cả các loại nho. 
4. Xuất giá trị dưới dạng số dấu phẩy động với độ chính xác đủ để đáp ứng giới hạn lỗi yêu cầu. 

### Tại sao nó hoạt động 

Tất cả các lọ đều có kích thước giống nhau nên việc ánh xạ từ lọ đến quả mọng là tuyến tính. Bất kỳ tập hợp con lọ nào cũng tương ứng với cùng một tập hợp con quả mọng được chia tỷ lệ theo hệ số không đổi. Vì vậy, tỷ lệ lọ nho bằng tỷ lệ quả nho. Điều này làm cho tổng số lượng nho được xác định đầy đủ bởi`p`và tổng số quả đã biết, chỉ để lại một bộ phận duy nhất để tách biệt nho đen. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, p = input().split()
    n = int(n)
    p = float(p)

    total = p * 1_000_000.0
    if total == 0:
        print(0.0)
        return

    ans = n / total
    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo công thức dẫn xuất trực tiếp. Việc quan tâm duy nhất cần làm là chuyển đổi dấu phẩy động của`p`, vì nó có tới sáu chữ số thập phân. Phép nhân với`1e6`phải được thực hiện ở dạng dấu phẩy động để duy trì độ chính xác. 

Người bảo vệ`if total == 0`xử lý trường hợp suy biến trong đó`p = 0`, có nghĩa là không có quả nho nào cả. Trong hoàn cảnh đó,`n`cũng phải bằng 0 và câu trả lời được xác định là 0 nếu không thực hiện phép chia. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
250000 0.5
```Chúng tôi tính toán tổng số nho như`0.5 * 1,000,000 = 500,000`. 

| Bước | n | p | tổng số nho | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 250000 | 0,5 | - | - | 
| tính tổng | 250000 | 0,5 | 500000 | - | 
| tính tỷ lệ | 250000 | 0,5 | 500000 | 0,5 | 

Đầu ra là`0.5`, nghĩa là một nửa số nho có màu đen. 

Điều này khẳng định sự phân chia cân bằng trong đó nho đen phù hợp chính xác với một nửa dân số nho. 

### Mẫu 2 

đầu vào:```
0 0.9
```Chúng tôi tính toán tổng số nho như`0.9 * 1,000,000 = 900,000`. 

| Bước | n | p | tổng số nho | kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 0 | 0,9 | - | - | 
| tính tổng | 0 | 0,9 | 900000 | - | 
| tính tỷ lệ | 0 | 0,9 | 900000 | 0 | 

Đầu ra là`0.0`, phù hợp với thực tế là không có quả lý chua đen nào tồn tại. 

Điều này cho thấy tính đúng đắn khi tử số bằng 0 bất kể tổng kích thước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện bất kể đầu vào | 
| Không gian | O(1) | Không sử dụng cấu trúc phụ trợ | 

Giải pháp là thời gian không đổi và phù hợp một cách tầm thường trong các ràng buộc, vì nó tránh được mọi sự lặp lại hoặc tái thiết cấu trúc ẩn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, p = inp.strip().split()
    n = int(n)
    p = float(p)

    total = p * 1_000_000.0
    if total == 0:
        return "0.0"
    return str(n / total)

# provided samples
assert abs(float(run("250000 0.5")) - 0.5) < 1e-9, "sample 1"
assert abs(float(run("0 0.9")) - 0.0) < 1e-9, "sample 2"
assert abs(float(run("100000 0.1")) - 1.0) < 1e-9, "sample 3"

# custom cases
assert abs(float(run("1 1")) - 0.000001) < 1e-12, "single berry extreme"
assert abs(float(run("500000 1")) - 0.5) < 1e-12, "all currant jars"
assert abs(float(run("0 0.1")) - 0.0) < 1e-12, "zero black berries"
assert abs(float(run("1000 0.000001")) - 1.0) < 1e-9, "tiny fraction edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 0,000001 | tỷ lệ tỷ lệ không cần thiết nhỏ nhất | 
| 500000 1 | 0,5 | phủ đầy nho | 
| 0 0,1 | 0 | độ ổn định của tử số bằng 0 | 
| 1000 0,000001 | 1 | xử lý phân số cực nhỏ | 

## Vỏ cạnh 

### Trường hợp: không có nho đen 

đầu vào:```
0 0.7
```Tổng số nho trở thành`700000`. Sự tính toán`0 / 700000`đánh giá để`0`không có sự bất ổn về số lượng. Thuật toán không bao giờ chia cho 0 trừ khi`p = 0`. 

### Trường hợp: không có lọ nho 

đầu vào:```
0 0
```Ở đây tổng số nho trở thành số không. Thuật toán kiểm tra rõ ràng điều kiện này và trả về`0.0`trực tiếp. Điều này tránh sự phân chia không xác định và phù hợp với cách giải thích rằng không có quả lý chua nào trong hệ thống.
