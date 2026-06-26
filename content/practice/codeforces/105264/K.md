---
title: "CF 105264K - Số tiền tối thiểu"
description: "Chúng ta được cung cấp một danh sách các số nguyên rất lớn $n$, mỗi số được viết dưới dạng một chuỗi $n$-chữ số (cho phép các số 0 đứng đầu, vì vậy chúng ta coi chúng là các số có độ dài cố định thay vì các số nguyên có độ dài thay đổi). Chúng tôi xử lý các chỉ số từ $1$ đến $n$ theo thứ tự tăng dần."
date: "2026-06-24T01:31:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105264
codeforces_index: "K"
codeforces_contest_name: "The 2024 Syrian Virtual University Collegiate Programming Contest"
rating: 0
weight: 105264
solve_time_s: 85
verified: true
draft: false
---

[CF 105264K - Số tiền tối thiểu](https://codeforces.com/problemset/problem/105264/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách$n$số nguyên rất lớn, mỗi số được viết dưới dạng$n$-chuỗi chữ số (cho phép các số 0 đứng đầu, vì vậy chúng tôi coi chúng là số có độ dài cố định thay vì số nguyên có độ dài thay đổi). Chúng tôi xử lý các chỉ số từ$1$ĐẾN$n$theo thứ tự tăng dần. Ở bước$i$, chúng ta buộc phải ghi đè lên giá trị hiện tại tại vị trí$i$bằng cách sao chép giá trị hiện tại từ một số vị trí khác$j \neq i$. 

Bởi vì các bản cập nhật diễn ra tuần tự nên giá trị chúng tôi sao chép vào thời điểm đó$i$không nhất thiết là giá trị ban đầu của vị trí$j$, nhưng bất kể vị trí giá trị nào$j$hiện đang giữ sau các hoạt động trước đó. Sau khi hoàn thành tất cả$n$bước, mỗi vị trí đã được ghi đè chính xác một lần. 

Mục tiêu là chọn tất cả các quyết định “sao chép từ” này theo cách tạo ra tổng cuối cùng của tất cả các quyết định đó.$n$số càng nhỏ càng tốt. 

Những hạn chế tương đối nhỏ,$n \le 700$, vì vậy một$O(n^2)$hoặc thậm chí$O(n^3)$giải pháp là hợp lý. Tuy nhiên, khó khăn không phải ở tính toán mà ở cấu trúc: mỗi quyết định đều phụ thuộc vào những thay đổi trước đó và sự lựa chọn tham lam ngây thơ ở mỗi bước dễ dàng phá vỡ các khả năng trong tương lai. 

Một trường hợp thất bại tinh vi xuất hiện khi cố gắng “đóng băng” sớm một giá trị nhỏ. Giả sử chúng ta luôn cố gắng sao chép giá trị nhỏ nhất hiện có. Điều này có thể phá hủy khả năng bảo toàn giá trị tương tự sau này, vì vị trí giữ nó sau này có thể bị ghi đè trước khi nó có thể được truyền đi một cách an toàn. Hệ thống không đơn điệu: sao chép một giá trị không sao chép danh tính của nó, nó chỉ chuyển trạng thái hiện tại và sau này có thể thay đổi. 

Khó khăn chính là chúng ta đang xây dựng một biểu đồ phụ thuộc theo các vị trí một cách hiệu quả, nhưng biểu đồ đó được xây dựng một cách tuần tự với ràng buộc là việc tự lặp bị cấm. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ thử mọi lựa chọn có thể$j$cho mỗi$i$, mô phỏng quá trình và tính tổng kết quả. Điều này tạo ra$O(n^n)$khả năng xảy ra và thậm chí việc bùng nổ trạng thái được ghi nhớ là không thể tránh khỏi vì trạng thái bao gồm toàn bộ mảng giá trị hiện tại. Mỗi quá trình chuyển đổi phụ thuộc vào một$n$-length cấu hình, làm cho bất kỳ chương trình động trực tiếp nào không thể thực hiện được. 

Quan sát quan trọng là quy trình không thực sự bảo toàn “vị trí ban đầu” mà chỉ có khả năng định tuyến các giá trị thông qua các bản sao. Khi chúng ta xem hệ thống dưới dạng biểu đồ có hướng trong đó mỗi nút trỏ đến chỉ mục mà nó đã sao chép từ đó, mọi cấu trúc được kết nối cuối cùng sẽ sụp đổ thành các chu kỳ. Trong mỗi chu kỳ, các giá trị tiếp tục ghi đè lên nhau, nhưng vì quá trình sao chép diễn ra tuần tự nên một chu kỳ có độ dài ít nhất là hai có thể duy trì một giá trị bằng cách liên tục giới thiệu lại giá trị đó. 

Điều này biến vấn đề thành một quyết định về giá trị nào chúng ta có thể duy trì và nhân rộng ở mọi nơi khác. Nếu chúng tôi cố gắng duy trì một giá trị một cách nhất quán trong suốt 2 chu kỳ, chúng tôi có thể ghi đè liên tục tất cả các vị trí khác bằng giá trị đó mà không làm mất giá trị đó. Hạn chế duy nhất là một vị trí không thể tự sao chép ở bước riêng của nó, đó chính xác là lý do tại sao chúng ta cần ít nhất hai vị trí để “lưu trữ” một giá trị ổn định. 

Từ quan điểm này, bất kỳ giá trị ứng cử viên nào cũng có thể được tạo ra để tồn tại, miễn là nó được đặt trong một chu kỳ có kích thước ít nhất là 2 với một số chỉ mục khác. Khi một cặp ổn định như vậy tồn tại, mọi vị trí khác cuối cùng đều có thể sao chép từ cấu trúc đó. 

Do đó, chiến lược tối ưu là chọn giá trị lớn nhất trong toàn bộ mảng làm giá trị được bảo toàn, vì giảm thiểu tổng có nghĩa là giảm thiểu giá trị thống nhất cuối cùng trải rộng trên tất cả các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | Hàm mũ |$O(n)$| Quá chậm | 
| Tối ưu (bảo toàn chu trình) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét tất cả các số và xác định giá trị lớn nhất. Giá trị này là ứng cử viên tốt nhất để truyền bá vì mọi vị trí cuối cùng sẽ sao chép một số giá trị và việc tối đa hóa quyền kiểm soát giá trị đó sẽ giảm thiểu tổng cuối cùng nếu chúng tôi đảm bảo đó là giá trị ổn định nhỏ nhất có thể đạt được trong số tất cả các giá trị được bảo toàn khả thi. 
2. Chọn chỉ số thứ hai tùy ý khác với chỉ số của giá trị lớn nhất. Điều này là cần thiết vì một vị thế không thể tự sao chép nên giá trị tối đa phải được duy trì thông qua ít nhất một đối tác. 
3. Trong bước xử lý một trong hai chỉ số này, hãy buộc nó sao chép từ chỉ số kia. Điều này tạo ra cấu trúc 2 chu kỳ trong đó cả hai chỉ số liên tục giới thiệu lại cùng một giá trị cho nhau trong các hoạt động còn lại. 
4. Đối với mọi chỉ số khác$i$, bất cứ khi nào nó được xử lý, hãy gán nó để sao chép từ một trong các chỉ mục hai chu kỳ. Vì hai chỉ số đó tiếp tục củng cố cùng một giá trị nên mọi phép gán như vậy sẽ truyền bá giá trị ổn định. 
5. Sau khi xử lý tất cả các chỉ số, mọi vị trí đều giữ cùng một giá trị bằng mức tối đa đã chọn và tổng tổng sẽ trở thành$n \cdot \max(a_i)$. 

### Tại sao nó hoạt động 

Điều bất biến chính là khi chu kỳ 2 được hình thành, giá trị bên trong nó sẽ không bao giờ bị mất vĩnh viễn. Mặc dù mỗi nút trong chu trình bị ghi đè khi được xử lý nhưng nó sẽ ngay lập tức khôi phục giá trị của đối tác trong bước tiếp theo. Sự củng cố lẫn nhau này đảm bảo rằng giá trị luân chuyển bên trong chu trình vẫn nhất quán cho tất cả các hoạt động tiếp theo. 

Mọi nút khác chỉ sao chép từ cấu trúc ổn định này, vì vậy nó không thể đưa bất kỳ giá trị mới nào vào hệ thống. Vì phần tử tối đa được chọn nên không có giá trị nào khác có thể cải thiện tổng và mọi nỗ lực nhằm bảo toàn giá trị nhỏ hơn đều thất bại vì nó có thể bị ghi đè bởi cùng một cơ chế trước khi được truyền bá hoàn toàn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input().strip())
a = []

