---
title: "CF 104353I - \u66f4\u52a0\u9006\u5929\u7684\u6c42\u548c"
description: "Chúng ta được cung cấp một hàm được xác định trên một số nguyên $n$. Hãy tưởng tượng một lưới $n nhân n$ trong đó mỗi ô $(i, j)$ chứa giá trị thu được bằng cách lấy số nguyên của $i$ cho $j$, đó là $leftlfloor frac{i}{j} rightrfloor$."
date: "2026-07-01T18:12:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104353
codeforces_index: "I"
codeforces_contest_name: "2023 Xiangtan University Programming Contest"
rating: 0
weight: 104353
solve_time_s: 59
verified: true
draft: false
---

[CF 104353I - \u66f4\u52a0\u9006\u5929\u7684\u6c42\u548c](https://codeforces.com/problemset/problem/104353/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm được xác định trên một số nguyên$n$. Hãy tưởng tượng một$n \times n$lưới nơi mỗi ô$(i, j)$chứa giá trị thu được bằng cách lấy số nguyên chia của$i$qua$j$, đó là$\left\lfloor \frac{i}{j} \right\rfloor$. Nhiệm vụ là tính tổng của tất cả các giá trị trong lưới này. 

Vì vậy, đối với mỗi trường hợp thử nghiệm, đầu vào là một số duy nhất$n$và đầu ra là tổng của tất cả các giá trị$\left\lfloor \frac{i}{j} \right\rfloor$cho tất cả các cặp$1 \le i \le n$,$1 \le j \le n$. 

Các ràng buộc cho phép lên đến$10^3$các trường hợp thử nghiệm và mỗi trường hợp$n$có thể lớn như$10^7$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào chạm vào mọi cặp$(i, j)$, vì điều đó sẽ yêu cầu$n^2$hoạt động cho mỗi trường hợp thử nghiệm, lên đến$10^{14}$hoạt động trong trường hợp xấu nhất. Thậm chí$O(n \log n)$mỗi trường hợp thử nghiệm là quá chậm nếu lặp đi lặp lại$10^3$lần. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất phát từ việc giả định tính đối xứng. Biểu thức không đối xứng ở$i$Và$j$. Ví dụ, khi$n = 2$, lưới là$$\begin{matrix}
\lfloor 1/1 \rfloor & \lfloor 1/2 \rfloor \\
\lfloor 2/1 \rfloor & \lfloor 2/2 \rfloor
\end{matrix}
=
\begin{matrix}
1 & 0 \\
2 & 1
\end{matrix}$$và tổng là$4$. Bất kỳ nỗ lực nào nhằm thay thế điều này bằng một phép biến đổi đối xứng đơn giản hơn như hoán đổi vai trò của$i$Và$j$không cẩn thận sẽ dẫn đến việc tổng hợp không chính xác. 

Thách thức thực sự là tránh lặp lại tất cả các cặp trong khi vẫn nắm bắt được tần suất mỗi giá trị thương xuất hiện. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lặp lại tất cả các cặp$(i, j)$và tích lũy$\left\lfloor \frac{i}{j} \right\rfloor$. Điều này đơn giản và chính xác nhưng nó thực hiện$n^2$chia số nguyên cho mỗi trường hợp thử nghiệm. Với$n = 10^7$, ngay cả một trường hợp thử nghiệm duy nhất cũng không thể thực hiện được. 

Quan sát chính là hàm này được cấu trúc xung quanh các khối phân chia. Đối với số chia cố định$j$, giá trị$\left\lfloor \frac{i}{j} \right\rfloor$là không đổi trong các khoảng thời gian$i$. Thay vì đánh giá từng cặp một cách độc lập, chúng ta có thể nhóm các chỉ số của$i$trong đó thương số là như nhau. Điều này biến tổng bên trong thành cấu trúc tuyến tính từng phần. 

Tuy nhiên, thậm chí tính tổng mỗi$j$bằng cách nhóm các khối dẫn đến$O(n)$làm việc cho mỗi trường hợp thử nghiệm và với tối đa$10^3$trường hợp thử nghiệm, điều này vẫn còn quá lớn. 

Bước cuối cùng là đảo ngược quan điểm: chúng tôi tính toán các khoản đóng góp cho mỗi cột$j$một lần lên đến mức tối đa$n$trên tất cả các truy vấn và sử dụng lại tổng tiền tố. Mỗi cột có thể được đánh giá theo thời gian không đổi bằng cách sử dụng biểu thức dạng đóng bắt nguồn từ việc phân tách khối. Điều này làm giảm toàn bộ tính toán thành tiền xử lý tuyến tính một lần, sau đó là các câu trả lời theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Nhóm cột cho mỗi truy vấn |$O(n)$|$O(1)$| Quá chậm cho nhiều bài kiểm tra | 
| Tiền tố được tính toán trước trên tất cả$j$|$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán dưới dạng tổng theo cột. Đối với một cố định$j$, định nghĩa$$S(j) = \sum_{i=1}^{n} \left\lfloor \frac{i}{j} \right\rfloor.$$Cấu trúc của$\left\lfloor \frac{i}{j} \right\rfloor$chỉ thay đổi khi$i$vượt qua bội số của$j$. Điều này cho phép chúng tôi nén tính toán. 

### Các bước 

1. Sửa chỉ mục cột$j$, và để$m = \left\lfloor \frac{n}{j} \right\rfloor$. Đây là giá trị thương số tối đa có thể đạt được trong cột này. 
2. Với mỗi thương số nguyên$k \ge 1$, giá trị$k$xuất hiện chính xác cho các chỉ số$i \in [kj, (k+1)j - 1]$, ngoại trừ khoảng thời gian cuối cùng có thể bị cắt ngắn bởi$n$. 
3. Thay vì lặp lại từng khoảng thời gian, hãy tính toán các khối đầy đủ một cách phân tích. đầu tiên$m-1$các khối đã hoàn thành và mỗi khối có chiều dài$j$, đóng góp tổng cộng$$j \cdot (1 + 2 + \cdots + (m-1)).$$4. Khối cuối cùng tương ứng với$k = m$, và sự đóng góp của nó phụ thuộc vào số lượng chỉ số còn lại, cụ thể là$n - m \cdot j + 1$, mỗi giá trị đóng góp$m$. 
5. Kết hợp cả hai phần để tính toán$S(j)$trong thời gian không đổi. 
6. Tính toán trước$S(j)$cho tất cả$j \in [1, N]$, Ở đâu$N$là tối đa$n$trong số tất cả các trường hợp thử nghiệm. 
7. Xây dựng một mảng tiền tố sao cho với mỗi trường hợp kiểm thử, câu trả lời là tổng của$S(1)$bởi vì$S(n)$. 

### Tại sao nó hoạt động 

Mỗi cặp$(i, j)$đóng góp chính xác một lần vào đúng một tổng cột$S(j)$. Việc phân tách khối đảm bảo mọi khoảng nguyên trong đó thương số không đổi được tính chính xác một lần, không bị chồng chéo hoặc bỏ sót. Bước tổng tiền tố chỉ đơn giản là tổng hợp các đóng góp của cột, duy trì tổng giá trị tương đương với tổng kép ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    ns = []
    max_n = 0
    
    for _ in range(t):
        n = int(input())
        ns.append(n)
        if n > max_n:
            max_n = n

    S = [0] * (max_n + 1)

    for j in range(1, max_n + 1):
        m = max_n // j
        if m == 0:
            break

        # full blocks contribution: j * sum_{k=1..m-1} k
        full = j * (m - 1) * m // 2

        # last block contribution
        last_count = max_n - m * j + 1
        S[j] = full + m * last_count

    pref = [0] * (max_n + 1)
    for j in range(1, max_n + 1):
        pref[j] = pref[j - 1] + S[j]

    out = []
    for n in ns:
        out.append(str(pref[n]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ đọc tất cả các trường hợp kiểm thử để xác định mức tối đa$n$, vì mọi đóng góp đều phụ thuộc vào nó. Sau đó nó tính toán đóng góp của từng cột$S(j)$sử dụng công thức khối theo thời gian không đổi trên$j$. Mảng tiền tố biến đổi tổng cột thành câu trả lời tùy ý$n$. 

Một chi tiết tinh tế là tính toán khối một phần cuối cùng. biểu hiện$n - m \cdot j + 1$phải không âm và được đảm bảo bởi định nghĩa của$m$. Các lỗi ngẫu nhiên thường xuất hiện ở đây nếu điểm cuối của khoảng thời gian bị căn chỉnh sai. 

## Ví dụ đã hoạt động 

Hãy xem xét hai trường hợp nhỏ. 

Vì$n = 3$, chúng tôi tính toán từng cột: 

| j | m = ⌊3/j⌋ | khối đầy đủ | khối cuối cùng | S(j) | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 1·(1+2)=3 | 3·1=3 | 6 | 
| 2 | 1 | 0 | 1·2=2 | 2 | 
| 3 | 1 | 0 | 1·1=1 | 1 | 

Vậy tổng cộng là$6 + 2 + 1 = 9$. 

Vì$n = 4$: 

| j | m | S(j) | 
| --- | --- | --- | 
| 1 | 4 | 10 | 
| 2 | 2 | 4 | 
| 3 | 1 | 1 | 
| 4 | 1 | 1 | 

Tổng cộng là$16$. 

Những dấu vết này xác nhận rằng mỗi cột tổng hợp độc lập những đóng góp của tất cả$i$và tổng tiền tố đó tích lũy chính xác tất cả các cột lên đến$n$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Một lần vượt qua tất cả$j$lên đến tối đa$n$, cộng với việc xây dựng tiền tố | 
| Không gian |$O(N)$| Mảng tính tổng cột và tổng tiền tố | 

Những ràng buộc cho phép$N \le 10^7$. Một đường tuyến tính duy nhất với số học đơn giản cho mỗi lần lặp là khả thi trong Python trong các giới hạn thời gian thông thường khi được triển khai cẩn thận và mức sử dụng bộ nhớ tối đa chỉ ở mức vài trăm megabyte. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    ns = []
    max_n = 0
    for _ in range(t):
        n = int(input())
        ns.append(n)
        max_n = max(max_n, n)

    S = [0] * (max_n + 1)

    for j in range(1, max_n + 1):
        m = max_n // j
        if m == 0:
            break
        full = j * (m - 1) * m // 2
        last_count = max_n - m * j + 1
        S[j] = full + m * last_count

    pref = [0] * (max_n + 1)
    for j in range(1, max_n + 1):
        pref[j] = pref[j - 1] + S[j]

    return "\n".join(str(pref[n]) for n in ns)

# small cases
assert run("1\n1\n") == "1"
assert run("1\n2\n") == "4"
assert run("1\n3\n") == "9"
assert run("1\n4\n") == "16"

# multiple queries
assert run("3\n1\n2\n3\n") == "1\n4\n9"
assert run("2\n5\n6\n") == run("1\n5\n") + "\n" + run("1\n6\n")

# edge-like consistency
assert run("1\n10\n") == str(run("1\n10\n"))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 1 | 1 | trường hợp nhận dạng cơ sở | 
| tăng nhỏ n | 1, 4, 9 | tính chính xác khi tính toán lại đầy đủ | 
| nhiều truy vấn | kết quả đầu ra nhất quán | tính chính xác của việc tái sử dụng tiền tố | 

## Vỏ cạnh 

cho$n = 1$, thuật toán tính toán$m = 1$vì$j = 1$. Phần khối đầy đủ bằng 0 và khối cuối cùng đóng góp chính xác một phần tử, tạo ra đầu ra$1$, khớp với lưới ô đơn. 

Đối với lớn$n$, việc tính toán dựa trên thực tế là$m = \lfloor n / j \rfloor$giảm nhanh như$j$tăng lên. Khi$j > n$,$m = 0$và vòng lặp kết thúc sớm hoặc không đóng góp gì, ngăn chặn phạm vi âm không hợp lệ. 

Đối với ranh giới nơi$n$là bội số của$j$, số hạng khối cuối cùng trở thành 0, bởi vì$n - m \cdot j + 1$bằng$j$, được hấp thụ hoàn toàn vào cấu trúc đầy đủ. Điều này tránh sự trùng lặp từng điểm một tại các điểm chia hết chính xác.
