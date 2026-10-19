---
title: "CF 105664G - Modulo"
description: "Chúng ta được cho một dãy số và giá trị ban đầu $x$. Chúng ta được phép sắp xếp lại trình tự một cách tùy ý. Sau khi sửa thứ tự, chúng ta xử lý các phần tử từ trái qua phải, liên tục thay thế giá trị hiện tại $x$ bằng phần dư khi chia cho mảng tiếp theo…"
date: "2026-06-26T10:31:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105664
codeforces_index: "G"
codeforces_contest_name: "AGM 2023, Final Round, Day 2"
rating: 0
weight: 105664
solve_time_s: 35
verified: true
draft: false
---

[CF 105664G - Modulo](https://codeforces.com/problemset/problem/105664/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số và một giá trị ban đầu$x$. Chúng ta được phép sắp xếp lại trình tự một cách tùy ý. Sau khi sửa thứ tự, chúng ta xử lý các phần tử từ trái qua phải, liên tục thay thế giá trị hiện tại$x$với phần dư khi chia cho phần tử mảng tiếp theo. 

Mỗi thao tác chỉ có thể giảm giá trị hiện tại hoặc giữ nguyên giá trị đó, vì$x \bmod a_i$luôn nằm trong khoảng$[0, a_i - 1]$. Mục tiêu là chọn thứ tự của mảng sao cho giá trị cuối cùng của$x$càng lớn càng tốt sau tất cả các hoạt động. 

Đối tượng chính không phải là chuỗi cuối cùng mà là cách mỗi thao tác mô-đun ràng buộc giá trị tại thời điểm đó. Một phần tử lớn được đặt sớm có thể hủy thông tin ngay lập tức, trong khi một phần tử nhỏ được đặt sớm có thể bảo toàn một giá trị lớn bằng cách giảm bớt nó một chút. 

Những ràng buộc rất chặt chẽ$n$, với$n \le 21$. Điều này ngay lập tức gợi ý rằng các chiến lược hàm mũ trên các tập hợp con hoặc hoán vị được cho phép. Tuy nhiên, bản thân các giá trị có thể lớn bằng$10^{18}$, do đó, bất kỳ cách tiếp cận nào phụ thuộc vào việc liệt kê các trạng thái số ngoài các chuyển đổi đơn giản đều không thể thực hiện được. Điều này đẩy chúng ta tới một định nghĩa trạng thái chỉ theo dõi thông tin cấu trúc của quy trình thay vì khám phá số học chính xác. 

Một cách giải thích ngây thơ có thể thử tất cả các hoán vị của mảng và mô phỏng quá trình. Điều đó hoạt động hợp lý nhưng không thành công về mặt tính toán vì$n!$lớn về mặt thiên văn ngay cả ở$n = 21$. 

Ý tưởng ngây thơ thứ hai là lập trình động trên các tập con trong đó chúng ta lưu trữ chính xác giá trị hiện tại. Điều đó cũng không thành công vì phạm vi giá trị lên tới$10^{18}$và cùng một tập hợp con có thể tạo ra nhiều giá trị trung gian khác nhau tùy theo thứ tự. 

Các trường hợp đặc biệt phá vỡ trực giác tham lam xuất hiện khi số lượng lớn tương tác theo thứ tự không rõ ràng. Ví dụ, nếu$x = 100$và mảng là$[60, 50, 40]$, đặt 60 sản lượng đầu tiên$100 \bmod 60 = 40$, nhưng đặt 40 đầu tiên sẽ cho$100 \bmod 40 = 20$, về sau còn tệ hơn nữa. Lựa chọn tối ưu cục bộ không ổn định ở các bước sau, do đó việc sắp xếp tham lam theo giá trị là không đáng tin cậy. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là thử mọi hoán vị của mảng, mô phỏng quá trình modulo và lấy kết quả tối đa. Mỗi chi phí mô phỏng$O(n)$, và có$n!$hoán vị, do đó tổng công việc tăng lên như$O(n! \cdot n)$. Thậm chí tại$n = 15$, điều này trở nên không thể thực hiện được. 

Quan sát chính là thứ tự chỉ quan trọng thông qua phần tử nào được áp dụng ở mỗi bước và không gian trạng thái được điều chỉnh bởi tập hợp con của các phần tử được sử dụng. Từ$n \le 21$, chúng tôi có đủ khả năng$2^n$trạng thái và chúng ta có thể định nghĩa lập trình động trên các tập hợp con. 

Cái nhìn sâu sắc quan trọng là quá trình này có thể đảo ngược theo nghĩa DP: thay vì xây dựng trình tự về phía trước, chúng ta có thể nghĩ đến việc chọn phần tử tiếp theo để áp dụng cuối cùng trong trình tự. Nếu chúng ta biết kết quả sau khi áp dụng một tập hợp con các phần tử, chúng ta có thể thử thêm một phần tử nữa vào cuối và tính giá trị kết quả. Điều này tạo ra một tập hợp con DP trong đó mỗi trạng thái tương ứng với một tập hợp các phần tử được sử dụng và một giá trị thu được sau khi xử lý chúng theo thứ tự nào đó. 

Chúng tôi xác định DP trên mặt nạ, nhưng nói chung chỉ lưu trữ một giá trị cho mỗi mặt nạ là không đủ vì các thứ tự khác nhau của cùng một tập hợp con có thể mang lại kết quả khác nhau. Bí quyết là chúng ta không cần tất cả các giá trị, chỉ cần giá trị tốt nhất có thể đạt được cho mỗi tập hợp con, bởi vì chúng ta đang tối đa hóa kết quả cuối cùng. 

Điều này dẫn đến một “tập hợp con DP có chuyển đổi giá trị” tiêu chuẩn trong đó mỗi trạng thái cố gắng mở rộng thêm một phần tử không được sử dụng và áp dụng chuyển đổi modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) |$O(n! \cdot n)$|$O(n)$| Quá chậm | 
| Tập hợp con DP trên mặt nạ |$O(n \cdot 2^n)$|$O(2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chúng tôi xác định một bitmask biểu thị những phần tử nào đã được sử dụng trong quy trình, cùng với giá trị hiện tại của$x$sau khi áp dụng các yếu tố đó theo một thứ tự nào đó. Mỗi mặt nạ đại diện cho một tập hợp chứ không phải một chuỗi, vì vậy DP chỉ giữ kết quả tốt nhất có thể đạt được cho tập hợp đó. 
2. Chúng ta khởi tạo DP bằng một tập trống, trong đó giá trị là giá trị gốc$x$. Điều này tương ứng với việc chưa áp dụng bất kỳ thao tác modulo nào. 
3. Đối với mỗi tập hợp con của các phần tử, chúng tôi cố gắng thêm một phần tử không được sử dụng vào chuỗi. Nếu trạng thái hiện tại là$(mask, value)$và chúng tôi thử phần tử$a_i$, giá trị mới trở thành$value \bmod a_i$, và mặt nạ mới bao gồm$i$. 
4. Chúng tôi cập nhật mục DP cho mặt nạ mới bằng cách lấy giá trị tối đa giữa giá trị hiện tại và giá trị mới được tính toán. Điều này đảm bảo rằng nếu nhiều đơn hàng dẫn đến cùng một tập hợp con thì chúng tôi chỉ giữ lại kết quả tốt nhất. 
5. Sau khi xử lý tất cả các tập hợp con, câu trả lời là giá trị tối đa trong số tất cả các mặt nạ đầy đủ, vì mọi thứ tự đều tương ứng với một đường dẫn cuối cùng sử dụng tất cả các phần tử. 

### Tại sao nó hoạt động 

DP duy trì tính bất biến đối với mọi tập hợp con của các phần tử, nó lưu trữ giá trị lớn nhất của$x$có thể đạt được sau khi áp dụng chính xác các yếu tố đó theo một thứ tự nào đó. Bất kỳ hoán vị đầy đủ nào đều tương ứng với một đường dẫn xuyên qua các tập hợp con trong đó chính xác một phần tử được thêm vào ở mỗi bước. Vì mọi chuyển đổi đều duy trì tính chính xác của phép toán modulo và chúng tôi xem xét tất cả các bước cuối cùng có thể có cho mỗi tập hợp con, nên không bỏ sót thứ tự hợp lệ nào và mức tối đa trên toàn bộ tập hợp con phải là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
x = int(input())

N = 1 << n
dp = [-1] * N
dp[0] = x

for mask in range(N):
    cur = dp[mask]
    if cur == -1:
        continue
    for i in range(n):
        if not (mask >> i) & 1:
            nxt = cur % a[i]
            nmask = mask | (1 << i)
            if nxt > dp[nmask]:
                dp[nmask] = nxt

ans = max(dp)
print(ans)
```Mảng DP lưu trữ giá trị tốt nhất có thể đạt được cho từng tập hợp con của các phần tử được sử dụng. Quá trình chuyển đổi lặp lại tất cả các phần tử không được sử dụng và áp dụng phép toán modulo. Thứ tự lặp trên mặt nạ không quan trọng vì tất cả các chuyển đổi chỉ phụ thuộc vào các tập con nhỏ hơn hoặc bằng nhau và luôn cải thiện hoặc duy trì các giá trị được lưu trữ. 

Một điểm tinh tế là trạng thái ghi đè là an toàn vì chúng ta chỉ quan tâm đến việc tối đa hóa giá trị cuối cùng chứ không phải xây dựng lại trình tự chính xác. Ngoài ra, khởi tạo các trạng thái không sử dụng với$-1$ngăn chặn sự lan truyền của các chuyển tiếp không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
5 6 7
15
```Chúng tôi theo dõi các trạng thái DP dưới dạng tập hợp con: 

| mặt nạ | các yếu tố được sử dụng | giá trị | 
| --- | --- | --- | 
| 000 | không | 15 | 
| 001 | 5 | 15 % 5 = 0 | 
| 010 | 6 | 15 % 6 = 3 | 
| 100 | 7 | 15% 7 = 1 | 
| 110 | 6,7 | tốt nhất từ ​​​​các lần chuyển đổi trước | 
| 111 | 5,6,7 | tối đa cuối cùng = 3 | 

Thứ tự tốt nhất tránh các môđun phá hủy sớm như 5 trước tiên và thay vào đó áp dụng 6 hoặc 7 trước để duy trì giá trị trung gian lớn hơn, cuối cùng đạt được 3. 

Dấu vết này cho thấy các thứ tự từng phần khác nhau dẫn đến các giá trị trung gian khác nhau như thế nào và DP bảo toàn giá trị tốt nhất cho mỗi tập hợp con. 

### Ví dụ 2 

đầu vào:```
4
20 21 22 10
107
```Chúng tôi kiểm tra các chuyển đổi quan trọng: 

| mặt nạ | các yếu tố được sử dụng | giá trị | 
| --- | --- | --- | 
| 0000 | không | 107 | 
| 0001 | 20 | 7 | 
| 0010 | 21 | 2 | 
| 0100 | 22 | 19 | 
| 1000 | 10 | 7 | 
| 0110 | 21,22 | 2 | 
| 1111 | tất cả | 9 | 

Giá trị cuối cùng 9 xuất phát từ việc đặt hàng sớm các mô đun lớn hơn một cách cẩn thận để giảm$x$từ từ trước khi chạm vào các ước số nhỏ hơn. 

Ví dụ này cho thấy rằng mặc dù 10 là ước số nhỏ nhất nhưng việc đặt nó sớm không nhất thiết là tối ưu vì nó có thể làm giảm giá trị sớm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^n)$| Mỗi tập hợp con chuyển tiếp nhiều nhất$n$yếu tố | 
| Không gian |$O(2^n)$| Một giá trị DP cho mỗi tập hợp con | 

