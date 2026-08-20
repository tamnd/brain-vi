---
title: "CF 102185E - \u0421\u043a\u0440\u0449\u043d\u044f"
description: "Chúng ta có một từ điển gồm (N) từ đầy đủ. Sau đó chúng ta nhận được (M) từ từ một văn bản. Một từ văn bản được coi là viết tắt nếu nó có thể thu được bằng cách xóa các ký tự khỏi chính xác một từ trong từ điển, trong khi không có từ điển nào khác chứa nó dưới dạng một chuỗi con."
date: "2026-08-19T06:30:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "E"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 122
verified: true
draft: false
---

[CF 102185E - \u0421\u043a\u0440\u0449\u043d\u044f](https://codeforces.com/problemset/problem/102185/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một từ điển gồm (N) từ đầy đủ. Sau đó chúng ta nhận được (M) từ từ một văn bản. Một từ văn bản được coi là viết tắt nếu nó có thể thu được bằng cách xóa các ký tự khỏi chính xác một từ trong từ điển, trong khi không có từ điển nào khác chứa nó dưới dạng một chuỗi con. Nếu chính xác một từ trong từ điển chứa nó, chúng tôi sẽ thay thế từ văn bản bằng từ trong từ điển đó. Nếu không hoặc có ít nhất hai từ trong từ điển chứa nó thì từ văn bản sẽ không thay đổi. 

Do đó, hoạt động trung tâm là một thử nghiệm tuần tự. Ví dụ,`sti`là một dãy con của`strtoint`, vì chúng ta có thể chọn`s`, sau đó`t`, sau đó`i`. Mặt khác,`aa`là dãy con của cả hai`aba`Và`ababa`, vì vậy nó mơ hồ và không được thay thế. 

Trong từ điển chỉ có (500) từ nhưng tổng chiều dài của chúng có thể lên tới (2\cdot10^6). Văn bản chứa tối đa (2000) từ và mỗi từ trong văn bản có độ dài tối đa (10). Độ dài truy vấn ngắn là hạn chế chính. Điều đó có nghĩa là khi chúng ta có thể nhanh chóng tìm thấy lần xuất hiện tiếp theo của ký tự được yêu cầu trong một từ trong từ điển, thì việc kiểm tra một từ văn bản chỉ cần một số thao tác. 

Quét trực tiếp thì quá tốn kém. Nếu từ điển có tổng chiều dài (2\cdot10^6), việc kiểm tra một từ văn bản với mọi từ trong từ điển có thể kiểm tra tất cả (2\cdot10^6) ký tự trong từ điển. Với (2000) từ văn bản riêng biệt, có thể đạt được kiểm tra ký tự (4\cdot10^9), vượt xa giới hạn 1,5 giây. 

Có một số trường hợp đặc biệt trong đó việc triển khai hợp lý có thể thất bại. Đầu tiên là sự mơ hồ. Coi như```
2
aba
ababa
1
aa
```Cả hai từ điển đều chứa`aa`như một chuỗi con, vì vậy đầu ra đúng là```
aa
```Việc triển khai bất cẩn dừng lại ở từ trong từ điển trùng khớp đầu tiên sẽ in sai`aba`. 

Trường hợp thứ hai là một từ điển chính xác. Coi như```
1
abc
1
abc
```Đầu ra đúng là```
abc
```Định nghĩa cho phép xóa các ký tự bằng 0, do đó, một từ trong từ điển tự nó là một chuỗi con của mục từ điển của nó. Việc triển khai yêu cầu ít nhất một ký tự bị xóa sẽ mắc lỗi này. 

Trường hợp thứ ba là từ viết tắt có thể xuất hiện trong từ điển ở các vị trí không liên tiếp. Ví dụ,```
1
strtoint
1
sti
```sản xuất```
strtoint
```các nhân vật`s`,`t`, Và`i`không cần phải tạo thành một chuỗi con liền kề. Chỉ tìm kiếm chuỗi con sẽ rời đi không chính xác`sti`không thay đổi. 

Trường hợp tinh vi cuối cùng là các mục từ điển trùng lặp. Vì```
2
abc
abc
1
abc
```câu trả lời là```
abc
```bởi vì từ văn bản được chứa trong hai mục từ điển khác nhau, mặc dù các mục đó có nội dung giống nhau. Chúng ta phải đếm các vị trí từ điển chứ không chỉ đơn thuần là các chuỗi từ điển riêng biệt. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi từ trong văn bản, hãy quét từng từ trong từ điển và thực hiện kiểm tra trình tự con trỏ hai con trỏ tiêu chuẩn. Việc kiểm tra giữ một con trỏ vào từ văn bản ngắn và nâng cao nó bất cứ khi nào ký tự từ điển hiện tại khớp với ký tự được yêu cầu tiếp theo. Khi tất cả các ký tự đã được tìm thấy, từ trong từ điển sẽ khớp. 

Điều này đúng vì việc kiểm tra trình tự tham lam luôn tìm ra vị trí sớm nhất có thể cho mọi ký tự được yêu cầu. Nếu thủ tục tham lam không thể tìm thấy ký tự tiếp theo thì không có lựa chọn nào sau đó có thể làm cho truy vấn phù hợp. 

Vấn đề là số lượng công việc lặp đi lặp lại. Giả sử tổng chiều dài từ điển là (D). Một truy vấn có thể yêu cầu kiểm tra ký tự (O(D)). Với (U) các từ văn bản riêng biệt, (U\le2000), trường hợp xấu nhất là kiểm tra ký tự (O(UD)=O(4\cdot10^9)). Thực tế là mỗi truy vấn riêng lẻ có độ dài tối đa (10) không giúp ích gì cho quá trình quét mạnh mẽ, bởi vì tổng số từ trong từ điển vẫn có thể có hàng triệu ký tự. 

Quan sát hữu ích là truy vấn ngắn, trong khi cùng một từ trong từ điển được truy vấn nhiều lần. Thay vì quét một từ trong từ điển ngay từ đầu cho mỗi truy vấn, hãy xử lý trước vị trí mỗi ký tự xuất hiện trong từ đó. 

Đối với một từ trong từ điển, hãy lưu trữ danh sách các vị trí được sắp xếp cho mỗi chữ cái. Để tìm sự xuất hiện tiếp theo của ký tự`c`sau vị trí`p`, tìm kiếm nhị phân danh sách vị trí của nó. Nếu vị trí tiếp theo tồn tại, hãy tiếp tục kiểm tra trình tự tiếp theo từ đó. Vì một truy vấn có tối đa (10) ký tự nên một từ trong từ điển yêu cầu tối đa (10) tìm kiếm nhị phân. 

Điều này thay đổi phần lặp lại từ quét toàn bộ từ trong từ điển sang thực hiện tối đa mười tìm kiếm logarit. Quá trình tiền xử lý là tuyến tính trong tổng chiều dài từ điển. 

Có một sự tối ưu hóa hữu ích khác. Văn bản có thể chứa cùng một từ nhiều lần và câu trả lời của nó luôn giống nhau. Trước tiên, chúng tôi thu thập các từ văn bản riêng biệt, giải từng từ một lần rồi sử dụng lại kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(UD)) | (O(D)) | Quá chậm | 
| Tối ưu | (O(D+UNK\log L)) | (O(D+UN)) | Đã chấp nhận | 

