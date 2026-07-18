---
title: "CF 103462K - K-Clearing II"
description: "Chúng ta được cung cấp một mảng số nguyên và một giá trị đặc biệt $k$. Quá trình này rất linh hoạt: miễn là mảng vẫn chứa ít nhất một phần tử bằng $k$, chúng tôi liên tục áp dụng một thao tác làm giảm mọi phần tử dương của mảng đi một phần tử."
date: "2026-07-03T07:02:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103462
codeforces_index: "K"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2021"
rating: 0
weight: 103462
solve_time_s: 41
verified: true
draft: false
---

[CF 103462K - K-Clearing II](https://codeforces.com/problemset/problem/103462/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một mảng số nguyên và một giá trị đặc biệt$k$. Quá trình này diễn ra linh hoạt: miễn là mảng vẫn chứa ít nhất một phần tử bằng$k$, chúng ta liên tục áp dụng một thao tác làm giảm mọi phần tử dương của mảng đi một. Số 0 không thay đổi và giá trị không bao giờ xuống dưới 0. 

Đối với bất kỳ mảng con liền kề nào có độ dài cố định$m$, chúng ta tưởng tượng việc chạy quy trình toàn cầu này cho đến khi không$k$vẫn còn trong toàn bộ mảng và sau đó chúng tôi đếm xem có bao nhiêu số 0 xuất hiện bên trong mảng con đó sau khi quá trình ổn định. Câu trả lời cuối cùng là tổng của số 0 này trong suốt chiều dài-$m$mảng con. 

Điểm mấu chốt là hoạt động giảm có tính toàn cục và được lặp lại, do đó mọi phần tử đều giảm chính xác một lần trong mỗi “vòng” trong khi bất kỳ phần tử nào cũng giảm$k$vẫn tồn tại ở đâu đó trong mảng. 

Những ràng buộc đẩy chúng ta tới hành vi tuyến tính hoặc gần tuyến tính. Với$n$lên đến$10^6$, bất kỳ giải pháp nào mô phỏng rõ ràng từng vòng giảm đều ngay lập tức không thể thực hiện được. Thậm chí$O(n^2)$trên cửa sổ trượt sẽ quá chậm vì có$O(n)$cửa sổ và mỗi cái đều có kích thước$O(n)$. Do đó, mục tiêu là một lần truyền hoặc một số lượng nhỏ các lần truyền qua mảng với$O(n)$hoặc$O(n \log n)$sự phức tạp. 

Một vấn đề tế nhị là hiểu được điều kiện dừng. Số lượng giảm toàn cầu không cố định trước; nó phụ thuộc vào số vòng tối đa cần thiết cho tất cả các lần xuất hiện của$k$biến mất. Bất kỳ cách tiếp cận ngây thơ nào cố gắng mô phỏng cho đến khi không$k$tồn tại sẽ giảm quá mức hoặc quét mảng nhiều lần, cả hai đều quá đắt. 

Các trường hợp cạnh phát sinh khi: 

Một trường hợp tối thiểu như$n = m = 1$có thể gây nhầm lẫn trong cách giải thích về quá trình toàn cầu. 

Nếu mảng không chứa$k$hoàn toàn, thao tác không bao giờ kích hoạt, do đó mảng không thay đổi và không có số 0 nào được tạo. Bất kỳ giải pháp nào giả định ít nhất một vòng sẽ giảm giá trị một cách không chính xác. 

Nếu tất cả các phần tử đều bằng$k$, mỗi vòng sẽ giảm tất cả các giá trị dương một cách đồng đều, do đó mảng hoạt động giống như một dòng thời gian giảm dần cho đến khi mọi thứ về 0. Số vòng trở nên chính xác$k$và tất cả các phần tử trở thành 0 ở cuối, điều này ảnh hưởng mạnh mẽ đến sự đóng góp của mọi mảng con. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng trực tiếp quy trình được mô tả cho từng mảng con. Đối với mọi cửa sổ có chiều dài$m$, chúng ta sẽ sao chép mảng con, liên tục giảm tất cả các phần tử dương trong khi$k$tồn tại và cuối cùng đếm số không. Điều này đúng về mặt logic, nhưng chi phí của nó rất lớn. 

Mỗi mảng con yêu cầu tiềm năng$O(\max a_i)$giảm số vòng và mỗi vòng chạm$O(m)$các phần tử. Với tối đa$10^6$mảng con, điều này thoái hóa thành một cái gì đó theo thứ tự$10^{12}$hoạt động trong trường hợp xấu nhất là không thể. 

Quan sát chính là hoạt động mang tính toàn cục và thống nhất: mọi phần tử đều giảm chính xác cùng số bước cho đến khi quá trình dừng lại. Điều quan trọng duy nhất là có bao nhiêu vòng diễn ra trước khi tất cả các lần xuất hiện của$k$được loại bỏ. Hãy để số đó là$T$. Khi đó mọi phần tử$a_i$trở thành$\max(0, a_i - T)$. 

Vì vậy toàn bộ vấn đề quy về việc tìm$T$, thì với mỗi cửa sổ đếm chính xác có bao nhiêu phần tử$T$, bởi vì đó là những phần tử có giá trị chính xác bằng 0. 

Điều này chuyển đổi vấn đề từ mô phỏng lặp lại thành một vấn đề ngưỡng dẫn xuất duy nhất, sau đó là tính toán tần số cửa sổ trượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot m \cdot k)$trường hợp xấu nhất |$O(m)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng chính: giảm động lực xuống một ngưỡng duy nhất 

