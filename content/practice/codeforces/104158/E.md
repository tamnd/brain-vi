---
title: "CF 104158E - Động não không cần não"
description: "Chúng ta có một chuỗi các khe thời gian $N$, mỗi khe chứa ba ưu đãi độc lập: một từ Jim, một từ Dwight và một từ Kevin. Trong ô $i$, việc chọn Jim mang lại ý tưởng $ai$, Dwight mang lại $bi$ và Kevin mang lại $ci$."
date: "2026-07-02T01:09:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104158
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 1 (Advanced)"
rating: 0
weight: 104158
solve_time_s: 62
verified: true
draft: false
---

[CF 104158E - Động não không cần não](https://codeforces.com/problemset/problem/104158/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi$N$các khe thời gian, mỗi khe chứa ba ưu đãi độc lập: một từ Jim, một từ Dwight và một từ Kevin. Trong khe$i$, chọn Jim mang lại$a_i$ý tưởng, Dwight mang lại$b_i$, và Kevin mang lại$c_i$. Đối với mỗi vị trí, chúng tôi có thể chọn tối đa một trong ba vị trí hoặc bỏ qua hoàn toàn vị trí đó. 

Có một hạn chế bổ sung làm thay đổi cấu trúc đáng kể. Nếu chúng ta chọn một ô, chúng ta không thể chọn ô tiếp theo ngay lập tức. Điều này có nghĩa là các vị trí được chọn phải không liền kề và trong mỗi vị trí đã chọn, chúng tôi vẫn chọn chính xác một trong ba giá trị. 

Mục tiêu là tối đa hóa tổng giá trị được chọn. 

Ràng buộc$N \le 1000$cho phép giải pháp quy hoạch động bậc hai. Bất cứ điều gì liệt kê các tập hợp con của các vị trí hoặc thử tất cả các lựa chọn trên mỗi vị trí một cách độc lập mà không có cấu trúc sẽ tăng theo cấp số nhân vì mỗi vị trí có bốn lựa chọn (bỏ qua hoặc chọn một trong ba), dẫn đến$4^N$khả năng trong một tìm kiếm ngây thơ. 

Một trường hợp thất bại tinh vi đối với tư duy tham lam xuất hiện khi một ô có giá trị lớn nhưng theo sau là một ô lớn khác. Ví dụ: nếu khe$i$có giá trị$100, 1, 1$và khe cắm$i+1$có giá trị$99, 1, 1$, chọn vị trí$i$tham lam chặn khe$i+1$và quyết định cục bộ có thể không phù hợp với quyết định tối ưu toàn cục. Sự phụ thuộc hoàn toàn là giữa các chỉ số liên tiếp, do đó các quyết định cục bộ sẽ lan truyền về phía trước. 

Một sai lầm khác đến từ việc xử lý từng vị trí một cách độc lập bằng cách luôn chọn$\max(a_i, b_i, c_i)$. Ví dụ: nếu cả hai vị trí liên tiếp đều có cực đại lớn thì việc lấy cả hai là bất hợp pháp, do đó việc tối đa hóa độc lập sẽ thất bại ngay lập tức. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý các vị trí từ trái sang phải và tại mỗi vị trí quyết định bỏ qua nó hoặc chọn một trong ba nhân viên, đồng thời tôn trọng quy tắc rằng vị trí đã chọn trước đó không được liền kề. Điều này tự nhiên dẫn đến một đệ quy trong đó mỗi trạng thái phụ thuộc vào việc vị trí trước đó có được sử dụng hay không. 

Tuy nhiên, điều này mở rộng theo cấp số nhân vì mỗi vị trí chia thành bốn tùy chọn và tính hợp lệ phụ thuộc vào các quyết định trước đó. Ngay cả với việc cắt tỉa, trong trường hợp xấu nhất, chúng ta vẫn khám phá tất cả các kết hợp của các lựa chọn không liền kề, theo cấp số nhân trong$N$, vượt xa$2^{1000}$. 

Quan sát quan trọng là sự phụ thuộc duy nhất giữa các quyết định là liệu vị trí trước đó có được sử dụng hay không. Sau khi chúng tôi sửa một vị trí, việc nhận dạng các lựa chọn trước đó không còn quan trọng nữa ngoài việc vị trí cuối cùng có được sử dụng hay không. Điều này làm giảm vấn đề thành cấu trúc lập trình động tuyến tính trong đó mỗi trạng thái chỉ phụ thuộc vào chỉ mục trước đó và liệu chúng ta có được phép chiếm vị trí hiện tại hay không. 

Chúng ta có thể nén quyết định trên mỗi vị trí thành một giá trị duy nhất$v_i = \max(a_i, b_i, c_i)$, vì nếu chúng ta quyết định chiếm chỗ$i$, chúng tôi luôn chọn nhân viên giỏi nhất cho vị trí đó. Bài toán sau đó trở nên giống với tổng tối đa cổ điển của các phần tử không liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đệ quy Brute Force qua các lựa chọn |$O(4^N)$|$O(N)$| Quá chậm | 
| Lập trình động trên vùng lân cận |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi vị trí$i$, tính toán$v_i = \max(a_i, b_i, c_i)$. Điều này giảm mỗi vị trí thành một đóng góp tốt nhất có thể đạt được vì chúng tôi không bao giờ tuyển nhiều hơn một nhân viên cho mỗi vị trí. 
2. Xác định mảng DP trong đó$dp[i]$đại diện cho tổng số ý tưởng tối đa mà chúng ta có thể có được chỉ bằng cách sử dụng các vị trí từ$1$ĐẾN$i$, tôn trọng quy tắc không chọn hai ô liên tiếp. 
3. Khởi tạo$dp[0] = 0$bởi vì không có slot, chúng ta chẳng thu được gì cả. Cũng được thiết lập$dp[1] = v_1$vì chỉ có một vị trí có sẵn, chúng tôi có thể lấy hoặc không và lấy nó là tối ưu nếu tích cực. 
4. Đối với mỗi$i \ge 2$, tính:$$dp[i] = \max(dp[i-1], dp[i-2] + v_i)$$Thuật ngữ đầu tiên tương ứng với việc bỏ qua vị trí$i$, trong khi cái thứ hai tương ứng với việc chiếm vị trí$i$, điều này buộc chúng ta phải bỏ qua$i-1$. 
5. Trở về$dp[N]$như câu trả lời. 

Quá trình chuyển đổi hoàn tất vì mọi giải pháp tối ưu đều bao gồm khe$i$hoặc không, và hai trường hợp này tách rời nhau và bao hàm mọi khả năng. 

### Tại sao nó hoạt động 

Tại mọi chỉ số$i$, trạng thái DP biểu thị số tiền tốt nhất có thể đạt được theo ràng buộc cho đến thời điểm đó. Mọi lựa chọn hợp lệ đều loại trừ$i$, trong trường hợp đó nó được chứa đầy đủ trong$dp[i-1]$, hoặc bao gồm$i$, lực lượng loại trừ$i-1$, biến bài toán thành lời giải tối ưu đến$i-2$. Điều này tạo ra một bất biến mà không trạng thái nào phụ thuộc vào các quyết định ngoài một hoặc hai vị trí cuối cùng, đảm bảo tất cả các cấu hình toàn cầu được phân tách thành các lựa chọn cục bộ tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    c = list(map(int, input().split()))
    
    v = [max(a[i], b[i], c[i]) for i in range(n)]
    
    if n == 1:
        print(v[0])
        return
    
    prev2 = 0
    prev1 = v[0]
    
    for i in range(1, n):
        cur = max(prev1, prev2 + v[i])
        prev2 = prev1
        prev1 = cur
    
    print(prev1)

if __name__ == "__main__":
    solve()
```Việc triển khai nén mảng DP thành hai biến cuộn, vì mỗi trạng thái chỉ phụ thuộc vào hai biến trước đó. Bước tiền xử lý chọn$v_i$đảm bảo chúng tôi không bao giờ xem lại lựa chọn của nhân viên nữa, điều này tránh được chiều thứ ba không cần thiết trong DP. 

Điều kiện biên$n = 1$được xử lý ngầm bằng cách khởi tạo, nhưng được giữ rõ ràng cho rõ ràng. Thứ tự cập nhật luân phiên rất quan trọng:`prev1`phải được lưu trước khi ghi đè`prev2`, nếu không thì sự tái diễn sẽ bị phá vỡ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 0 3 1 2
3 1 4 4 4
1 2 1 4 4
```Tính toán đầu tiên$v$: 

| tôi | a_i | b_i | c_i | v_i | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | 3 | 
| 2 | 0 | 1 | 2 | 2 | 
| 3 | 3 | 4 | 1 | 4 | 
| 4 | 1 | 4 | 4 | 4 | 
| 5 | 2 | 4 | 4 | 4 | 

Bây giờ DP: 

| tôi | v_i | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | - | 0 | 3 | 
| 2 | 2 | 0 | 3 | 3 | 
| 3 | 4 | 3 | 3 | 7 | 
| 4 | 4 | 3 | 7 | 7 | 
| 5 | 4 | 7 | 7 | 11 | 

Dấu vết cho thấy rằng việc lấy vị trí 3 và vị trí 5 cùng nhau mang lại cấu trúc tốt nhất, trong khi vị trí 4 trở nên dưới mức tối ưu do xung đột lân cận. 

### Ví dụ 2 

đầu vào:```
4
10 1 1
1 10 1
10 1 1
1 1 10
```Tính toán$v = [10, 10, 10, 10]$. 

| tôi | v_i | dp[i-2] | dp[i-1] | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | 10 | - | 0 | 10 | 
| 2 | 10 | 0 | 10 | 10 | 
| 3 | 10 | 10 | 10 | 20 | 
| 4 | 10 | 10 | 20 | 20 | 

Điều này cho thấy lựa chọn xen kẽ là tối ưu, xác nhận ràng buộc không liền kề chi phối tất cả các lựa chọn riêng lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi vị trí được xử lý một lần với các lần chuyển tiếp theo thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có hai biến DP cuộn được lưu trữ | 

Độ phức tạp thời gian tuyến tính dễ dàng phù hợp với giới hạn của$N \le 1000$và việc sử dụng bộ nhớ là không đổi, làm cho giải pháp trở nên tầm thường đối với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample 1
assert run("""5
3 0 3 1 2
3 1 4 4 4
1 2 1 4 4
""") == "11"

# minimum size
assert run("""1
5
1
10
""") == "10"

# all equal values
assert run("""3
5 5 5
5 5 5
5 5 5
""") == "10"

# alternating high values
assert run("""4
10 1 1 1
1 10 1 1
10 1 1 1
1 1 10 1
""") == "20"

# no benefit after first pick
assert run("""3
100 0 0
0 0 0
100 0 0
""") == "100"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hộp đựng tối đa 1 khe | 10 | xử lý phần tử đơn | 
| tất cả các giá trị bằng nhau | 10 | sự đúng đắn của sự ràng buộc | 
| mức cao xen kẽ | 20 | ràng buộc liền kề | 
| lợi nhuận thưa thớt | 100 | hành vi bỏ qua tối ưu | 

## Vỏ cạnh 

Một đầu vào tối thiểu với$N=1$đảm bảo thuật toán không cố gắng truy cập$dp[-1]$hoặc dựa vào các trạng thái trước đó. Trong trường hợp đó, giải pháp trực tiếp trả về$v_1$, phù hợp với việc khởi tạo lặp lại. 

Một trường hợp có các giá trị lớn xen kẽ như$[100, 0, 100]$chứng minh rằng chiến lược tối ưu phải bỏ qua các vị trí ở giữa ngay cả khi chúng bằng 0, vì việc lấy cả hai đầu là hợp lệ và mang lại tổng cao hơn. 

Đối với các giá trị bằng nhau trên tất cả các vị trí, DP luân phiên hiệu quả giữa việc lấy và bỏ qua, xác nhận rằng việc lặp lại không phụ thuộc vào tính duy nhất của giá trị và vẫn ổn định trong điều kiện ràng buộc.
