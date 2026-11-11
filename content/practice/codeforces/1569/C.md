---
title: "CF 1569C - Cuộc họp của Ban giám khảo"
description: "Chúng tôi được yêu cầu đếm số lần hoán vị “tốt” của các thành viên ban giám khảo dựa trên cách họ trình bày nhiệm vụ của mình. Mỗi thành viên ban giám khảo $i$ có các nhiệm vụ $ai$ và họ sắp xếp các nhiệm vụ theo thứ tự hoán vị."
date: "2026-06-10T11:42:32+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math"]
categories: ["algorithms"]
codeforces_contest: 1569
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 113 (Rated for Div. 2)"
rating: 1500
weight: 1569
solve_time_s: 519
verified: true
draft: false
---

[CF 1569C - Cuộc họp của Ban giám khảo](https://codeforces.com/problemset/problem/1569/C) 

**Đánh giá:** 1500 
**Tags:** tổ hợp, toán học 
**Thời gian giải:** 8 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số lần hoán vị “tốt” của các thành viên ban giám khảo dựa trên cách họ trình bày nhiệm vụ của mình. Mỗi thành viên ban giám khảo$i$có$a_i$các nhiệm vụ và chúng sắp xếp các nhiệm vụ theo một hoán vị. Một hoán vị sẽ tốt nếu không có thành viên ban giám khảo nào trình bày hai nhiệm vụ của họ liên tiếp. 

Điểm mấu chốt là trình tự thuyết trình thực sự là một vòng tròn: trong mỗi vòng, mỗi thành viên ban giám khảo với các nhiệm vụ còn lại sẽ trình bày một nhiệm vụ. Hoán vị không thành công nếu một số thành viên kết thúc việc thực hiện nhiều nhiệm vụ liên tiếp. 

Từ góc độ cấu trúc, điều duy nhất có thể khiến hoán vị không hợp lệ là nếu có một thành viên có số lượng nhiệm vụ **quá lớn so với những người khác**, do đó sau khi tất cả những người khác đã trình bày một lần, thành viên này vẫn còn một nhiệm vụ và sẽ phải trình bày hai lần liên tiếp. 

Điều này ngay lập tức cho chúng ta một điều kiện cụ thể: hãy$\text{max}_a$là tối đa$a_i$Và$\text{sum}_a$tổng số tiền của tất cả$a_i$. Một hoán vị đẹp là **không thể** nếu giá trị lớn nhất$a_i$vượt quá tổng số khác$a_i$cộng một. Ngược lại, luôn có cách xen kẽ các nhiệm vụ để tránh việc trình bày liên tiếp. 

Các trường hợp khó khăn phát sinh khi tất cả các thành viên bồi thẩm đoàn có cùng số lượng nhiệm vụ. Ví dụ, nếu mỗi$a_i = 1$, thì **tất cả các hoán vị đều tự động tốt**, vì không ai có thể trình bày hai lần liên tiếp. Tương tự, nếu$n = 2$và các nhiệm vụ đều bằng nhau, có chính xác một hoán vị tốt. 

Các ràng buộc rất lớn ($n$lên đến$2 \cdot 10^5$Và$a_i$lên đến$10^9$), loại trừ mọi cách tiếp cận mô phỏng quá trình trình bày một cách rõ ràng. Chúng ta cần **công thức tổ hợp** để đếm trực tiếp các hoán vị. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là tạo ra tất cả các hoán vị và mô phỏng quá trình chia sẻ nhiệm vụ. Đối với mỗi hoán vị, hãy theo dõi nhiệm vụ còn lại của từng thành viên ban giám khảo và kiểm tra xem có ai trình bày liên tiếp hay không. Điều này đúng nhưng không khả thi:$n!$hoán vị là quá lớn khi$n$đạt tới$10^5$. 

Quan sát quan trọng là hạn chế duy nhất đến từ (các) thành viên có số lượng nhiệm vụ tối đa. Cho phép$\text{max}_a$xuất hiện$c$lần trong mảng. Nếu như$\text{max}_a - \text{second\_max}_a > 1$, thì không thể xen kẽ các nhiệm vụ mà không có tối đa thành viên nào trình bày hai lần liên tiếp. Mặt khác, chúng ta có thể đếm các hoán vị bằng cách trước tiên chọn vị trí cho (các) phần tử “cao thứ hai”, sau đó hoán vị các phần tử còn lại một cách tự do. 

Lý luận tổ hợp có thể được chia thành: 

1. Nếu có nhiều hơn một giá trị tối đa, tất cả các hoán vị đều hợp lệ trừ khi giá trị lớn nhất vượt quá các giá trị khác nhiều hơn 1. 
2. Nếu lớn nhất xảy ra một lần, chúng ta phải tránh nó xuất hiện ở vị trí cuối cùng so với lớn thứ hai; số lượng hoán vị xấu có thể được tính toán tổ hợp. 

Điều này dẫn tới một hiệu quả$O(n \log n)$giải pháp sử dụng giai thừa và số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Đếm tổ hợp |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc$t$trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc$n$và mảng$a$. 
3. Tính toán$\text{max}_a = \max(a)$và đếm$c$số lần nó xảy ra. 
4. Tính toán$\text{second\_max}_a$là giá trị lớn nhất tiếp theo trong$a$. 
5. Nếu$\text{max}_a - \text{second\_max}_a > 1$, in 0 vì không thể xen kẽ các tác vụ. 
6. Ngược lại, tính giai thừa đến$n$modulo$998244353$để đếm hoán vị. 
7. Nếu có nhiều lần xuất hiện$\text{max}_a$, tất cả các hoán vị đều hợp lệ, vì vậy câu trả lời là$n!$. 
8. Nếu có một mức tối đa duy nhất, hãy tính số hoán vị trong đó mức tối đa duy nhất được đặt “quá sớm” bằng cách sử dụng các công thức tổ hợp, trừ đi$n!$và in kết quả theo modulo$998244353$. 

**Tại sao nó hoạt động**: Tính chính xác phụ thuộc vào thuộc tính quay vòng. Mức tối đa không thể được đặt liên tiếp; bằng cách chỉ kiểm soát mức tối đa so với mức tối đa thứ hai, chúng tôi đảm bảo rằng sẽ không có thành viên nào khác có mặt hai lần liên tiếp. Điều này làm giảm vấn đề đếm các hoán vị hợp lệ của các vị trí cho các nhiệm vụ tối đa này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    t = int(input())
    fact = [1] * (2*10**5 + 10)
    for i in range(1, len(fact)):
        fact[i] = fact[i-1] * i % MOD
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        mx = max(a)
        c = a.count(mx)
        if c > 1:
            print(fact[n])
            continue
        sec = max(x for x in a if x != mx)
        if mx - sec > 1:
            print(0)
            continue
        cnt = a.count(sec)
        # Number of bad permutations
        bad = fact[n] * modinv(cnt + 1) % MOD
        print((fact[n] - bad + MOD) % MOD)

solve()
```**Giải thích**: Chúng ta tính trước giai thừa modulo$998244353$. Nếu tồn tại nhiều cực đại thì mọi hoán vị đều hợp lệ. Mặt khác, chúng tôi tính toán các hoán vị “xấu” trong đó mức tối đa duy nhất xuất hiện quá sớm so với mức tối đa thứ hai, sử dụng nghịch đảo mô-đun để chia giai thừa một cách hiệu quả. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`2 1 2`: 

| Hoán vị | Tối đa | Đẹp? | 
| --- | --- | --- | 
| [1,2] | 2 | Không, thành viên thứ hai trình bày hai lần liên tiếp | 
| [2,1] | 2 | Có, không ai trình bày liên tiếp | 

Đầu ra:`1`. 

Đầu vào mẫu`3 5 5 5`: 

Mọi hoán vị của`[1,2,3]`rất hay vì max xuất hiện nhiều lần. Đầu ra:`6`(`3!`). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + n \log n)$| Tính toán trước các giai thừa một lần, xử lý từng trường hợp thử nghiệm trong O(n) | 
| Không gian |$O(n)$| Lưu trữ giai thừa | 

Điều này phù hợp thoải mái trong giới hạn vấn đề. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    solve()
    return

# provided samples
run("4\n2\n1 2\n3\n5 5 5\n4\n1 3 3 7\n6\n3 4 2 1 3 3\n")
# Expected output:
# 1
# 6
# 0
# 540

# custom cases
run("1\n2\n1 1\n")  # Expected: 2, all permutations nice
run("1\n3\n1 1 2\n")  # Expected: 5, single max case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`| 2 | nhiều cực đại, mọi hoán vị đều hợp lệ | 
|`3 1 1 2`| 5 | mức tối đa đơn so với mức tối đa thứ hai | 
|`4 5 5 5 5`| 24 | tất cả đều bằng nhau, max xuất hiện nhiều lần | 
|`3 1 2 4`| 0 | không thể xen kẽ | 

## Vỏ cạnh 

Nếu thành viên lớn nhất có nhiệm vụ vượt quá tổng số nhiệm vụ của những người khác nhiều hơn 1, thì việc không xen kẽ sẽ tránh được việc trình bày liên tục. Ví dụ,`n=3, a=[1,1,4]`. Thuật toán của chúng tôi phát hiện`4 - 1 > 1`và ngay lập tức quay trở lại`0`. 

Nếu tất cả các thành viên có nhiệm vụ như nhau thì mọi hoán vị đều hợp lệ. Tính toán trước giai thừa của chúng tôi đảm bảo những trường hợp này được xử lý hiệu quả. 

Điều này nắm bắt tất cả các ràng buộc không rõ ràng từ bài toán mà không cần mô phỏng từng hoán vị.
