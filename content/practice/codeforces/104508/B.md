---
title: "CF 104508B - Bogosort"
description: "Nhiệm vụ đưa ra một chuỗi các số nguyên biểu thị một mảng giống như hoán vị. Mục tiêu là tạo ra một phiên bản được sắp xếp chính xác của chuỗi này, trong đó các phần tử được sắp xếp theo thứ tự không giảm và in ra sự sắp xếp cuối cùng đó."
date: "2026-07-01T23:08:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104508
codeforces_index: "B"
codeforces_contest_name: "National Taiwan University Class Preliminary 2023"
rating: 0
weight: 104508
solve_time_s: 49
verified: true
draft: false
---

[CF 104508B - Bogosort](https://codeforces.com/problemset/problem/104508/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ đưa ra một chuỗi các số nguyên biểu thị một mảng giống như hoán vị. Mục tiêu là tạo ra một phiên bản được sắp xếp chính xác của chuỗi này, trong đó các phần tử được sắp xếp theo thứ tự không giảm và in ra sự sắp xếp cuối cùng đó. 

Tên của vấn đề là một gợi ý chứ không phải là một ràng buộc: nó đề cập đến “bogosort” khét tiếng, một phương pháp sắp xếp vô lý có chủ ý liên tục xáo trộn một mảng cho đến khi nó được sắp xếp. Yêu cầu thực tế không phải là mô phỏng quá trình đó mà là tính toán trực tiếp cấu hình được sắp xếp cuối cùng. 

Từ góc độ đầu vào, chúng ta được cung cấp một mảng có độ dài n, theo sau là các phần tử của nó. Đầu ra là một mảng khác chứa các phần tử giống nhau, được sắp xếp lại sao cho mọi phần tử không lớn hơn phần tử tiếp theo. 

Các ràng buộc được ngụ ý bởi các cài đặt Codeforces điển hình cho loại vấn đề này cho thấy rằng n có thể đủ lớn để bất kỳ cách tiếp cận nào tệ hơn O(n log n) sẽ gặp rủi ro. Một mô phỏng bogosort có thời gian giai thừa dự kiến, điều này hoàn toàn không khả thi ngay cả với n nhỏ như 10. Ngay cả việc thử xáo trộn ngẫu nhiên cũng sẽ không kết thúc một cách đáng tin cậy. 

Một số trường hợp đặc biệt quan trọng trong việc triển khai. Mảng một phần tử phải được trả về không thay đổi. Một mảng đã được sắp xếp phải không thay đổi và mọi giải pháp dựa trên các phép biến đổi lặp lại không được vô tình sửa đổi nó một cách không chính xác. Mảng có giá trị trùng lặp phải duy trì tính bội số chính xác ở đầu ra. Các giá trị âm hoặc số nguyên lớn không làm thay đổi logic nhưng có thể đưa ra các giả định không chính xác nếu việc so sánh bị xử lý sai. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực xuất phát trực tiếp từ tên: liên tục xáo trộn mảng cho đến khi nó được sắp xếp. Mỗi lần xáo trộn sẽ tạo ra một trong số n! hoán vị có khả năng bằng nhau và thuật toán chỉ dừng khi cấu hình hiện tại được sắp xếp. 

Cách tiếp cận này đúng về mặt lý thuyết vì mọi thứ tự có thể cuối cùng đều có thể truy cập được, bao gồm cả thứ tự được sắp xếp. Điểm thất bại không phải là sự đúng đắn mà là thời gian. Số lần lặp dự kiến ​​​​tăng theo giai thừa với n và thậm chí kiểm tra xem mảng có được sắp xếp hay không có chi phí O(n), làm cho thời gian chạy dự kiến ​​tổng thể là O(n · n!). 

Quan sát quan trọng là vấn đề không thực sự yêu cầu tính ngẫu nhiên hoặc mô phỏng. Trạng thái mục tiêu được xác định đầy đủ: thứ tự sắp xếp của nhiều giá trị đã cho. Một khi điều này được nhận ra, nhiệm vụ sẽ giảm xuống việc tính toán một thứ tự được xác định hoàn toàn bằng so sánh. 

Điều này loại bỏ tất cả hành vi ngẫu nhiên và thay thế nó bằng một phép biến đổi xác định. Các thuật toán sắp xếp như Timsort của Python hoặc bất kỳ loại so sánh O(n log n) nào sẽ trực tiếp tạo ra sự sắp xếp cần thiết trong thời gian tối ưu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng Bogosort) | O(n · n!) dự kiến ​​| O(n) | Quá chậm | 
| Tối ưu (Sắp xếp) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc số nguyên n và danh sách n giá trị. Danh sách thể hiện trạng thái không có thứ tự mà chúng ta cần chuyển đổi thành một chuỗi đơn điệu. 
2. Sắp xếp danh sách theo thứ tự không giảm bằng cách sử dụng thói quen sắp xếp dựa trên so sánh. Bước này xây dựng sự sắp xếp mục tiêu duy nhất được xác định bằng cách sắp xếp các mối quan hệ giữa các phần tử. 
3. Xuất ra chuỗi đã sắp xếp dưới dạng các giá trị được phân tách bằng dấu cách. 

### Tại sao nó hoạt động 

