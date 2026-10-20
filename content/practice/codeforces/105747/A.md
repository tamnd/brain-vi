---
title: "CF 105747A - Nước ép Fahim ghét"
description: "Nhiệm vụ mô tả một quy trình sản xuất rất đơn giản: mỗi ly nước ép trái cây hỗn hợp tiêu thụ đúng một quả xoài và một quả cam."
date: "2026-06-22T04:41:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105747
codeforces_index: "A"
codeforces_contest_name: "Bangladesh Olympiad in Informatics 2025 Preliminary Round"
rating: 0
weight: 105747
solve_time_s: 45
verified: true
draft: false
---

[CF 105747A - Nước ép Fahim ghét](https://codeforces.com/problemset/problem/105747/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một quy trình sản xuất rất đơn giản: mỗi ly nước ép trái cây hỗn hợp tiêu thụ đúng một quả xoài và một quả cam. Chúng ta được cho hai số lượng, số xoài có sẵn và số cam có sẵn, và chúng ta phải xác định có thể làm được bao nhiêu ly nước ép hoàn chỉnh. 

Mỗi ly bị hạn chế bởi cả hai thành phần cùng một lúc. Ngay cả khi một quả có nhiều, không thể hình thành một ly nếu không có quả kia, vì vậy yếu tố hạn chế luôn là quả nào hết trước trong quá trình ghép. 

Đầu vào bao gồm hai số nguyên biểu thị số lượng xoài và cam có sẵn. Đầu ra là một số nguyên biểu thị số ly nước ép đầy tối đa có thể được sản xuất. 

Các ràng buộc cho phép giá trị lên tới 10^9. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô phỏng việc tiêu thụ từng ly một. Một mô phỏng tuyến tính sẽ yêu cầu tới 10^9 lần lặp trong trường hợp xấu nhất, vượt xa giới hạn thời gian thông thường. Do đó, bài toán đòi hỏi phải suy luận trong thời gian không đổi. 

Trường hợp khó nhận thấy xuất hiện khi hai đại lượng cực kỳ mất cân bằng. Ví dụ: nếu đầu vào là 1 1000000000, thì chỉ có thể sản xuất một ly mặc dù có lượng cam dư thừa rất lớn. Một vòng lặp tham lam ngây thơ trừ đi nhiều lần cho đến khi một giá trị bằng 0 vẫn sẽ tạo ra câu trả lời đúng về mặt logic, nhưng sẽ không khả thi về mặt tính toán. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ liên tục tạo thành một chiếc ly bằng cách giảm số lượng xoài và cam cho đến khi một trong số chúng trở thành 0. Mỗi lần lặp tương ứng với việc sản xuất một ly, vì vậy số lần lặp bằng câu trả lời. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp phát biểu vấn đề. Tuy nhiên, trong trường hợp xấu nhất, khi cả M và O đều là 10^9 thì sẽ cần 10^9 lần lặp. Mỗi lần lặp thực hiện công việc không đổi, do đó độ phức tạp tổng thể trở thành O(min(M, O)), quá chậm đối với các ràng buộc thực thi thông thường. 

Quan sát quan trọng là mỗi hoạt động tiêu thụ chính xác một đơn vị từ cả hai tài nguyên. Do đó, quá trình dừng chính xác khi một tài nguyên cạn kiệt. Số lượng thao tác hoàn chỉnh hoàn toàn được xác định bởi giá trị nhỏ hơn trong hai giá trị ban đầu. Thay vì mô phỏng mức tiêu thụ từng bước, chúng ta có thể tính kết quả trực tiếp ở mức tối thiểu của hai số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(phút(M, O)) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên biểu thị số lượng xoài và cam hiện có. Các giá trị này xác định số lần chúng ta có thể thực hiện thao tác ghép nối. 
2. Quan sát rằng mỗi thao tác tiêu thụ chính xác một quả xoài và một quả cam, do đó tổng số thao tác bị giới hạn bởi nguồn tài nguyên nào nhỏ hơn. 
3. Tính giá trị nhỏ nhất của hai giá trị. Điều này tương ứng trực tiếp với số ly đầy có thể được tạo thành trước khi hết một thành phần. 
4. Xuất kết quả tối thiểu được tính toán này làm câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Ở mỗi bước làm nước trái cây, cả hai tài nguyên đều giảm đi đúng một đơn vị. Điều này có nghĩa là sự khác biệt giữa hai đại lượng không đổi trong suốt quá trình. Cuối cùng, một trong số chúng đạt đến số 0 trước tiên và tại thời điểm đó, không thể hình thành thêm cặp đầy đủ nào nữa. Do đó, tổng số hoạt động thành công chính xác là số lượng ban đầu của tài nguyên nhỏ hơn, vì đây là nút cổ chai ngay từ đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    m = int(data[0])
    o = int(data[1])
    return str(min(m, o))

if __name__ == "__main__":
    sys.stdout.write(solve())
```Giải pháp đọc hai số nguyên từ đầu vào, chuyển đổi chúng thành dạng số và tính trực tiếp mức tối thiểu của chúng. Kết quả được trả về dưới dạng một chuỗi cho đầu ra. Logic được chứa đầy đủ trong một phép tính duy nhất, tránh hoàn toàn các vòng lặp. Chi tiết triển khai chính là đảm bảo phân tích cú pháp đầu vào và xử lý khoảng trắng chính xác vì đầu vào được cung cấp trên một dòng. 

## Ví dụ đã hoạt động 

### Ví dụ 1: Đầu vào`3 5`| Bước | Xoài | Cam | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 3 | 5 | Trạng thái ban đầu | 
| 1 | 2 | 4 | Mẫu 1 ly | 
| 2 | 1 | 3 | Mẫu 1 ly | 
| 3 | 0 | 2 | Mẫu 1 ly | 

Lúc này xoài đã cạn kiệt nên không thể làm thêm ly nào nữa. Quá trình dừng lại ở 3 ly. Điều này khẳng định nguồn tài nguyên hạn chế là xoài. 

### Ví dụ 2: Nhập liệu`4 4`| Bước | Xoài | Cam | Hành động | 
| --- | --- | --- | --- | 
| Bắt đầu | 4 | 4 | Trạng thái ban đầu | 
| 1 | 3 | 3 | Mẫu 1 ly | 
| 2 | 2 | 2 | Mẫu 1 ly | 
| 3 | 1 | 1 | Mẫu 1 ly | 
| 4 | 0 | 0 | Mẫu 1 ly | 

Cả hai tài nguyên đều cạn kiệt đồng thời nên cả 4 cặp có thể đều được hình thành. Điều này thể hiện trường hợp cân bằng trong đó không có tài nguyên nào bị lãng phí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một thao tác tối thiểu trong thời gian không đổi được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp với các ràng buộc vì nó thực hiện một số thao tác cố định bất kể kích thước đầu vào, ngay cả đối với các giá trị lên tới 10^9. 

## Trường hợp thử nghiệm```python
import sys, io

def solve():
    data = sys.stdin.readline().strip().split()
    m = int(data[0])
    o = int(data[1])
    return str(min(m, o))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve().strip()

# provided samples
assert run("3 5\n") == "3", "sample 1"
assert run("4 4\n") == "4", "sample 2"

# custom cases
assert run("1 1000000000\n") == "1", "single mango limits output"
assert run("1000000000 1\n") == "1", "single orange limits output"
assert run("0 0\n") == "0", "both zero edge case"
assert run("7 2\n") == "2", "unequal small values"
assert run("10 10\n") == "10", "equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1000000000 | 1 | mất cân bằng cực độ, giá trị đầu tiên nhỏ | 
| 1000000000 1 | 1 | mất cân bằng đối xứng | 
| 0 0 | 0 | trường hợp thoái hóa không có tài nguyên | 
| 7 2 | 2 | trường hợp bất đẳng thức tổng quát | 
| 10 10 | 10 | trường hợp bình đẳng cân bằng | 

## Vỏ cạnh 

Đối với các đầu vào cực kỳ sai lệch như`1 1000000000`, một vòng lặp đơn giản sẽ thử một tỷ lần lặp. Giải pháp tối ưu thay vì tính toán trực tiếp`min(1, 1000000000)`và trả về 1 ngay lập tức, tránh mọi mô phỏng. 

Đối với đầu vào lớn cân bằng như`1000000000 1000000000`, cả hai tài nguyên đều cạn kiệt cùng nhau. Thuật toán trả về chính xác giá trị đầy đủ mà không có bất kỳ nguy cơ tràn hoặc lỗi lặp nào, vì nó chỉ dựa vào so sánh theo thời gian không đổi thay vì phép trừ lặp lại.
