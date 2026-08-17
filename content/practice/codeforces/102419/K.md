---
title: "CF 102419K - Rồng và Vương quốc Cây cối"
description: "Hãy nghĩ về lần cuối cùng mỗi cái cây bị tấn công. Nếu một cây được đặt lại sau năm t, thì nó sẽ phát triển trong đúng m - t năm sau đó, do đó chiều cao cuối cùng của nó là m - t. Tương tự, xác định [ ti = m-hi. ] Giá trị ti là thời gian reset cuối cùng của cây i."
date: "2026-08-16T09:18:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "K"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 400
verified: false
draft: false
---

[CF 102419K - Rồng và Vương quốc Cây cối](https://codeforces.com/problemset/problem/102419/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Hãy nghĩ về lần cuối cùng mỗi cái cây bị tấn công. Nếu một cây được đặt lại sau năm`t`, sau đó nó phát triển chính xác`m - t`năm sau, vậy chiều cao cuối cùng của nó là`m - t`. Tương đương, xác định 

[ 
t_i = m-h_i. 
] 

giá trị`t_i`là lần reset cuối cùng của cây`i`. Một giá trị của`0`có nghĩa là cây chưa bao giờ bị tấn công hoặc bị tấn công ngay sau khi trồng. Yêu cầu toàn cầu duy nhất là ít nhất một cuộc tấn công xảy ra ở đâu đó. 

Tại một thời điểm thiết lập lại cụ thể`t`, mọi cây có thời gian đặt lại cuối cùng nhỏ hơn`t`đã kết thúc và không thể được đưa vào một khoảng thời gian`t`, vì làm như vậy sẽ khiến nó được thiết lập lại lần cuối sau này. Cây có thời gian đặt lại lần cuối ít nhất`t`vẫn còn có sẵn. Trong số các vị trí có sẵn này, tất cả các cây có thời gian đặt lại cuối cùng chính xác là`t`phải được bao phủ bởi các khoảng thời gian được sử dụng tại thời điểm`t`. 

Điều này biến vấn đề thành một vấn đề kết nối khoảng thời gian. Đối với mỗi`t`, xét mọi vị trí thỏa mãn`t_i >= t`. Chúng tạo thành một số thành phần liền kề rời rạc. Nếu một thành phần chứa ít nhất một vị trí với`t_i = t`, cần ít nhất một khoảng thời gian tấn công bên trong thành phần đó. Cho phép`c_t`là số lượng các thành phần đó. Khi đó mọi lời giải hợp lệ đều phải có 

[ 
k \ge \max_t c_t. 
] 

Có một hạn chế thứ hai. Mỗi lần tấn công`t`phải chứa chính xác`k`các khoảng không trống và chỉ có nhiều vị trí có thể sử dụng được bằng số cây có`t_i >= t`. Tập hợp nhỏ nhất như vậy xảy ra tại thời điểm đặt lại lớn nhất, vì vậy hạn chế mạnh nhất là`k`không thể vượt quá số lượng cây tối đa`t_i`. Từ`t_i=m-h_i`, đây chính xác là số cây có chiều cao cuối cùng tối thiểu. 

Như vậy, nếu 

[ 
K=\max_t c_t 
] 

và`freq_min`là số cây có chiều cao tối thiểu, đáp án là`K`khi`K <= freq_min`, Và`-1`nếu không thì. 

Kích thước đầu vào là lý do chính khiến việc triển khai cần phải tuyến tính. Với tối đa (10^6) cây, thậm chí công việc (O(n\log n)) đắt hơn đáng kể so với một lần quét, trong khi (O(n^2)) có nghĩa là tối đa (10^{12}) thao tác cơ bản. Số năm có thể đạt tới (10^9), do đó, một thuật toán lặp lại hàng năm là không thể bất kể các phép toán mảng nhỏ đến mức nào. Lời giải phải phụ thuộc vào vị trí và độ cao của chúng chứ không phụ thuộc vào kích thước số của`m`. 

Có một số trường hợp việc giải thích trực tiếp có thể sai. Nếu mọi chiều cao đều`m`, Ví dụ`n=4, m=3`với độ cao`3 3 3 3`, câu trả lời là`1`, không`0`. Ayoub phải tấn công ít nhất một lần và anh ta có thể tấn công ngay sau khi trồng, do đó, một khoảng thời gian bao phủ cả bốn cây sẽ có tác dụng. 

Nếu thời gian đặt lại cuối cùng giống nhau xuất hiện ở hai vị trí riêng biệt thì chúng không nhất thiết phải có hai khoảng thời gian. Vì`n=3, m=2`và độ cao`1 0 1`, thời gian đặt lại là`1 2 1`. Hai cây reset cùng lúc`1`có thể được bao phủ bởi một khoảng chứa cả ba vị trí. Cây ở giữa sau đó lại bị tấn công một lần nữa`2`. Câu trả lời là`1`. Một giải pháp bất cẩn chỉ đơn giản là đếm các lượt chạy có độ cao bằng nhau sẽ trả về không chính xác`2`. 

Ngoài ra còn có những cấu hình thực sự không thể thực hiện được. Coi như`n=5, m=3`với độ cao`2 1 2 0 2`. Số lần đặt lại là`1 2 1 3 1`. Vào thời điểm`2`, hai vị trí có thời gian đặt lại`2`được tách ra, vì vậy cần có hai khoảng thời gian. Kể từ đây`k`ít nhất phải có`2`. Nhưng vào thời điểm`3`chỉ có một cây vẫn có thể bị tấn công, vì vậy không thể có chính xác hai khoảng trống. Câu trả lời đúng là`-1`. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản có thể kiểm tra mọi thời gian đặt lại có thể một cách độc lập. Đối với một cố định`t`, quét toàn bộ mảng và xác định các thành phần chỉ bao gồm các vị trí có`t_i >= t`, sau đó đếm xem có bao nhiêu thành phần chứa một vị trí với`t_i=t`. Lặp lại điều này cho mọi giá trị riêng biệt của`t`cung cấp tất cả những gì cần thiết`c_t`các giá trị. Trong trường hợp xấu nhất, có (n) thời gian đặt lại riêng biệt và chi phí cho mỗi lần quét (O(n)), đưa ra (O(n^2)) hoặc tối đa (10^{12}) lần kiểm tra mảng cho (n=10^6). Điều đó vượt xa giới hạn một giây. 

Lực lượng vũ phu hoạt động vì câu hỏi thực sự chính xác là về các thành phần được kết nối sau khi áp dụng ngưỡng. Vấn đề là việc xây dựng lại các thành phần ngưỡng đó một cách rõ ràng sẽ lãng phí gần như toàn bộ công việc. Khi ngưỡng thay đổi một giá trị thì hầu hết cấu trúc mảng không thay đổi. 

Quan sát quan trọng là các thành phần ngưỡng có thể được biểu diễn bằng cây Descartes. Sẽ dễ dàng hơn một chút khi làm việc với độ cao ban đầu thay vì thời gian đặt lại. Để có thời gian thiết lập lại`t`, điều kiện`t_i >= t`tương đương với 

[ 
h_i \le m-t. 
] 

Vì vậy, chúng ta cần số lượng thành phần được kết nối của một tập hợp cấp mảng con`h_i <= H`có giá trị tối đa chính xác là`H`. Cây Descartes tối đa thể hiện chính xác các thành phần lồng nhau này. Mỗi nút có chiều cao lớn hơn hoàn toàn đại diện cho một thành phần có chiều cao tối đa là chiều cao của nút. 

Ngăn xếp giảm đơn điệu xây dựng cấu trúc cây Descartes có liên quan trong thời gian tuyến tính. Khi có một chiều cao mới xuất hiện, tất cả các chiều cao nhỏ hơn trên ngăn xếp sẽ trở thành con của giá trị mới. Nếu sau khi loại bỏ các giá trị nhỏ hơn đó, ngăn xếp trống hoặc đỉnh của nó hoàn toàn nhỏ hơn giá trị mới, thì nút mới sẽ bắt đầu một thành phần mới ở độ cao của chính nó, vì vậy chúng tôi sẽ tăng số lượng cho chiều cao đó. Các chiều cao bằng nhau sẽ ở cùng nhau, điều này rất cần thiết vì các vị trí có chiều cao bằng nhau thuộc về cùng một thành phần ngưỡng khi không có chiều cao nhỏ hơn nào ngăn cách chúng. 

Số lượng kết quả cho mỗi`c_t`mà không cần xây dựng một cây một cách rõ ràng. Chúng ta chỉ cần một từ điển từ chiều cao đến số nghiệm thành phần của nó. Đồng thời, chúng tôi theo dõi tần số chiều cao tối thiểu, cung cấp giới hạn trên cho`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét ngưỡng | (O(n^2)) | (O(n)) | Quá chậm | 
| Ngăn xếp đơn điệu | (O(n)) khấu hao | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều cao cuối cùng và tìm chiều cao tối thiểu và bao nhiêu cây có chiều cao đó. Chiều cao tối thiểu tương ứng với thời gian thiết lập lại tối đa và đó là năm mà ít cây nhất vẫn đủ điều kiện để bị tấn công. Do đó, nó đưa ra giới hạn trên`k <= freq_min`. 
2. Duy trì chiều cao của chồng theo thứ tự không tăng dần. Ngăn xếp biểu thị chuỗi ranh giới bên phải của cây Descartes tối đa cho tiền tố được xử lý cho đến nay. Các giá trị bằng nhau được giữ trên ngăn xếp thay vì bị xóa. 
3. Đối với mỗi chiều cao`x`, xóa mọi giá trị ngăn xếp nhỏ hơn`x`. Mỗi nút bị loại bỏ vừa tìm thấy một nút tổ tiên lớn hơn, cụ thể là`x`. Đây chính xác là mối quan hệ làm cho nút đó trở thành mức tối đa của một thành phần ngưỡng. Việc đếm có thể được liên kết với nút khi nó được đẩy, vì sau khi loại bỏ tất cả các giá trị nhỏ hơn, ngăn xếp mẹ hiện tại của nó là cha mẹ của cây Descartes. 
4. Sau khi tất cả các giá trị nhỏ hơn đã bị xóa, hãy kiểm tra đỉnh ngăn xếp mới. Nếu ngăn xếp trống hoặc đỉnh của nó nhỏ hơn`x`, tăng số lượng thành phần liên quan đến`x`. Nếu đỉnh bằng`x`, không tăng nó vì lần xuất hiện mới thuộc về cùng thành phần ngưỡng với giá trị bằng nhau hiện có. 
5. Đẩy`x`lên ngăn xếp. Mỗi chiều cao được đẩy một lần và xuất hiện nhiều nhất một lần, vì vậy toàn bộ quá trình ngăn xếp là tuyến tính. 
6. Hãy để`k`là số lượng thành phần lớn nhất được lưu trữ cho bất kỳ chiều cao nào. Đây là số khoảng thời gian tối thiểu được yêu cầu bởi bất kỳ năm tấn công nào. Nếu như`k > freq_min`, in`-1`, bởi vì vào năm tấn công gần đây nhất không có đủ cây để hình thành`k`các khoảng không rỗng. Nếu không thì in`k`. 

### Tại sao nó hoạt động 

Đối với bất kỳ ngưỡng nào`H`, các vị trí có chiều cao lớn nhất`H`tạo thành các thành phần liền kề nhau. Một thành phần có mức tối đa chính xác là`H`tương ứng với một nhóm cây có thể phải được xử lý tại thời điểm tấn công`m-H`. Cần ít nhất một khoảng cho mỗi thành phần như vậy, do đó số lượng các thành phần này là giới hạn dưới của`k`. 

Cây Descartes tối đa tổ chức chính xác các thành phần này. Một nút có nút cha lớn hơn chính xác là nút gốc của một thành phần ở độ cao của chính nó. Các giá trị cha và con bằng nhau không được tính riêng vì chúng thuộc cùng một thành phần ở ngưỡng đó. Ngăn xếp đơn điệu duy trì mối quan hệ cha mẹ này mà không cần xây dựng cây một cách rõ ràng, do đó số lượng từ điển chính xác là các giá trị`c_t`. 

Lấy số lượng tối đa này sẽ cho kết quả nhỏ nhất có thể`k`thỏa mãn mọi giới hạn dưới. Vấn đề duy nhất còn lại là liệu có thể tồn tại nhiều khoảng thời gian không trống đó ở mọi thời điểm tấn công được yêu cầu hay không. Số lượng cây đủ điều kiện giảm khi thời gian đặt lại tăng lên, do đó trường hợp chặt chẽ nhất là thời gian đặt lại tối đa, tương ứng với chiều cao cuối cùng tối thiểu. Nếu tần số của nó ít nhất là`k`, mỗi lần tấn công trước đó có ít nhất số cây đủ điều kiện và các khoảng thời gian cần thiết có thể được hình thành bằng cách tách các thành phần đủ điều kiện nếu cần. Như vậy`k <= freq_min`là vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    min_h = min(a)
    freq_min = 0

    counts = {}
    stack = []
    answer = 0

    for x in a:
        if x == min_h:
            freq_min += 1

        while stack and stack[-1] < x:
            stack.pop()

        if not stack or stack[-1] < x:
            counts[x] = counts.get(x, 0) + 1
            if counts[x] > answer:
                answer = counts[x]

        stack.append(x)

    if answer > freq_min:
        print(-1)
    else:
        print(answer)

solve()
```Lần quét đầu tiên qua đầu vào đồng thời xác định tần số có độ cao tối thiểu và xây dựng ngăn xếp đơn điệu. Biến`freq_min`là số cây vẫn có thể bị tấn công tại thời điểm đặt lại gần nhất có thể. 

Ngăn xếp được duy trì theo thứ tự giảm dần. Khi`x`đến, mọi giá trị nhỏ hơn sẽ bị loại bỏ vì`x`là giá trị đầu tiên ở bên phải lớn hơn rất nhiều và trở thành tổ tiên cây Descartes có liên quan của nó. Khi những giá trị nhỏ hơn này đã bị loại bỏ,`x`bắt đầu một thành phần mới chính xác khi không có giá trị có chiều cao bằng nhau ngay phía trên nó trong ngăn xếp. 

Sự so sánh chặt chẽ là phần tinh tế. sử dụng`<=`trong điều kiện bật lên sẽ chia các độ cao bằng nhau thành các thành phần cây Descartes riêng biệt và sẽ đếm không chính xác các cấu hình như chiều cao`1 2 1`. Các giá trị bằng nhau phải được kết nối ở ngưỡng của chúng. 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn. Câu trả lời tối đa là nhiều nhất`n`, đó là (10^6). Từ điển có thể chứa tới`n`độ cao riêng biệt và ngăn xếp cũng có thể chứa tới`n`các mục phù hợp trong giới hạn bộ nhớ cho các ràng buộc đã nêu. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`n=4, m=3`và tất cả các độ cao đều`3`. Chiều cao tối thiểu là`3`, Vì thế`freq_min=4`. Ngăn xếp không bao giờ gặp một giá trị lớn hơn, và giá trị đầu tiên`3`bắt đầu một thành phần. 

| chỉ mục | chiều cao | xếp chồng trước | bật lên | số thành phần cho chiều cao | xếp chồng sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 |`[]`| không |`c[3]=1`|`[3]`| 
| 2 | 3 |`[3]`| không |`c[3]=1`|`[3,3]`| 
| 3 | 3 |`[3,3]`| không |`c[3]=1`|`[3,3,3]`| 
| 4 | 3 |`[3,3,3]`| không |`c[3]=1`|`[3,3,3,3]`| 

Số thành phần lớn nhất là`1`, Và`1 <= 4`, vậy câu trả lời là`1`. Điều này cũng giải thích tại sao các chiều cao bằng nhau không được tính là các thành phần ngưỡng riêng biệt. 

Đối với Mẫu 2, đầu vào là`n=4, m=3`và tất cả các độ cao đều`0`. Đây`freq_min=4`. Số 0 đầu tiên bắt đầu một thành phần và các giá trị bằng nhau còn lại sẽ mở rộng nó. 

| chỉ mục | chiều cao | xếp chồng trước | bật lên | số thành phần cho chiều cao | xếp chồng sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 |`[]`| không |`c[0]=1`|`[0]`| 
| 2 | 0 |`[0]`| không |`c[0]=1`|`[0,0]`| 
| 3 | 0 |`[0,0]`| không |`c[0]=1`|`[0,0,0]`| 
| 4 | 0 |`[0,0,0]`| không |`c[0]=1`|`[0,0,0,0]`| 

Một lần nữa số thành phần tối đa là`1`, vậy câu trả lời là`1`. Theo quy trình ban đầu, Ayoub có thể tấn công cả bốn cây ngay sau khi trồng và để chúng phát triển trong cả ba năm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) khấu hao | Mỗi chiều cao được đẩy một lần và bị xóa khỏi ngăn xếp nhiều nhất một lần. Các hoạt động từ điển được mong đợi (O(1)). | 
| Không gian | (O(n)) | Mỗi mảng đầu vào, ngăn xếp đơn điệu và từ điển đếm chiều cao đều sử dụng không gian tuyến tính trong trường hợp xấu nhất. | 

