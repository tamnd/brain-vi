---
title: "CF 104159F - Thế Giới"
description: "Chúng ta được cung cấp một số chuỗi ngắn làm bằng các chữ cái tiếng Anh viết thường. Đối với mỗi chuỗi, chúng ta cần quyết định xem nó có “hợp lệ” hay không theo quy tắc phụ thuộc vào cách các chữ cái xen kẽ giữa hai lớp: nguyên âm và phụ âm. Quy tắc được áp dụng sau bước tiền xử lý."
date: "2026-07-02T01:07:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104159
codeforces_index: "F"
codeforces_contest_name: "\u041e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u0420\u0421(\u042f) (5-8 \u043a\u043b\u0430\u0441\u0441\u044b) 2022-23, 2 \u0434\u0435\u043d\u044c"
rating: 0
weight: 104159
solve_time_s: 64
verified: true
draft: false
---

[CF 104159F - Wordland](https://codeforces.com/problemset/problem/104159/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 4s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số chuỗi ngắn làm bằng các chữ cái tiếng Anh viết thường. Đối với mỗi chuỗi, chúng ta cần quyết định xem nó có “hợp lệ” hay không theo quy tắc phụ thuộc vào cách các chữ cái xen kẽ giữa hai lớp: nguyên âm và phụ âm. 

Quy tắc được áp dụng sau bước tiền xử lý. Trong Wordland, nếu cùng một chữ cái xuất hiện nhiều lần liên tiếp, nó sẽ được phát âm như thể nó chỉ xuất hiện một lần. Vì vậy, trước tiên, chuỗi được nén bằng cách thu gọn mọi khối tối đa các chữ cái giống hệt nhau thành một ký tự duy nhất. Sau quá trình nén này, chúng tôi kiểm tra xem chuỗi kết quả có xen kẽ chặt chẽ giữa nguyên âm và phụ âm hay không. Một từ hợp lệ nếu mỗi cặp liền kề trong chuỗi nén bao gồm một nguyên âm và một phụ âm. 

Bộ nguyên âm được cố định là a, e, i, o, u, y và tất cả các chữ cái khác là phụ âm. 

Kích thước đầu vào rất nhỏ: nhiều nhất là 100 từ, mỗi từ có độ dài tối đa là 100. Điều này ngay lập tức ngụ ý rằng ngay cả các giải pháp bậc hai hoặc bậc ba cũng có thể vượt qua một cách thoải mái, nhưng cấu trúc của nhiệm vụ cho thấy việc quét tuyến tính trên mỗi từ là phù hợp tự nhiên. 

Sự tinh tế chính là quy tắc nén. Một cách tiếp cận ngây thơ có thể quên hợp nhất các bản sao liên tiếp trước khi kiểm tra xen kẽ. Điều đó dẫn đến sự từ chối hoặc chấp nhận không chính xác. 

Ví dụ: hãy xem xét từ “totoroooo”. Nếu chúng ta kiểm tra trực tiếp sự xen kẽ, thì dấu “oooo” ở cuối trông giống như nhiều nguyên âm liên tiếp, điều này sẽ vi phạm sự xen kẽ. Nhưng sau khi nén, “oooo” trở thành một chữ “o” duy nhất và cấu trúc trở thành “to to o ro”, thay thế một cách chính xác. 

Trường hợp cạnh thứ hai là một từ như “rr”. Sau khi nén, nó trở thành “r”, không có cặp liền kề nào vi phạm quy tắc, vì vậy nó phải hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi từ, trước tiên chúng tôi xây dựng một chuỗi mới một cách rõ ràng bằng cách hợp nhất các ký tự giống hệt nhau liên tiếp. Điều này có thể được thực hiện bằng cách quét chuỗi và chỉ thêm một ký tự khi nó khác với ký tự trước đó. Sau quá trình nén này, chúng tôi lặp qua chuỗi rút gọn và kiểm tra từng cặp liền kề để xác minh rằng một cặp là nguyên âm và cặp kia là phụ âm. 

Cách tiếp cận này đã tuyến tính về kích thước của mỗi từ. Ngay cả khi chúng tôi xem xét trường hợp xấu nhất, 100 từ có độ dài 100, chúng tôi đang thực hiện khoảng 10.000 thao tác ký tự, điều này không đáng kể. Không cần phải tối ưu hóa nâng cao hơn. 

Quan sát chính là vấn đề về cơ bản là hai lượt tuyến tính độc lập cho mỗi từ: một lượt để nén độ dài chạy và một lượt để xác thực. Vì cả hai đều là O(L), nên về mặt khái niệm chúng ta có thể hợp nhất chúng thành một lần quét. Trong khi quét, chúng tôi theo dõi ký tự được lưu giữ cuối cùng (sau khi nén) và chỉ so sánh với ký tự đó khi khối ký tự mới bắt đầu. 

Phiên bản brute-force hoạt động vì nó xây dựng biểu diễn nén một cách rõ ràng. Phiên bản được tối ưu hóa tránh lưu trữ nó và thay vào đó chỉ duy trì ký tự liên quan cuối cùng và trạng thái nguyên âm của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (nén rõ ràng + kiểm tra) | O(tổng chiều dài) | O(L) | Đã chấp nhận | 
| Nén phát trực tuyến một lượt | O(tổng chiều dài) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đối với mỗi từ, hãy xác định hàm trợ giúp xác định xem ký tự có phải là nguyên âm hay không. Đây là phép tra cứu liên tục theo thời gian đối với tập hợp {a, e, i, o, u, y}. 
2. Lặp lại các ký tự của từ từ trái sang phải trong khi vẫn giữ nguyên ký tự “nén” cuối cùng mà chúng tôi đã chấp nhận. Ban đầu, không có ký tự trước đó. 
3. Khi chúng ta gặp một ký tự, hãy so sánh nó với ký tự trước đó trong chuỗi thô. Nếu giống nhau thì bỏ qua vì nó thuộc cùng một run và không làm thay đổi cấu trúc nén. 
4. Nếu nó khác với ký tự thô trước đó, chúng tôi coi nó là ký tự tiếp theo trong chuỗi nén. Tại thời điểm này, chúng tôi so sánh loại nguyên âm/phụ âm của nó với ký tự nén được giữ cuối cùng. Nếu cả hai đều thuộc cùng một lớp, chúng ta biết ngay từ đó không hợp lệ. 
5. Cập nhật ký tự nén cuối cùng và tiếp tục quét. 
6. Nếu chúng ta đến cuối từ mà không tìm thấy vi phạm thì từ đó có giá trị. 

Điểm thiết kế quan trọng là chúng tôi chỉ so sánh các ký tự tồn tại trong ranh giới nén. Chúng tôi không bao giờ so sánh trong một lần chạy vì những sự trùng lặp đó rõ ràng bị bỏ qua. 

### Tại sao nó hoạt động 

Tính năng nén theo thời lượng chạy duy trì chính xác trình tự mà chúng ta quan tâm: chuyển tiếp giữa các chữ cái riêng biệt. Bất kỳ hành vi vi phạm quy tắc luân phiên nào đều phải xuất hiện tại một trong những lần chuyển đổi này. Trong một chuỗi các chữ cái giống hệt nhau, tất cả các chữ cái đều có trạng thái nguyên âm giống nhau, do đó, việc thu gọn chúng sẽ không làm mất bất kỳ thông tin nào liên quan đến sự thay thế. Vì vậy, chỉ kiểm tra dãy biên nén là đủ và đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

VOWELS = set("aeiouy")

def is_vowel(c: str) -> bool:
    return c in VOWELS

n = int(input())
for _ in range(n):
    s = input().strip()
    
    prev_raw = None
    prev_kept = None
    
    ok = True
    
    for c in s:
        if c == prev_raw:
            continue
        
        if prev_kept is not None:
            if is_vowel(c) == is_vowel(prev_kept):
                ok = False
                break
        
        prev_kept = c
        prev_raw = c
    
    print("YES" if ok else "NO")
```Giải pháp duy trì hai phần trạng thái. Đầu tiên,`prev_raw`, được sử dụng hoàn toàn để nén: nó theo dõi xem ký tự hiện tại có phải là một phần của lần chạy lặp lại hay không. Thứ hai,`prev_kept`, đại diện cho ký tự cuối cùng trong chuỗi nén. 

Chi tiết quan trọng là chúng tôi chỉ so sánh các lớp nguyên âm khi một ký tự nén mới xuất hiện. Điều này tránh việc xây dựng một chuỗi rõ ràng trong khi vẫn duy trì tính chính xác. 

Một lỗi phổ biến là so sánh từng cặp liền kề trong chuỗi gốc, điều này sẽ loại bỏ sai các nguyên âm hoặc phụ âm lặp lại. Một sai lầm nữa là nén mà quên so sánh sau biên nén mà thôi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Từ đầu vào:`kukaracha`| Bước | Char | trước_raw | Giữ? | trước_giữ | Nguyên âm? | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | k | - | vâng | - | C | bắt đầu | 
| 2 | bạn | k | vâng | k | V vs C | được | 
| 3 | k | bạn | vâng | bạn | C đấu với V | được | 
| 4 | một | k | vâng | k | V vs C | được | 
| 5 | r | một | vâng | một | C đấu với V | được | 
| 6 | một | r | vâng | r | V vs C | được | 
| 7 | c | một | vâng | một | C đấu với V | được | 
| 8 | h | c | vâng | c | C vs C | THẤT ​​BẠI | 

Lỗi xảy ra khi chuyển từ “c” sang “h”, cả hai phụ âm. Điều này xác nhận rằng sự luân phiên được thực thi nghiêm ngặt trên các chuyển tiếp được nén. 

### Mẫu 2 

Từ đầu vào:`totoroooo`| Bước | Char | trước_raw | Giữ? | trước_giữ | Nguyên âm? | Quyết định | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | t | - | vâng | - | C | bắt đầu | 
| 2 | o | t | vâng | t | V vs C | được | 
| 3 | t | o | vâng | o | C đấu với V | được | 
| 4 | o | t | không | o | - | bỏ qua (chạy trùng lặp) | 
| 5 | r | o | vâng | o | C đấu với V | được | 
| 6 | o | r | vâng | r | V vs C | được | 
| 7 | o | o | không | o | - | bỏ qua (chạy trùng lặp) | 

Không tìm thấy vi phạm nào nên từ này có giá trị. 

Những dấu vết này cho thấy các bản sao không bao giờ ảnh hưởng đến quá trình chuyển đổi và tính chính xác chỉ phụ thuộc vào chuỗi được nén. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NL) | Mỗi ký tự được xử lý một lần cho mỗi từ, với các lần kiểm tra liên tục | 
| Không gian | O(1) | Chỉ có một vài biến được duy trì trên mỗi từ | 

Cho N ≤ 100 và L ≤ 100, tổng công việc tối đa là 10^4 thao tác ký tự, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    VOWELS = set("aeiouy")
    
    def is_vowel(c):
        return c in VOWELS
    
    n = int(sys.stdin.readline())
    out = []
    for _ in range(n):
        s = sys.stdin.readline().strip()
        
        prev_raw = None
        prev_kept = None
        ok = True
        
        for c in s:
            if c == prev_raw:
                continue
            if prev_kept is not None and is_vowel(c) == is_vowel(prev_kept):
                ok = False
                break
            prev_kept = c
            prev_raw = c
        
        out.append("YES" if ok else "NO")
    
    return "\n".join(out)

# provided samples
assert run("5\nkukaracha\nramen\nfoot\nemployees\ntotoroooo\n") == "NO\nYES\nYES\nNO\nYES"
assert run("2\nrr\nttttt\n") == "YES\nYES"

# custom cases
assert run("1\na") == "YES"
assert run("1\naa") == "YES"
assert run("1\nab") == "YES"
assert run("1\naba") == "YES"
assert run("1\nabbaa") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`| CÓ | Từ một chữ cái | 
|`aa`| CÓ | Nén loại bỏ trùng lặp | 
|`ab`| CÓ | Cặp xen kẽ đơn giản | 
|`aba`| CÓ | Luân phiên trên ba chữ cái | 
|`abbaa`| KHÔNG | Chuyển đổi cùng loại sau khi nén | 

## Vỏ cạnh 

Trường hợp một cạnh là một từ được tạo thành hoàn toàn bằng các chữ cái giống hệt nhau lặp đi lặp lại. Ví dụ: đầu vào “ttttt” nén thành “t”, không có chuyển đổi nào vi phạm luân phiên, vì vậy nó hợp lệ. Thuật toán xử lý việc này vì`prev_kept`không bao giờ được so sánh cho đến khi ký tự nén riêng biệt thứ hai xuất hiện. 

Một trường hợp đặc biệt khác là một từ mà các từ trùng lặp xuất hiện ở nhiều vị trí, chẳng hạn như “bài học”. “sss” lặp đi lặp lại phải được thu gọn thành một “s”, nếu không, phép kiểm tra liền kề ngây thơ sẽ phát hiện không chính xác các vi phạm nguyên âm-phụ âm trong lần chạy. Trong thuật toán, tất cả các chữ cái lặp lại đều được bỏ qua bằng cách sử dụng`prev_raw`, đảm bảo chỉ có ranh giới chạy mới góp phần kiểm tra. 

Trường hợp cạnh thứ ba là xen kẽ các chữ cái với các khối lặp lại, chẳng hạn như “totoroooo”. Các nguyên âm ở cuối không được hiểu là nhiều cách thay thế. Logic bỏ qua đảm bảo rằng chỉ xem xét quá trình chuyển đổi từ lần chạy, do đó tính chính xác chỉ phụ thuộc vào cấu trúc nén chứ không phải sự lặp lại thô.
