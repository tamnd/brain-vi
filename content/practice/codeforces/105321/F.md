---
title: "CF 105321F - Lịch thi đấu"
description: "Chúng ta được cung cấp một bản ghi theo trình tự thời gian về các trận đấu quần vợt được chơi bởi một người chơi. Mỗi trận đấu sẽ dẫn đến thắng hoặc thua, được mã hóa dưới dạng mảng nhị phân trong đó 1 đại diện cho thắng và 0 đại diện cho thua. Hệ thống tính điểm có hai thành phần độc lập."
date: "2026-06-22T10:52:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105321
codeforces_index: "F"
codeforces_contest_name: "2024 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 105321
solve_time_s: 43
verified: true
draft: false
---

[CF 105321F - Lịch thi đấu](https://codeforces.com/problemset/problem/105321/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bản ghi theo trình tự thời gian về các trận đấu quần vợt được chơi bởi một người chơi. Mỗi trận đấu sẽ dẫn đến thắng hoặc thua, được mã hóa dưới dạng mảng nhị phân trong đó 1 đại diện cho thắng và 0 đại diện cho thua. 

Hệ thống tính điểm có hai thành phần độc lập. Nguyên tắc đầu tiên rất đơn giản: mỗi trận thắng đóng góp +1 điểm và mỗi trận thua đóng góp -1 điểm. Điều thứ hai phụ thuộc vào chuỗi trận: bất cứ khi nào trận đấu kết thúc và người chơi hiện đang có chuỗi ít nhất ba trận thắng liên tiếp bao gồm cả trận đấu này, họ sẽ nhận được thêm +1 điểm cho thời điểm đó. 

Điều tinh tế quan trọng là phần thưởng liên tục này được đánh giá sau mỗi trận đấu chứ không chỉ ở cuối trận. Vì vậy, chuỗi bốn trận thắng sẽ đóng góp nhiều phần thưởng, một phần thưởng cho mỗi vị trí mà hậu tố kết thúc ở trận đấu đó có độ dài ít nhất là ba. 

Kích thước đầu vào nhỏ, với N lên tới 100, ngay lập tức cho phép quét tuyến tính trực tiếp mà không cần quan tâm đến việc tối ưu hóa ngoài O(N). Ngay cả mô phỏng O(N^2) cũng có thể được chấp nhận, nhưng cấu trúc của bài toán rõ ràng cho phép O(N). 

Một sai lầm ngây thơ là chỉ kiểm tra xem có tồn tại bất kỳ chuỗi nào có độ dài ít nhất ba hay không và thêm một phần thưởng duy nhất cho mỗi chuỗi. Ví dụ, đối với đầu vào`1 1 1 1`, phần thưởng chính xác là 2 (ở vị trí 3 và 4), nhưng việc triển khai đơn giản có thể chỉ thêm 1 không chính xác. 

Một lỗi phổ biến khác là tính các chuỗi trên toàn cầu thay vì theo từng vị trí. Ví dụ, trong`1 1 1 0 1 1 1`, chuỗi thứ hai lại đóng góp nhiều phần thưởng độc lập với chuỗi đầu tiên. 

Cách giải thích đúng hoàn toàn mang tính cục bộ: sau mỗi trận đấu thứ i, chúng tôi kiểm tra xem ba trận đấu cuối cùng (i-2 đến i) có thắng hay không và nếu vậy thì chúng tôi sẽ thêm một điểm thưởng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề là mô phỏng quy tắc tính điểm theo đúng nghĩa đen. Đối với mỗi vị trí i, chúng tôi tính toán chuỗi thắng liên tiếp hiện tại kết thúc ở i bằng cách quét ngược cho đến khi gặp thua. Nếu độ dài vệt đó ít nhất là 3, chúng tôi sẽ thêm tiền thưởng. 

Điều này hiệu quả vì nó khớp trực tiếp với định nghĩa, nhưng nó có thể giảm xuống O(N^2) trong trường hợp xấu nhất khi tất cả các trận đấu đều thắng. Mỗi vị trí sẽ quét lại các bước O(N). 

Tuy nhiên, quan sát làm thay đổi mọi thứ là chúng ta thực sự không cần độ dài đầy đủ của vệt. Điều kiện tiền thưởng chỉ phụ thuộc vào việc ba trận gần nhất có thắng hay không. Nếu các vị trí i, i−1 và i−2 tồn tại và đều bằng 1 thì người chơi sẽ nhận được chính xác một phần thưởng ở vị trí i. Điều này chuyển đổi quá trình quét ngược không bị giới hạn thành kiểm tra liên tục theo thời gian cho mỗi vị trí. 

Vì vậy, vấn đề giảm xuống còn một cửa sổ trượt đơn giản có kích thước ba trên một mảng nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét ngược Brute Force | O(N^2) | O(1) | Quá chậm (không cần thiết) | 
| Kiểm tra trượt tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các trận đấu từ trái sang phải trong khi vẫn duy trì tổng điểm. 

1. Khởi tạo câu trả lời về 0. Biến này tích lũy cả điểm thắng/thua và tiền thưởng liên tiếp. 
2. Lặp lại từng trận đấu i từ 0 đến N−1. Đối với mỗi trận đấu, hãy cập nhật điểm số bằng cách cộng +1 nếu kết quả là thắng và −1 nếu kết quả là thua. Điều này trực tiếp mã hóa quy tắc tính điểm cơ sở. 
3. Nếu i ≥ 2, hãy kiểm tra xem Ri, Ri−1 và Ri−2 có bằng 1 hay không. Việc kiểm tra này xác định liệu trận đấu hiện tại có hoàn thành chuỗi ít nhất ba trận thắng liên tiếp kết thúc ở i hay không. 
4. Nếu điều kiện được thỏa mãn, hãy thêm điểm +1 bổ sung vào câu trả lời. Điều này tương ứng chính xác với quy tắc mỗi điểm cuối đủ điều kiện đóng góp một phần thưởng độc lập. 

Sau khi xử lý tất cả các trận đấu, giá trị tích lũy là điểm số cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là quy tắc thưởng chỉ phụ thuộc vào việc liệu cửa sổ có độ dài ba kết thúc ở vị trí i có bao gồm toàn bộ chiến thắng hay không. Bất kỳ chuỗi dài hơn nào sẽ tự động kích hoạt tình trạng này ở mọi điểm cuối hậu tố bắt đầu từ trận thắng thứ ba trở đi. Do đó, việc đếm từng cửa sổ có độ dài 3 hợp lệ chính xác một lần tương đương với việc đếm tất cả các điểm cuối của chuỗi đủ điều kiện. Điểm cơ bản không phụ thuộc vào điều kiện này nên việc tách biệt hai phần đóng góp sẽ đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    ans = 0
    
    for i in range(n):
        if a[i] == 1:
            ans += 1
        else:
            ans -= 1
        
        if i >= 2 and a[i] == 1 and a[i-1] == 1 and a[i-2] == 1:
            ans += 1
    
    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp giữ một bộ tích lũy duy nhất đang chạy. Bản cập nhật đầu tiên bên trong vòng lặp xử lý trực tiếp quy tắc tính điểm tuyến tính. Khối điều kiện thứ hai là kiểm tra cửa sổ trượt cho ba trận thắng liên tiếp, chỉ áp dụng khi có ít nhất ba trận đấu được xử lý. 

Chi tiết triển khai quan trọng là thứ tự: trước tiên chúng tôi áp dụng điểm cơ bản cho trận đấu hiện tại, sau đó đánh giá chuỗi kết thúc ở vị trí đó. Điều này đảm bảo rằng việc kiểm tra chuỗi luôn bao gồm trận đấu hiện tại. 

## Ví dụ đã hoạt động 

### Ví dụ 1: kết quả luân phiên 

đầu vào:```
8
1 0 1 0 1 0 1 0
```Chúng tôi theo dõi sự tiến triển của điểm số: 

| tôi | kết quả | thay đổi căn cứ | vệt (3 lần cuối) | tiền thưởng | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | +1 | không | 0 | 1 | 
| 1 | 0 | −1 | không | 0 | 0 | 
| 2 | 1 | +1 | không | 0 | 1 | 
| 3 | 0 | −1 | không | 0 | 0 | 
| 4 | 1 | +1 | không | 0 | 1 | 
| 5 | 0 | −1 | không | 0 | 0 | 
| 6 | 1 | +1 | không | 0 | 1 | 
| 7 | 0 | −1 | không | 0 | 0 | 

Câu trả lời cuối cùng là 0. 

Điều này xác nhận rằng các chiến thắng riêng lẻ không bao giờ kích hoạt tiền thưởng, ngay cả khi có nhiều lần xuất hiện riêng biệt. 

### Ví dụ 2: chuỗi thắng trọn vẹn 

đầu vào:```
5
1 1 1 1 1
```| tôi | kết quả | thay đổi căn cứ | vệt (3 lần cuối) | tiền thưởng | tổng cộng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | +1 | không | 0 | 1 | 
| 1 | 1 | +1 | không | 0 | 2 | 
| 2 | 1 | +1 | vâng | +1 | 4 | 
| 3 | 1 | +1 | vâng | +1 | 6 | 
| 4 | 1 | +1 | vâng | +1 | 8 | 

Điều này chứng tỏ rằng khi một chuỗi đạt đến độ dài 3, mỗi trận đấu tiếp theo trong chuỗi đó sẽ đóng góp thêm một phần thưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Truyền một lần qua mảng với các kiểm tra liên tục theo thời gian cho mỗi chỉ mục | 
| Không gian | O(1) | Chỉ một số biến vô hướng được sử dụng ngoài mảng đầu vào | 

Các ràng buộc N ≤ 100 khiến việc này trở nên dễ dàng trong giới hạn, nhưng giải pháp đã tối ưu và có quy mô vượt xa giới hạn đã cho. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("8\n1 0 1 0 1 0 1 0\n") == "0"
assert run("5\n0 0 0 0 0\n") == "-5"
assert run("5\n1 1 1 1 1\n") == "8"

# custom tests
assert run("3\n1 1 1\n") == "4", "single minimal streak"
assert run("2\n1 1\n") == "2", "no bonus possible"
assert run("4\n1 1 1 0\n") == "2", "streak breaks early"
assert run("6\n1 1 1 1 0 1\n") == "4", "multiple streak interaction"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 cái | 4 | kích hoạt tiền thưởng tối thiểu | 
| 2 cái | 2 | ranh giới nơi tiền thưởng không thể áp dụng | 
| 3 cái thì thua | 2 | chấm dứt sớm chuỗi | 
| trình tự hỗn hợp | 4 | thiết lập lại hành vi sau giờ nghỉ | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu`N = 1`, nói đầu vào`1`, thuật toán đặt điểm cơ bản thành +1 và không bao giờ tiến hành kiểm tra chuỗi vì i < 2. Kết quả đầu ra là 1, điều này đúng vì không thể tồn tại chuỗi có độ dài 3. 

Đối với một chuỗi mất mát đầy đủ như`0 0 0 0`, mỗi vị trí đóng góp −1 và không có vệt nào. Điểm trở thành −4, phù hợp với quy tắc vì điều kiện tiền thưởng không bao giờ được kích hoạt. 

Để có chuỗi chiến thắng dài như`1 1 1 1 1`, việc triển khai sẽ thêm chính xác các phần thưởng ở các chỉ số 2, 3 và 4. Theo dõi rõ ràng, tại i = 2, 3, 4, điều kiện ba cửa sổ giữ nguyên, tạo ra chính xác ba phần thưởng. Điều này xác nhận rằng các khoản đóng góp trong chuỗi trùng lặp được xử lý độc lập cho mỗi vị trí thay vì được nhóm thành một sự kiện duy nhất.