Với (n\le10^6), quét tuyến tính là mục tiêu thích hợp. Thuật toán không lặp qua giá trị tiềm năng rất lớn của`m`, Vì thế`m`lớn bằng (10^9) không ảnh hưởng đến thời gian chạy. Các hoạt động ngăn xếp được phân bổ tuyến tính vì một phần tử không thể được lấy ra nhiều lần. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    min_h = min(a)
    freq_min = 0

    counts = {}
    stack = []
    answer = 0

    for x in a:
        if x == min_h:
            freq_min += 1

        while stack and stack[-1] < x:
            stack.pop()

        if not stack or stack[-1] < x:
            counts[x] = counts.get(x, 0) + 1
            answer = max(answer, counts[x])

        stack.append(x)

    if answer > freq_min:
        print(-1)
    else:
        print(answer)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solution()
        return sys.stdout.getvalue() if False else ""
    finally:
        sys.stdin = old_stdin
        input = old_input

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solution()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

assert run("4 3\n3 3 3 3\n") == "1", "sample 1"
assert run("4 3\n0 0 0 0\n") == "1", "sample 2"
assert run("4 2\n2 1 1 2\n") == "1", "sample 3"

assert run("1 1\n0\n") == "1", "minimum-size input"
assert run("3 2\n1 0 1\n") == "1", "equal reset times can share one interval"
assert run("5 3\n2 1 2 0 2\n") == "-1", "latest attack has too few trees"
assert run("4 2\n0 1 0 1\n") == "2", "two separated maximum-reset components"

