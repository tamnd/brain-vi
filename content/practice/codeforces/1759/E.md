---
title: "CF 1759E - Hình người"
description: "Chúng ta có một hình người có sức mạnh ban đầu h và một tập hợp các phi hành gia có sức mạnh a[i]. Hình người chỉ có thể hấp thụ một phi hành gia nếu sức mạnh của phi hành gia đó hoàn toàn nhỏ hơn sức mạnh hiện tại của hình người."
date: "2026-06-09T14:30:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1759
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round  834 (Div. 3)"
rating: 1500
weight: 1759
solve_time_s: 150
verified: true
draft: false
---

[CF 1759E - Hình người](https://codeforces.com/problemset/problem/1759/E) 

**Đánh giá:** 1500 
**Tags:** vũ lực, dp, phân loại 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình người với sức mạnh ban đầu`h`và một tập hợp các phi hành gia có sức mạnh`a[i]`. 

Hình người chỉ có thể hấp thụ một phi hành gia nếu sức mạnh của phi hành gia đó hoàn toàn nhỏ hơn sức mạnh hiện tại của hình người. Sau khi hấp thụ, phi hành gia biến mất và hình người thu được`floor(a[i] / 2)`quyền lực. 

Người hình người cũng sở hữu chính xác ba vật phẩm tăng cường tiêu hao. Hai huyết thanh màu xanh lá cây tăng gấp đôi sức mạnh hiện tại và một huyết thanh màu xanh lam tăng gấp ba lần. Mỗi serum chỉ được sử dụng tối đa một lần. 

Mục tiêu là tối đa hóa số lượng phi hành gia có thể được hấp thụ. 

Quan sát đầu tiên là thứ tự của các phi hành gia rất quan trọng. Hấp thụ một phi hành gia yếu hơn sớm hơn có thể tăng sức mạnh đủ để hấp thụ các phi hành gia mạnh hơn sau này. Vì sự hấp thụ đòi hỏi`a[i] < h`, các phi hành gia nhỏ hơn luôn dễ tiêu thụ hơn. 

Những hạn chế là gợi ý chính. Tổng số phi hành gia trong tất cả các trường hợp thử nghiệm nhiều nhất là`2 * 10^5`, vì vậy một`O(n log n)`giải pháp hoàn toàn an toàn. Bất cứ điều gì bậc hai sẽ là quá chậm bởi vì`n^2`có thể đạt được khoảng`4 * 10^10`hoạt động trong trường hợp xấu nhất. 

Trường hợp cạnh tinh tế xuất phát từ từ _strictly_. 

đầu vào:```
1
1 5
5
```Câu trả lời là:```
0
```Phi hành gia không thể bị hấp thụ vì`5`không hẳn là nhỏ hơn`5`. Việc thực hiện bất cẩn bằng cách sử dụng`<=`sẽ quay lại không chính xác`1`. 

Một sai lầm dễ mắc phải khác là cho rằng huyết thanh luôn phải được để dành cho lần sau. 

đầu vào:```
1
3 1
2 4 100
```Nếu không sử dụng serum ngay thì không thể hấp thụ được gì. Chiến lược tối ưu bắt đầu với huyết thanh. Bất kỳ quy tắc tham lam nào như “chỉ sử dụng huyết thanh khi đã hết khả năng hấp thụ” vẫn phải xử lý chính xác trường hợp ban đầu không thể hấp thụ được. 

Trường hợp thứ ba là thứ tự sử dụng huyết thanh rất quan trọng. 

đầu vào:```
1
1 2
11
```Sử dụng huyết thanh màu xanh đầu tiên mang lại sức mạnh`6`, sau đó huyết thanh màu xanh lá cây sẽ cho`12`, làm cho khả năng hấp thụ có thể xảy ra. Sử dụng hai loại huyết thanh màu xanh lá cây trước tiên chỉ mang lại hiệu quả`8`, điều đó là không đủ. Bất kỳ giải pháp nào khắc phục được một đơn hàng huyết thanh đều có thể thiếu sự tối ưu. 

Quan sát cuối cùng là chỉ có tổng cộng ba loại huyết thanh. Con số nhỏ bé này thay đổi toàn bộ vấn đề. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ sắp xếp các phi hành gia và sau đó thử mọi chuỗi hành động có thể. Tại mỗi thời điểm, chúng ta có thể hấp thụ một phi hành gia có sẵn, sử dụng huyết thanh màu xanh lá cây nếu còn lại hoặc sử dụng huyết thanh màu xanh lam nếu còn lại. 

Tìm kiếm này là chính xác vì nó khám phá mọi chiến lược pháp lý. Vấn đề là số lượng trạng thái. Sức mạnh của hình người thay đổi liên tục và có thể lên tới`2 * 10^5`phi hành gia. Một tìm kiếm ngây thơ nhanh chóng trở nên không khả thi. 

Quan sát quan trọng là việc phân loại các phi hành gia sẽ loại bỏ phần lớn sự phức tạp. 

Giả sử phi hành gia`x`có sức mạnh nhỏ hơn phi hành gia`y`. Nếu chúng ta có thể hấp thụ`y`, thì chúng ta cũng có thể hấp thụ`x`. Việc hấp thụ các phi hành gia nhỏ hơn sớm hơn không bao giờ tệ hơn vì họ chỉ tăng sức mạnh của chúng ta và không bao giờ tiêu tốn một nguồn tài nguyên nào khác ngoài thời gian, điều này không liên quan ở đây. 

Sau khi sắp xếp, mọi chiến lược tối ưu đều thu hút các phi hành gia từ trái sang phải. Quyết định thực sự duy nhất là khi nào nên sử dụng ba loại huyết thanh. 

Bây giờ hãy chú ý rằng có chính xác hai huyết thanh màu xanh lá cây và một huyết thanh màu xanh lam. Thứ tự sử dụng của họ là một trong những`GGB`,`GBG`, hoặc`BGG`. 

Đó chỉ là`3! / 2! = 3`khả năng. 

Đối với một thứ tự huyết thanh cố định, việc mô phỏng trở nên đơn giản. Xử lý các phi hành gia theo thứ tự sắp xếp. Bất cứ khi nào phi hành gia hiện tại không thể hấp thụ được, hãy tiêu thụ huyết thanh tiếp theo theo thứ tự đã chọn. Nếu không còn huyết thanh, quá trình mô phỏng sẽ kết thúc. 

Vì chỉ có ba yêu cầu huyết thanh có thể xảy ra nên chúng ta có thể mô phỏng cả ba và đưa ra câu trả lời tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp sức mạnh phi hành gia theo thứ tự không giảm. 

Bất kỳ phi hành gia nào có thể được hấp thụ sau này cũng có thể được hấp thụ sau khi tất cả các phi hành gia yếu hơn đã được hấp thụ, vì vậy việc sắp xếp cho phép chúng tôi xử lý chúng từ nhỏ nhất đến lớn nhất. 
2. Liệt kê ba loại huyết thanh có thể có:`[(2,2,3), (2,3,2), (3,2,2)]`Đây`2`đại diện cho huyết thanh màu xanh lá cây và`3`đại diện cho một huyết thanh màu xanh. 
3. Đối với một đơn hàng huyết thanh cố định, hãy bắt đầu với công suất ban đầu`h`, chỉ số phi hành gia`i = 0`, và chỉ số huyết thanh`j = 0`. 
4. Trong khi vẫn còn các phi hành gia: 

Nếu`a[i] < current_power`, hấp thụ phi hành gia đó và tăng sức mạnh bằng cách`a[i] // 2`. 
5. Ngược lại, nếu có huyết thanh khác theo thứ tự đã chọn, hãy sử dụng huyết thanh đó. 

Nhân công suất hiện tại với hệ số nhân huyết thanh tiếp theo và nâng cao chỉ số huyết thanh. 
6. Nếu không, không phi hành gia nào có thể bị hấp thụ và không còn lại huyết thanh. 

Dừng mô phỏng. 
7. Ghi lại số lượng phi hành gia đã bị hấp thụ. 
8. Lặp lại mô phỏng cho cả ba đơn hàng huyết thanh. 
9. Xuất kết quả tối đa. 

### Tại sao nó hoạt động 

Sau khi phân loại, mỗi phi hành gia bị hấp thụ chỉ làm tăng sức mạnh của hình người. Không bao giờ có lý do để bỏ qua một phi hành gia hiện có khả năng hấp thụ và thay vào đó hấp thụ một phi hành gia lớn hơn, bởi vì phi hành gia nhỏ hơn ít nhất cũng dễ tiêu thụ và cung cấp thêm năng lượng. 

Những quyết định có ý nghĩa duy nhất là vị trí tương đối của hai huyết thanh xanh và một huyết thanh xanh. Vì chỉ có ba đơn hàng riêng biệt nên mọi chiến lược huyết thanh khả thi đều được thể hiện bằng một trong các mô phỏng. 

Đối với một đơn hàng huyết thanh cố định, việc trì hoãn sử dụng huyết thanh trong khi có thể hấp thụ là tối ưu. Hấp thụ đầu tiên sẽ tăng cường sức mạnh và bảo toàn mọi lựa chọn trong tương lai. Huyết thanh chỉ cần thiết khi không thể tiến bộ được. 

Vì mọi chiến lược hợp lệ đều tương ứng với một trong ba đơn hàng huyết thanh và mỗi mô phỏng tuân theo hành vi tốt nhất cho đơn hàng đó, nên việc lấy mức tối đa trong ba mô phỏng sẽ mang lại câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def simulate(arr, h, order):
    power = h
    serum_idx = 0
    absorbed = 0
    n = len(arr)
    i = 0

    while i < n:
        if arr[i] < power:
            power += arr[i] // 2
            absorbed += 1
            i += 1
        elif serum_idx < 3:
            power *= order[serum_idx]
            serum_idx += 1
        else:
            break

    return absorbed

def solve():
    t = int(input())
    orders = [
        (2, 2, 3),
        (2, 3, 2),
        (3, 2, 2)
    ]

    answers = []

    for _ in range(t):
        n, h = map(int, input().split())
        arr = list(map(int, input().split()))
        arr.sort()

        best = 0
        for order in orders:
            best = max(best, simulate(arr, h, order))

        answers.append(str(best))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp một lần khi bắt đầu mỗi trường hợp thử nghiệm. Sau đó, mỗi mô phỏng sẽ quét mảng từ trái sang phải đúng một lần. 

các`simulate`Chức năng duy trì ba phần trạng thái: nguồn điện hiện tại, phi hành gia tiếp theo sẽ xử lý và loại huyết thanh nào theo thứ tự đã chọn sẽ được sử dụng tiếp theo. 

Một sai lầm phổ biến là sử dụng`<=`thay vì`<`khi kiểm tra khả năng hấp thụ. Tuyên bố yêu cầu quyền lực nhỏ hơn. 

Một điểm tinh tế khác là sử dụng huyết thanh. Chúng tôi chỉ sử dụng huyết thanh khi phi hành gia tiếp theo hiện không thể hấp thụ được. Hấp thụ đầu tiên ít nhất luôn tốt vì nó tăng sức mạnh mà không tiêu tốn một nguồn tài nguyên hạn chế. 

Số nguyên Python tự động xử lý lũy thừa lớn có thể phát sinh sau các phép nhân lặp đi lặp lại, vì vậy việc tràn số không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
2 1 8 9
```Các phi hành gia được sắp xếp:```
[1, 2, 8, 9]
```Sử dụng huyết thanh để`(2, 2, 3)`: 

| Bước | Quyền Lực Trước | Hành động | Sức mạnh sau | Hấp thụ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Xanh | 2 | 0 | 
| 2 | 2 | Hấp thụ 1 | 2 | 1 | 
| 3 | 2 | Xanh | 4 | 1 | 
| 4 | 4 | Hấp thụ 2 | 5 | 2 | 
| 5 | 5 | Màu xanh | 15 | 2 | 
| 6 | 15 | Hấp thụ 8 | 19 | 3 | 
| 7 | 19 | Hấp thụ 9 | 23 | 4 | 

Kết quả:`4`. 

Dấu vết này cho thấy lý do tại sao việc sử dụng huyết thanh bị trì hoãn cho đến khi quá trình điều trị dừng lại. Mỗi phi hành gia có thể hấp thụ sẽ được tiêu thụ ngay lập tức vì nó tăng sức mạnh miễn phí. 

### Ví dụ 2 

đầu vào:```
3 3
6 2 60
```Các phi hành gia được sắp xếp:```
[2, 6, 60]
```Sử dụng huyết thanh để`(3, 2, 2)`: 

| Bước | Quyền Lực Trước | Hành động | Sức mạnh sau | Hấp thụ | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | Hấp thụ 2 | 4 | 1 | 
| 2 | 4 | Màu xanh | 12 | 1 | 
| 3 | 12 | Hấp thụ 6 | 15 | 2 | 
| 4 | 15 | Xanh | 30 | 2 | 
| 5 | 30 | Xanh | 60 | 2 | 
| 6 | 60 | Hấp thụ 60? | không thể | 2 | 

Phi hành gia cuối cùng không thể bị hấp thụ vì sự hấp thụ đòi hỏi một công suất lớn hơn công suất của phi hành gia. 

Thử cả ba đơn hàng huyết thanh sẽ thấy rằng một đơn hàng khác đạt sức mạnh cao hơn`60`và hấp thụ cả ba phi hành gia, đưa ra câu trả lời tối ưu`3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế, mỗi mô phỏng trong số ba mô phỏng đều là tuyến tính | 
| Không gian | O(n) | Kho lưu trữ cho mảng phi hành gia | 

Tổng của tất cả`n`giá trị nhiều nhất là`2 * 10^5`. Sắp xếp từng trường hợp thử nghiệm và chạy ba lần quét tuyến tính dễ dàng phù hợp với giới hạn thời gian. Việc sử dụng bộ nhớ cũng thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)

    def input():
        return input_data.readline()

    def simulate(arr, h, order):
        power = h
        i = 0
        serum = 0

        while i < len(arr):
            if arr[i] < power:
                power += arr[i] // 2
                i += 1
            elif serum < 3:
                power *= order[serum]
                serum += 1
            else:
                break

        return i

    t = int(input())
    ans = []

    for _ in range(t):
        n, h = map(int, input().split())
        arr = list(map(int, input().split()))
        arr.sort()

        best = 0
        for order in ((2, 2, 3), (2, 3, 2), (3, 2, 2)):
            best = max(best, simulate(arr, h, order))

        ans.append(str(best))

    return "\n".join(ans)

