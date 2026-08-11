---
title: "CF 102396G - Tràn trọng lượng"
description: "Chúng tôi có tới 25 quả cân và mỗi quả cân có thể được xử lý theo đúng một trong ba cách: có thể đặt nó lên đĩa thứ nhất, đặt lên đĩa thứ hai hoặc không sử dụng. Thang đo không so sánh số tiền thực tế."
date: "2026-08-11T23:31:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "G"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 427
verified: false
draft: false
---

[CF 102396G - Tràn trọng lượng](https://codeforces.com/problemset/problem/102396/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 7 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có tới 25 quả cân và mỗi quả cân có thể được xử lý theo đúng một trong ba cách: có thể đặt nó lên đĩa thứ nhất, đặt lên đĩa thứ hai hoặc không sử dụng. Thang đo không so sánh số tiền thực tế. Đầu tiên nó làm giảm tổng modulo của mỗi tấm`m`, sau đó so sánh các dư lượng đó. Chúng ta cần một phép gán khác rỗng mà hai số dư bằng nhau. 

Nếu trọng lượng`i`nằm ở đĩa đầu tiên, cho hệ số đi`+1`. Nếu nó ở tấm thứ hai thì cho hệ số`-1`. Nếu nó không được sử dụng, hãy cho nó hệ số`0`. Điều kiện trở thành 

[ 
\sum_{i=1}^{n} c_i a_i \equiv 0 \pmod m, 
] 

trong đó mọi hệ số đều thỏa mãn (c_i\in{-1,0,1}) và không phải tất cả các hệ số đều bằng 0. Khi các hệ số như vậy được tìm thấy, các hệ số dương mô tả tấm đầu tiên và các hệ số âm mô tả tấm thứ hai. 

Các ràng buộc hướng tới tìm kiếm theo cấp số nhân hơn là lập trình động đa thức. Chỉ có 25 trọng số, vì vậy sự phụ thuộc theo cấp số nhân vào`n`có thể được chấp nhận nếu số mũ được giảm bằng cách chia các trọng số. Một phép liệt kê trực tiếp có thể có (3^{25}=847288609443), vượt xa giới hạn một giây. Giá trị của`m`có thể gần như (4\cdot10^7), do đó, một mảng DP thông thường được lập chỉ mục theo phần dư cũng sẽ quá lớn để chuyển đổi cho mọi trọng số. Các ràng buộc vấn đề chính thức là`n <= 25`Và`m < 4 * 10^7`, với giới hạn thời gian một giây và bộ nhớ 512 MB. 

Một số trường hợp nhỏ có thể bộc lộ việc triển khai không chính xác. 

Vì`m = 1`, mọi vị trí không trống đều hợp lệ vì mọi số nguyên đều đồng dư với 0 modulo 1. Ví dụ:```
1 1
5
```có thể được trả lời bằng cách đặt vật nặng 1 lên đĩa thứ nhất và không đặt vật nặng nào lên đĩa thứ hai. Việc triển khai chỉ tìm kiếm hai tập hợp con khác nhau có thể vô tình báo cáo`-1`. 

Một trọng lượng có khối lượng đã chia hết cho`m`là một câu trả lời ngay lập tức. Ví dụ,```
1 7
14
```được giải quyết bằng cách đặt vật nặng 1 lên cả hai tấm. Điều kiện modulo là về phần dư chứ không phải về tổng thô. 

Nhiệm vụ trống không bao giờ được chấp nhận. Ví dụ,```
1 7
1
```không có giải pháp. Số tiền được ký duy nhất là`0`, từ việc không sử dụng gì, và`1`hoặc`-1`, từ việc sử dụng trọng lượng. Việc triển khai gặp nhau ở giữa một cách bất cẩn có thể tìm thấy phần dư bằng 0 từ nhiệm vụ trống ở cả hai nửa và chấp nhận nó một cách không chính xác. 

Không thể đặt cùng một trọng lượng lên cả hai tấm. Ví dụ,```
2 10
3 3
```được giải bằng cách đặt vật nặng 1 lên một đĩa và vật nặng 2 lên đĩa kia. Cả hai phần dư của mảng đều bằng 3. Một biểu diễn coi hai bên là tập con độc lập mà không nhớ rằng chúng phải rời nhau có thể vô tình sử dụng một chỉ mục hai lần. 

Cuối cùng, đẳng thức là modulo`m`, không bằng nhau của các khoản tiền thông thường. Vì```
2 5
7 2
```đặt hai vật nặng lên hai đĩa đối diện nhau có tác dụng vì`7 mod 5 = 2 mod 5`, mặc dù khối lượng thực tế của chúng khác nhau. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ xem xét từng trọng lượng một cách độc lập và thử cả ba lựa chọn: chưa sử dụng, tấm thứ nhất hoặc tấm thứ hai. Đối với mỗi bài tập, chúng tôi tính toán tổng modulo đã ký`m`và chấp nhận nếu nó bằng 0 và ít nhất một trọng số đã được chọn. Điều này đúng vì mọi vị trí hợp pháp đều tương ứng với chính xác một vectơ hệ số từ`{-1,0,1}`. 

Vấn đề là số lượng nhiệm vụ. Với 25 quả cân có 

[ 
3^{25}=847288609443 
] 

khả năng. Ngay cả khi việc kiểm tra một nhiệm vụ chỉ mất một vài lệnh máy, hàng trăm tỷ trạng thái cũng không thể đáp ứng được thời hạn. 

Quan sát hữu ích là số tiền đã ký có tính cộng. Chia các quả nặng thành hai nửa rời nhau. Đối với một bài tập trong nửa đầu, hãy để số tiền đã ký của nó là`x`. Đối với bài tập ở nửa sau, gọi tổng có dấu của nó là`y`. Chúng cùng nhau tạo thành một giải pháp hợp lệ chính xác khi 

[ 
x+y\equiv0\pmod m. 
] 

Vì vậy, thay vì liệt kê tất cả (3^n) phép gán, chúng ta liệt kê khoảng (3^{n/2}) phép gán trong mỗi nửa và so khớp các phần dư bổ sung. 

Với 25 trọng số, một nửa có tối đa 12 trọng số và nửa còn lại có nhiều nhất 13 trọng số. Không gian tìm kiếm của chúng chứa tối đa (3^{12}=531441) và (3^{13}=1594323) phép gán tương ứng. Chúng tôi lưu trữ một phép gán cho mỗi phần dư được tạo ra bởi nửa đầu trong bảng băm, sau đó liệt kê nửa sau và tìm phần dư là phủ định mô-đun của nó. 

Có một điểm tinh tế trong việc lưu trữ bài tập trống. Dư lượng bằng 0 luôn được tạo ra bằng cách không làm gì cả. Nếu chúng ta chỉ lưu trữ bài tập đó thì một tìm kiếm sau đó cũng cho kết quả bằng 0 có thể vô tình kết hợp hai bài tập trống. Việc triển khai từ chối trường hợp đó một cách rõ ràng và nó cũng ưu tiên phân công nửa đầu không trống cho phần dư bằng 0 khi tồn tại. 

Sự so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n)) | (O(n)) | Quá chậm | 
| Gặp nhau ở giữa | (O(3^{n/2})) | (O(3^{n/2})) | Đã chấp nhận | 

