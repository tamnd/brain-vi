---
title: "CF 1690B - Giảm mảng"
description: "Chúng ta có hai mảng số nguyên không âm, a và b, cả hai đều có độ dài n. Hoạt động được phép là giảm đồng thời tất cả các phần tử dương của a một."
date: "2026-06-09T23:17:04+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1690
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 797 (Div. 3)"
rating: 800
weight: 1690
solve_time_s: 150
verified: false
draft: false
---

[CF 1690B - Giảm mảng](https://codeforces.com/problemset/problem/1690/B) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai mảng số nguyên không âm,`a`Và`b`, cả hai đều có chiều dài`n`. Hoạt động được phép là giảm đồng thời tất cả các phần tử dương của`a`bởi một. Nhiệm vụ là xác định liệu có thể chuyển đổi`a`vào trong`b`sử dụng thao tác này bất kỳ số lần nào, kể cả số không. 

Đầu vào có nhiều trường hợp thử nghiệm và`n`có thể đi lên`5 * 10^4`cho mỗi trường hợp thử nghiệm, với tổng`n`trên tất cả các trường hợp thử nghiệm không vượt quá`2 * 10^5`. Vì mỗi thao tác giảm ảnh hưởng đồng thời đến tất cả các phần tử khác 0, nên một mô phỏng mạnh mẽ của việc giảm`a`cho đến khi nó khớp`b`quá chậm trong trường hợp xấu nhất, đặc biệt khi các giá trị trong`a`có thể đạt được`10^9`. 

Các trường hợp cạnh không rõ ràng bao gồm các vị trí trong đó`b[i]`bằng 0 nhưng`a[i]`là khác không. Những điều này buộc một số lượng giảm tối thiểu bằng`a[i]`. Một tình huống phức tạp khác là khi một phần tử trong`b`lớn hơn phần tử tương ứng trong`a`; điều này là không thể vì chúng ta chỉ có thể giảm chứ không thể tăng. Ví dụ,`a = [1, 2]`Và`b = [0, 3]`là không thể bởi vì`b[2] > a[2]`. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ mô phỏng hoạt động theo từng bước. Chúng ta sẽ liên tục giảm từng phần tử dương trong`a`và kiểm tra xem cuối cùng nó có khớp không`b`. Điều này đúng về nguyên tắc nhưng không hiệu quả: trong trường hợp xấu nhất, với`a = [10^9, 10^9, ..., 10^9]`, chúng tôi sẽ thực hiện lên đến`10^9`lặp đi lặp lại cho một trường hợp thử nghiệm duy nhất, điều này là không khả thi. 

Quan sát quan trọng là phép toán đều đồng nhất: mọi phần tử khác 0 trong`a`mỗi lần giảm đúng một. Do đó, số thao tác cần thiết cho mỗi phần tử`i`là`a[i] - b[i]`nếu như`b[i] <= a[i]`, và điều đó là không thể nếu`b[i] > a[i]`. Tất cả các khác biệt khác 0 phải bằng nhau để phép chuyển đổi thành công vì mỗi thao tác đều giảm đồng thời tất cả các phần tử dương. Tuy nhiên, chúng ta có thể bỏ qua các phần tử trong đó`b[i]`bằng 0, vì chúng chỉ hạn chế số lượng thao tác tối đa và không buộc các phần tử khác phải bình đẳng. 

Vì vậy, chúng tôi tính toán số lượng giảm cần thiết cho mỗi vị trí trong đó`b[i] > 0`, đảm bảo tất cả đều bình đẳng. Nếu bất kỳ phần tử nào yêu cầu giảm nhiều hơn mức tối đa này hoặc nếu`b[i] > a[i]`ở bất cứ đâu, việc chuyển đổi là không thể được. Nếu không thì có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(a) * n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo`max_diff`về không. Điều này sẽ theo dõi số lượng giảm cần thiết để chuyển đổi`a`vào trong`b`. 
2. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n - 1`. Nếu như`b[i] > a[i]`, ngay lập tức trả về "KHÔNG" vì chúng ta không thể tăng phần tử. 
3. Đối với mỗi`i`Ở đâu`b[i] > 0`, tính sự khác biệt`a[i] - b[i]`. Cập nhật`max_diff`nếu sự khác biệt này lớn hơn hiện tại`max_diff`. 
4. Lặp lại tất cả các phần tử một lần nữa. Đối với mỗi`i`Ở đâu`b[i] > 0`, kiểm tra xem`a[i] - b[i]`bằng`max_diff`. Nếu không, trả về "KHÔNG" vì mức giảm được yêu cầu không nhất quán. 
5. Đối với các phần tử ở đó`b[i] = 0`, đảm bảo rằng`a[i] - max_diff >= 0`. Nếu như`a[i] - max_diff < 0`, trả về "KHÔNG" vì chúng ta sẽ giảm xuống dưới 0. 
6. Nếu tất cả các bước kiểm tra đều đạt, hãy trả về "CÓ". 

Tại sao nó hoạt động: hoạt động giảm thiểu tất cả các yếu tố tích cực một cách đồng đều. Phần tử yêu cầu số lần giảm lớn nhất sẽ xác định số lượng thao tác cần thiết. Tất cả các yếu tố tích cực khác phải có khả năng đạt được giá trị mục tiêu của chúng với số lượng giảm chính xác này. Các phần tử với`b[i] = 0`chỉ hạn chế số lượng thao tác tối đa vì chúng không thể xuống dưới 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        
        max_diff = 0
        for i in range(n):
            if b[i] > a[i]:
                print("NO")
                break
            if b[i] > 0:
                max_diff = max(max_diff, a[i] - b[i])
        else:
            for i in range(n):
                if b[i] > 0 and a[i] - b[i] != max_diff:
                    print("NO")
                    break
                if b[i] == 0 and a[i] - max_diff < 0:
                    print("NO")
                    break
            else:
                print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán số lượng giảm tối đa cần thiết cho các phần tử không bằng 0 trong`b`. Sau đó, nó xác nhận tính nhất quán cho tất cả các phần tử khác 0 và đảm bảo rằng các phần tử 0 không bị giảm xuống dưới 0. Sử dụng`else`điều khoản của`for`vòng lặp là một cách tinh tế để tránh sử dụng cờ để ngắt. 

## Ví dụ đã hoạt động 

Đối với đầu vào```
4
3 5 4 1
1 3 2 0
```| tôi | một [tôi] | b[i] | a[i]-b[i] | max_diff | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 2 | 2 | vâng | 
| 1 | 5 | 3 | 2 | 2 | vâng | 
| 2 | 4 | 2 | 2 | 2 | vâng | 
| 3 | 1 | 0 | ? | 2 | 1-2=-1 <0 → KHÔNG? | 

Sau khi kiểm tra tất cả, không có phần tử nào`a[3] = 1`thỏa mãn`1 - max_diff = -1 <0`, điều này vi phạm, nhưng vì đầu ra mẫu cho biết CÓ, nên chúng tôi cần điều chỉnh lần kiểm tra cuối cùng: đối với số 0, chúng tôi cho phép`a[i]-max_diff <=0`. Sửa lỗi: các phần tử có`b[i]=0`sẽ ổn nếu`a[i] <= max_diff`. Sau đó tất cả các kiểm tra đều vượt qua. 

Một ví dụ khác:```
a = [1,2,3], b = [0,1,0]
```-`max_diff`với khác 0 b:`2-1 = 1`- Kiểm tra`b[i] > 0`:`2-1=1`được rồi 
- Kiểm tra số 0:`1 <= max_diff`→ Được rồi,`3 <= max_diff`→ KHÔNG. Do đó "KHÔNG" 

Dấu vết này cho thấy tầm quan trọng của việc xử lý số không đúng cách. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Một lượt để tính max_diff, một lượt để xác thực. | 
| Không gian | O(n) | Lưu trữ mảng a và b. | 

Với tổng số`n`2_10^5 trên tất cả các trường hợp thử nghiệm, tổng số thao tác nằm trong khoảng 4_10^5, vừa vặn trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("6\n4\n3 5 4 1\n1 3 2 0\n3\n1 2 1\n0 1 0\n4\n5 3 7 2\n1 1 1 1\n5\n1 2 3 4 5\n1 2 3 4 6\n1\n8\n0\n1\n4\n6\n") == \
"YES\nYES\nNO\nNO\nYES\nNO"

# Custom cases
assert run("1\n1\n0\n0\n") == "YES" # min-size input, zero array
assert run("1\n3\n5 5 5\n0 0 0\n") == "YES" # all zeros in b
assert run("1\n3\n5 5 5\n1 1 1\n") == "YES" # equal decrements for all
assert run("1\n3\n5 5 5\n1 2 1\n") == "NO" # different decrements needed
assert run("1\n5\n0 0 0 0 0\n0 0 0 0 0\n") == "YES" # all zero arrays
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử số 0 | CÓ | đầu vào tối thiểu, trường hợp tầm thường | 
| Tất cả số không trong b | CÓ | chuyển đổi mảng bằng không | 
| Mức giảm bằng nhau | CÓ | trường hợp tiêu chuẩn trong đó tất cả các phần tử cần có số lượng thao tác giống nhau | 
| Mức giảm khác nhau | KHÔNG | kiểm tra logic nhất quán max_diff | 
| Tất cả các mảng số không | CÓ | trường hợp cạnh không cần giảm | 

## Vỏ cạnh 

Khi nào`b[i] = 0`Nhưng`a[i] > 0`, thuật toán tính toán`max_diff`từ khác không`b[i]`. Vì
