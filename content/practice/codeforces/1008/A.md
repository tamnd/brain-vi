---
title: "CF 1008A - Romaji"
description: "Chúng ta được cung cấp một từ viết thường và được yêu cầu xác minh xem nó có tuân theo quy tắc ngữ âm cụ thể hay không. Quy tắc hạn chế cách phụ âm và nguyên âm có thể xuất hiện theo thứ tự."
date: "2026-06-16T23:03:26+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1008
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 497 (Div. 2)"
rating: 900
weight: 1008
solve_time_s: 77
verified: true
draft: false
---

[CF 1008A - Romaji](https://codeforces.com/problemset/problem/1008/A) 

**Xếp hạng:** 900 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một từ viết thường và được yêu cầu xác minh xem nó có tuân theo quy tắc ngữ âm cụ thể hay không. Quy tắc hạn chế cách phụ âm và nguyên âm có thể xuất hiện theo thứ tự. 

Mọi phụ âm phải được theo sau bởi một nguyên âm, ngoại trừ một ngoại lệ: phụ âm`'n'`được phép theo sau bởi bất kỳ chữ cái nào hoặc ở cuối chuỗi. Nguyên âm được cố định như`'a'`,`'e'`,`'i'`,`'o'`, Và`'u'`. Bất kỳ chữ cái nào khác được coi là phụ âm. 

Nhiệm vụ đơn giản là quyết định xem toàn bộ chuỗi có tuân thủ ràng buộc này ở mọi nơi nó áp dụng hay không. 

Độ dài đầu vào tối đa là 100, nghĩa là chỉ cần quét trực tiếp chuỗi là đủ. Thậm chí một$O(n^2)$Cách tiếp cận ở đây có thể không quan trọng, nhưng cấu trúc gợi ý chỉ cần xác thực từ trái sang phải là đủ. 

Một số trường hợp đặc biệt rất dễ bị bỏ sót khi suy nghĩ một cách không chính thức về quy tắc. Đầu tiên là phụ âm xuất hiện ở cuối chuỗi. Ví dụ,`"abc"`kết thúc bằng`'c'`, vốn là phụ âm nên vi phạm quy tắc ngay vì không có nguyên âm theo sau. 

Một trường hợp khác là phụ âm liên tiếp. Ví dụ,`"king"`thất bại vì`'k'`được theo sau bởi`'i'`điều đó tốt thôi, nhưng`'n'`được theo sau bởi`'g'`, Và`'g'`là một phụ âm không phải là`'n'`, do đó nó yêu cầu một nguyên âm sau đó nhưng không có nguyên âm nào được đảm bảo. 

Cuối cùng,`'n'`bản thân nó là đặc biệt nhưng chỉ ở địa phương. Một chuỗi như`"nbo"`là hợp lệ bởi vì`'n'`có thể được theo sau bởi một phụ âm, nhưng điều đó không mang lại sự an toàn cho ký tự tiếp theo nếu nó không phải là phụ âm`n`phụ âm. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ kiểm tra mọi vị trí phụ âm và quét về phía trước cho đến khi tìm thấy ký tự tiếp theo để xác minh xem đó có phải là nguyên âm hay phụ âm đó không.`'n'`. Điều này hoạt động chính xác, nhưng trong trường hợp xấu nhất, nó liên tục quét lại các hậu tố, dẫn đến hành vi bậc hai. 

Vì chuỗi ở đây ngắn nên thậm chí điều đó cũng có thể vượt qua, nhưng cấu trúc của điều kiện gợi ý một bất biến rõ ràng hơn: tính hợp lệ của từng vị trí chỉ phụ thuộc vào ký tự hiện tại và ký tự tiếp theo. Không có sự phụ thuộc tầm xa nên chúng ta không cần nhìn xa trông rộng tùy tiện. 

Điều này làm giảm vấn đề xuống một đường tuyến tính duy nhất. Tại mỗi vị trí, chúng ta chỉ cần xác minh xem ký tự hiện tại có phải là phụ âm vi phạm quy tắc hay không. Nếu là nguyên âm thì không cần kiểm tra gì cả. Nếu nó là`'n'`, nó luôn an toàn. Nếu không, ký tự tiếp theo phải tồn tại và phải là nguyên âm. 

Quan sát này thu gọn tất cả các kiểm tra dư thừa và làm cho việc xác thực hoàn toàn mang tính cục bộ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu phía trước | O(n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra một lần | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ trái sang phải, xác thực từng ký tự dựa trên việc đó có phải là nguyên âm hay không,`'n'`, hoặc một phụ âm khác. 

1. Xác định bộ nguyên âm`{a, e, i, o, u}`vì vậy việc kiểm tra tư cách thành viên diễn ra liên tục. Điều này tránh so sánh chuỗi lặp đi lặp lại. 
2. Lặp qua từng chỉ mục`i`của chuỗi. 
3. Nếu ký tự hiện tại là một nguyên âm, hãy tiếp tục mà không hạn chế vì các nguyên âm không áp đặt ràng buộc nào đối với các chữ cái theo sau. 
4. Nếu ký tự hiện tại là`'n'`, cũng tiếp tục vì`'n'`được miễn quy tắc “phải tuân theo nguyên âm”. 
5. Nếu ký tự hiện tại là phụ âm khác thì phải đảm bảo có ký tự tiếp theo. Nếu như`i + 1`nằm ngoài giới hạn, chuỗi không hợp lệ ngay lập tức. 
6. Nếu có ký tự tiếp theo, hãy xác minh đó là nguyên âm. Nếu không, quy tắc bị vi phạm và chúng tôi có thể chấm dứt sớm. 

### Tại sao nó hoạt động 

Điều bất biến chính là mọi cấu hình không hợp lệ đều có thể được phát hiện ở vị trí chính xác nơi cấu hình không hợp lệ`n`phụ âm xuất hiện. Một phụ âm như vậy có yêu cầu nghiêm ngặt của địa phương đối với người kế nhiệm trực tiếp của nó. Nếu yêu cầu đó bị vi phạm thì không thể sửa chữa sau này vì quy tắc không phải về cấu trúc toàn cục mà là về tính liền kề. Vì vậy, chỉ kiểm tra ký tự tiếp theo là đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    vowels = set("aeiou")
    
    n = len(s)
    for i, c in enumerate(s):
        if c in vowels:
            continue
        if c == 'n':
            continue
        
        # consonant that must be followed by vowel
        if i + 1 >= n or s[i + 1] not in vowels:
            print("NO")
            return
    
    print("YES")

if __name__ == "__main__":
    solve()
```Mã phản ánh thuật toán trực tiếp. Bộ nguyên âm được sử dụng để kiểm tra tư cách thành viên nhanh chóng, đảm bảo xác thực thời gian liên tục cho mỗi ký tự. Việc thoát sớm khi xảy ra lỗi sẽ tránh được việc quét không cần thiết sau khi phát hiện vi phạm. 

Một điểm tinh tế là kiểm tra ranh giới`i + 1 >= n`. Không có nó, truy cập`s[i + 1]`sẽ báo lỗi khi một phụ âm xuất hiện ở cuối chuỗi. Đây chính xác là trường hợp làm mất hiệu lực các từ như`"king"`. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`sumimasen`| tôi | char | gõ | ký tự tiếp theo | hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | s | phụ âm | bạn | được | 
| 1 | bạn | nguyên âm | - | được | 
| 2 | m | phụ âm | tôi | được | 
| 3 | tôi | nguyên âm | - | được | 
| 4 | m | phụ âm | một | được | 
| 5 | một | nguyên âm | - | được | 
| 6 | s | phụ âm | e | được | 
| 7 | e | nguyên âm | - | được | 
| 8 | n | đặc biệt | kết thúc | được | 

Điều này khẳng định rằng mọi không`n`phụ âm được theo sau bởi một nguyên âm và`'n'`xuất hiện an toàn ở cuối. 

### Ví dụ 2:`king`| tôi | char | gõ | ký tự tiếp theo | hợp lệ cho đến nay | 
| --- | --- | --- | --- | --- | 
| 0 | k | phụ âm | tôi | được | 
| 1 | tôi | nguyên âm | - | được | 
| 2 | n | đặc biệt | g | được | 
| 3 | g | phụ âm | kết thúc | thất bại | 

Ở chỉ số 3,`'g'`là phụ âm không có ký tự đứng sau, vi phạm quy tắc ngay. 

Dấu vết này cho thấy lỗi được phát hiện chính xác ở phần kề không hợp lệ đầu tiên mà không cần kiểm tra phần còn lại của chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được xử lý một lần với các lần kiểm tra liên tục | 
| Không gian | O(1) | Chỉ sử dụng một bộ nguyên âm cố định và các biến vòng lặp | 

Kích thước đầu vào tối đa là 100, do đó quá trình quét tuyến tính này có hiệu quả tức thời trong điều kiện hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from sys import stdout
    backup = stdout
    sys.stdout = io.StringIO()
    
    def solve():
        s = input().strip()
        vowels = set("aeiou")
        
        n = len(s)
        for i, c in enumerate(s):
            if c in vowels:
                continue
            if c == 'n':
                continue
            if i + 1 >= n or s[i + 1] not in vowels:
                print("NO")
                return
        print("YES")
    
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# provided sample
assert run("sumimasen\n") == "YES"

# single vowel
assert run("a\n") == "YES"

# single consonant (invalid)
assert run("b\n") == "NO"

# valid with n at end
assert run("man\n") == "YES"

# invalid consecutive consonants
assert run("king\n") == "NO"

# boundary case: consonant at end
assert run("abc\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`"a"`| CÓ | chuỗi hợp lệ tối thiểu | 
|`"b"`| KHÔNG | trường hợp cạnh phụ âm đơn | 
|`"man"`| CÓ | xử lý đặc biệt`'n'`| 
|`"king"`| KHÔNG | lỗi phụ âm liên tiếp | 
|`"abc"`| KHÔNG | phụ âm ở ranh giới cuối | 

## Vỏ cạnh 

Một chuỗi ký tự đơn như`"n"`là hợp lệ bởi vì`'n'`được miễn trừ rõ ràng khỏi việc cần một nguyên âm theo sau. Vòng lặp xử lý nó một lần, phân loại nó thành trường hợp đặc biệt và chấp nhận nó mà không có bất kỳ vấn đề giới hạn nào. 

Một chuỗi kết thúc bằng một ký tự không phải`n`phụ âm như`"abc"`thất bại ở ký tự cuối cùng. Khi`i`đạt đến chỉ số cuối cùng, điều kiện`i + 1 >= n`gây ra sự từ chối ngay lập tức. Điều này nắm bắt quy tắc cốt lõi rằng mọi phụ âm không đặc biệt phải được theo sau bởi một ký tự khác và ký tự đó phải là nguyên âm. 

Một loạt các phụ âm như`"bcd"`đã thất bại ở ký tự đầu tiên rồi, vì`'b'`được theo sau bởi`'c'`, không phải là nguyên âm và cũng không được miễn trừ. Thuật toán kết thúc sớm, cho thấy các vi phạm được phát hiện ở vị trí sớm nhất có thể mà không cần duyệt toàn bộ.
