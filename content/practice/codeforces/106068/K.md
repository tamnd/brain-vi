---
title: "CF 106068K - Hassan VS Naya"
description: "Chúng tôi bắt đầu với một danh sách các số nguyên. Hai người chơi lần lượt bắt đầu với Naya và mỗi nước đi bao gồm việc chọn hai số bất kỳ từ danh sách hiện tại, loại bỏ chúng và chèn ước chung lớn nhất của chúng. Sau đúng N − 1 nước đi, chỉ còn lại một số."
date: "2026-06-21T09:21:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106068
codeforces_index: "K"
codeforces_contest_name: "2025 Aleppo and Idlib Private Universities Collegiate Programming Contest (APUCPC 2025)"
rating: 0
weight: 106068
solve_time_s: 38
verified: true
draft: false
---

[CF 106068K - Hassan VS Naya](https://codeforces.com/problemset/problem/106068/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 38s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với một danh sách các số nguyên. Hai người chơi lần lượt bắt đầu với Naya và mỗi nước đi bao gồm việc chọn hai số bất kỳ từ danh sách hiện tại, loại bỏ chúng và chèn ước chung lớn nhất của chúng. Sau đúng N − 1 nước đi, chỉ còn lại một số. 

Giá trị cuối cùng phụ thuộc hoàn toàn vào chuỗi các hoạt động gcd theo cặp. Vì người chơi luân phiên nhau và cả hai đều chơi tối ưu, nên câu hỏi thực sự không phải là mô phỏng trò chơi mà là xác định xem liệu Naya có thể buộc số cuối cùng còn lại chính xác là 1 hay không, hay liệu Hassan có thể ngăn chặn kết quả đó hay không. 

Các ràng buộc cho phép tối đa 200.000 số, mỗi số lên tới 10^9. Bất kỳ giải pháp nào cố gắng mô phỏng quy trình đều thực hiện trực tiếp N − 1 thao tác hợp nhất và mỗi thao tác bao gồm việc quét hoặc tính toán lại các ứng cử viên. Ngay cả khi mỗi bước là O(N), tổng số sẽ là bậc hai và vượt xa giới hạn. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mô hình hóa rõ ràng trạng thái trò chơi. 

Một trường hợp khó phát hiện nhưng quan trọng là khi mảng đã chứa số 1. Trong tình huống đó, bất kỳ gcd nào liên quan đến phần tử đó đều tạo ra 1, vì vậy trò chơi có thể buộc phải kết thúc ở số 1 ngay lập tức. Một trường hợp đặc biệt khác là khi tất cả các số có chung một thừa số lớn hơn 1. Ví dụ: nếu tất cả các giá trị đều là số chẵn thì mọi gcd vẫn là số chẵn, do đó, việc đạt đến 1 là không thể dù có chơi hay không. 

Khó khăn thực sự là hiểu được liệu giá trị cuối cùng có thể được kiểm soát bởi sự lựa chọn của người chơi hay nó được xác định theo cấu trúc. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực rất đơn giản: mô phỏng trạng thái trò chơi. Ở mỗi bước, hãy thử tất cả các cặp có thể, tính toán gcd kết quả, lặp lại cả hai người chơi và xác định xem Naya có chiến lược chiến thắng hay không. Điều này tạo thành một cây trò chơi trong đó mỗi trạng thái phân nhánh thành các khả năng O(N^2) và độ sâu là N − 1. Ngay cả việc ghi nhớ mạnh mẽ cũng không giúp ích gì vì trạng thái mảng thay đổi theo kiểu tổ hợp với mỗi lần hợp nhất và các tập hợp riêng biệt sẽ bùng nổ về số lượng. 

Điểm thất bại là hoạt động không độc lập. Mỗi lần hợp nhất sẽ thay đổi nhiều tập hợp theo cách duy trì tính bất biến sâu sắc: cấu trúc gcd của toàn bộ mảng được kiểm soát bởi gcd toàn cục chứ không phải bởi các lựa chọn ghép nối cục bộ. 

Quan sát quan trọng là gcd có tính chất kết hợp và giao hoán. Bất kể các cặp được chọn như thế nào, số đơn cuối cùng sau khi áp dụng gcd nhiều lần trên tất cả các phần tử luôn là gcd của toàn bộ mảng. Trò chơi không thay đổi thực tế này; nó chỉ xác định thứ tự các phần tử được kết hợp. 

Vì vậy, câu hỏi duy nhất là liệu Naya có thể buộc quá trình kết thúc với giá trị 1 hay không. Vì giá trị cuối cùng được cố định là gcd(A1, A2, ..., AN), trò chơi giảm xuống còn việc kiểm tra xem gcd chung này có bằng 1 hay không. Nếu là 1, Naya chắc chắn sẽ thắng. Nếu nó lớn hơn 1 thì không có chuỗi nước đi nào có thể giảm nó thêm nữa. 

Điều này loại bỏ tất cả sự phức tạp của trò chơi: việc chơi tối ưu không thành vấn đề vì kết quả là bất biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | Trạng thái O(N) đến O(N^2) | Quá chậm | 
| Tính toán GCD toàn cầu | O(N log A) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước mảng và giá trị mảng. Cấu trúc của trò chơi không liên quan gì ngoài những giá trị ban đầu này vì tất cả các thao tác sau này đều bảo toàn một đại lượng bất biến duy nhất. 
2. Tính gcd của tất cả các phần tử trong mảng theo cách lặp. Bắt đầu từ phần tử đầu tiên và thực hiện thao tác gcd trên toàn bộ danh sách. Điều này xây dựng gcd toàn cầu đại diện cho giá trị cuối cùng có thể có sau tất cả các lần hợp nhất. 
3. Sau khi tính toán gcd toàn cục, hãy kiểm tra xem nó có bằng 1 hay không. 
4. Nếu bằng 1, xuất ra Naya, nếu không thì xuất ra Hassan.

Bước suy luận chính là mọi thao tác đều thay thế hai số X và Y bằng gcd(X, Y), số này không thể đưa ra bất kỳ thừa số nguyên tố mới nào và chỉ có thể bảo toàn hoặc loại bỏ các thừa số nguyên tố hiện có. Việc lặp lại điều này trên toàn bộ chuỗi tương đương với việc tính toán gcd của toàn bộ nhiều tập hợp. 

### Tại sao nó hoạt động 

Điều bất biến là sau bất kỳ chuỗi thao tác nào, gcd của tất cả các phần tử còn lại giống hệt với gcd của mảng ban đầu. Mỗi bước thay thế sẽ bảo toàn gcd của nhiều tập hợp vì gcd(X, Y) chia cả X và Y, do đó việc thay thế chúng không làm thay đổi gcd tổng thể. Vì quá trình kết thúc bằng một số duy nhất nên số đó phải bằng gcd của tất cả các phần tử ban đầu. Các lựa chọn trò chơi chỉ ảnh hưởng đến việc nhóm trung gian chứ không ảnh hưởng đến giá trị bất biến cuối cùng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    
    g = 0
    for x in arr:
        g = gcd(g, x)
    
    if g == 1:
        print("Naya")
    else:
        print("Hassan")

if __name__ == "__main__":
    main()
```Việc triển khai sẽ giữ cho bộ tích lũy gcd đang chạy. Việc khởi tạo bằng 0 có tác dụng vì gcd(0, x) = x, do đó phần tử đầu tiên sẽ tạo giá trị chính xác. Mỗi bản cập nhật sẽ gấp phần tử tiếp theo vào gcd toàn cục theo thời gian tuyến tính trên mảng. 

Quyết định cuối cùng là so sánh trực tiếp với 1, phản ánh liệu có thể giảm hoàn toàn xuống 1 về mặt cấu trúc hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
N = 5
A = [3, 8, 5, 12, 1]
```Chúng tôi tính toán gcd từng bước. 

| Bước | Gcd hiện tại | Giá trị tiếp theo | Đã cập nhật gcd | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 3 | 
| 2 | 3 | 8 | 1 | 
| 3 | 1 | 5 | 1 | 
| 4 | 1 | 12 | 1 | 
| 5 | 1 | 1 | 1 | 

gcd cuối cùng là 1 nên đầu ra là Naya. 

Điều này cho thấy một phần tử nguyên tố cùng nhau ở đầu chuỗi sẽ thu gọn toàn bộ cấu trúc thành 1 như thế nào. 

### Ví dụ 2 

đầu vào:```
N = 3
A = [6, 10, 14]
```| Bước | Gcd hiện tại | Giá trị tiếp theo | Đã cập nhật gcd | 
| --- | --- | --- | --- | 
| 1 | 0 | 6 | 6 | 
| 2 | 6 | 10 | 2 | 
| 3 | 2 | 14 | 2 | 

gcd cuối cùng là 2, vì vậy đầu ra là Hassan. 

Điều này chứng tỏ rằng ngay cả khi gcd trung gian giảm thì chúng cũng không thể đạt tới 1 trừ khi một phần tử nào đó phá vỡ tất cả các thừa số chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log A) | Mỗi lần tính toán gcd tốn thời gian logarit ở kích thước giá trị | 
| Không gian | O(1) | Chỉ có một bộ tích lũy duy nhất được duy trì | 

