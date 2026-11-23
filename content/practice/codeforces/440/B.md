---
title: "CF 440B - Cân bằng"
description: "Chúng ta được cấp một hàng hộp diêm, mỗi hộp chứa một số que diêm. Tổng số kết quả trùng khớp chia hết cho số hộp, do đó tồn tại một cấu hình mục tiêu trong đó mỗi hộp cuối cùng có cùng một số."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 440
codeforces_index: "B"
codeforces_contest_name: "Testing Round 10"
rating: 1600
weight: 440
solve_time_s: 63
verified: true
draft: false
---

[CF 440B - Bộ cân bằng](https://codeforces.com/problemset/problem/440/B) 

**Đánh giá:** 1600 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một hàng hộp diêm, mỗi hộp chứa một số que diêm. Tổng số kết quả trùng khớp chia hết cho số hộp, do đó tồn tại một cấu hình mục tiêu trong đó mỗi hộp cuối cùng có cùng một số. 

Hoạt động duy nhất được phép là di chuyển một que diêm từ hộp này sang hộp lân cận. Một lần di chuyển mang tính chất cục bộ nên việc vận chuyển một que diêm từ hộp 1 đến hộp 5 sẽ tốn nhiều lần di chuyển vì nó phải đi qua các hộp trung gian. 

Nhiệm vụ là tính toán số lần di chuyển liền kề tối thiểu cần thiết để biến phân bố ban đầu thành phân bố cân bằng hoàn hảo. 

Hạn chế chính là số lượng hộp có thể lớn tới 50000, trong khi giá trị có thể đạt tới 10^9. Điều này ngay lập tức loại trừ mọi mô phỏng di chuyển từng trận đấu riêng lẻ. Ngay cả một trận đấu cũng có thể yêu cầu các bước O(n) để di chuyển và việc thực hiện điều đó một cách ngây thơ cho tất cả các trận đấu sẽ dẫn đến hành vi O(nk), vượt xa giới hạn. 

Khó khăn tiềm ẩn là các bước di chuyển không độc lập. Việc di chuyển một trận đấu sẽ làm thay đổi sự mất cân bằng cục bộ và ảnh hưởng đến các quyết định trong tương lai, do đó, việc cân bằng cục bộ tham lam phải được cấu trúc cẩn thận để tránh tính hai lần hoặc bỏ sót các luồng gián tiếp. 

Một trường hợp lỗi phổ biến xuất hiện khi cố gắng “sửa chữa” từng hộp một cách độc lập bằng cách gửi phần thừa sang phải hoặc trái mà không theo dõi luồng tích lũy. Ví dụ: nếu chúng ta cố gắng đẩy thặng dư sang hàng xóm một cách tham lam, chúng ta có thể đếm cùng một chuyển động nhiều lần khi nó lan truyền trong đường dây. 

Một minh họa nhỏ về thất bại: xem xét đầu vào`3`với`[1, 0, 5]`. Một cách tiếp cận đơn giản có thể gửi 2 kết quả phù hợp từ hộp cuối cùng đến giữa, sau đó là 2 kết quả từ giữa đến hộp đầu tiên, nhưng nếu quá trình lan truyền không được theo dõi đúng cách thì chuyển động của cùng một đơn vị có thể được tính không nhất quán tùy thuộc vào cách xử lý hướng. 

Giải pháp đúng cần có cái nhìn tổng thể về luồng mất cân bằng thay vì sửa lỗi cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là quét liên tục mảng, tìm một hộp có phần dư và di chuyển từng bước một về phần thiếu hụt. Mỗi động thái như vậy đều được mô phỏng rõ ràng. Điều này đúng vì nó tuân theo đúng thao tác được phép, nhưng lại quá chậm. Trong trường hợp xấu nhất, một trận đấu có thể di chuyển khoảng cách O(n) và có thể có O(nk) những bước di chuyển như vậy, dẫn đến hành vi bậc hai hoặc tệ hơn tùy thuộc vào sự phân bố. 

Sự cải thiện đến từ việc nhận thấy rằng chúng tôi không cần phải mô phỏng từng trận đấu riêng lẻ. Điều quan trọng là có bao nhiêu trận đấu phải vượt qua từng ranh giới giữa các ô liền kề. 

Nếu chúng ta xem xét tiền tố của các hộp có chỉ mục i, nó có một số khác biệt giữa số lượng kết quả phù hợp mà nó hiện đang giữ và số mà nó sẽ giữ ở trạng thái cân bằng cuối cùng. Sự khác biệt đó phải được đẩy qua ranh giới giữa i và i+1. Điều này có nghĩa là mọi sự mất cân bằng trong tiền tố sẽ chuyển trực tiếp thành chuyển động bắt buộc trên cạnh đó. 

Vì vậy, thay vì theo dõi từng kết quả phù hợp, chúng tôi theo dõi sự mất cân bằng đang diễn ra. Khi chúng tôi di chuyển từ trái sang phải, chúng tôi sẽ duy trì được số trận đấu thừa hoặc thiếu đã tích lũy cho đến nay. Giá trị tuyệt đối của sự mất cân bằng này ở mỗi vị trí chính xác là số lượng khớp phải vượt qua ranh giới đó. 

Điều này làm giảm vấn đề xuống còn một lần quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(nk) trường hợp xấu nhất | O(n) | Quá chậm | 
| Theo dõi mất cân bằng tiền tố | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính số lượng kết quả phù hợp mục tiêu trên mỗi ô bằng cách chia tổng số tiền cho n. Điều này đưa ra giá trị yêu cầu cuối cùng cho mỗi vị trí. 
2. Khởi tạo hai biến: mất cân bằng đang chạy`carry`và một bộ tích lũy câu trả lời`moves`. các`carry`biểu thị số lượng trận đấu dư thừa đã được chuyển từ các hộp trước đó nhưng chưa được hấp thụ. 
3. Lặp lại từng ô từ trái sang phải. 
4. Tại mỗi ô i, cập nhật sự mất cân bằng bằng cách cộng độ lệch của ô hiện tại so với mục tiêu:`carry += a[i] - target`. 
5. Cộng giá trị tuyệt đối của`carry`để trả lời. Điều này thể hiện các kết quả trùng khớp phải vượt qua ranh giới giữa ô i và i+1 do sự mất cân bằng tích lũy. 
6. Tiếp tục cho đến hết. Câu trả lời cuối cùng là tổng của tất cả sự mất cân bằng tuyệt đối. 

Bước suy luận quan trọng là sau khi chúng tôi sửa tiền tố, mọi sự mất cân bằng còn lại về mặt vật lý phải vượt qua ranh giới đến phân đoạn tiếp theo và mỗi lần vượt qua như vậy sẽ tốn chính xác một lần di chuyển cho mỗi đơn vị khớp. 

### Tại sao nó hoạt động 

Tại mọi vị trí i, biến`carry`bằng với giá trị thặng dư ròng của các kết quả phù hợp trong tiền tố`[0..i]`so với những gì tiền tố đó sẽ chứa trong cấu hình cuối cùng. Vì que diêm chỉ có thể di chuyển qua các ranh giới liền kề nên mỗi đơn vị của phần dư này phải vượt qua ranh giới tại i tại một thời điểm nào đó. Tổng các giá trị tuyệt đối của các phần thặng dư này tính mỗi lần giao nhau chính xác một lần, bởi vì mỗi sự mất cân bằng được liên kết với chính xác một chuyển tiếp ranh giới trong biểu diễn luồng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    total = sum(a)
    target = total // n
    
    carry = 0
    moves = 0
    
    for x in a:
        carry += x - target
        moves += abs(carry)
    
    print(moves)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo ý tưởng mất cân bằng tiền tố một cách trực tiếp. Mục tiêu được tính toán một lần. Vòng lặp duy trì thặng dư hoặc thâm hụt đang hoạt động. Việc tích lũy giá trị tuyệt đối là bước quan trọng vì nó tính lượng dòng chảy phải vượt qua mỗi ranh giới. 

Một điểm tinh tế là chúng ta không thiết lập lại`carry`sau khi thêm vào`moves`. Việc mang thể hiện sự mất cân bằng dai dẳng lan truyền qua mảng, do đó việc đặt lại nó sẽ phá hủy cấu trúc luồng toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6
1 6 2 5 3 7
```Mục tiêu mỗi hộp là 4. 

| tôi | một [tôi] | mang trước | theo sau | cơ bụng(mang) | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | -3 | 3 | 3 | 
| 1 | 6 | -3 | -1 | 1 | 4 | 
| 2 | 2 | -1 | -3 | 3 | 7 | 
| 3 | 5 | -3 | -2 | 2 | 9 | 
| 4 | 3 | -2 | -3 | 3 | 12 | 
| 5 | 7 | -3 | 0 | 0 | 12 | 

Câu trả lời cuối cùng là 12. 

Dấu vết này cho thấy sự mất cân bằng lan truyền trên mảng thay vì được giải quyết cục bộ như thế nào. Mức thực hiện tuyệt đối của mỗi bước tương ứng với chuyển động xuyên biên giới cần thiết. 

### Ví dụ 2 

đầu vào:```
4
0 0 8 0
```Mục tiêu là 2. 

| tôi | một [tôi] | mang trước | theo sau | cơ bụng(mang) | di chuyển | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | -2 | 2 | 2 | 
| 1 | 0 | -2 | -4 | 4 | 6 | 
| 2 | 8 | -4 | 2 | 2 | 8 | 
| 3 | 0 | 2 | 0 | 0 | 8 | 

Điều này cho thấy thâm hụt tăng mạnh ở bên trái cho đến khi thặng dư đạt chỉ số 2, sau đó nó được hấp thụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mất cân bằng tiền tố điện toán mảng một lần truyền qua | 
| Không gian | O(1) | chỉ sử dụng các biến phụ không đổi | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì n có thể đạt tới 50000 và chúng tôi chỉ thực hiện quét tuyến tính với các phép tính số học đơn giản. 

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
assert run("6\n1 6 2 5 3 7\n") == "12"

# all equal (no moves needed)
assert run("3\n2 2 2\n") == "0"

# simple imbalance
assert run("2\n0 10\n") == "5"

# alternating imbalance
assert run("4\n0 0 8 0\n") == "8"

# minimum n
assert run("1\n0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 giá trị bằng nhau | 0 | không cần chuyển động | 
| 0 10 | 5 | dòng chảy biên đơn | 
| 0 0 8 0 | 8 | phân phối lại nhiều bước | 
| n=1 | 0 | độ ổn định của vỏ cạnh | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các trận đấu đã được phân bổ đều. Thuật toán xử lý việc này một cách rõ ràng vì`carry`xuyên suốt vẫn bằng 0 và không có đóng góp tuyệt đối nào được thêm vào. 

Một trường hợp khác là khi sự mất cân bằng tích tụ nhiều ở một bên trước khi được giải quyết sau đó. Ví dụ:```
4
0 0 8 0
```Việc mang theo ngày càng trở nên tiêu cực cho đến khi gặp phải tình trạng dư thừa. Mỗi bước đều đóng góp chính xác vào câu trả lời vì mỗi ranh giới phải mang mức thâm hụt đó về phía trước. 

Trường hợp cuối cùng là kích thước đầu vào tối thiểu:```
1
k
```Vì không có ranh giới nên không cần chuyển động. Vòng lặp chạy một lần, nhưng số lần mang ngay lập tức bằng 0 sau khi trừ đi mục tiêu, dẫn đến không có lượt di chuyển nào.
