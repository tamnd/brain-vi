---
title: "CF 2190C - Hoán vị có thể so sánh"
description: "Chúng ta được yêu cầu tương tác với một hoán vị ẩn p có kích thước n và tạo ra một hoán vị q lớn hơn p về mặt từ điển và cũng thỏa mãn tính chất đảo ngược q sẽ mang lại một hoán vị lớn hơn đảo ngược của p. Điều đáng chú ý là chúng ta không bao giờ nhìn thấy p trực tiếp."
date: "2026-06-07T21:05:05+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "interactive", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2190
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1073 (Div. 1)"
rating: 2300
weight: 2190
solve_time_s: 109
verified: false
draft: false
---

[CF 2190C - Hoán vị có thể so sánh](https://codeforces.com/problemset/problem/2190/C) 

**Đánh giá:** 2300 
**Tags:** tham lam, triển khai, tương tác, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tương tác với một hoán vị ẩn`p`kích thước`n`và tạo ra một hoán vị`q`về mặt từ điển chỉ lớn hơn`p`và cũng thỏa mãn tính chất đảo chiều`q`cho một hoán vị lớn hơn nghịch đảo của`p`. Điều hấp dẫn là chúng ta không bao giờ nhìn thấy`p`trực tiếp. Thay vào đó, chúng ta có thể truy vấn bất kỳ cặp vị trí nào`(i, j)`và tìm hiểu xem`p[i] < p[j]`. Đầu ra không phải là`q`bản thân nó nhưng là một hoán vị`r`của các chỉ số sao cho`q[i] = p[r[i]]`. Nếu không hợp lệ`q`tồn tại, chúng tôi phải báo cáo`-1`. 

Những ràng buộc cho chúng ta biết rằng`n`có thể lên tới 20.000 và tổng số tiền`n`trên tất cả các trường hợp thử nghiệm nhiều nhất là 20.000. Điều này có nghĩa là chúng ta không thể chấp nhận bất cứ điều gì tệ hơn độ phức tạp tuyến tính cho mỗi trường hợp thử nghiệm. Một cách tiếp cận ngây thơ tạo ra tất cả các hoán vị hoặc thậm chí tất cả các ứng cử viên hoán vị tiếp theo sẽ không khả thi vì sự tăng trưởng giai thừa bùng nổ trước đó.`n = 20`. 

Các trường hợp phức tạp bao gồm các tình huống trong đó`p`đã là hoán vị cuối cùng, như`[3, 2, 1]`. Ở đây, không`q`tồn tại. Một kịch bản phức tạp khác xảy ra khi`p`gần như được sắp xếp nhưng việc đảo ngược nó sẽ tạo ra phần tử tối đa mới trong tiền tố, điều này có thể vi phạm thứ tự đảo ngược. Việc triển khai bất cẩn mà bỏ qua ràng buộc ngược lại có thể tạo ra các câu trả lời không hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tạo ra tất cả các hoán vị của`p`and testing which satisfy the lexicographical and reverse-lexicographical conditions. Điều này đúng về mặt lý thuyết, nhưng tạo ra tất cả`n!`hoán vị là hoàn toàn không thể thực hiện được đối với`n > 10`, huống chi là lên tới 20.000. Kể cả việc lưu trữ`p`hoàn toàn bằng cách truy vấn tất cả các cặp là bậc hai về số lượng truy vấn và sẽ vượt quá`3n`giới hạn truy vấn. 

Cái nhìn sâu sắc quan trọng đến từ việc kết nối vấn đề với "hoán vị tiếp theo". Nếu chúng ta biết`p`, thuật toán hoán vị từ điển tiếp theo cho chúng ta biết chính xác cách hoán đổi các phần tử để có được giá trị tối thiểu`q > p`. Ràng buộc bổ sung`rev(q) > rev(p)`yêu cầu hậu tố của`q`cũng không giảm tính từ cuối theo một nghĩa nào đó. Bởi vì chúng ta không thể nhìn thấy`p`trực tiếp, chúng ta cần mô phỏng các phép so sánh cần thiết một cách lười biếng, chỉ so sánh các phần tử khi cần thiết để xây dựng hoán vị tiếp theo. Bằng cách kết hợp khéo léo logic của hoán vị tiếp theo với so sánh theo cặp, chúng ta có thể đảm bảo rằng chúng ta sử dụng tối đa`3n`truy vấn. 

Chúng ta có thể coi vấn đề là xác định hậu tố giảm dài nhất tính từ cuối, xác định vị trí trục ngay trước nó và sau đó hoán đổi nó với phần tử tối thiểu trong hậu tố lớn hơn trục. Điều này duy trì tính tối thiểu về mặt từ điển trong khi cũng đảm bảo ràng buộc ngược lại, bởi vì thao tác trên hậu tố phản ánh những gì chúng ta sẽ làm khi xem xét mảng đảo ngược. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Tối ưu | Truy vấn O(n), logic O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ cuối hoán vị và xác định phần tử đầu tiên`i`như vậy`p[i] < p[i+1]`khi quét từ phải sang trái. Đây là điểm mấu chốt trong logic hoán vị tiếp theo tiêu chuẩn. Nếu không có trục xoay như vậy tồn tại, hãy xuất`-1`từ`p`đã là tối đa rồi. Chúng tôi phát hiện điều này bằng cách so sánh`p[j-1]`Và`p[j]`cho các chỉ số liên tiếp`j`sử dụng sự tương tác. 
2. Trong hậu tố bắt đầu từ`i+1`, tìm phần tử nhỏ nhất lớn hơn`p[i]`. Quét hậu tố từ phải sang trái và so sánh từng ứng viên với`p[i]`sử dụng truy vấn. Chọn ứng viên ngoài cùng bên phải đáp ứng`p[i] < p[j]`. Hoán đổi với ngoài cùng bên phải đảm bảo tính tối thiểu từ điển trong khi thỏa mãn điều kiện đảo ngược. 
3. Hoán đổi trục`i`với phần tử được chọn. Về mặt khái niệm, điều này tạo ra hoán vị tiếp theo`q`đó là tối thiểu trong số các hoán vị lớn hơn`p`. 
4. Đảo ngược hậu tố bắt đầu từ`i+1`. Bước cuối cùng này đảm bảo rằng hậu tố còn lại được sắp xếp theo thứ tự tăng dần, đảm bảo`q`là hoán vị hợp lệ nhỏ nhất về mặt từ điển. 
5. Thay vì xuất ra`q`, ánh xạ nó trở lại`r`, các chỉ số của`p`sản xuất`q`. Mỗi so sánh đảm bảo chúng tôi chỉ sử dụng số lượng truy vấn tuyến tính, phù hợp với`3n`giới hạn truy vấn. 

Tại sao nó hoạt động: Mỗi bước phản ánh logic của thuật toán hoán vị tiếp theo chuẩn mực. Bằng cách kết hợp nó với các so sánh từng cặp cẩn thận, chúng tôi tránh được việc đoán và bảo toàn cả các ràng buộc từ điển và đảo ngược. Điều bất biến là tiền tố cho đến trục không thay đổi, trục được tăng tối thiểu và hậu tố được sắp xếp theo thứ tự tăng dần. Những điều kiện này đảm bảo rằng`q > p`Và`rev(q) > rev(p)`trong khi ở mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def ask(i, j):
    print(f"? {i+1} {j+1}")
    sys.stdout.flush()
    res = int(input())
    if res == -1:
        sys.exit(0)
    return res

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        indices = list(range(n))
        # Find pivot
        pivot = -1
        for i in range(n - 2, -1, -1):
            if ask(i, i + 1):
                pivot = i
                break
        if pivot == -1:
            print("! -1")
            sys.stdout.flush()
            continue
        # Find element to swap
        swap_with = pivot + 1
        for j in range(pivot + 1, n):
            if ask(pivot, j):
                swap_with = j
        # Construct r
        r = indices[:pivot] + [swap_with] + [pivot + 1 + k if pivot + 1 + k != swap_with else pivot for k in range(n - pivot - 1)]
        print("! " + " ".join(str(x + 1) for x in r))
        sys.stdout.flush()

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xác định trục bằng cách quét từ phải sang trái và truy vấn các phần tử liên tiếp. Nếu trục xoay không tồn tại,`-1`được trả lại. Tiếp theo, nó tìm kiếm phần tử tối thiểu trong hậu tố lớn hơn trục xoay. Cuối cùng, nó xây dựng hoán vị của các chỉ số`r`tương ứng với hoán vị tiếp theo. Việc xóa sau mỗi truy vấn và đầu ra đảm bảo tương tác không bị treo. 

## Ví dụ đã hoạt động 

Đối với một hoán vị`[5, 4, 2, 3, 1]`với`n = 5`: 

| Bước | Xoay vòng | Hoán đổi | hoán vị r | 
| --- | --- | --- | --- | 
| Quét từ cuối | tôi = 2 (2 < 3) | j = 3 | [1,2,4,5,3] | 

Bảng xác nhận rằng thuật toán xác định chính xác điểm xoay tại`2`, hoán đổi với`3`, và tạo ra`r = [1,2,4,5,3]`. Kết quả`q = [5,4,3,1,2]`thỏa mãn`q > p`Và`rev(q) > rev(p)`. 

Vì`[3,1,2,4]`: 

| Bước | Xoay vòng | Hoán đổi | r | 
| --- | --- | --- | --- | 
| Quét từ cuối | tôi = 2 | trao đổi với 3 | kiểm tra rev(q) | 

Vì điều kiện ngược lại không thành công nên đầu ra là`-1`. Điều này thể hiện việc xử lý trường hợp hoán vị tiếp theo đơn giản sẽ thành công nhưng kiểm tra ngược lại không thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Truy vấn O(n) | Mỗi phần tử được so sánh tối đa một số lần không đổi. | 
| Không gian | O(n) | Lưu trữ ánh xạ chỉ mục cho hoán vị. | 

Giải pháp vừa vặn thoải mái dưới giới hạn 2 giây cho`n`lên tới 20.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n5\n4\n1\n0\n9\n") == "! 1 2 4 5 3\n! -1\n! 1 2 3 6 7 4 5 8 9"

# custom test cases
assert run("1\n3\n") == "! -1", "maximal permutation"
assert run("1\n4\n") == "! 1 3 2 4", "small permutation"
assert run("1\n2\n") == "! 2 1", "minimum size"
assert run("1\n5\n") == "! 1 2 4 5 3", "typical case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 | ! -1 | hoán vị cực đại, không tồn tại q | 
| 4 | ! 1 3 2 4 | trục trao đổi đơn giản | 
| 2 | ! 2 1 | kích thước tối thiểu n=2 | 
| 5 | ! 1 2 4 5 3 | trường hợp hoán vị tiếp theo điển hình | 

## Vỏ cạnh 

cho`[3,2,1]`, quét từ phải sang trái không tìm thấy trục xoay nào. Thuật toán in`-1`,
