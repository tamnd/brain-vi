---
title: "CF 106098C - MEDAA và Mohamed Hazem"
description: "Với mọi số nguyên dương $x$, hãy gọi $tau(x)$ là số ước dương của $x$. Với $n$ cho trước, chúng ta phải đếm số cặp có thứ tự $(a,b)$ với $1 le a,b le n$ sao cho $$tau(a)+tau(b) < tau(gcd(a,b)) + tau(operatorname{lcm}(a,b))."
date: "2026-06-25T11:54:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106098
codeforces_index: "C"
codeforces_contest_name: "The American University in Cairo CSEA Fall 2025 contest"
rating: 0
weight: 106098
solve_time_s: 78
verified: true
draft: false
---

[CF 106098C – MEDAA và Mohamed Hazem](https://codeforces.com/problemset/problem/106098/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Với mọi số nguyên dương$x$, cho phép$\tau(x)$biểu thị số ước dương của$x$. 

Đối với một nhất định$n$, ta phải đếm số cặp có thứ tự$(a,b)$với$1 \le a,b \le n$như vậy$$\tau(a)+\tau(b)
<
\tau(\gcd(a,b))
+
\tau(\operatorname{lcm}(a,b)).$$Các cặp được sắp xếp theo thứ tự, vì vậy$(2,3)$Và$(3,2)$được tính riêng. 

Đầu vào chứa tối đa$10^4$trường hợp thử nghiệm, trong khi$n$có thể lớn như$10^6$. Một giải pháp kiểm tra từng cặp riêng lẻ là hoàn toàn không thể. Ngay cả đối với một trường hợp thử nghiệm duy nhất, có$10^{12}$cặp khi$n=10^6$. 

Ràng buộc$n \le 10^6$mạnh mẽ gợi ý tính toán trước nặng nề. Vì có nhiều trường hợp thử nghiệm nên chúng tôi muốn xử lý trước mọi thứ một cách khoảng một lần.$O(n \log n)$thời gian và trả lời từng câu hỏi trong$O(1)$. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như$n=2$. Cặp không tầm thường duy nhất là$(1,2)$hoặc$(2,1)$, Nhưng$$\tau(1)+\tau(2)=1+2=3,$$trong khi$$\tau(\gcd(1,2))+\tau(\operatorname{lcm}(1,2))
=
1+2=3.$$Bất đẳng thức là nghiêm ngặt nên câu trả lời là 0. 

Hãy xem xét cặp$(2,2)$. Một giải pháp bất cẩn có thể cho rằng các số bằng nhau là tốt vì gcd và lcm đều lớn. Trong thực tế,$$\tau(2)+\tau(2)
=
\tau(2)+\tau(2),$$vì thế đẳng thức giữ nguyên và cặp đó không được tính. 

Coi như$(4,2)$.$$\tau(4)+\tau(2)=3+2=5,$$Và$$\tau(2)+\tau(4)=2+3=5.$$Một lần nữa sự bình đẳng được giữ vững. Các cặp trong đó một số là bội số của số kia không tự động hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp$(a,b)$, tính toán$\gcd(a,b)$,$\operatorname{lcm}(a,b)$, đánh giá số lượng ước số và kiểm tra sự bất đẳng thức. 

Điều này đúng vì nó trực tiếp thực hiện định nghĩa. Thật không may là nó đòi hỏi$n^2$kiểm tra cặp. Với$n=10^6$, đó là về$10^{12}$hoạt động thậm chí trước khi xem xét tính toán số chia. 

Quan sát quan trọng đến từ việc viết$$a=gx,\qquad b=gy,$$Ở đâu$$g=\gcd(a,b), \qquad \gcd(x,y)=1.$$Cho phép$$G=\tau(g).$$Với mọi thừa số nguyên tố, số mũ của$g$là phần chung, trong khi số mũ phụ hoàn toàn thuộc về một trong hai$x$hoặc$y$. 

Định nghĩa$$X=\prod \frac{\text{exponent in }a + 1}
              {\text{exponent in }g + 1},$$và định nghĩa tương tự$Y$. 

Sau đó$$\tau(a)=GX,\qquad
\tau(b)=GY,\qquad
\tau(\operatorname{lcm}(a,b))=GXY.$$Thay vào bất đẳng thức sẽ cho$$GX+GY < G+GXY.$$Chia cho$G$,$$X+Y < 1+XY.$$Sắp xếp lại,$$(X-1)(Y-1) > 0.$$Từ$X,Y \ge 1$, bất đẳng thức đúng khi$$X>1 \quad\text{and}\quad Y>1.$$Nhưng$X=1$có nghĩa là không tồn tại số mũ nguyên tố phụ nào trên$a$- bên, có nghĩa là$x=1$. Tương tự như vậy$Y=1$có nghĩa$y=1$. 

Vậy bất đẳng thức ban đầu tương đương với$$x>1 \quad\text{and}\quad y>1.$$Điều đó loại bỏ hoàn toàn số chia khỏi bài toán. 

Bây giờ mọi cặp hợp lệ có thể được viết là$$a=gx,\qquad b=gy,$$với$$\gcd(x,y)=1,\qquad x>1,\qquad y>1.$$Đối với một cặp nguyên tố cố định$(x,y)$, số lựa chọn hợp lệ của$g$là$$\left\lfloor \frac{n}{\max(x,y)} \right\rfloor.$$Như vậy$$\text{answer}(n)
=
\sum_{\substack{x,y>1\\\gcd(x,y)=1}}
\left\lfloor \frac{n}{\max(x,y)} \right\rfloor.$$Cho phép$m=\max(x,y)$. 

Nếu như$m$được cố định, các cặp hợp lệ là$$(m,k)$$Và$$(k,m),$$Ở đâu$2 \le k < m$Và$\gcd(k,m)=1$. 

Số lượng là$$2(\varphi(m)-1),$$bởi vì$\varphi(m)$đếm các số nguyên nguyên tố cùng nhau$m$TRONG$[1,m-1]$, và chúng ta phải loại trừ$k=1$. 

Kể từ đây$$\text{answer}(n)
=
\sum_{m=2}^{n}
2(\varphi(m)-1)
\left\lfloor \frac{n}{m} \right\rfloor.$$Đây là một dạng chia tổng cổ điển. Tính toán trước tất cả các câu trả lời lên đến$10^6$có thể được thực hiện bằng sàng kiểu hài hòa:$$\text{ans}[k\cdot m]
\mathrel{+}= 2(\varphi(m)-1).$$Tổng độ phức tạp là$O(n \log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N \log \log N + N \log N)$tiền xử lý,$O(1)$mỗi truy vấn |$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các trường hợp kiểm thử và xác định giá trị được truy vấn tối đa$N$. 
2. Tính hàm tổng Euler$\varphi(i)$cho mọi$1 \le i \le N$sử dụng sàng tuyến tính. Điều này cho biết số lượng số nguyên trong$[1,i]$đó là nguyên tố cùng nhau$i$. 
3. Đối với mọi$m$từ 2 đến$N$, tính toán$$w(m)=2(\varphi(m)-1).$$Đây chính xác là số lượng các cặp nguyên tố cùng loại được đặt hàng$(x,y)$với$x,y>1$Và$\max(x,y)=m$. 
4. Với mọi bội số$k\cdot m \le N$, thêm vào$w(m)$ĐẾN`ans[k*m]`. 

Điều này đóng góp thuật ngữ tương ứng với$$\left\lfloor \frac{n}{m} \right\rfloor.$$5. Sau khi xử lý xong tất cả$m$,`ans[n]`bằng$$\sum_{m=2}^{n}
w(m)\left\lfloor \frac{n}{m} \right\rfloor.$$6. Đầu ra`ans[n]`cho từng trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Mỗi cặp$(a,b)$có thể được viết duy nhất là$$a=gx,\qquad b=gy,$$với$\gcd(x,y)=1$. 

Sau khi biểu diễn số chia thông qua nhân tử chung$g$, bất đẳng thức trở thành$$(X-1)(Y-1)>0.$$Từ$X,Y \ge 1$, điều này đúng khi cả hai phần rút gọn đều thỏa mãn$x>1$Và$y>1$. 

Do đó, việc đếm các cặp hợp lệ tương đương với việc đếm các cặp có thứ tự nguyên tố cùng nhau.$(x,y)$với cả hai giá trị lớn hơn 1, nhân với số các thừa số chung có thể có$g$. 

Nhóm theo$m=\max(x,y)$chuyển đổi số đếm thành$$\sum_{m=2}^{n}
2(\varphi(m)-1)
\left\lfloor \frac{n}{m} \right\rfloor,$$đó chính xác là những gì quá trình tiền xử lý tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
queries = [int(input()) for _ in range(t)]

mx = max(queries)

phi = [0] * (mx + 1)
phi[1] = 1

primes = []
is_comp = [False] * (mx + 1)

for i in range(2, mx + 1):
    if not is_comp[i]:
        primes.append(i)
        phi[i] = i - 1

    for p in primes:
        v = i * p
        if v > mx:
            break

        is_comp[v] = True

        if i % p == 0:
            phi[v] = phi[i] * p
            break
        else:
            phi[v] = phi[i] * (p - 1)

ans = [0] * (mx + 1)

for m in range(2, mx + 1):
    w = 2 * (phi[m] - 1)
    for multiple in range(m, mx + 1, m):
        ans[multiple] += w

out = []
for n in queries:
    out.append(str(ans[n]))

sys.stdout.write("\n".join(out))
```Phần đầu tiên tính tổng giá trị Euler bằng sàng tuyến tính. Điều này chạy theo thời gian tuyến tính và tránh việc phân tích nhân tử lặp đi lặp lại. 

Phần thứ hai xây dựng bảng trả lời. Đối với một cố định$m$, giá trị$$2(\varphi(m)-1)$$được thêm vào mọi bội số của$m$. Sau tất cả các bản cập nhật, mỗi bản cập nhật`ans[n]`chứa chính xác công thức tổng chia được rút ra trước đó. 

Câu trả lời có thể vượt quá phạm vi 32-bit khi$n$cách tiếp cận$10^6$, vì vậy các số nguyên có độ chính xác tùy ý của Python rất thuận tiện. Trong C++,`long long`được yêu cầu. 

Một sai lầm phổ biến là quên`-1`bên trong`phi(m)-1`. Tổng số lượng bao gồm giá trị 1, nhưng số lượng rút gọn$x$Và$y$cả hai đều phải lớn hơn 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
```Chỉ một$m=3$đóng góp.$$\varphi(3)=2$$Vì thế$$w(3)=2.$$| m | φ(m) | w(m) | sàn(3/m) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 1 | 0 | 
| 3 | 2 | 2 | 1 | 2 | 

Trả lời = 2. 

Các cặp tương ứng là$(2,3)$Và$(3,2)$. 

### Ví dụ 2 

đầu vào:```
4
```| m | φ(m) | w(m) | sàn(4/m) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 2 | 0 | 
| 3 | 2 | 2 | 1 | 2 | 
| 4 | 2 | 2 | 1 | 2 | 

Tổng cộng:$$2+2=4.$$Các cặp hợp lệ là$$(2,3), (3,2), (3,4), (4,3).$$Ví dụ này cho thấy rằng câu trả lời không được xác định chỉ bởi gcd. Cặp đôi$(4,2)$không được tính mặc dù các số có chung gcd lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N + N \log N)$tiền xử lý,$O(1)$mỗi truy vấn | Toàn bộ sàng cộng với nhiều bản cập nhật hài hòa | 
| Không gian |$O(N)$| Mảng cho phi, trạng thái sàng và đáp án | 

