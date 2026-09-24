---
title: "CF 104804A - \u0420\u0435\u0441\u0443\u0440\u0441\u044b"
description: "Mỗi trường hợp thử nghiệm mô tả một tập hợp các loại tài nguyên được thu thập trong phiên trò chơi board. Đối với mỗi loại, chúng tôi được cung cấp số lượng đơn vị tài nguyên đó đã được thu thập và giá trị của một đơn vị."
date: "2026-06-28T13:24:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104804
codeforces_index: "A"
codeforces_contest_name: "Central Russia Regional Contest, 2022, Qualification Contest"
rating: 0
weight: 104804
solve_time_s: 62
verified: true
draft: false
---

[CF 104804A - \u0420\u0435\u0441\u0443\u0440\u0441\u044b](https://codeforces.com/problemset/problem/104804/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm mô tả một tập hợp các loại tài nguyên được thu thập trong phiên trò chơi board. Đối với mỗi loại, chúng tôi được cung cấp số lượng đơn vị tài nguyên đó đã được thu thập và giá trị của một đơn vị. Nhiệm vụ là tính tổng số điểm bằng cách tổng hợp các đóng góp từ tất cả các loại tài nguyên. 

Nói một cách cụ thể hơn, mỗi dòng cho một cặp số. Số đầu tiên là số lượng mục thuộc một loại cụ thể và số thứ hai là giá trị của một mục như vậy. Sự đóng góp của loại đó vào điểm số cuối cùng chỉ đơn giản là sản phẩm của họ và câu trả lời là tổng của những đóng góp này đối với tất cả các loại. 

Các ràng buộc cho phép tối đa 100000 loại tài nguyên và mỗi giá trị có thể lớn tới 100000. Điều này ngay lập tức loại trừ mọi giải pháp thử bất kỳ điều gì ngoài một lần truyền tuyến tính duy nhất qua đầu vào. Ngay cả cách tiếp cận O(n log n) cũng không cần thiết vì không có cấu trúc nào để khai thác ngoài việc tổng hợp trực tiếp. Cách tiếp cận khả thi duy nhất là tích lũy số tiền khi chúng ta đọc dữ liệu đầu vào. 

Một vấn đề nhỏ có thể phát sinh khi triển khai bất cẩn là tràn số nguyên trong các ngôn ngữ có số nguyên có chiều rộng cố định. Ví dụ: nếu tất cả đầu vào ở mức tối đa thì một sản phẩm có thể đạt tới 10^10 và tổng tối đa 10^5 các giá trị đó có thể đạt tới 10^15. Trong Python điều này là an toàn, nhưng trong các ngôn ngữ khác, điều này yêu cầu số nguyên 64 bit. 

Một sai lầm tiềm ẩn khác là cố gắng tách tích lũy thành hai mảng và xử lý chúng sau đó một cách không cần thiết, điều này làm tăng thêm chi phí nhưng không thay đổi tính chính xác. Vì mỗi dòng là độc lập nên việc trì hoãn tính toán không mang lại lợi ích gì. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là mở rộng từng đơn vị tài nguyên riêng lẻ và sau đó gán từng giá trị một. Điều đó có nghĩa là đối với mỗi loại, về mặt khái niệm, chúng tôi tạo một danh sách có kích thước a_i và gán b_i cho từng phần tử. Điều này đúng nhưng ngay lập tức trở nên không khả thi khi a_i lớn, vì tổng số phần tử mở rộng có thể đạt tới 10^10 trong trường hợp xấu nhất. 

Quan sát quan trọng là tất cả các đơn vị cùng loại đều giống nhau về giá trị đóng góp. Không có sự tương tác giữa các loại tài nguyên và không có thứ tự hay ràng buộc nào giữa chúng. Điều này thu gọn bài toán thành việc tính tổng có trọng số trong đó mỗi trọng số chỉ đơn giản là a_i và mỗi giá trị là b_i. Thay vì mở rộng, chúng tôi nhân lên và tích lũy trực tiếp. 

Điều này làm giảm toàn bộ nhiệm vụ thành một lần chuyển qua đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mở rộng lực lượng vũ phu | O(∑a_i) | O(∑a_i) | Quá chậm | 
| Tổng trực tiếp | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`total`về không. Điều này sẽ lưu trữ tổng số tiền đóng góp. 
2. Đọc số lượng loại tài nguyên`n`. 
3. Đối với mỗi điều tiếp theo`n`dòng, đọc một cặp`(a_i, b_i)`. 
4. Tính phần đóng góp của loại này như sau:`a_i * b_i`. Giá trị này thể hiện tổng giá trị của tất cả các mục thuộc loại đó cộng lại. 
5. Thêm phần đóng góp này vào`total`. 
6. Sau khi xử lý tất cả các dòng, xuất ra`total`. 

Lựa chọn thiết kế chính là thực hiện phép nhân tại thời điểm đọc thay vì lưu trữ giá trị. Điều này tránh việc sử dụng bộ nhớ không cần thiết và đảm bảo việc tính toán hoàn toàn tuyến tính theo số dòng đầu vào. 

### Tại sao nó hoạt động 

Mỗi loại tài nguyên độc lập với tất cả các loại khác và mọi đơn vị trong một loại đều có giá trị giống nhau. Do đó, tổng số điểm là tổng của các nhóm rời rạc, trong đó mỗi nhóm đóng góp chính xác`a_i * b_i`. Vì phép cộng có tính chất kết hợp và giao hoán nên việc đóng góp tích lũy dần dần duy trì tính đúng đắn bất kể thứ tự đầu vào. Không có điều khoản tương tác nào tồn tại nên không cần trạng thái bổ sung ngoài tổng số tiền hiện có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    total = 0
    for _ in range(n):
        a, b = map(int, input().split())
        total += a * b
    print(total)

if __name__ == "__main__":
    solve()
```Lời giải đọc mỗi cặp một lần và ngay lập tức xếp nó vào câu trả lời. Phép nhân được thực hiện trước phép cộng để đảm bảo chúng ta không bao giờ cần lưu trữ các biểu diễn mở rộng trung gian. Việc sử dụng số nguyên chính xác tùy ý của Python đảm bảo tính chính xác ngay cả khi tổng vượt quá giới hạn 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 3
2 6
3 1
4 5
5 10
```| Bước | a_i | b_i | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 3 | 
| 2 | 2 | 6 | 12 | 15 | 
| 3 | 3 | 1 | 3 | 18 | 
| 4 | 4 | 5 | 20 | 38 | 
| 5 | 5 | 10 | 50 | 88 | 

Tổng cuối cùng là 88, phù hợp với kết quả mong đợi. Dấu vết này cho thấy mỗi loại đóng góp và tích lũy độc lập như thế nào mà không bị can thiệp. 

### Mẫu 2 

đầu vào:```
5
0 1
0 3
4 4
2 7
1 1
```| Bước | a_i | b_i | Đóng góp | Tổng cộng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | 0 | 
| 2 | 0 | 3 | 0 | 0 | 
| 3 | 4 | 4 | 16 | 16 | 
| 4 | 2 | 7 | 14 | 30 | 
| 5 | 1 | 1 | 1 | 31 | 

Ví dụ này nhấn mạnh rằng số 0 có thể bỏ qua đóng góp một cách hiệu quả và thuật toán xử lý chúng một cách tự nhiên mà không cần trường hợp đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi loại tài nguyên được xử lý chính xác một lần với số học thời gian không đổi | 
| Không gian | O(1) | Chỉ có một biến tích lũy duy nhất được sử dụng | 

Các ràng buộc cho phép tối đa 100000 mục nhập và quét tuyến tính với số học đơn giản cũng nằm trong giới hạn thông thường. Không có cấu trúc bổ sung hoặc tiền xử lý được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""5
1 3
2 6
3 1
4 5
5 10
""") == "88"

assert run("""5
0 1
0 3
4 4
2 7
1 1
""") == "31"

# custom cases
assert run("""1
0 100
""") == "0"

assert run("""3
100000 100000
0 5
1 1
""") == "10000000001"

assert run("""4
1 1
1 1
1 1
1 1
""") == "4"

assert run("""2
99999 99999
1 0
""") == "9999800001"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cặp số 0 đơn | 0 | xử lý đóng góp bằng không | 
| giá trị lớn | 10000000001 | độ chính xác của thang đo tràn | 
| giá trị nhỏ thống nhất | 4 | tích lũy cơ bản đúng đắn | 
| hệ số 0 hỗn hợp | 9999800001 | bỏ qua các điều khoản có giá trị bằng 0 | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả số lượng tài nguyên bằng 0. Đối với đầu vào:```
3
0 10
0 20
0 30
```mỗi khoản đóng góp bằng 0, do đó tổng hoạt động vẫn ở mức 0 trong suốt quá trình thực hiện. Thuật toán thực hiện ba phép nhân nhưng tất cả các kết quả đều bằng 0 và đầu ra cuối cùng vẫn là 0 mà không có bất kỳ sự phân nhánh đặc biệt nào. 

Một trường hợp khác là khi chỉ tồn tại một loại:```
1
12345 67890
```Thuật toán thực hiện một phép nhân đơn và đưa ra kết quả trực tiếp. Không có vấn đề khởi tạo nào phát sinh do bộ tích lũy bắt đầu từ 0 và bản cập nhật duy nhất sẽ ghi lại câu trả lời đầy đủ một cách chính xác. 

Trường hợp thứ ba là khi các giá trị ở mức tối đa. Mặc dù các sản phẩm trung gian có thể lớn, Python xử lý độ chính xác tùy ý một cách tự nhiên, do đó việc tích lũy lặp đi lặp lại không bị tràn hoặc mất độ chính xác.
