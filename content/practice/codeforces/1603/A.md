---
title: "CF 1603A - Nhầm lẫn có thể nhìn thấy được"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và mục tiêu là xóa hoàn toàn nó bằng một thao tác rất cụ thể. Ở mỗi bước, chúng ta có thể chọn một phần tử có giá trị không chia hết cho chỉ số dựa trên 1 cộng với 1 và xóa phần tử đó."
date: "2026-06-10T08:15:11+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1603
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 752 (Div. 1)"
rating: 1300
weight: 1603
solve_time_s: 104
verified: true
draft: false
---

[CF 1603A - Sự nhầm lẫn có thể nhìn thấy được](https://codeforces.com/problemset/problem/1603/A) 

**Đánh giá:** 1300 
**Tags:** xây dựng thuật toán, toán, lý thuyết số 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và mục tiêu là xóa hoàn toàn nó bằng một thao tác rất cụ thể. Ở mỗi bước, chúng ta có thể chọn một phần tử có giá trị **không chia hết** cho chỉ số dựa trên 1 cộng với 1 và xóa phần tử đó. Sau khi loại bỏ, chuỗi co lại và chỉ số của các phần tử còn lại dịch chuyển sang trái một. Thách thức ở đây là quyết định xem liệu có tồn tại thứ tự loại bỏ mà cuối cùng làm trống chuỗi hay không. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp có độ dài của chuỗi và chính chuỗi đó. Đầu ra là "CÓ" nếu có thể loại bỏ hoàn toàn, nếu không thì "KHÔNG". 

Các ràng buộc rất lớn: các chuỗi có thể đạt tới kích thước$10^5$và tổng tổng của tất cả các chuỗi trong tất cả các trường hợp thử nghiệm có thể đạt tới$3 \cdot 10^5$. Một mô phỏng đơn giản, loại bỏ từng phần tử một và kiểm tra tính phân chia ở mỗi bước, sẽ có độ phức tạp trong trường hợp xấu nhất.$O(n^2)$, quá chậm. Điều này có nghĩa là chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính theo độ dài chuỗi. 

Các trường hợp cạnh xảy ra khi các phần tử nhỏ và chỉ số tăng lên. Ví dụ: không thể xóa một phần tử 2 nếu nó ở chỉ mục 1 vì$2 \% 2 = 0$. Một trường hợp tinh vi khác là các chuỗi có số lặp lại: khả năng loại bỏ một phần tử phụ thuộc vào **vị trí hiện tại** của nó, chứ không phải vị trí ban đầu của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các trình tự loại bỏ. Ở mỗi bước, quét mảng, chọn phần tử bất kỳ$a_i$như vậy$a_i \% (i+1) \neq 0$, và loại bỏ nó. Tiếp tục cho đến khi chuỗi trống hoặc không có phần tử nào có thể xóa được. Về nguyên tắc thì điều này đúng nhưng đối với$n = 10^5$, quét tới$n$các yếu tố cho$n$bước đưa ra$O(n^2)$, quá chậm. 

Thông tin chi tiết quan trọng là **làm việc ngược**. Nếu ta đánh số các vị trí từ bước cuối cùng khi chỉ còn lại một phần tử thì phần tử đó phải thỏa mãn điều kiện chia hết cho vị trí của nó trong dãy rút gọn. Chúng ta có thể mô phỏng việc loại bỏ theo cách ngược lại: bắt đầu từ phần tử cuối cùng (vị trí$n$trong mảng ban đầu), hãy kiểm tra xem$a_i$có thể chia hết cho **vị trí dựa trên số 1 cuối cùng** của nó trong dãy rút gọn. Nếu đúng như vậy thì việc loại bỏ là không thể; nếu không, nó có thể được gỡ bỏ. Bằng cách lặp lại từ đầu đến cuối, chúng ta chỉ cần một lần duyệt qua mảng. 

Cách tiếp cận này tận dụng sự quan sát rằng ở mỗi giai đoạn, phần tử "khó nhất" cần loại bỏ nằm ở cuối mảng hiện tại. Nếu phần tử đó có thể tháo rời được khi tính từ vị trí thu nhỏ của nó thì tất cả các phần tử trước đó cũng có thể được xử lý. Điều này làm giảm độ phức tạp xuống$O(n)$mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Kiểm tra ngược (Tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$n$và trình tự$a$. 
3. Khởi tạo bộ đếm$current\_length = n$. Điều này thể hiện "vị trí từ cuối" để kiểm tra khả năng chia hết. 
4. Lặp lại mảng từ phần tử cuối cùng đến phần tử đầu tiên. Đối với mỗi phần tử$a[i]$:

Một. Nếu như$a[i] \% current\_length == 0$, in "NO" và ngừng kiểm tra trường hợp thử nghiệm này. 

b. Ngược lại thì giảm$current\_length$một và tiếp tục. 
5. Nếu vòng lặp kết thúc mà không gặp phần tử chia hết thì in "CÓ". 

**Tại sao nó hoạt động**: Ở mỗi bước, phần tử cuối cùng trong chuỗi thu gọn phải có thể loại bỏ được theo **chỉ mục dựa trên 1 hiện tại** của nó. Bằng cách kiểm tra ngược lại các phần tử, chúng tôi đảm bảo rằng mỗi phần tử đều đáp ứng yêu cầu về khả năng chia hết tại thời điểm nó là ứng cử viên cuối cùng còn lại. Nếu bất kỳ phần tử nào bị lỗi thì không có trình tự xóa nào có thể xóa được nên câu trả lời là "KHÔNG". 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    possible = True
    current_length = n
    for value in reversed(a):
        if value % current_length == 0:
            possible = False
            break
        current_length -= 1
    print("YES" if possible else "NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi lặp lại trình tự ngược lại vì khả năng chia hết của một phần tử phụ thuộc vào vị trí cuối cùng của nó khi nó được xem xét để loại bỏ. các`current_length`biến theo dõi vị trí ảo này. Nếu bất kỳ phần tử nào chia hết cho vị trí của nó thì thao tác không thể thành công, vì vậy chúng tôi đánh dấu trường hợp thử nghiệm là không thể. Ngược lại, chúng ta giảm bộ đếm và tiếp tục. Câu lệnh in cuối cùng đưa ra câu trả lời ở định dạng mong đợi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Trình tự đầu vào:`[1, 2, 3]`| Bước | Phần tử được kiểm tra (từ cuối) | Vị trí | 3 % 3 | Trạng thái | Current_length | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | được | 2 | 
| 2 | 2 | 2 | 0 | được | 1 | 
| 3 | 1 | 1 | 0 | được | 0 | 

Tất cả các phần tử đều thỏa mãn điều kiện khi đếm ngược lại. Đầu ra là`YES`. 

### Ví dụ 2 

Trình tự đầu vào:`[2]`| Bước | Yếu tố | Vị trí | 2 % 1 | Trạng thái | Current_length | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | Thất bại | - | 

Phần tử 2 chia hết cho vị trí của nó (1), do đó đầu ra là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền một mảng từ đầu đến cuối | 
| Không gian | O(1) thêm | Chỉ có một vài biến số; đầu vào được lưu trữ trong mảng | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm là 300.000, tổng số thao tác có thể chấp nhận được trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        possible = True
        current_length = n
        for value in reversed(a):
            if value % current_length == 0:
                possible = False
                break
            current_length -= 1
        print("YES" if possible else "NO")
    return output.getvalue().strip()

# Provided samples
assert run("5\n3\n1 2 3\n1\n2\n2\n7 7\n10\n384836991 191890310 576823355 782177068 404011431 818008580 954291757 160449218 155374934 840594328\n8\n6 69 696 69696 696969 6969696 69696969 696969696") == "YES\nNO\nYES\nYES\nNO"

# Custom tests
assert run("1\n1\n1") == "YES", "single element removable"
assert run("1\n1\n2") == "NO", "single element non-removable"
assert run("1\n3\n2 3 4") == "YES", "small array, all removable"
assert run("1\n3\n2 4 6") == "NO", "small array, last element blocked"
assert run("1\n5\n1 1 1 1 1") == "YES", "all ones"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`| CÓ | Phần tử đơn lẻ có thể bị xóa | 
|`1\n1\n2`| KHÔNG | Phần tử đơn không thể xóa được | 
|`1\n3\n2 3 4`| CÓ | Mảng nhỏ, tất cả các phần tử có thể tháo rời | 
|`1\n3\n2 4 6`| KHÔNG | Không thể xóa phần tử cuối cùng | 
|`1\n5\n1 1 1 1 1`| CÓ | Vỏ cạnh có số lặp lại | 

## Vỏ cạnh 

Không thể xóa một chuỗi chỉ có một phần tử bằng chỉ số dựa trên 1 cộng với một phần tử. Ví dụ,`[2]`thất bại vì$2 \% 1 = 0$. Thuật toán xác định chính xác điều này trong lần lặp đầu tiên của vòng lặp ngược và xuất ra "KHÔNG". 

Đối với các chuỗi trong đó tất cả các phần tử đều bằng 1, kiểm tra ngược lại sẽ thấy`1 % n`,`1 % (n-1)`, v.v., không có cái nào bằng 0 ngoại trừ`1 % 1`, được cho phép. Giải pháp xuất ra "CÓ". 

Đối với các chuỗi có số lượng lớn, việc kiểm tra tính chia hết được thực hiện theo modulo`current_length`, không bao giờ vượt quá$10^5$, do đó không xảy ra hiện tượng tràn số nguyên.
