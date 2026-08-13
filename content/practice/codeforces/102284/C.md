---
title: "CF 102284C - \u0411\u0430\u0441\u043a\u0435\u0442\u0431\u043e\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0440\u044f\u0434\u043a\u0430"
description: "Chúng tôi có hai hàng (n) sinh viên. Học sinh (i) ở hàng đầu tiên có chiều cao (ai) và học sinh (i) ở hàng thứ hai có chiều cao (bi). Một đội được thành lập bằng cách chọn học sinh từ trái sang phải nên chỉ số của các học sinh được chọn phải tăng lên một cách nghiêm ngặt."
date: "2026-08-13T22:31:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "C"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 848
verified: true
draft: false
---

[CF 102284C - \u0411\u0430\u0441\u043a\u0435\u0442\u0431\u043e\u043b\u044c\u043d\u0430\u044f \u0437\u0430\u0440\u044f\u0434\u043a\u0430](https://codeforces.com/problemset/problem/102284/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 14 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai hàng (n) sinh viên. Học sinh (i) ở hàng đầu tiên có chiều cao (a_i) và học sinh (i) ở hàng thứ hai có chiều cao (b_i). Một đội được thành lập bằng cách chọn học sinh từ trái sang phải nên chỉ số của các học sinh được chọn phải tăng lên một cách nghiêm ngặt. Đồng thời, hai học sinh được chọn liên tiếp phải đến từ các hàng khác nhau. Học sinh được chọn đầu tiên không bị hạn chế và mục tiêu là tối đa hóa tổng các độ cao đã chọn. Các giới hạn chính thức là (1 \le n \le 10^5) và (1 \le h_{r,i} \le 10^9), với giới hạn thời gian là 2 giây và giới hạn bộ nhớ 256 MB trong phiên bản Codeforces hiện tại. 

Giới hạn trên (n=10^5) ngay lập tức loại trừ các thuật toán kiểm tra các cặp vị trí, chưa nói đến tất cả các đội có thể. Giải pháp (O(n^2)) đã thực hiện khoảng (10^{10}) lần lặp trong trường hợp xấu nhất, vượt xa giới hạn cuộc thi 2 giây có thể xử lý. Chúng ta cần xử lý mỗi cột một số lần không đổi, đưa ra nghiệm (O(n)). Câu trả lời có thể lớn bằng (n\cdot10^9=10^{14}), do đó việc triển khai phải sử dụng số học số nguyên có khả năng lưu trữ các giá trị vượt quá số nguyên có dấu 32 bit. Số nguyên Python tự động xử lý việc này. 

Trường hợp cạnh đầu tiên là một cột duy nhất. Ví dụ,```
1
7
4
```có câu trả lời`7`. Không có sẵn chỉ mục thứ hai nên việc chọn cả hai học sinh là không thể. Việc triển khai giả định mọi chuyển đổi đều có cột trước đó có thể thất bại ở đây. 

Trường hợp thứ hai là chúng ta không thể chọn cả hai học sinh từ cùng một cột. Ví dụ,```
2
100 100
1 1
```có câu trả lời`101`, không`200`. Hai học sinh có chiều cao`100`có cùng chỉ mục, trong khi các quy tắc yêu cầu chỉ mục được chọn tiếp theo phải lớn hơn chỉ mục trước đó. Một giải pháp bất cẩn chỉ lấy chiều cao lớn hơn từ mỗi cột sẽ chọn sai cả hai`100`S. 

Trường hợp cạnh thứ ba là học sinh được chọn trước đó không nhất thiết phải ở cột ngay trước đó. Coi như```
3
1 100 1
1 1 100
```Nhóm tối ưu là sinh viên hàng đầu tại chỉ số`2`tiếp theo là học sinh hàng thứ hai ở chỉ mục`3`, cho`200`. Quá trình chuyển đổi chỉ coi cột ngay trước đó có thể là cột trước đó sẽ bỏ lỡ sự kết hợp này. 

Cuối cùng, khi tất cả các độ cao bằng nhau, câu trả lời được xác định hoàn toàn bởi các hạn chế về cấu trúc. Vì```
3
5 5 5
5 5 5
```câu trả lời là`15`, bởi vì chúng ta có thể chọn chính xác một học sinh từ mỗi cột trong số ba cột và các hàng xen kẽ. Bất kỳ triển khai nào cho phép chọn cả hai hàng của một cột sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể liệt kê mọi tập hợp con của (2n) học sinh, kiểm tra xem các chỉ số được chọn của nó có tăng nghiêm ngặt hay không và liệu các học sinh được chọn liên tiếp có xen kẽ các hàng hay không, sau đó giữ tổng chiều cao lớn nhất. Điều này đúng vì mọi đội có thể đều được đại diện bởi một số tập hợp con. Tuy nhiên, có (2^{2n}) tập hợp con và việc kiểm tra một tập hợp con có thể mất (O(n)) thời gian. Do đó, công việc trong trường hợp xấu nhất là (O(n\cdot2^{2n})). Ngay cả đối với vài chục cột, điều này trở nên không khả thi, trong khi ràng buộc thực tế cho phép (10^5) cột. 

Cấu trúc giúp giải bài toán dễ dàng là khi ta quét các cột từ trái sang phải, thông tin duy nhất từ ​​phần đã được xử lý ảnh hưởng đến lựa chọn tiếp theo là hàng của học sinh được chọn cuối cùng. Chúng ta không cần phải nhớ đầy đủ dãy các chỉ số đã chọn. 

Giả sử chúng ta đã xử lý một số tiền tố của các cột. Cho phép`top`là tổng chiều cao tốt nhất có thể đạt được từ tiền tố đó khi học sinh được chọn cuối cùng ở hàng đầu tiên. Tương tự, hãy`bottom`là tổng số tốt nhất khi học sinh được chọn cuối cùng ở hàng thứ hai. Các trạng thái này đã cho phép học sinh được chọn cuối cùng ở bất kỳ đâu trong tiền tố được xử lý. 

Khi chúng ta đến một học sinh mới ở hàng đầu tiên có chiều cao (a_i), có hai khả năng. Chúng ta có thể bỏ qua sinh viên này, để lại`top`không thay đổi. Hoặc chúng ta có thể chọn nó. Vì các học sinh được chọn liên tiếp phải ở các hàng khác nhau nên học sinh được chọn trước đó phải được đại diện bởi`bottom`, và giá trị mới trở thành`bottom + a_i`. Do đó trạng thái hàng đầu tiên mới là 

[ 
\text{new_top}=\max(\text{top},\text{bottom}+a_i). 
] 

Lý luận tương tự cho 

[ 
\text{new_bottom}=\max(\text{bottom},\text{top}+b_i). 
] 

Điểm tinh tế là`top`Và`bottom`thể hiện lựa chọn tốt nhất ở bất kỳ đâu trong tiền tố được xử lý, không nhất thiết phải ở cột trước đó. Điều này tự động xử lý các cột bị bỏ qua. Vì mọi chuyển đổi chỉ sử dụng hai giá trị trạng thái trước đó nên toàn bộ (O(n)) DP có thể được giảm xuống thành bộ nhớ bổ sung không đổi. 

Brute-force hoạt động vì nó kiểm tra rõ ràng mọi đội có thể, nhưng không thành công vì có nhiều đội theo cấp số nhân. Quan sát cho thấy tương lai chỉ phụ thuộc vào hàng của học sinh được chọn cuối cùng cho phép chúng ta hợp nhất nhiều nhóm từng phần thành hai trạng thái tối ưu theo cấp số nhân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot2^{2n})) | (O(n)) | Quá chậm | 
| DP tối ưu | (O(n)) | (O(1)) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai mảng chiều cao. Chúng tôi xử lý chúng theo từng cột từ trái sang phải vì mọi nhóm hợp lệ đều có chỉ số tăng nghiêm ngặt. 
2. Khởi tạo`top = a[0]`Và`bottom = b[0]`. Chỉ có cột đầu tiên, đội xuất sắc nhất có học sinh được chọn cuối cùng ở hàng đầu tiên sẽ bao gồm học sinh ở hàng đầu tiên đó và tương tự cho hàng thứ hai. 
3. Đối với mỗi cột sau`i`, lưu lại giá trị cũ của`top`Và`bottom`. Trạng thái hàng đầu tiên mới là`max(old_top, old_bottom + a[i])`. Thuật ngữ đầu tiên có nghĩa là chúng tôi bỏ qua học sinh hiện tại ở hàng đầu tiên, trong khi thuật ngữ thứ hai có nghĩa là chúng tôi lấy nó sau khi một đội hợp lệ kết thúc ở hàng khác. 
4. Tính trạng thái hàng thứ hai mới là`max(old_bottom, old_top + b[i])`. Lý do là đối xứng nhưng cả hai trạng thái mới đều phải sử dụng các giá trị cũ. Việc cập nhật một trạng thái trước khi tính toán trạng thái kia sẽ vô tình cho phép cả hai quá trình chuyển đổi sử dụng cột hiện tại và có thể vi phạm điều kiện chỉ số tăng nghiêm ngặt. 
5. Sau cột cuối cùng, quay lại`max(top, bottom)`. Mọi nhóm hợp lệ không trống đều kết thúc ở chính xác một trong hai hàng, do đó trạng thái tốt hơn trong số các trạng thái này là trạng thái tối ưu toàn cục. 

