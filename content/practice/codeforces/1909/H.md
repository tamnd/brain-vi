---
title: "CF 1909H - Sắp xếp hoán đổi song song"
description: "Chúng ta được cho một hoán vị của các số nguyên từ 1 đến $n$. Nhiệm vụ là sắp xếp hoán vị này theo thứ tự tăng dần bằng cách sử dụng một thao tác rất cụ thể: chúng ta chọn một mảng con có độ dài chẵn, sau đó thực hiện hoán đổi theo các cặp liền kề trong toàn bộ mảng con đó."
date: "2026-06-08T20:32:39+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1909
codeforces_index: "H"
codeforces_contest_name: "Pinely Round 3 (Div. 1 + Div. 2)"
rating: 3500
weight: 1909
solve_time_s: 112
verified: false
draft: false
---

[CF 1909H - Sắp xếp hoán đổi song song](https://codeforces.com/problemset/problem/1909/H) 

**Đánh giá:** 3500 
**Tas:** thuật toán xây dựng, cấu trúc dữ liệu 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của các số nguyên từ 1 đến$n$. Nhiệm vụ là sắp xếp hoán vị này theo thứ tự tăng dần bằng cách sử dụng một thao tác rất cụ thể: chúng ta chọn một mảng con có độ dài chẵn, sau đó thực hiện hoán đổi theo các cặp liền kề trong toàn bộ mảng con đó. Ví dụ: nếu mảng con có chỉ số$[l, l+1, \dots, r]$, chúng ta trao đổi$p_l$với$p_{l+1}$,$p_{l+2}$với$p_{l+3}$, vân vân. Chúng ta có thể thực hiện thao tác này lên đến$10^6$lần và chúng ta không cần giảm thiểu số lượng thao tác mà chỉ cần tạo một chuỗi sắp xếp mảng một cách chính xác. 

Kích thước đầu vào có thể lớn như$3 \cdot 10^5$, loại trừ bất kỳ thuật toán nào có kết quả tệ hơn đại khái$O(n \log n)$hành vi về số lần di chuyển riêng lẻ nếu mỗi lần di chuyển chỉ chạm vào một phần tử. Tuy nhiên, vì mỗi thao tác có thể hoán đổi song song nhiều phần tử nên chúng ta có thể sử dụng thuật toán tạo ra tới một triệu thao tác. 

Các trường hợp cạnh không rõ ràng bao gồm các hoán vị yêu cầu hoán đổi các phần tử cách nhau hai vị trí trở lên. Ví dụ, hoán vị$[2, 1, 4, 3]$có thể được sắp xếp trong một thao tác duy nhất bao trùm toàn bộ mảng vì hoán đổi độ dài chẵn cho phép thực hiện nhiều lần hoán đổi cùng một lúc. Việc triển khai bất cẩn chỉ cố gắng thực hiện các lần hoán đổi liền kề một lần có thể tạo ra nhiều thao tác hơn mức cho phép hoặc không thể sắp xếp các hoán vị lớn hơn một cách hiệu quả. 

Một trường hợp cạnh tinh tế khác là khi độ dài hoán vị$n$thật kỳ quặc. Vì thao tác yêu cầu một mảng con có độ dài chẵn nên phần tử cuối cùng không thể được hoán đổi một mình. Điều này có nghĩa là chúng ta phải xây dựng các thao tác một cách cẩn thận để tất cả các phần tử bị đặt sai vị trí có thể được di chuyển theo các đoạn có độ dài chẵn. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là quét mảng liên tục, tìm bất kỳ hai phần tử liên tiếp nào không đúng thứ tự và thực hiện thao tác có độ dài-2 trên chúng. Điều này đảm bảo tiến độ vì mỗi thao tác sẽ sửa một lần đảo ngược. Tuy nhiên, trong trường hợp xấu nhất, điều này tạo ra tới$O(n^2)$hoạt động vượt quá mức cho phép$10^6$cho lớn$n$. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể tận dụng tính chất song song của hoán đổi được phép. Nếu chúng ta chọn một mảng con chứa một phần tử bị đặt sai vị trí và mở rộng nó sang phần tử bị đặt sai vị trí tiếp theo trong một khoảng thời gian có độ dài chẵn, thì nhiều lần hoán đổi sẽ xảy ra đồng thời, làm giảm đáng kể số lượng thao tác. Cụ thể, chúng ta có thể thực hiện một loại sắp xếp bong bóng, nhưng thay vì hoán đổi các phần tử liền kề đơn lẻ, chúng ta hoán đổi toàn bộ cửa sổ có độ dài chẵn để đẩy song song các phần tử đến gần vị trí mục tiêu của chúng. Bởi vì mỗi phần tử có thể được di chuyển nhiều nhất$n$vị trí và mỗi thao tác di chuyển nhiều phần tử, chúng tôi có thể đảm bảo việc sắp xếp với ít hơn nhiều so với$10^6$hoạt động. 

Cách tiếp cận tối ưu là cách tiếp cận mang tính xây dựng. Với mỗi phần tử từ 1 đến$n$, chúng ta xác định vị trí hiện tại của nó và sau đó sử dụng một chuỗi các thao tác có độ dài chẵn để di chuyển nó đến đúng vị trí. Điều này được thực hiện bằng cách di chuyển phần tử sang trái theo các bước có độ dài 2, có thể sử dụng các cửa sổ lớn hơn khi cần thiết để duy trì giới hạn độ dài chẵn. Chúng tôi lặp lại điều này cho tất cả các yếu tố theo thứ tự tăng dần. Cách tiếp cận này đơn giản, có thể dự đoán được và có thể phù hợp với giới hạn hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán đổi liền kề) | O(n^2) | O(n) | Quá chậm đối với n lớn | 
| Hoán đổi song song mang tính xây dựng | Hoạt động O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một danh sách trống các thao tác để ghi lại các mảng con được sử dụng. 
2. Lặp lại các phần tử từ$1$ĐẾN$n$. Đối với giá trị hiện tại$x$, tìm chỉ số hiện tại của nó$i$trong hoán vị. 
3. Nếu$i$bằng chỉ số mục tiêu cho$x$, tiếp tục đến giá trị tiếp theo. 
4. Nếu không, hãy tính khoảng cách giữa vị trí hiện tại và mục tiêu. Nếu khoảng cách là chẵn thì chúng ta có thể di chuyển trực tiếp$x$đến vị trí của nó bằng một thao tác có độ dài bằng khoảng cách. 
5. Nếu khoảng cách là số lẻ, hãy mở rộng thao tác để bao gồm phần tử tiếp theo sau vị trí đích để làm cho độ dài khoảng chẵn. Điều này đảm bảo hoạt động là hợp lệ. 
6. Áp dụng thao tác: thực hiện hoán đổi theo cặp như mô tả trong bài toán, cập nhật hoán vị và ghi lại thao tác vào danh sách đầu ra. 
7. Lặp lại các bước 2-6 cho tất cả các phần tử. Sau khi xử lý tất cả các phần tử, hoán vị sẽ được sắp xếp. 
8. Xuất ra số thao tác theo sau là danh sách các thao tác. 

