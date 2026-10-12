---
title: "CF 105020H - Bánh quy"
description: "Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản có tổng cộng $n$ cookie, trong đó $a$ là lớn. Donia muốn ăn bánh quy nhưng cô phải tránh bị bắt. Quy tắc là sau khi cô ấy ăn bánh quy, ít nhất $m$ bánh quy vẫn phải chưa được ăn."
date: "2026-06-28T01:59:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105020
codeforces_index: "H"
codeforces_contest_name: "TCPC Tunisian Collegiate Programming Contest 2022"
rating: 0
weight: 105020
solve_time_s: 73
verified: false
draft: false
---

[CF 105020H - Cookie](https://codeforces.com/problemset/problem/105020/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra một số kịch bản độc lập. Trong mỗi kịch bản đều có$n$tổng số cookie, trong đó$a$lớn. Donia muốn ăn bánh quy nhưng cô phải tránh bị bắt. Quy tắc là sau khi cô ấy ăn bánh quy, ít nhất$m$cookie vẫn phải chưa được sử dụng. 

Những chiếc bánh quy duy nhất cô ấy quan tâm đến việc ăn là những chiếc bánh quy lớn. Vì vậy cô ấy cố gắng tối đa hóa bao nhiêu$a$những cookie lớn cô ấy có thể loại bỏ trong khi vẫn đảm bảo rằng số lượng cookie còn lại ít nhất là$m$. 

Nói lại, cô ấy bắt đầu với$n$cookie, trong đó$a$là “có giá trị” (lớn). Cô ấy loại bỏ càng nhiều cookie có giá trị càng tốt, nhưng cô ấy không thể giảm tổng số cookie còn lại bên dưới$m$. Câu hỏi đặt ra là có thể loại bỏ bao nhiêu cookie lớn một cách an toàn. 

Ràng buộc$n \le 10^{12}$ngay lập tức loại trừ mọi mô phỏng trên cookie. Giải pháp phải có thời gian không đổi cho mỗi trường hợp thử nghiệm, vì có tới$10^5$các trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào lặp đi lặp lại hoặc xây dựng bất cứ điều gì tỷ lệ thuận với$n$là không thể. 

Trường hợp cạnh tinh tế xuất hiện khi$n = m$. Trong trường hợp đó không thể ăn được cái bánh quy nào cả, vì vậy câu trả lời phải bằng 0 bất kể$a$. Một trường hợp cạnh khác là khi$n - m$lớn hơn$a$, nghĩa là có đủ “chỗ” để ăn hết những chiếc bánh quy lớn nhưng vẫn thỏa mãn sự ràng buộc. 

Một sai lầm ngây thơ là chỉ nghĩ$a$vấn đề và trở lại$a$, bỏ qua hạn chế từ$m$. Một sai lầm khác là diễn giải điều kiện là “để lại chính xác$m$” thay vì “ít nhất$m$”, điều này sẽ ép buộc sự bình đẳng và giảm tính linh hoạt một cách không chính xác. 

Ví dụ, nếu$n=10, m=3, a=7$, một cách tiếp cận bất cẩn có thể nghĩ rằng có thể ăn hết 7 chiếc bánh quy lớn. Nhưng nếu ăn hết 7 con thì chỉ còn lại 3 con là hợp lệ nên đáp án thực chất là 7. Điều này cho thấy ràng buộc là về tính khả thi chứ không phải hạn ngạch khắt khe. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ cố gắng mô phỏng việc ăn từng chiếc bánh quy, luôn thích những chiếc bánh quy lớn cho đến khi tất cả những chiếc bánh quy lớn được ăn hết hoặc số lượng còn lại sẽ giảm xuống dưới$m$. Điều này hoạt động về mặt khái niệm: ở mỗi bước, chúng tôi xóa một cookie lớn và kiểm tra xem các cookie còn lại có còn ít nhất không$m$. Tuy nhiên, trong trường hợp xấu nhất chúng ta có thể mô phỏng tới$10^{12}$việc loại bỏ, vượt xa mọi giới hạn thời gian khả thi. 

Quan sát quan trọng là quá trình này không phụ thuộc vào thứ tự hoặc tính ngẫu nhiên. Điều duy nhất quan trọng là có thể xóa tổng cộng bao nhiêu cookie mà không vi phạm ràng buộc. Nếu chúng ta loại bỏ$x$thì bánh quy$n - x \ge m$, Vì thế$x \le n - m$. Điều này có nghĩa là tổng số cookie chúng tôi có thể xóa bị giới hạn bởi$n - m$. Vì chúng tôi chỉ muốn xóa các cookie lớn nên số lượng thực tế chúng tôi có thể lấy cũng bị giới hạn bởi số lượng cookie lớn tồn tại, tức là$a$. Vì vậy, câu trả lời chỉ đơn giản là mức tối thiểu giữa số lượng cookie lớn hiện có và số lượng cookie tối đa có thể được loại bỏ một cách an toàn. 

Điều này làm giảm vấn đề thành một so sánh số học trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$| Quá chậm | 
| Tối ưu |$O(1)$mỗi trường hợp thử nghiệm |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$m$, Và$a$. Chúng xác định tổng số cookie, số lượng cookie còn lại tối thiểu được yêu cầu và số lượng cookie lớn. 
2. Tính tổng cộng có bao nhiêu cookie có thể được loại bỏ mà không vi phạm điều kiện. Vì ít nhất$m$nhiều nhất phải còn lại$n - m$tổng cộng có thể ăn được bánh quy. 
3. So sánh giới hạn này với$a$, vì chúng ta không thể ăn nhiều bánh quy lớn hơn mức tồn tại. 
4. Xuất ra giá trị nhỏ hơn giữa$a$Và$n - m$. 

Lý do đằng sau bước 2 là mỗi chiếc bánh quy được ăn sẽ giảm số lượng còn lại đi đúng một, bất kể nó lớn hay nhỏ. Vì vậy, ràng buộc chỉ phụ thuộc vào tổng số bị loại bỏ chứ không phụ thuộc vào loại của chúng. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, nếu Donia ăn$x$cookie, số lượng còn lại chính xác là$n - x$. Điều kiện đòi hỏi$n - x \ge m$, sắp xếp lại thành$x \le n - m$. Vì vậy, tổng số cookie cô ấy có thể xóa bị giới hạn trên toàn cầu bởi$n - m$. Vì cô ấy chỉ quan tâm đến những cookie lớn nên chiến lược tốt nhất là luôn dành toàn bộ “ngân sách” xóa này cho những cookie lớn cho đến khi hết ngân sách hoặc cô ấy dùng hết những cookie lớn. Điều này đảm bảo cả tính tối đa và hiệu lực. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, a = map(int, input().split())
        max_remove = n - m
        if max_remove < 0:
            max_remove = 0
        print(min(a, max_remove))

if __name__ == "__main__":
    solve()
```Mã xử lý từng trường hợp thử nghiệm một cách độc lập. Tính toán chính là`n - m`, đại diện cho số lượng cookie tối đa có thể bị xóa trong khi vẫn để lại ít nhất$m$phía sau. Nếu như$n < m$, giá trị này trở thành âm, do đó nó được giữ ở mức 0 vì không được phép loại bỏ. 

Câu trả lời cuối cùng là mức tối thiểu giữa ngân sách loại bỏ cho phép này và số lượng cookie lớn có sẵn. Điều này trực tiếp thực hiện ràng buộc dẫn xuất mà không cần bất kỳ mô phỏng nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, m = 2, a = 3
```Chúng tôi theo dõi số lượng bánh quy lớn có thể ăn được: 

| Bước | Công suất còn lại (n - x) | Ăn lớn x | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | vâng | 
| 1 | 4 | 1 | vâng | 
| 2 | 3 | 2 | vâng | 
| 3 | 2 | 3 | vâng | 

Đây$n - m = 3$, do đó có thể xóa tối đa 3 cookie. Từ$a = 3$, chúng ta có thể ăn cả 3 cái bánh quy lớn. 

Đầu ra là 3. 

### Ví dụ 2 

đầu vào:```
n = 6, m = 4, a = 5
```| Bước | Công suất còn lại (n - x) | Ăn lớn x | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 0 | 6 | 0 | vâng | 
| 1 | 5 | 1 | vâng | 
| 2 | 4 | 2 | vâng | 
| 3 | 3 | 3 | không | 

Đây$n - m = 2$, vậy nên mặc dù$a = 5$, chúng ta chỉ được ăn 2 cái bánh quy lớn trước khi vi phạm ràng buộc. 

Đầu ra là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T)$| Mỗi trường hợp thử nghiệm chỉ yêu cầu các phép tính số học có thời gian không đổi | 
| Không gian |$O(1)$| Không có cấu trúc bổ sung nào được sử dụng | 

