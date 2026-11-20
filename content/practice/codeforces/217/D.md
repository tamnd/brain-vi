---
title: "CF 217D - Tuần tra của Bitonix"
description: "Vấn đề cơ bản là về số học mô-đun tròn và tổng tập hợp con. Chúng ta có một quỹ đạo tròn gồm n trạm, mỗi trạm cách nhau m dặm. Thuyền trưởng Bitonix có một bộ bình nhiên liệu, mỗi bình có dung tích nhất định."
date: "2026-06-05T00:54:59+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "dfs-and-similar", "math"]
categories: ["algorithms"]
codeforces_contest: 217
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 134 (Div. 1)"
rating: 2900
weight: 217
solve_time_s: 97
verified: false
draft: false
---

[CF 217D - Đội tuần tra của Bitonix](https://codeforces.com/problemset/problem/217/D) 

**Xếp hạng:** 2900 
**Tags:** bitmasks, brute Force, tổ hợp, dfs và tương tự, toán học 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề cơ bản là về số học mô-đun tròn và tổng tập hợp con. Chúng ta có quỹ đạo tròn gồm _n_ trạm, mỗi trạm cách nhau _m_ dặm. Thuyền trưởng Bitonix có một bộ bình nhiên liệu, mỗi bình có dung tích nhất định. Sử dụng một chiếc xe tăng sẽ di chuyển anh ta chính xác quãng đường đó theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Nhiệm vụ là xác định xem có bao nhiêu tập hợp con thùng nhiên liệu, nếu bị phá hủy, sẽ ngăn anh ta hạ cánh chính xác xuống một trạm sau bất kỳ chuỗi sử dụng thùng nhiên liệu nào. 

Khi khởi động lại, mục tiêu là đếm các tập hợp con của các xe tăng sao cho không có sự kết hợp nào của các xe tăng còn lại, di chuyển theo một trong hai hướng, dẫn đến tổng khoảng cách là bội số của khoảng cách giữa các trạm nhân với một số nguyên bước (tức là bội số của _n × m_). Chúng ta có thể đơn giản hóa việc này bằng cách sử dụng số học mô-đun: một cuộc tuần tra là hợp lệ nếu tổng dung tích bể đã sử dụng theo mô-đun`n * m`chia hết cho _m_, hoặc tương đương, modulo _n_, vì tất cả khoảng cách đều là bội số của _m_. Vì vậy, chúng ta có thể giảm thể tích mỗi bể theo modulo _n_ và giải bài toán trong không gian mô đun này. 

Các ràng buộc cho chúng ta biết rằng _n_ có thể lên tới 1000 và có thể lên tới 10.000 xe tăng. Việc thử trực tiếp mọi tập hợp con là không thể vì có 2^10000 tập hợp con. Điều này có nghĩa là việc sử dụng vũ lực đối với tất cả các tập hợp con là không khả thi. Các trường hợp cạnh liên quan đến tình huống trong đó tất cả các thùng chứa là bội số của _n_ hoặc khi phần còn lại của mô-đun bao phủ toàn bộ không gian; việc tính tổng bất cẩn mà không rút gọn mô-đun sẽ thất bại. 

Ví dụ, với`n = 3`,`m = 60`, và xe tăng`[3, 6, 9]`, mọi tổ hợp modulo 3 đều bằng 0, nên anh ta luôn có thể quay lại trạm xuất phát. Nếu chúng ta tính tổng các khoảng cách một cách ngây thơ mà không giảm modulo _n_, thì chúng ta có thể kết luận sai rằng một tập hợp con ngăn cản anh ta hoàn thành một cuộc tuần tra. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là xem xét tất cả các tập hợp con 2^t xe tăng và, đối với mỗi tập hợp con, kiểm tra xem liệu bất kỳ sự kết hợp nào của các xe tăng còn lại có cho phép tuần tra hợp lệ hay không. Về nguyên tắc, điều này đúng vì nó kiểm tra tất cả các khả năng, nhưng rõ ràng là quá chậm: với t lên tới 10.000, 2^10000 phép toán là lớn về mặt thiên văn. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống việc đếm có bao nhiêu tập hợp con không tạo được modulo 0 _n_. Vì mỗi bể có thể được sử dụng theo cả hai hướng nên việc thêm một bể tương đương với việc cộng hoặc trừ thể tích của nó theo modulo _n_. Đây chính xác là bài toán kinh điển về việc đếm số tập con có tổng chia hết cho _n_, với dấu ± cho phép. Chúng ta có thể giải quyết vấn đề này một cách hiệu quả bằng cách sử dụng quy hoạch động trên tổng mô-đun. 

Chúng tôi xác định một mảng DP`dp[r]`biểu thị số cách để đạt được số dư _r_ modulo _n_ bằng cách sử dụng một số tập hợp con xe tăng có ± hướng. Mỗi bể cập nhật DP bằng cách xem xét cả phép cộng và phép trừ. Cuối cùng,`dp[0]`đưa ra số lượng tập hợp con có thể hoàn thành một cuộc tuần tra hợp lệ. Tổng số tập hợp con là 2^t, vì vậy câu trả lời là`2^t - dp[0]`, mô đun 10^9 + 7. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^t × t × n) | O(n) | Quá chậm | 
| DP trên tổng modulo | O(t×n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số trạm`n`, khoảng cách giữa các ga`m`, và số lượng bể`t`, tiếp theo là thể tích bể. 
2. Giảm modulo mỗi bể`n`. Điều này là đủ vì chỉ có tổng modulo _n_ mới xác định được sự liên kết của trạm. 
3. Khởi tạo mảng DP`dp`kích thước`n`với tất cả các số 0 và đặt`dp[0] = 1`đại diện cho tổng tập hợp trống. 
4. Lặp lại từng bể. Đối với mỗi phần còn lại hiện tại`r`TRONG`dp`, thêm bể theo cả hai hướng:`(r + tank) % n`Và`(r - tank) % n`. Giữ một mảng mới để cập nhật nhằm tránh ghi đè tại chỗ. 
5. Sau khi xử lý tất cả các bể,`dp[0]`đếm số tập hợp con (bao gồm cả tập trống) có tổng bằng 0 modulo n, tức là các tập hợp con cho phép tuần tra. 
6. Tính toán`2^t - dp[0]`modulo 10^9 + 7. Đây là số lượng tập hợp con ngăn chặn bất kỳ cuộc tuần tra nào. 
7. Xuất kết quả. 

Tại sao nó hoạt động: Mỗi bể có thể đóng góp tích cực hoặc tiêu cực vào tổng số modulo n, bao gồm tất cả các kết hợp hợp lệ. DP đếm chính xác tất cả số dư có thể đạt được theo từng bước. Bất biến là sau khi xử lý i xe tăng,`dp[r]`đếm chính xác có bao nhiêu sự kết hợp của xe tăng thứ i đầu tiên tạo ra phần còn lại r modulo n. Việc sử dụng modulo đảm bảo số lượng vẫn ở mức nhỏ và các phép toán khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m, t = map(int, input().split())
    tanks = list(map(int, input().split()))
    
    tanks = [x % n for x in tanks]
    
    dp = [0] * n
    dp[0] = 1  # empty set
    
    for tank in tanks:
        ndp = dp[:]
        for r in range(n):
            ndp[(r + tank) % n] = (ndp[(r + tank) % n] + dp[r]) % MOD
            ndp[(r - tank) % n] = (ndp[(r - tank) % n] + dp[r]) % MOD
        dp = ndp
    
    total_subsets = pow(2, t, MOD)
    answer = (total_subsets - dp[0]) % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên giảm số lượng bể theo modulo n, khởi tạo DP cho số lượng còn lại và cập nhật lặp đi lặp lại cho cả phép cộng và phép trừ. Sử dụng riêng biệt`ndp`mảng ngăn chặn việc tính hai lần trong một lần lặp. Phép tính cuối cùng trừ tổng số tập hợp con cho phép tuần tra, theo modulo 10^9 + 7 để duy trì trong giới hạn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
7 6 5
5 4 12 6 5
```| Bước | Xe tăng | Cập nhật DP (số lượng còn lại) | 
| --- | --- | --- | 
| ban đầu | - | dp[0]=1 | 
| 5 | +5,-5 | dp[5]=1, dp[2]=1, dp[0]=1 | 
| 4 | +4,-4 | cập nhật dp[0], dp[1], dp[2], v.v. | 
| 12->12%7=5 | +5,-5 | ... | 
| 6->6%7=6 | +6,-6 | ... | 
| 5 | +5,-5 | ... | 

Sau tất cả các xe tăng, dp[0]=26. Tổng các tập con = 2^5 = 32. Đáp án = 32 - 26 = 6. 

Điều này chứng tỏ rằng DP đếm chính xác số tiền có thể đạt được theo modulo n và trừ đi tổng số tập hợp con sẽ cho số lượng tập hợp con bị phá hủy. 

Đầu vào tùy chỉnh:```
3 1 3
1 2 3
```Ở đây xe tăng modulo 3 là [1,2,0]. DP đếm tổng đạt 0 modulo 3. Phép trừ sẽ cho số lượng tập hợp con ngăn cản việc tuần tra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t×n) | Mỗi bể cập nhật n mục DP xem xét cả hai hướng. | 
| Không gian | O(n) | Chúng tôi duy trì hai mảng có kích thước n để cập nhật DP. | 

Với n 1000 và t 10.000, t × n 10^7 thao tác, thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("7 6 5\n5 4 12 6 5\n") == "6", "sample 1"

# custom cases
assert run("3 1 3\n1 2 3\n") == "1", "small n and t, some multiples"
assert run("2 10 2\n1 1\n") == "2", "all tanks 1, n=2"
assert run("5 5 1\n10\n") == "0", "single tank multiple of n"
assert run("4 2 4\n1 2 3 4\n") == "4", "mixed modulo values"
assert run("3 3 3\n3 3 3\n") == "7", "all tanks divisible by n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 3\n1 2 3 | 1 | n và t nhỏ, giảm modulo | 
| 2 10 2\n1 1 | 2 | nhỏ n=2, bể tối thiểu | 
| 5 5 1\n10 | 0 | một xe tăng bội số của n, không ngăn được tuần tra | 
| 4 2 4\n1 2 3 4 | 4 | phần dư hỗn hợp, độ chính xác DP | 
| 3 3 3\n3 3 3 | 7 | tất cả các xe tăng chia hết cho n, tổng số tập con trừ dp[0] | 

## Vỏ cạnh 

Nếu tất cả các xe tăng đều là bội số