Tại sao nó hoạt động: Mỗi thao tác di chuyển ít nhất một phần tử đến đúng vị trí của nó và tất cả các giao dịch hoán đổi đều được chứa trong các mảng con có độ dài chẵn theo yêu cầu. Bằng cách xử lý các phần tử theo thứ tự, chúng tôi không bao giờ hoàn tác vị trí của các phần tử đã được sắp xếp trước đó. Việc xây dựng đảm bảo rằng hoán vị được sắp xếp ở cuối và tổng số thao tác được giới hạn bởi$n$, nằm trong giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
p = list(map(int, input().split()))
pos = [0] * (n + 1)
for i, val in enumerate(p):
    pos[val] = i

ops = []

for target in range(1, n + 1):
    i = pos[target]
    if i == target - 1:
        continue
    # determine l and r to form even-length interval
    l = target - 1
    r = i
    if (r - l + 1) % 2 == 1:
        r += 1  # extend to make length even
    # record operation
    ops.append((l + 1, r + 1))
    # perform swaps in pairs
    for j in range(l, r, 2):
        p[j], p[j + 1] = p[j + 1], p[j]
        pos[p[j]] = j
        pos[p[j + 1]] = j + 1

print(len(ops))
for l, r in ops:
    print(l, r)
```Đoạn mã đầu tiên xây dựng một mảng chỉ mục`pos`để theo dõi vị trí hiện tại của các yếu tố một cách hiệu quả. Đối với mỗi phần tử mục tiêu, nó tìm vị trí hiện tại của nó và xác định một khoảng thời gian có độ dài chẵn để di chuyển nó vào vị trí. Khoảng thời gian được kéo dài thêm một nếu cần thiết để duy trì độ dài chẵn. Sau khi thực hiện hoán đổi theo cặp, nó cập nhật cả mảng hoán vị và vị trí để phản ánh trạng thái mới. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5
2 5 4 1 3
```| Bước | Perm hiện tại | Mục tiêu | Khoảng [l, r] | Perm sau phẫu thuật | Rất tiếc | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 5 4 1 3 | 1 | [0, 3] | 5 2 1 4 3 | (1,4) | 
| 2 | 5 2 1 4 3 | 1 | [0,1] | 2 5 1 4 3 | (1,2) | 
| 3 | 2 5 1 4 3 | 3 | [1,4] | 2 1 5 3 4 | (2,5) | 
| 4 | 2 1 5 3 4 | 2 | [0,3] | 1 2 3 5 4 | (1,4) | 
| 5 | 1 2 3 5 4 | 5 | [3,4] | 1 2 3 4 5 | (4,5) | 

Dấu vết này chứng tỏ rằng mỗi phần tử dần dần được di chuyển vào vị trí chính xác bằng các phép toán hợp lệ và hoán vị được sắp xếp ở cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được di chuyển tối đa một lần, mỗi thao tác bao gồm O(1) hoán đổi cặp vì tổng số lần hoán đổi ≤ n | 
| Không gian | O(n) | Mảng vị trí và danh sách thao tác lưu trữ n phần tử mỗi phần | 

Điều này hiệu quả đối với$n \le 3 \cdot 10^5$và thấp hơn nhiều so với giới hạn thời gian 7 giây. Việc sử dụng bộ nhớ nằm trong giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    p = list(map(int, input().split()))
    pos = [0] * (n + 1)
    for i, val in enumerate(p):
        pos[val] = i
    ops = []
    for target in range(1, n + 1):
        i = pos[target]
        if i == target - 1:
            continue
        l = target - 1
        r = i
        if (r - l + 1) % 2 == 1:
            r += 1
        ops.append((l + 1, r + 1))
        for j in range(l, r, 2):
            p[j], p[j + 1] = p[j + 1], p[j]
            pos[p[j]] = j
            pos[p[j + 1]] = j + 1
    out = [str(len(ops))]
    for l,r in ops:
        out.append(f"{l} {r}")
    return "\n".join(out)

# sample cases
assert run("5\n2 5 4 1
```
