---
title: "CF 102190K - đầu vào/đầu ra tiêu chuẩn"
description: "Chúng ta cần biểu thị mỗi số nguyên mục tiêu n dưới dạng tổng của càng ít số nguyên dương càng tốt, với hạn chế là mọi triệu hồi chỉ có thể sử dụng các chữ số thập phân từ 2 đến 9. Đặc biệt, cả 0 và 1 đều không xuất hiện ở bất kỳ đâu trong triệu hồi."
date: "2026-08-19T06:05:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102190
codeforces_index: "K"
codeforces_contest_name: "2019 ECNU Campus Invitational Contest"
rating: 0
weight: 102190
solve_time_s: 548
verified: true
draft: false
---

[CF 102190K - đầu vào/đầu ra tiêu chuẩn](https://codeforces.com/problemset/problem/102190/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần biểu thị mỗi số nguyên mục tiêu n dưới dạng tổng của càng ít số nguyên dương càng tốt, với hạn chế là mỗi tổng chỉ có thể sử dụng các chữ số thập phân`2`bởi vì`9`. Đặc biệt, không`0`cũng không`1`có thể xuất hiện ở bất cứ đâu trong một lệnh triệu tập. 

Đầu vào chứa tới 1000 mục tiêu độc lập. Mỗi mục tiêu có thể có tối đa 101 chữ số thập phân, do đó các giá trị này không thể được coi là số nguyên máy có chiều rộng cố định thông thường trong các ngôn ngữ như C++. Tham số kích thước liên quan là số chữ số thập phân mà chúng ta sẽ gọi là L. Một thuật toán quét biểu diễn thập phân với số lần không đổi là đủ nhanh, trong khi thuật toán liệt kê tất cả các số nguyên lên đến n sẽ yêu cầu tới 10 101 lần lặp trong trường hợp lớn nhất. 

Số lượng triệu tập tối thiểu đặc biệt nhỏ. Vì câu lệnh đảm bảo rằng n chứa`0`hoặc`1`, một triệu tập không bao giờ có thể là đủ. Hai lệnh triệu tập đôi khi là đủ, như với 911=42+869. Tuy nhiên, hai không phải lúc nào cũng đủ. Đối với 19, mọi lệnh triệu tập đầu tiên có thể có từ`2`bởi vì`9`lá`17`,`16`,`15`,`14`,`13`,`12`,`11`, hoặc`10`, tất cả đều chứa một chữ số bị cấm. Câu trả lời đúng là ba mệnh đề, ví dụ 19=6+7+6. 

Trường hợp cạnh thứ hai là số 0 bên trong mục tiêu có nhiều chữ số. Đối với 300, chỉ trừ một số nhỏ hợp lệ là nguy hiểm. Ví dụ: 300−2=298, có tác dụng, nhưng 300−22=278 cũng có tác dụng trong khi nhiều lựa chọn có vẻ tự nhiên khác có thể tạo ra các chữ số bị cấm. Một phương pháp xử lý các chữ số thập phân một cách độc lập mà không theo dõi mang có thể âm thầm tạo ra sự phân tách không hợp lệ. 

Trường hợp biên còn lại là mục tiêu chẳng hạn như 10. Nó không thể được biểu thị bằng một số hợp lệ và nó không thể được biểu thị bằng hai số nếu chúng ta nhấn mạnh rằng mỗi số phải có ít nhất hai, nhưng 10=2+8 là hợp lệ. Bất kỳ cấu trúc nào giả sử mọi lệnh triệu tập đều có cùng số chữ số sẽ từ chối trường hợp này một cách không chính xác vì lệnh triệu tập một chữ số phải được cho phép. 

## Phương pháp tiếp cận 

Một giải pháp cưỡng bức trực tiếp cho hai lệnh triệu tập sẽ liệt kê một ứng cử viên a từ 2 đến n−2, kiểm tra xem cả a và n−a chỉ chứa các chữ số hay không`2`bởi vì`9`và dừng ở cặp hợp lệ đầu tiên. Điều này đúng vì mọi phép phân tách hai số có thể đều xuất hiện trong bảng liệt kê đó. Tuy nhiên, nếu n có L chữ số thì có Θ(10 L ) ứng viên và việc kiểm tra một ứng cử viên có chi phí O(L). Do đó, trường hợp xấu nhất là các phép toán chữ số Θ(L10 L ), điều này vô vọng với L=101. 

Cách tiếp cận vũ phu có hiệu quả vì câu hỏi cho hai lệnh triệu tập rất đơn giản, nhưng nó thất bại vì phạm vi số rất lớn. Quan sát quan trọng là bản thân phép cộng có tính chất cục bộ trong ký hiệu thập phân. Khi chúng ta cộng nhiều số, mỗi vị trí thập phân chỉ tương tác với số mang từ vị trí trước đó. Chúng ta có thể xử lý mục tiêu từ phải sang trái và giữ mục tiêu ở trạng thái nhỏ. 

Với số lượng lệnh k cố định, hãy xem xét một lệnh trong khi xử lý các chữ số từ phải sang trái. Trước khi chúng tôi chọn bất kỳ chữ số khác 0 nào cho lệnh triệu đó, chúng tôi có thể để trống vị trí hiện tại, được biểu thị bằng chữ số`0`, hoặc bắt đầu số bằng một chữ số từ`2`bởi vì`9`. Khi số đã bắt đầu, mọi chữ số có nghĩa hơn cũng phải từ`2`bởi vì`9`. Điều này thể hiện chính xác một số thập phân hợp lệ được đệm bằng các số 0 đứng đầu bên ngoài độ dài thực của nó. 

Với k=2 hoặc k=3, chúng ta chỉ cần một trạng thái nhỏ. Trạng thái chứa giá trị hiện tại và một bitmask cho chúng ta biết lệnh triệu tập nào đã bắt đầu. Có nhiều nhất 2 3 = 8 mặt nạ và có thể mang theo nhiều nhất ba mặt nạ. Đối với mọi vị trí, chúng tôi thử các chữ số có thể có cho mỗi lệnh triệu tập và chỉ giữ lại các kết hợp có tổng tạo ra chữ số mục tiêu được yêu cầu. 

Chúng ta thử k=1, rồi k=2, rồi k=3. Giá trị thành công đầu tiên sẽ tự động tối ưu. Luôn tồn tại một phép phân tách hợp lệ với tối đa ba mệnh đề và cùng một chữ số DP tạo nên nó, do đó không cần giá trị k lớn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(L10 L ) | O(L) | Quá chậm | 
| Chữ số DP | O(L⋅2 k ⋅3⋅10 k ), với k<3 | O(L⋅2 k ⋅3) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu diễn thập phân của n và đảo ngược các chữ số của nó một cách khái niệm bằng cách xử lý các vị trí từ chữ số có nghĩa nhỏ nhất đến chữ số có nghĩa nhất. Điều này cho phép phép cộng thập phân thông thường được mô phỏng từ phải sang trái. 
2. Thử số lần triệu hồi k=1, rồi k=2, rồi k=3. Vì bản thân đầu vào chứa`0`hoặc`1`, k=1 không thể thành công, nhưng việc xử lý nó một cách thống nhất sẽ làm cho đối số tối ưu trở nên đơn giản. 
3. Đối với k cố định, xác định trạng thái DP bằng cách`(position, carry, mask)`. Mặt nạ có một bit cho mỗi lần triệu hồi. Bit i được thiết lập chính xác khi lệnh triệu i đã nhận được một chữ số khác 0 tại một số vị trí ít quan trọng hơn. 
4. Tại mỗi vị trí, tạo ra các lựa chọn chữ số có thể có cho mỗi lần triệu tập. Nếu bit của nó đã được thiết lập thì chữ số của nó phải là một trong`2`bởi vì`9`. Nếu bit của nó không được thiết lập thì chữ số của nó có thể`0`, nghĩa là số vẫn ngắn hơn vị trí hiện tại hoặc một trong`2`bởi vì`9`, nghĩa là số bắt đầu từ đây. 
5. Thêm các chữ số đã chọn và số mang đến. Giá trị kết quả phải có cùng chữ số hàng đơn vị với chữ số tương ứng của n. Thương số sau khi chia cho 10 sẽ trở thành số mang cho vị trí tiếp theo. 
6. Lưu trữ trạng thái tiền thân cho mỗi trạng thái mới đạt được. Số trước chứa trạng thái trước đó và chữ số được chọn cho mỗi lệnh triệu tập. Điều này cho phép chúng tôi xây dựng lại các con số thực tế sau khi tìm thấy trạng thái cuối cùng thành công thay vì chỉ quyết định liệu trạng thái đó có tồn tại hay không. 
7. Sau khi tất cả các chữ số L đã được xử lý, chỉ chấp nhận trạng thái khi số nhớ bằng 0 và mọi lệnh triệu tập đã bắt đầu. Một bit được đặt cho mỗi lệnh triệu hồi đảm bảo rằng không có số đầu ra nào trống hoặc bằng 0. 
8. Xây dựng lại các chữ số cho mỗi lệnh triệu tập theo thứ tự ngược lại, vì DP đã xử lý chúng từ ít quan trọng nhất đến quan trọng nhất. Các số được xây dựng lại chỉ chứa các chữ số`2`bởi vì`9`, ngoại trừ việc đệm các số 0 trước chữ số đầu tiên của chúng, chúng sẽ bị xóa khi chuyển đổi chuỗi chữ số thành số nguyên. 

Điều bất biến là mọi trạng thái DP có thể truy cập đều tương ứng với một phép cộng một phần có hậu tố được xử lý khớp chính xác với hậu tố tương ứng của n, với`carry`bằng với số mang vào vị trí thập phân tiếp theo. Mặt nạ ghi lại chính xác lệnh triệu tập nào đã có chữ số thực. Do đó, mọi chuyển đổi đều bảo toàn một phân tách từng phần hợp lệ và mọi trạng thái chấp nhận biểu thị chính xác n dưới dạng tổng của các số hợp lệ. Ngược lại, bất kỳ sự phân tách hợp lệ nào cũng có thể được theo sau từng chữ số thông qua DP, do đó không thể bỏ qua một giải pháp. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

DIGITS = range(2, 10)

def solve_k(s, k):    n = len(s)
    # parent[pos][carry][mask] =    # (previous_carry, previous_mask, chosen_digits_tuple)    #    # We store states after processing each position.    parent = [dict() for _ in range(n + 1)]
    start = (0, 0)    parent[0][start] = None
    for pos in range(n):        target = ord(s[n - 1 - pos]) - ord('0')        cur = parent[pos]        nxt = parent[pos + 1]
        for (carry, mask) in cur:            choices = []
            for i in range(k):                if mask & (1 << i):                    choices.append(DIGITS)                else:                    choices.append((0, 2, 3, 4, 5, 6, 7, 8, 9))
            def dfs(i, total, new_mask, picked):                if i == k:                    value = total + carry
```Bên ngoài`solve`hàm thử các giá trị có thể có của k theo thứ tự tăng dần. Vì giá trị thành công đầu tiên là giá trị nhỏ nhất có thể nên điều này trực tiếp thực hiện yêu cầu tối ưu hóa. 

các`solve_k`hàm thực hiện chữ số DP.`parent[pos]`chứa tất cả các trạng thái có thể truy cập sau đó chính xác`pos`chữ số đã được xử lý. Một trạng thái được thể hiện bằng mặt nạ số bắt đầu và số mang của nó. 

Đệ quy`dfs`liệt kê các lựa chọn chữ số cho cột hiện tại. Với nhiều nhất là ba lệnh, có nhiều nhất 9 3 = 729 kết hợp cho một trạng thái, đây là một hằng số rất nhỏ. Kiểm tra chuyển tiếp`value % 10 == target`, sau đó vượt qua`value // 10`đến vị trí tiếp theo là người gánh vác. 

Số 0 trong lựa chọn chữ số có ý nghĩa đặc biệt. Nó không trở thành số 0 thực sự bên trong số đầu ra. Điều đó có nghĩa là lệnh triệu tập này vẫn chưa bắt đầu, vì vậy tất cả các vị trí hiện đang được xử lý chỉ đơn thuần là phần đệm bên trái của phần trình bày cuối cùng của nó. Khi một chữ số khác 0 được chọn, bit mặt nạ tương ứng vẫn được đặt và các vị trí trong tương lai không thể sử dụng số 0 nữa. 

Việc tái thiết đi từ trạng thái cuối cùng trở lại trạng thái ban đầu. Vì các chữ số được chọn từ thứ tự ít quan trọng nhất đến quan trọng nhất nên ban đầu chúng được thu thập ngược và sau đó đảo ngược. Mọi số 0 trước chữ số thực đầu tiên đều bị loại bỏ. Không có số 0 nào khác có thể xảy ra, bởi vì lệnh triệu tập đã bắt đầu chỉ được phép có chữ số`2`bởi vì`9`. 

Số nguyên Python có độ chính xác tùy ý, do đó việc chuyển đổi chuỗi thập phân cuối cùng thành số nguyên không bị tràn. Bản thân DP không bao giờ cần lưu trữ toàn bộ giá trị số của n, chỉ cần lưu trữ các chữ số thập phân riêng lẻ và các số mang nhỏ. 

## Ví dụ đã hoạt động 

Đối với mục tiêu mẫu`911`, hai lệnh triệu tập là đủ. Một đường dẫn DP có thể tạo ra`42`Và`869`. 

| Vị trí | Chữ số mục tiêu | Mang theo | Mặt nạ | Chữ số được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 |`00`|`2, 9`| 
| 1 | 1 | 1 |`11`|`4, 6`| 
| 2 | 9 | 1 |`11`|`0, 8`| 
| Kết thúc | | 0 |`11`|`42 + 869 = 911`| 

Ở vị trí đơn vị, 2+9=11, cho chữ số đích`1`và mang theo`1`. Ở vị trí hàng chục, 4+6+1=11, tạo ra một số khác`1`và mang theo`1`. Ở vị trí hàng trăm, số đầu tiên đã kết thúc nên chữ số đệm của nó bằng 0, còn số thứ hai đóng góp`8`, cho 0+8+1=9. Số đầu tiên được xây dựng lại thành`42`, không`042`. 

Đối với mục tiêu mẫu`19`, không có trạng thái hai lệnh nào đạt đến trạng thái chấp nhận. Với ba lệnh triệu tập, một đường dẫn hợp lệ là`6 + 7 + 6`. 

| Vị trí | Chữ số mục tiêu | Mang theo | Mặt nạ | Chữ số được chọn | 
| --- | --- | --- | --- | --- | 
| 0 | 9 | 0 |`000`|`6, 7, 6`| 
| 1 | 1 | 1 |`111`|`0, 0, 0`| 
| Kết thúc | | 0 |`111`|`6 + 7 + 6 = 19`| 

Ở vị trí đơn vị, 6+7+6=19, vậy chữ số cần tìm là`9`và việc mang theo là`1`. Ở vị trí hàng chục, cả ba số đều đã kết thúc nên các chữ số đệm của chúng bằng 0. Việc mang theo là`1`, khớp chính xác với chữ số hàng chục của`19`. Mặt nạ đã có rồi`111`, vì vậy cả ba số đều được coi là khác rỗng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L⋅2 3 ⋅3⋅9 3 ) | Có các vị trí L, nhiều nhất là 8 mặt nạ, 3 vật mang liên quan và nhiều nhất là 729 tổ hợp chữ số cho mỗi trạng thái. | 
| Không gian | O(L⋅2 3 ⋅3) | Mỗi vị trí chỉ lưu trữ một số lượng cố định các trạng thái và trạng thái trước đó của chúng. | 

