---
title: "CF 106068B - SCPC đang gõ..."
description: "Chúng ta được cho một danh sách các vị trí trên trục số, mỗi vị trí thể hiện nơi ở của một người. Mục tiêu là chọn một trong các vị trí nhất định làm điểm gặp mặt sao cho tổng khoảng cách đi bộ từ tất cả mọi người đến điểm đã chọn đó càng nhỏ càng tốt."
date: "2026-06-21T15:57:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "B"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 44
verified: true
draft: false
---

[CF 106068B - SCPC đang gõ...](https://codeforces.com/problemset/problem/106068/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một danh sách các vị trí trên trục số, mỗi vị trí thể hiện nơi ở của một người. Mục tiêu là chọn một trong các vị trí nhất định làm điểm gặp mặt sao cho tổng khoảng cách đi bộ từ tất cả mọi người đến điểm đã chọn đó càng nhỏ càng tốt. Khoảng cách được đo bằng độ lệch chuẩn tuyệt đối trên đường thẳng. 

Mặc dù bài toán cho phép chọn bất kỳ vị trí nào cho trước nhưng cái giá phải trả chỉ phụ thuộc vào việc điểm được chọn cân bằng như thế nào với tất cả các giá trị trên trục số. Theo trực giác, nếu điểm gặp nhau quá xa về bên trái, nhiều điểm sẽ nằm về bên phải và đóng góp khoảng cách lớn; nếu nó ở quá xa bên phải thì vấn đề đối xứng sẽ xảy ra. 

Kích thước đầu vào có thể lớn tới 200.000 giá trị và mỗi giá trị có thể lên tới 10^9. Một giải pháp bậc hai đánh giá mọi điểm gặp gỡ của ứng viên so với tất cả những điểm khác sẽ liên quan đến tính toán khoảng cách lên tới 4 × 10^10 trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong hai giây trong Python hoặc thậm chí mã được biên dịch được tối ưu hóa với các mẫu triển khai đơn giản. 

Cần phải có một cách tiếp cận tuyến tính hoặc toán học tuyến tính, điều này gợi ý rõ ràng rằng chúng ta phải tránh tính toán lại khoảng cách từ đầu cho mọi ứng viên. 

Một điểm tinh tế là câu trả lời không nhất thiết phải có giá trị duy nhất nếu nhiều vị trí mang lại cùng một chi phí tối thiểu. Vì bài toán yêu cầu in vị trí Pi từ mảng nên mọi vị trí giống trung vị hợp lệ đều được chấp nhận. 

Các trường hợp biên quan trọng ở đây bao gồm các mảng có giá trị lặp lại, các mảng đã được sắp xếp và các phân phối có độ lệch cao, chẳng hạn như một ngoại lệ cực đoan. Ví dụ: nếu đầu vào là [1, 1, 1, 100] thì điểm gặp tối ưu phải là 1; một trực giác ngây thơ có thể xem xét không chính xác logic trung bình hoặc trung điểm, điều này không liên quan vì chúng ta bị ràng buộc phải chọn một Pi hiện có. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi vị trí ứng viên Pi, hãy tính tổng các sai phân tuyệt đối |Pi − Pj| trên tất cả j, sau đó chọn Pi có tổng nhỏ nhất. Điều này đúng vì nó đánh giá trực tiếp hàm mục tiêu được xác định bởi bài toán. 

Tuy nhiên, cách tiếp cận này tính lại tổng O(N) cho mỗi N ứng cử viên, dẫn đến tổng số phép toán là O(N^2). Với N tối đa 2×10^5, điều này dẫn đến các phép toán khoảng 4×10^10, điều này là không khả thi. 

Quan sát quan trọng là việc giảm thiểu tổng độ lệch tuyệt đối trên một đường thẳng là thuộc tính trung vị cổ điển. Nếu chúng ta sắp xếp mảng, điểm thu nhỏ tổng khoảng cách là phần tử trung vị. Lý do mang tính cấu trúc: việc di chuyển điểm gặp nhau sang trái hoặc sang phải một chút sẽ thay đổi sự đóng góp của các phần tử ở mỗi bên theo các hướng ngược nhau và đạt được sự cân bằng chính xác ở phần phân chia ở giữa. 

Vì N là số lẻ nên trung vị là một phần tử được xác định rõ ràng sau khi sắp xếp, cụ thể là phần tử ở chỉ số N/2 trong lập chỉ mục dựa trên 0. 

Điều này chuyển đổi vấn đề từ việc đánh giá tất cả các ứng viên sang sắp xếp một lần và chọn một vị trí duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^2) | O(1) | Quá chậm | 
| Tối ưu (Trung vị sau khi sắp xếp) | O(N log N) | O(1) hoặc O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc mảng vị trí. Ở giai đoạn này, các giá trị theo thứ tự tùy ý, do đó chưa có thuộc tính cấu trúc nào như thứ tự hoặc cân bằng tiền tố. 
2. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp là cần thiết vì thuộc tính trung vị chỉ có ý nghĩa theo thứ tự sắp xếp. Không phân loại thì không có khái niệm nửa trái và nửa phải. 
3. Chọn phần tử tại chỉ số N // 2. Vì N là số lẻ nên chỉ số này tương ứng với phần tử ở giữa có số phần tử hai vế bằng nhau. 
4. Xuất phần tử này làm điểm gặp đã chọn. Giá trị này được đảm bảo là một trong những vị trí ban đầu và tối ưu theo mục tiêu tổng khoảng cách tuyệt đối. 

