---
title: "CF 104303D - \"\u9006\"\u5929\u6c42\u548c"
description: "Với mỗi truy vấn, chúng ta được cấp một số nguyên tố $p$. Chúng ta xem xét tất cả các số nguyên từ $1$ đến $p-1$, và với mỗi số nguyên $a$ như vậy, chúng ta tính modulo nghịch đảo nhân của nó $p$. Điều đó có nghĩa là chúng ta tìm thấy một số $b$ trong khoảng $[1, p-1]$ sao cho $a cdot b tương đương 1 pmod p$."
date: "2026-07-01T20:09:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104303
codeforces_index: "D"
codeforces_contest_name: "2023 Xiangtan Unversity Freshman Conteset"
rating: 0
weight: 104303
solve_time_s: 48
verified: true
draft: false
---

[CF 104303D - \"\u9006\"\u5929\u6c42\u548c](https://codeforces.com/problemset/problem/104303/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi truy vấn, chúng tôi được cung cấp một số nguyên tố$p$. Chúng tôi xem xét tất cả các số nguyên từ$1$ĐẾN$p-1$và với mỗi số nguyên như vậy$a$, chúng tôi tính modulo nghịch đảo nhân của nó$p$. Điều đó có nghĩa là chúng ta tìm thấy một số$b$trong phạm vi$[1, p-1]$như vậy$a \cdot b \equiv 1 \pmod p$. Bởi vì$p$là số nguyên tố, mọi dư lượng khác 0 đều có đúng một nghịch đảo như vậy. 

Nhiệm vụ là tính tổng của tất cả các nghịch đảo này:$$\sum_{i=1}^{p-1} i^{-1} \bmod p$$cho mỗi truy vấn. 

Đầu vào bao gồm nhiều số nguyên tố độc lập và với mỗi số nguyên tố chúng ta xuất ra tổng này. 

Ràng buộc$T \le 9592$có nghĩa là chúng tôi không thể đủ khả năng cho bất kỳ vòng lặp cho mỗi truy vấn nào lên tới$p$, từ$p$có thể lớn. Việc tính toán trực tiếp các nghịch đảo mô-đun cho mỗi truy vấn sẽ yêu cầu$O(p)$cho mỗi trường hợp thử nghiệm, tổng cộng sẽ quá chậm. 

Một điểm tinh vi có thể bẫy lối suy luận ngây thơ là lối suy nghĩ nghịch đảo hành xử độc lập. Ví dụ, việc tính toán một vài nghịch đảo và cố gắng suy ra một mẫu sẽ nhanh chóng dẫn đến kết luận sai trừ khi sử dụng cấu trúc nhóm. 

Một trường hợp cạnh khác là$p = 2$. Trong trường hợp đó phạm vi$1$ĐẾN$p-1$chỉ chứa$1$, và nghịch đảo của nó là chính nó, nên tổng là$1$. Nhiều lập luận mô-đun chung ngầm cho rằng$p > 2$, vì vậy điều này cần được chú ý riêng. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ tính toán từng nghịch đảo mô-đun bằng cách sử dụng phép lũy thừa nhanh thông qua định lý Fermat$a^{-1} \equiv a^{p-2} \pmod p$, sau đó tính tổng chúng. Điều này hoạt động chính xác vì mọi thuật ngữ được tính toán độc lập và chính xác theo modulo$p$. Tuy nhiên, nó thực hiện$p-1$lũy thừa cho mỗi trường hợp thử nghiệm và mỗi chi phí lũy thừa$O(\log p)$, dẫn đến$O(p \log p)$mỗi truy vấn. Với gần mười nghìn truy vấn, điều này trở nên không khả thi ngay khi các số nguyên tố có kích thước vừa phải. 

Quan sát cấu trúc quan trọng là việc lập bản đồ$a \mapsto a^{-1} \bmod p$là một hoán vị của tập hợp$\{1, 2, \dots, p-1\}$. Đây là hệ quả trực tiếp của thực tế là mọi phần tử khác 0 trong trường hữu hạn đều có một nghịch đảo duy nhất và việc áp dụng nghịch đảo hai lần sẽ trả về phần tử ban đầu. 

Bởi vì nghịch đảo là song ánh trên cùng một tập hợp, nên tổng tất cả các nghịch đảo tương đương với tổng tất cả các số ban đầu theo một thứ tự khác. Điều đó có nghĩa là nhiều tập nghịch đảo giống hệt như nhiều tập hợp nghịch đảo$\{1, 2, \dots, p-1\}$. Do đó tổng nghịch đảo giống hệt với:$$1 + 2 + \dots + (p-1) = \frac{(p-1)p}{2}$$Vì chúng tôi xuất giá trị này theo modulo$p$, biểu thức được đơn giản hóa ngay lập tức vì nó chứa thừa số của$p$, làm cho nó chia hết cho$p$. Do đó kết quả là$0$modulo$p$cho mọi số nguyên tố lẻ. Ngoại lệ duy nhất là$p = 2$, trong đó công thức suy biến. 

Vì vậy, toàn bộ quá trình tính toán giảm xuống còn việc kiểm tra liên tục theo thời gian cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán nghịch đảo) |$O(p \log p)$|$O(1)$| Quá chậm | 
| Tối ưu (hiểu biết sâu sắc về hoán vị nhóm) |$O(1)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta hoàn toàn dựa vào thực tế là phép nghịch đảo hoán vị hệ thặng dư khác 0 modulo thành một số nguyên tố. 

