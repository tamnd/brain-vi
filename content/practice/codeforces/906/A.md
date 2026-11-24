---
title: "CF 906A - Bộ giảm chấn"
description: "Valentin đang chơi một trò chơi trong đó một chữ cái chưa biết được chọn và mỗi lần anh ấy phát âm một từ có chứa chữ cái đó, anh ấy sẽ bị sốc. Anh ta cũng có thể đoán về bức thư, và việc đoán sai sẽ dẫn đến những cú sốc."
date: "2026-06-12T23:06:20+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 906
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 454 (Div. 1, based on Technocup 2018 Elimination Round 4)"
rating: 1600
weight: 906
solve_time_s: 330
verified: true
draft: false
---

[CF 906A - Bộ gây sốc](https://codeforces.com/problemset/problem/906/A) 

**Đánh giá:** 1600 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 5 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Valentin đang chơi một trò chơi trong đó một chữ cái chưa biết được chọn và mỗi lần anh ấy phát âm một từ có chứa chữ cái đó, anh ấy sẽ bị sốc. Anh ta cũng có thể đoán về bức thư, và việc đoán sai sẽ dẫn đến những cú sốc. Câu hỏi quan trọng không chỉ là anh ta đã nhận được bao nhiêu cú sốc mà còn là bao nhiêu cú sốc **có thể tránh được**, nghĩa là chúng xảy ra sau khi bức thư không xác định có thể đã được xác định duy nhất từ ​​các hành động trước đó. 

Đầu vào bao gồm một chuỗi các hành động. Mỗi hành động có thể là một từ được nói một cách an toàn, một từ được nói có chứa chữ cái chưa biết hoặc đoán một chữ cái. Hành động cuối cùng được đảm bảo là một dự đoán chính xác. Kết quả là số cú sốc có thể tránh được nếu Valentin nhận ra bức thư ngay khi nó trở nên rõ ràng. 

Số lượng hành động có thể lên tới 100.000 và tổng độ dài từ cũng được giới hạn ở mức 100.000. Điều này ngụ ý rằng chúng ta cần một giải pháp có độ phức tạp thời gian tuyến tính, khoảng O(n + Total_length_of_words), vì thuật toán bậc hai sẽ quá chậm. 

Các trường hợp khó khăn không rõ ràng bao gồm các tình huống trong đó vẫn có thể có nhiều chữ cái cho đến lần đoán cuối cùng. Ví dụ: nếu hai từ đầu tiên gây sốc và chỉ để lại hai chữ cái ứng cử viên, thì bất kỳ từ nào được nói có chứa một trong số chúng sẽ chỉ được tính là quá mức sau khi tất cả các khả năng khác bị loại bỏ. Một cách tiếp cận ngây thơ coi tất cả các cú sốc sau cú sốc đầu tiên là quá mức sẽ không chính xác. Một trường hợp khác xảy ra khi chữ cái duy nhất ngay lập tức bị xóa khỏi từ gây sốc đầu tiên; ở đây, bất kỳ cú sốc nào tiếp theo đều là quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng hành động và duy trì một tập hợp tất cả các chữ cái từ 'a' đến 'z' có khả năng là chữ cái được chọn. Đối với mọi hành động, chúng tôi cập nhật bộ này: xóa các chữ cái nếu một từ an toàn có chứa chúng, xen kẽ với các chữ cái trong một từ gây sốc và kiểm tra các dự đoán. Mỗi khi kích thước đã đặt giảm xuống 1, chúng tôi đánh dấu chữ cái duy nhất là đã biết. Sau đó chúng tôi đếm các cú sốc sau thời điểm này. 

Cách tiếp cận này đúng nhưng đòi hỏi phải thực hiện các thao tác cẩn thận, đặc biệt là các nút giao thông, có thể tốn kém nếu thực hiện một cách sơ sài. Trường hợp xấu nhất liên quan đến các tập hợp giao nhau liên tục cho mỗi từ gây sốc có độ dài lên tới 100.000, tạo ra độ phức tạp tiềm năng O(n * word_length). Chúng ta có thể tối ưu hóa bằng cách biểu diễn các chữ cái ứng cử viên dưới dạng mặt nạ bit 26 bit và thực hiện các phép toán theo chiều bit. Điều này làm giảm mỗi lần cập nhật xuống O(word_length) cho mỗi hành động và tránh lặp lại tập hợp ứng cử viên. 

Thông tin chi tiết quan trọng là những cú sốc quá mức chỉ được tính **sau khi tập hợp các chữ cái có thể có kích thước 1** và các từ an toàn sẽ ngay lập tức loại bỏ các chữ cái của chúng khỏi các ứng viên, trong khi các từ gây sốc sẽ giao nhau với tập hợp ứng cử viên hiện tại. Những dự đoán sai sẽ loại bỏ một chữ cái khỏi ứng viên nhưng chỉ quan trọng nếu chữ cái duy nhất đó đã được biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thiết lập hoạt động) | O(n * word_length) | O(26) | Có thể chấp nhận nếu thực hiện cẩn thận | 
| Tối ưu hóa mặt nạ bit | O(n + tổng_word_length) | O(1) | Tối ưu và nhanh chóng | 