Ở đây (D) là tổng chiều dài từ điển, (U\le2000) là số lượng từ văn bản riêng biệt, (N\le500) là kích thước từ điển, (K\le10) là độ dài truy vấn tối đa và (L) là độ dài từ trong từ điển tối đa. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các từ trong từ điển và xây dựng danh sách vị trí cho từng ký tự của mỗi từ. Đối với một từ trong từ điển như`abac`, danh sách vị trí bao gồm`a: [0, 2]`,`b: [1]`, Và`c: [3]`. Danh sách được sắp xếp tự nhiên vì từ được quét từ trái sang phải. 
2. Đọc văn bản và chỉ giữ lại những từ riêng biệt của nó. Mỗi lần xuất hiện của cùng một từ văn bản đều có cùng một tập hợp các mục từ điển phù hợp, vì vậy việc giải quyết nó nhiều lần sẽ chỉ lãng phí thời gian. 
3. Đối với một truy vấn và một từ trong từ điển, hãy đặt vị trí hiện tại trước phần đầu của từ trong từ điển. Xử lý các ký tự truy vấn từ trái sang phải. 
4. Đối với ký tự truy vấn hiện tại, tìm kiếm nhị phân danh sách vị trí của nó để tìm vị trí đầu tiên lớn hơn vị trí hiện tại. Đây chính xác là ký tự tiếp theo có thể được sử dụng trong một chuỗi con. 
5. Nếu vị trí đó không tồn tại thì truy vấn không phải là một dãy con của từ trong từ điển này. Hãy ngừng kiểm tra từ điển này ngay lập tức. 
6. Nếu tìm thấy mọi ký tự truy vấn, từ trong từ điển sẽ khớp. Tăng số lượng mục từ điển phù hợp và ghi nhớ từ tương ứng. 
7. Dừng lại sau khi tìm thấy hai kết quả phù hợp. Danh tính chính xác của kết quả trùng khớp thứ hai không còn quan trọng nữa vì truy vấn không rõ ràng và phải không thay đổi. 
8. Nếu khớp chính xác một từ trong từ điển, hãy thay thế truy vấn bằng từ đó. Nếu không hoặc ít nhất hai kết quả trùng khớp, hãy giữ nguyên truy vấn ban đầu. Lưu trữ kết quả này để sử dụng lại khi từ văn bản tương tự xuất hiện sau này. 
9. In văn bản đã chuyển đổi theo thứ tự ban đầu. Bảo đảm thay thế giới hạn tổng kích thước đầu ra bằng (2\cdot10^6), do đó, việc xây dựng đầu ra dưới dạng danh sách các chuỗi là an toàn. 

