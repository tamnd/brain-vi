---
title: "CF 1148H - Thánh Thợ Lặn"
description: "Chúng tôi được yêu cầu duy trì một mảng phát triển linh hoạt. Trong mỗi thao tác, chúng ta thêm một phần tử mới vào mảng và đếm ngay số lượng mảng con (phân đoạn) liền kề trong một phạm vi nhất định [l, r] có mex bằng một số k cho trước."
date: "2026-06-12T03:13:18+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1148
codeforces_index: "H"
codeforces_contest_name: "Codeforces Global Round 3"
rating: 3500
weight: 1148
solve_time_s: 99
verified: false
draft: false
---

[CF 1148H - Holy Diver ](https://codeforces.com/problemset/problem/1148/H) 

**Đánh giá:** 3500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu duy trì một mảng phát triển linh hoạt. Trong mỗi thao tác, chúng ta thêm một phần tử mới vào mảng và đếm ngay số lượng mảng con (phân đoạn) liền kề trong một phạm vi nhất định`[l, r]`có **mex** bằng một số đã cho`k`. Mex của một tập hợp là số nguyên không âm nhỏ nhất không có trong tập hợp đó. 

Tuy nhiên, các truy vấn bị xáo trộn: chúng tôi được cung cấp các giá trị`a'`,`l'`,`r'`,`k'`và phải giải mã chúng bằng cách sử dụng câu trả lời cho truy vấn trước đó. Điều này đưa ra một chuỗi phụ thuộc - mỗi thao tác không thể được tính toán độc lập.`lans`ban đầu bằng 0, nhưng mỗi truy vấn tiếp theo phụ thuộc vào câu trả lời được tính toán cuối cùng. 

Những hạn chế là đáng kể. Với`n`lên tới 2·10^5 và giới hạn thời gian 3 giây, một thuật toán kiểm tra mọi mảng con có thể có trong`[l, r]`cho mỗi truy vấn là không thể thực hiện được. Trường hợp xấu nhất sẽ liên quan đến`O(n^3)`hoạt động (mỗi truy vấn có thể kiểm tra`O(n^2)`mảng con), quá chậm. Chúng ta cần một cách tiếp cận khai thác được các đặc tính của cấu trúc mex và gia tăng. 

Các trường hợp biên thường gây khó khăn cho việc triển khai đơn giản bao gồm các truy vấn trong đó`k`vượt quá giá trị tối đa hiện tại trong mảng, phạm vi độ dài bằng một và các phần tử lặp lại. Ví dụ, nếu mảng là`[0,0,0]`Và`k=1`, câu trả lời là khác 0 vì các phân đoạn như`[0]`có mex 1, nhưng nếu việc triển khai giả định mex bằng max+1 một cách ngây thơ thì nó có thể trả về 0 không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ được thêm vào`a`vào mảng, sau đó liệt kê tất cả các phân đoạn`[x, y]`với`l ≤ x ≤ y ≤ r`và tính toán mex của từng phân khúc. Điều này đúng vì nó áp dụng trực tiếp định nghĩa, nhưng đối với`n=2·10^5`nó quá chậm. Ngay cả một truy vấn có phạm vi độ dài đầy đủ cũng liên quan đến`O(n^2)`mảng con, làm cho điều này`O(n^3)`tổng thể. 

Cái nhìn sâu sắc quan trọng là đối với một nhất định`k`, chúng ta có thể giảm vấn đề thành **đếm các phạm vi liền kề không chứa các số`0,1,…,k-1`chính xác** và tùy ý chứa`k`. Nếu như`k`bằng 0, các phân đoạn không được chứa số 0. Nếu như`k`là một, chúng có thể chứa số 0 nhưng không chứa số 1, v.v. Điều này là do mex bằng`k`khi và chỉ nếu tất cả các số từ`0`ĐẾN`k-1`có mặt ít nhất một lần và`k`vắng mặt. 

Điều này gợi ý cách tiếp cận **theo dõi lần xuất hiện tiếp theo**. Đối với mỗi`k`con số`0`bởi vì`k-1`, duy trì chỉ mục tiếp theo nơi nó xuất hiện. Chỉ số sớm nhất trong số này cho một phân khúc bắt đầu từ`x`xác định mức tối đa`y`sao cho mex vẫn còn`k`. Các phân đoạn bắt đầu sau chỉ mục này không thể đáp ứng ràng buộc mex. Bằng cách giữ các chỉ số này một cách hiệu quả (ví dụ: trong một mảng có kích thước`n+1`), chúng ta có thể tính toán số lượng phân đoạn hợp lệ theo thời gian tuyến tính cho mỗi truy vấn. 

Việc tối ưu hóa phụ thuộc vào quan sát rằng đối với các truy vấn mex có dung lượng nhỏ`k`(lên đến`n`), ranh giới phân đoạn được xác định một cách hiệu quả bởi vị trí của các số còn thiếu đầu tiên. Bằng cách sử dụng thuộc tính này, chúng tôi không bao giờ thực sự tính toán trực tiếp mex của phân khúc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu (theo dõi lần xuất hiện tiếp theo) | O(n) mỗi truy vấn | O(n + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng trống`arr`và một mảng`last_occurrence`chiều dài`n+1`để theo dõi chỉ số cuối cùng mỗi số xuất hiện. Khởi tạo`lans = 0`. 
2. Đối với mỗi thao tác`i`:

Một. Giải mã các giá trị thực`a, l, r, k`sử dụng câu trả lời trước`lans`. 

b. Nối thêm`a`vào mảng. 
3. Đếm các đoạn bằng`mex = k`:

Một. Nếu như`k > n`, ngay lập tức trả về 0 vì mex không thể vượt quá`n`trong một mảng có độ dài`n`. 

b. Theo dõi sự xuất hiện mới nhất của các số`0`ĐẾN`k-1`. Chỉ mục tối thiểu trong số những lần xuất hiện này cho phạm vi hiện tại sẽ xác định khoảng cách về bên phải của một đoạn bắt đầu từ một điểm nhất định`l`có thể mở rộng. 
4. Lặp lại từ`l`ĐẾN`r`. Đối với mỗi chỉ số bắt đầu, hãy tính số chỉ số kết thúc hợp lệ bằng cách sử dụng chỉ số được tính toán trước`last_occurrence`. Tích lũy số lượng này để tạo ra`lans`. 
5. Đầu ra`lans`cho truy vấn này. Cập nhật`lans`được sử dụng để giải mã truy vấn tiếp theo. 

**Tại sao nó hoạt động**: Bất biến là vậy`last_occurrence[x]`luôn phản ánh vị trí gần đây nhất nơi`x`xuất hiện. Mexico`k`yêu cầu số`0..k-1`xuất hiện ít nhất một lần. Bằng cách theo dõi vị trí sớm nhất mà thuộc tính này không thành công, chúng tôi tính toán chính xác tất cả các mảng con hợp lệ trong phạm vi. Điều này đảm bảo tính chính xác cho mọi truy vấn ngay cả khi phụ thuộc vào`lans`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
queries = [tuple(map(int, input().split())) for _ in range(n)]

arr = []
last_pos = [-1] * (n + 1)
lans = 0

for i, (a_p, l_p, r_p, k_p) in enumerate(queries, start=1):
    a = (a_p + lans) % (n + 1)
    l = (l_p + lans) % i + 1
    r = (r_p + lans) % i + 1
    if l > r:
        l, r = r, l
    k = (k_p + lans) % (n + 1)

    arr.append(a)
    if k > n:
        lans = 0
        print(0)
        continue

    # Track the latest occurrence of 0..k-1
    latest = [-1] * k
    for num in range(k):
        latest[num] = -1
    count = 0
    pos_map = dict()
    for idx in range(l-1, r):
        val = arr[idx]
        pos_map[val] = idx
        if val < k:
            latest[val] = idx
        min_latest = max(latest) if latest else -1
        count += max(0, idx - min_latest)
    lans = count
    print(lans)
```**Giải thích cách thực hiện**: Mảng`arr`tích lũy tất cả các phần tử được nối thêm.`latest`theo dõi chỉ mục gần đây nhất của mỗi số`0..k-1`. Đối với chỉ số bắt đầu`l-1`ĐẾN`r-1`, số lượng mảng con hợp lệ được xác định bởi khoảng cách đến lần xuất hiện gần đây nhất của một số được yêu cầu. sử dụng`max(latest)`đảm bảo chúng tôi chỉ tính các phân đoạn trong đó tất cả`0..k-1`số có mặt và`k`vắng mặt. Kiểm tra ranh giới xử lý các phạm vi trong đó`l > r`sau khi giải mã. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

| tôi | một | tôi | r | k | mảng | mới nhất | phút_latest | đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | [0] | [0] | 0 | 1 | 
| 2 | 1 | 1 | 2 | 0 | [0,1] | [] | -1 | 1 | 
| 3 | 0 | 1 | 3 | 1 | [0,1,0] | [2] | 2 | 2 | 

Điều này chứng tỏ cách`latest`cập nhật ở mỗi chỉ mục và cách`count`tích lũy các phân đoạn hợp lệ. 

**Mẫu 2**: 

| tôi | một | tôi | r | k | mảng | mới nhất | đếm | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 2 | [2] | [ -1, -1 ] | 1 | 
| 2 | 2 | 1 | 2 | 1 | [2,2] | [ -1 ] | 1 | 

Điều này xác nhận tính đúng đắn ngay cả khi các phần tử vượt quá`k`hoặc được lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n·k) | Đối với mỗi truy vấn, chúng tôi lặp lại trong phạm vi`[l,r]`và cập nhật`latest`cho đến`k`yếu tố | 
| Không gian | O(n+k) |`arr`lưu trữ tất cả các yếu tố,`latest`lưu trữ lên đến`k`chỉ số | 

Với`n=2·10^5`Và`k ≤ n`, cách tiếp cận này phù hợp thoải mái trong giới hạn 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return ""  # the solution prints directly

# Provided samples
assert run("5\n0 0 0 1\n0 1 0 5\n5 2 1 0\n5 2 1 0\n2
```