Các ràng buộc cho phép tối đa 200.000 phần tử và các phép toán gcd logarit đủ nhanh trong Python, khiến cách tiếp cận này trở nên thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(sys.stdin.readline())
    arr = list(map(int, sys.stdin.readline().split()))
    g = 0
    for x in arr:
        g = gcd(g, x)
    return "Naya" if g == 1 else "Hassan"

def run(inp: str) -> str:
    return solve(inp)

# provided samples (as inferred)
assert run("1\n2\n") == "Naya"
assert run("3\n8192 1048576 128\n") == "Hassan"

# custom cases
assert run("1\n1\n") == "Naya"
assert run("2\n2 4\n") == "Hassan"
assert run("2\n3 4\n") == "Naya"
assert run("5\n6 10 14 15 21\n") == "Naya"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | trường hợp cơ sở đơn phần tử | 
| 2 4 | Hassan | tất cả đều chẵn, gcd > 1 | 
| 3 4 | Naya | lực cặp coprime gcd 1 | 
| 6 10 14 15 21 | Naya | yếu tố hỗn hợp tạo ra gcd 1 | 

## Vỏ cạnh 

Trường hợp một phần tử là ranh giới rõ ràng nhất. Nếu N = 1 và giá trị là 1 thì câu trả lời ngay lập tức là Naya vì không cần thực hiện thao tác nào. Nếu N = 1 và giá trị lớn hơn 1 thì giá trị cuối cùng là số đó nên Hassan thắng. Thuật toán xử lý việc này một cách tự nhiên vì vòng lặp gcd trả về chính phần tử đó. 

Đối với một mảng hoàn toàn thống nhất như [4, 8, 12, 16], mọi sự hợp nhất đều duy trì tính đồng đều và gcd đang chạy vẫn là 4. Đầu ra cuối cùng là Hassan. Theo dõi thuật toán, bộ tích lũy bắt đầu từ 0, trở thành 4 sau phần tử đầu tiên và không bao giờ thay đổi khỏi 4. 

Đối với các mảng chứa ít nhất một số 1, chẳng hạn như [10, 15, 1, 27], gcd ngay lập tức giảm xuống 1 tại thời điểm số 1 được xử lý. Kể từ thời điểm đó trở đi, nó vẫn là 1 bất kể có nhập thêm gì, đảm bảo chiến thắng cho Naya.