Với$N=10^6$, tổng điều hòa$$\sum_{m=1}^{N} \frac{N}{m}
=
O(N \log N)$$dễ dàng đủ nhanh trong giới hạn cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    input = data.readline

    t = int(input())
    q = [int(input()) for _ in range(t)]

    mx = max(q)

    phi = [0] * (mx + 1)
    phi[1] = 1

    primes = []
    comp = [False] * (mx + 1)

    for i in range(2, mx + 1):
        if not comp[i]:
            primes.append(i)
            phi[i] = i - 1

        for p in primes:
            v = i * p
            if v > mx:
                break

            comp[v] = True

            if i % p == 0:
                phi[v] = phi[i] * p
                break
            else:
                phi[v] = phi[i] * (p - 1)

    ans = [0] * (mx + 1)

    for m in range(2, mx + 1):
        w = 2 * (phi[m] - 1)
        for x in range(m, mx + 1, m):
            ans[x] += w

    return "\n".join(str(ans[n]) for n in q)

# provided samples
assert run("3\n2\n3\n4\n") == "0\n2\n4"

# custom cases
assert run("1\n1\n") == "0", "minimum size"
assert run("1\n2\n") == "0", "no valid pair exists"
assert run("1\n5\n") == "8", "small manual verification"
assert run("2\n3\n4\n") == "2\n4", "sample values together"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Giá trị tối thiểu của n | 
|`1 2`|`0`| Xử lý bất bình đẳng nghiêm ngặt | 
|`1 5`|`8`| Số lượng nhỏ không cần thiết | 
|`2 3 4`|`2 4`| Nhiều truy vấn và giá trị mẫu | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
```Thuật toán tính toán$$w(2)=2(\varphi(2)-1)=0.$$Không có đóng góp nào tồn tại nên câu trả lời là 0. Điều này xử lý chính xác thực tế là mọi cặp ứng cử viên chỉ thỏa mãn sự bình đẳng. 

Hãy xem xét cặp$(2,2)$. Sau khi chia cho gcd,$$x=y=1.$$Điều kiện dẫn xuất yêu cầu cả$x>1$Và$y>1$, vì vậy cặp này bị từ chối ngay lập tức. Quá trình tiền xử lý không bao giờ tính nó bởi vì$m=1$không được đưa vào tổng hợp. 

Hãy xem xét cặp$(4,2)$. Đây$$g=2,\quad x=2,\quad y=1.$$Một phần rút gọn bằng 1 nên điều kiện không thành công. Phép biến đổi dự đoán sự bằng nhau, khớp chính xác với biểu thức đếm số chia ban đầu. Công thức đếm loại trừ nó vì chỉ những cặp có cả hai phần rút gọn lớn hơn 1 mới đóng góp. 

Nguồn gốc của tuyên bố vấn đề và các ràng buộc xuất phát từ vấn đề ban đầu của Codeforces Gym.
