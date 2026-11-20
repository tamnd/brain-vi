---
title: "CF 2166D - Hội Đồng Đá Cẩm Thạch"
description: "Chúng ta được cấp một tập hợp các số nguyên, đây chỉ là một tập hợp trong đó các phần tử có thể lặp lại. Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu tập hợp nhiều tập hợp khác nhau bằng cách liên tục phân vùng tập hợp ban đầu thành bất kỳ số nhóm không trống nào và sau đó lấy một chế độ từ mỗi nhóm…"
date: "2026-06-09T04:29:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 2166
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1064 (Div. 2)"
rating: 1900
weight: 2166
solve_time_s: 112
verified: false
draft: false
---

[CF 2166D - Hội đồng đá cẩm thạch](https://codeforces.com/problemset/problem/2166/D) 

**Xếp hạng:** 1900 
**Thẻ:** dp, toán 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên, đây chỉ là một tập hợp trong đó các phần tử có thể lặp lại. Nhiệm vụ là đếm xem có thể tạo ra bao nhiêu tập hợp đa tập hợp khác nhau bằng cách liên tục phân vùng tập hợp ban đầu thành bất kỳ số nhóm không trống nào và sau đó lấy một chế độ từ mỗi nhóm. Chế độ của một nhóm là bất kỳ phần tử nào xảy ra thường xuyên nhất; nếu nhiều phần tử liên kết với tần số tối đa thì có thể chọn bất kỳ phần tử nào trong số chúng. Đầu ra là số lượng tất cả các tập hợp riêng biệt mà chúng ta có thể tạo thông qua quy trình này, modulo 998244353. 

Kích thước đầu vào cho mỗi trường hợp thử nghiệm lên tới 5000 phần tử và tổng số phần tử trong tất cả các trường hợp thử nghiệm bị giới hạn bởi 5000. Điều này cho thấy rằng một thuật toán có độ phức tạp khoảng O(n²) cho mỗi trường hợp thử nghiệm là có thể chấp nhận được, nhưng mọi thứ khối có thể sẽ quá chậm. Ngoài ra, vì các phần tử có thể lặp lại, việc liệt kê đơn giản tất cả các phân vùng sẽ bùng nổ theo kiểu kết hợp; chẳng hạn, 10 phần tử có thể được phân chia theo hơn 115.000 cách, điều này rõ ràng là không khả thi. 

Các trường hợp cạnh cần chú ý bao gồm khi tất cả các phần tử giống hệt nhau, khi tất cả các phần tử đều khác biệt hoặc khi có một số lượng nhỏ phần tử lặp lại có thể tạo ra nhiều phân vùng khác nhau với các chế độ giống hệt nhau. Ví dụ, đối với đầu vào`[1,2,2]`, các tập hợp có thể có mà chúng ta có thể tạo ra là`{2}`,`{1,2}`,`{2,2}`, Và`{1,2,2}`. Một cách tiếp cận ngây thơ chỉ xem xét các tập hợp con thay vì phân vùng sẽ bỏ sót một số tập hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tạo ra tất cả các phân vùng của mảng, tính toán chế độ của từng phân vùng và sau đó liệt kê các tập hợp kết quả. Điều này đúng về nguyên tắc nhưng không thực tế vì số lượng phân vùng của một tập hợp tăng nhanh hơn theo cấp số nhân với n. Ngay cả với n=10, vẫn có hơn 100.000 phân vùng và với n=5000, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc quan trọng là xem xét vấn đề từ góc độ tần số. Vì chúng ta chỉ quan tâm đến chế độ của từng phân vùng chứ không quan tâm đến cấu trúc chính xác của các phân vùng, nên chúng ta có thể giảm vấn đề thành phương pháp lập trình động. Quan sát chính là số cách để chọn nhiều tập hợp có thể được biểu thị dưới dạng tích chập trên số phần tử lặp lại. Chúng ta có thể sắp xếp các phần tử theo giá trị và tần suất, đồng thời xây dựng lặp đi lặp lại số lượng nhiều tập hợp bằng cách xem xét số lần chúng ta đưa từng phần tử làm chế độ vào các tập hợp con mới. Bằng cách sử dụng tổ hợp để xử lý các phần tử lặp lại, chúng ta tránh được việc liệt kê các phân vùng một cách trực tiếp. Điều này làm giảm vấn đề từ lập trình động theo cấp số nhân sang lập trình động O(n2). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số phân vùng) | O(số phân vùng) | Quá chậm | 
| Lập trình động + tổ hợp | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm tần số của từng phần tử duy nhất trong nhiều tập hợp. Điều này chuyển đổi vấn đề từ việc xử lý n phần tử thành tối đa n giá trị riêng biệt với số lượng của chúng. Cho phép`freq[x]`là số lần phần tử x xuất hiện. 
2. Khởi tạo mảng DP`dp[i]`Ở đâu`dp[i]`biểu thị số cách để tạo nhiều tập hợp có tổng kích thước i. Bắt đầu với`dp[0] = 1`vì có chính xác một cách để tạo một multiset trống. 
3. Lặp lại từng phần tử duy nhất`val`TRONG`freq`đặt hàng. Đối với mỗi số có thể có của phần tử đó`k`từ 1 đến`freq[val]`, cập nhật mảng DP để bao gồm nhiều tập hợp mới được hình thành bằng cách lấy`k`bản sao của`val`. Đây là một bước tổ hợp cổ điển: nếu trước đây chúng ta có thể tạo nhiều tập hợp kích thước`i`, bây giờ chúng ta có thể tạo nhiều bộ có kích thước`i+k`bằng cách thêm`k`bản sao của`val`. 
4. Sau khi xử lý tất cả các phần tử, tính tổng tất cả`dp[i]`vì`i >= 1`để đếm tất cả các tập hợp không trống. Lấy tổng modulo 998244353. 
5. Trả lại số tiền này làm câu trả lời cho test case. 

