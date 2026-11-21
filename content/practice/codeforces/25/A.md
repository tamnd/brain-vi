---
title: "CF 25A - Kiểm tra IQ"
description: "Chúng ta có một dãy gồm n số tự nhiên, trong đó n ít nhất là 3 và nhiều nhất là 100, và mỗi số nhiều nhất là 100. Trong số những số này, tất cả các số này, ngoại trừ một số, có cùng tính chẵn lẻ - chẵn hoặc lẻ - và có chính xác một số khác nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 25
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 25 (Div. 2 Only)"
rating: 1300
weight: 25
solve_time_s: 67
verified: true
draft: false
---
[CF 25A - Kiểm tra IQ](https://codeforces.com/problemset/problem/25/A) 

**Đánh giá:** 1300 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi`n`số tự nhiên ở đâu`n`ít nhất là 3 và nhiều nhất là 100, và mỗi số nhiều nhất là 100. Trong số những số này, tất cả ngoại trừ một số đều có cùng tính chẵn lẻ - chẵn hoặc lẻ - và có chính xác một số khác nhau. Nhiệm vụ của chúng ta là xác định vị trí của số “lẻ một” này trong dãy, đếm các vị trí bắt đầu từ 1. 

Vấn đề có quy mô nhỏ. Với`n`giới hạn ở 100, chúng ta có thể sử dụng các thuật toán để kiểm tra trực tiếp từng số mà không cần lo lắng về hiệu quả. Khó khăn chính không nằm ở tính toán thô mà ở việc xác định chính xác số nào phá vỡ mô hình chẵn lẻ. 

Một trường hợp phức tạp xảy ra khi một vài số đầu tiên không thiết lập rõ ràng tính chẵn lẻ của đa số. Ví dụ, nếu trình tự là`3 2 7`, chúng ta không thể kết luận ngay về tính chẵn lẻ của đa số chỉ bằng cách nhìn vào hai số đầu tiên - cần phải kiểm tra cẩn thận để đảm bảo chúng ta chọn được một số thực sự khác biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là bạo lực: đối với mỗi số, hãy đếm xem có bao nhiêu số chẵn và bao nhiêu số lẻ, sau đó chọn số có số chẵn lẻ khác với đa số. Phương pháp này hiệu quả vì chúng ta được đảm bảo rằng chính xác có một số khác nhau. Vì`n`lên tới 100, điều này đòi hỏi nhiều nhất là vài trăm thao tác - tầm thường đối với các bộ xử lý hiện đại - vì vậy hiệu suất không phải là vấn đề đáng lo ngại. 

Thông tin chi tiết quan trọng giúp tạo ra giải pháp sạch hơn là chúng ta không cần kiểm tra toàn bộ chuỗi để xác định tính chẵn lẻ của đa số. Nhìn vào ba số đầu tiên là đủ. Nếu ít nhất hai trong số những số này có cùng số chẵn lẻ thì số chẵn lẻ đó là đa số. Khi chúng ta biết tính chẵn lẻ của đa số, chúng ta có thể quét chuỗi một lần để tìm số khác. Điều này làm giảm việc kiểm tra không cần thiết và làm cho logic trở nên đơn giản và mạnh mẽ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phần tử`n`và dãy số. Chúng tôi cần các số trong danh sách để có thể lập chỉ mục cho chúng. 
2. Tính chẵn lẻ của ba số đầu tiên bằng cách lấy mỗi số theo modulo 2. Ba số chẵn lẻ này xác định số chẵn lẻ của đa số trong dãy vì ít nhất hai trong số chúng phải khớp nhau. 
3. Xác định tính chẵn lẻ của đa số. Nếu hai hoặc nhiều hơn trong ba số đầu là số chẵn thì đa số là số chẵn; mặt khác, đa số là kỳ quặc. 
4. Lặp lại toàn bộ dãy số. Đối với mỗi số, hãy tính tính chẵn lẻ của nó và so sánh nó với số chẵn lẻ của đa số. 
5. Ngay khi chúng tôi tìm thấy một số có tính chẵn lẻ không khớp với đa số, hãy xuất vị trí của nó (chỉ số dựa trên 1) và kết thúc. Đây được đảm bảo là số duy nhất có tính chẵn lẻ khác nhau. 

Tại sao nó hoạt động: Điều bất biến là có chính xác một số khác với số còn lại. Bằng cách xác định tính chẵn lẻ của đa số từ ba số đầu tiên, chúng tôi đảm bảo rằng việc quét phần còn lại của chuỗi sẽ xác định chính xác giá trị ngoại lệ mà không có sự mơ hồ. Vì việc đảm bảo đầu vào đảm bảo tính chẵn lẻ khác nhau nên chúng tôi không bao giờ gặp rủi ro về kết quả dương tính giả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
nums = list(map(int, input().split()))

# determine majority parity from the first three numbers
first_three = nums[:3]
parities = [x % 2 for x in first_three]
majority_parity = 0 if parities.count(0) > 1 else 1

# find the index of the number with different parity
for i, num in enumerate(nums):
    if num % 2 != majority_parity:
        print(i + 1)
        break
```Đoạn mã đầu tiên cắt ba phần tử đầu tiên để thiết lập tính chẵn lẻ của đa số. Đếm số không trong`parities`kiểm tra xem có bao nhiêu số chẵn Vòng lặp sau đó trực tiếp tìm kiếm một số vi phạm tính chẵn lẻ của đa số. Chúng tôi thêm 1 vào chỉ mục để chuyển đổi từ lập chỉ mục Python dựa trên 0 sang lập chỉ mục dựa trên 1 theo yêu cầu của vấn đề. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:`5 2 4 7 8 10`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | n | 5 | 
| Đọc đầu vào | số | [2, 4, 7, 8, 10] | 
| Ba lần chẵn lẻ đầu tiên | chẵn lẻ | [0, 0, 1] | 
| Đa số ngang bằng | đa số_chẵn lẻ | 0 (chẵn) | 
| Lặp lại | i=2, num=7 | 7 % 2 = 1 ≠ 0 → đầu ra 3 | 

Bảng hiển thị thuật toán xác định chính xác 7 là số khác nhau về tính chẵn lẻ. 

**Ví dụ 2:** 

đầu vào:`4 1 1 2 1`| Bước | Biến | Giá trị | 
| --- | --- | --- | 
| Đọc đầu vào | n | 4 | 
| Đọc đầu vào | số | [1, 1, 2, 1] | 
| Ba lần chẵn lẻ đầu tiên | chẵn lẻ | [1, 1, 0] | 
| Đa số ngang bằng | đa số_chẵn lẻ | 1 (lẻ) | 
| Lặp lại | i=2, num=2 | 2 % 2 = 0 ≠ 1 → đầu ra 3 | 

Ở đây, trường hợp cạnh trong đó giá trị ngoại lệ xuất hiện dưới dạng số thứ ba chứng tỏ rằng việc phát hiện tính chẵn lẻ đa số chỉ hoạt động với ba phần tử đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chúng tôi quét ba phần tử đầu tiên để xác định phần lớn, sau đó lặp lại toàn bộ mảng một lần. Tối đa n = 100, vì vậy điều này không đáng kể. | 
| Không gian | O(n) | Chúng tôi lưu trữ dãy số trong danh sách; các biến bổ sung là không đáng kể. | 

Thuật toán dễ dàng nằm gọn trong giới hạn 2 giây và giới hạn bộ nhớ 256 MB, ngay cả ở mức lớn nhất`n = 100`. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    n = int(input())
    nums = list(map(int, input().split()))
    first_three = nums[:3]
    parities = [x % 2 for x in first_three]
    majority_parity = 0 if parities.count(0) > 1 else 1
    for i, num in enumerate(nums):
        if num % 2 != majority_parity:
            print(i + 1)
            break
    return output.getvalue().strip()

# provided samples
assert run("5\n2 4 7 8 10\n") == "3", "sample 1"

# custom cases
assert run("3\n2 3 4\n") == "2", "edge: middle number differs"
assert run("3\n1 2 3\n") == "2", "edge: middle number differs, smallest n"
assert run("5\n1 1 1 1 2\n") == "5", "outlier at end"
assert run("4\n2 2 1 2\n") == "3", "outlier in the middle"
assert run("100\n" + " ".join(["2"]*99 + ["3"]) + "\n") == "100", "maximum size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 3 4 | 2 | Ngoại lệ ở giữa, nhỏ n | 
| 3 1 2 3 | 2 | Chuỗi nhỏ nhất có thể, khác ở giữa | 
| 5 1 1 1 1 2 | 5 | Ngoại lệ ở cuối | 
| 4 2 2 1 2 | 3 | Ngoại lệ ở giữa | 
| 100 2 2 ... 3 | 100 | Tối đa n, số cuối khác nhau | 

## Vỏ cạnh 

Nếu ngoại lệ là phần tử đầu tiên, ví dụ:`3 2 4 6`, ba chẵn lẻ đầu tiên là`[1, 0, 0]`, đa số là chẵn, phép lặp tìm phần tử đầu tiên khác nhau và cho ra kết quả`1`. 

Nếu ngoại lệ là cuối cùng, việc lặp lại tiếp tục đến cuối, xuất ra chính xác`n`. 

Nếu ngoại lệ nằm ở giữa, ví dụ:`1 1 2 1`, tính chẵn lẻ của đa số được xác định chính xác từ ba số đầu tiên và việc quét chuỗi sẽ tìm thấy phần tử thứ ba khác. 

Tất cả các trường hợp biên này đều xác nhận rằng việc sử dụng ba phần tử đầu tiên để xác định tính chẵn lẻ của đa số là đủ và vòng quét luôn xác định một số duy nhất vi phạm đa số. 

Điều này hoàn thành một bài xã luận đầy đủ, cẩn thận phù hợp cho một lập trình viên cạnh tranh muốn hiểu sâu vấn đề và tìm lại giải pháp cho các vấn đề ngoại lệ dựa trên tính chẵn lẻ tương tự.
