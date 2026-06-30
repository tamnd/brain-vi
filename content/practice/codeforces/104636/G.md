---
title: "CF 104636G - Pangram"
description: "Chúng ta được cung cấp một chuỗi các chữ cái Latinh trong đó có thể xuất hiện cả ký tự viết hoa và viết thường. Nhiệm vụ là xác định xem chuỗi đó có phải là một pangram hay không, nghĩa là mọi chữ cái từ 'a' đến 'z' xuất hiện ít nhất một lần ở đâu đó trong chuỗi, bỏ qua các khác biệt về chữ hoa chữ thường."
date: "2026-06-29T17:07:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104636
codeforces_index: "G"
codeforces_contest_name: "\u041c\u0438\u0441\u0438\u0441 2023 \u043e\u0441\u0435\u043d\u044c - \u043c\u0430\u0441\u0441\u0438\u0432\u044b, \u0441\u0442\u0440\u043e\u043a\u0438"
rating: 0
weight: 104636
solve_time_s: 59
verified: true
draft: false
---

[CF 104636G - Pangram](https://codeforces.com/problemset/problem/104636/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái Latinh trong đó có thể xuất hiện cả ký tự viết hoa và viết thường. Nhiệm vụ là xác định xem chuỗi đó có phải là pangram hay không, nghĩa là mọi chữ cái từ`'a'`ĐẾN`'z'`xuất hiện ít nhất một lần ở đâu đó trong chuỗi, bỏ qua sự khác biệt về chữ hoa chữ thường. 

Nói một cách cụ thể hơn, chúng tôi quét một chuỗi ký tự và muốn kiểm tra xem tập hợp các chữ cái riêng biệt hiện tại có bao gồm tất cả 26 chữ cái trong bảng chữ cái tiếng Anh hay không. Trường hợp không thành vấn đề, vì vậy`'A'`Và`'a'`được coi là cùng một biểu tượng cho mục đích kiểm tra mức độ phù hợp. 

Kích thước đầu vào nhỏ, tối đa 100 ký tự. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa ngoài một lần truyền qua chuỗi. Thậm chí một$O(n \cdot 26)$hoặc$O(n^2)$về mặt kỹ thuật sẽ vượt qua, nhưng giải pháp tự nhiên là thời gian tuyến tính với bộ nhớ bổ sung liên tục. 

Các trường hợp đặc biệt chính xuất phát từ cách chuẩn hóa các chữ cái và cách kiểm tra tính đầy đủ. Một sai lầm ngây thơ là coi chữ hoa và chữ thường là các ký tự khác nhau, điều này sẽ đánh dấu các chuỗi không chính xác như`"TheQuickBrownFoxJumpsOverTheLazyDog"`vì không chứa tất cả các chữ cái nếu chữ hoa chữ thường không được chuẩn hóa. Một vấn đề tế nhị khác là giả định không chính xác độ dài chuỗi đảm bảo phạm vi bao phủ, điều này sai vì sự lặp lại không hàm ý tính đa dạng. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề là, với mỗi chữ cái từ`'a'`ĐẾN`'z'`, quét toàn bộ chuỗi và kiểm tra xem chữ cái đó xuất hiện ở dạng chữ thường hay chữ hoa. Điều này đúng vì nó trực tiếp xác minh định nghĩa của một pangram. Tuy nhiên, đối với mỗi chữ cái trong số 26 chữ cái, chúng tôi có thể quét tối đa 100 ký tự, dẫn đến kiểm tra tối đa 2600 ký tự. Điều này đã tầm thường trong thực tế, nhưng cấu trúc này không hiệu quả so với những gì cần thiết. 

Một cách tiếp cận tốt hơn là đảo ngược quan điểm. Thay vì hỏi “mỗi chữ cái có xuất hiện trong chuỗi không?”, chúng tôi xử lý chuỗi một lần và ghi lại những chữ cái chúng tôi đã thấy. Mỗi ký tự được chuẩn hóa thành chữ thường và chèn vào một tập hợp hoặc được đánh dấu trong một mảng boolean có kích thước 26. Cuối cùng, chúng ta chỉ cần kiểm tra xem tất cả 26 vị trí đã được đánh dấu hay chưa. Điều này hiệu quả vì thuộc tính mà chúng ta quan tâm là tư cách thành viên trong một vũ trụ cố định có kích thước 26, do đó việc quét lặp lại là không cần thiết. 

Quan sát quan trọng là kích thước bảng chữ cái là không đổi. Khi chúng tôi nhận ra rằng miền quan tâm là cố định và nhỏ, chúng tôi có thể giảm vấn đề thành vấn đề tích lũy một lượt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(26 · n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi ký tự theo ký tự và duy trì cấu trúc theo dõi những chữ cái đã xuất hiện. 

1. Khởi tạo một mảng boolean`seen`có kích thước 26, tất cả được đặt thành sai. Mỗi chỉ số tương ứng với một chữ cái từ`'a'`ĐẾN`'z'`. Điều này cho phép chúng tôi theo dõi tư cách thành viên liên tục. 
2. Lặp lại từng ký tự trong chuỗi đầu vào. Đối với mỗi ký tự, hãy chuyển đổi nó thành chữ thường để các phiên bản chữ hoa và chữ thường ánh xạ tới cùng một không gian chỉ mục. Việc chuẩn hóa này đảm bảo rằng chúng tôi không đếm gấp đôi các chữ cái dựa trên trường hợp. 
3. Tính chỉ số của ký tự là`ord(c) - ord('a')`. Bản đồ này`'a'`đến 0,`'b'`đến 1, v.v. cho đến`'z'`ở tuổi 25. Đánh dấu`seen[index] = true`. Điều này ghi lại rằng bức thư đã được gặp ít nhất một lần. 
4. Sau khi xử lý tất cả các ký tự, hãy kiểm tra xem mọi mục nhập trong`seen`là đúng. Nếu bất kỳ vị trí nào vẫn sai thì ít nhất một chữ cái trong chuỗi bị thiếu, do đó chuỗi đó không phải là một pangram. 
5. Đầu ra`"YES"`nếu tất cả các mục nhập đều đúng, nếu không thì xuất ra`"NO"`. 

Tính chính xác phụ thuộc vào thực tế là mỗi chữ cái được xử lý độc lập và chỉ sự hiện diện của nó là quan trọng chứ không phải tần suất. 

### Tại sao nó hoạt động 

Thuật toán duy trì bất biến sau khi xử lý k ký tự đầu tiên,`seen[i]`đúng khi và chỉ khi chữ cái tương ứng với chỉ mục i xuất hiện trong k ký tự đó. Bất biến này đúng vì mỗi ký tự cập nhật chính xác một chỉ mục và không có thao tác nào xóa một giá trị sau khi được đặt. Khi kết thúc quá trình quét, trạng thái thể hiện chính xác tập hợp tất cả các chữ cái trong chuỗi, vì vậy việc kiểm tra xem tất cả các mục nhập có đúng hay không tương đương với việc kiểm tra xem chuỗi có chứa tất cả 26 chữ cái ít nhất một lần hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    s = input().strip()

    seen = [False] * 26

    for c in s:
        if c.isalpha():
            idx = ord(c.lower()) - ord('a')
            seen[idx] = True

    if all(seen):
        print("YES")
    else:
        print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp đọc độ dài và chuỗi, mặc dù độ dài không thực sự cần thiết cho tính chính xác. Logic cốt lõi là mảng boolean có kích thước cố định để theo dõi độ bao phủ của chữ cái. 

Cuộc gọi tới`lower()`đảm bảo xử lý không phân biệt chữ hoa chữ thường. các`all(seen)`kiểm tra là bản dịch trực tiếp yêu cầu mỗi chữ cái trong bảng chữ cái phải xuất hiện ít nhất một lần. 

Một điểm tinh tế là chúng ta không bao giờ dựa vào tần số mà chỉ dựa vào sự hiện diện. Đây là lý do tại sao mảng boolean là đủ thay vì mảng truy cập. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Chuỗi đầu vào:`"toosmallword"`Chúng tôi theo dõi sự tiến hóa của`seen`về mặt khái niệm khi các chữ cái xuất hiện. 

| Nhân vật | Chữ thường | Chỉ mục | Đã xem cập nhật | 
| --- | --- | --- | --- | 
| t | t | 19 | đánh dấu 19 | 
| o | o | 14 | đánh dấu 14 | 
| o | o | 14 | đã được đánh dấu | 
| s | s | 18 | đánh dấu 18 | 
| m | m | 12 | đánh dấu 12 | 
| một | một | 0 | đánh dấu 0 | 
| tôi | tôi | 11 | đánh dấu 11 | 
| tôi | tôi | 11 | đã được đánh dấu | 
| w | w | 22 | đánh dấu 22 | 
| o | o | 14 | đã được đánh dấu | 
| r | r | 17 | đánh dấu 17 | 
| d | d | 3 | đánh dấu 3 | 

Sau khi xử lý, chỉ một tập hợp nhỏ các chữ cái được đánh dấu. Nhiều mục vẫn sai nên kết quả đầu ra là`"NO"`. 

Dấu vết này cho thấy rằng việc lặp lại một vài chữ cái sẽ không giúp ích gì trừ khi đạt được tất cả 26 chỉ số riêng biệt. 

### Mẫu 2 

Chuỗi đầu vào:`"TheQuickBrownFoxJumpsOverTheLazyDog"`Khi chúng tôi quét qua, mỗi chữ cái cuối cùng sẽ góp phần lấp đầy 26 vị trí. 

| Nhân vật | Chữ thường | Chỉ mục | Đã xem cập nhật | 
| --- | --- | --- | --- | 
| T | t | 19 | đánh dấu | 
| h | h | 7 | đánh dấu | 
| e | e | 4 | đánh dấu | 
| Hỏi | q | 16 | đánh dấu | 
| bạn | bạn | 20 | đánh dấu | 
| tôi | tôi | 8 | đánh dấu | 
| c | c | 2 | đánh dấu | 
| k | k | 10 | đánh dấu | 
| ... | ... | ... | ... | 
| g | g | 6 | đánh dấu | 

Cuối cùng, tất cả các chỉ số từ 0 đến 25 đã được đặt thành true ít nhất một lần. 

Kiểm tra cuối cùng xác nhận mức độ bao phủ đầy đủ, vì vậy kết quả đầu ra là`"YES"`. 

Những ví dụ này minh họa sự khác biệt chính giữa bao phủ một phần và bao phủ toàn bộ bảng chữ cái, đó là tiêu chí quyết định toàn bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần và được ánh xạ tới bản cập nhật liên tục | 
| Không gian | O(1) | các`seen`mảng có kích thước cố định 26 bất kể đầu vào | 

Cho rằng$n \le 100$, thuật toán chạy trong thời gian thực tế không đổi. Ngay cả dưới những hạn chế lớn hơn nhiều, quá trình quét tuyến tính vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline

    def solve():
        n = int(input().strip())
        s = input().strip()

        seen = [False] * 26

        for c in s:
            seen[ord(c.lower()) - ord('a')] = True

        print("YES" if all(seen) else "NO")

    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("12\ntoosmallword") == "NO"
assert run("35\nTheQuickBrownFoxJumpsOverTheLazyDog") == "YES"

# custom cases
assert run("1\na") == "NO"
assert run("52\nabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ") == "YES"
assert run("26\nabcdefghijklmnopqrstuvwxyz") == "YES"
assert run("3\nabc") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"1\na"`| KHÔNG | Đầu vào tối thiểu, một chữ cái không thể là pangram | 
|`"52\nabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"`| CÓ | Không phân biệt chữ hoa chữ thường và bảo hiểm đầy đủ | 
|`"26\nabcdefghijklmnopqrstuvwxyz"`| CÓ | Pangram hợp lệ tối thiểu chính xác | 
|`"3\nabc"`| KHÔNG | Bảo hiểm một phần bảng chữ cái không thành công | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là phân biệt chữ hoa chữ thường. Nếu thuật toán không chuẩn hóa các ký tự thì`"AaBbCc...Zz"`sẽ xuất hiện không chính xác vì thiếu nhiều chữ cái vì chữ hoa và chữ thường sẽ ánh xạ tới các chỉ mục khác nhau. Trong cách tiếp cận của chúng tôi,`lower()`đảm bảo cả hai biểu mẫu ánh xạ tới cùng một vị trí, vì vậy`"A"`Và`"a"`cả hai đều đặt chỉ số 0. 

Một trường hợp cạnh khác là đầu vào nặng trùng lặp, chẳng hạn như`"aaaaaaaaaaaaaaaaaa"`. Ở đây, chỉ có một chỉ mục được đánh dấu. Mảng boolean ngăn chặn việc đếm quá làm sai thuật toán, vì các lần cập nhật lặp lại không làm thay đổi tính chính xác. 

Trường hợp cạnh cuối cùng là đầu vào có phạm vi bảo hiểm tối thiểu như`"abcdefghijklmnopqrstuvwxyz"`so với việc thiếu một chữ cái như`"abcdefghijklmnopqrstuvwxy"`. Trong trường hợp đầu tiên, tất cả các chỉ số được điền chính xác một lần và kết quả là`"YES"`. Trong lần thứ hai, chỉ số 25 vẫn sai và lần kiểm tra cuối cùng mang lại kết quả chính xác`"NO"`.