Số học mô-đun cũng có nghĩa là mọi tổng trung gian đều có thể giảm đi ngay lập tức. Số nguyên Python không bị giới hạn, do đó không có vấn đề tràn số nguyên mặc dù khối lượng ban đầu có thể lớn bằng (10^9). 

## Hướng dẫn thuật toán 

1. Chia tạ thành hai nửa liên tiếp. Nếu như`n = 25`, nửa đầu chứa 12 trọng số và nửa thứ hai chứa 13. Các nửa rời rạc, có nghĩa là bất kỳ phép gán nào được chọn độc lập trong mỗi nửa sẽ tự động sử dụng mọi trọng số nhiều nhất một lần. 
2. Liệt kê mọi nhiệm vụ tạm thời của nửa đầu. Với mỗi trọng số, hệ số`0`có nghĩa là không sử dụng,`1`có nghĩa là tấm đầu tiên, và`2`có nghĩa là tấm thứ hai. Chuyển đổi các lựa chọn này thành hệ số`0`,`+1`, Và`-1`, và tính tổng có dấu modulo`m`. 
3. Lưu trữ một mã hóa bậc ba cho mỗi dư lượng gặp phải trong nửa đầu. Nếu phần dư bằng 0 và bảng hiện chỉ chứa mã hóa trống, hãy thay thế nó khi một mã hóa khác có cùng phần dư xuất hiện. Mã hóa được lưu trữ đủ để tái tạo lại trọng số nào thuộc về mỗi tấm. 
4. Liệt kê mọi nhiệm vụ tạm thời của nửa sau. Giả sử tổng có dấu của nó là`s`. Một bài tập tương thích từ nửa đầu phải có dư lượng`(-s) mod m`, bởi vì tổng có dấu kết hợp phải bằng 0 modulo`m`. 
5. Tra cứu`(-s) mod m`ở hiệp một. Nếu nó vắng mặt, nhiệm vụ nửa sau này không thể tạo thành giải pháp. Nếu có, hãy kết hợp cả hai bảng mã. 
6. Chỉ từ chối kết hợp khi cả hai bảng mã đều trống. Bất kỳ sự kết hợp nào khác đều chứa ít nhất một trọng số đã chọn và có tổng số có dấu chia hết cho`m`, vì vậy đó là một câu trả lời hợp lệ. 
7. Giải mã hai bảng mã bậc ba. Một chữ số đại diện`+1`đi đến tấm đầu tiên, trong khi một chữ số đại diện`-1`đi đến tấm thứ hai. In hai bộ chỉ mục. 

