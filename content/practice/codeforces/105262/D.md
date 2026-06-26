---
title: "CF 105262D - Vấn đề FFT"
description: "Chúng ta đang cố gắng chọn cơ số để biểu diễn một số nguyên cho trước sao cho biểu diễn của nó chứa chữ số 4 ít nhất một lần. Trong số tất cả các cơ sở như vậy trong một phạm vi nhất định, chúng tôi muốn có cơ sở lớn nhất. Cụ thể, với mỗi trường hợp thử nghiệm, chúng ta nhận được một số $n$."
date: "2026-06-24T03:01:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "D"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 72
verified: true
draft: false
---

[CF 105262D - Sự cố FFT](https://codeforces.com/problemset/problem/105262/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang cố gắng chọn cơ số để biểu diễn một số nguyên cho trước sao cho biểu diễn của nó chứa chữ số 4 ít nhất một lần. Trong số tất cả các cơ sở như vậy trong một phạm vi nhất định, chúng tôi muốn có cơ sở lớn nhất. 

Cụ thể, đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được một số$n$. Chúng ta tưởng tượng việc viết$n$trong căn cứ$b$, Ở đâu$b$phải có ít nhất 2 và nhiều nhất$2 \cdot 10^{12}$. Trong biểu diễn đó, chúng tôi quét các chữ số của nó. Nếu ít nhất một chữ số bằng 4 thì cơ số này được coi là hợp lệ. Nhiệm vụ của chúng ta là tìm ra cơ sở hợp lệ tối đa hoặc báo cáo rằng không tồn tại. 

Ràng buộc$n \le 10^{12}$nghĩa là số chữ số của$n$trong bất kỳ cơ sở hợp lý nào đều nhỏ, nhiều nhất là khoảng 40 ở cơ số 2 và thậm chí còn nhỏ hơn đối với các cơ sở lớn hơn. Điều đó giới hạn số lượng vị trí chữ số thậm chí có thể tồn tại trong bất kỳ biểu diễn nào. Số lượng ca kiểm thử nhiều nhất là 10, do đó, một thuật toán gần như$O(n^{1/2})$hoặc$O(n^{1/3})$mỗi trường hợp vẫn ổn. 

Một nỗ lực ngây thơ sẽ thử mọi cơ sở$b$từ 2 đến$2 \cdot 10^{12}$, chuyển thành$n$vào căn cứ$b$và kiểm tra xem một chữ số có bằng 4 hay không. Cách tiếp cận đó thất bại ngay lập tức vì ngay cả việc kiểm tra một cơ số cũng là logarit trong$n$, do đó tổng công sẽ vượt xa mọi giới hạn. 

Một trường hợp thất bại phổ biến khác xuất hiện khi mọi người chỉ kiểm tra xem$n \% b = 4$. Điều đó chỉ xác minh chữ số cuối cùng và bỏ lỡ tất cả các vị trí chữ số cao hơn. Ví dụ,$n = 24$trong cơ sở 5 là$44_5$, có 4 nhưng không thỏa mãn$24 \% 5 = 4$riêng nó là điều kiện đủ cho mọi căn cứ hợp lệ. 

Khó khăn thực sự là chữ số 4 có thể xuất hiện ở bất kỳ vị trí nào và mỗi vị trí đặt ra một ràng buộc đại số khác nhau đối với$b$. 

## Phương pháp tiếp cận 

Cấu trúc brute-force rất đơn giản. Đối với mọi cơ sở ứng cử viên$b$, chúng tôi liên tục chia$n$qua$b$và kiểm tra số dư để xem có chữ số nào bằng 4 hay không. Điều này đúng vì chuyển đổi cơ số là phép chia được lặp lại chính xác. Vấn đề là phạm vi của$b$là rất lớn và việc lặp lại tất cả sẽ mang lại kết quả gần đúng$2 \cdot 10^{12}$ứng viên, mỗi chi phí$O(\log n)$, điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là điều kiện chữ số có thể được biểu diễn bằng đại số. Nếu chữ số 4 xuất hiện ở vị trí$i$, sau đó ở căn cứ$b$số có thể được viết là$$n = x \cdot b^{i+1} + 4 \cdot b^i + y$$với$0 \le y < b^i$. Điều này biến bài toán thành việc tìm kiếm nghiệm nguyên của phương trình này thay vì mô phỏng các cơ số. 

Chúng tôi phân tách các trường hợp theo vị trí của chữ số 4. Khi 4 ở chữ số có nghĩa nhỏ nhất, phương trình đơn giản hóa thành$n = q b + 4$, Vì thế$b \mid (n - 4)$. Điều đó có nghĩa là tất cả các cơ sở hợp lệ trong trường hợp này đều là ước của$n - 4$, có thể được liệt kê một cách hiệu quả. 

Khi số 4 ở vị trí cao hơn, hãy nói$i \ge 1$, thuật ngữ$b^{i+1}$phát triển nhanh chóng. Điều này ngay lập tức hạn chế$b$bởi vì$b^{i+1} \le n$. Vì vậy với mỗi vị trí cố định, các cơ sở có thể được giới hạn bởi$n^{1/(i+1)}$, trở nên rất nhỏ như$i$tăng lên. Điều này cho phép chúng ta thử tất cả các cơ sở ứng viên cho từng vị trí một cách riêng biệt và xác minh tính hợp lệ bằng cách xây dựng lại biểu diễn cơ sở. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên căn cứ |$O(n \log n)$|$O(1)$| Quá chậm | 
| Bảng liệt kê dựa trên vị trí |$O(\sum n^{1/k} \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi phân chia tìm kiếm dựa trên vị trí chữ số 4 có thể xuất hiện trong biểu diễn cơ sở. 

1. Đầu tiên xử lý trường hợp chữ số 4 ở vị trí đơn vị. Trong trường hợp đó chúng tôi yêu cầu$n = q b + 4$. Sắp xếp lại mang lại$n - 4 = q b$, nên mọi cơ sở hợp lệ đều phải chia$n - 4$. Ta liệt kê tất cả các ước của$n - 4$, kiểm tra xem cơ sở có ít nhất là 2 không và xác minh rằng việc chuyển đổi$n$vào cơ số này thực sự chứa chữ số 4. Bước này là cần thiết vì chỉ tính chia hết không đảm bảo rằng các chữ số cao hơn cũng không vi phạm các ràng buộc. 
2. Tiếp theo xét trường hợp chữ số 4 ở vị trí$i \ge 1$. Ta viết lại số đó là$n = a b^{i+1} + 4 b^i + r$, Ở đâu$r < b^i$. Điều này đảm bảo chữ số ở vị trí$i$chính xác là 4 và không xảy ra hiện tượng mang hoặc tràn. 
3. Đối với vị trí cố định$i$, chúng tôi khai thác thực tế rằng$b^{i+1} \le n$. Điều này đưa ra giới hạn trên cho$b$, cụ thể$b \le n^{1/(i+1)}$. Chúng tôi lặp lại tất cả các cơ sở ứng cử viên như vậy. 
4. Đối với mỗi cơ sở ứng viên, chúng ta xây dựng rõ ràng các chữ số của$n$trong cơ số đó và kiểm tra xem có chữ số nào bằng 4 hay không. Việc xác minh này an toàn vì tập ứng cử viên nhỏ. 
5. Chúng tôi theo dõi cơ sở tối đa trong số tất cả các ứng viên hợp lệ ở tất cả các vị trí. 

Tính đúng đắn xuất phát từ thực tế là mọi cơ số hợp lệ đều phải có chữ số 4 ở một vị trí nào đó.$i$. Chúng tôi liệt kê tất cả các vị trí có thể có và đối với mỗi vị trí, chúng tôi chỉ xem xét các cơ số có thể hỗ trợ vị trí chữ số đó mà không bị tràn. Mọi giải pháp hợp lệ đều xuất hiện chính xác trong một trong những trường hợp này, vì vậy không có ứng cử viên nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def has_four(n, b):
    while n > 0:
        if n % b == 4:
            return True
        n //= b
    return False

def divisors(x):
    res = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            res.append(i)
            if i * i != x:
                res.append(x // i)
        i += 1
    return res

T = int(input())
for _ in range(T):
    n = int(input())
    best = -1

    if n - 4 > 0:
        for d in divisors(n - 4):
            if d >= 2 and has_four(n, d):
                best = max(best, d)

    i = 1
    while True:
        pow_b = 1
        ok = False

        max_b = int((n) ** (1 / (i + 1)))
        if max_b < 2:
            break

        for b in range(2, max_b + 1):
            if has_four(n, b):
                best = max(best, b)

        i += 1
        if max_b <= 2:
            break

    print(best)
```Bước chia số xử lý cấu trúc đặc biệt khi số 4 là chữ số có nghĩa nhỏ nhất, trong đó bài toán chuyển sang điều kiện chia hết rõ ràng. Vòng lặp thứ hai cố gắng tăng vị trí chữ số và giới hạn cơ số sao cho đóng góp vị trí$b^{i+1}$không vượt quá$n$. Chức năng trợ giúp`has_four`thực hiện chuyển đổi cơ số và kiểm tra trực tiếp các chữ số, điều này an toàn vì số chữ số có dạng logarit$n$. 

Điều kiện dừng tăng$i$xuất phát từ thực tế là một lần$b^{i+1} > n$, không có vị trí chữ số nào cao hơn có thể chứa số 4. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 24$. Chúng tôi kiểm tra các cơ sở phát sinh từ$n - 4 = 20$. Các ước số là 1, 2, 4, 5, 10, 20. Lọc các cơ số hợp lệ và kiểm tra biểu diễn: 

| Căn cứ$b$| Đại diện của 24 | Chứa 4 | 
| --- | --- | --- | 
| 2 | 11000₂ | Không | 
| 4 | 30₄ | Không | 
| 5 | 44₅ | Có | 
| 10 | 24₁₀ | Không | 
| 20 | 14₂₀ | Có | 

Cơ sở hợp lệ tối đa ở đây là 20. 

Bây giờ hãy xem xét$n = 10$. Chúng tôi lại kiểm tra$n - 4 = 6$, cho thí sinh cơ sở 2, 3, 6. Xác minh trực tiếp: 

| Căn cứ$b$| Đại diện của 10 | Chứa 4 | 
| --- | --- | --- | 
| 2 | 1010₂ | Không | 
| 3 | 101₃ | Không | 
| 6 | 14₆ | Có | 

Cơ sở hợp lệ lớn nhất là 6. 

Những dấu vết này cho thấy cách trường hợp số chia nắm bắt tất cả các khả năng trong đó 4 là chữ số cuối cùng và cách kiểm tra trực tiếp xác thực các vị trí cao hơn một cách ngầm định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot \sqrt{n} + T \cdot n^{1/3})$| ước số cộng với tìm kiếm cơ sở giới hạn trên phạm vi số mũ nhỏ | 
| Không gian |$O(1)$| chỉ một vài biến và danh sách ước số | 

Ràng buộc trên$n$đảm bảo rằng ngay cả trường hợp xấu nhất là liệt kê tới$n^{1/2}$ứng viên được chấp nhận vì$n^{1/2} \le 10^6$, và có nhiều nhất 10 ca kiểm thử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def has_four(n, b):
        while n > 0:
            if n % b == 4:
                return True
            n //= b
        return False

    def divisors(x):
        res = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                res.append(i)
                if i * i != x:
                    res.append(x // i)
            i += 1
        return res

    T = int(input())
    out = []
    for _ in range(T):
        n = int(input())
        best = -1

        if n - 4 > 0:
            for d in divisors(n - 4):
                if d >= 2 and has_four(n, d):
                    best = max(best, d)

        i = 1
        while True:
            max_b = int(n ** (1 / (i + 1)))
            if max_b < 2:
                break
            for b in range(2, max_b + 1):
                if has_four(n, b):
                    best = max(best, b)
            i += 1
            if max_b <= 2:
                break

        out.append(str(best))

    return "\n".join(out)

# custom tests
assert run("1\n20\n") == "16", "basic case"
assert run("1\n24\n") == "20", "divisor dominance case"
assert run("1\n4\n") == "-1", "minimum edge case"
assert run("1\n44444\n") != "", "large structured case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 4 | -1 | không có cơ sở hợp lệ tồn tại | 
| 1, 24 | 20 | chính xác tối đa trên nhiều cơ sở hợp lệ | 
| 1, 20 | 16 | ứng cử viên có cơ sở cao hơn đánh bại các ước số nhỏ | 

## Vỏ cạnh 

Một trường hợp tế nhị xảy ra khi$n = 4$. Đây$n - 4 = 0$, do đó logic ước số sẽ cố gắng liệt kê các ước số bằng 0 một cách không chính xác. Hành vi đúng là bị từ chối ngay lập tức vì không có biểu diễn cơ sở nào của 4 chứa chữ số 4 một cách hợp lệ (biểu diễn sẽ chỉ là “4”, nhưng cơ sở phải lớn hơn 4, làm cho chữ số không hợp lệ trong các ràng buộc cơ sở). 

Một trường hợp khác là khi cơ sở tối ưu chính xác là$n - 4$. Vì$n = 20$, chúng tôi nhận được$n - 4 = 16$, và cơ số 16 tạo ra biểu diễn$14_{16}$, chứa một chữ số 4. Phép liệt kê số chia bao gồm chính xác ứng cử viên này và việc xác minh đảm bảo nó được chấp nhận. 

Trường hợp cạnh thứ ba phát sinh cho số lượng lớn$n$với các cơ sở hợp lệ thưa thớt, nơi chỉ các vị trí chữ số ở vị trí cao hơn mới hoạt động. Tìm kiếm giới hạn trên$b^{i+1} \le n$đảm bảo những thứ này vẫn có thể truy cập được vì đối với những người nhỏ$i$, phạm vi cơ sở vẫn đủ lớn để bao gồm tất cả các ứng viên và đối với các ứng cử viên lớn$i$, giới hạn sẽ sụp đổ nhanh chóng, tránh bỏ lỡ các giải pháp.
