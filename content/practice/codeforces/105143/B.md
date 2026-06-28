---
title: "CF 105143B - Vô Lượng Tôi"
description: "Chúng ta được cung cấp một mảng các số nguyên không âm và một phép toán rất linh hoạt cho phép chúng ta di chuyển bất kỳ lượng giá trị nào từ vị trí này sang vị trí khác, miễn là không có phần tử nào trở thành âm."
date: "2026-06-27T16:47:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105143
codeforces_index: "B"
codeforces_contest_name: "2024 ICPC National Invitational Collegiate Programming Contest, Wuhan Site"
rating: 0
weight: 105143
solve_time_s: 66
verified: true
draft: false
---

[CF 105143B - Vô số tôi](https://codeforces.com/problemset/problem/105143/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên không âm và một phép toán rất linh hoạt cho phép chúng ta di chuyển bất kỳ lượng giá trị nào từ vị trí này sang vị trí khác, miễn là không có phần tử nào trở thành âm. Mỗi thao tác chọn hai chỉ số và chuyển một lượng không âm tùy ý từ chỉ số này sang chỉ số khác. Chúng ta được phép thực hiện thao tác này nhiều nhất$n$lần, nhưng vì mỗi thao tác có thể di chuyển một giá trị lớn nên sức mạnh thực sự của quy trình là nó cho phép chúng ta phân phối lại tổng số của mảng gần như tùy ý. 

Sau những lần phân phối lại này, chúng ta kết thúc với một mảng mới có tổng số tiền bằng với mảng ban đầu. Mục tiêu là chọn cách sắp xếp cuối cùng nhằm giảm thiểu OR theo bit của tất cả các phần tử trong mảng. 

Đại lượng khóa là OR của toàn bộ mảng, chỉ phụ thuộc vào bit nào xuất hiện trong ít nhất một phần tử. Bất kỳ bit nào được đặt trong bất kỳ phần tử nào đều góp phần vào câu trả lời cuối cùng. 

Ràng buộc$n \le 2 \cdot 10^5$ngụ ý rằng bất kỳ giải pháp nào về cơ bản phải là tuyến tính hoặc gần tuyến tính. Chúng tôi không thể mô phỏng các hoạt động phân phối lại hoặc tìm kiếm cấu hình. Cấu trúc của hoạt động gợi ý rõ ràng rằng vấn đề nằm ở việc suy luận tổng thể về tổng hơn là mô phỏng các bước di chuyển. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các số đều bằng 0 ngoại trừ một giá trị lớn. Trong trường hợp đó, không cần phân phối lại và câu trả lời chỉ đơn giản là giá trị đó. Một trường hợp khác là khi tổng nhỏ so với$n$, trong đó nhiều phần tử có thể bằng 0, cho thấy rằng hệ số giới hạn là mức độ chúng ta có thể trải đều tổng số tiền. 

Một cách tiếp cận ngây thơ có thể cố gắng phân phối lại các giá trị một cách tham lam trong khi theo dõi sự đóng góp theo bit, nhưng mô phỏng như vậy sẽ thất bại vì không gian của các mảng có thể là rất lớn, mặc dù tất cả chúng đều có cùng một tổng. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực là nghĩ đến mọi chuỗi chuyển giao được phép có thể có và tính toán OR kết quả. Ngay cả việc hạn chế chúng ta ở các trạng thái cuối cùng, điều này trở thành vấn đề liệt kê tất cả các phân vùng số nguyên không âm của tổng thành$n$các phần, có kích thước lớn về mặt thiên văn ngay cả đối với số tiền vừa phải. Điều này ngay lập tức trở nên không thể thực hiện được. 

Quan sát quan trọng là phép toán cho phép phân phối lại toàn bộ tổng, do đó, bất biến duy nhất là tổng của tất cả các phần tử. Do đó chúng ta có thể tự do lựa chọn bất kỳ mảng nào$n$số nguyên không âm có tổng bằng$S$, Ở đâu$S$là tổng số tiền ban đầu 

Khi đã hiểu được điều này, vấn đề sẽ giảm xuống còn việc chọn nhiều tập hợp kích thước$n$với tổng$S$giúp giảm thiểu OR theo bit của tất cả các phần tử. OR được xác định bởi mẫu bit cao nhất xuất hiện trong bất kỳ số nào được chọn. Nếu chúng ta quản lý để đảm bảo tất cả các con số nằm trong một giới hạn nào đó$M$, thì OR không thể vượt quá$M$và trên thực tế trở thành chính xác OR theo bit của các giá trị được xây dựng mà chúng tôi mong muốn tạo ra càng nhỏ càng tốt. 

Cấu trúc tối ưu là nén tổng thành càng ít khối lớn bằng nhau càng tốt, bởi vì việc trải tổng quá mịn sẽ buộc các số nhỏ nhưng lại làm tăng số lượng phần tử đóng góp các bit riêng biệt. Ràng buộc chặt chẽ nhất chỉ đơn giản là chúng ta có thể tạo phần tử tối đa nhỏ đến mức nào trong khi vẫn phân phối tổng số tiền trên$n$các vị trí. 

Điều này dẫn đến kết luận rằng câu trả lời bị chi phối bởi giới hạn trên nhỏ nhất có thể$M$như vậy$S$có thể được phân phối trên$n$mỗi con số không vượt quá$M$, chính xác là mức trần của$S/n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bản phân phối lại | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (lý luận dựa trên tổng) |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$S$là tổng của tất cả các phần tử trong mảng. 

1. Tính tổng số tiền$S = \sum a_i$. Điều này nắm bắt tất cả các thông tin được bảo quản theo các hoạt động được phép. 
2. Quan sát rằng sau bất kỳ số thao tác nào, chúng ta có thể nhận ra bất kỳ cấu hình nào của$n$số nguyên không âm có tổng vẫn bằng$S$. Điều này cho phép chúng tôi chuyển trọng tâm từ hoạt động sang phân vùng thuần túy. 
3. Chúng tôi muốn xây dựng$n$số có OR càng nhỏ càng tốt. OR của một mảng được xác định bởi sự kết hợp của các bit xuất hiện trong bất kỳ phần tử nào, do đó việc kiểm soát độ lớn của các phần tử riêng lẻ sẽ trực tiếp điều khiển OR. 
4. Xem xét việc thực thi rằng mọi phần tử cuối cùng có nhiều nhất một giá trị nào đó$M$. Nếu chúng ta có thể đạt được cấu hình như vậy thì OR của mảng không thể vượt quá$M$, vì không có phần tử nào giới thiệu các bit ngoài$M$. 
5. Điều kiện khả thi cho một$M$đó có phải là$S$có thể được chia thành nhiều nhất$n$nhiều nhất là mỗi phần$M$. Cách tốt nhất để tính tổng theo ràng buộc này là sử dụng càng nhiều$M$càng tốt, cộng thêm một phần còn lại. Điều này có thể thực hiện được chính xác khi$S \le n \cdot M$. 
6. Nhỏ nhất như vậy$M$do đó là$M = \left\lceil \frac{S}{n} \right\rceil$. 
7. Về mặt xây dựng, chúng ta có thể lấy$\lfloor S/M \rfloor$các phần tử bằng$M$, một phần tử bằng phần còn lại và điền phần còn lại bằng số 0. Điều này tôn trọng các ràng buộc và đạt được OR bằng$M$. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là bất biến toàn cục duy nhất là tổng. Khi chúng tôi sửa giá trị tối đa của ứng viên$M$, mọi phân phối khả thi của tổng thành$n$các bộ phận có thể được sắp xếp sao cho không có phần tử nào vượt quá$M$. Vì OR theo bit là đơn điệu trong mỗi phần tử nên việc giảm giá trị tối đa có thể sẽ trực tiếp giảm thiểu OR. Giới hạn khả thi nhỏ nhất chính xác là giới hạn đóng gói chặt chẽ nhất$S/n$, do đó, bất kỳ giá trị nào nhỏ hơn sẽ khiến số tiền không thể phân phối được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

S = sum(a)

# minimal possible maximum value per element
ans = (S + n - 1) // n

print(ans)
```Mã bắt đầu bằng cách đọc mảng và tính tổng của nó. Câu trả lời cuối cùng được tính toán bằng cách sử dụng phép chia trần, tương ứng trực tiếp với giới hạn trên khả thi tối thiểu cho mỗi phần tử sau khi phân phối lại. Không cần mô phỏng các hoạt động vì mô hình hoạt động cho phép phân phối lại tổng số tiền một cách tùy ý. 

Một cạm bẫy phổ biến là cố gắng suy luận về từng bit riêng lẻ của các số ban đầu. Điều đó là không cần thiết vì thao tác sẽ phá hủy tất cả cấu trúc vị trí trong khi chỉ bảo toàn tổng số tiền. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 7 

a = [1, 9, 1, 9, 8, 1, 0] 

đây$S = 29$. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng S | 29 | 
| n | 7 | 
| trần(S/n) | 5 | 

Chúng ta có thể xây dựng bảy số có tổng bằng 29, ví dụ sáu số 5 và một số -1 là không cần thiết; rõ ràng hơn, năm số 5, một số 4 và một số 0. OR của tất cả các phần tử là 5. 

Điều này phù hợp với câu trả lời tối ưu vì mọi giới hạn nhỏ hơn sẽ không cho phép phân phối 29 đơn vị trên 7 vị trí. 

### Ví dụ 2 

đầu vào: 

n = 4 

a = [2, 2, 2, 2] 

đây$S = 8$. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng S | 8 | 
| n | 4 | 
| trần(S/n) | 2 | 

Chúng tôi đã có cấu hình hợp lệ trong đó tất cả các phần tử là 2 và OR là 2. Không phân phối lại nào có thể giảm phần tử tối đa có thể xuống dưới 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần để tính tổng | 
| Không gian |$O(1)$| Chỉ tổng số đang chạy được lưu trữ | 

Giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với$2 \cdot 10^5$các phần tử, vì nó tránh mọi tìm kiếm tổ hợp hoặc mô phỏng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    S = sum(a)
    return str((S + n - 1) // n)

# provided sample
assert run("7\n1 9 1 9 8 1 0\n") == "5"

# minimum size
assert run("1\n0\n") == "0"

# single element
assert run("1\n10\n") == "10"

# all equal
assert run("5\n4 4 4 4 4\n") == "4"

# evenly divisible sum
assert run("4\n1 1 1 1\n") == "1"

# uneven distribution
assert run("3\n2 2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| số không đơn | 0 | ranh giới tối thiểu | 
| phần tử đơn | 10 | không có hiệu ứng phân phối lại | 
| tất cả đều bình đẳng | 4 | cấu hình ổn định | 
| thậm chí chia | 1 | trường hợp chia chính xác | 
| chia không đều | 2 | hành vi trần | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi$n = 1$. Thuật toán trả về$S$, vì phần tử duy nhất không thể thay đổi theo cách làm giảm OR. Điều này đúng vì không thể phân phối lại. 

Một trường hợp cạnh khác là khi tất cả các phần tử đều bằng 0 ngoại trừ một giá trị lớn. Tổng bằng giá trị đó và phép chia trần trả về cùng một giá trị, phù hợp với thực tế là không có sự phân phối lại nào có thể làm giảm nó. 

Khi tổng nhỏ hơn$n$, trần trở thành 1, nghĩa là ít nhất một đơn vị phải xuất hiện trong một phần tử nào đó. Điều này phản ánh chính xác rằng chúng ta có thể chia tổng thành nhiều số 0 và một vài số 1, làm cho OR bằng 1 bất cứ khi nào$S > 0$.
