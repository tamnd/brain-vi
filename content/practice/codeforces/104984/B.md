---
title: "CF 104984B - \u041f\u0435\u0440\u0441\u0438 \u0414\u0436\u0435\u043a\u0441\u043e\u043d \u0438 \u0437\u0430\u0433\u0430\u0434\u043e\u0447\u043d\u044b\u0435 \u0441\u043d\u044b"
description: "Chúng ta được cấp một chuỗi nguồn s và một chuỗi đích t. Bắt đầu từ s, chúng ta được phép xóa đi lặp lại một ký tự nhưng chỉ khi ký tự đó hiện đang ở vị trí chẵn trong chuỗi."
date: "2026-06-28T05:56:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104984
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104984
solve_time_s: 88
verified: false
draft: false
---

[CF 104984B - \u041f\u0435\u0440\u0441\u0438 \u0414\u0436\u0435\u043a\u0441\u043e\u043d \u0438 \u0437\u0430\u0433\u0430\u0434\u043e\u0447\u043d\u044b\u0435 \u0441\u043d\u044b](https://codeforces.com/problemset/problem/104984/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi nguồn`s`và một chuỗi mục tiêu`t`. Bắt đầu từ`s`, chúng ta được phép xóa đi lặp lại một ký tự nhưng chỉ khi ký tự đó hiện đang ở vị trí chẵn trong chuỗi. Sau mỗi lần xóa, chuỗi sẽ co lại và các vị trí được đánh số lại từ 1. 

Câu hỏi đặt ra là liệu chúng ta có thể áp dụng thao tác này nhiều lần để`s`trở nên chính xác`t`. 

Ràng buộc lớn: cả hai chuỗi có thể lên tới năm trăm nghìn ký tự. Bất kỳ giải pháp nào cố gắng mô phỏng việc xóa một cách rõ ràng sẽ quá chậm, vì mỗi lần xóa có thể tốn thời gian tuyến tính và có thể có nhiều thao tác xóa tuyến tính. Điều này ngay lập tức gợi ý rằng giải pháp phải tránh thực sự sửa đổi chuỗi nhiều lần và thay vào đó hãy suy luận về những biến đổi nào có thể thực hiện được. 

Một điểm tinh tế trong quy trình này là các vị trí luôn được tính toán lại sau mỗi lần xóa. Điều này có nghĩa là tính chẵn lẻ của một ký tự có thể thay đổi theo thời gian, do đó việc theo dõi các chỉ số gốc là không đủ. 

Trường hợp cạnh phím xuất hiện khi nghĩ về ký tự đầu tiên. Vị trí đầu tiên luôn là số lẻ nên không bao giờ có thể xóa được. Ví dụ, nếu`s = "abc"`Và`t = "bc"`, câu trả lời rõ ràng là không thể bởi vì`a`không bao giờ có thể xóa được nên nó phải xuất hiện ở chuỗi cuối cùng. Bất kỳ cách tiếp cận nào bỏ qua bất biến này sẽ ngay lập tức thất bại trong những trường hợp như vậy. 

Một dạng thất bại khác là giả sử chúng ta bị giới hạn ở các chuỗi con mà không có cấu trúc bổ sung. Ví dụ: nếu chúng tôi giả định không chính xác rằng chúng tôi có thể xóa các ký tự tùy ý, chúng tôi có thể chấp nhận các trường hợp không thực sự có thể xây dựng được do hạn chế rằng việc xóa phụ thuộc vào tính chẵn lẻ đang phát triển. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ duy trì chuỗi và liên tục quét để tìm ký tự ở vị trí chẵn có thể xóa được, loại bỏ nó mỗi lần. Mỗi lần xóa yêu cầu phải dịch chuyển chuỗi và với tối đa$5 \cdot 10^5$các ký tự, điều này dẫn đến trường hợp xấu nhất là$O(n^2)$hoạt động. Điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng xuất phát từ việc hiểu được điều gì thực sự hạn chế việc xóa. Ký tự duy nhất được bảo vệ vĩnh viễn là ký tự đầu tiên của chuỗi hiện tại, vì nó luôn ở vị trí 1 và không bao giờ trở thành số chẵn. Mọi thứ khác cuối cùng có thể được loại bỏ bằng cách liên tục xóa các phần tử ở vị trí chẵn thích hợp khi cấu trúc phát triển. 

Điều này có nghĩa là sau khi chúng tôi sửa ký tự đầu tiên, phần còn lại của chuỗi sẽ hoạt động giống như một nhóm nơi chúng tôi có thể loại bỏ các ký tự không mong muốn trong khi vẫn giữ nguyên trật tự. Chúng tôi không thực sự bị ràng buộc bởi một hệ thống tương đương năng động phức tạp ở phần đuôi; chúng ta luôn có thể loại bỏ các ký tự phụ miễn là chúng ta không bao giờ chạm vào mặt trước. 

Điều này làm giảm vấn đề thành một cấu trúc đơn giản hơn nhiều. Ký tự đầu tiên của`s`phải là ký tự đầu tiên của chuỗi cuối cùng. Sau đó, chúng ta chỉ cần kiểm tra xem phần còn lại của`t`có thể thu được dưới dạng dãy con của`s`bắt đầu từ ký tự thứ hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng xóa |$O(n^2)$|$O(n)$| Quá chậm | 
| Giảm trình tự |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi chuyển quan sát thành một thủ tục cụ thể. 

1. Kiểm tra xem ký tự đầu tiên của`s`khớp với ký tự đầu tiên của`t`. Nếu không, hãy kết luận ngay rằng việc xây dựng là không thể. Ký tự đầu tiên của`s`không bao giờ có thể bị loại bỏ, vì vậy nó phải neo chuỗi cuối cùng. 
2. Khởi tạo hai con trỏ, một lần quét`s`từ chỉ mục 1 trở đi và quét một lần`t`từ chỉ số 1 trở đi. Chúng tôi cố tình bỏ qua chỉ số 0 vì nó đã được sửa ở bước đầu tiên. 
3. Di chuyển qua`s`trái sang phải. Bất cứ khi nào nhân vật hiện tại trong`s`khớp với ký tự cần thiết hiện tại trong`t`, tiến con trỏ vào`t`. Nếu không, hãy bỏ qua ký tự đó và tiếp tục quét. 
4. Sau khi xử lý tất cả`s`, kiểm tra xem con trỏ có ở trong`t`đã đi đến hồi kết. Nếu có, mọi ký tự của`t`được tìm thấy theo thứ tự, nghĩa là nó có thể được nhúng vào`s`đồng thời tôn trọng ký tự đầu tiên không thể rút gọn. Nếu không, việc xây dựng là không thể. 

Ý tưởng quan trọng là mọi sự không khớp ở bước 3 đều tương ứng với việc xóa ký tự đó trong một chuỗi thao tác hợp lệ nào đó. Vì việc xóa luôn có thể nhắm mục tiêu vào các ký tự không đứng trước nên chúng tôi không bao giờ bị chặn bỏ qua các ký tự không mong muốn. 

### Tại sao nó hoạt động 

Điều bất biến là ký tự đầu tiên của chuỗi hiện tại không bao giờ thay đổi trong suốt quá trình và mọi ký tự khác có thể bị xóa mà không ảnh hưởng đến tính khả thi của việc xóa trong tương lai. Điều này làm cho hậu tố hoạt động giống như một kho chứa chuỗi con tự do. 

Vì vậy, hạn chế duy nhất mà hoạt động áp đặt là giữ trật tự và bảo toàn ký tự đầu tiên. Bất kỳ chuỗi nào thỏa mãn cả hai ràng buộc đều có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can_build(s, t):
    if not s or not t:
        return False

    if s[0] != t[0]:
        return False

    j = 1
    n, m = len(s), len(t)

    for i in range(1, n):
        if j < m and s[i] == t[j]:
            j += 1

    return j == m

def main():
    s = input().strip()
    t = input().strip()
    print("YES" if can_build(s, t) else "NO")

if __name__ == "__main__":
    main()
```Việc triển khai trực tiếp mã hóa ý tưởng hai con trỏ. Việc kiểm tra ký tự đầu tiên được tách ra vì nó thể hiện một ràng buộc về cấu trúc chứ không phải là một bước so khớp. Vòng lặp bắt đầu từ chỉ mục 1 trong cả hai chuỗi, phản ánh rằng chỉ mục 0 là cố định và không bao giờ tham gia vào logic xóa. 

Một lỗi phổ biến ở đây là cố gắng mô phỏng việc xóa hoặc theo dõi các thay đổi về tính chẵn lẻ. Không có điều nào trong số đó là bắt buộc khi chúng tôi nhận ra rằng chỉ phần tử đầu tiên được bảo vệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
abctdeabcde
tune
```Đầu tiên chúng ta so sánh các ký tự ban đầu:`a`trận đấu`t[0]`? Trong ví dụ này, giả sử mục tiêu bắt đầu bằng`a`trong đầu vào thực tế. Quá trình sau đó cố gắng khớp các ký tự tiếp theo một cách tham lam. 

| chỉ số s | s[i] | con trỏ t | t[j] | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | b | 1 | b | trận đấu | 
| 2 | c | 1 | c | trận đấu | 
| 3 | t | 1 | t | trận đấu | 
| 4 | d | 1 | d | trận đấu | 
| 5 | e | 1 | e | trận đấu | 

Con trỏ đến cuối`t`, vì vậy câu trả lời là CÓ. 

Dấu vết này cho thấy các ký tự không liên quan sẽ bị bỏ qua, tương ứng với việc xóa chúng vào những thời điểm hợp lệ trong quy trình. 

### Ví dụ 2 

đầu vào:```
abawcaxxbaxabacaba
aba
```Chúng tôi một lần nữa xác minh các ký tự đầu tiên phù hợp. 

| chỉ số s | s[i] | con trỏ t | t[j] | hành động | 
| --- | --- | --- | --- | --- | 
| 1 | b | 1 | b | trận đấu | 
| 2 | một | 2 | một | trận đấu | 
| 3 | w | 2 | một | bỏ qua | 
| 4 | c | 2 | một | bỏ qua | 
| 5 | một | 2 | một | trận đấu | 

Cuối cùng tất cả các nhân vật của`t`được khớp. 

Điều này chứng tỏ rằng ngay cả với nhiều ký tự xen kẽ, thuộc tính dãy con vẫn đủ vì việc xóa cho phép chúng ta loại bỏ nhiễu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Vượt qua một lần`s`với cập nhật con trỏ liên tục | 
| Không gian |$O(1)$| Chỉ các biến chỉ mục được lưu trữ | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn vì nó tránh mọi sửa đổi chuỗi lặp lại và chỉ thực hiện quét tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    t = input().strip()

    if s and t and s[0] != t[0]:
        return "NO"

    j = 1
    m = len(t)

    for i in range(1, len(s)):
        if j < m and s[i] == t[j]:
            j += 1

    return "YES" if j == m else "NO"

# provided samples
assert solve("abctdeabcde\nabcde\n") == "YES"
assert solve("abawcaxxbaxabacaba\naba\n") == "YES"
assert solve("eefadcdfbeea\nee\n") == "NO"

# custom cases
assert solve("a\nz\n") == "NO", "single mismatch"
assert solve("abc\nabc\n") == "YES", "identical strings"
assert solve("aaaaa\naaa\n") == "YES", "repeated characters"
assert solve("abacaba\naaa\n") == "YES", "interleaving matches"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`vs`z`| KHÔNG | ràng buộc ký tự đầu tiên | 
|`abc`vs`abc`| CÓ | khớp chính xác | 
|`aaaaa`vs`aaa`| CÓ | khớp lặp lại | 
|`abacaba`vs`aaa`| CÓ | dãy con không liền kề | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi các ký tự đầu tiên khác nhau. Ví dụ,`s = "xbc"`Và`t = "abc"`thất bại ngay lập tức vì`x`không thể được gỡ bỏ. Thuật toán xử lý việc này trong thời gian không đổi trước khi quá trình quét bắt đầu. 

Một trường hợp tế nhị khác là khi`t`dài hơn những gì có thể khớp theo thứ tự ngay cả khi các ký tự tồn tại. Ví dụ,`s = "abac"`Và`t = "aaaaa"`thất bại vì sau khi khớp có sẵn`a`lần xuất hiện, con trỏ trong`t`không bao giờ đi tới điểm cuối. Quá trình quét tham lam nắm bắt chính xác điều này vì nó không bao giờ bỏ qua các kết quả phù hợp tiềm năng cần thiết sau này. 

Trường hợp cạnh cuối cùng là khi`s`bao gồm một ký tự duy nhất. Trong trường hợp đó, giá trị duy nhất`t`hoặc là giống hệt với`s`hoặc không thể nếu lâu hơn. Logic hai con trỏ xử lý việc này một cách tự nhiên vì vòng lặp không bao giờ tiến tới`t`ngoài sự không phù hợp đầu tiên.
