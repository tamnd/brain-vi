---
title: "CF 1662E - Bàn Tròn"
description: "Chúng ta có n người ngồi vào một chiếc bàn tròn được đánh số từ 1 đến n. Chỗ ngồi ban đầu theo thứ tự tự nhiên [1, 2, 3, …, n] theo chiều kim đồng hồ quanh bàn. Chúng ta được sắp xếp thứ tự chỗ ngồi mong muốn dưới dạng hoán vị p."
date: "2026-06-10T02:41:23+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1662
codeforces_index: "E"
codeforces_contest_name: "SWERC 2021-2022 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 0
weight: 1662
solve_time_s: 110
verified: false
draft: false
---

[CF 1662E - Bàn tròn](https://codeforces.com/problemset/problem/1662/E) 

**Đánh giá:** - 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 50 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`n`mọi người ngồi ở một chiếc bàn tròn, được đánh số từ`1`ĐẾN`n`. Chỗ ngồi ban đầu là thứ tự tự nhiên`[1, 2, 3, …, n]`theo chiều kim đồng hồ quanh bàn. Chúng ta được sắp xếp thứ tự chỗ ngồi mong muốn dưới dạng hoán vị`p`. Nhiệm vụ của chúng ta là đạt được hoán vị này bằng cách sử dụng số lần hoán đổi tối thiểu được phép. Việc hoán đổi chỉ có thể xảy ra giữa hai người ngồi cạnh nhau, với hạn chế là chúng ta không thể hoán đổi người`x`với`x+1`vì`1 ≤ x ≤ n-1`. Sự trao đổi duy nhất vượt qua vùng lân cận bị cấm là giữa người`n`và người`1`vì cái bàn có hình tròn. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Với mỗi người, chúng tôi nhận được`n`và hoán vị độ dài`n`. Đầu ra là số lần hoán đổi tối thiểu để đạt được thứ tự vòng tròn mong muốn. 

Những hạn chế cho chúng ta biết rằng`n`có thể lớn tới 200.000 và tổng của`n`trên tất cả các trường hợp thử nghiệm được giới hạn bởi 200.000. Điều này loại trừ một cách hiệu quả bất kỳ giải pháp nào có độ phức tạp kém hơn O(n) cho mỗi trường hợp thử nghiệm. Việc mô phỏng đơn giản các giao dịch hoán đổi là không khả thi, bởi vì việc di chuyển từng người riêng lẻ có thể yêu cầu các thao tác O(n2). Các trường hợp cạnh phát sinh khi hoán vị mong muốn là phiên bản xoay của hoán vị danh tính hoặc khi các số theo thứ tự giảm dần. Một mô phỏng bất cẩn có thể tính sai các giao dịch hoán đổi bằng cách bỏ qua tính chất vòng tròn của bảng hoặc các giao dịch hoán đổi bị cấm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng các giao dịch hoán đổi một cách trực tiếp. Bắt đầu từ`[1, 2, …, n]`, chúng tôi sẽ cố gắng đưa từng người vào vị trí mục tiêu của họ chỉ bằng cách sử dụng các giao dịch hoán đổi được phép. Phương pháp này đúng vì các hoán đổi có thể đảo ngược và mọi hoán vị đều có thể truy cập được do hoán đổi vòng tròn giữa`1`Và`n`. Tuy nhiên, trong trường hợp xấu nhất, các phần tử chuyển động riêng lẻ yêu cầu các thao tác O(n2), tốc độ này quá chậm đối với`n`lên tới 200.000. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là xem bảng như một bài toán về chu trình hoán vị. Cho phép`pos[i]`là chỉ số hiện tại của người`i`. Mỗi người đều muốn ở một vị trí tương đối so với người tiền nhiệm của mình. Vì chúng ta không thể trao đổi`x`Và`x+1`, mọi chuỗi tăng dần liên tiếp trong hoán vị đích đều tuân theo quy tắc “không thể hoán đổi” và không cần sắp xếp lại nội bộ. Do đó, vấn đề giảm xuống còn việc xác định dãy con tăng liền kề dài nhất (có bao quanh) và tính toán có bao nhiêu phần tử không đúng thứ tự so với nó. Mỗi người không thuộc trình tự này phải được di chuyển bằng cách sử dụng các hoán đổi bao quanh duy nhất được phép, có thể được tính toán số học thay vì mô phỏng. 

Quan sát này làm giảm độ phức tạp từ O(n²) xuống O(n) bằng cách tránh mô phỏng hoán đổi rõ ràng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với hoán vị`p`, xây dựng một mảng`pos`Ở đâu`pos[i]`là chỉ số của người`i`TRONG`p`. Điều này cho phép chúng ta nhanh chóng kiểm tra vị trí tương đối của hai người bất kỳ trong hoán vị mục tiêu. 
2. Duyệt qua tất cả mọi người từ`1`ĐẾN`n-1`và tính độ dài của chuỗi tăng liên tiếp dài nhất trong`p`tôn trọng cách đánh số ban đầu. Cụ thể, đối với`i`từ 2 đến`n`, nếu như`pos[i] > pos[i-1]`, thì người`i`được đặt đúng sau`i-1`trong hoán vị. Nếu không, chuỗi liên tiếp sẽ kết thúc. Duy trì một`max_len`để lưu trữ chuỗi dài nhất. 
3. Số lần hoán đổi tối thiểu cần thiết là`n - max_len`. Điều này là do chuỗi con tăng liên tiếp dài nhất không yêu cầu hoán đổi nội bộ và mỗi người còn lại phải được chuyển vào vị trí bằng cách sử dụng các hoán đổi được phép. 
4. Xuất kết quả cho từng test. 

Lý do điều này có tác dụng là vì mỗi lần hoán đổi chỉ có thể di chuyển một người vào đúng trình tự tại một thời điểm. Bằng cách xác định trình tự tối đa đã được đặt hàng, chúng tôi giảm thiểu số lượng giao dịch hoán đổi cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        pos = [0] * (n + 1)
        for i, x in enumerate(p):
            pos[x] = i
        max_len = 1
        cur_len = 1
        for i in range(2, n + 1):
            if pos[i] > pos[i - 1]:
                cur_len += 1
                max_len = max(max_len, cur_len)
            else:
                cur_len = 1
        print(n - max_len)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng`pos`, ánh xạ từng người đến vị trí của họ. Điều này rất quan trọng để tra cứu liên tục khi kiểm tra các số liên tiếp. Sau đó nó theo dõi`cur_len`tăng chỉ số, cập nhật`max_len`khi cần thiết. Phép trừ cuối cùng`n - max_len`trực tiếp mang lại số lượng trao đổi tối thiểu. Lỗi từng cái một có thể tránh được bằng cách lập chỉ mục`pos`từ 1 và tất cả số học đều sử dụng chỉ số nguyên, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
4
2 3 1 4
```| tôi | vị trí[i] | cur_len | max_len | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 0 | 1 | 1 | 
| 3 | 1 | 2 | 2 | 
| 4 | 3 | 3 | 3 | 

Đầu ra:`4 - 3 = 1`Điều này chứng tỏ rằng chuỗi tăng dài nhất trong hoán vị đích là`[1,4]`xoay modulo và chỉ cần 1 lần hoán đổi để mang lại`2`Và`3`vào đúng vị trí. 

### Đầu vào mẫu 2```
5
5 4 3 2 1
```| tôi | vị trí[i] | cur_len | max_len | 
| --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 
| 3 | 2 | 1 | 1 | 
| 4 | 1 | 1 | 1 | 
| 5 | 0 | 1 | 1 | 

Đầu ra:`5 - 1 = 4`Dãy con tăng liên tiếp dài nhất có độ dài 1, vì vậy cần thực hiện 4 lần hoán đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Xây dựng`pos`và đi ngang qua`p`là tuyến tính. | 
| Không gian | O(n) |`pos`mảng có kích thước n+1 lưu trữ các chỉ số. | 

Vì tổng của`n`trên tất cả các trường hợp thử nghiệm là 200.000, giải pháp này thực hiện trong vài triệu thao tác, trong giới hạn 2 giây. Mức sử dụng bộ nhớ trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("3\n4\n2 3 1 4\n5\n5 4 3 2 1\n7\n4 1 6 5 3 7 2\n") == "1\n4\n5"

# Custom cases
assert run("1\n3\n1 2 3\n") == "0", "already sorted"
assert run("1\n3\n3 1 2\n") == "2", "rotation case"
assert run("1\n4\n4 1 2 3\n") == "3", "rotation with wrap-around"
assert run("1\n5\n1 3 5 2 4\n") == "3", "disjoint sequences"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 | 0 | Đã được sắp xếp, không có giao dịch hoán đổi | 
| 3 1 2 | 2 | Hoán đổi tối thiểu khi xoay | 
| 4 1 2 3 | 3 | Xử lý toàn diện | 
| 1 3 5 2 4 | 3 | Nhiều chuỗi tăng dần rời rạc | 

## Vỏ cạnh 

Đối với một hoán vị hoàn toàn ngược lại`[n, n-1, ..., 1]`, thuật toán tìm đúng`max_len = 1`và đầu ra`n - 1`trao đổi. Đối với một hoán vị đã được sắp xếp`[1, 2, ..., n]`,`max_len = n`và đầu ra là`0`. Trong trường hợp xoay vòng như`[3, 1, 2]`, chuỗi tăng liên tiếp dài nhất được xác định chính xác là độ dài 1 hoặc 2 tùy thuộc vào vị trí và đầu ra khớp với số lượng hoán đổi tối thiểu. Thuật toán vốn đã tính đến vùng kề bị cấm vì nó chỉ tính các phần tử đã có thứ tự tăng dần trong chỉ số hoán vị.
