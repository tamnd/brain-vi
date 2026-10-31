---
title: "CF 106310C - \u041c\u0430\u0433\u0438\u044f \u0441\u043b\u043e\u0432"
description: "Nhiệm vụ đưa ra ba từ tiếng Anh viết thường. Eliza coi một cụm từ là kỳ diệu nếu ba từ đó hoàn toàn khác nhau và cụm từ đó chứa một hoặc hai từ đặc biệt xem, tin và lặp lại, nhưng không phải cả ba từ đó cùng một lúc."
date: "2026-06-25T07:46:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106310
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435, 7-8 \u043a\u043b\u0430\u0441\u0441\u044b, \u041f\u0435\u0440\u043c\u0441\u043a\u0438\u0439 \u043a\u0440\u0430\u0439, 2025"
rating: 0
weight: 106310
solve_time_s: 31
verified: true
draft: false
---

[CF 106310C - \u041c\u0430\u0433\u0438\u044f \u0441\u043b\u043e\u0432](https://codeforces.com/problemset/problem/106310/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 31s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ đưa ra ba từ tiếng Anh viết thường. Eliza coi một cụm từ là kỳ diệu nếu ba từ đó hoàn toàn khác nhau và cụm từ đó chứa một hoặc hai từ đặc biệt`see`,`believe`, Và`repeat`, nhưng không phải cả ba điều đó cùng một lúc. Chúng ta cần quyết định xem ba từ đã cho có thỏa mãn các quy tắc này hay không và in ra`YES`hoặc`NO`. 

Kích thước đầu vào được cố tình nhỏ: có chính xác ba chuỗi và mỗi chuỗi có độ dài tối đa là 15. Điều này có nghĩa là ngay cả việc so sánh trực tiếp với ba từ ma thuật cũng là quá đủ nhanh. Không cần cấu trúc dữ liệu nâng cao hoặc tối ưu hóa. Một giải pháp kiểm tra từng từ và thực hiện khối lượng công việc liên tục sẽ có hiệu quả ngay lập tức. 

Khó khăn chính không phải là hiệu suất mà là diễn giải chính xác các điều kiện. Cụm từ yêu cầu ba từ khác nhau, vì vậy các từ nhập trùng lặp phải bị từ chối ngay cả khi từ trùng lặp đó là phép thuật. Đồng thời, số lượng từ thần kỳ trong ba đầu vào ít nhất phải là một và nhiều nhất là hai. 

Hãy xem xét đầu vào:```
see
see
cat
```Đầu ra đúng là:```
NO
```Một giải pháp bất cẩn chỉ đếm xem có bao nhiêu từ thần kỳ xuất hiện có thể được tìm thấy`see`hai lần và chấp nhận cụm từ không chính xác. Bản thân các từ phải khác biệt. 

Một trường hợp cạnh khác là:```
see
believe
repeat
```Đầu ra đúng là:```
NO
```Cả ba từ đặc biệt đều có mặt, nhưng các quy tắc rõ ràng cấm sử dụng cả ba từ này. 

Trường hợp cuối cùng là:```
see
cat
dog
```Đầu ra đúng là:```
YES
```Chỉ một trong những từ ma thuật bắt buộc xuất hiện, được cho phép. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là so sánh ba từ đầu vào với ba từ đặc biệt và đếm xem có bao nhiêu từ trong số đó thuộc về bộ phép thuật. Chúng tôi cũng so sánh các từ với nhau để xác minh rằng cả ba đều khác nhau. Vì chỉ có ba từ nên cách tiếp cận vũ phu có thể đơn giản thử trực tiếp tất cả các điều kiện. 

Phiên bản bạo lực đã là giải pháp thực tế tối ưu ở đây. Nếu chúng ta mô tả công việc dưới dạng các thao tác, thì chỉ có một số lượng so sánh chuỗi không đổi, nhiều nhất là vài chục ký tự kiểm tra vì mỗi từ có độ dài tối đa là 15. Ngay cả một cách tiếp cận toàn diện đối với các kiểm tra có thể cũng sẽ chỉ thực hiện vài trăm thao tác nguyên thủy. 

Quan sát quan trọng là vấn đề không có không gian tìm kiếm ẩn. Chúng tôi không xây dựng một cụm từ hoặc khám phá sự kết hợp của các từ. Đầu vào đã cung cấp toàn bộ cụm từ, vì vậy câu trả lời được xác định bởi hai thuộc tính: tính duy nhất của ba từ và số lượng từ ma thuật trong số đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba từ thành một mảng. Việc giữ chúng cùng nhau giúp dễ dàng kiểm tra tính duy nhất và tư cách thành viên trong bộ phép thuật. 
2. Kiểm tra xem ba từ có khác nhau theo từng cặp hay không. Nếu bất kỳ hai vị trí nào chứa cùng một từ thì cụm từ đó không thể có phép thuật, vì vậy câu trả lời là`NO`. 
3. Đếm xem trong ba từ có bao nhiêu từ bằng một trong`see`,`believe`, hoặc`repeat`. Số lượng này đại diện cho số lượng từ kỳ diệu được sử dụng trong cụm từ. 
4. Nếu số đếm chính xác là một hoặc chính xác là hai, hãy xuất ra`YES`. Ngược lại, xuất ra`NO`. 

Lý do điều này có tác dụng là vì các quy tắc ban đầu có thể được rút gọn hoàn toàn thành hai bước kiểm tra này. Lần kiểm tra đầu tiên cho thấy cụm từ đó có ba từ khác nhau. Lần kiểm tra thứ hai đảm bảo rằng số lượng từ đặc biệt nằm trong phạm vi cho phép. 

Tại sao nó hoạt động: thuật toán kiểm tra mọi quy tắc xác định một cụm từ kỳ diệu. Nếu chấp nhận thì các từ sẽ khác nhau và số lượng từ đặc biệt không phải là 0 hay 3 nên mọi yêu cầu đều được thỏa mãn. Nếu nó từ chối, ít nhất một quy tắc bị vi phạm, nghĩa là cụm từ đó không thể có phép thuật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    words = [input().strip() for _ in range(3)]

    if len(set(words)) != 3:
        print("NO")
        return

    magic = {"see", "believe", "repeat"}
    count = sum(1 for word in words if word in magic)

    if 1 <= count <= 2:
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Dữ liệu đầu vào được đọc chính xác là ba dòng riêng biệt vì bài toán luôn cung cấp ba từ. các`set`chuyển đổi sẽ loại bỏ các từ trùng lặp, do đó so sánh kích thước của nó với ba lần trực tiếp sẽ kiểm tra xem tất cả các từ có phải là duy nhất hay không. 

Các từ thần kỳ được lưu trữ trong một bộ vì việc kiểm tra tư cách thành viên là hoạt động tự nhiên ở đây. Sau đó, chương trình sẽ đếm xem có bao nhiêu trong số ba đầu vào thuộc về bộ đó. 

Điều kiện cuối cùng chỉ chấp nhận số lượng một hoặc hai. Việc sử dụng kiểm tra phạm vi sẽ tránh việc vô tình chấp nhận 0 từ ma thuật hoặc trường hợp bị cấm xuất hiện cả ba từ ma thuật. 

Không có thao tác lập chỉ mục hoặc vòng lặp tùy thuộc vào kích thước đầu vào, do đó không có vấn đề về ranh giới. Tràn số nguyên cũng không thể thực hiện được vì số nguyên duy nhất được lưu trữ là số đếm từ 0 đến 3. 

## Ví dụ đã hoạt động 

Ví dụ 1: 

đầu vào:```
see
cat
dog
```| Bước | Từ | Kiểm tra duy nhất | Số lượng huyền diệu | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | xem, mèo, chó | Ba từ khác nhau | 0 | Tiếp tục | 
| Đếm | xem, mèo, chó | Đã vượt qua | 1 | CÓ | 

Ví dụ này cho thấy tình huống được chấp nhận nhỏ nhất trong đó chính xác một từ thần kỳ được sử dụng. 

Ví dụ 2: 

đầu vào:```
see
believe
repeat
```| Bước | Từ | Kiểm tra duy nhất | Số lượng huyền diệu | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | thấy, tin, lặp lại | Ba từ khác nhau | 0 | Tiếp tục | 
| Đếm | thấy, tin, lặp lại | Đã vượt qua | 3 | KHÔNG | 

Ví dụ này chứng minh tại sao việc đếm các từ thần kỳ là cần thiết. Không được phép có cả ba từ đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có ba từ được xử lý, mỗi từ có độ dài giới hạn. | 
| Không gian | O(1) | Chương trình chỉ lưu trữ ba từ và một bộ ba từ ma thuật cố định. | 

Các ràng buộc đủ nhỏ để giải pháp sử dụng ít hơn nhiều so với giới hạn thời gian và bộ nhớ có sẵn. Cách tiếp cận này cũng sẽ vẫn hiệu quả nếu số lượng cụm từ được thử nghiệm lớn vì mỗi cụm từ riêng lẻ chỉ yêu cầu công việc liên tục. 

## Trường hợp thử nghiệm```python
import sys
import io

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    words = [sys.stdin.readline().strip() for _ in range(3)]

    if len(set(words)) != 3:
        ans = "NO"
    else:
        magic = {"see", "believe", "repeat"}
        count = sum(1 for word in words if word in magic)
        ans = "YES" if 1 <= count <= 2 else "NO"

    sys.stdin = old_stdin
    return ans + "\n"

assert solution("see\ncat\ndog\n") == "YES\n", "one magical word"
assert solution("see\nbelieve\nrepeat\n") == "NO\n", "all magical words"
assert solution("see\nsee\ncat\n") == "NO\n", "duplicate words"
assert solution("hello\nworld\ncat\n") == "NO\n", "no magical words"
assert solution("repeat\nabc\nbelieve\n") == "YES\n", "two magical words"
assert solution("a\nb\nc\n") == "NO\n", "minimum length ordinary words"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`see, cat, dog`|`YES`| Số từ ma thuật tối thiểu được phép | 
|`see, believe, repeat`|`NO`| Vụ án cấm có đủ ba chữ thần kỳ | 
|`see, see, cat`|`NO`| Phát hiện từ trùng lặp | 
|`hello, world, cat`|`NO`| Từ chối khi không có lời thần kỳ nào xuất hiện | 
|`repeat, abc, believe`|`YES`| Chấp nhận bằng hai chữ thần kỳ | 
|`a, b, c`|`NO`| Xử lý từ ngắn thông thường | 

## Vỏ cạnh 

Đối với các từ trùng lặp, đầu vào```
see
see
cat
```bị từ chối ngay lập tức. Tập hợp các từ có kích thước hai thay vì ba, vì vậy thuật toán không bao giờ đạt đến mức kiểm tra số từ kỳ diệu. Điều này ngăn chặn các từ ma thuật trùng lặp bị nhầm lẫn với một cụm từ hợp lệ. 

Đối với trường hợp có cả ba từ thần kỳ:```
see
believe
repeat
```điều kiện duy nhất thành công vì các từ khác nhau. Số lượng phép thuật trở thành ba và điều kiện cuối cùng từ chối nó vì chỉ cho phép một hoặc hai từ phép thuật. 

Đối với một cụm từ chứa chính xác một từ huyền diệu:```
see
apple
tree
```điều kiện duy nhất thành công và số lượng phép thuật là một. Thuật toán chấp nhận nó vì một từ thần kỳ là đủ. 

Đối với một cụm từ không có từ huyền diệu:```
apple
tree
stone
```điều kiện duy nhất thành công, nhưng số đếm bằng 0. Thuật toán từ chối nó vì mọi cụm từ hợp lệ phải chứa ít nhất một trong ba từ đặc biệt.