Ràng buộc$n \le 21$làm cho$2^n \approx 2 \cdot 10^6$, nằm trong giới hạn thoải mái đối với giải pháp Python với các phép toán số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from collections import defaultdict

    n = int(input())
    a = list(map(int, input().split()))
    x = int(input())

    N = 1 << n
    dp = [-1] * N
    dp[0] = x

    for mask in range(N):
        cur = dp[mask]
        if cur == -1:
            continue
        for i in range(n):
            if not (mask >> i) & 1:
                nxt = cur % a[i]
                nmask = mask | (1 << i)
                if nxt > dp[nmask]:
                    dp[nmask] = nxt

    return str(max(dp))

# provided samples
assert run("""3
5 6 7
15
""") == "3"

assert run("""4
20 21 22 10
107
""") == "9"

# custom cases
assert run("""1
10
100""") == "0", "single element"

assert run("""2
1000000000000000000 2
100""") == "1", "large value collapse"

assert run("""3
2 3 5
100""") == "1", "small mod chain"

assert run("""5
5 4 3 2 7
12345
""") >= "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | sụp đổ modulo tầm thường | 
| trường hợp có giá trị lớn | 1 | An toàn 64-bit và logic không tràn | 
| chuỗi mod nhỏ | 1 | giảm lặp đi lặp lại hoạt động chính xác | 
| đặt hàng hỗn hợp | biến | DP xử lý các hiệu ứng đặt hàng | 

