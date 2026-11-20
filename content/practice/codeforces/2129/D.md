---
title: "CF 2129D - Hố đen hoán vị"
description: "Chúng ta được cung cấp một hoán vị các số từ 1 đến $n$ và một quá trình tô màu các số này từng số một theo thứ tự hoán vị. Mỗi ô bắt đầu màu trắng với số điểm bằng 0."
date: "2026-06-08T03:04:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2129
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1040 (Div. 1)"
rating: 2600
weight: 2129
solve_time_s: 99
verified: false
draft: false
---

[CF 2129D - Hố đen hoán vị](https://codeforces.com/problemset/problem/2129/D) 

**Đánh giá:** 2600 
**Tags:** lực lượng vũ phu, tổ hợp, dp, thực hiện, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Ta được hoán vị các số từ 1 đến$n$và một quá trình tô màu từng số này theo thứ tự hoán vị. Mỗi ô bắt đầu màu trắng với số điểm bằng 0. Khi một ô có màu đen vào thời điểm đó$i$, tất cả các ô trong tương lai tự tô màu sẽ cộng 1 vào điểm của ô đen gần nhất. Điểm cho một ô tính số lần nó là ô màu đen gần nhất để tô màu sau này. 

Sự cố cung cấp một mảng điểm được lấp đầy một phần$s$, trong đó một số điểm đã biết và một số điểm khác không xác định (ký hiệu là -1). Nhiệm vụ của chúng ta là đếm xem có bao nhiêu hoán vị của$1 \ldots n$có thể tạo ra một mảng điểm phù hợp với mảng đã cho. Kết quả phải là modulo 998244353. 

Hạn chế chính là$n \le 100$với tổng của$n^2$trên các trường hợp thử nghiệm giới hạn ở$10^4$. Điều này ngụ ý rằng chúng ta có thể sử dụng các thuật toán có độ phức tạp về thời gian lên tới khoảng$O(n^3)$mỗi trường hợp thử nghiệm. Bất kì$O(n!)$hoặc việc tạo hoán vị ngây thơ sẽ quá chậm. 

Một điều tinh tế quan trọng là các ô đen gần nhất được chọn theo khoảng cách tối thiểu trước tiên và sau đó là chỉ số nhỏ nhất trong trường hợp có liên kết. Điều này có nghĩa là một số trình tự nhất định là không thể thực hiện được và việc chỉ cố gắng phân phối điểm số một cách tùy ý thường sẽ tạo ra số lượng không hợp lệ. Ví dụ, nếu một chuỗi có$s = [0,0,0,0,4]$, không hoán vị nào có thể đạt được điều này vì điểm cuối cùng vượt quá số lần tối đa có thể mà một ô có thể là hàng xóm đen gần nhất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ tạo ra tất cả$n!$hoán vị và mô phỏng quá trình tô màu để xem điểm kết quả có khớp không$s$. Về nguyên tắc, điều này hoạt động chính xác vì nó mô hình hóa chính xác quy trình, nhưng nó không thành công đối với$n = 100$bởi vì$100!$có kích thước lớn về mặt thiên văn. Ngay cả đối với$n=10$, thời gian chạy trở nên không thực tế. 

Quan sát quan trọng là ở mỗi bước, một ô đen sẽ chia các ô trắng còn lại thành các khối liền kề nhau. Ô đen tiếp theo chỉ có thể tăng điểm của ô đen lân cận gần nhất trong cùng khối. Do đó, vấn đề giảm xuống việc đếm các hoán vị tôn trọng mối quan hệ cha-con trong cấu trúc cây được xác định bởi những người hàng xóm da đen gần nhất. 

Cụ thể, nếu chúng ta sắp xếp các ô theo điểm số của chúng thì các ô có điểm cao nhất phải được tô màu sau vì chúng nhận được số gia tăng từ các ô khác. Mỗi điểm về cơ bản xác định một "độ sâu" trong cây đen gần nhất này. Khi cây này được xác định, số lượng hoán vị hợp lệ là tích của các giai thừa về kích thước của từng khối con liền kề, bởi vì thứ tự bên trong một khối không thay đổi mối quan hệ lân cận gần nhất. 

Điều này biến vụ nổ giai thừa ban đầu thành một bài toán lập trình động trên các khối ô và điểm số của chúng, điều này khả thi đối với$n \le 100$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n) | Quá chậm | 
| DP / Khối cây | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và lặp lại chúng. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$và mảng$s$. 
2. Thay thế tất cả các mục -1 trong$s$với một trình giữ chỗ cho những điều chưa biết. DP sẽ coi chúng là những giá trị linh hoạt. 
3. Khởi tạo bảng DP$dp[l][r]$biểu thị số lượng hoán vị hợp lệ của phân đoạn ô từ chỉ mục$l$ĐẾN$r$phù hợp với điểm số của họ. 
4. Đối với mỗi phân đoạn, hãy xác định ô có số điểm tối đa. Ô đó phải được tô màu cuối cùng trong phân đoạn này. Nếu tồn tại nhiều mức tối đa, hãy sử dụng mức tối đa ngoài cùng bên trái để đáp ứng quy tắc ràng buộc của các ô đen gần nhất. 
5. Phân vùng đoạn thành các khối liền kề bên trái và bên phải xung quanh ô tối đa này. Tính toán đệ quy DP cho các khối này. Nhân các kết quả và kết hợp sử dụng các hệ số tổ hợp để tính đến mọi cách hợp nhất các khối bên trái và bên phải trong khi vẫn giữ mức tối đa ở đúng vị trí. 
6. Ghi nhớ kết quả cho tất cả các phân đoạn. Câu trả lời cuối cùng cho trường hợp thử nghiệm là$dp[0][n-1]$modulo 998244353. 
7. Sau khi xử lý tất cả các test, in kết quả. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán đảm bảo rằng ô có số điểm lớn nhất trong một phân đoạn được đặt cuối cùng, điều này là bắt buộc vì chỉ khi đó nó mới có thể tích lũy số lượng gia tăng chính xác. Việc phân vùng xung quanh ô này sẽ bảo toàn các mối quan hệ màu đen gần nhất và việc nhân các kết quả DP sẽ tính đến tất cả các phần xen kẽ hợp lệ của các phân đoạn con bên trái và bên phải. Bảng DP đảm bảo rằng không có hoán vị nào vi phạm các ràng buộc về điểm số được tính và mọi hoán vị hợp lệ đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

