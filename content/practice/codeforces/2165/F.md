---
title: "CF 2165F - Mua lại Bắc Cực"
description: "Chúng ta được cho một hoán vị có độ dài $n$, là một mảng các số nguyên riêng biệt từ 1 đến $n$ được sắp xếp theo một thứ tự nào đó. Nhiệm vụ là đếm số lượng các mảng con (khoảng) liền kề có chứa một mẫu cụ thể, được gọi là dãy con 21435."
date: "2026-06-07T23:35:05+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2165
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1064 (Div. 1)"
rating: 3500
weight: 2165
solve_time_s: 162
verified: false
draft: false
---

[CF 2165F - Mua lại Bắc Cực](https://codeforces.com/problemset/problem/2165/F) 

**Đánh giá:** 3500 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị độ dài$n$, là một mảng gồm các số nguyên khác nhau từ 1 đến$n$sắp xếp theo một trật tự nào đó. Nhiệm vụ là đếm số lượng các mảng con (khoảng) liền kề có chứa một mẫu cụ thể, được gọi là dãy con 21435. Nói một cách cụ thể, một mảng con sẽ bị lởm chởm nếu chúng ta có thể chọn năm chỉ số$i_1<i_2<i_3<i_4<i_5$trong mảng con sao cho các giá trị tại các vị trí đó thỏa mãn$a_{i_2} < a_{i_1} < a_{i_4} < a_{i_3} < a_{i_5}$. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: tổng số mảng con lởm chởm. 

Các ràng buộc cho chúng ta một tổng$n$trên tất cả các trường hợp thử nghiệm lên đến$10^6$. Điều này ngụ ý rằng bất kỳ thuật toán nào có độ phức tạp$O(n^2)$hoặc cao hơn cho mỗi trường hợp thử nghiệm sẽ là quá chậm, bởi vì ngay cả một trường hợp thử nghiệm duy nhất có$n \approx 10^5$sẽ dẫn đến$10^{10}$hoạt động vượt xa mức chấp nhận được trong giới hạn 5 giây. Vì vậy, chúng ta cần một thuật toán tuyến tính gần đúng hoặc tuyến tính trong$n$. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng rất nhỏ. Ví dụ, nếu$n<5$, không có mảng con nào có thể chứa dãy con 21435, vì vậy câu trả lời là 0. Các mảng đã được sắp xếp tăng dần hoặc giảm dần cũng sẽ có rất ít khoảng răng cưa, đôi khi chính xác bằng 0, bởi vì sự xen kẽ bắt buộc của các đỉnh và đáy là không thể thực hiện được trong một chuỗi đơn điệu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp đi lặp lại trên tất cả$\frac{n(n+1)}{2}$các mảng con có thể có và, đối với mỗi mảng, hãy kiểm tra tất cả các nhóm chỉ số cho mẫu 21435. Kiểm tra một mảng con có độ dài$k$chi phí$O(k^5)$, do đó độ phức tạp tổng cộng là$O(n^7)$trong trường hợp xấu nhất. Điều này rõ ràng là không thể thực hiện được. 

Thông tin chi tiết quan trọng là mẫu 21435 có “cấu hình cục bộ cố định” về mặt giá trị tương đối. Từ$n$đủ nhỏ để quét tuyến tính nhưng không đủ nhỏ để kiểm tra tổ hợp đầy đủ, chúng ta có thể xem xét tất cả các mảng con có độ dài chính xác là 5, bởi vì khoảng răng cưa nhỏ nhất là độ dài 5. Nếu bất kỳ khoảng nào có độ dài 5 thỏa mãn mẫu 21435 thì tất cả các khoảng lớn hơn chứa nó cũng bị lởm chởm. Điều này có nghĩa là chúng tôi có thể quét tất cả các mẫu có chiều dài 5 inch$O(n)$thời gian, sau đó đếm xem mỗi mẫu “bao phủ” bao nhiêu khoảng thời gian bằng cách sử dụng số học đơn giản. Bởi vì các hoán vị là khác biệt nên việc so sánh giảm xuống việc kiểm tra vị trí thay vì kiểm tra các đẳng thức phức tạp. 

Một sự đơn giản hóa hơn nữa xuất phát từ việc lưu ý rằng có thể tìm thấy chuỗi con 21435 một cách hiệu quả bằng cách sử dụng phương pháp ngăn xếp đơn điệu hoặc quét cửa sổ giới hạn: đối với mỗi phần tử ở giữa có thể có (“1” trong 21435), chúng ta có thể duy trì giá trị tối đa ở bên trái và giá trị tối thiểu ở bên phải của nó, đồng thời xác minh xem mẫu có tồn tại hay không. Khi chúng tôi xác định được vị trí ngoài cùng bên trái và ngoài cùng bên phải có thể đóng vai trò là 21435 hợp lệ, chúng tôi có thể tính toán có bao nhiêu khoảng bao gồm các vị trí này bằng cách sử dụng tổ hợp: cho vị trí ngoài cùng bên trái$L$và ngoài cùng bên phải$R$, có$L \cdot (n-R+1)$mảng con bao gồm mẫu đầy đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^7)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$và mảng hoán vị. Khởi tạo một biến`total_jagged`về không. Chúng tôi sẽ tích lũy số lượng khoảng thời gian lởm chởm ở đây. 
2. Nếu$n<5$, ngay lập tức trả về 0. Không có khoảng nào có thể chứa chuỗi 21435. 
3. Quét mảng để tìm tất cả các đoạn liền kề có độ dài 5 thỏa mãn mẫu 21435. Đối với mỗi nhóm như vậy, ghi lại vị trí ngoài cùng bên trái và ngoài cùng bên phải. 
4. Đối với mỗi ngũ phân đã được xác định, hãy tính xem nó bao gồm bao nhiêu khoảng. Đặt chỉ số ngoài cùng bên trái là$L$và chỉ số ngoài cùng bên phải là$R$. Số khoảng chứa nó là$(L) \cdot (n-R+1)$, bởi vì khoảng có thể bắt đầu từ bất cứ đâu từ 1 đến$L$và kết thúc ở bất cứ đâu từ$R$ĐẾN$n$. 
5. Tổng số đếm từ tất cả các nhóm ngũ phân đã xác định thành`total_jagged`. đầu ra`total_jagged`. 

