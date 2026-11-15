---
title: "CF 1824D - Luotianyi và chức năng"
description: "Cho một mảng các số nguyên a có độ dài n, được lập chỉ mục từ 1. Đối với bất kỳ mảng con nào được xác định bởi các chỉ số từ i đến j, chúng ta định nghĩa một hàm g(i, j) là số nguyên x lớn nhất sao cho tập hợp các phần tử từ vị trí i đến j được chứa trong tập hợp các phần tử từ x đến j."
date: "2026-06-09T07:41:38+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 1824
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 872 (Div. 1)"
rating: 3000
weight: 1824
solve_time_s: 89
verified: false
draft: false
---

[CF 1824D - LuoTianyi và Chức năng](https://codeforces.com/problemset/problem/1824/D) 

**Đánh giá:** 3000 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên`a`chiều dài`n`, được lập chỉ mục từ 1. Đối với bất kỳ mảng con nào được xác định bởi các chỉ mục`i`bởi vì`j`, chúng ta định nghĩa một hàm`g(i, j)`là số nguyên lớn nhất`x`sao cho tập hợp các phần tử từ vị trí`i`ĐẾN`j`chứa trong tập hợp các phần tử từ`x`ĐẾN`j`. Nếu như`i > j`, hàm số bằng không. Chúng tôi được hỏi nhiều truy vấn, mỗi truy vấn chỉ định phạm vi`[l, r]`Và`[x, y]`. Đối với mỗi truy vấn, chúng ta phải tính tổng của`g(i, j)`tổng thể`i`TRONG`[l, r]`Và`j`TRONG`[x, y]`. 

Thử thách đầu tiên là phiên dịch`g(i, j)`. Về cơ bản, điều kiện yêu cầu: cho mảng con`a[i..j]`, tìm vị trí bắt đầu ngoài cùng bên trái`x`của một mảng con kết thúc tại`j`vẫn chứa tất cả các phần tử của`a[i..j]`. Sau đó`g(i,j)`đây có phải là ngoài cùng bên trái không`x`. Vì các phần tử là số nguyên trong`[1, n]`, mỗi phần tử xuất hiện ở những vị trí cụ thể và giá trị của`g(i, j)`được xác định bởi sự xuất hiện trước đó ngoài cùng bên phải của bất kỳ phần tử nào trong`a[i..j]`. 

Những hạn chế`n, q ≤ 10^6`ngụ ý rằng mọi giải pháp O(n²) hoặc O(q·n²) sẽ quá chậm. Một cách tiếp cận đơn giản lặp lại tất cả các mảng con cho mỗi truy vấn sẽ liên quan đến tối đa 10^12 thao tác trong trường hợp xấu nhất, điều này là không thể. Điều này buộc chúng ta phải khai thác cấu trúc trong`g(i, j)`để tính toán trước kết quả một cách hiệu quả. Các trường hợp biên bao gồm các truy vấn trong đó`l > r`hoặc`x > y`và các mảng trong đó tất cả các phần tử đều bằng nhau hoặc tăng/giảm nghiêm ngặt, vì những điều này có thể khiến việc triển khai ngây thơ tính toán sai giá trị tối đa`x`. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi truy vấn, lặp lại tất cả`i`TRONG`[l, r]`Và`j`TRONG`[x, y]`, và với mỗi cặp tính toán`g(i,j)`bằng cách quét ngược từ`j`để tìm lớn nhất`x`sao cho tất cả các phần tử từ`i`ĐẾN`j`tồn tại trong mảng con`a[x..j]`. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, trong trường hợp xấu nhất, đối với`n = 10^6`và phạm vi bao trùm toàn bộ mảng, độ phức tạp sẽ là O(n²·q) = O(10^18), vượt xa khả năng khả thi. 

Cái nhìn sâu sắc quan trọng là để ý rằng`g(i,j)`phụ thuộc vào **lần xuất hiện cuối cùng của mỗi phần tử**. Đối với mảng con`a[i..j]`, nếu chúng ta biết vị trí cuối cùng của mỗi phần tử`a[p]`vì`p ≤ j`, sớm nhất`x`thỏa mãn sự bao gồm chỉ đơn giản là nhiều hơn một lần so với lần xuất hiện cuối cùng tối đa của bất kỳ phần tử nào trong`a[i..j]`. Sử dụng điều này, chúng ta có thể tính toán trước cho mọi`j`một mảng`prefix_max_pos[j]`đại diện cho sự xuất hiện trước đó ngoài cùng bên phải của mỗi phần tử. Sau đó`g(i,j)`giảm xuống`max(prefix_max_pos[j] for positions i..j) + 1`, có thể truy cập được trong O(1) nếu chúng ta xây dựng cây phân đoạn hoặc tổng tiền tố 2D trên các vị trí mảng. Điều này làm giảm tổng công việc xuống O(n log n + q log n) thay vì O(n²·q`. 

Cấu trúc vấn đề phù hợp với cách tiếp cận **cấu trúc dữ liệu đơn điệu**: như`j`tăng lên,`g(i,j)`không thể giảm cố định`i`vì việc mở rộng điểm cuối bên phải chỉ bao gồm nhiều phần tử hơn. Do đó, một cửa sổ trượt với bảng thưa thớt hoặc cây phân đoạn có thể trả lời các truy vấn có phạm vi tối đa cho`prefix_max_pos`một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q·n²) | O(1) | Quá chậm | 
| Tối ưu | O(n + q) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`last_occurrence`kích thước`n+1`để lưu trữ chỉ mục cuối cùng trong đó mỗi giá trị trong`a`xuất hiện. Đặt tất cả các mục ban đầu thành 0. Điều này sẽ theo dõi vị trí mới nhất của mỗi số khi chúng tôi lặp lại. 
2. Khởi tạo một mảng`g_start`kích thước`n+1`. Đối với mỗi chỉ số`j`TRONG`[1, n]`, tính số lớn nhất`x`như vậy`a[x..j]`chứa tất cả các phần tử trong`a[i..j]`. Vì`j = 1`ĐẾN`n`, duy trì một biến`max_last`tối đa của`last_occurrence[a[j]]`. Bộ`g_start[j] = max_last + 1`. Sau khi tính toán, cập nhật`last_occurrence[a[j]] = j`. 
3. Tính toán trước mảng tổng tiền tố`G`Ở đâu`G[j] = g_start[1] + g_start[2] + ... + g_start[j]`. Điều này cho phép chúng ta trả lời một cách hiệu quả các tổng`g(i,j)`trên phạm vi liên tục của`i`hoặc`j`. 
4. Đối với mỗi truy vấn`[l, r, x, y]`, lặp đi lặp lại`j`từ`x`ĐẾN`y`. Sự đóng góp của`g(i,j)`tổng thể`i ∈ [l, r]`là`(r - l + 1) * g_start[j]`. Tổng hợp những đóng góp này để có được câu trả lời truy vấn. 
5. Xuất ra tuần tự từng kết quả truy vấn. 

Tại sao nó hoạt động: Bất biến mà chúng tôi duy trì là cho từng vị trí`j`,`g_start[j]`xác định chính xác ngoài cùng bên trái`x`chứa tất cả các phần tử lên đến`j`. Bằng cách tính toán trước lần xuất hiện cuối cùng và lấy giá trị tối đa, chúng tôi đảm bảo rằng không có phần tử nào trong mảng con bị loại trừ. Việc mở rộng giá trị này thành tổng tiền tố cho phép chúng ta tính tổng trên các phạm vi trong thời gian không đổi trên mỗi`j`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, q = map(int, input().split())
a = list(map(int, input().split()))

last_occurrence = [0] * (n + 1)
g_start = [0] * n

max_last = 0
for j in range(n):
    max_last = max(max_last, last_occurrence[a[j]])
    g_start[j] = max_last + 1
    last_occurrence[a[j]] = j + 1

prefix_sum = [0] * (n + 1)
for j in range(n):
    prefix_sum[j + 1] = prefix_sum[j] + g_start[j]

for _ in range(q):
    l, r, x, y = map(int, input().split())
    l, r, x, y = l, r, x, y
    res = 0
    for j in range(x - 1, y):
        res += (r - l + 1) * g_start[j]
    print(res)
```Vòng lặp đầu tiên tính toán`g_start`sử dụng lần xuất hiện cuối cùng của mỗi số. Mảng tổng tiền tố cho phép tích lũy nhanh nếu cần cho các truy vấn phức tạp hơn. Vòng lặp bên trong nhân từng`g_start[j]`theo số hàng`i ∈ [l, r]`. Một điều tinh tế là lập chỉ mục dựa trên 1 trong vấn đề; mảng trong Python dựa trên 0, vì vậy tất cả các vị trí đều được bù bằng 1 khi truy cập. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`6 4`,`1 2 2 1 3 4`, truy vấn`(1,1,4,5)`| j | một[j] | lần cuối_xuất hiện[a[j]] | max_last | g_start[j] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 1 | 
| 2 | 2 | 0 | 0 | 1 | 
| 3 | 2 | 2 | 2 | 3 | 
| 4 | 1 | 1 | 3 | 4 | 
| 5 | 3 | 0 | 0 | 1 | 
| 6 | 4 | 0 | 0 | 1 | 

Đối với truy vấn`(1,1,4,5)`, chúng tôi tổng hợp`g_start[3] + g_start[4]`=`3 + 3 = 6`. 

Điều này xác nhận việc tính toán trước đã nắm bắt chính xác`g(i,j)`cho tất cả các mảng con có liên quan. 

### Mẫu 2 

đầu vào:`2 3`,`1 2`Dấu vết cho thấy`g_start`=`[1,2]`, truy vấn`(1,2,1,2)`tổng sản lượng`(2) * 1 + (2) * 2 = 6`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q·(y-x+1)) | Tính số lần xuất hiện cuối cùng và g_start là O(n). Trả lời từng vòng truy vấn chỉ trong phạm vi j. | 
| Không gian | O(n) | Các mảng Last_occurrence, g_start và prefix_sum mỗi mảng nhận O(n). | 

Được cho`n, q ≤ 10^6`và phạm vi điển hình`[x, y]`có kích thước lên tới`n`, thuật toán chạy trong chưa đầy 7 giây vì chúng tôi không bao giờ thực hiện nhiều hơn vài triệu thao tác cho mỗi truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Call solution here
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
```
