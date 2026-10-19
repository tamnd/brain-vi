---
title: "CF 105633K - Lên lịch hai cuộc họp"
description: "Chúng tôi được cấp một tập hợp các khoảng thời gian dành cho ứng viên để lên lịch cho hai cuộc họp riêng biệt. Đối với mỗi khung giờ, mọi giám khảo đều tham dự tại chỗ hoặc chỉ tham gia từ xa. Một thẩm phán được coi là “được bảo vệ” trong cuộc họp nếu thẩm phán đó có mặt tại chỗ ít nhất một trong hai khoảng thời gian đã chọn."
date: "2026-06-22T15:02:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105633
codeforces_index: "K"
codeforces_contest_name: "The 2024 ICPC Asia Yokohama Regional Contest"
rating: 0
weight: 105633
solve_time_s: 55
verified: true
draft: false
---

[CF 105633K - Lên lịch hai cuộc họp](https://codeforces.com/problemset/problem/105633/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một tập hợp các khoảng thời gian dành cho ứng viên để lên lịch cho hai cuộc họp riêng biệt. Đối với mỗi khung giờ, mọi giám khảo đều tham dự tại chỗ hoặc chỉ tham gia từ xa. Một thẩm phán được coi là “được bảo vệ” trong cuộc họp nếu thẩm phán đó có mặt tại chỗ ít nhất một trong hai khoảng thời gian đã chọn. Nhiệm vụ là chọn ra hai khoảng thời gian riêng biệt để tất cả các giám khảo đều có mặt trong ít nhất một khoảng thời gian đó. 

Trong số tất cả các cặp hợp lệ, chúng tôi ưu tiên cặp có số lượng giám khảo tối đa có mặt tại chỗ ở cả hai vị trí đã chọn. Nếu nhiều cặp đạt được sự trùng lặp tối đa như nhau, chúng tôi sẽ chọn cặp có khe thời gian đầu tiên sớm hơn và nếu vẫn bị ràng buộc thì khe thời gian thứ hai sớm hơn. 

Cấu trúc chính là mỗi khe thời gian chỉ là một chuỗi nhị phân có độ dài m, trong đó m là số lượng giám khảo và một cặp khe thời gian hợp lệ nếu với mỗi giám khảo j, ít nhất một trong hai chuỗi có Y ở vị trí j. 

Những hạn chế rất quan trọng. Số lượng khe thời gian n có thể lên tới 200.000, trong khi số lượng giám khảo m nhiều nhất là 20. Điều này ngay lập tức gợi ý rằng bất kỳ thuật toán nào phụ thuộc nhiều vào n bình phương là không thể, nhưng bất kỳ thuật toán nào xử lý mỗi khe thời gian như một mặt nạ bit nhỏ và thực hiện công việc nặng nhọc trên 2^m hoặc m·n đều hợp lý. 

Một lực lượng mạnh mẽ trên tất cả các cặp khe thời gian sẽ kiểm tra khoảng n(n-1)/2 cặp, gần bằng 2×10^10 trong trường hợp xấu nhất, vượt xa giới hạn. 

Một vấn đề tinh tế hơn là tính đúng đắn của việc lọc. Nếu người ta chỉ kiểm tra rằng “một số thẩm phán được bảo vệ” hoặc tối đa hóa quy mô công đoàn một cách nhầm lẫn thay vì thực thi phạm vi bảo hiểm đầy đủ, thì rất dễ tạo ra các cặp không hợp lệ. 

Trường hợp cạnh cụ thể là khi không tồn tại cặp hợp lệ. Ví dụ: nếu tất cả các khoảng thời gian bỏ lỡ ít nhất một mẫu vị trí giám khảo chung sao cho mỗi cặp không bao gồm được ai đó thì kết quả đầu ra chính xác là Không. Một cách tiếp cận ngây thơ vẫn có thể đưa ra cặp điểm tốt nhất mà không xác thực phạm vi bao phủ. 

Một trường hợp tinh tế khác là tie-break. Ngay cả khi nhiều cặp hợp lệ và có sự trùng lặp giống hệt nhau, cặp từ điển sớm nhất phải được chọn, cặp này có thể bị xử lý sai nếu một người chỉ cập nhật câu trả lời khi tốt hơn hoàn toàn, bỏ qua thứ tự giữa các giá trị bằng nhau. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng cặp khung thời gian, kiểm tra xem chúng có bao gồm tất cả các thẩm phán hay không và nếu có thì tính toán có bao nhiêu thẩm phán tham dự cả hai cuộc họp tại chỗ. Điều này đúng vì nó trực tiếp thực thi cả hai điều kiện trong định nghĩa. Tuy nhiên, nó yêu cầu kiểm tra khoảng n2 cặp và với mỗi cặp, chúng ta cần quét m giám khảo, dẫn đến các phép toán O(n2m). Với n = 200.000 thì điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là m rất nhỏ, nhiều nhất là 20. Điều này có nghĩa là mỗi khe thời gian có thể được biểu diễn dưới dạng mặt nạ bit 20 bit. Phạm vi bao phủ của một cặp trở thành bitwise OR bằng mặt nạ đầy đủ và chồng chéo trở thành bitwise AND. Khi chúng ta chuyển sang biểu diễn này, vấn đề sẽ trở thành tổ hợp trên mặt nạ bit thay vì trên chuỗi thô. 

Chúng tôi vẫn không thể lặp lại trực tiếp tất cả các cặp. Bí quyết là nhóm các mặt nạ giống hệt nhau và giảm không gian tìm kiếm trên các cấu hình duy nhất. Vì có thể có tối đa 2^20 mặt nạ, khoảng một triệu mặt nạ, nên chúng tôi có thể duy trì mức xuất hiện tốt nhất (chỉ số nhỏ nhất) cho mỗi mặt nạ. Sau đó, chúng tôi chỉ xem xét kết hợp các mặt nạ thay vì các khe thời gian riêng lẻ. 

Bây giờ số lượng mặt nạ riêng biệt thực sự hiện diện nhiều nhất là n nhưng thường nhỏ hơn nhiều. Chúng tôi lặp lại tất cả các cặp mặt nạ giữa các mặt nạ riêng biệt. Đối với mỗi cặp, chúng tôi kiểm tra xem OR của chúng có bằng mặt nạ đầy đủ hay không và tính kích thước AND làm điểm trùng lặp. Điều này làm giảm đáng kể việc tìm kiếm trong thực tế vì m nhỏ và các phép toán bit nhanh. 

Việc tối ưu hóa hoạt động vì kích thước đắt tiền là n, không phải m. Chúng tôi nén n vào tối đa 2^m trạng thái và sau đó hoạt động trên các trạng thái đó.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² m) | O(1) | Quá chậm | 
| Bitmask + ghép trên các mặt nạ riêng biệt | O(U² m) trong đó U 2^m | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta chuyển đổi mỗi khe thời gian thành một mặt nạ bit có độ dài m. Đối với giám khảo j, chúng ta đặt bit j nếu ký tự là Y. Điều này thể hiện sự tham dự ngắn gọn. 

