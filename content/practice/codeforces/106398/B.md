---
title: "CF 106398B - \u0423\u0442\u0440\u0435\u043d\u043d\u044f\u044f \u043f\u0435\u0441\u043d\u044f \u0445\u043e\u043c\u044f\u043a\u043e\u0432"
description: "Chúng ta được cung cấp một chuỗi chữ hoa dài thể hiện bản ghi âm buổi biểu diễn của dàn hợp xướng. Mỗi chú chuột hamster trong dàn hợp xướng có một “bài hát” riêng và mỗi bài hát dài đúng hai nhân vật."
date: "2026-06-20T12:36:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106398
codeforces_index: "B"
codeforces_contest_name: "\u041e\u0442\u0431\u043e\u0440 \u043d\u0430 \u0412\u041a\u041e\u0428\u041f.Junior 2026"
rating: 0
weight: 106398
solve_time_s: 49
verified: true
draft: false
---

[CF 106398B - \u0423\u0442\u0440\u0435\u043d\u043d\u044f\u044f \u043f\u0435\u0441\u043d\u044f \u0445\u043e\u043c\u044f\u043a\u043e\u0432](https://codeforces.com/problemset/problem/106398/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ hoa dài thể hiện bản ghi âm buổi biểu diễn của dàn hợp xướng. Mỗi chú chuột hamster trong dàn hợp xướng có một “bài hát” riêng và mỗi bài hát dài đúng hai nhân vật. Dàn đồng ca hát theo một trật tự tuần hoàn nghiêm ngặt: đầu tiên chú hamster 1 hát bài hát có hai chữ cái, sau đó là chú hamster 2, v.v. cho đến chú hamster cuối cùng, sau đó trình tự lặp lại một lần nữa từ chú hamster 1. 

Trong khi hệ thống hoạt động chính xác, chuỗi đã ghi sẽ được hình thành bằng cách ghép các bài hát gồm hai chữ cái này theo chu kỳ đầy đủ. Tuy nhiên, nếu tại một thời điểm nào đó, hamster không bắt đầu lượt của mình kịp thời, quá trình này sẽ dừng ngay lập tức và không có chu kỳ hót hợp lệ nào tiếp tục được tiếp tục. Do đó, chuỗi được ghi là tiền tố của một số chu kỳ đầy đủ theo thứ tự cố định của các chuỗi hai chữ cái chưa xác định. 

Nhiệm vụ là xác định số lượng chuột đồng tối thiểu trong dàn hợp xướng có thể tạo ra bản ghi âm nhất định theo các quy tắc này. 

Độ dài chuỗi tối đa là 4000, loại trừ mọi thứ tệ hơn khoảng O(n^2) hoặc có thể là O(n sqrt n) tùy thuộc vào hằng số. Bất cứ điều gì theo cấp số nhân hoặc liên quan đến việc tái cấu trúc chuỗi con lặp đi lặp lại sẽ quá chậm hoặc không cần thiết. 

Một trường hợp phức tạp xuất phát từ sự mơ hồ trong việc căn chỉnh chu trình. Ví dụ: nếu chuỗi chứa các mẫu lặp lại như “AA”, thì rất dễ nhầm lẫn khi cho rằng có nhiều chuột hamster tham gia khi một con chuột lặp lại bài hát của nó cũng sẽ tạo ra kết quả tương tự. 

Một trường hợp cạnh khác là khi chuỗi có thể được giải thích bằng một chu trình lớn hơn nhưng cũng bằng một chu trình nhỏ hơn. Ví dụ: cấu trúc lặp lại hoàn hảo như “ABABABAB” có thể đến từ một con hamster lặp lại “AB” hoặc từ hai con hamster “AB” và “AB” ở các vị trí khác nhau, nhưng tính độc đáo của các bài hát ngăn cản các bài hát giống nhau giữa các con hamster, vì vậy việc phân tách như vậy phải được kiểm tra cẩn thận. 

## Phương pháp tiếp cận 

Việc ghi âm có thể được hiểu là sự kết hợp của các chu kỳ, trong đó mỗi chu kỳ bao gồm k khối hai chữ cái, theo thứ tự một khối cho mỗi con hamster. Nếu chúng ta chia chuỗi thành các khối có kích thước 2, chúng ta sẽ thu được một chuỗi các cặp và quá trình này tương đương với việc đọc các cặp này theo chu kỳ. 

Một ý tưởng mạnh mẽ là thử tất cả các giá trị có thể có của k từ 1 đến n/2, diễn giải chuỗi dưới dạng một chuỗi k vị trí và kiểm tra xem liệu nó có thể được giải thích một cách nhất quán hay không. Đối với k cố định, chúng ta gán cho mỗi vị trí i mod k một giá trị hai chữ cái bắt buộc. Khi chúng tôi quét chuỗi, bất cứ khi nào một vị trí được chỉ định, nó phải nhất quán; nếu không thì k này không hợp lệ. Việc kiểm tra này là tuyến tính theo số khối. Vì có O(n) ứng cử viên cho k và mỗi kiểm tra là O(n), điều này dẫn đến O(n^2), là đường biên nhưng vẫn có thể chấp nhận được đối với n lên tới 2000 hoặc 4000. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng các “chuột đồng” thực tế ngoài việc nhóm chuỗi thành các cặp liên tiếp và kiểm tra tính nhất quán định kỳ. Mỗi ứng cử viên k chỉ áp đặt một ràng buộc: tất cả các lần xuất hiện của chỉ mục i, i + k, i + 2k, v.v. phải ánh xạ tới cùng một vị trí khối hai chữ cái. Điều này làm giảm vấn đề tìm k nhỏ nhất mà tại đó tất cả các vị trí nhất quán theo modulo k xác định một phép gán hợp lệ. 

Do đó, chúng ta có thể kiểm tra trực tiếp k từ 1 trở lên và dừng ở giá trị hợp lệ đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm trong trường hợp xấu nhất | 
| Tối ưu | O(n^2) trường hợp xấu nhất, thoát sớm trong thực tế | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta chuyển chuỗi thành một danh sách các đoạn gồm hai chữ cái. Vì mỗi bài hát của hamster đều có độ dài chính xác là 2 nên đơn vị nguyên tử tự nhiên của quá trình này là các cặp này.

1. Chia chuỗi thành m = n/2 cặp, trong đó cặp i là s[2i : 2i+2]. Điều này chuyển vấn đề thành giải quyết một chuỗi ký hiệu thay vì ký tự thô. Lý do điều này hợp lệ là vì các lượt quay của hamster luôn tạo ra các khối có đầy đủ hai chữ cái. 
2. Hãy thử từng số hamster có thể có k từ 1 đến m. Mỗi k đại diện cho giả thuyết rằng có k vị trí trong chu trình. 
3. Đối với k cố định, hãy duy trì một mảng có kích thước k để lưu trữ cặp cần thiết cho mỗi ô hamster. Ban đầu tất cả các vị trí đều trống. 
4. Lặp lại tất cả các cặp theo thứ tự. Với mỗi cặp tại chỉ số i, hãy tính vị trí j = i mod k của nó. Nếu khe j trống, hãy gán nó. Nếu nó đã được chỉ định thì nó phải khớp chính xác với cặp hiện tại; nếu không thì k này không hợp lệ và chúng tôi ngừng kiểm tra nó. 
5. Nếu chúng tôi quét xong tất cả các cặp mà không có mâu thuẫn, hãy trả về k ngay lập tức vì chúng tôi đang tìm kiếm số lượng chuột đồng tối thiểu có thể. 

Ý tưởng chính là mỗi vị trí trong chu kỳ phải hoạt động nhất quán trong tất cả các chu kỳ ghi âm, vì mỗi chú chuột hamster luôn hát cùng một bài hát có hai chữ cái ở vị trí của nó theo thứ tự. 

### Tại sao nó hoạt động 

Đối với bất kỳ kích thước hợp xướng k hợp lệ nào, mọi vị trí i trong chuỗi cặp đều thuộc về chính xác một lớp dư lượng modulo k. Trong mỗi lớp, tất cả các cặp phải giống hệt nhau vì chúng đến từ cùng một chú hamster lặp lại bài hát cố định của nó trong mỗi chu kỳ. Nếu bất kỳ lớp nào có hai cặp khác nhau thì việc gán các bài hát duy nhất không nhất quán có thể giải thích được bản ghi âm, vì vậy k là không thể. Ngược lại, nếu tất cả các lớp dư lượng đều nhất quán, chúng ta có thể gán cho mỗi vị trí hamster cặp được quan sát và xây dựng lại mẫu hát tuần hoàn hợp lệ khớp với toàn bộ tiền tố. Điều này chứng tỏ rằng việc kiểm tra tính nhất quán theo modulo k là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    
    # convert into 2-letter blocks
    blocks = []
    for i in range(0, n, 2):
        blocks.append(s[i:i+2])
    
    m = len(blocks)
    
    for k in range(1, m + 1):
        slots = [None] * k
        ok = True
        
        for i, b in enumerate(blocks):
            j = i % k
            if slots[j] is None:
                slots[j] = b
            elif slots[j] != b:
                ok = False
                break
        
        if ok:
            print(k)
            return

solve()
```Giải pháp này hoạt động trực tiếp trên biểu diễn cặp, giúp tránh phải xử lý nhiều lần các vấn đề căn chỉnh cấp ký tự. Việc lập chỉ mục modulo thực thi cấu trúc tuần hoàn của các lượt quay của hamster. Điểm tinh tế duy nhất là chúng ta phải so sánh các khối gồm hai ký tự đầy đủ chứ không phải các ký tự riêng lẻ, bởi vì danh tính của mỗi chú chuột hamster được xác định bởi toàn bộ bài hát của nó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
AOAUAYOOAOAU
```Khối: 

| tôi | khối | k=1 | k=2 | k=3 | k=4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | AO | AO | AO | AO | AO | 
| 1 | AU | AO→AU | AU | AU | AU | 
| 2 | AY | - | AO≠AY | AY | AY | 
| 3 | OO | - | AU | AO≠OO | OO | 
| 4 | AO | - | AO | AU≠AO | AO | 
| 5 | AU | - | AU | AY≠AU | AU | 

K hợp lệ đầu tiên là 4, vì vậy câu trả lời là 4. 

Điều này cho thấy các giả định về chu trình nhỏ hơn sẽ thất bại nhanh chóng như thế nào do các nhiệm vụ xung đột trong các lớp dư lượng, trong khi k = 4 phân tách rõ ràng tất cả các mẫu được quan sát. 

### Ví dụ 2 

đầu vào:```
AAAA
```Khối: 

| tôi | khối | k=1 | k=2 | 
| --- | --- | --- | --- | 
| 0 | AA | AA | AA | 
| 1 | AA | AA | AA | 

k = 1 có tác dụng ngay lập tức nên đáp án là 1. 

Điều này chứng tỏ rằng ngay cả khi chuỗi dài, một con hamster lặp lại là đủ khi tất cả các khối giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m^2) | Với mỗi k đến m, chúng tôi quét tất cả m khối và thực hiện kiểm tra O(1) | 
| Không gian | O(m) | Chúng tôi lưu trữ k slot và mảng khối | 

