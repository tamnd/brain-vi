---
title: "CF 71E - Phản ứng tổng hợp hạt nhân"
description: "Chúng ta được cấp một tập hợp các nguyên tử đã biết số nguyên tử và một tập hợp nguyên tử mục tiêu mà chúng ta muốn tạo ra bằng phản ứng tổng hợp. Mỗi hoạt động nhiệt hạch kết hợp chính xác hai nguyên tử thành một và số nguyên tử của nguyên tử thu được là tổng của hai số ban đầu. Chúng ta không thể phân chia nguyên tử."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp"]
categories: ["algorithms"]
codeforces_contest: 71
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 65 (Div. 2)"
rating: 2200
weight: 71
solve_time_s: 110
verified: false
draft: false
---

[CF 71E - Phản ứng tổng hợp hạt nhân](https://codeforces.com/problemset/problem/71/E) 

**Đánh giá:** 2200 
**Thẻ:** bitmask, dp 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các nguyên tử đã biết số nguyên tử và một tập hợp nguyên tử mục tiêu mà chúng ta muốn tạo ra bằng phản ứng tổng hợp. Mỗi hoạt động nhiệt hạch kết hợp chính xác hai nguyên tử thành một và số nguyên tử của nguyên tử thu được là tổng của hai số ban đầu. Chúng ta không thể phân chia nguyên tử. Mỗi nguyên tử ban đầu phải được sử dụng chính xác một lần để tạo ra các nguyên tử mục tiêu, vì vậy về cơ bản chúng ta đang phân chia tập hợp ban đầu thành các tập con có tổng bằng số nguyên tử mong muốn. 

Đầu vào cung cấp các ký hiệu của các nguyên tố mà chúng tôi chuyển đổi thành số nguyên tử của chúng thông qua bảng tra cứu của bảng tuần hoàn. Những giới hạn nhỏ$n \le 17$, ngay lập tức gợi ý rằng các thuật toán hàm mũ liệt kê các tập hợp con là khả thi. Tổng số nguyên tử của các nguyên tử ban đầu bằng tổng số nguyên tử của các nguyên tử mục tiêu, điều này đảm bảo rằng nếu một giải pháp tồn tại thì nó có thể được biểu thị hoàn toàn bằng cách sắp xếp lại và tính tổng các tập hợp con. Khó khăn chính là đảm bảo rằng mỗi nguyên tử được sử dụng chính xác một lần và mỗi tập hợp con tính tổng chính xác bằng một nguyên tử mục tiêu. 

Trường hợp cạnh tinh tế phát sinh khi nhiều nguyên tử mục tiêu có cùng giá trị. Một cách tiếp cận tham lam ngây thơ gán số lớn nhất trước có thể chọn sai phần tử và khiến các tập con sau không thể giải được. Ví dụ: nếu số ban đầu là`[1,2,3,4]`và mục tiêu là`[5,5]`, hái`[4,1]`cho 5 lá đầu tiên`[2,3]`đối với cái thứ hai, nó hoạt động được, nhưng một lựa chọn sai như`[3,2]`những chiếc lá đầu tiên`[1,4]`cho lần thứ hai. Các phương pháp tiếp cận toàn diện hoặc dựa trên DP xử lý vấn đề này một cách chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các phân vùng của tập hợp ban đầu thành$k$các tập hợp con, kiểm tra xem mỗi tập hợp con có tính tổng bằng một mục tiêu hay không và đưa ra các hoạt động hợp nhất tương ứng. Số cách phân vùng$n$mục vào$k$các tập hợp không rỗng được cho bởi các số Stirling loại hai, chúng tăng rất nhanh, đại khái là$k^n$. Với$n=17$, đây là theo thứ tự của$17^{17} \approx 10^{20}$, vượt xa tính toán khả thi. Thậm chí kiểm tra tất cả$2^n$các tập hợp con đã đắt nếu được thực hiện một cách ngây thơ nhiều lần. 

Cái nhìn sâu sắc cho phép một giải pháp khả thi có hai mặt. Đầu tiên,$n$nhỏ nên chúng ta có thể biểu diễn các tập hợp con dưới dạng mặt nạ bit. Thứ hai, chúng ta có thể sử dụng lập trình động trên mặt nạ bit để theo dõi tập hợp con nguyên tử nào có thể tính tổng bằng từng số nguyên tử mục tiêu. Điều này làm giảm vấn đề xuống còn: đối với mỗi nguyên tử mục tiêu, hãy tìm một tập hợp con không chồng chéo của các nguyên tử ban đầu có tổng khớp với nó. Vì có$2^n$các tập hợp con có thể, việc lặp lại chúng là khả thi ($2^{17} = 131072$). Chúng ta có thể tính toán trước tổng của từng tập hợp con và sau đó cố gắng gán các tập hợp con này cho các mục tiêu bằng cách sử dụng tính năng ghi nhớ để tránh tính toán lại. Cuối cùng, việc xây dựng lại trình tự hợp nhất có thể được thực hiện một cách tham lam bằng cách luôn hợp nhất hai nguyên tử lớn nhất còn lại cho đến khi còn lại một nguyên tử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các phân vùng) | O(k^n) | O(k^n) | Quá chậm | 
| Bitmask DP + tổng tập hợp con | O(k * 2^n * n) | O(2^n * n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi ký hiệu nguyên tố thành số nguyên tử. Lưu trữ cả ký hiệu và số để xây dựng lại lời giải. 
2. Biểu diễn từng tập hợp con của tập hợp ban đầu dưới dạng mặt nạ bit. Tính toán trước tổng các số trong mỗi tập hợp con. Điều này cho phép tra cứu nhanh chóng tập hợp con nào có thể tạo ra từng nguyên tử mục tiêu. 
3. Sử dụng DP đệ quy để cố gắng gán các tập hợp con cho các nguyên tử mục tiêu. Ở mỗi bước, hãy chọn một nguyên tử mục tiêu và thử mọi tập hợp con có tổng khớp với số nguyên tử của nó và các bit của chúng chưa được sử dụng. Đánh dấu tập hợp con là đã sử dụng và lặp lại trên các mục tiêu còn lại. Nếu tất cả các mục tiêu đều khớp, chúng ta có một nhiệm vụ hợp lệ. 
4. Nếu không có tập hợp con nào phù hợp với mục tiêu hoặc các mục tiêu còn lại không thể hoàn thành, hãy quay lại. Việc ghi nhớ có thể được áp dụng trên mặt nạ của các nguyên tử không được sử dụng và chỉ số của nguyên tử mục tiêu để ngăn việc tính toán lại bài toán con tương tự. 
5. Sau khi tìm thấy sự phân công hợp lệ các tập hợp con cho các mục tiêu, hãy xây dựng lại các chuỗi hợp nhất. Đối với mỗi tập hợp con, liên tục hợp nhất hai số lớn nhất cho đến khi còn lại một nguyên tử. Theo dõi các ký hiệu nguyên tử ban đầu để in chuỗi dưới dạng`A+B+...->Target`. 
6. Xuất "YES" và chuỗi kết hợp k. Nếu không tìm thấy nhiệm vụ nào, xuất ra "NO". 

Tại sao nó hoạt động: DP đảm bảo mọi mục tiêu đều nhận được một tập hợp con rời rạc gồm các nguyên tử tính tổng chính xác và quá trình tái cấu trúc phản ứng tổng hợp luôn giảm từng tập hợp con thành một nguyên tử bằng cách sử dụng các phản ứng tổng hợp theo cặp hợp pháp. Điều bất biến là mỗi nguyên tử ban đầu được sử dụng một lần và tổng khớp với mục tiêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# map of element symbols to atomic numbers
periodic_table = {
    "H":1,"He":2,"Li":3,"Be":4,"B":5,"C":6,"N":7,"O":8,"F":9,"Ne":10,
    "Na":11,"Mg":12,"Al":13,"Si":14,"P":15,"S":16,"Cl":17,"Ar":18,"K":19,"Ca":20,
    "Sc":21,"Ti":22,"V":23,"Cr":24,"Mn":25,"Fe":26,"Co":27,"Ni":28,"Cu":29,"Zn":30,
    "Ga":31,"Ge":32,"As":33,"Se":34,"Br":35,"Kr":36,"Rb":37,"Sr":38,"Y":39,"Zr":40,
    "Nb":41,"Mo":42,"Tc":43,"Ru":44,"Rh":45,"Pd":46,"Ag":47,"Cd":48,"In":49,"Sn":50,
    "Sb":51,"Te":52,"I":53,"Xe":54,"Cs":55,"Ba":56,"La":57,"Ce":58,"Pr":59,"Nd":60,
    "Pm":61,"Sm":62,"Eu":63,"Gd":64,"Tb":65,"Dy":66,"Ho":67,"Er":68,"Tm":69,"Yb":70,
    "Lu":71,"Hf":72,"Ta":73,"W":74,"Re":75,"Os":76,"Ir":77,"Pt":78,"Au":79,"Hg":80,
    "Tl":81,"Pb":82,"Bi":83,"Po":84,"At":85,"Rn":86,"Fr":87,"Ra":88,"Ac":89,"Th":90,
    "Pa":91,"U":92,"Np":93,"Pu":94,"Am":95,"Cm":96,"Bk":97,"Cf":98,"Es":99,"Fm":100
}

n, k = map(int, input().split())
initial_symbols = input().split()
target_symbols = input().split()

initial_numbers = [periodic_table[s] for s in initial_symbols]
target_numbers = [periodic_table[s] for s in target_symbols]

from itertools import combinations

# precompute subset sums
subset_sum = {}
for mask in range(1, 1<<n):
    total = 0
    for i in range(n):
        if mask & (1<<i):
            total += initial_numbers[i]
    subset_sum[mask] = total

memo = {}
assignment = [0]*k

def solve(mask, idx):
    if idx == k:
        return mask == 0
    key = (mask, idx)
    if key in memo:
        return False
    t = target_numbers[idx]
    submask = mask
    while submask:
        if subset_sum[submask] == t:
            if solve(mask ^ submask, idx+1):
                assignment[idx] = submask
                return True
        submask = (submask - 1) & mask
    memo[key] = False
    return False

if not solve((1<<n)-1, 0):
    print("NO")
    sys.exit(0)

print("YES")

# reconstruct fusion sequence
for idx in range(k):
    mask = assignment[idx]
    elems = []
    for i in range(n):
        if mask & (1<<i):
            elems.append((initial_numbers[i], initial_symbols[i]))
    elems.sort(reverse=True)
    sequence = []
    while len(elems) > 1:
        a = elems.pop(0)
        b = elems.pop(0)
        elems.append((a[0]+b[0], f"{a[1]}+{b[1]}"))
        elems.sort(reverse=True)
    print(f"{elems[0][1]}->{target_symbols[idx]}")
```Mỗi phần tương ứng trực tiếp với các bước thuật toán: ánh xạ các ký hiệu thành số, tính toán trước các tổng tập hợp con, sử dụng DP để gán các tập hợp con và sau đó hợp nhất các nguyên tử lớn nhất trước để tái tạo lại mục tiêu. Thao tác mặt nạ bit cẩn thận sẽ ngăn chặn việc đếm hai lần các nguyên tử và việc sắp xếp đảm bảo ghép nối thích hợp. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Bước | Mặt nạ | Idx mục tiêu | Tập hợp con được chọn | Mặt nạ còn lại | 
| --- | --- | --- | --- | --- | 
| 1 | 1111111111 | 0 (Sn) | Mn+C+K | bit còn lại | 
| 2 | ... | 1 (Pt) | Co+Zn+Sc | ... | 
| 3 | ... | 2 (Y) | Li+Mg+P+F | 0 |