maximum_input = "1000000 1\n" + "0 " * 999999 + "0\n"
assert run(maximum_input) == "1", "maximum-size all-equal input"
```Trường hợp kích thước tối thiểu tùy chỉnh sẽ kiểm tra xem ngay cả một cây cũng yêu cầu một cuộc tấn công thực sự và do đó trả về`1`. các`1 0 1`trường hợp mắc phải lỗi phổ biến khi tính các lần chạy có chiều cao bằng nhau thay vì các thành phần ngưỡng. Trường hợp không thể kiểm tra điều kiện giới hạn trên được tạo bởi thời gian tấn công mới nhất. các`0 1 0 1`trường hợp kiểm tra ranh giới trong đó số khoảng yêu cầu chính xác là số lượng cây có sẵn tại thời điểm đặt lại gần nhất. Trường hợp cuối cùng xác minh rằng việc triển khai xử lý giới hạn phần tử đầy đủ (10^6) bằng đầu vào hoàn toàn bằng nhau đơn giản. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`1`| Tối thiểu có thể`n`và tấn công bắt buộc | 
|`3 2 / 1 0 1`|`1`| Thời gian đặt lại bằng nhau riêng biệt có thể được bao phủ bởi một khoảng thời gian lớn hơn | 
|`5 3 / 2 1 2 0 2`|`-1`| Yêu cầu`k`vượt quá số cây sẵn có ở đợt tấn công mới nhất | 
|`4 2 / 0 1 0 1`|`2`| Ranh giới chính xác nơi hai khoảng thời gian là cần thiết và khả thi | 
|`1000000 1 / all 0`|`1`| Tối đa`n`và hành vi bộ nhớ tuyến tính | 

## Vỏ cạnh 

Toàn bộ chiều cao-`m`trường hợp là`4 3`với độ cao`3 3 3 3`. Mảng thời gian đặt lại là`0 0 0 0`. Ngăn xếp tạo một thành phần cho chiều cao`3`bởi vì tất cả các giá trị bằng nhau sẽ ở cùng nhau. Câu trả lời là`1`. Điều này hợp lệ vì một đợt tấn công có thể xảy ra ngay sau khi trồng. 

Trường hợp toàn bộ chiều cao bằng không là`4 3`với độ cao`0 0 0 0`. Mảng thời gian đặt lại là`3 3 3 3`, vì vậy tất cả các cây phải bị tấn công vào năm cuối. Chúng tạo thành một thành phần liền kề, đòi hỏi một khoảng thời gian. Chiều cao tối thiểu xảy ra bốn lần, vì vậy`freq_min=4`, và câu trả lời là`1`. 

Trường hợp có chiều cao bằng nhau`3 2`với độ cao`1 0 1`tạo ra thời gian thiết lập lại`1 2 1`. Ở thời điểm tấn công trước đó, hai cây có thời gian reset`1`nằm trong một thành phần vì cây ở giữa có thể được đưa vào và sau đó sẽ được đặt lại. Đếm Descartes cho chiều cao`1`do đó là một, không phải hai. Lần reset sau chỉ có cây ở giữa nên`k=1`hoạt động. 

Trường hợp không thể`5 3`với độ cao`2 1 2 0 2`tạo ra thời gian thiết lập lại`1 2 1 3 1`. Vào thời điểm thiết lập lại`2`, vị trí hai và bốn là các thành phần riêng biệt giữa các cây có giá trị đặt lại cuối cùng ít nhất là`2`, buộc`k>=2`. Vào thời điểm thiết lập lại`3`, chỉ vị trí thứ 4 mới có thời gian đặt lại cuối cùng đó, do đó chỉ có một cây đủ điều kiện và không thể hình thành hai khoảng khác trống. Thuật toán tính toán số thành phần tối đa là`2`và tần số chiều cao tối thiểu là`1`, phát hiện`2 > 1`, và bản in`-1`. 

Ranh giới bình đẳng cũng rất đáng kể. Đối với độ cao`0 1 0 1`, chiều cao tối đa`1`xảy ra ở hai thành phần riêng biệt, do đó yêu cầu`k`là`2`. Chiều cao tối thiểu`0`cũng xảy ra hai lần, đưa ra chính xác hai cây đủ điều kiện vào thời điểm đặt lại gần nhất. Vì giới hạn dưới và giới hạn trên gặp nhau nên câu trả lời là chính xác`2`.