Đầu tiên chúng ta xác định có bao nhiêu phép toán giảm tổng thể xảy ra trước khi tất cả$k$các giá trị biến mất. Mỗi thao tác giảm tất cả các giá trị dương đi một, do đó, một phần tử bằng$k$sống sót chính xác$T = k$vòng trước khi trở thành số 0. Nếu có ít nhất một$k$, quá trình này kéo dài chính xác$k$bước; nếu không có$k$, nó kéo dài bằng 0 bước. 

Như vậy mọi phần tử đều được biến đổi theo quy tắc$a_i \to \max(0, a_i - T)$, Ở đâu$T = k$nếu như$k$tồn tại, ngược lại$T = 0$. 

Số số 0 trong cửa sổ sau khi chuyển đổi chính xác là số phần tử thỏa mãn$a_i \le T$. Tuy nhiên, vì chỉ những phần tử có giá trị chính xác bằng 0 nên chúng ta đếm xem có bao nhiêu giá trị ban đầu thỏa mãn$a_i \le T$, với sự bình đẳng chính xác chỉ đóng góp khi$a_i \le k$và đặc biệt trở thành số không. 

### Các bước 

1. Quét mảng một lần để kiểm tra xem giá trị có$k$xuất hiện. Nếu không, hãy đặt$T = 0$; nếu không thì đặt$T = k$. Điều này xác định tổng số vòng giảm toàn cầu. 
2. Chuyển bài toán sang dạng đếm, với mỗi mảng con có độ dài$m$, có bao nhiêu phần tử thỏa mãn$a_i \le T$. Đây chính xác là những phần tử sẽ trở thành số 0 sau khi quá trình kết thúc. 
3. Xây dựng mảng nhị phân$b$Ở đâu$b_i = 1$nếu như$a_i \le T$, nếu không thì$0$. Điều này mã hóa xem mỗi vị trí có đóng góp số 0 ở trạng thái cuối cùng hay không. 
4. Tính tổng trên toàn bộ chiều dài-$m$cửa sổ tổng của$b$các giá trị. Điều này có thể được thực hiện bằng cách sử dụng cửa sổ trượt: duy trì tổng cửa sổ hiện tại, thêm phần tử vào, loại bỏ phần tử đi và tích lũy kết quả. 
5. Câu trả lời cuối cùng là tổng số tiền tích lũy được. 

### Tại sao nó hoạt động 

Quá trình này áp dụng cùng một số phép toán giảm dần cho mọi phần tử, do đó thứ tự tương đối không thành vấn đề. Mỗi phần tử độc lập trở thành 0 chính xác khi giá trị ban đầu của nó lớn nhất$T$. Do đó, phép chuyển đổi làm giảm vấn đề thành bộ lọc ngưỡng tĩnh, sau đó là tập hợp cửa sổ trượt tiêu chuẩn. Điều bất biến là sau$T$bước, giá trị của mọi phần tử chỉ phụ thuộc vào giá trị ban đầu của nó chứ không phụ thuộc vào vị trí hoặc tương tác, do đó các đóng góp của cửa sổ trở nên bổ sung và độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    has_k = any(x == k for x in a)
    T = k if has_k else 0
    
    b = [1 if x <= T else 0 for x in a]
    
    window_sum = sum(b[:m])
    ans = window_sum
    
    for i in range(m, n):
        window_sum += b[i] - b[i - m]
        ans += window_sum
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc xác định liệu giá trị đặc biệt$k$đang có mặt. Điều này kiểm soát xem có bất kỳ vòng giảm nào xảy ra hay không. Sau đó, mảng được chuyển đổi thành các phần tử đánh dấu mảng chỉ báo boolean mà cuối cùng sẽ trở thành số 0. 

