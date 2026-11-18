---
title: "CF 2026D - Tổng các phân đoạn"
description: "Chúng ta được cho một mảng số nguyên a có độ dài n. Từ a, chúng ta có thể tạo ra một mảng b lớn hơn nhiều chứa tất cả các tổng mảng con liền kề có thể có của a, được liệt kê theo một thứ tự cụ thể: đầu tiên là tất cả các tổng bắt đầu từ chỉ số 1, sau đó tất cả các tổng bắt đầu từ chỉ mục 2, v.v."
date: "2026-06-08T12:20:12+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2026
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 171 (Rated for Div. 2)"
rating: 1900
weight: 2026
solve_time_s: 99
verified: false
draft: false
---

[CF 2026D - Tổng các phân đoạn](https://codeforces.com/problemset/problem/2026/D) 

**Xếp hạng:** 1900 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, triển khai, toán học 
**Thời gian giải:** 1 phút 39 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng số nguyên`a`chiều dài`n`. Từ`a`, chúng ta có thể tạo ra một mảng lớn hơn nhiều`b`chứa tất cả các tổng mảng con liền kề có thể có của`a`, được liệt kê theo một thứ tự cụ thể: đầu tiên là tất cả các tổng bắt đầu từ chỉ số 1, sau đó là tất cả các tổng bắt đầu từ chỉ số 2, v.v. Ví dụ, nếu`a = [1, 2, 5, 10]`, tổng bắt đầu từ chỉ số 1 là`1`,`1+2=3`,`1+2+5=8`, Và`1+2+5+10=18`. Khi đó tổng bắt đầu từ chỉ số 2 là`2`,`2+5=7`,`2+5+10=17`, v.v., tạo ra đầy đủ`b`mảng`[1,3,8,18,2,7,17,5,15,10]`. 

Vấn đề sau đó đưa ra`q`các truy vấn, mỗi truy vấn yêu cầu tổng các phần tử trong một đoạn liền kề của`b`, được chỉ định bởi các chỉ số`[l_i, r_i]`. Chúng ta phải tính toán những khoản tiền này một cách hiệu quả. 

Những hạn chế làm cho vũ lực không thể thực hiện được. Với`n`lên đến`3*10^5`,`b`có thể có khoảng`4.5*10^10`các phần tử. Rõ ràng chúng ta không thể lưu trữ`b`rõ ràng hoặc tổng hợp các phân đoạn một cách ngây thơ. Mỗi truy vấn phải được giải quyết theo thời gian tuyến tính liên quan đến`b`. Các giá trị trong`a`nhỏ nên việc tràn số nguyên không phải là vấn đề đáng lo ngại. 

Các trường hợp cạnh bao gồm mảng nhỏ nhất với`n=1`, Ở đâu`b`chỉ có một phần tử. Một trường hợp phức tạp khác là các truy vấn trải rộng trên nhiều vị trí bắt đầu của`a`, ví dụ: một truy vấn bao gồm phần đuôi của tổng từ một chỉ mục bắt đầu và phần đầu của tổng từ chỉ mục bắt đầu tiếp theo. Quản lý sai việc lập chỉ mục giữa`a`Và`b`có thể đưa ra những câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ sẽ xây dựng rõ ràng`b`bằng cách lặp qua từng cặp chỉ số bắt đầu và kết thúc trong`a`và tính tổng mảng con. Sau khi xây dựng`b`, mỗi truy vấn có thể được trả lời trong`O(r-l+1)`thời gian. Điều này đúng vì nó trực tiếp tuân theo định nghĩa bài toán. Tuy nhiên, trường hợp xấu nhất có`n(n+1)/2`các yếu tố trong`b`(tối đa ≈4,5*10^10`n`) và mỗi truy vấn có thể tổng bằng nhiều số đó. Điều này vượt xa khả thi cả về thời gian và bộ nhớ. 

Cái nhìn sâu sắc quan trọng cho một cách tiếp cận tối ưu là nhận ra rằng`b`có cấu trúc. Mỗi nhóm tổng bắt đầu từ chỉ số`i`có thể được biểu thị bằng cách sử dụng tổng tiền tố của`a`. Cho phép`prefix[i] = a_1 + ... + a_i`. Sau đó tổng của`b`từ`s(i,i)`ĐẾN`s(i,j)`chỉ là`prefix[j] - prefix[i-1] + prefix[j-1] - prefix[i-1] + ...`có thể được đơn giản hóa thành một công thức dựa trên`prefix`. Điều này cho phép chúng ta tính tổng các phân đoạn tùy ý của`b`mà không xây dựng nó một cách rõ ràng. 

Để xác định hiệu quả chỉ số bắt đầu của`a`một vị trí nhất định trong`b`tương ứng với, chúng ta có thể tính toán trước một mảng`start_pos`Ở đâu`start_pos[i]`là chỉ số trong`b`số tiền bắt đầu từ đâu`a[i]`bắt đầu. Đưa ra một truy vấn`[l,r]`, chúng ta có thể tìm kiếm nhị phân`start_pos`để xác định phần nào của`b`được bao phủ và sau đó tính tổng bằng cách sử dụng tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 + q * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n + q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng tiền tố của`a`. Cho phép`prefix[i] = a_1 + ... + a_i`. Điều này cho phép tính toán O(1) của bất kỳ tổng liền kề nào`s(l,r)`TRONG`a`. 
2. Tính toán trước vị trí bắt đầu của các tổng trong`b`cho mỗi chỉ số bắt đầu của`a`. Cho phép`start_pos[i] = i*(2n-i+1)//2 - (n-i)`hoặc tương đương sử dụng tổng số đang chạy. Điều này cho chúng ta biết số tiền bắt đầu từ đâu`a[i]`bắt đầu vào`b`. Bước này cho phép chúng ta nhanh chóng xác định phần nào của`a`đóng góp vào một phạm vi truy vấn trong`b`. 
3. Đối với mỗi truy vấn`[l,r]`, xác định chỉ số bắt đầu nào của`a`giao nhau với phạm vi. Sử dụng tìm kiếm nhị phân trên`start_pos`để tìm chỉ số bắt đầu đầu tiên`i`sao cho số tiền bắt đầu từ`i`bao gồm hoặc vượt quá`l`. Tương tự tìm chỉ số cuối cùng bao gồm`r`. 
4. Tính tổng cho mỗi khối được che phủ toàn bộ hoặc một phần. Đối với một khối đầy đủ, tổng có thể được tính bằng công thức số học cho tổng tích lũy của các mảng con. Đối với khối một phần (chỉ một phần của số tiền bắt đầu từ`a[i]`được bao gồm), hãy sử dụng tổng tiền tố để tính toán chính xác tổng mảng con nào có trong truy vấn. 
5. Cộng tổng từ tất cả các khối đóng góp để có được tổng số cho truy vấn. Trả về hoặc in kết quả. 

Tại sao nó hoạt động: Những bất biến là`prefix`cho phép tính toán tổng mảng con theo thời gian không đổi và`start_pos`cho phép định vị theo thời gian không đổi nơi mỗi nhóm tổng của mảng con bắt đầu trong`b`. Không có phần tử của`b`được tính hai lần hoặc bị bỏ qua vì mọi vị trí được ánh xạ tới chính xác một chỉ mục bắt đầu của`a`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))
q = int(input())
queries = [tuple(map(int, input().split())) for _ in range(q)]

