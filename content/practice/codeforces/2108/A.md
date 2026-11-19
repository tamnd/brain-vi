---
title: "CF 2108A - Khởi động hoán vị"
description: "Chúng ta được cho một hoán vị của các số từ 1 đến n và chúng ta đo xem mỗi phần tử di chuyển ra xa vị trí ban đầu của nó bao xa. Đối với mỗi vị trí i, chúng tôi lấy chênh lệch tuyệt đối giữa giá trị ở đó và chính i rồi tính tổng giá trị này trên toàn bộ mảng."
date: "2026-06-08T04:43:26+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2108
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1022 (Div. 2)"
rating: 800
weight: 2108
solve_time_s: 69
verified: true
draft: false
---

[CF 2108A - Khởi động về hoán vị](https://codeforces.com/problemset/problem/2108/A) 

**Đánh giá:** 800 
**Tags:** tổ hợp, tham lam, toán học 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số từ 1 đến n và chúng ta đo xem mỗi phần tử di chuyển ra xa vị trí ban đầu của nó bao xa. Đối với mỗi vị trí i, chúng tôi lấy chênh lệch tuyệt đối giữa giá trị ở đó và chính i rồi tính tổng giá trị này trên toàn bộ mảng. Điều này tạo ra một số nguyên duy nhất cho hoán vị đó. 

Nhiệm vụ không phải là tính giá trị này cho một hoán vị đơn lẻ mà là xem xét tất cả các hoán vị có thể có của độ dài n và xác định có bao nhiêu tổng điểm khác nhau có thể xuất hiện. 

Đối tượng chính không phải là các hoán vị mà là tập hợp các giá trị có thể đạt được của thước đo giống như khoảng cách. Mỗi hoán vị sắp xếp lại nhiều tập hợp số nguyên giống nhau, do đó tổng bị ràng buộc bởi mức độ dịch chuyển có thể xảy ra trên toàn cầu. 

Các ràng buộc cho phép n lên tới 500 với nhiều trường hợp thử nghiệm. Bất kỳ giải pháp nào liệt kê rõ ràng các hoán vị đều không thể thực hiện được vì ngay cả với n = 10, số lượng hoán vị đã là 3.628.800. Điều này loại trừ hoàn toàn việc tạo ra lực lượng vũ phu. Giải pháp phải dựa vào đặc tính cấu trúc của những khác biệt tuyệt đối này có thể khác nhau như thế nào. 

Trường hợp cạnh tinh tế xuất hiện ở mức n rất nhỏ. Khi n = 1 thì chỉ có một hoán vị và giá trị luôn là 0 nên đáp án phải là 1 chứ không phải 0. Khi n = 2 thì chỉ có thể có hai giá trị là 0 và 2, điều này cho thấy hàm không nhất thiết phải tạo ra các số nguyên liên tiếp mà vẫn tạo thành một tập hợp có cấu trúc. 

## Phương pháp tiếp cận 

Ý tưởng Brute Force rất đơn giản: tạo ra mọi hoán vị, tính tổng chênh lệch tuyệt đối, chèn kết quả vào một tập hợp và trả về kích thước của nó. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, nó yêu cầu lặp lại trên n! hoán vị và tính tổng O(n) cho mỗi hoán vị, dẫn đến các phép toán O(n · n!). Điều này trở nên không khả thi gần như ngay lập tức khi vượt quá n = 10. 

Quan sát quan trọng là chúng ta không nghiên cứu các hoán vị riêng lẻ mà nghiên cứu phạm vi các giá trị dịch chuyển tổng có thể có. Biểu thức ∑|p_i − i| được gọi là tổng độ dịch chuyển hoặc khoảng cách theo quy tắc bước chân Spearman so với danh tính. Một thực tế cấu trúc quan trọng là các phần tử hoán đổi ảnh hưởng đến tổng theo các bước tăng được kiểm soát và tập hợp các giá trị có thể đạt được hóa ra tạo thành tất cả các số chẵn cho đến giá trị tối đa, với một số ràng buộc chẵn lẻ tùy thuộc vào n. 

Chính xác hơn, giá trị tối thiểu là 0 (hoán vị nhận dạng) và giá trị tối đa đạt được bằng cách đảo ngược mảng. Các giá trị trung gian có thể thu được bằng cách điều chỉnh dần dần hoán vị ngược bằng cách sử dụng các giao dịch hoán đổi để điều chỉnh tổng chi phí theo các bước có thể dự đoán được. Tính chẵn lẻ của tổng phụ thuộc vào việc n là chẵn hay lẻ, nhưng bất chấp điều đó, số lượng các giá trị phân biệt hóa ra chỉ phụ thuộc vào n và tuân theo một biểu thức bậc hai đơn giản trong n. 

Dạng đóng cuối cùng có thể được rút ra bằng cách phân tích xem chúng ta có thể di chuyển các phần tử một cách đối xứng bao xa. Mỗi lần hoán đổi giữa các vị trí sẽ góp phần thay đổi tổng được kiểm soát và tập hợp có thể truy cập sẽ lấp đầy tất cả các số nguyên của một phạm vi chẵn lẻ nhất định trong khoảng từ 0 đến giá trị tối đa. Việc đếm các giá trị đó sẽ giảm xuống việc đếm độ dài cấp số cộng, mang lại công thức cuối cùng: 

Với n chẵn: 

số lượng giá trị phân biệt là n² / 4 + 1 

Đối với n lẻ: 

số lượng giá trị riêng biệt là (n² + 3)/4 

Điều này đơn giản hóa thành một biểu thức thống nhất: 

⌊n² / 4⌋ + 1 

Do đó, vấn đề giảm xuống còn tính toán theo thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · n!) | Ồ (n!) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc n cho mỗi test case. Điều này xác định kích thước hoán vị và xác định toàn bộ cấu trúc của các giá trị có thể. 
2. Tính n × n. Điều này thể hiện thang đo của tổng công suất dịch chuyển, vì mỗi vị trí đóng góp nhiều nhất là O(n) vào tổng. 
3. Chia giá trị này cho 4 bằng phép chia số nguyên. Điều này tương ứng với việc đếm xem có bao nhiêu “bước” riêng biệt phù hợp với toàn bộ phạm vi giá trị có thể đạt được, đối xứng xung quanh hoán vị danh tính. 
4. Thêm 1 để bao gồm hoán vị dịch chuyển bằng không. Điều này đảm bảo giá trị hoán vị danh tính được tính. 