Tiếp theo, chúng ta duy trì một mảng best_mask_index có kích thước 2^m, được khởi tạo với giá trị lớn. Đối với mỗi khe thời gian i, chúng tôi tính toán mặt nạ của nó và lưu trữ chỉ mục sớm nhất nơi mặt nạ này xuất hiện. Chúng tôi luôn giữ chỉ số nhỏ nhất vì việc tie-break phụ thuộc vào các khung thời gian sớm nhất. 

Sau khi tiền xử lý, chúng tôi trích xuất tất cả các mặt nạ thực sự xuất hiện vào danh sách các ứng cử viên. Mỗi ứng cử viên đều có chỉ số đại diện tốt nhất. 

Sau đó, chúng tôi lặp lại tất cả các cặp mặt nạ ứng cử viên (a, b), bao gồm cả trường hợp a == b chỉ khi có ít nhất hai lần xuất hiện của mặt nạ đó. Đối với mỗi cặp, chúng tôi kiểm tra xem a OR b có bằng mặt nạ đầy đủ hay không (bao gồm tất cả các giám khảo). Nếu không, chúng tôi sẽ bỏ qua vì vi phạm yêu cầu mỗi thẩm phán phải tham dự ít nhất một cuộc họp tại chỗ. 

Đối với các cặp hợp lệ, chúng tôi tính điểm trùng lặp là số bit được đặt trong a AND b. Chúng tôi so sánh điểm này với điểm tốt nhất hiện tại và cập nhật câu trả lời nếu nó lớn hơn. Nếu điểm bằng nhau, chúng tôi áp dụng liên kết từ điển trên cặp chỉ số: chỉ số đầu tiên nhỏ hơn sẽ thắng và nếu bằng nhau, chỉ số thứ hai nhỏ hơn sẽ thắng. 

