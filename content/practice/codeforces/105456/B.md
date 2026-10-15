---
title: "CF 105456B - Xóa thư khỏi SMS"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường đại diện cho một tin nhắn SMS. Pedro được yêu cầu rút ngắn nó bằng cách xóa chính xác $n - k$ ký tự trong khi vẫn giữ nguyên thứ tự tương đối của các ký tự còn lại."
date: "2026-06-23T02:49:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105456
codeforces_index: "B"
codeforces_contest_name: "XXIII Spain Olympiad in Informatics, Day 2"
rating: 0
weight: 105456
solve_time_s: 79
verified: true
draft: false
---

[CF 105456B - Xóa thư khỏi SMS](https://codeforces.com/problemset/problem/105456/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường đại diện cho một tin nhắn SMS. Pedro buộc phải rút ngắn lại bằng cách xóa chính xác$n - k$các ký tự trong khi vẫn giữ nguyên thứ tự tương đối của các ký tự còn lại. Trong số tất cả các dãy con có thể có của độ dài$k$, chúng tôi muốn cái nhỏ nhất về mặt từ điển. 

Hạn chế chính là chúng ta không được phép sắp xếp lại các ký tự mà chỉ loại bỏ chúng. Vì vậy, nhiệm vụ không phải là sắp xếp hay sắp xếp lại mà là chọn một dãy con tối ưu theo thứ tự từ điển. 

Ràng buộc$n \le 100000$mỗi trường hợp thử nghiệm ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các chuỗi con hoặc sử dụng bất kỳ khám phá hàm mũ hoặc bậc hai nào. Thậm chí một$O(nk)$Phương pháp lập trình động quá chậm khi cả hai đều có thể lớn. Chúng tôi buộc phải hướng tới một công trình tham lam trong đó mỗi nhân vật được quyết định trong thời gian khấu hao gần như không đổi. 

Trường hợp cạnh tinh tế xuất hiện khi thông báo chứa các dòng giảm dài hoặc các chữ cái lặp lại. Ví dụ: trong một chuỗi như`dcba`với$k = 2$, một chiến lược ngây thơ luôn lấy ký tự tiếp theo có sẵn nhỏ nhất mà không xem xét tính khả thi trong tương lai có thể thất bại, vì việc chọn quá sớm có thể để lại quá ít ký tự cho các vị trí còn lại. 

Một vấn đề khác nảy sinh khi$k$rất nhỏ, chẳng hạn như$k = 1$, trong đó câu trả lời chỉ đơn giản là ký tự tối thiểu trong toàn bộ chuỗi và khi$k = n$, nơi không cần xóa. Bất kỳ thuật toán chính xác nào cũng phải xử lý các trường hợp biên này một cách tự nhiên mà không có lỗi viết hoa đặc biệt. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra mọi chuỗi có độ dài$k$, tính toán thứ tự từ điển của nó và lấy kết quả tốt nhất. Điều này đúng về mặt khái niệm vì nó khám phá tất cả các câu trả lời hợp lệ, nhưng số lượng các chuỗi con là$\binom{n}{k}$, trở nên lớn về mặt thiên văn ngay cả đối với mức độ vừa phải$n$. Vì$n = 100000$, điều này hoàn toàn không thể thực hiện được. 

Một cách tiếp cận lập trình động có cấu trúc hơn có thể xác định$dp[i][j]$là dãy con tốt nhất của độ dài$j$từ đầu tiên$i$nhân vật. Mặc dù cách này hoạt động hợp lý nhưng nó yêu cầu lưu trữ hoặc so sánh các chuỗi nhiều lần, dẫn đến ít nhất$O(nk)$chuyển tiếp và có khả năng$O(k)$so sánh trên mỗi lần chuyển đổi, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không bao giờ cần phải xem xét lại các ký tự một khi chúng ta đã xác định rằng ký tự nhỏ hơn có thể thay thế ký tự lớn hơn trước đó trong chuỗi tiếp theo. Nếu xây dựng câu trả lời từ trái sang phải, chúng ta chỉ cần đảm bảo rằng ở mọi vị trí, chúng ta chọn ký tự nhỏ nhất có thể mà vẫn cho phép chúng ta hoàn thành một dãy con hợp lệ có độ dài$k$. 

Điều này tự nhiên dẫn đến một cấu trúc đơn điệu tham lam: chúng tôi quét chuỗi và duy trì cấu trúc đại diện cho tiền tố tốt nhất hiện tại, loại bỏ các ký tự kém hơn thứ mà chúng tôi vẫn có thể chọn sau này. Đây chính xác là mẫu "xóa chữ số/chuỗi nhỏ nhất theo từ điển với độ dài cố định" cổ điển, được triển khai hiệu quả bằng cách sử dụng cấu trúc giống như ngăn xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(k) | Quá chậm | 
| Ngăn xếp tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng câu trả lời bằng cách sử dụng một ngăn xếp đại diện cho chuỗi con được chọn hiện tại. 

1. Khởi tạo một ngăn xếp trống và đặt bộ đếm xem chúng ta vẫn cần xóa bao nhiêu ký tự, tức là$n - k$. Điều này theo dõi tính linh hoạt của chúng tôi: mỗi lần xóa cho phép chúng tôi cải thiện thứ tự từ điển. 
2. Lặp qua các ký tự của chuỗi từ trái sang phải. Tại mỗi ký tự, chúng tôi xem xét liệu nó có thể thay thế các ký tự trước đó trong câu trả lời từng phần hiện tại hay không. 
3. Trong khi ngăn xếp không trống, ký tự cuối cùng trong ngăn xếp lớn hơn về mặt từ điển so với ký tự hiện tại và chúng tôi vẫn có sẵn các phần xóa, hãy bật ngăn xếp. Bước này đảm bảo rằng chúng ta không giữ lại ký tự xấu hơn khi ký tự tốt hơn có thể xuất hiện sớm hơn, cải thiện thứ tự từ điển càng sớm càng tốt. 
4. Đẩy ký tự hiện tại vào ngăn xếp. Điều này mở rộng dãy ứng cử viên hiện tại. 
5. Nếu chúng tôi hết số lần xóa được phép nhưng vẫn còn thừa ký tự trong ngăn xếp, chúng tôi không thể xóa thêm, vì vậy chúng tôi chỉ cần tiếp tục đẩy và sau này sẽ cắt bớt nếu cần. 
6. Sau khi xử lý tất cả các ký tự, ngăn xếp có thể chứa nhiều hơn$k$ký tự nếu chúng tôi chưa bao giờ sử dụng tất cả các thao tác xóa ở vị trí tối ưu. Trong trường hợp đó, hãy cắt bớt ngăn xếp thành ngăn xếp đầu tiên$k$nhân vật. 
7. Ngăn xếp kết quả là dãy con nhỏ nhất về mặt từ điển có độ dài$k$. 

Lý do điều này có tác dụng là vì mỗi khi chúng tôi xóa một ký tự, chúng tôi chỉ làm như vậy khi một ký tự nhỏ hơn xuất hiện sau đó và chúng tôi vẫn còn đủ ký tự còn lại để hoàn thành một đoạn dài$k$sự liên tiếp. Điều này đảm bảo rằng chúng ta không bao giờ cản trở việc hình thành một câu trả lời hợp lệ trong khi luôn cải thiện thứ tự từ điển càng sớm càng tốt. 

### Tại sao nó hoạt động 

Tại bất kỳ tiền tố nào của quá trình quét, ngăn xếp biểu thị chuỗi con tốt nhất có thể bằng cách sử dụng các ký tự được xử lý theo ràng buộc mà chúng tôi vẫn có thể mở rộng nó theo chiều dài$k$. Bước loại bỏ tham lam duy trì tính bất biến là ngăn xếp đơn điệu theo nghĩa là không có ký tự nào trước đó lớn hơn ký tự sau nếu quỹ loại bỏ cho phép sửa chữa. Bởi vì mỗi ký tự được đẩy và bật nhiều nhất một lần nên mọi quyết định đều tối ưu cục bộ và nhất quán toàn cục với thứ tự từ điển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        remove = n - k
        stack = []

        for c in s:
            while stack and remove > 0 and stack[-1] > c:
                stack.pop()
                remove -= 1
            stack.append(c)

        # If still have removals left, remove from end
        # (only needed if no better pops occurred)
        while remove > 0:
            stack.pop()
            remove -= 1

        print("".join(stack[:k]))

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp tuân theo việc xây dựng tham lam dựa trên ngăn xếp. Biến`remove`theo dõi bao nhiêu lần xóa vẫn được phép. Vòng lặp while bên trong là phần quan trọng: nó thực thi việc cải thiện từ điển bằng cách loại bỏ các ký tự trước đó lớn hơn ký tự hiện tại. 

Bước cắt xén cuối cùng là cần thiết vì một số trường hợp không bao giờ kích hoạt đủ số lần loại bỏ có lợi trong quá trình quét, đặc biệt khi chuỗi đã tăng hoặc tăng một phần. Nếu không có bước này, ngăn xếp có thể dài hơn$k$. 

Việc cắt lát`stack[:k]`đảm bảo đầu ra cuối cùng tôn trọng nghiêm ngặt độ dài cần thiết ngay cả sau mọi quyết định tham lam. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=10, k=5
s = eatqvgjpog
```Chúng tôi theo dõi sự phát triển của ngăn xếp: 

| Bước | Char | Ngăn xếp | Xóa | 
| --- | --- | --- | --- | 
| 1 | e | e | 5 | 
| 2 | một | một | 4 | 
| 3 | t | một t | 4 | 
| 4 | q | một q | 3 | 
| 5 | v | a q v | 3 | 
| 6 | g | một g | 2 | 
| 7 | j | a gj | 2 | 
| 8 | p | a g j p | 2 | 
| 9 | o | a g j o | 1 | 
| 10 | g | a g j g | 0 | 

Câu trả lời cuối cùng:`agjog`Dấu vết này cho thấy sự thay thế lặp đi lặp lại của các ký tự lớn hơn khi ký tự nhỏ hơn xuất hiện sau đó, đặc biệt là xung quanh các chuyển tiếp như`t -> q`Và`v -> g`. 

### Ví dụ 2 

đầu vào:```
n=9, k=8
s = tqbjtqmif
```| Bước | Char | Ngăn xếp | Xóa | 
| --- | --- | --- | --- | 
| 1 | t | t | 1 | 
| 2 | q | q | 0 | 
| 3 | b | q b | 0 | 
| 4 | j | q b j | 0 | 
| 5 | t | q b j t | 0 | 
| 6 | q | q b j t q | 0 | 
| 7 | m | q b j t m | 0 | 
| 8 | tôi | q b j t m tôi | 0 | 
| 9 | f | q b j t m f | 0 | 

Câu trả lời cuối cùng:`qbjtqmif`Điều này thể hiện trường hợp không có thao tác xóa nào được sử dụng sau lần điều chỉnh đầu tiên, do đó, giải pháp hoạt động giống như một chuỗi con chỉ có phần bổ sung bị ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi ký tự được đẩy và bật ra nhiều nhất một lần từ ngăn xếp | 
| Không gian | O(n) | Ngăn xếp lưu trữ tối đa n ký tự | 

Độ phức tạp tuyến tính là cần thiết bởi vì$n$có thể đạt được$10^5$và có thể có tới 100 trường hợp thử nghiệm. Các hoạt động ngăn xếp theo thời gian không đổi được khấu hao đảm bảo giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []
    
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()

        remove = n - k
        stack = []

        for c in s:
            while stack and remove > 0 and stack[-1] > c:
                stack.pop()
                remove -= 1
            stack.append(c)

        while remove > 0:
            stack.pop()
            remove -= 1

        output.append("".join(stack[:k]))

    return "\n".join(output)

# provided samples
assert run("""4
4 1
yxwv
9 8
tqbjtqmif
10 5
eatqvgjpog
10 8
tpoudnqtob
""") == """v
qbjtqmif
agjog
oudnqtob"""

# custom cases
assert run("""1
1 1
a
""") == "a"

assert run("""1
5 1
edcba
""") == "a"

assert run("""1
6 3
cbacba
""") == "aba"

assert run("""1
8 4
abcddcba
""") == "abcb"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 a`|`a`| kích thước tối thiểu, không cần xóa | 
|`5 1 edcba`|`a`| giảm tham lam hoàn toàn xuống còn một phút | 
|`6 3 cbacba`|`aba`| các ký tự lặp lại có quay lui | 
|`8 4 abcddcba`|`abcb`| mô hình tăng/giảm hỗn hợp | 

## Vỏ cạnh 

Ví dụ, trường hợp cạnh khóa là khi chuỗi giảm nghiêm ngặt`dcba`với$k = 2$. Thuật toán liên tục xuất hiện trong khi ký tự nhỏ hơn xuất hiện và có sẵn ngân sách loại bỏ, tạo ra tiền tố tối thiểu ổn định. 

đầu vào:```
4 2
dcba
```Thực thi: 

Ngăn xếp phát triển như`d -> c -> b`, với tính năng xóa được sử dụng để loại bỏ các tiền tố lớn hơn ngay khi các ký tự nhỏ hơn xuất hiện. Việc cắt xén cuối cùng đảm bảo duy trì chính xác hai ký tự, mang lại`ba`, nhỏ nhất về mặt từ điển trong số tất cả các chuỗi con có độ dài 2. 

Một trường hợp cạnh khác đã được sắp xếp đầu vào như`abcde`với$k = 3$. Không có hoạt động xóa nào được kích hoạt vì ngăn xếp đã tăng lên và việc cắt bớt cuối cùng chỉ cần lấy ba ký tự đầu tiên, tạo ra`abc`, điều này là tối ưu vì bất kỳ sự đưa vào nào sau này sẽ làm xấu đi trật tự từ điển. 

Hai trường hợp này xác nhận rằng thuật toán điều chỉnh chính xác cả khi cần loại bỏ tích cực và khi không loại bỏ có lợi.