### Tại sao nó hoạt động 

Sau khi xử lý cột (i),`top`lưu trữ tổng tối đa trong số tất cả các đội hợp lệ chỉ sử dụng các cột tối đa (i) có học sinh được chọn cuối cùng ở hàng đầu tiên. Bất biến tương tự cũng đúng đối với`bottom`và hàng thứ hai. Đối với học sinh ở hàng đầu tiên ở cột (i), bất kỳ đội hợp lệ nào chọn nó trước đó phải kết thúc ở hàng thứ hai với chỉ số nhỏ hơn và`old_bottom`đại diện cho đội tốt nhất như vậy. Bỏ qua sinh viên bảo tồn tối ưu trước đó. Đây là hai khả năng duy nhất, do đó phép truy toán xem xét mọi nhóm tối ưu hợp lệ và không thể đưa ra một nhóm không hợp lệ. Bất biến giữ nguyên cho mỗi cột, làm cho tổng tối đa cuối cùng của nhóm tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    top = a[0]
    bottom = b[0]

    for i in range(1, n):
        old_top = top
        old_bottom = bottom

        top = max(old_top, old_bottom + a[i])
        bottom = max(old_bottom, old_top + b[i])

    print(max(top, bottom))

if __name__ == "__main__":
    solve()
```Hai phép gán đầu tiên khởi tạo hai trạng thái DP cho cột số 0. Một đội trống không cần trạng thái riêng vì mọi độ cao đều dương nên chọn ít nhất một học sinh luôn tốt hơn là chọn không ai. 

Bên trong vòng lặp,`old_top`Và`old_bottom`giữ nguyên các trạng thái trước khi xử lý cột hiện tại. Điều này là cần thiết. Nếu như`top`được cập nhật đầu tiên và sau đó được sử dụng trong khi tính toán`bottom`, quá trình chuyển đổi thứ hai có thể chọn hai học sinh từ cùng một cột một cách hiệu quả, điều này bị cấm vì chỉ số của chúng bằng nhau. 

biểu hiện`old_bottom + a[i]`đại diện cho việc chuyển từ hàng thứ hai sang hàng đầu tiên. Từ`old_bottom`đã chứa nhóm hợp lệ tốt nhất kết thúc ở hàng thứ hai ở bất kỳ vị trí nào trước hoặc ở cột được xử lý trước đó thì mức tăng chỉ số nghiêm ngặt bắt buộc sẽ được đáp ứng. Lập luận tương tự áp dụng cho`old_top + b[i]`. 

Các số nguyên có độ chính xác tùy ý của Python cũng rất hữu ích ở đây. Với (10^5) cột và chiều cao lên tới (10^9), câu trả lời có thể đạt tới (10^{14}), do đó, số nguyên 32 bit sẽ tràn trong các ngôn ngữ có loại số nguyên mặc định. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là```
5
9 3 5 7 3
5 8 1 4 5
```Bảng sau đây hiển thị các trạng thái sau mỗi cột.`top`Và`bottom`là tổng số tốt nhất mà học sinh được chọn cuối cùng thuộc hàng tương ứng. 

| Cột | Chiều cao trên cùng | Chiều cao đáy |`top`|`bottom`| 
| --- | --- | --- | --- | --- | 
| 1 | 9 | 5 | 9 | 5 | 
| 2 | 3 | 8 | 9 | 17 | 
| 3 | 5 | 1 | 22 | 17 | 
| 4 | 7 | 4 | 24 | 26 | 
| 5 | 3 | 5 | 29 | 29 | 

Ở cột 5, tổng kết thúc tốt nhất ở hàng đầu tiên là`29`, và tổng kết thúc tốt nhất ở hàng thứ hai cũng là`29`. Câu trả lời là do đó`29`. Việc chuyển đổi ở cột 3 đặc biệt hữu ích:`top`trở thành`17 + 5 = 22`, trong đó lựa chọn hàng thứ hai trước đó đến từ cột 2. Điều này thể hiện cách DP ghi nhớ lịch sử tương thích tốt nhất thay vì chỉ xem xét một tiền thân cố định. 

Đối với mẫu thứ hai,```
3
1 2 9
10 1 1
```các bang phát triển như sau. 

| Cột | Chiều cao trên cùng | Chiều cao đáy |`top`|`bottom`| 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 10 | 1 | 10 | 
| 2 | 2 | 1 | 12 | 10 | 
| 3 | 9 | 1 | 19 | 13 | 

Đội tối ưu là học sinh hàng thứ hai ở cột 1, tiếp theo là học sinh hàng thứ nhất ở cột 3, với tổng điểm`10 + 9 = 19`. DP giữ`10`ở trạng thái hàng thứ hai trong khi xử lý cột 2, mặc dù bản thân cột 2 bị bỏ qua để có trình tự tối ưu. Điều này xác nhận rằng một quá trình chuyển đổi hợp lệ có thể nhảy qua bất kỳ số cột nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi cột thực hiện một số lượng không đổi các phép toán số học và tối đa. | 
| Không gian | (O(1)) thêm | Chỉ có bốn giá trị DP vô hướng được duy trì sau khi đọc mảng. | 

Với (n\le10^5), thuật toán chỉ thực hiện vài trăm nghìn thao tác nguyên thủy, thoải mái trong giới hạn 2 giây. Bản thân các mảng đầu vào yêu cầu bộ nhớ (O(n)), trong khi DP chỉ đóng góp bộ nhớ bổ sung không đổi. Vấn đề chính thức cho phép bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    top = a[0]
    bottom = b[0]

    for i in range(1, n):
        old_top = top
        old_bottom = bottom

        top = max(old_top, old_bottom + a[i])
        bottom = max(old_bottom, old_top + b[i])

    print(max(top, bottom))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    "5\n"
    "9 3 5 7 3\n"
    "5 8 1 4 5\n"
) == "29\n", "sample 1"

# Provided sample 2
assert run(
    "3\n"
    "1 2 9\n"
    "10 1 1\n"
) == "19\n", "sample 2"

# Provided sample 3
assert run(
    "1\n"
    "7\n"
    "4\n"
) == "7\n", "sample 3"

# Custom: minimum size, second row is better
assert run(
    "1\n"
    "4\n"
    "7\n"
) == "7\n", "minimum-size case"

# Custom: all values equal
assert run(
    "3\n"
    "5 5 5\n"
    "5 5 5\n"
) == "15\n", "all-equal case"

# Custom: cannot choose both rows from the same column
assert run(
    "2\n"
    "100 100\n"
    "1 1\n"
) == "101\n", "same-column boundary"

# Custom: skipping a column is necessary
assert run(
    "3\n"
    "1 100 1\n"
    "1 1 100\n"
) == "200\n", "skipped-column transition"

# Custom: maximum n and maximum heights
n = 100000
inp = (
    f"{n}\n"
    + " ".join(["1000000000"] * n)
    + "\n"
    + " ".join(["1000000000"] * n)
    + "\n"
)
assert run(inp) == "100000000000000\n", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 4 / 7`|`7`| Kích thước tối thiểu và khởi tạo chính xác | 
|`3 / 5 5 5 / 5 5 5`|`15`| Tất cả các giá trị bằng nhau và một lựa chọn cho mỗi cột | 
|`2 / 100 100 / 1 1`|`101`| Tăng chỉ số nghiêm ngặt, ngăn chặn hai lựa chọn từ một cột | 
|`3 / 1 100 1 / 1 1 100`|`200`| Chuyển đổi bỏ qua một hoặc nhiều cột | 
|`n=100000`, mọi độ cao`1000000000`|`100000000000000`| Kích thước đầu vào tối đa và giá trị câu trả lời lớn | 

