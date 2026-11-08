---
title: "CF 1428C - ABBB"
description: "Chúng ta được cấp một chuỗi chỉ gồm các ký tự A và B. Chúng ta có thể loại bỏ nhiều lần bất kỳ chuỗi con liền kề nào bằng AB hoặc BB. Sau khi loại bỏ một cặp như vậy, các phần còn lại của chuỗi sẽ nối với nhau, có khả năng tạo ra các cặp mới có thể tháo rời được."
date: "2026-06-11T05:28:49+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 1428
codeforces_index: "C"
codeforces_contest_name: "Codeforces Raif Round 1 (Div. 1 + Div. 2)"
rating: 1100
weight: 1428
solve_time_s: 108
verified: true
draft: false
---

[CF 1428C - ABBB](https://codeforces.com/problemset/problem/1428/C) 

**Đánh giá:** 1100 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, chuỗi 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi chỉ bao gồm các ký tự`A`Và`B`. Chúng tôi có thể liên tục loại bỏ bất kỳ chuỗi con liền kề nào bằng`AB`hoặc`BB`. Sau khi loại bỏ một cặp như vậy, các phần còn lại của chuỗi sẽ nối với nhau, có khả năng tạo ra các cặp mới có thể tháo rời được. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi cần xác định độ dài tối thiểu có thể có của chuỗi sau khi thực hiện bất kỳ số lần xóa hợp lệ nào. 

Tổng chiều dài của tất cả các chuỗi tối đa là`2 × 10^5`, đó là hạn chế thực sự quan trọng. Bất kỳ thuật toán nào liên tục quét chuỗi và xóa các ký tự về mặt vật lý đều có thể dễ dàng trở thành bậc hai. Một giải pháp bậc hai sẽ yêu cầu khoảng`4 × 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cách tiếp cận thời gian tuyến tính, chỉ xử lý mỗi ký tự một số lần không đổi. 

Một số tình huống rất dễ bị đánh giá sai. 

Hãy xem xét chuỗi:```
AB
```Câu trả lời là`0`, vì toàn bộ chuỗi có thể được xóa chỉ bằng một thao tác. 

Bây giờ hãy xem xét:```
BA
```Câu trả lời là`2`. không có`AB`hoặc`BB`chuỗi con nên không thể xóa được gì. Một giải pháp chỉ đếm số lượng`A`Và`B`các ký tự sẽ xử lý không chính xác hai chuỗi này giống nhau. 

Một ví dụ tinh tế khác là:```
ABB
```Một trình tự có thể là:```
ABB → A → 1
```Đầu tiên loại bỏ cái cuối cùng`BB`, rời đi`A`. Ngoài ra:```
ABB → B → 1
```Di dời`AB`Đầu tiên. Các thứ tự hoạt động khác nhau tạo ra các chuỗi trung gian khác nhau nhưng độ dài tối thiểu có thể đạt được là như nhau. Giải pháp không được phụ thuộc vào việc chọn một chuỗi xóa cụ thể. 

Một trường hợp khó khăn cuối cùng là:```
BBB
```Chúng ta có thể loại bỏ một`BB`đôi:```
BBB → B
```Câu trả lời là`1`, không`0`. Bất cứ khi nào một khối chứa một số lẻ`B`ký tự và không`A`có sẵn để hủy bỏ cái cuối cùng, một`B`sống sót. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ liên tục tìm kiếm sự xuất hiện của`AB`hoặc`BB`, xóa một trong số chúng, nối các phần còn lại và tiếp tục cho đến khi không còn động thái nào. 

Cách tiếp cận này đúng vì nó tuân theo các quy tắc của trò chơi theo đúng nghĩa đen. Vấn đề là hiệu quả. Mỗi lần xóa có thể yêu cầu dịch chuyển gần như toàn bộ chuỗi và có thể có tới`O(n)`việc xóa. Một chuỗi có độ dài`n`do đó có thể yêu cầu`O(n²)`công việc. 

Quan sát quan trọng là cả hai mẫu có thể tháo rời đều kết thúc bằng`B`. 

Giả sử chúng ta xử lý chuỗi từ trái sang phải và duy trì chuỗi chưa rút gọn hiện tại. Khi chúng ta đọc một ký tự mới: 

Nếu nó là`A`, nó không thể ngay lập tức tạo thành một cặp có thể tháo rời được nên tạm thời nó phải ở lại. 

Nếu nó là`B`, thì bất kỳ ký tự nào trước đó đều cho phép xóa:```
AB → removed
BB → removed
```Điều duy nhất quan trọng là liệu có tồn tại một ký tự ngay trước nó trong chuỗi rút gọn hiện tại hay không. Nếu có thì cái mới`B`loại bỏ ký tự đó. Nếu không có thì`B`sống sót. 

Hành vi này chính xác là những gì ngăn xếp cung cấp. 

Chúng tôi lưu trữ các ký tự chưa được rút gọn hiện tại trong một ngăn xếp. 

Khi đọc`A`, đẩy nó. 

Khi đọc`B`: 

Nếu ngăn xếp không trống, hãy bật một ký tự. Nhân vật xuất hiện cùng với điều này`B`tương ứng với một`AB`hoặc`BB`xóa. 

Nếu ngăn xếp trống, hãy nhấn nút`B`, vì không có gì có sẵn để loại bỏ nó. 

Sau khi xử lý toàn bộ chuỗi, ngăn xếp chứa chính xác các ký tự không thể loại bỏ được. Kích thước của nó là câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một ngăn xếp trống. 
2. Quét chuỗi từ trái sang phải. 
3. Nếu ký tự hiện tại là`A`, đẩy nó vào ngăn xếp. 

MỘT`A`một mình không thể hoàn thành mẫu có thể tháo rời. 
4. Nếu ký tự hiện tại là`B`và ngăn xếp không trống, hãy bật phần tử trên cùng. 

Nhân vật xuất hiện cùng với hiện tại`B`hình thức hoặc`AB`hoặc`BB`, cả hai đều có thể tháo rời. 
5. Nếu ký tự hiện tại là`B`và ngăn xếp trống, đẩy`B`. 

Không thể xóa vì không có ký tự trước đó. 
6. Sau khi tất cả các ký tự đã được xử lý, xuất ra kích thước ngăn xếp. 

### Tại sao nó hoạt động 

Ngăn xếp luôn biểu thị chuỗi dư ngắn nhất có thể cho tiền tố được xử lý cho đến nay. 

Khi mới`B`đến, bất kỳ nhân vật hàng đầu hiện có nào cũng có thể được ghép nối ngay với nó. Cho dù nhân vật hàng đầu đó là`A`hoặc`B`, cặp biến mất. Giữ một trong hai ký tự sẽ không bao giờ giúp tạo ra kết quả cuối cùng ngắn hơn, bởi vì cả hai đều`AB`Và`BB`là những thao tác xóa hợp lệ. 

Nếu ngăn xếp trống,`B`không có đối tác và phải ở lại. Không thao tác nào trong tương lai có thể xóa nó mà không đặt một ký tự khác trước nó, điều này là không thể vì chúng tôi xử lý các ký tự theo thứ tự ban đầu của chúng. 

Bằng cách quy nạp tiền tố đã được xử lý, ngăn xếp vẫn giữ nguyên dạng rút gọn hoàn toàn của tiền tố đó. Khi quá trình quét kết thúc, độ dài của nó là độ dài cuối cùng tối thiểu có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        s = input().strip()

        stack = []

        for ch in s:
            if ch == 'A':
                stack.append('A')
            else:
                if stack:
                    stack.pop()
                else:
                    stack.append('B')

        answers.append(str(len(stack)))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Giải pháp tuân theo mô phỏng ngăn xếp được mô tả ở trên. 

Ngăn xếp lưu trữ dạng rút gọn của tiền tố đã được xử lý cho đến nay. Mọi`A`bị đẩy vì nó không thể tham gia xóa ngay lập tức. 

Khi một`B`xuất hiện, chỉ có hai khả năng. Nếu ngăn xếp không trống, phần tử trên cùng và phần tử hiện tại`B`cùng biến mất. Điều này phù hợp với cả hai hoạt động được phép. Nếu ngăn xếp trống,`B`tồn tại và phải được lưu trữ. 

Một lỗi phổ biến là kiểm tra rõ ràng xem phần trên có phải là`A`hoặc`B`. Điều đó là không cần thiết vì cả hai`AB`Và`BB`có thể tháo rời. Điều duy nhất quan trọng là liệu một nhân vật có tồn tại hay không. 

Một lỗi dễ xảy ra khác là quên rằng một người sống sót`B`phải được đẩy khi ngăn xếp trống. Nếu không có bước đó, những trường hợp như`"B"`hoặc`"BBB"`đưa ra những câu trả lời sai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
BABA
```| Nhân vật | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
| B | [] | đẩy B | [B] | 
| A | [B] | đẩy A | [B,A] | 
| B | [B,A] | bật | [B] | 
| A | [B] | đẩy A | [B,A] | 

Kích thước ngăn xếp cuối cùng = 2. 

Dấu vết cho thấy một`AB`cặp biến mất khi ký tự thứ ba được xử lý. Chuỗi còn lại không thể giảm thêm nữa. 

### Ví dụ 2 

đầu vào:```
AABBBABBBB
```| Nhân vật | Xếp chồng trước | Hành động | Xếp chồng sau | 
| --- | --- | --- | --- | 
| A | [] | đẩy | [A] | 
| A | [A] | đẩy | [A,A] | 
| B | [A,A] | bật | [A] | 
| B | [A] | bật | [] | 
| B | [] | đẩy | [B] | 
| A | [B] | đẩy | [B,A] | 
| B | [B,A] | bật | [B] | 
| B | [B] | bật | [] | 
| B | [] | đẩy | [B] | 
| B | [B] | bật | [] | 

Kích thước ngăn xếp cuối cùng = 0. 

Ví dụ này thể hiện cả hai loại xóa. Đôi khi một`B`loại bỏ một`A`, và đôi khi nó loại bỏ một cái khác`B`. Thuật toán xử lý cả hai giống hệt nhau bằng cách bật một phần tử bất cứ khi nào có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và mỗi phần tử ngăn xếp được đẩy và xuất ra nhiều nhất một lần | 
| Không gian | O(n) | Trong trường hợp xấu nhất, ngăn xếp có thể chứa toàn bộ chuỗi | 

Đây`n`biểu thị độ dài của chuỗi trường hợp thử nghiệm. Vì tổng của tất cả các độ dài lớn nhất`2 × 10^5`, tổng thời gian chạy trên tất cả các trường hợp thử nghiệm là tuyến tính theo kích thước đầu vào và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        s = input().strip()

        stack = []

        for ch in s:
            if ch == 'A':
                stack.append('A')
            else:
                if stack:
                    stack.pop()
                else:
                    stack.append('B')

        ans.append(str(len(stack)))

    return "\n".join(ans)

# provided samples
assert run("3\nAAA\nBABA\nAABBBABBBB\n") == "3\n2\n0", "sample 1"

# minimum size
assert run("1\nA\n") == "1", "single A"

# minimum size
assert run("1\nB\n") == "1", "single B"

# all equal characters
assert run("1\nBBBBB\n") == "1", "odd number of B"

# removable pair
assert run("1\nAB\n") == "0", "entire string removed"

# order matters
assert run("1\nBA\n") == "2", "no valid move"

# off-by-one style case
assert run("1\nABB\n") == "1", "multiple possible deletions"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`A`|`1`| Đầu vào nhỏ nhất có thể chứa`A`| 
|`B`|`1`| Đầu vào nhỏ nhất có thể chứa`B`| 
|`BBBBB`|`1`| Khối có kích thước lẻ chỉ`B`nhân vật | 
|`AB`|`0`| Loại bỏ trực tiếp một cặp hợp lệ | 
|`BA`|`2`| Không tồn tại chuỗi con có thể tháo rời | 
|`ABB`|`1`| Nhiều lệnh xóa hợp lệ dẫn đến cùng một câu trả lời | 

## Vỏ cạnh 

Hãy xem xét:```
1
BA
```Sự phát triển của ngăn xếp là:```
B -> [B]
A -> [B, A]
```KHÔNG`B`đến sau trận chung kết`A`, vì vậy không có gì có thể được gỡ bỏ. Kích thước cuối cùng là`2`. Thuật toán tránh loại bỏ các ký tự không liền kề một cách chính xác. 

Coi như:```
1
BBB
```Sự phát triển của ngăn xếp là:```
B -> [B]
B -> []
B -> [B]
```Một`BB`cặp biến mất, nhưng cái cuối cùng`B`sống sót. Kích thước cuối cùng là`1`, phù hợp với kết quả tối ưu. 

Coi như:```
1
AB
```Sự phát triển của ngăn xếp là:```
A -> [A]
B -> []
```Toàn bộ chuỗi biến mất, tạo ra câu trả lời`0`. 

Coi như:```
1
ABB
```Sự phát triển của ngăn xếp là:```
A -> [A]
B -> []
B -> [B]
```Câu trả lời là`1`. Ví dụ này cho thấy tại sao thuật toán không cần phải chọn trong số các thứ tự xóa khác nhau. Biểu diễn ngăn xếp đã nắm bắt trạng thái rút gọn hoàn toàn sau mỗi tiền tố, tự động dẫn đến độ dài tối thiểu chính xác.