### Tại sao nó hoạt động 

Xét điểm x bất kỳ trên trục số. Mỗi điểm ở bên trái của x đóng góp một khoảng cách giảm đi nếu x di chuyển sang trái và mỗi điểm ở bên phải đóng góp một khoảng cách tăng lên nếu x di chuyển sang trái. Hàm tổng chi phí tuyến tính từng phần và thay đổi độ dốc tùy thuộc vào số lượng điểm nằm ở hai bên của x. Độ dốc là âm khi có ít hơn một nửa số điểm ở bên trái và dương khi có ít hơn một nửa số điểm ở bên phải. Điểm cân bằng mà cả hai hướng đều không cải thiện được chi phí chính xác là khi x là trung vị. Vì N là số lẻ nên sự cân bằng này xảy ra ở một phần tử duy nhất theo thứ tự được sắp xếp, đảm bảo rằng phần tử trung vị giảm thiểu tổng độ lệch tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n = int(input())
    p = list(map(int, input().split()))
    
    p.sort()
    print(p[n // 2])

if __name__ == "__main__":
    main()
```Giải pháp đọc tất cả các vị trí, sắp xếp chúng và in phần tử ở giữa. Chi tiết triển khai tinh vi duy nhất là sử dụng phép chia số nguyên n // 2, chọn chính xác chỉ số trung vị cho các mảng dựa trên 0 khi n là số lẻ. Không cần xử lý đặc biệt đối với các bản sao hoặc đầu vào chưa được sắp xếp vì việc sắp xếp tự nhiên xử lý cả hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

[7, 3, 5, 1, 4] 

Mảng được sắp xếp: 

[1, 3, 4, 5, 7] 

Chỉ số trung bình là 2. 

| Bước | Trạng thái mảng | Chỉ số được chọn | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Sắp xếp | [1, 3, 4, 5, 7] | - | - | 
| Chọn | [1, 3, 4, 5, 7] | 2 | 4 | 

Điều này cho thấy 4 cân bằng ba phần tử ở bên trái và một phần tử ở bên phải, tạo ra tổng khoảng cách tuyệt đối tối thiểu. 

### Ví dụ 2 

đầu vào: 

[10, 2, 8, 9, 2] 

Mảng được sắp xếp: 

[2, 2, 8, 9, 10] 

Chỉ số trung bình là 2. 

| Bước | Trạng thái mảng | Chỉ số được chọn | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Sắp xếp | [2, 2, 8, 9, 10] | - | - | 
| Chọn | [2, 2, 8, 9, 10] | 2 | 8 | 

Ở đây, 8 giảm thiểu tổng khoảng cách vì nó chia hai giá trị ở bên trái và hai giá trị ở bên phải, mang lại sự cân bằng tối ưu. 

Các dấu vết xác nhận rằng giải pháp chọn điểm cân bằng trung tâm một cách nhất quán sau khi đặt hàng, đây chính xác là hành vi trung bình cần thiết để giảm thiểu độ lệch tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế, quét tuyến tính sau đó | 
| Không gian | O(1) hoặc O(N) | Tùy thuộc vào việc triển khai sắp xếp tại chỗ | 

Các ràng buộc cho phép tối đa 2×10^5 phần tử và việc sắp xếp kích thước này vừa vặn một cách thoải mái trong giới hạn thời gian thông thường trong Python. Phần còn lại của phép tính là thời gian không đổi so với N, vì vậy giải pháp này hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from types import SimpleNamespace

    # capture output
    from io import StringIO
    backup = sys.stdout
    sys.stdout = StringIO()

    try:
        main()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdout = backup

# provided sample
assert run("5\n7 3 5 1 4\n") == "4"

# minimum size
assert run("1\n10\n") == "10"

# all equal
assert run("5\n2 2 2 2 2\n") == "2"

# already sorted
assert run("5\n1 2 3 4 5\n") == "3"

# reverse sorted
assert run("5\n5 4 3 2 1\n") == "3"

# skewed distribution
assert run("5\n1 1 1 1 100\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | cùng giá trị | trường hợp cạnh tối thiểu | 
| tất cả đều bình đẳng | giá trị đó | ổn định dưới sự trùng lặp | 
| đầu vào được sắp xếp | phần tử ở giữa | không phụ thuộc vào thứ tự đầu vào | 
| giá trị sai lệch | 1 | sự vượt trội của trung vị so với ngoại lệ | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như [10], việc sắp xếp giữ cho nó không thay đổi và chọn chỉ mục 0 trả về 10, mức này tối ưu một cách tầm thường vì không cần chuyển động. 

Đối với một mảng hoàn toàn bằng nhau như [2, 2, 2, 2, 2], việc sắp xếp không làm gì cả và trung vị vẫn là 2. Mọi lựa chọn sẽ mang lại tổng khoảng cách bằng 0 và thuật toán luôn trả về một điểm tối ưu hợp lệ. 

Đối với một mảng có độ lệch cao như [1, 1, 1, 1, 100], việc sắp xếp sẽ cho [1, 1, 1, 1, 100] và chỉ số trung vị là 2, mang lại giá trị 1. Nếu người ta cố gắng chọn dựa trên tính trung bình hoặc trực giác về việc cân bằng độ lớn thay vì số lượng, người ta có thể thích 100 hoặc giá trị nào đó gần nó hơn, nhưng chi phí sai lệch tuyệt đối chỉ phụ thuộc vào số lượng ở mỗi bên chứ không phải khoảng cách trong không gian giá trị.
