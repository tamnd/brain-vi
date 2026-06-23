---
title: "CF 105321D - Bộ đôi"
description: "Ba người chơi tham gia vào một trò chơi liên minh đơn giản, trong đó có chính xác hai người trong số họ thành lập một đội và người chơi còn lại thi đấu một mình. Mỗi người chơi có một số điểm nguyên cố định."
date: "2026-06-22T17:22:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105321
codeforces_index: "D"
codeforces_contest_name: "2024 Argentinian Programming Tournament (TAP)"
rating: 0
weight: 105321
solve_time_s: 53
verified: true
draft: false
---

[CF 105321D - Duo](https://codeforces.com/problemset/problem/105321/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ba người chơi tham gia vào một trò chơi liên minh đơn giản, trong đó có chính xác hai người trong số họ thành lập một đội và người chơi còn lại thi đấu một mình. Mỗi người chơi có một số điểm nguyên cố định. Đối với bất kỳ cặp nào được chọn, điểm của bộ đôi là tổng điểm của cá nhân họ và tổng này được so sánh với điểm của người chơi đơn độc. 

Quy tắc kết quả là không đối xứng: nếu tổng của bộ đôi lớn hơn điểm của người chơi solo thì bộ đôi sẽ thắng. Ngược lại, nghĩa là tổng số điểm nhỏ hơn hoặc bằng số điểm của người chơi solo, người chơi solo sẽ thắng một mình. 

Câu hỏi đặt ra liệu có tồn tại ít nhất một người chơi có thể đảm bảo chiến thắng một mình theo quy tắc này hay không, điều này có nghĩa là tồn tại sự lựa chọn của hai người chơi còn lại sao cho điểm tổng hợp của họ không vượt quá điểm của người chơi đã chọn. 

Dữ liệu đầu vào bao gồm ba số nguyên biểu thị điểm của ba người tham gia. Đầu ra là một ký tự duy nhất cho biết liệu một số người chơi có thể giành chiến thắng một mình hay không. 

Mặc dù các ràng buộc là cực kỳ nhỏ, vấn đề này về cơ bản là về việc kiểm tra tất cả các phân vùng có thể có của ba phần tử thành một phần tử duy nhất so với một cặp. 

Không có ràng buộc hiệu suất thuật toán có ý nghĩa nào ở đây vì kích thước đầu vào không đổi. Bất kỳ giải pháp đúng nào đều chạy trong thời gian không đổi, vì vậy các lớp phức tạp vượt quá O(1) đều không liên quan. 

Các trường hợp cạnh chủ yếu là về sự bình đẳng và đối xứng. 

Trường hợp tinh tế đầu tiên là khi tất cả các giá trị đều bằng nhau. Ví dụ, đầu vào`50 50 50`dẫn đến tổng điểm của mỗi cặp là 100, lớn hơn 50 điểm còn lại, vì vậy không người chơi solo nào có thể giành chiến thắng. 

Một trường hợp khác là khi một giá trị chiếm ưu thế nhưng hầu như không. Ví dụ,`100 10 10`cho phép người chơi đạt 100 điểm giành chiến thắng một mình vì 10 + 10 = 20 không lớn hơn 100. 

Một trường hợp sai lầm đối với cách giải thích ngây thơ là nghĩ “giá trị lớn nhất luôn thắng một mình”. Điều này không phải lúc nào cũng đúng; nó chỉ hoạt động khi giá trị lớn nhất ít nhất bằng tổng của hai giá trị còn lại. 

## Phương pháp tiếp cận 

Chỉ với ba người chơi, phương pháp trực tiếp nhất là thử mọi lựa chọn có thể có của người chơi solo và tính tổng của hai người còn lại. Đối với mỗi ứng cử viên, chúng tôi kiểm tra xem tổng điểm của hai người chơi còn lại có nhỏ hơn hoặc bằng số điểm của ứng cử viên đó hay không. Nếu có bất kỳ cấu hình nào như vậy, người chơi đó có thể giành chiến thắng một mình. 

Chế độ xem bạo lực này đã tối ưu vì số lượng cấu hình được cố định ở mức ba. Đối với mỗi người chơi, chúng tôi thực hiện một lượng số học không đổi, do đó tổng công việc là không đổi. 

Quan sát quan trọng là vấn đề giảm xuống còn việc kiểm tra xem phần tử tối đa có lớn ít nhất bằng tổng của hai phần tử còn lại hay không. Nếu người chơi mạnh nhất không thể chế ngự được sức mạnh tổng hợp của hai người còn lại thì việc sắp xếp lại các liên minh không thể tạo ra chiến thắng đơn lẻ. Ngược lại, nếu bất kỳ người chơi nào thỏa mãn sự bất bình đẳng này, họ có thể được chọn là người chiến thắng solo và hai người còn lại tạo thành bộ đôi đối lập. 

Do đó, lời giải sẽ rút gọn thành việc đánh giá ba bất đẳng thức đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các lựa chọn solo) | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu (kiểm tra trực tiếp các bất đẳng thức) | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

## Hướng dẫn thuật toán 

