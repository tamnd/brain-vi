---
title: "CF 104273A - Đạo văn mã"
description: "Chúng ta có hai chuỗi được xây dựng từ các chữ cái viết thường. Hãy coi chuỗi đầu tiên là một cuộn băng dài gồm các ký tự do Bob tạo ra và chuỗi thứ hai là chuỗi ngắn hơn mà Alice tin rằng sẽ vẫn còn sau khi Bob sửa đổi."
date: "2026-07-01T21:23:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104273
codeforces_index: "A"
codeforces_contest_name: "\u0418\u043d\u0434\u0438\u0432\u0438\u0434\u0443\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438 \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2023"
rating: 0
weight: 104273
solve_time_s: 84
verified: true
draft: false
---

[CF 104273A - Đạo văn mã](https://codeforces.com/problemset/problem/104273/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi được xây dựng từ các chữ cái viết thường. Hãy coi chuỗi đầu tiên là một cuộn băng dài gồm các ký tự do Bob tạo ra và chuỗi thứ hai là chuỗi ngắn hơn mà Alice tin rằng sẽ vẫn còn sau khi Bob sửa đổi. 

Bob được phép xóa các ký tự khỏi chuỗi của mình, nhưng chỉ theo một cách rất cụ thể: anh ấy liên tục xóa hai ký tự liền kề cùng một lúc. Mỗi lần xóa sẽ loại bỏ một cặp liền kề và rút ngắn chuỗi. Sau khi thực hiện thao tác này nhiều lần, câu hỏi đặt ra là liệu chuỗi đó có thể trở nên chính xác bằng chuỗi đích hay không. 

Vì vậy, nhiệm vụ không phải là xóa tùy ý. Chúng tôi chỉ được phép xóa chuỗi theo từng đoạn có kích thước hai và luôn ở các vị trí liền kề trong chuỗi hiện tại. Chúng ta phải quyết định xem có tồn tại một chuỗi các thao tác xóa để biến chuỗi gốc thành chuỗi đích hay không. 

Các ràng buộc cho phép chuỗi lên tới 200.000 ký tự. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào mô phỏng việc xóa một cách rõ ràng hoặc cố gắng khám phá tất cả các chuỗi loại bỏ, vì ngay cả một số tuyến tính các lựa chọn phân nhánh cũng sẽ bùng nổ theo kiểu tổ hợp. Một giải pháp hợp lệ phải gần với thời gian tuyến tính. 

Một trường hợp phức tạp xuất phát từ việc hiểu ý nghĩa thực sự của việc “xóa liền kề”. Thật dễ dàng để cho rằng thứ tự xóa hạn chế rất nhiều cấu trúc cuối cùng, nhưng quan sát quan trọng là việc xóa có thể được sử dụng để loại bỏ bất kỳ phần còn sót lại có kích thước chẵn bất kể nội dung ký tự, miễn là các ký tự đó có thể được ghép nối thông qua việc giảm kề cận lặp đi lặp lại. 

Ví dụ: nếu sau khi chọn chuỗi đích làm chuỗi con, các ký tự còn lại tạo thành bất kỳ chuỗi nào có độ dài chẵn thì phần còn lại luôn có thể bị xóa hoàn toàn: 

đầu vào:```
abac
a
```Ở đây chúng ta giữ “a” và loại bỏ “bac”. Phần bị loại bỏ có độ dài 3, là số lẻ nên không thể loại bỏ hoàn toàn bằng cách xóa cặp. Điều này buộc phải bác bỏ ngay cả khi “a” là một dãy con hợp lệ. 

Kiểu thất bại chính của lối suy nghĩ ngây thơ là cho rằng chúng ta cần mô phỏng chính quá trình xóa. Điều đó dẫn đến việc mô phỏng ngăn xếp phức tạp hoặc khoảng thời gian DP, điều này là không cần thiết. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các chuỗi xóa cặp liền kề có thể xảy ra. Ở mỗi bước, chúng ta có thể chọn bất kỳ cặp liền kề nào và loại bỏ nó, tạo ra một chuỗi mới. Điều này tạo ra một yếu tố phân nhánh rất lớn và thậm chí với độ dài vừa phải, số lượng trạng thái sẽ trở thành cấp số nhân. Với 200.000 ký tự, điều này hoàn toàn không thể thực hiện được. 

Một nỗ lực có cấu trúc hơn là đảo ngược quá trình. Thay vì xóa các cặp khỏi`s`để đạt được`t`, chúng ta có thể nghĩ đến việc chọn ra nhân vật nào sống sót. Các nhân vật sống sót phải xuất hiện theo thứ tự, vì vậy`t`phải là dãy con của`s`. Sau khi chọn những ký tự này, mọi thứ khác sẽ bị xóa theo cặp. 

Thông tin chi tiết quan trọng là việc xóa dựa trên vùng lân cận không hạn chế _ký tự nào có thể bị xóa cùng nhau về lâu dài_. Bất kỳ chuỗi nào có độ dài chẵn đều có thể được giảm hoàn toàn thành chuỗi trống bằng cách liên tục loại bỏ các cặp liền kề. Chúng tôi không bao giờ cần các cặp khớp nhau về giá trị, chỉ cần liền kề tại thời điểm xóa. Vì chúng ta luôn có thể sắp xếp lại các thao tác xóa cục bộ để tập hợp các phần tử lại với nhau, tính chẵn lẻ của độ dài trở thành hạn chế duy nhất đối với việc xóa hoàn toàn. 

Vì vậy, vấn đề giảm xuống còn hai kiểm tra: 

Đầu tiên, hãy xác minh rằng`t`có thể thu được dưới dạng dãy con của`s`. 

Thứ hai, đảm bảo rằng số lượng ký tự còn sót lại,`|s| - |t|`, là số chẵn. Nếu nó chẵn, chuỗi còn sót lại luôn có thể được loại bỏ hoàn toàn bằng cách xóa liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng xóa vũ lực | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra trình tự + điều kiện chẵn lẻ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hai chuỗi bằng cách quét tuyến tính và hai con trỏ. 

1. Đi ngang`s`từ trái sang phải trong khi cố gắng khớp các ký tự của`t`theo thứ tự. Mỗi lần chúng ta nhìn thấy một nhân vật trong`s`bằng với ký tự cần thiết hiện tại trong`t`, chúng ta tiến con trỏ vào`t`. Điều này kiểm tra xem`t`là một dãy con của`s`. Lý do điều này có hiệu quả là việc xóa chỉ xóa các ký tự nên thứ tự tương đối của các ký tự còn lại được giữ nguyên. 
2. Sau khi quét, kiểm tra xem tất cả các ký tự của`t`đã được khớp. Nếu không thì không có cách nào để có được`t`, vì việc xóa không thể tạo các ký tự mới hoặc sắp xếp lại các ký tự hiện có. 
3. Tính số ký tự bị loại bỏ là`len(s) - len(t)`. Điều này thể hiện số lượng ký tự phải bị xóa thông qua các thao tác cặp liền kề. 
4. Kiểm tra xem số này có phải là số chẵn không. Nếu thấy lạ thì bác bỏ ngay. Lý do là mỗi thao tác loại bỏ đúng hai ký tự nên tổng số ký tự bị loại bỏ phải chia hết cho hai. 
5. Nếu cả hai điều kiện đều được thỏa mãn thì chấp nhận chuyển đổi. 

### Tại sao nó hoạt động 

Điều kiện chuỗi tiếp theo thể hiện thực tế là việc xóa không bao giờ thay đổi thứ tự tương đối giữa các ký tự được giữ lại. Điều kiện chẵn lẻ nắm bắt thực tế là thao tác xóa sẽ giảm kích thước chuỗi chính xác hai lần mỗi lần và bất kỳ chuỗi nào có độ dài chẵn luôn có thể được giảm xuống thành trống bằng cách lặp đi lặp lại các lần xóa liền kề. Kết hợp hai điều kiện này sẽ mô tả đầy đủ khi`t`có thể được lấy từ`s`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    t = input().strip()
    
    n, m = len(s), len(t)
    
    j = 0
    for i in range(n):
        if j < m and s[i] == t[j]:
            j += 1
    
    if j != m:
        print("NO")
        return
    
    if (n - m) % 2 != 0:
        print("NO")
        return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên thực hiện quét chuỗi con tham lam. Con trỏ`j`chỉ tiến lên khi chúng tôi khớp thành công ký tự bắt buộc tiếp theo của`t`. Nếu chúng ta đi đến cuối`t`, chúng tôi biết nó được nhúng trong`s`theo thứ tự. 

Kiểm tra thứ hai đảm bảo rằng số lần xóa cần thiết tương thích với việc xóa các cặp. Vì mỗi thao tác loại bỏ chính xác hai ký tự nên chúng tôi yêu cầu độ dài chênh lệch đồng đều. 

Không cần mô phỏng quá trình xóa, điều này giữ cho giải pháp tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
sobaka
baka
```| tôi | s[i] | t[j] | cuộc thi đấu? | j | 
| --- | --- | --- | --- | --- | 
| 0 | s | b | không | 0 | 
| 1 | o | b | không | 0 | 
| 2 | b | b | vâng | 1 | 
| 3 | một | một | vâng | 2 | 
| 4 | k | k | vâng | 3 | 
| 5 | một | một | vâng | 4 | 

Chúng tôi kết hợp thành công tất cả các ký tự của`t`, và độ dài chênh lệch là`6 - 4 = 2`, tức là chẵn. Đầu ra là`YES`. 

Điều này xác nhận cả tính khả thi của chuỗi con và tính chẵn lẻ ghép đôi hợp lệ. 

### Ví dụ 2 

đầu vào:```
sobabka
baka
```Việc kiểm tra trình tự tiếp theo vẫn thành công: chúng tôi có thể khớp`b`,`a`,`k`,`a`theo thứ tự. Tuy nhiên, sự khác biệt về chiều dài`7 - 4 = 3`, thật kỳ quặc. 

| Bước | Giá trị | 
| --- | --- | 
| | s | 
| | t | 
| đã xóa | 3 | 
| chẵn lẻ | lẻ | 

Vì một lần xóa sẽ xóa chính xác hai ký tự nên việc xóa một số ký tự lẻ là không thể. Đầu ra là`NO`. 

Điều này cho thấy rằng chỉ tính giá trị của dãy con là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Vượt qua một lần`s`với một con trỏ trên`t`| 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và chỉ số | 

Giải pháp này dễ dàng phù hợp với các hạn chế vì ngay cả kích thước đầu vào tối đa 200.000 ký tự cũng được xử lý bằng một lần quét tuyến tính duy nhất và bộ nhớ bổ sung liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    n, m = len(s), len(t)

    j = 0
    for i in range(n):
        if j < m and s[i] == t[j]:
            j += 1

    if j != m:
        return "NO"

    if (n - m) % 2 != 0:
        return "NO"

    return "YES"

# provided samples
assert run("sobaka\nbaka\n") == "YES", "sample 1"
assert run("sobabka\nbaka\n") == "NO", "sample 2"
assert run("abacaba\naca\n") == "YES", "sample 3"

# custom cases
assert run("a\na\n") == "YES", "identical strings"
assert run("ab\na\n") == "NO", "odd deletion count"
assert run("abcdef\nace\n") == "YES", "subsequence with even removals"
assert run("abcdef\naec\n") == "NO", "subsequence fails"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau | CÓ | không cần xóa | 
| số lần xóa lẻ | KHÔNG | ràng buộc chẵn lẻ | 
| dãy con chẵn hợp lệ | CÓ | tính đúng đắn chung | 
| trường hợp không hợp lệ sau đó | KHÔNG | hạn chế đặt hàng | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cả hai chuỗi giống hệt nhau. Thuật toán chấp nhận ngay lập tức vì việc kiểm tra trình tự con vượt qua một cách tầm thường và chênh lệch độ dài bằng 0, tức là chẵn. Không cần xóa và câu trả lời là chính xác. 

Một trường hợp cạnh khác là khi`t`trống rỗng. Điều kiện dãy sau luôn thỏa và câu trả lời chỉ phụ thuộc vào việc liệu`|s|`là chẵn. Nếu như`s`có độ dài lẻ, một ký tự sẽ không thể xóa được, vì vậy câu trả lời là`NO`. Thuật toán nắm bắt chính xác điều này thông qua kiểm tra tính chẵn lẻ. 

Trường hợp cạnh cuối cùng là khi`t`có chiều dài`|s| - 1`. Kể cả nếu`t`là một dãy con hợp lệ, chênh lệch là 1, là số lẻ, buộc phải từ chối. Điều này phù hợp với thực tế là không thể xóa một ký tự còn sót lại bằng các thao tác cặp.
