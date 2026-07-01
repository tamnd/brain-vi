---
title: "CF 104426N - Chứng sợ cá"
description: "Chúng ta được cung cấp một chuỗi các quan sát độc lập, mỗi quan sát đại diện cho một hồ nước và số lượng cá trong đó. Đối với mỗi hồ, chúng ta phải quyết định xem Kaitokid có thể đến thăm nó một cách an toàn hay không. An toàn được xác định một cách rất nghiêm ngặt: một hồ nước chỉ được chấp nhận khi nó không chứa cá nào cả."
date: "2026-06-30T19:09:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "N"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 89
verified: false
draft: false
---

[CF 104426N - Chứng sợ Ichthyo](https://codeforces.com/problemset/problem/104426/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các quan sát độc lập, mỗi quan sát đại diện cho một hồ nước và số lượng cá trong đó. Đối với mỗi hồ, chúng ta phải quyết định xem Kaitokid có thể đến thăm nó một cách an toàn hay không. An toàn được xác định một cách rất nghiêm ngặt: một hồ nước chỉ được chấp nhận khi nó không chứa cá nào cả. Bất kỳ số lượng cá tích cực nào cũng làm cho hồ không phù hợp. 

Đầu vào bao gồm một số nguyên T mô tả số lượng hồ được báo cáo. Sau đó, chúng ta đọc T số nguyên, mỗi số được giới hạn từ 0 đến 100. Đối với mỗi giá trị này, chúng tôi đưa ra một chuỗi quyết định cho biết hồ cụ thể đó có an toàn hay không. 

Các ràng buộc đủ nhỏ để tổng lượng tính toán không đáng kể. Ngay cả ở mức T tối đa là 10^4, chúng tôi chỉ thực hiện kiểm tra liên tục trên mỗi hồ. Điều này có nghĩa là quét tuyến tính trực tiếp là đủ và mọi giải pháp có độ phức tạp O(T) sẽ chạy thoải mái trong giới hạn. 

Không có những phức tạp về cấu trúc tiềm ẩn như sự phụ thuộc thứ tự hoặc tương tác giữa các hồ. Mỗi hồ có thể được xử lý độc lập, đó là sự đơn giản hóa quan trọng. 

Trường hợp cạnh duy nhất quan trọng là khi số lượng cá chính xác bằng 0. Việc triển khai bất cẩn có thể vô tình coi số 0 là giả hoặc bỏ qua nó không chính xác tùy thuộc vào cấu trúc ngôn ngữ, nhưng về mặt logic, đây là trường hợp duy nhất tạo ra phản hồi tích cực. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ vẫn sẽ xem xét từng hồ một, kiểm tra xem số lượng cá của nó có bằng 0 hay không và in kết quả. Không có cách nào để giảm số lần kiểm tra vì mỗi hồ phải được kiểm tra ít nhất một lần để đưa ra sản lượng tương ứng. Ngay cả khi chúng ta cố gắng lưu trữ tất cả các giá trị trước và xử lý chúng sau, tổng công việc vẫn tỷ lệ thuận với T. 

"Tối ưu hóa" thực sự duy nhất ở đây là nhận ra rằng không cần phải có cấu trúc dữ liệu hoặc tiền xử lý bổ sung. Quy tắc quyết định là một so sánh duy nhất cho mỗi phần tử: nếu x bằng 0, in CÓ, nếu không in NO. Cấu trúc của vấn đề đã phù hợp với mẫu đánh giá tối ưu. 

Chế độ xem vũ phu và chế độ xem tối ưu trùng khớp với nhau, nhưng nhận thức quan trọng là không có tính toán tổng hợp, sắp xếp hoặc tiền tố nào thay đổi bất cứ điều gì. Việc tính toán vốn đã là O(T). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Kiểm tra trực tiếp trên mỗi hồ | O(T) | O(1) | Đã chấp nhận | 
| Bất kỳ chuyển đổi thay thế nào | O(T) | O(T) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng hồ một cách độc lập theo thứ tự được đưa ra. 

1. Đọc số nguyên T, cho biết chúng ta phải đánh giá bao nhiêu hồ. Điều này xác định số lần lặp mà chúng tôi sẽ thực hiện. 
2. Với mỗi số nguyên T tiếp theo, hãy đọc giá trị x biểu thị số lượng cá trong một hồ. 
3. Kiểm tra xem x có bằng 0 hay không. Đây là điều kiện xác định về an toàn, vì chỉ những hồ trống mới được chấp nhận. 
4. Nếu x bằng 0, xuất ngay YES. Nếu không thì xuất ra NO. 
5. Lặp lại quá trình này cho đến khi tất cả hồ T đã được xử lý. 

Lý do đằng sau mỗi bước là không có hồ nào ảnh hưởng đến hồ nào khác nên việc đánh giá ngay lập tức vừa an toàn vừa tối ưu. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên sự tương đương trực tiếp giữa định nghĩa bài toán và điều kiện x == 0. Thuật toán đánh giá vị từ này chính xác một lần cho mỗi giá trị đầu vào và tạo ra nhãn tương ứng. Vì mỗi quyết định là độc lập và được xác định đầy đủ bởi một số nguyên duy nhất nên không có khả năng xảy ra sự không nhất quán hoặc hiệu ứng tương tác giữa các lần lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input().strip())
    for _ in range(t):
        x_line = input().strip()
        if not x_line:
            x_line = input().strip()
        x = int(x_line)
        if x == 0:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai đọc T trước rồi xử lý từng hồ trong một vòng lặp. Mỗi dòng đầu vào được phân tích cú pháp thành một số nguyên và được kiểm tra bằng 0. Điều kiện là cốt lõi của giải pháp và phản ánh trực tiếp định nghĩa toán học của một hồ hợp lệ. 