Tại sao nó hoạt động: mọi vị trí hợp pháp đều có một đại diện có chữ ký duy nhất với các hệ số trong`{-1,0,1}`. Việc chia các chỉ số sẽ chia số tiền đã ký của nó thành phần đóng góp từ mỗi nửa, chẳng hạn`x`Và`y`, với`x + y ≡ 0 (mod m)`. Trong quá trình liệt kê nửa sau, thuật toán tìm kiếm chính xác phần dư nửa đầu bằng`-y`, vì vậy mọi giải pháp khả thi đều được xem xét. Ngược lại, mỗi cặp được tra cứu trả về có`x + y ≡ 0`và các nửa rời nhau nên phép gán được giải mã của chúng tạo thành một vị trí hợp pháp. Kiểm tra gán trống rõ ràng đảm bảo rằng ít nhất một trọng số thực sự được đặt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_map(values, mod):
    """
    Map residue -> one ternary encoding for this half.

    Ternary digit:
        0 = unused
        1 = first plate
        2 = second plate
    """
    result = {}

    def dfs(pos, total, code, place):
        if pos == len(values):
            old = result.get(total)
            if old is None or (total == 0 and old == 0 and code != 0):
                result[total] = code
            return

        # Leave this weight unused.
        dfs(pos + 1, total, code, place * 3)

        # Put it on the first plate.
        nxt = total + values[pos]
        if nxt >= mod:
            nxt -= mod
        dfs(pos + 1, nxt, code + place, place * 3)

        # Put it on the second plate.
        nxt = total - values[pos]
        if nxt < 0:
            nxt += mod
        dfs(pos + 1, nxt, code + 2 * place, place * 3)

    dfs(0, 0, 0, 1)
    return result