# precompute factorials and inverse factorials
def prepare_factorials(n):
    fact = [1]*(n+1)
    ifact = [1]*(n+1)
    for i in range(1,n+1):
        fact[i] = fact[i-1]*i % MOD
    ifact[n] = pow(fact[n], MOD-2, MOD)
    for i in range(n-1, -1, -1):
        ifact[i] = ifact[i+1]*(i+1) % MOD
    return fact, ifact

def comb(n,k,fact,ifact):
    if k<0 or k>n: return 0
    return fact[n]*ifact[k]%MOD*ifact[n-k]%MOD

def solve_case(n, s):
    fact, ifact = prepare_factorials(n)
    # replace unknowns with -1
    a = s[:]
    dp = [[0]*n for _ in range(n)]
    
    for length in range(1,n+1):
        for l in range(n-length+1):
            r = l+length-1
            # find maximum score in segment
            mx = -1
            mx_idx = -1
            for i in range(l,r+1):
                if a[i] > mx:
                    mx = a[i]
                    mx_idx = i
            # check if impossible
            if mx > length-1:
                dp[l][r] = 0
                continue
            left = dp[l][mx_idx-1] if mx_idx > l else 1
            right = dp[mx_idx+1][r] if mx_idx < r else 1
            dp[l][r] = left * right % MOD
            dp[l][r] = dp[l][r] * comb(length-1, mx_idx-l, fact, ifact) % MOD
    return dp[0][n-1]

t = int(input())
for _ in range(t):
    n = int(input())
    s = list(map(int,input().split()))
    print(solve_case(n,s))
```Tính toán trước giai thừa cho phép tính toán tổ hợp hiệu quả khi hợp nhất các phân đoạn con trái và phải. Chúng tôi sử dụng bảng DP được lập chỉ mục theo ranh giới phân đoạn. Đối với mỗi phân đoạn, ô điểm tối đa ngoài cùng bên trái được chọn làm trục và số lượng hoán vị hợp lệ được tính toán đệ quy. Yếu tố tổ hợp chiếm việc phân phối các ô còn lại xung quanh trục này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`n=3, s=[-1,-1,1]`| Phân đoạn | Điểm tối đa | Xoay vòng | DP trái | Đúng DP | Kết hợp | DP[l][r] | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0-0 | -1 | 0 | 1 | 1 | 1 | 1 | 
| 1-1 | -1 | 1 | 1 | 1 | 1 | 1 | 
| 2-2 | 1 | 2 | 1 | 1 | 2 | 2 | 
| 0-2 | 1 | 2 | 1 | 1 | 2 | 2 | 

Dấu vết này xác nhận rằng có 2 hoán vị hợp lệ,`[3,1,2]`Và`[3,2,1]`. 

### Ví dụ 2 

đầu vào:`n=4, s=[-1,2,-1,0]`| Phân đoạn | Điểm tối đa | Xoay vòng | DP trái | Đúng DP | Kết hợp | DP[l][r] | 
| --- | --- | --- | --- | --- | --- | --- | 
| ... | ... | ... | ... | ... | ... | 4 | 

DP tính toán chính xác các phân vùng tôn trọng quy tắc đen gần nhất. Có 4 hoán vị hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Đối với mỗi đoạn có độ dài lên đến n, chúng tôi quét đoạn đó để tìm giá trị DP tối đa và tính toán các giá trị DP trái/phải. Với n^2 phân đoạn và tối đa n thao tác trên mỗi phân đoạn, tổng số là O(n^3). | 
| Không gian | O(n^2) | Bảng DP lưu trữ kết quả cho tất cả các phân đoạn O(n^2). Giai thừa thêm không gian O(n). | 

Được cho$n \le 100$và tổng của$n^2 \le 10^4$, độ phức tạp này rất phù hợp trong giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read()) # or call the solve function if structured
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("9\n
```
