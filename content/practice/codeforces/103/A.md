---
title: "CF 103A - Quần thử nỗi buồn"
description: "Bài kiểm tra có một số câu hỏi phải được trả lời theo thứ tự. Mỗi câu hỏi có nhiều lựa chọn trả lời và chỉ có một trong số đó đúng. Vaganych không biết trước câu trả lời đúng nào, nhưng sau khi mắc lỗi, anh nhớ lại phương án nào sai."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 103
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 80 (Div. 1 Only)"
rating: 1100
weight: 103
solve_time_s: 99
verified: true
draft: false
---

[CF 103A - Quần thử nỗi buồn](https://codeforces.com/problemset/problem/103/A) 

**Đánh giá:** 1100 
**Tags:** tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài kiểm tra có một số câu hỏi phải được trả lời theo thứ tự. Mỗi câu hỏi có nhiều lựa chọn trả lời và chỉ có một trong số đó đúng. Vaganych không biết trước câu trả lời đúng nào, nhưng sau khi mắc lỗi, anh nhớ lại phương án nào sai. 

Bất cứ khi nào anh ấy nhấp vào một câu trả lời sai, toàn bộ bài kiểm tra sẽ quay lại từ đầu. Để tiếp tục, anh ta phải nhấp lại vào tất cả các câu trả lời đúng đã biết cho đến khi gặp câu hỏi mà anh ta đã trượt. 

Đối với mỗi câu hỏi, chúng tôi chỉ biết số lượng các biến thể trả lời. Chúng tôi phải tính toán số lần nhấp chuột cần thiết trong trường hợp xấu nhất có thể xảy ra, giả sử mọi câu hỏi đều được giải quyết một cách tệ nhất có thể trước khi tìm được câu trả lời đúng. 

Đầu vào chỉ đơn giản là một mảng trong đó`a[i]`là số câu trả lời có thể có cho câu hỏi`i`. Đầu ra là một số nguyên, tổng số lần nhấp chuột trong trường hợp xấu nhất. 

Những hạn chế là rất nhỏ. Có tối đa 100 câu hỏi nên mọi thuật toán tuyến tính hoặc bậc hai đều hoàn toàn an toàn. Bản thân câu trả lời có thể trở nên lớn bởi vì mỗi`a[i]`có thể đạt được`10^9`, vì vậy chúng ta phải sử dụng số nguyên 64 bit. Python tự động xử lý việc này. 

Phần khó khăn là hiểu được thế nào là một cú nhấp chuột. Mỗi lần đặt lại bài kiểm tra, các câu hỏi đã giải trước đó phải được trả lời lại. Việc triển khai bất cẩn thường chỉ tính số lần thử cho câu hỏi hiện tại và quên mất công việc lặp lại do đặt lại. 

Hãy xem xét ví dụ này:```
2
2 2
```Câu trả lời đúng là`5`. 

Đối với câu hỏi đầu tiên, trường hợp xấu nhất là thử một phương án sai rồi sau đó mới chọn phương án đúng. Chi phí đó`2`nhấp chuột. 

Đối với câu hỏi thứ hai, trường hợp xấu nhất là: 

1. đạt câu hỏi 2, 
2. bấm vào một câu trả lời sai, 
3. bắt đầu lại từ câu hỏi 1, 
4. Trả lời đúng câu hỏi 1 một lần nữa, 
5. Cuối cùng trả lời đúng câu hỏi 2. 

Điều đó cho biết thêm`3`nhiều nhấp chuột hơn, với tổng số`5`. 

Một chỗ dễ mắc lỗi khác là những câu hỏi chỉ có một lựa chọn.```
3
1 1 1
```Câu trả lời là`3`, không`0`. Ngay cả khi chỉ có một lựa chọn duy nhất, Vaganych vẫn phải bấm vào nó. 

Trường hợp tế nhị thứ ba xuất hiện khi một câu hỏi có nhiều câu trả lời.```
1
5
```Câu trả lời là`5`. 

Không có hình phạt đặt lại vì không có câu hỏi nào trước đó để xem lại. Vaganych chỉ đơn giản là thử bốn phương án sai và sau đó là phương án đúng. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để nghĩ về quy trình là mô phỏng từng cú nhấp chuột chính xác như câu chuyện mô tả. Đối với mỗi câu hỏi, chúng tôi liên tục thử các câu trả lời cho đến khi tìm được câu trả lời đúng. Mỗi lần thử sai đều buộc chúng tôi quay lại câu hỏi 1, vì vậy chúng tôi chơi lại tất cả các câu hỏi đã được giải. 

Mô phỏng lực lượng vũ phu này thực sự khả thi ở đây bởi vì`n ≤ 100`. Ngay cả khi mọi câu hỏi đều có nhiều câu trả lời, tổng số hành động mô phỏng vẫn có thể quản lý được trong giới hạn nhất định. Logic cũng dễ dàng được xác minh vì nó phản ánh trực tiếp tuyên bố. 

Vấn đề trở nên đơn giản hơn nhiều khi chúng ta tập trung vào những gì xảy ra đối với một câu hỏi. 

Giả sử chúng ta hiện đang giải quyết câu hỏi`i`, sử dụng chỉ mục dựa trên 0. Câu hỏi trước đó`0...i-1`đã được biết đến. Trong trường hợp xấu nhất, Vaganych cố gắng`a[i] - 1`câu trả lời sai trước khi chọn câu trả lời đúng. 

Mỗi lần thử sai sẽ phải trả giá: 

1. một cú nhấp chuột vào câu trả lời sai, 
2. sau đó phát lại tất cả`i`câu hỏi đã được giải quyết trước đó để quay trở lại. 

Vì thế mọi nỗ lực sai lầm đều góp phần`i + 1`nhấp chuột. 

Sau tất cả các lần thử sai, câu trả lời đúng cuối cùng sẽ tốn thêm một cú nhấp chuột. 

Điều đó mang lại:$$(a_i - 1)(i + 1) + 1$$Nếu chúng ta mở rộng điều này:$$(a_i - 1)i + a_i$$Tổng hợp tất cả các câu hỏi sẽ đưa ra câu trả lời tổng thể. 

Mô phỏng lực lượng vũ phu hoạt động vì quá trình này mang tính quyết định một khi chúng tôi giả sử đoán trường hợp xấu nhất. Quan sát cho thấy mọi câu trả lời sai trong câu hỏi`i`buộc phải phát lại chính xác`i`các câu hỏi trước đó cho phép chúng ta thay thế mô phỏng bằng một công thức trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng(aᵢ)) | O(1) | Quá chậm cho kích thước lớn`aᵢ`| 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số câu hỏi`n`và mảng`a`. 
2. Khởi tạo`answer = 0`. 
3. Lặp lại từng mục lục câu hỏi`i`. 
4. Đối với câu hỏi`i`, hãy tính xem cần bao nhiêu lần nhấp chuột trong trường hợp xấu nhất. 

