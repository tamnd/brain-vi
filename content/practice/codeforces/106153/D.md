---
title: "CF 106153D - \u742a\u9732\u8bfa\u7684\u65e0\u9650\u5faa\u73af\u5c0f\u6570"
description: "Nhiệm vụ xoay quanh việc quyết định xem một phân số đã cho tạo ra số thập phân hữu hạn hay số thập phân lặp lại vô hạn khi viết ở cơ số 10. Mỗi trường hợp kiểm thử đưa ra một mẫu số nguyên và chúng ta ngầm xem xét phân số $frac{1}{n}$."
date: "2026-06-20T22:10:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106153
codeforces_index: "D"
codeforces_contest_name: "HNNU Freshman Competition Round 2"
rating: 0
weight: 106153
solve_time_s: 47
verified: true
draft: false
---

[CF 106153D - \u742a\u9732\u8bfa\u7684\u65e0\u9650\u5faa\u73af\u5c0f\u6570](https://codeforces.com/problemset/problem/106153/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ xoay quanh việc quyết định xem một phân số đã cho tạo ra số thập phân hữu hạn hay số thập phân lặp lại vô hạn khi viết ở cơ số 10. Mỗi trường hợp kiểm thử đưa ra một mẫu số nguyên và chúng ta ngầm xem xét phân số đó$\frac{1}{n}$. Mục đích là để xác định liệu quá trình giãn nở thập phân của nó cuối cùng sẽ dừng lại hay tiếp tục mãi mãi với một chu kỳ lặp lại. 

Biểu diễn thập phân kết thúc chính xác khi mẫu số, sau khi rút gọn hoàn toàn với tử số, không chứa thừa số nguyên tố nào ngoài 2 và 5. Điều này xuất phát từ cấu trúc của cơ số 10, vì$10 = 2 \cdot 5$. Bất kỳ hệ số nào của 2 hoặc 5 đều có thể được hấp thụ thành lũy thừa của 10, dịch chuyển các chữ số trong phần thập phân mở rộng cho đến khi phân số trở thành số nguyên. 

Kích thước đầu vào cho phép nhiều trường hợp thử nghiệm và giá trị lớn của$n$, điều này ngay lập tức loại trừ mọi mô phỏng khai triển số thập phân hoặc phép chia dài theo mô đun. Ngay cả việc chia lặp đi lặp lại cho 10 hoặc xây dựng các chữ số cũng sẽ quá chậm nếu thực hiện trực tiếp với các số lớn, đặc biệt là khi$n$có thể đủ lớn để cấu trúc phân tích nhân tử của nó quan trọng hơn độ lớn của nó. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong trường hợp này, phân số đã là số nguyên nên rõ ràng nó kết thúc. Một trường hợp góc khác là khi$n$là lũy thừa của 2 hoặc 5, vì chúng luôn kết thúc bất kể kích thước. Mặt khác, ngay cả một thừa số nguyên tố bổ sung như 3 cũng phá vỡ hoàn toàn việc kết thúc, chẳng hạn$n = 6$sản xuất$0.1666...$, được lặp lại vô hạn mặc dù nó nhỏ và đơn giản. 

Một cách tiếp cận ngây thơ có thể thử kiểm tra tính chia hết của lũy thừa lớn bằng 10 hoặc kiểm tra một giới hạn nào đó xem liệu$10^k \equiv 0 \pmod{n}$. Điều này không thành công vì số mũ cần thiết có thể cực kỳ lớn và nếu không loại bỏ các yếu tố không liên quan trước tiên thì quy trình sẽ không khả thi. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng trực tiếp việc mở rộng thập phân của$\frac{1}{n}$sử dụng phép chia dài. Ở mỗi bước, chúng ta nhân số dư với 10 và lấy các chữ số, kiểm tra xem số dư có bằng 0 hay không. Nếu nó trở thành số 0 thì số thập phân sẽ chấm dứt. 

Cách tiếp cận này đúng vì phép chia dài sẽ xây dựng chính xác biểu diễn thập phân. Tuy nhiên, trường hợp xấu nhất xảy ra khi phân số lặp lại, trong trường hợp đó phần dư sẽ chuyển qua các giá trị modulo$n$. Vì có nhiều nhất$n$phần còn lại có thể có, mô phỏng này có thể mất$O(n)$các bước cho mỗi trường hợp thử nghiệm. Với đầu vào lớn và nhiều truy vấn, tốc độ này quá chậm. 

Quan sát quan trọng là chúng ta không cần phải mô phỏng quá trình này. Lý do duy nhất một số thập phân kết thúc là sau khi loại bỏ tất cả các thừa số của 2 và 5 khỏi mẫu số, không còn lại gì. Điều này là do lũy thừa của 10 chỉ đưa ra thừa số của 2 và 5, nên bất kỳ thừa số nguyên tố nào khác không bao giờ có thể bị loại bỏ khi nhân với 10. 

Vì vậy, thay vì mô phỏng phép chia, chúng tôi liên tục loại bỏ tất cả các thừa số của 2 và 5 khỏi$n$. Nếu số còn lại là 1 thì số thập phân chấm dứt; nếu không thì nó là vô hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng phép chia dài |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Loại bỏ yếu tố (2, 5) |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi liên tục chia số cho 2 và 5 trong khi nó chia hết cho chúng. Khi không còn các yếu tố như vậy tồn tại nữa, chúng tôi kiểm tra xem kết quả có phải là 1 hay không. 

1. Đọc số nguyên$n$cho trường hợp thử nghiệm hiện tại. Điều này đại diện cho mẫu số của phân số chúng ta đang phân tích. 
2. Trong khi$n$chia hết cho 2, chia nó cho 2. Mỗi phép chia loại bỏ một lũy thừa của 2 khỏi hệ số. Bước này an toàn vì lũy thừa của 2 chính xác là lũy thừa có thể được hấp thụ vào cơ số 10. 
3. Trong khi$n$chia hết cho 5, chia cho 5. Điều này loại bỏ tất cả các yếu tố tương thích với tỷ lệ cơ số 10 vì lý do tương tự như trên. 
4. Sau khi loại bỏ hoàn toàn các yếu tố này, hãy kiểm tra xem$n$bằng 1. Nếu không, thì vẫn còn một thừa số nguyên tố nào khác, nghĩa là nó không thể bị loại bỏ bằng cách nhân lũy thừa của 10. 
5. Xuất "KHÔNG" nếu$n = 1$, nếu không thì xuất ra "CÓ". 

### Tại sao nó hoạt động 

Bất kỳ số thập phân tận cùng nào đều có nghĩa là tồn tại một lũy thừa nào đó$10^k$như vậy$\frac{1}{n} \cdot 10^k$trở thành một số nguyên. Từ$10^k = 2^k \cdot 5^k$, điều này hàm ý tất cả các thừa số nguyên tố của$n$phải chứa trong 2 và 5. Loại bỏ tất cả các thừa số của 2 và 5 khỏi$n$để lại 1 chính xác khi điều kiện này được giữ, vì vậy việc kiểm tra là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    while n % 2 == 0:
        n //= 2
    while n % 5 == 0:
        n //= 5
    if n == 1:
        print("NO")
    else:
        print("YES")

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Cốt lõi của việc thực hiện là vòng lặp loại bỏ yếu tố lặp đi lặp lại. Thứ tự loại bỏ 2 và 5 không quan trọng vì phép nhân có tính chất giao hoán và cả hai vòng lặp đều giảm số một cách độc lập cho đến khi không còn chia hết. 

Kiểm tra cuối cùng để phân biệt giữa các mẫu số rõ ràng chỉ gồm 2 giây và 5 giây so với các mẫu số vẫn chứa các số nguyên tố khác. 

Một điểm tinh tế là quy ước đầu ra trong bài toán này bị đảo ngược so với câu lệnh thông thường: sau khi chuẩn hóa, giá trị còn lại lớn hơn 1 tương ứng với một số thập phân vô hạn, được gắn nhãn "CÓ" ở đây. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 20$. 

| Bước | n | Hành động | 
| --- | --- | --- | 
| bắt đầu | 20 | giá trị ban đầu | 
| chia cho 2 | 10 | loại bỏ một thừa số 2 | 
| chia cho 2 | 5 | loại bỏ yếu tố thứ hai của 2 | 
| chia cho 5 | 1 | loại bỏ hệ số 5 | 
| kiểm tra | 1 | chấm dứt | 

Vì giá trị cuối cùng là 1 nên đầu ra là "NO", nghĩa là nó không có thành phần lặp lại vô hạn. 

Bây giờ hãy xem xét$n = 6$. 

| Bước | n | Hành động | 
| --- | --- | --- | 
| bắt đầu | 6 | giá trị ban đầu | 
| chia cho 2 | 3 | loại bỏ yếu tố 2 | 
| dừng 2 vòng | 3 | không chia hết cho 2 | 
| dừng 5 vòng | 3 | không chia hết cho 5 | 
| kiểm tra | 3 | giá trị cuối cùng | 

Vì kết quả là 3 nên đầu ra là "CÓ", biểu thị số thập phân lặp lại vô hạn. 

Những dấu vết này cho thấy chỉ các số nguyên tố bên ngoài {2, 5} mới tồn tại được trong quá trình chuẩn hóa và xác định đầy đủ hành vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(t \log n)$| mỗi bài kiểm tra chia lũy thừa của 2 và 5 | 
| Không gian |$O(1)$| chỉ sử dụng số học số nguyên | 

Các ràng buộc cho phép điều này một cách dễ dàng vì mỗi phép chia nhanh chóng giảm số lượng và thậm chí các giá trị lớn cũng co lại nhanh chóng do loại bỏ yếu tố lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        while n % 2 == 0:
            n //= 2
        while n % 5 == 0:
            n //= 5
        out.append("NO" if n == 1 else "YES")
    return "\n".join(out) + "\n"

# provided samples (illustrative)
assert run("2\n20\n6\n") == "NO\nYES\n"

# all ones
assert run("3\n1\n2\n5\n") == "NO\nNO\nNO\n"

# pure powers of 2 and 5
assert run("2\n32\n125\n") == "NO\nNO\n"

# mixed with other primes
assert run("3\n3\n7\n14\n") == "YES\nYES\nYES\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 20, 6 | KHÔNG, CÓ | hành vi hỗn hợp cơ bản | 
| 1, 2, 5 | KHÔNG, KHÔNG, KHÔNG | trường hợp chấm dứt nhỏ nhất | 
| 32, 125 | KHÔNG, KHÔNG | sức mạnh thuần túy lớn 2/5 | 
| 3, 7, 14 | CÓ, CÓ, CÓ | số nguyên tố không phải 2/5 luôn lặp lại | 

## Vỏ cạnh 

cho$n = 1$, các vòng lặp không làm gì vì nó không chia hết cho 2 hoặc 5. Giá trị cuối cùng vẫn là 1, do đó thuật toán xuất ra "NO" một cách chính xác. 

Vì$n = 8$, phép chia lặp lại cho 2 làm giảm nó thành 4, 2 và cuối cùng là 1. Trạng thái cuối cùng là sạch, xác nhận việc kết thúc. 

Vì$n = 45$, chúng ta loại bỏ 5 hai lần để được 9 và 9 không thể giảm thêm nữa. Vì 9 chứa thừa số 3 nên thuật toán xác định chính xác nó là vô hạn. 

Đối với mỗi trường hợp, bất biến chính giữ nguyên: sau khi loại bỏ tất cả các thừa số của 2 và 5, số còn lại thể hiện chính xác sự cản trở việc viết mẫu số dưới dạng lũy ​​thừa của ước số 10.