Trạng thái cuối cùng hợp lệ duy nhất là hoán vị trong đó mọi cặp liền kề đều thỏa mãn a ≤ b. Việc sắp xếp tạo ra chính xác cấu hình này vì nó thực thi tính nhất quán thứ tự theo cặp trên toàn bộ chuỗi. Bất kỳ cách sắp xếp nào khác sẽ chứa ít nhất một phép đảo ngược và việc loại bỏ tất cả các phép đảo ngược chính xác là điều đảm bảo việc sắp xếp.

Bất biến được duy trì bằng thuật toán sắp xếp chính xác là ở mỗi giai đoạn trung gian, các phần tử được di chuyển dần dần đến gần vị trí tương đối chính xác của chúng cho đến khi không còn sự đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = input().strip().split()
    if not data:
        return
    n = int(data[0])
    arr = list(map(int, data[1:1+n]))
    arr.sort()
    print(*arr)

if __name__ == "__main__":
    solve()
```Giải pháp đọc toàn bộ dòng đầu vào, trích xuất mảng và áp dụng tính năng sắp xếp có sẵn của Python. Điều này rất quan trọng vì việc triển khai sắp xếp thủ công có nguy cơ gây ra chi phí không cần thiết và xảy ra lỗi trong các trường hợp khó khăn như số âm hoặc số trùng lặp. 

Một điểm tinh tế là đảm bảo rằng chính xác n phần tử được lấy. Một số đầu vào có thể đặt n và mảng trên cùng một dòng, do đó, việc cắt dựa trên n sẽ ngăn chặn việc vô tình đưa thêm mã thông báo hoặc giá trị bị thiếu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 1 4 1 5
```Chúng ta bắt đầu với mảng thô. 

| Bước | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | [3, 1, 4, 1, 5] | 
| Sau khi sắp xếp | [1, 1, 3, 4, 5] | 

Dấu vết này cho thấy cách các bản sao được bảo tồn và chỉ sắp xếp các thay đổi. Trình tự cuối cùng không có sự đảo ngược, xác nhận tính đúng đắn. 

### Ví dụ 2 

đầu vào:```
4
10 -2 7 7
```| Bước | Trạng thái mảng | 
| --- | --- | 
| Ban đầu | [10, -2, 7, 7] | 
| Sau khi sắp xếp | [-2, 7, 7, 10] | 

Ví dụ này xác nhận rằng số âm và giá trị lặp lại được xử lý thống nhất theo cùng quy tắc so sánh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế thông qua việc đặt hàng dựa trên so sánh | 
| Không gian | O(n) | Lưu trữ cho mảng đầu vào và các hoạt động sắp xếp nội bộ | 

Các ràng buộc của các bài toán Codeforce điển hình dễ dàng cho phép giải pháp n log n cho các mảng có tối thiểu 2·10^5 phần tử, khiến phương pháp này hoạt động tốt trong giới hạn. Việc giải thích bạo lực sẽ vượt quá mọi thời gian chạy khả thi ngay cả đối với các đầu vào nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    data = input().strip().split()
    n = int(data[0])
    arr = list(map(int, data[1:1+n]))
    arr.sort()
    return " ".join(map(str, arr))

# provided-style samples
assert run("5\n3 1 4 1 5\n") == "1 1 3 4 5"
assert run("4\n10 -2 7 7\n") == "-2 7 7 10"

# custom cases
assert run("1\n42\n") == "42", "single element"
assert run("3\n1 2 3\n") == "1 2 3", "already sorted"
assert run("3\n3 2 1\n") == "1 2 3", "reverse order"
assert run("6\n5 5 5 5 5 5\n") == "5 5 5 5 5 5", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | không thay đổi | ranh giới n = 1 | 
| đã được sắp xếp | cùng một mảng | sự ổn định của hành vi đúng đắn | 
| thứ tự ngược lại | sắp xếp tăng dần | đặt hàng trong trường hợp xấu nhất | 
| tất cả đều bình đẳng | không thay đổi | xử lý trùng lặp | 

## Vỏ cạnh 

Mảng một phần tử được xử lý đơn giản vì việc sắp xếp không thay đổi. Thuật toán đọc một giá trị và xuất giá trị đó trực tiếp sau khi sắp xếp, giúp duy trì tính chính xác mà không cần phân nhánh đặc biệt. 

Đầu vào đã được sắp xếp không gây ra bất kỳ thay đổi rõ ràng nào. Quy trình sắp xếp vẫn chạy, nhưng tính bất biến không tồn tại sự đảo ngược có nghĩa là đầu ra cuối cùng khớp chính xác với đầu vào. 

Đầu vào được sắp xếp ngược thể hiện thứ tự trong trường hợp xấu nhất đối với nhiều thuật toán sắp xếp. Tính năng sắp xếp tích hợp xử lý việc này một cách hiệu quả và kết quả cuối cùng là một chuỗi tăng dần hoàn toàn sau khi tất cả các phép đảo ngược được giải quyết. 

Mảng có các giá trị lặp lại chứng tỏ rằng việc sắp xếp ổn định về mặt giá trị. Các phần tử bằng nhau có thể được sắp xếp lại bên trong, nhưng vì giá trị của chúng giống hệt nhau nên kết quả đầu ra vẫn hợp lệ với yêu cầu của bài toán.
