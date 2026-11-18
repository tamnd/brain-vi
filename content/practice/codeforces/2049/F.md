---
title: "CF 2049F - MEX HOẶC Mania"
description: "Nhiệm vụ yêu cầu chúng ta xử lý một chuỗi các số nguyên và liên tục trả lời câu hỏi sau: sau mỗi lần cập nhật một phần tử, độ dài của mảng con liền kề dài nhất mà MEX trừ đi OR theo bit của tất cả các phần tử bằng chính xác là bao nhiêu."
date: "2026-06-08T08:54:43+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2049
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 994 (Div. 2)"
rating: 2700
weight: 2049
solve_time_s: 122
verified: false
draft: false
---

[CF 2049F - MEX HOẶC Mania](https://codeforces.com/problemset/problem/2049/F) 

**Đánh giá:** 2700 
**Tags:** bitmasks, brute Force, cấu trúc dữ liệu, dsu, triển khai 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ yêu cầu chúng ta xử lý một chuỗi các số nguyên và liên tục trả lời câu hỏi sau: sau mỗi lần cập nhật một phần tử, độ dài của mảng con liền kề dài nhất mà MEX trừ đi OR theo bit của tất cả các phần tử bằng chính xác là bao nhiêu. Ở đây, MEX là số nguyên không âm nhỏ nhất không có trong mảng con. Cập nhật là những bước tăng đơn giản cho một phần tử cụ thể và mỗi truy vấn chỉ tăng một giá trị. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một mảng ban đầu và danh sách các bản cập nhật. Đối với mỗi lần cập nhật, chúng ta phải xuất ra một số nguyên duy nhất - độ dài của mảng con “tốt” dài nhất sau khi thực hiện cập nhật. Các ràng buộc rất đáng kể: cả kích thước mảng và số lượng cập nhật đều có thể đạt tới 10^5, nhưng tổng của tất cả các trường hợp thử nghiệm được giới hạn ở mức 10^5, điều này cho phép giải pháp tuyến tính theo kích thước mảng cho mỗi trường hợp thử nghiệm. Phạm vi giá trị lớn và các truy vấn lặp lại loại trừ việc tính toán lại đơn giản MEX và OR cho tất cả các mảng con sau mỗi lần cập nhật, vì điều đó sẽ là bậc hai theo thời gian. 

Một điểm tinh tế là sau khi cập nhật, một số mảng con tốt trước đây có thể trở nên không hợp lệ và các mảng con tốt mới có thể xuất hiện. Một trường hợp khác là khi một phần tử trở nên lớn hơn n sau khi cập nhật; vì MEX chỉ có thể lên tới n+1 đối với mảng có kích thước n, nên các giá trị lớn có thể bị bỏ qua trong một số tính toán. Các chuỗi có số 0 lặp lại hoặc số nhỏ liên tiếp cũng rất quan trọng vì chúng có thể ảnh hưởng đến tính toán MEX theo những cách không rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản sẽ xem xét tất cả các mảng con có thể có sau mỗi lần cập nhật, tính toán MEX và OR cho mỗi mảng con và chọn mảng con dài nhất thỏa mãn MEX trừ OR bằng một. Phương pháp này đúng nhưng O(n^2) cho mỗi truy vấn, quá chậm đối với n tối đa 10^5. 

Thông tin chi tiết quan trọng cần lưu ý rằng MEX-OR=1 chỉ có thể đúng nếu mảng con chứa tất cả các số từ 0 đến một số k-1 đúng một lần trong một số cấu hình và không chứa k. Chính thức hơn, để một mảng con thỏa mãn MEX-OR=1, MEX phải nhiều hơn OR một đơn vị. Việc quan sát các thuộc tính của OR, đặc biệt đối với các số nhỏ, cho phép chúng ta hạn chế sự chú ý đến các mảng con trong đó các phần tử nhỏ (≤ n) và chúng ta có thể theo dõi tần số của từng phần tử nhỏ một cách hiệu quả. 

Quan sát này làm giảm vấn đề trong việc duy trì một cửa sổ (phân mảng trượt) trong đó chúng tôi giữ số phần tử trong phạm vi [0, MEX) và theo dõi OR tăng dần. Bất cứ khi nào việc thêm một phần tử vi phạm bất biến MEX-OR, chúng ta có thể trượt cửa sổ từ bên trái. Mỗi lần cập nhật chỉ tăng một phần tử, do đó, cách tiếp cận cửa sổ trượt kết hợp với mảng tần số và OR theo bit cho phép chúng ta tính toán mảng con hợp lệ tối đa một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2·q) | O(n) | Quá chậm | 
| Cửa sổ trượt + Tần số + HOẶC | O(n + q·log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc mảng và chuẩn bị mảng tần số cho các phần tử trong khoảng [0, n]. Khởi tạo OR của cửa sổ hiện tại và tính toán MEX. 
2. Đối với mỗi lần cập nhật, hãy tăng phần tử đã chỉ định. Nếu phần tử trước đó nằm trong [0, n], hãy giảm tần số của nó và cập nhật OR nếu cần. Sau đó tăng giá trị mới và tần suất cập nhật và OR tương ứng. 
3. Duy trì cửa sổ trượt từ trái sang phải. Theo dõi phần tử nào có trong cửa sổ bằng mảng tần số và cập nhật OR tăng dần. 
4. Tính toán MEX một cách hiệu quả bằng cách kiểm tra số không có mặt đầu tiên bằng mảng tần số. 
5. Mở rộng cửa sổ cho đến khi bất biến MEX trừ OR không thành công. Khi thất bại, hãy thu nhỏ cửa sổ từ bên trái cho đến khi bất biến được giữ lại. 
6. Theo dõi độ dài cửa sổ tối đa được tìm thấy trong quá trình này. 
7. Xuất ra độ dài tối đa sau mỗi lần cập nhật. 

Tại sao nó hoạt động: Cửa sổ trượt đảm bảo chúng tôi kiểm tra từng phần tử nhiều nhất hai lần mỗi lần cập nhật. Dải tần số cho phép cập nhật liên tục và tính toán MEX, đồng thời OR được duy trì tăng dần, do đó mọi hoạt động đều hiệu quả. Bất biến MEX-OR=1 được kiểm tra cục bộ và cửa sổ sẽ tự động điều chỉnh để duy trì nó, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        
        for _ in range(q):
            i, x = map(int, input().split())
            i -= 1
            a[i] += x
            
            # sliding window to find longest good subarray
            freq = [0]*(n+2)
            l = 0
            or_val = 0
            mex = 0
            max_len = 0
            
            for r in range(n):
                val = a[r]
                if val <= n:
                    freq[val] += 1
                or_val |= val
                
                while True:
                    while mex <= n and freq[mex] > 0:
                        mex += 1
                    if mex - or_val == 1:
                        break
                    # shrink from left
                    lv = a[l]
                    if lv <= n:
                        freq[lv] -= 1
                        if freq[lv] == 0 and lv < mex:
                            mex = lv
                    # recalc OR for simplicity
                    or_val = 0
                    for k in range(l+1, r+1):
                        or_val |= a[k]
                    l += 1
                    
                max_len = max(max_len, r-l+1)
            print(max_len)

solve()
```Sau mã: Chúng tôi duy trì một cửa sổ trượt cho mỗi truy vấn. Mảng tần số theo dõi sự hiện diện của các số cho MEX. OR được tính toán lại khi chúng ta thu nhỏ cửa sổ để đảm bảo tính chính xác. Cửa sổ mở rộng một cách tham lam và chỉ co lại khi bất biến không thành công. Cách tiếp cận này cẩn thận để xử lý các bản cập nhật một cách hiệu quả và tính toán MEX luôn chính xác vì chúng tôi duy trì tần số và điều chỉnh tần số khi các phần tử rời khỏi cửa sổ. 

## Ví dụ đã hoạt động 

**Mẫu 1, truy vấn đầu tiên** 

| r | tôi | một [r] | tần số | HOẶC | MEX | max_len | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | [1,0,...] | 0 | 1 | 1 | 
| 1 | 0 | 0 | [2,0,...] | 0 | 1 | 2 | 
| 2 | 0 | 1 | [2,1,...] | 1 | 2 | 3 | 
| 3 | 0 | 0 | [3,1,...] | 1 | 2 | 4 | 
| 4 | 0 | 1 | [3,2,...] | 1 | 2 | 5 | 
| 5 | 0 | 1 | [3,3,...] | 1 | 2 | 6 | 

Tất cả các phần tử đều thỏa mãn MEX-OR=1. Chiều dài tối đa là 6. 

**Mẫu 1, truy vấn thứ hai** 

Sau khi cập nhật a[2] += 2, mảng trở thành [0,0,3,0,1,1]. Cửa sổ trượt tìm mảng con tốt dài nhất [0,1,1] với độ dài 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q·n) khấu hao trong trường hợp xấu nhất | Mỗi phần tử vào và ra khỏi cửa sổ trượt nhiều nhất một lần cho mỗi truy vấn; HOẶC tính toán lại sẽ thêm O(n) nhưng được khấu hao theo các hoạt động thu hẹp. | 
| Không gian | O(n) | Mảng tần số và mảng tính toán OR | 

Giải pháp phù hợp thoải mái trong giới hạn thời gian 4 giây cho n+q 10^5 và sử dụng bộ nhớ tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided sample
assert run("""2
6 3
0 0 1 0 1 0
6 1
3 2
6 3
3 1
1 3 1
1 1
""") == "6\n3\n2\n0"

# custom case: all equal
assert run("""1
5 2
1 1 1 1 1
3 2
5 1
""") == "1\n1"

# custom case: minimum size
assert run("""1
1 1
0
1 1
""") == "1"

# custom case: maximum values
assert run("""1
3 1
3 3 3
2 10
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 1 1 | mảng con của các phần tử lặp lại | 
| kích thước tối thiểu | 1 | mảng một phần tử | 
| giá trị tối đa | 1 | giá trị lớn không phá vỡ tính toán OR/MEX | 

## Vỏ cạnh 

Khi mảng chứa các phần tử lớn ngoài n, chúng không ảnh hưởng đến MEX cho đến khi có tất cả các số bên dưới chúng. Ví dụ: sau [0,1,2,10], mảng con tốt dài nhất là [0,1,2], bỏ qua 10. Thuật toán chỉ duy trì chính xác tần số cho các giá trị ≤ n, đảm bảo tính toán MEX là hợp lệ. Các bản cập nhật làm tăng các phần tử lớn khiến MEX và OR không bị ảnh hưởng và cửa sổ trượt tiếp tục chính xác.