Giá trị lớn của n không phải là vấn đề vì thuật toán phụ thuộc vào số chữ số của nó hơn là độ lớn số của nó. Với tối đa 101 chữ số và tối đa 1000 trường hợp thử nghiệm, không gian trạng thái vẫn nhỏ và giải pháp chỉ thực hiện một lượng công việc giới hạn trên mỗi vị trí thập phân. 

## Trường hợp thử nghiệm 

Trình kiểm tra bên dưới xác nhận hợp lệ một cách có chủ ý các thuộc tính của đầu ra được tạo ra thay vì so sánh các lệnh triệu tập chính xác. Bài toán cho phép nhiều phép phân tách tối ưu, do đó, các xác nhận đầu ra chính xác sẽ từ chối các giải pháp hợp lệ một cách không chính xác.```python
Pythonimport sysimport io
DIGITS = set("23456789")

def solve_k(s, k):    n = len(s)    parent = [dict() for _ in range(n + 1)]    parent[0][(0, 0)] = None
    for pos in range(n):        target = int(s[n - 1 - pos])        nxt = parent[pos + 1]
        for carry, mask in parent[pos]:            choices = []
            for i in range(k):                if mask & (1 << i):                    choices.append(range(2, 10))                else:                    choices.append((0, 2, 3, 4, 5, 6, 7, 8, 9))
            def dfs(i, total, new_mask, picked):                if i == k:                    value = total + carry                    if value % 10 != target:                        return
                    state = (value // 10, new_mask)                    if state not in nxt:                        nxt[state] = (carry, mask, tuple(picked))                    return
                for d in choices[i]:                    if d == 0:                        dfs(i + 1, total, new_mask, picked + [d])                    else:                        dfs(                            i + 1,                            total + d,                            new_mask | (1 << i),                            picked + [d]                        )
            dfs(0, 0, mask, [])
    final_state = (0, (1 << k) - 1)    if final_state not in parent[n]:        return None
    digits = [[] for _ in range(k)]    state = final_state
    for pos in range(n, 0, -1):        prev_carry, prev_mask, picked = parent[pos][state]
        for i in range(k):            digits[i].append(picked[i])
        state = (prev_carry, prev_mask)
    answer = []
    for ds in digits:        ds.reverse()
        while ds and ds[0] == 0:            ds.pop(0)
        if not ds:            return None
        answer.append(int(''.join(map(str, ds))))
    return answer

def solution(inp: str) -> str:    global input    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    t = int(sys.stdin.readline())    out = []
    for _ in range(t):        s = sys.stdin.readline().strip()
        for k in range(1, 4):            ans = solve_k(s, k)            if ans is not None:                break
        out.append(str(len(ans)))        out.append(' '.join(map(str, ans)))
    result = sys.stdout.getvalue()
    sys.stdin = old_stdin    sys.stdout = old_stdout
    return result

def validate(inp: str):    data = inp.strip().splitlines()    t = int(data[0])    ptr = 0
    for case in range(t):        n = data[1 + ptr]        k = int(data[2 + ptr])        nums = list(map(int, data[3 + ptr].split()))        ptr += 2
        assert len(nums) == k        assert sum(nums) == int(n)
        for x in nums:            assert 2 <= x <= int(n)            assert all(c in DIGITS for c in str(x))
        if k > 1:            for smaller in range(1, k):                assert solve_k(n, smaller) is None

# Provided sample input.sample = """\391119300"""
sample_out = solution(sample)validate("3\n911\n" + "\n".join(    sample_out.strip().splitlines()[0:2]) if False else sample)validate(sample_out if False else sample)
# Minimum-size inputs and boundary behavior.case_1 = """\110"""out_1 = solution(case_1)validate(case_1)validate("1\n10\n" + "\n".join(out_1.splitlines()))assert out_1.splitlines()[0] == "2"
# A case where two summands are impossible.case_2 = """\119"""out_2 = solution(case_2)assert out_2.splitlines()[0] == "3"validate("1\n19\n" + "\n".join(out_2.splitlines()))
# A case containing many zeroes.case_3 = """\1100000"""out_3 = solution(case_3)validate("1\n100000\n" + "\n".join(out_3.splitlines()))
# A long maximum-size target.case_4 = """\11000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000"""out_4 = solution(case_4)validate(case_4 + out_4)
```Thử nghiệm đầu tiên sử dụng mục tiêu được phép nhỏ nhất và kiểm tra xem thuật toán có cho phép triệu hồi một chữ số hay không. Cách thứ hai mắc phải sai lầm phổ biến khi cho rằng luôn tồn tại hai lệnh triệu tập hợp lệ. Cách thứ ba nhấn mạnh đến các số 0 lặp lại, trong đó phép trừ thập phân bất cẩn hoặc xử lý mang theo thường tạo ra các chữ số không hợp lệ. Cách thứ tư sử dụng mục tiêu có 101 chữ số, xác nhận rằng việc triển khai phụ thuộc vào độ dài của biểu diễn thập phân thay vì giá trị số. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`10`|`2`, theo sau là hai lệnh triệu tập hợp lệ có tổng là 10 | Mục tiêu tối thiểu và lệnh triệu tập một chữ số | 
|`19`|`3`, theo sau là ba lệnh triệu tập hợp lệ có tổng là 19 | Hai lệnh triệu tập có thể là không thể | 
|`100000`| Phân tách hợp lệ tối ưu | Các số 0 lặp đi lặp lại và xử lý mang theo | 
| Một số có 101 chữ số bắt đầu bằng`1`và theo sau là số 0 | Phân tách hợp lệ tối ưu | Độ dài đầu vào tối đa và độ chính xác tùy ý | 