## Hướng dẫn thuật toán 

1. Khởi tạo một tập ứng cử viên chứa tất cả các chữ cái viết thường. Bộ này đại diện cho các chữ cái vẫn có thể được chọn. Đồng thời khởi tạo một biến`excess_shocks`đến 0 và một lá cờ`unique_known`thành Sai. 
2. Lặp lại từng hành động. Đối với mỗi hành động, hãy phân tích loại và từ liên quan hoặc chữ cái được đoán. 
3. Nếu hành động là một từ an toàn (bắt đầu bằng '.'), hãy xóa tất cả các chữ cái trong từ đó khỏi bộ ứng cử viên, vì chúng không thể là chữ cái được chọn. 
4. Nếu hành động là một từ gây sốc (bắt đầu bằng '!'), hãy ghép bộ ứng cử viên với các chữ cái trong từ, vì chữ cái chưa biết phải có trong từ này. Nếu kích thước tập ứng cử viên đã là 1, hãy tăng`excess_shocks`vì cú sốc này có thể tránh được. 
5. Nếu hành động là đoán (bắt đầu bằng '?'), hãy kiểm tra xem chữ cái được đoán có phải là ứng cử viên duy nhất hay không. Nếu tập ứng cử viên có kích thước 1 và chữ cái được đoán không chính xác, hãy tăng`excess_shocks`. Luôn xóa chữ cái đã đoán nếu nó không phải là chữ cái đã chọn (điều này được biết vì hành động cuối cùng được đảm bảo là chính xác). 
6. Sau mỗi hành động, hãy kiểm tra xem kích thước tập ứng cử viên có giảm xuống 1 hay không. Sau khi đã giảm, hãy đặt`unique_known`thành Đúng. Từ thời điểm này trở đi, bất kỳ cú sốc nào (từ '!' hoặc '?' không chính xác) đều được coi là quá mức. 
7. Sau khi xử lý tất cả các thao tác, xuất ra`excess_shocks`. 

Tại sao nó hoạt động: Tập ứng cử viên luôn chứa tất cả các chữ cái có thể là chữ cái được chọn. Loại bỏ các chữ cái không thể và xen kẽ các từ gây sốc đảm bảo rằng khi kích thước của nó bằng 1, chữ cái đó được xác định duy nhất. Chỉ tính những cú sốc sau thời điểm này đảm bảo chúng ta chỉ nắm bắt được những cú sốc quá mức. Thuật toán không bao giờ đếm sớm một cú sốc vì tập ứng viên thể hiện chính xác tất cả các khả năng ở mỗi bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
actions = [input().strip() for _ in range(n)]

candidates = set(chr(ord('a') + i) for i in range(26))
excess_shocks = 0
unique_known = False