Chi tiết triển khai tinh tế duy nhất là đảm bảo rằng mỗi dòng đầu vào được loại bỏ đúng cách trước khi chuyển đổi, điều này tránh được các vấn đề về khoảng trắng ở cuối hoặc số lần đọc trống trong một số môi trường. Kết quả đầu ra được in ngay lập tức, tránh việc lưu trữ kết quả một cách không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1
5
0
```Chúng tôi theo dõi từng hồ một cách tuần tự. 

| Bước | x | Điều kiện (x == 0) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | Sai | KHÔNG | 
| 2 | 5 | Sai | KHÔNG | 
| 3 | 0 | Đúng | CÓ | 

Dấu vết cho thấy chỉ trường hợp 0 ​​mới kích hoạt sự chấp nhận, trong khi tất cả các giá trị dương đều bị từ chối một cách nhất quán. 

### Ví dụ 2 

đầu vào:```
4
0
0
2
1
```| Bước | x | Điều kiện (x == 0) | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 0 | Đúng | CÓ | 
| 2 | 0 | Đúng | CÓ | 
| 3 | 2 | Sai | KHÔNG | 
| 4 | 1 | Sai | KHÔNG | 

Điều này xác nhận rằng nhiều hồ hợp lệ được xử lý độc lập mà không có bất kỳ sự chuyển giao trạng thái nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi hồ được kiểm tra chính xác một lần bằng cách so sánh theo thời gian không đổi | 
| Không gian | O(1) | Không cần bộ nhớ phụ ngoài phân tích cú pháp đầu vào | 

Thời gian chạy tỷ lệ tuyến tính với số lượng hồ, đây là mức tối ưu vì mọi giá trị đầu vào phải được đọc ít nhất một lần. Việc sử dụng bộ nhớ không đổi vì chúng tôi không lưu trữ toàn bộ mảng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run("3\n1\n5\n0\n") == "NO\nNO\nYES"

# minimum case
assert run("1\n0\n") == "YES"

# all non-zero
assert run("3\n1\n2\n3\n") == "NO\nNO\nNO"

# alternating values
assert run("4\n0\n1\n0\n1\n") == "YES\nNO\nYES\nNO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 0 | CÓ | Trường hợp hợp lệ tối thiểu | 
| 1, 1 | KHÔNG | Trường hợp từ chối tối thiểu | 
| 1 2 3 | KHÔNG KHÔNG KHÔNG | Tất cả xử lý khác không | 
| 0 1 0 1 | CÓ KHÔNG CÓ KHÔNG | Sự đúng đắn xen kẽ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi số lượng cá chính xác bằng 0. Thuật toán coi điều này một cách rõ ràng là điều kiện chấp nhận duy nhất. Đối với đầu vào`0`, điều kiện x == 0 đánh giá là đúng và đầu ra là CÓ, xác nhận việc xử lý đúng ranh giới giữa hồ được chấp nhận và không được chấp nhận. 

Một trường hợp cạnh khác là khi tất cả các giá trị khác 0. Ví dụ, đầu vào`3 1 2 3`tạo ra NO cho mỗi bước vì không có giá trị nào thỏa mãn điều kiện chấp nhận. Thuật toán không dựa vào tính tích cực hay tiêu cực mà chỉ dựa vào sự bình đẳng chính xác nên không có sự mơ hồ. 

Cuối cùng, đầu vào nhỏ nhất có thể có T = 1 được xử lý chính xác vì vòng lặp thực hiện chính xác một lần và tạo ra một quyết định duy nhất mà không yêu cầu bất kỳ logic khởi tạo hoặc tích lũy nào.