Giải pháp dễ dàng phù hợp trong giới hạn vì thậm chí$10^5$trường hợp thử nghiệm chỉ yêu cầu$10^5$hoạt động liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n, m, a = map(int, input().split())
        max_remove = max(0, n - m)
        out.append(str(min(a, max_remove)))
    return "\n".join(out)

# provided samples
assert run("3\n5 2 1\n5 2 2\n5 5 5\n") == "1\n2\n0", "sample 1"

# custom cases
assert run("1\n1 1 1\n") == "0", "minimum edge"
assert run("1\n1000000000000 1 1000000000000\n") == "999999999999", "large upper bound"
assert run("1\n10 10 5\n") == "0", "exact equality case"
assert run("2\n10 3 20\n6 4 10\n") == "7\n2", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 0 | không được phép gỡ bỏ | 
| thang đo 10^12 | giá trị lớn | xử lý giới hạn trên | 
| n = m | 0 | điều kiện biên | 
| trường hợp hỗn hợp | đúng phút | tính đúng đắn chung | 

## Vỏ cạnh 

Khi nào$n = m$, ràng buộc buộc phải loại bỏ bằng 0 vì bất kỳ cookie nào được ăn sẽ vi phạm yêu cầu tối thiểu còn lại. Thuật toán tính toán$n - m = 0$, vì vậy câu trả lời cuối cùng trở thành$\min(a, 0) = 0$, phù hợp chính xác với yêu cầu. 

Khi$n < m$, được cho phép bởi các ràng buộc, giá trị được tính toán$n - m$là tiêu cực. Việc triển khai kẹp nó ở mức 0, nghĩa là không thể ăn được cookie. Điều này phản ánh chính xác rằng điều kiện đã bị vi phạm hoặc không thể đáp ứng sau bất kỳ lần xóa nào. 

Khi$a \ge n - m$, yếu tố hạn chế trở thành hạn chế còn lại thay vì tính sẵn có của các cookie lớn. Thuật toán trả về chính xác$n - m$, đảm bảo sử dụng hết công suất cho phép.