1. Đọc từng số nguyên tố$p$từ đầu vào. Mỗi truy vấn là độc lập nên không cần xử lý trước. 
2. Nếu$p = 2$, xuất trực tiếp$1$. Điều này xuất phát từ thực tế là phần tử duy nhất là$1$, Và$1^{-1} = 1$. 
3. Nếu$p > 2$, đầu ra$0$. Điều này xảy ra vì tập hợp các nghịch đảo chỉ là sự sắp xếp lại của$1$ĐẾN$p-1$và tổng của hệ dư lượng đầy đủ modulo$p$loại trừ số 0 luôn sụp đổ thành$0 \bmod p$. 

### Tại sao nó hoạt động 

chức năng$f(a) = a^{-1} \bmod p$là một bijection hơn$\{1, \dots, p-1\}$. Phép song ánh bảo toàn nhiều tập hợp, do đó nó bảo toàn tổng modulo$p$. Vì thế:$$\sum_{a=1}^{p-1} a^{-1} \equiv \sum_{a=1}^{p-1} a \pmod p$$Vế phải bằng$\frac{p(p-1)}{2}$, chia hết cho$p$, do đó phù hợp với$0$modulo$p$. Bất biến này đúng với mọi số nguyên tố$p > 2$, đảm bảo tính đúng đắn của lời giải theo thời gian không đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        p = int(input())
        if p == 2:
            out.append("1")
        else:
            out.append("0")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo đặc tính dẫn xuất trực tiếp. Mỗi truy vấn được xử lý trong thời gian không đổi. Điều kiện phân nhánh duy nhất là trường hợp đặc biệt$p = 2$, phải được phân tách để tránh áp dụng sai đối số hủy mô-đun khi tập hợp chỉ chứa một phần tử. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$$p = 5$$Chúng tôi xem xét nghịch đảo modulo 5: 

| một | a^{-1} mod 5 | 
| --- | --- | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 2 | 
| 4 | 4 | 

Tổng nghịch đảo là$1 + 3 + 2 + 4 = 10$, đó là$0 \bmod 5$. 

Điều này xác nhận rằng ánh xạ nghịch đảo chỉ hoán vị các phần tử, tạo ra cùng một tập hợp. 

### Ví dụ 2 

đầu vào:$$p = 2$$| một | a^{-1} mod 2 | 
| --- | --- | 
| 1 | 1 | 

Tổng là$1$, đó là đầu ra trường hợp đặc biệt chính xác. 

Điều này chứng tỏ tại sao đối số hủy chung không áp dụng khi kích thước được đặt là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi truy vấn được trả lời bằng cách kiểm tra liên tục | 
| Không gian |$O(1)$| Chỉ các biến đơn giản và lưu trữ đầu ra | 

Lời giải dễ dàng nằm trong giới hạn vì thậm chí$T \approx 10^4$hoạt động là tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        p = int(input())
        out.append("1" if p == 2 else "0")
    return "\n".join(out)

# provided-like samples
assert run("1\n2\n") == "1", "p=2 case"
assert run("1\n5\n") == "0", "odd prime case"

# custom cases
assert run("3\n3\n5\n7\n") == "0\n0\n0", "all odd primes"
assert run("2\n2\n2\n") == "1\n1", "repeated minimal prime"
assert run("1\n11\n") == "0", "larger prime"
assert run("4\n2\n3\n2\n5\n") == "1\n0\n1\n0", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lặp lại 2 | tất cả 1 | xử lý số nguyên tố nhỏ nhất nhiều lần | 
| một số số nguyên tố lẻ | tất cả 0 | tính đúng đắn chung | 
| hỗn hợp 2 và số nguyên tố lẻ | 1 hoặc 0 đúng | logic phân nhánh | 

## Vỏ cạnh 

cho$p = 2$, tập hợp đảo ngược sẽ thu gọn thành một phần tử duy nhất. Thuật toán kiểm tra rõ ràng trường hợp này trước khi áp dụng quy tắc chung. 

Vì$p > 2$, mọi phần tử đều có một nghịch đảo riêng biệt trong cùng một phạm vi, do đó tổng chỉ là tổng hoán vị. Ví dụ với$p = 3$, chúng ta có nghịch đảo$1 \leftrightarrow 1$Và$2 \leftrightarrow 2$, tính tổng$3 \equiv 0 \pmod 3$. Thuật toán xuất ra chính xác$0$. 

Đối với các số nguyên tố lớn hơn như$p = 11$, mặc dù các nghịch đảo riêng lẻ trông không đều, thuộc tính hoán vị đảm bảo tổng vẫn cố định và thuật toán tránh tính toán trực tiếp bất kỳ nghịch đảo nào trong số đó trong khi vẫn trả về kết quả chính xác.
