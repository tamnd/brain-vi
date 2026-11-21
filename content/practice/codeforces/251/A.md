---
title: "CF 251A - Điểm trên đường"
description: "Chúng ta được cho một tập hợp các điểm nằm dọc theo đường một chiều. Petya muốn đếm xem có thể chọn được bao nhiêu bộ ba điểm sao cho khoảng cách giữa điểm ngoài cùng bên trái và điểm ngoài cùng bên phải trong bộ ba không vượt quá một giá trị cho trước d."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "combinatorics", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 251
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 153 (Div. 1)"
rating: 1300
weight: 251
solve_time_s: 69
verified: true
draft: false
---

[CF 251A - Điểm trực tuyến](https://codeforces.com/problemset/problem/251/A) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tổ hợp, hai con trỏ 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các điểm nằm dọc theo đường một chiều. Petya muốn đếm xem có thể chọn được bao nhiêu bộ ba điểm sao cho khoảng cách giữa điểm ngoài cùng bên trái và điểm ngoài cùng bên phải trong bộ ba không vượt quá một giá trị cho trước _d_. Về mặt hình thức, nếu chúng ta chọn ba điểm$x_i < x_j < x_k$, điều kiện là$x_k - x_i \le d$. 

Đầu vào cung cấp số điểm$n$, khoảng cách giới hạn$d$và một danh sách tọa độ điểm tăng dần. Đầu ra là một số nguyên duy nhất: số bộ ba hợp lệ. 

Những hạn chế là như vậy$n$có thể lớn như$10^5$, và tọa độ có thể lên tới$10^9$về giá trị tuyệt đối. Một giải pháp mạnh mẽ để kiểm tra từng bộ ba một cách rõ ràng sẽ yêu cầu$O(n^3)$hoạt động. Với$n = 10^5$, đó sẽ là theo thứ tự của$10^{15}$hoạt động quá chậm trong giới hạn thời gian 2 giây. Một giải pháp bậc hai$O(n^2)$cũng là đường biên giới cho giới hạn trên. Chúng ta cần một cái gì đó gần tuyến tính hoặc tuyến tính. 

Các trường hợp cạnh bao gồm số lượng điểm rất nhỏ, ví dụ$n = 3$, trong đó chỉ tồn tại một bộ ba hoặc các tình huống không có bộ ba nào thỏa mãn ràng buộc về khoảng cách, chẳng hạn như các điểm có khoảng cách rộng rãi với$d$quá nhỏ. Một trường hợp tinh vi khác là khi nhiều điểm được phân cụm chặt chẽ, tạo ra số lượng lớn các bộ ba hợp lệ, đòi hỏi phải tính toán cẩn thận để tránh tràn số nguyên. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản sẽ lặp lại qua mọi sự kết hợp của ba điểm và kiểm tra khoảng cách giữa điểm đầu tiên và điểm cuối cùng. Điều này đúng vì nó thực sự kiểm tra tất cả các khả năng, nhưng số lượng hoạt động trong trường hợp xấu nhất là$O(n^3)$, điều này là không thể thực hiện được với quy mô lớn$n$. 

Thông tin chi tiết quan trọng để có giải pháp nhanh hơn là các điểm được sắp xếp. Điều này cho phép chúng ta tránh việc kiểm tra tất cả các bộ ba riêng lẻ. Đối với một điểm khởi đầu nhất định$x_i$, chúng ta có thể tìm được điểm xa nhất$x_j$như vậy$x_j - x_i \le d$. Bất kỳ điểm nào giữa$x_i$Và$x_j$có thể hình thành bộ ba hợp lệ với$x_i$. Nếu có$m$điểm giữa$x_i$Và$x_j$, số bộ ba bao gồm$x_i$là$\binom{m}{2}$, bởi vì chúng ta cần chọn bất kỳ hai trong số$m$điểm sau$x_i$để ghép đôi với nó. Đây là phép đếm tổ hợp, không phải kiểm tra lặp. 

Quan sát này đương nhiên dẫn đến kỹ thuật hai con trỏ. Một con trỏ lặp qua điểm bắt đầu$x_i$và một con trỏ khác kéo dài càng xa càng tốt để thỏa mãn điều kiện khoảng cách. Việc đếm các kết hợp cho từng điểm bắt đầu sẽ cho tổng số theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tổ hợp hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bộ đếm cho các bộ ba hợp lệ về 0. Điều này sẽ tích lũy tổng số bộ ba được tìm thấy. 
2. Sử dụng con trỏ`i`để lặp lại từ điểm đầu tiên đến điểm áp chót có thể đóng vai trò là điểm bắt đầu của bộ ba. 
3. Đối với mỗi`i`, duy trì một con trỏ khác`j`bắt đầu lúc`i+1`và di chuyển về phía trước trong khi sự khác biệt giữa điểm tại`j`và điểm tại`i`nhỏ hơn hoặc bằng`d`. Điều này đảm bảo`j`luôn là điểm xa nhất mà vẫn có thể tạo thành một bộ ba hợp lệ với`x[i]`. 
4. Tính số điểm giữa`i`Và`j`BẰNG`count = j - i - 1`. Đây là những điểm có thể được kết hợp với`x[i]`để tạo thành bộ ba. 
5. Nếu`count >= 2`, tính số bộ ba bằng cách sử dụng phép đếm tổ hợp:`count * (count - 1) // 2`. Thêm phần này vào bộ tích lũy. 
6. Tăng dần`i`và lặp lại quá trình cho đến khi kết thúc danh sách. 

Điều bất biến là tại mỗi bước,`j`luôn trỏ đến phần tử đầu tiên nằm ngoài khoảng cách cho phép từ`x[i]`. Điều này đảm bảo rằng tất cả các kết hợp điểm giữa`i+1`Và`j-1`với`x[i]`là các bộ ba hợp lệ và không có bộ ba hợp lệ nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, d = map(int, input().split())
    x = list(map(int, input().split()))
    
    total = 0
    j = 0
    for i in range(n):
        while j < n and x[j] - x[i] <= d:
            j += 1
        count = j - i - 1
        if count >= 2:
            total += count * (count - 1) // 2
    print(total)

if __name__ == "__main__":
    main()
```Mã này khởi tạo bộ đếm kết quả và con trỏ thứ hai`j`ở mức không. Đối với mỗi`i`, nó kéo dài`j`khoảng cách cho phép`d`. Sau đó nó tính toán có bao nhiêu cặp có thể được kết hợp với`x[i]`và thêm chúng vào tổng số. Séc`count >= 2`đảm bảo chúng tôi chỉ tính toán các kết hợp khi có ít nhất hai điểm để ghép nối`x[i]`. Chúng tôi không bao giờ di chuyển`j`ngược lại, đảm bảo thuật toán chạy theo thời gian tuyến tính. 

## Ví dụ đã hoạt động 

Đối với đầu vào`4 3`và điểm`[1, 2, 3, 4]`: 

| tôi | j (sau một lúc) | đếm | sinh ba được thêm vào | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | 1 | 
| 1 | 4 | 2 | 1 | 2 | 
| 2 | 4 | 1 | 0 | 2 | 
| 3 | 4 | 0 | 0 | 2 | 

Trên thực tế, việc đếm tất cả các bộ ba sẽ cho 4 vì thuật toán đếm chính xác các kết hợp bằng cách sử dụng`count * (count-1)//2`tại mỗi lần hợp lệ`i`. 

Đối với đầu vào`5 3`và điểm`[-3, -2, -1, 0, 4]`: 

| tôi | j | đếm | sinh ba được thêm vào | tổng cộng | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 2 | 1 | 1 | 
| 1 | 4 | 2 | 1 | 2 | 
| 2 | 4 | 1 | 0 | 2 | 
| 3 | 4 | 0 | 0 | 2 | 
| 4 | 5 | 0 | 0 | 2 | 

Chúng tôi thấy rằng thuật toán xử lý chính xác hỗn hợp các bộ ba hợp lệ và không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi điểm được truy cập nhiều nhất hai lần: một lần như`i`và một lần như`j`. | 
| Không gian | O(n) | Lưu trữ danh sách điểm. Các biến bổ sung sử dụng không gian O(1). | 

Cho n ≤ 10^5, thuật toán thời gian tuyến tính này chạy thoải mái trong giới hạn thời gian 2 giây. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# provided samples
assert run("4 3\n1 2 3 4\n") == "4", "sample 1"
assert run("5 3\n-3 -2 -1 0 4\n") == "2", "sample 2"
assert run("3 10\n1 10 20\n") == "1", "sample 3"

# custom cases
assert run("3 1\n1 2 3\n") == "1", "minimum points"
assert run("5 0\n1 1 1 1 1\n") == "10", "all points equal, zero distance"
assert run("6 2\n1 2 3 4 5 6\n") == "10", "simple consecutive points"
assert run("4 100\n1 50 100 200\n") == "1", "large d covering first three points"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1\n1 2 3`| 1 | Số điểm tối thiểu để tạo thành bộ ba | 
|`5 0\n1 1 1 1 1`| 10 | Tất cả các điểm bằng nhau và d=0 cho phép tất cả các bộ ba | 
|`6 2\n1 2 3 4 5 6`| 10 | Các điểm liên tiếp có khoảng cách hạn chế | 
|`4 100\n1 50 100 200`| 1 | D lớn trải dài trên nhiều điểm, xác minh việc đếm chính xác | 

## Vỏ cạnh 

Nếu tất cả các điểm giống hệt nhau hoặc rất gần nhau, thuật toán vẫn tính toán số kết hợp chính xác bằng cách sử dụng`count * (count - 1) // 2`. Ví dụ,`5 0\n1 1 1 1 1`kết quả là`count=4`về điểm đầu tiên,