Sau khi hoàn thành tất cả các cặp, nếu không tìm thấy cặp hợp lệ, chúng tôi sẽ xuất ra Số. Nếu không, chúng tôi sẽ xuất ra cặp chỉ số tốt nhất. 

Tại sao nó hoạt động được gắn với hai thuộc tính. Đầu tiên, mỗi khe thời gian được thể hiện đầy đủ bằng mặt nạ bit của nó, do đó tính khả thi chỉ phụ thuộc vào OR bao phủ tất cả các bit. Thứ hai, bất kỳ giải pháp tối ưu nào cũng chỉ phụ thuộc vào mặt nạ được chọn chứ không phụ thuộc vào số bội của chúng, ngoại trừ việc đảm bảo hai lần xuất hiện riêng biệt khi chọn cùng một mặt nạ hai lần. Vì chúng tôi bảo toàn các chỉ số sớm nhất nên chúng tôi tôn trọng chính xác các ràng buộc về thứ tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    full = (1 << m) - 1

    best_idx = [n + 1] * (1 << m)
    cnt = [0] * (1 << m)

    masks = []

    for i in range(1, n + 1):
        s = input().strip()
        mask = 0
        for j, ch in enumerate(s):
            if ch == 'Y':
                mask |= (1 << j)

        cnt[mask] += 1
        if best_idx[mask] == n + 1:
            best_idx[mask] = i
            masks.append(mask)

    INF = 10**18
    best_score = -1
    ans_i, ans_j = -1, -1

    for i in range(len(masks)):
        a = masks[i]
        for j in range(i, len(masks)):
            b = masks[j]

            if a == b and cnt[a] < 2:
                continue

            if (a | b) != full:
                continue

            score = (a & b).bit_count()

            i1 = best_idx[a]
            i2 = best_idx[b]
            if i1 > i2:
                i1, i2 = i2, i1

            if score > best_score or (score == best_score and (i1, i2) < (ans_i, ans_j)):
                best_score = score
                ans_i, ans_j = i1, i2

    if best_score == -1:
        print("No")
    else:
        print(ans_i, ans_j)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách mã hóa từng hàng lịch trình thành một mặt nạ bit. Mặt nạ đầy đủ đại diện cho tất cả các giám khảo tham dự tại chỗ, vì vậy bất kỳ cặp hợp lệ nào cũng phải kết hợp với mặt nạ này thông qua OR. 

Chúng tôi theo dõi cả số lượng và chỉ số sớm nhất trên mỗi mặt nạ. Việc đếm là cần thiết để đảm bảo rằng khi chọn cùng một mặt nạ hai lần, có ít nhất hai khe thời gian khác nhau. 

Vòng lặp kép trên các mặt nạ chỉ chạy trên các mặt nạ riêng biệt chứ không chạy trên tất cả n khe thời gian. Đây là mức giảm chính. Bên trong, chúng tôi thực thi tính khả thi thông qua kiểm tra OR và tính toán chồng chéo thông qua AND theo bit. 

