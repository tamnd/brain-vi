---
title: "CF 104770A - Chiếu sáng vuông"
description: "Chúng ta có một quảng trường hình chữ nhật có kích thước $n nhân m$. Mỗi đèn đủ mạnh để chiếu sáng một vùng hình vuông thẳng hàng có trục nhỏ hơn có kích thước $k nhân k$. Khi một chiếc đèn được đặt ở bất cứ đâu trong quảng trường, nó sẽ bao phủ toàn bộ khối $k nhân k$ đó."
date: "2026-06-28T19:51:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104770
codeforces_index: "A"
codeforces_contest_name: "The XXXI Saint-Petersburg High School Programming Contest (SpbKOSHP 2023) | Qualification for the XXIV Russia Open High School Programming Contest (VKOSHP 2023)"
rating: 0
weight: 104770
solve_time_s: 63
verified: true
draft: false
---

[CF 104770A - Chiếu sáng vuông](https://codeforces.com/problemset/problem/104770/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một quảng trường hình chữ nhật có kích thước$n \times m$. Mỗi đèn đủ mạnh để chiếu sáng một vùng hình vuông có trục nhỏ hơn$k \times k$. Khi một ngọn đèn được đặt ở bất cứ đâu trong quảng trường, nó sẽ che phủ toàn bộ khu vực đó.$k \times k$khối. 

Mục tiêu là bao phủ toàn bộ$n \times m$hình chữ nhật sử dụng ít như vậy$k \times k$đèn càng tốt. Đèn có thể chồng lên nhau và có thể vượt ra ngoài ranh giới của quảng trường, nhưng phạm vi bao phủ bên ngoài hình chữ nhật không giúp ích được gì. 

Nhiệm vụ giảm xuống còn việc quyết định có bao nhiêu$k \times k$hình vuông là cần thiết để lát gạch hoàn toàn hoặc che phủ một$n \times m$lưới. 

Những ràng buộc cho phép$n, m, k$lên tới$10^9$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp qua các hàng hoặc cột một đơn vị tại một thời điểm. Bất kỳ giải pháp hợp lệ nào cũng phải là số học theo thời gian không đổi. 

Một sự hiểu lầm ngây thơ xuất phát từ việc nghĩ đến việc “khớp” các ô vuông mà không xem xét đến việc bao phủ một phần ở biên giới. Ví dụ, nếu$n = 4, k = 3$, một chiếc đèn đặt ở phía trên bên trái không che hết hàng dưới cùng, mặc dù$4 < 2 \cdot 3$có thể cám dỗ một giả định tham lam. 

Một trường hợp tinh vi khác là khi kích thước là bội số của$k$. Nếu như$n = 6, k = 2$, thì vùng phủ sóng sẽ sạch sẽ. Nhưng nếu$n = 7, k = 2$, dải đèn cuối cùng vẫn cần thêm một đèn phụ đầy đủ dù chỉ còn một hàng không được che chắn. 

Những hiệu ứng ranh giới này chính xác là những gì làm cho hành vi trần trở nên thiết yếu. 

## Phương pháp tiếp cận 

Cách tiếp cận mạnh mẽ sẽ mô phỏng việc đặt đèn theo hàng và cột theo cột. Người ta có thể tưởng tượng việc quét lưới và đặt đèn bất cứ khi nào một ô được phát hiện, đánh dấu toàn bộ ô đó$k \times k$khu vực như được bảo hiểm. 

Điều này hoạt động về mặt khái niệm, nhưng chi phí là rất lớn. Trong trường hợp xấu nhất, mỗi đèn chiếu sáng khoảng$k^2$các ô, do đó số lượng vị trí là khoảng$(n \cdot m) / k^2$. Với$n, m \le 10^9$, kích thước lưới lên tới$10^{18}$, làm cho việc mô phỏng không thể thực hiện được. 

Quan sát quan trọng là phạm vi bao phủ hoàn toàn định kỳ ở cả hai chiều. Mỗi đèn bao phủ một$k$-qua-$k$khối, vì vậy dọc theo một trục chúng ta chỉ quan tâm có bao nhiêu khối đầy đủ kích thước$k$phù hợp và liệu phần dư có tồn tại hay không. Mỗi trục trở thành một bài toán làm tròn độc lập. 

Dọc theo chiều dài$n$, chúng tôi cần$\lceil n / k \rceil$phân đoạn. Dọc theo chiều rộng$m$, chúng tôi cần$\lceil m / k \rceil$phân đoạn. Mỗi cặp đoạn như vậy tương ứng với một vị trí đặt đèn trong một lưới các khối, do đó tổng số là tích của chúng. 

Điều này làm giảm vấn đề che phủ hình học 2D thành hai phần trần 1D độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem có bao nhiêu đầy đủ$k$- Cần có những đoạn có chiều dài để che phủ$n$. Việc này được thực hiện bằng cách sử dụng phép chia trần, tính một đoạn bổ sung nếu$n$không chia hết cho$k$. Điều này đảm bảo không còn dải vải nào bị che khuất ở cạnh dưới. 
2. Tính xem có bao nhiêu đầy đủ$k$- Cần có những đoạn có chiều dài để che phủ$m$sử dụng cùng một logic trần. Điều này đảm bảo không còn dải chưa che nào ở cạnh phải. 
3. Nhân hai kết quả. Mỗi đoạn ngang phải được ghép nối với từng đoạn dọc, tạo thành một lưới các vị trí đèn bao phủ toàn bộ hình chữ nhật. 

### Tại sao nó hoạt động 

Mỗi đèn bao phủ chính xác một$k \times k$khối căn chỉnh theo trục. Bất kỳ chiến lược vị trí hợp lệ nào cũng sẽ phân chia mặt phẳng thành các vùng trong đó mỗi vùng chỉ có thể được bao phủ hoàn toàn bởi một đèn nếu nó nằm bên trong một đèn duy nhất.$k \times k$khối của một lưới bao phủ. 

Sự phân chia trần dọc theo mỗi trục tạo ra số lượng khối tối thiểu cần thiết để trải dài hoàn toàn trục đó. Vì phạm vi bao phủ là hình chữ nhật và độc lập trên các trục nên tích Descartes của các phân vùng này mang lại số lượng đèn tối thiểu. Không có sự sắp xếp nào có thể giảm số lượng vì việc giảm số lượng trục sẽ khiến một đoạn dài hơn$k$chưa được khám phá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())
    
    # ceiling division without floats
    need_n = (n + k - 1) // k
    need_m = (m + k - 1) // k
    
    print(need_n * need_m)

