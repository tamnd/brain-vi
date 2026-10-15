---
title: "CF 105416D - Tranh giành!"
description: "Chúng ta được cấp một tập hợp nhiều chữ cái viết thường. Những chữ cái này ban đầu đến từ một chuỗi có cấu trúc rất mạnh: nó là một palindrome, và trong số tất cả các palindrome có thể được hình thành bằng cách sử dụng chính xác những chữ cái này, nó là cái nhỏ nhất về mặt từ điển."
date: "2026-06-23T17:24:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105416
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 2 (Beginner)"
rating: 0
weight: 105416
solve_time_s: 75
verified: true
draft: false
---

[CF 105416D - Bị xáo trộn!](https://codeforces.com/problemset/problem/105416/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp nhiều chữ cái viết thường. Những chữ cái này ban đầu đến từ một chuỗi có cấu trúc rất mạnh: nó là một palindrome, và trong số tất cả các palindrome có thể được hình thành bằng cách sử dụng chính xác những chữ cái này, nó là cái nhỏ nhất về mặt từ điển. 

Sau khi xáo trộn, thứ tự bị mất nhưng số lượng chữ cái vẫn giữ nguyên. Nhiệm vụ là xây dựng lại chuỗi duy nhất thỏa mãn đồng thời cả hai ràng buộc: nó phải là một chuỗi palindrome và nó phải là chuỗi nhỏ nhất có thể theo thứ tự từ điển trong số tất cả các chuỗi palindrome có thể được xây dựng từ nhiều tập hợp nhất định. 

Một quan sát quan trọng là đầu vào không lưu giữ bất kỳ thông tin vị trí nào. Chỉ có tần số quan trọng. Khi chúng tôi nhận ra điều đó, vấn đề sẽ trở thành vấn đề xây dựng bị hạn chế về số lượng ký tự. 

Ràng buộc n lên tới 10^5 có nghĩa là mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính. Việc sắp xếp là ổn, O(n log n), nhưng bất cứ điều gì liên quan đến việc tính toán lại nhiều lần trên các chuỗi con hoặc quay lui các hoán vị đều không thể thực hiện được. Cấu trúc của palindromes cũng gợi ý rõ ràng rằng chúng ta chỉ nên xây dựng một nửa chuỗi và phản chiếu nó, điều này giữ cho độ phức tạp trở nên tuyến tính. 

Trường hợp khó nhận biết là khi nhiều chữ cái có tần số lẻ. Một palindrome có thể có nhiều nhất một ký tự có số lẻ. Tuyên bố đảm bảo tính khả thi nên chúng ta không cần xác nhận tính không thể thực hiện được mà phải đặt chính xác ký tự tần số lẻ vào giữa. 

Một cạm bẫy khác là giả định rằng việc sắp xếp toàn bộ chuỗi sẽ trực tiếp mang lại câu trả lời. Ví dụ: trong "xe đua", việc sắp xếp sẽ cho ra "aacce rr", thậm chí còn không gần với bảng màu. Cấu trúc phải được thực thi một cách rõ ràng, không xuất hiện sau khi sắp xếp. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là tạo ra tất cả các hoán vị của các chữ cái đã cho và kiểm tra xem chữ cái nào là bảng màu, sau đó chọn chữ cái nhỏ nhất về mặt từ điển trong số chúng. Điều này đúng về mặt khái niệm vì nó khám phá toàn bộ không gian giải pháp, nhưng nó thất bại ngay lập tức về quy mô. Ngay cả khi chúng ta chỉ giới hạn bản thân ở những hoán vị riêng biệt, thì số cách sắp xếp vẫn là giai thừa theo n trong trường hợp xấu nhất và việc lọc palindrome không làm giảm bản chất hàm mũ đủ để có thể sử dụng được. Với n = 10^5, điều này hoàn toàn không khả thi. 

Cái nhìn sâu sắc về cấu trúc quan trọng là một palindrome được xác định hoàn toàn bởi nửa đầu của nó và tùy chọn một ký tự ở giữa. Nếu chúng ta sửa nhiều bộ ký tự, nửa bên trái phải chứa chính xác tầng (n/2) ký tự và nửa bên phải được buộc làm bản sao của nó. Do đó, vấn đề giảm xuống còn việc xây dựng nhiều tập hợp nhỏ nhất có thể về mặt từ điển cho nửa bên trái, đồng thời tôn trọng các ràng buộc về tần số và đặt bất kỳ ký tự lẻ nào còn sót lại ở giữa. 

Để đạt được sự tối giản về mặt từ điển, chúng tôi tham lam điền vào nửa bên trái từ 'a' đến 'z', luôn sử dụng càng nhiều lần xuất hiện của một ký tự càng tốt mà không vi phạm yêu cầu rằng các ký tự còn lại vẫn có thể tạo thành một nửa phản chiếu hợp lệ. Vì thứ tự từ điển được quyết định từ trái sang phải nên việc chọn các ký tự nhỏ hơn sớm hơn luôn là tối ưu, miễn là duy trì được tính khả thi. 

Điều này biến vấn đề thành vấn đề phân bổ tần số tham lam hơn là vấn đề hoán vị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Cấu trúc nửa tham lam tối ưu | O(26 · n) ≈ O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta đếm tần số của mỗi ký tự. Điều này nén đầu vào thành trạng thái có kích thước cố định, điều này rất quan trọng vì chúng ta sẽ không bao giờ quan tâm đến thứ tự ban đầu nữa. 

1. Tính toán mảng tần số`cnt[26]`cho tất cả các ký tự trong chuỗi. Điều này nắm bắt tất cả thông tin có thể sử dụng từ đầu vào. 
2. Tính xem nửa bên trái sẽ có bao nhiêu ký tự`n // 2`. Một palindrome tạo ra sự đối xứng, do đó mọi thứ bên ngoài trung tâm đều được phản chiếu. 
3. Xác định ký tự ở giữa, nếu có. Vì nhiều nhất một ký tự có thể có tần số lẻ nên chúng tôi chọn ký tự đó nếu có. Phần giữa không ảnh hưởng đến các ràng buộc về thứ tự trên các nửa. 
4. Xây dựng nửa bên trái một cách tham lam từ 'a' đến 'z'. Đối với mỗi ký tự, chúng tôi cố gắng đặt nó càng sớm càng tốt theo thứ tự từ điển. Tuy nhiên, chúng ta không thể đơn giản lấy tất cả các lần xuất hiện có sẵn; chúng ta phải đảm bảo rằng sau khi đặt các ký tự ở nửa bên trái, số ký tự còn lại vẫn khớp với nửa bên phải được yêu cầu. 
5. Đối với mỗi ký tự, chúng tôi lấy liên tục trong khi vẫn cần các ký tự ở nửa bên trái và trong khi lấy không vi phạm yêu cầu các ký tự còn lại có thể lấp đầy phần còn lại của nửa bên trái. Vì tổng công suất còn lại đã được biết nên điều kiện này giúp đơn giản hóa việc theo dõi số lượng vị trí còn lại thay vì thực hiện kiểm tra tính khả thi. 
6. Khi nửa bên trái được xây dựng, bảng màu đầy đủ được hình thành bằng cách ghép nửa bên trái, ký tự ở giữa (nếu có) và đảo ngược của nửa bên trái. 

Tính đúng đắn phụ thuộc vào thực tế là thứ tự từ điển hoàn toàn được xác định bởi vị trí sớm nhất nơi hai chuỗi khác nhau. Bằng cách lấp đầy nửa bên trái một cách tham lam từ ký tự nhỏ nhất đến ký tự lớn nhất, chúng tôi đảm bảo tính tối thiểu ở vị trí khác nhau đầu tiên. 

### Tại sao nó hoạt động 

Tại mọi vị trí ở nửa bên trái, chúng ta chọn ký tự nhỏ nhất có thể mà vẫn cho phép hoàn thành các vị trí còn lại. Vì tất cả các ký tự còn lại không bị ràng buộc ngoại trừ tổng số lượng và chúng tôi không giải quyết vấn đề khả thi tổ hợp ngoài số lượng khớp, nên lựa chọn tham lam là an toàn. Bất kỳ sai lệch nào đối với ký tự lớn hơn trước đó sẽ ngay lập tức làm tăng giá trị từ điển của bảng màu kết quả và không có cơ chế nào sau này trong quá trình xây dựng để bù đắp cho sự gia tăng đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)
    
    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1
    
    left_len = n // 2
    left = []
    mid = ""
    
    for i in range(26):
        if cnt[i] % 2 == 1:
            mid = chr(i + 97)
            cnt[i] -= 1
    
    remaining = left_len
    
    for i in range(26):
        if remaining == 0:
            break
        take = min(cnt[i] // 2, remaining)
        if take > 0:
            left.append(chr(i + 97) * take)
            remaining -= take
    
    left = "".join(left)
    right = left[::-1]
    
    print(left + mid + right)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách thu gọn chuỗi thành các số đếm tần số, loại bỏ tất cả thông tin thứ tự không liên quan. Việc lựa chọn ký tự ở giữa được xử lý bằng cách sử dụng bất kỳ ký tự đếm lẻ nào, đảm bảo rằng tất cả các số đếm còn lại trở thành số chẵn, điều này cần thiết cho tính đối xứng. 

Việc xây dựng nửa bên trái sử dụng phân bổ tham lam trực tiếp: với mỗi ký tự từ 'a' đến 'z', chúng ta gán càng nhiều cặp càng tốt cho đến hết dung lượng còn lại. Bộ đếm còn lại đảm bảo chúng ta không bao giờ vượt quá giới hạn nửa độ dài. 

Cuối cùng, việc phản chiếu nửa bên trái đảm bảo cấu trúc palindrome mà không cần tính toán thêm. 

Một điểm tinh tế là chúng ta không cần phải kiểm tra nhiều lần tính khả thi ngoài việc theo dõi các vị trí còn lại. Bởi vì chúng ta luôn sử dụng các cặp nên các ràng buộc chẵn lẻ vẫn tự động nhất quán. 

## Ví dụ đã hoạt động 

### Ví dụ 1: "xe đua" 

Chúng tôi tính toán tần số: a:2, c:1, e:1, r:2. 

Ký tự ở giữa được chọn từ số lẻ; trong số 'c' và 'e', ​​từ nhỏ nhất về mặt từ điển là 'c' hoặc 'e', ​​nhưng chỉ có thể sử dụng một từ và việc xây dựng đảm bảo tính nhất quán bằng cách giảm cả hai thành các cặp chẵn ngoại trừ một cặp được giữ lại làm trung tâm. 

Chiều dài nửa bên trái là 3. 

Chúng tôi điền từ 'a' đến 'z': 

| Bước | Char | Các slot còn lại | Hành động | Trái | 
| --- | --- | --- | --- | --- | 
| 1 | một | 3 → 2 | lấy 1 cặp càng tốt | một | 
| 2 | c | 2 → 2 | bỏ qua phần ghép nối (xử lý lẻ sau) | một | 
| 3 | e | 2 → 2 | bỏ qua để ghép nối | một | 
| 4 | r | 2 → 0 | lấy 1 cặp | ar | 

Giữa trở thành trung tâm còn lại hợp lệ nhỏ nhất, ở đây được xác định bằng cách xử lý kỳ quặc. Kết quả cuối cùng là`acrerca`. 

Điều này cho thấy cách ghép nối tham lam không chỉ đơn giản là sắp xếp tất cả các chữ cái mà còn thực thi cấu trúc bảng màu. 

### Ví dụ 2: "aabbccdde" 

Tần số: a2 b2 c2 d2 e1. 

Chiều dài nửa bên trái là 4, giữa là 'e'. 

| Bước | Char | Các slot còn lại | Hành động | Trái | 
| --- | --- | --- | --- | --- | 
| 1 | một | 4 → 3 | lấy 1 cặp | một | 
| 2 | b | 3 → 2 | lấy 1 cặp | ab | 
| 3 | c | 2 → 1 | lấy 1 cặp | abc | 
| 4 | d | 1 → 0 | lấy 1 cặp | abcd | 

Nửa bên phải gương để`dcba`, chuỗi cuối cùng là`abcdedcba`. 

Điều này xác nhận rằng việc phân bổ tham lam một cách tự nhiên mang lại cấu trúc tối thiểu về mặt từ điển vì các ký tự trước đó luôn bị cạn kiệt trước tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | đếm tần số và chuyển một lần trên 26 chữ cái | 
| Không gian | O(1) | mảng bảng chữ cái cố định và chuỗi đầu ra | 

Giải pháp tuyến tính theo độ dài của đầu vào, vừa vặn thoải mái trong giới hạn 10^5 ký tự và giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    s = input().strip()
    n = len(s)

    cnt = [0] * 26
    for ch in s:
        cnt[ord(ch) - 97] += 1

    left_len = n // 2
    left = []
    mid = ""

    for i in range(26):
        if cnt[i] % 2 == 1:
            mid = chr(i + 97)
            cnt[i] -= 1

    remaining = left_len
    for i in range(26):
        take = min(cnt[i] // 2, remaining)
        left.append(chr(i + 97) * take)
        remaining -= take

    left = "".join(left)
    return left + mid + left[::-1]

# provided samples
assert run("racecar\n") == "acrerca", "sample 1"
assert run("aabbccdde\n") == "abcdedcba", "sample 2"
assert run("ajcvoiwqnexajcvoiwqnex\n") == "aceijnoqvwxxwvqonjieca", "sample 3"

# custom cases
assert run("a\n") == "a", "single char"
assert run("aa\n") == "aa", "minimal palindrome"
assert run("abcba\n") == "abcba", "already palindrome"
assert run("cbaabc\n") == "abccba", "reordering needed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một | một | đầu vào tối thiểu | 
| aa | aa | palindrome tầm thường có độ dài chẵn | 
| abcba | abcba | bảng màu đã hợp lệ | 
| cbaabc | abccba | tối ưu hóa từ điển với tính đối xứng | 

## Vỏ cạnh 

Đối với chuỗi ký tự đơn như`z`, thuật toán trực tiếp tạo ra`z`bởi vì phần trung tâm được chọn và không có nửa bên trái nào được hình thành. Bộ mảng tần số`mid = z`và cả hai nửa vẫn trống, do đó việc ghép sẽ mang lại kết quả chính xác. 

Đối với một đầu vào như`zzzaaa`, số đếm là a3 z3. Một ký tự trở thành trung tâm (cả hai đều là ứng cử viên, nhưng chỉ một ký tự được sử dụng) và các ký tự còn lại tạo thành cặp. Nửa bên trái trở thành`az`, và sự phản chiếu tạo ra`azza`, với phần trung tâm được lấp đầy một cách thích hợp tùy thuộc vào chi tiết triển khai. Việc ghép nối tham lam đảm bảo tính đối xứng mà không vi phạm các ràng buộc về từ điển vì 'a' luôn được đặt trước 'z'. 

Đối với các chuỗi có tất cả các ký tự giống hệt nhau, chẳng hạn như`aaaaaa`, thuật toán giảm trực tiếp thành việc chia thành hai nửa và tạo ra một palindrome hoàn hảo mà không cần đưa ra quyết định nào, chứng tỏ rằng logic tham lam thoái hóa hoàn toàn thành một trường hợp thống nhất.
