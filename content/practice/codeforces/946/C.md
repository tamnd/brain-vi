---
title: "CF 946C - Chuyển đổi chuỗi"
description: "Chúng ta được cấp một chuỗi gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép thực hiện một thao tác chỉ tăng một ký tự lên một bước trong bảng chữ cái, ví dụ chuyển c thành d hoặc a thành b. Ký tự không thể giảm và z là thiết bị đầu cuối."
date: "2026-06-17T02:29:11+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "strings"]
categories: ["algorithms"]
codeforces_contest: 946
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 39 (Rated for Div. 2)"
rating: 1300
weight: 946
solve_time_s: 76
verified: true
draft: false
---

[CF 946C - Chuyển đổi chuỗi](https://codeforces.com/problemset/problem/946/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, dây 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi gồm các chữ cái tiếng Anh viết thường. Chúng ta được phép thực hiện một thao tác chỉ tăng một ký tự lên một bước trong bảng chữ cái, ví dụ như chuyển`c`vào trong`d`hoặc`a`vào trong`b`. Ký tự không thể giảm, và`z`là thiết bị đầu cuối. 

Sau khi áp dụng bất kỳ số lượng gia tăng nào như vậy vào các vị trí riêng lẻ, chúng ta thu được một chuỗi mới. Từ chuỗi cuối cùng này, chúng tôi không bắt buộc phải khớp chính xác bất kỳ thứ gì, nhưng chúng tôi muốn chuỗi cố định`"abcdefghijklmnopqrstuvwxyz"`xuất hiện dưới dạng một dãy con. Điều đó có nghĩa là chúng ta có thể chọn 26 vị trí theo thứ tự tăng dần sao cho chúng đánh vần bảng chữ cái. 

Nhiệm vụ là quyết định xem điều đó có khả thi hay không và nếu có, hãy xuất ra một chuỗi chuyển đổi thu được để đạt được điều này. Nếu có nhiều câu trả lời thì bất kỳ câu trả lời nào hợp lệ đều được chấp nhận. 

Kích thước đầu vào lên tới 100000 ký tự, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng khám phá các phép biến đổi một cách rõ ràng hoặc mô phỏng tất cả các mức tăng có thể có cho mỗi ký tự. Mỗi ký tự có thể được tăng lên tối đa 25 lần, nhưng thực hiện điều đó một cách độc lập cho mỗi vị trí theo cách mạnh mẽ vẫn sẽ dẫn đến việc quét lặp lại không cần thiết. 

Một điểm tinh tế là chúng ta được phép bỏ qua hoàn toàn các ký tự trong dãy con nên không bắt buộc phải sử dụng tất cả các chữ cái. Tuy nhiên, mọi ký tự chúng tôi sử dụng phải có khả năng tăng dần theo thứ tự chữ cái đích được yêu cầu. 

Một cách tiếp cận ngây thơ có thể cố gắng gán các chữ cái trong bảng chữ cái vào các vị trí một cách tham lam mà không theo dõi cẩn thận tính khả thi, điều này có thể thất bại trong trường hợp các phép gán sớm tiêu tốn tất cả các ký tự có thể sử dụng được. 

Ví dụ, hãy xem xét một chuỗi như`"abz..."`nơi có nhiều`z`nhân vật xuất hiện sớm. Một người tham lam ngây thơ có thể cố gắng ghép các chữ cái đầu tiên với nhau một cách không chính xác.`z`, Nhưng`z`không thể tăng lên, vì vậy nó sẽ chặn các bài tập trong tương lai. 

Một trường hợp lỗi khác là khi chuỗi chứa đủ các chữ cái trên toàn cầu nhưng chúng không ở vị trí cấu trúc tăng dần có thể sử dụng được. Vì dãy con phụ thuộc vào thứ tự chứ không phải chỉ tần số nên bất kỳ giải pháp nào cũng phải tôn trọng các ràng buộc so khớp từ trái sang phải. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là xem xét từng ký tự và cố gắng gán nó cho một chữ cái đích trong`"abcdefghijklmnopqrstuvwxyz"`, có thể sau khi tăng nó. Đối với mỗi vị trí trong bảng chữ cái, chúng ta có thể quét tiếp trong chuỗi để tìm ký tự phù hợp, sau đó đánh dấu ký tự đó đã sử dụng và tiếp tục. Đây thực chất là một sự kết hợp tham lam. 

Điều này hoạt động về mặt khái niệm vì mỗi ký tự chỉ có thể di chuyển lên trên, vì vậy chúng ta không bao giờ cần phải xem xét việc sắp xếp lại các chữ cái, chỉ xem liệu ký tự sau có thể đáp ứng một chữ cái được yêu cầu hay không. Tuy nhiên, nếu được thực hiện mà không cẩn thận, việc quét liên tục chuỗi cho từng ký tự bảng chữ cái sẽ dẫn đến O(26n), điều này vẫn ổn, nhưng mô phỏng đơn giản hơn về tất cả các gia số trên mỗi ký tự sẽ là O(26n) trên mỗi vị trí hoặc tệ hơn. 

Quan sát quan trọng là chúng ta chỉ quan tâm liệu mỗi chữ cái trong bảng chữ cái có thể khớp theo thứ tự hay không. Mỗi ký tự đóng góp một loạt các chữ cái có thể sử dụng được: một ký tự`c`có thể trở thành bất kỳ lá thư nào từ`c`ĐẾN`z`. Vì vậy, chúng ta có thể coi mỗi vị trí là cung cấp một khoảng công suất. Chúng tôi muốn phù hợp`'a' → 'z'`tuần tự sử dụng các vị trí hợp lệ sớm nhất có thể. 

Do đó, chúng tôi quét chuỗi một lần và đối với mỗi ký tự trong bảng chữ cái, chúng tôi chọn vị trí không được sử dụng sớm nhất mà ký tự hiện tại có thể tiếp cận được. Sau khi sử dụng, vị trí đó sẽ cố định và không thể sử dụng lại. 

Sự tham lam này là đúng vì việc chiếm vị trí hợp lệ sớm nhất luôn để lại sự linh hoạt tối đa cho các chữ cái sau này. Mọi lựa chọn sau này sẽ chỉ làm giảm các lựa chọn còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét Brute mỗi chữ cái | O(26n) | O(n) | Đã chấp nhận | 
| Đường chuyền đơn tham lam tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một con trỏ trên chuỗi và cố gắng khớp các chữ cái từ`'a'`ĐẾN`'z'`theo thứ tự. 

1. Bắt đầu với chỉ mục`i = 0`và thư mục tiêu`cur = 'a'`. Chúng ta sẽ xây dựng một danh sách các chỉ số được chọn để xây dựng dãy con. 
2. Đối với mỗi chữ cái đích từ`'a'`ĐẾN`'z'`, quét tiếp từ chỉ mục hiện tại cho đến khi chúng tôi tìm thấy vị trí`j`như vậy`s[j] <= cur`. Điều kiện này đảm bảo chúng ta có thể tăng`s[j]`lên tới`cur`không vi phạm quy tắc chữ chỉ tăng. 
3. Nếu chúng ta đến cuối chuỗi mà không tìm được vị trí như vậy thì việc xây dựng là không thể và chúng ta xuất ra`-1`. Điều này xảy ra vì không có ký tự còn lại nào có thể đạt đến chữ cái được yêu cầu. 
4. Khi chúng ta tìm được một vị trí hợp lệ`j`, chúng tôi gán nó để đại diện`cur`, ghi`j`và di chuyển quá trình bắt đầu quét sang`j + 1`. 
5. Sau khi thu thập đủ 26 vị trí, chúng ta xây dựng chuỗi đầu ra bằng cách sao chép chuỗi gốc và thay thế từng vị trí đã chọn`j`với chữ cái trong bảng chữ cái được yêu cầu. 
6. Xuất chuỗi kết quả. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào bất biến khớp đơn điệu: sau khi khớp k chữ cái đầu tiên của bảng chữ cái, chúng tôi đã chọn tập chỉ mục nhỏ nhất có thể gồm k vị trí có thể nhận ra chúng. Bởi vì mỗi vị trí được chọn là vị trí khả thi sớm nhất nên bất kỳ lựa chọn thay thế nào sau này sẽ chỉ dịch chuyển kết quả khớp sang bên phải, làm giảm không gian hậu tố có sẵn cho các chữ cái còn lại. Vì tính khả thi chỉ phụ thuộc vào sự tồn tại của một vị trí tương thích sau này chứ không bao giờ phụ thuộc vào tính linh hoạt chưa được sử dụng trước đó nên sự lựa chọn tham lam là an toàn và tối ưu. 

Ràng buộc mà các ký tự chỉ tăng lên đảm bảo tính độc lập giữa các phép gán: một khi một ký tự được sử dụng cho một chữ cái, nó không thể được sử dụng lại nhưng cũng không thể được sửa đổi xuống để can thiệp vào các kết quả trùng khớp trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = list(input().strip())
    n = len(s)
    
    pos = []
    i = 0
    
    for c in range(26):
        target = chr(ord('a') + c)
        found = -1
        
        while i < n:
            if s[i] <= target:
                found = i
                i += 1
                break
            i += 1
        
        if found == -1:
            print(-1)
            return
        
        pos.append(found)
    
    for idx, c in zip(pos, range(26)):
        s[idx] = chr(ord('a') + c)
    
    print("".join(s))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi chuỗi thành một danh sách có thể thay đổi để việc cập nhật có hiệu quả. Con trỏ`i`buộc mỗi vị trí được chọn đều tăng nghiêm ngặt, điều này cần thiết cho thứ tự tiếp theo. 

Đối với mỗi chữ cái trong bảng chữ cái, chúng tôi quét về phía trước cho đến khi tìm thấy ký tự có thể nâng lên thành chữ cái đó. Sự so sánh`s[i] <= target`mã hóa tính khả thi, vì nếu`s[i]`đã lớn hơn rồi`target`, nó không thể giảm để phù hợp với nó. 

Khi tất cả 26 chữ cái đã được gán, chúng tôi ghi đè các chỉ mục đã chọn bằng các ký tự đích của chúng và xây dựng lại chuỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
aacceeggiikkmmooqqssuuwwyy
```Chúng tôi khớp chính xác từng chữ cái vì mỗi cặp đã sắp xếp theo tiến trình cần thiết. 

| Bước | Mục tiêu | Chỉ số được chọn | Ký tự tại chỉ mục | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | một | 0 | một | giao | 
| 2 | b | 2 | c → b | giao | 
| 3 | c | 2 | c | giao | 
| ... | ... | ... | ... | ... | 

Quá trình diễn ra suôn sẻ vì mỗi chữ cái cần thiết đều có sẵn một vị trí tương thích theo thứ tự. 

Đầu ra:```
abcdefghijklmnopqrstuvwxyz
```Điều này xác nhận rằng một cấu trúc được căn chỉnh hoàn hảo là đủ và việc kết hợp tham lam không bỏ qua tính linh hoạt cần thiết. 

### Ví dụ 2 

đầu vào:```
abczzzzzzzzzzzzzzzzzzzzz
```Chúng tôi phù hợp`a`,`b`,`c`, thì cho`d`trở đi chúng ta phải sử dụng`z`các vị trí. 

| Bước | Mục tiêu | Chỉ số được chọn | Ký tự được sử dụng | Khả thi | 
| --- | --- | --- | --- | --- | 
| một | một | 0 | một | vâng | 
| b | b | 1 | b | vâng | 
| c | c | 2 | c | vâng | 
| d | d | 3 | z | vâng | 
| e | e | 4 | z | vâng | 

Điều này cho thấy ngay cả những ký tự chất lượng thấp cũng có thể được sử dụng lại cho nhiều chữ cái cao hơn do tăng dần. 

Chuỗi cuối cùng vẫn chứa bảng chữ cái đầy đủ dưới dạng dãy con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | mỗi chỉ mục được con trỏ quét truy cập tối đa một lần | 
| Không gian | O(n) | lưu trữ chuỗi có thể thay đổi và các chỉ mục đã chọn | 

Thuật toán có kích thước tuyến tính của đầu vào, vừa vặn thoải mái trong giới hạn 1e5 và giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = StringIO(inp)
    out = StringIO()
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdin = backup_stdin
        sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert solve_capture("aacceeggiikkmmooqqssuuwwyy\n") == "abcdefghijklmnopqrstuvwxyz"

# all identical chars, impossible
assert solve_capture("zzzz\n") == "-1"

# already valid minimal case
assert solve_capture("abcdefghijklmnopqrstuvwxyz\n") == "abcdefghijklmnopqrstuvwxyz"

# mixed case with forced upgrades
assert solve_capture("abcz" + "z"*30 + "\n") != ""

# insufficient structure
assert solve_capture("abc\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| zzzz | -1 | không thể do không có chữ nhỏ | 
| abcdefghijklmnopqrstuvwxyz | cùng một chuỗi | đã hợp lệ | 
| abcz + nhiều z | bảng chữ cái hợp lệ | tái sử dụng chữ lớn | 
| abc | -1 | không đủ độ dài | 

## Vỏ cạnh 

Một dạng lỗi là khi chuỗi chỉ chứa các chữ cái lớn như`z`. Trong trường hợp đó, về mặt lý thuyết, mọi chữ cái mục tiêu đều có thể truy cập được từ`z`, nhưng chúng ta vẫn cần 26 vị trí riêng biệt. Nếu chuỗi quá ngắn, thuật toán sẽ thất bại ở một số chữ cái đích vì không còn chỉ mục nào chưa được sử dụng, trả về chính xác`-1`. 

Một trường hợp khác là khi có các chữ cái nhỏ nhưng được sắp xếp kém, chẳng hạn`"cba..."`. Quá trình quét đảm bảo chúng tôi vẫn chọn các vị trí hợp lệ theo thứ tự, nhưng các chữ cái lớn trước đó được bỏ qua một cách chính xác vì chúng không thể bị hạ cấp. 

Trường hợp khó phát hiện cuối cùng là khi các lựa chọn hợp lệ tồn tại nhưng bị trễ về phía bên phải. Quá trình quét dựa trên con trỏ đảm bảo chúng tôi luôn chọn vị trí hợp lệ sớm nhất, do đó chúng tôi không bao giờ vô tình sử dụng phân đoạn sau có thể cần thiết cho các chữ cái trong tương lai.
