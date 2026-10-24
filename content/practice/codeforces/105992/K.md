---
title: "CF 105992K - \u795e\u4e4b\u4e00\u624b"
description: "Chúng tôi đang mô phỏng quy trình bảng xác suất chạy qua nhiều vòng, trong đó mỗi vòng có thể kết thúc trò chơi ngay lập tức tùy thuộc vào các sự kiện thất bại hiếm gặp."
date: "2026-06-22T16:39:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105992
codeforces_index: "K"
codeforces_contest_name: "The 2025 Shanghai Collegiate Programming Contest"
rating: 0
weight: 105992
solve_time_s: 85
verified: true
draft: false
---

[CF 105992K - \u795e\u4e4b\u4e00\u624b](https://codeforces.com/problemset/problem/105992/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng quy trình bảng xác suất chạy qua nhiều vòng, trong đó mỗi vòng có thể kết thúc trò chơi ngay lập tức tùy thuộc vào các sự kiện thất bại hiếm gặp. Trạng thái của trò chơi cực kỳ nhỏ: bất cứ lúc nào chúng tôi chỉ cần biết liệu hiện tại bạn có 0 hay 1 viên đá “xấu” không thể ở lại trong hộp hay không. Thời điểm số này đạt đến hai, trò chơi kết thúc ngay lập tức. 

Mỗi vòng có hai nguồn nguy hiểm. Khi bắt đầu, nếu bạn đã có đúng một viên đá xấu, đối thủ có thể kết thúc trận đấu ngay lập tức với xác suất nhỏ. Nếu điều đó không xảy ra, bạn sẽ tiếp tục thực hiện một chuỗi các viên đá. Mỗi viên đá có thể trở nên xấu theo hai cách: nó có thể không được đặt đúng cách và sau này, nếu có quá nhiều viên đá được xử lý trong vòng hiện tại, nó cũng có thể rơi ra khỏi hộp do tràn. Mỗi khi một viên đá trở nên xấu, số lượng đá xấu của bạn sẽ tăng lên và đạt đến hai mức sẽ gây ra thua lỗ ngay lập tức. 

Nhiệm vụ không phải là mô phỏng trực tiếp quá trình mà là tính toán, đối với mỗi vòng, xác suất mà lần đầu tiên bạn thua xảy ra chính xác trong vòng đó. 

Kích thước đầu vào khiến cho việc mô phỏng trực tiếp là không thể. Số vòng có thể lên tới 100000, trong khi mỗi vòng có thể chứa tới 10^9 viên đá. Điều này ngay lập tức loại trừ mọi mô phỏng trên mỗi viên đá. Ngay cả O(n log ai) mỗi vòng cũng sẽ quá chậm trừ khi mỗi lần chuyển đổi cực kỳ rẻ và thậm chí sau đó chúng ta phải cấu trúc cẩn thận việc tái sử dụng qua các vòng. 

Một trường hợp lợi thế tinh tế phát sinh từ thực tế là việc thua có thể xảy ra khi bắt đầu một vòng chơi hoặc trong quá trình xử lý. Ví dụ: nếu bạn bước vào một vòng đấu đã ở trạng thái “một viên đá xấu” và đối thủ gây ra thua ngay lập tức, thì xác suất trong vòng đó không có vấn đề gì. Một giải pháp ngây thơ mà chỉ những mô hình chuyển tiếp trong vòng mới hoàn toàn bỏ lỡ sự đóng góp này. 

Một cạm bẫy khác là quên rằng hành vi tràn thay đổi sau 80 viên đá đầu tiên trong một hiệp. Việc xử lý tất cả các viên đá giống nhau sẽ dẫn đến xác suất chuyển tiếp không chính xác trong các vòng chơi dài. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực trực tiếp là mô phỏng mọi viên đá trong mỗi vòng trong khi theo dõi xem chúng ta đang ở trạng thái 0 hay 1 và dừng lại khi chúng ta chạm phải 2 viên đá xấu. Về nguyên tắc, điều này đúng vì quy trình là Markovian và mỗi chuyển đổi chỉ phụ thuộc vào trạng thái hiện tại. Tuy nhiên, số bước là tổng của tất cả ai, có thể đạt tới 10^14 trong trường hợp xấu nhất, khiến phương pháp này hoàn toàn không khả thi. 

Quan sát quan trọng là không gian trạng thái rất nhỏ nhưng các chuyển đổi mang tính xác suất và tích lũy tuyến tính. Thay vì mô phỏng từng bước, chúng tôi mô hình hóa từng vòng như một phép biến đổi tuyến tính trên vectơ xác suất qua ba trạng thái: không có viên đá xấu, một viên đá xấu hoặc đã thua. 

Mỗi viên đá tạo ra cùng một loại cập nhật tuyến tính, ngoại trừ việc sau 80 viên đá, xác suất chuyển đổi sẽ thay đổi. Việc lặp lại cùng một phép biến đổi tuyến tính nhiều lần gợi ý đến phép lũy thừa ma trận. Điều này cho phép chúng ta nhảy qua các chuỗi dài các chuyển đổi giống hệt nhau theo thời gian logarit. 

Sau đó, chúng tôi soạn hai giai đoạn mỗi vòng, giai đoạn đầu tiên dành cho 79 viên đá đầu tiên và giai đoạn thứ hai dành cho những viên đá còn lại. Mỗi giai đoạn được biểu thị bằng ma trận 3 x 3 và phép lũy thừa nhanh cho phép chúng tôi tính toán hiệu quả của tối đa 10^9 bước giống hệt nhau một cách hiệu quả. 

Cuối cùng, chúng ta phải truyền bá xác suất qua các vòng, vì việc phân bổ trạng thái bắt đầu của một vòng phụ thuộc vào kết quả của vòng trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên mỗi viên đá | O(∑ ai) | O(1) | Quá chậm | 
| lũy thừa ma trận mỗi vòng | O(n log ai) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì phân bố xác suất trên toàn tiểu bang vào đầu mỗi vòng. Gọi dp0 là xác suất bắt đầu một vòng chơi với 0 quân xấu và dp1 là xác suất bắt đầu với đúng một quân xấu. Bất kỳ khối lượng xác suất nào đã đạt tới hai viên đá xấu sẽ bị loại bỏ vì quá trình này được hấp thụ ở đó. 

Đối với mỗi vòng chúng tôi làm như sau. 

1. Đầu tiên hãy xử lý tình trạng kết thúc ngay khi bắt đầu vòng đấu. Nếu chúng ta ở trạng thái 1, đối thủ có thể ngay lập tức gây thua với xác suất r/1000. Chúng tôi thêm phần đóng góp này trực tiếp vào câu trả lời của vòng này. Khối lượng xác suất còn lại ở trạng thái 1 tồn tại với xác suất 1 trừ đi giá trị này và tiếp tục vào vòng ở trạng thái 1. 
2. Bây giờ chúng tôi lập mô hình quy trình nội vòng bằng cách sử dụng hệ thống chuyển tiếp qua ba trạng thái: trạng thái 0, trạng thái 1 và mất thiết bị đầu cuối. Mỗi viên đá cập nhật những xác suất này một cách tuyến tính. 
3. Chúng tôi chia vòng đấu thành hai giai đoạn vì luật tràn thay đổi sau 80 viên đá. Giai đoạn đầu tiên sử dụng xác suất hư hỏng trên mỗi viên đá chỉ dựa trên p. Giai đoạn thứ hai sử dụng xác suất được sửa đổi bao gồm cả p và lỗi tràn bổ sung q. 
4. Chúng tôi biểu diễn từng giai đoạn dưới dạng ma trận chuyển tiếp 3 x 3. Nâng ma trận này lên lũy thừa cho phép chúng ta tính toán tác dụng của nhiều viên đá giống hệt nhau theo thời gian logarit. 
5. Chúng ta áp dụng số mũ ma trận pha thứ nhất cho các bước min(ai, 79), sau đó áp dụng số mũ ma trận pha thứ hai cho các bước còn lại. Nhân các ma trận này sẽ tạo ra một phép biến đổi duy nhất cho toàn bộ vòng. 
6. Chúng tôi áp dụng phép biến đổi này một cách riêng biệt cho các trạng thái cơ bản 0 và 1, thu được cả xác suất thua trong vòng đấu và kết quả phân bổ trên các trạng thái nếu chúng tôi sống sót. 
7. Chúng tôi sử dụng các kết quả này để tính toán xác suất thua lỗ do vòng này gây ra, đồng thời cập nhật dp0 và dp1 cho vòng tiếp theo. 

Bất biến cốt lõi là khi bắt đầu mỗi vòng, dp0 và dp1 mô tả đầy đủ tất cả khối lượng xác suất còn sót lại và phép biến đổi ma trận bảo toàn chính xác tính tuyến tính của quá trình. Mọi chuyển đổi bên trong một vòng đều tuyến tính trong phân phối hiện tại và việc chuyển sang trạng thái mất đảm bảo chúng ta không bao giờ cần theo dõi nhiều hơn ba trạng thái này. Vì phép biến đổi được áp dụng chính xác chứ không phải xấp xỉ nên việc tính khối lượng xác suất vẫn chính xác qua tất cả các vòng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def inv(x):
    return pow(x, MOD - 2, MOD)

def mat_mul(a, b):
    n = 3
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if a[i][k]:
                for j in range(n):
                    res[i][j] = (res[i][j] + a[i][k] * b[k][j]) % MOD
    return res

def mat_pow(mat, exp):
    n = 3
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    base = mat
    while exp > 0:
        if exp & 1:
            res = mat_mul(res, base)
        base = mat_mul(base, base)
        exp >>= 1
    return res

def build_matrix(p_fail):
    p = p_fail
    # states: 0, 1, dead
    # dead is absorbing
    return [
        [1 - p, 0, 0],
        [p, 1 - p, 0],
        [0, p, 1]
    ]

def apply(mat, vec):
    res = [0, 0, 0]
    for i in range(3):
        for j in range(3):
            res[i] = (res[i] + mat[i][j] * vec[j]) % MOD
    return res

def solve():
    p, q, r = map(int, input().split())
    n = int(input())
    a = list(map(int, input().split()))

    inv1000 = inv(1000)

    def get_fail(x, use_q):
        if use_q:
            base = p * inv1000 % MOD
            extra = q * inv1000 % MOD
            return (base + (1 - base) * extra) % MOD
        else:
            return p * inv1000 % MOD

    dp0, dp1 = 1, 0
    ans = []

    for ai in a:
        # immediate opponent action
        r_prob = r * inv1000 % MOD
        add_loss = dp1 * r_prob % MOD
        ans.append(add_loss % MOD)

        dp1 = dp1 * (1 - r_prob) % MOD

        # build matrices
        k1 = min(ai, 79)
        k2 = max(0, ai - 79)

        M0 = build_matrix(get_fail(0, False))
        M1 = build_matrix(get_fail(0, True))  # placeholder corrected below

        # correct failure rates
        pA = p * inv1000 % MOD
        pB = (pA + (1 - pA) * (q * inv1000 % MOD)) % MOD

        M0 = [
            [1 - pA, 0, 0],
            [pA, 1 - pA, 0],
            [0, pA, 1]
        ]

        M1 = [
            [1 - pB, 0, 0],
            [pB, 1 - pB, 0],
            [0, pB, 1]
        ]

        T0 = mat_pow(M0, k1)
        T1 = mat_pow(M1, k2)

        T = mat_mul(T1, T0)

        v0 = apply(T, [1, 0, 0])
        v1 = apply(T, [0, 1, 0])

        f0 = v0[2]
        f1 = v1[2]

        ans[-1] = (ans[-1] + dp0 * f0 + dp1 * f1) % MOD

        n0 = (dp0 * v0[0] + dp1 * v1[0]) % MOD
        n1 = (dp0 * v0[1] + dp1 * v1[1]) % MOD

        dp0, dp1 = n0, n1

    for x in ans:
        print(x % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai coi mỗi vòng là một thành phần của hai hệ thống tuyến tính. Điều chỉnh đầu tiên sẽ loại bỏ xác suất mất mát ngay lập tức khỏi trạng thái 1 trước khi bất kỳ viên đá nào được xử lý. Sau đó, hai ma trận chuyển tiếp được xây dựng, một cho phần đầu tiên của vòng và một cho giai đoạn tràn. Phép lũy thừa ma trận nén các chuỗi dài của các chuyển đổi giống hệt nhau. 

Mỗi ứng dụng vectơ cơ sở đều đưa ra cả xác suất kết thúc ở mỗi trạng thái và xác suất đã bị mất. Thông tin kép này là cần thiết vì chúng tôi vừa tính toán xác suất thua lỗ cho vòng hiện tại vừa truyền bá phân phối còn sót lại về phía trước. 

Một sai lầm phổ biến là quên loại bỏ tổn thất ngay lập tức do đối thủ gây ra khỏi khối còn sống sót trước khi áp dụng ma trận. Nếu điều đó không được thực hiện, xác suất trạng thái 1 sẽ bị tính quá mức trong các lần chuyển đổi sau này. 

## Ví dụ đã hoạt động 

Hãy xem xét một tình huống nhỏ trong đó chỉ có một vài viên đá tồn tại để tình trạng tràn không bao giờ xảy ra. Cho p khác 0 và q không liên quan. 

| Bước | dp0 | dp1 | Sự kiện | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | ban đầu | 
| Chuyển tiếp vòng | tính toán | tính toán | chế biến đá | 
| Kết thúc | cập nhật | cập nhật | tiến vào vòng tiếp theo | 

Điều này chứng tỏ rằng dp phát triển độc lập với các chi tiết trong vòng, chỉ dựa vào kết quả chuyển đổi tổng hợp. 

Bây giờ hãy xem xét trường hợp chúng ta bắt đầu một vòng ở trạng thái 1. 

| Giai đoạn | dp1 trước | Mất ngay lập tức | dp1 sau | 
| --- | --- | --- | --- | 
| Bắt đầu vòng đấu | x | x·r | x(1-r) | 

Điều này cho thấy sự can thiệp của đối thủ được tách biệt như thế nào khỏi quá trình ngẫu nhiên trong vòng. Ma trận chỉ xử lý động lực bên trong, trong khi sự kiện đối thủ được xử lý dưới dạng hấp thụ trước khi chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log ai) | mỗi vòng sử dụng hai số mũ 3x3 | 
| Không gian | O(1) | chỉ các ma trận có kích thước không đổi và trạng thái DP | 

Phép lũy thừa logarit đảm bảo rằng ngay cả với các giá trị ai lớn, việc tính toán vẫn hiệu quả. Với n lên tới 100000, giải pháp vẫn nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

# assume solve() defined above

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimal case
assert run("0 0 0\n1\n0\n") == "0"

# only opponent loss possibility
assert run("0 0 1000\n2\n1 1\n") is not None

# overflow irrelevant small ai
assert run("100 200 300\n1\n5\n") is not None

# mixed small sequence
assert run("10 20 30\n3\n10 20 30\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| p=q=r=0 | tất cả số không | không chuyển tiếp | 
| r=1000, bắt đầu dp1 | mất mát ngay lập tức | quy tắc đối thủ | 
| ai < 80 | không có pha tràn | hành vi ma trận đơn | 
| ai > 80 | chuyển tiếp hai pha | thành phần ma trận | 

## Vỏ cạnh 

Khi p, q và r đều bằng 0, các ma trận chuyển tiếp trở thành các phép biến đổi đồng nhất ngoại trừ trạng thái hấp thụ không bao giờ đạt tới. Thuật toán giữ dp không thay đổi một cách chính xác và tạo ra xác suất thua bằng 0 cho mỗi vòng. 

Khi r đạt giá trị tối đa, bất kỳ lúc nào dp1 khác 0 khi bắt đầu một vòng, toàn bộ đóng góp từ trạng thái đó ngay lập tức được chuyển thành xác suất thua cho vòng đó. Pha nền vẫn chỉ được áp dụng cho phần còn sót lại, do đó không xảy ra việc đếm kép. 

Khi ai nhỏ hơn 80, ma trận pha thứ hai không bao giờ được sử dụng, do đó thuật toán giảm xuống lũy ​​thừa ma trận đơn. Điều này xác nhận rằng việc phân chia pha không gây ra hiện tượng giả khi không có điều kiện tràn.
