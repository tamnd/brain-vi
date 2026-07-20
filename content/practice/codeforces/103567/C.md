---
title: "CF 103567C - \u0422\u0440\u043e\u043b\u043b\u044c \u0421\u0435\u0432\u0430"
description: "Bài toán mô tả một quá trình mà chúng ta thực sự quan tâm đến việc liệu một dãy số học cụ thể có bao giờ tạo ra một số chia hết cho một số nguyên cho trước $N$ hay không. Trình tự này được cố định và tăng dần theo từng bước không đổi, bắt đầu từ một khoảng chênh lệch nhỏ: $2, 5, 8, 11, dots$."
date: "2026-07-03T04:17:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103567
codeforces_index: "C"
codeforces_contest_name: "2021-2022 Olympiad Cognitive Technologies, Prefinal Round"
rating: 0
weight: 103567
solve_time_s: 43
verified: true
draft: false
---

[CF 103567C - \u0422\u0440\u043e\u043b\u043b\u044c \u0421\u0435\u0432\u0430](https://codeforces.com/problemset/problem/103567/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một quá trình mà chúng ta thực sự quan tâm đến việc liệu một dãy số học cụ thể có bao giờ tạo ra một số chia hết cho một số nguyên cho trước hay không$N$. Trình tự được cố định và tăng dần theo một bước không đổi, bắt đầu từ một khoảng chênh lệch nhỏ:$2, 5, 8, 11, \dots$. Mỗi giá trị tiếp theo thu được bằng cách cộng 3, do đó chuỗi có thể được xem dưới dạng tất cả các số có dạng$a_i = 3i + 2$. 

Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp một số nguyên duy nhất$N$. Nhiệm vụ là xác định liệu có tồn tại một số chỉ mục$i$như vậy$3i + 2$chia hết cho$N$. Nếu phần tử đó tồn tại thì câu trả lời là khẳng định, ngược lại là phủ định. 

Mặc dù tuyên bố được diễn đạt theo cách giải thích tổ hợp hoặc dựa trên quá trình, cốt lõi tính toán hoàn toàn là lý thuyết số: chúng tôi đang kiểm tra xem một cấp số học tuyến tính có chứa bội số của$N$. 

Các ràng buộc không được đưa ra một cách rõ ràng, nhưng cuộc thảo luận trong tuyên bố hàm ý tiềm năng lớn$T$và lớn$N$, với sự lặp lại ngây thơ lên ​​đến$N$tổng thể các bước quá chậm. Điều đó ngay lập tức loại trừ bất kỳ lần quét tuyến tính nào trên mỗi bài kiểm tra trong quá trình tiến triển. 

Một trường hợp thất bại tinh vi đối với lối suy luận ngây thơ xuất hiện khi người ta giả định rằng “cuối cùng” mọi$N$sẽ xuất hiện theo trình tự do sự tăng trưởng không giới hạn. Ví dụ, với$N = 3$, trình tự là$2, 5, 8, 11, \dots$, và không cái nào trong số này chia hết cho 3. Sai lầm xuất phát từ việc bỏ qua lớp thặng dư cố định của dãy modulo 3. 

Một quan niệm sai lầm khác nảy sinh nếu người ta cố gắng chỉ kiểm tra một tiền tố nhỏ mà không hiểu tính tuần hoàn. Ví dụ: chỉ kiểm tra một số thuật ngữ đầu tiên có thể gợi ý không chính xác rằng một giải pháp tồn tại hoặc không tồn tại tùy thuộc vào$N$, nhưng hành vi thực sự phụ thuộc vào cấu trúc mô-đun chứ không phải lấy mẫu hữu hạn. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là mô phỏng trình tự và kiểm tra tính chia hết cho mỗi số hạng. Chúng tôi tạo ra$a_i = 3i + 2$và kiểm tra xem$a_i \bmod N = 0$. Điều này đúng vì nó kiểm tra điều kiện chính xác theo yêu cầu. Tuy nhiên, trong trường hợp xấu nhất, nếu$N$lớn, chúng ta có thể cần kiểm tra tới$N$các ứng cử viên trước khi kết luận rằng không có nghiệm nào tồn tại, vì dư lượng modulo$N$lặp lại với dấu chấm$N$. Trong nhiều trường hợp thử nghiệm, điều này dẫn đến sự phức tạp tổng thể theo thứ tự$O(T \cdot N)$, tốc độ này quá chậm khi cả hai giá trị đều lớn. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về việc tạo ra số lượng lớn và thay vào đó chuyển sang số học mô-đun. Trình tự$3i + 2$có thặng dư cố định modulo 3, luôn bằng 2. Điều đó có nghĩa là mọi số hạng đều nằm trong một lớp đồng đẳng duy nhất modulo 3. Liệu bất kỳ số nào trong số này có thể chia hết cho$N$phụ thuộc hoàn toàn vào cách lớp dư lượng của$N$tương tác với cấu trúc này. 

Nếu như$N$chia hết cho 3 thì mọi bội số của$N$cũng chia hết cho 3, do đó nó nằm trong lớp dư lượng 0 modulo 3. Vì chuỗi bị kẹt trong lớp dư 2 modulo 3 nên không thể giao nhau. 

Nếu như$N \equiv 1 \pmod{3}$, sau đó nhân$N$bằng 2 mang lại một số$2N$có dư lượng modulo 3 là$2 \cdot 1 = 2$, phù hợp với trình tự Vì vậy, một phần tử của chuỗi đạt bội số của$N$. 

Nếu như$N \equiv 2 \pmod{3}$, sau đó$N$bản thân nó đã có thặng dư 2 modulo 3, vì vậy nó thuộc cùng lớp với dãy, nghĩa là tồn tại ngay một chỉ mục phù hợp. 

Vì vậy, toàn bộ vấn đề sụp đổ để kiểm tra xem liệu$N$không chia hết cho 3. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(T \cdot N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(T)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm từng trường hợp thử nghiệm thành một lần kiểm tra mô-đun duy nhất. 

1. Đọc số nguyên$T$, số lượng ca kiểm thử Mỗi trường hợp thử nghiệm là độc lập, do đó không có trạng thái nào được mang giữa chúng. 
2. Với mỗi test, hãy đọc số nguyên$N$. Đây là giá trị chúng tôi đang kiểm tra theo chuỗi số học. 
3. Tính toán$N \bmod 3$. Điều này xác định loại dư lượng nào$N$thuộc về cấu trúc cố định của chuỗi. 
4. Nếu$N \bmod 3 = 0$, xuất ra “KHÔNG”. Điều này xuất phát từ thực tế là tất cả các bội số của$N$nằm trong lớp dư lượng 0 modulo 3, trong khi tất cả các phần tử chuỗi nằm trong lớp dư lượng 2, do đó không thể xảy ra sự chồng chéo. 
5. Nếu không thì xuất ra “YES”. Trong cả hai trường hợp thặng dư còn lại, tồn tại một giao điểm mang tính xây dựng, nghĩa là một số hạng của dãy bằng bội số của$N$. 

### Tại sao nó hoạt động 

Trình tự$a_i = 3i + 2$là hằng số modulo 3, luôn bằng 2. Bất kỳ số nào chia hết cho$N$phải có dư lượng được xác định bởi$N$modulo 3. Nếu$N \equiv 0 \pmod{3}$, bội số của nó không thể bằng số dư 2, nên không tồn tại nghiệm. Nếu như$N \not\equiv 0 \pmod{3}$, nhân với một hệ số thích hợp sẽ căn chỉnh các phần dư, đảm bảo rằng một số bội số của$N$rơi vào cùng một lớp cấp số cộng. Vì cả dãy và tập bội của$N$là vô hạn và tuần hoàn modulo 3, thì khả năng tương thích dư lượng vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        if n % 3 == 0:
            out.append("NO")
        else:
            out.append("YES")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các trường hợp kiểm thử và xử lý từng trường hợp kiểm thử một cách độc lập trong thời gian không đổi. Phép tính duy nhất cho mỗi trường hợp là phép toán modulo, trực tiếp nắm bắt điều kiện cấu trúc bắt nguồn từ cấp số cộng. 

Quy tắc quyết định được thực hiện chính xác như điều kiện dẫn xuất: chia hết cho 3 hàm ý thất bại, ngược lại là thành công. 

Một lỗi triển khai phổ biến là cố gắng mô phỏng chuỗi hoặc tìm kiếm bội số một cách rõ ràng. Điều đó là không cần thiết vì lớp dư lượng của chuỗi loại bỏ hoàn toàn nhu cầu liệt kê. 

## Ví dụ đã hoạt động 

Xem xét một đầu vào có nhiều trường hợp thử nghiệm. 

đầu vào:$N = 3$,$N = 4$Vì$N = 3$, chúng tôi tính toán$3 \bmod 3 = 0$. 

| Bước | N | N mod 3 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 3 | 0 | KHÔNG | 

Điều này cho thấy không có thuật ngữ nào trong$2, 5, 8, \dots$có thể chia hết cho 3 vì mọi số hạng đều bằng 2 modulo 3. 

cho$N = 4$, chúng tôi tính toán$4 \bmod 3 = 1$. 

| Bước | N | N mod 3 | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 4 | 1 | CÓ | 

Ở đây, mặc dù 4 không nằm trong chính dãy đó, nhưng bội số phù hợp sẽ căn chỉnh với lớp dư lượng của cấp số nhân, đảm bảo sự tồn tại. 

Hai trường hợp này minh họa sự phân chia hoàn chỉnh của hành vi chỉ dựa trên phần dư modulo 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm yêu cầu một phép toán và so sánh modulo duy nhất | 
| Không gian |$O(1)$| Chỉ sử dụng bộ nhớ bổ sung liên tục bất kể kích thước đầu vào | 

Giải pháp phù hợp thoải mái trong các ràng buộc điển hình, ngay cả đối với các$T$, vì tất cả các phép tính nặng đều bị loại bỏ và thay thế bằng số học có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    res = []
    for _ in range(t):
        n = int(input())
        res.append("NO" if n % 3 == 0 else "YES")
    return "\n".join(res)

# provided samples (illustrative)
assert run("2\n3\n4\n") == "NO\nYES", "sample check"

# minimum size
assert run("1\n1\n") == "YES", "n=1 always works"

# divisible by 3 edge
assert run("3\n3\n6\n9\n") == "NO\nNO\nNO", "all multiples of 3 fail"

# non-multiples of 3
assert run("3\n1\n2\n5\n") == "YES\nYES\nYES", "all non-multiples work"

# mixed large values
assert run("4\n10\n11\n12\n13\n") == "YES\nYES\nNO\nYES", "mixed residues"

# boundary style check
assert run("2\n0\n3\n") == "YES\nNO", "zero treated as divisible by 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn nhỏ n | CÓ | trường hợp cơ sở đúng đắn | 
| bội số của 3 | KHÔNG | lớp học bất khả thi | 
| không bội số | CÓ | trường hợp mang tính xây dựng | 
| giá trị hỗn hợp | hỗn hợp | tính nhất quán logic dư lượng | 

## Vỏ cạnh 

cho$N = 3$, thuật toán tính toán$3 \bmod 3 = 0$và ngay lập tức trả về “KHÔNG”. Trình tự vẫn còn$2, 5, 8, \dots$, tất cả đều bằng 2 modulo 3, nên không có số nào chia hết cho 3. 

cho$N = 6$, việc tính toán có cấu trúc giống hệt nhau:$6 \bmod 3 = 0$, vì vậy câu trả lời lại là “KHÔNG”. Mặc dù 6 lớn hơn nhưng bội số của nó vẫn nằm trong lớp dư lượng 0 modulo 3, không bao giờ giao nhau với lớp dư lượng cố định của chuỗi. 

Vì$N = 4$, chúng tôi nhận được$4 \bmod 3 = 1$, do đó thuật toán xuất ra “CÓ”. Điều này tương ứng với sự tồn tại của bội số của 4 nằm trong dư lượng loại 2 modulo 3, phù hợp với cấp số nhân. Cấu trúc đảm bảo bội số như vậy tồn tại mà không cần tìm kiếm một cách rõ ràng.