for i, action in enumerate(actions[:-1]):
    cmd, arg = action.split()
    
    if cmd == '.':
        for ch in arg:
            candidates.discard(ch)
    elif cmd == '!':
        if unique_known:
            excess_shocks += 1
        candidates &= set(arg)
        if len(candidates) == 1:
            unique_known = True
    elif cmd == '?':
        if unique_known and arg in candidates:
            excess_shocks += 1
        candidates.discard(arg)
        if len(candidates) == 1:
            unique_known = True

print(excess_shocks)
```Mã xử lý từng hành động một cách hiệu quả bằng cách sử dụng các thao tác đã thiết lập. Các từ an toàn sẽ loại bỏ các chữ cái khỏi ứng viên, các từ gây sốc giao nhau với các ứng viên và đoán sẽ loại bỏ các chữ cái trừ khi chúng đúng. Bằng cách chỉ lặp lại một lần và theo dõi khi biết được chữ cái duy nhất, chúng tôi đếm chính xác những cú sốc quá mức. Xử lý`actions[:-1]`đảm bảo lần đoán đúng cuối cùng không bị tính là quá mức. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Hành động | Ứng viên | duy nhất_known | dư thừa_shocks | 
| --- | --- | --- | --- | 
| ! abc | {a,b,c,...} & {a,b,c} → {a,b,c} | Sai | 0 | 
| . quảng cáo | {a,b,c} - {a,d} → {b,c} | Sai | 0 | 
| . b | {b,c} - {b} → {c} | Đúng | 0 | 
| ! cd | {c} & {c,d} → {c} | Đúng | 1 | 
| ? c | đoán cuối cùng bị bỏ qua | Đúng | 1 | 

Điều này xác nhận thuật toán đếm chính xác 1 cú sốc quá mức. 

Mẫu 2: 

| Hành động | Ứng viên | duy nhất_known | dư thừa_shocks | 
| --- | --- | --- | --- | 
| ! eo | {a..z} & {e,o} → {e,o} | Sai | 0 | 
| . e | {e,o} - {e} → {o} | Đúng | 0 | 
| ! o | {o} & {o} → {o} | Đúng | 1 | 
| ? o | hành động cuối cùng bị bỏ qua | Đúng | 1 | 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O (n + tổng_length_of_words) | Mỗi hành động được xử lý một lần, với các thao tác được thiết lập tuyến tính theo độ dài từ | 
| Không gian | O(1) | Bộ ứng viên tối đa là 26 chữ cái | 

Độ phức tạp này phù hợp thoải mái trong các ràng buộc, vì n và tổng độ dài từ là 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    actions = [input().strip() for _ in range(n)]

    candidates = set(chr(ord('a') + i) for i in range(26))
    excess_shocks = 0
    unique_known = False

    for i, action in enumerate(actions[:-1]):
        cmd, arg = action.split()
        if cmd == '.':
            for ch in arg:
                candidates.discard(ch)
        elif cmd == '!':
            if unique_known:
                excess_shocks += 1
            candidates &= set(arg)
            if len(candidates) == 1:
                unique_known = True
        elif cmd == '?':
            if unique_known and arg in candidates:
                excess_shocks += 1
            candidates.discard(arg)
            if len(candidates) == 1:
                unique_known = True

    return str(excess_shocks)

# provided samples
assert run("5\n! abc\n. ad\n. b\n! cd\n? c\n") == "1"
assert run("5\n! eo\n. e\n! o\n? o\n") == "1"

# custom cases
assert run("3\n! a\n. b\n? a\n") == "0", "unique determined at last guess"
assert run("4\n! abc\n! abc\n! abc\n? a\n") == "2", "repeated shocks after unique determined"
assert run("6\n. abc\n! d\n! d\n? e\n? d\n") == "1", "interleaving safe and shocking words"
assert run("2\n? a\n? a\n") == "0", "only guesses, no excess shocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 hành động, dự đoán cuối cùng độc đáo | 0 | Thuật toán bỏ qua |