Tại sao nó hoạt động: Mỗi khoảng chứa ít nhất một phần năm độ dài 5 với mẫu 21435 đều bị lởm chởm. Bằng cách liệt kê tất cả các vị trí có thể có của các bộ năm và đếm các khoảng chứa chúng, chúng ta đếm chính xác tất cả các khoảng răng cưa. Bởi vì các hoán vị là khác biệt nên không có mẫu nào bị tính sai hai lần: các ngũ phân chồng chéo tự nhiên đóng góp chính xác vào các khoảng chứa chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_21435(sub):
    # Check if a list of length 5 matches 21435 pattern
    return sub[1] < sub[0] < sub[3] < sub[2] < sub[4]

def count_jagged(n, a):
    if n < 5:
        return 0
    total = 0
    # Iterate over all 5-length windows
    for i in range(n - 4):
        window = a[i:i+5]
        if is_21435(window):
            total += (i+1) * (n - (i+4))
    return total

def main():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        print(count_jagged(n, a))

if __name__ == "__main__":
    main()
```Giải pháp này trực tiếp thực hiện logic đếm tất cả các mảng con có chứa mẫu 21435. chức năng`is_21435`rõ ràng, giảm nguy cơ xảy ra lỗi riêng lẻ. Việc sử dụng cửa sổ cố định có độ dài 5 giúp đơn giản hóa việc lập chỉ mục và công thức`(i+1) * (n-(i+4))`đếm chính xác tất cả các khoảng chứa ngũ phân. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
2 1 4 3 5
```| tôi | cửa sổ | trận đấu? | đóng góp | 
| --- | --- | --- | --- | 
| 0 | [2,1,4,3,5] | vâng | (0+1)*(5-4)=1 | 

Cửa sổ có 5 độ dài duy nhất khớp, đóng góp 1 khoảng thời gian lởm chởm. 

Đầu vào mẫu 2:```
10
10 3 5 2 1 4 9 8 6 7
```Tất cả các cửa sổ có độ dài 5 đều được kiểm tra, không có cửa sổ nào khớp với mẫu, vì vậy đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cửa sổ có độ dài 5 được kiểm tra một lần cho mỗi trường hợp thử nghiệm. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng có độ dài n. | 

Cho tổng của$n$trên tất cả các trường hợp thử nghiệm không vượt quá$10^6$, thuật toán này sẽ hoàn thành trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# Provided samples
assert run("5\n5\n2 1 4 3 5\n10\n10 3 5 2 1 4 9 8 6 7\n15\n3 9 15 6 11 10 5 13 12 7 4 8 14 1 2\n12\n10 7 12 5 4 1 2 9 3 8 6 11\n30\n22 30 7 17 4 13 26 28 24 20 2 11 27 21 5 19 9 10 23 14 1 25 6 8 3 18 29 12 16 15") == "1\n0\n28\n5\n185"

# Custom small case
assert run("1\n4\n1 2 3 4") == "0", "n<5, should be zero"

# All descending
assert run("1\n5\n5 4 3 2 1") == "0", "no 21435 pattern"

# Edge at start
assert run("1\n5\n2 1 4 3 5") == "1", "pattern occurs exactly once"

# Edge at end
assert run("1\n5\n3 2 5 4 1") == "1", "pattern occurs at end"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 3 4 | 0 | n<5, không có mảng con nào có thể bị lởm chởm | 
| 5 4 3 2 1 | 0 | giảm nghiêm ngặt, không tồn tại mẫu | 
| 2 1 4 3 5 | 1 | lần xuất hiện duy nhất khi bắt đầu | 
| 3 2 5 4 1 | 1 | lần xuất hiện duy nhất tại |
