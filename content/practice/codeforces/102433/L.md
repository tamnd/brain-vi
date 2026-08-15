---
title: "CF 102433L - Lỗi cam mang"
description: "Phép toán trong bài toán này là phép nhân thập phân thông thường với một thay đổi quan trọng: bất cứ khi nào một số sản phẩm ở cùng một vị trí thập phân, tổng của chúng được lấy theo modulo 10, do đó không có số nào chuyển sang vị trí tiếp theo."
date: "2026-08-12T07:42:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "L"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 73
verified: true
draft: false
---

[CF 102433L - Lỗi cam hộp đựng](https://codeforces.com/problemset/problem/102433/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Phép toán trong bài toán này là phép nhân thập phân thông thường với một thay đổi quan trọng: bất cứ khi nào một số sản phẩm ở cùng một vị trí thập phân, tổng của chúng được lấy theo modulo 10, do đó không có số nào chuyển sang vị trí tiếp theo. 

Nếu các chữ số của số chưa biết là 0 ​, a 1 ​,…, được tính từ phải sang trái thì chữ số ở vị trí k của hình vuông không mang số đó là 

c k ​ ≡ i=0 ∑ k ​ a i ​ a k−i ​ (mod10). 

Chúng ta được cấp các chữ số thập phân của N, có tối đa 25 chữ số và cần số nguyên thập phân dương nhỏ nhất có bình phương không mang chính xác là N. Nếu không tồn tại số nguyên đó, chúng ta sẽ in`-1`. 

Giới hạn 25 chữ số loại trừ việc liệt kê các gốc có thể. Nếu N có 25 chữ số thì gốc của nó có thể có nhiều nhất là 13 chữ số, do đó, tìm kiếm trực tiếp sẽ kiểm tra tới 9⋅10 12 ứng viên dương tính. Ngay cả một tấm séc rất rẻ cho mỗi ứng viên cũng sẽ vượt quá giới hạn một giây. 

Hạn chế về cấu trúc đầu tiên là chiều dài. Một số có m chữ số dương có một bình phương không có giá trị có vị trí cao nhất có thể khác 0 là 2m−2, vì chữ số hàng đầu được nhân với chính nó và không thể trở thành số 0 modulo 10 trừ khi chữ số hàng đầu bằng 0, điều này bị cấm. Như vậy kết quả có đúng 2m−1 chữ số. Một N có độ dài chẵn không bao giờ có thể có căn bậc hai không mang dương. 

Ví dụ,`15`có hai chữ số, vì vậy câu trả lời của nó là`-1`. Việc triển khai bất cẩn chỉ tìm kiếm các gốc ứng viên trong phạm vi giới hạn số nào đó có thể lãng phí thời gian tìm kiếm khi câu trả lời là không thể về mặt cấu trúc. 

Chữ số có nghĩa nhỏ nhất cũng phải là bình phương modulo 10. Các số dư có thể là 0,1,4,5,6,9. Như vậy`2`ngay lập tức là không thể. Việc tìm kiếm chỉ cần chọn một chữ số tùy ý cho vị trí ít quan trọng nhất sẽ khám phá nhiều nhánh có thể bị từ chối ngay lập tức. 

Cũng có thể có một số gốc hợp lệ, vì vậy việc tìm bất kỳ gốc nào là không đủ. Vì`121`, cả hai`11`Và`99`là căn bậc hai không có giá trị, bởi vì bình phương của`99`có các chữ số 1,2,1 sau khi số mang bị loại bỏ. Câu trả lời bắt buộc là`11`. Tìm kiếm các chữ số theo thứ tự tăng dần cho phép chúng ta có được nghiệm nhỏ nhất mà không cần phải sắp xếp tất cả các nghiệm sau đó. 

Cuối cùng, một gốc có thể chứa các chữ số 0 bên trong. Ví dụ: số có 13 chữ số`1000000000000`có hình vuông không cần mang theo`1000000000000000000000000`. Việc coi số 0 có nghĩa là số có ít chữ số hơn sẽ từ chối trường hợp hợp lệ này một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi căn dương có thể có với số chữ số cần thiết, tính bình phương không mang số của nó và so sánh nó với N. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra rõ ràng. Với N có 25 chữ số, nghiệm có 13 chữ số, cho 9⋅10 12 nghiệm có thể có. Việc tính toán một hình vuông không mang số thực hiện các phép toán chữ số O(13 2 ) nếu được thực hiện trực tiếp, vì vậy cách tiếp cận vũ phu là vô vọng. 

Quan sát hữu ích là các chữ số kết quả được xác định từ phải sang trái. Giả sử chúng ta đã biết a 0 ​ ,…,a k−1 ​ và muốn xác định a k ​. Hệ số tại vị trí k là 

c k ​ ≡a 0 ​ a k ​ +a 1 ​ a k−1 ​ +⋯+a k−1 ​ a 1 ​ +a k ​ a 0 ​ (mod10). 

Tất cả các số hạng ngoại trừ hai số có chứa k ​ đều đã được biết. Do đó 

c k ​ ≡S k ​ +2a 0 ​ a k ​ (mod10), 

trong đó S k ​ chỉ phụ thuộc vào các chữ số đã chọn trước đó. 

Cách lập chỉ mục thuận tiện hơn là tạo ra các chữ số từ phía ít quan trọng nhất. Tại vị trí k, mọi chữ số ứng viên từ 0 đến 9 có thể được kiểm tra dựa trên tiền tố đã biết của hình vuông. Vì chữ số chưa biết xuất hiện trong biểu thức tuyến tính có hệ số 2 modulo 10 nên có nhiều nhất hai giá trị có thể hoạt động. Chữ số đầu tiên có phương trình đặc biệt a 0 2 ​ ≡c 0 ​ (mod10), phương trình này cũng có nhiều nhất hai nghiệm cho mọi chữ số mục tiêu có thể có. 

Điều này thay đổi việc tìm kiếm từ khoảng 10 13 khả năng thành nhiều nhất là 2 13 = 8192 nhánh. Khi đã biết tất cả các chữ số gốc, chúng tôi xác minh các vị trí cao còn lại của hình vuông. Đây chính là nguyên tắc tiền tố giúp cho việc xây dựng chữ số đệ quy có hiệu quả ở đây: k+1 chữ số đầu tiên của hình vuông không mang chỉ phụ thuộc vào k+1 chữ số đầu tiên của gốc. 

Bởi vì các ứng cử viên được tạo theo thứ tự chữ số tăng dần ở mọi vị trí, nên nghiệm hoàn chỉnh đầu tiên được tìm thấy là nghiệm nhỏ nhất. Ngoài ra, chúng ta có thể giữ mức tối thiểu trong số tất cả các nghiệm hợp lệ, điều này làm cho đối số đúng đắn không phụ thuộc vào thứ tự truyền tải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10 n/2 n 2 ) | O(n) | Quá chậm | 
| Chữ số DFS | O(2 n/2 n 2 ) | O(n) | Đã chấp nhận | 

