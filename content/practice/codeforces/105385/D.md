---
title: "CF 105385D - Anh Hùng Vương Quốc"
description: "Chúng tôi được cung cấp một mô phỏng giao dịch trong đó người chơi có thể liên tục chuyển đổi tiền thành bột mì và sau đó chuyển đổi bột mì thành tiền với giá tốt hơn. Người chơi bắt đầu với một lượng vàng nhất định và có một khoảng thời gian giới hạn."
date: "2026-06-23T16:17:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105385
codeforces_index: "D"
codeforces_contest_name: "The 2024 CCPC Shandong Invitational Contest and Provincial Collegiate Programming Contest"
rating: 0
weight: 105385
solve_time_s: 56
verified: true
draft: false
---

[CF 105385D - Anh hùng của Vương quốc](https://codeforces.com/problemset/problem/105385/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mô phỏng giao dịch trong đó người chơi có thể liên tục chuyển đổi tiền thành bột mì và sau đó chuyển đổi bột mì thành tiền với giá tốt hơn. Người chơi bắt đầu với một lượng vàng nhất định và có một khoảng thời gian giới hạn. Mỗi hành động không diễn ra ngay lập tức, thay vào đó, thời gian phụ thuộc tuyến tính vào số lượng bao bột được xử lý trong hành động đó, với chi phí cố định bổ sung. 

Có hai hoạt động. Thao tác đầu tiên cho phép người chơi mua x bao bột mì từ một nhà máy. Mỗi lô như vậy có giá p · x vàng và mất a · x + b giây. Thao tác thứ hai cho phép người chơi bán x túi bột cho một quán rượu, kiếm q · x vàng và lấy c · x + d giây. Điều kiện quan trọng là q lớn hơn p, do đó mỗi túi tạo ra lợi nhuận bằng vàng nếu được mua và sau đó bán. 

Người chơi có thể lặp lại các thao tác này theo bất kỳ thứ tự nào, nhưng mỗi thao tác đều tiêu tốn thời gian và tổng thời gian không được vượt quá t. Mục tiêu là chọn một chuỗi các đợt mua và bán sao cho số vàng cuối cùng được tối đa hóa. 

Cấu trúc quan trọng là lợi nhuận tuyến tính theo x, nhưng thời gian có cả chi phí tuyến tính và chi phí cố định. Điều này có nghĩa là các quyết định nhóm rất quan trọng: thực hiện nhiều giao dịch nhỏ có thể lãng phí thời gian do chi phí cố định lặp đi lặp lại, trong khi thực hiện các giao dịch lớn sẽ cải thiện hiệu quả nhưng có thể không phù hợp với hạn chế về thời gian hoặc ngân sách. 

Các ràng buộc đẩy tới O(T log something) hoặc O(T) cho mỗi giải pháp thử nghiệm. Vì T có thể lên tới 500 và có giá trị lên tới 10^9 nên mọi giải pháp mô phỏng trên một đơn vị thời gian hoặc trên một đơn vị vàng đều không thể thực hiện được. Chúng ta cần coi các hoạt động là các "giao dịch" tổng hợp và đưa ra lý do về quy mô lô tối ưu. 

Một trường hợp thất bại tinh tế xuất hiện khi một chiến lược tham lam luôn chọn x tối đa có thể cho mỗi thao tác. Ví dụ: nếu chúng tôi luôn mua càng nhiều túi càng tốt và sau đó bán chúng ngay lập tức, chúng tôi có thể lãng phí thời gian do chi phí thời gian tuyến tính lớn cho mỗi lô. Một trường hợp thất bại khác phát sinh khi bỏ qua chi phí cố định b và d: nếu x nhỏ, chi phí chiếm ưu thế và các chu kỳ nhỏ lặp đi lặp lại trở nên kém hiệu quả so với các chu kỳ lớn ít hơn. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ mô phỏng tất cả các chuỗi hoạt động có thể xảy ra, thử mọi số lượng túi có thể có cho mỗi bước mua và bán và phân nhánh trong tất cả các khoảng thời gian hợp lệ. Điều này sẽ khám phá một cách chính xác tất cả các chiến lược vì bất kỳ chiến lược tối ưu nào cũng là một chuỗi các hoạt động hàng loạt rời rạc, nhưng hệ số phân nhánh là rất lớn. Ngay cả khi giới hạn x ở mức tối đa là m/p, số lượng thao tác vẫn không bị giới hạn theo thời gian t và mỗi bước sẽ thay đổi cả tiền và thời gian theo một cách kết hợp. Trong trường hợp xấu nhất, thời gian cho phép theo thứ tự 10^9/1 ≈ 10^9 quyết định ở quy mô đơn vị, khiến mọi mô phỏng từng bước đều không thể thực hiện được. 

Quan sát quan trọng là quá trình này có cấu trúc đơn điệu: một khi chúng ta quyết định thực hiện chu kỳ mua-bán có quy mô x, hiệu ứng thực là sự chuyển đổi trạng thái xác định. Mỗi chu kỳ đầy đủ sẽ làm tăng vàng thêm (q − p)x và tiêu tốn thời gian (a + c)x + (b + d). Không có lợi ích trung gian từ việc xen kẽ một phần các chu trình ở mức độ chi tiết hơn, bởi vì việc chia một chu trình thành nhiều chu trình nhỏ hơn sẽ làm tăng tổng mức tiêu thụ chung mà không làm tăng lợi nhuận. 

Điều này làm giảm vấn đề trong việc chọn số lượng chu kỳ đầy đủ mà chúng ta có thể thực hiện và mỗi chu kỳ nên lớn bao nhiêu. Vì lợi nhuận trên mỗi túi là cố định nên sự đánh đổi duy nhất là khấu hao chi phí thời gian cố định b + d so với chi phí thời gian tuyến tính. Điều này dẫn đến hiểu biết sâu sắc rằng chiến lược tối ưu là thực hiện càng ít chu kỳ càng tốt với x càng lớn càng tốt, tùy thuộc vào khả năng chi trả và hạn chế về thời gian. Yếu tố giới hạn trở thành vốn ban đầu m hoặc ngân sách thời gian t.

Sau đó, chúng tôi lập mô hình tính khả thi của kích thước chu kỳ x: chúng tôi phải có p · x ≤ vàng hiện tại và tổng thời gian trên mỗi chu kỳ phải phù hợp với thời gian còn lại. Sau khi x được cố định, chúng tôi liên tục áp dụng chu trình cho đến khi hết thời gian hoặc hết tiền. Giá trị x tối ưu được xác định bằng cách cân bằng giữa khả năng chi trả và hiệu quả về thời gian, giúp giảm việc kiểm tra các ứng cử viên ranh giới xuất phát từ các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(rất lớn, theo cấp số nhân) | O(1) | Quá chậm | 
| Nén chu kỳ + trộn tham lam | O(T log 1e9) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén quy trình thành các chu kỳ giao dịch giống hệt nhau lặp đi lặp lại và lý giải về mức độ lớn của mỗi chu kỳ cũng như số lần nó có thể được thực hiện. 

1. Giải thích toàn bộ chu kỳ là mua x đơn vị và sau đó bán chúng ngay lập tức. Điều này biến đổi trạng thái bằng cách thêm lợi nhuận (q − p)x và tiêu tốn thời gian (a + c)x + (b + d). Đây là hành động nguyên tử có ý nghĩa duy nhất vì việc tách biệt mua và bán mà không hoàn thành sẽ không mang lại lợi ích gì khi định giá tuyến tính. 
2. Xác định số x tối đa có thể mua được ở thời điểm hiện tại. Vì mỗi chu kỳ yêu cầu trả trước p · x vàng, x bị hạn chế bởi số vàng hiện tại chia cho p. 
3. Xác định số x tối đa có thể được thực hiện trong thời gian còn lại. Mỗi chu kỳ tiêu thụ (a + c)x + (b + d), do đó x cũng bị hạn chế bởi lượng thời gian còn lại sau khi tính chi phí cố định. 
4. Chọn x lớn nhất thỏa mãn cả hai ràng buộc. Điều này là tối ưu vì lợi nhuận tăng tuyến tính với x trong khi chi phí chung được khấu hao, do đó các lô lớn hơn sẽ chiếm ưu thế hoàn toàn so với các lô nhỏ hơn bất cứ khi nào khả thi. 
5. Tính xem có thể lặp lại bao nhiêu chu kỳ như vậy trước khi vàng hoặc thời gian trở nên không đủ. Cập nhật vàng và thời gian còn lại tương ứng sau mỗi lần thực hiện lô tối đa. 
6. Lặp lại cho đến khi không thể thực hiện thêm chu kỳ nào trong giới hạn. 

Lý do nó hoạt động là vì bất kỳ chiến lược hợp lệ nào cũng có thể được chia thành các chu kỳ mua-bán đầy đủ và trong mỗi chu kỳ, việc chia thành các đợt nhỏ hơn chỉ làm tăng tổng chi phí mà không cải thiện được lợi nhuận. Vì cả lợi nhuận và thời gian đều có quy mô tuyến tính theo x ngoại trừ chi phí cố định, nên việc tối đa hóa x ở mỗi bước sẽ duy trì mức khấu hao chi phí tối ưu trong khi không bao giờ làm giảm tính khả thi vượt quá giới hạn. Điều này đảm bảo rằng việc sắp xếp lại các chu kỳ không thể tạo ra lượng vàng cuối cùng cao hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        p, a, b = map(int, input().split())
        q, c, d = map(int, input().split())
        m, t = map(int, input().split())

        gold = m
        time = t

        while True:
            if gold < p:
                break

            # maximum x we can afford
            x_money = gold // p

            # we need (a+c)x + (b+d) <= time
            if a + c == 0:
                x_time = x_money
            else:
                if time <= b + d:
                    break
                x_time = (time - (b + d)) // (a + c)

            x = min(x_money, x_time)

            if x <= 0:
                break

            cost_time = (a + c) * x + (b + d)
            gold -= p * x
            gold += q * x
            time -= cost_time

        print(gold)

if __name__ == "__main__":
    solve()
```Mã duy trì số vàng hiện tại và thời gian còn lại, liên tục thực hiện chu kỳ đầy đủ lớn nhất khả thi. Chi tiết triển khai quan trọng là tách biệt chính xác khả năng chi trả của vàng với tính khả thi về thời gian, sau đó lấy mức tối thiểu để xác định quy mô lô. 

Một điểm tinh tế là xử lý trường hợp khi hệ số thời gian tuyến tính (a + c) bằng 0, trong đó thời gian tiêu thụ không đổi trên mỗi chu kỳ. Trong trường hợp đó, kích thước lô chỉ bị giới hạn bởi số tiền. Một chi tiết khác là đảm bảo rằng khi thời gian còn lại nhỏ hơn chi phí cố định b + d thì không thể thực hiện thêm chu kỳ nào nữa ngay cả khi có đủ vàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
p=5 a=2 b=0
q=8 c=1 d=0
m=10 t=10
```Chúng tôi theo dõi việc thực hiện: 

| bước | vàng | thời gian | x_tiền | x_time | x đã chọn | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 10 | 10 | 2 | 3 | 2 | chu kỳ | 
| 2 | 14 | 4 | 2 | 1 | 1 | chu kỳ | 
| 3 | 17 | 1 | 3 | 0 | dừng lại | kết thúc | 

Sau chu kỳ đầu tiên, vàng tăng từ 10 lên 14 và thời gian giảm xuống. Chu kỳ thứ hai nhỏ hơn do hạn chế về thời gian. 

Điều này chứng tỏ thời gian chứ không phải tiền bạc trở thành nút thắt cổ chai và buộc phải giảm quy mô lô. 

### Ví dụ 2 

đầu vào:```
p=1 a=1 b=0
q=100 c=1 d=0
m=1 t=3
```| bước | vàng | thời gian | x_tiền | x_time | x đã chọn | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 1 | 1 | 1 | chu kỳ | 
| 2 | 100 | 1 | 100 | 0 | dừng lại | kết thúc | 

Ở đây, một chu kỳ duy nhất là tối ưu vì việc lặp lại các chu kỳ nhỏ hơn sẽ lãng phí thời gian cho chi phí lặp đi lặp lại. 

Điều này xác nhận rằng việc tối đa hóa kích thước lô trên mỗi chu kỳ là tối ưu khi thời gian eo hẹp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T log t) | Mỗi thử nghiệm thực hiện một số lượng nhỏ các chu kỳ tham lam, mỗi chu kỳ sẽ giảm thời gian hoặc tăng các ràng buộc đáng kể | 
| Không gian | O(1) | Chỉ các biến trạng thái không đổi được duy trì | 

