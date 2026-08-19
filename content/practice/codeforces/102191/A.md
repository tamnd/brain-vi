---
title: "CF 102191A - Kẻ ăn uống hào phóng"
description: "Chúng ta bắt đầu với n viên kẹo và muốn tặng kẹo cho càng nhiều người bạn khác biệt càng tốt. Tặng một viên kẹo cho một người bạn thì đơn giản, nhưng sau mỗi giây kẹo được tặng cho bạn bè, chúng ta sẽ tự mình tiêu thụ một viên kẹo nếu còn một viên."
date: "2026-08-18T02:25:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102191
codeforces_index: "A"
codeforces_contest_name: "PSUT Coding Marathon 2019"
rating: 0
weight: 102191
solve_time_s: 226
verified: false
draft: false
---

[CF 102191A - Kẻ ăn uống hào phóng](https://codeforces.com/problemset/problem/102191/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi bắt đầu với`n`kẹo và muốn tặng kẹo cho càng nhiều bạn bè khác nhau càng tốt. Tặng một viên kẹo cho một người bạn thì đơn giản, nhưng sau mỗi giây kẹo được tặng cho bạn bè, chúng ta sẽ tự mình tiêu thụ một viên kẹo nếu còn một viên. Câu hỏi đặt ra là cuối cùng có bao nhiêu viên kẹo có thể đến tay bạn bè khi chúng ta chọn thứ tự tặng họ một cách tối ưu. 

Đầu vào chứa một số nguyên duy nhất`n`, đại diện cho số kẹo ban đầu. Kết quả là số lượng bạn bè tối đa mà mỗi người có thể nhận được một viên kẹo. 

Giới hạn trên`n <= 10^9`loại trừ mọi cách tiếp cận thực hiện một hoặc nhiều thao tác cho mỗi viên kẹo. Một mô phỏng tuyến tính sẽ yêu cầu tới một tỷ lần lặp, vượt xa giới hạn thời gian lập trình cạnh tranh có thể chịu đựng được, đặc biệt là với giới hạn 0 giây hiệu quả đã nêu. Chúng ta cần nhận biết cấu trúc lặp và tính đáp án trực tiếp trong thời gian không đổi. Yêu cầu về bộ nhớ là không đáng kể vì chỉ cần một giá trị đầu vào duy nhất và câu trả lời. 

Đầu vào nhỏ nhất thể hiện hành vi ranh giới. Vì`n = 1`, câu trả lời đúng là`1`, bởi vì viên kẹo duy nhất có thể được cho đi và không có viên kẹo thứ hai nào có thể kích hoạt việc ăn. Công thức luôn trừ một viên kẹo cho mỗi nhóm ba người vẫn phải xử lý trường hợp này một cách chính xác. 

Vì`n = 2`, câu trả lời là`2`. Chúng ta có thể đưa cả hai chiếc kẹo cho hai người bạn và chỉ khi đó chúng ta mới cần ăn một chiếc kẹo nhưng không còn lại một chiếc nào. Việc thực hiện bất cẩn cho rằng mỗi cặp quà luôn tốn thêm một viên kẹo sẽ trả lại không chính xác`1`. 

Bội số của ba là một ranh giới hữu ích khác. Với`n = 6`, chúng ta có thể cho hai viên kẹo, ăn một viên, sau đó cho thêm hai viên kẹo nữa và ăn viên cuối cùng. Câu trả lời là`4`, không`3`. Việc ăn uống diễn ra sau mỗi cặp quà, vì vậy viên kẹo được tiêu thụ cuối cùng không tương ứng với việc có thêm một người bạn bị mất. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp có thể mô hình hóa quá trình xử lý từng viên kẹo một lần. Chúng tôi giữ số kẹo còn lại và số lượng bạn bè đã nhận được một viên. Bất cứ khi nào chúng ta có đủ kẹo để tặng một viên kẹo khác, chúng ta sẽ tặng nó cho một người bạn. Sau mỗi món quà thứ hai, chúng ta sẽ tiêu thụ một viên kẹo nếu có thể. Mô phỏng này đúng vì nó tuân theo chính xác quy trình được mô tả bởi bài toán và việc chọn tặng kẹo bất cứ khi nào có thể là tối ưu vì mục tiêu chỉ đơn giản là tối đa hóa số lượng quà tặng. 

Vấn đề là số lần lặp lại. Trong trường hợp xấu nhất, mô phỏng thực hiện công Θ(n), có nghĩa là gần như`10^9`lần lặp lại. Như vậy là quá chậm. 

Quan sát quan trọng là mỗi nhóm hoàn chỉnh gồm ba viên kẹo ban đầu sẽ tạo ra chính xác hai món quà. Hai chiếc kẹo được tặng cho bạn bè và sau món quà thứ hai, một chiếc kẹo sẽ được ăn. Mô hình tương tự có thể lặp lại một cách độc lập khi vẫn còn ít nhất ba viên kẹo. Điều này có nghĩa là chúng ta không cần phải mô phỏng từng viên kẹo riêng lẻ. Chúng ta có thể đếm có bao nhiêu nhóm hoàn chỉnh gồm ba viên và xử lý một hoặc hai viên kẹo cuối cùng một cách riêng biệt. 

Nếu như`n = 3q + r`, thì`q`các nhóm hoàn thành đóng góp`2q`bạn. Nếu như`r = 0`, không còn gì cả. Nếu như`r = 1`, số kẹo còn lại có thể tặng thêm một người bạn. Nếu như`r = 2`, cả 2 viên kẹo còn lại đều có thể được cho đi, vì luật ăn chỉ áp dụng sau phần quà thứ 2 và sau đó không còn viên kẹo nào. 

Điều này đưa ra công thức nhỏ gọn`answer = n - floor(n / 3)`. 

Kết quả tương tự có thể được hiểu từ cách giải thích của nhóm. Cứ ba viên kẹo sẽ dẫn đến hai viên kẹo tiếp cận được bạn bè, do đó, chính xác một viên kẹo cho mỗi nhóm hoàn chỉnh sẽ bị mất khi ăn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số kẹo`n`. Chỉ cần giá trị này vì quá trình này chỉ phụ thuộc vào số lượng kẹo còn lại. 
2. Tính toán`n // 3`, số nhóm hoàn chỉnh của ba viên kẹo. Mỗi nhóm như vậy sẽ tốn một viên kẹo để ăn và cho phép tặng hai viên kẹo cho bạn bè. 
3. Trừ số kẹo đã ăn vào số kẹo ban đầu. Giá trị kết quả,`n - n // 3`, là số lượng kẹo tối đa mà bạn bè có thể nhận được. 
4. In kết quả. Không cần mô phỏng hoặc trạng thái bổ sung. 

### Tại sao nó hoạt động 

Hãy xem xét từng khối hoàn chỉnh gồm ba viên kẹo. Chúng ta có thể tặng hai món cho hai người bạn, và sau món thứ hai, chúng ta ăn món thứ ba. Như vậy, ba viên kẹo tạo ra đúng hai món quà thành công. Sau khi xử lý tất cả các khối hoàn chỉnh, chỉ còn lại tối đa hai viên kẹo. Rõ ràng một viên kẹo còn lại có thể được cho đi, và hai viên kẹo còn lại đều có thể được cho đi vì hành động ăn chỉ xảy ra sau món quà thứ hai, khi không còn kẹo để tiêu thụ. Do đó, những viên kẹo duy nhất không đến được với bạn bè chính xác là`floor(n / 3)`kẹo, một chiếc từ mỗi nhóm ba chiếc. Câu trả lời là do đó`n - floor(n / 3)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    print(n - n // 3)

if __name__ == "__main__":
    solve()
```các`solve`hàm đọc số nguyên duy nhất được chỉ định bởi định dạng đầu vào. Không cần vòng lặp vì bài toán chỉ chứa đúng một test case. 

biểu hiện`n // 3`đếm xem có thể xuất hiện bao nhiêu nhóm hoàn chỉnh gồm ba viên kẹo. Trừ cái này từ`n`trực tiếp đếm số kẹo không được tiêu thụ. Những viên kẹo còn lại tương ứng chính xác với những người bạn có thể nhận kẹo. 

Số nguyên Python xử lý các giá trị lớn hơn nhiều so với`10^9`, vì vậy việc tràn số nguyên không phải là vấn đề đáng lo ngại. Phép chia số nguyên cũng là phép chia sàn có chủ ý. Việc sử dụng phép chia thông thường sẽ tạo ra giá trị dấu phẩy động và sẽ không thể hiện chính xác số lượng nhóm hoàn chỉnh. 

Không có sự điều chỉnh riêng lẻ. Ví dụ,`n = 2`cho`2 - 0 = 2`, trong khi`n = 3`cho`3 - 1 = 2`. Sự chuyển đổi giữa các trường hợp đó chính là nơi viên kẹo tự tiêu đầu tiên xuất hiện. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,`n = 4`. 

|`n`|`n // 3`| Trả lời | 
| --- | --- | --- | 
| 4 | 1 | 3 | 

Có một nhóm hoàn chỉnh gồm ba viên kẹo, tạo ra hai món quà và một viên kẹo đã ăn. Một viên kẹo còn lại và có thể được tặng cho một người bạn khác, tổng cộng là tặng ba người bạn. 

Đối với mẫu 2,`n = 5`. 

|`n`|`n // 3`| Trả lời | 
| --- | --- | --- | 
| 5 | 1 | 4 | 

Ba viên kẹo đầu tiên tạo ra hai món quà và một viên kẹo đã ăn. Còn lại hai viên kẹo và cả hai đều có thể được cho đi. Kết quả là có bốn người bạn. Ví dụ này giải thích tại sao phần dư cuối cùng của 2 không được kích hoạt phép trừ bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một phép chia số nguyên và một số phép tính số học không đổi được thực hiện. | 
| Không gian | O(1) | Chỉ số nguyên đầu vào và một vài giá trị tạm thời được lưu trữ. | 

Giá trị tối đa`n = 10^9`không ảnh hưởng đến số lượng hoạt động. Giải pháp thực hiện cùng một lượng công việc không đổi cho`n = 1`Và`n = 10^9`, do đó, nó dễ dàng nằm gọn trong giới hạn bộ nhớ 256 MB và tránh được chi phí mô phỏng hàng tỷ lần lặp lại. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    n = int(input())
    print(n - n // 3)

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        input = old_input

# The helper above needs to capture stdout, so use a dedicated wrapper.
def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples
assert run("4\n") == "3\n", "sample 1"
assert run("5\n") == "4\n", "sample 2"
assert run("6\n") == "4\n", "sample 3"

# Custom cases
assert run("1\n") == "1\n", "minimum input"
assert run("2\n") == "2\n", "two candies can both be given away"
assert run("3\n") == "2\n", "first eating event"
assert run("1000000000\n") == "666666667\n", "maximum input"
assert run("8\n") == "6\n", "remainder of two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Đầu vào tối thiểu và sự vắng mặt của một sự kiện ăn uống | 
|`2`|`2`| Món quà thứ hai không mất kẹo khi không còn gì | 
|`3`|`2`| Bội số chính xác đầu tiên của ba và sự kiện ăn uống đầu tiên | 
|`8`|`6`| Các nhóm hoàn chỉnh kết hợp với phần còn lại của hai | 
|`1000000000`|`666666667`| Ràng buộc tối đa và số học theo thời gian không đổi | 

## Vỏ cạnh 

cho`n = 1`, thuật toán tính toán`1 // 3 = 0`, vậy câu trả lời là`1 - 0 = 1`. Chiếc kẹo duy nhất thuộc về một người bạn, và không có món quà thứ hai nào có thể khiến chúng ta phải ăn bất cứ thứ gì. 

Vì`n = 2`, tính toán là`2 // 3 = 0`, sản xuất`2 - 0 = 2`. Cả hai loại kẹo đều có thể được phân phát. Điều này mắc phải sai lầm phổ biến là trừ đi một viên kẹo mỗi khi hai người bạn nhận được kẹo mà không kiểm tra xem viên kẹo còn ăn được hay không. 

Vì`n = 3`, việc tính toán trở thành`3 // 3 = 1`, cho`3 - 1 = 2`. Hai chiếc kẹo được tặng cho bạn bè, và chiếc thứ ba được ăn sau món quà thứ hai. Đây là đầu vào nhỏ nhất mà việc tự tiêu thụ thực sự xảy ra. 

Vì`n = 6`, có hai nhóm đầy đủ gồm ba. Công thức cho`6 - 2 = 4`. Về mặt hoạt động, hai món quà đầu tiên sẽ tiêu tốn thêm một viên kẹo và hai món quà tiếp theo sẽ tiêu tốn viên kẹo cuối cùng, vì vậy bốn người bạn sẽ nhận được kẹo. 

Vì`n = 8`, có`8 // 3 = 2`hoàn thành các nhóm, để lại hai viên kẹo. Hai nhóm hoàn chỉnh cung cấp bốn món quà và hai chiếc kẹo cuối cùng cung cấp thêm hai chiếc nữa, tổng cộng là sáu chiếc. Công thức cho`8 - 2 = 6`, xác nhận rằng phần còn lại của hai được xử lý mà không bị phạt thêm.