1. Đọc ba điểm số nguyên A, B và C. Những điểm này thể hiện sức mạnh cố định của người chơi và sẽ được so sánh theo các cặp khác nhau. 
2. Tính xem A có thể thắng một mình hay không bằng cách kiểm tra xem A có lớn hơn hoặc bằng B + C hay không. Điều này trực tiếp mã hóa quy luật A đánh bại bộ đôi của hai người chơi còn lại. 
3. Tính xem B có thể thắng một mình hay không bằng cách kiểm tra xem B có lớn hơn hoặc bằng A + C hay không. Điều này kiểm tra điều kiện tương tự đối với người chơi thứ hai ở một cặp khác. 
4. Tính xem C có thể thắng một mình hay không bằng cách kiểm tra xem C có lớn hơn hoặc bằng A + B hay không. Điều này bao gồm kịch bản solo cuối cùng có thể xảy ra. 
5. Nếu có bất kỳ điều kiện nào trong ba điều kiện này, hãy xuất ra "S". Nếu không thì xuất ra "N". 

Lý do đằng sau việc kiểm tra cả ba trường hợp là mỗi người chơi phải được đánh giá độc lập với tư cách là đối thủ solo tiềm năng, vì việc ghép đôi sẽ thay đổi tổng đối phương. 

### Tại sao nó hoạt động 

Kết quả trò chơi chỉ phụ thuộc vào sự so sánh giữa một giá trị và tổng của hai giá trị còn lại. Mọi cấu hình hợp lệ đều tương ứng chính xác với một trong ba bất đẳng thức được kiểm tra. Nếu không ai trong số họ giữ vững, thì đối với mọi lựa chọn solo có thể có, tổng của bộ đôi sẽ vượt quá số điểm solo, điều này buộc bộ đôi phải thắng trong mọi trường hợp. Nếu có ít nhất một người giữ, người chơi đó được đảm bảo giành chiến thắng khi được chọn làm người tham gia solo vì không có cặp đôi thay thế nào có thể thay đổi thực tế là tổng số đối lập đã được ấn định cho phân vùng đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, b, c = map(int, input().split())

    if a >= b + c or b >= a + c or c >= a + b:
        print("S")
    else:
        print("N")

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh trực tiếp các điều kiện toán học có được trong thuật toán. Mỗi bất đẳng thức tương ứng với một cấu hình solo có thể có. Quyết định cuối cùng tổng hợp các kiểm tra này bằng OR logic. 

Không có vấn đề về ranh giới ngoài phép cộng số nguyên tiêu chuẩn, an toàn với các ràng buộc đã cho. Việc so sánh sử dụng`>=`còn hơn là`>`bởi vì sự bình đẳng có nghĩa là bộ đôi không vượt quá người chơi solo, điều này vẫn dẫn đến chiến thắng solo. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`1 2 3`| Bước | A | B | C | A vs B+C | B vs A+C | C đấu với A+B | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Kiểm tra | 1 | 2 | 3 | 1 >= 5 sai | 2 >= 4 sai | 3 >= 3 đúng | S | 

Người chơi thứ ba khớp chính xác với tổng của hai người còn lại. Vì bộ đôi phải vượt xa người chơi solo để giành chiến thắng nên sự bình đẳng có nghĩa là người chơi solo sẽ thắng. 

### Ví dụ 2:`4 5 6`| Bước | A | B | C | A vs B+C | B vs A+C | C đấu với A+B | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Kiểm tra | 4 | 5 | 6 | 4 >= 11 sai | 5 >= 10 sai | 6 >= 9 sai | N | 

Không người chơi nào có thể bằng hoặc vượt quá tổng của hai người còn lại, vì vậy mọi bộ đôi có thể sẽ thắng người chơi còn lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba so sánh cố định được thực hiện bất kể đầu vào | 
| Không gian | O(1) | Không có cấu trúc dữ liệu bổ sung nào được sử dụng | 

Bản chất thời gian không đổi đảm bảo lời giải thỏa mãn một cách tầm thường các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    a, b, c = map(int, sys.stdin.readline().split())
    print("S" if (a >= b + c or b >= a + c or c >= a + b) else "N")

# provided samples
assert run("1 2 3") == "S", "sample 1"
assert run("4 5 6") == "N", "sample 2"

# custom cases
assert run("100 10 10") == "S", "dominant player"
assert run("50 50 50") == "N", "perfect symmetry"
assert run("1 1 2") == "S", "equality edge case"
assert run("2 3 4") == "N", "no dominant player"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 100 10 10 | S | người chơi đơn chiếm ưu thế chiến thắng một mình | 
| 50 50 50 | N | đối xứng hoàn toàn ngăn cản chiến thắng solo | 
| 1 1 2 | S | trường hợp ranh giới đẳng thức | 
| 2 3 4 | N | không thỏa mãn bất đẳng thức | 

## Vỏ cạnh 

Trường hợp lợi thế quan trọng nhất là sự bình đẳng, trong đó tổng của bộ đôi khớp chính xác với người chơi solo. Đối với đầu vào`1 2 3`, điều kiện`3 >= 1 + 2`giữ, vì vậy người chơi solo sẽ thắng. Một sai lầm phổ biến là sử dụng bất đẳng thức nghiêm ngặt sai hướng, điều này sẽ bác bỏ trường hợp này một cách không chính xác. 

Một trường hợp khác là sự đối xứng hoàn toàn như`50 50 50`. Ở đây, tổng mỗi cặp là 100, vượt quá 50 còn lại, vì vậy không thể giành chiến thắng một mình. Thuật toán đánh giá chính xác cả ba bất đẳng thức là sai. 

Trường hợp thứ ba là phân phối bị sai lệch nhiều như`100 10 10`. Séc`100 >= 20`thành công ngay lập tức, xác nhận một chiến thắng solo. Điều này chứng tỏ rằng chỉ cần giữ một bất đẳng thức để có câu trả lời khẳng định.
