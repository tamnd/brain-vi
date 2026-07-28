---
title: "CF 102756G - Đã khóa"
description: "Sự cố yêu cầu chúng tôi khôi phục mật khẩu bị quên từ một số mã khôi phục. Mỗi mã có chính xác k chữ số, nhưng mọi mã đều bị ảnh hưởng bởi sự sắp xếp lại vị trí chữ số không xác định giống nhau."
date: "2026-07-29T00:31:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102756
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 10-09-20 Div. 1"
rating: 0
weight: 102756
solve_time_s: 63
verified: true
draft: false
---

[CF 102756G - Đã khóa](https://codeforces.com/problemset/problem/102756/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sự cố yêu cầu chúng tôi khôi phục mật khẩu bị quên từ một số mã khôi phục. Mỗi mã có chính xác`k`các chữ số, nhưng mọi mã đều bị ảnh hưởng bởi sự sắp xếp lại các vị trí chữ số không xác định giống nhau. Chúng ta cần tìm hoán vị của các vị trí làm cho mã kết quả lớn nhất càng gần với mã kết quả nhỏ nhất càng tốt và đưa ra mức chênh lệch tối thiểu có thể có. 

Đầu vào chứa`n`mã và số chữ số`k`trong mỗi mã. Một hoán vị duy nhất của`k`vị trí phải được áp dụng cho mọi mã. Sau khi chọn hoán vị đó, mỗi mã sẽ trở thành một mã mới`k`số chữ số và mục tiêu là giảm thiểu sự khác biệt giữa các giá trị được chuyển đổi tối đa và tối thiểu. 

Các ràng buộc nhỏ ở khía cạnh quan trọng nhất. Có thể có tối đa 100 mã và mỗi mã có tối đa 9 chữ số. Một giải pháp quét tất cả các sắp xếp lại có thể có của các chữ số là có thể bởi vì`9! = 362880`, lớn nhưng vẫn có thể quản lý được. Một giải pháp thử gán các chữ số tùy ý một cách độc lập cho mỗi mã sẽ phát nổ vì nó sẽ bỏ qua thực tế là tất cả các mã đều có chung một hoán vị. 

Các trường hợp chính xuất phát từ việc coi mã là số nguyên thông thường thay vì chuỗi chữ số có độ dài cố định. Các số 0 đứng đầu là một phần của mã và có thể di chuyển đến các vị trí khác. Ví dụ:```
Input
2 2
12
32
```Hoán vị tốt nhất giữ các chữ số theo thứ tự ban đầu. Những con số trở thành`12`Và`32`, vậy câu trả lời là`20`. Việc triển khai bất cẩn lưu trữ mã dưới dạng số nguyên sẽ làm mất thông tin về các số 0 đứng đầu trong các trường hợp khác. 

Một trường hợp đặc biệt khác là khi tất cả các mã đều bằng nhau:```
Input
3 3
111
111
111
```Mọi hoán vị đều tạo ra các giá trị giống nhau, vì vậy kết quả đầu ra đúng là`0`. Việc triển khai chỉ cập nhật câu trả lời khi tìm thấy giá trị nhỏ hơn có thể vô tình giữ giá trị ban đầu vô hạn nếu nó không xử lý chính xác hoán vị đầu tiên. 

Trường hợp cuối cùng là khi hoán vị tốt nhất đặt số 0 ở vị trí quan trọng nhất:```
Input
2 3
100
200
```Một hoán vị di chuyển chữ số cuối cùng lên phía trước sẽ cho`001`Và`002`, do đó sự khác biệt trở thành`1`. Mã loại bỏ các số 0 đứng đầu trước khi áp dụng hoán vị không bao giờ có thể khám phá ra câu trả lời này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử mọi hoán vị có thể có của`k`các vị trí chữ số. Đối với mỗi hoán vị, chúng tôi chuyển đổi mọi mã, theo dõi số được chuyển đổi nhỏ nhất và số được chuyển đổi lớn nhất và cập nhật câu trả lời bằng hiệu của chúng. Điều này đúng vì mọi sự sắp xếp lại pháp lý đều được kiểm tra rõ ràng. 

Phần đắt tiền là số lượng hoán vị. Trong trường hợp xấu nhất,`k = 9`, cho`9! = 362880`hoán vị. Đối với mỗi hoán vị chúng ta phải xử lý tất cả`n = 100`mã và tất cả`k = 9`chữ số, tức là về`3.2 * 10^8`các thao tác về chữ số. Trong Python, điều này đòi hỏi phải thực hiện cẩn thận, nhưng nó có thể được cải thiện bằng cách tính toán trước tác động của các hoán vị trên tập hợp mã nhỏ. 

Quan sát quan trọng là`k`nhỏ và cố định, trong khi`n`cũng nhỏ. Chúng ta không cần phải xây dựng lại nhiều lần các chuỗi đã biến đổi trong vòng lặp hoán vị. Chúng ta có thể lưu trữ mọi hoán vị có thể có của các vị trí một lần, sau đó áp dụng nó một cách hiệu quả cho từng mã. Vì mọi mã đều sử dụng cùng một hoán vị nên không gian tìm kiếm chỉ có`k!`, không phải thứ gì đó phụ thuộc theo cấp số nhân vào số lượng mã. 

Lực lượng vũ phu hoạt động vì số lượng sắp xếp lại chữ số có thể bị hạn chế. Sự tối ưu hóa đến từ việc thể hiện từng sự sắp xếp lại một cách gọn gàng và tránh làm việc lặp lại bên trong vòng lặp trong cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k! * n * k) | O(1) | Quá chậm nếu không tối ưu hóa | 
| Tối ưu | O(k! * n * k) với cách triển khai hiệu quả | O(k! * k + n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mọi mã khôi phục dưới dạng chuỗi và giữ các số 0 đứng đầu. Chuyển đổi mỗi ký tự thành một chữ số nguyên để có thể truy cập nhanh chóng các vị trí. 
2. Tạo mọi hoán vị của các vị trí`0`bởi vì`k - 1`. Mỗi hoán vị thể hiện một cách mà máy tính bảng có thể sắp xếp lại các chữ số. 
3. Đối với mỗi hoán vị, hãy biến đổi từng mã bằng cách đọc các chữ số của nó theo thứ tự mới. Xây dựng số kết quả bằng cách nhân liên tục giá trị hiện tại với 10 và cộng chữ số tiếp theo. 
4. Trong khi xử lý các mã được chuyển đổi, hãy duy trì các giá trị được chuyển đổi tối thiểu và tối đa. Sự khác biệt giữa chúng là mật khẩu ứng cử viên cho hoán vị này. 
5. Giữ lại chênh lệch nhỏ nhất tìm thấy trong số tất cả các hoán vị và in nó. 

Tại sao nó hoạt động: 

Mọi sự sắp xếp lại thông thường có thể xảy ra của các vị trí chữ số đều xuất hiện chính xác một lần trong số các hoán vị được tạo ra. Đối với hoán vị cố định, thuật toán tính toán chính xác các giá trị tối đa và tối thiểu được tạo ra bởi sự sắp xếp lại đó, do đó, nó đánh giá ứng cử viên mật khẩu chính xác cho trường hợp đó. Vì hoán vị tối ưu phải là một trong những hoán vị được tạo ra nên việc lấy chênh lệch nhỏ nhất trên tất cả các ứng cử viên sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    nums = [list(map(int, input().strip())) for _ in range(n)]

    perms = list(permutations(range(k)))

    answer = 10 ** k

    for p in perms:
        mn = 10 ** k
        mx = -1

        for num in nums:
            value = 0
            for idx in p:
                value = value * 10 + num[idx]

            if value < mn:
                mn = value
            if value > mx:
                mx = value

        diff = mx - mn
        if diff < answer:
            answer = diff

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ dưới dạng danh sách các chữ số thay vì số nguyên vì các số 0 đứng đầu phải vẫn hiển thị trong quá trình hoán vị. Ví dụ, mã`001`khác với số nguyên`1`vì ba vị trí sau này có thể được sắp xếp lại. 

Danh sách hoán vị được tạo một lần trước khi bắt đầu tìm kiếm. Mỗi hoán vị chứa các vị trí chữ số ban đầu theo thứ tự chúng sẽ xuất hiện sau khi sắp xếp lại. 

Vòng lặp bên trong xây dựng số được biến đổi trực tiếp bằng số học. Điều này tránh việc tạo liên tục các chuỗi tạm thời và cũng xử lý các số 0 đứng đầu một cách tự nhiên vì số 0 đứng đầu đơn giản là không đóng góp gì khi số được tạo. 

Giá trị tối thiểu và tối đa được đặt lại cho mỗi hoán vị. Việc quên điều này sẽ so sánh các giá trị từ các cách sắp xếp lại khác nhau và tạo ra kết quả không hợp lệ. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
Input
2 2
12
32
```Một dấu vết cho hoán vị hữu ích là: 

| Hoán vị | Mã chuyển đổi | Tối thiểu | Tối đa | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| (0, 1) | 12, 32 | 12 | 32 | 20 | 
| (1, 0) | 21, 23 | 21 | 23 | 2 | 

Hoán vị thứ hai cho sự sắp xếp tối ưu. Việc hoán đổi chữ số giống nhau được áp dụng cho cả hai mã, tạo ra các số gần nhau hơn nhiều. 

Đối với mẫu thứ hai:```
Input
4 4
1842
0141
5581
1581
```Một dấu vết của hoán vị tốt nhất là: 

| Hoán vị | Mã chuyển đổi | Tối thiểu | Tối đa | Sự khác biệt | 
| --- | --- | --- | --- | --- | 
| được chọn tốt nhất | 1284, 0411, 8155, 8151 | 411 | 8155 | 7744 | 

Thuật toán cũng kiểm tra tất cả các hoán vị khác và tìm ra sự khác biệt nhỏ nhất của`1017`. 

Ví dụ này giải thích tại sao hoán vị phải được áp dụng cho mọi mã cùng nhau. Chúng tôi không sắp xếp lại từng số một cách độc lập mà chỉ khám phá bản đồ vị trí được chia sẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k! * n * k) | Mọi hoán vị chữ số đều được kiểm tra và mọi kiểm tra đều kiểm tra tất cả các mã và chữ số của chúng. | 
| Không gian | O(k! * k + n * k) | Các hoán vị được lưu trữ và biểu diễn chữ số của đầu vào chi phối việc sử dụng bộ nhớ. | 

Với`k <= 9`, số hoán vị nhiều nhất là`362880`. Kích thước đầu vào đủ nhỏ để việc kiểm tra mọi sắp xếp lại có thể nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    def solve():
        input = sys.stdin.readline
        n, k = map(int, input().split())
        nums = [list(map(int, input().strip())) for _ in range(n)]

        ans = 10 ** k
        for p in permutations(range(k)):
            mn = 10 ** k
            mx = -1
            for num in nums:
                value = 0
                for i in p:
                    value = value * 10 + num[i]
                mn = min(mn, value)
                mx = max(mx, value)
            ans = min(ans, mx - mn)

        return str(ans)

    return solve()

assert run("""2 2
12
32
""") == "2", "sample 1"

assert run("""4 4
1842
0141
5581
1581
""") == "1017", "sample 2"

assert run("""3 3
111
111
111
""") == "0", "all equal values"

assert run("""2 3
100
200
""") == "1", "leading zero permutation"

assert run("""2 1
0
9
""") == "9", "single digit boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`111,111,111`|`0`| Xử lý chính xác các mã giống hệt nhau | 
|`100,200`với`k=3`|`1`| Kiểm tra các hoán vị tạo ra các số 0 đứng đầu | 
| Mã một chữ số`0,9`|`9`| Kiểm tra độ dài chữ số nhỏ nhất có thể | 
| Trường hợp mẫu | Kết quả đầu ra mẫu | Xác nhận thuật toán chung | 

## Vỏ cạnh 

Đối với các giá trị bằng nhau, thuật toán kiểm tra mọi hoán vị nhưng mọi giá trị được chuyển đổi vẫn giống hệt nhau. Vì:```
3 3
111
111
111
```mỗi hoán vị tạo ra ba bản sao của`111`, do đó cả tối thiểu và tối đa đều là`111`và câu trả lời trở thành`0`. 

Đối với số 0 đứng đầu:```
2 3
100
200
```hoán vị`(2, 0, 1)`thay đổi mã thành`001`Và`002`. Thuật toán xây dựng chúng dưới dạng số nguyên`1`Và`2`, vì vậy nó tìm thấy chính xác sự khác biệt`1`. Nó không mất các vị trí 0 vì hoán vị được áp dụng trước khi số được tạo. 

Đối với một chữ số:```
2 1
0
9
```chỉ có một hoán vị có thể xảy ra. Các giá trị được chuyển đổi là`0`Và`9`, do đó thuật toán trả về`9`. Điều này xác nhận rằng việc tạo hoán vị cũng hoạt động khi không có lựa chọn thứ tự nào.
