---
title: "CF 102439G - Thăm dò trình tự"
description: "Chúng ta bắt đầu với chuỗi 1. Mỗi thuật ngữ tiếp theo thu được bằng cách đọc thuật ngữ trước từ trái sang phải, nhóm các chữ số liên tiếp bằng nhau và thay thế mỗi nhóm bằng hai chữ số: độ dài của nó theo sau là chính chữ số đó."
date: "2026-08-10T06:52:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102439
codeforces_index: "G"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Semifinal"
rating: 0
weight: 102439
solve_time_s: 140
verified: true
draft: false
---

[CF 102439G - Khám phá trình tự](https://codeforces.com/problemset/problem/102439/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với chuỗi`1`. Mỗi thuật ngữ tiếp theo có được bằng cách đọc thuật ngữ trước từ trái sang phải, nhóm các chữ số liên tiếp bằng nhau và thay thế mỗi nhóm bằng hai chữ số: độ dài của nó theo sau là chính chữ số đó. Ví dụ,`13112221`bao gồm các nhóm`1`,`3`,`11`,`222`,`1`, trở thành`11`,`13`,`21`,`32`,`11`. 

Đầu vào chứa`n`, chỉ mục của thuật ngữ được yêu cầu và`m`, số chữ số phải được giữ lại từ đầu bên phải của nó. Chúng ta cần in thuật ngữ hoàn chỉnh khi nó có ít hơn`m`chữ số, nếu không thì chỉ là chữ số cuối cùng`m`chữ số. Những ràng buộc chính thức cho phép`n`để đạt được`10^18`trong khi`m`nhiều nhất là`1000`. 

Giá trị to lớn của`n`loại trừ việc xây dựng từng thuật ngữ một. Ngay cả độ dài của chuỗi cũng tăng theo cấp số nhân, với hệ số tăng trưởng nhìn và nói thông thường gần đạt đến hằng số Conway, khoảng`1.303577`. Một mô phỏng trực tiếp sẽ đạt tới các chuỗi thiên văn từ rất lâu trước khi`n`đã trở nên gần gũi từ xa`10^18`. 

Có một số trường hợp ranh giới mà giải pháp dựa trên hậu tố phải xử lý cẩn thận. Với đầu vào`1 2`, số hạng đầu tiên chỉ chứa một chữ số, vì vậy đáp án là`1`, không`01`. Với đầu vào`4 2`, số hạng thứ tư là`1211`, có hai chữ số tận cùng là`11`, vậy câu trả lời là`11`. Việc triển khai bất cẩn luôn định dạng chính xác`m`chữ số sẽ tạo ra`01`đối với trường hợp đầu tiên, trong khi việc triển khai ở phía sai của chuỗi sẽ không thành công trong trường hợp thứ hai. 

Một trường hợp tinh vi khác xảy ra khi hậu tố được yêu cầu chứa toàn bộ thuật ngữ hiện tại. Ví dụ, đầu vào`3 10`yêu cầu số hạng thứ ba, đó là`21`, vậy câu trả lời là`21`. Chúng ta không được đệm nó bằng số 0 hoặc phát minh ra những chữ số không tồn tại. 

Trình tự này có cấu trúc bổ sung giúp cho hậu tố có thể quản lý được. Bắt đầu từ`1`, chỉ có các chữ số`1`,`2`, Và`3`xảy ra, mọi số hạng đều kết thúc bằng chữ số gốc`1`và các chữ số bằng nhau liên tiếp không bao giờ tạo thành một chuỗi có độ dài bốn hoặc nhiều hơn. Thuộc tính cuối cùng có nghĩa là mỗi lần chạy có thể được biểu thị bằng một chữ số thập phân. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Lưu trữ toàn bộ chuỗi hiện tại, quét từ trái sang phải, tạo các chuỗi chạy và xây dựng chuỗi tiếp theo. Sau đó`n-1`lặp đi lặp lại, lấy cái cuối cùng`m`nhân vật. Điều này đúng vì nó tuân theo chính xác định nghĩa của dãy. 

Vấn đề là kích thước của chuỗi trung gian. Nếu chiều dài hiện tại là`L`, một lần chuyển đổi đã tốn phí`O(L)`thời gian và tạo ra một chuỗi có độ dài khoảng`1.3L`. Do đó, việc xây dựng ngay cả vài chục thuật ngữ đầu tiên cũng đòi hỏi phải xử lý hàng nghìn hoặc hàng triệu ký tự, trong khi`n`Có lẽ`10^18`. Một mô phỏng lực lượng vũ phu sẽ thực hiện theo thứ tự tổng của tất cả các độ dài thuật ngữ, theo cấp số nhân trong`n`, vì vậy nó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần toàn bộ số hạng. Chúng tôi chỉ cần hậu tố của nó và việc chuyển đổi mang tính cục bộ đối với các lần chạy. 

Biểu thị một thuật ngữ bằng các chữ số thay vì các chữ số riêng lẻ. Một lần chạy được lưu trữ dưới dạng`(digit, count)`. Một lần chạy đầu vào tạo ra chính xác hai ký tự đầu ra,`count`Và`digit`. Hãy xem xét điều cuối cùng`K`chạy của một thuật ngữ. Khi các lần chạy này được chuyển đổi, các cặp mã hóa của chúng luôn chứa ít nhất`K`đầu ra chạy. Lý do rất đơn giản: chữ số cuối cùng của mỗi cặp được mã hóa là chữ số của lần chạy đầu vào tương ứng và các lần chạy đầu vào liên tiếp có các chữ số khác nhau. Do đó, mỗi lần chạy đầu vào đóng góp ít nhất một lần chạy đầu ra riêng biệt. 

Kết quả là, cuối cùng`K`lần chạy của học kỳ tiếp theo chỉ phụ thuộc vào học kỳ cuối cùng`K`chạy của nhiệm kỳ hiện tại. Bất kỳ tương tác nào với các lần chạy đã bị loại bỏ chỉ có thể xảy ra ở phần đầu của hậu tố được chuyển đổi, trong khi phần cuối cùng`K`lần chạy đầu ra đã được xác định bởi số lần giữ lại`K`đầu vào chạy. 

Bây giờ chúng ta có thể chọn`K = m`. cuối cùng`m`chữ số thập phân chứa nhiều nhất`m`chạy, vì vậy cuối cùng`m`chạy là đủ để xây dựng lại hậu tố được yêu cầu. Quan trọng hơn, điều này mang lại cho chúng ta một trạng thái hữu hạn tất định: trạng thái đơn giản là trạng thái cuối cùng`m`chạy. 

Khi một trạng thái lặp lại, tất cả các trạng thái tiếp theo sẽ lặp lại với cùng một khoảng thời gian. Đối với trình tự nhìn và nói bắt đầu từ`1`, các trạng thái hậu tố này nhanh chóng chuyển sang trạng thái tuần hoàn quen thuộc ở đầu bên phải. Sự ổn định lan truyền từ phải sang trái, do đó, đối với một hậu tố chứa`m`chỉ chạy`O(m)`chuyển đổi là cần thiết trước khi trạng thái bước vào chu kỳ của nó. Trong thực tế, chu kỳ rất ngắn nhưng chúng ta không cần phải mã hóa chu kỳ của nó. Chúng ta có thể phát hiện chu trình trực tiếp bằng từ điển. 

Cách tiếp cận kết quả chỉ xử lý`O(m)`chạy trên mỗi trạng thái được tạo và chỉ`O(m)`trạng thái trước khi đạt đến chu kỳ hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| số mũ trong`n`| Quá chậm | 
| Mô phỏng hậu tố chạy với tính năng phát hiện chu kỳ |`O(m²)`|`O(m²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`, và đặt`K = m`. Chúng tôi sử dụng các lần chạy vì mỗi lần chạy chiếm ít nhất một chữ số, vì vậy số cuối cùng`m`chữ số không bao giờ có thể chứa nhiều hơn`m`chạy. 
2. Bắt đầu với học kỳ đầu tiên,`1`, được biểu diễn dưới dạng lần chạy đơn`(1, 1)`. Chúng tôi cũng giữ chỉ số thuật ngữ hiện tại, ban đầu`1`. 
3. Nếu kỳ hạn hiện tại có ít hơn`K`chạy, chuyển đổi danh sách chạy hoàn chỉnh bình thường. Ở giai đoạn này, toàn bộ thuật ngữ được biểu diễn nên không có vấn đề cắt ngắn. 
4. Một khi thuật ngữ có ít nhất`K`chạy, chỉ giữ lại cái cuối cùng của nó`K`chạy. Chuyển đổi những hoạt động đó bằng cách thay thế`(digit, count)`với hai nhân vật`count`Và`digit`, sau đó mã hóa độ dài chạy chuỗi ký tự kết quả. Bởi vì mỗi lần chạy đầu vào được giữ lại đóng góp ít nhất một lần chạy đầu ra, nên lần chạy cuối cùng`K`các lần chạy đầu ra là chính xác mặc dù tiền tố của thuật ngữ ban đầu đã bị loại bỏ. 
5. Sau mỗi lần chuyển đổi, hãy cắt bớt danh sách chạy kết quả xuống cuối cùng của nó`K`chạy. Trạng thái bây giờ là một bộ chứa nhiều nhất`K` `(digit, count)`cặp. 
6. Bắt đầu từ trạng thái đầu tiên chứa`K`chạy, lưu trữ từng trạng thái cùng với chỉ mục thuật ngữ nơi nó xuất hiện lần đầu tiên. Nếu một trạng thái xuất hiện trở lại thì sự khác biệt giữa hai chỉ số là độ dài chu kỳ. 
7. Hãy để`remaining`là số lần chuyển đổi vẫn cần thiết để đạt được số hạng`n`. Nếu như`remaining`lớn hơn độ dài chu kỳ, hãy giảm nó theo modulo độ dài chu kỳ. Điều này có khả năng thay thế`10^18`chuyển tiếp bằng giá trị chuyển đổi tối đa của một chu kỳ. 
8. Xây dựng lại cái cuối cùng`m`các chữ số từ danh sách chạy cuối cùng bằng cách đọc các lần chạy từ phải sang trái. Một cuộc chạy`(digit, count)`đóng góp`count`bản sao của`digit`; ít nhất hãy dừng lại ngay khi`m`chữ số đã được thu thập. Nếu toàn bộ thuật ngữ chứa ít hơn`m`chữ số, thay vào đó hãy trả lại cụm từ đầy đủ. 

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào trạng thái được lưu trữ chứa`K`chạy, nó chính xác là lần cuối cùng`K`chạy của số hạng dãy thực. Giả sử điều này đúng với thuật ngữ hiện tại. Mỗi lần chạy đầu vào được lưu trữ được mã hóa thành một cặp có chữ số cuối cùng bằng chữ số của lần chạy đầu vào đó. Vì các lần chạy đầu vào liên tiếp có các chữ số khác nhau nên mỗi lần chạy đầu vào đóng góp ít nhất một lần chạy đầu ra, vì vậy lần chạy cuối cùng`K`lần chạy đầu ra không thể phụ thuộc vào bất kỳ lần chạy đầu vào nào trước hậu tố được giữ lại. Do đó, phép biến đổi tạo ra chính xác kết quả cuối cùng`K`số hạng thực tiếp theo. Bất biến giữ sau mỗi lần chuyển đổi. 

Khi cùng một trạng thái xảy ra hai lần, thuyết tất định sẽ đưa ra cùng một trạng thái kế tiếp từ cả hai lần xuất hiện. Do đó, toàn bộ tương lai có tính tuần hoàn. Bỏ qua các khoảng thời gian hoàn chỉnh sẽ khiến trạng thái không thay đổi, do đó trạng thái đạt được ở kỳ hạn`n`được bảo tồn một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def normalize(runs, k):
    """
    Apply one look-and-say operation to the supplied suffix of runs.

    When k runs are supplied from the end of a real term, the last k
    output runs are determined completely by these runs.
    """
    chars = []

    for digit, count in runs:
        chars.append(str(count))
        chars.append(digit)

    out = []

    for ch in chars:
        if out and out[-1][0] == ch:
            out[-1] = (ch, out[-1][1] + 1)
        else:
            out.append((ch, 1))

    if len(out) > k:
        out = out[-k:]

    return tuple(out)

def suffix_from_runs(runs, m):
    parts = []
    need = m

    for digit, count in reversed(runs):
        take = min(count, need)
        parts.append(digit * take)
        need -= take
        if need == 0:
            break

    return ''.join(reversed(parts))

def solve():
    n, m = map(int, input().split())

    if n == 1:
        print("1")
        return

    k = m

    # The complete first term.
    state = (("1", 1),)
    index = 1

    # States are only needed once the suffix contains k runs.
    seen = {}
    history = []

    while index < n:
        if len(state) < k:
            new_state = normalize(state, k)
        else:
            if state not in seen:
                seen[state] = index
                history.append(state)

            new_state = normalize(state, k)

        index += 1
        state = new_state

        if index >= n:
            break

        if len(state) == k:
            if state in seen:
                cycle_start = seen[state]
                cycle_len = index - cycle_start

                remaining = n - index
                if remaining >= cycle_len:
                    skip = remaining // cycle_len
                    index += skip * cycle_len

                if index >= n:
                    break

            else:
                seen[state] = index
                history.append(state)

    print(suffix_from_runs(state, m))

if __name__ == "__main__":
    solve()
```Trạng thái được lưu trữ dưới dạng`(digit, count)`cặp chứ không phải là một chuỗi. Vì trình tự được tạo từ`1`không bao giờ chứa một chuỗi dài hơn ba chữ số bằng nhau, mỗi số đếm là một chữ số thập phân, khớp chính xác với quy tắc chuyển đổi.`normalize`đầu tiên tạo luồng ký tự được mã hóa từ các lần chạy được giữ lại. Sau đó, nó thực hiện mã hóa độ dài chạy thông thường trên luồng ngắn đó. Chỉ có cái cuối cùng`k`các lần chạy kết quả được giữ lại vì các lần chạy trước đó không thể ảnh hưởng đến hậu tố được yêu cầu. 

Từ điển chu trình được khóa theo trạng thái chạy hoàn chỉnh chứ không chỉ bằng một vài chữ số cuối cùng. Điều này quan trọng vì hai chuỗi có thể có cùng một hậu tố văn bản trong khi có các ranh giới chạy khác nhau và những ranh giới đó ảnh hưởng đến lần chuyển đổi tiếp theo. 

Không có chuyển đổi số nguyên ở bất kỳ đâu trong giải pháp. Câu trả lời có thể chứa tới`1000`các chữ số, vì vậy việc xử lý nó như một số nguyên Python sẽ không cần thiết và cũng có thể mất thông tin hàng đầu nếu hậu tố được yêu cầu bắt đầu bằng 0 trong một phiên bản khác của vấn đề. Ở đây cách biểu diễn an toàn nhất luôn là một chuỗi. 

Số lần chuyển đổi dựa trên các chỉ số thuật ngữ, không phải độ lệch dựa trên số 0. Thuật ngữ`1`là nhà nước`1`, và một phép biến đổi chuyển sang số hạng`2`. Việc duy trì quy ước đó xuyên suốt sẽ tránh được lỗi thường gặp nhất khi bỏ qua chu kỳ. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`1 2`. Không cần chuyển tiếp vì thuật ngữ được yêu cầu đã là thuật ngữ đầu tiên. 

| Chỉ số kỳ hạn | Chạy | Hậu tố được yêu cầu | 
| --- | --- | --- | 
| 1 |`(1,1)`|`1`| 

Thuật ngữ đầy đủ chỉ có một chữ số, do đó thuật toán trả về`1`thay vì đệm nó thành hai chữ số. Điều này thể hiện điều kiện biên ngắn hạn. 

Đối với Mẫu 2, đầu vào là`42 1`. Chúng tôi chỉ cần lần chạy cuối cùng vì`m = 1`. 

| Chỉ số kỳ hạn | Lần chạy cuối cùng | Chữ số cuối cùng | 
| --- | --- | --- | 
| 1 |`(1,1)`|`1`| 
| 2 |`(1,2)`|`1`| 
| 3 |`(1,1)`|`1`| 
| 4 |`(1,2)`|`1`| 
| ... | ... | ... | 
| 42 |`(1,1)`hoặc`(1,2)`|`1`| 

Mọi số hạng đều kết thúc bằng chữ số gốc`1`, do đó, bất kể số lần chạy cuối cùng xảy ra ở kỳ hạn nào`42`, chữ số cuối cùng của nó là`1`. Do đó, đầu ra là`1`. 

Hành vi thú vị hơn xuất hiện khi`m`lớn hơn. Ví dụ, bốn chữ số cuối cùng của dãy cuối cùng sẽ di chuyển theo một mẫu tuần hoàn ngắn: 

| Kỳ hạn | Bốn chữ số cuối | 
| --- | --- | 
| 8 |`3211`| 
| 9 |`1221`| 
| 10 |`2211`| 
| 11 |`2221`| 
| 12 |`3211`| 

Khi trạng thái chạy tương tự lặp lại, thuật toán không cần mô phỏng riêng các thuật ngữ còn lại. Nó nhảy theo chiều dài toàn bộ chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(m²)`| Nhiều nhất`O(m)`các trạng thái liên quan được tạo ra và mỗi quá trình chuyển đổi`O(m)`chạy | 
| Không gian |`O(m²)`| Cửa hàng từ điển chu trình`O(m)`trạng thái, mỗi trạng thái chứa`O(m)`chạy | 

Tối đa`m`chỉ là`1000`, do đó, khoảng một triệu mục nhập chạy được lưu trữ nằm trong giới hạn bộ nhớ, trong khi chu trình thực tế của trình tự này ngắn hơn nhiều. Sự cải tiến quan trọng là giá trị to lớn của`n`, lên tới`10^18`, không bao giờ xuất hiện trong vòng lặp mô phỏng ngoại trừ trong số học được sử dụng để bỏ qua các chu trình hoàn chỉnh. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_string(inp: str) -> str:
    data = inp.strip().split()
    n = int(data[0])
    m = int(data[1])

    if n == 1:
        return "1"

    def normalize(runs, k):
        chars = []
        for digit, count in runs:
            chars.append(str(count))
            chars.append(digit)

        out = []
        for ch in chars:
            if out and out[-1][0] == ch:
                out[-1] = (ch, out[-1][1] + 1)
            else:
                out.append((ch, 1))

        return tuple(out[-k:])

    def get_suffix(runs, k):
        parts = []
        need = k

        for digit, count in reversed(runs):
            take = min(count, need)
            parts.append(digit * take)
            need -= take
            if need == 0:
                break

        return ''.join(reversed(parts))

    state = (("1", 1),)
    index = 1
    seen = {}

    while index < n:
        state = normalize(state, m)
        index += 1

        if len(state) == m:
            if state in seen:
                cycle_start = seen[state]
                cycle_len = index - cycle_start

                remaining = n - index
                if remaining >= cycle_len:
                    index += (remaining // cycle_len) * cycle_len

                if index >= n:
                    break
            else:
                seen[state] = index

    return get_suffix(state, m)

# Provided sample 1.
assert solve_string("1 2\n") == "1", "sample 1"

# Provided sample 2.
assert solve_string("42 1\n") == "1", "sample 2"

# Minimum-size input.
assert solve_string("1 1\n") == "1", "minimum input"

# The fourth term is 1211, so its final two digits are 11.
assert solve_string("4 2\n") == "11", "off-by-one around fourth term"

# The fifth term is 111221, so its final two digits are 21.
assert solve_string("5 2\n") == "21", "suffix extraction"

# Large n with the smallest suffix.
assert solve_string("1000000000000000000 1\n") == "1", "maximum n"

# m is larger than the entire third term 21.
assert solve_string("3 100\n") == "21", "m larger than term"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Thuật ngữ tối thiểu và kích thước hậu tố tối thiểu | 
|`4 2`|`11`| Lập chỉ mục thuật ngữ chính xác và trích xuất bên phải | 
|`5 2`|`21`| Chạy chuyển đổi và xử lý hậu tố | 
|`1000000000000000000 1`|`1`| Tối đa`n`và bỏ qua chu kỳ | 
|`3 100`|`21`|`m`lớn hơn số hạng hoàn chỉnh | 

## Vỏ cạnh 

Đối với đầu vào`1 2`, thuật toán trả về ngay`1`. Danh sách chạy hiện tại chỉ chứa`(1,1)`và tổng chiều dài của nó nhỏ hơn`m`. Không có khoảng đệm bằng 0 nào được đưa ra, phù hợp với yêu cầu in chính thuật ngữ đó khi nó ngắn hơn hậu tố được yêu cầu. 

Đối với đầu vào`4 2`, các thuật ngữ được tạo ra là`1`,`11`,`21`, Và`1211`. Học kỳ thứ tư đã bắt đầu`(1,1)`,`(2,1)`,`(1,2)`. Đọc từ bên phải, lần chạy cuối cùng đóng góp`11`, vì vậy hậu tố được yêu cầu chính xác là`11`. Thuật toán không bao giờ nhầm lẫn số lần chạy`2`với hai chữ số đầu ra riêng biệt, vì lần chạy được biểu diễn có cấu trúc như`(digit='1', count=2)`. 

Đối với đầu vào`3 100`, hậu tố được yêu cầu dài hơn toàn bộ cụm từ. Thuật ngữ thứ ba là`21`, đại diện bởi`(2,1), (1,1)`. Việc xây dựng lại hậu tố tiêu tốn cả hai lần chạy và sau đó dừng lại vì thuật ngữ hoàn chỉnh đã được khôi phục. Đầu ra vẫn còn`21`, không có phần đệm số 0 nhân tạo. 

Đối với đầu vào`1000000000000000000 1`, thuật toán không cố gắng thực hiện`10^18`những biến đổi. Với`m = 1`, trạng thái được lưu trữ chỉ chứa lần chạy ngoài cùng bên phải. Sau thời gian ngắn nhất thời, trạng thái đó là tuần hoàn và phép tính chu trình bỏ qua phần lớn các chỉ số thuật ngữ được yêu cầu. Chữ số cuối cùng luôn là`1`, vì vậy đầu ra là`1`. 

Nguy cơ triển khai chính là cắt ngắn theo chữ số thay vì chạy. Hậu tố bị cắt giữa chừng sẽ mất toàn bộ số lượng và có thể thay đổi thuật ngữ tiếp theo. Việc lưu trữ các lần chạy hoàn chỉnh sẽ tránh được vấn đề đó hoàn toàn. Mối nguy hiểm khác là phát hiện các chu kỳ chỉ từ một hậu tố văn bản. Các ranh giới chạy là một phần của trạng thái vì phép biến đổi tiếp theo đọc các nhóm chứ không phải các ký tự riêng lẻ. Bằng cách lưu trữ cuối cùng`m`hoàn tất các lần chạy, thuật toán sẽ lưu giữ chính xác thông tin cần thiết cho quá trình chuyển tiếp hậu tố trong tương lai.