def find_in_second(values, mod, first_map):
    """
    Search all ternary assignments of the second half.
    Returns (first_code, second_code), or None.
    """

    answer = None

    def dfs(pos, total, code, place):
        nonlocal answer

        if answer is not None:
            return

        if pos == len(values):
            need = (-total) % mod
            first_code = first_map.get(need)

            if first_code is not None:
                if first_code != 0 or code != 0:
                    answer = (first_code, code)
            return

        # Unused.
        dfs(pos + 1, total, code, place * 3)

        if answer is not None:
            return

        # First plate.
        nxt = total + values[pos]
        if nxt >= mod:
            nxt -= mod
        dfs(pos + 1, nxt, code + place, place * 3)

        if answer is not None:
            return

        # Second plate.
        nxt = total - values[pos]
        if nxt < 0:
            nxt += mod
        dfs(pos + 1, nxt, code + 2 * place, place * 3)

    dfs(0, 0, 0, 1)
    return answer

def decode(code, length, offset, first, second):
    for i in range(length):
        digit = code % 3
        code //= 3

        index = offset + i + 1

        if digit == 1:
            first.append(index)
        elif digit == 2:
            second.append(index)

def solve():
    n, mod = map(int, input().split())
    a = list(map(int, input().split()))

    # Reducing the masses once makes every later transition smaller.
    a = [x % mod for x in a]

    # A split near the middle minimizes the larger ternary search space.
    mid = n // 2
    left = a[:mid]
    right = a[mid:]

    first_map = build_map(left, mod)
    answer = find_in_second(right, mod, first_map)

    if answer is None:
        print(-1)
        return

    left_code, right_code = answer

    first_plate = []
    second_plate = []

    decode(left_code, len(left), 0, first_plate, second_plate)
    decode(right_code, len(right), mid, first_plate, second_plate)

    print(len(first_plate))
    print(*first_plate)
    print(len(second_plate))
    print(*second_plate)

if __name__ == "__main__":
    solve()
```Dây chuyền tiền xử lý đầu tiên giảm thiểu mọi`a[i]`modulo`m`. Điều này an toàn về mặt toán học vì chỉ có dư lượng mới ảnh hưởng đến kết quả so sánh cuối cùng. Nó cũng cho phép mỗi lần chuyển đổi đệ quy ở trong khoảng`[0, m)`sử dụng một điều chỉnh có điều kiện thay vì liên tục xây dựng các số nguyên lớn hơn.`build_map`thực hiện toàn bộ tìm kiếm nửa đầu. các`code`biến là mã hóa cơ sở ba của các quyết định đã được đưa ra. các`place`biến là lũy thừa hiện tại của ba, do đó việc chọn tấm đầu tiên sẽ thêm`place`để mã hóa và chọn tấm thứ hai sẽ thêm`2 * place`. 

Số tiền đã ký được giữ theo modulo`m`sau mỗi sự lựa chọn. Đối với một quá trình chuyển đổi tích cực, việc thêm một giá trị có thể đạt được nhiều nhất`2m - 2`, vì vậy một phép trừ là đủ. Đối với một chuyển đổi âm, kết quả có thể thấp đến mức`-(m - 1)`, vậy chỉ cần thêm một lần là đủ. Điều này tránh được một`%`hoạt động tại mọi nút đệ quy. 

Việc xử lý đặc biệt lượng cặn bằng 0 rất dễ bị bỏ qua. Bài tập trống phải được lưu trữ vì nó có thể kết hợp hợp pháp với một bài tập không trống từ nửa còn lại. Tuy nhiên, nếu phép gán nửa đầu không trống cũng tạo ra số 0 thì tốt hơn nên thay thế mã hóa trống bằng nó. Điều kiện ở`build_map`xử lý chính xác trường hợp đó.`find_in_second`tìm kiếm nửa còn lại. Đối với mỗi dư lượng đã ký`total`, nó tính toán`(-total) % mod`và thực hiện một lần tra cứu từ điển. Quá trình đệ quy dừng ngay sau khi tìm thấy một cặp hợp lệ, do đó, các đầu vào thông thường kết thúc sớm hơn nhiều so với bảng liệt kê đầy đủ (3^{13}). 

Mã hóa bậc ba sử dụng chữ số có nghĩa nhỏ nhất cho trọng số sớm nhất trong mỗi nửa.`decode`nhiều lần mất`code % 3`và sau đó chia cho ba, phục hồi các quyết định theo đúng thứ tự mà chúng được tạo ra. Phần bù chỉ số cho nửa sau là`mid`, bởi vì vị trí địa phương đầu tiên của nó tương ứng với trọng số toàn cầu`mid + 1`. 

Python không có lỗi tràn số nguyên có chiều rộng cố định, vì vậy các tổng như khối lượng ban đầu sẽ an toàn. Việc triển khai vẫn thực hiện rút gọn mô-đun trong suốt quá trình tìm kiếm vì bản thân thuật toán hoạt động trên các lớp dư lượng. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 14
1 3 7 10
```sự chia tách là`[1, 3]`Và`[7, 10]`. 