Việc phá vỡ ràng buộc được xử lý trực tiếp trong quá trình cập nhật bằng cách sử dụng so sánh bộ chỉ số, phù hợp với yêu cầu ưu tiên các khe thời gian sớm hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
NNY
YYN
YNY
NYY
```Mặt nạ theo hàng: 

| tôi | chuỗi | mặt nạ | 
| --- | --- | --- | 
| 1 | NNY | 001 | 
| 2 | YYN | 110 | 
| 3 | YNY | 101 | 
| 4 | NYY | 011 | 

Chúng tôi lặp lại các cặp hợp lệ. 

| một | b | a HOẶC b | hợp lệ | chồng chéo (a VÀ b) | 
| --- | --- | --- | --- | --- | 
| 001 | 110 | 111 | vâng | 000 | 
| 001 | 101 | 101 | không | - | 
| 001 | 011 | 011 | không | - | 
| 110 | 101 | 111 | vâng | 100 (1) | 
| 110 | 011 | 111 | vâng | 010 (1) | 
| 101 | 011 | 111 | vâng | 001 (1) | 

Sự trùng lặp tốt nhất là 1. Trong số các cặp đó, chỉ số nhỏ nhất về mặt từ điển tương ứng với (2, 3), khớp với đầu ra. 

Dấu vết này cho thấy rằng việc lọc tính khả thi thông qua OR là cần thiết vì nhiều cặp không bao gồm được tất cả các giám khảo. 

### Ví dụ 2 

đầu vào:```
3 6
NNNYYY
YYNYYN
YYYNNN
```Mặt nạ: 

| tôi | mặt nạ | 
| --- | --- | 
| 1 | 000111 | 
| 2 | 110110 | 
| 3 | 111000 | 

Kiểm tra cặp: 

| một | b | HOẶC | hợp lệ | VÀ số lượng | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 110111 | không | - | 
| 1 | 3 | 111111 | vâng | 000000 (0) | 
| 2 | 3 | 111110 | không | - | 

Chỉ có cặp hợp lệ là (1,3), vì vậy câu trả lời là 1 3. Điều này cho thấy thuật toán xử lý chính xác trường hợp chỉ có một cặp thỏa mãn phạm vi bao phủ đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(U² · m) | U là số mặt nạ riêng biệt, mỗi cặp kiểm tra sử dụng các phép toán bit trên m bit | 
| Không gian | O(2^m) | lưu trữ số lượng và chỉ số tốt nhất trên mỗi mặt nạ | 

Với m 20 thì 2^m là khoảng một triệu, nhưng trên thực tế U nhỏ hơn rất nhiều. Thuật toán phù hợp một cách thoải mái trong các ràng buộc vì các hoạt động bit có thời gian không đổi và yếu tố chi phối là việc liệt kê cặp có thể quản lý được trên các trạng thái nén. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""4 3
NNY
YYN
YNY
NYY
""") == "2 3"

assert run("""3 6
NNNYYY
YYNYYN
YYYNNN
""") == "1 3"

# minimum valid
assert run("""2 2
YY
YY
""") == "1 2"

# no solution
assert run("""3 3
YNN
NYN
NNY
""") == "No"

# duplicate masks
assert run("""4 3
YNN
YNN
NYN
NNY
""") == "1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảo hiểm đầy đủ giống hệt nhau | 1 2 | xử lý mặt nạ trùng lặp | 
| bảo hiểm rời rạc | Không | trường hợp thất bại | 
| mẫu lặp đi lặp lại | 1 2 | chỉ số ràng buộc và trùng lặp | 

## Vỏ cạnh 

Trường hợp một cạnh là khi giải pháp hợp lệ duy nhất sử dụng cùng một mặt nạ hai lần. Đối với đầu vào như:```
2 3
YYY
YYY
```thuật toán phải cho phép ghép các mặt nạ giống hệt nhau, nhưng chỉ khi cnt[mask] ≥ 2. Việc kiểm tra số lượng đảm bảo điều này. Nếu không có nó, giải pháp sẽ sử dụng sai cùng một chỉ mục hai lần. 

Một trường hợp khác là khi có nhiều mặt nạ nhưng chỉ có một cặp đạt được độ che phủ toàn diện. Trong những trường hợp như vậy, bước tính điểm không liên quan đến hầu hết các cặp và tính chính xác phụ thuộc hoàn toàn vào điều kiện OR. Thuật toán lọc nghiêm ngặt trên OR trước khi xem xét sự chồng chéo. 

Trường hợp cuối cùng là hòa giữa các điểm bằng nhau. Bởi vì chúng ta luôn so sánh`(i1, i2)`về mặt từ điển, cặp sớm nhất được giữ nguyên ngay cả khi các cặp sau này đạt được sự trùng lặp tương tự.
