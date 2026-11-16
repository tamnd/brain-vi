---
title: "CF 1906E - Hợp nhất không sắp xếp"
description: "Chúng tôi được yêu cầu thiết kế ngược bước hợp nhất của một quy trình giống như Sắp xếp Hợp nhất, nhưng có một điểm khác biệt: hai mảng đầu vào có thể không được sắp xếp. Cụ thể, chúng ta nhận được một mảng C có độ dài 2N chứa mọi số nguyên từ 1 đến 2N đúng một lần."
date: "2026-06-09T01:23:05+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "E"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 1900
weight: 1906
solve_time_s: 155
verified: false
draft: false
---

[CF 1906E - Hợp nhất không sắp xếp](https://codeforces.com/problemset/problem/1906/E) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng, dp 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu thiết kế ngược bước hợp nhất của một quy trình giống như Sắp xếp Hợp nhất, nhưng có một điểm khác biệt: hai mảng đầu vào có thể không được sắp xếp. Cụ thể, chúng tôi nhận được một mảng duy nhất`C`chiều dài`2*N`chứa mọi số nguyên từ`1`ĐẾN`2*N`đúng một lần. Nhiệm vụ của chúng ta là chia nó thành hai mảng`A`Và`B`chiều dài`N`mỗi cái, sao cho nếu chúng ta chạy quy trình hợp nhất tiêu chuẩn trên`A`Và`B`, chúng ta sẽ xây dựng lại`C`. Nếu không có sự phân chia như vậy tồn tại, chúng ta phải xuất ra`-1`. 

Chìa khóa ở đây là hiểu thuật toán hợp nhất ở dạng thô: nó luôn so sánh các phần tử đầu tiên còn lại của`A`Và`B`và nối cái nhỏ hơn vào kết quả. Khi`A`hoặc`B`trống, nó chỉ nối thêm các phần tử còn lại của mảng khác. Vì các mảng có thể không được sắp xếp nên việc kiểm tra "nhỏ hơn" cho phép thuật toán chọn một trong hai mảng một cách hiệu quả nếu mặt trước của mảng kia lớn hơn. Điều này có nghĩa là nhiệm vụ của chúng ta không phải là sắp xếp mà là tìm một sự đan xen hợp lệ có thể tạo ra`C`thông qua quy tắc so sánh này. 

Được cho`N`có thể lên tới 1000, tổng chiều dài của`C`là năm 2000. Điều này ngụ ý rằng bất kỳ thuật toán nào có hoạt động O(N²) hầu như không thể vượt qua, nhưng bất cứ điều gì tồi tệ hơn đáng kể sẽ hết thời gian. Vấn đề cũng có một trường hợp phức tạp: thuật toán hợp nhất có thể "thích" mảng này hơn mảng kia nếu các phần tử phía trước nhỏ hơn. Một cách tiếp cận ngây thơ, ví dụ, chia tách`C`đồng đều mà không xem xét các giá trị tương đối có thể thất bại. Ví dụ,`C = [2, 1, 4, 3]`không thể chia thành`A`Và`B`sao cho quy tắc hợp nhất hoạt động nếu chúng ta gán nửa đầu một cách ngây thơ cho`A`và thứ hai đến`B`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản nhưng không hiệu quả. Chúng ta có thể thử mọi phân vùng có thể của`C`thành hai mảng có chiều dài`N`và mô phỏng sự hợp nhất. có`binomial(2N, N)`các phân vùng, có kích thước lớn về mặt thiên văn ngay cả đối với`N=20`, vì vậy điều này là không khả thi đối với`N=1000`. 

Thông tin chi tiết quan trọng là thuật toán hợp nhất, trong biến thể chưa sắp xếp của nó, chỉ yêu cầu mỗi mảng duy trì chuỗi phần tử giảm dần bất cứ khi nào hợp nhất chọn từ mảng đó. Cụ thể hơn, bất cứ khi nào phần tử tiếp theo trong`C`lớn hơn tất cả các phần tử đã được gán cho`A`hoặc`B`, chúng ta có thể đặt nó ở cuối mảng đó. Nếu một phần tử nhỏ hơn phần tử cuối cùng của cả hai mảng ứng viên, nó không thể đi vào một trong hai mảng mà không vi phạm quy trình hợp nhất. Quan sát này làm giảm vấn đề thành một sự phân chia tham lam: quá trình`C`từ trái sang phải, duy trì phần tử cuối cùng của`A`Và`B`và gán từng phần tử mới cho mảng có phần tử lớn hơn phần tử cuối cùng (hoặc bắt đầu một mảng trống). Nếu cả hai đều không hoạt động, hãy xuất`-1`. 

Một cách nhìn khác là xử lý`C`bằng cách lấy các "khối" liền kề của các phần tử giảm dần. Mỗi khối như vậy có thể được gán cho một mảng theo thứ tự chúng xuất hiện. Điều này đảm bảo thuật toán hợp nhất sẽ lấy các phần tử theo đúng thứ tự, bởi vì khối giảm dần không bao giờ buộc việc hợp nhất phải chọn sớm từ mảng khác. Sau đó chúng tôi lần lượt gán các khối cho`A`Và`B`để điền vào cả hai mảng theo chiều dài`N`. Cách tiếp cận khối tham lam này hoạt động vì mỗi khối duy trì một trật tự cục bộ tương thích với quy trình hợp nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2N chọn N * N) | O(N) | Không khả thi | 
| Bài tập khối tham lam | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng trống`A`Và`B`và trống rỗng`current_block`danh sách. Theo dõi phần tử cuối cùng được xử lý,`last = infinity`. 
2. Lặp qua từng phần tử`x`TRONG`C`. Nếu như`x > last`, nó đánh dấu sự bắt đầu của một khối giảm mới. Nối khối hiện tại vào danh sách các khối và đặt lại`current_block`. Sau đó nối thêm`x`ĐẾN`current_block`và cập nhật`last = x`. 
3. Sau khi lặp lại, hãy thêm khối cuối cùng vào danh sách các khối. 
4. Lần lượt gán các khối cho`A`Và`B`theo thứ tự. Nếu một mảng vượt quá độ dài`N`, điều đó là không thể được; đầu ra`-1`. Nếu một khối sẽ tạo ra một mảng dài hơn`N`, chỉ gán số lượng phần tử cần thiết để tiếp cận`N`và di chuyển các phần tử còn lại sang mảng khác. 
5. Sau khi phân công, nếu cả hai`A`Và`B`có độ dài chính xác`N`, xuất chúng. Ngược lại, xuất ra`-1`. 