if __name__ == "__main__":
    solve()
```Lời giải đọc ba số nguyên và tính phép chia trần bằng thủ thuật số nguyên tiêu chuẩn$(x + k - 1) // k$. Điều này tránh được các lỗi dấu phẩy động và hoạt động an toàn tới$10^9$. 

Bước nhân an toàn trong phạm vi số nguyên của Python và không cần xử lý tràn bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n = 10, m = 9, k = 3$| Bước | giá trị n | giá trị m | Tính toán | 
| --- | --- | --- | --- | 
| 1 | 10 | 9 | cần_n = trần(10/3) = 4 | 
| 2 | 10 | 9 | cần_m = trần(9/3) = 3 | 
| 3 | - | - | kết quả = 4 × 3 = 12 | 

Điều này cho thấy rằng mặc dù 10 và 9 không phải là bội số của 3, nhưng mỗi trục độc lập yêu cầu một khối riêng bổ sung. Lưới các vị trí khối là 4 x 3. 

### Ví dụ 2 

đầu vào:$n = 4, m = 6, k = 2$| Bước | giá trị n | giá trị m | Tính toán | 
| --- | --- | --- | --- | 
| 1 | 4 | 6 | cần_n = trần(4/2) = 2 | 
| 2 | 4 | 6 | cần_m = trần(6/2) = 3 | 
| 3 | - | - | kết quả = 2 × 3 = 6 | 

Ở đây cả hai chiều đều chia đều nên không cần bao phủ thêm một phần. Các đèn tạo thành một tấm lát 2 x 3 hoàn hảo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số phép tính số học không đổi được thực hiện | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Giải pháp là thời gian không đổi và dễ dàng thỏa mãn các ràng buộc lên đến$10^9$, vì nó chỉ thực hiện hai phép chia số nguyên và một phép nhân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("10 9 3\n") == "12"
assert run("4 6 2\n") == "6"

# minimum case
assert run("1 1 1\n") == "1"

# exact multiples
assert run("6 8 2\n") == "12"

# non-multiples both directions
assert run("7 7 3\n") == "9"

# large values
assert run("1000000000 1000000000 1\n") == "1000000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | trường hợp hợp lệ nhỏ nhất | 
| 6 8 2 | 12 | ốp lát sạch sẽ không còn sót lại | 
| 7 7 3 | 9 | cả hai kích thước đều yêu cầu trần | 
| 10^9 10^9 1 | 10^18 | kiểm tra căng thẳng cho phép nhân lớn | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một chiều nhỏ hơn$k$. Ví dụ,$n = 2, m = 10, k = 3$. 

Đây,$need_n = \lceil 2/3 \rceil = 1$Và$need_m = \lceil 10/3 \rceil = 4$, cho tổng cộng 4 đèn. 

Mặc dù chiều cao nhỏ hơn kích thước đèn nhưng vẫn cần ít nhất một dãy đèn vì vùng phủ sóng không cho phép bố trí theo từng phần. 

Một trường hợp cạnh khác là khi cả hai chiều đều là bội số chính xác, chẳng hạn như$n = 6, m = 6, k = 3$. Sau đó$need_n = 2$,$need_m = 2$, và câu trả lời là 4. Mọi nỗ lực nhằm “hợp nhất” vùng phủ sóng xuyên ranh giới đều thất bại vì mỗi đèn được cố định vào một$k \times k$quảng trường; không có sự tối ưu hóa chồng chéo nào có thể làm giảm số lượng lưới. 

Cuối cùng, khi$k = 1$, mỗi ô cần có đèn riêng. Công thức tạo ra$n \cdot m$, phù hợp với trực giác rằng mỗi ô vuông đơn vị phải được chiếu sáng riêng lẻ.