Mỗi bước tương ứng trực tiếp với việc nén cấu trúc của khoảng cách hoán vị thành một phạm vi số học duy nhất. Bước không tầm thường duy nhất là nhận ra rằng các giá trị có thể tiếp cận tạo thành một tập hợp liền kề trong một lớp chẵn lẻ. 

### Tại sao nó hoạt động 

Hàm f(p) đo tổng độ dịch chuyển khỏi hoán vị danh tính. Hoán đổi hai phần tử sẽ thay đổi tổng theo cách được kiểm soát và tăng dần, đồng thời bất kỳ hoán vị nào cũng có thể được chuyển đổi thành bất kỳ hoán vị nào khác thông qua một chuỗi các hoán đổi. Đặc tính cấu trúc quan trọng là những hoán đổi này cho phép chúng ta nhận ra mọi chuyển vị tổng khả thi trong một khoảng cố định, nhưng chỉ tăng dần theo các ràng buộc chẵn lẻ. Kết quả là, tập hợp các giá trị có thể đạt được tạo thành một cấp số cộng bắt đầu từ 0, kết thúc ở độ dịch chuyển tối đa và bao gồm chính xác một lớp chẵn lẻ. Việc đếm các phần tử trong tiến trình này giảm xuống việc tính toán độ dài của nó, độ dài này chỉ phụ thuộc vào n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    print(n * n // 4 + 1)
```Mã trực tiếp thực hiện biểu mẫu đóng dẫn xuất. Phép nhân n * n an toàn cho n lên tới 500, vì kết quả vừa khít với các số nguyên Python. Phép chia số nguyên cho 4 sẽ nắm bắt được mật độ của các giá trị có thể đạt được trong phạm vi. Tài khoản +1 cho hoán vị giá trị bằng 0. 

Lời giải tránh mọi cách xây dựng hoán vị và hoàn toàn dựa vào kết quả cấu trúc rằng số lượng các giá trị phân biệt chỉ phụ thuộc vào n. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 2 

Chúng tôi đánh giá tất cả các hoán vị: 

| hoán vị | f(p) | 
| --- | --- | 
| [1, 2] | 0 | 
| [2, 1] | 2 | 

Các giá trị riêng biệt là {0, 2}. 

Với n = 2, công thức cho 22 // 4 + 1 = 1 + 1 = 2. 

Điều này xác nhận rằng ngay cả ở kích thước tối thiểu không tầm thường, các giá trị không phải là số nguyên liền kề mà vẫn tuân theo cấu trúc số học. 

### Ví dụ 2 

đầu vào: 

n = 3 

| hoán vị | f(p) | 
| --- | --- | 
| [1, 2, 3] | 0 | 
| [1, 3, 2] | 2 | 
| [2, 1, 3] | 2 | 
| [2, 3, 1] | 4 | 
| [3, 1, 2] | 4 | 
| [3, 2, 1] | 4 | 

Các giá trị riêng biệt là {0, 2, 4}, vì vậy câu trả lời là 3. 

Công thức cho 9 // 4 + 1 = 2 + 1 = 3. 

Điều này cho thấy rằng chỉ các giá trị chẵn mới xuất hiện và phạm vi đã được điền đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm là một phép tính số học duy nhất | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài biến | 

Việc tính toán là thời gian không đổi cho mỗi trường hợp thử nghiệm, dễ dàng nằm trong giới hạn ngay cả khi t = 100 tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        out.append(str(n * n // 4 + 1))
    return "\n".join(out)

# provided samples
assert run("5\n2\n3\n8\n15\n43\n") == "2\n3\n17\n57\n463"

# minimum size
assert run("1\n1\n") == "1"

# small even/odd comparison
assert run("2\n2\n3\n") == "2\n3"

# larger boundary
assert run("1\n500\n") == str(500 * 500 // 4 + 1)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n = 1 | 1 | trường hợp cạnh hoán vị đơn | 
| n = 2,3 | 2,3 | cấu trúc không tầm thường nhỏ nhất | 
| n = 500 | 62501 | xử lý hạn chế tối đa | 

## Vỏ cạnh 

Với n = 1 thì chỉ có một hoán vị. Thuật toán tính 1 * 1 // 4 + 1 = 1, đếm chính xác giá trị duy nhất có thể đạt được là 0. 

Với n = 2, phép tính cho kết quả 4 // 4 + 1 = 2. Hai hoán vị tạo ra giá trị 0 và 2, khớp với dự đoán. 

Đối với n lớn chẳng hạn như 500, công thức vẫn áp dụng trực tiếp mà không gặp sự cố tràn trong Python. Cấu trúc không phụ thuộc vào phép liệt kê hoặc tạo tổ hợp, do đó hiệu suất không đổi. 

Những trường hợp này xác nhận rằng cả đầu vào tối thiểu và tối đa đều được xử lý thống nhất bằng cùng một biểu thức số học.