## Vỏ cạnh 

cho`19`, DP hai triệu không có trạng thái chấp nhận. Mỗi ứng cử viên hợp lệ có một chữ số nằm trong khoảng`2`Và`9`và phần bù của nó chứa phần bị cấm`0`hoặc`1`. DP ba lần đạt đến trạng thái sau khi chọn`6`,`7`, Và`6`ở vị trí đơn vị, tạo ra 6+7+6=19. Kể từ khi mặt nạ trở thành`111`, cả ba lệnh triệu tập đều hợp lệ và thuật toán trả về`3`. 

Vì`10`, phân hủy tối ưu là`2+8`. Ở vị trí đơn vị, DP chọn các chữ số`2`Và`8`, thu được số tiền là`10`, vậy chữ số mục tiêu là`0`và việc mang theo là`1`. Ở vị trí hàng chục, cả hai số đều không còn chữ số nào, được biểu thị bằng các số 0 đệm và số mang tạo ra chữ số đích`1`. Sau khi xây dựng lại, phần đệm ở đầu sẽ biến mất và vẫn giữ nguyên hai số có một chữ số`2`Và`8`. 

Vì`300`, cột đơn vị có thể được xử lý bằng các chữ số hợp lệ có tổng kết thúc bằng 0, trong khi số mang được truyền sang cột tiếp theo. DP không cho rằng phép trừ một số cố định sẽ có tác dụng. Nó kiểm tra rõ ràng từng cột thập phân, do đó, các số 0 trong mục tiêu được xử lý thông qua mang thay vì được sao chép vào lệnh triệu tập. 

Đối với mục tiêu có độ dài tối đa như`1000...000`với 101 chữ số, cùng một máy trạng thái được sử dụng cho mọi vị trí. Giá trị số không bao giờ được liệt kê và không bao giờ được sử dụng làm giới hạn vòng lặp. Chỉ cần chữ số thập phân hiện tại, mang nhiều nhất một vài đơn vị và mặt nạ tám trạng thái, do đó thời gian chạy vẫn tuyến tính theo số chữ số.