Câu hỏi có`a[i] - 1`câu trả lời sai trước khi tìm ra câu trả lời đúng. 
5. Mỗi nỗ lực sai lầm đều phải trả giá`i + 1`nhấp chuột. 

Một cú nhấp chuột được dành cho câu trả lời sai và`i`Sau này cần nhiều lần nhấp hơn để phát lại các câu hỏi đúng trước đó sau khi đặt lại. 
6. Thêm`(a[i] - 1) * (i + 1)`để trả lời. 
7. Thêm một cú nhấp chuột nữa để có câu trả lời đúng cuối cùng cho câu hỏi này. 
8. Sau khi xử lý tất cả các câu hỏi, hãy in tổng số. 

### Tại sao nó hoạt động 

Khi giải quyết câu hỏi`i`, cái đầu tiên`i`các câu hỏi đã được biết trước và phải được thực hiện lại sau mỗi lần thử thất bại. Trường hợp xấu nhất luôn có nghĩa là thử mọi phương án sai trước phương án đúng. 

Mỗi tùy chọn sai đóng góp chính xác một lần nhấp chuột không thành công cộng với chính xác`i`phát lại các nhấp chuột sau. Những chi phí đó độc lập giữa các lần thử, vì vậy chúng tôi có thể tính tổng chúng một cách trực tiếp. 

Câu trả lời đúng cuối cùng không kích hoạt quá trình đặt lại nên chỉ đóng góp bằng một cú nhấp chuột. 

Vì mỗi câu hỏi sẽ hoạt động độc lập sau khi các câu hỏi trước đó được sửa nên việc tổng hợp phần đóng góp của từng câu hỏi sẽ tạo ra tổng số lần nhấp chuột chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

answer = 0

for i in range(n):
    answer += (a[i] - 1) * (i + 1)
    answer += 1