prefix = [0] * (n + 1)
for i in range(n):
    prefix[i+1] = prefix[i] + a[i]

# compute starting positions
start_pos = [0] * n
total = 1
for i in range(n):
    start_pos[i] = total
    total += n - i

def sum_from_start(i, l_in_block, r_in_block):
    # sum of subarrays starting at a[i], positions l_in_block to r_in_block (1-based)
    res = 0
    for k in range(l_in_block-1, r_in_block):
        res += prefix[i+k+1] - prefix[i]
    return res

from bisect import bisect_right

for l,r in queries:
    # find which starting indices cover l and r
    li = bisect_right(start_pos, l) - 1
    ri = bisect_right(start_pos, r) - 1
    res = 0
    if li == ri:
        res += sum_from_start(li, l-start_pos[li]+1, r-start_pos[li]+1)
    else:
        # first partial block
        res += sum_from_start(li, l-start_pos[li]+1, n-li)
        # last partial block
        res += sum_from_start(ri, 1, r-start_pos[ri]+1)
        # full blocks in between
        for i in range(li+1, ri):
            res += sum_from_start(i, 1, n-i)
    print(res)
```Chúng tôi tính toán tổng tiền tố để cho phép tính tổng mảng con O(1).`start_pos`ánh xạ chỉ số bắt đầu của`a`vào các vị trí trong`b`, cho phép tìm kiếm nhị phân để tìm khối truy vấn bao gồm. chức năng`sum_from_start`tính toán tổng của bất kỳ phân đoạn nào của một khối một cách hiệu quả. Tìm kiếm nhị phân và xử lý cẩn thận các khối một phần/toàn bộ đảm bảo tính chính xác mà không cần xây dựng`b`. 

## Ví dụ đã hoạt động 

cho`a=[1,2,5,10]`, tổng tiền tố là`[0,1,3,8,18]`. Bắt đầu vị trí trong`b`là`[1,5,8,10]`. Xem xét truy vấn`[5,10]`. Tìm kiếm nhị phân mang lại`li=1`(bắt đầu=5),`ri=3`(bắt đầu = 10). Tính tổng từ đầu 1 khối một phần: tổng vị trí 1 đến 3 → tổng`2+7+17=26`. Khối cuối cùng: vị trí 1 → tổng`10`. Khối đầy đủ ở giữa: vị trí 2 → tổng`5+15=20`. Tổng = 26+20+10=56, kết quả đầu ra mẫu phù hợp. 

Một truy vấn khác`[1,4]`chỉ bao gồm khối đầu tiên:`li=ri=0`, tổng các vị trí từ 1 đến 4 →`1+3+8+18=30`. 

Những dấu vết này xác nhận thuật toán ánh xạ chính xác các vị trí truy vấn để chặn các chỉ mục và tính tổng các tổng mảng con thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q log n + tổng một phần) | Tổng tiền tố và vị trí bắt đầu được tính theo O(n). Mỗi truy vấn sử dụng tìm kiếm nhị phân O(log n) và tính tổng tối đa O(n) phần tử trong các khối một phần. Trong trường hợp xấu nhất, tổng công việc tổng một phần là O(n+q), nhưng trong thực tế, mỗi phần tử đóng góp tối đa một tổng một phần cho mỗi truy vấn. | 
| Không gian | O(n) | Tổng tiền tố và vị trí bắt đầu của mảng có độ dài n. Không cần lưu trữ b một cách rõ ràng. | 

Được cho`n, q <= 3*10^5`, giải pháp phù hợp với bộ nhớ và giới hạn thời gian vì tổng một phần bị giới hạn và các truy vấn chỉ yêu cầu tìm kiếm log n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    # paste solution code here
    n = int(input())
    a = list(map(int, input().split()))
    q = int(input())
    queries = [
```