Ở đây n<25 là số chữ số của N. 

## Hướng dẫn thuật toán 

1. Đọc N dưới dạng một chuỗi và gọi n là số chữ số của chuỗi đó. Nếu n chẵn thì in ngay`-1`. Một bình phương không mang số dương có m chữ số luôn có chính xác 2m−1 chữ số, do đó mục tiêu có độ dài chẵn không thể xảy ra. 
2. Đặt m=(n+1)/2, số chữ số duy nhất có thể có của gốc. Lưu trữ các chữ số của N từ ít quan trọng nhất đến quan trọng nhất, vì việc xây dựng đệ quy sẽ tiến hành theo hướng đó. 
3. Bắt đầu tìm kiếm theo chiều sâu mà không chọn chữ số gốc nào. Tại vị trí k=0, thử từng chữ số x từ 0 đến 9 và giữ những chữ số thỏa mãn 

x 2 mod10=N 0 ​ . 

Đây là vị trí duy nhất mà chữ số chưa biết chỉ xuất hiện một lần trong phép chập. 
4. Với mỗi vị trí k tiếp theo, hãy thử từng chữ số x từ 0 đến 9. Tạm thời đặt a k ​ =x và tính toán 

( i=0 ∑ k ​ a i ​ a k−i ​ )mod10. 