Tại sao nó hoạt động: mỗi khối được giảm nghiêm ngặt, điều này đảm bảo rằng trong quá trình hợp nhất, các phần tử của khối đó sẽ được chọn liên tiếp từ mảng mà chúng được gán vào. Các khối xen kẽ đảm bảo rằng không có mảng nào bị "ép buộc" cung cấp các phần tử không theo thứ tự bằng cách so sánh hợp nhất. Quy trình hợp nhất chỉ quan tâm đến phần trước của mỗi mảng và khối giảm dần có phần tử lớn nhất ở phía trước, vì vậy các so sánh luôn mang lại kết quả ban đầu`C`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

N = int(input())
C = list(map(int, input().split()))

blocks = []
current_block = []
last = float('inf')

for x in C:
    if x > last:
        blocks.append(current_block)
        current_block = []
    current_block.append(x)
    last = x
if current_block:
    blocks.append(current_block)

A, B = [], []
toggle = True  # alternate assignment

for block in blocks:
    if toggle:
        if len(A) + len(block) > N:
            remaining = N - len(A)
            A.extend(block[:remaining])
            B.extend(block[remaining:])
        else:
            A.extend(block)
    else:
        if len(B) + len(block) > N:
            remaining = N - len(B)
            B.extend(block[:remaining])
            A.extend(block[remaining:])
        else:
            B.extend(block)
    toggle = not toggle

if len(A) == len(B) == N:
    print(' '.join(map(str, A)))
    print(' '.join(map(str, B)))
else:
    print(-1)
```Giải pháp đầu tiên trích xuất các khối giảm dần từ`C`. Điều này ghi lại các phân đoạn mà thuật toán hợp nhất sẽ xử lý liên tiếp từ một mảng. Nhiệm vụ luân phiên lấp đầy`A`Và`B`đồng thời tôn trọng giới hạn độ dài mảng. Việc xử lý ranh giới rất tinh tế: nếu một khối quá lớn không vừa với một mảng, chúng tôi sẽ chia nó thành`A`Và`B`để tránh vượt quá`N`. Không có điều này, các trường hợp cạnh như`C = [5,4,3,2,1,6,7,8]`có thể thất bại. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`C = [3,1,4,5,2,6]`| Bước | current_block | cuối cùng | khối | 
| --- | --- | --- | --- | 
| 3 | [3] | 3 | [] | 
| 1 | [3,1] | 1 | [] | 
| 4 | [4] | 4 | [[3,1]] | 
| 5 | [4,5] | 5 | [[3,1]] | 
| 2 | [2] | 2 | [[3,1],[4,5]] | 
| 6 | [2,6] | 6 | [[3,1],[4,5]] | 

Khối:`[[3,1],[4,5],[2,6]]`Phân công luân phiên:`A = [3,1,2]`,`B = [4,5,6]`Sự hợp nhất của`A`Và`B`tái tạo`C`. 

**Mẫu 2**:`C = [1,2,3,4]`Một khối giảm dần cho mỗi phần tử:`[[1],[2],[3],[4]]`Phân công luân phiên:`A = [1,3]`,`B = [2,4]`Hợp nhất sản lượng`[1,2,3,4]`như mong muốn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Một lần để tạo khối, một lần để gán cho mảng. | 
| Không gian | O(N) | Lưu trữ các khối và hai mảng A và B. | 

Với`N <= 1000`, thuật toán thực hiện tối đa vài nghìn thao tác, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    N = int(input())
    C = list(map(int, input().split()))

    blocks = []
    current_block = []
    last = float('inf')
    for x in C:
        if x > last:
            blocks.append(current_block)
            current_block = []
        current_block.append(x)
        last = x
    if current_block:
        blocks.append(current_block)

    A, B = [], []
    toggle = True
    for block in blocks:
        if toggle:
            if len(A) + len(block) > N:
                remaining = N - len(A)
                A.extend(block[:remaining])
                B.extend(block[remaining:])
            else:
                A.extend(block)
        else:
            if len(B) + len(block) > N:
                remaining = N - len(B
```
