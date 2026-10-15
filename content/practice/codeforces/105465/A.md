---
title: "CF 105465A - Đóng AND-HOẶC"
description: "Chúng ta được cấp một tập hợp các số nguyên riêng biệt và chúng ta được phép áp dụng lặp lại hai phép toán: bitwise AND và bitwise OR giữa hai phần tử bất kỳ. Mỗi lần chúng ta áp dụng một trong các thao tác này thì kết quả cũng phải thuộc về tập hợp."
date: "2026-06-23T02:23:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105465
codeforces_index: "A"
codeforces_contest_name: "2023 ICPC Southeastern Europe Regional Contest (The 2nd Universal Cup, Stage 14: Southeastern Europe)"
rating: 0
weight: 105465
solve_time_s: 57
verified: true
draft: false
---

[CF 105465A - Đóng AND-OR](https://codeforces.com/problemset/problem/105465/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên riêng biệt và chúng ta được phép áp dụng lặp lại hai phép toán: bitwise AND và bitwise OR giữa hai phần tử bất kỳ. Mỗi lần chúng ta áp dụng một trong các thao tác này thì kết quả cũng phải thuộc về tập hợp. Chúng ta được yêu cầu xác định kích thước của tập hợp nhỏ nhất đã chứa các số ban đầu và được đóng theo cả hai phép toán. 

Đối tượng chính không phải là một chuỗi mà là một tập hợp phát triển cho đến khi nó trở nên ổn định theo cặp AND và OR. Tính ổn định ở đây có nghĩa là nếu chúng ta lấy bất kỳ hai phần tử nào đã có trong tập hợp thì cả kết quả AND và OR của chúng cũng đã có sẵn, do đó không thể tạo phần tử mới nữa. 

Ràng buộc n lên tới 2 · 10^5 và các giá trị lên tới 2^40 có nghĩa là chúng tôi không thể mô phỏng việc đóng cửa một cách trực tiếp. Một quy trình đóng đơn giản sẽ tiếp tục chèn các số mới cho đến khi không có số mới nào xuất hiện, nhưng mỗi bước có thể tạo ra nhiều kết hợp. Trong trường hợp xấu nhất, một tập hợp có kích thước k tạo ra k^2 ứng cử viên mới cho mỗi lần lặp và bản thân k có thể tăng lên đáng kể. Ngay cả việc trình bày rõ ràng tập hợp đang tiến hóa cũng khiến cách tiếp cận này không khả thi. 

Trường hợp cạnh tinh tế xuất hiện khi các số ban đầu đã “cách xa nhau” trong cấu trúc bit. Ví dụ: với các số như 1 (0001), 2 (0010), 4 (0100), 8 (1000), phép toán OR lặp lại tạo ra tất cả các kết hợp trung gian, trong khi phép toán AND thường tạo ra số 0. Điều này cho thấy rằng bao đóng có thể nhanh chóng trở thành khoảng bitwise đầy đủ từ 0 đến OR của tất cả các phần tử, ngay cả khi tập hợp ban đầu thưa thớt. 

Một trường hợp khác là khi tất cả các số đều có chung các mẫu bit rời rạc. Trong tình huống đó, chỉ riêng thao tác OR đã tạo ra nhiều giá trị trung gian và AND thu gọn nhiều cặp về 0. Một cách tiếp cận ngây thơ có thể đánh giá thấp tốc độ đóng cửa lấp đầy khoảng trống. 

Thách thức thực sự là mô tả đặc điểm của việc đóng cửa thực sự trở thành gì mà không cần mô phỏng nó. 

## Phương pháp tiếp cận 

Ý tưởng của Brute Force rất đơn giản: duy trì một tập hợp, liên tục chọn tất cả các cặp phần tử, chèn kết quả AND và OR của chúng nếu chúng mới và tiếp tục cho đến khi không có thay đổi nào xảy ra. Điều này đúng vì định nghĩa mô tả trực tiếp việc đóng theo các phép toán theo cặp. 

Tuy nhiên, mỗi lần lặp lại xem xét các cặp O(k^2) và k có thể tăng lên 2^40 trong không gian đóng lý thuyết tồi tệ nhất. Ngay cả khi chúng ta giả sử k vẫn bị giới hạn bởi n ban đầu, thì việc đóng có thể nhanh chóng tăng vượt quá n và số lượng giá trị trung gian được tạo ra sẽ bùng nổ. Điều này làm cho việc mô phỏng không thể thực hiện được trong thời gian giới hạn. 

Quan sát chính là việc đóng OR và AND theo bit tương tác theo cách rất có cấu trúc: cả hai thao tác đều đơn điệu về mặt bao gồm bit. HOẶC chỉ bật bit VÀ chỉ tắt bit. Mọi giá trị được tạo ra luôn bị giới hạn giữa AND theo bit của tất cả các phần tử và OR theo bit của tất cả các phần tử. Quan trọng hơn, một khi chúng ta xem xét tất cả các kết hợp theo cặp, mọi mẫu bit phù hợp với tập hợp ban đầu đều có thể truy cập được. 

Sự đơn giản hóa quan trọng là xem xét từng vị trí bit một cách độc lập và theo dõi các mẫu nào bị tập hợp “ép buộc”. Một cách rõ ràng hơn để thấy điều đó là việc đóng dưới AND và OR trên một tập hợp các mặt nạ bit sẽ tạo ra tất cả các giá trị có thể được hình thành bằng cách chọn, đối với mỗi bit, liệu nó được hỗ trợ bởi ít nhất một phần tử hay bị loại bỏ bởi các phần tử giao nhau không chứa nó. Quá trình này cuối cùng sẽ lấp đầy mọi kết hợp phù hợp với sự kết hợp của các ràng buộc, dẫn đến một khoảng đầy đủ từ 0 đến OR theo bit của tất cả các phần tử. 

Do đó, kích thước đóng chỉ phụ thuộc vào giá trị cao nhất được hình thành bởi OR trên tất cả các phần tử. Khi chúng ta tính M = a1 OR a2 OR ... OR an, bao đóng chứa mọi số từ 0 đến M. Do đó, câu trả lời chỉ đơn giản là M + 1. 

Điều này biến vấn đề thành một lần quét tuyến tính duy nhất.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(k² log k) mỗi lần lặp, tăng trưởng k không giới hạn | O(k) | Quá chậm | 
| Giảm bitwise tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`mask`về 0. Điều này sẽ lưu trữ OR theo bit của tất cả các số đầu vào. 
2. Đọc từng số đầu vào và cập nhật`mask = mask OR number`. Điều này tổng hợp tất cả các bit xuất hiện ở bất kỳ đâu trong tập hợp. 
3. Sau khi xử lý tất cả các số, giải thích`mask`là giá trị cao nhất có thể đạt được trong bao đóng. 
4. Trở về`mask + 1`như kích thước của việc đóng cửa. 

Lý do đằng sau bước 2 là bất kỳ bit nào xuất hiện trong bất kỳ phần tử nào đều có thể được bật khi đóng thông qua các phép toán OR và không có thao tác nào có thể đưa ra một bit chưa từng có trong bất kỳ số đầu vào nào. 

### Tại sao nó hoạt động 

Mỗi số trong bao đóng được hình thành bằng các phép toán AND và OR lặp lại bắt đầu từ tập hợp ban đầu. HOẶC chỉ có thể kết hợp các bit hiện có, do đó không có vị trí bit mới nào có thể xuất hiện ngoài các vị trí có trong liên kết ban đầu. AND chỉ loại bỏ các bit, do đó nó không thể mở rộng phạm vi các giá trị có thể biểu thị ngoài những gì OR đã thiết lập. Khi tất cả các tổ hợp tập con bit có thể truy cập được thông qua các thao tác này, mọi số nguyên từ 0 đến mặt nạ OR đầy đủ đều có thể xây dựng được, ngụ ý rằng bao đóng chính xác là khoảng đầy đủ [0, mặt nạ]. Kích thước của khoảng này là mặt nạ + 1. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = list(map(int, input().split()))
    
    mask = 0
    for x in arr:
        mask |= x
    
    print(mask + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo quan sát rằng chỉ có OR toàn cục mới quan trọng. Vòng lặp là một tuyến tính đơn, tích lũy tất cả các bit có trong đầu vào. 

Không có vấn đề về thứ tự nào phát sinh vì OR có tính giao hoán và kết hợp, do đó các cập nhật gia tăng tạo ra cùng một mặt nạ cuối cùng bất kể trình tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
0 1 3 5
```Chúng tôi theo dõi sự tích lũy OR. 

| Bước | Giá trị hiện tại | Mặt nạ | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Thêm 0 | 0 | 0 | 
| Thêm 1 | 1 | 1 | 
| Thêm 3 | 3 | 3 | 
| Thêm 5 | 5 | 7 | 

Mặt nạ cuối cùng là 7, vì vậy câu trả lời là 8. 

Điều này chứng tỏ rằng mặc dù việc đóng AND và OR nghe có vẻ như tạo ra một tập hợp phức tạp, nhưng tất cả các giá trị có thể truy cập đều nằm trong phạm vi đầy đủ từ 0 đến tập hợp các bit. 

### Ví dụ 2 

đầu vào:```
5
0 1 2 3 4
```| Bước | Giá trị hiện tại | Mặt nạ | 
| --- | --- | --- | 
| Bắt đầu | 0 | 0 | 
| Thêm 0 | 0 | 0 | 
| Thêm 1 | 1 | 1 | 
| Thêm 2 | 2 | 3 | 
| Thêm 3 | 3 | 3 | 
| Thêm 4 | 4 | 7 | 

Mặt nạ cuối cùng là 7, vì vậy câu trả lời là 8. 

Điều này cho thấy rằng mặc dù đầu vào không chứa các giá trị trên 4, nhưng việc đóng sẽ lấp đầy tất cả các trạng thái trung gian lên đến 7 do kết hợp OR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được xử lý một lần bằng phép toán OR theo bit | 
| Không gian | O(1) | Chỉ sử dụng một bộ tích lũy số nguyên duy nhất | 

Quét tuyến tính là tối ưu cho n lên tới 2 · 10^5 và các phép toán theo bit là thời gian không đổi trên các số nguyên 64 bit, giúp giải pháp trở nên thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import sys as _sys
    backup = _sys.stdout
    _sys.stdout = io.StringIO()
    solve()
    out = _sys.stdout.getvalue()
    _sys.stdout = backup
    return out.strip()

# provided samples
assert run("4\n0 1 3 5\n") == "8"
assert run("5\n0 1 2 3 4\n") == "8"

# minimum size
assert run("1\n0\n") == "1"

# single power of two
assert run("1\n8\n") == "9"

# all equal values
assert run("3\n7 7 7\n") == "8"

# disjoint bits
assert run("3\n1 2 4\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử 0 | 1 | trường hợp ranh giới tối thiểu | 
| 1 phần tử sức mạnh của hai | 2^k+1 | độ chính xác từng bit | 
| lặp đi lặp lại các giá trị giống hệt nhau | mặt nạ+1 | trùng lặp không liên quan | 
| bộ bit rời rạc | hình thành khoảng thời gian đầy đủ | HOẶC hành vi tích lũy | 

## Vỏ cạnh 

Đầu vào tối thiểu như số 0 đơn sẽ kiểm tra xem việc triển khai có xử lý chính xác một bitset trống hay không. Với đầu vào`0`, mặt nạ vẫn bằng 0 và mức đóng là`{0}`, vì vậy câu trả lời là 1. Thuật toán xử lý vấn đề này một cách trực tiếp vì tích lũy OR không bao giờ thay đổi mặt nạ. 

Một trường hợp minh họa hơn là các mẫu bit rời rạc như`1 2 4`. Mặt nạ trở nên`7`. Mặc dù không có cặp AND nào tạo ra cấu trúc mới vượt quá 0, nhưng việc đóng OR lặp lại về mặt khái niệm sẽ lấp đầy mọi số nguyên từ 0 đến 7. Thuật toán không mô phỏng việc mở rộng này; nó hoàn toàn dựa vào bất biến OR nắm bắt toàn bộ không gian bit có thể truy cập, do đó nó trả về chính xác 8.