Chỉ tiếp tục đệ quy nếu số này bằng chữ số đích N k ​. Chữ số kết quả mới được kiểm tra không thể phụ thuộc vào bất kỳ chữ số gốc nào ngoài vị trí k, vì vậy việc từ chối một nhánh ở đây không bao giờ có thể loại bỏ một giải pháp hợp lệ. 
5. Khi vị trí m−1 được chọn, yêu cầu chữ số phải khác 0. Nếu không thì gốc giả định sẽ có ít hơn m chữ số, mâu thuẫn với cách tính độ dài ở bước 1. 
6. Khi tất cả m chữ số gốc đã cố định, hãy tính mọi chữ số bình phương còn lại từ các vị trí m đến 2m−2 và so sánh nó với N. Những vị trí này là những vị trí đầu tiên mà gốc không chứa chữ số mới chưa biết, vì vậy chúng không thể được kiểm tra trong quá trình xây dựng nửa dưới. 
7. Chuyển đổi mọi mảng chữ số hợp lệ đầy đủ thành một số nguyên và giữ lại số nhỏ nhất. Nếu không có nhánh nào đạt đến một hình vuông hoàn chỉnh hợp lệ, hãy in`-1`. 

### Tại sao nó hoạt động 

Ở độ sâu đệ quy k, mọi chữ số gốc dưới k đã được sửa. Chữ số bình phương không mang số ở vị trí k chỉ phụ thuộc vào các chữ số gốc a 0 ​,…,a k ​, vì vậy việc kiểm tra chữ số đó sẽ xác định chính xác liệu nghiệm một phần hiện tại có còn dẫn đến nghiệm hay không. Mỗi gốc hợp lệ sẽ theo sau một nhánh vượt qua tất cả các bước kiểm tra này, vì vậy nó không thể bị cắt bớt. Ngược lại, một nhánh được chấp nhận thông qua tất cả m chữ số gốc sẽ được kiểm tra đầy đủ theo từng chữ số của N, do đó mọi gốc được báo cáo đều hợp lệ thực sự. Lấy giá trị nhỏ nhất trong số tất cả các nghiệm hợp lệ sẽ cho chính xác nghiệm dương nhỏ nhất cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(s):
    n = len(s)

    # A carryless square of an m-digit positive number
    # has exactly 2*m-1 digits.
    if n % 2 == 0:
        return "-1"

    m = (n + 1) // 2

    # Target digits, least significant first.
    target = [ord(ch) - ord('0') for ch in reversed(s)]

    # Root digits, least significant first.
    a = [0] * m
    best = None

    def dfs(k):
        nonlocal best

        if k == m:
            # The lower m digits were already checked while constructing
            # the root. Check the remaining digits of the square.
            for pos in range(m, n):
                total = 0
                lo = pos - (m - 1)
                hi = m - 1

                for i in range(max(0, lo), hi + 1):
                    j = pos - i
                    if 0 <= j < m:
                        total += a[i] * a[j]

                if total % 10 != target[pos]:
                    return

            value = 0
            for d in reversed(a):
                value = value * 10 + d

            if best is None or value < best:
                best = value
            return

        start = 0
        end = 10

        # The most significant digit must be nonzero.
        if k == m - 1:
            start = 1

        for x in range(start, end):
            a[k] = x

            total = 0
            for i in range(k + 1):
                total += a[i] * a[k - i]

            if total % 10 == target[k]:
                dfs(k + 1)

    dfs(0)

    return "-1" if best is None else str(best)

def main():
    s = input().strip()
    print(solve_case(s))

if __name__ == "__main__":
    main()
