---
title: "CF 105902E - Trò đùa nhị phân: Đếm các bit tổ hợp"
description: "Chúng ta được cho một số $n$, và với mọi số nguyên $i$ từ $0$ đến $n$, chúng ta xét toàn bộ hàng thứ $i$-của tam giác Pascal. Đối với mỗi mục trong hàng đó, chúng tôi lấy hệ số nhị thức $binom{i}{j}$, giảm modulo 2 và tính tổng tất cả các giá trị đó."
date: "2026-06-22T03:02:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105902
codeforces_index: "E"
codeforces_contest_name: "2025 Fujian Normal University Programming Contest"
rating: 0
weight: 105902
solve_time_s: 57
verified: true
draft: false
---

[CF 105902E - Trò đùa nhị phân: Đếm số bit tổ hợp](https://codeforces.com/problemset/problem/105902/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$n$và với mọi số nguyên$i$từ$0$ĐẾN$n$, chúng tôi xem xét toàn bộ$i$-hàng thứ của tam giác Pascal. Đối với mỗi mục trong hàng đó, chúng tôi lấy hệ số nhị thức$\binom{i}{j}$, giảm nó theo modulo 2 và tính tổng tất cả các giá trị đó. Cuối cùng, chúng ta tính tổng số lượng này trên tất cả các hàng từ$0$ĐẾN$n$. 

Vì vậy, nhiệm vụ là tính xem có bao nhiêu mục lẻ xuất hiện trong mỗi hàng Pascal lên đến$n$, cộng các số đó lại với nhau và xuất kết quả theo modulo$998244353$. 

Những hạn chế làm rõ rằng$n$có thể lớn như$10^{18}$, và có thể có tới$3 \cdot 10^5$các trường hợp thử nghiệm. Bất kỳ giải pháp nào cố gắng lặp lại tất cả các giá trị lên đến$n$là không thể, vì ngay cả một trường hợp thử nghiệm cũng có thể yêu cầu tới$10^{18}$hoạt động. Giải pháp mong muốn chỉ phải phụ thuộc vào biểu diễn nhị phân của$n$, có nhiều nhất là 60 bit. 

Một vấn đề tế nhị nảy sinh từ ý nghĩa của “$\binom{i}{j} \mod 2$". Chúng tôi không tính tổng các hệ số nhị thức mà chỉ tính chẵn lẻ của chúng. Điều này thay đổi hoàn toàn cấu trúc: các giá trị giảm xuống 0 hoặc 1 và vấn đề trở thành tổ hợp chứ không phải số học. Một cạm bẫy tiềm ẩn khác là cố gắng tính trực tiếp các hệ số nhị thức hoặc giai thừa, điều này không khả thi đối với các hệ số nhị thức lớn.$i$và cũng không cần thiết. 

## Phương pháp tiếp cận 

Một cách diễn giải trực tiếp sẽ tính toán cho mỗi$i$, tất cả$i+1$các hệ số nhị thức, rút ​​gọn chúng theo modulo 2 và đếm xem có bao nhiêu số lẻ. Cái này đã tốn rồi$O(i)$mỗi hàng và trên tất cả các hàng lên đến$n$nó trở thành$O(n^2)$, vượt xa mọi giới hạn khả thi ngay cả đối với những$n$. 

Sự đơn giản hóa chính xuất phát từ việc hiểu khi nào các hệ số nhị thức là số lẻ. Theo định lý Lucas cơ số 2, hệ số nhị thức$\binom{i}{j}$là số lẻ khi và chỉ khi mọi bit được đặt vào$j$cũng được thiết lập trong$i$. Nói cách khác, số mục lẻ trong hàng$i$bằng số lượng tập hợp con của các bit được đặt của$i$, đó là$2^{\mathrm{popcount}(i)}$. 

Điều này biến vấn đề thành tính toán$$\sum_{i=0}^{n} 2^{\mathrm{popcount}(i)}.$$Bây giờ cấu trúc đã rõ ràng hơn. Mỗi số đóng góp một giá trị chỉ tùy thuộc vào số lượng số xuất hiện trong biểu diễn nhị phân của nó. Điều này gợi ý làm việc trên các bit thay vì lặp lại các giá trị. 

Ý tưởng brute-force thất bại vì nó liệt kê tất cả các số lên đến$n$, nhưng quan sát thấy rằng hàm chỉ phụ thuộc vào số lượng bit cho phép chúng ta chia phạm vi theo bit được đặt cao nhất và giảm bớt vấn đề theo cách đệ quy. 

Chúng tôi sử dụng nhận dạng rằng tất cả các số trong một khối đầy đủ$2^k$các giá trị đóng góp độc lập trên mỗi bit, tạo ra dạng đóng cho các khối hoàn chỉnh và mối quan hệ đệ quy cho tiền tố còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Đệ quy bit |$O(\log n)$mỗi bài kiểm tra |$O(\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$S(x)$như số tiền yêu cầu từ$0$ĐẾN$x$, nghĩa$S(x) = \sum_{i=0}^{x} 2^{\mathrm{popcount}(i)}$. 

1. Nếu$x = 0$, trả về 1 vì số duy nhất là 0, có số lượng là 0, đóng góp$2^0 = 1$. 
2. Tìm lũy thừa cao nhất của hai không vượt quá$x$, nói$2^k$. Điều này phân vùng phạm vi$[0, x]$thành hai phần: số từ$0$ĐẾN$2^k - 1$, và các số từ$2^k$ĐẾN$x$. 
3. Đối với khối đầy đủ$[0, 2^k - 1]$, mọi số chính xác là một$k$-số bit. Mỗi bit độc lập đóng góp 0 hoặc 1 và một bit được đặt đóng góp hệ số 2 trong trọng số. Điều này làm cho tổng số tiền đóng góp bằng$(1 + 2)^k = 3^k$. 
4. Đối với các số trong$[2^k, x]$, mọi số đều có tập bit cao nhất, đóng góp hệ số cố định là 2. Việc loại bỏ bit đó sẽ để lại số nhỏ hơn trong$[0, x - 2^k]$, do đó sự đóng góp trở thành$2 \cdot S(x - 2^k)$. 
5. Kết hợp cả hai phần:$$S(x) = 3^k + 2 \cdot S(x - 2^k).$$6. Tính toán trước lũy thừa 3 và lũy thừa 2 lên tới 60 bit và đánh giá đệ quy cho mỗi trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Phép đệ quy hợp lệ vì mỗi số được phân tách duy nhất thành bit được đặt cao nhất và hậu tố còn lại. Sự đóng góp$2^{\mathrm{popcount}(i)}$phân tích thành phần đóng góp cố định từ bit cao nhất và phần đóng góp độc lập từ các bit thấp hơn. Toàn bộ phạm vi được chia rõ ràng thành các khối rời rạc được căn chỉnh theo lũy thừa hai, do đó không có giá trị nào được tính hai lần và không có giá trị nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

pow3 = [1] * 61
for i in range(1, 61):
    pow3[i] = pow3[i - 1] * 3 % MOD

def solve_one(n: int) -> int:
    if n == 0:
        return 1

    # find highest bit
    k = n.bit_length() - 1
    highest = 1 << k

    # full block [0, 2^k - 1]
    res = pow3[k]

    # remainder
    res += 2 * solve_one(n - highest)
    return res % MOD

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        print(solve_one(n) % MOD)

if __name__ == "__main__":
    main()
```chức năng`solve_one`tuân theo sự phân rã đệ quy trực tiếp. Việc tính toán trước`pow3[k]`mang lại sự đóng góp hoàn chỉnh$k$-bit không gian. Cuộc gọi đệ quy xử lý hậu tố còn lại sau khi loại bỏ bit cao nhất. 

Một lỗi phổ biến là quên rằng hậu tố vẫn đóng góp với cấu trúc giống nhau, chỉ được chia tỷ lệ theo hệ số 2 vì bit cao nhất luôn là 1 trong phân đoạn đó. 

## Ví dụ đã hoạt động 

Hãy xem xét$n = 3$. Chúng tôi tính toán$S(3)$. 

| Bước | n | k | đóng góp 3^k | phần đệ quy | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 1 | 3 | S(1) | 3 + 2·S(1) | 
| 2 | 1 | 0 | 1 | S(0) | 1 + 2·1 | 

Hiện nay$S(1) = 3$, Vì thế$S(3) = 3 + 2 \cdot 3 = 9$. 

Điều này phù hợp với liệt kê trực tiếp:$1 + 2 + 2 + 4 = 9$. 

Bây giờ hãy xem xét$n = 5$. 

| Bước | n | k | đóng góp 3^k | phần đệ quy | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 9 | S(1) | 9 + 2·S(1) | 
| 2 | 1 | 0 | 1 | S(0) | 1 + 2·1 | 

Vì thế$S(5) = 9 + 2 \cdot 3 = 15$. 

Điều này xác nhận rằng phép đệ quy phân chia chính xác phạm vi có lũy thừa bằng 2 và bảo toàn cấu trúc trọng số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$mỗi bài kiểm tra | Mỗi cuộc gọi sẽ loại bỏ bit được đặt cao nhất, giảm kích thước sự cố | 
| Không gian |$O(\log n)$| độ sâu đệ quy bằng số bit trong$n$| 

Độ dài nhị phân của$n$nhiều nhất là 60, do đó độ đệ quy cực kỳ nhỏ ngay cả với số lượng ca kiểm thử tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

pow3 = [1] * 61
for i in range(1, 61):
    pow3[i] = pow3[i - 1] * 3 % MOD

def solve_one(n: int) -> int:
    if n == 0:
        return 1
    k = n.bit_length() - 1
    highest = 1 << k
    return (pow3[k] + 2 * solve_one(n - highest)) % MOD

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    t = int(sys.stdin.readline())
    out = []
    for _ in range(t):
        n = int(sys.stdin.readline())
        out.append(str(solve_one(n) % MOD))
    return "\n".join(out)

# provided samples (from statement description)
assert run("1\n3\n") == "9"
assert run("1\n5\n") == "15"

# custom cases
assert run("1\n0\n") == "1", "minimum"
assert run("1\n1\n") == "3", "small edge"
assert run("1\n2\n") == "5", "binary boundary"
assert run("1\n7\n") == "27", "full 3-bit block"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 0 | 1 | tính đúng đắn của trường hợp cơ sở | 
| 1 | 3 | phạm vi nhị phân không tầm thường nhỏ nhất | 
| 2 | 5 | chuyển tiếp qua bit cao nhất | 
| 7 | 27 | công thức khối đầy đủ$3^k$sự đúng đắn | 

## Vỏ cạnh 

cho$n = 0$, việc đệ quy phải kết thúc ngay lập tức. Hàm trả về 1, khớp với thực tế là chỉ có số lượng trống mới đóng góp. 

Đối với các giá trị của biểu mẫu$2^k - 1$, đệ quy không bao giờ đi vào trường hợp hậu tố. Câu trả lời trở nên chính xác$3^k$và việc triển khai phải đảm bảo bit cao nhất được tính toán chính xác để không có lỗi nào thay đổi$k$. 

Đối với sức mạnh của hai như$n = 8$, phép tách tạo ra một khối đầy đủ cộng với hậu tố bằng 0. Đệ quy hậu tố phải trả về 1 chính xác; nếu không thì sự đóng góp của phân khúc cao nhất sẽ bị tính thiếu đi hệ số 2 ở các mức đệ quy sâu hơn.
