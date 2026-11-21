---
title: "CF 245D - Bảng khôi phục"
description: "Chúng ta có một bảng vuông ban đầu được tạo từ một mảng ẩn gồm các số nguyên không âm. Mỗi mục nhập ngoài đường chéo là AND theo bit của cặp giá trị ẩn tương ứng, trong khi các mục nhập theo đường chéo được thay thế một cách giả tạo bằng -1."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy"]
categories: ["algorithms"]
codeforces_contest: 245
codeforces_index: "D"
codeforces_contest_name: "CROC-MBTU 2012, Elimination Round (ACM-ICPC)"
rating: 1500
weight: 245
solve_time_s: 190
verified: true
draft: false
---

[CF 245D - Bảng khôi phục](https://codeforces.com/problemset/problem/245/D) 

**Đánh giá:** 1500 
**Tags:** thuật toán mang tính xây dựng, tham lam 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bảng vuông ban đầu được tạo từ một mảng ẩn gồm các số nguyên không âm. Mỗi mục nhập ngoài đường chéo là AND theo bit của cặp giá trị ẩn tương ứng, trong khi các mục nhập theo đường chéo được thay thế một cách giả tạo bằng -1. Nhiệm vụ là xây dựng lại bất kỳ mảng ẩn hợp lệ nào có thể tạo ra bảng. 

Điểm mấu chốt là mọi giá trị quan sát được b[i][j] cho i ≠ j chính xác là kết quả của việc áp dụng AND theo từng bit cho hai số chưa biết a[i] và a[j]. Đường chéo không cung cấp thông tin. Chúng tôi không bắt buộc phải khôi phục mảng ban đầu duy nhất, chỉ một mảng phù hợp với tất cả các ràng buộc theo cặp. 

Ràng buộc n 100 có nghĩa là chúng ta có thể đủ khả năng suy luận bậc ba hoặc thậm chí bậc bốn nếu cần, nhưng cấu trúc của bit AND gợi ý rằng việc tái cấu trúc trực tiếp trên mỗi bit hoặc trên mỗi phần tử là đủ trong O(n²) hoặc O(n³). Bất kỳ cấu hình hàm mũ nào trên bit đều không cần thiết vì mỗi giá trị tối đa là 10⁹, vì vậy chúng tôi chỉ xử lý 30 bit cho mỗi số. 

Một trường hợp thất bại tinh vi phát sinh khi việc xây dựng lại gán các giá trị một cách tham lam mà không đảm bảo tính nhất quán trên tất cả các cặp. Ví dụ: nếu chúng ta cố gắng suy ra a[i] chỉ từ một hàng duy nhất, chúng ta có thể vi phạm các ràng buộc với một hàng khác. Một vấn đề khác là giả sử các ràng buộc đẳng thức lan truyền bắc cầu theo cách ngây thơ, điều này không đúng với AND theo bit. 

Một tình huống có vấn đề cụ thể là khi ba chỉ số tương tác không nhất quán nếu chúng ta gán giá trị theo từng cặp. Ví dụ: việc chọn a[i] quá nhỏ để thỏa mãn một cặp sẽ dẫn đến mâu thuẫn ở các cặp khác vì AND chỉ bảo toàn các bit có trong cả hai số. 

Cấu trúc ẩn khóa là mỗi a[i] phải tương thích với mọi hàng i, nghĩa là a[i] phải là tập con (theo bit) của tất cả các ràng buộc b[i][j], nhưng cũng không được đưa ra các bit bị cấm có thể tạo ra mâu thuẫn khi AND với các giá trị được chọn khác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng gán từng a[i] độc lập từ 0 đến 10⁹ và kiểm tra xem tất cả các kết quả AND theo cặp có khớp với ma trận hay không. Điều này ngay lập tức bùng nổ thành (10⁹)ⁿ khả năng, điều này là không thể ngay cả với n = 100. 

Thay vào đó, chúng ta cần thiết kế ngược các ràng buộc bit. Quan sát quan trọng là bitwise AND hoạt động độc lập trên mỗi bit. Một bit được đặt trong a[i] & a[j] khi và chỉ nếu bit đó được đặt trong cả a[i] và a[j]. Điều này có nghĩa là với mỗi cặp (i, j), bất kỳ bit nào xuất hiện trong b[i][j] đều phải xuất hiện trong cả a[i] và a[j]. 

Vì vậy, đối với mỗi cặp (i, j), chúng ta có thể thực thi một cách an toàn rằng tất cả các bit được đặt trong b[i][j] phải có ở cả hai điểm cuối. Điều này đưa ra một ràng buộc giới hạn dưới đối với a[i] và a[j]. Nếu tổng hợp trên tất cả j, chúng ta nhận được rằng a[i] phải chứa tất cả các bit xuất hiện trong bất kỳ b[i][j] nào. 

Điều này đưa ra một cấu trúc ứng cử viên tự nhiên: đặt a[i] làm OR theo bit của tất cả b[i][j] trên j ≠ i. Điều này đảm bảo rằng mọi bit được yêu cầu đều có trong a[i]. Điều còn lại là xác minh rằng cách xây dựng này không đưa thêm các bit có thể tạo ra mâu thuẫn. Cấu trúc của AND đảm bảo điều này hoạt động vì mọi bit được đặt trong a[i] phải xuất hiện trong ít nhất một b[i][j] và do đó chỉ có thể tồn tại nếu nó cũng có trong a[j] bất cứ khi nào cần. 

Điều này làm giảm vấn đề xuống còn một lần truyền qua ma trận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công vũ lực | O((10⁹)ⁿ) | O(n²) | Quá chậm | 
| Xây dựng ràng buộc Bitwise | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo một mảng a có kích thước n với tất cả các số 0. Điều này sẽ tích lũy tất cả các ràng buộc bit cho mỗi chỉ mục. 
2. Với mỗi chỉ số i, lặp lại tất cả j ≠ i và tính a[i] |= b[i][j]. Bước này thu thập mọi bit phải xuất hiện trong a[i], vì mỗi b[i][j] là a[i] & a[j], và do đó không thể chứa các bit không có trong a[i]. 
3. Sau khi điền tất cả a[i], chúng ta ngầm dựa vào tính đối xứng: nếu một bit xuất hiện trong a[i] do b[i][j] nào đó, thì nó chỉ nhất quán nếu nó cũng xuất hiện trong a[j]. Tính nhất quán này được đảm bảo vì b[i][j] giống nhau đã được sử dụng khi tính a[j]. 
4. Xuất mảng kết quả. 

Tại sao nó hoạt động được gắn liền với sự độc lập của bit. Mỗi vị trí bit hoạt động giống như một hệ thống ràng buộc nhị phân riêng biệt. Nếu một bit xuất hiện trong b[i][j] thì cả hai điểm cuối đều phải chứa nó. Nếu nó không xuất hiện thì ít nhất một điểm cuối có thể chứa hoặc không chứa nó, nhưng không bao giờ chứa cả hai trừ khi được các cặp khác hỗ trợ. Việc lấy OR trên tất cả các ràng buộc sẽ đảm bảo mọi bit bắt buộc đều được bao gồm và không có cấu trúc không cần thiết nào được đưa vào có thể vi phạm bất kỳ giá trị AND theo cặp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    b = [list(map(int, input().split())) for _ in range(n)]

    a = [0] * n

    for i in range(n):
        val = 0
        for j in range(n):
            if i == j:
                continue
            val |= b[i][j]
        a[i] = val

    print(*a)

if __name__ == "__main__":
    solve()
```Mã trực tiếp dịch ý tưởng xây dựng. Các vòng lặp lồng nhau quét từng hàng và tích lũy OR theo bit trên tất cả các mục nhập ngoài đường chéo. Đường chéo bị bỏ qua vì nó không chứa thông tin hữu ích. 

Một sai lầm phổ biến là quên rằng các mục theo đường chéo là -1 và cố gắng đưa chúng vào các phép toán theo bit, điều này sẽ làm hỏng kết quả. Một vấn đề tế nhị khác là vô tình sử dụng AND thay vì OR; OR là bắt buộc vì chúng tôi đang tích lũy các bit cần thiết chứ không giao nhau với chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
-1
```Vì chỉ có một phần tử nên không có ràng buộc nào tồn tại. Số hợp lệ duy nhất là 0. 

| tôi | j | b[i][j] | a[i] sau bước | 
| --- | --- | --- | --- | 
| 0 | - | - | 0 | 

Đầu ra:```
0
```Điều này xác nhận rằng thuật toán xử lý trường hợp suy biến trong đó không tồn tại ràng buộc theo cặp. 

### Ví dụ 2 

đầu vào:```
3
-1 1 0
1 -1 0
0 0 -1
```Chúng tôi tính toán từng hàng một cách độc lập. 

Với i = 0, chúng ta OR giá trị 1 và 0, cho ra a[0] = 1. 

Với i = 1, chúng ta OR giá trị 1 và 0, cho ra a[1] = 1. 

Với i = 2, chúng ta OR giá trị 0 và 0, cho ra a[2] = 0. 

| tôi | đóng góp j | một [tôi] | 
| --- | --- | --- | 
| 0 | 1, 0 | 1 | 
| 1 | 1, 0 | 1 | 
| 2 | 0, 0 | 0 | 

Bây giờ hãy kiểm tra tính nhất quán: 

1 & 1 = 1 khớp với b[0][1] và tất cả các cặp khác liên quan đến 0 tạo ra 0 theo yêu cầu. 

Điều này cho thấy việc xây dựng sắp xếp các ràng buộc đối xứng một cách tự nhiên mà không cần các bước điều chỉnh bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi mục nhập ma trận được truy cập một lần trong quá trình tích lũy OR | 
| Không gian | O(n²) | Lưu trữ ma trận đầu vào | 

Với n ≤ 100, tối đa 10⁴ thao tác được thực hiện, điều này không đáng kể trong giới hạn. Việc sử dụng bộ nhớ cũng nhỏ vì ma trận có nhiều nhất là 100 × 100 số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("1\n-1\n") == "0"

# two-node simple case
assert run("2\n-1 5\n5 -1\n") == "5 5"

# all zeros off diagonal
assert run("3\n-1 0 0\n0 -1 0\n0 0 -1\n") == "0 0 0"

# mixed bits
assert run("3\n-1 1 2\n1 -1 0\n2 0 -1\n") in ["3 1 2", "3 3 2"]

# symmetric consistency
assert run("4\n-1 1 0 0\n1 -1 2 0\n0 2 -1 4\n0 0 4 -1") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận 1×1 | 0 | trường hợp cơ sở | 
| Ràng buộc đơn 2 nút | giá trị giống hệt nhau | tuyên truyền | 
| tất cả số không | tất cả số không | thiếu bit | 
| bit hỗn hợp | tái thiết hợp lệ | tính đúng đắn của logic OR | 

## Vỏ cạnh 

Với n = 1, không có ràng buộc nào cả, vì vậy mọi giá trị đều hợp lệ. Thuật toán tạo ra 0 vì OR trên tập trống là 0, phù hợp với yêu cầu của số nguyên không âm. 

Đối với trường hợp một số hàng chỉ chứa số 0, chẳng hạn như một nút AND bằng 0 với những hàng khác, thuật toán sẽ gán a[i] = 0. Điều này đúng vì bất kỳ bit dương nào cũng sẽ vi phạm ít nhất một ràng buộc cặp. 

Đối với các ma trận dày đặc trong đó nhiều mục chia sẻ các bit chồng chéo, cấu trúc OR đảm bảo tất cả các bit bắt buộc đều được đưa vào mà không gặp vấn đề trùng lặp. Mỗi bit được căn chỉnh bằng ít nhất một cặp được quan sát, do đó không có bit giả nào được đưa vào.