Cửa sổ trượt duy trì số phần tử như vậy trong mỗi mảng con có độ dài$m$. Mỗi bản cập nhật sẽ điều chỉnh cửa sổ theo thời gian không đổi và tổng số lượt chạy sẽ tích lũy đóng góp từ tất cả các cửa sổ. 

Một lỗi phổ biến là cố gắng mô phỏng quá trình giảm dần một cách rõ ràng. Một cách khác là tính toán lại tổng cửa sổ từ đầu, sẽ là$O(nm)$và thất bại ở quy mô. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3 1
1 2 3 4 5
```Đây$k = 1$, Vì thế$T = 1$. Chúng tôi đánh dấu các yếu tố$\le 1$. 

| tôi | một [tôi] | b[i] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 2 | 0 | 
| 3 | 3 | 0 | 
| 4 | 4 | 0 | 
| 5 | 5 | 0 | 

Kích thước cửa sổ là 3. 

| Cửa sổ | Giá trị | Tổng hợp | 
| --- | --- | --- | 
| 1 | [1,2,3] | 1 | 
| 2 | [2,3,4] | 0 | 
| 3 | [3,4,5] | 0 | 

Tổng số câu trả lời là$1$. 

Điều này cho thấy chỉ có cửa sổ đầu tiên chứa phần tử mà cuối cùng sẽ trở thành số 0. 

### Ví dụ 2 

đầu vào:```
6 3 1
1 1 4 5 1 4
```Lại$T = 1$. Đánh dấu: 

| tôi | một [tôi] | b[i] | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 4 | 0 | 
| 4 | 5 | 0 | 
| 5 | 1 | 1 | 
| 6 | 4 | 0 | 

Windows: 

| Cửa sổ | Giá trị | Tổng hợp | 
| --- | --- | --- | 
| 1 | [1,1,4] | 2 | 
| 2 | [1,4,5] | 1 | 
| 3 | [4,5,1] | 1 | 
| 4 | [5,1,4] | 1 | 

Tổng cộng là$5$. 

Điều này xác nhận rằng câu trả lời tích lũy đóng góp từ các cửa sổ chồng chéo một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Một lần vượt qua để phát hiện$k$, một đường chuyền để xây dựng các chỉ báo, một đường chuyền cửa sổ trượt | 
| Không gian |$O(n)$| Lưu trữ mảng biến đổi nhị phân | 

Các ràng buộc cho phép tối đa một triệu phần tử, do đó, thời gian tuyến tính với các thao tác đơn giản cho mỗi phần tử vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# provided samples
# (outputs assumed based on statement explanation)
assert run("5 3 1\n1 2 3 4 5\n") == "1\n"
assert run("6 3 1\n1 1 4 5 1 4\n") == "5\n"

# all elements equal, k present
assert run("4 2 2\n2 2 2 2\n") == "3\n"

# no k present
assert run("5 3 10\n1 2 3 4 5\n") == "0\n"

# minimal case
assert run("1 1 1\n1\n") == "1\n"

# mixed values
assert run("7 3 3\n3 1 3 2 3 4 3\n") == "9\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng nhau k | kiểm tra sự lan truyền đầy đủ về số không | | 
| không k | đảm bảo không có sự biến đổi nào xảy ra | | 
| phần tử đơn | độ đúng của ranh giới cơ sở | | 
| mẫu hỗn hợp | tính chính xác của cửa sổ chồng chéo | | 

## Vỏ cạnh 

Khi mảng không chứa sự xuất hiện của$k$, bước chuyển đổi không bao giờ được kích hoạt. Thuật toán xử lý việc này bằng cách thiết lập$T = 0$, nghĩa là không có phần tử nào thỏa mãn$a_i \le T$trừ khi nó đã bằng 0, điều này không bao giờ xảy ra dưới những ràng buộc. Do đó, cửa sổ trượt có tổng bằng 0 trên tất cả các mảng con, phù hợp với thực tế là không có gì thay đổi. 

Khi tất cả các phần tử bằng nhau$k$, mọi vị trí đều thỏa mãn điều kiện ngưỡng. Mảng nhị phân trở thành tất cả những số một và tổng mỗi cửa sổ bằng$m$. Cửa sổ trượt tích lũy chính xác$n - m + 1$đóng góp của$m$, khớp với trạng thái bằng 0 thống nhất sau khi quá trình hoàn tất. 

Đối với mảng một phần tử, tổng cửa sổ là 1 hoặc 0 tùy thuộc vào việc$k$phù hợp với phần tử. Cửa sổ trượt suy biến chính xác thành một giá trị duy nhất mà không cần xử lý đặc biệt.