Với m 2000, trường hợp xấu nhất là khoảng 4 triệu phép so sánh dễ dàng phù hợp trong một giây trong Python, đặc biệt vì các phép so sánh là các chuỗi ngắn có độ dài cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.readline().strip()

# helper wrapper around solution
def solve_wrapper(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert solve_wrapper("AOAUAYOOAOAU\n") == "4"

# single hamster
assert solve_wrapper("AAAA\n") == "1"

# two hamsters alternating
assert solve_wrapper("ABCDABCD\n") == "2"

# minimal case
assert solve_wrapper("AB\n") == "1"

# conflicting early failure
assert solve_wrapper("ABAC\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| AB | 1 | ranh giới tối thiểu | 
| AAAA | 1 | sự lặp lại đầy đủ sụp đổ | 
| ABCDABCD | 2 | cấu trúc tuần hoàn sạch | 
| ABAC | 2 | mâu thuẫn ban đầu cho k=1 | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các khối đều giống hệt nhau. Đối với đầu vào như “AAAAAAAA”, mọi k từ 1 đến m đều hợp lệ nhưng tối thiểu là 1. Thuật toán trả về chính xác 1 vì k=1 chuyển tính nhất quán ngay lập tức với một vị trí duy nhất. 

Một trường hợp khác là khi xung đột xuất hiện rất muộn. Đối với một chuỗi trong đó một số khối đầu tiên vừa với một k nhỏ nhưng khối sau phá vỡ nó, thuật toán vẫn từ chối chính xác k đó vì nó kiểm tra tất cả các lần xuất hiện chứ không chỉ căn chỉnh ban đầu. Ví dụ: “AB AB AB AC” (dưới dạng khối) sẽ chỉ thất bại k=1 khi đạt đến khối không nhất quán cuối cùng, đảm bảo tính chính xác. 

Trường hợp cạnh thứ ba là khi tồn tại nhiều k hợp lệ. Thứ tự vòng lặp đảm bảo rằng k nhất quán đầu tiên được trả về, phù hợp với yêu cầu về số lượng chuột đồng tối thiểu.
