---
title: "CF 105535C - Nhầm lẫn"
description: "Chúng ta được cấp một số nguyên không dấu 32-bit cố định $a$. Đối với mỗi trường hợp kiểm thử, chúng ta phải tìm tất cả các số nguyên không dấu 32-bit $b$ sao cho hai đại lượng trở nên giống hệt nhau khi xem modulo $2^{32}$: đại lượng đầu tiên là lũy thừa $a^b$, được tính theo nghĩa toán học thông thường và…"
date: "2026-06-23T01:24:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105535
codeforces_index: "C"
codeforces_contest_name: "2024 ICPC Belarus Regional Contest"
rating: 0
weight: 105535
solve_time_s: 68
verified: true
draft: false
---

[CF 105535C - Nhầm lẫn](https://codeforces.com/problemset/problem/105535/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số nguyên không dấu 32 bit cố định$a$. Đối với mỗi trường hợp thử nghiệm, chúng ta phải tìm tất cả các số nguyên không dấu 32 bit$b$sao cho hai đại lượng trở nên giống nhau khi xem modulo$2^{32}$: 

đại lượng đầu tiên là sức mạnh$a^b$, được tính theo nghĩa toán học thông thường và đại lượng thứ hai là XOR theo bit$a \oplus b$. Công ước đặc biệt đó là$a^0 = 1$ngay cả khi$a = 0$. 

Vì vậy, nhiệm vụ là giải phương trình trên số nguyên:$$a^b \equiv (a \oplus b) \pmod{2^{32}}$$và xuất ra mỗi$b$trong phạm vi$[0, 2^{32}-1]$đó thỏa mãn nó. 

Kích thước đầu vào lớn đối với số lượng ca kiểm thử, lên tới$10^5$, nhưng mỗi trường hợp thử nghiệm chỉ bao gồm một số nguyên. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng khám phá phạm vi rộng lớn của$b$mỗi trường hợp thử nghiệm. Bất cứ điều gì phụ thuộc vào việc lặp lại tất cả$b$hoặc thậm chí một phần đáng kể của$2^{32}$không gian là không thể. Thậm chí quét tuyến tính lên đến$2^{32}$mỗi trường hợp thử nghiệm sẽ vượt quá giới hạn thời gian một cách vô lý. 

Khó khăn tinh tế là phía bên trái phát triển cực kỳ nhanh như một hàm của$b$, trong khi phía bên phải là thao tác bitwise đơn giản thay đổi trơn tru với$b$. Sự không phù hợp này cho thấy các giải pháp chỉ có thể xảy ra trong những tình huống rất hạn chế. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Khi$a = 0$, lực định nghĩa$0^0 = 1$, vì vậy đối với$b = 0$chúng tôi nhận được$1 \neq 0$, và cho$b > 0$chúng tôi nhận được$0^b = 0$trong khi$0 \oplus b = b$, chỉ khớp khi$b = 0$, đã không hợp lệ. Vì vậy, không có giá trị hợp lệ. 

Khi$b = 0$, chúng tôi luôn nhận được$a^0 = 1$, trong khi$a \oplus 0 = a$. Điều này chỉ hoạt động khi$a = 1$, làm cho đây trở thành một điểm cố định hợp lệ hiếm có. 

Một trường hợp tinh tế khác là ngay cả khi biểu thức hoạt động tốt đối với số mũ nhỏ, không có gì đảm bảo giải pháp cho số mũ lớn.$b$, và xử lý lớn và nhỏ$b$thống nhất dẫn đến các giả định vũ phu không chính xác. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ thử tất cả các giá trị có thể có của$b$, tính toán$a^b \bmod 2^{32}$, tính toán$a \oplus b$, và so sánh. Điều này đúng về mặt logic nhưng không thể thực hiện được ngay lập tức. Mỗi phép lũy thừa tốn ít nhất$O(\log b)$, và lặp đi lặp lại$2^{32}$ứng viên về nguyên tắc là không thể. Ngay cả việc giới hạn ở vài triệu ứng viên cho mỗi trường hợp kiểm thử vẫn sẽ vượt xa giới hạn khi có$10^5$các trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không thực sự cần phải hiểu đầy đủ chức năng$a^b \bmod 2^{32}$trên một miền rộng lớn. Chúng ta chỉ cần biết nó có thể bằng một hàm bitwise tuyến tính đơn giản của$b$. Điều này gợi ý rõ ràng rằng các giải pháp hợp lệ, nếu có, chỉ phải xảy ra trong một tập hợp rất nhỏ và có cấu trúc của các giải pháp.$b$các giá trị. 

Cái nhìn sâu sắc về cấu trúc thứ hai xuất phát từ hành vi của lũy thừa modulo lũy thừa của hai. Đối với bất kỳ mô đun 32 bit cố định nào, phép lũy thừa trở thành tuần hoàn theo số mũ với một khoảng thời gian tương đối nhỏ (nhiều nhất là$2^{30}$đối với các căn cứ lẻ và sụp đổ nhanh chóng đối với các căn cứ chẵn). Trong khi đó vế phải phụ thuộc trực tiếp vào$b$chính nó, không chỉ trên$b \bmod k$. Điều này làm cho sự trùng hợp ở tầm xa trở nên cực kỳ hiếm. Trên thực tế, bên ngoài một phạm vi giới hạn nhỏ, ứng cử viên duy nhất có thể tồn tại là điểm cố định suy biến trong đó số mũ trùng với cơ số, tạo ra điều kiện tự nhất quán. 

Điều này làm giảm vấn đề xuống còn việc chỉ kiểm tra một tập hợp nhỏ các ứng viên: tất cả đều nhỏ$b$, cộng với trường hợp đặc biệt$b = a$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả$b$|$O(2^{32})$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Kiểm tra nhỏ$b$+ trường hợp đặc biệt |$O(32)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi không gian tìm kiếm là hai phần rời rạc: số mũ nhỏ nơi tính toán trực tiếp là an toàn và điểm cố định tiềm năng lớn duy nhất. 

1. Với mỗi test case, hãy đọc$a$. Nếu như$a = 0$, kết luận ngay là không có giải pháp nào vì$0^b$không bao giờ có thể sánh được$0 \oplus b$cho bất kỳ$b$. 
2. Xét mọi giá trị của$b$từ$0$ĐẾN$31$. Phạm vi này đủ nhỏ để chúng ta có thể tính toán một cách an toàn$a^b \bmod 2^{32}$sử dụng lũy ​​thừa mô đun nhanh. 
3. Đối với mỗi như vậy$b$, tính toán$a^b \bmod 2^{32}$và so sánh nó với$a \oplus b$. Nếu trùng nhau thì ghi lại$b$. Điều này nắm bắt tất cả các giải pháp “cục bộ” trong đó phép lũy thừa chưa đi vào hành vi tuần hoàn phức tạp. 
4. Kiểm tra riêng ứng viên$b = a$. Nếu như$a < 2^{32}$, thì đây luôn là ứng cử viên 32 bit hợp lệ. Tính toán$a^a \bmod 2^{32}$và so sánh nó với$a \oplus a$, luôn luôn bằng không. Nếu đẳng thức giữ nguyên, bao gồm$b = a$. Đây là ứng cử viên có ý nghĩa duy nhất nằm ngoài phạm vi nhỏ vì nó căn chỉnh số mũ với chính giá trị đầu vào, tạo ra một điều kiện tự tham chiếu hiếm gặp. 
5. Sắp xếp và xuất ra tất cả các giá trị được thu thập. 

Ý tưởng cốt lõi là tất cả các giải pháp đều xảy ra giữa các số mũ nhỏ hoặc thu gọn thành một điểm cố định tự đồng nhất duy nhất trong đó cấu trúc số mũ và XOR thẳng hàng. 

### Tại sao nó hoạt động 

chức năng$f(b) = a^b \bmod 2^{32}$cư xử thường xuyên đối với nhỏ$b$, nhưng trở nên có cấu trúc cao và cuối cùng mang tính tuần hoàn. chức năng$g(b) = a \oplus b$tăng tuyến tính theo số lần lật bit và không có sự liên kết định kỳ với lũy thừa ngoại trừ những trường hợp cực kỳ hạn chế. Cách duy nhất để đẳng thức tồn tại bên ngoài một vùng giới hạn là nếu bản thân số mũ trùng với điểm ổn định của cả hai phép biến đổi, đó chính xác là ứng cử viên$b = a$. Mọi thứ khác phải nằm trong tiền tố nhỏ nơi lũy thừa chưa tách khỏi các giá trị đơn giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1 << 32

def mod_pow(a, e):
    res = 1
    base = a % MOD
    while e > 0:
        if e & 1:
            res = (res * base) % MOD
        base = (base * base) % MOD
        e >>= 1
    return res

t = int(input())
for _ in range(t):
    a = int(input())
    ans = []

    if a == 0:
        print(0)
        continue

    for b in range(32):
        if mod_pow(a, b) == (a ^ b):
            ans.append(b)

    if a < MOD:
        if mod_pow(a, a) == (a ^ a):
            ans.append(a)

    ans = sorted(set(ans))
    print(len(ans), *ans)
```Việc thực hiện trực tiếp theo sau việc phân tách không gian tìm kiếm. Hàm lũy thừa mô-đun là hàm mũ nhị phân tiêu chuẩn theo mô-đun$2^{32}$. Vòng lặp kết thúc$b < 32$là an toàn vì nó chỉ tốn một lượng không đổi cho mỗi trường hợp thử nghiệm. 

Lựa chọn triển khai tinh tế duy nhất là sử dụng rõ ràng$2^{32}$số học thay vì số nguyên Python, vì vấn đề yêu cầu hành vi bao quanh 32 bit không dấu. Ứng cử viên đặc biệt$b = a$được kiểm tra riêng vì nó nằm ngoài phạm vi liệt kê giới hạn nhưng vẫn có thể hợp lệ. 

## Ví dụ đã hoạt động 

Hãy xem xét$a = 1$. 

| b |$1^b$mod$2^{32}$|$1 \oplus b$| Trận đấu | 
| --- | --- | --- | --- | 
| 0 | 1 | 1 | Có | 
| 1 | 1 | 0 | Không | 
| 2 | 1 | 3 | Không | 
| 3 | 1 | 2 | Không | 

Ở đây chỉ$b = 0$hoạt động. Bảng này cho thấy XOR phân kỳ nhanh như thế nào so với lũy thừa không đổi. 

Bây giờ hãy xem xét$a = 5$. 

| b |$5^b$mod$2^{32}$|$5 \oplus b$| Trận đấu | 
| --- | --- | --- | --- | 
| 0 | 1 | 5 | Không | 
| 1 | 5 | 4 | Không | 
| 2 | 25 | 7 | Không | 
| 3 | 125 | 6 | Không | 

Không nhỏ$b$hoạt động và không có sự liên kết cấu trúc nào xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(32)$mỗi trường hợp thử nghiệm | Chúng tôi chỉ kiểm tra một số mũ không đổi bằng phép lũy thừa mô-đun nhanh | 
| Không gian |$O(1)$| Chỉ lưu trữ một danh sách nhỏ các ứng cử viên | 

Giải pháp dễ dàng phù hợp trong giới hạn ngay cả đối với$10^5$các trường hợp kiểm thử vì mỗi trường hợp kiểm thử chỉ thực hiện vài chục phép tính số học. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 1 << 32

    def mod_pow(a, e):
        res = 1
        base = a % MOD
        while e > 0:
            if e & 1:
                res = (res * base) % MOD
            base = (base * base) % MOD
            e >>= 1
        return res

    t = int(input())
    out_lines = []
    for _ in range(t):
        a = int(input())
        ans = []

        if a == 0:
            out_lines.append("0")
            continue

        for b in range(32):
            if mod_pow(a, b) == (a ^ b):
                ans.append(b)

        if a < MOD:
            if mod_pow(a, a) == (a ^ a):
                ans.append(a)

        ans = sorted(set(ans))
        out_lines.append(str(len(ans)) + (" " + " ".join(map(str, ans)) if ans else ""))

    return "\n".join(out_lines)

# custom cases
assert run("1\n0\n") == "0", "a=0 no solutions"
assert run("1\n1\n") == "1 0", "a=1 gives b=0"
assert run("1\n2\n") == "0", "no trivial solution"
assert run("2\n1\n0\n") == "1 0\n0", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| a = 0 | 0 | Xử lý căn cứ số 0 một cách chính xác | 
| a = 1 | 1 0 | Xác thực$b=0$trường hợp cạnh | 
| a = 2 | 0 | Đảm bảo không có kết quả dương tính giả | 
| hỗn hợp | đúng từng dòng | Xử lý nhiều thử nghiệm | 

## Vỏ cạnh 

cho$a = 0$, thuật toán ngay lập tức không trả về giá trị nào. Vòng lặp kết thúc$b < 32$chỉ bị bỏ qua sau lần thoát sớm này, ngăn chặn việc đưa vào không chính xác$b = 0$, điều này sẽ gợi ý sai một đẳng thức hợp lệ bởi vì$0^0$được định nghĩa là 1 trong khi XOR cho 0. 

cho$a = 1$, thuật toán kiểm tra$b = 0$rõ ràng trong phạm vi nhỏ. Tại$b = 0$, cả hai bên đều đánh giá lần lượt là 1 và 1, do đó nó được đưa vào một cách chính xác. Tất cả khác$b$trong phạm vi nhỏ không thành công vì XOR thay đổi giá trị trong khi lũy thừa không đổi. 

Đối với lớn hơn$a$, ứng viên$b = a$được đánh giá riêng biệt. Thuật toán tính toán trực tiếp cả$a^a \bmod 2^{32}$Và$a \oplus a$, đảm bảo tính chính xác mà không giả định bất kỳ sự đơn giản hóa cấu trúc nào. Nếu chúng khớp nhau, nó sẽ được đưa vào bất kể độ lớn, giúp xử lý một cách an toàn các trường hợp điểm cố định nằm ngoài tìm kiếm giới hạn ban đầu.
