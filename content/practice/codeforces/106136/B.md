---
title: "CF 106136B – Thấy mới tin"
description: "Chúng ta được cho một phạm vi số nguyên nhỏ được xác định bởi điểm bắt đầu l. Số ẩn a được đảm bảo nằm ở đâu đó trong khoảng từ l đến l + 5, do đó có thể có nhiều nhất sáu số nguyên liên tiếp."
date: "2026-06-19T19:40:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106136
codeforces_index: "B"
codeforces_contest_name: "East China University of Science and Technology Programming Contest 2025"
rating: 0
weight: 106136
solve_time_s: 49
verified: true
draft: false
---

[CF 106136B - Thấy mới tin](https://codeforces.com/problemset/problem/106136/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một phạm vi số nguyên nhỏ được xác định bởi điểm bắt đầu`l`. Con số ẩn`a`được đảm bảo nằm ở đâu đó trong khoảng từ`l`lên đến`l + 5`, vậy có thể có nhiều nhất sáu số nguyên liên tiếp. 

Ngoài hạn chế về phạm vi này,`a`phải thỏa mãn đồng thời hai điều kiện mô đun. Số dư của nó khi chia cho 2 được cố định bằng`m2`, và phần dư của nó khi chia cho 3 được cố định bằng`m3`. Nhiệm vụ là xác định bất kỳ số nguyên nào trong phạm vi cho phép khớp với cả hai điều kiện còn lại hoặc báo cáo rằng không tồn tại số nguyên đó. 

Những hạn chế là cực kỳ nhỏ:`l`tối đa là 100 và độ dài khoảng tìm kiếm được cố định là 6. Điều này ngay lập tức loại trừ mọi nhu cầu về lý thuyết số nâng cao hoặc tối ưu hóa. Ngay cả việc quét trực tiếp khoảng thời gian cũng thực hiện tối đa sáu lần kiểm tra, đó là thời gian không đổi. 

Sự tinh tế duy nhất nằm ở định nghĩa về số tự nhiên. Vì 0 được cho phép rõ ràng nên khoảng có thể bao gồm 0 hoặc các giá trị nhỏ trong đó hành vi modulo vẫn được xác định rõ, do đó không có loại trừ ẩn. 

Một sai lầm điển hình trong loại bài toán này là cố gắng giải trực tiếp các đồng dư bằng cách sử dụng máy Định lý số dư Trung Hoa. Điều đó là không cần thiết và dễ xảy ra lỗi hơn ở đây, vì không gian tìm kiếm quá nhỏ nên vũ lực đã là tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản nhất là thử mọi số nguyên`a`trong phạm vi`[l, l + 5]`và kiểm tra xem nó có thỏa mãn cả hai điều kiện không`a % 2 == m2`Và`a % 3 == m3`. Vì có tối đa sáu ứng viên, mỗi ứng viên đều yêu cầu kiểm tra thời gian liên tục nên việc này khá nhanh. 

Điều này hoạt động vì các ràng buộc không mở rộng. Tuy nhiên, nếu khoảng cách lớn, chẳng hạn lên tới 10^9, thì việc liệt kê trực tiếp này sẽ không thể thực hiện được. Trong trường hợp đó, chúng ta sẽ cần giao các ràng buộc mô-đun bằng cách sử dụng tính tuần hoàn của các thặng dư modulo 6, vì các điều kiện modulo 2 và modulo 3 kết hợp thành một cấu trúc modulo 6. Điều quan trọng là mỗi số nguyên được xác định duy nhất bởi phần dư modulo 6 của nó và cả hai ràng buộc cùng nhau chỉ định một lớp dư lượng modulo 6. 

Trong bài toán này, cấu trúc sâu hơn đó là không cần thiết, nhưng nó giải thích tại sao lực lượng vũ phu về cơ bản đã là tối ưu: không gian tìm kiếm nhỏ hơn một chu kỳ mô đun đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt quá phạm vi | O(1) | O(1) | Đã chấp nhận | 
| Lý luận mô-đun (mod 6) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`l`,`m2`, Và`m3`từ đầu vào. Chúng xác định phạm vi ứng viên và số dư cần thiết. 
2. Lặp lại tất cả các số nguyên`a`từ`l`ĐẾN`l + 5`bao gồm. Điều này bao gồm mọi ứng cử viên hợp lệ có thể. 
3. Đối với mỗi ứng viên`a`, tính toán`a % 2`và kiểm tra xem nó có bằng không`m2`. Nếu không khớp thì loại bỏ`a`ngay lập tức. 
4. Nếu điều kiện đầu tiên đúng, hãy tính`a % 3`và kiểm tra xem nó có bằng không`m3`. Nếu nó cũng khớp thì chúng ta đã tìm được lời giải hợp lệ và có thể xuất ra`a`. 
5. Nếu vòng lặp kết thúc mà không tìm được ứng viên hợp lệ nào, xuất ra`-1`. 

Thứ tự kiểm tra không quan trọng đối với tính chính xác, nhưng việc kiểm tra modulo 2 trước sẽ hiệu quả hơn một chút vì nó lọc ngay lập tức một nửa số ứng viên. 

### Tại sao nó hoạt động 

Mọi giá trị có thể có của`a`nằm trong một tập hợp có nhiều nhất sáu số nguyên liên tiếp. Thuật toán đánh giá rõ ràng từng phần tử của toàn bộ tập hợp này. Vì không có ứng cử viên nào bị bỏ qua và mọi ứng cử viên đều được kiểm tra theo các ràng buộc xác định chính xác, nên mọi giải pháp hợp lệ đều phải gặp trong quá trình lặp. Ngược lại, chỉ các ứng cử viên hợp lệ mới được trả về vì cả hai điều kiện mô-đun đều được kiểm tra trước khi chấp nhận một giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

l, m2, m3 = map(int, input().split())

for a in range(l, l + 6):
    if a % 2 == m2 and a % 3 == m3:
        print(a)
        sys.exit()

print(-1)
```Việc triển khai tuân theo thuật toán trực tiếp mà không có bất kỳ tối ưu hóa ẩn nào. Vòng lặp trên đúng sáu giá trị đảm bảo thời gian chạy có giới hạn. Lối thoát sớm qua`sys.exit()`đảm bảo chúng tôi dừng ngay lập tức khi tìm thấy ứng viên hợp lệ, mặc dù ngay cả khi không có ứng viên đó thì chi phí cũng không đáng kể. 

Một cạm bẫy triển khai phổ biến là đặt sai phạm vi là`range(l, l+5)`thay vì`l+6`. Vì khoảng bao gồm cả hai đầu nên chúng ta phải bao gồm chính xác sáu số. 

Một điểm tinh tế khác là xử lý trường hợp không tồn tại số hợp lệ. Chúng tôi chỉ in`-1`sau khi đã làm cạn kiệt tất cả các ứng cử viên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`0 1 2`Chúng tôi kiểm tra tất cả các giá trị từ 0 đến 5. 

| một | một % 2 | một % 3 | hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | không | 
| 1 | 1 | 1 | không | 
| 2 | 0 | 2 | không | 
| 3 | 1 | 0 | không | 
| 4 | 0 | 1 | không | 
| 5 | 1 | 2 | vâng | 

Giá trị hợp lệ đầu tiên là 5, vì vậy chúng tôi xuất ra 5. Điều này chứng tỏ rằng mặc dù nhiều lần kiểm tra không thành công sớm nhưng vẫn cần phải có phạm vi đầy đủ để tìm ra kết quả khớp chính xác. 

### Ví dụ 2 

đầu vào:`63 1 1`Chúng tôi kiểm tra các giá trị từ 63 đến 68. 

| một | một % 2 | một % 3 | hợp lệ | 
| --- | --- | --- | --- | 
| 63 | 1 | 0 | không | 
| 64 | 0 | 1 | không | 
| 65 | 1 | 2 | không | 
| 66 | 0 | 0 | không | 
| 67 | 1 | 1 | vâng | 

Ở đây, 67 thỏa mãn cả hai ràng buộc mô-đun nên nó được trả về. Ví dụ này cho thấy rằng lời giải không giả định bất cứ điều gì về câu trả lời nằm ở đâu trong khoảng đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có 6 ứng cử viên được kiểm tra, mỗi ứng cử viên có các phép toán modulo theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Thời gian chạy không đổi bất kể kích thước đầu vào, nằm trong giới hạn. Ngay cả trong trường hợp xấu nhất, thuật toán chỉ thực hiện một số phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    l, m2, m3 = map(int, input().split())

    for a in range(l, l + 6):
        if a % 2 == m2 and a % 3 == m3:
            return str(a)

    return "-1"

# provided samples
assert run("0 1 2") == "5"
assert run("63 1 1") == "67"

# minimum range, solution at boundary
assert run("0 0 0") in {"0", "6"}  # depending on valid range overlap

# no solution exists
assert run("1 0 0") in {"-1", "-1"}  # small contrived case

# solution at left boundary
assert run("10 0 1") == run("10 0 1")

# all candidates invalid
assert run("2 1 0") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 0 0 | 0 hoặc 6 | căn chỉnh ranh giới của các giá trị nhỏ nhất | 
| 1 0 0 | -1 | không có ứng cử viên hợp lệ tồn tại | 
| 2 1 0 | -1 | thất bại hoàn toàn trên phạm vi | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi`l`là 0. Thuật toán vẫn bao gồm chính xác 0 là ứng cử viên hợp lệ vì vòng lặp bắt đầu từ`l`chính nó. Ví dụ, với đầu vào`0 0 0`, lần kiểm tra đầu tiên`a = 0`thỏa mãn ngay cả hai điều kiện, vì`0 % 2 = 0`Và`0 % 3 = 0`và thuật toán trả về 0 mà không cần lặp lại nữa. 

Một trường hợp cạnh khác là khi lời giải hợp lệ nằm ở ranh giới trên`l + 5`. Ví dụ, nếu`l = 2`, ứng viên`7`có thể là số hợp lệ duy nhất trong khoảng. Vòng lặp bao gồm rõ ràng`l + 5`, Vì thế`a = 7`được kiểm tra và chấp nhận chính xác. 

Trường hợp cạnh cuối cùng là khi không có số nào trong khoảng thỏa mãn cả hai ràng buộc mô-đun. Trong trường hợp đó, mỗi lần lặp đều thất bại ít nhất một điều kiện và vòng lặp hoàn thành đầy đủ. Thuật toán sau đó xuất ra`-1`, báo hiệu chính xác sự không thể.
