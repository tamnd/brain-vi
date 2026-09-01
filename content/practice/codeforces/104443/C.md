---
title: "CF 104443C - Morco-Feely Palindromes"
description: "Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ số, có độ dài tối đa là 100. Nhiệm vụ là quyết định xem chuỗi này có thỏa mãn một điều kiện đối xứng rất cụ thể hay không và đưa ra phán quyết đơn giản là có hoặc không."
date: "2026-06-30T18:45:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104443
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #18 (JuneIsApril-Forces)"
rating: 0
weight: 104443
solve_time_s: 68
verified: true
draft: false
---

[CF 104443C - Morco-Feely Palindromes](https://codeforces.com/problemset/problem/104443/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi duy nhất chỉ bao gồm các chữ số, có độ dài tối đa là 100. Nhiệm vụ là quyết định xem chuỗi này có thỏa mãn một điều kiện đối xứng rất cụ thể hay không và đưa ra phán quyết đơn giản là có hoặc không. 

Cấu trúc mà chúng ta đang tìm kiếm hoàn toàn là về cách các ký tự đọc từ trái sang phải so sánh với các ký tự đọc từ phải sang trái. Không cần giải thích số học các chữ số, không cần phân tích số hoặc thực hiện các phép biến đổi. Toàn bộ vấn đề giảm xuống còn việc suy luận về chuỗi ký tự như một đối tượng tĩnh. 

Vì độ dài tối đa chỉ là 100 nên mọi giải pháp kiểm tra ký tự trực tiếp đều đủ hiệu quả. Ngay cả một cách tiếp cận liên tục quét chuỗi vẫn sẽ chạy trong thời gian không đổi so với giới hạn lập trình cạnh tranh thông thường. Điều này ngay lập tức loại bỏ những lo ngại về tối ưu hóa hiệu suất và chuyển sự tập trung hoàn toàn vào tính chính xác và xử lý các trường hợp ranh giới. 

Sự tinh tế chính nằm ở việc nhận biết hành vi của cạnh trên các đầu vào rất nhỏ. Chuỗi ký tự đơn hoạt động khác với chuỗi nhiều ký tự vì tính đối xứng trở nên tầm thường, trong khi chuỗi hai ký tự có thể đáp ứng hoặc không đáp ứng điều kiện bắt buộc tùy thuộc vào việc cả hai đầu có khớp hay không. 

Một sai lầm ngây thơ ở đây là giả sử một thuộc tính dựa trên quan sát một phần, chẳng hạn như chỉ kiểm tra ký tự đầu tiên và cuối cùng mà không xác minh cấu trúc đầy đủ. Ví dụ, đầu vào`343`sẽ vượt qua lần kiểm tra đầu tiên cuối cùng một cách không chính xác, nhưng vẫn cần phải xác minh đầy đủ để đảm bảo tính đối xứng hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là kiểm tra xem chuỗi có đọc tiến và lùi giống nhau hay không. Điều này có nghĩa là so sánh các vị trí đối xứng: ký tự đầu tiên với ký tự cuối cùng, ký tự thứ hai với ký tự thứ hai cuối cùng, v.v. cho đến khi đạt đến giữa. 

Một cách mạnh mẽ để nghĩ về điều này là đảo ngược chuỗi và so sánh nó với chuỗi gốc. Điều này hoạt động vì việc đảo ngược mã hóa tất cả các ràng buộc đối xứng thành một so sánh đối tượng duy nhất. Chi phí là tuyến tính theo độ dài chuỗi, vì việc xây dựng chuỗi đảo ngược và so sánh cả hai đều mất O(n) thời gian. 

Với giới hạn độ dài tối đa là 100, ngay cả những so sánh lặp đi lặp lại hoặc những lần quét đơn giản cũng không đáng kể. Tuy nhiên, nếu chúng ta khái quát hóa ý tưởng, thì cái nhìn sâu sắc về cấu trúc quan trọng là việc kiểm tra palindrome không yêu cầu lưu trữ toàn bộ chuỗi đảo ngược. Thay vào đó, chúng ta có thể so sánh trực tiếp các chỉ số được phản chiếu, điều này tránh được việc phân bổ bổ sung và làm cho logic rõ ràng hơn. 

Brute-force hoạt động vì nó xây dựng rõ ràng biểu diễn đảo ngược và kiểm tra tính bằng nhau. Chi phí sẽ trở nên không cần thiết khi chúng tôi nhận ra rằng việc so sánh là độc lập theo cặp, cho phép kết thúc sớm ngay khi tìm thấy sự không khớp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đảo ngược và so sánh | O(n) | O(n) | Đã chấp nhận | 
| Kiểm tra hai con trỏ | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tập trung vào phương pháp hai con trỏ vì nó thể hiện cấu trúc của bài toán một cách trực tiếp nhất. 

1. Bắt đầu với hai chỉ số, một ở đầu chuỗi và một ở cuối chuỗi. Chúng đại diện cho các ký tự phải khớp để chuỗi vẫn đối xứng. Nếu tại bất kỳ điểm nào chúng khác nhau thì tính đối xứng sẽ bị phá vỡ ngay lập tức. 
2. So sánh ký tự ở hai chỉ số. Nếu chúng không bằng nhau thì chúng ta có thể dừng lại và kết luận chuỗi không đối xứng. Việc thoát sớm này là hợp lệ vì một sự không khớp duy nhất sẽ vi phạm điều kiện chung. 
3. Nếu chúng khớp nhau, hãy di chuyển cả hai con trỏ vào trong, tiến chỉ mục bên trái lên phía trước và chỉ mục bên phải lùi lại. Điều này đảm bảo chúng tôi xác minh dần dần tất cả các vị trí được phản chiếu mà không lặp lại. 
4. Lặp lại quá trình này cho đến khi các con trỏ giao nhau hoặc gặp nhau. Nếu chúng ta hoàn thành tất cả các phép so sánh mà không tìm thấy điểm không khớp thì chuỗi đó thỏa mãn yêu cầu về tính đối xứng. 

Lý do đằng sau quá trình này là mọi chuỗi đối xứng hợp lệ phải đồng thời thỏa mãn sự bằng nhau ở tất cả các vị trí được nhân đôi. Việc kiểm tra chúng một cách độc lập đảm bảo tính đầy đủ vì mỗi so sánh đều thực thi một điều kiện cần thiết của cấu trúc toàn cục. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến là tất cả các cặp ký tự nằm ngoài phạm vi con trỏ hiện tại đã được xác minh là khớp. Mỗi bước sẽ giảm phần chưa được xác minh của chuỗi trong khi vẫn duy trì tính chính xác. Nếu sự không khớp tồn tại ở bất cứ đâu, nó phải xảy ra ở một số cặp được phản chiếu và cặp đó cuối cùng sẽ được kiểm tra trực tiếp. Do đó, thuật toán không thể bỏ sót một vi phạm nào và nếu nó kết thúc mà không tìm thấy vi phạm nào thì chuỗi phải đối xứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

i, j = 0, len(s) - 1
ok = True

while i < j:
    if s[i] != s[j]:
        ok = False
        break
    i += 1
    j -= 1

print("YES" if ok else "NO")
```Mã đọc chuỗi đầu vào và khởi tạo hai con trỏ ở điểm cực trị của nó. Vòng lặp thực thi logic so sánh được nhân đôi được mô tả trước đó. Khi phát hiện thấy sự không khớp, chúng tôi thoát ra sớm vì việc kiểm tra thêm không thể sửa chữa được tính đối xứng. 

Một chi tiết triển khai tinh tế là việc sử dụng`strip()`để đảm bảo không có dòng mới nào cản trở việc lập chỉ mục. Một điểm quan trọng khác là xử lý trường hợp chuỗi có độ dài 1, trong trường hợp đó vòng lặp không bao giờ chạy và kết quả mặc định chính xác là`YES`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`5`| tôi | j | s[i] | s[j] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 5 | con trỏ gặp nhau, dừng lại | 

Vì không có sự không khớp nào nên chuỗi này có tính chất đối xứng theo định nghĩa. 

Điều này xác nhận hành vi trên kích thước đầu vào tối thiểu, trong đó tính đối xứng là đúng. 

### Ví dụ 2 

đầu vào:`43`| tôi | j | s[i] | s[j] | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 3 | tìm thấy không khớp | 

Phép so sánh đầu tiên đã vi phạm tính đối xứng nên quá trình này sẽ dừng ngay lập tức. 

Điều này chứng tỏ sự kết thúc sớm trong các chuỗi không đối xứng, trong đó không cần tính toán thêm một khi tìm thấy mâu thuẫn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được so sánh tối đa một lần thông qua ghép nối được phản chiếu | 
| Không gian | O(1) | Chỉ có hai con trỏ được sử dụng, không có cấu trúc dữ liệu bổ sung | 

Kích thước đầu vào được giới hạn bởi 100, do đó thuật toán chạy trong thời gian không đổi trong thực tế. Ngay cả trong cài đặt lớn hơn nhiều, quét tuyến tính vẫn hiệu quả và dễ dàng trong các giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    s = input().strip()
    i, j = 0, len(s) - 1
    ok = True
    while i < j:
        if s[i] != s[j]:
            ok = False
            break
        i += 1
        j -= 1
    return "YES" if ok else "NO"

# provided samples
assert run("5\n") == "YES"
assert run("43\n") == "NO"
assert run("76\n") == "NO"

# custom cases
assert run("1\n") == "YES"
assert run("11\n") == "YES"
assert run("12321\n") == "YES"
assert run("12345\n") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | CÓ | đối xứng ký tự đơn | 
| 11 | CÓ | bảng màu có độ dài chẵn | 
| 12321 | CÓ | palindrome dài lẻ dài hơn | 
| 12345 | KHÔNG | trình tự không đối xứng rõ ràng | 

## Vỏ cạnh 

Một đầu vào một chữ số như`7`là kịch bản đơn giản nhất. Bộ thuật toán`i = 0`Và`j = 0`, do đó vòng lặp không thực thi. Kết quả vẫn còn`YES`, điều này đúng vì theo định nghĩa, mọi chuỗi ký tự đơn đều đối xứng. 

Đối với hai ký tự không khớp, chẳng hạn như`90`, so sánh đầu tiên`s[0]`vs`s[1]`thất bại ngay lập tức. Thuật toán xuất ra chính xác`NO`mà không cần kiểm tra không cần thiết. 

Đối với đầu vào đối xứng dài hơn như`1221`, so sánh tiến hành như`(1,1)`Và`(2,2)`ở những vị trí được nhân đôi. Tất cả các lần kiểm tra đều thành công, xác nhận rằng thuật toán tổng hợp chính xác đẳng thức cục bộ thành đối xứng toàn cục.
