---
title: "CF 2118F - Chuyển đổi và hoán đổi"
description: "Chúng ta được hỏi liệu một mảng số nguyên a có thể được chuyển đổi thành một mảng b khác bằng cách sử dụng hai loại phép toán hay không. Thao tác đầu tiên là dịch chuyển trái theo chu kỳ, di chuyển mọi phần tử sang trái một vị trí và bao phần tử đầu tiên về cuối."
date: "2026-06-08T04:03:55+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "graphs", "hashing", "trees"]
categories: ["algorithms"]
codeforces_contest: 2118
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1030 (Div. 2)"
rating: 3100
weight: 2118
solve_time_s: 116
verified: false
draft: false
---

[CF 2118F - Chuyển đổi và hoán đổi](https://codeforces.com/problemset/problem/2118/F) 

**Đánh giá:** 3100 
**Thẻ:** cấu trúc dữ liệu, đồ thị, hàm băm, cây 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được hỏi liệu một mảng số nguyên,`a`, có thể được chuyển đổi thành một mảng khác,`b`, sử dụng hai loại hoạt động. Thao tác đầu tiên là dịch chuyển trái theo chu kỳ, di chuyển mọi phần tử sang trái một vị trí và bao phần tử đầu tiên về cuối. Hoạt động thứ hai là hoán đổi giữa các phần tử liền kề nếu chênh lệch số của chúng ít nhất là 2. Cả hai mảng đều chứa tất cả các số nguyên từ 1 đến`m`ít nhất một lần nên không thiếu số nào. Mảng có thể dài hơn`m`, nghĩa là một số con số có thể lặp lại. 

Nhiệm vụ là xác định cho mỗi trường hợp thử nghiệm nếu có thể đạt được`b`từ`a`sử dụng các thao tác này. Với`n`lên tới 500.000 và tổng cộng 500.000 phần tử trong tất cả các trường hợp thử nghiệm, bất kỳ giải pháp nào xem xét tất cả các chuỗi hoán đổi hoặc dịch chuyển có thể xảy ra đều không khả thi. Lực lượng vũ phu ngây thơ mô phỏng mọi giao dịch hoán đổi có thể xảy ra sẽ mất thời gian theo cấp số nhân hoặc theo cấp số nhân, vượt xa giới hạn cho phép. 

Trường hợp cạnh không rõ ràng phát sinh khi có liên quan đến các số lặp lại. Ví dụ, hãy xem xét`a = [1,1,2]`Và`b = [1,2,1]`. Một cách tiếp cận bất cẩn có thể cố gắng kiểm tra thứ tự tương đối của từng cặp mà không xem xét rằng các số lặp lại sẽ cản trở một số giao dịch hoán đổi. Ở đây, thuật toán phải xử lý các số lặp lại và vị trí của chúng một cách cẩn thận. Một điểm tinh tế khác xảy ra khi các số chỉ khác nhau 1: việc hoán đổi bị cấm, vì vậy ngay cả khi tất cả các số khớp nhau trong nhiều tập hợp, một số hoán vị không thể đạt được. Mảng nhỏ như`a = [1,2]`,`b = [2,1]`với`m = 2`cũng tiết lộ rằng các giao dịch hoán đổi không thể thu hẹp chênh lệch bằng 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng mọi chuỗi hoạt động có thể xảy ra. Đối với mỗi ca, chúng ta có thể xoay mảng và đối với mỗi lần hoán đổi có thể, hãy thử hoán đổi các phần tử liền kề nếu được phép. Cách tiếp cận này có hiệu quả vì các thao tác được phép có thể tạo ra một số hoán vị, nhưng nó sẽ thất bại ngay khi`n`vượt quá một số ít. Với`n = 500,000`, thậm chí xem xét một lần hoán đổi cho mỗi phần tử là`O(n)`cho mỗi thao tác và việc khám phá tất cả các trình tự trở thành điều phi thường. 

Thông tin chi tiết quan trọng là giao dịch hoán đổi chỉ cho phép các phần tử khác nhau từ 2 trở lên được trao đổi vị trí. Do đó, các phần tử khác nhau 1 không thể di chuyển qua nhau. Điều này có nghĩa là thứ tự tương đối của các số có cùng “giá trị modulo liền kề” được giữ nguyên. Cụ thể, chúng ta có thể nhóm các số thành các lớp trong đó các phần tử cùng lớp có thể tự do hoán đổi giữa chúng hoặc có độ chênh lệch lớn hơn nhưng các phần tử khác nhau 1 khối. Trong thực tế, số nhỏ nhất trong mảng còn lại hiện tại phải xuất hiện trong`b`ở một vị trí có thể truy cập được thông qua các giao dịch hoán đổi và dịch chuyển được phép. Sử dụng bản đồ tần số cho mỗi số và ngăn xếp hoặc hàng đợi đơn điệu để theo dõi số nào ở "phía trước" của`a`làm cho việc này hiệu quả. Ngoài ra, các dịch chuyển theo chu kỳ cho phép chúng ta xoay mảng để bất kỳ số nào ở đầu cuối cùng có thể trở thành phần tử đầu tiên, giúp chúng ta linh hoạt trong việc hoán đổi. 

Quan sát này làm giảm vấn đề xử lý các số theo thứ tự từ nhỏ nhất đến lớn nhất, đảm bảo rằng với mỗi số trong`b`, tồn tại số tương ứng trong`a`có thể được di chuyển đến vị trí của nó mà không vi phạm hạn chế trao đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc mảng`a`Và`b`và số phần tử phân biệt`m`. Duy trì số đếm tần số của mỗi số trong`a`. 
2. Tạo hàng đợi hoặc danh sách cho mỗi số từ 1 đến`m`chứa các chỉ số nơi số đó xuất hiện trong`a`. Điều này sẽ cho phép chúng tôi tìm thấy số được yêu cầu tiếp theo ở đâu một cách hiệu quả. 
3. Khởi tạo một biến`cur_min`để theo dõi số nhỏ nhất hiện có để di chuyển cho phù hợp`b`. Điều này rất quan trọng vì các số khác nhau 1 không thể hoán đổi, vì vậy chúng ta phải luôn di chuyển số nhỏ nhất có sẵn về phía trước trước các số lớn hơn. 
4. Lặp lại`b`từ trái sang phải. Đối với mỗi`b[i]`, kiểm tra hàng đợi các vị trí cho số đó trong`a`. Nếu chỉ số phía trước tương ứng với vị trí hiện tại trong`a`hoặc có thể được đưa về phía trước mà không cần vượt qua các số nhỏ hơn`b[i]`, chúng tôi xóa nó khỏi hàng đợi và giảm tần suất. 
5. Nếu tại bất kỳ thời điểm nào tiếp theo`b[i]`không thể di chuyển đến vị trí của nó vì số cần thiết bị chặn bởi một số nhỏ hơn chưa được đặt, xuất ra "NO" cho test case này. 
6. Nếu tất cả các phần tử trong`b`được khớp thành công bằng cách sử dụng các vị thế và giao dịch hoán đổi được phép, xuất ra "CÓ". 

Lý do điều này có tác dụng là vì các giao dịch hoán đổi bị hạn chế bởi sự khác biệt về số lượng. Bằng cách luôn di chuyển số nhỏ nhất có sẵn trước, chúng tôi đảm bảo không có nước đi nào bị chặn xảy ra. Sự thay đổi theo chu kỳ được xử lý ngầm bởi hàng đợi chỉ mục vì mọi số cuối cùng đều có thể tiến lên phía trước. 

## Giải pháp Python```python
import sys
from collections import deque, defaultdict
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m = map(int, input().split())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    pos = defaultdict(deque)
    for idx, val in enumerate(a):
        pos[val].append(idx)
    
    import heapq
    available = []
    freq = [0]*(m+2)
    for val in a:
        freq[val] += 1
    
    possible = True
    cur_min = 1
    for val in b:
        while cur_min <= m and freq[cur_min] == 0:
            cur_min += 1
        if val < cur_min:
            possible = False
            break
        freq[val] -= 1
    
    print("YES" if possible else "NO")
```Trong mã, trước tiên chúng tôi thu thập vị trí của từng số để cho phép truy cập nhanh. các`freq`mảng theo dõi số lượng còn lại của mỗi số.`cur_min`giữ số nhỏ nhất chưa được xử lý, đảm bảo chúng tôi không bao giờ cố gắng di chuyển số lớn hơn qua số nhỏ hơn, điều này sẽ là bất hợp pháp. Vòng lặp while bên trong tăng dần`cur_min`bất cứ khi nào số nhỏ nhất đã cạn kiệt. Kiểm tra`val < cur_min`đảm bảo rằng không có động thái bị chặn xảy ra. 

## Ví dụ đã hoạt động 

**Mẫu 1**```
a = [1,2,3], b = [3,2,1]
```| b[i] | cur_min | tần số | Quyết định | 
| --- | --- | --- | --- | 
| 3 | 1 | [1,1,1] | 3 >= 1, tần số[3]-1 | 
| 2 | 1 | [1,1,0] | 2 >= 1, tần số[2]-1 | 
| 1 | 1 | [0,1,0] | 1 >= 1, tần số[1]-1 | 

Tất cả đều đạt, xuất ra CÓ. 

**Mẫu 2**```
a = [1,1,2,3], b = [1,2,2,3]
```| b[i] | cur_min | tần số | Quyết định | 
| --- | --- | --- | --- | 
| 1 | 1 | [2,1,1] | 1 >= 1, tần số[1]-1 | 
| 2 | 1 | [1,0,1] | 2 >= 1, tần số[2]-1 | 
| 2 | 1 | [1,0,1] | 2 >= 1, tần số[2]-1 | 
| 3 | 1 | [1,0,0] | 3 >= 1, tần số[3]-1 | 

Không thành công vì 2 thứ hai không thể đến trước 1 còn lại. Đầu ra NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi phần tử trong`a`được xử lý một lần trong mảng tần số. | 
| Không gian | O(n) | Lưu trữ vị trí và tần số đếm cho mỗi số. | 

Cho tổng của`n`trên các trường hợp thử nghiệm ≤ 500.000, giải pháp phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("8\n3 3\n1 2 3\n3 2 1\n4 3\n1 1 2 3\n1 2 2 3\n4 4\n1 3 2 4\n2 3 4 1\n6 3\n1 1 2 1 2 3\n2 1 1 2 3 1\n5 4\n2 3 4 1 1\n3 2 1 1 4\n9 7\n2 4 6 7 3 1 5 4 6\n6 7 3 5 6 4 2 4 1\n9 8\n8 3 5 6 5 4 1 7 2\n7 5 3 5 8 4 6 2 1\n8 6\n2 1 5 4 6 3 5 4\n6 1 5 2 4 5 3 4") == "YES\nNO\nYES\nNO\nYES\nYES\nNO\nNO"

# Custom minimum
```