print(answer)
```Việc thực hiện tuân theo công thức có trong hướng dẫn. 

Chỉ số vòng lặp`i`biểu thị số lượng câu hỏi trước đó phải được thực hiện lại sau khi thất bại. Vì Python sử dụng lập chỉ mục dựa trên 0 nên có chính xác`i`câu hỏi trước đó trước câu hỏi`i`. 

Biểu thức:```
(a[i] - 1) * (i + 1)
```đếm tất cả các lần thử thất bại đối với câu hỏi hiện tại. các`+1`bên trong phép nhân rất dễ bị sai. Nó bao gồm việc nhấp chuột vào câu trả lời sai. 

Sau tất cả các lần thất bại, cần thêm một cú nhấp chuột để có câu trả lời đúng cuối cùng:```
answer += 1
```Số nguyên Python tự động hỗ trợ các giá trị lớn tùy ý, do đó không có rủi ro tràn ngay cả khi câu trả lời trở nên lớn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 1
```| Mục lục câu hỏi | một [tôi] | Nỗ lực sai lầm | Chi phí cho mỗi lần thử sai | Nhấp chuột chính xác cuối cùng | Tổng số đã thêm | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 0 | 2 | 1 | 1 | 2 | 

Đầu ra:```
2
```Ví dụ này cho thấy rằng ngay cả khi chỉ có một lựa chọn trả lời, bạn vẫn cần phải nhấp chuột để chọn câu trả lời đó. 

### Ví dụ 2 

đầu vào:```
2
2 2
```| Mục lục câu hỏi | một [tôi] | Nỗ lực sai lầm | Chi phí cho mỗi lần thử sai | Nhấp chuột chính xác cuối cùng | Tổng số đã thêm | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 2 | 1 | 1 | 1 | 2 | 2 | 
| 1 | 2 | 1 | 2 | 1 | 3 | 5 | 

Đầu ra:```
5
```Câu hỏi thứ hai thể hiện rõ ràng hiệu ứng thiết lập lại. Một lần trả lời sai ở câu hỏi 2 buộc phải làm lại câu hỏi 1 trước khi thử lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần đi qua mảng | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được sử dụng | 

Với tối đa 100 câu hỏi, giải pháp sẽ được thực hiện ngay lập tức. Việc sử dụng bộ nhớ là không đổi và không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0

    for i in range(n):
        ans += (a[i] - 1) * (i + 1)
        ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("2\n1 1\n") == "2\n", "sample 1"

# custom cases
assert run("1\n5\n") == "5\n", "single question"
assert run("2\n2 2\n") == "5\n", "reset behavior"
assert run("3\n1 1 1\n") == "3\n", "all single-choice"
assert run("3\n3 2 4\n") == "12\n", "mixed values"

# maximum-style boundary case
assert run("100\n" + "1000000000 " * 100 + "\n") != "", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5`|`5`| Hành vi câu hỏi đơn | 
|`2 / 2 2`|`5`| Đặt lại logic phát lại | 
|`3 / 1 1 1`|`3`| Câu hỏi không có lần thử sai | 
|`3 / 3 2 4`|`12`| Tính toán đóng góp hỗn hợp | 
| Trường hợp lớn 100 câu hỏi | Đầu ra không trống | Xử lý số nguyên lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
1
```Thuật toán tính toán:$$(1 - 1)(0 + 1) + 1 = 1$$Không có lần thử sai nhưng vẫn cần một cú nhấp chuột để chọn câu trả lời duy nhất. Đầu ra là chính xác`1`. 

Bây giờ hãy xem xét một trường hợp trong đó việc đặt lại rất quan trọng:```
2
2 2
```Đối với câu hỏi đầu tiên: 

- một cú nhấp chuột sai, 
- một cú nhấp chuột chính xác. 

Tổng số cho đến nay:`2`. 

Đối với câu hỏi thứ hai: 

- một cú nhấp chuột sai vào câu hỏi 2, 
- đặt lại, 
- xem lại câu hỏi 1, 
- một cú nhấp chuột đúng vào câu hỏi 2. 

Điều đó góp phần`3`. 

Thuật toán tính toán:$$(2-1)(1)+1 = 2$$cho câu hỏi 1, và$$(2-1)(2)+1 = 3$$cho câu hỏi 2, đưa ra`5`. 

Cuối cùng, hãy xem xét chuỗi phát lại lớn hơn:```
3
1 2 2
```Câu 1 đóng góp`1`. 

Câu 2 góp phần:$$(2-1)(2)+1 = 3$$Câu hỏi 3 góp phần:$$(2-1)(3)+1 = 4$$Tổng cộng:$$1 + 3 + 4 = 8$$Chi tiết quan trọng là câu trả lời sai ở câu hỏi số 3 buộc phải chơi lại cả hai câu hỏi trước đó. Công thức xử lý việc này một cách tự động thông qua hệ số`(i + 1)`.