Tại sao nó hoạt động: đối với mỗi từ trong từ điển, vị trí được chọn cho mỗi ký tự truy vấn là vị trí xuất hiện sớm nhất có thể sau vị trí đã chọn trước đó. Việc chọn một lần xuất hiện sớm hơn không bao giờ có thể làm cho truy vấn còn lại khó khớp hơn, vì vậy quy trình tham lam thành công chính xác khi truy vấn là một chuỗi con. Chúng tôi kiểm tra mọi mục từ điển, vì vậy số lượng khớp chính xác là số mục từ điển có chứa truy vấn. Do đó, một truy vấn được thay thế chính xác khi số lượng đó là một. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    n = int(input())

    dictionary = []
    positions = []

    for _ in range(n):
        word = input().strip()
        dictionary.append(word)

        pos = [[] for _ in range(26)]
        for i, ch in enumerate(word):
            pos[ord(ch) - 97].append(i)
        positions.append(pos)

    m = int(input())
    text = [input().strip() for _ in range(m)]

    cache = {}

    for query in set(text):
        matches = 0
        replacement = None

        for idx in range(n):
            pos = positions[idx]
            current = -1
            ok = True

            for ch in query:
                arr = pos[ord(ch) - 97]
                j = bisect_right(arr, current)

                if j == len(arr):
                    ok = False
                    break

                current = arr[j]

            if ok:
                matches += 1
                replacement = dictionary[idx]

                if matches == 2:
                    break

        if matches == 1:
            cache[query] = replacement
        else:
            cache[query] = query

    sys.stdout.write("\n".join(cache[word] for word in text))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên đọc từ điển và xây dựng chỉ mục vị trí. Chỉ mục được lưu trữ riêng cho từng từ trong từ điển vì các vị trí chỉ có ý nghĩa bên trong từ đó. 

Trong một truy vấn,`current`là vị trí của ký tự cuối cùng đã được chọn.`bisect_right(arr, current)`trả về lần xuất hiện đầu tiên ngay sau nó. Việc theo sau là cần thiết vì một vị trí trong từ điển không thể được sử dụng hai lần cho hai ký tự truy vấn khác nhau. 

Mã dừng lại sau trận đấu thứ hai. Điều này là an toàn vì đầu ra chỉ phụ thuộc vào số lượng kết quả khớp bằng 0, một hay ít nhất là hai. 

các`cache`từ điển xử lý các từ văn bản lặp lại. biểu hiện`cache[word]`tại thời điểm đầu ra vẫn giữ nguyên thứ tự văn bản gốc mặc dù các truy vấn riêng biệt được xử lý theo thứ tự được đặt tùy ý. 

Không có vấn đề tràn số nguyên trong Python và đầu ra lớn nhất bị giới hạn bởi câu lệnh. Chi phí bộ nhớ chính là các vị trí xuất hiện được lưu trữ, có tổng số chính xác bằng tổng chiều dài từ điển. 

## Ví dụ đã hoạt động 

Mẫu được cung cấp thể hiện cả chữ viết tắt duy nhất và mơ hồ. 

Vì`sti`, chỉ một`strtoint`chứa ba ký tự theo thứ tự. Vì`aa`, cả hai`aba`Và`ababa`chứa nó, vì vậy nó không thay đổi. Truy vấn`aaa`được chứa trong`ababa`nhưng không ở trong`aba`, làm`ababa`mở rộng độc đáo của nó. 