```Đầu vào được giữ dưới dạng chuỗi vì mục tiêu có thể chứa 25 chữ số, mặc dù bản thân Python có thể xử lý số nguyên đó một cách dễ dàng. Quan trọng hơn, việc lưu trữ các chữ số một cách rõ ràng làm cho phép tích chập không mang theo trở nên tự nhiên. 

Mảng`target`bị đảo ngược vậy`target[0]`là chữ số hàng đơn vị Mảng gốc`a`sử dụng cùng một hướng, điều này làm cho hệ số tại vị trí`k`đơn giản là tổng của`a[i] * a[k-i]`. 

Trong lúc`dfs(k)`, chỉ có hệ số tại vị trí`k`cần phải được kiểm tra. Tất cả các điều khoản của nó liên quan đến chỉ số nhiều nhất`k`, vì vậy không có lựa chọn nào trong tương lai có thể thay đổi hệ số này. Đây là điều kiện cắt tỉa quan trọng của thuật toán. 

Kiểm tra giới hạn trên sau khi tất cả các chữ số gốc được chọn bắt đầu tại vị trí`m`. Vị trí bên dưới`m`đã được kiểm tra trong quá trình đệ quy. Đối với căn bậc 13 chữ số, hình vuông có vị trí`0`bởi vì`24`, do đó vòng lặp kiểm tra chính xác vị trí`13`bởi vì`24`. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn khi chuyển đổi gốc thành số nguyên. Căn lớn nhất chỉ có 13 chữ số. 

Giới hạn khác 0 chỉ được áp dụng khi`k == m - 1`. Số không hoàn toàn hợp lệ ở tất cả các vị trí thấp hơn. Áp dụng hạn chế trước đó sẽ loại bỏ các gốc không chính xác như`1000000000000`. 

## Ví dụ đã hoạt động 

Các giá trị mẫu của câu lệnh ban đầu được khôi phục từ bản PDF vấn đề chính thức:`6 -> 4`,`149 -> 17`,`123476544 -> 11112`, Và`15 -> -1`. 

### Mẫu 1 

cho`N = 6`, mục tiêu có một chữ số, do đó gốc cũng phải có một chữ số. Việc tìm kiếm kiểm tra các chữ số đơn vị có thể. 

| Vị trí k | chữ số ứng cử viên | Số bình phương được tính | Chữ số mục tiêu | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 6 | từ chối | 
| 0 | 1 | 1 | 6 | từ chối | 
| 0 | 2 | 4 | 6 | từ chối | 
| 0 | 3 | 9 | 6 | từ chối | 
| 0 | 4 | 6 | 6 | chấp nhận | 
| hoàn thành | 4 | 6 | 6 | hợp lệ | 

Gốc hợp lệ nhỏ nhất là`4`. Điều này thể hiện trường hợp cơ bản trong đó phương trình duy nhất là a 0 2 ​ mod10=N 0 ​. 

### Mẫu 2 

cho`N = 149`, gốc phải có hai chữ số. Các chữ số của nó là 0 ​, a 1 ​, trong khi các chữ số mục tiêu là 9,4,1. 

| Vị trí k | Đã chọn ak ​ | Chữ số được tính | Chữ số mục tiêu | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 3 2 mod10=9 | 9 | chấp nhận | 
| 1 | 1 | 3⋅1+1⋅3=6 | 4 | từ chối | 
| 1 | 2 | 3⋅2+2⋅3=12mod10=2 | 4 | từ chối | 
| 1 | 3 | 18mod10=8 | 4 | từ chối | 
| 1 | 4 | 24mod10=4 | 4 | chấp nhận | 
| hoàn thành | a 1 ​ a 0 ​ =43 | chữ số trên 4 2 =6 | 1 | từ chối | 

Các nhánh khác được khám phá, cuối cùng đạt 0 ​ =7, a 1 ​ =1. 

| Vị trí k | Đã chọn ak ​ | Chữ số được tính | Chữ số mục tiêu | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | 49mod10=9 | 9 | chấp nhận | 
| 1 | 1 | 7⋅1+1⋅7=14mod10=4 | 4 | chấp nhận | 
| hoàn thành | a=17 | chữ số trên 1 2 =1 | 1 | hợp lệ | 

Như vậy câu trả lời là`17`. Dấu vết cho thấy tại sao các chữ số thấp hơn có thể được cố định một cách độc lập trước khi xem xét các chữ số cao hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2 n/2 n 2 ) | Tối đa 2 n/2 nhánh đệ quy, với chữ số O(n 2 ) hoạt động trên mỗi nhánh hoàn chỉnh trong quá trình triển khai đơn giản | 
| Không gian | O(n) | Mảng chữ số gốc và độ sâu đệ quy đều là O(n) | 

Với n=25 tối đa, tìm kiếm có nhiều nhất 2 13 = 8192 nhánh. Mỗi nhánh chỉ thực hiện vài trăm phép tính số nguyên nhỏ, do đó điều này dễ dàng nằm trong giới hạn dự kiến. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm 

Dây nịt sau đây kiểm tra bốn mẫu chính thức và một số trường hợp nhằm vào các cạm bẫy về cấu trúc.```python
import sys
import io