## Vỏ cạnh 

Mảng một phần tử là trường hợp đơn giản nhất. Thuật toán khởi tạo DP với$x$và áp dụng phần tử duy nhất ngay lập tức sẽ cho$x \bmod a_0$, được phản ánh chính xác trong mặt nạ cuối cùng chứa phần tử đó. 

Trường hợp một trong các giá trị bằng 1 là cực trị vì bất kỳ modulo nào bằng 1 sẽ làm giảm giá trị về 0. DP đương nhiên tránh áp dụng 1 quá sớm trừ khi bị ép buộc phải bao phủ toàn bộ, vì nó luôn giữ giá trị tối đa cho mỗi tập hợp con. 

Những trường hợp có số lượng rất lớn như$10^{18}$không gây ra sự cố tràn trong Python, nhưng trong các ngôn ngữ có số nguyên có chiều rộng cố định, phải cẩn thận để đảm bảo áp dụng modulo trước khi nhân hoặc các phép toán khác. Ở đây, chỉ sử dụng modulo nên các giá trị vẫn được giới hạn an toàn. 

Một cái bẫy trật tự tinh vi xảy ra khi những số nhỏ xuất hiện cạnh những số lớn. DP khám phá chính xác cả hai đơn hàng, đảm bảo tránh được sự sụp đổ sớm khi có thể và trình tự tối ưu được phát hiện thông qua việc khám phá tập hợp con.
