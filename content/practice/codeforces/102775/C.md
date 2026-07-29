---
title: "CF 102775C - \u0422\u0430\u043a\u0438\u0435 \u0440\u0430\u0437\u043d\u044b\u0435 \u0441\u0442\u0440\u043e\u043a\u0438"
description: "Chúng ta được cấp một chuỗi chữ Latinh viết thường. Một chuỗi được coi là tốt nếu nó không bao giờ chứa ba nguyên âm liên tiếp và không bao giờ chứa ba phụ âm liên tiếp. Nếu một trong hai trường hợp này xuất hiện ở bất kỳ đâu trong chuỗi thì chuỗi đó bị hỏng."
date: "2026-07-28T03:03:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "C"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 77
verified: true
draft: false
---

[CF 102775C - \u0422\u0430\u043a\u0438\u0435 \u0440\u0430\u0437\u043d\u044b\u0435 \u0441\u0442\u0440\u043e\u043a\u0438](https://codeforces.com/problemset/problem/102775/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi chữ Latinh viết thường. Một chuỗi được coi là tốt nếu nó không bao giờ chứa ba nguyên âm liên tiếp và không bao giờ chứa ba phụ âm liên tiếp. Nếu một trong hai trường hợp này xuất hiện ở bất kỳ đâu trong chuỗi thì chuỗi đó bị hỏng. 

Nhiệm vụ chỉ đơn giản là xác định xem chuỗi đầu vào thuộc về loại nào. Nếu tồn tại một chuỗi bị cấm, hãy in`BAD`. Ngược lại, in`GOOD`. 

Độ dài chuỗi tối đa là`100000`, điều này ngay lập tức gợi ý rằng mỗi ký tự chỉ nên được xử lý một lần. Một thuật toán quét lại nhiều lần các phần của chuỗi, chẳng hạn như kiểm tra từng chuỗi con một cách độc lập, sẽ thực hiện quá nhiều thao tác trong trường hợp xấu nhất. Mặt khác, quét tuyến tính chỉ thực hiện khoảng một trăm nghìn kiểm tra ký tự, dễ dàng nằm trong giới hạn. 

Một số trường hợp đáng chú ý. 

Một chuỗi ngắn hơn ba ký tự không bao giờ có thể chứa ba nguyên âm hoặc phụ âm liên tiếp. Ví dụ:```
ab
```Đầu ra đúng là:```
GOOD
```Việc triển khai bất cẩn, kiểm tra bộ ba một cách mù quáng mà không kiểm tra độ dài có thể truy cập các ký tự bên ngoài chuỗi. 

Trình tự phải liên tiếp. Ví dụ:```
ababa
```Đầu ra đúng là:```
GOOD
```Mặc dù tổng thể có nhiều nguyên âm và nhiều phụ âm nhưng chúng liên tục bị gián đoạn nên không có chuỗi nào dài đến ba. 

Các lần chạy có thể xuất hiện ở đầu hoặc cuối chuỗi. Ví dụ:```
aaab
```Đầu ra đúng là:```
BAD
```Tương tự,```
baaa
```cũng là`BAD`. Việc triển khai chỉ kiểm tra phần giữa của chuỗi có thể bỏ sót những trường hợp này. 

lá thư`y`là một nguyên âm trong bài toán này. Ví dụ:```
yyy
```phải sản xuất:```
BAD
```điều trị`y`như một phụ âm sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra từng nhóm ba ký tự liên tiếp. Đối với mỗi bộ ba, hãy xác định xem cả ba đều là nguyên âm hay cả ba đều là phụ âm. Nếu một trong hai điều kiện được giữ, câu trả lời là ngay lập tức`BAD`; nếu không thì tiếp tục cho đến hết chuỗi. 

Phương pháp này đã đúng vì mọi mẫu bị cấm đều bao gồm chính xác ba chữ cái liên tiếp. Bất kỳ nguyên âm nào dài hơn, chẳng hạn như bốn nguyên âm liên tiếp, nhất thiết phải chứa một bộ ba bên trong nó. 

Một ý tưởng kém hiệu quả hơn nhiều là liệt kê mọi chuỗi con và kiểm tra xem nó có chứa ba nguyên âm hoặc phụ âm liên tiếp hay không. Một chuỗi có độ dài`100000`có khoảng`5 × 10^9`chuỗi con, làm cho cách tiếp cận như vậy hoàn toàn không thực tế. 

Quan sát quan trọng là chỉ có lần chạy liên tiếp hiện tại mới quan trọng. Bất cứ khi nào ký tự hiện tại có cùng loại, nguyên âm hoặc phụ âm với ký tự trước đó, độ dài chạy sẽ tăng lên. Nếu không thì độ dài chạy sẽ đặt lại thành một. Ngay khi một trong hai lượt chạy đến thứ ba, chúng tôi đã biết câu trả lời là`BAD`và không cần xử lý thêm. 

Điều này cho phép chuỗi được xử lý theo một lượt từ trái sang phải chỉ bằng một vài biến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các nguyên âm trong một bộ chứa`a`,`e`,`i`,`o`,`u`, Và`y`. Kiểm tra tư cách thành viên trong một bộ là thời gian không đổi. 
2. Khởi tạo hai bộ đếm, một bộ đếm cho các nguyên âm liên tiếp hiện tại và một bộ đếm cho các phụ âm liên tiếp hiện tại. Cả hai đều bắt đầu từ con số 0. 
3. Quét chuỗi từ trái sang phải. 
4. Nếu ký tự hiện tại là nguyên âm, hãy tăng bộ đếm nguyên âm lên một và đặt lại bộ đếm phụ âm về 0. Điều này phản ánh rằng mọi phụ âm đều bị gián đoạn. 
5. Nếu ký tự hiện tại là phụ âm, hãy tăng bộ đếm phụ âm lên một và đặt lại bộ đếm nguyên âm về 0. 
6. Sau khi cập nhật bộ đếm, hãy kiểm tra xem một trong hai bộ đếm đã đạt tới ba chưa. Nếu có thì in`BAD`ngay lập tức vì một cuộc chạy bị cấm đã được tìm thấy. 
7. Nếu quá trình quét kết thúc mà bộ đếm không đạt tới ba, hãy in`GOOD`. 

### Tại sao nó hoạt động 

Ở mọi vị trí, bộ đếm nguyên âm bằng độ dài của hậu tố hiện tại bao gồm toàn bộ các nguyên âm liên tiếp và bộ đếm phụ âm bằng độ dài của hậu tố hiện tại bao gồm toàn bộ các phụ âm liên tiếp. Các giá trị này được duy trì chính xác vì chính xác một bộ đếm tăng lên trong khi bộ đếm kia được đặt lại bất cứ khi nào một ký tự mới được xử lý. Vì mọi mẫu bị cấm chính xác là một chuỗi gồm ít nhất ba chữ cái liên tiếp cùng loại, nên việc đạt được giá trị bộ đếm bằng ba vừa cần thiết vừa đủ để khai báo chuỗi xấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    vowels = set("aeiouy")

    vowel_run = 0
    consonant_run = 0

    for ch in s:
        if ch in vowels:
            vowel_run += 1
            consonant_run = 0
        else:
            consonant_run += 1
            vowel_run = 0

        if vowel_run >= 3 or consonant_run >= 3:
            print("BAD")
            return

    print("GOOD")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc tạo ra một tập hợp các nguyên âm sao cho mỗi ký tự có thể được phân loại theo thời gian không đổi. 

Hai bộ đếm đại diện cho độ dài chạy hiện tại. Chính xác một trong số chúng phát triển sau mỗi lần lặp, trong khi cái còn lại được đặt lại vì quá trình chạy hiện tại đã bị gián đoạn. 

Việc kiểm tra độ dài ba xảy ra ngay sau khi cập nhật bộ đếm. Điều này cho phép chương trình kết thúc sớm ngay khi biết câu trả lời. 

sử dụng`strip()`xóa dòng mới ở cuối khỏi đầu vào trong khi vẫn giữ nguyên chuỗi thực tế. Không sử dụng chỉ mục nên không có vấn đề về ranh giới khi chuỗi có ít hơn ba ký tự. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
good
```| Nhân vật | Loại | Nguyên âm Chạy | Chạy phụ âm | Quyết định | 
| --- | --- | --- | --- | --- | 
| g | Phụ âm | 0 | 1 | Tiếp tục | 
| o | Nguyên âm | 1 | 0 | Tiếp tục | 
| o | Nguyên âm | 2 | 0 | Tiếp tục | 
| d | Phụ âm | 0 | 1 | Tiếp tục | 

Quá trình quét kết thúc mà bộ đếm không đạt tới ba, vì vậy câu trả lời là`GOOD`. Điều này chứng tỏ rằng việc chuyển đổi giữa các nguyên âm và phụ âm sẽ đặt lại chính xác bộ đếm ngược lại. 

### Mẫu 2 

đầu vào:```
bad
```| Nhân vật | Loại | Nguyên âm Chạy | Chạy phụ âm | Quyết định | 
| --- | --- | --- | --- | --- | 
| b | Phụ âm | 0 | 1 | Tiếp tục | 
| một | Nguyên âm | 1 | 0 | Tiếp tục | 
| d | Phụ âm | 0 | 1 | Tiếp tục | 

Không bộ đếm nào đạt tới số ba, vì vậy câu trả lời là`GOOD`. Ví dụ này xác nhận rằng các nguyên âm và phụ âm biệt lập không tích lũy khi bị gián đoạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý chính xác một lần. | 
| Không gian | O(1) | Chỉ một bộ nguyên âm có kích thước cố định và hai bộ đếm được lưu trữ. | 

Thuật toán thực hiện một lần truyền qua chuỗi và sử dụng bộ nhớ bổ sung không đổi. Điều này dễ dàng thỏa mãn các ràng buộc ngay cả đối với các chuỗi có độ dài`100000`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    s = input().strip()
    vowels = set("aeiouy")

    vowel_run = 0
    consonant_run = 0

    for ch in s:
        if ch in vowels:
            vowel_run += 1
            consonant_run = 0
        else:
            consonant_run += 1
            vowel_run = 0

        if vowel_run >= 3 or consonant_run >= 3:
            print("BAD")
            return

    print("GOOD")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout
    return out

# provided samples
assert run("good\n") == "GOOD", "sample 1"
assert run("bad\n") == "GOOD", "sample 2"
assert run("zashtsheeshtschayjushtsheekhsya\n") == "BAD", "sample 3"
assert run("dlinnosheee\n") == "BAD", "sample 4"

# custom cases
assert run("a\n") == "GOOD", "single character"
assert run("yyy\n") == "BAD", "y is a vowel"
assert run("bcdf\n") == "BAD", "four consecutive consonants"
assert run(("ab" * 50000) + "\n") == "GOOD", "maximum length alternating pattern"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`GOOD`| Độ dài tối thiểu có thể | 
|`yyy`|`BAD`| Điều trị đúng`y`như một nguyên âm | 
|`bcdf`|`BAD`| Phát hiện các phụ âm dài | 
|`ab`lặp lại 50000 lần |`GOOD`| Kích thước đầu vào tối đa không bị cấm chạy | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
ab
```Các quá trình thuật toán`a`, sau đó`b`. Bộ đếm trở thành`(1, 0)`và sau đó`(0, 1)`. Không đạt đến ba, vì vậy đầu ra là`GOOD`. Vì thuật toán không bao giờ giả sử chuỗi có ít nhất ba ký tự nên không có vấn đề về ranh giới. 

Bây giờ hãy xem xét:```
ababa
```Các bộ đếm xen kẽ giữa một nguyên âm và một phụ âm trong suốt quá trình quét. Mỗi thay đổi kiểu sẽ đặt lại bộ đếm ngược lại, do đó không lần chạy nào vượt quá một. Đầu ra là chính xác`GOOD`. 

Đối với đầu vào:```
aaab
```Bộ đếm nguyên âm phát triển như`1`,`2`,`3`. Thời điểm nó đạt tới ba, thuật toán sẽ in ngay lập tức`BAD`mà không quét các ký tự còn lại. Điều này phát hiện chính xác tiền tố bị cấm. 

Cuối cùng, hãy xem xét:```
yyy
```Từ`y`thuộc bộ nguyên âm, bộ đếm nguyên âm trở thành`1`,`2`,`3`và kết quả đầu ra của thuật toán`BAD`. Điều này phù hợp với định nghĩa của nguyên âm trong bài toán và tránh được lỗi thường gặp khi xử lý`y`như một phụ âm.