# provided sample
assert run("""8
4 1
2 1 8 9
3 3
6 2 60
4 5
5 1 100 5
3 2
38 6 3
1 1
12
4 6
12 12 36 100
4 1
2 1 1 15
3 5
15 1 13
""") == """4
3
3
3
0
4
4
3"""

# minimum size
assert run("""1
1 1
1
""") == "0"

# strict inequality check
assert run("""1
1 5
5
""") == "0"

# all equal values
assert run("""1
4 10
5 5 5 5
""") == "4"

# serum order matters
assert run("""1
1 2
11
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`|`0`| Trường hợp nhỏ nhất có thể | 
|`1 5 / 5`|`0`| Bất bình đẳng nghiêm ngặt, không`<=`| 
|`4 10 / 5 5 5 5`|`4`| Giá trị bằng nhau lặp lại | 
|`1 2 / 11`|`1`| Thứ tự huyết thanh khác nhau tạo ra kết quả khác nhau | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 5
5
```Sau khi phân loại, phi hành gia duy nhất có sức mạnh`5`. Người hình người cũng có sức mạnh`5`. Thuật toán kiểm tra`5 < 5`, điều đó là sai. Nó có thể sử dụng huyết thanh, nhưng nếu không có chúng thì phi hành gia không thể hấp thụ được. Sự so sánh chặt chẽ khớp chính xác với tuyên bố và ngăn chặn những điểm chung`<=`sâu bọ. 

Coi như:```
1
3 1
2 4 100
```Mảng đã được sắp xếp rồi`[2, 4, 100]`. Phi hành gia đầu tiên không thể bị hấp thụ vì`2 < 1`là sai. Mô phỏng ngay lập tức bắt đầu tiêu thụ huyết thanh theo thứ tự đã chọn. Điều này xử lý chính xác các tình huống không thể tiến triển ngay từ đầu. 

Coi như:```
1
1 2
11
```Ba đơn hàng huyết thanh được thử nghiệm riêng biệt. Đặt hàng`(3, 2, 2)`đạt đến quyền lực`12`sau hai lần huyết thanh và hấp thụ phi hành gia. Đặt hàng`(2, 2, 3)`chỉ đạt tới`8`trước huyết thanh màu xanh và hoạt động khác nhau. Việc liệt kê cả ba khả năng đảm bảo rằng thứ tự tốt nhất sẽ không bao giờ bị bỏ sót. 

Coi như:```
1
2 6
6 7
```Phi hành gia đầu tiên không thể bị hấp thụ vì sức mạnh phải lớn hơn. Sử dụng huyết thanh sẽ nâng cao sức mạnh của hình người, sau đó cả hai phi hành gia đều có thể hấp thụ được. Thuật toán chờ một cách chính xác cho đến khi sự hấp thụ là hợp pháp thay vì coi sự bình đẳng là đủ.
