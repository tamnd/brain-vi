---
title: "CF 463D - Gargari và hoán vị"
description: "Chúng ta được cho k hoán vị của các số từ 1 đến n. Hoán vị chỉ đơn giản là sự sắp xếp lại các số này. Mục tiêu của chúng ta là tìm độ dài của dãy số dài nhất xuất hiện theo cùng một thứ tự tương đối trong tất cả k hoán vị."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 463
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 264 (Div. 2)"
rating: 1900
weight: 463
solve_time_s: 58
verified: true
draft: false
---

[CF 463D - Gargari và hoán vị](https://codeforces.com/problemset/problem/463/D) 

**Xếp hạng:** 1900 
**Thẻ:** dfs và tương tự, dp, đồ thị, triển khai 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho _k_ hoán vị của các số từ 1 đến _n_. Hoán vị chỉ đơn giản là sự sắp xếp lại các số này. Mục tiêu của chúng ta là tìm độ dài của dãy số dài nhất xuất hiện theo cùng một thứ tự tương đối trong tất cả các hoán vị _k_. Nói cách khác, chúng ta được yêu cầu tính dãy con chung dài nhất (LCS) trên nhiều hoán vị, không chỉ hai chuỗi hoặc chuỗi. 

Các ràng buộc cho phép chúng ta _n_ lên đến 1000 và _k_ lên đến 5. Điều này ngay lập tức loại trừ mọi cách tiếp cận bạo lực lặp lại trên tất cả các chuỗi con, bởi vì số lượng các chuỗi con có độ dài _n_ là 2^n, lớn về mặt thiên văn đối với _n_ = 1000. Ngay cả một phương pháp lập trình động tiêu chuẩn cho hai chuỗi (O(n^2)) cũng có tỷ lệ tốt cho _n_ = 1000, nhưng chúng ta cần một giải pháp mở rộng hiệu quả đến chuỗi _k_. 

Trường hợp cạnh tinh tế là khi tất cả các hoán vị đều giống hệt nhau. Trong trường hợp đó, LCS chỉ là hoán vị đầy đủ của độ dài _n_. Một trường hợp khác là khi các hoán vị có thứ tự hoàn toàn đảo ngược so với nhau. Ví dụ: nếu n=3, k=2 và các chuỗi là [1,2,3] và [3,2,1] thì LCS chỉ là [2] hoặc [1] hoặc [3], tức là độ dài 1. Cách tiếp cận bất cẩn chỉ so sánh chuỗi đầu tiên với các chuỗi khác theo cặp khác có thể đếm quá nhiều phần tử và đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các chuỗi con của hoán vị đầu tiên và kiểm tra xem mỗi chuỗi có xuất hiện trong tất cả các hoán vị khác hay không. Điều này có tác dụng vì việc kiểm tra tư cách thành viên của một chuỗi trong một hoán vị khác là tuyến tính, nhưng số lượng các chuỗi con là 2^n, vì vậy ngay cả với n=20 thì nó cũng đã quá chậm. Với n=1000 thì điều đó là không khả thi. 

Điểm mấu chốt để giải quyết vấn đề này một cách hiệu quả là mọi hoán vị đều chứa chính xác các số từ 1 đến n. Điều này cho phép chúng ta ánh xạ các số tới vị trí của chúng trong mỗi hoán vị. Để bất kỳ dãy con nào xuất hiện trong tất cả các hoán vị, thứ tự tương đối của các số này phải tăng dần ở các vị trí chỉ mục trong tất cả các dãy. Vì vậy, bài toán quy về việc tìm dãy số dài nhất 1..n có chỉ số tăng đồng thời trong tất cả các hoán vị. 

Khi chúng ta mã hóa mỗi số dưới dạng một bộ vị trí trên tất cả k hoán vị, bài toán sẽ tương đương với việc tìm chuỗi tăng dài nhất trong không gian k chiều. Vì n ≤ 1000 và k ≤ 5, nên phương pháp lập trình động lặp lại các số theo thứ tự hoán vị đầu tiên và kiểm tra các số trước đó sẽ hoạt động. Cụ thể, chúng ta có thể định nghĩa dp[x] là độ dài của dãy con chung dài nhất kết thúc ở số x. Chúng tôi lặp lại các số theo thứ tự hoán vị đầu tiên và đối với mỗi số, hãy kiểm tra tất cả các số trước đó để xem liệu chúng có xuất hiện sớm hơn trong tất cả các hoán vị khác hay không. Nếu vậy, chúng ta có thể mở rộng dp[x] từ dp[y] + 1. 

Điều này mang lại giải pháp O(n^2 * k), khả thi vì n^2 * k 1000^2 * 5 = 5 * 10^6 hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n * k) | O(n) | Quá chậm | 
| Lập trình động với ánh xạ vị trí | O(n^2 * k) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số phần tử n và số hoán vị k. 
2. Lưu trữ tất cả k hoán vị trong danh sách để dễ dàng truy cập. 
3. Xây dựng sơ đồ vị trí cho mỗi hoán vị: pos[i][x] lưu trữ chỉ số gốc 0 của số x trong hoán vị i. Điều này cho phép tra cứu chỉ mục của một số theo thời gian liên tục. 
4. Khởi tạo một mảng dp có độ dài n+1. dp[x] sẽ lưu trữ độ dài của dãy con chung dài nhất kết thúc bằng số x. 
5. Lặp lại các số theo thứ tự hoán vị đầu tiên. Đối với mỗi số`current`, khởi tạo dp[current] = 1 vì dãy con chỉ chứa số này có độ dài 1. 
6. Với mỗi số`previous`điều đó đã đến trước đó`current`trong hoán vị đầu tiên, hãy kiểm tra xem nó có xuất hiện trước không`current`trong tất cả các hoán vị khác. Điều này được thực hiện bằng cách so sánh pos[i][previous] < pos[i][current] với tất cả i từ 1 đến k-1. 
7. Nếu`previous`xuất hiện trước`current`trong tất cả các hoán vị, cập nhật dp[current] = max(dp[current], dp[previous] + 1). 
8. Sau khi xử lý tất cả các số, giá trị lớn nhất trong dp là độ dài của dãy con chung dài nhất. 
9. In giá trị tối đa này. 

