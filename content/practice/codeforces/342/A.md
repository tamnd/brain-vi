---
title: "CF 342A - Xenia và các ước số"
description: "Chúng ta được cho một dãy các số nguyên dương, tất cả đều nằm trong khoảng từ 1 đến 7 và độ dài của dãy đó chia hết cho ba."
date: "2026-06-06T17:34:32+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 342
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 199 (Div. 2)"
rating: 1200
weight: 342
solve_time_s: 96
verified: true
draft: false
---

[CF 342A - Xenia và Ước số](https://codeforces.com/problemset/problem/342/A) 

**Đánh giá:** 1200 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên dương, tất cả đều nằm trong khoảng từ 1 đến 7 và độ dài của dãy đó chia hết cho ba. Xenia muốn phân chia chuỗi này thành các nhóm gồm ba số sao cho trong mỗi nhóm, các số tăng lên một cách chặt chẽ và mỗi số chia hết cho số tiếp theo. Đầu ra là danh sách các bộ ba này hoặc -1 nếu không tồn tại phân vùng như vậy. 

Các ràng buộc cho chúng ta biết rằng n có thể lớn bằng 99999, loại trừ bất kỳ thuật toán nào thử tất cả các bộ ba có thể có trong chuỗi, bởi vì điều đó sẽ liên quan đến việc chọn kết hợp n phần tử ba phần tử cùng một lúc và kiểm tra tính chia hết, dẫn đến các phép toán gần như n³. Vì n³ ở mức 10¹⁵ với n gần 10⁵ nên điều này hoàn toàn không khả thi. Chúng ta cần một cách tiếp cận tuyến tính hoặc nhiều nhất là O(n). 

Các trường hợp cạnh không rõ ràng phát sinh vì không phải tất cả các số từ 1 đến 7 đều có thể cùng tồn tại trong một bộ ba hợp lệ. Ví dụ: ba số 1 không thể tạo thành bộ ba vì 1,1,1 không tăng một cách nghiêm ngặt. Tương tự, một số như 5 không thể xuất hiện trong bất kỳ bộ ba nào vì không có dãy nào có dạng a chia b chia c kết thúc bằng 5 chỉ sử dụng các số từ 1 đến 7. Một trường hợp phức tạp khác là có sự không khớp về số đếm: nếu chúng ta có nhiều hơn 2 số 1, chúng ta không thể tạo thành bộ ba 1,2,x cho tất cả 2 số. 

Một ví dụ thất bại cụ thể là đầu vào`6\n1 1 1 2 2 2`. Số 1 và số 2 không thể kết hợp để thỏa mãn điều kiện tăng và chia hết nghiêm ngặt, do đó kết quả đúng là -1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các kết hợp của ba phần tử và kiểm tra xem các điều kiện a<b<c và a chia b chia c có đúng hay không. Đối với mỗi bộ ba hợp lệ, chúng tôi sẽ đánh dấu các phần tử là đã sử dụng và tiếp tục cho đến khi tất cả các phần tử được nhóm lại. Cách tiếp cận này đúng về nguyên tắc, nhưng nó liên quan đến việc chọn kết hợp n phần tử ba phần tử cùng một lúc, đó là O(n³), quá chậm đối với n lên tới 10⁵. 

Quan sát quan trọng là các số được giới hạn trong phạm vi 1-7. Điều đó làm cho tổng số các số khác nhau rất nhỏ. Chúng ta có thể đếm số lần xuất hiện của mỗi số, sau đó cố gắng chỉ tập hợp các bộ ba từ các chuỗi hợp lệ. Bằng cách liệt kê tất cả các bộ ba có thể thỏa mãn các ràng buộc (1,2,4), (1,2,6), (1,3,6), chúng ta thấy rằng chỉ những kết hợp này mới có thể xuất hiện. Nếu số lượng của bất kỳ số nào không khớp với số lượng yêu cầu trên các bộ ba này thì không thể có giải pháp. 

Do đó, cách tiếp cận tối ưu là phương pháp đếm tham lam: đếm số lần xuất hiện từ 1 đến 7, kiểm tra xem số đếm có cho phép hình thành các bộ ba cần thiết hay không, sau đó xuất ra các bộ ba bằng cách sử dụng số đếm. Điều này làm giảm độ phức tạp xuống còn O(n) vì chúng ta chỉ cần duyệt qua danh sách một lần để đếm, sau đó xuất ra các bộ ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| Đếm + Tham Lam | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem mỗi số nguyên từ 1 đến 7 xuất hiện trong dãy bao nhiêu lần. Chúng tôi sẽ lưu trữ cái này trong một mảng`cnt`Ở đâu`cnt[i]`đại diện cho số lượng của số i. 
2. Kiểm tra những số không thể. Nếu bất kỳ số nào xuất hiện không phải là một phần của bộ ba hợp lệ, như 5 hoặc 7, chúng ta có thể trả về -1 ngay lập tức. 
3. Xác nhận số lượng đối với các bộ ba khả thi. Các bộ ba hợp lệ duy nhất là (1,2,4), (1,2,6) và (1,3,6). Tổng số đếm của 1 phải bằng n/3 và mỗi số 1 phải được sử dụng chính xác một lần. Tương tự, số đếm 2, 3, 4 và 6 phải thỏa mãn mối quan hệ mà bộ ba ngụ ý: 

- Mỗi 2 là một phần của (1,2,4) hoặc (1,2,6). 
- Mỗi 3 phải được ghép nối với 6 in (1,3,6). 
- Số đếm 4 và 6 phải đủ để bao gồm tất cả các bộ ba. 
- 5 và 7 không hợp lệ. 
4. Xây dựng bộ ba bằng cách sử dụng phương pháp tham lam. Đầu tiên hãy tạo càng nhiều (1,2,4) bộ ba càng tốt bằng cách sử dụng`min(cnt[1], cnt[2], cnt[4])`, sau đó tạo thành bộ ba (1,2,6) với các số 1, 2 và 6 còn lại, sau đó tạo thành các bộ ba (1,3,6) với các số 1, 3 và 6 còn lại. 
5. Nếu sau khi hình thành bộ ba, bất kỳ số đếm nào vẫn khác 0, hãy trả về -1. Ngược lại, xuất ra tất cả các bộ ba. 

Điều bất biến là ở mỗi bước, chúng ta chỉ tạo thành một bộ ba hợp lệ với các số có sẵn, đảm bảo mỗi số được sử dụng chính xác một lần. Bởi vì số lượng xác định đầy đủ liệu một phân vùng có tồn tại hay không, điều này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

cnt = [0] * 8
for x in a:
    cnt[x] += 1

# impossible numbers
if cnt[5] > 0 or cnt[7] > 0:
    print(-1)
    sys.exit()

triplets = []

# form (1,2,4) as much as possible
k = min(cnt[1], cnt[2], cnt[4])
for _ in range(k):
    triplets.append((1,2,4))
    cnt[1] -= 1
    cnt[2] -= 1
    cnt[4] -= 1

# form (1,2,6)
k = min(cnt[1], cnt[2], cnt[6])
for _ in range(k):
    triplets.append((1,2,6))
    cnt[1] -= 1
    cnt[2] -= 1
    cnt[6] -= 1

# form (1,3,6)
k = min(cnt[1], cnt[3], cnt[6])
for _ in range(k):
    triplets.append((1,3,6))
    cnt[1] -= 1
    cnt[3] -= 1
    cnt[6] -= 1

# if any leftover numbers, impossible
if sum(cnt) != 0:
    print(-1)
else:
    for t in triplets:
        print(*t)
```Phần đầu tiên đếm số lần xuất hiện và ngay lập tức từ chối mọi số không hợp lệ. Các bước tham lam đảm bảo rằng chúng ta tạo thành các bộ ba theo một trình tự hợp lệ mà không để lại phần thừa. Việc kiểm tra tổng số còn lại sẽ xác nhận rằng không có số nào bị bỏ chỉ định. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
6
1 1 1 2 2 2
```| Bước | cnt[1] | cnt[2] | cnt[3] | cnt[4] | cnt[6] | Sinh ba | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | 3 | 3 | 0 | 0 | 0 | [] | 
| Mẫu (1,2,4) | 3 | 3 | 0 | 0 | 0 | 0 bộ ba, không thể hình thành | 
| Mẫu (1,2,6) | 3 | 3 | 0 | 0 | 0 | 0 bộ ba, không thể hình thành | 
| Mẫu (1,3,6) | 3 | 3 | 0 | 0 | 0 | 0 bộ ba, không thể hình thành | 
| Số tiền còn lại | 6 | => -1 | | | | | 

Điều này chứng tỏ rằng khi thiếu số lượng cần thiết cho các bộ ba hợp lệ, thuật toán sẽ phát hiện chính xác những điều không thể xảy ra. 

Đầu vào tùy chỉnh 2:```
6
1 1 2 2 4 6
```| Bước | cnt[1] | cnt[2] | cnt[4] | cnt[6] | Sinh ba | 
| --- | --- | --- | --- | --- | --- | 
| Ban đầu | 2 | 2 | 1 | 1 | [] | 
| Mẫu (1,2,4) | 1 | 1 | 0 | 1 | [(1,2,4)] | 
| Mẫu (1,2,6) | 0 | 0 | 0 | 1 | [(1,2,4),(1,2,6)] | 
| Số tiền còn lại | 0 | hợp lệ | | | | 

Bảng cho thấy thuật toán tạo thành bộ ba một cách tham lam mà không để lại phần thừa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đếm từng phần tử một lần, sau đó hình thành các bộ ba dựa trên số lượng lấy tổng O(n) | 
| Không gian | O(1) | Mảng đếm có kích thước 8 và mảng bộ ba lưu trữ n/3 bộ ba | 

Với n ≤ 99999, giải pháp tuyến tính này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided sample
assert run("6\n1 1 1 2 2 2\n") == "-1", "sample 1"

# Custom cases
assert run("6\n1
```