| Sân khấu | Bài tập | Tổng có dấu modulo 14 | Dư lượng nửa đầu yêu cầu | 
| --- | --- | --- | --- | 
| Nửa đầu |`+1, +3`| 4 | | 
| Hiệp hai | trống | 0 | 0 | 
| Hiệp hai |`+7`| 7 | 7 | 
| Hiệp hai |`+10`| 10 | 4 | 
| Trận đấu |`(+1,+3)`với`(+10)`|`4 + 10 = 14 ≡ 0`| 4 | 

Do đó, thuật toán có thể đặt các trọng số 1, 2 và 4 lên tấm đầu tiên và để trống tấm thứ hai. Tổng số của họ là 14, vì vậy thang đo sẽ tính toán`14 mod 14 = 0`trên tấm đầu tiên và`0`vào thứ hai. Đầu ra của mẫu là khác nhau, nhưng cả hai đều hợp lệ vì vấn đề yêu cầu bất kỳ cấu trúc hợp lệ nào. 

Đối với mẫu 2,```
3 7
1 2 4
```sự chia tách là`[1]`Và`[2, 4]`. 

| Sân khấu | Bài tập | Tổng đã ký modulo 7 | Dư lượng nửa đầu yêu cầu | 
| --- | --- | --- | --- | 
| Nửa đầu |`+1`| 1 | | 
| Hiệp hai |`+2`| 2 | 5 | 
| Hiệp hai |`-2`| 5 | 2 | 
| Hiệp hai |`+4`| 4 | 3 | 
| Hiệp hai |`-4`| 3 | 4 | 
| Hiệp hai |`+2,+4`| 6 | 1 | 
| Trận đấu |`+1`với`+2,+4`|`1 + 6 = 7 ≡ 0`| 1 | 

Cấu trúc thu được đặt cả ba quả nặng lên tấm đầu tiên. Tổng của nó là 7, phần dư của nó theo modulo 7 bằng 0, trong khi tấm thứ hai trống. Đây chính xác là cấu trúc của mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(3^{n/2})) | Mỗi nửa được liệt kê một lần và mỗi trạng thái thực hiện số học mô-đun theo thời gian không đổi và đối với nửa sau, tra cứu bảng băm. | 
| Không gian | (O(3^{n/2})) | Nửa đầu lưu trữ một mã hóa bậc ba cho mỗi phần dư riêng biệt, với tối đa (3^{n/2}) mục nhập. | 

