---
title: "CF 104435B - Sùng bái Wah!"
description: "Mỗi trường hợp thử nghiệm chứa hai tập hợp các từ viết thường. Bộ sưu tập đầu tiên là Wah-List, chứa mọi từ phải xuất hiện sau khi tin nhắn được mã hóa được giải mã. Bộ sưu tập thứ hai chính là tin nhắn được mã hóa."
date: "2026-06-30T18:41:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104435
codeforces_index: "B"
codeforces_contest_name: "2023 UP ACM Algolympics Final Round"
rating: 0
weight: 104435
solve_time_s: 59
verified: true
draft: false
---

[CF 104435B - Giáo phái Wah!](https://codeforces.com/problemset/problem/104435/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm chứa hai tập hợp các từ viết thường. 

Bộ sưu tập đầu tiên là Wah-List, chứa mọi từ phải xuất hiện sau khi tin nhắn được mã hóa được giải mã. 

Bộ sưu tập thứ hai chính là tin nhắn được mã hóa. Mọi từ đều được mã hóa bằng cùng một dịch chuyển mật mã Caesar. Trong quá trình mã hóa, mỗi chữ cái đều được chuyển tiếp bởi`k`vị trí trong bảng chữ cái, bao quanh sau`z`. Để giải mã, chúng ta phải dịch chuyển từng chữ cái về phía sau một lượng như nhau. 

Nhiệm vụ của chúng ta là tìm giá trị dịch chuyển **dương** nhỏ nhất`k`sao cho sau khi giải mã toàn bộ tin nhắn, mọi từ trong Danh sách Wah sẽ xuất hiện ở đâu đó trong tin nhắn được giải mã. Nếu không có sự dịch chuyển dương thỏa mãn điều kiện này, chúng ta xuất ra`-1`. 

Bảng chữ cái chỉ có 26 chữ cái nên chỉ có 26 ca Caesar riêng biệt. Vì bài toán chỉ cho phép giá trị dương của`k`, chúng ta chỉ cần kiểm tra sự dịch chuyển từ`1`bởi vì`25`. Một sự thay đổi`26`sẽ tạo lại tin nhắn gốc và tương đương với`0`, vì vậy nó không bao giờ có thể là câu trả lời tích cực nhỏ nhất. 

Độ dài tin nhắn tối đa là 1500 ký tự và Danh sách Wah chứa tối đa 20 từ có độ dài tối đa là 50. Ngay cả việc giải mã toàn bộ tin nhắn cho mỗi lần dịch chuyển có thể cũng chỉ cần khoảng`25 × 1500 = 37500`các thao tác ký tự trên mỗi trường hợp thử nghiệm, khá nhỏ. 

Một trường hợp tinh tế là khi một số ca tạo ra các thông điệp được giải mã hợp lệ. Ví dụ,```
Wah-List:
aaa

Message:
bbb ccc ddd
```ca`1`,`2`, Và`3`tất cả tạo ra một thông điệp có chứa`aaa`. Câu trả lời đúng là`1`bởi vì chúng ta phải trả về sự dịch chuyển dương nhỏ nhất, không phải bất kỳ sự dịch chuyển hợp lệ nào. 

Một sai lầm dễ mắc phải khác là quên rằng sự thay đổi bao quanh bảng chữ cái.```
Wah-List:
xyz

Message:
abc
```Giải mã bằng`k = 3`lần lượt`abc`vào trong`xyz`. Chỉ cần trừ ba từ mỗi ký tự mà không bao bọc sẽ tạo ra các chữ cái không hợp lệ. 

Cạm bẫy thứ ba là xử lý không chính xác các từ trong thông báo trùng lặp. Coi như```
Wah-List:
cat dog

Message:
fdw fdw grj
```Sau khi giải mã với sự dịch chuyển chính xác, tin nhắn sẽ trở thành```
cat cat dog
```Các bản sao không quan trọng. Chúng ta chỉ cần biết mỗi từ được yêu cầu có xuất hiện ít nhất một lần hay không, do đó việc lưu trữ các từ được giải mã thành một bộ là đủ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi sự thay đổi tích cực có thể. Đối với mỗi ca, hãy giải mã từng từ trong tin nhắn, thu thập các từ đã giải mã thành một bộ và kiểm tra xem mọi từ Wah-List có thuộc bộ đó hay không. Vì chỉ có 25 ca ứng viên nên cách tiếp cận này cực kỳ hiệu quả. 

Giả sử chúng ta bỏ qua thực tế là bảng chữ cái chỉ có 26 chữ cái và thay vào đó tìm kiếm trên các giá trị lớn tùy ý của`k`. Điều đó rõ ràng là không thể bởi vì có vô số sự thay đổi tồn tại. Quan sát quan trọng là các phép dịch mã Caesar lặp lại sau mỗi 26 vị trí. Dịch chuyển theo 27 hoàn toàn giống như dịch chuyển theo 1, dịch chuyển theo 52 giống như dịch chuyển theo 0, v.v. Tính tuần hoàn này làm giảm không gian tìm kiếm xuống chỉ còn 25 ứng viên có ý nghĩa. 

Khi chúng tôi nhận ra điều này, không cần đến các thuật toán phức tạp hơn như băm, thử hoặc khớp chuỗi. Việc giải mã mọi tin nhắn ứng cử viên đã nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua những ca làm việc tùy ý | Không giới hạn | O(1) | Không thể | 
| Hãy thử tất cả 25 ca Caesar | O(25 × L) | O(M) | Đã chấp nhận | 

Đây,`L`là tổng số ký tự trong tin nhắn được mã hóa và`M`là số từ trong tin nhắn. 

## Hướng dẫn thuật toán 

1. Đọc Wah-List và lưu trữ các từ trong đó theo một bộ. Việc sử dụng một bộ giúp việc kiểm tra tư cách thành viên sau này diễn ra liên tục. 
2. Đọc tin nhắn được mã hóa dưới dạng danh sách các từ. 
3. Đối với mỗi ca`k`từ`1`ĐẾN`25`, giải mã mọi từ trong thông điệp bằng cách dịch ngược từng ký tự bằng cách`k`vị trí với số học modulo 26. 
4. Chèn từng từ được giải mã vào một bộ. Các từ được giải mã trùng lặp không liên quan vì chúng tôi chỉ quan tâm liệu từ được yêu cầu có xuất hiện ít nhất một lần hay không. 
5. Kiểm tra xem mọi từ trong Wah-List có thuộc tập hợp từ được giải mã hay không. 
6. Ngay khi một ca thỏa mãn điều kiện, hãy xuất ca đó và ngừng tìm kiếm. Vì các ca được kiểm tra theo thứ tự tăng dần nên câu trả lời hợp lệ đầu tiên tự động là câu trả lời dương nhỏ nhất. 
7. Nếu không có ca nào trong số 25 ca thành công, đầu ra`-1`. 

### Tại sao nó hoạt động 

Mọi mật mã Caesar có thể có được xác định duy nhất bởi độ dịch chuyển modulo 26 của nó. Việc kiểm tra dịch chuyển từ`1`bởi vì`25`bao gồm mọi mật mã dương riêng biệt chính xác một lần. Đối với một dịch chuyển cố định, việc giải mã sẽ tái tạo lại chính xác thông điệp đã tồn tại trước khi mã hóa bằng dịch chuyển đó. Thuật toán chấp nhận chính xác khi mọi từ được yêu cầu xuất hiện trong tin nhắn được giải mã đó. Vì các ca được kiểm tra theo thứ tự tăng dần nên ca thành công đầu tiên được đảm bảo là câu trả lời hợp lệ nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def decode_word(word, shift):
    res = []
    for c in word:
        x = (ord(c) - ord('a') - shift) % 26
        res.append(chr(ord('a') + x))
    return "".join(res)

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        wah = set(input().split())

        m = int(input())
        message = input().split()

        answer = -1

        for shift in range(1, 26):
            decoded = set()

            for word in message:
                decoded.add(decode_word(word, shift))

            if wah.issubset(decoded):
                answer = shift
                break

        print(answer)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc lưu trữ Wah-List trong một bộ vì kiểm tra tư cách thành viên là thao tác duy nhất chúng ta cần. Mỗi ca ứng viên đều được xử lý độc lập. 

Hàm trợ giúp thực hiện giải mã Caesar. Mỗi chữ cái được chuyển đổi thành một giá trị từ`0`ĐẾN`25`, phép dịch bị trừ đi, số học modulo tự động xử lý bao quanh và kết quả được chuyển đổi trở lại thành ký tự. 

Đối với mỗi ca, mỗi từ thông báo được giải mã chính xác một lần và được chèn vào một bộ. Việc sử dụng một bộ sẽ bỏ qua các bản sao một cách tự nhiên trong khi cung cấp khả năng tra cứu liên tục. Bài kiểm tra tập hợp con trực tiếp trả lời xem có đủ từ được yêu cầu hay không. 

Vòng lặp dừng ngay sau khi tìm thấy ca hợp lệ đầu tiên. Vì các ca được kiểm tra theo thứ tự tăng dần nên điều này đảm bảo câu trả lời dương tối thiểu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giả sử đầu vào là```
1
2
wah umu
3
zhv zdk xpx
```Sự dịch chuyển đúng là`3`. 

| Thay đổi | Từ được giải mã | Chứa wah? | Chứa umu? | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | từ khác nhau | Không | Không | Không | 
| 2 | từ khác nhau | Không | Không | Không | 
| 3 | vâng wa ừm | Có | Có | Có | 

Việc tìm kiếm dừng ngay sau khi thay đổi`3`, chứng minh tại sao việc kiểm tra các ca theo thứ tự tăng dần sẽ tự động đưa ra câu trả lời hợp lệ nhỏ nhất. 

### Ví dụ 2```
1
1
aaa
3
bbb ccc ddd
```| Thay đổi | Từ được giải mã | Chứa aaa? | hợp lệ | 
| --- | --- | --- | --- | 
| 1 | aaa bbb ccc | Có | Có | 

Mặc dù các dịch chuyển lớn hơn cũng có tác dụng nhưng thuật toán không bao giờ đạt tới chúng vì dịch chuyển`1`đã là câu trả lời hợp lệ nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(25 × L) | Thông báo được giải mã một lần cho mỗi ca trong số 25 ca dương có thể có. | 
| Không gian | O(M) | Bộ từ được giải mã lưu trữ tối đa một bản sao của mỗi từ thông báo được giải mã. | 

Vì tổng độ dài tin nhắn tối đa là 1500 ký tự nên việc giải mã tin nhắn 25 lần chỉ cần vài chục nghìn thao tác ký tự. Điều này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    from contextlib import redirect_stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    def decode_word(word, shift):
        return "".join(chr((ord(c) - 97 - shift) % 26 + 97) for c in word)

    def solve():
        input = sys.stdin.readline
        t = int(input())
        for _ in range(t):
            n = int(input())
            wah = set(input().split())
            m = int(input())
            msg = input().split()

            ans = -1
            for k in range(1, 26):
                dec = {decode_word(w, k) for w in msg}
                if wah.issubset(dec):
                    ans = k
                    break
            print(ans)

    with redirect_stdout(out):
        solve()

    return out.getvalue()

assert run("""1
1
aaa
3
bbb ccc ddd
""") == "1\n"

assert run("""1
1
xyz
1
abc
""") == "3\n"

assert run("""1
1
abc
1
abc
""") == "-1\n"

assert run("""1
2
cat dog
3
fdw fdw grj
""") == "3\n"

assert run("""1
1
a
1
b
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aaa`với`bbb ccc ddd`|`1`| Sự thay đổi hợp lệ nhỏ nhất được chọn. | 
|`xyz`với`abc`|`3`| Việc bao bọc bảng chữ cái được xử lý chính xác. | 
|`abc`với`abc`|`-1`| Sự thay đổi`0`không được phép. | 
| Từ được giải mã trùng lặp |`3`| Tin nhắn trùng lặp không ảnh hưởng đến tính chính xác. | 
| Từ có một ký tự |`1`| Đầu vào kích thước tối thiểu hoạt động chính xác. | 

## Vỏ cạnh 

Hãy xem xét```
1
1
abc
1
abc
```Sự thay đổi duy nhất khiến thông điệp không thay đổi là`0`, nhưng chỉ cho phép dịch chuyển dương. Thuật toán kiểm tra sự dịch chuyển`1`bởi vì`25`, không tìm thấy giải mã hợp lệ và xuất ra chính xác`-1`. 

Bây giờ hãy xem xét```
1
1
xyz
1
abc
```Khi thuật toán đạt đến độ dịch chuyển`3`, mỗi ký tự được giải mã bằng số học modulo 26. từ`abc`trở thành`xyz`, thỏa mãn Wah-List. Điều này xác nhận rằng tính năng bao bọc được triển khai chính xác. 

Cuối cùng, hãy xem xét```
1
2
cat dog
3
fdw fdw grj
```Với sự thay đổi`3`, tin nhắn được giải mã là`cat cat dog`. Tập từ được giải mã trở thành`{cat, dog}`bất chấp sự xuất hiện trùng lặp của`cat`. Thử nghiệm tập hợp con thành công, cho thấy rằng các bản sao trong tin nhắn không bao giờ ảnh hưởng đến tính chính xác.