| Truy vấn | Từ điển | Vị trí đã chọn | Cuộc thi đấu? | Số trận đấu | 
| --- | --- | --- | --- | --- | 
|`sti`|`abc`|`s`vắng mặt | Không | 0 | 
|`sti`|`strtoint`|`s=0, t=1, i=4`| Có | 1 | 
|`sti`|`aba`|`s`vắng mặt | Không | 1 | 
|`sti`|`ababa`|`s`vắng mặt | Không | 1 | 
|`aa`|`abc`|`a=0`, thứ hai`a`vắng mặt | Không | 0 | 
|`aa`|`strtoint`|`a`vắng mặt | Không | 0 | 
|`aa`|`aba`|`a=0, a=2`| Có | 1 | 
|`aa`|`ababa`|`a=0, a=2`| Có | 2 | 
|`aaa`|`aba`| chỉ có hai`a`nhân vật | Không | 0 | 
|`aaa`|`ababa`|`a=0, a=2, a=4`| Có | 1 | 
|`bb`| tất cả các từ điển | ít hơn hai`b`nhân vật | Không | 0 | 
|`abc`|`abc`|`a=0, b=1, c=2`| Có | 1 | 

Dấu vết hiển thị bất biến khóa: sau mỗi ký tự truy vấn,`current`là vị trí sớm nhất có thể mà tiền tố được xử lý cho đến nay có thể kết thúc. Điều đó làm cho mọi tìm kiếm nhị phân sau này trở nên dễ dãi nhất có thể. 

Ví dụ thứ hai thể hiện sự mơ hồ và kết hợp từ điển chính xác:```
3
abc
abc
axbyc
4
abc
aby
ac
zzz
```| Truy vấn | Từ điển | Kết quả kiểm tra trình tự tiếp theo | Trận đấu | 
| --- | --- | --- | --- | 
|`abc`| Đầu tiên`abc`| Có | 1 | 
|`abc`| thứ hai`abc`| Có | 2 | 
|`abc`|`axbyc`| Có | 2 | 
|`aby`| Đầu tiên`abc`| Không | 0 | 
|`aby`| thứ hai`abc`| Không | 0 | 
|`aby`|`axbyc`| Có | 1 | 
|`ac`| Đầu tiên`abc`| Có | 1 | 
|`ac`| thứ hai`abc`| Có | 2 | 
|`zzz`| Đầu tiên`abc`| Không | 0 | 

Kết quả đầu ra là```
abc
axbyc
ac
zzz
```Truy vấn đầu tiên không thay đổi vì có ba mục từ điển chứa nó. Truy vấn thứ hai có chính xác một kết quả phù hợp, vì vậy nó mở rộng thành`axbyc`. Truy vấn thứ ba không rõ ràng vì cả hai bản sao của`abc`được tính là các mục từ điển riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(D+UNK\log L)) | Chi phí xây dựng vị trí (O(D)); mỗi truy vấn riêng biệt (U) kiểm tra tối đa (N) từ và tối đa (K\le10) ký tự trên mỗi từ | 
| Không gian | (O(D+UN+M)) | Vị trí ký tự chứa (D) mục nhập, trong khi bộ đệm truy vấn và văn bản đầu vào chứa tối đa (O(UN+M)) tham chiếu bổ sung | 

Với (D\le2\cdot10^6), (U\le2000), (N\le500) và (K\le10), phần lặp lại đắt tiền thực hiện tối đa (10^7) thao tác tìm kiếm nhị phân, thay vì hàng tỷ lần quét ký tự từ điển. Bản thân các tìm kiếm nhị phân chạy trong C thông qua Python`bisect`mô-đun, làm cho công thức này trở nên thực tế trong giới hạn nhất định. Chỉ mục vị trí sử dụng bộ nhớ tuyến tính trong tổng kích thước từ điển, thoải mái trong khoảng 512 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    n = int(input())

    dictionary = []
    positions = []

    for _ in range(n):
        word = input().strip()
        dictionary.append(word)

        pos = [[] for _ in range(26)]
        for i, ch in enumerate(word):
            pos[ord(ch) - 97].append(i)
        positions.append(pos)

    m = int(input())
    text = [input().strip() for _ in range(m)]

    cache = {}

    for query in set(text):
        matches = 0
        replacement = None

        for idx in range(n):
            current = -1
            ok = True
            pos = positions[idx]

            for ch in query:
                arr = pos[ord(ch) - 97]
                j = bisect_right(arr, current)

                if j == len(arr):
                    ok = False
                    break

                current = arr[j]

            if ok:
                matches += 1
                replacement = dictionary[idx]
                if matches == 2:
                    break

        cache[query] = replacement if matches == 1 else query

    return "\n".join(cache[word] for word in text)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
