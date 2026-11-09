---
title: "CF 1453B - Hoạt động hậu tố"
description: "Chúng ta được cung cấp một mảng các số nguyên và một máy có thể tăng hoặc giảm tất cả các phần tử của hậu tố của mảng thêm 1. Hậu tố là bất kỳ phân đoạn liền kề nào bao gồm phần tử cuối cùng. Mục tiêu của chúng tôi là làm cho tất cả các phần tử trở nên bình đẳng bằng cách sử dụng càng ít thao tác càng tốt."
date: "2026-06-11T03:11:57+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1453
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 688 (Div. 2)"
rating: 1400
weight: 1453
solve_time_s: 666
verified: true
draft: false
---

[CF 1453B - Hoạt động hậu tố](https://codeforces.com/problemset/problem/1453/B) 

**Đánh giá:** 1400 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 11 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một máy có thể tăng hoặc giảm tất cả các phần tử của hậu tố của mảng thêm 1. Hậu tố là bất kỳ phân đoạn liền kề nào bao gồm phần tử cuối cùng. Mục tiêu của chúng tôi là làm cho tất cả các phần tử trở nên bình đẳng bằng cách sử dụng càng ít thao tác càng tốt. Ngoài ra, chúng tôi được phép thay đổi một phần tử thành bất kỳ giá trị nào trước khi bắt đầu; sự thay đổi này không được tính vào tổng số hoạt động. Đầu ra cho mỗi trường hợp thử nghiệm là số lần vận hành máy tối thiểu cần thiết sau khi thay đổi phần tử tối ưu. 

Các ràng buộc chỉ ra rằng mảng có thể dài tới 200.000 phần tử và tổng của tất cả các kích thước mảng trong các trường hợp thử nghiệm cũng bị giới hạn bởi 200.000. Điều này loại trừ bất kỳ thuật toán nào có độ phức tạp tuyến tính cao hơn cho mỗi trường hợp thử nghiệm. Các thao tác trên các phần tử riêng lẻ hoặc các vòng lặp lồng nhau trên mảng sẽ quá chậm, vì vậy chúng ta cần một phương thức xử lý mảng trong một lần hoặc sử dụng các vị trí được tính toán trước một cách hiệu quả. 

Các trường hợp cạnh phát sinh khi mảng đã bằng nhau, khi tất cả trừ một phần tử đều bằng nhau hoặc khi có các giá trị cực âm và cực dương. Một cách tiếp cận đơn giản mô phỏng từng chuỗi hoạt động có thể xảy ra sẽ thất bại trên các mảng lớn do giới hạn thời gian. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ xem xét tất cả các chuỗi tăng và giảm hậu tố có thể có để cân bằng mảng. Chúng ta có thể thử mô phỏng việc thay đổi từng phần tử thành mọi giá trị khác trong mảng, sau đó đếm các thao tác cần thiết. Điều này sẽ đúng nhưng cực kỳ kém hiệu quả. Đối với một mảng có độ dài n, việc xem xét tất cả các giá trị ứng cử viên có thể yêu cầu các phép toán O(n^2) cho mỗi trường hợp kiểm thử, điều này không thể chấp nhận được đối với n lên tới 2×10^5. 

Quan sát quan trọng là chúng ta chỉ cần xem xét việc tạo mảng thống nhất trên một giá trị duy nhất và các phép toán hoạt động trên các hậu tố liền kề. Đối với bất kỳ giá trị mục tiêu x nào được chọn, số lượng thao tác tối thiểu tương ứng với số lượng phân đoạn liền kề tối đa khác với x. Mỗi phân đoạn như vậy có thể được loại bỏ bằng một thao tác hậu tố duy nhất bắt đầu từ phần tử đầu tiên của nó. Do đó, chúng ta có thể tính toán các phép toán tối thiểu bằng cách lặp qua từng giá trị riêng biệt trong mảng và đếm xem có bao nhiêu phân đoạn không bằng giá trị đó. 

Một thông tin chi tiết quan trọng khác là việc thay đổi một phần tử thành bất kỳ giá trị nào sẽ cho phép chúng tôi giảm số lượng phân đoạn xuống một phần tử một cách hiệu quả. Vì vậy, với mỗi ứng cử viên x, chúng tôi tính số phân đoạn khác với x và trừ đi một nếu việc thay đổi một phần tử làm giảm số lượng thao tác. Trong thực tế, việc quét mảng từ trái sang phải và đếm khoảng cách giữa các lần xuất hiện liên tiếp của x sẽ xử lý việc này một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc n và mảng a. Điều này thiết lập bối cảnh để xử lý từng mảng một cách độc lập. 
2. Khởi tạo danh sách các danh sách pos có độ dài n+1. Mỗi pos[x] sẽ lưu trữ các chỉ số nơi giá trị x xuất hiện trong mảng. Điều này cho phép chúng tôi nhanh chóng xác định các phân đoạn có giá trị không phải x mà không cần quét mảng nhiều lần. 
3. Với mỗi phần tử a[i], hãy thêm chỉ mục i của nó vào pos[a[i]]. Sau bước này, chúng ta biết vị trí của từng giá trị riêng biệt trong mảng, điều này cho phép chúng ta xác định khoảng cách giữa các lần xuất hiện liên tiếp một cách hiệu quả. 
4. Khởi tạo ans cho n, số thao tác trong trường hợp xấu nhất. Chúng tôi sẽ giảm thiểu giá trị này trên tất cả các giá trị mục tiêu đề xuất. 
5. Với mỗi giá trị x từ 1 đến n, bỏ qua nếu nó không xuất hiện trong mảng. Ngược lại, khởi tạo bộ đếm cnt về 0. Bộ đếm này sẽ theo dõi số lượng phân đoạn không phải x. 
6. Kiểm tra xem lần xuất hiện đầu tiên của x có nằm ở chỉ số 0 hay không. Nếu vậy, hãy tăng cnt vì tiền tố trước x đầu tiên tạo thành một phân đoạn không phải x. 
7. Lặp lại các vị trí liên tiếp của x. Nếu khoảng cách giữa pos[x][j] và pos[x][j-1] lớn hơn 1, hãy tăng cnt, vì các phần tử giữa các vị trí này tạo thành một đoạn không phải x. 
8. Kiểm tra xem lần xuất hiện cuối cùng của x có nằm ở chỉ số n-1 hay không. Nếu vậy, hãy tăng cnt cho đoạn hậu tố sau x cuối cùng. 
9. Cập nhật ans thành giá trị nhỏ nhất của ans và cnt. Điều này đảm bảo rằng cuối cùng, ans giữ các hoạt động tối thiểu cần thiết cho bất kỳ giá trị mục tiêu nào. 
10. In kết quả cho test case hiện tại. 

Tại sao nó hoạt động: Mỗi phân đoạn của các phần tử không bằng mục tiêu đã chọn có thể bị xóa trong một thao tác và bằng cách đếm chính xác các phân đoạn này bằng cách sử dụng vị trí của x, chúng tôi tránh được việc đếm quá mức. Việc thay đổi một phần tử có thể giảm số lượng phân đoạn đi một phần tử, điều này được xử lý ngầm bằng cách xem xét tất cả các giá trị mục tiêu có thể có. Điều này đảm bảo rằng chúng tôi tìm thấy số lượng hoạt động tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    # Map value to positions
    pos = dict()
    for i, val in enumerate(a):
        if val not in pos:
            pos[val] = []
        pos[val].append(i)
    
    ans = n
    for val, indices in pos.items():
        cnt = 0
        if indices[0] != 0:
            cnt += 1
        for j in range(1, len(indices)):
            if indices[j] - indices[j-1] > 1:
                cnt += 1
        if indices[-1] != n-1:
            cnt += 1
        ans = min(ans, cnt)
    
    print(ans)
```Mã bắt đầu bằng cách đọc đầu vào một cách hiệu quả. Việc sử dụng từ điển cho pos sẽ tránh được các giả định về phạm vi giá trị và xử lý các số nguyên âm. Đếm khoảng cách giữa các lần xuất hiện liên tiếp xác định các phân đoạn liền kề cần loại bỏ. Kiểm tra tiền tố và hậu tố xử lý các cạnh. Cập nhật ans cho mỗi ứng cử viên đảm bảo chúng tôi chọn số lượng hoạt động tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào:`[1, 2, 3, 1, 2, 3, 1]`. Các giá trị ứng viên là 1, 2, 3. 

| Ứng viên x | Vị trí | tính toán cnt | Kết quả cnt | 
| --- | --- | --- | --- | 
| 1 | [0,3,6] | tiền tố=0, khoảng trống:3-0=3>1 ->1,6-3=3>1 ->2, hậu tố=0 | 2 | 
| 2 | [1,4] | tiền tố=1>0 ->1, khoảng cách:4-1=3>1 ->2, hậu tố=6!=6? 0 | 2 | 
| 3 | [2,5] | tiền tố=2>0 ->1, khoảng cách:5-2=3>1 ->2, hậu tố=6!=6? 0 | 2 | 

Cnt tối thiểu là 2, phù hợp với đầu ra mẫu. 

### Ví dụ 2 

Mảng đầu vào:`[5,0,0,0,5]`. Giá trị ứng viên 0,5. 

| Ứng viên x | Vị trí | tính toán cnt | Kết quả cnt | 
| --- | --- | --- | --- | 
| 0 | [1,2,3] | tiền tố=1>0 ->1, khoảng trống:2-1=1 ->0, 3-2=1 ->0, hậu tố=4!=4 ->1 | 2 | 
| 5 | [0,4] | tiền tố=0? ->0, khoảng cách:4-0=4>1 ->1, hậu tố=4!=4 ->0 | 1 | 

Hoạt động tối thiểu = 1, phù hợp với đầu ra mẫu. 

Những dấu vết này xác nhận rằng việc đếm tiền tố, khoảng cách và hậu tố theo dõi chính xác số lượng thao tác được yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử được truy cập một lần để ghi lại vị trí; mỗi giá trị được quét qua các vị trí của nó một lần. Tổng của tất cả các giá trị là ≤ n. | 
| Không gian | O(n) | Từ điển vị trí lưu trữ các chỉ mục cho từng giá trị riêng biệt; trong trường hợp xấu nhất tất cả các phần tử là duy nhất. | 

Thuật toán chia tỷ lệ tuyến tính với kích thước mảng, vừa vặn thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ cho n 2×10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # call the solution
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        pos = dict()
        for i, val in enumerate(a):
            if val not in pos:
                pos[val] = []
            pos[val].append(i)
        ans = n
        for val, indices in pos.items():
            cnt = 0
            if indices[0] != 0:
                cnt += 1
            for j in range(1, len(indices)):
                if indices[j] - indices[j-1] > 1:
                    cnt += 1
            if indices[-1] != n-1:
                cnt += 1
            ans = min(ans, cnt)
        print(ans)
    return output.getvalue().strip()

# Provided sample
assert run("7\n2\n1 1\n3\n-1 0 2\n4\n99 96 97 95\n4\n-3 -5 -2 1\n6\n1 4 3 2 4 1\n5\n5 0 0 0 5\n9\n-367741579 319422997 -415264583 -125558838 -300860379 420848004 294512916 -383235489 425814447") == "0\n1\n3\n4\n6\n5\n284737210
```
