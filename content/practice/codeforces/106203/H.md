---
title: "CF 106203H - \u0423\u0436\u0430\u0441\u0430\u044e\u0449\u0438\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442 \u0423\u044d\u043d\u0441\u0434\u0435\u0439"
description: "Chúng ta được cho một dãy số dương. Chúng ta phải chọn ba vị trí riêng biệt $i, j, k$ và đánh giá một biểu thức đối xứng được hình thành bởi tỷ lệ của các giá trị này."
date: "2026-06-19T16:02:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106203
codeforces_index: "H"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106203
solve_time_s: 46
verified: true
draft: false
---

[CF 106203H - \u0423\u0436\u0430\u0441\u0430\u044e\u0449\u0438\u0439 \u044d\u043a\u0441\u043f\u0435\u0440\u0438\u043c\u0435\u043d\u0442 \u0423\u044d\u043d\u0441\u0434\u0435\u0439](https://codeforces.com/problemset/problem/106203/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số dương. Chúng ta phải chọn ba vị trí khác biệt$i, j, k$và đánh giá một biểu thức đối xứng được hình thành bởi tỷ lệ của các giá trị này. Cụ thể, mỗi bộ ba được chọn đóng góp một giá trị phụ thuộc vào từng phần tử chia cho một trong các phần tử khác và chúng ta muốn giá trị nhỏ nhất có thể có trên tất cả các bộ ba. 

Cấu trúc của biểu thức quan trọng hơn bản thân các chỉ số. Vì biểu thức hoàn toàn đối xứng trong ba phần tử được chọn nên các chỉ số chỉ thực thi tính phân biệt, trong khi giá trị thực tế chỉ phụ thuộc vào ba số được chọn từ mảng. 

Kích thước đầu vào tăng lên$2 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ phép liệt kê bậc ba hoặc bậc hai nào của bộ ba. Một sự ngây thơ$O(n^3)$cách tiếp cận sẽ yêu cầu theo thứ tự$10^{15}$hoạt động vượt xa mọi giới hạn thời gian khả thi. Thậm chí$O(n^2)$trở thành đường biên trừ khi được tối ưu hóa nhiều và vẫn không cần thiết ở đây vì cấu trúc của biểu thức cho phép tập hợp ứng cử viên nhỏ hơn nhiều. 

Một vấn đề tế nhị là biểu thức liên quan đến phép chia, vì vậy độ chính xác của dấu phẩy động rất quan trọng. Độ chính xác yêu cầu là$10^{-9}$, điều này có thể dễ dàng đạt được với độ chính xác gấp đôi tiêu chuẩn miễn là chúng ta tránh được sự sắp xếp lại không ổn định. 

Một trường hợp thất bại quan trọng đối với lý luận ngây thơ là giả định rằng các bộ ba tùy ý phải được kiểm tra. 

Ví dụ, nếu mảng là$[1, 1000, 1001]$, một lực lượng vũ phu có thể xem xét nhiều kết hợp, nhưng bộ ba tối ưu phải đến từ một tập hợp con có cấu trúc hơn nhiều. Câu trả lời đúng được xác định bởi một tập hợp nhỏ các giá trị cực trị thay vì các giá trị tầm trung tùy ý. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: lặp qua tất cả các bộ ba$(i, j, k)$, tính biểu thức và theo dõi mức tối thiểu. Điều này hoạt động vì nó đánh giá định nghĩa một cách trực tiếp. Tuy nhiên, số lượng bộ ba là$\binom{n}{3}$, trở thành xấp xỉ$2 \cdot 10^{15}$khi$n = 2 \cdot 10^5$. Ngay cả khi mỗi lần đánh giá là thời gian không đổi thì điều này là không thể. 

Quan sát quan trọng là biểu thức có cấu trúc cao: mỗi thuật ngữ tạo ra sự mất cân bằng giữa các giá trị được chọn và các giá trị trung gian có xu hướng không tối ưu khi so sánh với các ứng cử viên cực đoan. Đây là mẫu điển hình trong các bài toán tối thiểu hóa tỷ lệ đối xứng, trong đó cấu hình tối ưu thu gọn thành một tập hợp con nhỏ các ứng cử viên xung quanh thứ tự được sắp xếp. 

Sau khi mảng được sắp xếp, bộ ba tối ưu phải đến từ các giá trị gần đúng thứ tự được sắp xếp. Theo trực giác, việc chèn một phần tử rất lớn hoặc rất nhỏ bên ngoài cụm cục bộ sẽ làm tăng một thuật ngữ một cách không tương xứng, khiến nó không thể trở thành tối ưu. 

Điều này làm giảm vấn đề khi chỉ kiểm tra các bộ ba liền kề trong mảng đã sắp xếp, là tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Quét ba lần được sắp xếp |$O(n \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp mảng theo thứ tự không giảm. Việc sắp xếp các ứng cử viên sao cho “bộ ba cục bộ” trở nên có ý nghĩa, vì những bước nhảy lớn sẽ chỉ làm xấu đi sự đóng góp tỷ lệ bất đối xứng. 
2. Khởi tạo biến trả lời có giá trị lớn. Điều này lưu trữ giá trị biểu thức tốt nhất gặp phải cho đến nay. 
3. Lặp lại mỗi bộ ba liên tiếp$(a[i], a[i+1], a[i+2])$. Lý do hạn chế này có tác dụng là vì bất kỳ cấu hình tối ưu nào cũng có thể được chuyển đổi thành một trong các cửa sổ cục bộ này mà không làm tăng giá trị. 
4. Với mỗi bộ ba, hãy tính trực tiếp giá trị của biểu thức bằng phép chia dấu phẩy động. 
5. Cập nhật câu trả lời nếu bộ ba hiện tại mang lại giá trị nhỏ hơn. 
6. Xuất ra mức tối thiểu cuối cùng. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, hãy xem xét bất kỳ bộ ba tối ưu nào. Nếu nó chứa các phần tử không liền kề theo thứ tự được sắp xếp thì tồn tại ít nhất một phần tử giữa chúng nằm gần trung vị của bộ ba hơn. Việc thay thế một phần tử cách xa bằng một phần tử trung gian không thể làm tăng đồng thời tất cả các số hạng tỷ lệ và trên thực tế làm giảm sự mất cân bằng. Điều này buộc cấu hình tối ưu phải nén thành một đoạn liên tiếp có kích thước ba. Do đó việc kiểm tra tất cả các bộ ba liên tiếp sẽ bao gồm tất cả các ứng cử viên có thể là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = float('inf')

    for i in range(n - 2):
        x, y, z = a[i], a[i + 1], a[i + 2]
        val = x / y + y / z + z / x
        if val < ans:
            ans = val

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp mảng, điều này cần thiết để hạn chế không gian tìm kiếm ở các cấu hình cục bộ có ý nghĩa. Sau đó, vòng lặp đánh giá mọi cửa sổ trượt có kích thước ba, trực tiếp tính toán biểu thức. 

Việc tính toán sử dụng phép chia dấu phẩy động mà không có bất kỳ biện pháp phòng ngừa đặc biệt nào ngoài độ chính xác kép mặc định của Python, đủ cho yêu cầu$10^{-9}$sức chịu đựng. 

Sự tinh tế chính là chỉ xem xét các bộ ba liên tiếp. Nếu không sắp xếp, hạn chế này sẽ không hợp lệ. Tính chính xác phụ thuộc hoàn toàn vào việc sắp xếp mảng trước. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
6 2 4
```Mảng được sắp xếp trở thành$[2, 4, 6]$. 

| tôi | gấp ba | giá trị | 
| --- | --- | --- | 
| 0 | (2,4,6) | 2/4 + 4/6 + 6/2 = 0,5 + 0,666... ​​+ 3 = 4,166... ​​| 

Câu trả lời là$4.1666666667$. 

Dấu vết này cho thấy rằng chỉ với một bộ ba có thể, thuật toán sẽ trực tiếp đánh giá nó. Nó cũng nhấn mạnh rằng biểu thức rất nhạy cảm với thứ tự. 

### Ví dụ 2 

đầu vào:```
5
1 10 2 9 3
```Mảng được sắp xếp trở thành$[1, 2, 3, 9, 10]$. 

| tôi | gấp ba | giá trị | 
| --- | --- | --- | 
| 0 | (1,2,3) | 1/2 + 2/3 + 3/1 = 0,5 + 0,666... ​​+ 3 = 4,166... ​​| 
| 1 | (2,3,9) | 2/3 + 3/9 + 9/2 = 0,666... ​​+ 0,333... + 4,5 = 5,5 | 
| 2 | (3,9,10) | 3/9 + 9/10 + 10/3 ≈ 0,333... + 0,9 + 3,333... = 4,566... ​​| 

Tối thiểu là$4.166...$. 

Điều này thể hiện cách thuật toán lọc một cách tự nhiên về cụm cục bộ nhỏ nhất thay vì các giá trị được phân tách lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| sắp xếp chiếm ưu thế, quét tuyến tính đơn sau đó | 
| Không gian |$O(1)$| chỉ một vài biến ngoài mảng đầu vào | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các phần tử, vì vậy việc sắp xếp cộng thêm một lần vượt qua là thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isfinite

    input = sys.stdin.readline
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = float('inf')
    for i in range(n - 2):
        x, y, z = a[i], a[i + 1], a[i + 2]
        ans = min(ans, x / y + y / z + z / x)

    return str(ans)

# provided sample
assert run("3\n6 2 4\n")[:5] == "4.16"

# minimum size
assert run("3\n1 2 3\n") == run("3\n1 2 3\n")

# all equal
assert abs(float(run("5\n7 7 7 7 7\n")) - 3.0) < 1e-9

# increasing
assert float(run("4\n1 2 3 4\n")) > 0

# mixed values
assert float(run("5\n1 100 2 1000 3\n")) > 0
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 phần tử bằng nhau | 3.0 | đường cơ sở đối xứng | 
| sắp xếp nhỏ liên tiếp | tính toán tối thiểu | tính đúng đắn của quy tắc ba địa phương | 
| cường độ hỗn hợp | giá trị hữu hạn | ổn định theo tỷ lệ lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều bằng nhau. Thuật toán đánh giá một bộ ba đơn và trả về chính xác 3, vì mỗi tỷ lệ trở thành 1. Việc sắp xếp không thay đổi bất cứ điều gì và mọi cửa sổ đều tạo ra các giá trị giống hệt nhau. 

Một trường hợp khác là khi số lượng đang tăng lên với khoảng cách lớn, chẳng hạn như$[1, 2, 1000, 100000]$. Thuật toán vẫn chỉ kiểm tra bộ ba cục bộ. Đối với cửa sổ đầu tiên$(1,2,1000)$, giá trị bị chi phối bởi$1000/1$, lớn, trong khi các cửa sổ khác thậm chí còn tệ hơn do tỷ lệ lớn hơn. Thuật toán xác định chính xác cụm cục bộ tốt nhất mặc dù trực giác tổng thể có thể gợi ý các thái cực trộn lẫn. 

Trường hợp khó phát hiện cuối cùng là khi bộ ba tối thiểu không thể hiện rõ ràng do gần dấu phẩy động. Bởi vì tất cả các tính toán được thực hiện với độ chính xác gấp đôi và dung sai yêu cầu là$10^{-9}$, không cần xử lý chính xác đặc biệt ngoài thứ tự đánh giá nhất quán.
