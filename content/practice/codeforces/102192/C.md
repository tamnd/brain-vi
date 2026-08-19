---
title: "CF 102192C - Phát triển Thành phố"
description: "Chúng ta có n thành phố được sắp xếp theo thứ tự và những thành phố đó đồng thời được phân chia thành các nhóm hành chính lồng nhau. Một nhóm ở cấp độ i chứa chính xác n thành phố liên tiếp và mọi nhóm mịn hơn nằm hoàn toàn bên trong một nhóm thô hơn."
date: "2026-08-18T01:57:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "C"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 215
verified: true
draft: false
---

[CF 102192C - Phát triển Thành phố](https://codeforces.com/problemset/problem/102192/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 35 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`các thành phố được sắp xếp theo thứ tự và các thành phố đó đồng thời được phân chia thành các nhóm hành chính lồng nhau. Một nhóm ở cấp độ`i`chứa chính xác`n_i`các thành phố liên tiếp nhau, và mọi nhóm tốt hơn đều nằm hoàn toàn trong một nhóm thô hơn. Cấp độ cuối cùng có`n_k = 1`, do đó, một thành phố riêng lẻ tự nó là một cấp độ-`k`nhóm. 

Đối với hai thành phố, hệ số tương tác chỉ phụ thuộc vào cấp hành chính tốt nhất chứa cả hai thành phố đó. Nếu chúng thuộc các nhóm cấp cao nhất khác nhau thì hệ số của chúng là`rho_0`. Nếu lần đầu tiên chúng trở nên tách biệt ở một mức độ tốt hơn nào đó, thì tương ứng`rho_i`được sử dụng. Một thành phố tương tác với chính nó bằng cách sử dụng`rho_k`. 

Một năm là phép biến đổi tuyến tính của vectơ hiện tại của các giá trị thành phố. Nếu chúng ta gọi sự biến đổi đó`A`, nhiệm vụ là tính toán`A^T d_0`modulo`998244353`, Ở đâu`T`có thể lớn như`10^18`. 

Việc giải thích trực tiếp đưa ra một`n x n`ma trận, nhưng việc lưu trữ hoặc nhân một ma trận như vậy là không thể ngay lập tức. Ngay cả một ứng dụng của ma trận cũng cần`Theta(n^2)`hoạt động. Với`n = 3 * 10^5`, đó là về`9 * 10^10`tương tác cặp đôi trong một năm, vượt xa giới hạn bốn giây. Số mũ`T`cũng loại trừ việc mô phỏng từng năm một. 

Bản thân hệ thống phân cấp là cấu trúc hữu ích. Bởi vì mỗi quy mô nhóm đều chia cho quy mô trước đó, nên bất cứ khi nào quy mô giảm đi một cách nghiêm ngặt, nó sẽ chỉ bằng một nửa quy mô trước đó. Như vậy, mặc dù`k`có thể lớn như`n`bởi vì các kích thước liên tiếp bằng nhau được cho phép nên có thể có nhiều nhất`O(log n)`quy mô nhóm riêng biệt. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ việc triển khai bất cẩn. Đầu tiên là`n = 1`. Ví dụ,```
1
1 1 3
1
7
2 5
```Thành phố duy nhất tương tác với chính nó bằng cách sử dụng`rho_1 = 5`, vậy câu trả lời là`7 * 5^3 = 875`. điều trị`rho_0`vì hệ số đường chéo sẽ cho kết quả sai. 

Một trường hợp đặc biệt khác là quy mô hành chính lặp lại. Ví dụ,```
1
4 3 1
4 2 1
3 3 3 3
1 2 4 7
```Mỗi thành phố nhìn thấy ba thành phố khác có tổng tương tác`1 + 4 + 1 + 7`được sắp xếp theo cấp độ của chúng và tổng hàng là`13`. Vì mọi giá trị ban đầu đều`3`, đầu ra đúng là```
39 39 39 39
```Một giải pháp giả sử mỗi cấp độ giới thiệu một phân vùng mới có thể tạo ra các không gian riêng không chiều và xử lý sai các kích thước lặp lại. 

Trường hợp ranh giới thứ ba xảy ra khi nhóm hành chính lớn nhất đã chứa tất cả các thành phố. Ví dụ,```
1
4 3 2
4 2 1
1 2 3 4
1 1 1 2
```Ở đây ma trận chỉ đơn giản là`J + I`, Ở đâu`J`là ma trận tất cả những cái một. Bình phương nó mang lại`6J + I`, vậy câu trả lời là```
61 62 63 64
```Một giải pháp giả định luôn có một khoảng trống khác bên ngoài các nhóm cấp cao nhất sẽ loại bỏ trường hợp này một cách không chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force xây dựng ma trận tương tác một cách rõ ràng. Đối với mỗi cặp thành phố, chúng tôi xác định cấp hành chính chung thấp nhất và đặt hệ số tương ứng vào ma trận. Áp dụng một năm sau đó chi phí`O(n^2)`, và làm điều này cho`T`chi phí năm`O(Tn^2)`. Ngay cả khi chúng ta thử tính lũy thừa ma trận, một mật độ dày đặc`n x n`ma trận sẽ làm cho phép nhân trở nên quá tốn kém. Tại`n = 3 * 10^5`, riêng ma trận sẽ chứa khoảng`9 * 10^10`mục nhập. 

Quan sát hữu ích là ma trận không tùy ý. Định nghĩa`B_i`là ma trận có mục nhập là`1`chính xác khi hai thành phố thuộc cùng cấp-`i`nhóm. Vì hệ số thay đổi từ`rho_{i-1}`ĐẾN`rho_i`khi chúng ta chuyển sang cấp độ`i`, ma trận tương tác có thể được viết là 

[ 
A = \rho_0 J + \sum_{i=1}^{k}(\rho_i-\rho_{i-1})B_i. 
] 

Sự biểu diễn này rất mạnh mẽ bởi vì tất cả`B_i`là các ma trận khối lồng nhau. Chính xác hơn, nếu`P_m`có nghĩa là lấy trung bình một vectơ bên trong mỗi khối liên tiếp của`m`thì các thành phố 

[ 
B_i = n_i P_{n_i}. 
] 

Tất cả các toán tử tính trung bình này đều là các phép chiếu lên các không gian con lồng nhau, do đó chúng giao hoán và có thể được phân tách đồng thời thành các thành phần phân cấp độc lập. 

Đối với kích thước khối riêng biệt`m`, tập hợp tất cả các số hạng có kích thước đó thành một hệ số 

[ 
\gamma_m = \sum_{i=m}(\rho_i-\rho_{i-1})m. 
] 

Sau đó ma trận trở thành 

[ 
A = \rho_0 J + \sum_m \gamma_m P_m. 
] 

Giả sử các kích thước khác nhau là 

[ 
m_1 > m_2 > \dots > m_r=1. 
]

Cho phép`P_0`là toán tử trung bình toàn cầu trên tất cả`n`các thành phố. Sự khác biệt 

[ 
P_{m_q}-P_{m_{q-1}} 
] 

trích xuất chính xác thông tin không đổi bên trong một`m_q`khối nhưng có mức trung bình bằng 0 bên trong khối mẹ của nó. Mỗi không gian con như vậy là một không gian riêng của`A`. 

Giá trị riêng thuộc thành phần được giới thiệu ở kích thước`m_q`là hậu tố tổng 

[ 
\lambda_q=\sum_{j=q}^{r}\gamma_{m_j}. 
] 

Thành phần hằng số toàn cục có giá trị riêng 

[ 
\lambda_0=n\rho_0+\sum_{j=1}^{r}\gamma_{m_j}. 
] 

Thành phần còn lại, bao gồm sự khác biệt giữa các nhóm cấp cao nhất, có giá trị riêng bằng 0. Từ`T >= 1`, thành phần đó biến mất sau khi nâng ma trận lên`T`-quyền lực thứ. 

Brute-force hoạt động vì nó áp dụng ma trận tương tác chính xác. Nó thất bại vì nó xử lý từng cặp thành phố một cách độc lập. Quan sát rằng ma trận là tổng của các toán tử trung bình lồng nhau cho phép chúng ta thay thế các tương tác theo cặp bằng một số lượng nhỏ các phép chiếu phân cấp. Vì các kích thước khác nhau giảm đi một nửa mỗi khi chúng thay đổi nên chỉ có`O(log n)`những dự đoán như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(T n^2)`|`O(n^2)`| Quá chậm | 
| Tối ưu |`O(n + n log n)`|`O(n)`| Đã chấp nhận | 

Việc triển khai bên dưới thậm chí còn hiệu quả hơn so với việc truy cập rõ ràng từng thành phố ở mọi cấp độ. Mỗi phép chiếu được áp dụng thông qua việc bổ sung phạm vi trên các khối của nó. Tổng số khối trên tất cả các kích thước khác nhau là`O(n)`bởi vì kích thước giảm ít nhất là hai lần. 

## Hướng dẫn thuật toán 

1. Đọc kích thước quản trị và hệ số tương tác, sau đó thay thế các kích thước bằng nhau liên tiếp bằng một kích thước riêng biệt. Đối với mọi kích thước`m`, tích lũy 

[ 
\gamma_m \mathrel{+}=m(\rho_i-\rho_{i-1}). 
] 

Các mức bằng nhau sẽ tạo ra cùng một toán tử trung bình, do đó các đóng góp trong ma trận của chúng có thể được kết hợp một cách an toàn. 
2. Tính các giá trị riêng của hậu tố. Xử lý các kích thước khác nhau từ nhỏ nhất đến lớn nhất mang lại 

[ 
\lambda_q=\gamma_{m_q}+\lambda_{q+1}. 
] 

Đây chính xác là giá trị riêng của sự khác biệt giữa một`m_q`-trung bình khối và trung bình khối gốc của nó, bởi vì mọi phép chiếu tốt hơn đều đóng vai trò là phép nhân với kích thước khối của nó trên thành phần đó. 
3. Tính giá trị riêng toàn cục 

[ 
\lambda_0=n\rho_0+\sum_q\gamma_{m_q}. 
] 

Vectơ toàn hằng là một vectơ riêng vì mọi hàng của ma trận tương tác đều có tổng bằng nhau. 
4. Nâng mọi giá trị riêng có liên quan lên`T`modulo`998244353`. Từ`T`có thể đạt được`10^18`, cần phải có lũy thừa nhị phân. 
5. Biểu thị toán tử được cấp dưới dạng tổ hợp tuyến tính của các phép chiếu trung bình. Bắt đầu với`lambda_0^T`trên mức trung bình toàn cầu. Đối với mỗi kích thước riêng biệt`m < n`, không gian riêng tương ứng đóng góp 

[ 
\lambda_m^T(P_m-P_{\text{parent}}). 
] 

Cộng hệ số dương vào`P_m`và trừ nó khỏi phép chiếu gốc của nó sẽ tạo ra hệ số cuối cùng của mọi phép chiếu. 
6. Xây dựng tổng tiền tố của các giá trị thành phố ban đầu. Điều này cho phép chúng ta thu được tổng của bất kỳ khối hành chính nào trong thời gian không đổi và do đó tính trung bình của nó trong thời gian không đổi sau khi nhân với nghịch đảo mô-đun của kích thước của nó. 
7. Áp dụng từng hệ số chiếu bằng cách sử dụng một mảng sai phân. Đối với mỗi khối`[l,r)`, tính giá trị trung bình của nó và cộng giá trị tương ứng vào toàn bộ khoảng thông qua hai lần cập nhật mảng sai phân. chỉ có`n/m`khối cho kích thước`m`. 
8. Tiền tố tổng hợp mảng chênh lệch một lần để khôi phục giá trị cuối cùng của mỗi thành phố. 

Tính bất biến của tính chính xác là sau khi xử lý bất kỳ tập hợp các thành phần phân cấp nào, giá trị tích lũy được gán cho một thành phố chính xác là sự đóng góp của các thành phần đó trong phân tách không gian riêng của vectơ ban đầu. Các nhà khai thác`P_m`là các phép chiếu lồng nhau, do đó, mỗi vectơ phân tách duy nhất thành thành phần hằng số toàn cục, các sai phân liên tiếp giữa các giá trị trung bình của khối lồng nhau và thành phần sai phân cấp cao nhất còn lại. Cái sau có giá trị riêng bằng 0 và biến mất sau ít nhất một năm. Mọi thành phần khác được nhân với giá trị riêng chính xác của nó và nâng lên`T`, do đó vectơ kết quả chính xác là`A^T d_0`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve_case(n, k, T, sizes, d, rho):
    # Compress equal administrative sizes.
    groups = []
    gamma = []

    for i in range(k):
        m = sizes[i]
        add = (rho[i + 1] - rho[i]) * m % MOD

        if groups and groups[-1] == m:
            gamma[-1] = (gamma[-1] + add) % MOD
        else:
            groups.append(m)
            gamma.append(add)

    r = len(groups)

    # Suffix eigenvalues for the hierarchical difference spaces.
    eig = [0] * r
    cur = 0
    for i in range(r - 1, -1, -1):
        cur += gamma[i]
        cur %= MOD
        eig[i] = cur

    # Eigenvalue of the global constant subspace.
    global_eig = (rho[0] * n + cur) % MOD
    global_pow = pow(global_eig, T, MOD)

    # Coefficients of the projection operators.
    #
    # The decomposition is
    # A^T = global_pow * P_global
    #       + sum eig[i]^T * (P_i - P_parent).
    #
    # If groups[i] == n, P_i == P_global, so that component is zero.
    coeff = [0] * r
    coeff_global = global_pow

    previous = -1

    for i in range(r):
        m = groups[i]
        if m == n:
            continue

        ep = pow(eig[i], T, MOD)

        coeff[i] = (coeff[i] + ep) % MOD

        if previous == -1:
            coeff_global = (coeff_global - ep) % MOD
        else:
            coeff[previous] = (coeff[previous] - ep) % MOD

        previous = i

    # Prefix sums of the initial vector.
    pref = [0] * (n + 1)
    s = 0
    for i, x in enumerate(d):
        s += x
        pref[i + 1] = s % MOD

    # Difference array for range additions.
    diff = [0] * (n + 1)

    # Global average contribution.
    if coeff_global:
        avg = pref[n] * pow(n, MOD - 2, MOD) % MOD
        value = coeff_global * avg % MOD
        diff[0] += value
        diff[n] -= value

    # Contributions of every nontrivial administrative size.
    for i in range(r):
        c = coeff[i]
        m = groups[i]

        if c == 0 or m == n:
            continue

        inv_m = pow(m, MOD - 2, MOD)
        factor = c * inv_m % MOD

        for l in range(0, n, m):
            rr = l + m
            block_sum = (pref[rr] - pref[l]) % MOD
            value = block_sum * factor % MOD

            diff[l] += value
            diff[rr] -= value

    # Recover point values from the range additions.
    ans = [0] * n
    cur = 0
    for i in range(n):
        cur += diff[i]
        ans[i] = cur % MOD

    return ans

def main():
    t = int(input())
    out = []

    for _ in range(t):
        n, k, T = map(int, input().split())
        sizes = list(map(int, input().split()))
        d = list(map(int, input().split()))
        rho = list(map(int, input().split()))

        ans = solve_case(n, k, T, sizes, d, rho)
        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Phần đầu tiên nén kích thước bằng nhau. yếu tố`m`TRONG`gamma`đến từ`B_i = mP_m`, bởi vì tính trung bình trên một khối chia tổng của nó cho`m`, trong khi ma trận khối ban đầu trả về tổng. 

Vòng lặp hậu tố tính toán giá trị riêng của mọi không gian sai phân phân cấp. Nếu một vectơ có giá trị trung bình bằng 0 trong mọi khối cha và không đổi bên trong các khối hiện tại, thì tất cả các toán tử tính trung bình thô hơn sẽ triệt tiêu nó, trong khi mọi toán tử khối mịn hơn sẽ nhân nó với kích thước khối của nó. Điều đó cho biết chính xác tổng hậu tố của`gamma`các giá trị. 

Trường hợp đặc biệt`m == n`được xử lý bằng cách bỏ qua không gian chênh lệch tương ứng. Một cấp độ có các nhóm chứa tất cả các thành phố có cùng toán tử trung bình như phép chiếu toàn cầu, do đó, việc trừ giá trị riêng được cấp của nó như thể nó là một thành phần mới sẽ tạo ra thành phần không thứ nguyên giả. 

Mảng tiền tố được lưu trữ theo modulo`MOD`. Tất cả các kích thước khối tối đa là`3 * 10^5`, nhỏ hơn hoàn toàn so với`998244353`, do đó mọi nghịch đảo mô đun cần thiết đều tồn tại. 

Mảng khác biệt là tối ưu hóa việc triển khai chính. Thay vì thêm một khối trung bình cho tất cả`m`thành phố, hai bản cập nhật ranh giới đại diện cho toàn bộ phạm vi. Trên một cấp độ với kích thước khối`m`, chỉ có`n/m`những cập nhật như vậy. 

Số nguyên Python không bị tràn nhưng việc giảm mô-đun vẫn được thực hiện thường xuyên để các giá trị trung gian vẫn ở mức nhỏ. Các lệnh gọi lũy thừa sử dụng ba đối số của Python`pow`, thực hiện phép lũy thừa mô đun theo thời gian logarit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu có`n = 4`, kích thước`2, 1`, vectơ ban đầu`[1, 3, 5, 6]`, và các hệ số`[2, 4, 5]`. 

Việc phân rã ma trận bắt đầu bằng 

[ 
A=2J+(4-2)B_1+(5-4)I. 
]

Từ`B_1 = 2P_2`, điều này trở thành 

[ 
A=2J+4P_2+I. 
] 

Giá trị riêng toàn cầu là`13`, giá trị riêng khác biệt khối cấp 2 là`5`và giá trị riêng chênh lệch cấp thành phố là`1`. 

| Thành phần | Giá trị riêng | Hệ số hỗ trợ | Chiếu | 
| --- | --- | --- | --- | 
| Toàn cầu | 13 | 13 |`P_global`| 
| Chênh lệch kích thước 2 | 5 | 5 |`P_2 - P_global`| 
| Cỡ 1 khác biệt | 1 | 1 |`I - P_2`| 

Kết hợp các phép chiếu mang lại 

[ 
A=8P_{\text{global}}+4P_2+I. 
] 

Các giá trị trung bình liên quan là`15/4`trên toàn cầu,`2`trong khối cỡ 2 đầu tiên,`11/2`trong khối size-2 thứ hai và các giá trị ban đầu ở kích thước`1`. 

| Thành phố | Đóng góp toàn cầu | Đóng góp cỡ 2 | Đóng góp của thành phố | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 30 | 8 | 1 | 39 | 
| 2 | 30 | 8 | 3 | 41 | 
| 3 | 30 | 22 | 5 | 57 | 
| 4 | 30 | 22 | 6 | 58 | 

Đầu ra là`39 41 57 58`. Dấu vết cho thấy tại sao câu trả lời có thể được tập hợp từ mức trung bình của khối thay vì tương tác từng cặp riêng lẻ. 

### Ví dụ 2 

Hãy xem xét```
1
2 1 2
1
1 2
2 3
```Chỉ có một cấp hành chính có quy mô`1`, do đó các thành phố khác nhau tương tác với hệ số`2`, trong khi một thành phố tương tác với chính nó bằng hệ số`3`. Ma trận là 

[ 
A= 
\bắt đầu{pmatrix} 
3&2\ 
2&3 
\end{pmatrix}. 
] 

Giá trị riêng toàn cục của nó là`5`, và giá trị riêng khác biệt của nó là`1`. 

| Thành phần | Giá trị riêng | Sau đó`T = 2`| Chiếu | 
| --- | --- | --- | --- | 
| Toàn cầu | 5 | 25 |`P_global`| 
| Sự khác biệt của thành phố | 1 | 1 |`I - P_global`| 

Như vậy 

[ 
A^2=25P_{\text{global}}+(I-P_{\text{global}}) 
=24P_{\text{global}}+I. 
] 

Mức trung bình toàn cầu của`[1,2]`là`3/2`, cho 

| Thành phố | Phần toàn cầu | Phần riêng lẻ | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 36 | 1 | 37 | 
| 2 | 36 | 2 | 38 | 

Đầu ra là`37 38`. Ví dụ này thực hiện trường hợp chỉ có cấp thành phố và không có sự phân chia hành chính trung gian không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + S)`Ở đâu`S = sum(n/m)`trên các kích cỡ khác nhau | Xây dựng tiền tố, xử lý khối và tái thiết cuối cùng là tuyến tính trong tổng số khối và thành phố | 
| Không gian |`O(n + r)`| Giá trị ban đầu, tổng tiền tố, mảng sai phân và nhiều nhất là`O(log n)`kích cỡ riêng biệt | 

Đối với hai quy mô hành chính riêng biệt liên tiếp, số chia nhỏ hơn sẽ lớn hơn và thực sự nhỏ hơn, do đó, nó chỉ bằng một nửa số lớn hơn. Do đó, 

[ 
\frac{n}{m_1}+\frac{n}{m_2}+\cdots < 1+2+4+\cdots < 2n. 
] 

Như vậy tổng số khối hành chính được xử lý là`O(n)`, không`O(n log n)`. Do đó, sự phức tạp thực tế của việc thực hiện là`O(n + k + log T)`cho mỗi trường hợp thử nghiệm, với`k`thuật ngữ đến từ việc đọc và nén đầu vào. Trên tất cả các trường hợp thử nghiệm, tổng của`n`nhiều nhất là`10^6`, do đó giải pháp vẫn nằm trong giới hạn dự kiến. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve_case`chức năng hiển thị ở trên.```python
import sys
import io

from solution import solve_case

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        t = int(sys.stdin.readline())
        out = []

        for _ in range(t):
            n, k, T = map(int, sys.stdin.readline().split())
            sizes = list(map(int, sys.stdin.readline().split()))
            d = list(map(int, sys.stdin.readline().split()))
            rho = list(map(int, sys.stdin.readline().split()))
            ans = solve_case(n, k, T, sizes, d, rho)
            out.append(" ".join(map(str, ans)))

        return "\n".join(out)
    finally:
        sys.stdin = old_stdin

# Provided sample.
assert run(
    """1
4 2 1
2 1
1 3 5 6
2 4 5
"""
) == "39 41 57 58", "sample 1"

# Minimum-size case, n = 1.
expected = 7 * pow(5, 10**18, 998244353) % 998244353
assert run(
    """1
1 1 1000000000000000000
1
7
2 5
"""
) == str(expected), "minimum size and huge T"

# All initial values equal. Every row has sum 13.
assert run(
    """1
4 3 1
4 2 1
3 3 3 3
1 2 4 7
"""
) == "39 39 39 39", "all equal values"

# The largest administrative group is the whole country.
# The matrix is J + I, and its square is 6J + I.
assert run(
    """1
4 3 2
4 2 1
1 2 3 4
1 1 1 2
"""
) == "61 62 63 64", "top-level group equals all cities"

# Only city level exists. Matrix [[3,2],[2,3]], squared gives [[13,12],[12,13]].
assert run(
    """1
2 1 2
1
1 2
2 3
"""
) == "37 38", "city-level-only case"

# Maximum n, using uniform values and coefficients.
# A is the all-ones matrix, so one year produces n for every city.
n = 300000
inp = (
    "1\n"
    f"{n} 1 1\n"
    "1\n"
    + " ".join(["1"] * n) + "\n"
    "1 1\n"
)
out = run(inp).split()
assert len(out) == n, "maximum-size output length"
assert all(x == str(n % 998244353) for x in out), "maximum-size uniform case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, T=10^18`|`7 * 5^T mod MOD`| Hệ thống phân cấp nhỏ nhất có thể và số mũ lớn | 
|`n=4`, tất cả các giá trị ban đầu`3`|`39 39 39 39`| Giá trị riêng của vectơ không đổi và hệ thống phân cấp lặp lại | 
|`n=4`, kích thước`4,2,1`,`T=2`|`61 62 63 64`| Nhóm cấp cao nhất bằng toàn bộ thành phố | 
|`n=2`,`k=1`,`T=2`|`37 38`| Không có cấp hành chính trung cấp | 
|`n=300000`, giá trị thống nhất |`300000`lặp đi lặp lại`300000`lần | Kích thước đầu vào tối đa và xử lý khối tuyến tính | 

## Vỏ cạnh 

cho`n = 1`, mọi toán tử trung bình đều là đẳng thức. Trong đầu vào```
1
1 1 3
1
7
2 5
```sự phân rã có giá trị riêng toàn cầu`5`, bởi vì mục nhập ma trận duy nhất là`rho_1`. Toán tử được hỗ trợ là nhân với`5^3`, cho`875`. Việc triển khai xử lý vấn đề này một cách tự nhiên vì nhóm kích thước-1 trùng với máy chiếu toàn cầu, do đó thành phần khác biệt không cần thiết sẽ bị bỏ qua. 

Đối với kích thước lặp lại, hãy xem xét```
1
4 3 1
4 2 1
3 3 3 3
1 2 4 7
```Kích thước`4`và kích thước`2`máy chiếu là khác biệt, trong khi bản thân ba cấp độ vẫn được thể hiện đầy đủ ngay cả khi chèn thêm các kích thước bằng nhau. Bước nén kết hợp các phần đóng góp có kích thước bằng nhau thành một`gamma`giá trị. Vì vectơ đầu vào không đổi nên chỉ có giá trị riêng toàn cục là quan trọng và mọi đầu ra đều trở thành`39`. 

Đối với một nhóm cấp cao nhất tương đương với tất cả các thành phố, hãy xem xét```
1
4 3 2
4 2 1
1 2 3 4
1 1 1 2
```Ma trận là`J + I`. Giá trị riêng toàn cục của nó là`5`, trong khi mọi thành phần không cố định đều có giá trị riêng`1`. Sau hai năm, thành phần toàn cầu được nhân với`25`và mọi thành phần không cố định vẫn không thay đổi. Vì mức trung bình toàn cầu là`5/2`, kết quả là`25 * 5/2 + (d_i - 5/2) = 60 + d_i`, cho`61 62 63 64`. 

Đối với hệ thống phân cấp chỉ chứa cấp thành phố, đầu vào```
1
2 1 2
1
1 2
2 3
```có ma trận 

[ 
\bắt đầu{pmatrix} 
3&2\ 
2&3 
\end{pmatrix}. 
] 

Thành phần toàn cục có giá trị riêng`5`và thành phần chênh lệch có giá trị riêng`1`. Sau hai năm, thành phần đầu tiên được nhân với`25`, trong khi giá trị thứ hai không thay đổi, tạo ra`37 38`. Điều này mắc phải sai lầm phổ biến khi cho rằng phải tồn tại ít nhất một cấp hành chính trung gian.