Vì`n = 25`, nửa lớn hơn chỉ có 13 trọng số nên nó chứa nhiều nhất`3^13 = 1,594,323`bài tập. Nửa nhỏ hơn có nhiều nhất`3^12 = 531,441`bài tập. Đây là một số bậc độ lớn nhỏ hơn so với trực tiếp`3^25`tìm kiếm và vừa vặn thoải mái trong giới hạn bộ nhớ 512 MB. Giá trị tối đa của`m`không xuất hiện dưới dạng kích thước của DP, do đó giới hạn modulo lớn không làm cho mức sử dụng bộ nhớ tỷ lệ với 40 triệu. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên các thử nghiệm sẽ xác thực vị trí được trả về thay vì so sánh chuỗi đầu ra chính xác. Quy trình khai thác sau đây sẽ kiểm tra xem mọi chỉ số được báo cáo đều hợp lệ, không có chỉ mục nào được sử dụng hai lần, đặt ít nhất một trọng lượng và tổng hai tấm có dư lượng bằng nhau.```python
import sys
import io

# Paste the solve_data implementation from the solution here.
def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        # Call the submitted solve() here.
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    n, mod = data[0], data[1]
    a = data[2:2 + n]

    tokens = out.split()
    if not tokens:
        return False

    if tokens[0] == "-1":
        return True

    p = 0

    k = int(tokens[p])
    p += 1
    first = list(map(int, tokens[p:p + k]))
    p += k

    q = int(tokens[p])
    p += 1
    second = list(map(int, tokens[p:p + q]))
    p += q

    if p != len(tokens):
        return False

    if k + q == 0:
        return False

    if any(x < 1 or x > n for x in first + second):
        return False

    if len(set(first)) != len(first):
        return False

    if len(set(second)) != len(second):
        return False

    if set(first) & set(second):
        return False

    s1 = sum(a[i - 1] for i in first) % mod
    s2 = sum(a[i - 1] for i in second) % mod

    return s1 == s2

# Provided sample 1.
sample1 = """\
4 14
1 3 7 10
"""
assert validate(sample1, solve_data(sample1)), "sample 1"

# Provided sample 2.
sample2 = """\
3 7
1 2 4
"""
assert validate(sample2, solve_data(sample2)), "sample 2"

# Minimum-size case and m = 1.
case1 = """\
1 1
123456789
"""
assert validate(case1, solve_data(case1)), "minimum size and modulo 1"

# A weight is itself divisible by m.
case2 = """\
1 7
14
"""
assert validate(case2, solve_data(case2)), "single divisible weight"

# Equal weights must be placed on opposite plates.
case3 = """\
2 10
3 3
"""
assert validate(case3, solve_data(case3)), "all equal values"

# Maximum n, with no signed sum able to reach a nonzero multiple
# of m. The total absolute sum is smaller than m.
case4 = "25 39999989\n" + " ".join(str(1 << i) for i in range(25)) + "\n"
result4 = solve_data(case4)
assert result4.strip() == "-1", "maximum-size no-solution case"

# Empty assignment must not be accepted.
case5 = """\
1 7
1
"""
assert solve_data(case5).strip() == "-1", "empty assignment"
```Hai thử nghiệm đầu tiên xác nhận cấu trúc mẫu đồng thời cho phép chương trình tạo ra một phép gán hợp lệ khác. Bài kiểm tra thứ ba kiểm tra nhỏ nhất có thể`n`và trường hợp đặc biệt`m = 1`. Phần thứ tư kiểm tra nghiệm trọng lượng đơn trực tiếp khi khối lượng chia cho môđun. Bước thứ năm kiểm tra xem hai vật nặng bằng nhau có thể cân bằng trên các tấm đối diện mà không cần sử dụng lại vật chia độ hay không. Trường hợp thứ sáu là trường hợp căng thẳng có kích thước tối đa buộc thuật toán phải khám phá không gian tìm kiếm và xác nhận rằng việc triển khai có thể báo cáo chính xác rằng không có giải pháp nào tồn tại. Bài kiểm tra cuối cùng đặc biệt phát hiện ra lỗi phổ biến là nhận bài trống. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 14 / 1 3 7 10`| Bất kỳ vị trí hợp lệ nào | Mẫu 1 | 
|`3 7 / 1 2 4`| Bất kỳ vị trí hợp lệ nào | Mẫu 2 | 
|`1 1 / 123456789`| Vị trí không trống | Kích thước tối thiểu và`m = 1`| 
|`1 7 / 14`| Trọng lượng 1 trên một trong hai đĩa | Trọng lượng chia trực tiếp | 
|`2 10 / 3 3`| Một trọng lượng trên mỗi đĩa | Giá trị bằng nhau và sự rời rạc | 
|`25 39999989 / 1 2 4 ... 2^24`|`-1`| Tìm kiếm toàn diện kích thước tối đa | 
|`1 7 / 1`|`-1`| Từ chối bài tập trống | 

## Vỏ cạnh 

Khi nào`m = 1`, mọi dư lượng đều bằng không. Đối với đầu vào```
1 1
5
```bản đồ nửa đầu chứa phần dư bằng 0 từ cả phép gán trống và phép gán sử dụng trọng số. Việc triển khai có chủ ý ưu tiên mã hóa khác rỗng cho phần dư bằng 0. Nửa thứ hai trống nên kết quả xây dựng chứa trọng số 1 và được chấp nhận. 

Khi một trọng số được chia cho`m`, bài tập trống ở nửa còn lại là đủ để hoàn thành nó. Vì```
1 7
14
```phần dư có dấu của trọng số 1 bằng 0. Bản đồ nửa đầu lưu trữ mã hóa khác trống cho phần dư bằng 0 và tìm kiếm nửa sau có thể sử dụng phép gán trống của nó. Vị trí kết hợp chứa một trọng số và có tổng bằng 0 theo modulo 7. 

Đối với trọng lượng bằng nhau, hãy xem xét```
2 10
3 3
```Nhiệm vụ`+3 - 3`đã ký tổng số bằng không. Vì hai trọng số thuộc về hai nửa khác nhau nên việc tra cứu gặp nhau ở giữa sẽ tìm thấy phần dư`3`từ một nửa và dư lượng`7`, sự phủ định mô-đun của nó, từ nửa còn lại. Kết quả được giải mã đặt hai chỉ số khác nhau lên các tấm đối diện nhau, cho ra dư lượng`3`Và`3`. 

Đối với một trường hợp không thể,```
1 7
1
```số tiền được ký không rỗng duy nhất là`1`Và`-1`, số dư của nó là`1`Và`6`. Cả hai đều không bằng không. Phần dư bằng 0 duy nhất xuất phát từ việc không chọn gì, nhưng tìm kiếm ở nửa sau loại bỏ rõ ràng cặp trong đó cả hai mã hóa đều bằng 0, do đó chương trình sẽ in ra`-1`. 

Đối với trường hợp kích thước tối đa, phần tách chứa 12 và 13 trọng số. Bản đồ đầu tiên có chỗ cho tối đa (3^{12}) bài tập riêng biệt, trong khi bảng liệt kê thứ hai kiểm tra nhiều nhất (3^{13}). Không có phần nào của việc triển khai phụ thuộc tuyến tính vào giá trị tiềm năng to lớn của`m`và mọi phép gán được biểu diễn gọn bằng một số nguyên bậc ba chứ không phải bằng một danh sách các chỉ số đã chọn. 

Bất biến trung tâm rất đơn giản: mỗi phần dư được lưu trữ biểu thị một phép gán thực có dấu của một nửa của nó và mỗi trạng thái nửa thứ hai chỉ được so khớp với phần bù mô-đun của tổng có dấu của chính nó. Sau khi tìm thấy một cặp, các hệ số của chúng mô tả vị trí hợp lệ trên các tập hợp chỉ số rời rạc, do đó đẳng thức của hai phần dư tấm được suy ra trực tiếp từ phương trình (x+y\equiv0\pmod m).