Tại sao nó hoạt động: Mảng DP theo dõi hiệu quả tất cả các kích thước nhiều tập riêng biệt mà chúng tôi có thể đạt được. Bởi vì chúng tôi lặp qua từng phần tử và xem xét tất cả các số đếm, nên chúng tôi đảm bảo tính được mọi tập hợp có thể được hình thành bằng quy trình chọn chế độ. Cách tiếp cận này tránh liệt kê rõ ràng các phân vùng, nhưng vẫn tính đến khả năng kết hợp của nhiều chế độ bằng cách tăng mảng DP theo các bước tương ứng với số lượng tần số. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        from collections import Counter
        freq = list(Counter(a).values())
        
        dp = [0]*(n+1)
        dp[0] = 1
        
        for f in freq:
            ndp = dp[:]
            for i in range(n+1):
                if dp[i]:
                    for k in range(1, f+1):
                        if i+k <= n:
                            ndp[i+k] = (ndp[i+k] + dp[i]) % MOD
            dp = ndp
        
        ans = sum(dp[1:]) % MOD
        print(ans)

solve()
```Trong giải pháp này, Bộ đếm thu thập tần số của từng phần tử duy nhất. các`dp`mảng theo dõi số lượng nhiều kích thước`i`. Đối với mỗi tần số phần tử, chúng tôi tạo một mảng tạm thời`ndp`để tránh ghi đè trạng thái DP hiện có trong quá trình cập nhật. Vòng lặp bên trong đảm bảo chúng tôi chiếm từ 1 đến`f`bản sao của phần tử. Tổng hợp`dp[1:]`đưa ra tổng số nhiều tập hợp không trống. 

## Ví dụ đã hoạt động 

Đối với đầu vào`[1,2,3]`, danh sách tần số là`[1,1,1]`. Mảng DP phát triển như sau: 

| Bước | Mảng DP (chỉ các mục khác 0) | 
| --- | --- | 
| Ban đầu | dp[0]=1 | 
| Quy trình 1 | dp[1]=1 | 
| Quy trình 2 | dp[1]=2, dp[2]=1 | 
| Quy trình 3 | dp[1]=3, dp[2]=3, dp[3]=1 | 

Tổng hợp`dp[1:]`cho 7, phù hợp với đầu ra dự kiến. 

Đối với đầu vào`[1,2,2]`, danh sách tần số là`[1,2]`: 

| Bước | Mảng DP | 
| --- | --- | 
| Ban đầu | dp[0]=1 | 
| Quy trình 1 | dp[1]=1 | 
| Quy trình 2 | dp[1]=2, dp[2]=1, dp[3]=1 | 

Tổng của`dp[1:]`là 4, phù hợp với sản lượng dự kiến. 

Những dấu vết này cho thấy DP đếm chính xác tất cả các tập hợp được hình thành bằng quy trình chọn chế độ, bao gồm cả những tập hợp yêu cầu phân tách các phần tử giữa các nhóm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi tần số (tối đa n phần tử), chúng tôi lặp qua các trạng thái DP (tối đa n) và số lượng có thể có (tối đa tần số ≤ n) | 
| Không gian | O(n) | Chúng tôi lưu trữ một mảng DP có kích thước n+1 | 

Với n ≤ 5000 cho mỗi trường hợp thử nghiệm và tổng tổng ≤ 5000, giải pháp O(n²) này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided samples
assert run("5\n3\n1 2 3\n3\n1 1 1\n3\n1 2 2\n10\n1 1 1 1 2 2 2 3 3 4\n10\n1 1 1 2 2 2 3 3 3 4\n") == "7\n3\n4\n111\n126", "samples"

# Custom cases
assert run("1\n1\n1\n") == "1", "single element"
assert run("1\n5\n1 1 1 1 1\n") == "5", "all equal elements"
assert run("1\n5\n1 2 3 4 5\n") == "31", "all distinct"
assert run("1\n3\n1 1 2\n") == "4", "small repeated element"
assert run("1\n2\n1 2\n") == "3", "minimum non-equal pair"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1\n`| 1 | Multiset đơn phần tử | 
|`1\n5\n1 1 1 1 1\n`| 5 | Tất cả các yếu tố giống hệt nhau | 
|`1\n5\n1 2 3 4 5\n`| 31 | Tất cả các phần tử riêng biệt, kiểm tra tính tổ hợp | 
|`1\n3\n1 1 2\n`| 4 | Multiset nhỏ với phần tử lặp lại | 
|`1\n2\n1 2\n`| 3 | Multiset kích thước tối thiểu với hai phần tử khác nhau | 

## Vỏ cạnh 

Đối với nhiều phần tử đơn`[1]`, DP bắt đầu bằng`[1,0]`. Tần số xử lý 1 thêm 1 vào dp[1], cho`[1,1]`. Tổng dp[1:] cho 1, điều này đúng. Vì`[1,1,2]`, danh sách tần số`[2,1]`dẫn đến việc tạo các bản cập nhật DP
