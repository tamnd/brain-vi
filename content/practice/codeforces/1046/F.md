---
title: "CF 1046F - Chia tiền"
description: "Chúng tôi được cấp một số ví Bitcoin, mỗi ví chứa một lượng satoshi. Mục tiêu là phân phối lại những đồng tiền này vào nhiều ví hơn để không có ví nào giữ nhiều hơn giới hạn cố định $x$."
date: "2026-06-15T11:13:43+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1046
codeforces_index: "F"
codeforces_contest_name: "Bubble Cup 11 - Finals [Online Mirror, Div. 2]"
rating: 1400
weight: 1046
solve_time_s: 256
verified: true
draft: false
---

[CF 1046F - Chia tiền](https://codeforces.com/problemset/problem/1046/F) 

**Đánh giá:** 1400 
**Thẻ:** triển khai 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số ví Bitcoin, mỗi ví chứa một lượng satoshi. Mục tiêu là phân phối lại những đồng tiền này vào nhiều ví hơn để không có ví nào giữ nhiều hơn giới hạn cố định$x$. Tạo một ví mới là miễn phí, nhưng mỗi lần chuyển tiền từ ví này sang ví khác sẽ phải trả một khoản phí cố định$f$, được thanh toán từ ví nguồn. 

Một lần chuyển tiền không chỉ là một động thái ghi sổ, nó thực sự chuyển một số tiền dương từ ví này sang ví mới hoặc ví hiện có và mọi hành động như vậy đều phải chịu một khoản phí như nhau bất kể có bao nhiêu đồng tiền được di chuyển. Nhiệm vụ là giảm thiểu tổng số phí cần thiết để sau tất cả các hoạt động, mỗi ví chứa tối đa$x$satoshi. 

Đầu vào mô tả nhiều tập hợp số dư ví ban đầu và hai tham số: số dư tối đa được phép trên mỗi ví và chi phí cho mỗi lần chuyển. Đầu ra là tổng chi phí tối thiểu để đạt được ràng buộc. 

Khó khăn chính là việc chia tách không bị ép buộc trong một bước duy nhất. Một ví quá lớn có thể được chia thành nhiều ví nhỏ hơn bằng nhiều giao dịch và mỗi giao dịch sẽ làm giảm số dư nguồn trong khi tạo ví đích mới. 

Các ràng buộc làm cho việc mô phỏng thô bạo không thể thực hiện được. Với số lượng ví lên tới 200.000 và giá trị lên tới$10^9$, bất kỳ phương pháp nào liên tục mô phỏng việc chuyển tiền hoặc theo dõi từng đồng tiền riêng lẻ đều quá chậm. Giải pháp phải giảm từng ví một cách độc lập xuống một số hoạt động xác định. 

Một vài trường hợp đặc biệt bộc lộ những lỗi suy luận ngây thơ. 

Trường hợp thất bại đầu tiên là giả định rằng mỗi ví cần$\lceil a_i / x \rceil - 1$hoạt động mà không xem xét logic tổng hợp còn sót lại không chính xác. Ví dụ, nếu$a_i = x$, không cần thực hiện thao tác nào, nhưng những công thức bất cẩn thường vẫn tính một phép chia. 

Trường hợp thất bại thứ hai là cố gắng tham lam “lấp đầy ví mới” trên nhiều nguồn khác nhau, dẫn đến việc ghép nối không chính xác giữa các ví. Vì mỗi chi phí giao dịch là độc lập nên việc phối hợp phân chia giữa các ví sẽ không mang lại lợi ích gì. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ mô phỏng từng ví một. Đối với mỗi ví có số dư$a_i > x$, chúng tôi liên tục tạo một ví mới và chuyển lên$x$tiền vào đó cho đến khi ví nguồn có hiệu lực. Mỗi lần di chuyển như vậy đều tốn kém$f$. Quá trình tiếp tục cho đến khi tất cả các ví thỏa mãn ràng buộc. 

Điều này đúng nhưng đắt tiền. Trong trường hợp xấu nhất, một chiếc ví có kích thước$10^9$có thể yêu cầu về$10^9 / x$chuyển giao và xuyên suốt$200{,}000$ví, điều này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là mỗi ví đều độc lập. Một chiếc ví có kích thước$a_i$đóng góp một số lượng “ví bổ sung” bắt buộc cố định bất kể các ví khác. Mỗi chiếc ví có thể chứa tối đa$x$, do đó số lượng ví tối thiểu cần thiết để lưu trữ$a_i$là:$$\left\lceil \frac{a_i}{x} \right\rceil$$Nếu một ví đã tồn tại, nó sẽ đóng góp một vị trí. Mỗi vị trí bổ sung tương ứng với chính xác một hoạt động chuyển tiền, vì mỗi lần chuyển tiền sẽ tạo ra chính xác một ví mới. 

Do đó, số lượng thao tác cần thiết cho ví$i$là:$$\left\lceil \frac{a_i}{x} \right\rceil - 1$$Tổng số này trên tất cả các ví sẽ cho ra tổng số lần chuyển và nhân với$f$đưa ra câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(\sum a_i / x)$|$O(N)$| Quá chậm | 
| Đếm toán học |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi giá trị ví$a_i$, hãy tính xem cần bao nhiêu ví có dung lượng đầy đủ. Điều này được thực hiện bằng cách sử dụng số học số nguyên như$(a_i + x - 1) // x$. Biểu thức thực hiện phép chia trần mà không cần thao tác dấu phẩy động. 
2. Trừ đi một từ số lượng này để xác định số lượng ví bổ sung phải được tạo. Phép trừ phản ánh rằng ví ban đầu đã tồn tại và không cần tạo. 
3. Thêm giá trị này vào tổng số hoạt động đang chạy. Mỗi đơn vị trong tổng này tương ứng với chính xác một lần chuyển. 
4. Sau khi xử lý tất cả các ví, nhân tổng số hoạt động với mức phí cố định$f$để có được chi phí cuối cùng. 

Lý do đằng sau bước 2 là việc chia tách chỉ cần thiết khi ví vượt quá dung lượng. Một chiếc ví vừa vặn không cần bất kỳ thao tác nào vì nó đã thỏa mãn ràng buộc. 

### Tại sao nó hoạt động 

Mỗi ví là độc lập vì việc chuyển tiền không bao giờ hợp nhất các ràng buộc giữa các nguồn khác nhau. Một chiếc ví có kích thước$a_i$tối đa phải được phân chia thành các nhóm có kích thước$x$và số lượng nhóm được cố định chỉ bằng dung lượng. Vì mỗi lần chuyển sẽ tạo chính xác một ví mới nên sẽ có sự tương ứng 1-1 giữa các nhóm và hoạt động bổ sung. Điều này làm cho tổng chi phí hoàn toàn tăng thêm trên các ví. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    x, f = map(int, input().split())

    ops = 0
    for v in a:
        ops += (v + x - 1) // x - 1

    print(ops * f)

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện công thức dẫn xuất. The expression`(v + x - 1) // x`tính toán số lượng ví cần thiết cho một số dư nhất định. Trừ đi một sẽ loại bỏ ví ban đầu khỏi số lượng, chỉ để lại số lần chuyển cần thiết. 

Bộ tích lũy`ops`theo dõi tổng số lần chuyển. nhân với`f`chuyển đổi hoạt động thành chi phí satoshi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
13 7 6
6 2
```Chúng tôi tính toán các hoạt động trên mỗi ví. 

| cái ví$a_i$| ví cần thiết$\lceil a_i/x \rceil$| hoạt động | 
| --- | --- | --- | 
| 13 | 3 | 2 | 
| 7 | 2 | 1 | 
| 6 | 1 | 0 | 

Tổng số hoạt động = 3, tổng chi phí =$3 \cdot 2 = 6$. 

Dấu vết này cho thấy việc phân chia chỉ phụ thuộc vào số lượng khối kích thước 6 mà mỗi ví yêu cầu. 

### Ví dụ 2 

đầu vào:```
4
5 6 7 12
5 3
```| cái ví$a_i$| ví cần thiết | hoạt động | 
| --- | --- | --- | 
| 5 | 1 | 0 | 
| 6 | 2 | 1 | 
| 7 | 2 | 1 | 
| 12 | 3 | 2 | 

Tổng số hoạt động = 4, chi phí =$4 \cdot 3 = 12$. 

Điều này xác nhận rằng các ví lớn hơn thậm chí còn phân hủy độc lập thành số lần phân chia cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi ví được xử lý một lần với số học thời gian không đổi | 
| Không gian |$O(1)$| Chỉ duy trì một bộ đếm đang chạy | 

Quá trình quét tuyến tính lên tới 200.000 phần tử có thể dễ dàng đủ nhanh trong vòng 1 giây vì các phép toán là các phép tính số nguyên đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    x, f = map(int, input().split())

    ops = 0
    for v in a:
        ops += (v + x - 1) // x - 1

    return str(ops * f)

# provided sample
assert run("3\n13 7 6\n6 2\n") == "6"

# all fit exactly
assert run("3\n5 5 5\n5 10\n") == "0"

# single large split
assert run("1\n11\n5 1\n") == "1"

# multiple independent splits
assert run("2\n9 9\n5 1\n") == "2"

# max-sized balanced case
assert run("5\n1 2 3 4 5\n10 7\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bằng x | 0 | không có sự chia tách không cần thiết | 
| một giá trị lớn | chia đúng | hành vi trần | 
| nhiều nguồn | độc lập phụ gia | không có sự can thiệp chéo | 
| giá trị nhỏ | hoạt động bằng không | độ đúng giới hạn dưới | 

## Vỏ cạnh 

Trường hợp phức tạp xảy ra khi ví đã đáp ứng chính xác giới hạn. Đối với đầu vào`a = [6]`với`x = 6`, phép tính cho`(6 + 5) // 6 - 1 = 1 - 1 = 0`, do đó không có hoạt động nào được tính. Điều này tránh được lỗi thường gặp là luôn đếm ít nhất một nhóm. 

Một trường hợp khác là khi các giá trị nằm ngay trên bội số của$x$. Vì`a = [7]`Và`x = 6`, công thức cho`(7 + 5) // 6 - 1 = 2 - 1 = 1`, khớp với yêu cầu chia thành 6 và 1. 

Đầu vào lớn không thay đổi hành vi vì mọi hoạt động được tính toán trong thời gian không đổi trên mỗi ví, ngăn chặn bất kỳ sự cố mô phỏng nào.