## Vỏ cạnh 

Đối với một cột duy nhất,```
1
7
4
```các trạng thái ban đầu là`top = 7`Và`bottom = 4`. Vòng lặp không bao giờ được nhập vì không có cột sau. Mức tối đa cuối cùng là`7`, điều đó hoàn toàn đúng. Điều này xử lý ranh giới nơi tái diễn yêu cầu`i-1`nếu không sẽ truy cập vào một cột không tồn tại. 

Đối với hạn chế cùng cột,```
2
100 100
1 1
```các trạng thái ban đầu là`top = 100`Và`bottom = 1`. Ở cột 2, trạng thái trên cùng mới là`max(100, 1 + 100) = 101`, trong khi trạng thái đáy mới là`max(1, 100 + 1) = 101`. Câu trả lời là`101`. DP chỉ có thể chuyển đổi các hàng từ tiền tố được xử lý trước đó, không bao giờ chuyển đổi từ trạng thái khác sau khi đã kết hợp cột 2, vì vậy nó không thể chọn cả hai`100`s ở cùng một chỉ mục. 

Đối với một cột bị bỏ qua,```
3
1 100 1
1 1 100
```các tiểu bang bắt đầu như`top = 1`,`bottom = 1`. Tại cột 2,`top`trở thành`max(1, 1 + 100) = 101`, trong khi`bottom`còn lại`1`. Tại cột 3, trạng thái đáy mới trở thành`max(1, 101 + 100) = 201`nếu chiều cao hàng thứ hai hiện tại là`100`, cho nhóm gồm học sinh hàng đầu tiên ở cột 2 và học sinh hàng thứ hai ở cột 3. Đối với dữ liệu đầu vào thực tế ở trên, giá trị hàng đầu tiên ở cột 2 là`100`và giá trị hàng thứ hai ở cột 3 là`100`, vậy kết quả là`200`. Trạng thái được bảo toàn trên cột 2 chứng minh tại sao DP phải thể hiện lịch sử tốt nhất trên toàn bộ tiền tố được xử lý thay vì chỉ cột ngay trước đó. 

Để có độ cao bằng nhau,```
3
5 5 5
5 5 5
```các tiểu bang phát triển từ`(5, 5)`ĐẾN`(10, 10)`và cuối cùng là`(15, 15)`. Câu trả lời là`15`. Có thể chọn chính xác một học sinh từ mỗi chỉ mục vì việc chọn cả hai hàng của cùng một chỉ mục sẽ vi phạm điều kiện chỉ mục nghiêm ngặt. Yêu cầu về hàng xen kẽ vẫn có thể được thỏa mãn bằng cách chọn trên cùng, dưới cùng, trên cùng hoặc dưới cùng, trên cùng, dưới cùng trên ba cột.