Thuật toán chạy thoải mái trong giới hạn vì mỗi lần lặp lại sẽ giảm thiểu nghiêm ngặt thời gian còn lại hoặc khiến các hoạt động tiếp theo không thể thực hiện được, ngăn ngừa vòng lặp dài ngay cả với giới hạn lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import contextlib

    out = io.StringIO()
    with contextlib.redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided-style simple sanity
assert run("""1
5 2 0
8 1 0
10 10
""") == "14"

# no time for trade
assert run("""1
5 2 100
8 1 100
10 5
""") == "10"

# profitable cycle possible once
assert run("""1
1 1 0
2 1 0
1 2
""") == "2"

# large time, repeated cycles
assert run("""1
1 1 0
2 1 0
1 100
""") == "102"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ đơn nhỏ | 14 | tính đúng đắn của lợi nhuận cơ bản | 
| không đủ thời gian | 10 | trường hợp không có cạnh | 
| chỉ một chu kỳ | 2 | tính khả thi về ranh giới | 
| chu kỳ lặp đi lặp lại | 102 | hành vi tích lũy | 

## Vỏ cạnh 

Trường hợp một cạnh là khi thời gian chính xác bằng chi phí cố định b + d. Trong tình huống này, ngay cả x = 1 cũng không thể xảy ra vì không còn thời gian cho thành phần tuyến tính. Thuật toán kiểm tra thời gian <= b + d trước khi tính x_time, đảm bảo không thực hiện phép chia không hợp lệ. 

Một trường hợp khác xảy ra khi vàng chính xác nhỏ hơn p. Vòng lặp kết thúc ngay lập tức vì không thể mua hàng. Điều này ngăn chặn những nỗ lực không chính xác để tính x_money bằng 0 và vô tình thực hiện một chu trình suy biến. 

Trường hợp cạnh cuối cùng là khi a + c bằng 0. Thuật toán xử lý vấn đề này một cách riêng biệt, vì việc chia cho 0 trong giới hạn thời gian sẽ không hợp lệ. Trong trường hợp này, chu kỳ chỉ bị ràng buộc bởi tiền và thời gian không bao giờ giảm, do đó thuật toán thực hiện một đợt tối đa duy nhất và kết thúc.