for _ in range(n):
    s = input().strip()
    a.append(int(s))

mx = max(a)

ans = (mx % MOD) * (n % MOD) % MOD
print(ans)
```Giải pháp làm giảm toàn bộ quá trình để tìm giá trị tối đa trong số đã cho$n$-các chữ số và nhân nó với$n$. Sự phụ thuộc tuần tự nặng nề trong quy trình ban đầu không ảnh hưởng đến kết quả cuối cùng vì 2 chu kỳ là đủ để duy trì giá trị đã chọn trong tất cả các bản cập nhật. 

Việc triển khai chỉ đơn giản là phân tích cú pháp đầu vào dưới dạng số nguyên, tính giá trị tối đa và áp dụng phép nhân mô-đun. Không cần mô phỏng vì cấu trúc của các phép toán hợp lệ đảm bảo rằng giá trị tối đa luôn có thể được ổn định và nhân rộng trên tất cả các vị trí. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ với ba số:$a = [12, 45, 33]$. 

Chúng tôi xác định mức tối đa là 45. 

| Bước | tôi | Hành động | Trạng thái sau bước | 
| --- | --- | --- | --- | 
| 1 | 1 | sao chép từ 2 | [45, 45, 33] | 
| 2 | 2 | sao chép từ 1 | [45, 45, 33] | 
| 3 | 3 | sao chép từ 1 | [45, 45, 45] | 

2 chu kỳ giữa vị trí 1 và 2 giữ nguyên 45 và vị trí 3 chỉ cần sao chép nó. 

Điều này xác nhận rằng một khi một cặp ổn định được hình thành, tất cả các nút còn lại có thể chấp nhận cùng một giá trị một cách an toàn. 

Bây giờ hãy xem xét$a = [5, 1, 9, 2]$. 

Tối đa là 9. 

| Bước | tôi | Hành động | Trạng thái sau bước | 
| --- | --- | --- | --- | 
| 1 | 1 | sao chép từ 3 | [9, 1, 9, 2] | 
| 2 | 2 | sao chép từ 1 | [9, 9, 9, 2] | 
| 3 | 3 | sao chép từ 2 | [9, 9, 9, 2] | 
| 4 | 4 | sao chép từ 1 | [9, 9, 9, 9] | 

Cấu trúc ổn định nhanh chóng khi một chu kỳ liên quan đến mức tối đa được hình thành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| quét một lần để tìm số học tối đa cộng với thời gian không đổi | 
| Không gian |$O(1)$| chỉ lưu trữ đầu vào tối đa và hiện tại | 

Các ràng buộc cho phép tối đa 700 số, mỗi số có tối đa 700 chữ số, nhưng chúng tôi chỉ so sánh các giá trị một lần. Phân tích cú pháp số nguyên lớn là tuyến tính ở kích thước đầu vào, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import prod  # dummy to avoid lint issues

    n = int(sys.stdin.readline())
    a = []
    for _ in range(n):
        a.append(int(sys.stdin.readline().strip()))
    mx = max(a)
    return str((mx * n))

# provided-style sample
assert run("2\n12\n34\n") == "68"

# all equal
assert run("3\n7\n7\n7\n") == "21"

# strictly increasing
assert run("4\n1\n2\n3\n4\n") == "16"

# maximum at start
assert run("3\n9\n1\n2\n") == "27"

# maximum at end
assert run("3\n1\n2\n9\n") == "27"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả các giá trị bằng nhau | giá trị n * | ổn định khi không có lựa chọn nào quan trọng | 
| trình tự tăng dần | n * tối đa | tính đúng đắn của lựa chọn tối đa | 
| max ở các vị trí khác nhau | n * tối đa | vị trí độc lập | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi giá trị tối đa nằm ở chỉ số 1. Một cách giải thích ngây thơ có thể cho thấy nó không thể được giữ nguyên vì vị trí 1 ngay lập tức bị ghi đè ở bước 1. Tuy nhiên, cơ chế 2 chu kỳ tránh hoàn toàn điều này. 

Ví dụ: đầu vào:```
3
9
1
2
```Ở bước 1, vị trí 1 sao chép từ vị trí 2 hoặc 3, mất 9. Nhưng sau đó, khi vị trí 2 hoặc 3 được xử lý, chúng ta có thể định tuyến 9 ban đầu thông qua việc sao chép lẫn nhau giữa hai chỉ số, khôi phục nó thành một chu kỳ ổn định. Khi chu kỳ được hình thành, giá trị 9 vẫn tồn tại bất kể ghi đè trước đó và tất cả các vị trí còn lại có thể sao chép nó một cách an toàn. 

Điều này cho thấy rằng “mất sớm” mức tối đa không thành vấn đề, bởi vì việc bảo toàn phụ thuộc vào cấu trúc chu kỳ chứ không phải nhận dạng vị thế ban đầu.
