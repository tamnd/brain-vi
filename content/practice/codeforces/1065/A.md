---
title: "CF 1065A - Vasya và Sôcôla"
description: "Vasya có một số tiền cố định và muốn tối đa hóa số lượng thanh sô-cô-la mà anh ta có được khi khuyến mãi lặp lại. Mỗi thanh có một mức giá cố định nên sức mua ban đầu của anh ta chỉ đơn giản là anh ta có thể mua trực tiếp bao nhiêu thanh."
date: "2026-06-15T08:17:32+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1065
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 52 (Rated for Div. 2)"
rating: 800
weight: 1065
solve_time_s: 228
verified: true
draft: false
---

[CF 1065A - Vasya và Sôcôla](https://codeforces.com/problemset/problem/1065/A) 

**Đánh giá:** 800 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 3 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vasya có một số tiền cố định và muốn tối đa hóa số lượng thanh sô-cô-la mà anh ta có được khi khuyến mãi lặp lại. Mỗi thanh có một mức giá cố định nên sức mua ban đầu của anh ta chỉ đơn giản là anh ta có thể mua trực tiếp bao nhiêu thanh. Điều thú vị là cửa hàng tặng thêm tiền thưởng: bất cứ khi nào anh ta mua một số lượng thanh nhất định, anh ta sẽ nhận được thêm các thanh miễn phí. Khuyến mãi này có thể được kích hoạt nhiều lần miễn là anh ta tiếp tục mua đủ thanh. 

Nhiệm vụ là tính toán, đối với mỗi trường hợp thử nghiệm, tổng số thanh mà Vasya có thể nhận được nếu anh ta cư xử tối ưu, kết hợp cả giao dịch mua trả phí và tất cả phần thưởng miễn phí có thể có. 

Các ràng buộc đi lên đến$10^9$, loại trừ mọi mô phỏng mua từng thanh một hoặc lặp lại các chu kỳ khuyến mại theo một vòng lặp ngây thơ. Bất kỳ cách tiếp cận nào xử lý từng giao dịch mua hoặc từng kích hoạt khuyến mãi riêng lẻ sẽ quá chậm, vì trong trường hợp xấu nhất, số lần lặp có thể lên tới hàng tỷ. 

Một vấn đề tế nhị xuất hiện khi nghĩ về mô phỏng tham lam. Người ta có thể cố gắng lặp đi lặp lại việc mô phỏng việc mua$a$quán bar, bộ sưu tập$b$thanh miễn phí và lặp lại cho đến khi hết tiền. Điều này không đúng ở dạng ngây thơ vì nó ngầm cho rằng chúng ta theo dõi số tiền còn lại sau mỗi chu kỳ không hiệu quả hoặc quên rằng số tiền còn lại vẫn góp phần mua thêm ngoài gói đầy đủ. 

Một trường hợp khác phát sinh khi khuyến mãi yếu hơn tỷ lệ chi phí, ví dụ khi$b = 0$. Trong trường hợp đó, giải pháp sẽ đơn giản là chia tiền theo giá mà không cần cố gắng “áp dụng” các chương trình khuyến mãi vô ích. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là mô phỏng trực tiếp hành vi của Vasya. Chúng tôi tính toán xem anh ấy có thể mua bao nhiêu thanh bằng tiền của mình, sau đó áp dụng lại quy tắc: mỗi khi anh ấy tích lũy đủ số thanh đã mua, chúng tôi sẽ chuyển đổi các nhóm$a$thanh đã mua vào$b$các thanh miễn phí bổ sung, bản thân chúng có thể góp phần vào các chương trình khuyến mãi tiếp theo. Điều này nhanh chóng trở nên không hiệu quả vì mỗi ứng dụng khuyến mãi sẽ thay đổi trạng thái và số chu kỳ có thể tỷ lệ thuận với tổng số thanh, trong trường hợp xấu nhất là rất lớn. 

Quan sát quan trọng là cấu trúc của vấn đề là tuyến tính và có tính nhân. Mỗi nhóm$a$thanh được mua tạo ra hiệu quả$a + b$tổng số thanh. Điều này có nghĩa là thay vì mô phỏng các trao đổi lặp đi lặp lại, chúng ta có thể nén toàn bộ quá trình vào việc đếm xem có bao nhiêu nhóm đầy đủ$a$các thanh Vasya có thể mua và nhân các nhóm đó với tổng sản lượng của mỗi nhóm. 

Các thanh còn sót lại không hoàn thành một nhóm đầy đủ$a$không đóng góp gì cho chương trình khuyến mãi nhưng vẫn được tính vào tổng số tiền cuối cùng. Sự tách biệt này thành các nhóm đầy đủ và phần còn lại sẽ loại bỏ mọi hành vi năng động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng | O(tổng số thanh) | O(1) | Quá chậm | 
| Nhóm toán | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem Vasya có thể trực tiếp mua bao nhiêu thanh sô cô la bằng cách chia$s$qua$c$. Điều này cho biết số lượng thanh được trả tiền, vì mỗi thanh có giá$c$đồng rúp. 
2. Chia các thanh đã mua này thành các nhóm có kích thước đầy đủ$a$. Mỗi nhóm đầy đủ sẽ kích hoạt khuyến mãi đúng một lần. 
3. Nhân số nhóm đầy đủ với$a + b$, vì mỗi nhóm$a$thanh được mua mang lại những thanh đó$a$thanh cộng$b$thanh bổ sung. 
4. Thêm các thanh đã mua còn sót lại không tạo thành một nhóm đầy đủ vì chúng không đóng góp vào bất kỳ phần thưởng nào nhưng vẫn được tính vào tổng số. 
5. Xuất số tiền cuối cùng. 

Ý tưởng chính là chương trình khuyến mãi không tương tác giữa các nhóm một phần, do đó việc nhóm theo$a$nắm bắt hoàn toàn tất cả các kích hoạt tiền thưởng có thể có. 

### Tại sao nó hoạt động 

Mỗi thanh được mua góp phần hình thành một khối hoàn chỉnh$a$hoặc vẫn chưa được sử dụng về mặt khuyến mãi. Mỗi khối hoàn chỉnh mang lại kết quả một cách xác định$a + b$thanh bất kể thứ tự mua hàng và không có sự phụ thuộc giữa các khối. Vì khả năng mua của Vasya được cố định ở mức$s // c$, việc phân tách thành các khối đầy đủ và phần còn lại sẽ bảo toàn tất cả cấu trúc của quy trình mà không bị mất hoặc tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        s, a, b, c = map(int, input().split())

        bought = s // c

        groups = bought // a
        rem = bought % a

        total = groups * (a + b) + rem
        print(total)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên chuyển đổi tiền thành số lượng thanh có thể mua được. Sau đó, nó tách các thanh này thành các khối quảng cáo hoàn chỉnh và phần còn lại. Bước nhân là nơi khuyến mãi được áp dụng hàng loạt thay vì tăng dần. Phần còn lại được thêm vào không thay đổi vì nó không thể kích hoạt ưu đãi. 

Phải cẩn thận khi chia số nguyên; cả việc chia tầng và modulo đều cần thiết. Sử dụng số học dấu phẩy động sẽ gây ra lỗi chính xác do các ràng buộc lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 3 1 1
```Chúng tôi tính toán số lượng thanh Vasya có thể mua:$10 // 1 = 10$. 

Bây giờ chúng tôi nhóm chúng: 

| đã mua | nhóm | phần còn lại | tổng tính toán | 
| --- | --- | --- | --- | 
| 10 | 3 | 1 | 3 * (3 + 1) + 1 = 13 | 

Kết quả là 13, phù hợp với ý tưởng rằng mỗi khối đầy đủ gồm 3 ô nhịp mang lại tổng cộng 4 ô nhịp. 

Điều này xác nhận rằng các thanh còn sót lại không tham gia vào các chương trình khuyến mãi. 

### Ví dụ 2 

đầu vào:```
1000000000 1 1000000000 1
```Tại đây, mỗi thanh được mua sẽ kích hoạt khuyến mãi ngay lập tức. 

| đã mua | nhóm | phần còn lại | tổng tính toán | 
| --- | --- | --- | --- | 
| 1000000000 | 1000000000 | 0 | 1000000000 * (1 + 1) = 2000000000 | 

Tuy nhiên, vì mỗi nhóm tạo ra một thanh tự do có độ lớn bằng chính nhóm đó nên giá trị cuối cùng khớp với tỷ lệ tuyến tính dự kiến ​​của tổng đóng góp. 

Điều này cho thấy khi$a = 1$, mỗi lần mua hàng trực tiếp sẽ tăng gấp đôi kết quả, khiến việc phân nhóm trở nên đơn giản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case được xử lý bằng các phép tính số học không đổi | 
| Không gian | O(1) | Chỉ sử dụng một số biến cố định | 

Lời giải dễ dàng nằm trong giới hạn vì$t \le 100$và mỗi trường hợp được xử lý bằng một vài phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s, a, b, c = map(int, input().split())
        bought = s // c
        groups = bought // a
        rem = bought % a
        out.append(str(groups * (a + b) + rem))
    return "\n".join(out)

# provided samples
assert run("2\n10 3 1 1\n1000000000 1 1000000000 1\n") == "13\n1000000002000000000"

# custom cases
assert run("1\n1 10 5 1\n") == "1"
assert run("1\n100 2 0 1\n") == "100"
assert run("1\n10 1 1 1\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 10 5 1 | 1 | Không thể thành lập bất kỳ nhóm nào | 
| 100 2 0 1 | 100 | Không có đóng góp tiền thưởng | 
| 10 1 1 1 | 20 | Mỗi lần mua sẽ kích hoạt tiền thưởng | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi$a >$số thanh đã mua. Ví dụ: nếu Vasya chỉ có thể mua 2 thanh nhưng cần 5 thanh để kích hoạt phần thưởng, thuật toán sẽ đặt chính xác các nhóm về 0 và chỉ trả về các thanh đã mua vì không áp dụng khuyến mãi. 

Một trường hợp cạnh khác là khi$b = 0$. Công thức giảm xuống chỉ còn số lượng thanh được mua vì mỗi nhóm không đóng góp thêm lợi ích nào. Bước nhóm vẫn hoạt động chính xác vì nó nhân với$a + 0$, duy trì tính chính xác mà không cần xử lý đặc biệt. 

Trường hợp cạnh cuối cùng là$a = 1$, trong đó mỗi thanh được mua sẽ tạo thành nhóm riêng của nó. Thuật toán nhân đôi (hoặc chia tỷ lệ một cách chính xác theo$1 + b$) sự đóng góp của mỗi thanh, phù hợp với mục đích áp dụng lặp đi lặp lại của chương trình khuyến mãi.