4
abc
strtoint
aba
ababa
5
sti
aa
aaa
bb
abc
"""

assert run(sample1) == """\
strtoint
aa
ababa
ababa
abc
""", "sample 1"

sample2 = """\
3
abc
abc
axbyc
4
abc
aby
ac
zzz
"""

assert run(sample2) == """\
abc
axbyc
ac
zzz
""", "duplicate dictionary entries"

sample3 = """\
1
a
4
a
aa
b
a
"""

assert run(sample3) == """\
a
aa
b
a
""", "minimum dictionary size"

sample4 = """\
4
aaaaaaaaaa
bbbbbbbbbb
ababababab
baaaaaaaaa
6
aaaaaaaaaa
abab
bbbb
baaa
ab
zzzz
"""

assert run(sample4) == """\
aaaaaaaaaa
abab
bbbbbbbbbb
baaa
ababababab
zzzz
""", "boundary query length and subsequences"

sample5 = """\
500
""" + "\n".join(["a" * 4000 for _ in range(500)]) + """
4
a
aaaaaaaaaa
b
a
"""

# Every dictionary word is identical, so every nonempty sequence of a's
# is ambiguous. The b query matches none.
assert run(sample5) == """\
a
aaaaaaaaaa
b
a
""", "large dictionary and repeated values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`strtoint`,`aa`,`ababa`,`ababa`,`abc`| Hành vi vấn đề ban đầu, các chuỗi con duy nhất và mơ hồ | 
| Mẫu 2 |`abc`,`axbyc`,`ac`,`zzz`| Các mục từ điển trùng lặp và khớp không liền kề | 
| Mẫu 3 |`a`,`aa`,`b`,`a`| Kích thước từ điển tối thiểu và xử lý khớp chính xác | 
| Mẫu 4 |`aaaaaaaaaa`,`abab`,`bbbbbbbbbb`,`baaa`,`ababababab`,`zzzz`| Truy vấn mười ký tự, sắp xếp các vị trí đã chọn và các ký tự vắng mặt | 
| Mẫu 5 |`a`,`aaaaaaaaaa`,`b`,`a`| Kích thước từ điển lớn, giá trị từ điển lặp lại bằng nhau và sự mơ hồ | 

## Vỏ cạnh 

Trường hợp mơ hồ từ việc hiểu vấn đề được xử lý bằng cách đếm các mục từ điển thay vì các chuỗi riêng biệt. Vì```
2
aba
ababa
1
aa
```từ đầu tiên khớp với nhau nên số đếm trở thành một. Từ thứ hai cũng trùng khớp nên số đếm trở thành hai và thuật toán dừng lại. Vì số đếm không phải là một nên đầu ra là`aa`. 

Trường hợp khớp chính xác```
1
abc
1
abc
```bắt đầu bằng`current = -1`. Việc tìm kiếm chọn vị trí (0), (1) và (2) nên cả ba ký tự đều được chấp nhận. Số lượng trận đấu là một và sự thay thế được lưu trữ là`abc`, cho kết quả đầu ra không thay đổi chính xác. 

Trường hợp không liền kề```
1
strtoint
1
sti
```chọn vị trí của`s`, sau đó tìm kiếm nghiêm ngặt sau nó để tìm`t`, thì đúng theo vị trí đó cho`i`. Các vị trí được chọn tạo thành một dãy con hợp lệ mặc dù chúng không liền kề nhau, do đó kết quả là`strtoint`. 

Các mục từ điển trùng lặp được tính hai lần một cách có chủ ý. Vì```
2
abc
abc
1
abc
```mục đầu tiên cho một kết quả phù hợp và mục thứ hai cho hai kết quả. Thuật toán dừng ngay sau lần so khớp thứ hai và trả về truy vấn ban đầu. Đây chính xác là những gì định nghĩa yêu cầu vì hai vị trí từ điển đại diện cho hai mục nhập khác nhau. 

Truy vấn chứa một ký tự không có trong từ điển sẽ thất bại ngay lập tức. Ví dụ, với từ điển`abc`và truy vấn`zzz`, tìm kiếm đầu tiên sẽ xem xét danh sách vị trí cho`z`, trống rỗng. Từ bị từ chối mà không kiểm tra các ký tự truy vấn còn lại. Lỗi sớm này cũng hữu ích cho hiệu suất của các truy vấn phủ định.