def solve_case(s):
    n = len(s)

    if n % 2 == 0:
        return "-1"

    m = (n + 1) // 2
    target = [ord(ch) - ord('0') for ch in reversed(s)]
    a = [0] * m
    best = None

    def dfs(k):
        nonlocal best

        if k == m:
            for pos in range(m, n):
                total = 0
                for i in range(m):
                    j = pos - i
                    if 0 <= j < m:
                        total += a[i] * a[j]

                if total % 10 != target[pos]:
                    return

            value = 0
            for d in reversed(a):
                value = value * 10 + d

            if best is None or value < best:
                best = value
            return

        start = 1 if k == m - 1 else 0

        for x in range(start, 10):
            a[k] = x

            total = 0
            for i in range(k + 1):
                total += a[i] * a[k - i]

            if total % 10 == target[k]:
                dfs(k + 1)

    dfs(0)

    return "-1" if best is None else str(best)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        s = sys.stdin.readline().strip()
        return solve_case(s)
    finally:
        sys.stdin = old_stdin

# Official samples
assert run("6\n") == "4", "sample 1"
assert run("149\n") == "17", "sample 2"
assert run("123476544\n") == "11112", "sample 3"
assert run("15\n") == "-1", "sample 4"

# Minimum-size valid input
assert run("1\n") == "1", "single digit, smallest root"

# A digit that cannot be a square modulo 10
assert run("2\n") == "-1", "impossible units digit"

# Multiple roots, answer must be the smallest one
assert run("121\n") == "11", "smallest among multiple roots"

# Maximum-size target, with a 13-digit root
assert run("1000000000000000000000000\n") == "1000000000000", \
    "25-digit target and leading-zero boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Trường hợp dương tính kích thước tối thiểu | 
|`2`|`-1`| Chữ số đơn vị không thể có | 
|`121`|`11`| Nhiều gốc hợp lệ và lựa chọn tối thiểu | 
|`1000000000000000000000000`|`1000000000000`| Độ dài đầu vào tối đa và các chữ số 0 bên trong | 

## Vỏ cạnh 

Một số chữ số mục tiêu chẵn là không thể. Vì`15`, độ dài là 2, nhưng một nghiệm có m chữ số dương luôn tạo ra 2m−1 chữ số, là số lẻ. Thuật toán trả về`-1`trước khi bắt đầu DFS, tránh việc tìm kiếm không cần thiết. 

Mục tiêu có chữ số cuối cùng không phải là thặng dư bậc hai modulo 10 cũng không thể thực hiện được. Vì`2`, phương trình đầu tiên sẽ yêu cầu 0 2 ​ ≡2(mod10). Không có chữ số thập phân nào có phần dư bình phương như vậy, vì vậy DFS không có nhánh còn tồn tại và trả về`-1`. 

Nhiều gốc không được gây ra một câu trả lời tùy ý được trả về. Vì`121`, rễ bao gồm`11`Và`99`. DFS khám phá các chữ số có thể có và xác nhận hình vuông hoàn chỉnh, trong khi`best`lưu trữ gốc số tối thiểu. Câu trả lời kết quả là`11`. 

Các số 0 đứng đầu không được coi là một phần của gốc ngắn hơn. Vì`1000000000000000000000000`, đích có 25 chữ số nên gốc phải có 13 chữ số. Gốc`1000000000000`có độ dài chính xác như vậy và số hạng tích chập khác 0 duy nhất của nó là bình phương chữ số đầu của nó, tạo ra đích có 25 chữ số. Việc triển khai cho phép số 0 ở mọi vị trí thấp hơn nhưng yêu cầu chữ số gốc cuối cùng, đại diện cho chữ số có ý nghĩa nhất, phải khác không. 

Trường hợp một chữ số cũng thực hiện ranh giới của đệ quy. Vì`6`, gốc có một chữ số nên không có hệ số nào cao hơn để xác minh. Chỉ riêng phương trình đơn vị đã cho các nghiệm hợp lệ`4`Và`6`, và tối thiểu là`4`, phù hợp với câu trả lời được yêu cầu.
