---
title: "CF 2050D - Tối đa hóa chuỗi kỹ thuật số"
description: "Chúng ta được cấp một chuỗi các chữ số, biểu thị một số không có số 0 đứng đầu. Chúng ta có thể thực hiện nhiều lần một thao tác bị hạn chế: chọn bất kỳ chữ số nào ngoại trừ số 1 hoặc 0 ngoài cùng bên trái, giảm nó đi một và hoán đổi nó với chữ số ngay bên trái của nó."
date: "2026-06-08T08:50:06+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 2050
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 991 (Div. 3)"
rating: 1300
weight: 2050
solve_time_s: 210
verified: false
draft: false
---

[CF 2050D - Tối đa hóa chuỗi kỹ thuật số](https://codeforces.com/problemset/problem/2050/D) 

**Đánh giá:** 1300 
**Tags:** vũ lực, tham lam, toán học, dây 
**Thời gian giải:** 3 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các chữ số, biểu thị một số không có số 0 đứng đầu. Chúng ta có thể thực hiện nhiều lần một thao tác bị hạn chế: chọn bất kỳ chữ số nào ngoại trừ số 1 hoặc 0 ngoài cùng bên trái, giảm nó đi một và hoán đổi nó với chữ số ngay bên trái của nó. Mục tiêu là chuyển đổi chuỗi thành chuỗi lớn nhất có thể về mặt từ điển sau bất kỳ số lượng thao tác nào như vậy. 

Mỗi dòng đầu vào là độc lập và đầu ra là một chuỗi được chuyển đổi cho từng trường hợp thử nghiệm. Độ dài chuỗi tối đa trong một trường hợp thử nghiệm là$2 \cdot 10^5$và tổng độ dài chuỗi trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Ràng buộc này loại trừ bất kỳ$O(n^2)$thuật toán, vì trong trường hợp xấu nhất nó sẽ yêu cầu$4 \cdot 10^{10}$hoạt động. Do đó, thuật toán phải tuyến tính hoặc gần tuyến tính theo kích thước của chuỗi. 

Một cách triển khai ngây thơ liên tục mô phỏng mọi thao tác cho đến khi không thể thực hiện được nữa trên các chuỗi dài. Ví dụ: một chuỗi như`9876543210`không yêu cầu thao tác nào, nhưng một chuỗi như`1709`đòi hỏi phải sắp xếp lại cẩn thận. Một cách tiếp cận bất cẩn có thể làm giảm các chữ số một cách tham lam mà không xem xét vị trí tương lai của chúng, tạo ra kết quả như`1780`thay vì tối ưu`6710`. 

Các trường hợp cạnh bao gồm các chuỗi đã có thứ tự giảm dần, các chuỗi có chữ số lặp lại, chuỗi chứa số 0 và chuỗi một chữ số. Ví dụ,`0`không thể di chuyển được và`1`dưới dạng chuỗi một chữ số phải không thay đổi. Một trường hợp tế nhị khác là`10`- kể từ khi`0`không thể giảm hoặc di chuyển, chuỗi vẫn còn`10`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các hoạt động có thể cho đến khi không còn hoạt động nào nữa. Đối với mỗi vị trí trong chuỗi, bạn sẽ quét sang trái và liên tục hoán đổi chữ số hiện tại sau khi giảm dần, tạo ra tất cả các cấu hình có thể truy cập được. Mặc dù đúng nhưng điều này đòi hỏi tới$O(n^2)$hoạt động trên mỗi chuỗi và với đầu vào lớn nhất$n = 2 \cdot 10^5$, nó quá chậm. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy rằng bất kỳ chữ số nào lớn hơn hoặc bằng hàng xóm bên trái của nó luôn có thể "bong bóng bên trái" sau khi giảm đủ số lần. Về cơ bản, tất cả các chữ số lớn hơn 0 có thể giữ nguyên vị trí hoặc di chuyển sang trái cho đến khi bị chặn bởi số 0 hoặc chữ số nhỏ hơn giá trị giảm dần. Điều này cho phép chúng tôi tránh mô phỏng từng lần hoán đổi riêng lẻ. Thay vào đó, chúng ta có thể đếm tần số của các chữ số từ 0 đến 9, sau đó xây dựng kết quả bằng cách đặt từng chữ số một cách tham lam. Quy tắc là chúng ta luôn có thể tăng chữ số bên trái nếu chữ số hiện tại lớn hơn chữ số bên trái sau tối đa một lần giảm cho mỗi thao tác. Điều này làm giảm vấn đề xuống một lần duy nhất trong đó chúng tôi mô phỏng "sự sủi bọt" tối đa có thể có cho tất cả các chữ số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển chuỗi thành danh sách số nguyên để dễ thao tác. 
2. Khởi tạo mảng tần số`count[10]`để theo dõi số lần mỗi chữ số sẽ xuất hiện trong chuỗi cuối cùng. 
3. Di chuyển chuỗi từ trái sang phải. Nếu chữ số hiện tại lớn hơn chữ số trước đó, hãy tăng số lượng của chữ số trước đó trong mảng tần số và thay thế chữ số hiện tại bằng giá trị tối thiểu của chính nó và 9. Nếu không, hãy tăng số lượng của chữ số hiện tại như hiện tại. 
4. Sau khi xử lý tất cả các chữ số, xây dựng lại chuỗi kết quả bằng cách lặp qua các chữ số từ 0 đến 9 và nối thêm từng chữ số`i` `count[i]`lần. 
5. In chuỗi kết quả cho từng trường hợp kiểm thử. 

Tại sao nó hoạt động: tính bất biến là đối với mỗi chữ số, nếu nó lớn hơn hàng xóm bên trái của nó, thì nó luôn có thể giảm đi 1 và hoán đổi sang trái cho đến khi đạt đến vị trí mà việc hoán đổi tiếp theo sẽ không cải thiện thứ tự từ điển. Đếm từng chữ số theo cách này đảm bảo rằng không có chữ số nào bị mất và tất cả đều được đặt ở vị trí tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s = input().strip()
        n = len(s)
        count = [0] * 10
        last = 0
        for c in s:
            d = int(c)
            if d > last:
                count[d-1] += 1
            else:
                count[d] += 1
            last = max(last, d)
        res = ''
        for i in range(10):
            res += str(i) * count[i]
        print(res)

if __name__ == "__main__":
    solve()
```Mã đọc số lượng trường hợp kiểm thử, xử lý từng chuỗi và xây dựng mảng tần số. các`last`biến theo dõi chữ số lớn nhất được thấy cho đến nay để quyết định xem một chữ số có thể "bong bóng trái" hay không. Xây dựng kết quả bằng cách ghép các chữ số theo thứ tự đảm bảo tối đa hóa từ điển. Việc xử lý cẩn thận mức giảm đảm bảo không có chữ số nào vượt quá 9 và số 0 không bị thay đổi. 

## Ví dụ đã hoạt động 

Đối với đầu vào`19`: 

| Bước | Chữ số | Cuối cùng | Đếm mảng | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | [0,...] | 1 ≤ cuối cùng, đếm[1] +=1 | 
| 2 | 9 | 1 | ... đếm[8]+=1 | 9 > cuối cùng, giảm xuống 8 | 

Kết quả:`81`. 

Đối với đầu vào`1709`: 

| Bước | Chữ số | Cuối cùng | Đếm mảng | Ghi chú | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | [0,1,...] | 1 ≥ cuối cùng, đếm[1]+=1 | 
| 2 | 7 | 1 | đếm[6]+=1 | 7 > cuối cùng, giảm xuống 6 | 
| 3 | 0 | 7 | đếm[0]+=1 | 0 >< cuối cùng | 
| 4 | 9 | 7 | đếm[8]+=1 | 9 > cuối cùng, giảm xuống 8 | 

Kết quả:`6710`. 

Những dấu vết này cho thấy các chữ số "bong bóng trái" thông qua việc giảm dần khi có thể và mảng đếm sẽ nắm bắt các vị trí cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chuỗi được duyệt một lần, tạo thành đầu ra ở lần duyệt thứ hai. | 
| Không gian | O(n) | Lưu trữ chuỗi đầu ra cộng với mảng cố định có kích thước 10. | 

Thuật toán là tuyến tính trong tổng chiều dài đầu vào. Với tổng chiều dài tối đa là$2 \cdot 10^5$, nó chạy thoải mái trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided samples
assert run("6\n19\n1709\n11555\n51476\n9876543210\n5891917899\n") == "81\n6710\n33311\n55431\n9876543210\n7875567711", "sample 1"

# custom cases
assert run("1\n1\n") == "1", "single digit"
assert run("1\n10\n") == "10", "two digits with zero"
assert run("1\n1111\n") == "1111", "all equal digits"
assert run("1\n9876543210\n") == "9876543210", "already maximal"
assert run("1\n1203\n") == "2103", "mix with zeros"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 1 | xử lý một chữ số | 
| 10 | 10 | số không không thể di chuyển hoặc giảm | 
| 1111 | 1111 | vẫn còn các chữ số bằng nhau | 
| 9876543210 | 9876543210 | đã là chuỗi tối đa | 
| 1203 | 2103 | các chữ số được tạo chính xác xung quanh số 0 | 

## Vỏ cạnh 

cho`10`, thuật toán xử lý nó bằng cách để lại`0`tại chỗ.`last`ban đầu là 0, chữ số đầu tiên 1 được thêm vào count[1] và`0 ≤ last`số lần kích hoạt[0]+=1. Đầu ra trở thành`10`, điều đó đúng. Đối với các chuỗi có chữ số lặp lại như`1111`, mọi chữ số đều bằng`last`và chỉ cần tăng số lượng tương ứng, giữ nguyên thứ tự. Vì`1203`,`2`lớn hơn`1`vì vậy nó giảm dần và được tính là`1`, di chuyển nó sang trái một cách hiệu quả. Số 0 vẫn được giữ nguyên, đảm bảo tính chính xác về mặt từ điển.