Tại sao nó hoạt động: dp[x] lưu trữ chính xác dãy con chung dài nhất kết thúc bằng x vì chúng tôi chỉ mở rộng các chuỗi có số đứng trước hợp lệ trong tất cả các hoán vị. Bằng cách xử lý các số theo thứ tự hoán vị đầu tiên, chúng tôi đảm bảo rằng chuỗi con vẫn được sắp xếp theo chuỗi đầu tiên và việc kiểm tra các hoán vị khác đảm bảo rằng thứ tự chuỗi con được tôn trọng trên toàn cầu. Bất biến này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
perms = [list(map(int, input().split())) for _ in range(k)]

# position mapping: pos[i][x] = index of x in permutation i
pos = [{} for _ in range(k)]
for i in range(k):
    for idx, val in enumerate(perms[i]):
        pos[i][val] = idx

dp = [0] * (n + 1)
first_perm = perms[0]

for current in first_perm:
    dp[current] = 1
    for previous in first_perm:
        if previous == current:
            break
        if all(pos[i][previous] < pos[i][current] for i in range(1, k)):
            dp[current] = max(dp[current], dp[previous] + 1)

print(max(dp))
```Bản đồ vị trí đảm bảo rằng chúng ta có thể nhanh chóng so sánh thứ tự tương đối giữa các hoán vị. Việc lặp lại các số trong hoán vị đầu tiên đảm bảo rằng dp được xây dựng tôn trọng chuỗi trong ít nhất một hoán vị. các`all`kiểm tra đảm bảo điều kiện LCS trên tất cả các chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
1 4 2 3
4 1 2 3
1 2 4 3
```| hiện tại | trước | kiểm tra vị trí | dp[hiện tại] | 
| --- | --- | --- | --- | 
| 1 | - | - | 1 | 
| 4 | 1 | vâng | 2 | 
| 2 | 1 | vâng | 2 | 
| 2 | 4 | không | 2 | 
| 3 | 1 | vâng | 2 | 
| 3 | 4 | vâng | 3 | 
| 3 | 2 | vâng | 3 | 

Dp tối đa = 3, tương ứng với dãy con [1,2,3]. 

### Ví dụ 2 

đầu vào:```
3 2
1 2 3
3 2 1
```| hiện tại | trước | kiểm tra vị trí | dp[hiện tại] | 
| --- | --- | --- | --- | 
| 1 | - | - | 1 | 
| 2 | 1 | không | 1 | 
| 3 | 1 | không | 1 | 
| 3 | 2 | không | 1 | 

Dp tối đa = 1, điều này đúng vì độ dài LCS là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * k) | Đối với mỗi số trong hoán vị đầu tiên, chúng tôi kiểm tra tất cả các số trước đó và so sánh vị trí trong k hoán vị | 
| Không gian | O(n * k) | Bản đồ vị trí và mảng dp có kích thước n+1 | 

Cho n 1000 và k 5, n^2 * k 5 * 10^6, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ ở mức tối thiểu, dễ dàng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, k = map(int, input().split())
    perms = [list(map(int, input().split())) for _ in range(k)]
    pos = [{} for _ in range(k)]
    for i in range(k):
        for idx, val in enumerate(perms[i]):
            pos[i][val] = idx
    dp = [0] * (n + 1)
    first_perm = perms[0]
    for current in first_perm:
        dp[current] = 1
        for previous in first_perm:
            if previous == current:
                break
            if all(pos[i][previous] < pos[i][current] for i in range(1, k)):
                dp[current] = max(dp[current], dp[previous] + 1)
    return str(max(dp))

# Provided samples
assert run("4 3\n1 4 2 3\n4 1 2 3\n1 2 4 3\n") == "3", "sample 1"
assert run("3 2\n1 2 3\n3 2 1\n") == "1", "reversed order"

# Custom cases
assert run("1 2\n1\n1\n") == "1", "single element"
assert run("5 2\n1 2 3 4 5\n1 2 3 4 5\n") == "5", "all identical"
assert run("5 3\n1 2 3 4 5\n5 4 3
```
